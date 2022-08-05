# javascript-study

# 48장 모듈

### 48.1 모듈의 일반적 의미

- 모듈이란 애플리케이션을 구성하는 개별적 요소로서 재사용 가능한 코드 조각을 말한다.

- 모듈이 성립하려면 모듈은 자기만의 모듈 스코프를 가져야한다.

<br>

모듈의 리소스(변수, 함수, 객체)는 기본적으로 비공개 상태이다. 즉 개별적 존재로 애플리케이션과 분리되어 있지만 이런 모듈은 재사용이 불가능하므로 존재 의미가 없다. 따라서 모듈은 애플리케이션이나 다른 모듈에 의해 재사용되어야 한다.

- 모듈은 공개가 필요한 리소스에 한정하여 명시적으로 선택적 공개가 가능하다. `export`

- 모듈의 사용자는 모듈이 공개한 리소스 중 일부 또는 전체를 자신의 스코프 내로 불러들여 재사용할 수 있다. `import`
- 모듈은 기능별로 분리되어 개별적인 파일로 작성된다.
- 재사용성이 좋아서 개발 효율성과 유지보수성을 높일 수 있다.

<br>

### 48.2 자바스크립트와 모듈

원래 자바스크립트는 모듈 시스템을 지원하지 않았다. 그래서 script 태그로 불러온 여러 스크립트들은 마치 하나의 파일안에 작성된 코드들처럼 동작한다.

하지만 자바스크립트 런타임 환경인 Node.js는 모듈 시스템의 사실상 표준인 CommonJS 사양을 따르고 있다.

즉 Node.js는 ECMAScript 표준 사양은 아니지만 모듈 시스템을 지원한다. 따라서 Node.js 환경에서는 파일별로 독립적인 모듈 스코프를 갖는다.

<br>

### 48.3 ES6 모듈(ESM)

ES6에서 클라이언트 사이드 자바스크립트에서도 동작하는 모듈 기능을 추가했다. ES6모듈(ESM)의 사용법은 간단하다.

- cript태그에 type="module" 어트리뷰트를 추가하면 로드된 자바스크립트 파일은 모듈로 동작한다.

```
<script type="module" src="app.js"></script>
```

- ESM에서는 기본적으로 strict mode가 적용된다.

<br>

#### 48.3.1 모듈 스코프

ESM은 독자적인 모듈 스코프를 제공한다.

- 하지만 ESM이 아닌 일반 자바스크립트 파일은 script 태그로 분리해서 로드해도 독자적은 모듈 스코프를 갖지 않는다.

```js
[48-02]

<foo.js>

// x 변수는 전역 변수다.
var x = 'foo';
console.log(window.x); // foo
```

```js
[48-03]

<bar.js>

// x 변수는 전역 변수다. foo.js에서 선언한 전역 변수 x와 중복된 선언이다.
var x = 'bar';

// foo.js에서 선언한 전역 변수 x의 값이 재할당되었다.
console.log(window.x); // bar
```

HTML에서 script 태그로 분리해서 로드된 2개의 자바스크립트 파일은 하나의 자바스크립트 파일 내에 있는 것처럼 작동한다. 즉 하나의 전역을 공유한다.

따라서 foo.js에서 선언한 x 변수와 bar.js에서 선언한 x 변수는 중복 선언되며 의도치 않게 x 변수의 값이 덮어써진다.

```js
[48-04]

<!DOCTYPE html>
<html>
<body>
  <script src="foo.js"></script>
  <script src="bar.js"></script>
</body>
</html>
```

<br>

ESM은 파일 자체의 독자적인 모듈 스코프를 제공한다. 따라서 모듈 내에서 var 키워드로 선언한 변수는 더는 전역 변수가 아니며 window 객체의 프로퍼티도 아니다.

```js
[48-05]

<foo.mjs>

// x 변수는 전역 변수가 아니며 window 객체의 프로퍼티도 아니다.
var x = 'foo';
console.log(x); // foo
console.log(window.x); // undefined
```

