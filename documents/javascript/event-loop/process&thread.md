# Process & Thread

## Process

- 운영체제 위에서 연속적으로 실행되고 있는 프로그램.

  - 독립적으로 메모리에서 실행 됨.
  - 운영체제 위에서는 여러개의 프로세스가 실행 될 수 있음.
  - 첫 번째 프로세스에 문제가 발생된다면, 첫 번째 프로세스만 종료 된다. (독립적)

- 프로세스는 각각 리소스를 가지고 있음.

  - 프로세스마다 할당 된 메모리나 데이터가 다름.

- Process
  - Code
    - 프로그램을 실행하기 위한 코드
  - Stack
    - 함수들의 실행 순서 같은 정보를 저장함 (동작순서를 기억)
  - Heap
    - Object 나 Data가 저장되는 공간 (동적 데이터)
  - Data
    - 전역 변수나 static 변수들이 할당

## Thread

- 프로세스 안에서 실행 될 수 있는 흐름의 단위
- 한 프로세스 안에서 여러개가 동작 할 수 있음.
- Thread 마다 stack 이 할당 되어 있음.
- process 의 code, heap, data를 공통적으로 접근해서 업데이트 할 수 있음.
  - process 1의 thread 1, thread 2가 process 1의 공통적인 데이터에 접근 가능하단 뜻.
  - 여러 thread가 하나의 process에 접근하기 때문에 멀티 쓰레딩의 경우 발생할 수 있는 문제들도 있음.

## JavaScript의 Event 처리

- JavaScript는 Single Thread Language 이다.
  - 다만, 웹 브라우저는 Multi Thread 이기 때문에, Web APIs를 이용해 다중 처리가 가능하다.

### JavaScript Runtime Environment

- 3초 뒤 실행되는 setTimeOut 함수를 호출 한다면?

  1. 일단 call stack에 setTimeout 함수가 등록 된다.
  2. setTimeout은 call stack에서 지워지고 Web API는 타이머를 시작한다.
  3. 타이머가 끝나면 Web APIs는 Task Queue에 timeout callback 을 전달함.
  4. 주기적으로 call stack과 task queue를 관촬하는 Event Loop가 task queue의 call back을 call stack에 전달함. (call stack이 비어져 있을 경우, 한 번에 1개의 call back 을 가져옴)

- Event loop는 call stack에서 수행중인 끝날때까지 다른 일들을 할 수 없다.
  - call stack 의 작업이 끝나야 task queue 의 작업을 수행 할 수 있다.
  - 그렇기 때문에, call stack 에서 작업이 굉장히 오래 걸린다면 task queue의 작업이 진행 되지 않을 수 있으므로 주의.

### Task queue

- 흔히 등록한 callback 이 등록 됨. (click event 라던지)
- 한 번에 한 개를 처리

### Microtask queue

- Promise에 등록된 callback, 즉 then 에 등록된 callback 들이 등록 됨.
- mutation observer 에 등록된 콜백도 Miscrotask queue에 등록됨.
- Microtask queue의 이벤트가 끝날 때 까지 처리됨.
  - 즉, Microtask queue의 이벤트가 진행중에 새로운 callback 이 발생한다면 그 것 까지 처리함. (task queue의 경우는 한 번에 1개지만, microtask queue 의 경우는 끝날 때 까지)

> Task queue에 자신을 호출하는 무한 재귀함수가 등록 된다 해도 1개씩 처리를 하다보니 Render가 실행된다. 즉, 화면이 멈추어 있지 않고 무한적으로 반복된다.

> Microtask Queue의 경우, 모든 이벤트가 처리 될 때 까지 진행되므로, 자신을 호출하는 무한 재귀함수(Promise)가 있다면 화면이 멈춘다. (재귀로 추가되는 callback 이 끝날 때 까지 실행하기 때문.)

### Render

- 브라우저에서 변형한 코드가 주기적으로 업데이트 되기 위해 호출되는 순서 (Event loop 가 순회 될 때마다 매번 실행 되지 않음)
- call stack 의 작업이 수행되고 있는 동안은 render 는 수행되지 않는다.
  - 즉, call stack의 작업이 무겁다면 브라우저는 업데이트 되지 않기 때문에 최대한 callback은 간단하게 작성하거나 끝나지 않는 for문이나 재귀함수는 주의 하는게 좋다.

### Request Animation Frame

- Render 가 발생하기 직전에 callback 함수를 실행
  - call stack에서 처리 하기 보다는, 브라우저가 화면을 업데이트 하기 전에 행동을 해야 한다면 Request Animation Frame 에 등록하는게 좋다.

### TIP

```javascript
button.addEventListener("click", () => {
  document.body.style.color = "red";
  document.body.style.color = "blue";
  document.body.style.color = "green";
  setTimeout(() => {}, 0);
});
```

- 위의 코드의 내용은 기본적으로 call stack 에서 다 실행 되지만, 이벤트 루프가 call stack을 벗어 났을 때, 실행하고 싶은 코드가 있다면 setTimeout (0ms) 를 이용할 수 있다.
  - call stack 에서 setTimeout을 호출하면 task queue에 등록 되고 call stack이 종료되면 task queue의 내용을 수행하므로.
