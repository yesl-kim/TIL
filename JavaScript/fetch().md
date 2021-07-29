## GET

---

- api 명세

```
설명: 유저 정보를 가져온다.
base url: https://api.google.com
endpoint: /user/3
method: get
응답형태:
    {
        "success": boolean,
        "user": {
            "name": string,
            "batch": number
        }
    }
```

- api 호출시

```js
fetch('https://api.google.com/user/3')
  .then(res => res.json())
  .then(res => {
    if (res.success) {
        console.log(`${res.user.name}` 님 환영합니다);
    }
  });
```

get은 읽어오는 함수기 때문에

읽으려는 데이터의 주소(api 주소)만 서버로 보내면 (요청)
성공여부 데이터와 읽으려는 데이터를 함께 보내준다. (응답)

응답형태가 다르다는 것에 주의하자

## POST

---

- api 명세

```
설명: 유저를 저장한다.
base url: https://api.google.com
endpoint: /user
method: post
요청 body:
    {
        "name": string,
        "batch": number
    }

응답 body:
    {
        "success": boolean
    }
```

- api 호출시

```
fetch('https://api.google.com/user', {
    method: 'post',
    body: JSON.stringify({
        name: "yeri",
        batch: 1
    })
  })
  .then(res => res.json())
  .then(res => {
    if (res.success) {
        alert("저장 완료");
    }
  })
```

post는 읽음과 동시에 api를 수정하는 함수이다.

요청을 할 때는 api 주소와 함께 수정하려는 데이터를 함께 보낸다.
**이때 보내는 데이터는 json형태로 보내야하기 때문에 꼭 stringyfy() 함수를 거쳐야한다.**
그러면 서버는 그에 대한 성공여부(boolean type)만 보내준다

## query string

---

다음 예제에서 '/3'이 아이디라고 할 때 경로로 (경로에 포함해서) 전달해 줘야하는 경우도 있고 파라미터로 전달해줘야 하는 경우도 있다. (이 부분은 백엔드 개발자가 결정한다.) 파라미터로 전달해줘야 할 때 쓰는 형식이 query string 이다.

템플릿 리터럴과는 다른 개념이니 주의하자.

기본적인 문법은 `?`로 query string의 시작을 알리고 `=`로 데이터의 key와 value를 구분한다. 파라미터가 여러개일 경우 `&`로 연결한다.

> `엔드포인트?key=value&key=value`

```
// 경로로 전달해주는 경우

설명: 유저 정보를 가져온다.
base url: https://api.google.com
endpoint: /user/3
method: get
응답형태:
    {
        "success": boolean,
        "user": {
            "name": string,
            "batch": number
        }
    }

// 파라미터로 전달해주는 경우

설명: 유저 정보를 가져온다.
base url: https://api.google.com
endpoint: /user
method: get
query string: ?id=아이디
응답형태:
    {
        "success": boolean,
        "user": {
            "name": string,
            "batch": number
        }
    }
```

- api 호출시

```js
// user id가 props를 통해 넘어온다고 가정
const { userId } = this.props;

// 경로로 전달
fetch('https://api.google.com/user/${userId}')
  .then(res => res.json())
  .then(res => {...}
  });

// 파라미터로 전달
fetch('https://api.google.com/user?id=${userId}')
  .then(res => res.json())
  .then(res => {...}
  });
```

## 첫번째 .then에 들어오는 res : 백엔드에서 응답 body를 주지 않는 경우 해결방법

---

![콘솔 창에 찍힌 reseponse object]()

이때 res는 서버에서 받아온 response 객체이다. 이 객체 안의 body가 실제 클라이언트에서 필요한 데이터인데, (api 명세에 적힌 body 데이터가 바로 이것) 잠겨있어서 볼 수가 없다. 이걸 풀어주고 body를 불러오는 함수가 `.json()`이라고 할 수 있다.

하지만 간혹 body를 담고 있지 않은 response 객체가 있다. ~~많다고 한다.~~

예를 들어 'POST' 방식으로 데이터와 수정할 데이터를 요청했을 때, 수정의 성공여부를 항상 body로 주는 것이 아니라 수정에 성공했을 떄는 status 코드로만 알려주고, 실패했을 때만 body에 {success: false, message: "에러메시지"} 로 응답할 수도 있다.

이럴 때 첫번째 then에서 .json() 함수를 쓰면, 수정에는 분명히 성공했지만 res 객체에 body가 없기 때문에 에러가 난다.

이럴 때 첫번째 then 함수에서 status code를 확인하는 로직을 추가할 수 있다.

- 상황

```
설명: 유저를 저장한다.
base url: https://api.google.com
endpoint: /user
method: post
요청 body:
    {
        "name": string,
        "batch": number
    }

응답 body:
    1. 제대로 저장했으면 status code를 200 전달. 응답 body는 없음
    2. 권한 오류가 생기면 status code를 403으로 전달하고. 응답 body는 아래와 같음
        {
            success: false,
            message: "권한이 없습니다"
        }
```

- api 호출 시

```js
fetch("https://api.google.com/user", {
  method: "post",
  body: JSON.stringify({
    name: "yeri",
    batch: 1,
  }),
})
  .then((res) => {
    if (res.status === 200) {
      alert("저장 완료");
    } else if (res.status === 403) {
      return res.json();
    }
  })
  .then((res) => {
    console.log("에러 메시지 ->", res.message);
  });
```
