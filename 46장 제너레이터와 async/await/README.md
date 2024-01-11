> # 📖46.1 제너레이터란?

ES6에서 도입된제너레이터는 코드 블록의 실행을 일시 중지했다가 필요한 시점에 재개할 수 있는 특수한 함수다.

## 제너레이터와 일반 함수의 차이

### 차이점 1. 제너레이터 함수는 함수 호출자에게 함수 실행의 제어권을 양도할 수 있다.

- 일반 함수
  - 호출 시 제어권이 함수에게 넘어가고 함수 코드를 일괄 실행
  - 즉, 함수 호출자(caller)는 함수를 호출한 이후 함수 실행을 제어할 수 없다.
- 제너레이터 함수
  - 함수 실행을 함수 호출자가 제어할 수 있다.
  - 즉, 함수 호출자가 함수 실행을 일시 중지시키거나 재개시킬 수 있다.
  - 함수의 제어권을 함수가 독점하는 것이 아니라 함수 호출자에게 양도(yield)할 수 있다.

### 차이점 2. 제너레이터 함수는 함수 호출자와 함수의 상태를 주고받을 수 있다.

- 일반 함수
  - 호출하면 매개변수를 통해 함수 외부에서 값을 주입받고 함수 코드를 일괄 실행하여 결과값을 함수 외부로 반환한다.
  - 즉, 함수가 실행되고 있는 동안에는 함수 외부에서 함수 내부로 값을 전달하여 함수의 상태를 변경할 수 없다.
- 제너레이터 함수
  - 함수 호출자와 양방향으로 함수의 상태를 주고 받을 수 있다.
  - 즉, 제너레이터 함수는 함수 호출자에게 상태를 전달할 수 있고 함수 호출자로부터 상대를 전달받을 수 있다.

### 차이점 3. 제너레이터 함수를 호출하면 제너레이터 객체를 반환한다.

- 일반 함수
  - 호출하면 함수 코드를 일괄 실행하고 값을 반환한다.
- 제너레이터 함수
  - 함수 코드를 실행하는 것이 아니라 이터러블이면서 동시에 이터레이터인 제너레이터 객체를 반환한다.

<br>
<br>
<br>
<br>
<br>

> # 📖46.1 제너레이터 함수의 정의

- 제너레이터 함수는 function\* 키워드로 선언하며 하나 이상의 yield 표현식을 포함한다.
- 에스터리스크(\*)의 위치는 function 키워드와 함수 이름 사이라면 어디든 상관없다.
  - 다만, 일관성을 위해 function 키워드 바로 뒤에 붙이는 것을 권장한다.

```js
// 제너레이터 함수 선언문
function* genDecFunc() {
  yield 1;
}

// 제너레이터 함수 표현식
const genExpFunc = function* () {
  yield 1;
};

// 제너레이터 메서드
const obj = {
  *genObjMethod() {
    yield 1;
  },
};

// 제너레이터 클래스 메서드
class MyClass {
  *genClsMethod() {
    yield 1;
  }
}
```

- 제너레이터 함수는 화살표 함수로 정의할 수 없다.

```js
const genArrowFunc = * () => {
  yield 1;
}; // SyntaxError: Unexpected token '*'
```

- 제너레이터 함수는 new 연산자와 함께 생성자 함수로 호출할 수 없다.

```js
function* genFunc() {
  yield 1;
}

new genFunc(); // TypeError: genFunc is not a constructor
```

<br>
<br>
<br>
<br>
<br>

> # 📖46.3 제너레이터 객체

- 제너레이터 함수를 호출하면 일반 함수처럼 함수 코드 블록을 실행하는 것이 아니라 제너레이터 객체를 생성해 반환한다.
- 제너레이터 함수가 반환한 제너레이터 객체는 이터러블이면서 동시에 이터레이터다.
  - 즉, Symbol.iterator 메서드를 상속받는 이터러블이면서 value, done 프로퍼티를 갖는 이터레이터 리절트 객체를 반환하는 next 메서드를 소유하는 이터레이터다.

```js
// 제너레이터 함수
function* genFunc() {
  yield 1;
  yield 2;
  yield 3;
}

// 제너레이터 함수를 호출하면 제너레이터 객체를 반환한다.
const generator = genFunc();

// 제너레이터 객체는 이터러블이면서 동시에 이터레이터다.
// 이터러블은 Symbol.iterator 메서드를 직접 구현하거나 프로토타입 체인을 통해 상속받은 객체다.
console.log(Symbol.iterator in generator); // true
// 이터레이터는 next 메서드를 갖는다.
console.log('next' in generator); // true

generator.next();
//{value: 1, done: false}
generator.next();
//{value: 2, done: false}
generator.next();
//{value: 3, done: false}
generator.next();
//{value: undefined, done: true}
```

