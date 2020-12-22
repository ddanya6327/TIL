# Error Handling

express의 middleware에서 에러가 발생하면 express는 에러를 error handler로 보낸다.

```javascript
app.get("*", function (req, res, next) {
  // error가 발생하면 error handler 로 이동한다.
  throw new Error("test");
});

app.get("*", function (req, res, next) {
  // error handler가 아닌 middleware는 생략된다.
  console.log("skip");
});

app.use("*", function (error, req, res, next) {
  // 에러 처리기(error handler)는 인자가 4개다.
  // error handler는 마지막에 위치하는게 좋다.
  res.json({ message: error.message });
});
```

단, 비동기로 발생한 에러는 error handler에서도 처리 하지 못하고 서버가 정지하게 된다.

- JavaScript 환경은 비동기 처리를 하는 경우가 많기 때문에 문제가 될 수 있다.

## 비동기 에러 해결법

```javascript
app.get("*", function (req, res, next) {
  // 비동기로 발생한 에러지만 next에 넣은 상태로 발생시킨다면 error handler로 보내 줄 수 있게 된다.
  setImmediate(() => {
    next(new Error("test"));
  });
});

app.use("*", function (error, req, res, next) {
  // next 에 담긴 error는 검지 할 수 있다.
  res.json({ message: error.message });
});
```
