# javascript-study

# 6장 데이터 타입

데이터 타입은 **값의 종류**를 말한다.

| 원시 타입 (primitive type) | 숫자, 문자열, 불리언, undefined, null, 심벌 타입 |
| 객체/참조 타입 (object/reference type)||

자세한 내용은 11장 "원시 값과 객체의 비교"

## 6.1 숫자 타입

- 자바스크립트의 숫자 타입은 하나의 숫자 타입인 실수(integer)만 존재한다.

```javascript
// 모두 숫자 타입
var integer = 10; // 정수
var double = 10.12; // 실수
var negative = -20; // 음의 정수

// 자바스크립트에서 모든 정수는 '정수로 표현된 실수'이다
console.log(1 === 1.0); // true

// 2, 8, 16진수 값을 참조하면 10진수의 (정수로 표현된) 실수로 해석된다.
var binary = 0b1000001; // 2진수
var octal = 0o101; // 8진수
var hex = 0x41; // 16진수

console.log(binary); // 65
console.log(octal); // 65
console.log(hex); // 65
console.log(binary === octal); // true
console.log(octal === hex); // true
```

- 세 가지 특별한 값도 모두 숫자 타입 (Infinity, -Infinity, NaN)

```javascript
typeof Infinity; // 'number'
typeof -Infinity; // 'number'
typeof NaN; // 'number'

Infinity - 100; // Infinity
-2 / 0; // -Infinity
Infinity - Infinity; // NaN
0 / 0; // NaN
```

- (참고) parseInt와 parseFloat 메서드

  - parseInt는 문자열을 정수로만 바꾼다. 정수가 아닌 값을 입력하면 정수 부분만 추출해 표시한다.

  ```javascript
  parseInt("3.14"); // 3
  ```

  - 문자열을 실수로 바꾸고 싶으면 parseInt 대신에 parseFloat을 사용한다.

  ```javascript
  parseFloat("3.14"); // 3.14
  ```

## 6.2 문자열 타입

- 문자열은 작은따옴표, 큰따옴표 또는 백틱으로 텍스트를 감싼다. (작은따옴표가 JS에서 가장 일반적)

- 작은따옴표로 감싼 문자열 내의 큰따옴표 & 큰따옴표로 감싼 문자열 내의 작은따옴표는 문자열로 인식된다.

```javascript
var string;
string = '작은따옴표로 감싼 문자열 내의 "큰따옴표"';
string = "큰따옴표로 감싼 문자열 내의 '작은따옴표'";
```

- 일반 문자열 내에서 줄바꿈 등의 공백을 표현하려면 백슬래시(\)로 시작하는 이스케이프 시퀀스(escape sequence)를 사용해야 한다.
  ※ 대표적인 이스케이프 시퀀스와 의미
  ![escape_sequence](/assets/images/js_study/escape_sequence.png)

## 6.3 템플릿 리터럴

- ES6부터 적용된 문자열 표기법

