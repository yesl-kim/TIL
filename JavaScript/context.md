> 정재남, <코어 자바스크립트>, 위키북스

# 실행 컨텍스트

이 글은 '코어 자바스크립트' 책을 참고하여 요약한 것으로 크게 두 가지 개념을 담고 있다.

1. 호이스팅
2. 스코프

---

## 실행 컨텍스트

실행할 코드에 필요한 환경 정보들을 모아놓은 객체

- 전역 컨텍스트  
  전역 공간에 대한 정보.  
  자동으로 생성
- eval 및 함수에 의한 컨텍스트  
  함수가 실행될 때 필요한 정보들을 수집

~~이외에도 있는 것 같지만 책을 통해 알 수 있는 것은 여기까지였다.~~

### 실행 컨텍스트의 구성

- **VariableEnvironment**

  변경사항은 반영되지 않음  
  선언 시점의 LexicalEnvironment의 스냅샷이라고 생각하면 됨

  - environmentRecord  
    현재 컨텍스트 내의 식별자들에 대한 정보
  - outerEnvironmentReference  
    외부 환경 정보  
    바로 직전 컨텍스트 (= 상위 레벨 스코프)의 LexicalEnvironment를 참조

- **LexicalEnvironment**

  변경사항 반영

  - environmentRecord
  - outerEnvironmentReference

- **ThisBinding**

  this 식별자가 바라봐야 할 대상 객체

<br/>

처음에 현재 컨텍스트 내의 식별자들에 대한 정보 + 외부 환경 정보를 모아 `VariableEnvironment`를 생성한 후, 이를 그대로 복사하여 `LexicalEnvironment`를 만들고 이후에는 주로 `LexicalEnvironment`를 활용한다.

`VariableEnvironment`와 `LexicalEnvironment`는 현재 컨텍스트 내의 식별자들에 대한 정보가 담긴 `environmentRecord`와 외부 환경 정보가 담긴 `outerEnvironmentReference`로 구성된다.

## environmentRecord와 호이스팅

`environmentRecord` 에는 현재 컨텍스트 내부의 식별자들에 대한 정보, 즉 매개변수명, 함수 내부에서 선언된 함수, var로 선언된 변수명 등이 저장된다.

이런 정보는 함수가 호출이 되면 **실행되기 전에** 자바스크립트 엔진에 의해 작서된 **순서대로** 한 데 모아 저장된다. 호이스팅(= "끌어올리는 것")이란 실제 물리적으로 식별자 정보들을 끌어올리는 것이 아니라 함수 실행 전 식별자 정보들만 먼저 저장하는 과정을 표현한 추상의 개념이라고 보면 된다.

이때 **변수는 선언부만 호이스팅하는 반면에 함수 선언문은 전체를 호이스팅**하기 때문에 함수 선언문으로 정의한 함수는 선언 전에 호출해도 오류없이 실행이 된다. **이런 점은 혼란을 일으킬 수 있기 때문에 함수 표현식으로 정의하는 것이 좋다.**

---

## outerEnvironmentReference와 스코프

- **Block**  
  중괄호 `{}`로 감싸진 범위 (`for`, `if` 등)

- **Scope**  
  식별자에 대한 유효범위  
  참조 대상 식별자(변수명, 함수명처럼 어떤 대상을 다른 대상과 구분하여 식별할 수 있는 이름)에 접근할 수 있는 범위

스코프란 식별자에 대한 유효범위를 말한다. 여기서 식별자란 값이나 표현식에 대한 주소값 (변수명이나 함수명)을 말한다.

앞서 언급했던 실행 컨텍스트의 `outerEnvironmentReference`는 현재 호출된 함수가 **선언될 당시의** LexicalEnvironment를 **참조**한다. 간단히 말하면 부모의 (조상x, 바로 위 함수) LexicalEnvironment를 가리킨다. 그래서 함수를 실행할 때 필요한 식별자에 대한 정보가 함수 내부, 즉 자신의 `LexicalEnvironment` 안에 저장되었지 않으면 `outerEnvironmentReference`가 가리키고 있는 (부모 함수의) `LexicalEnvironment`에서 그 정보를 찾는다.

역시 그 위의 함수는 다시 더 위의 `LexicalEnvironment`를 가리키고 있어서 `outerEnvironmentReference`는 연결 리스트의 형태를 띤다. 이런 형태로 내부에 저장되어 있지 않은 식별자 정보를 찾아 찾아가는 것, 이렇게 연결되어 있는 것을을 스코프 체인이라고 한다. 이런 구조 때문에 스코프가 중첩되어 있을 경우 가장 가까운 스코프의 정보를 우선 참조한다.

자바스크립트는 함수 레벨 스코프를 만들지만 ES6의 let, const 키워드를 통해 블록 레벨 스코프를 만들 수 있다.

### 정리

- 스코프란 식별자에 대한 유효범위를 말한다.
- 식별자는 자신이 어디서 선언되었는지에 의해 유효한 범위를 갖는다.
- 하위 스코프는 상위 스코프에 접근할 수 있지만 상위 스코프는 하위 스코프에 접근이 불가하다.
- 중첩 스코프는 가장 인접한 지역을 우선 참조한다.
- 자바스크립트는 블록 레벨이 아닌 **함수 레벨 스코프**를 따른다. 하지만 ES6 문법의 `let`, `const` 키워드를 쓰면 블록 레벨 스코프를 사용할 수 있다.

### Scope 사용에 주의할 점

1. 최소한의 전역 변수 사용

   - 최대한 `let`과 `const` 키워드를 사용하여 전역 변수를 줄이는 것이 좋다.
   - 전역 변수의 사용을 줄이는 방법으로 전역 변수의 객체를 만들어 사용하는 것이다.
   - 즉시 실행함수를 사용하면 함수 실행 후 전역 변수는 즉시 삭제된다.

2. 다른 블록, 스코프에 있는 변수끼리도 변수명이 중복되지 않도록 주의한다.

scope의 오염 예제.

```js
function logSkyColor() {
  const dusk = true;
  let myColor = "blue";

  if (dusk) {
    let myColor = "pink";
    console.log(myColor); // pink
  }

  console.log(myColor); // blue
}

console.log(myColor); // 에러!!
```
