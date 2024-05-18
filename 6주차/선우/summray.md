# 19장 프로토타입

자바스크립트의 특징: 

명령형, 함수형, 프로토타입 기반, 객체지향 프로그래밍등

<aside>
☝🏻 ES6의 클래스 도입
- 기존의 프로토타입 기반 객체지향 모델의 sugar 같은 역할
- 클래스 VS 생성자 함수 프로토타입 기반의 인스턴스 생성
- 공통점: 클래스와 생성자 함수 프로토타입 기반의 인스턴스 생성
- 차이점: 정확히 동일하게 동작하지 않음. 클래스는 생성자 함수보다 엄격하여 클래스는 생성자 함수에서 제공하지 않는 기능도 제공

</aside>

자바스크립트는 객체 기반, 원시 타입의 값을 제외한 나머지 값들(함수, 배열, 정규 표현식 등)은 모두 객체이다.

## 19.1 객체지향 프로그래밍

정의

- 명령형 프로그래밍의 절차지향적 관점과 대비되는, 객체의 집합으로 프로그램을 표현하는 프로그래밍 패러다임

속성

- 실체의 특징이나 성질을 나타내는 속성

추상화란?

- 다양한 속성 중에서 프로그램에 필요한 속성만 간추려 내어 표현하는 것

객체란?

- 속성을 통해 여러 개의 값을 하나의 단위로 구성한 복합적인 자료구조
- 상태 데이터(프로퍼티)와 동작(메서드)을 하나의 논리적인 단위로 묶은 복합적인 자료구조

## 19.2 상속과 프로토타입

상속이란?

- 어떤 객체의 프로퍼티 또는 메서드를 다른 객체가 상속받아 그대로 사용할 수 있는 것

중복을 제거하는 방법

- 프로토타입을 기반으로 상속을 구현해 불필요한 중복을 제거
- 상속을 통해 불필요한 중복 코드 제거하는 방법
    
    ```c
    function Circle(radius) {
    	this.raadius = radius;
    }
    
    Circle.prototype.getArea = function () {
    	return Math.PI * this.radius ** 2;
    };
    
    const circle1 = new Circle(1);
    const circle2 = new Circle(2);
    
    console.log(circle1.getArea == circle2.getArea);
    
    console.log(circle1.getArea());  // 3.14159...
    console.log(circle2.getArea());  // 12.5663...
    ```
    

## 19.3 프로토타입 객체

- 프로토타입 객체란 객체지향 프로그래밍의 근간을 이루는 객체 간 상속을 구현하기 위해 사용됨
- 프로토타입은 어떤 객체의 상위(부모) 객체의 역할을 하는 객체로서 다른 객체에 공유 프로퍼티(메서드 포함)을 제공함
- 모든 객체는 [[prototype]] 이라는 내부 슬롯 값을 가지며 이 내부 슬롯의 값은 프로토타입의 참조이다.(내부 슬롯의 값이 null인 경우도 있음)
- 모든 객체는 하나의 프로토타입을 갖음 + 모든 프로토타입은 생성자 함수와 연결되어 있음
    
    ![스크린샷 2024-05-16 오후 7.51.43.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/b278eae5-15fb-42de-a493-229718a0b327/d40881c2-f552-49be-bf6f-296c5a66ea8f/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2024-05-16_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_7.51.43.png)
    
- [[proto]] 내부 슬롯에 직접 접근 불가능하지만, __proto__ 접근자 프로퍼티를 통해 자신의 프로토타입 간접 접근 가능
    - 프로토 타입은) 자신의 constructor 프로퍼티를 통해 생성자 함수에 접근 가능
    - 생성자 함수는) 자신의 prototype 프로퍼티를 통해 프로토타입에 접근 가능

### 19.3.1 __proto__ 접근자 프로퍼티

