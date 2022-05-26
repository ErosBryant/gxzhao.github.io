---
layout: page
title: Transport Layer
tags: [Network]

share-title : 컴퓨터 네트워크 
share-description : 네트워크 5계층 중 Transport Layer
---



- 단편화(fragmentation): 링크가 허용하는 최대 데이터 크기 이하로 패킷을 작은 조각으로 나누는 과정 (참조: https://en.wikipedia.org/wiki/IP_fragmentation)

- 다중화(multiplexing): 다수의 응용에서 동시에 발생된 독립적인 세그먼트들을 하나의 전송로를 통해 전달하는 과정 (http://www.tcpipguide.com/free/t_TCPIPProcessesMultiplexingandClientServerApplicati-2.htm)

- 역다중화(demultiplexing): 수신된 다양한 세그먼트들을 전송 계층에서 분리하여 해당 응용으로 전달하는 과정

- 간이 망 관리 프로토콜(Simple Network Management Protocol, SNMP): 네트워크상에 존재하는 장치들의 정보 수집, 관리, 제어를 위해 사용되는 인터넷 표준 프로토콜

- 체크섬(checksum): 수신된 데이터의 정확성을 검사하기 위해 사용되는 전송 데이터의 합계

- ACK(acknowledge): 송신측이 전송한 데이터가 올바르게 도착했음을 알리는 수신측의 응답

- 파이프라이닝(pipelining): 하나의 처리과정이 끝나고 다음 처리과정이 수행되는 것이 아닌, 여러 단계를 병렬적으로 수행하여 처리속도를 높이는 기술

# transport layer
> 기본 기능 : 다중화,역 다중화 
> process 들 간의 논리적 연결 (집,호실(sorcket) 까지 구분,)
> segment: 전송계층에서 정보 단위
> - sender:메시지를 보낼때 segement 단위로 (분할)짜른다.
> - reciver:받으면 재조립한다.

        network layer：ip주소 : 인터넷 주소 ：
                      host간의  논리적 연결(집 구분 )


### TCP 와 UDP 차이점
 ||TCP(안정적인 서비스)|UDP|
 |:--|:--:|:--:|
|1|Reliable|UnReliable|
|2|응용입장:순서대로(in-order)| Un Order|
|3|connection setup| connetion less (즉 안함)|
|4|flow control| faster than TCP |
|5|congestion control||

### multiplexing (다중화) 보내는 쪽
>  여러 서비스들을 동시에 하나의 링크로 내보내는 다중화 라고 합니다.

![image](https://user-images.githubusercontent.com/86946575/161181178-ff763697-8fe2-4ac9-8551-327ac7a9b9d1.png)

### demultiplexing (역다중화) 받는 쪽
> 공유 된 채널을 통해 날아온 데이터들을 각각에 맞는 프로세스한테 전달 해 주는 것, 이것이 reciever의 역할
- ####  port number  (socket)
>   받을때 prot number로 식별해서 맞는 process에게 전달


        다중화,역 다중화 둘다 같은 network,link,physical을 겨쳐간다.

- 제일 위 한줄이 prot number 
  - source prot: 시작 주소
  - dest port: 목적지 주소

        
![image](https://user-images.githubusercontent.com/86946575/161181970-bde35b26-a27b-4c67-9d09-34a3cddc45bc.png)

            (TCP)dest port 번호는 갔다. 하지만 source port 번호는 다르다.
            그러므로 다른 socket  [port 들은  socket의 head 부분에있지만 하나라도 다르면 다른것]
                
                    80번 web server 
![image](https://user-images.githubusercontent.com/86946575/161213177-98c18764-4b8c-4e58-ad5a-c0393a8d7f1f.png)

---
# UDP
> 부가적인 기능이 제공 되고 있지 않고 전송 계층 프로토콜의 가장 기본인 **multiflexing, demultiflexing 기능만** 거의 제공하고 있다
> UDP 어떤 서비스를 제공하기 위해서는 정보를 위 Header에 저장해서 보낸다

- UDP는 전송실패가 발생하더라도 재전송을 하지 않는다.
- 각각의 UDP segment들은 독립적으로 취급이 된다
- 비 신뢰성: 순서 보장, 에러보장 x
    > 에러가 있더라고 속도까 빨라야하는데 사용(미디어 스트리밍app )
    > DNS
    > SNM


            이 한단위를 "word"라고도 한다
            호스트 내에 있는 프로세스를 구별 하기 위한 기본적인 정보가 **포트 번호**, 포트 번호가 맨 상단
            보낸 프로세스의 포트 번호 16 bit, 받는 프로세스의 포트 번호 16 bit
            length: UDP Segment byte(즉 길이)가 담겨 있다.[그래야 다음 segment 가 언제 시작하는 지 안다]
            
            장점 4개 중 point는 : simple
![image](https://user-images.githubusercontent.com/86946575/161218973-480c965b-5aaa-4bf4-9287-057f14c2b0e1.png)

###  UDP 필드 중 checksum
> checksum은 전체 UDP segment에 에러가 포함되어 있는지, 있지 않은 지를 판단
> Checksum은16 bit의 integer 값
> - sender: segment 정보 기반으로 이값을 만들어서 보낸다.
> - receiver는 역시 자기가 수신 한 전체 segment를 기반으로  다시 checksum을 재 생성 해 봅니다.만약 둘 사이에 checksum 정보가 일치한다고 하면 전송 중에 이 segment에 문제가 발생하지 않았다는 것이죠

            첫줄:sorce port 두번쨰 줄: dest prot ：여러줄 모두다 더하기
            앞 부분에 1 일 생긴걸 **carry** 라 한다:carry 발생하면 1을 마지막에 더하기
            
            이렇게 모든 정보를 다 sum을해서 나온 값을  반대로 한다 (1의 보수 )
            
![image](https://user-images.githubusercontent.com/86946575/161223323-e85ff0ad-7b8c-42aa-9d09-2f515c1ec111.png)

---
# TCP 기능 RDT
## principles of reliable data Transfer (신뢰적 데이터 전송)
> (ip)하위 레벨에서는 에러가 발생 할수 있는데 이것을 transport layer에 있는 어떤 서비스가 하위에 있는 에러가 보이지 않도록 응용 프로그램에 보이지 않도록 감춰주는 것을 신뢰성 있는 전송의 역할



                만약에 어떤 회사의 회장님, 사장님이 계신다면 그 밑에 비서가 있고 비서가 밑에서 올라오는 모든 서비스들,
                모든 결재 서류나 그런 것들을 모두 그대로 회장님, 사장님에게 갖다 드리는 것이 아니라
                자기가 먼저 체크를 해서 문제가 있는지 없는지를 검사를 해서 없는 것만, 문제가 있으면 다시 해 오게 하고
                없는 경우만 위에 전달을 하게 되면 사장님은 비서가 제공하는 서비스를 신뢰 할 수 있는 것이잖아요.

![image](https://user-images.githubusercontent.com/86946575/161226079-54fbd8bb-4201-4615-9fda-e5408c4b61ea.png)
          
           sending process는 자기가 데이터를 보낼 때 신뢰성 있는 전송 형태로 보내는 것으로 생각 할 수 있다는 것입니다.
            그렇지만 실제로 아래 레벨은 unreliable 하다는 것이죠. 실제 에러가 발생 할 수 있다는 것이고.
            그래서 트랜스포트 레이어에 있는 신뢰성 있는 전송 프로토콜이 실제로 unreliable 하지만
            이 사이에 데이터를 주고 받아서 신뢰성 있는 전송을 하게 만든다는 것입니다.
            그리고 결국 receiver 쪽의 트랜스포트 레이어 프로세스는 자기가 먼저 체크를 해서 그것이 에러가 없도록
            다시 이 둘 사이에 재전송을 한다던지 에러를 복구한다던지 해서 에러를 다 해결 한 뒤에 그것을 receiver한테 전달하게 되면
            receiver는 여기 이 데이터는 믿을 만 하구나 이렇게 생각 할 수 있다는 것이죠.

            rdt_rcv() 함수가 packet 체크해서 문제가 있으면  ack,nak 와 packet 마다 sequnce number로 표기, 오류 수정
![image](https://user-images.githubusercontent.com/86946575/161227154-f95dfbce-95c2-4ecb-b90b-590464ab4261.png)

- 문제 가 없으면  event 가 발생하면 action  전송
![image](https://user-images.githubusercontent.com/86946575/161233278-2fe48060-8b9b-4814-bc32-418a5977e787.png)


### rdt2.0: 발생 가능한 문제 1  Biterror만 해결
> error detection
> feedback
> sequnce number 
> header fild에 sequnce number(0,1 두개의 숫자만 필요) ,feedback, checksum 
- header fild는 overhead
            
            checksum으로 에러 탐색
            문제 없으면 :ACK
            문제 있으면 :NAK 

            ACK,NAK는 Bit error에만 사용

            메시지를 여러 segment로 나눠서 보낸다. sequnce number 는 이 segment 의 순서 보장  및 데이터 로스로 인한 재전송  중복성 
            TCP는 ->  2,1,3,5,4 를  1,2,3,4,5 로 재정렬

![4cba0d8adee30c80f25d2b6151fd357](https://user-images.githubusercontent.com/86946575/161503306-9a76e8be-4e33-4f04-8dc6-f2883e04d568.jpg)

        
#### ret2.1:sequnce number
- receiver가 받은 메시지가 새로 받은 메세지 인지 ,중복된 메시지 인지 구별
- **SEQUNCE number을 매 pkt에 추가 한다.**(0,1 두개의 숫자만 필요)
- receiver는 무조거 feedback을 보낸다.


![image](https://user-images.githubusercontent.com/86946575/161515640-99064400-01b9-489e-8136-3429a6c64aa5.png)

#### rdt2.2: NAK-free 프로토콜 
- NAK： ACK를 사용안하고 NAK에 0,1를 추가하여 선별 (왜냐, 어차피 DATA 왔다 갔다 해야하기 때문에)
- 0을 받아야하는데 안오면 계속 재전송
        
![image](https://user-images.githubusercontent.com/86946575/161232403-45ce372d-09d8-48ff-9b64-5c6dc7608671.png)

### rdt 3.0:발생 가능한 문제 2  Bit error, loss 해결
- loss 가 발생 했을 때 : timer 을 설정 
- 정해진 시간이 지나면 재전송 

##### stpo-and-wait 프로토콜
![image](https://user-images.githubusercontent.com/86946575/161235021-b24085f8-1a52-4444-b4f0-5d363cf04d33.png)


## pipelined protocol
> pkt을 여러개 multiple 하게 보낸다.

- window: 한번에 보낼수 있는 data size: 
* **window 크기는 항상 seq num의 절반보다 작거나 같아야한다.**

#### pipelined 패킷 재전송 방법
**ARQ**:패킷 재전송 방법
![image](https://user-images.githubusercontent.com/86946575/161234604-b65b0ad7-64ef-4055-8d97-60d41018eee9.png)

### Go-back-n (GBN)
> cumlative ack (누적)
> timer은 가장 먼저 보낸 pkt에 하나만 있고 안오면 다시, 오면 next one
> 송신만 buffer에 저장
- 0,1,2,() ,4,5  :3번이 시간 지나도 안오면 3번부터 다시 보냄

![image](https://user-images.githubuserc ontent.com/86946575/162381757-4feba743-524a-44e7-9f68-c1a141d6f18d.png)

### selective repeat(SR)
> individual ack (각각)
> timer는 pkt마다 다 각각 있다.
> 송수신 모두 buffer에 저장 
- 0,1,2,() ,4,5  :3번이 시간 지나도 안오면 3만 다시 보내고, 뒤애 4,5는 버퍼에 저장 .후에 3번오면 같이 app layer에 올려줌 

![image](https://user-images.githubusercontent.com/86946575/162383482-2de219b4-f258-47e1-8f50-28d9669987ac.png)
![image](https://user-images.githubusercontent.com/86946575/162381806-fa86c381-a3ea-4450-bf6f-9feee104afe8.png)

---
# TCP
> 1024~ 6만 까지
- MSS:1460 byte
- MTU:MSS+TCP head +IP head=1460+20+20 =1500 Byte
- TCP segment= TCP HEAD+ Cilent DATA
#### tcp 특징
1. point-to-point: 1대1,one sender,one server
2. Connection-oriented: 연결지향 :3 way handshaking
3. reliable,in-order byte stream: 순서적
4. full duplex data:양방향 data 전송
5. Congestion,flow controlled: reciver에게 맞춰서 전송 (X 트랙 )
6. pipelined

#### tcp segment
![image](https://user-images.githubusercontent.com/86946575/162386370-c9971583-e4cf-4211-b6a5-33de4341f1a2.png)
- head  len: head가 항상 20byte 아니기 때문에 필요,option이 붙을수 있다, 그래서. data 시작 위치를 알려준다.
- Urg data pointer : 긴급 명령 전달,사용 x
#####  sequence number 
> byte 단위이며, 앞부분의 first byte를 sequence number로 사용
#####  acknoledgement number
> ack 100 : ack 99까지만 잘 받았다.
> cumulative ack:누적 확인  응답
  - 4번째줄 A:는 바로 ACK 값        


#### 송수신  seq,ack
- client에서 server로 갈때 
- seq = ack+1  로 변함 
- seq +1 ，ack+1
![image](https://user-images.githubusercontent.com/86946575/163357938-bc030757-57be-4e21-b6b6-e3e271a1524d.png)


#### TCP RTT 시간 설정 
- 짧으면:  RTT 갔다오기 전에 재전송 문제
- 너무 길면:  loss에 대해 반응이 너무 늦어 진다

##### sample RTT
>  ACK 해서 돌아오는 시간 측정
> 이것의 평균으로  RTT 설정 

#### Estimated RTT
> 최근 값 중에  가중차
> Estimated RTT= (1-a) *Estimated RTT + a * sample RTT : 이전 값 + 새로운 값
> typical value : 0.125

#### Time out interval (간격)
>Time out interval (간격) = Estimated RTT + 4 * DevRTT(0.25)

---
# TCP rdt (신뢰적인 전송)
1. pipelined segfments
2. cumulate acks
3. SR Timer 

### tcp sender 전송 절차
1. data 를 app 에서 받다
   - seq # 을 생성
   - byte stream
   - start timer
2. timeout
   - 재전송
   - time 재시작
3. ack rcvd
   - ack 최신 번호로 update
   - start timer

##### 누적확인 중요: TCP는 n 세그먼트 만 재전송한다. 또한 TCP는 만약 timeout 전에 n+1 에 대한 누적 ACK가 도착한다면 재전송을 하지 않는다 ( 5가 안왔는데 6 이오면 5를 재전송 x)
#####  GNB 는 5가 안오면 Timeout되면 5부터 다시 보냄
- 재 전송 과 누적 확인 
![image](https://user-images.githubusercontent.com/86946575/163370936-45eaa2ec-900f-4e4f-b01e-f548637192f6.png)

- timeout 주기 2 배로 설정 
![image](https://user-images.githubusercontent.com/86946575/163371129-3a603b68-8d12-40c3-9ed3-5edfa3cf8154.png)

### tcp 빠른 재전송
- 같은걸 ack번호를 3번 ACKs 를 받으면 ,timeout이 안됐더라고 재전송 

![image](https://user-images.githubusercontent.com/86946575/163371662-8d58b839-da8f-44f5-a78d-11f948766368.png)

### 선택적 확인 응답
![image](https://user-images.githubusercontent.com/86946575/163372779-6ba75767-6407-4bdf-8b28-165eaa5867c0.png)

---
### TCP flow control
> buffer overflow 예방
> 수신의 buffer 용량 고려 

### rwmd= 수신자의 buffer 여유 공간을 알려줌

- 수신자의 buffer : RcvBuffer =4096 byte  
  

        rwnd = Rcvbuffer –(buffered data)
             = Rcvbuffer –(LastByteRcv – LastByteRead)


        송신자: (LastByteSent – LastByteAcked) ≤ rwnd :
        즉, (전송 확인응답이 안된 데이터 양) ≤ rwnd 보다 작은 량을 유지하며 전송
#### ☆수신자의 rwnd=0으로 full 경우( deadloack 상태); 1byte 데이터 probe 세그먼트 전송
![image](https://user-images.githubusercontent.com/86946575/163374879-7e18904e-b082-46cc-ad2c-c88ee8328ca0.png)

--- 
### TCP 3-way handsahke: 상태를 가진 통신 방식 
> 통신하기 전에 미리 사전 연결 확인
- SYNbit
-   - ACKbit
![image](https://user-images.githubusercontent.com/86946575/163376037-fa402aa4-4e09-4332-880e-867158322191.png)

### TCP 연결 끊는 것
> 통신 끊는 것도 비트 보냄
- FINbit
  - ACKbit
![image](https://user-images.githubusercontent.com/86946575/163376648-12c0b90d-15a2-45d1-9d56-3efe201676ff.png)

----

### TCP 혼잡 제어 
> ACK , loss 문제가 자주 발생 때문 
> 이것도 송신자가 제어 
> cwnd is dynamic
- 통신이 문제가 없으면 :  ssthresh(slow start의 임계값) 까지는 cwnd * 2: 넘으면 cwnd +1 (mss 사이즈만큼 )
- 문제가 발생하면  : cwnd=1 하고 ssthresh / 2
-  cwnd : window size 

#### TCP Slow Start
> loss, 3ack가 발생하면 바로 cwnd를 1로 낮춤
>  요약: 초기 속도는 느리지만 기하급수적으로 빠르게 증가합니다.

![image](https://user-images.githubusercontent.com/86946575/163501434-c1163d5b-ba66-41ea-9f7d-7e4bae3fb518.png)


#### 혼잡 회피
.
.
.

#### 빠른 회복
> 3개의 중복 ack 발생시 전환
.
.
.

#### ECN
- 명시적 혼잡 제어 표시
- IP 계층 router가 packet에서 TCP에게 알려준다.
- 송신 tcp는 혼잡 윈도우를 반으로줄이고, 전송되는 세그먼트 헤드의 cwr비트를 세팅 
