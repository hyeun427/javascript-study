## 41.1 호출 스케줄링

- 일정 시간이 경과된 이후에 함수가 호출되도록 타이머 함수를 사용해 호출 예약을 하는 것을 ```호출 스케줄링```이라고 한다.
- 자바스크립트에는 타이머를 생성하거나 제거하는 함수를 제공한다,
  - 타이머를 생성하는 함수 : ```setTimeout```,```setUnterval```
  - 타이머를 제거하는 함수 : ```clearTimeout```.```clearInterval```
- 타이머 합수는 브라우저 환경, Node.js 환경에서 모두 전역 객체의 메서드로 타이머 함수를 제공한다. 즉, 타이머 함수는 호스트 객체다.

## 41.2 타이머 함수

### 41.2.1 setTimeout / clearTimeout

**setTimeout**
![](https://velog.velcdn.com/images/hyeun427/post/6a6255ea-997e-4700-922f-2c2cbfbb0689/image.png)

![](https://velog.velcdn.com/images/hyeun427/post/5a63ee02-e9f4-45e5-8246-4f95795c99d2/image.png)

- 두 번째 인수로 전달받은 시간으로 단 한 번 동작하는 타이머를 생성한다.
- 타이머가 만료되면 첫 번째 인수로 전달받은 콜백 함수가 호출된다.

```js
// 1초(1000ms) 후 타이머가 만료되면 콜백 함수가 호출된다.
setTimeout(() => console.log('Hi!'), 1000);

// 1초(1000ms) 후 타이머가 만료되면 콜백 함수가 호출된다.
// 이때 콜백 함수에 'Lee'가 인수로 전달된다.
setTimeout(name => console.log(`Hi! ${name}.`), 1000, 'Lee');

// 두 번째 인수(delay)를 생략하면 기본값 0이 지정된다.
setTimeout(() => console.log('Hello!'));
```

**clearTimeout**
- 호출 스케줄링을 취소하는 함수이다.
- ```setTimeout 함수```는 생성된 타이머를 식별할 수 있는 고유한 타이머 id를 반환한다. 이때 브라우저 환경이면 숫자, Node.js 환경이면 객체를 id로 반환한다.
- ```setTimeout 함수```가 반환한 타이머 id를 ```clearTimeout 함수의 인수```로 전달하여 타이머를 취소할 수 있다.

```js
// 1초(1000ms) 후 타이머가 만료되면 콜백 함수가 호출된다.
// setTimeout 함수는 생성된 타이머를 식별할 수 있는 고유한 타이머 id를 반환한다.
const timerId = setTimeout(() => console.log('Hi!'), 1000);

// setTimeout 함수가 반환한 타이머 id를 clearTimeout 함수의 인수로 전달하여 타이머를
// 취소한다. 타이머가 취소되면 setTimeout 함수의 콜백 함수가 실행되지 않는다.
clearTimeout(timerId);
```

### 41.2.2 setInterval / clearInteval

**setInterval**
- 두 번째 인수로 전달받은 시간으로 반복 동작하는 타이머를 생성한다.
- 타이머가 만료될 때마다 첫 번쨰 인수로 전달받은 콜백 함수가 반복 호출되고 타이머가 취소될 때까지 반복한다.


**clearInteval**
- 호출 스케줄링을 취소한느 함수이다.
- ```setInterval 함수```는 생성된 타이머를 식별할 수 있는 고유한 타이머 id를 반환한다. 이때 브라우저 환경이면 숫자, Node.js 환경이면 객체를 id로 반환한다.
- 타이머 id를 ```clearInterval 함수의 인수```로 전달하여 타이머를 취소할 수 있다.

```js
let count = 1;

// 1초(1000ms) 후 타이머가 만료될 때마다 콜백 함수가 호출된다.
// setInterval 함수는 생성된 타이머를 식별할 수 있는 고유한 타이머 id를 반환한다.
const timeoutId = setInterval(() => {
  console.log(count); // 1 2 3 4 5
  // count가 5이면 setInterval 함수가 반환한 타이머 id를 clearInterval 함수의
  // 인수로 전달하여 타이머를 취소한다. 타이머가 취소되면 setInterval 함수의 콜백 함수가
  // 실행되지 않는다.
  if (count++ === 5) clearInterval(timeoutId);
}, 1000);
```

## 41.3 디바운스와 스로틀

- 디바운스와 스로틀은 짧은 시간 간격으로 연속해서 발생하는 이벤트를 그룹화해서 마지막에 한 번만 이벤트 핸들러가 호출되도록 한다.
  - 짧은 시간 간격으로 연속해서 발생하는 이벤트 : ```scroll```, ```resize```, ```input```, ```mousemove```

### 41.3.1 디바운스

- 디바운스는 짧은 시간 간격으로 이벤트가 연속해서 발생하면 이벤트 핸들러를 호출하지 않다가 일정 시간이 경과한 이후에는 이벤트 핸들러가 한 번만 호출되도록 한다.

```js
<!DOCTYPE html>
<html>
<body>
  <input type="text">
  <div class="msg"></div>
  <script>
    const $input = document.querySelector('input');
    const $msg = document.querySelector('.msg');

    const debounce = (callback, delay) => {
      let timerId;
      // debounce 함수는 timerId를 기억하는 클로저를 반환한다.
      return event => {
        // delay가 경과하기 이전에 이벤트가 발생하면 이전 타이머를 취소하고
        // 새로운 타이머를 재설정한다.
        // 따라서 delay보다 짧은 간격으로 이벤트가 발생하면 callback은 호출되지 않는다.
        if (timerId) clearTimeout(timerId);
        timerId = setTimeout(callback, delay, event);
      };
    };

    // debounce 함수가 반환하는 클로저가 이벤트 핸들러로 등록된다.
    // 300ms보다 짧은 간격으로 input 이벤트가 발생하면 debounce 함수의 콜백 함수는
    // 호출되지 않다가 300ms 동안 input 이벤트가 더 이상 발생하면 한 번만 호출된다.
    $input.oninput = debounce(e => {
      $msg.textContent = e.target.value;
    }, 300);
  </script>
</body>
</html>
```
- ```input 이벤트```는 사용자가 텍스트 입력 필드에 값을 입력할 때마다 발생하는데, 무거운 처리를 동시에 수행한다면 서버에 부담을 줄 것이다.
- 이 때 ```debounce```를 사용하여 ```debounce 함수```에 두 번째 인수로 전달한 시간보다 짧은 간격으로 이벤트가 발생하면 이전 타이머를 취소하고 새로운 타이머를 재설정한다.
- 디바운스는 ```resize 이벤트```, ```input```, ```버튼 중복 클릭 방지 처리 ```등에 유용하게 사용된다.

### 41.3.2 스로틀

- 스로틀은 짧은 시간 간격으로 이벤트가 발생하더라도 일정 시간 간격으로 이벤트 핸들러가 최대 한번만 호출되도록 한다.
- 스로틀은 짧은 시간 간격으로 연속해서 발생하는 이벤트를 그룹화해서 일정 시간 단위로 이벤트 핸들러가 호출되도록 호출 주기를 만든다.
- 스로틀은 ```scroll 이벤트 처리```나 ```무한 스크롤 UI 구현``` 등에 유용하게 사용된다.


---
블로그에 작성한 파트 외 정리본은 아래의 깃헙에서 확인 가능합니다!
> https://github.com/hyeun427/javascript-study
