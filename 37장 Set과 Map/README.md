> # 📖33.1 Set

- Set 객체는 중복되지 않는 유일한 값들의 집합이다.
- 배열과 유사하지만 다음과 같은 차이점이 존재한다.

  | 구분 | 배열 | Set 객체 |
  | ----------------------- | ---- | -------- |
  | 동일한 값의 중복을 허용 | O | X |
  | 요소 순서에 의미 | O | X |
  | 인덱스로 요소에 접근 | O | X |

<br>
<br>
<br>
<br>
<br>

## 📌37.1.1 Set 객체의 생성

- Set 객체는 Set 생성자 함수로 생성한다.
- Set 생성자 함수는 이터러블을 인수로 전달받아 Set 객체를 생성한다.
  - 이터러블의 중복된 값은 Set 객체에 요소로 저장되지 않는다.

```js
// Set 기본
const set1 = new Set([1, 2, 2, 3]);
console.log(set1); // Set(3) { 1, 2, 3 }

const set2 = new Set("Javascript");
console.log(set2); // Set(9) { 'J', 'a', 'v', 's', 'c', 'r', 'i', 'p', 't' }

// 중복된 요소 제거 -> 배열
const uniq = (arr) => arr.filter((v, i, self) => self.indexOf(v) === i);
console.log(uniq([2, 1, 2, 3, 4, 3, 4])); // [ 2, 1, 3, 4 ]

// 중복된 요소 제거 -> Set 객체
const uniq = (arr) => [...new Set(arr)];
console.log(uniq([2, 1, 2, 3, 4, 3, 4])); // [ 2, 1, 3, 4 ]
```

## 📌37.1.2 요소 개수 확인

- Set 객체의 요소 개수를 확인할 떄는 Set.prototype.size 프로퍼티를 사용한다.
- setter 함수 없이 getter 함수만 존재하는 접근자 프로퍼티다.

```js
const set1 = new Set([1, 2, 2, 3]);
console.log(set1.size); // 3

// size 접근자 프로퍼티는 setter 함수가 없어서 무시
set1.size = 10;
console.log(set1.size); // 3
```

## 📌37.1.3 요소 추가

- Set 객체에 요소를 추가할 때는 Set.prototype.add 메서드를 사용한다.
- add 메서드를 연속적으로 호출할 수 있으며 중복된 요소의 추가는 허용되지 않는다.

```js
const set1 = new Set([1, 2, 2, 3]);
console.log(set1); // Set(3) { 1, 2, 3 }

set1.add(4);
console.log(set1); // Set(4) { 1, 2, 3, 4 }

set1.add(4).add(5).add(5).add(6); // add 메서드를 연속적으로 호출할 수 있으며 중복된 요소의 추가는 허용되지 않는다.
console.log(set1); // Set(4) { 1, 2, 3, 4, 5, 6 }
```

- Set 객체는 객체나 배열 같이 자바스크립트의 모든 값을 요소로 저장할 수 있다.

```js
const set = new Set();

// add 메서드 체이닝
set
  .add(1)
  .add("a")
  .add(true)
  .add(undefined)
  .add(null)
  .add({})
  .add([])
  .add(() => {});

console.log(set);
// Set(8) {
//   1,
//   'a',
//   true,
//   undefined,
//   null,
//   {},
//   [],
//   [Function (anonymous)]
// }
```

## 📌37.1.4 요소 존재 여부 확인

- Set 객체에 특정 요소가 존재하는지 확인하려면 Set.prototype.has 메서드를 사용한다.

```js
const set = new Set([1, 2, 3]);

console.log(set.has(3)); // true
console.log(set.has(4)); // false
```

## 📌37.1.5 요소 삭제

- Set 객체의 특정 요소를 삭제하려면 Set.prototype.delete 메서드를 사용한다.
- 삭제하려는 요소값을 인수로 전달해야 한다.

