# Context

- 애플리케이션에서 전역적으로 사용되어야 할 데이터가 있을 경우 사용.

  - [React.Context](https://ko.reactjs.org/docs/context.html)

- 상위 컴포넌트에서 하위 컴포넌트로 데이터를 전달 할 때 속성 값을 사용한다. 가까운 거리면 속성 값으로도 충분하지만, 많은 수의 컴포넌트를 거처야 한다면 많은 컴포넌트를 통해 값을 전달해야 함.
  - C에서 데이터가 필요한데 A -> B -> C 형태로 값을 전달한다면 B는 사용하지도 않는 데이터를 오로지 전달을 위해 받아야 함. 이 경우, Context를 사용하면 B에서 불필요한 데이터를 넘겨주지 않고 C에서 바로 사용 가능.

## Context 사용시 주의점

1.

```javascript
return (
  <div>
    <UserContext.Provider value={{ username, age }}>
      <Profile />
    </UserContext.Provider>
  </div>
);
```

- 위와 같은 경우, 컴포넌트가 render 될 때 마다 새로운 객체가 생성되므로 consumer가 불필요하게 랜더링 될 수 있다.

```javascript
const [user, setUser] = createContext({ username: "mike", age: 23 });

return (
  <div>
    <UserContext.Provider value={user}>
      <Profile />
    </UserContext.Provider>
  </div>
);
```

- value를 하나의 객체로 관리해서 매번 render가 일어나지 않도록 하면 된다.

2.

```javascript
return (
    <div>
        <UserContext.Provider value="make">{ ... }</UserContext.Provider>
        <Profile />
        // 가끔 이렇게 consumer가 provider 밖에 위치하는 경우가 있는데 이 경우, consumer 는 provider를 찾을 수 없기 때문에 재대로 작동하지 않을 수 있다.
    </div>
)
```
