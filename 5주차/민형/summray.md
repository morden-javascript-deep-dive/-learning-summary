# 프로퍼티 어트리뷰트

## 내부 슬롯과 내부 메서드

- 내부 슬롯과 내부 메서드는 자바스크립트 엔진의 구현 알고리즘을 설명하기 위해 ECMAScript 사양에서 사용하는 의사 프로퍼티와 의사 메서드
- 내부 슬롯, 내부 메서드는 직접 접근할 수 없다.
- [[Proto]] 내부 슬롯의 경우, \_\_proto\_\_를 통해 간접적으로 접근할 수 있다.

## 프로퍼티 어트리뷰트와 프로퍼티 디스크립터 객체

- 자바스크립트 엔진은 프로퍼티를 생성할 때 프로퍼티의 상태를 나타내는 프로퍼티 어트리뷰트를 기본값으로 자동정의한다.
- 프로퍼티 어트리뷰트는 자바스크립트 엔진이 관리하는 내부 상태 값인 내부 슬록 [[Value]], [[Writable]], [[Enumerable]], [[Configurable]]이다.
- 프로퍼티 어트리뷰트는 직접 접근할 수 없지만 Object.getOwnPropertyDescriptor 메서드를 사용하여 간접적으로 확인할 수는 있다.

```js
const person = {
  name: "Lee",
};

console.log(Object.getOwnPropertyDescriptor(person, "name"));
// {value: 'Lee', writable: true, enumerable: true, configurable: true}
```

- 프로퍼티 디스크립터 객체
  - Object.getOwnPropertyDescriptor 메서드가 반환하는 객체. 즉, 프로퍼티 어트리뷰트로 구성된 객체라고 생각하면 됨
  - Object.getOwnPropertyDescriptor 메서드는 존재하지 않는 혹은 상속받은 프로퍼티를 요구하면 undefined를 반환한다.
- Object.getOwnPropertyDescriptors: 모든 프로퍼티의 프로퍼티 어트리뷰터 정보를 제공하는 프로퍼티 디스크립터 객체들을 반환

```js
const person = {
  name: "Lee",
  age: 25,
};

console.log(Object.getOwnPropertyDescriptors(person);
// {
//   name: {value: 'Lee', writable: true, enumerable: true, configurable: true},
//   age: {value: 20, writable: true, enumerable: true, configurable: true}
// }
```

## 데이터 프로퍼티와 접근자 프로퍼티

- 데이터 프로퍼티: 키와 값으로 구성된 일반적인 프로퍼티. 지금까지 살펴본 모든 프로퍼티는 데이터 프로퍼티
- 접근자 프로퍼티: 자체적으로 값을 갖지 않고 다른 데이터 프로퍼티의 값을 읽거나 저장할 때 호출되는 접근자 함수로 구성된 프로퍼티

1. 데이터 프로퍼티

- 프로퍼티 어트리뷰트
  - [[Value]]
  - [[Writable]](프로퍼티 값의 변경 가능 여부)
  - [[Enumerable]](프로퍼티의 열거 가능 여부)
  - [[Configurable]](프로퍼티 재정의 여부)

2. 접근자 프로퍼티

- 프로퍼티 어트리뷰트
  - [[Get]](접근자 프로퍼티를 통해 데이터 프로퍼티의 값을 읽을 때 호출되는 접근자)
  - [[Set]](접근자 프로퍼티를 통해 데이터 프로퍼티의 값을 저장할 때 호출되는 접근자)
  - [[Enumerable]](프로퍼티의 열거 가능 여부)
  - [[Configurable]](프로퍼티 재정의 여부)

## 프로퍼티 정의

- 프로퍼티 정의란 새로운 프로퍼티를 추가하면서 프로퍼티 어트리뷰트를 명시적으로 정의하거나, 기존 프로퍼티의 프로퍼티 어트리뷰트를 재정의하는 것
- Object.defineProperty 메서드를 사용해 프로퍼티 어트리뷰트를 정의할 수 있다. 인수로는 객체의 참조와 데이터 프로퍼티의 키인 문자열, 프로퍼티 디스크립터 객체를 전달
- 접근자 프로퍼티도 정의가능

```js
const person = {};

Object.defineProperty(person, "firstNmae", {
  value: "park",
  writable: true,
  enumerable: true,
  configurable: true,
});

let descriptor = Object.getOwnPropertyDescriptor(person, "firstName");
console.log(descriptor);
// fristName { value: 'park', writable: true, enumerable: true, configurable: true }
```

