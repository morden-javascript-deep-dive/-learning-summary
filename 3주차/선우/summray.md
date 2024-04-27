# 10장. 객체 리터럴

## 10.1 객체란?

- 자바스크립트는 객체(object) 기반의 프로그래밍 언어
    - 원시값을 제외한 나머지 값(함수, 배열, 정규 표현식 등)은 모두 객체
- 원시타입 VS 객체타입
    - 원시타입: 단 하나의 값만 나타냄 → 변경 불가능한 값
    - 객체타입: 다양한 타입의 값(원시 값 또는 다른 객체)을 하나의 단위로 구성한 자료구조 → 변경 가능한 값
- 객체란?
    - 0개 이상의 프로퍼티로 구성된 집합
        
        ```jsx
        var person = {
        	name: 'Lee',   // 프로퍼티 1
        	age:20         // 프로퍼티 2(프로퍼티 키: 값)
        }
        ```
        
- 메서드(method)란?
    - 프로퍼티 값이 함수일 경우, 일반 자바스크립트 함수와 구분하기 위해 부르는
        
        ```jsx
        var counter = {
        	num: 0,                    // 프로퍼티
        	increase: function () {    // 메서드
        	    this.num++;
        	}
        }
        ```
        
- 정리
    - `객체` = 프로터티 + 메서드로 구성된 집합
        - 객체의 상태를 나타내는 값과 프로퍼티를 참조하고 조작할 수 있는 동작을 모두 포함할 수 있기 때문에 상태와 동작을 하나의 단위로 구조화 할 수 있어 유용
    - `프로퍼티`: 객체의 상태를 나타내는 값(data)
    - `메서드`: 프로퍼티(상태 데이터)를 참조하고 조직할 수 있는 동작(behavior)
- 자바스크립트의 객체는 함수와 밀접한 관계를 가진다.
    - JavaScript가 함수를 '일급 객체'로 취급하기 때문에, 함수를 변수에 할당하거나 다른 함수에 전달할 수 있음을 의미합니다.
    
    1) 함수로 객체를 생성하기(생성자 함수): 
    
    생성자 함수를 사용하여 새로운 객체를 만들 때는 new 키워드를 사용
    
    ```jsx
    // 생성자 함수 정의
    function Person(name, age) {
      this.name = name;
      this.age = age;
    }
    
    // 객체 생성
    var person1 = new Person('John', 30);
    console.log(person1); // 출력: { name: 'John', age: 30 }
    ```
    
     
    
    2) 함수 자체가 객체: 자바스크립트에서 함수는 객체. 함수가 프로퍼티와 메서드를 가질 수 있고, 다른 변수에 할당되거나 다른 함수의 인수로 전달될 수 있음
    
    ```jsx
    // 함수를 변수에 할당
    var greet = function(name) {
      console.log('Hello, ' + name + '!');
    };
    
    // 함수 실행
    greet('Alice'); // 출력: Hello, Alice!
    
    ```
    

## 10.2 객체 리터럴에 의한 객체 생성

- 클래스 기반 객체지향 언어(C++, JAVA) VS 프로토타입 기반 객체지향 언어(JavaScript)
    - 클래스 기반: new 연산자와 함께 생성자를 호출해서 인스턴스를 생성하는 방식
        - 인스턴스란?
            - 클래스에 의해 생성되어 메모리에 저장
            - 클래스는 인스턴스를 생성하기 위한 템플릿 역할
    - 프로토타입 기반: 다양한 객체 생성방법을 지원
        - 객체 리터럴
        - 함수이용: Object 생성자 함수, 생성자 함수, Object.create 메서드, 클래스(ES6)
- 객체 리터럴이란?
    - 중괄호 {…} 내에 0개 이상의 프로퍼티를 정의
    - 변수에 할당되는 시점에 자바스크립트 엔진은 객체 리터럴을 해석해 객체를 생성
    - 중괄호 내에 프로퍼티 정의하지 않으면({ }) 빈 객체 생성
    - 장점: 클래스를 먼저 정의하고 new 연산자와 함께 생성자를 호출할 필요없이 바로 리터럴로 객체를 생성할 수 있음 → 자바스크립트의 유연함과 강력함
    
    ```jsx
    var person = {
       name: 'Lee',
       sayHello: function () {
          console.log(`Hello! &{name}.`);
       }
    };
    
    console.log(typeof person);   //object
    console.log(person);   //{name: "Lee", sayHello: f}
    ```
    
    ```jsx
    var empty = {};   //빈 객체
    console.log(typeof empty);   //object
    ```
    
- 객체 리터럴 VS 코드블럭
    - 객체 리터럴은 코드 블럭이 아니다. 하나의 객체로 평가되는 표현식이기에 뒤에 세미콜론(;)을 붙임

## 10.3프로퍼티

- 프로퍼티의 구성
    - 프로퍼티 키: 빈 문자열을 포함하는 모든 문자열 또는 심벌 값
        - 식별자 네이밍을 준수?
            - `O`: 프로퍼티 키에서는 따옴표 생략 가능
            - `X`: 프로퍼티 키에서는 따옴표 생략 불가 → 다른 의미로 해석될 수 있기 때문에 → 프로퍼티 접근시에 반드시 `대괄호 접근법`을 사용해야 함.
            
            ex) 
            
            ```jsx
            var name = {
            	firstName: seonu,   //네이밍규칙을 준수했으므로, 키 값에 따옴표 생략가능
            	last-name : Hong   //네이밍규칙 준수안해서, 다른 의미로 해석될 여지 있어서 따옴표 필수
            }
            ```
            
    - 프로퍼티 값: 자바스크립트에서 사용할 수 있는 모든 값
    - 각 프로퍼티 뒤에 `,`를 붙이고, 마지막에는 붙여도 안붙여도 상관없음
- 프로퍼티 키 특징
    - 프로퍼티 키 동적 생성 가능
        - 프로퍼티 키로 사용할 표현식을 대괄호([…])로 묶어야 함
            
            ```jsx
            var obj = {};
            var key = 'hello';
            
            //ES5: 프로퍼티 키 동적 생성
            obj[key] = 'world';
            // ES6: 계산된 프로퍼티 이름
            // var obj = {[key]: 'world'};
            
            console.log(obj); //{hello: "world"}
            ```
            
    - 빈 문자열도 프로퍼티 키로 사용 가능 → 권장하지 않음… 키로서의 의미가 없음
    - 프로퍼티 키에 문자열이나 심벌 값 이외의 값을 사용하면 암묵적 타입변환을 통해 문자열이 됨
    - 예약어(var, function등)을 프로퍼티 키로 사용 가능 →  권장하지 않음…예상치  못한 에러가 발생할 수 있음
    - 이미 존재하는 프로퍼티 키를 중복 선언하면, 나중에 선언한 프로퍼티가 먼저 선언한 애를 덮어씀

## 10.4 메서드

- 메서드란?
    - 프로퍼티 값이 함수일때를 말함
    - 즉, 객체에 묶여있는 함수를 의미

## 10.5 프로퍼티 접근

- 대괄호 표기법(마침표 접근 연산자를 사용)
- 마침표 표기법(대괄호 접근 연산자를 사용): 대괄호 접근시에는 [ “…” ] 대괄호 안에 키값에는 무조건 따옴표가 들어가 있어야함.

