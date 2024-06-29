# RegExp

## 정규 표현식이란?

- 일정한 패턴을 가진 문자열의 집합을 표현하기 위해 사용하는 형식 언어
- 문자열을 대상으로 패턴 매칭 기능을 제공한다.

```js
// 사용자로부터 입력받은 휴대폰 전화번호
const tel = "010-1234-567팔";

// 정규 표현식 리터럴로 휴대폰 전화번호 패턴 정의
const regExp = /^\d{3}-\d{4}-\d{4}$/;

// 패턴 매칭 확인
regExp.test(tel); // false
```

## 정규 표현식의 생성

- 정규 표현식 객체를 생성하기 위해서는 정규 표현식 리터럴과 RegExp 생성자 함수를 사용할 수 있다.
- 일반적인 방식은 정규 표현식 리터럴을 사용하는 것이다.

```js
const target = "Is this all there is?";

// 패턴: is
// 플래그: i => 대소문자를 구별하지 않고 검색한다.
const regexp = /is/i;

// test 메서드는 target 문자열에 대해 정규 표현식 regexp의 패턴을 검색하여
// 매칭 결과를 불리언 값으로 반환한다.
regexp.test(target); // true

// RegExp 생성자 함수를 사용하여 RegExp 객체를 생성할 수도 있다.
const target = "Is this all there is?";

const regexp = new RegExp(/is/i); // ES6

regexp.test(target); // true
```

## RegExp 메서드

1. RegExp.prototype.exec

- 인수로 전달받은 문자열에 대해 정규 표현식의 패턴을 검색하여 매칭 결과를 배열로 반환
- 매칙 결과가 없으면 null을 반환
- 문자열 내의 모든 패턴을 검색하는 g플래그를 지정해도 첫 번째 매칭결과만 반환하므로 주의해야 한다.

```js
const target = "Is this all there is?";
const target2 = "aaaaa";
const regexp = /is/i;

regexp.exec(target); // [ 'Is', index: 0, input: 'Is this all there is?', groups: undefined ]
regexp.exec(target2); // null
```

2. RegExp.prototype.test

- 인수로 전달받은 문자열에 대해 정규 표현식의 패턴을 검색하여 매칭된 결과를 불리언으로 반환한다.

```js
const target = "Is this all there is?";
const target2 = "aaaaa";
const regexp = /is/i;

regexp.test(target); // true
regexp.test(target2); // false
```

3. String.prototype.match

- String 표준 빌트인 객체가 제공하는 match 메서드는 대상 문자열과 인수로 전달받은 정규 표현식과의 매칭 결과를 배열로 반환한다.

```js
const target = "Is this all there is?";
const regexp = /is/;

target.match(regexp); // ['Is', index: 0, input: 'Is this all there is', groups: undefined]
```

- exec 메서더는 문자열 내의 모든 패턴을 검색하는 g플래그를 지정해도 첫 번째 매칭결과를 반환하는 반면 match 메서드는 모든 매칭 결과를 배열로 반환한다.

```js
const target = "Is this all there is?";
const regexp = /is/g;

target.match(regexp); // ["is", "is"]
```

## 플래그

- 플래그
  - i(Ignore case): 대소문자를 구별하지 않고 패턴을 검색한다.
  - g(Global): 대상 문자열 내에서 패턴과 일치하는 모든 문자열을 전역 검색한다.
  - m(Multi line): 문자열의 행이 바뀌더라도 패턴 검색을 계속한다.

```js
const target = "Is this all there is?";

// target 문자열에서 is 문자열을 대소문자를 구별하여 한 번 만 검색한다.
target.match(/is/);
// [ 'is', index: 5, input: , groups: undefined ]?'

// target 문자열에서 is 문자열을 대소문자를 구별하지 않고 한 번 만 검색한다.
target.match(/is/i);
// [ 'Is', index: 0, input: 'Is this all there is?', groups: undefined ]

// target 문자열에서 is 문자열을 대소문자를 구별하여 전역 검색한다.
target.match(/is/g); // [ 'is', 'is' ]

// target 문자열에서 is 문자열을 대소문자를 구별하지 않고 전역 검색한다.
target.match(/is/gi); // [ 'Is', 'is', 'is' ]
```

## 패턴

1. 문자열 검색

- 정규 표현식의 패턴에 문자열을 지정하면 검색 대상 문자열에서 패턴으로 지정한 문자 또는 문자열을 검색한다.

```js
const target = "Is this all there is?";

// target 문자열에서 is 문자열을 대소문자를 구별하여 한 번 만 검색한다.
target.match(/is/);
// [ 'is', index: 5, input: , groups: undefined ]?'

// target 문자열에서 is 문자열을 대소문자를 구별하지 않고 한 번 만 검색한다.
target.match(/is/i);
// [ 'Is', index: 0, input: 'Is this all there is?', groups: undefined ]

// target 문자열에서 is 문자열을 대소문자를 구별하여 전역 검색한다.
target.match(/is/g); // [ 'is', 'is' ]

// target 문자열에서 is 문자열을 대소문자를 구별하지 않고 전역 검색한다.
target.match(/is/gi); // [ 'Is', 'is', 'is' ]
```

2. 임의의 문자열 검색

- .은 임의의 문자 한개를 의미한다.

```js
const target = "Is this all there is?";

// 임의의 3자리 문자열을 대소문자를 구별하여 전역 검색한다.
const regExp = /.../g;

// ['Is ', 'thi', 's a', 'll ', 'the', 're ', 'is?']
console.log(target.match(regExp));
```

3. 반복 검색

