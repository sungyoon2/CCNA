# NAT
---
NAT
---
> NAT란<br>
```
- Network Address Translation의 약자
- 주소변환(IPv4주소) 기술
- IPv4주소 절약을 위한 기술
- 보안 통신(내부의 IP주소를 사설IP 주소를 씀으로써)
- 사설 구간 연결시에는 라우팅이 필요없음
- 라우팅시 network주소에 사설구간 NW IP를 넘을 필요가 없음
```
> 원리<br>
- 사설 구간과 공인 구간으로 나누어 관리
- 외부로 나갈때나 들어올때 공인 IP로 변환이 필요한데 이를 실현하는 기술
> 사설 ip<br>
```
- A클래스 : 10.x.x.x
- B클래스 : 172.16~31.x.x
- C클래스 : 192.168.x.x
```
> 종류<br>
- 사설 IP Group과 공인 IP Group의 관계
```
- Dynamic NAT(DNAT) : Host용 NAT, 공인 주소로 나갈수는 있으나, 상대방은 공인 ip 주소로 들오지 못함.       N      :      M        -> 공인 IP개수만큼 통신
- Static NAT(SNAT) : Server용 NAT, 공인 주소로 나가고 들어옴                                            1      :      1        -> 공인 IP와 사설 IP가 1대1 대응
- Port address Translation(PNAT) : 동시 통신가능한 NAT                                                 N      :    1 or M
```
> 사용법<br>
- 라우터에서 동작
- 전역설정모드에서 
---
- DNAT
```
access-list [ID] permit [사설대역 네트워크 ip 주소] [범위 wildcard mask]  -> 사설대역 지정(access-list)
ip nat pool dnat [공인ip 시작주소] [공인ip 끝주소] netmask [subnet mask]  -> 공인 ip 지정
** 만약 공인ip주소가 하나면 시작주소 = 끝 주소 **
ip nat inside source list [access-list id] pool dnat                    -> 공인 ip와 사설 ip Mapping
int f?/? (내부 포트)                                                     -> interface적용과정
ip nat inside
int s?/? (외부 포트)
ip nat outside
```
- PNAT
```
access-list [ID] permit [사설대역 네트워크 ip 주소] [범위 wildcard mask]  -> 사설대역 지정(access-list)
ip nat pool pnat [공인ip 시작주소] [공인ip 끝주소] netmask [subnet mask]  -> 공인 ip 지정
** 만약 공인ip주소가 하나면 시작주소 = 끝 주소 **
ip nat inside source list [access-list id] pool pnat overload            -> 공인 ip와 사설 ip Mapping
int f?/? (내부 포트)                                                     -> interface적용과정
ip nat inside
int s?/? (외부 포트)
ip nat outside
```
- SNAT(1대1대응)
```
ip nat inside soure static [사설구간 ip] [공인 ip]        -> 사설 구역 지정필요없이 바로 매핑
int f?/? (내부 포트)                                                     -> interface적용과정
ip nat inside
int s?/? (외부 포트)
ip nat outside
```
---
> 용어 정리<br>
```
- inside global : 외부네트워크에서 노출되는 내부 호스트의 공인 ip주소
- inside local : 내부 네트워크에서 사용되는 사설 ip주소
- outside local : 내부 네트워크에서 본 외부 호스트의 ip(보통목적지 공인ip)
- outside global : 실제 외부 호스트의 공인 ip주소
```
> NAT와 ACL함께 설정시 순서<br>
```
1. WAN구간 IP + Routing
2. Lan구간 IP
3. NAT 설정
4. ACL 설정
```

















