# Hook

- Function Component 에서도 상태값(state)이나 Life Cycle를 관리하거나 자식 요소에 접근 할 수 있도록 도와주는 함수

- useState() 를 통해 선언할 수 있다.

```javascript
function Example() {
  // useState 함수를 이용해서 state 변수와 state를 관리 할 수 있는 함수를 만듬.
  const [count, setCount] = useState(0);

  return (
    <div>
      // 함수형 컴퍼넌트는 this를 붙이지 않아도 된다.
      <p>You clicked {count} times</p>
      <button onClick={() => setCount(count + 1)}>Click me</button>
    </div>
  );
}
```

## useState 주의점

- 리액트는 상태값 변경을 일괄(batch)로 처리한다.
  - 내부에서 관리하는 이벤트의 경우. (2020/10 기준)

```javascript
function App() {
  const [count, setCount] = useState(0);
  function onClick() {
    setCount(count + 1);
    setCount(count + 1);
    // count + 1 이 2번 실행되어 count는 2가 될 것 같지만 1이 된다.
    // React는 상태값 변경을 batch로 처리하기 때문.
    // 단, 리액트 내부가 아닌 외부에서 처리 할 경우 batch로 처리하지 않는다.
  }
  console.log("render called"); // 1번만 호출된다.

  return (
    <div>
      <h2>{count}</h2>
      <button onClick={onClick}>증가</button>
    </div>
  );
}
```

- 상태값 변경 함수의 인자에 함수를 넣으면 정상적으로 작동

```javascript
function App() {
  const [count, setCount] = useState(0);
  function onClick() {
    setCount((v) => v + 1);
    setCount((v) => v + 1);
    // 위의 경우는 처리전에 값을 불러오므로 정상적으로 count는 2가 된다.
  }
  console.log("render called"); // 1번만 호출된다.

  return (
    <div>
      <h2>{count}</h2>
      <button onClick={onClick}>증가</button>
    </div>
  );
}
```

- 리액트 내부가 아닌 외부에서 처리 할 경우

```javascript
function App() {
  const [count, setCount] = useState(0);
  function onClick() {
    setCount((v) => v + 1);
    setCount((v) => v + 1);
  }
  useEffect(() => {
    window.addEventListener("click", onClick);
    return () => window.removeEventListener("click", onClick);
    // 이 경우 Log는 2번 호출된다.
  });
  console.log("render called");

  return (
    <div>
      <h2>{count}</h2>
      <button onClick={onClick}>증가</button>
    </div>
  );
}
```

## Function Component 주의점

- Hook은 function component에 사용하는 함수.
- Function Component의 경우, Class Component 와 다르게 매 호출마다 코드 블럭 전체가 반복해서 호출된다.

Class Component

```javascript
class Habit extends Component {
    // 클래스 컴퍼넌트의 경우, 내장 함수들이 클래스 생성시 딱 한번만 만들어짐.
    handleIncrement = () => {
        ...
    }

    handleDecrement = () => {
        ...
    }

    // state가 변경 되더라도 render만 반복적으로 호출.
    render() {
        return (
            ...
        )
    }
}
```

Function Component

```javascript
const Habit = props => { // { } 블록의 내용 전부가 반복해서 호출 됨.
    const [count, setCount] = useState(0);

    // 매 호출시(값 변경시) handleIncrement는 계속 새로운 함수가 생성됨.
    const handleIncrement = () => {
        ...
    };

    return (
        ...
    );
}
```

- 매번 새롭게 호출 되지만, useState를 통해 만든 변수는 메모리에 저장해둠.

  - 그래서 매번 만들어도 같은 값을 사용 가능.

- ref를 사용하고 싶은 경우 아래처럼 사용함.

```javascript
const Habit = props => { // { } 블록의 내용 전부가 반복해서 호출 됨.
    const [count, setCount] = useState(0);

    // const spanRef = React.createRef(); 이렇게 사용하면 매 번 새로운 Ref를 만듬
    const spanRef = useRef(); // 1번만 Ref를 만들고 메모리에 저장.

    // 매 호출시(값 변경시) handleIncrement는 계속 새로운 함수가 생성됨.
    const handleIncrement = () => {
        ...
    };

    return (
        ...
    );
}
```

## useCallback

- 자식 컴퍼넌트에 콜백 함수를 보낸다면?
  - Function Component는 매 호출시 새롭게 생성하므로 자식에게 보내는 함수도 새로운 것이라고 인식함.

