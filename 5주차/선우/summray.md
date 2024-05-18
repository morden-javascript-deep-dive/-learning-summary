# 16장 프로퍼티 어트리뷰트

# 16.1 내부 슬롯과 내부 메서드

- 내부 슬롯 & 내부 메서드
    - 자바스크립트 엔진의 구현 알고리즘을 설명하기 위해 ECMAScript 사양에서 사용하는 **의사 프로퍼티**와 **의사 메서드**이다.
    - ECMAScript 사양에 등장하는 이중 대괄호 (`[[ … ]]`)로 감싼 이름들이 내부 슬롯과 내부 메서드이다.
- 자바스크립트 엔진에서 실제로 동작하지만 개발자가 직접 접근할 수 없고, 외부로 공개된 객체의 프로퍼티는 아니다.
    - 일부만 간접적으로 접근할 수 있는 수단을 제공한다.
        
        ex) prototype
        
        ```jsx
        const o = {};
        o.[[Prototype]] // SyntaxError
        o.__proto__ // Object.prototype
        
        ```
        

# 16.2 프로퍼티 어트리뷰트와 프로퍼티 디스크럽터 객체

### 프로퍼티 어트리뷰트

- 자바스크립트 엔진은 **프로퍼티를 생성할 때** 프로퍼티의 상태를 나타내는 프로퍼티 어트리뷰트를 기본값으로 자동 정의한다.
- 프로퍼티 어트리뷰트
    - [[Value]] - 프로퍼티의 값
    - [[Writable]] - 프로퍼티 값의 갱신 가능 여부
    - [[Enumerable]] - 열거 가능 여부
    - [[Configurable]] - 재정의 가능 여부
    - 내부 슬롯이므로 직접 접근할 수 없지만, **Object.getOwnPropertDescriptor** 메서드를 사용해 간접적으로 확인 가능

### 프로퍼티 디스크립터

- **Object.getOwnPropertDescriptor**
    - (객체의 참조, 프로퍼티 키)를 매개변수로 받는다.
    - 프로퍼티 어트리뷰트 정보를 제공하는 **프로퍼티 디스크립터 객체**를 반환한다.
    - ES8에서 도입된 **Object.getOwnPropertDescriptors** 메서드는 모든 프로퍼티의 모든 프로퍼티 디스크립터 객체들을 반환한다.

# 16.3 데이터 프로퍼티와 접근자 프로퍼티

- **`데이터 프로퍼티`**
    - 키와 값으로 구성된 일반적인 프로퍼티
- **`접근자 프로퍼티`**
    - **다른 데이터 프로퍼티**의 값을 읽거나 저장할 때 호출되는 **접근자 함수**로 구성된 프로퍼티

## 16.3.1 데이터 프로퍼티

- 데이터 프로퍼티의 어트리뷰트

프로퍼티 생성 시 기본값으로 자동 생성된다.

| 프로퍼티 어트리뷰트 | 프로퍼티 디스크립터 객체의 프로퍼티 | 설명 |
| --- | --- | --- |
| [[Value]] | value | - 프로퍼티 키를 통해 프로퍼티 값에 접근하면 반환되는 값 |
- 프로퍼티 키의 값이 변경되면 [[Value]]에 값을 재할당한다. 이 때, 프로퍼티가 없으면 프로퍼티를 동적 생성하고 생성된 프로퍼티의 [[Value]]값에 저장한다. |
| [[Writable]] | writable | - 프로퍼티 값의 변경 여부. 불리언 값이다.
- [[Writable]]의 값이 false인 경우 해당 프로퍼티의 [[Value]] 값을 변경할 수 없는 읽기 전용 프로퍼티다. |
| [[Enumerable]] | enumerable | - 프로퍼티의 열거 가능 여부. 불리언 값이다.
- [[Enumerable]]의 값이 false인 경우 해당프로퍼티는 for…in이나 Object.keys 메서드등으로 열거할 수 있다. |
| [[Configurable]] | configurable | - 프로퍼티의 재정의가능여부. 불리언 값이다.
- [[Configurable]]의 값이 false인 경우 해당 프로퍼티의 삭제, 프로퍼티 어트리뷰트의 값의 변경이 금지된다.
- [[Configurable]]이 false이고 [[Writable]]이 true인 경우 [[Value]]의 변경과 [[Writable]]을 false로 변경하는 것은 허용된다. |

```jsx
const person = {
	name: 'Lee'
}

console.log(Object.getOwnPropertyDescriptor(person, 'name'));
// {value: "Lee", writable: true, enumerable: true, configurable: true }

```

프로퍼티가 생성될 때 [[Value]]의 값은 프로퍼티 값으로 초기화되며 [[Writable]], [[Enumerable]], [[Configurable]]의 값은 true로 초기화된다.

## 16.3.2 접근자 프로퍼티

- 접근자 프로퍼티는 자체적으로 값을 갖지 않고 다른 데이터 프로퍼티의 값을 읽거나 저장할 때 사용하는 **접근자 함수로 구성된** 프로퍼티다.
- 접근자 프로퍼티의 어트리뷰트

