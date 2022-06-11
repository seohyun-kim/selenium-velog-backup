---
title: "[펌웨어] 임베디드 HW - 6편. RLC 논리회로"
description: "C는 AC인 고주파만 통과시켜 3V 전원이 인가될 때 AC성분을 GND로 흐르게 하여 Chip에 들어가는 것을 차단전원 켜질 때 Transition으로 생기는 noise를 제거하기 위함 (= Ripple 제거)(Ripple = 작은 양의 DC 흔들림으로 AC성분임)3V"
date: 2022-04-20T13:38:32.569Z
tags: []
---
# RLC와 Chip

## C(캐패시터)만 있는 경우

![](/images/108105d1-6172-4e34-8c62-ae82578c89f7-image.png)

- C는 AC인 고주파만 통과시켜 3V 전원이 인가될 때 AC성분을 GND로 흐르게 하여 Chip에 들어가는 것을 차단

- 전원 켜질 때 Transition으로 생기는 noise를 제거하기 위함 (= `Ripple` 제거)
  (Ripple = 작은 양의 DC 흔들림으로 AC성분임)


<br/>  

## L,C의 예
![](/images/6d2931c6-118c-4c84-9dc0-853d79b82fef-image.png)


- 3V Vcc의 DC성분을 VCC_IN에 안정적으로 공급하기 위함 (`Ripple`제거)

- R대신 L을 넣은 경우는 보통 그냥 코일이 아니라, bead 라고 부르는 inductor의 한 종류를 삽입, 이 bead의 특성이 특정 주파수에서 저항역할과 주파수를 없애는 역할을 같이함
	=> L을 넣어 AC와 특정 주파수를 없애는 장점
    	( L대신 저항을 달면 전압이 다소 떨어지는 문제)
        
<br/>  

### Bypass Capacitor (Decoupling Capacitor)
![](/images/e3f03a8a-0412-4d9b-924f-84cb6f7256fa-image.png)

> Bypass Capacitor : 깨끗한 직류성분만 남겨서 Chip쪽으로 보냄
> Decoupling Capacitor : 제거  

<br/>  
<br/>  

## 논리회로로의 확장
![](/images/0e4e1ea0-87a5-4d16-b9e0-4fd209c04e57-image.png)

![](/images/cac264da-b359-431b-bb36-c4a474258f8b-image.png)

![](/images/f3a58805-93ad-40f3-bc7a-35de0c57db80-image.png)

![](/images/53d584be-9560-44bf-b5b5-4e71f5d287cc-image.png)


### 1. AND
![](/images/ac4b5f54-9967-47bf-be48-0518632617f1-image.png)

- 둘 다 1이어야 y도 흐름

<br/>  

### 2. OR
![](/images/1db85899-a0f4-4aa5-8d8a-7b66b2c7db57-image.png)

- 둘 중 하나라도 1이 있으면 y도 1

<br/>  

### 3. Inverter 회로
![](/images/00da3027-318c-48b7-bda9-31d0983eeca7-image.png)

- A의 값을 뒤집어 y의 값을 만들어 냄
- A가 1이 되면 TR이 ON이 되어 Vcc에서 전류가 상대적으로 저항이 적은 TR쪽으로 빠져버림 -> Y는 0이 됨

<br/>  

### 4. 버퍼

- A의 값을 그대로 Y에 내보냄
- 시간지연을 보정해주는 역할을 함
- 회로의 나가는 선들의 타이밍을 맞춰줌

