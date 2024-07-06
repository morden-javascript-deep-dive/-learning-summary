# **35장 스프레드 문법**

ES6에서 도입된 스프레드 문법(전개문법은) `...`은 하나로 뭉쳐 있는 여러 값들의 집합을 펼쳐서(전개, 분산하여, spread) 개별적인 값들의 목록으로 만든다.

```jsx
console.log(...[1,2,3]); // 1 2 3
const list = ...[1,2,3]; // SyntaxError
```

스프레드 문법의 결과는 값이 아니다.

⇒ 따라서 스프레드 문법의 결과는 변수에 할당할 수 없다.
다음과 같이 쉼표로 구분한 값의 목록을 사용하는 문맥에서만 사용할 수 있다.

- 함수 호출문의 인수 목록
- 배열 리터럴의 요소 목록
- 객체 리터럴의 프로퍼티 목록

## **35.1 함수 호출문의 인수 목록에서 사용하는 경우**

요소들의 집합인 배열을 펼쳐서 개별적인 값들의 목록으로 만든 후, 이를 함수의 인수 목록으로 전달해야 하는 경우가 있다.

```jsx
const arr = [1, 2, 3];
const max = Math.max(arr); // NaN
const max = Math.max(...arr); // -> 3

// 참고 - Function.property.apply
var max = Math.max.apply(null, arr);
```

Rest 파라미터와 형태가 동일하여 혼동할 수 있으므로 주의 필요!

Rest 파라미터는 함수에 전달된 인수들의 목록을 배열로 받기 위해 사용된다.
스프레드 문법은 여러 개의 값이 하나로 뭉쳐 있는 배열과 같은 이터러블을 펼쳐서 개별적인 값들의 목록을 받는 것이다. 따라서 서로 반대의 개념이다

```jsx
// Rest 파라미터는 인수들의 목록을 배열로 전달받는다.
function foo(...rest) {
  console.log(rest); // 1, 2, 3 -> [ 1, 2, 3 ]
}

// 스프레드 문법은 배열과 같은 이터러블을 펼쳐서 개별적인 값들의 목록을 만든다.
// [1, 2, 3] -> 1, 2, 3
foo(...[1, 2, 3]);
```

## **35.2 배열 리터럴 내부에서 사용하는 경우**

### 배열 결합

```jsx
// ES5에서는 concat 메서드 활용
var arr = [1, 2].concat([3, 4]);
console.log(arr); // [1, 2, 3, 4]

// ES6
const arr = [...[1, 2], ...[3, 4]];
console.log(arr); // [1, 2, 3, 4]
```

### 배열 중간에 다른 배열의 요소 추가하거나 제거

```jsx
// ES5 에서는 splice 메서드 활용
var arr1 = [1, 4];
var arr2 = [2, 3];

Array.prototype.slice.apply(arr1, [1, 0].concat(arr2));

// ES6
arr1.splice(1, 0, ...arr2);
```

### 배열 복사

```jsx
// ES5
var orgin = [1,2];
var copy = origin.slice();

// ES6
const orgin = [1,2];
const copy = [...orgin];
```

### 이터러블을 배열로 변환

```jsx
// ES5
// Function.prototype.apply 또는 Function.prototype.call 메서드 사용
// 이터러블뿐만 아니라 이터러블이 아닌 유사 배열 객체도 배열로 변환할 수 있다.

function sum() {
  var args = Array.prototype.slice.call(arguments);
  
  return args.reduce(funtion (pre, cur) {
    return pre + cur;
  }, 0);
}

// ES6
// 스프레드 문법 사용
funtion sum() {
  return [...arguments].reduce((pre, cur) => pre + cur, 0);
}
// Rest 파리미터 사용
funtion sum(...args) {
  return args.reduce((pre, cur) => pre + cur, 0);
}

// 단, 이터러블이 아닌 유사 배열 객체는 스프레드의 문법 대상이 될 수 없다.
const arrayLike = {0: 1, 1: 2, 2: 3, length:3};
const arr = [...arrayLike]; // TypeError

Array.from(arrayLike);
```