```js
const set = new Set([1, 2, 3]);

let result = set.delete(3);
console.log(set, result); // Set(2) { 1, 2 } true

result = set.delete(4);
console.log(set, result); // Set(2) { 1, 2 } false
```

## 📌37.1.6 요소 일괄 삭제

- Set 객체의 모든 요소를 일괄 삭제하려면 Set.prototype.clear 메서드를 사용한다.

```js
const set = new Set([1, 2, 3]);

let result = set.clear();
console.log(set, result); // Set(0) {} undefined
```

## 📌37.1.7 요소 순회

- Set 객체의 요소를 순회하려면 Set.prototype.forEach 메서드를 사용한다.
- Array.prototype.forEach 메서드와 유사하지만, 전달받는 인수가 다르다.
  - 첫 번째 인수 → 현재 순회 중인 요소값
  - 두 번째 인수 → 현재 순회 중인 요소값
  - 세 번째 인수 → 현재 순회 중인 Set 객체 자신
  - 첫 번째, 두 번째 인수가 같은 것은 단순히 Array.prototoype.forEach 메서드와 인터페이스를 통일하기 위함이다.

```js
const set = new Set([1, 2, 3]);

set.forEach((v1, v2, self) => console.log(v1, v2, self));
// 1 1 Set(3) { 1, 2, 3 }
// 2 2 Set(3) { 1, 2, 3 }
// 3 3 Set(3) { 1, 2, 3 }
```

- Set 객체는 이터러블이다.
  - for...of 문으로 순회할 수 있다.
  - 스프레드 문법과 배열 디스트럭처링의 대상이 될 수도 있다.

```js
const set = new Set([1, 2, 3]);

// Set 객체가 이터러블 -> Symbol.iterator 프로퍼티가 존재하는지 확인
console.log(Symbol.iterator in set); // true

// Set 객체가 이터러블 -> for - of문 가능
for (const value of set) {
  console.log(value); // 1 2 3
}

// Set 객체가 이터러블 -> 스프레드 문법 가능
console.log([...set]); // [ 1, 2, 3 ]

// Set 객체가 이터러블 -> 배열 디스트럭처링 가능
const [a, ...rest] = set;
console.log(a, rest); // 1 [ 2, 3 ]
```

- Set 객체는 요소의 순서가 무의미하지만, Set 객체를 순회하는 순서는 요소가 추가된 순서를 따른다.
- ECMAScript 표준 사양에 규정되어 있지는 않지만, 다른 이터러블의 순회와 호환성을 유지하기 위함이다.

## 📌37.1.8 집합 연산

### 교집합

```js
// Set 객체 프로토타입에 "교집합" 정의
Set.prototype.intersection = function (set) {
  const result = new Set();

  for (const val of set) {
    if (this.has(val)) result.add(val);
  }

  return result;
};

const setA = new Set([1, 2, 3, 4]);
const setB = new Set([2, 4]);

// setA 와 setB의 교집합
console.log(setA.intersection(setB)); // Set(2) { 2, 4 }
// setB 와 setA의 교집합
console.log(setB.intersection(setA)); // Set(2) { 2, 4 }
```

- ES6 고차 함수를 활용한 교집합 구현

```js
Set.prototype.intersection = function (set) {
  return new Set([...this].filter((v) => set.has(v)));
};

const setA = new Set([1, 2, 3, 4]);
const setB = new Set([2, 4]);

console.log(setA.intersection(setB)); // Set(2) { 2, 4 }
console.log(setB.intersection(setA)); // Set(2) { 2, 4 }
```

### 합집합

```js
// Set 객체 프로토타입에 "합집합" 정의
Set.prototype.union = function (set) {
  const result = new Set(this);

  for (const val of set) {
    result.add(val);
  }

  return result;
};

const setA = new Set([1, 2, 3, 4]);
const setB = new Set([2, 4]);

// setA 와 setB의 합집합
console.log(setA.union(setB)); // Set(4) { 1, 2, 3, 4 }
// setB 와 setA의 합집합
console.log(setB.union(setA)); // Set(4) { 2, 4, 1, 3 }
```

