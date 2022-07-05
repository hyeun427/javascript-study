## 9.1 타입 변환이란?

자바스크립트의 모든 값은 타입이 있고, 값의 타입은 의도에 따라 다른 타입으로 변환할 수 있다.

**`의도적으로 값의 타입을 변환`**하는 것을 **`명시적 타입변환`** 또는 **`타입 캐스팅`**이라 한다.

> **예제 09-01**
>
> var x = 10;
>
> // 명시적 타입 변환
> // 숫자를 문자열로 타입 캐스팅한다.
> var str = x.toString();
> console.log(typeof str, str); // string 10
>
> // x 변수의 값이 변경된 것은 아니다.
> console.log(typeof x, x); // number 10

개발자의 의도와는 상관없이 자바스크립트 엔진에 의해 암묵적으로 타입이 자동 변환되기도 하는데, 이를 **`암묵적 타입 변환`** 또는 **`타입 강제 변환`**이라 한다.

> **예제 09-02**
>
> var x = 10;
>
> var str = x + ''; // 암묵적 타입 변환
> console.log(typeof str, str) // => string 10

명시적 타입 변환이나 암묵적 타입 변환이 기존 원시 값(위 예제의 x 변수의 값)을 직접 변경하는 것은 아니다. (원시 값은 변경 불가능한 값이므로 변경이 불가능 하다.)

타입 변환이란 기존 원시 값을 사용해 다른 타입의 **`새로운 원시 값을 생성`**하는 것이다.

개발자는 암묵적 타입 변환이 발생하는지, 발생한다면 어떤 타입의 어떤 값으로 변환 되는지, 그리고 타입 변환된 값으로 표현식이 어떻게 평가될 것인지 예측 가능해야 한다. **`예측하지 못한다면? 오류 발생 확률은 올라간다.`**

하지만 무조건 암묵적 타입 변환이 발생하지 않도록 코드를 작성한다면?
문법을 잘 이해하고 있는 개발자에게는 (10).toString() 보다 10+""이 간결하고 이해하기가 쉽기 때문에 가독성 측면에서도 좋을수가 있다.

---

## 9.2 암묵적 타입 변환

자바스크립트 엔진은 표현식을 평가할 때 개발자의 의도와는 상관없이 **`코드의 문맥을 고려`**해 **`암묵적으로 데이터 타입을 강제 변환`**할 때가 있다.

> **예제 09-03**
>
> // 피연산자가 모두 문자열 타입이어야 하는 문맥
> '10' + 2 // -> '102'
>
> // 피연산자가 모두 숫자 타입이어야 하는 문맥
> 5 \* '10' // -> 50
>
> // 피연산자 또는 표현식이 불리언 타입이어야 하는 문맥
> !0 // -> true
> if (1) { }

코드의 문맥에 부합하지 않는 다양한 상황이 발생할 수 있다. **`자바스크립트는 가급적 에러를 발생시키지 않도록 암묵적 타입 변환`**을 통해 표현식을 평가한다.

암묵적 타입 변환이 발생하면 문자열, 숫자, 불리언과 같은 원시 타입중 하나로 타입을 자동 변환한다.

> **예제 09-04**
>
> 1 + "2" // => "12"

09-04 예제의 + 연산자는 피연산자 중 하나 이상이 문자열이므로 문자열 연결 연산자로 동작한다.

자바스크립트 엔진은 문자열 연결 연산자 표현식을 평가하기 위해 문자열 연결 연산자의 피연산자 중에서 문자열 타입이 아닌 피연산자를 문자열 타입으로 암묵적 타입 변환하다.

