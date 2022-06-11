---
title: "[Node.js] 2-1. 노드 기능 (모듈, 객체, 순환참조)"
description: "자바스크립트는 스크립트 언어라서 즉석에서 코드 실행 가능REPL이라는 콘솔 제공 (Read, Evaluate, Print, Loop)  cmd에서 실행 가능  ctrl+(백틱)\`하면 터미널 창 띄움노드는 자바스크립트 코드를 모듈로 만들 수 있음  (모듈: 특정한 기능"
date: 2022-04-29T12:41:07.557Z
tags: []
---
# 1. REPL와 js 파일 실행하기

## (1). REPL

- 자바스크립트는 스크립트 언어라서 즉석에서 코드 실행 가능
    - REPL이라는 콘솔 제공 (Read, Evaluate, Print, Loop)
        
        ![](/images/1f8045bc-088c-40eb-9f01-2353b316da04-image.png)

        
    - cmd에서 실행 가능
        
        ![](/images/3c0470d5-1a24-4876-b064-844fbd5b287d-image.png)

        
<br/>  

## (2). JS 파일 만들어 실행

- `ctrl+`(백틱)`하면 터미널 창 띄움

<br/>  
<br/>  

# 2. 모듈 만들기

## (1). 모듈

- 노드는 자바스크립트 코드를 모듈로 만들 수 있음
    
    (모듈: 특정한 기능을 하는 함수나 변수들의 집합)
    
    ![](/images/e9d3a18f-f681-4d43-9e3a-946e6a70f83a-image.png)

    
<br/>  

## (2). 모듈 만들어보기

- 같은 폴더 내에 .js파일 생성
    - 파일 끝에 `module.exports` 로 모듈로 만들 값을 지정
    - 다른 파일에서 `require(파일경로)` 로 그 모듈의 내용 가져올 수 있음
    

```jsx
// index.js
const { odd, even } = require('./var');
const checkNumber = require('./func');

function checkStringOddOrEven(str) {
  if (str.length % 2) { // 홀수면
    return odd;
  }
  return even;
}

console.log(checkNumber(10));
console.log(checkStringOddOrEven('hello'));
```

<br/>  

```jsx
// var.js
const odd = '홀수입니다';
const even = '짝수입니다';

module.exports = {
  odd,
  even,
};
```

<br/>  


```jsx
//func.js
const { odd, even } = require('./var');

function checkOddOrEven(num) {
  if (num % 2) { // 홀수면
    return odd;
  }
  return even;
}

module.exports = checkOddOrEven;
```

<br/>  


## (3). 파일 간의 모듈 관계

![](/images/a72ac00f-57ec-4f86-97a7-cd649bc61ae2-image.png)


<br/>  

## (4). 자바스크립트 자체 모듈 시스템 문법

- 아직 노드에서의 지원은 완벽하지 않아 `.mjs` 확장자를 사용해야 함.
- `require` 대신 `import` / `module.exports` 대신 `export default`를 쓰는 것으로 바뀜
    - 완전히 같은 것은 아닌데 유사함!

```jsx
import { odd, even } from './var';

function checkOddOrEven(num) {
  if (num % 2) { // 홀수면
    return odd;
  }
  return even;
}

export default checkOddOrEven;
```

<br/>  
<br/>  

# 3. global 과 콘솔, 타이머

## (1). global

- 노드의 전역 객체
    - 브라우저의 window 같은 역할
    - 모든 파일에서 접근 가능
    - 생략도 가능(console, require도 global 의 속성)
    
    ![](/images/18b742f8-c09a-40ef-ac0e-01d93fd0014e-image.png)

    
<br/>  

## (2). global 속성 공유

- global 속성에 값을 대입하면 다른 파일에서도 사용 가능 (권장❌)

```jsx
const A = require('./globalA');

global.message = '안녕하세요';
console.log(A());
```

- 파일이 많아졌을 때 global에 값을 대입하여 쓰면 관리가 어렵기 때문에 그냥 모듈 쓰셈

<br/>  


## (3). console 객체

```jsx
const string = 'abc';
const number = 1;
const boolean = true;
const obj = {
  outside: {
    inside: {
      key: 'value',
    },
  },
};
console.time('전체시간'); // 👉 시간 로깅

// 👉 평범한 로그
console.log('평범한 로그입니다 쉼표로 구분해 여러 값을 찍을 수 있습니다');
console.log(string, number, boolean); 

console.error('에러 메시지는 console.error에 담아주세요');// 👉 에러 로깅

console.table([{ name: '제로', birth: 1994 }, { name: 'hero', birth: 1988}]);

// 👉 객체 로깅
console.dir(obj, { colors: false, depth: 2 });
console.dir(obj, { colors: true, depth: 1 });

console.time('시간측정');
for (let i = 0; i < 100000; i++) {}
console.timeEnd('시간측정');