- [[Enumerable]]가 false인 프로퍼티는 Object.keys(perosn)를 호출했을 때 결과값에서 제외된다.
- [[Writable]]가 false이면 프로퍼티 값을 수정하려해도 무시된다.
- [[Configurable]]가 false면 해당 프로퍼티를 삭제하려해도 무시된다.
- 프로퍼티 디스크립터 객채의 프로퍼티, 대응하는 프로퍼티 어트리뷰트, 생략했을 때의 기본값
  - (value, [[Value]], undefiend)
  - (get, [[Get]], undefined)
  - (set, [[Set]], undefined)
  - (writable, [[Writable]], false)
  - (enumerable, [[Enumerable]], false)
  - (configurable, [[Configurable]], false)
- Object.defineProperties를 통해 여러 개의 프로퍼티를 한 번에 정의할 수 있다.

## 객체 변경 방지

1. 객체 확장 금지

- Object.preventExtensions
- 확장이 금지된 객체는 프로퍼티 추가가 금지

2. 객체 밀봉

- Object.seal
- 밀봉된 객체는 읽기와 쓰기만 가능하다.

3. 객체 동결

- Object.freeze
- 동결된 객체는 읽기만 가능하다.

4. 불변 객체

- Object.freeze 메서드로 객체를 동결하여도 중첩 객체까지 동경할 수 없다.
- 객채의 중첩 객체까지 동결하여 변경이 불가능한 읽기 전용의 객체를 구현하려면 객체를 값으로 갖는 모든 프로퍼티에 대해 재귀적으로 Object.freeze 메소드를 호출해야한다.

# 생성자 함수에 의한 객체 생성

## Object 생성자 함수

- new 연산자와 함께 Object 생성자 함수를 호출하면 빈 객체를 생성하여 반환. 빈 객체를 생성한 이후 프로퍼티 또는 메서드를 추가하여 객체를 완성할 수 있다.
- 생성자 함수란 new 연산자와 함께 호출하여 객체(인스턴스)를 생성하는 함수. 생성자 함수에 의해 생성된 객체를 인스턴스라 한다.

## 생성자 함수

1. 객체 리터럴에 의한 객체 생성 방식의 문제점

- 동일한 프로퍼티를 갖는 객체를 여러 개 생성해야 하는 경우 매번 같은 프로퍼티를 기술해야 하기 때문에 비효율적

2. 생성자 함수에 의한 객체 생성 방식의 장점

- 생성자 함수에 의한 객체 생성 방식은 마치 객체(인스턴스)를 생성하기 위한 템플릿(클래스)처럼 생성자 함수를 사용하여 프로퍼티 구조가 동일한 객체 여러개를 간편한게 생성할 수 있다.
- new 연산자와 함께 호출하면 해당 함수는 생성자 함수로 동작

3. 생성자 함수의 인스턴스 생성 과정

- 생성자 함수의 역할은 인스턴스를 생성하는 것과 생성된 인스턴스를 초기화(인스턴스 프로퍼티 추가 및 초기값 할당)하는 것이다.
- 인스턴스 생성 과정

  1. 인스턴스 생성과 this 바인딩

  - 암묵적으로 빈 객체가 생성(이 빈 객체가 바로 생성자 함수가 생성한 인스턴스)
  - 이 인스턴스는 this에 바인딩

  2. 인스턴스 초기화

  - 생성자 함수에 기술되어 있는 코드가 한 줄씩 실행되어 this에 바인딩 되어있는 인스턴스를 초기화
  - 인스턴스에 프로퍼티나 메서드를 추가하고 생성자 함수가 인수로 전달받은 초기값을 인스턴스 프로퍼티에 할당하여 초기화하거나 고정값을 할당

  3. 인스턴스 반환

  - 모든 처리가 끝나면 완성된 인스턴스가 바인딩된 this를 암묵적으로 반환

  ```js
      function Circle(radius) {
          // 1. 암묵적으로 인스턴스가 생성되고 this에 바인딩
          console.log(this); // Circle {}

          // 2. this에 바인딩되어 있는 인스턴스를 초기화한다.
          this.radius = radius;
          this.getDiameter = function (0 {
              return 2 * this.radius;
          })

          // 3. 완성된 인스턴스가 바인딩된 this가 암묵적으로 반환
      }

      // 인스턴스 생성. Circle 생성자 함수는 암묵적으로 this를 반환
      const circle = new Circle(1)
      console.log(cricle) // Circle { raidus: 1, getDiamert: f }
  ```

  - this가 아닌 다른 객체를 명시적으로 반환하면 this가 반환되지 못하고 return 문에 명시한 객체가 반환, 하지만 명시적으로 원시 값을 반환하면 값 반환은 무시되고 암묵적으로 this가 반환

4. 내부 메서드 [[Call]] 과 [[Construct]]

