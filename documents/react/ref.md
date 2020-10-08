# Ref

- React에서는 DOM 요소를 직접적으로 쓰지 않기 때문에, React의 요소에 접근하고 싶으면 Ref를 쓰면 된다.

https://ko.reactjs.org/docs/refs-and-the-dom.html

```javascript
class HabitAddForm extends Component {
    // Ref 생성
  formRef = React.createRef();

  ...

    render() {
    return (
       // form 태그에 생성한 formRef 를 연결
      <form ref={this.formRef} className="add-form" onSubmit={this.onSubmit}>
        ...
      </form>
    );
  }
}
```

## hook의 경우

1.

```javascript
export default function App() {
  const inputRef = useRef(); // useRef hook을 사용
  useEffect(() => {
    inputRef.current.focus();
  }, []);

  return (
    <div>
      // 반환된 hook을 원하는 값에 ref속성으로 입력
      <input type="text" ref={inputRef} />
      <button>저장</button>
    </div>
  );
}
```

- component에도 ref를 사용 할 수 있지만, 함수형 컴포넌트의 경우 인스턴스로 만들어지지 않지만 useImperativeHandle 이라는 훅을 사용해 접근 할 수 있다.

2. ref를 넘겨서 함수형 컴포넌트 내부에서 직접 설정 할 수도 있다.

- 단, ref라는 속성값으로 넘기면 리액트 내부에서 처리 하기 때문에 안 될 수 있음.

```javascript
export default function App() {
  const inputRef = useRef(); // useRef hook을 사용
  useEffect(() => {
    inputRef.current.focus();
  }, []);

  return (
    <div>
      <InputAndSave inputRef={inputRef} />
      <button onClick={() => inputRef.current.focus()}>텍스트로 이동</button>
    </div>
  );
}

function InputAndSave({ inputRef }) {
  return (
    <div>
      <input type="text" ref={inputRef} />
      <button>저장</button>
    </div>
  );
}
```

### 여러 요소의 Ref의 저장이 필요 할 때

```javascript
// 여러 Ref값을 저장 할 수 있다. ({}는 초기 값)
const boxListRef = useRef({});

return (
  ...
  {BOX_LIST.map(item => {
    <div
      ref={ref => (boxListRef.current[item.id] = ref)}
    >
  })}
)
```

## Ref 사용시 주의 점

- 컴포넌트가 생성 된 이후라도 ref객체가 없는 경우가 있을 수 있음.
  - 조건부 랜더링이 사용된 경우는 ref 객체를 검사 할 필요가 있음.
    - 예를 들어, a 요소를 focus 해주는 b라는 버튼이 있는데 a 요소를 잠시 제거하거나 숨겨둔 경우, b 버튼을 클릭하면 에러가 발생

```javascript
export default function App() {
  const inputRef = useRef();
  const [showText, setShowText] = useState(true);

  return (
    <div>
      {showText && <input type="text" ref={inputRef} />}
      <button onClick={() => setShowText(!showText)}>show text/hide</button>
      <button onClick={() => inputRef.current && inputRef.current.focus()}>
        {" "}
        // inputRef.current && 를 추가해서 값이 있는지 검사 focus text
      </button>
    </div>
  );
}
```

## Ref 객체의 활용법

- 랜더링과 관련 없는 값을 저장 할 때 useRef를 사용 할 수 있다.

  - 예를 들어, 이전 상태 값을 기억하고 싶을 때

- 아래의 예 처럼 이전 값을 받아와서 처리가 필요한 경우에 사용 할 수 있음.

```javascript
export default function App() {
  const [age, setAge] = useState(20);
  const prevAgeRef = useRef(20);
  useEffect(() => {
    prevAgeRef.current = age;
  }, [age]);
  const prevAge = prevAgeRef.current;
  const text = age === prevAge ? "same" : age > prevAge ? "older" : "younger";
  return (
    <div>
      <p>{`age ${age} is ${text} than age ${prevAge}`}</p>
      <button
        onClick={() => {
          const age = Math.fllor(Math.random() * 50 + 1);
          setAge(age);
        }}
      >
        나이 변경
      </button>
    </div>
  );
}
```
