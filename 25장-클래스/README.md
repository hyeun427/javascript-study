# javascript-study

# 25장 클래스

### 25.1 클래스는 프로토타입의 문법적 설탕인가?

> 프로토타입 기반 객체지향 언어인 자바스크립트는 클래스가 필요 없는(class free)객체지향 프로그래밍 언어다.

ES5에서는 클래스 없이도 생성자 함수와 프로토타입을 통해 객체지향 언어의 상속을 구현할 수 있다. `ES6` 에서 도입된 `클래스`는 기존의 프로토타입 기반 객체지향 모델을 폐지하고 새롭게 클래스 기반 객체지향 모델을 제공하는 것은 아니다.

- `클래스`는 `함수`이며 기존 프로토타입 기반 패턴을 클래스 기반 패턴처럼 사용할 수 있도록 하는 `문법적 설탕`이라고 볼 수도 있다.
  `문법적 설탕(Syntax Sugar)`: 사람이 이해 하고 표현하기 쉽게 디자인된 프로그래밍 언어 문법

- `클래스`와 `생성자 함수`는 모두 프로토타입 기반의 인스턴스를 생성하지만 정확히 동일하게 동작하지는 않는다.

<br>

**차이점은 아래와 같다.**

- 클래스를 `new`연산자 없이 호출하면 에러가 발생한다. 하지만 생성자 함수를 new연산자 없이 호출하면 `일반 함수`로 호출된다.

- 클래스는 상속을 지원하는 `extends`와 `super`키워드를 제공한다.

- 클래스는 `호이스팅`이 발생하지 않는 것처럼 동작한다.

- 클래스 내의 모든 코드에는 암묵적으로 `strict mode`가 지정되어 실행되며 해제할 수 없다

- 클래스의 `constructor`, `프로토타입 메서드`, `정적 메서드`는 열거되지 않는다.모두 프로퍼티 어트리뷰트 `[[Enumerable]]`의 값이 `false`이기 때문이다.

> 따라서 클래스를 프로토타입 기반 객체 생성 패턴의 단순한 문법적 설탕이라고 보기보다는 새로운 객체 생성 매커니즘으로 보는 것이 좀 더 합당하다.

<br>

---

### 25.2 클래스 정의

> 클래스는 어떤 사물의 공통 속성을 모아 정의한 추상적인 개념이고, 인스턴스는 클래스의 속성을 지닌 구체적인 사례이다. by, 코어자바스크립트

```js
[25 - 02];

// 클래스 선언문
class Person {}
```

- `class` 를 사용하여 정의한다.
- `class 이름`은 `파스칼 케이스`를 사용하는 것이 일반적이다.
  <br>

```js
[25 - 03];

// 익명 클래스 표현식
const Person = class {};

// 기명 클래스 표현식
const Person = class MyClass {};
```

- `표현식`으로 클래스를 정의할 수도 있다.
- 클래스는 함수와 마찬가지로 이름을 가질 수도 있고, 갖지 않을 수도 있다.

---

> 클래스를 표현식으로 정의할 수 있다느 것은 값으로 사용할 수 있는 일급 객체라는 것을 의미한다.

- `무명의 리터럴`로 생성할 수 있다, 즉 런타임에 생성이 가능하다.
- `변수`나 `자료구조`(객체, 배열 등)에 저장할 수 있다.
- 함수의 `매개변수`에 전달할 수 있다.
- 함수의 `반환값`으로 사용할 수 있다.

---

> 클래스 몸체에는 0개 이상의 메서드만 정의할 수 있다.

- constructor(생성자)
- 프로토타입 메서드
- 정적 메서드

```js
[25 - 04];

// 📌 클래스 선언문
class Person {
  // 📌 생성자
  constructor(name) {
    // 인스턴스 생성 및 초기화
    this.name = name; // name 프로퍼티는 public하다.
  }

  // 📌 프로토타입 메서드
  sayHi() {
    console.log(`Hi! My name is ${this.name}`);
  }

  // 📌 정적 메서드
  static sayHello() {
    console.log("Hello!");
  }
}

// 인스턴스 생성
const me = new Person("Lee");

// 인스턴스의 프로퍼티 참조
console.log(me.name); // Lee
// 프로토타입 메서드 호출
me.sayHi(); // Hi! My name is Lee
// 정적 메서드 호출
Person.sayHello(); // Hello!
```