| 프로퍼티 어트리뷰트 | 프로퍼티 디스크립터 객체의 프로퍼티 | 설명 |
| --- | --- | --- |
| [[Get]] | get | - 접근자 프로퍼티를 통해 데이터 프로퍼티의 값을 읽을 때 호출되는 접근자 함수 |
- getter함수가 호출되고 그 결과가 프로퍼티 값으로 반환된다. |
| [[Set]] | set | - 접근자 프로퍼티를 통해 데이터 프로퍼티의 값을 저장할 때 호출되는 접근자 함수
- setter함수가 호출되고 그 결과가 프로퍼티 값으로 저장된다. |
| [[Enumerable]] | enumerable | 데이터 프로퍼티와 같음 |
| [[Configurable]] | configurable | 데이터 프로퍼티와 같음 |
- 접근자 함수는 getter/setter 함수라고도 부른다.
- getter와 setter를 둘 다 정의할 수도 있고 하나만 정의할 수도 있다

```jsx
const person = {
	firstName: 'Ungmo',
	lastName: 'Lee',

	get fullName(){
		return `${this.firstName} ${this.lastName}`;
	},

	set fullName(name){
		[this.firstName, this.lastName] = name.split(' ');
	}
};

//데이터 프로퍼티
let descriptor = Object.getOwnPropertyDescriptor(person, 'firstName');
console.log(descriptor);
// {value: 'Heeun', writable: true, enumerable: true, configurable: true}

//접근자 프로퍼티
let descriptor = Object.getOwnPropertyDescriptor(person, 'fullName');
console.log(descriptor);
// {get: f, set: f, enumerable: true, configurable: true}

```

- person객체의 firstName, latName 프로퍼티는 데이터 프로퍼티이다.
- 메서드 앞에 get, set 붙은 메서드가 getter, setter 함수이고, getter/setter함수가 접근자 프로퍼티다.
    - 접근자 fullName으로 프로퍼티 값에 접근하면 내부적으로 [[Get]]함수가 호출되어 다음과 같이 동작한다.
        1. 프로퍼티 키가 유효(문자열/심벌)한지 확인한다.
        2. 프로토타입 체인에서 프로퍼티를 검색한다. person 객체에 fullName 프로퍼티가 존재한다.
        3. 프로퍼티가 데이터 프로퍼티인지 접근자 프로퍼티인지 확인한다.
        4. 접근자 프로퍼티 fullName의 프로퍼티 어트리뷰트 [[Get]]의 값, getter 함수를 호출하여 그 결과를 반환한다.
        getter가 반환하는 값은 Object.getOwnPropertyDescriptor 메서드가 반환하는 프로퍼티 디스크립터 객체의 get 프로퍼티 값과 같다.

<aside>
💡 **프로토타입**

프로토타입은 어떤 객체의 상위 객체의 역할을 하는 객체다.
프로토타입 체인은 프로토타입이 단방향 링크드 리스트 형태로 연결되어있는 상속 구조를 말한다. 객체의 프로퍼티나 메서드에 접근할 때, 해당 객체에서 못찾으면 프로토타입 체인을 따라 프로토타입의 프로퍼티나 메서드를 차례대로 검색한다.

</aside>

# 16.4 프로퍼티 정의

- `프로퍼티 정의`: 새로운 프로퍼티를 추가하면서 프로퍼티 어트리뷰트를 명시적으로 정의하거나, 기존 프로퍼티의 프로퍼티 어트리뷰트를 재정의하는 것
    - 프로퍼티의 값을 갱신 가능하도록 할지, 열거 가능하도록 할지, 재정의 가능하도록 할 지 정의할 수 있다.
- Object.defineProperty(객체 참조, 키, property descriptor 객체) 로 프로퍼티 어트리뷰트를 정의할 수 있다.
    
    ```jsx
    const person = {};
    
    //데이터 프로퍼티 정의
    Object.defineProperty(person, 'firstName', {
    	value: 'Ungmo',
    	writable: true,
    	enumerable: true,
    	configurable: true
    });
    
    //접근자 프로퍼티 정의
    Object.defineProperty(person, 'fullName', {
    	get(){
    		return `${this.firstName} ${this.lastName}`;
    	}
    	set(name){
    		[this.firstName, this.lastName] = name.split(' ');
    	}
    	enumerable: true,
    	configurable: true
    });
    
    ```
    
- Object.defineProperty로 프로퍼티를 정의할 때 프로퍼티 디스크립터 객체의 프로퍼티를 일부 생략할 수 있다. 생략 시 기본값이 적용된다.
    
    
    | 프로퍼티 디스크립터 객체의 프로퍼티 | 대응하는 프로퍼티 어트리뷰트 | 생략했을 때의 기본값 |
    | --- | --- | --- |
    | value | [[Value]] | undefined |
    | get | [[Get]] | undefined |
    | set | [[Set]] | undefined |
    | writable | [[Writable]] | false |
    | enumerable | [[Enumerable]] | false |
    | configurable | [[Configurable]] | false |
