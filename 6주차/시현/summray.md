# 6주차

## 🌀 19장 프로토타입

자바스크립트 : 프로토타입 기반의 **객체지향** 프로그래밍 언어

### 19.1 객체지향 프로그래밍

다양한 속성 중에서 프로그램에 필요한 속성만 간추려 내어 표현하는 것 → **추상화**

객체는 상태 데이터(→프로퍼티)와 동작(→메서드)을 하나의 논리적인 단위로 묶은 복합적인 자료구조라고 할 수 있다.

### 19.2 상속과 프로토타입

```jsx
function Circle(radius) {
  this.radius = radius;
  this.getArea = function () {
    return Math.PI * this.radius ** 2;
  };
}

const circle1 = new Circle(1);
const circle2 = new Circle(2);

console.log(circle1.getArea === circle2.getArea);	// false
console.log(circle1.getArea()); // 3.14159...
console.log(circle2.getArea());	// 12.56637...
```

Circle 생성자 함수는 인스턴스를 생성할 때마다 `getArea` 메서드를 중복 생성하고 모든 인스턴스가 중복 소유한다. → 메모리 불필요한 낭비 및 성능 악영향

위의 코드를 상속을 통해 필요한 중복 제거하면?

```jsx
function Circle(radius) {
  this.radius = radius;
}
// 모든 인스턴스가 getArea 메서드를 사용할 수 있도록 프로토타입에 추가한다.
// 프로토타입은 Circle 생성자 함수의 prototype 프로퍼티에 바인딩되어 있다.
Circle.prototype.getArea = function () {
  return Math.PI * this.radius ** 2;
};

const circle1 = new Circle(1);
const circle2 = new Circle(2);
// Circle 생성자 함수가 생성하는 모든 인스턴스는 하나의 getArea 메서드를 공유한다.
console.log(circle1.getArea === circle2.getArea);	// true
console.log(circle1.getArea()); // 3.14159...
console.log(circle2.getArea());	// 12.56637...
```

Circle 생성자 함수가 생성한 모든 인스턴스는 **자신의 프로토타입, 즉 상위(부모)** 객체 역할을 하는 `Circle.prototype`의 모든 프로퍼티와 메서드를 상속받는다.

### 19.3 프로토타입 객체

모든 객체는 `[[Prototype]]` 이라는 내부 슬롯을 가지며, 이 내부 슬롯의 값은 프로토타입의 참조다.

모든 객체는 **하나의 프로토타입**을 갖는다. 그리고 모든 프로토타입은 **생성자 함수**와 연결되어 있다.

- `__proto__` 접근자 프로퍼티
    
    모든 객체는 __proto__ 접근자 프로퍼티를 통해 자신의 프로토타입, 즉 `[[Prototype]]` 내부 슬롯에 간접적으로 접근할 수 있다.
    
    - **__proto__ 는 접근자 프로퍼티다.**
        
        접근자 프로퍼티는 자체적으로는 값을 갖지 않고 다른 데이터 프로퍼티의 값을 읽거나 저장할 때 사용하는 접근자 함수, 즉 `[[Get]], [[Set]]` 프로퍼티 어트리뷰트로 구성된 프로퍼티다.
        
        ```jsx
        const obj = {};
        const parent = { x: 1 };
        
        // get __proto__가 호출되어 obj 객체의 프로토타입을 취득
        obj.__proto__;
        // set __proto__가 호출되어 obj 객체의 프로토타입을 교체
        obj.__proto__ = parent;
        
        console.log(obj.x);	// 1
        ```
        
    - **__proto__ 접근자 프로퍼티는 상속을 통해 사용된다.**
        
        `__proto__` 접근자 프로퍼티는 객체가 직접 소유하는 프로퍼티가 아니라 `Object.prototype`의 프로퍼티 이다. 모든 객체는 상속을 통해 `Object.prototype.__proto__` 접근자 프로퍼티를 사용할 수 있다.
        
    - __proto__ 접근자 프로퍼티를 통해 프로토타입에 접근하는 이유
        
        상호 참조에 의해 프로토타입 체인이 생성되는 것을 방지하기 위함
        
        ```jsx
        const parert = {};
        const child = {};
        
        // child의 프로토타입을 parent로 설정
        child.__proto__ = parent;
        // parent 프로토타입을 child로 설정
        parent.__proto__ = child;	// TypeError: Cyclic __proto__ value
        ```
        
        → 서로가 자신의 프로토타입이 되는 비정상적인 프로토타입 체인이 만들어 지므로 에러를 발생시킨다.
        
    - `__proto__` 접근자 프로퍼티를 코드 내에서 직접 사용하는 것은 권장하지 않는다.
        
        모든 객체가 `__proto__` 접근자 프로퍼티를 사용할 수 있는 것은 아니기 때문! (직접 상속을 통해 `Object.prototype`을 상속받지 않는 객체를 생성할 수도 있기 때문)
        
        따라서 프로토타입의 참조를 취득하고 싶을 경우 `Object.getPrototypeOf` 메서드를 사용하고, 프로토타입을 교체하고 싶을 경우 `Object.setPrototypeOf` 메서드를 사용할 것을 권장한다.
        
        ```jsx
        const obj = {};
        const parent = {x: 1};
        // ES5에서 도입된 메서드, get Object.prototype.__proto__와 처리내용 동일
        Object.getPrototypeOf(obj);				// obj.__proto__;
        // ES6에서 도입된 메서드, set Object.prototype.__proto__와 처리내용 동일
        Object.setPrototypeOf(obj, parent);		// obj.__proto__ = parent;
        ```
        
