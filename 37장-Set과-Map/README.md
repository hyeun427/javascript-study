## 37.1 Set

- Set 객체는 ```중복되지 않는 유일한 값들의 집합```이다.
- 배열과 유사하지만 ```1)동일한 값을 중복하여 포함할 수 없고, 2) 요소 순서에 의미가 없고, 3)인덱스로 요소에 접근할 수 없다는 점```에서 차이가 있다.
- 이러한 특성을 가지고 있어서 set은 수학적 집합을 구현하기 위한 자료구조이다.
	-> 따라서, Set을 통해 교집합, 합집합, 차집합, 여집합 등을 구현할 수 있다.

### 37.1.1 Set 객체의 생성

- Set 객체는 Set 생성자 함수로 생성한다.
- Set 생성자 함수에 인수를 전달하지 않으면 빈 set 객체가 생성된다.
- Set 생성자 함수는 이터러블을 인수로 전달받아 Set 객체를 생성한다.
- 이때 이터러블의 중복된 값은 Set 객체에 요소로 저장되지 않는다.
	-> 중복을 허용하지 않는 Set 객체의 특성을 활용해서 배열에서 중복된 요소를 제거할 수 있다
    
```js
// 배열의 중복 요소 제거
const uniq = array => array.filter((v, i, self) => self.indexOf(v) === i);
console.log(uniq([2, 1, 2, 3, 4, 3, 4])); // [2, 1, 3, 4]

// Set을 사용한 배열의 중복 요소 제거
const uniq = array => [...new Set(array)];
console.log(uniq([2, 1, 2, 3, 4, 3, 4])); // [2, 1, 3, 4]
```

### 37.1.2 요소 개수 확인

- Set 객체의 요소 개수를 확인할 때는 ```Set.prototype.size```프로퍼티를 사용한다.
- size 프로퍼티는 setter 함수 없이 getter 함수만 존재하는 접근자 프로퍼티다.
	-> 그렇기 때문에 size 프로퍼티에 숫자를 할당하여 Set 객체의 요소 개수를 변경할 수 없다.

```js
// 요소 개수 확인
const { size } = new Set([1, 2, 3, 3]);
console.log(size); // 3

// 요소 개수 변경 불가
const set = new Set([1, 2, 3]);

console.log(Object.getOwnPropertyDescriptor(Set.prototype, 'size'));
// {set: undefined, enumerable: false, configurable: true, get: ƒ}

set.size = 10; // 무시된다.
console.log(set.size); // 3
```

### 37.1.3 요소 추가

- Set.prototype.add 메서드를 사용해서 Set 객체에 요소를 추가한다.
- add 메서드는 새로운 요소가 추가된 Set 객체를 반환하기 때문에 add 메서드를 연속적으로 호출할 수 있다.
- Set 객체에 중복된 요소의 추가는 허용되지 않는다. 만약 중복 추가를 하려고 하면 에러가 발생하지는 않고 무시된다.
- Set 객체는 ```NaN과 NaN```, ```+0과 -0```을 서로 같다고 평가하기때문에 중복 추가를 허용하지 않는다.

```js
const set = new Set();

console.log(NaN === NaN); // false
console.log(0 === -0); // true

// NaN과 NaN을 같다고 평가하여 중복 추가를 허용하지 않는다.
set.add(NaN).add(NaN);
console.log(set); // Set(1) {NaN}

// +0과 -0을 같다고 평가하여 중복 추가를 허용하지 않는다.
set.add(0).add(-0);
console.log(set); // Set(2) {NaN, 0}
```
- Set 객체는 객체나 배열과 같이 자바스크립트의 모든 값을 요소로 저장할 수 있다.
```js
const set = new Set();

set
  .add(1)
  .add('a')
  .add(true)
  .add(undefined)
  .add(null)
  .add({})
  .add([]);

console.log(set); // Set(7) {1, "a", true, undefined, null, {}, []}
```

### 37.1.4 요소 존재 여부 확인

- ```Set.prototype.has 메서드```를 사용하면 Set 객체에 특정 요소가 존재하는지 확인 할 수 있다.
- has 메서드는 요소의 존재 여부를 불리언 값으로 반환한다.

```js
const set = new Set([1, 2, 3]);

console.log(set.has(2)); // true
console.log(set.has(4)); // false
```

### 37.1.5 요소 삭제

- ```Set.prototoype.delete 메서드```를하면 사용하면 특정 요소를 삭제할 수 있다.
- delete 메서드는 삭제 성공 여부를 불리언 값으로 반환한다.
	-> add 메서드와 달리 연속적으로 호출을 할 수 없다.
