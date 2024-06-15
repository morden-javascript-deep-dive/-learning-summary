# Number

## Number 생성자 함수

- 표준 빌트인 객체인 Number 객체는 생성자 함수 객체다.

```js
const numObj = new Number();
console.log(numObj); // [Number: 0]
```

- new 연산자를 사용하지 않고 Number 생성자 함수를 호출하면 Number 생성자 함수를 호출하면 Number 인스턴스가 아닌 숫자를 반환한다.

```js
Number("0"); // 0
Number("-1"); // -1
Number("10.53"); // 10.53

Number(true); // 1
Number(false); // 0
```

## Number 프로퍼티

1. Number.EPSILON

- 1과 1보다 큰 가장 작은 부동 소수점 수 사이의 차이를 나타내는 정적 속성

```js
let a = 0.1 + 0.2;
console.log(a === 0.3); // false

function isEqual(a, b) {
  // a와 b를 뺀 값의 절대값이 Number.EPSILON 보다 작으면 같은 수로 인정한다.
  return Math.abs(a - b) < Number.EPSILON;
}

console.log(isEqual(0.1 + 0.2, 0.3)); // true
```

2. 자바스크립트의 Number 수 표현

```js
// Number.MAX_VALUE 는 자바스크립트에서 표현할 수 있는 가장 큰 양수값이다.
// Number.MAX_VALUE보다 큰 숫자는 Infinity다.
Infinity > Number.MAX_VALUE; // true

// Number.MIN_VALUE 는 자바스크립트에서 표현할 수 있는 가장 작은 양수 값이다.
// Number.MIN_VALUE 보다 작은 숫자는 0이다.
0 < Number.MIN_VALUE; // true

// Number.MAX_SAFE_INTEGER 는 자바스크립트에서 안전하게 표현할 수 있는 가장 큰 정수값이다.
Number.MAX_SAFE_INTEGER; // 9007199254740991

// Number.MIN_SAFE_INTEGER 는 자바스크립트에서 안전하게 표현할 수 있는 가장 작은 정수값이다.
Number.MIN_SAFE_INTEGER; // -9007199254740991

// Number.POSITIVE_INFINITY 는 양의 무한대를 나타내는 숫자값 Infinity와 같다.
Number.POSITIVE_INFINITY; // Infinity

// Number.NEGATIVE_INFINITY 는 음의 무한대를 나타내는 숫자값 -Infinity와 같다.
Number.NEGATIVE_INFINITY; // -Infinity

// Number.NaN 은 숫자가 아님을 나타내는 숫자값이다.
Number.NaN; // NaN
```

## Number 메서드

1. Number.isFinite

- 인수로 전달된 숫자값이 정상적인 유한수, 즉 Infinity 또는 -Infinity가 아닌지 검사하여 그 결과를 불리언 값으로 반환

```js
// 인수가 정상적인 유한수이면 true를 반환한다.
Number.isFinite(0); // true
Number.isFinite(Number.MAX_VALUE); // true
Number.isFinite(Number.MIN_VALUE); // true

// 인수가 무한수이면 false를 반환한다.
Number.isFinite(Infinity); // false
Number.isFinite(-Infinity); // false

// 인수가 NaN이면 항상 false를 반환한다.
Number.isFinite(NaN); // false
```

- Number.isFinite는 전달받은 인수를 숫자로 암묵적 타입 변환하지 않는다.
- 숫자가 아닌 인수가 주어졌을 때 반환값은 언제나 false다.

```js
// Number.isFinite는 인수를 숫자로 암묵적 타입 반환하지 않는다.
Number.isFinite(null);

// isFinite는 인수를 숫자로 암묵적 타입 반환한다. null은 0으로 암묵적 타입 반환된다.
isFinite(null); // true
```

2. Number.isInteger

- 인수로 전달된 숫자값이 정수인지 검사하여 그 결과를 불리언 값으로 반환한다.
- 검사하기 전에 인수를 숫자로 암묵적 타입 변환하지 않는다.

```js
// 인수가 정수이면 true를 반환한다.
Number.isInteger(0); // true
Number.isInteger(123); // true
Number.isInteger(-123); // true

// 0.5는 정수가 아니다.
Number.isInteger(0.5); // false
// "123"을 숫자로 암묵적 타입 변환하지 않는다.
Number.isInteger("123"); // false
// false를 숫자로 암묵적 타입 변환하지 않는다.
Number.isInteger(false); // false
// Infinity/-Infinity 는 정수가 아니다.
Number.isInteger(Infinity); // false
Number.isInteger(-Infinity); // false
```

3. Number.isNaN