- Object.defineProperties를 쓰면 여러개의 프로퍼티를 한 번에 정의할 수 있다.
    
    ```jsx
    const person = {};
    
    Object.defineProperties(person, {
    	//데이터 프로퍼티 정의
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
    	//접근자 프로퍼티 정의
    	fullName: {
    		get(){
    			return `${this.firstName} ${this.lastName}`;
    		}
    		set(name){
    			[this.firstName, this.lastName] = name.split(' ');
    		}
    		enumerable: true,
    		configurable: true
    });
    
    ```
    

# 16.5 객체 변경 감지

- 객체는 변경 가능한 값이므로 재할당 없이 직접 변경할 수 있다.
- 자바스크립트는 객체의 변경을 방지하는 다양한 메서드를 제공한다.
    
    | 구분 | 메서드 | 프로퍼티
    추가 | 프로퍼티
    삭제 | 프로퍼티
    값 읽기 | 프로퍼티
    값 쓰기 | 프로퍼티
    어트리뷰트 재정의 |
    | --- | --- | --- | --- | --- | --- | --- |
    | 객체 확장 금지 | Object.preventExtentsions | X | O | O | O | O |
    | 객체 밀봉 | Object.seal | X | X | O | O | X |
    | 객체 동결 | Object.freeze | X | X | O | X | X |
    

## 16.5.1 객체 확장 금지

- `Object.preventExtensions`메서드는 객체의 확장을 금지한다.
- 확장이 금지된 객체는 프로퍼티 추가가 금지된다.
    - 프로퍼티 추가는 프로퍼티 동적 추가와 Object.definenProperty메서드로 추가할 수 있는데, 이 두 가지가 금지된다.
- 확장이 가능한 객체인지 여부는 `Object.isExtensible` 메서드로 확인할 수 있다.

## 16.5.2 객체 밀봉

- `Object.seal` 메서드는 객체를 밀봉한다.
- `*객체 밀봉**`: 프로퍼티 추가 및 삭제와 프로퍼티 어트리뷰트 재정의 금지를 의미한다.
- 밀봉된 객체는 **읽기와 쓰기**만 가능하다.
- 밀봉된 객체인지 여부는 `Object.isSealed` 메서드로 확인할 수 있다.

## 16.5.3 객체 동결

- `Object.freeze` 메서드는 객체를 동결한다.
- `*객체 동결**`: 프로퍼티 추가 및 삭제와 프로퍼티 어트리뷰트 재정의 금지, 프로퍼티 값 갱신 금지를 의미한다.
- 동결된 객체인지 여부는 `Object.isFrozen` 메서드로 확인할 수 있다.

## 16.5.4 불변 객체

- 지금까지 살펴본 변경 방지 메서드들은 `얕은 변경 방지`로 직속 프로퍼티만 변경이 방지되고, 중첩 객체까지는 영향을 주지는 못한다.
- 따라서 Object.freee 메서드로 객체를 동결하여도 중첩 객체까지 동결할 수 없다.
- 중첩 객체까지 동결하여 변경이 불가능한 읽기 전용의 `불변 객체`를 구현하려면, 객체를 값으로 갖는 모든 프로퍼티에 대해 **재귀적으로** Object.freeze 메서드를 호출해야 한다.

---

# 17장.생성자 함수에 의한 객체 생성

## 17.1 Object 생성자 함수<a name="17.1"></a>

- new 연산자와 함께 Object 생성자 함수를 호출하면 빈 객체를 생성하여 반환함
- 빈 객체를 생성한 이후 프로퍼티 또는 메서드를 추가하여 객체 완성 가능함

```jsx
// 빈 객체의 생성
const person = new Object();

// 프로퍼티 추가
person.name = 'Lee';
person.sayHello = function () {
	console.log('Hi! My name is ' + this.name);
};

console.log(person); // {name: "Lee", sayHello: f}
person.sayHello(); // Hi! My name is Lee

```

- 생성자 함수: new 연산자와 함께 호출하여 객체를 생성하는 함수
    - ex) Object, String, Number, Boolean, Function, Array, Date, RegExp, Promise 등
- 인스턴스: 생성자 함수에 의해 생성된 객체

```jsx
// String 생성자 함수에 의한 String 객체 생성
const strObj = new String('Lee');
console.log(typeof strObj); // object
console.log(strObj); // String {"Lee"}

// Number 생성자 함수에 의한 Number 객체 생성
const numObj = new Number(123);
console.log(typeof numObj); // object
console.log(num0bj); // Number {123}

// Boolean 생성자 함수에 의한 Boolean 객체 생성
const boolObj= new Boolean(true);
console.log(typeof boolObj); // object
console.log(boolObj); // Boolean {true}

// Function 생성자 함수에 의한 Function 객체(함수) 생성
const func = new Function('x', 'return x * x');
console.log(typeof func); // function
console/dir(func); // f anonymous(x)

// Array 생성자 함수에 의한 Array 객체(배열) 생성
const arr = new Array(1, 2, 3);
console.log(typeof arr); // object
console.log(arr); // [1, 2, 3]

// RegExp 생성자 함수에 의한 RegExp 객체(정규 표현식) 생성
const regExp = new RegExp(/ab+c/i);
console.log(typeof regExp); // object
console.log(regExp); // /ab+c/i

// Date 생성자 함수에 의한 Date 객체 생성
const date = new Date();
console.log(typeof date); // object
console.log(date); // Mon May 04 2020 08:36:33 GMT+0900 (대한민국표준시)

```

