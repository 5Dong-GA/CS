# OS(운영체제) 정리
Computer의 h/w 관리를 해주는 것 == h/w 자원 신경 쓰지 않고 program 사용할 수 있게끔

OS 목적
-	처리능력 향상 : 일정시간동안 작업 최대한 많이 처리
-	응답시간 최소화 : 작업 빨리 처리
-	신뢰도 향상 : 작업 정확하게 처리
-	사용가능도 : 시스템 빨리 사용할 수 있어야 함

CPU가 I/O controller의 Data Buffer를 채우고 Write 명령 수행 시, I/O controller는 사용 가능 여부
확인하고 작업 수행, 작업 끝나면 Interrupt 발생시켜 CPU에게 작업 완료됨을 알림
cf) Interrupt
-	interupt service routine or interrupt handler에 전송(OS의 일부분)
-	interrupt 아키텍처는 interrupt된 명령어의 주소 저장해야함
-	Trap(오류 or 사용자 요청으로 s/w에서 생성된 interrupt) ex(page fault, 0으로 나누기등)

Register와 Cache는 CPU 내부에 존재(CPU가 memory에 더 빨리 접근하기 위해 나눈 구조)
Register: 연산을 위한 저장소 | Cache: CPU와 RAM 사이의 중간 저장소
Main Memory: CPU 외부에 있지만 CPU의 Cache와 연결됨
그 외 디스크들은 CPU 직접 접근 x -> data를 Main Memory로 옮기는 과정 필요

OS아래 여러 job들이 있음(memry에 유지), Scheduling 통해서 하나의 job이 선택됨
I/O 발생 시, 다른 job으로 Context Switching함


시 분할(멀티 태스킹) 시스템은 CPU가 스위칭을 자주 수행하는 방식
-	User는 memory에서 적어도 1개의 program 실행(Process)
-	모든 Job이 Ready이면 Scheduling 수행
-	Process들이 Memory에 맞지 않는다면 Swapping 수행
-	가상 메모리 : RAM이 부족해도 Process 수행하도록 해 줌

Dual Mode Operation: OS가 system 구성 보호할 수 있도록 User | Kernel mode로 나누어짐
일부 명령들은 Kernel에서만 수행 | System Call은 User -> Kernel로 바꾸고 종료 시 -> User
I/O 명령들은 privileged instruction으로 만들어져 Kernel Mode에서만 사용 가능

System Call: OS에 의해 제공되는 서비스에 대한 Programming Interface
-	User가 S/W interrupt 통해서 kernel 기능 이용하기 위해 서비스 요청하는 방식
-	번호가 할당되어 있음
-	1. Register에 그대로 매개변수를 전달 
-	2. 매개변수들을 메모리 블록에 저장, 해당 블록 주소를 register에 전달(Linux, Solaris)
-	3. OS에 의해 Stack에서 Pop or Program에 의해 Push 되는 방식

Kernel에서 사용되는 자료구조: Stack, Queue, List, Tree, Hash Function, Bitmap

----------------------------------------------------------------------------------------
# Process, Memory 관련
Program: disk에 존재하며 명령들의 리스트를 포함하는 파일
Process: Program과 Program Counter를 의미, 프로그램이 memory에 올라가면 Process가 됨
Code | Data | Stack | Heap | Program Counter
1. Data – 전역변수
2. Stack – 임시변수(지역) - 
3. Heap – 동적으로 할당된 객체들 포함하는 메모리

Process State
1. New – Process가 생성됨
2. Ready – CPU에 할당되기를 기다림
3. Running – 명령들 실행 중
4. Waiting – Event 발생 기다림
5. Terminated – 실행 종료됨
 
PCB(Process Control Block)
Process 관련 정보 포함, Process 상태, 번호 포함, Program Counter
CPU Registers, CPU Scheduling 정보, Memory 관리 정보, I/O 상태 정보

MultiThread Model – Process 생성 / Context Switching 부하가 크므로 Thread 사용
Thread
CPU 스케줄링 기본 단위이자 Process 내의 제어의 흐름 | PC, Register, Stack으로 구성
각 Thread는 Register 상태와 Stack을 가진다. -> Independent Executable
Process 내부에서 여러 Thread 생성 가능, Code | 주소공간 | OS 자원을 공유

Signal
UNIX 시스템에서 특정 event 발생시 Process에게 알리는 데 사용
1. Synchronous signals: signal 유발하는 작업 수행한 동일한 Process로 전달(0으로 나누기등)
2. Asynchronous signals: 외부 event에 의해서 발생
Thread Pool – 여러 개의 대기 Thread를 만들어 둠
	Thread 매번 새로 만드는 것보다 이미 만들어져 있는 거 사용하는 것이 빠름
	Thread Pool의 미리 만들 Thread 수 지정 가능
