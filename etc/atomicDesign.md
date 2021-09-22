## basic

### Atoms

- 페이지 구성 요소 중 더이상 분해할 수 없는 가장 작은 단위
- HTML 요소를 포함하며, **기능을 중단하지 않고는 분해할 수 없다**  
  태그 당 하나라고 하지만 꼭 하나의 원자가 하나의 태그를 갖는 것은 아닌 것 같다.  
  가령 `<select><option /></select>` 와 같은 구조는 두 개의 태그지만 기능적으로 이 둘을 나눌 수는 없다

### Molcules

- 둘 이상의 원자의 그룹
- 하나 단위로 동작하는 컴포넌트
- 페이지 안에서 반복되는 요소가 주로 molecules
- atoms보다 구체적이고 작동적
- 하지만 여러 개의 원자가 꼭 분자로 묶여야 하는 것은 아니다.  
  여러개의 원자는 페이지 안에서 하나의 유기체로 동작할 수도 있고, 분자로 묶일 수도 있다.

### Organism

- 페이지에서 찾아낼 수 있는 큰 영역
- 유기체는 또 다른 유기체나 분자를 포함한다

> Tips
> 아토믹 디자인 시스템에 따라 컴포넌트를 나눌 때의 팁!  
> **Organism -> Molcules -> Atoms 순으로 나눈다**  
> 사람의 인식체계가 하향식이기 때문에 이 방식이 훨씬 쉽게 다가갈 수 있다.

- 한 페이지 안에서 한 덩어리의 영역이 유기체
- 유기체 안의 더 작은 영역은 또 다른 유기체이거나 분자이다.  
  유기체 안의 영역이 페이지 안에서 여러번 반복된다면 분자!
- 분자 안의 더 이상 쪼갤 수 없는 요소가 원자

> 반면, 나눈 컴포넌트를 코드로 작성할 때는 작은 것부터 구현하는 것이 많이 추천된다.

## Tips

[디자인-시스템을-만들때-마주하는-고민사항-23가지와-32개-팁](https://medium.com/lemonade-engineering/%EB%94%94%EC%9E%90%EC%9D%B8-%EC%8B%9C%EC%8A%A4%ED%85%9C%EC%9D%84-%EB%A7%8C%EB%93%A4%EB%95%8C-%EB%A7%88%EC%A3%BC%ED%95%98%EB%8A%94-%EA%B3%A0%EB%AF%BC%EC%82%AC%ED%95%AD-23%EA%B0%80%EC%A7%80%EC%99%80-32%EA%B0%9C-%ED%8C%81-cabc58670421)

## Reference

[TOAST UI | 리액트 어플리케이션 구조 - 아토믹 디자인](https://ui.toast.com/weekly-pick/ko_20200213)
[인스타그램으로 아토믹 디자인 시스템 적용 사례](https://medium.com/backticks-tildes/visually-breaking-down-ui-components-using-atomic-design-part-1-476e1ddd73ca)
