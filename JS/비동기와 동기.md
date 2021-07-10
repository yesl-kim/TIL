# 동기와 비동기

> [자바스크립트 비동기 처리와 콜백 함수](https://joshua1988.github.io/web-development/javascript/javascript-asynchronous-operation/)  
> [poiemaweb | 동기식 처리모델 vs 비동기식 처리모델](https://poiemaweb.com/js-async)

<br/>

### 자바스크립트는 동기적 언어

> 직렬적, 순차적 수행, 단일 스레드

동기식 처리모델은 직렬적으로 작업을 수행한다. 즉 코드가 작성된 순서대로 실행되며 특정 작업이 수행중일 때에는 다음 작업은 대기하게 된다. (단일 스레드)

- 단일 스레드란?  
  스레드란 프로세스 내에서 프로그램 명령을 실행하는 기본 단위  
  스레드 ID, 프로그램 카운터, 레지스터 집합, 스택으로 구성  
  **단일 스레드는 하나의 프로세스에서 하나의 스레드를 실행**  
  하나의 레지스터와 스택으로 표현

![동기식 처리 모델](https://poiemaweb.com/img/synchronous.png)

### 비동기식 처리 모델?

> 병렬적, 비순차적 수행

**특정 로직의 실행이 끝날 때까지 기다려주지 않고 나머지 코드를 먼저 실행하는 것**

서버와 통신을 할 때 주로 비동기 처리가 일어나는데, 서버에서 응답이 오기까지 얼마나 많은 시간이 소요될지 모르기 때문에 응답이 오는 동안 다음 로직을 실행하는 것이다.

자바스크립트의 대부분의 DOM 이벤트 핸들러와 Timer 함수(setTimeout, setInterval), Ajax 요청은 비동기식 처리 모델로 동작한다.

이때 setTimeout의 콜백함수는 즉시 실행되지 않고 지정 대기 시간만큼 기다리다가 “tick” 이벤트가 발생하면 태스크 큐로 이동한 후 Call Stack이 비어졌을 때 Call Stack으로 이동되어 실행된다.

![비동기식 처리 모델](https://poiemaweb.com/img/asynchronous.png)
