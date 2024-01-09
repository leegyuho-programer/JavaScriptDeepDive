> # 📖 REST API

- REST는 HTTP를 기반으로 클라이언트가 서버의 리소스에 접근하는 방식, REST API는 REST를 기반으로 서비스 API를 구현한 것을 의미
- RESTful은 REST의 기본원칙을 성실히 지킨 서비스 디자인이다
- REST API는 자원, 행위, 표현 3가지 구성요소로 구성된다.

| 구성요소 | 내용                           | 표현방법         |
| -------- | ------------------------------ | ---------------- |
| 자원     | 자원                           | URI(엔드포인트)  |
| 행위     | 자원에 대한 행위               | HTTP 요청 메서드 |
| 표현     | 자원에 대한 행위의 구체적 내용 | 페이로드         |

## REST API 설계 원칙

1. URI는 리소스를 표현해야한다
   - 이름에 get 같은 행위에 대한 표현이 들어가서는 안된다(식별할 수 있는 이름에는 동사보다는 명사 사용)
2. 리소스에 대한 행위는 HTTP 요청 메서드로 표현한다
   - HTTP 요청 메서드는 클라이언트가 서버에 요청의 종류와 목적을 알리는 방법이다.
   - 주로 5가지 요청 메서드를 사용하여 CRUD를 구현한다

| HTTP 요청 메서드 | 종류           | 목적                  | 페이로드 |
| :--------------: | -------------- | --------------------- | :------: |
|       GET        | index/retrieve | 모든/특정 리소스 취득 |    X     |
|       POST       | create         | 리소스 생성           |    O     |
|       PUT        | replace        | 리소스의 전체 교체    |    O     |
|      PATCH       | modify         | 리소스의 일부 수정    |    O     |
|      DELETE      | delete         | 모든/특정 리소스 삭제 |    X     |

## JSON Server 를 이용한 REST API 실습

임시 서버 데이터를 생성하는 실습

### 1. JSON Server 설치

- npm 사이트 참고

```
$ mkdir json-server-exam && cd json-server-exam
$ npm init -y
$ npm install json-server --save-dev
```

### 2. db.json파일 생성

```json
{
  "todos": [
    {
      "id": 1,
      "content": "HTML",
      "completed": true
    },
    {
      "id": 2,
      "content": "CSS",
      "completed": false
    },
    {
      "id": 3,
      "content": "Javascript",
      "completed": true
    }
  ]
}
```

### 3. JSON Server 실행

db.json 파일의 변경을 감지 하려면 watch 옵션 추가

```
### 기본 포트(3000)사용 / watch 옵션 적용
$ json-server --watch db.json
```

포트 변경하려면 port 옵션 추가

```
#포트 변경 / port 옵션 적용
$ json-server --watch db.json --port 5000
```

위와 같이 매번 명령어를 입력하기 번거로우니 package.json 파일의 script 수정

```json
{
  "name": "json-server-exam",
  "version": "1.0.0",
  "scripts": {
    "start": "json-server --watch db.json"
  },
  "devDependencies": {
    "json-server": "^0.16.1"
  }
}
```

그리고 터미널에 npm start 입력하면 JSON Server 실행

### GET 요청

여러 데이터 조회

```javascript
<!DOCTYPE html>
<html>
<body>
  <pre></pre>
  <script>
    // XMLHttpRequest 객체 생성
    const xhr = new XMLHttpRequest();

    // HTTP 요청 초기화
    // todos 리소스에서 모든 todo를 취득(index)
    xhr.open('GET', '/todos');

    // HTTP 요청 전송
    xhr.send();

    // load 이벤트는 요청이 성공적으로 완료된 경우 발생한다.
    xhr.onload = () => {
      // status 프로퍼티 값이 200이면 정상적으로 응답된 상태다.
      if (xhr.status === 200) {
        document.querySelector('pre').textContent = xhr.response;
      } else {
        console.error('Error', xhr.status, xhr.statusText);
      }
    };
  </script>
</body>
</html>
```

하나의 데이터 조회

```javascript
<!DOCTYPE html>
<html>
<body>
  <pre></pre>
  <script>
    // XMLHttpRequest 객체 생성
    const xhr = new XMLHttpRequest();

    // HTTP 요청 초기화
    // todos 리소스에서 id를 사용하여 특정 todo를 취득(retrieve)
    xhr.open('GET', '/todos/1');

    // HTTP 요청 전송
    xhr.send();

    // load 이벤트는 요청이 성공적으로 완료된 경우 발생한다.
    xhr.onload = () => {
      // status 프로퍼티 값이 200이면 정상적으로 응답된 상태다.
      if (xhr.status === 200) {
        document.querySelector('pre').textContent = xhr.response;
      } else {
        console.error('Error', xhr.status, xhr.statusText);
      }
    };
  </script>
</body>
</html>
```

### POST 요청