TLS(Thread Local Storage)
	각각의 Thread는 고유 스택이 있음. 전역, 정적 변수 같은 경우 Process내 모든 Thread가 공유하게 됨
	일반 전역 변수: 모든 Thread가 공유하므로 Race Condition 발생 가능
	정적/전역 변수를 Thread 각각 독립적으로 메모리 공간을 만들어주고 싶을 때

Process 실행 – CPU 실행 Cycle과 I/O 대기로 구성

CPU Scheduler – Main Memory에 있는 Ready 상태 Process들 중 하나에게 CPU 할당
1. Running -> Wait (I/O 요청 시)
2. Running -> Ready (Interrupt 발생)
3. Wait -> Ready (I/O 요청 완료)
4. 종료 시

Scheduling의 척도
1. CPU 사용률 극대화: CPU 놀지 않게끔 최대한 사용
2. 처리 능력 극대화: 주어진 시간에 최대한 많은 작업 처리(처리한 Process 수 / 시간)
3. 경과시간(Turnaround Time) 최소화
- Ready queue에서 대기한 시간 ~ 작업 마치고 반환하기까지 걸린 시간 최소화
4. 대기시간(Waiting Time) 최소화: Ready queue에서 대기한 시간의 총합
5. 응답시간(Response Time) 최소화
- Ready queue에 들어와서 처음으로 CPU 할당 받기까지 대기한 시간

FCFS(비선점 / CPU 먼저 요청한 Process가 먼저 처리됨)
-	Ready queue에 도착한 순서에 따라 CPU 할당, FIFO queue로 구현됨
-	Convoy Effect: 수행시간 긴 Process가 CPU 독점 가능해짐, 다른 Process들이 한무 기다림
-	Waiting Time이 긴 경우 종종 발생

SJF(선점 or 비선점 | Ready queue 내 Process중 수행시간 짧다고 판단되는 것을 먼저 수행)
-	각 Process CPU Burst 길이 비교 -> 가장 작은 CPU Burst Process에게 할당
-	평균 대기시간 최소인 최적 알고리즘

Priority Scheduling(선점 or 비선점 | 각 process에 우선순위번호 부여, 높은 Process부터 할당)
-	일반적으로 낮은 번호 -> 높은 우선순위
-	SJF도 일종의 우선순위 스케줄링이긴 함
-	기아문제: 낮은 우선순위 Process 영원히 수행되지 않을 수도 있음
-	Aging: 기아문제 해결하는 방법 -> 시간 지날수록 우선순위를 높이는 방식

Round Robin(선점)
-	각 Process는 동일한 크기의 Time Slice를 할당 받음
-	Time Slice동안 처리하고도 완료하지 못했다면 Ready queue의 맨 뒤로 간다.
-	CPU 제어는 그 다음 Process에게 넘어간다.
-	일반적으로 평균 처리시간 길지만 응답시간이 짧다.






Process Synchronize
공유 메모리에 동시에 접근하면 크나큰 문제가 생긴다!!

Race Condition – 여러 Process가 공유 데이터를 동시에 액세스하고 조작하는 상황
공유 Data의 최종적인 값은 마지막으로 완료되는 Process에 의해 결정됨 = 이러면 안됨
연산의 Atomic화가 필요하다

Critical Section (임계영역)
N개의 Process들이 공유 데이터 사용하기 위해서 경쟁하는 상황을 해결하기 위한 방법
공유 데이터에 액세스하게 해주는 Critical Section이라는 것을 가진다.
상호 배제: 한 Process가 그 곳에 access중인 경우 다른 Process가 실행하는 것을 허용하지 않음
진행(Progress): 임계영역에 Process 없을 경우 다음 들어갈 Process 정해준다.
한정대기(Bounded Waiting): Thread 기아 상태 막기 위해 임계영역에 대한 대기시간 한정시킴

구현방법
-	동기화 H/W 사용 ( TestAndSet | Swap )
-	Semaphore 사용
-	High-Level 동기화 구성 이용 (ex: monitor)







Memory 관리 (Program이 실행되려면 메모리로 가져와야 한다)
Input queue(job queue): program 실행 위해 memory로 가져오는 것을 기다리는 Process 집합

logical Address: Process마다 독립적으로 갖는 주소공간(Virtual Address), CPU가 인식하는 주소체계
Physical Address: 물리적인 Memory 주소, 실제 H/W의 Memory 주소
Symbolic Address: 변수 이름 선언, 그 이름 통해 주소에 접근

주소 바인딩: Instruction | Data를 메모리 주소에 바인딩하는 것
1. Compile Time
- 주소 변환이 Compile시 이루어짐 | 물리 memory 비어도 이미 결정되어 변경할 수 없다
- 주소 변환하려면 다시 Compile해야함 == Absolute Code
2. Load Time
- Compile시 memory 위치 알 수 없는 경우 -> 재배치 가능한 코드로 생성해야 함
- Symbol과 실제 주소의 binding은 program이 main memory에 적재될 때 이루어짐
3. Execution Time
- binding이 runtime 이후에도 진행 | CPU가 주소 참조할 때마다 binding 상태 점검 해야함
- MMU H/W 필요 (논리 주소를 물리 주소로 mapping 해주는 H/W)
- relocation register(물리 메모리 주소 최소값), limit register(논리 주소 범위) 이용해 매핑 수행
- CPU가 사용한 값 + relocation register 값 -> 실제 물리 메모리 주소 값으로 변환
- 범위 넘어가면 Trap 통해 오류 알림