- 모든 객체는 __proto __ 접근자 프로퍼티를 통해 자신의 프로토타입, 즉 [[prototype]] 내부 슬롯에 간접적으로 접근 가능
- 생성자 함수가 생성한 모든 인스턴스는 자신의 프로토타입, 즉 상위(부모) 객체 역할을 하는 Circle.prototype의 모든 프로퍼티와 메서드를 상속받음
- 자신의 상태를 나타내는 radius 프로퍼티만 개별적으로 소유하고 동일한 메서드는 상속을 통해 공유하여 사용하는 것
- 상속은 코드의 재사용이란 관점에서 유용

`[[Get]]`, `[[Set]]` 프로퍼티 어트리뷰트로 구성된 프로퍼티

- 내부 슬롯은 프로퍼티가 아니다 → 프로토타입에 접근할 수 없음
- 접근자 프로퍼티는 자체적으로 값을 갖지 않고 다른 데이터 프로퍼티의 값을 읽거나 저장할 때 사용하는 접근자 함수, 즉 [[Get]], [[Set]] 프로퍼티 어트리뷰트로 구성된 프로퍼티임
- Object.prototype의 접근자 프로퍼티인 __proto __는 getter/setter 함수라고 부르는 접근자 함수([[Get]], [[Set]] 프로퍼티 어트리큐트에 할당된 함수)를 통해 [[prototype]] 내부 슬롯의 값, 즉 프로토타입을 취득하거나 할당함.
    - __proto__에 접근하면 내부적으로 __proto__ 접근자 프로퍼티의 getter 함수인 [[Get]]이 호출된다.
    - __proto__ 접근자 프로퍼티를 통해 새로운 프로토타입을 할당하면 __proto__ 접근자 프로퍼티의 setter 함수인 [[Set]]이 호출된다
        
        ```c
        const obj = {};
        const parent = { x : 1};
        
        // getter 함수인 get __proto__ 가 호출되어 obj 객체의 프로토타입을 취득
        obj.__proto__;
        
        //setter 함수인 set __proto__가 호출되어 obj 객체의 프로토타입을 교체
        obj.__proto__ = parent;
        
        console.log(obj.x);  // 1
        ```
        

`Object.prototype`

- 프로토타입의 체인의 종점(최상위 객체)는 `Object.prototype`이다.
- 이 객체의 프로퍼티와 메서드는 모든 객체에 상속됨

**proto 접근자 프로퍼티를 통해 프로토타입에 접근하는 이유**

- 상호 참조에 의해 프로토타입 체인이 생성 방지를 위해서 접근자 프로퍼티 사용

```jsx
const parent = {};
const child = {};

child.__proto__ = parent;
parent.__proto__ = child; // TypeError
```

**proto 접근자 프로퍼티를 코드 내에서 직접 사용하는 것은 권장하지 않는다.**

- 모든 객체가 `__proto__` 접근자 프로퍼티를 사용할 수 없기 때문에, `__proto__` 접근자 프로퍼티 직접 사용은 권장하지 않음

```jsx
const obj = Object.create(null); // 상속받을 수 없음

console.log(obj.__proto__); // undefined

console.log(obj.getPrototypeOf(obj)); // null
```

### 19.3.2 함수 객체의 prototype 프로퍼티

- 함수 객체만이 소유하는 prototype 프로퍼티는 생성자 함수가 생성할 인스턴스의 프로토타입을 가리킴

```jsx
(function () {}).hasOwnProperty('prototype'); // true

({}).hasOwnProperty('prototype'); // false

```

- 화살표 함수(non-constructor), ES6 메서드 축약 표현으로 정의한 메서드는 prototype 프로퍼티를 소유하지 않으며, 프로토타입 생성하지 않음
- 모든 객체가 가지고 있는 `__proto__` 접근자 프로퍼티와 함수 객체만이 가지고 있는 prototype 프로퍼티는 프로퍼티를 사용하는 주체는 다르나, 결국 동일한 프로토타입을 가리킴

| 구분 | 소유 | 값 | 사용 주체 | 사용 목적 |
| --- | --- | --- | --- | --- |
| __proto__ 접근자 프로퍼티 | 모든 객체 | 프로토타입의 참조 | 모든 객체 | 객체가 자신의 프로토타입에 접근 또는 교체하기 위해 사용 |
| prototype 프로퍼티 | constructor | 프로토타입의 참조 | 생성자 함수 | 생성자 함수가 자신이 생성할 객체(인스턴스)의 프로토타입을 할당하기 위해 사용 |

