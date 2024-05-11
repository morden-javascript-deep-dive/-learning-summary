## 🌀16장 프로퍼티 어트리뷰트

## **16.1 내부 슬롯과 내부 메서드**

- 내부 슬롯 (Internal Slot)
    
    자바스크립트 엔진의 구현 알고리즘을 설명하기 위해 사용하는 의사 프로퍼티 (Pseudo Property)
    
- 내부 메서드 (Internal Method)
    
    자바스크립트 엔진의 구현 알고리즘을 설명하기 위해 사용하는 의사 메서드 (Pseudo Method)
    

ECMAScript 사양에 등장하는 **이중 대괄호 ( `[ [ … ] ]` )** 로 감싸져 있는 이름들

자바스크립트 엔진에서 실제로 동작은 하지만, 개발자가 직접적으로 접근 또는 호출 할 수 없다.

하지만 `[[Prototype]]` 내부 슬롯을 `__proto__` 프로퍼티를 통해 간접적으로 접근할 수 있다.

```tsx
const o = {};

// 내부 슬롯은 자바스크립트 엔진의 내부 로직이므로 직접 접근할 수 없다.
o.[[Prototype]] // -> Uncaught SyntaxError: Unexpected token '['
// 단, 일부 내부 슬롯과 내부 메서드에 한하여 간접적으로 접근할 수 있는 수단을 제공하기는 한다.
o.__proto__ // -> Object.prototype
```

## **16.2 프로퍼티 어트리뷰트와 프로퍼티 디스크립터 객체**

- 프로퍼티 어트리뷰트 (Property Attribute)
    - 자바스크립트 엔진이 프로퍼티를 생성할 때 기본값으로 자동 정의하며, 프로퍼티의 상태를 나타낸다.
    - 프로퍼티의 값 (Value), 값의 갱신 가능 여부 (Writable), 값의 열거 가능 여부 (Enumerable), 값의 재정의 가능 여부 (Configurable)
    - 자바스크립트 엔진이 관리하는 내부 상태 값인 내부 슬롯 `[[Value]], [[Writable]], [[Enumberable]], [[Configurable]]`
        - 직접 접근 불가
        - `Object.getOwnPropertyDescriptor` 메서드를 사용하여 간접적으로 확인할 수 있음

```tsx
const person = {
  name: 'Lee'
};

// 프로퍼티 동적 생성
person.age = 20;

// 프로퍼티 어트리뷰트 정보를 제공하는 프로퍼티 디스크립터 객체를 반환한다.
console.log(Object.getOwnPropertyDescriptor(person, 'name'));
// {value: "Lee", writable: true, enumerable: true, configurable: true}

// 모든 프로퍼티의 프로퍼티 어트리뷰트 정보를 제공하는 프로퍼티 디스크립터 객체들을 반환한다.
console.log(Object.getOwnPropertyDescriptors(person));
/*
{
  name: {value: "Lee", writable: true, enumerable: true, configurable: true},
  age: {value: 20, writable: true, enumerable: true, configurable: true}
}
*/
```

## **16.3 데이터 프로퍼티와 접근자 프로퍼티**

프로퍼티는 데이터 프로퍼티와 접근자 프로퍼티로 구분할 수 있다.

- 데이터 프로퍼티
    - 키와 값으로 구성된 일반적인 프로퍼티
- 접근자 프로퍼티
    - 다른 데이터 프로퍼티의 값을 읽거나 저장할 때 호출되는 접근자 함수 (Accessor Function)로 구성된 프로퍼티

### 16.3.1 데이터 프로퍼티

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/f7f68ca3-9747-436a-9960-99211015ef6a/3755bb00-95aa-4fd1-853e-3ce668401c9b/Untitled.png)

### 16.3.2 접근자 프로퍼티

자체적으로는 값을 갖지 않고 다른 데이터 프로퍼티의 값을 읽거나 저장할 때 사용하는 접근자 함수로 구성된 프로퍼티다.

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/f7f68ca3-9747-436a-9960-99211015ef6a/a0bb3471-7441-441a-bc4d-e4182d844714/Untitled.png)

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/f7f68ca3-9747-436a-9960-99211015ef6a/3df95d18-b028-4df4-a328-b45f862a9385/Untitled.png)