- {m, n}은 앞서 패턴이 최소 m번, 최대 n번 반복되는 문자열을 의미한다.
- 콤마 뒤에 공백이 있으면 정상 동작하지 않으므로 주의해야한다.

```js
const target = "A AA B BB Aa Bb AAA";

// A가 최소 1번, 최대 2번 반복되는 문자열을 전역 검색한다.
let regExp = /A{1,2}/g;

target.match(regExp); // [ 'A', 'AA', 'A', 'AA', 'A' ]

// {n}은 앞선 패턴이 n번 반복되는 문자열을 의미한다.
// -> {n} === {n, n}
regExp = /A{2}/g;

target.match(regExp); // ['AA', 'AA']

// {n,}은 앞선 패턴이 최소 n번 이상 반복되는 문자열을 의미한다.
regExp = /A{2,}/g;

target.match(regExp); // ['AA', 'AAA']

// +는 앞선 패턴이 최소 한번 이상 반복되는 문자열을 의미한다. +는 {1,}
// 'A'가 최소 한 번 이상 반복되는 문자열('A', 'AA', 'AAA',....)을 전역 검색한다.
regExp = /A+/g;

target.match(regExp); // [ 'A', 'AA', 'A', 'AAA' ]

// ?는 앞선 패턴이 최대 한 번(0번 이상) 이상 반복되는 문자열을 의미한다.
// ?는 {0,1}과 같다.
// 'colo' 다음 'u'가 최대 한 번(0번 포함) 이상 반복되고 'r'이 이어지는
// 문자열 'color', 'colour'를 전역 검색한다.
const target = "color colour";

const regExp = /colou?r/g;
target.match(regExp); // ['color', 'colour']
```

4. OR 검색

```js
const target = "A AA B BB Aa Bb";

// 'A' 또는 'B'를 전역 검색한다.
let regExp = /A|B/g;

target.match(regExp); // ['A', 'A', 'A', 'B', 'B', 'B', 'A', 'B']

// 분해되지 않은 단어 레벨로 검색하기 위해서는 +를 함께 사용한다.
// 'A', 'AA', 'AAA', 'B', 'BB'...
regExp = /A+|B+/g;

target.match(regExp); // [ 'A', 'AA', 'B', 'BB', 'A', 'B' ]

// []내의 문자는 or로 동작한다. 그 뒤에 +를 사용하면 앞선 패턴을 한 번 이상 반복한다.
regExp = /[AB]+/g;

target.match(regExp); // [ 'A', 'AA', 'B', 'BB', 'A', 'B' ]

// 범위를 지정하려면 []내에 -를 사용한다.
const target = "A AA BB ZZ Aa Bb";

// 'A' ~ 'Z'가 한 번 이상 반복되는 문자열을 전역 검색한다.
regExp = /[A-Z]+/g;

target.match(regExp); // [ 'A', 'AA', 'BB', 'ZZ', 'A', 'B' ]

// 대소문자를 구별하지 않고 알파벳을 검색하는 방법은 다음과 같다.
const target = "AA BB Aa Bb 12";

// 'A' ~ 'Z' 또는 'a' ~ 'z'가 한 번 이상 반복되는 문자열을 전역 검색한다.
regExp = /[A-Za-z]+/g;

target.match(regExp); // [ 'AA', 'BB', 'Aa', 'Bb' ]
```

- 숫자를 검색 하는 방법

```js
const target = "AA BB 12,345";

// '0' ~ '9'가 한 번 이상 반복되는 문자열을 전역 검색한다.
let regExp = /[0-9]+/g;

target.match(regExp); // ['12', '345']

// 쉼표를 패턴에 포함한다면
regExp = /[0-9,]+/g;

target.match(regExp); // ['12,345']

// \d는 숫자를 의미한다.
let regExp = /[\d,]+/g;

target.match(regExp); // ['12,345']

// \D는 숫자가 아닌 문자를 의미한다.
// 숫자가 아닌 문자 또는 ','가 한 번 이상 반복되는 문자열을 전역 검색한다.
regExp = /[\D,]+/g;

target.match(regExp); // [ 'AA BB ', ',' ]

// \w는 알파벳, 숫자, 언더스코어를 의미한다.
// \w는 [A-Za-z0-9_]와 같다.
const target = "Aa Bb 12,345 _$%&";

// 알파벳, 숫자, 언더스코어, ','가 한 번 이상 반복되는 문자열을 전역 검색한다.
let regExp = /[\w,]+/g;

target.match(regExp); // [ 'Aa', 'Bb', '12,345', '_' ]

// 알파벳, 숫자, 언더스코어가 아닌 문자 또는 ','가 한 번 이상 반복되는 문자열을 전역 검색한다.
regExp = /[\W,]+/g;

target.match(regExp); // [ ' ', ' ', ',', ' ', '$%&' ]
```

5. NOT 검색

- [...] 내의 ^은 not의 의미를 갖는다.

```js
const target = "AA BB 12 Aa Bb";

// 숫자를 제외한 문자열을 전역 검색한다.
const regExp = /[^\d]+/g;

target.match(regExp); // [ 'AA BB ', ' Aa Bb' ]
```

6. 시작 위치로 검색

- [...] 밖의 ^은 문자열의 시작을 의미

```js
const target = "https://naver.com";

// 'https'로 시작되는지 검사한다.
const regExp = /^https/g;

regExp.test(target); // true
```

7. 마지막 위치로 검색

- $는 문자열의 마지막을 의미

```js
const target = "https://naver.com";

// 'com'로 시작되는지 검사한다.
const regExp = /com$/g;

regExp.test(target); // true
```

## 자주 사용하는 정규 표현식

1. 특정 단어로 시작하는지 검사

