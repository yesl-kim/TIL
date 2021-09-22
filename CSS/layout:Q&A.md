> instagram clone coding

## Question

기본 위치에 위치하다가 스크롤시 고정된 위치를 갖는 사이드 메뉴

0. { display : flex; justify-content : center }

- 메인 피드 div와 사이드 메뉴 div를 감싸는 부모 요소에 { display : flex } 설정을 하여 두 자식요소를 전체 브라우저 화면에 수평 가운데 정렬

- 스크롤 시 사이드 영역이 고정되지 않아서 문제

1. position: fixed
   스크롤 시 고정된 위치값을 갖지만,
   메인 피드 영역의 위치값에 따라 좌우 위치값은 가변값을 가져야 하는데 좌우 위치값도 고정값을 가져서 문제

2. position : sticky
   무슨 이유에서인지 적용이 되지 않고
   높이값을 지정하지 않았음에도 불구하고 높이값이 자식요소들이 높이값의 합이 아닌 100vh가 적용된 모습이었다.

```css
.main-right {
  position: sticky;
  top: 84px;
}
```

3. 부모요소에 높이값을 0으로 지정

```css
.main-right {
  position: sticky;
  top: 84px;
}
```

자식 요소는 높이값을 가지고 있고 position: sticky 값을 가진 부모요소에 height : 0을 설정했더니 자식요소를 가리지 않고, 스크롤 시 올바른 고정값을 지니게 되었다.

~~하지만 이게 왜 적용되는건지는 이해가 안감..~~
sticky는 부모요소의 영역 안에서만 고정된 위치값을 갖는 것이기 때문에, 부모요소보다 높이값이 크거나 같을 경우 고정됐는지 안됐는지 확인이 안될뿐이다.

---

## position : sticky

- 하나의 위치값은 반드시 가져야 한다. (top, left, right, bottom)
- 위치값은 fixed처럼 동작 -> 브라우저 화면 기준 위치값
- 조상 요소 중에 overflow : hidden이 설정되어 있으면 무시된다. (= poition: static 처럼 동작한다.)
- 높이값이 부모요소의 높이값보다 작아야한다. (추가)