```tsx
const person = {
  // 데이터 프로퍼티
  firstName: 'Ungmo',
  lastName: 'Lee',

  // fullName은 접근자 함수로 구성된 접근자 프로퍼티다.
  // getter 함수
  get fullName() {
    return `${this.firstName} ${this.lastName}`;
  },
  // setter 함수
  set fullName(name) {
    // 배열 디스트럭처링 할당: "31.1 배열 디스트럭처링 할당" 참고
    [this.firstName, this.lastName] = name.split(' ');
  }
};

// 데이터 프로퍼티를 통한 프로퍼티 값의 참조.
console.log(person.firstName + ' ' + person.lastName); // Ungmo Lee

// 접근자 프로퍼티를 통한 프로퍼티 값의 저장
// 접근자 프로퍼티 fullName에 값을 저장하면 setter 함수가 호출된다.
person.fullName = 'Heegun Lee';
console.log(person); // {firstName: "Heegun", lastName: "Lee"}

// 접근자 프로퍼티를 통한 프로퍼티 값의 참조
// 접근자 프로퍼티 fullName에 접근하면 getter 함수가 호출된다.
console.log(person.fullName); // Heegun Lee

// firstName은 데이터 프로퍼티다.
// 데이터 프로퍼티는 [[Value]], [[Writable]], [[Enumerable]], [[Configurable]] 프로퍼티 어트리뷰트를 갖는다.
let descriptor = Object.getOwnPropertyDescriptor(person, 'firstName');
console.log(descriptor);
// {value: "Heegun", writable: true, enumerable: true, configurable: true}

// fullName은 접근자 프로퍼티다.
// 접근자 프로퍼티는 [[Get]], [[Set]], [[Enumerable]], [[Configurable]] 프로퍼티 어트리뷰트를 갖는다.
descriptor = Object.getOwnPropertyDescriptor(person, 'fullName');
console.log(descriptor);
// {get: ƒ, set: ƒ, enumerable: true, configurable: true}
```

- 접근자 프로퍼티의 동작 순서
    1. 전달 받는 프로퍼티 키의 유효성을 검사한다. 프로퍼티의 키는 String 또는 Symbol 이어야 한다.
    2. 프로토타입 체인 (Prototype Chain)을 통해 프로퍼티를 검색한다.
    3. 검색된 프로퍼티가 데이터 프로퍼티인지 접근자 프로퍼티인지 확인한다.
        
        ```tsx
        // 일반 객체의 __proto__는 접근자 프로퍼티다.
        Object.getOwnPropertyDescriptor(Object.prototype, '__proto__');
        // {get: ƒ, set: ƒ, enumerable: false, configurable: true}
        
        // 함수 객체의 prototype은 데이터 프로퍼티다.
        Object.getOwnPropertyDescriptor(function() {}, 'prototype');
        // {value: {...}, writable: true, enumerable: false, configurable: false}
        ```
        
    4. 접근자 프로퍼티일 경우, getter 또는 setter 함수를 반환한다.

## **16.4 프로퍼티 정의**

- 프로퍼티 정의
    
    새로운 프로퍼티를 추가하면서 프로퍼티 어트리뷰트를 새로 정의하거나 재정의 하는 행위
    
    - 프로퍼티의 값을 갱신 가능 여부
    - 프로퍼티의 열거 가능 여부
    - 프로퍼티의 재정의 가능 여부 등…
    
- `Object.defineProperty` 메서드를 사용하면 프로퍼티의 어트리뷰트를 정의할 수 있다.

