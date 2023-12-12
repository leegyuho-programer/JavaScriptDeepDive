> # 📖 스프레드 문법

ES6에서 도입된 스프레드 문법(전개 문법) ...은 하나로 뭉쳐 있는 여러 값들의 집합을 펼쳐서 개별적인 값들의 목록으로 만든다.

스프레드 문법을 사용할 수 있는 대상(for ... of 문으로 순회할 수 있는 이터러블)

- Array
- String
- Map
- Set
- DOM 컬렉션(NodeList, HTMLCollection)
- arguments

```js
// ...[1, 2, 3]은 [1, 2, 3]을 개별 요소로 분리한다.
console.log(...[1, 2, 3]); // 1 2 3

// 문자열은 이터러블이다.
console.log(..."Hello"); // H e l l o

// Map과 Set은 이터러블이다.
console.log(
  ...new Map([
    ["a", "1"],
    ["b", "2"],
  ])
); // ['a', '1'], ['b', '2']
console.log(...new Set([1, 2, 3])); // 1 2 3

// 이터러블이 아닌 일반 객체는 스프레드 문법의 대상이 될 수 없다.
console.log(...{ a: 1, b: 2 });
// TypeError: Found non-callable @iterator
```

스프레드의 결과는 값이 아니며, 다음과 같이 수미표로 구분한 값의 목록을 사용하는 문맥에서만 사용할 수 있다.

- 함수 호출문의 인수 목록
- 배열 리터럴의 요소 목록
- 객체 리터럴의 프로퍼티 목록

<br/>

## 함수 호출문의 인수 목록에서 사용하는 경우

요소들의 집합인 배열을 펼쳐서 개별적인 값들의 목록으로 만든 후, 이를 함수의 인수 목록으로 전달해야 하는 경우

```js
const arr = [1, 2, 3];

// 배열 arr의 요소 중에서 최대값을 구하기 위해 Math.max를 사용한다.
const max = Math.max(arr); // -> NaN
```

Math.max 메서드는 매개변수 개수를 확정할 수 없는 가변 인자 함수이다.

숫자가 아닌 배열을 인수로 전달하면 최대값을 구할 수 없어 NaN을 반환한다.

```js
const arr = [1, 2, 3];

// 스프레드 문법을 사용해 배열 arr을 1, 2, 3으로 펼쳐서 Math.max에 전달한다.
// Math.max(...[1, 2, 3])은 Math.max(1, 2, 3)과 같다.
const max = Math.max(...arr); // 3
```

🤔 스프레드 문법은 Rest 파라미터와 형태가 동일하여 혼동될 수 있으니 주의!

Rest 파라미터는 함수에 전달된 인수들의 목록을 배열로 전달받기 위해 매개변수 이름 앞에 ...을 붙이는 것(스프레드 문법과 서로 반대 개념)

```js
// Rest 파라미터는 인수들의 목록을 배열로 전달 받음
function foo(...rest) {
  console.log(rest); // 1, 2, 3 -> [1, 2, 3]
}

// 스프레드 문법은 배열과 같은 이터러블을 펼쳐서 개별적인 값들의 목록을 만듦.
// [1, 2, 3] -> 1, 2, 3
foo(...[1, 2, 3]);
```

<br/>

## 배열 리터럴 내부에서 사용하는 경우

스프레드 문법을 배열 리터럴에서 사용하면 ES5 방식보다 더 간결하고 가독성 좋게 표현 가능하다.

<br/>

✅ concat

ES5에서 2개의 배열을 1개의 배열로 결합하고 싶은 경우, concat 메서드를 써야 한다.

스프레드 문법을 사용하면 별도의 메서드 사용 없이 배열 리터럴만으로 결합 가능하다.

```js
// ES5
var arr = [1, 2].concat([3, 4]);
console.log(arr); // [1, 2, 3, 4]

// ES6
const arr = [...[1, 2], ...[3, 4]];
console.log(arr); // [1, 2, 3, 4]
```

<br/>

✅ splice

ES5에서 어떤 배열의 중간에 다른 배열의 요소들을 추가하거나 제거하려면 splice 메서드를 사용해야 한다.
이때 splice 메서드의 세 번째 인수로 배열을 전달하면 배열 자체가 추가된다.