```jsx
var person = {
   name: 'Lee'
};

// 마침표 표기법에 의한 프로퍼티 접근
console.log(person.name); // Lee
// 대괄호 표기법에 의한 프로퍼티 접근
console.log(person['name']); // Lee
console.log(person[name]); //ReferenceError: name is not defined
```

식별자 name을 평가하기 위해 person 객체 내에 선언된 name을 찾았지만 찾지 못해 undefined를 반환.

즉, 객체에 존재하지 않는 프로퍼티에 접근하면 undefined를 반환함.

- 의문점??: 객체에 존재하지 않는 프로퍼티인데 ReferenceError가 아닌 undefined를 반환?
    
    ```jsx
    var person = {
    	name: 'John',
    	age: 30
    };
    
    console.log(person.address); // undefined
    console.log(nonExistentVariable); // ReferenceError: nonExistentVariable is not defined
    ```
    

- node.js 환경 VS 브라우저 환경
    
    ```jsx
    var person = {
    	'last-name' : 'Hong'
    }
    
    person['last-name']; // -> Hong
    person.last-name; // -> node.js 환경: ReferenceError: name is not defined
                      // -> 브라우저 환경: NaN
    ```
    
    - node.js 환경에서의 person.last-name
        
        1) person.last 를 평가. person에는 last라는 객체가 없기에, person.last는 undefined
        
        2) 그러면, undefined -name과 같아진다
        
        3) 자바스크립트 엔진이 name이라는 식별자를 찾는다.(키가 아니라 식별자(변수, 함수..)를 찾음)
        
        4) node.js는 name이라는 식별자가 없으므로, name is noe defined가 뜸
        
    - 브라우저 환경에서의 person.last-name
        
        1) person.last 를 평가. person에는 last라는 객체가 없으므로, person.last는 undefined
        
        2) 그러면, undefined -name과 같아짐.
        
        3) name이라는 식별자를 찾음
        
        4) 자바스크립트 환경에서는 name이라는 객체가 존재. 초기값 빈 문자열 → name은 창(window)의 이름을 나타냄
        
        5) undefined -‘ ‘과 같아짐으로 NaN이 뜸
        

## 10.6 프로퍼티 값 갱신

- 이미 존재하는 프로퍼티에 값을 할당하면 프로퍼티 값이 갱신됨

```jsx
var person = {
	name: 'Lee'
};

person.name = 'Hong';

console.log(person); // {name: "Hong"}
```

## 10.7 프로퍼티 동적 생성

- 존재하지 않는 프로퍼티에 값을 할당하면 프로퍼티가 동적으로 생성되어 추가되고 프로퍼티 값이 할당됨

```jsx
var person = {
	lastName: 'Hong'
};
// person에는 firstName이라는 프로퍼티가 존재하지 않음
// 따라서 person 객체에 firstName이라는 프로퍼티가 동적으로 생성되고 할당됨
person.firstName = 'seonu';

console.log(person); //{ lastName: 'Hong', firstName: 'seonu'}

```

## 10.8 프로퍼티 삭제

- delete 연산자는 프로퍼티를 삭제함
- delete 연산자의 피연산자 접근할 수 있는 표현식이어야 함
- delete 연산자의 피연산자가 존재하지 않으면, 에러없이 무시됨

```jsx
var name = {
	firstName: 'seonu',
	lastName: 'Hong'
}
// firstName의 프로퍼티 지워짐
delete name.firstName;
// middleName의 프로퍼티 존재하지 않으므로, 에러없이 무시됨
delete name.middleName;

console.log(name); // {lastName: "Hong"}
```

## 10.9 ES6에서 추가된 객체 리터럴의 확장 기능

### 10.9.1 프로퍼티 축약표현

- 프로퍼티 값은 변수로 사용 가능
    
    ```jsx
    // ES5
    var x = 1, y = 2;
    
    var obj = {
    	x: x,
    	y: y
    }
    
    console.log(obj); // {x:1, y:2}
    ```
    
- 프로퍼티 축약 표현: 프로퍼티 값 변수 사용시, 변수 이름과 프로퍼티 키가 동일 이름일 때 프로퍼티 키를 생략할 수 있음 → 프로퍼티 키는 변수 이름으로 자동 생성됨
    
    ```jsx
    // ES6
    let x = 1, y =2;
    
    // 프로퍼티 축약 표현
    const obj = { x, y };
    
    console.log(obj);  // {x: 1, y: 2}
    ```
    

### 10.9.2 계산된 프로퍼티 이름

- 문자열 또는 문자열로 타입 변환할 수 있는 값으로 평가되는 표현식을 사용해 프로퍼티 키를 동적으로 생성할 수 있음
- 단, 프로퍼티 키로 사용할 표현식을 대괄호([ … ])로 묶어야 함

```jsx
// ES5
var prefix = 'prop';
var i = 0;

var obj = {};

// 계산된 프로퍼티 이름으로 프로퍼티 키 동적 생성
obj[prefix + '-' + ++i] = i;
obj[prefix + '-' + ++i] = i;
obj[prefix + '-' + ++i] = i;

console.log(obj);  // {prop-1:1, prop-2:2, prop-3:3}
```

- ES6 에서는 객체 리터럴 내부에서도 계산된 프로퍼티 이름으로 프로퍼티 키를 동적 생성할 수 있음

```jsx
// ES6
const prefix = 'prop';
let i = 0;

// 객체 리터럴 내부에서 계산된 프로퍼티 이름으로 프로퍼티 키를 동적 생성
const obj = {
	[`${prefix}-${++i}`]: i,
	[`${prefix}-${++i}`]: i,
	[`${prefix}-${++i}`]: i
};

console.log(obj); // {prop-1: 1, prop-2: 2, prop-3: 3}
```

### 10.9.3 메서드 축약 표현

- 매서드 정의하려면 프로퍼티 값으로 함수를 할당함

```jsx
// ES5
var obj = {
	name: 'Hong',
	sayHi: function () {
		console.log('Hi! '+this.name);
	}
};

obj.sayHi(); // Hi! Hong
```

- function 키워드를 생략한 축약 표현 사용 가능

```jsx
// ES6
const obj = {
	name: 'Hong',
	sayHi() {
		console.log('Hi! '+this.name);
	}
};

obj.sayHi();  // Hi! Hong
```
---
# 11장. 원시값과 객체의 비교

## 11.1 원시 값

- 7가지의 데이터 타입
    - 원시타입: `숫자`/ `문자열`/ `불리언`/ `undefined`/ `null` / `심벌` 타입
    - `객체` 타입: 객체, 함수, 배열등
- 원시타입과 객체타입의 구분 기준
    
    1) 변경 가능한가?
    
    - 원시: 변경 불가능한 값
    - 객체(참조): 변경 가능한 값
    
    2) 변수(확보된 메모리 공간)에 할당하면 저장되는 값
    
    - 원시: 실제 값이 저장
    - 객체(참조): 참조 값이 저장
    
    3) (원시 값 or 참조 값) 저장된 변수를 또 다른 변수에 할당하면
    
    - 원시: 값에 의한 전달 → 원시 값이 복사되어 전달
    - 객체(참조): 참조에 의한 전달 → 참조 값이 복사되어 전달

### 11.1.1 변경 불가능한 값

원시 값 = 변경 불가능한 값 = 읽기 전용 값 = 데이터의 신뢰성을 보장 = 불변성

