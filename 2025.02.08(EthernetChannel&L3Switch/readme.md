# EthernetnChannel & L3 Switch
---
EthernetChannel
---
> 개요<br>
- 케이블을 이중화하는 기술 -> GW와 GW사이를 묶어서 1개처럼 사용
> 기능<br>
```
1. 문제시 그 즉시 다른걸로 전환하는 failover 기능
2. 부하분산기능 load-balancing기능
3. STP기능은 자동으로 실행(루핑방지용)
```
> 조건<br>
```
- 포트 그룹핑 단위 : 2,4 포트 씩(짝수 포트)
- 같은 그룹내 포트들 : 연속적이어야 함
- 그룹핑 포트 특성 : 모두 엑세스 포트들이거나, 프렁크 포트들이어야 함 -> 하나로 통일되어야 함
- 포트 속도 및 방식 : 100 Mbps 이상, 동일 속도 및 동일 듀플랙싱 방식이여야 함, 점대점(PTP) 연결
```
> 자동설정의 종류<br>
- PAgP
```
- Port Aggregation Protocol
- CSICO 전용
- Desirable 모드와 Auto모드
- Desirable모드는 무조건 PAgP 동작
- Auto 모드는 상대 스위치 포트가 PAgp일 경우만 PAgP동작
- 최대 8개
```
- LACP
```
- Link Aggregatio Control Protocol
- IEEE 표준
- Active모드와 Passive 모드가 있음
- Active모드 : 무조건 LACP동작
- Passive모드 : 상대 스위치 포트가 LACP일 경우만 LACP 동작
- 최대 16개
```
> 설정 명령어<br>
- PAgP
```
int range f0/a-b
channel-protocol pagp
channel-group [Group ID] mode [desirable/auto]
exit

int port-channel [Group ID]
switchport mode trunk
```
- LACP
```
int range f0/a-b
channel-protocol lacp
channel-group [Group ID] mode [active/passive]
exit

int port-channel [Group ID]
switchport mode trunk
```
- 확인 명령어
```
show etherchannel 	                 -> etherchannel 설정 확인
show etherchannel summary	           -> etherchannel 설정 확인(간단히)
show etherchannel load-balance	     -> etherchannel 로드밸런싱 방식확인(기본 출발지 MAC주소별 부하분산)
```
---
L3 Switch
---
> 개요<br>
- 스위치 기능(Forwarding)과 라우터 기능(Routing)기능을 갖춘 장비
- 주로 VLAN간의 연결통신을 제공
> 기본 동작 방식<br>
```
- 서로 다른 VLAN ID간에는 라우터를 통해 패킷이 전송된다.
- But, L3 스위치는 수신된 패킷에서 L3에 해당하는 IP정보 부분을 살펴보고 있다가, 자신이 이미 알고있는(캐시되어있는) 주소면 굳이 라우터를 통하지 않고 직접 하드웨어적으로 곧바로 전송함.
- 즉, 패킷 플로우에서 첫번째 패킷에 대하여는 목적지 경로를 라우터로부터 파악하고, 이후엔 자신의 스우칭기능으로 고속 포워딩을 수행
```
> 비교<br>
- 라우터 VS L3 스위치
```
라우터                        L3 스위치
소프트웨어 기반의 라우팅        하드웨어 기반의 라우팅
CPU에 의함                     ASIC 기반의 고속의 라우팅 수행
```
- L2 스위치 VS L3 스위치
```
                  L2                        L3
참조테이블        MAC 테이블                라우팅 테이블
참조PDU          목적지 MAC주소             목적지 IP주소
스위칭프레임      이더넷 프레임              이더넷프레임, 프레임 릴레이 프레임, PPP 프레임등
L2헤더처리        그대로                    새로운 헤더로 변경 교체
```
> 사용명령어<br>
- 기본 매커니즘은 라우터에서 라우팅하듯이 수행
```
<Inter Vlan>
vlan [ID]
int vlan [id]
ip add [ip주소] [subnet mask]
no shotdown
ex
ip routing                    -> routing기능을 실행하도록하는 명령어
<외부로 나가는 router와 연결시>
int f?/?(라우터와 연결된 케이블)
no sw                          -> SW기능이 아님을 명시
ip add [ip주소] [subnet mask]
no sh
다음은 routing protocol설정하는 데로 시행
```























