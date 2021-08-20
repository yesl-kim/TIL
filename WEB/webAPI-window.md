# Web API > window

- **pageYOffset**

  읽기전용속성  
  문서가 상단에서부터 얼마나 스크롤되었는지 픽셀값을 반환

  `scrollY`와 동일하지만, 일부 오래된 브라우저에서는 `pageYOffset`만 지원

  항상 정수값을 반환하지 않기 떄문에 Math.around와 함께 사용하는 것을 권장

  > 여기서 잠깐,  
  > `window.pageYOffset` vs `document.documetElement.scrollTop` vs `document.body.scrollTop`?  
  > IE9 미만에서도 지원하는 속성이 `document.documetElement.scrollTop`, `document.body.scrollTop`이다.  
  > **브라우저 호환성**  
  > `document.body.scrollTop` > `document.documetElement.scrollTop` > `window.pageYOffset` > `window.scrollY`  
  > 이 셋을 Math.max와 함께 사용할 수 있다.
