---
title: "[Node.js] 5-2. 익스프레스 웹 서버 만들기 (미들웨어, 템플릿 엔진)"
description: "자주 사용하는 미들웨어  morgan  서버로 들어온 요청과 응답을 기록해주는 미들웨어     로그의 자세한 정도 선택 가능(dev, tiny, short, common, combined)     개발 환경에서는 dev, 배포환경에서는 combined를 애용      "
date: 2022-04-29T14:19:57.950Z
tags: []
---
# 자주 사용하는 미들웨어

<br/>  

## morgan

- 서버로 들어온 요청과 응답을 기록해주는 미들웨어
    - 로그의 자세한 정도 선택 가능(dev, tiny, short, common, combined)
    - 개발 환경에서는 `dev`, 배포환경에서는 `combined`를 애용
        - combined는 정확한 시간까지 나옴
    
    ```jsx
    const morgan = require('morgan');
    
    app.use(morgan('dev'));
    ```
    

![](/images/610e3be0-ee01-432a-a541-e5440ed80c02-image.png)


- 순서대로
`HTTP요청 요청주소 상태코드 응답속도 - 응답바이트`


<br/>  

## cookieParser

- 요청 헤더의 쿠키를 해석해주는 미들웨어
    - `parseCookies` 함수와 기능 비슷
    - `res.cookies` 안에 쿠키들이 들어있음
    - 비밀 키로 쿠키 뒤에 서명을 붙여 내 서버가 만든 쿠키임을 검증할 수 있음

![](/images/0cd249b6-a130-48be-9934-ad18ca6f71d6-image.png)

```jsx
    const cookieParser = require('cookie-parser');

    app.use(cookieParser(process.env.COOKIE_SECRET));
```
    
<br/>  

- 사용 예제 (쿠키관련 조작들이 편리해짐)
    
    ```jsx
    app.get('/', (req, res, next) => {
    
    	 //쿠키 설정
    	 res.cookie('name', encodeURIComponent(name), {
    		expires: new Date(),
    		httpOnly:true,
    		path: '/',
    		})
    
    	//쿠키 지우기
    	res.clearCookie('name', encodeURIComponent(name), 
    			httpOnly:true,
    			path: '/',
    			})
    		res.sendFile(path.join(__dirname, 'index.html'));
    });
    ```
    
    - `writeHead` 문자열로 쭉 쓰는 것 보다 `method` 사용해 더 편리

<br/>  

- 쿠키의 암호화(서명)
    
    ```jsx
    app.use(cookieParser('암호화키'));
    
    app.get('/', (req, res, next) => {
    	 res.signedCookies;
    ...
    ```
    
    - 이때는 `signedCookies`라고 써 주어야 함

<br/>  

## bodyParser (옛날 사람이 쓰는 거임 😫)

- express에 들어가서 따로 require 해 주지 않아도 됨
- 요청의 본문을 해석해주는 미들웨어
    - 폼 데이터나 AJAX 요청의 데이터 처리
    - `json` 미들웨어는 요청 본문이 `json`인 경우 해석, `urlencoded` 미들웨어는 폼 요청 해석
    - `put`이나 `patch`, `post` 요청 시에 `req.body`에 프런트에서 온 데이터를 넣어 줌
    
- `qs`가 `querystring` 보다 훨씬 강력하니까 `true`로 쓰는 것을 추천 👍
    
    ```jsx
    app.use(express.json());
    app.use(express.urlencoded({ extended: false }));
    //true면 qs모듈사용, false이면 querystring모듈 사용
    ```
    

- name 가져오는 것에 대한 http와 express의 비교

```jsx
// 요청의 body를 다 받은 후 실행됨
return req.on('end', () => {
  console.log('POST 본문(Body):', body);
  const { name } = JSON.parse(body);
  const id = Date.now();
  users[id] = name;
  res.writeHead(201, { 'Content-Type': 'text/plain; charset=utf-8' });
  res.end('ok');
});
```

```jsx
app.get('/', (req, res, next) => {

  req.body.name ~~~
 //그냥 이렇게 접근해서 사용 가능함!

});
```

- express에 안 들어있는 bodyParser의 두 가지 (잘 쓰진 않음)

```jsx
app.use(bodyParser.raw()); //바이너리 데이터
app.use(bodyParser.text()); //문자열
```

<br/>  

## static

