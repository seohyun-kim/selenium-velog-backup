---
title: "[Node.js] 6-2. MySQL (Sequelize ORM)"
description: "SQL 작업을 쉽게 할 수 있도록 도와주는 라이브러리ORM: Object Relational Mapping: 객체와 데이터를 매핑(1대1 짝지음)MySQL 외에도 다른 RDB(Maria, Postgre, SQLite, MSSQL)와도 호환됨자바스크립트 문법으로 데이터베"
date: 2022-04-29T14:36:28.600Z
tags: []
---
# 시퀄라이즈

<br/>  

## 1. 시퀄라이즈 ORM

- SQL 작업을 쉽게 할 수 있도록 도와주는 라이브러리
    - ORM: Object Relational Mapping: 객체와 데이터를 매핑(1대1 짝지음)
    - MySQL 외에도 다른 RDB(Maria, Postgre, SQLite, MSSQL)와도 호환됨
    - 자바스크립트 문법으로 데이터베이스 조작 가능
-  [예제](https://github.com/seohyun-kim/nodejs-book/tree/master/ch7/7.6/learn-sequelize)
    

<br/>  

## 2. 시퀄라이즈 CLI 사용하기

- 시퀄라이즈 명령어 사용하기 위해 sequelize-cli 설치
    - mysql2는 MySQL DB가 아닌 드라이버(Node.js와 MySQL을 이어주는 역할)
    
   ![](/images/6e53eef0-1be2-4aa7-911f-396bb962dcf9-image.png)


- npx sequelize init으로 시퀄라이즈 구조 생성
    ![](/images/27ff5870-fe59-4f15-aa3d-60db372a6764-image.png)

    

<br/>  

## 3. models/index.js

```jsx
const Sequelize = require('sequelize');

const env = process.env.NODE_ENV || 'development';
const config = require('../config/config')[env];
const db = {};

const sequelize = new Sequelize(config.database, config.username, config.password, config);

db.sequelize = sequelize;

module.exports = db;
```

<br/>  


## 4. MySQL 연결하기 (app.js)

```jsx
const express = require('express');
const path = require('path');
const morgan = require('morgan');
const nunjucks = require('nunjucks');

const { sequelize } = require('./models');

const app = express();
app.set('port', process.env.PORT || 3001);
app.set('view engine', 'html');
nunjucks.configure('views', {
  express: app,
  watch: true,
});
sequelize.sync({ force: false }) //⭐
  .then(() => {
    console.log('데이터베이스 연결 성공');
  })
  .catch((err) => {
    console.error(err);
  });

app.use(morgan('dev'));
app.use(express.static(path.join(__dirname, 'public')));
app.use(express.json());
app.use(express.urlencoded({ extended: false }));

app.use((req, res, next) => {
  const error =  new Error(`${req.method} ${req.url} 라우터가 없습니다.`);
  error.status = 404;
  next(error);
});

app.use((err, req, res, next) => {
  res.locals.message = err.message;
  res.locals.error = process.env.NODE_ENV !== 'production' ? err : {};
  res.status(err.status || 500);
  res.render('error');
});

app.listen(app.get('port'), () => {
  console.log(app.get('port'), '번 포트에서 대기 중');
});
```

<br/>  


## 5. DB 연결 정보 설정 (config.json)

```jsx
{
  "development": {
    "username": "root",
    "password": "nodejsbook",
    "database": "nodejs",
    "host": "127.0.0.1",
    "dialect": "mysql"
  },
...
}
```

<br/>  

## 6. 연결 테스트

- npm start로 실행해서 SELECT 1+1 AS RESULT가 나오면 연결 성공
    
    ![](/images/029f56f4-70de-4f2b-b47d-0e5c30456f55-image.png)


<br/>  

## 7. 모델 생성

- 테이블에 대응되는 시퀄라이즈 모델 생성
    
    ```jsx
    const Sequelize = require('sequelize');
    
    module.exports = class User extends Sequelize.Model {
      static init(sequelize) {
        return super.init({
    
    			// 시퀄라이즈는 id를 자동으로 넣어주기 때문에 생략 가능!!!
    
          name: {
            type: Sequelize.STRING(20),
            allowNull: false,
            unique: true,
          },
          age: {
            type: Sequelize.INTEGER.UNSIGNED,
            allowNull: false,
          },
          married: {
            type: Sequelize.BOOLEAN, //true, false
            allowNull: false,
          },
          comment: {
            type: Sequelize.TEXT,
            allowNull: true,
          },
          created_at: {
            type: Sequelize.DATE, 
    				// 시퀄라이즈에서 DATA는 MySQL에서 DATETIME
    				//               DateOnly          DATE
    
            allowNull: false,
            defaultValue: Sequelize.NOW,
          },
        }, {
          sequelize,
          timestamps: false,
          underscored: false,
          modelName: 'User',
          tableName: 'users',
          paranoid: false,
          charset: 'utf8',
          collate: 'utf8_general_ci',
        });
      }
    
      static associate(db) {
        db.User.hasMany(db.Comment, { foreignKey: 'commenter', sourceKey: 'id' });
      }
    };
    ```
    
<br/>  
    

## 8. 모델 옵션들

- 시퀄라이즈 모델의 자료형은 MySQL의 자료형과 조금 다름
    
    ![](/images/cd8ff8b9-87e7-44db-b447-2a1e7efc39cc-image.png)

    

- define 메서드의 세 번째 인자는 테이블 옵션
    - `timestamps`: true면 createdAt(생성 시간), updatedAt(수정 시간) 컬럼을 자동으로 만듦
        - 예제에서는 직접 created_at 컬럼을 만들었으므로 false로 함
    - `paranoid` 옵션은 true면 deletedAt(삭제 시간) 컬럼을 만듦, 로우 복구를 위해 * 완전히 삭제하지 않고 deletedAt에 표시해둠
    - `underscored` 옵션은 캐멀케이스로 생성되는 컬럼을 스네이크케이스로 생성
    - `modelName`은 모델 이름, tableName 옵션은 테이블 이름을 설정
    - `charset`과 collate는 한글 설정을 위해 필요(이모티콘 넣으려면 utf8mb4로)


<br/>  


## 9. 댓글 모델 생성

```jsx
const Sequelize = require('sequelize');

module.exports = class Comment extends Sequelize.Model {
  static init(sequelize) {
    return super.init({
      comment: {
        type: Sequelize.STRING(100),
        allowNull: false,
      },
      created_at: {
        type: Sequelize.DATE,
        allowNull: true,
        defaultValue: Sequelize.NOW,
      },
    }, {
      sequelize,
      timestamps: false,
      modelName: 'Comment',
      tableName: 'comments',
      paranoid: false,
      charset: 'utf8mb4',
      collate: 'utf8mb4_general_ci',
    });
  }

  static associate(db) {
    db.Comment.belongsTo(db.User, { foreignKey: 'commenter', targetKey: 'id' });
  }
};
```

<br/>  

## 10. 댓글 모델 활성화

- index.js에 모델 연결
    - init으로 sequelize와 연결
    - associate로 관계 설정
    
    ```jsx
    const Sequelize = require('sequelize');
    const User = require('./user');
    const Comment = require('./comment');
    
    ...
    const sequelize = new Sequelize(config.database, config.username, config.password, config);
    
    db.sequelize = sequelize;
    
    db.User = User;
    db.Comment = Comment;
    
    User.init(sequelize);
    Comment.init(sequelize);
    
    User.associate(db);
    Comment.associate(db);
    
    module.exports = db;
    ```
    
<br/>  


## 11. 관계 정의하기

- users 모델과 comments 모델 간의 관계를 정의
    - 1:N 관계 (사용자 한 명이 댓글 여러 개 작성)
    - 시퀄라이즈에서는 1:N 관계를 hasMany로 표현(사용자.hasMany(댓글))
    - 반대의 입장에서는 belongsTo(댓글.belongsTo(사용자))
    - belongsTo가 있는 테이블에 컬럼이 생김(댓글 테이블에 commenter 컬럼)

![](/images/b000325a-5ac0-4983-918e-fef640c89e92-image.png)


![](/images/2d2bed79-77f4-4842-890d-24f9181efd3b-image.png)


<br/>  

### 11-1. 1대 1 관계

![](/images/4808a311-05e5-4e4e-8cf0-c8d82b6b4c24-image.png)


- 예) 사용자 테이블과 사용자 정보 테이블
    
    ![](/images/de423e9f-81ba-4e07-b266-e6fcbc4cafa7-image.png)

    
