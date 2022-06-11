---
title: "[Server Study] REST 와 HTTP, RESTful API"
description: "네트워크 아키텍처 원리(자원을 정의하고 자원에 대한 주소를 지정하는 방법)의 모음임인터페이스 일관성 : 일관적인 인터페이스로 분리되어야 함무상태(Stateless) : 각 요청 간 클라이언트의 콘텍스트가 서버에 저장되어서는 안됨캐시 처리가능(Cacheable) : ww"
date: 2022-05-08T16:44:25.775Z
tags: []
---


## REST(Representational State Transfer)란?
> 네트워크 아키텍처 원리(자원을 정의하고 자원에 대한 주소를 지정하는 방법)의 모음임

<br/>  

### REST 아키텍처에 적용되는 6가지 제한 조건

1. **인터페이스 일관성** : 일관적인 인터페이스로 분리되어야 함

2. **무상태(Stateless)** : 각 요청 간 클라이언트의 콘텍스트가 서버에 저장되어서는 안됨

3. **캐시 처리가능(Cacheable)** : www에서와 같이 클라이언트는 응답을 캐싱할 수 있어야 함

4. **계층화(Layered System)** : 클라이언트는 보통 대상 서버에 직접 연결되었는지 또는 중간 서버를 통해 연결 되었는지를 알 수 없음, 중간 서버는 로드 밸런싱 기능이나 공유캐시 기능을 제공함으로써 시스템 규모 확장성을 향상시키는 데 유용함

5. **Code on demand (optional)** : 자바 애플릿이나 자바스크립트의 제공을 통해 서버가 클라이언트가 실행시킬 수 있는 로직을 전송하여 기능을 확장시킬 수 있음

6. **클라이언트/서버 구조** : 아키텍처를 단순화시키고 작은 단위로 분리(decouple)함으로써 클라이언트-서버의 각 파트가 독립적으로 개선될 수 있도록 해줌

<br/>  

### REST 의 구성 요소
1. 자원(Resource) : `HTTP URI`

2. 자원에 대한 행위 : `HTTP Method`

3. 자원에 대한 행위 내용 : `HTTP Message Pay load`


<br/>  
<br/> 


## 그럼 REST API는 뭔데?
> REST 원리를 따르는 API

- 서버에 요청을 보낼 때는 주소를 통해 요청의 내용을 표현

    - /index.html 이면 index.html을 보내 달라는 뜻
    - 항상 html을 요구할 필요는 없음
    - 서버가 이해하기 쉬운 주소가 좋음
    - /user이면 사용자 정보에 관한 정보를 요청하는 것
    - /post면 게시글에 관련된 자원을 요청하는 것

    
- REST API( Representational State Transfer)
    - 서버의 자원을 정의하고 자원에 대한 주소를 지정하는 방법
    - 구조적으로 지정해 놓으면 클라이언트가 알아보기 쉽다.
    - 하지만 예측 가능한 만큼 해커의 공격 위협이 존재한다.


<br/> 

## 서버와 REST API - HTTP Method

REST 원리를 따라 서버에 요청을 보낼 때는 [HTTP 규약](https://velog.io/@selenium/Server-Study-Web%EA%B3%BC-HTTP-%ED%94%84%EB%A1%9C%ED%86%A0%EC%BD%9C)에 따른다.

### HTTP 요청을 보낼 때 사용하는 Method(메서드)

> - `GET` : 서버 자원을 가져오라고 할 때 사용
> 
> - `POST` : 서버에 자원을 새로 등록하고자 할 때 사용(또는 뭘 써야할 지 애매할 때)
> 
> - `PUT` : 서버의 자원을 요청에 들어있는 자원으로 치환하고자할 때 사용 (보통 전체 수정)
> 
> - `PATCH` : 서버 자원의 일부만 수정하고자 할 때 사용
> 
> - `DELETE` : 서버의 자원을 삭제하고자 할 때 사용



<br/> 


## URI 랑 URL이랑 다른거야?

 URI는 URL의 의미를 내포하고 있음

### URI(Uniform Resource Indentifier)
- 자원의 위치를 포함하는 자원에 대한 고유 식별자
- scheme, host, url path, query, bookmark를 포함함

![](/images/492e3913-2cff-40b0-aafd-29e59437fc9f-image.png)

<br/> 

### URL(Uniform Resource Locators)
- 자원이 실제로 존재하는 위치



<br/>  
<br/>  


## HTTP 상태코드

- writeHead 메서드에 첫 번째 인수로 넣은 값

    - 요청이 성공했는지 실패 했는지를 알려줌
    
    - `2XX`: **성공**을 알리는 상태 코드입니다. 대표적으로 200(성공), 201(작성됨)이 많이 사용됩니다.
    
    - `3XX`: **리다이렉션(다른 페이지로 이동)**을 알리는 상태 코드입니다. 어떤 주소를 입력했는데 다른 주소의 페이지로 넘어갈 때 이 코드가 사용됩니다. 대표적으로 301(영구 이동), 302(임시 이동)가 있습니다.
    
    - `4XX`: **요청 오류**를 나타냅니다. 요청 자체에 오류가 있을 때 표시됩니다. 대표적으로 401(권한 없음), 403(금지됨), 404(찾을 수 없음)가 있습니다.
    
    - `5XX`: **서버 오류**를 나타냅니다. 요청은 제대로 왔지만 서버에 오류가 생겼을 때 발생합니다. 이 오류가 뜨지 않게 주의해서 프로그래밍해야 합니다. 이 오류를 클라이언트로 res.writeHead로 직접 보내는 경우는 없고, 예기치 못한 에러 발생 시 서버가 알아서 5XX대 코드를 보냅니다. 500(내부 서버 오류), 502(불량 게이트웨이), 503(서비스를 사용할 수 없음)이 자주 사용됩니다.

- 참고 :  [HTTP 상태 코드 - HTTP | MDN](https://developer.mozilla.org/ko/docs/Web/HTTP/Status)
    
<br/>  



### [> Node.js 와 HTTP 바로가기](https://velog.io/@selenium/Node.js-3-1.-http-%EB%AA%A8%EB%93%88%EB%A1%9C-%EC%84%9C%EB%B2%84-%EB%A7%8C%EB%93%A4%EA%B8%B0)


<br/>  
<br/> 
<br/>  

References
https://ko.wikipedia.org/wiki/REST
https://developer.mozilla.org/ko/docs/Web/HTTP/Methods
https://khj93.tistory.com/entry/%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC-REST-API%EB%9E%80-REST-RESTful%EC%9D%B4%EB%9E%80
https://joshua1988.github.io/web-development/http-part1/
[Node.js 교과서 - 조현영]