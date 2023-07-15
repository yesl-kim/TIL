# this : Q&A

> [노마드 코더 | 바닐라 JS로 그림판 만들기](https://nomadcoders.co/)
>
> [poiemaweb | 5.17 this](https://poiemaweb.com/js-this)
>
> [poiemaweb | 6.3 Arrow function](https://poiemaweb.com/es6-arrow-function)

## 1. 상황

유사 배열 객체에 담긴 여러개의 `div.controls_color`에 `click` 이벤트가 발생했을 때 클릭된 div의 스타일 속성(인라인 스타일)을 가져오는 함수를 작성한다.

```javascript
const colors = document.getElementsByClass("controls_color");
let _COLOR;

for (let color of colors) {
  color.addEventListener("click", () => {
    _COLOR = this.style.backgroundColor;
  });
}
```

## 2. 문제

`console.log`로 찍어보면 콜백 함수의 `this`는 클릭된 요소를 받아오는 것이 아니라 window 객체를 호출한다.

클릭된 요소를 `this`로 받아오는 것은 예전에 사용했던 방법으로, 그땐 문제가 없었는데 이번엔 문제가 되어서 의아했다. 이유는 자바스크립트의 경우 **함수 호출 방식에 따라 `this`가 가리키는 객체가 달라지기** 때문이다.

## 3. 원인

### 1). `function` 키워드로 선언한 함수와 화살표 함수의 `this`는 다른 방식으로 바인딩된다.

`addEventListener` 함수의 콜백 함수 내에서 `this`를 사용하는 경우, `function` 키워드로 정의한 일반 함수의 `this`는 이벤트 리스너에 바인딩된 요소(`currentTarget`)을 가리키는 반면, 화살표 함수의 `this`는 상위 컨택스트인 `window`를 가리킨다.

## 4. 해결책

### 1) `addEventListener`의 콜백 함수를 일반 함수로 정의한다.

```javascript
for (let color of colors) {
  color.addEventListener("click", function () {
    _COLOR = this.style.backgroundColor;
  });
}
```

### 2) `this` 대신 `event.target`을 사용한다.

```javascript
for (let color of colors) {
  color.addEventListener("click", (event) => {
    _COLOR = event.target.style.backgroundColor;
  });
}
```

콜백 함수를 화살표 함수로 정의하더라도 `event`를 매개변수로 받아 `event.target`을 사용하면 별다른 문제없이 클릭된 요소를 받아올 수 있다. 하지만 혼란을 불러올 수 있기 때문에 `addEventListener` 함수의 콜백함수로 화살표 함수를 쓰는 것은 피하는 것이 좋겠다.
