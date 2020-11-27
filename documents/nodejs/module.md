# Module

## Module Exports

```javascript
// 함수를 exports 하는 경우

function edit() {}
function write() {}

// 단일 함수를 보내는 경우 (클래스나 변수도 가능)
module.exports = edit;

// 여러 함수를 보낸다면 객체 형태로
module.exports = {
  edit, // 함수명과 exports 명이 같다면 한 번만 입력하면 된다.
  write, // write: write 같이 작성하지 않아도 됨.
  fn: () => console.log("fn test"),
};
```

## Module Require

```javascript
// var.js
const odd = "odd";
const even = "even";

module.exports = {
  odd,
  even,
};
```

```javascript
// func.js
const { odd, even } = require("./var"); // .js 는 생략 가능
function checkOdd(num) {
  if (num % 2) {
    return odd;
  }
  return even;
}

module.exports = checkOdd;
```
