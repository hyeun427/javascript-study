## 22.1 this 키워드

객체는 **상태를 나타내는 프로퍼티**와 **동작을 나타내는 메서드**를 하나의 **논리적인 단위로 묶은 복합적인 자료구조**다. 메서드는 프로퍼티를 참조하고 변경할 수 있어야하는데, 자신이 속한 객체의 프로퍼티를 참조하려면 자신이 속한 객체를 가리키는 식별자를 참조할 수 있어야 한다.

**예제 22-02**

```jsx
function Circle(radius) {
  // 이 시점에는 생성자 함수 자신이 생성할 인스턴스를 가리키는 식별자를 알 수 없다.
  ????.radius = radius;
}

Circle.prototype.getDiameter = function () {
  // 이 시점에는 생성자 함수 자신이 생성할 인스턴스를 가리키는 식별자를 알 수 없다.
  return 2 * ????.radius;
};

// 생성자 함수로 인스턴스를 생성하려면 먼저 생성자 함수를 정의해야 한다.
const circle = new Circle(5);
```

예제 02 코드를 보면, 생성자 함수 내부에서는 프로퍼티 또는 메서드를 추가하기 위해 **자신이 생성할 인스턴스를 참조할 수 있어야 한다.** 하지만 생성자 함수에 의한 객체 생성 방식은 먼저 생성자 함수를 정의한 이후 new 연산자와 함께 생성자 함수를 호출하는 단계가 추가로 필요하다. 다시 말해, **생성자 함수로 인스턴스를 생성하려면 먼저 생성자 함수가 존재**해야 한다.

생성자 함수를 정의하는 시점에는 아직 인스턴스를 생성하기 이전이므로 생성자 함수가 생성할 인스턴스를 가리키는 식별자를 알 수 없다. 따라서 **자신이 속한 객체 또는 자신이 생성할 인스턴스를 가리키는 특수한 식별자가 필요**하다. 이를 위해 자바스크립트는 this라는 특수한 식별자를 제공한다.

this는 자신이 속한 객체 또는 자신이 생성할 인스턴스를 가리키는 자기 참조 변수다. **this를 통해 자신이 속한 객체 또는 자신이 생성할 인스턴스의 프로퍼티나 메서드를 참조할 수 있다.**

this는 자바스크립트 엔진에 의해 암묵적으로 생성되고, 코드 어디서든 참조할 수 있으며, **this가 가리키는 값(this 바인딩)은 함수 호출 방식에 의해 동적으로 결정된다.**

> 바인딩이랑 식별자와 값을 연결하는 과정을 의미한다. 예를 들어 변수 선언은 변수 이름(식별자)와 확보된 메모리 공간의 주소를 바인딩하는것.

**예제 22-03**

```jsx
// 생성자 함수
function Circle(radius) {
    // this는 생성자 함수가 생성할 인스턴스를 가리킨다.
    this.radius = radius;
}

Circle.prototype.getDiameter = function () {
    // this는 생성자 함수가 생성할 인스턴스를 가리킨다.
    return 2 * this.radius;
};

// 인스턴스 생성
const circle = new Circle(5);
console.log(circle.getDiameter()); // 10
```

생성자 함수 내부의 this는 생성자 함수가 생성할 인스턴스를 가리킨다. 이처럼 this는 상황에 따라 가리키는 대상이 다르다.

자바나 C++ 같은 클래스 기반 언어에서 this는 언제나 클래스가 생성하는 인스턴스를 가리키지만, **자바스크립트의 this는 함수가 호출되는 방식에 따라 this가 바인딩될 값, 즉 this 바인딩이 동적으로 결정된다. **

**예제 22-05**

```jsx
// this는 어디서든지 참조 가능하다.
// 전역에서 this는 전역 객체 window를 가리킨다.
console.log(this); // window

function square(number) {
    // 일반 함수 내부에서 this는 전역 객체 window를 가리킨다.
    console.log(this); // window
    return number * number;
}
square(2);

const person = {
    name: "Lee",
    getName() {
        // 메서드 내부에서 this는 메서드를 호출한 객체를 가리킨다.
        console.log(this); // {name: "Lee", getName: ƒ}
        return this.name;
    },
};
console.log(person.getName()); // Lee

function Person(name) {
    this.name = name;
    // 생성자 함수 내부에서 this는 생성자 함수가 생성할 인스턴스를 가리킨다.
    console.log(this); // Person {name: "Lee"}
}

const me = new Person("Lee");
```

