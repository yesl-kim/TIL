# Link

**푸시 알림 클릭 시 앱 내 특정화면으로 이동**하기 위해

- deep link 혹은
- react native firebase에서 제공하는 dynamic link

를 사용할 수 있다.

## deep link vs dynamic link

### deep link

- **앱 외부, 즉 웹이나 다른 앱에서 앱을 열거나 앱 내의 특정화면으로 이동하기 위한 링크**를 말한다.
- URL scheme 방식, universal link, app link 방식이 있다.
- URL scheme 방식은 React Native 에서 자체적으로 제공하며, 별도 라이브러리 필요없이 구현할 수 있다.

#### URL scheme

- URL scheme이라는 것이 http://, https:// 와 같이 앞에 붙는 `http`와 `https`를 말한다.
- 이 둘은 웹을 여는 URL scheme인데, 앱을 여는 scheme도 있다. (잘 알려진 것으로 `mailto`)
- 특정 앱을 열기 위해 특정 URL scheme을 만드는 것도 가능하다.
- 하지만 누구나 가능한 만큼 내가 생성한 URL scheme이 고유하리라 보장할 수도 없다.
- *연결 프로그램 선택 모달*이 뜨는 경우가 바로 열기를 시도한 scheme과 동일한 앱이 여러 개인 경우이다.

#### universal link, app link 방식

- 다른 앱과 중복되지 않는, 고유한 링크를 사용하는 방식이 바로 universal link, app link 방식이다.
- 모두 웹 도메인처럼 도메인 링크를 사용하는 방식으로 지원하는 플랫폼만 다르다.
- 두 플랫폼에 따라 아직 호환이 잘 되지 않아 아직 안전하지 않다.

### dynamic link

- react native firebase에서 제공하는 서비스로 딥링크와는 역할이 **다르다**
- 앱 내 특정 화면으로 이동하려고 할 때는 그 전에 사용자가 어떤 상태인지 확인해야 한다.
  1. 사용자의 플랫폼이 android인지, ios인지
  2. 앱이 설치되어 있는지 아닌지
- 위 조건에 따라 1) 앱을 여는 방식도, 2) 열어야하는 앱도 (스토어vs앱) 달라지게 된다.
- dynamic 링크는 이런 세부사항을 GUI로 쉽게 설정할 수 있도록 해주며
- 사용자 지표도 측정가능하다 (마케팅 지표로 활용)

### 정리

- 다이나믹 링크로 링크를 호출했을 때의 사용자의 플랫폼과 상황을 쉽게 대응할 수 있고, 지표 측정도 가능하다.
- 앱이 이미 설치되어 있다면 다이나믹 링크는 알아서 딥링크를 호출하여 앱 내 특정화면을 연다.

주어진 요구사항은,

- 구매한 쿠폰이 만료되기 전 3회
- 쿠폰을 소유한 사용자 기기로 알림을 보내는 것

이기 때문에, 딥링크로도 충분할 것으로 판단했다.

## deep link 사용방법

### set up

## 참고자료

- [React Native 공식문서 | Linking](https://reactnative.dev/docs/linking?syntax=ios#enabling-deep-links) : deep link 와 URL scheme 에 대한 대략적인 설명 참고
- [React navigation 공식문서 | Set up with bare React Native projects](https://reactnavigation.org/docs/deep-linking/#set-up-with-bare-react-native-projects): 초기세팅 참고
- [Deep Linking with React Native Push Notifications](https://medium.com/knock-engineering/react-native-push-notification-deep-links-5d0aa0375f2): 푸시알림 클릭 시 딥링크를 사용하여 화면 이동하는 전체적인 프로세스 이해
- [React navigation 공식문서 | Configuring links](https://reactnavigation.org/docs/configuring-links/#mapping-path-to-route-names): 딥링크 사용을 위한 navigation 설정 방법
- [React navigation 공식문서 | Third-party integrations](https://reactnavigation.org/docs/deep-linking/#third-party-integrations) : 앱이 종료된 상태에서 푸시 알림 클릭시 다음 이동할 화면의 경로를 navigation container에 전달하기 위한 코드 구현 부분 참고