- 정적인 파일을 제공하는 미들웨어
    - 인수로 정적 파일의 경로를 제공
    - 파일이 있을 때 fs.readFile로 직접 읽을 필요 없음
    - 요청하는 파일이 없으면 알아서 next를 호출해 다음 미들웨어로 넘어감
    - 파일을 발견했다면 다음 미들웨어는 실행되지 않음
    
- `app.use('요청경로', express.static('실제경로');` 를 통해 경로 유출 방지 (보안에 도움)
    
    ```jsx
    app.use('/', express.static(path.join(__dirname, 'public-3030')); //예시
    ```
    
- 거의 모든 미들웨어는 내부적으로 `next`를 실행하는데 `static`은 파일을 못 찾으면 종료되어야 하기 때문에 `morgan` 아래에 써 주는 것이 좋음
    
    ```jsx
    app.use(morgan('dev'));
    app.use('/', express.static(path.join(__dirname, 'public'))); //요기!
    app.use(express.json());
    app.use(express.urlencoded({ extended: false }));
    ```
    
    - 필요에 따라 쿠키와 세션 이후에 static 줄 수도 있음
    - 미들웨어들 간의 순서 중요함!

<br/>  

## express-session

- 세션 관리용 미들웨어
    
    ```jsx
    app.use(session({
      resave: false,
      saveUninitialized: false,
      secret: process.env.COOKIE_SECRET,
      cookie: {
        httpOnly: true, //true해야 js공격 안당함
        secure: false,
      },
      name: 'connect.sid', //기본값은 이거임
    }));
    ```
    
    ```jsx
    req.session.name = 'seohyun'; //세션등록
    req.sessionID; //세션 아이디 확인
    req.session.destroy(); //세션 모두 제거
    ```
    
    - `resave`: 요청이 왔을 때 세션에 수정 사항이 생기지 않아도 다시 저장할 지 여부
    - `saveUninitialized`: 세션에 저장할 내역이 없더라도 세션을 저장할지
    - `req.session.save`로 수동 저장도 가능하지만 할 일 거의 없음
    

<br/>  

## 미들웨어간 데이터 전달하기

- `req`나 `res` 객체 안에 값을 넣어 데이터 전달 가능
    
    ```jsx
    app.use((req, res, next) => {
    	req.data = '데이터 넣기';
    	next();
    	}, (req, res, next) => {
    		console.log(req.data); //데이터 받기
    		next();
    });
    ```
    
    - `app.set`과의 차이점: `app.set`은 서버 내내 유지 / `req`, `res`는 요청 하나 동안만 유지
    - `req.body`나 `req.cookies`같은 미들웨어의 데이터와 겹치지 않게 조심

- 주의 사항 🚨
    - 이렇게 변수를 넣으면 사용자에게 알려질 수 있어 위험해 🚨
        
        ```jsx
        let password;
        app.use((req, res, next) => {
        	password = '알려지면 안되는 비밀번호';
        });
        
        app.get('/', (req, res, next) => {
        	console.log(password); // 안돼!!!
        	res.sendFile(path.join(__dirname, 'index/html'));
        });
        ```
        
    
    - 이건 더 위험해!!  🚨
        
        ```jsx
        app.use((req, res, next) => {
        	app.set('password', '알려지면 안되는 비밀번호');
        });
        
        app.get('/', (req, res, next) => {
        	app.get('password'); // 진쨔로 큰일남...!
        	res.sendFile(path.join(__dirname, 'index/html'));
        });
        ```
        

- 요청 한번만 할 때는 `req.data`
    
    ```jsx
    app.use((req, res, next) => {
    	req.data = '비밀번호';
    });
    
    app.get('/', (req, res, next) => {
    	req.data  //비밀번호
    	res.sendFile(path.join(__dirname, 'index/html'));
    });
    ```
    

- 영구적으로 데이터를 남기고 싶으면 `req.session`
    
    ```jsx
    app.use((req, res, next) => {
    	req.session.data = '비밀번호';
    });
    
    app.get('/', (req, res, next) => {
    	req.session.data // 비번
    	res.sendFile(path.join(__dirname, 'index/html'));
    });
    ```
    
<br/>  

## 미들웨어 확장하기

- 미들웨어 안에 미들웨어를 넣는 방법으로 아래 두 코드는 동일한 역할이고, 다양하게 확장 가능