- 객체 생성하는 방법은 객체 리터럴을 사용하는 것이 더 간편함

## 17.2 생성자 함수<a name="17.2"></a>

### 17.2.1 객체 리터럴에 의한 객체 생성 방식의 문제점

- 객체 리터럴에 의한 객체 생성 방식은 단 하나의 객체만 생성함
    - 동일한 프로퍼티를 갖는 객체를 여러 개 생성해야하는 경우 매법ㄴ 같은 프로퍼티를 기술해야 하기 때문에 비효율적

```jsx
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

- 객체는 프로퍼티를 통해 객체 고유의 상태를 표현하며, 메서드를 통해 상태 데이터인 프로퍼티를 참조하고 조작하는 동작 표현함
- 프로퍼티는 객체마다 프로퍼티 값이 다를 수 있지만 메서드는 내용이 동일한 경우가 일반적
- 객체 리터럴에 의해 객체를 생성하는 경우 프로퍼티 구조가 동일함에도 불구하고 매번 같은 프로퍼티와 메서드를 기술해야 함

### 17.2.2 생성자 함수에 의한 객체 생성 방식의 장점

- 템플릿(클래스)처럼 생성자 함수를 사용하여 프로퍼티 구조가 동일한 객체 여러 개를 간편하게 생성 가능

```jsx
// 생성자 함수
function Circle(radius) {
// 생성자 함수 내부의 this는 생성자 함수가 생성할 인스턴스를 가리킴
	this.radius = radius;
	this.getDiameter = function () {
		return 2 * this.radius;
	};
}

// 인스턴스의 생성
const circle1 = new Circle(5); // 반지름이 5인 Circle 객체를 생성
const circle2 = new Circle(10); // 반지름이 10인 Circle 객체를 생성

console.log(circle1.getDiameter()); // 10
console.log(circ1e2.getDiameter()); // 20

```

- this
    - 객체 자신의 프로퍼티나 메서드를 참조하기 위한 자기 참조 변수
    - this가 가리키는 값(this 바인딩)은 함수 호출 방식에 따라 동적으로 결정됨

| 함수 호출 방식 | this가 가리키는 값(this 바인딩) |
| --- | --- |
| 일반 함수로서 호출 | 전역 객체 |
| 메서드로서 호출 | 메서드를 호출한 객체(마침표 앞의 객체 |
| 생성자 함수로서 호출 | 생성자 함수가 (미래에) 생성할 인스턴스 |
- 생성자 함수
    - 객체(인스턴스)를 생성하는 함수
    - 클래스 기반 객체지향 언어(자바 등)의 생성자와는 다르게 형식이 정해져 있지 않고 일반 함수와 동일한 방법으로 생성자 함수 정의하고, new 연산자와 함께 호출하면 해당 함수는 생성자 함수로 동작함
    - new 연산자와 함께 생성자 함수를 호출하지 않으면 생성자 함수가 아니라 일반 함수로 동작함

```jsx
// new 연산자와 함께 호출하지 않으면 생성자 함수로 동작하지 않음
// 즉, 일반 함수로서 호출됨
const circle3 = Circle(15);

// 일반 함수로서 호출된 Circle은 반환문이 없으므로 암묵적으로 undefined 반환
console.log(circle3); // undefined

// 일반 함수로서 호출된 Circle 내의 this는 전역 객체 가리킴
console.log(radius); // 15

```

### 17.2.3 생성자 함수의 인스턴스 생성 과정

- 생성자 함수의 역할: 인스턴스 생성하는 것과 생성된 인스턴스를 초기화(인스턴스 프로퍼티 추가 및 초기값 할당)하는 것
- 생성자 함수가 인스턴스를 생성하는 것이 필수이며, 생성된 인스턴스를 초기화하는 것은 옵션

```jsx
// 생성자 함수
function Circle(radius) {
	// 인스턴스 초기화
	this.radius = radius;
	this.getDiameter = function () {
		return 2 * this.radius;
	};
}

// 인스턴스 생성
const circle1 = new Circle(5); // 반지름이 5인 Circle 객체 생성

```

- new 연산자와 함께 생성자 함수를 호출하면 자바스크립트 엔진은 암묵적으로 인스턴스를 생성하고 인스턴스를 초기화한 후 암묵적으로 인스턴스를 반환함
1. 인스턴스 생성과 this 바인딩
    - 암묵적으로 빈 객체 생성
    - 빈 객체는 바로 생성자 함수가 생성한 인스턴스
    - 암묵적으로 생성된 빈 객체
    - 바인딩: 식별자와 값을 연결하는 과정 의미함. this 바인딩은 this(키워드로 분류되나 식별자 역할을 함)와 this가 가리킬 객체를 바인딩한다.

```jsx
function Circle(radius) {
	// 1. 암묵적으로 인스턴스가 생성되고 this에 바인딩됨
	console.log(this); // Circle {}

	this.radius = radius;
	this.getDiameter = function () {
		return 2 * this.radius;
	};
}

