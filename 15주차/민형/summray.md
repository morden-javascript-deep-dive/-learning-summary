# Ajax

## Ajax란

- Ajax란 자바스크립트를 사용하여 브라우저가 서버에게 비동기 방식으로 데이터를 요청하고, 서버가 응답한 데이터를 수신하여 웹페이지를 동적으로 갱신하는 프로그래밍 방식을 말한다.
- Ajax는 브라우저에서 제공하는 Web API인 XMLHttpRequest 객체를 기반으로 동작한다.
- XMLHttpRequest는 HTTP 비동기 통신을 위한 메서드와 프로퍼티를 제공한다.
- 이전의 웹페이지는 html 태그로 시작해서 html 태그로 끝나는 완전한 HTML을 서버로부터 전송받아 웹페이지 전체를 처음부터 다시 렌더링하는 방식으로 동작했다.
  - 화면이 전환되면 서버로부터 HTML을 전송받아 웹페이 전체를 처음부터 다시 렌더링했다.
- 위의 방식 문제점
  - 이전 웹페이지와 차이가 없어서 변경할 필요가 없는ㄴ 부분까지 포함된 HTML을 서버로부터 매번 다시 전송받기 때문에 불필요한 데이터 통신이 발생한다.
  - 변경할 필요가 ㅇ벗는 부분까지 처음부터 다시 렌더링한다(깜빡이는 화면이 발생)
  - 클라이언트와 서버와의 통신이 동기 방식으로 동작하기 때문에 서버로부터 응답이 있을 때까지 다음 처리는 블로킹된다.
- Ajax 장점
  - 변경할 부분을 갱신하는데 필요한 데이터만 서버로부터 전송받기 때문에 데이터 통신이 발생하지 않는다.
  - 변경할 필요가 없는 부분은 다시 렌더링 하지 않는다.(깜빡이는 화면이 발생하지 않음)
  - 클라이언트와 서버와의 통신이 비동기 방식으로 동작하기 때문에 서버에게 요청을 보낸 이후 블로킹이 발생하지 않는다.

## json

- json(javascript object notation)은 크라이넝트와 서버간의 HTTP 통신을 통한 텍스트 데이터 포맷

1. json 표기 방식

- json은 자바스크립트의 객체 리터럴과 유사하게 키왁 ㅏㅄ으로 구성된 순수한 텍스트

```js
{
  "name": "Son",
  "age": 20,
  "alive": true,
  "hobby": ["traveling", "football"]
}
```

2. json.stringfigy

- 해당 메서드는 객체를 json 포맷의 문자열로 변환
- 클라이언트가 서버로 객체를 전송하려면 객체를 문자열화해야하는데 이것을 직력화라 한다.

```js
const obj = {
  name: "Son",
  age: 20,
  alive: true,
  hobby: ["traveling", "football"],
};

// 객체를 JSON 포맷의 문자열로 변환
const json = JSON.stringify(obj);
console.log(typeof json, json);

// string {"name":"Son","age":20,"alive":true,"hobby":["traveling","football"]}

// 객체를 JSON 포맷의 문자열로 변환하면서 들여쓰기 한다.
const prettyJson = JSON.stringify(obj, null, 2);
console.log(typeof prettyJson, prettyJson);

/*
string {
  "name": "Son",
  "age": 20,
  "alive": true,
  "hobby": [
    "traveling",
    "football"
  ]
}
*/

// replacer 함수. 값의 타입이 Number이면 필터링되어 반환하지 않음
function filter(key, value) {
  // undefined: 반환하지 않음
  return typeof value === "number" ? undefined : value;
}

// JSON.stringfy 메서드에 두 번째 인수로 replacer 함수를 전달한다.
const strFilteredObject = JSON.stringify(obj, filter, 2);
console.log(typeof strFilteredObject, strFilteredObject);

/*
string {
  "name": "Son",
  "alive": true,
  "hobby": [
    "traveling",
    "football"
  ]
}
*/
```

- json.stringfy 메서드는 객체뿐만 아니라 배열도 json 포맷의 문자열로 변환

```js
const todos = [
    {id: 1, content: 'HTML', completed: false},
    {id: 2, content: 'CSS', completed: true},
    {id: 3, content: 'JavaScript', completed: false},
];

// 배열을 JSON 포맷의 문자열로 변환한다.
const json = JSON.stringify(todos, null, 2);
console.log(typeof json, json);

/*
string [
  {
    "id": 1,
    "content": "HTML",
    "completed": false
  },
  {
    "id": 2,
    "content": "CSS",
    "completed": true
  },
  {
    "id": 3,
    "content": "JavaScript",
    "completed": false
  }
]
/*
```

3. json.parse

- 해당 메서드는 json 포맷의 문자열을 객체로 변환
- 서버로부터 클라이언트에게 전송된 json 데이터는 문자열이다. 이 문자열을 객체로서 사용하려면 json 포맷의 문자열을 객체화해야 하는데 이를 역직렬화라 한다.
- 배열이 json 포맷의 문자열로 변환되어 있는 경우 json.parse는 문자열을 배열 객체로 변환한다.
- 배열의 요소가 객체인 경우 배열의 요소까지 객체로 변환

