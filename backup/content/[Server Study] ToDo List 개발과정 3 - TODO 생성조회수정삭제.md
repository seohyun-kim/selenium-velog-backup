---
title: "[Server Study] ToDo List 개발과정 3 - TODO 생성/조회/수정/삭제"
description: "지난 포스트 : ToDo List 개발과정 1 - SetUp & Table 생성 및 연결지난 포스트 : ToDo List 개발과정 2 - 회원 생성/조회/수정https&#x3A;//github.com/seohyun-kim/TodoListNode.js + JavaScri"
date: 2022-05-29T15:37:26.552Z
tags: []
---
[지난 포스트 : ToDo List 개발과정 1 - SetUp & Table 생성 및 연결](https://velog.io/@selenium/Server-Study-ToDo-List-%EA%B0%9C%EB%B0%9C%EA%B3%BC%EC%A0%95-1-SetUp-Table-%EC%83%9D%EC%84%B1-%EB%B0%8F-%EC%97%B0%EA%B2%B0)
[지난 포스트 : ToDo List 개발과정 2 - 회원 생성/조회/수정](https://velog.io/@selenium/Server-Study-ToDo-List-%EA%B0%9C%EB%B0%9C%EA%B3%BC%EC%A0%95-2-%ED%9A%8C%EC%9B%90-%EC%83%9D%EC%84%B1%EC%A1%B0%ED%9A%8C%EC%88%98%EC%A0%95)



> ### 코드 링크 (Github) 
https://github.com/seohyun-kim/TodoList

> ### Tools
Node.js + JavaScript + MySql + Sequelize

<br/>  
<br/>  

## 9. TODO 생성 (Create)

![](/images/2ebd3af8-1f3a-489f-9196-5185b6f5d9b0-image.png)


- 미 입력 예외 처리
- 해당 id의 사용자가 없는 경우 상태 코드 리턴

![](/images/ca387199-c340-4c3e-9bba-47070cd25355-image.png)

![](/images/6963c976-190f-4431-b516-ce20fd1cab0a-image.png)


[/routes/todos.js](https://github.com/seohyun-kim/TodoList/blob/main/routes/todos.js)

```jsx
router.post('/', async (req, res, next) => {
    try {
        const todo = await Todo.create({ //INSERT
            member: req.body.id,
            content: req.body.content,
            isCompleted: req.body.isCompleted
        });
        console.log(todo);
        res.status(201).json(todo);
    } catch (err) {
        console.error(err);
        next(err);
    }
});
```
<br/>  


[public/sequelize.js](https://github.com/seohyun-kim/TodoList/blob/main/public/sequelize.js)    
```jsx
// todo 등록 시
document.getElementById('comment-form').addEventListener('submit', async (e) => {
    e.preventDefault();
    const id = e.target.userid.value;
    const content = e.target.content.value;
    const isCompleted = e.target.isCompleted.checked;
    if (!id) {
        return alert('아이디를 입력하세요');
    }
    if (!content) {
        return alert('할일을 입력하세요');
    }
    try {
        await axios.post('/todos', { id, content, isCompleted });
        getTodo();
    } catch (err) {
        console.error(err);
    }
    e.target.userid.value = '';
    e.target.comment.value = '';
    e.target.isCompleted.value = false;
});
```

<br/>  
<br/>  


## 10. 회원 단건 조회 (회원 + 회원의 TODO 리스트)

![](/images/181ec2be-87b6-4d1c-a431-4a8574600231-image.png)


### 회원N의 TODO 조회 버튼을 클릭하면 해당 회원의 TODO LIST 출력
![](/images/c5580742-d6d5-44c8-9661-c38ecd2643dc-image.png)

![](/images/e7f4cb4b-da42-4a95-8071-2bea4677cabf-image.png)

[/routes/members.js](https://github.com/seohyun-kim/TodoList/blob/main/routes/members.js)
```jsx
router.route('/:id')
    .get(async (req, res, next) => {
        try {
            const todos = await Todo.findAll({
                include: {
                    model: Member,
                    where: { id: req.params.id },
                },
            });
            console.log(todos);
            res.json(todos);
        } catch (err) {
            console.error(err);
            next(err);
        }
    })
```

<br/>  
<br/>  

## 11. Todo 수정 기능 (완료 여부 수정 :  patch-update)

![](/images/cab24632-d346-481c-9c43-b219ad16f3a9-image.png)
![](/images/bb1b8e87-ee29-4db1-b04e-d2ac89ce83df-image.png)
![](/images/ef1afaf5-0935-4844-b591-b87fd08de44b-image.png)


<br/>  

[/routes/todos.js](https://github.com/seohyun-kim/TodoList/blob/main/routes/todos.js)
```jsx
router.route('/:id')
    .patch(async (req, res, next) => {
        try {
            const result = await Todo.update({
                isCompleted: req.body.isCompleted,
            }, {
                where: { id: req.params.id },
            });
            res.json(result);
        } catch (err) {
            console.error(err);
            next(err);
        }
    })
```

<br/>  

[public/sequelize.js](https://github.com/seohyun-kim/TodoList/blob/main/public/sequelize.js)   
```jsx
const edit = document.createElement('button');
edit.textContent = '수정';
edit.addEventListener('click', async () => { // 수정 클릭 시
    const newCompleted = confirm('완료는 확인, 미완료는 취소를 클릭하세요.');
    try {
        await axios.patch(`/todos/${todo.id}`, { isCompleted: newCompleted });
        getTodo();
    } catch (err) {
        console.error(err);
    }
});
```


<br/>  
<br/>  

## 12. Todo 삭제 기능 (delete-destroy)
![](/images/cced5c96-5234-4f76-93b3-bb0a9dd5466b-image.png)


[/routes/todos.js](https://github.com/seohyun-kim/TodoList/blob/main/routes/todos.js)

```jsx
router.route('/:id')
    .delete(async (req, res, next) => {
        try {
            const result = await Todo.destroy({ where: { id: req.params.id } });
            res.json(result);
        } catch (err) {
            console.error(err);
            next(err);
        }
    });
```
<br/>  

[public/sequelize.js](https://github.com/seohyun-kim/TodoList/blob/main/public/sequelize.js)   
```jsx
const remove = document.createElement('button');
remove.textContent = '삭제';
remove.addEventListener('click', async () => { // 삭제 클릭 시
    try {
        await axios.delete(`/todos/${todo.id}`);
        getTodo();
    } catch (err) {
        console.error(err);
    }
});
```

<br/>  
<br/>  


## 13. TODO 식별자 조회 (TODO + 회원 정보)

![](/images/1b55ec34-470d-4ee6-b042-08206818e18f-image.png)

![](/images/fda3daeb-afe4-455c-a1f3-38443d0b0a62-image.png)


[/routes/todos.js](https://github.com/seohyun-kim/TodoList/blob/main/routes/todos.js)

```jsx
router.route('/:id')
    //할 일 단건 조회
    .get(async (req, res, next) => {
        try {
            const id = req.params.id;
            const todos = await Todo.findOne({
                include: {
                    model: Member,
                    where: { id: req.params.id },
                },
            });
            console.log(todos);
            res.json(todos);
        } catch (err) {
            console.error(err);
            next(err);
        }
    })
```


<br/>  
<br/>  

## 14. TODO 전체 리스트 조회
![](/images/ee117af8-ad88-4a78-a134-ecdb9545f37c-image.png)

![](/images/d4f20559-f8ea-40f4-9451-4ef229cb5335-image.png)


[/routes/todos.js](https://github.com/seohyun-kim/TodoList/blob/main/routes/todos.js)
```jsx
router.route('/')
    .get(async (req, res, next) => {
        try {
            const resTodos = await Todo.findAll({
                include: {
                    model: Member,
                    where: {  },
                },
            });
            res.json(resTodos);
        } catch (err) {
            console.error(err);
            next(err);
        }
    })
```


<br/>  

[public/sequelize.js](https://github.com/seohyun-kim/TodoList/blob/main/public/sequelize.js)   

```jsx
// 전체 TODO 로딩
async function getTodo() {
    try {
        const res = await axios.get(`/todos`);
        const todos = res.data;
        const tbody = document.querySelector('#comment-list tbody');
        tbody.innerHTML = '';
        todos.map(function (todo) {
            console.log("메롱=>"+JSON.stringify(todo));
            // 로우 셀 추가
            const row = document.createElement('tr');
            let td = document.createElement('td');
            td.textContent = todo.id;
            row.appendChild(td);
            td = document.createElement('td');
            td.textContent = todo.Member.email;
            row.appendChild(td);
            td = document.createElement('td');
            td.textContent = todo.Member.name;
            row.appendChild(td);
            td = document.createElement('td');
            td.textContent = todo.content;
            row.appendChild(td);
            td = document.createElement('td');
            td.textContent = todo.isCompleted ? '⭕' : '❌';
            row.appendChild(td);

            const edit = document.createElement('button');
            edit.textContent = '수정';
            edit.addEventListener('click', async () => { // 수정 클릭 시
                const newCompleted = confirm('완료는 확인, 미완료는 취소를 클릭하세요.');
                try {
                    await axios.patch(`/todos/${todo.id}`, { isCompleted: newCompleted });
                    getTodo();
                } catch (err) {
                    console.error(err);
                }
            });
            const remove = document.createElement('button');
            remove.textContent = '삭제';
            remove.addEventListener('click', async () => { // 삭제 클릭 시
                try {
                    await axios.delete(`/todos/${todo.id}`);
                    getTodo();
                } catch (err) {
                    console.error(err);
                }
            });
            // 버튼 추가
            td = document.createElement('td');
            td.appendChild(edit);
            row.appendChild(td);
            td = document.createElement('td');
            td.appendChild(remove);
            row.appendChild(td);
            tbody.appendChild(row);
        });
    } catch (err) {
        console.error(err);
    }
}
```



