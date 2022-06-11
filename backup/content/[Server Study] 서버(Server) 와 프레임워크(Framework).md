---
title: "[Server Study] 서버(Server) 와 프레임워크(Framework)"
description: "클라이언트에게 네트워크를 통해 정보나 서비스를 제공하는 컴퓨터 시스템컴퓨터 프로그램 또는 장치(device)를 의미클라이언트의 요청에 의해 서비스를 제공설계목적에서 차이가 있음일반 컴퓨터 :일반 사용자를 위해 상대적으로 작은 크기의 하드웨어에서도 원활한 그래픽, 사운드"
date: 2022-05-08T08:34:22.241Z
tags: []
---
## 서버란?
- 클라이언트에게 네트워크를 통해 정보나 **서비스를 제공**하는 컴퓨터 시스템
- 컴퓨터 프로그램 또는 장치(device)를 의미
- 클라이언트의 요청에 의해 서비스를 제공
![](/images/109b0a35-d7e0-411c-b54b-86047d58adb8-image.png)


<br/>  


## 서버 vs 일반 컴퓨터(desktop)
`설계목적`에서 차이가 있음

- 일반 컴퓨터 :
    - 일반 사용자를 위해 상대적으로 작은 크기의 하드웨어에서도 원활한 그래픽, 사운드를 가진 멀티미디어 환경을 제공

- 서버 컴퓨터 : 
    - 언제, 어디서나 접속할 수 있는 365일 중단없는 서비스를 제공하기 위해 **신뢰성**에 초점이 맞춰져 있음
    - **대용량의 데이터**를 빠르게 처리하기 위한 컴퓨팅 능력을 제공하는 데 목적이 있기도 함
    

<br/>  

## 서버의 종류(하드웨어 개발환경)
  #### 1. 웹 서버(Web Server)  

  - 웹 브라우저의 요청을 받아 HTTP를 통해 웹 브라우저에서 요청하는 HTML 문서나 오브젝트(이미지 파일 등)를 전송해주는 서버
  - ex) Apache HTTP Server, Google Web Server,..

<br/>  

  #### 2. 웹 애플리케이션 서버(Web Application Server) 
  - 사용자에게 동적 서비스를 제공하기 위해 웹 서버로부터 요청을 받아 데이터 처리를 수행하거나, 웹 서버와 데이터베이스 서버 또는 웹 서버와 파일 서버 사이에서 인터페이스 역할을 수행하는 서버
  - ex) Apache Tomcat, Oracle WebLogic,..

<br/>  

  #### 3. 데이터베이스 서버(Database Server)
  - 데이터베이스와 이를 관리하는 DMBS를 운영하는 서버
  - ex) MySQL Server, Oracle Server,..

<br/>  

  #### 4. 파일 서버(File Server)
  - 파일 저장 하드웨어로 물리 저장 장치를 활용한 서버
  - 대용량 HDD, SSD 등의 장치가 존재
  - ex) AWS S3, ..



<br/>  

## 프레임워크(Framework)?
- 프레임워크는 소프트웨어의 구체적인 부분에 해당하는 설계와 구현을 재사용이 가능하게끔 일련의 협업화된 형태로 클래스들을 제공하는 틀
- 모듈화(Modularity), 재사용성(Reusability), 확장성(Extensibility), 제어의 역행(Inversion of Control) 의 특징이 있음

<br/>  

## 서버 개발 프레임워크
- 서버 프로그램을 개발할 때 네트워크의 설정이나 다양한 요청 및 응답 처리를 쉽게할 수 있도록 인터페이스나 클래스를 제공하는 소프트웨어
- 주로 MVC(Model-View-Controller) 디자인 패턴이 많이 사용됨

<br/>  

서버 개발 프레임 워크 종류 (대표 3가지)

### 1. [Spring](https://velog.io/@selenium/Server-Study-스프링Spring-스프링-부트Spring-Boot)
![](/images/38b6d5ee-fe79-4e76-bfe7-829cbc0d617d-image.png)

- `Java`를 기반으로 만들어진 오픈소스 프레임워크로, 여러가지 서비스를 제공하여 동적인 웹사이트를 개발하는 데 도움을 줌
- 대한민국 전자정부 표준 프레임워크로 사용됨
- 라이브러리가 많고 확장성이 높음

<br/>  

### 2. [Node.js](https://velog.io/@selenium/Server-Study-Node.js-노드)
![](/images/637ffe1b-e58d-4b6a-acc5-d18d61180783-image.png)

- `Javascript`를 기반으로 만들어진 소프트웨어 플랫폼
- Non-Blocking(비동기) I/O와 단일 스레드 [이벤트 루프](https://velog.io/@selenium/Node.js-1.-알아-두어야-할-자바스크립트Javascript)를 통한 높은 처리 성능을 가짐


<br/>  

### 3. Django(장고)
![](/images/67666f32-cd72-4d47-a191-4500579e1145-image.png)

- `python`을 기반으로 작성된 오픈소스 프레임워크
- 복잡한 데이터베이스 기반의 웹 사이트를 개발하는 데 있어서 효율적으로 작동하고자 하는 것이 Django의 주된 목적임
- 편리한 admin 패널과 풀스택 개발에 강점을 가지고 있음
- 상대적으로 느린 속도, 무거움.. 유연성이 낮음
<br/>  
<br/>  

References
https://library.gabia.com/contents/infrahosting/794/
https://ko.wikipedia.org/wiki/Node.js
https://iwuooh.com/
https://tech.toktokhan.dev/2021/06/28/python-web-framework/
