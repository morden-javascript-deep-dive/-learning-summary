# 이벤트

## 이벤트 드리븐 프로그래밍

- 브라우저는 처리해야 할 특정 사건이 발생하면 이를 감지하여 이벤트를 발생시킨다.
- 이때 이벤트가 발생했을 떄 호출될 함수를 이벤트 핸들러라 하고, 이벤트가 발생했을 때 브라우저에게 이벤트 핸들러의 호출을 위임하는 것은 이벤트 핸들러 등록이라 한다.
- 함수를 언제 호출할지 알 수 없으므로 개발자가 명시적으로 함수를 호출하는 것이 아니라 브라우저에게 함수 호출을 위임하는 것이다. 코드는 다음과 같다.

```html
<!DOCTYPE html>
<html lang="en">
  <body>
    <button>Click me!</button>
    <script>
      const $button = document.querySelector("button");

      // 사용자가 버튼을 클릭하면 함수를 호출하도록 요청
      $button.onclick = () => {
        alert("button click");
      };
    </script>
  </body>
</html>
```

- 이벤트와 그에 대응하는 함수(이벤트 핸들러)를 통해 사용자와 애플리케이션은 상호작용을 할 수 있다.
- 프로그램의 흐름을 이벤트 중심으로 제어하는 프로그래밍 방식을 이벤트 드리븐 프로그래밍이라 한다.

## 이벤트 타입

- 이벤트 타입은 이벤트의 종류를 나타내는 문자열이다. 이벤트 타입은 약 200여가지가 있다.
- 그 중 사용빈도가 높은 이벤트는 다음과 같다.

1. 마우스 이벤트

- click
- dbclick
- mousedown
- mouseup
- mousemove
- mouseenter(버블링 x)
- mouseover(버블링 o)
- mouseleave(버블링 x)
- mouseout(버블링 o)

2. 키보드 이벤트

- keydown: 모든 키를 눌렀을 때 발생
  - control, option, shift, tab, delete, 방향 키와 문자, 숫자, 특수 문자키
  - 단, 문자, 숫자, 특수 문자 키를 눌렀을 때 연속적으로 발생하나 그외엔 한 번만 발생
- keypress: 문자키를 눌렀을 때 연속적으로 발생(문자, 숫자, 특수 문자 키만 동작)
- keyup: 누르고 있던 키를 놓았을때 한번만 발생, keydown 이벤트와 마찬가지로 모든 키로 동작

3. 포커스 이벤트

- focus: HTML 요소가 포커스를 받았을 때(버블링 x)
- blur: HTML 요소가 포커스를 잃었을 때(버블링 x)
- focusin: HTML 요소가 포커스를 받았을 때(버블링 o)
- focusout: HTML 요소가 포커스를 잃었을 때(버블링 o)

4. 폼 이벤트

- submit: form 요소 내의 submit 버튼을 클릭했을 때
- reset: form 요소 내의 reset 버튼을 클릭했을때(최근에 사용하지 않음)

5. 값 변경 이벤트

- input: input(text, checkbox, radio), select, textarea 요소의 값이 입력되었을 때
- change: input(text, checkbox, radio), select, textarea 요소의 값이 변경되었을 때
  - change 이벤트는 input 이벤트와 달리 HTML 요소가 포커스를 잃었을 때, 즉, 사용자 입력이 종료되었을 때 이벤트 발생

6. DOM 뮤테이션 이벤트

- DOMContentLoaded: HTML 문서의 로드와 파싱이 완료되어 DOM 생성이 완료되었을 때

7. 뷰 이벤트

- resize: 브라우저 윈도우(window) 크기를 리사이즈할 때 연속적으로 발생한다. 오직 window 객체에서만 발생한다.
- scroll: 웹페이지(document) 또는 HTML 요소를 스크롤할 때 연속적으로 발생한다.

8. 리소스 이벤트

- load: DOMContentLoaded 이벤트가 발생한 이후, 모든 리소스(이미지, 폰트 등)의 로딩이 완료되었을 때(주로 window 객체에서 발생)
- unload: 리소스가 언로드 될 때(주로 새로운 웹페이지를 요청한 경우)
- abort: 리소스 로딩이 중단되었을 때
- error: 리소스 로딩이 실패했을 때

## 이벤트 핸들러 등록

- 이벤트 핸들러는 이벤트가 발생했을 떄 브라우저에 호출을 위임한 함수다.
  - 이벤트가 발생하면 브라우저에 의해 호출될 함수가 이벤트 핸들러다.
- 이벤트가 발생했을 때 브라우저에게 이벤트 핸들러의 호출을 위임하는 것을 이벤트 핸들러 등록이라한다.

1. 이벤트 핸들러 어트리뷰트 방식

- HTML 요소의 어트리뷰트 중에는 이벤트에 대응하는 이벤트 핸들러 어트리뷰트가 있다.
- 이벤트 핸들러 어트리뷰트 값으로 함수 호출문 등의 문(statement)을 할당하면 이벤트 핸들러가 등록된다.

```html
<!DOCTYPE html>
<html lang="en">
  <body>
    <button onclick="sayHi('Son')">Click me!</button>
    <script>
      function sayHi(name) {
        console.log(`Hi! ${name}`);
      }
    </script>
  </body>
</html>
```

- 주의할 점은 이벤트 핸들러 어트리뷰트 값으로 함수 참조가 아닌 함수 호출문 등의 문을 할당한다는 것이다.
- 이벤트 핸들러 어트리뷰트 값은 사실 암묵적으로 생성될 이벤트 핸들러의 함수 몸체를 의미한다.
  - 이벤트 핸들러 어트리뷰트 값으로 여러개의 문을 할당할 수 있다.

```html
<button onclick="console.log('Hi! '); console.log('Son');">Click me!</button>
```

- 가능하면 HTML과 자바스크립트는 관심사가 다르므로 혼재하는 것보다 분리하는 것이좋다.
- 하지만 모던 자바스크립트에서는 이벤트 핸들러 어트리뷰트 방식을 사용하는 경우가 있다.
- CBD(COmponent Based Development) 방식의 React/Vue.js 같은 프레임워크/라이브러리는 이벤트 핸들러 어트리뷰트 방식으로 이벤트를 처리한다.
- CBD는 HTML, CSS, js를 관심사가 다른 개별적인 요소가 아닌 뷰를 구성하기 위한 구성요소로 보기 때문에 관심사가 다르다고 생각하지 않는다.

```jsx
// React
<button onClick="{handleClick}">Save</button>
```

2. 이벤트 핸들러 프로퍼티 방식

- winddow 객체와 Documnet, HTMLElement 방식의 DOM 노드 객체는 이벤트에 대응하는 이벤트 핸들러 프로퍼티를 가지고 있다.
- 이벤트 핸들러 프로퍼티에 함수를 바인딩하면 이벤트 핸들러가 등록된다.

```html
<!DOCTYPE html>
<html lang="en">
  <body>
    <button>Click me!</button>
    <script>
      const $button = document.querySelector("button");

      // 이벤트 핸들러 프로퍼티에 이벤트 핸들러를 바인딩
      $button.onclick = function () {
        console.log("button click");
      };
    </script>
  </body>
</html>
```

- 이벤트 핸들러를 등록하기 위해서는 이벤트를 발생시킬 객체를 이벤트 타깃과 이벤트의 종류를 나타내는 문자열인 이벤트 타입 그리고 이벤트 핸들러를 지정할 필요가 있다.
- 이벤트 핸들러는 대부분 이벤트를 발생시킬 이벤트 타깃에 바인딩한다 하지만 반드시 이벤트 타깃에 이벤트 핸들러를 바인딩하지 않아도된다. 이벤트 타깃 또는 전파된 이벤트를 캐치할 DOM 노드 객체에 바인딩한다.
- 이벤트 핸들ㄹ러 프로퍼티 방식은 이벤트 핸들러 어트리뷰트 방식의 HTML과 자바스크립트가 뒤섞이는 문제를 해결할 수 있으나 하나의 이벤트 핸들러만 바인딩할 수 있다는 단점이 있다.

```html
<!DOCTYPE html>
<html lang="en">
  <body>
    <button>Click me!</button>
    <script>
      const $button = document.querySelector("button");

      // 이벤트 핸들러 프로퍼티에 이벤트 핸들러를 바인딩은 하나만 바인딩된다.
      // 두 번째 바인딩된 이벤트 핸들러에 의해 재할당되어 실행되지 않는다.
      $button.onclick = function () {
        console.log("button clicked 1");
      };

      // 두 번째 바인딩된 이벤트 핸들러
      $button.onclick = function () {
        console.log("button clicked 2");
      };
    </script>
  </body>
</html>
```

3. addEventListener 메서드 방식

