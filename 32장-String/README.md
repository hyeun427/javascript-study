# 32장 String

표준 빌트인 객체인 String은 원시 타입인 문자열을 다룰 때 유용한 프로퍼티와 메서드를 제공한다

## 32.1 String 생성자 함수

- 표준 빌트인 객체인 String 객체는 생성자 함수 객체이기 때문에 new 연산자와 함께 호출하여 String 인스턴스를 생성할 수 있다.

- String 생성자 함수에 인수를 전달하지 않고 new 연산자와 함께 호출하는 경우:

[[StringData]] 내부 슬롯에 빈 문자열을 할당한 String 래퍼 객체를 생성한다.

```js
const strObj = new String();
console.log(strObj); // String {length: 0, [[PrimitiveValue]]: ""}
```

(ES5에서는 [[StringData]]를 [[PrimitiveValue]]라 불렀다.)

- String 생성자 함수의 인수로 문자열을 전달하면서 new 연산자와 함께 호출하면 [[StringData]] 내부 슬롯에 인수로 전달받은 문자열을 할당한 String 래퍼 객체를 생성한다.

```js
const strObj = new String("Lee");
console.log(strObj);
// String {0: "L", 1: "e", 2: "e", length: 3, [[PrimitiveValue]]: "Lee"}
```

- String 래퍼 객체는 배열과 마찬가지로 length 프로퍼티와 인덱스를 나타내는 숫자 형식의 문자열을 프로퍼티 키로, 각 문자를 프로퍼티 값으로 갖는 **유사 배열 객체이면서 이터러블**이다.

- 따라서 배열과 유사하게 **인덱스를 사용하여 각 문자에 접근**할 수 있다.

```js
console.log(strObj[0]); // L
```

- 단, 문자열은 원시 값이므로 변경할 수 없다. 하지만 에러는 발생하지 않는다.

```js
// 문자열은 원시값이므로 변경할 수 없다. 이때 에러가 발생하지 않는다.
strObj[0] = "S";
console.log(strObj); // 'Lee'
```

- String 생성자 함수의 인수로 문자열이 아닌 값을 전달하면 인수를 문자열로 강제 변환한다.

```js
let strObj = new String(123);
console.log(strObj);
// String {0: "1", 1: "2", 2: "3", length: 3, [[PrimitiveValue]]: "123"}

strObj = new String(null);
console.log(strObj);
// String {0: "n", 1: "u", 2: "l", : "l", length: 4, [[PrimitiveValue]]: "null"}
```

- new 연산자를 사용하지 않고 String 생성자 함수를 호출하면 String 인스턴스가 아닌 문자열을 반환한다. 이를 이용해 명시적으로 타입을 변환하기도 한다.

```js
// 숫자 타입 => 문자열 타입
String(1); // -> "1"
String(NaN); // -> "NaN"
String(Infinity); // -> "Infinity"

// 불리언 타입 => 문자열 타입
String(true); // -> "true"
String(false); // -> "false"
```

---

## 32.2 length 프로퍼티

- length 프로퍼티는 문자열의 문자 개수를 반환한다.

```js
"Hello".length; // -> 5
"안녕하세요!".length; // -> 6
```

- String 래퍼 객체는 **유사 배열 객체**다.
  - 배열과 마찬가지로 length 프로퍼티를 갖기 때문에
  - 인덱스를 나타내는 숫자를 프로퍼티 키로, 각 문자를 프로퍼티 값으로 갖기 때문에

---

## 32.3 String 메서드

- 배열에는 원본 배열을 직접 변경하는 메서드(mutator method)와 원본 배열을 직접 변경하지 않고 새로운 배열을 생성하여 반환하는 메서드(accessor method)가 있다.

- 하지만 String 객체에는 원본 String 래퍼 객체를 직접 변경하는 메서드는 존재하지 않는다.

  => 즉, String 객체의 메서드는 항상 새로운 문자열을 반환한다.

  - 문자열은 변경 불가능한 원시 값이기 때문에 String 래퍼 객체도 읽기 전용(read only) 객체로 제공된다.