![](https://velog.velcdn.com/images/hjthgus777/post/90549291-5a0d-460e-97fc-80d41c1e7fa7/image.png)

<br>

---

### 25.3 클래스 호이스팅

> class 는 함수로 평가된다.

```js
[25 - 05];

// 클래스 선언문
class Person {}

console.log(typeof Person); // function
```

- 클래스 선언문으로 정의한 클래스는 함수 선언문과 같이 소스코드 평가 과정, 즉 런타임 이전에 먼저 평가되어 `함수 객체`를 생성한다.
- 이때 클래스가 평가되어 생성된 함수 객체는 `호출할 수 있는 생성자 함수(constructor)`다.
- 생성자 함수는 함수 정의가 평가되어 함수 객체를 생성하는 시점에 프로토타입도 더불어 생성된다.

> 프로토타입과 생성자 함수는 언제나 쌍으로 존재하기 때문이다.

---

클래스는 클래스 정의 이전에 `참조`할 수 없다.

- const, let 과 같은 오류가 나온다.

```js
[25 - 06];

console.log(Person);
// 📌 ReferenceError: Cannot access 'Person' before initialization

// 클래스 선언문
class Person {}
```

---

클래스 선언문은 `호이스팅`이 발생하지 않는 것처럼 보이나 그렇지 않다.

```js
[25 - 07];

const Person = "";

{
  // 호이스팅이 발생하지 않는다면 ''이 출력되어야 한다.
  console.log(Person);
  // ReferenceError: Cannot access 'Person' before initialization

  // 클래스 선언문
  class Person {}
}
```

- 클래스 선언문도 변수 선언, 함수 정의와 마찬가지로 `호이스팅`이 발생한다.
- 단, 클래스는 `let`, `const` 키워드로 선언한 변수처럼 `호이스팅`된다.

> 따라서 클래서 이전에 `일시적 사각지대(TDZ)`에 빠지기 때문에 호이스팅이 발생하지 않는 것처럼 동작한다.

<br>

---

### 25.4 인스턴스 생성

- `클래스`는 `생성자 함수`이며 `new 연산자`와 함께 호출되어 `인스턴스`를 생성한다.

- `함수`는 new 연산자를 사용여부에 따라 일반함수로 호출되거나, 인스턴스 생성을 위한 생성자 함수로 호출되지만, `클래스`는 인스턴스를 생성하는 것이 유일한 존재이뮤이므로 **반드시 new 연산자와 함께 호출해야한다.**

```js
[25-08], [25-09]

class Person {}

1. new 연산자와 함께 호출
// 인스턴스 생성
const me = new Person();
console.log(me); // Person {}


2. 클래스를 new 연산자 없이 호출하면 타입 에러가 발생한다.
const me = Person();
// TypeError: Class constructor Foo cannot be invoked without 'new'
```

<br>

---

### 25.5 메서드

> 클래스 몸체에는 0개 이상의 메서드만 정의할 수 있다.

- constructor(생성자)
- 프로토타입 메서드
- 정적 메서드
  <br>

#### 25.1 constructor

- `constructor`는 `인스턴스`를 `생성`하고 `초기화`하기 위한 `메서드`다.

- `constructor`는 이름을 변경할 수 없다.

```js
[25 - 11];

class Person {
  // 생성자
  constructor(name) {
    // 인스턴스 생성 및 초기화
    this.name = name;
  }
}

// 클래스는 함수다.
console.log(typeof Person); // function
```

---

**1️⃣ `클래스` 내부를 들여다보자**

![](https://velog.velcdn.com/images/hjthgus777/post/6038fefe-ba04-458f-a645-ab5e0d4347df/image.png)

- 클래스도 함수 객체 `고유의 프로퍼티`를 모두 가지고 있다.
- 함수와 동일하게 프로토타입과 연결되어 있으며 자신의 `스코프 체인`을 구성한다.
- prototype 프로퍼티가 가리키는 프로토타입 객체의 constructor 프로퍼티는 `클래스 자신`을 가리키고 있다.
- 이는 클래스가 인스턴스를 생성하는 `생성자 함수`라는 것을 의미한다.

<br>

**2️⃣ 클래스가 생성한 `인스턴스`의 내부를 들여다보자.**
![](https://velog.velcdn.com/images/hjthgus777/post/4ff5ce83-5cdd-4b9d-b817-b0c6035cf968/image.png)

- constructor 내부에서 this 에 추가한 name 프로퍼티가 클래스가 생성한 인스턴스의 프로퍼티로 추가되었다.
- 즉 생성장 함수와 마찬가지로 constructor 내부에서 this에 추가한 프로퍼티는 인스턴스 프로퍼티가 된다.
- constructor 내부의 this는 생성자 함수와 마찬가지로 클래스가 생성한 인스턴스를 가리킨다.

---

**클래스의 cunstructor 메서드 🆚 프로토타입의 cunstructor 프로퍼티**

- 이 둘은 이름이 같지만 직접적인 관련이 없다.
- 프로토타입의 cuntructor 프로퍼티는 모든 프로토타압이 가지고 있는 프로퍼티이며 생성자 함수를 가리킨다.

---

**constructor는 생성자 함수와 유사하지만 몇가지 차이가 있다.**

```js
[25 - 14];

// 📌 클래스
class Person {
  // 생성자
  constructor(name) {
    // 인스턴스 생성 및 초기화
    this.name = name;
  }
}

// 📌 생성자 함수
function Person(name) {
  // 인스턴스 생성 및 초기화
  this.name = name;
}
```

<br>

- constructor 는 클래스 내에 `한 개`만 존재할 수 있다.

```js
[25-15]

class Person {
  constructor() {}
  constructor() {}
}
// SyntaxError: A class may only have one constructor
```

<br>

- constructor 는 `생략`할 수 있다.
- constructor 는 생략하면 클래스에 빈 constructor가 `암묵적으로 정의`된다.
- constructor를 생략한 클래스는 빈 constructor 에 의해 `빈 객체`를 생성한다.

```js
[25 - 16];

class Person {}

[25 - 17];

class Person {
  // constructor를 생략하면 다음과 같이 빈 constructor가 암묵적으로 정의된다.
  constructor() {}
}

// 빈 객체가 생성된다.
const me = new Person();
console.log(me); // Person {}
```

<br>

- 프로퍼티가 추가되어 초기화된 인스턴스를 생성하려면 constructor 내부에서 `this`에 인스턴스 프로퍼티를 추가한다.

```js
[25 - 18];

class Person {
  constructor() {
    // 고정값으로 인스턴스 초기화
    this.name = "Kim";
    this.address = "Seoul";
  }
}

// 인스턴스 프로퍼티가 추가된다.
const me = new Person();
console.log(me); // Person {name: "Kim", address: "Seoul"}
```

<br>

- 클래스 `외부`에서 인스턴스 프로퍼티의 초기값을 전달하려면 constructor에 매개변수를 선언하고 인스턴스를 생성할 때 초기값을 전달한다.
- 이때 초기값은 constructor의 `매개변수`에 전달된다.

```js
class Person {
  constructor(name, address) {
    // 인수로 인스턴스 초기화
    this.name = name;
    this.address = address;
  }
}

// 인수로 초기값을 전달한다. 초기값은 constructor에 전달된다.
const me = new Person("Kim", "Seoul");
console.log(me); // Person {name: "Kim", address: "Seoul"}
```

<br>

- constructor는 별도의 반환문을 갖지 않는다. return 문 반드시 생략!
- new 연산자와 함께 클래스가 호출되면 암묵적으로 this 즉 인스턴스를 반환하기 때문이다.
- 명시적으로 원시값을 반환하면 원시값은 무시되고 암묵적으로 this가 반환된다.

```js
[25 - 20];

class Person {
  constructor(name) {
    this.name = name;

    // 명시적으로 객체를 반환하면 암묵적인 this 반환이 무시된다.
    return {};
  }
}

// constructor에서 명시적으로 반환한 빈 객체가 반환된다.
const me = new Person("Kim");
console.log(me); // {}

[25 - 21];

class Person {
  constructor(name) {
    this.name = name;

    // 명시적으로 원시값을 반환하면 원시값 반환은 무시되고 암묵적으로 this가 반환된다.
    return 100;
  }
}

const me = new Person("Kim");
console.log(me); // Person { name: "Kim" }
```

<br>

---

#### 25.2 프로토타입 메서드

> 클래스 prototype 내부에 정의된 메서드를 프로토타입 메서드라고 하며, 이들은 인스턴스가 마치 자신의 것처럼 호출할 수 있다. by. 코어자바스크립트

- 클래스는 생성자 함수에 의한 객체 생성 방식과는 다르게 클래스의 prototype 프로퍼티 메서드를 추가하지 않아도 기본적으로 프로토타입 메서드가 된다.

```js
[25 - 23];

class Person {
  // 생성자
  constructor(name) {
    // 인스턴스 생성 및 초기화
    this.name = name;
  }

  // 📌 프로토타입 메서드
  sayHi() {
    console.log(`Hi! My name is ${this.name}`);
  }
}

const me = new Person("Lee");
me.sayHi(); // Hi! My name is Lee
```

<br>

- 생성자 함수와 마찬가지로 클래스가 생성한 인스턴스틑 프로토타입 체인의 일원이 된다.

```js
[25 - 24];

// me 객체의 프로토타입은 Person.prototype이다.
Object.getPrototypeOf(me) === Person.prototype; // -> true
me instanceof Person; // -> true

// Person.prototype의 프로토타입은 Object.prototype이다.
Object.getPrototypeOf(Person.prototype) === Object.prototype; // -> true
me instanceof Object; // -> true

// me 객체의 constructor는 Person 클래스다.
me.constructor === Person; // -> true
```

---

#### 25.3 정적 메서드

> 클래스(생성자함수)에 직접 정의한 메서드를 static 메서드라고 하며, 이들은 인스턴스가 직접 호출할 수 없고, 클래스(생성자함수)에 의해서만 호출할 수 있다. by. 코어자바스크립트

- 정적 메서드는 인스턴스를 생성하지 않아도 호출할 수 있는 메서드다.
- 클래스에서는 메서드에 static 키워드를 붙이면 정적 메서드(클래스 메서드)가 된다.

```js
class Person {
  // 생성자
  constructor(name) {
    // 인스턴스 생성 및 초기화
    this.name = name;
  }

  // 📌 정적 메서드
  static sayHi() {
    console.log("Hi!");
  }
}
```

<br>

- 정적 메서드는 프로토타입 메서드처럼 인스턴스로 호출하지 않고 클래스로 호출한다.

```js
[25 - 27];
// 정적 메서드는 클래스로 호출하며 인스턴스 없이도 호출할 수 있다.
Person.sayHi(); // Hi!

[25 - 28];
// 인스턴스 생성
// 정적 메서드는 인스턴스로 호출할 수 없다.
const me = new Person("Lee");
me.sayHi(); // TypeError: me.sayHi is not a function
```

---

#### 25.4 '정적 메서드'와 '프로토타입 메서드'의 차이

- 정적 메서드와 프로토타입 메서드는 자신이 속해있는 프로토타입 체인이 다르다.
- 정적 메서드는 클래스로 호출, 프로토타입 메서드는 인스턴스로 호출한다.
- 정적 메서드는 인스턴스 프로퍼티를 참조할 수 없지만, 프로토타입 메서드는 인스턴스 프로퍼티를 참조할 수 있다.

---

#### 25.5 클래스에서 정의한 메서드의 특징

- function 키워드를 생략한 메서드 축약 표현을 사용한다.
- 객체 리터럴과는 다르게 클래스에 메서드를 정의할 땐 `콤마`가 필요없다.
- 암묵적으로 `strict mode`로 실행된다.
- `for...in`문이나 `Object.key` 메서드 등으로 열거할 수 없다. 즉, 프로퍼티의 열거 가능 여부를 나타내며, 불리언 값을 갖는 프로퍼티 어트리뷰트 `[[Enumerable ]]`의 값이 `false`다.
- 내부 메서드 `[[Construct]]`를 갖지 않는 `non-constructor`다. 따라서 `new 연산자`와 함께 호출할 수 없다.

---

### 25.6 클래스의 인스턴스 생성과정

#### 1️⃣ 인스턴스 생성과 this 바인딩

- `new`연산자와 함께 클래스를 호출하면 암묵적으로 빈 객체(인스턴스)가 생성된다.
- 암묵적으로 생성된 빈 객체, 즉 인스턴스는 `this`에 `바인딩`된다.

#### 2️⃣ 인스턴스 초기화

- `constructor` 내부의 코드가 실행되며 `this`에 바인딩되어 있는 인스턴스를 초기화한다.
- 인스턴스에 프로퍼티 `추가`하고 `초기화`한다.

#### 3️⃣ 인스턴스 반환

- 클래스의 모든 처리가 끝나면 완성된 인스턴스가 바인딩된 `this`가 암묵적으로 `반환`된다.

```js
[25 - 32];

class Person {
  // 생성자
  constructor(name) {
    // 1. 암묵적으로 인스턴스가 생성되고 this에 바인딩된다.
    console.log(this); // Person {}
    console.log(Object.getPrototypeOf(this) === Person.prototype); // true

    // 2. this에 바인딩되어 있는 인스턴스를 초기화한다.
    this.name = name;

    // 3. 완성된 인스턴스가 바인딩된 this가 암묵적으로 반환된다.
  }
}
```