```jsx
function Person(name) { // 생성자 함수
	this.name = name;
}

const me = new Person('Lee');

console.log(Person.prototype === me.__proto__); // true

```

### 19.3.3 프로토타입의 constructor 프로퍼티와 생성자 함수

- 모든 프로토타입은 constructor 프로퍼티를 가짐
- constructor 프로퍼티는 prototype 프로퍼티로 자신이 참조하고 있는 생성자 함수를 가리킴

```jsx
function Person(name) { // 생성자 함수
	this.name = name;
}

const me = new Person('Lee');

console.log(me.constructor === Person); // true

```

## 19.4 라터럴 표기법에 의해 생성된 객체의 생성자 힘수와 프로토타입<a name="19.4"></a>

```jsx
const obj = new Object();
console.log(obj.constructor === Object); // true

const add = new Function('a', 'b', 'return a + b');
console.log(add.constructor === Function); // true

function Person(name) {
	this.name = name;
}

const me = new Person('Lee');
console.log(me.constructor === Person); // true

```

```jsx
// 리터럴 표기법을 이용해 인스턴스를 생성하지 않는 객체 생성 방식

const obj = {}; // 객체 리터럴

const add = function (a, b) { return a + b; }; // 함수 리터럴

const arr = [1, 2, 3]; // 배열 리터럴

const regexp = /is/ig; // 정규 표현식 리터럴

```

```jsx
const obj = {};

console.log(obj.constructor === Object); // true

```

- 추상 연산
    - ECMAScript 사양에서 내부 동작의 구현 알고리즘을 표현한 것
- 프로토타입과 생성자 함수는 단독으로 존재할 수 없고, 쌍으로만 존재함

| 리터럴 표기법 | 생성자 함수 | 프로토타입 |
| --- | --- | --- |
| 객체 리터럴 | Object | Object.prototype |
| 함수 리터럴 | Function | Function.prototype |
| 배열 리터럴 | Array | Array.prototype |
| 정규 표현식 리터럴 | RegExp | RegExp.prototype |

## 19.5 프로토타입의 생성 시점<a name="19.5"></a>

- 객체는 리터럴 표기법 또는 생성자 함수에 의해 성성되므로 모든 객체는 생성자 함수와 연결되어 있음
- Object.create 메서드와 클래스에 의한 객체 생성
    - Object.create 메서드와 클래스로 객체를 생성하는 방법 존재
- 프로토타입은 생성자 함수가 생성되는 시점에 생성됨
    - 프로토타입과 생성자 함수는 단독으로 존재할 수 없으며, 쌍으로 존재하기 때문
- 생성자 함수는 사용자가 직접 정의한 사용자 정의 생성자 함수와 자바스크립트가 기본 제공하는 빌트인 생성자 함수로 구분

### 19.5.1 사용자 정의 생성자 함수와 프로토타입 생성 시점

- 생성자 함수로서 호출할 수 있는 함수(constructor)는 함수 정의가 평가되어 함수 객체 생성 시점에 프로토타입도 함께 생성됨

```jsx
console.log(Person.prototype); // {constructor: f}

function Person(name) {
	this.name = name;
}

```

```jsx
// 화살표 함수는 non-constructor
const Person = (name) => {
	this.name = name;
};

console.log(Person.prototype); // undefined

```

- 사용자 정의 생성자 함수의 프로토타입은 함수 객체로 생성되는 시점에 생성되며, 생성된 프로토타입의 프로토타입은 언제나 `Object.prototype`

### 19.5.2 빌트인 생성자 함수와 프로토타입 생성 시점

- 모든 빌트인 생성자 함수는 전역 객체가 생성되는 시점에 생성되어, 객체가 생성되기 이전에 생성자 함수와 프로토타입은 이미 객체화되어 존재함
- 생성자 함수 또는 리터럴 표기법으로 객체를 생성하면 프로토타입은 생성된 객체의 `[[Prototype]]` 내부 슬롯에 할당됨