```

1. 인스턴스 초기화
    - this에 바인딩되어 있는 인스턴스에 프로퍼티나 메서드를 추가하고 생성자 함수가 인수로 전달받은 초기값을 인스턴스 프로퍼티에 할당하여 초기화하거나 고정값을 할당함

```jsx
function Circle(radius) {
	// 1. 암묵적으로 인스턴스가 생성되고 this에 바인딩

	// 2. this에 바인딩되어 있는 인스턴스를 초기화함
	this.radius = radius;
	this.getDiameter = function () {
		return 2 * this.radius;
	};
}

```

1. 인스턴스 반환
    - 생성자 함수 내부에 모든 처리가 끝나면 완성된 인스턴스가 바인딩된 this가 암묵적으로 반환됨

```jsx
function Circle(radius) {
	// 1. 암묵적으로 빈 객체가 생성되고 this에 바인딩됨

	// 2. this에 바인딩되어 있는 인스턴스 초기화
	this.radius = radius;
	this.getDiameter = function () {
		return 2 * this.radius;
	};

	// 3. 완성된 인스턴스가 바인딩된 this가 암묵적으로 반환
}

	// 인스턴스 생성 // Circle 생성자 함수는 암묵적으로 this 반환
const circle = new Circle(1);
console.log(circle); // Circle {radius: 1, getDiameter: f}

```

- this가 아닌 다른 객체를 명시적으로 반환하면 this가 반환되지 못하고 return 문에 명시한 객체가 반환됨

```jsx
function Circle(radius) {
	// 1. 암묵적으로 빈 객체가 생성되고 this에 바인딩됨

	// 2. this에 바인딩되어 있는 인스턴스 초기화
	this.radius = radius;
	this.getDiameter = function () {
		return 2 * this.radius;
	};

	// 3. 암묵적으로 this 반환
	// 명시적으로 객체 반환하면 암묵적인 this 반환 무시
	return {};
}

// 인스턴스 생성 // Circle 생성자 함수는 암묵적으로 this 반환
const circle = new Circle(1);
console.log(circle); // {}

```

- 명시적으로 원시 값을 반환하면 원시 값 반환은 무시되고 암묵적으로 this가 반환됨

```jsx
function Circle(radius) {
	// 1. 암묵적으로 빈 객체가 생성되고 this에 바인딩됨

	// 2. this에 바인딩되어 있는 인스턴스를 초기화함
	this.radius = radius;
	this.getDiameter = function () {
		return 2 * this.radius;
	};

	// 3. 암묵적으로 this 반환
	// 명시적으로 원시 값을 반환하면 원시 값 반환은 무시되고 암묵적으로 this가 반환
	return 100;
}

// 인스턴스 생성 // Circle 생성자 함수는 명시적으로 반환한 객체를 반환함
const circle = new Circle(1);
console.log(circle); // Circle {radius: 1, getDiameter: f}

```

- 생성자 함수 내부에서 명시적으로 this가 아닌 다른 값을 반환하는 것은 생성자 함수의 기본 동작을 훼손함
- 생성자 함수 내부에서 return 문을 반드시 생략해야 함

### 17.2.4 내부 메서드 [[Call]]과 [[Construct]]

- 함수 선언문 또는 함수 표현식으로 정의한 함수는 일반적인 함수로서 호출할 수 있는 것은 물론 생성자 함수로서 호출할 수 있음
- 생성자 함수로서 호출한다는 것은 new 연산자와 함께 호출하여 객체를 생성하는 것을 의미
- 함수는 객체이므로 일반 객체와 동일하게 동작 가능하며, 일반 객체가 가지고 있는 내부 슬롯과 내부 메서드를 모두 가지고 있음

```jsx
// 함수는 객체
function foo() {}

// 함수는 객체이므로 프로퍼티 소유 가능
foo.prop = 10;

// 함수는 객체이므로 메서드 소유 가능
foo.method = function () {
	console.log(this.prop);
};

foo.method(); // 10

```

- 일반 객체는 호출할 수 없지만 함수는 호출 가능(함수는 객체이나, 일반 객체는 다름)
- 함수 객체는 일반 객체가 가지고 있는 내부 슬롯과 내부 메서드, 함수로서 동작하기 위해 함수 객체만을 위한 [[Environment]], [[FormalParameters]] 등 내부 슬롯과 [[Call]], [[Construct]] 같은 내부 메서드를 추가로 가지고 있음

```jsx
function foo() {}
// 일반적인 함수로서 호출: [[Call]] 호출됨
foo();

/// 생성자 함수로서 호출: [[Construct]]가 호출됨
new foo();