- 함수 객체의 prototype 프로퍼티

함수 객체만의 소유하는 prototype 프로퍼티는 **생성자 함수**가 생성할 인스턴스의 프로토타입을 가리킨다.

생성자 함수로서 호출할 수 없는 함수는 prototype 프로퍼티를 소유하지 않으며 프로토타입도 생성하지 않는다.(화살표 함수, ES6 메서드 축약 표현)

생성자 함수로 호출하기 위해 정의하지 않은 일반 함수도 prototype 프로퍼티를 소유하지만 객체를 생성하지 않는 일반 함수의 prototype 프로퍼티는 아무런 의미가 없다.

모든 객체가 가지고 있는 __proto__ 접근자 프로퍼티와 함수 객체만이 가지고 있는 prototype 프로퍼티는 결국 동일한 프로토타입을 가리킨다. 하지만 이들이 프로퍼티를 사용하는 주체가 다르다.

| 구분 | 소유 | 값 | 사용 주체 | 사용 목적 |
| --- | --- | --- | --- | --- |
| __proto__ 접근자 프로퍼티 | 모든 객체 | 프로토타입의 참조 | 모든 객체 | 객체가 자신의 프로토타입에 접근 또는 교체하기 위해 사용 |
| prototype 프로퍼티 | constructor | 프로토타입의 참조 | 생성자 함수 | 생성자 함수가 자신이 생성할 객체(인스턴스)의 프로토타입을 할당하기 위해 사용 |

```jsx
// 생성자 함수
function Person(name) {
	this.name = name;
}

const me = new Person('Lee');

console.log(Person.prototype === me.__proto__); // true
```

- 프로토타입의 contructor 프로퍼티와 생성자 함수

모든 프로토타입은 constructor 프로퍼티를 갖고 이 contructor 프로퍼티는 prototype 프로퍼티로 자신을 참조하고 있는 생성자 함수를 가리킨다.

위의 예제 참고) Person 생성자 함수는 me 객체를 생성 → 이때 me 객체는 프로토타입의 contructor 프로퍼티를 통해 생성자 함수와 연결 → me 객체에는 contructor 프로퍼티가 없지만 me 객체의 프로토타입인 Person.prototype 에는 contructor 프로퍼티가 있다. → 따라서 me 객체는 프로토타입인 Person.prototype의 constructor 프로퍼티를 상속받아 사용할 수 있다.

### 19.4 리터럴 표기법에 의해 생성된 객체의 생성자 함수와 프로토타입

 리터럴 표기법에 의한 객체 생성 방식과 같이 명시적으로 new 연산자와 함께 생성자 함수를 호출하여 인스턴스를 생성하지 않는 객체 생성 방식도 있다.