```js
[48-06]

<bar.mjs>

// x 변수는 전역 변수가 아니며 window 객체의 프로퍼티도 아니다.
// foo.mjs에서 선언한 x 변수와 스코프가 다른 변수다.
var x = 'bar';
console.log(x); // bar
console.log(window.x); // undefined
```

```js
[48-07]

<!DOCTYPE html>
<html>
<body>
  <script type="module" src="foo.mjs"></script>
  <script type="module" src="bar.mjs"></script>
</body>
</html>
```

<br>

#### 48.3.2 export 키워드

모듈 내부에서 선언한 식별자를 외부에 공개하여 다른 모듈들이 재사용할 수 있게 하려면 export 키워드를 사용한다.

- export 키워드는 선언문 앞에 사용하며 변수, 함수, 클래스 등 모든 식별자를 export 할 수 있다.

```js
[48 - 11];

// lib.mjs
// 변수의 공개
export const pi = Math.PI;

// 함수의 공개
export function square(x) {
  return x * x;
}

// 클래스의 공개
export class Person {
  constructor(name) {
    this.name = name;
  }
}
```

<br>

선언문 앞에 매번 export 키워드를 붙이는 것이 번거롭다면 export 할 대상을 하나의 객체로 구성하여 한 번에 export할 수 있다.

```js
// lib.mjs
const pi = Math.PI;

function square(x) {
  return x * x;
}

class Person {
  constructor(name) {
    this.name = name;
  }
}

// 변수, 함수 클래스를 하나의 객체로 구성하여 공개
export { pi, square, Person };
```

<br>

#### 48.3.3 import 키워드

다른 모듈에서 공개한 식별자를 자신의 모듈 스코프 내부로 로드하려면 import 키워드를 사용한다.

- 다른 모듈이 export한 식별자 이름으로 import해야한다.

- ESM의 경우 파일 확장자를 생략할 수 없다.

```js
[48 - 13];

// app.mjs
// 같은 폴더 내의 lib.mjs 모듈이 export한 식별자 이름으로 import한다.
// ESM의 경우 파일 확장자를 생략할 수 없다.
import { pi, square, Person } from "./lib.mjs";

console.log(pi); // 3.141592653589793
console.log(square(10)); // 100
console.log(new Person("Lee")); // Person { name: 'Lee' }
```

<br>

모듈이 export한 식별자 이름을 일일이 지정하지 않고 하나의 이름으로 한 번에 import할 수도 있다.

- import되는 식별자는 as 뒤에 지정한 이름의 객체에 프로퍼티로 할당된다.

```js
[48 - 15];

// app.mjs
// lib.mjs 모듈이 export한 모든 식별자를 lib 객체의 프로퍼티로 모아 import한다.
import * as lib from "./lib.mjs";

console.log(lib.pi); // 3.141592653589793
console.log(lib.square(10)); // 100
console.log(new lib.Person("Lee")); // Person { name: 'Lee' }
```

<br>

모듈이 export한 식별자 이름을 변경하여 import할 수도 있다.

```js
[48 - 16];

// app.mjs
// lib.mjs 모듈이 export한 식별자 이름을 변경하여 import한다.
import { pi as PI, square as sq, Person as P } from "./lib.mjs";

console.log(PI); // 3.141592653589793
console.log(sq(2)); // 4
console.log(new P("Kim")); // Person { name: 'Kim' }
```

<br>

모듈에서 하나의 값만 export한다면 default 키워드를 사용할 수 있다.

- default 키워드를 사용하는 경우 기본적으로 이름 없이 하나의 값을 export 한다.

- default 키워드를 var, let, const 키워드는 사용할 수 없다.

- default 키워드와 함께 export한 모듈은 {} 없이 임의의 이름으로 import한다.

```js
[48-17]
// lib.mjs
export default x => x * x;

[48-18]
// lib.mjs
export default const foo = () => {};
// => SyntaxError: Unexpected token 'const'
// export default () => {};

[48-19]
// app.mjs
import square from './lib.mjs';

console.log(square(3)); // 9
```

<br>
<br>

> 모던 자바스크립트 Deep Dive

블로그에 작성한 파트 외 정리본은 [깃헙](https://github.com/sohyeonkim-0707/javascript-study.git)에서 확인 가능합니다.
