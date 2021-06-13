### Event Handling

React에서는 React element에 onClick 이벤트를 추가해서 event handler 함수를 넘겨주겠습니다.

```js
class Button extends React.Component {
  constructor() {
    super();

    this.state = {
      clicked: false,
    };
  }

  render() {
    return (
      <div
        className={`btn ${this.props.type === "like" ? "like-btn" : ""}`}
        onClick={() => {
          this.setState({ clicked: !this.state.clicked });
        }}
      >
        {this.state.clicked ? "좋아요" : "싫어요"}
      </div>
    );
  }
}

ReactDOM.render(<Button type="like" />, document.getElementById("root"));
```

```js
class Button extends React.Component {
  constructor() {
    super();

    this.state = {
      clicked: false,
    };

    this.handleClick = this.handleClick.bind(this);
  }

  handleClick() {
    this.setState({
      clicked: !this.state.clicked,
    });
  }

  render() {
    return (
      <div
        className={`btn ${this.props.type === "like" ? "like-btn" : ""}`}
        onClick={this.handleClick}
      >
        {this.state.clicked ? "좋아요" : "싫어요"}
      </div>
    );
  }
}

ReactDOM.render(<Button type="like" />, document.getElementById("root"));
```

#### bind()

> _function_.bind(_this_) : function
> 함수 안에서 `this`키워드로 사용될 요소를 매개변수로 받아 지정해주는 함수
> 함수 `bind`의 인자로 전달된 요소는 함수 내에서 `this`키워드로 호출된다.
> bind는 `this`의 키워드가 정의된 함수를 반환한다.

```js
const obj = { name: kims };
function bindTest() {
  return this.name;
}
const bindTest2 = bindTest.bind(obj);

bindTest(); // undefined
bindTest2(); // kims
```

---

- [x] 노션 react intro
- [x] 생활코딩 - react
- [ ] 생활코딩 - npm1
- [ ] npx create-react-app
- [ ] 드림코딩엘리 - class
- [ ] poiemaweb - this > bind
- [x] react - fragment

---

이해안감

### lifecycle

생성한 timer는 this.timerId에 저장합니다. setInterval **함수를 호출해서 timer를 생성하면, 해당 timer 의 id를 리턴**하기 때문에 저장했습니다.