```tsx
const person = {};

// 1. 데이터 프로퍼티 정의
Object.defineProperty(person, 'firstName', {
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
// firstName {value: "Ungmo", writable: true, enumerable: true, configurable: true}

// 디스크립터 객체의 프로퍼티를 누락시키면 undefined, false가 기본값이다.
descriptor = Object.getOwnPropertyDescriptor(person, 'lastName');
console.log('lastName', descriptor);
// lastName {value: "Lee", writable: false, enumerable: false, configurable: false}

// [[Enumerable]]의 값이 false인 경우
// 해당 프로퍼티는 for...in 문이나 Object.keys 등으로 열거할 수 없다.
// lastName 프로퍼티는 [[Enumerable]]의 값이 false이므로 열거되지 않는다.
console.log(Object.keys(person)); // ["firstName"]

// [[Writable]]의 값이 false인 경우 해당 프로퍼티의 [[Value]]의 값을 변경할 수 없다.
// lastName 프로퍼티는 [[Writable]]의 값이 false이므로 값을 변경할 수 없다.
// 이때 값을 변경하면 에러는 발생하지 않고 무시된다.
person.lastName = 'Kim';

// [[Configurable]]의 값이 false인 경우 해당 프로퍼티를 삭제할 수 없다.
// lastName 프로퍼티는 [[Configurable]]의 값이 false이므로 삭제할 수 없다.
// 이때 프로퍼티를 삭제하면 에러는 발생하지 않고 무시된다.
delete person.lastName;

// [[Configurable]]의 값이 false인 경우 해당 프로퍼티를 재정의할 수 없다.
// Object.defineProperty(person, 'lastName', { enumerable: true });
// Uncaught TypeError: Cannot redefine property: lastName

descriptor = Object.getOwnPropertyDescriptor(person, 'lastName');
console.log('lastName', descriptor);
// lastName {value: "Lee", writable: false, enumerable: false, configurable: false}

// 2. 접근자 프로퍼티 정의
Object.defineProperty(person, 'fullName', {
  // getter 함수
  get() {
    return `${this.firstName} ${this.lastName}`;
  },
  // setter 함수
  set(name) {
    [this.firstName, this.lastName] = name.split(' ');
  },
  enumerable: true,
  configurable: true
});

descriptor = Object.getOwnPropertyDescriptor(person, 'fullName');
console.log('fullName', descriptor);
// fullName {get: ƒ, set: ƒ, enumerable: true, configurable: true}

person.fullName = 'Heegun Lee';
console.log(person); // {firstName: "Heegun", lastName: "Lee"}
```

- `Object.defineProperties`를 통해 여러개의 프로퍼티를 한 번에 정의할 수 있다.

```tsx
const person = {};

Object.defineProperties(person, {
  // 데이터 프로퍼티 정의
  firstName: {
    value: 'Ungmo',
    writable: true,
    enumerable: true,
    configurable: true
  },
  lastName: {
    value: 'Lee',
    writable: true,
    enumerable: true,
    configurable: true
  },
  // 접근자 프로퍼티 정의
  fullName: {
    // getter 함수
    get() {
      return `${this.firstName} ${this.lastName}`;
    },
    // setter 함수
    set(name) {
      [this.firstName, this.lastName] = name.split(' ');
    },
    enumerable: true,
    configurable: true
  }
});

person.fullName = 'Heegun Lee';
console.log(person); // {firstName: "Heegun", lastName: "Lee"}
```

## **16.5 객체 변경 방지**

기본적으로 객체는 변경 가능한 값이므로 재할당 없이 직접 변경할 수 있다.

- 프로퍼티 추가, 삭제, 갱신 등
- Object.defineProperty, Object.defineProperties 등

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/f7f68ca3-9747-436a-9960-99211015ef6a/c2f3d523-50f4-44fa-af53-c57ed643b0cd/Untitled.png)

### 16.5.1 객체 확장 금지

**`Object.preventExtensions`**

- 프로퍼티의 추가가 금지된다.

`Object.isExtensible()`를 통해 확장 금지 여부를 알 수 있다.

```tsx
const person = { name: 'Lee' };

// person 객체는 확장이 금지된 객체가 아니다.
console.log(Object.isExtensible(person)); // true

// person 객체의 확장을 금지하여 프로퍼티 추가를 금지한다.
Object.preventExtensions(person);

// person 객체는 확장이 금지된 객체다.
console.log(Object.isExtensible(person)); // false

// 프로퍼티 추가가 금지된다.
person.age = 20; // 무시. strict mode에서는 에러
console.log(person); // {name: "Lee"}

// 프로퍼티 추가는 금지되지만 삭제는 가능하다.
delete person.name;
console.log(person); // {}

// 프로퍼티 정의에 의한 프로퍼티 추가도 금지된다.
Object.defineProperty(person, 'age', { value: 20 });
// TypeError: Cannot define property age, object is not extensible
```

### 16.5.2 객체 밀봉

**`Object.seal`**

- 프로퍼티의 읽기와 갱신만 가능하다.
- 프로퍼티 추가, 삭제, 재정의가 금지된다.

`Object.isSealed()`를 통해 밀봉 여부를 알 수 있다.

