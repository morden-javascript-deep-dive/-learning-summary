# DOM

- DOM(Document Object Model)은 HTML 문서의 계층적 구조와 정보를 표현하며 이를 제어할 수 있는 API, 즉 프로퍼티와 메서드를 제공하는 트리 자료구조이다.

## 노드

1. HTML 요소와 노드 객체

- HTML 요소는 HTML 문서를 구성하는 개별적인 요소
- HTML 요소는 렌더링 엔진에 의해 파싱되어 DOM을 구성하는 요소 노드 객체로 변환된다.
- 이때 HTML 요소의 어트리뷰트는 어트리뷰트 노드로, HTML 요소의 텍스트 콘텐츠는 텍스트 노드로 변환된다.
- HTML 요소 간의 부자 관계를 반영하여 HTML 문서의 구성 요소인 HTML 요소를 객체화한 모든 노드 객체들은 트리 자료구조로 구성된다.

2. 노드 객체의 타입

```html
<!DOCTYPE html>
<html lang="kr">
  <head>
    <meta charset="UTF-8" />
    <link rel="stylesheet" href="style.css" />
    <title>Title</title>
  </head>
  <body>
    <ul>
      <li id="apple">Apple</li>
      <li id="banana">Banana</li>
      <li id="orange">Orange</li>
    </ul>
    <script src="app.js"></script>
  </body>
</html>
```

- DOM은 노드 객체의 계층적인 구조로 구성된다. 노드 객체는 종류가 있고 상속 구조를 갖는다.
- `문서 노드`
  - 문서 노드는 DOM 트리의 최상위에 존재하는 루트 노드로서 document 객체를 가리킨다.
  - 문서 노드, 즉 document 객체는 DOM 트리의 루트 노드 이므로 DOM 트리의 노드들에 접근하기 위해 진입점 역할을 담당한다.
  - 요소, 어트리뷰트, 텍스트 노드에 접근하려면 문서 노드를 통해야한다.
- `요소 노드`: 요소 노드는 HTML 요소를 가리키는 객체다. 요소 노드는 문서의 구조를 표현한다고 할 수 있다.
- `어트리뷰트 노드`
  - 어트리뷰트 노드는 HTML 요소의 어트리뷰트를 가리키는 객체다. 어트리뷰트 노드는 어트리뷰트가 지정된 HTML 요소의 요소 노드와 형제 관계를 갖는다.
  - 단, 요소 노드는 부모 노드와 연결되어 있지만 어트리뷰트 노드는 부모 노드와 연결되어 있지 않고 형제 노드인 요소 노드에만 연결되어 있다.
  - 어트리뷰트 노드에 접근하여 어트리뷰트를 참조하거나 변경하려면 먼저 형제 노드인 요소 노드에 접근해야 한다.
- `텍스트 노드`
  - 텍스트 노드는 HTML 요소의 텍스트를 가리키는 객체다. 텍스트 노드는 요소 노드의 자식 노드이며, 자식 노드를 가질 수 없는 리프 노드다.
  - 텍스트 노드는 DOM 트리의 최종단이다.

3. 노드 객체의 상속 구조

- DOM을 구성하는 노드 객체는 자신의 구조와 정보를 제어할 수 있는 DOM API를 사용할 수 있다.
- 노드 객체는 자신의 부모, 형제, 자식을 탐색할 수 있으며, 자신의 어트리뷰트와 텍스트를 조작할 수 있다.
- DOM은 HTML 문서의 계층적 구조와 정보를 표현하는 것은 물론 노드 객체의 종류, 즉 노드 타입에 따라 필요한 기능을 프로퍼티와 메서드의 집합인 DOM API로 제공한다. 이 DOM API를 통해 HTML의 구조나 내용 또는 스타일 등을 동적으로 조작할 수 있다.

## 요소 노드 취득

- 요소 노드의 취득은 HTML 요소를 조작하는 시작점이다.

1. id를 이용한 요소 노드 취득

- Document.prototype.geElementById 메서드는 인수로 전달한 id 어트리뷰트 값을 갖는 하나의 요소 노드를 탐색하여 반환한다.
- 반드시 문서 노드인 document를 통해 호출해야한다.

```html
<!DOCTYPE html>
<html lang="kr">
  <head>
    <meta charset="UTF-8" />
    <link rel="stylesheet" href="style.css" />
    <title>Title</title>
  </head>
  <body>
    <ul>
      <li id="apple">Apple</li>
      <li id="banana">Banana</li>
      <li id="orange">Orange</li>
    </ul>
    <script>
      // id 값이 banana 인 요소 노드를 탐색하여 반환
      // 두 번째 li 요소가 파싱되어 생성된 요소 노드가 반환
      const $elem = document.getElementById("banana");

      // 취득한 요소 노드의 style.color 프로퍼티 값을 변경한다.
      $elem.style.color = "red";
    </script>
  </body>
</html>
```

- id 값은 HTML 문서 내에서 유일한 값이어야 하며, class 어트리뷰터와는 달리 공백 문자로 구분하여 여러개의 값을 가질 수 없다.
- 단, HTML 문서내에 중복된 id 값을 갖는 HTML 요소가 여러개 존대하더라도 어떠한 에러도 발생하지 않는다.
- 언제나 하나의 요소 노드를 반환한다.

```html
<body>
  <ul>
    <li id="banana">Apple</li>
    <li id="banana">Banana</li>
    <li id="banana">Orange</li>
  </ul>
  <script>
    // getElementById 메서드는 언제나 단 하나의 요소 노드를 반환.
    // 첫 번째 li 요소가 파싱되어 생성된 요소 노드가 반환
    const $elem = document.getElementById("banana");

    // 취득한 요소 노드의 style.color 프로퍼티 값을 변경한다.
    $elem.style.color = "red";
  </script>
</body>
```

- 만약 인수로 전달된 id 값은 갖는 HTML 요소가 존재하지 않는 경우 getElementId 메서드는 null을 반환한다.

```html
<body>
  <ul>
    <li id="apple">Apple</li>
    <li id="banana">Banana</li>
    <li id="orange">Orange</li>
  </ul>
  <script>
    // id가 grape 인 요소 노드를 탐색하여 반환. null이 반환된다.
    const $elem = document.getElementById("grape");

    // 취득한 요소 노드의 style.color 프로퍼티 값을 변경한다.
    $elem.style.color = "red"; // TypeError
  </script>
</body>
```

- HTML 요소에 id 어트리뷰트를 부여하면 id 값과 동일한 이름의 전역 변수가 암묵적으로 선언되고 해당 노드 객체가 할당되는 부수효과가 있다.

```html
<body>
  <div id="foo"></div>
  <body>
    <div id="foo"></div>
    <body>
      <div id="foo"></div>
      <script>
        let foo = 1;

        // id 값과 동일한 이름의 전역 변수가 이미 선언되어 있으면 노드 객체가 재할당되지 않는다.
        console.log(foo);
      </script>
    </body>
    <script>
      let foo = 1;

      // id 값과 동일한 이름의 전역 변수가 이미 선언되어 있으면 노드 객체가 재할당되지 않는다.
      console.log(foo);
    </script>
  </body>
  <script>
    // id 값과 동일한 이름의 전역 변수가 암묵적으로 선언되고 해당 노드 객체가 할당.
    console.log(foo === document.getElementById("foo")); // true

    // 암묵적 전역으로 생성된 전역 프로퍼티는 삭제되지만 전역 변수는 삭제되지 않는다.
    delete foo;
    console.log(foo); // <div id="foo"></div>
  </script>
</body>
```

- 단, id 값과 동일한 이름의 전역 변수가 이미 선어되어 있으면 이 전역 변수에 노드 객체가 재할당되지 않는다.

```html
<body>
  <div id="foo"></div>
  <script>
    let foo = 1;

    // id 값과 동일한 이름의 전역 변수가 이미 선언되어 있으면 노드 객체가 재할당되지 않는다.
    console.log(foo);
  </script>
</body>
```

2. 태그 이름을 이용한 요소 노드 취득

- Document.prototype/Element.prototype.getElementsByTagName 메서드는 인수로 전달한 태그 이름을 갖는 모든 요소 노드들을 탐색하여 반환한다.
- getElementsByTagName 메서드는 여러개의 요소 노드 객체를 갖는 DOM 컬렉션 객체인 HTMLCollection 객체를 반환한다.

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <title>Title</title>
  </head>
  <body>
    <ul>
      <li id="apple">Apple</li>
      <li id="banana">Banana</li>
      <li id="orange">Orange</li>
    </ul>
    <script>
      // HTMLCollection 객체는 유사 배열 객체이면서 이터러블이다.
      const $elem = document.getElementsByTagName("li");

      // HTMLCollection 객체를 배열로 변환하여 순회하면서 color 프로퍼티 값을 변경한다.
      [...$elem].forEach((elem) => {
        elem.style.color = "red";
      });
    </script>
  </body>
</html>
```

- getElementsByTagName 메서드가 반환하는 DOM 컬렉션 객체인 HTMLCollection 객체는 유사 배열 객체이면서 이터러블이다.
- HTML 문서의 모든 요소 노드를 취득하려면 getElementsByTagName 메서드의 인수로 \*를 전달한다.

```js
const $all = document.getElementsByIdTagName("*");
```

- Document의 getElementsByTagName 메서드는 DOM의 루트 노드인 문서 노드, 즉 documnet를 통해 호출하며 DOM 전체에서 요소 노드를 탐색하여 반환한다.
- Element의 getElementsByTagName 메서드는 특정 요소 노드를 통해 호출하며, 특정 요소 노드의 자손 노드 중에서 요소 노드를 탐색하여 반환하다.

```html
<!DOCTYPE html>
<html lang="kr">
  <head>
    <meta charset="UTF-8" />
    <title>Title</title>
  </head>
  <body>
    <ul id="fruits">
      <li>Apple</li>
      <li>Banana</li>
      <li>Orange</li>
    </ul>
    <ul>
      <li>HTML</li>
    </ul>
    <script>
      const $lisFromDocument = document.getElementsByTagName("li");
      console.log($lisFromDocument); // HTMLCollection(4) [li, li, li, li]

      // ul#fruits 요소의 자손 노드 중에서 태그 이름이 li인 요소 노드를 모두 탐색하여 반환
      const $fruits = document.getElementById("fruits");
      const $lisFromFruits = $fruits.getElementsByTagName("li");
      console.log($lisFromDocument); // HTMLCollection(3) [li, li, li]
    </script>
  </body>
