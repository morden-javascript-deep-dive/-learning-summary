# ES6 함수의 추가기능

## 함수의 구분

- 자바스크립트의 함수는 일반적인 함수로서 호출할 수도 있고, new 연산자와 함께 호출하여 인스턴스를 생성할 수 있는 생성자 함수로서 호출할 수도 있으며, 객체에 바인딩되어 메서드로서 호출할 수도 있다.
- ES6 이전의 모든 함수는 일반 함수로서 호출할 수 있는 물론 생성자 함수로서 호출할 수 있다.
- 객체에 바인딩된 함수도 일반 함수로서 호출할 수 있는 것은 물론 생성자 함수로서 호출할 수도 있다.
- 함수에 전달되어 보조 함수의 역할을 수행하는 콜백 함수도 마찬가지다. 콜백 함수도 constructor이기 때문에 불필요한 프로토타입 객체를 생성한다.

## 메서드

- ES6 사양에서 메서드는 메서드 축약 표현으로 정의된 함수만을 의미한다.

```js
const obj = {
  x: 1,
  // foo는 메서드다.
  foo() {
    return this.x;
  },
  // bar에 바인딩된 함수는 메서드가 아닌 일반 함수다.
  bar: function () {
    return this.x;
  },
};

console.log(obj.foo()); // 1
console.log(obj.bar()); // 1
```

- ES6 사양에서 정의한 메서드는 인스턴스를 생성할 수 없는 non-constructor다.

```js
new obj.foo(); // TypeError
new obj.bar(); // bar {}
```

- ES6 메서드는 인스턴스를 생성할 수 없으므로 prototype 프로퍼티가 없고 프로토타입도 생성하지 않는다.

```js
obj.foo.hasOwnProperty("prototype"); // false

// obj.bar는 constructor인 일반 함수이므로 prototype 프로퍼티가 있다.
obj.bar.hasOwnProperty("prototype"); // true
```

- ES6 메서드는 자신을 바인딩한 객체를 가리키는 내부 슬롯 HomeObject를 가지므로 super 키워드를 사용할 수 있다.
- ES6 메서드가 아닌 함수는 super 키워드를 사용할 수 없다.

```js
const base = {
  name: "Son",
  sayHi() {
    return `Hi! ${this.name}`;
  },
};

const derived = {
  __proto__: base,
  // sayHi는 ES6 메서드다.
  // sayHi의 HomeObject는 derived를 가리키고
  // super는 derived의 프로토타입인 base를 가리킨다.
  sayHi() {
    return `${super.sayHi()}. how are you doing?`;
  },
};

console.log(derived.sayHi()); // Hi! Son. how are you doing?
```

## 화살표 함수

1. 화살표 함수 정의

- 화살표 함수는 함수 선언문으로 정의할 수 없고 함수 표현식으로 정의해야 한다.

```js
const multiply = (x, y) => x * y;
multiply(2, 3); // 6
```

```js
// 매개변수가 여러 개인 경우 소괄호 ()안에 매개변수를 선언한다.
const arrow = (x, y) => {...};

// 매개변수가 한 개인 경우 소괄호 ()를 생략할 수 있다.
const arrow = x => {...};

// 매개변수가 없는 경우 소괄호 ()를 생략할 수 없다.
const arrow = () => {...};
```

- 함수 몸체가 하나의 문으로 구성된다면 함수 몸체를 감싸는 중괄호 {}를 생략할 수 있다.
- 이때 함수 몸체 내부의 문이 값으로 평가될 수 있는 표현식인 문이라면 암묵적으로 반환된다.

```js
// concise body
const power = (x) => x ** 2;
power(2); // 4

// 위 표현은 다음과 같다.
// block body
const power = (x) => {
  return x ** 2;
};
```

- 함수 몸체를 감싸는 중괄호 {}를 생략한 경우 함수 몸체 내부의 문이 표현식이 아닌 문이라면 에러가 발생한다.

```js
const arrow = () => const x = 1; // SyntaxError

// 위 표현은 다음과 같이 해석된다.
const arrow = () => { return const x = 1; };
```

- 객체 리터럴을 반환하는 경우 객체 리터럴을 소괄호 ()로 감싸 주어야 한다.