function b() {
  console.trace('에러 위치 추적'); // 👉 호출스택 로깅 (함수 안에서 사용)
}
function a() {
  b();
}
a();

console.timeEnd('전체시간'); // 👉 시간 로깅
```

<br/>  


## (4). 타이머 메서드

- `set` 메서드에 `clear` 메서드가 대응됨
    - set 메서드의 리턴 값(아이디)을 clear 메서드에 넣어 취소
    
    - setTimeout(콜백함수, 밀리초): 주어진 밀리 초 이후에 콜백 함수 실행
    - setInterval(콜백함수, 밀리초): 주어진 밀리 초마다 콜백 함수를 반복 실행
    - setImmediate(콜백함수): 콜백함수를 즉시 실행
    
    - clearTimeout(아이디): setTimeout을 취소
    - clearInterval(아이디): setInterval을 취소
    - clearImmediate(아이디): setImmediate를 취소
    
- 타이머 예제

```jsx
// timer.js
const timeout = setTimeout(() => { // 변수에 저장을 해 두어야 나중에 취소 가능
  console.log('1.5초 후 실행');
}, 1500);

const interval = setInterval(() => {
  console.log('1초마다 실행');
}, 1000);

const timeout2 = setTimeout(() => {
  console.log('실행되지 않습니다');
}, 3000);

setTimeout(() => {
  clearTimeout(timeout2);
  clearInterval(interval);
}, 2500);

const immediate = setImmediate(() => {
  console.log('즉시 실행');
});
// setImmediate() 바로 실행 되지만 취소도 가능함
// (백그라운드 -> 태스크 큐 -> 호출스택으로 가기 때문에 이 전에 취소 가능 )

const immediate2 = setImmediate(() => {
  console.log('실행되지 않습니다');
});

clearImmediate(immediate2);
```

<br/>  

<br/>  


# export 와 this

## (1). _filename, _dirname

- 노드는 파일 시스템에 접근 가능

```jsx
console.log(__filename); // 현재파일 경로 출력
console.log(__dirname); // 현재 폴더(디렉토리) 경로
```

<br/>  


## (2). module, exports

```jsx
//셋이 같음
module.exports === export === {}

// 한가지만 export 할 때
module.exports = 함수호출; //하면 module.exports !== export

//두가지이상 export 할 때

	// 방법1
exports.odd =odd;
exports.even =even;

	// 방법2
module.exports = {
	odd,
	even
}
```

- 방법1로 한 뒤에 방법 2에서 바꿔버리면 참조 관계 끊김
    
    ⇒ `exports` 랑 `module.exports` 는 같이 쓸 수 ❌
    

<br/>  

## (3). this

- 노드에서 this를 사용할 때 주의 점 ⚠
    - 최상위 스코프의 this는 module.exports를 가리킴
    - 그 외에는 브라우저의 자바스크립트와 동일
    - 함수 선언문 내부의 this는 global 객체를 가리킴
    
    ```jsx
    console.log(this);
    console.log(this === module.exports);
    console.log(this === exports);
    
    function whatIsThis() {
      console.log('function', this === exports, this === global);
    }
    whatIsThis();
    ```
    
<br/>  

<br/>  

# 모듈심화, 순환 참조

## (1). require의 특성

```jsx
console.log('require가 가장 위에 오지 않아도 됩니다.');

module.exports = '저를 찾아보세요.';

require('./var'); // 파일을 실행만 하고싶을 때

console.log('require.cache입니다.');
console.log(require.cache);
console.log('require.main입니다.');
console.log(require.main === module);
console.log(require.main.filename);
```

- require가 제일 위에 올 필요는 없음
- require.cache 에 한 번 require한 모듈에 대한 캐싱 정보가 들어 있음
- require.main은 노드 실행 시 첫 모듈을 가리킴


<br/>  


## (2). 순환참조

⚠ **두 개의 모듈이 서로를 require 하는 상황을 조심해야 함!!**

```jsx
const dep2 = require('./dep2');
console.log('require dep2', dep2);
module.exports = () => {
  console.log('dep2', dep2);
};
```

```jsx
const dep1 = require('./dep1');
console.log('require dep1', dep1);
module.exports = () => {
  console.log('dep1', dep1);
};
```

- dep1와 dep2는 서로 require함
- dep1의 module.exports가 함수가 아니라 빈 객체가 됨 (무한 반복을 막기 위해 의도 됨)

![](/images/50e01ce3-3841-4deb-8953-2b86177237db-image.png)


- `setImmediate` 와 `setTimeout` 중 뭐가 먼저 실행될 지는 환경에 따라 다름




<br/>  


[인프런 Node.js 강의](https://www.inflearn.com/course/%EB%85%B8%EB%93%9C-%EA%B5%90%EA%B3%BC%EC%84%9C/dashboard)
Zerocho 님의 "Node.js 교과서 - 기본부터 프로젝트 실습까지" 강의를 기반으로 작성한 문서입니다. 