### **35.3 객체 리터럴 내부에서 사용하는 경우**

스프레드 프로퍼티를 사용하면 객체 리터럴의 프로퍼티 목록에서도 스프레드 문법을 사용할 수 있다. 스프레드 문법의 대상은 이터러블이어야 하지만 스프레드 프로퍼티 제안은 일반 객체를 대상으로도 스프레드 문법의 사용을 허용한다.

```jsx
// 스프레드 프로퍼티
// 객체 복사(얕은 복사)
const obj = { x: 1, y: 2 };
const copy = { ...obj };
console.log(copy); // { x: 1, y: 2 }
console.log(obj === copy); // false
```

스프레드 프로퍼티가 제안되기 이전…

Object.assign 메서드를 사용하여 여러 객체를 병합하거나 특정 프로퍼티를 변경 또는 추가했다.

```jsx
// 스프레드 프로퍼티가 제안되기 이전에는 ES6에서 도입된 Object.assign 메서드를 사용하여 여러 개의 객체를 병합하거나 특정 프로퍼티를 변경 또는 추가했다.// 객체 병합. 프로퍼티가 중복되는 경우, 뒤에 위치한 프로퍼티가 우선권을 갖는다.
const merged = Object.assign({}, { x: 1, y: 2 }, { y: 10, z: 3 });
console.log(merged); // { x: 1, y: 10, z: 3 }

// 특정 프로퍼티 변경
const changed = Object.assign({}, { x: 1, y: 2 }, { y: 100 });
console.log(changed); // { x: 1, y: 100 }

// 프로퍼티 추가
const added = Object.assign({}, { x: 1, y: 2 }, { z: 0 });
console.log(added); // { x: 1, y: 2, z: 0 }

// 스프레드 프로퍼티는 Object.assign 메서드를 대체할 수 있는 간편한 문법이다.// 객체 병합. 프로퍼티가 중복되는 경우, 뒤에 위치한 프로퍼티가 우선권을 갖는다.
const merged = { ...{ x: 1, y: 2 }, ...{ y: 10, z: 3 } };
console.log(merged); // { x: 1, y: 10, z: 3 }

// 특정 프로퍼티 변경
const changed = { ...{ x: 1, y: 2 }, y: 100 };
// changed = { ...{ x: 1, y: 2 }, ...{ y: 100 } }
console.log(changed); // { x: 1, y: 100 }

// 프로퍼티 추가
const added = { ...{ x: 1, y: 2 }, z: 0 };
// added = { ...{ x: 1, y: 2 }, ...{ z: 0 } }
console.log(added); // { x: 1, y: 2, z: 0 }
```

# 36. 디스트럭처링 할당

디스트럭처링 할당 = 구조 분해 할당

구조화된 배열과 같은 이터러블 또는 객체를 destructuring(비구조화)하여 1개 이상의 변수에 개별적으로 할당하는 것을 말한다. 배열과 같은 이터러블 또는 객체 리터럴에서 필요한 값만 추출하여 변수에 할당할 때 유용하다.

## 36.1 배열 디스트럭처링 할당

```jsx
// ES5
var arr = [1, 2, 3];

var one   = arr[0];
var two   = arr[1];
var three = arr[2];
```

```jsx
const arr = [1, 2, 3];

// ES6 배열 디스트럭처링 할당
// 변수 one, two, three를 선언하고 배열 arr을 디스트럭처링하여 할당한다.
// 이때 할당 기준은 배열의 인덱스다.
const [one, two, three] = arr;

console.log(one, two, three); // 1 2 3

// 이 때, 우변에 이터러블을 할당하지 않으면 에러 발생
const [x, y]; // SyntaxError: Missing initializer in destructuring declaration

const [a, b] = {}; // TypeError: {} is not iterable

// 순서대로 할당되므로 변수 개수와 이터러블 요소 개수가 반드시 일치할 필요는 없다.
const [c,d] = [1]; // 1, undefined

// 배열 디스트럭처링 할당을 위한 변수에 기본값을 설정할 수 있다.
const [c,d = 3] = [1 , 2]; // 1, 2
```