```js
const todos = [
  { id: 1, content: "HTML", completed: false },
  { id: 2, content: "CSS", completed: true },
  { id: 3, content: "JavaScript", completed: false },
];

// 배열을 JSON 포맷의 문자열로 변환한다.
const json = JSON.stringify(todos);

// JSON 포맷의 문자열을 배열로 변환. 배열의 요소까지 객체로 변환된다.
const parsed = JSON.parse(json);
console.log(typeof parsed, parsed);

/*
object [
  { id: 1, content: 'HTML', completed: false },
  { id: 2, content: 'CSS', completed: true },
  { id: 3, content: 'JavaScript', completed: false }
]
*/
```

## XMLHttpRequest

- 브라우저는 주소창이나 HTML의 form 태그 또는 a 태그를 통해 HTTP 요청 전송 기능을 기본 제공한다.
- 자바스크립트를 사용하여 HTTP 요청을 전송하려면 XMLHttpRequest 객체를 사용한다.
- Web API인 XMLHttpRequest 객체는 HTTP 요청 전송과 HTTP 응답 수신을 위한 다양한 메서드와 프로퍼티를 제공한다.

1. XMLHttpRequest 객체 생성

```js
// XMLHttpRequest 객체 생성
const xhr = new XMLHttpRequest();
```

2. XMLHttpRequest 객체의 프로퍼티와 메서드

- XMLHttpRequest 객체의 프로토타입 프로퍼티
  - readyState: HTTP 요청의 현재 상태를 나타내는 정수
  - status: HTTP 요청에 대한 응답 상태(HTTP 상태 코드)를 나타내는 정수
  - statusText: HTTP 요청에 대한 응답 메시지를 나타내는 문자열
  - responseType: HTTP 응답 타입
  - response: HTTP 요청에 대한 응답 몸체(reponseBody), responseTYpe에 따라 타입이 다르다.
- XMLHttpRequest 객체의 이벤트 핸들러 프로퍼티
  - onreadystatechage: readyState 프로퍼티 값이 변경된 경우
  - onerror: HTTP 요청에 에러가 발생한 경우
  - onload: HTTP 요청이 성공적으로 완료한 경우
- XMLHttpRequest 객체의 메서드
  - open: HTTP 요청 초괴화
  - send: HTTP 요청 전송
  - abort: 이미 전송된 HTTP 요청 중단
  - setReqeustHeader: 특정 HTTP 요청 헤더의 값을 설정

3. HTTP 요청 전송

```js
// XMLHttpRequest 객체 생성
const xhr = new XMLHttpRequest();

// HTTP 요청 초기화
xhr.open("GET", "/users");

// HTTP 요청 헤더 설정
// 클라이언트가 서버로 전송할 데이터의 MIME 타입 지정: json
xhr.setRequestHeader("content-type", "application/json");

// HTTP 요청 전송
xhr.send();
```

- XMLHttpRequest.prototype.open

  - open 메서드는 서버에 전송할 HTTP 요청을 초기화한다.

  ```js
  xhr.open(method, url[, async]);
  ```

  - method: HTTP 요청 메서드(GET, POST, PUT, PATCH, DELETE)
  - url: HTTP 요청을 전송한 URL
  - async, 비동기 요청 여부, 옵션으로 기본값은 true

- XMLHttpRequest.prototype.send

  - send 메서드는 open 초기화된 HTTP 요청을 서버에 전송한다.
  - 요청 몸체에 담아 전송할 데이터(페이로드)를 인수로 전달할 수 있다.
  - 페이로드가 객체인 경우 json.stringfy 메서드를 사욯하여 직렬화한 다음 전달해야한다.

  ```js
  xhr.send(JSON.stringfy({ id: 1, content: "HTML", completed: false }));
  ```

  - HTTP 요청 메서드가 GET인 경우 send 메서드에 페이로드로 전달할 인수는 무시되고 요청 몸체는 null로 설정된다.

