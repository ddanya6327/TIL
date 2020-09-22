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

## 주의점 (바인딩)

- JavaScript에서 클래스에 있는 함수를 매개변수로 전달할 때에는, 클래스 정보가 무시 되므로 바인딩을 해줘야 한다.

```javascript
class Field {
  constructor() {
    this.onItemClick = onItemClick;

    this.field.addEventListener('click', this.onClick);
    // onClick 함수를 전달하지만, onClick 함수는 내부적으로 this.onItemClick 을 사용하고 있다.
    // 그렇지만, 함수를 전달할 때 클래스의 정보(this)가 전달되지 않으므로 전달된 함수 onClick은 this.onItemClick를 찾을 수 없다.
  // 왜냐 하면, JavaScript에서 함수를 전달 할 떄에는 클래스 정보를 같이 전달하지 않기 때문. 이 경우 클래스 정보가 같이 전달 될 수 있도록 바인딩을 해줘야 한다.
  }

  ...

  onClick() {
    ...
    this.onItemClick && this.onItemClick('carrot');
  }
}
```

### 바인딩을 하는 방법

1. 직접 bind 해주기

```javascript
this.onClick = this.onClick.bind(this);
this.field.addEventListener("click", this.onClick);
// this.onclick은 this와 바인딩
```

- 단점 : 하나 하나 일일히 bind 해줘야 함.

2. Arrow Function 을 사용

```javascript
this.field.addEventListener("click", (event) => this.onClick(event));
```

3. 함수를 멤버 변수로 만들기

```javascript
this.field.addEventListener('click', this.onClick);

...

/*
onClick(event) {
  ...
}
*/

// onClick을 멤버 변수 형태로 만들기 (Arrow Function)
onClick = event => {
  ...
}
```
