> # 📖43.1 Ajax란?

Ajax(Asynchronous Javascript and XML)란 자바스크립트를 사용하여 브라우저가 서버에게 비동기 방식으로 데이터를 요청하고, 서버가 응답한 데이터를 수신한여 웹페이지를 동적으로 갱신하는 프로그래밍 방식이다.

- 브라우저에서 제공하는 Web API인 XMLHttpRequest 객체를 기반으로 동작
- XMLHttpRequest 객체는 HTTP 비동기 통신을 위한 메서드와 프로퍼티를 제공

<br>
<br>

![전통적인 웹 페이지 생명 주기](https://poiemaweb.com/img/traditional-webpage-lifecycle.png)

### 전통적인 방식

- Ajax 등장 이전 웹페이지는 완전한 HTML을 서버로부터 전송받아 웹페이지 전체를 처음부터 다시 렌더링 하는 방식으로 동작
- 화면이 전환되면 서버로부터 새로운 HTML을 전송받아 웹페이지 전체를 리렌더링했다.
- 이러한 방식의 단점
  - 이전 웹 페이지와 차이가 없어서 변경할 필요가 없는 부분까지 포함된 완전한 HTML을 서버로부터 매번 다시 전송받기 때문에 불필요한 데이터 통신 발생
  - 변경할 필요가 없는 부분까지 처음부터 다시 렌더링하기 때문에 화면 전환이 일어나면 화면이 순간적으로 깜빡이는 현상 발생
  - 클라이언트와 서버와의 통신이 동기 방식으로 동작하기 때문에 서버로부터 응답이 있을 때까지 다음 처리는 블로킹된다.

<br>
<br>

![Ajax](https://poiemaweb.com/img/ajax-webpage-lifecycle.png)

### Ajax

- 전통적인 방식과 비교했을 때 다음과 같은 장점 존재
  - 변경할 부분을 갱신하는 데 필요한 데이터만 서버로부터 전송받기 때문에 불필요한 데이터 통신이 발생하지 않는다.
  - 변경할 필요가 없는 부분은 리렌더링하지 않기 때문에 화면이 순간적으로 깜빡이는 현상이 발생하지 않는다.
  - 클라이언트와 서버와의 통신이 비동기 방식으로 동작하기 때문에 서버에게 요청을 보낸 이후 블로킹이 발생하지 않는다.

<br>
<br>
<br>
<br>
<br>

> # 📖43.2 JSON

- JSON(JavaScript Object Notation)은 클라이언트와 서버 간의 HTTP 통신을 위한 텍스트 데이터 포맷이다.
- 자바스크립트에 종속되지 않은 언어 독립형 데이터 포맷이다.

<br>

## 📌43.2.1 JSON 표기 방식

- JSON은 자바스크립트 객체 리터럴과 유사하게 키와 값으로 구성된 순수한 텍스트다.
- JSON의 키는 반드시 큰따옴표로 묶어야 한다.

```js
{
  "name": "Lee",
  "age": 20,
  "alive": true,
  "hobby": ["traveling", "tennis"]
}
```

<br>
<br>

## 📌43.2.2 JSON.stringify

- JSON.stringify 메서드는 객체를 JSON 포맷의 문자열로 변환한다.
- 클라이언트가 서버로 객체를 전송하려면 객체를 문자열화해야 한다.
  - 이를 직렬화(serializing)라 한다.

```js
const obj = {
  name: 'Lee',
  age: 20,
  alive: true,
  hobby: ['traveling', 'tennis'],
};

// 객체를 JSON 포맷의 문자열로 변환한다.
const json = JSON.stringify(obj);
console.log(typeof json, json);
// string {"name":"Lee","age":20,"alive":true,"hobby":["traveling","tennis"]}

// 객체를 JSON 포맷의 문자열로 변환하면서 들여쓰기 한다.
const prettyJson = JSON.stringify(obj, null, 2);
console.log(typeof prettyJson, prettyJson);
/*
string {
  "name": "Lee",
  "age": 20,
  "alive": true,
  "hobby": [
    "traveling",
    "tennis"
  ]
}
*/

// replacer 함수. 값의 타입이 Number이면 필터링되어 반환되지 않는다.
function filter(key, value) {
  // undefined: 반환하지 않음
  return typeof value === 'number' ? undefined : value;
}

// JSON.stringify 메서드에 두 번째 인수로 replacer 함수를 전달한다.
const strFilteredObject = JSON.stringify(obj, filter, 2);
console.log(typeof strFilteredObject, strFilteredObject);
/*
string {
  "name": "Lee",
  "alive": true,
  "hobby": [
    "traveling",
    "tennis"
  ]
}
*/
```

<br>
<br>

## 📌43.2.3 JSON.parse

- JSON.parse 메서드는 JSON 포맷의 문자열을 객체로 변환한다.
- 서버로부터 클라이언트에게 전송된 JSON 데이터는 문자열인데 객체로 사용하기 위해 객체화가 필요하다.
- 이를 역직렬화(deserializing)라 한다.

```js
const obj = {
  name: 'Lee',
  age: 20,
  alive: true,
  hobby: ['traveling', 'tennis'],
};

// 객체를 JSON 포맷의 문자열로 변환한다.
const json = JSON.stringify(obj);

// JSON 포맷의 문자열을 객체로 변환한다.
const parsed = JSON.parse(json);
console.log(typeof parsed, parsed);
//  object {
//    name: "Lee",
//    age: 20,
//    alive: true,
//    hobby: ["traveling", "tennis"]
//  }

// 간단한 깊은 복사 방법
const json1 = JSON.parse(JSON.stringify(obj));
```

<br>
<br>
<br>
<br>
<br>

> # 📖43.3 XMLHttpRequest

- 자바스크립트를 사용하여 HTTP 요청을 전송할 때 사용
- 브라우저는 주소창이나 HTML의 form 태그 또는 a 태그를 통해 HTTP 요청 전송 기능을 기본 제공
- XMLHttpRequest 객체는 HTTP 요청 전송과 HTTP 응답 수신을 위한 다양한 프로퍼티와 메서드를 제공

<br>
<br>

## 📌43.3.1 XMLHttpRequest 객체 생성

- XMLHttpRequest 객체는 XMLHttpRequest 생성자 함수를 호출하여 생성
- XMLHttpRequest 객체는 브라우저에서 제공하는 Web API이므로 브라우저 환경에서만 정상적으로 실행된다.

```js
// XMLHttpRequest 객체 생성
const xhr = new XMLHttpRequest();
```

<br>
<br>

## 📌43.3.2 XMLHttpRequest 객체의 프로퍼티와 메서드

- XMLHttpRequest 객체는 다양한 프로퍼티와 메서드를 제공

<br>

### XMLHttpRequest 객체의 프로토타입 프로퍼티

| 프로토타입 프로퍼티 | 설명                                                                                               |
| ------------------- | -------------------------------------------------------------------------------------------------- |
| **readyState**      | HTTP 요청의 현재 상태를 나타내는 정수: 다음과 같은 XMLHttpRequest의 정적 프로퍼티를 값으로 갖는다. |
|                     | UNSENT: 0                                                                                          |
|                     | OPENED: 1                                                                                          |
|                     | HEADERS_RECEIVED: 2                                                                                |
|                     | LOADING: 3                                                                                         |
|                     | DONE: 4                                                                                            |
| **status**          | HTTP 요청에 대한 응답 상태(HTTP 상태 코드)를 나타내는 정수 예) 200                                 |
| **statusText**      | HTTP 요청에 대한 응답 메시지를 나타내는 문자열 예)"OK"                                             |
| **responseType**    | HTTP 응답 타입 예) document, json, text, blob, arraybuffer                                         |
| **response**        | HTTP 요청에 대한 응답 몸체(response body), responseType에 따라 타입이 다르다.                      |