- 변수 VS 값
    - 변수: 하나의 값을 저장하기 위해 확보한 메모리 공간 자체
    - 값: 변수에 저장된 데이터로서 표현식으로 평가되어 생성된 결과
    
    → 원시 값이 변경 불가능하다 ≠ 원시 값을 담은 변수 값이 변경 불가능하다
    
    (변수 값은 다른 원시 값으로 교체 가능)
    
- 변수 VS 상수
    - 상수: 재할당이 금지된 변수 → 변수(확보된 공간)에 한번의 할당만이 가능
- 값의 재할당시
    
    1) 새로운 메모리 공간 확보
    
    2) 재할당한 원시 값 저장
    
    3) 새롭게 재할당한 원시 값 가리킴 = 주소의 변동 = 변수가 참조하던 메모리 주소가 변함
    

(불변성을 갖는 원시 값을 할당한 변수는 재할당 이외의 변수 값을 변경할 수 있는 방법이 없다 → 만약 재할당 이외에 원시값인 변수 값을 변경할 수 있다면 예기치 않게 변수 값이 변경될 수 있음을 의미 → 값의 변경(상태의 변경)을 추적하기 어렵게 만듦) → 문자열에서도 한 문자만 변경 불가능, 무조건 재할당해야지 변경됨

### 11.1.2 문자열과 불변성

- 원시 값 저장시 먼저 확보해야하는 메모리 공간의 크기를 결정해야 함
- 원시 타입별로 메모리 공간의 크기가 미리 정해져 있음
- 데이터 타입별 크기
    - 숫자(8바이트)
    - 문자열(문자 1개당 2바이트): 0개이상으로 이루어진 문자열, 빈 문자열도 가능(’ ‘)
- 문자열 길이에 의해 메모리 공간의 크기가 변화하는 문자열 타입을 처리하는 법
    - C언어: 하나의 문자를 위한 데이터 타입(char)만이 존재
    - 자바: String 객체로 처리
    - 자바스크립트: 원시 타입인 문자열 타입을 제공 = 변경 불가능
- 문자열의 재할당
    
    ```jsx
    var string = 'Hello';
    console.log(string);  //Hello
    string = 'world';
    console.log(string);  //world
    ```
    
    - 문자열 Hello와 world 모두 메모리에 존재
    - string 변수가 가리키는 셀 주소가 변경됨 → 원시 타입인 문자열이 변경되지 않음
- 문자열의 특징
    
    1) 자바스크립트의 문자열 = 유사 객체 = 이터러블 = 배열과 유사하게 접근 가능
    
    ```jsx
    var str = 'string';
    // 배열과 유사하게 인덱스를 사용해 각 문자에 접근 가능
    console.log(str[0]);  // s
    
    // 원시 값인 문자열이 객체처럼 동작
    console.log(str.length);  //6
    console.log(str.toUpperCase());  //STRING
    ```
    
    2) 이미 생성된 문자열 일부 변경 불가능
    
    - 문자열은 원시 값으로 변경불가능하기 때문에, 재할당만이 가능
    
    ```jsx
    var str = 'string';
    
    // 배열과 유사하게 인덱스를 사용해 각 문자에 접근
    // 하지만 문자열은 원시 값이므로 변경 불가. 이 때 에러발생하지 않음
    str[0] = 'S';
    console.log(str);  //string
    ```
    

### 11.1.2 값에 의한 전달

- A변수에 원시 값을 갖는 B변수를 할당하면, B변수의 값이 복사되어 전달됨 = (새로운 공간에 값이 배치되고 이를 가리킴) = 같은 80이지만, 다른 메모리 공간에 저장된 별개의 값 → 값에 의한 전달

```jsx
var score = 80;
var copy = score;

console.log(score);  //80
console.log(copy);  //80

score = 100;

console.log(score);  //100
console.log(copy);  //80
```

- 엄밀하게) 값에 의한 전달X (메모리 주소에 의한 전달O)
    
    : 변수에는 값이 전달되는게 아니라, 메모리 주소가 전달되고, 즉, 식별자는 메모리 주소이고, 식별자가 기억하고 있는 메모리 주소를 통해 메모리 공간에 저장된 값에 접근 가능해짐
    

→ 메모리 주소에 의해 전달하기에 달라질 수 있는 평가 방식

- 엄밀하게) 서로 다른 메모리 공간에 저장되어 별개의 값이 되는 시점 = 2가지의 평가 방식
    
    1) A변수에 B변수를 할당할 때(변수 할당하는 시점)
    
    2) 할당받은 B변수가 재할당받을 때(할당받을 때에는 같은 주소 가리키다가…)(파이썬도 이처럼 동작)
    
    → 공통점: 재할당되면 서로 간섭할 수 없음
    

## 11.2 객체

객체의 특징

- 객체의 프로퍼티 개수를 동적으로 생성, 삭제 가능
- 프로퍼티 값에 제약 없음 = 미리 데이터 타입에 맞게 메모리 공간을 확보할 수 없음
- 자바스크립트 객테의 관리 방식
    - 프로퍼티 키를 인덱스로 사용하는 해시 테이블과 유사 (단, 더 높은 성능을 위해 더 나은 방법으로 객체를 구현함)
- 객체의 프로퍼티 접근 비용 비교
    - `자바`, `C++`: 은 클래스 기반 객체지향 프로그래밍 언어로 객체(인스턴스)를 생성 + 동적으로 추가, 삭제 불가
    - `자바스크립트` : 클래스 없이 객체 생성 가능 + 동적으로 추가, 삭제 가능
        
        → 편리하지만, 생성과 프로퍼티 접근에 비용이 많이 듦 
        
        → 히든 클래스 방식을 사용해 C++ 객체의 프로퍼티 접근하는 정도의 성능 보장
        

### 11.2.1 변경 가능한 값

- 객체(참조) 타입의 값 = 변경 가능한 값
- 변수에 담긴 메모리 주소를 통해 해당 메모리 공간에 접근하면
    - 원시 값을 할당한 변수: 원시 값 담고 있음
    - 참조 값을 할당한 변수: 참조 값 담고 있음(참조값 = 생성된 객체가 저장된 메모리 주소)
- 원시 값과 객체를 담은 변수의 표현의 차이
    - 변수는 O의 값의 갖는다. = 변수의 값은 O이다. = 변수 안에는 해당 값이 담겨있음
    - 변수는 객체를 참조하고 있다. = 변수는 객체를 가리키고 있다. = 변수 안에는 해당 값을 가리키는 주소가 담겨있음
- 객체 변경시
    - 재할당 없이 객체를 직접 변경(수정) 가능
    - 변수 안의 참조 값 변경 X
    - 메모리 안에 객체가 직접 변경됨
    
    ```jsx
    var person = {
    	name: 'Lee'
    };
    
    // 프로퍼티 값 갱신
    person.name = 'Kim';
    
    // 프로퍼티 동적 생성
    person.address = 'Seoul';
    
    console.log(person); //{name: "Kim", address: "Seoul"}
    ```
    
    - 이유?
        - 원시 값처럼 새롭게 생성하면, 원시 값처럼 크기 일정하지도 않고, 프로퍼티 값도 객체일 수 있어서, 복사해서 생성하는 비용이 많음 듦 → 메모리의 효율적 소비가 어렵고 성능 나빠짐
    - 객체 직접 변경시 장단점
        - 장점) 메모리의 효율성과 성능
        - 단점) 여러 개의 식별자가 하나의 객체 공유 가능 (새로운 메모리 공간이 안만들어지니깐)
