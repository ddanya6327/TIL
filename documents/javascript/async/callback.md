# Callback function

- 함수의 실행이 끝나고 난 뒤 실행되는 함수
- 비동기 처리를 위한 방법

## Synchronous callback (동기 콜백)

- 즉각적으로 실행

```javascript
console.log("1");
setTimeout(() => console.log("2"), 1000);
console.log("3");

function printImmediately(print) {
  print();
}
printImmediately(() => console.log("hello"));

/*
출력 결과
1
3
hello
2
*/
```

## Asynchronous callback (비동기 콜백)

- 언제 실행 될지 모름

```javascript
console.log("1");
setTimeout(() => console.log("2"), 1000);
console.log("3");

function printImmediately(print) {
  print();
}

function printWithDelay(print, timeout) {
  setTimeout(print, timeout);
}

printImmediately(() => console.log("hello"));
printWithDelay(() => console.log("async callback"), 2000);

// 결과
// 1
// 3
// hello
// 2
// async callback
```

- callback 안에 callback 을 너무 많이 넣게 되면 가독성이 떨어지고 유지보수가 어려워지는 callback hell이 발생 할 수 있다.
