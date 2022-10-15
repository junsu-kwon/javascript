과제 모음
=============

> 1번 : '읽음'상태인 메시지 저장하기

메시지가 저장되어 있는 배열이 있습니다.

```javascript
let messages = [
  {text: "Hello", from: "John"},
  {text: "How goes?", from: "John"},
  {text: "See you soon", from: "Alice"}
];
```

여러분이 작성하고 있는 코드를 사용해 이 배열에 접근할 수 있지만, 메시지를 관리하는 건 다른 코드에서 이뤄지고 있는 상황입니다. 새로운 메시지가 있으면 배열에 추가되고, 오래된 메시는 배열에서 삭제되지만 이런 작업은 외부코드에 의해 이뤄지고 있기 때문에 여러분은 언제 메시지가 추가/삭제되는지 알 수 없는 상황이죠.

이와 같은 상황에서 ‘읽음’ 상태의 메시지는 어떤 자료구조에 저장해야 좋을까요? 특정 메시지 객체를 대상으로 "메시지가 읽음 상태인가요?"라는 질문을 던지면 제대로 된 답이 반환되는 자료구조는 무엇일까요?

주의: messages에서 특정 메시지가 삭제되면 여러분이 구현할 자료구조에서도 해당 메시지가 삭제되어야 합니다.

주의: 메시지 객체에 프로퍼티를 추가하는 것과 같이 메시지 객체를 수정해선 안 됩니다. 메시지 객체는 외부코드에서 관리하고 있기 때문에 메시지 객체를 직접 수정하면 예상치 않은 결과가 나타날 수 있습니다.


<details>
<summary>답변</summary>
<div markdown="1">

> 내 생각

주제가 WeakMap이나 WeakSet을 활용하는 건 맞는 거 같은데..
생각나는대로 한번 써보자

일단 결국 저 메시지 데이터에 대하여 
나는 접근권한 중 "읽기" 밖에 하지 못한다는 것이고
저 읽은 메시지 데이터를 읽음상태저장 목적으로 특정 자료구조에
저장을 해야한다.

이때 일반적인 배열이나 Map같은 자료구조에 저장할 경우

```
주의: messages에서 특정 메시지가 삭제되면 여러분이 구현할 자료구조에서도 해당 메시지가 삭제되어야 합니다.
```

해당 조건에서 굉장히 까다롭다는 것을 기억해야한다.

삭제된 메시지 데이터의 처리 방식이
특정 상태값만을 삭제로 표시한 것이라면 크게 고민을 할 필요는 없을 것 같다. 삭제된 항목만 읽어온 후 읽음상태를 저장한 자료구조에서
매치되는 메시지를 제거해주면 되기 때문이다.

그런데 아예 메시지데이터에서 제거가 되버렸다면
이때는 말이 달라진다.

아예 삭제가 되버렸다면 해당 메시지데이터에 대해
읽음 권한밖에 없는 나는 삭제되었는지 아닌지 
알 수 없을 뿐더러 삭제된것을 찾기위해 
읽음상태를 저장한 자료구조에서 
모든 메시지를 비교해가면서 삭제된 항목을 찾아야 될 것이다.

그래서 이런 경우라면 
WeakSet이 유용할 것이다

```
특정 메시지 객체를 대상으로 "메시지가 읽음 상태인가요?"라는 질문을 던지면 제대로 된 답이 반환되는 자료구조는 무엇일까요?
```

라는 질문을 토대로
읽었는지 안읽었는지 판단만 하는 것이라면
Set의 특징과 닮은 WeakSet이 알맞을 것이다.
원본 메시지가 삭제가 되면 자동으로 메모리에서 삭제도 될 것이니깐.

</div>
</details>

<details>
<summary>정답</summary>
<div markdown="1">

```javascript
let messages = [
  {text: "Hello", from: "John"},
  {text: "How goes?", from: "John"},
  {text: "See you soon", from: "Alice"}
];

let readMessages = new WeakSet();

// 메시지 두 개가 읽음 상태로 변경되었습니다.
readMessages.add(messages[0]);
readMessages.add(messages[1]);
// readMessages엔 요소 두 개가 저장됩니다.

// 첫 번째 메시지를 다시 읽어봅시다!
readMessages.add(messages[0]);
// readMessages에는 요소 두 개가 여전히 저장되어 있습니다(중복 요소 없음).

// "'message[0]'은 읽음 상태인가요?"에 대한 답변
alert("message 0은 읽음 상태인가요?: " + readMessages.has(messages[0])); // true

messages.shift();
// 이제 readMessages에는 요소가 하나만 남게 되었습니다(실제 메모리에서 사라지는 건 나중이 되겠지만 말이죠).
```


위크셋을 사용하면 메시지를 저장하고 위크셋 내에 메시지가 있는지 여부를 쉽게 확인할 수 있습니다.

