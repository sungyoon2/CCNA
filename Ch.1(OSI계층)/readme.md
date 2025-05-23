# 2024.12.28(OSI 7계층)

기본 용어
---
```
<용어 정리>
1. LAN : 2Km이내, 고속 통신이 목적

2. WNA : 광역, 도시-도시, 나라-나라, ISP(Internet service)업체들이 보유 -> AS넘버를 보유함

3. 토플로지 : 네트워크 연결형태를 말함
 1) 성형   : - 중앙집중식 구조
             - 중앙의 교환장비가 데이타 경로를 개설하고 유도
             - 이용자의 스테이션은 중앙의 교환장비에 point-to-point 링크로 연결

 2) 버스형 : - 어떠한 통신망에서 선을 따 통신선으로 끌어다가 쓰는 방식이다. 멀티탭의 원리와 동일하다고 보면 된다.
             - 필요할때마다 선을 끌어쓰면 되기 때문에 기기가 추가될때마다
             - 추가적인 회선을 깔 필요가 없어져 구축비용을 아낄 수 있다.
             - 하지만 한 선에 데이터가 몰리게 된다면, 병목 현상이 일어나게 된다

 3) 망형   : - 네트워크상의 모든 노드를 상호 연결
             - 통신선로의 총길이가 가장 긴 네트워크 구조
             - 초기 데이타 통신 네트워크의 전형적인 형태
             - 공중통신망에 많이 사용

 4) 트리형  : - 지역과 거리에 따라 연결하므로 통신선로의 총경로가 가장 짧음
              - 접속되는 단말기의 숫자에 맞는 통신장비 이용이 가능

 5) 링형    : - 각 링크가 단방향이어서 데이터는 한 방향으로만 전송
              - 각 노드는 데이타의 송수신을 제어하는 엑세스 제어논리(토큰)을 보유

4. Internet : 수많은 통신망 종류를 연결한 거대 통신망(LAN, PAN, WAN)

5. Protocol : 통신을 하기 위해 미리 약속된 규칙
            : 통신을 하기 위한 Program
            : 1대이상의 데이터 전달을 위해 사용
  1) 3요소 : 올바른 데이터 교환을 위한 요소
    (1) 구문(Syntax) : 문법적인 요소
    (2) 의미(Meaning) : 특정의미의 적절한 선택
    (3) 타이밍(Timing) : 적절한 시기의 사용

6. Network Model -> 아무렇게나 만들면 문제가 생길 가능성이 높은 점을 해결하기 위해 등장
  1) 목적
    (1) 망설계시 지침서
    (2) 하자 해결을 위한 문제 해결의 방식
    (3) 표준 프로토콜을 제정할 필요성이 대두(나라, 기업마다 사용하는 프로그램이 달랐기에) -> OSI 모델 등장 -> 다른 기종사이에도 호환이 가능

7. OSI 7 계층과 TCP/IP 모델
  1) 구분 
      OSI 모델                    TCP/IP 모델                    내용
  L7 응용계층                     \
  L6 표현계층                     애플리케이션                    OS(운영체제)
  L5 세션계층                     /
  L4 전송계층                     전송계층                        송수신간(종단to 종단) 데이터의 안전한 전송(신뢰성) -> 데이터가 온전하게 도착여부 확인
  L3 네트워크계층                 네트워크계층                     멀리 떨어져있는 상대방에게 갈때 네비게이션 역할 -> 최적 경로 탐색
  L2 데이터링크계층               데이터링크계층                   장치간(node to node)(point to point) 연결 -> 장치간 데이터 운송방식
  L1 물리계층                     물리계층                        장치간 물리적 연결 -> 장치간 연결
```

OSI 7계층
---

