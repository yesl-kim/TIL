# mocking

실제 동작을 하기 위해서는 여러 의존성 라이브러리, 데이터베이스로의 접근 등 여러 기능들이 필요하다.
테스트할 때도 실제와 같은 환경에서 하는 것은 권장되지 않는다.

- api경우 네트워크를 타게 되면 속도가 느려진다.
- 네트워크 상태 혹은 다른 예외 상황에 따라 테스트의 결과가 달라질 수 있어 온전히 독립된 테스트가 어렵다.

이러한 이유로 테스트할 때는 테스트하려는 기능에만 집중할 수 있도록
내부에서 사용되는 다른 의존성 모듈이나 함수 등을 모의로 만들어 사용한다.

## [jest.fn()](https://jestjs.io/docs/mock-function-api/#mockfnmockimplementationfn)

- 모의 함수를 만드는 메소드
- 아무런 동작도, 반환도 하지 않는다.
- 반환된 모의함수의 메소드를 통해 다른 기능들을 추가로 할 수 있다

### .mockReturnValue()

- 반환값을 정할 수 있다.

```js
const mock = jest.fn()
mock.mockReturnValue(3)
expect(mock()).toBe(3)
```

### .mockImplemetation()

- 모의함수는 아무런 동작도 하지 않지만, 특정 동작을 하도록 설정하고 싶을 때 사용
- jest.fn()에서 바로 지정할 수도 있다
  > jest.fn(implementation) is a shorthand for jest.fn().mockImplementation(implementation).

```js
const mockFn = jest.fn((scalar) => 42 + scalar)

mockFn(0) // 42
mockFn(1) // 43

mockFn.mockImplementation((scalar) => 36 + scalar)

mockFn(2) // 38
mockFn(3) // 39
```

## jest.spyOn

- 어떤 함수나 메소드를 모의 함수로 대체하지는 않지만,
- 모의함수처럼 함수가 호출되었는지, 어떤 값으로 호출되었는지 등 matcher를 사용하여
- 추적하고 싶다면! 사용

```js
const calculator = {
  add: (a, b) => a + b,
}

const spyFn = jest.spyOn(calculator, 'add')

const result = calculator.add(2, 3)

expect(spyFn).toBeCalledTimes(1)
expect(spyFn).toBeCalledWith(2, 3)
expect(result).toBe(5)
```
