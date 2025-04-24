# Port Security & DTP-VTP
---
Port Security
---
> Port Security란<br>
- switch에서 작동하며, Mac flooding attack으로 인해 Mac table이 꽉차서 switch가 재기능을 하지 못하도록하는 것을 방지하기 위한 기술
- switch가 재기능 못하면 hub처럼 flooding기능만 하기에 부하가 높아짐
> 작동원리<br>
- Port별로 학습할 MAC주소 제한하며, 위반시 작동할 정책(action)을 설정함
> 적용<br>
- 전역설정모드에서 설정
```
clear mac-address-table or port에서 shoutdown / no shutdown             --> Mac table내의 저장되었던 주소를 지우는 과정
int f?/?                                                                --> Port security를 지정할 포트로 이동
switchport mode access  -> sw mo ac                                     --> Port security는 access mode에서만 작동가능하기에
switchport port-security -> sw po
switchport port-security maxium [저장가능한 주소수 설정] -> sw po max []  --> mac table내에 저장할 수 있는 주소의 수를 설정
switchport port-security violation [정책] -> sw po vio []                --> 최대 허용가능한 개수를 위반시 적용되는 정책을 설정
**  check : do show port-security
```
> 정책의 종류<br>
```
- shoutdown : 보안 침해시 해당 포트를 shoutdown
- protect   : 보안 침해시 해당 장비의 접속만 차단, 침해 count는 증가시키지 않음=> 보안로그를 남기지않음
- restrict  : 보안 침해시 해당 장비의 접속만 차단, 침해 count가 증가함 => 보안로그로 확인가능
```
> Mac table의 학습방법<br>
```
1. Dynamic : Default,  우선순위에 따라 저장, 주소저장 불가(running-config에 저장x) -> Reload시 정보가 날아감
2. Static : 고정 주소선택, 주소저장 가능(runnig-config에 저장O)-> copt r s로
            사용법 -> 정책설정전에 (violarion 전에)
                      switchport port-security mac-address [저장할 Mac주소]
                      ** 저장된 거 지우는 방법 : erease startup-config 
3. Stickt : dynamic(우선순위에 따라 학습) + static(running config에 저장)
            사용법 -> 정책설정전에 (violarion 전에)
                      switchport port-security mac aticky
                      정책까지 설정후에 안 지워지도록 항상 copy r s로 저장
```
---
DTP-VTP
---
> DTP란<br>
- Dynamic Trunk Protocol
- 자동으로 Trunk mode로 바꿔주는 protocol
- => 한쪽이 Trunk mode면 자동으로 반대쪽도 Trunk mode로 바꿔주는 역할
> DTP의 종류<br>
```
cf) access mode : 현재 포트를 Access mode로 지정,             DTP 송신 x, 수신 X
- ON            : 현재포트를 강제 Trunk mode로 지정,          DTP 송신 O, 수신 X
- AUTO (DA)     : 현재포트모드는 미지점(상대포트에 따라 결정),  DTP 송신 O, 수신 O (단, 상대포트에서 DTP를 수신받은 상태에서 동작, DTP를 먼저 전송하진 않음)
- Desirable(DD) : 현재포트모드는 미지점(상대포트에 따라 결정),  DTP 송신 O, 수신 O (DD mode임을 DTP통신하므로, DA와 만날시 협상을 통해 Trunk mode로 변환해 줌)
```
> DTP모드별 관계도<br>
```
Access - Access : Access mode
Trunk - Trunk   : Trunk mode
Access - Trunk  : 통신X(해당 세그먼트에서)
Access - DD     : Access mode
Access - DA     : Access mode
Trunk - DD      : Trunk mode(협상)
Trunk - DA      : Trunk mode(협상)
DD - DD         : Trunk mode(협상)
DD - DA         : Trunk mode(협상)
DA - DA         : Access mode -> DA는 DTP를 먼저 주는 경우가 없기에 기본 모드인 Access mode가 됨
```
> DTP Mode의 사용법<br>
```
<mode의 변환>
switchport mode dynamic [mode]
<DTP이용 안함>
'선행' -> Switchport mo trunk -> DTP는 Trunk mode에서 작동하므로
switchport mode no negotiation
```
---
VTP
---
> VTP란<br>
```
- VLAN Trunk Mode의 약자
- VLAN ID 전달 Protocol
- 복수의 Vlan ID를 VTP사용하는 모든 Software에 생성해주는 Protocol
```
> 필요성<br>
```
- 중간 스위치에 VLAN 정보다 없을 시 통신이 불가능
- 모든 스위치엣 빠짐없이 VLAN정보가 필요하기에
```
> 조건<br>
- VTP 도메인의 일치
- 기본 Trunk 모드
> 동작 방식<br>
```
1. Server에서 Vlan을 추가, 수정, 삭제
2. Server의 VTP 설정번호의 번호가 상승(+1)시켜 다른 스위치에게 변경된 VLAN 정보와 함께 전송
3. 수신한 VTP 프레임의 VTP 설정 번호가 더 높으면 자신의 VLAN정보를 새로운 정보로 대체
4.수신한 VTP 프레임의 VTP 설정 번호가 동일하면 수신한 프레임을 무시
5. 수신한 VTP 프레임의 VTP 설정 번호가 자신의 번호보다 낮으면 자신의 VTP 정보를 전송
```
> Mode의 종류<br>
```
- Server : VLAN 생성, 수정, 삭제 가능                        / VLAN 전파
- Client : VLAN 생성, 수정, 삭제 불가 only 내용을 받아서 적용 / VLAN 적용
- Transparent : VLAN 생성, 수정, 삭제 가능                   / VLAN 적용X(서버로부터 받은 정보)
                                                            / 다른 Client로 받은 정보전달만(VLAN정보 전파)
```
> 설정 순서<br>
```
1. Server mode에서 도메인명과 동기화를 위한 password지정
vtp mode sever
vtp domain [name]
vtp password [password]
2. 각 mode의 VTP들어가서 password로 동기화
vtp mode [client or Transparent)
vtp domain [name]
vtp password [password] -> 서버와 Vlan정보 동기화
```
























