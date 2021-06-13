# pressable vs Touchable Component

> [Pressable vs Touchable in React Native](https://medium.com/@mahyarmohammadi/react-native-pressable-vs-touchable-5fec6b332f15).

둘은 터치와 관련된 사용자 상호작용(Interaction)을 제공한다는 점에서 같은 기능을 한다. 둘의 차이점을 알아보자.

Touchable Components

- 기본 스타일을 제공한다.

- `Button`
- `TouchableOpacity`

Pressable

- `touchable` 컴포넌트보다 더 세밀한 동작에 대한 상호작용을 제공한다. (`onLongPress`, `delayLongPress` 등등)

---

리액트 네이티브 공식문서에 따르면 `pressable` 컴포넌트로 `touchable` 컴포넌트를 대체할 것을 권한다.
