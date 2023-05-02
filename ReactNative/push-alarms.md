# 리액트 네이티브 앱에서 푸시알림 보내기

- [ ] 기존 프로세스 파악 (사내 푸시 알림 설정이 어떻게 되어있는지)
- [x] 기존 환경설정 파악 (부족한 부분이 없는지)
- [ ] 개발
  - [ ] fcm token 업데이트 하는 시점 확인
  - [ ] user 데이터와 fcm token 연결
  - [ ] ios에서는 앱이 background, quit 상태에서 알림이 오지 않는 문제 (알림o, 진동x, 알림함x)
- [ ] 문서화 (알게된 것들, 백업 및 인수인계를 위해 필요한 배경지식, 설명 문서화)

## 특정 조건의 사용자에게 푸시알림 보내기

> **요구사항**
>
> 1. 전체 사용자가 아닌, 특정 조건에 부합하는 사용자에게만 알림을 보낸다
> 2. 앱이 꺼져있어도 알림이 울린다
> 3. 기기 알림, 앱 내의 알림함 두 곳에서 확인할 수 있다.

### Intro

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

3. 셋업: 사용자 데이터를 관리하고 알림을 보낼 수 있는 서버 필요 (예. 파이어베이스)
4. 푸시 알림 기능: 앱에서 푸시 알림을 보내도록 하는 기능은 react-native-push-notification 라이브러리 사용 (ios - @react-native-community/push-notification-ios)

## 참고

- [ ] [react native 푸시알림 공식문서 따라하기 - 한 번 훑어보기 좋음. 전반적인 이해에 도움](https://velog.io/@kwonh/ReactNative-%ED%91%B8%EC%89%AC%EC%95%8C%EB%A6%BC-%EA%B3%B5%EC%8B%9D%EB%AC%B8%EC%84%9C-%EB%94%B0%EB%9D%BC%ED%95%98%EA%B8%B0-Firebase-Cloud-Messaging-react-native-firebase-notification-%EC%95%88%EB%93%9C%EB%A1%9C%EC%9D%B4%EB%93%9C)
- [ ] [꿀팀저장소 블로그 | ReactNative, 푸시 알림 완벽하게 구현하기](https://honeystorage.tistory.com/306)
- [ ] [혜지니 블로그 | [Android] FCM을 이용해 push 구현하기 (키발급방법)](https://maejing.tistory.com/entry/Android-FCM%EC%9D%84-%EC%9D%B4%EC%9A%A9%ED%95%B4-Push-%EA%B5%AC%ED%98%84%ED%95%98%EA%B8%B0)
- [ ] [react native firebase 공식문서 | device token 발급방법](https://rnfirebase.io/messaging/server-integration#device-tokens)
