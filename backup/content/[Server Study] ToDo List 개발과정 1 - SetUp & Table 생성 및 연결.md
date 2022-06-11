---
title: "[Server Study] ToDo List 개발과정 1 - SetUp & Table 생성 및 연결"
description: "작년에 구현했던... 작지만 소듕한 TodoList..입니다 ㅎㅎ'Node.js 교과서-조현영'님의 강의를 참고했습니다.https&#x3A;//github.com/seohyun-kim/TodoListNode.js + JavaScript + MySql + Sequeliz"
date: 2022-05-29T15:17:04.272Z
tags: []
---



작년에 구현했던... 작지만 소듕한 TodoList..입니다 ㅎㅎ

'Node.js 교과서-조현영'님의 강의를 참고했습니다.

<br/>  

> ### 코드 링크 (Github) 
https://github.com/seohyun-kim/TodoList

> ### Tools
Node.js + JavaScript + MySql + Sequelize

<br/>  
<br/>  

## 1. 필요 모듈 다운로드

```
npm install express
npm i express morgan nunjucks sequelize sequelize-cli mysql2
npm i -D nodemon
npx sequelize init
```

<br/>  
<br/>  

## 2. Database 생성 및 Sequelize 연결

```powershell
MySQL [(none)]> show databases;
+--------------------+
| Database           |
+--------------------+

| todolist           |
+--------------------+
12 rows in set (0.017 sec)
```

<br/>  

/config/config.json
```json
{
  "development": {
    "username": "root",
    "password": "[YOURPASSWORD]",
    "database": "todolist",
    "host": "127.0.0.1",
    "dialect": "mysql"
  }
```

<br/>  
app.js

```jsx
sequelize.sync({ force: false })
    .then(() => {
        console.log('데이터베이스 연결 성공');
    })
    .catch((err) => {
        console.error(err);
    });
```



<br/>  
<br/>  

## 3. Sequelize model 작성 (1:N 관계)

- **Members**
[models/Member.js](https://github.com/seohyun-kim/TodoList/blob/main/models/Member.js)

  ![](/images/5fa1bc19-4431-4b31-93c9-c9001822e13f-image.png)

        
  ```jsx
  const Sequelize = require('sequelize');

  module.exports = class Member extends Sequelize.Model {
    static init(sequelize) {
      return super.init({
        // id 자동이므로 생략
        email: {
          type: Sequelize.STRING(20),
          allowNull: false,
          unique: true,
        },
        age: {
          type: Sequelize.TINYINT.UNSIGNED,
          allowNull: false,
        },
        name: {
          type: Sequelize.STRING(20),
          allowNull: false,
        },
        content: {
          type: Sequelize.STRING(45),
          allowNull: true
        },

      }, {
        sequelize,
        timestamps: true, //createdAt, updatedAt 자동 생성
        underscored: false,
        modelName: 'Member',
        tableName: 'Members',
        paranoid: false,
        charset: 'utf8',
        collate: 'utf8_general_ci',
      });
    }

    static associate(db) {
      // 1: N 관계 ( Member의 id를 참조)
      db.Member.hasMany(db.Todo, { foreignKey: 'member', sourceKey: 'id' });
    }
  };
  ```
<br/>  

- **Todos**
  [models/Todo.js](https://github.com/seohyun-kim/TodoList/blob/main/models/Todo.js)  
  
  ![](/images/85c224a3-a205-4ca1-a867-5af9cb8bed0d-image.png)


  ```jsx
  const Sequelize = require('sequelize');

  module.exports = class Todo extends Sequelize.Model {
    static init(sequelize) {
      return super.init({
        // id 자동이므로 생략
        content: {
          type: Sequelize.STRING(45),
          allowNull: true
        },
        isCompleted: {
          type: Sequelize.BOOLEAN,
          allowNull: false,
        }
      }, {
        sequelize,
        timestamps: true, //createdAt, updatedAt 자동 생성
        underscored: false,
        modelName: 'Todo', // js에서 쓰는 모델 명
        tableName: 'Todos', // sql에서 쓰는 table명
        paranoid: false, // deletedAt은 X (hard delete 할거임)
        charset: 'utf8',
        collate: 'utf8_general_ci',
      });
    }

    static associate(db) {
      // Todo는 Member에 속해있음 (Member의 target key는 id)
      db.Todo.belongsTo(db.Member, { foreignKey: 'id', targetKey: 'id' });
    }
  };
```
    
<br/>  
<br/>  


## 4. model 활성화

[/models/index.js](https://github.com/seohyun-kim/TodoList/blob/main/models/index.js)

```jsx
const Sequelize = require('sequelize');
const Member = require('./Member');
const Todo = require('./Todo');

const env = process.env.NODE_ENV || 'development';
const config = require('../config/config')[env];
const db = {};

const sequelize = new Sequelize(config.database, config.username, config.password, config);

db.sequelize = sequelize; // 연결객체 연결
db.Sequelize = Sequelize;

db.Member = Member;
db.Todo = Todo;

Member.init(sequelize);
Todo.init(sequelize);

Member.associate(db);
Todo.associate(db);

module.exports = db;
```


<br/>  
<br/>  


## 5. Sequelize 모델 연결을 통해 table 생성

```powershell
MySQL [todolist]> show tables;
+--------------------+
| Tables_in_todolist |
+--------------------+
| members            |
| todos              |
+--------------------+
2 rows in set (0.002 sec)


MySQL [todolist]> DESC members;
+-----------+------------------+------+-----+---------+----------------+
| Field     | Type             | Null | Key | Default | Extra          |
+-----------+------------------+------+-----+---------+----------------+
| id        | int              | NO   | PRI | NULL    | auto_increment |
| email     | varchar(20)      | NO   | UNI | NULL    |                |
| age       | tinyint unsigned | NO   |     | NULL    |                |
| name      | varchar(20)      | NO   |     | NULL    |                |
| content   | varchar(45)      | YES  |     | NULL    |                |
| createdAt | datetime         | NO   |     | NULL    |                |
| updatedAt | datetime         | NO   |     | NULL    |                |
+-----------+------------------+------+-----+---------+----------------+
7 rows in set (0.002 sec)


MySQL [todolist]> DESC todos;
+-------------+-------------+------+-----+---------+----------------+
| Field       | Type        | Null | Key | Default | Extra          |
+-------------+-------------+------+-----+---------+----------------+
| id          | int         | NO   | PRI | NULL    | auto_increment |
| content     | varchar(45) | YES  |     | NULL    |                |
| isCompleted | tinyint(1)  | NO   |     | NULL    |                |
| createdAt   | datetime    | NO   |     | NULL    |                |
| updatedAt   | datetime    | NO   |     | NULL    |                |
| member      | int         | YES  | MUL | NULL    |                |
+-------------+-------------+------+-----+---------+----------------+
6 rows in set (0.029 sec)
```
