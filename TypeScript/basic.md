# Basic Type

## Intro

- **input**
- **operation** | 로직
- **output** | operation의 결과물, 반환값

타입을 최대한 명확하게 명시함으로써 **타입이 최대한 보장되도록** 작성하는 것이 중요!

## 기본 타입

- number
- string
- boolean
- undefined
- null
- object 👎🏻
- unknown 👎🏻
- any 👎🏻
- void
- never

### object

- 객체 타입에는 배열도, 함수도 올 수 있기 때문에 타입이 명확히 보장되지 않는다. 👎🏻

### undefined과 unll

- 값이 단독으로 사용되는 경우는 없다.  
  단독으로 사용될 경우 그 타입"만" 가질 수 있기 때문에  
  다른 타입과 함께 유니온 타입으로 정의될 때 주로 사용된다.
  ex) `let age: number | undefined;`

- **undefined**  
  값이 있거나 아직 결정되지 않았거나

- **null**  
  값이 있거나 없거나

### unknown과 any

- 모든 종류의 값을 허용한다.

- 주로 타입을 알 수 없는 경우나 타입 정의가 안 된 외부 패키지를 사용하는 경우에 사용한다.

- 하지만 그만큼 타입이 보장되지 않기 때문에 주의가 필요하다  
  남발 x

### void와 never

- 함수의 반환값에 쓰이는 타입이다.

- **void**  
  반환값이 없을 때  
  생략가능하다.

```ts
function void(): void {
  console.log('void')
}
```

- **never**  
  항상 예외가 발생해서 정상적으로 종료되지 않거나, 무한 루프로 인해 아예 종료되지 않는 함수의 반환 타입  
  => 반환 x, 종료 x,

```ts
function never(): never {
  throw new Error("never");
  // or
  // while (true) {...}
}
```

### Array와 Tuple

#### Array

배열의 타입 표기법은 다음과 같다.  
후자는 제약이 있는 경우도 있기 때문에 통일성을 위해 전자로 표기하는 것이 좋다는 것이 한 쪽의 의견

- _primitive type_[]  
  ex) `number[]`

- Array<_primitive type_>  
  ex) `Array<number>`

#### Tuple

- 서로 다른 타입을 포함하는 배열

- interface, type alias, class 등으로 대체가 가능하다  
  인덱스로 접근하는 배열의 방식이 직관적이지 않다는 단점때문에 웬만하면 대체해서 쓰는 것이 좋음

- 꼭 사용해야 한다면 비구조화할당으로 단점을 보안할 수 있다.
  ```ts
  const student: [string, number] = ["name", 123];
  const [name, age] = student;
  ```

### Alias

원하는 타입을 변수처럼 생성할 수 있다.

### UNION

- 발생할 수 있는 다양한 케이스 중의 '하나만' 지정할 수 있을 때 사용

- OR `|`로 표시한다.

- **discriminated union**
  union과 비슷하지만, state별로 다른 값을 가진 '동일한 키'를 가지고 있다.  
  => 동일한 프로퍼티의 값을 비교하여 타입을 구분한다.

### Intersection

- 지정된 타입 모두 만족해야하는 타입

- AND `&`로 표시

## 열거형 타입: enum

- 자바스크립트에서는 존재하지 않지만 타입스크립트에서 자체제공하는 타입

- 관련있는 여러 값들을 관리할 수 있다.

- number, string 타입의 값을 할당할 수 있다.

- 값을 할당하지 않을경우, 0부터 하나씩 커지는 숫자가 자동으로 할당된다.

- 모든 enum은 union 타입으로 대체할 수 있으며, 대체하는 편이 좋다.  
  타입을 명확하게 보장하지 않기 때문에

  ```ts
  enum Fruit {
    Apple,
    Banna,
    Orange,
  }

  let fruit: Fruit = Fruit.Apple; // 0
  fruit = 100; // 오류가 나지 않는다.
  ```

- 네이티브 언어는 union 타입을 제공하지 않기 때문에 모바일과 웹이 서로 교류가 있을 경우 enum 타입을 써야한다.

## 함수 타입

함수의 매개변수에 지정할 수 있는 타입

### optional parameter `?`

선택사항인 매개변수에 대해서 `?`기호를 사용할 수 있다. 해당 매개변수를 함수를 호출할 때 전달하지 않아도 오류를 던지지 않는다.

### default parameter

기본값이 정해진 매개변수는 선택 매개변수와 마찬가지로 동작한다. (값을 전달하지 않아도 오류를 던지지 않는다.)

```ts
function default(a : string = 'default parameter') {}
```

### rest parameter

나머지 매개변수의 타입은 배열로 정의할 수 있다.

## 타입 추론
