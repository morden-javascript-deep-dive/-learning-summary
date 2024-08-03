# 에러 처리

## 에러 처리의 필요성

- 에러에 대해 대처하지 않고 방치하면 프로그램은 강제 종료된다.

```js
console.log("[Start]");

foo(); // ReferenceError

// 에러에 의해 프로그램이 강제 종료되어 이 코드는 실행되지 않는다.
console.log("[End]");
```

- try .. catch문을 사용해 발생한 에러에 적절하게 대응하면 프로글매이 강제 종료되지 않고 계쏙해서 코드를 실행할 수 있따.

```js
console.log("[Start]");

try {
  foo(); // ReferenceError
} catch (error) {
  console.error("[에러 발생]", error);
}

console.log("[End]");
```

- 직접적으로 에러를 발생시키지 않는 예외적인 상황이 발생할 수도 있다. 예오.적인 상황에 적절하게 대응하지 않으면 에러로 이어질 가능성이 크다.

## try...catch...finally 문

- 에러 처리를 구현하는 방법은 크게 두가지가 있다.

  1. 예외적인 상황이 발생하면 반환하는 값(null or -1)을 if문이나 옵셔널 체이닝 연산자를 통해 확인해서 처리하는 방법
  2. try .. catch ... finally 문을 활용하여 처리하는 방법

  ```js
  try {
    // 실행할 코드(에러가 발생할 가능성이 있는 코드)
  } catch (err) {
    // try 코드 블록에서 에러가 발생하면 이 코드 블록의 코드가 실행된다.
    // err에는 try 코드 블록에서 발생한 Error 객체가 전달된다.
  } finally {
    // 에러 발생과 상관없이 반드시 한 번 실행한다.
  }
  ```

## Error 객체

- Error 생성자 함수는 에러객체를 생성한다. Error 생성자 함수에는 에러를 상세히 설명하는 에러 메시지를 인수로 전달할 수 있다.

```js
const error = new Error("invalid");
```

- Error 생성자 함수가 생성한 에러 객체는 message 프로퍼티와 stack 프로퍼티를 갖는다. message 프로퍼티의 값은 Error 생성자 함수에 인수로 전달한 에러 메시지이고, stack 프로퍼티의 값은 에러를 발생시킨 콜 스택의 호출 정보를 나타내는 문자열이며 디버깅 목적으로 사용된다.
- 자바스크립트는 Error 생성자 함수를 포함해 7가지의 에러 객체를 생성할 수 있는 Error 생성자 함수를 제공
  - Error, SyntaxError, ReferenceError, TypeError, RangeError, URIError, EvalError

## throw 문

- Error 생성자 함수로 에러 객체를 생성한다고 에러가 발생하는 것은 아니다. 즉, 에러 객체 생성과 에러 발생은 의미가 다르다.

```js
try {
  // 에러 객체를 생성한다고 에러가 발생하는 것은 아니다.
  new Error("something wrong");
} catch (error) {
  console.log(error);
}
```

- 에러를 발생시키려면 try 코드 블록에서 throw 문으로 에러 객체를 던져야 한다.

```js
try {
  // 에러 객체를 던지면 catch 코드 블록이 실행되기 시작한다.
  throw new Error("something wrong");
} catch (error) {
  console.log(error);
}
```

## 에러의 전파

- 에러의 호출자(caller) 방향으로 전파된다. 즉, 콜 스택의 아래방향으로 전파된다.

```js
const foo = () => {
  throw Error("foo에서 발생한 에러");
};

const bar = () => {
  foo();
};

const baz = () => {
  bar();
};

try {
  baz();
} catch (err) {
  console.error(err);
}
```

- foo -> bar -> bax -> 전역 실행 컨텍스트 방향으로 전파된다. thorw된 에러를 어디에서도 캐치하지 않으면 프로그램은 강제 종료된다.
- 주의할 것은 비동기 함수인 setTimeout이나 프로미스 후속 처리 메서드의 콜백 함수는 호출자가 없다는 것이다. setTimeout이나 프로미스 후속 처리 메서드의 콜백함수는 태스크 큐나 마이크로태스 큐에 일시 저장되었다가 콜 스택이 비면 이벤트 루프에 의해 콜 스택으로 퓟되어 실행된다.
- 이때 콜 스택에 푸시된 콜백 함수의 실행 컨텍스트는 콜 스택의 가장 하부에 존재하게된다. 따라서 에러를 전파할 호출자가 존재하지 않는다.

## 모듈

## 모듈의 일반적 의미

