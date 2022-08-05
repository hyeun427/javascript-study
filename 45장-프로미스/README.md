자바스크립트는 비동기 처리를 위한 하나의 패턴으로 콜백 함수를 사용한다. 하지만 전통적인 콜백 패턴은 콜벡 헬로 인해 가독성이 나쁘고 비동기 처리 중 발생한 에러의 처리가 곤란하며 여러 개의 비동기 처리를 한 번에 처리하는 데도 한계가 있다.

ES6에서는 비동기 처리를 위한 또 다른 패턴으로 프로미스를 도입했다. 프로미스는 전통적인 콜백 패턴이 가진 단점을 보완하며 비동기 처리 시점을 명확하게 표현할 수 있다.

---

## 45.1 비동기 처리를 위한 콜백 패턴의 단점

![](https://velog.velcdn.com/images/dooin/post/71c08fb1-c162-4c2c-956b-0a0ec021b19c/image.png)

-   동기적 (synchronous) : 순서대로 처리
-   비동기적 (asynchronous) : 동시적으로 처리 (효율적이지만 복잡하다)

### 45.1.1 콜백 헬

비동기 함수란 함수 내부에 비동기로 동작하는 코드를 포함한 함수를 말한다. 비동기 함수를 호출하면 함수 내부의 비동기로 동작하는 코드가 완료되지 않았다 해도 즉시 종료된다.

**즉, 비동기 함수 내부의 비동기로 동작하는 코드는 비동기 함수가 종료된 이후에 완료된다.** 따라서 비동기 함수 내부의 비동기로 동작하는 코드에서 처리 결과를 외부로 반환하거나 상위 스코프의 변수에 할당하면 기대한 대로 동작하지 않는다.

**예제 45-02**

```jsx
let g = 0;

// 비동기 함수인 setTimeout 함수는 콜백 함수의 처리 결과를 외부로 반환하거나
// 상위 스코프의 변수에 할당하지 못한다.
setTimeout(() => {
    g = 100;
}, 0);
console.log(g); // 0
```

![](https://velog.velcdn.com/images/dooin/post/0bde82a0-fb77-43cf-880c-cf33bf5eda2f/image.png)

예를 들어, setTimeout 함수는 비동기 함수이다. 비동기인 이유는 콜백 함수의 호출이 비동기로 동작하기 때문이다.

즉, 비동기 함수인 setTimeout 함수의 콜백 함수는 setTimeout 함수가 종료된 이후에 호출된다. 따라서 함수 내부의 콜백 함수에서 처리 결과를 외부로 반환하거나 상위 스코프의 변수에 할당하면 기대한 대로 동작하지 않는다.

비동기 함수는 비동기 처리 결과를 외부에 반환할 수 없고, 상위 스코프의 변수에 할당할 수도 없다. 따라서 비동기 함수의 처리 결과(서버의 응답 등)에 대한 후속 처리는 비동기 함수 내부에서 수행해야 한다. 이때 비동기 함수를 범용적으로 사용하기 위해 콜백 함수를 전달하는 것이 일반적이다.

필요에 따라 비동기 처리가 성공하면 호출될 콜백 함수와 비동기 처리가 실패하면 호출될 콜백 함수를 전달할 수 있다.

**예제 45-07**

```jsx
// GET 요청을 위한 비동기 함수
const get = (url, callback) => {
    const xhr = new XMLHttpRequest();
    xhr.open("GET", url);
    xhr.send();

    xhr.onload = () => {
        if (xhr.status === 200) {
            // 서버의 응답을 콜백 함수에 전달하면서 호출하여 응답에 대한 후속 처리를 한다.
            callback(JSON.parse(xhr.response));
        } else {
            console.error(`${xhr.status} ${xhr.statusText}`);
        }
    };
};

const url = "https://jsonplaceholder.typicode.com";

// id가 1인 post의 userId를 취득
get(`${url}/posts/1`, ({ userId }) => {
    console.log(userId); // 1
    // post의 userId를 사용하여 user 정보를 취득
    get(`${url}/users/${userId}`, (userInfo) => {
        console.log(userInfo); // {id: 1, name: "Leanne Graham", username: "Bret",...}
    });
});
```

콜백 함수를 통해 비동기 처리 결과에 대한 후속 처리의 후속 처리를 하려고 한다면 함수 호출이 중첩되어 복잡도가 높아지는 현상이 발생하는데 이를 콜백 헬이라 한다.

콜백 헬은 가독성을 나쁘게 하며 실수를 유발하는 원인이 된다. 다음은 콜백 헬이 발생하는 전형적인 사례다.

**예제 45-08**

```jsx
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

### 45.1.2 에러 처리의 한계

**예제 45-09**

```jsx
try {
    setTimeout(() => {
        throw new Error("Error!");
    }, 1000);
} catch (e) {
    // 에러를 캐치하지 못한다
    console.error("캐치한 에러", e);
}
```

setTimeout 함수는 1초 후에 실행되고 이후 에러를 발생시킨다. 하지만 이 에러는 catch 코드 블록에서 캐치되지 않는다.

**에러는 호출자 방향으로 전파된다.** 즉, 콜 스택의 아래 방향으로 전파된다. 하지만 setTimeout함수의 콜백 함수를 호출한 것은 setTimeout 함수가 아니다. 따라서 setTimeout 함수의 콜백 함수가 발생시킨 에러는 catch 블록에서 캐치되지 않는다.

비동기 처리를 위한 콜백 패터은 콜백 헬이나 에러 처리가 곤란하다는 문제가 있다. 이를 극복하기 위해 ES6에서 프로미스가 도입되었다.

---

## 45.2 프로미스의 생성

Promise 생성자 함수를 new 함수와 함께 호출하면 Promise 객체를 생성한다.

Promise 생성자 함수는 비동기 처리를 수행할 콜백 함수를 인수로 받는다. 이 콜백 함수는 resolve와 reject 함수를 인수로 전달받는다.

**예제 45-10**

```jsx
// 프로미스 생성
const promise = new Promise((resolve, reject) => {
  // Promise 함수의 콜백 함수 내부에서 비동기 처리를 수행한다.
  if (/* 비동기 처리 성공 */) {
    resolve('result');
  } else { /* 비동기 처리 실패 */
    reject('failure reason');
  }
});
```

Promise 생성자 함수가 인수로 전달받은 콜백 함수 내부에서 비동기 처리를 수행한다. 이때 비동기 처리가 성공하면 resolve 함수를 호출하고, 비동기 처리가 실패하면 reject 함수를 호출한다.

프로미스는 다음과 같이 현재 비동기 처리가 어떻게 진행되고 있는지를 나타내는 상태 정보를 갖는다.

![](https://velog.velcdn.com/images/dooin/post/8e56c9d2-1d60-4e05-8979-dc438446f811/image.png)

생성된 직후의 프로미스는 기본적으로 pending 상태다. 이후 비동기 처리가 수행되면 비동기 처리 결과에 따라 다음과 같이 프로미스의 상태가 변경된다.

![](https://velog.velcdn.com/images/dooin/post/0ec3069b-bb74-4788-8b1c-04104bcb7a50/image.png)

fulfilled 또는 rejected 상태를 settled 상태라고 한다. settled 상태는 fulfilled 또는 rejected 상태와 상관없이 pending이 아닌 상태로 비동기 처리가 수행된 상태를 말한다.

프로미스는 pending 상태에서 settled 상태로 변활 수 있다. 하지만 settled 상태가 되면 더는 다른 상태로 변화할 수 없다.

**예제 45-12**

```jsx
// fulfilled된 프로미스
const fulfilled = new Promise((resolve) => resolve(1));
```

![](https://velog.velcdn.com/images/dooin/post/a9ccc7c6-f57a-4d7c-ac5f-b44bfc1306d5/image.png)

비동기 처리가 성공하면 프로미스는 pending 상태에서 fulfilled 상태로 변화한다. 그리고 비동기 처리 결과인 1을 값으로 갖는다.

**예제 45-13**

```jsx
// rejected된 프로미스
const rejected = new Promise((_, reject) =>
    reject(new Error("error occurred"))
);
```

![](https://velog.velcdn.com/images/dooin/post/9725c544-65ec-420d-922f-a6f46ff96d1a/image.png)

비동기 처리가 실패하면 프로미스는 pending 상태에서 rejected 상태로 변화한다. 그리고 비동기 처리 결과인 Error 객체를 값으로 갖는다.

즉, **프로미스는 비동기 처리 상태와 처리 결과를 관리하는 객체다.**

---

## 45.3 프로미스의 후속 처리 메서드

프로미스의 비동기 처리 상태가 변화하면 이에 따른 후속 처리를 해야 한다.

예를 들어, 프로미스가 fulfilled 상태가 되면 프로미스의 처리 결과를 가지고 무언가를 해야 하고, 프로미스가 rejected 상태가 되면 프로미스의 처리 결과(에러)를 가지고 에러 처리를 해야 한다.

이를 위해 프로미스는 후속 메서드 then, catch, finally를 제공한다.

**프로미스의 비동기 처리 상태가 변화하면 후속 처리 메서드에 인수로 전달한 콜백 함수가 선택적으로 호출된다.**

이때 후속 처리 메서드의 콜백 함수에 프로미스의 처리 결과가 인수로 전달된다.

### 45.3.1 Promise.prototype.then

**예제 45-14**

```jsx
// fulfilled
new Promise((resolve) => resolve("fulfilled")).then(
    (v) => console.log(v),
    (e) => console.error(e)
); // fulfilled

// rejected
new Promise((_, reject) => reject(new Error("rejected"))).then(
    (v) => console.log(v),
    (e) => console.error(e)
); // Error: rejected
```

then 메서드는 두 개의 콜백 함수를 인수로 전달 받는다.

첫 번째 콜백 함수는 프로미스가 fulfilled 상태가 되면 호출되고, 두 번째 콜백 함수는 프로미스가 rejected 상태가 되면 호출된다.

### 45.3.2 Promise.prototype.catch

**예제 45-15**

```jsx
// rejected
new Promise((_, reject) => reject(new Error("rejected"))).catch((e) =>
    console.log(e)
); // Error: rejected
```

catch 메서드는 한 개의 콜백 함수를 인수로 전달받는다. catch 메서드의 콜백 함수는 프로미스가 rejected 상태인 경우만 호출된다.

### 45.3.3 Promise.prototype.finally

**예제 45-16**

```jsx
// rejected
new Promise((_, reject) => reject(new Error("rejected"))).then(undefined, (e) =>
    console.log(e)
); // Error: rejected
```

finally 메서드는 한 개의 콜백 함수를 인수로 전달 받는다. finally 메서드의 콜백 함수는 프로미스의 성공, 실패와 상관없이 무조건 한 번 호출된다.

---

## 45.4 프로미스의 에러 처리

**예제 45-19**

```jsx
const wrongUrl = "https://jsonplaceholder.typicode.com/XXX/1";

// 부적절한 URL이 지정되었기 때문에 에러가 발생한다.
promiseGet(wrongUrl).then(
    (res) => console.log(res),
    (err) => console.error(err)
); // Error: 404
```

예제 19 코드에서 부적절한 URL을 넣었기 때문에 에러가 발생한다.

**예제 45-20**

```jsx
const wrongUrl = "https://jsonplaceholder.typicode.com/XXX/1";

// 부적절한 URL이 지정되었기 때문에 에러가 발생한다.
promiseGet(wrongUrl)
    .then((res) => console.log(res))
    .catch((err) => console.error(err)); // Error: 404
```

비동기 처리에서 발생한 에러는 프로미스의 후속 처리 메서드 catch를 사용해 처리할 수도 있다. catch 메서드를 호출하면 내부적으로 then(undefined, onRejected)을 호출한다.

단, then 메서드의 두 번째 콜백 함수는 첫 번째 콜백 함수에서 발생한 에러를 캐치하지 못하고 코드가 복잡해져서 가독성이 좋지 않다.

**예제 45-23**

```jsx
promiseGet("https://jsonplaceholder.typicode.com/todos/1")
    .then((res) => console.xxx(res))
    .catch((err) => console.error(err)); // TypeError: console.xxx is not a function
```

catch 메서드를 모든 then 메서드를 호출한 이후에 호출하면 비동기 처리에서 발생한 에러(rejected 상태) 뿐만 아니라 then 메서드 내부에서 발생한 에러까지 모두 캐리할 수 있다.

또한 then 메서드에 두 번째 콜백 함수를 전달하는 것보다 catch 메서드를 사용하는 것이 가독성이 좋고 명확하다. 따라서 **에러 처리는 then 메서드에서 하지말고 catch 메서드에서 하는 것을 권장한다.**

---

## 45.5 프로미스 체이닝

비동기 처리를 위한 콜백 패턴은 콜백 헬이 발생하는 문제가 있다. 프로미스는 then, catch, finally 후속 처리 메서드를 통해 콜백 헬을 해결한다.

**예제 45-24**

```jsx
const url = "https://jsonplaceholder.typicode.com";

// id가 1인 post의 userId를 취득
promiseGet(`${url}/posts/1`)
    // 취득한 post의 userId로 user 정보를 취득
    .then(({ userId }) => promiseGet(`${url}/users/${userId}`))
    .then((userInfo) => console.log(userInfo))
    .catch((err) => console.error(err));
```

24번 예제 코드에서 then -> then -> catch 순서로 후속 처리 메서드를 호출했다. 후석 처리 메서드는 언제나 프로미스를 반환하므로 연속적인 호출이 가능하다. 이를 `프로미스 체이닝`이라 한다.

![](https://velog.velcdn.com/images/dooin/post/32f28110-1308-4458-a61e-bff290893bd5/image.png)

이처럼 then, catch, finally 후속 처리 메서드는 콜백 함수가 반환한 프로미스를 반환한다. 만약 후속 처리 메서드의 콜백 함수가 프로미스가 아닌 값을 반환하더라도 그 값을 암묵적으로 resolve 또는 reject하여 프로미스를 생성해 반환한다.

프로미스는 프로미스 체이닝을 통해 비동기 처리 결과를 전달받아 후속 처리를 하므로 비동기 처리를 위한 콜백 패턴에서 발생하던 콜백 헬이 발생하지 않는다.

콜백 패턴은 가독성이 좋지 않기 때문에 ES8에서 도입된 async/await를 통해 해결할 수 있다. 프로미스의 후속 처리 메서드 없이 마치 동기 처리처럼 프로미스가 처리 결과를 반환하도록 구현할 수 있다.

async/await도 프로미스를 기반으로 동작하므로 프로미스는 잘 이해하고 있어야 한다.

---

## 45.6 프로미스의 정적 메서드

Promise는 주로 생성자 함수로 사용되지만 함수도 객체이므로 메서드를 가질 수 있다. promise는 5가지 정적 메서드를 제공한다.

### 45.6.1 Promise.resolve / Promise.reject

이미 존재하는 값을 래핑하여 프로미스를 생성하기 위해 사요한다.

Promise.resolve 메서드는 인수로 전달받은 값을 resolve하는 프로미스를 생성한다.

Promise.reject 메서드는 인수로 전달받은 값을 reject하는 프로미스를 생성한다.

### 45.6.2 Promise.all

Promise.all 메서드는 여러개의 비동기 처리를 모두 병렬처리할 때 사용한다.

**예제 45-30**

```jsx
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
        console.log(res); // [1, 2, 3] ⇒ 약 6초 소요
    })
    .catch(console.error);
