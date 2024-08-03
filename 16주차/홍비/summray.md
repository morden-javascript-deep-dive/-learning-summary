# 47장 에러처리
## 에러 처리의 필요성
- 에러가 발생하지 않는 코드작성은 불가능하므로 언제나 에러나 예외처리가 발생할 수 있다는 것을 염두해두고 이에 대응하는 코드를 작성해야함
## try...catch...finally 문
- try : 가장 먼저 실행되는 코드 블럭, 여기서 에러가 발생한다면 catch로 넘어간다.
- catch : try문에서 발생한 에러 객체가 전달된다.
- finally : 에러가 발생하든 하지않든 무조건 한번 실행된다.
- try...catch...finally문으로 에러처리 시 프로그램 강제 종료 회피 가능

## Error 객체
- 에러 객체를 생성함, 생성자 함수에 인수로 에러를 설명하는 에러 메세지 전달 가능
- 생성된 에러 객체는 message 프로퍼티와 stack 프로퍼티를 가짐
- message 프로퍼티 : Error 생성자 함수에 전달한 인수 메세지
- stack 프로퍼티 : 에러를 발생시킨 콜스택 호출 정보를 나타내는 문자열, 디버깅 목적으로 사용
- Error : 일반적 에러 객체
- SyntacError : 자바스크립트 문법에 맞지 않는 문을 해석할 때 발생
- ReferenceError : 참조할 수 없는 식별자 참조 시 발생
- TypeError : 피연산자, 인수의 데이터 타입이 유효하지 않을 때 발생
- RangeError : 숫자값의 허용 범위를 벗어났을 때 발생
- URIError : encodeURI 또는 decodeURI 함수에 부적절한 인수 전달 시 발생
- EvalError : eval 함수에서 발생되는 에러

## throw 문
- Error 생성자 함수로 에러 객체를 생성한다고 에러가 생기는 것은 아님
- 에러를 발생하려면 throw문으로 에러 객체를 던저야함
```javascript
try{
    new Error('something wrong'); // 에러 발생 X, catch문 실행 x
    throw new Error('something wrong'); // 에러 발생 O, catch문 실행 o
}catch (error) { 
    console.log(error);
}
```

## 에러의 전파
- 에러는 호출자 방향으로 전파됨
```javascript
const foo = () => {
    throw Error('foo에서 발생한 에러');
};

const bar = () => {
    foo();
};

const baz = () => {
    bar();
}

try {
    baz();
} catch (err) {
    console.log(err);
}
```

- 에러는 foo에서 발생
- foo 함수를 bar 실행 컨텍스트에서 호출
- bar 함수를 baz 실행 컨텍스트에서 호출
- baz 함수를 전역 실행 컨텍스트에서 호출
- 따라서 에러 전파 방향은 foo 실행 컨텍스트 - bar 실행 컨텍스트 - baz 실행 컨텍스트 - 전역 실행 컨텍스트 가 된다.
- 만약 throw된 에러를 어디서도 캐치하지 않는다면 프로그램은 강제 종료 된다.
- 따라서 에러를 캐치해 적절한 대응으로 코드의 실행흐름을 복구 시켜야한다.
- but, setTimeout 및 프로미스 후속 처리 메서드의 콜백함수는 호출자가 없다.
- setTimeout이나 프로미스의 콜백함수는 특정 조건이 되면 큐에 저장했다가 나중에 실행되므로 에러가 발생해도 받아줄 호출자가 없기 때문에 콜백함수는 독립적으로 실행되어 에러를 받지 않는다.