- 제너레이터 객체는 추가적으로 이터레이터에는 없는 return, throw 메서드를 갖는다.
  - next 메서드 호출
    - 제너레이터 함수의 yield 표현식까지 코드 블록을 실행
    - yield 된 값을 value 프로퍼티 값으로
    - false 를 done 프로퍼티 값으로 갖는 이터레이터 리절트 객체를 반환
  - return 메서드 호출
    - 인수로 전달받은 값을 value 프로퍼티 값으로
    - true를 done 프로퍼티 값으로 갖는 이터레이터 리절트 객체를 반환
  - throw 메서드 호출
    - 인수로 전달받은 에러를 발생시키고
    - undefined 를 value 프로퍼티 값으로
    - true 를 done 프로퍼티 값으로 갖는 이터레이터 갖는 이터레이터 리절트 객체를 반환

```js
function* generatorFunc() {
  try {
    yield 1;
    yield 2;
    yield 3;
  } catch (e) {
    console.log(e);
  }
}

const generator = generatorFunc();

console.log(generator.next()); // { value: 1, done: false }
console.log(generator.return()); // { value: undefined, done: true }
```

```js
function* generatorFunc() {
  try {
    yield 1;
    yield 2;
    yield 3;
  } catch (e) {
    console.log(e);
  }
}

const generator = generatorFunc();

console.log(generator.next());
// { value: 1, done: false }
console.log(generator.throw('Error !'));
// Error !
// { value: undefined, done: true }
```

<br>
<br>
<br>
<br>
<br>

> # 📖46.4 제너레이터 일시 중지와 재개

- 제너레이터는 yield 키워드와 next 메서드를 통해 실행을 일시 중지했다가 필요한 시점에 다시 재개할 수 있다.
- yield 키워드는 제너레이터 함수의 실행을 일시 중지시키거나 yield 키워드 뒤에 오는 표현식의 평가 결과를 제너레이터 함수 호출자에게 반환한다.

```js
// 제너레이터 함수
function* genFunc() {
  yield 1;
  yield 2;
  yield 3;
}

// 제너레이터 함수를 호출하면 제너레이터 객체를 반환한다.
// 이터러블이면서 동시에 이터레이터인 제너레이터 객체는 next 메서드를 갖는다.
const generator = genFunc();

// 처음 next 메서드를 호출하면 첫 번째 yield 표현식까지 실행되고 일시 중지된다.
// next 메서드는 이터레이터 리절트 객체({value, done})를 반환한다.
// value 프로퍼티에는 첫 번째 yield 표현식에서 yield된 값 1이 할당된다.
// done 프로퍼티에는 제너레이터 함수가 끝까지 실행되었는지를 나타내는 false가 할당된다.
console.log(generator.next()); // {value: 1, done: false}

// 다시 next 메서드를 호출하면 두 번째 yield 표현식까지 실행되고 일시 중지된다.
// next 메서드는 이터레이터 리절트 객체({value, done})를 반환한다.
// value 프로퍼티에는 두 번째 yield 표현식에서 yield된 값 2가 할당된다.
// done 프로퍼티에는 제너레이터 함수가 끝까지 실행되었는지를 나타내는 false가 할당된다.
console.log(generator.next()); // {value: 2, done: false}

// 다시 next 메서드를 호출하면 세 번째 yield 표현식까지 실행되고 일시 중지된다.
// next 메서드는 이터레이터 리절트 객체({value, done})를 반환한다.
// value 프로퍼티에는 세 번째 yield 표현식에서 yield된 값 3이 할당된다.
// done 프로퍼티에는 제너레이터 함수가 끝까지 실행되었는지를 나타내는 false가 할당된다.
console.log(generator.next()); // {value: 3, done: false}

// 다시 next 메서드를 호출하면 남은 yield 표현식이 없으므로 제너레이터 함수의 마지막까지 실행한다.
// next 메서드는 이터레이터 리절트 객체({value, done})를 반환한다.
// value 프로퍼티에는 제너레이터 함수의 반환값 undefined가 할당된다.
// done 프로퍼티에는 제너레이터 함수가 끝까지 실행되었음을 나타내는 true가 할당된다.
console.log(generator.next()); // {value: undefined, done: true}
```

- 제너레이터 객체의 next 메서드를 호출하면 yield 표현식까지 실행되고 일시 중지된다.
- 이때 함수의 제어권이 호출자로 양도된다. 이후 필요한 시점에 호출자가 또다시 next 메서드를 호출하면 일시 중지된 코드부터 실행을 재개하기 시작하여 다음 yield 표현식까지 실행되고 또 다시 일시 중지된다.
- 제너레이터 객체의 next 메서드에 전달한 인수는 제너레이터 함수의 yield 표현식을 할당받는 변수에 할당된다.