배열 디스트럭처링 할당을 위한 변수에 Rest 파라미터와 유사하게 Rest 요소 …을 사용할 수 있는데, Rest 요소는 Rest 파라미터와 마찬가지로 반드시 마지막에 위치해야 한다.

```jsx
const [x, … y] = [1, 2, 3]; // 1 [2, 3]
```

## 36.2 객체 디스트럭처링 할당

ES5에서 객체의 각 프로퍼티를 객체로부터 디스트럭처링하여 변수에 할당하기 위해서는 프로퍼티 키를 사용해야 한다.

```jsx
// ES5
var user = { firstName : 'Ungmo', lastName : 'Lee' };

var firstName = user.firstName;
```

ES6의 객체 디스트럭처링 **할당 기준은 프로퍼티 키**다. 객체 디스트럭처링 할당의 대상(할당문의 우변)은 객체이어야 함.

```jsx
const user = { firstName : 'Ungmo', lastName : 'Lee' };

// ES6 객체 디스트럭처링 할당
const { firstName, lastName } = user;

// 객체의 프로퍼티 키와 다른 변수 이름으로 프로퍼티 값을 할당받으려면 다음과 같이 변수를 선언한다.

const { lastName : ln, firstName : fn } = user;

// 기본값 설정도 가능
const { lastName, firstName = 'Ungmo' } = user;

객체에서 프로퍼티 키로 필요한 키로 필요한 프로퍼티 값만 추출하여 변수에 할당하고 싶을 때 유용하다.

```

객체에서 프로퍼티 키로 필요한 키로 필요한 프로퍼티 값만 추출하여 변수에 할당하고 싶을 때 유용하다.

```jsx
const str = 'Hello';
// String 래퍼 객체로부터 length 프로퍼티만 추출한다.
const { length } = str;
console.log(length); // 5
```

배열의 요소가 객체인 경우 배열 디스트럭처링 할당과 객체 디스트럭처링 할당을 혼용할 수 있다.

```jsx
const todos = [
  { id : 1, content : 'HTML', completed : true },
  { id : 2, content : 'CSS', completed : false }
  ];

// 배열의 두 번째 요소인 객체로부터 id 프로퍼티만 추출한다.
const [, { id }] = todos;
console.log(id); // 2
```

중첩객체의 경우는 다음과 같이 사용한다.

```jsx
const user = {
  name : 'Lee',
  address : {
    zipCode : '03068',
    city : 'Seoul'
  }
};

// address 프로퍼티 키로 객체를 추출하고 이 객체의 city 프로퍼티 키로 값을 추출한다.
const { address : { city } } = user;
console.log(city); // 'Seoul'
```

객체 디스트럭처링 할당을 위한 변수에 Rest 파라미터나 Rest 요소와 유사하게 Rest 프로퍼티 `...`을 사용할 수 있다. 반드시 마지막에 위치해야 함!

```
// Rest 프로퍼티
const { x, ...rest} = { x : 1, y : 2, z : 3 };
console.log(x, rest); // 1 { y : 2, z : 3 }
```

# 37. Set과 Map

## Set

Set 객체는 중복되지 않는 유일한 값들의 집합(set)이다.

배열과 유사하지만, Set 객체는 배열과 다르게 동일한 값을 중복하여 포함할 수 없고, 요소 순서에 의미가 없으며 인덱스로 요소에 접근할 수 없다.

Set은 수학적 집합을 구현하기 위한 자료구조로, Set을 통해 교집합, 합집합, 차집합, 여집합 등을 구현할 수 있다.

### Set 객체의 생성

Set 객체는 Set 생성자 함수로 생성한다.

```jsx
const set = new Set();
console.log(set); // Set(0) {}
```

Set 생성자 함수는 이터러블을 인수로 전달받아 Set 객체를 생성한다. 이때 이터러블의 중복된 값은 Set 객체에 요소로 저장되지 않는다.

