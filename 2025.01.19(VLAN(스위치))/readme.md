# VLAN(스위치에서 사용하는 기술)
---
스위치
---
> 스위치와 허브의 공통점과 차이점<br>
- 스위치=> 2계층 장비, 허브=> 1계층 장비
```
- 공통점 : 집선장치(선들을 모아줌)
- 차이점(ARP request의 관점)
   -- HUB --
    HUB에는 어떠한 정보도 저장되지않기에 들어온 데이터를 지속적으로 연결된 모든 장치로 Flooding시킴
      -> 트래픽의 낭비가 발생 -> 속도 저하가 발생
   -- Switch --
   1. ARP request요청이 switch로 들어오면 일단 목적지의 mac주소가 없기에 broadcast로 전체장비에 flooding(이까지는 허브와 동일)
   2. 데이터가 들어왔던 출발지에 대한 mac주소와 방향이 Switch내의 Mac table에 저장이 됨(학습(laerning))
   3. 목적지에서 ARP response가 Switch로 들어오면(목적지는 응답은 유니캐스트로 전송) Mac table에 출발지에 대해 저장된 정보를 이용
   4. HUB처럼 연결된 모든 장비에 뿌리는게아닌, 해당 정보대로 출발지에만 응답이 전송됨(Forwarding)
*
- 학습된 데이터는 영구적이 X, 시간이 지남에 따라 소멸(Aging)
**
- Mac table의 기능을 못하게 하는 공격 => switch jamming공격
```
---
VLAN
---
> VLAN이란<br>
- 가상 통신망
- 물리적인 배성 구성에 제한을 받지 않음으로서, 네트워크내 구성 단망의 추가, 삭제, 변경이 쉬움
- 쉽게 말해 브로드캐스트 영역분할 기술
- 가변적인 영역이기에 VLAN ID로 묶기나름임.
> VLAN을 사용하는 이유<br>
- Broadcast가 미치는 영역을 제한하기위해
- => 각 영역(부서)별로 통신할 수 있도록 하기위해(동일 NW IP를 사용함에도 불구하고)
> VLAN의 포트 구분<br>
```
- ACCESS Mode : 단일 VLAN_ID 적용 -> Switch와 end device사이에 
- TRUNK Mode  : 여러 VLAN_ID 전달(송수신) ->  Switch간 or Switch와 router사이에
              : 한쪽 port에서만 하면 반대쪽은 자동으로 설정됨, VLAN_ID는 디폴트(1)로 됨.
```
> VLAN 적용 방법(switch에서 CLI)<br>
ACCESS Mode
```
en
conf t
vlan 10
ex
vlan 20                               ==> vlan_id를 10과 20을 생성
int <cable>
switchport mode access                ==> mode를 지정   
switchport access vlan <vlan_id>      ==> access mode적용하는 vlan id
===> 스위치에서 연결된 케이블별로 vlan을 적용하여 id별 영역을 나누는 메커니즘
요약 명령
sw mo ac
sw ac vlan~
```
TRUNK Mode
```
en
conf t
int <케이블>
switchport mode trunk
요약 명령 -> sw mo tr
```
** 해당 VLAM영역이 없더라도 경유하여 없는 영역으로 간다면 없는 VLAN영역도 등록해야됨
> Native VLAN<br>
- Vlan정보를 지원하지 않는 장치(ex. hub)에서 전달되는 데이터에 적용하는 ID
- Trunk mode에서 적용되기에 디폴드 id인 1을 가짐
```
Native vlan에게 vlan_id를 부여하는 방법
- switchport trunk native vlan <vlan_id>
```
> VLAN 태그<br>
- Vlan간 프레임 전달용(스위칭역할)
- MAC 헤더에서 어느 VLAN인가를 알려줌
```
- 표준 규격 : 802.1Q -> Native vlan지원
- 비표준(CISCO전용) : ISL -> Native vlan지원 x
```




































