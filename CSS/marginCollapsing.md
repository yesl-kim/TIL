# 여백 상쇄 현상

> [MDN | 여백 상쇄](https://developer.mozilla.org/ko/docs/Web/CSS/CSS_Box_Model/Mastering_margin_collapsing)

형제 요소 간의 상하 마진 값이 상쇄되는 현상을 말한다.
부모 요소에 border, padding 등 특정요소가 없거나 부모요소의 display 속성이 inline이 아닌 경우,
맏이와 막내 요소 각각의 margin-top, margin-bottom 값은 상쇄된다. (적용이 되지 않는다.)

이런 경우 우회하는 정확한 방법은 아직 찾지 못했지만,  
부모 요소에 border, padding, display: inline 속성을 추가함으로써 해결할 수 있다.

```css
// border
.parent {
  border: 1px solid transparent;
}

// padding
.parent {
  padding: 1px;
}

// 이런식으로...
```