```js
const url = "https://naver.com";

// 'http://' 또는 'https://'로 시작하는지 검사한다.
const regExpHttp = /^https:\/\//;

regExpHttp.test(url); // true
```

2. 특정 단어로 끝나는지 검사

```js
const fileName = "index.html";

const regExp = /html$/;

regExp.test(fileName); // true
```

3. 숫자로만 이루어진 문자열인지 검사

4. 하나 이상의 공백으로 시작하는지 검사

5. 아이디로 사용 가능한지 검사

6. 메일 주소 형식에 맞는지 검사

7. 핸드폰 번호 형식에 맞는지 검사

8. 특수 문자 포함 여부 검사

# String

## String 생성자 함수

- 표준 빌트인 객체인 String 객체는 생성자 함수 객체다.
- String 래퍼 객체는 배열과 마찬가지로 length 프로퍼티와 인덱스를 나타내는 숫자 형식의 문자열을 프로퍼티 키로, 각 문자를 프로퍼티 값으로 갖는 유사 배열 객체이면서 이터러블이다.

```js
const strObj = new String("Park");
console.log(strObj); // [String: 'Park']

console.log(strObj[0]); // P
```

- 문자열은 원시 값이므로 변경할 수 없다. 이때 에러는 발생하지 않는다.

```js
const strObj = new String("Park");
strObj[0] = "K";
console.log(strObj); // [String: 'Park']
```

- String 생성자 함수의 인수로 문자열이 아닌 값을 전달하면 인수를 문자열로 강제 변환한 후, StringData 내부 슬롯에 변환된 문자열을 할당한 String 객체 래퍼를 생성한다.

```js
let strObj = new String(123);
console.log(strObj); // [String: '123']

strObj = new String(null);
console.log(strObj); // [String: 'null']
```

- new 연산자를 사용하지 않고 String 생성자 함수를 호출하면 String 인스턴스가 아닌 문자열을 반환한다.
- 명시적으로 타입을 변환하기도 한다.

```js
// 숫자 타입 -> 문자열 타입
String(1); // "1"
String(NaN); // "NaN"
String(Infinity); // "Infinity"

// 불리언 타입 -> 문자열 타입
String(true); // "true"
String(false); // "false
```

## length 프로퍼티

- length 프로퍼티는 문자열의 문자 개수를 반환

```js
"Hello".length; // 5
"안녕하세요!".length; // 6
```

## String 메서드

- String 객체에는 원본 String 래퍼 객체를 직접 변경하는 메서드는 존재하지 않는다. 즉, String 객체의 메서드는 언제나 새로운 문자열을 반환한다.
- 문자열은 변경 불가능한 원시 값이기 때문에 String 래퍼 객체도 읽기 전용 객체로 제공

1. indexOf

- indexOf 메서드는 대상 문자열에서 인수로 전달받은 문자열을 검색하여 첫 번째 인덱스를 반환
- 검색에 실패하면 -1을 반환

```js
const str = "Hello World";

// 문자열 str에서 'l'을 검색하여 첫 번째 인덱스를 반환
str.indexOf("l"); // 2

// 문자열 str에서 'or'을 검색하여 첫 번째 인덱스를 반환
str.indexOf("or"); // 7

// 문자열 str에서 'x'을 검색하여 첫 번째 인덱스를 반환. 검색을 실패하여 -1 반환
str.indexOf("x"); // -1

// indexOf 메서드의 2번째 인수로 검색을 시작할 인덱스를 전달할 수 있다.

// 문자열 str에서 인덱스 3부터 'l'을 검색하여 첫 번째 인덱스를 반환
str.indexOf("l", 3); // 3
```

2. search

- 대상 문자열에서 인수로 전달받은 정규 표현식과 매치하는 문자열을 검색하여 일치하는 문자열의 인덱스를 반환
- 검색에 실패하면 -1을 반환

```js
const str = "Hello World";

// 문자열 str에서 정규 표현식과 매치하는 문자열을 검색하여 일치하는 문자열의 인덱스를 반환
str.search(/o/); // 4
str.search(/x/); // -1
```

3. includes

- 대상 문자열에 인수로 전달받은 문자열이 포함되어 있는지 확인하여 그 결과를 true 또는 false로 반환

```js
const str = "Hello World";

str.includes("Hello"); // true
str.includes(" "); // true
str.includes("x"); // false
str.includes(); // false
```

- includes 메서드에 2번째 인수로 검색을 시작할 인덱스를 전달할 수 있다.

```js
const str = "Hello World";

str.includes("l", 3); // true
str.includes("H", 3); // false
```

4. startsWith

- 대상 문자열이 인수로 전달받은 문자열로 시작하는지 확인하여 그 결과를 true or false로 반환

```js
const str = "Hello World";

str.startsWith("He"); // true
str.startsWith("x"); // false

// startsWith 메서드의 2번째 인수로 검색을 시작할 인덱스를 전달할 수 있다.
str.startsWith(" ", 5); // true
```

5. endsWidth

- 대상 문자열이 인수로 전달받은 문자열로 끝나는지 확인하여 그 결과를 true or false로 반환

```js
const str = "Hello World";

str.endsWith("ld"); // true
str.endsWith("x"); // false

// endsWith 메서드의 2번째 인수로 검색할 문자열의 길이를 전달할 수 있다.
// 문자열 str의 처음부터 5자리까지("Hello")가 "lo"로 끝나는지 확인
str.startsWith("lo", 5); // true
```

6. charAt

- 대상 문자열에서 인수로 전달받은 인덱스에 위치한 문자를 검색하여 반환

