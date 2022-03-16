# Operating System

## Index
1. Kernel과 Shell
2. Task
   1. 프로세스 구성요소: 코드, 데이터, 스택, 프로세스 제어 블록(PCB: Process Control Block)
   2. 프로세스 제어 블록(PCB: Process Control Block) 구성요소
   3. 프로세스 스케줄링
      1. 상태: 생성, 준비, 실행, 대기, 완료
      2. 상태 전이
         1. 디스패치(Dispatch)
         2. 할당 시간 쵸과(Timeout)
         3. 입출력 발생(Block)
         4. 깨움(Wake-Up)
      3. 성능 관련 용어
         1. 서비스 시간
         2. 응답 시간(Response Time)
         3. 반환 시간(Turnaround Time)
         4. 대기시간
         5. 평균 대기시간
         6. 종료 시간
         7. 시간 할당량(Time Quantum or Time Slice)
         8. 응답률
      4. 스케줄링 알고리즘
         1. 유형
            1. 선점형
            2. 비선점형
         2. 선점형
            1. 라운드 로빈
            2. SRT(Shortest Remaining Time First)
            3. 다단큐(MLQ: Multi Level Queue)
            4. 다단계 피드백 큐(MLFQ: Multi Level Feedback Queue)
         3. 비선점
            1. 우선순위
            2. 기한부
            3. FCFS(First Come First Served)
            4. SJF(Shortest Job First)
            5. HRN(Highest Response Ratio Next)
   4. 교착상태
      1. 발생조건
         1. 상호배제
         2. 점유와 대기
         3. 비선점
         4. 환형 대기
      2. 해결방법
         1. 예방
         2. 회피
            1. 은행가 알고리즘
         3. 발견
            1. 자원할당 그래프
            2. Wait for Graph
         4. 복구
            1. 프로세스 Kill, 자원선점
   5. 동시성
      1. Deadlock
      2. 뮤텍스와 세마포어
         1. 뮤텍스
         2. 세마포어
   6. 쓰레드
      1. 유저 쓰레드
      2. 커널 쓰레드
      3. 프로세스와 쓰레드 비교
3. 메모리
   1. 가상메모리
      1. 메모리 관리 장치(MMU)
   2. 스왑
   3. 메모리 관리 기법
      1. 반입 기법
         1. 요구 반입 기법
         2. 예상 반입 기법
      2. 배치 기법
         1. 최초 적합(First Fit)
         2. 최적 적합(Best Fit)
         3. 최악 적합(Worst Fit)
      3. 할당 기법
         1. 연속 할당 기법
         2. 분산 할당 기법
            1. Paging
            2. Segmentation
            3. Paging + Segmentation
      4. 교체 기법
         1. Page Fault
         2. 프로세스 Swap In/Out
         3. FIFO, LRU, LFU
   4. 단편화
      1. 내부 단편화 : Paging
      2. 외부 단편화 : Segmentation
   5. 페이징 기법의 문제 및 해결 방안
      1. 스레싱(Thrashing)
      2. 위킹 세트
      3. 페이지 부재 빈도(PFF: Page-Fault Frequency)
   6. 지역성
      1. 시간
      2. 공간
4. 디스크
   1. 디스크 드라이버
   2. 디스크 스케줄링
      1. 종류
         1. FCFS(=FIFO)
         2. SSTF(Shortest Seek Time First)
         3. SCAN
         4. C-SCAN(Circular SCAN)
         5. LOOK(엘리베이터 알고리즘)
         6. N-STEP SCAN
         7. SLTF(Shortest Latency Time First)
   3. 파일시스템
   4. VFS
   5. 파일 디스크립터(파일 서술자, 파일 제어 블록(File Control Block))
   6. i-node
5. 인터럽트