---
title: "[Node.js] 3-1. http 모듈로 서버 만들기 (HTTP, fs, REST)"
description: "클라이언트가 서버로 요청을 보내, 서버는 요청을 처리하고 클라이언트로 응답을 보냄res 메서드로 응답을 보냄 ( write는 응답내용, end는 응답 마무리)listen으로 특정 포트에 연결Https 는 :443 포트가 생략되어있고, Http는 :80 이 생략되어있음비"
date: 2022-04-29T13:39:53.894Z
tags: []
---
# HTTP 서버 만들기

## 1. 서버와 클라이언트

![](/images/54d37229-b795-4d33-bbb3-a441e1059621-image.png)


- 클라이언트가 서버로 요청을 보내, 서버는 요청을 처리하고 클라이언트로 응답을 보냄

<br/>  

## 2. 노드로 http 서버 만들기

- `res` 메서드로 응답을 보냄 ( `write`는 응답내용, `end`는 응답 마무리)
- `listen`으로 특정 포트에 연결

```jsx
const http = require('http');

http.createServer((req, res) => {
  res.writeHead(200, { 'Content-Type': 'text/html; charset=utf-8' });
  res.write('<h1>Hello Node!</h1>');
  res.end('<p>Hello Server!</p>');
})
  .listen(8080, () => { // 서버 연결
    console.log('8080번 포트에서 서버 대기 중입니다!');
  });
```

> Https 는 `:443` 포트가 생략되어있고, Http는 `:80` 이 생략되어있음
> 

- 비동기이기 때문에 에러 처리를 해 주는 것이 안전함
    
    ```jsx
    // 위 코드에서 에러처리 한 버전임
    const http = require('http');
    const server = http.createServer((req, res) => {
      res.writeHead(200, { 'Content-Type': 'text/html; charset=utf-8' });
      res.write('<h1>Hello Node!</h1>');
      res.end('<p>Hello Server!</p>');
    });
    server.listen(8080);
    
    server.on('listening', () => {
      console.log('8080번 포트에서 서버 대기 중입니다!');
    });
    server.on('error', (error) => {
      console.error(error);
    });
    ```
    

> 서버 코드를 수정하였으면 재 실행 해야 반영 됨!!
> 

<br/>  

## 3. 포트

- 스크립트를 실행하면 8080포트에 연결 됨
    
    ![](/images/1c81c346-01dc-4117-bc6e-4efb7e3c068b-image.png)

    
- Localhost는 컴퓨터 내부의 주소로 외부에서 접근 불가능
- 포트는 서버 내에서 프로세스를 구분하는 번호임
- 다른 포트로 DB나 다른 서버 동시 연결 가능
    
    ![](/images/73661235-4904-446e-9f53-511e46856de0-image.png)


<br/>  

<br/>  


# fs로 HTML 읽어 제공하기

- html 파일을 작성하고 fs를 이용해 js에 넣어줌

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8" />
    <title>Node.js 웹 서버</title>
</head>
<body>
    <h1>Node.js 웹 서버</h1>
    <p>만들 준비되셨나요?</p>
</body>
</html>
```

```jsx
const http = require('http');
const fs = require('fs').promises;

http.createServer(async (req, res) => {
  try {
    const data = await fs.readFile('./server2.html');
    res.writeHead(200, { 'Content-Type': 'text/html; charset=utf-8' });
    res.end(data);
  } catch (err) {
    console.error(err);
    res.writeHead(500, { 'Content-Type': 'text/plain; charset=utf-8' });
    res.end(err.message);
  }
})
  .listen(8081, () => {
    console.log('8081번 포트에서 서버 대기 중입니다!');
  });