![](/images/a20f5231-305d-4634-be96-d65618a02c2f-image.png)

![](/images/685b8ac1-e44e-4d01-88ae-bfd3facac722-image.png)


- 미들웨어 확장 예시
    
    ```jsx
    app.use('/', (req, res, next) => {
    	if(req.session.id){
    			express.static(__dirname, 'public') (req, res, next)
    	}else {
    		next();
    	}
    });
    ```
    
    - 로그인을 해서 session에 아이디가 있다면, static 실행하고, 아니면 next 하는 코드
        
        로그인 했다면 사진, 파일 등 데이터를 프론트로 전달
        
    - 내가 만든 미들웨어 안에 남의 미들웨어를 넣고 뒤에 `(req, res, next)` 만 붙여주면 됨
    

<br/>  

## multer

- 멀티파트 데이터 형식
    - form 태그의 `enctype`이  `multipart/form-data`인 경우
        - body-parser로는 요청 본문을 해석할 수 없음
        - `multer` 패키지 필요

![](/images/fe01cc1f-0a8b-479d-a3df-0ff7f41af548-image.png)


![](/images/54797f78-9f12-4d77-b01d-52b4b4618ee4-image.png)


- multer 예제
    
    ```jsx
    const multer = require('multer');
    const fs = require('fs');
    
    try {
      fs.readdirSync('uploads');
    } catch (error) {
      console.error('uploads 폴더가 없어 uploads 폴더를 생성합니다.');
      fs.mkdirSync('uploads');
    }
    
    const upload = multer({
      storage: multer.diskStorage({
        destination(req, file, done) {
          done(null, 'uploads/');
        },
        filename(req, file, done) {
          const ext = path.extname(file.originalname);
          done(null, path.basename(file.originalname, ext) + Date.now() + ext);
    			//데이터가 덮어씌워지는 것을 막고자 현재 시간 date 넣음
        },
      }),
      limits: { fileSize: 5 * 1024 * 1024 },
    });
    
    // 업로드라는 객체를 라우터에 장착
    app.get('/upload', (req, res) => {
      res.sendFile(path.join(__dirname, 'multipart.html'));
    });
    
    app.post('/upload', upload.single('image'), (req, res) => {
      console.log(req.file); // 이미지 1개일때 req.file로 씀
      res.send('ok');
    });
    ```
    
    - `multer` 함수 호출
        - `storage` 는 저장할 공간에 대한 정보 (업로드한 파일 어디에 저장할 거냐~)
            - `diskStorage`는 하드디스크에 업로드 파일을 저장한다는 것
            - `destination`은 저장할 경로
        - `filename`은 저장할 파일명 (파일명+날짜+확장자 형식)
        - `Linits`는 파일 개수나 파일 사이즈를 제한할 수 있음
        
    - multer 미들웨어들
        - single과 none, array, fields 미들웨어 존재
            - `single` : 하나의 파일을 업로드 할 때
                
                ```jsx
                app.post('/upload', upload.single('image'), (req, res) => {
                  console.log(req.file); // 이미지 1개일때 req.file로 씀
                  res.send('ok');
                });
                ```
                
            
            - `none` : 파일을 업로드 하지 않을 때 (이미지는 없는데 enctype이 form-data일 때)
            
            - `array`는 하나의 요청 body 이름 아래 여러 파일이 있는 경우
                
                ```jsx
                app.post('/upload', upload.array('many'), (req, res) => {
                  console.log(req.files, req.body); 
                  res.send('ok');
                });
                ```
                
            
            - `fields` 는 여러 개의 요청 body이름 아래 파일이 하나씩 있는 경우
                
                ```jsx
                app.post('/upload', 
                	upload.fields([{ name : 'image1'}, { name : 'image2'}]), (req, res) => {
                	  console.log(req.files, req.body); 
                	  res.send('ok');
                });
                ```
                
        
        - `req.file` 안에 업로드 정보 저장
            
            ![](/images/bf6c9981-a91c-4564-998b-69967114127e-image.png)

            
<br/>          

## dotenv

- 미들웨어는 아니지만 비밀 키 같은 것들을 관리할 수 있음

