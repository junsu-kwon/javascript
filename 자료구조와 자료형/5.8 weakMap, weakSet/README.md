WeakMap
=============

> 설명

```javascript
let john = { name: "John" };

// 위 객체는 john이라는 참조를 통해 접근할 수 있습니다.

// 그런데 참조를 null로 덮어쓰면 위 객체에 더 이상 도달이 가능하지 않게 되어
john = null;

// 객체가 메모리에서 삭제됩니다.

```

자료구조를 구성하는 요소도 자신이 속한 자료구조가 메모리에 남아있는 동안 대개 도달 가능한 값으로 취급되어 메모리에서 삭제되지 않습니다. 객체의 프로퍼티나 배열의 요소, 맵이나 셋을 구성하는 요소들이 이에 해당합니다.

```javascript
let john = { name: "John" };

let array = [ john ];

john = null; // 참조를 null로 덮어씀

// john을 나타내는 객체는 배열의 요소이기 때문에 가비지 컬렉터의 대상이 되지 않습니다.
// array[0]을 이용하면 해당 객체를 얻는 것도 가능합니다.
alert(JSON.stringify(array[0]));
```

맵과 위크맵의 첫 번째 차이는 위크맵의 키가 반드시 객체여야 한다는 점입니다. 원시값은 위크맵의 키가 될 수 없습니다.

위크맵의 키로 사용된 객체를 참조하는 것이 아무것도 없다면 해당 객체는 메모리와 위크맵에서 자동으로 삭제됩니다.

```javascript
let john = { name: "John" };

let weakMap = new WeakMap();
weakMap.set(john, "...");

john = null; // 참조를 덮어씀

// john을 나타내는 객체는 이제 메모리에서 지워집니다!
```

<br>

> 위크맵이 지원하는 메서드
* weakMap.get(key)
* weakMap.set(key, value)
* weakMap.delete(key)
* eakMap.has(key)

<br>

> 활용1 : 추가 데이터

위크맵은 부차적인 데이터를 저장할 곳이 필요할 때 그 진가를 발휘합니다.

서드파티 라이브러리와 같은 외부 코드에 ‘속한’ 객체를 가지고 작업을 해야 한다고 가정해 봅시다. 이 객체에 데이터를 추가해줘야 하는데, 추가해 줄 데이터는 객체가 살아있는 동안에만 유효한 상황입니다. 이럴 때 위크맵을 사용할 수 있습니다.

위크맵에 원하는 데이터를 저장하고, 이때 키는 객체를 사용하면 됩니다. 이렇게 하면 객체가 가비지 컬렉션의 대상이 될 때, 데이터도 함께 사라지게 됩니다.

```javascript
// 📁 visitsCount.js
let visitsCountMap = new Map(); // 맵에 사용자의 방문 횟수를 저장함

// 사용자가 방문하면 방문 횟수를 늘려줍니다.
function countUser(user) {
  let count = visitsCountMap.get(user) || 0;
  visitsCountMap.set(user, count + 1);
}

```
아래는 John이라는 사용자가 방문했을 때, 어떻게 방문 횟수가 증가하는지를 보여줍니다.

```javascript
// 📁 main.js
let john = { name: "John" };

countUser(john); // John의 방문 횟수를 증가시킵니다.

// John의 방문 횟수를 셀 필요가 없어지면 아래와 같이 john을 null로 덮어씁니다.
john = null;
```

이제 john을 나타내는 객체는 가비지 컬렉션의 대상이 되어야 하는데, visitsCountMap의 키로 사용되고 있어서 메모리에서 삭제되지 않습니다.

특정 사용자를 나타내는 객체가 메모리에서 사라지면 해당 객체에 대한 정보(방문 횟수)도 우리가 손수 지워줘야 하는 상황입니다. 이렇게 하지 않으면 visitsCountMap가 차지하는 메모리 공간이 한없이 커질 겁니다. 애플리케이션 구조가 복잡할 땐, 이렇게 쓸모 없는 데이터를 수동으로 비워주는 게 꽤 골치 아픕니다.

이런 문제는 위크맵을 사용해 예방할 수 있습니다.

```javascript
// 📁 visitsCount.js
let visitsCountMap = new WeakMap(); // 위크맵에 사용자의 방문 횟수를 저장함

// 사용자가 방문하면 방문 횟수를 늘려줍니다.
function countUser(user) {
  let count = visitsCountMap.get(user) || 0;
  visitsCountMap.set(user, count + 1);
}
```

위크맵을 사용해 사용자 방문 횟수를 저장하면 visitsCountMap을 수동으로 청소해줄 필요가 없습니다. john을 나타내는 객체가 도달 가능하지 않은 상태가 되면 자동으로 메모리에서 삭제되기 때문입니다. 위크맵의 키(john)에 대응하는 값(john의 방문 횟수)도 자동으로 가비지 컬렉션의 대상이 됩니다.

<br>

> 활용2 : 캐싱