```js
const strObj = new String("Lee");

console.log(Object.getOwnPropertyDescriptors(strObj));
/* String 래퍼 객체는 읽기 전용 객체다. 즉, writable 프로퍼티 어트리뷰트 값이 false다.
{
  '0': { value: 'L', writable: false, enumerable: true, configurable: false },
  '1': { value: 'e', writable: false, enumerable: true, configurable: false },
  '2': { value: 'e', writable: false, enumerable: true, configurable: false },
  length: { value: 3, writable: false, enumerable: false, configurable: false }
}
*/
```

- String 래퍼 객체가 읽기 전용 객체가 아니면 변경된 String 래퍼 객체를 문자열로 되돌릴 때 문자열(원시 값)이 변경된다.

  => String 객체의 모든 메서든 String 래퍼 객체를 직접 변경할 수 없고, 언제나 새로운 문자열을 생성하여 반환한다.

### 32.3.1 String.prototype.indexOf

> indexOf 메서드는 대상 문자열에서 인수로 전달 받은 문자열을 검색하여 첫 번째 인덱스를 반환한다. 검색에 실패하면 -1을 반환한다.

```js
const str = "Hello World";

// 문자열 str에서 'l'을 검색하여 첫 번째 인덱스를 반환한다.
str.indexOf("l"); // -> 2

// 문자열 str에서 'or'을 검색하여 첫 번째 인덱스를 반환한다.
str.indexOf("or"); // -> 7

// 문자열 str에서 'x'를 검색하여 첫 번째 인덱스를 반환한다. 검색에 실패하면 -1을 반환한다.
str.indexOf("x"); // -> -1
```

- indexOf 메서드의 두 번째 인수로 검색을 시작할 인덱스를 전달할 수 있다.

```js
// 문자열 str의 인덱스 3부터 'l'을 검색하여 첫 번째 인덱스를 반환한다.
str.indexOf("l", 3); // -> 3
```

- indexOf 메서드는 대상 문자열에 특정 문자열이 존재하는지 확인할 때 유용하다.

```js
if (str.indexOf("Hello") !== -1) {
  // 문자열 str에 'Hello'가 포함되어 있는 경우에 처리할 내용
}
```

- ES6에서 도입된 `String.prototype.includes` 메서드를 사용하는 게 가독성이 더 좋다.

```js
if (str.includes("Hello")) {
  // 문자열 str에 'Hello'가 포함되어 있는 경우에 처리할 내용
}
```

### 32.3.2 String.prototype.search

> search 메서드는 대상 문자열에서 인수로 전달 받은 정규 표현식과 매치하는 문자열을 검색하여 일치하는 문자열의 인덱스를 반환한다. 검색에 실패하면 -1을 반환한다.

```js
const str = "Hello world";

// 문자열 str에서 정규 표현식과 매치하는 문자열을 검색하여 일치하는 문자열의 인덱스를 반환한다.
str.search(/o/); // -> 4
str.search(/x/); // -> -1
```

### 32.3.3 String.prototype.includes

> ES6에서 도입된 includes 메서드는 대상 문자열에 인수로 전달 받은 문자열이 포함되어 있는지 확인하여 그 결과를 true 또는 false로 반환한다.

```js
const str = "Hello world";

str.includes("Hello"); // -> true
str.includes(""); // -> true
str.includes("x"); // -> false
str.includes(); // -> false
```

- includes 메서드의 두번째 인수로 검색을 시작할 인덱스를 전달할 수 있다.

```js
const str = "Hello world";

// 문자열 str의 인덱스 3부터 'l'이 포함되어 있는지 확인
str.includes("l", 3); // -> true
str.includes("H", 3); // -> false
```

### 32.3.4 String.prototype.startsWith

