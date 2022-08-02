# javascript-study

# 40장 이벤트

### 40.1 이벤트 드리븐 프로그래밍

- 브라우저는 처리해야 할 특정 사건이 발생하면 이를 감지하여 이벤트를 발생시킨다. 예를 들어 클릭, 키보드 입력, 마우스 이동이 있다.

- 애플리케이션이 특정 타입의 이벤트에 대해 반응하여 어떤 일을 하고 싶다면 해당하는 타입의 이벤트가 발생했을 때 호출될 함수를 브라우저에 알려 호출을 위임한다.
  - `이벤트 헨들러` : 이벤트가 발생했을 때 호출될 함수
  - `이벤트 헨들러 등록` : 이벤트가 발생했을 때 브라우저에게 이벤트 핸들러의 호출을 위임하는 것

```js
[40-01]

<!DOCTYPE html>
<html>
<body>
  <button>Click me!</button>
  <script>
    const $button = document.querySelector('button');

    // 사용자가 버튼을 클릭하면 함수를 호출하도록 요청
    $button.onclick = () => { alert('button click'); };
  </script>
</body>
</html>

```

이처럼 이벤트와 그에 대응하는 함수를 통해 사용자와 애플리케이션은 상호작용을 할 수 있다.

이와 같이 프로그램의 흐름을 이벤트 중심으로 제어하는 프로그래밍 방식을 `이벤트 드리븐 프로그래밍`이라 한다.

---

### 40.2 이벤트 타입

- 이벤트 타입은 이벤트의 종류를 나타내는 문자열이다. 약 200여가지가 있다.

- 이벤트 타입에 대한 상세목록은 아래 링크에서 확인할 수 있다.

