
# GC

## 알고리즘
### mark and sweap
- root 부터 탐색하여 mark하고 mark 되지 않는 객체를 sweap(정리)
- 메모리 파편화 방지를 위해 compaction 함.(사용하는 객체를 한쪽으로)(필수는 아님)
- 의도적으로 실행시켜야 함.
- 어플리케이션 실행과 gc 실행이 병행된다.


## parallel gc
[참조](https://www.youtube.com/watch?v=FMUpVA0Vvjw)
- default java 8

## g1gc
- default java 9

## Zgc 
- ZGC was initially introduced as an experimental feature in JDK 11, and was declared Production Ready in JDK 15.