```jsx
const set1 = new Set([1, 2, 3, 3]);
console.log(set1); // Set(3) {1, 2, 3}
```

### 요소 개수 확인

`Set.prototype.size` 프로퍼티를 사용한다.

```jsx
const { size } = new Set([1, 2, 3, 3]);
console.log(size); // 3
```

size 프로퍼티는 setter 함수 없이 getter 함수만 존재하는 접근자 프로퍼티로, size 프로퍼티에 숫자를 할당하여 Set 객체의 요소 개수를 변경할 수 없음.

### 요소 추가

`Set.prototype.add` 메서드를 사용한다.

```jsx
const set = new Set();
console.log(set); // Set(0) {}

set.add(1);
console.log(set); // Set(1) {1}
```

add 메서드는 **새로운 요소가 추가된 Set 객체를 반환**하기 때문에 add 메서드를 호출한 후 add 메서드를 연속적으로 호출(method chaining)할 수 있다.

```jsx
const set = new Set();

set.add(1).add(2);
console.log(set); // Set(2) {1, 2}
```

Set 객체는 NaN과 NaN을 같다고 평가하여 중복 추가를 허용하지 않음. +0과 -0은 일치 비교 연산자 ===와 마찬가지로 같다고 평가하여 중복 추가를 허용하지 않음.

```
const set = new Set();

set.add(NaN).add(NaN);
console.log(set); // Set(1) {NaN}

set.add(0).add(-0);
console.log(set); // Set(2) {NaN, 0}
```

Set 객체는 객체나 배열과 같이 자바스크립트의 모든 값을 요소로 지정할 수 있다.

### 요소 존재 여부 확인

`Set.prototype.has` 메서드를 사용한다.

has 메서드는 특정 요소의 존재 여부를 나타내는 불리언 값을 반환한다.

```jsx
const set = new Set([1, 2, 3]);
console.log(set.has(2)); // true
```

### 요소 삭제

`Set.prototype.delete` 메서드를 사용한다.

삭제하는 요소값을 인수로 전달해야 한다.

```jsx
const set = new Set([1, 2, 3]);

set.delete(2)
console.log(set); // Set(2) {1, 3}
```

존재하지 않는 Set 객체의 요소를 삭제하려 하면 에러 없이 무시됨

delete 메서드는 삭제 성공 여부를 나타내는 불리언 값을 반환하기 때문에 optional chaining이 불가능하다.

### 요소 일괄 삭제

`Set.prototype.clear` 메서드를 사용한다. clear 메서드는 언제나 undefined를 반환한다.

```
set.clear();
console.log(set); // Set(0) {}
```

### 요소 순회

`Set.prototype.forEach` 메서드를 사용한다.

forEach 메서드의 콜백 함수 내부에서 this로 사용될 객체(옵션)를 인수로 전달한다.

콜백 함수는 3개의 인수를 전달받는다.

- 첫 번째 인수 : 현재 순회 중인 요소값
- 두 번째 인수 : 현재 순회 중인 요소값
- 세 번째 인수 : 현재 순회 중인 Set 객체 자체

첫 번째 인수와 두 번째 인수는 같은 값이며, `Array.prototype.forEach` 메서드와 인터페이스를 통일하기 위함일 뿐 다른 의미는 없다.

```jsx
const set = new Set([1, 2]);

set.forEach((v, v2, set) => console.log(v, v2, set));

/*
1 1 Set(2) {1, 2}
2 2 Set(2) {1, 2}
*/
```

**Set 객체는 이터러블**이다. 따라서 for...of 문으로 순회할 수 있으며, 스프레드 문법과 배열 디스트럭처링의 대상이 될 수도 있다.

```jsx
const set = new Set([1, 2 ,3]);

// Set.prototype의 Symbol.iterator 메서드를 상속받는 이터러블.
console.log(Symbol.iterator in set); // true

for (const value of set) {
  console.log(value); // 1, 2, 3
}
```

