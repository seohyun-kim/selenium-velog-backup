---
title: "[Node.js] 2-3. 노드 기능 (암호화, 워커스레드)"
description: "Hash 에서는 sha512 추천👍양방향 암호화는 AES 추천👍비대칭 암호화는 RSA 추천👍복호화 불가능sha512 예제 (Hash 에서는 sha512 추천👍)creatHash(알고리즘) :  사용할 해시 알고리즘 넣어줌update(문자열) : 변환할 문자열을 "
date: 2022-04-29T12:54:15.118Z
tags: []
---
# Crypto 와 util

- Hash 에서는 `sha512` 추천👍
- 양방향 암호화는 `AES` 추천👍
- 비대칭 암호화는 `RSA` 추천👍

<br/> 

## Hash

- 복호화 불가능

- `sha512` 예제 (Hash 에서는 sha512 추천👍)
    
    ```jsx
    const crypto = require('crypto');
    
    console.log('base64:', crypto.createHash('sha512').update('비밀번호').digest('base64'));
    console.log('hex:', crypto.createHash('sha512').update('비밀번호').digest('hex'));
    console.log('base64:', crypto.createHash('sha512').update('다른 비밀번호').digest('base64'));
    ```
    
    - `creatHash(알고리즘)` :  사용할 해시 알고리즘 넣어줌
    - `update(문자열)` : 변환할 문자열을 넣어줌
    - `digest(인코딩)` : 인코딩 할 알고리즘을 넣어줌  
    
    <br/> 
    
- pbkdf2 알고리즘 예제 (salt값 있음)
    
    ```jsx
    const crypto = require('crypto');
    
    crypto.randomBytes(64, (err, buf) => {
      const salt = buf.toString('base64');
      console.log('salt:', salt);
      crypto.pbkdf2('비밀번호', salt, 100000, 64, 'sha512', (err, key) => {
        console.log('password:', key.toString('base64'));
      });
    });
    ```
    
    - `crypto.randomBytes` 로 64바이트 문자열 생성 → `salt` 역할
    - `pbkdf2` 인수로 순서대로 `비밀번호, salt, 반복 횟수, 출력 바이트, 알고리즘`
    - 반복 횟수를 조정해 암호화하는데 1초 정도 걸리게 맞추는 것이 권장됨

<br/> 

## 양방향 암호화

- 대칭형 암호화( 복호화 가능)
    
  ```jsx
   const crypto = require('crypto');
    
    const algorithm = 'aes-256-cbc';
    const key = 'abcdefghijklmnopqrstuvwxyz123456'; //32byte
    const iv = '1234567890123456'; //초기화 벡터 (16byte)
    
    const cipher = crypto.createCipheriv(algorithm, key, iv);
    let result = cipher.update('암호화할 문장', 'utf8', 'base64');
    result += cipher.final('base64');
    console.log('암호화:', result);
    
    const decipher = crypto.createDecipheriv(algorithm, key, iv);
    let result2 = decipher.update(result, 'base64', 'utf8');
    result2 += decipher.final('utf8');
    console.log('복호화:', result2);
    ```
    
    - 암호화할 때와 복호화 할 때 같은 key를 사용해야 함
    - 문자열은 `utf8` 인코딩, 암호는 `base64` 를 많이 사용

<br/> 

## util

- 각종 편의 기능을 모아둔 모듈

```jsx
const util = require('util');
const crypto = require('crypto');

const dontUseMe = util.deprecate((x, y) => {
  console.log(x + y);
}, 'dontUseMe 함수는 deprecated되었으니 더 이상 사용하지 마세요!');
dontUseMe(1, 2);

const randomBytesPromise = util.promisify(crypto.randomBytes);
randomBytesPromise(64)
  .then((buf) => {
    console.log(buf.toString('base64'));
  })
  .catch((error) => {
    console.error(error);
  });
```

- `deprecated`
    - 함수가 deprecated(중요도가 떨어져 더 이상 사용되지 않고 앞으로 사라지게 될) 처리되었음을 알려줌
    - 경고 메세지로 알려준 다음 버전 업데이트 시 삭제하는 방향이 좋음
- `promisify`
    - 콜백 패턴을 프로미스 패턴으로 바꿔 줌
    - `util.promisify()` 로 콜백함수를 감싸주면 promise 화 됨
    - 단 콜백이 `(error, data) ⇒ {}` 형식이어야 사용 가능
    - 프로미스 → 콜백 : `util.callbackify` 도 있지만 잘 사용되지는 X


<br/> 
<br/> 

# Worker_threads

- 노드에서 멀티 스레드 방식으로 작업을 할 수 있도록 하는 모듈

```jsx
const {
  Worker, isMainThread, parentPort,
} = require('worker_threads');

if (isMainThread) { // 부모일 때(메인스레드)
  const worker = new Worker(__filename);
  worker.on('message', message => console.log('from worker', message));
  worker.on('exit', () => console.log('worker exit'));
  worker.postMessage('ping');
} else { // 워커일 때
  parentPort.on('message', (value) => {
    console.log('from parent', value);
    parentPort.postMessage('pong');
    parentPort.close();
  });
}
```