```js
Set.prototype.union = function (set) {
  return new Set([...this, ...set]);
};

const setA = new Set([1, 2, 3, 4]);
const setB = new Set([2, 4]);

console.log(setA.union(setB)); // Set(4) { 1, 2, 3, 4 }
console.log(setB.union(setA)); // Set(4) { 2, 4, 1, 3 }
```

### 차집합

```js
// Set 객체 프로토타입에 "차집합" 정의
Set.prototype.difference = function (set) {
  const result = new Set(this);

  for (const val of set) {
    result.delete(val);
  }

  return result;
};

const setA = new Set([1, 2, 3, 4]);
const setB = new Set([2, 4]);

// setA - setB
console.log(setA.difference(setB)); // Set(2) { 1, 3 }
// setB - setA
console.log(setB.difference(setA)); // Set(0) {}
```

- ES6 고차 함수를 활용한 차집합 구현

```js
Set.prototype.difference = function (set) {
  return new Set([...this].filter((v) => !set.has(v)));
};

const setA = new Set([1, 2, 3, 4]);
const setB = new Set([2, 4]);

console.log(setA.difference(setB)); // Set(2) { 1, 3 }
console.log(setB.difference(setA)); // Set(0) {}
```

### 부분 집합과 상위 집합

- 집합 A가 집합 B에 포함되는 경우 집합 A는 집합 B의 부분 집합(subset)이며, 집합 B는 집합 A의 상위 집합(superset)이다.

```js
// Set 객체 프로토타입에 "서브셋에 대한 상위 집합" 정의
Set.prototype.isSuperset = function (subset) {
  for (const val of subset) {
    if (!this.has(val)) return false;
  }

  return true;
};

const setA = new Set([1, 2, 3, 4]);
const setB = new Set([2, 4]);

// setB가 setA의 부분집합인지 판별 (= setA가 setB의 상위 집합인지 판별)
console.log(setA.isSuperset(setB)); // true
// setA가 setB의 부분집합인지 판별 (= setB가 setA의 상위 집합인지 판별)
console.log(setB.isSuperset(setA)); // false
```

- ES6 고차 함수를 활용한 부분 집합과 상위 집합 구현

```js
Set.prototype.isSuperset = function (subset) {
  const superSetArr = [...this];
  return [...subset].every((v) => superSetArr.includes(v));
};

const setA = new Set([1, 2, 3, 4]);
const setB = new Set([2, 4]);

console.log(setA.isSuperset(setB)); // true
console.log(setB.isSuperset(setA)); // false
```

> # 📖33.2 Map

- Map 객체는 키와 값의 쌍으로 이루어진 컬렉션이다.
- 객체와 유사하지만 다음과 같은 차이점이 존재한다.
  | 구분 | 객체 | Map 객체 |
  | ---------------------- | ----------------------- | --------------------- |
  | 키로 사용할 수 있는 값 | 문자열 or 심벌 값 | 객체를 포함한 모든 값 |
  | 이터러블 | X | O |
  | 요소 개수 확인 | Object.keys(obj).length | Map.prototype.size |

## 📌37.2.1 Map 객체의 생성

- Map 객체는 Map 생성자 함수로 생성한다.
- Map 생성자 함수는 이터러블을 인수로 전달받아 Map 객체를 생성한다.
  - 이때 인수로 전달되는 이터러블은 키와 값의 쌍으로 이루어진 요소로 구성되어야 한다.

```js
const map1 = new Map([
  ["key1", "value1"],
  ["key2", "value2"],
]);
console.log(map1); // Map(2) { 'key1' => 'value1', 'key2' => 'value2' }

const map2 = new Map([1, 2]); // TypeError: Iterator value 1 is not an entry object
```