리터럴 표기법에 의해 생성된 객체도 물론 프로토타입이 존재한다. 하지만 리터럴 표기법에 의해 생성된 객체의 경우 **프로토타입의 constructor 프로퍼티가 가리키는 생성자 함수가 반드시 객체를 생성한 생성자 함수라고 단정할 수 없다.**

리터럴 표기법에 의해 생성된 객체도 상속을 위해 프로토타입이 필요하다. 따라서 리터럴 표기법에 의해 생성된 객체도 **가상적인 생성자 함수**를 갖는다. 프로토타입은 생성자 함수와 더불어 생성되며 prototype, constructor 프로퍼티에 의해 연결되어 있기 떄문이다. **다시 말해, 프로토타입과 생성자 함수는 단독으로 존재할 수 없고 언제나 쌍으로 존재한다.**

### 19.5 프로토타입의 생성 시점

프로토타입은 생성자 함수가 생성되는 시점에 더불어 생성된다.

생성자 함수

- 사용자 정의 생성자 함수와 프로토타입 생성 시점

자신이 평가되어 함수 객체로 생성되는 시점에 프로토타입도 더불어 생성되며, 생성된 프로토타입의 프로토타입은 언제나 `Object.prototype` 이다.

- 빌트인 생성자 함수와 프로토타입 생성 시점

빌트인 생성자 함수도 일반 함수와 마찬가지로 빌트인 함수가 생성되는 시점에 프로토타입이 생성된다. 모든 빌트인 생성자 함수는 전역 객체가 생성되는 시점에 생성된다. 생성된 프로토타입은 빌트인 생성자 함수의 prototype 프로퍼티에 바인딩된다.

이처럼 객체가 생성되기 이전에 **생성자 함수와 프로토타입은 이미 객체화되어 존재**한다. 이후 생성자 함수 또는 리터럴 표기법으로 객체를 생성하면 **프로토타입은 생성된 객체의 [[Prototype]] 내부 슬롯에 할당**된다. 

19.6 객체 생성 방식과 프로토타입의 결정

다양한 객체 생성 방법이 있으나… → **추상 연산**에 의해 생성된다는 공통점!

프로토타입은 추상연산에 **전달되는 인수**에 의해 결정된다. 이 인수는 객체가 생성되는 시점에 객체 생성 방식에 의해 결정된다.

- 객체 리터럴에 의해 생성된 객체의 프로토타입

```jsx
const obj = { x: 1 };

// 객체 리터럴에 의해 생성된 obj 객체는 Object.prototype을 상속받는다.
console.log(obj.constructor === Object); // true
console.log(obj.hasOwnProperty('x')); // true
```

- Object 생성자 함수에 의해 생성된 객체의 프로토타입

Object 생성자 함수를 호출하면 객체 리터럴과 마찬가지로 추상연산 호출 →  추상 연산에 전달되는 프로토타입은 Object.prototype 이다. (객체 리터럴에 의해 생성된 객체와 동일한 구조를 가짐!) 

둘의 차이는 프로퍼티를 추가하는 방식에 있다. (객체 리터럴 방식은 객체 리터럴 내부에 프로퍼티 추가, Object 생성자 함수 방식은 일단 빈 객체 생성한 이후 프로퍼티 추가)

- 생성자 함수에 의해 생성된 객체의 프로토타입

추상연산에 전달되는 프로토타입은 **생성자 함수의 prototype 프로퍼티에 바인딩 되어 있는 객체**이다. 

### 19.7 프로토타입 체인

자바스크립트는 객체의 프로퍼티에 접근하려고 할 때 해당 **객체에 접근하려는 프로퍼티가없다면 [[Prototype]] 내부 슬롯의 참조를 따라 자신의 부모 역할을 하는 프로토타입의 프로퍼티를 순차적으로 검색**한다. → **프로토 타입체인**  

이는 자바스크립트가 객체 지향 프로그래밍의 상속을 구현하는 메커니즘이다.

프로토타입 체인의 최상위에 위치하는 객체는 언제나 Object.prototype이다. (따라서 모든 객체는 Object.prototype을 상속받는다.)

프로퍼티가 아닌 식별자는 스코프 체인에서 검색한다. 따라서 스코프 체인은 식별자 검색을 위한 메커니즘이라고 할 수 있다.

