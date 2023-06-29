# 리액트 네이티브 앱에서 푸시알림 보내기

- [ ] 기존 프로세스 파악 (사내 푸시 알림 설정이 어떻게 되어있는지)
- [x] 기존 환경설정 파악 (부족한 부분이 없는지)
- [ ] 개발
  - [x] fcm token 업데이트 하는 시점 확인
    - 핀로그인에서 값을 비교하여 update, create 함
    - 지갑 생성 혹은 가져오기할 때 deviceId (= fcm token)을 전달
    - [ ] createWalletHandler에 전달한 fcm token을 어떻게 처리하는지 확인
    - [ ] 전역에서 가져오는 fcm token이 올바른지 확인
  - [ ] user 데이터와 fcm token 연결
  - [x] ios에서는 앱이 background, quit 상태에서 알림이 오지 않는 문제 (알림o, 알림함△, 진동x)
    - setBackgroundMessageHandler 함수가 동작을 안함 -> 알림함, 진동 작동 x
    - 푸시알림을 통해 앱으로 들어가면 onNotificationOpenedApp 함수 동작 -> 알림함 o
    - [참고글](https://github.com/invertase/react-native-firebase/issues/5656)
- [ ] 문서화 (알게된 것들, 백업 및 인수인계를 위해 필요한 배경지식, 설명 문서화)

## Intro

### 푸시알림 종류

1. 로컬 푸시 알림
   - 푸시 서버없이 앱 자체에서 알림을 보내는 기능
   - 예시) 앱에서 특정 액션을 했을 때, local storage 혹은 기기에 저장되는 정보에 의해 알림이 갈 때
   - notifee, react-native-push-notification 등의 라이브러리 사용
2. 원격 푸시 알림
   - 서버를 통해 알림을 보내는 기능
   - 주로 FCM (firebase cloude messaging) 사용

### usecase

푸시알림을 보내는 사용자 범위에 따라

1. 전체 사용자
2. 특정 주제를 구독한 사용자
3. 특정 사용자

로 나뉠 수 있으며, 이번에는 특정 사용자에게 알림을 보내는 것이 요구사항이었다.

## 구현

> **요구사항**
>
> 1. 전체 사용자가 아닌, 특정 조건에 부합하는 사용자에게만 알림을 보낸다. (구매한 쿠폰이 만료되기 전 n일 전, 오전 9시에 푸시 알림)
> 2. 앱이 꺼져있어도 알림이 울린다
> 3. 기기 알림, 앱 내의 알림함 두 곳에서 확인할 수 있다.
> 4. 알림 클릭시 특정 화면으로 이동한다.

## 특정 조건의 사용자에게 푸시알림 보내기

### 프로세스

일반적으로 푸시알림 서버를 사용하여 리모트 알림을 보냄
q. 로컬 푸시알림을 위해서 react native push notification 라이브러리가 필요한건가?

1. firebase 앱 등록
2. firebase 모듈 설치
3. firebase의 token 발급 후 서버에 저장
4. 발급받은 token을 통해 메세지 발신
5. 앱에서 메세지 수신 (수신받은 메세지를 어떻게 노출시킬지 처리)

할 일 목록

- [x] 푸시 알림 스케줄러 개발: **쿠폰 유효기간 만료 전 30일, 15일, 7일**
  - [x] 알림 시간은? !확인 필요!
  - [x] fcm token 업데이트 시점 확인
  - [ ] ~~fcm token을 userInfo 와 연결~~
- [ ] 푸시 알림 시 배지 (ios)
- [x] 알림 클릭시 이동하는 페이지 설정을 세밀하게 하기 위해 전송 데이터 포맷이 수정되어야할 것 같다.
  - [x] 전역스토어에서는 다음 페이지 이름만 저장하고 있다. 파라미터도 저장할 수 있도록 수정
  - [x] 푸시알림 클릭 시 페이지 이동하는지 확인 (파이어베이스 어드민, 일반 알림 전송, 주제 알림 전송, 스케줄러)

|            | ios |           android           |
| :--------: | :-: | :-------------------------: |
| foreground | ✅  |             ✅              |
| background | ✅  |             ✅              |
|    quit    | ✅  | 🔺 알림 o, 알림함 x, 진동 x |

=> ~~안드로이드에서 onMessage에는 찍히는데 알림함에 추가가 안됨~~ (리덕스 상에서 중복 공지를 막기 위한 조건문이 원인)
=> App component 밖, index.js 에서 setBackgroundHandler 를 지정해주어야함

- 알림함 내용은 전역 스토어로 관리되고 있음
- index에서는 전역 스토어 사용불가

알림함 관련 기능

- [x] 알림 추가
- [x] 알림 읽기
- [x] 알림 모두 읽기
- [x] 알림 삭제 (90일 이전의 알림 자동 삭제)
- [x] 안 읽은 알림 있는지 확인

## 참고

