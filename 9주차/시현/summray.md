# 26장 ES6 함수의 추가 기능

## **함수의 구분**

ES6 이전의 모든 함수는 일반함수로서 호출할 수 있는것은 생성자 함수로서 호출할 수 있다. 

```jsx
var foo = function () {
  return 1;
};

// 일반적인 함수로서 호출
foo(); // -> 1

// 생성자 함수로서 호출
new foo(); // -> foo {}

// 메서드로서 호출
var obj = { foo: foo };
obj.foo(); // -> 1
```

ES6 이전의 모든 함수는 사용 목적에따라 명확한 구분이 없으므로 호출 방식에 특별한 제약이 없고 생성자 함수로 호출되지 않아도 프로토타입 객체를 생성한다. → 실수, 성능 X

- ES6
    
    함수를 사용 목적에 따라 세가지 종류로 구분
    

일반함수 : constructor, prototype, arguments

**메서드** : non-constructor, super, arguments

**화살표 함수** : non-constructor

## 메서드

ES6이전에는 일반적으로 메서드는 객체에 바인딩 된 함수를 일컫는 의미로 사용되었다. 

하지만 ES6 사양 부터는 메서드는 메서드 축약표현으로 정의된 함수만을 의미한다.

```jsx
const obj = {
  x: 1,
  // foo는 메서드이다.
  foo() { return this.x; },
  // bar에 바인딩된 함수는 메서드가 아닌 일반 함수이다.
  bar: function() { return this.x; }
};
```

ES6 메서드는 인스턴스를 생성할 수 없는 non-constructor 이다.

ES6 메서드는 자신을 바인딩한 객체를 가리키는 내부 슬롯 `[[HomeObject]]` 를 갖는데, super 참조는 내부 슬롯 `[[HomeObject]]`를 사용하여 수퍼클래스의 메서드를 참조하므로 내부 슬롯을 갖는 메서드는 super 키워드를 사용할 수 있다.

## 화살표 함수

function 키워드 대신 화살표를 사용해서 기존의 함수 정의 방식보다 간략하게 함수를 정의할 수 있다.

### **화살표 함수 정의**

함수 선언문말고 함수 표현식으로 정의할 수 있다.

```jsx
const multiply = (x, y) => x * y;
multiply(2, 3); // -> 6
```

- 매개변수 선언
    - `()` 안에 매개변수 선언
- 함수 몸체 정의
    - 여러개의 문으로 된 경우에는 `{}`로 감싸주고 반환값이 있으면 return을 적어줘야 한다.
    - 함수 몸체 내부의 문이 값으로 평가될 수 있는 표현식인 문이라면 암묵적으로 반환된다.
    - 화살표 함수는 일급객체이므로 map, filter, reduce 같은 고차함수에 인수로 전달할수 있다. (콜백함수로 정의할때 유용)
    
    ```jsx
    // ES5
    [1, 2, 3].map(function (v) {
      return v * 2;
    });
    
    // ES6
    [1, 2, 3].map(v => v * 2); // -> [ 2, 4, 6 ]
    ```
    

### **화살표 함수와 일반 함수의 차이**

**1) 화살표함수는 인스턴스를 생성할수 없는 non-constructor이다.**

화살표함수는 인스턴스를 생성할수 없으므로 prototype프로퍼티가없음

**2) 중복된 매개변수 이름을 선언할 수 없다.**

일반함수는 매개변수이름을 중복해서 사용할수있지만, 화살표함수에서 이렇게하면 에러발생함

**3) 화살표함수는 함수 자체의 this, argument, super, new.target 바인딩을 갖지않는다.**

화살표함수 내부에서 this, argument, super, new.target을 참조하면 스코프체인을 통해 상위 스코프의 this, argument, super, new.target를 참조한다.

(화살표함수가 중첩되어있을시 스코프체인상 가장 가까운 상위함수 중 화살표함수가 아닌 함수를 참조한다.)

### this

화살표함수의 this는 일반함수의 this와 다르게 동작한다.

this 바인딩은 함수를 호출한 방식에 따라 동적으로 결정되는데, 고차함수의 경우 인수로 전달되어 고차함수 내부에서 호출되는 **콜백함수를 일반함수로서 호출**하게 되면 콜백함수내부의 this는 전역객체를 가리킨다. 그래서, **콜백함수내부의 this문제와 외부함수의 this가 서로 다른 값**을 가리키게 되는 문제가 발생한다.