</html>
```

- 만약 인수로 전달된 태그 이름을 갖는 요소가 존재하지 않는 경우 getElementsByTagName 메서드는 빈 HTMLCollection 객체를 반환한다.

3. class를 이용한 요소 노드 취득

- Documnet.prototype/Element.prototype.getElementByClassName 메서드는 인수로 전달한 class 어트리뷰트 값을 갖는 모든 요소 노드들을 탐색하여 반환한다.

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <title>Title</title>
  </head>
  <body>
    <ul>
      <li class="fruit apple">Apple</li>
      <li class="fruit banana">Banana</li>
      <li class="fruit orange">Orange</li>
    </ul>
    <script>
      // class 값이 fruit 인 요소 노드를 모두 탐색하여 HTMLCollection 객체를 담아 반환
      const $elem = document.getElementsByClassName("fruit");

      // 취득한 모든 요소의 CSS color 프로퍼티 값을 변경.
      [...$elem].forEach((elem) => {
        elem.style.color = "red";
      });

      // class 값이 fruit apple 인 요소 노드를 모두 탐색하여 HTMLCollection 객체를 담아 반환
      const $apples = document.getElementsByClassName("fruit apple");

      [...$apples].forEach((elem) => {
        elem.style.color = "blue";
      });
    </script>
  </body>
</html>
```

- Document의 getElementsByClassName 메서드는 DOM의 루트 노드인 문서 노드, 즉 doucment를 통해 호출하며 DOM 전체에서 요소 노드를 탐색하여 반환한다.
- Element의 getElementsByClassName 메서드는 특정 요소 노드를 통해 호출하며, 특정 요소 노드의 자손 노드 중에서 요소 노드를 탐색하여 반환한다.

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <title>Title</title>
  </head>
  <body>
    <ul id="fruits">
      <li class="apple">Apple</li>
      <li class="banana">Banana</li>
      <li class="orange">Orange</li>
    </ul>
    <div class="banana">Banana</div>
    <script>
      // DOM 전체에서 class 값이 banana 인 요소 노드를 모두 탐색하여 반환
      const $bananasFromDocument = document.getElementsByClassName("banana");
      console.log($bananasFromDocument);
      // HTMLCollection(2) [li.banana, div.banana]

      // #fruits 요소의 자손 노드 중에서 class 값이 banana 인 요소 노드를 모두 탐색하여 반환
      const $fruits = document.getElementById("fruits");
      const $bananaFromFruits = $fruits.getElementsByClassName("banana");

      console.log($bananaFromFruits); // HTMLCollection [li.banana]
    </script>
  </body>
</html>
```

- 만약 인수로 전달된 태그 이름을 갖는 요소가 존재하지 않는 경우 getElementsByClassName 메서드는 빈 HTMLCollection 객체를 반환한다.

4. CSS 선택자를 이용한 요소 노드 취득

- CSS 선택자는 스타일을 적용하고자 하는 HTML 요소를 특정할 때 사용하는 문법이다.

```
/* 전체 선택자 */
* {
  ...;
}
/* 태그 선택자 */
p {
  ...;
}
/* id 선택자 */
#foo {
  ...;
}
/* 등등 */
```

- Document.prototype/Element.prototype.querySelector 메서드는 인수로 전달한 CSS 선택자를 만족시키는 하나의 요소 노드를 탐색하여 반환한다.
  - 인수로 전달한 CSS 선택자를 만족시키는 요소 노드가 여러개인 경우 첫번째 요소 노드만 반환
  - 인수로 전달한 CSS 선택자를 만족시키느 ㄴ요소 노드가 존재하지 않는 경우 null 반환
  - 인수로 전달한 CSS 선택자가 문법에 맞지 않는 경우 DOMException 에러가 발생

```html
<!DOCTYPE html>
<html lang="kr">
  <head>
    <meta charset="UTF-8" />
    <title>Title</title>
  </head>
  <body>
    <ul>
      <li class="apple">Apple</li>
      <li class="banana">Banana</li>
      <li class="orange">Orange</li>
    </ul>
    <script>
      // class 어트리뷰트 값이 banana 인 첫 번째 요소 노드를 탐색하여 반환
      const $elem = document.querySelector(".banana");

      // 취득한 요소 노드의 style.color 프로퍼티 값을 변경
      $elem.style.color = "red";
    </script>
  </body>
</html>
```

- Document.prototype/Element.prototype.querySelectorAll 메서드는 인수로 전달한 CSS 선택자를 만족시키는 모든 요소 노드를 탐ㄴ색하여 반환한다.
- querySelectorAll 메서드는 여러 개의 요소 노드 객체를 갖는 DOM 컬렉션 객체인 NodeList 객체를 반환한다. NodeList 객체는 유사 배열 객체이면서 이터러블이다.
  - 인수로 전달한 CSS 선택자를 만족시키는 요소 노드가 존재하지 않는 경우 null 반환
  - 인수로 전달한 CSS 선택자가 문법에 맞지 않는 경우 DOMException 에러가 발생

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <title>Title</title>
  </head>
  <body>
    <ul>
      <li class="apple">Apple</li>
      <li class="banana">Banana</li>
      <li class="orange">Orange</li>
    </ul>
    <script>
      // ul 요소의 자식 요소인 li 요소를 모두 탐색하여 반환
      const $elem = document.querySelectorAll("ul > li");
      // 취득한 요소 노드들은 NodeList 객체에 담겨 반환
      console.log($elem); // NodeList(3) [li.apple, li.banana, li.orange]

      // 취득한 모든 요소 노드의 style.color 프로퍼티 값을 변경
      $elem.forEach((elem) => {
        elem.style.color = "red";
      });
    </script>
  </body>
</html>
```

- HTML 문서의 모든 요소 노드를 취득하려면 querySelectorAll 메서드의 인수로 전체 선태가 \*를 전달한다.

```js
const $all = document.querySelectorAll("*");
```

- CSS 선택자 문법을 사용하는 querySelector, querySelectorAll 메서드는 getElemntById, getElementBy\*\* 메서드보다 다소 느린것으로 알려져 있다.
- 하지만 CSS 선택자 문법을 사용하여 좀 더 구체적인 조건으로 요소 노드를 취득할 수 있고 일관된 방식으로 요소 노들르 취득할 수 있다는 장점이 있다.
- id 어트리뷰트가 있는 요소 노드를 취득하는 경우에는 getElemnetById 메서드를 사용하고 그 외의 경우에는 qeurytSlector, querySelectorAll 메서드를 사용하는 것이 권장된다.

5. 특정 요소 노드를 취득할 수 있는지 확인

- Element.prototype.matches 메서드는 인수로 전달한 CSS 선택자를 통해 특정 요소 노드를 취득할 수 있는지 확인한다.

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <title>Title</title>
  </head>
  <body>
    <ul id="fruits">
      <li class="apple">Apple</li>
      <li class="banana">Banana</li>
      <li class="orange">Orange</li>
    </ul>
    <script>
      const $apple = document.querySelector(".apple");

      // $apple 노드는 #fruits > li.apple 로 취득할 수 있다.
      console.log($apple.matches("#fruits > li.apple")); // true

      // $apple 노드는 #fruits > li.banana 로 취득할 수 없다.
      console.log($apple.matches("#fruits > li.banana")); // false
    </script>
  </body>
</html>
```

- Element.prototype.matches 메서드는 이벤트 위임을 사용할 때 유용하다.

6. HTMLCollection과 NodeList

- getElementsByTagName, getElementsByClassName 메서드가 반환하는 HTMLCollection 객체는 객체의 상태 변화를 실시간으로 반영하는 살아있는 DOM 컬렉션 객체다.

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <style>
      .red {
        color: red;
      }
      .blue {
        color: blue;
      }
    </style>
  </head>
  <body>
    <ul id="fruits">
      <li class="red">Apple</li>
      <li class="red">Banana</li>
      <li class="red">Orange</li>
    </ul>
    <script>
      // class 값이 red 인 요소 노드를 모두 탐색하여 HTMLCollection 객체에 담아 반환
      const $elems = document.getElementsByClassName("red");
      // 이 시점에 HTMLCollection 객체에는 3개의 요소 노드가 담겨 있다.
      console.log($elems); // HTMLCollection(3) [li.red, li.red, li.red]

      // HTMLCollection 객체의 모든 요소의 class 값을 blue 로 변경
      for (let i = 0; i < $elems.length; i++) {
        $elems[i].className = "blue";
      }

      // HTMLCollection 객체의 요소가 3개에서 1개로 변경
      console.log($elems); // HTMLCollection(1) [li.red]
    </script>
  </body>
</html>
```

- 위 코드에서 두번째 li 요소만 class 값이 변경되지 않는다.
- HTMLCollction
  - HTMLCollction 객체는 실시간으로 노드 객체의 상태 변경을 반영하여 요소를 제거할 수 있기 때문에 HTMLCollection 객체를 for문으로 순회하면서 노드 객체의 상태를 변경해야 할 떄 주의해야한다.
  - 이 문제를 해결할 수 있는 방법은 HTML 객체를 사용하지 않는 것이다.
  - 유사 배열 객체이면서 이터러블인 HTMLCollection 객체를 변환하면 부작용을 발생시키지 않고 고차 함수를 사용할 수 있다.