```tsx
const person = { name: 'Lee' };

// person 객체는 밀봉(seal)된 객체가 아니다.
console.log(Object.isSealed(person)); // false

// person 객체를 밀봉(seal)하여 프로퍼티 추가, 삭제, 재정의를 금지한다.
Object.seal(person);

// person 객체는 밀봉(seal)된 객체다.
console.log(Object.isSealed(person)); // true

// 밀봉(seal)된 객체는 configurable이 false다.
console.log(Object.getOwnPropertyDescriptors(person));
/*
{
  name: {value: "Lee", writable: true, enumerable: true, configurable: false},
}
*/

// 프로퍼티 추가가 금지된다.
person.age = 20; // 무시. strict mode에서는 에러
console.log(person); // {name: "Lee"}

// 프로퍼티 삭제가 금지된다.
delete person.name; // 무시. strict mode에서는 에러
console.log(person); // {name: "Lee"}

// 프로퍼티 값 갱신은 가능하다.
person.name = 'Kim';
console.log(person); // {name: "Kim"}

// 프로퍼티 어트리뷰트 재정의가 금지된다.
Object.defineProperty(person, 'name', { configurable: true });
// TypeError: Cannot redefine property: name
```

### 16.5.3 객체 동결

**`Object.freeze`**

프로퍼티의 읽기만 가능하다.

- 프로퍼티 추가, 삭제, 재정의, 갱신이 금지된다.

`Object.isFrozen()`를 통해 동결 여부를 알 수 있다.

```tsx
const person = { name: 'Lee' };

// person 객체는 동결(freeze)된 객체가 아니다.
console.log(Object.isFrozen(person)); // false

// person 객체를 동결(freeze)하여 프로퍼티 추가, 삭제, 재정의, 쓰기를 금지한다.
Object.freeze(person);

// person 객체는 동결(freeze)된 객체다.
console.log(Object.isFrozen(person)); // true

// 동결(freeze)된 객체는 writable과 configurable이 false다.
console.log(Object.getOwnPropertyDescriptors(person));
/*
{
  name: {value: "Lee", writable: false, enumerable: true, configurable: false},
}
*/

// 프로퍼티 추가가 금지된다.
person.age = 20; // 무시. strict mode에서는 에러
console.log(person); // {name: "Lee"}

// 프로퍼티 삭제가 금지된다.
delete person.name; // 무시. strict mode에서는 에러
console.log(person); // {name: "Lee"}

// 프로퍼티 값 갱신이 금지된다.
person.name = 'Kim'; // 무시. strict mode에서는 에러
console.log(person); // {name: "Lee"}

// 프로퍼티 어트리뷰트 재정의가 금지된다.
Object.defineProperty(person, 'name', { configurable: true });
// TypeError: Cannot redefine property: name
```

### 16.5.4 불변 객체

앞서 살펴본 메서드는 **얕은 변경 방지 (Shallow Only) 방식**이다.

상위의 프로퍼티만 변경이 방지되고, 그 안에 있는 **자식 객체는 영향을 받지 못한다.**

중첩된 객체까지 모두 불변 객체를 만드려면, 재귀적으로 `Object.freeze` 메서드를 호출해야 한다.

```tsx
function deepFreeze(target) {
  // 객체가 아니거나 동결된 객체는 무시하고 객체이고 동결되지 않은 객체만 동결한다.
  if (target && typeof target === 'object' && !Object.isFrozen(target)) {
    Object.freeze(target);
    /*
      모든 프로퍼티를 순회하며 재귀적으로 동결한다.
      Object.keys 메서드는 객체 자신의 열거 가능한 프로퍼티 키를 배열로 반환한다.
      ("19.15.2. Object.keys/values/entries 메서드" 참고)
      forEach 메서드는 배열을 순회하며 배열의 각 요소에 대하여 콜백 함수를 실행한다.
      ("27.9.2. Array.prototype.forEach" 참고)
    */
    Object.keys(target).forEach(key => deepFreeze(target[key]));
  }
  return target;
}

const person = {
  name: 'Lee',
  address: { city: 'Seoul' }
};

// 깊은 객체 동결
deepFreeze(person);

console.log(Object.isFrozen(person)); // true
// 중첩 객체까지 동결한다.
console.log(Object.isFrozen(person.address)); // true

person.address.city = 'Busan';
console.log(person); // {name: "Lee", address: {city: "Seoul"}}
```

