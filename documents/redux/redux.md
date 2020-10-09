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

### Action

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

### Middleware

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

### Reducer

- 액션이 발생 했을 때, 새로운 상태 값을 만드는 함수

  - redux 에서 상태 값을 수정할 때는 action 객체와 함께 dispatch를 호출해야 한다.

  - 상태 값은 불변 객체로 관리한다.

```javascript
// 리덕스가 처음 실행 될 때, state에 undifined를 넣어서 reducer를 호출한다.
function reducer(state = INITIAL_STATE, action) {
    switch (action.type) { // action 객체의 type에 따라 해당하는 action 에 대한 처리를 해주면 됨.
        // ...
        case REMOVE_ALL:
            return {
                ...state,
                todos: [],
            };
             // 데이터는 불변 객체로 관리 해야한다.
        case REMOVE:
            ...
        default:
            return state;
    }
}

const INITIAL_STATE = { todos: [] };
```

- reducer 코드 작성시 주의점

1. reference 값이 아닌 id 값으로 참조하기

```javascript
switch (action.type) {
  case SET_SELECTED_PEOPLE:
    // 이 경우, draft.selectedPeople 에는 find로 찾은 reference 값이 들어 있기 때문에 EDIT_PEOPLE_NAME 에서 수정이 실행되어도 draft.selectedPeople는 옛날 reference 값을 가지고 있어서 문제가 될 수 있다.
    draft.selectedPeople = draft.peopleList.find(
      (item) => item.id === action.id
    );
    // 그래서 주소가 아닌 id 값만 기억 해서 다른 곳에서 활용하는 식으로 하는게 좋다.
    //draft.selectedPeople = action.id;
    break;
  case EDIT_PEOPLE_NAME:
    const people = draft.peopleList.find((item) => item.id === action.id);
    people.name = action.name;
    break;
  // ...
}
```

2. 순수 함수로 작성하기 (부수 효과가 없어야 함.)

- reducer 에서 서버 api를 호출한다면 매 호출시 결과가 달라 질 수 있으므로 순수함수가 아니다.
- random 함수를 사용하면 입력이 같아도 출력이 다를 수 있기 때문에 순수 함수가 될 수 없다. (time 함수도 마찬가지)

  - random 값이 필요하다면 action 객체를 호출 할 때 만들어서 넣어주는게 좋다.

### Store

- state를 저장하거나 액션 처리 완료를 외부에 알려주는 역할

```javascript
// createStore 함수를 이용하고 reducer를 파라메터로 받음
const store = createStore(reducer);

// 액션 처리가 끝난걸 알기 위해서는 store의 subscribe 메소드를 사용
store.subscribe(() => {
  console.log("action end");
});
```
