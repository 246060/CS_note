# Tablespace
Ref : https://pangtrue.tistory.com/190


데이터를 관리한다는 것은? 
-> 디스크에 데이터를 저장하고, 추출하고, 삽입하고, 삭제하는 작업을 한다는 것.

그럼 데이터는 어디에 저장되어 관리되어지는 것일까? 
-> 데이터 파일이다. (파일은 데이터가 저장되는 물리적인 공간)


MySQL 8.0 기준으로 총 5가지의 Tablespace가 존재한다.

1. System Tablespace
2. File-Per-Table Tablespace
3. General Tablespace
4. Undo Tablespace
5. Temporary Tablespace
 

데이터 파일에 아무런 체계없이 데이터를 무작정 저장하지는 않는다. 오라클 DBMS를 예로 들면, 오라클은 데이터 파일 내부의 공간을 4개의 논리적인 공간으로 나눠놨다.

1. 테이블스페이스 (Tablespace)
   - Segment를 담는 컨테이너
   - 여러 개의 데이터 파일을 묶어놓은 논리적인 저장공간

2. 세그먼트 (Segment)
   - 데이터 저장이 필요한 하나의 객체를 저장하는 단위 (테이블, 인덱스 등)
   - board 테이블을 만들면 하나의 세그먼트가 생성이 되는 것

3. 익스텐트 (Extent)
   - 공간을 확장하는 단위
   - board라는 테이블이 있으면 이는 하나의 세그먼트로 할당이 되어 데이터가 저장이될텐데, board 테이블을 최초 생성할 때에는 데이터가 없으니까 익스텐트를 하나만 생성해도 된다. 그러다가 레코드가 점점 늘어나면 저장 공간의 추가 할당이 필요할텐데, 그때의 확장 단위.

4. 데이터 블록 (Data Block) 
   - DBMS가 디스크에서 데이터를 읽고 쓰는 최소 단위.
   - 디스크 I/O 단위가 블록이므로, 특정 레코드 하나만 읽고 싶어도 해당 블록을 통째로 읽는다.
   - 오라클은 디폴트로 8KB 크기의 블록을 사용한다. (설정에 따라 2, 4, 16, 32KB도 사용할 수 있음)
   - 디스크에서 1Byte만 읽어오고 싶어도 어쩔 수 없이 8KB를 읽어와야한다. 
 

Tablespace 공간을 표현하면 아래 그림과 같다.

MySQL InnoDB 엔진의 디폴트가 file-per-table Tablespace인데, 이는 테이블 당 파일로 저장한다는 말이다.

테이블은 하나의 세그먼트와 대응되니깐, 하나의 세그먼트 당 하나의 파일로 저장한다는 얘기.

![ref1](https://blog.kakaocdn.net/dn/EfrLq/btqCn0wtdH7/GlnU22P2ftQgDZhjeAjgQ1/img.png)


 

 

이어서 Sequencial Access(Table Full Scan)와 Random Access(Index Range Scan)에 대해 살펴보자.

![ref2](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FcfqL0E%2FbtqCoRlQE2Y%2FKDIsmKn9Ci3WcfU4JHkdoK%2Fimg.png)


 

Sequencial Access는 논리적 또는 물리적으로 연결된 순서에 따라 차례대로 블럭을 읽는 방식이다.

익스텐트 내부에 있는 블록들을 1번부터 차례대로 읽어나가는 이 방식이 Full Table Scan이다.

 

Random Access는 논리적, 물리적 순서를 따르지 않고, 레코드 하나를 읽기 위해 한 블럭씩 접근하는 방식이다.

여기서 Table Full Scan과 Index Range Scan에 대해 한 가지 짚고 넘어가야할 것이 있다.

개발자들은 실행계획을 분석할 때 Table Full Scan이 나타나는지를 주로 보는데, 이것이 항상 옳은 것은 아니다.

Table Full Scan : 대용량의 데이터를 추출할 때 적합한 방식. ( Sequencial Access + Multiblock I/O )
Index Range Scan : 큰 테이블에서 소량의 데이터를 검색할 때 적합한 방식 ( Random Access + Single Block I/O )
 

 

 