Set 객체는 요소의 순서에 의미를 갖지 않지만 Set 객체를 순회하는 순서는 요소가 추가된 순서를 따른다. 이는 ECMAScript 사양에 규정되어 있지는 않지만 다른 이터러블의 순회와 호환성을 유지하기 위함

### 집한 연산

**교집합**

```jsx
Set.prototype.intersection = function (set) {
  const result = new Set();

  for (const value of set) {
    // 2개의 set의 요소가 공통되는 요소이면 교집합의 대상이다.
    if(this.has(value)) result.add(value)
  }

  return result;
};

const setA = new Set([1, 2, 3, 4]);
const setB = new Set([2, 4]);

console.log(setA.intersection(setB)); // Set(2) {2, 4}

```

또다른 방법

```jsx
Set.prototype.intersection = function(set){
  return new Set([...this].filter(v => set.has(v)));
};
```

**합집합**

```jsx
Set.prototype.union = function(set){
  // this(Set 객체)를 복사
  const result = new Set(this);

  for(const value of set){
    // 중복된 요소는 포함되지 않음.
    result.add(value);
  }

  return result;
};

const setA = new Set([1, 2, 3, 4]);
const setB = new Set([2, 4]);

console.log(setA.union(setB)); // Set(4) {1, 2, 3, 4}
```

또다른 방법

```jsx
Set.prototype.union = function(set){
  return new Set([...this, ...set]);
};

const setA = new Set([1, 2, 3, 4]);
const setB = new Set([2, 4]);

console.log(setA.union(setB)); // Set(4){1, 2, 3, 4}

```

**차집합**

차집합 A-B는 A에는 존재하지만 집합 B에는 존재하지 않은 요소로 구성된다.

```jsx
Set.prototype.difference = function(set){
  const result = new Set(this);

  for (const value of set) {
    result.delete(value);
  }

  return result;
};

console.log(setA.difference(setB)); // Set(2){1, 3}

```

또다른 방법

```jsx
Set.prototype.difference = function(set){
  return new Set([...this].filter(v => !set.has(v)));
};
```

**부분 집합(subset)과 상위 집합(superset)**

```jsx
Set.prototype.isSuperset = function(subset){
  for(const value of subset){
    // superset의 모든 요소가 subset의 모든 요소를 포함하는지 확인
    if(!this.has(value)) return false;
  }

  return false;
};

console.log(setA.isSuperset(setB)); // true
```

또다른 방법

```jsx
Set.prototype.isSuperset = function(subset){
  const supersetArr = [...this];
  return [...subset].every(v => supersetArr.includes(v));
};
```

## Map

**Map 객체는 키와 값의 쌍으로 이루어진 컬렉션**이다. Map 객체는 객체와 유사하지만, 다음과 같은 차이가 있다.

- 키로 사용할 수 있는 값
    - 객체 : 문자열 또는 심벌 값
    - Map 객체 : 객체를 포함한 모든 값
- 이터러블인지?
    - 객체: X
    - Map 객체: 이터러블
- 요소 개수 확인
    - 객체 : `Object.keys(obj).length`
    - Map 객체 : `map.size`

### Map 객체의 생성

Map 생성자 함수로 생성.

```jsx
const map = new Map();
console.log(map); // Map(0) {}
```

Map 생성자 함수는 이터러블을 인수로 전달받아 Map 객체를 생성한다. 이때 인수로 전달되는 이터러블은 키와 값의 쌍으로 이루어진 요소로 구성되어야 한다.

```jsx
const map1 = new Map([['key1', 'value1'], ['key2', 'value2']);

console.log(map1); // Map(2) {"key1" => "value1", "key2" => "vlaue2"}
```

Map 생성자 함수의 인수로 전달한 이터러블에 중복된 키를 갖는 요소가 존재하면 값이 덮어써진다. 따라서 Map 객체에는 중복된 키를 갖는 요소가 존재할 수 없다.

### 요소 개수 확인

`Map.prototype.size` 프로퍼티 사용

