> # 📖 타이머

## 호출 스케줄링

함수를 명시적으로 호출하면 함수가 즉시 실행된다. 만약 함수를 명시적으로 호출하지 않고 일정 시간이 경과된 이후에 호출되도록 함수 호출을 예약하려면 타이머 함수를 사용한다. 이를 `호출 스케줄링`이라고 한다.

자바스크립트는 타이머를 생성할 수 있는 setTimeout과 setInterval, 타이머를 제거할 수 있는 타이머 함수 clearTimeout과 clearInterval을 제공한다.

setTimeout 함수가 생성한 타이머는 단 한 번 동작하고, setInterval 함수가 생성한 타이머는 반복 동작한다. 즉, setTimeout 함수의 콜백 함수는 타이머가 만료되면 단 한 번 호출되고, setInterval 함수의 콜백 함수는 타이머가 만료될 때마다 반복 호출된다.

자바스크립트 엔진은 `싱글 스레드` 방식으로 동작하기 때문에 타이머 함수는 `비동기 처리 방식`으로 동작한다.

<br/>

## 타이머 함수

```js
const timeoutId = setTimeout(func|code[, delay, param1, param2, ...]);
```

| 매개변수            | 설명                                                                                                                                          |
| ------------------- | --------------------------------------------------------------------------------------------------------------------------------------------- |
| func                | 타이머가 만료된 뒤 호출될 콜백 함수                                                                                                           |
| delay               | 타이머 만료 시간(ms 밀리초 단위). setTimeout 함수는 delay 시간으로 단 한 번 동작하는 타이머를 생성. 인수 전달을 생략하면 자동으로 0이 지정됨. |
| param1, param2, ... | 호출 스케줄링된 콜백함수에 전달해야 할 인수가 존재하는 경우 세 번째 이후 인수로 전달                                                          |

```js
const timerId = setTimeout(() => console.log("Hi"));
// 두 번째 인수를 생략하면 기본값 0이 지정된다.

setTimeout((name) => console.log(`Hi ${name}.`, 1000, "Lee"));
// 1초(1000ms) 후 타이머가 만료되면 콜백 함수가 호출되며, 'Lee'가 인수로 전달된다.

clearTimeout(timerId);
// 인수로 전달한 timerId 타이머를 취소한다.
```

setInterval 함수는 두 번째 인수로 전달받은 시간(ms)으로 반복 동작하는 타이머를 생성한다.

```js
const timeoutId = setInterval(func|code[, delay, param1, param2, ...]);
```

```js
let count = 1;

const timeoutId = setInterval(() => {
  console.log(count); // 1 2 3 4 5 -> 1초 후 타이머가 만료될 때마다 콜백 함수가 호출된다. setInterval 함수는 생성된 타이머를 식별할 수 있는 고유한 타이머 id를 반환한다.

  if (count++ === 5) clearInterval(timeoutId);
  // count가 5이면 setInterval 함수가 반환한 타이머 id를 clearInterval 함수의 인수로 전달하여 타이머를 취소한다.
}, 1000);
```

<br/>

## 디바운스와 스로틀

scroll, resize, input, mousemove 같은 이벤트는 짧은 시간 간격으로 연속해서 발생한다. 이러한 이벤트에 바인딩한 이벤트 핸들러는 과도하게 호출되어 성능에 문제를 일으킬 수 있다. `디바운스와 스로틀은 짧은 시간 간격으로 연속해서 발생하는 이벤트를 그룹화해서 과도한 이벤트 핸들러의 호출을 방지`하는 프로그래밍 기법이다.

✅ 디바운스

디바운스는 짧은 시간 간격으로 이벤트가 연속해서 발생하면 이벤트 핸들러를 호출하지 않다가 일정 시간이 경과한 이후에 이벤트 핸들러가 한 번만 호출되도록 한다. 즉, 이벤트를 그룹화해서 마지막 한 번만 이벤트 핸들러가 호출되도록 하는 방식.