이러한 문제를 해결하기위해 의도적으로 설계된것이 화살표함수이다.

화살표함수는 함수 자체의 this바인딩을 갖지않기때문에 상위스코프의 this를 그대로 참조하는데, 이를 **lexical this**라고 한다.

- 프로토타입 객체의 프로퍼티에 화살표함수 할당 X
- 메서드를 화살표함수로 정의 X

프로퍼티에 할당한 화살표함수의 this는 스코프체인상에서 가장 가까운 상위함수의 this를 참조하게 된다.

```jsx
const person = {
  name: 'Lee',
  sayHi: () => console.log(`Hi ${this.name}`) // 전역 this
  sayHi() {
    console.log(`Hi ${this.name}`);
  }
};

// 이 예제를 브라우저에서 실행하면 화살표 함수의 this가 전역 this이므로 빈 문자열을 갖는 window.name과 같다.
```

```jsx
class Person {
  constructor() {
    this.name = 'Lee';
    this.sayHi = () => () => console.log(`Hi ${this.name}`);
  }
}
```

상위 스코프는 클래스 외부이나 this는 클래스 외부의 this를 참조하지 않고 **클래스가 생성할** **인스턴스**를 참조한다. 따라서, 클래스필드에 할당한 화살표 함수 내부에서 참조한 this는 constuctor 내부의 this 바인딩과 같다. 하지만, 클래스 필드에 할당한 화살표 함수는 프로토타입 메서드가 아니라 **인스턴스 메서드**가 된다. 

### super

화살표함수는 함수자체의 super바인딩을 갖지않는다.

화살표 함수 내에서 super를 참조하면 this와 마찬가지로 상위스코프의 super를 참조한다.

```jsx
class Base {
  constructor(name) {
    this.name = name;
  }

  sayHi() {
    return `Hi! ${this.name}`;
  }
}

class Derived extends Base {
  sayHi = () => `${super.sayHi()} how are you doing?`;
}

const derived = new Derived('Lee');
console.log(derived.sayHi()); // Hi! Lee how are you doing?
```

화살표 함수는 함수 자체의 super 바인딩을 갖지 않으므로 super을 참조해도 에러 발생 X

constructor 의 super 바인딩 참조

### **arguments**

화살표 함수는 함수 자체의 arguments 바인딩을 갖지 않는다.

그래서 화살표함수내의 arguments는 상위스코프의 arguments를 참조한다.

```jsx
(function () {
  // 화살표 함수 foo의 arguments는 상위 스코프인 즉시 실행 함수의 arguments를 가리킨다.
  const foo = () => console.log(arguments); // [Arguments] { '0': 1, '1': 2 }
  foo(3, 4);
}(1, 2));

// 화살표 함수 foo의 arguments는 상위 스코프인 전역의 arguments를 가리킨다.
// 하지만 전역에는 arguments 객체가 존재하지 않는다. arguments 객체는 함수 내부에서만 유효하다.
const foo = () => console.log(arguments);
foo(1, 2); // ReferenceError: arguments is not defined
```

화살표 함수로 가변 인자 함수를 구현해야 할 때는 반드시 Rest 파라미터를 사용해야 한다. 

## Rest 파라미터

### 기본 문법

rest파라미터(나머지 매개변수)는 매개변수 이름앞에 ...를 붙여서 정의한 매개변수를 의미한다. Rest파라미터는 함수에 전달된 인수들의 목록을 배열로 전달받는다

```jsx
function foo(...rest) {
  console.log(rest); // [ 1, 2, 3, 4, 5 ]
}

foo(1, 2, 3, 4, 5);

// 일반 매개변수와 함께 사용가능
function foo2(param, ...rest) {
  console.log(param); // 1
  console.log(rest);  // [ 2, 3, 4, 5 ]
}

foo2(1, 2, 3, 4, 5);
```

Rest 파라미터는 먼저 선언된 매개변수에 할당된 인수를 제외한 나머지 인수들로 구성된 배열이 할당된다. Rest파라미터는 반드시 마지막 파라미터여야 한다. (그렇지 않으면 SyntaxError가 발생)

### **Rest 파라미터와 arguments객체**

arguments객체는 함수 호출시 전달된 인수들의 정보를 담고있는 유사배열 객체이다.

