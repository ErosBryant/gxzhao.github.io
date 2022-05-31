---
layout: page
title: network Layer- data plane
tags: [Network]

share-title : 컴퓨터 네트워크 
share-description : 네트워크 5계층 중 network Layer- data plane
---


# * network layer:
> host,router 마다 IP 프로토콜이 있다.
> router는 IP의 헤더 필드 검사
> best-effort service(최선형 서비스): 최선을 할뿐 보장은 못한다.
- sending:세그먼트를 캡슐화 하여 into datagram
- receiving: 위계층(transport layer)에 세그먼트를 전송 

### *  forwarding(전달) - Data plane: 패킥이 라우터의 **입력 링크**에 도달했을 때 라우터는 그 패킷을 적절한 **출력 링크**로 이동
- hard ware: 나노 세컨
- input   -> forwarding table로 인해서 -> 알맞는 output
### * routing(라우팅) - Control plane : 송신자가 수신자에게 패킷을 전송할 때 네트워크 계층은 **패킷 경로**를 결정
- soft ware: 밀리 세컨
- 라우팅 알고리즘:  경로 계산 알고리즘(모든 라우터에서 실행
- forwarding table을 만드 는것
> 네트워크 전반에 걸쳐 출발지에서 목적지까지 datagram의 종단간 경로를 결정


- Two control-plan approaches: 
  1. 전통적인 라우팅 알고리즘: 라우터에 존재
  2. software-defined networking(SDN): 서버에 존재
     - 데이터 평면과 제어 평면의 기능을 분리

### * forwarding table: 라우터는 도착하는 패킷 헤더의 필드 값을 조사하여 패킷을 forwarding

        control plan
        data plan 
        원격 컨트롤러(Remote Controller): forwarding table을 계산과 배분
- 1. 전통적:
  <img src="https://user-images.githubusercontent.com/86946575/165719460-5c7e07e9-09aa-48b0-b9a8-899388ba6a14.png" alt="image" style="zoom:50%;" />   

- 2.SDN:
  <img src="https://user-images.githubusercontent.com/86946575/165721753-1508b5ee-30db-4327-9bd5-9bd3ac2fcfb9.png" alt="image" style="zoom:50%;" />

- 목적지 기반 전달：범위
- 32bit 주소 
![image](https://user-images.githubusercontent.com/86946575/165877949-34fa0a5b-3ff8-4a11-9dc6-d4c924e2c7df.png)


- ####b #  logest orefix matching(최장 프리픽스 매칭  규칙 )
> 테이블에서 가장긴  대응 엔트리를 찾고, 여기에 연관덴 링크 인테페이스로 패킷을 보낸다.
![image](https://user-images.githubusercontent.com/86946575/165891837-6d5d52ab-8132-4a1e-b55e-fb85b86cefa3.png)



### * Network service model: 송수신 호스트 간 패킷 전송 특성을 정의
1. **보장된 전달** :패킷이 source 부터 dest까지 도착 보장
2. **지연 제한 이내 보장된 전달**: 패킷의 전달 보장뿐만 아니라 **특정 지연 제한** 안에 전달
3. **In order 패킷 전달**: 패킷이 송신된 순서로 도착 보장
4. **최소 대역폭 보장**: 패킷 간 간격 변화에 대한 제한 사항
5. **보안**:기밀성 제공

            ATM (AsynchronousTransfer Mode,비동기 전달 모드)
            ATM CBR(constantBit rate): 고정 비트율 서비스
            ATM ABR(available Bit rate): 가용비트율서비스, VBR(가변)

            Internet QoS Architecture: 차등화서비스(DiffServ)  & 통합서비스(IntServ)

---
#  라우터 내부
### * 1.  Input prot (입력 포트)
- 입력포트에서 검색 기능을 수행한다.forwarding table을 참조해 패킷이  스위칭 구조를 통해 전달되는 출력포트를 결정
- 포트: 물리적인  입출력 라우터 인터페이스

            1. 목적지 기반 전달 : 입구에서 최종목적지를 검색하고  최종으로 연결되는 교차로 출구를 결정한 후  운전자에게 알려준다

            2. 일반적인 전달 :　목적지 외에도 많은 요인들로 인해 자동차의 출구를 결정한다.

![image](https://user-images.githubusercontent.com/86946575/165732268-7855dcfe-40d4-4813-a6b8-fba54789e761.png)


▪ TCAM (내용 주소화 기억장치): 
- 32bitIP주소가메모리에제공되면
- 해당주소에대한포워딩테이블항목의내용을반환
- (0,1, x) 3가지입력가능


> Head-of-the-Line(HOL) blocking: datagram  queue 전달 할때   다른 queque 앞에 가지 못한다.

## * 2. switching fabric(스위칭 구조)
> 라우터의 입력 포트와 출력 포트를 연결한다. (라우터 내부에 있다)

- switching rate: input에서 output까지 전송 속도
![image](https://user-images.githubusercontent.com/86946575/165893715-6b4bc430-940a-4e28-84b6-47407de29864.png)

 #### ▪ Switching via memory
> 전통적인 I/O방식
- packet을 memory에 복사하고 다시 output으로 전송

- 단점:
    - 처리 속도 늦음
    - 2 bus 
![image](https://user-images.githubusercontent.com/86946575/165894195-ab71a32e-da25-454f-82a0-04dbc4cd2ec9.png)


- #### ▪ Switching via a bus
> memory 를 격는게 아니라 Bus를 통해서 바로 ouput 연결 （shared bus ： 백플레인 버스）
- 32Gbps bus：Cisco 5600

- 장점:
  - 속도는 빠르다
- 단점:
  - 동시에 전송 x ,한번에 하나
  - 속도 제약 x
![image](https://user-images.githubusercontent.com/86946575/165898331-7b16919f-c5af-4859-bc30-5737c24b962a.png)

- #### ▪ Switching via interconnection network(crossbar)
- 60Gbps bus: cisco 12000
- 장점: 동시에 전송 가능 
- 단점: 비용  많이 든다.


        banayan network: 다단계 스위칭 구조
         c
        level을 둬서 level을 통해 연결 하므로 적절한 비용
        switch는 적지만  crossbar 보다보니 충돌이 발생하는 확율이 높다.

![image](https://user-images.githubusercontent.com/86946575/165898710-ba8d89db-4ecd-4325-b7a5-821271965b5a.png)

## * 3. Output
> 스위칭 구조에서 수신한 패킷을 저장,필요한 link layer 및 physical layer 기능을 수행하여 패킷을 전송
- buffering, scheduling
![image](https://user-images.githubusercontent.com/86946575/165900857-322e5c98-6cc0-4a07-8021-7f2d1571f832.png)
- 라우팅 프로세스
  - 전통적 : 라우팅 프로토콜을 실행하고 라우팅 테이블 과 연결된 링크 상태 정보를 유지 관리하며 forwarding table을 계산
  - SDN : 라우팅 프로세스는  원격 컨트롤러와 통신해서 원격컨트롤러에서 계산된 forwarding table을 수신하고 입력 포트에 이런 항목 설치

![image](https://user-images.githubusercontent.com/86946575/165731730-9b5d0645-2da8-4f1a-876c-0b07b4348a58.png)

- AQM (ActiveQueue Mgmt)은 라우터에서 사용되고 있으며 큐의 순간적 길이나 평균 길이를 관찰하여 혼잡을 예측하고 탐지하는 알고리즘 사용,라우터 큐에서 혼잡 상태정보를 단말 호스트로 전달하여, 혼잡 회피 제어
- AQM 알고리즘의 하나인 RED (random early detection)는 패킷을 무작위로 폐기 시켜 혼잡제어를하는 큐기반의 혼잡 제어 기술


### Scheduling mechanisms
> scheduling : 다음  어느 패킷이 링크로 전송되는지 선택

#### 1. FIFO 
- 오는 순서에 따라 전송

- discard policy: if packet arrives to full queue: who to discard?
  - tail drop: drop arriving packet
  - priority: drop/remove on priority basis
  - random: drop/remove randomly

![image](https://user-images.githubusercontent.com/86946575/165901478-08a19c9d-07fe-4e48-8df4-e1663e4373de.png)

#### 2. priorty: 우선 정책
- 두개의 queue을 **high prioiry** 와 **low priority**로 나눠서  High만 먼저 전부  내보내고 low를 내보냄

![image](https://user-images.githubusercontent.com/86946575/165901795-409f2da5-f0ee-4fce-9a47-9c4ab3ebc1fd.png)

#### 3.RR(라운드 로빈)
- 두개의 queue지만 각각 하나하나씩 내보냄
![image](https://user-images.githubusercontent.com/86946575/165901965-e3e365c6-2410-401e-8e97-3d03a99e878d.png) -   ![image](https://user-images.githubusercontent.com/86946575/165901942-ff703564-db84-43ec-b442-82ec2eb0ed38.png)

#### 4. WFQ (가중치 부여 RR)
- queue를 3개로 나누어 
  - 10번중 5번 3번 2번 이렇게 가중치를 줘서 전송
![image](https://user-images.githubusercontent.com/86946575/165902139-e308b304-d38a-435a-bff3-ca222f0ea942.png)


---
# IP:Internet protocol
- network layer에 있는 3 가지
  1. routing protocols
      - forwarding table. path selection
  2. IP  protocol
      - addressing 모임
      - datagram format 
  3. ICMP protocol
      - error reporting
      - router signaling
  
### IP datagram format
- ver(버전): IPv4 or IPv6
- head.len (헤더 길이) : byte
- type of service ***[TOF]***: data 서비스 :리얼타임, 스케쥴링 
- length:data 길이
- 16-bit identifier, figs, fragment offset : 단편화에 사용 (MTU:1500byte)
- time to live: router를 지나갈수 있는 횟수 
- upper layer(윗층) : TCP,UDP
- Header checksum:TTL 필드와 옵션 필드의 값이 변경됨, 각 라우터에서 재계산 
![image](https://user-images.githubusercontent.com/86946575/168037384-d13697d6-4c5d-45d0-9ae6-38ddd7ad0ddb.png)

### fragmentation(단편화)
- largest possible **link-level frame**
- 마지막 목적지에서 볶합
<img src="https://user-images.githubusercontent.com/86946575/168040232-473860f2-b8fc-47d4-8086-e4b51693106c.png" alt="image" style="zoom:50%;" />
- datagrame이 4000이면 최대 1500(1480)으로 단편화해서 전송
- offset은 8의 배수로 나눈다
- fragflag는 1 , 마지막에 0 setting
<img src="https://user-images.githubusercontent.com/86946575/168040858-bc872209-1289-45c0-a7c4-7a2aa80d7432.png" alt="image" style="zoom:50%;" />

----
### IPv4 
- IPv4 address: 32bit 
- 라우터는 여러개 interfaces 있다

- ###  subnet
-  subnet은 라우터 없이도 서로 통신가능 한것을 말한다.(local 영역 능력)
   -  직접,인터넷 스위치를 통해 
- IP 주소의 동일한 서브넷 부분을 가진 장치 인터페이스
> subnet part : 223.1.1.0/24 [앞 24bit 가  subnet ] 
> subnet mask:[/24] 24면 앞 24 
> host part  : 223.1.1.0/24 [ 마지막 8bit가 host 구분 ]
 
<img src="https://user-images.githubusercontent.com/86946575/168042129-4f4c262e-a2a8-4dee-8174-826b8093dba2.png" alt="image" style="zoom:50%;" />

### CIDR (classless Inter  Domain Routing)
> subnet mask을 /24로 제한 하는 것이 아니라 [/20,/21] 등 여러 bit로 설정 

![image](https://user-images.githubusercontent.com/86946575/168044939-9d7fde97-be75-4a65-800a-363be1feb686.png)
- #### IP broad cast 주소: 255.255.255.255 (port:고정 67) : 같은 subnet에 있는 모든 호스트에 한테 전송 


### DHCP: Dynamic Host Configuration Protocol
> DHCP서버는 주로 router안에 있다
> DHCP -> UDP 사용
> 서버에서 동적으로 주소를 획득하는 것
1. 반납
2. 재사용
3. 연결시에만 사용
-  6x8 =48bit (16진수)  ; IP subnet broadcast:255.255.255.255
<img src="https://user-images.githubusercontent.com/86946575/168048716-8c81b0de-2b47-4084-9a5c-3c1e0cfd260b.png" alt="image" style="zoom:50%;" />

- #### DHCP의 4가지 상태
1. DHCP discover: host의 broadcast에 메시지 전송
2. DHCP offer : 응답, 주소 알려줌
3. DHCP request: 알려준 address을 사용한다고 broadcast에 메시지 전송
4. DHCP  ACK:  확정

<img src="https://user-images.githubusercontent.com/86946575/168046763-13f07aa7-51fa-47c3-a6a3-8d0119ec5ffd.png" alt="image" style="zoom:45%;" />
<img src="https://user-images.githubusercontent.com/86946575/168046887-2e293bc4-e5e3-4530-9066-4d9f46ae0291.png" alt="image" style="zoom:45%;" />

- #### DHCP 추가 확보 해야하는 것 
1. address of first-hop router for client
2. DNS 서버의 이름이랑 주소 확보
3. network mask
  

        그냥 주소 할당: DHCP -> CIDR

        그룹별 라우터 관리:
        IP addres의 subnet부분을 획득 (주소 블럭):isp's  block, isp 주소 공간 
        주소 블럭: 각각 조직 들이 나눠져 있다
        조직: 200.23.16.0/23 ,200.23.17.0/23 ...200.23.30.0/23
        Fly-By-Night-ISP: 인터넷에게 200.23.16.0/23 으로 시작하는 주소를 보내기

#### NAT (주소 변환)
- NAT 상자는 헤더 IP 주소와 포트 번호를 다시 씁니다
> NAT translation table에서 WAN,LAN쪽으로 나눠서 관리
> WAN,LAN은 서로 대응한다.
- 로컬 네트워크는 외부 세계에 관한 한 단 하나의 IP 주소를 사용합니다: Port 번호
- 16bit prot-number field
![image](https://user-images.githubusercontent.com/86946575/168508176-2a2fe3c9-5f6e-45a6-aaec-c487efedb2b5.png)
- NAT 논란 사항
  1. router는 3계층만 사용해야 하는데 4계층의 port 번호 사용 
  2. 주소 부족은 IPv6으로 해결
  3. P2P applications에서 문제
  4. 처음부터 외부에서 들어면 어떡함?
     - 해결: 포트 포워딩 방법 사용(포트 연결)

---
# IPv6 
> IPv4의 주소 고갈 때문에 개발 
- 주소
  - 32bit -> 128bit 주소 공간
  - anycast address:애니캐스트 주소로 명시된 데이터그램은 호스트 그룹의 어떤 이에게도 전달될수 있다.
- 헤더(header)
  - 40byte
- 흐름 라벨링(flow lable) 
  - 20bit 필드로 동일한 '흐름'에서  데이터 그램을 식별합니다
- 버전
  - IPv4-6
- 트랙픽 클래스(pri)
  - 흐름의 데이터 그램 중 우선 순위
- payload len
  - 16bit:헤더의 뒤에 추가 된 바이트 길이: unsigned integer
- next header
  - 위 계층의 프로토콜(UDP,TCP)
- hop limit
  - 데이터 그램의 수명: 라우터 하나지날때마다 하나 감소\
- 단편화(fragmentation)
  - 출발지와 목적지에서만 실행: 중도에서 너무 크면 ICMP오류를 보내  송신자가 다시 보낸다
  - 시간이 너무 걸려서 ,라우터에서 이 기능을 삭제 하였다.
- head checksum
  - 처리시간을 줄이기 위해서 삭제
- option
  - 표준은 아니지만 필요하면 , next header에 중 하나가 된다.
![image](https://user-images.githubusercontent.com/86946575/168510273-71ec1653-7da7-41a8-8d4c-af14be65bb80.png)

#### IPv4 to IPv6
> 터널링: IPv4 라우터 간에 IPv4 데이터그램에서 페이로드로 운반되는 IPv6 데이터그램
- 캡슐화: IPv6를 IPv4의 payload에 저장해서 전달
![image](https://user-images.githubusercontent.com/86946575/168510386-1a21e36b-e3a3-416d-a514-fe120e9f243f.png)

----
# SDN
- 라우터+ 스위치=패킷 스위치
- 일반적으로 라우터 포워딩 결정은 패킷의 목적지 주소만 기반
- SDN은 네트워크 기능과 특정 링크 계층 기능을 제공하기위한 통합된 접근 방식


![image](https://user-images.githubusercontent.com/86946575/168518116-a88fc2ac-a66f-439d-b78c-115a587f5d4e.png)


####  openflow 
1. Router 
   - • match: longest destination IP prefix
   - • action: forward out a link
2. Switch
   - match: destination MAC address
   -  action: forward orflood
3. Firewall
   - match: IP addresses and TCP/UDP port numbers
   - action: permit or deny
4. NAT
   - match: ip address and port
   - action: rewrite address and port 

- flow table in a router ,define router's match+ action rules
![image](https://user-images.githubusercontent.com/86946575/168519630-df2a5849-dd45-461d-92af-99038c6438d7.png)
> • Pattern: match values in packet header field
> • Actions:[drop, forward, modify]삭제, 전달, 수정, 일치하는 패킷 또는 일치하는 패킷을 컨트롤러로 보냅니다.

        drop: 아무 동작이 없는 플로우 테이블 항목은 일치된 패킷을 삭제 해야 함을 나타남
        modify: 패킷이 선택된 출력 포트로 전달되기 전에 10개의 패킷 헤더 필드의 값을 다시 쓸수 있다.


#### match + actuib openflow 

![image](https://user-images.githubusercontent.com/86946575/168521259-83540f4c-d795-4f2f-8032-4ae06259bd54.png)

- 부하 균등화 (load balacing)
![image](https://user-images.githubusercontent.com/86946575/168521768-d0d71c52-9094-4076-9ac7-25f9ef6a4c5a.png)