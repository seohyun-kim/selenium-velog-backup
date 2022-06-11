---
title: "[펌웨어] 임베디드 HW - 3편. Filter"
description: "저주파 성분만 통과시키는 역할 (고주파 성분은 걸러냄)C(캐패시터)의 저항이 무한대가 됨R저항과 무한대 저항이 직렬연결인 상태		\-> 저항의 크기가 매우 큼  \-> R에서 소모하는 전압이 거의 ❌ (상대적으로 너무 작으니까)C(캐패시터)는 AC입장에서 보면 shor"
date: 2022-04-20T08:19:56.091Z
tags: []
---

## LPF (Low Pass Filter)
![](/images/47a720fa-786d-4cb1-927b-d819276bfd4e-image.png)

- 저주파 성분만 통과시키는 역할 (고주파 성분은 걸러냄)


### (1). DC (저주파) 입장 (C 저항:무한대)

![](/images/da4847c6-3288-4412-b7bc-c99bc6e295cc-image.png)

- C(캐패시터)의 저항이 무한대가 됨
- R저항과 무한대 저항이 직렬연결인 상태
	-> 저항의 크기가 매우 큼
    -> R에서 소모하는 전압이 거의 ❌ (상대적으로 너무 작으니까)

<br/>  

### (2). AC (고주파) 입장 (C 저항: short)
![](/images/5a9588f1-de3d-49b7-bdf7-b65fee7f0a27-image.png)

- C(캐패시터)는 AC입장에서 보면 short나 다름 없음
- R에서 모든 전압강하가 일어나게 됨
- `DC`에 가까운 성분만 `Vout`으로 나오게 됨


<br/>  
<br/>  

## 예시

### (1). Low Pass Filter
![](/images/9feb1e60-9b91-4818-b6b3-240c6b780f7f-image.png)


- `L`은 `저주파`일수록 더 잘 통과 (DC)
- `C`은 `고주파`일수록 더 잘 통과 (AC)

![](/images/445c1fb8-4181-4a1b-b263-d9c708590c11-image.png)

![](/images/934de571-a7a0-485d-b6c5-a3231a18f143-image.png)

<br/>  
<br/>  



### (2). High Pass Filter
![](/images/49b8966d-34d7-479a-92a2-63fad5c3731f-image.png)

![](/images/0da6544e-ba57-4d51-873e-ad6a11aae965-image.png)

![](/images/6466aadd-3bcc-4d85-83fa-a4b9a7d2fa5f-image.png)

<br/>  
<br/>  

### (3). Band Pass Filter - 1

![](/images/bc6eb265-2bd9-4ac7-a785-f54ef2f2d594-image.png)

![](/images/cb1e3427-c0da-4364-8d2b-75ce73e3778b-image.png)

![](/images/5190be40-5914-4ec1-ad7d-f81812a45d33-image.png)
- 고주파 저주파 짤라내고 필요한 중간 부분만 걸러냄

<br/>  
<br/>  

### (4). Band Pass Filter - 2
![](/images/d641c7d9-b6b1-40c7-8032-c7c9cbdccf7e-image.png)

![](/images/612ed155-b671-4c15-a054-5e70f60829aa-image.png)


<br/>  
<br/>  

### (4). Band Stop Filter

![](/images/f5a9b99b-faaa-44d1-a26e-165ca62159f3-image.png)

- L와 C에 절묘하게 걸리는 일부 구간만 막는 역할

![](/images/cec27f98-99a6-46a2-9923-922baec9bed4-image.png)