- EventTarget.prototype.addEventListener 메서드를 사용하여 이벤트 핸들러를 등록할 수 있다.
- addEventListener 메서드의 첫 번째 매개변수에는 이벤트의 종류를 나타내는 문자열인 이벤트 타입을 전달한다.
- 두번쨰 매개변수에는 이벤트 핸들러를 전달한다. 마지막 배개변수에는 이벤트 전파단계(캡처링, 버블링)를 지정할 수 있다.

```html
<!DOCTYPE html>
<html lang="en">
  <body>
    <button>Click me!</button>
    <script>
      const $button = document.querySelector("button");

      // addEventListener 메서드 방식
      $button.addEventListener("click", function () {
        console.log("button click");
      });
    </script>
  </body>
</html>
```

- addEventListener 메서드에는 이벤트 핸들러를 바인딩하지 않고 인수로 전달한다.

```html
<!DOCTYPE html>
<html lang="en">
  <body>
    <button>Click me!</button>
    <script>
      const $button = document.querySelector("button");

      // 이벤트 핸들러 프로퍼티 방식
      $button.onclick = function () {
        console.log("[이벤트 핸들러 프로퍼티 방식]button click");
      };

      // addEventListener 메서드 방식
      $button.addEventListener("click", function () {
        console.log("[addEventListener 방식]button click");
      });
    </script>
  </body>
</html>
```

- addEventListener 메서드 방식은 이벤트 핸들러 프로퍼티에 바인딩된 이벤트 핸들러에 아무런 영향을 주지 않는다.
  - 버튼 요소에서 클릭 이벤트가 발생하면 2개의 이벤트 핸들러가 모두 호출된다.
- addEventListener 메서드는 하나 이상의 이벤ㅌ트 핸들러르 등록 할 수 있으며, 등록된 순서대로 호출된다.

```html
<!DOCTYPE html>
<html lang="en">
  <body>
    <button>Click me!</button>
    <script>
      const $button = document.querySelector("button");

      // 이벤트 둘 모두 동작한다.
      $button.addEventListener("click", function () {
        console.log("[1]button click");
      });

      $button.addEventListener("click", function () {
        console.log("[2]button click");
      });
    </script>
  </body>
</html>
```

- 단, addEventListener 메서드를 통해 참조가 동일한 이벤트 핸들러를 중복 등록하면 하나의 이벤트 핸들러만 등록된다.

```html
<!DOCTYPE html>
<html lang="en">
  <body>
    <button>Click me!</button>
    <script>
      const $button = document.querySelector("button");

      const handleClick = () => console.log("button click");

      // 참조가 동일한 이벤트 핸들러를 중복 등록하면 하나의 핸들러만 등록된다.
      $button.addEventListener("click", handleClick);
      $button.addEventListener("click", handleClick);
    </script>
  </body>
</html>
```

## 이벤트 핸들러 제거

- addEventListener 메서드로 등록한 이벤트 핸들러를 제거하려면 EventTarget.prototype.removeEventListener 메서드를 사용한다.
- removeEventListener 메서드에 전달한 인수는 addEventListener 메서드와 동일하나 addEventListener 인수와 일치하지 않으면 이벤트 핸들러가 제거되지 않는다.

```html
<!DOCTYPE html>
<html lang="en">
  <body>
    <button>Click me!</button>
    <script>
      const $button = document.querySelector("button");

      const handleClick = () => console.log("button click");

      // 이벤트 핸들러 등록
      $button.addEventListener("click", handleClick);

      // 이벤트 핸들러 제거
      $button.removeEventListener("click", handleClick, true); // 실패
      $button.removeEventListener("click", handleClick); // 성공
    </script>
  </body>
</html>
```

- 무명 함수를 이벤트 핸들러로 등록한 경우 제거할 수 없다.
- 이벤트 핸들러를 제거하려면 이벤트 핸들러의 참조를 변수나 자료구조에 저장하고 있어야한다.

```js
// 삭제 불가
$button.addEventListener("click", () => cnosole.log("button click"));
```

- 단, 기명 이벤트 핸들러 내부에서 removeEventListener 메서드를 호출하여 이벤트 핸들러를 제거하는 것은 가능하며 이때 이벤트 핸들러는 단 한 번만 호출된다.

```js
$button.addEventListner("click", function foo() {
  console.log("button click");
  // 이벤트 핸들러를 제거하며 단 한 번만 호출
  $button.removeEventListener("click", foo);
});
```

- 이벤트 핸들러 프로퍼티 방식으로 등록한 이벤트 핸들러는 removeEventListener 메서드로 제거할 수 없다.
- 이베트 핸들러 프로퍼티에 null을 할당하여 제거해야 한다.

```html
<!DOCTYPE html>
<html lang="en">
  <body>
    <button>Click me!</button>
    <script>
      const $button = document.querySelector("button");

      const handleClick = () => console.log("button click");

      // 이벤트 핸들러 프로퍼티 방식으로 등록
      $button.onclick = handleClick;

      // 이벤트 핸들러 제거 불가
      $button.removeEventListener("click", handleClick);

      // null을 할당하여 핸들러 제거
      $button.onclick = null;
    </script>
  </body>
</html>
```

## 이벤트 객체

- 이벤트가 발생하면 이벤트에 관련한 다양한 정보를 담고 있는 이벤트 객체가 동적으로 생성된다.
- 생성된 이벤트 객체는 이벤트 핸들러의 첫 번째 인수로 전달된다.

```html
<!DOCTYPE html>
<html lang="kr">
  <head>
    <meta charset="UTF-8" />
    <title>Practice</title>
  </head>
  <body>
    <p>클릭하세요. 클릭한 곳의 좌표가 표시됩니다.</p>
    <em class="message"></em>
    <script>
      const $msg = document.querySelector(".message");

      // 클릭 이벤트에 의해 생성된 이벤트 핸들러의 첫 번째 인수로 전달된다.
      function showCoords(e) {
        $msg.textContent = `clientX: ${e.clientX}, clientY: ${e.clientY}`;
      }

      document.onclick = showCoords;
    </script>
  </body>
</html>
```

- 클릭 이벤트에 의해 생성된 이벤트 객체는 이벤트 핸들러의 첫 번째 인수로 전달되어 매개변수 e에 암묵적으로 할당된다.
- 이는 브라우저가 이벤트 핸들러를 호출할 때 이벤트 객체를 인수로 전달하기 때문이다.
- 이벤트 객체를 전달받으려면 이벤트 핸들러를 정의할떄 이벤트 객체를 전달받을 때 매개변수를 명시적으로 선언해야한다.

```html
<!DOCTYPE html>
<html lang="kr">
  <head>
    <meta charset="UTF-8" />
    <title>Practice</title>
    <style>
      html,
      body {
        height: 100%;
      }
    </style>
  </head>
  <!--이벤트 핸들러 어트리뷰트 방식의 경우 event가 아닌 다른 이름으로는 이벤트 객체를 전달받지 못한다.-->
  <body onclick="showCoords(event)">
    <p>클릭하세요. 클릭한 곳의 좌표가 표시됩니다.</p>
    <em class="message"></em>
    <script>
      const $msg = document.querySelector(".message");

      // 클릭 이벤트에 의해 생성된 이벤트 핸들러의 첫 번째 인수로 전달된다.
      function showCoords(e) {
        $msg.textContent = `clientX: ${e.clientX}, clientY: ${e.clientY}`;
      }
    </script>
  </body>
</html>
```

- 이벤트 핸들러 어트리뷰트 방식의 경우 이벤트 객체를 전달받으려면 이벤트 핸들러의 첫 번째 전달인자명이 반드시 event이어야한다. 만약 다른 이름일 경우 이벤트를 전달받지 못한다.

1. 이벤트 객체의 상속 구조

- 다음 코드와 같이 생성자 함수를 호출하여 이벤트 객체를 생성할 수 있다.

```html
<!DOCTYPE html>
<html lang="kr">
  <head>
    <meta charset="UTF-8" />
    <title>Practice</title>
  </head>
  <body>
    <script>
      // Event 생성자 함수를 호출하여 foo 이벤트 타입의 Event 객체를 생성한다.
      let e = new Event("foo");
      console.log(e);
      // Event {...}
      console.log(e.type); // "foo"
      console.log(e instanceof Event); // true
      console.log(e instanceof Object); // true

      // FocusEvent 생성자 함수를 호출하여 focus 이벤트 타입의 FocusEvent 객체를 생성한다.
      e = new FocusEvent("focus");
      console.log(e);
      // FocusEvent {...}

      // MouseEvent 생성자 함수를 호출하여 click 이벤트 타입의 MouseEvent 객체를 생성한다.
      e = new MouseEvent("click");
      console.log(e);
      // MouseEvent {...}

      // KeyboardEvent 생성자 함수를 호출하여 keyup 이벤트 타입의 KeyboardEvent 객체를 생성
      e = new KeyboardEvent("keyup");
      console.log(e);
      // KeyBoardEvent {...}

      // InputEvent 생성자 함수를 호출하여 change 이벤트 타입의 InputEvent 객체를 생성
      e = new InputEvent("change");
      console.log(e);
      // InputEvent {...}
    </script>
  </body>
</html>
```