```

- callable: 내부 메서드 [[Call]]을 갖는 함수 객체
- constructor: 내부 메서드 [[Construct]]를 갖는 함수 객체
- non—constructor: 내부 메서드 [[Construct]]를 갖지 않는 함수 객체
- 모든 함수 객체는 호출할 수 있지만 모든 함수 객체를 생성자 함수로서 호출할 수 있는 것은 아니기 때문에, callable이면서 constructor이거나 callable이면서 non-constructor이다.

### 17.2.5 constructor와 non-constructor의 구분

- 함수 정의를 평가하여 함수 객체를 생성할 때 함수 정의 방식에 따라 함수를 constructor와 non-constructor로 구분함
    - constructor: 함수 선안문, 함수 표현식, 클래스(클래스도 함수다)
    - non-constructor: 메서드(ES6 메서드축약표현), 화살표함수

```jsx
// 일반 함수 정의: 함수 선언문, 함수 표현식
function foo() {}
const bar = function () {};
// 프로퍼티 x의 값으로 할당된 것은 일반 함수로 정의된 함수 // 이는 메서드로 인정하지 않음
const baz = {
	x: function () {}
};

// 일반 함수로 정의된 함수만이 constructor
new foo(); // foo {}
new bar(); // bar {}
new baz.x(); // x {}

// 화살표 함수 정의
const arrow = () => {};
new arrow(); // TypeError

// 메서드 정의: ES6의 메서드 축약 표현만 메서드로 인정
const obj = {
	x() {}
};

new obj.x(); // TypeError

```

- 함수를 프로퍼티 값을 사용하면 일반적으로 메서드로 통칭함

```jsx
function foo() {}

// 일반 함수로서 호출
// [[Call]]이 호출됨
// 모든 함수 객체는 [[Call]]이 구현되어 있음
foo();

// 생성자 함수로서 호출
// [[Construct]]가 호출됨
// 이때[[Construct]]를 갖지 않는다면 에러 발생
new foo();

```

- 생성자 함수로서 호출될 것을 기대하고 정의하지 않은 일반 함수(callable이면서 constructor)에 new 연산자를 붙여 호출하면 생성자 함수처럼 동작 가능

### 17.2.6 new 연산자

- new 연산자와 함께 함수 호출하면 해당 함수는 생성자 함수로 동작함
    - 함수 객체의 내부 메서드 [[Call]이 호출되는 것이 아니라 [[Construct]]가 호출됨
- new 연산자와 함께 호출하는 함수는 non-constructor가 아닌 constructor이어야 함

```jsx
// 생성자 함수로서 정의하지 않은 일반 함수
function add(x, y) {
	return x + y;
}

// 생성자 함수로서 정의하지 않은 일반 함수를 new 연산자와 함께 호출
let inst = new add();

// 함수가 객체를 반환하지 않았으므로 반환문 무시 // 따라서 빈 객체가 생성되어 반환됨
console.log(inst); // {}

// 객체를 반환하는 일반 함수
function createUser(name, role) {
	return { name, role };
}

// 일반 함수를 new 연산자와 함께 호출
inst = new createUser('Lee', 'admin');
// 함수가 생성한 객체 반환
console.log(inst); // {name: "Lee", role: "admin"}

```

- new 연산자 없이 생성자 함수를 호출하면, 함수 객체의 내부 메서드 [[Construct]]가 호출되는 것이 아니라 [[Call]]이 호출됨

```jsx
// 생성자 함수
function Circle(radius) {
	this.radius = radius;
	this.getDiameter = function () {
		return 2 * this.radius;
	};
}

// new 연산자 없이 생성자 함수 호출하면 일반 함수로서 호출됨
const circle = Circle(5);
console.log(circle); // undefined

// 일반 함수 내부의 this는 전역 객체 window를 가리킴
console.log(radius); // 5
console.log(getDiameter()); // 10

circle.getDiameter(); // TypeError

```

### 17.2.7 new.target

- new 연산자와 함께 생성자 함수로서 호출되면 함수 내부의 `new.target`은 함수 자신을 가리킨다. new 연산자 없이 일반 함수로서 호출된 함수 내부의 new.target은 undefined다.

```jsx
// 생성자 함수
function Circle(radius) {
	// 이 함수가 new 연산자와 함께 호출되지 않았다면 new.target === undefined
	if (!new.target) {
		// new 연산자와 함께 생성자 함수를 재귀 호출하여 생성된 인스턴스 반환
		return new Circle(radius);
	}

	this.radius = radius;
	this.getDiameter = function () {
		return 2 * this.radius;
	};
}

// new 연산자 없이 생성자 함수를 호출하여도 new.target을 통해 생성자 함수로서 호출됨
const circle = Circle(5);
console.log(circle.getDiameter());