- 서버로 전송할 페이로드의 MIME 타입 지정
- 새로운 데이터를 body(몸체)에 담아서 요청한다

```javascript
<!DOCTYPE html>
<html>
<body>
  <pre></pre>
  <script>
    // XMLHttpRequest 객체 생성
    const xhr = new XMLHttpRequest();

    // HTTP 요청 초기화
    // todos 리소스에 새로운 todo를 생성
    xhr.open('POST', '/todos');

    // 요청 몸체에 담아 서버로 전송할 페이로드의 MIME 타입을 지정
    xhr.setRequestHeader('content-type', 'application/json');

    // HTTP 요청 전송
    // 새로운 todo를 생성하기 위해 페이로드를 서버에 전송해야 한다.
    xhr.send(JSON.stringify({ id: 4, content: 'Angular', completed: false }));

    // load 이벤트는 요청이 성공적으로 완료된 경우 발생한다.
    xhr.onload = () => {
      // status 프로퍼티 값이 200(OK) 또는 201(Created)이면 정상적으로 응답된 상태다.
      if (xhr.status === 200 || xhr.status === 201) {
        document.querySelector('pre').textContent = xhr.response;
      } else {
        console.error('Error', xhr.status, xhr.statusText);
      }
    };
  </script>
</body>
</html>
```

### PUT 요청

- 특정 리소스 전체를 교체할 때 사용
- 서버로 전송할 페이로드의 MIME 타입 지정
- 수정할 데이터를 body(몸체)에 담아서 요청한다

```javascript
<!DOCTYPE html>
<html>
<body>
  <pre></pre>
  <script>
    // XMLHttpRequest 객체 생성
    const xhr = new XMLHttpRequest();

    // HTTP 요청 초기화
    // todos 리소스에서 id로 todo를 특정하여 id를 제외한 리소스 전체를 교체
    xhr.open('PUT', '/todos/4');

    // 요청 몸체에 담아 서버로 전송할 페이로드의 MIME 타입을 지정
    xhr.setRequestHeader('content-type', 'application/json');

    // HTTP 요청 전송
    // 리소스 전체를 교체하기 위해 페이로드를 서버에 전송해야 한다.
    xhr.send(JSON.stringify({ id: 4, content: 'React', completed: true }));

    // load 이벤트는 요청이 성공적으로 완료된 경우 발생한다.
    xhr.onload = () => {
      // status 프로퍼티 값이 200이면 정상적으로 응답된 상태다.
      if (xhr.status === 200) {
        document.querySelector('pre').textContent = xhr.response;
      } else {
        console.error('Error', xhr.status, xhr.statusText);
      }
    };
  </script>
</body>
</html>
```

### PATCH 요청

- 특정 리소스 일부를 교체할 때 사용
- 서버로 전송할 페이로드의 MIME 타입 지정
- 수정할 데이터를 body(몸체)에 담아서 요청한다

```javascript
<!DOCTYPE html>
<html>
<body>
  <pre></pre>
  <script>
    // XMLHttpRequest 객체 생성
    const xhr = new XMLHttpRequest();

    // HTTP 요청 초기화
    // todos 리소스의 id로 todo를 특정하여 completed만 수정
    xhr.open('PATCH', '/todos/4');

    // 요청 몸체에 담아 서버로 전송할 페이로드의 MIME 타입을 지정
    xhr.setRequestHeader('content-type', 'application/json');

    // HTTP 요청 전송
    // 리소스를 수정하기 위해 페이로드를 서버에 전송해야 한다.
    xhr.send(JSON.stringify({ completed: false }));

    // load 이벤트는 요청이 성공적으로 완료된 경우 발생한다.
    xhr.onload = () => {
      // status 프로퍼티 값이 200이면 정상적으로 응답된 상태다.
      if (xhr.status === 200) {
        document.querySelector('pre').textContent = xhr.response;
      } else {
        console.error('Error', xhr.status, xhr.statusText);
      }
    };
  </script>
</body>
</html>
```

### DELETE 요청

- 리소스에서 id를 사용하여 삭제
- 제가 개인적으로 사용했을 때는 클릭이벤트를 사용해서 id 값을 가져왔음

```javascript
<!DOCTYPE html>
<html>
<body>
  <pre></pre>
  <script>
    // XMLHttpRequest 객체 생성
    const xhr = new XMLHttpRequest();

    // HTTP 요청 초기화
    // todos 리소스에서 id를 사용하여 todo를 삭제한다.
    xhr.open('DELETE', '/todos/4');

    // HTTP 요청 전송
    xhr.send();

    // load 이벤트는 요청이 성공적으로 완료된 경우 발생한다.
    xhr.onload = () => {
      // status 프로퍼티 값이 200이면 정상적으로 응답된 상태다.
      if (xhr.status === 200) {
        document.querySelector('pre').textContent = xhr.response;
      } else {
        console.error('Error', xhr.status, xhr.statusText);
      }
    };
  </script>
</body>
</html>
```