<br>

### XMLHttpRequest 객체의 이벤트 핸들러 프로퍼티

| 이벤트 핸들러 프로퍼티 | 설명                                              |
| ---------------------- | ------------------------------------------------- |
| **onreadystatechange** | readyState 프로퍼티 값이 변경된 경우              |
| **onerror**            | HTTP 요청에 에러가 발생한 경우                    |
| **onload**             | HTTP 요청이 성공적으로 완료한 경우                |
| onloadstart            | HTTP 요청에 대한 응답을 받기 시작한 경우          |
| onloadend              | HTTP 요청이 완료한 경우 ( 성공 or 실패하면 발생 ) |
| onabort                | abort 메서드에 의해 HTTP 요청이 중단된 경우       |
| onprogress             | HTTP 요청에 대한 응답을 받는 도중 주기적으로 발생 |
| ontimeout              | HTTP 요청 시간이 초과한 경우                      |

<br>

### XMLHttpRequest 객체의 메서드

| 메서드               | 설명                                     |
| -------------------- | ---------------------------------------- |
| **open**             | HTTP 요청 초기화                         |
| **send**             | HTTP 요청 전송                           |
| **abort**            | 이미 전송된 HTTP 요청 중단               |
| **setRequestHeader** | 특정 HTTP 요청 헤더의 값 설정            |
| getResponseHeader    | 특정 HTTP 요청 헤더의 값을 문자열로 반환 |