![](/images/a078cf3e-1776-44df-b86a-1bc4c40090c2-image.png)

<br/>  


## 여러 워커스레드 사용하기

```jsx
const {
  Worker, isMainThread, parentPort, workerData,
} = require('worker_threads');

if (isMainThread) { // 부모일 때
  const threads = new Set();
  threads.add(new Worker(__filename, {
    workerData: { start: 1 },
  }));
  threads.add(new Worker(__filename, {
    workerData: { start: 2 },
  }));
  for (let worker of threads) {
    worker.on('message', message => console.log('from worker', message));
    worker.on('exit', () => {
      threads.delete(worker);
      if (threads.size === 0) {
        console.log('job done');
      }
    });
  }
} else { // 워커일 때
  const data = workerData;
  parentPort.postMessage(data.start + 100);
}
```

![](/images/49c662a0-bccd-40c7-9f59-6f195d4433cb-image.png)

![](/images/ae9371bc-3370-4432-90c6-8185c189c06e-image.png)

<br/>  


## 소수 찾기 예제

1. Worker thread 사용❌ (싱글 스레드)☹

```jsx
const min = 2;
const max = 10000000;
const primes = [];

function findPrimes(start, range) {
  let isPrime = true;
  const end = start + range;
  for (let i = start; i < end; i++) {
    for (let j = min; j < Math.sqrt(end); j++) {
      if (i !== j && i % j === 0) {
        isPrime = false;
        break;
      }
    }
    if (isPrime) {
      primes.push(i);
    }
    isPrime = true;
  }
}

console.time('prime');
findPrimes(min, max);
console.timeEnd('prime');
console.log(primes.length);
```

![](/images/b94dc768-fd20-45d6-a75d-d0b99ae43864-image.png)

- 8.7초동안 아무것도 못함


<br/>  


2. Worker thread 사용할 때😁

```jsx
const { Worker, isMainThread, parentPort, workerData } = require('worker_threads');

const min = 2;
let primes = [];

function findPrimes(start, range) {
  let isPrime = true;
  const end = start + range;
  for (let i = start; i < end; i++) {
    for (let j = min; j < Math.sqrt(end); j++) {
      if (i !== j && i % j === 0) {
        isPrime = false;
        break;
      }
    }
    if (isPrime) {
      primes.push(i);
    }
    isPrime = true;
  }
}

if (isMainThread) {
  const max = 10000000;
  const threadCount = 8;
  const threads = new Set();
  const range = Math.ceil((max - min) / threadCount);
  let start = min;
  console.time('prime');
  for (let i = 0; i < threadCount - 1; i++) {
    const wStart = start;
    threads.add(new Worker(__filename, { workerData: { start: wStart, range } }));
    start += range;
  }
  threads.add(new Worker(__filename, { workerData: { start, range: range + ((max - min + 1) % threadCount) } }));
  for (let worker of threads) {
    worker.on('error', (err) => {
      throw err;
    });
    worker.on('exit', () => {
      threads.delete(worker);
      if (threads.size === 0) {
        console.timeEnd('prime');
        console.log(primes.length);
      }
    });
    worker.on('message', (msg) => {
      primes = primes.concat(msg);
    });
  }
} else {
  findPrimes(workerData.start, workerData.range);
  parentPort.postMessage(primes);
}
```

![](/images/05c4ed89-7a43-4cb8-90f0-bbb553fb66f2-image.png)


- 8개의 스레드 사용해서 1.7초 만에 가능!
- 각 워커가 어떤 일을 할 지 분배 해 주어야 함
- 스레드 많이 쓴다고 무조건 빨라 지는 건 아님


<br/>  

# child process

- 노드에서 다른 프로그램을 실행하고 싶거나 명령어를 수행하고 싶을 때 사용
    - 현재 노드 프로세스 외에 새로운 프로세스를 띄워서 명령 수행
    - 명령 프롬프트 명령어인 `dir`을 노드를 통해 실행 가능 (리눅스는 `ls`)

```jsx
const exec = require('child_process').exec;

var process = exec('dir');

process.stdout.on('data', function(data) {
  console.log(data.toString());
}); // 실행 결과

process.stderr.on('data', function(data) {
  console.error(data.toString());
}); // 실행 에러
```

<br/>  

## 파이썬 프로그램 실행하기

```jsx
const spawn = require('child_process').spawn;

var process = spawn('python', ['test.py']);

process.stdout.on('data', function(data) {
  console.log(data.toString());
}); // 실행 결과

process.stderr.on('data', function(data) {
  console.error(data.toString());
}); // 실행 에러
```

- 파이썬3가 설치되어 있으면, test.py를 실행함


<br/>  


[인프런 Node.js 강의](https://www.inflearn.com/course/%EB%85%B8%EB%93%9C-%EA%B5%90%EA%B3%BC%EC%84%9C/dashboard)
Zerocho 님의 "Node.js 교과서 - 기본부터 프로젝트 실습까지" 강의를 기반으로 작성한 문서입니다. 