### 🌀 17장 생성자 함수에 의한 객체 생성

## **17.1 Object 생성자 함수**

객체 생성 방식 - 생성자 함수를 사용하여 객체 생성

## **17.2 생성자 함수**

`new` 연산자와 `Object` 생성자 함수 사용 시 빈 객체 생성

자바스크립트는 Object 생성자 함수 이외에도 `String, Number, Boolean, Function, Array, Date, RegExp, Promise` 등의 빌트인 생성자 함수를 제공

```tsx
// 빈 객체의 생성 (인스턴스)
const person = new Object();

// 프로퍼티 추가
person.name = 'Lee';
person.sayHello = function () {
  console.log('Hi! My name is ' + this.name);
};

console.log(person); // {name: "Lee", sayHello: ƒ}
person.sayHello(); // Hi! My name is Lee
```

### 17.2.1 객체 리터럴에 의한 객체 생성 방식의 문제점

단 하나의 객체만 생성하기 때문에, **객체 재사용이 불가능**

동일한 프로퍼티를 갖는 객체를 여러 개 생성할 경우 비효율적이다.

```tsx
// 프로퍼티 값은 다를 수 있지만, 일반적으로 메서드는 동일하다.
const circle1 = {
  radius: 5,
  getDiameter() {
    return 2 * this.radius;
  }
};

console.log(circle1.getDiameter()); // 10

const circle2 = {
  radius: 10,
  getDiameter() {
    return 2 * this.radius;
  }
};

console.log(circle2.getDiameter()); // 20
```

### 17.2.2 생성자 함수에 의한 객체 생성 방식의 장점

프로퍼티 구조가 동일한 객체 여러 개를 간편하게 생성 가능.

```tsx
// 생성자 함수
function Circle(radius) {
  // 생성자 함수 내부의 this는 생성자 함수가 생성할 인스턴스를 가리킨다.
  this.radius = radius;
  this.getDiameter = function () {
    return 2 * this.radius;
  };
}

// 인스턴스의 생성
const circle1 = new Circle(5);  // 반지름이 5인 Circle 객체를 생성
const circle2 = new Circle(10); // 반지름이 10인 Circle 객체를 생성

console.log(circle1.getDiameter()); // 10
console.log(circle2.getDiameter()); // 20
```

일반 함수와 동일한 방법으로 정의하고, `new 연산자` 와 함께 호출하면 해당 함수는 생성자 함수로 동작

`new 연산자` 없이 사용하면 일반 함수로 동작

```tsx
// new 연산자와 함께 호출하지 않으면 생성자 함수로 동작하지 않는다.
// 즉, 일반 함수로서 호출된다.
const circle3 = Circle(15);

// 일반 함수로서 호출된 Circle은 반환문이 없으므로 암묵적으로 undefined를 반환한다.
console.log(circle3); // undefined

// 일반 함수로서 호출된 Circle내의 this는 전역 객체를 가리킨다.
console.log(radius); // 15
```

### 17.2.3 생성자 함수의 인스턴스 생성 과정

자바스크립트 엔진은 암묵적인 처리를 통해 인스턴스를 생성하고 반환한다.

- 인스턴스 생성과 this 바인딩
    - 암묵적으로 빈 객체 생성되며, 이 빈 객체(인스턴스)는 this에 바인딩된다.
    - 런타임 이전에 실행
- 인스턴스 초기화
    - 인스턴스에 프로퍼티, 메서드를 추가
    - 전달받은 매개변수로 프로퍼티의 초기값을 할당
- 인스턴스 반환
    - 생성자함수 내부에서 모든 처리가 끝나면 완성된 인스턴스가 바인딩된 this를 암묵적으로 반환한다.  (**return 값을 지정하지 않을 경우**)
    - **명시적으로 특정 객체를 반환할 경우,** this 반환이 무시되고 해당 객체를 반환.
    
    ```tsx
    function Circle(radius) {
      // 1. 암묵적으로 빈 객체가 생성되고 this에 바인딩된다.
      console.log(this); // Circle {}
      
     // 2. this에 바인딩되어 있는 인스턴스를 초기화한다.
      this.radius = radius;
      this.getDiameter = function ()   {
        return 2 * this.radius;
      };
      
      // 3. 완성된 인스턴스가 바인딩된 this가 암묵적으로 반환된다
    }
    
    const circle1 = new Circle(5);
    ```
    