- XMLHttpRequest.prototype.setRequestHeader

  - setRequestHeader 메서드는 특정 HTTP 요청의 헤더 값을 설정한다.
  - 반드시 open 메서드를 호출한 이후에 호출해야한다.
  - content-type: 요청 몸체에 담아 전송할 데이터의 MIME 타입의 정보를 표현
    - text: text/plain, text/html, text/css, text/javascript
    - application: application/json
    - mutipart: mutipart/fomed-data
  - HTTP 클라이언트가 서버에 요청할 때 서버가 응답할 데이터의 MIME 타입을 Accept로 지정할 수 있다.

  ```js
  // 서버가 응답할 데이터의 MIME 타입 지정: json
  xhr.setRequestHeader("accept", "application/json");
  ```

  - AccEPT 헤더를 설정하지 않으면 SEND 메서드가 호출될 떄 aCCEPT 헤더가 */*로 전송

4. HTTP 응답 처리

- 서버가 전송한 응답을 처리하려면 XMLHttpRequest 객체가 발생시키는 이벤트를 캐치해야한다.

```js
// XMLHttpRequest 객체 생성
const xhr = new XMLHttpRequest();

// HTTP 요청 초기화
xhr.open("GET", "/users");

// HTTP 요청 전송
xhr.send();

// readystatechange 이벤트는 HTTP 요청의 현재 상태를 나타내는 readyState 프로퍼티가
// 변경될 때마다 발생한다.
xhr.onreadystatechange = () => {
  // readyState 프로퍼티는 HTTP 요청의 현재 상태를 나타낸다.
  // readyState 프로퍼티 값이 4(XMLHttpRequest.DONE)가 아니면 서버 응답이 완료되지 않음.
  // 서버 응답 완료되지 않으면 아무런 처리하지 않음
  if (xhr.readyState !== XMLHttpRequest.DONE) return;

  // status 프로퍼티는 응답 상태 코드를 나타낸다.
  // status 프로퍼티 값이 200이 정상 코드
  // 200이 아니면 error 발생
  if (xhr.status === 200) {
    console.log(JSON.parse(xhr.response));
  } else {
    console.error("Error", xhr.status, xhr.statusText);
  }
};
```

- readystatechage 이벤트 대신 load 이벤트를 캐치해도 된다.
- load 이벤트는 HTTP 요청이 성공적으로 완료된 경우 발생한다.
  - load 이벤트를 캐치하는 경우 xhr.readystate가 XMLHTTPRequest.DONE인지 확인할 필요가 없다.

```js
// XMLHttpRequest 객체 생성
const xhr = new XMLHttpRequest();

// HTTP 요청 초기화
xhr.open("GET", "/users");

// HTTP 요청 전송
xhr.send();

// load 이벤트는 HTTP 요청이 성공적으로 완료된 경우에만 발생
xhr.onload = () => {
  // status 프로퍼티는 응답 상태 코드를 나타낸다.
  // status 프로퍼티 값이 200이 정상 코드
  // 200이 아니면 error 발생
  if (xhr.status === 200) {
    console.log(JSON.parse(xhr.response));
  } else {
    console.error("Error", xhr.status, xhr.statusText);
  }
};
```

# REST API

## REST API의 구성

- REST API는 자원, 행위, 표현의 3가지 요소로 구성된다. REST는 자체 표현 구조로 구성되어 REST API만으로 HTTP 요청의 내용을 이해할 수 있다.

- 구성 요소(자원, 행위, 표현)
- 내용(자원, 자원에 대한 행위, 자원에 대한 행위의 구체적 내용)
- 표현 방법(URI(엔드 포인트), HTTP 요청 메서드, 페이로드)

## REST API 설계 원칙

1. URI는 리소스를 표현해야한다.

- URI는 리소스를 표현하는데 중점을 두어야한다.

```
GET /todos/1;
```

2. 리소스에 대한 행위는 HTTTP 요청 메서드로 표현한다.

- 주로 5가지 요청 메서드를 사용해 CRUD를 구현
- HTTP 요청 메서드(get, post, put, patch, delete)
- 종류(index/retrieve, create, replace, modify, delete)
- 목적(모든/특정 리소스 취득, 리소스 생성, 리소스의 전체 교체, 리소스의 일부 수정, 모든/특정 리소스 삭제)
- 페이로드(x, o, o, o, x)

# 프로미스

- ES5에서는 비동기 처리를 위한 또 다른 패턴으로 프로미스를 도입했다. 프로미스는 전통적인 콜백 패턴이 가진 단점을 보완하며 미동기 처리 시점을 명확하게 표현할 수 있다는 장점이 있다.

## 비동기 처리를 위한 콜백 패턴의 단점

1. 콜백 헬

```js
// GET 요청을 위한 비동기 함수
const get = (url) => {
  const xhr = new XMLHttpRequest();
  xhr.open("GET", url);
  xhr.send();

  xhr.onload = () => {
    if (xhr.status === 200) {
      // 서버의 응답을 콘솔에 출력한다.
      console.log(JSON.parse(xhr.response));
    } else {
      console.error(`${xhr.status} ${xhr.statusText}`);
    }
  };
};

// id가 1인 post를 취득
get("https://jsonplaceholder.typicode.com/posts/1");
```

- get 함수는 비동기 함수다. 비동기 함수를 호출하면 함수 내부의 디오기로 동작하는 코드가 완료되지 않았다 해도 기다리지 않고 즉시 종료된다.
- 즉, 비동기 함수 내부의 비동기로 동작하는 코드는 비동기 함수가 종료된 이후에 완료된다.
- 비동기 함수 내부의 비동기로 동작하는 코드에서 처리 결과를 외부로 반환하거나 사위 스코프의 변수에 할당하면 기대한 대로 동작하지 않는다.
- seTimeout 함수를 활용한 코드

```js
let g = 0;

// 비동기 함수인 setTimeout 함수는 콜백 함수의 처리 결과를
// 외부로 반환하거나 상위 스코프의 변수에 할당하지 못한다.
setTimeout(() => {
  g = 100;
}, 0);
console.log(g); // 0
```

- onload 이벤트 핸들러가 비동기로 동작한다.

  - get 함수의 onload 이벤트 핸들러에서 서버의 응답 결과를 반환하거나 상위 스코프의 변수에 할당하면 기대한 대로 동작하지 않는다.

  ```js
  // GET 요청을 위한 비동기 함수
  const get = (url) => {
    const xhr = new XMLHttpRequest();
    xhr.open("GET", url);
    xhr.send();

    xhr.onload = () => {
      if (xhr.status === 200) {
        // 서버의 응답을 콘솔에 출력한다.
        console.log(JSON.parse(xhr.response));
      } else {
        console.error(`${xhr.status} ${xhr.statusText}`);
      }
    };
  };

  // id가 1인 post를 취득

  const response = get("https://jsonplaceholder.typicode.com/posts/1");
  console.log(response); // undefined
  ```

- xhr.onload 핸들러 프로퍼티에 바인딩한 이벤트 핸들러가 즉시 싱행되는 것이 안디.
- xhr.onload 이벤트 핸들러는 load 이벤트가 밸생하면 일단 태스크 큐에 저장되어 대기하다, 콜 스택이 비면 이벤트 루프에의해 콜 스택으로 푸시되어 실행 된다.
- 비동기 함수는 처리 결과를 외부에 반환할 수 없고, 상위 스코프의 변수에 할당할 수도 없다.
- 따라서 비동기 함수의 처리결과(서버의 응답 등)에 대한 후속처리는 비동기 함수 내부에서 수행해야한다.
- 이때 비동기 함수를 범용적으로 사용하기 위해 비동기 함수에 비동기 처리 결과에 대한 후속 처리를 수행하는 콜백 함수를 전달하는 것이 일반적이다.
- 필요에 따라 비동기 처리가 성공하면 호출될 콜백함수와 비동기 처리가 실패하면 호출될 콜백함수를 전달할 수 있다.

```js
// GET 요청을 위한 비동기 함수
const get = (url, successCallback, failureCallback) => {
  const xhr = new XMLHttpRequest();
  xhr.open("GET", url);
  xhr.send();

  xhr.onload = () => {
    if (xhr.status === 200) {
      // 서버의 응답을 콜백 함수에 인수로 전달하면서 호출하여 응답에 대한 후속처리를 한다.
      successCallback(JSON.parse(xhr.response));
    } else {
      // 에러 정보를 콜백 함수에 인수로 전달하면서 호출하여 에러 처리를 한다.
      failureCallback(xhr.status);
    }
  };
};

// id가 1인 post를 취득
// 서버의 응답에 대한 후속 처리를 위한 콜백 함수를 비동기 함수인 get에 전달한다.
get("https://jsonplaceholder.typicode.com/posts/1", console.log, console.error);
```

- 이처럼 콜백함수를 비동기 처리 결과에 대한 후속 처리를 수행하는 비동기 함수가 비동기 처리 결과를 가지고 또다시 비동기 함수를 호출해야 한다면 콜백 함수 호출이 중첩되어 복잡도가 높아지는 현상이 발생하는데, 이를 콜백 헬이라 한다.

```js
// GET 요청을 위한 비동기 함수
const get = (url, callback) => {
  const xhr = new XMLHttpRequest();
  xhr.open("GET", url);
  xhr.send();

  xhr.onload = () => {
    if (xhr.status === 200) {
      // 서버의 응답을 콜백 함수에 인수로 전달하면서 호출하여 응답에 대한 후속처리를 한다.
      callback(JSON.parse(xhr.response));
    } else {
      console.error(`${xhr.status} ${xhr.statusText}`);
    }
  };
};

const url = "https://jsonplaceholder.typicode.com/posts/1";

// id가 1인 post의 userId를 취득
get(`{$url}/posts/1`, ({ userId }) => {
  console.log(userId); // 1
  // post의 userId를 사용하여 user 정보를 취득
  get(`${url}/users/${userId}`, (userInfo) => {
    console.log(userInfo);
  });
});
```

- GET 요청을 통해 서버로부터 응답을 취득하고 이 데이터를 사용하여 또다시 GET 요청을한다.
- 콜백 헬은 가독성을 나쁘게 하며 실수를 유발하는 원인이된다.
- 다음이 콜백 헬이 발생하는 전형적인 사례

```js
get("/step1", (a) => {
  get(`/step2/${a}`, (b) => {
    get(`/step3/${b}`, (c) => {
      get(`/step4/${c}`, (d) => {
        console.log(d);
      });
    });
  });
});
```

2. 에러 처리의 한계

- 비동기 처리를 위한 콜백 패턴의 문제점 중에서 가장 심각한 것은 에러 처리가 곤란하다는 점이다.

```js
try {
  setTimeout(() => {
    throw new Error(`Error!`);
  }, 1000);
} catch (e) {
  // 에러를 캐치하지 못한다.
  console.error("캐치한 에러", e);
}
```

- 에러는 호출자 방향으로 전파된다. 즉, 콜 스택의 아래 방향(실행 중인 실행 컨텍스트가 푸시되기 직전에 퓟된 실행 컨텍스트 방향)으로 전파된다.
- 하지만 setTimeout 함수의 콜백 함수를 호출한 것은 setTimeout 함수가 아니다.
  - setTimeout 함수의 콜백 함수가 발생시킨 에러는 catch 블록에서 캐치되지 않는다.

## 프로미스의 생성

- Promise 생성자 함수를 new 연산자와 함께 호출하면 프로미스를 생성한다.
- Promise 생성자 함수는 비동기 처리를 수행할 콜백 함수를 인수로 전달받는데 이 콜백 함수는 resolve와 reject 함수를 인수로 전달받는다.

```js
// 프로미스 생성
const promise = new Promise((resolve, reject) => {
   // Promise 함수의 콜백 함수 내부에서 비동기 처리를 수행한다.
    if (/* 비동기 처리 성공 */) {
        resolse('result');
    } else {
        // 비동기 처리 실패
        reject('failure reason');
    }
});
```

- Promise 생성자 함수가 인수로 전달받은 콜백 함수 내부에서 비동기 처리를 수행한다. 이떄 비동기 처리가 성공하면 콜백 함수의 인수로 전달받은 resolve 함수를 호출하고, 비동기 처리가 실패하면 reject 함수를 호출한다.
- 다음은 비동기 함수 get을 프로미스를 사용하여 구현

```js
// GET 요청을 위한 비동기 함수
const promiseGet = (url) => {
  return new Promise((resolve, reject) => {
    const xhr = new XMLHttpRequest();
    xhr.open("GET", url);
    xhr.send();

    xhr.onload = () => {
      if (xhr.status === 200) {
        // 성공적으로 응답을 전달받으면 resolve 함수를 호출한다.
        resolve(JSON.parse(xhr.response));
      } else {
        // 에러 처리를 위해 reject 함수를 호출한다.
        reject(new Error(xhr.status));
      }
    };
  });
};