- 인수로 전달된 숫자값이 NaN인지 검사하여 그 결과를 불리언 값으로 반환한다.

```js
// 인수가 NaN이면 true를 반환한다.
Number.isNaN(NaN); // true
```

- Number.isNaN 메서드는 빌트인 전역 함수 isNaN과 차이가 있다.
- 빌트인 전역 함수 isNaN은 전달받은 인수를 숫자로 암묵적 타입 변환하여 검사를 수행하지만 Number.isNaN 메서드는 전달받은 인수를 숫자로 암묵적 타입 변환하지 않는다.

```js
// Number.isNaN은 인수를 숫자로 암묵적 타입 변환하지 않는다.
Number.isNaN(undefined); // false

// isNaN은 인수를 숫자로 암묵적 타입 변환한다. undefined는 NaN으로 암묵적 타입 변환된다.
isNaN(undefined); // true
```

4. Number.isSafeInteger

- 인수로 전달된 숫자값이 안전한 정수인지 검사하여 그 결과를 불리언 값으로 반환한다.
- 안전한 정수값은 -(253 - 1) 과 253 - 1 사이의 정수값이며, 검사전에 인수를 숫자로 암묵적 타입 변환하지 않는다.

```js
// 0은 안전한 정수다.
Number.isSafeInteger(0); // true

// 0.5는 정수가 아니다.
Number.isSafeInteger(0.5); // false
// "123"을 숫자로 암묵적 타입 변환하지 않는다.
Number.isSafeInteger("123"); // false
// false를 숫자로 암묵적 타입 변환하지 않는다.
Number.isSafeInteger(false); // false
// Infinity/-Infinity 는 정수가 아니다.
Number.isSafeInteger(Infinity); // false
Number.isSafeInteger(-Infinity); // false
```

5. Number.prototype.toExpoential

- 숫자를 지수 표기법으로 변환하여 문자열로 반환
- 지수 표기법은 매우 크거나 작은 숫자를 표기할 때 주로 사용하며 e(Exponent) 앞에 있는 숫자에 10의 n승을 곱하는 형식으로 수를 나타내는 방식이다.

```js
console.log((77.1234).toExponential()); // 7.71234e+1
console.log((77.1234).toExponential(4)); // 7.7123e+1
console.log((77.1234).toExponential(2)); // 7.71e+1
```

6. Number.prototype.toFixed

- 숫자를 반올림하여 문자열로 반환
- 반올림하는 소수점 이하 자리수를 나타내는 0~20 사이의 정수값을 인수로 전달할 수 있다.
- 인수를 생략하면 기본값 0이 지정된다.

```js
// 소수점 이하 반올림. 인수를 생략하면 기본값 0이 지정된다.
(12345.6789).toFixed(); // 12345
// 소수점 이하 1자릿수 유효, 나머지 반올림
(12345.6789).toFixed(1); // 12345.7
// 소수점 이하 2자릿수 유효, 나머지 반올림
(12345.6789).toFixed(2); // 12345.68
```

7. Number.prototype.toPrecision

- 수로 전달받은 전체 자리수까지 유효하도록 나머지 자리수를 반올림 하여 문자열로 반환
- 인수로 전달받은 전체 자릿수로 표현할 수 없는 경우 지수 표기법으로 결과를 반환

```js
// 전체 자릿수 유효. 인수를 생략하면 기본값 0이 지정된다.
(12345.6789).toPrecision(); // "12345.6789"
// 전체 1자릿수 유효, 나머지 반올림
(12345.6789).toPrecision(1); // "1e+4"
// 전체 2자릿수 유효, 나머지 반올림
(12345.6789).toPrecision(2); // "1.2e+4"
// 전체 6자릿수 유효, 나머지 반올림
(12345.6789).toPrecision(6); // "12345.7"
```

8. Number.prototype.toString

- 숫자를 문자열로 변환하여 반환한다.
- 진법을 나타내는 2~36 사이의 정수값을 인수로 전달할 수 있다.
- 인수를 생략하면 기본값 10진법이 지정된다.

```js
// 인수를 생략하면 10진수 문자열을 반환한다.
(10).toString(); // "10"
// 2진수
(16).toString(2); // "10000"
// 8진수
(16).toString(8); // "20
// 16진수
(16).toString(16); // "10"
```

# Math

## Math 프로퍼티

1. Math.PI

- 원주율 PI값(3.14)을 반환한다.

```js
Math.PI; // 3.141592
```

## Math 메서드

1. Math.abs

2. Math.round

3. Math.ceil

4. Math.floor

5. Math.sqrt