> <물리 계층><br>
```
1. 디지털 신호(전기적 신호)가 잘 연결(전달)되도록 하기 위한 장치적 부분

2. 장치
  1) 케이블 : 전기를 이용해서 신호를 전달하는 선
  2) 리피터 : 신호를 증폭
  3) 허브  : 연결

* 신호 : 부호를 전달하는 형태
  부호 : 의미를 가지는 약속된 기호로 이미 기기안에 저장되어 있음
  ### 기본원칙 -> 선저장 후처리
(신호 전달시 필요요소)
  1) 에너지 : 전기 에너지
  2) 인프라 : 통신망
```
> <데이터 링크 계층><br>
```
1. 링크 -> 케이블로 생각 -> 잘 전달되게 하기 위한 여러가지 기술

2. Point to Point(장치와 장치 사이) 데이터 운송 방식 결정

3. 필요요소
  1) 장비간 식별 주소 -> MAC 주소(장치의 고유 주소)
  2) 오류 제어 -> Trail을 얹음으로 써 이를 확인함으로써 훼손여부를 확인할 수 있도록 함(무결성 Check)
  3) 흐름 제어 -> 약속되어 있는 속도(타이밍)을 맞춘다
    not) 오류수정

4. 장치
  1) L2 Switch : Lan과 Wan 모두 사용   cf) Lan Switch -> Lan에서만 사용
  2) Bridge    :  Switch 사용 이전에 사용
  3) NIC       : 네트워크 인터페이스카드
  4) WAP(Wireless Access Point) 

5. LAN
  1) 이더넷 : IEEE 802.3표준, CSMA/CD : 고속통신에 적합했던 통신방식 -> 충돌의 문제점으로 인해 Lan switch를 이용
  2) wifi : IEEE 802.11표준, CSMA.CA
  3) 토큰링 : 단방향 전송(토큰 패싱 방식 사용-> 네트워크 상의 충돌을 방지
  4) FDDI   : 더블링 방식 -> 양방향 전송 => 장애 허용성과 신뢰성을 높임, 광섬유를 사용하여 고속 데이터 전송이지만 이더넷보다 느림

6. WAN
  1) 프레임 릴레이 : 가상 회선이용, 데이터 패킷을 빠르고 효율적으로 전송
  2) ATM(Asynchronous Transfer Mode) : 고속 네트워크 프로토콜, 셀 단위로 데이터를 전송, 고정된 크기의 셸(53바이트) 사용 => 일정한 지연시간, 높은 품질의 서비스 제공
  3) PPP(Point to Point Protocol) : 두 네트워크 장비간의 직접연결
  4) HDLC(Hige-Level Data Link Control) : 동기식 데이터 링크 제어 프로토콜, 흐름제어와 오류 검출을 제공

* 발전 순서 : HDLC -> 이더넷 -> PPP -> 프레임릴레이, ATM

7. 이더넷의 발전
  1) 초기 CSMA/CD -> 반송파를 이용해 이용가능한지 여부 확인하고 데이터를 전송하는 방식
        -> 통신량이 증가시 일시적으로 통신이 안되거나 충돌이 발생하는 문제점이 발생
  2) Lan switch를 사용 -> 전용 포트를 사용함으로써 Collison 영역을 분할 -> 이더넷 스위치화, 고속화 지
  
```

> <네트워크 계층><br>
```
1. 데이터의 갈래길 중 잘 전달할(빠르고 안전한) 경로로 안내하기위한 최적의 경로 탐색
2. 장치 
  1) router
      cf) routing protocol : 경로를 안내해주기위한 통신 program
          - 과정 -    (1) 전체 네트워크 탐색
                      (2) 알고리즘을 이용해 필터링
                      (3) 최적의 경로를 산출
3. Protocol의 종류
  1) IP : 논리적(변경가능) 주소, V4와 V6가 있음, 기본 주소체계-> 인터넷 공간에서 특정위치를 찾기위해
  2) ICMP : 목적지로 데이터가 가기전에 통신가능 유무를 확인해 주는 것
  3) IGMP
  4) ARP : IP주소를 MAC주소로 FIND함 즉, 3계층주소로 2계층주소를 탐색
  5) Routing protocol : RIP, EIGRP, OSPF, ISIS, BGP, Static Routing 등등
```

> <전송계층><br>
```
1. 종단간(end to end)(송신자와 수신자사이) 의 신뢰성 보장-> 잘 전송됬는지 확인
                                                      -> 상위계층이 데이터의 전달의 유효성이나 효율성은 생각하지 않도록
2. 과정 
  1) Segmentate(데이터 나누기)
  2) 전달
  3) 재조립 ->if) 손실시 재전송을 요구

3. Protocol의 종류
  1) TCP : 신뢰, 연결지향(계속적 데이터 교환을 위한 통신루트(암호화&타이밍 설정)를 만듬, 작업수행시에만)
         : 대량의 정보전달, 양방향 -> Handshaking 연결시 - 3way / 연결해제시 - 4way
  2) UDP : 비연결, 단방향, 빠른 전송에 적합, 안정성 낮음
         : 실시간 여러다수 지점에 전송(멀티 캐스팅)
```