- NodeList

  - querySelectorAll 메서드는 DOM 컬렉션 객체인 NodeList 객체를 반환한다.
  - 이때 NodeList 객체는 실시간으로 노드 객체의 상태 변경을 반영하지 않는 객체다.

  ```js
  const $elems = document.querySelectorAll(".red");

  $elems.forEach((elem) => (elem.className = "blue"));
  ```

  - NodeList 객체는 대부분의 경우 노드 객체의 상태 변경을 실시간으로 반영하지 않고 과거의 정적 상태를 유지하는 non-live 객체로 동작한다.
  - 하지만 ChildNodes 프로퍼티가 반환하는 NodeList 객체는 HTMLCollection 객체와 같이 실시간으로 노드 객체의 상태 변경을 반영하는 live 객체로 동작하므로 주의가 필요하다.

  ```html
  <!DOCTYPE html>
  <html lang="en">
    <head> </head>
    <body>
      <ul id="fruits">
        <li>Apple</li>
        <li>Banana</li>
        <li>Orange</li>
      </ul>
      <script>
        const $fruits = document.getElementById("fruits");

        // childNodes 프로퍼티는 NodeList 객체(live)를 반환한다.
        const { childNodes } = $fruits;
        console.log(childNodes instanceof NodeList); // true

        // $fruits 요소의 자식 노드는 공백 텍스트 노드를 포함해 모두 5개다.
        console.log(childNodes); // NodeList(5) [text, li, text, li, text]

        for (let i = 0; i < childNodes.length; i++) {
          // removeChild 메서드는 $fruits 요소의 자식 노드를 DOM 에서 삭제
          // removeChild 메서드는 호출할 때마다 NodeList 객체인 childNodes 실시간으로 변경
          // 첫 번째, 세 번째, 다섯 번째 요소만 삭제된다.
          $fruits.removeChild(childNodes[i]);
        }

        // $fruits 요소의 모든 자식 노드가 삭제되지 않는다.
        console.log(childNodes); // NodeList(2) [li, li]
      </script>
    </body>
  </html>
  ```

  - 노드 객체의 상태 변경과 상관없이 안전하게 DOM 컬렉션을 사용하려면 HTMLCollection이나 NodeList 객체를 배영로 변환하여 사용하는 것을 권장한다.

```js
[...$elems].forEach((elem) => (elem.className = "blue"));
```

## 노드 탐색

- DOM 트리의 노드를 옮겨 다니며 탐색해야 할 때가 있다.

```html
<ul id="fruits">
  <li class="apple">Apple</li>
  <li class="banana">Banana</li>
  <li class="orange">Orange</li>
</ul>
```

- ul#fruits 요소는 3개의 자식 요소를 갖는다. 먼저 ul#fruits 요소 노드를 취득한 다음, 자식 노드를 모두 탐색하거나 자식 노드 중 하나만 탐색할 수 있다.
- 노드 탐색 프로퍼티는 모두 접근자 프로퍼티다. 단, 노드 탐색 프로퍼티는 setter 없이 getter만 존재하여 참조한 가능한 읽기 전용 접근자 프로퍼티다.

1. 공백 텍스트 노드

- 공백 문자는 텍스트 노드를 생성한다. 이를 공백 텍스트 노드라 한다.

```html
<!DOCTYPE html>
<html lang="en">
  <head> </head>
  <body>
    <ul id="fruits">
      <li class="apple">Apple</li>
      <li class="banana">Banana</li>
      <li class="orange">Orange</li>
    </ul>
  </body>
</html>
```

- HTML 문서의 공백 문자는 공백 텍스트 노드를 생성한다. 노드를 탐색할 때는 공백 문자가 생성한 공백 텍스트 노드에 주의해야 한다.
- 인위적으로 HTML 문서의 공백 문자를 제거하면 공백 텍스트 노드를 생성하지 않는다.

```html
<ul id="fruits">
  <li class="apple">Apple</li>
  <li class="banana">Banana</li>
  <li class="orange">Orange</li>
</ul>
```

2. 자식 노드 탐색

- 자식 노드를 탐색하기 위해서는 다음과 같은 노드 탐색 프로퍼티를 사용한다.

```html
<!DOCTYPE html>
<html lang="en">
  <body>
    <ul id="fruits">
      <li class="apple">Apple</li>
      <li class="banana">Banana</li>
      <li class="orange">Orange</li>
    </ul>
    <script>
      // 노드 탐색의 기점이 되는 #fruits 요소 노드를 취득한다.
      const $fruits = document.getElementById("fruits");

      // #fruits 요소의 모든 자식 노드를 탐색한다.
      // childNodes 프로퍼티가 반환한 NodeList에는 요소 노드뿐만 아니라 텍스트 노드도 포함되어 있다.
      console.log($fruits.childNodes);
      // NodeList(7) [text, li.apple, text, li.banana, text, li.orange, text]

      // #fruits 요소의 모든 자식 노드를 탐색한다.
      // children 프로퍼티가 반환한 HTMLCollection에는 요소 노드만 포함되어 있다.
      console.log($fruits.children);
      // HTMLCollection(3) [li.apple, li.banana, li.orange]

      // #fruits 요소의 첫 번째 자식 노드를 탐색한다.
      // firstChild 프로퍼티는 텍스트 노드를 반환할 수도 있다.
      console.log($fruits.firstChild); // #test

      // #fruits 요소의 마지막 자식 노드를 탐색한다.
      // lastChild 프로퍼티는 텍스트 노드를 반환할 수도 있다.
      console.log($fruits.lastChild);

      // #fruits 요소의 첫 번째 자식 노드를 탐색한다.
      // firstElementChild 프로퍼티는 요소 노드만 반환한다.
      console.log($fruits.firstElementChild); // li.apple

      // #fruits 요소의 마지막 자식 노드를 탐색한다.
      // lastElementChild 프로퍼티는 요소 노드만 반환한다.
      console.log($fruits.lastElementChild); // li.orange
    </script>
  </body>
</html>
```

3. 자식 노드 존재 확인

- 자식 노드가 존재하는 확인하려면 Node.prototype.hasChildNodes 메서드를 사용한다. hasChildNodes 메서드는 자식 노드가 존재하면 true, 자식 노드가 존재하지 않으면 false를 반환한다.
- 단, hasChildNodes 메서드는 childNodes 프로퍼티와 마찬가지로 텍스트 노드를 포함하여 자식 노드의 존재를 확인한다.
- 요소 노드를 확인하려면 childElementCount 프로퍼티를 사용한다.

```html
<!DOCTYPE html>
<html lang="en">
  <body>
    <ul id="fruits"></ul>
    <script>
      // 노드 탐색의 기점이 되는 #fruits 요소 노드를 취득한다.
      const $fruits = document.getElementById("fruits");

      // #fruits 요소에 자식 노드가 존재하는지 확인
      // hasChildNodes 메서드는 텍스트 노드를 포함하여 자식 노드의 존재 확인
      console.log($fruits.hasChildNodes()); // true

      // 자식 노드 중에 텍스트 노드가 아닌 요소 노드가 존재하는지 확인
      console.log(!!$fruits.childElementCount); // 0 -> false
    </script>
  </body>
</html>
```

4. 요소 노드의 텍스트 노드 탐색

- 요소 노드의 텍스트 노드는 요소 노드의 자식노드다. 즉, 요소 노드의 텍스트 노드는 firstChild 프로퍼티로 접근할 수 있다.
- firstChild 프로퍼티는 첫 번째 자식 노드를 반환하며, 텍스트 노드이거나 요소 노드다.

```html
<!DOCTYPE html>
<html lang="en">
  <body>
    <div id="foo">Hello</div>
    <script>
      // 요소 노드의 텍스트 노드는 firstChild 프로퍼티로 접근할 수 있다.
      console.log(document.getElementById("foo").firstChild); // #text
    </script>
  </body>
</html>
```

5. 부모 노드 탐색

- 부모 노드를 탐색하려면 Node.prototype.parentNode 프로퍼티를 사용한다.
- 텍스트 노드는 DOM 트리의 최종단 노드인 리프 노드이므로 부모 노드가 텍스트 노드인 경우는 없다.

```html
<!DOCTYPE html>
<html lang="en">
  <body>
    <ul id="fruits">
      <li class="apple">Apple</li>
      <li class="banana">Banana</li>
      <li class="orange">Orange</li>
    </ul>
    <script>
      // 노드 탐색의 기점이 되는 .banana 요소 노드를 취득한다.
      const $banana = document.querySelector(".banana");

      // .banana 요소 노드의 부모 노드를 탐색한다.
      console.log($banana.parentNode); // ul#fruits
    </script>
  </body>
</html>
```

6. 형제 노드 탐색

- 부모 노드가 같은 형제 노드를 탐색하려면 다음과 같은 노드 탐색 프로퍼티를 사용한다.
- 단, 어트리뷰트 노드는 요소 노드의 형제 노드이지만 부모 노드가 같은 형제 노드가 아니기 때문에 반환되지 않는다.

