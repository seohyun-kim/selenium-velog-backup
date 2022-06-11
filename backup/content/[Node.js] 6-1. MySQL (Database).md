---
title: "[Node.js] 6-1. MySQL (Database)"
description: "지금까지는 데이터를 서버 메모리에 저장했음서버를 재 시작하면 데이터도 사라져버림 ⇒  영구적으로 저장할 공간 필요MySQL 관계형 데이터베이스 사용데이터베이스: 관련성을 가지며 중복이 없는 데이터들의 집합DBMS: 데이터베이스를 관리하는 시스템RDBMS: 관계형 데이터"
date: 2022-04-29T14:29:54.407Z
tags: []
---
# Database

![](/images/ec6e8ad1-b0ad-4d58-9fb4-03224c1dbc39-image.png)


- 지금까지는 데이터를 서버 메모리에 저장했음
    - 서버를 재 시작하면 데이터도 사라져버림 ⇒  영구적으로 저장할 공간 필요
    
- MySQL 관계형 데이터베이스 사용
    - 데이터베이스: 관련성을 가지며 중복이 없는 데이터들의 집합
    - DBMS: 데이터베이스를 관리하는 시스템
    - RDBMS: 관계형 데이터베이스를 관리하는 시스템
    - 서버의 하드 디스크나 SSD 등의 저장 매체에 데이터를 저장
    - 서버 종료 여부와 상관 없이 데이터를 계속 사용할 수 있음
    - 여러 사람이 동시에 접근할 수 있고, 권한을 따로 줄 수 있음


<br/>  

<br/>  



# 데이터 베이스, 테이블 생성하기

<br/>  

## 데이터 베이스 생성

![](/images/6b229c49-0cee-41d7-993d-6f168ad663ad-image.png)


- `CREATE SCHEMA nodejs;`로 nodejs 데이터베이스 생성
- `use nodejs;`로 생성한 데이터베이스 선택

<br/>  

## 테이블 생성

![](/images/b0f8ce96-c0a6-47f9-a9af-20ffb9318f71-image.png)


- CREATE TABLE [데이터베이스명.테이블명]으로 테이블 생성
- 사용자 정보를 저장하는 테이블

<br/>  

## 컬럼과 로우

![](/images/75a9c975-dd17-4954-9faf-b67f0dd00258-image.png)

- 나이, 결혼 여부, 성별같은 정보가 컬럼
- 실제로 들어가는 데이터는 로우

<br/>  

## 컬럼 옵션들

```sql
id INT NOT NULL AUTO_INCREMENT
```

- `INT`: 정수 자료형(FLOAT, DOUBLE은 실수)
- `VARCHAR`: 문자열 자료형, 가변 길이(CHAR은 고정 길이)
- `TEXT`: 긴 문자열은 TEXT로 별도 저장
- `DATETIME`: 날짜 자료형 저장
- `TINYINT`: -128에서 127까지 저장하지만 여기서는 1 또는 0만 저장해 불 값 표현
- `NOT NULL`: 빈 값은 받지 않는다는 뜻(NULL은 빈 값 허용)
- `AUTO_INCREMENT`: 숫자 자료형인 경우 다음 로우가 저장될 때 자동으로 1 증가
- `UNSIGNED`: 0과 양수만 허용
- `ZEROFILL`: 숫자의 자리 수가 고정된 경우 빈 자리에 0을 넣음
- `DEFAULT now()`: 날짜 컬럼의 기본값을 현재 시간으로

<br/>  

## Primary Key, Unique Index

- `PRIMARY KEY(id)`
    - id가 테이블에서 로우를 특정할 수 있게 해주는 고유한 값임을 의미
    - 학번, 주민등록번호같은 개념
    
- `UNIQUE INDEX name_UNIQUE (name ASC)`
    - 해당 컬럼(name)이 고유해야 함을 나타내는 옵션
    - name_UNIQUE는 이 옵션의 이름(아무거나 다른 걸로 지어도 됨)
    - ASC는 인덱스를 오름차순으로 저장함의 의미(내림차순은 DESC)

<br/>  

## 테이블 옵션