```javascript
const Habit = props => { // { } 블록의 내용 전부가 반복해서 호출 됨.
    const [count, setCount] = useState(0);

    const handleIncrement = () => {
        ...
    };

    return (
        // 이 경우, 매번 handleIncrement를 생성해서 전달 하므로, 자식들은 매 번 부모의 값이 갱신 되었다고 생각함
        <button onClick={handleIncrement}>Button</button>
    );
}
```

```javascript
const Habit = props => { // { } 블록의 내용 전부가 반복해서 호출 됨.
    const [count, setCount] = useState(0);

    // useCallback 함수를 사용하면 해당 함수르 생성해서 메모리에 올려두고 같은 함수를 보내게 됨.
    const handleIncrement = useCallback(() => {
        ...
    });

    return (
        <button onClick={handleIncrement}>Button</button>
    );
}
```

## useEffect

- componentDidMount 와 componentDidUpdate를 결합한 함수

- useEffect(function, [값]);
  - function : 부수효과 함수
  - [값] : 의존성 배열, 배열의 값이 변경되면 부수효과 함수가 실행됨
    - [] : 빈 배열의 경우 mount 되는 한 번만 실행 (unmount 될 때도 실행)

```javascript
// component가 mount 되거나 update 될 때 실행됨
useEffect(() => {
  console.log(`mounted & Updated!`);
});

// count, name 값이 바뀔 때 만 호출
useEffect(() => {
  console.log(`mounted & Updated!`);
}, [count, name]);

// 처음에만 호출
useEffect(() => {
  console.log(`mounted & Updated!`);
}, []);
```

## 그 외 내장 훅

- useMemo
- useCallback
  - 함수 memoization에 특화된 코드
  - 의존성 배열로 관리
- useReducer
  - 여러 개의 상태 값을 관리 할 때에 좋음.
  - https://ko.reactjs.org/docs/hooks-reference.html#usereducer
- useImperativeHandle
  - ForwardRef 와 함께 사용
  - 모에게 꼭 자식의 실제 reference를 보내지 않고 우리가 원하는 일종의 proxy reference를 보내는게 가능해짐
- useLayoutEffect
  - useEffect 훅과 거의 비슷하게 동작하지만 부수효과 함수를 동기로 호출함.
  - 랜더링 직후에 돔 요소 값을 읽거나 조건에 따라 컴포넌트를 재 랜더링 하고 싶은 경우 적합.
    - 그렇기 때문에 useLayoutEffect 훅에서 연산을 많이 하면 브라우저가 멈춰 보일 수 있음.
    - 특별한 이유가 없다면 useEffect를 사용.
- useDebugValue
  - 리액트 개발자 도구에서 편리하게 디버깅 할 수 있도록 사용하는 훅

### TIP

- function component 에서 부모의 속성 값(props)는 비구조화 할당을 하면 쓰기 편함

```javascript
function Title(props) {
  return <p>{props.title}</p>;
}
```

비구조화 할당의 경우

```javascript
function Title({ title })) {
    return <p>{title}</p>; // title에 props를 붙이지 않아도 됨
}
```

- 부모의 값이 바뀌면 자식 컴포넌트도 자동으로 render 되는데, 자식의 속성값이 바뀐 경우에만 render 되도록 하고 싶다면 React.memo를 사용하자.
  - [React Memo에 대해](./component.md)

### 주의점

- Hook은 컴포넌트의 최상위(the top of level)에서 호출한다.
  - 또한, React의 Hook은 호출되는 순서에 의존하기 때문에 호출하는 순서는 항상 같아야 한다.
  - if, for 같이 조건문이나 반복문 내부에 Hook을 사용하면 안 된다. 호출되는 횟수가 달라 질 수 있기 때문

```javascript
if (!user) {
  return null;
}
const [value, setValue] = useState();
// 이 경우도 user 정보가 없다면 return 되므로 Hook의 호출 횟수가 달라 질 수 있다. 그러므로 Hook은 항상 최상위에 호출 하는 것이 좋다.
```

- React 함수 안에서만 Hook을 호출 해야 한다.
  - React의 함수 컴포넌트에서 호출
  - Custom Hook 에서 호출

[Hook 규칙](https://ko.reactjs.org/docs/hooks-rules.html)