- [react native firebase 공식문서 | 초기세팅 및 remote notification 송수신](https://rnfirebase.io/messaging/usage)
- [react native firebase 공식문서 | 앱에서 알림 보여주기](https://rnfirebase.io/messaging/notifications)
- [react native 푸시알림 공식문서 따라하기 - 한 번 훑어보기 좋음. 전반적인 이해에 도움](https://velog.io/@kwonh/ReactNative-%ED%91%B8%EC%89%AC%EC%95%8C%EB%A6%BC-%EA%B3%B5%EC%8B%9D%EB%AC%B8%EC%84%9C-%EB%94%B0%EB%9D%BC%ED%95%98%EA%B8%B0-Firebase-Cloud-Messaging-react-native-firebase-notification-%EC%95%88%EB%93%9C%EB%A1%9C%EC%9D%B4%EB%93%9C)
- [혜지니 블로그 | [Android] FCM을 이용해 push 구현하기 - 안드로이드 native 코드만 있어서 키 발급 과정만 참고하면 좋음](https://maejing.tistory.com/entry/Android-FCM%EC%9D%84-%EC%9D%B4%EC%9A%A9%ED%95%B4-Push-%EA%B5%AC%ED%98%84%ED%95%98%EA%B8%B0)
- [꿀팀저장소 블로그 | ReactNative, 푸시 알림 완벽하게 구현하기](https://honeystorage.tistory.com/306)
- [react native firebase 공식문서 | device token 발급방법](https://rnfirebase.io/messaging/server-integration#device-tokens)

---

그냥 메모

- [라이브러리 react-native-push-notification은 로컬 알림 (앱 내부 자체에서 보내는 알림)을 위해서 필요하지만, 원격 알림을 위해서도 필요하다](https://github.com/zo0r/react-native-push-notification#if-you-use-remote-notifications) (진작 말하지)

푸시알림에 대한 use case는 두가지

1. 알림을 끄거나
2. 알림을 클릭해서 앱으로 들어오거나

이 둘을 구분하기 위해서는 ⬇️

```
const isClicked = notification.getData().userInteraction === 1;
```

오늘 한 것

- react-native-push-notification, @react-native-community/push-notification-ios 설치
- @react-native-community/push-notification-ios 환경 설정 - [여기부터](https://github.com/react-native-push-notification/ios#how-to-determine-push-notification-user-click) 다시 보면 됨
- react-native-push-notification은 [여기부터 (사용법)](https://github.com/zo0r/react-native-push-notification#usage)

---

react-native-push-notification 라이브러리가 아닌 다른 라이브러리를 쓸 수도 있다.
react-native-push-notification 라이브러리가 가장 자료가 많기는 한데 공식문서에는 이제 유지를 멈춘다고 공표되어 있고, 때문에 다른 라이브러리를 추천하고 있기도 하다.
그 중 하나가 onesignal

- [onesignal 공식문서](https://github.com/OneSignal/react-native-onesignal)
- [LogRocket | Implement push notifications in React Native with OneSignal](https://blog.logrocket.com/implement-push-notifications-react-native-onesignal/)
- [medium | OneSignal 리엑트네이티브 sdk 설치 방법 및 간단한 사용법](https://medium.com/crossplatformkorea/onesignal-%EB%A6%AC%EC%97%91%ED%8A%B8%EB%84%A4%EC%9D%B4%ED%8B%B0%EB%B8%8C-sdk-%EC%84%A4%EC%B9%98-%EB%B0%A9%EB%B2%95-%EB%B0%8F-%EA%B0%84%EB%8B%A8%ED%95%9C-%EC%82%AC%EC%9A%A9%EB%B2%95-6a7fd1058ee7)

---

react-native-firebase 라이브러리는 remote message를 보낼 때 사용된다.
공식문서에는 해당 라이브러리를 통해 알림이 가는 경우를 이렇게 정리해놓았다.
[image]
ㅓ어
=> 띵동! 하고 울리는 기기 알림은 앱이 켜진 상태에서 가지 울리지 않는다
=> 앱이 켜진 상태에서도 알림이 가길 원한다면

- 앱이 켜진 상태에서 알림 이벤트를 감지하여 (.onMessage)
- local notification을 한 번 더 수신하는 것으로 구현할 수 있다.
- local notification 구현을 위해 추천하는 라이브러리로는 [notifee](https://notifee.app/) 가 있고,
- 파이어베이스의 in-app-messaging도 비슷한 역할을 하는 것 같다만, 어느정도 한계가 있는 것 같다.

  > FCM provides support for displaying basic notifications to users with minimal integration required. If however you require more advanced notifications we recommend using our separate local notifications package 'Notifee'.

---

지금은 푸시 알림 객체의 data 필드를 통해 알림 클릭시 이동할 화면 전달
-> 알림 클릭시
-> 푸시 알림 수신 이벤트 핸들러는 navigator 밖에 존재하기 때문에 navigationRef로 화면 이동
or 전역 스토어에 다음 화면 정보 저장 -> 핀로그인 이후 해당 화면으로 이동

그런데 react native navigation 공식문서에는 push notification이 이유라면 navigation ref가 아닌 다른 방법을 권하고 있었다.
그게 바로 link.
external link 혹은 react native deep link를 사용하는 것

---

관련 추가 보완 기능

- 인증 프로세스
- deep link
- 공유하기
- 친구 추천 (친구가 만든 링크를 통해 앱 설치 or 가입)
- 연동시 dynammic link 사용 및 통계
  - 다이나믹 링크를 통해 어떤 제휴사를 통해 사용자가 유입됐는지 알 수 있음
