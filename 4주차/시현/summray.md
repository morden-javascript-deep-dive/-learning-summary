## 13장 스코프

# 13.1 스코프란?

모든 식별자는 자신이 선언된 위치에 따라 유효 범위를 갖는다.

유효 범위에 따라 다른 코드가 이 식별자를 참조할 수 있다.

```tsx
function add(x, y) {
  // 매개변수는 함수 몸체 내부에서만 참조할 수 있다.
  // 즉, 매개변수의 스코프(유효범위)는 함수 몸체 내부다.
  console.log(x, y); // 2 5
  return x + y;
}

add(2, 5);

// 매개변수는 함수 몸체 내부에서만 참조할 수 있다.
console.log(x, y); // ReferenceError: x is not defined
```

```tsx
var var1 = 1; // 코드의 가장 바깥 영역에서 선언한 변수

if (true) {
  var var2 = 2; // 코드 블록 내에서 선언한 변수
  if (true) {
    var var3 = 3; // 중첩된 코드 블록 내에서 선언한 변수
  }
}

function foo() {
  var var4 = 4; // 함수 내에서 선언한 변수

  function bar() {
    var var5 = 5; // 중첩된 함수 내에서 선언한 변수
  }
}

console.log(var1); // 1
console.log(var2); // 2
console.log(var3); // 3
console.log(var4); // ReferenceError: var4 is not defined
console.log(var5); // ReferenceError: var5 is not defined
```

- 식별자 결정 (Identifier Resolution)
    - 이름이 같은 두 개의 변수 중에서 참조할 변수를 결정하는 일

전역 변수 x와 foo 함수 안에 있는 x는 스코프가 다른 별개의 변수이다.

```tsx
var x = 'global';

function foo() {
  var x = 'local';
  console.log(x); // ①
}

foo();

console.log(x); // ②
```

스코프는 안에서는 식별자가 유일해야 한다.

하지만 다른 스코프에서는 같은 이름의 식별자를 사용할 수 있다.

```tsx
function foo() {
  var x = 1;
  // var 키워드로 선언된 변수는 같은 스코프 내에서 중복 선언을 허용한다.
  // 아래 변수 선언문은 자바스크립트 엔진에 의해 var 키워드가 없는 것처럼 동작한다.
  var x = 2;
  console.log(x); // 2
}
foo();
```

# 13.2 스코프의 종류

## 13.2.1 전역과 전역 스코프

- 전역 변수 (Global Variable)
    - 전역에 선언한 변수로서 전역 스코프를 갖는다.
    - 어디서든지 참조할 수 있다.

## 13.2.2 지역과 지역 스코프

- 지역 변수 (Local Variable)
    - 자신의 지역 스코프 및 하위 지역 스코프에서만 유효한 변수.

# 13.3 스코프 체인

스코프는 함수 중첩에 의해 계층적인 구조를 갖으며 밑에서 위로 올라간다.

최상위 스코프는 전역 스코프

- 스코프 체인 (Scope Chain)
    - 스코프가 계층적으로 연결되어있는 관계
- 스코프 체이닝 (Scope Chaining)
    - 변수를 참조할 때 현재 스코프에서 상위 스코프로 이동하며 선언된 변수를 검색해가는 과정

## 13.3.1 스코프 체인에 의한 변수 검색

스코프는 **“식별자를 검색하는 규칙”** 이라고 표현하는 것이 더 적합

```tsx
// 전역 함수
function foo() {
  console.log('global function foo');
}

function bar() {
  // 중첩 함수
  function foo() {
    console.log('local function foo');
  }

  foo(); // ①
}

bar();
```

# 13.4 함수 레벨 스코프

var로 선언된 변수는 **함수 레벨 스코프 (Function Level Scope)** 만을 인정한다

```tsx
var x = 1;

if (true) {
  // var 키워드로 선언된 변수는 함수의 코드 블록(함수 몸체)만을 지역 스코프로 인정한다.
  // 함수 밖에서 var 키워드로 선언된 변수는 코드 블록 내에서 선언되었다 할지라도 모두 전역 변수다.
  // 따라서 x는 전역 변수다. 이미 선언된 전역 변수 x가 있으므로 x 변수는 중복 선언된다.
  // 이는 의도치 않게 변수 값이 변경되는 부작용을 발생시킨다.
  var x = 10;
}

console.log(x); // 10
```