## 19.6 객체 생성 방식과 프로토타입의 결정<a name="19.6"></a>

- 객체는 다양한 생성 방법이 존재하나, 추상 연산 OrdinaryObjectCreate에 의해 생성된다는 공통점이 존재함
- 추상 연산 OrdinaryObjectCreate는 필수적으로 자신이 생성할 객체의 프로토타입을 인수로 전달받으며, 자신이 생성할 객체에 추가할 프로퍼티 목록을 옵션으로 전달 가능

### 19.6.1 객체 리터럴에 의해 생성된 객체의 프로토타입

- 객체 리터럴을 평가하여 객체를 생성할 때 추상 연산 `OrdinaryObjectCreate`를 호출하며, 추상 연산자에 전달되는 프로토타입은 `Object.prototype`이다.(=객체 리터럴에 생성되는 객체의 프로토타입은 `Object.prototype`이다.)

```jsx
const obj = { x: 1 };

// 객체 리터럴에 의해 생성된 obj 객체는 Object.prototype을 상속받음
console.log(obj.constructor === Object); // true
console.log(obj.hasOwnProperty('x')); // true

```

### 19.6.2 Object 생성자 함수에 의해 생성된 객체의 프로토타입

- Ojbect 생성자 함수를 인수 없이 호출하면 빈 객체가 생성되고, Object 생성자 함수를 호출하면 추상 연산 `OrdinaryObjectCreate` 호출되어서, 추상 연산 `OrdinaryObjectCreate`에 전달되는 프로토타입은 `Object.prototype`이다.(=Object 생성자 함수에 의해 생성되는 객체의 프로토타입은 `Object.prototype`이다.)

```jsx
const obj = new Object();
obj.x = 1;

// Object 생성자 함수에 의해 생성된 obj 객체는 Object.prototype을 상속받음
console.log(obj.constructor === Object); // true
console.log(obj.hasOwnProperty('x')); // true

```

### 19.6.3 생성자 함수에 의해 생성된 객체의 프로토타입

- new 연산자와 함께 생성자 함수를 호출하여 인스턴스를 생성하면 다른 객체 생성 방식과 마찬가지로 추상 연산 `OrdinaryObjectCreate` 호출되며, 추상 연산에 전달되는 프로토타입은 생성자 함수의 `prototype` 프로퍼티에 바인딩되어 있는 객체이다.(=생성자 함수에 의해 생성되는 객체의 프 로토타입은 생성자 함수의 prototype 프로퍼티에 바인딩되어 있는 객체다.)

```jsx
function Person(name) {
	this.name = name;
}

const me = new Person('Lee');

```

```jsx
function Person(name) {
	this.name = name;
}

// 프로토타입 메서드
Person.prototype.sayHello = function () {
	console.log(`Hi! My name is ${this.name}`);
};

const me = new Person('Lee');
const you = new Person('Kim');

me.sayHello(); // Hi! My name is Lee
you.sayHello(); // Hi! My name is Kim

```

## 19.7 프로토타입 체인

- 자바스크립트는 객체의 프로퍼티(메서드 포함)에 접근하려고 할 때 해당 객체에 접근하려는 프로퍼티가 없다면 `[[Prototype]]` 내부 슬롯의 참조에 따라 부모 역할을 하는 프로토타입의 프로퍼티를 순차적으로 검색하는 것
- 자바스크립트가 객체지향 프로그래밍의 상속을 구현하는 메커니즘이며, 상속과 프로퍼티 검색을 위한 메커니즘
- call 메서드
    - this로 사용할 객체를 전달하면서 함수 호출함
- 프로토타입 체인의 최상위에 위치하는 객체는 언제나 `Object.prototype`이다. `Object.prototype`을 프로토타입 체인의 종점이라 한다.
- 모든 객체는 `Object.prototype`을 상속받음
- 프로토타입 체인의 종점인 `Object.prototype`에서 프로퍼티를 검색할 수 없는 경우 `undefined` 반환
- 식별자는 스코프 체인에서 검색하는데, 이는 자바스크립트 엔진이 함수의 중첩 관계로 이루어진 스코프의 계층적 구조에서 식별자를 검색하는 것이다.
    - 스코프 체인은 식별자 검색을 위한 메커니즘

