---
title: "[펌웨어] 임베디드 HW - 4편. Transistor"
description: "트랜지스터의 기능 : 증폭(내 보내는 양을 결정) , switching(내보낼지말지)  트랜지스터는 저항의 값을 변경시킬 수 있음 -> 전류의 양 조절 가능B: Base 베이스에 있는 놈을C: Collector 콜렉터로 들어온 전류를E: Emitter 이미터로 내 보냄"
date: 2022-04-20T10:00:05.065Z
tags: []
---
# 트랜지스터(Transistor)

![](/images/7e50f943-5592-4482-939c-ab75e07e185b-image.png)

- 트랜지스터의 기능 : `증폭(내 보내는 양을 결정)` , `switching(내보낼지말지)`  
- 트랜지스터는 저항의 값을 변경시킬 수 있음 -> 전류의 양 조절 가능

<br/>  



### npn 형
![](/images/18d2a6bd-5796-4b6a-afea-c3076b874b60-image.png)

- B: Base 베이스에 있는 놈을
- C: Collector 콜렉터로 들어온 전류를
- E: Emitter 이미터로 내 보냄


<br/>  <br/>  
### 트랜지스터 영역
![](/images/555536e9-2e06-4dcb-8c50-bdc50468f033-image.png)

- 활성영역이 트랜지스터의 기본 역할

- 차단영역(OFF)과 포화 영역을 이용해 Switch On/Off 역할을 구현

- 너무 높거나(포화) 낮아도(차단) 전류 흐르지 ❌

<br/>  <br/>  
### 트랜지스터 기능
![](/images/8fe1ea75-1296-4a62-ae5d-4e033ece27ff-image.png)

>  #### [스위치 역할]
- B에 의해 Switch가 ON되면 C와 E사이에 전류가 흐름
- B는 트랜지스터가 동작하게하는 스위치 역할
- 차단 & 포화 영역을 이용하여 ON/OFF 표현

>  #### [증폭 기능]
- 활성영역에서 이용 가능
- 아주 작은 전압신호를 B에 흘려주면 CE간의 전류가 더 큰 폭으로 변화함  

<br/>  <br/>  

# Clip Activation

![](/images/987b96f2-d903-45b6-8e6d-7b978fac966c-image.png)

- 우리는 트랜지스터를 스위치 역할로 쓸 것임


> Active High : 1을 주었을 때 동작 (`CS` 라고 표기)
> Active low  : 0을 주었을 때 동작 (CS bar: `CS/`, `CS_N`, `CS*`)


- 보통의 대부분 칩셋들은 Active Low를 사용함  
    - 명시적으로 0을 준다는 것이 생각보다 쉽지 않음 -> 오류가 생겨 오동작 가능성
    - 회로가 Hatsh Environment에 있을 때 가끔 신호가 High 될 때가 있어 오동작 가능성
    - Low Active는 상대적으로 이런 변화에 둔감함
    



<br/>  <br/>  

# Pull up, Pull down



### Pull up
![](/images/c0bc543f-e6f6-4be6-885d-63d34dc7f084-image.png)
- 1번 스위치 ON 상태에서는 GND로 흐름
	Digital Chip에 0V가 인가됨 (큰 저항 느낌)  
    
- 2번으로 가서 스위치 OFF 될때는 작은 저항과 큰 저항이 직렬로 있는 회로가 됨
	남아있는 Voltage가 대부분 Chip으로 감 (3V에 가까운 전압)
    

<br/>  <br/>  

 
### Active High Pull down

![](/images/89b374b5-9256-4301-a2ce-ac9f49ff5c8f-image.png)


