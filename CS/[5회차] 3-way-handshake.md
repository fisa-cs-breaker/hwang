염씨의 주제선정 3-way handshake
그게 뭔데….. 처음 들어봄
블로그 대충만 훑어봐도 어려운데요? 휴…


## 3-Way Handshake란?

<aside>
👉 TCP/IP 프로토콜을 이용해서 통신하는 응용프로그램은 데이터를 주고받기 전에 먼저 연결을 진행하는데,
이 연결 과정을 3-Way-HandShake라고 한다네요

</aside>

### 그 전에, TCP/IP 프로토콜을 확실히 짚고 넘어가염

> **IP (3-네트워크 계층)
패킷 통신 방식의 인터넷 프로토콜**
> 
- 패킷 전달 여부를 보증하지 않음
- 패킷을 보낸 순서, 받는 순서가 다를 수 있음

+ **네트워크 계층(IP)**

- IP가 활용되는 부분으로
- 한 엔드포인트가 다른 엔드포인트에 가고자할 때, 경로와 목적지를 찾아줌 ⇒ “라우팅”이라고 함
- 대역이 다른 IP들이 목적지를 향해 제대로 찾아갈 수 있도록 돕는 역할

> **TCP(4-전송계층)
전송 조절 프로토콜**
> 
- IP 위에서 동작하는 프로토콜
- 데이터의 전달을 보증하고
- 보낸 순서대로 받게 해준다.

**+ 전송계층(TCP&UDP)**

- 송수신자의 논리적 연결 담당하는 계층으로,
- 신뢰성 있는 연결 유지할 수 있게 도와줌
- 엔드포인트 간 연결 생성하고 데이터 얼마보내고 받았는지 잘 받았는지를 확인함

> **TCP/IP**
> 

HTTP, FTP, SMTP등 TCP를 기반으로 한 많은 애플리케이션 프로토콜들이 IP 위에서 동작하기 때문에, 묶어서 TCP/IP로 부른다고…

- TCP/IP를 쓰겠다는 건
    - IP 주소 체계를 따르고, IP 라우팅을 이용해 목적지에 도달하며
    - TCP 특성을 활용해서 송/수신자의 논리적 연결을 생성하고 신뢰성을 유지할 수 있도록 하겠다는 의미

즉, TCP/IP를 말한다는 건…

송신자가 수신자에게 IP 주소를 사용하여 데이터를 전달하고 그 데이터가 제대로 갔는지, 너무 빠르지는 않은지, 제대로 받았다고 연락은 오는 지에 대한 이야기를 하는 거다.

⇒ 그래서 결론은 이 엔드포인트끼리 TCP/IP 기반으로 데이터 주고 받기 전의 연결을 어떻게 하는 지 그 과정을 알아보잠

## 3-Way Handshake에서 사용되는 TCP 헤더 필드는?

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/e2e97d7e-cd66-43af-87fe-6de6ed3e48fc/6ee42e98-69c6-4277-ba70-69b3cb69549f/Untitled.png)

### Sequence Number

> Segment에 있는 첫 번째 바이트의 바이트 스트림 번호
> 
- TCP는 데이터를 순서대로 정렬되어 있는 바이트 스트림으로 봄
    - ex)
        - 0~999의 segment를 보내면 seq#=0
        - 1000~1999의 segment를 보내면 seq#=1000
- TCP 연결과 종료 시에는 Sequence Number를 임의의 랜덤 값으로 설정 함.
    - 이게 노출되면 공격자가 위조 패킷을 보낼 수 있기 때문에, 보안 상!

### Acknowldgement Number

> 받고 싶은 다음 바이트 번호
> 

ex) 

0~999의 segment를 받았을 때, ack# = 1000

1000~1999의 segment를 받았을 때, ack# =2000

### Control bits

### ACK

> 패킷을 받았다는 응답을 할 때 사용함.
> 
- Acknowldgement Number가 유효한지를 나타냄
- 최초 연결의 첫번째 세그먼트를 제외한 모든 세그먼트의 ACK 비트는 1로 설정
    - 최초 연결, 첫번째 HandShake과정에서는 응답할 요청이 없으므로

### SYN

> Synchronize Sequence Number
연결을 요청할 때 SYN bit 사용
> 
- 연결을 요청하는 경우에 bit 1로 설정
    - 그래서 이 비트가 1이면 TCP 연결을 요청하는 중이라는 걸 알 수 있음
- 다른 모든 경우에는 bit 0으로 설정
    - 연결 요청에 응답하는 마지막 HandShake의 세그먼트에서도 0으로 설정

## 그럼 이제 실제 과정을 알아보자

TCP 통신은 PAR (Positive Acknowledgement with Re-transmission)을 통해 신뢰적인 통신을 제공. PAR 사용하는 기기는 ack를 받을 때까지 데이터 유닛(세그먼트)을 재전송함.

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/e2e97d7e-cd66-43af-87fe-6de6ed3e48fc/fca73aff-1dda-482b-8f74-b1dee806b083/Untitled.png)

수신자가 데이터 유닛이 손상되는 것을 확인하면, 해당 세그먼트를 없앤다.