```

<br/>  

# REST API 서버 만들기

## REST API (주소 정하는 규칙?)

1. 서버에 요청을 보낼 때는 주소를 통해 요청의 내용을 표현
    - /index.html 이면 index.html을 보내 달라는 뜻
    - 항상 html을 요구할 필요는 없음
    - 서버가 이해하기 쉬운 주소가 좋음
2. REST API( Representational State Transfer) 
    - 서버의 자원을 정의하고 자원에 대한 주소를 지정하는 방법
    - 구조적으로 지정해 놓으면 클라이언트가 알아보기 쉽다.
    - 하지만 예측 가능한 만큼 해커의 공격 위협이 존재한다.
3. HTTP 요청 메서드
    - GET: 서버 자원을 가져오라고 할 때 사용
    - POST: 서버에 자원을 새로 등록하고자 할 때 사용(또는 뭘 써야할 지 애매할 때)
    - PUT: 서버의 자원을 요청에 들어있는 자원으로 치환하고자할 때 사용 (보통 **전체** 수정)
    - PATCH: 서버 자원의 **일부**만 수정하고자 할 때 사용
    - DELETE: 서버의 자원을 삭제하고자 할 때 사용


<br/>  

## HTTP 프로토콜

![](/images/8d2f9ec1-9d6a-421b-8708-635ae421df5c-image.png)


- 클라이언트가 누구든 서버와 HTTP프로토콜로 소통 가능
    - IOS, 안드로이드, 웹이 모두 같은 주소로 요청 보낼 수 있음
    - 서버와 클라이언트의 분리
    

![](/images/a99a6945-7c6e-4674-bcc2-6e81719c6fce-image.png)


- RESTful
    - REST API를 사용한 주소 체계를 이용하는 서버
    - GET/user 는 사용자를 조회하는 요청, POST/user은 사용자를 등록하는 요청

<br/>  

## REST 서버 만들기

code file

- **about.html**
    
    ```html
    <!DOCTYPE html>
    <html>
    <head>
      <meta charset="utf-8" />
      <title>RESTful SERVER</title>
      <link rel="stylesheet" href="./restFront.css" />
    </head>
    <body>
    <nav>
      <a href="/">Home</a>
      <a href="/about">About</a>
    </nav>
    <div>
      <h2>소개 페이지입니다.</h2>
      <p>사용자 이름을 등록하세요!</p>
    </div>
    </body>
    </html>
    ```
    
- **restFront.html**
    
    ```html
    <!DOCTYPE html>
    <html lang="ko">
    <head>
      <meta charset="utf-8" />
      <title>RESTful SERVER</title>
      <link rel="stylesheet" href="./restFront.css" />
    </head>
    <body>
    <nav>
      <a href="/">Home</a>
      <a href="/about">About</a>
    </nav>
    <div>
      <form id="form">
        <input type="text" id="username">
        <button type="submit">등록</button>
      </form>
    </div>
    <div id="list"></div>
    <script src="https://unpkg.com/axios/dist/axios.min.js"></script>
    <script src="./restFront.js"></script>
    </body>
    </html>
    ```
    
- **restFront.css**
    
    ```css
    a { color: blue; text-decoration: none; }
    ```
    
- **restFront.js**
    
    ```jsx
    async function getUser() { // 로딩 시 사용자 가져오는 함수
      try {
        const res = await axios.get('/users');
        const users = res.data;
        const list = document.getElementById('list');
        list.innerHTML = '';
        // 사용자마다 반복적으로 화면 표시 및 이벤트 연결
        Object.keys(users).map(function (key) {
          const userDiv = document.createElement('div');
          const span = document.createElement('span');
          span.textContent = users[key];
          const edit = document.createElement('button');
          edit.textContent = '수정';
          edit.addEventListener('click', async () => { // 수정 버튼 클릭
            const name = prompt('바꿀 이름을 입력하세요');
            if (!name) {
              return alert('이름을 반드시 입력하셔야 합니다');
            }
            try {
              await axios.put('/user/' + key, { name });
              getUser();
            } catch (err) {
              console.error(err);
            }
          });
          const remove = document.createElement('button');
          remove.textContent = '삭제';
          remove.addEventListener('click', async () => { // 삭제 버튼 클릭
            try {
              await axios.delete('/user/' + key);
              getUser();
            } catch (err) {
              console.error(err);
            }
          });
          userDiv.appendChild(span);
          userDiv.appendChild(edit);
          userDiv.appendChild(remove);
          list.appendChild(userDiv);
          console.log(res.data);
        });
      } catch (err) {
        console.error(err);
      }
    }
    
    window.onload = getUser; // 화면 로딩 시 getUser 호출
    // 폼 제출(submit) 시 실행
    document.getElementById('form').addEventListener('submit', async (e) => {
      e.preventDefault();
      const name = e.target.username.value;
      if (!name) {
        return alert('이름을 입력하세요');
      }
      try {
        await axios.post('/user', { name });
        getUser();
      } catch (err) {
        console.error(err);
      }
      e.target.username.value = '';
    });
    ```
    
- **restServer.js**
    
    ```jsx
    const http = require('http');
    const fs = require('fs').promises;
    
    const users = {}; // 데이터 저장용
    
    http.createServer(async (req, res) => {
      try {
        if (req.method === 'GET') {
          if (req.url === '/') {
            const data = await fs.readFile('./restFront.html');
            res.writeHead(200, { 'Content-Type': 'text/html; charset=utf-8' });
            return res.end(data);
          } else if (req.url === '/about') {
            const data = await fs.readFile('./about.html');
            res.writeHead(200, { 'Content-Type': 'text/html; charset=utf-8' });
            return res.end(data);
          } else if (req.url === '/users') {
            res.writeHead(200, { 'Content-Type': 'application/json; charset=utf-8' });
            return res.end(JSON.stringify(users));
          }
          // /도 /about도 /users도 아니면
          try {
            const data = await fs.readFile(`.${req.url}`);
            return res.end(data);
          } catch (err) {
            // 주소에 해당하는 라우트를 못 찾았다는 404 Not Found error 발생
          }
        } else if (req.method === 'POST') {
          if (req.url === '/user') {
            let body = '';
            // 요청의 body를 stream 형식으로 받음 ⭐⭐⭐
            req.on('data', (data) => {
              body += data;
            });
            // 요청의 body를 다 받은 후 실행됨
            return req.on('end', () => {
              console.log('POST 본문(Body):', body);
              const { name } = JSON.parse(body);
              const id = Date.now();
              users[id] = name;
              res.writeHead(201, { 'Content-Type': 'text/plain; charset=utf-8' });
              res.end('ok');
            });
          }
        } else if (req.method === 'PUT') {
          if (req.url.startsWith('/user/')) {
            const key = req.url.split('/')[2];
            let body = '';
            req.on('data', (data) => {
              body += data;
            });
            return req.on('end', () => {
              console.log('PUT 본문(Body):', body);
              users[key] = JSON.parse(body).name;
              res.writeHead(200, { 'Content-Type': 'text/plain; charset=utf-8' });
              return res.end('ok');
            });
          }
        } else if (req.method === 'DELETE') {
          if (req.url.startsWith('/user/')) {
            const key = req.url.split('/')[2];
            delete users[key];
            res.writeHead(200, { 'Content-Type': 'text/plain; charset=utf-8' });
            return res.end('ok');
          }
        }
        res.writeHead(404);
        return res.end('NOT FOUND');
      } catch (err) {
        console.error(err);
        res.writeHead(500, { 'Content-Type': 'text/plain; charset=utf-8' });
        res.end(err.message);
      }
    })
      .listen(8082, () => {
        console.log('8082번 포트에서 서버 대기 중입니다');
      });
    ```
    
<br/>  

- GET 메서드에서 `/`, `/about` 요청 주소는 페이지를 요청하는 것이므로 HTML 파일을 읽어서 전송.
    
    AJAX 요청을 처리하는 `/users`에서는 users 데이터를 전송.
    
    JSON 형식으로 보내기 위해 JSON.stringify를 해줌. `JSON.stringify(users)`
    
    그 외 GET 요청은 CSS나 JS 파일을 요청하는 것이므로 찾아서 보내주고, 없다면 404 NOT FOUND 에러를 응답
    
- POST와 PUT 메서드는 클라이언트로부터 데이터를 받으므로 특별한 처리가 필요.
    
    `req.on('data', 콜백)`과 `req.on('end', 콜백` ( 3.6.2절의 버퍼와 스트림에서 배웠던 readStream임)
    
    readStream으로 요청과 같이 들어오는 요청 본문을 받을 수 있음.
    
    단, 문자열이므로 JSON으로 만드는 `JSON.parse` 과정이 한 번 필요.
    
- DELETE 메서드로 요청이 오면 주소에 들어 있는 키에 해당하는 사용자를 제거.
- 해당하는 주소가 없을 경우 404 NOT FOUND 에러를 응답.


<br/>  

## REST 요청 확인하기

![](/images/84e06625-2dd8-41ba-9f56-8ec33e8d6af0-image.png)


- F12 Network 탭에서 요청 내용 실시간 확인 가능
    - Name은 요청 주소, Method는 요청 메서드, Status 는 HTTP응답코드
    - Protocol은 HTTP 프로토콜, Type은 요청 종류 (xhr은 AJAX요청)
    
    
    
<br/>  


[인프런 Node.js 강의](https://www.inflearn.com/course/%EB%85%B8%EB%93%9C-%EA%B5%90%EA%B3%BC%EC%84%9C/dashboard)
Zerocho 님의 "Node.js 교과서 - 기본부터 프로젝트 실습까지" 강의를 기반으로 작성한 문서입니다. 