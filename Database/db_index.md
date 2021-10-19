# DB Index

1. 목적
2. 과정
3. 상황 분석
4. DML이 일어났을 때의 상황
5. B-Tree

<br/><br/>

## 목적
인덱스는 데이터 저장, 수정, 삭제에 대한 성능을 희생시켜 탐색에 대한 성능을 대폭 상승하는 방식  
B-tree 자료구조를 이용

Table의 Column을 색인화 함 (따로 파일로 저장)

→ 해당 Table의 Record를 Full scan 하지 않음
→ 색인화 된 (B+ Tree 구조로) Index 파일 검색으로 검색 속도 향상


<br/><br/>

## 과정
Table을 생성하면, MYD, MYI, FRM 3개의 파일이 생성됨.

- FRM : 테이블 구조가 저장되어 있는 파일
- MYD : 실제 데이터가 있는 파일
- MYI : Index 정보가 들어가 있는 파일


Index를 사용하지 않는 경우, MYI 파일은 비어져 있음.  
그러나, 인덱싱하는 경우 MYI 파일이 생성됨.

이후에 사용자가 Select 쿼리로 Index를 사용하는 Column을 탐색 시,  
MYI 파일의 내용을 검색함.


#### 단점
1. Index 생성시, .mdb 파일 크기가 증가함
2. 한 페이지를 동시에 수정할 수 있는 병행성이 줄어듬.
3. 인덱스 된 Field에서 Data를 업데이트하거나, Record를 추가 또는 삭제시 성능이 떨어짐.
4. 데이터 변경 작업이 자주 일어나는 경우, Index를 재작성해야 하므로, 성능에 영향을 미침.


<br/><br/>

## 상황 분석

#### 사용하면 좋은 경우

 1. Where 절에서 자주 사용되는 Column
 2. 외래키가 사용되는 Column
 3. Join에 자주 사용되는 Column

#### Index 사용을 피해야 하는 경우

 1. Data 중복도가 높은 Column
 2. DML이 자주 일어나는 Column


<br/><br/>

## DML이 일어났을 때의 상황

### INSERT
기존 Block에 여유가 없을 때, 새로운 Data가 입력됨

-> 새로운 Block을 할당 받은 후, Key를 옮기는 작업을 수행 (**많은 양의 Redo가 기록**되고, 유발)

-> Index split 작업 동안, 해당 Block의 Key 값에 대해서 DML이 블로킹 됨... 대기 이벤트 발생


<br/>

### DELETE

[Table과 Index 상황 비교]

Table에서 data가 delete 되는 경우 : Data가 지워지고, 다른 Data가 그 공간을 사용 가능  
Index에서 Data가 delete 되는 경우 : Data가 지워지지 않고, 사용 안 됨 표시만 해둠.

-> Table의 Data 수와 Index의 Data 수가 다를 수 있음.

<br/>

### UPDATE

Table에서 update가 발생하면 -> Index는 Update 할 수 없음
Index에서는 Delete가 발생한 후, 새로운 작업의 Insert 작업 (2배의 작업이 소요)


<br/><br/>

## B-Tree
[참조](https://helloinyong.tistory.com/296)

### DB 인덱스로 B-Tree가 가장 적합한 이유

1. 항상 정렬된 상태로 특정 값보다 크고 작은 부등호 연산에 문제가 없다. 
   - hash table 부적합 이유

2. 참조 포인터가 적어 방대한 데이터 양에도 빠른 메모리 접근이 가능하다. 
   - red-black 부적합 이유

3. 데이터 탐색뿐 아니라, 저장, 수정, 삭제에도 항상 O(logN)의 시간 복잡도를 가진다.
   - 배열 부적합 이유




