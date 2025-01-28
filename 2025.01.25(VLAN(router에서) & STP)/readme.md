# VLAN(router에서) & STP
---
In router(VLAN)
---
> 의미<br>
- 하나의 케이블을 각각의 서브인터페이스로 나눠서 영역별로 가상의 케이블을 사용토록 함
---
> 사용법<br>
```
en
conf t
f?/?.[vlan ID]
encapsulation dot1q [valn ID]       --> en dot1q [] []
ip add [ip주소] [서브넷 마스크]
no shotdouwm
```
---
VLAN 총정리 명령어
---
> SWITCH<br>
- Global configuration
```
enable
congfig terminal
```
- Vlan id enabled
```
vlan [vlan id]
ex
```
- Access Mode
```
inter f?/?
switchport mode access
switchport access vlan [vlan id]
```
- Trunk Mode
```
inter f?/?
switchport mode trunk
```
---
> Router<br>
- Router on a site Command
```
enable
configure terminel
int f?/?
no shotdown
int f?/? [vlan id]
encapsulation dot1q [vlan id]
ip add [GW Host ip] [Subnet mask]
```
---
STP(Spanning Tree Protocol)
---
> STP란 <br>
- 스위치 루핑 방지 프로토콜
- 모든 점들이 연결되되 폐쇄적인 형태가 아니도록 만드는 프로토콜
- Switch에 연결된 port중 BPDU에 따라 하나를 잠재적으로 DOWN시킴으로써 퍠쇄적 형태를 퍠쇄적 형태가 아니도록 만듬
---
> STP의 진행과정<br>
```
1. Root Switch 선출
    - BID(Bridge id값) 비교  *BID = prioity + Mac (32768(defualt) + mac)
    - Vlan이 존재할 시 Vlan id를 더해줌
    - BID값이 낮으면 ROOT SW로 지점
2. Root Switch아닌 나머지 Switch에서 RP(Root Port)선정
    - 기준 : ROOT까지 경유되는 Path cost값 비교(fe: 19)
    - 즉, Root Switch에서 가장 가까운 port
    - 조건 : Switch당 1개
3. DP가 아닌 Port는 DP(Designed Port)로 지정
    - root sw의 port는 모두 DP
    - Segment당 1개이여야함
    => segment에 2개의 DP가 존재한다면 그 Port중 하나는 규칙에 의해 차단됨


** 차단되는 port가 지정되는 규칙 **
  1. Root sw BID값 비교
  2. Root Path cost 비교
  3. Sender BID 비교 -> 낮은 수가 Live, 높은 숫자는 died(차단)
    --> 보통은 여기에서 끝남
  4. port id BID 비교
```
> STP로 인해 차단되는 Port 변경법<br>
- Switch의 priority값을 적절하게 변경
```
Spanning-tree vlan 1 priority [값]
(vlan 1 : vlan지정이 없을시 defualt vlan id)
```
---
> STP의 실행과정별 상태<br>
```
상태                        BPDU의 송신     수신      MAC주소 학습       데이터 전송
Disabled: 포트사용불가 상태          X        X            X                  X
Blocked : STP BLK계산완료           X        O            X                  X
Listening : DP, RP초기 상태         O        O            X                  X
Learning : MAC주소 학습단계          O        O           중                  X
Forwarding                          O        O            O                  X
```
---
> STP의 발전<br>
- priority값 변경하면서 지정 -> 속도 오래 걸리기에 발전
```
STP : Disable -> Blocked -> Listening -15초-> Leaning -15초-> Forawading
 |
RSTP : Discharding -> Lenarning -> Fowarding : 3초 걸림
 |
PVST : Block관리를 따로 따로 함으로(Vlan이용) 흐름을 제어 -> load 부하를 막기 위해(=load balancing)
 |
MSTP
```





























