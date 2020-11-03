# CSS를 이용한 가운대 정렬

## 수평 정렬

- Display: box의 경우
  - margin: auto를 이용하여 가운대로 정렬

```css
.box {
  margin: auto;
}
```

- display: inlin-block 또는 inline 의 경우
  - text-align: center 를 이용하여 가운대로 정렬

```css
.text {
  text-align: center;
}
```

## 수직 정렬의 경우

- transform 을 이용한 방법

```css
.box {
  transform: translate(50%, 50%);
}
```

- text의 경우 line-height를 이용할 수 있음
  - 정상적이진 않고 hacky한 방법 (텍스트가 2줄 이상의 경우 사용불가)

```css
.text {
  line-height: 100px;
}
```
