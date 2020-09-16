# Event

- https://developer.mozilla.org/en-US/docs/Web/Events

- 요소에 Event Handler 를 등록해서 이벤트에 대한 처리를 등록 할 수 있다.

## Event Target

- 이벤트를 등록하거나, 제거하거나, 인공적으로 이벤트를 발생 시킬 수 있다.
  - https://developer.mozilla.org/en-US/docs/Web/API/EventTarget

```javascript
// 이벤트 등록
div.addEventListener("clicl", () => colsole.log("clicked!"));

// 인공적으로 이벤트 호출
div.dispatchEvent(new Event("click"));

// 등록한 이벤트 삭제
div.removeEventListener("click", listener);
```

## Bubbling

- 한 요소에서 이벤트가 발생하면, 그 요소의 이벤트에 이어서 부모 요소의 핸들러도 같이 동작함

```html
<div class="parent">
  <p class="child"></p>
</div>

<!-- div와 p 둘 다 click 이벤트가 있다고 가정할 때, p 영역을 클릭하면 div의 click 이벤트도 발생한다. -->
```

이 경우, 부모의 이벤트를 중지 하기 위해 `event.stopPropagation();` 같은 함수를 사용 할 수 있지만, 다른 이벤트가 중지 되기 때문에 팀으로 작업할 때 예상치 못한 일이 발생할 수 있다.

- 자식의 이벤트가 발생할 때 마다 부모에서 다른 처리를 한다던지

그런 경우, 어느 쪽에서 실행된 이벤트인지 비교 해주는 if문을 넣어주면 방지 할 수 있다.

```javascript
parent.addEventListener("click", (event) => {
  if (event.target !== event.currentTarget) {
    return;
  }
});
```

## preventDefault

- 브라우저상의 이벤트를 취소하는 함수

```javascript
checkbox.addEventListener("click", (event) => {
  event.preventDefault();
  // checkbox를 click해도 브라우저 상의 event가 취소되므로 체크되지 않음.
});
```

- 단, scroll 같이 빠르게 동작되는 이벤트는 addEventListner 함수보다 자기의 할 일(scrolling) 을 먼저 시행하므로, 브라우저의 행동을 취소할 수 없다.

```javascript
document.addEventListener("wheel", (event) => {
  event.preventDefault();
  // 이 경우, 휠을 내리면 스크롤은 내려가지만 console.log에 경고창이 뜬다.
});
```

scrolling 을 취소 하고 싶다면, addEventListener에 passive 라는 속성의 값을 변경해서 scrolling 을 막을 수 있다.

```javascript
document.addEventListener("wheel", event => {
  event.preventDefault();
},
{ passive: false }
// scroll의 경우 passive의 default값이 true
```

- 단, passive의 default의 값이 true인 애들은 억지로 passive를 바꾸는 것은 왠만하면 좋지 못하다.
  - default가 true인 이유가 있음.

## 이벤트의 위임 (BAD vs GOOD)

`li`가 클릭 된 경우, 이벤트를 실행하고 싶다면?

```html
<ul>
  <li>1</li>
  <li>2</li>
  <li>3</li>
  <li>4</li>
  <li>5</li>
  <li>6</li>
  <li>7</li>
  <li>8</li>
  <li>9</li>
</ul>
```

### Bad

- 모든 `li` 요소에 이벤트를 등록

```javascript
const lis = document.querySelectorAll("li");
lis.forEach((li) => {
  li.addEventListener("click", () => {
    li.classList.add("selected");
  });
});
```

### Good

- 부모의 노드에 이벤트를 등록해서 자식을 구분.

```javascript
const ul = document.querySelector("ul");
ul.addEventListener("click", (event) => {
  if (event.target.tagName == "LI") {
    event.target.classList.add("selected");
  }
});
```
