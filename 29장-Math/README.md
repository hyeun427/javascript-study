## 29.1 Math 프로퍼티

### 29.1.1 Math.PI

- 원주율 PI값을 반환한다.

```js
Math.PI; // -> 3.141592653589793
```

---

## 29.2 Math 메서드

### 29.2.1 Math.abs

- 인수로 전달된 숫자의 절대값을 반환한다.
- 절대값은 반드시 `0` 또는 양수여야 한다.

```js
Math.abs(-1); // -> 1
Math.abs("-1"); // -> 1
Math.abs(""); // -> 0
Math.abs([]); // -> 0
Math.abs(null); // -> 0
Math.abs(undefined); // -> NaN
Math.abs({}); // -> NaN
Math.abs("string"); // -> NaN
Math.abs(); // -> NaN
```

### 29.2.2 Math.round

- 인수로 전달된 숫자의 소수점 이하를 **반올림**한 정수를 반환한다.

```js
Math.round(1.4); // -> 1
Math.round(1.6); // -> 2
Math.round(-1.4); // -> -1
Math.round(-1.6); // -> -2
Math.round(1); // -> 1
Math.round(); // -> NaN
```

### 29.2.3 Math.ceil

- 인수로 전달된 숫자의 소수점 이하를 **올림**한 정수를 반환한다.

```js
Math.ceil(1.4); // -> 2
Math.ceil(1.6); // -> 2
Math.ceil(-1.4); // -> -1
Math.ceil(-1.6); // -> -1
Math.ceil(1); // -> 1
Math.ceil(); // -> NaN
```

### 29.2.4 Math.floor

- 인수로 전달된 숫자의 소수점 이하를 **내림**한 정수를 반환한다.

```js
Math.floor(1.9); // -> 1
Math.floor(9.1); // -> 9
Math.floor(-1.9); // -> -2
Math.floor(-9.1); // -> -10
Math.floor(1); // -> 1
Math.floor(); // -> NaN
```

### 29.2.5 Math.sqrt

- 인수로 전달된 숫자의 **제곱근**을 반환한다.

```js
Math.sqrt(9); // -> 3
Math.sqrt(-9); // -> NaN
Math.sqrt(2); // -> 1.414213562373095
Math.sqrt(1); // -> 1
Math.sqrt(0); // -> 0
Math.sqrt(); // -> NaN
```

### 29.2.6 Math.random

- 임의의 난수(랜덤 숫자)를 반환한다.
- 반환한 난수는 `0 포함, 1 미만`의 실수이다.

### 29.2.7 Math.pow

- 첫 번째 인수를 밑으로, 두 번째 인수를 지수로 **거듭제곱**한 결과를 반환한다.

```js
Math.pow(2, 8); // 2의 8승 -> 256
Math.pow(2, -1); // 2의 -1승 -> 0.5
Math.pow(2); // NaN
```

### 29.2.8 Math.max

- 전달받은 인수 중에서 가장 큰 수를 반환한다.
- 인수가 전달되지 않으면 `-Infinity`를 반환한다.
- 전달받은 인수가 배열이라면 그 배열의 요소 중에서 최대값을 구해야한다. 최대값을 구하려면 `Function.prototype.apply 메서드` 또는 `스프레드 문법`을 사용해야한다.

```js
// 배열 요소 중에서 최대값 취득
Math.max.apply(null, [1, 2, 3]); // -> 3

// ES6 스프레드 문법
Math.max(...[1, 2, 3]); // -> 3
```

### 29.2.9 Math.min

- 전달받은 인수 중에서 가장 작은 수를 반환한다.
- 인수가 전달되지 않으면 `Infinity`를 반환한다.
- 전달받은 인수가 배열이라면 그 배열의 요소 중에서 최소값을 구해야한다. 최대값을 구하려면 `Function.prototype.apply 메서드` 또는 `스프레드 문법`을 사용해야한다.

```js
// 배열 요소 중에서 최소값 취득
Math.min.apply(null, [1, 2, 3]); // -> 1

// ES6 스프레드 문법
Math.min(...[1, 2, 3]); // -> 1
```
