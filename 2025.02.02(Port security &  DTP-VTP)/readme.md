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
