- 얕은 복사(shallow copy) VS 깊은 복사(deep copy)
    - 얕은 복사와 깊은 복사 둘다 원본과는 다른 객체 = 참조 값이 다른 별개의 객체
        - 얕은 복사는 객체에 중첩되어 있는 경우 참조 값을 복사
        - 깊은 복사는 객체에 중첩되어 있는 경우 중첩되어 있는 객체까지 모두 복사 = 완전한 복사본
            
            ```jsx
            const o = {x: {y: 1} };
            
            // 얕은 복사
            const c1 = { ... o };
            console.log(c1 === o);  //false
            console.log(c1.x === o.x);  //true (참조 값 === 참조 값)
            
            // 깊은 복사
            // loadash의 cloneDeep을 사용
            // "npm install lodash"로 lodash 설치 후, Node.js 환경에서 실행
            const _ = require('lodash');
            
            const c2 = _.cloneDeep(o);  //false
            console.log(c2 === o);  //false
            console.log(c2.x === o.x);  //false (완전한 복사본인데 왜 다르지?...?)
            
            ```
            
- 다른 의미) 원시 값을 할당한 변수를 다른 변수에 할당하는 것을 깊은 복사, 객체를 할당한 변수를 다른 변수에 할당하는 것을 얕은 복사라고 부르기도…

### 11.2.2 참조에 의한 전달

- 참조 값이 같음 = 같은 주소를 가리킴 = 여러 개의 식별자가 하나의 객체를 공유
- 참조에 의한 전달 = 참조 값이 복사되어 전달
- 부작용) 공유하는 원본 또는 사본 중 하나가 객체를 변경하면 같이 바뀜
    
    ```jsx
    var person = {
    	name: 'Lee'
    }
    
    // 얕은 복사 = 참조 값을 복사
    var copy = person;
    
    // copy와 person은 동일한 객체를 참조
    console.log(copy === person);  //true
    
    // copy를 통해 객체를 변경
    copy.name = 'Kim';
    
    // person을 통해 객체를 변경
    person.name = 'Seoul';
    
    // copy와 person은 동일한 객체를 가리킴
    // 따라서 어느 한쪽에서 객체를 변경하면 서로 영향을 주고 받을 수 있음
    console.log(person);  // {name: "Kim", address: "Seoul"}
    console.log(copy);  // {name: "Kim", address: "Seoul"}
    ```
    
    - ‘값에 의한 전달’과 ‘참조에 의한 전달’은 식별자가 기억하는 메모리 공간에 저장되어 있는 값을 복사해서 전달한다는 면에서 동일
---
# 12장. 함수

## 12.1 함수란?

- 정의: 일련의 과정을 문statement으로 구현하고, 코드 블록으로 감싸서 하나의 실행 단위로 정의한 것
- 구성요소: 매개변수, 인수, 반환 값
    
    ```jsx
    // 함수 정의
    // 함수이름(add), 매개변수(x,y), 반환 값(x+y)
    function add(x, y) {
    	return x + y
    }
    // 인수(2,7)
    add(2, 7)
    ```
    
- 생성 방법: 함수 정의 통해 생성
- 실행 방법: 인수 + 함수 호출
    
    ```jsx
    // 함수 호출
    var result = add(2, 5);
    ```
    

## 12.2 함수를 사용하는 이유

- 장점) 필요시 여러번 호출 가능
    - 실행시점 개발자가 정할 수 있음
    - 재사용 가능
    - 유지 보수의 편리성: 코드 수정 필요시 한번에 고칠 수 있어서 편리
    
    → 코드의 신뢰성 높임
    
- 함수 이름의 가독성: 함수 이름 작명시 함수의 역할을 이해할 수 있게끔 짓자(개발자를 위한 문서임으로)

## 12.3 함수 리터럴

- 자바스크립트의 함수는 객체 타입 → 함수는 함수 리터럴 생성

```jsx
// 변수에 함수 리터럴을 할당
var f = function add(x, y) {
	return x + y
};
```

- 함수 리터럴의 구성요소 (function 키워드, 함수 이름, 매개 변수 목록, 함수 몸체)
    - 함수 이름: 식별자 네이밍 규칙 준수해야, 생략 가능(무기명 or 무명 함수)
    - 매개변수 목록: 0개이상, 매개변수 순서대로 할당(순서에 의미있음), 매개변수도 식별자 네이밍 규칙 준수해야 함.
    - 함수 몸체: 함수 호출 시 하나의 실행단위
- 함수 리터럴 평가되어 값을 생성 → 이 값은 객체 → 함수는 객체다 →(다른 객체와 다르게 호출 가능한) + (함수 객체만의 고유한 프로퍼티를 가짐)
- 함수가 객체라는 사실은 자바스크립트의 중요한 특징

## 12.4 함수 정의

- 호출하기 이전에 인수를 전달받을 매개변수롸 실행한 문들, 반환할 값을 지정하는 것
- 정의된 함수는 자바스크립트 엔진에 의해 평가되어 함수 객체가 됨
- 함수를 정의하는 4가지 방법
    - 함수 선언문
        
        ```jsx
        function add(x, y) {
        return x + y;
        }
        ```
        
    - 함수 표현식
        
        ```jsx
        var add = function (x, y) {
        	return x + y;
        };
        ```
        
    - Function 생성자 함수
        
        ```jsx
        var add = new function('x', 'y', 'return x + y');
        ```
        
    - 화살표 함수(ES6)
        
        ```jsx
        var add = (x, y) => x + y
        ```
        
- 변수 선언과 함수 정의
    - 함수 정의: 함수 선언문이 평가되면 식별자가 암묵적으로 생서하고 함수 객체가 할당됨

### 12.4.1 함수 선언문