- Map 생성자 함수의 인수로 전달한 이터러블에 중복된 키를 갖는 요소가 존재한다면 값이 덮어써진다.
  - 따라서 중복된 키를 갖는 요소가 존재할 수 없다.

```js
const map1 = new Map([
  ["key1", "기존 값"],
  ["key1", "덮어쓰인 값"],
]);
console.log(map1); // Map(1) { 'key1' => '덮어쓰인 값' }
```

## 📌37.2.2 요소 개수 확인

- Map 객체의 요소 개수를 확인할 때는 Map.prototype.size 프로퍼티를 사용한다.
- settet 함수 없이 getter 함수만 존재하는 접근자 프로퍼티다.

```js
const map = new Map([['key1', 'value1'], ['key2', 'value2']]);

console.log(Object.getOwnPropertyDescriptor(Map.prototype, 'size'));
// {set: undefined, enumerable: false, configurable: true, get: ƒ}

map.size = 10; // 무시된다.
console.log(map.size); // 2
```

## 📌37.2.3 요소 추가

- Map 객체에 요소를 추가할 때는 Map.prototype.set 메서드를 사용한다.
- set 메서드를 연속적으로 호출할 수 있다.

```js
const map = new Map();
console.log(map); // Map(0) {}

map.set("key1", "value1");
console.log(map); // Map(1) { 'key1' => 'value1' }



const map = new Map();
console.log(map); // Map(0) {}

map.set("key1", "value1").set("key2", "value2");
console.log(map); // Map(2) { 'key1' => 'value1', 'key2' => 'value2' }
```

- Map 객체에는 중복된 키를 갖는 요소가 존재할 수 없기 때문에 중복된 키를 갖는 요소를 추가하면 값이 덮어써진다. 이때 에러가 발생하지는 않는다.

```js
const map = new Map();
console.log(map); // Map(0) {}

map.set("key1", "기존 값");
map.set("key1", "덮어쓰인 값");
console.log(map); // Map(1) { 'key1' => '덮어쓰인 값' }
```

- 객체는 문자열과 심벌 값만 키로 허용하지만, Map 객체는 자바스크립트의 모든 값을 키로 허용한다.

```js
const map = new Map();

const lee = { name: 'Lee' };
const kim = { name: 'Kim' };

// 객체도 키로 사용할 수 있다.
map
  .set(lee, 'developer')
  .set(kim, 'designer');

console.log(map);
// Map(2) { {name: "Lee"} => "developer", {name: "Kim"} => "designer" }
```

## 📌37.2.4 요소 취득

- Map 객체에 특정 요소를 취득하려면 Map.prototype.get 메서드를 사용한다.

```js
const map = new Map();

const obj1 = { name: "L" };
const obj2 = { name: "GH" };

map.set(obj1, "developer").set(obj2, "Front Dev");

console.log(map.get(obj1)); // developer
console.log(map.get(obj2)); // Front Dev
console.log(map.get("key")); // undefined
```

## 📌37.2.5 요소 존재 여부 확인

- Map 객체에 특정 요소가 존재하는지 확인하려면 Map.prototype.has 메서드를 사용한다.

```js
const obj1 = { name: "L" };
const obj2 = { name: "GH" };

const map = new Map([
  [obj1, "developer"],
  [obj2, "Front Dev"],
]);

console.log(map.has(obj1)); // true
console.log(map.has("key")); // false
```

## 📌37.2.6 요소 삭제

- Map 객체에 특정 요소를 삭제하려면 Map.prototype.delete 메서드를 사용한다.
- 인수로 키를 전달한다.

```js
const obj1 = { name: "L" };
const obj2 = { name: "GH" };

const map = new Map([
  [obj1, "developer"],
  [obj2, "Front Dev"],
]);

let result = map.delete(obj1);
console.log(map, result); // Map(1) { { name: 'YM' } => 'Front Dev' } true

result = map.delete("key");
console.log(map, result); // Map(1) { { name: 'YM' } => 'Front Dev' } false
```

