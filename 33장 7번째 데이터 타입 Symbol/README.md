> # 📖33.1 심벌이란?

- 심벌(Symbol)은 ES6에서 추가된 원시타입으로 모든 심벌 타입의 값은 다른 값과 중복되지 않는 유일한 값으로 취급된다.
- 따라서 주로 이름의 충돌 위험이 없는 유일한 프로퍼티키를 만들기 위해 주로 사용한다.

<br>
<br>
<br>
<br>
<br>

> # 📖33.2 심벌 값의 생성

## 📌33.2.1 Symbol 함수

### Symbol 함수를 통한 생성

- 다른 원시값들처럼 리터럴 표기법을 통해 값을 생성할 수 없다.
- 심벌 값은 Symbol 함수를 호출하여 생성해야 한다.
- 다른 값과 절대 중복되지 않는 유일무이한 값이다.
- 생성된 심벌 값은 외부로 노출되지 않는다.

```js
// Symbol 함수를 호출하여 유일무이한 심벌 값을 생성한다.
const mySymbol = Symbol();
console.log(typeof mySymbol); // symbol

// 심벌 값은 외부로 노출되지 않아 확인할 수 없다.
console.log(mySymbol);        // Symbol()
```

- 생성자 함수가 아니기 때문에 new 연산자와 함께 호출하지 않는다.

```js
new Symbol(); // TypeError: Symbol is not a constructor
```

- 선택적으로 문자열을 인수로 전달할 수 있다.
  - 생성된 심벌 값에 대한 설명으로 디버깅 용도로만 사용
  - 심벌 값 생성에 어떠한 영향도 주지 않는다.
  - 설명이 같더라도 생성된 심벌 값은 유일무이하다.

```js
// 심벌 값에 대한 설명이 같더라도 유일무이한 심벌 값을 생성한다.
const mySymbol1 = Symbol('mySymbol');
const mySymbol2 = Symbol('mySymbol');

console.log(mySymbol1 === mySymbol2); // false
```

- 심벌 값도 문자열, 숫자 등과 같이 객체처럼 접근하면 암묵적으로 래퍼 객체를 생성한다.

```js
const mySymbol = Symbol('mySymbol');

// 심벌도 레퍼 객체를 생성한다
console.log(mySymbol.description); // mySymbol
console.log(mySymbol.toString());  // Symbol(mySymbol)
```

- 심벌 값은 암묵적으로 문자열이나 숫자 타입으로 변환되지 않는다.
  - 불리언 타입으로는 암묵적으로 타입 변환된다.

```js
const mySymbol = Symbol();

// 심벌 값은 암묵적으로 문자열이나 숫자 타입으로 변환되지 않는다.
console.log(mySymbol + ''); // TypeError: Cannot convert a Symbol value to a string
console.log(+mySymbol);     // TypeError: Cannot convert a Symbol value to a number
```

```js
const mySymbol = Symbol();

// 불리언 타입으로는 암묵적으로 타입 변환된다.
console.log(!!mySymbol); // true

// if 문 등에서 존재 확인을 위해 사용할 수 있다.
if (mySymbol) console.log('mySymbol is not empty.');
```

## 📌33.2.2 Symbol.for / Symbol.keyFor 메서드

- Symbol.for 메서드는 인수로 전달받은 문자열을 키로 사용하여 키와 심벌 값의 쌍들이 저장되어 있는 전역 심벌 레지스트리(global symbol registry)에서 해당 키와 일치하는 심벌 값을 검색한다.

  - 검색에 성공 → 검색된 심벌 값을 반환
  - 검색에 실패 → 새로운 심벌 값을 생성하여 Symbol.for 메서드의 인수로 전달된 키로 전역 심벌 레지스트리에 저장된 후, 생성된 심벌 값을 반환
  - 애플리케이션 전역에서 중복되지 않는 유일무이한 상수인 심벌 값을 단 하나만 생성하여 전역 심벌 레지스트리를 통해 공유할 수 있다.