```

30번 예제 코드는 세 개의 비동기 처리를 순차적으로 처리한다. 첫 번째 비동기 처리에 3초, 두 번째 비동기 처리에 2초, 세 번째 비동기 처리에 1초가 소요되어 총 6초 이상이 소요된다.

하지만 세 개의 비동기 처리는 서로 의존하지 않고 개별적으로 수행된다. 앞선 비동기 처리 결과를 다음 비동기 처리가 사용하지 않는다.

**예제 45-31**

```jsx
const requestData1 = () =>
    new Promise((resolve) => setTimeout(() => resolve(1), 3000));
const requestData2 = () =>
    new Promise((resolve) => setTimeout(() => resolve(2), 2000));
const requestData3 = () =>
    new Promise((resolve) => setTimeout(() => resolve(3), 1000));

Promise.all([requestData1(), requestData2(), requestData3()])
    .then(console.log) // [ 1, 2, 3 ] ⇒ 약 3초 소요
    .catch(console.error);
```

이때 Promise.all 메서드를 사용하면 세 개의 비동기 처리를 병렬로 처리가 가능해진다.

Promise.all 메서드는 프로미스를 요소로 갖는 배열 등의 이터러블을 인수로 전달받는다. 전달받은 모든 프로미스가 모두 fulfilled 상태가 되면 모든 처리 결과를 배열에 저장해 새로운 프로미스를 반환한다.

따라서 모든 처리에 걸리는 시간은 가장 늦게 fulfilled 상태가 되는 첫 번쨰 프로미스의 처리 시간인 3초보다 조금 더 소요된다.

모든 프로미스가 fulfiled 상태가 되면 resolve된 처리 결과를 모두 배열에 저장해 새로운 프로미스를 반환한다. 이때 첫 번째 프로미스가 가장 나중에 fulfiled 상태가 되어도 Promise.all 메서드는 첫 번째 프로미스가 resolve한 처리 결과부터 차례대로 배열에 저장한다. 즉 처리 순서가 보장된다.

### 45.6.3 Promise.race

Promise.race 메서드는 Promise.all 메서드와 동일하게 프로미스를 요소로 갖는 배열 등의 이터러블을 인수로 전달받는다.

하지만 모든 프로미스가 fulfilled 상태가 되는 것을 기다리는 것이 아니라 가장 먼저 fulfilled 상태가 된 프로미스의 처리 결과를 resolve하는 새로운 프로미스를 반환한다.

### 45.6.4 Promise.allSettled

Promise.allSettled 메서드는 fulfilled 또는 rejected 상태와는 상관없이 인수로 전달받은 모든 프로미스들의 처리 결과가 모두 담겨있다.

---

## 45.7 마이크로태스크 큐

**예제 45-39**

```jsx
setTimeout(() => console.log(1), 0);

