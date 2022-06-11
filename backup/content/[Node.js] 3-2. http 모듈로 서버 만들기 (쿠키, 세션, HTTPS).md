---
title: "[Node.js] 3-2. http 모듈로 서버 만들기 (쿠키, 세션, HTTPS)"
description: "요청에는 한 가지 단점이 있음누가 요청을 보냈는지 모름(IP주소와 브라우저 정보 정도만 앎)로그인을 구현하면 됨! ⇒ 쿠키와 세션 필요쿠키 : 키=값 의 쌍  매 요청마다 서버에 동봉해서 보냄 , 서버는 쿠키를 읽어 누구인지 파악브라우저에서 알아서 쿠키를 서버에 보내줌"
date: 2022-04-29T13:44:58.351Z
tags: []
---
# 쿠키 🍪

## 쿠키의 필요성

- 요청에는 한 가지 단점이 있음
    - 누가 요청을 보냈는지 모름(IP주소와 브라우저 정보 정도만 앎)
    - 로그인을 구현하면 됨! ⇒ 쿠키와 세션 필요

- 쿠키 : `키=값` 의 쌍
    
    ![](/images/0d7b7ae4-a5cd-41e4-8a81-20ca399b4400-image.png)

    
    - 매 요청마다 서버에 동봉해서 보냄 , 서버는 쿠키를 읽어 누구인지 파악
    - 브라우저에서 알아서 쿠키를 서버에 보내줌. 우리는 그 사람이 누구 인지만 적어주면 됨!!
    - 처음에 한번 쿠키를 받으면 그다음부터 쿠키와 함께 요청을 보냄

<br/>  

## 쿠키 서버

- 쿠키 넣는 것 직접 구현
    
    ```jsx
    const http = require('http');
    
    http.createServer((req, res) => {
      console.log(req.url, req.headers.cookie); //요청의 헤더에 들어있는 쿠키를 찍음
      res.writeHead(200, { 'Set-Cookie': 'mycookie=test' }); //서버가 헤더에 쿠키를 넣어 보냄
      res.end('Hello Cookie');
    })
      .listen(8083, () => {
        console.log('8083번 포트에서 서버 대기 중입니다!');
      });
    ```
    
    - `writeHead`: 요청 헤더에 입력하는 메서드
    - `Set-Cookie`: 브라우저에게 쿠키를 설정하라고 명령
    - `req.headers.cookie` : 쿠키가 문자열로 담겨있음

- 개발자 도구에서 쿠키 확인
    - 요청이 전송되고 응답이 왔을 때 쿠키가 설정 됨
    - favicon.ico는 브라우저가 자동으로 보내는 요청
    - 두번 째 요청인 fabicon.ico에 쿠키가 넣어짐

![](/images/79be161b-85e7-4845-bb79-36c1ef531ea0-image.png)


![](/images/1c143313-cca1-469c-829e-537468762efb-image.png)


<br/>  

## 헤더와 본문

![](/images/8207486b-8f10-441b-8ca3-cd93ab1ca3dd-image.png)


- http 요청과 응답은 헤더와 본문을 가짐
    - 헤더는 요청 또는 응답에 대한 정보를 가짐
    - 본문은 주고받는 실제 데이터
    - 쿠키는 부가적인 정보이므로 헤더에 저장

<br/>  

## http 상태코드

- writeHead 메서드에 첫 번째 인수로 넣은 값
    - 요청이 성공했는지 실패 했는지를 알려줌
    - `2XX`: **성공**을 알리는 상태 코드입니다. 대표적으로 200(성공), 201(작성됨)이 많이 사용됩니다.
    - `3XX`: **리다이렉션(다른 페이지로 이동)**을 알리는 상태 코드입니다. 어떤 주소를 입력했는데 다른 주소의 페이지로 넘어갈 때 이 코드가 사용됩니다. 대표적으로 301(영구 이동), 302(임시 이동)가 있습니다.
    - `4XX`: **요청 오류**를 나타냅니다. 요청 자체에 오류가 있을 때 표시됩니다. 대표적으로 401(권한 없음), 403(금지됨), 404(찾을 수 없음)가 있습니다.
    - `5XX`: **서버 오류**를 나타냅니다. 요청은 제대로 왔지만 서버에 오류가 생겼을 때 발생합니다. 이 오류가 뜨지 않게 주의해서 프로그래밍해야 합니다. 이 오류를 클라이언트로 res.writeHead로 직접 보내는 경우는 없고, 예기치 못한 에러 발생 시 서버가 알아서 5XX대 코드를 보냅니다. 500(내부 서버 오류), 502(불량 게이트웨이), 503(서비스를 사용할 수 없음)이 자주 사용됩니다.