```js
const create = (id, content) => ({ id, content });
create(1, "javascript"); // { id:1, content:"javascript"}

// 위 표현은 다음과 같다.
const create = (id, content) => {
  return { id, content };
};
```

2. 화살표 함수와 일반 함수의 차이

- 화살표 함수는 인스턴스를 생성할 수 없는 non-constructor다.

```js
const Foo = () => {};
// 화살표 함수는 생성자 함수로서 호출할 수 없다.
new Foo(); // TypeError
// 화살표 함수는 prototype 프로퍼티가 없다.
Foo.hasOwnProperty("prototype"); // false
```

- 중복된 매개변수 이름을 선언할 수 없다.

```js
const fn = (a, a) => a + a; // Uncaught SyntaxError: Duplicate parameter name not allowed in this context
```

- 화살표 함수는 함수 자체의 this, arguments, super, new.target 바인딩을 갖지 않는다.

3. this

- 화살표 함수는 함수 자체의 this 바인딩을 갖지 않는다.
- 따라서 화살표 함수 내부에서 this를 참조하면 상위 스코프 this를 그대로 참조한다. → `lexical this` 라 한다.
- 만약 화살표 함수와 화살표 함수가 중첩되어 있다면 상위 화살표 함수에도 this 바인딩이 없으므로 스코프 체인 상에서 가장 가까운 상위 함수 중에서 화살표 함수가 아닌 함수의 this를 참조

```js
(function () {
  const foo = () => console.log(this);
  foo();
}).call({ a: 1 }); // {a:1}

(function () {
  const bar = () => () => console.log(this);
  bar()();
}).call({ a: 1 }); // {a:1}
```

- 프로퍼티에 할당한 화살표 함수도 스코프 체인 상에서 가장 가까운 this 참조

```js
const counter = {
  num: 1,
  increase: () => ++this.num, // 여기서의 this는 전역 객체를 가리킨다.
};

console.log(counter.increase()); // NaN
```

- 화살표 함수는 함수 자체의 this 바인딩을 갖지 않기 때문에 this를 교체할 수 없고 언제나 상위 스코프의 this 바인딩을 참조한다.

```js
const add = (a, b) => a + b;

console.log(add.call(null, 1, 2)); // 3
console.log(add.apply(null, [1, 2])); // 3
console.log(add.bind(null, 1, 2)()); // 3
```

- 객체에 함수값을 프로퍼티로 정의할 때 축약 표현으로 정의한 ES6 메서드를 사용하는 것이 좋다.

```js
const person = {
  name: "Son",
  sayHi() {
    console.log(`Hi! ${this.name}`);
  },
};

person.sayHi(); // Hi! Son
```

- 클래스 필드에 할당한 화살표 함수는 프로토타입 메서드가 아니라 인스턴스 메서드가 된다.

```js
class Person {
  // 클래스 필드 정의
  name = "Son";
  sayHi() {
    console.log(`Hi ${this.name}`);
  }
}

const person = new Person();
person.sayHi();
```

4. super

- 화살표 함수는 함수 자체의 super 바인딩을 갖지 않는다.
- 화살표 함수 내부에서 super를 참조하면 this와 마찬가지로 상위 스코프의 super를 참조한다.

```js
class Base {
  constructor(name) {
    this.name = name;
  }

  sayHi() {
    return `Hi! ${this.name}`;
  }
}

class Derived extends Base {
  // 화살표 함수의 super는 상위 스코프인 constructor의 super를 가리킨다.
  sayHi = () => `${super.sayHi()}. how are you doing?`;
}

const derived = new Derived("Son");
console.log(derived.sayHi());
//Hi! Son. how are you doing?
```

5. arguments

- 화살표 함수는 함수 자체의 arguments 바인딩을 갖지 않는다.
- 화살표 함수 내부에서 arguments를 참조하면 this와 마찬가지로 상위 스코프의 arguments를 참조한다.

```js
(function () {
  // 화살표 함수 foo의 argumetns는 상위 스코프인 즉시 실행 함수의 arguments를 가리킨다.
  const foo = () => console.log(arguments);
  foo(3, 4);
})(1, 2); // Arguments(2) [1, 2, callee: ƒ, Symbol(Symbol.iterator): ƒ]

// 전역에는 arguments 객체가 존재하지 않는다.
const foo = () => console.log(arguments);
foo(1, 2); // ReferenceError
```