<br/>  

### 11-2. N대  M 관계

![](/images/be42f05e-ac25-4f9a-91f3-e7685f283773-image.png)


- 예) 게시글과 해시태그 테이블

하나의 게시글이 여러 개의 해시태그를 가질 수 있고 하나의 해시태그가 여러 개의 게시글을 가질 수 있음

- DB 특성상 다대다 관계는 중간 테이블이 생김
    ![](/images/976f838f-ecd5-4e10-a281-622cb79a04eb-image.png)

   
    ![](/images/b841820f-dc11-41c3-b961-a8d4c972ab40-image.png)

    

<br/>  


## 12. 시퀄라이즈 쿼리

윗 줄이 SQL문, 아랫 줄은 시퀄라이즈 쿼리(자바스크립트)

[시퀄라이즈 쿼리 예시](https://www.notion.so/Section-6-MySQL-ce3eaa8edef24e079846b89aac959f51#49f830346a964507b54cb0ad54ae6f15)
<br/>  


## 13. 관계 쿼리

[관계 쿼리 예시](https://www.notion.so/Section-6-MySQL-ce3eaa8edef24e079846b89aac959f51#486a0b98dc8c4a4690723486310ff977)
    
<br/>  


## 14. raw 쿼리

- 직접 SQL을 쓸 수 있음    
    ![](/images/e689c7fa-17e0-48ef-841f-2abfbd29313a-image.png)


<br/>  
<br/>  


[인프런 Node.js 강의](https://www.inflearn.com/course/%EB%85%B8%EB%93%9C-%EA%B5%90%EA%B3%BC%EC%84%9C/dashboard)
Zerocho 님의 "Node.js 교과서 - 기본부터 프로젝트 실습까지" 강의를 기반으로 작성한 문서입니다. 