> ES6에서 도입된 startsWith 메서드는 대상 문자열이 인수로 전달 받은 문자열로 시작하는지 확인하여 그 결과를 true 또는 false로 반환한다.

```js
const str = "Hello world";

// 문자열 str이 'He'로 시작하는지 확인
str.startsWith("He"); // -> true
// 문자열 str이 'x'로 시작하는지 확인
str.startsWith("x"); // -> false
```

- startsWith 메서드의 두번째 인수로 검색을 시작할 인덱스를 전달할 수 있다.

### 32.3.5 String.prototype.endsWith

> ES6에서 도입된 endsWith 메서드는 대상 문자열이 인수로 전달 받은 문자열로 끝나는지 확인하여 그 결과를 true 혹은 false로 반환한다.

```js
const str = "Hello world";

// 문자열 str이 'ld'로 끝나는지 확인
str.endsWith("ld"); // -> true
// 문자열 str이 'x'로 끝나는지 확인
str.endsWith("x"); // -> false
```

endsWith 메서드의 두번째 인수로 검색할 문자열의 길이를 전달할 수 있다.

```js
// 문자열 str의 처음부터 5자리까지('Hello')가 'lo'로 끝나는지 확인
str.endsWith("lo", 5); // -> true
```

### 32.3.6 String.prototype.charAt

> charAt 메서드는 대상 문자열에서 인수로 전달 받은 인덱스에 위치한 문자를 검색하여 반환한다.

```js
const str = "Hello";

for (let i = 0; i < str.length; i++) {
  console.log(str.charAt(i)); // H e l l o
}
```

- 인덱스는 문자열의 범위(0 ~ 문자열길이-1) 사이의 정수여야 한다.
- 인덱스가 문자열의 범위를 벗어난 정수인 경우 빈 문자열을 반환한다.

```js
// 인덱스가 문자열의 범위(0 ~ str.length-1)를 벗어난 경우 빈문자열을 반환한다.
str.charAt(5); // -> ''
```

- charaAt 메서드와 유사한 메서드는 charCodeAt 메서드와 codePointAt 메서드가 있다.

### 32.3.7 String.prototype.substring

> substring 메서드는 대상 문자열에서 첫 번째 인수로 전달 받은 인덱스에 위치하는 문자부터 두 번째 인수로 전달 받은 인덱스에 위치하는 문자의 바로 이전 문자까지의 부분 문자열을 반환한다.

```js
const str = "Hello World";

// 인덱스 1부터 인덱스 4 이전까지의 부분 문자열을 반환한다.
str.substring(1, 4); // -> ell
```

![](../public/images/32-1.jpeg)

- 두 번째 인수는 생략할 수 있다. 이때 첫 번째 인수로 전달한 인덱스에 위치하는 문자부터 마지막 문자까지 부분 문자열을 반환한다.

```js
const str = "Hello World";

// 인덱스 1부터 마지막 문자까지 부분 문자열을 반환한다.
str.substring(1); // -> 'ello World'
```

- substring 메서드의 첫 번째 인수는 두 번째 인수보다 작은 정수여야 정상이지만, 다음과 같이 인수를 전달해도 정상 동작한다.
  - `첫 번째 인수 > 두 번째 인수`인 경우 서로 교환된다.
  - `인수 < 0` 또는 `NaN`인 경우 0으로 취급된다.
  - `인수 > 문자열의 길이`인 경우 인수는 문자열의 길이로 취급된다.

```js
const str = "Hello World"; // str.length == 11

// 첫 번째 인수 > 두 번째 인수인 경우 두 인수는 교환된다.
str.substring(4, 1); // -> 'ell'

// 인수 < 0 또는 NaN인 경우 0으로 취급된다.
str.substring(-2); // -> 'Hello World'

// 인수 > 문자열의 길이(str.length)인 경우 인수는 문자열의 길이(str.length)으로 취급된다.
str.substring(1, 100); // -> 'ello World'
str.substring(20); // -> ''
```

