과제 모음
=============

> 1번 : 객체를 JSON으로 바꾼 후 다시 객체로 바꾸기

user를 JSON 형태의 문자열로 바꾼 다음 이를 다시 객체로 바꾼 후 제2의 변수에 저장해보세요.

```javascript
let user = {
  name: "John Smith",
  age: 35
};
```

<details>
<summary>답변</summary>
<div markdown="1">

```javascript
let user = {
    name: "John Smith",
    age: 35
};

let newUser = JSON.parse(JSON.stringify(user));

console.log(newUser);
```

</div>
</details>

<details>
<summary>정답</summary>
<div markdown="1">

```javascript
let user = {
  name: "John Smith",
  age: 35
};

let user2 = JSON.parse(JSON.stringify(user));
```

</div>
</details>