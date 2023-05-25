# [testing-library/user-event](https://testing-library.com/docs/user-event/intro)

- tesing library의 fireEvent보다 좀 더 사용자 상호작요과 가까운 이벤트 메소드를 제공
- fire event 가 컴퓨터의 이벤트를 일으키는 반면
- user event는 실제 사용자가 이벤트를 일으키는 것처럼 동작한다. fire event 보다 더 복합적인 이벤트를 일으키낟.

## setup

- user event를 사용할 때는 setup 메소드를 통해 반환되는 user 객체로 이벤트를 트리거시킨다.
- userEvent는 항상 promise를 반환한다는 것에 주의!

```js
const user = userEvent.setup()
await user.click(...)

```
