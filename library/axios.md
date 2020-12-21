# Axios

HTTP 비동기 통신 라이브러리

- https://github.com/axios/axios

## 특징

- 브라우저(XMLHttpRequests), node.js(Http) 요청을 만들 수 있다.
- Promise(ES6) API 사용
- 요청과 응답을 JSON 형태로 자동 변경

## 설치

```
// Yarn
yarn add axios

// NPM
npm install axios
```

## 예제

```javascript
// GET
const axios = require("axios");

axios.get("/user?ID=12345");

axios.get("/user", {
    params: {
        ID: 12345,
    });
    // then, catch ...

// POST
axios.post("/user", {
  firstName: "Fred",
  lastName: "Flintstone",
});
// then, catch ...
```

##