this는 객체의 프로퍼티나 메서드를 참조하기 위한 자기 참조 변수이므로 일반적으로 객체의 메서드 내부 또는 생성자 함수 내부에서만 의미가 있다. 따라서 strict mode가 적용된 일반 함수 내부의 this에는 undefined가 바인딩 된다.

---

## 22.2 함수 호출 방식과 this 바인딩

this바인딩은 함수 호출 방식, 즉 함수가 어떻게 호출되었는지에 따라 동적으로 결정된다.

> 1. 일반 함수 호출
> 2. 메서드 호출
> 3. 생성자 함수 호출
> 4. Function 객체의 메서드에 의한 호출

**예제 22-06**

```jsx
// this 바인딩은 함수 호출 방식에 따라 동적으로 결정된다.
const foo = function () {
    console.dir(this);
};

// 동일한 함수도 다양한 방식으로 호출할 수 있다.

// 1. 일반 함수 호출
// foo 함수를 일반적인 방식으로 호출
// foo 함수 내부의 this는 전역 객체 window를 가리킨다.
foo(); // window

// 2. 메서드 호출
// foo 함수를 프로퍼티 값으로 할당하여 호출
// foo 함수 내부의 this는 메서드를 호출한 객체 obj를 가리킨다.
const obj = { foo };
obj.foo(); // obj

// 3. 생성자 함수 호출
// foo 함수를 new 연산자와 함께 생성자 함수로 호출
// foo 함수 내부의 this는 생성자 함수가 생성한 인스턴스를 가리킨다.
new foo(); // foo {}

// 4. Function.prototype.apply/call/bind 메서드에 의한 간접 호출
// foo 함수 내부의 this는 인수에 의해 결정된다.
const bar = { name: "bar" };

foo.call(bar); // bar
foo.apply(bar); // bar
foo.bind(bar)(); // bar
```

### 22.2.1 일반 함수 호출

기본적으로 this에는 전역 객체가 바인딩 된다.

**예제 22-07**

```jsx
function foo() {
    console.log("foo's this: ", this); // window
    function bar() {
        console.log("bar's this: ", this); // window
    }
    bar();
}
foo();
```

전역 함수는 물론, 중첩 함수를 일반 함수로 호출하면 함수 내부의 this에는 전역 객체가 바인딩된다.

**예제 22-08**

```jsx
function foo() {
    "use strict";

    console.log("foo's this: ", this); // undefined
    function bar() {
        console.log("bar's this: ", this); // undefined
    }
    bar();
}
foo();
```

strict mode가 적용된 일반 함수 내부의 this에는 undefined가 바인딩 된다.

**예제 22-09**

```jsx
// var 키워드로 선언한 전역 변수 value는 전역 객체의 프로퍼티다.
var value = 1;
// const 키워드로 선언한 전역 변수 value는 전역 객체의 프로퍼티가 아니다.
// const value = 1;

const obj = {
    value: 100,
    foo() {
        console.log("foo's this: ", this); // {value: 100, foo: ƒ}
        console.log("foo's this.value: ", this.value); // 100

        // 메서드 내에서 정의한 중첩 함수
        function bar() {
            console.log("bar's this: ", this); // window
            console.log("bar's this.value: ", this.value); // 1
        }

        // 메서드 내에서 정의한 중첩 함수도 일반 함수로 호출되면 중첩 함수 내부의 this에는 전역 객체가 바인딩된다.
        bar();
    },
};

obj.foo();
```

**예제 22-10**

```jsx
var value = 1;

const obj = {
    value: 100,
    foo() {
        console.log("foo's this: ", this); // {value: 100, foo: ƒ}
        // 콜백 함수 내부의 this에는 전역 객체가 바인딩된다.
        setTimeout(function () {
            console.log("callback's this: ", this); // window
            console.log("callback's this.value: ", this.value); // 1
        }, 100);
    },
};

obj.foo();
```