그래서 배열메서드를 사용하려면 객체를 배열로 먼저 변환해줘야 한다.

```jsx
function sum() {
  // 유사 배열 객체인 arguments 객체를 배열로 변환한다.
  var array = Array.prototype.slice.call(arguments);

  return array.reduce(function (pre, cur) {
    return pre + cur;
  }, 0);
}

console.log(sum(1, 2, 3, 4, 5)); // 15
```

```jsx
function sum(...args) {
  // args에는 배열 [1, 2, 3, 4, 5]가 할당된다.
  return args.reduce((pre, cur) => pre + cur, 0);
}
console.log(sum(1, 2, 3, 4, 5)); // 15
```

## 매개변수 기본 값

함수를 호출할 때 매개변수의 개수만큼 인수를 전달하지 않으면 인수가 전달되지 않은 매개변수의 값은 undefined이다.

매개변수에 인수가 전달되었는지 확인하여 인수가 전달되지 않은 경우 매개변수에 기본값을 할당할필요가 있다.

```jsx
function sum(x = 0, y = 0) {
  // x = x || 0;
  // y = y || 0;
  return x + y;
}

console.log(sum(1, 2)); // 3
console.log(sum(1));    // 1
```

# 27장 배열

## 배열이란?

여러 개의 값을 순차적으로 나열한 자료구조이다.

```jsx
const example = [1,2,3,4,5]
typeof example // object
```

배열이 가진 값들 : 요소

요소에 접근하기 위한 위치 : 인덱스

우리는 이러한 인덱스를 바탕으로 **값의 순서**를 매길 수 있으며, 순회 할 수 있다.

나아가 **`length` 프로퍼티**를 통해 길이를 알 수 있다.

## 자바스크립트 배열은 배열이 아니다

자료구조에서 말하는 배열

동일한 크기의 메모리 공간이 빈틈없이 연속적으로 나열된 자료구조

즉, 배열 요소는 하나의 데이터 타입으로 통일되어 있으며 서로 연속적으로 인접해 있다. (= 밀집 배열)

인덱스를 통해 단 한번의 연산으로 임의의 요소에 접근 → 효율적, 고속으로 동작

배열에 요소를 삽입하거나 삭제하는 경우 배열의 요소를 연속적으로 유지하기 위해 요소를 이동시켜야 한다는 단점도 존재

자바스크립트의 배열

배열의 요소를 위한 각각의 메모리 공간은 동일한크기를 갖지 않아도 되며,  연속적으로 이어져 있지 않을 수도 있다. (= 희소 배열)

```jsx
console.log(Object.getOwnPropertyDescriptors([1, 2, 3]));
// {
//     '0': { value: 1, writable: true, enumerable: true, configurable: true },
//     '1': { value: 2, writable: true, enumerable: true, configurable: true },
//     '2': { value: 3, writable: true, enumerable: true, configurable: true },
//     length: { value: 3, writable: true, enumerable: false, configurable: false }
//   }
```

자바스크립트 배열은 인덱스를 나타내는 문자열을 프로퍼티 키로 가지며, length 프로퍼티를 갖는 특수한 객체다. 또한 어떤 타입의 값도 요소가 될 수 있다.

자바스크립트 배열은 해시 테이블로 구현된 객체이므로 인덱스로 요소에 접근하는 경우 일반적인 배열보다 성능적인 면에서 느릴 수밖에 없는 구조적인 단점이 있다. 

(실제로는 인덱스로 접근할 때에는 다른 언어보다 느릴 수밖에 없다는 한계가 있습니다. 이를 보완하기 위해서, 실제로 자바스크립트 엔진들은 배열을 최적화)

하지만 요소를 삽입/삭제하는 경우에는 일반적인 배열보다 빠른 성능을 기대할 수 있다.

## **length 프로퍼티와 희소 배열**

length 프로퍼티는 요소의 개수, 즉 배열의 길이를 나타내는 0이상의 정수를 값으로 갖는다.

length 프로퍼티 값보다 작은 숫자를 할당하면 배열의 길이다 줄어든다. 반대로 큰 숫자를 할당하면 배열의 길이가 늘어나지 않는다.

```jsx
const arr = [1, 2, 3, 4, 5];

arr.length = 3; // 5->3
console.log(arr); // [1, 2, 3]

arr.length = 5;
console.log(arr); // [1, 2, 3, empty*2]
```

