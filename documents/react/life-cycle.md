# Life Cycle

https://ko.reactjs.org/docs/state-and-lifecycle.html#gatsby-focus-wrapper

## componentDidMount

- 새로운 컴포넌트가 Mount 되었을 때 호출
  - 타이머라면 타이머를 시작하거나, 채팅이라면 소켓을 시작할 때 사용

## componentWillUnmount

- 컴포넌트가 unmount 되기 전에 호출
  - 타이머라면 타이머를 중지하거나, 채팅이라면 소켓을 종료할 때 사용
