# 19장 프로토타입
- 자바스크립트는 클래스 기반 객체지향 언어이다.
- 자바스크립트를 이루고 있는 "모든 것"이 객체
## 객체지향 프로그래밍
- 객체의 집합으로 프로그램을 표현하려는 프로그래밍 패러다임
- 실체를 프로그래밍에 접목하려는 시도에서 생겨났다.
- 실체는 특징이나 성질을 나타내는 속성을 가지고 있다.
    - ex) 사람 : 이름, 주소, 성별 나이, 신장, 체중
- 프로그램에서 다양한 속성 중에 필요한 속상만 간추려 내어 표현하는 것을 추상화라고 한다.
- 객체지향 프로그래밍은 객체의 상태(state)를 나타내는 데이터와 상태 데이터를 조작할 수 있는 동작(be-havior)을 하나의 논리적인 단위로 묶어서 생각한다.
- 객체는 상태 데이터와 동작을 하나의 논리적인 단위로 묶은 복합적인 자료구조
    - 상태 데이터 : 프로퍼티
    - 동작 : 메서드
```javascript
const circle = {
    radius: 5,

    // 원의 지름
    getDiameter() {
        return 2 * this.radius;
    }
}
```
## 상속과 프로토타입
- 상속 : 어떤 객체의 프로퍼티 또는 메서드를 다른 객체가 상속받아 그대로 사용이 가능한 것
    - 코드 재사용으로 개발 비용을 현저히 줄일 수 있다.
- 자바스크립트는 프로토타입을 기반으로 상속을 구현한다.
```javascript
function Circle(radius) {
    this.radius = radius;
    this.getArea = function () {
        return Math.PI * this.radius ** 2;
    }
}

const circle1 = new Circle(1);
const circle2 = new Circle(2);

console.log(circle1.getArea === circle2.getArea); // false
```
- 위의 코드에서 circle1과 circle2의 getArea는 내용은 동일하나 메서드가 중복 생성된다.
- 이는 메모리를 불필요하게 낭비하고 인스턴스 생성마다 메서드를 생산해 퍼포먼스에도 악역향을 준다.
```javascript
function Circle(radius) {
    this.radius = radius;
}

Circle.prototype.getArea = function () {
    return Math.PI * this.radius ** 2;
};

const circle1 = new Circle(1);
const circle2 = new Circle(2);

console.log(circle1.getArea === circle2.getArea); // true
```
- 하지만 프로토타입을 통해 상속을 구현한다면 해결 가능하다.
- 위 코드에서는 getArea메서드는 Circle.prototype의 메서드로 할당되어 있다.
- Circle 인스턴스를 생성하면 getArea 메서드를 상속받아 사용 가능하다.
## 프로토타입 객체
- 객체 간 상속을 구현하기 위해 사용된다.
- 부모 객체의 역할을하는 객체로써 다른 객체에 공유 프로퍼티를 제공한다.
- 모든 객체는 생성될 때 객체 생성 방식에 따라 프로토타입이 결정되며 이를 [[Prototype]] 내부 슬롯에 저장한다.
    - ex) 객체 리터럴에 의해 생성된 객체의 프로토타입 : Object.prototype
    - 생성자 함수에 의해 생성된 객체의 프로토타입 : 생성자 함수의 prototype 프로퍼티에 바인딩되어 있는 객체
