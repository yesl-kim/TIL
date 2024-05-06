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

## 객체의 형 변환: 원시적 타입으로의 변환

> **형 변환**? 함수나 연산자에 의해 피연산자의 자료형이 변환되는 것

다른 원시형 자료와 마찬가지로 객체 또한 연산 수행 전에 자동 형 변환이 일어난다.

객체의 형 변환은 아래와 같다

1. 논리 평가시 true
2. 문자열을 인자로 받는 함수에서는 문자열로 변환 (alert, template literal, ...)
3. 숫자 연산시 숫자로 반환 (`-`, `*`, `/`, `//`, `Number()`)

```js
const obj = {};
console.log(!!obj); // true
console.log(`${obj}`); // [object Object]
console.log(+obj); // NaN
```

하지만 문자열, 숫자 형태로 변환된다고 한들 실제 원하는 형태의 연산은 불가능하고, 그 형태가 썩 좋은 정보를 반환하지도 않는다.
이 때 객체는 원시형 자료와 다르게 **형 변환이 일어날 때의 값을 지정할 수도 있다.**
~~문자형으로 변환될 때 '[object Object]' 가 아니라 'apple' 따위의 값으로 변환되게끔 지정할 수 있다!~~

객체 형 변환을 지정하는 것은 로그를 찍거나 디버깅 시에 주로 활용된다.

### hint

#### hint란?

> _원시 값의 선호 유형 - MDN_

- 객체가 주어진 연산을 위해 기대되는 자료형
- 어떤 자료형으로 변환할 것이라고 기대되는 값 -> 'string', 'number', 'default' 세 값 중 하나로 설정됨
- 'booleand' 값이 없는 이유는 객체는 논리 연산에서 항상 true 값을 갖기 때문
- default로 설정되는 경우는 해당 표현식에서 문자열도, 숫자도 모두 다뤄질 수 있어 둘 중 하나를 선택하기 애매할 경우 설정된다. (ex. `+`, `==`, ... )
- ~~default와 number 모두 처리되는 과정은 같기 때문에 number, string만 구분해도 된다.~~
- hint는 지극히 **선호** 유형이기 때문에 형 변환 지정 시 hint 대로 지정하지 않아도 된다. (hint가 string이라고 해서 꼭 문자열을 반환하지 않아도 된다.) => **hint 값이 변환 타입을 보장해주는 것은 아니다.**

### 객체 형 변환은 어떻게 이루어지는가

- [Symbol.toPrimitive] 메소드 있으면 hint와 함께 해당 메소드 호출
- 없으면, hint에 따라 toString, valueOf 메소드를 다른 우선순위로 호출
  - string: toString -> valueOf
  - number/default: valueOf -> toString

### 객체의 형 변환 지정하기

#### 1. [Symbol.toPrimitive] 메소드 지정

```js
// Symbol.toPrimitive 속성이 없는 객체.
const obj1 = {};
console.log(+obj1); // NaN
console.log(`${obj1}`); // "[object Object]"
console.log(obj1 + ""); // "[object Object]"

// Symbol.toPrimitive 속성이 있는 객체.
const obj2 = {
  [Symbol.toPrimitive](hint) {
    if (hint === "number") {
      return 10;
    }
    if (hint === "string") {
      return "hello";
    }
    // hint = default
    return true;
  },
};
console.log(+obj2); // 10        — hint is "number"
console.log(`${obj2}`); // "hello"   — hint is "string"
console.log(obj2 + ""); // "true"    — hint is "default"
```

- 해당 메소드에는 hint가 인자로 전달된다
- hint 값을 통해 형 변환시의 값을 지정할 수 있다.
- 반드시 원시값을 반환해야한다. (그렇지 않으면 에러 발생)

#### 2. toString, valueOf 메소드 지정

```js
const obj3 = {
  toString: () => "string",
  valueOf: () => 3,
};

console.log(+obj3); // 3
console.log(`${obj3}`); // string
console.log(obj3 + ""); // '3'
```

- 원시값을 반환하지 않아도 에러가 발생하지 않고, 호출이 무시된다. = 원시값을 반환한다고 보장하지 않는다.
- 일반적인 (위의 메소드를 오버라이딩하지 않은) 객체의 valueOf는 객체 자신을 반환한다 -> 무시됨
- 그렇기 때문에 숫자형으로 평가되는 연산에도 toString 메소드가 호출된다 ('[object Object]')
- ~~그래서 toString만 지정해도 충분~~
