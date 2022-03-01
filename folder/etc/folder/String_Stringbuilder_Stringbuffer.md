# String, StringBuilder, StringBuffer

## String
- 새로운 값을 할당할 때마다 새로 클래스에 대한 객체가 생성된다.
- String에서 저장되는 문자열은 private final char[]의 형태이기 때문에 String 값은 바꿀수 없다.
  - private: 외부에서 접근 불가
  - final: 초기값 변경 불가

- String + String + String…
  - 각각의 String 주솟값이 Stack에 쌓이고, Garbage Collector가 호출되기 전까지 생성된 String 객체들은 Heap에 쌓이기 때문에 메모리 관리에 치명적이다.

- String을 직접 더하는 것보다는 StringBuffer나 StringBuilder를 사용하는 것이 좋다.

## StringBuilder, StringBuffer
memory에 append하는 방식으로, 클래스에 대한 객체를 직접 생성하지 않는다.

### StringBuilder
- 변경가능한 문자열
- 비동기 처리

### StringBuffer
- 변경가능한 문자열
- 동기 처리
- multiple thread 환경에서 안전한 클래스(thread safe)
  

