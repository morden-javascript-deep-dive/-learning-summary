# 07장 연산자

- 연산자는 하나 이상의 표현식을 대상으로 연산을 수행하는 기호, 피연산자는 연산의 대상
- 피연산자 및 피연산자와 연산자의 조합 모두 값으로 평가될 수 있는 표현식이다.

```javascript
var x = 5;

var x = 5 * 4;
```

## 산술 연산자

- 피연산자를 대상으로 수학적 계산을 수행해 새로운 숫자 값을 만든다
- 이항 상술 연산자
  - 2개의 피연산자를 산술 연산해서 숫자 값을 만드는 것
  - +, -, \*, /, %
  - 피연산자의 값을 변경하는 부수 효과 없이 언제나 새로운 값을 만든다.
- 단항 산술 연산자

  - 1개의 피연산자를 산술 연산해서 숫자 값을 만드는 것
  - ++, --, +, -
  - 증가/감소(++/--)에만 부수 효과가 적용된다.
  - +, -에는 부수효과가 적용되지 않는다.

  ```javascript
  // 증가/감소(++/--) 연산자는 위치에 의미가 있다.
  var x = 5,
    result;

  // 선할당 후증가
  result = x++;
  console.log(result, x); // 5 6

  // 선증가 후할당
  result = ++x;
  console.log(result, x); // 7 7

  // +,-는 문자열, 불리언을 숫자로 타입변환 해 반환하나, 피연산자는 변경하지 x
  var y = "1";
  console.log(+y); // 1

  y = true;
  console.log(+y); // 1

  y = "Hello";
  console.log(+y); //NaN
  ```

- 문자열 연결 연산자

  - +연산자는 피연산자 중 하나 이상이 문자열이라면 문자열 연결 연산자로 동작한다.
  - 자바스크립트 엔진에 의해 암묵적으로 타입이 변경되는 것으로 암묵적 타입변환(implicit coercion) 혹은 타입 강제 변환(type coercion)이라고 한다.

  ```javascript
  "1" + 2; // '12'
  1 + "2"; // '12;

  1 + 2; // 3
  ```

## 할당 연산자

- 우항에 있는 피연산자의 평가 결과를 좌항에 있는 변수에 할당한다.
- 좌항의 변수에 값을 할당하므로 변수 값이 변하는 부수 효과가 있다.
- =, +=, -=, \*=, /=, %=

```javascript
var x;

x = 10;
x += 5; // 15
x -= 5; // 10
x *= 5; // 50
x /= 5; // 10
x %= 5; // 0

var str = "My name is";
str += "Yun"; // 'My name is Yun'
```

- 할당문은 값으로 평가되는 표현식인 문으로서 할당된 값으로 평가된다.

```javascript
var a, b, c;
a = b = c = 0;

console.log(a, b, c); // 0 0 0
```

## 비교 연산자

- 좌항과 우항의 피연산자를 비교한 다음 불리언 값을 반환한다.
- 주로 제어문이나 조건식에서 활용된다.
- 동등 비교 연산자
  - 느슨한 비교, ==, !=
  - 암묵적 타입 변환을 통해 타입을 일치 시킨 후, 비교한다.
  - 예측하기 어렵고 실수하기 쉽기 때문에 일치 비교 연산자를 사용
- 일치 비교 연산자

  - 엄격한 비교, ===, !==
  - 좌항과 우항의 피연산자가 타입도 같고 값도 같은 경우에 true 반환
  - 일치 비교 연산자에서 주의할 것

  ```javascript
  // NaN은 자신과 일치하지 않는 유일한 값, Number.isNaN을 사용하자
  NaN === NaN; // false

  // 비교한 값을 불리언 값으로 반환한다.
  Number.isNaN(NaN); // true
  Number.isNaN(10); // false
  Number.isNaN(1 + undefined); // true, undefined는 숫자로 변환하지 못하기 때문에 NaN

  // 양의 0과 음의 0을 비교했을 때 동등, 일치 비교 연산자 모두 true이다.
  0 === -0; // true
  0 == -0; // true

  // ES6, Object.is 메서드는 위의 사례에서 정확한 비교 결과를 반환한다.
  Object.is(-0, +0); // false
  Object.is(NaN, NaN); // true
  ```

  - 부동등 비교 연산자(!=)와 불일치 비교 연산자(!==)는 각각 동긍 비교, 일치 비교의 반대 개념