<br>

### XMLHttpRequest 객체의 정적 프로퍼티

| 정적 프로퍼티    | 값  | 설명                                  |
| ---------------- | --- | ------------------------------------- |
| UNSENT           | 0   | open 메서드 호출 이전                 |
| OPENED           | 1   | open 메서드 호출 이후                 |
| HEADERS_RECEIVED | 2   | send 메서드 호출 이후                 |
| LOADING          | 3   | 서버 응답 중(응답 데이터 미완성 상태) |
| **DONE**         | 4   | 서버 응답 완료                        |

<br>
<br>
<br>
<br>
<br>

## 📌43.3.3 HTTP 요청 전송

HTTP 요청을 전송하는 경우 다음 순서를 따른다.

- XMLHttpRequest.prototype.open 메서드로 HTTP 요청을 초기화한다.
- 필요에 따라 XMLHttpRequest.prototype.setRequestHeader 메서드로 특정 HTTP 요청의 헤더 값을 설정한다.
- XMLHttpRequest.prototype.send 메서드로 HTTP 요청을 전송한다.

```js
// XMLHttpRequest 객체 생성
const xhr = new XMLHttpRequest();

// HTTP 요청 초기화
xhr.open('GET', '/users');

// HTTP 요청 헤더 설정
// 클라이언트가 서버로 전송할 데이터의 MIME 타입 지정: json
xhr.setRequestHeader('content-type', 'application/json');

// HTTP 요청 전송
xhr.send();
```

### XMLHttpRequest.prototype.open

서버에 전송할 HTTP 요청을 초기화한다.

```js
xhr.open(mehtod, url[, async])
```

| 매개변수 | 설명                                                              |
| -------- | ----------------------------------------------------------------- |
| method   | HTTP 요청 메서드(”GET”, “POST”, “PUT”, “PATCH”, “DELETE” 등 )     |
| url      | HTTP 요청을 전송할 URL                                            |
| async    | 비동기 요청 여부, 옵션이며 기본값은 true이다 비동기 방식으로 동작 |

- HTTP 요청 메서드는 클라이언트가 서버에게 요청의 종류와 목적을 알리는 방법으로 CRUD를 구현한다.

| HTTP 요청 메서드 | 종류           | 목적                  | 페이로드 |
| ---------------- | -------------- | --------------------- | -------- |
| GET              | index/retrieve | 모든/특정 리소스 취득 | X        |
| POST             | create         | 리소스 생성           | O        |
| PUT              | replace        | 리소스의 전체 교체    | O        |
| PATCH            | modify         | 리소스의 일부 수정    | O        |
| DELETE           | delete         | 모든/특정 리소스 삭제 | X        |

### XMLHttpRequest.prototype.send

open 메서드로 초기화된 HTTP 요청을 서버에 전송

- 서버에 전송하는 데이터는 GET, POST 요청 메서드에 따라 전송 방식 차이가 있다.
  - GET 요청 메서드의 경우 데이터를 URL의 일부인 쿼리 문자열(query string)로 서버에 전송
    - send 메서드에 페이로드로 전달한 인수는 무시되고 요총 몸체는 null로 설정
  - POST 요청 메서드의 경우 데이터(페이로드(payload))를 요청 몸체(request body)에 담아 전송
    - 페이로드가 객체인 경우 반드시 JSON.stringify 메서드를 사용하여 직렬화 후 전달

### XMLHttpRequest.prototype.setRequestHeader

특정 HTTP 요청의 헤더 값을 설정

