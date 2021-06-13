# scope

> **Block**? 중괄호 `{}`로 감싸진 범위 (`for`, `if` 등)  
> **Scope**? 참조 대상 식별자(변수명, 함수명처럼 어떤 대상을 다른 대상과 구분하여 식별할 수 있는 이름)에 접근할 수 있는 범위  
    - **전역 스코프** (Global scope)
    코드 어디에서도 식별자에 접근이 가능하다.  
    - **지역 스코프** (Local scope or Function-level scope)  
    코드 블록이 만든 스코프로 선언된 블록 내 혹은 하위 블록에서만 접근이 가능하다.


- 식별자는 자신이 어디서 선언되었는지에 의해 유효한 범위를 갖는다.
- 자바스크립트는 블록 레벨이 아닌 **함수 레벨 스코프**를 따른다. 하지만 `let` 키워드를 쓰면 블록 레벨 스코프를 사용할 수 있다.
- 중첩 스코프는 가장 인접한 지역을 우선 참조한다.

```js
// 자바스크립트는 비 블록 레벨 스코프
if (true) {
    var x = 'not block level scope';
    console.log(x)  // 'not block level scope'
};
console.log(x);     // 'not block level scope'

// 함수 레벨 스코프를 따른다
var x = 'global';

function testScope() {
    var x = 'local';
    if (x) {
        var x = 'hi';
        console.log(x)  // 'hi'
    }
    console.log(x)  // 'hi'
};
console.log(x)  // 'global'

// let 키워드를 쓰면 블록 레벨 스코프를 사용할 수 있다.
function testScope() {
    let x = 'local';
    if (x) {
        let x = 'hi';
        console.log(x)  // 'hi'
    }
    console.log(x)  // 'local'
};

// 함수 안에서는 밖을 볼 수 있고 밖에서는 안을 볼 수 없다.
var x = 'global'

function testScope() {
    var x = 'local'
    function inner(){
        x = "I'm in.";   // x 접근가능. 'local' 값의 변경
        var y = 'can you see?';
        console.log(x, y);
    }
    
    inner();    // 'I'm in. can you see?'
    console.log(x); // 'I'm in.'
    console.log(y); // error : y is not defined. y 접근 불가능
}
console.log(x)  // 'global'
```

## 렉시컬 스코프 (Lexical scope)
앞서 얘기 했듯이 자바스크립트에서는 식별자가 ~~어디서 호출되었는지~~가 아니라 **어디서 선언되었는지**에 따라 상위 스코프가 결정된다. 이를 렉시컬 스코프(Lexical scope), 정적인 스코프라고 한다.
따라서 다음 예제의 `bar` 함수의 상위 스코프는 전역 스코프가 되며, 함수 내의 변수 x는 전역 변수 x를 참조한다.

```js
var x = 1;

function foo() {
  var x = 10;
  bar();
}

function bar() {
  console.log(x);
}

foo();  // 1
bar();  // 1
```

## scope 사용에 주의할 점

1. 최소한의 전역 변수 사용
    - 최대한 `let`과 `const` 키워드를 사용하여 전역 변수를 줄이는 것이 좋다.
    - 전역 변수의 사용을 줄이는 방법으로 전역 변수의 객체를 만들어 사용하는 것이다.
    - 즉시 실행함수를 사용하면 함수 실행 후 전역 변수는 즉시 삭제된다.

2. 다른 블록, 스코프에 있는 변수끼리도 변수명이 중복되지 않도록 주의한다.

scope의 오염 예제.
```js
function logSkyColor() {
  const dusk = true;
  let myColor = 'blue'; 
 
  if (dusk) {
    let myColor = 'pink';
    console.log(myColor); // pink
  }
 
  console.log(myColor); // blue 
}
 
console.log(myColor); // 에러!!
```