```jsx
me.hasOwnProperty('name');

```

## 19.8 오버라이딩과 프로퍼티 새도잉<a name="19.8"></a>

- 상속 관계에 의해 프로퍼티가 가려지는 현상
- 오버라이딩: 상위 클래스가 가지고 있는 메서드를 하위 클래스가 재정의하여 사용하는 방식
- 오버로딩: 함수 이름은 동일하지만 매개변수의 타입 또는 개수가 다른 메서드를 구현하고 매개변수에 의해 메서드를 구별하여 호출하는 방식을 말하며, `arguments` 객체를 사용하여 구현 가능

```jsx
// 프로토타입 메서드 변경
Person.prototype.sayHello = function () {
	console.log(`Hey! My name is ${this.name}`);
};

me.sayHello(); // Hey! My name is Lee

// 프로토타입 메서드 삭제
delete Person.prototype.sayHello;
me.sayHello; // TypeError

```

## 19.9 프로토타입의 교체<a name="19.9"></a>

- 프로토타입은 임의의 다른 객체로 변경 가능하며, 생성자 함수 또는 인스턴스에 의해 교체 가능함

### 19.9.1 생성자 함수에 의한 프로토타입의 교체

```jsx
const Person = (function () {
	function Person(name) {
		this.name = name;
	}

	// 생성자 함수의 prototype 프로퍼티를 통해 프로토타입을 교체
	Person.prototype = {
		sayHello() {
			console.log(`Hi! My name is ${this.name}`);
		}
	};

	return Person;
}());

const me = new Person('Lee');

console.log(me.constructor === Person); // false
console.log(me.constructor === Object); // ture

```

- 프로토타입을 교체하면 constructor 프로퍼티와 생성자 함수 간의 연결이 파괴됨

```jsx
const Person = (function () {
	function Person(name) {
		this.name = name;
	}

	// 생성자 함수의 prototype 프로퍼티를 통해 프로토타입을 교체
	Person.prototype = {
		// constructor 프로퍼티와 생성자 함수 간의 연결 설정
		constructor: Person,
		sayHello() {
			console.log(`Hi! My name is ${this.name}`);
		}
	};

	return Person;
}());

const me = new Person('Lee');

// constructor 프로퍼티가 생성자 함수를 가리킴
console.log(me.constructor === Person); // true
console.log(me.constructor === Object); // false

```

### 19.9.2 인스턴스에 의한 프로토타입의 교체

- 프로토타입은 생성자 함수의 prototype 프로퍼티뿐만 아니라, 인스턴스의 `__proto__` 접근자 프로퍼티(or `Object.getPrototypeOf` 메서드)를 통해 접근 가능해서, 이를 이용해 프로토타입 교체 가능
- 생성자 함수의 prototype 프로퍼티에 다른 임의의 객체를 바인딩하는 것은 미래에 생성할 인스턴스의 프로토타입을 교체하는 것
- `__proto__` 접근자 프로퍼티를 통해 프로토타입을 교체하는 것은 이미 생성된 객체의 프로토타입을 교체하는 것

```jsx
function Person(name) {
	this.name = name;
}

const me = new Person('Lee');

// 프로토타입으로 교체할 객체
const parent = {
	sayHello() {
		console.log(`Hi! My name is ${this.name}`);
	}
};

// me 객체의 프로토타입을 parent 객체로 교체
Object.setPrototypeOf(me, parent);
// 위 코드는 아래의 코드와 동일하게 동작
// me.__proto__ = parent;
me.sayHello(); // Hi! My name is Lee

// 프로토타입 교체로 constructor 프로퍼티와 생성자 함수 간의 연결 파괴
console.log(me.constructor === Person); // false
// 프로토타입 체인을 따라 Object.prototype의 constructor 프로퍼티가 검색됨
console.log(me.constructor === Object); // true

```