### 19.8 오버라이딩과 프로퍼티 섀도잉

인스턴스 메서드는 프로토타입 메서드를 오버라이딩! 이 과정에서 프로토타입 메서드가 가려지는 현상을 프로퍼티 섀도잉이라 한다.

하위 객체를 통해 프로토타입의 프로퍼티를 변경/삭제하는 것을 불가능하다. (get은 허용, set은 허용 X)

프로토타입 체인으로 접근할 것이 아니라 프로토타입에 직접 접근해야함

```jsx
Person.prototype.sayHello = function () {
  console.log('Hey! my name is ${this.name}');
}

delete me.sayHello;
// 인스턴스에 sayHello 메서드가 없으므로 프로토타입 메서드가 호출됨.
me.sayHello();

delete Person.prototype.sayHello;
// TypeError - 프로퍼티에 직접 접근해서 삭제했기 때문
me.sayHello();
```

### 19.9 프로토타입의 교체

- 생성자 함수에 의한 프로토타입의 교체

프로토타입을 교체하면 constructor 프로퍼티와 생성자 함수 간의 연결이 파괴된다.

```jsx
const Person = (fuction () {
	function Person(name) {
		this.name = name;
		}
		
		Person.prototype = {
			constructor: Person,
			sayHello() {
				console.log('Hi! My name is ${this.name}');
			}
		};
		
		return Person;
}());

const me = new Person('Lee');

console.log(me.constructor === Person); // true
```

- 인스턴스에 의한 프로토타입의 교체

마찬가지로 프로토타입으로 교체한 객체에  constructor 프로퍼티가 없다면 contructor 프로퍼티와 생성자 함수 간의 연결이 파괴된다.

```jsx
function Person(name) {
	this.name = name;
}

const me = new Person('Lee');

// 프로토타입으로 교체할 객체
const parent = {
	constructor: Person,
	sayHello() {
		console.log('Hi! My name is ${this.name}');
	}
}

// 생성자 함수의 prototype 프로퍼티와 프로토타입 간의 연결을 설정
Person.prototype = parent;

// me 객체의 프로토타입을 parent 객체로 교체한다.
Object.setPrototypeOf(me, parent);
```

→ 번거로움.. 상속 관계를 인위적으로 설정하려면 직접 상속 or 클래스 사용이 더 편리하고 안전

### 19.10 instanceof 연산자

우변의 생성자 함수의 prototype에 바인딩된 객체가 좌변의 객체의 프로토타입 체인 상에 존재하는가?

### 19.11 직접 상속

**Object.create에 의한 직접 상속**

명시적으로 프로토타입을 지정하여 새로운 객체 생성

```jsx
const obj = Object.create(Person.prototype);
obj.name = 'Lee';
```

첫 번째 매개변수에 전달한 객체의 프로토타입 체인에 속하는 객체 생성 

→ new 연산자 없이도 객체 생성 가능

→ 프로토타입을 지정하면서 객체 생성 가능

→ 객체 리터럴에 의해 생성된 객체도 상속받을 수 있다.ㄷㄷ

하지만, 해당 메서드를 통해 프로토타입 체인의 종점에 위치하는 객체를 생성할 수 있어, Object.prototype의 빌트인 메서드를 객체가 직접 호출하는 것을 권장하지 않음

**객체 리터럴 내부에서 proto에 의한 직접 상속**

Object.create 메서드에 의한 직접 상속에서 프로퍼티를 정의하는 과정이 번거로움

ES6에서는 객체 리터럴 내부에서 **proto** 접근자 프로퍼티를 사용하여 직접 상속을 구현할 수 있다.

```jsx
const myProto = {x:10};

const obj = {
y: 20,
__proto__ : myProto // 객체를 직접 상속받는다.
}

console.log(obj.x, obj.y) // 10 20
```

### 19.12 정적 프로퍼티/메서드

생성자 함수로 인스턴스를 생성하지 않아도 참조/호출할 수 있는 프로퍼티/메서드를 말한다.

인스턴스의 프로토타입 체인에 속한 객체의 프로퍼티/메서드가아 아니므로 인스턴스로 접근할 수 없다.

### 19.13 프로퍼티 존재 확인

