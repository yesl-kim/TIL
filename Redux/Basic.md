# Core Concept

> 김민준, <리액트를 다루는 기술>, 길벗

```
action -> reducer -> setState (state값을 반환)
```

## action

하나의 객체 형태  
type 필드를 반드시 가지고 있어야 함  
액션이 발생하면 리듀서가 발생하고 리듀서 안에서 정의한대로 상태값이 변화함

- 액션 생성 함수  
  액션을 오류없이 (오타나 실수) 발생시키기 위한 액션 생성자 함수  
  즉, 액션 객체를 반환하는 함수

## reducer

변화를 일으키는 함수  
**< 순수한 함수 >**

- 현재 상태(state)와 전달받은 액션(action) 객체를 파라미터로 받아옴
- 이전의 상태를 절대로 건들이지 않고, 변화를 일으킨 새로운 상태 객체를 만들어서 반환
- **똑같은 인풋이라면 똑같은 아웃풋을 반환해야함**  
  리듀서 함수안에서는 new Date(), Math.random(), 네트워크 요청 등 다른 결과값을 반환하게 하는 로직이 포함되어서는 안됨!

```js
const initialState = {
  counter: 1,
};

reducer = (state = initialState, action) => {
  switch (action.type) {
    case INCREMENT:
      return {
        couter: state.counter + 1,
      };
    default:
      return state;
  }
};
```

## store

현재 애플리케이션의 상태와 리듀서, 여러 내장함수를 담고 있음  
한 개의 프로젝트는 단 하나의 스토어만 가질 수 있음

**< 내장 함수 >**

- getState
  state값을 읽음

- dispatch
  액션을 발생시키는 함수

```js
dispatch(action);
```

- subscribe  
  함수가 업데이트될 때마다 실행되는 함수를 인자로 받음

---

## Summery

1. 리덕스의 기본 동작 순서

- 액션 객체가 만들어지면
- 리듀서가 호출
- 리듀서에서는 액션의 타입에 따라 지정된 함수가 실행되며 새로운 state값을 반환 (혹은 기존의 state를 반환)

2. store 생성
   스토어를 만들 때는 리덕스에서 {createStore}를 불러와 사용  
   리덕스에서 제공하는 createStore함수에 리듀서를 인자로 넘겨주면 스토어를 반환!
   새로 생성된 store를 Provider 컴포넌트의 store prop으로 넘겨준다
   Provider로 기존의 루트 컴포넌트를 감싸주면 컴포넌트에서 스토어 사용가능

3. store 사용  
   생성된 스토어의 내장함수로 상태값을 읽거나 수정하거나 ~~ 리듀서에 지정한 동작을 할 수 있음