```js
const str = "Hello";

for (let i = 0; i < str.length; i++) {
  console.log(str.charAt(i)); // H e l l o
}

// 인덱스가 문자열의 범위를 벗어난 경우 빈 문자열을 반환한다.
str.charAt(5); // ""
```

7. subString

- 대상 문자열에서 첫 번째 인수로 전달받은 인덱스에 위치하는 문자부터 두 번째 인수로 전달받은 인덱스에 위치하는 문자의 바로 이전 문자까지의 부분 문자열을 반환

```js
const str = "Hello World";

// 인덱스 1부터 인덱스 4 이전까지의 부분 문자열을 반환한다.
str.substring(1, 4); // ell

// substring 메서드의 두번째 인수는 생략할 수 있다.
// 이때, 첫 번째 인수로 전달한 인덱스에 위치하는 문자부터 마지막 문자까지 부분 문자열을 반환한다.
str.substring(1); // ello World
```

- 두 번째 인수는 첫 번째 인수보다 큰 정수이어야 정상이지만 다음과 같이 인수를 전달하여도 정상 동작한다.

```js
const str = "Hello World";

// 첫 번째 인수 > 두 번째 인수인 경우 두 인수는 교환된다.
str.substring(4, 1); // ell

// 인수 < 0 또는 NaN인 경우 0으로 취급된다.
str.substring(-2); // Hello World

// 인수 > 문자열의 길이인 경우 인수는 문자열의 길이로 취급된다.
str.substring(1, 20); // ello World
str.substring(20); // ""
```

8. slice

- slice 메서드는 substring 메서드와 동일하게 동작한다.
- 단, slice 메서드에서는 음수인 인수를 전달할 수 있다. 음수인 인수를 전달하면 대상 문자열의 가장 뒤에서부터 시작하여 문자열을 잘라내어 반환

```js
const str = "Hello World";

str.substring(0, 5); // Hello
str.slice(0, 5); // Hello

str.substring(2); // llo World
str.slice(2); // llo World

// 음수인 경우 0으로 취급된다.
str.substring(-5); // Hello World
// 음수인 경우 뒤에서 5자리를 잘라낸다.
str.slice(-5); // World
```

9. toUpperCase

```js
const str1 = "hello world";

str1.toUpperCase(); // HELLO WORLD
```

10. toLowerCase

```js
const str2 = "HELLO WORLD";

str2.toLowerCase(); // hello world
```

11. trim

- 대상 문자열 앞뒤에 공백 문자가 있을 경우 제거한 문자열을 반환

```js
const str = "      foo    ";

str.trim(); // foo
```

- String.prototype.trimStart, String.prototype.trimEnd를 사용하면 대상 문자열 앞 또는 뒤에 공백 문자가 있을 경우 이를 제거한 문자열을 반환

```js
const str = "      foo    ";

str.trimStart(); // "foo    "
str.trimEnd(); // "      foo"
```

- String.prototype.replace 메서드에 정규표현식을 인수로 전달하여 공백 문자를 제거할 수도 있다.

```js
const str = "      foo    ";

str.replace(/\s/g, ""); // "foo"
str.replace(/^\s+/g, ""); // "foo    "
str.replace(/\s+$/g, ""); // "      foo"
```

12. repeat

- 대상 문자열을 인수로 전달받은 정수만큼 반복해 연결한 새로운 문자열을 반환
- 인수로 전달받은 정수가 0이면 빈 문자열을 반환하고, 음수이면 RangeError를 발생
- 인수를 생략하면 기본값 0이 설정된다.

```js
const str = "abc";

str.repeat(); // ""
str.repeat(0); // ""
str.repeat(1); // "abc"
str.repeat(2); // "abcabc"
str.repeat(2.5); // "abcabc" => (2.5 -> 2)
str.repeat(-1); // RangeError
```

13. replace

- 대상 문자열에서 첫 번째 인수로 전달받은 문자열 또는 정규 표현식을 검색하여 두 번째 인수로 전달한 문자열로 치환한 문자열을 반환

```js
const str = "Hello world";

// str에서 첫 번째 인수 'world'를 검색하여 두 번째 인수 'Park'으로 치환한다.
str.replace("world", "Park"); // Hello Park

// 검색된 문자열이 여럿 존재할 경우 첫 번째로 검색된 문자열만 치환한다.
const str = "Hello world world";
str.replace("world", "Park"); // Hello Park world

// 특수한 교체 패턴을 사용할 수 있다.
const str = "Hello world";

// 특수한 교체 패턴을 사용할 수 있다. ($& -> 검색된 문자열
str.replace("world", "<string>$&</string>");

// replace 메서드의 첫 번째 인수로 정규 표현식을 전달할 수 있다.
const str = "Hello hello";

// 'Hello"를 대소문자를 구별하지 않고 전역 검색
str.replace(/hello/gi, "Park"); // Park Park
```

14. split

- 상 문자열에서 첫 번째 인수로 전달한 문자열 또는 정규 표현식을 검색하여 문자열을 구분한 후 분리된 각 문자열로 이루어진 배열을 반환
- 인수로 빈 문자열을 전달하면 각 문자를 모두 분리하고, 인수를 생략하면 대상 문자열 전체를 단일 요소로 하는 배열을 반환

