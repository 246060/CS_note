# 효율적인 쿼리 작성 Tip-1
해당 페이지는 MySQL 기준 설명

## index
 1. WHERE 조건 이해
    1. 묵시적 형변환
    2. 잘못 사용된 함수
    3. LIKE 검색
 2. SQL 레벨에서의 접근법
    1. 데이터 연산을 최소한으로 유도
    2. 불필요한 조인을 피하기
    3. Semi Join으로 인한 비효율 제거
    4. Outer Join이 반드시 필요한지 파악
    5. 서브쿼리 활용
 3. 스키마 레벨에서의 접근법
    1. Primary Key 직접적인 영향 여부
    2. 불필요한 인덱스 삭제





<br/><br/>

## WHERE 조건 이해
- 묵시적 형변환
- 잘못 사용된 함수
- LIKE 검색

<br/>

### 묵시적 형변환
묵시적 형변환이란 조건절의 데이터 타입이 다를 때 우선순위가 높은 타입으로 형이 내부적으로 변환되는 것을 말한다. 

> **우선순위**  
> 정수 타입 > 문자열 타입

만약 문자열과 정수값을 비교하는 쿼리가 들어오면 두 개의 컬럼 중 우선순위가 낮은 문자열은 자연스럽게 정수 타입으로 형 변환되어 처리된다. 

하지만 주의할 것은 때로는 예기치 않은 결과값이 나올 수도 있다. 특히 문자열 사이에 보이지 않는 문자가 포함된 경우에는 문제를 찾기도 어렵다. 

그러므로 묵시적 형변환 함정에 빠지지말고 반드시 조건절에는 컬럼 타입을 맞춰서 질의하는 것을 권한다.


<br/>


### 잘못 사용된 함수
함수는 잘못 쓰면 불필요한 시스템 부하를 야기한다. SQL을 논리적으로 작성하는 것도 중요하지만 DBMS가 이해하기 쉽게 SQL을 작성하는 것도 DB 튜닝에 중요하다.

```sql
SELECT userid, count(*) AS cnt
FROM user_access_log
WHERE DATE_FORMATI(reg_date, '%Y%m%d') = '2016-10-10'
  AND DATE_FORMATI(reg_date, '%H') >= '10'
  AND DATE_FORMATI(reg_date, '%H') < '21'
GROUP BY userid;

```

위와 같이 SQL 작성하면 어떤 목적의 쿼리 인지를 쉽게 알아볼 수 있다. 그러나 DB 내부적으로 테이블 풀스캔을 수행할 수밖에 없다. 설령 적절한 인덱스 또는 테이블 파티셔닝이 적용되어 있을지라도 말이다. 옵티마이저는 컬럼의 분포도를 기준으로 데이터를 추출하는 가장 빠른 방법을 도출한다. 그런데 위 쿼리와 같이 reg_date 컬럼 검색 시 DATE_FORMAT 함수를 사용하면 옵티마이저는 reg_date와 연관된 데이터 분포도를 알 수 없게 된다. DATE_FORMAT 함수로 인해 변경될 결과값을 옵티마이저가 예상하지 못하기 때문이다. 물론 인덱스가 없는 경우에도 시스템적으로 부하가 있기는 마찬가지다. DATE_FORMAT 함수를 쓸데없이 많이 호출하기 때문이다.



<br/>


### LIKE 검색
LIKE 검색은 상당히 편리하지만 대용량 테이블인 경우에는 위험하다. LIKE 검색은 ‘%’ 문자 위치에 따라 다르게 수행된다. 

- [col LIKE ‘abc%’] : abc로 시작하는 문자 조회
  - 데이터 분포도에 따라 인덱스를 탈 수도 있고 안 탈수도 있음. 
- [col LIKE ‘%abc%’] : abc 포함한 문자 조회
  - 테이블 풀스캔이 발생
- [col LIKE ‘abc’] : abc로 끝나는 문자 조회
  - 테이블 풀스캔이 발생

다음과 같이 SQL 작성 팁 지켜도 LIKE 검색의 성능을 향상시킬 수 있다.

- LIKE 조건이 ‘검색어%’ 와 같이 검색어가 앞 단에 있다면 데이터 분포도를 따져서 수행한다.
- LIKE 조건이 ‘%검색어’와 같은 형태로 반드시 수행해야 한다면 LIKE 조건 이외의 조건절을 적극 활용하여 LIKE 처리가 필요한 데이터 범위를 최대한 줄인다.