- 삭제하려는 요소값을 인수로 전달해야 한다. ***Set 객체는 인덱스를 갖지않는다.***
- 만약 존재하지않는 요소를 삭제하려하면 에러 없이 무시된다.

```js
const set = new Set([1, 2, 3]);

// 요소 2를 삭제한다.
set.delete(2);
console.log(set); // Set(2) {1, 3}

// 요소 1을 삭제한다.
set.delete(1);
console.log(set); // Set(1) {3}
```

### 37.1.6 요소 일괄 삭제

- ```Set.prototype.clear 메서드```를 사용하면 모든 요소를 일괄 삭세할 수 있다.
- clear 메서드는 언제나 ```undefined```를 반환한다.

### 37.1.7 요소 순회

- ```Set.prototype.forEach 메서드```를 사용하면 객체의 요소를 순회할 수 있다.
- 콜백 함수와 forEach 메서드의 콜백 함수 내부에서 this로 사용될 객체를 인수로 전달한다.
- 이때 콜백 함수는 ```1)첫 번째 인수: 현재 순회 중인 요소값, 2)두 번째 인수: 현재 순회 중인 요소값, 3)세 번째 인수: 현재 순회 중인 Set 객체 자체```를 인수로 전달받는다.
- Set 객체는 ***이터러블***이기때문에 ```for...of문```으로 순회할 수 있으며, ```스프레드 문법```과 ```배열 디스트럭처링의 대상```이 될 수도 있다.

```js
const set = new Set([1, 2, 3]);

// Set 객체는 Set.prototype의 Symbol.iterator 메서드를 상속받는 이터러블이다.
console.log(Symbol.iterator in set); // true

// 이터러블인 Set 객체는 for...of 문으로 순회할 수 있다.
for (const value of set) {
  console.log(value); // 1 2 3
}

// 이터러블인 Set 객체는 스프레드 문법의 대상이 될 수 있다.
console.log([...set]); // [1, 2, 3]

// 이터러블인 Set 객체는 배열 디스트럭처링 할당의 대상이 될 수 있다.
const [a, ...rest] = [...set];
console.log(a, rest); // 1, [2, 3]
```

### 37.1.8 집합 연산

- Set 객체를 통해 교집합, 합집합, 차집합 등을 구현할 수 있다.

#### 교집합

```js
// 방법1
Set.prototype.intersection = function (set) {
  const result = new Set();

  for (const value of set) {
    // 2개의 set의 요소가 공통되는 요소이면 교집합의 대상이다.
    if (this.has(value)) result.add(value);
  }

  return result;
};

const setA = new Set([1, 2, 3, 4]);
const setB = new Set([2, 4]);

// setA와 setB의 교집합
console.log(setA.intersection(setB)); // Set(2) {2, 4}
// setB와 setA의 교집합
console.log(setB.intersection(setA)); // Set(2) {2, 4}

//방법2
Set.prototype.intersection = function (set) {
  return new Set([...this].filter(v => set.has(v)));
};

const setA = new Set([1, 2, 3, 4]);
const setB = new Set([2, 4]);

// setA와 setB의 교집합
console.log(setA.intersection(setB)); // Set(2) {2, 4}
// setB와 setA의 교집합
console.log(setB.intersection(setA)); // Set(2) {2, 4}
```

#### 합집합

```js
//방법1
Set.prototype.union = function (set) {
  // this(Set 객체)를 복사
  const result = new Set(this);

  for (const value of set) {
    // 합집합은 2개의 Set 객체의 모든 요소로 구성된 집합이다. 중복된 요소는 포함되지 않는다.
    result.add(value);
  }

  return result;
};

const setA = new Set([1, 2, 3, 4]);
const setB = new Set([2, 4]);

// setA와 setB의 합집합
console.log(setA.union(setB)); // Set(4) {1, 2, 3, 4}
// setB와 setA의 합집합
console.log(setB.union(setA)); // Set(4) {2, 4, 1, 3}

//방법2
Set.prototype.union = function (set) {
  return new Set([...this, ...set]);
};

const setA = new Set([1, 2, 3, 4]);
const setB = new Set([2, 4]);

// setA와 setB의 합집합
console.log(setA.union(setB)); // Set(4) {1, 2, 3, 4}
// setB와 setA의 합집합
console.log(setB.union(setA)); // Set(4) {2, 4, 1, 3}
```

#### 차집합

