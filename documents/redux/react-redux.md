# React Redux

- React용 Redux

- Provider 컴포넌트를 이용해서 store를 전달함.
  - provider에서는 리액트에서 액션이 처리 됐을 때 이벤트를 받아서 하위의 컴포넌트들이 랜더링 될 수 있도록 도와줌
  - 보통 root 컴포넌트에서 전달

```javascript
export default function App() {
  return (
    <Provider store={store}>
      <div>...</div>
    </Provider>
  );
}
```

## useSelector

- Redux에서 데이터를 가져 올 때에는 useSelector 훅을 사용.

```javascript
export default function FriendMain() {
  const friends = useSelector((state) => state.friend.friends);

  // 여러 값을 받아 올 때
  const friends2 = useSelector((state) => state.friend.friends2);
  const friends3 = useSelector((state) => state.friend.friends3);
}
```

### 여러 개의 state를 받아오는 경우

1. 배열 형태로 받아오기

```javascript
...
const [friends, friends2] = useSelector(state => [state.friend.friends, state.friend.friends2]);
...
```

- 단, 위와 같이 사용하면 배열이 매번 새로 생성되므로 불필요한 랜더링이 발생할 수 있다.
  - 2번째 매개변수에 함수를 입력해서 랜더링을 할지 말지 결정 할 수 있다.

```javascript
// react-redux에서 제공하는 shallowEqual 을 이용하는 경우
import { useSelector, shallowEqual } from 'react-redux';
...
const [friends, friends2] = useSelector(state => [state.friend.friends, state.friend.friends2], shallowEqual);
// 배열 내부의 값을 얕게 비교 함.
// 단, 하나만 반환 하는 경우에 내부의 모든 값을 비교하므로 하나만 반환 하더라도 배열의 형태로 반환 하는게 좋음. [friends]
...
```

2. 메모이제이션을 이용한 방법 (ex: reselect 패키지)

## dispatch 호출 방법

- 기존에는 store.dispatch(action) 의 형태로 호출했지만, useDispatch(); 훅을 이용하면 됨.

```javascript
const dispatch = useDispatch();
```
