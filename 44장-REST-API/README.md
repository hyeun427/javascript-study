# javascript-study

# 44장 REST API

> REST는 HTTP를 기반으로 클라이언트가 서버의 리소스에 접근하는 방식을 규정한 아키텍처고, REST API는 REST를 기반으로 서비스 API를 구현한 것을 의미한다.

<br>

### 44.1 REST API의 구성

- REST API는 자원, 행위, 표현의 3가지 요소로 구성된다.

- REST는 자체 표현 구조로 구성되어 REST API만으로 HTTP 요청의 내용을 이해할 수 있다.

| 구성요소 | 내용                           | 표현 방법        |
| :------- | :----------------------------- | :--------------- |
| 자원     | 자원                           | URI(엔드 포인트) |
| 행위     | 자원에 대한 행위               | HTTP 요청 메서드 |
| 표현     | 자원에 대한 행위의 구체적 내용 | 페이로드         |

<br>

### 44.2 REST API 설계 원칙

REST에서 가장 중요한 기본적인 원칙은 두 가지다.

- URI 는 리소스를 표현하는 데 집중하고 행위에 대한 정의는 HTTP 요청 메서드를 통해 하는 것이 RESTful API를 설계하는 중심 규칙이다.

<br>

#### 1️⃣ URI는 리소스를 표현해야한다.

- 리소스를 식별할 수 있는 이름은 동사보다는 명사를 사용한다.

```
#bad
GET/getTodos/1
GET/todos/show/1

#good
GET/todos/1
```

<br>

#### 2️⃣ 리소스에 대한 행위는 HTTP 요청 메서드로 표현한다.

- HTTP요청 메서드는 클라이언트가 서버에 요청의 종류와 목적(리소스에 대한 행위)을 알리는 방법이다.

- 주로 5가지 요청 메서드를 사용하여 CRUD를 구현한다.

| HTTP 요청 메서드 | 종류           | 목적                  | 페이로드(전송되는 데이터) |
| :--------------- | :------------- | :-------------------- | :------------------------ |
| GET              | index/retrieve | 모든/특정 리소스 취득 | X                         |
| POST             | create         | 리소스 생성           | O                         |
| PUT              | replace        | 리소스 전체 교체      | O                         |
| PATCH            | modify         | 리소스 일부 수정      | O                         |
| DELETE           | delete         | 모든/특정 리소스 삭제 | X                         |

- 리소스에 대한 행위는 HTTP 요청 메서드를 통해 표현하며 URI에 표현하지 않는다.

```
#bad
GET/getTodos/delete/1

#good
DELETE/todos/1
```

<br>

### 44.3 JSON Server를 이용한 REST API 실습

