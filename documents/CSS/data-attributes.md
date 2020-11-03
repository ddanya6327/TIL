# Data Attributes

- DOM 요소에 원하는 속성을 추가 할 수 있는 기능

  - HTML5에서 추가됨.

- `data-*` 형태로 정의

```html
<div data-index="1" data-display-name="mobile"></div>
<div data-index="2" data-display-name="pc"></div>
<span data-index="2" data-display-name="pc"></span>
```

## data- 을 이용한 DOM 요소 선택

- css

```css
[data-display-name="mobile"] {
  background-color: black;
}

/* div 태그만 적용됨 */
div[data-display-name="pc"] {
  background-color: black;
}
```

- JavaScript

```javascript
const data = document.querySelector('div[data-display-name="mobile"]');
// data- 는 dataset을 통해 접근 가능하다

// data.dataset;
// -> displayName:mobile
// -> index: 1

// 단, 정의시에는 display-name 과 같은 형태지만, dataset을 통해 접근하면 displayName과 같은 CamelCase 로 보임
```

## 주의점

- DOM 요소는 누구나 확인 할 수 있기 때문에, 사용자 정보 같은 보안이 필요한 데이터는 data- 에 넣지 않는 편이 좋다. 잘 암호화 해서 JavaScript를 통해 관리하는게 안전.
