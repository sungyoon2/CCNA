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






















