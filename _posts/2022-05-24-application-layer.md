---
layout: page
title: Application Layer
tags: [Network]

share-title : 컴퓨터 네트워크 
share-description : 네트워크 5계층 중 Application Layer
---



- 서버 클라이언트 모델(server/client model): 하나 또는 다수의 중앙 서버에 여러 사용자가 접속하여 서비스와 정보를 제공 받는 모델

- 피어투피어 모델(peer-to-peer model): 중앙 서버 없이 사용자들이 서로 정보를 공유하는 모델

- HTTP (HyperText Transfer Protocol): 웹 서버와 사용자 클라이언트(예. 웹 브라우저) 사이에서 웹 문서를 전송하기 위한 규정

- SMTP (Simple Mail Transfer Protocol): 인터넷에서 이메일을 보내기 위한 규정


- FTP (File Transfer Protocol): 인터넷에서 파일전송을 위한 규정


- DNS (Domain Name System): 호스트의 도메인 이름을 호스트의 주소로 또는 주소를 도메인 이름으로 변환하는 시스템

- 캐싱(caching): 빠른 접근을 위해서 데이터(또는 웹 문서)를 일시적으로 특별한 곳(프록시 서버)에 저장하는 기술

- SSL (Secure Socket Layer): 네스케이프가 만든 TCP/IP 네트웨크에서 암호화 통신을 위한 규정 (TLS 초기 모델)

- TLS (Transport Layer Security): SSL 3.0 이후 표준화되면서 바뀐 이름이며, TCP/IP 네트워크에서 암호화 통신을 위한 규정


> 네트워크 애플리케이션에 5계층이 있다.
> (network application != application layer)
1. application
2. transport
3. network
4. link
5. physical

# network application architecture 
1. ## P2P architecture
> 서버가 없이  client 들 끼리 직접 연결 (peer) 하여 통신하는 방식

- 단점: 관리가 힘들다,  보안 ,신뢰성에 취약
- 장점: 분산구조 특징.비용효율적
 
##### 자가 확장성(self-scalability)
> 새로운 피어가 새로운 서비스 용량과 새로운 서비스 수요를 가져옵니다.
> 많은 호스트가 참여할수록 서로 서로 data를 업로드가 많아 진다.

2. ## client - sever arcgutecture
##### sever 
- always-on host : 항상 커져 있는 호스트를 sever 
- client 한테서 요청을 받는다.
- 영구적으로 같은 IP address
- data centers for scaling(확장성)

> 하나의 sever 가 모든 client로부터  요청 응답을 다 못할수 있다. 그래서 **data center** 라는 가상 서버를 사용한다.

##### client
- 동적 IP address
----
# application layer
> HTTP ,[SSL(보안 TCP)],SMTP,FTP  
- ## process 간 통신 
> program running within a host
#### ***같은 호스트*** 안의 투 호스트는  **inter-process communication**을 사용한다

#### ***다른 호스트*** 에서 process들끼리 연락하면  Message를 보냄
- client process : messages 보내는 쪽
- server process : messages 받는 쪽

- ### Sockets (door로 비유) 
> process와 computer network 사이의 (API 함수)interface(application 와 transport 계층간의  interface ) 
> process sends/receives messages to/from its socket
> 통신시 상대 socket만 고려하고  밑에 계층들은 고려안 한다.

