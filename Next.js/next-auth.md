# next-auth 공식문서 야곰야곰 파악하기

이 모든 글은 jwt 인증방식을 사용한다는 가정 하에 작성!

## options

### session

session은 사용자 정보를 얻기 위해 저장하고 있는 값이고, token은 서버 요청시 자동으로 전달하는 access token이 담긴, 쿠키에 저장되는 값이디.

- strategy: 인증방식. 기본값은 jwt ("database" | "jwt")
  - 데이터베이스와 연결하는 adapter 설정도 할 수 있는데, 어답터를 사용할 경우 기본적으로 인증 방식은 세션 방식을 사용하게 된다.
  - jwt 인증방식을 사용할 경우 더 자세한 설정은 jwt 속성을 통해 할 수 있다. (인코딩 방식, 토큰의 유효기간 등)
- maxAge: session 유효기간(초). 기본값은 30일 (number)
  - session.maxAge와 token.maxAge는 엄연히 다르다.
  - token은 자체 서버에서 발행하는 토큰과 다르게 next auth에서 발행하는 토큰을 말한다.

### jwt

- maxAge: 유효기간 (초)

## callbacks

- 클라이언트에서 필요한 사용자 정보일 경우, 즉 유지시켜야하는 정보가 있다면 session에 저장해주어야한다.
- 유지하고 싶은 정보는 jwt, session 콜백에서 token 인자를 통해 상호 전달하여 session 객체에 저장할 수 있다.
- 결국 유지되는 값은 session 객체이고, token 객체는 기본적으로 session에 저장되지 않는다.
- jwt callback -> token에 저장 -> session callback 안에서 token으로 전달받아서 session에 저장 -> session 유지, useSession, getSession 함수를 통해 원하는 값에 접근

### jwt

```ts
jwt: (params: {
  token: JWT
  user?: User | AdapterUser
  account?: A | null
  profile?: P
  isNewUser?: boolean
}) => Awaitable<JWT>
```

- jwt가 생성될 때 (로그인시) + 변경될 때 (session에 접근할 때)마다 호출된다. _(jwt 인증방식을 사용한다는 가정하에 (`{strategy: 'jwt'}`))_
- **반환값을 token이라 간주하며 쿠키에 저장된다** (=반환값이 곧 jwt가 된다)
- 이렇게 쿠키에 저장된 **token은 getToken 메소드**를 통해 얻을 수 있다

  ```ts
  export const getServerSideProps: GetServerSideProps = async ({ req }) => {
    // env 파일에서 NEXTAUTH_SECRET 변수를 사용하면 secret 인자 생략가능
    // const token = await getToken({ req, secret })
    const token = await getToken({ req })
    // ...
  }

  export default async function handler(req, res) {
    const token = await getToken({ req })
    console.log('JSON Web Token', token)
    res.end()
  }
  ```

- 처음 로그인하고 호출되는 jwt callback에서만 user, account, profile, isNewUser 인자를 사용할 수 있다.
- 로그인시 서버 응답값이 user 인자로 들어온다. (이 때 token은 없다)
- user, account, profile, isNewUser 값은 로그인사는 형태(provider)와 데이터베이스 사용유무에 따라 다 다르다!
  ```js
  // 예시
  user:  {
    token: 'eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpZCI6MTYsInR5cGUiOiJpbnRlcnByZXRlciIsImlhdCI6MTY4MDkxODA5MSwiZXhwIjoxNjgxMDA0NDkxfQ.V0Eh-f6G0CYkZl1Z_UbDg3lMd84VMQWVLp7T91F79xo',
    interpreter: {
      id: 16,
      userId: 'test@test.com',
      name: '김예슬',
      __typename: 'Interpreter'
    },
    __typename: 'InterpreterAuthPayload'
  }
  token:  {
    name: undefined,
    email: undefined,
    picture: undefined,
    sub: undefined
  }
  ```
- 로그인 후 session에 접근할 때 호출되는 jwt callback에는 user는 들어오지 않고 token에만 값이 들어온다. 이 때의 값은 위와 좀 다르다.
  ```ts
  // 예시
  user:  undefined
  token:  {
    iat: 1680918091,
    exp: 1683510091,
    jti: '2f714992-65c3-4e97-a191-0f1cb9a5fb1a'
  }
  ```

### session

```ts
session: (params: { session: Session; user: User | AdapterUser; token: JWT }) =>
  Awaitable<Session>
```

