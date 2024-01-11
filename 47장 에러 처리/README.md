> # 📖 에러 처리

## 에러처리의 필요성

```javascript
console.log("[Start]");

foo(); // ReferenceError: foo is not defined
// 발생한 에러를 방치하면 프로그램은 강제 종료된다.

// 에러에 의해 프로그램이 강제 종료되어 아래 코드는 실행되지 않는다.
console.log("[End]");
```

위와 같은 코드로 작성하게 되면 발생한 에러에 대해 대처하지 않고 프로그램이 강제 종료된다.
이러한 현상을 해결하기 위해서는 `try...catch`문을 사용하면 된다.

```javascript
console.log("[Start]");

try {
  foo();
} catch (error) {
  console.error("[에러 발생]", error);
  // [에러 발생] ReferenceError: foo is not defined
}

// 발생한 에러에 적절한 대응을 하면 프로그램이 강제 종료되지 않는다.
console.log("[End]");
```

## try...catch...finally 문

- 3개의 블록으로 구성된다.
- finally 문은 불필요하다면 생략가능
- catch 문도 생략 가능하지만 try문 쓰는 의미가 없어지므로 생략하지 않는다

```javascript
console.log("[Start]");

try {
  // 실행할 코드(에러가 발생할 가능성이 있는 코드)
  foo();
} catch (err) {
  // try 코드 블록에서 에러가 발생하면 이 코드 블록의 코드가 실행된다.
  // err에는 try 코드 블록에서 발생한 Error 객체가 전달된다.
  console.error(err); // ReferenceError: foo is not defined
} finally {
  // 에러 발생과 상관없이 반드시 한 번 실행된다.
  console.log("finally");
}

// try...catch...finally 문으로 에러를 처리하면 프로그램이 강제 종료되지 않는다.
console.log("[End]");
```

## Error 객체

- Error 생성자 함수는 에러 객체를 생성한다.
- Error 생성자 함수에는 에러를 상세히 설명하는 에러 메시지를 인수로 전달
- 에러 객체에는 message 프로퍼티와 stack프로퍼티를 갖는다

message: Error 생성자 함수에 인수로 전달한 에러 메시지<br>
stack: 에러를 발생시킨 콜스택의 혹출 정보를 나타내는 문자열이며 디버깅 목적으로 사용

```javascript
const error = new Error("invalid");
```

### 에러 객체 종류

|  생성자 함수   | 인스턴스                                                                       |
| :------------: | ------------------------------------------------------------------------------ |
|     Error      | 일반적인 에러 객체                                                             |
|  SyntaxError   | 자바스크립트 문법에 맞지 않는 문을 해석할 때 발생하는 에러 객체                |
| ReferenceError | 참조할 수 없는 식별자를 참조했을 때 발생하는 에러 객체                         |
|   TypeError    | 피연산자 또는 인수의 데이터 타입이 유효하지 않을 때 발생하는 에러 객체         |
|    URIError    | encodeURI 또는 decodeURI 함수에 부적절한 인수를 전달했을 때 발생하는 에러 객체 |
|   EvalError    | eval 함수에서 발생하는 에러 객체                                               |

그럼 에러 내용들은 어떤식으로 전달을 해줄까요?

```javascript
1 @ 1;    // SyntaxError: Invalid or unexpected token
foo();    // ReferenceError: foo is not defined
null.foo; // TypeError: Cannot read property 'foo' of null
new Array(-1); // RangeError: Invalid array length
decodeURIComponent('%'); // URIError: URI malformed
```

## throw 문

Error 생성자 함수로 에러 객체를 생성한다고 에러가 발생하는 것은 아니다. 즉 에러 객체 생성과 에러 발생은 의미가 다르다.

```javascript
try {
  // 에러 객체를 생성한다고 에러가 발생하는 것은 아니다.
  new Error("something wrong");
} catch (error) {
  console.log(error);
}
```

```javascript
try {
  // 에러 객체를 던지면 catch 코드 블록이 실행되기 시작한다.
  throw new Error("something wrong");
} catch (error) {
  console.log(error);
}
```

### 에러의 전파

```javascript
const foo = () => {
  throw Error("foo에서 발생한 에러"); // 4
};

const bar = () => {
  foo(); // 3
};

const baz = () => {
  bar(); // 2
};

try {
  baz(); // 1
} catch (err) {
  console.error(err);
}
```

1. baz함수를 호출
2. bar함수 호출 실행
3. foo 함수 호출 실행
4. 에러 발생하고 호출자에게 전파되어 전역에서 캐치된다

이처럼 throw된 에러를 캐치하지 않으면 호출자 방향으로 전파된다. 이때 throw된 에러를 캐치하면 적절히 대응하면 프로그램을 강제 종료시키지않고 코드의 실행 흐름을 복구할 수 있다.