```js
const str = "How are you doing?";

// 공백으로 구분
str.split(" "); // [ 'How', 'are', 'you', 'doing?' ]

// \s 는 여러 가지 공백 문자(스페이스, 탭 등)를 의미
str.split(/\s/); // [ 'How', 'are', 'you', 'doing?' ]

// 인수로 빈 문자열을 전달하면 각 문자를 모두 분리
str.split("");
/*
[
  'H', 'o', 'w', ' ', 'a',
  'r', 'e', ' ', 'y', 'o',
  'u', ' ', 'd', 'o', 'i',
  'n', 'g', '?'
]
*/

// 인수를 생략하면 대상 문자열 전체를 단일 요소로 하는 배열 반환
str.split(); // [ 'How are you doing?' ]

// 두 번째 인수로 배열의 길이를 지정
str.split(" ", 2); // [ 'How', 'are' ]

// 문자열 역순 만들기(Array.prototype.reverse, Array.prototype.join)
function reverseString(str) {
  return str.split("").reverse().join("");
}

reverseString(str); // ?gniod uoy era woH
```

# Symbol

## 심벌이란?

- 심벌은 ES6에서 도입된 7번째 데이터 타입으로 변경 불가능한 원시 타입의 값이다.
- 심벌 값은 다른 값과 중복되지 않는 유일무이한 값으로 주로 이름의 충돌 위험이 없는 유일한 프로퍼티 키를 만들기 위해 사용한다.

## 심벌 값의 생성

1. Symbol 함수

- 심벌 값은 Symbol 함수를 호출하여 생성
- 이때 생성한 심벌 값은 외부로 노출되지 않아 확인할 수 없으며, 다른 값과 절대 중복되지 않는 유일무이한 값이다.

```js
// Symbol 함수를 호출하여 유일무이한 심벌 값을 생성한다.
const mySymbol = Symbol();
console.log(typeof mySymbol); // symbol

// 심벌 값은 외부로 노출되지 않아 확인할 수 없다.
console.log(mySymbol); // Symbol()
```

- Symbol 함수는 String, Number, Boolean 생성자 함수와 달리 new 연산자와 함께 호출하지 않는다.

```js
new Symbol(); // TypeError
```

- Symbol 함수에는 선택적으로 문자열을 인수로 전달할 수 있다. 이 문자열은 생성된 심벌 값에 대한 설명으로 디버깅 용도로만 사용되며, 심벌 값 생성에 어떠한 영향을 주지 않는다.
- 심벌 값에 대한 설명이 같더라도 생성된 심벌 값은 유일무이한 값이다.

```js
// 심벌 값에 대한 설명이 같더라도 유일무이한 심벌 값을 생성한다.
const mySymbol1 = Symbol("mySymbol");
const mySymbol2 = Symbol("mySymbol");

console.log(mySymbol1 === mySymbol2); // false
```

- 심벌 값도 문자열, 숫자, 불리언과 같이 객체처럼 접근하면 암묵적으로 래퍼 객체를 생성한다.

```js
const mySymbol = Symbol("mySymbol!!");

// 심벌도 래퍼 객체를 생성한다.
console.log(mySymbol.description); // mySymbol!!
console.log(mySymbol.toString()); // Symbol(mySymbol!!)
```

- 심벌 값은 암묵적으로 문자열이나 숫자 타입으로 변환되지 않는다.

```js
const mySymbol = Symbol();

// 심벌 값은 암묵적으로 문자열이나 숫자 타입으로 변환되지 않는다.
console.log(mySymbol + ""); // TypeError
console.log(+mySymbol); // TypeError
```

```js
const mySymbol = Symbol();

// 불리언 타입으로는 암묵적으로 타입 변환된다.
console.log(!!mySymbol); // true

// if 문 등에서 존재 확인이 가능하다.
if (mySymbol) {
  console.log("mySymbol is not empty.");
}
// mySymbol is not empty.
```

2. Symbol.for / Symbol.keyFor 메서드

- Symbol.for 메서드는 인수로 전달받은 문자열을 키로 사용하여 키와 심벌 값의 쌍들이 저장되어 있는 전역 심벌 레지스트리에서 해당 키와 일치하는 심벌 값을 검색한다.

  - 검색에 성공하면 새로운 심벌 값을 생성하지 않고 검색된 심벌 값을 반환한다.
  - 검색에 실패하면 새로운 심벌 값을 생성하여 Symbol.for 메서드의 인수로 전달된 키로 전역 심벌 레지스트리에 저장한 후, 생성된 심벌 값을 반환한다.

  ```js
  // 전역 심벌 레지스트리에 mySymbol 이라는 키로 저장된 심벌 값이 없으면 새로운 심벌 값을 생성
  const s1 = Symbol.for("mySymbol");
  // 전역 심벌 레지스트리에 mySymbol 이라는 키로 저장된 심벌 값이 있으면 해당 심벌 값을 반환
  const s2 = Symbol.for("mySymbol");

  console.log(s1 === s2); // true
  ```

- Symbol.for 메서드를 사용하면 애플리케이션 전역에서 중복되지 않는 유일무이한 상수인 심벌 값을 단 하나만 생성하여 전역 심벌 레지스트리를 통해 공유할 수 있다.
- Symbol.keyFor 메서드를 사용하면 전역 심벌 레지스트리에 저장된 심벌 값의 키를 추출할 수 있다.

```js
// 전역 심벌 레지스트리에 mySymbol 이라는 키로 저장된 심벌 값이 없으면 새로운 심벌 값을 생성
const s1 = Symbol.for("mySymbol");
// 전역 심벌 레지스트리에 저장된 심벌 값의 키를 추출
Symbol.keyFor(s1); // mySymbol

// Symbol 함수를 호출하여 생성한 심벌 값은 전역 심벌 레지스트리에 등록되어 관리되지 않는다.
const s2 = Symbol("foo");
Symbol.keyFor(s2); // undefined
```

## 심벌과 상수

- 4방향, 즉 위, 아래, 왼쪽, 아래쪽을 나타내는 상수를 심벌로 정의한다면