## Rest 파라미터

1. 기본 문법

- Rest 파라미터는 함수에 전달된 인수들의 목록을 배열로 전달받는다.

```js
function foo(...rest) {
  // 매개변수 rest는 인수들의 목록을 배열로 전달받는 Rest 파라미터다.
  console.log(rest);
}

foo(1, 2, 3, 4, 5);
// [ 1, 2, 3, 4, 5 ]
```

```js
function foo(param, ...rest) {
  console.log(param); // 1
  console.log(rest); // [2, 3, 4, 5]
}

foo(1, 2, 3, 4, 5);

function bar(param1, param2, ...rest) {
  console.log(param1); // 1
  console.log(param2); // 2
  console.log(rest); // [3, 4, 5]
}

bar(1, 2, 3, 4, 5);
```

- Rest 파라미터는 반드시 마지막 파라미터이어야 한다.

```js
function foo(...rest, param1, param2);

foo(1, 2, 3, 4, 5); // SyntaxError
```

- Rest 파라미터는 단 하나만 선언할 수 있다.

```js
function foo(...rest1, ...rest2);

foo(1, 2, 3, 4, 5); // SyntaxError
```

```js
function foo(...rest) {}
console.log(foo.length); // 0

function bar(x, ...rest) {}
console.log(bar.length); // 1

function baz(x, y, ...rest) {}
console.log(baz.length); // 2
```

2. Rest 파라미터와 arguments 객체

- ES6에서는 Rest 파라미터를 사용해서 가변 인자 함수의 인수 목록을 배열로 직접 전달 받을 수 있다.

```js
function sum(...args) {
  // Rest 파라미터 args에는 배열 [1, 2, 3, 4, 5]가 할당된다.
  return args.reduce((pre, cur) => pre + cur, 0);
}
console.log(sum(1, 2, 3, 4, 5)); // 15
```

## 매개변수 기본값

- ES6에서 도입된 매개변수 기본값을 사용하면 함수 내에서 수행하던 인수 체크 및 초기화를 간소화할 수 있다.

```js
function sum(x = 0, y = 0) {
  return x + y;
}

console.log(sum(1, 2)); // 3
console.log(sum(1)); // 1
```

- 매개변수 기본값은 매개변수에 인수를 전달하지 않은 경우와 undefined를 전달한 경우에만 유효하다.

```js
function logName(name = "Son") {
  console.log(name);
}

logName(); // Son
logName(undefined); // Son
logName(null); // null
```

- Rest 파라미터에는 기본값을 지정할 수 없다.

```js
function foo(...rest = []) {
	console.log(rest);
}
// SyntaxError
```

# 배열

## 배열이란

- 배열(array)은 여러 개의 값을 순차적으로 나열한 자료구조

```js
const arr = ["apple", "banana", "orange"];
```

## 자바스크립트 배열은 배열이 아니다.

- 자료구조에서 말하는 배열은 동일한 크기의 메모리 공간이 빈틈으로 연속적으로 나열된 자료구조를 말한다.
- 밀집 배열(dense array)

  - 배열에 인덱스를 통해 단 한 번의 연산으로 임의의 요소에 접근(임의 접근, 시간복잡도 O(1))할 수 있다.
  - 정렬되지 않은 배열에서 특정한 요소를 검색하는 경우 배열의 모든 요소를 처음부터 특정 요소를 발견할 때까지 차례대로 검색(선형 검색, 시간복잡도 O(n))해야 한다.

  ```js
  function linearSearch(array, target) {
    const length = array.length;

    for (let i = 0; i < length; i++) {
      if (array[i] === target) {
        return i;
      }
    }
    return -1;
  }

  console.log(linearSearch([1, 2, 3, 4, 5, 6], 3)); // 2
  console.log(linearSearch([1, 2, 3, 4, 5, 6], 0)); // -1
  ```

- 희소 배열(sparse array)

  - 자바스크립트의 배열은 일반적인 배열이 아니라 각각의 메모리 공간이 동일한 크기를 갖지 않아도 되며, 연속적으로 이어져 있지 않을 수도 있다.