- `empty`
    
    따로 메모리에 값을 저장하지 않을 때 알려주기 위한 장치
    
    실제로는 배열의 길이가 늘어나지도 않으며, 메모리 공간을 확보하지 않음
    
    따라서 자바스크립트는 희소배열이며, 희소배열의 `length`는 항상 배열의 길이와 일치하지는 않는다. (`length` >= `실제 배열의 길이`)
    

모던 자바스크립트 엔진들은 기본적으로 타입이 같고 요소가 일치하는 배열을 생성할 때 가장 최적화가 잘 되므로 배열을 생성할 경우 희소 배열을 생성하지 않도록 주의!

## 배열 생성

### **배열 리터럴**

배열 리터럴은 0개 이상의 요소를 쉼표로 구분하여 대괄호([])로 묶는다. 배열 리터럴은 프로퍼티 키가 없고 값만 존재한다.
`const arr = [1, 2, 3, 4, 5];`

### **Array 생성자 함수**

Array 생성자 함수를 통해 배열을 생성할 수도 있다. 이때 생성된 배열은 희소 배열이다.

```jsx
const arr = new Array(10);
console.log(arr);//[ <10 empty items> ]
console.log(arr.length); // 10
```

### Array.of

ES6에서 도입된 `Array.of` 메서드는 전달된 인수를 요소로 갖는 배열을 생성한다.
`Array.of(1, 2, 3); // [1, 2, 3]`

### **Array.from**

ES6에서 도입된 `Array.from` 메서드는 유사 배열 객체 또는 이터러블 객체를 인수로 전달받아 배열로 변환하여 반환한다.
`Array.from({ length: 2, 0: 'a', 1: 'b' }) // [ 'a', 'b' ]`

## **배열 요소의 참조**

배열의 요소를 참조할 때에는 대괄호([]) 표기법을 사용한다. 대괄호 안에는 인덱스가 와야 한다.

```jsx
const arr = [1, 2]
console.log(arr[0]); // 1 , 인덱스가 0인 요소 참조
console.log(arr[1]); // 2 , 인덱스가 1인 요소 참조
console.log(arr[2]); // undefined , 요소가 존재하지 않을 때
```

## **배열 요소의 추가와 갱신**

존재하지 않는 인덱스를 사용해 값을 할당하면 새로운 요소가 추가된다.

정수 이외의 값을 인덱스처럼 사용하면 요소가 생성되는 것이 아니라 프로퍼티가 생성된다.

이때 추가된 프로퍼티는 length 프로퍼티 값에 영향을 주지 않는다. 

```jsx
const arr = [0];
arr[1] = 1;
arr[100] = 100;
console.log(arr); // [ 0, 1, <98 empty items>, 100 ]
console.log(arr.length); // 101
arr['foo'] = 3;
console.log(arr); // [ 0, 1, <98 empty items>, 100, foo: 3 ]
console.log(arr.length); // 101
```

## **배열 요소의 삭제**

배열은사실 객체이기 때문에 `delete`연산자를 사용해 요소를 삭제할 수 있다.

```jsx
const arr = [1, 2, 3];

delete arr[1];
console.log(arr); // [ 1, <1 empty item>, 3 ]
console.log(arr.length); // 3
```

delete 연산자를 사용하면 희소 배열이 되므로 사용하지 않는 것이 좋다.

요소를 완전히 삭제하려면 `splice`메서드를 사용해야 한다.

```jsx
arr.splice(1, 1);
console.log(arr); // [ 1, 3 ]
console.log(arr.length); // 2
```

## 배열 메서드

배열에는 원본 배열(배열 메서드를 호출한 배열, 즉 배열 메서드의 구현체 내부에서 this가 가리키는 객체)을 직접 변경하는 메서드와 원본 배열을 직접 변경하지 않고 새로운 배열을 생성하여 반환하는 메서드가 있다.

전자의 경우 외부 상태를 직접 변경하는 부수 효과가 있으므로 가급적 원본 배열을 직접 변경하지 않는 메서드를 사용하는 편이 좋다.

### **Array.isArray**

`Array.isArray`메서드는 전달된 인수가 배열이면 `true`, 아니면 `false`를 반환한다.

### **Array.prototype.indexOf**

