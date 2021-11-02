# DDL(Data Definition Language)
 - 데이터베이스를 정의하는 언어
 - 골격을 결정하는 역할
 - 조작 단위: 테이블 이상의 객체
 - SCHEMA, DOMAIN, TABLE, VIEW, INDEX를 정의하거나 변경 또는 삭제할 때 사용
 
## 종류
 1. CREATE : 데이터베이스, 테이블등을 생성 
 2. ALTER : 테이블 수정 
 3. DROP : 데이터베이스, 테이블 삭제 
 4. TRUNCATE : 테이블 초기화 



<br/><br/><br/>

# DML(Data Manipulation Language) 
- 데이터 조작어 
- 레코드를 조회하거나 수정하거나 삭제하는 등의 역할
- 조작 단위: 레코드 

## 종류
 1. SELECT : 레코드 조회
 2. INSERT : 레코드 삽입
 3. UPDATE : 레코드 수정
 4. DELETE : 레코드 삭제




<br/><br/><br/>

# DCL(Data Control Language)
- 데이터베이스에 접근하고 객체들을 사용하도록 권한을 주고 회수하는 명령어
- 조작 단위 : 권한

## 종류
 1. GRANT : 사용자 권한 부여
 2. REVOKE : 사용자 권한 회수




<br/><br/><br/>

# TCL (Transaction Control Language)
- 조작 단위 : 트랜잭션

## 종류
 1. COMMIT : 트랜잭션 작업 결과 반영
 2. ROLLBACK : 트랜잭션 복구
 3. SAVEPOINT : 저장점(SAVEPOINT)을 정의하면 롤백(ROLLBACK)할 때 트랜잭션에 포함된 전체 작업을 롤백하는 것이 아니라 현 시점에서 SAVEPOINT까지 트랜잭션의 일부만 롤백





<br/><br/><br/>
Reference
- https://server-talk.tistory.com/159