```js
// ES5
var arr1 = [1, 4];
var arr2 = [2, 3];

// 세 번째 인수 arr2를 해체하여 전달해야 한다. 안그러면 아래처럼 배열 자체가 추가 됨
arr1.splice(1, 0, arr2);
console.log(arr1); // [1, [2, 3], 4]

Array.prototype.splice.apply(arr1, [1, 0].concat(arr2));
console.log(arr1); // [1, 2, 3, 4]

// ES6
const arr1 = [1, 4];
const arr2 = [2, 3];

arr1.splice(1, 0, ...arr2);
console.log(arr1); // [1, 2, 3, 4]
```

<br/>

✅ 배열 복사

ES5에서 배열을 복사하려면 slice 메서드를 사용한다.

스프레드 문법을 사용하면 다음과 같이 더욱 간결하고 가독성 좋게 표현 가능하다.

```js
// ES5
var origin = [1, 2];
var copy = origin.slice();

console.log(copy); // [1, 2]
console.log(copy === origin); // false

// ES6
const origin = [1, 2];
const copy = [...origin];

console.log(copy); // [1, 2]
console.log(copy === origin); // false
```

얕은 복사로 새로운 복사본을 생성하기 때문에 일치 비교시 false

<br/>

✅ 이터러블을 배열로 변환

ES5에서 이터러블을 배열로 변환하려면 Function.prototype.apply 또는 Function.prototype.call 메서드를 사용해 slice 메서드를 호출해야 한다.

스프레드 문법을 사용하면 간편하게 이터러블을 배열로 변환할 수 있다. arguments 객체는 이터러블이면서 유사 배열 객체이다.

```js
// ES5
function sum() {
  // 이터러블이면서 유사 배열 객체인 arguments를 배열로 변환
  var args = Array.prototype.slice.call(arguments);

  return args.reduce(function (pre, cur) {
    return pre + cur;
  }, 0);
}

console.log(sum(1, 2, 3)); // 6

// ES6
function sum() {
  return [...arguments].reduce((pre, cur) => pre + cur, 0);
}

console.log(sum(1, 2, 3)); // 6

// 더 나은 방법: Rest 파라미터 사용
const sum = (...args) => args.reduce((pre, cur) => pre + cur, 0);

console.log(sum(1, 2, 3)); // 6
```

단, 이터러블이 아닌 유사 배열 객체는 스프레드 문법의 대상이 될 수 없다.

이터러블이 아닌 유사 배열 객체를 배열로 변경하려면 ES6에서 도입된 `Array.from 메서드`를 사용하면 된다.

```js
// 이터러블이 아닌 유사 배열 객체
const arrayLike = {
  0: 1,
  1: 2,
  2: 3,
  length: 3,
};

const arr = [...arrayLike]; // TypeError: object is not iterable

Array.from(arrayLike); // [1, 2, 3]
// Array.from은 유사 배열 객체 또는 이터러블을 배열로 변환한다.
```

<br/>

## 객체 리터럴 내부에서 사용하는 경우

스프레드 프로퍼티를 사용하면 객체 리터럴의 프로퍼티 목록에서도 스프레드 문법을 사용할 수 있다.

스프레드 프로퍼티 이전에는 Object.assign 메서드로 여러 개의 객체를 병합하거나 특정 프로퍼티를 변경/추가 했다.

1. 객체 병합 (중복될 경우 뒤에 위치한 프로퍼티가 우선권 지님)

```js
// Object.assign
const merged = Object.assign({}, { x: 1, y: 2 }, { y: 10, z: 3 });
console.log(merged); // { x:1, y:10, z:3 }

// 스프레드 프로퍼티
const merged = { ...{ x: 1, y: 2 }, ...{ y: 10, z: 3 } };
console.log(merged); // { x:1, y:10, z:3 }
```

2. 특정 프로퍼티 변경

```js
// Object.assign
const changed = Object.assign({}, { x:1, y:2 }, { y:100 });
console.log(changed); // { x:1, y:100 }

// 스프레드 프로퍼티
const changed = {...{ x:1, y:2 }, y:100 }};
console.log(changed); // { x:1, y:100 }
```

3. 프로퍼티 추가

```js
// Object.assign
const added = Object.assign({}, { x:1, y:2 }, { z:0 });
console.log(added); // { x:1, y:2, z:0 }

// 스프레드 프로퍼티
const added = {...{ x:1, y:2 }, z:0 }};
console.log(added); // { x:1, y:2, z:0 }
```

스프레드 프로퍼티는 Object.assign 메서드를 대체할 수 있는 간편한 문법이다.

<br/>
