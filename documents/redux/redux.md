# Redux

- 상태 관리 라이브러리

## 장점

- 컴포넌트 코드로부터 상태 관리 코드를 분리 할 수 있다.

- 미들웨어를 활용한 다양한 기능 추가

  - ex) redux-saga
  - 로컬 스토리지에 데이터 저장, 불러오기

- SSR 시 데이터 전달이 간편하다 (하나의 객체로 관리하기 때문)

- 리액트 콘텍스트보다 효율적인 랜더링이 가능 (조건부)

## 리덕스의 구성 요소

액션 > 미들웨어 > 리듀서 > 스토어
(액션) < 뷰 < (스토어)

- 뷰가 값을 변경 하고 싶으면 액션을 발생시킴
  -> 그 액션을 미들웨어가 처리
  -> 미들웨어 처리가 끝나면 리듀서로 넘어가고 해당 액션이 상태값을 어떻게 변경하는지 로직을 수행, 수행이 끝나면 새로운 상태값을 반환
  -> 스토어가 상태값을 받아서 저장
  -> 액션 처리가 끝나면 옵저버들에게 알려주고 뷰가 그 값을 갱신

### 액션

- type 속성 값을 가지고 있는 객체
  - type 속성은 유니크 해야 함. (액션을 구분하기 위해)
    - prefix를 붙여서 많이 사용. (ex: todo/ADD 같이 todo/ 를 붙임)
  - type 속성 이외에도 필요한 값을 전달 가능

```javascript
store.dispatch({ type: "todo/ADD", title: "영화 감상", priority: "high" });
store.dispatch({ type: "todo/REMOVE", id: 123 });
```

- 위 처럼 액션 객체를 직접 입력 하기 보다는 action creator 함수를 만들어서 사용한다. (각 액션 객체의 구조를 일관성 있게 만들기 위해)

```javascript
function addTodo({ title, priority }) {
  return { type: "todo/ADD", title, priority };
}

store.dispatch(addTodo({ title: "영화 감상", prioity: "high" }));
```

- action type은 reducer 에서도 사용하기 때문에 상수로 만드는게 좋다.

```javascript
export const ADD = 'todo/ADD';
...

export function addTodo({ title, priority }) {
  return { type: ADD, title, priority };
}
```

### middleware

- 액션이 dispatch 되어서 reducer를 처리 하기 전에 수행되는 작업. (사전 등록 필요)

```javascript
const myMiddleware = (store) => (next) => (action) => next(action);
// action 이후의 함수에서 store와 next를 사용하기 위해 이런 식으로 작성.
```

- 액션이 발생되면 미들웨어부터 처리되고 reducer가 처리된다.
  - 미들웨어가 여러개라면 next 함수를 통해 순차적으로 미들웨어가 불러지고 마지막 미들웨어가 next를 호출하면 reducer가 호출된다.

```javascript
const middleware1 = (store) => (next) => (action) => {
  console.log("middleware1 start");
  const result = next(action);
  console.log("middleware1 end");
  return result;
};
const middleware2 = (store) => (next) => (action) => {
  console.log("middleware2 start");
  const result = next(action);
  console.log("middleware2 end");
  return result;
};
const myReducer = (state, action) => {
  console.log("reducer");
  return state;
};
const store = createStore(myReducer, applyMiddleware(middleware1, middleware2));
store.dispatch({ type: "someAction" });

// 결과
// "reducer"     # 상태 값을 초기하기 위해 호출된 reducer
// middleware1 start
// middleware2 start
// "reducer"
// middleware1 end
// middleware2 end
```
