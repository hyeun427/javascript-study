# javascript-study

## 19장 프로토타입

자바스크립트는 `클래스 기반` 객체지향 프로그래밍 언어보다 효율적이며 강력한 객체지향 프로그래밍 능력을 지니고 있는 `프로토타입 기반`의 `객체지향` 프로그래밍 언어다.

> 자바스크립트는 객체 기반의 프로그래밍 언어이며 자바스크립트를 이루고 있는 거의 '모든 것'이 객체다.

<br>

---

### 19.1 객체지향 프로그래밍

> 객체지향 프로그래밍은 실세계의 실체(사물이나 개념)를 인식하는 철학적 사고를 프로그래밍에 접목하려는 시도에서 시작한다.

`속성`을 통해 여러 개의 값을 하나의 단위로 구성한 복합적인 자료구조를 `객체`라하며, `객체 지향 프로그래밍`은 독립적인 객체의 `집합`으로 프로그램을 표현하려는 `프로그래밍 패러다임`이다.

#### 원이라는 개념을 객체로 만들어보자.

- 원에는 반지름이라는 속성이 있다. 이 반지름을 가지고 원의 지름, 둘레, 넓이를 구할 수 있다.

- 반지름은 원의 `상태를 나타내는 데이터`이며 원의 지름, 둘레, 넓이를 구하는 것은 `동작`이다.

> 객체지향 프로그래밍은 객체의 `상태`를 나타내는 데이터와 상테 데이터를 조작할 수 있는 `동작`을 하나의 논리적인 단위로 묶어 생각한다.

- 즉 객체는 `상태 데이터와 동작을 하나의 논리적인 단위로 묶은 복합적인 자료구조`이다.

- 여기서 객체의 상태 데이터를 `프로퍼티`, 동작을 `메서드`라 부른다.

<br>

---

### 19.2 상속과 프로토타입

> `상속`은 객체지향 프로그래밍의 핵심 개념으로, 어떤 객체의 프로퍼티 또는 메서드를 다른 객체가 상속받아 그대로 사용할 수 있는 것을 말한다.

<br>

---

### 19.3 프로토타입

> `프로토타입 객체(또는 줄여서 프로토타입)`란 객체지향 프로그래밍의 근간을 이루는 객체 간 상속을 구현하기 위해 사용된다.

- 프로토타입은 어떤 객체의 상위(부모) 객체의 역할을 하는 객체로서 다른 객체에 `공유 프로퍼티(메서드 포함)`을 제공한다.

- 프로토타입을 상속받은 하위(자식) 객체는 상위 객체의 프로퍼티를 자신의 프로퍼티처럼 자유롭게 사용할 수 있다.

#### 19.3.1 \_\_ **proto ** \_\_ 접근자 프로퍼티