```tsx
var i = 10;

// for 문에서 선언한 i는 전역 변수다. 이미 선언된 전역 변수 i가 있으므로 중복 선언된다.
for (var i = 0; i < 5; i++) {
  console.log(i); // 0 1 2 3 4
}

// 의도치 않게 변수의 값이 변경되었다.
console.log(i); // 5
```

# 13.5 렉시컬 스코프

- 동적 스코프 (Dynamic Scope)
    - 함수의 호출 위치에 따라 스코프가 결정된다.
- 정적 스코프 (Static Scope) or 렉시컬 스코프 (Lexical Scope)
    - 함수의 정의 위치에 따라 스코프가 결정된다.

자바스크립트는 렉시컬 스코프를 따른다.

따라서 함수의 정의 위치에 따라 스코프가 결정된다.

즉, 함수의 상위 스코프는 언제나 자신이 정의된 스코프

## 14장 전역 변수의 문제점

# 14.1 변수의 생명 주기

## 14.1.1 지역 변수의 생명 주기

변수는 선언에 의해 생성되고 할당을 통해 값을 갖으며, 언젠가는 소멸되는 **생명 주기**를 갖고 있다.

변수의 생명 주기는 메모리 공간이 확보 (Allocate)된 시점부터 메모리 공간이 해제 (Release) 되어 가용 메모리 풀 (Memory Pool)에 반환되는 시점까지를 의미

지역 변수의 생명 주기는 함수의 생명 주기와 같다.

```jsx
function foo() {
  var x = 'local';
  console.log(x); // local
  return x;
}

foo();
console.log(x); // ReferenceError: x is not defined
```

## 14.1.2 전역 변수의 생명 주기

전역 변수는 코드가 로드 되자마자 곧바로 해석되고 실행된다.

전체 코드의 마지막 문이 실행될 때 까지 메모리 상에 남아있게 된다.

var 키워드로 선언한 저역 변수의 생명 주기는 전역 객체의 생명 주기와 일치한다.

# 14.2 전역 변수의 문제점

- 암묵적 결합

모든 코드가 전역 변수를 참조 및 변경이 가능하다.

- 긴 생명 주기

생명 주기가 길기 때문에 메모리 리소르를 오래 소비한다.

- 스코프 체인 상에서 종점에 존재

스코프 체인 상에서 최상단 종점에 위치하기 때문에 변수 검색 속도가 제일 느리다.

- 네임스페이스 오염

다른 파일 내에서 같은 이름의 전역 변수가 있을 때, 같이 사용될 경우 예상치 못한 에러가 발생할 수 있다.

# 14.3 전역 변수의 사용을 억제하는 방법

## 14.3.1 즉시 실행 함수

모든 코드를 즉시 실행 함수로 바꾸면 모든 변수는 즉시 실행 함수의 지역 변수가 된다.

## 14.3.2 네임스페이스 객체

네임스페이스 객체를 사용하여 네임스페이스를 분리하는 방법

식별자 충돌은 방지할 수 있겠지만 네임스페이스 객체 자체가 전역 변수에 할당되므로 그다지 유용한 방법은 아니다.

```jsx
var MYAPP = {}; // 전역 네임스페이스 객체

MYAPP.person = {
  name: 'Lee',
  address: 'Seoul'
};

console.log(MYAPP.person.name); // Lee
```

## 14.3.3 모듈 패턴

관련 있는 변수와 함수를 모아 즉시 실행 함수로 감싸서 하나의 모듈로 만드는 방식

전역 변수 억제 + 캡슐화까지 챙길 수 있는 유용한 패턴

```jsx
var Counter = (function () {
  // private 변수
  var num = 0;

  // 외부로 공개할 데이터나 메서드를 프로퍼티로 추가한 객체를 반환한다.
  return {
    increase() {
      return ++num;
    },
    decrease() {
      return --num;
    }
  };
}());

// private 변수는 외부로 노출되지 않는다.
console.log(Counter.num); // undefined

console.log(Counter.increase()); // 1
console.log(Counter.increase()); // 2
console.log(Counter.decrease()); // 1
console.log(Counter.decrease()); // 0
```

## 14.3.4 ES6 모듈

ES6 모듈부터는 전역 변수를 사용할 수 없다.

파일 자체의 독립적인 모듈 스코프를 제공하기 때문

