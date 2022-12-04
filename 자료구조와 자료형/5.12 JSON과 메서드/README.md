JSON과 메서드
=============

> 설명

JSON은 본래 자바스크립트에서 사용할 목적으로 만들어진 포맷입니다. 그런데 라이브러리를 사용하면 자바스크립트가 아닌 언어에서도 JSON을 충분히 다룰 수 있어서, JSON을 데이터 교환 목적으로 사용하는 경우가 많습니다. 특히 클라이언트 측 언어가 자바스크립트일 때 말이죠. 서버 측 언어는 무엇이든 상관없습니다.

- JSON.stringify - 객체를 JSON으로 바꿔줍니다.
- JSON.parse - JSON을 객체로 바꿔줍니다.

<br>

> JSON.stringify 

기본 형식
```javascript
let json = JSON.stringify(value[, replacer, space])
```
- value : 값

- replacer : JSON으로 인코딩 하길 원하는 프로퍼티가 담긴 배열. 또는 매핑 함수 function(key, value)

- space : 서식 변경 목적으로 사용할 공백 문자 수, 가독성을 높이기 위해 중간에 삽입해 줄 공백 문자 수를 나타냅니다.

<br>

```javascript
let student = {
  name: 'John',
  age: 30,
  isAdmin: false,
  courses: ['html', 'css', 'js'],
  wife: null
};

let json = JSON.stringify(student);

alert(typeof json); // 문자열이네요!

alert(json);
/* JSON으로 인코딩된 객체:
{
  "name": "John",
  "age": 30,
  "isAdmin": false,
  "courses": ["html", "css", "js"],
  "wife": null
}
*/
```
객체는 이렇게 문자열로 변환된 후에야 비로소 네트워크를 통해 전송하거나 저장소에 저장할 수 있습니다.

<br>
ℹ️ JSON으로 인코딩된 객체와 일반 객체의 서로 다른 특징

1.  문자열은 큰따옴표로 감싸야 합니다. JSON에선 작은따옴표나 백틱을 사용할 수 없습니다('John'이 "John"으로 변경된 것을 통해 이를 확인할 수 있습니다).
2. 객체 프로퍼티 이름은 큰따옴표로 감싸야 합니다(age:30이 "age":30으로 변한 것을 통해 이를 확인할 수 있습니다).

JSON.stringify는 객체뿐만 아니라 원시값에도 적용할 수 있습니다.

<br>
ℹ️ 적용할 수 있는 자료형

- 객체 { ... }
- 배열 [ ... ]
- 원시형:
    - 문자형
    - 숫자형
    - 불린형 값 true와 false
    - null

<br>
ℹ️ 변환되지 않는 자료형

자바스크립트 특유의 객체 프로퍼티는 JSON.stringify가 처리할 수 없습니다.

- 함수 프로퍼티 (메서드)
- 심볼형 프로퍼티 (키가 심볼인 프로퍼티)
- 값이 undefined인 프로퍼티

<br>
ℹ️ 중첩객체도 포함

```javascript
let meetup = {
  title: "Conference",
  room: {
    number: 23,
    participants: ["john", "ann"]
  }
};

alert( JSON.stringify(meetup) );
/* 객체 전체가 문자열로 변환되었습니다.
{
  "title":"Conference",
  "room":{"number":23,"participants":["john","ann"]},
}
*/
```

<br>
⚠️ 순환참조는 불가

```javascript
let room = {
  number: 23
};

let meetup = {
  title: "Conference",
  participants: ["john", "ann"]
};

meetup.place = room;       // meetup은 room을 참조합니다.
room.occupiedBy = meetup; // room은 meetup을 참조합니다.

JSON.stringify(meetup); // Error: Converting circular structure to JSON
```
<br>

ℹ️ toJSON

객체에 toJSON이라는 메서드가 구현되어 있으면 객체를 JSON으로 바꿀 수 있을 겁니다. JSON.stringify는 이런 경우를 감지하고 toJSON을 자동으로 호출해줍니다.

```javascript
let room = {
  number: 23
};

let meetup = {
  title: "Conference",
  date: new Date(Date.UTC(2017, 0, 1)),
  room
};

alert( JSON.stringify(meetup) );
/*
  {
    "title":"Conference",
    "date":"2017-01-01T00:00:00.000Z",  // (1)
    "room": {"number":23}               // (2)
  }
*/
```
Date 객체에 toJSON이 내장

<br>

```javascript
let room = {
  number: 23,
  toJSON() {
    return this.number;
  }
};

let meetup = {
  title: "Conference",
  room
};

alert( JSON.stringify(room) ); // 23

alert( JSON.stringify(meetup) );
/*
  {
    "title":"Conference",
    "room": 23
  }
*/
```
이런식으로 커스텀

<br>
<br>

> JSON.parse 

```javascript
let value = JSON.parse(str, [reviver]);
```

- str
JSON 형식의 문자열

- reviver
모든 (key, value) 쌍을 대상으로 호출되는 function(key,value) 형태의 함수로 값을 변경시킬 수 있습니다.


```javascript
// 문자열로 변환된 배열
let numbers = "[0, 1, 2, 3]";

numbers = JSON.parse(numbers);

alert( numbers[1] ); // 1

// 중첩객체도 가능
let userData = '{ "name": "John", "age": 35, "isAdmin": false, "friends": [0,1,2,3] }';

let user = JSON.parse(userData);

alert( user.friends[1] ); // 1
```

<br>

⚠️ 자주하는 실수

```javascript
let json = `{
  name: "John",                     
  // 실수 1: 프로퍼티 이름을 큰따옴표로 감싸지 않았습니다.
  
  "surname": 'Smith',               
  // 실수 2: 프로퍼티 값은 큰따옴표로 감싸야 하는데, 작은따옴표로 감쌌습니다.

  'isAdmin': false                  
  // 실수 3: 프로퍼티 키는 큰따옴표로 감싸야 하는데, 작은따옴표로 감쌌습니다.

  "birthday": new Date(2000, 2, 3), 
  // 실수 4: "new"를 사용할 수 없습니다. 순수한 값(bare value)만 사용할 수 있습니다.

  "friends": [0,1,2,3]              
  // 이 프로퍼티는 괜찮습니다.
}`;
```
JSON은 주석을 지원하지 않는다는 점도 기억해 놓으시기 바랍니다. 주석을 추가하면 유효하지 않은 형식이 됩니다.


ℹ️ reviver 사용하기

```javascript
let str = '{"title":"Conference","date":"2017-11-30T12:00:00.000Z"}';

let meetup = JSON.parse(str);

alert( meetup.date.getDate() ); // 에러!
```

<br>

모든 값은 “그대로”, 하지만 date만큼은 Date 객체를 반환하도록 함수를 구현
```javascript
let str = '{"title":"Conference","date":"2017-11-30T12:00:00.000Z"}';

let meetup = JSON.parse(str, function(key, value) {
  if (key == 'date') return new Date(value);
  return value;
});

alert( meetup.date.getDate() ); // 이제 제대로 동작하네요!
```
중첩 객체에도 적용