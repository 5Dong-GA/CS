# 네트워크 총 정리

# Protocol 
서로 다른 시스템에 있는 개체 간에 성공적으로 데이터를 전송하기 위한 통신 규약
7계층으로 정의되어 있고 각 층마다의 역할이 구분되어 있다.

# OSI 7계층 Layer
왜 7계층? 단계별로 파악 가능하고 문제 발생시 그 계층만 고치면 해결 가능하기 때문
특징
-	송신은 7 -> 1 계층으로, 수신은 1 -> 7 계층으로 진행
-	계층의 독립: 어떤 계층의 변화가 다른 계층에 영향을 주지 않는다.
-	상위 > 하위: 하위 계층은 상위 계층을 위해 기능하고 상위는 하위에 관여 X

# 1. Physical Layer (물리 계층)
전기적, 기계적, 기능적인 특성 이용해 통신 케이블로 Data 전송
-	사용되는 통신 단위는 Bit (1 or 0)
-	Data를 전기적인 신호로만 변환해서 주고받는 기능
-	장비로는 통신케이블, Repeater, Hub가 있다.
-	Data 전송만 하고 어떤 Error가 있는지 신경 쓰지 않는다?

# 2. Data Link Layer(데이터 링크 계층)
물리계층 통해 송수신되는 정보의 오류, 흐름을 관리해 안전한 전달 수행하도록 도와주는 계층
-	MAC 주소로 통신(data link 계층위한 network interface에 할당된 고유 식별자)
-	사용되는 통신 단위는 Frame
-	장비로는 Bridge, Switch가 있다.
-	물리 계층에서 발생할 수 있는 오류 찾고, 수정하는데 필요한 기능, 절차 수단 제공

# 3. Network Layer(네트워크 계층)
데이터를 목적지까지 가장 안전하고 빠르게 전달하는 기능 (= Routing)
-	경로를 선택, 주소 정하고, 경로에 따라 패킷을 전달하는 것이 역할
-	장비로는 Router가 있다.
-	여러 개의 Node를 거칠 때마다 경로를 찾아주는 역할을 한다.
-	Routing, Flow Control, Segmentation, Error Control, Internetworking등을 수행
-	IP를 할당해주는 역할

# 4. Transport Layer(전송 계층)
통신 활성화하기 위한 계층. TCP Protocol 이용해, Port 열어 전송할 수 있게끔 
만약 데이터가 왔다면 4계층에서 해당 데이터를 하나로 합쳐서 5계층에 전달
-	단대단 오류제어, 흐름제어 이 계층 까지는 물리적인 계층 느낌이다.
-	TCP / UDP Protocol 사용
-	양 끝단의 사용자들이 신뢰성 있는 데이터 주고받게 해 줌
-	상위 계층들이 데이터 전달의 유효성이나 효율성 생각하지 않도록 해 줌
-	Sequence Number 기반의 오류 제어 방식을 사용한다.
Sequence Number란?
	통신과 제어에서 Data를 관리하기 위해 번호를 부여 ex) TCP 패킷 헤더의 ACK number
	해당 번호는 순서 역전 방지, 중복 패킷 방지
	데이터를 한 번에 송수신하지 않고 패킷 단위로 작업하니 섞이거나 방지하기 위해 사용
-	특정 연결의 유효성을 제어하고, 일부 프로토콜은 상태개념이 있고 연결기반이다.
	패킷들의 전송이 유효한지 확인, 전송 실패한 패킷들은 다시 전송
	종단간 통신을 다루는 최하위 계층으로 신뢰성 있고 효율적인 데이터를 전송
	기능은 오류 검출, 복구, 흐름 제어, 중복 검사 등을 수행

# 5. Session Layer (세션 계층)
데이터가 통신하기 위한 논리적인 연결을 의미한다.
종단간 프로세스가 데이터 통신을 관리하기 위한 방법을 제시하는 계층
-	동시 송수신 방식, 반이중 방식, 전이중 방식 / 체크 포인트 유무, 종료, 다시 시작 과정
-	TCP/IP 세션을 만들고 없애는 책임을 진다.

# 6. Presentation Layer (표현 계층)
데이터 표현이 상이한 응용 Process의 독립성을 제공하고, 암호화한다.
-	코드 간의 번역을 담당하여 사용자 시스템에서 데이터의 형식상 차이를 다루는 부담을 응용 계층으로부터 덜어준다. MIME econding이나 암호화 등의 동작이 이루어짐

# 7. Application Layer (응용 계층)
사용자가 보는 S/W의 UI, Network Service, 사용자 I/O 부분 등을 담당하는 계층이다.
HTTP, FTP, SMTP, POP3, IMAP, Telnet 등과 같은 Protocol이다.
해당 통신 패킷들은 방금 나열한 Protocol에 의해 모두 처리되며 우리가 사용하는 브라우저나, 메일 프로그램은 Protocol을 보다 쉽게 사용하게 해주는 응용프로그램이다.
즉 모든 통신 양 끝단은 HTTP와 같은 Protocol이지 응용프로그램이 아니다.
-	응용 프로세스와 직접 관계하여 일반적은 응용 서비스를 수행
-	일반적인 응용 서비스는 관련된 응용 프로세스들 사이의 전환을 제공 ex) JVM, Terminal







# Port 
그 컴퓨터 안에서 프로그램을 찾기 위한 수단(전송계층에서 응용프로그램 구분)
논리적인 접속 장소를 의미하고 인터넷 프로토콜인 TCP/IP를 사용할 때 Client가 network 상의 특정 서버 프로그램을 지정하여 사용
IP
컴퓨터를 찾을 때 필요한 주소
# HTTP
web 상에서 web server 및 web browser 상호 간의 데이터 전송을 위한 응용계층 프로토콜이다.
특징
-	요청과 응답 구조
	동작 형태는 Client / Server 로 동작한다.