```html
<!DOCTYPE html>
<html lang="en">
  <body>
    <ul id="fruits">
      <li class="apple">Apple</li>
      <li class="banana">Banana</li>
      <li class="orange">Orange</li>
    </ul>
    <script>
      // 노드 탐색의 기점이 되는 #fruits 요소 노드를 취득한다.
      const $fruits = document.getElementById("fruits");

      // #fruits 요소의 첫 번째 자식 노드를 탐색
      // firstChild 프로퍼티는 요소 노드뿐만 아니라 텍스트 노드를 반환할 수 있다.
      const { firstChild } = $fruits;
      console.log(firstChild); // #text

      // #fruits 요소의 첫 번째 자식 노드(텍스트 노드)의 다음 형제 노드를 탐색
      // nextSibling 프로퍼티는 요소 노드뿐만 아니라 텍스트 노드를 반환할 수 있다.
      const { nextSibling } = firstChild;
      console.log(nextSibling); // li.apple

      // li.apple 요소의 이전 형제 노드를 탐색
      // previousSibling 프로퍼티는 요소 노드뿐만 아니라 텍스트 노드를 반환할 수 있다.
      const { previousSibling } = nextSibling;
      console.log(previousSibling); // #text

      // #fruits 요소의 첫 번째 자식 노드를 탐색
      // firstElementChild 프로퍼티는 요소 노드만 반환
      const { firstElementChild } = $fruits;
      console.log(firstElementChild); // li.apple

      // #fruits 요소의 첫 번째 자식 노드(li.apple)의 다음 형제 노드를 탐색
      // nextElementSibling 프로퍼티는 요소 노드만 반환
      const { nextElementSibling } = firstElementChild;
      console.log(nextElementSibling); // li.banana

      // li.banana 요소의 이전 요소 노드를 탐색
      // previousElementSibling 프로퍼티는 요소 노드만 반환
      const { previousElementSibling } = nextElementSibling;
      console.log(previousElementSibling); // li.apple
    </script>
  </body>
</html>
```

## 노드 정보 취득

- 노드 객체에 대한 정보를 취득하려면 다음과 같은 노드 정보 프로퍼티를 사용한다.

- Node.prototype.nodeType
  - 노드 객체의 종류, 즉, 노드 타입을 나타내는 상수를 반환한다.
  - 노드 타입 상수는 Node에 정의 되어 있다.
    - Node.ELEMENT_NODE: 요소 노드 타입을 나타내는 상수 1을 반환
    - Node.TEXT_NODE: 텍스트 노드 타입을 나타내는 상수 3을 반환
    - Node.DOCUMNENT_NODE: 문서 노드 타입을 타나내는 상수 9를 반환
- Node.prototype.nodeName
  - 노드의 이름을 문자열로 반환
    - 요소 노드: 대문자 문자열을 테그이름("UL", "LI" 등)을 반환
      - 텍스트 노드: 문자열 "#text"를 반환
      - 문서 노드: 문자열 "#document"를 반환

```html
<!DOCTYPE html>
<html lang="en">
  <body>
    <div id="foo">Hello</div>
    <script>
      // 문서 노드의 노드 정보를 취득
      console.log(document.nodeType); // 9
      console.log(document.nodeName); // #document

      // 요소 노드의 노드 정보를 취득
      const $foo = document.getElementById("foo");
      console.log($foo.nodeType); // 1
      console.log($foo.nodeName); // DIV

      // 텍스트 노드의 노드 정보를 취득
      const $textNode = $foo.firstChild;
      console.log($textNode.nodeType); // 3
      console.log($textNode.nodeName); // #text
    </script>
  </body>
</html>
```

## 요소 노드의 텍스트 조작

1. nodeValue

- setter, getter 모두 존재하는 접근자 프로퍼티
- 노드 객체의 nodeValue 프로퍼티를 참조하면 노드 객체의 값을 반환한다. 노드 객체의 값이란 텍스트 노드의 텍스트
- 텍스트 노드가 아닌 노드, 즉 문서 노드나 요소 노드의 nodeValue 프로퍼티를 참조하면 null을 반환한다.

```html
<!DOCTYPE html>
<html lang="en">
  <body>
    <div id="foo">Hello</div>
    <script>
      // 문서 노드의 nodeValue 프로퍼티를 참조한다.
      console.log(document.nodeValue); // null

      // 요소 노드의 nodeValue 프로퍼티를 참조한다.
      const $foo = document.getElementById("foo");
      console.log($foo.nodeValue); // null

      // 텍스트 노드의 nodeValue 프로퍼티를 참조한다.
      const $textNode = $foo.firstChild;
      console.log($textNode.nodeValue); // Hello
    </script>
  </body>
</html>
```

- 텍스트 노드의 nodeValue 프로퍼티에 값을 할당하면 텍스트 노드의 값, 즉 텍스트를 변경할 수 있다.
- 요소 노드의 텍스트를 변경하려면 다음과 같은 순서의 처리가 필요하다.

```html
<!DOCTYPE html>
<html lang="en">
  <body>
    <div id="foo">Hello</div>
    <script>
      // 1. #foo 요소 노드의 자식 노드인 텍스트 노드를 취득한다.
      const $textNode = document.getElementById("foo").firstChild;

      // 2. nodeValue 프로퍼티를 사용하여 텍스트 노드의 값을 변경한다.
      $textNode.nodeValue = "World";

      console.log($textNode.nodeValue); // World
    </script>
  </body>
</html>
```

2. textContent

- setter, getter 모두 존재하는 접근자 프로퍼티로서 요소 노드의 텍스트와 모든 자손 노드의 텍스트를 모두 취득하거나 변경한다.
- HTML 마크업은 무시하고 모든 텍스트를 반환한다.

```html
<!DOCTYPE html>
<html lang="en">
  <body>
    <div id="foo">Hello <span>world!</span></div>
    <script>
      // #foo 요소 노드의 텍스트를 모두 취득한다. 이때 HTML 마크업은 무시된다.
      console.log(document.getElementById("foo").textContent); // Hello world!
    </script>
  </body>
</html>
```

- 만약 요소 노드의 콘텐츠 영역에 자식 요소 노드가 없고 텍스트만 존재한다면 firstChild.Nodevalue와 textContent 프로퍼티는 같은 결과를 반환한다.
- 이 경우 textContent 프로퍼티를 사용하는 편이 코드가 더 간단하다.
- 요소 노드의 textContent 프로퍼티에 문자열을 할당하면 요소 노드의 모든 자식 노드가 제거되고 할당한 문자열이 텍스트로 추가된다.
- 이때 할당한 문자열에 HTML 마크업이 포함되어 있더라도 문자열 그대로 인식되어 텍스트로 취급된다.

```html
<!DOCTYPE html>
<html lang="en">
  <body>
    <div id="foo">Hello <span>world!</span></div>
    <script>
      document.getElementById("foo").textContent = "Hi <span>there!</span>";
    </script>
  </body>
</html>
```

## DOM 조작

- DOM 조작은 새로운 노드를 생성하여 DOM 추가하거나 기존 노드를 삭제 또는 교체 하는 것을 말한다.
- DOM 조작에 의해 DOM에 새로운 노드가 추가되거나 삭제되면 리플로우와 리페인트가 발생하는 원인이 되므로 성능에 영향을 준다.

1. innerHTML

- Element.prototype.innerHTML 프로퍼티는 setter, getter 모두 존재하는 접근자 프로퍼티
- 요소 노드의 HTML 마크업을 취득하거나 변경한다.

```html
<!DOCTYPE html>
<html lang="en">
  <body>
    <div id="foo">Hello <span>world!</span></div>
    <script>
      // #foo 요소의 콘텐츠 영역 내의 HTML 마크업을 문자열로 취득한다.
      console.log(document.getElementById("foo").innerHTML);
      // "Hello <span>world!</span>"
    </script>
  </body>
</html>
```

- 요소 노드의 innerHTML 프로퍼티에 문자열을 할당하면 요소 노드의 모든 자식 노드가 제거되고 할당한 문자열에 포함되어 있는 HTML 마크업이 파싱되어 요소 노드의 자식 노드로 DOM에 반영된다.

```html
<!DOCTYPE html>
<html lang="en">
  <body>
    <div id="foo">Hello <span>world!</span></div>
    <script>
      document.getElementById("foo").innerHTML = "Hi <span>there!</span>";
    </script>
  </body>
</html>
```

- 이처럼 innerHTML 프로퍼티를 사용하면 HTML 마크업 문자열로 간단히 DOM 조작이 가능하다.
- 요소 노드의 innerHTML 프로퍼티에 할당한 HTML 마크업 문자열은 렌더링 엔진에 의해 파싱되어 요소 노드의 자식으로 DOM에 반영된다.
- 이때 사용자로부터 입력받은 데이터를 그대로 innerHTML 프로퍼티에 할당하는 것은 XSS에 취약하므로 위험하다.

```html
<!DOCTYPE html>
<html lang="en">
  <body>
    <div id="foo">Hello</div>
    <script>
      // 에러 이벤트를 강제로 발생시켜서 자바스크립트 코드가 실행되도록 한다.
      document.getElementById(
        "foo"
      ).innerHTML = `<img src="x" onerror="alert(document.cookie)">`;
    </script>
  </body>
</html>
```

- innterHTML 프로퍼티의 또 다른 단점은 요소 노드의 interHTML 프로퍼티에 HTML 마크업 문자열을 할당하는 경우 요소 노드의 모든 자식 노드를 제거하고 할당한 HTML 마크업 문자열을 파싱하여 DOM을 변경시키는 것이다.

```html
<!DOCTYPE html>
<html lang="en">
  <body>
    <ul id="fruits">
      <li class="apple">Apple</li>
    </ul>
  </body>
  <script>
    const $fruits = document.getElementById("fruits");

    // 노드 추가
    $fruits.innerHTML += "<li class="banana">Banana</li>";
  </script>
</html>
```

