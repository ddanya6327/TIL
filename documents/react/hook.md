# Hook

- Function Component 에서도 상태값이나 Life Cycle를 관리 할 수 있도록 도와주는 함수

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