promiseGet("https://jsonplaceholder.typicode.com/posts/1");
```

- 비동기 처리는 Promise 생성자 함수가 인수로 전달받은 콜백 함수 내부에서 수행한다.
- 프로미스는 다음과 같이 현재 비동기 처리가 어떻게 진행되고 있는지를 나타내는 상태 정보를 갖는다.
  - pending: 비동기 처리가 아직 수행하지 않은 상태(프로미스가 생성된 직후 기본상태)
  - fulfilled: 비동기처리가 수행된 상태(성공, resolve 함수 호출)
  - rejected: 비동기 처리가 수행된 상태(실패, reject 함수 호출)
- 생성된 직후의 프로미스는 기본적으로 pending 상태다. 이후 비동기 처리가 수행되면 비동기 처리 결과에 따라 다음과 같이 프로미스의 상태가 변경된다.
  - 비동기 처리 성공: resolve 함수를 호출해 프로미스를 fullfilled 상태로 변경한다.
  - 비동기 처리 실패: reject 함수를 호출해 프로미스를 rejected 상태로 변경한다.
- 프로미스의 상태는 resolve 또는 reject 함수를 호출하는 것으로 결정된다.
- fulfilled 또는 rejected 상태를 settled 상태라고 한다. settled 상태는 fulfilled 또는 rejected 상태를 settled 상태와 상관없이 pending이 아닌 상태로 비동기 처리가 수행된 상태를 말한다.
- **`프로미스는 비동기 처리 상태와 처리 결과를 관리하는 객체`**

## 프로미스의 후속 처리 메서드

- 프로미스의 비동기 처리 상태가 변화하면 후속 처리 메서드에 인수로 전달한 콜백 함수가 선택적으로 호출된다.

1. then

- then 메서드는 두개의 콜백 함수를 인수로 전달받는다(fullfilled, rejected)
- 첫번째 콜백함수는 비동기 처리가 성공했을 때 호출되는 성공 처리 콜백함수이며, 두 번째 콜백 함수는 비동기 처리가 실패했을 때 호출되는 실패 처리 콜백함수다.

```js
// fulfilled
new Promise((resolve) => resolve("fulfilled")).then(
  (v) => console.log(v),
  (e) => console.log(e)
); // fulfilled

