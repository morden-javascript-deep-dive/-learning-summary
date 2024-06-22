# 28. Number

## 28.1 Number 생성자 함수

`const num = new Number();`

new 연산자와 함께 호출하면 [[NumberData]] 내부 슬롯에 인수로 전달받은 숫자를 할당한 Number 래퍼 객체 생성

인수가 숫자가 아니면 강제 변환, 숫자로 변환할 수 없다면 NaN을 할당

new 연산자를 사용하지 않고 호출하면 Number 인스턴스가 아닌 숫자를 반환한다. (명시적으로 타입 변환하는 수단으로 쓰기도 함)

`Number('0');` // -> 0 (문자열 타입 -> 숫자 타입)

## 28.2 Number 프로퍼티

### **Number.EPSILON**

1과 1보다 큰 숫자 중에서 가장 작은 숫자와의 차이

부동 소수점으로 인해 발생하는 오차를 해결하기 위해 사용

`return Math.abs(a-b) < Number.EPSILON;` // a와 b를 뺀 값의 절대값이 Number.EPSILON 보다 작으면 같은 수로 인정

### **Number.MAX_VALUE**

자바스크립트에서 표현할 수 있는 가장 큰 양수 값(이보다 큰 숫자는 Infinity다.)

### **Number.MIN_VALUE**

자바스크립트에서 표현할 수 있는 가장 작은 양수 값(이보다 작은 숫자는 0이다.)

### **Number.MAX_SAFE_INTEGER**

자바스크립트에서 안전하게 표현할 수 있는 가장 큰 정수값

### **Number.MIN_SAFE_INTEGER**

자바스크립트에서 안전하게 표현할 수 있는 가장 작은 정수값

### **Number.POSITIVE_INFINITY**

양의 무한대를 나타내는 숫자값 Infinity와 같다.

### **Number.POSITIVE_INFINITY**

음의 무한대를 나타내는 숫자값 -Infinity와 같다.

### **Number.NaN**

숫자가 아님을 나타내는 숫자값. (=window.NaN)

### Number.isFinite

인수로 전달된 숫자값이 정상적인 유한수, 즉 Infinity 또는 -Infinity가 아닌지 검사하여 불리언 값으로 반환

- isFinite와 차이점!
    
    `isFinite` -> 전달받은 인수를 숫자로 암묵적 타입 변환하여 검사 수행
    
    `Number.isFinite` -> 전달받은 인수를 숫자로 암묵적 타입 변환하지 않음! 따라서 숫자가 아닌 인수는 언제나 false 반환
    

### Number.isInteger

인수로 전달된 숫자 값이 정수인지 검사하여 결과를 불리언 값으로 반환

인수를 숫자로 암묵적 타입 변환하지 않는다.

### Number.isNaN

인수로 전달된 숫자값이 NaN인지 검사하여 결과를 불리언 값으로 반환

isFinite와 마찬가지로, isNaN과 다르게 Number.isNaN는 전달받은 인수를 숫자로 암묵적 타입 변환하지 않는다!

### Number.isSafeInteger

인수로 전달된 숫자 값이 안전한 정수인지 검사하여 결과를 불리언 값으로 반환한다.

검사전에 인수를 암묵적 타입 변환하지 않는다.

### Number.prototype.toExponential

숫자를 지수 표기법으로 변환하여 문자열로 반환. 인수로 소수점 이하로 표현할 자릿수를 전달할 수 있다.

`(77.1234).toExponential();`

숫자 리터럴과 함께 Number 프로토타입 메서드를 사용할 경우 에러 발생

`77.toExponential(); // SyntaxError`

77 뒤의 .을 소수 구분 기호로 해석하면 뒤에 이어지는 toExponential을 프로퍼티로 해석할 수 없기 때문!

`77.1234.toExponential();` // 첫번째 .은 소수 구분기호이므로 두번째 .은 프로퍼티 접근 자로 해석할 수 있음

`toExponential();` // .뒤에 공백이 오면 .을 프로퍼티 접근 연산자로 해석할 수 있어 에러 발생 x

하지만 혼란 방지할 수 있어 그룹 연산자를 사용할 것을 권장한다.

### Number.prototype.toFixed

숫자를 반올림하여 문자열로 반환한다. 반올림하는 소수점 이하 자릿수를 인수로 전달할 수 있음

### Number.prototype.toPrecision

인수로 전달받은 전체 자릿수까지 유효하도록 나머지 자릿수를 반올림하여 문자열로 반환

### Number.prototype.toString

숫자를 문자열로 변환하여 반환

# 29. Math

## 29.1 Math 프로퍼티

### Math.PI

원주율 PI 값 반환

## 29.2 Math 메서드

### Math.abs

인수로 전달된 숫자의 절대값을 반환

### Math.round

인수로 전달된 숫자의 소수점 이하를 반올림한 정수를 반환

### Math.ceil

인수로 전달된 숫자의 소수점 이하를 올림한 정수 반환

### Math.floor

인수로 전달된 숫자의 소수점 이하를 내림한 정수 반환

### Math.sqrt

인수로 전달된 숫자의 제곱근을 반환

### Math.random

임의의 난수를 반환. 반환한 난수는 0에서 1미만의 실수다.

### Math.pow

첫번째 인수를 밑으로, 두번째 인수를 지수로 거듭제곱한 결과 반환

ES7에서 도입된 지수 연산자를 사용하면 가독성이 더 좋다.!

### Math.max

전달받은 인수 중에서 가장 큰 수를 반환. 인수가 전달되지 않으면 -Infinity를 반환한다.

배열을 인수로 전달받을 때는 Function.prototype.apply 메서드 또는 스프레드 문법을 사용해야 함

### Math.min

Math.min 메서드는 전달받은 인수 중에서 가장 작은 수를 반환한다. 인수가 전달되지 않으면 Infinity를 반환한다.

# 30. Date