- 함수 선언문 또는 함수 표현식으로 정의한 함수는 일반적인 함수로서 호출할 수 있는 것은 물론 생성자 함수로서 호출할 수 있다.
- 함수는 객체이므로 일반객체와 동일하게 동작할 수 있다. 함수 객체는 일반 객체가 가지고 있는 내부 슬롯과 내부 메서드를 모두 가지고 있기 때문이다.
- 일반 객체는 호출할 수 없지만 함수는 호출할 수 있다.
- 함수 객체는 일반 객체가 가지고 있는 내부 슬롯과 내부 메서드는 물론, 함수로서 동작하기 위해 함수 객체만을 위한 [[Environment]], [[FormalParameters]] 등의 내부 슬롯과 [[Call]], [[Construct]] 같은 내부 메서드를 추가로 가지고 있다.
- 함수가 일반 함수로서 호출되면 함수 객체의 내부 메서드 [[Call]]이 호출되고 new 연산자와 함께 생성자 함수로 호출되면 내부 메서드 [[Construct]]가 호출
- 내부 메서드 [[Call]]을 갖는 함수 객체를 callable이라 하며, 내부 메서드 [[Construct]]를 갖는 함수 객체를 constructor, [[Construct]]를 갖지 않는 함수 객체를 non-constructor라고 부른다.
- 결론적으로 함수 객체는 callable이면서 constructor이거나 callable이면서 non-constructor다. 즉 모든 함수 객체는 호출할 수 있지만 모든 함수 객체를 생성자 함수로 호출 할 수 있는 것은 아니다.
  - 일반 함수로서만 호출할 수 있는 객체(여집합이 non-constructor)
  - 일반 함수 또는 생성자 함수 또는 생성자 함수로서 호출할 수 있는 객체(여집합이 constructor)
    ![모든 함수 객체는 callable이지만 모든 함수 객체가 constructor인 것은 아니다.](https://images.velog.io/images/gavri/post/187f5c59-2ad0-45fe-a51b-9701d33faa0b/image.png)

5. constructor와 non-constructor의 구분

- constructor: 함수 선언문, 함수 표현식, 클래스(클래스도 함수다)
- non-constructor: 메서드(ES6 메서드 축약 표현), 화살표 함수

6. new 연산자

- new 연삱와 함께 함수를 호출하면 해당함수는 생성자 함수로 동작한다. 다시 말해, 함수 객체의 내부 메서드 [Call]이 호출되는 것이 아니라 [[Construct]]가 호출된다.
- 단, new 연산자와 함께 호출하는 함수는 non-constructor가 아닌 constructor이어야 한다.

7. new.target

- new 연산자와 함께 생성자 함수로서 호출되면 함수 내부의 new.target은 함수 자신을 가리킨다. new 연산자 없이 일반 함수로 호출된 함수 내부의 new.target은 undefined다.

# 함수와 일급 객체

## 일급 객체

- 일급 객체 조건
  1. 무명의 리터럴로 생성할 수 있다. 즉, 런타임에 생성이 가능하다.
  2. 변수나 자료구조(객체, 배열 등)에 저장할 수 있다.
  3. 함수의 매개변수에 전달할 수 있다.
  4. 함수의 반환값으로 사용할 수 있다.

## 함수 객체의 프로퍼티

1. arguments 프로퍼티

- arguments 객체는 함수 호출 시 전달된 인수들의 정보를 담고 있는 순회 가능한 유사 배열 객체
- 유사 배열 객체와 이터러블
  - ES6에서 도입된 이터레이션 프로토콜을 준수하면 순회 가능한 자료구조인 이터러블이 된다.
  - 이터러블의 개념이 없었던 ES5에서 arguments 객체는 유사 배열 객체로 구분되었다. 하지만 이터러블이 도입된 ES6부터 arguments 객체는 유사 배열 객체이면서 동시에 이터러블이다.

2. caller 프로퍼티

3. length 프로퍼티

- 함수를 정의할 때 선언한 매개변수의 개수를 가리킨다.

4. name 프로퍼티

- 함수의 이름을 나타낸다.

5. \_\_proto\_\_ 접근자 프로퍼티

- 모든 객체는 [[Prototype]]이라는 내부 슬롯을 갖는다. [[Prototype]] 내부 슬롯은 객체지향 프로그래밍의 상속을 구현하는 프로토타입 객체를 가리킨다.
- \_\_proto\_\_ 프로퍼티는 [[Prototype]] 내부 슬롯이 가리키는 프로토타입 객체에 접근하기 위해 사용하는 접근자 프로퍼티

6. prototype 프로퍼티

- 생성자 함수로 호출할 수 있는 함수 객체, 즉 constructor 만이 소유하는 프로퍼티다.
- 일반 객체와 생성자 함수로 호출할 수 없는 non-construcotr에는 prototype 프로퍼티가 없다.
- prototype 프로퍼티는 함수가 객체를 생성하는 생성자 함수로 호출될 때 생성자 함수가 생성할 인스턴스의 프로토타입 객체를 가리킨다.