- 이벤트 객체 중 일부는 사용자의 행위에 의해 생성된 것이고 일부는 자바스크립트 코드에 의해 인위적으로 생성된 것이다.
- Event 인터페이스는 DOM 내에서 발생한 이벤트에 의해 생성되는 객체를 나타낸다.
- Event 인터페이스에는 모든 이벤트 객체의 공통 프로퍼티가 정의되어 있고 FocusEvent, MouseEvent, KeyboardEvent, WheelEvent 같은 하위 인터페이스에는 이벤트 타입에 따라 고유한 프로퍼티가 정의되어있다.

2. 이벤트 객체의 공통 프로퍼티

- Event 인터페이스, 즉 Event.prototype에 정의되어 있는 이벤트 관련 프로퍼티는 UIEvent, CustomEvent, MouseEvent 등 모든 파생 이벤트 객체에 상속된다.

  - type: 이벤트 타입(string)
  - target: 이벤트를 발생시킨 DOM 요소(DOM 요소 노드)
  - currentTarget: 이벤트 핸들러가 바인딩 된 DOM 요소(DOM 요소 노드)
  - eventPhase: 이벤트 전파 단계
    - 0: 이벤트 없음
    - 1: 캡처링 단계
    - 2: 타깃 단계
    - 3: 버블링 단계
  - bubble: 이벤트를 버블링으로 전파하는지 여부.
    - 다음 이벤트는 bubbles: false로 버블링하지 않는다.
    - 포커스 이벤트 focus/blur
    - 리소스 이벤트 load/unload/abort/error
    - 마우스 이벤트 mouseenter/mouseleave
  - cancelabel: preventDefault 메서드를 호출하여 이벤트의 기본동작을 취소할 수 있는지 여부. 다음 이벤트는 cancelable: fasle로 취소할 수 없다.
    - 포커스 이벤트 focus/blur
    - 리소스 이벤트 load/unload/abort/error
    - 마우스 이벤트 mouseenter/mouseleave
  - defaultPrevented: preventDefault 메서드를 호출하여 이벤트를 취소했는지 여부
  - isTrusted: 이벤트가 사용자 액션에 의해 발생한 이벤트인지 구분할 수 있다. 만약 사용자 액션에 의해 발생한 이벤트라면 isTrusted 는 true 가 된다. 스크립트에서 생성/수정했거나 dispatchEvent() 로 발송한 이벤트(커스텀 이벤트)는 false 가 된다.
  - timeStamp: 이벤트가 발생한 시각(1970/01/01/00:00:0부터 경과한 밀리초)

- 다음 코드는 체크박스 요소의 체크 상태가 변경되면 현재 체크 상태를 출력하는 코드다.

```html
<!DOCTYPE html>
<html lang="kr">
  <head>
    <meta charset="UTF-8" />
    <title>Practice</title>
  </head>
  <body>
    <input type="checkbox" />
    <em class="message">off</em>
    <script>
      const $checkbox = document.querySelector("input[type=checkbox]");
      const $msg = document.querySelector(".message");

      // change 이벤트가 발생하면 Event 타입의 이벤트 객체가 생성된다.
      $checkbox.onchange = (e) => {
        console.log(Object.getPrototypeOf(e) === Event.prototype); // true

        // e.target.checked는 체크박스 요소의 현재 체크 상태를 나타낸다.
        $msg.textContent = e.target.checked ? "on" : "off";
      };
    </script>
  </body>
</html>
```

- 이벤트 객체의 currentTarget 프로퍼티는 이벤트 핸들러가 바인딩된 DOM 요소를 가리킨다. - 이벤트 객체의 target 프로퍼티와 currentTarget 프로퍼티는 동일한 객체 $checkbox를 가리킨다.

```js
$checkbox.onchange = (e) => {
  // e.currentTarget은 이벤트 핸들러가 바인딩된 DOM 요소 $checkbox를 가리킨다.
  console.log(e.target === e.currentTarget); // true

  $msg.textContent = e.target.checked ? "on" : "off";
};
```

3. 마우스 정보 취득

- click, dbclick, mousedown, mouseup, mousemove, mouseenter, mouseleave 이벤트가 발생하면 생성되는 MouseEvent 타입의 이벤트 객체는 다음과 같은 고유의 프로퍼티를 갖는다.
- 마우스 포인터의 좌표 정보를 나타내는 프로퍼티
  - screenX/screenY
  - clientX/clientY
  - pageX/pageY
  - offsetX/offsetY
- 버튼 정보를 나타내는 프로퍼티
  - altKey, ctrlKey, shiftKey, button

```html
<!DOCTYPE html>
<html lang="kr">
  <head>
    <meta charset="UTF-8" />
    <title>Practice</title>
    <style>
      .box {
        width: 100px;
        height: 100px;
        background-color: #fff700;
        border: 5px solid orange;
        cursor: pointer;
      }
    </style>
  </head>
  <body>
    <div class="box"></div>
    <script>
      // 드래그 대상 요소
      const $box = document.querySelector(".box");

      // 드래그 시작 시점의 마우스 포인터 위치
      const initialMousePos = { x: 0, y: 0 };
      // 오프셋: 이동할 거리
      const offset = { x: 0, y: 0 };

      // mousemove 이벤트 핸들러
      const move = (e) => {
        // 오프셋 = 현재 마우스 포인터 좌표 - 드래그 시작 시점의 마우스 포인터 좌표
        offset.x = e.clientX - initialMousePos.x;
        offset.y = e.clientY - initialMousePos.y;

        // translage3d는 GPU를 사용하므로 absolute의 top, left를 사용하는 것보다 빠르다.
        // top, left는 레이아웃에 영향을 준다.
        $box.style.transform = `translate3d(${offset.x}px, ${offset.y}px, 0)`;
      };

      // mousedown 이벤트 핸들러가 발생하면 드래그 시작 시점의 마우스 포인터 좌표를 저장
      $box.addEventListener("mousedown", (e) => {
        // 이동 거리를 계산하기 위해 mousedown 이벤트가 발생
        // 마우스 포인터 좌표(clientX/clientY)를 저장
        // 한 번 이상 드래그로 이동한 경우 move에서 translate3d로
        // 이동한 상태이므로 offset.x/offset.y를 빼야한다.
        initialMousePos.x = e.clientX - offset.x;
        initialMousePos.y = e.clientY - offset.y;

        // mousedown 이벤트가 발생한 상태에서 mousemove 이벤트가 발생하면 box 요소를 이동시킨다.
        document.addEventListener("mousemove", move);
      });

      // mouseup 이벤트가 발생하면 mousemove 이벤트를 제거해 이동을 멈춘다.
      document.addEventListener("mouseup", () => {
        document.removeEventListener("mousemove", move);
      });
    </script>
  </body>
</html>
```

4. 키보드 정보 취득

- keydown, keyup, keypress 이벤트가 발생하면 생성되는 KeyboardEvent 타입의 이벤트 객체는
- altKey, ctrlKey, metaKey, key, keyCode(폐지되어 key를 사용할 것) 같은 고유의 프로퍼티를 갖는다.
- 다음 코드는 input 요소의 입력 필드에 엔터 키가 입력되면 현재까지 입력 필드에 입력된 값을 출력하는 예제다.

```html
<!DOCTYPE html>
<html lang="kr">
  <head>
    <meta charset="UTF-8" />
    <title>Practice</title>
  </head>
  <body>
    <input type="text" />
    <em class="message"></em>
    <script>
      const $input = document.querySelector("input[type=text]");
      const $msg = document.querySelector(".message");

      $input.onkeyup = (e) => {
        // e.key는 입력한 키 값을 문자열로 반환
        // 입력한 키가 'Enter', 엔터 키가 아니면 무시한다.
        if (e.key !== "Enter") return;

        // 엔터키가 입력되면 현재까지 입력 필드에 입력된 값을 출력한다.
        $msg.textContent = e.target.value;
        e.target.value = "";
      };
    </script>
  </body>
</html>
```

- input 요소의 입력 필드에 한글을 입력하고 엔터 키를 누르면 keyup 이벤트 핸들러가 두 번 호출되는 현상이 발생한다.
  - keyup 이벤트 대신 keydown 이벤트를 캐치한다.

## 이벤트 전파

- DOM 트리 상에 존재하는 DOM 요소 노드에서 발생한 이벤트는 DOM 트리를 통해 전파된다.
  - 이벤트 전파(event propagation)라고 한다.

```html
<!DOCTYPE html>
<html lang="kr">
  <body>
    <ul id="fruits">
      <li id="apple">Apple</li>
      <li id="banana">Banana</li>
      <li id="orange">Orange</li>
    </ul>
  </body>
</html>
```