// rejected
new Promise((_, reject) => reject(new Error("rejected"))).then(
  (v) => console.log(v),
  (e) => console.error(e)
); // Error: rejected
```

- then 메서드는 언제나 프로미스를 반환한다.

2. catch

- catch 메서드는 한 개의 콜백 함수를 인수로 전달받는다. catch 메서드의 콜백함수는 프로미스가 rejected 상태인 경우만 호출된다.

```js
// rejected
new Promise((_, reject) => reject(new Error("rejected"))).catch((e) =>
  console.log(e)
); // Error: rejected
```

3. finally

- finally 메서드는 한 개의 콜백 함수를 인수로 전달받는다. finally 메서드의 콜백 함수는 프로미스의 성공 또는 실패와 상관없이 무조건 한 번 호출된다.
- finally 메서드는 프로미스의 상태와 상고나없이 공통적으로 수행해야 할 퍼리 내용이 있을 때 유용하다.
- 마찬가지로 언제나 프로미스를 반환한다.

```js
// rejected
new Promise(() => {}).finally(() => console.log("finally")); // finally
```

- 프로미스로 구현한 비동기 함수 get을 사용해 후속 처리

```js
const promiseGet = (url) => {
  return new Promise((resolve, reject) => {
    const xhr = new XMLHttpRequest();
    xhr.open("GET", url);
    xhr.send();

    xhr.onload = () => {
      if (xhr.status === 200) {
        // 성공적으로 응답을 전달받으면 resolve 함수를 호출한다.
        resolve(JSON.parse(xhr.response));
      } else {
        // 에러 처리를 위해 reject 함수를 호출한다.
        reject(new Error(xhr.status));
      }
    };
  });
};