```js
// 텍스트 입력 필드에서 input 이벤트가 짧은 간격으로 연속해서 발생하는 경우
// (인풋 입력 값으로 Ajax 요청 같은 무거운 처리를 수행한다면 디바운스를 적용해주는 것이 좋다)

<!DOCTYPE html>
<html>
<body>
  <input type="text">
  <div class="msg"></div>
  <script>
    const $input = document.querySelector('input');
    const $msg = document.querySelector('.msg');

    const debounce = (callback, dealy) => {
      let timerId;
      // 디바운스 함수는 timerId를 기억하는 클로저를 반환한다.

      return (...args) => {
        if(timerId) clearTimeout(timerId);
        timerId = setTimeout(callback, delay, ...args);
        // delay가 경과하기 이전에 이벤트가 발생하면, 이전 타이머를 취소하고 새로운 타이머를 세팅한다.
        // delay 보다 짧은 간격으로 이벤트가 발생하면 callback은 호출되지 않는다.
      };
    };

    $input.oninput = debounce(e => {
      $msg.textContent = e.target.value;
    }, 300);
    // 300ms 보다 짧은 간격으로 input 이벤트가 발생하면 debounce의 콜백함수는 호출되지 않다가 300ms 동안 input 이벤트가 더 이상 발생하지 않으면 한 번만 호출된다.

  </script>
</body>
</html>
```

위 예제는 이해를 위한 간략한 구현 버전이므로, 실무에서는 디바운스가 필요한 경우 Underscore의 debounce 함수(https://underscorejs.org/#debounce) 혹은 Lodash의 debounce 함수(https://lodash.com/docs/4.17.15#debounce)를 사용하는 것을 권장한다.

<br/>

✅ 스로틀

짧은 시간 간격으로 이벤트가 연속해서 발생하더라도 일정 시간 간격으로 이벤트 핸들러가 최대 한 번만 호출되도록 한다. 즉, 이벤트를 그룹화해서 일정 시간 단위로 이벤트 핸들러가 호출되도록 호출 주기를 만드는 방식.

```js
// 스크롤 이벤트가 짧은 시간 간격으로 연속해서 발생하는 경우

<!DOCTYPE html>
<html>
<head>
  <style>
    .container {
      width: 300px;
      height: 300px;
      background-color: purple;
      overflow: scroll;
    }

    .content {
      width: 300px;
      height: 1000vh;
    }
  </style>
</head>
<body>
  <div class="container">
    <div class="content"></div>
  </div>
  <div>
    일반 이벤트 핸들러가 scroll 이벤트를 처리한 횟수:
    <span class="normal-count">0</span>
  </div>
  <div>
    스로틀 이벤트 핸들러가 scroll 이벤트를 처리한 횟수:
    <span class="throttle-count">0</span>
  </div>

  <script>
    const $container = document.querySelector('.container');
    const $normalCount = document.querySelector('.normal-count');
    const $throttleCount = document.querySelector('.throttle-count');

    const throttle = (callback, dealy) => {
      let timerId;
      // 스로틀 함수는 timerId를 기억하는 클로저를 반환한다.

      return (...args) => {
        if(timerId) return;
        timerId = setTimeout(() => {
          callback(...args);
          timerId = null;
        }, delay);
        // delay가 경과하기 이전에 이벤트가 발생하면, 아무것도 하지 않다가 delay가 경과했을 때 이벤트가 발생하면 새로운 타이머를 재설정.
        // delay 간격으로 callback이 호출된다.
      };
    };

    let normalCount = 0;
    $container.addEventListener('scroll', () => {
      $normalCount.textCount = ++normalCount;
    });

    let throttleCount = 0;
    $container.addEventListener('scroll', throttle() => {
      $throttleCount.textCount = ++throttleCount;
    }, 100);
    // 스로틀 함수가 반환하는 클로저가 이벤트 핸들러로 등록된다.

  </script>
</body>
</html>
```

위 예제는 이해를 위한 간략한 구현 버전이므로, 실무에서는 스로틀이 필요한 경우 Underscore의 throttle 함수(https://underscorejs.org/#throttle) 혹은 Lodash의 throttle 함수(https://lodash.com/docs/4.17.15#throttle)를 사용하는 것을 권장한다.

<br/>