- [JSON Server](https://www.npmjs.com/package/json-server)를 사용해 가상 REST API 서버를 구축하여 HTTP 요청을 전송하고 응답을 받는 실습을 해보자.

`JSON Server` : 별도의 서버가 없는 환경에서 프론트엔드 작업을 진행하고자 할 때 최소한의 실무적인 환경을 제공 받을 수 있는 API-Server

<br>

#### 44.3.1 JSON Server 설치

JSON Server는 json 파일을 사용하여 가상 REST-API 서버를 구축할 수 있는 툴이다.

- npm을 사용하여 설치한다.

```
npm init -y
npm install json-server --save-dev
```

<br>

#### 44.3.2 db.json 파일 생성

- 프로젝트 루트 폴더에 db.json 파일을 만든다.

- db.json 파일은 리소스를 제공하는 데이터 베이스 역할을 한다.

![](https://velog.velcdn.com/images/hjthgus777/post/2092b00a-23f8-48ae-bad0-8c6b528d8456/image.png)

#### 44.3.3 JSON Server 실행

- 터미널에 다음과 같은 명령어를 입력하여 서버를 실행한다.

- JSON Server가 데이터베이스 역할을 하는 db.json 파일의 변경을 감지하게 하려면 watch 옵션을 추가한다.
- 기본 포트는 3000이고, 포트 변경하려면 port 옵션을 추가한다.

```js
## 기본 포트(3000)사용 / watch 옵션 적용
json-server --watch db.json

## 포트 변경 / watch 옵션 적용
json server --watch db.json --port 5000
```

- 위와 같이 매번 명령어를 입력하는 것은 번거로우니 pakage.json 파일의 script를 아래처럼 변경해서 실행하자. (불필요한 부분은 삭제했다.)
  ![](https://velog.velcdn.com/images/hjthgus777/post/687d8d76-5e09-456e-898c-24bf2e399332/image.png)

- npm start 명령어를 입력하여 JSON server를 실행해보자.
  ![](https://velog.velcdn.com/images/hjthgus777/post/535e5d9e-c03a-454b-b350-5fa808eaa821/image.png)

<br>

#### 44.3.4 GET 요청

- toto 리소스에서 모든 todo를 취득(indx)한다.

- 루트 폴더에 public 폴더를 생성하고 JSON Server를 중단한 후 재실행한다.

- public 폴더에 get_index.html을 추가하고 브라우저에서 http://localhost:3000/get_index.html 로 접속한다.
  ![](https://velog.velcdn.com/images/hjthgus777/post/e460e412-3864-42ae-9c6e-425f8f5988f7/image.png)![](https://velog.velcdn.com/images/hjthgus777/post/8eec17c3-30a8-494f-a8f0-3f71621b2e94/image.png)

- todo 리소스에서 id를 사용하여 특정 todo를 취득(retrieve)한다.

- get_retrieve.html을 추가하고 http://localhost:3000/get_retrieve.html 로 접속한다.
  ![](https://velog.velcdn.com/images/hjthgus777/post/74fcd241-08a5-44a8-9ec4-d1c302eecbb6/image.png)![](https://velog.velcdn.com/images/hjthgus777/post/c965069e-47d2-4bfc-a2a6-3491f3093988/image.png)

<br>

#### 44.3.5 POST 요청

- todo 리소스에서 새로운 todo를 생성한다.

- POST 요청 시에는 setRequestHeader 메서드를 사용하여 요청 몸체에 담아 서버로 전송할 페이로드의 MIME 타입을 지정해야 한다.

- post.html 파일을 만들고 http://localhost:3000/post.html 로 접속한다.

![](https://velog.velcdn.com/images/hjthgus777/post/8399f36e-1ded-4b9c-bbc3-da69f05d8f5f/image.png)![](https://velog.velcdn.com/images/hjthgus777/post/917a25e1-2f04-45b5-8816-c040bedd873e/image.png)

<br>

#### 44.3.6 PUT 요청

- PUT은 특정 리소스 전체를 교체할 때 사용한다.

- 다음 예제에서는 todo 리소스에서 id로 toto를 특정하여 id를 제외한 리소스 전체를 교체한다.
- PUT 요청시 setRequestHeader 메서드를 사용하여 요청 몸체에 담아 서버로 전송할 페이로드의 MIME 타입을 지정해야한다.
- put.html 파일을 만들고 http://localhost:3000/put.html 로 접속한다.
  ![](https://velog.velcdn.com/images/hjthgus777/post/8b036ac3-bb41-41f2-98c8-f2a824f7bde6/image.png)![](https://velog.velcdn.com/images/hjthgus777/post/b2681a21-313a-4471-aa93-ba82d1016a1f/image.png)

<br>

#### 44.3.7 PATCH 요청

- PATCH는 특정 리소스의 일부를 수정할 때 사용한다.

- todo 리소스의 id로 todo를 특정하여 completed만 수정한다.

- PATCH 요청시 setRequestHeader 메서드를 사용하여 요청 몸체에 담아 서버로 전송할 페이로드의 MIME 타입을 지정해야한다

- patch.html 파일을 만들고 http://localhost:3000/patch.html 로 접속한다.
  ![](https://velog.velcdn.com/images/hjthgus777/post/d4417763-3945-4818-8312-35bc2fea2838/image.png)![](https://velog.velcdn.com/images/hjthgus777/post/c98c74d9-0d02-401e-b1b9-79132205c0b8/image.png)

<br>

#### 44.3.8 DELETE 요청

- todo 리소스에서 id로 사용하여 todo를 삭제한다.

- delete.html 파일을 만들고 http://localhost:3000/delete.html 로 접속한다.
  ![](https://velog.velcdn.com/images/hjthgus777/post/a50cc99d-599d-4df3-a169-b531ee6d9b0d/image.png)![](https://velog.velcdn.com/images/hjthgus777/post/8f506b05-d814-4e50-8873-7bcfc51ee6fa/image.png)
