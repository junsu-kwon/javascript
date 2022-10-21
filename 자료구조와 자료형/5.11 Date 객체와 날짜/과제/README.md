과제 모음
=============

> 1번 : 날짜 생성하기

2012년 2월 20일, 오전 3시 12분을 나타내는 Date 객체를 만들어보세요(시간대는 로컬).

그리고 alert 함수를 이용해 생성한 객체를 출력하세요.

<details>
<summary>답변</summary>
<div markdown="1">

```javascript
console.log(Date.parse("2012-02-20T03:12"));
```

</div>
</details>

<details>
<summary>정답</summary>
<div markdown="1">

new Date 생성자는 로컬 시간대를 사용하기 때문에 특별히 지정해주지 않아도 됩니다. 주의할 점은 월이 0부터 시작한다는 것입니다.

따라서 2월은 숫자 1을 사용해 만듭니다.

```javascript
//new Date(year, month, date, hour, minute, second, millisecond)
let d1 = new Date(2012, 1, 20, 3, 12);
alert( d1 );
```

</div>
</details>

<br>

> 2번 : 요일 보여주기

날짜를 입력하면 ‘MO’, ‘TU’, ‘WE’, ‘TH’, ‘FR’, ‘SA’, ‘SU’ 형식으로 요일을 보여주는 함수 getWeekDay(date)를 만들어보세요.

예시:

```javascript
let date = new Date(2012, 0, 3);  // 2012년 1월 3일
alert( getWeekDay(date) );        // "TU"가 출력되어야 합니다.
```

<details>
<summary>답변</summary>
<div markdown="1">

```javascript
let date = new Date(2012, 0, 3); // 2012년 1월 3일

console.log(getWeekDay(date)); // "TU"가 출력되어야 합니다.

function getWeekDay(date) {
  let day_array = ["SU", "MO", "TU", "WE", "TH", "FR", "SA"];
  return day_array[date.getDay()];
}
```

</div>
</details>

<details>
<summary>정답</summary>
<div markdown="1">

```javascript
date.getDay() 메서드는 요일을 나타내는 숫자를 반환합니다(일요일부터 시작).

요일이 담긴 배열을 만들고, date.getDay()를 호출해 반환받은 값을 인덱스로 사용하면 원하는 기능을 구현할 수 있습니다.

function getWeekDay(date) {
  let days = ['SU', 'MO', 'TU', 'WE', 'TH', 'FR', 'SA'];

  return days[date.getDay()];
}

let date = new Date(2014, 0, 3); // 2014년 1월 3일
alert( getWeekDay(date) ); // FR
```

</div>
</details>


<br>

> 3번 : 유럽 기준 달력

유럽국가의 달력은 월요일부터 시작합니다(월요일-1, 화요일-2, … 일요일-7). ‘유럽’ 기준 숫자를 반환해주는 함수 getLocalDay(date)를 만들어보세요.

```javascript
let date = new Date(2019, 11, 5);  // 2019년 11월 5일
alert( getLocalDay(date) );       // 금요일이므로, 5가 출력되어야 함
```


<details>
<summary>답변</summary>
<div markdown="1">

```javascript
let date = new Date(2019, 11, 5);

console.log(getLocalDay(date)); // "TU"가 출력되어야 합니다.

function getLocalDay(date) {
  let day = date.getDay();
  return day == 0 ? 7 : day;
}
```

</div>
</details>

<details>
<summary>정답</summary>
<div markdown="1">

```javascript
function getLocalDay(date) {

  let day = date.getDay();

  if (day == 0) { // 일요일(숫자 0)은 유럽에선 7입니다.
    day = 7;
  }

  return day;
}
```

</div>
</details>


<br>

> 4번 : n일 전 '일' 출력하기

date를 기준으로 days일 전 '일’을 반환하는 함수 getDateAgo(date, days)를 만들어보세요,

오늘이 20일이라면 getDateAgo(new Date(), 1)는 19를, getDateAgo(new Date(), 2)는 18을 반환해야 합니다.

days가 365일 때도 제대로 동작해야 합니다.

