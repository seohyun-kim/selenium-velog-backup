---
title: "[Node.js] 5-1. 익스프레스 웹 서버 만들기 (express, 미들웨어, 에러처리, next)"
description: "http 모듈로 웹 서버를 만들 때 코드가 보기 좋지 않고, 확장성도 떨어짐  ⇒ 프레임 워크로 해결대표적인 것이 Express, Koa, Hapi코드 관리도 용이하고 편의성이 많이 높아짐"scripts": { "start": "nodemon app" } 를 작성해 두"
date: 2022-04-29T14:00:06.888Z
tags: []
---
# express 서버 사용해보기

<br/>  


## Express 소개

- http 모듈로 웹 서버를 만들 때 코드가 보기 좋지 않고, 확장성도 떨어짐
    
    ⇒ 프레임 워크로 해결
    
    - 대표적인 것이 `Express`, `Koa`, `Hapi`
    - 코드 관리도 용이하고 편의성이 많이 높아짐


<br/>  

## package.json 생성

```jsx
{
  "name": "learn-express",
  "version": "0.0.1",
  "scripts": {
    "start": "nodemon app"
  },
  "devDependencies": {
    "nodemon": "^2.0.3"
  },
  "dependencies": {
    "cookie-parser": "^1.4.5",
    "express": "^4.17.1",
    "express-session": "^1.17.1",
    "morgan": "^1.10.0"
  }
}
```

- `"scripts": { "start": "nodemon app" }` 를 작성해 두면

       ⇒  `npm run start` 또는 `npm start` 를 통해 실행시킬 수 있음


<br/>  

## app.js 작성

```jsx
const express = require('express');
const path = require('path');

const app = express();
app.set('port', process.env.PORT || 3000);

app.get('/', (req, res) => {
  // res.send('Hello, Express');
  res.sendFile(path.join(__dirname, '/index.html'));
});

app.listen(app.get('port'), () => {
  console.log(app.get('port'), '번 포트에서 대기 중');
});
```

- github에는 node-modules 올리지 마셈! (용량 크니까~)
- `nodemon app` 으로 실행시키면 프로젝트 파일이 바뀌었는지 여부를 검사해서 알아서 잘 처리해줌~ 알아서 재 시작 하고 에러 띄우고~
- `res.sendFile(path.join(__dirname, '/index.html'));` 이 코드가 실행이 되면서 그때 그때 html을 읽어서 보여줌
- 서버 구동의 핵심이 되는 파일
    - `app.set('port', 포트)` 로 서버가 실행될 포트 지정
    - `app.get('주소', 라우터)`로 GET 요청이 올 때 어떤 동작을 할지 지정
    - `app.listen('포트', 콜백)`으로 몇 번 포트에서 서버를 실행할 지 지정
- 서버 실행하기
    - app.js : 핵심 서버 스크립트
    - public : 외부에서 접근 가능한 파일들 모아둠
    - views : 템플릿 파일을 모아둠
    - routes : 서버의 라우터와 로직을 모아둠
        
        (추후에 models를 만들어 데이터베이스 사용)
        

<br/>  
<br/>  


# 미들웨어

<br/>  

## 미들웨어

- 익스프레스는 미들웨어로 구성됨
    - 요청과 응답의 중간에 위치하여 미들웨어.
    - `app.get()`에 공통된 코드를 `app.use()`에 묶어 한번에 처리 가능!
    - **위에서 아래 순서대로** 실행됨
    - 미들웨어는 req, res, next가 매개변수인 함수
    - req: 요청, res: 응답 조작 가능
    - next()로 다음 미들웨어로 넘어감
    
    ![](/images/a69012eb-4a6b-423c-99f9-9a8eb2c2b76c-image.png)

    
    ```jsx
    ...
    app.set('port', process.env.PORT || 3000);
    
    app.get('/', (req, res, next) => {
      console.log('모든 요청에 다 실행됨');
      next();
    });
    
    app.get('/', (req, res, next) => {
      console.log('GET / 요청에서만 실행됩니다.');
      next();
    }, (req, res) => {
      throw new Error('에러는 에러 처리 미들웨어로 갑니다.')
    });
    app.use((err, req, res, next) => {
      console.error(err);
      res.status(500).send(err.message);
    });
    ...
    ```
    
- 와일드카드~
    
    ```jsx
    app.get('/category/:name', (req, res, next) => {
      console.log(`hello ${req.params.name}`);
    });
    ```
    
    카테고리 하위에서는 모두 동작함!
    
    `app.get('/category/test', ...`  처럼  또 지정해 준 게 있어도 와일드 카드부분 소스가 먼저 나오면 우선 실행
    
    - 그래서 와일드 카드는 다른 카테고리보다 소스 코드 순서 상 아래에 위치 해야 함
        
        ```jsx
        app.get('/category/test', (req, res, next) => {
          console.log('hello test');
        });
        
        //와일드 카드 포함 된 코드를 더 아래에 작성
        app.get('/category/:name', (req, res, next) => {
          console.log(`hello ${req.params.name}`);
        }); 
        ```
        
    - 모든 것에 대해 처리하는 `*`도 하위에 넣어주어야 문제 발생하지 않음
        
        ```jsx
        app.get('*', (req, res, next) => {
          console.log('hello everybody');
        });
        ```
        
    