```js
// Math.abs 메서드는 인수로 전달된 숫자의 절대값을 반환한다. 절대값은 반드시 0 또는 양수다.
Math.abs(-1); // 1
Math.abs("1"); // 1
Math.abs(""); // 0
Math.abs([]); // 0
Math.abs(null); // 0

// NaN으로 표기된다.
Math.abs(undefined);
Math.abs({});
Math.abs("string");
Math.abs();

// Math.Round 메서드는 인수로 전달된 숫자의 소수점 이하를 반올림한 정수를 반환한다.
Math.round(1.4); // 1
Math.round(1.6); // 2
Math.round(-1.4); // -1
Math.round(-1.6); // -2
Math.round(1); // 1
Math.round(); // NaN

// Math.ceil 메서드는 인수로 전달된 숫자의 소수점 이하를 올림한 정수를 반환한다.
Math.ceil(1.4); // 2
Math.ceil(1.6); // 2

// Math.floor 메서드는 인수로 전달된 숫자의 소수점 이하를 내림한 정수를 반환한다.
// 인수가 양수인 경우 소수점 이하를 떼어 버린 다음 정수를 반환한다.
Math.floor(1.4); // 1
Math.floor(2.6); // 2
// 인수가 음수인 경우 소수점 이하를 떼어 버린 다음 -1을 곱한 정수를 반환한다.
Math.floor(-1.9); // -2
Maht.floor(-9.1); // 10

// Math.sqrt 메서드는 인수로 전달된 숫자의 제곱근을 반환한다.
Math.sqrt(9); // 3
Math.sqrt(-9); // NaN
Math.sqrt(2); // 1.414213....
Math.sqrt(1); // 1
Math.sqrt(0); // 0
Math.sqrt(); // NaN
```

6. Math.random

- Math.random 메서드는 임의의 난수(랜덤 숫자)를 반환한다.
- Math.random 메서드가 반환한 난수는 0에서 1미만의 실수다. → 0은 포함되지만 1은 포함되지 않는다.

```js
Math.random(); // 0 ~ 1 미만의 랜덤 실수

const random = Math.floor(Math.random() * 10 + 1);
console.log(random); // 1 ~ 10 범위의 정수
```

7. Math.pow

- Math.pow 메서드는 첫 번째 인수를 밑으로, 두 번째 인수를 지수로 거듭제곱한 결과를 반환한다.

```js
Math.pow(2, 8); // 256
Math.pow(2 - 1); // 0.5
Math.pow(2); // NaN

// ES7에서 도입된 지수 연산자를 사용하면 가독성이 더 좋다.
2 ** (2 ** 2); // 16
Math.pow(Math.pow(2, 2), 2); // 16
```

8. Math.max

- Math.max 메서드는 전달받은 인수 중에서 가장 큰 수를 반환한다.
- 인수가 전달되지 않으면 -Infinity를 반환한다.

9. Math.min

- Math.min 메서드는 전달받은 인수 중에서 가장 작은 수를 반환한다.
- 인수가 전달되지 않으면 Infinity를 반환한다.

```js
Math.max(1); // 1
Math.max(1, 2); // 2
Math.max(1, 2, 3); // 3
Math.max(); // -Infinity

Math.min(1); // 1
Math.min(1, 2); // 1
Math.min(1, 2, 3); // 1
Math.min(); // Infinity
```

# Date

## Date 생성자 함수

- Date는 생성자 함수다. Date 생성자 함수로 생성한 Date 객체는 기본적으로 현재 날짜와 시간을 나타내는 정수값을 가진다.
- 현재 날짜와 시간이 아닌 다른 날짜와 시간을 다루고 싶은 경우 Date 생성자 함수에 명시적으로 해당 날짜와 시간 정보를 인수로 지정한다.

1. new Date()

- Date 생성자 함수를 인수 없이 new 연산자와 함께 호출하면 현재 날짜와 시간을 가지는 Date 객체를 반환한다.
- Date 객체는 내부적으로 날짜와 시간을 나타내는 정수값을 갖지만 Date 객체를 호출하면 기본적으로 날짜와 시간 정보를 출력한다.

```js
new Date(); // 2023-08-15T12:50:28.906Z
```

- Date 생성자 함수를 new 연산자 없이 호출하면 Date 객체를 반환하지 않고 날짜와 시간 정보를 나타내는 문자열을 반환한다.

```js
Date(); // Tue Aug 15 2023 21:52:47 GMT+0900 (Korean Standard Time)
```

2. new Date(milliseconds)

