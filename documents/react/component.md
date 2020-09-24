# Component

- Component는 Class Component와 Function Component 로 나뉜다.

- state가 필요하고 상태에 따라 컴포넌트가 주기적으로 업데이트 되어야 한다면 Class Component, state가 필요 없고 정적으로 표시한다면 Function Component로 간단히 만들 수 있다. (React 16.8 이전)

## Class Component

- state Object를 가지고 있음.
- Life cycle method를 가지고 있음.

## Function Component

- state와 life cycle가 없음.
  - 단, React 16.8에서는 React Hook이 도입되어 state와 life cycle를 사용 할 수 있게 됨.

### memo

- 렌더링 결과를 메모이징(memoizing) 함으로써 불필요한 리렌더링을 하지 않음.
- 성능 최적화를 위해 사용

```javascript
const MyComponent = React.memo(function MyComponent(props) {
  /* props를 사용하여 렌더링 */
});
```

## Naming

- Component도 JavaScript 이므로 .js를 붙이면 순수 JavaScript와 구분하기 힘들다. 파일명은 소문자로 시작하고 확장자는 .jsx를 붙이면 구분하기 쉽다.
  - app.jsx

# Pure Component

- shouldComponentUpdate 가 이미 구현되어 있고 props와 state를 가볍게(shallow) 비교 한 뒤 변경이 있는 경우에만 리렌더링 한다.
  - Object 형태의 state나 props가 있는 경우, 그 안의 value가 변경되면 Pure Component는 동일 하다고 판단.
    - habit = [{ name: 'yang', age: 2}] 같은 형태의 props가 전달된다면, age가 5로 바뀌어도 Object의 형태는 그대로이므로 변경이 없다고 판단 함.
    - 이런 경우, habit = [{ name: 'yang', age: 2}] 이 아닌, habit.age 같은 형태로 props를 보내면 age는 reference 형태가 아니므로 값을 바뀐 걸 눈치 챔.

```javascript
// PureComponent 를 import
import React, { PureComponent } from "react";

// PureComponent 를 extend
class HabitAddForm extends PureComponent {
  ...
}
```
