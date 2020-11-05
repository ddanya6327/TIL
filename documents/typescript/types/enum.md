# Enum

```typescript
enum Fruit {
  Apple,
  Banana,
  Orange,
}
const v1: Fruit = Fruit.Apple;
const v2: Fruit.Apple | Fruit.Banana = Fruit.Banana;
```

- Enum의 첫 번째 원소에 값을 할당하지 않으면 자동으로 0이 할당되고, 첫 번째 이후의 원소는 이전 원소에서 1만큼 증가한 값이 할당됨.

```typescript
enum Fruit {
  Apple, // 할당하지 않으면 0
  Banana = 5,
  Orange, // 이전 원소가 5 이므로 1 이 증가한 6
}
```

- Enum의 요소에 숫자를 할당하면 양방향으로 맵핑이 된다.

```typescript
enum Fruit {
  Apple,
  Banana = 5,
  Orange,
}
console.log(Fruit.Banana); // 5
console.log(Fruit["Banana"]); // 5
console.log(Fruit[5]); // Banana
```

- Enum의 요소에 문자를 할당하면 단방향으로 맵핑이 된다.

```typescript
enum Language {
  Korean = "ko",
  English = "en",
  Japanese = "jp",
}
// Fruit[5] 와 같이 Language["ko"] 를 호출할 수 없다. 단방향이기 때문.
```

## 주의점

- enum을 사용하면 컴파일 후에도 enum 객체가 남아있기 때문에 번들 파일의 크기가 커질 수 있음. 이런 경우, const enum을 사용해서 enum 객체를 생성하지 않아 번들 파일의 크기를 줄일 수 있다.
  - 단, Enum 객체를 사용하는 함수를 사용하지 못하는 경우가 발생 할 수 있으므로, 상황에 따라 결정할 것.