- #fruits 요소에 자식 요소 li.banana를 추가한다. 이떄 #fruits 요소의 자식 요소 li.aplle은 아무런 변경이 없으므로 다시 생성할 필요가 없다.
- 다만 새롭게 추가할 banana 요소 노드만 생성하연 #fruits 요소의 자식 요소로 추가하면 된다.
- 하지만 #fruits 요소의 모든 자식 노드를 제거하고 새롭게 요소 노드 li.apple과 li.banana를 생성하여 #fruits 요소의 자식요소로 추가한다.
- 또 innerHTML은 새로운 요소를 삽입할 때 삽입될 위치를 지정할 수 없다는 단점도 있다.

```html
<ul id="fruits">
  <li class="apple">Apple</li>
  <li class="orange">Orange</li>
</ul>
```

- li.apple 요소와 li.orange 요소 사이에 새로운 요소를 삽입하고 싶은 경우 innerHTML 프로퍼티를 사용하면 삽입 위치를 지정할 수 없다.
- 이처럼 innerHTML 프로퍼티는 복잡하지 않은 요소를 새롭게 추가할 때 유용하지만 기존 요소를 제거하지 않으면서 위치를 지정해 새로운 요소를 삽입해야 할때는 사용하지 않는 것이 좋다.

2. insertAdjacentHTML 메서드

- 기존 요소를 제거하지 않으면서 위치를 지정해 새로운 요소를 삽입한다.

```html
<!DOCTYPE html>
<html lang="en">
  <body>
    <!--beforebegin-->
    <div id="foo">
      <!--afterbegin-->
      text
      <!--beforeend-->
    </div>
    <!--afterend-->
  </body>
  <script>
    const $foo = document.getElementById("foo");

    $foo.insertAdjacentHTML("beforebegin", "<p>beforebigin</p>");
    $foo.insertAdjacentHTML("afterbegin", "<p>afterbigin</p>");
    $foo.insertAdjacentHTML("beforeend", "<p>beforeend</p>");
    $foo.insertAdjacentHTML("afterend", "<p>afterend</p>");
  </script>
</html>
```

- insertAdjacentHTML 메서드는 기존 요소에는 영향을 주지 않고 새롭게 삽입될 요소만을 파싱하여 자식요소로 추가하므로 기존의 자식 노드를 모두 제거하고 다시처음부터 새롭게 자식노드를 생성하여 자식 요소로 추가하는 innerHTML 프로퍼티보다 효율적이고 빠르다
- insertAdjacentHTML 메서드도 또한 XSS 공격에 취약점이 있다.

3. 노드 생성과 추가

- DOM은 노드를 직접 생성/삽인/삭제/치환하는 메서드도 제공한다.

```html
<!DOCTYPE html>
<html lang="en">
  <body>
    <ul id="fruits">
      <li>Apple</li>
    </ul>
  </body>
  <script>
    const $fruits = document.getElementById("fruits");

    // 1. 요소 노드 생성
    const $li = document.createElement("li");

    // 2. 텍스트 노드 생성
    const textNode = document.createTextNode("Banana");

    // 3. 텍스트 노드를 $li 요소 노드의 자식 노드로 추가
    $li.appendChild(textNode);

    // 4. $li 요소 노드를 #fruits 요소 노드의 마지막 자식 노드로 추가
    $fruits.appendChild($li);
  </script>
</html>
```

- 요소 노드 생성

  - Document.prototype.createElement(tagName) 메서드는 요소 노드를 생성하여 반환한다.

  ```js
  // 1. 요소 노드 생성
  const $li = document.createElement("li");
  // 생성된 요소 노드는 아무런 자식 노드가 없다.
  console.log($li.childNodes); // NodeList []
  ```

- 텍스트 노드 생성

  - Document.prototype.createTextNode(text) 메서드는 텍스트 노드를 생성하여 반환한다.

  ```js
  // 2. 텍스트 노드 생성
  const textNode = document.createTextNode("Banana");
  ```

  - createTextNode 메서드는 텍스트 노드를 생성할 뿐 요소 노드에 추가하지 않는다.
  - 이후에 생성된 텍스트 노드를 요소 노드에 추가하는 처리가 별도로 필요하다.

- 텍스트 노드를 요소 노드의 자식 노드로 추가

  - Node.prototype.appendChild(childNode) 메서드는 매개변수 childNode에게 인수로 전달한 노드를 appendChild 메서드를 호출한 노드의 마지막 자식 노드로 추가한다.

  ```js
  // 3. 텍스트 노드를 $li 요소 노드의 자식 노드로 추가
  $li.appendChild(textNode);
  ```

  - 요소 노드에 자식 노드가 하나도 없는 경우에는 텍스트 노드를 생성하여 요소 노드의 자식 노드로 텍스트 노드로 추가하는 것보다 textContet 프로퍼티를 사용하는 편이 더욱 간편하다.

  ```js
  // 텍스트 노드를 생성하여 요소 노드의 자식 노드로 추가
  $li.appendChild(document.createTextnode("Banana"));

  // $li 요소 노드에 자식 노드가 하나도 없는 위 코드와 동일하게 동작
  $li.textContent = "Banana";
  ```

  - 단, 요소 노드에 자식 노드가 있는 경우 요소 노드의 textContet 프로퍼티에 문자열을 할당하면 요소 노드의 모든 자식 노드가 제거되고 할당한 문자열이 텍스트로 추가되므로 주의해야한다.

- 요소 노드를 DOM에 추가

  - Node.prototype.appendChild 메서드를 사용하여 텍스트 노드와 부자 관계로 연결한 요소 노드를 #fruits 요소 노드의 마지막 자식 요소로 추가한다.

  ```js
  // 4. $li 요소 노드를 #fruits 요소 노드의 마지막 자식 노드로 추가
  $fruits.appendChild($li);
  ```

  - 단 하나의 요소 노드를 생성하여 DOM에 한번 추가하므로 DOM은 한 번 변경된다.
  - 리플로우와 리페인트가 실행

4. 복수의 노드 생성과 추가

```html
<!DOCTYPE html>
<html lang="en">
  <body>
    <ul id="fruits"></ul>
  </body>
  <script>
    const $fruits = document.getElementById("fruits");

    ["Apple", "Banana", "Orange"].forEach((text) => {
      // 1. 요소 노드 생성
      const $li = document.createElement("li");

      // 2. 텍스트 노드 생성
      const textNode = document.createTextNode(text);

      // 3. 텍스트 노드를 $li 요소 노드의 자식 노드로 추가
      $li.appendChild(textNode);

      // 4. $li 요소 노드를 #fruits 요소 노드의 마지막 자식 노드로 추가
      $fruits.appendChild($li);
    });
  </script>
</html>
```

- 위 예제와 같이 기존 DOM 요소에 노드를 반복하여 추가하는 것은 비효율적이다.
- 컨테이너 요소를 미리 생성한 다음, DOM에 추가해야할 3개의 요소 노드를 컨테이너 요소에 자식 노드로 추가하고, 컨테이너 #fruits 요소에 자식으로 추가한다면 DOM은 한번만 변경된다.

```html
<!DOCTYPE html>
<html lang="en">
  <body>
    <ul id="fruits"></ul>
  </body>
  <script>
    const $fruits = document.getElementById("fruits");

    // 컨테이너 요소 노드 생성
    const $container = document.createElement("div");

    ["Apple", "Banana", "Orange"].forEach((text) => {
      // 1. 요소 노드 생성
      const $li = document.createElement("li");

      // 2. 텍스트 노드 생성
      const textNode = document.createTextNode(text);

      // 3. 텍스트 노드를 $li 요소 노드의 자식 노드로 추가
      $li.appendChild(textNode);

      // 4. $li 요소 노드를 #container 요소 노드의 마지막 자식 노드로 추가
      $container.appendChild($li);
    });

    // 5. 컨테이너 요소 노드를 #fruits 요소 노드의 마지막 자식 노드로 추가
    $fruits.appendChild($container);
  </script>
</html>
```

- DOM을 한 번만 변경하므로 성능에 유리하기는 하지만 불필요한 컨테이너 요소(div)가 DOM에 추가되는 부작용이 있다.
- 이 문제는 DocumentFragment 노드를 통해 해결할 수 있다.
- DocumentFragment 노드는 자식 노드들의 부모 노드로서 별도의 서브 DOM을 구성하여 기존 DOM에 추가하기 위한 용도로 사용한다.

```html
<!DOCTYPE html>
<html lang="en">
  <body>
    <ul id="fruits"></ul>
  </body>
  <script>
    const $fruits = document.getElementById("fruits");

    // DocumentFragment 노드 생성
    const $fragment = document.createDocumentFragment();

    ["Apple", "Banana", "Orange"].forEach((text) => {
      // 1. 요소 노드 생성
      const $li = document.createElement("li");

      // 2. 텍스트 노드 생성
      const textNode = document.createTextNode(text);

      // 3. 텍스트 노드를 $li 요소 노드의 자식 노드로 추가
      $li.appendChild(textNode);

      // 4. $li 요소 노드를 DocumentFragment 요소 노드의 마지막 자식 노드로 추가
      $fragment.appendChild($li);
    });

    // 5. DocumentFragment 노드를 #fruits 요소 노드의 마지막 자식 노드로 추가
    $fruits.appendChild($fragment);
  </script>
</html>
```

- 실제로 DOM 변경이 발생하는 것은 한번뿐이며 리플로우와 리페인트도 한 번만 실행된다.
- 여러개의 요소 노드를 DOM에 추가하는 경우 DocumentFragment 노드를 사용하는 것이 더 효율적이다.

5. 노드 삽입

