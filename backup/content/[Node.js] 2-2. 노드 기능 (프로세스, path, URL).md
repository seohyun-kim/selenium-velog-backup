---
title: "[Node.js] 2-2. 노드 기능 (프로세스, path, URL)"
description: "현재 실행 중인 노드 프로세스에 대한 정보를 담고 있음시스템 환경 변수들이 들어있는 객체비밀 키(DB비밀번호, 서드 파티 앱 키 등)를 보관하는 용도로 쓰임환경 변수는 process.env 로 접근 가능일부 환경 변수는 노드 실행 시 영향을 미쳐 메모리가 부족하거나, "
date: 2022-04-29T12:49:16.308Z
tags: ["JavaScript","node.js"]
---
# Process

- 현재 실행 중인 노드 프로세스에 대한 정보를 담고 있음

```jsx
> process.version
'v14.16.0' // 설치 된 노드 버전
> process.arch
'x64' // 프로세서 아키텍처 정보
> process.platform
'win32' // 운영체제 플랫폼
> process.pid
7528 // 현재 프로세스의 아이디
> process.uptime()
359.828333 //프로세스가 시작된 후 흐른 시간(초)
> process.execPath
'C:\\Program Files\\nodejs\\node.exe' // 노드의 경로
> process.cwd()
'C:\\Users\\selen' //현재 프로세스가 실행되는 위치
> process.cpuUsage()
{ user: 796000, system: 484000 } // 현재 cpu 사용량
```

<br/>  

## process.env

- 시스템 환경 변수들이 들어있는 객체
    - 비밀 키(DB비밀번호, 서드 파티 앱 키 등)를 보관하는 용도로 쓰임
    - 환경 변수는 `process.env` 로 접근 가능
        
        ```jsx
        const secretId = process.env.SECRET_ID;
        const secretCode = process.env.SECRET_CODE;
        ```
        
    
- 일부 환경 변수는 노드 실행 시 영향을 미쳐 메모리가 부족하거나,  스레드 풀 개수가 부족할 수 있음  ⇒ 늘려줌
    
    ```jsx
    NODE_OPTIONS= --max-old-space-size=8192
    UV_THREADPOOL_SIZE = 8
    ```
    
<br/>  

## Process.nextTick (콜백)

- 이벤트 루프가 다른 콜백 함수들 보다 `nextTick`의 콜백 함수를 우선적으로 처리

```jsx
setImmediate(() => {
  console.log('immediate');
});
process.nextTick(() => {
  console.log('nextTick'); //우선적으로 처리
});
setTimeout(() => {
  console.log('timeout');
}, 0);
Promise.resolve().then(() => console.log('promise'));
```
<br/>  

## Process.exit(코드)

- 현재의 프로세스를 멈춤
    - 코드가 없거나 0 이면 정상 종료
    - 이외의 코드는 비정상 종료를 의미

```jsx
let i = 1;
setInterval(() => {
  if (i === 5) {
    console.log('종료!');
    process.exit();
  }
  console.log(i);
  i += 1;
}, 1000);
```

![](/images/8f118543-cc16-4cb6-b00d-8dd2308e89f2-image.png)


<br/>  
<br/>  

# OS와 Path

## OS