- 사용 예제
    
    ```jsx
    app.use(cookieParser(process.env.COOKIE_SECRET));
    
    app.use(session({
      resave: false,
      saveUninitialized: false,
      secret: process.env.COOKIE_SECRET,
      cookie: {
        httpOnly: true,
        secure: false,
      },
      name: 'session-cookie',
    }));
    ```
    
    ```jsx
    COOKIE_SECRET=mycookiesecret
    DB_PASSWORD=mydbpw
    ```
    
    - .env 파일을 읽어서 process.env로 만듦
        - `process.env.COOKIE_SECRET` 에 cookiesecret 값이 할당됨 (키=값 형식)
        - 비밀 키들을 소스코드에 그대로 적어두면 소스 코드가 유출되었을 때 비밀 키도 같이 유출됨
        - .env 파일에 비밀 키들을 모아두고 .env 파일만 잘 관리하면 됨
        
- 최대한 위에서 dotenv 파일을 불러 오는 게 좋음
    
    ```jsx
    const dotenv = require('dotenv');
    dotenv.config();
    ```
    

> 소스 코드에는 절대 비밀 key를 넣지마라. ⇒ dotenv 같은 것을 사용해라
> 
> 
> 2차 적으로 개개인 별로 권한을 다르게 해서 접근 가능한 범위를 나눠라
> 

<br/>  
<br/>  

# Router 분리하기

- app.js 가 길어지는 것을 막을 수 있음

- 사용예시
    
    ```jsx
    const indexRouter = require('./routes');
    const userRouter = require('./routes/user');
    
    app.use('/', indexRouter);
    app.use('/user', userRouter);
    ```
    
    ```jsx
    const express = require('express');
    
    const router = express.Router();
    
    // GET / 라우터
    router.get('/', (req, res) => {
      res.send('Hello, Express');
    });
    
    module.exports = router;
    ```
    
    ```jsx
    const express = require('express');
    
    const router = express.Router();
    
    // GET /user 라우터
    router.get('/', (req, res) => { 
      res.send('Hello, User');
    });
    
    module.exports = router;
    ```
    
<br/>  

## 라우트 매개변수

- userRouter 의 get은 /user 와 / 가 합쳐져서 `GET/user/`가 됨
- `:id`를 넣으면 `req.params.id`로 받을 수 있음
    - 동적으로 변하는 부분을 라우트 매개변수로 만듦 (와일드 카드)
        
        ```jsx
        router.get('/user/:id', function(req, res) => {
        	 console.log(req.params, req.query);
        });
        ```
        
    
    - 일반 라우터보다 뒤에 위치해야 함
        
        ```jsx
        router.get('/user/:id', function(req, res) => {
        	 console.log("얘만 실행됨");
        });
        
        router.get('/user/like', function(req, res) => {
        	 console.log("전혀 실행되지 X");
        });
        ```
        
    
    - `/users/123?limit=5&skip=10` 주소 요청인 경우
        
        ![](/images/f4191d56-9fa6-4a9e-a362-a506a1fb8399-image.png)

        
<br/>      

## 404 미들웨어

- 요청과 일치하는 라우터가 없는 경우를 대비해 404 라우터를 만듦
    
    ```jsx
    app.use((req, res, next) => {
      res.status(404).send('Not Found');
    });
    ```
    
    - 이게 없으면 단순히 Cannot GET 주소라는 문자열이 표시됨

<br/>  

## 라우터 그룹화하기

```jsx
router.get('/abc', (req, res) => {
	 res.send('GET /abc');
});

router.post('/abc', (req, res) => {
	 res.send('POST /abc');
});
```

- 주소는 같지만 메서드가 다른 코드가 있을 때 (근데 이걸 더 많이 씀~)

```jsx
router.route('/abc')
	.get((req, res) => {
		res.send('GET /abc');
	})
  .post((req, res) => {
		res.send('POST /abc');
	});
```

⇒  `router.route` 로 묶음


<br/>  
<br/>  


# req, res 객체

<br/>  

## req

- `req.app`: req 객체를 통해 app 객체에 접근할 수 있습니다.
    - `req.app.get('port')`와 같은 식으로 사용할 수 있음
- `req.body`: body-parser 미들웨어가 만드는 요청의 본문을 해석한 객체
- `req.cookies`: cookie-parser 미들웨어가 만드는 요청의 쿠키를 해석한 객체
- `req.ip`: 요청의 ip 주소가 담겨 있음
- `req.params`: 라우트 매개변수에 대한 정보가 담긴 객체
- `req.query`: 쿼리스트링에 대한 정보가 담긴 객체
- `req.signedCookies`: 서명된 쿠키들은 `req.cookies` 대신 여기에 담겨 있음
- `req.get(헤더 이름)`: 헤더의 값을 가져오고 싶을 때 사용하는 메서드

