
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
thisIsPw.addEventListener('keydown', function(e) {
  if (e.keyCode === 13) {
     //로그인 함수로 이동
  }
});
```