### 17.2.4 내부 메서드 [[Call]]과 [[Construct]]

- 일반 객체 : 호출 불가능
- 함수 : 호출 가능
    - 일반 객체가 갖고 있는 내부 슬롯, 내부 메서드.
    - 함수 객체만을 위한 `[[Environment]]`, `[[FormalParameters]]` 등의 내부 슬롯
    - `[[Call]]`, `[[Construct]]` 등의 내부 메서드

- `[[Call]]`
    - 함수가 일반 함수로서 호출될 때 호출되는 내부 메서드
- `[[Construct]]`
    - 함수가 new 연산자와 함께 생성자 함수로 호출될 때 호출되는 내부 메서드

```tsx
function foo() {}

// 일반적인 함수로서 호출: [[Call]]이 호출된다.
foo();

// 생성자 함수로서 호출: [[Construct]]가 호출된다.
new foo();
```

- callable, constructor, non-constructor
    - **callable**
        - `[[Call]]`을 갖는 함수 객체
        - 호출할 수 있는 객체 즉, 함수
    - **constructor**
        - `[[Construct]]`를 갖는 함수 객체
        - 생성자 함수로서 호출할 수 있는 함수
    - **non-constructor**
        - `[[Constructor]]`를 갖지 않는 함수 객체
        - 생성자 함수로서 호출할 수 없는 함수

함수 객체는 호출할 수 있지만, 모든 함수 객체를 생성자 함수로서 호출할 수 있지는 않다.

### 17.2.5 constructor와 non-constructor의 구분

- **constructor**

함수 선언문, 함수 표현식, 클래스

- **non-constructor**

메서드 (ES6 메서드 축약 표현), 화살표 함수

```tsx
// 일반 함수 정의: 함수 선언문, 함수 표현식
function foo() {}
const bar = function () {};
// 프로퍼티 x의 값으로 할당된 것은 일반 함수로 정의된 함수다. 이는 메서드로 인정하지 않는다.
const baz = {
  x: function () {}
};

// 일반 함수로 정의된 함수만이 constructor이다.
new foo();   // -> foo {}
new bar();   // -> bar {}
new baz.x(); // -> x {}

// 화살표 함수 정의
const arrow = () => {};

new arrow(); // TypeError: arrow is not a constructor

// 메서드 정의: ES6의 메서드 축약 표현만을 메서드로 인정한다.
const obj = {
  x() {}
};

new obj.x(); // TypeError: obj.x is not a constructor
```

### 17.2.6 new 연산자

- new 연산자와 함께 호출되는 함수는 생성자 함수로 동작 (`[[Construct]] 호출`)
    - 즉, new 연산자와 함께 호출되는 함수는 constructor 이어야만 한다.
- new 연산자 없이 호출되는 함수는 일반 함수로 동작 (`[[Call]] 호출`)

```tsx
// 생성자 함수
function Circle(radius) {
  this.radius = radius;
  this.getDiameter = function () {
    return 2 * this.radius;
  };
}

// new 연산자 없이 생성자 함수 호출하면 일반 함수로서 호출된다.
const circle = Circle(5);
console.log(circle); // undefined

// 일반 함수 내부의 this는 전역 객체 window를 가리킨다.
console.log(radius); // 5
console.log(getDiameter()); // 10

circle.getDiameter();
// TypeError: Cannot read property 'getDiameter' of undefined
```

일반 함수와 생성자 함수는 특별한 차이가 없다.

보통 생성자 함수를 파스칼 케이스로 명명하여 일반 함수와 구별한다.

### 17.2.7 new.target

**new.target**

생성자 함수가 new 연산자없이 호출되는 것을방지하기 위해 파스칼 케이스 컨벤션을 사용한다고 하더라도, 실수를 발생할 수 있어  ES6에서 도입.

this와 유사하게 constructor인 모든 함수 내부에서 암묵적인 지역 변수와 같이 사용되며 메타 프로퍼티라 부른다.

`new 연산자`와 함께 ****생성자 함수로서 호출되었는지 여부를 반환