- 큰/작은따옴표 대신 백틱(`)을 사용한다.

- 템플릿 리터럴은 런타임에 일반 문자열로 변환되어 처리된다.
  (런타임: [컴퓨터 과학](https://ko.wikipedia.org/wiki/%EC%BB%B4%ED%93%A8%ED%84%B0_%EA%B3%BC%ED%95%99)에서 [컴퓨터 프로그램](https://ko.wikipedia.org/wiki/%EC%BB%B4%ED%93%A8%ED%84%B0_%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%A8)이 실행되고 있는 동안의 동작)

### 6.3.1 멀티라인 문자열

- 템플릿 리터럴 내에서는 이스케이프 시퀀스를 사용하지 않고도 줄바꿈, 모든 공백이 있는 그대로 적용된다.

```javascript
var hello = `hello
friends`;

console.log(hello);
```

```javascript
// 출력 결과
hello;
friends;
```

### 6.3.2 표현식 삽입

- +를 사용해 문자열을 연결하는 것보다 가독성 좋고 간편하게 문자열을 조합한다.

```javascript
var first = "서형";
var last = "권";

// ES5: 문자열 연결
console.log("내 이름은 " + last + first + ".");

// ES6: 표현식 삽입
console.log(`내 이름은 ${last}${first}.`);
```

- 표현식은 ${ }로 감싼 후 삽입한다. 표현식의 평가 결과가 문자열이 아니더라도 문자열로 타입이 강제로 변환되어 삽입된다.

```javascript
console.log(`1 + 2 = ${1 + 2}`); // 1 + 2 = 3
console.log(`${1 + 2}`); // 3
console.log(typeof `${1 + 2}`); // string
```

## 6.4 불리언 타입

- 불리언 타입의 값은 논리적 참, 거짓을 나타내는 true와 false 두 가지뿐이다.

- 조건문에서 자주 사용한다.

## 6.5 undefined 타입

- undefined 타입의 값은 undefined뿐이다.

- var 키워드로 변수를 선언한 후 값을 할당하지 않은 변수를 참조하면 undefined가 반환된다. (자바스크립트 엔진이 변수를 초기화하는 데 사용하는 것이 undefined)

```javascript
var foo;
console.log(foo); // undefined
```

- 개발자가 의도적으로 변수에 할당하지 않도록 주의한다!

## 6.6. null 타입

- 변수에 값이 없다는 것을 명시하고 싶을 때는 undefined 대신 null을 할당한다.

- null ≠ Null ≠ NULL (자바스크립트는 대소문자를 구분한다.)

- 함수가 유효한 값을 반환할 수 없는 경우 명시적으로 null을 반환하기도 한다.
  e.g., document.querySelector 메서드가 조건에 부합하는 HTML 요소를 검색할 수 없는 경우

  ```javascript
  <!DOCTYPE html>
  <html>
    <body>
      <script>
        var element = document.querySelector('.myClass');

        // HTML 문서에 myClass 클래스를 갖는 요소가 없다면 에러 대신 null을 반환한다.
        console.log(element); // null
      </script>
    </body>
  </html>
  ```

## 6.7 심벌 타입

33장 “7번째 타입 Symbol”

- ES6에서 추가된 7번째 타입

- 다른 값과 중복되지 않는 유일무이한 값

- 이름이 충돌할 위험이 없는 객체의 유일한 프로퍼티 키를 만들기 위해 사용한다.

- Symbol 함수를 호출해 생성한다.

- 생성된 심벌 값은 외부에 노출되지 않는다.

```javascript
// 심벌 값 생성 (레이블은 없어도 무관)
var key = Symbol("key");
console.log(typeof key); // symbol

// 객체 생성
var obj = {};

// 이름이 충돌할 위험이 없는 유일무이한 값인 심벌을 프로퍼티 키로 사용한다.
obj[key] = "value";
console.log(obj[key]); // value
```

## 6.8 객체 타입

11장 “원시 값과 객체의 비교”

- 자바스크립트는 객체 기반의 언어이며, 자바스크립트를 이루고 있는 거의 모든 것이 객체이다. (6종류의 원시 타입을 제외한 모든 것이 객체 타입)

## 6.9 데이터 타입의 필요성

- 값은 메모리에 저장하고 참조할 수 있어야 함.
  - 메모리에 값을 저장하려면 확보해야 할 메모리 공간의 크기를 결정해야 함.
  - 값을 참조하려면, 한 번에 들여야 할 메모리 공간의 크기(바이트 수)를 알아야 한다. (숫자 타입의 경우, 숫자 타입이므로 8바이트 단위로 읽어들이지 않으면 값이 훼손된다.

### 6.9.1 데이터 타입에 의한 메모리 공간의 확보와 참조

- 값을 저장할 때 확보해야 하는 메모리 공간의 크기를 결정하기 위해

  - 변수 선언과 할당 과정

  ```javascript
  var score = 100;
  ```

  1. 숫자 값 100을 저장하기 위해 메모리 공간 확보한다. (변수에 할당하는 값의 타입에 따라 확보해야 할 메모리 공간의 크기가 달라짐)
  2. 확보된 메모리에 숫자 값 100을 2진수로 저장한다.

- 값을 참조할 때 한 번에 읽어 들여야 할 메모리 공간의 크기를 결정하기 위해

  - 변수 참조 과정

  ```javascript
  console.log(score);
  ```

  1. 식별자 score를 통해 숫자 타입의 값 100이 저장된 메모리 공간의 주소를 찾아간다.
  2. score 변수에는 숫자 타입의 값이 할당되어 있으므로 자바스크립트 엔진은 score 변수를 숫자 타입으로 인식하고, 8바이트 단위로 메모리 공간에 저장된 값을 읽어 들인다.

### 6.9.2 데이터 타입에 의한 값의 해석

- 메모리에서 읽어 들인 2진수를 어떻게 해석할지 결정하기 위해
  - e.g., 메모리에 저장된 2진수 값 0100 0001을 숫자로 해석하면 65, 문자열로 해석하면 ‘A’.
  - score 변수에 할당된 값의 데이터 타입이 숫자이므로, 숫자로 해석한다.

## 6.10 동적 타이핑

### 6.10.1 동적 타입 언어와 정적 타입 언어

- 정적 타입(static/strong type) 언어는 변수를 선언할 때, 변수에 할당할 수 있는 데이터 타입을 사전에 선언해야 한다. (C, C++, 자바, 코틀린, 고, 하스켈, 러스트, 스칼라 등)

```javascript
// C의 경우:

// c 변수에는 1바이트 정수 타입의 값만 할당 가능
char c;

// num 변수에는 4바이트 정수 타입의 값만 할당 가능
int num;
```

- 동적 타입(dynamic/weak type) 언어의 변수는 타입을 갖지 않는다. 변수에 할당되어 있는 값에 의해 변수의 타입이 동적으로 결정된다. (자바스크립트, 파이썬, PHP, 루비, 리스프, 펄 등)

  - 동적 타이핑(dynamic typing): 변수의 타입은 재할당에 의해 언제든지 동적으로 변할 수 있다.
  - 자바스크립트의 변수는 선언이 아닌 할당에 의해 타입이 결정(타입 추론, type inference)된다.

  ```javascript
  // 변수 선언
  var foo;
  console.log(foo); // undefined

  // 할당
  foo = 3;
  console.log(typeof foo); // number

  // 재할당
  foo = "Hello";
  console.log(typeof foo); // string

  foo = true;
  console.log(typeof foo); // boolean

  foo = null;
  console.log(typeof foo); // object (외우기!!)

  foo = Symbol();
  console.log(typeof foo); // symbol
  ```

### 6.10.2 동적 타입 언어와 변수

- 변수에 어떤 데이터 타입의 값이라도 자유롭게 할당할 수 있다. (편리함)

- 유연성은 높지만 신뢰성은 떨어진다. (위험함)
  - 값의 변경에 의해 타입도 언제든 변경될 수 있기 때문에 오류 발생 가능성 증가

※ 변수를 사용할 때 주의할 사항

- 변수는 꼭 필요한 경우에만 제한적으로 사용
- 변수의 유효 범위를 좁게 만듦, 전역 변수 사용 자제
- 변수보다는 상수(const) 사용하여 값의 변경 억제
- 변수명은 변수의 목적이나 의미를 파악할 수 있도록 명시적으로 작성
