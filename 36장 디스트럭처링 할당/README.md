- 디스트럭처링 할당 (구조 분해 할당): 구조화된 **배열과 같은 이터러블 또는 객체를 비구조화하여 1개 이상의 변수에 개별적으로 할당하는 것**
  --> 즉, 구조 분해 할당은 배열이나 객체 변수들에 분해해서 할당하는 것
  (필요한 값만 추출할 때 유용)

## 😀 배열 디스트럭처링 할당

- 우변: 이터러블 (배열, 객체)
- 좌변: 할당받을 변수 (배열 리터럴 형태로)
- 할당 기준: 배열의 인덱스 (순서대로 할당)
  -> 배열의 각 요소를 추출하여 1개 이상의 변수에 할당

```js
const arr = [1, 2, 3];

const [one, two, three] = arr;
console.log(one, two, three);
```

-> 좌변엔 변수들 배열 리터럴 형태로, 우변엔 이터러블

```js
const [a, b] = {};
```

-> 우변 이터러블 할당하지 않으면 에러 발생

```js
let x, y;
[x, y] = [1, 2];
```

-> 선언과 할당 분리 가능
-> const 키워드 사용할 수 없으므로 권장하진 않음!!!

```js
const [a, b] = [1, 2];
console.log(a, b); //1 2

const [c, d] = [1];
console.log(c, d); //1 undefined

const [e, f] = [1, 2, 3];
console.log(e, f); //1 2

const [g, , h] = [1, 2, 3];
console.log(g, h); //1 3
```

-> 배열의 인덱스를 기준으로 순서대로 할당

```js
const [a, b, c = 3] = [1, 2];
console.log(a, b, c); //1 2 3

const [e, f = 10, g = 3] = [1, 2];
console.log(e, f, g); //1 2 3
```

-> 변수에 기본값을 설정할 수 있다

```js
const [x, ...y] = [1, 2, 3];
console.log(x, y); //1 [2, 3]
```

-> rest요소 사용할 수 있는데, 마지막에 위치해야 함

## 😀 객체 디스트럭처링 할당

- 우변: 객체, 객체로 평가될 수 있는 표현식
- 좌변: 할당받을 변수 (객체 리터럴)
- 할당 기준: 프로퍼티 키
  --> **순서는 의미 없고**, 변수 이름과 프로퍼티 키 일치하면 할당

```js
const { lastName, firstName } = user;
console.log(firstName, lastName); //Ungmo Lee
```

```js
const { latName, firstName } = { firstName: "Ungmo", lastName: "Lee" };
```

```js
const { lastName, firstName } = null; //TypeError
```

-> 우변에 객체 또는 객체로 평가될 수 있는 표현식을 할당하지 않으면 에러 발생

```js
const { lastName, firstName } = user;
const { lastName: lastName, firstName: firstName } = user;
```

-> 위 둘은 동치이다

```js
const user = { firstName: "Ungmo", lastName: "Lee" };
const { lastName: ln, firstName: fn } = user;

console.log(fn, ln); //Ungmo Lee
```

-> **객체의 프로퍼티 키와 다른 변수 이름**으로 프로퍼티 값 할당받기 위해선 위와 같이 **좌변에 변수 선언**

```js
const { firstName = "Ungmo", lastName } = { lastName: "Lee" };
console.log(firstName, lastName); //Ungmo Lee

const { firstName: fn = "Ungmo", lastName: ln } = { lastName: "Lee" };
console.log(fn, ln); //Ungmo Lee
```

-> 객체 구조 분해할당에 위와 같이 기본값을 지정할 수 있다

```js
const str = "Hello";
const { length } = str;
console.log(length); //5

const todo = { id: 1, content: "HTML", completed: true };
const { id } = todo;
console.log(id); // 1
```

-> 객체에서 프로퍼티 키로 필요한 프로퍼티 값만 추출하여 변수에 할당할 수 있음

```js
function printTodo({ content, completed }) {
  console.log(`할일 ${content}은 ${completed ? "완료" : "비완료"} 상태입니다.`);
}
printTodo({ id: 1, content: "HTML", completed: true });
```

-> 객체를 인수로 받는 경우 객체 디스트럭팅 할당을 통해 받을 수 있다

### ❤️ 배열의 요소가 객체인 경우

: 배열 구조분해 할당과 객체 구조분해 할당을 혼용할 수 있다

```js
const todos = [
  { id: 1, content: "HTML", completed: true },
  { id: 2, content: "CSS", completed: false },
  { id: 3, content: "JS", completed: false },
];

const [, { id }] = todos;
console.log(id); //2
```

-> 배열의 두번째 요소의 객체 값 중에서도 id 값을 가져오게 된다

### ❤️ 중첩 객체

```js
const user = {
  name: "Lee",
  address: {
    zipCode: "03068",
    city: "Seoul",
  },
};

const {
  address: { city },
} = user;
console.log(city); //'Seoul'
```

-> address 프로퍼티 키로 객체 추출하고, 이 객체의 city 프로퍼티 키로 값을 추출

```js
const { x, ...rest } = { x: 1, y: 2, z: 3 };
console.log(x, rest); //1 {y:2, z:3}
```

-> rest 프로퍼티를 사용할 수 있고, 마지막에 위치해야 함!!

### 객체던 배열이던 좌변엔 변수, 우변엔 이터러블 형태의 배열이나 객체!!!