## 📌37.2.7 요소 일괄 삭제

- Map 객체의 요소를 일괄 삭제하려면 Map.prototype.clear 메서드를 사용한다.

```js
const obj1 = { name: "L" };
const obj2 = { name: "GH" };

const map = new Map([
  [obj1, "developer"],
  [obj2, "Front Dev"],
]);

let result = map.clear();
console.log(map, result); // Map(0) {} undefined
```

## 📌37.1.7 요소 순회

- Map 객체의 요소를 순회하려면 Map.prototype.forEach 메서드를 사용한다.
- Array.prototype.forEach 메서드와 유사하지만, 전달받는 인수가 다르다.
  - 첫 번째 인수 → 현재 순회 중인 요소값
  - 두 번째 인수 → 현재 순회 중인 요소키
  - 세 번째 인수 → 현재 순회 중인 Map 객체 자신

```js
const obj1 = { name: "L" };
const obj2 = { name: "GH" };

const map = new Map([
  [obj1, "developer"],
  [obj2, "Front Dev"],
]);

map.forEach((v, k, self) => console.log(v, k, self));
// developer { name: 'L' } Map(2) {
//   { name: 'L' } => 'developer',
//   { name: 'GH' } => 'Front Dev' 
// }
// Front Dev { name: 'GH' } Map(2) { 
//    { name: 'L' } => 'developer',
//    { name: 'GH' } => 'Front Dev'
// }
```

- Map 객체는 이터러블이다.
  - for...of 문으로 순회할 수 있다.
  - 스프레드 문법과 배열 디스트럭처링의 대상이 될 수도 있다.

```js
const obj1 = { name: "L" };
const obj2 = { name: "GH" };

const map = new Map([
  [obj1, "developer"],
  [obj2, "Front Dev"],
]);

// Map 객체 -> 이터러블 -> Symbol.iterator 프로퍼티 존재 판별
console.log(Symbol.iterator in map); // true

// Map 객체 -> 이터러블 -> for - of문 가능
for (const entry of map) {
  console.log(entry);
}
// [{ name: "L" }, "developer"]
// [({ name: "GH" }, "Front Dev")]

// Map 객체 -> 이터러블 -> 스프레드 문법 가능
console.log([...map]); // [ [ { name: 'L' }, 'developer' ], [ { name: 'GH' }, 'Front Dev' ] ]

// Map 객체 -> 이터러블 -> 배열 디스트럭처링 할당 가능
const [a, b] = map;
console.log(a, b); // [ { name: 'L' }, 'developer' ] [ { name: 'GH' }, 'Front Dev' ]
```

- Map 객체는 이터러블이면서 동시에 이터레이터인 객체를 반환하는 메서드를 제공한다.
    | Map 메서드 | 설명 |
    | --------------------- | ----------------------------------------------------------------------------------------- |
    | Map.prototype.keys | Map 객체에서 요소키를 값으로 갖는 이터러블이면서 동시에 이터레이터인 객체를 반환 |
    | Map.prototype.values | Map 객체에서 요소값을 값으로 갖는 이터러블이면서 동시에 이터레이터인 객체를 반환 |
    | Map.prototype.entries | Map 객체에서 요소키와 요소값을 값으로 갖는 이터러블이면서 동시에 이터레이터인 객체를 반환 |

```js
const obj1 = { name: "L" };
const obj2 = { name: "GH" };

const map = new Map([
  [obj1, "developer"],
  [obj2, "Front Dev"],
]);

for (const key of map.keys()) {
  console.log(key);
}
// { name: 'L' }
// { name: 'GH' }

for (const value of map.values()) {
  console.log(value);
}
// developer
// Front Dev

for (const entry of map.entries()) {
  console.log(entry);
}
// [ { name: 'L' }, 'developer' ]
// [ { name: 'GH' }, 'Front Dev' ]
```