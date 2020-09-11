# Function

## 함수 정의에 대해

1. Function declaration (함수 정의)

function name(param1, param2) { body ... return; }

- 하나의 함수는 한 가지의 일만 하도록 만들자.
- 함수의 이름은 무언가를 하는 일이기 때문에 동사나 커맨드 형태로 짓는게 좋다.
  - 예를 들어, createCardAndPoint 라는 함수가 있다면, 2가지의 기능을 하고 있으니 createCard, createPoint 같이 2개로 나누어 만들 수 있으면 좋을 듯. 그러면 깨끗한 함수가 만들어진다.
- JavaScript에서 function은 object이다.

```javascript
function log(message) {
  console.log(message);
}

log("Hello");
```

2. Parameters

premitive parameters: passed by value
object parameters: passed by reference

```javascript
function changeName(obj) {
  obj.name = "coder";
}

const yang = { name: "yang" };
changeName(yang);
console.log(yang); // yang.name = 'coder'
```

3. Default parameters (added in ES6)

사용자가 parameter를 입력하지 않으면 대체 값을 사용함.

```javascript
// ES5
function showMessage(message, from) {
  if (from === undefined) {
    from = "unknown";
  }
  console.log(message + from);
}

// ES6
function showMessage(message, from = "unknown") {
  console.log(message + from);
}
```

4. Rest parameters (added in ES6)

```javascript
function printAll(...args) {
  for (let i = 0; i < args.length; i++) {
    console.log(args[i]);
  }

  //   for (const arg of args) {
  //       console.log(arg);
  //   }
}

printAll("y", "a", "n", "g");
```

5. Local scope

- global (전역)
- local (지역)

```javascript
let globalMessage = "global"; // global variable
// 어디서든 참조 가능

function printMessage() {
  let message = "hello";
  console.log(message); // local variable
  console.log(globalMessage);
}
console.log(message); // block 밖이므로 참조 불가능. error
```

6. Return a value

- Return 을 명시하지 않은 함수는 암묵적으로 `return undefined` 를 하고 있다.

7. Early return, early exit

```javascript
// bad
function upgradeUser(user) {
  if (user.point > 10) {
    // long upgrade logic ...
  }
}

// good
function upgradeUser(user) {
  if (user.point <= 10) {
    return;
  }
  // long upgrade logic ...
}
```

## First-class Function

- 함수는 변수에 할당이 된다.
- 함수는 parameter로 전달된다.
- 함수는 Return 값으로 사용 가능하다.

1. Function Exprression (함수 표현식)

```javascript
const print = function () {
  // anonymous function
  console.log("print");
};

print(); // "print"

const printAgain = print;
printAgain(); // "print"
```

- function exprression 은 할당 된 후 호출이 가능하다.
  - function declaration (함수 선언) 의 경우, hoisting을 지원하므로, 선언 전에 호출 할 수 있다.

```javascript
print(); // 선언 전에 호출 가능. hoisting을 하기 때문.

function print() {
  console.log("print");
}

print_exprression(); // 선언 전이므로 호출 불가능.

const print_exprression = function () {
  console.log("print exprression");
};
```

2. Callback

```javascript
function randomQuiz(answer, printYes, printNo) {
  if (answer === "love you") {
    printYes();
  } else {
    printNo();
  }
}

// anonymous function
const printYes = function () {
  console.log("yes!");
};

// named function
// debugging 할 때 유용하다.
const printNo = function print() {
  console.log("no!");
};
randomQuiz("wrong", printYes, printNo);
randomQuiz("love you", printYes, printNo);
```

## Arrow function

- 간결한 형태의 함수 선언
- arrow function은 항상 anonymous 함수 형태

```javascript
const simplePrint = function () {
    console.log('simple');
}

const simple print = () => console.log('simple');

const add = (a, b) => a + b;
// 한 줄의 경우, 간결하게 쓸 수 있다.
// block을 사용하는 경우 return 키워드를 써야 한다.
```

## IIFE

- 정의와 동시에 실행되는 함수
- Immediately Invoked Function Expression

```javascript
(function hello() {
  console.log("IIFE");
})();
// 정의와 동시에 실행
```
