## Hoisting

- var, function declaration 선언들이 제일 위로 올라가는 것.

```javascript
// 선언 -> 호출 순서가 맞지만
function hello(word) {
  console.log(word);
}
say("hello");
```

```javascript
// 호출 -> 함수선언 순서로 해도 실행에는 문제가 없다.
// 그 이유가 호이스팅 때문
say("hello");
function hello(word) {
  console.log(word);
}
```

## DOM (Document Object Model)

- HTML, XML 문서의 프로그래밍 interface
- 페이지의 콘텐츠 및 구조, 스타일을 읽고 조작할 수 있는 API를 제공
- 트리 구조로 되어있어 DOM 트리 라고도 한다.

## CSSOM (CSS Object Model)

- CSS를 브라우저가 이해하고 처리 할 수 있는 형식으로 변환
- DOM와 마찬가지로 트리 구조로 되어있어 CSSOM 트리 라고도 부름

## Render Tree

- DOM Tree 에 CSSOM Tree를 적용하여 Render Tree 를 생성한다.
- 출력에 반영되지 않는 불필요한 노드들은 생략
  - ex) script, meta
  - `display: none` 처럼 출력에 반영되지 않는 노드들도 제외

## IIFE

- 정의와 동시에 즉시 실행되는 JavaScript 함수
  - 함수 리터럴을 `()`로 감싼 뒤 바로 실행하는 형태가 일반적
  - `()`로 묶어주지 않는다면, 함수 선언문으로 인식한다. (선언문의 경우 값을 출력하지 않으므로 선언식으로 해야됨) 그렇기 때문에 ()로 묶어서 함수 표현식이라는것을 인식시킴

```javascript
// IIFE
(function () {
  //
})();

(function (a, b) {
  return a + b;
})(1, 2);
```