script 태그에 type=’module’ 을 추가하여 실행할 수 있다.

확장자는 mjs를 권장

```jsx
<script type="module" src="lib.mjs"></script><script type="module" src="app.mjs"></script>
```

## 15장 let, const 키워드와 블록 레벨 스코프

# 15.1 var 키워드로 선언한 변수의 문제점

## 15.1.1 변수 중복 선언 허용

```jsx
var x = 1;
var y = 1;

// var 키워드로 선언된 변수는 같은 스코프 내에서 중복 선언을 허용한다.
// 초기화문이 있는 변수 선언문은 자바스크립트 엔진에 의해 var 키워드가 없는 것처럼 동작한다.
var x = 100;
// 초기화문이 없는 변수 선언문은 무시된다.
var y;

console.log(x); // 100
console.log(y); // 1
```

## 15.1.2 함수 레벨 스코프

var 변수는 함수 레벨의 스코프만 인정한다.

함수 외부에서 선언하면 모두 전역 변수로 취급된다.

```jsx
var x = 1;

if (true) {
  // x는 전역 변수다. 이미 선언된 전역 변수 x가 있으므로 x 변수는 중복 선언된다.
  // 이는 의도치 않게 변수값이 변경되는 부작용을 발생시킨다.
  var x = 10;
}

console.log(x); // 10
```

```jsx
var i = 10;

// for문에서 선언한 i는 전역 변수이다. 이미 선언된 전역 변수 i가 있으므로 중복 선언된다.
for (var i = 0; i < 5; i++) {
  console.log(i); // 0 1 2 3 4
}

// 의도치 않게 i 변수의 값이 변경되었다.
console.log(i); // 5
```

## 15.1.3 변수 호이스팅

var 변수는 변수 선언문 이전에 참조할 수 있으며 undefined를 반환한다.

```jsx
// 이 시점에는 변수 호이스팅에 의해 이미 foo 변수가 선언되었다(1. 선언 단계)
// 변수 foo는 undefined로 초기화된다. (2. 초기화 단계)
console.log(foo); // undefined

// 변수에 값을 할당(3. 할당 단계)
foo = 123;

console.log(foo); // 123

// 변수 선언은 런타임 이전에 자바스크립트 엔진에 의해 암묵적으로 실행된다.
var foo;
```

# 15.2 let 키워드

## 15.2.1 변수 중복 선언 금지

let 키워드는 재할당은 가능하지만 재선언은 불가능하다.

let 변수를 중복 선언하면 문법 에러 (SyntaxError)가 발생한다.

```jsx
var foo = 123;
// var 키워드로 선언된 변수는 같은 스코프 내에서 중복 선언을 허용한다.
// 아래 변수 선언문은 자바스크립트 엔진에 의해 var 키워드가 없는 것처럼 동작한다.
var foo = 456;

let bar = 123;
// let이나 const 키워드로 선언된 변수는 같은 스코프 내에서 중복 선언을 허용하지 않는다.
let bar = 456; // SyntaxError: Identifier 'bar' has already been declared
```

## 15.2.2 블록 레벨 스코프

모든 블록을 스코프로 인정하는 블록 레벨 스코프를 따른다.

```jsx
let foo = 1; // 전역 변수

{
  let foo = 2; // 지역 변수
  let bar = 3; // 지역 변수
}

console.log(foo); // 1
console.log(bar); // ReferenceError: bar is not defined
```

## 15.2.3 변수 호이스팅

let 변수는 변수 호이스틩이 발생하지 않는 것처럼 동작한다.

```jsx
console.log(foo); // ReferenceError: foo is not defined
let foo;
```

```jsx
// var 키워드로 선언한 변수는 런타임 이전에 선언 단계와 초기화 단계가 실행된다.
// 따라서 변수 선언문 이전에 변수를 참조할 수 있다.
console.log(foo); // undefined

var foo;
console.log(foo); // undefined

foo = 1; // 할당문에서 할당 단계가 실행된다.
console.log(foo); // 1
```

var 변수와는 달리 선언 단계와 초기화 단계가 분리되어 진행

선언 단계는 런타임 이전에 암묵적으로 실행된다.

하지만 초기화 단계는 변수 선언문에 도달했을 때 실행된다.

이 사이의 갭을 일시적 사각지대 (Temporal Dead Zone, TDZ) 라고 한다.

