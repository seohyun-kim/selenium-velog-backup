---
title: "[Server Study] 인터넷(Internet)과 네트워크(Network)"
description: "너무 흔하게 듣는 말인 인터넷. 그러나 인터넷의 정의에 대해 묻는 다면 얼마나 제대로 대답할 수 있을까?인터넷은 컴퓨터로 연결하여 TCP/IP 통신 프로토콜을 이용해 정보를 주고받는 컴퓨터 네트워크이다.Host = end system(종단 시스템)		: PC, Serv"
date: 2022-05-08T11:52:07.732Z
tags: []
---

## 인터넷(Internet) 이 뭘까?
너무 흔하게 듣는 말인 인터넷. 
그러나 인터넷의 정의에 대해 묻는 다면 얼마나 제대로 대답할 수 있을까?

> 인터넷은 컴퓨터로 연결하여 `TCP/IP` 통신 `프로토콜`을 이용해 정보를 주고받는 컴퓨터 `네트워크`이다.

<br/>  

## 구성 요소로 보는 인터넷

![](/images/6036f733-9922-485f-8b10-f5bbb9bd07ec-image.png)


- `Host` = `end system(종단 시스템)`
	: PC, Server, Wireless laptop, smartphone

- 종단 시스템은 `통신 링크(communication link)`와 `패킷 스위치(packet switch)`의 네트워크로 연결됨

- 한 end system 에서 다른 end system으로 보낼 데이터를 가지고 있을 때, 송신 종단 시스템은 그 데이터를 `Segment(세그먼트)`로 나누고 각 segment에 `Header(헤더)`를 붙임.
이렇게 만들어진 정보 패키지는 컴퓨터 네트워크에서 `Packet(패킷)`이라 부름
packet은 목적지 end system으로 네트워크를 통해 보내지고, 목적지에서 원래의 데이터로 다시 조립됨.

<br/>  

### 패킷 스위치(Packet Switch)
- 종류:  `라우터(router)`, `링크 계층 스위치(link-layer switch)` 가 있음

- 두 형태의 스위치는 최종 목적지 방향으로 패킷을 전달
- 링크 계층 스위치는 보통 엑세스 네트워크에서 사용되고, 라우터는 네트워크 코어에서 사용됨

<br/>  

### ISP(Internet Service Provider)
- 종단 시스템은 ISP를 통해서 인터넷에 접속함
- ISP는 패킷 스위치와 통신 링크로 이루어진 네트워크임
- 인터넷 서비스를 제공하는 업체를 칭함

<br/>  

### 프로토콜(Protocol)
- 인터넷에서 정보 송수신을 제어하는 여러 프로토콜(Protocol)을 수행함
- 특히 TCP(Transmission Control Protocol) 와 IP(Internet Protocol)는 인터넷에서 가장 중요한 프로토콜임
- IP 프로토콜은 라우터와 종단 시스템 사이에서 송수신 되는 패킷 포맷을 기술함




<br/>  
<br/>  


References
[컴퓨터 네트워킹 하향식 접근 - James F.Kurose]