- 함수 선언문은 함수 이름을 생략할 수 없음
- 함수 선언문은 표현식이 아닌 문(완료값은 undefined, 함수 호출 전까지는 표현식으로 평가 아직 안됨)
- `console.log` VS `console.dir`
    
    ```jsx
    function Car(make, model) {
      this.make = make;
      this.model = model;
      this.info = function() {
        return this.make + ' ' + this.model;
      };
    }
    
    let myCar = new Car('Toyota', 'Corolla');
    
    console.log(myCar);
    // Car { make: 'Toyota', model: 'Corolla', info: [Function] }
    console.dir(myCar);
    // Car
      info: ƒ ()
      make: "Toyota"
      model: "Corolla"
      __proto__: Object
    ```
    
    - console.dir은 console.log와 달리 함수 프로퍼티(prototype 같은…)까지 출력된다
        - 프르토타입이란?
            
            (= 함수에  메서드 추가하는 행위)
            
            함수 프로토타입(prototype)은 JavaScript에서 객체 지향 프로그래밍의 핵심 개념 중 하나입니다. 모든 JavaScript 함수는 `prototype`이라는 특별한 속성을 가지며, 이 속성은 해당 함수로부터 생성된 객체들이 상속할 수 있는 프로퍼티와 메서드를 담고 있는 객체를 가리킵니다.
            
            함수의 프로토타입 객체는 해당 함수가 생성자로 사용될 때, 그 함수로부터 생성된 모든 객체가 공유하는 프로퍼티와 메서드를 정의합니다. 이를 통해 코드의 재사용성을 높이고 상속을 구현할 수 있습니다.
            
            예를 들어, 다음은 함수를 사용하여 객체를 생성하고 프로토타입을 활용하는 예시입니다:
            
            ```jsx
            // Person 함수 정의
            function Person(name, age) {
              this.name = name;
              this.age = age;
            }
            
            // Person 함수의 프로토타입에 메서드 추가
            Person.prototype.introduce = function() {
              console.log(`Hello, my name is ${this.name} and I am ${this.age} years old.`);
            };
            
            // Person 함수를 사용하여 객체 생성
            let person1 = new Person('Alice', 30);
            let person2 = new Person('Bob', 25);
            
            // 객체들이 프로토타입의 메서드를 공유
            person1.introduce(); // "Hello, my name is Alice and I am 30 years old."
            person2.introduce(); // "Hello, my name is Bob and I am 25 years old."
            
            ```
            
            위 예시에서 `Person.prototype.introduce` 메서드는 `Person` 함수로부터 생성된 모든 객체가 공유하는 메서드입니다. `person1`과 `person2`는 `Person` 함수를 사용하여 생성되었기 때문에 `introduce` 메서드를 사용할 수 있습니다. 이러한 공유된 메서드들을 통해 코드의 중복을 줄이고 객체들 간에 일관된 동작을 구현할 수 있습니다.
            
            따라서 함수 프로토타입은 JavaScript에서 객체 지향 프로그래밍의 상속을 구현하는 중요한 개념으로, 코드의 재사용성과 유지보수성을 높이는 데 도움을 줍니다.
            
- 코드의 문맥에 따라 달라지게 되는 해석 방식: 함수 선언문VS 함수 리터럴
    
    1) 표현식이 아닌 함수 선언문으로 해석
    
    2) 표현식인 함수 리터럴 표현식으로 해서
    
    (유일한 차이점이 함수 이름 생략 가능한지임으로 JS 엔진입장에선 혼동의 여지가 있음)
    
- 이름이 있는(기명) 함수 선언문의 해석
    - 단독 사용시) 함수 선언문으로 해석
    - 함수 리터럴을 변수에 할당하거나 피연산자로 사용시) 함수 리터럴 표현식으로 해석
        
        ```jsx
        // 함수 선언문은 표현식이 아닌 문이므로 변수에 할당할 수 없다
        // 하지만 함수 선언문이 변수에 할당하는 것처럼 보임
        // 이렇게 사용시 함수 리터럴처럼 해석되지 때문
        var add = function add(x, y) {
        	return x + y;
        }
        
        // 함수 호출
        console.log(add(2, 5));  //7
        ```
        
- 함수 선언문으로 해석 VS 함수 리터럴로 해석시 식별자
    
    ```jsx
    // foo는 함수 선언문으로 해석
    function foo() {console.log('foo');}
    foo();  // foo
    
    // bar는 함수 리터럴 표현식으로 해석
    // 그룹 연산자()의 피연산자는 값으로 평가될 수 있는 표현식이어야 함으로
    // 표현식이 아닌 문인 함수 선언문은 피연산자로 사용 못함
    // 그래서 bar를 함수 리터럴인 표현식으로 해석함
    (function bar() { console.log('bar');});
    bar();  //ReferenceError: bar is not defined
    ```
    
- 함수 호출에서의 차이
    - 함수 선언문(foo): 호출 가능
        - 호출 가능하다 = 함수 몸체 외부에서 참조하고 있다
        - 즉, foo는 함수이름이 아니라 함수 객체를 기리키는 식별자여야함
        - JS엔진은 함수호출을 위해 함수 이름과 동일한 이름의 식별자를 암묵적으로 생성하고, 거기에 함수 객체를 할당
    - 함수 리터럴 표현식(bar): 호출 불가능
        - 함수 리터럴에서 함수 이름은 함수 몸체 내에서만 참조할 수 있는 식별자
        - 함수 몸체 외부에서는 함수 이름으로 함수를 참조 할 수 없음 =  함수를 가리키는 식별자가 없다
        - 따라서 함수 리터럴은 호출할수 없다
- 함수는 함수 이름으로 호출하는 것이 아니라 함수 객체를 가리키는 식별자로 호출

```jsx
// 첫번째 add는 식별자, 두번째 add는 함수이름
var add = function add(x, y) {
	return x + y;
};
// 호출되는 add는 식별자
console.log(add(2, 5));  // 7
```

- 단, 함수 선언문과 함수 표현식이 정확히 일치하게 동작하지 않음

### 12.4.2 함수 표현식

- 자바스크립트 함수는 일급 객체이다
    - 값처럼 변수에 할당, 프로퍼티 값이 될 수도 있으며, 배열의 요소가 될 수있음 = 값처럼 자유롭게 사용 가능
    - 함수 호출시에는 함수 이름이 아닌 함수 객체를 가리키는 식별자 사용해야함
        
        ```jsx
        // 기명 함수 표현식
        var add = function foo (x, y) {
        	return x + y;
        }
        
        // 함수 객체를 가리키는 식별자로 호출
        console.log(add(2, 5));  // 7
        
        // 함수 이름으로 호출시 ReferenceError 발생
        // 함수 이름은 함수 몸체 냅에서만 유효한 식별자
        console.log(foo(2, 5));  // ReferenceError: foo is not defined
        ```
        

### 12.4.3 함수 생성 시점과 함수 호이스팅

- 함수 호이스팅이란?
    - 함수 선언문이 코드의 선두로 끌어올려진 것처럼 동작하는 자바스크립트 고유의 특징
    - 코드가 한줄씩 실행되는 런타임(runtime)에는 이미 함수 객체가 생성되어 있고 함수 이름과 동일한 식별자에 할당까지 완료된 상태
    - 따라서 함수 선언문 이전에 함수 참조 또는 호출 가능
- 함수 선언문과 함수 표현식으로 정의한 함수의 생성 시점의 차이
    - 런타임 이전:
        - 함수 선언문) 함수 객체가 생성 + 함수 이름과 동일한 식별자에 할당 완료
        - var 키워드를 사용한 함수 표현식) 실행되어 식별자 생성 + 식별자에 undefined로 초기화
    - 런타임 이후:
        - 함수 선언문) X
        - var 키워드를 사용한 함수 표현식) 식별자에 함수 객체 할당

```jsx
console.dir(add);  // f add(x, y)
console.dir(sub);  // undefined

// 함수 호출
console.log(add(2, 5));  // 7
console.log(sun(2, 5));  // TypeError: sub is not a function

// 함수 선언문
function add(x, y) {
	return x + y;
}

// 함수 표현식
var add = function (x, y) {
	rerturn x - y;
}
```

- 함수 호이스팅은 함수 호출전에 반드시 함수를 선언해야된다는 규칙을 무시한 것 → 함수 표현식을 쓸 것을 권장

### 12.4.4 Function 생성자 함수

- 일반적이지도 않고, 권장하는 방법이 아님
- new 연산자 없이 호출해도 결과 동일
- Function 생성자 함수로 생성한 함수는 함수 선언문이나 함수 표현식으로 생성한 함수와 다르게 동작함(생성자 함수로 생성하면 클로저(closure) 생성 안함)