```javascript
let date = new Date(2015, 0, 2); // 2015년 1월 2일

alert( getDateAgo(date, 1) ); // 1, (2015년 1월 1일)
alert( getDateAgo(date, 2) ); // 31, (2014년 12월 31일)
alert( getDateAgo(date, 365) ); // 2, (2014년 1월 2일)
```
주의: 함수는 date를 변경하지 않아야 합니다.

<details>
<summary>답변</summary>
<div markdown="1">

```javascript
let date = new Date(2019, 11, 5);

console.log(getLocalDay(date)); // "TU"가 출력되어야 합니다.

function getLocalDay(date) {
  let day = date.getDay();
  return day == 0 ? 7 : day;
}
```

</div>
</details>

<details>
<summary>정답</summary>
<div markdown="1">

```javascript
function getLocalDay(date) {

  let day = date.getDay();

  if (day == 0) { // 일요일(숫자 0)은 유럽에선 7입니다.
    day = 7;
  }

  return day;
}
```

</div>
</details>

<br>

> 5번 : 달의 마지막 일

특정 달의 마지막 일을 반환하는 함수 getLastDayOfMonth(year, month)를 작성해보세요. 반환 값은 30이나 31, 29(2월), 28(2월)이 될 겁니다.

매개변수:

* year – 숫자 4개로 구성된 연(예: 2012)
* month – 월(0부터 11)

윤년인 2012년의 2월은 29가 반환되어야 합니다. getLastDayOfMonth(2012, 1) = 29


<details>
<summary>답변</summary>
<div markdown="1">

```javascript
function getLastDayOfMonth(year, month) {
  return new Date(year, month + 1, 0).getDate();
}

console.log(getLastDayOfMonth(2012, 2));
```

</div>
</details>

<details>
<summary>정답</summary>
<div markdown="1">

다음 달을 나타내는 객체를 만들고 day에는 0을 넘겨주면 됩니다.

```javascript
function getLastDayOfMonth(year, month) {
  let date = new Date(year, month + 1, 0);
  return date.getDate();
}

alert( getLastDayOfMonth(2012, 0) ); // 31
alert( getLastDayOfMonth(2012, 1) ); // 29
alert( getLastDayOfMonth(2013, 1) ); // 28
```

new Date의 세 번째 매개변수의 기본값은 1입니다. 그런데 어떤 숫자를 넘겨줘도 자바스크립트는 이를 자동 조정해줍니다. 0을 넘기면 '첫 번째 일의 1일 전’을 의미하게 됩니다. 이는 '이전 달의 마지막 일’과 동일합니다.

</div>
</details>

<br>

> 6번 : 몇 초나 지났을까요?

오늘 하루가 시작된 이후 몇 초나 지났는지 반환하는 함수 getSecondsToday()를 만들어보세요.

현재 시각이 10:00 am이고, 서머타임이 적용되지 않은 경우라면 아래와 같은 결과가 나와야 합니다.

```javascript
getSecondsToday() == 36000 // (3600 * 10)
```

주의: 어떤 날이든 함수를 호출했을 때, 원하는 결과가 반환되어야 합니다. '오늘’을 나타내는 값을 하드 코딩하지 마세요.

<details>
<summary>답변</summary>
<div markdown="1">

현재시간에서 현재의 자정시간을 mm초단위로 빼준 다음 초단위로 변환시켜준다.

```javascript
function getSecondsToday() {
  return (Date.now() - new Date().setHours(0, 0, 0)) / 1000;
}

console.log(getSecondsToday());
```

</div>
</details>

<details>
<summary>정답</summary>
<div markdown="1">

경과 초를 알려면 오늘 00시 00분 00초를 나타내는 Date 객체를 만들고, '지금’을 나타내는 객체에서 이 객체를 빼야 합니다.

차이는 밀리초 기준이기 때문에 1000을 나눠 초로 변경해야 합니다.

```javascript
function getSecondsToday() {
  let now = new Date();

  // 현재 년, 월, 일을 나타내는 객체를 생성
  let today = new Date(now.getFullYear(), now.getMonth(), now.getDate());

  let diff = now - today; // 차이(ms)
  return Math.round(diff / 1000); // 초로 변환
}

alert( getSecondsToday() );
```