```js
const Direction = {
  UP: Symbol("up"),
  DOWN: Symbol("down"),
  LEFT: Symbol("left"),
  RIGHT: Symbol("right"),
};

const myDirection = Direction.UP;

if (myDirection === Direction.UP) {
  console.log("위로 갑니다."); // 위로 갑니다.
}
```

## 심벌과 프로퍼티 키

- 객체의 프로퍼티 키는 빈 문자열을 포함하는 모든 문자열 또는 심벌 값으로 만들 수 있으며, 동적으로 생성할 수도 있다.

```js
const obj = {
  // 심벌 값으로 프로퍼티 키를 생성
  [Symbol.for("mySymbol")]: 1,
};

console.log(obj[Symbol.for("mySymbol")]); // 1
```

## 심벌과 프로퍼티 은닉

- 심벌 값을 프로퍼티 키로 생성한 프로퍼티는 for…in , Object.keys , Object.getOwnPropertyNames 메서드로 찾을 수 없다.

```js
const obj = {
  // 심벌 값으로 프로퍼티 키를 생성
  [Symbol("mySymbol")]: 1,
};

for (const key in obj) {
  console.log(key); // 아무것도 출력되지 않는다.
}

console.log(Object.keys(obj)); // []
console.log(Object.getOwnPropertyNames(obj)); // []
```

- ES6에서 도입된 Object.getOwnPropertySymbols 메서드를 사용하면 심벌 값을 프로퍼티 키로 사용하여 생성한 프로퍼티를 찾을 수 있다.

```js
const obj = {
  // 심벌 값으로 프로퍼티 키를 생성
  [Symbol("mySymbol")]: 1,
};

console.log(Object.getOwnPropertySymbols(obj)); // [Symbol(mySymbol)]

// getOwnPropertySymbols 메서드로 심벌 값도 찾을 수 있다.
const symbolKey = Object.getOwnPropertySymbols(obj)[0];
console.log(obj[symbolKey]); // 1
```

## 심벌과 표준 빌트인 객체 확장

- 표준 빌트인 객체는 읽기 전용으로 사용하는 것이 좋다.
- 그 이유는 개발자가 직접 추가한 메서드와 미래에 표준 사용으로 추가될 메서드의 이름이 중복될 수 있기 때문이다.

```js
// 표준 빌트인 객체를 확장하는 것은 권장하지 않는다.
Array.prototype.sum = function () {
  return this.reduce((acc, cur) => acc + cur, 0);
}[(1, 2)].sum(); // 3
```

- 하지만 중복될 가능성이 없는 심벌 값으로 프로퍼티 키를 생성하여 표준 빌트인 객체를 확장하면 표준 빌트인 객체의 기존 프로퍼티 키와 충돌하지 않는다.
- 더불어 표준 사양의 버전이 올라감에 따라 추가될지 모르는 어떤 프로퍼티 키와도 충돌할 위험이 없어 안전하게 표준 빌트인 객체를 확장할 수 있다.

```js
Array.prototype[Symbol.for("sum")] = function () {
  return this.reduce((acc, cur) => acc + cur, 0);
}[(1, 2)][Symbol.for("sum")](); // 3
```

## Well-known Symbol

- 자바스크립트가 기본 제공하는 빌트인 심벌 값이 있다.
- 브라우저 콘솔에서 Symbol 함수를 참조하면 다음과 같다.

