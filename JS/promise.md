# promise

> [유튜브 | 드림코딩 엘리 | 프로미스 개념붙 활용까지](https://www.youtube.com/watch?v=JB_yU6Oe2eE&t=722s).  
> [유튜브 | 코딩앙마 | promis? 나도 써보자, 기본 개념부터~](https://www.youtube.com/watch?v=CA5EDD4Hjz4&t=2s).  
> [poiemaweb | 프로미스](https://poiemaweb.com/es6-promise)

1. state
2. producer vs consumer

---

## 프로미스란?

> “A promise is an object that may produce a single value some time in the future”

**비동기 처리를 위한 자바스크립트의 내장 객체**

자바스크립트는 비동기 처리를 위한 하나의 패턴으로 콜백 함수를 사용한다. 하지만 전통적인 콜백 패턴은 콜백 헬로 인해 가독성이 나쁘고 비동기 처리 중 발생한 에러의 처리가 곤란하며 여러 개의 비동기 처리를 한번에 처리하는 데도 한계가 있다.

ES6에서는 비동기 처리를 위한 또 다른 패턴으로 프로미스(Promise)를 도입했다. 프로미스는 전통적인 콜백 패턴이 가진 단점을 보완하며 비동기 처리 시점을 명확하게 표현할 수 있다는 장점이 있다.

### 프로미스의 생성

프로미스는 Promise 생성자 함수를 통해 인스턴스화한다. Promise 생성자 함수는 비동기 작업을 수행할 콜백 함수를 인자로 전달받는데 이 콜백 함수는 resolve와 reject 함수를 인자로 전달받는다.

```js
const promise = new Promise((resolve, reject) => {...});
```

---

## state

프로미스는 비동기 처리가 성공하였는지, 실패하였는지에 따라 상태값 (state)를 갖는다.

Promise 생성자 함수가 인자로 전달받은 콜백 함수는 내부적으로 비동기 처리를 수행한다. 이때 비동기 처리가 성공하면 프로미스의 상태는 'fulfilled'가 되고 resolve 함수를 호출한다. 비동기 처리가 실패하면 상태는 'rejected'가 되고 reject 함수를 호출한다.

## 프로미스의 후속 처리 메소드

> then(), catch()

프로미스의 상태가 'rejected', 실패 상태가 되면 실패에 대한 오류값을 catch 함수로 전달받을 수 있다.

```js
function getData() {
  return new Promise(function (resolve, reject) {
    reject(new Error("Request is failed"));
  });
}

// reject()의 결과 값 Error를 err에 받음
getData()
  .then()
  .catch(function (err) {
    console.log(err); // Error: Request is failed
  });
```

## 프로미스의 에러 처리 방법

1. then 메소드의 두번째 인자로 전달
2. catch 메소드 사용

**하지만 에러처리는 가급적 catch 사용**

---

## producer

프로미스 객체는 선언이 되자마자 프로미스의 콜백함수가 바로 실행된다.
프로미스 내부에서 콜백함수로 네트워크 통신을 하려는 경우 이런 점을 염두에 두고, 불필요한 네트워크 통신이 되지 않도록 주의하여야 한다. (클릭시 executer가 호출되어야 하는 경우)

## consumer : then, catch, finallyv

---

```js
// sec 밀리초 뒤에 callback함수 (현재 시간을 호출하는 함수)를 호출하는 함수 선언
function delay(sec, callback) {
  setTimeout(() => {
    callback(new Date().toISOString());
  }, sec * 1000);
}

// 1초 뒤에 현재 시간을 콘솔창에 찍겠다!
delay(1, (result) => {
  console.log(1, result);
});

// delay 함수를 프로미스 방식으로 바꾼 함수
// 밀리초를 인자로 받고,
// 인자로 받은 밀리초 뒤에
// 현재시간을 반환하는 "프로미스"를 반환
// 프로미스에서는 return 값을 두 개 지정할 수 있는데 그 둘은 `resolve`와 `reject` 키워드를 써서 지정한다.
// resolve는 프로미스가 정상작동했을 때의 반환값
// reject는 프로미스가 정상적으로 작동하지 못했을 때 반환하는 오류값
function delayP(sec) {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      resolve(new Date().toISOString());
    }, sec * 1000);
  });
}

// 프로미스는 .then 메소드를 제공한다.
// 이 then 메소드는 콜백함수를 인자로 받고,
// 인자로 받은 함수를 프로미스를 완료한 후에, 즉 동기적으로 실행된다.
delayP(1)
  .then((result) => {
    console.log(1, result);
    return delayP(1);
  })
  .then((result) => {
    console.log(2, result);
  });
```

---

# async, await

함수를 선언할 때, function 앞에 async 키워드를 붙이는 것 만으로도 간단하게 프로미스 인스턴스를 만들 수 있다.
-> 앞에 async를 붙이고 함수를 선언하게 되면 그 함수는 자동으로 프로미스를 반환하게 된다. 이 때 함수 안의 return값이 프로미스의 resolve값이 된다.
-> async는 그 함수를 프로미스를 반환하는 함수로 만들어준다.
syntatic sugar

```js
async function myAsync() {
  return "async"; // promise{<resolved> : "async"}
}

myAsync().then((result) => console.log(result)); // "async"
```
