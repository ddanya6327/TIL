# Node.JS

## 일반(FrontEnd)의 JavaScript와의 차이점

- Node.js 에서 글로벌 객체는 Window가 아닌 Global 이다.
- 외부의 module을 사용하려면 require를 사용한다. (node에서 별도로 셋팅하지 않는 이상 import가 아니라 require)

## REPL

- terminal 에서 `node` 를 입력
- 특정한 객체에 대한 정보가 필요 할 때 사용하거나 한 줄 단위로 실행하고 싶을 때 사용.
  - 예를 들어, String 객체에 대해 알고 싶다면 `String.`까지 입력하고 Tap키를 2번 누른다.

## npm

```javascript
// npm 초기화
npm init [패키지명]

// 모듈 설치 (install)
npm i [패키지명]
// global install ) npm install -g [패키지명]

// - 해당 패키지를 설치하고 package json 에 추가
npm install [패키지명] --save-dev

// 이전에 설치한 패키지를 업데이트 할 때
npm update

// -해당 패키지 삭제
npm uninstall [패키지명]
npm uninstall -g [패키지명]
```

## npx

- npm으로 설치한 패키지를 실행할 때 사용하거나 설치하지 않고 이례적으로 사용해 보고 싶을 때 사용. (설치하지 않고 실행, 1회성)

## nodemon

- 파일의 변경이 있다면 저절로 재실행 되도록 도와주는 패키지.

```
// 설치
npm i nodemon -g

// 실행
nodemon [app.js]
```

## module exports

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