DB에서 처리하는 데이터 범위를 MySQL Explain 활용하여 최대한 줄이는 것이 성능 최적화의 가장 기본적인 요소이다.






<br/><br/><br/><br/>

## SQL 레벨에서의 접근법
- 데이터 연산을 최소한으로 유도
- 불필요한 조인을 피하기
- Semi Join으로 인한 비효율 제거
- Outer Join이 반드시 필요한지 파악
- 서브쿼리 활용


<br/>

Join은 여러 테이블에서 하나의 결과셋를 가져올 수 있지만 무분별하게 조인을 사용하면 데이터가 누적됨에 따라 쿼리 성능이 저하된다.

> ✏ MySQL에서는 모든 처리를 단일 코어에서 Nested Loop Join 방식으로만 데이터를 처리한다. 병렬 처리라는 것은 없다. 그렇기 때문에 MySQL 입장에서는 CPU 코어 개수를 늘리는 Scale-Out보다는 오히려 단위처리량이 좋은 CPU로 Scale-Up을 하는 것이 훨씬 유리하다.



<br/>

### 데이터 연산을 최소한으로 유도
Nested Loop Join이란 선행 테이블(A)의 조건 검색 결과 값 하나하나를 엑세스 하면서 연결할 테이블(B)에 대입하여 조인하는 방식이다. 상용 DBMS에서 흔히 제공하는 Hash Join과 Merge Join 같은 조인 알고리즘이 MySQL에는 구현되어 있지 않다. 그러므로 확인하여 Nested Loop Join 횟수를 최대한 줄이는 것이 쿼리 성능을 향상시키는 첫걸음이다.



<br/>

### 불필요한 조인을 피하기



<br/>

### Semi Join으로 인한 비효율 제거
> 8.0 이상의 버전은 어떠한지 확인 필요! 일단 참고만 하자!

MySQL 5.5 버전까지는 Semi Join (‘세미 조인’ 또는 ‘부분 조인’)의 성능이 최적화 되어 있지 않았다. Semi Join이란 Exists와 IN 같은 조인 안에 SELECT가 있는 경우이다. 아래와 같은 경우를 예를 들어보자.

```sql
SELECT * 
FROM Country
WHERE Continent = 'Europe'     
AND Country.Code IN (SELECT City.country FROM City WHERE City.Population > 1*1000*1000);
```

서브 쿼리 실행 후 WHERE 조건을 수행하는 것이 아니라 매번 데이터를 Nested Loop Join 탐색하면서 서브 쿼리를 수행하기 때문에 불필요한 부하가 발생한다. Oracle에서는 IN 구문 안의 SELECT를 먼저 수행한 후 결과 값을 Hash 형식으로 만들어서  데이터를 처리한다고 예상할 것이다. 그러나 아쉽게도 MySQL에서는 위와 같이 Country 테이블의 Continent가 Europe인 조건을 먼저 검색하고, 그 결과를 IN 조건의 서브 쿼리를 일일이 반복 수행하여 최종 데이터를 가져온다.




<br/>

### Outer Join이 반드시 필요한지 파악
무분별하게 Outer Join (‘아우터 조인’ 또는 ‘외부 조인’)을 수행하면 성능이 상당히 떨어진다. 추가적인 데이터 조회를 위해 Outer Join을 사용할 때는 특히 주의하여 SQL 작성 해야 한다.

<수정 전>
```sql
SELECT 
	M.MASTER_NO,
	M.TITLE,
	MI.PATH,
	M.REGDATE,
	CM.TYPE
FROM MAIN AS MAIN
INNER JOIN TAB01 AS CM ON CM.MASTER_NO = M.MASTER_NO
LEFT OUTER JOIN TAB02 AS MI ON M.MASTER_NO = MI.MASTER_NO
WHERE M.DEL_YN = 'N'
ORDER BY M.MASTER_NO DESC
LIMIT 10000, 10;
```

데이터를 10,000번째 위치부터 10건을 가져온다면 결과적으로 불필요한 10,000번의 Outer Join이 발생한다. 물론 데이터가 적을 경우에는 문제가 없지만 데이터가 누적되면 서버에 큰 영향을 미칠 수 있다.

<수정 후>
```sql
SELECT
	M.MASTER_NO,
	M.TITLE,
	MI.PATH,
	M.REGDATE,
	CM.TYPE
FROM (
	SELECT
		M.MASTER_NO,
		M.TITLE,
		MI.PATH,
		M.REGDATE,
		CM.TYPE	
	FROM MAIN AS MAIN
	INNER JOIN tab01 AS CM ON CM.MASTER_NO = M.MASTER_NO
	ORDER BY M.MASTER_NO DESC
	LIMIT 10000, 10 
	) A
	LEFT OUTER JOIN tab02 AS MI ON A.MASTER_NO = MI.MASTER_NO;
```



