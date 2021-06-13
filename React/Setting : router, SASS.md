## SPA
> Single Page Application

## Routing
> 하나의 html 에 조건에 따라 다른 컴포넌트를 렌더링 하는 것
> 다른 경로에 따라 다른 View를 보여주는 것

1. setting
> npm 


## 페이지 이동

- link tag  vs a tag
프로젝트 내부에서는 Link 태그, 프로젝트 외부 페이지로 이동할 떄는 a 태그 사용

link 태그는 페이지로 이동하지만 새로고침을 하지 않는다
즉 화면에 보여지는 엘리먼트와 모든 데이터를 다시 렌더링하지 않는다.
스타일링할 때 선택자는 a 태그를 사용하거나 클래스명을 사용

a 태그는 라우터를 사용하더라도 렌더링을 다시 하기 때문에 라우터를 사용하는 의도와 맞지 않는다.

- withRouter
함수
인풋으로 컴포넌트를 받고,
아웃풋으로 인자로 받은 컴포넌트에 페이지 이동 기능을 추가한 컴포넌트를 반환

```
export default withRouter(Login);
```

- 차이점
link 태그는 페이지 이동에 조건을 걸 수가 없다.
조건을 걸어야 하는 (로그인같이) 페이지 이동에는 withRouter를 사용

## SASS
convention : 컴포넌트의 이름과 같은 클래스명 주기
fragment 에 클래스명을 주어야할 땐
```
<>
</>

<React.fragment className="">
</React.fragment>
```

- [ ] Sass 홈페이지 훑어보기