```jsx
var add = new Function('x', 'y', 'return x + y');
```

### 12.4.5 화살표 함수

- function 키워드 대신 화살표(⇒) 사용
- 익명 함수로 정의함
- 함수 선언문과 함수 표현식에 비래 내부 동작이 간략화되어 있음
    - 화살표 함수는 생성자 함수로 사용할 수 없음
    - 기족 함수와 this 바인딩 방식이 다름
    - prototype 프로퍼티가 없음
    - arguments 객체 생성하지 않늠
    
    → 26.3 에서 더 자세히 살펴보자
    

## 12.5 함수 호출

- 함수 호출시 현재의 실행 흐름을 중단하고 호출된 함수로 실행 흐름을 옮김

### 12.5.1 매개변수와 인수

- 인수란?
    - 함수 실행하기 위해 필요한 값을 함수 외부에서 함수 내부로 전달할 때 사용
    - 인수는 값으로 평가될 수 있는 표현식이어야 함
- 매개변수란?
    - 매개변수는 함수 정의시 선언
    - 함수 몸체 내부에서 변수와 동일하게 취급
    - 함수 호출시, 함수 몸체 내에서 암묵적으로 매개변수가 생성되고 undefined로 초기화된 이후 인수가 순서대로 할당됨
    - 매개변수의 스코프(유효 범위)는 함수 내부임
    - 함수의 매개변수의 개수와 인수의 개수 일치 여부는 상관없음 → 에러 발생하지 않음
        - 인수가 부족할 경우) undefined로 처리됨
        - 인수가 넘치는 경우) 초과된 인수는 함수 호출시에는 버려짐 (= arguments 객체의 프로퍼티로 보관됨)

### 12.5.2 인수 확인

```jsx
function add(x, y) {
	return x + y;
}

console.log(add(2));  // 2 + undefined = NaN
console.log(add('a', 'b');  // 'ab'
```

- 문제점
    - 본래의 의도는 숫자의 합을 나타내고 싶었는데, 오류없이 마음대로 동작
    - 이를 방지하기 위해
- 해결책
    
    1) 적절한 인수가 전달되었는지 확인
    
    ```jsx
    function add(x, y) {
    	if(typeof x !== 'number' || typeof y !== 'number') {
    		throw new TypeError('인수는 모두 숫자 값이어야 함.');
    	}
    	return x + y;
    }
    ```
    
    2) 타입스크립트를 사용해 정적 타입을 선언
    
    3) arguments 객체를 통해 인수 개수 확인
    
    4) 단축평가 사용해 매개변수에 기본값 할당하기
    
    ```jsx
    function add(x, y) {
    	x = x || 0;
    	y = y || 0;
    	return x + y;
    }
    ```
    
    5) ES6에 도입된 매개변수 기본값 사용해 인수 체크 및 초기와 간소화
    
    ```jsx
    function add(x = 0, y = 0) {
    	return x + y;
    }
    ```
    

### 12.5.3 매개변수의 최대 개수

- 매개변수에 최대 개수
    - 명시적으로 제한하진 X
    - 함수의 매개변수는 코드를 이해하는게 방해요소이므로 0개가 가장 좋다 = “함수는 한 가지 일만 해야 하며 가급적 작게 만들어야 한다.”
        - 3개 이상 넘지 않는 것을 권장
        - 3개 이상이 필요하면 객체를 인수로 전달하는 것이 유리
- 매개변수의 순서
    - 매개변수 순서에 의미가 있다. 매개변수의 개수나 순서가 변경되면 함수의 호출 방법이 바뀜.
        
        ```jsx
        function greet(firstName, lastName) {
          console.log(`Hello, ${firstName} ${lastName}!`);
        }
        // 매개변수 순서가 다른 두 함수 호출
        greet('John', 'Doe'); // "Hello, John Doe!"
        greet('Doe', 'John'); // "Hello, Doe John!" (잘못된 결과)
        ```
        
    - 객체 인수로 사용하는 경우 프로퍼티 키만 정확히 지정하면, 매개변수의 순서 신경 안써도 됨
        
        ```jsx
        $.ajax ({
        	method: 'POST',
        	url: '/user',
        	data: { id: 1, name: 'Lee'},
        	cache: false
        });
        ```
        

### 12.5.4 반환문

- 정의
    - return 을 이용한 반환문을 사용해, 실행결과를 함수 외부로 반환 가능
- 역할
    
    1) 함수의 실행을 중단하고 함수 몸체를 빠져나감 → return 문 아래는 무시됨
    
    2) return 키워드 뒤에 오는 표현식을 평가해 반환 → 명시적이지 않으면 undefined
    
- 특징
    - return 키워드와 반환문 동시에 생략 가능 → 암묵적으로 undefined가 반환됨
    - 반환문 생략 가능 → 암묵적으로 undefined가 반환됨
        
        ```jsx
        // 반환문 생략
        function foo () {
        	return ;
        }
        // return + 반환문 생략
        function bar () {
        }
        // 두 경우 모두, 암묵적으로 undefined 반환
        console.log(foo());  // undefined
        console.log(bar());  // undefined
        ```
        
    - return 과 반환값 사이에 줄바꿈있으면, 자동 세미 콜론 자동 삽입에 의해, return 뒤의 반환값이 무시됨.
        
        ```jsx
        function multiply(x, y) {
        	return  // 자동 세미콜론 추가 
        	x + y;
        }
        
        console.log(multiply(3, 5));  // undefined
        ```
        
    - 함수 내부에서만 사용 가능 → 전역에서 사용시 문법 에러
        - 단, node.js 환경에서는 파일별로 독립적인 파일 스코프를 가짐으로, 파일의 바깥 영역에 반환문 사용해도 에러 발생 안함

## 12.6 참조에 의한 전달과 외부 상태의 변경

```jsx
function changeVal(pri, obj) {
	pri += 100;
	obj.name = 'Kim';
}

// 외부 상태
var num = 100;
var person = { name: 'Lee'};

console.log(num);
console.log(person);

// 원시 값은 값 자체가 복사되어 전달되고 객체는 참조 값이 복사되어 전달됨
changeVal(num, person);

// 원시 값은 원본이 훼손되지 않는다
console.log(num);  // 100 (changeVal 내의 num은 200, 외부의 num은 100)

// 객체는 원본이 훼손된다 = 같은 주소를 참조하고 있기에
console.log(person);  // {name: "Kim"}
```

- 문제점
    - 함수가 외부상태 변경하면 상태 변화 추적하기 어려움
    - 코드의 복잡성을 능가시키고 가독성을 해침
    - 객체 변경 추적하려면 옵저버 패턴등을 통해 추가적인 대응 필요
- 해결방법
    - 불변 객체로 만듦 = 원시 값처럼
    - 방어적 복사 = 갚은 복사를 통해 새로운 객체 생성하고 재할당을 통해 교체
    - 이렇게 순수함수로 만들자. (이렇게 외부 상태에 의존하지 않는 함수)

## 12.7 다양한 함수의 형태

### 12.7.1 즉시 실행 함수

- 정의
    - 함수 실행과 동시에 즉시 호출되는 함수
    - IIFE, Immediately invoked Function Expression
    - 단 한번만 호출되며 다시 호출 불가
