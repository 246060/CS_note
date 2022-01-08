# 기타 정리

- [기타 정리](#기타-정리)
  - [1. Data block](#1-data-block)
  - [2. MySql storage engine](#2-mysql-storage-engine)
    - [innodb](#innodb)
    - [myisam](#myisam)
  - [3. char vs varchar vs text vs blob](#3-char-vs-varchar-vs-text-vs-blob)
    - [char](#char)
    - [varchar](#varchar)
    - [10.5.10-MariaDB utf8 에서 테스트 결과](#10510-mariadb-utf8-에서-테스트-결과)
  - [4. blob vs clob](#4-blob-vs-clob)
  - [5. db buffer pool](#5-db-buffer-pool)
    - [개념](#개념)
    - [버퍼 풀의 크기 (innodb_buffer_pool_size)](#버퍼-풀의-크기-innodb_buffer_pool_size)
    - [버퍼 풀 인스턴스 수 (innodb_buffer_pool_instances)](#버퍼-풀-인스턴스-수-innodb_buffer_pool_instances)
    - [버퍼 관리자](#버퍼-관리자)
  - [6. db log 종류](#6-db-log-종류)


## 1. Data block
- logical block
데이터베이스 파일은 블록(block)이라고 불리는 고정된 길이의 저장 단위로 저장되고, 이 블록은 디스크 할당(storage allocation)과 데이터 전송(data transfer)의 기본 단위이다.



## 2. [MySql storage engine](https://velog.io/@gillog/DBInnoDB-VS-MyISAM)
### innodb 
- 트랜젝션에 안전한 테이블을 제공하는 트랜잭션-세이프 스토리지 엔진
- 디폴트
- 자체적으로 메인 메모리 안에 데이터 캐싱과 인덱싱을 위한 버퍼 풀(pool)을 관리
- 테이블과 인덱스를 테이블 스페이스에 저장
  - 테이블 스페이스는 몇개의 서버파일이나 디스크 파티션으로 구성
- row level locking
- CRUD가 많은 서비스에 적합
- full-text 인덱스를 지원하지 못한다.

### myisam
- 비-트랜젝션-세이프(non-transactional-safe) 테이블을 관리
- 전체 문장 검색 능력 뿐만 아니라, 고-성능 스토리지 및 복구 기능을 제공
- 블로그라던지, 게시판 처럼 한사람이 글을 쓰면 다른 많은 사람들이 글을 읽는 방식에 최적의 성능을 발휘
- 테이블과 인덱스를 각각 분리된 파일로 관리
- 항상 테이블에 ROW COUNT를 가지고 있기 때문에 SELECT count(*) 명령시 빠르고, SELECT 명령시에도 빠른 속도를 지원
- 풀텍스트 인덱스를 지원 
- Read Only기능이 많은 서비스일수록 효율적
- table level locking
  - SELECT INSERT UPDATE DELETE시 해당 Table 전체에 Locking이 걸린다.
- row의 수가 커지면 커질수록 CRUD 속도는 엄청나게 느려진다.


[참조](https://owlyr.tistory.com/22)
|   Details   |                                       MyISAM                                        |                                                                                                              InnoDB                                                                                                              |
| ----------- | ----------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Datafile    | - 인덱스(.MYI)와 데이터 파일(.MYD)가 분리<br/> - innoDB보다 파일 크기가 작다        | - 데이터량이 많을 경우 MyISAM에 비해 빠름<br/> - shared datafile : 인덱스와 데이터 공간이 공유<br/> - innodb_file_per_table : 테이블 단위의 데이터 파일로 분리<br/> - 테이블 정보는 shared datafile 에 저장                      |
| Transaction | - 테이블 락을 기본으로 insert, update, delete가 이루어진다.                         | - ib_logfileN log 파일을 통하여 대량의 트랜잭션을 버퍼링하고 serialization<br/> - 로그파일은 최소 2개의 그룹이며, 서로 rotate 스위칭되며 갱신되어 데이터를 파일로 적용한다.<br/> - Transation 이외에 Foreign Key, Trigger를 지원 |
| 속도        | - 인덱스와 데이터 파일이 테이블 단위이므로 속도가 빠르다                            | - 인덱스와 데이터 파일이 같은 파일에 있어 속도가 느림<br/> - 대량의 insert, update가 일어나면 fragment가 발생되어 더 느려진다.                                                                                                   |
| 백업        | - 테이블 단위의 hot backup(파일 복사) 가능<br/>- 테이블 파일만 있더라도 복구가 가능 | - 테이블 단위의 hot backup(파일 복사)가 불가능<br/> - mysqldump나 db 전체적인 복사가 필요<br/> - 백업에 반드시 메인 sharedDB파일도 같이 이루어 져야한다.                                                                         |
| 기타 특징   | - Table 단위 Lock 사용<br/> - COUNT(*) 정보를 별도 저장                             | - 행(Row)단위 Lock을 사용                                                                                                                                                                                                        |




## 3. char vs varchar vs text vs blob

### char
- 고정 사이즈
- 남는 공간 공백으로 채움
- char(n) n는 글자 수!!! byte 아님(10.5.10-MariaDB utf8 기준)

### varchar
- 가변길이
- 실질적인 데이터와 길이 정보도 같이 저장
  - 문자 길이용 추가 byte : 255글자 이하에는 1바이트, 그 이상은 2바이트의 추가 공간을 필요
  - 예를 들어 VARCHAR(10)에 ‘test’라는 4자짜리 문자열을 삽입하면
    4byte + 1byte(길이를 저장하기 위한 메모리) = 5byte가 소모된다.
- (조사 필요)기존 보다 큰 데이터를 저장하기 위해서 새로운 저장 영역에 새로 할당해야하므로 데이터 파편화가 발생


### 10.5.10-MariaDB utf8 에서 테스트 결과

```SQL
SELECT  
  f_c, LENGTH (f_c) AS byte, char_length (f_c) AS len, 
  f_vc, LENGTH(f_vc) AS byte, char_length(f_vc) AS len 
FROM test_utf8.tbl;
```

| char(10)             | byte  |  len  | varchar(10)          | byte  |  len  |
| -------------------- | :---: | :---: | -------------------- | :---: | :---: |
| DDDDDAAAAA           |  10   |  10   | DDDDDAAAAA           |  10   |  10   |
| 한글한글             |  12   |   4   | 한글한글             |  12   |   4   |
| 한글한글한글한글한글 |  30   |  10   | 한글한글한글한글한글 |  30   |  10   |



## 4. blob vs clob  
많은 양의 데이터를 저장하도록 설계된 타입 

- BLOB 
   - Binary Large Object 큰 이진객체
   - file과 같은 많은양의 이진 데이터를 저장

- CLOB Character Large Object 큰 문자객체
  - 많은양의 텍스트를 저장



## 5. db buffer pool
[참조](https://owlyr.tistory.com/23?category=893523)


### 개념
버퍼 풀은 `InnoDB`가 액세스 할 때 `테이블 및 인덱스 데이터를 캐시`하는 `메인 메모리 영역`

대량 읽기 조작의 효율성을 위해 버퍼 풀은 여러 행을 보유할 수 있는 페이지로 분할된다. 
캐시 관리 효율성을 위해 버퍼풀은 링크된 페이지 목록으로 구현된다. 거의 사용되지 않는 데이터는 다양한 알고리즘을 사용하여 캐시에서 종료(혹은 만료:aged out)된다.

버퍼 풀을 활용하여 자주 액세스하는 데이터를 메모리에 유지하는 노하우(기술)은 MySQL튜닝의 중요한 측면이다.


### 버퍼 풀의 크기 (innodb_buffer_pool_size)
> innodb_buffer_pool_size → 비어있는 메모리의 70〜80%정도를 할당

버퍼 풀은 두 가지 역할을 담당한다.

1. 데이터 파일과 로그 파일이 기록되는 순서를 조정하는 역할
2. 디스크 액세스를 줄이기 위한 캐시의 역할

시스템(OS)에서 파일 캐시의 크기가 클수록 성능에 유리하듯이, Database 에서도 마찬가지로 버퍼 풀의 크기가 클수록 성능에 유리하다.

특히 조회 처리를 위한 캐시 효과가 크기 마련인데, 이는 읽으려는 데이터가 메모리에 올라와 있으므로 Disk I/O 를 발생시키지 않기 때문이다. 이론적으로는 다른 버퍼에 할당하는 메모리를 제외하고는 대부분의 메모리를 버퍼 풀에 할당하는 것이 좋다.

인덱스 설계가 잘 되어 있는데도 슬로우 쿼리가 해결되지 않는다면?  
`innodb_buffer_pool_size` 파라메터를 의심해봐야 한다. (이름이 의미하듯이 InnoDB 스토리지 엔진에만 해당한다.)  
innodb_buffer_pool_size의 크기가 클수록 쿼리 실행시 디스크보다 메모리를 사용하게 되어 빠른 결과를 얻을 수 있다.

버퍼 풀 메모리가 충분히 큰 양으로 할당되어 있다면 innodb는 in-memory 데이터베이스처럼 동작한다.

Access를 위한 select 데이터 뿐 아니라, Insert 및 Update 작업에도 도움이 되는 캐싱을 하기 때문에 적절하게 조정하여 사용하는 것이 핵심이다.

버퍼 풀 메모리는 내부적으로 LRU 알고리즘을 사용하는 리스트의 형태이다.



### 버퍼 풀 인스턴스 수 (innodb_buffer_pool_instances)
> innodb_buffer_pool_instances → core 수 * 2

`innodb_buffer_pool_instances`는 설정된 `innodb_buffer_pool_size`를 쪼개어 병렬로 제어할 쓰레드의 개수, 각 인스턴스의 크기가 1GB 이상일 경우에만 작동

MySQL 5.5 부터 버퍼 풀의 인스턴스 수를 설정할 수 있는데, 인스턴스 수를 늘리면 `트랜잭션 간의 Lock 경합`을 줄일 수 있다. 

멀티 스레드 구조인 MySQL에서는 `스레드 간 버퍼 풀 조작에서 Exclusive Lock 처리가 필요`한데, 이 때 버퍼 풀접근을 위해 뮤텍스를 사용하고 동시 다발적으로 접근 시 **뮤텍스에 대한 경합**이 발생한다. 

**인스턴스 수를 늘릴수록 많은 수의 스레드가 동시에 버퍼 풀에 접근하더라도 Lock 경합을 피할 수 있다.** 

CPU 코어 수가 많은 시스템일수록 인스턴스 수를 늘릴 수 있다고 보면 된다. 인스턴스 수의 기본 값은 8 이다.


> innodb_buffer_pool_size = innodb-buffer-pool-instances * innodb_buffer_pool_chunk_size   
→ 이와 같지 않게 구성한다면 버퍼 풀 크기는 자동으로 innodb-buffer-pool-instances * innodb_buffer_pool_chunk_size와 같거나 여러 값으로 조정된다고 한다.


### 버퍼 관리자 
프로그램은 디스크로부터 블록을 가져와야 할 때 버퍼 관리자를 호출한다.
만약 필요한 블록이 버퍼에 이미 있다면, 버퍼 관리자는 메인 메모리의 블록 주소를 요청한 프로세스에 전달한다.
블록이 버퍼에 있지 않다면, 버퍼 관리자는

1. 먼저 버퍼에 블록을 저장하기 위한 공간을 할당한다.
이때, 버퍼가 가득 차있으면 read only block을 read only block이 없으면 dirty block을 디스크로 내보내고, 
보내진 블록은 디스크에 쓰인 가장 최근의 것과 비교해서 변경된 것이 있다면 update한다. 

2. 그 후 요청된 블록을 디스크에서 버퍼로 읽어와서 메인 메모리의 블록 주소를 요청한 프로세스에 전달한다. 


## 6. db log 종류
   1. 에러 로그(error log)
   2. 제네럴 로그(general log)
   3. 슬로우 쿼리 로그(slow query log)
   4. 바이너리 로그(binary log)
   5. 릴레이 로그(relay log)


