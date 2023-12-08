## 😀 정규 표현식이란?

: 일정한 패턴을 가진 문자열의 집합을 표현하기 위한 형식 언어
-> 자바스크립트 고유 문법 아님
-> 대부분 프로그래밍 언어와 코드 에디터에 내장
-> 자바스크립트는 ES3부터 도입
-> **패턴 매칭 기능**제공 (특정 패턴과 일치하는 문자열을 검색, 추출, 치환 기능)
-> 주석, 공백 허용하지 않음 (가독성은 좋지 않음)

```js
const tel = "010-1234-567팔";
const regExp = /^\d{3}-\{4}-\d{4}$/;
regExp.test(tel);
```

## 😀 정규 표현식의 생성

: 정규 표현식 객체(RegExp 객체)를 생성하기 위해서는 정규 표현식 리터럴과 RegExp 생성자 함수 사용
-> 정규표현식은 패턴, 플래그로 구성

### ❤️ 1. 정규 표현식 리터럴

/regexp/i
-> '/': 시작, 종료 기호
-> 그 사이에 **패턴**
-> 마지막엔 **플래그** (i: 대소문자를 구별하지 않음)

```js
const target = "Is this all there is?";
const regexp = /is/i;
regexp.text(target); //true
```

-> target 문자열에 대해 정규 표현식 regexp 패턴을 검색하여 매칭 결과 불리언으로 return

### ❤️ 2. 생성자 함수

```js
const target = "Is this all there is?";
const regexp = new RexExp(/is/i);
regexp.text(target); //true
```

#### 생성자 함수를 통한 RegExp 객체 동적 생성

```js
const count = (str, char) => (str.match(new RegExp(char, "gi")) ?? []).length;
count("Is this all there is?", "is"); //3
```

## 😀 RegExp 메서드

### ❤️ RegExp.prototype.exec

: 인수로 전달받은 문자열에 대해 정규 표현식의 패턴을 검색하여 매칭 결과 배열로 반환 (매칭 결과 없는 경우 null 반환)

```js
const target = "Is this all there is?";
const regExp = /is/;
regExp.exec(target); //["is", index: 5, input: ~~]
```

-> 모든 매칭 결과 반환하는 'g'플래그 지정해도, **첫번째 매칭 결과만** 반환함!!

### ❤️ RegExp.prototype.test

: 인수로 전달받은 문자열에 대해 정규 표현식의 패턴을 검색하여 매칭 결과를 불리언 값으로 반환

```js
const target = "Is this all there is?";
const regExp = /is/;

regExp.test(target); //true
```

### ❤️ String.prototype.match

: String 표준 빌트인 객체가 제공하는 match 메서드는 대상 문자열과 인수로 전달받은 정규 표현식과의 매칭 결과를 배열로 반환

```js
const target = "Is this all there is?";
const regExp = /is/;
regExp.exec(target); //["is", index: 5, input: ~~]
```

-> 모든 패턴을 검색하는 'g'플래그 지정하면 모든 매칭 결과 배열로 반환

```js
const target = "Is this all there is?";
const regExp = /is/g;

target.match(regExp);
```

## 😀 플래그

: 정규 표현식의 검색 방식을 설정 (옵션)
-> 하나 이상의 플래그 동시에 설정 가능
-> 첫 번째 매칭 대상만 검색하고 종료

#### i: 대소문자를 구별하지 않고 패턴 검색

#### g: 패턴과 일치하는 모든 문자열을 전역 검색

#### m: 문자열의 행이 바뀌더라도 패턴 검색을 계속

## 😀 패턴

: 문자열의 일정한 규칙을 표현하기 위해 사용 ('/'로 열고 닫고, 문자열의 따옴표는 생략)
-> 메타문자, 기호로 표현 가능
-> 패턴과 일치하는 문자열이 존재할 때, 정규 표현식과 **매치**한다고 표현

### ❤️ 문자열 검색

- test: 매치하는 지 (boolean 값 반환)
- match: 매칭 결과 반환

```js
const target='Is this all there is?';
const regExp = /is/플래그;
regExp.test(target); //true
target.match(regExp);
```

### ❤️ 임의의 문자열 검색 (.)

'.'은 임의의 문자 한 개를 의미

```js
const target = "Is this all there is?";
const regEXP = /.../g;
target.match(regExp);
```

-> 3자리 문자열과 매치

### ❤️ 반복 검색 ({n,m})

{n,m}: 앞선 패턴이 최소 n번, 최대 m번 반복되는 문자열

```js
const target = "A AA B BB Aa Bb AAA";
const regExp = /A{1,2}/g;
target.match(regExp); //["A", "AA", "A", "AA", "A"]
```

-> A가 최소 1번, 최대 2번 반복되는 문자열 전역으로 검색

#### n}: 앞선 패턴이 n번 반복되는 문자열

```js
const target = "A AA B BB Aa Bb AAA";
const regExp = /A{2}/g;
target.match(regExp); //["AA", AA"]
```

#### {n,}: 앞선 패턴이 최소 n번 이상 반복되는 문자열

```js
const target = "A AA B BB Aa Bb AAA";
const regExp = /A{2,}/g;
target.match(regExp); //["AA", AAA"]
```

#### '+': 앞선 패턴이 최소 한 번 이상 반복되는 문자열

{1,}과 같음

```js
const target = "A AA B BB Aa Bb AAA";
const regExp = /A+/g;
target.match(regExp); //["A", "AA", "A", "AAA"]
```

-> A가 한번 이상 반복되는 문자열