- ul 요소의 두 번째 자식 요소인 li 요소를 클릭하면 클릭 이벤트가 발생한다.
  - 생성된 이벤트 객체는 이벤트를 발생시킨 DOM 요소인 이벤트 타깃을 중심으로 DOM 트리를 통해 전파된다.
  - 캡처링 단계: 이벤트가 상위 요소에서 하위 요소 방향으로 전파
  - 타깃 단계: 이벤트가 이벤트 타깃을 도달
  - 버블링 단계: 이벤트가 하위 요소에서 상위 요소 방향으로 전파

```html
<!DOCTYPE html>
<html lang="kr">
  <head>
    <meta charset="UTF-8" />
    <title>Practice</title>
  </head>
  <body>
    <ul id="fruits">
      <li id="apple">Apple</li>
      <li id="banana">Banana</li>
      <li id="orange">Orange</li>
    </ul>
    <script>
      const $fruits = document.getElementById("fruits");

      // #fruits 요소의 하위 요소인 li 요소를 클릭한 경우
      $fruits.addEventListener("click", (e) => {
        console.log(`이벤트 단계: ${e.eventPhase}`); // 3: 버블링 단계
        console.log(`이벤트 타깃: ${e.target}`); // [object HTMLElement]
        console.log(`커런트 타깃: ${e.currentTarget}`); // [object HTMLUListElement]
      });
    </script>
  </body>
</html>
```

- li 요소를 클릭하면 이벤트가 발생하여 클릭 이벤트 객체가 생성되고 클릭된 li 요소가 이벤트 타깃이 된다.
- 이때 클릭 이벤트 객체는 window에서 시작해서 이벤트 타깃 방향으로 전파된다.(캡쳐링 단계)
- 이후 이벤트 객체는 이벤트를 발생시킨 이벤트 타깃에 도달한다.(타깃 단계)
- 이후 이벤트 객체는 이벤트 타깃에서 시작해서 window 방향으로 전파된다.(버블링 단계)
- 이베트 핸들러 어트리뷰트/프로퍼티 방식으로 등록한 이벤트 핸들러는 타깃 단계와 버블링 단계의 이벤트만 캐치할 수 있다.
- addEventListener 메서드 방식으로 등록한 이벤트 핸들러는 타깃 단계, 버블링 단계, 캡쳐링 단계의 이벤트도 선별적으로 캐치할 수 있다. 3번째 인수로 true로 지정하면 캡쳐링 단계를 캐치할 수 있다.

```html
<!DOCTYPE html>
<html lang="kr">
  <head>
    <meta charset="UTF-8" />
    <title>Practice</title>
  </head>
  <body>
    <ul id="fruits">
      <li id="apple">Apple</li>
      <li id="banana">Banana</li>
      <li id="orange">Orange</li>
    </ul>
    <script>
      const $fruits = document.getElementById("fruits");
      const $banana = document.getElementById("banana");

      // #fruits 요소의 하위 요소인 li 요소를 클릭한 경우 캡처링 단계의 이벤트를 캐치
      $fruits.addEventListener(
        "click",
        (e) => {
          console.log(`이벤트 단계: ${e.eventPhase}`); // 1: 캡처링 단계
          console.log(`이벤트 타깃: ${e.target}`); // [object HTMLElement]
          console.log(`커런트 타깃: ${e.currentTarget}`); // [object HTMLUListElement]
        },
        true
      );

      // 타깃 단계의 이벤트를 캐치
      $banana.addEventListener("click", (e) => {
        console.log(`이벤트 단계: ${e.eventPhase}`); // 2: 타깃 단계
        console.log(`이벤트 타깃: ${e.target}`); // [object HTMLElement]
        console.log(`커런트 타깃: ${e.currentTarget}`); // [object HTMLULIElement]
      });

      // 버블링 단계의 이벤트를 캐치
      $fruits.addEventListener("click", (e) => {
        console.log(`이벤트 단계: ${e.eventPhase}`); // 3: 버블링 단계
        console.log(`이벤트 타깃: ${e.target}`); // [object HTMLElement]
        console.log(`커런트 타깃: ${e.currentTarget}`); // [object HTMLUListElement]
      });
    </script>
  </body>
</html>
```

- 이처럼 이벤트는 이벤트를 발생시킨 이벤트 타깃은 물론 상위의 DOM 요소에서도 캐치할 수 있다.
- 대부분의 이벤트는 캡쳐링과 버블리을 통해 전파한다. 하지만 다음 이벤트는 버블리을 통해 전파되지 않는다.
  - 포커스 이벤트 focus/blur
  - 리소스 이벤트 load/unload/abort/error
  - 마우스 이벤트 mouseenter/mouseleave
- 위 이벤트를 캐치하려면 캡처링 단계의 이벤트를 캡쳐해야한다. 하지만 대체할 수 있는 이벤트가 존재하기 때문에 캡처링 단계에서 이벤트를 캐치해야 할 경우는 거의 없다.
- 다음 코드는 캠처링 단계의 이벤트와 버블링 단계의 이벤트를 캐치하는 이벤트 핸들러가 혼용되는 경우다.

```html
<!DOCTYPE html>
<html lang="kr">
  <head>
    <meta charset="UTF-8" />
    <title>Practice</title>
    <style>
      html,
      body {
        height: 100%;
      }
    </style>
  </head>
  <body>
    <p>버블링과 캡처링 이벤트 <button>버튼</button></p>
    <script>
      // 버블링 단계의 이벤트를 캐치
      document.body.addEventListener("click", () => {
        console.log("Handler for body.");
      });

      // 캡처링 단계의 이밴트를 캐치
      document.querySelector("p").addEventListener(
        "click",
        () => {
          console.log("Handler for paragraph.");
        },
        true
      );

      // 버블링 단계의 이벤트를 캐치
      document.querySelector("button").addEventListener("click", () => {
        console.log("Handler for button.");
      });
    </script>
  </body>
</html>
```

- body, button 요소는 버블링 단계의 이벤트를 캐치하고, p 요소는 캡처링 단계의 이벤트만 캐치한다.
- button 요소에서 클릭 이벤트가 발생하면 먼저 캡처링 단계를 캐치하는 p 요소의 이벤트 핸들러가 호출되고, 그후 버블링 단계의 이벤트를 캐치하는 button, body 요소의 이벤트 핸들러가 순차적으로 호출된다.

```
Handler for paragraph.
Handler for button.
Handler for body.
```

- 만약 p 요소에서 클릭 이벤트가 발생하면 캡처링 단계를 캐치하는 p 요소의 이벤트 핸들러가 호출되고 버블링 단계를 캐치하는 body 요소의 이벤트 핸들러가 순차적으로 호출된다.

```
Handler for paragraph.
Handler for body.
```

## 이벤트 위임

- 사용자가 내비게이션 아이템(li 요소)을 클릭하여 선택하면 현재 선택된 내비게이션 아이템에 active 클래스를 추가하고 그 외의 모든 내비게이션 아이템의 active 클래스는 제거하는 예제다.

```html
<!DOCTYPE html>
<html lang="kr">
  <head>
    <meta charset="UTF-8" />
    <title>Practice</title>
    <style>
      #fruits {
        display: flex;
        list-style-type: none;
        padding: 0;
      }

      #fruits li {
        width: 100px;
        cursor: pointer;
      }

      #fruits .active {
        color: red;
        text-decoration: underline;
      }
    </style>
  </head>
  <body>
    <nav>
      <ul id="fruits">
        <li id="apple" class="active">Apple</li>
        <li id="banana">Banana</li>
        <li id="orange">Orange</li>
      </ul>
    </nav>
    <div>선택된 내비게이션 아이탬: <em class="msg">apple</em></div>
    <script>
      const $fruits = document.getElementById("fruits");
      const $msg = document.querySelector(".msg");

      // 사용자 클릭에 의해 선택된 내비게이션 아이탬에 active 클래스를 추가
      // 그 외에 모든 내비게이션 아이템의 active 클래스를 제거
      function activate({ target }) {
        [...$fruits.children].forEach(($fruit) => {
          $fruit.classList.toggle("active", $fruit === target);
          $msg.textContent = target.id;
        });
      }

      // 모든 내비게이션 아이템에 이벤트 핸들러를 등록
      document.getElementById("apple").onclick = activate;
      document.getElementById("banana").onclick = activate;
      document.getElementById("orange").onclick = activate;
    </script>
  </body>
</html>
```

- 코드는 모든 내비게이션 아이템에 이벤트 핸들러인 activate를 등록했다.
- 만일 내비게이션 아이템이 100개라면 100개의 이벤트 핸들러를 등록해야 한다.
- 이 경우 많은 DOM 요소에 이벤트 핸들러를 등록하므로 성능 저하의 원인이 될뿐더러 유지보수에도 부적합한 코드를 생산하게 된다.
- 이벤트 위임(event delegation)은 여러 개의 하위 DOM 요소에 각각 이벤트 핸들러를 등록하는 대신 하나의 상위 DOM 요소에 이벤트 핸들러를 등록하는 방법을 말한다.
- 이벤트 위임을 통해 상위 DOM 요소에 이벤트 핸들러를 등록하면 여러 개의 하위 DOM 요소에 이벤트 핸들러를 등록할 필요가 없다.
- 동적으로 하위 DOM 요소를 추가하더라도 일일이 추가된 DOM 요소에 이벤트 핸들러를 등록할 필요가 없다.

