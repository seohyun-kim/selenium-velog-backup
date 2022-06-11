---
title: "[딥러닝] GAN(Generative Adversarial Network) 이론"
description: "Generative Adversarial Network (적대적 심층 신경망)Generative : "그럴듯한 가짜"를 만드는 생성 모델Adversarial : 두 개의 모델을 적대적으로 경쟁시키며 발전함을 의미Network : 인공신경망(Artificial Neura"
date: 2022-05-26T16:36:08.838Z
tags: []
---

## GAN 이란?
> **G**enerative **A**dversarial **N**etwork (적대적 심층 신경망)
> 
> **Generative** : "그럴듯한 가짜"를 만드는 생성 모델
> **Adversarial** : 두 개의 모델을 적대적으로 경쟁시키며 발전함을 의미
> **Network** : 인공신경망(Artificial Neural Network)으로 만들어짐


딥러닝 모델 중 이미지 생성에 많이 쓰이는 모델.

실제로는 존재하지 않지만 **있을 법한 데이터를 만들어 내는 모델**을 의미.



<br/>  

### Generative Models

![](/images/cc6fbb1d-0951-49cc-b5aa-3b3d870c6394-image.png)

(ref : https://github.com/ndb796/Deep-Learning-Paper-Review-and-Practice/blob/master/lecture_notes/GAN.pdf)

생성 모델은 실존하지 않지만 있을 법한 데이터(이미지, 문장, 오디오, ...등) 을 생성할 수 있는 모델

- 분류모델과 생성모델 
  - `분류모델 (Discrimenative)` : 특정한 dicision boundary를 학습하는 형태
  - `생성모델 (Generative)` : 각 Class에 대해 적절한 분포를 학습하는 형태

- 여러 변수에 대해 joint 된 확률 분포의 형태로 통계적인 모델로 표현할 수 있음

- 새로운 데이터 인스턴스를 만들 수 있는 아키텍처

- 생성 모델의 목표
	: 이미지 데이터의 분포를 근사하는 모델 G를 만드는 것




<br/>  

### Adversarial - 위조 지폐범과 판별자
![](/images/1d47994d-56f9-4cfc-a7fb-b4d4542bc1cf-image.png)

(ref : https://dreamgonfly.github.io/blog/gan-explained/)

이 GAN 모델을 위조지폐범과 판별자에 빗대어 많이 설명을 한다.

위조 지폐범은 판별자를 속이기 위해 위조지폐 제조 기술을 발전시키고,
판별자는 이 범인을 잡기 위해 판별 기술을 발전시킨다.

상호 협력이 아니고 **서로 적대적(Adversarial)**으로 노력하다보면 결과적으로 같이 발전한다.

결과적으로는 모순, 1/2 상태로 도달하게 된다고 한다.


<br/>  


## 좀 더 자세하게 알아보자.

> GAN 은 생성자(generator)와 판별자(discriminator) 두 개의 네트워크를 활용한 생성 모델임.

<br/>  

### GAN 의 목적함수
![](/images/ffac9bba-854b-4588-9e7a-79e1f0360eef-image.png)

<br/>  


- V라는 목적함수에 대해 G(생성자)는 낮추려고(min) 하고, D(판별자)는 높이려고(max)함
	![](/images/ef7e4363-f5a1-437a-82d6-a8044228b44b-image.png)
    
<br/>  


- 원본 데이터(`Pdata`)에서 한 개의 데이터(`x`)를 sampling 한 뒤,
log를 취한 값의 기대값(평균)을 구함
  ![](/images/7909f4ac-bd81-4201-a190-3bc0baebe85c-image.png)

<br/>  

- 생성자는 Noise vector로부터 입력을 받아 새로운 이미지를 만드는데,
하나의 Noise를 sampling (`Pz(z)`) 한 뒤 생성자(G)에 넣어 가짜 이미지를 만들고,
가짜 이미지를 D에 넣은 값을 1에서 뺀 뒤 log을 취해 기대값을 구함
	![](/images/651c7da4-2bdf-43e6-80a0-1e8f3b3cf43e-image.png)

<br/>  



- 생성자(G)는 Noise Vector(z)를 받아 새로운 instance를 만들고,
판별자(D)는 x 데이터를 받아 얼마나 진짜 같은지 출력으로 내보냄
( 진짜 이미지는 1, 가짜 이미지는 0 을 부여하는 방식으로 학습하고,
결과 값은 1~0 사이의 확률로 표현됨 ) 
  ![](/images/7874373e-32b2-465a-a129-530e720e929e-image.png)

<br/>  


> - 판별자(D)는 원본 데이터에 대해서는 1을 출력할 수 있도록 학습을 하고(왼쪽 식), 가짜 이미지에 대해서는 0을 출력할 수 있도록(오른쪽 식) 함 
> 
>
> - 생성자(G)는 왼쪽 식은 원본 데이터가 없어 상수로 취급되고, 오른 쪽 식을 minimize 하기 때문에 자신이 만든 가짜 instance를 진짜(1)로 출력할 수 있도록 학습을 진행

<br/>  

<br/>  





### Ref
https://dreamgonfly.github.io/blog/gan-explained/
https://ysbsb.github.io/gan/2020/06/17/GAN-newbie-guide.html
https://www.youtube.com/watch?v=AVvlDmhHgC4
https://www.youtube.com/watch?v=N9ewzLUZhL8

Generative Adversarial Nets 논문 : https://proceedings.neurips.cc/paper/2014/file/5ca3e9b122f61f8f06494c97b1afccf3-Paper.pdf