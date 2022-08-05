## 34.1 이터레이션 프로토콜

ES6에서 도입된 이터레이션 프로토콜은 순회 가능한 데이터 컬렉션(자료구조)을 만들기 위해 ECMAScript 사양에 정의하여 미리 약속한 규칙이다.

ES6 이전의 순회 가능한 데이터 컬렉션, 즉 배열, 문자열, 유사 배열 객체, DOM 컬렉션 등은 통일된 규약없이 각자 나름의 구조를 가지고 for 문, for...in 문, forEach 메서드 등 다양한 방법으로 순회할 수 있엇다.

ES6에서는 순회 가능한 데이터 컬렉션을 **이터레이션 프로토콜을 준수하는 이터러블로 통일**하여 **for...of 문, 스프레드 문법, 배열 디스트럭처링 할당의 대상으로 사용할 수 있도록 일원화**했다.

이터레이션 프로토콜에는 `이터러블 프로토콜`과 `이터레이터 프로토콜`이 있다.

### 34.1.1 이터러블

**이터러블 프로토콜을 준수한 객체**를 이터러블이라 한다. 즉, 이터러블은 Symbol.iterator를 프로터키 키로 사용한 메서드를 직접 구현하거나 프로토타입 체인을 통해 상속받은 객체를 말한다.