- `COMMENT`: 테이블에 대한 보충 설명(필수 아님)
- `DEFAULT CHARSET`: utf8로 설정해야 한글이 입력됨(utf8mb4 하면 이모티콘 가능)
- `ENGINE`: InnoDB 사용(이외에 MyISAM이 있음, 엔진별로 기능 차이 존재)

<br/>  

## 테이블 생성 확인, 삭제, 목록

- 생성확인 : `DESC tablename;`
- 테이블 삭제 : `DROP TABLE tablename;`
- 테이블 목록 보기: `SHOW TABLES;`

<br/>  

## 외래키(foreign key)

- 댓글 테이블은 사용자 테이블과 관계가 있음(사용자가 댓글을 달기 때문)
    - 외래키를 두어 두 테이블이 관계가 있다는 것을 표시
    
    - `FOREIGN KEY (컬럼명) REFERENCES 데이터베이스.테이블명 (컬럼)`
    - `FOREIGN KEY (commenter) REFERENCES nodejs.users (id)`
    - 댓글 테이블에는 commenter 컬럼이 생기고 사용자 테이블의 id값이 저장됨
    
    - `ON DELETE CASCADE, ON UPDATE CASCADE`
    - 사용자 테이블의 로우가 지워지고 수정될 때 댓글 테이블의 연관된 로우들도 같이 지워지고 수정됨
    - 데이터를 일치 시키기 위해 사용하는 옵션(CASCADE 대신 SET NULL과 NO ACTION도 있음)

<br/>  
<br/>  


# CRUD

- Create, Read, Update, Delete의 두문자어
    
    ![](/images/8a093a8a-375b-467d-a297-121a9ee6886d-image.png)

    
<br/>  

## Create

```sql
INSERT INTO 테이블 (컬럼명들) VALUES (값들)
```

![](/images/2bd9b871-e013-4053-b7e7-c26feac48201-image.png)


<br/>  


## Read

```sql
SELECT 컬럼 FROM 테이블명
```

- SELECT * 은 모든 컬럼을 선택한다는 의미
    ![](/images/d938a9f7-7d1e-4624-ad91-ba5d7f20d279-image.png)



- 컬럼만 따로 추리는 것도 가능
  ![](/images/03bf8b4c-14f5-4ca4-bba2-19a8fe3978a2-image.png)



<br/>  

### `WHERE`로 조건을 주어 선택 가능

- `AND`로 여러가지 조건을 동시에 만족하는 것을 찾음
  ![](/images/8d4735e0-e431-4c95-9b21-c2c3583f390e-image.png)



- `OR`로 여러가지 조건 중 하나 이상을 만족하는 것을 찾음
  ![](/images/7d7da498-b3a6-4a43-a6db-75cdfeece6e9-image.png)



<br/>  


### 정렬해서 찾기

![](/images/7b132e31-2e1e-4717-8df1-14cacd768173-image.png)
- `ORDER BY`로 특정 컬럼 값 순서대로 정렬 가능
- `DESC`는 내림차순, `ASC` 오름차순

<br/>  

### LIMIT, OFFSET

- LIMIT으로 조회할 개수 제한
  ![](/images/e07dd925-d594-4b7f-8e30-8b621aadcf82-image.png)


- OFFSET으로 앞의 로우들 스킵 가능(OFFSET 2면 세 번째 것부터 찾음)
  ![](/images/418d0fef-7445-4340-bf0f-427f12f804f0-image.png)




<br/>  


## Update

데이터베이스에 있는 데이터를 수정하는 작업

```sql
UPDATE 테이블명 SET 컬럼=새값 WHERE 조건
```

- 예시 
    ![](/images/ac0f829f-88ad-4f1f-a915-6f2e9f24bbdf-image.png)

    

<br/>  


## Delete

데이터베이스에 있는 데이터를 삭제하는 작업

```sql
DELETE FROM 테이블명 WHERE 조건
```

- 예시
    ![](/images/3e33dddd-bdd4-4556-b2a2-e1fee5b5a3aa-image.png)

<br/>  


[인프런 Node.js 강의](https://www.inflearn.com/course/%EB%85%B8%EB%93%9C-%EA%B5%90%EA%B3%BC%EC%84%9C/dashboard)
Zerocho 님의 "Node.js 교과서 - 기본부터 프로젝트 실습까지" 강의를 기반으로 작성한 문서입니다. 