```html
<!DOCTYPE html>
<html lang="kr">
  <head>
    <meta charset="UTF-8" />
    <title>Practice</title>
    <style>
      #fruits {
        display: flex;
        list-style-type: none;
        padding: 0;
      }

      #fruits li {
        width: 100px;
        cursor: pointer;
      }

      #fruits .active {
        color: red;
        text-decoration: underline;
      }
    </style>
  </head>
  <body>
    <nav>
      <ul id="fruits">
        <li id="apple" class="active">Apple</li>
        <li id="banana">Banana</li>
        <li id="orange">Orange</li>
      </ul>
    </nav>
    <div>선택된 내비게이션 아이탬: <em class="msg">apple</em></div>
    <script>
      const $fruits = document.getElementById("fruits");
      const $msg = document.querySelector(".msg");

      // 사용자 클릭에 의해 선택된 내비게이션 아이탬에 active 클래스를 추가
      // 그 외에 모든 내비게이션 아이템의 active 클래스를 제거
      function activate({ target }) {
        // 이벤트를 발생시킨 요소(target)가 ul#fruits의 자식 요소가 아니라면 무시
        if (!target.matches("#fruits > li")) return;

        [...$fruits.children].forEach(($fruit) => {
          $fruit.classList.toggle("active", $fruit === target);
          $msg.textContent = target.id;
        });
      }

      // 이벤트 위임: 상위 요소(ul#fruits)는 하위 요소의 이벤트를 캐치할 수 있다.
      $fruits.onclick = activate;
    </script>
  </body>
</html>
```

- 이벤트 위임을 통해 하위 DOM 요소에서 발생한 이벤트를 처리할 때 주의할 점은 사위 요소에 이벤트 핸들러를 등록하기 때문에 이벤트 타깃, 즉 이벤트를 실제로 발생 시킨 DOM 요소가 개발자가 기대한 DOM 요소가 아닐 수도 있다는 것이다.
- Element.prototype.matches 메서드는 인수로 전달된 선택자에 의해 특정 노드를 탐색 가능한지 확인한다.

```js
function activate({target}) {
    // 이벤트를 발생시킨 요소(target)가 ul#fruits의 자식 요소가 아니라면 무시
    if (!target.matches('#fruits > li')) return;
....
}
```

## DOM 요소의 기본 동작의 조작

1. DOM 요소의 기본 동작 중단

- 이벤트 객체의 preventDefault 메서드는 DOM 요소의 기본 동작을 중단시킨다.

```html
<!DOCTYPE html>
<html lang="kr">
  <head>
    <meta charset="UTF-8" />
    <title>Practice</title>
  </head>
  <body>
    <a href="https://www.google.com">go</a>
    <input type="checkbox" />
    <script>
      document.querySelector("a").onclick = (e) => {
        // a 요소의 기본 동작을 중단한다.
        e.preventDefault();
      };

      document.querySelector("input[type=checkbox]").onclick = (e) => {
        // checkbox 기본 동작을 중단한다.
        e.preventDefault();
      };
    </script>
  </body>
</html>
```

2. 이벤트 전파 방지

- 이벤트 객체의 stopPropagation 메서드는 이벤트 전파를 중지시킨다.

```html
<!DOCTYPE html>
<html lang="kr">
  <head>
    <meta charset="UTF-8" />
    <title>Practice</title>
  </head>
  <body>
    <div class="container">
      <button class="btn1">Button 1</button>
      <button class="btn2">Button 2</button>
      <button class="btn3">Button 3</button>
    </div>
    <script>
      // 이벤트 위임. 클릭된 하위 버튼 요소의 color를 변경한다.
      document.querySelector(".container").onclick = ({ target }) => {
        if (!target.matches(".container > button")) return;

        target.style.color = "red";
      };

      // .btn2 요소는 이벤트를 전파하지 않으므로 상위 요소에서 이벤트를 캐치할 수 없다.
      document.querySelector(".btn2").onclick = (e) => {
        e.stopPropagation(); // 이벤트 전파 중단
        e.target.style.color = "blue";
      };
    </script>
  </body>
</html>
```

- stopPrapagation 메서드는 하위 DOM 요소의 이벤트를 개별적으로 처리하기 위해 이벤트의 전파를 중단시킨다.

## 이벤트 핸들러 내부의 this

1. 이벤트 핸들러 어트리뷰트 방식

```html
<!DOCTYPE html>
<html lang="kr">
  <head>
    <meta charset="UTF-8" />
    <title>Practice</title>
  </head>
  <body>
    <button onclick="handleClick()">Click me</button>
    <script>
      function handleClick() {
        console.log(this); // window
      }
    </script>
  </body>
</html>
```

- this는 전역 객체를 가리킨다.
- 단, 이벤트 핸들러를 호출할 때 인수로 전달한 this는 이벤트를 바인딩한 DOM 요소를 가리킨다.

```html
<!DOCTYPE html>
<html lang="kr">
  <head>
    <meta charset="UTF-8" />
    <title>Practice</title>
  </head>
  <body>
    <button onclick="handleClick(this)">Click me</button>
    <script>
      function handleClick(button) {
        console.log(button); // 이벤트를 바인딩한 button 요소
        console.log(this);
      }
    </script>
  </body>
</html>
```

- 이벤트 핸들러 어트리뷰트 방식에 의해 암묵적으로 생성된 이벤트 핸들러 내부의 this는 이벤트를 바인딩한 DOM 요소를 가리킨다.
  - 이벤트 핸들러 프로퍼티 방식과 동일

2. 이벤트 핸들러 프로퍼티 방식과 addEventListener 메서드 방식

- 이벤트 핸들러 프로퍼티 방식과 addEventListener 메서드 방식 모두 이벤트 핸들러 내부의 this는 이벤트를 바인딩한 DOM 요소를 가리킨다.
  - 이벤트 핸들러 내부의 this는 이벤트 객체의 currentTarget 프로퍼티와 같다.

```html
<!DOCTYPE html>
<html lang="kr">
  <head>
    <meta charset="UTF-8" />
    <title>Practice</title>
  </head>
  <body>
    <button class="btn1">0</button>
    <button class="btn2">0</button>
    <script>
      const $button1 = document.querySelector(".btn1");
      const $button2 = document.querySelector(".btn2");

      // 이벤트 핸들러 프로퍼티 방식
      $button1.onclick = function (e) {
        // this는 이벤트를 바인딩한 DOM 요소를 가리킨다.
        console.log(this); // $button1
        console.log(e.currentTarget); // $button1
        console.log(this === e.currentTarget); // true

        // $button1의 textContent를 1 증가.
        ++this.textContent;
      };

      // addEventListener 메서드 방식
      $button2.addEventListener("click", function (e) {
        // this는 이벤트를 바인딩한 DOM 요소를 가리킨다.
        console.log(this); // $button2
        console.log(e.currentTarget); // $button2
        console.log(this === e.currentTarget); // true

        // $button2의 textContent를 1 증가.
        ++this.textContent;
      });
    </script>
  </body>
</html>
```

- 화살표 함수는 함수 자체 this 바인딩을 갖지 않아 상위 스코프의 this를 가리킨다.

```html
<!DOCTYPE html>
<html lang="kr">
  <head>
    <meta charset="UTF-8" />
    <title>Practice</title>
  </head>
  <body>
    <button class="btn1">0</button>
    <button class="btn2">0</button>
    <script>
      const $button1 = document.querySelector(".btn1");
      const $button2 = document.querySelector(".btn2");

      // 이벤트 핸들러 프로퍼티 방식
      $button1.onclick = (e) => {
        // this는 이벤트를 바인딩한 DOM 요소를 가리킨다.
        console.log(this); // window
        console.log(e.currentTarget); // $button1
        console.log(this === e.currentTarget); // false

        // this는 window를 가리키므로 window.textContent에 NaN을 할당한다.
        ++this.textContent;
      };

      // addEventListener 메서드 방식
      $button2.addEventListener("click", (e) => {
        // this는 이벤트를 바인딩한 DOM 요소를 가리킨다.
        console.log(this); // window
        console.log(e.currentTarget); // $button2
        console.log(this === e.currentTarget); // false

        // this는 window를 가리키므로 window.textContent에 NaN을 할당한다.
        ++this.textContent;
      });
    </script>
  </body>
</html>
```

- 클래스에서 이벤트 핸들러를 바인딩하는 경우 this에 주의해야한다.