## 에러처리 미들웨어

- 에러가 발생하면 에러 처리 미들웨어로
    - `err`, `req`, `rs`, `next` 까지 **매개변수가 4개** (**next까지 꼭 붙어 있어야 함!!!)**
    - 첫 번째 err에는 에러에 관한 정보가 담김
    - `res.status` 메서드로 HTTP 상태 코드를 지정 가능(기본 값 200)
    - 에러처리 미들웨어를 연결 안 해도 express가 에러를 알아서 처리해 주긴 함.
        
        (그런데 에러 내용이 사용자 화면에 노출되면 보안 위협 발생 가능🚨)
        
    - 특별한 경우가 아니면 **가장 아래**에 위치하도록 함.
    
    ```jsx
    app.use((err, req, res, next) => {
      console.error(err); 
      res.status(500).send(err.message); 
    });
    ```
    
    - 에러 상태 코드를 지정하여 클라이언트에게 사기를 칠 수 있음! ⇒ 해커에게 혼란 가능👍
    
    - 하나의 라우터에 **`sendFile`,  `send`나 `json`이 여러 개 있으면 안돼**!! (⚠**반드시 기억**⚠)
        
        응답 보낸 다음 `res.writeHead()` 하는 것도 같은 에러 발생함!!
        
        ```jsx
        app.get('/upload', (req, res) => {
          res.sendFile(path.join(__dirname, 'multipart.html'));
          res.send('send 여러 개 있으면 안됨!!!'); // X
        	res.json({Nonono : '이것도 여러개있으면안돼!'}); // X
        
          res.writeHead(); // 응답 보낸 다음 이것도 쓰면 안돼!
        });
        ```
        
    
    - http에서 상속 받은 `req`, `res` 똑같이 쓸 수 있지만 **그냥 express꺼 써라**
        
        ```jsx
        app.get('/', (req, res) => {
        
          //http에서 쓰던 방식 이거두개 합한게 아래와 같음
        	res.writeHead(200, {'Content-Type': 'text/html'});
        	res.end('안녕하세요');
        
          //익스프레스에서는 이렇게 써라
        	res.status(200).send('안녕하세요.');
        
        });
        ```
        

# next 활용법

- `next`를 호출해야 다음 코드로 넘어 감
    - next를 주석 처리하면 응답이 전송되지 X
    - 다음 미들웨어(라우터 미들웨어)로 넘어가지 않기 때문
    - next에 인수로 값을 넣으면 에러 핸들러로 넘어감('route'인 경우 다음 라우터로)

![](/images/787033ca-d3a1-435d-a842-af77d611435c-image.png)


![](/images/32b51666-efa6-4fd2-8f9f-f6cb6352caed-image.png)


- `res.json`은 응답을 보낼 뿐이지 함수를 종료하는 `return`은 아니다.
- `res.json`으로 알아서 타입 맞추어 넣어줌
    
    API 만들 때는 `res.json` 많이 쓰고 웹 서버 만들 때는 `res.sendFile` 을 많이 사용함
    
    ```jsx
    app.get('/', (req, res) => {
    
      //이 두줄이
    	res.writeHead(200, {'Content-Type': 'application/json'});
    	res.end(JSON.stringify({hello: 'zerocho'}));
    
      //이것과 같음
    	res.json({hello: 'zerocho'});
    
    });
    ```
    

## next의 에러처리

- `next`에 인수가 없을 때는 다음 미들웨어로 넘어감
    
    ```jsx
    app.get('/', (req, res, next) => {
      console.log('GET / 요청에서만 실행됩니다.');
      next(); //next의 인수가 없을 때는 다음 미들웨어로 넘어감
    }, (req, res) => {
      throw new Error('에러는 에러 처리 미들웨어로 갑니다.')
    });
    ```
    

- `next`에 인수가 들어가 있으면 다음 미들웨어로 넘어가는 게 아니라 바로 에러 처리 미들웨어로 넘어감
    
    ```jsx
    app.get('/', (req, res, next) => {
      console.log('GET / 요청에서만 실행됩니다.');
      next();
    }, (req, res, next) => {
      try{
    		console.log('에러!');
    	} catch(error) {
    		next(error); // next에 인수가 들어가 있으면 다음미들웨어로 넘어가는게아니라 바로 에러처리 미들웨어로 넘어감
    	}
    });
    ```
    
- 모든 에러는 에러 처리 미들웨어로 넘어가서 처리됨
    
    ```jsx
    app.use((err, req, res, next) => {
      console.error(err);
      res.status(500).send(err.message);
    });
    ```
    
    
    

<br/>  


[인프런 Node.js 강의](https://www.inflearn.com/course/%EB%85%B8%EB%93%9C-%EA%B5%90%EA%B3%BC%EC%84%9C/dashboard)
Zerocho 님의 "Node.js 교과서 - 기본부터 프로젝트 실습까지" 강의를 기반으로 작성한 문서입니다. 