```js
function* genFunc() {
  // 복잡한 로직
  const a = yield 1;

  // 복잡한 로직
  const b = yield 2 + a;

  // 복잡한 로직
  yield 3 + b;
}

const gen = genFunc();

gen.next();
// {value: 1, done: false}

gen.next(10);
// {value: 12, done: false}

gen.next(20);
// {value: 23, done: false}
```

<br>
<br>
<br>
<br>
<br>

> # 📖46.5 제너레이터의 활용

## 📌46.5.1 이터러블의 구현

- 제너레이터 함수를 활용하면 이터레이션 프로토콜을 준수해 이터러블을 생성하는 방식보다 간단히 이터러블을 구현할 수 있다.

<br>

- 기존 이터레이션 프로토콜을 준수하여 구현한 이터러블 예시

```js
// 무한 이터러블을 생성하는 함수
const infiniteFibonacci = (function () {
  let [pre, cur] = [0, 1];

  return {
    [Symbol.iterator]() {
      return this;
    },
    next() {
      [pre, cur] = [cur, pre + cur];
      // 무한 이터러블이므로 done 프로퍼티를 생략한다.
      return { value: cur };
    },
  };
})();

// infiniteFibonacci는 무한 이터러블이다.
for (const num of infiniteFibonacci) {
  if (num > 10000) break;
  console.log(num); // 1 2 3 5 8...2584 4181 6765
}
```

- 제너레이터 함수를 활용하여 구현한 이터러블 예시

```js
// 무한 이터러블을 생성하는 제너레이터 함수
const infiniteFibonacci = (function* () {
  let [pre, cur] = [0, 1];

  while (true) {
    [pre, cur] = [cur, pre + cur];
    yield cur;
  }
})();

// infiniteFibonacci는 무한 이터러블이다.
for (const num of infiniteFibonacci) {
  if (num > 10000) break;
  console.log(num); // 1 2 3 5 8...2584 4181 6765
}
```

<br>
<br>

## 📌46.5.2 비동기 처리

- 제너레이터 함수를 활용하면 프로미스를 사용한 비동기 처리를 동기 처리처럼 구현할 수 있다.
- 프로미스의 후속 처리 메서드 then/catch/finally 없이 비동기 처리 결과를 반환하도록 구현할 수 있다.

```js
// 제너레이터 실행기
const async = (generatorFunc) => {
  const generator = generatorFunc(); // ②

  const onResolved = (arg) => {
    const result = generator.next(arg); // ⑤

    return result.done
      ? result.value // ⑨
      : result.value.then((res) => onResolved(res)); // ⑦
  };

  return onResolved; // ③
};

async(function* fetchTodo() {
  // ①
  const url = 'https://jsonplaceholder.typicode.com/todos/1';

  const response = yield fetch(url); // ⑥
  const todo = yield response.json(); // ⑧
  console.log(todo);
  // {userId: 1, id: 1, title: 'delectus aut autem', completed: false}
})(); // ④
```

<br>
<br>
<br>
<br>
<br>

> # 📖46.6 async/await

- ES8에서 도입되었으며 간단하고 가독성 좋게 비동기 처리를 동기 처리처럼 동작하도록 구현할 수 있다.

<br>
<br>

## 📌46.6.1 async 함수

- await 키워드는 반드시 async 함수 내부에서 사용해야 한다.
- 언제나 프로미스를 반환한다.
- async 함수가 명시적으로 프로미스를 반환하지 않더라도 async 함수는 암묵적으로 반환값을 resolve하는 프로미스를 반환한다.

```js
// async 함수 선언문
async function foo(n) {
  return n;
}
foo(1).then((v) => console.log(v)); // 1

// async 함수 표현식
const bar = async function (n) {
  return n;
};
bar(2).then((v) => console.log(v)); // 2

// async 화살표 함수
const baz = async (n) => n;
baz(3).then((v) => console.log(v)); // 3

// async 메서드
const obj = {
  async foo(n) {
    return n;
  },
};
obj.foo(4).then((v) => console.log(v)); // 4

// async 클래스 메서드
class MyClass {
  async bar(n) {
    return n;
  }
}
const myClass = new MyClass();
myClass.bar(5).then((v) => console.log(v)); // 5
```

- 클래스의 constructor 메서드는 async 메서드가 될 수 없다.

```js
class MyClass {
  async constructor() { }
  // SyntaxError: Class constructor may not be an async method
}

const myClass = new MyClass();
```

<br>
<br>

## 📌46.6.2 await 키워드

