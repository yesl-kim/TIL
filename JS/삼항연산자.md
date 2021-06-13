참고
[MDN | 삼항 조건 연산자](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Operators/Conditional_Operator).

---

if / else 문을 대체하는 삼항연산자가 return을 여러 번 사용하고 if 블록 내부에서 한줄만 나올때 return을 대체 할 수 있는 좋은 방법이 된다.

삼항연산자를 여러 행으로 나누고 그 앞에 공백을 사용하면 긴 if / else 문을 매우 깔끔하게 만들 수 있습니다. 이것은 동일한 로직을 표현하지만 코드를 읽기 쉽게 만들어 준다.

```js
var func1 = function( .. ) {
  if (condition1) { return value1 }
  else if (condition2) { return value2 }
  else if (condition3) { return value3 }
  else { return value4 }
}

var func2 = function( .. ) {
  return condition1 ? value1
       : condition2 ? value2
       : condition3 ? value3
       :              value4
}
```
