# 2025.01.05(Routing Protocol(정적, 동적)

정적 Protocol
---
> Defualt<br>
```
1) 목적
  - Static으로 지정된 경로외에 나머지 모든 목적지를 지정 -> 단일 방향으로
    -> 가장 자리의 라우터에만 지정함
      if) 중간에 있는 Router에 Defualt 경로를 설정하면, 계속 도는 루핑현상이 발생할 수 있음

2) Defualt 경로 설정하는 방법
  - ip route 0.0.0.0 0.0.0.0 [경로]
              ------  ------
              나머지 모든 네트워크 주소 및 서브넷 마스크를 의미

3) 기타
  - Defualt가 설정되면 나머지 static 경로가 필요없는 경우 routing을 삭제함으로써 데이터 사용을 감소시킴
      -> 명령어 : no ip route ~
```
SERVER와 SERVICE
---
> Network<br>
```
- 여러 형태의 Service를 전달하는 역할

<구조>
Server(제공)     <-------------->    Client(요청)
                    무언가
                    -> Service : Client의 요구사항을 만족하는 Program
```
> - Server<br>
  - '-'가 설치된(동작하는) 컴퓨터
```
1) DNS Service/Server/System
   (Domain Name Service/Server/System)- 문자 형태의 주소
    - DNS ZONE내부에 DN과 IP를 매칭시켜 저장시켜놓음
    - DN으로 요청이 들어오면 매칭되는 IP주소를 제공함

2) Web Service : 웹 문서 파일을 제공
                 * 웹 문서(Hyper Text document) & hyper text : 문서와 문서간의 연결점을 만들어 놓고 빠른 이동을 지원
  (1) 구성요소
    - HTTP(Hyper Text Transfer)
    - HTLM(Hyper Text Markup Language) : 웹문서를 제작하는 언어
  (2) 과정
    - Web server로 HTTP 요청이 들어옴
    - Web service내부에 저장된 index.html의 내용을 client에게 보냄
```
동적 Routing Protocol
---
> 개요<br>
```
1) AS(Autonomus System): 자치시스템(관리자에 의해 관리되는 라우터의 집합/집단)
                         ISP(인터넷을 서비스하는 업체)의 통신망의 범위 ,0 ~ 65535의 고유 Number를 가짐
2) IGP(Interior Gateway Protocol) : AS의 내에서만 동작하는 Protocol -> 속도를 중시
3) EGP(Exterior Gateway Protocol) : 서로 다른 AS간(GW외부의)을 연결하는 것에 중점을 둠 -> 대단위의 네트워크망을 구성
                                    -> 속도보단 정책관련된 처리
```
> IGP - Distance Vector(RIP)<br>
```
1) 정의
  - 전체 네트워크를 학습한 이후 최적의 경로를 산출하여 제공

2) 전체 네트워크를 학습하는 방법
  - 특정 주기마다 직접 연결된 자신의 정보를 다른 라우터에게 전달(RIP는 30초마다)

3) 최적의 경로를 산출하는 방법
  - 거리가 짧은 것 -> 거리재는 단위 : hop(지나가는 router 한개당 하나씩 증가함)

4) 문제가 발생하는 상황
  - Looping이 발생 : 이미 삭제된 연결이나 주기적 정보전달로 인해 업데이트전에 삭제된 정보가 다른 라우터에서 들어와 없는
                     파일이지만 업데이트가 반복되는 것으로 hop수가 무한정으로 생성됨

5) 문제의 대책
  - Hop count를 제한 (RIP MAX Hop은 15) => 대규모 네트워크 연결엔 적합X  
  - route poisioning

6) 설정 명령
  [R1]
  router rip
  version ?
  network ?
  network ?
  no auto-summary

  [R2]
  router rip
  version ?
  network ?
  network ?
  no auto-summary
```




  