- 마지막 노드 추가

  - Node.prototype.appendChild 메서드는 인수로 전달받은 노드를 자신을 호출한 노드의 마지막 자식 노드로 DOM에 추가한다.
  - 노드를 추가할 위치를 지정할 수 없고 언제나 마지막 자식 노드로 추가한다.

  ```html
  <!DOCTYPE html>
  <html lang="en">
    <body>
      <ul id="fruits">
        <li>Apple</li>
        <li>Banana</li>
      </ul>
    </body>
    <script>
      // 요소 노드 생성
      const $li = document.createElement("li");

      // 텍스트 노드를 $li 요소 노드의 마지막 자식 노드로 추가
      $li.appendChild(document.createTextNode("Orange"));

      // $li 요소 노드를 #fruits 요소 노드의 마지막 자식 노드로 추가
      document.getElementById("fruits").appendChild($li);
    </script>
  </html>
  ```

- 지정한 위치에 노드 삽입

  - Node.prototype.insertBefore(newNode, childNode) 메서드는 첫 번째 인수로 전달받은 노드를 두 번째 인수로 전달받은 노드 앞에 삽입한다.

  ```html
  <!DOCTYPE html>
  <html lang="en">
    <body>
      <ul id="fruits">
        <li>Apple</li>
        <li>Banana</li>
      </ul>
    </body>
    <script>
      const $fruits = document.getElementById("fruits");

      // 요소 노드 생성
      const $li = document.createElement("li");

      // 텍스트 노드를 $li 요소 노드의 마지막 자식 노드로 추가
      $li.appendChild(document.createTextNode("Orange"));

      // $li 요소 노드를 #fruits 요소 노드의 마지막 자식 노드로 추가
      $fruits.insertBefore($li, $fruits.lastElementChild);
      // Apple - Orange - Banana
    </script>
  </html>
  ```

  - 두 번째 인수로 전달받은 노드는 반드시 insertBefore 메서드를 호출한 노드의 자식 노드이어야한다.
  - 그렇지 않으면 DOMException이 발생한다.
  - 두 번째 인수로 전달받은 노드가 null이면 첫 번째 인수로 전달받은 노드를 insertBefore 메서드를 호출한 노드의 마지막 자식 노드로 추가된다 -> appendChild와 동일

6. 노드 이동

- DOM에 이미 존재하는 노드를 appendChild 또는 insertBefore 메서드를 사용하여 DOM에 다시 추가하면 현재 위치에서 노드를 제거하고 새로운 위치에 노드를 추가한다 -> 노드가 이동한다.

```html
<!DOCTYPE html>
<html lang="en">
  <body>
    <ul id="fruits">
      <li>Apple</li>
      <li>Banana</li>
      <li>Orange</li>
    </ul>
  </body>
  <script>
    const $fruits = document.getElementById("fruits");

    // 이미 존재하는 요소 노드를 취득
    const [$apple, $banana] = $fruits.children;

    // 이미 존재하는 $apple 요소 노드를 #fruits 요소 노드의 마지막 노드로 이동
    $fruits.appendChild($apple);

    $fruits.insertBefore($banana, $fruits.lastElementChild);
    // Orange - Banana - Apple
  </script>
</html>
```

7. 노드 복사

- Node.prototype.cloneNode([deep: true | false]) 메서드는 노드의 사본을 생성하여 반환한다.
- 매개변수 deep에 true를 인수 전달하면 노드를 깊은 복사하여 모든 자손 노드가 포함된 사본을 생성한다.
- 얕은 복사로 생성된 요소 노드는 자손 노드를 복사하지 않으므로 텍스트 노드도 없다.

```html
<!DOCTYPE html>
<html lang="en">
  <body>
    <ul id="fruits">
      <li>Apple</li>
    </ul>
  </body>
  <script>
    const $fruits = document.getElementById("fruits");
    const $apple = $fruits.firstElementChild;

    // $apple 요소를 얕은 복사하여 사본을 생성. 텍스트 노드가 없는 사본이 생성
    const $shallowClone = $apple.cloneNode();
    // 사본 요소 노드에 텍스트 추가
    $shallowClone.textContent = "Banana";
    // 사본 요소 노드를 #fruits 요소 노드의 마지막 노드로 추가
    $fruits.appendChild($shallowClone);

    // #fruits 요소를 깊은 복사하여 모든 자손 노드가 포함된 사본을 생성
    const $deepClone = $fruits.cloneNode(true);
    // 사본 요소 노드를 #fruits 요소 노드의 마지막 노드로 추가
    $fruits.appendChild($deepClone);
  </script>
</html>
```

8. 노드 교체

- Node.prototype.replaceChild(newChild, oldChild) 메서드는 자신을 호출한 노드의 자식 노드를 다른 노드로 교체한다.
- 첫 번째 매개변수 newChild에는 교체할 새로운 노드를 인수로 전달하고, 두 번쨰 매개변수 oldChild에는 이미 존재하는 교체될 노드를 인수로 전달한다.
- oldChild 매개변수에 인수로 전달한 노드는 replaceChild 메서드를 호출한 노드의 자식 노드이어야한다.
- replaceChild 메서드는 자신을 호출한 노드의 자식 노드인 oldChild 노드를 newChild 노드로 교체한다.

```html
<!DOCTYPE html>
<html lang="en">
  <body>
    <ul id="fruits">
      <li>Apple</li>
    </ul>
  </body>
  <script>
    const $fruits = document.getElementById("fruits");

    // 기존 노드와 교체할 요소 노드를 생성
    const $newChild = document.createElement("li");
    $newChild.textContent = "Banana";

    // #fruits 요소 노드의 첫 번째 요소 노드를 $newChild 요소 노드로 교체
    $fruits.replaceChild($newChild, $fruits.firstElementChild);
  </script>
</html>
```

9. 노드 삭제

- Node.prototype.removeChild(child) 메서드는 child 매개변수에 인수로 전달한 노드를 DOM에서 삭제한다.
- 인수로 전달한 노드는 removeChild 메서드를 호출한 노드의 자식 노드이어야한다.

```html
<!DOCTYPE html>
<html lang="en">
  <body>
    <ul id="fruits">
      <li>Apple</li>
      <li>Banana</li>
    </ul>
  </body>
  <script>
    const $fruits = document.getElementById("fruits");

    // #fruits 요소 노드의 마지막 요소를 삭제
    $fruits.removeChild($fruits.lastElementChild);
  </script>
</html>
```

## 어트리뷰트

1. 어트리뷰트 노드와 attributes 프로퍼티

- HTML 문서의 구성 요소인 HTML 요소는 여러개의 어트리뷰트를 가질 수 있다.
- HTML 요소의 동작을 제어하기 위한 추가적인 정보를 제공하는 HTML 어트리뷰트는 HTML 요소의 시작 태그에 어트리뷰트 이름="어트리뷰트 값" 형식으로 정의한다.
- 요소 노드의 모든 어트리뷰트는 요소 노드의 Element.prototype.attributes 프로퍼티로 취득할 수 있다.
- attributes 프로퍼티는 getter만 존재하는 읽기 전용 접근자 프로퍼티이며, 요소 노드의 모든 어트리뷰트 노드의 참조가 담긴 NamedNodeMap 객체를 반환한다.

```html
<!DOCTYPE html>
<html lang="en">
  <body>
    <input id="user" type="text" value="sunny" />
    <script>
      // 요소 노드의 attribute 프로퍼티는 요소 노드의 모든 어트리뷰트 노드의 참조가 담긴
      // NamedNodeMap 객체를 반환한다.
      const { attributes } = document.getElementById("user");
      console.log(attributes);

      // 어트리뷰트 값 취득
      console.log(attributes.id.value); // user
      console.log(attributes.type.value); // text
      console.log(attributes.value.value); // sunny
    </script>
  </body>
</html>
```

2. HTML 어트리뷰트 조작

- Element.prototype.getAttribute/setAttribute 메서드를 사용하면 attributes 프로퍼티를 통하지 않고 요소 노드에서 메서드를 통해 직접 HTML 어트리뷰트 값을 취득하거나 변경할 수 있어서 편리하다.

```html
<!DOCTYPE html>
<html lang="en">
  <body>
    <input id="user" type="text" value="sunny" />
    <script>
      const $input = document.getElementById("user");

      // value 어트리뷰트 값을 취득
      const inputValue = $input.getAttribute("value");
      console.log(inputValue); // sunny

      // value 어트리뷰트 값을 변경
      $input.setAttribute("value", "foo");
      console.log($input.getAttribute("value")); // foo
    </script>
  </body>
</html>
```

- 특정 HTML 어트리뷰트가 존재하는지 확인하려면 Element.prototype.hasAttribute(attributeName) 메서드를 사용한다.
- 특정 HTML 어트리뷰트를 삭제하려면 Element.prototype.removeAttribute(attributeName) 메서드를 사용한다.

```html
<!DOCTYPE html>
<html lang="en">
  <body>
    <input id="user" type="text" value="sunny" />
    <script>
      const $input = document.getElementById("user");

      // value 어트리뷰트의 존재 확인
      if ($input.hasAttribute("value")) {
        // value 어트리뷰트 삭제
        $input.removeAttribute("value");
      }

      // value 어트리뷰트가 삭제되었다.
      console.log($input.hasAttribute("value")); // false
    </script>
  </body>
</html>
```

3. HTML 어트리뷰트 vs. DOM 프로퍼티

- 요소 노드 객체에는 HTML 어트리뷰트에 대응하는 프로퍼티(DOM 프로퍼티)가 존재한다.
- 이 DOM 프로퍼티들은 HTML 어트리뷰트 값을 초기값으로 가지고 있다.
- DOM 프로퍼티는 setter, getter 모두 존재하는 접근자 프로퍼티다. 참조와 변경이 가능하다.

```html
<!DOCTYPE html>
<html lang="en">
  <body>
    <input id="user" type="text" value="sunny" />
    <script>
      const $input = document.getElementById("user");

      // 요소 노드의 value 프로퍼티 값을 변경
      $input.value = "foo";

      // 요소 노드의 프로퍼티값을 참조
      console.log($input.value); // foo
    </script>
  </body>
</html>
```

