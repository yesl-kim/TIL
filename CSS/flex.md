## flex container 속성

### flex-wrap

> flex-wrap : nowrap(deflaut) | wrap | nowrap-reverse | wrap-reverse

기본값은 nowrap

- nowrap  
  자식 요소가 1행으로 표현된다.
  부모의 너비가 줄어들면 자식요소의 너비도 같이 줄어든다.
  복수의 자식 요소 너비의 합이 부모 요소의 너비보다 커질 경우 부모 요소 밖으로 넘치게 된다. 이때 부모 요소에 {overflow : auto} 설정을 해주면 넘치는 자식요소가 보이지 않고 가로 스크롤이 나타난다.

- wrap
  여러개의 자식 요소 너비의 합이 부모 요소 너비보다 커질 경우 다음 자식요소들은 줄바꿈되어 나타난다.

### flex-flow

> flex-flow : _flex-direction_ _flex-wrap_

- default : row nowrap