`indexOf`메서드는 원본 배열에서 인수로 전달된 요소를 검색하여 인덱스를 반환한다. 이는 특정 요소가 존재하는지 확인할 때 유용하다.

```jsx
const arr = [1, 2, 3, 2];

console.log(arr.indexOf(2)); // 1
console.log(arr.indexOf(4)); // -1 , 요소가 없으면 -1 반환
```

`indexOf`메서드 대신 ES7에서 도입된 `Array.prototype.include` 메서드를 사용하면 가독성이 더 좋다.

```jsx
if (!arr.includes('1')){
    arr.push('1');
}
```

### **Array.prototype.push**

`push`메서드는 인수로 전달받은 값을 배열 마지막 요소로 추가한다. `push`메서드는 원본 배열을 직접 변경한다.

추가할 요소가 하나라면 직접 추가하는 방법도 있는데 이 방법이 push 보다 빠르다.
`arr[arr.length] = 5;`

`push`메서드는 성능 면에서 좋지 않다. 따라서 push 메서드보다는 ES6의 스프레드 문법을 사용하는 편이 좋다.

```jsx
const arr = [1, 2, 3, 2];
CONST newArr = [...arr, 3];
```

### **Array.prototype.pop**

`pop`메서드는 배열 마지막 요소를 제거하고 제거한 요소를 반환한다. 빈 배열이라면 undefined를 반환한다. `pop`메서드는 원본 배열을 직접 변경한다.

```jsx
const arr = [1, 2, 3, 2];
let result = arr.pop();
console.log(result); // 2
console.log(arr); // [ 1, 2, 3 ]
```

### **Array.prototype.unshift**

```jsx
const arr = [1, 2, 3];
let result = arr.unshift(3, 4);
console.log(result); // 5
console.log(arr); // [ 3, 4, 1, 2, 3 ]
```

`unshift`메서드는 배열의 선두에 요소를 추가한다. `unshift`메서드는 원본 배열을 직접 변경한다.
마찬가지로 부수효과 없는 ES6의 스프레드 문법을 사용하는 편이 좋다.

### Array.prototype.concat

`concat`메서드는 인수로 전달된 값들을 배열 마지막 요소로 추가한 새로운 배열을 반환한다. 이때 원본 배열은 변경되지 않는다.

```jsx
const arr1 = [1, 2, 3];
const arr2 = [4, 5];
let result = arr1.concat(arr2);
console.log(result); // [ 1, 2, 3, 4, 5 ]
```

`push`와 `unshift`메서드는 `concat`메서드로 대체할 수 있다.

`concat`메서드는 ES6의 스프레드 문법으로 대체할 수 있다.

### Array.prototype.splice

배열 중간에 있는 요소를 제거하는 경우 `splice`메서드를 사용한다. 이때 원본 배열을 직접 변경한다.

```jsx
const arr = [1, 2, 3, 4, 5];
const result = arr.splice(1, 2, 20, 30);
// 인덱스 1부터 2개의 요소를 제거하고, 20, 30을 삽입한다.

console.log(result); // [ 2, 3 ]
console.log(arr); // [ 1, 20, 30, 4, 5 ]

const result = arr.splice(1, 0, 100); // 1부터 0개의 요소를 제거하고 그 자리에 100 삽입
const result = arr.splice(1, 2); // 1부터 2개의 요소를 제거
const result = arr.splice(1); // 모든 요소 제거
```

### **Array.prototype.slice**

`slice`메서드는 인수로 전달된 범위 요소들을 복사해 배열로 반환한다. 이때 원본 배열을 변경하지 않는다.

이때 복사는 얕은 복사로 이루어짐 

```jsx
const arr = [1, 2, 3, 4, 5];
arr.slice(0, 1); // 1, [0]부터 [1]이전까지 복사(이때 [1]은 미포함)
arr.slice(1, 2); // 2
arr.slice(1); // [1]부터 이후 모든 요소 복사 반환([1]포함)
arr.slice(); // 원본 전체 복사
arr.slice(-1); // 배열 끝에서 부터 요소 하나 반환 // 5
arr.slice(-2); // 배열 끝에서 부터 요소 두개 반환 // 4, 5
```

### **Array.prototype.join**

`join`배열을 문자열로 변환 후 구분자로 연결한 문자열 반환한다. 기본 구분자는 콤마(’,’)다.