- 자바스크립트의 배열은 일반적인 배열의 동작을 흉내 낸 특수한 객체
  - 자바스크립트 배열은 인덱스를 나타내는 문자열을 프로퍼티 키로 가지며, length 프로퍼티를 갖는 특수한 객체다.
  - 자바스크립트 배열의 요소는 사실 프로퍼티 값이다.
  - 자바스크립트에서 사용할 수 있는 모든 값은 객체의 프로퍼티 값이 될 수 있으므로 어떤 타입의 값이라도 배열이 요소가 될 수 있다.
  ```js
  const arr = [
    "string",
    10,
    true,
    null,
    undefined,
    NaN,
    Infinity,
    [],
    {},
    function () {},
  ];
  ```
- 자바스크립트 배열은 해시 테이블로 구현된 객체이므로 인덱스로 요소에 접근하는 경우 일반적인 배열보다 성능적인 면에서 느릴 수밖에 없는 구조적인 단점이 있다.
  - 특정 요소를 검색하거나 요소를 삽입 또는 삭제하는 경우에는 일반적인 배열보다 빠른 성능을 기대할 수 있다

## length 프로퍼티와 희소 배열

- length 프로퍼티는 요소의 개수, 즉 배열의 길이를 나타내는 0 이상의 정수를 값으로 갖는다.
- length 프로퍼티의 값은 배열에 요소를 추가하거나 삭제하면 자동 갱신된다.

```js
const arr = [1, 2, 3];
console.log(arr.length); // 3

// 요소 추가
arr.push(4);
// 요소를 추가하면 length 프로퍼티의 값이 자동 갱신된다.
console.log(arr.length); // 4

// 요소 삭제
arr.pop();
// 요소를 삭제하면 length 프로퍼티의 값이 자동 갱신된다.
console.log(arr.length); // 3
```

- 희소 배열은 length와 배열 요소의 개수가 일치하지 않는다.
- 희소 배열의 length는 희소 배열의 실제 요소 개수보다 언제나 크다.
- 희소 배열을 생성하지 않도록 주의해야 한다.

```js
// 희소 배열
const sparse = [, 2, , 4];

// 희소 배열의 length 프로퍼티 값은 요소의 개수와 일치하지 않는다.
console.log(sparse.length); // 4
console.log(sparse); // [비어 있음, 2, 비어 있음, 4]

console.log(Object.getOwnPropertyDescriptors(sparse));

// 4
// [ <1 empty item>, 2, <1 empty item>, 4 ]
// {
//  '1': { value: 2, writable: true, enumerable: true, configurable: true },
//  '3': { value: 4, writable: true, enumerable: true, configurable: true },
//  length: { value: 4, writable: true, enumerable: false, configurable: false }
// }
```

## 배열 생성

1. 배열 리터럴

2. Array 생성자 함수

3. Array.of

- ES6에서 도입된 Array.of 메서드는 전달된 인수를 요소로 갖는 배열을 생성한다.
- Array.of는 Array 생성자 함수와 다르게 전달된 인수가 1개이고 숫자이더라도 인수를 요소로 갖는 배열을 생성한다.

```js
Array.of(1); // [1]

Array.of(1, 2, 3); // [1, 2, 3]

Array.of("string"); // ['string']
```

4. Array.from

- ES6에서 도입된 Array.from 메서드는 유사 배열 객체 또는 이터러블 객체를 인수로 전달받아 배열로 변환하여 반환한다.

```js
// 유사 배열 객체를 변환하여 배열을 생성한다.
Array.from({
  length: 2,
  0: "a",
  1: "b",
});

// 이터러블을 변환하여 배열을 생성한다. 문자열을 이터러블이다.
Array.from("Hello"); // ["H", "e", "l", "l", "o"]
```

## 배열 요소의 참조

## 배열 요소의 추가와 갱신

## 배열 요소의 삭제

## 배열 메서드

1. isAarry

2. indexOf

3. push

4. pop

5. unshift

6. shift

7. concat

8. splice

9. slice

10. join

11. reverse

12. fill

13. includes

14. flat

## 배열 고차 함수

1. sort

2. forEach

3. map

4. filter

5. reduce

6. some

7. every

8. find

9. findIndex

10. flatMap
