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
  - [5. db buffer pool size](#5-db-buffer-pool-size)
  - [6. db log 종류](#6-db-log-종류)



## 1. Data block
- logical block






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
| 한글한글               |  12   |   4   | 한글한글              |  12   |   4   |
| 한글한글한글한글한글     |  30   |  10   | 한글한글한글한글한글     |  30   |  10   |



## 4. blob vs clob  
많은 양의 데이터를 저장하도록 설계된 타입 

- BLOB 
   - Binary Large Object 큰 이진객체
   - file과 같은 많은양의 이진 데이터를 저장

- CLOB Character Large Object 큰 문자객체
  - 많은양의 텍스트를 저장



## 5. db buffer pool size


## 6. db log 종류
   1. 에러 로그(error log)
   2. 제네럴 로그(general log)
   3. 슬로우 쿼리 로그(slow query log)
   4. 바이너리 로그(binary log)
   5. 릴레이 로그(relay log)