<br/>

### 서브쿼리 활용
서브 쿼리는 DB 내부적으로 Temporary Table을 생성하여 데이터를 처리하기 때문에 과용하면 좋지 않다. 쿼리 안에 또 다른 쿼리를 넣어서 데이터를 질의할 수 있기 때문에 데이터를 조회하는 것이 상당히 편하게 변모하였다. 

그러나 서브 쿼리가 편하다고 해서 무분별하게 사용한다면 서버 성능이 급격히 저하되는 사태에 이르는 원인이 될 수도 있다. 그러나 필요한 부분에 적극 사용하면 SQL 작성 및 성능 향상에 큰 도움이 된다.





<br/><br/><br/><br/>

## 스키마 레벨에서의 접근법
- Primary Key 직접적인 영향 여부
- 불필요한 인덱스 삭제


InnoDB에서 Primary Key 관련 영향 여부 및 불필요한 인덱스 삭제


<br/>

### Primary Key 직접적인 영향 여부
MySQL의 InnoDB에서는 Primary Key 순서로 데이터가 저장된다. Primary Key는 데이터에 접근하는 물리적인 주소로 사용된다고 봐도 된다. 

즉, ‘인덱스 순서’로 데이터가 정렬되어 디스크에 저장된다. 


<br/>

### 불필요한 인덱스 삭제
보통 인덱스가 많으면 DB 성능이 무조건 좋아진다고 생각한다. 인덱스가 있으면 데이터를 빨리 가져올 수 있고 관련 DISK I/O 혹은 메모리 연산 작업을 최소화해주기 때문일 것이다. 

하지만 기억해야 할 것은 인덱스도 바로 데이터라는 것이다. 일반 데이터와 다르게 인덱스는 수시로 메모리 상에서 구조가 변화하는 동적인 데이터다. 때로는 불필요한 인덱스가 생성되어 있는 것도 볼 수 있는데, 이러한 인덱스는 아예 사용이 안 되는 경우도 다수이다.

```sql
CREATE TABLE `test_0_index` (
	`i` INT(10) UNSIGNED NOT NULL AUTO_INCREMENT,
	`j` INT(10) UNSIGNED NOT NULL,
	`s` VARCHAR(64) NOT NULL,
	`d` DATETIME NOT NULL,
	PRIMARY KEY (`i`),
	INDEX `idx_j` (`j`),
	INDEX `idx_d` (`d`),
	INDEX `cidx_jd` (`j`, `d`),
	INDEX `cidx_dj` (`d`, `j`)
);
```

먼저 불필요한 인덱스 idx_j와 idx_d는 불필요한 인덱스이다. cidx_jd와 cidx_dj 인덱스에서 각 칼럼을 복합 인덱스 형식으로 구성하였기 때문에 j 혹은 d만으로 데이터를 조회하는 쿼리 요청이 와도 정상적으로 데이터를 빠르게 추출한다. 그렇다고 무조건 idx_j와 idx_d를 제거해서는 안 된다. 만약 d로 데이터를 검색한 결과를 i컬럼 (Primary Key) 순서(혹은 역순)로 가져오는 게 목적이라면, cidx_dj가 오히려 필요 없는 인덱스이다. idx_d로 검색한 결과는 Primary Key 순으로 나오지만 cidx_dj 인덱스에서 d로 검색한 결과는 j 순서로 나오기 때문이다. 따라서 인덱스를 정리하기 전에 인덱스의 사용 목적을 정확히 하며 모니터링 해야 한다.

효율적인 SQL 작성 외에도 MySQL 스토리지 엔진에 대한 정보는 여기를 참고하시면 되겠습니다. 지금까지 MySQL 효율적인 SQL 작성 3가지 방법 알아보았으며 위의 3가지 조건 (WHERE 조건 이해 / SQL 레벨에서의 접근법 / 스키마 레벨에서의 접근법) 에 유의하여 쿼리를 작성하면 효율적인 SQL 작성에 도움이 될 것으로 보여집니다.




<br/><br/><br/><br/>


Reference
1. https://nomadlee.com/mysql-%ED%9A%A8%EC%9C%A8%EC%A0%81%EC%9D%B8-sql-%EC%9E%91%EC%84%B1-3%EA%B0%80%EC%A7%80-%EB%B0%A9%EB%B2%95/