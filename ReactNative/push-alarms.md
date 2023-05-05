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

## 푸시 알림 getting started

#### 푸시알림 종류

1. 로컬 푸시 알림
   - 푸시 서버없이 앱 자체에서 알림을 보내는 기능
2. 원격 푸시 알림
   - 서버를 통해 알림을 보내는 기능
   - 주로 FCM (firebase cloude messaging) 도구 사용

#### usecase

푸시알림을 보내는 사용자 범위에 따라

1. 전체 사용자
2. 특정 주제를 구독한 사용자
3. 특정 사용자
   - device token 발급
   - token을 통해 메세지 수신

안드로이드, ios에 따라 다른 라이브러리가 필요

#### 모듈 설치

안드로이드

- @react-native-firebase/analytics
- @react-native-firebase/app
- @react-native-firebase/messaging

IOS

- react-native-push-notification
- @react-native-community/push-notification-ios

typescript

- @types/react-native-push-notification (dev dependency)

### 프로세스

일반적으로 푸시알림 서버를 사용하여 리모트 알림을 보냄
q. 로컬 푸시알림을 위해서 react native push notification 라이브러리가 필요한건가?

1. firebase 앱 등록
2. firebase 모듈 설치
3. firebase의 token 발급 후 서버에 저장
4. 발급받은 token을 통해 메세지 발신
5. 앱에서 메세지 수신 (수신받은 메세지를 어떻게 노출시킬지 처리)

## 특정 조건의 사용자에게 푸시알림 보내기

> **요구사항**
>
> 1. 전체 사용자가 아닌, 특정 조건에 부합하는 사용자에게만 알림을 보낸다
> 2. 앱이 꺼져있어도 알림이 울린다
> 3. 기기 알림, 앱 내의 알림함 두 곳에서 확인할 수 있다.

할 일 목록

- [x] 푸시 알림 스케줄러 개발: **쿠폰 유효기간 만료 전 30일, 15일, 7일**
  - [ ] 알림 시간은? !확인 필요!
  - [ ] fcm token 업데이트 시점 확인
  - [ ] fcm token을 userInfo 와 연결
- [ ] 푸시 알림 시 배지 (ios)

|            | ios |                    android                     |
| :--------: | :-: | :--------------------------------------------: |
| foreground | ✅  | ❌ onMessage에는 찍히는데 알림함에 추가가 안됨 |
| background | ✅  |          🔺 알림 o, 알림함 x, 진동 x           |
|    quit    | ✅  |          🔺 알림 o, 알림함 x, 진동 x           |

=> 안드로이드에서 onMessage에는 찍히는데 알림함에 추가가 안됨

## 참고

- [ ] [react native 푸시알림 공식문서 따라하기 - 한 번 훑어보기 좋음. 전반적인 이해에 도움](https://velog.io/@kwonh/ReactNative-%ED%91%B8%EC%89%AC%EC%95%8C%EB%A6%BC-%EA%B3%B5%EC%8B%9D%EB%AC%B8%EC%84%9C-%EB%94%B0%EB%9D%BC%ED%95%98%EA%B8%B0-Firebase-Cloud-Messaging-react-native-firebase-notification-%EC%95%88%EB%93%9C%EB%A1%9C%EC%9D%B4%EB%93%9C)
- [ ] [혜지니 블로그 | [Android] FCM을 이용해 push 구현하기 - 안드로이드 native 코드만 있어서 키 발급 과정만 참고하면 좋음](https://maejing.tistory.com/entry/Android-FCM%EC%9D%84-%EC%9D%B4%EC%9A%A9%ED%95%B4-Push-%EA%B5%AC%ED%98%84%ED%95%98%EA%B8%B0)
- [ ] [꿀팀저장소 블로그 | ReactNative, 푸시 알림 완벽하게 구현하기](https://honeystorage.tistory.com/306)
- [ ] [react native firebase 공식문서 | device token 발급방법](https://rnfirebase.io/messaging/server-integration#device-tokens)

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
