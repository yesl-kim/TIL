# React?

> 사용자 인터페이스를 만들기 위한 **자바스크립트 라이브러리**

MVC Architecture(vue.js, Angular)와는 다르게 리액트는 view만 담당한다. 그만큼 내장되어 있는 기능이 부족해 third-party 라이브러리(ex. React-router, Redux)를 함께 사용한다.

## JSX

> Javascript의 확장버전

JS에 html의 마크업이 합쳐진 형태로 리액트 프로젝트에서 **UI를 표현하기 위해 사용되는 문법**이다. javascript 파일 내에서 작성할 수 있지만 js 문법은 아니기 때문에 javascript 파일에서는 읽히지 못하고 별도의 컴파일 과정이 필요하다.
<br/>

- **속성값을 추가할때는 꼭 쌍따옴표**로 감싸주어야 한다.
  속성이름은 html과 다를 수 있기 때문에 react.js 공식문서를 참고한다.

```js
const item = <li className="list-item">programming</li>;
```

<br/>

- **자바스크립트 표현과 inline style 은 `{}`로 묶어준다.**

```js
<div style={{color : "red"}}>Hello React</div>
<div onClick={() => console.log('clicked')} />
```

<br/>

- **Nested JSX**
  첫 태그는 반드시 하나의 태그로 시작해야 되며 태그를 감싸는 태그는 항상 **소괄호**로 감싼다.
  fragment tag `<></>`로 감싸게 되면 불필요한 부모태그를 추가해야하는 점을 해결할 수 있다. fragment tag는 html에서 태그로 인식하지 않는다 (아무런 태그도 추가되지 않는다)

```jsx
// wrong
const wrong = (
<p>list1</p>
<p>list2</p>
);

// right
const right = (
<div>
    <p>list1</p>
    <p>list2</p>
</div>
);
```

---

## component

> 재사용 가능한 ui 단위

- **컴포넌트 정의 : `function` vs `class`**  
  컴포넌트를 정의하는 데는 함수를 사용하는 것과 `class`를 사용하는 것, 두가지 방법이 있다.
  `class`로 컴포넌트를 정의할 때는 꼭 `render()` 함수가 필요하고, 그 안에서 화면에 그려줄 부분을 return한다.

```js
// 함수로 컴포넌트 구현하기
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}

// class로 구현하기
class Welcome extends React.Component {
  render() {
    return <h1>Hello, {this.props.name}</h1>;
  }
}
```

- **컴포넌트의 속성값 가져오기 : `props`**  
  컴포넌트의 속성값은 `컴포넌트.props`에 객체형태 `{ 속성명: 속성값 }`로 저장된다. 함수로 컴포넌트를 정의할 때 `props`를 매개변수로 전달하고 `props.속성명` 형태로 속성값에 접근할 수 있다.

```js
// function
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}
// class
class Welcome extends React.Component {
  render() {
    return <h1>Hello, {this.props.name}</h1>;
  }
}

const element = <Welcome name="Sara" />;
ReactDOM.render(element, document.getElementById("root"));
```

- **컴포넌트 내에서 데이터 제어하기 : `state`**  
  `props`는 컴포넌트를 사용하는 부모태그에서 그 값을 받아와 사용하지만 `state`는 온전히 컴포넌트 내에서 관리하고 제어하기 때문에 재사용성을 높일 수 있고, 보다 비공개적인 정보를 담을 때 사용한다.

---

## Lifecycle

> 컴포넌트 생성 -> 업데이트 -> 제거

![컴포넌트의 생명주기](https://media.vlpt.us/images/jeanbaek/post/60e94ed2-4a82-4a04-9d9d-03d841e44bcb/ogimage.png)

컴포넌트는 생성, 업데이터, 제거의 생명주기를 가진다. 그리고 이러한 생명주기 단계에서 실행할 명령을 지정할 수 있는 여러 **생명주기 메소드**를 가진다.

**1. 컴포넌트 생성 단계의 메소드**

- `constructor()`
  컴포넌트의 속성값을 초기화하고 컴포넌트 내부에서 지정한다.

- `render()`
  실제 화면에 그릴 컴포넌트를 지정한다.

- `componentDidMount()`
  렌더 후 마운팅이 진행되는데 이때 실행시킬 명령을 지정한다.
  컴포넌트 출력물이 DOM에 렌더된 후 실행시킬 명령을 지정한다.

**2. 컴포넌트 업데이트 단계의 메소드**
컴포넌트의 props 또는 state가 변경되면 업데이트가 발생하고 렌더 함수가 다시 호출된다. 아래 메소드는 컴포넌트가 다시 렌더링될 때 순서대로 호출된다.

- `render()`
- `componentDidUpdate``

**3. 컴포넌트 제거 단계의 메소드**

- `componentWillUnmount`
  컴포넌트가 DOM에서 제거되기 직전에 호출된다. 이 메소드가 호출된 이후에 해당 컴포넌트는 다시 렌더링 되지 않으므로 메소드 내에서 setState()를 호출하면 안된다.

---

[리액트 공식문서](https://ko.reactjs.org/docs/state-and-lifecycle.html)