```tsx
// 생성자 함수
function Circle(radius) {
  // 이 함수가 new 연산자와 함께 호출되지 않았다면 new.target은 undefined다.
  if (!new.target) {
    // new 연산자와 함께 생성자 함수를 재귀 호출하여 생성된 인스턴스를 반환한다.
    return new Circle(radius);
  }

  this.radius = radius;
  this.getDiameter = function () {
    return 2 * this.radius;
  };
}

// new 연산자 없이 생성자 함수를 호출하여도 new.target을 통해 생성자 함수로서 호출된다.
const circle = Circle(5);
console.log(circle.getDiameter());
```

- 빌트인 생성자 함수

대부분의 빌트인 생성자 함수 (String, Number, Boolean, Function, Array, Date, RegExp, Promise 등)는 new 연산자와 함께 호출되었는지 여부를 확인하여 적절한 값을 반환

- `Object`, `Function` new 연산자 없이 호출해도, 생성자 함수로서 동일하게 동작.

```tsx
let obj = new Object();
console.log(obj); // {}

obj = Object();
console.log(obj); // {}

let f = new Function('x', 'return x ** x');
console.log(f); // ƒ anonymous(x) { return x ** x }

f = Function('x', 'return x ** x');
console.log(f); // ƒ anonymous(x) { return x ** x }
```

- `String`, `Number`, `Boolean` new 연산자가 있으면 **객체 반환.** new 연산자가 없으면 **원시 타입 값 반환.**
    
    ```tsx
    const str = String(123);
    console.log(str, typeof str); // 123 string
    
    const num = Number('123');
    console.log(num, typeof num); // 123 number
    
    const bool = Boolean('true');
    console.log(bool, typeof bool); // true boolean
    ```
    

### 🌀 18장 함수와 일급 객체

## **18.1 일급 객체**

- 일급 객체의 조건
    - 무명의 리터럴로 생성 가능. 즉, 런타임에서 생성이 가능
    - 변수나 자료 구조에 저장이 가능
    - 함수의 매개 변수에 전달이 가능
    - 함수의 반환값으로 사용이 가능

**자바스크립트의 함수는 위의 조건을 모두 만족하므로 일급 객체이다.** (자바스크립트의 함수형 프로그래밍이 가능해진다.)

- 함수와 일반 객체의 차이
    - 일반 객체는 호출할 수 없지만 함수는 호출이 가능하다.
    - 함수 객체는 일반 객체에는 없는 함수 고유의 프로퍼티를 소유한다.

## **18.2 함수 객체의 프로퍼티**

```tsx
function square(number) {
  return number * number;
}

console.log(Object.getOwnPropertyDescriptors(square));
/*
{
  length: {value: 1, writable: false, enumerable: false, configurable: true},
  name: {value: "square", writable: false, enumerable: false, configurable: true},
  arguments: {value: null, writable: false, enumerable: false, configurable: false},
  caller: {value: null, writable: false, enumerable: false, configurable: false},
  prototype: {value: {...}, writable: true, enumerable: false, configurable: false}
}
*/

// __proto__는 square 함수의 프로퍼티가 아니다.
console.log(Object.getOwnPropertyDescriptor(square, '__proto__')); // undefined

// __proto__는 Object.prototype 객체의 접근자 프로퍼티다.
// square 함수는 Object.prototype 객체로부터 __proto__ 접근자 프로퍼티를 상속받는다.
console.log(Object.getOwnPropertyDescriptor(Object.prototype, '__proto__'));
// {get: ƒ, set: ƒ, enumerable: false, configurable: true}
```

 

### 18.2.1 arguments 프로퍼티

함수 호출 시, 전달된 인수들의 정보를 담고 있는 순회 가능한 유사 배열 객체

함수 내부에서 지역 변수처럼 사용되며, 함수 외부에서는 참조가 불가능하다.

```tsx
function multiply(x, y) {
  console.log(arguments);
  return x * y;
}

console.log(multiply());        // NaN
console.log(multiply(1));       // NaN
console.log(multiply(1, 2));    // 2
console.log(multiply(1, 2, 3)); // 2
```

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/f7f68ca3-9747-436a-9960-99211015ef6a/ba8bfcd9-7d9b-4e89-97a8-9642230eae40/Untitled.png)

