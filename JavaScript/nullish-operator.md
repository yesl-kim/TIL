# ?? 연산자 (null 병합 연산자)

> [코어 자바스크립트 - 자바스크립트 기본](https://ko.javascript.info/nullish-coalescing-operator)

## 기본값을 지정할 때 `||` 연산자의 문제점

- 기본값을 지정할 때 많이 사용되던 `||` 연산자는
- null, undefined 뿐 아니라 0, 빈 문자열, false, NaN 같은 값도 falsy한 값으로 간주하기 때문에
- 이런 falsy한 값을 유효한 값으로 간주할 때는 `||` 연산자가 적합하지 않음

```js
const height = 0;
const b = height || 100; // 100

// height가 0일 때 0으로 하고 싶다면???
```

## `??` 연산자

> null, undefined만 확인한다!

- 피연산자 중 null이나 undefined가 **아닌** 값을 반환하는 연산자
  ```js
  const a = b !== null && b !== undefined ? b : c; // a = b ?? c 와 같다
  ```
- `??` 연산자는 (`=`, `?` 연산자 외의 연산자) 거의 대부분의 연산자보다 우선순위가 낮기 때문에 **복잡한 연산을 할 때는 `()`를 붙여주는 것이 좋다**
  ```js
  let a = b ?? c + d ?? e; // b ?? (c + d) ?? e
  a = (b ?? c) + (d ?? e);
  ```
- `&&` 연산자와 `||` 연산자를 함께 쓸 수 없다.
  ```js
  const a = b && c ?? d // syntax error
  const a = (b && c) ?? d // 꼭 써야하는 상황이라면 ()를 사용해야한다.
  ```

### `??`와 `||` 의 차이점

- `??`: 피연산자가 `null` 또는 `undefined` 인지 확인
- `||`: 피연산자가 `falsy` 한 값인지 확인

```js
"" || "test"; // test
"" ?? "test"; // ''

0 || 100; // 100
0 ?? 100; // 0

false || true; // test
false ?? true; // false

NaN || 100; // 100
NaN ?? 100; // NaN
```
