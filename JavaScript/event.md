이벤트의 종류

- contextmenu
- mouseout
- pointerlockchange
- pointerlockerror
- select
- wheel

## key event

enter키의 key code는 13입니다. 아래와 같이 코드를 짤 수 있습니다.

```js
thisIsPw.addEventListener("keydown", function (e) {
  if (e.keyCode === 13) {
    //로그인 함수로 이동
  }
});
```

## event.target vs event.currentTarget

- `target`  
  부모로부터 이벤트가 위임되어 이벤트를 발생시키는 자식 요소  
  가령, 부모에 클릭이벤트를 달았을 때 이벤트 타깃은 내가 '클릭한 자식요소'

- `currentTarget`  
  실제 이벤트를 부착한 요소