```js
//방법1
Set.prototype.difference = funct\ion (set) {
  // this(Set 객체)를 복사
  const result = new Set(this);

  for (const value of set) {
    // 차집합은 어느 한쪽 집합에는 존재하지만 다른 한쪽 집합에는 존재하지 않는 요소로 구성된 집합이다.
    result.delete(value);
  }

  return result;
};

const setA = new Set([1, 2, 3, 4]);
const setB = new Set([2, 4]);

// setA에 대한 setB의 차집합
console.log(setA.difference(setB)); // Set(2) {1, 3}
// setB에 대한 setA의 차집합
console.log(setB.difference(setA)); // Set(0) {}

//방법2
Set.prototype.difference = function (set) {
  return new Set([...this].filter(v => !set.has(v)));
};

const setA = new Set([1, 2, 3, 4]);
const setB = new Set([2, 4]);

// setA에 대한 setB의 차집합
console.log(setA.difference(setB)); // Set(2) {1, 3}
// setB에 대한 setA의 차집합
console.log(setB.difference(setA)); // Set(0) {}
```

#### 부분 집합과 상위 집합

```js
//방법1
// this가 subset의 상위 집합인지 확인한다.
Set.prototype.isSuperset = function (subset) {
  for (const value of subset) {
    // superset의 모든 요소가 subset의 모든 요소를 포함하는지 확인
    if (!this.has(value)) return false;
  }

  return true;
};

const setA = new Set([1, 2, 3, 4]);
const setB = new Set([2, 4]);

// setA가 setB의 상위 집합인지 확인한다.
console.log(setA.isSuperset(setB)); // true
// setB가 setA의 상위 집합인지 확인한다.
console.log(setB.isSuperset(setA)); // false

//방법2
// this가 subset의 상위 집합인지 확인한다.
Set.prototype.isSuperset = function (subset) {
  const supersetArr = [...this];
  return [...subset].every(v => supersetArr.includes(v));
};

const setA = new Set([1, 2, 3, 4]);
const setB = new Set([2, 4]);

// setA가 setB의 상위 집합인지 확인한다.
console.log(setA.isSuperset(setB)); // true
// setB가 setA의 상위 집합인지 확인한다.
console.log(setB.isSuperset(setA)); // false
```

---

## 37.2 Map

- Map 객체는 키와 값의 쌍으로 이루어진 컬렉션이다.
- 객체와 유사하지만 아래와 같은 차이가 있다.

