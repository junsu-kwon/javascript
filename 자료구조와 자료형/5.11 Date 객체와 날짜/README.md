Date 객체와 날짜
=============

> 설명

```javascript
// 현재 날짜 및 시간
new Date();

// 1970년 1월 1일 0시 0분 0초(UTC+0)를 나타내는 객체
new Date(0);

// 1970년 1월 1일의 24시간 후는 1970년 1월 2일(UTC+0)임
new Date(24 * 3600 * 1000);

// 1970년 1월 1일 이전 날짜에 해당하는 타임스탬프 값은 음수입니다 31 Dec 1969 
new Date(-24 * 3600 * 1000);

new Date("2017-01-26");
// 인수로 시간은 지정하지 않았기 때문에 GMT 자정이라고 가정하고
// 코드가 실행되는 시간대(timezone)에 따라 출력 문자열이 바뀝니다.
// 따라서 얼럿 창엔
// Thu Jan 26 2017 11:00:00 GMT+1100 (Australian Eastern Daylight Time)
// 혹은
// Wed Jan 25 2017 16:00:00 GMT-0800 (Pacific Standard Time)등이 출력됩니다.
```

```javascript
new Date(year, month, date, hours, minutes, seconds, ms)
```
주어진 인수를 조합해 만들 수 있는 날짜가 저장된 객체가 반환됩니다(지역 시간대 기준). 첫 번째와 두 번째 인수만 필수값입니다.

* year는 반드시 네 자리 숫자여야 합니다. 2013은 괜찮고 98은 괜찮지 않습니다.
* month는 0(1월)부터 11(12월) 사이의 숫자여야 합니다.
* date는 일을 나타내는데, 값이 없는 경우엔 1일로 처리됩니다.
* hours/minutes/seconds/ms에 값이 없는 경우엔 0으로 처리됩니다.

```javascript
new Date(2011, 0, 1, 0, 0, 0, 0); // 2011년 1월 1일, 00시 00분 00초
new Date(2011, 0, 1); // hours를 비롯한 인수는 기본값이 0이므로 위와 동일
new Date(2011, 0, 1, 2, 3, 4, 567);// 최소 정밀도는 1밀리초(1/1000초)입니다.  2011년 1월 1일, 02시 03분 04.567초
```

<br>

> 날짜 구성요소 얻기

* **getFullYear()**

    연도(네 자릿수)를 반환합니다.
* **getMonth()**

    월을 반환합니다(0 이상 11 이하).
* **getDate()**

    일을 반환합니다(1 이상 31 이하). 어! 그런데 메서드 이름이 뭔가 이상하네요.
* **getHours(), getMinutes(), getSeconds(), getMilliseconds()**

    시, 분, 초, 밀리초를 반환합니다.
* **getDay()**

    일요일을 나타내는 0부터 토요일을 나타내는 6까지의 숫자 중 하나를 반환합니다. 

<br>

> 날짜 구성요소 설정하기


* **setFullYear(year, [month], [date])**
* **setMonth(month, [date])**
* **setDate(date)**
* **setHours(hour, [min], [sec], [ms])**
* **setMinutes(min, [sec], [ms])**
* **setSeconds(sec, [ms])**
* **setMilliseconds(ms)**
* **setTime(milliseconds) (1970년 1월 1일 00:00:00 UTC부터 밀리초 이후를 나타내는 날짜를 설정)**

<br>

> 자동 고침

Date 객체엔 자동 고침(autocorrection) 이라는 유용한 기능이 있습니다. 범위를 벗어나는 값을 설정하려고 하면 자동 고침 기능이 활성화되면서 값이 자동으로 수정됩니다.