- 모듈: 재사용 가능한 코드 조각이며 파일 단위로 분리한다. 또한 자신만의 파일 스코프를 가진다.
- 모듈은 공개가 필요한 자신에 한정하여 명시적으로 성택적 공개가 가능함(exprot)
- 모듈 사용자는 모듈이 공개한 자산 중 일부 또는 전체를 선택해 자신의 스코프 내로 불러들여 재사용할 수 있다.(import)
- 모듈 사용의 이점
  - 코드 단위 명확히 분리
  - 재사용성
  - 개발 효율성
  - 유지보수성

## 자바스크립트와 모듈

- 처음 사용목적이 단순한 보조 기능 처리였기에 모듈이 존재하지 않았지만, CommonS와 AMD 등 다양한 모듈시스템이 나오면서 지원하게 됨
- 현재 Node.js는 CommonS를 모듈 시스템 표준으로 채택함
  - 파일별로 독립적이 파일 스코프를 갖는다.

## ES6 모듈(ESM)

```js
<script type="module" src="app.mjs"></script>
```

- 사용법은 type="module" 어트리뷰트를 추가하며, 일반적인 js 파일이 아닌 esm임을 명확히 하기위해 ESM 파일 확장자는 mjs를 사용할 것을 권자(strict mode가 기본적으로 적용)

1. 모듈 스코프

- 일반적인 js 파일은 독자적인 모듈 스코프를 갖지 않아서 var x로 변수를 선언한 2개의 파일이 존재할 시, 마지막으로 로드된 js 파일의 var x 선언문에 할당된 값이 window.x에 들어가 있게된다.
- 그러나 ESM은 아래와 같이 다르게 동작한다.

```html
<script type="module" src="foo.mjs"></script>
<script type="module" src="bar.mjs"></script>
```

```js
// foo.mjs
// x 변수는 전역 변수가 아니고 window 객체의 프로퍼티도 아님

var x = "foo";

console.log(x); // foo
console.log(window.x); // undefined

// -------------------------

// bar.mjs

var x = "bar";

console.log(x); // bar
console.log(window.x); // undefined
```

- 이 뿐만 아니라 모듈 내에서 선언한 식별자는 모듈 외부에서 스코프가 다르기 때문에 참조할 수 없다.

2. export 키워드

- 모듈 내부에서 선언한 식별자를 외부에 공개하여 다른 모듈들이 재사용할 수 있게 하려면 export 키워드를 사용한다.
- export 키워들르 매번 선언문 앞에 붙여서 사용할 수 있지만 객체로 구성하여 한번에 export가 가능하다.

```js
// lib.mjs
const pi = Math.PI;

function square(x) {
  return x * x;
}

class Person {
  constructor(name) {
    this.name = name;
  }
}

// 변수, 함수 클래스를 하나의 객체로 구성하여 공개
export { pi, square, Person };
```

3. import 키워드

- 다른 모듈에서 공개한 식별자를 자신의 모듈 스코프 내부로 로드하려면 import 키워들르 사용한다.
- 다른 모듈이 export한 식별자 이름으로 import 하고, ESM의 경우 파일 확장자 생략 불가능!

```mjs
// app.mjs
import { pi, square, Person } from "./lib.mjs";

console.log(pi);
console.log(square(10));
console.log(new Person("Lee"));
```

- 그 외 사용법

```js
// 하나의 이름으로 한 번에 import 하는 방법
import * as lib from "./lib.mjs";

// export한 식별자 이름 변경
import { pi as PI, square as sq, Person as p } from "./lib.mjs";
```

- 하나의 값만 export 하는 경우 default 키워드르 ㄹ사용할 수 있고, 이 키워드와 함께 export한 모듈은 {} 없이 임의의 이름으로 import할 수 있다.

```mjs
// lib.mjs

export default (x) => x * x;

// --------------------
// app.mjs

import square from "./lib.mjs";

console.log(square(3)); // 9
```

- default 키워드 사용시 var, let, const 키워드는 사용할 수 없다.

```js
export default const foo = () => {}; // SyntaxError
```

# Babel과 Webpack을 이용한 ES6+/ES.NEXT 개발 환경 구축

- 대부분의 최신 브라우저의 ES6 지원율은 높지만, IE 11등은 ES6의 지원률은 낮기 때문에 IE 포함한 구형 브라우저에서 문제 없이 동작시키기 위한 개발 환경을 구축해야 한다.
  - 트랜스파일러인 Babel과 모듈 번들러인 Webpack을 이용
