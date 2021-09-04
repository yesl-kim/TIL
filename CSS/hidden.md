# 텍스트만 스크린에서 가리는 방법

> [](https://velog.io/@ursr0706/%EC%9B%B9-%EC%A0%91%EA%B7%BC%EC%84%B1%EC%9D%84-%EA%B3%A0%EB%A0%A4%ED%95%98%EC%97%AC-%ED%85%8D%EC%8A%A4%ED%8A%B8-%EC%88%A8%EA%B8%B0%EA%B8%B0)

간혹 UI가 라벨없이 이미지나 아이콘만 가지고 있는 경우가 있다.  
이미지라면 alt속성을 사용하면 되지만 아이콘만 가지고 있는 경우, 스크린 리더기 사용자를 위한 제목이 필요하다.

웹 접근성을 위한, 화면에서는 보아지 말아야 하는 제목은 어떻게 화면에서 '알맞게' 숨길 수 있을까

**결론은 `clip-path` 속성을 사용하자**

```css
.visually-hidden {
  position: absolute;
  width: 1px;
  height: 1px;
  margin: -1px;
  overflow: hidden;
  clip-path: polygon(0 0, 0 0, 0 0);
}
```
