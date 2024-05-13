
# 16장 프로퍼퍼티 어트리뷰트

## 내부 슬롯과 내부 메서드

- ECMAScript 사양에서 사용하는 의사 프로퍼티와 의사 메서드
- 이중 대괄호([[...]])로 감싼 이름
- 자바스크립트 엔진에서 동작하나 개발자가 직접 접근 가능한 외부 공개 객체의 프로퍼티는 아니다.
- 단 일부 내부 슬롯과 내부 메서드에 한해 간접적으로 접근이 가능하다.
  - 모든 객체는 [[Prototype]] 내부 슬롯을 가지나 직접 접근할 수 없지만 **proto**를 통해 간접적으로 접근이 가능하다.

## 프로퍼티 어트리뷰트와 프로퍼티 디스크립터 객체

- 자바스크립트 엔진은 프로퍼티를 생성할 때 프로퍼티의 상태를 나타내는 프로퍼티 어트리뷰트를 기본값으로 자동 정의한다.
- 프로퍼티의 상태
  - 프로퍼티의 값
  - 값의 갱신 가능여부
  - 열거 가능 여부
  - 재정의 가능 여부
- 프로퍼티 어트리뷰트의 내부 슬롯([[Value]], [[Writable]], [[Enumerable]], [[Configurable]])은 직접 접근이 불가능 하지만 Object.getOwnPropertyDescriptor 메서드를 사용해 간접적으로 접근이 가능하다.

```javascript
const person = {
  name: "Lee",
};

console.log(Object.getOwnPropertyDescriptor(person, "name"));
// { value: "Lee", writable: true, enmerable: true, configurable: true }
```

- 첫 번째 매개변수에는 객체의 참조를 두번째 매개변수에는 프로퍼티 키를 전달하면 프로퍼티 어트리뷰트 정보를 제공하는 프로퍼티 디스크립터 객체를 반환한다.
- 존재하지 않는 프로퍼티라면 undefined를 반환한다.
- Object.getOwnPropertyDescriptors를 사용하면 하나의 프로퍼티가 아닌 모든 프로퍼티의 프로퍼티 어트리뷰트 정보를 제공한다.

## 데이터 프로퍼티와 접근자 프로퍼티

