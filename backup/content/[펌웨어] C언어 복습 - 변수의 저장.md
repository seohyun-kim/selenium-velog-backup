---
title: "[펌웨어] C언어 복습 - 변수의 저장"
description: "  "abcde" 는 텍스트 영역에 저장됨  (상수)  stack 영역  주소값을 봐라 &c1 : 0x005dfd48 {0x00bf7b38 "abcde"} &c2 : 0x005dfd3c {0x00bf7b38 "abcde"}  ⭕ c1 : 0x00797b38 | c2 :"
date: 2022-04-20T15:54:11.619Z
tags: []
---





## 코드를 분석해보자!

```c
char* c1 = "abcde";
char* c2 = "abcde";
```

<br/>  

#### 1. "abcde" 는 어디에 저장되었을까?
>   "abcde" 는 텍스트 영역에 저장됨  (상수)

#### 2. c1, c2 는 어디에 만들어 졌을까?
>   stack 영역

#### 3. c1, c2 가 만들어는 졌을까?
>   주소값을 봐라
 &c1 : 0x005dfd48 {0x00bf7b38 "abcde"}
 &c2 : 0x005dfd3c {0x00bf7b38 "abcde"}

#### 4. "abcde"가 하나만 만들어 졌을까? ( = c1과 c2의 값은 동일할까?)
>   ⭕
 c1 : 0x00797b38 | c2 : 0x00797b38


<br/>  


#### 5. 이렇게 하면 어떻게 될까?
```c
char* c1 = "abcde";

char* c3;
c3 = c1;

c3[2] = 'X'; // 쓰기 엑세스 위반
```
> 바뀌겠냐? 
쓰기 엑세스 위반  


<br/>  
<br/>  


## Static 변수
```c
#include <stdio.h>

int d() {
	static int n = 0;

	int x;
	int y = 0;

	n++;
	printf("n=%d\n", n);
	/*
		static 변수 값이 유지될까?
		함수가 종료되어도 n값이 이전 값으로 유지되고 있음!
	*/
}

int main() {

	for (int i = 0; i < 3 ; i++)
	{
		d();
	}
}
```

> Static 변수는 함수가 종료되어도 n값이 이전 값으로 유지되고 있음!!!