- String.prototype.indexOf 메서드와 함께 사용하면 특정 문자열을 기준으로 앞뒤에 위치한 부분 문자열을 취득할 수 있다.

```js
const str = "Hello World";

// 스페이스를 기준으로 앞에 있는 부분 문자열 취득
str.substring(0, str.indexOf(" ")); // -> 'Hello'

// 스페이스를 기준으로 뒤에 있는 부분 문자열 취득
str.substring(str.indexOf(" ") + 1, str.length); // -> 'World'
```

### 32.3.8 String.prototype.slice

> slice 메서드는 substring 메서드와 동일하게 동작한다. 단, slice 메서드에는 음수인 인수를 전달할 수 있다.
> 음수인 인수를 전달하면 대상 문자열의 가장 뒤에서부터 시작하여 문자열을 잘라내어 반환한다.

```js
const str = "hello world";

// substring과 slice 메서드는 동일하게 동작한다.
// 0번째부터 5번째 이전 문자까지 잘라내어 반환
str.substring(0, 5); // -> 'hello'
str.slice(0, 5); // -> 'hello'

// 인덱스가 2인 문자부터 마지막 문자까지 잘라내어 반환
str.substring(2); // -> 'llo world'
str.slice(2); // -> 'llo world'

// 인수 < 0 또는 NaN인 경우 0으로 취급된다.
str.substring(-5); // -> 'hello world'
// slice 메서드는 음수인 인수를 전달할 수 있다. 뒤에서 5자리를 잘라내어 반환한다.
str.slice(-5); // ⟶ 'world'
```

### 32.3.9 String.prototype.toUpperCase

> toUpperCase 메서드는 대상 문자열을 모두 대문자로 변경한 문자열을 반환한다.

```js
const str = "Hello World!";

str.toUpperCase(); // -> 'HELLO WORLD!'
```

### 32.3.10 String.prototype.toLowerCase

> toLowerCase 메서드는 대상 문자열을 모두 소문자로 변경한 문자열을 반환한다.

```js
const str = "Hello World!";

str.toLowerCase(); // -> 'hello world!'
```

### 32.3.11 String.prototype.trim

> trim 메서드는 문자열 앞뒤에 공백 문자가 있을 경우 이를 제거한 문자열을 반환한다.

```js
const str = "   foo  ";

str.trim(); // -> 'foo'
```

- 2021년 1월 기준, String.prototype.trimStart, String.prototype.trimEnd를 사용하면 대상 문자열 앞 뒤 또는 뒤에 공백 문자가 있을 경우 이를 제거한 문자열을 반환한다.

```js
const str = "   foo  ";

// String.prototype.{trimStart,trimEnd} : Proposal stage 4
str.trimStart(); // -> 'foo  '
str.trimEnd(); // -> '   foo'
```

### 32.3.12 String.prototype.repeat

> ES6에서 도입된 repeat 메서드는 대상 문자열을 인수로 전달 받은 정수만큼 반복해 연결한 새로운 문자열을 반환한다.
> 인수로 전달 받은 정수가 0이면 빈 문자열을 반환하고, 음수면 RangeError를 발생시킨다. 인수를 생략하면 기본값 0이 설정된다.

```js
const str = "abc";

str.repeat(); // -> ''
str.repeat(0); // -> ''
str.repeat(1); // -> 'abc'
str.repeat(2); // -> 'abcabc'
str.repeat(2.5); // -> 'abcabc' (2.5 → 2)
str.repeat(-1); // -> RangeError: Invalid count value
```

### 32.3.13 String.prototype.replace

> replace 메서든 ㄴ대상 문자열에서 첫 번째 인수로 전달 받은 문자열 또는 정규표현식을 검색하여 두 번째 인수로 전달한 문자열로 치환한 문자열을 반환한다.

```js
const str = "Hello world";

// str에서 첫 번째 인수 'world'를 검색하여 두 번째 인수 'Lee'로 치환한다.
str.replace("world", "Lee"); // -> 'Hello Lee'
```