- Date 생성자 함수에 숫자 타입의 밀리초를 인수로 전달하면 UTC 기점으로 인수로 전달된 밀리초만큼 경과한 날짜와 시간을 나타내는 Date 객체를 반환한다.

3. new Date(dateString)

- Date 생성자 함수에 날짜와 시간을 나타내는 문자열을 인수로 전달하면 지정된 날짜와 시간을 Date 객체를 반환한다.
- 이때 인수로 전달한 문자열은 Date.parse 메서드에 의해 해석 가능한 형식이어야 한다.

```js
new Date("May 26, 2020 10:00:00"); // 2020-05-26T01:00:00.000Z

new Date("2023/08/15/10:00:00"); // 2023-08-15T01:00:00.000Z
```

4. new Date(year, month, day, hour, minute, second, millisecond)

- Date 생성자 함수에 연, 월, 일, 시, 분, 초, 밀리초를 의미하는 숫자를 인수로 전달하면 지정된 날짜와 시간을 나타내는 Date 객체를 반환한다.
- 이때 연, 월은 반드시 지정해야 한다

```js
new Date(2020, 2); // 2020-02-29T15:00:00.000Z
new Date(2020, 2, 26, 10, 0, 0, 0); // 2020-03-26T01:00:00.000Z

// 가독성이 훨씬 좋다.
new Date("2020/2/26/10:00:00:00"); // 2020-02-26T01:00:00.000Z
```

## Date 메서드

1. Date.now

- 1970년 1월 1일 00:00:00(UTC) 을 기점으로 현재 시간까지 경과한 밀리초를 숫자로 반환한다.

```js
Date.now(); // 1692105562943
```

2. Date.parse

- 1970년 1월 1일 00:00:00(UTC)을 기점으로 인수로 전달된 지정시간(new Date(dateString)) 까지의 밀리초를 숫자로 반환한다.

```js
Date.parse("Jan 2, 1970 00:00:00 UTC"); // 86400000
Date.parse("Jan 2, 1970 00:00:00"); // 54000000
Date.parse("2023/08/16/10:00:00"); // 1692147600000
```

3. Date.UTC