```js
const s1 = Symbol.for('mySymbol');
const s2 = Symbol.for('mySymbol');

// 상단에서 symbol 이름의 심벌 값을 symbol1 생성시 이미 전역 심벌 레지스트리에 등록했다.
// symbol2 에서 Symbol.for 호출할 경우, 이미 전역 심벌 레지스트리에는 symbol 이라는 이름을 키로 가지는 심벌 값이 존재한다.
// 따라서, symbol1 과 symbol2 에 할당된 심벌 값은 같은 심벌 값이다.
console.log(s1 === s2); // true
```

- Symbol.keyFor 메서드를 사용하면 전역 심벌 레지스트리에 저장된 심벌 값의 키를 추출할 수 있다.

```js
// Symbol.for 메서드로 생성된 심벌 값은 전역 심벌 레지스트리에 존재하지 않을 경우 생성 후 등록되어 관리된다.
const symbol1 = Symbol.for("symbol1");
console.log(Symbol.keyFor(symbol1)); // symbol1

// Symbol 함수로 생성한 심벌 값은 전역 심벌 레지스트리에 등록되어 관리되지 않는다.
const symbol2 = Symbol("symbol2");
console.log(Symbol.keyFor(symbol2)); // undefined
```

<br>
<br>
<br>
<br>
<br>

> # 📖33.3 심벌과 상수

- 값에는 특별한 의미가 없고 상수 이름 자체에 의미가 있는 경우 다른 변수 값과 중복될 수 있는 단점이 있다.
  - 변경/중복될 가능성이 있는 무의미한 상수 대신 중복될 가능성이 없는 유일무이한 심벌 값을 사용할 수 있다.

```js
// { 키: 값(상수) } 형태로 4방향을 의미하는 Direction 객체
// 1,2,3,4 라는 상수는 특별한 의미도 없고, 이후 다른 변수에 값과 중복될 가능성이 크다.
const Direction = {
  UP: 1,
  DOWN: 2,
  LEFT: 3,
  RIGHT: 4,
};

// { 키 : 값(심벌 값) } 형태로 변경한 Direction 객체
// 키의 값들이 심벌 값으로, 중복될 가능성이 없는 상수 값이 되었다.
const Direction = {
  UP: Symbol("up"),
  DOWN: Symbol("down"),
  LEFT: Symbol("left"),
  RIGHT: Symbol("right"),
};
```

<!-- <details>
<summary>💡 enum</summary>

- 명명된 숫자 상수(named numeric constant)의 집합, "열거형" 이라고 부른다.
- 자바스크립트는 enum을 지원 X ( 타입스크립트에서는 enum을 지원 O )
- 자바스크립트에서 enum을 흉내 내어 사용하려면 객체의 변경을 방지하기 위해 객체를 동결하는 Object.freeze 메서드와 심벌 값을 사용

</details> -->

<br>
<br>
<br>
<br>
<br>

> # 📖33.4 심벌과 프로퍼티 키

- 객체의 프로퍼티 키는 빈 문자열을 포함하는 모든 문자열 또는 심벌 값으로 만들 수 있으며, 동적으로 생성할 수도 있다.
- 심벌 값을 포로퍼티 키로 사용하려면 프로퍼티 키로 사용할 심벌 값에 대괄호를 사용해야 한다.
- 심벌 값은 유일무이한 값으로 심벌 값으로 프로퍼티 키를 만들면 다른 객체와 절대 충돌하지 않는다.
  - 미래에 추가될 어떤 프로퍼티 키와도 충돌할 위험이 없다.

```js
const obj = {
  // 심벌 값으로 프로퍼티 키를 생성
  [Symbol.for('mySymbol')]: 1
};

obj[Symbol.for('mySymbol')]; // -> 1
```

<br>
<br>
<br>
<br>
<br>

> # 📖33.5 심벌과 프로퍼티 은닉

- `for...in문, Object.keys, Object.getOwnPropertyNames` 메서드 등으로는 찾을 수 없다.
- 단, ES6에 도입된 `Object.getOwnPropetySymbols` 메서드를 사용하면 심벌 값을 프로퍼티 키로 사용하여 생성한 프로퍼티를 찾을 수 있다.