-	메시지 교환 형태의 Protocol
	Client와 Server 간 HTTP 메세지를 주고받으며 통신
-	트랜잭션 중심의 비연결성 Protocol
	종단간 연결 없음, 이전의 상태를 유지하지 않음
-	전송 계층 Protocol: TCP, Port 번호: 80
HTTPS
HTTP + S(Secure Sockets Layer): 보안 소켓 계층 사용함으로써 보안 유지 가능








# TCP/IP
데이터가 의도된 목적지에 닿을 수 있도록 보장해주는 통신 규약이다.
TCP와 IP Protocol로 이루어져 있다.

# TCP
두 호스트가 교환하는 데이터와 승인 메세지의 형식을 정의하며
Server와 Client간의 Data를 신뢰성 있게 전달하기 위해 만들어진 규약이다.
-	일련 번호 부여 -> data 손실 찾아내서 교정, 순서 재조합, Client에게 전달
-	복잡해서 신뢰성이 높다.
-	비연결성, 비신뢰성, 프로그램 구분
-	위와 같은 문제점들을 해결하기 위해서 TCP 3-way handshake 방식 적용
TCP 3-way handshake
-	Client에서 정보를 받는 곳과 SYN(연결)을 확인한다.
-	서버는 SYN 확인하고 거기에 ACK(인증)을 보내 정보전달 가능함을 알림
-	다시 Client에서 ACK를 보내면 이제 데이터 전송 준비가 완료된 것

# IP
32bit로 구성되어 있는데 4등분하여서 8비트씩 4개로 쪼갬
IP는 TCP와는 달리 Data의 재조합이나 손실여부 확인이 불가능, 단지 data 전달하는 역할
-	MAC 주소와는 다르게 임시 주소이므로 바뀔 수 있다.
IP 기반에 TCP가 사용되어 TCP/IP라 함
TCP가 Data의 추적을, IP가 전송을 처리한다.

# UDP와 TCP의 차이
TCP, UDP란 전송 계층에서 사용하는 Protocol로 목적지 장비까지 전송한 패킷을
상위의 응용 Protocol에게 전달하는 것에 목적을 가진 전송 방식


# UDP (User Datagram Protocol)
비연결형 서비스를 지원하는 전송계층 Protocol로써, Internet상에서 서로 정보를 주고받을 때 정보를 보낸다는 신호나 받는다는 신호 절차를 거치지 않고, 보내는 쪽에서 일방적으로 Data를 전달하는 통신 Protocol이다.
-	Data를 Datagram 단위로 처리하는 Protocol이다.
-	신뢰성이 낮은 Protocol -> 완전성을 보증하지 않는다.
-	가상회선 굳이 확립할 필요가 없고 유연, 효율적 응용의 Data 전송에 사용
	일방적인 Data 전달(중간에 신호절차 없음)
	순서제어, 흐름제어, 오류제어가 없다
-	비연결접속상태에서 통신한다.
-	실시간 응용 및 멀티캐스팅 가능
	빠른 요청, 응답이 필요한 실시간 응용에 적합, 일대다에 적합하다.
-	헤더가 단순하다.
	헤더의 고정크기가 8비트만 사용하여 헤더처리에 많은 시간 쓰지 X
-	신뢰성보다는 연속성이 중요한 서비스 ex) 실시간 Streaming
Telnet, SSH
Telnet
인터넷이나 로컬영역 네트워크 연결에 쓰이는 네트워크 프로토콜
Internet과 같이 신뢰할 수 없는 Network 사용하는 경우 안전하지가 않다.
-	일반 Text로 data를 교환해서 기밀 데이터 전송하는데 적합하지 않다.
SSH (Secure Shell)
Internet 또는 Network 내에서 두 원격 호스트간 보안 연결을 설정하는데 사용되는 Protocol
-	암호화된 형식을 사용하여 Computer간에 Data 전송함
-	사용자 이름, 비밀번호 등 기밀 데이터를 모두 암호화된 형식으로 되어있다.
-	원격 로그인, 시스템 및 원격 명령 실행이 가능하다. (보안에 강함)

# DNS (Domain Name System)
호스트의 도메인 이름을 호스트의 Network 주소로 바꾸거나 그 반대의 변환 수행하는 System
1. Domain 이름 입력
2. DNS server로 해당되는 IP 주소를 물어보고
3. DNS server는 해당 Domain의 IP를 알려준다.
4. Computer는 이를 받아서 IP 주소에 해당하는 Computer에 접속하게 됨

# Netmask (넷마스크)
Network의 주소 부분의 비트를 1로 치환한 것이 Netmask이다.
IP 주소와 Netmask를 AND 연산을 하면 Network 주소를 얻을 수 있다.

# Subnetmask (서브넷 마스크)
형태
-	IP주소와 똑같이 32bit로 구성 8bit마다 .으로 구분한다. 
-	형태가 똑같은 이유 == AND 연산을 하기 위해서
Default Subnetmask
-	IP Class를 나눈다는 이유 == Subnetmask를 사용할 것이다.
-	C class를 사용한다는 것은 C class network를 쪼개지 않고 그대로 하나의 네트워크에 할당할 수 있는 2^8 - 2개의 host ID를 사용하겠다는 뜻
-	A : 255.0.0.0 B : 255.255.0.0 C : 255.255.255.0
-	별도 서브넷 마스크를 생성하지 않고도 기본적으로 적용되어 있는게 기본 서브넷 마스크
최근에는 Netmask와 Subnetmask를 구분하지 않는다. 왜? CIDR이후 subnet mask만 사용함




Nagle Algorithm

