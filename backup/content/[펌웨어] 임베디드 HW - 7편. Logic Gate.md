---
title: "[펌웨어] 임베디드 HW - 7편. Logic Gate"
description: "2bit Binary Adder 하위 1bit : 각각 OR연산하고 NAND연산한 결과를 다시 AND연산 한 값상위 1bit(Carry) : 그냥 AND연산의 결과 버퍼를 붙여주면 연산 타이밍을 맞출 수 있음카르노 맵을 구사하면 간단하게 input에 대한 output을"
date: 2022-04-20T14:13:18.887Z
tags: []
---

## Logic Gate 예제  

![](/images/15934b7a-62c5-44f3-b931-a24e71986ebb-image.png)

![](/images/bf6583b7-dfa8-4294-b8f0-3936485c5850-image.png)

- 2bit Binary Adder 
    - 하위 1bit : 각각 OR연산하고 NAND연산한 결과를 다시 AND연산 한 값
    - 상위 1bit(Carry) : 그냥 AND연산의 결과 
    
- 버퍼를 붙여주면 연산 타이밍을 맞출 수 있음

- 카르노 맵을 구사하면 간단하게 input에 대한 output을 만들어내는 논리조합을 만들 수 있음
    - 2bit Adder의 하위 1bit는 `XOR`와 동일

<br/>  
<br/>  

## 회로의 구성

![](/images/c473a0fd-a76b-4841-bd01-35499df2df60-image.png)

- 좌: 아날로그 회로
- 우: 디지털 회로 소자

<br/>  

## IC

![](/images/aee04443-fe0f-44bf-bfd4-f5963b071cc8-image.png)

- U -> IC unit
- L -> 인덕터
- C -> 캐패시터  

- CLK : 클럭을 의미
	- 삼각형은 엣지(올라가는/내려가는 순간)를 의미
    	![](/images/98438e4e-230e-4846-9997-a7181c97bf9f-image.png)


<br/>  

## Register

![](/images/ebb440a1-aae8-4c7d-b890-de93f1dfa8ae-image.png)

- Register는 Flip-flop의 집합
- Latch는 1bit를 기억할 수 있는 소자를 통칭함
- n-bit Register는 n개의 Latch로 이루어짐

