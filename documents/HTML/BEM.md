# BEM

- Block, Element, Modifier 로 나누어서 이름을 작성하는 것.

- `block__element--modifier` 형태로 작성

```html
.card .card__img .card__title .card__button .card--dark .card__button--blue
```

- button 같이 특정 block에서만 사용되는게 아니라면 굳이 한 block에 속해 있을 필요는 없다.

```html
.card__button
<!-- 하지만 버튼의 경우 어디에서나 사용 하므로 아래에 같이 block으로 사용하는게 좋다. -->

.button .button--blue
```

## 같은 block이 묶여있는 경우는?

- 예를 들어, card라는 block이 여러개 묶여있는 cards의 경우는?

```html
.cards .cards__card .cards__card__title

<!-- bad -->
```

위와 같이 cards 안에 card로 class를 정의 해야 할까?
-> 아니다. 컴포넌트 단위, block 레벨이기 때문에 card로 붙이면 된다.

```html
.cards .card .card__title

<!-- good -->
```

http://getbem.com/introduction/
https://css-tricks.com/bem-101/