```

- 스코프 세이프 생성자 패턴
    - new.target은 ES6에서 도입된 최신 문법으로 IE에서는 지원하지 않음
    - new.target을 사용하지 못한다면, 스코프 세이프 생성자 패턴 사용 가능

```jsx
// Scope-Safe Constructor Pattern
function Circle(radius) {
	// 생성자 함수가 new 연산자와 함께 호출되면 함수의 선두에서 빈 객체를 생성하고 this에 바인딩
	// 이때 this와 Circle은 프로토타입에 의해 연결

	// 이 함수가 new 연산자와 함께 호출되지 않았다면 이 시점의 this는 전역 객체 window 가리킴
	// 즉, this와 Circle은 프로토타입에 의해 연결되지 않

	if (!(this instanceof Circle)) {
	// new 연산자와 함께 호출하여 생성된 인스턴스 반환
	return new Circle(radius);
	}

	this.radius = radius;
	this.getDiameter = function () {
		return 2 * this.radius;
	};
}

// new 연산자 없이 생성자 함수를 호출하여도 생성자 함수로서 호출됨
const circle = Circle(5);
console.log(circle.getDiameter()); // 10

```

- 대부분 빌트인 생성자 함수는 new 연산자와 함께 호출되었는지를 확인 후 적절한 값 반환함

```jsx
let obj = new Object();
console.log(obj); // {}

obj = Object();
console.log(obj); // {}

let f = new Function('x', 'return x ** x');
console.log(f); // f anonymous(x) { return x ** x }

f = Function('x', 'return x ** x');
console.log(f); // f anonymous(x) { return x ** x }

```

- String, Number, Boolean 생성자 함수는 new 연산자와 함께 호출했을 때 String, Number, Boolean 객체를 생성 반환하지만 new 연산자 없이 호출하면 문자열, 숫자, 불리언 값을 반환함

```jsx
const str = String(123);
console.log(str, typeof str); // 123 string

const num = Number('123');
console.log(num, typeof num); // 123 number

const bool = Boolean('true');
console.log(bool, typeof bool); // true boolean
```

---

# 18장 함수와 일급 객체

# 18.1 일급 객체

- 일급 객체는 다음과 같은 조건을 만족한다.
    1. **무명의 리터럴**로 생성할 수 있다. 즉, **런타임에 생성**이 가능하다.
    2. **변수나 자료구조에 저장**할 수 있다.
    3. **함수의 매개변수**에 전달할 수 있다.
    4. **함수의 반환값**으로 사용할 수 있다.
- 자바스크립트 함수의 일급 객체로서의 특징
    - 자바스크립트의 함수는 **일급 객체**다. 즉, 함수를 객체와 동일하게 사용할 수 있다.
    **값**과 동일하게 취급될 수 있고, 값을 사용할 수 있는 곳이라면 어디든지 리터럴로 정의할 수 있으며, **런타임**에 함수 객체로 평가된다.
    - 일급 객체로서 함수가 가지는 가장 큰 특징은 일반 객체와 같이 **함수의 매개변수에 전달**할 수 있으며, **반환값**으로 사용할 수 있다는 것이다.
    ⇒ **함수형 프로그래밍**을 가능케 하는 자바스크립트의 장점이다.
- 일반 객체와의 차이점
    - 일반 객체는 호출할 수 없지만 함수 객체는 호출할 수 있다.
    - 함수 객체는 일반 객체에는 없는 함수 고유의 프로퍼티를 소유한다.

# 18.2 함수 객체의 프로퍼티

- 함수도 객체이기 때문에 프로퍼티를 가질 수 있다.

```jsx
function square(number) {
  return number * number;
}

console.dir(square);

```

<img src='./square.png' width='50%'/>

- Object.getOwnPropertyDescriptors 메서드로 확인해보면 다음과 같다.

```jsx
console.log(Object.getOwnPropertyDescriptors(square));