그럼 발신자는 positive ack이 오지 않은 데이터 유닛을 다시 보내야 한다.

⇒ 이 과정에서 클라이언트 서버 사이에서 3개의 세그먼트(데이터 유닛)이 교환됨. 이게 3-way handshake의 기본 매커니즘

### 작동 방식

- Client는 Server와 연결하기 위해 3-way handshake를 통해 연결 요청을 함.
    - 우리가 일반적으로 생각하는 Client와 Server는 모두 서로 연결 요청을 먼저 할 수 있기 때문에, 연결 요청을 먼저 시도한 요청자를 Client(아래 그림에서 HOST P) 로, 연결 요청을 받은 수신자를 Server(아래 그림에서 HOST Q)쪽으로 생각하기
- **SYN(Synchronization)** : 연결 요청, 세션을 설정하는데 사용되며 초기에 시퀀스 번호(seq=x)를 보냄
- **ACK(Acknowledgement)** : 보낸 시퀀스 번호에 TCP 계층에서의 길이 또는 양을 더한 것과 같은 값을 ACK에 포함하여 전송
    - 동기화 요청에 대한 답변 : `Client의 Sequence Number+1`(seq=x+1)을 하여 ACK로 돌려줍니다.

!https://velog.velcdn.com/images%2Faverycode%2Fpost%2F22a2bab1-c8dd-4559-88b2-62d03cbff927%2F%E1%84%80%E1%85%B5%E1%84%89%E1%85%AE%E1%86%AF%E1%84%86%E1%85%A7%E1%86%AB%E1%84%8C%E1%85%A5%E1%86%B8-5.jpg

### Port 상태 정보

- **LISTEN** : 포트가 열린 상태로 연결 요청을 대기하는 상태
- **SYN-SENT** : 연결 요청을 하고 Server의 ACK를 기다리는 상태
- **SYN_RCVD** : 연결 요청에 응답/연결을 요청하고 Client의 응답을 기다리는 상태
- **ESTABLISHED** : 연결된 상태

### 작동 순서

- **Step 1 (SYN)**
    
    **클라이언트는 서버와 연결하기 위해 SYN을 보낸다. (seq = x)**
    
    - 송신자가 최초로 데이터를 전송할 때 Sequence Number를 임의의 랜덤 숫자로 지정(보안 위해서)하고, SYN 플래그 비트를 1로 설정(TCP 전송 중 이라는거 표시!)한 세그먼트를 전송한다.
    - PORT 상태
        - Client : `CLOSEDSYN_SENT` 로 변함
        - Server : `LISTEN`
- **Step 2 (SYN + ACK)**
    
    **서버가 SYN(x)을 받고, 클라이언트로 받았다는 신호인 ACK와 SYN 패킷을 보냄 (seq : y, ACK : x + 1)**
    
    - 접속 요청을 받은 Q(server)가 요청을 수락했으며, 접속 요청 프로세스인 P(client)도 포트를 열어 달라는 메세지를 전송 (SYN-ACK signal bits set)
    - ACK Number필드를 Sequence Number(x) + 1 로 지정하고 SYN과 ACK 플래그 비트를 1로 설정한 세그먼트 전송 (`Seq=y, Ack=x+1, SYN, ACK`)
    - PORT 상태
        - Client : `CLOSED`
        - Server : `SYN_RCV`
    
    <aside>
    👉 **[Step 2]**는 엄밀히 말하면 **2개의 Handshake에 해당**한다.
    
    1. 응답하는 ACK Segment 전송
    
    2. 연결을 요청하는 SYN Segment 전송
    
    **두 과정은 서로에게 영향을 끼치지 않고 독립적.** 따라서, 1번과 2번 Segment 정보를 **동시에 전송**함으로써 1개의 Handshake로 처리할 수 있고, **네트워크 트래픽을 절약**할 수 있다.
    
    </aside>
    
- **Step 3 (ACK)**
    
    **클라이언트는 서버의 응답인 ACK(x+1)와 SYN(y) 패킷을 받고, ACK(y+1)를 서버로 보냄**
    
    - 마지막으로 접속 요청 프로세스 P가 수락 확인을 보내 연결을 맺음 (ACK)
    - 이때, 전송할 데이터가 있으면 이 단계에서 데이터를 전송할 수 있다.
    - PORT 상태
        - Client : `ESTABLISED`
        - Server : `SYN_RCV` ⇒ ACK ⇒ `ESTABLISED`

### 추가) full-duplex 통신의 구성

- Step 1, 2에서는 P→Q 방향에 대한 연결 파라미터(시퀀스 번호)를 설정하고 이를 승인한다.
- Step 2, 3에서는 Q→P 방향에 대한 연결 파라미터(시퀀스 번호)를 설정하고 이를 승인한다.

⇒ 이를 통해 full-duplex 통신이 구축.

참고: [https://velog.io/@averycode/네트워크-TCPUDP와-3-Way-Handshake4-Way-Handshake](https://velog.io/@averycode/%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC-TCPUDP%EC%99%80-3-Way-Handshake4-Way-Handshake)

https://hojunking.tistory.com/106