```jsx
const arr = [1, 2, 3, 4, 5];

console.log(arr.join()); // 1,2,3,4,5
console.log(arr.join('')); // 12345
console.log(arr.join(':')); // 1:2:3:4:5
```

### Array.prototype.reverse

`reverse`메서드는 배열 순서를 반대로 뒤집는다. 이때 원본 배열이 변경된다.

```jsx
console.log(arr.reverse()); // [ 5, 4, 3, 2, 1 ]
```

### **Array.prototype.fill**

ES6에서 도입된 `fill`메서드는 인수로 전달받은 값을 배열 처음부터 끝까지 요소로 채운다. 이때 원본 배열이 변경된다.

```jsx
const arr = [1, 2, 3, 4, 5];
arr.fill(0); // [ 0, 0, 0, 0, 0 ]
arr.fill(0, 1); // 인덱스 1부터 0으로 채운다 [ 1, 0, 0, 0, 0 ]
arr.fill(0, 1, 3); // 인덱스 1부터 3이전까지 0으로 채운다 (3미포함)  [ 1, 0, 0, 4, 5 ]
```

### Array.prototype.includes

ES7에서 도입된 includes메서드는 배열에 특정 요소가 포함되었는지 확인하여 true, false를 반환한다.

```jsx
arr.includes(2); //true
arr.includes(100); //false
arr.includes(1, 2); // 1이 포함되어있는지 인덱스 2부터 확인한다.
arr.includes(3, -1); // 3이 포함되어있는지 인덱스 arr.length-1 부터 확인한다.
```

### **Array.prototype.flat**

ES10에서 도입된 `flat`메서드는 인수로 전달한 깊이만큼 재귀적으로 배열을 평탄화한다.

```jsx
console.log([1, [1, 2, 3, 4]].flat()); // 깊이 기본값 : 1 [ 1, 1, 2, 3, 4 ]
console.log([1, [1, [2, [3, 4]]]].flat(2)); //[ 1, 1, 2, [ 3, 4 ] ]
[1, [1, [2, [3, 4]]]].flat(Infinity); // 모두 평탄화
```

## **배열 고차 함수**

고차 함수는 함수를 인수로 전달받거나 함수를 반환하는 함수를 말하며, 외부 상태의 변경이나 가변 데이터를 피하고 불변성을 지향하는 **함수형 프로그래밍**에 기반을 두고 있다.

함수형 프로그래밍은 조건문과 반복문을 제거하여 복잡성을 해결하고 변수의 사용을 억제하여 상태 변경을 피하려는 패러다임이다.

결국 순수 함수를 통해 부수 효과를 최대한 억제하여 오류를 피하고 프로그램의 안전성을 높이려는 노력의 일환이라고 할 수 있다.

### Array.prototype.sort

`sort`메서드는 배열의 요소를 정렬한다. 기본적으로 오름차순이다.

```jsx
const fruits = ['Banana', 'Aplle', 'Orange']
fruits.sort();
console.log(fruits); // [ 'Aplle', 'Banana', 'Orange' ]

// 내림차순 정렬
fruits2.reverse();
```

`sort`메서드 기본 정렬 순서는 유니코드 코드 포인터 순서를 따르는데 숫자 타입이라도 **문자열로 변환 후 정렬**하기 때문에 문제가 발생한다.

따라서 숫자 정렬은 정렬 순서를 정의하는 **비교 함수**를 인수로 전달해야 한다.

```jsx
const arr = [1, 5, 100, 30, 2, 25]
arr.sort((a, b) => a - b);
console.log(arr); // [ 1, 2, 5, 25, 30, 100 ]

arr.sort((a, b) => b - a); // 내림차순 정렬
```

### Array.prototype.forEach

`forEach`메서드는 `for`문을 대체할 수 있는 고차 함수다.

`forEach`메서드는 반복문을 통해 배열을 순회하며 수행해야 할 처리를 콜백 함수로 전달받아 반복 호출한다.

```jsx
[1, 2, 3].forEach((item, index, arr) => {
    console.log(`값${item}, 인덱스${index}, this:${JSON.stringify(arr)}`);
});
```

`forEach`메서드는 원본 배열을 변경하지 않는다. 하지만 콜백 함수를 통해 원본 배열을 변경할 수는 있다.

**Array.prototype.map**