- 데이터 프로퍼티 - 키와 값으로 구성된 일반적인 프로퍼티 - 데이터 프로퍼티 어트리뷰트 종류
  ![데이터프로퍼티](https://i.ibb.co/r4M7pQB/image.png)
- 위에 예시 코드 참조
- 접근자 프로퍼티

  - 자체적으로는 값을 갖지 않고 다른 데이터 프로퍼티의 값을 읽거나 저장할 때 호출되는 접근자 함수로 구성된 프로퍼티
  - 접근자 프로퍼티 어트리뷰트 종류
    ![접근자프로퍼티](https://i.ibb.co/mHwrn3X/image.png)
  - 접근자 함수는 getter/setter 함수로도 불리며 접근자 프로퍼티는 두 개 모두 정의하거나 하나만 정의할 수 있다.

  ```javascript
  const person = {
    // 데이터 프로퍼티
    firstName: "Ungmo",
    lastName: "Lee",

    // fullName은 접근자 함수로 구성된 접근자 프로퍼티
    get fullName() {
      return `${this.firstName} ${this.lastName}`;
    },

    set fullName(name) {
      // 배열 디스트럭처링 할당
      [this.firstName, this.lastName] = name.split(" ");
    },
  };

  // 데이터 프로퍼티를 통한 프로퍼티 값의 참조
  console.log(person.firstNName + " " + person.lastName); // Ungmo Lee

  // 접근자 프로퍼티를 통한 프로퍼티 값의 저장
  // 접근자 프로퍼티 fullName에 값을 저장하면 setter 함수가 호출된다.
  person.fullName = "Heegun Lee";
  console.log(person); // { firstName : "Heegun", lastName : "Lee"}

  // 접근자 프로퍼티를 통한 프로퍼티 값의 참조
  // 접근자 프로퍼티 fullName에 접근하면 getter함수가 호출된다.
  console.log(person.fullName); // Heegun Lee

  let descriptor = Object.getOwnProperyDescriptor(person, "firstName");
  console.log(descriptor);
  // { value: "Heegun", writable: true, enumerable: true, configurable: "true: }

  descriptor = Object.getOwnPropertyDescriptor(person, "fullName");
  console.log(descriptor);
  // { get: f, set: f, enumerable: true, configuable: true }
  ```

  - fullName으로 프로퍼티 값에 접근하면 내부적으로 [[Get]] 내부 메서드가 호출되어 다음과 같이 동작한다.
    - 프로퍼티 키가 유효한지 확인한다. 프로퍼티 키는 문자열 또는 심벌이여야 함
    - 프로토타입 체인에서 프로퍼티를 검색한다. person 객체에 fullName 프로퍼티가 존재한다.
    - 검색된 fullName 프로퍼티가 데이터 프로퍼티인지 접근자 프로퍼티인지 확인한다.
    - 접근자 프로퍼티 fullName의 프로퍼티 어트리뷰트 [[Get]]의 값 getter 함수를 호출하여 그 결과를 반환한다.

## 프로퍼티 정의

- 새로운 프로퍼티를 추가하면서 프로퍼티 어트리뷰트를 명시적으로 정의, 기존 프로퍼티의 프로퍼티 어트리뷰트를 재정의 하는 것을 말한다.
- Object.defineProperty 메서드를 사용해 프로퍼티의 어트리뷰트를 재정의 가능하다.
  - 인수로는 객체의 참조, 데이터 프로퍼티의 키인 문자열, 프로퍼티 디스크립터 객체를 전달한다.

```javascript
const person = {}

// 데이터 프로퍼티 정의
Object.defineProperty(person. 'firstName', {
    value: 'Ungmo',
    writable: true,
    enumerable: true,
    configurable: true
});

Object.defineProperty(person, 'lastName', {
    value: 'Lee'
});

let descriptor = Object.getOwnPropertyDescriptor(person, 'firstName');
console.log('firstName', descriptor);
// firstName { value: "Ungmo", writable: true, enumerable: true, configurable: true }

// 디스크립터 객체의 프로퍼티 누락 시 undefined, false가 기본 값
descriptor = ObjectgetOwnPropertyDescriptor(person, 'lastName');
console.log('lastName', descriptor);
// lastName { value: "Lee", writable: false, enumerable: false, configurable: false }

// [[Enumerable]] 값이 false라면 해당 프로퍼티는 for...in 문이나 Object.keys로 열거할 수 없다.
console.log(Object.keys(person));
// ["firstName"]

// [[Writable]] 값이 false인 경우 해당 프로퍼티의 [[Value]]를 변경하면 에러는 발생하지 않으나 무시한다.
person.lastName = "Kim";

// [[Configurable]]의 값이 false라면 해당 프로퍼티를 삭제할 수 없고 에러는 안나나 무시한다.
delete person.lastName;


// [[Configurable]]의 값이 false라면 프로퍼티 재정의가 불가능하다.
// Object.defineProperty(person, 'lastName', { enumerable: true });
// Uncaught TypeError: Cannot redefine property: lastName
descriptor = Object.getOwnPropertyDescriptor(person. 'lastName');
console.log('lastName', descriptor);
// lastName { value: "Lee", writable: false, enumerable: false, configurable: false }
```

- Object.defineProperty 메서드로 프로퍼티를 정의할 때 프로퍼티 디스크립터 객체의 프로퍼티를 생략된 어트리뷰트의 기본 값
  ![생략된어트리뷰트](https://i.ibb.co/p4HXWYZ/image.png)
- Object.definePropertys를 사용하면 여러 개의 프로퍼티를 정의할 수 있다.

## 객체 변경 방지

- 객체 변경 방지 메서드들은 객체의 변경을 금지하는 강도가 다르다.
- 객체 확장 금지
  - Object.preventExtensions 메서드
  - 프로퍼티 추가를 금지하며 확장이 금지된 객체는 프로퍼티 추가가 금지된다.
  - Object.isExtensible 메서드로 확장이 가능한 객체인지 확인 가능하다.
- 객체 밀봉
  - Object.seal 메서드
  - 프로퍼티 추가 및 삭제와 프로퍼티 어트리뷰트 재정의를 금지한다.
  - 밀봉된 객체는 읽기와 쓰기만 가능하다.
  - Object.isSealed 메서드로 밀봉된 객체인지 확인 가능하다.
- 객체 동결
  - Object.freeze 메서드
  - 프로퍼티 추가 및 삭제와 프로퍼티 어트리뷰트 재정의 금지, 프로퍼티 값 갱신 금지
  - 동결된 객체는 읽기만 가능하다.
  - Object.isFrozen 메서드를 통해 동결된 객체인지 확인 가능하다.
- 불변 객체

  - 앞에서 살펴본 메서드들은 얕은 변경 방지로 중첩 객체는 영향을 주지 못한다.

  ```javascript
  const person = {
    name: "Lee",
    address: { city: "Seoul" },
  };
  Object.freeze(person);

  console.log(Object.isFrozen(person)); // true
  console.log(Object.isFrozen(person.address)); // false

  person.address.city = "Busan";
  console.log(person); // { name: "Lee", address: {city: "Busan"}}
  ```

  - 따라서 중첩 객체까지 동결시키려면 재귀적으로 Object.freeze 메서드를 호출해야한다.

# 17장 생성자 함수에 의한 객체 생성

- Obejct 생성자 함수
- new 연산자와 Obejct 생성자 함수를 호출해 빈 객체를 생성하며 반환 후 프로퍼티 또는 메서드를 추가해 객체를 완성한다.
  - 생성자 함수란 new 연산자와 함께 호출해 객체를 생성하는 함수
  - Object 이외에도 String, Number, Boolean 등등 여러 빌트인 생성자 함수를 제공한다.
- 객체 리터럴이 더 간편하므로 특별한 이유 없이는 Object 생성자 함수를 사용해 객체를 생성하는 것은 유용해보이지 않는다.

```javascript
const person = new Object();

person.name = "Lee";
person.sayHello = function () {
  console.log("Hi! My name is + this.name");
};

console.log(person); // {name: 'Lee', sayHello: f}
person.sayHello(); // Hi! My name is Lee
```

- 생성자 함수

  - 객체 리터럴 생성 방식은 직관적이고 편리하나 하나의 객체만 생성하기 때문에 동일한 프로퍼티 객체를 여러 개 생성하려면 매번 같은 프로퍼티를 기술해야한다.
  - 생성자 함수에 의한 객체 생성 방식을 사용하면 프로퍼티 구조가 동일한 객체 여러개를 간편하게 생성 가능하다.

  ```javascript
  // 생성자 함수
  function Circle(radius) {
    // 인스턴스 초기화
    this.radius = radius;
    this.getDiameter = function () {
      return 2 * this.radius;
    };
  }

  // 인스턴스 생성
  const circle1 = new Circle(5);
  const circle2 = new Circle(10);

  console.log(circle1.getDiameter());
  console.log(circle2.getDiameter());
  ```

  - new 연산자와 함께 호출하면 해당 함수는 생성자 함수로 동작하지만 new 연산자가 없다면 일반 함수로 작동한다.
  - 생성자 함수의 인스턴스 생성 과정

    1. 인스턴스 생성과 this 바인딩

    - 암묵적으로 빈 객체가 생성되고 빈 객체(인스턴스)는 this에 바인딩 된다.
    - 런타임 이전에 처리된다.

    2. 인스턴스 초기화

    - 생성자 함수에 기술되있는 코드가 한 줄씩 실행되며 this에 바인딩 되어있는 인스턴스를 초기화한다.
    - 이는 개발자가 기술한다.

    3. 인스턴스 반환

    - 생성자 함수 내부에서 모든 처리가 끝나면 완성된 인스턴스가 바인딩된 this를 암묵적으로 반환한다.

      ```javascript
      function Circle(radius) {
        // 1. 암묵적으로 빈 객체가 생성되고 this에 바인딩 된다.

        // 2. this에 바인딩되어 있는 인스턴스를 초기화 한다.
        this.radius = radius;
        this.getDiameter = function () {
          return 2 * this.radius;
        };
      }

      // 인스턴스 생성. Circle 생성자 함수는 암묵적으로 this를 반환한다.
      const circle1 = new Circle(5);
      const circle2 = new Circle(10);

      console.log(circle1.getDiameter());
      console.log(circle2.getDiameter()); // Circle {radius: 1, getDiameter: f}
      ```

  - 만약 this가 아닌 다른 객체를 명시적으로 반환하면 return문에 명시한 객체가 반환된다. ex) return {}
  - 하지만 명시적으로 원시값을 반환하면 이는 무시되고 암묵적으로 this가 반환된다.
    - 따라서 생성자 함수 내에서 return문은 반드시 생략해야한다.

- 내부 메서드 [[Call]]과 [[Contruct]]
  - 함수는 객체이므로 일반객체와 동일하게 동작할 수 있다. 또한 생성자 함수로써도 호출할 수 있다.
    - 일반 객체처럼 프로퍼티나 메서드를 소유할 수 있다.
  - 하지만 일반 객체와는 다르다. 일반 객체는 호출할 수 없지만 함수는 호출할 수 있다.
  - 함수 객체는 일반 객체가 가지고 있는 내부 슬롯, 내부 메서드를 포함해서 추가적인 내부슬롯과 메서드가 있다.
    - [[Environment]], [[FormalParameters]] 내부 슬롯
    - [[Call]], [[Construct]] 내부 메서드
  - 함수가 일반 함수로서 호출되면 [[Call]]이 호출되고 new 연산자와 함께 호출하면 [[Construct]]가 호출된다.
  - 내부 메서드 [[Call]]을 갖는 함수 객체를 callable이라 하며 내부 메서드 [[Construct]]를 갖는 함수 객체를 constructor, [[Construct]]를 갖지 않는 함수 객체를 non-constructor라고 한다.
  - callable은 호출할 수 있는 객체, constructor는 생성자 함수로서 호출할 수 있는 함수, non-constructor는 그 반대를 뜻한다.
  - 호출할 수 없는 객체는 함수 객체가 아니므로 callable이 아닌 함수 객체는 없다.
- constructor와 non-constructor의 구분
  - constructor : 함수 선언문, 함수 표현식, 클래스
  - non-constructor : 메서드, 화살표 함수
    - 메서드는 ES6의 메서드 축약 표현만을 의미한다.
  - 즉 정의 방식에 따라 구분한다.
  - non-constructor 함수 객체를 생성자 함수로서 호출하면 에러 발생
- new 연산자
  - new 연산자와 함께 함수를 호출하면 해당 함수는 생성자 함수로 동작한다 이는 [[Construct]]가 호출된다는 뜻이다.
  - new 연산자 없이 호출하면 일반 함수로 호출되며 이는 [[Call]]이 호출되는 것을 뜻한다.
- new.target
  - 생성자 함수가 new 없이 호출되는 것을 방지하기 위해 ES6에는 new.target을 지원한다.
  - this와 유사하게 constructor인 모든 함수 내부에서 암묵적 지역변수와 같이 사용되며 메타 프로퍼티라고도 부른다.
  - new 연산자와 함께 생성자 함수로서 호출되면 함수 내부의 new.target은 함수 자신을 가리킨다.
  - new 연산자 없이 일반 함수로서 호출된 함수 내부의 new.target은 undefined이다.
  - IE에선 지원하지 않는다.

# 18장 함수와 일급 객체

- 일급 객체의 조건
  - 무명의 리터럴로 생성할 수 있다.
  - 변수나 자료구조(객체, 배열)등에 저장할 수 있다.
  - 함수의 매개변수에 전달할 수 있다.
  - 함수의 반환값으로 사용할 수 있다.
- 함수 객체의 프로퍼티
  - 함수는 일급 객체이므로 프로퍼티를 가질 수 있다.
  - 함수 객체의 프로퍼티 어트리뷰트를 Object.getOwnPropertyDescriptors로 확인하면 arguments, caller, length, name, prototype 프로퍼티가 나온다.
  - 이는 모두 함수의 데이터 프로퍼티이며 일반 객체에 없는 함수 객체 고유의 프로퍼티 이다.
  - 하지만 __proto__ 접근자 프로퍼티는 Object.prototype 객체의 프로퍼티를 상속받은 것으로 함수 객체가 아닌 모든 객체가 사용할 수 있다.
- arguments 프로퍼티

  - arguments 프로퍼티 값은 arguments 객체
  - arguments 객체는 함수 호출 시 전달된 인수들의 정보를 담고 있는 순회 가능한 유사 배열 객체
  - 함수 외부에선 참조 불가능하다.
  - ES3부터 표준에서 폐지되어 Function.argments 같은 사용법은 권장하지 않으며 지역변수처럼 사용할 수 있는 arguments 객체를 참조하도록 한다.
  - 함수는 인수가 전달되지 않으면 undefined를, 인수가 초과하면 초과된 인수는 무시한다.
  - 하지만 초과된 인수는 버려지지 않으며 암묵적으로 arguments 객체의 프로퍼티로 보관된다.
  - 따라서 arguments는 가변 인자 함수를 구현할 때 유용하다.

  ```javascript
  function sum() {
    let res = 0;
    for (let i = 0; i > arguments.length; i++) {
      res += arguments[i];
    }

    return res;
  }

  console.log(sum()); // 0
  console.log(sum(1, 2)); // 3
  console.log(sum(1, 2, 3)); // 6
  ```

  - arguments 객체는 배열이 아닌 유사 배열(length 프로퍼티를 가진 객체)로 배열 메서드를 사용하면 에러가 난다.
  - 이를 해결하기 위해 ES6에서는 Rest 파라미터를 도입했다.

  ```javascript
  function sum(...args) {

    return args.reduce((pre, cur) -> pre + cur, 0);
  }

  console.log(sum()); // 0
  console.log(sum(1,2)); // 3
  console.log(sum(1,2,3)); // 6
  ```

- caller 프로퍼티
  - ECMAScript 사양에 포함되지 않은 비표준 프로퍼티
  - 함수 자신을 호출한 함수를 가리킨다.
  - 함수호출 foo(bar)의 경우 bar 함수를 foo 함수 내에서 호출했기 때문에 bar 함수의 caller프로퍼티는 foo를 가리킨다.
  - bar 함수의 caller 프로퍼티는 Null이다.
- length 프로퍼티
  - 함수를 정의할 때 선언한 매개변수의 개수
  - arguments 객체의 length 프로퍼티는 인자의 개수를 가리키고 함수 객체의 length 프로퍼티는 매개변수를 가리킨다.
- name 프로퍼티
  - 함수 이름을 나타내며 ES6 이전까지는 비표준이였다가 이후에서 정식 표준이 되었다.
  - ES5에서는 빈 문자열을 값으로 가지지만 ES6에서는 함수 객체를 가리키는 식별자를 값으로 갖는다.
  - 함수 이름과 함수 객체를 가리키는 식별자는 의미가 다르므로 주의해야한다.
- __proto__접근자 프로퍼티
  - 객체지향 프로그래밍의 상속을 구현하는 프로토타입 객체를 가리킨다.
  - [[Prototype]] 내부 슬롯이 가리키는 프로토타입 객체에 접근하기 위한 접근자 프로퍼티이다.
  - 직접 접근은 불가능하므로 __proto__를 통해 간접적으로 접근한다.
- prototype 프로퍼티

  - constructor만이 소유하는 프로퍼티
  - 함수가 객체를 생성하는 생성자 함수로 호출될 때 생성자 함수가 생성할 인스턴스의 프로토타입 객체를 가리킨다.

  ```javascript
  // 함수 객체는 prototype 프로퍼티를 소유한다.
  (function ( {}).hasOwnProperty('prototype')); //true

  // 일반 객체는 prototype 프로퍼티를 소유하지 않는다.
  ({}.hasOwnProperty('prototype')); //false
  ```