```js
const obj = {
  [Symbol.for("mySymbol")]: 1,
};

for (const key in obj) {
  console.log(key); // 아무것도 출력되지 않는다.
}

console.log(Object.keys(obj)); // []
console.log(Object.getOwnPropertyNames(obj)); // []

console.log(Object.getOwnPropertySymbols(obj)); // [ Symbol(mySymbol) ]
```

<br>
<br>
<br>
<br>
<br>

> # 📖33.6 심벌과 표준 빌트인 객체 확장

- 일반적으로 표준 빌트인 객체에 사용자 정의 메서드를 직접 추가하여 확장하는 것은 권장하지 않는다.
  - 미래에 표준 사양으로 추가될 메서드의 이름이 중복될 수 있기 때문
  - 중복될 가능성이 없는 심벌 값으로 프로퍼티 키를 생성하여 표준 빌트인 객체를 확장하면, 기존 프로퍼티 키와 충돌되지 않는 것을 보장하기 때문에 문제없다.

```js
// Array 표준 빌트인 객체에 메서드를 추가하여 확장하는 것 => 권장 X
Array.prototype.sum = function () {
  return this.reduce((acc, cur) => acc + cur, 0);
};

[1, 2].sum(); // -> 3

// // 심벌 값으로 프로퍼티 키를 동적 생성하면 다른 프로퍼티 키와 절대 충돌하지 않아 안전하다.
Array.prototype[Symbol.for('sum')] = function () {
  return this.reduce((acc, cur) => acc + cur, 0);
};

[1, 2][Symbol.for('sum')](); // -> 3
```

<br>
<br>
<br>
<br>
<br>

> # 📖33.7 Well-known Symbol

- 자바스크립트가 기본 제공하는 빌트인 심벌 값을 ECMAScript 사양에서는 Well-known Symbol이라고 한다.
- 자바스크립트 엔진의 내부 알고리즘에 사용된다.
- for...of 문으로 순회 가능한 이터러블은 Well-known Symbol 인 Symbol.iterator를 키로 갖는 메서드를 가진다.
  - Symbol.iterator 메서드를 호출하면 이터레이터를 반환한다.
- 만약, 일반 객체를 이터러블처럼 동작하도록 구현하고 싶다면 이터레이션 프로토콜을 따르면 된다.
  - 즉, Symbol.iterator 를 키로 갖는 메서드를 객체에 추가하고 이터레이터를 반환하도록 구현하면, 그 객체는 이터러블이 된다.

```js
// 1 ~ 5 범위의 정수로 이루어진 이터러블
const iterable = {
  // Symbol.iterator 메서드를 구현하여 이터러블 프로토콜을 준수
  [Symbol.iterator]() {
    let cur = 1;
    const max = 5;
    // Symbol.iterator 메서드는 next 메서드를 소유한 이터레이터를 반환
    return {
      next() {
        return { value: cur++, done: cur > max + 1 };
      }
    };
  }
};

for (const num of iterable) {
  console.log(num); // 1 2 3 4 5
}
```

<details>
<summary>💡 코드 해석</summary>

Symbol.iterator라는 메서드가 정의되어 있는데 이 메서드는 이터러블 프로토콜을 준수하기 위한 것으로 이 메서드를 호출하면 이터레이터를 반환해야 합니다.<br>
이터레이터는 next라는 메서드를 가진 객체를 의미하며 이 메서드를 호출할 때마다 순회할 수 있는 값을 하나씩 반환합니다. <br>이 next 메서드는 { value, done } 형태의 객체를 반환하는데 value는 순회할 값, done은 모든 데이터를 순회하였는지의 여부를 나타냅니다.
<br>이 코드에서는 next 메서드가 호출될 때마다 cur의 값을 1씩 증가시키며 반환합니다. <br>그리고 cur의 값이 max + 1보다 크면 done을 true로 반환하여 모든 데이터를 순회하였음을 알립니다.
<br>마지막으로 for...of 문을 사용하여 iterable 객체를 순회합니다. <br>이때, iterable 객체의 Symbol.iterator 메서드가 호출되어 이터레이터를 반환하며, 이터레이터의 next 메서드가 순차적으로 호출되어 순회할 값을 하나씩 가져옵니다. <br>이 과정을 통해 1부터 5까지의 숫자가 순차적으로 콘솔에 출력됩니다.

</details>