// promiseGet 함수는 프로미스를 반환한다.
promiseGet("https://jsonplaceholder.typicode.com/posts/1")
  .then((res) => console.log(res))
  .catch((err) => console.error(err))
  .finally(() => console.log("Bye!"));
```

## 프로미스의 에러 처리

- 비동기 함수 get은 프로미스를 반환한다. 비동기 처리 결과에 대한 후속 처리는 프로미스가 제공하는 후속 처리 메서드 then, catch, finally를 사용하여 수행
- catch 메서드를 호출하면 내부적으로 then(undefined, onRejected)를 호출
- catch 메서드는 모든 then 메서드를 호출한 이후에 호출하면 비동기 처리에서 발생한 에러뿐만 아니라 then 메서드 내부에서 발생한 에러까지 모두 캐치할 수 있다.

```js
const wrongUrl = "https://jsonplaceholder.typicode.com/XXX/1";

promiseGet(wrongUrl)
  .then((res) => console.log(res))
  .catch((err) => console.error(err));
```

- then 메서드에 두 번째 콜백 함수를 전달하는 것보다 catch 메서드를 사용하는 것이 가독성이 좋고 명확

## 프로미스 체이닝

```js
const url = "https://jsonplaceholder.typicode.com";

// id가 1인 post의 userId를 취득
promiseGet(`${url}/posts/1`)
  // 취득한 post의 userId로 user 정보를 취득
  .then(({ userId }) => promiseGet(`${url}/users/${userId}`))
  .then((userInfo) => console.log(userInfo))
  .catch((err) => console.error(err));
```

- 프로미스는 프로미스 체이닝을 통해 비동기 처리 결과를 전달받아 후속 처리를 하므로 비동기 처리를 위한 콜백 패턴에서 발생하던 콜백 헬이 발생하지 않는다.
- 다만 프로미스도 콜백 패턴을 사용하므로 콜백 함수를 사용하지 않는 것은 아니다.
- 콜백 패턴은 가독성이 좋지 않아 ES8에서 도입된 async/await를 통해 해결할 수 있다.

```js
const url = "https://jsonplaceholder.typicode.com";

(async () => {
  // id가 1인 post의 userId를 취득
  const { userId } = await promiseGet(`${url}/posts/1`);
  // 취득한 post의 userId를 user 정보를 취득
  const userInfo = await promiseGet(`${url}/users/${userId}`);

  console.log(userInfo);
})();
```

## 프로미스의 정적 메서드

1. Promise.resolve / Promise.reject

- 이미 존재하는 값을 래핑하여 프로미스를 사용하기 위해 사용
- Promise.resolve 메서드는 인수로 전달받은 값을 resolve하는 프로미스를 생성

```js
// 배열을 resolve하는 프로미스를 생성
const resolvePromise = Promise.resolve([1, 2, 3]);
// 위 코드가 아래와 같다.
const resolvePromise = new Promise((resolve) => resolve([1, 2, 3]));
resolvePromise.then(console.log); // [1, 2, 3]
```

- Promise.reject 메서드는 인수로 전달받은 값을 reject하는 프로미스를 생성

```js
// 배열을 resolve하는 프로미스를 생성
const rejectedPromise = Promise.reject(new Error("Error!"));
// 위 코드가 아래와 같다.
const rejectedPromise = new Promise((_, reject) => reject(new Error("Error!")));
rejectedPromise.catch(console.log); // Error: Error!
```

2. Promise.all

- Promise.all 메서드는 여러개의 비동기 처리를 모두 병렬처리할 떄 사용

```js
const requestData1 = () =>
  new Promise((resolve) => setTimeout(() => resolve(1), 3000));
