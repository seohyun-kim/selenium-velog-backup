---
title: "[Server Study] 스터디 주제 선정(Study Topics)"
description: "Node.js만 해왔던 사람으로서, Spring boot 스터디원까지 포괄하는 스터디 주제를 선정하는 것에 대한 어려움이 있었습니다..잘 굴러갈 수 있을 지 아직도 확신이 들진 않네요!진행해 보면서 피드백을 반영하고 유동적으로 변하며 더 좋은 방향을 찾아 갔으면 좋겠습"
date: 2022-05-07T05:15:14.987Z
tags: []
---
Node.js만 해왔던 사람으로서, 
Spring boot 스터디원까지 포괄하는 스터디 주제를 선정하는 것에 대한 어려움이 있었습니다..

잘 굴러갈 수 있을 지 아직도 확신이 들진 않네요!

진행해 보면서 피드백을 반영하고 유동적으로 변하며 더 좋은 방향을 찾아 갔으면 좋겠습니다 😊

<br/>  



> 💡 주제는 진행 상황을 고려하여 언제든지 바뀔 수 있습니다.
> 주제 선정에 대한 피드백은 언제나 환영합니다 :)

<br/>  
<br/>  



## 1 주차 : Basic

---

- [서버의 개념, 구조와 전반적인 동작 과정](https://velog.io/@selenium/Server-Study-%EC%84%9C%EB%B2%84Server-%EC%99%80-%ED%94%84%EB%A0%88%EC%9E%84%EC%9B%8C%ED%81%ACFramework#%EC%84%9C%EB%B2%84%EB%9E%80)

- [서버 개발 프레임 워크 종류와 특징](https://velog.io/@selenium/Server-Study-%EC%84%9C%EB%B2%84Server-%EC%99%80-%ED%94%84%EB%A0%88%EC%9E%84%EC%9B%8C%ED%81%ACFramework#%EC%84%9C%EB%B2%84-%EA%B0%9C%EB%B0%9C-%ED%94%84%EB%A0%88%EC%9E%84%EC%9B%8C%ED%81%AC)
- [네트워크](https://velog.io/@selenium/Server-Study-%EC%9D%B8%ED%84%B0%EB%84%B7Internet%EA%B3%BC-%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%ACNetwork)
    - [OSI 7계층](https://velog.io/@selenium/Server-Study-%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%ACNetwork%EC%99%80-%ED%94%84%EB%A1%9C%ED%86%A0%EC%BD%9CProtocol-OSI-7-%EA%B3%84%EC%B8%B5Layer)
    - 프로토콜 종류별 특징([TCP, UDP](https://velog.io/@selenium/Server-Study-TCP%EC%99%80-UDP), HTTP, ...)
    - [3-Way Hand shake](https://velog.io/@selenium/Server-Study-TCP%EC%99%80-UDP#hand-shake%EA%B0%80-%EB%AD%90%EA%B8%B8%EB%9E%98)
- [Restful API](https://velog.io/@selenium/Server-Study-REST-%EC%99%80-HTTP-RESTful-API)
- 아키텍처 패턴 ( MVC, Client-Server, ..)
- SQL - `DML`, `DDL`, `DCL`
- 관계형 데이터베이스(1:1, 1:N, N:M)
- ORM
<br/>  


## 2 주차 : Database

---
- JDBC API를 이용한 데이터베이스 연동
- JPA를 이용해 DB생성, Entity, Repository 작성
- Node.js ↔ Sequelize 연동, DB생성, Entity, Repository 작성


<br/>  


## 3 주차: 이미지 업/다운로드

---

- **이미지 업/다운로드 (4 주차)**
    - API
        - `POST` : 이미지 업로드
        - `GET`   : 이미지 다운로드


<br/>  

## 4 주차 : **Log in**

---

- Session, Cookie 의 특징과 차이
- Access Tocken 이용한 인증(JWT)
- Oauth 원리와 과정
- 로그인 API 구현
    - `POST` : 회원 가입
    - `POST` : 로그인
    - `GET`   : 아이디 중복 확인
    - `POST` : 로그아웃

<br/>  


## 5-6 주차 : Todo List

---

- **ToDo List ( 5-6 주차)**  — *자세한 내용은 추후 문서 제공 예정*
    - DB 생성 및 연결, 연관 관계 설정
    - API
        - `POST` : 회원 생성
        - `GET` : 회원 목록 조회
        - `PATCH` : 회원 정보 수정
        - `POST` : Todo 생성
        - `GET` : 회원 + 회원의 Todo리스트 조회
        - `PATCH` : Todo 수정
        - `DELETE` : Todo 삭제
        - `GET` : Todo 식별자 조회( TODO + 회원 정보)

<br/>  


## 7-8 주차 : 게시판 API (옵션)

---

- **게시판(7-8 주차)** — *자세한 내용은 추후 문서 제공 예정*
    - DB 생성 및 연결, 연관 관계 설정
    - API
	![](/images/1ec66f9e-439b-407a-84d2-858d65ee87d0-image.png)

       