---
title: "[Server Study] 스프링(Spring), 스프링 부트(Spring Boot)"
description: "서버 개발을 쉽게 할 수 있게 하기 위해 프레임워크 를 사용하게 된다.이 중, 대한민국 전자정부 표준으로 사용하고있는 스프링에 대해 다루고자 한다.스프링 공식 페이지스프링은 자바 기반의 웹 어플리케이션을 만들 수 있는 프레임 워크동적인 웹 사이트 개발을 위한 여러 서비"
date: 2022-05-08T09:18:21.541Z
tags: []
---

서버 개발을 쉽게 할 수 있게 하기 위해 [프레임워크](https://velog.io/@selenium/Server-Study-서버Server-와-프레임워크Framework) 를 사용하게 된다.

이 중, 대한민국 전자정부 표준으로 사용하고있는 스프링에 대해 다루고자 한다.

<br/>  

## 스프링(Spring)?
![](/images/29acd324-3569-4106-8618-c9ff66ceae7d-image.png)

- [스프링 공식 페이지](https://spring.io/)

- 스프링은 자바 기반의 웹 어플리케이션을 만들 수 있는 프레임 워크
- 동적인 웹 사이트 개발을 위한 여러 서비스를 제공
- 기존의 EJB(Enterprise Java Bean) 시절을 겨울에 빗대어 새로운 봄이 왔다는 의미라고 한다 ㅎㅎ

<br/>  

## 스프링 프레임워크의 특징
- 경량 컨테이너로서 자바 객체를 직접 관리

- [Plain Old Java Object(POJO)](https://ko.wikipedia.org/wiki/Plain_Old_Java_Object) 방식의 프레임워크로 기존에 존재하는 라이브러리 등을 지원하기에 용이하고 객체가 가벼움
- `IoC(Inversion of Control, 제어반전)`을 지원함. 컨트롤의 제어권이 사용자가 아니라 프레임워크에 있어서 필요에 따라 **스프링에서 사용자의 코드를 호출**함
    - 스프링 프레임워크의 심장부라고 할 수 있음...!
    - IoC 컨테이너는 POJO 를 구성하고 관리함
- `DI(Dependency Injection, 의존성 주입)`을 지원함. 각 계층이나 서비스들 간에 의존성이 존재할 경우 **프레임워크가 서로 연결**시켜줌
- `AOP(Aspect-Oriented Programming, 관점 지향 프로그래밍)`을 지원함. 여러 모듈에서 공통적으로 사용하는 기능의 경우 해당 **기능을 분리하여 관리**할 수 있음
- MVC(Model-View-Controller) 패턴을 사용
- 확장성이 높음


<br/>  

## 그래서 Spring Boot는 뭔데?
![](/images/b7583e57-fa6f-4803-8053-4a672eceeafc-image.png)

- 스프링을 쉽게 이용하기 위한 도구 (Spring의 많은 부분을 자동화 함)
- 단독 실행 가능한 스프링 애플리케이션을 생성
- 최소한의 초기 스프링 구성으로 빠르게 시작할 수 있도록 설계됨
- 웹 컨테이너를 내장하고 있어 최소한의 설정으로 쉽게 웹 어플리케이션을 만들 수 있음

<br/>  



<br/>  

<br/>  

References
https://spring.io/projects/spring-framework
https://spring.io/projects/spring-boot
https://goddaehee.tistory.com/238
http://melonicedlatte.com/2021/07/11/174700.html
[스프링 5 레시피(4판) - 마틴 데니엄 외]