# 48장 모듈
## 모듈의 일반적 의미
- 모듈은 애플리케이션을 구성하는 개별적 요소, 재사용 가능한 코드 조각
- 일반적으로 기능으로 파일을 분리하며 자신만의 파일 스코프(모듈 스코프)를 가질 수 있어야함
- 자신만의 파일 스코프를 가지는 모듈의 자산(변수, 함수..)는 캡슐화되어 다른 모듈에서 접근이 불가능 하다. (개별적 존재)
- 하지만 접근이 안된다면 존재의 의미가 없다 따라서 공개가 필요한 자산에 한정해 export로 선택적 공개를 한다.
- 공개된 모듈은 재사용이 가능하고 이 모듈의 자산을 사용하는 모듈을 모듈 사용자라한다.
- 모듈 사용자는 공개한 자산 중 일부 또는 전체를 선택해 자신의 스코프 내로 불러들여 재사용한다. 이를 import라고 한다.
- 모듈을 사용해 코드의 단위를 명확하게 하고 재사용성을 높여 개발 효율과 유지보수성을 높일 수 있다.
## 자바스크립트와 모듈
- 자바스크립트 탄생 때 자바스크립트는 그저 웹페이지의 보조 기능에 불과했음
- 따라서 모듈이 성립하기 위해 필요한 파일 스코프, import, export가 없었음
- 대부분의 프로그래밍 언어는 모듈기능을 가지고 있으나 클라이언트 사이드 자바스크립트는 script 태극를 사용해 외부의 자바스크립트 파일을 로드할 순 있으나 파일마다 독립적 파일 스코프를 갖진 않았음
- 따라서 여러 파일이 마치 하나의 파일에 있는 것처럼 동작해 전역 변수가 중복되거나 모듈 구현이 불가능했음
- 하지만 자바스크립트를 브라우저 환경에 국한하지 않고 범용적으로 사용하려는 움직임이 생겨 모듈시스템이 필요하게 됨
- 이런상황에서 자바스크립트 모듈은 크게 CommonJS와 AMD 진영으로 나뉘게됨
- Node.js는 CommonJS를 채택해 독자적 진화로 ECMAScript 표준 사양은 아니지만 모듈 시스템을 지원하고 있다 따라서 Node.js환경에서는 파일별 독립적인 파일 스코프를 가진다.

## ES6 모듈(ESM)
- 이런 상황에서 ES6는 클라이언트 사이드 자바스크립트에서도 동작하는 모듈 기능을 추가했다.
- IE를 제외한 대부분의 브라우저에서 사용이 가능하다.
```javascript
<script type="module" src="app.mjs"></script>
```
- script 태그에 type="module" 어트리뷰트를 추가
- ESM이라는 것을 명시하기 위해 src="app.mjs" 파일 확장자 사용
- 기본적으로 strict mode 적용됨

- 모듈 스코프
    - ESM은 독자적 모듈 스코프를 가짐
    - ESM이 아닌 일반적인 자바스크립트 파일은 script 태그로 분리해도 독자적 모듈 스코프를 가지지 않음
    - ESM에서 파일 모듈 내에서 var 키워드로 선언한 변수는 더는 전역 변수가 아니며 window 객체의 프로퍼티도 아니다.
- export 키워드
    - export는 선언문 앞에 사용한다. 이로써 변수, 함수, 클래스 등 모든 식별자를 export 가능하다.
    - 선언문 앞에 매번 export를 붙이기 번거롭다면 export 대상을 하나의 객체로 구성해 한번에 export가 가능하다.
- import 키워드
    - 다른 모듈에서 내보낸(export) 식별자를 자신의 모듈 스코프 내부로 로드하려면 import를 쓴다.
    - export한 식별자 이름으로 import해야하며 ESM이라면 파일 확장자 생략이 불가능하다.
    ```javascript
    import {pi, square, Person} from './lib.mjs';

    console.log(pi);
    console.log(square(10));
    console.log(new Person('Lee'));

    <!DOCTTYPE html>
    <html>
    <body>
        <script type="module" src="app.mjs"></script>
    </body>
    </html>
    ```
    - app.mjs는 애플리케이션의 진입점이므로 반드시 script태그로 로드해야한다.
    - 하지만 lib.mjs는 app.mjs의 import문에 로드되는 의존성이므로 따라서 lib.mjs는 script 태그로 로드하지 않아도 된다.
    ```javascript
    import * as lib from './lib.mjs';

    console.log(lib.pi);
    console.log(lib.square(10));
    console.log(new lib.Person('Lee'));
    ```
    - 모듈이 export한 식별자 이름을 일일이 지정하지 않고 하나의 이름으로 import가 가능하다.
    - 별칭 지정 앞에 *로 모든 식별자를 as 뒤의 별칭 객체의 프로퍼티로 모아 import 가능하다.
    ```javascript
    import {pi as PI, square as sq, Person as P} from './lib.mjs';
    console.log(PI);
    console.log(sq(10));
    console.log(new P('Lee'));
    ```
    - 모듈이 export 한 식별자 이름을 as로 별칭을 지정해 이름 변경 가능하다.
    ```javascript
    export default x => x * x;
    ```
    - 모듈에서 하나의 값만 export 하고 싶다면 뒤에 default를 사용해 하나의 값만 export가능하게 한다.
    - but, default를 사용한다면 var, let, const 키워드는 사용하지 못한다.
    ```javascript
    import square from './lib.mjs'
    ```
    - default 키워드로 export한 모듈은 {}없이 임의의 이름으로 import 한다.

