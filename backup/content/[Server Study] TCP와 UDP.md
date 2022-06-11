---
title: "[Server Study] TCP와 UDP"
description: "새로운 인터넷 어플리케이션을 만들 때 첫번 째 결정해야 하는 것 중 하나는 UDP 혹은 TCP 중 어느 것을 사용할지를 결정하는 것이다.OSI 7계층(https&#x3A;//velog.io/@selenium/Server-Study-%EB%84%A4%ED%8A%B8%EC"
date: 2022-05-08T14:46:37.719Z
tags: []
---

새로운 인터넷 어플리케이션을 만들 때 첫번 째 결정해야 하는 것 중 하나는 UDP 혹은 TCP 중 어느 것을 사용할지를 결정하는 것이다.

[OSI 7계층](https://velog.io/@selenium/Server-Study-%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%ACNetwork%EC%99%80-%ED%94%84%EB%A1%9C%ED%86%A0%EC%BD%9CProtocol-OSI-7-%EA%B3%84%EC%B8%B5Layer#%EA%B7%B8%EB%9E%98%EC%84%9C-%EC%96%B4%EB%96%BB%EA%B2%8C-%EA%B3%84%EC%B8%B5%EC%9D%84-%EB%82%98%EB%88%B4%EB%8A%94%EB%8D%B0---osi-7%EA%B3%84%EC%B8%B5-tcpip-4%EA%B3%84%EC%B8%B5)에서 [Transport Layer(전송계층)](https://velog.io/@selenium/Server-Study-%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%ACNetwork%EC%99%80-%ED%94%84%EB%A1%9C%ED%86%A0%EC%BD%9CProtocol-OSI-7-%EA%B3%84%EC%B8%B5Layer#4-transport-layer%EC%A0%84%EC%86%A1-%EA%B3%84%EC%B8%B5)에 속하는 프로토콜인 TCP와 UDP 에 대해 조금 더 자세히 알아보고자 한다.


<br/>  

## TCP(Transmission Control Protocol)

- 항상 단일 송신자와 단일 수신자 사이의 점대점(Point-to-point)임

- `연결 지향형 서비스`

    - 애플리케이션 계층 메세지를 전송하기 전에 TCP는 클라이언트와 서버가 서로 전송 제어 정보를 교환하도록 함
    
    - 이 Hand shaking 과정이 클라이언트와 서버에 패킷이 곧 도달할 것이니 준비하라고 알려주는 역할을 함
    - Hand Shaking 단계를 지나면, TCP 연결이 두 프로세스의 소켓 사이에 존재한다고 얘기함
    - 이 연결은 두 프로세스가 서로에게 동시에 메세지를 보낼 수 있기에 **Full-Duplex(전이중)** 연결이라고 함
    	![](/images/07f23acf-de3b-406c-8923-228e76371323-image.png)

    - 애플리케이션이 메세지 전송을 마치면 연결을 끊어야 함
    
    
- `신뢰적인 데이터 전송 서비스`
    - 통신 프로세스는 모든 데이터를 오류없이 올바른 순서로 전달하기 위해 TCP에 의존함
    - TCP는 애플리케이션의 한쪽이 바이트 스트림을 소켓으로 전달하면 그 바이트 스트림을 손실하거나 중복되지 않게 수신 소켓으로 전달함
    
    - 데이터에 loss가 없도록 하기 위해 구조가 복잡하고 헤더 size도 더 큼

<br/>  

- TCP는 **혼잡제어(Congestion Control)** 방식, 즉 통신하는 프로세스의 직접 이득보다는 인터넷의 전체 성능 향상을 위한 서비스를 포함
   - TCP의 혼잡 제어 방식은 네트워크가 혼잡상태에 이르면 프로세스 속도를 낮춤
   - 각 TCP연결이 네트워크 대역폭을 공평하게 공유할 수 있게끔 제한하려고 시도함


- `Connection- Oriented`
    - 바로 데이터를 보내는 것이 아니라 Handshaking을 한 후에 보냄

<br/>  

- 보안의 관점에서 보자!
    - TCP나 UDP는 암호화를 제공하지 않음
    - 암호화 되지 않은 평문은 송신자와 수신자 사이의 모든 링크를 흘러가게 되는데 중간에 어떤 링크에서나 탐지되고 발견될 가능성이 높음
    - TCP를 강화한 **`SSL(Secure Sockets Layer)`**을 개발함
    - SSL은 TCP가 하는 모든 것을 할 뿐만 아니라 암호화, 데이터 무결성 그리고 종단 인증을 포함하는 중요한 프로세스 대 프로세스 보안 서비스를 제공함
    
    

<br/>  
<br/>  

## UDP(User Datagram Protocol)

- 최소의 서비스 모델을 가진 간단한 전송 프로토콜 (깔꼼)

- 할 수 있는 최선을 다하지만 보장은 하지 않음

- `비 연결형`

    - 두 프로세스가 통신을 하기 전에 Hand Shaking을 하지 않음

- `비 신뢰적인 데이터 전송 서비스`

    - 하나의 프로세스가 UDP 소켓으로 메세지를 보내면 UDP는 그 메세지가 수신 소켓에 도착하는 것을 보장하지 않음❌
    - 게다가 수신 소켓에 도착하는 메세지들의 순서가 뒤바뀔 수도 있음
    
- 혼잡제어 방식을 포함하지 않음
	- UDP 송신 측은 데이터를 원하는 속도로 하위계층(네트워크 계층)dmfh qhsof tn dlTdma
    
    


<br/>  
<br/>  


## Hand Shake가 뭐길래?

위에서 언급했듯
`TCP`는 장치들 사이에 접속을 Establish 하기 위해** 3-way handshake** 를 사용함


### 2-way handshake 은?

![](/images/754dcbd3-d5e9-4586-bda6-7d7945e4b7b3-image.png)


- 요청을 보내고
- 요청이 정상적으로 돌아오면
- 실질 데이터를 전송하는 방법임

<br/>  

###  2-way handshake 가 무슨 문제점이 있는데?

![](/images/1ad00eba-ce74-4ef1-a9ec-93cab4270cea-image.png)

- 요청을 보내고, 요청에 대한 응답이 정상적으로 돌아오지 않았다면
- 송신자는 다시 똑같은 요청을 재전송 할 것임
- 그 요청을 이전과 다른 연결로 생각해서 문제 발생 가능성이 있음

<br/>  

### 그럼 어떻게 해야돼? - 3-way handshake

![](/images/f84de321-f3b4-4de1-b3b8-1de0fc6e00d1-image.png)

- 일반적으로 TCP연결을 초기화 할 때 사용함

1. client가 연결에 대한 요청 `SYN`을 보내고
2. Server가 SYN에 대해 요청을 수락한다는 `SYNACK`를 보낸다
3. client가 잘 받았다고 `ACK`를 보내고 연결이 이루어 짐
4. 데이터 송/수신

<br/>  

### 4-way handshake 도 있다고?

![](/images/8114849f-8e79-46e9-b5a7-db378314006e-image.png)

- 일반적으로 TCP연결이 종료될 때 사용됨

1. client가 연결을 종료하겠다는 `FIN`을 전송
2. Server가 알겠다고 일단`ACK`를 보냄
	- 이 상태에서 data 아직 전송 가능함)
  	- client는 서버가 close 를 할 때까지 기다림
3. Server가 연결 해지 준비 다 되었다고 `FIN`을 보냄
4. client는 알겠다고 `ACK`보냄
5. 연결 종료




<br/>  
<br/>  

References
[컴퓨터 네트워킹 하향식 접근 - James F.Kurose]