```jsx
// 런타임 이전에 선언 단계가 실행된다. 아직 변수가 초기화되지 않았다.
// 초기화 이전의 일시적 사각 지대에서는 변수를 참조할 수 없다.
console.log(foo); // ReferenceError: foo is not defined

let foo; // 변수 선언문에서 초기화 단계가 실행된다.
console.log(foo); // undefined

foo = 1; // 할당문에서 할당 단계가 실행된다.
console.log(foo); // 1
```

## 15.2.4 전역 객체와 let

let 전역 변수는 전역 객체의 프로퍼티가 아니다.

보이지 않는 개념적인 블록 (전역 렉시컬 환경의 선언적 환경 레코드) 에 존재하게 된다.

```jsx
// 이 예제는 브라우저 환경에서 실행해야 한다.

// 전역 변수
var x = 1;
// 암묵적 전역
y = 2;
// 전역 함수
function foo() {}

// var 키워드로 선언한 전역 변수는 전역 객체 window의 프로퍼티다.
console.log(window.x); // 1
// 전역 객체 window의 프로퍼티는 전역 변수처럼 사용할 수 있다.
console.log(x); // 1

// 암묵적 전역은 전역 객체 window의 프로퍼티다.
console.log(window.y); // 2
console.log(y); // 2

// 함수 선언문으로 정의한 전역 함수는 전역 객체 window의 프로퍼티다.
console.log(window.foo); // ƒ foo() {}
// 전역 객체 window의 프로퍼티는 전역 변수처럼 사용할 수 있다.
console.log(foo); // ƒ foo() {}
```

# 15.3 const 키워드

const는 상수 (Constant)를 선언하기 위해 사용한다.

## 15.3.1 선언과 초기화

const 변수는 반드시 선언과 동시에 초기화를 해야한다.

```jsx
const foo = 1;
```

```jsx
const foo; // SyntaxError: Missing initializer in const declaration
```

```jsx
{
  // 변수 호이스팅이 발생하지 않는 것처럼 동작한다
  console.log(foo); // ReferenceError: Cannot access 'foo' before initialization
  const foo = 1;
  console.log(foo); // 1
}

// 블록 레벨 스코프를 갖는다.
console.log(foo); // ReferenceError: foo is not defined
```

## 15.3.2 재할당 금지

const 변수는 재할당이 금지된다.

```jsx
const foo = 1;
foo = 2; // TypeError: Assignment to constant variable.
```

## 15.3.3 상수

상수는 재할당이 금지된 변수를 의미한다.

상태 유지, 가독성, 유지보수 등의 이점을 살리기 위해서라면 상수 사용은 필수

const 변수의 원시 값은 불변한 값이므로 수정이 불가능하다.

상수명은 일반적으로 스네이크 케이스로 표현하는 것이 좋다.

```jsx
// 세율을 의미하는 0.1은 변경할 수 없는 상수로서 사용될 값이다.
// 변수 이름을 대문자로 선언해 상수임을 명확히 나타낸다.
const TAX_RATE = 0.1;

// 세전 가격
let preTaxPrice = 100;

// 세후 가격
let afterTaxPrice = preTaxPrice + (preTaxPrice * TAX_RATE);

console.log(afterTaxPrice); // 110
```

## 15.3.4 const 키워드와 객체

const 변수의 참조 타입 값 (객체)는 값 변경이 가능하다.

객체 자체가 변경 가능한 값이기 때문

하지만 재할당과 재선언은 여전히 불가능하다.

즉, const 키워드는 재할당을 금지할 뿐 “불변”을 의미하지는 않는다.

```jsx
const person = {
  name: 'Lee'
};

// 객체는 변경 가능한 값이다. 따라서 재할당없이 변경이 가능하다.
person.name = 'Kim';

console.log(person); // {name: "Kim"}
```

# 15.4 var vs. let vs. const

변수 선언은 기본적으로 const를 사용하는 것이 좋다.

재할당이 필요한 경우에 한정하여 let을 사용하는 것이 좋다.

- ES6를 사용한다면 var 키워드는 사용하지 않는다.
- 재할당이 필요한 경우 let 키워드를 사용한다. 변수 스코프는 최대한 좁게 유지
- 변경이 필요 없는 읽기 전용의 값은 const 키워드를 사용한다.
