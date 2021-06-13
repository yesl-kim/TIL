# 1. 타입 변환

> [poiemaweb | 5.9 타입 변환과 단축 평가](https://poiemaweb.com/js-type-coercion)

## 1. 타입 변환이란?

- **암묵적 타입 변환**(implicit coercion) 또는 **타입 강제 변환**(Type coercion)  
  개발자의 의도와는 상관없이 자바스크립트 엔진에 의해 암묵적으로 타입이 자동 변환되는 것  
  자바스크립트 엔진은 <u>문맥</u>에 따라 데이터의 타입을 암묵적 변환한다.
  <br><br/>
- **명시적 타입 변환**(Explicit coercion) 또는 **타입 캐스팅**(Type casting)  
  개발자에 의해 의도적으로 값의 타입을 변환하는 것
  <br/>

---

<br/>

## 2. 암묵적 타입 변환

### 2-1. 문자열 타입으로 변환

- 문자열 연결 연산자 `+`

<br>

`+` 연산자를 문자열 연결 연산자로 인식하여 피연산자 중에서 문자열 타입이 아닌 피연산자를 문자열 타입으로 변환한다.

```javascript
1 + '2' // '12'

// 객체 타입
({}) + '' // '[object object]'
Math + '' // '[object Math]'
[] + '' // ''
[10, 20] + '' // '10,20'
(function(){}) + '' // "function(){}"
Array + '' // "function Array() { [native code] }"
```

<br>

### 2-2. 숫자 타입으로 변환

- 산술 연산자 `-` , `*` , `/`
- 비교 연산자 `>` , `<`
- 단항 연산자 `+`

<br>

단항 연산자 `+` 가 숫자 타입이 아닌 피연산자를 만났을 경우, 빈 문자열 `''`, 빈 배열 `[]`, `null`, `false`는 0으로, `true`는 1로 변환한다. 객체와 빈 배열이 아닌 배열, 빈 문자열이 아닌 문자열, undefined는 변환되지 않아 NaN이 된다.

```javascript
1 - "1"; // 0
1 * "10"; // 10
1 / "2"; // 0.5

1 >
  "0" + // true
    "" + // 0
    [] + // 0
    null + // 0
    false + // 0
    true + // 1
    {} + // NaN
    [1, 2] + // NaN
    undefined; // NaN
```

<br>

### 2-3. 불리언 타입으로 변환

- if 문이나 for문과 같은 제어문의 조건식

- false로 취급되는 falsy한 값
  - false
  - undefined
  - null
  - 0, -0
  - NaN
  - '' (빈 문자열)

---

<br>

## 3. 명시적 타입 변환

### 3-1. 문자열 타입으로 변환

<br>

1. String 생성자 함수를 new 연산자 없이 호출하는 방법
   > String(value);

```Javascript
String(1)  // '1'
String(NaN)  // 'NaN'
String(true)  // 'true'
```

2. Object.prototype.toString 메소드를 사용하는 방법
   > value.toString();

```javascript
(1)
  .toString()(
    // '1'
    NaN
  )
  .toString()(
    // 'NaN'
    true
  )
  .toString(); // 'true'
```

3. 문자열 연결 연산자를 이용하는 방법
   > value + ''

```javascript
1 + ""; // '1'
NaN + ""; // 'NaN'
true + ""; // 'true'
```

<br>

### 3-2. 숫자 타입으로 변환

<br>

1. Number 생성자 함수를 new 연산자 없이 호출하는 방법
   > Number(value)

```javascript
Number("0"); // 0
Number(true); // 1
Number(false); // 0
```

2. parseInt, parseFloat 함수를 사용하는 방법
   > parseInt( string, radix );

- 특정 진수의 **정수**를 반환하는 함수
- radix  
   수의 진법 체계에 기준이 되는 값
  2와 36사이의 수

> parseFloat(value);

- 문자열을 **실수**로 반환하는 함수
- 첫 번째 문자를 숫자로 반환할 수 없는 경우 `NaN`을 반환

```javascript
parseInt("0"); // 0
parseFloat("10.53"); // 10.53
```

3. 단항 연결 연산자를 이용하는 방법
   > `+` value

```javascript
+"0" + // 0
  "10.53" + // 10.53
  true; // 1
```

4. `*` 산술 연산자를 이용하는 방법
   > value `*` 1

```javascript
"0" * 1; // 0
"10.53" * 1; // 10.53
true * 1; // 1
false * 1; // 0
```

<br>

### 3-3. 불리언 타입으로 변환

<br>

1. Boolean 생성자 함수를 new 연산자 없이 호출하는 방법
   > Boolean(value);

```javascript
Boolean(""); // false
```

2. ! 부정 논리 연산자를 두번 사용하는 방법
   > `!!` value

```javascript
!!"x"; // true
!!""; // false
```

---

<br>

## 4. 단축 평가

<br>

논리곱 연산자 `&&` 와 논리합 연산자 `||` 는 **논리 평가를 결정한 피연산자의 평가 결과를 그대로 반환한다.** 이를 **단축 평가**라고 부른다.
<br>

| 단축 평가 표현식   | 평가 결과 |
| ------------------ | --------- |
| true OR anything   | true      |
| false OR anything  | anything  |
| true AND anything  | anything  |
| false AND anything | false     |

<br>

```javascript
"Cat" && "Dog"; // 'Dog'
"Cat" || "Dog"; // 'Cat'
```

### 4-1. 단축 평가 사용 사례

<br>

1. 객체의 프로퍼티를 참조하기 전에 객체가 `null`인지 확인할 때

객체가 `null`인 경우 객체의 프로퍼티를 참조하면 타입 에러가 발생하지만, 단축 평가를 사용하면 객체의 값 (`null`)을 그대로 반환하기 때문에 에러가 발생하지 않는다.

```javascript
var elem = null;

console.log(elem.value); // TypeError: Cannont read property 'value' of null
console.log(elem && elem.value); // null
```

2. 함수의 인수의 기본값을 설정할 때

함수를 호출할 때 인수를 전달하지 않으면 매개변수는 undefined를 갖고 타입 에러를 발생시킨다. 이때 단축 평가를 사용하여 매개변수의 기본값을 설정하면 undefined로 인해 발생할 수 있는 에러를 방지할 수 있다.

```javascript
function getStringLenth_error(str) {
  return str.length;
}

//매개변수의 기본값 설정
//단축평가 사용
function getStringLength(str) {
  str = str || "";
  return str.length;
}
//ES6
function getStringLength(str = "") {
  return str.length;
}

getStringLength_error(); // TypeError: Cannot read ... property 'length' ...
getStringLength(); //0
getStringLength("hi"); // 2
```
