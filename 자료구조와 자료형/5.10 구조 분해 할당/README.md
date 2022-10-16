구조 분해 할당
=============

> 배열 분해하기

```javascript
// 이름과 성을 요소로 가진 배열
let arr = ["Bora", "Lee"]

// 구조 분해 할당을 이용해
// firstName엔 arr[0]을
// surname엔 arr[1]을 할당하였습니다.
let [firstName, surname] = arr;

alert(firstName); // Bora
alert(surname);  // Lee

// 활용 예시
let [firstName, surname] = "Bora Lee".split(' ');
```

<br>
ℹ️ 쉼표를 사용하여 요소 무시하기

쉼표를 사용하면 필요하지 않은 배열 요소를 버릴 수 있습니다.
```javascript
// 두 번째 요소는 필요하지 않음
let [firstName, , title] = ["Julius", "Caesar", "Consul", "of the Roman Republic"];

alert( title ); // Consul
```

<br>
ℹ️ 할당 연산자 우측엔 모든 이터러블이 올 수 있습니다.

배열뿐만 아니라 모든 이터러블(iterable, 반복 가능한 객체)에 구조 분해 할당을 적용할 수 있습니다.

```javascript
let [a, b, c] = "abc"; // ["a", "b", "c"]
let [one, two, three] = new Set([1, 2, 3]);
```

<br>
ℹ️ .entries()로 반복하기

```javascript
let user = {
  name: "John",
  age: 30
};

// 객체의 키와 값 순회하기
for (let [key, value] of Object.entries(user)) {
  alert(`${key}:${value}`); // name:John, age:30이 차례대로 출력
}
```

<br>
ℹ️ 변수 교환 트릭

```javascript
let guest = "Jane";
let admin = "Pete";

// 변수 guest엔 Pete, 변수 admin엔 Jane이 저장되도록 값을 교환함
[guest, admin] = [admin, guest];

alert(`${guest} ${admin}`); // Pete Jane(값 교환이 성공적으로 이뤄졌습니다!)
```

<br>
ℹ️ '…'로 나머지 요소 가져오기

``` javascript
let [name1, name2, ...rest] = ["Julius", "Caesar", "Consul", "of the Roman Republic"];

alert(name1); // Julius
alert(name2); // Caesar

// `rest`는 배열입니다.
alert(rest[0]); // Consul
alert(rest[1]); // of the Roman Republic
alert(rest.length); // 2
```

⚠️ 변수 앞의 점 세 개(...)와 변수가 가장 마지막에 위치해야 한다는 점은 지켜주시기 바랍니다.

<br>
ℹ️ 기본값

```javascript
// 기본값
// 함수도 가능
let [name = "Guest", surname = prompt('성을 입력하세요.')] = ["Julius"];
```

<br>

> 객체 분해하기

```javascript
let {var1, var2} = {var1:…, var2:…}
```
할당 연산자 우측엔 분해하고자 하는 객체를, 좌측엔 상응하는 객체 프로퍼티의 '패턴’을 넣습니다. 분해하려는 객체 프로퍼티의 키 목록을 패턴으로 사용하는 예시를 살펴봅시다.

```javascript
let options = {
  title: "Menu",
  width: 100,
  height: 200
};

let {title, width, height} = options;

alert(title);  // Menu
alert(width);  // 100
alert(height); // 200
```

<br>
ℹ️ 다양한 사용 방법

```javascript
let options = {
  title: "Menu",
  width: 100,
  height: 200
};

let {width = 100, height: h = 200, title = prompt("title?")} = options;
```

<br>
ℹ️ 나머지 패턴 '...'

나머지 패턴(rest pattern)을 사용하면 배열에서 했던 것처럼 나머지 프로퍼티를 어딘가에 할당하는 게 가능합니다. 참고로 모던 브라우저는 나머지 패턴을 지원하지만, IE를 비롯한 몇몇 구식 브라우저는 나머지 패턴을 지원하지 않으므로 주의해서 사용해야 합니다. 물론 바벨(Babel)을 이용하면 되지만요.

```javascript
let options = {
  title: "Menu",
  height: 200,
  width: 100
};

// title = 이름이 title인 프로퍼티
// rest = 나머지 프로퍼티들
let {title, ...rest} = options;

// title엔 "Menu", rest엔 {height: 200, width: 100}이 할당됩니다.
alert(rest.height);  // 200
alert(rest.width);   // 100
```

⚠️ 주의 : let 없이 사용하기

```javascript
let title, width, height;

// SyntaxError: Unexpected token '=' 이라는 에러가 아랫줄에서 발생합니다.
{title, width, height} = {title: "Menu", width: 200, height: 100};
```

```javascript
let title, width, height;

// 에러가 발생하지 않습니다.
({title, width, height} = {title: "Menu", width: 200, height: 100});

alert( title ); // Menu
```

<br>
ℹ️ 똑똑한 함수 매개변수

리팩토링 전
```javascript
function showMenu(title = "Untitled", width = 200, height = 100, items = []) {
  // ...
}

// 기본값을 사용해도 괜찮은 경우 아래와 같이 undefined를 여러 개 넘겨줘야 합니다.
showMenu("My Menu", undefined, undefined, ["Item1", "Item2"])
```

리팩토링 후
```javascript
// 함수에 전달할 객체
let options = {
  title: "My menu",
  items: ["Item1", "Item2"]
};

function showMenu({
  title = "Untitled",
  width: w = 100,  // width는 w에,
  height: h = 200, // height는 h에,
  items: [item1, item2] // items의 첫 번째 요소는 item1에, 두 번째 요소는 item2에 할당함
}) {
  alert( `${title} ${w} ${h}` ); // My Menu 100 200
  alert( item1 ); // Item1
  alert( item2 ); // Item2
}

showMenu(options);
```

⚠️ 참고로 이렇게 함수 매개변수를 구조 분해할 땐, 반드시 인수가 전달된다고 가정되고 사용된다는 점에 유의하시기 바랍니다. 모든 인수에 기본값을 할당해 주려면 빈 객체를 명시적으로 전달해야 합니다.

```javascript
showMenu({}); // 모든 인수에 기본값이 할당됩니다.

showMenu(); // 에러가 발생할 수 있습니다.
```

해결방법
```javascript
function showMenu({ title = "Menu", width = 100, height = 200 } = {}) {
  alert( `${title} ${width} ${height}` );
}

showMenu(); // Menu 100 200
```