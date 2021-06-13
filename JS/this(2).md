# this(2) : 일반함수와 화살표 함수의 this

> [poiemaweb | 5.17 this](https://poiemaweb.com/js-this)
> [poiemaweb | 6.3 Arrow function](https://poiemaweb.com/es6-arrow-function)
> [MDN | 화살표 함수](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Functions/Arrow_functions)

## 1. 일반 함수의 this

자바스크립트는 함수 호출 방식에 따라 `this`에 바인딩되는 객체가 달라진다.

1. 함수 호출 방식
2. 메소드 호출 방식
3. 생성자 함수 호출 방식

### 1-1. 함수 호출 방식 : this -> 전역객체

기본적으로 `this`는 전역객체에 바인딩된다. 이는 내부함수, 콜백함수에서도 마찬가지이다.
브라우저 환경에서 전역객체는 `window`, Node.js 환경에서는 `global`이다.

```javascript
var value = 1;

var obj = {
  value: 100,
  foo: function () {
    console.log("foo's this: ", this); // obj
    console.log("foo's this.value: ", this.value); // 100
    function bar() {
      console.log("bar's this: ", this); // window
      console.log("bar's this.value: ", this.value); // 1
    }
    bar();
  },
};

obj.foo();
```

메소드의 내부함수에서 `this`가 전역객체에 바인딩되는 문제는 메소드에서 `this`를 (비전역) 변수에 할당하여 해결할 수 있다.

```javascript
var obj = {
  value: 100,
  foo: function () {
    var that = this;
    console.log("foo's this: ", that); // obj
    console.log("foo's this.value: ", that.value); // 100
    function bar() {
      console.log("bar's this: ", that); // obj
      console.log("bar's this.value: ", that.value); // 100
    }
    bar();
  },
};

obj.foo();
```

### 1-2. 메소드 호출 방식 : this -> obj

![메소드 호출](https://poiemaweb.com/img/Method_Invocation_Pattern.png)
함수를 객체의 메소드로 호출할 경우 `this`는 메소드를 호츨한 객체에 바인딩된다.

### 1-3. 생성자 함수 호출 : this -> instance

생성자 함수와 일반 함수의 형식적 차이는 존재하지 않지만 new 연산자로 호출되는 생성자 함수는 인스턴스 객체를 생성하는 역할을 한다. 이때 **생성자 함수 내의 `this`는 새로 생성된 객체(인스턴스)에 바인딩 된다.**

## 참고로 생성자 함수는 일단 빈 객체를 만들고 함수 안에서 `this`를 통해 빈 객체에 새로운 프로퍼티를 추가하는 방식으로 동작한다.

## 2. 이벤트 핸들러 함수 내부의 this

### 2-1. 인라인 이벤트 핸들러 방식 : this -> 전역객체

인라인 이벤트 핸들러 방식의 경우, 이벤트 핸들러는 일반 함수로 호출이 되므로 이때 `this`는 **전역객체**에 바인딩된다.

```html
<button onclick="foo()"></button>

<script>
  function foo() {
    console.log(this); // window
  }
</script>
```

### 2-2. 이벤트 핸들러 프로퍼티 방식 : this -> event.currentTarget

메소드로 호출되는 프로퍼티 방식의 이벤트 핸들러 내부의 `this`는 **이벤트에 바인딩된 요소**를 가리킨다. 이것은 이벤트 객체의 currentTarget 프로퍼티와 같다.

```javascript
const btn = document.getElementByTagName("button");

btn.onclick = function (event) {
  console.log(this); // <button>
  console.log(event.currentTarget); // <button>
  console.log(this === event.currentTarget); // true
};
```

### 2-3. addEventListener 메소드 방식

## 이 방식의 이벤트 핸들러는 콜백 함수로 호출이 되지만 이벤트 핸들러 내부의 `this`는 **이벤트 리스너에 바인딩된 요소(currentTarget)**를 가리킨다.

## 3. 화살표 함수의 this

함수 호출 방식에 따라 `this`가 동적으로 결정되는 일반함수와 달리 화살표 함수는 함수가 선언될 때 `this`에 바인딩될 객체가 정적으로 결정된다.
**화살표 함수의 `this`는 언제나 상위 스코프의 `this`를 가리킨다.**