```jsx
const os = require('os');

console.log('운영체제 정보---------------------------------');
console.log('os.arch():', os.arch()); //process.arch와 동일
console.log('os.platform():', os.platform()); //process.platform과 동일
console.log('os.type():', os.type()); //운영체제의 종류
console.log('os.uptime():', os.uptime()); //운영체제 부팅 이후 흐른 시간(초)
//process.uptime()은 node의 실행시간임!!
console.log('os.hostname():', os.hostname()); //컴퓨터의 이름
console.log('os.release():', os.release()); //운영체제의 버전

console.log('경로------------------------------------------');
console.log('os.homedir():', os.homedir()); //홈디렉토리 경로
console.log('os.tmpdir():', os.tmpdir()); // 임시 파일 저장 경로

console.log('cpu 정보--------------------------------------');
console.log('os.cpus():', os.cpus()); //컴퓨터 코어 정보
console.log('os.cpus().length:', os.cpus().length);

console.log('메모리 정보-----------------------------------');
console.log('os.freemem():', os.freemem()); //사용 가능한 메모리(RAM)
console.log('os.totalmem():', os.totalmem()); //전체 메모리 용량
```

    
> [Node.js 공식 문서(API Docs)](https://nodejs.org/dist/latest-v17.x/docs/api/os.html)
    

<br/>  

## Path

- 운영체제 별로 경로 구분 자가 다른데 이걸 알아서 path모듈이 처리해줌
    
    (Windows: `\` , POSIX: `/` )
    
    ```jsx
    const path = require('path');
    
    const string = __filename;
    
    console.log('path.sep:', path.sep); // 경로의 구분자
    console.log('path.delimiter:', path.delimiter); // 환경변수의 구분자
    
    console.log('path.dirname():', path.dirname(string)); //파일이 위치한 폴더 경로
    console.log('path.extname():', path.extname(string)); //파일의 확장자를 보여줌
    console.log('path.basename():', path.basename(string)); //파일의 이름을 보여줌
    console.log('path.basename - extname:', path.basename(string, path.extname(string)));
    
    console.log('path.parse()', path.parse(string)); //파일 경로를 분리
    console.log('path.format():', path.format({
      dir: 'C:\\users\\zerocho',
      name: 'path',
      ext: '.js',
    }));
    console.log('path.normalize():', path.normalize('C://users\\\\zerocho\\\path.js'));
    // 경로 구분자를 여러번 사용했거나 혼용했을 때 정상적인 경로로 변환해줌
    
    console.log('path.isAbsolute(C:\\):', path.isAbsolute('C:\\')); 
    console.log('path.isAbsolute(./home):', path.isAbsolute('./home'));
    // 파일의 경로가 절대경로인지 상대경로인지 true나 false로 알려줌
    
    console.log('path.relative():', path.relative('C:\\users\\zerocho\\path.js', 'C:\\'));
    console.log('path.join():', path.join(__dirname, '..', '..', '/users', '.', '/zerocho'));
    console.log('path.resolve():', path.resolve(__dirname, '..', 'users', '.', '/zerocho'));
    ```
    
    - `path.join()` 은 절대 경로를 무시함
    - `path.resolve()`는 절대 경로가 들어있으면 앞의 것(`__dirname`)을 무시


<br/>  
<br/>  

# URL과 Querystring

## URL 모듈

- 인터넷 주소를 쉽게 조작하도록 도와주는 모듈
    
    ```jsx
    ┌────────────────────────────────────────────────────────────────────────────────────────────────┐
    │                                              href                                              │
    ├──────────┬──┬─────────────────────┬────────────────────────┬───────────────────────────┬───────┤
    │ protocol │  │        auth         │          host          │           path            │ hash  │
    │          │  │                     ├─────────────────┬──────┼──────────┬────────────────┤       │
    │          │  │                     │    hostname     │ port │ pathname │     search     │       │
    │          │  │                     │                 │      │          ├─┬──────────────┤       │
    │          │  │                     │                 │      │          │ │    query     │       │
    
    "  https:   //    user   :   pass   @ sub.example.com : 8080   /p/a/t/h  ?  query=string   #hash "
    
    │          │  │          │          │    hostname     │ port │          │                │       │
    │          │  │          │          ├─────────────────┴──────┤          │                │       │
    │ protocol │  │ username │ password │          host          │          │                │       │
    ├──────────┴──┼──────────┴──────────┼────────────────────────┤          │                │       │
    │   origin    │                     │         origin         │ pathname │     search     │ hash  │
    ├─────────────┴─────────────────────┴────────────────────────┴──────────┴────────────────┴───────┤
    │                                              href                                              │
    └────────────────────────────────────────────────────────────────────────────────────────────────┘
    (All spaces in the "" line should be ignored. They are purely for formatting.)
    ```
    
    - url처리에 크게 두 가지 방식이 있음 (기존 노드 방식 vs WHATWG 방식)
    - 위 사진에서 위쪽은 기존 노드, 아래쪽은 WHATWG (최신)방식임
    
- 기존 노드 방식 메서드
    - `url.parse(주소)` : 주소를 분해. WHATWG 방식과 비교하면 `username`과 `password`대신 `auth` 속성이 있고, `serchParams` 대신 `query`가 있음
    - `url.format(객체)` : WHATWG 방식의 url과 기존 노드의 url 모두 사용할 수 있음. 분해되었던 url 객체를 다시 원래 상태로 조립
    
- querystring은 url에 data가 담겨 있는 것
    ![](/images/d55066c5-8a15-41ef-884f-85a581765d02-image.png)

    
<br/>  

## searchParams

- WHATWG 방식에서 쿼리 스트링(search) 부분 처리를 도와주는 객체

```jsx
const { URL } = require('url');

const myURL = new URL('http://www.gilbut.co.kr/?page=3&limit=10&category=nodejs&category=javascript');
console.log('searchParams:', myURL.searchParams);
console.log('searchParams.getAll():', myURL.searchParams.getAll('category'));
console.log('searchParams.get():', myURL.searchParams.get('limit'));
console.log('searchParams.has():', myURL.searchParams.has('page'));

console.log('searchParams.keys():', myURL.searchParams.keys());
console.log('searchParams.values():', myURL.searchParams.values());

myURL.searchParams.append('filter', 'es3');
myURL.searchParams.append('filter', 'es5');
console.log(myURL.searchParams.getAll('filter'));

myURL.searchParams.set('filter', 'es6');
console.log(myURL.searchParams.getAll('filter'));

myURL.searchParams.delete('filter');
console.log(myURL.searchParams.getAll('filter'));

console.log('searchParams.toString():', myURL.searchParams.toString());
myURL.search = myURL.searchParams.toString();
```

<br/>  

## querystring

- 기존 노드 방식에서는 url querystring을 `querystring` 모듈로 처리
    - querystring.parse(쿼리) : url의 query 부부을 자바스크립트 객체로 분해해줌
    - querystring.stringify(객체) : 분해된 query 객체를 문자열로 다시 조립해줌

```jsx
const url = require('url');
const querystring = require('querystring');

const parsedUrl = url.parse('http://www.gilbut.co.kr/?page=3&limit=10&category=nodejs&category=javascript');
const query = querystring.parse(parsedUrl.query);
console.log('querystring.parse():', query);
console.log('querystring.stringify():', querystring.stringify(query));
```