# 프로미스

- ES6부터 포함된 문법
- 비동기 상태를 값으로 가지는 **객체**
- **비동기 프로그래밍을 동기 프로그래밍 방식으로 코드를 작성할 수 있도록 함**

## 등장배경

### 콜백 패턴의 문제점

**코드의 흐름이 순차적이지 않다**

- 기존에는(프로미스가 등장하기 전) 비동기 처리를 위해 주로 콜백 함수를 사용
- 콜백 패턴은 코드의 가독성을 해칠 뿐 아니라 (너무 복잡), 함수의 실행시점을 바로 알기 어렵다는 단점
- 이를 보안하기 위해 등장한 것이 프로미스

**=> 프로미스를 사용하면, 코드가 순차적으로 실행되도록 작성할 수 있다.**

---

## 프로미스 기본

### 세가지 상태

프로미스는 비동기 상태에 따라 세가지의 상태값을 가진다.

| 상태      | 의미                                               |
| --------- | -------------------------------------------------- |
| pending   | 대기                                               |
| fulfilled | 성공 <br/> 정상적으로 완료 후 결과값을 가지고 있음 |
| rejected  | 실패                                               |

### 프로미스를 생성하는 방법

```js
// new 키워드 사용
const p1 = new Promise(resolve, reject);

// 프로미스 메소드 사용
const p2 = Promise.resolve(123);
const p3 = Promise.reject("error message");
```

- 프로미스는 resolve함수와, reject 함수를 인자로 받는다.

- 생성과 동시에 실행된다.

- 프로미스 생성자 함수 내에서 비동기 처리가 성공하면 resolve 함수를 호출하고 fulfilled 상태의 프로미스 객체를 반환한다.

- 프로미스 생성자 함수 내에서 비동기 처리가 실패하면 reject 함수를 호출하고 rejected 상태의 프로미스 객체를 반환한다.

- 메소드를 사용하면 각 상태의 프로미스를 생성한다.  
  resolve -> fulfilled  
  reject -> rejected

- p2: resolve 메소드에 인자로  
  프로미스가 아닌 값을 전달 -> 인자를 값으로 가진 프로미스 반환  
  프로미스 전달 -> 프로미스를 그대로 반환

### then과 catch: 프로미스 체이닝과 예외 처리

- `then(onResolve, onReject)`  
  then은 fulfilled 상태의 프로미스를 전달받았을 때 실행되는 onResolve 함수와 rejected 상태의 프로미스를 전달받았을 때 실행되는 onReject 함수를 인자로 받는다.

- then은 내부에서 반환하는 값을 가지는 **프로미스**를 반환한다.

- 그렇기 때문에 then은 또 다른 then으로 연결될 수 있다. (프로미스 체이닝)

- then은 반드시 반환값이 있어야 한다.

- **예외 처리는 catch로**  
  then에도 두번째 인자로 예외 처리 함수를 전달할 수 있지만 그렇게 되면
  1. 가독성에 좋지 않고
  2. then 안에서 예외가 발생했을 때 처리할 수 없다.  
     그렇기 때문에 예외 처리는 catch로 한다.

```js
Promise.resolve(123)
  .then(
    () => {
      throw new Error("hi");
    },
    (error) => {
      console.log("in then", error); // (건너뜀)
    }
  )
  .catch((error) => {
    console.log("in catch", error); // 'in catch' Error: 'hi'
  });
```

---

## 프로미스 활용

### Promise.finally

- 프로미스 가장 마지막에 사용된다.

- 이전의 사용된 프로미스의 값을 건드리지 않고 그대로 반환한다.

- ex) 데이터 요청의 성공, 실패 여부와 상관없이 서버에 로그를 보낼 때

### Promise.all

- 여러개의 프로미스를 **병렬**로 처리할 때 사용
- 병렬적으로 처리하고자 하는 프로미스를 배열 형태로 전달
- 전달된 프로미스가 **모두** 성공했을 때 fulfilled 상태의 프로미스 반환

```js
Promise.all([asyncFunc1(), asyncFunc2()]).then(([data1, data2]) => {
  console.log(data1, data2);
});
```

### Promise.race

- 전달받은 여러개의 프로미스 중 가장 빨리 완료된 프로미스를 반환
- 하나라도 처리됨 상태(fulfilled || rejected)가 되면 처리됨 상태의 프로미스 반환

---

## async, await

- then을 사용한 프로미스 체이닝 형식보다 가독성이 좋아진다.

- 프로미스를 완전히 대체하는 것은 아니다.  
  프로미스는 비동기 상태도 값으로 다룰 수 있기 때문에 프로미스가 더 큰 개념이다.

- 프로미스는 객체로 존재, async, await는 함수에 적용되는 개념이다.

- async 를 사용한 함수는, 그 내부의 반환값을 가지는 프로미스를 반환한다.

- **예외 처리는 try catch**  
  try 안의 코드를 실행 후 예외가 발생하면 catch 안의 코드를 실행한다.

- 비동기 함수를 병렬로 실행  
  await를 사용하지 않고 프로미스를 먼저 생성 후, 그 다음에 await를 사용한다.

```js
async function getData() {
  const p1 = asyncFunc1();
  const p2 = aynscFunc2();
  const data1 = await p1;
  const data2 = await p2;
  // ...
}
```

- async, await는 thenable을 지원한다.  
  thenable: 프로미스는 아니지만 then 메소드를 가진 객체  
  async await 함수는 thenabel을 프로미스처럼 처리한다.

```js
const thenable = {
  then(resolve, reject) {
    setTimeout(() => resolve(123), 1000);
  },
};

{
  const test = thenable.then();
  console.log(test);
  // undefined
}

async function ayncFunc() {
  const result = await thenable;
  console.log(result);
  // 123
}

ayncFunc();
```
