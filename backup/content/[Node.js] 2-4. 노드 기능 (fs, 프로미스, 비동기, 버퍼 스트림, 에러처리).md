---
title: "[Node.js] 2-4. 노드 기능 (fs, 프로미스, 비동기, 버퍼 스트림, 에러처리)"
description: "파일 시스템에 접근하는 모듈콜백방식 대신 프로미스 방식으로 사용 가능!fs 로 파일 쓰고 읽기fs 프로미스로 파일 쓰고 읽기실행할 때마다 순서 매번 다름노드에서는 대부분 이 두 가지 방식임  비동기- 논블로킹 방식이 효율적이나, 매번 실행 순서가 달라짐콜백형식을 유지하"
date: 2022-04-29T13:18:13.575Z
tags: []
---
# 파일 시스템 사용하기

## fs

- 파일 시스템에 접근하는 모듈
    
    ```jsx
    const fs = require('fs');
    
    fs.readFile('./readme.txt', (err, data) => {
      if (err) {
        throw err;
      }
      console.log(data);
      console.log(data.toString());
    });
    ```
    
<br/>  

## fs 프로미스 (추천👍)

- 콜백방식 대신 프로미스 방식으로 사용 가능!
    
    ```jsx
    const fs = require('fs').promises;
    
    fs.readFile('./readme.txt')
      .then((data) => {
        console.log(data);
        console.log(data.toString());
      })
      .catch((err) => {
        console.error(err);
      });
    ```
    
<br/>  

## fs로 파일 만들기

- fs 로 파일 쓰고 읽기
  ```jsx
  const fs = require('fs');

  fs.writeFile('./writeme.txt', '글이 입력됩니다', (err) => {
    if (err) {
      throw err;
    }
    fs.readFile('./writeme.txt', (err, data) => {
      if (err) {
        throw err;
      }
      console.log(data.toString());
    });
  });
  ```
  
<br/>  

- fs 프로미스로 파일 쓰고 읽기

  ```jsx
  const fs = require('fs').promises;

  fs.writeFile('./writeme.txt', '글이 입력됩니다')
    .then(() => {
      return fs.readFile('./writeme.txt');
    })
    .then((data) => {
      console.log(data.toString());
    })
    .catch((err) => {
      console.error(err);
    });
  ```



<br/>  

## 동기 메서드와 비동기 메서드

```jsx
const fs = require('fs');

console.log('시작');
fs.readFile('./readme2.txt', (err, data) => {
  if (err) {
    throw err;
  }
  console.log('1번', data.toString());
});
fs.readFile('./readme2.txt', (err, data) => {
  if (err) {
    throw err;
  }
  console.log('2번', data.toString());
});
fs.readFile('./readme2.txt', (err, data) => {
  if (err) {
    throw err;
  }
  console.log('3번', data.toString());
});
console.log('끝');
```

![](/images/3b6cacf5-bace-45d4-a175-e5de913bc14f-image.png)

- 실행할 때마다 순서 매번 다름


<br/>  


- 노드에서는 대부분 이 두 가지 방식임
    
    ![](/images/36915b00-8a2b-4b7f-882b-674f9651bb53-image.png)
    
    - 비동기- 논블로킹 방식이 효율적이나, 매번 실행 순서가 달라짐

<br/>  


## 비동기 메서드로 순서 유지하기

- 콜백형식을 유지하는 방법 ☹
    
    ```jsx
    const fs = require('fs');
    
    console.log('시작');
    fs.readFile('./readme2.txt', (err, data) => {
      if (err) {
        throw err;
      }
      console.log('1번', data.toString());
      fs.readFile('./readme2.txt', (err, data) => {
        if (err) {
          throw err;
        }
        console.log('2번', data.toString());
        fs.readFile('./readme2.txt', (err, data) => {
          if (err) {
            throw err;
          }
          console.log('3번', data.toString());
          console.log('끝');
        });
      });
    });
    ```
    

  ![](/images/ca7499f1-8db6-4028-a5a2-1d842ed1ae35-image.png)
- 코드가 우측으로 너무 들어가는 현상 발생 (콜백 헬)


<br/>  

- **프로미스로 극복😁**
    
    ```jsx
    const fs = require('fs').promises;
    
    console.log('시작');
    fs.readFile('./readme2.txt')
      .then((data) => {
        console.log('1번', data.toString());
        return fs.readFile('./readme2.txt');
      })
      .then((data) => {
        console.log('2번', data.toString());
        return fs.readFile('./readme2.txt');
      })
      .then((data) => {
        console.log('3번', data.toString());
        console.log('끝');
      })
      .catch((err) => {
        console.error(err);
      });
    ```
    
    - 순서도 지키면서 다같이 백 그라운드로 가 동시 성도 지킬 수 있음

