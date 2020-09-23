# Component

- Component는 Class Component와 Function Component 로 나뉜다.

- state가 필요하고 상태에 따라 컴포넌트가 주기적으로 업데이트 되어야 한다면 Class Component, state가 필요 없고 정적으로 표시한다면 Function Component로 간단히 만들 수 있다. (React 16.8 이전)

## Class Component

- state Object를 가지고 있음.
- Life cycle method를 가지고 있음.

## Function Component

- state와 life cycle가 없음.
  - 단, React 16.8에서는 React Hook이 도입되어 state와 life cycle를 사용 할 수 있게 됨.

## Naming

- Component도 JavaScript 이므로 .js를 붙이면 순수 JavaScript와 구분하기 힘들다. 파일명은 소문자로 시작하고 확장자는 .jsx를 붙이면 구분하기 쉽다.
  - app.jsx
