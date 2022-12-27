나머지 매개변수와 스프레드 문법
=============

> 나머지 매개변수 ...

남아있는 매개변수들을 한데 모아 배열에 집어넣는다.

```javascript
function showName(firstName, lastName, ...titles) {
  alert( firstName + ' ' + lastName ); // Bora Lee

  // 나머지 인수들은 배열 titles의 요소가 됩니다.
  // titles = ["Software Engineer", "Researcher"]
  alert( titles[0] ); // Software Engineer
  alert( titles[1] ); // Researcher
  alert( titles.length ); // 2
}

showName("Bora", "Lee", "Software Engineer", "Researcher");
```

⚠️ 항상 마지막에 선언해야한다.

<br>

> arguments

유사 배열 객체(array-like object)인 arguments를 사용하면 인덱스를 사용해 인수에 접근할 수 있다.

```javascript
function showName() {
  alert( arguments.length );
  alert( arguments[0] );
  alert( arguments[1] );

  // arguments는 이터러블 객체이기 때문에
  // for(let arg of arguments) alert(arg); 를 사용해 인수를 펼칠 수 있습니다.
}

// 2, Bora, Lee가 출력됨
showName("Bora", "Lee");

// 1, Bora, undefined가 출력됨(두 번째 인수는 없음)
showName("Bora");
```

arguments는 유사 배열 객체이면서 이터러블(반복 가능한) 객체이다. 즉 배열이 아니므로 배열 메서드를 사용할 수 없다는 단점이 있다. (arguments.map (...) X)

배열 메서드를 사용하거나 인수 일부만 사용할 때는 나머지 매개변수를 사용하는게 좋다

<br>
⚠️ 화살표 함수는 arguments 객체를 지원하지 않는다.
화살표 함수에서 arguments 객체에 접근하면, 외부에 있는 ‘일반’ 함수의 arguments 객체를 가져온다.

```javascript
function f() {
  let showArg = () => alert(arguments[0]);
  showArg();
}
f(1); // 1
```

<br>

> 스프레드 문법

배열을 통째로 매개변수에 넘겨주는 것

```javascript
let arr = [3, 5, 1];

alert( Math.max(arr) ); // NaN

//-------------------- 

let arr = [3, 5, 1];

alert( Math.max(...arr) ); // 5 (스프레드 문법이 배열을 인수 목록으로 바꿔주었습니다.)

//-------------------- 

let arr1 = [1, -2, 3, 4];
let arr2 = [8, 3, -8, 1];

alert( Math.max(...arr1, ...arr2) ); // 8

//-------------------- 

let arr = [3, 5, 1];
let arr2 = [8, 9, 15];

let merged = [0, ...arr, 2, ...arr2];

alert(merged); // 0,3,5,1,2,8,9,15 (0, arr, 2, arr2 순서로 합쳐집니다.)

```

<br>

ℹ️ Array.from(obj)와 [...obj]의 차이

```javascript
let str = "Hello";

alert( [...str] ); // H,e,l,l,o

let str = "Hello";

alert( Array.from(str) ); // H,e,l,l,o
```

- Array.from은 유사 배열 객체와 이터러블 객체 둘 다에 사용할 수 있다.
- 스프레드 문법은 이터러블 객체에만 사용할 수 있다.

이런 이유때문에 무언가를 배열로 바꿀 때는 스프레드 문법보다 Array.from이 보편적으로 사용된다.


<br>

> 배열과 객체의 복사본 만들기

```javascript
let arr = [1, 2, 3];
let arrCopy = [...arr]; // 배열을 펼쳐서 각 요소를 분리후, 매개변수 목록으로 만든 다음에
                        // 매개변수 목록을 새로운 배열에 할당함

// 배열 복사본의 요소가 기존 배열 요소와 진짜 같을까요?
alert(JSON.stringify(arr) === JSON.stringify(arrCopy)); // true

// 두 배열은 같을까요?
alert(arr === arrCopy); // false (참조가 다름)

// 참조가 다르므로 기존 배열을 수정해도 복사본은 영향을 받지 않습니다.
arr.push(4);
alert(arr); // 1, 2, 3, 4
alert(arrCopy); // 1, 2, 3


// --------------------------


let obj = { a: 1, b: 2, c: 3 };
let objCopy = { ...obj }; // 객체를 펼쳐서 각 요소를 분리후, 매개변수 목록으로 만든 다음에
                          // 매개변수 목록을 새로운 객체에 할당함

// 객체 복사본의 프로퍼티들이 기존 객체의 프로퍼티들과 진짜 같을까요?
alert(JSON.stringify(obj) === JSON.stringify(objCopy)); // true

// 두 객체는 같을까요?
alert(obj === objCopy); // false (참조가 다름)

// 참조가 다르므로 기존 객체를 수정해도 복사본은 영향을 받지 않습니다.
obj.d = 4;
alert(JSON.stringify(obj)); // {"a":1,"b":2,"c":3,"d":4}
alert(JSON.stringify(objCopy)); // {"a":1,"b":2,"c":3}

```

훨씬 간단하게 구현 객체를 복사할 수 있다.