- 대소 관계 비교 연산자
  - 피연산자의 크기를 비교해 불리언 값을 반환한다.
  - x<y, x>y, x<=y, x>=y

## 삼항 조건 연산자

- 조건식의 평가 결과에 따라 반환할 값을 결정한다.
- 유일한 삼항 연산자이며 부수 효과는 없다.
- 조건식 ? 조건식이 true일 때 반환할 값 : 조건식이 false일 때 반환할 값
- if...else문으로 유사하게 처리 가능
- 하지만 삼항 조건 연산자는 값으로 평가할 수 있는 표현식이고 if...else는 값으로 평가할 수 없는 문이다.

```javascript
var x = 10;

var result = if (x % 2) { result = '홀수'; } else { result = '짝수';};
// SyntaxError: Unexpected token if

var result = x % 2 ? '홀수' : '짝수';
console.log(result); // 짝수
```

- 조건에 따라 어떤 값을 결정한다면 삼항 조건 연산자, 조건이 여러 개라면 if...else문의 가독성이 좋다.

## 논리 연산자

- 우항과 좌항의 피연산자를 논리 연산한다.
- 논리합(||)은 or로 하나만 true라면 true를 반환
- 논리곱(&&)dms and로 모두 다 true여야 true
- 논리 부정(!)은 not으로 논리의 반대
- 논리 부정은 언제나 불리언 값을 반환하지만 논리합(||) 또는 논리곱(&&)은 불리언 값을 반환하지 않을 수도 있다.
  - 언제나 2개의 피연산자 중 어느 한쪽으로 평가된다.

```javascript
// 단축 평가
"Cat" && "Dog"; // 'Dog'
```

- 드모르간의 법칙을 사용하면 복잡한 논리 연산자 표현식을 가독성있게 표현할 수 있다.

```javascript
!(x || y) === (!x && !y);
!(x && y) === (!x || !y);
```

## 쉼표 연산자

- 왼쪽 피연산자부터 차례대로 평가하며 평가가 끝나면 마지막 피연산자의 평가 결과를 반환한다.

```javascript
var a = (1 + 2, 3 + 4, 5 + 6);
console.log(a); // 11
```

## 그룹 연산자

- 소괄호('()')로 감싸지는 식을 가장 먼저 평가한다.
- 연산자 우선순위가 가장 높으며 연산자의 우선 순위를 조절할 수 있다.

## typeof 연산자

- 피연산자의 데이터 타입을 문자열로 반환한다.
- 'null'을 반환하는 경우는 없으며 함수라면 'function'을 반환한다.
- 반환되는 문자열은 정확하지 않을 수 있다.

```javascript
typeof NaN; // number
typeof null; // object
```

- typeof null이 object가 반환되는 것은 첫 번째 버전의 버그이나 기존 코드의 영향 때문에 아직까지 수정을 못하고 있다.
- 따라서 null 타입을 확인하고 싶다면 일치 연산자(===)를 사용하자
- 선언 되지 않는 식별자를 typeof로 연산하면 ReferenceError가 아닌 undefined를 반환한다.

## 지수 연산자

- ES7에서 도입, 좌항의 피연산자를 밑(base)로 우항의 피연산자를 지수(exponent)로 거듭제곱한다.
- 지수 연산자(\*\*)가 도입되기 이전에는 Math.pow 메서드를 사용했다.
- 음수를 거듭제곱의 밑으로 사용하려면 음수를 괄호로 묶어야한다.
- 할당 연산자와 사용 가능하다. (num \*\*= 2)
- 이항 연산자 중 우선순위가 가장 높다.

## 그 외의 연산자

- 옵셔널 체이닝 연산자, null 병합 연산자, 프로퍼티 삭제 등등 다양한 연산자가 있지만 다른 주제와 밀접하게 연관되어 있어 해당 주제를 소개하는 장에서 살펴보자