- await 키워드는 프로미스가 settled 상태가 될 때까지 대기하다가 settled 상태가 되면 프로미스가 resolve한 처리 결과를 반환한다.
- await 키워드는 반드시 프로미스 앞에서 사용해야 한다.

```js
const fetch = require('node-fetch');

async function fetchTodo() {
  const url = 'https://jsonplaceholder.typicode.com/todos/1';

  const response = await fetch(url);
  const todo = await response.json();
  console.log(todo);
  // {userId: 1, id: 1, title: 'delectus aut autem', completed: false}
}

fetchTodo();
```

- await 키워드는 다음 실행을 일시 중지시켰다가 프로미스가 settled 상태가 되면 다시 재개하는 방식이다.

```js
async function foo() {
  const a = await new Promise((resolve) => setTimeout(() => resolve(1), 3000));
  const b = await new Promise((resolve) => setTimeout(() => resolve(2), 2000));
  const c = await new Promise((resolve) => setTimeout(() => resolve(3), 1000));

  console.log(a, b, c);
}

foo();
// ( 약 6초 뒤... )
// 1 2 3

/**
 * 첫 번째 비동기 처리가 약 3초 소요되는 동안 대기
 * 첫 번째 비동기 처리가 완료되고, 두 번째 비동기 처리가 약 2초 소요되는 동안 대기
 * 두 번째 비동기 처리가 완료되고, 세 번째 비동기 처리가 약 1초 소요되는 동안 대기
 *
 * 마지막으로 console 출력하고 종료
 *
 * 총 = 약 6초
 */
```

- 모든 프로미스에 await 키워드를 사용하는 것은 주의해야 한다.

  - 각각의 비동기 처리가 서로 연관성 없이 개별적으로 수행되도 상관없는 것이라면, 굳이 앞선 비동기 처리가 완료될 때까지 대기할 필요가 없기 때문이다.
  - 따라서, 모든 프로미스가 서로 연관되어 순서가 보장되면서 실행되어야 하는 경우에만 모든 프로미스의 await 를 적용한다.

  ```js
  async function foo() {
    // 각각의 비동기 처리가 서로 연관이 없다.
    const a = new Promise((resolve) => setTimeout(() => resolve(1), 3000));
    const b = new Promise((resolve) => setTimeout(() => resolve(2), 2000));
    const c = new Promise((resolve) => setTimeout(() => resolve(3), 1000));

    // 모든 비동기 처리를 "병렬"로 실행
    const res = await Promise.all([a, b, c]);
    console.log(res);
  }

  foo();
  // ( 약 3초 뒤... )
  // 1 2 3
  ```

  - 그렇지 않은 경우는, 오히려 모든 프로미스의 await 를 적용하는 것은 전체 프로그램 실행 속도만 늦춘다.

  ```js
  async function foo() {
    // 각각의 비동기 처리가 서로 연관되어 있다.
    // 첫 번째 비동기 처리 결과 -> 두 번째 비동기 처리에 필요
    // 두 번째 비동기 처리 결과 -> 세 번째 비동기 처리에 필요
    const a = await new Promise((resolve) => setTimeout(() => resolve(1), 3000));
    const b = await new Promise((resolve) => setTimeout(() => resolve(a + 1), 2000));
    const c = await new Promise((resolve) => setTimeout(() => resolve(b + 1), 1000));

    console.log(a, b, c);
  }

  foo();
  // ( 약 6초 뒤... )
  // 1 2 3
  ```

<br>
<br>

## 📌46.6.3 에러 처리

- 기존 콜백 패턴의 단점은 비동기 처리에서 에러가 발생 시, 에러가 호출자(caller)방향으로 전파되는 원리를 이용해 에러를 캐치하지 못하는 단점이 존재했다.

```js
try {
  setTimeout(() => {
    throw new Error('Error!');
  }, 1000);
} catch (e) {
  // 에러를 캐치하지 못한다
  console.error('캐치한 에러', e);
}
```

- async / await 방식의 비동기 처리를 하면 try - catch 문을 사용해 에러를 캐치할 수 있다.

```js
const fetch = require('node-fetch');

const foo = async () => {
  try {
    const wrongUrl = 'https://wrong.url';

    const response = await fetch(wrongUrl);
    const data = await response.json();
    console.log(data);
  } catch (err) {
    console.error(err); // TypeError: Failed to fetch
  }
};

foo();
```

- async 함수 내에서 catch 문을 사용해서 에러 처리를 하지 않으면 async 함수는 발생한 에러를 reject 하는 프로미스를 반환한다.

```js
const fetch = require('node-fetch');

const foo = async () => {
  const wrongUrl = 'https://wrong.url';

  const response = await fetch(wrongUrl);
  const data = await response.json();
  return data;
};

foo().then(console.log).catch(console.error); // TypeError: Failed to fetch
```