```html
<!DOCTYPE html>
<html lang="kr">
  <head>
    <meta charset="UTF-8" />
    <title>Practice</title>
  </head>
  <body>
    <button class="btn">0</button>
    <script>
      class App {
        constructor() {
          this.$button = document.querySelector(".btn");
          this.count = 0;

          // increase 메서드를 이벤트 핸들러로 등록
          this.$button.onclick = this.increase;
        }

        increase() {
          // 이벤트 핸들러 increase 내부의 this는 DOM 요소(this.$button)를 가리킨다.
          // this.$button은 this.$button.$button과 같다.
          this.$button.textContent = ++this.count; // TypeError
        }
      }
      new App();
    </script>
  </body>
</html>
```

- increase 메서드를 이벤트 핸들러로 바인딩할 때 bind 메서드를 사용해 this를 전달하여 increase 메서드 내부의 this가 클래스가 생성할 인스턴스를 가리키도록 해야 한다.

```html
<!DOCTYPE html>
<html lang="kr">
  <head>
    <meta charset="UTF-8" />
    <title>Practice</title>
  </head>
  <body>
    <button class="btn">0</button>
    <script>
      class App {
        constructor() {
          this.$button = document.querySelector(".btn");
          this.count = 0;

          // increase 메서드 내부의 this가 인스턴스를 가리키도록 한다.
          this.$button.onclick = this.increase.bind(this);
        }

        increase() {
          // 이벤트 핸들러 increase 내부의 this는 DOM 요소(this.$button)를 가리킨다.
          // this.$button은 this.$button.$button과 같다.
          this.$button.textContent = ++this.count; // typeError
        }
      }

      new App();
    </script>
  </body>
</html>
```

- 또한 클래스 필드에 할당한 화살표 함수를 이벤트 핸들러로 등록하여 이벤트 핸들러 내부의 this가 인스턴스를 가리키도록 할 수도 있다.
- 다만 이때 이벤트 핸들런 increase는 프로토타입 메서드가 아닌 인스턴스 메서드가 된다.

```html
<!DOCTYPE html>
<html lang="kr">
  <head>
    <meta charset="UTF-8" />
    <title>Practice</title>
  </head>
  <body>
    <button class="btn">0</button>
    <script>
      class App {
        constructor() {
          this.$button = document.querySelector(".btn");
          this.count = 0;

          // 화살표 함수인 increase를 이벤트 핸들러로 등록
          this.$button.onclick = this.increase;
        }

        // 클래스 필드 정의
        // increase는 인스턴스 메서드이며 내부의 this는 인스턴스를 가리킨다.
        increase = () => (this.$button.textContent = ++this.count);
      }

      new App();
    </script>
  </body>
</html>
```

## 이벤트 핸들러에 인수 전달

- 이벤트 핸들러 내부에서 함수를 호출하면서 전달하는 방법이 있다.

```html
<!DOCTYPE html>
<html lang="kr">
  <head>
    <meta charset="UTF-8" />
    <title>Practice</title>
  </head>
  <body>
    <label>User name <input type="text" /></label>
    <em class="message"></em>
    <script>
      const MIN_USER_NAME_LENGTH = 5; // 이름 최소 길이
      const $input = document.querySelector("input[type=text]");
      const $msg = document.querySelector(".message");

      const checkUserNameLength = (min) => {
        $msg.textContent =
          $input.value.length < min ? `이름은 ${min}자 이상 입력해 주세요` : "";
      };

      // 이벤트 핸들러 내부에서 함수를 호출하면서 인수를 전달한다.
      $input.onblur = () => {
        checkUserNameLength(MIN_USER_NAME_LENGTH);
      };
    </script>
  </body>
</html>
```

- 또는 이벤트 핸들러를 반환하는 함수를 호출하면서 인수를 전달할 수도 있다.

```html
<!DOCTYPE html>
<html lang="kr">
  <head>
    <meta charset="UTF-8" />
    <title>Practice</title>
  </head>
  <body>
    <label>User name <input type="text" /></label>
    <em class="message"></em>
    <script>
      const MIN_USER_NAME_LENGTH = 5; // 이름 최소 길이
      const $input = document.querySelector("input[type=text]");
      const $msg = document.querySelector(".message");

      const checkUserNameLength = (min) => (e) => {
        $msg.textContent =
          $input.value.length < min ? `이름은 ${min}자 이상 입력해 주세요` : "";
      };

      // 이벤트 핸들러를 반환하는 함수를 호출하면서 인수를 전달한다.
      $input.onblur = checkUserNameLength(MIN_USER_NAME_LENGTH);
    </script>
  </body>
</html>
```

## 커스텀 이벤트

1. 커스텀 이벤트 생성

- 이벤트가 발생하면 암묵적으로 생성되는 이벤트 객체는 발생한 이벤트의 종류에 따라 이벤트 타입이 결정된다.
- 하지만 Event, UIEvent, MouseEvent 같은 이벤트 생성자 함수를 호출하여 명시적으로 생성한 이벤트 객체는 임의의 이벤트 타입을 지정할 수 있다.
  - 개발자의 의도로 생성된 이벤트를 커스텀 이벤트라 한다.
- 일반적으로 CustomEvent 이벤트 생성자 함수를 사용한다.

```js
// KeyboardEvent 생성자 함수로 keyup 이벤트 타입의 커스텀 이벤트 객체를 생성
const keyboardEvent = new KeyboardEvent("keyup");
console.log(keyboardEvent.type); // keyup

// CustomEvent 생성자 함수로 foo 이벤트 타입의 커스텀 이벤트 객체를 생성
const customEvent = new CustomEvent("foo");
console.log(customEvent.type); // foo
```

- 생성된 커스텀 이벤트 객체는 버블링되지 않으며 preventDefault 메서드로 취소할 수도 없다.
  - 커스텀 이벤트 객체는 bubbles, cancelable 프로퍼티의 값이 false가 기본값이다.

```js
// MouseEvent 생성자 함수로 click 이벤트 타입의 커스텀 이벤트 객체를 생성
const customEvent = new MouseEvent("click");
console.log(customEvent.type); // click
console.log(customEvent.bubbles); // false
console.log(customEvent.cancelable); // false
```

- 커스텀 이벤트 객체의 bubbles or cancelable 프로퍼티를 true로 설정하려면 이벤트 생성자 함수의 두 번째 인수로 bubbles or cancelable 프로퍼티를 갖는 객체를 전달한다.

```js
// MouseEvent 생성자 함수로 click 이벤트 타입의 커스텀 이벤트 객체를 생성
const customEvent = new MouseEvent("click", {
  bubbles: true,
  cancelable: true,
});

console.log(customEvent.bubbles); // true
console.log(customEvent.cancelable); // true
```

- 커스텀 이벤트 객체에는 bubbles or cancelable 프로퍼티뿐만 아니라 이벤트 타입에 따라 가지는 이벤트 고유의 프로퍼티 값을 지정할 수 있다.

```js
// MouseEvent 생성자 함수로 click 이벤트 타입의 커스텀 이벤트 객체를 생성
const mouseEvent = new MouseEvent("click", {
  bubbles: true,
  cancelable: true,
  clientX: 50,
  clientY: 100,
});

console.log(mouseEvent.clientX); // 50
console.log(mouseEvent.clientY); // 100

// KeyboardEvent 생성자 함수로 keyup 이벤트 타입의 커스텀 이벤트 객체를 생성
const keyboardEvent = new KeyboardEvent("keyup", { key: "Enter" });

console.log(keyboardEvent.key); // Enter
```

- 이벤트 생성자 함수로 생성한 커스텀 이벤트는 isTrusted 프로퍼티의 값이 언제나 false다.
- 커스텀 이벤트가 아닌 사용자의 행위에 의해 발생한 이벤트에 의해 생성된 이벤트 객체의 isTrusted 프로퍼티 값은 언제나 true다.

```js
// InputEvent 생성자 함수로 foo 이벤트 타입의 커스텀 이벤트 객체를 생성
const customEvent = new InputEvent("foo");
console.log(customEvent.isTrusted); // false
```

2. 커스텀 이벤트 디스패치

- 생성된 커스텀 이벤트는 dispatchEvent 메서드로 디스패치(이벤트를 발생시키는 행위)할 수 있다.

```html
<!DOCTYPE html>
<html lang="kr">
  <head>
    <meta charset="UTF-8" />
    <title>Practice</title>
  </head>
  <body>
    <button class="btn">Click me</button>
    <script>
      const $button = document.querySelector(".btn");

      $button.addEventListener("click", (e) => {
        console.log(e); // MouseEvent {...}
        alert(`${e} clicked!`);
      });

      // 커스텀 이벤트 생성
      const customEvent = new MouseEvent("click");

      // 커스텀 이벤트 디스패치(동기 처리). click 이벤트가 발생한다.
      $button.dispatchEvent(customEvent);
    </script>
  </body>
</html>
```