![](https://velog.velcdn.com/images/hyeun427/post/8493b9e3-abb2-44cc-b8c4-da04e59ca30a/image.png)

### 37.2.1 Map 객체의 생성

- Map 객체는 ```Map 생성자 함수```로 생성한다.
- Map 생성자 함수에 인수를 전달하지 않으면 빈 Map 객체가 생성된다.
```js
const map = new Map();
console.log(map); // Map(0) {}
```
- ***Map 생성자 함수는 이터러블을 인수로 전달받아 Map 객체를 생성한다.*** 이때 인수로 전달되는 이터러블은 ```키와 값의 쌍```으로 이루어진 요소로 구성되어야 한다.
- 만약 인수로 전달한 이터러블에 중복된 키를 갖는 요소가 존재하면 값이 덮어써진다.
	-> 그렇기 때문에 중복된 키를 갖는 요소가 존재할 수 없다.
    
### 37.2.2 요소 개수 확인

- ```Map.prototype.size 프로퍼티```로 Map 객체의 요소 개수를 확인할 수 있다.
- size 프로퍼티는 getter 함수만 존재하는 접근자 프로퍼티이기때문에 요소 개수를 직접 변경할 수 없다.
```js
const { size } = new Map([['key1', 'value1'], ['key2', 'value2']]);
console.log(size); // 2

// 만약 요소 개수를 직접 변경하려하면 무시된다!
const map = new Map([['key1', 'value1'], ['key2', 'value2']]);

console.log(Object.getOwnPropertyDescriptor(Map.prototype, 'size'));
// {set: undefined, enumerable: false, configurable: true, get: ƒ}

map.size = 10; // 무시된다.
console.log(map.size); // 2
```

### 37.2.3 요소 추가

- ```Map.prototype.set 프로퍼티```로 Map 객체의 요소를 추가할 수 있다.
```js
const map = new Map();
console.log(map); // Map(0) {}

map.set('key1', 'value1');
console.log(map); // Map(1) {"key1" => "value1"}
```
- set 메서드는 ```새로운 요소가 추가된 Map 객체를 반환```하기 때문에 연속적으로 호출 할 수 있다.
- Map 객체에는 중복된 키를 갖는 요소가 존재할 수 없기 때문에 중복된 키를 갖는 요소를 추가하면 값이 덮어써진다.
- Map 객체는 ```NaN과 NaN```, ```+0과 -0```을 서로 같다고 평가하기 때문에 중복 추가를 허용하지 않는다.

```js
const map = new Map();

console.log(NaN === NaN); // false
console.log(0 === -0); // true

// NaN과 NaN을 같다고 평가하여 중복 추가를 허용하지 않는다.
map.set(NaN, 'value1').set(NaN, 'value2');
console.log(map); // Map(1) { NaN => 'value2' }

// +0과 -0을 같다고 평가하여 중복 추가를 허용하지 않는다.
map.set(0, 'value1').set(-0, 'value2');
console.log(map); // Map(2) { NaN => 'value2', 0 => 'value2' }
```
- Map 객체는 키 타입에 제한이 없기때문에 객체를 포함한 ```모든 값```을 키로 사용할 수 있다.

### 37.2.4 요소 취득

- ```Map.prototype.get 메서드```를 사용해서 Map 객체의 특정 요소를 취득할 수 있다.
- get 메서드의 인수로 키를 전달하면 Map 객체에서 인수로 전달한 키를 갖는 값을 반환한다.
- 만약 인수로 전달한 키를 갖는 요소가 존재하지 않으면 ```undefined```를 반환한다.
```js
const map = new Map();

const lee = { name: 'Lee' };
const kim = { name: 'Kim' };

map
  .set(lee, 'developer')
  .set(kim, 'designer');

console.log(map.get(lee)); // developer
console.log(map.get('key')); // undefined
```

### 37.2.5 요소 존재 여부 확인

- ```Map.prototype.has 메서드```로 특정 요소가 존재하는지 확인 할 수 있다.
- ```has 메서드```는 특정 요소의 존재 여부를 	불리언 값으로 반환한다.

### 37.2.6 요소 삭제

- ```Map.prototype.delete 메서드```로 객체의 요소를 삭제 할 수 있다.
- ```delete 메서드```는 삭제 성공 여부를 불리언 값으로 반환하기 때문에 연속적으로 호출 할 수 없다.
- 만약 존재하지 않는 키로 요소를 삭제하려하면 에러 없이 무시된다.

### 37.2.7 요소 일괄 삭제

- ```Map.prototype.clear 메서드```로 객체의 요소를 일괄 삭제할 수 있다.
- ```clear 메서드```는 언제나 ```undefined```를 반환한다.

### 37.2.8 요소 순회

- ```Map.prototype.forEach 메서드```를 사용하면 객체의 요소를 순회할 수 있다.
- 콜백 함수와 forEach 메서드의 콜백 함수 내부에서 this로 사용될 객체를 인수로 전달한다.
- 이때 콜백 함수는 ```1)첫 번째 인수: 현재 순회 중인 요소값, 2)두 번째 인수: 현재 순회 중인 요소키, 3)세 번째 인수: 현재 순회 중인 Map 객체 자체```를 인수로 전달받는다.
- Map 객체는 ***이터러블***이기때문에 ```for...of문```으로 순회할 수 있으며, ```스프레드 문법```과 ```배열 디스트럭처링의 대상```이 될 수도 있다.
- Map 객체는 이터러블이면서 이터레이터인 객체를 반환하는 메서드를 제공한다.
![](https://velog.velcdn.com/images/hyeun427/post/8c6fe676-d375-4f62-b88c-4d9bc4183f14/image.png)
```js
const lee = { name: 'Lee' };
const kim = { name: 'Kim' };

const map = new Map([[lee, 'developer'], [kim, 'designer']]);

// Map.prototype.keys는 Map 객체에서 요소키를 값으로 갖는 이터레이터를 반환한다.
for (const key of map.keys()) {
  console.log(key); // {name: "Lee"} {name: "Kim"}
}

// Map.prototype.values는 Map 객체에서 요소값을 값으로 갖는 이터레이터를 반환한다.
for (const value of map.values()) {
  console.log(value); // developer designer
}

// Map.prototype.entries는 Map 객체에서 요소키와 요소값을 값으로 갖는 이터레이터를 반환한다.
for (const entry of map.entries()) {
  console.log(entry); // [{name: "Lee"}, "developer"]  [{name: "Kim"}, "designer"]
}
```
- Map 객체는 요소의 순서에 의미를 갖지 않지만 순회하는 순서는 요소가 추가된 순서를 따른다.