# 49장 Babel과 Webpack을 이용한 ES6+/ES.NEXT 개발 환경 구축
- IE 11의 ES6 지원율은 약 11%이며 매년 새롭게 도입되는 ES6+와 제안단계에 있는 ES 제안 사양은 브라우저에 따라 지원률이 제각각임
- 따라서 ES6+, ES, NEXT의 최신 ECMAScript 사양을 사용해 프로젝트를 진행하려면 최신 사양으로 작성된 코드를 어떤 브라우저든 문제 없이 작동시키기 위한 개발 환경 구축 필요
- 또한 ESM보다는 별도의 모듈 로더를 사용하는 것이 일반적이다.
    - IE를 포함한 구형 브라우저는 ESM을 지원하지 않는다.
    - ESM을 사용해도 트랜스파일링이나 번들링이 필요하다.
    - ESM이 아직 지원하지 않는 기능이 있고 몇가지 이슈가 존재한다.
- 따라서 트랜스파일러인 Babel, 모듈 번들러 Webpack을 이용해 ES6+/ES.NEXT 개발 환경을 구축해보자.
## Babel
- Babel은 ES6+/ES.NEXT로 구현된 최신 사양의 소스코드를 IE 같은 구형 브라우저에서도 동작하는 ES5 사양의 소스코드로 변환(트랜스파일링) 가능하다.
- Babel 설치
    ```
    # 프로젝트 폴더 생성
    $ mkdir esnext-project && cd esnext-project

    # package.json 생성
    $ npm init -y

    # babel-core, babel-cli 설치
    $ npm install --save-dev @babel/core 
    // --save-dev로 설치 시 개발 시에만 필요한 패키지(`devDependencies`)를 설치함
    ```
    - 만약 패키지 버전을 지정하고 싶다면 이름 뒤에 @과 설치하고 싶은 버전을 지정하면 된다.
        ```
        $ npm install --save-dev @babel/core@7.10.3@babel/cli@7.10.3
        ```
- Babel 프리셋 설치와 babel.config.json 설정 파일 작성
    - Babel을 사용하려면 Babel과 함께 사용되어야하는 플러그인을 모아둔 Babel 프리셋을 설치해야한다.
    - Bebel 프리셋 종류
        - @babel/preset-env
        - @babel/preset-flow
        - @babel/preset-react
        - @babel/preset-typescript
    - @babel/preset-env는 필요한 플러그인들을 프로젝트 지원환경에 맞춰 동적으로 결정해준다.
    - 프로젝트 지원환경은 Browserslist 형식으로 .borwserslistrc 파일에 설정 가능하다.
        - 환경설정 작업을 생략하면 기본값으로 설정
    ```
    $ npm install --save-dev @babel/preset-env
    ```
    - 설치가 완료되면 프로젝트 루트 폴더에 babel.config.json 설정 파일을 생성 후 사용 코드를 작성한다.
    ```javascript
    {
        "presets" : ["@babel/preset-env"]
    }
    ```
## 트랜스파일링
- Babel을 사용해 ES6+/ES.NEXT 사양의 코드를 ES5 사양의 코드로 트랜스파일링 하는 것은 Babel CLI 명령어를 사용할 수도 있으나 매번 Babel CLI 명령어를 입력하는 것은 번거롭기 때문에 npm scripts에 Babel CLI 명령어를 등록해 사용하면 간편하다.
- package.json 파일에 script를 추가
```json
{
    "name" : "esnext-project",
    "version" : "1.0.0",
    "scripts" : {
        "build" : "babel src/js -w -d dist /js"
    },
    "devDependencies" : {
        "@babel/cli" : "^7.10.3",
        "@babel/core" : "7.10.3",
        "@babel/preset-env" : "^7.10.3"
    }
}
```
- build는 src/js 폴더에 있는 모든 자바스크립트 파일을 트랜스파일링 후, dist/js에 저장한다.
    - -w: 타깃 폴더에 있는 모든 자바스크립트 파일의 변경을 감지해 자동으로 트랜스파일한다.
    - -d: 트랜스파일링된 결과물이 저장될 폴더를 지정한다. 폴더가 없다면 자동 생성한다.