- 일반적으로 이벤트 핸들러는 비동기처리 방식으로 동작하지만 dispatchEvent 메서드는 이벤트 핸들러를 동기처리 방식으로 호출한다.
  - dispatchEvent 메서드로 이벤트를 디스패치하기 이전에 커스텀 이벤트를 처리할 이벤트 핸들러를 등록해야 한다.

```html
<!DOCTYPE html>
<html lang="kr">
  <head>
    <meta charset="UTF-8" />
    <title>Practice</title>
  </head>
  <body>
    <button class="btn">Click me</button>
    <script>
      const $button = document.querySelector(".btn");

      // 버튼 요소에 foo 커스텀 이벤트 핸들러를 등록
      // 커스텀 이벤트를 디스패치하기 이전에 이벤트 핸들러를 등록해야 한다.
      $button.addEventListener("foo", (e) => {
        // e.detail에는 CustomEvent 함수의 두 번째 인수로 전달한 정보가 담겨 있다.
        alert(e.detail.message);
      });

      // CustomEvent 생성자 함수로 foo 이벤트 타입의 커스텀 이벤트 객체를 생성
      const customEvent = new CustomEvent("foo", {
        detail: { message: "Hello" }, // 이벤트와 함께 전달하고 싶은 정보
      });

      // 커스텀 이벤트 디스패치
      $button.dispatchEvent(customEvent);
    </script>
  </body>
</html>
```

# 타이머

## 호출 스케줄링

- 함수를 명시적으로 호출하면 함수가 즉시 실행된다.
- 함수를 명시적으로 호출하지 않고 일정 시간이 경과된 이후에 호출되도록 함수 호출을 예약하려면 타이머 함수를 사용한다.
  - 호출 스케줄링이라한다.
- 자바스크립트 엔진은 단 하나의 실행 컨텍스트 스택을 갖기 때문에 두 가지 이상의 테스크를 동시에 실행할 수 없다. 즉, 자바스크립트 엔진은 싱글 스레드로 동작한다.
- 타이머 함수 setTimeout, setInterval은 비동기처리 방식으로 동작한다.

## 타이머 함수

1. setTimeout / clearTimeout

- setTimeout 함수는 두 번째 인수로 전달받은 시간으로 단 한 번 동작하는 타이머를 생성한다.

```js
const timeoutId = setTimeout((name) => console.log(`hi ${name}`), 1000, "park");
```

- setTimeout 함수가 반환한 타이머 id를 clearTimeout 함수의 인수로 전달하여 타이머를 취소할 수 있다.
- clearTimeout 함수는 호출 스케줄링을 취소한다.

```js
const timeoutId = setTimeout((name) => console.log(`hi ${name}`), 1000, "park");

clearTimeout(timeoutId);
```

2. setInterval / clearInterval

- setInterval 함수는 두 번째 인수로 전달받은 시간으로 반복 동작하는 타이머를 생성한다.
- 이후 타이머가 만료될 때마다 첫번째 인수로 전달받은 콜백함수가 반복 호출된다.
- setInterval 함수가 반환한 타이머 id를 clearInterval 함수의 인수로 전달하여 타이머를 취소할 수 있다.
- clearInterval 함수는 호출 스케줄링을 취소한다.

```js
let count = 1;

// 1초(1000ms) 후 타이머가 만료될 때마다 콜백 함수가 호출된다.
// setInterval 함수는 생성된 타이머를 식별할 수 있는 고유한 타이머 id를 반환한다.
const timeoutId = setInterval(() => {
  console.log(count); // 1 2 3 4 5

  if (count++ === 5) clearInterval(timeoutId);
}, 1000);
```

## 디바운스와 스로틀

- scroll, resize, input, mousemove, mouseover 같은 이벤트는 짧은 시간 간격으로 연속으로 발생한다.
- 이러한 이벤트에 바인딩한 이벤트 핸들러는 과도하게 호출되어 성능에 문제를 일으킬 수 있다.
- 디바운스와 스로틀은 짧으 ㄴ시간 간격으로 연속으로 발생하는 이벤트를 그룹화하여 과도한 이벤트 핸들러의 호출을 방지하는 프로그래밍 기법이다.
- 다음 코드는 버튼을 짧은 시간 간격으로 연속으로 클릭했을 때 일반적인 이벤트 핸들러와 디바운스, 스로틀된 이벤트 핸들러의 호출 빈도를 나타낸 코드다.

```html
<!DOCTYPE html>
<html lang="kr">
  <head>
    <meta charset="UTF-8" />
    <title>Practice</title>
  </head>
  <body>
    <button>Click me</button>
    <pre>일반 클릭 카운터 <span class="normal-msg">0</span></pre>
    <pre>디바운스 클릭 카운터 <span class="debounce-msg">0</span></pre>
    <pre>스로틀 클릭 카운터 <span class="throttle-msg">0</span></pre>
    <script>
      const $button = document.querySelector("button");
      const $normalMsg = document.querySelector(".normal-msg");
      const $debounceMsg = document.querySelector(".debounce-msg");
      const $throttleMsg = document.querySelector(".throttle-msg");

      const debounce = (callback, delay) => {
        let timeId;
        return (event) => {
          if (timerId) {
            clearTimeout(timeoutId);
          }
          timerId = setTimeout(callback, delay, event);
        };
      };

      const throttle = (callback, delay) => {
        let timerId;
        return (event) => {
          if (timerId) {
            return;
          }
          timerId = setTimeout(
            () => {
              callback(event);
              timerId = null;
            },
            delay,
            event
          );
        };
      };

      $button.addEventListener("click", () => {
        $normalMsg.textContent = +$normalMsg.textContent + 1;
      });

      $button.addEventListener(
        "click",
        debounce(() => {
          $normalMsg.textContent = +$debounceMsg.textContent + 1;
        }, 500)
      );

      $button.addEventListener(
        "click",
        throttle(() => {
          $normalMsg.textContent = +$throttleMsg.textContent + 1;
        }, 500)
      );
    </script>
  </body>
</html>
```

1. 디바운스

- 디바운스는 짧은 시간 간격으로 이벤트가 연속으로 발생하면 이벤트 핸들러를 호출하지 않다가 일정 시간이 경과한 이후에 이벤트 핸들러가 한 번만 호출되도록 한다.
  - 디바운스는 짧은 시간 간격으로 발생하는 이벤틀르 그룹화해서 마지막에 한 번만 이벤트 핸들러가 호출되도록한다.

```html
<!DOCTYPE html>
<html lang="kr">
  <head>
    <meta charset="UTF-8" />
    <title>Practice</title>
  </head>
  <body>
    <input type="text" />
    <div class="msg"></div>
    <script>
      const $input = document.querySelector("input");
      const $msg = document.querySelector(".msg");

      const debounce = (callback, delay) => {
        let timerId;
        // debounce 함수는 timerId를 기억하는 클로저를 반환한다.
        return (event) => {
          // delay가 경과하기 이전에 이벤트가 발생하면 이전 타이머를 취소하고 새로운 타이머를 설정
          // delay보다 짧은 간격으로 이벤트가 발생하면 callback은 호출되지 않음
          if (timerId) {
            clearTimeout(timeoutId);
          }
          timerId = setTimeout(callback, delay, event);
        };
      };

      // debounce 함수가 반환하는 클로저가 이벤트 핸들러로 등록된다.
      // 300ms보다 짧은 간격으로 input 이벤트가 발생하면 debounce 함수의 콜백 함수는
      // 호출되지 않다가 300ms 동안 input 이벤트가 더 이상 발생하면 한 번만 호출된다.
      $input.oninput = debounce((e) => {
        $msg.textContent = e.target.value;
      });
    </script>
  </body>
</html>
```

- input 이벤트는 사용자가 텍스트 입력 필드에 값을 입력할때마다 연속해서 발생한다.
- 사용자가 입력을 완료했는지 여부를 정확히 알 수 없으므로 일정 시간 동안 텍스트 입력 필드에 값을 입력하지 않으면 입력이 완료된 것으로 간주한다.
- debounce 함수가 반환한 함수는 debounce 함수에 두 번째 인수로 전달한 시간보다 짧은 간격으로 이벤트가 발생하면 이전 타이머를 취소하고 새로운 타이머를 재설정한다.
  - delay보다 짧은 간격으로 이벤트가 연속으로 발생하면 debounce 함수의 첫 번째 인수로 전달한 콜백 함수는 호출되지 않다가 delay 동안 input 이벤트가 더 이상 발생하지 않으면 한 번만 호출된다.
- 이처럼 짧은 시간 간격으로 이벤트가 연속해서 발생하면 이벤트 핸들러를 호출하지 않다가 일정 시간 동안 이벤트가 더 이상 발생하지 않으면 이벤트 핸들러가 한번만 호출되도록하는 디바운스는 resize 이벤트 처리나 input 요소에 입력된 ㄱ밧으로 ajax 요청으로 입력 필드 자동완성 UI 구현, 버튼 중복 클릭 방지 처리 등에 유용하게 사용된다.
- 실무에서는 Undescore의 디바운스 함수나 로데시의 디바운스 함수를 사용한다.