<br/>  

## res

- `res.app`: req.app처럼 res 객체를 통해 app 객체에 접근할 수 있음
- `res.cookie(키, 값, 옵션)`: 쿠키를 설정하는 메서드
- `res.clearCookie(키, 값, 옵션)`: 쿠키를 제거하는 메서드
- `res.end()`: 데이터 없이 응답을 보냄
- `res.json(JSON)`: JSON 형식의 응답을 보냄
- `res.redirect(주소)`: 리다이렉트할 주소와 함께 응답을 보냄
    
    ```jsx
    res.writeHead(302, {
          Location: '/',
          'Set-Cookie': `session=${uniqueInt}; Expires=${expires.toGMTString()}; HttpOnly; Path=/`,
        });
    ```
    
    ```jsx
    res.status(302).redirect('/'); 
    ```
    
    - 위처럼 했던 것을 오른쪽 처럼 해주기만 하면 됨
- `res.render(뷰, 데이터)`:  템플릿 엔진을 렌더링해서 응답할 때 사용하는 메서드
- `res.send(데이터)`: 데이터와 함께 응답을 보냅니다.
    - 데이터는 문자열일 수도 있고~ HTML일 수도 있으며, 버퍼일 수도 있고 객체나 배열일 수도~
- `res.sendFile(경로)`: 경로에 위치한 파일을 응답
- `res.setHeader(헤더, 값)`: 응답의 헤더를 설정
- `res.status(코드)`: 응답 시의 HTTP 상태 코드를 지정

<br/>  

## 기타

- 메서드 체이닝을 지원함

![](/images/286dfd4e-7b76-47ea-a042-27a3d461abcd-image.png)


- 응답은 한 번만 보내야 함

![](/images/08bbbe56-2db3-456c-a44d-6e0ac742fc92-image.png)



<br/>  
<br/>  

# 템플릿 엔진

- HTML의 정적인 단점을 개선
    - 반복문, 조건문, 변수 등을 사용할 수 있음
    - 동적인 페이지 작성 가능
    - PHP, JSP와 유사

    
<br/>  

## Pug 템플릿 엔진 (구 Jade)

```jsx
app.set('views', path.join(__dirname, 'views'));
app.set('view engine', 'pug');
```
[Pug 문법 자세히 보기](https://www.notion.so/Section-5-8ba252ded4554e73a46001e222894f6d#59b278c03185423b9f324b253070b65a)

<br/>  

## 넌적스 템플릿 엔진

```jsx
const nunjucks = require('nunjucks');
```

[넌적스 문법 자세히 보기](https://www.notion.so/Section-5-8ba252ded4554e73a46001e222894f6d#64c2b0acb1ac4e54877f650dcd708d5d)    

<br/>  

## 에러 처리 미들웨어

- 에러 발생 시 템플릿 엔진과 상관없이 템플릿 엔진 변수를 설정하고 error 템플릿을 렌더링함
    - res.locals.변수명으로도 템플릿 엔진 변수 생성 가능
    - process.env.NODE_ENV는 개발환경인지 배포환경인지 구분해주는 속성
    
    ```jsx
    app.use((req, res, next) => {
      const error =  new Error(`${req.method} ${req.url} 라우터가 없습니다.`);
      error.status = 404;
      next(error);
    });
    
    //404에러도 여기서 같이 처리
    app.use((err, req, res, next) => {
      res.locals.message = err.message;
    
      res.locals.error = process.env.NODE_ENV !== 'production' ? err : {};
    	// 개발용이면 에러를 넣어줌, 배포시에는 넣지 X
    
      res.status(err.status || 500); //에러상태 구분
      res.render('error');
    });
    ```
    
    
    
<br/>  


[인프런 Node.js 강의](https://www.inflearn.com/course/%EB%85%B8%EB%93%9C-%EA%B5%90%EA%B3%BC%EC%84%9C/dashboard)
Zerocho 님의 "Node.js 교과서 - 기본부터 프로젝트 실습까지" 강의를 기반으로 작성한 문서입니다. 