위크셋은 자동으로 자신을 청소한다는 장점이 있습니다. 다만 이 장점 때문에 반복 작업이 불가능해서 읽음 상태의 메시지를 ‘한꺼번에’ 가지고 오지 못한다는 단점도 생깁니다. 배열에 저장된 모든 메시지를 대상으로 반복 작업을 수행해 해당 메시지가 위크셋에 저장되어 있는지 확인하면 읽음 상태의 메시지를 ‘한 번에’ 얻어올 수 있습니다.

위크셋을 사용하지 않고 메시지 객체에 message.isRead=true같은 프로퍼티를 추가해도 메시지가 읽음 상태인지 확인할 수 있습니다. 그런데 messages와 메시지 객체는 외부 코드에서 관리하고 있기 때문에 이 방법을 권장하는 편은 아닙니다. 심볼형 프로퍼티를 사용하면 충돌을 피할 수는 있습니다.

예시:

```javascript
// 우리 코드에서만 심볼형 프로퍼티의 정보를 얻을 수 있습니다.
let isRead = Symbol("isRead");
messages[0][isRead] = true;
```

이렇게 하면 서드파티 코드에서는 위에서 추가한 여분의 프로퍼티를 볼 수 없습니다.

심볼을 사용하면 문제 발생 확률을 낮출 순 있지만, 위크셋을 쓰는 게 보다 건설적인 접근법입니다.

</div>
</details>



<details>
<summary>반성?</summary>
<div markdown="1">

```
문제 내용 중 ..

주의: 메시지 객체에 프로퍼티를 추가하는 것과 같이 메시지 객체를 수정해선 안 됩니다. 메시지 객체는 외부코드에서 관리하고 있기 때문에 메시지 객체를 직접 수정하면 예상치 않은 결과가 나타날 수 있습니다.
```
```
정답 내용 중..

위크셋을 사용하지 않고 메시지 객체에 message.isRead=true같은 프로퍼티를 추가해도 메시지가 읽음 상태인지 확인할 수 있습니다. 
```

외부에서 관리하기 때문에 안된다고 했지만
이렇게 할수도 있다는 뜻인 것 같다.

그리고 Symbol을 사용할지는 꿈에도 몰랐다. 

좀 더 생각의 폭을 넓혀보자!
</div>
</details>

<br>

---

<br>

> 2번 : 읽은 날짜 저장하기

이전 과제처럼 배열에 메시지를 저장하고 있다고 가정해 봅시다.

```javascript
let messages = [
  {text: "Hello", from: "John"},
  {text: "How goes?", from: "John"},
  {text: "See you soon", from: "Alice"}
];
```

이번 문제에선 "메시지를 언제 읽었나요?"라는 질문을 던지면 제대로 된 답이 반환되는 자료구조가 무엇인지 생각해봅시다.

위 문제에선 'yes’나 'no’만 저장해도 괜찮았는데, 이제는 날짜 정보를 저장해야 하고, 이 날짜 정보는 메시지가 기비지 컬렉션의 대상이 되기 전까지만 메모리에 남아있어야 합니다.

참고: Date라는 내장 클래스의 구현체(객체)를 사용하면 날짜를 저장할 수 있습니다(Date클래스에 대해선 추후에 학습할 예정입니다).


<details>
<summary>답변</summary>
<div markdown="1">

> 내 생각

weakMap의 특징 중 하나는 Key값은 무조건 객체여야 하는 것이다.
그렇기 때문에 읽어들인 메시지를 키값으로 읽은 날짜를 지정한다면 어떨까?

```javascript
let messages = [
  { text: "Hello", from: "John" },
  { text: "How goes?", from: "John" },
  { text: "See you soon", from: "Alice" }
];

let dateMessages = new WeakMap();

dateMessages.set(messages[0], Date());
dateMessages.set(messages[1], Date());

console.log(dateMessages.get(dateMessages[0]));
```

이렇게 한다면 

```
이 날짜 정보는 메시지가 기비지 컬렉션의 대상이 되기 전까지만 메모리에 남아있어야 합니다.
```

라는 조건도 만족시킬 수 있다.

</div>
</details>


<details>
<summary>정답</summary>
<div markdown="1">

다음과 같이, 날짜를 저장하기 위해 위크맵을 사용할 수 있습니다.

```javascript
let messages = [
  {text: "Hello", from: "John"},
  {text: "How goes?", from: "John"},
  {text: "See you soon", from: "Alice"}
];

let readMap = new WeakMap();

readMap.set(messages[0], new Date(2017, 1, 1));
// Date 객체는 추후에 배우게 될 것입니다.
```

</div>
</details>


<details>
<summary>반성</summary>
<div markdown="1">

완벽했다(?)

</div>
</details>



