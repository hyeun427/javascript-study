# javascript-study

# 31장 RegExp

### 31.1 정규 표현식이란? (regular expression)

> 일정한 패턴을 가진 문자열의 집합을 표현하기 위해 사용하는 형식 언어

- 자바스크립트의 고유 문법이 아니다.

- 대부분의 프로그래밍 언어와 코드 에디터에 내장되어 있다.

- 정규 표현식은 특정 패턴과 일치하는 문자열을 검색하거나 추출 또는 치환할 수 있는 패턴 매칭 기능을 제공한다.

```js
[31 - 01];

// 사용자로부터 입력받은 휴대폰 전화번호

const tel = "010-1234-567팔";
// 정규 표현식 리터럴로 휴대폰 전화번호 패턴을 정의한다.
const regExp = /^\d{3}-\d{4}-\d{4}$/;

// tel이 휴대폰 전화번호 패턴에 매칭하는지 테스트(확인)한다.
regExp.test(tel); // -> false
```

- 정규 표현식은 반복문과 조건문 없이 패턴을 정의하고 테스트하는 것으로 간단히 체크할 수 있다.

- 단점으로는 주석이나 공백을 허용하지 않고 여러 기호를 혼합하여 사용하기 때문에 가독성이 좋지 않다.

<br>

---

### 31.2 정규 표현식의 생성