- MDN [Event reference](https://developer.mozilla.org/ko/docs/Web/Events)

<br>

_(자주 쓰이는 이벤트만 적어놓았습니다.)_

#### 40.2.1 마우스 이벤트

| 이벤트 타입 | 이벤트발생시점                                                  |
| :---------- | :-------------------------------------------------------------- |
| click       | 마우스 버튼을 클릭했을 때                                       |
| dbclick     | 마우스 버튼을 더블 클랙했을 때                                  |
| mousedown   | 마우스 버튼을 눌렀을 때                                         |
| mouseup     | 누르고 있던 마우스 버튼을 놓았을 때                             |
| mousemove   | 마우스 커서를 움직였을 때                                       |
| mouseenter  | 마우스 커서를 HTML요소 안으로 이동했을 때 (버블링 되지 않는다)  |
| mouseover   | 마우스 커서를 HTML 요소 안으로 이동했을 때 (버블링된다)         |
| mouseleave  | 마우스 커서를 HTML 요소 밖으로 이동했을 때 (버블링 되지 않는다) |
| mouseout    | 마우스 커서를 HTML 요소 밖으로 이동했을 때 (버블링된다)         |

<br>

#### 40.2.2 키보드 이벤트

| 이벤트타입 | 이벤트 발생 시점                                                        |
| :--------- | :---------------------------------------------------------------------- |
| keydown    | 모든 키를 눌렀을 때 발생한다.                                           |
| keypress   | 문자 키를 눌렀을 때 연속적으로 발생한다. (문자, 숫자, 특수 문자, enter) |
| keyup      | 누르고 있던 키를 놓았을 때 한 번만 발생한다.                            |

---

### 40.3 이벤트 헨들러 등록

- 이벤트 핸들러는 이벤트가 발생하면 브라우저에 의해 호출될 함수다.

- 이벤트 핸들러를 등록하는 방법은 3가지다.

<br>

#### 40.3.1 이벤트 헨들러 어트리뷰트 방식

- 이벤트 핸들러 어트리뷰트의 이름은 onclick과 같이 on 접두사와 이벤트의 종류를 나타내는 이벤트 타입으로 이루어져 있다.

- 이벤트 핸들러 어트리뷰트 값으로 함수 호출문 등의 문을 할당하면 이벤트 핸들러가 등록된다.
- 주의할 점은 이벤트 핸들러 어트리뷰트 값으로 함수 참조가 아닌, 함수 호출문 등의 문을 할당한다는 것이다.

```js
[40-02]

<!DOCTYPE html>
<html>
<body>
  <button onclick="sayHi('Lee')">Click me!</button>
  <script>
    function sayHi(name) {
      console.log(`Hi! ${name}.`);
    }
  </script>
</body>
</html>
```

<br>

- CBD(Component Based Development) 방식의 Angular, React, Vue, Svelt 같은 프레임워크/ 라이브러리에서는 핸들러 어트리뷰트 방식으로 이벤트를 처리한다.

```js
[40-06]

<!-- Angular -->
<button (click)="handleClick($event)">Save</button>

{ /* React */ }
<button onClick={handleClick}>Save</button>

<!-- Svelte -->
<button on:click={handleClick}>Save</button>

<!-- Vue.js -->
<button v-on:click="handleClick($event)">Save</button>
```

<br>

#### 40.3.2 이벤트 헨들러 프로퍼티 방식

- window 객체와 Document, HTMLElement 타입의 DOM 노드 객체는 이벤트에 대응하는 `이벤트 핸들러 프로퍼티`를 가지고 있다.

- 이벤트 헨들러 프로퍼티의 키는 이벤트 헨들러 어트리뷰트와 마찬가지로 onclick과 같이 on 접두사와 이벤트의 종류를 나타내는 이벤트 타입으로 이루어져 있다.
- 이벤트 핸들러 프로퍼티에 함수를 바인딩하면 이벤트 핸들러가 등록된다.

```js
<!DOCTYPE html>
<html>
<body>
  <button>Click me!</button>
  <script>
    const $button = document.querySelector('button');

    // 이벤트 핸들러 프로퍼티에 이벤트 핸들러를 바인딩
    $button.onclick = function () {
      console.log('button click');
    };
  </script>
</body>
</html>
```

<br>

이벤트 핸들러를 등록하기 위해서는 이벤트를 발생시킬 객체인 `이벤트 타겟` 과 이벤트의 종류를 나타내는 문자열인 `이벤트 타입` 그리고 `이벤트 핸들러`를 지정할 필요가 있다.

![](https://velog.velcdn.com/images/hjthgus777/post/0c111bad-1eef-4369-852a-9a0e98628e30/image.jpeg)

- 이벤트 핸들러는 대부분 이벤트를 발생시킬 `이벤트 타킷`에 바인딩한다.

- 하지만 반드시 이벤트 타킷에 이벤트 핸들러를 바인딩해야하는 것은 아니다.
- 이벤트 핸들러는 이벤트 타킷 또는 전파된 이벤트를 캐치할 `DOM 노드 객체`에 바인딩한다.
- `이벤트 핸들러 어트리뷰트 방식`도 결국 DOM 노드 객체의 이벤트 객체의 `이벤트 핸들러 프로퍼티`로 변환되므로 결과적으로 동일하다
- `이벤트 핸들러 프로퍼티 방식`은 `이벤트 핸들러 어트리뷰트 방식`의 HTML과 자바스크립트가 뒤섞이는 문제를 해결 할 수 있다.
- 하지만 `이벤트 핸들러 프로퍼티`에 하나의 이벤트 핸들러만 바인딩할 수 있다는 단점이 있다.

```js
[40-08]

<!DOCTYPE html>
<html>
<body>
  <button>Click me!</button>
  <script>
    const $button = document.querySelector('button');

    // 이벤트 핸들러 프로퍼티 방식은 하나의 이벤트에 하나의 이벤트 핸들러만을 바인딩할 수 있다.
    // 첫 번째로 바인딩된 이벤트 핸들러는 두 번째 바인딩된 이벤트 핸들러에 의해 재할당되어
    // 실행되지 않는다.
    $button.onclick = function () {
      console.log('Button clicked 1');
    };

    // 두 번째로 바인딩된 이벤트 핸들러
    $button.onclick = function () {
      console.log('Button clicked 2');
    };
  </script>
</body>
</html>
```

<br>

#### 40.3.3 addEventListner 방식

- DOM Level 2에서 도입된 `EventTarget.prototype.addEventListener` 메서드를 사용하여 이벤트 핸들러를 등록할 수 있다.
  ![](https://velog.velcdn.com/images/hjthgus777/post/d9e705f6-7e0c-43f3-b7bd-f95c869dfe35/image.jpeg)

```js
[40-09]

<!DOCTYPE html>
<html>
<body>
  <button>Click me!</button>
  <script>
    const $button = document.querySelector('button');

    // 이벤트 핸들러 프로퍼티 방식
    // $button.onclick = function () {
    //   console.log('button click');
    // };

    // addEventListener 메서드 방식
    $button.addEventListener('click', function () {
      console.log('button click');
    });
  </script>
</body>
</html>
```

<br>

- 이벤트 핸들러 프로퍼티 방식은 이벤트 핸들러 프로퍼티에 이벤트 핸들러를 바인딩하지만 addEventListener 메서드에는 이벤트 핸들러를 인수로 전달한다.

```js
[40-10]

<!DOCTYPE html>
<html>
<body>
  <button>Click me!</button>
  <script>
    const $button = document.querySelector('button');

    // 이벤트 핸들러 프로퍼티 방식
    $button.onclick = function () {
      console.log('[이벤트 핸들러 프로퍼티 방식]button click');
    };

    // addEventListener 메서드 방식
    $button.addEventListener('click', function () {
      console.log('[addEventListener 메서드 방식]button click');
    });
  </script>
</body>
</html>
```

<br>

- addEventListener 메서드 방식은 이벤트 핸들러 프로퍼티에 바인딩된 이벤트 핸들러에 아무런 영향을 주지 않는다. 따라서 버튼 요소에서 클릭 이벤트가 발생하면 2개의 이벤트 핸들러가 모두 호출된다.

- 동일한 HTML 요소에 addEventListener를 이용하여 하나 이상의 이벤트 핸들러를 등록할 수 있다. 이때 이벤트 핸들러는 등록된 순서대로 호출된다.

```js
[40-11]

<!DOCTYPE html>
<html>
<body>
  <button>Click me!</button>
  <script>
    const $button = document.querySelector('button');

    // addEventListener 메서드는 동일한 요소에서 발생한 동일한 이벤트에 대해
    // 하나 이상의 이벤트 핸들러를 등록할 수 있다.
    $button.addEventListener('click', function () {
      console.log('[1]button click');
    });

    $button.addEventListener('click', function () {
      console.log('[2]button click');
    });
  </script>
</body>
</html>
```

<br>

- 단, addEventListener 메서드를 통해 참조가 동일한 이벤트 핸들러를 중복 등록하면 하나의 이벤트 핸들러만 등록된다.

```js
[40-12]

단, addEventListener 메서드를 통해 참조가 동일한 이벤트 핸들러를 중복 등록하면 하나의 이벤트 핸들러만 등록된다.
<!DOCTYPE html>
<html>
<body>
  <button>Click me!</button>
  <script>
    const $button = document.querySelector('button');

    const handleClick = () => console.log('button click');

    // 참조가 동일한 이벤트 핸들러를 중복 등록하면 하나의 핸들러만 등록된다.
    $button.addEventListener('click', handleClick);
    $button.addEventListener('click', handleClick);
  </script>
</body>
</html>
```

---

### 40.4 이벤트 헨들러 제거

- addEventListener로 등록한 이벤트 핸들러를 제거하려면 EventTarget.prototype.removeEventListener 메서드를 사용한다.

- 단, addEvenetListener 메서드에 전달한 인수와 removeEventListener 메서드에 전달한 인수가 일치하지 않으면 이벤트 핸들러가 제거되지 않는다.

```js
[40-13]

<!DOCTYPE html>
<html>
<body>
  <button>Click me!</button>
  <script>
    const $button = document.querySelector('button');

    const handleClick = () => console.log('button click');

    // 이벤트 핸들러 등록
    $button.addEventListener('click', handleClick);

    // 이벤트 핸들러 제거
    // addEventListener 메서드에 전달한 인수와 removeEventListener 메서드에
    // 전달한 인수가 일치하지 않으면 이벤트 핸들러가 제거되지 않는다.
    $button.removeEventListener('click', handleClick, true); // 실패
    $button.removeEventListener('click', handleClick); // 성공
  </script>
</body>
</html>
```

<br>

- 이벤트 핸들러 프로퍼티 방식으로 등록한 이벤트 핸들러는 removeEventListener로 제거할 수 없다. 이벤트 핸들러 프로퍼티 방식으로 등록한 이벤트 핸들러를 제거하려면 이벤트 핸들러 프로퍼티에 null을 할당한다.

```js
[40-17]

<!DOCTYPE html>
<html>
<body>
  <button>Click me!</button>
  <script>
    const $button = document.querySelector('button');

    const handleClick = () => console.log('button click');

    // 이벤트 핸들러 프로퍼티 방식으로 이벤트 핸들러 등록
    $button.onclick = handleClick;

    // removeEventListener 메서드로 이벤트 핸들러를 제거할 수 없다.
    $button.removeEventListener('click', handleClick);

    // 이벤트 핸들러 프로퍼티에 null을 할당하여 이벤트 핸들러를 제거한다.
    $button.onclick = null;
  </script>
</body>
</html>
```

---

### 40.5 이벤트 객체

- 이벤트가 발생하면 이벤트에 관련한 다양한 정보를 담고 있는 이벤트 객체가 동적으로 생성된다.

- 생성된 이벤트 객체는 이벤트 핸들러의 첫 번째 인수로 전달된다.

```js
[40-18]

<!DOCTYPE html>
<html>
<body>
  <p>클릭하세요. 클릭한 곳의 좌표가 표시됩니다.</p>
  <em class="message"></em>
  <script>
    const $msg = document.querySelector('.message');

    // 클릭 이벤트에 의해 생성된 이벤트 객체는 이벤트 핸들러의 첫 번째 인수로 전달된다.
    function showCoords(e) {
      $msg.textContent = `clientX: ${e.clientX}, clientY: ${e.clientY}`;
    }

    document.onclick = showCoords;
  </script>
</body>
</html>
```

<br>

- 이벤트 핸들러 어트리뷰트 방식으로 이벤트를 등록했다면 다음과 같이 event를 통해 이벤트 객체를 전달받을 수 있다.

```js
[40-19]

<!DOCTYPE html>
<html>
<head>
  <style>
    html, body { height: 100%; }
  </style>
</head>
<!-- 이벤트 핸들러 어트리뷰트 방식의 경우 event가 아닌 다른 이름으로는 이벤트 객체를
전달받지 못한다. -->
<body onclick="showCoords(event)">
  <p>클릭하세요. 클릭한 곳의 좌표가 표시됩니다.</p>
  <em class="message"></em>
  <script>
    const $msg = document.querySelector('.message');

    // 클릭 이벤트에 의해 생성된 이벤트 객체는 이벤트 핸들러의 첫 번째 인수로 전달된다.
    function showCoords(e) {
      $msg.textContent = `clientX: ${e.clientX}, clientY: ${e.clientY}`;
    }
  </script>
</body>
</html>
```

<br>

#### 40.5.1 이벤트 객체 상속 구조

- 이벤트가 발생하면 이벤트 사앹에 따라 다양한 타입의 이벤트 객체가 생성된다.

- Event, UIEvent, MouseEvent 등은 모두 생성자 함수다. 따라서 다음과 같이 생성자 함수를 호출하여 이벤트 객체를 생성할 수 있다.

```js
[40-21]

<!DOCTYPE html>
<html>
<body>
  <script>
    // Event 생성자 함수를 호출하여 foo 이벤트 타입의 Event 객체를 생성한다.
    let e = new Event('foo');
    console.log(e);
    // Event {isTrusted: false, type: "foo", target: null, ...}
    console.log(e.type); // "foo"
    console.log(e instanceof Event); // true
    console.log(e instanceof Object); // true

    // FocusEvent 생성자 함수를 호출하여 focus 이벤트 타입의 FocusEvent 객체를 생성한다.
    e = new FocusEvent('focus');
    console.log(e);
    // FocusEvent {isTrusted: false, relatedTarget: null, view: null, ...}

    // MouseEvent 생성자 함수를 호출하여 click 이벤트 타입의 MouseEvent 객체를 생성한다.
    e = new MouseEvent('click');
    console.log(e);
    // MouseEvent {isTrusted: false, screenX: 0, screenY: 0, clientX: 0, ... }

    // KeyboardEvent 생성자 함수를 호출하여 keyup 이벤트 타입의 KeyboardEvent 객체를
    // 생성한다.
    e = new KeyboardEvent('keyup');
    console.log(e);
    // KeyboardEvent {isTrusted: false, key: "", code: "", ctrlKey: false, ...}

    // InputEvent 생성자 함수를 호출하여 change 이벤트 타입의 InputEvent 객체를 생성한다.
    e = new InputEvent('change');
    console.log(e);
    // InputEvent {isTrusted: false, data: null, inputType: "", ...}
  </script>
</body>
</html>
```

- 이처럼 이벤트가 발생하면 암묵적으로 생성되는 이벤트 객체도 생성자 함수에 의해 생성된다.

- 그리고 생성된 이벤트 객체는 생성자 함수와 더불어 생성되는 프로토타입으로 구성된 프로토타입 체인의 일원이 된다.

<br>

#### 40.5.2 이벤트 객체의 공통 프로퍼티

- Event 인터페이스의 이벤트 관련 프로퍼티는 모든 이벤트 객체가 상속받는 공통 프로퍼티다. 이벤트 객체의 공통 프로퍼티는 다음과 같다.

| 공통 프로퍼티    | 설명                                                                        | 타입          |
| :--------------- | :-------------------------------------------------------------------------- | :------------ |
| type             | 이벤트 타입                                                                 | string        |
| target           | 이벤트를 발생시킨 DOM요소                                                   | DOM 요소 노드 |
| currentTarget    | 이벤트 헨들러가 바인딩된 DOM요소                                            | DOM 요소 노드 |
| eventPhase       | 이벤트 전파 단계                                                            | number        |
| bubbles          | 이벤트를 버블링으로 전파하는지 여부                                         | boolean       |
| cancelable       | preventDefault 메서드를 호출하여 이벤트의 기본 동작을 취소할 수 있는지 여부 | boolean       |
| defaultPrevented | preventDefault를 호출하여 이벤트를 취소했는지 여부                          | boolean       |
| isTrusted        | 사용자의 행위에 의해 발생한 이벤트인지 여부                                 | boolean       |
| timeStamp        | 이벤트가 발생한 시각(1970/01/01/00:00:0부터 경과한 밀리초(ms))              | number        |

<br>

#### 40.5.3 마우스 정보 취득

- click, dblclick, mousedown, mouseup, mousemove, mouseenter, mouseleave 이벤트가 발생하면 생성되는 MouseEvent 타입의 이벤트 객체는 다음과 같은 고유의 프로퍼티를 갖는다.

  - 마우스 포인터의 좌표를 나타내는 프로퍼티 : screenX/screenY, clientX/clientY, pageX/pageY, offsetX/offsetY

  - 버튼 정보를 나타내는 프로퍼티 : alKey, ctrlKey, shiftKey, button

<br>

#### 40.5.2 키보드 정보 취득

- keydown, keyup, keypress 이벤트가 발생하면 생성되는 KeyboardEvent 타입의 이벤트 객체는 altKey, ctrlKey, shiftKey, metaKey, key, keyCode 같은 고유의 프로퍼티를 갖는다.