- 객체와 프로토타입과 생성자 함수는 서로 연결되어 있다.
![](https://i.ibb.co/xz71CCY/image.png)
- &lowbar;&lowbar;proto&lowbar;&lowbar; 접근자 프로퍼티를 통해 [[Prototype]] 내부 슬롯이 가리키는 프로토타입에 간접적으로 접근가능
- 자신의 constructor 프로퍼티를 통해 생성자 함수에 접근 가능
- 생성자 함수는 prototype 프로퍼티를 통해 프로토타입에 접근 가능
## __proto__접근자 프로퍼티
- &lowbar;&lowbar;proto&lowbar;&lowbar;는 접근자 프로퍼티다.
    - 내부 슬롯은 프로퍼티가 아니므로 직접 접근이 불가능하지만 &lowbar;&lowbar;proto&lowbar;&lowbar; 접근자 프로퍼티를 통해 간접적으로 접근이 가능하다.
    - 접근자 프로퍼티는 자체적으로 값을 갖지 않고 데이터 프로퍼티의 값을 읽거나 저장([[Get]], [[Set]] 프로퍼티 어트리뷰트)할 때 사용하는 접근자 함수이다.
    - 프로토타입에 getter함수를 통해 값에 접근하며 setter 함수를 통해 값을 할당한다.
- &lowbar;&lowbar;proto&lowbar;&lowbar; 접근자 프로퍼티는 상속을 통해 사용된다.
    - &lowbar;&lowbar;proto&lowbar;&lowbar; 접근자 프로퍼티는 Object.prototype의 프로퍼티
    - 모든 객체는 상속을 통해 Object.prototype.&lowbar;&lowbar;proto&lowbar;&lowbar; 접근자 프로퍼티를 사용할 수 있다.
- &lowbar;&lowbar;proto&lowbar;&lowbar; 접근자 프로퍼티를 통해 프로토타입에 접근하는 이유
    - 상호 참조에 의해 프로토타입 체인이 생성되는 것을 방지하기 위해서이다.
    ```javascript
    const parent = {};
    const child = {};

    child.__proto__ = parent;
    parent.__proto__ = child;
    ```
    - 서로가 자신의 프로토타입이 되는 비정상적인 프로토타입 체인이 만들어져 &lowbar;&lowbar;proto&lowbar;&lowbar;접근자 프로퍼티는 에러가 발생한다.
    - 프로토타입은 단방향 링크드 리스트로 구현되어야한다.
    - 순환 참조 프로토타입 체인이 만들어지면 종점이 존재하지 않아 무한 루프에 빠진다.
    - 아무런 체크 없이 무조건적으로 프로토타입을 교체할 수 없도록 &lowbar;&lowbar;proto&lowbar;&lowbar; 접근자 프로퍼티를 통해 프로토타입에 접근하고 교체하도록 구현되어있다.
- &lowbar;&lowbar;proto&lowbar;&lowbar; 접근자 프로퍼티를 코드 내에서 직접 사용하는 것은 권장하지 않는다.
    - 직접 상속과 같이 모든 객체가 Object.prototype을 상속받진 않기 때문에 모든 객체가 &lowbar;&lowbar;proto&lowbar;&lowbar; 접근자 프로퍼티를 사용할 수 있는 것은 아니다.
    - 따라서 &lowbar;&lowbar;proto&lowbar;&lowbar; 접근자 프로퍼티 대신 프로토타입의 참조를 취득하고 싶다면 Object.getPrototypeOf 메서드를 사용하고 프로토타입을 교체하고 싶다면 Object.setPrototypeOf 메서드를 사용할 것을 권장한다.
    ```javascript
    const obj = {};
    const parent = { x: 1};

    Object.getPrototypeOf(obj); // obj.__proto__;

    Object.getsetPrototypeOf(obj, parent); //obj.__proto__ = parent;

    console.log(obj.x); // 1
    ```
    ## 함수 객체의 prototype 프로퍼티
    - 일반 객체는 소유하지 않고 함수 객체만이 소유하며 생성자 함수가 생성할 인스턴스의 프로토타입을 가리킨다.
    - 생성자 함수로서 호출할 수 없는 함수(화살표함수, ES6 메서드 축약 표현으로 정의한 메서드)는 prototype 프로퍼티를 소유하지 않으며 프로토타입도 생성하지 않는다.
    ```javascript
    const Person = name => {
        this.name = name;
    } // 화살표 함수 Person은 prototype 프로퍼티를 소유하지 않으며 프로토타입을 생성하지 않는다.

    const obj = {
        foo() {} //ES6 메서드 축약 표현으로 정의한 메서드 obj.foo는 prototype 프로퍼티를 소유하지 않으며 프로토타입을 생성하지 않는다.
    }; 

    ```
    - 모든 객체(Object.prototype으로 상속받은)가 가지고 있는 &lowbar;&lowbar;proto&lowbar;&lowbar;접근자 프로퍼티와 함수 객체만이 가지고 있는 prototype 프로퍼티는 결국 동일한 프로토타입을 가진다.
    ```javascript
    function Person(name) {
        this.name = name;
    }

    const me = new Person('Lee');

    console.log(Person.prototype === me.__proto__); // true
    ```
    ![](https://i.ibb.co/D4HxGNF/image.png)
- 프로토타입의 constructor 프로퍼티와 생성자 함수
    - 모든 프로토타입이 가지며 prototype 프로퍼티로 자신을 참조하고 있는 생성자 함수를 가리킨다.
    ```javascript
    function Person(name) {
        this.name = name;
    }

    const me = new Person('Lee');

    console.log(me.constructor === Person);  // true
    ```
    - Person 생성자 함수가 me 객체를 생성할 때 me 객체는 프로토타입의 constructor 프로퍼티를 통해 생성자 함수와 연결된다.
    - me 객체에 contructor 프로퍼티가 있는 것이 아닌 me 객체에 프로토타입인 Person.prototype에 contructor가 있다.
    - me 객체는 이를 상속받아 사용한다.
## 리터럴 표기법에 의해 생성된 객체의 생성자 함수와 프로토타입
- constructor 프로퍼티가 가리키는 생성자 함수는 인스턴스를 생성한 생성자 함수다.
```javascript
const obj = new Object();
console.log(obj.constructor === Object); //true

const add = new Function('a', 'b', 'return a + b');
console.log(add.constructor === Function); // true

fuction Person(name) {
    this.name = name;
}

const me = new Person('Lee');
console.log(me.construcotr === Person); //true
```
- 하지만 명시적으로 new 연산자와 함께 생성자 함수를 호출해 인스턴스를 생성하지 않는 객체 생성방식도 있다. ex) 리터럴
- 리터럴 표기법에 의해 생성된 객체의 경우 프로토타입의 contructor 프로퍼티가 가리키는 생성자 함수가 반드시 객체를 생성한 생성자 함수라고 단정하기 어렵다.
```javascript
let obj = {};

console.log(obj.constructor === Object); // true
```
- 위 코드에서 객체 리터럴에 의해 생성된 객체가 Object 생성자 함수와 연결되어 있는 것은 Object 생성자 함수에 인수를 전달하지 않거나 undefined 또는 null을 인수로 전하며 호출하면 내부적으로 추상연산 OrdinaryObjectCreate를 호출하여 Object.prototype을 프로토타입으로 갖는 빈 객체를 생성하기 때문이다.
- 하지만 객체 리터럴의 평가는 OrdinaryObjectCreate를 호출하여 빈 객체를 생성하는 점에선 동일하나 new.target의 확인이나 프로퍼티를 추가하는 처리 등 세부 내용이 다르다.
- 따라서 객체 리터럴에 의해 생성된 객체는 Object 생성자 함수가 생성한 객체가 아니다.
## 프로토타입의 생성 시점
- 프로토타입은 생성자 함수가 생성되는 시점에 더불어 생성된다.
    - 프로토타입과 생성자함수는 단독으로 존재할 수 없고 쌍으로 존재하기 때문
- 사용자 정의 생성자 함수와 프로토타입 생성 시점
    - non-constructor가 아닌 contructor는 new 연산자와 함께 호출이 가능하다.
    - contructor는 함수 정의가 평가되어 함수 객체를 생성하는 시점에 프로토타입도 더불어 생성된다.
    - 함수 선언문은 런타임 이전에 자바스크립트 엔진에 의해 먼저 실행되므로 이때 프로토타입도 더불어 생성된다.
    - 생성된 프로토타입은 Person생성자 함수의 prototype 프로퍼티에 바인딩된다.
    - 생성된 프로토타입은 오직 constructor 프로퍼티만을 갖는 객체이다.
    - 생성된 프로토타입의 프로토타입은 언제나 Object.prototype이다.
- Object, String, Number, Funtion...빌트인 생성자 함수와 프로토타입 생성 시점
    - 빌트인 생성자 함수 또한 함수 생성 시점에 생성된다.
    - 모든 빌트인 생성자 함수는 전역 객체가 생성되는 시점에 생성된다.
- 객체가 생성되기 이전에 생성자 함수와 프로토타입은 이미 객체화되어 존재한다.
- 이후에 객체를 생성하면 프로토타입은 생성된 객체의 [[Prototype]] 내부 슬롯에 할당되는 것이다.
## 객체 생성 방식과 프로토타입 결정
- 객체 리터럴에 의해 생성된 객체의 프로토타입
    - 리터럴로 생성 시 추상 연산 OrdinaryObjectCreate를 호출한다.
    - 이때 OrdinaryObjectCreate에 전달되는 프로토타입은 Object.prototype이다.
    - 따라서 객체 리터럴에 의해 생성되는 객체의 프로토타입은 Object.prototype 이다.
- Obejct 생성자 함수에 의해 생성된 객체의 프로토타입
    - Object 생성자 함수를 인수 없이 호출하면 빈 객체가 생성되며 이는 객체 리터럴과 마치 OrdinaryObjectCreate를 호출한다.
    - 따라서 Object.prototype을 상속받는다.
    - 객체 리터럴과의 차이는 객체 리터럴은 내부에 프로퍼티를 추가하지만 Object 생성자 함수 방식은 빈 객체를 생성한 후 프로퍼티를 추가해야한다.
- 생성자 함수에 의해 생성된 객체의 프로토타입
    - 생성자 함수 또한 OrdinaryObjectCreate가 호출되지만 OrdinaryObjectCreate에 전달되는 프로토타입은 생성자 함수에 prototype 프로퍼티에 바인딩 되어있는 객체이다.
    - 따라서 생성자 함수에 의해 생성되는 객체의 프로토타입은 prototype 프로퍼티에 바인딩 되어있는 객체이다.
    - 이때 생성된 프로토타입 객체.prototype의 프로퍼티는 contructor뿐이다.
## 프로토타입 체인
```javascript
fuction Person(name) {
    this.name = name;
}

Person.prototype.sayHello = function() {
    console.log(`Hi! My name is ${this.name}`);
};

const me = new Person('Lee');

console.log(me.hasOwnProperty('name')); // true
```
- Person 생성자 함수에 의해 생성된 me 객체는 Object.prototype의 메서드 hasOwnProperty를 호출할 수 있다.
- 이는 Object.prototype도 상속 받았다는 것을 알 수 있다.
- 자바스크립트는 객체의 프로퍼티에 접근하려 할때 해당 객체에 접근하려는 프로퍼티가 없다면 [[Prototype]] 내부 슬롯의 참조를 따라 자신의 부모 역할을 하는 프로토타입의 프로퍼티를 순차적으로 검색한다.
- 이를 프로토타입 체인이라고 하며 이는 자바스크립트가 객체지향의 상속을 구현하는 매커니즘이다.
- 체인의 최상위 객체는 Object.prototype이며 모든 객체는 이를 상속받는다.
- 따라서 Object.prototype은 체인의 종점이라 한다.
- 이때 식별자 검색을 위한 스코프 체인은 프로토타입 체인과 연관없이 동작하는 것이 아닌 서로 협력해 식별자와 프로퍼티를 검색하는데 사용한다.
## 오버라이딩과 프로퍼티 섀도잉
- 프로토타입이 소유한 프로퍼티를 프로토타입 프로퍼티, 인스턴스가 소유한 프로퍼티를 인스턴스 프로퍼티라고 한다.
- 만약 프로토타입 프로퍼티와 같은 이름의 프로퍼티를 인스턴스에 추가하면 덮어쓰는 것이 아닌 인스턴스 프로퍼티로 추가한다.
- 이때 인스턴스 메서드는 프로토타입 메서드를 오버라이딩했고 프로토타입 메서드는 가려진다.
- 이를 프로퍼티 섀도잉 이라고 한다.
- 이는 프로퍼티를 삭제하는 것도 마찬가지다.
![](https://i.ibb.co/ZVbZjwL/image.png)
## 프로토타입의 교체
- 생성자 함수에 의한 프로토타입의 교체
    ```javascript
    const Person = (function()  {
        function Person(name) {
            this.name = name;
        }

        // prototype 프로퍼티를 통해 프로토타입을 새로운 객체로 교체
        Person.prototype = {
            sayHello() {
                console.log(`Hi! My name is ${this.name}`);
            }
        }
        return Person;
    }());

    const me = new Person('Lee');

    console.log(me.constructor === Person); // false
    console.log(me.constructor === Object); // true
    ```
    - 이때 교체한 객체 리터럴에는 constructor 프로퍼티가 없다.
    - 따라서 이 방법으로는 constructor 프로퍼티와 생성자 함수 간의 연결이 파괴된다.
    ```javascript
    const Person = (function()  {
        function Person(name) {
            this.name = name;
        }

        // 객체 리터럴에 constructor 프로퍼티를 추가해 되살린다.
        Person.prototype = {
            constructor: Person,
            sayHello() {
                console.log(`Hi! My name is ${this.name}`);
            }
        }
        return Person;
    }());

    const me = new Person('Lee');

    console.log(me.constructor === Person); // false
    console.log(me.constructor === Object); // true
    ```
- 인스턴스에 의한 프로토타입의 교체
    - 인스턴스의 &lowbar;&lowbar;proto&lowbar;&lowbar;접근자 프로퍼티를 통해 프로토타입을 교체할 수 있다.
    - 이는 이미 생성된 객체의 프로토타입을 교체하는 것을 의미한다.
    ```javascript
     function Person(name) {
        this.name = name;
     }
    
    const me = new Person('Lee');

    // 프로토타입으로 교체할 객체
    const parent = {
        // constructor과의 연결 설정
        constructor: Person,
        sayHello() {
            console.log(`Hi My name is ${this.name}`);
        }
    };

    // 생성자 함수의 prototype 프로퍼티와 프로토타입 간의 연결 설정
    Person.prototype = parent;

    // me 객체의 프로토타입을 parent 객체로 교체
    Object.setPrototypeOf(me, parent);

    me.sayHello(); // Hi! My name is Lee

    console.log(me.constructor === Person); // true;
    console.log(me.constructor === Object); // false;
    ```
    - 이처럼 프로토타입 교체는 매우 번거롭기 때문에 상속 관계를 인위적으로 변경하려면 직접 상속이 더 편리하고 안전하다.
    ## instanceof 연산자
    - 이항 연산자, 좌변에 객체를 가리키는 식별자
    - `객체를 가리키는 식별자 instanceof 생성자 함수를 가리키는 식별자`
    - 우변의 생성자 함수의 prototype에 바인딩된 객체가 좌변의 객체에 프로토타입 체인상에 존재하면 true, 아니면 false를 반환한다.
    ```javascript
     function Person(name) {
        this.name = name;
     }
    
    const me = new Person('Lee');

    const parent = {};

    Object.setPrototypeOf(me, parent);

    console.log(Person.prototype === parent); // false
    console.log(parent.constructor === Person); // false

    Person.prototype = parent;

    // 프로토타입의 constructor 프로퍼티가 가리키는 생성자 함수를 찾는것이 아닌 생성자 함수의 prototype에 바인딩된 객체가 프로토타입 체인 상에 존재하는지 확인한다.
    console.log(me instanceof Person); // true
    console.log(me instanceof Object); // true

    ```
    ## 직접 상속
    - Object.create에 의한 직접상속
        - 첫번째 매개변수 : 생성할 객체의 프로토타입으로 지정할 객체
        - 두번째 매개변수 : 생성할 객체의 프로퍼티 키와 프로퍼티 디스크립터 객체로 이루어진 객체
        - 두번째는 생략 가능
        ```javascript
        let obj = Object create(null);
        console.log(Object.getPrototypeOf(obj) === null) // true

        obj = Object.create(Object.prototype);
        console.log(Object.getPrototypeOf(obj) === Object.prototype) // true

        obj = Object.create(Object.prototype, {
            x: { value: 1, writable: true, enumerable: true, configuable: true}
        });

        console.log(obj.x); // 1
        ```
        - Object.create의 장점
            - new 연산자가 없이도 객체를 생성 가능하다.
            - 프로토타입을 지정하면서 객체를 생성할 수 있다.
            - 객체 리터럴에 의해 생성된 객체도 상속 받을 수 있다.
    - 객체 리터럴 내부에서 &lowbar;&lowbar;proto&lowbar;&lowbar;에 의한 상속
        - Object.create은 두번 째 인자로 프로퍼티를 정의하는 것이 번거롭다.
        - ES6에서는  &lowbar;&lowbar;proto&lowbar;&lowbar;접근자 프로퍼티를 사용해 직접 상속을 구현할 수 있다.
        ```javascript
        const myProto = { x: 10 };

        const obj = {
            y: 20,
            __proto__: myProto
        };

        console.log(obj.x, obj.y); // 10 20
        ```
## 정적 프로퍼티/메서드
- 생성자 함수로 인스턴스를 생성하지 않아도 참조/호출할 수 있는 프로퍼티/메서드를 말한다.
```javascript
     function Person(name) {
        this.name = name;
     }
    
    Person.staticProp = 'static prop';

    Person.staticMethod = function() {
        console.log('staticMethod');
    }

    Person.staticMethod(); //staticMethod
```
- 정적 프로퍼티/메서드는 인스턴스의 프로토타입 체인에 속한 객체의 프로퍼티/메서드가 아니므로 인스턴스로 접근이 불가능하다.
## 프로퍼티 존재 확인
- in 연산자
    - 객체 내에 특정 프로퍼티가 존재하는지 확인한다.
    - key(프로퍼티 키를 나타내는 문자열) in object(객체로 평가되는 표현식)
    ```javascript
    const person = {
        name: 'Lee',
        address: 'Seoul'
    };

    console.log('name' in person); // true
    ```
    - 확인 대상 객체가 상속받은 모든 프로토타입의 프로퍼티를 확인한다.
- Object.prototype.hasOwnProperty 메서드
    ```javascript
    console.log(person.hasOwnProperty('name')); // true
    ```
    - 위 메서드는 상속받은 프로토타입의 프로퍼티 키는 false를 반환한다.
## 프로퍼티 열거
- for...in 문
    - 객체의 모든 프로퍼티를 순회하며 열거하려면 for...in문을 쓴다.
    - `for (변수선언문 in 객체) {...}`
    ```javascript
    const person = {
        name: 'Lee',
        address: 'Seoul'
    };

    for (const key in person) {
        console.log(key + ': ' + person[key]);
    }
    // name: Lee
    // address: Seoul
    ```
    - 객체의 프로퍼티 개수만큼 순회한다.
    - key에 해당 프로퍼티 키를 할당한다.
    - 상속받은 프로토타입의 프로퍼티까지 열거한다.
        - 하지만 열거할 수 없도록 정의되어있는 프로퍼티([[Enumerable]]이 false인 경우)는 열거되지 않는다.
    - 배열에는 for문이나 for...of를 권장한다.
- Object.keys/values/entries 메서드
    - for...in은 상속받은 프로퍼티도 열거하기 때문에 객체 자신의 프로퍼티인지 확인하는 추가 처리가 필요하다.
    - 따라서 고유 프로퍼티만 열거하려면 Obejct.keys/values/entries 메서드를 쓰는 것이 좋다.
    ```javascript
    const person = {
        name: 'Lee',
        address: 'Seoul',
        __proto__: {age : 20}
    };

    console.log(Object.keys(person)); // ["name", "address"]

    console.log(Object.values(person)); // ["Lee", "Seoul" ]

    console.log(Object.entries(person));  // ["name", "Lees"], ["address", "Seoul" ]
    ```
# 20장 strict mode
## strict mode란?
- 암묵적 전역은 개발자의 의도와 상관없이 오류를 일으킨다.
- 따라서 잠재적인 오류를 발생시키기 어려운 환경을 만들어 개발하는 것이 좋다
- 그 환경을 만들어주는 것이 strict mode(엄격 모드)이다.
- 자바스크립트 엔진의 최적화 작업에 문제를 일으킬 수 있는 코드에 대해 명시적 에러를 일으킨다.
## strict mode의 적용
- 전역의 선두 또는 함수 몸체의 선두에 'use strict';을 추가한다.
- 선두에 위치시키지 않으면 제대로 동작하지 않는다.
## 전역에 strict mode를 적용하는 것은 피하자
- 전역에 작용한 strict mode는 스크립트 단위로 적용된다.
- 스크립트 단위로 적용된 strict mode는 다른 스크립트에 영향을 주지 않아 non-strict mode와 strict mode가 혼용될 수 있다.
- 이는 오류를 발생시킬 수 있으며 이러한 경우 즉시 실행함수로 스크립트 전체를 감싸 스코프를 구분하고 즉시 실행함수의 선두에 strict mode를 적용한다.
## 함수 단위로 strict mode를 적용하는 것도 피하자
- 위와 같이 혼용되는 것은 바람직하지 않기 때문에 즉시 실행 함수로 감싼 스크립트 단위로 적용하는 것이 바람직하다.
## strict mode가 발생시키는 에러
- 암묵적 전역
    - 선언하지 않은 변수를 참조하면 참조에러가 발생한다.
- 변수 함수 매개변수의 삭제
    - delete 연산자로 변수, 함수, 매개변수를 삭제하면 Syntax에러가 발생한다.
- 매개변수 이름의 중복
    - 중복된 매개변수 이름을 사용하면 SyntaxError가 발생한다.
- with 문의 사용
    - with문을 사용하면 SyntaxError가 발생한다.
    - with문은 전달된 객체를 스코프 체인에 추가한다.
    - 이는 객체의 프로퍼티를 반복해서 사용할 때 객체 이름을 생략 가능해 코드가 간단해지지만 성능과 가독성 문제가 있어 with문은 사용하지 않는 것이 좋다.
## strict mode 적용에 의한 변화
- 일반 함수의 this
    - stirct mode에서 함수를 일반 함수로 호출하면 this에 undefined가 바인딩 된다.
- arguments 객체
    - 매개변수에 전달된 인수를 재할당하여 변경해도 argument 객체에 반영되지 않는다.