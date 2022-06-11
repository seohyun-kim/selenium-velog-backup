---
title: "[Github] 깃허브 레파지토리 생성 (visual studio 연동)"
description: "GitHub 바로가기  repository 이름 (= 프로젝트 이름)	영어 소문자, 띄어쓰기 대신 - 로 해주는 것이 좋음      Description은 상세 설명으로 필수입력은 아니고 옵셔널 함  Public은 모든 사용자에게 나의 소스코드가 공개됨!Private은"
date: 2022-03-29T05:29:33.515Z
tags: ["git","github"]
---

## 0. GitHub 접속& 로그인

[GitHub 바로가기](https://github.com)

<br>  

## 1. Repositories 탭에서 New 클릭
![](/images/124fe541-bbec-4106-8177-7eeaa9c14f2a-image.png)

<br>  

## 2. Repository 정보 입력
![](/images/26c41cdc-b523-4f9a-ab38-27660f7b9fa6-image.png)  
- **repository 이름** (= 프로젝트 이름)
	`영어 소문자`, `띄어쓰기 대신 -` 로 해주는 것이 좋음   
<br>      
- **Description**은 상세 설명으로 필수입력은 아니고 옵셔널 함  
<br>  
- **Public**은 모든 사용자에게 나의 소스코드가 공개됨!
**Private**은 나만 볼 수 있고 팀원을 초대하면 팀원끼리만 볼 수 있음
이 옵션은 언제든 공개 <-> 비공개 간 전환이 가능하므로 크게 신경 안써도 됨!!
<br>  
- **Add a README file**은 메인 화면에 나타나는 마크업 문서로 나중에 언제든 추가하고 없앨 수 있음! 굳이 안해도 됨~ 근데 해주는게 예쁘긴 함!
보통 README를 넣으면 default branch이름이 `main`으로 생기고, 안넣으면 `master`로 생기는 듯?
- **Add .gitignore**은 깃허브에 올라가면 안되는 정보 파일 (DB비밀번호 라던지~ 환경변수 같은거~)이 올라가지 않도록 함! 나중에 만들 수 있으니까 지금은 pass하자!



- 이렇게 주고 일단 만들게요
![](/images/ef29a292-a59b-461f-8929-a63b8fdb5e9c-image.png)

<br>  

## 3. Repository 생성됨!
![](/images/22394285-4cc4-47bd-81ea-837e969ca425-image.png)

- 기본으로 레파지토리 이름이랑 상세설명이 README파일에 들어가있음!
- README.md 는 `.md` 확장자의 마크업 문서인데 이 부분은 나중에 다룰게요~

<br>  


## 4. 소스코드 파일 생성
![](/images/e679834a-bc34-497b-b7bc-cb7c81476e85-image.png)
<br>  


## 5. Git 연동
- Git 리포지토리 만들기
![](/images/ccc80c9f-85c3-4ae7-ae39-af77b2fb5617-image.png)

- 기존 원격으로 코드 푸시(아까 만들었으니까~)
![](/images/12cc9e62-5946-431f-baa1-0b66bc42b42d-image.png)  

  ![](/images/d8ab4b10-8963-421f-8cf6-01c8c2091c27-image.png)