- 종류
    - 그룹 연산자를 사용
        - 익명 즉시 실행 함수
            - 즉시 실행 함수 일반적으로 익명함수 사용
            - 기명함수도 가능하지만, 다시 호출할 수 없으므로 필요없음
                
                (그룹 연산자 내에 있으면, 함수 선언문이 아닌 함수 리터럴로 평가되기 때문에)
                
                ```jsx
                (function () {
                	var a = 3;
                	var b = 5;
                	return a * b;
                }());
                ```
                
        - 기명 즉시 실행 함수
            
            ```jsx
            (function foo() {
            	var a = 3;
            	var b = 5;
            	return a * b;
            }());
            
            foo();  //ReferenceError: foo is not defined
            ```
            
    - 그룹 연산자 이외의 방식
        
        → 함수 리터럴을 평가해 함수 객체를 생성할 수 있는 다른 방법
        
        ```jsx
        // 그룹 연산자를 이용한 방법
        (function () {
        	// ...
        } ());
        
        // (함수 표현식) + (); 즉시실행
        (function () {
        	// ...
        })();
        
        // !의 피연산자로 함수 객체 생성
        !function () {
        	// ...
        }();
        
        // +의 피연산자로 함수 객체 생성
        +function () {
        	// ...
        }(); 
        ```
        
        - `(function ( ) {…}) ( );` VS `function () {…} ( );` 고찰
            
            `(function foo() {})()`에서 뒤에 나오는 `()`는 실제로 함수를 즉시 실행하기 위해 사용되는 즉시 실행 연산자(Immediate Invocation Operator)입니다. 이 연산자는 함수를 정의한 후 바로 실행하도록 하는 역할을 합니다.
            
            위의 코드에서 각 부분을 살펴보겠습니다:
            
            1. `(function foo() {})`: 이 부분은 함수 표현식을 나타냅니다. `function foo() {}`는 익명 함수를 정의하는 것이고, 이를 함수 표현식으로 사용하기 위해 `( ... )`로 감싸줍니다. 함수 표현식은 변수에 할당할 수도 있고, 다른 표현식의 일부로 사용할 수 있습니다.
            2. `()`: 이 부분은 함수 표현식을 즉시 실행하기 위한 즉시 실행 연산자입니다. 함수 표현식이 평가되면 함수 객체가 생성되고, 이 연산자는 해당 함수 객체를 즉시 실행시킵니다.
            
            따라서 `(function foo() {})()`는 함수 표현식을 정의하고 즉시 실행하는 패턴을 나타냅니다. 이러한 패턴은 즉시 실행 함수(IIFE, Immediately Invoked Function Expression)로 알려져 있으며, 주로 스코프를 생성하거나 변수의 유효 범위를 제한하기 위해 사용됩니다.
            
            뒤에 나오는 `()`는 문법적으로 유효한 구문으로서, JavaScript 엔진은 이를 함수를 즉시 실행하는 연산으로 해석하고 실행합니다. 따라서 `function foo() {}`라는 함수를 정의하고 바로 실행하는 것이 `()`의 역할입니다.
            
- 특징
    - 즉시 실행 함수도 일반 함수와 같이 값을 반환하고 인수를 전달할 수 있음
    - 즉시 실행 함수 내에 코드를 모아 두어 변수나 함수 이름의 충돌을 방지하자
    - 그룹 연산자로 감싸지 않는다면…
        - 익명 함수일때는
            - SyntaxError: Function statements require a funcion name
                
                → 함수 선언문에서는 함수 이름 생략이 불가능하니깐 에러 발생
                
        - 기명 함수일때는
            - SyntaxError: Unexpected token ‘)’
            
            ```jsx
            function foo( ) { … }( ); 
            // 자동 세미콜론 삽입에 의해 
            // function foo() { … }; ( ); 로 해석됨
            // 함수 선언문 + 피연산자가 없는 그룹 연선자 로 해석됨
            // 그룹 연산자에 피연산자가 없기에 에러 발생
            ```
            

### 12.7.2 재귀 함수

- 재귀 호출
    - 함수가 자기 자신을 호출
- 재귀 함수
    - 재귀 호출(자기 자신을 호출하는 행위)를 수행하는 함수
- 반복되는 처리를 위해 사용
- 재귀함수를 사용하면 반복되는 처리를 반복문 없이 구현 가능
- 특징
    - 재귀 함수는 자신을 무한 재귀 호출함 → 재귀 호출을 멈출 수 있는 탈출 조건 필요
    - 함수 내부에서 사용시)
        
        함수 이름은 함수 몸체 내부에서만 유효함으로 함수 내부에서 함수 이름을 이용해 호출 가능
        
        ```jsx
        function factorial(n) {
        	// 탈출조건
        	if (n <= 1) return 1;
        	// 함수 내부에서 함수 이름을 이용해, 재귀 호출
        	return n * factorial(n -1);
        }
        ```
        
    - 함수 외부에서 사용시)
        
        함수를 가리키는 식별자를 이용해  
        
        ```jsx
        var factorial = function foo(n) {
        	if (n <= 1) return 1;
        	return n * facotrial(n -1);
        	// return n * foo(n -1); 도 가능
        }
        console.log(factorial(5));
        ```
        
        - 질문) 함수 외부에서 재귀 호출할 일이 있나요?
        - 답변)
            
            일반적으로 JavaScript에서는 함수 외부에서 함수를 재귀 호출하는 것은 일반적이지 않습니다. 재귀 호출은 주로 함수 내부에서 자기 자신을 호출하여 반복적인 작업을 수행하는 패턴으로 사용됩니다. 함수 외부에서 함수를 재귀 호출하는 경우는 드뭅니다만, 몇 가지 특수한 상황에서는 이런 패턴을 사용할 수 있습니다.
            
            다음은 함수 외부에서 재귀 호출이 발생할 수 있는 경우의 예시입니다:
            
            1. **콜백 함수 내에서**: 비동기 함수에서 사용되는 콜백 함수는 외부에서 재귀 호출될 수 있습니다. 예를 들어, 타이머 함수인 `setTimeout`이나 `setInterval`을 사용하여 비동기 작업을 수행할 때 콜백 함수가 외부에서 재귀 호출될 수 있습니다.
                
                ```jsx
                function doSomething(callback) {
                  setTimeout(function() {
                    // 콜백 함수 내에서 재귀 호출
                    callback();
                  }, 1000);
                }
                
                function recursiveCallback() {
                  console.log('Recursive callback called!');
                  // 외부에서 재귀 호출
                  doSomething(recursiveCallback);
                }
                
                // 초기 호출
                recursiveCallback();
                
                ```
                
            2. **이벤트 핸들러 내에서**: DOM 이벤트 핸들러에서도 외부에서 재귀 호출이 발생할 수 있습니다. 예를 들어, 클릭 이벤트 핸들러에서 함수가 자기 자신을 호출할 수 있습니다.
                
                ```jsx
                const button = document.getElementById('myButton');
                
                function handleClick(event) {
                  console.log('Button clicked!');
                  // 이벤트 핸들러 내에서 재귀 호출
                  button.removeEventListener('click', handleClick); // 이벤트 해제
                  button.addEventListener('click', handleClick); // 이벤트 다시 등록
                }
                
                // 초기 이벤트 등록
                button.addEventListener('click', handleClick);
                
                ```
                
            
            이와 같은 경우들을 제외하고는 보통 함수 외부에서 함수를 재귀 호출하는 것은 권장되지 않습니다. 대부분의 재귀 호출은 함수 내부에서 자기 자신을 호출하여 사용되며, 이는 함수의 로직을 간결하고 자기 참조적인 방식으로 구현할 수 있게 해줍니다.
            