```javascript
let date = new Date(2013, 0, 32); // 2013년 1월 32일은 없습니다.
alert(date); // 2013년 2월 1일이 출력됩니다.

//------------
let date = new Date(2016, 1, 28);
date.setDate(date.getDate() + 2);
alert( date ); // 2016년 3월 1일

//------------
let date = new Date();
date.setSeconds(date.getSeconds() + 70);
alert( date ); // 70초 후의 날짜가 출력됩니다.

//------------
let date = new Date(2016, 0, 2); // 2016년 1월 2일
date.setDate(0); // 일의 최솟값은 1이므로 0을 입력하면 전 달의 마지막 날을 설정한 것과 같은 효과를 봅니다.
alert( date ); // 31 Dec 2015
```

<br>

> Date 객체를 숫자로 변경해 시간차 측정하기

```javascript
let date = new Date();
alert(+date); // 타임스탬프(date.getTime()를 호출한 것과 동일함)
```


```javascript
// 나만의 스톱워치
let start = new Date(); // 측정 시작

// 원하는 작업을 수행
for (let i = 0; i < 100000; i++) {
  let doSomething = i * i * i;
}

let end = new Date(); // 측정 종료

alert( `반복문을 모두 도는데 ${end - start} 밀리초가 걸렸습니다.` );
```
ℹ️ Date.now()

Date 객체를 만들지 않고도 시차를 측정할 방법이 있습니다.

현재 타임스탬프를 반환하는 메서드 Date.now()를 응용하면 됩니다.

Date.now()는 new Date().getTime()과 의미론적으로 동일하지만 중간에 **Date 객체를 만들지 않는다는 점**이 다릅니다. 따라서 new Date().getTime()를 사용하는 것보다 빠르고 **가비지 컬렉터의 일을 덜어준다는 장점**이 있습니다.

자바스크립트를 사용해 게임을 구현한다던가 전문분야의 애플리케이션을 구현할 때와 같이 성능이 중요한 경우에 Date.now()가 자주 활용됩니다. 물론 편의를 위해 활용되기도 하죠.

위 예시를 Date.now()를 사용해 변경하면 성능이 더 좋습니다.
```javascript
let start = Date.now(); // 1970년 1월 1일부터 현재까지의 밀리초

// 원하는 작업을 수행
for (let i = 0; i < 100000; i++) {
  let doSomething = i * i * i;
}

let end = Date.now(); // done

alert( `반복문을 모두 도는데 ${end - start} 밀리초가 걸렸습니다.` ); // Date 객체가 아닌 숫자끼리 차감함
```

<br>

> 벤치마크 테스트

```javascript
// 두 함수 중 date1과 date2의 차이를 어떤 함수가 더 빨리 반환할까요?
function diffSubtract(date1, date2) {
  return date2 - date1;
}

// 반환 값은 밀리초입니다.
function diffGetTime(date1, date2) {
  return date2.getTime() - date1.getTime();
}
```
두 함수는 완전히 동일한 작업을 수행하지만
getTime()을 이용한 방법이 형 변환이 없어서 엔진 최적화에 드는 자원이 줄어들므로 훨씬 빠릅니다.

<br>

> Date.parse와 문자열

메서드 Date.parse(str)를 사용하면 문자열에서 날짜를 읽어올 수 있습니다.

단, 문자열의 형식은 YYYY-MM-DDTHH:mm:ss.sssZ처럼 생겨야 합니다.

* YYYY-MM-DD – 날짜(연-월-일)
* "T" – 구분 기호로 쓰임
* HH:mm:ss.sss – 시:분:초.밀리초
* 'Z'(옵션) – +-hh:mm 형식의 시간대를 나타냄. Z 한 글자인 경우엔 UTC+0을 나타냄

YYYY-MM-DD, YYYY-MM, YYYY같이 더 짧은 문자열 형식도 가능합니다.

위 조건을 만족하는 문자열을 대상으로 Date.parse(str)를 호출하면 문자열과 대응하는 날짜의 타임스탬프가 반환됩니다. 문자열의 형식이 조건에 맞지 않은 경우엔 NaN이 반환됩니다.
