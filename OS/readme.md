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