## Babel 플러그인 설치
- public/private 클래스 필드를 지원하는 @babel/plugin-proposal-class-properties 설치
```
$ npm install --save-dev @babel/plugin-proposal-class-properties
```
- 설치가 완료됬다면 babel.config.json 설정 파일에 추가해야함
```json
{
    "presets" : ["@babel/preset-env"]
    "plugins" : ["@babel/plugin-proposal-class-properties"]
}
```
- 추가가 완료되면 트랜스파일링 실행
```
$ npm run build
```
- 트랜스파일링에 성공하면 dist/js 폴더가 자동생성되고 트랜스파일링된 main.js와 lib.js가 저장됨
## 브라우저에서 모듈 로딩 테스트
- 위에서 트랜스파일링된 것은 Node.js 환경에서는 CommonJS 방식의 모듈 로딩 시스템에 의해 잘 동작하나 브라우저는 CommonJS 방식의 require 함수를 지원하지 않으므로 위에서 트랜스파일링 된 결과를 그대로 브라우저에서 실행하면 에러가 발생한다.
- 따라서 Webpack을 통해 이러한 문제를 해결 가능하다.
## Webpack
- Webpack은 의존 관계에 있는 자바스크립트, css, 이미지 등이 리소스들을 하나(또는 여러개) 파일로 번들링하는 모듈 번들러
- 의존 모듈이 하나의 파일로 번들링되어 별도의 모듈 로더가 필요하지 않음
- 여러개의 자바스크립트 파일을 하나로 번들링해 HTML파일에서 script 태그로 여러 개의 파일을 로드해야하는 번거로움도 없음
- Webpack 설치
    ```
    $ npm install --save-dev webpack webpack-cli
    ```
- babel-loader 설치
    - Webpack이 모듈을 번들링할 때 Babel을 사용해 트랜스파일링이 되도록 babel-loader 설치
    ```
    $ npm install --save-dev babel-loader
    ```
- babel 대신 Webpack을 실행하도록 package.json수정
```json
{
    "name" : "esnext-project",
    "version" : "1.0.0",
    "scripts" : {
        "build" : "webpack -w"
    },
    "devDependencies" : {
        "@babel/cli" : "^7.10.3",
        "@babel/core" : "^7.10.3",
        "@babel/plugin-proposal-class-properties" : "^7.10.1",
        "@babel/preset-env" : "^7.10.3",
        "babel-loader" : "^7.10.3",
        "webpack" : "^4.43.0",
        "webpack-cli" : "^3.3.12"
    }
}
```
- webpack.config.js 설정 파일 작성
    - webpack.config.js는 Webpack이 실행될 때 참조하는 설정파일, 프로젝트 루트폴더에 webpack.config.js 파일생성
    ```javascript
    const path = require('path');

    module.exports = {
        entry: './src/js/main.js',
        output: {
            path: path.resolve(__dirname, 'dist'),
            filename: 'js/bundle.js'
        },
        module: {
            rules: [
                {
                    test: ^.js$/,
                    include: [
                        path.resolve(__dirname, 'src/js')
                    ],
                    exclude: /node_modules/,
                    use: {
                        loader: 'babel-loader',
                        options: {
                            presets: ['@babel/preset-env'],
                            plugins: ['@babel/plugin-proposal-class-properties']
                        }
                    }
                }
            ]
        },
        devtool: 'soure-map',
        mode: 'development'
    };
    ```
- babel-polyfill 설치
    - Promise, Object.assign, Array.from 등은 트랜스파일링이 되지 못하고 그대로 남아서 이를 위해 babel-polyfill을 설치한다.
    ```
    npm install @babel/polyfill
    ```
    - 이는 개발 환경에서만 사용하는 것이 아닌 실제 운영 환경에서도 사용해야하기 때문에 --save-dev 옵션을 지정하지 않는다.
    - babel만 사용하는 경우 진입점 파일의 선두에 import @babel/polyfill";을 작성한다.
    - Webpack을 사용하는 경우 entry 배열에 폴리필을 추가하여 사용한다. 
        - entry: ['@babel/polyfill', './src/js/main.js'],
    - 이후 명령어로 실행 후, 번들링된 파일을 확인해보면 폴리필이 추가된 것을 확인할 수 있다.