- HTML 어트리뷰트는 DOM에서 중복관리되는 것처럼 보이지만 그렇지 않다.
- HTML 어트리뷰트의 역할은 HTML 요소의 초기 상태를 지정하는 것이다. 즉, HTML 어트리뷰트 값은 HTML 요소의 초기 상태를 의미하며 이는 변하지 않는다.

```html
<!DOCTYPE html>
<html lang="en">
  <body>
    <input id="user" type="text" value="sunny" />
    <script>
      const $input = document.getElementById("user");

      // attributes 프로퍼티에 저장된 value 어트리뷰트 값
      console.log($input.getAttribute("value")); // sunny

      // 요소 노드의 value 프로퍼티에 저장된 value 어트리뷰트 값
      console.log($input.value); // sunny
    </script>
  </body>
</html>
```

- 현재는 동일하지만 첫 렌더링 이후 사용자가 input 요소에 무언가를 입력하기 시작하면 상황이 달라진다.
- 요소 노드는 상태를 가지고 있다.
- 사용자가 input 요소의 입력 필드에 "foo"라는 값을 입력한 경우, 최신 상태("foo")를 관리해햐는 것은 물론, HTML 어트리뷰트로 지정한 초기상태("sunny")도 관리해야한다.
- 요소 노드는 2개의 상태, 즉 초기 상태와 최신 상태를 관리해야한다. 요소 노드의 초기 상태는 어트리뷰트 노드가 관리하며, 요소 노드의 최신 상태는 DOM 프로퍼티가 관리한다.

- 어트리뷰트 노드

  - HTML 어트리뷰트로 지정한 HTML 요소의 초기 상태는 어트리뷰트 노드에서 관리한다.
  - HTML 요소에 지정한 어트리뷰트 값은 사용자의 입력에 의해 변하지 않으므로 결과는 언제나 동일하다.

  ```js
  // attributes 프로퍼티에 지정된 value 어트리뷰트 값을 취득한다. 결과는 언제나 동일
  document.getElementById("user").getAttribute("value"); // sunny
  ```

  - setAttribute 메서드는 어트리뷰트 노드에서 관리하는 HTML 요소에 지정한 어트리뷰트 값, 즉 초기 상태 값을 변경한다.

  ```html
  <!DOCTYPE html>
  <html lang="en">
    <body>
      <input id="user" type="text" value="sunny" />
      <script>
        document.getElementById("user").setAttribute("value", "foo");
      </script>
    </body>
  </html>
  ```

- DOM 프로퍼티

  - 사용자가 입력한 최신 상태는 HTML 어트리뷰트에 대응하는 요소 노드의 DOM 프로퍼티가 관리한다.
  - DOM 프로퍼티는 사용자의 입력에 의한 상태 변화에 반응하여 언제나 최신 상태를 유지한다.

  ```html
  <!DOCTYPE html>
  <html lang="en">
    <body>
      <input id="user" type="text" value="sunny" />
      <script>
        document.getElementById("user").setAttribute("value", "foo");
      </script>
    </body>
  </html>
  ```

  - DOM 프로퍼티에 값을 할당하는 것은 HTML 요소의 최신 상태 값을 변경하는 것을 의미한다.
  - 즉, 사용자가 상태를 변경하는 행우와 같다. HTML 요소에 지정한 어트리뷰트 값에는 어떠한 영향도 주지 않는다.

  ```html
  <!DOCTYPE html>
  <html lang="en">
    <body>
      <input id="user" type="text" value="sunny" />
      <script>
        const $input = document.getElementById("user");

        // DOM 프로퍼티에 값을 할당하여 HTML 요소의 최신 상태를 변경한다.
        $input.value = "foo";
        console.log($input.value);

        // getAttribute 메서드로 취득한 HTML 어트리뷰트 값, 초기 상태 값은 변하지 않고 유지된다.
        console.log("value 어트리뷰트 값", $input.getAttribute("value"));
      </script>
    </body>
  </html>
  ```

  - 사용자 입력에 의한 상태 변화와 관계없는 id 어트리뷰트와 id 프로퍼티는 사용자 입력과 관계없이 항상 동일한 값을 유지한다.
  - id 어트리뷰트 값이 변하면 id 프로퍼티 값도 변하고 그 반대도 마찬가지다.

  ```html
  <!DOCTYPE html>
  <html lang="en">
    <body>
      <input id="user" type="text" value="sunny" />
      <script>
        const $input = document.getElementById("user");

        // id 프로퍼티와 id 프로퍼티는 사용자 입력과 관계없이 항상 동일한 값으로 연동한다.=
        $input.id = "foo";

        console.log($input.id); // foo
        console.log($input.getAttribute("id")); // foo
      </script>
    </body>
  </html>
  ```

- HTML 어트리뷰트와 DOM 프로퍼티의 대응관계

  - id 어트리뷰터와 id 프로퍼티는 1대1 대응하며, 동일한 값으로 연동한다.
  - input 요소의 value 어트리뷰트는 value 프로퍼티와 1대1 대응한다. 하지만 value 어트리뷰트는 초기 상태를 value 프로퍼티는 최신 상태를 갖는다.
  - class 어트리뷰트는 className, classList 프로퍼티와 대응한다.
  - for 어트리뷰트는 htmlFor 프로퍼티와 1대1 대응한다.
  - td 요소의 colspan 어트리뷰트는 대응하는 프로퍼티가 존대하지 않는다.
  - textContent 프로퍼티는 대응하는 어트리뷰트가 존재하지 않는다.
  - 어트리뷰트의 이름은 대소문자를 구별하지 않지만 대응하는 프로퍼티 키는 카멜 케이스를 따른다.

- DOM 프로퍼티 값의 타입

  - getAttribue 메서드로 취득한 어트리뷰트 값은 언제나 문자열이다. 하지만 DOM 프로퍼티로 취득한 최신 상태 값은 문자열이 아닐 수 도 있다.

  ```html
  <!DOCTYPE html>
  <html lang="en">
    <body>
      <input type="checkbox" checked />
      <script>
        const $checkbox = document.querySelector("input[type=checkbox]");

        // getAttribute 메서드로 취득한 어트리뷰트 값은 언제나 문자열이다.
        console.log($checkbox.getAttribute("checked")); // ''

        // DOM 프로퍼티로 취득한 최신 상태 값은 문자열이 아닐 수도 있다.
        console.log($checkbox.checked); // true
      </script>
    </body>
  </html>
  ```

4. data 어트리뷰트와 dataset 프로퍼티

- data 어트리뷰트와 dataset 프로퍼티를 사용하면 HTML 요소에 정의한 사용자 정의 어트리뷰트와 자바스크립트 간에 데이터를 교환할 수 있다.
- data 어트리뷰트는 data-user-id, data-role와 같이 data- 접두사 다음에 임의의 이름을 붙여 사용한다.

```html
<!DOCTYPE html>
<html lang="en">
  <body>
    <ul class="users">
      <li id="1" data-user-id="1234" data-role="admin">Son</li>
      <li id="2" data-user-id="5678" data-role="subscriber">Kim</li>
    </ul>
  </body>
</html>
```

- data 어트리뷰트의 값은 HTMLElement.dataset 프로퍼티로 취득할 수 있다.
- dataset 프로퍼티는 HTML 요소의 모든 data 어트리뷰트의 정보를 제공하는 DOMStringMap 객체를 반환한다. DOMStringMap 객체는 data 어트리뷰트의 data- 접두사 다음에 붙인 임의이 이름을 카멜 케이스로 변환한 프로퍼티를 가지고 있다.

```html
<!DOCTYPE html>
<html lang="en">
  <body>
    <ul class="users">
      <li id="1" data-user-id="1234" data-role="admin">Son</li>
      <li id="2" data-user-id="5678" data-role="subscriber">Kim</li>
    </ul>
    <script>
      const users = [...document.querySelector(".users").children];

      // user-id 가 '1234'인 요소 노드를 취득한다.
      const user = users.find((user) => user.dataset.userId === "1234");
      // user-id가 '1234' 인 요소 노드에서 data-role의 값을 취득한다.
      console.log(user.dataset.role);

      // user-id가 '1234'인 요소 노드의 data-role 값을 변경한다.
      user.dataset.role = "subscriber";
      // dataset 프로퍼티는 DOMStringMap 객체를 반환한다.
      console.log(user.dataset);
      // DOMStringMap {userId: "1234", role: "subscriber"}
    </script>
  </body>
</html>
```

- data 어트리뷰트의 data- 접두사 다음에 존재하지 않는 이름을 키로 사용하여 dataset 프로퍼티에 값을 할당하면 HTML 요소에 data 어트리뷰트가 추가된다.
- dataset 프로퍼티에 추가한 카멜케이스(fooBar)의 프로퍼티 키는 data 어트리뷰트의 data- 접두사 다음에 케밥케이스(data-foo-bar)로 자동 변경되어 추가된다.

```html
<!DOCTYPE html>
<html lang="en">
  <body>
    <ul class="users">
      <li id="1" data-user-id="1234" data-role="admin">Son</li>
      <li id="2" data-user-id="5678" data-role="subscriber">Kim</li>
    </ul>
    <script>
      const users = [...document.querySelector(".users").children];

      // user-id 가 '1234'인 요소 노드를 취득한다.
      const user = users.find((user) => user.dataset.userId === "1234");

      // user-id가 '1234'인 요소 노드에 새로운 data 어트리뷰트를 추가한다.
      user.dataset.fooBar = "abc";
      console.log(user.dataset);
      // DOMStringMap {userId: "1234", role: "subscriber", fooBar: "abc"}
    </script>
  </body>
</html>
```

## 스타일

1. 인라인 스타일 조작