## 연산자의 부수 효과

- 다른 코드에 영향을 주는 효과를 부수 효과라 한다.
- 할당연산자(=), 증가/감소 연산자(++/--), delete 연산자가 있다.

## 연산자의 우선순위

- 여러 개의 연산자로 이루어진 문이 실행될 때 연산자가 실행되는 순서를 뜻한다.
- 연산자의 종류가 많기 때문에 우선순위를 외우기 보다는 그룹 연산자('()')를 통해 우선 순위를 명시적으로 조절하는 것을 권장한다.

## 연산자 결합 순서

- 연산자의 어느 쪽부터 평가를 수행할 것인지 나타내는 순서를 말한다.
- 좌항->우항
  - +, - , /, %, <, <=, >, >=, &&, ||, ., [], (), ??, ?., in, instanceof
- 우항->좌항
  - ++, --, 할당연산자, !x, +x, -x, ++x, --x, typeof, delete, ? ... : ..., \*\*

# 08 제어문

- 제어문을 사용하면 코드의 실행 흐름을 인위적으로 제어할 수 있다.
- 순차적인 코드의 흐름을 혼란스럽게해 가독성을 해치는 단점이 있다.

## 블록문

- 0개 이상의 문을 중괄호({})로 묶은 것으로 하나의 실행단위로 취급한다.
- 일반적으로 제어문이나 함수를 정의할 때 사용한다.
- 문의 종료를 의미하는 자체 종결성을 갖기 때문에 세미콜론(;)을 붙이지 않는다.

## 조건문

- 조건식의 결과에 따라 코드 블록(블록문)의 실행을 결정한다.
- if...else 문

  - 조건식의 평가 결과가 true라면 if문의 블록문이 실행되고, flase라면 else문의 블록문이 실행된다.
  - if문의 조건식이 불리언 값이 아닌 값으로 평가된다면 암묵적 타입 변환에 의해 강제로 불리언 값으로 변경된다.
  - 조건을 추가하고 싶다면 else if 문을 사용하면 된다.

  ```javascript
  var num = 2;
  var kind;

  if (num > 0) {
    kind = "양수";
  } else if (num < 0) {
    kind = "음수";
  } else {
    kind = "영";
  }
  console.log(kind); // 양수
  ```

  - 코드 블록 내에 문이 하나라면 중괄호를 생략 가능하다.
  - 대부분의 if...else문은 삼항 조건 연산자로 바꿔 쓸 수 있다.

- switch 문

  - 주어진 표현식을 평가해 일치하는 표현식을 가지는 case 문을 실행한다.
  - 일치하는 case 문이 없다면 default 문으로 이동한다. default는 선택사항이다.
  - 불리언로 실행할 코드 블록을 정하는 if...else 문에 비해 switch 문은 다양한 상황(case)에 따라 코드 블록을 결정한다.
  - 코드 블록에서 탈출하는 역할인 break를 사용하지 않는다면 모든 case문과 default문을 실행해 폴스루(fall through)가 발생하기 때문에 올바른 switch문이 아니다.

  ```javascript
  var num = 2;
  var kind;

  switch (num) {
    case 2:
      kind = "짝수";
      break;
    default: // default 문에는 break가 필요없음
      kind = "홀수";
  }

  console.log(kind); // 짝수
  ```

  - 폴스루를 활용해 여러 개의 case 문을 하나의 조건으로 사용할 수도 있다.
  - 다양한 키워드, 폴스루, 문법의 복잡성으로 switch를 지원하지 않는 프로그래밍 언어도 있다. ex) python
  - 만약 if...else로 해결할 수 있다면 if...else를 쓰는게 좋지만 조건이 너무 많다면 switch를 쓰는것이 가독성이 좋다.

## 반복문