- 참고 :  [HTTP 상태 코드 - HTTP | MDN](https://developer.mozilla.org/ko/docs/Web/HTTP/Status)
    
<br/>  

## 쿠키로 나를 식별하기 (로그인 요청과 쿠키)

- **cookie2.html**
    
    ```html
    <!DOCTYPE html>
    <html>
    <head>
        <meta charset="utf-8" />
        <title>쿠키&세션 이해하기</title>
    </head>
    <body>
    <form action="/login">
        <input id="name" name="name" placeholder="이름을 입력하세요" />
        <button id="login">로그인</button>
    </form>
    </body>
    </html>
    ```
    
- **cookie2.js**
    
    ```jsx
    const http = require('http');
    const fs = require('fs').promises;
    const url = require('url');
    const qs = require('querystring');
    
    //문자열을 객체로 바꿔주는 함수
    const parseCookies = (cookie = '') =>
      cookie
        .split(';')
        .map(v => v.split('='))
        .reduce((acc, [k, v]) => {
          acc[k.trim()] = decodeURIComponent(v);
          return acc;
        }, {});
    
    http.createServer(async (req, res) => {
      const cookies = parseCookies(req.headers.cookie); // { mycookie: 'test' }
      // 주소가 /login으로 시작하는 경우
      if (req.url.startsWith('/login')) {
        const { query } = url.parse(req.url);
        const { name } = qs.parse(query);
        const expires = new Date();
    
        expires.setMinutes(expires.getMinutes() + 5); // 쿠키 유효 시간을 현재시간 + 5분으로 설정
    	
        res.writeHead(302, {  //리다이렉트 됨
          Location: '/',
          'Set-Cookie': `name=${encodeURIComponent(name)}; Expires=${expires.toGMTString()}; HttpOnly; Path=/`,
        }); // 쿠키에 추가설정 해 줄 수 있음
        res.end();
      // name이라는 쿠키가 있는 경우
      } else if (cookies.name) {
        res.writeHead(200, { 'Content-Type': 'text/plain; charset=utf-8' });
        res.end(`${cookies.name}님 안녕하세요`);
      } else {
        try {
          const data = await fs.readFile('./cookie2.html');
          res.writeHead(200, { 'Content-Type': 'text/html; charset=utf-8' });
          res.end(data);
        } catch (err) {
          res.writeHead(500, { 'Content-Type': 'text/plain; charset=utf-8' });
          res.end(err.message);
        }
      }
    })
      .listen(8084, () => {
        console.log('8084번 포트에서 서버 대기 중입니다!');
      });
    ```
    

- 쿠키에 내 정보를 입력
    - `parseCookies` : 쿠키 문자열을 객체로 변환
    - 주소가 `/login`인 경우와 `/`인 경우로 나뉨
    - `/login`인 경우 쿼리 스트링으로 온 이름을 쿠키로 저장
    - 그 외의 경우 쿠키가 있는지 없는지 판단 (있으면 환영 인사, 없으면 로그인 페이지로 리다이렉트)

- Set-Cookie 의 다양한 옵션
    
    ![](/images/f57601d4-c536-44bb-833c-886e5f3cc5c2-image.png)

    
    - `쿠키명=쿠키값:` 기본적인 쿠키의 값.
    - `Expires=날짜` : 만료 기한.
        
        이 기한이 지나면 쿠키가 제거됨. (기본 값은 클라이언트가 종료될 때까지)
        
    - `Max-age=초` : Expires와 비슷하지만 날짜 대신 초를 입력
        
        해당 초가 나면 쿠키가 제거됨. Expires보다 우선
        
    - `Domain=도메인명` : 쿠키가 전송될 도메인을 특정. (기본 값은 현재 도메인)
    - `Path=URL` : 쿠키가 전송될 URL을 특정.( 기본 값은 ‘/’이고 이 경우 모든 URL에서 쿠키를 전송 가능)
    - `Secure`: HTTPS일 경우에만 쿠키가 전송됩니다.
    - `HttpOnly`: 설정 시 자바스크립트에서 쿠키에 접근❌. 쿠키 조작 방지하기 위해 설정 하셈


<br/>  
<br/>  

# 세션 🔑

- 쿠키의 정보는 노출되고 수정되는 위험이 있음
    - 중요한 정보는 서버에서 관리하고, 그 정보에 접속할 수 있는 세션 Key만 클라이언트에 보냄
    - 서버에 세션 객체(`session`) 생성 후, `uniqueInt(키)`를 만들어 속성 명으로 사용
    - 속성 값에 정보 저장하고 `uniqueInt`를 클라이언트에 보냄

```jsx
const http = require('http');
const fs = require('fs').promises;
const url = require('url');
const qs = require('querystring');

const parseCookies = (cookie = '') =>
  cookie
    .split(';')
    .map(v => v.split('='))
    .reduce((acc, [k, v]) => {
      acc[k.trim()] = decodeURIComponent(v);
      return acc;
    }, {});

const session = {};

http.createServer(async (req, res) => {
  const cookies = parseCookies(req.headers.cookie);
  if (req.url.startsWith('/login')) {
    const { query } = url.parse(req.url);
    const { name } = qs.parse(query);
    const expires = new Date();
    expires.setMinutes(expires.getMinutes() + 5);
    const uniqueInt = Date.now();
    session[uniqueInt] = {
      name,
      expires,
    };
    res.writeHead(302, {
      Location: '/',
      'Set-Cookie': `session=${uniqueInt}; Expires=${expires.toGMTString()}; HttpOnly; Path=/`,
    }); // 브라우저로 중요한 정보를 노출시키지 않음!
    res.end();

  // 세션쿠키가 존재하고, 만료 기간이 지나지 않았다면
  } else if (cookies.session && session[cookies.session].expires > new Date()) {
    res.writeHead(200, { 'Content-Type': 'text/plain; charset=utf-8' });
    res.end(`${session[cookies.session].name}님 안녕하세요`);
		//서버쪽에서 세션에 접근해 이름 꺼내서 씀
  } else {
    try {
      const data = await fs.readFile('./cookie2.html');
      res.writeHead(200, { 'Content-Type': 'text/html; charset=utf-8' });
      res.end(data);
    } catch (err) {
      res.writeHead(500, { 'Content-Type': 'text/plain; charset=utf-8' });
      res.end(err.message);
    }
  }
})
  .listen(8085, () => {
    console.log('8085번 포트에서 서버 대기 중입니다!');
  });
```

<br/>  
<br/>  

# HTTPS와 HTTP2

## https

- 웹 서버에 SSL 암호화를 추가하는 모듈
    - 오고 가는 데이터를 암호화해서 중간에 다른 사람이 요청을 가로채더라도 내용을 확인할 수 없음
    - 요즘은 https 적용이 필수!!!
    
    ![](/images/2a81d2bf-578a-4d10-a371-96eb04fb5cf9-image.png)

    
- HTTP를 HTTPS로
    
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
    
    🔽
    
    ```jsx
    const https = require('https');
    const fs = require('fs');
    
    https.createServer({
      cert: fs.readFileSync('도메인 인증서 경로'),
      key: fs.readFileSync('도메인 비밀키 경로'),
      ca: [
        fs.readFileSync('상위 인증서 경로'),
        fs.readFileSync('상위 인증서 경로'),
      ],
    }, (req, res) => {
      res.writeHead(200, { 'Content-Type': 'text/html; charset=utf-8' });
      res.write('<h1>Hello Node!</h1>');
      res.end('<p>Hello Server!</p>');
    })
      .listen(443, () => {
        console.log('443번 포트에서 서버 대기 중입니다!');
      });
    ```
    
    - `https`는인증 기관의 인증서가 필요함
    - `readFileSync`는 거의 쓰지 말라했는데, 서버 시작할 때나 초기화 할 때 Sync써도 됨!
        
        서버 시작 하기 전 인증서를 넣어줌
        
        - 무료 인증서 발급 개꿀딱 사이트  
        
            
            [Let's Encrypt - 무료 SSL/TLS 인증서](https://letsencrypt.org/ko/)
            
    - `createServer` 가 인자를 두 개 받음
        
        첫 번째 인자는 인증서와 관련된 옵션 객체 (pem, crrt, key등 인증서 구입 시 얻을 수 있는 파일 넣기)
        
        두 번째 인자는 서버 로직
        
<br/>  

## http2 (빠름💨 + 암호화🔏)

![](/images/d81b227e-4e3e-4838-b980-59833421eafc-image.png)


- SSL 암호화와 더불어 최신 HTTP 프로토콜인 http/2 를 사용하는 모듈
- http1.1 보다 요청이 빠름
    
    동시에 요청 보내서 처리하도록 함
    
- 실무 배포할 때 http2 쓰셈~

```jsx
const http2 = require('http2'); //⭐
const fs = require('fs');

http2.createSecureServer({  //⭐
  cert: fs.readFileSync('도메인 인증서 경로'),
  key: fs.readFileSync('도메인 비밀키 경로'),
  ca: [
    fs.readFileSync('상위 인증서 경로'),
    fs.readFileSync('상위 인증서 경로'),
  ],
}, (req, res) => {
  res.writeHead(200, { 'Content-Type': 'text/html; charset=utf-8' });
  res.write('<h1>Hello Node!</h1>');
  res.end('<p>Hello Server!</p>');
})
  .listen(443, () => {
    console.log('443번 포트에서 서버 대기 중입니다!');
  });
```

<br/>  
<br/>  

# Cluster

- 기본적으로 싱글 스레드인 노드가 CPU 코어를 모두 사용할 수 있게 해주는 모듈
    - 포트를 공유하는 노드 **프로세스를 여러 개** 둘 수 있음
    - 요청이 많이 들어왔을 때 병렬로 실행된 서버의 개수만큼 요청이 분산 됨
    - 서버에 무리가 덜 감
    - 코어가 8개인 서버가 있을 때: 보통은 코어 1개만 활용
        
        cluster로 코어 하나 당 노드 프로세스 하나를 배정 가능
        
        성능이 8배가 되는 것은 아니지만 개선됨
        
    - 단점: 컴퓨터 자원(메모리, 세션 등) 공유 못함☹ →  Redis 등 별도 서버로 해결
    
- 서버 클러스터링
    
    ```jsx
    const cluster = require('cluster');
    const http = require('http');
    const numCPUs = require('os').cpus().length;
    
    if (cluster.isMaster) {
      console.log(`마스터 프로세스 아이디: ${process.pid}`);
    
      // CPU 개수만큼 워커를 생산
      for (let i = 0; i < numCPUs; i += 1) {
        cluster.fork();
      }
      // 워커가 종료되었을 때
      cluster.on('exit', (worker, code, signal) => {
        console.log(`${worker.process.pid}번 워커가 종료되었습니다.`);
        console.log('code', code, 'signal', signal);
        cluster.fork(); //종료시켰으니까 다시 생성
      });
    } else {
    
      // 워커들이 포트에서 대기
      http.createServer((req, res) => {
        res.writeHead(200, { 'Content-Type': 'text/html; charset=utf-8' });
        res.write('<h1>Hello Node!</h1>');
        res.end('<p>Hello Cluster!</p>');
        setTimeout(() => { // 워커 존재를 확인하기 위해 1초마다 강제 종료
          process.exit(1);
        }, 1000);
      }).listen(8086);
    
      console.log(`${process.pid}번 워커 실행`);
    }
    ```
    
    - 마스터 프로세스와 워커 프로세스
        - 마스터 프로세스는 CPU개수만큼 워커 프로세스를 만듦 (worker_threads 랑 구조 비슷)
        - 요청이 들어오면 워커 프로세스에 고르게 분배 ( 마스터 프로세스가 분배 역할)
    - 클러스터링을 하면 여러 개의 프로세스를 하나의 포트에 묶어 놓을 수 있음
        
        워커마다 다른 포트를 쓰지 않고 하나의 포트를 쓰면서 고르게 분배 (by. Round Robin)
        
        
        
        
<br/>  


[인프런 Node.js 강의](https://www.inflearn.com/course/%EB%85%B8%EB%93%9C-%EA%B5%90%EA%B3%BC%EC%84%9C/dashboard)
Zerocho 님의 "Node.js 교과서 - 기본부터 프로젝트 실습까지" 강의를 기반으로 작성한 문서입니다. 