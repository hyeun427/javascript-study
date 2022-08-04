## 43.1 Ajax란?

- Asynchronous JavaScript and XML의 약어로 자바스크립트를 사용해 브라우저가 서버와 비동기 방식으로 통식하도록 하는 방법이다.
- XMLHttpRequest 객체 혹은 fetch 함수를 이용해 구현할 수 있다.
- Ajax를 통해 서버로부터 웹페이지의 변경에 필요한 데이터만 비동기 방식으로 전송받아 웹페이지를 변경할 필요가 없는 부분은 다시 렌더링하지 않고, 변경할 부분만 렌더링할 수 있게 되었다.

## 43.2 JSON

- JSON(JavaScript Object Notation)은 클라이언트와 서버 간 HTTP 통신을 위한 데이터 포맷이다.
- 자바스크립트에 종속되지 않는 언어 독립형 데이터 포맷이라 대부분의 프로그래밍 언어에서 사용할 수 있다.

### 43.2.1 JSON 표기방식

- 자바스크립트의 객체 리터럴과 유사하게 ```키와 값으로 구성```된 순수한 텍스트이다.

```js
{
  "name": "Lee",
  "age": 20,
  "alive": true,
  "hobby": ["traveling", "tennis"]
}
```

### 43.2.2 JSON.stringify

- ```JSON.stringify``` 메서드는 객체를 JSON 포맷의 문자열로 변환한다. 
- 이를 ```직렬화(serializing)``` 라고 한다.
- 객체 뿐만 아니라 배열도 JSON 포맷의 문자열로 변환할 수 있다.

```js
const obj = {
  name: 'Lee',
  age: 20,
  alive: true,
  hobby: ['traveling', 'tennis']
};

// 객체를 JSON 포맷의 문자열로 변환한다.
const json = JSON.stringify(obj);
console.log(typeof json, json);
// string {"name":"Lee","age":20,"alive":true,"hobby":["traveling","tennis"]}
```
### 43.2.3 JSON.parse

- ```JSON.parse```메서드는 JSON 포맷의 문자열을 객체로 변환한다.
- 이를 ```역직렬화(deserializing)```하고 한다.

## 43.3 XMLHttpRequest

- 브라우저에서는 주소창이나 form 태그나 a 태그를 통해 HTTP 요청 전송 기능을 제공한다.

### 43.3.1 XMLHttpRequest 객체 생성

- XMLHttpRequest 객체는 XMLHttpRequest 생성자 함수를 호출하여 생성한다.

### 43.3.2 XMLHttpRequest 객체의 프로퍼티와 메서드

![](https://velog.velcdn.com/images/hyeun427/post/1e9d01f6-558e-4a89-9538-b824e59a461d/image.png)
![](https://velog.velcdn.com/images/hyeun427/post/947fe3f9-6444-490e-a695-52ce3e731cc1/image.png)

### 43.3.3 HTTP 요청 전송

**요청 순서**
1. XMLHttpRequest.prototype.open 메서드로 HTTP 요청을 초기화한다.
2. 필요에 따라 XMLHttpRequest.prototype.setRequsetHeader 메서드로 특정 HTTP 요청의 헤더 값을 설정한다.
3. XMLHttpRequest.prototype.send 메서드로 HTTP 요청을 전송한다.

### 43.3.4 HTTP 응답 처리

- HTTP 요청의 현재 상태를 나타내는 ```readyState 프로퍼티```값이 변경된 경우 발생하는 readystatechange 이벤트를 캐치해서 HTTP 응답을 처리한다.
- 