const requestData2 = () =>
  new Promise((resolve) => setTimeout(() => resolve(2), 2000));
const requestData3 = () =>
  new Promise((resolve) => setTimeout(() => resolve(3), 1000));

// 세 개의 비동기 처리를 순차적으로 처리
const res = [];
requestData1()
  .then((data) => {
    res.push(data);
    return requestData2();
  })
  .then((data) => {
    res.push(data);
    return requestData3();
  })
  .then((data) => {
    res.push(data);
    console.log(res); // [1, 2, 3] => 약 6초 소요
  })
  .catch(console.error);
```

- 위 예제는 세개의 비동기 처리를 순차적으로 처리
- 총 6초 이상이 소요. 그러나 위 예제의 경우 세 개의 비동기 처리는 서로 의존하지 않고 개별적으로 수행
- 순차적으로 처리할 필요가 없기 때문에 병렬적으로 처리할 수 있는 방법을 알아본다.
- Promise.all 메서드를 사용

```js
const requestData1 = () =>
  new Promise((resolve) => setTimeout(() => resolve(1), 3000));
const requestData2 = () =>
  new Promise((resolve) => setTimeout(() => resolve(2), 2000));
const requestData3 = () =>
  new Promise((resolve) => setTimeout(() => resolve(3), 1000));

// 세 개의 비동기 처리를 순차적으로 처리
Promise.all([requestData1(), requestData2(), requestData3()])
  .then(console.log) // [1, 2, 3] => 약 3초 소요
  .catch(console.error);
```

- Promise.all 메서드는 인수로 전달받은 배열의 모든 프로미스가 모두 fulfilled 상태가 되면 종료한다.
- 모든 프로미스가 fulfilled 상태가 되면 resolve된 처리 결과를 모두 배열에 저장해 새로운 프로미스를 반환한다.
- 다른 프로미스가 가장 나중에 fulfilled 상태가 되어도 Promise.all 메서드는 첫 번쨰 프로미스가 resolve한 처리 결과부터 차례대로 배열에 저장해 그 배열을 resolve하는 새로운 프로미스를 반환(처리 순서가 보장)
- Promise.all 메서드는 인수로 전달받은 배열의 프로미스가 하나라도 rejecte 상태가 되면 나머지 프로미스가 fulfilled 상태가 되는 것을 기다리지 않고 즉시 종료

```js
Promise.all([
  new Promise((_, reject) =>
    setTimeout(() => reject(new Error("Error 1")), 3000)
  ),
  new Promise((_, reject) =>
    setTimeout(() => reject(new Error("Error 2")), 2000)
  ),
  new Promise((_, reject) =>
    setTimeout(() => reject(new Error("Error 3")), 1000)
  ),
])
  .then(console.log)
  .catch(console.error); // Error: Error 3
```

- 위 예제의 경우 세번쨰 프로미스가 가장 먼저 rejected 상태가 되므로 세번쨰 프로미스가 reject한 에러가 catch 메서드로 전달된다.
- Promise.all 메서드는 인수로 전달받은 이터러블의 요소가 프로미스가 아닌 경우 Promise.resolve 메서드를 통해 프로미스로 래핑한다.

- 다음은 깃허브 아이디로 깃허브 사용자 이름을 취득하는 3개의 비동기 처리를 모두 병렬로 처리하는 코드

```js
// GET 요청을 위한 비동기 함수
const promiseGet = (url) => {
  return new Promise((resolve, reject) => {
    const xhr = new XMLHttpRequest();
    xhr.open("GET", url);
    xhr.send();
    xhr.onload = () => {
      if (xhr.status === 200) {
        // 성공적으로 응답을 전달받으면 resolve 함수를 호출한다.
        resolve(JSON.parse(xhr.response));
      } else {
        // 에러 처리를 위해 reject 함수를 호출한다.
        reject(new Error(xhr.status));
      }
    };
  });
};

const githubIds = ["kses1010", "sunny1234", "cloud12"];

Promise.all(
  githubIds.map((id) => promiseGet(`https://api.github.com/users/${id}`))
)
  .then((users) => users.map((user) => user.name))
  .then(console.log)
  .catch(console.error);
```

3. Promise.race

- Promise.race 메서드는 Promise.all 메서드와 동일하게 프로미스를 요소로 갖는 배열 등의 이터러블을 인수로 전달받는다.
- Promise.race 메서드는 Promise.all 메서드처럼 모든 프로미스가 fulfilled 상태가 되는 것을 기다리는 것이 아니라 가장 먼저 fulfilled 상태가 된 프로미스의 처리 결과를 resolve 하는 새로운 프로미스를 반환

```js
Promise.race([
  new Promise((resolve) => setTimeout(() => resolve(1), 3000)), // 1
  new Promise((resolve) => setTimeout(() => resolve(2), 2000)), // 2
  new Promise((resolve) => setTimeout(() => resolve(3), 1000)), // 3
])
  .then(console.log) // 3
  .catch(console.log);