## 19.10 instanceof 연산자

- 이항 연산자로, 좌변에서 객체를 가리키는 식별자와 우변에 생성자 함수를 가리키는 식별자를 피연산자로 받음. 우변의 피연산자가 함수가 아닌 경우 TypeError 발생

```jsx
객체 instanceof 생성자 함수

```

- 우변의 생성자 함수의 prototype에 바인딩된 객체가 좌변의 객체의 프로토타입 체인 상에 존재하면 true로 평가되고, 그렇지 않은 경우 false로 평가됨

```jsx
// 생성자 함수
function Person(name) {
	this.name = name;
}

const me = new Person('Lee');

// 프로토타입으로 교체할 객체
const parent = {};

// 프로토타입의 교체
Object.setPrototypeOf(me, parent);

// Person 생성자 함수와 parent 객체는 연결되어 있지 않음
console.log(Person.prototype === parent); // false
consoel.log(Person.constructor === Person); // false

// parent 객체를 Person 생성자 함수의 prototype 프로퍼티에 바인딩
Person.prototype = parent;

// Person.prototype이 me 객체의 프로토타입 체인 상에 존재하므로 true로 평가됨
console.log(me instanceof Person); // true

// Object.prototype이 me 객체의 프로토타입 체인 상에 존재하므로 true로 평가됨
console.log(me instanceof Object); // true

```

- instanceof 연산자는 생성자 함수의 prototype에 바인딩된 객체가 프로토타입 체인 상에 존재하는지 확인함

```jsx
// instanceof 연산자를 함수로 표현하면 아래와 같음
function isInstanceof(instance, constructor) {
	// 프로토타입 취득
	const prototype = Object.getPrototypeOf(instance);

	// 재귀 탈출 조건
	// prototype이 null이면 프로토타입 체인의 종점에 다다른 것
	if (prototype === null) return false;
	return prototype === constructor.prototype || isInstanceof(prototype, constructor);
}

console.log(isInstanceof(me, Person)); // true
console.log(isInstanceof(me, Object)); // true
console.log(isInstanceof(me, Array)); // false

```

- 생성자 함수에 의해 프로토타입이 교체되어 constructor 프로퍼티와 생성자 함수 간의 연결이 파괴되어도, 생성자 함수의 prototype 프로퍼티와 프로토타입 간의 연결은 파괴되지 않으므로 instanceof는 아무런 영향을 받지 않음

```jsx
const Person = (function () {
	function Person(name) {
		this.name = name;
	}

	// 생성자 함수 prototype 프로퍼티를 통해 프로토타입 교체
	Person.prototype = {
		sayHello() {
			console.log(`Hi! My name is ${this.name}`);
		}
	};

	return Person;
}());

const me = new Person('Lee');

// constructor 프로퍼티와 생성자 함수 간의 연결이 파괴되어도 instanceof는 아무런 영향을 받지 않음
console.log(me.constructor === Person); // false

// Person.prototype이 me 객체의 프로토타입 체인 상에 존재
console.log(me instanceof Person); // true
// Object.prototype이 me 객체의 프로토타입 체인 상에 존재
console.log(me instanceof Object); // true

```

## 19.11 직접 상속<a name="19.11"></a>

### 19.11.1 Object.create에 의한 상속

- Object.create 메서드는 명시적으로 프로토타입을 지정하여 새로운 객체 생성
    - 추상 연산 OrdinaryObjectCreate 호출함

```jsx
/**
	* 지정된 프로토타입 및 프로퍼티를 갖는 새로운 객체를 생성하여 반환하
	* @param {Object} prototype - 생성할 객체의 프로토타입으로 지정할 객체
	* @param {Object} [propertiesObject] - 생성할 객체의 프로퍼티를 갖는 객체
	* @returns {Object} 지정된 프로토타입 및 프로퍼티를 갖는 새로운 객체
	*/
Object.create(prototype[, propertiesObject])

```