- 조건식이 참인 경우 코드블록을 실행하며 조건식을 다시 평가해 여전히 참이면 다시 실행한다.
- forEach 메서드, for...in 문, for...of 문으로 반복문을 대체할 수 있는 다양한 기능을 제공한다. 자세한건 해당 장에서 살펴보자
- for 문
  - 조건식이 거짓으로 평가될 때까지 코드블록을 실행한다.
  ```javascript
  for (var i = 1; i >= 0; i--) {
    // 변수 선언문, 조건식, 증감식
    console.log(i);
  }
  ```
  - 변수 선언문, 조건식, 증감식은 필수가 아니지만 어떤 식도 선언하지 않으면 무한루프에 빠진다. for(;;) {...}
  - for문 내에 for문을 중첩사용 가능하며 이를 중첩 for문이라고 한다.
- while 문

  - 주어진 조건식의 평가결과가 참이면 코드 블록을 계속해서 반복 실행한다.
  - for문은 반복 횟수가 명확할 때, while문은 반복 횟수가 불명확 할때 사용한다.
  - 만약 조건식의 평가 결과가 불리언 값이 아니면 불리언 값으로 강제 변환해 참, 거짓을 구별한다.
  - 만약 조건이 참이라면 무한 루프에 빠지며 if문으로 탈출 조건을 만들고 break 문으로 코드블럭을 빠져나와야 한다.

  ```javascript
  var count = 0;

  // 무한 루프
  while (true) {
    console.log(count);
    count++;
    if (count === 3) break; // 만약 count가 3이라면 빠져나온다.
  }
  ```

- do...while 문

  - 코드 블록을 먼저 실행하고 조건식을 평가한다.

  ```javascript
  var count = 0;

  do {
    console.log(count);
    count++;
  } while (count < 3); // 만약 count가 3이라면 빠져나온다.
  ```

## break 문

- 레이블 문(식별자가 붙은 문), 반복문, switch 문의 코드 블록을 탈출할 때 쓰인다.
- 이외의 코드 블록에 break를 쓴다면 SyntaxError(문법 에러)가 발생한다.
  - 레이블 문은 중첩된 for 문 외부로 탈출할 때 유용하지만 그 외에는 가독성이 나빠지고 오류를 발생시킬 가능성이 있기 때문에 권장하지 않는다.
    ```javascript
    outerloop: for (var i = 0; i < 5; i++) {
      for (var j = 0; j < 5; j++) {
        if (i === 2 && j === 2) {
          console.log("Breaking out of the outerloop");
          break outerloop; // 외부 루프로 탈출, 레이블 식별자
        }
        console.log("i =", i, ", j =", j);
      }
    }
    ```
  - 반복문, switch 문에서는 break 문에 레이블 식별자를 지정하지 않는다.

## Countinue 문

- break와 달리 반복문을 완전히 중단하지 않고 현재 반복만 중단하고 다음 반복을 진행한다.

```javascript
for (var i = 0; i < 10; i++) {
  if (i % 2 === 0) {
    continue;
  }
  console.log(i);
} // 1 3 5 7 9
```

- if 문 내에서 실행해야할 코드가 한줄이면 countinue를 사용하지 않아도 가독성이 좋지만 여러줄이라면 continue 문을 사용하는 것이 가독성이 더 좋다.

```javascript
// 코드가 한 줄인 경우
for (var i = 0; i < 10; i++) {
  if (i % 2 === 0) console.log(i); // 한 줄인 경우, continue를 사용하지 않아도 가독성이 좋다.
}
```

```javascript
// 코드가 여러 줄인 경우
for (var i = 0; i < 10; i++) {
  if (i % 2 === 0) {
    console.log("현재 짝수입니다.");
    console.log("현재 값은 " + i + "입니다.");
    console.log("다음 값을 확인합니다.");
    continue; // 여러 줄인 경우, continue를 사용하여 가독성을 높일 수 있다.
  }
  console.log(i);
}
```

# 09장 타입 변환과 단축 평가

## 타입 변환이란?

- 개발자가 의도적으로 값의 타입을 변경하는 것을 명시적 타입 변환(explicit coercion) or 타입 캐스팅(type casting) 이라고 한다.
- 자바스크립트 엔진에 의해 암묵적으로 타입이 바뀌는 것을 암묵적 타입 변환(implicit coercion) or 타입 강제 변환(type coercion)이라고 한다.
- 타입 변환이란 기존 원시 값을 변경하는 게 아닌 원시 값을 사용해 다른 타입의 새로운 원시 값을 생성하는 것이다.
  ```javascript
  var str = "123";
  var num = Number(str);
  console.log(typeof str); // string
  console.log(typeof num); // number
  ```
