> [poiemaweb | 5.26 정규표현식](https://poiemaweb.com/js-regexp)  
> [zvon | Regular Expression Tutorial](http://zvon.org/comp/r/tut-Regexp.html#Pages~Contents)  
> [MDN | 정규표현식을 사용하는 메소드 6개](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/split)

## 정규표현식 개념

![정규표현식 리터럴](https://poiemaweb.com/img/regular_expression.png)
정규표현식은 문자열을 처리하는 방법 중 하나로, 문자열에서 특정 내용을 찾거나 대체 또는 발췌하는데 사용한다.
반복문이나 조건문으로 표현해야하는 복잡한 연산도 정규표현식을 사용하면 보다 간단하게 표현할 수 있다.
정규표현식은 리터럴 표기법으로 생성할 수 있으며 리터럴 표기법의 형식은 위의 그림과 같다.

### 플래그

플래그는 정규표현식을 어떻게 검색할 것인지를 지정하는 옵션사항이다.
플래그를 지정하지 않을 경우 타깃 문자열 내에서 검색하려는 문자는 대소문자를 구분하여 검색하게 되며,
검색 문자와 매칭되는 대상이 1개 이상이더라도 첫 번째 매칭한 대상만을 반환하고 검색을 종료한다.

| Flag |   Meaning    |                Description                |
| :--: | :----------: | :---------------------------------------: |
|  i   | Imgnore Case |    대소문자를 구분하지 않고 검색한다.     |
|  g   |    Global    |     문자열 내의 모든 패턴을 검색한다.     |
|  m   |  Multi Line  | 문자열의 행이 바뀌더라도 검색을 계속한다. |

```
const targetStr = 'Is this all there is?';

// 문자열 내의 is를 대소문자 구별하여 한 번만 검색
let regExp = /is/;
targetStr.match(regExp) // ["is", index: 5, input: "Is this all there is?", groups: undefined]

// 문자열 내의 is를 대소문자 "구별하지 않고 모두" 검색
regExp = /is/ig
targetStr.match(regExp) //  (3) ["Is", "is", "is"]
```

### 패턴

패턴에는 검색하고 싶은 문자열을 지정한다.

|           표현식           |                                       의미                                       |
| :------------------------: | :------------------------------------------------------------------------------: |
|            `.`             | 문자, 숫자, 기호, 공백 등을 포함한 모든 문자 <br> 문자 자릿수를 지정할 때도 사용 |
|            `^`x            |                               x로 시작하는 문자열                                |
|         `[^`xyz`]`         |     not. 부정을 의미 <br> x, y, z를 제외한 모든 문자 (숫자, 문자, 기호 포함)     |
|            x`$`            |                                x로 끝나는 문자열                                 |
|            x`?`            |                             x가 없거나 하나인 문자열                             |
|            x`*`            |                      x가 없거나 한 번 이상 반복되는 문자열                       |
|            x`+`            |                          x가 한 번이상 반복되는 문자열                           |
|           x`{n}`           |                             x가 n번 반복되는 문자열                              |
|          x`{n,}`           |                           x가 n번 이상 반복되는 문자열                           |
|         x`{n, m}`          |                   앞문자 x가 n번 이상 m번 이하 반복되는 문자열                   |
| x`버티컬바`y <br> `[`xy`]` |                                 OR <br> x 또는 y                                 |
|        `[`x`-`y`]`         |                         범위지정 <br> x ~ y 까지의 문자                          |
|            `()`            |                         그룹지정 <br> 서브패턴으로 간주                          |
|            `\d`            |                                       숫자                                       |
|            `\D`            |                                  숫자가 아닌 것                                  |
|            `\w`            |                  word를 의미 <br> 알파벳, 숫자, \_ 중의 한 문자                  |
|            `\W`            |                      알파벳, 숫자, \_를 제외한 나머지 문자                       |
|            `\s`            |                                    공백 문자                                     |
|            `\S`            |                               공백 문자가 아닌 것                                |

- 이스케이핑(escaping)  
  위와 같은 정규식에서 특정 의미를 지닌 특수 문자를 검색하고 싶은 문자로서 패턴에 지정하고 싶다면 백슬래시 `\`를 사용한다.
  예를 들어 마침표 `.`가 들어가는 문자를 검색하고 싶을 때 `/./`이 아닌 `/\./`로 패턴을 지정한다.
  이러한 `\`의 역할을 이스케이핑(escaping)이라 한다.

- `[]`로 묶인 문자는 OR로 연결된 하나의 문자로 인식한다.

```javascript
// 문자의 위치를 지정하는 ^ 문자
const tgStrs = [
  "who is who",
  "who is it",
  "it is who",
  "when who what",
  "what is it",
];
const regExp1 = /who/g;
const regExp2 = /^who/g;

// ^를 쓰지 않았을 경우
const passedStrs1 = tgStrs.filter((tg) => regExp1.test(tg));
// []
const passedStrs2 = tgStrs.filter((tg) => regExp2.test(tg));

passedStrs1;
```

---

## JS에서 정규식 생성방식

리터럴 표기법과 생성자, 두가지 방식을 통해 정규표현식 객체를 만들 수 있다.

> var regExp = /pattern/flags
> var regExp = new RegExp( pattern[, flags])

```javascript
// 정규식 리터럴
/ab+c/i;

new RegExp("ab+c", "i");
new RegExp(/ab+c/, "i");
new RegExp(/ab+c/i); // ES6
```

---

## 정규표현식을 사용하는 자바스크립트 메소드

### RegExp.exec()

> RegExp.exec( target : string ) : RegExpExecArray | null

정규식에 매칭되는 문자열을 반환하며 **전역 플래그(`g`)를 지정하여도 첫번째 매칭 대상만 반환한다.**

```javascript
const target = "Is this all there is?";
const regExp1 = /is/;
const regExp2 = /is/g;

regExp1.exec(target); // ['is', index: 5, ...]
regExp2.exec(target); // ['is', index: 5, ...]
```

### RegExp.test()

> RegExp.test( target : string ) : boolean

문자열을 검색하여 일치하는 부분이 있으면 `true`, 없으면 `false`를 반환한다.

```javascript
const target = "Is this all there is?";
const regExp = /is/;

regExp.test(target); // true
```

### String.match()

> String.match( regExp ) : Array | null

정규식에 매칭되는 문자열을 배열에 담아 반환한다. 전역 플래그를 사용하면 일치되는 모든 항목을 반환한다.

전역 플래그를 포함하지 않았을 경우, 특정 문자열을 캡쳐할 수 있는데 캡쳐된 문자열은 `groups` 속성의 객체 형태로 저장된다.
캡쳐할 문자열은 `(?<name>캡쳐할 문자열)`형태로 지정할 수 있다.

```javascript
const str = "The sheep sleeps, and the dog barks";
let regExp = /(?<animal>sheep|dog) barks/;

console.log(str.match(regExp));
/* 
(2) ['dog barks', 'dog']
0: 'dog barks',
1: 'dog',
groups: {animal: 'dog'}
index: 26,
input: 'The sheep sleeps, and the dog barks'
*/
```

### String.replace()

> String.replace( regExp : regExp | string, newSubstr : string) : string

매칭되는 문자열을 주어진 문자열로 교체한다.

### String.search()

> String.search( regExp )

매칭되는 문자열이 있는지 검사한다. 있을 경우 첫 번째 매칭 대상의 위치값(인덱스)을, 없을 경우 -1을 반환한다.

### String.split()

> String.split( [separator : string | regExp[, limit : number]] ) : Array

매칭되는 문자열(separator)을 기준으로 현재 문자열을 분할한다.

- limit  
  반환되는 배열에 포함시킬 항목의 개수를 제한할 수 있다.
  음이 아닌 정수로 전달한다.

## 정규표현식 관련 유용한 사이트

[regexp.com](https://regexr.com/) 작성한 정규표현식의 매칭 결과를 실시간으로 확인해볼 수 있다.
[regexper.com](https://regexper.com/) 작성한 정규표현식을 시각화하여 보여준다.
[regular-expressions.info](http://www.regular-expressions.info/) 정규표현식 자습서. (영문이라 보기 어렵지만 가끔 꺼내보자)