2. 스로틀

- 스로틀은 짧은 시간 간격으로 이벤트가 연속해서 발생하더라도 일정 시간 간격으로 이벤트 핸들러가 최대 한 번만 이벤트 핸들러가 최대 한 번만 호출되도록한다.
  - 스로틀은 짧은 시간 간격으로 연속해서 발생하는 이벤트를 그룹화해서 일정 시간 단위로 이벤트 핸들ㄹ러가 호출되도록 호출 주기를 만든다.

```html
<!DOCTYPE html>
<html lang="kr">
  <head>
    <meta charset="UTF-8" />
    <title>Practice</title>
    <style>
      .container {
        width: 300px;
        height: 300px;
        background-color: rebeccapurple;
        overflow: scroll;
      }

      .content {
        width: 300px;
        height: 1000vh;
      }
    </style>
  </head>
  <body>
    <div class="container">
      <div class="content"></div>
    </div>
    <div>
      일반 이벤트 핸들러가 scroll 이벤트를 처리한 횟수:
      <span class="normal-count">0</span>
    </div>
    <div>
      스로틀 이벤트 핸들러가 scroll 이벤트를 처리한 횟수:
      <span class="throttle-count">0</span>
    </div>

    <script>
      const $container = document.querySelector(".container");
      const $normalCount = document.querySelector(".normal-count");
      const $throttleCount = document.querySelector(".throttle-count");

      const throttle = (callback, delay) => {
        let timerId;
        // throttle 함수는 timerId를 기억하는 클로저를 반환
        return (event) => {
          // delay가 경과하기 이전에 이벤트가 발생하면 아무것도 하지 않다가
          // delay가 경과했을 때 이벤트가 발생하면 새로운 타이머를 재설정.
          // delay 간격으로 callback 호출.
          if (timerId) {
            return;
          }
          timerId = setTimeout(
            () => {
              callback(event);
              timerId = null;
            },
            delay,
            event
          );
        };
      };

      let normalCount = 0;
      $container.addEventListener("scroll", () => {
        $normalCount.textContent = ++normalCount;
      });

      let throttleCount = 0;
      // throttle 함수가 반환하는 클로저가 이벤트 핸들러로 등록된다.
      $container.addEventListener(
        "scroll",
        throttle(() => ($throttleCount.textContent = ++throttleCount), 100)
      );
    </script>
  </body>
</html>
```

- scroll 이벤트는 사용자가 사용자가 스크롤할 때 짧은 시간 간격으로 연속해서 발생한다.
- 짧은 시간 간격으로 연속해서 발생하는 이벤트의 과도한 이벤트 핸들러의 호출을 방지하기 위해 throttle 함수는 이벤트를 그룹화해서 일정 시간 단위로 이벤트 핸들러가 호출되도록 호출 주기를 만든다.
- 짧은 시간 간격으로 연속해서 발생하는 이벤트를 그룹화해서 일정 시간 간격으로 이벤트 핸들러를 호출하는 스로틀은 scroll 이벤트 처리나 무한 스크롤 UI구현 등에 유용하게 사용된다.
- 실무에서는 언더스코어, 로데시의 스로틀 함수를 사용한다.

# 비동기 프로그래밍

## 동기 처리와 비동기 처리

- 다음 코드의 함수는 호출된 순서대로 스택 자료구조인 실행 컨텍스트 스택에 푸시되어 실행된다.

```js
const foo = () => {};
const bar = () => {};

foo();
bar();
```

- 자바스크립트 엔진은 단 하나의 실행 컨텍스트 스택을 갖는다.
- 자바스크립트 엔진은 한 번에 하나의 테스크만 실행할 수 있는 싱글 스레드 방식으로 동작한다.
- 싱글 스레드 방식은 한 번에 하나의 태스크만 실행할 수 있기 때문에 처리에 시간이 걸리는 테스크를 실행하는 경우 블로킹(작업 중단)이 발생한다.

```js
// sleep 함수는 일정 시간(delay)이 경과한 이후에 콜백 함수(func)를 호출한다.
function sleep(func, delay) {
  // Date.now()는 현재 시간을 숫자(ms)로 반환한다.
  const delayUntil = Date.now() + delay;

  // 현재 시간에 delay를 더한 delayUntil이 현재 시간보다 작으면 계속 반복한다.
  while (Date.now() < delayUntil);

  // 일정 시간이 경과한 이후에 콜백 함수(func)를 호출한다.
  func();
}

function foo() {
  console.log("foo");
}

function bar() {
  console.log("bar");
}

// sleep 함수는 3초 이상 실행된다.
sleep(foo, 3 * 1000);
// bar 함수는 sleep 함수의 실행이 종료된 이후에 호출되므로 3초 이상 블로킹된다.
bar();
// 3초 경과 후 foo 호출 -> bar 호출
```

- 현재 실행 중인 테스크가 종료할 때까지 다음에 실행될 태스크가 대기하는 방식을 동기 처리하고한다.
- 동기 처리 방식은 태스크를 순서대로 하나씩 처리하므로 실행 순서가 보장된다는 장점이 있지만, 앞선 태스크가 종료할 때까지 이후 태스크들이 블로킹되는 단점이 있다.

```js
// sleep 함수는 일정 시간(delay)이 경과한 이후에 콜백 함수(func)를 호출한다.
function sleep(func, delay) {
  // Date.now()는 현재 시간을 숫자(ms)로 반환한다.
  const delayUntil = Date.now() + delay;

  // 현재 시간에 delay를 더한 delayUntil이 현재 시간보다 작으면 계속 반복한다.
  while (Date.now() < delayUntil);

  // 일정 시간이 경과한 이후에 콜백 함수(func)를 호출한다.
  func();
}

function foo() {
  console.log("foo");
}

function bar() {
  console.log("bar");
}

// 타이머 함수 setTimeout은 일정 시간이 경과한 이후에 콜백 함수 foo를 호출한다.
// 타이머 함수 setTimeout은 bar 함수를 블로킹 하지 않는다.
setTimeout(foo, 3 * 1000);
bar();
// bar 호출 -> (3초 대기) foo 호출
```

- 현재 실행중인 태스크가 종료되지 않은 상태라 해도 다음 태스크를 곧바로 실행하는 방식을 비동기 처리하고 한다.
- 비동기 처리 방식은 현재 실행중인 태스크가 종료되지 않은 상태라 해도 다음 태스크를 곧바로 실행하므로 블로킹이 발생하지 않는다는 장점이 있지만, 태스크의 실행 순서가 보장되지 않는 단점이 있다.
- 타이머 함수인 setTimeout과 setInterval, HTTP 요청, 이벤트 핸들러는 비동기 처리 방식으로 동작한다.

## 이벤트 루프와 태스크 큐

- 브라우저가 동작하는 것을 살펴보면 많은 태스크가 동시에 처리되는 것처럼 느껴진다.
- 자바스크립트의 동시성을 지원하는 것이 바로 이벤트 루프

### 콜 스택

- 실행 컨텍스트가 추가되고 제거되는 스택 자료구조인 실행 컨텍스트 스택이 콜스택

### 힙

- 힙은 객체가 저장되는 메모리 공간이다. 콜스택의 요소인 실행 컨텍스트는 힙에 저장된 객체를 참조한다.
- 비동기 처리에서 소스코드의 평가와 실행을 제외한 모든 처리는 자바스크립트 엔진을 구동하는 환경인 브라우저 또는 Node.js가 담당

### 태스크 큐

- setTimeout이나 setInterval과 같은 비동기 함수의 콜백함수 또는 이벤트 핸들러가 일시적으로 보관되는 영역

### 이벤트 루프

- 이벤트 루프는 콜 스택에 현재 실행 중인 실행 컨텍스트가 있는지, 그리고 태스크 큐에 대기중인 함수(콜백 함수, 이벤트 핸들러 등)가 있는지 반복해서 확인한다.
- 콜스택이 비어있고 태스크 큐에 대기 중인 함수가 있다면 이벤트 루프는 순차적으로(FIFO)으로 태스크 큐에 대기 중인 함수를 콜스택으로 이동시킨다.

- 비동기 함수인 setTimeout의 콜백 함수는 태스크 큐에 푸시되어 대기하다가 콜 스택이 비게되면, 다시 말해 전역 코드 및 명식적으로 호출된 한수가 모두 종료하면 비로소 콜 스택에 푸시되어 실행된다.
- 자바스크립트 싱글 스레드 방식으로 동작한다.
- 이때 싱글 스레드 방식으로 동작하는 것은 브라우저가 아니라 브라우저에 내장된 자바스크립트 엔진이다.
- 즉, 자바스크립트 엔진은 싱글 스레드로 동작하지만 브라우저는 멀티 스레드로 동작한다.