모든 객체는 `__proto__` 접근자 프로퍼티를 통해 자신의 프로토타입, 즉 [[Prototype]] 내부 슬롯에 간접적으로 접근할 수 있다.
![](https://velog.velcdn.com/images/hjthgus777/post/5632b3bb-2290-4b25-8368-88a525b62757/image.png)

<br>

► `__proto__` 는 접근자 프로퍼티다.

► `__proto__` 접근자 프로퍼티는 상속을 통해 사용된다.

► `__proto__` 접근자 프로퍼티를 통해 프로토타입에 접근하는 이유

- [[Prototype]] 내부 슬롯의 값, 즉 포로토타입에 접근하기 위해 접근자 프로퍼티를 사용하는 이유는 상호 참조에 의해 `프로토타입 체인`이 생성되는 것을 `방지`하기 위해서다.

- 프로토타입 체인은 `단방향 링크드 리스트`로 구현되어야한다.

- 다시 말해 순환참조하는 프로토타입 체인이 만들어지면 프로토타입 체인 종점이 존재하지 않기 때문에 프로토타입 체인에서 프로퍼티를 검색할 때 `무한루프` 에 빠진다.

► `__proto__` 접근자 프로퍼티를 코드 내에서 직접 사용하는 것은 권장하지 않는다.

- `__proto__` 접근자 프로퍼티 대신 프로토타입의 참조를 취득하고 싶은 겨우에는 `Object.getPrototypeOf` 메서드를 사용한다.

- 프로토타입을 `교체`하고 싶은 경우에는 `Object.setPrototypeOf` 메서드를 사용할 것을 권장한다.

<br>

#### 19.3.2 함수 객체의 prototype 프로퍼티

함수 객체만이 소유하는 prototype 프로퍼티는 생성자 함수가 생성할 인스턴스의 프로토타입을 가리킨다.

```js
// 함수 객체는 prototype 프로퍼티를 소유한다.
(function () {}.hasOwnProperty("prototype")); // -> true

// 일반 객체는 prototype 프로퍼티를 소유하지 않는다.
({}.hasOwnProperty("prototype")); // -> false
```

- prototype 프로퍼티는 생성자 함수가 생성할 객체(인스턴스)의 프로토타입을 가리킨다.

- 따라서 생성자 함수로 호출할 수 없는 화살표 함수와 ES6 메서드 축약 표현으로 정의한 메서드는 prototype 프로퍼티를 소유하지 않으며 프로토타입도 생성하지 않는다.

```js
// 화살표 함수는 non-constructor다.
const Person = (name) => {
  this.name = name;
};

// non-constructor는 prototype 프로퍼티를 소유하지 않는다.
console.log(Person.hasOwnProperty("prototype")); // false

// non-constructor는 프로토타입을 생성하지 않는다.
console.log(Person.prototype); // undefined

// ES6의 메서드 축약 표현으로 정의한 메서드는 non-constructor다.
const obj = {
  foo() {},
};

// non-constructor는 prototype 프로퍼티를 소유하지 않는다.
console.log(obj.foo.hasOwnProperty("prototype")); // false

// non-constructor는 프로토타입을 생성하지 않는다.
console.log(obj.foo.prototype); // undefined
```

- 아래는 생성자 함수로 객체를 생성한 후 `__proto__` 접근자 프로퍼티와 `prototype 프로퍼티`로 프로토타입 객체에 접근한 예시이다.

```js
// 생성자 함수
function Person(name) {
  this.name = name;
}
// Person 이라는 생성자 함수를 사용해서 me 인스턴스 생성
const me = new Person("Lee");

// 결국 Person.prototype과 me.__proto__는 결국 동일한 프로토타입을 가리킨다.
console.log(Person.prototype === me.__proto__); // true
console.log(me.constructor === Person);
```

![](https://velog.velcdn.com/images/hjthgus777/post/1badd680-08f0-42c1-8ff7-bfd0f043c8f1/image.png)

![](https://velog.velcdn.com/images/hjthgus777/post/120eff83-7081-4618-8dfb-dee697f7768f/image.png)

- `__proto__` 로 접근했을때 그리고 `getPrototypeof(me)` 로 접근했을때 Person.protype이 같다.

<br>

#### 19.3.3 프로토타입의 constructor 프로퍼티와 생성자 함수

- 즉 me 객체에는 `constuctor` 프로퍼티가 없지만 me 객체의 프로토타입인 `Person.prototype`에는 `constructor` 프로퍼티가 있다. 따라서 me 객체는 프로토타입인 `Person.prototype`의 `constructor` 프로퍼티를 상속받아 사용할 수 있다.

<br>

---

### 19.4 리터럴 표기법에 의해 생성된 객체의 생성자 함수와 프로토타입

- `생성자 함수`에 의해 생성된 `인스턴스`는 프로토타입의 `constructor` 프로퍼티에 의해 `생성자 함수`와 연결된다.

- 이때 `constructor` 프로퍼티가 가리키는 `생성자 함수`는 `인스턴스`를 생성한 `생성자 함수`다.

```js
// obj 객체를 생성한 생성자 함수는 Object다.
const obj = new Object();
console.log(obj.constructor === Object); // true

// add 함수 객체를 생성한 생성자 함수는 Function이다.
const add = new Function("a", "b", "return a + b");
console.log(add.constructor === Function); // true

// 생성자 함수
function Person(name) {
  this.name = name;
}

// me 객체를 생성한 생성자 함수는 Person이다.
const me = new Person("Lee");
console.log(me.constructor === Person); // true
```

- 하지만 `리터럴 표기법`에 의한 객체 생성 방식과 같이 new 연산자와 함께 생성자 함수를 호출하여 인스턴스를 생성하지 않는 객체 생성 방식도 있다.

```js
// 객체 리터럴
const obj = {};

// 함수 리터럴
const add = function (a, b) {
  return a + b;
};

// 배열 리터럴
const arr = [1, 2, 3];

// 정규표현식 리터럴
const regexp = /is/gi;
```

- 리터럴 표기법에 의해 생성된 객체도 물론 프로토타입이 존재한다.
  ![](https://velog.velcdn.com/images/hjthgus777/post/5e98fe86-ef73-4c8b-8643-a95c2dbbf650/image.png)

- 하지만 리터럴 표기법에 의해 생성된 객체의 경우 프로토타입 constructor 프로퍼티가 가리키는 생성자 함수가 반드시 객체를 생성한 생성자 함수라고 단정할 수는 없다.

```js
// obj 객체는 Object 생성자 함수로 생성한 객체가 아니라 객체 리터럴로 생성했다.
const obj = {};

// 하지만 obj 객체의 생성자 함수는 Object 생성자 함수다.
console.log(obj.constructor === Object); // true
```

- object 생성자 함수에 인수를 전달하지 않거나 undefined 또는 null을 인수로 전달하면서 호출하면 내부적으로Object.prototype을 프로토타입으로 갖는 빈 객체를 생성한다.

```js
// 2. Object 생성자 함수에 의한 객체 생성
// Object 생성자 함수는 new 연산자와 함께 호출하지 않아도 new 연산자와 함께 호출한 것과 동일하게 동작한다.
// 인수가 전달되지 않았을 때 추상 연산 OrdinaryObjectCreate를 호출하여 빈 객체를 생성한다.
let obj = new Object();
console.log(obj); // {}
```

```js
// 1. new.target이 undefined나 Object가 아닌 경우
// 인스턴스 -> Foo.prototype -> Object.prototype 순으로 프로토타입 체인이 생성된다.
class Foo extends Object {}
new Foo(); // Foo {}
```

_풀이 by youtube_

class 로 Foo를 Object를 extends 했다. 이것의 경우
const f = new FOO() 로 만들어보면

![](https://velog.velcdn.com/images/hjthgus777/post/f96de432-290c-475a-b273-1d60fce783c4/image.png)

이것은 Foo 라는 class를 상속받았는데 Foo 는 또 Object를 상속 받았다.
![](https://velog.velcdn.com/images/hjthgus777/post/c782c355-378e-4118-8af5-703561fef9b3/image.png)

![](https://velog.velcdn.com/images/hjthgus777/post/5401dc0f-d331-4048-b8a2-69bb33dd0532/image.png)

1. 그래서 Foo 의 [[Prototype]] 을 열어보면 class Foo를 가리키는 프로토타입이 나오고

2. 다시 class Foo 안에 들어가 있는 [[Prototype]] 을 열어보면 Object 를 가리킨다.

3. 그리고 이것이 마지막이기 때문에 더 이상의 [[Prototype]] 이것은 보이지 않는다.

4. 다시 접근을 하려고 하면 자기 자신이 나오는 것을 알 수 있다.

```js
// 3. 인수가 전달된 경우에는 인수를 객체로 변환한다.
// Number 객체 생성
obj = new Object(123);
console.log(obj); // Number {123}

// String  객체 생성
obj = new Object("123");
console.log(obj); // String {"123"}
```

---

- 함수 객체의 경우 차이가 더 명확하다.

```js
// foo 함수는 Function 생성자 함수로 생성한 함수 객체가 아니라 함수 선언문으로 생성했다.
function foo() {}

// 하지만 constructor 프로퍼티를 통해 확인해보면 함수 foo의 생성자 함수는 Function 생성자 함수다.
console.log(foo.constructor === Function); // true
```

- 리터럴 표기법에 의해 생성된 객체도 상속을 위해 프로토타입이 필요하다.
- 프로토타입과 생성자 함수는 단독으로 존재할 수 없고 언제나 쌍으로 존재한다.

<br>

---

### 19.5 프로토타입의 생성 시점

> 프로토타입은 생성자 함수가 생성되는 시점에 더불어 생성된다.

#### 19.5.1 사용자 정의 생성자 함수와 프로토타입 생성 시점

- 생성자 함수로서 호출할 수 있는 함수, 즉 constructor는 함수 정의가 평가되어 함수 객체를 생성하는 시점에 프로토타입도 더불어 생성된다.

```js
// 함수 정의(constructor)가 평가되어 함수 객체를 생성하는 시점에 프로토타입도 더불어 생성된다.
console.log(Person.prototype); // {constructor: ƒ}

// 생성자 함수
function Person(name) {
  this.name = name;
}
```

함수 선언 시점에 이미 프로토타입이 잇다는 것을 확인할 수 있다. (아래 이미지)
![](https://velog.velcdn.com/images/hjthgus777/post/09a78c62-ff0d-45db-9ca9-c9dbc5a51f60/image.png)

<br>

> 생성자 함수로서 호출할수 없는 함수는 프로토타입이 생성되지 않는다.

```js
// 화살표 함수는 non-constructor다.
const Person = (name) => {
  this.name = name;
};

// non-constructor는 프로토타입이 생성되지 않는다.
console.log(Person.prototype); // undefined
```

<br>

---

### 19.6 객체 생성 방식과 프로토타입의 결정

- 다양한 방식으로 생성된 모든 객체는 각 방식마다 세부적인 객체 생성 방식의 차이는 있으나 추상 연산 OrdinaryObjectCreate 에 의해 생성된다는 공통점이 있다.

- 즉, 프로토타입은 OrdinaryObjectCreate에 전달되는 인수에 의해 결정된다.

- 이 인수는 객체가 생성되는 시점에 객체 생성 방식에 의해 결정된다.

---

[youtube]
![](https://velog.velcdn.com/images/hjthgus777/post/d28628ac-8a52-47c9-a50f-3df400f73d5f/image.png)

`생성자함수 Object`가 `인스턴스 obj`를 만들었을 때 `Object`가 `new 연산자`로 만든 것과 비슷하다. 또 `Object.prototype`이 obj에게 `[[Prototype]]`이 전달 된다.

---

#### 19.6.1 객체 리터럴에 의해 생성된 객체의 프로토타입

- 객체 리터럴에 의해 생성된 obj 객체는 Object.prototype을 프로토타입으로 갖게 되며, 이로써 Object.prototype을 상속받는다.

- obj객체는 constructor 프로퍼티와 hasOwnProperty 메서드 등을 소유하지 않지만 프로토타입인 Object.prototype의 constructor 프로퍼티와 hasOwnProperty 메서드를 자유롭게 사용할 수 있다.

- 이는 obj 객체가 자신의 프로토타입인 Object.prototype 객체를 상속 받았기 때문이다.

```js
const obj = { x: 1 };

// 객체 리터럴에 의해 생성된 obj 객체는 Object.prototype을 상속받는다.
console.log(obj.constructor === Object); // true
console.log(obj.hasOwnProperty("x")); // true
```

<br>

#### 19.6.2 Object 생성자 함수에 의해 생성된 객체의 프로토타입

- Object 생성자 함수에 의해 생성된 obj 객체는 Object.prototype을 프로토타입으로 갖게 된다.

```js
const obj = new Object();
obj.x = 1;

// Object 생성자 함수에 의해 생성된 obj 객체는 Object.prototype을 상속받는다.
console.log(obj.constructor === Object); // true
console.log(obj.hasOwnProperty("x")); // true
```

<br>

#### 19.6.3 생성자 함수에 의해 생성된 객체의 프로토타입

- 생성자 함수에 의해 생성되는 객체의 프로토타입은 생성자 함수의 prototype 프로퍼티에 바인딩되어 있는 객체다.

- `Person.prototype`에 프로퍼티를 추가하여 하위 객체가 상속받을 수 있도록 구현할 수도 있다.

```js
function Person(name) {
  this.name = name;
}

// 프로토타입 메서드
Person.prototype.sayHello = function () {
  console.log(`Hi! My name is ${this.name}`);
};

const me = new Person("Lee");
const you = new Person("Kim");

me.sayHello(); // Hi! My name is Lee
you.sayHello(); // Hi! My name is Kim
```

- Person 생성자 함수를 통해 생성된 모든 객체는 프로토타입에 추가된 `sayHello` 메서드를 상속받아 자신의 메서드처럼 사용할 수 있다.

![](https://velog.velcdn.com/images/hjthgus777/post/370e2c30-64b9-4749-9fd1-ac60b33aaa93/image.png)

<br>

---

### 19.7 프로토타입 체인

```js
function Person(name) {
  this.name = name;
}

// 프로토타입 메서드
Person.prototype.sayHello = function () {
  console.log(`Hi! My name is ${this.name}`);
};

const me = new Person("Lee");

// hasOwnProperty는 Object.prototype의 메서드다.
console.log(me.hasOwnProperty("name")); // true
```

- Pesron 생성자 함수에 의해 생성된 me 객체는 `Object.prototype`의 메서드인 `hasOwnProperty`를 호출할 수 있다.

- 이것은 me 객체가 `Person.prototype` 뿐만 아니라 `Object.prototype`도 상속 받았다는 것을 의미한다.

- me 객체의 프로토타입은 `Person.prototype` 이다.

```js
Object.getPrototypeOf(me) === Person.prototype; // -> true
```

- `Person.prototype` 의 프로토타입은 `Object.prototype`이다. 프로토타입의 프로토타입은 언제나 `Object.prototype`이다.

```js
Object.getPrototypeOf(Person.prototype) === Object.prototype; // -> true
```

---

> Q. me 에게는 `hasOwnProperty` 이 없다. 근데 `hasOwnProperty` 를 어떻게 쓸 수 있는가?

1. 프로토타입을 출력해보면 `hasOwnProperty` 이 있는 것을 확인할 수 있다.
   ![](https://velog.velcdn.com/images/hjthgus777/post/2c37bb43-a014-47ce-8edd-2aaf5a29857d/image.png)

2. me 를 보면 name 이라는 프로퍼티만 있어야하는데
   ![](https://velog.velcdn.com/images/hjthgus777/post/9989a2c5-a829-4240-bd52-61a7c594f52a/image.png)

상위에 `sayHello`가 있고, 또 그 상위에 `Object.prototype` 그 안에 `hasOwnProperty` 이 있는 것을 확인할 수 있다.

![](https://velog.velcdn.com/images/hjthgus777/post/7503ca4b-8f7e-4f01-abd0-e83376c79ce4/image.png)

정리하자면 me 가 `hasOwnProperty` 메서드를 실행하고자 할 때 맨 처음 가장 가까운 자기자신에게 `hasOwnProperty` 를 찾는다. 그런데 없다?
그럼 상위로 올라가 상위 생성자 함수 `Person`의 `[[Prototype]]`에서 `hasOwnProperty` 가 있는지 찾는다.

![](https://velog.velcdn.com/images/hjthgus777/post/7802819d-5086-48f8-80d6-1f588c8bbd88/image.png)

그러나 이곳에는 `sayHello` 뿐이다.
그럼 다시 상위로 올라가서 찾아본다.
![](https://velog.velcdn.com/images/hjthgus777/post/e245df55-49fd-4922-973c-d3f1bc50d6a6/image.png) 이곳에는 `hasOwnProperty` 가 있다. 발견했으니 실행한다!
![](https://velog.velcdn.com/images/hjthgus777/post/c32566ba-bc87-4dfa-a056-c90d6c78b98a/image.png)

---

자바스크립트는 객체의 프로퍼티에 접근하려고 할 때 해당 객체에 접근하려는 프로퍼티가 없다면 [[Prototype]] 내부 슬롯의 참조를 따라 자신의 부모 역할을 하는 프로토타입의 프로퍼티를 순차적으로 검색한다.

> 이를 `프로토타입 체인`이라고 한다. 프로토타입 체인은 자바스크립트가 객체지향 프로그래밍의 상속을 구현하는 매커니즘이다.

```js
// hasOwnProperty는 Object.prototype의 메서드다.
// me 객체는 프로토타입 체인을 따라 hasOwnProperty 메서드를 검색하여 사용한다.
me.hasOwnProperty("name"); // -> true
```

- 프로토타입 체인의 최상위에 위치하는 객체는 언제나 Object.prototype이다. 따라서 모든 객체는 Object.prototype을 상속받는다.

- Object.prototype을 `프로토타입 체인의 종점(end of prototype chain)`이라 한다.

- Object.prototype의 프로토타입, 즉 [[Prototype]] 내부 슬롯의 값은 null이다.

- 프로토타입 체인의 종점인 Object.prototpye에서도 프로퍼티를 검색할 수 없는 경우 undefined를 반환한다.

```js
console.log(me.foo); // undefined
```

- 프로토타입 체인은 상속과 프로퍼티 검색을 위한 메커니즘 이라고 할 수 있다.

- 프로퍼티가 아닌 식별자는 스코프 체인에서 검색한다. 즉 자바스크립트 엔진은 함수의 중첩 관계로 이루어진 스코프의 계층적 구조에서 식별자를 검색한다.

- 스코프 체인은 식별자 검색을 위한 메커니즘 이라 할 수 있다.

> 스코프 체인과 프로토타입 체인은 서로 연관없이 별도로 동작하는 것이 아니라 서로 협력하여 식별자와 프로퍼티를 검색하는데 사용된다.

### 19.8 오버라이딩과 프로퍼티 섀도잉

```js
const Person = (function () {
  // 생성자 함수
  function Person(name) {
    this.name = name;
  }

  // 프로토타입 메서드
  Person.prototype.sayHello = function () {
    console.log(`Hi! My name is ${this.name}`);
  };

  // 생성자 함수를 반환
  return Person;
})();

const me = new Person("Lee");

// 인스턴스 메서드
me.sayHello = function () {
  console.log(`Hey! My name is ${this.name}`);
};

// 인스턴스 메서드가 호출된다. 프로토타입 메서드는 인스턴스 메서드에 의해 가려진다.
me.sayHello(); // Hey! My name is Lee
```

`Person` 생성자 함수로 `객체(인스턴스)`를 생성한 다음, `인스턴스`에 메서드 `sayHello` 를 추가했다.

![](https://velog.velcdn.com/images/hjthgus777/post/062a1846-fa9f-492e-a01d-03ff2a5ee89d/image.png)

`프로토타입 프로퍼티` : 프로토타입이 소유한 프로퍼티(메서드 포함)
`인스턴스 프로퍼티` : 인스턴스가 소유한 프로퍼티

- 프로토타입 프로퍼티와 같은 이름의 프로퍼티를 인스턴스에 추가하면 프로토타입 체인을 따라 프로토타입 프로퍼티를 검색하여 프로토타입 프로퍼티를 덮어쓰는 것이 아니라 인스턴스 프로퍼티로 추가한다.

- 이때 인스턴스 메서드 sayHello는 프로토타입 메서드 sayHello 를 오버라이딩했고 프로토타입 메서드 sayHello는 는 가려진다.

> 이처럼 상속 관계에 의해 프로퍼티가 가려지는 현상을 `섀도잉`이라한다.

`오버라이딩`: 상위 클래스가 가지고 있는 메서드를 하위 클래스가 재정의하여 사용하는 방식

<br>

> 프로퍼티를 삭제하는 경우도 마찬가지다.

```js
// 인스턴스 메서드 sayHello를 삭제한다.
delete me.sayHello;
// 인스턴스에는 sayHello 메서드가 없으므로 프로토타입 메서드가 호출된다.
me.sayHello(); // Hi! My name is Lee
```

- 프로토타입 메서드가 아닌 `인스턴스 메서드 sayHello`가 삭제된다.

<br>

> 이번엔 다시 sayHello 메서드를 삭제하여 `프로토타입 메서드 sayHello` 를 삭제해보자.

```js
// 프로토타입 체인을 통해 프로토타입 메서드 sayHello 가 삭제되지 않는다.
delete me.sayHello;
// 프로토타입 메서드가 호출된다.
me.sayHello(); // Hi! My name is Lee
```

- 하위 객체를 통해 프로토타입의 프로퍼티를 변경 또는 삭제하는 것은 불가능하다.
- 즉 하위 객체를 통해 프로토타입에 `get 엑세스`는 허용되나 `set 엑세스`는 허용되지 않는다.

<br>

> 프로토타입 프로퍼티를 변경 또는 삭제하려면 하위 객체를 통해 프로토타입에 접근하는 것이 아니라 프로토타입에 직접 접근해야한다.

```js
// 프로토타입 메서드 변경
Person.prototype.sayHello = function () {
  console.log(`Hey! My name is ${this.name}`);
};
me.sayHello(); // Hey! My name is Lee

// 프로토타입 메서드 삭제
delete Person.prototype.sayHello;
me.sayHello(); // TypeError: me.sayHello is not a function
```

<br>

---

### 19.9 프로토타입의 교체

> 프로토타입은 임의의 다른 객체로 변경할 수 있다. 이것은 부모 객체인 프로토타입을 `동적`으로 변경할 수 있다는 것을 의미한다.

`객체 간의 상속관계`를 `동적`으로 변경하며 `프로토타입`은 `생성자 함수` 또는 `인스턴스`에 의해 `교체`할 수 있다.

#### 19.9.1 생성자 함수에 의한 프로토타입의 교체

```js
const Person = (function () {
  function Person(name) {
    this.name = name;
  }

  // ① 생성자 함수의 prototype 프로퍼티를 통해 프로토타입을 교체
  Person.prototype = {
    sayHello() {
      console.log(`Hi! My name is ${this.name}`);
    },
  };

  return Person;
})();

const me = new Person("Lee");
```

①에서 Person.prototype에 객체 리터럴을 할당했다. 이는 Person 생성자 함수가 생성할 객체의 프로토타입을 객체 리터럴로 교체한 것이다.

![](https://velog.velcdn.com/images/hjthgus777/post/9835ed2c-ba41-453f-8202-1c0c5c28ee6a/image.png)

- 프로토타입이 교체한 객체 리터럴에는 `constructor` 프로퍼티가 없다.
- constructor 프로퍼티는 자바스크립트 엔진이 프로토타입을 생성할 때 암묵적으로 추가한 프로퍼티다.
- 따라서 me 객체의 생성자 함수를 검색하면 Person이 아닌 `Object`가 나온다.

```js
// 프로토타입을 교체하면 constructor 프로퍼티와 생성자 함수 간의 연결이 파괴된다.
console.log(me.constructor === Person); // false
// 프로토타입 체인을 따라 Object.prototype의 constructor 프로퍼티가 검색된다.
console.log(me.constructor === Object); // true
```

- 위의 예제처럼 프로토타입을 교체하면 constuctor 프로퍼티와 생성자 함수간의 연결이 파괴된다.

---

파괴된 constructor 프로퍼티와 생성자 함수 간의 연결을 되살리려면 프로토타입으로 교체한 객체 리터럴에 constructor 프로퍼티를 추가하여 프로토타입의 constructor 프로퍼티를 되살린다.

```js
const Person = (function () {
  function Person(name) {
    this.name = name;
  }

  // 생성자 함수의 prototype 프로퍼티를 통해 프로토타입을 교체
  Person.prototype = {
    // constructor 프로퍼티와 생성자 함수 간의 연결을 설정
    constructor: Person,
    sayHello() {
      console.log(`Hi! My name is ${this.name}`);
    },
  };

  return Person;
})();

const me = new Person("Lee");

// constructor 프로퍼티가 생성자 함수를 가리킨다.
console.log(me.constructor === Person); // true
console.log(me.constructor === Object); // false
```

<br>

#### 19.9.2 인스턴스에 의한 프로토타입의 교체

인스턴스의 `__proto__` 접근자 프로퍼티를 통해 프로토타입을 교체하는 것은 이미 생성된 객체의 프로토타입을 교체하는 것이다.

```js
function Person(name) {
  this.name = name;
}

const me = new Person("Lee");

// 프로토타입으로 교체할 객체
const parent = {
  sayHello() {
    console.log(`Hi! My name is ${this.name}`);
  },
};

// ① me 객체의 프로토타입을 parent 객체로 교체한다.
Object.setPrototypeOf(me, parent);
// 위 코드는 아래의 코드와 동일하게 동작한다.
// me.__proto__ = parent;

me.sayHello(); // Hi! My name is Lee
```

위 코드를 그림으로 나타내면 아래와 같다.
![](https://velog.velcdn.com/images/hjthgus777/post/2eacd664-4df8-4f84-a27f-0806ecad4462/image.png)

- 프로토타입으로 교체한 객체에는 contructor 프로퍼티가 없으므로 constructor 프로퍼티와 생성자 함수 간의 연결이 파괴된다.

- 따라서 프로토타입의 contructor 프로퍼티로 me 객체의 생성자 함수를 검색하면 Person이 아닌 Object가 나온다.

```js
// 프로토타입을 교체하면 constructor 프로퍼티와 생성자 함수 간의 연결이 파괴된다.
console.log(me.constructor === Person); // false
// 프로토타입 체인을 따라 Object.prototype의 constructor 프로퍼티가 검색된다.
console.log(me.constructor === Object); // true
```

<br>
생성자 함수에 의한 프로토타입의 교체와 인스턴스에 의한 프로토타입 교체는 별다른 차이가 없지만 미묘한 차이가 존재한다. 그리고 프로토타입 교체를 통해 객체 간의 상속 관계를 동적으로 변경하는 것은 꽤나 번거롭기 때문에 프로토타입은 직접 교체하지 않는 것이 좋다.

상속 관계를 인위적으로 설정하려면 19.11절의 '직접 상속'이 더 편리하고 안전하다. 또는, ES6에서 도입된 클래스를 사용하면 간편하고 직관적인 상속관계를 구현할 수 있다.

---

### 19.10 instanceof 연산자

> `instanceof` 연산자는 생성자의 `prototype 속성`이 객체의 프로토타입 체인 어딘가 존재하는지 판별합니다. by. [MDN](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Operators/instanceof)

<br>

---

### 19.11 직접 상속

#### 19.11.1 Object.create에 의한 직접 상속

```js
Object.create(proto[, propertiesObject])
```

`proto`: 새로 만든 객체의 프로토타입이어야 할 객체.

`propertiesObject`: 선택사항. 지정되고 undefined가 아니면, 자신의 속성(즉, 자체에 정의되어 그 프로토타입 체인에서 열거가능하지 않은 속성)이 열거가능한 객체는 해당 속성명으로 새로 만든 객체에 추가될 속성 설명자(descriptor)를 지정합니다.  
[MDN](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Object/create)

---

`Object.create` : 명시적으로 프로토타입을 지정하여 새로운 객체를 생성함. 다른 객체 방식과 마찬가지로 추상 연산 OrdinaryObjectCreate를 호출한다.

- `첫 번째 매개변수` : 생성할 객체의 프로토타입으로 지정할 객체를 전달한다.

- `두 번째 매개변수` : 생성할 객체의 프로퍼티 키와, 프로퍼티 디스크립터 객체로 이뤄진 객체를 전달. (Object.defineProperties메서드의 두번째 인수와 동일). 옵션이므로 생략가능하다.

- 첫 번째 매개변수에 전달한 객체의 프로토타입 체인에 속하는 객체를 생성한다.

- 즉, 객체를 생성하면서 직접적으로 상속을 구현하는 것이다.

```js
// 프로토타입이 null인 객체를 생성한다. 생성된 객체는 프로토타입 체인의 종점에 위치한다.
// obj → null
let obj = Object.create(null);
console.log(Object.getPrototypeOf(obj) === null); // true
// Object.prototype을 상속받지 못한다.
console.log(obj.toString()); // TypeError: obj.toString is not a function

// obj → Object.prototype → null
// obj = {};와 동일하다.
obj = Object.create(Object.prototype);
console.log(Object.getPrototypeOf(obj) === Object.prototype); // true

// obj → Object.prototype → null
// obj = { x: 1 };와 동일하다.
obj = Object.create(Object.prototype, {
  x: { value: 1, writable: true, enumerable: true, configurable: true },
});
// 위 코드는 다음과 동일하다.
// obj = Object.create(Object.prototype);
// obj.x = 1;
console.log(obj.x); // 1
console.log(Object.getPrototypeOf(obj) === Object.prototype); // true

const myProto = { x: 10 };
// 임의의 객체를 직접 상속받는다.
// obj → myProto → Object.prototype → null
obj = Object.create(myProto);
console.log(obj.x); // 10
console.log(Object.getPrototypeOf(obj) === myProto); // true

// 생성자 함수
function Person(name) {
  this.name = name;
}

// obj → Person.prototype → Object.prototype → null
// obj = new Person('Lee')와 동일하다.
obj = Object.create(Person.prototype);
obj.name = "Lee";
console.log(obj.name); // Lee
console.log(Object.getPrototypeOf(obj) === Person.prototype); // true
```

#### Object.create 메서드의 장점

- new 연산자가 없이도 객체를 생성할 수 있다.

- 프로토타입을 지정하면서 객체를 생성할 수 있다.

- 객체 리터럴에 의해 생성된 객체도 상속받을 수 있다.

<br>

#### Object.prototype의 빌트인 메서드

`Object.prototype.hasOwnProperty`, `Object.prototype.isPrototypeOf`, `Object.prototype.propertyIsEnumerable`등은 **모든 객체의 프로토타입 체인의 종점**, 즉 `Object.prototpye`의 메서드이므로 모든 객체가 상속받아 호출할 수 있다.

```js
const obj = { a: 1 };

obj.hasOwnProperty("a"); // -> true
obj.propertyIsEnumerable("a"); // -> true
```

그러나 `ESLint`에서는 `Object.prototype`의 빌트인 메서드를 객체가 **직접 호출하는 것을 권장하지 않는다.** `Object.create` 메서드를 통해 프로토타입 체인의 종점에 위치하는 객체를 생성할 수 있기 때문이다.

> 프로토타입 체인의 종점에 위치하는 객체는 `Object.prototype`의 빌트인 메서드를 사용할 수 없다.

```js
// 프로토타입이 null인 객체, 즉 프로토타입 체인의 종점에 위치하는 객체를 생성한다.
const obj = Object.create(null);
obj.a = 1;

console.log(Object.getPrototypeOf(obj) === null); // true

// obj는 Object.prototype의 빌트인 메서드를 사용할 수 없다.
console.log(obj.hasOwnProperty("a")); // TypeError: obj.hasOwnProperty is not a function
```

따라서 이 같은 에러를 발생시킬 위험을 없애기 위해 `Object.prototype`의 빌트인 메서드는 다음과 같이 간접적으로 호출하는 것이 좋다.

```js
// 프로토타입이 null인 객체를 생성한다.
const obj = Object.create(null);
obj.a = 1;

// console.log(obj.hasOwnProperty('a')); // TypeError: obj.hasOwnProperty is not a function

// Object.prototype의 빌트인 메서드는 객체로 직접 호출하지 않는다.
console.log(Object.prototype.hasOwnProperty.call(obj, "a")); // true
// Fucntion.prototype.call 메서드는 22.2.4절에서 살펴본다. (Function.prototype.apply/call/bind)
```

[MDN](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Object/create)

<br>

#### 19.11.2 객체 리터럴 내부에서 \_**\_proto\_\_**에 의한 직접 상속

`ES6`에서는 객체 리터럴 내부에서 `__proto__`접근자 프로퍼티를 사용하여 직접 상속을 구현할 수 있다.

```js
const myProto = { x: 10 };

// 객체 리터럴에 의해 객체를 생성하면서 프로토타입을 지정하여 직접 상속받을 수 있다.
const obj = {
  y: 20,
  // 객체를 직접 상속받는다.
  // obj → myProto → Object.prototype → null
  __proto__: myProto,
};
/* 위 코드는 아래와 동일하다.
const obj = Object.create(myProto, {
  y: { value: 20, writable: true, enumerable: true, configurable: true }
});
*/

console.log(obj.x, obj.y); // 10 20
console.log(Object.getPrototypeOf(obj) === myProto); // true
```

<br>

---

### 19.12 정적 프로퍼티/ 메서드

`정적 프로퍼티/메서드`는 생성자 함수로 인스턴스를 생성하지 않아도 `참조/호출`할 수 있는 `프로퍼티/메서드`를 말한다.

```js
// 생성자 함수
function Person(name) {
  this.name = name;
}

// 프로토타입 메서드
Person.prototype.sayHello = function () {
  console.log(`Hi! My name is ${this.name}`);
};

// 정적 프로퍼티
Person.staticProp = "static prop";

// 정적 메서드
Person.staticMethod = function () {
  console.log("staticMethod");
};

const me = new Person("Lee");

// 생성자 함수에 추가한 정적 프로퍼티/메서드는 생성자 함수로 참조/호출한다.
Person.staticMethod(); // staticMethod

// 정적 프로퍼티/메서드는 생성자 함수가 생성한 인스턴스로 참조/호출할 수 없다.
// 인스턴스로 참조/호출할 수 있는 프로퍼티/메서드는 프로토타입 체인 상에 존재해야 한다.
me.staticMethod(); // TypeError: me.staticMethod is not a function
```

- Person 생성자 함수는 객체이므로 자신의 프로퍼티/메서드를 소유할 수 있다.

- Person 생성자 함수 객체가 소유한 프로퍼티/메서드를 정적 프로퍼티/메서드라고 한다.

- 정적 프로퍼티/메서드는 생성자 함수가 생성한 인스턴스로 참조/호출할 수 없다.

- 생성자 함수가 생성한 인스턴스는 자신의 프로토타입 체인에 속한 객체의 프로퍼티/메서드에 접근이 가능하다.

- 정적 프로퍼티/메서드는 인스턴스의 프로토타입 체인에 속한 객체의 프로퍼티/메서드가 아니므로 인스턴스로 접근 불가하다.

<br>

---

### 19.13 프로퍼티 존재 확인

#### 19.13.1 in 연산자

`in` 연산자는 객체 내에 특정 프로퍼티가 존재하는지 여부를 확인한다.

```js
key in object;
// key: 프로퍼티 키를 나타내는 문자열
// object: 객체로 평가되는 표현식
```

[MDN](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Operators/in)

```js
속성 in 객체명;
// 속성: 속성의 이름이나 배열의 인덱스를 뜻하는 문자열 또는 수 값
// 객체명: 객체의 이름
```

<br>

```js
const person = {
  name: "Lee",
  address: "Seoul",
};

// person 객체에 name 프로퍼티가 존재한다.
console.log("name" in person); // true
// person 객체에 address 프로퍼티가 존재한다.
console.log("address" in person); // true
// person 객체에 age 프로퍼티가 존재하지 않는다.
console.log("age" in person); // false
```

- 확인 대상 객체의 프로퍼티와, 확인 대상 객체가 상속받는 모든 프로토타입의 프로퍼티를 확인한다.

- 아래 예시코드에서 `person`객체는 `toString`이라는 프로퍼티가 없지만 `'toString' in person` 은 `true`를 반환한다.

```js
console.log("toString" in person); //true

const person = { name: "Lee" };

console.log(Reflect.has(person, "name")); // true
console.log(Reflect.has(person, "toString")); // true
```

`person` 객체가 속한 `프로토타입 체인` 상 존재하는 `모든 프로토타입`에서 `toString` 프로퍼티를 검색했기 때문. `toString`은 `Object.prototpye`의 메서드!

<br>

#### 19.13.2 Object.prototype.hasOwnProperty 메서드

`Object.prototype.hasOwnProperty` 메서드를 사용해도 객체에 특정 프로퍼티가 존재하는지 확인할 수 있다.

- `인수`로 전달받은 프로퍼티 `키가 객체의 고유의 프로퍼티 키`인 경우에만 `true`를 반환하고 `상속받은 프로토타입의 프로퍼티 키`인 경우 `false`를 반환한다.

```js
console.log(person.hasOwnProperty("name")); // true
console.log(person.hasOwnProperty("age")); // false

// 상속받은 프로토타입의 프로퍼티 키 toString
console.log(person.hasOwnProperty("toString")); // false
```

<br>

---

### 19.14 프로퍼티 열겨

#### 19.14.1 for...in 문

`for...in` 문 객체의 모든 프로퍼티를 열거할 때 사용한다.

```js
for(변수 선언문 in 객체) {...}
```

<br>

```js
const person = {
  name: "Lee",
  address: "Seoul",
};

// for...in 문의 변수 prop에 person 객체의 프로퍼티 키가 할당된다.
for (const key in person) {
  console.log(key + ": " + person[key]);
}
// name: Lee
// address: Seoul
```

- `for ... in` 문은 객체의 프로퍼티 개수만큼 순회한다.

- `for ... in` 문의 변수 선언문에서 선언한 변수 key 에 프로퍼티 키를 할당한다.

- `for ... in` 문은 프로퍼티가 심벌인 프로퍼티는 열거하지 않는다.

- 상속받은 프로퍼티는 제외하고 객체 자신의 프로퍼티만 열거하려면 `Object.prototype.hasOwnProperty` 메서드를 사용해야한다.

- 프로퍼티를 열거할 때 순서를 보장하지 않는다.

- `배열`에는 `for...in`문보다 일반 `for`문이나 `for...of`문 또는 `Array.prototpye.forEach`메서드를 사용하길 권장한다.

<br>

---

`for ... in` 문은 상속받은 프로토타입의 프로퍼티까지 열거한다. 하지만 `toString`과 같은 `Object.prototype`의 프로퍼티가 열거되지 않는다.

```js
const person = {
  name: "Lee",
  address: "Seoul",
};

// in 연산자는 객체가 상속받은 모든 프로토타입의 프로퍼티를 확인한다.
console.log("toString" in person); // true

// for...in 문도 객체가 상속받은 모든 프로토타입의 프로퍼티를 열거한다.
// 하지만 toString과 같은 Object.prototype의 프로퍼티가 열거되지 않는다.
for (const key in person) {
  console.log(key + ": " + person[key]);
}

// name: Lee
// address: Seoul
```

이는 `toString` 메서드가 열거할 수 없도록 즉 `[[Enumerable]]`의 값이 `false`이기 때문이다.
`[[Enumerable]]` : 열거 가능 여부, `boolean` 값을 가짐

```js
// Object.getOwnPropertyDescriptor 메서드는 프로퍼티 디스크립터 객체를 반환한다.
// 프로퍼티 디스크립터 객체는 프로퍼티 어트리뷰트 정보를 담고 있는 객체다.
console.log(Object.getOwnPropertyDescriptor(Object.prototype, "toString"));
// {value: ƒ, writable: true, enumerable: false 📌, configurable: true}
```

> `for ... in` 문은 객체의 `프로토타입 체인` 상에 존재하는 모든 프로토타입의 프로퍼티 중에서 프로퍼티 어트리뷰트 `[[Enumerable]]` 의 값이 `true`인 프로퍼티를 순회하며 열거한다.

<br>

#### 19.14.2 Object.keys/values/entries 메서드

`for ... in` 문은 객체 자신의 고유 프로퍼티뿐 아니라 상속받은 프로퍼티도 열거한다.

**객체 자신의 고유 프로퍼티만** 열거하기 위해서는 `for ... in` 문을 사용하는 것보다. `Object.keys()`, `Object.values()`, `Object.entries()` 메서드를 사용하는 것을 권장한다.

#### object.key()

객체 자신의 열거 가능한 프로퍼티 키를 배열로 반환한다.

```js
let user = {
  name: "훈이",
  age: 20,
};

console.log(Object.keys(user));
// ['name', 'age']
```

<br>

#### object.values()

ES8에서 도입된 메서드이며 객체 자신의 열거 가능한 프로퍼티 값을 배열로 반환한다.

```js
let user = {
  name: "훈이",
  age: 20,
};

console.log(Object.values(user));
// ['훈이', 20]
```

<br>

#### object.entries()

ES8에서 도입된 메서드이며 객체 자신의 열거 가능한 프로퍼티 [키, 값]의 쌍의 배열을 배열에 담아 반환한다.

```js
let user = {
  name: "훈이",
  age: 20,
};

console.log(Object.entries(user));
// ["name", "훈이"], ["age", 20]
```
