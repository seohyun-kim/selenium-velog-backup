---
title: "[펌웨어] STM32F GPIO / Clock datasheet"
description: "RM0385 Reference manual STM32F75xxx and STM32F74xxx advanced Arm®-based 32-bit MCUs  GPIO  ![](https://velog.velcdn.com/images/selenium/post/b3d8361"
date: 2022-04-23T07:45:14.044Z
tags: []
---
## RM0385 Reference manual
### STM32F75xxx and STM32F74xxx advanced Arm®-based 32-bit MCUs
<br/>  
<br/>  


### LED 
| LED | GPIO |
|:---:|:---:|
| [SYS_USER_LED 1]  |  PG 12  |  
| [SYS_USER_LED 2]  |  PE  5  |
| [SYS_USER_LED 3]  |  PE  4  |
| [SYS_USER_LED 4]  |  PG 10  |
    
<br/>      


### < Base address >   ------ p.70  

| GPIO | Base Address | AHB|
|:---:|:---:|:---:|
|GPIO G | 0x4002 1800 | AHB 1 |
|GPIO E | 0x4002 1000 | AHB 1 |

    
<br/>  


 
### < Offset >  ------------ p.208-210

| 종류 | Offset | Option | Option | Option | Option |
|:---:|:---:|:---:|:---:|:---:|:---:|
|MODER | 0x00  | {00: input}  |{01: GP Output}|{10: Alternate Function} | {11: Analog}|
|OSPEEDR | 0x08   |  {00: Low Speed} | {01: Medium}   | {10: High}              |  {11: Very high}
|PUPDR   | 0x0C   |  {00: No}    |    {01: Pull-up}  | {10: Pull-down}       |    {11: Reserved}
|BSRR    | 0X18   |  {Bits 31:16 RESET}  | {Bits 15:0 SET}||


<br/>  
<br/>  

## GPIO
![](/images/968c011f-554a-4d62-8d79-169565c0540f-image.png)

<br/>  

![](/images/225df13e-4f7e-4693-bf7f-53821e81d31f-image.png)

<br/>  

![](/images/f7b51661-24d9-4f96-8a7e-3c7aabc166da-image.png)


<br/>  

![](/images/b3d83610-f37d-4429-ad7d-5f4468dd234b-image.png)

<br/>  

![](/images/a30d4211-642d-4189-a6f0-4605869004f1-image.png)

![](/images/bfedc2f2-d88b-45a3-8f84-23c9d3022a35-image.png)

<br/>  
<br/>  

## Clock

### AHB1

![](/images/ae3730fb-de95-4382-81aa-171e3d4d3470-image.png)

<br/>  
<br/>  

## Base Address
![](/images/f891c96f-607e-4fe5-9dfd-2ee797f9efb0-image.png)