<br/>  
<br/>  


# 버퍼와 스트림

![](/images/7df1caa3-a514-4c9c-ac09-0a169054101d-image.png)

- 버퍼 : 일정한 크기로 모아두는 데이터
    - 일정한 크기가 되면 한번에 처리
    - 버퍼링: 버퍼에 데이터가 찰 때까지 모으는 작업
   
 <br/>  
  
![](/images/0958670c-39cc-46b9-b838-e0ae315a8df7-image.png)

- 스트림(👍) : 데이터의 흐름
    - 일정한 크기로 나눠서 여러 번에 걸쳐 처리
    - 버퍼(또는 chunk)의 크기를 작게 만들어서 주기적으로 데이터 전달
    - 스트림 방식은 서버의 메모리를 적게 차지하면서 효율적으로 데이터를 보낼 수 있음

<br/>  

## 버퍼 사용하기

- 노드에서는 Buffer 객체 사용
    
    ```jsx
    const buffer = Buffer.from('저를 버퍼로 바꿔보세요');
    console.log('from():', buffer);
    console.log('length:', buffer.length);
    console.log('toString():', buffer.toString());
    
    const array = [Buffer.from('띄엄 '), Buffer.from('띄엄 '), Buffer.from('띄어쓰기')];
    const buffer2 = Buffer.concat(array); //버퍼 합침
    console.log('concat():', buffer2.toString());
    
    const buffer3 = Buffer.alloc(5);
    console.log('alloc():', buffer3);
    ```
    
<br/>  

## 파일 읽는 스트림 사용하기

- fs.createReadStream
    
    ```jsx
    const fs = require('fs');
    
    const readStream = fs.createReadStream('./readme3.txt', { highWaterMark: 16 });
    const data = [];
    
    readStream.on('data', (chunk) => {
      data.push(chunk);
      console.log('data :', chunk, chunk.length);
    });
    
    readStream.on('end', () => {
      console.log('end :', Buffer.concat(data).toString());
    });
    
    readStream.on('error', (err) => {
      console.log('error :', err);
    });
    ```
    
    ![](/images/f3a7d55d-c0f3-41a3-a9c0-c4bd4a09345d-image.png)

    
    - stream은 순서대로 옴 (동시 X)
    - `readStream` 은 비동기 이기 때문에 에러 처리를 해 주는 것이 좋음
    - `{ highWaterMark: 16 }` 를 해 줌으로써 한번에 읽는 크기를 지정 (버퍼 크기, 바이트 단위, 기본 64kB) ⇒ 메모리 절약 가능
    
<br/>  

## 파일 쓰는 스트림 사용하기

- fs.createWriteStream
    
    ```jsx
    const fs = require('fs');
    
    const writeStream = fs.createWriteStream('./writeme2.txt');
    writeStream.on('finish', () => {
      console.log('파일 쓰기 완료');
    });
    
    writeStream.write('이 글을 씁니다.\n');
    writeStream.write('한 번 더 씁니다.');
    writeStream.end();
    ```
    
    - `write`로 chunk 입력, `end`로 스트림 종료
    - 스트림 종료 시  finish  이벤트 발생
    
<br/>  
<br/>  

# Pipe와 스트림 메모리 효율 확인

## 스트림 사이에 pipe 사용하기

- pipe로 여러 개의 스트림을 이을 수 있음
    
    ```jsx
    const fs = require('fs');
    
    // 읽어서 쓰기 (복사 너낌~)
    const readStream = fs.createReadStream('readme4.txt');
    const writeStream = fs.createWriteStream('writeme3.txt');
    readStream.pipe(writeStream);
    ```
    
<br/>  


## 여러개의 스트림 연결하기

- pipe로 여러 개의 스트림을 이을 수 있음
    
    ```jsx
    //파일을 압축한 후 복사하는 예제
    const zlib = require('zlib'); //zlib 내장 모듈 사용
    const fs = require('fs');
    
    const readStream = fs.createReadStream('./readme4.txt');
    const zlibStream = zlib.createGzip();
    const writeStream = fs.createWriteStream('./readme4.txt.gz');
    readStream.pipe(zlibStream).pipe(writeStream);
    ```
    
<br/>  

## 큰 파일 만들기

- 1GB 정도의 파일을 만들어 봄.
    
    ```jsx
    const fs = require('fs');
    const file = fs.createWriteStream('./big.txt');
    
    for (let i = 0; i <= 10000000; i++) {
      file.write('안녕하세요. 엄청나게 큰 파일을 만들어 볼 것입니다. 각오 단단히 하세요!\n');
    }
    file.end();
    ```
    
    - `createWriteStream`으로 만들어야 메모리 문제가 생기지 않음
    
<br/>  