- session이 확인될 때마다 호출된다
- (callback)말고 session에 접근하면 보안을 위해 token의 일부만 반환한다. token에 담긴 다른 정보도 (혹은 jwt callback에서 token에 추가한 값도) session 안에서 확인하고 싶다면 session callback 안에서 token의 값을 명시적으로 전달해줘야한다.
- (jwt 인증방식을 사용하면) token의 페이로드가 token 인자를 통해 전달된다. (=jwt callback에서의 token과 동일한 값)
- oauth로 로그인 시 jwt의 user 와 동일한 값이 session.user로 들어온다.
- 하지만 credential로 로그인시 전달되지 않는다.

> callback 순서는 꼭 jwt > session 순!

## 내장 API

### getCsrfToken

앞서, next auth에서 발행한 token은 **쿠키**에 저장된다고 했다
next의 server를 사용하면 token이 알아서 보내지기도 하고,
getToken을 통해 쉽게 token에 접근할 수도 있다. (알아서 decode, encode도 해줌!)

쿠키는 서버 통신시 자동으로 보내지니 아주 편리하지만,
반대로 자동으로 보내져서 보안에 위험하기도 하다 (CSRF 공격!)

그렇다면 getCsrfToken 메소드는 뭘까

## protect page

방법으로는

1. next auth middleware 사용
   - config 객체를 통해 보호할 페이지를 설정해주면 next auth가 알아서 해줌
   - jwt 인증방식을 사용할 때만 가능
2. getServerSession 메소드 사용
   - 보호할 모든 페이지의 getServerSideProps 메소드 안에서 getServerSession 메소드를 통해 session을 넘겨주면 보호가 된단다.. 🧐

## silent refresh

### 토큰 만료 전에 로그인 연장

- access token이 만료되기 전에 보다 유효기간이 긴 refresh token을 통해, 사용자에게 다시 로그인 요청없이 새로운 access token과 refresh token을 받아오는 인증 플로우
- 이 기능은 next auth에서 제공하는 refreshInterval 옵션을 통해 구현에 도움을 받을 수 있는데,
- refreshInterval 기능은 지정한 시간(초)에 한 번씩 jwt, session callback을 호출시킨다
- 즉 refreshInterval 을 통해 토큰이 만료되기 전에 jwt callback이 다시 호출되기 때문에, jwt callback에 silent refresh 기능을 넣어주면! 기능 완성이다 🙂
- 만료기한이 넘으면 더이상 콜백은 호출되지 않는다

### 토큰이 만료된 이후 refresh -> api 재요청

혹시

---

## 참고글

- [next-auth 공식문서](https://next-auth.js.org/)

## 메모

그래서 내가 뭘했고
뭘 해야 하는지
중간에 발생한, 잊지 말고 해결해야할 이슈는 뭔지

- [ ] issue: 회원가입하지 않고 로그인을 하려는 사용자의 에러처리
      소셜로그인시 profile 콜백함수에서 token을 받아오기 때문에
      소셜회원가입을 하지 않고 소셜 로그인을 하는 등
      profile 함수 내에서 에러가 발생하면 에러 핸들링이 어려움
      에러가 클라이언트까지 오지 않고 서버에서 그대로 에러가 반환되기 때문에
- 대안 1.
  [loginType, userId, password] 가 unique하게 두고
  회원가입/로그인시 해당 데이터를 upsert하고 로그인시킴
  => 회원가입을 진행하지 않고 로그인할 경우 자동 회원가입이 되어버림 (이게 맞나?)
- 대안 2. 회원가입, 로그인은 분리하되 앞 단에서 에러핸들링할 수 있는 방법을 찾는다.
  => **signin 콜백이 바로 그런 용도이다**
  > Use the signIn() callback to control if a user is allowed to sign in.
  - signin 콜백 안에서 false를 리턴하면 앞단에서 에러로 감지된다
  - 설정한 에러페이지로 이동하며, 에러 메세지는 'AccessDenied'
- [ ] issue: 그 외의 에러처리
      그 외의 발생할 수 있는 에러는 없을까?
      우선 에러 발생시 특정 페이지 (커스텀 페이지)로 이동시킬 수는 있다
      그리고 그 페이지에서 error 파라미터를 전달받을 수 있다 (`?error=<errorcode>`)
  ```ts
  //[...nextauth].ts
  pages: {
    error: '/auth/error'
  }
  ```
- [ ] access token 확인하는 로직 수정 필요 (role에 따라서)

---

how to refresh jwt

- https://dev.to/mabaranowski/nextjs-authentication-jwt-refresh-token-rotation-with-nextauthjs-5696
- https://github.com/nextauthjs/next-auth/discussions/4229
- https://next-auth.js.org/tutorials/securing-pages-and-api-routes#server-side