```jsx
const { size } = new Map([['key1', 'value1'], ['key2', 'value2']);

console.log(size); // 2
```

size 프로퍼티는 setter 함수 없이 getter 함수만 존재하는 접근자 프로퍼티로, size 프로퍼티에 숫자를 할당하여 Map 객체의 요소 개수를 **변경할 수 없다**.

### 요소 추가

`Map.prototype.set` 메서드 사용

```jsx
map.set('key1', 'value1');
```

set 메서드는 새로운 요소가 추가된 Map 객체를 반환하기 때문에 method chaining 이 가능하다.

```jsx
map
  .set('key1', 'value1')
  .set('key2', 'value2');
```

map 객체에는 중복된 키를 갖는 요소가 존재할 수 없기 때문에 중복된 키를 갖는 요소를 추가하면 값이 덮어지며, 이 때 에러가 발생하지는 않는다.

NaN과 NaN, +0과 -0를 같다고 평가하여 중복 추가 허용하지 않는다.

Map 객체는 키 타입에 제한이 없다. 따라서 객체를 포함한 모든 값을 키로 사용할 수 있음.

```jsx
const map = new Map();

const lee = { name : 'Lee' };
const kim = { name : 'Kim' };

map
  .set(lee, 'developer')
  .set(kim, 'designer');

```

### 요소 취득

`Map.prototype.get` 메서드를 사용.

get 메서드의 인수로 키를 전달하면 Map 객체에서 인수로 전달한 키를 갖는 값을 반환함.

```jsx
console.log(map.get(lee)); // developer
```

### 요소 존재 여부 확인

`Map.prototype.has` 메서드를 사용. 불리언값 반환

```jsx
console.log(map.has('key')); // false
```

### 요소 삭제

`Map.prototype.delete` 메서드를 사용. 삭제 성공 여부를 나타내는 불리언값 반환

```jsx
map.delete(kim);
```

method chaining 불가능함.

### 요소 일괄 삭제

`Map.prototype.clear` 메서드를 사용. 언제나 undefined 반환

```jsx
map.clear();
```

### 요소 순회

`Map.prototype.forEach` 메서드 사용.

- 첫번째 인수 : 현재 순회 중인 요소값
- 두번째 인수 : 현재 순회 중인 요소키
- 세번째 인수 : 현재 순회 중인 Map 객체 자체

```jsx
const lee = { name : 'Lee' };
const kim = { name : 'Kim' };

const map = new Map([[lee, 'developer'], [kim, 'designer']]);

map.forEach((v, k, map) => cosole.log(v, k));

/*
developer {name : "Lee"}
desiguner {name : "Kim"}
*/

```

**Map 객체는 이터러블**. `for ...of` 문으로 순회할 수 있으며 스프레드 문법과 배열 디스트럭처링 할당의 대상이 될 수도 있다.

```
const map = new Map();

// Map 객체는 Map.prototype의 Symbol.iterator 메서드를 상속받는 이터러블.
console.log(Symbol.iterator in map); // true
```

Map 객체는 이터러블이면서 동시에 이터레이터인 객체를 반환하는 메서드를 제공한다.

- `map.prototype.keys`
- `map.prototype.values`
- `map.prototype.entries`

```jsx
const lee = { name : 'Lee' };
const kim = { name : 'Kim' };

const map = new Map([[lee, 'developer'], [kim, 'designer']]);

for (const key of map.keys()){
  console.log(key); // {name: "Lee"} {name : "Kim"}
}

for (const key of map.values()){
  console.log(key); // developer designer
}

for (const key of map.keys()){
  console.log(key); // [{name: "Lee"}, "developer"] [{name : "Kim"}, "designer"]
}

```

Map 객체는 요소의 순서에 의미를 갖지 않지만 Map 객체를 순회하는 순서는 요소가 **추가된 순서를 따른다**. 이는 ECMAScript 사양에 규정되어 있지는 않지만 다른 이터러블의 순회와 호환성을 유지하기 위함이다.