## 버퍼와 스트림방식 메모리 사용량 비교

- 버퍼방식 ☹

![](/images/a6d18875-505f-4c56-8ae4-3e90f6d6a8de-image.png)

- 서버 메모리 사용량이 1GB가 넘어감!!!☹
- 이러면 서버 금방 터져버림 !!!


<br/>  

- 스트림 방식 👍

![](/images/0ef062bc-fc06-4383-9e9d-fad3cfcecab5-image.png)


- 메모리를 효율적으로 사용 가능

<br/>  

## 기타 fs 메서드

- 파일 및 폴더 생성
    
    ```jsx
    const fs = require('fs');
    
    fs.access('./folder', fs.constants.F_OK | fs.constants.R_OK | fs.constants.W_OK, (err) => {
      if (err) {
        if (err.code === 'ENOENT') {
          console.log('폴더 없음');
          fs.mkdir('./folder', (err) => {
            if (err) {
              throw err;
            }
            console.log('폴더 만들기 성공');
            fs.open('./folder/file.js', 'w', (err, fd) => {
              if (err) {
                throw err;
              }
              console.log('빈 파일 만들기 성공', fd);
              fs.rename('./folder/file.js', './folder/newfile.js', (err) => {
                if (err) {
                  throw err;
                }
                console.log('이름 바꾸기 성공');
              });
            });
          });
        } else {
          throw err;
        }
      } else {
        console.log('이미 폴더 있음');
      }
    });
    ```
    
<br/>  

- 폴더 내용 확인 및 삭제
    
    ```jsx
    const fs = require('fs');
    
    fs.readdir('./folder', (err, dir) => {
      if (err) {
        throw err;
      }
      console.log('폴더 내용 확인', dir);
      fs.unlink('./folder/newfile.js', (err) => {
        if (err) {
          throw err;
        }
        console.log('파일 삭제 성공');
        fs.rmdir('./folder', (err) => {
          if (err) {
            throw err;
          }
          console.log('폴더 삭제 성공');
        });
      });
    });
    ```
    

- 파일을 복사하는 방법
    
    ```jsx
    const fs = require('fs');
    
    fs.copyFile('readme4.txt', 'writeme4.txt', (error) => {
      if (error) {
        return console.error(error);
      }
      console.log('복사 완료');
    });
    ```
    

- 파일을 감시하는 방법 (변경 사항 발생 시 이벤트 호출)
    
    ```jsx
    const fs = require('fs');
    
    fs.watch('./target.txt', (eventType, filename) => {
      console.log(eventType, filename);
    });
    ```
    

<br/>  
<br/>  


# 스레드 풀과 커스텀 이벤트

- `fs`, `crypto`, `zlib` 모듈의 메서드를 실행할 때는 백그라운드에서 동시에 실행 됨 (스레드풀이 동시에 처리해줌)

```jsx
const crypto = require('crypto');

const pass = 'pass';
const salt = 'salt';
const start = Date.now();

crypto.pbkdf2(pass, salt, 1000000, 128, 'sha512', () => {
  console.log('1:', Date.now() - start);
});

crypto.pbkdf2(pass, salt, 1000000, 128, 'sha512', () => {
  console.log('2:', Date.now() - start);
});

crypto.pbkdf2(pass, salt, 1000000, 128, 'sha512', () => {
  console.log('3:', Date.now() - start);
});

crypto.pbkdf2(pass, salt, 1000000, 128, 'sha512', () => {
  console.log('4:', Date.now() - start);
});

crypto.pbkdf2(pass, salt, 1000000, 128, 'sha512', () => {
  console.log('5:', Date.now() - start);
});

crypto.pbkdf2(pass, salt, 1000000, 128, 'sha512', () => {
  console.log('6:', Date.now() - start);
});

crypto.pbkdf2(pass, salt, 1000000, 128, 'sha512', () => {
  console.log('7:', Date.now() - start);
});

crypto.pbkdf2(pass, salt, 1000000, 128, 'sha512', () => {
  console.log('8:', Date.now() - start);
});
```

![](/images/0623a50a-0d2e-4c3a-a723-ec8c09a3ff5d-image.png)


<br/>  


## UV_THREAD_SIZE

![](/images/615224e7-912a-4185-95c9-99df6cd9a68e-image.png)


- 스레드풀을 직접 컨트롤할 수는 없지만 개수 조절은 가능
    
    ```jsx
    SET UV_THREADPOOL_SIZE = 개수 // 윈도우
    UV_THREADPOOL_SIZE = 개수 // 맥, 리눅스
    ```
    
    - 코어 개수에 맞게 적절히 조절!
    

<br/>  

## 커스텀 이벤트

