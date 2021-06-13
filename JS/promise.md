# promise

> [유튜브 | 드림코딩 엘리 | 프로미스 개념붙 활용까지](https://www.youtube.com/watch?v=JB_yU6Oe2eE&t=722s).
> [유튜브 | 코딩앙마 | promis? 나도 써보자, 기본 개념부터~](https://www.youtube.com/watch?v=CA5EDD4Hjz4&t=2s).

> 비동기 처리를 위한 자바스크립트의 내장 객체

1. state
2. producer vs consumer

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
