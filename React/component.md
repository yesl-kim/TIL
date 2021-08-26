# component: class vs function

함수는 실행 뒤 메모리에서 사라진다. -> state, 라이프사이클을 사용할 수 없다.

클래스를 사용해야만 state와 라이프사이클을 사용할 수 있었다.

하지만 클래스를 사용했을 때 나타나는 여러 문제점들과 버그들이 존재했다.

hook이 등장하면서 함수형 컴포넌트에서도 상태관리와 컴포넌트의 생명주기를 관리할 수 있게 되었고, class를 사용하면서 겪는 여러 문제들을 거의 해결할 수 있게 됨 (거의 this와 관련된 문제들)

또한 생명주기에 따라 함수가 묶이는 것이 아닌 (componentDidMound, componentsDidUpdate 등등), 관심사에 따라 함수를 분리하고 로직을 분리할 수 있게 되었다.

> 어떻게 함수 컴포넌트 안에서 state와 라이프사이클을 사용할 수 있었나?  
> 클로저!

### 기본 hooks

- useState
- useEffect
- useContext
