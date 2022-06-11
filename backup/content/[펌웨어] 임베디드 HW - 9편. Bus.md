---
title: "[펌웨어] 임베디드 HW - 9편. Bus"
description: "Bus : 장치들이 정보 공유를 위해서 공유하는 선들의 집합특정 한 시점에서는 Bus 위에서 그 당시에 Bus를 쓸 수 있는 허가받은 장치들의 신호만 보임S0, S1 은 Mux Control 신호결국 4개 중 한개의 Reg를 선택해서 사용하게 됨Bus의 종류gate에서"
date: 2022-04-20T15:34:35.889Z
tags: []
---

## Bus Transfer Mechanism  

![](/images/423f2183-9f60-48aa-a2da-6e5c3a13df7a-image.png)

- **Bus** : 장치들이 정보 공유를 위해서 공유하는 선들의 집합
- 특정 한 시점에서는 Bus 위에서 그 당시에 Bus를 쓸 수 있는 허가받은 장치들의 신호만 보임
- S0, S1 은 Mux Control 신호

![](/images/0d6f4951-0d5b-45f0-bdad-2fca9aab9d4e-image.png)

- 결국 4개 중 한개의 Reg를 선택해서 사용하게 됨

<br/>  

- Bus의 종류
  ![](/images/42fab240-44b3-4a64-a0f0-e74094aac9a9-image.png)

<br/>  
<br/>  

## Timing
![](/images/c42f1e13-219c-4d18-b883-f516512fee37-image.png)

> gate에서 입력신호가 들어온 후 출력신호가 나오기 까지의 시간을 `전달지연시간`

![](/images/506627fa-359d-4474-9d78-038e845499a7-image.png)

- 상승/하강 시간 동안의 기다리는 시간이 필요하게 됨

<br/>  

- 타이밍의 표현
  ![](/images/4d8b623e-9246-44cf-ad12-51182d18908e-image.png)


- Timing Diagram 예시
	![](/images/b7ac3890-2947-4a9a-8a4b-09a27e884ed3-image.png)


<br/>  
<br/>  

## Memory
![](/images/dfc5d13c-51c8-4822-96f9-67877ce3b770-image.png)

- XIP : Execution In Place 
	메모리 상에서 직접 Program/Code 를 실행할 수 있는 기술

<br/>  

![](/images/d3e38a41-8746-4382-bf2f-3ed072922167-image.png)

- Embedded 에서는 쓰고 지울 수 있는 Flash 메모리가 ROM으로 많이 사용됨

- Flash 메모리에는 2가지 종류가 있음
	1. **NOR** 
    	- cell이 병렬로 연결
        - Address line과 Data line을 모두 가질 수 있음
        - RAM 처럼 Byte 단위로 Random Access가 가능
        - NOR형은 읽기가 빠르다는 특성이 있지만 대용량 데이터를 저장하는 데에는 한계가 있음
     <br/>     
	2. **NAND** 
    	- 읽기는 느리지만
        - 쓰기와 지우기가 빠름
        - 대용량 저장이 가능함
		- Byte 단위로 Random Access는 안됨


![](/images/b2d868bd-8c36-4292-b80d-f7ab2f0d32a5-image.png)

![](/images/bc8c7690-892a-4451-be7d-f0c712af1902-image.png)

![](/images/66a755b6-94a0-4792-b9fc-408628612cb1-image.png)



<br/>  
<br/>  

## CPU
![](/images/c87417ac-5d1f-4a0d-97ff-153895668b91-image.png)


![](/images/3b652289-d949-4e9b-8d5f-48f8c1fdd3ab-image.png)


- CU : 전기적인 신호에 대해 반응하는 논리회로 집합 (다른 Unit에 동작 지시)
- Decoder : 명령어 해석
- ALU : 계산을 하는 논리회로의 집합


![](/images/989def35-85c9-4d15-b7f0-857483bec468-image.png)


![](/images/1efabc84-6a01-47ac-8a98-d782f2099261-image.png)


![](/images/30a3c4fb-82c4-40dc-a08c-aa8545052b5f-image.png)


![](/images/83bb4cdb-cce5-4e9c-b1b8-88dc29d41744-image.png)


![](/images/aeba8d3c-59a7-4626-9dc6-c2d2b764e82c-image.png)

<br/>  

### CPU 동작  

![](/images/f6561862-2a47-44e6-94b9-cd22da2b74d4-image.png)
> ** 1. Fetch **
> - Memory에서 명령 가져옴 
> - Bus 사용
> 
> ** 2. Decode **
> - 명령 해석해서 어떤 일 하는지 알아보고 Register값 확인
> - CPU 내에서만 돌아감
> 
> ** 3. Execution **
> - 명령을 처리(실행)
> - CPU 내에서만 돌아가고 그 결과가 다시 저장될 수 있음

![](/images/8a38058a-29ae-4bb2-865c-d7af84f5a24a-image.png)

- Fetch Decode Execution 각각 독립적으로 구성되어있음


![](/images/3cfc6e56-b9be-4dd7-94ac-466e8dc3f503-image.png)