> **예제 09-05**
>
> `1 + 1 = ${1 + 1}` // -> "1 + 1 = 2"
> ![](https://velog.velcdn.com/images/dooin/post/23f69e71-ef4c-4d4c-bb0d-82eb847d65d0/image.png)

템플릿 리터럴의 **`표현식 삽입은 표현식의 평가 결과`**를 문자열 타입으로 암묵적 타입 변환한다.

> **예제 09-06**
>
> // 숫자 타입
> 0 + '' // -> "0"
> -0 + '' // -> "0"
> 1 + '' // -> "1"
> -1 + '' // -> "-1"
> NaN + '' // -> "NaN"
> Infinity + '' // -> "Infinity"
> -Infinity + '' // -> "-Infinity"
>
> // 불리언 타입
> true + '' // -> "true"
> false + '' // -> "false"
>
> // null 타입
> null + '' // -> "null"
>
> // undefined 타입
> undefined + '' // -> "undefined"
>
> // 심벌 타입
> (Symbol()) + '' // -> TypeError: Cannot convert a Symbol value to a string
>
> // 객체 타입
> ({}) + '' // -> "[object Object]"
> Math + '' // -> "[object Math]"
> [] + '' // -> ""
> [10, 20] + '' // -> "10,20"
> (function(){}) + '' // -> "function(){}"
> Array + '' // -> "function Array() { [native code] }"

> **예제 09-07**
> 1 - '1' // -> 0
> 1 \* '10' // -> 10
> 1 / 'one' // -> NaN

위 예제에서 사용한 연산자는 모두 산술 연산자이다. 산술 연산자의 역할은 숫자 값을 만드는 것이다. 이때 숫자 타입을 변환할 수 없는 경우는 NaN이 된다.

> **예제 09-08**
> '1' > 0 // -> true

비교연산자의 역할은 불리언 값을 만드는 것이다. >피연산자의 크기를 비교하므로 모든 피연산자는 **`코드 문맥상 모두 숫자 타입`**이어야 하기 때문에 암묵적 타입 변환한다.

> **예제 09-09**
> // 문자열 타입
> +'' // -> 0
> +'0' // -> 0
> +'1' // -> 1
> +'string' // -> NaN
>
> // 불리언 타입
> +true // -> 1
> +false // -> 0
>
> // null 타입
> +null // -> 0
>
> // undefined 타입
> +undefined // -> NaN
>
> // 심벌 타입
> +Symbol() // -> ypeError: Cannot convert a Symbol value to a number
>
> // 객체 타입
> +{} // -> NaN
> +[] // -> 0 +[10, 20] // -> NaN
> +(function(){}) // -> NaN

빈 문자열 (""), 빈 배열([]), null, false는 0으로, true는 1로 변환된다. 객체와 빈 배열이 아닌 배열, undefined는 변환되지 않아 NaN이 된다는 것에 주의하자.

> **예제 09-11** > ![](https://velog.velcdn.com/images/dooin/post/3d4f2024-2e73-4bf6-9ba9-226d286db291/image.png)

자바스크립트 엔진은 불리언 타입이 아닌 값을 Truthy 값(참으로 평가되는 값) 또는 Falsy 값(거짓으로 평가되는 값)으로 구분한다.
![](https://velog.velcdn.com/images/dooin/post/6356dee6-63cd-4881-8a91-c2aa00c17278/image.png)

**예제 09-12**

> // 아래의 조건문은 모두 코드 블록을 실행한다.
> if (!false) console.log(false + ' is falsy value');
> if (!undefined) console.log(undefined + ' is falsy value');
> if (!null) console.log(null + ' is falsy value');
> if (!0) console.log(0 + ' is falsy value');
> if (!NaN) console.log(NaN + ' is falsy value');
> if (!'') console.log('' + ' is falsy value');
> ![](https://velog.velcdn.com/images/dooin/post/092de659-04da-4795-837d-df5e3c9625d0/image.png)

**예제 09-13**

> // 전달받은 인수가 Falsy 값이면 true, Truthy 값이면 false를 반환한다.
> function isFalsy(v) {
> return !v;
> }
>
> // 전달받은 인수가 Truthy 값이면 true, Falsy 값이면 false를 반환한다.
> function isTruthy(v) {
> return !!v;
> }
>
> // 모두 true를 반환한다.
> isFalsy(false);
> isFalsy(undefined);
> isFalsy(null);
> isFalsy(0);
> isFalsy(NaN);
> isFalsy('');
>
> // 모두 true를 반환한다.
> isTruthy(true);
> isTruthy('0'); // 빈 문자열이 아닌 문자열은 Truthy 값이다.
> isTruthy({});
> isTruthy([]);

---

## 9.3 명시적 타입 변환

문자열 타입이 아닌 값을 문자열 타입으로 변환하는 방법은 다음과 같다.

-   **`String 생성자 함수`**를 new 연산자 없이 호출하는 방법
-   Object.prototype.toString **`메서드를 사용하는 방법`**
-   **`문자열 연결 연산자`**를 이용하는 방법

> **예제 09-14**
> // 1. String 생성자 함수를 new 연산자 없이 호출하는 방법
> // 숫자 타입 => 문자열 타입
> String(1); // -> "1"
> String(NaN); // -> "NaN"
> String(Infinity); // -> "Infinity"
> // 불리언 타입 => 문자열 타입
> String(true); // -> "true"
> String(false); // -> "false"
>
> // 2. Object.prototype.toString 메서드를 사용하는 방법
> // 숫자 타입 => 문자열 타입
> (1).toString(); // -> "1"
> (NaN).toString(); // -> "NaN"
> (Infinity).toString(); // -> "Infinity"
> // 불리언 타입 => 문자열 타입
> (true).toString(); // -> "true"
> (false).toString(); // -> "false"
>
> // 3. 문자열 연결 연산자를 이용하는 방법
> // 숫자 타입 => 문자열 타입
> 1 + ''; // -> "1"
> NaN + ''; // -> "NaN"
> Infinity + ''; // -> "Infinity"
> // 불리언 타입 => 문자열 타입
> true + ''; // -> "true"
> false + ''; // -> "false"

숫자 타입이 아닌 값을 숫자 타입으로 변환하는 방법은 다음과 같다.

-   **`Number 생성자 함수`**를 new 연산자 없이 호출하는 방법
-   **`parseInt, parseFloat 함수`**를 사용하는 방법 (문자열만 숫자 타입으로 변환가능)
-   **`+ 단항 산술 연산자`**를 이용하는 방법
-   **`산술 연산자`**를 이용하는 방법

> **예제 09-15**
> // 1. Number 생성자 함수를 new 연산자 없이 호출하는 방법
> // 문자열 타입 => 숫자 타입
> Number('0'); // -> 0
> Number('-1'); // -> -1
> Number('10.53'); // -> 10.53
> // 불리언 타입 => 숫자 타입
> Number(true); // -> 1
> Number(false); // -> 0
>
> // 2. parseInt, parseFloat 함수를 사용하는 방법(문자열만 변환 가능)
> // 문자열 타입 => 숫자 타입
> parseInt('0'); // -> 0
> parseInt('-1'); // -> -1
> parseFloat('10.53'); // -> 10.53
>
> // 3. + 단항 산술 연산자를 이용하는 방법
> // 문자열 타입 => 숫자 타입
> +'0'; // -> 0
> +'-1'; // -> -1
> +'10.53'; // -> 10.53
> // 불리언 타입 => 숫자 타입
> +true; // -> 1
> +false; // -> 0
>
> // 4. _ 산술 연산자를 이용하는 방법
> // 문자열 타입 => 숫자 타입
> '0' _ 1; // -> 0
> '-1' _ 1; // -> -1
> '10.53' _ 1; // -> 10.53
> // 불리언 타입 => 숫자 타입
> true _ 1; // -> 1
> false _ 1; // -> 0

불리언 타입이 아닌 값을 불리언 타입으로 변환하는 방법은 다음과 같다.

-   **`Boolean 생성자 함수`**를 new 연산자 없이 호출하는 방법
-   **`! 부정 논리 연산자를 두번 사용`**하는 방법

> **예제 09-16**
> // 1. Boolean 생성자 함수를 new 연산자 없이 호출하는 방법
> // 문자열 타입 => 불리언 타입
> Boolean('x'); // -> true
> Boolean(''); // -> false
> Boolean('false'); // -> true
> // 숫자 타입 => 불리언 타입
> Boolean(0); // -> false
> Boolean(1); // -> true
> Boolean(NaN); // -> false
> Boolean(Infinity); // -> true
> // null 타입 => 불리언 타입
> Boolean(null); // -> false
> // undefined 타입 => 불리언 타입
> Boolean(undefined); // -> false
> // 객체 타입 => 불리언 타입
> Boolean({}); // -> true
> Boolean([]); // -> true
>
> // 2. ! 부정 논리 연산자를 두번 사용하는 방법
> // 문자열 타입 => 불리언 타입
> !!'x'; // -> true
> !!''; // -> false
> !!'false'; // -> true
> // 숫자 타입 => 불리언 타입
> !!0; // -> false
> !!1; // -> true
> !!NaN; // -> false
> !!Infinity; // -> true
> // null 타입 => 불리언 타입
> !!null; // -> false
> // undefined 타입 => 불리언 타입
> !!undefined; // -> false
> // 객체 타입 => 불리언 타입
> !!{}; // -> true
> !![]; // -> true

---

## 9.4 단축 평가

논리합(||) 또는 논리곱(&&) 연산자 표현식의 평가 결과는 불리언 값이 아닐수도 있다. 논리합 또는 논리곱 연산자 표현식은 2개의 피연산자 중 어느 한쪽으로 평가된다.

> **예제 09-17**
> 'Cat' && 'Dog' // -> "Dog"

논리곱 연산자는 두 개의 피연산자가 모두 true로 평가될 때 true를 반환한다. 논리곱 연산자는 좌항에서 우항으로 평가가 진행된다.

두 번째 피연산자가 위 논리곱 연산자 표현식의 평가 결과를 결정한다.

이때 논리곱 연산자는 논리 연산의 결과를 결정하는 두 번째 피연산자. 즉 문자열 'Dog'를 그대로 반환한다.

**예제 09-18**

> 'Cat' || 'Dog' // -> "Cat"

첫번째 피연산자 'Cat'은 Truthy 값이므로 true로 평가된다. 이 시점에 두 번째 피연산자까지 평가해 보지 않아도 위 표현식을 평가할 수 있다.

이때 논리합 연산자는 논리 연산의 결과를 결정한 첫 번째 피연산자, 즉 문자열 'Cat'을 그대로 반환한다.

단축 평가는 표현식을 평가하는 도중에 평가 결과가 확정된 경우 나머지 평가 과정을 생략하는 것을 말한다.

> **예제 09-19**
> // 논리합(||) 연산자
> 'Cat' || 'Dog' // -> "Cat"
> false || 'Dog' // -> "Dog"
> 'Cat' || false // -> "Cat"
>
> // 논리곱(&&) 연산자
> 'Cat' && 'Dog' // -> "Dog"
> false && 'Dog' // -> false
> 'Cat' && false // -> false

> **예제 09-20**
> var done = true;
> var message = '';
>
> // 주어진 조건이 true일 때
> if (done) message = '완료';
>
> // if 문은 단축 평가로 대체 가능하다.
> // done이 true라면 message에 '완료'를 할당
> message = done && '완료';
> console.log(message); // 완료

![](https://velog.velcdn.com/images/dooin/post/b1b1366a-59e5-4ee7-9e3b-0267295339a3/image.png)

![](https://velog.velcdn.com/images/dooin/post/f8a91e28-580b-4fea-84b9-d7eee26da9a3/image.png)

**`단축 평가를 사용하면 if 문을 대체`**할 수 있다. 어떤 조건이 Truthy 값일 때 무언가를 해야 한다면 논리곱(&&) 연산자 표현식으로 if 문을 대체할 수 있다.

> **예제 09-21**
> var done = false;
> var message = '';
>
> // 주어진 조건이 false일 때
> if (!done) message = '미완료';
>
> // if 문은 단축 평가로 대체 가능하다.
> // done이 false라면 message에 '미완료'를 할당
> message = done || '미완료';
> console.log(message); // 미완료

![](https://velog.velcdn.com/images/dooin/post/a9e0526f-e9eb-4aac-bec4-18f84cce95ce/image.png)

![](https://velog.velcdn.com/images/dooin/post/e1e1fc45-6341-4de3-bfd5-5d41c66933d4/image.png)

조건이 Falsy 값일 때 무언가를 해야 한다면 **`논리합(||) 연산자 표현식으로 if 문을 대체`**할 수 있다.

> **예제 09-22**
> var done = true;
> var message = '';
>
> // if...else 문
> if (done) message = '완료';
> else message = '미완료';
> console.log(message); // 완료
>
> // if...else 문은 삼항 조건 연산자로 대체 가능하다.
> message = done ? '완료' : '미완료';
> console.log(message); // 완료

**`삼항 조건 연산자는 if else 문을 대체`**할 수 있다.

> **예제 09-24**
> var elem = null;
> // elem이 null이나 undefined와 같은 Falsy 값이면 elem으로 평가되고
> // elem이 Truthy 값이면 elem.value로 평가된다.
> var value = elem && elem.value; // -> null

![](https://velog.velcdn.com/images/dooin/post/cee67860-625c-41eb-bd6d-587026483e66/image.png)

> **예제 09-25**
> // 단축 평가를 사용한 매개변수의 기본값 설정
> function getStringLength(str) {
> str = str || '';
> return str.length;
> }
>
> getStringLength(); // -> 0
> getStringLength('hi'); // -> 2
>
> // ES6의 매개변수의 기본값 설정
> function getStringLength(str = '') {
> return str.length;
> }
>
> getStringLength(); // -> 0
> getStringLength('hi'); // -> 2

![](https://velog.velcdn.com/images/dooin/post/98f97dcc-0f05-4037-8cdb-779306ac285d/image.png)

![](https://velog.velcdn.com/images/dooin/post/c7d03b5e-2a11-43e6-9357-b6701eb2d2ac/image.png)

**`함수를 호출할 때 인수를 전달하지 않으면 매개변수에는 undefined가 할당`**된다. 이때 단축 평가를 사용해 매개변수의 기본값을 설정하면 undefined로 인해 발생할 수 있는 에러를 방지할 수 있다.

> **예제 09-26**
> var elem = null;
>
> // elem이 null 또는 undefined이면 undefined를 반환하고, 그렇지 않으면 우항의 프로퍼티 참조를 이어간다.
> var value = elem?.value;
> console.log(value); // undefined

**`옵셔널 체이닝 연산자 ?.는 좌항의 피연산자가 null 또는 undefined인 경우 undefined를 반환`**하고, 그렇지 않으면 우항의 프로퍼티 참조를 이어간다.

> **예제 09-27**
> var elem = null;
>
> // elem이 Falsy 값이면 elem으로 평가되고 elem이 Truthy 값이면 elem.value로 평가된다.
> var value = elem && elem.value;
> console.log(value); // null

좌항 피연산자가 Falsy 값이면 좌항 피연산자를 그대로 반환한다.

> **예제 09-32**
> // 좌항의 피연산자가 Falsy 값이라도 null 또는 undefined이 아니면 좌항의 피연산자를 반환한다.
> var foo = '' ?? 'default string';
> console.log(foo); // ""

**`ES11에서 도입된 nuyll 병합 연산자 ?? 는 좌항의 피연산자가 null 또는 undefined인 경우 우항의 피연산자를 반환`**하고, **`그렇지 않으면 좌항의 피연산자를 반환`**한다. null 병합 연산자 ??는 변수에 기본값을 설정할 때 유용하다.