#### '?': 앞선 패턴이 최대 한 번이상 반복되는 문자열

{0,1}과 같음

```js
const target = "color colour";
const regExp = /colou?r/g;
target.match(regExp); //["color", "colour"]
```

### ❤️ OR 검색 (|)

```js
const target = "A AA B BB Aa Bb AAA";
const regExp = /A|B/g;
target.match(regExp); //["A", "A", "A", "B", "B", "B", "A", "B"]
```

#### A가 1번 이상이거나 B가 1번 이상인 경우

```js
const target = "A AA B BB Aa Bb AAA";
const regExp = /A+|B+/g;
target.match(regExp); //["A", "AA", "B", "BB", "A", "B"]
```

-> 간단히 표현하면, [AB]+와 같이 나타낼 수도 있음

#### A~Z 한 번 이상 반복되는 문자열

```js
const target = "A AA BB ZZ Aa Bb";
const regExp = /[A-Z]+/g;
target.match(regExp); //["A", "AA", "BB", "ZZ", "A", "B"]
```

#### 대소문자 구별하지 않고 알파벳 검색

```js
const target = "A AA Aa Bb 12";
const regExp = /[A-Za-z]+/g;
target.match(regExp); //["AA", "BB", "Aa", "Bb"]
```

#### 0-9가 한 번 이상 반복되는 문자열

```js
const target = "AA BB 12,345";
const regExp = /[0-9]+/g;
target.match(regExp); //["12","345"]
```

-> 쉼표 때문에 매칭 결과가 분리 (쉼표를 패턴에 포함)
-> 쉼표 포함시키기: /[0-9,]+/g
-> **'\d'가 숫자를 의미: [0-9] **
-> **'\D'는 문자를 의미**
-> **'\w': 알파벳, 순자, 언더스코어 ([A-Za-z0-9])**
-> '\W': 알파벳, 숫자, 언더스코어가 아닌 문자

```js
const target = "AA BB 12,345";
const regExp = /[\d,]+/g;
target.match(regExp); //["12,345"]
const regExp = /[\D,]+/g;
target.match(regExp); //["AA BB ", ","]
```

### ❤️ NOT 검색 (^)

: []내의 '^'는 not의 의미
-> [^0-9]: 숫자를 제외한 문자 ===\D

```js
const target = "AA BB 12 Aa Bb";
const regEXP = /[^0-9]+/g;
target.match(regExp); //["AA BB ", " Aa Bb"]
```

### ❤️ 시작 위치로 검색 (^)

: []밖의 '^'는 문자열의 시작을 의미

```js
const target = "https://~";
const regEXP = /^https/g;
target.test(regExp); //true
```

-> http로 시작하는 지 검사

### ❤️ 마지막 위치로 검색 ($)

: 문자열의 마지막을 의미하는 '$'

```js
const target = "https: //poiemaweb.com";
const regExp = /com$/;
regExp.test(target); //true
```

-> 마지막이 com으로 끝나는 지 검사

## 😀 자주 사용하는 정규표현식

### ❤️ 특정 단어로 시작하는 지 검사

```js
const target = "https: //poiemaweb.com";
/^https?:\/\//.test(target); //true
```

-> http://또는 https://로 시작하는 지 검사 ('s'가 없어도 매치)

### ❤️ 특정 단어로 끝나는 지 검사

```js
const target = "index.html";
/html$/.test(target); //true
```

-> 문자열이 html로 끝나는지 검사 ($는 문자열의 마지막을 의미)

### ❤️ 숫자로만 이루어진 문자열인지 검사

```js
const target = "12345";
/^\d+$/.test(target); //true
```

-> 처음과 끝이 숫자이고 최소 한 번 이상 반복되는 문자열과 매치

### ❤️ 하나 이상의 공백으로 시작하는 지 검사

```js
const target = " hI!";
/^[\s]+/.test(target); //true
```

-> 하나 이상의 공백으로 시작하는 지
[\s]: 여러가지 공백 문자를 의미 ([\t\r\n\v\f])

### ❤️ 아이디로 사용 가능한 지 검사

```js
const target = "abc123";
/^[A-Za-z0-9]{4,10}$/.test(target); //true
```

-> 알파벳 대소문자 또는 숫자로 시작하고 끝나며 4~10자리인지 검사

### ❤️ 메일 주소 형식에 맞는 지 검사

```js
const target = "anjisu2001@naver.com";
/^[0-9a-zA-Z]([-_\.]?[0-9a-zA-Z])*@[0-9a-zA-Z]([-_\.]?[0-9a-zA-Z])*\.[a-zA-Z]{2,3}$/.test(
  target
); //true
```

### ❤️ 핸드폰 번호 형식에 맞는 지 검사

```js
const target = "010-1234-5678";
/^\d{3}-\d{3,4}-\d{4}$/.test(target); //true
```

### ❤️ 특수 문자 포함 여부 검사

: A-Za-z0-9 이외의 문자를 특수문자라고 함

```js
const target = 'abc#123';
/([^A-Za-z0-9]/gi).test(target)/; //true
```

-> 특수 문자 제거: replace메서드 사용
target.replace([^A-Za-z0-9]/gi, '') //abc123

### ❤️ 주민등록번호

\d{6} - [1-4]\d{6}

### ❤️ IP 주소

([0-9]{1,3}) . ([0-9]{1,3}) . ([0-9]{1,3}) . ([0-9]{1,3})

### ❤️ 숫자 1000단위 콤마찍기

변수명.toString().replace(/\B(?=(\d{3})+(?!\d))/g, ","
