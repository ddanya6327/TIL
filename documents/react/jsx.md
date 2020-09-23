# JSX

- JavaScript XML

  - JavaScript 확장 문법
  - HTML 같은 문법으로 작성하고 babel-loader 를 통해 JavaScript로 변환한다.

- 변수나 함수를 쓰고 싶다면 `{}` 를 이용한다.

```javascript
function App() {
  const name = "yang";
  return <h1>hello! {name}! </h1>;
}
```

- JSX는 다수의 태그를 return 할 수 없다.
  - 형제 노드 사용이 불가능하다.

```javascript
// 다음 코드는 Error가 발생
function App() {
    const name = 'yang';
    return <h1>hello! {name}! </h1><h1>형제 노드</h1>;
}
```

- 2개 이상의 노드를 반환하고 싶다면 div 태그로 묶어주거나 React.Fragment를 이용하면 된다.

```javascript
// React.Fragment 를 이용해서 묶을 수 있다.
function App() {
  const name = "yang";
  retrun(
    <React.Fragment>
      <h1>hello! {name}!</h1>
      <h1>형제 노드</h1>
    </React.Fragment>
  );
}

// React.Fragment는 생략 가능. 아래의 코드는 위와 똑같음.
function App() {
  const name = "yang";
  retrun(
    <>
      <h1>hello! {name}!</h1>
      <h1>형제 노드</h1>
    </>
  );
}
```

- 비즈니스 로직도 이용 가능

```javascript
function App() {
  const name = "yang";
  retrun(
    <>
      {name && <h1> {name} </h1>}
      {[1, 2].map((item) => (
        <h1>{item}</h1>
      ))}
    </>
  );
}
```

https://reactjs.org/docs/jsx-in-depth.html
