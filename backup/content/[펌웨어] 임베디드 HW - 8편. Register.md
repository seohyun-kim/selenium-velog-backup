---
title: "[펌웨어] 임베디드 HW - 8편. Register"
description: "Flip-flop은 NOR gate 2개의 output을 서로의 input으로 feedback 하는 형태 Write = 0 이면 R, S값은 둘다 0이 됨 ( 둘 다 AND니까 ) Write = 1 이면 S는 Data in 값이 그대로 나가게 됨R는 Data in 값이"
date: 2022-04-20T14:59:15.521Z
tags: []
---

## Register의 동작
![](/images/d2ad1abd-76b5-44a6-9ec3-caee739da6d1-image.png)

- Flip-flop은 NOR gate 2개의 output을 서로의 input으로 feedback 하는 형태


> ** Write = 0 이면 **
- R, S값은 둘다 0이 됨 ( 둘 다 AND니까 )

> ** Write = 1 이면 **
- S는 Data in 값이 그대로 나가게 됨
- R는 Data in 값이 **반대로** 나가게 됨 (인버터 붙었으니까)

<br/>  
<br/>  

### Write = True로 가정/ S : Set, R : Reset이라 생각해보자

![](/images/91e0577d-2e27-46c0-ac0f-150786afeacf-image.png)


- NOR 진리표  

|NOR| 0 | 1 |
|:---:|:---:|:---:|
| **0** | 1 | 0 |
| **1** | 0 | 0 |


> ** R = 1 이고, S = 0 이면 **
- NOR gate의 output(`Q`)은 무조건 0이 됨
- Reset 값을 주었더니 Data는 0으로 reset이 됨!

> ** R = 0 이고, S = 1 이면 **
- S = 1 이므로, NOR gate의 output(`Q/`)은 무조건 0이 됨
	다시 `Q/` 의 값이 R과의 NOR의 Input으로 들어가 `Q`는 항상 1이 됨
- Set 값을 주었더니 Data는 1으로 set이 됨!


> ### 즉, Write = 1 일 때는 Data에 새로운 값을 쓰는 역할

<br/>  
<br/>  

### Write = False 일 때를 생각해보자
![](/images/91e0577d-2e27-46c0-ac0f-150786afeacf-image.png)

>- WRITE = 0 이면 Reset과 Set이 둘다 0이 됨! -> 따질 것은 `Q`밖에 없음
> 
> **  Q = 0 일 때 (이전 값이 0)**
> 	- `S` 와 같이 들어가는 input이 0 
	- 즉, `Q/` = 1
	- `R` 와 같이 들어가는 input이 1
    -  즉, `Q` = 0
    - **원래 있던 데이터 값이 (data in에 관계없이) 유지가 됨!!!**
>
> **  Q = 1 일 때 (이전 값이 1)**
> 	- `S` 와 같이 들어가는 input이 1
	- 즉, `Q/` = 0
	- `R` 와 같이 들어가는 input이 0
    -  즉, `Q` = 1
    - **원래 있던 데이터 값이 (data in에 관계없이) 유지가 됨!!!**

> ### 즉, Write = 0 일때는 다음 input이 들어올 때까지 Data out을 유지! (메모리 기능)


<br/>  
<br/>  

## Latch

![](/images/2a24491c-cfb3-4e35-957d-a3e87460dded-image.png)

![](/images/6c573a5a-a68c-43fc-8820-7d1292ff71dd-image.png)

### 1 bit Level Trigger Latch
![](/images/748a049f-577a-4a56-8775-f57af94ea381-image.png)

- W가 High를 유지할 때, Write 가능
- 그 외에는 값을 기억하고 있음


<br/>  

### 1 bit Edge Trigger Latch
![](/images/ba517bdc-541d-4f63-9c80-b06ddddb3f76-image.png)

- Edge Trigger는 RS Flip-flop 2개를 묶어서 만듦
- Clock 이 올라가는 순간이나 내려가는 순간에만 Write가 가능함
- 아닌 순간은 그 전의 write된 값을 기억

<br/>  

### n bit Register  
![](/images/f218f53f-5491-453f-90b9-ecf112b7f955-image.png)

- 레지스터는 Latch 여러개를 엮어 n-bit로 만든 전체를 의미함
- 레지스터의 사용  
    - I/O의 용도
    - CPU Core 내의 임시 저장 공간 (훨씬 빠른 속도로 사용 가능)


<br/>  
<br/>  

## Register의 종류
![](/images/6e828514-198c-42a6-a2bd-c91121a55333-image.png)

<br/>  
<br/>  

## Clock
![](/images/f42c0a27-aa41-4d38-a685-86048ab6b7da-image.png)

- 모든 Digital 회로는 Clock에 의해 동작 결정

- Clock = 1일 때 Data 값 조작 
- Clock = 0일 때 기존 값을 유지
