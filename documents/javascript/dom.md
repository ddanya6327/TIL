# DOM 관련 함수

## querySelector

- 선택자와 일치하는 첫 번째 Element를 반환, 요소가 없으면 Null 을 반환

```javascript
document.querySelector(selectorrs);

document.querySelector(".class");

document.querySelector("user input[name=login]");
```

## createElement

- HTML 요소 생성

```javascript
let createDiv = document.createElement("div");
let h2 = document.createElement("h1");

// text 생성
document.createTextNode("my text");
```

### 생성한 요소에 속성 추가

- setAttribute()

```javascript
h2.setAttribute("class", "title");
h2.textContent = "this is a title";
```

### 생성한 요소를 추가하기

- appendChild(selector)

```javascript
document.body.appendChild(createDiv);
```

- append()
  - appendChild() 후에 나온 함수

```javascript
document.body.append();
// appendChild와 달리 반환 값이 없음
```

- insertBefore(a, b)
  - b 앞에 a 요소를 넣는다.

### 요소 삭제

- removeChild(selector);
