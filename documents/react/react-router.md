# React Router

- 하나의 페이지에서 동적으로 화면을 업데이트 해서 보여주는 SPA의 특성상, 특정 화면을 북마크 하거나 브라우저 상의 네비게이션의 관리가 어렵다. (페이지 안에서만 동작이 일어나기 때문) 위와 같은 문제를 보완하기 위해 만들어진 라이브러리

- https://reactrouter.com/web/guides/quick-start

```javascript
<BrowserRouter>
  <nav>
    <Link to="/">Home</Link>
    <Link to="/profile">Profile</Link>
  </nav>

  <Switch>
    <Route path="/home">
      <Home />
    </Route>
    <Route path="/profile">
      <Profile />
    </Route>
  </Switch>
</BrowserRouter>
```

- Router의 path의 값은 배열을 사용해 여러 주소를 사용할 수 있다.

```javascript
...
<Route path={["/", "/home"]}>
...
```

- 다만, 위와 같은 경우 일부만 일치해도 값이 적용 되므로 /가 들어간 모든 주소가 home을 보여주게 된다.

  - `<Route path="/profile">` 의 경우도 /가 포함되므로 home을 보여줌.

- 이런 경우, `exact` 속성을 줘서 완전히 일치하는 경우에만 라우팅 되도록 설정할 수 있다.

```javascript
...
// '/' 또는 'home' 일 경우에만 일치
<Route path={["/", "/home"]} exact>
...
```

## History

- React Hooks에 제공되는 라이브러리들
  - https://reactrouter.com/web/api/Hooks

## 주의점

```javascript
// bad
<Route path="/home" component={Home} />

// good
<Route path="/home">
    <Home />
</Route>
```

- 과거에는 위와 같은 방법을 사용하기도 했지만, 현재의 react는 이런 식으로 코드를 작성하면 기존의 component를 삭제하고 route 코드에서 component를 재 랜더링 하게 된다. 이렇게 하면 성능상으로도 좋지 못하고 화면이 깜빡이는 효과가 나타날 수 있으니 현재는 이렇게 사용하지 않는 편이 좋다.