위크맵은 캐싱(caching)이 필요할 때 유용합니다. 캐싱은 시간이 오래 걸리는 작업의 결과를 저장해서 연산 시간과 비용을 절약해주는 기법입니다. 동일한 함수를 여러 번 호출해야 할 때, 최초 호출 시 반환된 값을 어딘가에 저장해 놓았다가 그다음엔 함수를 호출하는 대신 저장된 값을 사용하는 게 캐싱의 실례입니다.

아래 예시는 함수 연산 결과를 맵에 저장하고 있습니다.

```javascript
// 📁 cache.js
let cache = new Map();

// 연산을 수행하고 그 결과를 맵에 저장합니다.
function process(obj) {
  if (!cache.has(obj)) {
    let result = /* 연산 수행 */ obj;

    cache.set(obj, result);
  }

  return cache.get(obj);
}

// 함수 process()를 호출해봅시다.

// 📁 main.js
let obj = {/* ... 객체 ... */};

let result1 = process(obj); // 함수를 호출합니다.

// 동일한 함수를 두 번째 호출할 땐,
let result2 = process(obj); // 연산을 수행할 필요 없이 맵에 저장된 결과를 가져오면 됩니다.

// 객체가 쓸모없어지면 아래와 같이 null로 덮어씁니다.
obj = null;

alert(cache.size); // 1 (엇! 그런데 객체가 여전히 cache에 남아있네요. 메모리가 낭비되고 있습니다.)
```
process(obj)를 여러 번 호출하면 최초 호출할 때만 연산이 수행되고, 그 이후엔 연산 결과를 cache에서 가져옵니다. 그런데 맵을 사용하고 있어서 객체가 필요 없어져도 cache를 수동으로 청소해 줘야 합니다.

맵을 위크맵으로 교체하면 이런 문제를 예방할 수 있습니다. 객체가 메모리에서 삭제되면, 캐시에 저장된 결과(함수 연산 결과) 역시 메모리에서 자동으로 삭제되기 때문입니다.

```javascript
// 📁 cache.js
let cache = new WeakMap();

// 연산을 수행하고 그 결과를 위크맵에 저장합니다.
function process(obj) {
  if (!cache.has(obj)) {
    let result = /* 연산 수행 */ obj;

    cache.set(obj, result);
  }

  return cache.get(obj);
}

// 📁 main.js
let obj = {/* ... 객체 ... */};

let result1 = process(obj);
let result2 = process(obj);

// 객체가 쓸모없어지면 아래와 같이 null로 덮어씁니다.
obj = null;

// 이 예시에선 맵을 사용한 예시처럼 cache.size를 사용할 수 없습니다.
// 하지만 obj가 가비지 컬렉션의 대상이 되므로, 캐싱된 데이터 역시 메모리에서 삭제될 겁니다.
// 삭제가 진행되면 cache엔 그 어떤 요소도 남아있지 않을겁니다.
```

<br>
<br>

WeakSet
=============

> 설명

- 위크셋은 셋과 유사한데, 객체만 저장할 수 있다는 점이 다릅니다. 원시값은 저장할 수 없습니다.

- 셋 안의 객체는 도달 가능할 때만 메모리에서 유지됩니다.

- 셋과 마찬가지로 위크셋이 지원하는 메서드는 단출합니다. add, has, delete를 사용할 수 있고, size, keys()나 반복 작업 관련 메서드는 사용할 수 없습니다.

'위크’맵과 유사하게 '위크’셋도 부차적인 데이터를 저장할 때 사용할 수 있습니다. 다만, 위크셋엔 위크맵처럼 복잡한 데이터를 저장하지 않습니다. 대신 "예"나 “아니오” 같은 간단한 답변을 얻는 용도로 사용됩니다. 물론 위크셋에 저장되는 값들은 객체이겠죠.

예시와 함께 위크셋의 용도를 알아봅시다. 아래 코드에선 사용자의 사이트 방문 여부를 추적하는 용도로 위크셋을 사용하고 있습니다.


```javascript
let visitedSet = new WeakSet();

let john = { name: "John" };
let pete = { name: "Pete" };
let mary = { name: "Mary" };

visitedSet.add(john); // John이 사이트를 방문합니다.
visitedSet.add(pete); // 이어서 Pete가 사이트를 방문합니다.
visitedSet.add(john); // 이어서 John이 다시 사이트를 방문합니다.

// visitedSet엔 두 명의 사용자가 저장될 겁니다.

// John의 방문 여부를 확인해보겠습니다.
alert(visitedSet.has(john)); // true

// Mary의 방문 여부를 확인해보겠습니다.
alert(visitedSet.has(mary)); // false

john = null;

// visitedSet에서 john을 나타내는 객체가 자동으로 삭제됩니다.
```

위크맵과 위크셋의 가장 큰 단점은 반복 작업이 불가능하다는 점입니다. 위크맵이나 위크셋에 저장된 자료를 한 번에 얻는 게 불가능하죠. 이런 단점은 불편함을 초래하는 것 같아 보이지만, 위크맵과 위크셋을 이용해 할 수 있는 주요 작업을 방해하진 않습니다. 위크맵과 위크셋은 객체와 함께 ‘추가’ 데이터를 저장하는 용도로 쓸 수 있습니다.