![](https://velog.velcdn.com/images/dooin/post/e7631a13-cd40-471a-8e4f-510436fa4cec/image.png)

**예제 34-01**

```jsx
const isIterable = (v) =>
    v !== null && typeof v[Symbol.iterator] === "function";

// 배열, 문자열, Map, Set 등은 이터러블이다.
isIterable([]); // -> true
isIterable(""); // -> true
isIterable(new Map()); // -> true
isIterable(new Set()); // -> true
isIterable({}); // -> false
```

이터러블인지 확인하는 함수는 예제 01코드와 같이 구현할 수 있다.

**예제 34-02**

```jsx
const array = [1, 2, 3];

// 배열은 Array.prototype의 Symbol.iterator 메서드를 상속받는 이터러블이다.
console.log(Symbol.iterator in array); // true

// 이터러블인 배열은 for...of 문으로 순회 가능하다.
for (const item of array) {
    console.log(item);
}

// 이터러블인 배열은 스프레드 문법의 대상으로 사용할 수 있다.
console.log([...array]); // [1, 2, 3]

// 이터러블인 배열은 배열 디스트럭처링 할당의 대상으로 사용할 수 있다.
const [a, ...rest] = array;
console.log(a, rest); // 1, [2, 3]
```

이터러블은 for...of 문으로 순회할 수 있으며, 스프레드 문법과 배열 디스트럭처링 할당의 대상으로 사용할 수 있다.

**예제 34-03**

```jsx
const obj = { a: 1, b: 2 };

// 일반 객체는 Symbol.iterator 메서드를 구현하거나 상속받지 않는다.
// 따라서 일반 객체는 이터러블 프로토콜을 준수한 이터러블이 아니다.
console.log(Symbol.iterator in obj); // false

// 이터러블이 아닌 일반 객체는 for...of 문으로 순회할 수 없다.
for (const item of obj) {
    // -> TypeError: obj is not iterable
    console.log(item);
}

// 이터러블이 아닌 일반 객체는 배열 디스트럭처링 할당의 대상으로 사용할 수 없다.
const [a, b] = obj; // -> TypeError: obj is not iterable
```

일반 객체는 for...of 문으로 순회할 수 없으며 스프레드 문법과 배열 디스트럭처링 할당의 대상으로 사용할 수 없다.

### 34.1.2 이터레이터

**예제 34-05**

```jsx
// 배열은 이터러블 프로토콜을 준수한 이터러블이다.
const array = [1, 2, 3];

// Symbol.iterator 메서드는 이터레이터를 반환한다.
const iterator = array[Symbol.iterator]();

// Symbol.iterator 메서드가 반환한 이터레이터는 next 메서드를 갖는다.
console.log("next" in iterator); // true
```

이터러블의 Symbol.iterator 메서드를 호출하면 이터레이터 프로토콜을 준수한 이터레이터를 반환한다. **이터러블의 Symbol.iterator 메서드가 반환한 이터레이터는 next 메서드를 갖는다.**

![](https://velog.velcdn.com/images/dooin/post/385ebb65-c18b-45ca-953d-a0a6b9b7008d/image.png)

**예제 34-06**

```jsx
// 배열은 이터러블 프로토콜을 준수한 이터러블이다.
const array = [1, 2, 3];

// Symbol.iterator 메서드는 이터레이터를 반환한다. 이터레이터는 next 메서드를 갖는다.
const iterator = array[Symbol.iterator]();

// next 메서드를 호출하면 이터러블을 순회하며 순회 결과를 나타내는 이터레이터 리절트 객체를
// 반환한다. 이터레이터 리절트 객체는 value와 done 프로퍼티를 갖는 객체다.
console.log(iterator.next()); // { value: 1, done: false }
console.log(iterator.next()); // { value: 2, done: false }
console.log(iterator.next()); // { value: 3, done: false }
console.log(iterator.next()); // { value: undefined, done: true }
```

이터레이터는 next 메서드는 이터러블의 각 요소를 순회하기 위한 포인터의 역할을 한다. 즉, next 메서드를 호출하면 이터러블을 순차적으로 한 단계식 순회하며 순회 결과를 나타내는 이**터레이트 리절트 객체를 반환**한다.

이러한 규약을 이터레이터 프로토콜이라 하며, **이터레이터 프로토콜을 준수한 객체를 이터레이터라 한다.** 이터레이터는 이터러블의 요소를 탐색하기 위한 포인터 역할을 한다.

![](https://velog.velcdn.com/images/dooin/post/3434110e-a0ff-4777-ab5f-5a5783aa9155/image.png)

-   이터러블 객체는 range 이다. **Symbol.iterator 메서드를 가지고 있기 때문이다.**
-   Symbol.iterator() 메서드에서 리턴한 객체가 바로 이터레이터다. **이 객체 안에는 {value : 값 , done : true/false}를 리턴하는 next()메서드가 있기 때문이다.**

---

## 34.2 빌트인 이터러블

자바스크립트는 이터레이션 프로토콜을 준수한 객체인 빌트인 이터러블을 제공한다. 다음의 표준 빌트인 객체들은 빌트인 이터러블이다.

![](https://velog.velcdn.com/images/dooin/post/5f505059-805e-4939-9311-8d3c6d21a808/image.png)

---

## 34.3 for...of 문

for...of 문은 이터러블을 순회하면서 이터러블의 요소를 변수에 할당한다. for...of 문의 문법은 다음과 같다.

```jsx
for (변수선언문 of 이터러블) {...}
```

for...of 문은 내부적으로 이터레이터의 next 메서드를 통해 순회하는데 next가 반환하는 리절트 객체의 value 프로퍼티를 for...of 문 변수에 할당한다. 이때, 순회는 리절트 객체의 done 프로퍼티의 값이 true일 때까지 반복한다.

---

## 34.4 이터러블과 유사 배열 객체

**예제 34-11**

```jsx
// 유사 배열 객체
const arrayLike = {
    0: 1,
    1: 2,
    2: 3,
    length: 3,
};

// Array.from은 유사 배열 객체 또는 이터러블을 배열로 변환한다
const arr = Array.from(arrayLike);
console.log(arr); // [1, 2, 3]
```

유사 배열 객체는 각 프로퍼티에 인덱스로 접근할 수 있고 length 프로퍼티를 갖는 객체를 의미한다.

유사 배열 객체는 length 프로퍼티를 갖기 때문에 for 문으로 순회할 수 있고, 인덱스를 나타내는 숫자 형식의 문자열을 프로퍼티 키로 가지므로 마치 배열처럼 인덱스로 프로퍼티 값에 접근할 수 있다.

하지만 **일반 객체이기 때문에 Symbol.iterator 메서드가 없고, for...of 문으로 순회할 수 없다.**

단, arguments, NodeList, HTMLCollection은 유사 배열 객체이면서 이터러블이기도 하다. 배열도 마찬가지로 ES6에서 이터러블이 도입되면서 Symbol.iterator 메서드를 구현하여 이터러블이 되었다.

예제 11의 arrayLike 객체는 유사 배열 객체지만 이터러블이 아니다. 다만 ES6에서 도입된 `Array.from` 메서드를 사용하여 간단히 배열로 변환할 수 있다.

---

## 34.5 이터레이션 프로토콜의 필요성

이터러블은 for...of 문, 스프레드 문법, 배열 디스트럭처링 할당과 같은 데이터 소비자(data consumer)에 의해 사용되므로 데이터 공급자(data provider)의 역할을 한다고 할 수 있다.

만약 다양한 데이터 공급자가 각자의 순회 방식을 가진다면 데이터 소비자는 다양한 데이터 공급자의 순회방식을 모두 지원해야 한다. 이는 효율적이지 않다.

하지만 다양한 데이터 공급자가 이터레이션 프로토콜을 준수하도록 규정하면 데이터 소비자는 이터레이션 프로토콜만 지원하도록 구현하면 된다.

이처럼 이터레이션 프로토콜은 다양한 데이터 공급자가 하나의 순회 방식을 갖도록 규정하여 데이터 소비자가 효율적으로 다양한 데이터 공급자를 사용할 수 있도록 데이터 소비자와 공급자를 연결하는 인터페이스의 역할을 한다.

![](https://velog.velcdn.com/images/dooin/post/dc530ab0-b1c4-4984-be1a-d87aeff685aa/image.png)

---

## 34.6 사용자 정의 이터러블

예제참고
