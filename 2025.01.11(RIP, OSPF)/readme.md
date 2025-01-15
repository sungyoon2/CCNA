# RIP 요약과 OSPF 정리
RIP
---
> 정리<br>
```
- Metric : Hop(rip는 최대 15)
*Metric -> routing protocal의 기준값
- Multicast : 224.0.0.0
- AD(Adminstrative Distance) : 120 cf) Static : 1. OSPF : 110, EIGRP : 90
* AD -> routing protocol의 우선순위
- 광고주기(전달주기) : 30초
- Version : V1 -> 클래스풀만 지원 => 첫마디만 보고 클래스 판단 즉 서브넷 마스크가 자동지점
          : V2 -> 클래스 리스도 지원
```
> RIP 연결 실습<br>
```
*주의 사항*
- 동적 라우팅을 설정할때는 멀리있는 NW상관할 필요 없음 => 직접 연결된 NW만 연결해주면 됨
- auto-summary : 클래스풀 기본지정 해줌  -> 되도록이면 no로 설정하여 클래스리스로 되도록
    = 단축키 : no auto
- 네트워크 변화에 따라 유동적 적용
*실습 코드*
원본                                  단축키
router rip
version 2            ------------->   ver 2                      
network <연결된 NW주소>  ---------->   net <->
network <연결된 NW주소>  ---------->   net <->
no auto-summary         ---------->   no auto
```
---
OSPF
---
> OSPF란 <br>
```
1. Link state기반으로 경로를 탐색하여 지정하는 protocol
2. Link state란, 케이블 상태정보를 가지고 최적 경로를 산출
3. 케이블 상태정보는 bandwidth(대역폭)을 이용하여 산출함
4. 이동하는 COST를 모두 합한 값을 통해 최적의 경로를 산출
* 대역폭을 이용해 link state인 cost를 산출하는 식*
cost = 10^8 / bandwidth

< 케이블별 cost값 >
- S's cost : 64
- E's cost : 10
- FE's cost : 1
- GE'S cost : 10^8/10^9 = 0.1(=>1)

< 케이블 할당 대역폭 알아보는 법>
1. 관리자모드
2. show int <케이블> 입력
3. BW의 값이 대역폭 임
추가 BW : 한번에 보낼 수 있는 양
     MTU : 한번에 전송 제한
```
> OSFP의 처리과정<br>
전체 경로 학습 -> 최적의 경로 산출'
```
< 전체 경로 학습 >
1. cost정보를 취합하는 router(DR)을 지정
2. DR로 모든 ROUTER에서 cost정보를 전송
3. DR에서 취합된 cost정보를 각 router에 전송
====> DR은 고성능의 장치이여야합

< 경로추가에 따른 과정 >
- active 상태 : 경로 정보를 update하는 상태(DR로부터 COST정보를 받음으로써)
- passive 상태 : 경로 정보의 update가 끝난 상태로 더 이상 정보를 주고 받지 않음, 안정적인 상태
===> 수렵시간이 단축(네트워크 변화시 해당 내용만 나머지 라우터에 전달하기에) -> 대규모 네트워크 환경에 적합
1. Hello packet 교환(ospf쓰는지 안쓰는지 여부를 확인)
2. router간 이웃 형성(= neighbor)
3. DR선정 문제시 BDR(백업DR)을 생성하며 나머지 DR OTHER로 지정
4. DR과 나머지 ROUTER간 빠른 이동 경로 지정
5. 나머지 ROUTER가 최적경로로 DR에 LSA패킷(COST정보) 전달
6. DR의 취합 후 나머지 ROUTER에게 LSA패킷을 전달
7. 각 ROUTER들을 OSPF DB에 저장(ROUTING TABLE INSTALL(적용))
* 224.0.0.5 : HELLO 패킷 전달경로, LSA 패킷 전달경로 => 목적지 주소
* 224.0.0.6 : 백업 DR -> DB에 오류 정보를 보내는 주소
```
> OSFP 정리<br>
```
- metric : cost(10^8/bandwidth)
- Multicast : 224.0.0.5(ALL), 224.0.0.6(오류 감시)
- AD : 110
- 광고시간 X : passive 상태일때 변경되는 정보 즉시 전달 => 변경될때만 정보교환
```
> OSFP 연결 실습<br>
```
router ospf <process id>   -> process id(PID)부여 필
router-id <1.1.1.1>        -> 각 라우터 별로 지정
net <연결된 네트워크 ID> <Wildcard> area 0  -> wildcard mask : 서브넷 마스크를 뒤집어 놓은 형태 ex) 255.255.255.0 -> 0.0.0.255
                                                            : 이용시 no auto summary가 따로 필요없음
```
> OSFP 확인시 속성<br>
```
- O : 같은 영역안에 소속
- IA : 내부에서
- N1, N2 : 네트워크에서 넘어옴
- E1, E2 : 외부에서 넘어옴
```













