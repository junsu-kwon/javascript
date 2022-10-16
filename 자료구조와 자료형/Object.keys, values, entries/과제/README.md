과제 모음
=============

> 1번 : 프로퍼티 값 더하기

급여 정보가 저장되어있는 객체 salaries가 있습니다.

Object.values 와 for..of 반복문을 사용해 모든 급여의 합을 반환하는 함수 sumSalaries(salaries)를 만들어보세요.

salaries가 빈 객체라면, 0이 반환되어야 합니다.

예시:
```javascript
let salaries = {
  "John": 100,
  "Pete": 300,
  "Mary": 250
};

alert( sumSalaries(salaries) ); // 650
```

<details>
<summary>답변</summary>
<div markdown="1">

for of를 쓰라고 했지만 괜히 하기 싫어지는 기분에 내 맘대로 해봤다.

```javascript
let salaries = {
  John: 100,
  Pete: 300,
  Mary: 250
};

function sumSalaries(salaries) {
  return Object.values(salaries).reduce((acc, val) => acc + val, 0);
}

console.log(sumSalaries(salaries));
```
</div>
</details>

<details>
<summary>정답</summary>
<div markdown="1">

```javascript
function sumSalaries(salaries) {

  let sum = 0;
  for (let salary of Object.values(salaries)) {
    sum += salary;
  }

  return sum; // 650
}

let salaries = {
  "John": 100,
  "Pete": 300,
  "Mary": 250
};

alert( sumSalaries(salaries) ); // 650
```

Object.values 와 reduce를 이용해 합을 구할 수도 있습니다.

```javascript
// reduce는 급여 정보가 저장되어있는 배열을 순회해
// 급여의 총합을 만들고
// 그 결과를 반환합니다.
function sumSalaries(salaries) {
  return Object.values(salaries).reduce((a, b) => a + b, 0) // 650
}
```
</div>
</details>

<details>
<summary>반성</summary>
<div markdown="1">
할게 없넹
</div>
</details>

<br>


> 2번 : 프로퍼티 개수 세기

객체 프로퍼티 개수를 반환하는 함수 count(obj)를 만들어보세요.

```javascript
let user = {
  name: 'John',
  age: 30
};

alert( count(user) ); // 2
```

가능한 짧게 코드를 작성해 보세요.

주의: 심볼형 프로퍼티는 무시하고 ‘일반’ 프로퍼티 개수만 세주세요.


<details>
<summary>답변</summary>
<div markdown="1">

```javascript
let user = {
  name: "John",
  age: 30
};

function count(user) {
  return Object.keys(user).length;
}

console.log(count(user)); // 2
```

</div>
</details>

<details>
<summary>정답</summary>
<div markdown="1">

```javascript
function count(obj) {
  return Object.keys(obj).length;
}
```

</div>
</details>

<details>
<summary>반성</summary>
<div markdown="1">
할게 없넹
</div>
</details>
