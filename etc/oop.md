# OOP

## 객체지향 원칙

### 캡슐화 (Encapsulation)

- 외부에서 객체 내부의 상태를 변경할 수 없지만, 외부의 행동을 통해 내부 상태를 변경할 수는 있다.

### 추상화 (Abstraction)

- 내부의 복잡한 구조를 이해하지 않고도 제공하는 기능을 통해 외부에서 내부 기능을 사용할 수 있다.

### 상속 (Inheritance)

### 다양화 (Polymolphism)

=> 완전한 class를 만드는 것보다는 원하는 기능은 사용할 때 설정이 가능하도록 만드는 것이 재사용성을 높일 수 있는 방법이다.

---

## static

- instance level이 아닌, **class level**의 변수나 메소드를 생성할 때 사용
- 해당 변수나 메소드에 접근할 때는 클래스의 프로퍼티로 접근해야 한다.

```js
class StaticClass {
  static classLevel = 'class level variable';

  constructor() { ... }

  static func() { ... }
}

const instance = new StaticClass();
instance.classLevel // error
StaticClass.classLevel
StaticClass.func()
```

### static 메소드의 활용

- 생성자 (contructor)함수를 private로 하여 접근을 막은 후에, 생성자와 비슷한 역할을 하는 (인스턴스 객체를 만드는) static 메소드를 만들어 new 키워드가 아닌 해당 메소드로만 인스턴스 객체를 만들게 하는 경우가 있다.

- 이는 메소드 안에서 다양한 로직을 추가할 수 있고, 싱글톤 패턴처럼 생성하는 **인스턴스 객체 갯수에 제한**을 둘 수 있다.

- 인스턴스를 생성하는데 로직이 복잡해진다면, static 함수로 로직을 캡슐화할 수 있다.

## 접근제어자 : public vs private vs protected

- **public** (default)  
  클래스 외부에서, 인스턴스에서 접근이 가능  
  공개적 멤버

- **private**  
  클래스 외부에서 접근이 불가능  
  조회, 수정, 삭제 등이 모두 불가능

- **protected**  
  클래스 외부에서 접근 불가능  
  클래스를 상속한 자식 클래서 내부에서는 접근 가능

- 클래스 멤버변수는 private로 걸어놓고, 클래스 메소드로 접근하여 변수를 변경할 수 있게 하는 것이 안정적으로 변수를 관리할 수 있다. 메소드 내에서 예외처리를 해줄 수 있기 때문에 (ex. 들어온 인자값을 검사하는 등)

- constructor 함수의 인자 앞에 접근제어자를 붙여주면, 따로 멤버변수를 생성하지 않고도 바로 멤버변수를 생성할 수 있다.

```ts
class Test {
  contructor(public a: string, private b: number) {
    this.a = a;
    this.b = b;
  }
}
```

## getter & setter

- class 내부에서는 메소드로 선언하지만, 접근할 때는 class의 멤버변수로 접근한다.

- 함수이기 때문에 유효성 검사 등 다양한 로직을 추가할 수 있다.

```

```