- 암묵적 타입 변환 시 개발자의 의지가 들어가 있지 않기 때문에 타입 변환 결과를 예측 가능해야한다.
- 타입 변환 결과를 예측하지 못하거나 예측이 일치하지 않는 다면 오류를 생산할 가능성이 높아진다. -> 타입스크립트 사용
- 명시적 타입 변환만 사용하도록 유도한다면 오히려 코드가 복잡해지기 때문에 가독성이 떨어진다.

## 암묵적 타입 변환

- 코드에 문맥에 부합하지 않는 상황에서 자바스크립트 엔진은 암묵적 타입 변환을 시도한다.
- 문자열 타입 변환
  - +연산자에서 피연산자가 하나라도 문자열일 경우 문자열 연결 연산자로 동작해 나머지 피연산자가 문자열로 암묵적 타입 변환이 일어난다.
    - 심벌 타입은 문자열 연결 연산자로 타입 변환 시 TypeError로 타입 변환 에러가 뜬다.
  - 리터럴의 표현식 삽입은 표현식의 평가 결과를 문자열로 암묵적 타입 변환이 일어난다.
    ```javascript
    `1 + 1 = ${1 + 1}`; // "1 + 1 = 2"
    ```
- 숫자 타입으로 변환
  - +를 제외한 산술 연산자 사용 시 숫자 타입으로 암묵적 타입 변환이 일어난다.
    - 숫자로 변환할 수 없다면 평가 결과는 NaN를 반환한다.
  - 비교 연산자에서는 값을 비교하기 위해 숫자 타입으로 암묵적 타입 변환이 일어난다.
  - +단항 연산자는 피연산자가 숫자 타입의 값이 아니면 암묵적 타입 변환이 일어난다.
    - 빈 문자열(''), 빈 배열([]), null, false는 0, true는 1로 변환된다.
    - 객체, 빈 배열이 아닌 배열, undefined는 NaN를 반환한다.
- 불리언 타입으로 변환

  - if 문, for 문 같은 제어문 또는 삼항 조건 연산자의 조건식은 논리적 참/거짓으로 평가 되어야 하기 때문에 조건식의 평가 결과를 불리언으로 암묵적 타입 변환한다.
  - 자바스크립트 엔진은 불리언 타입이 아닌 값을 Truthy 값, 또는 Falsy 값으로 구분해 불리언 값으로 타입 변환 한다.
  - false, undefined, null, 0, -0, NaN, ''(빈 문자열)은 Falsy로 구분해 false로 평가된다.
  - 위 값 외의 모든 값은 true로 평가되는 Truthy 값이다.

  ## 명시적 타입 변환

- 개발자의 의도에 따라 명시적으로 타입을 변환한다.
- 문자열 타입으로 변환

  ```javascript
  var num = 123;

  // String 생성자 함수를 사용한 문자열 변환
  var str1 = String(num);
  console.log(typeof str1); // 출력: "string"

  // Object.prototype.toString 메서드를 사용한 문자열 변환
  var str2 = Object.prototype.toString.call(num);
  console.log(typeof str2); // 출력: "string"

  // 문자열 연결 연산자를 이용한 문자열 변환
  var str3 = "" + num;
  console.log(typeof str3); // 출력: "string"
  ```

- 숫자 타입으로 변환

  ```javascript
  var str = "123";

  // Number 생성자 함수를 사용한 숫자 타입 변환
  var num1 = Number(str);
  console.log(typeof num1); // 출력: "number"

  // parseInt 함수를 사용한 숫자 타입 변환 (문자열만 가능)
  var num2 = parseInt(str);
  console.log(typeof num2); // 출력: "number"

  // parseFloat 함수를 사용한 숫자 타입 변환 (문자열만 가능)
  var num3 = parseFloat(str);
  console.log(typeof num3); // 출력: "number"

  // + 단항 산술 연산자를 이용한 숫자 타입 변환
  var num4 = +str;
  console.log(typeof num4); // 출력: "number"

  // * 산술자를 이용한 숫자 타입 변환
  var num5 = str * 1;
  console.log(typeof num5); // 출력: "number"
  ```