```

- 프로미스가 rejected 상태가 되면 Promise.all 메서드와 동일하게 처리
- 즉, Promise.race 메서드의 전달된 프로미스가 하나라도 rejected 상태가 되면 에러를 reject하는 새로운 프로미스를 즉시 반환

4. Promise.allSettled

- Promise.allSettled 메서드는 프로미스를 요소로 갖는 배열등의 이터러블을 인수로 전달받는다.
- 전달받은 프로미스가 모두 setteld 상태(fulfilled or rejecte)가 되면 처리결과를 배열로 반환

```js
Promise.allSettled([
  new Promise((resolve) => setTimeout(() => resolve(1), 2000)),
  new Promise((_, reject) =>
    setTimeout(() => reject(new Error("Error!")), 1000)
  ),
]).then(console.log);
```

- 해당 메서드가 반환하는 배열에는 fulfilled or rejected 상태와는 상관 없이 인수로 전달받은 모든 프로미스들의 처리 결과가 모두 담겨 있다.

  - fulfilled: 비동기 처리 상태 status 프로퍼티와 처리 결과를 나타내는 value 프로퍼티를 갖는다.
  - rejected: 비동기 처리 상태 status 프로퍼티와 에러를 나타내는 reason 프로퍼티를 갖는다.

  ```js
    [
  // 프로미스가 fulfilled 상태인 경우
  {status: "fulfilled", value: 1},
  // 프로미스가 rejected 상태인 경우
  {status: "rejected", reason: Error: Error! at <anonymous>:3:60}
  ]
  ```

## 마이크로태스크 큐

```js
setTimeout(() => console.log(1), 0);

Promise.resolve()
  .then(() => console.log(2))
  .then(() => console.log(3));
```

- 프로미스의 후속 처리 메서드도 비동기로 동작하므로 1,2,3 순으로 출력될 것처럼 보이지만 2,3,1의 순으로 출력
- 그 이유는 프로미스의 후속 처리 메서드의 콜백 함수는 태스크 큐가 아니라 마이크로태스크 큐에 저장되기 떄문
- 마이크로 태스크 큐에는 프로밋의 후속 처리 메서드의 콜백함수가 일시 저장된다. 마이크로 태스트 큐는 태스크 큐보다 우선순위가 높다.
- 즉, 이벤트 루프는 콜 스택이 비면 먼저 마이크로태스크 큐에서 대기하고 있는 함수를 가져와 실행
- 이후 마이크로태스크 큐가 비면 태스크 뮤에서 대기하고 이쓴ㄴ 함수를 가져와 실행

## fetch

- fetch 함수는 XMLHttpRequest 객체와 마찬가지로 HTTP 요청 전송 기능을 제공하는 클라이언트 사이드 Web API다.
- fetch 함수는 XMLHttpReques 객체보다 사용법이 간단하고 프로미스를 지원하기 때문에 비동기 처리를 위한 콜백 패턴의 단점에서 자유롭다.
- fetch 함수는 HTTP 응답을 나타내는 Response 객체를 래핑한 Promise 객체를 반환한다.

```js
fetch("https://jsonplaceholder.typicode.com/todos/1").then((response) =>
  console.log(response)
);
```

- fetch 함수는 HTTP 응답을 나타내는 Response 객체를 래핑한 프로미슬ㄹ 반환하므로 후속 처리 메서드 then을 통해 프로미스가 ressolve한 Response 객체를 전달받을 수 있다.

```js
fetch("https://jsonplaceholder.typicode.com/todos/1")
  // response는 HTTP 응답을 나타내는 Response 객체다.
  // json 메서드를 사용하여 Response 객체에서 HTTP 응답 몸체를 취득하여 역직렬화한다.
  .then((response) => response.json())
  // json은 역직렬화 HTTP 응답 몸체다.
  .then((json) => console.log(json));
```

- fetch 함수를 통해 HTTP 요청을 전송하면 다음과 같다.
- fetch 함수에 첫 번째 인수로 HTTP 요청을 전송할 URL과 두번째 인수로 HTTP 요청메섣, HTTTP 요청 헤더, 페이로드 등을 설정한 객체를 전달

```js
const request = {
  get(url) {
    return fetch(url);
  },
  post(url, payload) {
    return fetch(url, {
      method: "POST",
      headers: { "content-Type": "application/json" },
      body: JSON.stringify(payload),
    });
  },
  patch(url, payload) {
    return fetch(url, {
      method: "PATCH",
      headers: { "content-Type": "application/json" },
      body: JSON.stringify(payload),
    });
  },
  delete(url) {
    return fetch(url, { method: "DELETE" });
  },
};
```

# 제너레이터와 async/await

## 제너레이터란?

## 제너레이터 함수의 정의

## 제너레이터 객체

## 제너레이터의 일시 중지와 재개

## 제너레이터의 활용

1. 이터러블의 구현

2. 비동기 처리

## async/await

1. async 함수

2. await 키워드

3. 에러처리
