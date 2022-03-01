# Java Collection

여러 원소들을 담을 수 있는 자료구조
데이터의 집합, 그룹을 의미

데이터, 자료구조인 컬렌션과 이를 구현하는 클래스를 정의하는 인터페이스를 제공

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2F4wkUX%2Fbtq44bqazqr%2F80s6KaCrGqfETw4Pk5CGqK%2Fimg.png)

## 종류
- ArrayList
- Vector
- LinkedList
- Stack
- Queue
- List
- Set
- PriorityQueue
- Deque
- ArrayDeque
- SortedSet
- HashSet
- HashMap
- HashTable
- TreeSet
- TreeMap
- LinkedHashSet
- LinkedHashMap
- ConcurrentHashMap 
- WeakHashMap

Iterable


## List
순서대로 저장하고 중복 저장 가능


## Set 인터페이스
중복 저장이 안됨

1. 구현 방식
 - HashSet은 해싱을 이용하여 구현
 - TreeSet은 이진탐색트리(BinarySearchTree)를 이용하여 구현(정렬상태 유지)
   - 데이터가 정렬된 상태로 저장되는 이진 탐색 트리(binary search tree)의 형태로 요소를 저장

2. 속도 비교
 - HashSet > TreeSet
 - 해싱이 이진탐색트리보다 빠르다

3. 정렬 기능
 - TreeSet
 - 이진탐색트리를 이용했기 때문에 데이터 정렬이 가능 (Comparator 이용)
  
성능
HashSet > TreeSet > LinkedHashSet




Set 컬렉션은 저장 순서가 유지되지 않는다.
객체를 중복 저장할 수 없고, 하나의 null만 저장 할 수 있다.
Set 컬렉션은 수학의 집합에 비유 될 수 있는데, 순서와 상관없이 중복이 허용되지 않기 때문이다.

Set 인터페이스를 구현한 클래스로는 HashSet과 TreeSet, LinkedHashSet이 있는데 HashSet의 경우 정렬을 해주지 않고 
TreeSet의 경우 오름차순으로 자동정렬을 해준다는 차이점이 있다. 
LinkedHashSet은 입력된 순서대로 데이터를 관리한다.

이를 정리하면 아래와 같다.

클래스 명	특징
HashSet	Hashing을 이용해서 구현한 컬렉션이다.
데이터를 중복 저장할 수 없고, 순서를 보장하지 않는다.
equals()나 hashCode()를 오버라이딩해서,
인스턴스가 달라도 동일 객체를 구분해 중복 저장을 막을 수 있다.

TreeSet	이진탐색트리(Red-Black Tree)의 형태로 데이터를 저장한다.
데이터 추가, 삭제에는 시간이 더 걸리지만, 검색과 정렬이 더 뛰어나다.
기본적으로 오름차순으로 데이터를 정렬한다.
SortedSet 인터페이스 구현체 중 하나

LinkedHashSet	HashSet 클래스를 상속받은 LinkedList이다.
데이터에 삽입된 순서대로 데이터를 관리한다.

HashSet, TreeSet, LinkedHashSet의 시간 복잡도
HashSet	TreeSet	LinkedHashSet
O(1)	O(log n)	O(1)

HashSet이 TreeSet이나 LinkedHashSet보다 성능이 더 빠르고, 메모리를 적게 사용한다.


Set의 가장 큰 장점은 중복을 자동으로 제거해준다는 점이다.




## Map 인터페이스
map는 기본적으로 entry(key, value) 형태로 관리된다.
키(Key), 값(Value)의 쌍으로 이루어진 데이터으 집합으로,

순서는 유지되지 않으며 키(Key)의 중복을 허용하지 않으나 값(Value)의 중복은 허용한다.

### Hashtable
- HashMap보다는 느리지만 동기화 지원
- single lock
- null 허용하지 않음
  
### HashMap
- 중복과 순서가 허용되지 않으며 null값이 올 수 있다.
  
### TreeMap
- `정렬된 순서`대로 키(Key)와 값(Value)을 저장하여 검색이 빠름
- 키 값 기준으로 정렬된 상태

### LinkedHashMap
- `입력된 순서`를 보장하며 키와 값을 저장

### ConcurrentHashMap
- multiple lock
- update 할 때만 동기처리
- null 허용하지 않음