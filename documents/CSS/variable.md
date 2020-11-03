# CSS Variable

- CSS에서 사용 할 수 있는 변수
  - IE에선 지원하지 않음.

## 선언

- '--[변수명]'으로 선언

```css
.box {
  --font-size: 32px;
}
```

- 지역변수로 선언 되기 때문에 .box에서 선언한 것들은 .box와 .box의 자식 노드에서만 사용 할 수 있다.
- 전체적으로 사용하려면 :root에 선언 해주면 좋다.

```css
:root {
  --background-color: black;
  --font-size: 32px;
}
```

## 사용

- var(변수명); 을 통해 사용

```css
.box {
  font-size: var(--font-size);
}
```

- calc 함수와 같이 사용하면 편리

```css
.box {
  font-size: calc(var(--font-size) * 2);
}
```

- 정의 되지 않은 변수의 경우 기본 값을 설정 할 수 있다.

```css
.box {
  font-size: var(--font-size, 8px);
}
```

## 활용

- 미디어 쿼리에 활용하면 좋다.

```css
:root {
  --font-size: 12px;
  --background-color: black;
}

.box {
  font-size: var(--font-size);
  background-color: var(--background-color);
}

@media screen and (max-width: 768px) {
  :root {
    --font-size: 10px;
    --background-color: white;
  }
  /* 별도의 css수정 없이 variable 값의 수정 만으로도 화면 구성을 바꿀 수 있다. */
}
```