![image](https://user-images.githubusercontent.com/86946575/159232711-40f37d4b-2544-4d58-96f1-fc15c8102f9d.png)

            ▪ TCP socket identified
                        by 4-tuple:
                        • source IP address
                        • source port number
                        • dest IP address
                        • dest port number
<img width="" alt="image" src="https://user-images.githubusercontent.com/86946575/161185069-41c7164f-aa12-444c-be3d-9bbf2a93d317.png">



### 프로세스 주소배정
- messages를 받는 쪽이 **식별자**
- 식별자는 두가지 정보를  포함하고 있다
  - IP address: 32bit unique 주소 ( 건물 주소 ):호스트 지정 HTTP
  - port number: 소켓 번호 , well-known[포트 번호](방 번호) SMTP [process 선별 용]


        application layer protocol 정의:
        importent!!
        message type: request(요구), response(응답)
        message syntax: 어떤 필드에 메시지가 있는지, 어떻게 필드를 구분
        message semantice(의미): 필드 정보의 뜻
        Rule : 언제,어떻게 메시지를 보내고 받는지

        open protocols: 호환성 허용 (무료)
                       e.g., HTTP,SMTP 
        
        proprietary(폐쇠적) protocols: (유로 , 기술 공개x)
                       e.g., skype 

        


---
# transport layer(Network application이 이용가능한 transport service)
> TCP,UDP
- ## application이 요구하는 전송 서비스 4 분류
#### 1.  신뢰적 데이터 전송
- 무손실, 무손상 기반 data integrity 보장 
> 프로토콜이 보장된 데이터 전송 서비스를 제공한다면 이를 **신뢰적 데이터 전송**을 제공한다고 한다.
> 트랜스포트 프로토콜(즉 계층)이 서비스를 제공할 때, 송신 프로세스는 데이터를 소켓으로 보내고 그 데이터가 오류 없이 수신 프로세스에 도착할 것이라 확신을 갖는것

          손실 허용 애플리케이션(loss-tolerant application): 신뢰적 데이터 전송을 제공 안 할때 송신 프로세스가  보낸 데이터는 수신 프로세스에 전혀 도착하지 않을수 있다.이것을 말한다. (라디오)

#### 2. throughput (처리량)


          대역폭 민감 애플리케이션 : 최소 처리율 보장,
                                    대역폭 민감 애플리케이션이 특정 처리율을 요구사항을 갖고 있다.

          탄력적 애플리케이션 : 가용한 처리율을 많으면 많은대로 적으료 적은대로 이용한다.( 전자메일,파일,웹)

#### 3. 시간 보장
> 낮은 지연 시간, 실시간 애플리케이션

#### 4. 보안
> 암호화, 종단 인증(end-point authentication), data integrity


-  ##  TCP 서비스 
    TCP 연결-> HTTP 요청 보내기
1. **reliable transport**(신뢰적 데이터 전송 서비스 ): process 간 믿음 통신을 한다.(손상이나 손실) 문서를  하나도 빠드리지 않고 손실이나 손상이나 손상이 없게 전송
2. **connection-oriented** : client and server 연결되어 있다.
3. **flow control** :  송신자가 배려를 한다.(packet이 오버플로우 될수 있으니)
4. **congestion(拥塞) control** : network가 트래픽이 혼잡상태에 이르면 프로세스 속도를 낮춘다.
5. **제공 안하는 것들**: timing,최소 전송율,**보안**

#### Securing TCP(보안 TCP) -> ssl
> 최소 서비스 모델을 가진 간단한 전송 프로토콜
- TCP, UDP는 암호화를 제공하지 않는다.(그래서 위협에 노출 된다.)


        SSL(secure socket layer): TCP 암호화 제공
                                  DATA 무결성
                                  end-point authentication(통신상대 인증)
        SSL 은 Application layer에 존재한다.
        - 애플리케이션이 SSL 서비스를 사용할때 Client process는 data를 SSL socket에 전달하고 SSL은 Data를 암호화 하여 TCP에 전달을 한다.



- ## UDP 서비서
> RTP
> 손실이나 손상이 있어도 문서를 보낸다.
> TCP에 제공하는 것을 다 제공 안한다.

---
# Web
> web page는 객체들로 구성된다.
>객체(object) 는 url로 지정할수 있는 하나의 파일(html,jpeg,gif)

- 클라이언트:browser -> 요청
- 웹 서버: sever -> 응답
- 둘다 HTTP 프로토콜 사용

# HTTP(hypertext transfer protocol)
- application layer
> HTTP는 웹 클라이언트가 웹 서버에게 웹 페이지를 어떻게 요청하는지 와 서버가 클라이언트로 어떻게 웹 페이지를 전송하는지 정의

> **비상태 프로토콜**:HTTP 서버는 클라이언트에 대한 상태 정보를 유지,기억하지 않는다.그래서 칭한다.

![image](https://user-images.githubusercontent.com/86946575/159415717-2afb1a87-7b73-420c-ad93-1fbcd3d11f34.png)

> HTTP는 TCP를 사용
1.  client가 TCP를 사용 하여 서버에 연결(socket 번호 생성) 안전하게 하기 위해
2.  server TCP연결을 받다.
3.  HTTP message를 서로 교환한다.
4.  TCP 연결 끝

- ## 비지속(No3n-persistent) 연결 HTTP

      RTT(round trip time) : 패킷이 client로 부터 sever까지 가고 다시 돌아오는 데 걸리는 시간을 말한다.

1. 첫번째 RTT는 TCP 연결 요청
2. 두번쨰 RTT는 file 요청

- 비지속 연결 == 2RTT + file transmiision time (파일 전송 시간)
![image](https://user-images.githubusercontent.com/86946575/159417839-777be0eb-cdad-4c7e-8941-76db743170db.png)

- 단점
1. 각 요청 객체에 대한 새로운 연결이 설정 작업과 TCP버퍼 할당, TCP 변수들이 클라이언트 서버 양쪽 유지
2. 2RTT per object
3. OS overhead

- ## 지속 연결 HTTP ( defalut )
- 서버는 응답을 보낸 후에 TCP연결을 그대로 유지 한다.
- pipelining 가능
  - 응답을 기다리지 않곻 여러 요청을 연속 전송 가능(싱글 connection)

- HTTP/2 :다중 요청과 응답. 우선수위 부여
![image](https://user-images.githubusercontent.com/86946575/159418946-111e3493-5911-4bf1-9366-7b4ffda065c7.png)


- ## HTTP 메시지 포맷
![image](https://user-images.githubusercontent.com/86946575/159420363-65a45c33-e1e9-487e-b381-554e3277b0cc.png)
![image](https://user-images.githubusercontent.com/86946575/159419678-fea6cf64-a5c3-4179-94b0-8f9d17c53fd1.png)


        요청 line : ( GET,POST,HEAD,PUT )  "URL 필드 "  "HTTP 버전"
        헤더 line :( 헤더 필드 이름)  //호스트 명시
        
        상세:95page

        \CR\IF 만 있으면  헤더 필드 끝났고 바디로 진입 



![image](https://user-images.githubusercontent.com/86946575/159421600-89eee5e6-a68b-4f12-896c-e07170cf4eb9.png)

        status line: "protocol", "atatus code",  "status pharse"

        200  -OK
          • request succeeded, requested object later in this msg
        301 -Moved Permanently
          • requested object moved, new location specified later in this msg
          (Location:)
        400 -Bad Request
           • request msg not understood by server
        404 -Not Found (항상 나오자나)
           • requested document not found on this server
        505 - HTTP Version Not Supported


        Telnet: 피시를 터미널로 열어서 테스트

## 쿠키 (cooki)
> 지금 web의 client 가 어떤 상태의 고객인지
> 사용자 식별

- 개인 정보침해소지 ,논란 보안 취약
![image](https://user-images.githubusercontent.com/86946575/159423630-028d142e-9005-4a9d-b8b6-a50bef859d85.png)

## web cache ( proxy server )
> 목표: 원본 서버를 포함하지 않고 클라이언트 요청을 충족
- 웹 캐시는 client 이자 server 이다.
> 웹 캐시는 자체의 저장 디스크를 갖고 있어 최근에 호출된 사본을 저장 및 보존
- 보통 0.4% 캐시   | 0.6 origin server 


        사용이유:1. 웹 캐시는 클라이언트의 요구에 대한 응답시간을 줄인다.(특히 클라이언트와 원본 서버가 명목 대역폭이 차이가 있을때 )
                 2. 웹 캐시는 링크상의 웹 트래픽을 대폭으로 줄일수 있다.
![image](https://user-images.githubusercontent.com/86946575/159429806-a0227b3f-8e95-452a-b4c3-5d97760683a7.png)



      LAN 트래픽 강도: (15요청/초) *(1Mbit)/(100Mbps)=0.15
      access link 트래픽 강도: (15요청/초) *(1Mbit)/(15Mbps)=1 
      end-to delay= internet + lan + accesslink( 분 )
                      2    + 0.15 + 1 분  = 1분 2.15초  
![image](https://user-images.githubusercontent.com/86946575/159434779-1a96b09f-b3c0-4eaa-bfc2-ff031a371400.png)

      로컬 캐시: 0.6 *(2.01) + 0.4*(~10msecs)=~ 1.2 secs

### 조건부 get 
> proxy server data 최신인지 
> 목표는 최신 data만 전송 


          최신이 아니면 : 304 전송  not modified
          최신 data 라면: data 전송 하고  200
![image](https://user-images.githubusercontent.com/86946575/160795862-fbd3656e-8356-460a-8064-9cc0e80209d1.png)

---
# 전자메일 :electronic mail (application layer )
- 구성 요소 세가지:
  - user agents :  사용자
  - mail server : 메일간 통신하는 서버
    - mail box:외부에서 받은 메일을 저정한다 ,접속하면 전달 
    - message queue: 메시지를 외부로 보낼 것을 저장 ,후에 전송
    - smtp protocol
  - SMTP : 메일 통신  Protocol

## SMTP
> 메일 서버로부터 다른 메일 서버로 파일 전송
> TCP 사용 ,Port 25 (HTTP 80)
> ASCII 7bit ,1bit는 미리 에러 검증
- 전송 단계 ：
  1. handshaking(greeting) : TCP가 DATA를 상대의 reliably 하게 하기위해 (연결)
  2. Transfer of messages
  3. closure
> command:ASCII text
> reponse: status cpde amd phrase 


            SMTP 와 HTTP 차이점:
                1.HTTP는 풀(pull) 프로토콜
                  SMTP는 푸시(push) 프로토콜  ：push이기 때문에 agent -> server -> serber   (!=-> agent)  안되기 때문에 마지막만 다른 protocol(HTTP,IMAP,POP3) 사용한다.
                
                2. SMTP는  7비트 ASCII
                   http는 제한 이 없다

                3. HTTP는 자신의 응답 메시지 각 객체를 캡슐화 한다.
                   SMTP는  모든 메시지의 객체를 한 메시지로 만든다.  
            
            같은점 :둘다 지속 연결 전송시

- RFC 5322:Standard for text message format:
  - TO
  - From
  - subject

#### 메일 접속 프로토콜(user agent가 메일을 읽어올때 사용)
##### POP3
> 3단계 과정
> - Authorization(인가)：사용자 인증하기 위해 **이름**과 **비밀번호**를 보낸다
> - 트랜잭션 : 메시지를 가져오고 메일 통계를 얻을 수 있다.
> - 갱신 :클라이언트가 pop3 세션을 끝내는 quit 명령이 내려진 후에 일어난다.
- 문제점 : 데이터 저장을 안한다, 유저 statless;

##### IMAP
> 데이터를 저장
> 메시지를 폴더화
> Keep user state 

![image](https://user-images.githubusercontent.com/86946575/160957411-52f0786c-20cb-46e5-a196-c3ccd791593f.png)

#### HTTP
---
# DNS
> 전화 번호 부 같은 느낌
- IP address : 32bit
- applecation- layer
- 분산 DB
- 인터넷 기능 제공
![](https://user-images.githubusercontent.com/86946575/160958105-e739513a-e42a-4095-a715-75670d348742.jpg)

### DNS 특징 기능
1. hostname to IP address : 주소찾아주기
2. host aliasing :별칭 기능 (즉 정식호스트 네임 [canonical hostname] 길고 복잡한 ) :정식 호스트를 가르치는 별명 병칭
3. mail server aliasing : 기억하기 쉬운 mail server 별칭 이름  사용 
4. load distrubution: 중복 웹서버 들은 여러 ip 주소,ip 주소 집합중에 순환식으로 제공 (부하 분산)

### DNS 왜 집중 관리
1. Single point of failure
2. traffic volume
3. distant centralized database
4. maintain

          확장성이 전혀 없다. 

### 분산 계층 데이터베이스
1. root DNS Server
  - 13개 기관에서 관리 (세계)

1. TLD(top level dns)
  - 일반: .com.org etc.
  - 국가: .kr.uk  etc.

2. authoritative DNS Server(책임 )
  - 기관 책임 DNS 서버
  - 보조 책임 DNS 서버(백업)

3. Local DNS name server 
  - proxy server 역할
  - 최근에 방문했던적이 있으면 기록
  - DNS 캐싱
> 반복적 질의 O
> 재귀적 질의 x (서버에 부담이 크다)

![image](https://user-images.githubusercontent.com/86946575/160962428-66f86abe-77f4-4256-b4ad-f14f0c6bd589.png)

### DNS resource record (RR)
> DNS 서버들은 호스트 네임을 ip 주소로 매핑하기 위한 **자원레코드(RR)**을 저장한다.
> TTL은 생존기간

![9c898c0afde185bf963b1a14e8d44fc](https://user-images.githubusercontent.com/86946575/160968949-9af3c6bf-415d-45da-a865-9831702511a3.jpg)


### DNS 메시지 포맷
![373f3f15ff95b4d73a885b573da3c78](https://user-images.githubusercontent.com/86946575/160969552-16973890-c5c1-442a-b2c5-1bee1c86ca44.jpg)

### DNS 보안 취약점
- DDOS :
  - Botnet으로 대량의 ICMP Ping message를 전송

- dns spoofing:
  - man-in-the-middle attack
  - 가까 응답을 리턴
- 반사증폭
  - 가짜 질의 메시지를 전송 
  - 위장된 출발지 주소에 다량의 응답메시지 


---
# P2P
> 특징들:
>  - no always-on server
>  - 임의의 end system끼리 직접 연결
>  - intermittently 연결,ip 주소 변경


      client-server:
                    server transmission: F/us (1개의 copy를 전송)
                                        NF/us (N개의 copy를 전송)
                    client download rate= d min
                    client download time= F/d min
                    
                  (client-server)  Dc-s >=max{NF/us,F/d min}

               P2P :
                    server transmission: F/us (1개의 copy를 전송)
                    client download time= F/d min
                    clients들이 통합  download bit: us+ 합ui
                    Dp2p >=max{NF/us,F/d min , NF/(us+합ui)}

### bittorrent(비트 토렌토)
> 파일 분대를 위한  프로토콜
> 파일 256kb로 짜른다 ==> 이것을 **chunk**라 칭
- torrent: group of peers
- tracker: 트랙커는 토렌트에 참여하는 피어들을 추적

        bittorrent 문제: Free-rider 문제: download만 하고 upload 안하는
                        이걸 해결하는 방법: "tit-for-tat": TFT
                                          5개의 Peer만 활성화
                                          - 그중 4개랑은 자주 통신(TOP 4)
                                          - 마지막 하나는 30초마다 랜덤으로 연결해서 잘해주기

---
# video streaming and CDNs
- 이질성 문제
- 해결방안 : 서버분산, application-level 기초구조

### DASH
> 모든 클라이언트들이 가용 대역폭의 차이에 불구하고 똑같이 인코딩된 비디오를 전송 
> - 서로 다른 cilent의 접속 대역폭 차이 뿐만 아니라
> * 혼잡, 이동 등 시간에 따른 가용 대역폭 변화 에서도 발생 
> DASH에서 비디오는 여러 개의 서로 다은 버전으로 인코딩, 각각 버전은 서로 다른 비트율과 품질
- chunk 단위로 비디오 요청
- client에게 맞는 화질 전송
- 기존 http streaming문제점: 여러 client의 가용 대역폭의 차이가 있음에도 불구 동일한 인코딩 비트율로 비디오 전송
- Streaming video=encoding + dash + playout buffering 


# CDN 나온이유 
- 어떻게 수천수만명한테 동시 스트림?
  - 큰 하나의 mega-server 구축 (문제점 3개)
    - 1. long path
    - 2. 대역폭 낭비
    - 3. single point of failure
  - CDN(분산 방식)
    - 지연시간 및 처리율 향상

### CDN 두가지 방식 
#### Enter deep
> 서버 클러스트를 접속 네트워크에 구축함으로서 ISP의 접속 네트워크로  들어가는 것 