# 38. 브라우저의 렌더링 과정

브라우저가 HTML, CSS, JS로 작성된 텍스트 문서를 어떻게 parsing(해석)하여 브라우저에 rendering 하는가 ?!

- *브라우저의 렌더링 과정 (Critical Rendering Path)*
    - 브라우저는 HTML, CSS, JS, 이미지, 폰트 파일 등 렌더링에 필요한 리소스를 서버로 요청하고, 응답 받음
    - **렌더링 엔진**은 서버로부터 응답된 HTML, CSS 파싱하여 DOM과 CSSOM 생성, 결합 -> **렌더 트리 생성**
    - **JS 엔진**은 서버로부터 응답된 JS를 파싱하여 AST(Abstract Syntax Tree)를 생성하고 바이트코드로 변환 후 실행한다. 이때, JS는 DOM API를 통해 DOM이나 CSSOM 변경 가능, 변경된 DOM과 CSSOM이 다시 렌더 트리로 결합됨
    - *렌더 트리* 기반으로 HTML 요소의 레이아웃(위치와 크기)을 계산하고 브라우저 화면에 HTML 요소 페인팅
    

## 38.1 요청과 응답

브라우저의 핵심 기능은 필요한 리소스`HTML, CSS, JavaScript 등의 정적 파일 또는 서버가 동적으로 생성한 데이터`를 서버에 요청(requset)하고 서버로부터 응답(response)받아 브라우저에 렌더링하는 것이다.

URL 입력 ⇒ 서버 요청 ⇒ 서버는 루트 요청에 대해 암묵적으로 index.html 응답

브라우저의 렌더링 엔진이 HTML 파싱 도중 외부 리소스를 로드하는 태그를 만나면, HTML 파싱을 일시 중단하고 해당 리소스 파일을 서버로 요청함

## 38.2 HTTP 1.1과 HTTP 2.0

**HTTP**: 웹에서 브라우저와 서버가 통신하기 위한 프로토콜(규약)

- *HTTP 1.1 vs. HTTP 2.0*
    - **HTTP 1.1** (1999)
    - connection당 하나의 요청과 응답만 처리 (다중 요청/응답 불가능)
    - 리소스의 동시 전송이 불가능한 구조, 요청할 리소스 개수에 비례하여 응답 시간 증가하는 단점
- **HTTP 2.0** (2015)
    - connection당 여러 개의 요청과 응답 (다중 요청/응답 가능)
    - 여러 리소스 동시 전송 가능한 구조, HTTP/1.1 보다 페이지 로드 속도 약 50% 빠름

## 38.3 HTML 파싱과 DOM 생성

브라우저의 렌더링 엔진은 서버로부터 응답받은 HTML 문서를 파싱하여, 브라우저가 이해할 수 있는 자료구조인 DOM 생성한다.

- *HTML 파싱, DOM 생성 과정*
    - 브라우저 -> 서버에 HTML 파일 요청
    - 서버는 브라우저가 요청한 HTML 파일을 읽어들여 메모리에 저장, 메모리에 저장된 바이트(2진수)를 응답
    - 브라우저는 응답 받은 바이트(2진수)를 meta 태그의 charset 어트리뷰트에 의해 지정된 인코딩 방식으로 문자열로 변환 (응답 헤더 - content-type: text/html; charset=utf-8)
    - 문자열로 변환된 HTML 문서를 토큰들로 분해
    - 각 토큰들을 객체로 변환하여 노드 생성
    - 모든 노드들을 트리 자료구조로 구성 => **DOM** (HTML 문서 파싱한 결과물)

## 38.4 CSS 파싱과 CSSOM 생성

렌더링 엔진은 DOM 생성하다가 CSS 로드하는 link 태그나 style 태그 만나면 DOM 생성 일시 중단

서버에게 요청하여 로드한 CSS를 HTML과 동일한 파싱 과정 거침 (바이트 -> 문자 -> 토큰 -> 노드 -> CSSOM)

CSSOM은 CSS의 상속을 반영하여 생성됨

