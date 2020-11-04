# Element의 class 요소 제어

- Element.classname 으로 class 값을 가져 올 수 있다.

```javascript
// get class name
element.classname;

// set class name
element.classname = "active";
```

## classList

- Element.classList 를 통해 class를 제어 할 수 있다.

```javascript
// class 추가
element.classList.add("active");

// class 삭제
element.classList.remove("active");

// class가 있는지 확인
element.classList.contains("active");
// return => true/false

// class가 있으면 제거하고 없으면 추가한다.
element.toggle("active);
```

- https://developer.mozilla.org/ko/docs/Web/API/Element/classList
