---
title: "[펌웨어] 임베디드 HW - 5편. Pull up, Pull down"
description: "항상 High 상태를 유지해야하는 전자회로에 사용Switch 눌렀을 때 Low 입력풀업 저항의 주 목적 : Switch OFF될 때 HighPull up 저항 쓰는 이유풀업 저항을 통해 전원 +5V로 연결이 되기 때문에 스위치가 OFF 되어도 입력 값의 혼동이 X풀업 "
date: 2022-04-20T12:25:21.627Z
tags: []
---

# Pull up, Pull down 저항

<br/>  

## Pull up
![](/images/9f9a1c55-5285-4e33-a09c-6268ae5a464c-image.png)

> - 항상 High 상태를 유지해야하는 전자회로에 사용
> - Switch 눌렀을 때 Low 입력

- 풀업 저항의 주 목적 : Switch OFF될 때 High

- Pull up 저항 쓰는 이유
    - 풀업 저항을 통해 전원 +5V로 연결이 되기 때문에 스위치가 OFF 되어도 입력 값의 혼동이 X
    - 풀업 저항이 없으면, Switching시 과도한 전류가 흐를 가능성이 많음
    ( -> 디바이스에 안좋은 영향 )
    - 풀업 저항이 없으면, floating 상태에 있게 된다.
    	0 도 아니고 1도 아닌 상태로 제어가 불가능함

<br/>  

## Pull down 
- 논리적인 Low 상태를 유지시키기 위해서는 풀다운 저항을 사용

![](/images/a647a600-5792-452a-868e-02ca389fe2c3-image.png)


<br/>  

## Transistor를 사용한 Pull up system 구현
![](/images/92c4136a-44d4-4d15-bad9-de85018d43a1-image.png)

- Rc는 풀업을 위한 저항  

- R1은 TR이 ON 될 만큼의 전압 이하에서는 정확하게 0V가 되도록 하여 Base로 전류 흐르지 못하게 함  

- Master chip이 `0`을 주면 Transistor 은 `OFF`
	-> Vcc에서 주는 값은 모두 Slave chip으로 흐르게 됨 `(1 = High)`
    
- Master chip이 `1`을 주면 Transistor 은 `ON` 
 	-> Tr의 Collector로 가는 부분은 Slave chip에 비해 저항이 아주 작으므로(접지로 가니까) Vcc는 다 GND로 가버림
    -> Slave chip으로 들어가는 값은 `0v (Low)`
    
| Master | Tr | Slave |
|:----------:|:----------:|:----------:|
| 0 | OFF | 1 |
| 1 | ON  | 0 |




<br/>  


## Open Collector

- Switch 가 Master chip 내부에 들어가있는 경우

![](/images/8e5f48fd-9e76-44cf-ab53-7cd736ccf66c-image.png)

![](/images/1360c277-f3ea-420e-84e2-8fab98f82bfb-image.png)
- 인버터를 줌으로써 반전을 시켜 0이 들어오면 0이 되도록 함

- master 1에 Low를 주면 반전이 되어 High가 됨으로써 TR이 활성화가 되어 Vcc에 있던 전류가 TR과 연결되어있는 GND로 다 빠져나감
	master 2에 무엇을 주는 지에 관계없이 Slave는 Low(0)

- Master 1에 High를 주면 반전이 되어 Low가 들어가게 됨
	이때 트랜지스터는 OFF상태로 Vcc의 전류가 아래로 흐르게 된다.
    이때 Master 2에 주는 인풋에 따라 상태가 달라진다.
    1. Master2 에 0을 주면 반전이 되어 1, 트랜지스터 ON GND로 다 빠져나감
    2. Master2 에 1을 주면 반전이 되어 0, 트랜지스터 OFF -> Slave에 1이 들어감


(각 Master의 최초 입력을 의미함)

| Master 1 | Master 2 | Slave |
|:----------:|:----------:|:----------:|
| 0 |  0 or 1 | 0 |
| 1 | 0  | 0 |
| 1 | 1  | 1 |


> Invert 됨에 주의하라
> AND 연산과 결과 동일함