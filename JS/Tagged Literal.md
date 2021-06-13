# Tagged Literal

> [MDN | Template literals](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Template_literals#expression_interpolation%ED%91%9C%ED%98%84%EC%8B%9D_%EC%82%BD%EC%9E%85%EB%B2%95).

태그를 사용하면 템플릿 리터럴을 함수로 파싱할 수 있다.

```js
var person = "mike";
var age = 28;

function myTag(strings, personExp, ageExp) {
  var str0 = strings[0];
  var str1 = strings[1];

  var ageStr;
  if (ageExp > 99) {
    ageStr = "centenarian";
  } else {
    ageStr = "youngster";
  }

  return str0 + personExp + str1 + ageStr;
}

var output = myTag`that ${person} is a ${age}`;

console.log(output);
// that mike is a youngster
```

템플릿 리터럴을 함수의 인자로 넘겨주면, `${}`로 구분된 문자열이 담긴 배열이 함수의 첫 번째 인자로 전달이 되고, 그 뒤로 `${}`로 전달된 변수가 순서대로 인자로 전달된다.

**Tag function 이 string을 반환할 필요는 없다.**

```js
function template(strings, ...keys) {
  return (function)
}
```