- ES6 모듈은 대부분의 모던 브라우저에서 사요할 수 있지만 다음의 이유로 별도의 모듈 로더를 사용하는 것이 일반적
  - IE를 포함한 구형 브라우저는 ESM 지원 x
  - ESM을 사용하더라도 트랜스파일링이나 번들링이 필요한 것은 변함이 없다.
  - ESM이 아직 지원하지 않는 기능이 있고 점차 해결되고는 있지만 아직 몇가지 이슈가 존재

## Babel

- Babel은 ES6+/ES.NEXT로 구현된 최신 사양의 소스코드를 IE 같은 구형 브라우저에서도 동작하는 ES5 사양의 소스코드로 변환시킬 수 있다.

1. Babel 설치

```
npm init -y

npm install --save-dev@babel/core @babel/cli
```

2. Babel 프리셋 설치와 babel.config.json 설정 파일 작성

- babel을 사용하기 위해서 @babel/preset-env 설치
- 함께 사용되어야하는 babel 플러그인을 모아둔것을 바벨 프리셋이라고 부른다.

```
npm install --save-dev @babel/preset-env
```

- babel.config.json

```json
{
  "presets": ["@babel/preset-env"]
}
```

3. 트랜스파일링

- 매번 Babel CLI를 입력할 순 없으므로 package.json을 활용하여 scripts에 명령어를 등록하여 사용

```json
"scripts": {
  "build": "babel src/js -w -d dist/js"
}
```

- src/js 폴더에 있는 모든 js 파일들을 트랜스파일링한 후, 결과물을 dist/js 폴더에 저장하겠다는 의미

  - -w: 타킷 폴더에 있는 모든 js 파일들의 변경을 감지하여 자동으로 트랜스파일한다.
  - -d: 트랜스파일링된 결과물이 저장될 폴더를 지정한다. 만약 지정된 폴더가 존재하지 않으면 자동 생성한다.

- 이후 npm run build 명령어를 사용하여 트랜스파일링을 진행할 수 있지만, ES6 클래스의 #private와 같으 클래스 필드 저으이 제안의 경우 지원을 하지 않아 에러가 발생
  - 이를 위해선 별도의 플러그인 설치가 필요

4. Babel 플러그인 설치

```
// 코드에 따라 필요한 플러그인 설치
npm install --save-dev @babel/plugin-proposal-class-properties
```

- babel.config.json에 다음과 같이 추가

```json
"plugins": ["@babel/plugin-proposal-class-properties"]
```

5. 브라우저에서 모듈 로딩 테스트

- Node.js 환경에서는 CommonS 방식의 모듈 로딩 시스템을 따라서 정상적으로 잘동작되지만, 브라우저는 CommonS의 require 함수를 지원하지 않으므로 트랜스파일링된 결과를 그대로 실행하면 에러가 발생
- 브라우저의 ES6 모듈을 사용하도록 Babel을 설정할 수 있으나, ESM은 앞서 말한 3가지 문제점이 존재하기 때문에 WebPack을 사용

## Webpack

- 웹팩은 의존 관계에 있는 js, css, 이미지 등의 리소스들을 하나의 파일로 번들링하는 모듈 번들러

1. Webpack 설치

```
npm install --save-dev webpack webpack-cli
```

2. babel-loader 설치

```
npm install --save-dev babel-loader
```

- npm scripts를 변경하여 Babel 대신 Webpack을 실행하도록 수정

```json
"scripts": {
 "build": "webpack -w"
}
```

3. webpack.config.js 설정 파일 작성

- 이후 Webpack을 실행 후, 생성된 결과물을 index.html의 script src로 지정하고 브라우저를 싱행시키면 정상적으로 작동된 것을 확인할 수 있다.

4. babel-polyfill 설치

- Promise, Object.assign, Array.from 등은 트랜스파일링이 되지 못하고 그대로 남아서 이를 위해 babel-ployfill을 설치한다.

```
npm install @babel/polyfill
```

- 이는 개발 환경에서만 사용하는 것이 아닌 실제 운영 환경에서도 사용해야기 때문에 --save-dev 옵션을 사용하지 않는다.
- 바벨만 사용하는 경우 진입점 파일의 선두에 import @babel/polyfill을 작성
- Webpack을 사용하는 경우 entry 배열에 폴리필을 추가하여 사용한다.
  - entry: ['@babel/polyfill', './src/js/main.js'],
- 이후 명령어로 실행 후, 번들링된 파일을 확인해보면 폴리필이 추가된 것을 확인할 수 있다.