- 단점
    - 무한 반복에 빠질 위험이 있다
    - 탈출 조건 없으면 스택 오버플로에러 발생
    
    → 반복문을 사용하는 것보다 재귀함수를 사용하는게 더 직관적일때만 한정적으로 사용하자
    

### 12.7.3 중첩 함수

- 함수 내부에 정의된 함수를 중첩 함수 또는 내부 함수
- 중첩 함수를 포함하는 함수는 외부 함수
- 중첩 함수는 외부 함수 내부에서만 호출
- 중첩 함수는 자신을 포함하는 외부 함수를 돕는 헬퍼 함수의 역할을 함

```jsx
function outer() {
	var x = 1;

	// 중첩 함수
	function inner() {
		var y = 2;
		// 외부 함수의 변수 참조 가능
		console.log(x + y);  // 3
	}
	inner();
}

outer();
```

- 주의) 호이스팅으로 인해 혼란 발생할 수 있으므로, if 문이나 for 문 등의 코드 블록에서 함수 선언문을 통해 함수를 정의하는 것은 바람직하지 않음.

### 12.7.4 콜백 함수

- 콜백 함수의 필요성
    - 함수의 일부분만 다를 경우, 매번 새롭게 함수를 정의해야 하는 수고를 덜어줌
    - 함수를 합성함으로써 공통로직은 정해주고,  변경되는 로직은 추상화해서 함수 외부에서 함수 내부로 전달
- 예시
    - Before
        
        ```jsx
        function repeat1(n) {
        	for (var i = 0; i < n; i++) console.log(i);
        }
        repeat1(5);  // 0 1 2 3 4
        
        function repeat2(n) {
        	for (var i = 0; i < n; i++) {
        		if (i%2) console.log(i);
        	}
        }
        repeat2(5);  // 1 3
        ```
        
    - After
        
        ```jsx
        // 고차함수 repeat
        // 콜백함수 logAll, logOdds
        
        // 공통 로직
        function repeat(n, func) {
        	for (var i = 0; i < n; i++) {
        			func(i);  // 변경되는 로직은 func으로 추상화
        	}
        
        var logAll = function (i) {
        	console.log(i);
        }
        
        repeat(5, logAll);  // 0 1 2 3 4
        
        var logOdds = function (i) {
        	if (i % 2) console.log(i);
        };
        
        repeat(5, logOdds);  // 1 3
        ```
        
- 콜백 함수란?
    - 함수의 매개변수를 통해 다른 함수로 전달되는 함수를 콜백함수
- 고차 함수란?
    - 매개변수를 통해 함수의 외부에서 콜백 함수를 전달받은 함수를 고차함수
- 고차 함수의 특징
    - 고차함수는 콜백 함수를 자신의 일부분으로 함성함.
    - 고차 함수는 매개변수를 통해 전달받은 콜백 함수의 호출 시점을 결정해 호출한다 (고차함수에 따라 콜백 스택에 담기는 순서가 결정됨)
    - 콜백 함수는 고차 함수에 의해 호출되며 이때 고차함수는 필요에 의해 콜백 함수에 인수를 전달할 수 있다
- 콜백 함수 전달방법 2가지
    - 콜백 함수가 고차 함수 내부에서만 실행된다면 → 익명 함수 리터럴로 정의해 내부에 바로 전달
        
        ```jsx
        // repeat 호출시마다 익명 함수 리터럴은 평가되어 함수 객체를 생성
        repeat(5, function(i) {
        	if(i % 2) console.log(i);
        });  // 1 3
        ```
        
    - 콜백 함수가 다른 곳에서도 호출할 필요가 있고, 자주 호출된다면, 외부에서 콜백함수 정의 후 고차함수에 전달하는 것이 효율적임
        
        ```jsx
        // logOdds 함수는 단 한 번만 생성
        var logOdds = function (i) {
        	if(i % 2) console.log(i);
        };
        // 고차함수에 콜백 함수를 가리키는 함수 참조를 전달
        repeat(5, logOdds);  // 1 3
        ```
        
- 콜백 함수의 활용
    
    1) 함수형 프로그래밍 패러다임(함수를 일급객체로 취급하여 함수의 주요한 구성요소로 사용하는 방식 - 인자를 주고 받는)
    
    2) 비동기 처리(이벤트 처리, Ajax 통신, 타이머 함수등)
    
    ```jsx
    // 콜백함수를 이용한 이벤트 처리
    document.getElementById('myButton').addEventListener('click', function () {
    	console.log('Click!!');
    });
    
    // 콜백함수를 이용한 비동기 처리
    setTimeout(function () {
    	console.log('1초 경과');
    }, 1000);
    ```
    
    3) 배열 고차 함수에서 사용
    
    ```jsx
    // 콜백 함수를 사용하는 고차 함수 map
    var res = [1, 2, 3].map(function (item) {
    	return item * 2;
    });
    
    console.log(res);  // [2, 4, 6]
    
    // 콜백 함수를 사용하는 고차 함수 filter
    res = [1, 2, 3].filter(function (item) {
    	return item % 2;
    });
    
    // 콜백함수를 사용하는 고차 함수 reduce
    res = [1, 2, 3].reduce(function (acc, cur) {
    	return acc + cur;
    }, 0);
    
    console.log(res);  // 6
    ```
    

### 12.7.5 순수 함수와 비순수 함수

- 순수 함수
    - 정의
        - 부수 효과가 없는, 어떤 외부 상태에도 의존하지도 않고 변경하지도 않는
    - 특징
        - 외부 상태에 의존하지 않음 `and` 외부 상태를 변경하지 않음
        - 동일한 인수가 전달되면 언제나 동일한 값을 반환하는 함수
        - 최소 1개 이상의 인수를 받음
            
            (인수를 전달받지 않는 순수 함수는 결국 상수와 마찬가지임으로)
            
        - 인수의 불변성을 유지 = 인수가 변경되지 않음
- 비순수 함수:
    - 정의
        - 부수 효과가 있는, 외부 상태에 의존하거나 외부 상태를 변경하는
        - 외부 상태를 직접 참조하지 않더라도 매개변수를 통해 객체를 전달받으면 비순수함수임
    - 특징
        - 외부상태에 의존 `or` 외부상태를 변경
        - 외부에 의존하지 않더라도, 내부 상태가 호출될 때마다 변화하는 값이면 순수 함수가 아님 (ex: 현재 시간)
        - 외부 상태에는 전역변수, 서버 데이터, 파일, console, DOM 등이 있다
- 함수형 프로그래밍이란?
    - 순수 함수와 보조 함수의 조합을 통해 외부 상태를 변경하는 부수 효과를 최소화해서 불변성을 지향하는 프로그래밍 패러다임
    - 로직 내 조건문과 반복문을 제거하여 복잡성을 해결 → 로직 흐름을 복잡하게 만들어 가독성을 해침
    - 변수 사용을 억제하거나 생명주기를 최소화해서 상태 변경을 피해 오류 최소화 → 누군가에 의해 언제든지 변경될 수 있어 오류 발생의 근본적인 원인이 될 수있기에