```jsx
// 프로토타입이 null인 객체 생성 // 생성된 객체는 프로토타입 체인의 종점에 위치
// obj -> null
let obj = Object.create(null);
console.log(Object.getPrototypeOf(obj) === null); // true
console.log(obj.toString()); // TypeError // Object.prototype을 상속 받지 못함

// obj -> Object.prototype -> null
// obj -> {};와 동일
obj = Object.create(Object.prototype);
console.log(Object.getPrototypeOf(obj) === Object.prototype); // true

// obj -> Object.prototype -> null
// obj = { x: 1 };와 동일
obj = Object.create(Object.prototype, {
	x: { value: 1, writable: true, enumerable: true, configurable: true }
});
// 위 코드는 아래와 동일
// obj = Object.create(Object.prototype);
// obj.x = 1;
console.log(obj.x); // 1
console.log(Object.getPrototypeOf(obj) === Object.prototype); // true

const myProto = { x: 10 };
// 임의의 객체를 직접 상속 받음
// obj -> myProto -> Object.prototype -> null
obj = Object.craete(myProto);
console.log(obj.x); // 10
console.log(Object.getPrototypeOf(obj) === myProto); // true

// 생성자 함수
function Person(name) {
	this.name = name;
}

// obj -> Person.prototype -> Object.prototype -> null
// obj -> new Person('Lee')와 동일
obj = Object.craete(Person.prototype);
obj.name = 'Lee';
console.log(obj.name); // Lee
console.log(Object.getPrototypeOf(obj) === Person.prototype); // true

```

- `Object.create` 메서드는 첫 번째 매개변수에 전달한 객체의 프로토타입 체인에 속하는 객체를 생성하면서 직접적으로 상속을 구현함.
- `Object.create` 메서드의 장점
    - new 연산자가 없어도 객체 생성 가능
    - 프로토타입을 지정하면서 객체 생성 가능
    - 객체 리터럴에 의해 생성된 객체 상속 가능

```jsx
const obj = { a: 1 };

obj.hasOwnProperty('a'); // true
obj.propertyIsEnumerable('a'); // true

```

- `Object.create` 메서드를 통해 프로토타입 체인의 종점에 위치하는 객체를 생성할 수 있기 때문에, `Object.prototype`의 빌트인 메서드를 객체가 직접 호출하는 것을 권장하지 않음
- 프로토타입 체인의 종점에 위치하는 객체는 `Object.prototype`의 빌트인 메서드를 사용할 수 없음

```jsx
// 프로토타입이 null인 객체, 즉 프로토타입 체인의 종점에 위치하는 객체 생성
const obj = Object.create(null);
obj.a = 1;

console.log(Object.getPrototypeOf(obj) === null); // true

// obj는 Object.prototype의 빌트인 메서드 사용 불가
console.log(obj.hasOwnProperty('a')); // TypeError

```

- `Object.prototype`의 빌트인 메서드는 간접 호출하는 것이 좋음

```jsx
// 프로토타입이 null인 객체 생성
const obj = Object.create(null);
obj.a = 1;

// console.log(obj.hasOwnProperty('a')); // TypeError

// Object.prototype의 빌트인 메서드는 객체로 직접 호출하지 않음
console.log(Object.prototype.hasOwnProperty.call(obj, 'a')); // true

```

### 19.11.2 객체 리터럴 내부에서 __proto__에 의한 직접 상속

```jsx
const myProto = { x: 10 };

// 객체 리터럴에 의해 객체를 생성하면서 프로토타입을 지정해 직접 상속 가능
const obj = {
	y: 20,
	// 객체 직접 상속받음
	// obj -> myProto -> Object.prototype -> null
	__proto__: myProto
};
/* 위 코드는 아래와 동일함
const obj = Object.crate(myProto, {
	y: { value: 20, writable: true, enumerable: true, configurable: true }
});
*/

console.log(obj.x, obj.y); // 10 20
console.log(Object.getPrototypeOf(obj) === myProto ); // true

```

## 19.12 정적 프로퍼티/메서드

