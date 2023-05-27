# 갖가지 트러블 슈팅

#### ReferenceError: expect is not defined

```
// jest.config.js
// setupFiles -> setupFilesAfterEnv
setupFilesAfterEnv: [
  './src/__setup__/setupTests.js',
],
```

- jest.config.js 파일에서 setupFiles 속성이 아닌 setupFilesAfterEnv으로 써줘야한다.
- [참고글 | "expect is not defined" when using with jest](https://github.com/testing-library/jest-dom/issues/122)
- [참고글 | "expect is not defined" when importing jest-enzyme in setup.js](https://github.com/enzymejs/enzyme-matchers/issues/86#issuecomment-312489052)
- ~~두 참고글 결국 내용은 같다~~

#### press, scroll, changeText 외의 다른 이벤트가 필요할 때

> fireEvent(<element>, <eventName>)

```js
const button = screen.getByRole('button')
fireEvent(button, 'pressIn')
fireEvent(button, 'pressOut')
```

- [참고글 | Fire a "swipe" event on a Swipeable component (이벤트를 일으키는 형태 참고 ~~따라했더니 되네?~~)](https://github.com/callstack/react-native-testing-library/issues/918)
- [ ] [이 글을 보면 createEvent 로 이벤트도 만드는 것 같던데 그걸 뭘까](https://stackoverflow.com/questions/62030095/how-to-test-paste-event-in-a-generic-input-text-with-react-testing-library)
- [ ] generic form of fireEvent ???
