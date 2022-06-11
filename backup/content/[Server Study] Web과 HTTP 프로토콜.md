---
title: "[Server Study] Web과 HTTP 프로토콜"
description: "서버 면접 기본 질문이였던 HTTP에 대해 알아보자.HTTP에 관해서만 몇 백 페이지 짜리 책이 있을 정도로 파고들다 보면 어떻게 보면 굉장히 광범위한 내용이다.간단하게 핵심만 알아보도록 하자! 왜냐면 나도 잘 모루니까~Hyper Text Transfer Protoco"
date: 2022-05-08T15:12:45.451Z
tags: []
---

서버 면접 기본 질문이였던 HTTP에 대해 알아보자.

HTTP에 관해서만 몇 백 페이지 짜리 책이 있을 정도로 파고들다 보면 어떻게 보면 굉장히 광범위한 내용이다.

간단하게 핵심만 알아보도록 하자! 왜냐면 나도 잘 모루니까~

<br/>  

## HTTP 란?
> Hyper Text Transfer Protocol 의 약어로, **인터넷에서 데이터를 주고받을 수 있는 [프로토콜](https://velog.io/@selenium/Server-Study-%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%ACNetwork%EC%99%80-%ED%94%84%EB%A1%9C%ED%86%A0%EC%BD%9CProtocol-OSI-7-%EA%B3%84%EC%B8%B5Layer#%ED%94%84%EB%A1%9C%ED%86%A0%EC%BD%9Cprotocol%EC%9D%B4-%EB%AD%94%EB%8D%B0) **

- 웹의 [Application Layer(응용 계층)](https://velog.io/@selenium/Server-Study-%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%ACNetwork%EC%99%80-%ED%94%84%EB%A1%9C%ED%86%A0%EC%BD%9CProtocol-OSI-7-%EA%B3%84%EC%B8%B5Layer#1-application-layer%EC%9D%91%EC%9A%A9-%EA%B3%84%EC%B8%B5)의 프로토콜임

- 서로 다른 엔드 시스템에서 수행되는 클라이언트 프로그램과 서버 프로그램은 서로 HTTP 메세지를 교환하여 통신함

- HTTP는 웹 클라이언트가 웹 서버에게 웹 페이지를 어떻게 요청하는지와 서버가 클라이언트로 어떻게 웹 페이지를 전송하는지를 정의
  ![](/images/93024242-4e1b-4a27-98d5-353a80ca1f61-image.png)


<br/>  

## HTTP는 [TCP](https://velog.io/@selenium/Server-Study-TCP%EC%99%80-UDP#tcptransmission-control-protocol)를 전송 프로토콜로 사용한다!
- HTTP 클라이언트는 먼저 서버에게 TCP연결을 시작함

- 일단 연결이 이루어지면, 브라우저와 서버 프로세스는 그들의 소켓 인터페이스를 통해 TCP로 접속함
- 클라이언트 측에서 보면 소켓 인터페이스는 클라이언트 프로세스와  TCP연결 사이에서의 출입구임

- TCP는 신뢰적 데이터 전송 서비스를 제공하므로 HTTP요청 메세지가 궁극적으로 서버에 잘 도착함을 의미함

- Server가 Client에게 요청 파일을 보낼 때, 서버는 클라이언트에 관한 어떠한 상태 정보도 저장하지 않음 -> **`Stateless Protocol(비상태 프로토콜)`**
	(Cookie를 사용하여 stateful 하게 만듦)


<br/>  


## HTTP 연결(non-persistance, persistance)

클라이언트 - 서버 상호작용이 TCP상에서 발생할 때,
각 요구/응답 쌍이 분리된 TCP 연결을 통해 보내져야 하는가? - `비지속연결`
혹은 모든 요구와 해당하는 응답들이 같은 TCP 연결 상으로 보내져야 하는가? - `지속연결`

를 결정할 필요가 있다.

- HTTP는 비지속연결과 지속연결 모두를 사용할 수 있음
- 디폴트 모드는 `지속연결`임


<br/>  

### 1. 비지속 연결(non-persistent connection)

- 한 TCP 연결 마다 한 Object만 보냄

- 진행과정
    1. 클라이언트가 해당 서버에 TCP 연결 요청
    2. 서버에서는 연결을 승인
    3. client가 URL을 포함한 request msg 보냄
    4. 서버에서 메세지에 따라 해당 object file을 response
    5. object 잘 받았으면 서버에서 TCP연결 끊음
    6. object 개수에 맞춰 반복

- 독립적으로 수행되기 때문에 중간에 문제 생겨도 받은 만큼 표시

- 한 object 에 대해 `2개의 RTT + file전송시간`이 요구됨
	![](/images/49c485da-fd11-4ffd-9c8f-91386043ceed-image.png)





<br/>  

### 2. 지속 연결(persistent connection)


![](/images/aefb57a3-7568-4356-8896-1462e8df0068-image.png)

- multi object 들이 TCP연결 하나로 전송될 수 있음

- 한번  TCP연결 되고 나서 계속 object 요청과 전송 반복됨

- 중간에 file이 잘못되어 연결이 끊어지면 처음부터 다시 해야 한다는 단점이 있음😥

- 요즘은 거의 persistent 방식 사용함

- persistent HTTP의 response time = `1RTT + {num(object) * (RTT+file transmisson time)}`

<br/>  

HTTP 메소드에 대해서는 REST와 함께 다음 글에서 알아보도록 하자!

<br/>  
<br/>  


References
http://www.ktword.co.kr/test/view/view.php?m_temp1=648
[컴퓨터 네트워킹 하향식 접근 - James F.Kurose]