- 생성자 함수로 인스턴스를 생성하지 않아도 참조/호출할 수 있는 프로퍼티/메서드
- 메서드 내에서 인스턴스를 참조할 필요가 없다면 정적 메서드로 변경해도 동작함
- 프로토타입 메서드를 호출하려면 인스턴스를 생성해야 하지만 정적 메서드는 인스턴스를 생성하지 않아도 호출 가능

```jsx
function Foo() {}

// 프로토타입 메서드
// this를 참조하지 않은 프로토타입 메서드는 정적 메서드로 변경하여도 동일한 효과를 얻을 수 있음
Foo.prototype.x = fuction () {
	console.log('x');
};

const foo = new Foo();
// 프로토타입 메서드를 호출하려면 인스턴스를 생성해야 함
foo.x(); // x

// 정적 메서드
Foo.x = fuction () {
	console.log('x');
};

// 정적 메서드는 인스턴스를 생성하지 않아도 호출 가능
Foo.x(); // x

```

- 프로토타입/메서드 표기시, prototype을 #으로 표기하는 경우도 존재
    - ex) Object.prototype.isPrototypeOf === Object#isPrototypeOf

## 19.13 프로퍼티 존재 확인<a name="19.13"></a>

### 19.13.1 in 연산자

- 객체 내에 특정 프로퍼티 존재 여부 확인

```jsx
/**
	* key: 프로퍼티 키를 나타내는 문자열
	* object: 객체로 평가되는 표현식
	*/
key in object

```

- ES6에서 도입된 Reflect.has 메서드는 in 연산자와 동일하게 동작

```jsx
const person = { name: 'Lee' };

console.log('name' in person); // true
console.log('toString' in person); // true

console.log(Reflect.has(person, 'name'); // true
console.log(Reflect.has(person, 'toString'));

```

### 19.13.2 Object.prototype.hasOwnProperty 메서드

- 객체에 특정 프로퍼티가 존재하는지 확인

```jsx
console.log(person.hasOwnProperty('name')); // true

```

- 프로퍼티 키가 객체 고유의 프로퍼티 키인 경우에만 true 반환하고, 상속받은 프로토타입의 프로퍼티 키인 경우 false 반환

```jsx
console.log(person.hasOwnProperty('toString')); // false

```

## 19.14 프로퍼티 열거<a name="19.14"></a>

### 19.14.1 for...in 문

- 객체의 모든 프로퍼티를 순회하며 열거

```jsx
for (변수 선언문 in 객체) { ...}

```

```jsx
const person = {
	name = 'Lee',
	address = 'Seoul'
};

for (const key in person) {
	console.log(key + ': ' + person[key]);
}
// name: Lee
// address: Seoul

```

- 객체의 프로토타입 체인 상에 존재하는 모든 프로토타입의 프로퍼티 중에서 프로퍼티 어트리뷰터 [[Enumerable]]의 값이 true인 프로퍼티를 순회하며 열거함
- 프로퍼티 열거 시 순서를 보장하지 않으나, 대부분의 모던 브라우저는 순서를 보장하고 숫자인 프로퍼티 키에 대해서는 정렬을 실시함
- 배열에는 for...in문 대신 일반적인 for문이나 for...of문, Array.prototype.forEach 메서드 사용 권장

### 19.14.2 Object.keys/values/entries 메서드

- 객체의 고유 프로퍼티만 열거하기 위해서는 for...in문 대신에 Object.keys/values/entries 메서드 사용 권장
- Object.keys 메서드는 객체 자신의 열거 가능한 프로퍼티 키를 배열로 반환함

```jsx
const person = {
	name: 'Lee',
	address: 'Seoul',
	__proto__: { age: 20 }
};

console.log(Object.keys(person)); // ["name", "address"]

```

- ES8에서 도입된 Object.entries 메서드는 객체 자신의 열거 가능한 프로퍼티 값과 값의 쌍의 배열을 배열에 담아 반환

```jsx
console.log(Object.entries(person)); // [["name", "address"], ["address", "Seoul"]]

Object.entries(person).forEach([key, value] => console.log(key, value));
/*
name Lee
address Seoul
*/

```