- HTMLElemnet.prototype.style 프로퍼티는 setter, getter 모두 존재하는 접근자 프로퍼티로서 요소 노드의 인라인 스타일을 취득하거나 추가 또는 변경한다.

```html
<!DOCTYPE html>
<html lang="en">
  <body>
    <div style="color: red">Hello World</div>
    <script>
      const $div = document.querySelector("div");

      // 인라인 스타일 취득
      console.log($div.style);

      // 인라인 스타일 변경
      $div.style.color = "blue";

      // 인라인 스타일 추가
      $div.style.width = "100px";
      $div.style.height = "100px";
      $div.style.backgroundColor = "yellow";
    </script>
  </body>
</html>
```

- style 프로퍼티를 참조하려면 CSSStyleDeclaration 타입 객체를 반환한다.
- CSSStyleDeclaration 객체는 다양한 CSS 프로퍼티에 대응하는 프로퍼티를 가지고 있으며, 이 프로퍼티에 값을 할당하면 CSS 프로퍼티가 인라인 스타일로 HTML 요소에 추가되거나 변경된다.
- CSS 프로퍼티는 케밥 케이스를 따른다. 이에 대응하는 CSSStyleDeclaration 객체의 프로퍼티는 카멜 케이스를 따른다.

```js
$div.style.backgroundColor = "yellow";
// 케밥 케이스의 CSS 프로퍼티를 그대로 사용하려면
$div.style.["background-color"] = "yellow";
```

2. 클래스 조작

- .으로 시작하는 클래스 선택자를 사용하여 CSS class를 미리 정의한 다음, HTML 요소의 class 어트리뷰트의 값을 변경하여 HTML 요소의 스타일을 변경할 수도 있따.
- 단, class 어트리뷰트에 대응하는 DOM 프로퍼티는 class가 아니라 className과 classList다.

- ClassName

  - Element.prototype.className 프로퍼티는 setter, getter 모두 존재하는 접근자 프로퍼티로서 HTML 요소의 class 어트리뷰트 값을 취득하거나 변경한다.

  ```html
  <!DOCTYPE html>
  <html lang="en">
    <head>
      <style>
        .box {
          width: 100px;
          height: 100px;
          background-color: antiquewhite;
        }
        .red {
          color: red;
        }
        .blue {
          color: blue;
        }
      </style>
    </head>
    <body>
      <div class="box red">Hello World</div>
      <script>
        const $box = document.querySelector(".box");

        // .box 요소의 class 어트리뷰트 값을 취득
        console.log($box.className); // "box red"

        // .box 요소의 class 어트리뷰트 값 중에서 "red"만 "blue" 로 변경
        $box.className = $box.className.replace("red", "blue");
      </script>
    </body>
  </html>
  ```

  - className 프로퍼티는 문자열을 반환하므로 공백으로 구분된 여러개의 클래스를 반환하는 경우 다루기가 불편하다.

- classList

  - Element.prototype.classList 프로퍼티는 class 어트리뷰트의 정보를 담은 DOMTokenList 객체를 반환한다.

  ```html
  <!DOCTYPE html>
  <html lang="en">
    <head>
      <style>
        .box {
          width: 100px;
          height: 100px;
          background-color: antiquewhite;
        }
        .red {
          color: red;
        }
        .blue {
          color: blue;
        }
      </style>
    </head>
    <body>
      <div class="box red">Hello World</div>
      <script>
        const $box = document.querySelector(".box");

        // .box 요소의 class 어트리뷰트 정보를 담은 DOMTokenList 객체를 취득
        console.log($box.classList);
        // DOMTokenList(2) [length: 2, value: "box blue", 0: "box", 1: "blue"]

        // .box 요소의 class 어트리뷰트 값 중에서 "red"만 "blue" 로 변경
        $box.classList.replace("red", "blue");
      </script>
    </body>
  </html>
  ```

  - DOMTokenList 객체는 class 어트리뷰트의 정보를 나타내는 컬렉션 객체로서 유사 배열이면서 이터러블이다.
  - DOMTokenList 객체는 다음과 같은 메서드를 제공한다.

  ```js
  // add(...className)
  // 인수로 전달한 1개 이상의 문자열을 class 어트리뷰트 값으로 추가한다.
  $box.classList.add("foo"); // class="box red foo"

  // remove(...className)
  // 인수로 전달한 1개 이상의 문자열과 일치하는 클래스를 class 어트리뷰트에서 삭제한다.
  $box.classList.remove("foo"); // class="box red"

  // item(index)
  // 인수로 전달한 index에 해당하는 클래스를 class 어트리뷰트에서 반환한다.
  $box.classList.item(0); // "box"
  $box.classList.item(1); // "red"

  // contains(className)
  // 인수로 전달한 문자열과 일치하는 클래스가 class 어트리뷰트에 포함되어 있는지 확인한다.
  $box.classList.contains("box"); // true
  $box.classList.contains("blue"); // false

  // replace(oldClassName, newClassName)
  // 첫 번째 인수로 전달한 문자열을 두 번째 인수로 전달한 문자열로 변경한다.
  $box.classList.replace("red", "blue"); // class="box blue"

  // toggle(className[force])
  // 인수로 전달한 문자열과 일치하는 클래스가 존재하면 제거하고, 존재하지 않으면 추가한다.
  $box.classList.toggle("foo"); // class="box red foo"
  $box.classList.toggle("foo"); // class="box red"

  // 두 번째 인수로 불리언 값으로 평가되는 조건식을 전달할 수 있다.
  // true 면 class 어트리뷰트에 강제로 첫 번째 인수로 전달받은 문자열을 추가
  // false 면 class 어트리뷰트에 강제로 첫 번째 인수로 전달받은 문자열을 제거한다.
  $box.classList.toggle("foo", true); // class="box red foo"
  $box.classList.toggle("foo", false); // class="box red"
  ```

3. 요소에 적용되어 있는 CSS 스타일 참조

- style 프로퍼티는 인라인 스타일만 반환한다.
- HTML 요소에 적용되어 있는 모든 CSS 스타일을 참조해야 할 경우 getComputedStyle 메서드를 사용한다.

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <style>
      .box {
        width: 100px;
        height: 100px;
        background-color: cornsilk;
        border: 1px solid black;
      }
      .red {
        color: red;
      }
    </style>
  </head>
  <body>
    <div class="box red">Hello World</div>
    <script>
      const $box = document.querySelector(".box");

      // .box 요소에 적용된 모든 CSS 스타일을 담고 있는 CSSStyleDeclaration 객체를 취득
      const computedStyle = window.getComputedStyle($box);
      console.log(computedStyle); // CSSStyleDeclaration

      // 임베딩 스타일
      console.log(computedStyle.width); // 100px
      console.log(computedStyle.height); // 100px
      console.log(computedStyle.backgroundColor); // rgb(255, 248, 220)
      console.log(computedStyle.border); // 1px solid rgb(0, 0, 0)

      // 상속 스타일(body -> .box)
      console.log(computedStyle.color); // rgb(255, 0, 0)

      // 기본 스타일
      console.log(computedStyle.display); // block
    </script>
  </body>
</html>
```

- getComputedStyle 메서드의 두 번째 인수(pseudo)로 :after, :before 와 같은 의사 요소를 지정하는 문자열을 전달할 수 있다. 의사 요소가 아닌 일반 요소의 경우 두 번째 인수는 생략한다.

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <style>
      .box:before {
        content: "Hello";
      }
    </style>
  </head>
  <body>
    <div class="box">Box</div>
    <script>
      const $box = document.querySelector(".box");

      // 의사 요소 :before의 스타일을 취득한다.
      const computedStyle = window.getComputedStyle($box, ":before");
      console.log(computedStyle.content); // "Hello"
    </script>
  </body>
</html>
```

## DOM 표준

# DOM 관련 프로퍼티, 메서드 정리

## 요소 노드 취득

- Document.prototype.getElementById
- Document.prototype/Element.prototype.getElementsByTagName(HTMLCollection)
- Doclument.prototype/Element.prototype.getElementsByClassName(HTMLCollection)
- Document.prototype/Element.prototype.querySelector
- Document.prototype/Element.prototype.querySelectorAll(NodeList)

## 노드 탐색

- Node.prototype.childNodes(텍스트 노드 포함)
- Elemnet.prototype.children(텍스트 노드 미포함)
- Node.prototype.firstChild(텍스트 노드 포함)
- Element.firstElementChild(텍스트 노드 미포함)
- Node.prototype.lastChild(텍스트 노드 포함)
- Element.lastElementChild(텍스트 노드 미포함)
- Node.prototype.hasChildNodes(텍스트 노드 포함)
- Node.prototype.parentNode
- Node.prototype.nextSibling
- Node.prototype.previousSibling
- Element.prototype.nextElementSibling
- Element.prototype.previousElementSibling

## 노드 정보 취득

- Node.prototype.nodeType
- Node.prototype.nodeName

## 요소 노드의 텍스트 조작

- Node.prototype.nodeValue
- Node.prototype.textContent

## DOM 조작

- Element.prototype.innerHTML
- Element.prototype.insertAdjacentHTML(position, DOMString)
- Document.prototype.createElement(tagName)
- Document.prototype.createTextNode(text)
- Node.prototype.appendChild(childNode)
- Document.prototype.createDocumentFragment()
- Node.prototype.insertBefore(newNode, childNode)
- Node.prototype.cloneNode([deep: true | false])
- Node.prototype.replaceChild(newChild, oldChild)
- Node.prototype.removeChild(child)

## 어트리뷰트

- Element.prototype.attributes
- Element.prototype.getAttribute/setAttribute
- Element.prototype.hasAttribute(attributeName)
- Element.prototype.removeAttribute(attributeName)

## 인라인 스타일, 클래스 조작

- HTMLElemnet.prototype.style
- Element.prototype.className/classList
