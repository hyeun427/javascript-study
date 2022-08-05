## 27.1 배열이란?

배열은 여러 개의 값을 순차적으로 나열한 자료구조이며, **사용 빈도가 매우 높은** 가장 기본적인 자료구조이다.

배열이 가지고 있는 값을 요소라고 부른다. 자바스크립트의 모든 값은 배열의 요소가 될 수 있다. 즉, 원시값은 물론 객체, 함수, 배열 등 자바스크립트에서 값으로 인정하는 모든 것은 배열의 요소가 될 수 있다.

배열의 요소는 배열에서 자신의 위치를 나타내는 0이상의 정수인 인덱스를 갖는다. 이 인덱스는 배열의 요소에 접근할 때 사용한다. 요소에 접근할 때는 대괄호 표기법을 사용한다.

**예제 27-03**

```jsx
arr.length; // -> 3
```

배열은 요소의 개수, 즉 배열의 길이를 나타내는 length 프로퍼티를 갖는다.

**예제 27-05**

```jsx
typeof arr; // -> object
```

자바스크립트에 배열이라는 타입은 존재하지 않는다. 배열은 객체 타입이며 배열 리터럴, Array 생성자 함수, Array.of, Array.from 메서드로 생성할 수 있다.

배열은 객체지만 일반 객체와는 구별되는 독특한 특징이 있다.
![](https://velog.velcdn.com/images/dooin/post/e7ebb586-bd50-4965-8755-04fd4f20eb04/image.png)
일반 객체와 배열을 구분하는 가장 명확한 차이는 "`값의 순서`"와 "`length 프로퍼티`"다.

배열의 장점은 처음부터 순차적으로 요소에 접근할 수도 있고, 마지막부터 역순으로 요소에 접근할 수도 있으며, 특정 위치부터 순차적으로 요소에 접근할 수도 있다는 것이다. **배열이 인덱스와 length 프로퍼티를 갖기 때문에 가능한 것이다.**

---

## 27.2 자바스크립트 배열은 배열이 아니다.

일반적인 의미의 배열은 각 요소가 동일한 데이터 크기를 가지고, 빈틈없이 연속적으로 이어져 있으며, 인덱스를 통해 단 한번의 연산으로 임의의 요소에 접근 가능하여 매우 효율적이고 고속으로 동작한다.

정렬되지 않은 배열에서 특정한 요소를 검색하는 경우 배열의 모든 요소를 처음부터 특정 요소를 발견할 때까지 차례대로 검색해야한다. 또한 배열에 요소를 삽입하거나 삭제하는 경우 배열의 요소를 연속적으로 유지하기 위해 요소를 이동시켜야하는 단점도 있다.

자바스크립트의 배열은 자료구조에서 말하는 일반적인 의미의 배열과 다른 배열을 갖는다. 배열의 요소를 위한 각각의 메모리 공간은 동일한 크기를 갖지 않아도 되며, 연속적으로 이어져 있지 않을 수도 있다.

아초롬 자바스크립트의 배열은 어밀히 말해 일반적 의미의 배열이 아니다. 자바스크립트의 배열은 일반적인 배열의 동작을 흉내 낸 특수한 객체이다.

**예제 27-09**

```jsx
// "16.2. 프로퍼티 어트리뷰트와 프로퍼티 디스크립터 객체" 참고
console.log(Object.getOwnPropertyDescriptors([1, 2, 3]));
/*
{
  '0': {value: 1, writable: true, enumerable: true, configurable: true}
  '1': {value: 2, writable: true, enumerable: true, configurable: true}
  '2': {value: 3, writable: true, enumerable: true, configurable: true}
  length: {value: 3, writable: true, enumerable: false, configurable: false}
}
*/
```

예제09 코드처럼 자바스크립트 배열은 인덱스를 나타내는 문자열을 프로퍼티 키로 가지며, length 프로퍼티를 갖는 특수한 객체이다. 자바스크립트 배열의 요소는 사실 프로퍼티 값이다. 자바스크립트에서 사용할 수 있는 모든 값은 객체의 프로퍼티 값이 될 수 있으므로 어떤 타입의 값이라도 배열의 요소가 될 수 있다.

정리하자면 일반적인 배열은 인덱스 요소에 빠르게 접근할 수 있지만 특정 요소를 검색하거나 요소를 삽입 또는 삭제하는 경우에는 효율적이지 않다.

자바스크립트 배열은 인덱스 요소에 접근하는 경우 일반적인 배열보다 성능적인 면에서는 느리지만, 특정 요소를 검색하거나 삽입 또는 삭제하는 경우에는 빠른 성능을 기대할 수 있다.

**예제 27-11**

```jsx
const arr = [];

console.time("Array Performance Test");

for (let i = 0; i < 10000000; i++) {
    arr[i] = i;
}
console.timeEnd("Array Performance Test");
// 약 340ms

const obj = {};

console.time("Object Performance Test");

for (let i = 0; i < 10000000; i++) {
    obj[i] = i;
}

console.timeEnd("Object Performance Test");
// 약 600ms
```

![](https://velog.velcdn.com/images/dooin/post/de084e67-f7d8-4e84-ac03-e26070b2e321/image.png)

---

## **27.3 length 프로퍼티와 희소 배열**

length 프로퍼티는 요소의 개수, 즉 배열의 길이를 나타내는 0 이상의 정수를 값으로 갖는다. 빈 배열일 경우 0이며, 빈 배열이 아닐 경우 가장 큰 인덱스에 1을 더한 것과 같다. 또한 배열에 요소를 추가하거나 삭제하면 자동 갱신된다.

**예제 27-13**

```jsx
const arr = [1, 2, 3];
console.log(arr.length); // 3

// 요소 추가
arr.push(4);
// 요소를 추가하면 length 프로퍼티의 값이 자동 갱신된다.
console.log(arr.length); // 4

// 요소 삭제
arr.pop();
// 요소를 삭제하면 length 프로퍼티의 값이 자동 갱신된다.
console.log(arr.length); // 3
```

**예제 27-14**

```jsx
const arr = [1, 2, 3, 4, 5];

// 현재 length 프로퍼티 값인 5보다 작은 숫자 값 3을 length 프로퍼티에 할당
arr.length = 3;

// 배열의 길이가 5에서 3으로 줄어든다.
console.log(arr); // [1, 2, 3]
```

length 프로퍼티 값은 요소의 개수, 즉 배열의 길이를 바탕으로 결정되지만 임의의 숫자 값을 명시적으로 할당할 수도 있다. 예제 14 코드와 같이 length 프로퍼티 값보다 작은 숫자 값을 할당하면 배열의 길이가 줄어든다.

**예제 27-15**

```jsx
const arr = [1];

// 현재 length 프로퍼티 값인 1보다 큰 숫자 값 3을 length 프로퍼티에 할당
arr.length = 3;

// length 프로퍼티 값은 변경되지만 실제로 배열의 길이가 늘어나지는 않는다.
console.log(arr.length); // 3
console.log(arr); // [1, empty × 2]
```

주의할 것은 현재 length 프로퍼티 값보다 큰 숫자 값을 할당하는 경우인데, 이때 length 프로퍼티 값은 변경되지만 실제로 배열의 길이가 늘어나지는 않는다. 예제 15의 코드를 보면 실제로 추가된 배열의 요소가 아니다. 즉, 값이 존재하지 않는다.

배열의 요소가 연속적으로 위치하지 않고 일부가 비어 있는 배열을 희소 배열이라 하는데, 자바스크립트는 희소 배열을 문법적으로 허용한다.

**예제 27-17**

```jsx
// 희소 배열
const sparse = [, 2, , 4];

// 희소 배열의 length 프로퍼티 값은 요소의 개수와 일치하지 않는다.
console.log(sparse.length); // 4
console.log(sparse); // [empty, 2, empty, 4]

// 배열 sparse에는 인덱스가 0, 2인 요소가 존재하지 않는다.
console.log(Object.getOwnPropertyDescriptors(sparse));
/*
{
  '1': { value: 2, writable: true, enumerable: true, configurable: true },
  '3': { value: 4, writable: true, enumerable: true, configurable: true },
  length: { value: 4, writable: true, enumerable: false, configurable: false }
}
*/
```

일반적인 배열의 length는 배열 요소의 개수, 즉 배열의 길이와 언제나 일치한다. 하지만 희소 배열은 length와 배열 요소의 개수가 일치하지 않는다. 희소 배열의 length는 희소 배열의 실제 요소 개수보다 언제나 크다.

자바스크립트는 문법적으로 희소 배열을 허용하지만 사용하지 않는 것이 좋다. 의도적으로 희소배열을 만들어야 하는 상황은 발생하지 않는다. 배열에는 같은 타입의 요소를 연속적으로 위치시키는 것이 최선이다.

---

## 27.4 배열 생성

### 27.4.1 배열 리터럴

객체와 마찬가지로 배열도 다양한 생성 방식이 있다. 가장 일반적이고 간편한 배열 생성 방식은 배열 리터럴을 사용하는 것이다.

**예제 27-18**

```jsx
const arr = [1, 2, 3];
console.log(arr.length); // 3
```

### 27.4.2 Array 생성자 함수

Object 생성자 함수를 통해 객체를 생성할 수 있듯이 Array 생성자 함수를 통해 배열을 생성할 수도 있다. Array 생성자 함수는 전달된 인수의 개수에 따라 다르게 동작하므로 주의가 필요하다.

**예제 27-25**

```jsx
// 전달된 인수가 2개 이상이면 인수를 요소로 갖는 배열을 생성한다.
new Array(1, 2, 3); // -> [1, 2, 3]

// 전달된 인수가 1개지만 숫자가 아니면 인수를 요소로 갖는 배열을 생성한다.
new Array({}); // -> [{}]
```

### **27.4.3 Array.of**

ES6에서 도입된 Array.of 메서드는 전달된 인수를 요소로 갖는 배열을 생성한다. Array.of는 Array 생성자 함수와 다르게 전달된 이눗가 1개이고 숫자이더라도 인수를 요소로 갖는 배열을 생성한다.

### 27.4.4 Array.from

ES6에서 도입된 Array.from 메서드는 유사 배열 객체 또는 이터러블 객체를 인수로 전달받아 배열로 변환하여 반환한다.

**예제 27-28**

```jsx
// 유사 배열 객체를 변환하여 배열을 생성한다.
Array.from({ length: 2, 0: "a", 1: "b" }); // -> ['a', 'b']

// 이터러블을 변환하여 배열을 생성한다. 문자열은 이터러블이다.
Array.from("Hello"); // -> ['H', 'e', 'l', 'l', 'o']
```

유사 배열 객체는 마치 배열처럼 인덱스로 프로퍼티 값에 접근할 수 있고, length 프로퍼티를 갖는 객체를 말한다. (이터러블 객체는 뒤에서 자세히 설명...)

---

## 27.5 배열 요소의 참조

배열의 요소를 참조할 때에는 대괄호([]) 표기법을 사용한다. 대괄호 안에는 인덱스가 와야 한다. 값을 참조할 수 있다는 의미에서 객체의 프로퍼티 키톼 같은 역할을 한다.

**예제 27-31**

```jsx
const arr = [1, 2];

// 인덱스가 0인 요소를 참조
console.log(arr[0]); // 1
// 인덱스가 1인 요소를 참조
console.log(arr[1]); // 2
```

**예제 27-32**

```jsx
const arr = [1, 2];

// 인덱스가 2인 요소를 참조. 배열 arr에는 인덱스가 2인 요소가 존재하지 않는다.
console.log(arr[2]); // undefined
```

존재하지 않는 요소에 접근하면 undefined가 반환된다.

---

## 27.6 배열 요소의 추가와 갱신

객체에 프로퍼티를 동적으로 추가할 수 있는 것처럼 배열에도 요소를 동적으로 추가 가능하다.

**예제 27-34**

```jsx
const arr = [0];

// 배열 요소의 추가
arr[1] = 1;

console.log(arr); // [0, 1]
console.log(arr.length); // 2
```

존재하지 않는 인덱스를 사용해 값을 할당하면 새로운 요소가 추가된다.

**예제 27-37**

```jsx
// 요소값의 갱신
arr[1] = 10;

console.log(arr); // [0, 10, empty × 98, 100]
```

이미 요소가 존재하는 요소에 값을 재할당하면 요소값이 갱신된다.

**예제 27-38**

```jsx
const arr = [];

// 배열 요소의 추가
arr[0] = 1;
arr["1"] = 2;

// 프로퍼티 추가
arr["foo"] = 3;
arr.bar = 4;
arr[1.1] = 5;
arr[-1] = 6;

console.log(arr); // [1, 2, foo: 3, bar: 4, '1.1': 5, '-1': 6]

// 프로퍼티는 length에 영향을 주지 않는다.
console.log(arr.length); // 2
```

인덱스는 요소의 위치를 나타내므로 반드시 0이상의 정수(또는 정수 형태의 문자열)를 사용해야 한다. 만약 정수 이외의 값을 인덱스처럼 사용하면 요소가 생성되는 것이 아니라 프로퍼티가 생성되며, 추가된 프로퍼티는 length 프로퍼티 값에 영향을 주지 않는다.

---

## 27.7 배열 요소의 삭제

배열은 사실 객체이기 때문에 배열의 특정 요소를 삭제하기 위해 delete 연산자를 사용할 수 있다.

**예제 27-39**

```jsx
const arr = [1, 2, 3];

// 배열 요소의 삭제
delete arr[1];
console.log(arr); // [1, empty, 3]

// length 프로퍼티에 영향을 주지 않는다. 즉, 희소 배열이 된다.
console.log(arr.length); // 3
```

이때 배열은 희소 배열이 되기 때문에 delete 연산자는 사용하지 않는 것이 좋다. 완전히 삭제하려면 Array.prototype.splice 메서드를 사용한다.

---

## 27.8 배열 메서드

배열 메서드는 결과물을 반환하는 패턴이 두 가지이므로 주의가 필요하다. 원본 배열을 직접 변경하는 메서드와 원본 배열을 직접 변경하지 않고 새로운 배열을 생성하여 반환하는 메서드가 있다.

**예제 27-41**

```jsx
const arr = [1];

// push 메서드는 원본 배열(arr)을 직접 변경한다.
arr.push(2);
console.log(arr); // [1, 2]

// concat 메서드는 원본 배열(arr)을 직접 변경하지 않고 새로운 배열을 생성하여 반환한다.
const result = arr.concat(3);
console.log(arr); // [1, 2]
console.log(result); // [1, 2, 3]
```

원본 배열을 직접 변경하는 메서드는 외부 상태를 직접 변경하는 부수 효과가 있으므로 사용할 때 주의해야 한다. 가급적 원본 배열을 직접 변경하지 않는 메서드를 사용하는 편이 좋다.

### 27.8.1 Array.isArray

**예제 27-42**

```jsx
// true
Array.isArray([]);
Array.isArray([1, 2]);
Array.isArray(new Array());

// false
Array.isArray();
Array.isArray({});
Array.isArray(null);
Array.isArray(undefined);
Array.isArray(1);
Array.isArray("Array");
Array.isArray(true);
Array.isArray(false);
Array.isArray({ 0: 1, length: 1 });
```

전달된 인수가 배열이면 true, 배열이 아니면 false를 반환한다.

### 27.8.2 Array.prototype.indexOf

**예제 27-43**

```jsx
const arr = [1, 2, 2, 3];

// 배열 arr에서 요소 2를 검색하여 첫 번째로 검색된 요소의 인덱스를 반환한다.
arr.indexOf(2); // -> 1
// 배열 arr에 요소 4가 없으므로 -1을 반환한다.
arr.indexOf(4); // -> -1
// 두 번째 인수는 검색을 시작할 인덱스다. 두 번째 인수를 생략하면 처음부터 검색한다.
arr.indexOf(2, 2); // -> 2
```

원본 배열에서 인수로 전달된 요소를 검색하여 인덱스를 반환한다.

### 27.8.3 Array.prototype.push

**예제 27-46**

```jsx
const arr = [1, 2];

// 인수로 전달받은 모든 값을 원본 배열 arr의 마지막 요소로 추가하고 변경된 length 값을 반환한다.
let result = arr.push(3, 4);
console.log(result); // 4

// push 메서드는 원본 배열을 직접 변경한다.
console.log(arr); // [1, 2, 3, 4]
```

인수로 전달받은 모든 값을 원본 배열의 마지막 요소로 추가하고 변경된 length 프로퍼티 값을 반환한다. **원본 배열을 직접 변경한다.**

push 메서드는 성능 면에서 좋지 않다. 마지막 요소로 추가할 요소가 하나뿐이라면 push 메서드를 사용하지 않고 length 프로퍼티를 사용하여 배열의 마지막에 요소를 직접 추가할 수도 있다. 이 방법이 더 빠르다.

**예제 27-47**

```jsx
const arr = [1, 2];

// arr.push(3)과 동일한 처리를 한다. 이 방법이 push 메서드보다 빠르다.
arr[arr.length] = 3;
console.log(arr); // [1, 2, 3]
```

### 27.8.4 Array.prototype.pop

**예제 27-49**

```jsx
const arr = [1, 2];

// 원본 배열에서 마지막 요소를 제거하고 제거한 요소를 반환한다.
let result = arr.pop();
console.log(result); // 2

// pop 메서드는 원본 배열을 직접 변경한다.
console.log(arr); // [1]
```

원본 배열에서 마지막 요소를 제거하고 제거한 요소를 반환한다. 원본 배열에 빈 배열이면 undefined를 반환한다. **원본 배열을 직접 변경한다.**

### 27.8.5 Array.prototype.unshift

**예제 27-52**

```jsx
const arr = [1, 2];

// 인수로 전달받은 모든 값을 원본 배열의 선두에 요소로 추가하고 변경된 length 값을 반환한다.
let result = arr.unshift(3, 4);
console.log(result); // 4

// unshift 메서드는 원본 배열을 직접 변경한다.
console.log(arr); // [3, 4, 1, 2]
```

인수로 전달받은 모든 값을 원본 배열의 선두에 요소로 추가하고 변경된 length 프로퍼티 값을 반환한다. **원본 배열을 직접 변경한다.**

### 27.8.5 Array.prototype.shift

**예제 27-54**

```jsx
const arr = [1, 2];

// 원본 배열에서 첫 번째 요소를 제거하고 제거한 요소를 반환한다.
let result = arr.shift();
console.log(result); // 1

// shift 메서드는 원본 배열을 직접 변경한다.
console.log(arr); // [2]
```

우너본 배열에서 첫 번째 요소를 제거하고 제거한 요소를 반환한다. 원본 배열이 빈 배열이면 undefined를 반환한다. **원본 배열을 직접 변경한다.**

### 27.8.7 Array.prototype.concat

**예제 27-57**

```jsx
const arr1 = [1, 2];
const arr2 = [3, 4];

// 배열 arr2를 원본 배열 arr1의 마지막 요소로 추가한 새로운 배열을 반환한다.
// 인수로 전달한 값이 배열인 경우 배열을 해체하여 새로운 배열의 요소로 추가한다.
let result = arr1.concat(arr2);
console.log(result); // [1, 2, 3, 4]

// 숫자를 원본 배열 arr1의 마지막 요소로 추가한 새로운 배열을 반환한다.
result = arr1.concat(3);
console.log(result); // [1, 2, 3]

// 배열 arr2와 숫자를 원본 배열 arr1의 마지막 요소로 추가한 새로운 배열을 반환한다.
result = arr1.concat(arr2, 5);
console.log(result); // [1, 2, 3, 4, 5]

// 원본 배열은 변경되지 않는다.
console.log(arr1); // [1, 2]
```

인수로 전달된 값을(배열 또는 원시값)을 원본 배열의 마지막 요소로 추가한 새로운 배열을 반환한다. 인수로 전달한 값이 배열인 경우 배열을 해체하여 새로운 배열의 요소로 추가한다.

concat 메서드는 ES6의 스프레드 문법으로 대체할 수 있다. 결론적으로 push/unshift 메서드와 concat 메서드 대신 ES6의 스프레드 문법을 일관성 있게 사용하는 것을 권장한다.

### 27.8.8 Array.prototype.splice

**예제 27-61**

```jsx
const arr = [1, 2, 3, 4];

// 원본 배열의 인덱스 1부터 2개의 요소를 제거하고 그 자리에 새로운 요소 20, 30을 삽입한다.
const result = arr.splice(1, 2, 20, 30);

// 제거한 요소가 배열로 반환된다.
console.log(result); // [2, 3]
// splice 메서드는 원본 배열을 직접 변경한다.
console.log(arr); // [1, 20, 30, 4]
```

push, pop, shift, unshift 메서드는 모두 원본 배열을 직접 변경하는 메서드이며 원본 배열의 처음이나 마지막에 요소를 추가하거나 제거한다. 원본 배열의 중간에 요소를 추가하거나 중간에 있는 요소를 제거하는 경우 splice 메서드를 사용한다. 3개의 매개변수가 있으며 **원본 배열을 직접 변경한다.**

splice 메서드는 두 번째 인수, 즉 제거할 요소의 개수를 0으로 지정하면 아무런 요소도 제거하지 않고 새로운 요소들을 삽입한다.

또한 세 번째 인수, 즉 제거한 위치에 추가할 요소들의 목록을 전달하지 않으면 원본 배열에서 지정된 요소를 제거하기만 한다.

### 27.8.9 Array.prototype.slice

**예제 27-67**

```jsx
const arr = [1, 2, 3];

// arr[0]부터 arr[1] 이전(arr[1] 미포함)까지 복사하여 반환한다.
arr.slice(0, 1); // -> [1]

// arr[1]부터 arr[2] 이전(arr[2] 미포함)까지 복사하여 반환한다.
arr.slice(1, 2); // -> [2]

// 원본은 변경되지 않는다.
console.log(arr); // [1, 2, 3]
```

인수로 전달된 범위의 요소들을 복사하여 배열로 반환한다. 원본 배열은 변경되지 않는다. 두 개의 매개변수를 갖는다.

첫 번쨰 인수로 전달받은 인덱스부터 두 번째 인수로 전달받은 인덱스 이전까지 요소들을 복사하여 배열로 반환한다.

두 번째 인수를 생략하면 첫 번째 인수로 전달받은 인덱스부터 모든 요소를 복사하여 배열로 반환한다.

**예제 27-69**

```jsx
const arr = [1, 2, 3];

// 배열의 끝에서부터 요소를 한 개 복사하여 반환한다.
arr.slice(-1); // -> [3]

// 배열의 끝에서부터 요소를 두 개 복사하여 반환한다.
arr.slice(-2); // -> [2, 3]
```

첫번째 인수가 음수인 경우 배열의 끝에서 요소를 복사하여 배열로 반환한다.

**예제 27-70**

```jsx
const arr = [1, 2, 3];

// 인수를 모두 생략하면 원본 배열의 복사본을 생성하여 반환한다.
const copy = arr.slice();
console.log(copy); // [1, 2, 3]
console.log(copy === arr); // false
```

인수를 모두 생략하면 원본 배열의 복사본을 생성하여 반환한다. 이때 생성된 복사본은 얕은 복사를 통해 생성된다.

### 27.8.10 Array.prototype.join

**예제 27-75**

```jsx
const arr = [1, 2, 3, 4];

// 기본 구분자는 ','이다.
// 원본 배열 arr의 모든 요소를 문자열로 변환한 후, 기본 구분자 ','로 연결한 문자열을 반환한다.
arr.join(); // -> '1,2,3,4';

// 원본 배열 arr의 모든 요소를 문자열로 변환한 후, 빈문자열로 연결한 문자열을 반환한다.
arr.join(""); // -> '1234'

// 원본 배열 arr의 모든 요소를 문자열로 변환한 후, 구분자 ':'로 연결한 문자열을 반환한다.ㄴ
arr.join(":"); // -> '1:2:3:4'
```

원본 배열의 모든 요소를 문자열로 변환한 후, 인수로 전달받은 문자열, 즉 구분자로 연결한 문자열을 반환한다. 구분자는 생략 가능하며 기본 구분자는 콤마(,)다.

### 27.8.11 Array.prototype.reverse

**예제 27-76**

```jsx
const arr = [1, 2, 3];
const result = arr.reverse();

// reverse 메서드는 원본 배열을 직접 변경한다.
console.log(arr); // [3, 2, 1]
// 반환값은 변경된 배열이다.
console.log(result); // [3, 2, 1]
```

원본 배열의 순서를 반대로 뒤집는다. 원본 배열이 변경되며, 반환값은 변경된 배열이다.

### 27.8.12 Array.prototype.fill

**예제 27-77**

```jsx
const arr = [1, 2, 3];

// 인수로 전달 받은 값 0을 배열의 처음부터 끝까지 요소로 채운다.
arr.fill(0);

// fill 메서드는 원본 배열을 직접 변경한다.
console.log(arr); // [0, 0, 0]
```

ES6에서 도입된 fill 메서드는 인수로 전달받은 값을 배열의 처음부터 끝까지 요소로 채운다. 이때 원본 배열이 변경된다.

두번째 인수로 요소 채우기를 시작할 인덱스를 전달할 수 있고 세 번째 인수로 요소 채우기를 멈출 인덱스를 전달할 수 있다.

### 27.8.13 Array.prototype.includes

**예제 27-82**

```jsx
const arr = [1, 2, 3];

// 배열에 요소 2가 포함되어 있는지 확인한다.
arr.includes(2); // -> true

// 배열에 요소 100이 포함되어 있는지 확인한다.
arr.includes(100); // -> false
```

ES7에서 도입된 includes 메서드는 배열 내에 특정 요소가 포함되어 있는지 확인하여 true 또는 false를 반환한다. 첫 번째 인수로 검색할 대상을 지정한다.

### 27.8.14 Array.prototype.flat

ES10에서 도입된 flat 메서드는 인수로 전달한 깊이만큼 재귀적으로 배열을 평탄화 한다.

**예제 27-85**

```jsx
[1, [2, 3, 4, 5]].flat(); // -> [1, 2, 3, 4, 5]
```

중첩 배열을 평탄화할 깊이를 인수로 전달할 수 있다. 인수를 생략할 경우 기본값은 1이다. 인수로 infinity를 전달하면 중첩 배열을 모두 평탄화한다.

---

## 27.9 배열 고차 함수

고차 함수는 함수를 인수로 전달받거나 함수를 반환하는 함수를 말한다. 자바스크립트의 함수는 일급 객체이므로 함수를 값처럼 인수로 전달할 수 있으며 반환할 수도 있다. 배열 고차 함수는 활용도가 매우 높으므로 사용법을 잘 이해해야 한다.

### 27.9.1 Array.prototype.sort

**예제 27-87**

```jsx
const fruits = ["Banana", "Orange", "Apple"];

// 오름차순(ascending) 정렬
fruits.sort();

// sort 메서드는 원본 배열을 직접 변경한다.
console.log(fruits); // ['Apple', 'Banana', 'Orange']
```

배열의 요소를 정렬한다. **원본 배열을 직접 변경하며** 정렬된 배열을 반환한다. 기본적으로 오름차순으로 요소를 정렬한다.

내림차순으로 정렬하려면 sort 메서드 사용 후 reverse 메서드를 사용하여 순서를 뒤집으면 된다.

**예제 27-90**

```jsx
const points = [40, 100, 1, 5, 2, 25, 10];

points.sort();

// 숫자 요소들로 이루어진 배열은 의도한 대로 정렬되지 않는다.
console.log(points); // [1, 10, 100, 2, 25, 40, 5]
```

sort 메서드의 기본 정렬 순서는 유니코드 코드 포인트의 순서를 따른다. 숫자 요소로 이루어진 배열을 정렬할 떄는 예제 89코드와 같은 문젝 발생하게 된다. 따라서 숫자 요소를 정렬할 때는 정렬 순서를 정의하는 비교 함수를 인수로 전달해야 한다.

**예제 27-93**

```jsx
const points = [40, 100, 1, 5, 2, 25, 10];

// 숫자 배열의 오름차순 정렬. 비교 함수의 반환값이 0보다 작으면 a를 우선하여 정렬한다.
points.sort((a, b) => a - b);
console.log(points); // [1, 2, 5, 10, 25, 40, 100]

// 숫자 배열에서 최소/최대값 취득
console.log(points[0], points[points.length]); // 1

// 숫자 배열의 내림차순 정렬. 비교 함수의 반환값이 0보다 작으면 b를 우선하여 정렬한다.
points.sort((a, b) => b - a);
console.log(points); // [100, 40, 25, 10, 5, 2, 1]

// 숫자 배열에서 최대값 취득
console.log(points[0]); // 100
```

### 27.9.2 Array.prototype.forEach

for 문을 대체할 수 있는 고차 함수이다. 자신의 내부에서 반복문을 실행한다.

**예제 27-95**

```jsx
const numbers = [1, 2, 3];
let pows = [];

// for 문으로 배열 순회
for (let i = 0; i < numbers.length; i++) {
    pows.push(numbers[i] ** 2);
}
console.log(pows); // [1, 4, 9]
```

**예제 27-96**

```jsx
const numbers = [1, 2, 3];
let pows = [];

// forEach 메서드는 numbers 배열의 모든 요소를 순회하면서 콜백 함수를 반복 호출한다.
numbers.forEach((item) => pows.push(item ** 2));
console.log(pows); // [1, 4, 9]
```

forEach 메서드는 for 문에 비해 성능이 좋지는 않지만 가독성이 더 좋다. 요소가 대단히 많은 배열을 순회하거나 시간이 많이 걸리는 복잡한 코드 또는 높은 성능이 필요한 경우가 아니라면 for 문 대신 forEach 메서드를 사용할 것을 권장한다.

### 27.9.3 Array.prototype.map

**예제 27-106**

```jsx
const numbers = [1, 4, 9];

// map 메서드는 numbers 배열의 모든 요소를 순회하면서 콜백 함수를 반복 호출한다.
// 그리고 콜백 함수의 반환값들로 구성된 새로운 배열을 반환한다.
const roots = numbers.map((item) => Math.sqrt(item));

// 위 코드는 다음과 같다.
// const roots = numbers.map(Math.sqrt);

// map 메서드는 새로운 배열을 반환한다
console.log(roots); // [ 1, 2, 3 ]
// map 메서드는 원본 배열을 변경하지 않는다
console.log(numbers); // [ 1, 4, 9 ]
```

자신을 호출한 배열의 모든 요소를 순회하면서 인수로 전달받은 콜백 함수를 반복 호출한다. 그리고 콜백 함수의 반환값들로 구성된 새로운 배열을 반환한다. 원본 배열은 변경되지 않는다.

forEach 메서드와 map 메서드의 공통점은 자신을 호출한 배열의 모든 요소를 순회하면서 인수로 전달받은 콜백 함수를 반복 호출한다는 것이다. 하지만 forEach 메서드는 언제나 undefined를 반환하고, map 메서드는 콜백 함수의 반환값들로 구성된 새로운 배열을 반환하는 차이가 있다.

즉, forEach 메서드는 단순히 반복문을 대체하기 위한 고차 함수이고, map 메서드는 요소값을 다른 값으로 매핑한 새로운 배열을 생성하기 위한 고차 함수다.

### 27.9.4 Array.prototype.filter

**예제 27-111**

```jsx
// filter 메서드는 콜백 함수를 호출하면서 3개(요소값, 인덱스, this)의 인수를 전달한다.
[1, 2, 3].filter((item, index, arr) => {
    console.log(
        `요소값: ${item}, 인덱스: ${index}, this: ${JSON.stringify(arr)}`
    );
    return item % 2;
});
/*
요소값: 1, 인덱스: 0, this: [1,2,3]
요소값: 2, 인덱스: 1, this: [1,2,3]
요소값: 3, 인덱스: 2, this: [1,2,3]
*/
```

자신을 호출한 배열의 모든 요소를 순회하면서 인수로 전달받은 콜백 함수를 반복 호출한다. 그리고 콜백 함수의 반환값이 true인 요소로만 구성된 새로운 배열을 반환한다. filter 메서드는 콜백 함수의 반환값이 true인 요소만 추출한 새로운 배열을 반환한다.

### 27.9.5 Array.prototype.reduce

**예제 27-113**

```jsx
// [1, 2, 3, 4]의 모든 요소의 누적을 구한다.
const sum = [1, 2, 3, 4].reduce(
    (accumulator, currentValue, index, array) => accumulator + currentValue,
    0
);

console.log(sum); // 10
```

자신을 호출한 배열을 모든 요소로 순회하며 인수로 전달받은 콜백 함수를 반복 호출한다. 그리고 콜백 함수의 반환값을 다음 순회 시에 콜백 함수의 첫 번째 인수로 전달하면서 콜백 함수를 호출하여 하나의 결과값을 만들어 반환한다. 원본 배열은 변경되지 않는다.

![](https://velog.velcdn.com/images/dooin/post/7b1ab2ce-6f3a-4216-87f3-772612a0ef2f/image.png)

### 27.9.6 Array.prototype.some

**예제 27-128**

```jsx
// 배열의 요소 중에 10보다 큰 요소가 1개 이상 존재하는지 확인
[5, 10, 15].some((item) => item > 10); // -> true

// 배열의 요소 중에 0보다 작은 요소가 1개 이상 존재하는지 확인
[5, 10, 15].some((item) => item < 0); // -> false

// 배열의 요소 중에 'banana'가 1개 이상 존재하는지 확인
["apple", "banana", "mango"].some((item) => item === "banana"); // -> true

// some 메서드를 호출한 배열이 빈 배열인 경우 언제나 false를 반환한다.
[].some((item) => item > 3); // -> false
```

자신을 호출한 배열의 요소를 순회하면서 인수로 전달된 콜백 함수를 호출한다. 이때 some 메서드는 콜백 함수의 반환값이 단 한번이라도 참이면 true, 모두 거짓이면 false를 반환한다.

### 27.9.7 Array.prototype.every

**예제 27-129**

```jsx
// 배열의 모든 요소가 3보다 큰지 확인
[5, 10, 15].every((item) => item > 3); // -> true

// 배열의 모든 요소가 10보다 큰지 확인
[5, 10, 15].every((item) => item > 10); // -> false

// every 메서드를 호출한 배열이 빈 배열인 경우 언제나 true를 반환한다.
[].every((item) => item > 3); // -> true
```

자신을 호출한 배열의 요소를 순회하면서 인수로 전달된 콜백 함수를 호출한다. 이때 every 메서드는 콜백 함수의 반환값이 모두 참이면 true, 단 한번이라도 거짓이면 false를 반환한다. 모든 요소가 정의한 조건을 만족하는지 확인한다.

### 27.9.8 Array.prototype.find

**예제 27-130**

```jsx
const users = [
    { id: 1, name: "Lee" },
    { id: 2, name: "Kim" },
    { id: 2, name: "Choi" },
    { id: 3, name: "Park" },
];

// id가 2인 첫 번째 요소를 반환한다. find 메서드는 배열이 아니라 요소를 반환한다.
users.find((user) => user.id === 2); // -> {id: 2, name: 'Kim'}
```

ES6에서 도입된 find 메서드는 자신을 호출한 배열의 요소를 순회하면서 인수로 전달된 콜백 함수를 호출하여 반환값이 true인 첫 번째 요소를 반환한다. true인 요소가 존재하지 않는다면 undefined를 반환한다.

### 27.9.9 Array.prototype.findIndex

**예제 27-132**

```jsx
const users = [
    { id: 1, name: "Lee" },
    { id: 2, name: "Kim" },
    { id: 2, name: "Choi" },
    { id: 3, name: "Park" },
];

// id가 2인 요소의 인덱스를 구한다.
users.findIndex((user) => user.id === 2); // -> 1

// name이 'Park'인 요소의 인덱스를 구한다.
users.findIndex((user) => user.name === "Park"); // -> 3

// 위와 같이 프로퍼티 키와 프로퍼티 값으로 요소의 인덱스를 구하는 경우
// 다음과 같이 콜백 함수를 추상화할 수 있다.
function predicate(key, value) {
    // key와 value를 기억하는 클로저를 반환
    return (item) => item[key] === value;
}

// id가 2인 요소의 인덱스를 구한다.
users.findIndex(predicate("id", 2)); // -> 1

// name이 'Park'인 요소의 인덱스를 구한다.
users.findIndex(predicate("name", "Park")); // -> 3
```

ES6에서 도입된 find 메서드는 자신을 호출한 배열의 요소를 순회하면서 인수로 전달된 콜백 함수를 호출하여 반환값이 true인 첫 번째 요소의 인덱스를 반환한다. true인 요소가 존재하지 않는다면 -1을 반환한다.

### 27.9.10 Array.prototype.flatMap

ES10에서 도입된 flatMap 메서드는 map 메서드를 통해 생성된 새로운 배열을 평탄화한다. 즉, map 메서드와 flast 메서드를 순차적으로 실행하는 효과가 있다. 단 flat 메서드처럼 인수를 전달하여 평탄화 깊이를 지정할 수는 없고 1단계만 평탄화한다.
