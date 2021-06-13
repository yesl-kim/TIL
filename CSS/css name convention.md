
js와 다르게 css는 `-`으로 공백을 구분한다.

## BEM (Block Element Modifier)
- 상위요소와 하위요소의 관계 : `__`
상위요소의 하위요소를 나타낼 때는, 상위요소의 밑줄 두개 `__`를 사용하여 구분한다.

```css
.nav {...}
.nav__secondary {...}

.stick-man {...}
.stick-man__head {...}
.stick-man__arm {...}
````

- 수정가능한 속성을 나타내는 경우 : `--`
색깔이나 형태같은 수정가능한 속성을 나타내는 경우 두개의 하이픈 `--`으로 연결한다.

```css
.stick-man--blue {...}
.stick-man--red {...}
```

이를 응용하면 이렇게 된다
```
.stick-man__head--big {...}
.stick-man__head--small {...}

.profile-image--small {...}
.profile-image--big {...}
```