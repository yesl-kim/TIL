# CSSOM : CSS Object Model
javascript로 CSS 제어하기

## 1. 인라인 스타일
> *element*.style.*attribute*
```javascript
document.body.style.backgroundColor // #fff;
```
`.`를 사용하여 DOM으로 접근하는 방식이다. 인라인 스타일에만 접근이 가능하다.

---
<br>

## 2. 외부 스타일 시트
> window.getComputedStyle(*element*, *selector*)
- selector (optional)  
가상 요소 선택자


```css
.box::before{
    display: block;
    content:'';
    width: 50px;
}
```
```javascript
const box = document.getElementByClassName('box');
getComputedStyle(box, '::before').width  // 50px
```

외부 스타일 시트로 정의한 css에 접근할 수 있으며 **가상요소에도 접근이 가능하다.**

하지만 실제 사용자가 정의한 스타일이 아닌 웹 브라우저에서 정의한 초기값들도 같이 반환될 수 있다.

실제 선언된 값이 아닌 **계산된 수치로 반환된다.** 예를 들어 width나 height를 퍼센트 값으로 정의했을 때 자바스크립트에서는 픽셀값으로 계산되어 반환된다.

### 2-1. css 속성값 가져오기
> .getPropertyValue(*attribute*);

```javascript
// 동일한 표현이다.
getComputedStyle(document.body).backgroundColor;
getComputedStyle(document.body).getPropertyValue('background-color');
```

### 2-3. css 속성값 설정하기
> .setProperty(*attribute*, *value*);

### 2-4. css 속성 삭제하기
> .removeProperty(*attribute*);