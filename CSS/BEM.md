js와 다르게 css는 `-`으로 공백을 구분한다.

## BEM (Block Element Modifier)

### 개념

- css의 클래스명을 짓는 방법론

- Block, Element, Modifier로 구성

- **목적에 따른 네이밍**  
  이름지으려는 요소의 역할이 페이지의 영역(block)인지,  
  그 block의 구성요소(element)인지,  
  혹은 요소의 형태를 나타내기 위함인지(modifier)

- **Block**  
  재사용가능한, 기능적으로 독립된 컴포넌트

- **Element**  
  블럭을 구성하는 단위  
  독립적이지 않고 의존적  
  개인적인 생각) 태그의 위치상 상-하 개념이 아니라, 역할 상의 상-하 개념  
  ex) `.block > .block__element1 > .block__element2`  
  -> element2가 `.block__element1`의 하위로 들어가지 않고 .block의 element로 들어감

- **Modifier**  
  블럭이나 엘리먼트의 속성  
  색깔이나 크기 등

### 작성법

- 상위요소와 하위요소의 관계 : `__`
  상위요소의 하위요소를 나타낼 때는, 상위요소의 밑줄 두개 `__`를 사용하여 구분한다.

```css
.nav {
  ...;
}
.nav__secondary {
  ...;
}

.stick-man {
  ...;
}
.stick-man__head {
  ...;
}
.stick-man__arm {
  ...;
}
```

- 수정가능한 속성을 나타내는 경우 : `--`
  색깔이나 형태같은 수정가능한 속성을 나타내는 경우 두개의 하이픈 `--`으로 연결한다.

```css
.stick-man--blue {
  ...;
}
.stick-man--red {
  ...;
}
```

이를 응용하면 이렇게 된다

```
.stick-man__head--big {...}
.stick-man__head--small {...}

.profile-image--small {...}
.profile-image--big {...}
```