```jsx
const EventEmitter = require('events');

const myEvent = new EventEmitter();
myEvent.addListener('event1', () => {
  console.log('이벤트 1');
});
myEvent.on('event2', () => {
  console.log('이벤트 2');
});
myEvent.on('event2', () => {
  console.log('이벤트 2 추가');
});
myEvent.once('event3', () => {
  console.log('이벤트 3');
}); // 한 번만 실행됨

myEvent.emit('event1'); // 이벤트 호출
myEvent.emit('event2'); // 이벤트 호출

myEvent.emit('event3');
myEvent.emit('event3'); // 실행 안 됨

myEvent.on('event4', () => {
  console.log('이벤트 4');
});
myEvent.removeAllListeners('event4');
myEvent.emit('event4'); // 실행 안 됨

const listener = () => {
  console.log('이벤트 5');
};
myEvent.on('event5', listener);
myEvent.removeListener('event5', listener);
myEvent.emit('event5'); // 실행 안 됨

console.log(myEvent.listenerCount('event2'));
```

![](/images/84c4b208-4807-420d-8a28-0ffd44e51d58-image.png)


- 이벤트 3는 한번만 실행!

- `myEvent.removeAllListeners('event4')` 하면 event4에 엮여있는 모든 콜백 함수가 다 지워 짐

- 하나만 지우려면 콜백 함수까지 인자로 넣어줌 `myEvent.removeListener('event5', listener)`


<br/>  
<br/>  

# 에러 처리하기

- 자바스트립트에서는 에러랑 예외를 따로 구분하지 않음, 필수로 해줘야함
- 예외(Exception) : 처리하지 못한 에러
- fs 같은 것들은 에러 나도 멈추지는 않아서 try -catch로 해줄 필요는 없음
    - 노드가 기본으로 제공하는 비동기 메서드의 콜백 에러는 노드 프로세스를 멈추진 않음

## Try-catch 문

```jsx
setInterval(() => {
  console.log('시작');
  try {
    throw new Error('서버를 고장내주마!');
  } catch (err) {
    console.error(err);
  }
}, 1000);
```

![](/images/8c68ba00-4e30-4eb7-bed6-b035b6ed0090-image.png)



<br/>  

## 노드 비동기 메서드의 에러

- 노드 비동기 메서드의 에러는 따로 처리하지 않아도 됨, 콜백 함수에서 에러 객체 제공

```jsx
const fs = require('fs');

setInterval(() => {
  fs.unlink('./abcdefg.js', (err) => {
    if (err) {
      console.error(err);
    }
  });
}, 1000);
```

![](/images/80009a4a-0000-4224-9b4a-5f9f9b069d1d-image.png)



<br/>  

## 프로미스의 에러

- 프로미스의 에러는 따로 처리하지 않아도 됨
- 버전이 올라가면 동작이 바뀔 수 있음

```jsx
const fs = require('fs').promises;

setInterval(() => {
  fs.unlink('./abcdefg.js')
}, 1000);
```

![](/images/2cfd15e3-b8a4-4e62-85b7-f230be5f54e7-image.png)


- 프로미스에 catch 안 붙이면 Warning 뜸
    
    (웬만하면 **catch 붙여라**!)
    
<br/>  

## 예측 불가능한 에러 처리하기

- 모든 코드를 try-catch로 감싸기엔 너무 번거로움
- 최후의 수단 (에러를 한방에 처리할 수 있음)
    - 콜백함수의 동작이 보장되지 않음
    - 따라서 복구 작업용으로 쓰는 것은 부적합 (에러 내용 기록용으로만 쓰는 게 좋음)

```jsx
process.on('uncaughtException', (err) => {
  console.error('예기치 못한 에러', err);
});

setInterval(() => {
  throw new Error('서버를 고장내주마!');
}, 1000);

setTimeout(() => {
  console.log('실행됩니다');
}, 2000);
```

- 에러가 프로세스를 멈추지 않고 코드가 실행됨
- 그냥 넘어가면 안되고 복구 코드를 여기에 작성해 둬도 안되고 고쳐야 함... 그냥 기록 용으로 쓰셈
- 에러가 발생하면 코드를 고치고 프로세스를 재 시작 하는 게 베스트 임!

<br/>  

## 프로세스 종료하기

- 윈도우

![](/images/50c57cf7-2046-4c71-a966-d3216634a63a-image.png)


- 맥/리눅스

![](/images/a71f0629-2e49-4ddc-8ce0-fac38c0e43cc-image.png)


<br/>  


[인프런 Node.js 강의](https://www.inflearn.com/course/%EB%85%B8%EB%93%9C-%EA%B5%90%EA%B3%BC%EC%84%9C/dashboard)
Zerocho 님의 "Node.js 교과서 - 기본부터 프로젝트 실습까지" 강의를 기반으로 작성한 문서입니다. 