Dynamic Loading vs Dynamic Linking
1. Dynamic Loading (OS가 지원 x, Programmer가 구현)
- 루틴이 호출되는 시점에 로드하는 방식
- 메모리 공간 사용 효율 증대: 사용되지 않는 루틴은 로드되지 않는다
- 사용 빈도가 낮은데 코드량이 많은 루틴이 필요할 때 유용
2. Dynamic Linking (OS에서 지원, 해당 루틴이 Process memory 공간에 있는지 확인해야함)
- 실제로 실행될 때까지 Linking이 지연된다(Dynamic Loading + Auto Linking)
- Compile 통해 생성된 File과 Library 사이 Linking을 실제로 수행되기 전까지 지연
-> Program 실행 중에 Library 링크할 필요 있을 때 사용
- Stub 포함, Stub(자신을 루틴의 주소로 변경하고 실행) | Shared Library에 유용

Swapping
Process는 일시적으로 Memory에서 disk로 swap 되었다가 실행 재개 시 다시 메모리로 가져옴
반드시 보조메모리 필요 | 보통 디스크가 사용
CPU는 Ready queue에서 Process 가져와 CPU 할당, CPU 스케줄러는 다음 Process 선택 시 
Dispatcher 호출, Dispatcher는 다음 실행할 Process가 Ready queue에 존재하는지 확인
없으면 Disk에서 가져옴
Swap 수행시간의 대부분 = 메모리 전송 시간

Memory Allocation
1. Contiguous Allocation: 고정 크기 | 가변 크기 분할
2. Non-Contiguous Allocation: Paging | Segmentation

1. Contiguous Allocation (Main Memory는 2개의 Partition으로 나누어짐)
- OS는 Interrupt vector와 함께 낮은 memory 주소에 올라감
- User Process들은 높은 memory 주소에 올라감
- relocation register의 시작 주소, limit register의 메모리 범위 이용해서 서로 불가침하게 함

고정 크기 (이제 사용 안됨)
-	memory를 고정된 크기의 Partition으로 분할
-	각 Partition은 하나의 Process 가짐
-	Partition 수에 따라 Multi Programming 정도 결정
가변 크기
-	Hole: 사용 가능한 Memory Block -> 다양한 크기의 Hole이 memory에 흩어져 있음
-	Process 도착 시 할당될 수 있는 충분한 크기의 Hole 찾아 할당
-	OS는 Allocated-partition, Free-partition에 대한 정보 유지
-	할당 시 방법이 3가지가 있다 (First-Fit, Best-Fit, Worst-Fit)
-	cf) 50% rule: N개 memory block 있을 때, 0.5N개의 memory block은 Fragmentation 발생

Fragmentation (단편화)
1. External Fragmentation
- 메모리의 전체적인 공간 계산 시 Process 할당 가능하지만 연속적이지 않아서 불가능 한 경우
- Compaction(Hole들을 하나로 합쳐 단편화 영역들을 몰아주는 것) 통해 외부 단편화 감소 가능
- 각 Process의 주소들을 동적으로 relocation해야 하기 때문에 부하가 크다
2. Internal Fragmentation
- 할당된 메모리 영역이 Process 크기보다 약간 큰 경우
- 할당되고 남은 영역이 애매하게 많으면서도 작은 형식으로 낭비가 생길 때

Paging (페이징)
-	물리 memory를 고정된 크기의 블록으로 나누는데 이를 Frame이라함(크기: 2의 승수)
-	가상 주소 Memory를 같은 크기의 블록으로 나누는데 이를 Page라함
-	가장 주소를 물리 주소로 변경하기 위해 Page Table을 사용
-	외부 단편화 문제는 해결되지만, 내부 단편화 문제는 여전히 존재함
-	Page Table은 32Bit 기준 4GB에 해당하는 Page 수를 가짐

주소 변환
가상 주소 = Page 번호 (p): page table에서 page의 index | 각 page별 frame 시작 주소 있음
         + Page Offset (d)
         = 물리 memory 주소
페이지 크기 2^n이고 가상 주소 공간 2^m이면
  
1. 가상메모리 4GB 사용 = 2^32 -> m은 32
2. 페이지 크기 4KB     = 2^12 -> n은 12
p = m – n = 32 – 12 = 20 | 상위 20 bit는 page 번호, 하위 12 bit는 offset을 나타냄
page 수 = 4GB / 4KB = 2^20개 => 2^20개의 번호가 존재
page 내의 범위를 offset에 담는다 | 한 page = 2^12 -> 0~2^12 – 1의 수가 들어감