![Well-known Symbol](https://github.com/kses1010/sunny-devlog/assets/49144662/de509a12-0b9e-46eb-b9de-08fbff58c562)

- 자바스크립트가 기본 제공하는 심벌 값을 ECMAScript 사양에서는 Well-known Symbol 이라 부른다.
- Well-known Symbol은 자바스크립트 엔진의 내부 알고리즘에 사용된다.
- `심벌은 중복되지 않는 상수 값을 생성하는 것은 물론 기존에 작성된 코드에 영향을 주지 않고 새로운 프로퍼티를 추가하기 위해, 즉 하위 호환성을 보장하기 위해 도입되었다`

# 이터러블

## 이터레이션 프로토콜

- ES6에서 도입된 이터레이션 프로토콜은 순회 가능한 데이터 컬렉션을 만들기 위해 ECMAScript 사양에 정의하여 미리 약속한 규칙이다.
- 이터레이션 프로토콜에는 이터러블 프로토콜과 이터레이터 프로토콜이 있다.

1. 이터러블

- 이터러블 프로토콜은 준수한 객체를 이터러블이라 한다.
- 이터러블은 Symbol.iterator를 프로퍼티 키로 사용한 메서드를 직접 구현하거나 프로토타입 체인을 통해 상속받은 객체를 말한다.

```js
const isIterable = (v) =>
  v !== null && typeof v[Symbol.iterator] === "function";

// 배열, 문자열, Map, Set 등은 이터러블이다.
isIterable([]); // true
isIterable(""); // true
isIterable(new Map()); // true
isIterable(new Set()); // true
isIterable({}); // false
```

- 이터러블은 `for...of` 문으로 순회할 수 있으며, 스프레드 문법과 배열 디스트럭처링 할당의 대상으로 사용할 수 있다.

```js
const array = [1, 2, 3];

// 배열은 Array.prototype 의 Symbol.iterator 메서드를 상속받은 이터러블이다.
console.log(Symbol.iterator in array); // true

// 이터러블인 배열은 for...of 문으로 순회 가능하다.
for (const item of array) {
  console.log(item);
}

// 이터러블인 배열은 스프레드 문법의 대상으로 사용할 수 있다.
console.log([...array]); // [1, 2, 3]

// 이터러블인 배열은 배열 디스트럭처링 할당의 대상으로 사용할 수 있다.
const [a, ...rest] = array;
console.log(a, rest); // 1, [2, 3]
```

- Symbol.iterator 메서드를 직접 구현하지 않거나 상속받지 않은 일반 객체는 이터러블 프로토콜을 준수한 이터러블이 아니다.
- 일반 객체는 `for...of` 문으로 순회할 수 없으며 스프레드 문법과 배열 디스트럭처링 할당의 대상으로 사용할 수 없다.

```js
const obj = { a: 1, b: 2 };

// 일반 객체는 Symbol.iterator 메서드를 구현하거나 상속받지 않는다.
// 일반 객체는 이터러블 프로토콜을 준수한 이터러블이 아니다.
console.log(Symbol.iterator in obj); // false

// 이터러블이 아닌 일반 객체는 for...of 문으로 순회할 수 없다.
for (const item of obj) {
  // TypeError
  console.log(item);
}

// 이터러블이 아닌 일반 객체는 배열 디스트럭처링 할당의 대상으로 사용할 수 없다.
const [a, b] = obj; // TypeError
```

- 스프레드 프로퍼티 제안은 일반 객체에 스프레드 문법의 사용을 허용한다.

```jsx
const obj = { a: 1, b: 2 };

// 스프레드 프로퍼티 제안은 객체 리터럴 내부에서 스프레드 문법의 사용을 허용한다.
console.log({ ...obj }); // { a: 1, b: 2 }
```

2. 이터레이터

- 이터러블의 Symbol.iterator 메서드가 반환한 이터레이터는 next 메서드를 갖는다.

```js
// 배열은 이터러블 프로토콜을 준수한 이터러블이다.
const array = [1, 2, 3];

// Symbol.iterator 메서드는 이터리이터를 반환한다.
const iterator = array[Symbol.iterator]();

// Symbol.iterator 메서드가 반환한 이터리이터는 next 메서드를 갖는다.
console.log("next" in iterator); // true
```

- next 메서드를 호출하면 이터러블을 순차적으로 한 단계씩 순회하며 순회 결과를 나타내는 **이터레이터 리절트 객체**를 반환한다.

```js
// 배열은 이터러블 프로토콜을 준수한 이터러블이다.
const array = [1, 2, 3];

// Symbol.iterator 메서드는 이터리이터를 반환한다.
const iterator = array[Symbol.iterator]();

// 이터레이터 리절트 객체는 value 와 done 프로퍼티를 갖는 객체다.
console.log(iterator.next());
console.log(iterator.next());
console.log(iterator.next());
console.log(iterator.next());

/*
    { value: 1, done: false }
    { value: 2, done: false }
    { value: 3, done: false }
    { value: undefined, done: true }
*/
```

## 빌트인 이터러블

- 표준 빌트인 객체들은 빌트인 이터러블이다.
  - Array
  - String
  - Map
  - Set
  - TypedArray
  - arguments
  - DOM 컬렉션

## for…of 문

- `for...of` 문은 이터러블을 순회하면서 이터러블의 요소를 변수에 할당한다.
- `for...of` 문은 내부적으로 이터레이터의 next 메서드를 호출하여 이터러블을 순회하며 next 메서드가 반환한 이터레이터 리절트 객체의 value 프로퍼티 값을 `for...of` 문의 변수에 할당한다.
- 이터레이터 리절트 객체의 done 프로퍼티 값이 false이면 이터러블의 순회를 계속하고 true면 이터러블의 순회를 중단한다.

```js
for (const item of [1, 2, 3]) {
  // item 변수에 순차적으로 1, 2, 3 이 할당
  console.log(item); // 1, 2, 3
}
```

- 위 예제의 내부동작은 다음과 같다.

```jsx
// 이터러블
const iterable = [1, 2, 3];

// 이터러블의 Symbol.iterator 메서드를 호출하여 이터레이터를 생성한다.
const iterator = iterable[Symbol.iterator]();

for (;;) {
  // 이터레이터의 next 메서드를 호출하여 이터러블을 순회한다.
  const res = iterator.next();

  // next 메서드가 반환한 이터레이터 리절트 객체의 done 프로퍼티 값이 true 이면 이터러블의 순회를 중단한다.
  if (res.done) break;

  // 이터레이터 리절트 객체의 value 프로퍼티 값을 item 변수에 할당한다.
  const item = res.value;
  console.log(item); // 1, 2, 3
}
```

## 이터러블과 유사 배열 객체

- 유사 배열 객체는 length 프로퍼티를 갖기 때문에 for 문으로 순회할 수 있고, 인덱스를 나타내는 숫자 형식의 문자열을 프로퍼티 키로 가지므로 마치 배열처럼 인덱스로 프로퍼티 값에 접근할 수 있다.

```js
// 유사 배열 객체
const arrayLike = {
  0: 1,
  1: 2,
  2: 3,
  length: 3,
};

// 유사 배열 객체는 length 프로퍼티를 갖기 때문에 for 문으로 순회할 수 있다.
for (let i = 0; i < arrayLike.length; i++) {
  // 유사 배열 객체는 마치 배열처럼 인덱스로 프로퍼티 값에 접근할 수 있다.
  console.log(arrayLike[i]); // 1 2 3
}
```

- 유사 배열 객체는 이터러블이 아닌 일반 객체다.
- 유사 배열 객체에는 Symbol.iterator 메서드가 없기 때문에 `for...of` 문으로 순회할 수 없다.

```js
for (const item of arrayLike) {
  console.log(item); // TypeError
}
```

- ES6에서 도입된 Array.from 메서드를 사용하여 배열로 간단히 변환할 수 있다.
- Array.from 메서드는 유사 배열 객체 또는 이터러블을 인수로 전달받아 배열로 변환하여 반환한다.

```js
// 유사 배열 객체
const arrayLike = {
  0: 1,
  1: 2,
  2: 3,
  length: 3,
};

// Array.from은 유사 배열 객체 또는 이터러블을 배열로 반환한다.
const arr = Array.from(arrayLike);
console.log(arr); // [1, 2, 3]
```

## 이터레이션 프로토콜의 필요성

- 이터레이션 프로토콜은 다양한 데이터 공급자가 하나의 순회 방식을 갖도록 규정하여 데이터 소비자가 효율적으로 다양한 데이터 공급자를 사용할 수 있도록 **데이터 소비자와 데이터 공급자를 연결하는 인터페이스의 역할을 한다.**

## 사용자 정의 이터러블

1. 사용자 정의 이터러블 구현

```js
// 피보나치 수열을 구현한 사용자 정의 이터러블
const fibonacci = {
  // Symbol.iterator 메서드를 구현하여 이터러블 프로토콜을 준수한다.
  [Symbol.iterator]() {
    let [pre, cur] = [0, 1];
    const max = 10; // 수열의 최대값

    // next 메서드는 이터레이터 리절트 객체를 반환한다.
    return {
      next() {
        [pre, cur] = [cur, pre + cur];
        // 이터레이터 리절트 객체를 반환한다.
        return { value: cur, done: cur >= max };
      },
    };
  },
};

// 이터러블인 fibonacci 객체를 순회할 때마다 next 메서드가 호출한다.
for (const num of fibonacci) {
  console.log(num); // 1 2 3 5 8
}
```

- 이터러블은 `for...of` 문뿐만 아니라 스프레드 문법, 배열 디스트럭처링 할당에도 사용할 수 있다.

```jsx
// 이터러블은 스프레드 문법의 대상이 될 수 있다.
const arr = [...fibonacci];
console.log(arr); // [1, 2, 3, 5, 8]

// 이터러블은 배열 디스트럭처링 할당의 대상이 될 수 있다.
const [first, second, ...rest] = fibonacci;
console.log(first, second, rest); // 1 2 [3, 5, 8]
```

2. 이터러블을 생성하는 함수

- 수열의 최대값을 인수로 전달받아 이터러블을 반환하는 함수를 만들 수 있다.

```jsx
// 피보나치 수열을 구현한 사용자 정의 이터러블
const fibonacci = function (max) {
  let [pre, cur] = [0, 1];

  // Symbol.iterator 메서드를 구현한 이터러블을 반환한다.
  return {
    [Symbol.iterator]() {
      return {
        next() {
          [pre, cur] = [cur, pre + cur];
          return { value: cur, done: cur >= max };
        },
      };
    },
  };
};

for (const num of fibonacci(10)) {
  console.log(num); // 1 2 3 5 8
}
```

3. 이터러블이면서 이터레이터인 객체를 생성하는 함수

- 이터러블이면서 이터레이터 객체를 생성하여 반환하는 함수는 다음과 같다.

```js
const fibonacci = function (max) {
    let [pre, cur] = [0, 1];

    // Symbol.iterator 메서드를 구현한 이터러블을 반환한다.
    return {
        [Symbol.iterator]() {
            return this;
        },
        // next 메서드는 이터레이터 리절트 객체를 반환
        next() {
            [pre, cur] = [cur, pre + cur];
            return {value: cur, done: cur >= max};
        }

    }
};

// iter는 이터러블이면서 이터레이터
let iter = fibonacci(10);

for (const num of iter) {
    console.log(num); // 1 2 3 5 8
}

iter = fibonacci(10);

// iter는 이터레이터이므로 이터레이션 리절트 객체를 반환하는 next 메서드르 소유한다.
console.log(iter.next());
console.log(iter.next());
console.log(iter.next());
console.log(iter.next());
console.log(iter.next());
console.log(iter.next());

/*
    { value: 1, done: false }
    { value: 2, done: false }
    { value: 3, done: false }
    { value: 5, done: false }
    { value: 8, done: false }
    { value: 13, done: true }
/*
```

4. 무한 이터러블과 지연 평가

```js
const fibonacci = function () {
  let [pre, cur] = [0, 1];

  // Symbol.iterator 메서드를 구현한 이터러블을 반환한다.
  return {
    [Symbol.iterator]() {
      return this;
    },
    // next 메서드는 이터레이터 리절트 객체를 반환
    next() {
      [pre, cur] = [cur, pre + cur];
      // 무한을 구현하므로 done 프로퍼티를 생략
      return { value: cur };
    },
  };
};

for (const num of fibonacci()) {
  if (num > 10000) break;
  console.log(num); // 1 2 3 5 8... 4181 6765
}

// 배열 디스트럭처링 할당을 통해 무한 이터러블에서 3개의 요소만 취득한다.
const [f1, f2, f3] = fibonacci();
console.log(f1, f2, f3); // 1 2 3
```

- 이터러블은 **지연 평가**를 통해 데이터를 생성
- 지연 평가는 데이터가 필요한 시점 이전까지는 미리 데이터를 생성하지 않다가 데이터가 필요한 시점이 되면 그때야 비로소 데이터를 생성하는 기법이다. 즉, 펼가 결과가 필요할 때까지 평가를 늦추는 기법
- 지연평가를 사용하면 불필요한 데이터를 미리 생성하지 않고 필요한 데이터를 필요한 순간에 생성하므로 빠른 실행 속도를 기대할 수 있고 불필요한 메모리를 소비하지 않으며 무한도 표현할 수 있따는 장점이 있다.