**in 연산자**

특정 프로퍼티가 존재하는지 여부 확인

`key in object`

확인 대상 객체의 프로퍼티 뿐 아니라 확인 대상 객체가 상속받은 모든 프로토타입의 프로퍼티를 확인한다는 점을 주의!

ES6에서 도입된 `Reflect.has` 메서드도 in과 동일한 역할을 한다.

**Object.prototype.hasOwnProperty**

인수로 전달받은 프로퍼티 키가 객체 고유의 프로퍼티 키인 경우에만 true를 반환한다.

### 19.14 프로퍼티 열거

**for…in문**

`for (변수선언문 in 객체) {...}`

객체의 프로토타입 체인 상에 존재하는 모든 프로토타입의 프로퍼티 중에서 프로퍼티 어트리뷰트 `[[Enumerable]]`의 값이 true인 프로퍼티를 순회하며 열거한다.

따라서, 배열도 객체이므로 프로퍼티와 상속받은 프로퍼티가 포함될 수 있으므로 `for…in` 문 보다 `for…of`문, `Array.prototype.forEach` 메서드를 사용하기를 권장

**Object.keys/values/entries 메서드**

객체자신의 고유 프로퍼티만을 열거하기 위해 권장됨

`Object.keys` : 객체 자신의 열거 가능한 프로퍼티 키를 배열로 반환

`Object.values` : 객체 자신의 열거 가능한 프로퍼티 값을 배열로 반환   

`Object.entries` : 객체 자신의 열거 가능한 프로퍼티 키와 값의 쌍의 배열을 배열에 담아 반환

## 🌀 20. strict mode

### 20.1 strict mode란?

선언하지 않은 변수에 값을 할당하면 함수 스코프(현 위치) → 상위 스코프 .. 순으로 찾게된다.

전역 스코프에도 변수 선언이 존재하지 않는다. → ReferenceError를 발생시킬 것 같으나 아님! 자바스크립트 엔진이 암묵적으로 전역 객체에 프로퍼티를 동적 생성함(암묵적 전역)

⇒ 개발자 의도와는 상관없이 발생한 암묵적 전역은 오류를 발생시키는 원인이 될 가능성이 크다.

이러한 오류 줄이기 위해서는 근본적으로 잠재적 오류를 발생시키기 어려운 개발환경을 만들자! → strict mode 

+) ESLint 같은 린트 도구 사용

### 20.2 strict mode의 적용

전역의 선두 또는 함수 몸체의 선두에 `use strict;` 추가 

### 20.3 전역에 strict mode를 적용하는 것은 피하자

전역에 적용한 strict mode는 스크립트 단위로 적용된다.

스크립트 단위로 적용된 strict mode는 해당 스크립트에 한정되어 적용되는데, 혼용하는 것은 오류를 발생시킬 수 있다.

실행함수로 스크립트 전체를 감싸서 적용! 

### 20.4 함수 단위로 strict mode를 적용하는 것도 피하자

마찬가지로 어떤 함수는 적용하고 어떤 함수는 적용하지 않는다… → 번거로우며 오류 발생 가능성 높아짐

### 20.5 strict mode가 발생시키는 에러

**암묵적 전역**

선언하지 않은 변수를 참조하면 ReferenceError 발생

**변수, 함수, 매개변수의 삭제**

delete 연산자로 변수, 함수, 매개변수를 삭제하면 SyntaxError 발생

**매개변수 이름의 중복**

중복된 매개변수 이름 사용 시 SyntaxError 발생

**with 문의 사용**

with 문은 전달된 객체를 스코프 체인에 추가한다.

동일한 객체의 프로퍼티를 반복해서 사용할 때 객체 이름을 생략할 수 있어 코드가 간단해지는 효과가 있지만 성능과 가독성이 나빠지는 문제가 있어 사용하지 않는 것이 좋다.

### 20.6 strict mode 적용에 의한 변화

**일반 함수의 this**

함수를 일반함수로서 호출하면 this에 undefined가 바인딩 되는데, 일반 함수 내부에서 this를 사용할 필요가 없기 때문

**arguments 객체**

매개변수에 전달된 인수를 재할당하여 변경해도 arguments 객체에 반영되지 않는다.