```

<img src='./getOwnPropertyDescriptors.png' width='50%'/>

- `arguments`, `caller`, `length`, `name`, `prototype` 프로퍼티는 모두 함수 객체의 데이터 프로퍼티이다.
- `__proto__` 는 접근자 프로퍼티이며, 함수 객체의 고유 프로퍼티가 아니라 Object.prototype 객체의 프로퍼티를 상속 받은 것이다.

## 18.2.1 arguments 프로퍼티

- 함수 객체의 arguments 프로퍼티 값은 arguments 객체다.
- arguments 객체는 함수 호출 시 전달된 **인수들의 정보**를 담고 있는 **순회 가능한 유사 배열 객체**다.
- 함수 내부에서 지역변수처럼 사용된다. 즉, 외부에서는 참조할 수 없다.
- 자바스크립트의 함수는 매개변수와 인수의 개수가 일치하는지 확인하지 않는다.
    - 매개변수 개수 > 인수 개수 : 전달되지 않은 매개변수는 undefined로 초기화된 상태를 유지한다.
    - 매개변수 개수 < 인수 개수 : 초과된 인수는 무시된다. 그렇지만 암묵적으로 arguments 객체의 프로퍼티로 보관된다.
    
    ```jsx
    function multiply(x, y) {
      console.log(arguments);
      return x * y;
    }
    
    console.log(multiply()); //NaN
    console.log(multiply(1)); //NaN
    console.log(multiply(1, 2)); // 2
    console.log(multiply(1, 2, 3)); //2
    
    ```
    
    <img src='./arguments.png' width='50%'/>
    `arguments`의 객체는 인수를 프로퍼티 값으로 소유하며, 프로퍼티 키는 **`인수의 순서`**를 나타낸다.
    `callee` 프로퍼티는 함수 자신을 가리키고, `length` 프로퍼티는 인수의 개수를 가리킨다.
    
- arguments 객체는 매개변수의 개수를 확정할 수 없는 `*가변 인자 함수**`를 구현할 때 유용하다.
    
    ```jsx
    function sum() {
    	let sum = 0;
    
    	for(let i = 0 ; i < **arguments**.length ; i++){
    		res += **arguments**[i];
    	}
    	return res;
    }
    
    ```
    
- arguments 객체는 실제 배열이 아닌 유사 배열 객체다. `유사 배열 객체`란 **length** 프로퍼티를 가진 객체로, **for문으로 순회**할 수 있는 객체를 말한다.
    - 유사 배열 객체에 배열 메서드를 사용하면 에러가 발생한다. 따라서 배열 메서드를 사용하려면 Function.prototpye.call, Function,prototype.apply 를 통해 간접 호출 해야하는 번거로움이 있다.
        
        ```jsx
        function sum() {
          const array = Array.prototype.slice.call(arguments);
        
          return (array = array.reduce(function (pre, cur) {
            return pre + cur;
          }, 0));
        }
        
        ```
        
    - 이런 번거로움을 해결하기 위해 ES6에서는 **Rest 파라미터**를 도입했다.
        
        ```jsx
        function sum(...args) {
          return args.reduce(function (pre, cur) {
            return pre + cur;
          }, 0);
        }
        
        ```
        
        rest 파라미터의 도입으로 모던 자바스크립트에서는 arguments 객체의 중요성이 이전 같지는 않지만 언제나 ES6만 사용하지는 않을 수 있기 때문에 알아둘 필요가 있다.
        

## 18.2.2 caller 프로퍼티

- caller프로퍼티는 ECMAScript 사양에 포함되지 않은 비표준 프로퍼티이고, 표준화 될 예정도 없는 프로퍼티 이므로 **사용하지 말고** 참고로만 알아둔다.
- caller 프로퍼티는 함수 자신을 호출한 함수를 가리킨다.

```jsx
function foo(func) {
  return func();
}

function bar() {
  return "caller : " + bar.caller;
}

console.log(foo(bar)); // caller: function foo(func) {...}
console.log(bar()); // caller: null

```

## 18.2.3 length 프로퍼티

- 함수를 정의할 때 선언한 매개변수의 개수를 가리킨다.
- **arguments객체의 length 프로퍼티**와 **함수 객체의 length 프로퍼티**의 값은 다를 수 있으므로 주의해야 한다.
    - **arguments의 length**: 인자의 개수
    - **함수 객체의 length**: 매개변수의 개수

## 18.2.4 name 프로퍼티

- 함수 객체의 name 프로퍼티는 함수 이름을 나타낸다.
- ES6에서 정식 표준이 되었다.
- name 프로퍼티는 ES5과 ES6에서 동작을 달리하므로 주의한다.
    - 익명 함수의 경우 ES5에서는 빈 문자열을 값으로 갖지만, ES6에서는 함수 객체를 가리키는 **식별자**를 값으로 갖는다.
    
    ```jsx
    // 기명 함수 표현식
    var namedFunc = function foo() {};
    console.log(named.name); //foo
    
    // 익명 함수 표현식
    var anonymousFunc = function () {};
    console.log(anonymousFunc.name); //anonymousFunc
    
    // 함수 선언문
    function bar() {}
    console.log(bar.name); //bar
    
    ```
    
    **함수 이름**과 **함수 객체를 가리키는 식별자**는 의미가 다르다는 것을 잊지 말자.
    함수를 호출할 때는 함수 이름이 아닌 함수 객체를 가리키는 **식별자**로 호출한다.
    

## 18.2.5 **\*\proto*\*\* 접근자 프로퍼티

- 모든 객체는 [[Prototype]]이라는 내부 슬롯을 갖는다. [[Prototype]] 내부 슬롯은 객체지향 프로그래밍의 상속을 구현하는 프로토타입 객체를 가리킨다.
- **proto** 프로퍼티는 [[Prototype]] **내부 슬롯**이 가리키는 프로토타입 객체에 접근하기 위해 사용하는 접근자 프로퍼티다.
내부 슬롯은 직접 접근할 수 없고, 간접적으로 접근 방법을 제공할 때에만 접근할 수 있다.

```jsx
const obj = { a: 1 };

console.log(obj.__proto__ === Object.prototype); //true

console.log(obj.hasOwnProperty("a")); //true
console.log(obj.hasOwnProperty("__proto__")); //false

```

## 18.2.6 prototype 프로퍼티

- prototype 프로퍼티는 생성자 함수로 호출될 수 있는 함수 객체, 즉 `constructor`만이 소유하는 프로퍼티다.
- 일반 객체와 생성자 함수로 호출할 수 없는 `non-constructor`에는 prototype 프로퍼티가 없다.

```jsx
(function () {}).hasOwnProperty("prototype"); //true
({}).hasOwnProperty("prototype"); //false
```