- 검색된 문자열이 여러 개일 경우 첫 번째로 검색된 문자열만 치환한다.

```js
const str = "Hello world world";

str.replace("world", "Lee"); // -> 'Hello Lee world'
```

- 특수한 교체 패턴을 사용할 수 있다. 예를 들어, `$&`는 검색된 문자열을 의미한다.

```js
const str = "Hello world";

// 특수한 교체 패턴을 사용할 수 있다. ($& => 검색된 문자열)
str.replace("world", "<strong>$&</strong>");
```

- replace 메서드의 첫 번째 인수로 정규 표현식을 전달할 수도 있다.

```js
const str = "Hello Hello";

// 'hello'를 대소문자를 구별하지 않고 전역 검색한다.
str.replace(/hello/gi, "Lee"); // -> 'Lee Lee'
```

- replace 메서드의 두 번째 인수로 치환함수를 전달할 수 있다.

```js
// 카멜 케이스를 스네이크 케이스로 변환하는 함수
function camelToSnake(camelCase) {
  // /.[A-Z]/g는 임의의 한 문자와 대문자로 이루어진 문자열에 매치한다.
  // 치환 함수의 인수로 매치 결과가 전달되고, 치환 함수가 반환한 결과와 매치 결과를 치환한다.
  return camelCase.replace(/.[A-Z]/g, (match) => {
    console.log(match); // 'oW'
    return match[0] + "_" + match[1].toLowerCase();
  });
}

const camelCase = "helloWorld";
camelToSnake(camelCase); // -> 'hello_world'

// 스네이크 케이스를 카멜 케이스로 변환하는 함수
function snakeToCamel(snakeCase) {
  // /_[a-z]/g는 _와 소문자로 이루어진 문자열에 매치한다.
  // 치환 함수의 인수로 매치 결과가 전달되고, 치환 함수가 반환한 결과와 매치 결과를 치환한다.
  return snakeCase.replace(/_[a-z]]/g, (match) => {
    console.log(match); // '_w'
    return match[1].toUpperCase();
  });
}

const snakeCase = "hello_world";
snakeToCamel(snakeCase); // -> 'helloWorld'
```

### 32.3.14 String.prototype.split

> split 메서드는 대상 문자열에서 첫 번째 인수로 전달한 문자열 또는 정규 표현식을 검색하여 문자열을 구분한 후, 분리된 각 문자열로 이루어진 배열을 반환한다.
> 인수로 빈 문자열을 전달하면 각 문자를 모두 분리하고, 인수를 생략하면 전체를 단일 요소로 하는 배열을 반환한다.

```js
const str = "How are you doing?";

// 공백으로 구분(단어로 구분)하여 배열로 반환한다.
str.split(" "); // -> ["How", "are", "you", "doing?"]

// \s는 여러 가지 공백 문자(스페이스, 탭 등)를 의미한다. 즉, [\t\r\n\v\f]와 같은 의미다.
str.split(/\s/); // -> ["How", "are", "you", "doing?"]

// 인수로 빈 문자열을 전달하면 각 문자를 모두 분리한다.
str.split(""); // -> ["H", "o", "w", " ", "a", "r", "e", " ", "y", "o", "u", " ", "d", "o", "i", "n", "g", "?"]

// 인수를 생략하면 대상 문자열 전체를 단일 요소로 하는 배열을 반환한다.
str.split(); // -> ["How are you doing?"]
```

- 두 번째 인수로 배열의 길이를 지정할 수 있다.

```js
// 공백으로 구분하여 배열로 반환한다. 단, 배열의 길이는 3이다
str.split(" ", 3); // -> ["How", "are", "you"]
```

- split 메서드는 배열을 반환한다. 따라서, reverse 메서드, join 메서드와 함께 사용하면 문자열을 역순으로 뒤집을 수 있다.
