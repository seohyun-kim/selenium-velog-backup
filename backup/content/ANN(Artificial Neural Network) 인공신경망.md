---
title: "ANN(Artificial Neural Network): 인공신경망"
description: "인간의 뉴런이 연결된 형태를 수학적으로 모방한 모델ANN 뉴런은 여러 입력값을 받아서 일정 수준(threshold)이 넘어서면 활성화되어 출력값을 내보냄ANN은 뉴런들을 여러개 쌓아, 두개의 층(Layer)이상으로 구성되는 것이 특징가중치를 적용한 방향성 그래프은닉 계"
date: 2022-06-11T10:48:36.859Z
tags: []
---



> - 인간의 뉴런이 연결된 형태를 수학적으로 모방한 모델
> - ANN 뉴런은 여러 입력값을 받아서 일정 수준(threshold)이 넘어서면 활성화되어 출력값을 내보냄
> - ANN은 뉴런들을 여러개 쌓아, 두개의 층(Layer)이상으로 구성되는 것이 특징
> - 가중치를 적용한 방향성 그래프
> - 은닉 계층을 포함하는 인공신경망 기술

![](/images/afb74ec8-9c23-41cb-af2b-b19b187c27d0-image.png)


## 퍼셉트론 인공신경망

퍼셉트론(perceptron)은 가장 단순한 유형의 인공 신경망임
보통 이진 예측을 하는 데 쓰이며, 데이터를 선형적으로 분리할 수 있는 경우만 효과있음
![](/images/4dfc2d93-bc1e-4362-a18c-888c57a23d10-image.png)

<br/>


## ANN 동작
1. 입력 계층에서 입력된 데이터에 대해 가중치 행렬을 곱하여 은닉 계층으로 보냄
2. 은닉 계층 내부에서 활성화 함수를 통해 데이터 가공
3. 은닉 계층에서 나온 데이터를 새로운 가중치 행렬을 곱해 출력 계층으로 보냄
4. 출력을 위한 활성화 함수를 반영하여 결과를 출력
![](/images/dbfbf258-2e9e-4bdf-a987-7f793565355b-image.png)

<br/>

## 활성화 함수
> 입력 신호의 총합이 활성화를 일으키는지를 정하는 역할

### (1). Step Function(계단 함수)
- 0보다 작은 수는 0, 큰 수는 1
	![](/images/885a1e9f-7141-48d9-840e-e908f270064e-image.png)

### (2). Sigmoid Function(시그모이드 함수)
- 미세한 변화에 대한 값도 반영하기 위해 사용
	![](/images/9e704524-5e1e-4856-9e21-9d03c05dcf3a-image.png)

### (3). Rectifien Linear Unit Function(ReL U 함수)
- 0을 넘으면 그대로 출력, 0 이하일 땐 0을 출력
	![](/images/09827167-0241-4891-b4c4-779f44dcd1bf-image.png)

### (4). Softmax Function(소프트맥스 함수)
- 입력 받은 값을 0~1 사이의 값으로 정규화 하며 총합이 항상 1이 되는 특성을 가진 함수
- N개 이상의 class의 확률 분포를 계산할 때 사용
	

![](/images/9ad17326-0ea5-446c-a5a8-7e4955b2dede-image.png)

<br/>

## 무슨 문제점이 있는데?

### - 학습 과정에서 파라미터의 최적값을 찾기 어렵다
- 출력 값을 결정하는 활성화 함수의 사용은 기울기 값에 의해 가중치(weight)가 결정되었음
- 이런 기울기(gradient) 값이 뒤로갈수록 점점 작아져 0에 수렴하는 오류를 낳기도..
- 부분적인 에러를 최저 에러로 인식하여 더이상 학습을 하지 않기도...
- sigmoid 함수의 문제점 때문임 (local minima를 global minimum으로 인식)

### - Overfitting 문제

### - 학습 시간이 너무 느림
- 은닉 층이 많을 수록 그 연산량이 기하 급수적으로 늘어나게 됨..


> ▶️ ANN기법의 문제가 해결되면서, 은닉 층을 많이 늘려서 학습의 결과를 향상시키는 방법이 두등장!! , 그것이 바로 `DNN`
	





<br/>
<br/>


### ref
https://ebbnflow.tistory.com/119
https://www.youtube.com/watch?v=r7gWQicV-Yw
https://databricks.com/kr/glossary/artificial-neural-network
http://contents2.kocw.or.kr/KOCW/data/document/2020/edu1/bdu/hongseungwook1118/021.pdf
https://jjeongil.tistory.com/977
https://hororolol.tistory.com/187