## 38.5 렌더 트리 생성

DOM과 CSSOM은 렌더링을 위해 렌더 트리로 결합됨

- **렌더 트리**
    - 렌더링을 위한 트리 구조의 자료구조
    - 브라우저 화면에 렌더링되는 노드만으로 구성
    - 각 HTML 요소의 레이아웃(위치와 크기)을 계산하는데 사용되며, 브라우저 화면에 픽셀을 렌더링하는 페인팅 처리에 입력됨
    - 리렌더링은 성능에 영향 많이 주므로 가급적 리렌더링이 빈번하게 발생하지 않도록 주의 필요!

## 38.6 자바스크립트 파싱과 실행

DOM은 HTML 문서의 구조와 정보뿐만 아니라 HTML 요소와 스타일 등을 변경할 수 있는 프로그래밍 인터페이스로서 DOM API를 제공한다.

JS 코드에서 *DOM API* 사용하면 이미 생성된 DOM을 동적으로 조작 가능 ⇒ 39장 “DOM”

렌더링 엔진은 HTML을 파싱하며 DOM을 생성해가다가, script 태그를 만나면 DOM 생성 일시 중단하고 JS 엔진에 제어권을 넘긴다. (JS 파싱과 실행은 렌더링 엔진이 아니라 JS 엔진이 처리함)

JS 엔진은 JS를 해석하여 AST(Abstract Syntax Tree) 생성

AST 기반으로 인터프리터가 실행할 수 있는 중간 코드인 바이트 코드 생성하여 실행

## 38.7 리플로우와 리페인트

DOM API에 의해 DOM이나 CSSOM이 변경될 수 있음

이때 변경된 DOM과 CSSOM은 다시 렌더 트리로 결합되고, 변경된 렌더 트리를 기반으로 레이아웃과 페인트 과정 거쳐 다시 화면에 리렌더링 ⇒ 리플로우, 리페인트

- **리플로우:** 레이아웃 다시 계산 (레이아웃 영향 주는 변경 발생한 경우에 한하여 실행됨)
- **리페인트:** 재결합된 렌더 트리를 기반으로 다시 페인트

## 38.8 자바스크립트 파싱에 의한 HTML 파싱 중단

렌더링 엔진과 JS 엔진은 직렬적으로/동기적으로 파싱 수행

script 태그의 위치에 따라 HTML **파싱 블로킹** 되어 DOM 생성 지연 가능성 문제

- 자바스크립트를 body 요소의 가장 아래에 JS 위치시키는 것은 좋은 아이디어!!
    - JS 실행될 시점에는 이미 렌더링 엔진이 HTML 요소 모두 파싱하여 DOM 생성 완료한 이후임
    - JS 실행 전에 DOM 생성이 완료되어 렌더링되서 페이지 로딩 시간 단축되는 이점

## 38.9 script 태그의 async/defer 어트리뷰트

자바스크립트 파싱에 의한 DOM 생성이 중단되는 문제를 근본적으로 해결하기 위해 HTML5 부터 script 태그에 async와 defer 어트리뷰트가 추가되었다.

외부 JS 파일 로드하는 경우에만 사용 가능

```jsx
<script async src="extern.js"></script>
<script defer src="extern.js"></script>
```

async와 defer 어트리뷰트를 사용하면 HTML 파싱과 외부 자바스크립트파일의 로드가 비동기적으로 동시에 진행된다.

- *async 어트리뷰트*
    - HTML 파싱와 외부 JS 파일 로드가 비동기적으로 동시에 진행됨
    - JS 파싱과 실행은 JS 파일 로드 완료된 직후에 진행되고, HTML 파싱 중단됨
    - 순서 보장 X
- *defer 어트리뷰트*
    - HTML 파싱와 외부 JS 파일 로드가 비동기적으로 동시에 진행됨
    - JS 파싱과 실행은 HTML 파싱 완료된 직후 (DOM 생성 완료 직후)에 진행됨
    - DOM 생성 완료 이후 실행될 JS에 유용
