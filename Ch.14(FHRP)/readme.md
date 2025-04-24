# FHRP
---
FHRP
---
> FHRP란<br>
- First Hop Redundancy Protocol
- 게이트웨이 역할을 하는 라우터나 L3 스위치를 복수개로 구성, 하나 다운되면 다른 하나가 그 역할을 계속 수행하게 하는 프로토콜
> 종류<br>
- HSRP
```
- CiscO 전용
- Active와 Standby router를 선출하여 하나의 GW처럼 동작
- Hello 주기 : 3초(Active 라우터는 3초 간 Hello 패킷 전송 : Keepalive Massege)
    └ Multicast : 224.0.0.2
- Dead 주기 : 10초(10초 동안 Hello 패킷을 못 받을 경우 Active로 전환)
- HSRPv2 가상 MAC : 0000.0c9f.f0xx(xx:GROUP 번호(16진수))
- HSRPv1 가상 MAC : 0000.0c07.acxx(xx:GROUP 번호(16진수))
```
- VRRP
```
- IEEE 표준 이중화 프로토콜
- Fail-Over : 대체 시스템이 존재.
- Hello 주기 : 1초
    └ Multicast : 224.0.0.18
- Dead 주기 : 4초
- 가상 MAC 주소 : 0000.5e00.01xx(xx : GROUP ID)
```
- GLBP
> 이중화 GW의 상태 변화<br>
```
- initial	: HSRP동작x, 설정변경,인터페이스 sh/no sh
- learn	: HSRP동작o, Hello(x) , VIP(x)
- listen	: Hello 수신(o) -> VIP(o), Active/Standby(x)
- speak	: Hello 송수신(o) -> VIP(o), Active/Standby(o)
- standby	: 동일 그룹내에 1개의 standby 존재
- Actice 	: 동일 그룹내에 1개의 active 존재
```
> 설정 명령어<br>
- HSRP
```
[MSW1] - Active Router (주 시스템)
int vlan [ID]
ip add [SVI] [subnet mask]
standby 1 ip [Vlan ip]
        └ 그룹 번호 : 0 - 255
standby 1 priority 105
             └ 기본 값 : 100              ==> priority값이 기본 값보다 크기에 active상태가 되는 라우터가 됨
standby 1 preempt
*****
standby 1 track fa0/2
            └ 포트 추적 priority 값에서 -10  ==> track option은 외부 감시중 문제 발생시 standby가 실행가능하도록 
*****
[MSW2] - Standby Router (대체 시스템)
int vlan [id]
ip add [SVI] [subnet mask]
standby 1 ip [Vlan ip]
```
- VRRP
```
[ESW1] - Master
vlan [id]
int vlan [id]
ip add [SVI] [subnet mask]
vrrp 1 ip [Vlan ip]
vrrp 1 priority 105
vrrp 1 preempt delay minimum 5
vrrp 1 track 2
exit
track 2 int fa1/3 line-protocol

[ESW2] - Backup
vlan [id]
int vlan [id]
ip add [SVI] [subnet mask]
vrrp 1 ip [Vlan ip]
```




