정규표현식 객체를 생성하기 위해서 `정규 표현식 리터럴`과 `RegExp 생성자 함수`를 사용하지만 `정규 표현식 리터럴` 을 사용하는 것이 일반적이다.
![](https://velog.velcdn.com/images/hjthgus777/post/d195c119-fa3b-43dc-85a9-e1fd6a445092/image.png)

`정규 표현식 리터럴` 은 `패턴` 과 `플래그` 로 구성된다.

`//` : / 안에 패턴을 작성
`패턴` : 우리가 찾고자 하는 패턴
`플래그` : 어떤 옵션을 통해서 검색을 할 것인가?

<br>

1️⃣ `정규 표현식 리터럴` 사용

```js
[31 - 02];

const target = "Is this all there is?";

// 패턴: is
// 플래그: i => 대소문자를 구별하지 않고 검색한다.
const regexp = /is/i;

// test 메서드는 target 문자열에 대해 정규표현식 regexp의 패턴을 검색하여 매칭 결과를 불리언 값으로 반환한다.
regexp.test(target); // -> true
```

<br>

2️⃣ `RegExp 생성자 함수` 사용

```js
new RegExp(pattern[, flags])
```

```js
[31 - 03];

const target = "Is this all there is?";

const regexp = new RegExp(/is/i); // ES6
// const regexp = new RegExp(/is/, 'i');
// const regexp = new RegExp('is', 'i');

regexp.test(target); // -> true
```

<br>

---

### 31.3 RegExp 메서드

#### 31.3.1 RegExp.prototype.exec

인수로 전달받은 문자열에 대해 정규 표현식의 패턴을 검색하여 `매칭 결과`를 `배열`로 반환한다. 매칭 결과가 없는 경우 `null`을 반환한다.

```js
[31 - 04];

const target = "Is this all there is?";
const regExp = /is/;

target.match(regExp); // -> ["is", index: 5, input: "Is this all there is?", groups: undefined]
```

`[0]` : 찾아낸 값 하나

`input` : 원본 문자열

`groups` : 그룹화의 유무

`exec 메서드`는 **문자열 내 모든 패턴을 검색하는 `g` 플래그**를 지정해도 첫 번째 매칭 결과만 반환하므로 주의하자.

<br>

#### 31.3.2 RegExp.prototype.test

인수로 전달받은 문자열에 대해 정규 표현식의 패턴을 검색하여 매칭 결과를 `boolean` 값으로 반환한다.

```js
[31 - 6];

const target = "Is this all there is?";
const regExp = /is/;

regExp.test(target); // -> true
```

<br>

#### 31.3.3 String.prototype.match

String 표준 빌트인 객체가 제공하는 match 메서드는 대상 문자열과 인수로 전달받은 정규 표현식과의 매칭 결과를 `배열`로 반환한다.

```js
[31 - 06];

const target = "Is this all there is?";
const regExp = /is/;

target.match(regExp); // -> ["is", index: 5, input: "Is this all there is?", groups: undefined]
```

위의 exec 메서드는 문자열 내의 모든 패턴을 검색하는 g 플래그를 지정해도 첫 번째 매칭 결과만 반화한다. **하지만 String.prototype.match 메서드는 g 플래그가 지정되면 모든 결과를 배열로 반환한다. **

```js
const target = "Is this all there is?";
const regExp = /is/g;

target.match(regExp); // -> ["is", "is"]
// this 의 is
// 문장 끝의 is
```

<br>

---

### 31.4 플래그

`플래그`는 정규 표현식의 `검색 방식`을 설정하기 위해 사용한다. 총 6개가 있으며 그 중 3개를 살펴보자.

| 플래그 | 의미        | 설명                                                            |
| :----- | :---------- | :-------------------------------------------------------------- |
| i      | ignore case | 대소문자를 구별하지 않고 패턴을 검색한다.                       |
| g      | Global      | 대상 문자열 내에서 패턴과 일치하는 모든 문자열을 전역 검색한다. |
| m      | Multi line  | 문자열의 행이 바뀌더라도 패턴 검색을 계속한다.                  |

- 보편적으로 `g`와 `m` 을 사용한다. `/g` `/m` `/i`

- 플래그는 옵션이므로 선택적으로 사용할 수 있다.

- 순서와 상관없이 하나 이상의 플래그를 동시에 설정할 수 있다. `/gm`

- 플래그를 사용하지 않은 경우 대소문자를 구별해서 패턴을 검색하며 문자열에 패턴 검색 매칭 대상이 1개 이상 존재해도 첫 번째 매칭한 대상만 검색하고 종료한다.

```js
[31 - 08];

const target = "Is this all there is?";

// target 문자열에서 is 문자열을 대소문자를 구별하여 한 번만 검색한다.
target.match(/is/);
// -> ["is", index: 5, input: "Is this all there is?", groups: undefined]

// target 문자열에서 is 문자열을 대소문자를 구별하지 않고 한 번만 검색한다.
target.match(/is/i);
// -> ["Is", index: 0, input: "Is this all there is?", groups: undefined]

// target 문자열에서 is 문자열을 대소문자를 구별하여 전역 검색한다.
target.match(/is/g);
// -> ["is", "is"]

// target 문자열에서 is 문자열을 대소문자를 구별하지 않고 전역 검색한다.
target.match(/is/gi);
// -> ["Is", "is", "is"]
```

<br>

---

### 31.5 패턴

- 패턴은 `/` 로 열고 닫으며 문자열의 따옴표는 생략한다. 따옴표를 포함하면 따옴표까지도 패턴에 포함되어 검색된다.

- 패턴은 특별한 의미를 가지는 [메타문자](https://codevang.tistory.com/114) 또는 기호로 표현할 수 있다.
- 어떤 문자열 내에 패턴과 일치하는 문자열이 존재할 때 '정규 표현식과 매치한다'고 표현한다.

<br>

---

#### 31.5.1 문자열 검색

정규표현식의 패턴에 문자 또는 문자열을 지정하면 검색 대상 문자열에서 패턴으로 지정한 문자열을 RegExp 메서드를 사용하여 검색한다.

```js
[31 - 09];

const target = "Is this all there is?";

// 'is' 문자열과 매치하는 패턴. 플래그가 생략되었으므로 대소문자를 구별한다.
const regExp = /is/;

// target과 정규 표현식이 매치하는지 테스트한다.
regExp.test(target); // -> true

// target과 정규 표현식의 매칭 결과를 구한다.
target.match(regExp);
// -> ["is", index: 5, input: "Is this all there is?", groups: undefined]
```

<br>

대소문자를 구별하지 않고 검색하려면 플래그 `i` 를 사용한다.

```js
[31 - 10];

const target = "Is this all there is?";

// 'is' 문자열과 매치하는 패턴. 플래그 i를 추가하면 대소문자를 구별하지 않고 한 번만 검색한다.
const regExp = /is/i;

target.match(regExp);
// -> ["Is", index: 0, input: "Is this all there is?", groups: undefined]
```

<br>

검색 대상 문자열 내에서 패턴과 매치하는 모든 문자열을 전역 검색하려면 플래그 `g`를 사용한다.

```js
[31 - 11];

const target = "Is this all there is?";

// 'is' 문자열과 매치하는 패턴.
// 플래그 g를 추가하면 대상 문자열 내에서 패턴과 일치하는 모든 문자열을 전역 검색한다.
const regExp = /is/gi;

target.match(regExp); // -> ["Is", "is", "is"]
```

<br>

---

#### 31.5.2 임의의 문자열 검색

`.` 은 임의의 문자 한 개를 의미한다.(공백 포함) 문자의 내용은 무엇이든 상관없다.

아래 예제의 경우 `.`을 3개 연속하여 패턴을 생성했으므로 문자의 내용과 상관없이 3자리 문자열과 매치한다.

```js
[31 - 12];

const target = "Is this all there is?";

// 임의의 3자리 문자열을 대소문자를 구별하여 전역 검색한다.
const regExp = /.../g;

target.match(regExp); // -> ["Is ", "thi", "s a", "ll ", "the", "re ", "is?"]
```

<br>

---

#### 31.5.3 반복 검색

`{m,n}`은 앞선 패턴이 `최소 m 번`, `최대 n 번` **반복되는 문자열**을 의미한다. 콤마 뒤에 공백이 있으면 정상 작동하지 않으므로 주의한다.

```js
[31 - 13];

const target = "A AA B BB Aa Bb AAA";

// 'A'가 최소 1번, 최대 2번 반복되는 문자열을 전역 검색한다.
const regExp = /A{1,2}/g;

target.match(regExp); // -> ["A", "AA", "A", "AA", "A"]
```

<br>

`{n}`은 앞선 패턴이 `n번` 반복되는 문자열을 의미하며 `{n, n}`과 같다.

```js
[31 - 14];

const target = "A AA B BB Aa Bb AAA";

// 'A'가 2번 반복되는 문자열을 전역 검색한다.
const regExp = /A{2}/g;

target.match(regExp); // -> ["AA", "AA"]
```

<br>

`{n,}`은 앞선 패턴이 최소`n번` 이상 반복되는 문자열을 의미한다.

```js
[31 - 15];

const target = "A AA B BB Aa Bb AAA";

// 'A'가 최소 2번 이상 반복되는 문자열을 전역 검색한다.
const regExp = /A{2,}/g;

target.match(regExp); // -> ["AA", "AAA"]
```

<br>

`+`는 앞선 패턴이 최소 한번 이상 반복되는 문자열을 의미하며 `{1,}`과 같다.
아래 예제는 `A+`는 패턴 `A`가 한번 이상 반복되는 문자열, 즉 `A`만으로 이루어진 문자열 'A', 'AA', 'AAA', ...와 매치한다.

```js
[31 - 16];

const target = "A AA B BB Aa Bb AAA";

// 'A'가 최소 한 번 이상 반복되는 문자열('A, 'AA', 'AAA', ...)을 전역 검색한다.
const regExp = /A+/g;

target.match(regExp); // -> ["A", "AA", "A", "AAA"]
```

<br>

`?`는 앞선 패턴이 최대 한 번(0번 포함) 이상 반복되는 문자열을 의미하며 `{0,1}`과 같다.
아래 예제는 `/colou?r/`는 `colo` 다음 `u`가 최대 한 번(0번 포함) 이상 반복되고 `r`이 이어지는 문자열 `color`, `colour`와 매치한다.

```js
[31 - 17];

const target = "color colour";

// 'colo' 다음 'u'가 최대 한 번(0번 포함) 이상 반복되고
// 'r'이 이어지는 문자열 'color', 'colour'를 전역 검색한다.
const regExp = /colou?r/g;

target.match(regExp); // -> ["color", "colour"]
```

<br>

---

#### 31.5.4 OR 검색

`|` 은 `or`의 의미를 갖는다.
아래의 `/A|B/`는 'A' 또는 'B'를 의미한다.

```js
[31 - 18];

const target = "A AA B BB Aa Bb";

// 'A' 또는 'B'를 전역 검색한다.
const regExp = /A|B/g;

target.match(regExp); // -> ["A", "A", "A", "B", "B", "B", "A", "B"]
```

<br>

분해되지 않은 단어 레벨로 검색하기 위해서는 `+`함께 사용한다.

```js
[31 - 19];

const target = "A AA B BB Aa Bb";

// 'A' 또는 'B'가 한 번 이상 반복되는 문자열을 전역 검색한다.
// 'A', 'AA', 'AAA', ... 또는 'B', 'BB', 'BBB', ...
const regExp = /A+|B+/g;

target.match(regExp); // -> ["A", "AA", "B", "BB", "A", "B"]
```

<br>

위 예제는 패턴을 or로 한 번 이상 반복하는 것인데 이를 간단하게 표현하면 다음과 같다. `[ ]` 내의 문자는 `or`로 동작하고 뒤에 `+`를 사용하면 앞선 패턴을 한 번 이상 반복한다.

```js
[31 - 20];

const target = "A AA B BB Aa Bb";

// 'A' 또는 'B'가 한 번 이상 반복되는 문자열을 전역 검색한다.
// 'A', 'AA', 'AAA', ... 또는 'B', 'BB', 'BBB', ...
const regExp = /[AB]+/g;

target.match(regExp); // -> ["A", "AA", "B", "BB", "A", "B"]
```

<br>

범위를 지정하려면 `[ ]` 내에 `-`를 사용한다.

```js
[31 - 21];

const target = "A AA BB ZZ Aa Bb";

// 'A' ~ 'Z'가 한 번 이상 반복되는 문자열을 전역 검색한다.
// 'A', 'AA', 'AAA', ... 또는 'B', 'BB', 'BBB', ... ~ 또는 'Z', 'ZZ', 'ZZZ', ...
const regExp = /[A-Z]+/g;

target.match(regExp); // -> ["A", "AA", "BB", "ZZ", "A", "B"]
```

<br>

대소문자를 구별하지 않고 알파벳을 검색하는 방법은 아래와 같다.

```js
[31 - 22];

const target = "AA BB Aa Bb 12";

// 'A' ~ 'Z' 또는 'a' ~ 'z'가 한 번 이상 반복되는 문자열을 전역 검색한다.
const regExp = /[A-Za-z]+/g;

target.match(regExp); // -> ["AA", "BB", "Aa", "Bb"]
```

<br>

숫자를 검색하는 방법은 아래와 같다.

```js
[31 - 23];

const target = "AA BB 12,345";

// '0' ~ '9'가 한 번 이상 반복되는 문자열을 전역 검색한다.
const regExp = /[0-9]+/g;

target.match(regExp); // -> ["12", "345"]
```

위 예제의 경우 쉼표 때문에 매칭 결과가 분리되므로 쉼표를 패턴에 포함시킨다.

```js
[31 - 24];

const target = "AA BB 12,345";

// '0' ~ '9' 또는 ','가 한 번 이상 반복되는 문자열을 전역 검색한다.
const regExp = /[0-9,]+/g;

target.match(regExp); // -> ["12,345"]
```

<br>

위 예제를 더 간단하게 나타낼 수 있다. `\d`는 `숫자`를 의미한다. 즉 `[0-9]` 과 같다.
`\D`는 `\d`와 반대로 동작하며 `문자`를 의미한다.

```js
[31 - 25];
const target = "AA BB 12,345";

// '0' ~ '9' 또는 ','가 한 번 이상 반복되는 문자열을 전역 검색한다.
let regExp = /[\d,]+/g;

target.match(regExp); // -> ["12,345"]

// '0' ~ '9'가 아닌 문자(숫자가 아닌 문자) 또는 ','가 한 번 이상 반복되는 문자열을 전역 검색한다.
regExp = /[\D,]+/g;

target.match(regExp); // -> ["AA BB ", ","]
```

<br>

`\w`는 `알파벳`, `숫자`, `언더스코어(_)`를 의미하며 `[A-Za-z0-9_]`와 같다.(1개의 문자로 취급, 공백이나 다른 특수문자는 제외)
`\W`는 `\w`와 반대로 동작하며 `알파벳`, `숫자`, `언더스코어(_)` 제외한 모든 `문자`를 의미한다.

```js
[31 - 26];

const target = "Aa Bb 12,345 _$%&";

// 알파벳, 숫자, 언더스코어, ','가 한 번 이상 반복되는 문자열을 전역 검색한다.
let regExp = /[\w,]+/g;

target.match(regExp); // -> ["Aa", "Bb", "12,345", "_"]

// 알파벳, 숫자, 언더스코어가 아닌 문자 또는 ','가 한 번 이상 반복되는 문자열을 전역 검색한다.
regExp = /[\W,]+/g;

target.match(regExp); // -> [" ", " ", ",", " $%&"]
```

<br>

---

#### 31.5.5 NOT 검색

`[...]` 내의 `^`은 `not`의 의미를 갖는다.

`[^0-9]` 는 숫자를 제외한 문자를 의미하며 `\D` 와 같다.
`[^A-Za-z0-9]`는 `\W`와 같다.

```js
[31 - 27];

const target = "AA BB 12 Aa Bb";

// 숫자를 제외한 문자열을 전역 검색한다.
const regExp = /[^0-9]+/g;

target.match(regExp); // -> ["AA BB ", " Aa Bb"]
```

<br>

---

#### 31.5.6 시작 위치로 검색

`[...]` 밖의 `^`은 문자열의 시작을 의미한다.

```js
[31 - 28];
const target = "https://poiemaweb.com";

// 'https'로 시작하는지 검사한다.
const regExp = /^https/;

regExp.test(target); // -> true
```

<br>

---

#### 31.5.7 마지막 위치로 검색

`$` 는 문자열의 마지막을 의미한다.

```js
[31 - 29];

const target = "https://poiemaweb.com";

// 'com'으로 끝나는지 검사한다.
const regExp = /com$/;

regExp.test(target); // -> true
```

<br>

---

### 31.6 자주 사용하는 정규표현식

#### 31.6.1 특정 단어로 시작하는지 검사

검색 대상 문자열이 `'http://'` 또는 `'https://'`로 시작하는지 검사한다.

- [...] 바깥의 `^`은 문자열의 시작을 의미한다.

- `?`은 앞선 패턴(s)이 최대 한 번(0번 포함) 이상 반복되는지를 의미한다.

- 다시말해 검색 대상 문자열에 앞선 패턴 's' 이 있어도 없어도 매치된다.

```js
[31-30]

const url = 'https://example.com';

// 'http://' 또는 'https://'로 시작하는지 검사한다.
/^https?:\/\//.test(url); // -> true
// 특수 문자 검색시 :  \찾고자하는특수문자 \찾고자하는특수문자

[31-31]

/^(http|https):\/\//.test(url); // -> true

```

<br>

---

#### 31.6.2 특정 단어로 끝나는지 검사

검색 대상 문자열이 `'html'`로 끝나는지 검사한다.
`$`는 문자열의 마지막을 의미한다.

```js
[31 - 32];

const fileName = "index.html";

// 'html'로 끝나는지 검사한다.
/html$/.test(fileName); // -> true
```

<br>

---

#### 31.6.3 숫자로만 이루어진 문자열인지 검사

검색 대상 문자열이 숫자로만 이루어진 문자열인지 검사한다.

- [...] 바깥의 `^` 은 문자열의 시작

- `$`는 문자열의 마지막

- `\d` 숫자

- `+` 앞선 패턴이 최소 한 번 이상 반복되는 문자열

즉, 츠음과 끝이 숫자이고 최소 한 번 이상 반복되는 문자열

```js
[31 - 33];

const target = "12345";

// 숫자로만 이루어진 문자열인지 검사한다.
/^\d+$/.test(target); // -> true
```

<br>

---

#### 31.6.4 하나 이상의 공백으로 시작하는지 검사

검색 대상 문자열이 하나 이상의 공백으로 시작하는지 검사한다.

- `\s` 는 여러가지 `공백 문자`(스페이스, 탭 등)를 의미한다.

- `\s` 는 `[\t\r\n\v\f]`와 같은 의미이다.

```js
[31 - 34];

const target = " Hi!";

// 하나 이상의 공백으로 시작하는지 검사한다.
/^[\s]+/.test(target); // -> true
```

<br>

---

#### 31.6.5 아이디로 사용 가능한지 검사

검색 대상 문자열이 알파벳 대소문자 또는 숫자로 시작하고 끝나며 4~10자리인지 검사한다.

```js
[31 - 35];

const id = "abc123";

// 알파벳 대소문자 또는 숫자로 시작하고 끝나며 4 ~ 10자리인지 검사한다.
/^[A-Za-z0-9]{4,10}$/.test(id); // -> true
```

<br>

---

#### 31.6.6 메일 주소 형식에 맞는지 검사

검색 대상 문자열이 메일 주소 형식에 맞는지 검사한다.

```js
[31-36]

const email = 'ungmo2@gmail.com';

/^[0-9a-zA-Z]([-_\.]?[0-9a-zA-Z])*@[0-9a-zA-Z]([-_\.]?[0-9a-zA-Z])*\.[a-zA-Z]{2,3}$/.test(email); // -> true


[a-zA-Z0-9._+-]+@[a-zA-Z0-9-]+
// [] 이 안에 있는 것들이 여러번 반복

```

<br>

---

#### 31.6.7 핸드폰 번호 형식에 맞는지 검사

검색 대상 문자열이 핸드폰 번호 형식에 맞는지 검사한다.

```js
[31-38]

const cellphone = '010-1234-5678';

/^\d{3}-\d{3,4}-\d{4}$/.test(cellphone); // -> true

010-0000-0000
010.0000.0000
010 0000 0000
02-0000-0000

/^\d{2,3}[- .]\d{3,4}[- .]\d{4}$/
최소는 2개 최대 3개

```

<br>

---

#### 31.6.8 특수 무자 포함 여부 검사

검색 대상 문자열에 특수 문자열이 포함되어 있는지 검사한다. 특수문자는 `A-Za-z0-9` 이와의 문자다.

```js
[31 - 39];

const target = "abc#123";

// A-Za-z0-9 이외의 문자가 있는지 검사한다.
/[^A-Za-z0-9]/gi.test(target); // -> true
```

<br>

아래 방식으로 대체해서 사용할 수 있다. 이 방식은 특수 문자를 선택적으로 검사할 수 있다.

```js
[31 - 40](/[\{\}\[\]\/?.,;:|\)*~`!^\-_+<>@\#$%&\\\=\(\'\"]/gi).test(target); // -> true
```

<br>

특수 문자를 제거할 때는 `String.prototype.replace` 메서드를 사용한다.

```js
[31 - 41];

target.replace(/[^A-Za-z0-9]/gi, ""); // -> abc123
```
