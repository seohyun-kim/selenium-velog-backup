---
title: "[Node.js] 4. 패키지 매니저"
description: "npm (Node Package Manager) : 노드의 패키지 매니저현재 프로젝트에 대한 정보와 사용 중인 패키지에 대한 정보를 담음같은 패키지라도 버전 별로 기능이 다를 수 있으므로 버전을 기록해 두어야 함동일한 버전을 설치하지 않으면 문제가 생길 수 있음노드 프"
date: 2022-04-29T13:55:05.309Z
tags: []
---

# package.json

- npm (Node Package Manager) : 노드의 패키지 매니저

<br/>  

## package.json

- 현재 프로젝트에 대한 정보와 사용 중인 패키지에 대한 정보를 담음
    - 같은 패키지라도 버전 별로 기능이 다를 수 있으므로 버전을 기록해 두어야 함
    - 동일한 버전을 설치하지 않으면 문제가 생길 수 있음
    - 노드 프로젝트 시작 전 package.json 부터 만들고 시작함 (npm init)

<br/>  

## package.json 속성들

```jsx
{
  "name": "npmtest",
  "version": "0.0.1",
  "description": "hello package.json",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "author": "ZeroCho",
  "license": "ISC",
  "dependencies": {
    "cookie-parser": "^1.4.5",
    "express": "^4.17.1",
    "express-session": "^1.17.1",
    "morgan": "^1.10.0"
  },
  "devDependencies": {
    "nodemon": "^2.0.3",
    "rimraf": "^3.0.2"
  }
```

- `package name`: 패키지의 이름입니다.
- `version`: 패키지의 버전입니다.
- `entry point`: 자바스크립트 실행 파일 진입점입니다. 보통 마지막으로 module.exports를 하는 파일을 지정합니다. package.json의 main 속성에 저장됩니다.
- `test command`: 코드를 테스트할 때 입력할 명령어를 의미합니다. package.json scripts 속성 안의 test 속성에 저장됩니다.
- `git repository`: 코드를 저장해둔 Git 저장소 주소를 의미합니다. 나중에 소스에 문제가 생겼을 때 사용자들이 이 저장소에 방문해 문제를 제기할 수도 있고, 코드 수정본을 올릴 수도 있습니다. package.json의 repository 속성에 저장됩니다.
- `keywords:` 키워드는 npm 공식 홈페이지([https://npmjs.com](https://npmjs.com/))에서 패키지를 쉽게 찾을 수 있게 해줍니다. package.json의 keywords 속성에 저장됩니다.
- `license`: 해당 패키지의 라이선스를 넣어주면 됩니다.

<br/>  

## node_modules

![](/images/32acca04-f18c-4a44-9941-0a93a2fc1188-image.png)


- npm install 시 node_modules 폴더 생성
    - 내부에 설치한 패키지들이 들어있음
    - express 외에도 express와 의존 관계가 있는 패키지들이 모두 설치됨
- package-lock.json 도 생성되어 패키지 간 의존 관계를 명확하게 표시

> node_modules 는 용량을 많이 차지하기 때문에 package.json , package.lock 만 배포함.
> 
> 
> 이후에 npm i 를 하면 의존 관계있는 패키지들 설치됨.
> 

- 개발용 패키지
    
    `npm install —save-dev 패키지명` 또는 `npm i -D 패키지명`
    

- 글로벌(전역) 패키지
    
    `npm install —global 패키지명` 또는 `npm i -g 패키지명` ( 이건 좀 기피 ☹)
    
    - 모든 프로젝트와 콘솔에서 패키지를 사용할 수 있음
    - npx로 글로벌 설치 없이 글로벌 명령어 사용 가능 ⭐ ( 이거 쓰셈)
        
        `npx rimraf node_modules`
        
        `npm install —save-dev rimraf`
        

<br/>  

## 패키지 버전

- SemVer 버저닝(유의적 버저닝)
    - 노드 패키지 버전은 SemVer방식을 따름
    - 노드에서는 배포를 할 때 항상 버전을 올려야함
    
    ![](/images/7b7f6039-9cf0-4826-839a-9e1cd4a10f7c-image.png)

    
- 버전 기호 사용하기
    - 버전 앞에 기호를 붙여 의미를 더함
        - `^1.1.1`: 패키지 업데이트 시 minor 버전까지만 업데이트 됨(2.0.0버전은 안 됨)
        - `~1.1.1`: 패키지 업데이트 시 patch버전까지만 업데이트 됨(1.2.0버전은 안 됨)
        - =, <=, >, <는 이상, 이하, 초과, 미만.
        - @latest는 최신 버전을 설치하라는 의미
        - 실험적인 버전이 존재한다면 @next로 실험적인 버전 설치 가능(불안정함)
        - 각 버전마다 부가적으로 알파/베타/RC 버전이 존재할 수도 있음(1.1.1-alpha.0, 2.0.0-beta.1, 2.0.0-rc.0)


<br/>  
<br/>  

# npm 명령어들

- `npm outdated` : 어떤 패키지에 기능 변화가 생겼는지 알 수 있음
    
    ![](/images/35226169-08bc-4ad7-9c25-9e6db03ec755-image.png)

- `npm update`: package.json에 따라 패키지 업데이트
- `npm uninstall 패키지명`: 패키지 삭제(npm rm 패키지명으로도 가능)
- `npm search 검색어`: npm 패키지를 검색할 수 있음(npmjs.com에서도 가능)
- `npm info 패키지명`: 패키지의 세부 정보 파악 가능
- `npm login`: npm에 로그인을 하기 위한 명령어(npmjs.com에서 회원가입 필요)
- `npm whoami`: 현재 사용자가 누구인지 알려줌
- `npm logout`: 로그인한 계정을 로그아웃
- `npm version 버전`: package.json의 버전을 올림(Git에 커밋도 함)
- `npm deprecate [패키지명][버전] [메시지]`: 패키지를 설치할 때 경고 메시지를 띄우게 함(오류가 있는 패키지에 적용)
- `npm publish`: 자신이 만든 패키지를 배포
- `npm unpublish --force`: 자신이 만든 패키지를 배포 중단(배포 후 72시간 내에만 가능)
    - 다른 사람이 내 패키지를 사용하고 있는데 배포가 중단되면 문제가 생기기 때문


<br/>  
<br/>  

# npm 배포

1. npm 가입 ([https://www.npmjs.com/](https://www.npmjs.com/))
2. 배포할 패키지 작성 
    - package.json 과 main 부분과 배포할 파일 경로명이 일치해야함
3. 배포 시도하기
    
    `npm publish`
    
4. 배포 취소하기
    
    `npm unpublish 패키지명 —force`
    
    
<br/>  


[인프런 Node.js 강의](https://www.inflearn.com/course/%EB%85%B8%EB%93%9C-%EA%B5%90%EA%B3%BC%EC%84%9C/dashboard)
Zerocho 님의 "Node.js 교과서 - 기본부터 프로젝트 실습까지" 강의를 기반으로 작성한 문서입니다. 