`map`메서드는 자신을 호출한 배열을 순회하면서 인수로 전달받은 콜백 함수를 반복 호출한다. 그리고 콜백 함수의 반환값들로 구성된 새로운 배열을 반환한다. 이때 원본 배열은 변경되지 않는다.

```jsx
[1, 2, 3].map((item, index, arr) => {
    console.log(`값${item}, 인덱스${index}, this:${JSON.stringify(arr)}`);
});
```

`map`메서드가 생성하여 반환하는 새로운 배열의 length 프로퍼티 값은 `map`메서드가 호출한 배열의 length 프로퍼티 값과 반드시 일치한다. 즉 `map`이 반환한 배열은 1:1 매핑한다.

### Array.prototype.filter

`filter`메서드는 배열을 순회하면서 인수로 전달받은 콜백 함수를 반복 호출한다. 그리고 콜백 함수 반환값이 true인 요소로 구성된 새로운 배열을 반환한다.

```jsx
const arr = [1, 2, 3, 4, 5];
const odd = arr.filter(item => item % 2); // 2로 나눠서 나머지가 0이 아니면 true
console.log(odd); // [ 1, 3, 5 ]
```

### Array.prototype.reduce

`reduce`메서드는 배열을 순회하고 콜백 함수를 반복 호출한다. 그리고 콜백 함수 반환값을 다음 순회에 전달하면서 하나의 결과값을 만들어 반환한다. 이때 원본 배열은 변경되지 않는다.

```jsx
const arr = [1, 2, 3, 4, 5].reduce((accumulator, currentValue, index, array) => 
  accumulator + currentValue
  , 0
);
console.log(arr); // 15
```

reduce메서드의 초기값(0)을 생략할 수도 있지만 **초기값을 전달하는것이 안전**하다.

```jsx
const arr = [].reduce((acc, cur) => acc+ cur); // TypeError
```

### Array.prototype.some

`some`메서드는 콜백 함수의 반환값이 단 **한번이라도 참이면 true**, 모두 거짓이면 false를 반환한다.

배열이 빈 배열인 경우 언제나 false를 반환하므로 주의

```jsx
[5, 10, 15].some(item => item > 10); // true
['banana', 'apple', 'orange'].some(item => item === 'banana'); //true
```

### Array.prototype.every

`every`메서드는 콜백 함수의 반환값이 **모두 참이면 true**, 단 한번이라도 거짓이면 false를 반환한다. 배열이 빈 배열인 경우 언제나 true를 반환하므로 주의

```jsx
[5, 10, 15].every(item => item > 3); // true
[].every(item => item > 3); // true
```

### **Array.prototype.find**

ES6에서 도입된 `find`메서드는 콜백 함수 반환값이 true인 첫 번째 요소를 반환한다.

```jsx
const user = [
    {id:1, name:'lee'},
    {id:2, name:'kim'},
    {id:2, name:'foo'},
    {id:4, name:'bar'},
];

user.find(user => user.id === 2); // {id:2, name:'kim'} 배열이 아니라 요소 반환
```

### Array.prototype.findIndex

ES6에서 도입된 `findIndex`메서드는 콜백 함수 반환값이 true인 첫 번째 요소의 인덱스를 반환한다. 요소가 존재하지 않으면 -1 반환한다.

```jsx
user.findIndex(user => user.id === 2); // 1
user.findIndex(user => user.name === 'foo'); // 2
```

### Array.prototype.flatMap

ES10에서 도입된 `flatMap`메서드는 map메서드를 통해 생성된 새로운 배열을 평탄화한다. 즉 map메서드와 flat메서드를 순차적으로 실행하는 효과가 있다.

```jsx
const arr = ['hello', 'world'];

console.log(arr.flatMap(x => x.split('')))

// [ 'h', 'e', 'l', 'l', 'o', 'w', 'o', 'r', 'l', 'd']
```

평탄화 깊이를 지정할 수는 없고 1단계만 평탄화하므로, 깊이를 지정해야 하면 map 메서드와 flat 메서드를 각각 호출

```jsx
const arr = ['hello', 'world'];

arr.flatMap((str, index) => [index, [str, str.length]]);
// [0, ['hello', 5], 1, ['hello', 5]]
arr.map((str, index) => [index, [str, str.length]]).flat(2);
// [0, 'hello', 5, 1, 'hello', 5]

```