정리하자면 일반 함수로 호출된 모든 함수(중첩 함수, 콜백 함수 포함) 내부의 this에는 전역 객체가 바인딩된다.

## 22.2.2 메서드 호출

메서드 내부의 this에는 메서드를 호출한 객체, 즉 **메서드를 호출할 때 메서드 이름 앞의 마침표(.) 연산자 앞에 기술한 객체가 바인딩 된다.** 주의할 것은 메서드 내부의 this는 메서드를 소유한 객체가 아닌 메서드를 **호출한 객체에 바인딩** 된다는 것이다.

**예제 22-14**

```jsx
const person = {
    name: "Lee",
    getName() {
        // 메서드 내부의 this는 메서드를 호출한 객체에 바인딩된다.
        return this.name;
    },
};

// 메서드 getName을 호출한 객체는 person이다.
console.log(person.getName()); // Lee
```

**예제 22-15**

```jsx
const anotherPerson = {
    name: "Kim",
};
// getName 메서드를 anotherPerson 객체의 메서드로 할당
anotherPerson.getName = person.getName;

// getName 메서드를 호출한 객체는 anotherPerson이다.
console.log(anotherPerson.getName()); // Kim

// getName 메서드를 변수에 할당
const getName = person.getName;

// getName 메서드를 일반 함수로 호출
console.log(getName()); // ''
// 일반 함수로 호출된 getName 함수 내부의 this.name은 브라우저 환경에서 window.name과 같다.
// 브라우저 환경에서 window.name은 브라우저 창의 이름을 나타내는 빌트인 프로퍼티이며 기본값은 ''이다.
// Node.js 환경에서 this.name은 undefined다.
```