- 반드시 open 메서드 호출한 후에 호출
- 자주 사용하는 HTTP 요청 헤더에는 Content-type과 Accept가 있다.

  - Content-type은 request body에 담아 전송할 데이터의 MIME 타입의 정보를 표현  
    | MIME 타입 | 서브타입 |
    | ----------- | -------------------------------------------------- |
    | text | text/plain, text/html/, text/css, text/javascript |
    | application | application/json, application/x-www-form-urlencode |
    | multipart | multipart/formed-date |

    ```js
    // XMLHttpRequest 객체 생성
    const xhr = new XMLHttpRequest();

    // HTTP 요청 초기화
    xhr.open('POST', '/users');

    // HTTP 요청 헤더 설정
    // 클라이언트가 서버로 전송할 데이터의 MIME 타입 지정: json
    xhr.setRequestHeader('content-type', 'application/json');

    // HTTP 요청 전송
    xhr.send(JSON.stringify({ id: 1, content: 'HTML', completed: false }));
    ```

  - HTTP 클라이언트가 서버에 요청할 때 서버가 응답할 데이터의 MIME 타입을 Accept로 설정 가능
    ```js
    xhr.setRequestHeader('accept', 'application/json');
    ```
  - Accept 헤더를 설정하지 않으면 send 메서드가 호출될 때 Accept 헤더가 */*으로 전송

<br>
<br>
<br>
<br>
<br>

## 📌43.3.4 HTTP 응답 처리

- 서버가 전송한 응답을 처리하려면 XMLHttpRequest 객체가 발생시키는 이벤트를 캐치해야 한다.
- HTTP 요청의 현재 상태를 나타내는 readyState 프로퍼티 값이 변경된 경우 발생하는 readystatechange 이벤트를 캐치하여 다음과 같이 처리

  - send 메서드로 HTTP 요청을 서버에 전송하면 서버는 응답을 반환해야 한다.
  - 하지만, 언제 응답이 클라이언트에 도달하는지 알 수 없기 때문에 사용

  ```js
  // XMLHttpRequest 객체 생성
  const xhr = new XMLHttpRequest();

  // HTTP 요청 초기화
  // https://jsonplaceholder.typicode.com은 Fake REST API를 제공하는 서비스다.
  xhr.open('GET', 'https://jsonplaceholder.typicode.com/todos/1');

  // HTTP 요청 전송
  xhr.send();

  // readystatechange 이벤트는 HTTP 요청의 현재 상태를 나타내는 readyState 프로퍼티가
  // 변경될 때마다 발생한다.
  xhr.onreadystatechange = () => {
    // readyState 프로퍼티는 HTTP 요청의 현재 상태를 나타낸다.
    // readyState 프로퍼티 값이 4(XMLHttpRequest.DONE)가 아니면 서버 응답이 완료되지 않은 상태다.
    // 만약 서버 응답이 아직 완료되지 않았다면 아무런 처리를 하지 않는다.
    if (xhr.readyState !== XMLHttpRequest.DONE) return;

    // status 프로퍼티는 응답 상태 코드를 나타낸다.
    // status 프로퍼티 값이 200이면 정상적으로 응답된 상태이고
    // status 프로퍼티 값이 200이 아니면 에러가 발생한 상태다.
    // 정상적으로 응답된 상태라면 response 프로퍼티에 서버의 응답 결과가 담겨 있다.
    if (xhr.status === 200) {
      console.log(JSON.parse(xhr.response));
      // {userId: 1, id: 1, title: "delectus aut autem", completed: false}
    } else {
      console.error('Error', xhr.status, xhr.statusText);
    }
  };
  ```

  - readystatechange 이벤트 대신 load 이벤트를 캐치할 수도 있다.
    - HTTP 요청이 성공적으로 완료된 경우에 발생, readyState가 XMLHttpRequest.DONE 인지 확인할 필요가 없다.

  ```js
  // XMLHttpRequest 객체 생성
  const xhr = new XMLHttpRequest();

  // HTTP 요청 초기화
  // https://jsonplaceholder.typicode.com은 Fake REST API를 제공하는 서비스다.
  xhr.open('GET', 'https://jsonplaceholder.typicode.com/todos/1');

  // HTTP 요청 전송
  xhr.send();

  // load 이벤트는 HTTP 요청이 성공적으로 완료된 경우 발생한다.
  xhr.onload = () => {
    // status 프로퍼티는 응답 상태 코드를 나타낸다.
    // status 프로퍼티 값이 200이면 정상적으로 응답된 상태이고
    // status 프로퍼티 값이 200이 아니면 에러가 발생한 상태다.
    // 정상적으로 응답된 상태라면 response 프로퍼티에 서버의 응답 결과가 담겨 있다.
    if (xhr.status === 200) {
      console.log(JSON.parse(xhr.response));
      // {userId: 1, id: 1, title: "delectus aut autem", completed: false}
    } else {
      console.error('Error', xhr.status, xhr.statusText);
    }
  };
  ```
