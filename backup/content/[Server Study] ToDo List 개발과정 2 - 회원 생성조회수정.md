---
title: "[Server Study] ToDo List 개발과정 2 - 회원 생성/조회/수정"
description: "지난 포스트 : ToDo List 개발과정 1 - SetUp & Table 생성 및 연결https&#x3A;//github.com/seohyun-kim/TodoListNode.js + JavaScript + MySql + Sequelizepublic/sequelize."
date: 2022-05-29T15:27:08.710Z
tags: []
---


[지난 포스트 : ToDo List 개발과정 1 - SetUp & Table 생성 및 연결](https://velog.io/@selenium/Server-Study-ToDo-List-%EA%B0%9C%EB%B0%9C%EA%B3%BC%EC%A0%95-1-SetUp-Table-%EC%83%9D%EC%84%B1-%EB%B0%8F-%EC%97%B0%EA%B2%B0)


> ### 코드 링크 (Github) 
https://github.com/seohyun-kim/TodoList

> ### Tools
Node.js + JavaScript + MySql + Sequelize

<br/>  
<br/>  


## 6. 회원 생성 기능
![](/images/58a78f68-9774-4f67-aed1-1978d66f9b0a-image.png)


<br/>  

### 6-1. 이메일 형식 예외 처리, 미 입력 예외 처리
    
![](/images/4e594b2a-e9d3-46db-a19e-d03a07a0a2f4-image.png)
    
[public/sequelize.js](https://github.com/seohyun-kim/TodoList/blob/main/public/sequelize.js)    
    
```jsx
// 사용자 등록 시
    
if (!email) {
  return alert('이메일을 입력하세요');
}
const reg_email = /^([0-9a-zA-Z_\.-]+)@([0-9a-zA-Z_-]+)(\.[0-9a-zA-Z_-]+){1,2}$/;
if(!reg_email.test(email)) {
  return alert('이메일 형식이 아닙니다.');
}
if (!name) {
  return alert('이름을 입력하세요');
}
if (!age) {
  return alert('나이를 입력하세요');
}
```
    
<br/>  

### 6-2.이메일 중복 처리 및 DB에 저장

![](/images/ce9789c6-9d52-42c4-a288-b9cf8d3f7a07-image.png)

![](/images/cd164b13-f8c4-459a-a1a7-503f4d3756a2-image.png)


[/routes/members.js](https://github.com/seohyun-kim/TodoList/blob/main/routes/members.js)
```jsx
// localhost:8080/members/

router.route('/')

  .post(async (req, res, next) => {
  try {
    //이메일 중복 처리
    const email = req.body.email;
    const exUser = await Member.findAll({  where: { email : email } });
    if (exUser.length >0) {
      console.log("이메일이 중복됩니다.");
      res.status(201);
    }else{
      console.log("이메일 사용 가능합니다.");
      //DB에 저장
      const members = await Member.create({
        email: req.body.email,
        age: req.body.age,
        name: req.body.name,
      }); // INSERT
      console.log(members);
      res.status(201).json(members);
    }
  } catch (err) {
    console.error(err);
    next(err);
  }
});
```

<br/>  
<br/>  

## 7. 회원 목록 조회 기능

![](/images/b033764d-0447-4b33-b9f1-7029833a4502-image.png)


### DB에서 전체 회원 정보를 가져와 동적 table 형태로 출력

![](/images/ab8920d1-b0a2-4c70-a33b-f6a76c476c66-image.png)


![](/images/d3635e58-5d00-418b-b480-15dd895abd1c-image.png)


[/routes/members.js](https://github.com/seohyun-kim/TodoList/blob/main/routes/members.js)
```jsx
// localhost:8080/members/
router.route('/')
    .get(async (req, res, next) => {
        try {
            const members = await Member.findAll(); //회원 전체 조회
            res.json(members);
            console.log("200 : 회원 리스트가 정상적으로 조회되었습니다.")
        } catch (err) {
            console.error(err);
            next(err);
        }
    })
```

<br/>  


[/public/sequelize.js](https://github.com/seohyun-kim/TodoList/blob/main/public/sequelize.js)
```jsx
// 사용자 로딩
async function getUser() {
    try {
        const res = await axios.get('/members');
        const members = res.data;
        console.log(members);
        const tbody = document.querySelector('#user-list tbody');
        tbody.innerHTML = '';
        members.map(function (member) {
            // 로우 셀 추가
            const row = document.createElement('tr');
            row.addEventListener('click', () => {
                console.log("row clicked!");
                getTodo();
            });
            let td = document.createElement('td');
            td.textContent = member.id;
            row.appendChild(td);

            td = document.createElement('td');
            td.textContent = member.email;
            row.appendChild(td);

            td = document.createElement('td');
            td.textContent = member.age;
            row.appendChild(td);

            td = document.createElement('td');
            td.textContent = member.name;
            row.appendChild(td);
					 
						...
```

<br/>  
<br/>  

## 8. 회원 정보 수정 기능

![](/images/afd815ef-3f12-435e-95d4-d0a842cb1fd2-image.png)

- 미 입력 에러 처리
- 나이 숫자 입력 제한
- 초기 값으로 기존의 나이와 이름 값을 가져와 미 입력을 사전에 방지
- 회원 정보 수정 후 리스트 출력 함수 호출하여 업데이트

![](/images/2e0b07fc-d201-4de3-b739-677b784cf158-image.png)


[/public/sequelize.js](https://github.com/seohyun-kim/TodoList/blob/main/public/sequelize.js)

```jsx
const edit = document.createElement('button');
edit.textContent = `회원${member.id} 수정`;
edit.addEventListener('click', async () => { // 수정 클릭 시

    // 초기 값을 기존 값으로 넣어 미입력을 방지, 나이는 숫자만 입력 받도록 처리
    const newAge =  parseInt(prompt('변경할 나이을 입력하세요', `${member.age}`));
    if (!newAge) { // 내용 없을 경우, 숫자 아닌 경우
        return alert('변경을 취소합니다.');
    }
    const newName = prompt('변경할 이름을 입력하세요', `${member.name}`);
    if (!newName) {
        return alert('변경을 취소합니다.');
    }
    try {
        await axios.patch(`/members/${member.id}`, { age: newAge ,  name: newName});
        getUser(); //함수 재 호출 (새로고침)
    } catch (err) {
        console.error(err);
    }
});

const retrieve = document.createElement('button');
retrieve.textContent = `회원${member.id} 의 TODO 조회`;
retrieve.addEventListener('click', async () => { // 수정 클릭 시
    getTodo();
});
```