- callee 프로퍼티 : 호출되어 arguments 객체를 생성한 함수. (함수 자기 자신)
- length 프로퍼티 : 인수의 개수
- Symbol (Symbol.iterator) 프로퍼티 : arguments 객체를 순회 가능한 자료구조인 이터러블로 만들기 위한 프로퍼티
    
    ```css
    function multiply(x, y) {
      // 이터레이터
      const iterator = arguments[Symbol.iterator]();
    
      // 이터레이터의 next 메서드를 호출하여 이터러블 객체 arguments를 순회
      console.log(iterator.next()); // {value: 1, done: false}
      console.log(iterator.next()); // {value: 2, done: false}
      console.log(iterator.next()); // {value: 3, done: false}
      console.log(iterator.next()); // {value: undefined, done: true}
    
      return x * y;
    }
    
    multiply(1, 2, 3);
    ```
    

arguments 객체는 매개 변수를 확정할 수 없는 **가변 인자 함수** 구현에 유용 `arguments.length`
but!! arguments 객체는 유사 배열 객체이므로 배열 메서드를 사용할 경우 에러 발생(배열 메서드 사용하려면 Function.prototype.call, Function.prototype.apply 를 사용해 간접 호출해야함)

```tsx
// ES6 Rest parameter
function sum(...args) {
  return args.reduce((pre, cur) => pre + cur, 0);
}

console.log(sum(1, 2));          // 3
console.log(sum(1, 2, 3, 4, 5)); // 15
```

### 18.2.2 caller 프로퍼티

함수 객체의 caller 프로퍼티는 함수 자신을 호출한 함수를 가리킨다. 

ECMAScript 사양에 포함되지 않는 비표준 프로퍼티

```tsx
function foo(func) {
  return func();
}

function bar() {
  return 'caller : ' + bar.caller;
}

// 브라우저에서의 실행한 결과
console.log(foo(bar)); // caller : function foo(func) {...}
console.log(bar());    // caller : null
```

### 18.2.3 length 프로퍼티

함수를 정의할 때 선언한 매개변수의 개수

```tsx
function foo() {}
console.log(foo.length); // 0

function bar(x) {
  return x;
}
console.log(bar.length); // 1

function baz(x, y) {
  return x * y;
}
console.log(baz.length); // 2
```

arguments 객체의 length 프로퍼티 : 인자 (argument)의 개수.

함수 객체의 length 프로퍼티 : 매개변수 (parameter)의 개수.

### 18.2.4 name 프로퍼티

함수 이름을 나타내며 ES6부터 정식 표준이 되었다.

ES5와 ES6에서 동작을 달리한다는 점에 주의가 필요하다.

ES5에서는 빈 문자열을 값으로 갖는 반면, ES6에서는 함수 객체를 가리키는 식별자를 값으로 갖는다.

```tsx
// 기명 함수 표현식
var namedFunc = function foo() {};
console.log(namedFunc.name); // foo

// 익명 함수 표현식
var anonymousFunc = function() {};
// ES5: name 프로퍼티는 빈 문자열을 값으로 갖는다.
// ES6: name 프로퍼티는 함수 객체를 가리키는 변수 이름을 값으로 갖는다.
console.log(anonymousFunc.name); // anonymousFunc

// 함수 선언문(Function declaration)
function bar() {}
console.log(bar.name); // bar
```

### 18.2.5 `__proto__` 접근자 프로퍼티

`[[Prototype]]` 내부 슬롯이 가리키는 프로토타입 객체에 접근할 수 있는 접근자 프로퍼티

```tsx
const obj = { a: 1 };

// 객체 리터럴 방식으로 생성한 객체의 프로토타입 객체는 Object.prototype이다.
console.log(obj.__proto__ === Object.prototype); // true

// 객체 리터럴 방식으로 생성한 객체는 프로토타입 객체인 Object.prototype의 프로퍼티를 상속받는다.
// hasOwnProperty 메서드는 Object.prototype의 메서드다.
console.log(obj.hasOwnProperty('a'));         // true
console.log(obj.hasOwnProperty('__proto__')); // false
```

### 18.2.6 prototype 프로퍼티

생성자 함수로 호출할 수 있는 constructor 만 소유할 수 있는 프로퍼티

반대로 non-constructor 에는 존재하지 않는다.

```tsx
// 함수 객체는 prototype 프로퍼티를 소유한다.
(function () {}).hasOwnProperty('prototype'); // -> true

// 일반 객체는 prototype 프로퍼티를 소유하지 않는다.
({}).hasOwnProperty('prototype'); // -> false
```