경과 시간, 분, 초를 초로 변환하는 것도 방법이 될 수 있습니다.

```javascript
function getSecondsToday() {
  let d = new Date();
  return d.getHours() * 3600 + d.getMinutes() * 60 + d.getSeconds();
}

alert( getSecondsToday() );
```

</div>
</details>

<details>
<summary>반성</summary>
<div markdown="1">

```javascript
 let d = new Date();
 d.getHours() * 3600 + d.getMinutes() * 60 + d.getSeconds();
```
이렇게 현재시간에서 분해해서 계산할 생각은 못했다 
생각의 범위를 넓히자!

</div>
</details>


<br>

> 7번 : 몇 초나 남았을까요?

오늘 하루가 끝날 때까지 남은 초를 반환해주는 함수 getSecondsToTomorrow()를 만들어보세요.

현재 시각이 23:00이라면 아래와 같은 결과가 나와야 합니다.

```javascript
getSecondsToTomorrow() == 3600
```

주의: 어떤 날이든 함수를 호출했을 때, 원하는 결과가 반환되어야 합니다. '오늘’을 나타내는 값을 하드 코딩하지 마세요.

<details>
<summary>답변</summary>
<div markdown="1">

```javascript
function getSecondsToTomorrow() {
  let date = new Date();
  date.setDate(date.getDate() + 1);
  date.setHours(0, 0, 0);
  return (date - Date.now()) / 1000;
}

console.log(getSecondsToTomorrow());
```

</div>
</details>

<details>
<summary>정답</summary>
<div markdown="1">

오늘 하루가 끝날 때까지 남은 밀리초를 구하려면 '내일 00시 00분 00초’에서 현재 날짜를 빼면 됩니다.

내일을 나타내는 객체를 먼저 만들고, 이후 작업을 하면 됩니다.

```javascript
function getSecondsToTomorrow() {
  let now = new Date();

  // 내일 날짜
  let tomorrow = new Date(now.getFullYear(), now.getMonth(), now.getDate()+1);

  let diff = tomorrow - now; // 차이(ms)
  return Math.round(diff / 1000); // 초로 변환
}
```
```javascript
function getSecondsToTomorrow() {
  let now = new Date();
  let hour = now.getHours();
  let minutes = now.getMinutes();
  let seconds = now.getSeconds();
  let totalSecondsToday = (hour * 60 + minutes) * 60 + seconds;
  let totalSecondsInADay = 86400;

  return totalSecondsInADay - totalSecondsToday;
}
```

일광 절약 시간제(Daylight Savings Time, DST)를 실시하는 나라에선 하루가 23시간 혹은 25시간으로 구성될 수 있습니다. 이런 경우는 따로 다뤄야 합니다.

</div>
</details>

<details>
<summary>반성</summary>
<div markdown="1">


</div>
</details>


<br>

> 8번 : 상대 날짜 출력하기

date를 아래와 같은 형식으로 변경해주는 함수 formatDate(date)를 만들어보세요.

* date가 지금으로부터 1초 미만 전의 날짜를 나타내면 "현재"를 반환해야 합니다.
* 그렇지 않고, date가 지금으로부터 1분 이하 전의 날짜를 나타내면 "n초 전"을 반환해야 합니다.
* 그렇지 않고, date가 지금으로부터 1시간 미만 전의 날짜를 나타내면 "n분 전"을 반환해야 합니다.
* 이 외의 경우는 전체 날짜를 "DD.MM.YY HH:mm"형식("일.월.연 시:분")으로 반환해야 합니다(예시: 31.12.16 10:00).

예시:

```javascript
alert( formatDate(new Date(new Date - 1)) ); // "현재"

alert( formatDate(new Date(new Date - 30 * 1000)) ); // "30초 전"

alert( formatDate(new Date(new Date - 5 * 60 * 1000)) ); // "5분 전"

// 어제를 나타내는 날짜를 "일.월.연 시:분" 포맷으로 출력
alert( formatDate(new Date(new Date - 86400 * 1000)) );
```

<details>
<summary>답변</summary>
<div markdown="1">

</div>
</details>

<details>
<summary>정답</summary>
<div markdown="1">

</div>
</details>