Promise.resolve()
    .then(() => console.log(2))
    .then(() => console.log(3));
```

1 -> 2 -> 3 순으로 출력될 것처럼 보이지만 2 -> 3 -> 1의 순으로 출력된다. 그 이유는 프로미스의 후속 처리 메서드의 콜백 함수는 태스트 큐가 아니고 마이크로태스트 큐에 저장되기 때문이다. 둘은 각각 별도의 큐다.

마이크로태스크 큐에는 프로미스의 후속 처리 메서드의 콜백 함수가 임시 저장된다. 콜백 함수나 이벤트 핸들러를 일시 저장한다는 점에서 동일하지만, 마이크로태스크 큐는 태스크 큐보다 우선순위가 높다.

---

## 45.8 fetch

fetch 함수는 XMLHttpRequest 객체와 마찬가지로 HTTP 요청 전송 기능을 제공하는 클라이언트 사이드 Web API다.

fetch 함수는 XMLHttpRequest 객체보다 사용법이 간단하고 프로미스를 지원하기 때문에 비동기 처리를 위한 콜백 패턴의 단점에서 자유롭다.

**fetch 함수는 HTTP 응답을 나타내는 Response 객체를 래핑한 Promise 객체를 반환한다.**

fetch 함수는 HTTP 응답을 나타내는 Response 객체를 래핑한 프로미스를 반환하므로 후속 처리 메서드 then을 통해 프로미스가 resolve한 Response 객체를 전달받을 수 있다.