- 1970년 1월 1일 00:00:00(UTC)을 기점으로 인수로 전달된 지정 시간까지의 밀리초를 숫자로 반환한다.
- Date.UTC 메서드는 new Date(year, month[, day…]와 같은 형식의 인수를 사용해야 한다.

```js
Date.UTC(1970, 0, 2); // 86400000
Date.UTC("2020/02/02"); // NaN
```

4. Date 메서드들

```js
// Date.prototype.getFullYear
// Date 객체의 연도를 나타내는 정수를 반환한다.
new Date("2020/07/23").getFullYear(); // 2020

// Date.prototype.setFullYear
// Date 객체의 연도를 나타내는 정수를 설정한다. 연도 이외에 옵션으로 월, 일도 설정할 수 있다.
const today = new Date();

// 년도 지정
today.setFullYear(2000);
today.getFullYear(); // 2000

// 년도/월/일 지정
today.setFullYear(2023, 0, 2);
today.getFullYear(); // 2023

// Date.prototype.getMonth
// Date 객체의 월을 나타내는 0 ~ 11의 정수를 반환한다. 1월은 0, 12월은 11이다.
new Date("2023/08/14").getMonth(); // 6

// Date.prototype.setMonth
// Date 객체의 월을 나타내는 0 ~ 11의 정수를 설정한다.
const today = new Date();

// 월 지정
today.setMonth(0); // 1월
today.getMonth(); // 0

// 월/일 지정
today.setMonth(11, 1); // 12월 1일
today.getMonth(); // 11

// Date.prototype.getDate
// Date 객체의 날짜(1 ~ 31)를 나타내는 정수를 반환한다.
new Date("2023/08/15").getDate(); // 24

// Date.prototype.setDate
// Date 객체의 날짜(1 ~ 31)를 나타내는 정수를 설정한다.
const today = new Date();

// 날짜 지정
today.setDate(1);
today.getDate(); // 1

// Date.prototype.getDay
// Date 객체의 요일(0 ~ 6)을 나타내는 정수를 반환한다.
new Date("2023/08/15").getDay(); // 2

// Date.prototype.getHours
// Date 객체의 시간(0 ~ 23)을 나타내는 정수를 반환한다.
new Date("2023/08/15/12:00").getHours(); // 12

// Date.prototype.setHours
// Date 객체의 시간(0 ~ 23)을 나타내는 정수를 설정한다
// 시간 이외에 옵션으로 분, 초, 밀리초도 설정할 수 있다.
const today = new Date();

// 시간 지정
today.setHours(7);
today.getHours(); // 0

// 시간/분/초/밀리초 지정
today.setHours(0, 0, 0, 0); // 00:00:00:00
today.getHours();

// Date.prototype.getMinutes
// Date 객체의 분(0 ~ 59)을 나타내는 정수를 반환한다.
new Date("2023/08/15/12:20").getMinutes(); // 20

// Date.prototype.setMinutes
// Date 객체에 분(0 ~ 59)을 나타내는 정수를 설정한다.
// 분 이외에 옵션으로 초, 밀리초도 설정할 수 있다.
const today = new Date();

// 시간 지정
today.setMinutes(5);
today.getMinutes(); // 5

// 시간/분/초/밀리초 지정
today.setMinutes(5, 10, 0); // HH:05:10:00
today.getMinutes(); // 5

// Date.prototype.getSeconds
// Date 객체의 초(0 ~ 59)를 나타내는 정수를 반환한다.
new Date("2023/08/15/12:20:10").getSeconds(); // 10

// Date.prototype.setSeconds
// Date 객체의 초(0 ~ 59)를 나타내는 정수를 설정한다.
const today = new Date();

// 시간 지정
today.setSeconds(5);
today.getSeconds(); // 5

// 시간/분/초/밀리초 지정
today.setSeconds(5, 10); // HH:MM:05:10
today.getSeconds(); // 5

// Date.prototype.getMilliseconds
// Date 객체의 밀리초(0 ~ 999)를 나타내는 정수를 반환한다.
new Date("2023/08/15/12:20:10:150").getMilliseconds(); // 150
```

5. Date Time, Timezone 메서드들

```js
// Date.prototype.getTime
// 1970년 1월 1일 00:00:00(UTC)를 기점으로 Date 객체의 시간까지 경과된 밀리초를 반환한다.
new Date("2023/08/15/12:30").getTime(); // 1692070200000

// Date.prototype.setTime
// 1970년 1월 1일 00:00:00(UTC)를 기점으로 Date 객체의 시간까지 경과된 밀리초를 설정한다.
const today = new Date();

today.setTime(86400000); // 86400000은 1day를 의미

// Date.prototype.getTimezoneOffset
// UTC와 Date 객체에 지정된 locale 시간과의 차이를 분 단위로 반환한다.
// KST는 UTC에 9시간을 더한 시간이다.
const today = new Date();

today.getTimezoneOffset() / 60; // -9

// Date.prototype.toDateString
// 사람이 읽을 수 있는 문자열로 Date 객체의 날짜를 반환한다.
const today = new Date("2023/08/15/12:30");

today.toString(); // Tue Aug 15 2023 12:30:00 GMT+0900 (Korean Standard Time)
today.toDateString(); // Tue Aug 15 2023

// Date.prototype.toTimeString
// 사람이 읽을 수 있는 문자열로 Date 객체의 날짜를 반환한다.
const today = new Date("2023/08/15/12:30");

today.toTimeString(); // 12:30:00 GMT+0900 (Korean Standard Time)

// Date.prototype.toISOString
// ISO 8601 형식으로 Date 객체의 날짜와 시간을 표현한 문자열을 반환한다.
const today = new Date("2023/08/15/12:30");

today.toISOString(); // 2023-08-15T03:30:00.000Z
today.toISOString().slice(0, 10); // 2023-08-15
today.toISOString().slice(0, 10).replace(/-/g, ""); // 20230815

// Date.prototype.toLocaleString
// 인수로 전달한 로캘을 기준으로 Date 객체의 날짜와 시간을 문자열을 반환한다.
// 인수를 생략한 경우 브라우저가 동작 중인 시스템의 로캘을 적용한다.
const today = new Date("2023/08/15/12:30");

today.toLocaleString(); // 8/15/2023, 12:30:00 PM
today.toLocaleString("ko-KR"); // 2023. 8. 15. 오후 12:30:00
today.toLocaleString("en-US"); // 8/15/2023, 12:30:00 PM
today.toLocaleString("ja-JP"); // 2023/8/15 12:30:00

// Date.prototype.toLocaleTimeString
// 인수로 전달한 로캘을 기준으로 Date 객체의 시간을 문자열을 반환한다.
// 인수를 생략한 경우 브라우저가 동작 중인 시스템의 로캘을 적용한다.
const today = new Date("2023/08/15/12:30");

today.toLocaleTimeString()); // 12:30:00 PM
today.toLocaleTimeString("ko-KR"); // 오후 12:30:00
today.toLocaleTimeString("en-US"); // 12:30:00 PM
today.toLocaleTimeString("ja-JP"); // 12:30:00
```
