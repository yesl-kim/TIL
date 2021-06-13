# layout

공간을 분할할 때는 먼저 **행을 구분한 후, 행 내부 요소를 분리**하는 것이 일반적이다.

## 0. display
> display : inline | inline-block | block | ...

html 요소는 모두 박스 형태의 영역을 가지고 있는데 이를 크게 두가지로 나누면, 요소들이 수평으로 정렬되는 `inline`, 수직 정렬되는 `block` 요소가 있다. 이는 display 속성으로 따로 지정하여 바꿀 수도 있다.

|    | 정렬 | 특징 | 태그 |
|:---:|:---:|:---|:---:|
| inline | 수평정렬 | - width,height 값을 갖지 않는다 <br> - 텍스나 이미지 같은 내용만큼의 크기값을 갖는다. | `span`, `a`, `i` 등 |
| inline-block | 수평정렬 | - width, height 값을 갖는다. | |
| block  | 수직정렬 | - width, height 값을 갖는다. <br> width는 기본적으로 100%, height는 내용의 크기 | `div`, `h`, `p` 등 |

## 1. float
> float : left | right

`float` 속성을 사용하면 블록 요소에 `display : inline-block` 하지 않고 왼쪽 혹은 오른쪽으로 수평정렬을 시킬 수 있다. `float`은 원래 이미지 주변에 텍스트를 위치 시키게 하기 위해 사용했지만 레이아웃을 잡는 용도로도 많이 사용하고 있다.

```html
<body>
<nav>
    <ul>
        <li>home</li>
        <li>profile</li>
        <li>blog</li>
        <li>cafe</li>
    </ul>
</nav>
</body>
```
```css
nav {
  border: 1px solid #ccc;
}

ul {
  list-style: none;
  float:right;
}

li {
  padding: 0 10px;
  float: left;
}
```

이미지

### clearfix
float 속성을 갖는 요소가 부모요소 밖으로 벗어나는 문제 (부모요소가 float 요소를 감싸지 못하는 문제)를 해결하기 위한 방법이다.

#### 1. clear : both
```css
.container::after {
    content: '';
    display: block;
    clear: both;
}
```

#### 2. overflow : auto
```css
.container {
    overflow : auto;
}
```

위의 예시 문제에 적용해보면,

```css
nav::after {
  content: '';
  display: block;
  clear: both;
}
```
이미지
`ul`에 `float` 속성이 적용되어 `nav`가 높이값을 제대로 가지지 못한것이 해결되었다.
부모요소의 가상요소에 `clear:both`를 적용해주면 `html`애서 불필요한 요소를 추가하지 않아도 되는 장점이 있다.

#### 3. 부모요소에도 float 속성을 적용시킨다.


## 2. position
> position : static | relative | absolute | fixed

- static (기본위치)
position 프로퍼티를 지정하지 않을 때의 위치와 같다.
기본값이 static이기 때문에 따로 지정할 일은 없지만 먼저 설정된 position을 초기화하기 위해 사용되기도 한다.
좌표 프로퍼티(top, bottom, right, left)를 같이 사용할 수 없다. (사용해도 무시된다.)

- relative (상대위치)
relative 로만 지정했을 때는 기본 위치와 같다.
하지만 좌표 프로퍼티를 사용할 수 있으며 기본 위치를 기준으로 이동한다.

- absolute (절대위치)
부모요소 또는 가장 가까운 조상 요소 중에 statice을 제외한 position 값을 가진 요소를 기준으로 좌표 프로퍼티만큼 이동한다. 조상 요소 중에 position 값을 가진 요소가 없다면 document body를 기준으로 위치하게 된다.

absolute가 지정된 요소는 inline형식으로 바뀌므로 **width 값을 지정해야한다.**

- fixed (고정위치)
브라우저 화면을 기준으로 위치한다.
스크롤을 하더라도 브라우저 화면에 고정된 형태로 존재하게 된다.
absolute와 같은 이유로 width 값을 지정해야한다.

## z-index
![z-index](https://poiemaweb.com/img/z-index.jpeg)
static을 제외한 position 값을 설정한 경우, 요소는 마치 다른 레이어에 있는 것과 같다. 이때 position 값을 가진 요소들은 html문서에서 나중에 쓰여진 순서대로 화면에 쌓이게 되는데, 쌓이는 순서를 z-index 속성으로 지정을 해줄 수 있다.
z-index 값이 클수록 가장 위 레이어에 위치하게 된다.





---
#### 요소를 수직 정렬하는 방법

1. 부모의 height 값 = 자식의 line-height 값

2. 

#### 수평 정렬하는 방법
1. inline/ inline-block 요소인경우
- text-align : center
부모 요소에 `text-align : center` 지정한다.

2. block 요소인 경우
- { margin : 0 auto }
마진 좌우에 `auto`를 지정하면 수평 정렬이 된다

3. 복수의 block 요소인 경우
- flex 속성
부모요소에 `{ display: flex; justify-content: center; }` 를 준다.

#### 수평, 수직 정렬하는 방법
1. 요소의 너비, 높이값을 알지 못하는 경우
position: absolute;
top: 50%;
left: 50%;
trasform: traslate(-50%, -50%)