- 불리언 타입으로 변환

  ```javascript
  var value = "hello";

  // Boolean 생성자 함수를 사용한 불리언 타입 변환
  var bool1 = Boolean(value);
  console.log(typeof bool1); // 출력: "boolean"

  // ! 부정 논리 연산자를 두번 사용한 불리언 타입 변환
  var bool2 = !!value;
  console.log(typeof bool2); // 출력: "boolean"
  ```

## 단축 평가

- 논리합(||) 또는 논리곱(&&)은 논리 연산의 결과를 결정하는 피연산자를 반환한다.
- 만약 'Cat' && 'Dog'가 있다면 앞의 암묵적 타입 변환에 의해 'Cat'은 Truthy 값이므로 true를 반환한다.
- 논리곱(&&)은 두개의 피연산자 모두 참이여야 하므로 'Dog'의 값까지 봐야한다.
- 'Dog'가 논리곱 연산자 표현식의 평가 결과를 결정하므로 'Dog'를 그대로 반환한다.
- 즉 논리 연산의 결과를 결정하는 피연산자를 타입 변환 없이 그대로 반환하는 것을 단축 평가(short-circuit evaluation)이라고 한다.
- 단축 평가를 활용해 if 문을 대체할 수 있다.

  ```javascript
  var done = true;
  var message = "";

  if (done) {
    message = "완료";
  } else {
    message = "미완료";
  }
  console.log(message); // 출력: "완료"

  var message = (done && "완료") || "미완료";
  console.log(message); // 출력: "완료"
  ```

- 객체를 가리키기를 기대하는 변수가 null 또는 undefined가 아닌지 확인하고 프로퍼티를 참조할때 논리곱(&&)단축 평가 주로 사용

  ```javascript
  var elem = null;
  var value = elem.value; // TypeError: Cannot read property 'value' of null

  var value = elem && elem.value; // null
  ```

- 함수 매개변수에 기본값을 설정할 때 인수를 전달하지 않으면 매개변수에 undefined가 할당되어 발생하는 에러를 논리합(||) 단축 평가로 방지한다.

  ```javascript
  function greet(name) {
    name = name || "Guest";
    return "Hello, " + name + "!";
  }

  console.log(greet("John")); // 출력: "Hello, John!"
  console.log(greet()); // 출력: "Hello, Guest!"
  ```

- 옵셔널 체이닝 연산자

  - ES11(ECMAScript2020)에서 도입된 옵셔널 체이닝(optional chaining) 연산자(?.)
  - 좌항의 피연산자가 null or undefined면 undefined 반환, 아니라면 우항의 참조를 이어간다.

    ```javascript
    var elem = null;
    var value = elem?.value; // TypeError: Cannot read property 'value' of null
    console.log(value); // undefined
    ```

  - 기존 논리곱(&&)단축 평가를 통해 값을 확인한다면 null이나 undefined가 아닌 Falsy 값이라면(0, '') 좌항 피연산자를 그대로 반환했다.
  - 하지만 옵셔널 체이닝 연산자 ?.를 사용하면 같은 Falsy 값이라도 null이나 undefined가 아니라면 우항의 프로퍼티 참조를 이어갔다.

- null 병합 연산자
  - ES11(ECMAScript2020)에서 도입된 null 병합(nullish coalescing) 연산자(??)
  - 좌항의 피연산자가 null 또는 undefined 인 경우 우항의 피연산자를 반환, 아니라면 좌항의 피연산자를 반환
    ```javascript
    var foo = null ?? "default string";
    console.log(foo); // "default string";
    ```
  - 기존 논리합(||)단축 평가를 통해 값을 확인한다면 null이나 undefined가 아닌 Falsy 값이라면(0, '') 우항의 피연산자를 반환해 예기치 않은 동작 발생
  - null 병합 연산자(??)는 같은 Falsy 값이라도 null이나 undefined가 아니라면 좌항의 피연산자를 그대로 반환한다.
