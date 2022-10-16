Object.keys, values, entries
=============

> 설명

일반 객체엔 다음과 같은 메서드를 사용할 수 있습니다.

* Object.keys(obj) – 객체의 키만 담은 배열을 반환합니다.
* Object.values(obj) – 객체의 값만 담은 배열을 반환합니다.
* Object.entries(obj) – [키, 값] 쌍을 담은 배열을 반환합니다.

Map, Set, Array 전용 메서드와 일반 객체용 메서드의 차이를 (맵을 기준으로) 비교하면 다음과 같습니다.

||맵|객체|
|------|---|---|
|호출 문법|map.keys()|Object.keys(obj)(obj.keys() 아님)|
|반환 값|iterable 객체|‘진짜’ 배열|

첫 번째 차이는 obj.keys()가 아닌 Object.keys(obj)를 호출한다는 점입니다.

이렇게 문법이 다른 이유는 유연성 때문입니다. 아시다시피 자바스크립트에선 복잡한 자료구조 전체가 객체에 기반합니다. 그러다 보니 객체 data에 자체적으로 data.values()라는 메서드를 구현해 사용하는 경우가 있을 수 있습니다. 이렇게 커스텀 메서드를 구현한 상태라도 Object.values(data)같은 형태로 메서드를 호출할 수 있으면 <span style='color: red'>커스텀 메서드와 내장 메서드 둘 다를 사용할 수 있습니다.</span>

두 번째 차이는 메서드 Object.*를 호출하면 iterable 객체가 아닌 객체의 한 종류인 <span style='color: red'>배열을 반환</span>한다는 점입니다. ‘진짜’ 배열을 반환하는 이유는 하위 호환성 때문입니다.

```javascript
let user = {
  name: "John",
  age: 30
};
```
* Object.keys(user) = ["name", "age"]
* Object.values(user) = ["John", 30]
* Object.entries(user) = [ ["name","John"], ["age",30] ]


```
⚠️Object.keys, values, entries는 심볼형 프로퍼티를 무시합니다.
for..in 반복문처럼, Object.keys, Object.values, Object.entries는 키가 심볼형인 프로퍼티를 무시합니다.
```

<br>

> 객체 변환하기

객체엔 map, filter 같은 배열 전용 메서드를 사용할 수 없습니다.
하지만 Object.entries와 Object.fromEntries를 순차적으로 적용하면 객체에도 배열 전용 메서드 사용할 수 있습니다. 적용 방법은 다음과 같습니다.

1. Object.entries(obj)를 사용해 객체의 키-값 쌍이 요소인 배열을 얻습니다.
2. 1.에서 만든 배열에 map 등의 배열 전용 메서드를 적용합니다.
3. 2.에서 반환된 배열에 Object.fromEntries(array)를 적용해 배열을 다시 객체로 되돌립니다.

```javascript
let prices = {
  banana: 1,
  orange: 2,
  meat: 4,
};

let doublePrices = Object.fromEntries(
  // 객체를 배열로 변환해서 배열 전용 메서드인 map을 적용하고 fromEntries를 사용해 배열을 다시 객체로 되돌립니다.
  Object.entries(prices).map(([key, value]) => [key, value * 2])
);

alert(doublePrices.meat); // 8
```