![](https://velog.velcdn.com/images/dooin/post/959b9565-9248-4b26-880c-1d50cda932a8/image.png)

**예제 22-16**

```jsx
function Person(name) {
    this.name = name;
}

Person.prototype.getName = function () {
    return this.name;
};

const me = new Person("Lee");

// getName 메서드를 호출한 객체는 me다.
console.log(me.getName()); // ① Lee

Person.prototype.name = "Kim";

// getName 메서드를 호출한 객체는 Person.prototype이다.
console.log(Person.prototype.getName()); // ② Kim
```

예제 16번 ①의 경우 getName 메서드를 호출한 객체는 me다. 따라서 메서드 내부의 this는 me를 가리키며 this.name은 "lee"다.

②의 경우 getName 메서드를 호출한 객체는 Person.prototype이다. Person.ptototype도 객체이므로 직접 메서드를 호출할 수 있다. 따라서 getName 메서드 내부의 this는 Person.prototype을 가리키며 this.name은 "kim"이다.

## 22.2.3 생성자 함수 호출

생성자 함수 내부의 this에는 생성자 함수가 (미래에) 생성할 인스턴스가 바인딩된다.

**예제22-17**

```jsx
// 생성자 함수
function Circle(radius) {
    // 생성자 함수 내부의 this는 생성자 함수가 생성할 인스턴스를 가리킨다.
    this.radius = radius;
    this.getDiameter = function () {
        return 2 * this.radius;
    };
}

// 반지름이 5인 Circle 객체를 생성
const circle1 = new Circle(5);
// 반지름이 10인 Circle 객체를 생성
const circle2 = new Circle(10);

console.log(circle1.getDiameter()); // 10
console.log(circle2.getDiameter()); // 20
```

**예제 22-18**

```jsx
// new 연산자와 함께 호출하지 않으면 생성자 함수로 동작하지 않는다. 즉, 일반적인 함수의 호출이다.
const circle3 = Circle(15);

// 일반 함수로 호출된 Circle에는 반환문이 없으므로 암묵적으로 undefined를 반환한다.
console.log(circle3); // undefined

// 일반 함수로 호출된 Circle 내부의 this는 전역 객체를 가리킨다.
console.log(radius); // 15
```

new 연산자와 함께 호출하면 해당 함수는 생성자 함수로 동작하고, new 연산자와 함께 생성자 함수를 호출하지 않으면 생성자 함수가 아닌 일반 함수로 동작한다.

![](https://velog.velcdn.com/images/dooin/post/6a7ad6fc-50a8-4e0b-9af3-959e4769e779/image.png)

생성자 함수는 사진과 같은 알고리즘으로 동작한다. this라는 빈객체를 생성하고, 함수 내용의 프로퍼티를 할당 후 this를 반환한다.

## 22.2.4 Function.prototype.apply/call/bind 메서드에 의한 간접 호출

Function객체는 this를 명시적으로 바인딩할 수 있는 메서드들을 제공한다.
apply, call 메서드는 this로 사용할 객체와 인수 리스트를 인수로 전달받아 함수를 호출한다.

**예제 22-19**

```jsx
function getThisBinding() {
    return this;
}

// this로 사용할 객체
const thisArg = { a: 1 };

console.log(getThisBinding()); // window

// getThisBinding 함수를 호출하면서 인수로 전달한 객체를 getThisBinding 함수의 this에 바인딩한다.
console.log(getThisBinding.apply(thisArg)); // {a: 1}
console.log(getThisBinding.call(thisArg)); // {a: 1}
```

**apply와 call 메서드의 본질적인 기능은 함수를 호출**하는 것이다.

**예제 22-20**

```jsx
function getThisBinding() {
    console.log(arguments);
    return this;
}

// this로 사용할 객체
const thisArg = { a: 1 };

// getThisBinding 함수를 호출하면서 인수로 전달한 객체를 getThisBinding 함수의 this에 바인딩한다.
// apply 메서드는 호출할 함수의 인수를 배열로 묶어 전달한다.
console.log(getThisBinding.apply(thisArg, [1, 2, 3]));
// Arguments(3) [1, 2, 3, callee: ƒ, Symbol(Symbol.iterator): ƒ]
// {a: 1}

// call 메서드는 호출할 함수의 인수를 쉼표로 구분한 리스트 형식으로 전달한다.
console.log(getThisBinding.call(thisArg, 1, 2, 3));
// Arguments(3) [1, 2, 3, callee: ƒ, Symbol(Symbol.iterator): ƒ]
// {a: 1}
```

apply 메서드는 호출할 함수의 인수를 배열로 묶어 전달한다. call 메서드는 호출할 함수의 인수를 쉼표로 구분한 리스트 형식으로 전달한다. 이 둘은 전달하는 방식만 다를 뿐 this로 사용할 객체를 전달하면서 함수를 호출하는 것은 동일하다. (이해가 잘 안되어도 27장 배열에서 다시 배우니 그때 자세히 배웁시다...)

**예제 22-22**

```jsx
function getThisBinding() {
    return this;
}

// this로 사용할 객체
const thisArg = { a: 1 };

// bind 메서드는 첫 번째 인수로 전달한 thisArg로 this 바인딩이 교체된
// getThisBinding 함수를 새롭게 생성해 반환한다.
console.log(getThisBinding.bind(thisArg)); // getThisBinding
// bind 메서드는 함수를 호출하지는 않으므로 명시적으로 호출해야 한다.
console.log(getThisBinding.bind(thisArg)()); // {a: 1}
```

bind 메서드는 apply와 call 메서드와 달리 함수를 호출하지 않고 this로 사용할 객체만 전달한다.

지금까지 배운내용을 정리하자면 사진과 같다.
![](https://velog.velcdn.com/images/dooin/post/926195f3-b847-4bca-be6a-aaa7b31ef4c7/image.png)

---

**추가내용1 - this 바인딩 우선순위**

살펴본 바와 같이 자바스크립트에서 this는 각 상황에 맞게 다양한 방식으로 바인딩된다. 그리고 이러한 바인딩 방식에는 우선순위가 존재한다. this의 바인딩 우선 순위는 다음과 같다.

new 연산자에 의한 바인딩 > 명시적 바인딩 > 암시적 바인딩 > 일반함수 호출에 의한 바인딩

**추가 내용2 - 화살표 함수와 this**

화살표함수는 this 키워드에 해당하는 값을 자기 스코프 내에 정의하지 않는다. 따라서 화살표 함수 내에서 this를 참조하게 되면 스코프 체인상 상위 스코프를 탐색한다.
