# Element size를 얻는 방법

- HTMLElement.offset
- HTMLElement.client
- getBoundingClientRect()

## offset

- 컨텐트의 너비, 스크롤바, 패딩까지 포함하여 엘리먼트가 차지하는 전체 공간을 알고 싶을 때
  - HTMLElement.offsetWidth
  - HTMLElement.offsetHeight

## client

- 패딩은 포함하지만 경계선, 여백, 스크롤바는 포함하지 않고 보이는 크기를 알고 싶을 때
  - HTMLElement.clientWidth
  - HTMLElement.clientHeight

## offset과 getBoundingClientRect의 차이점

- offset의 경우는 해당 엘리먼트의 실제 크기를 반환하지만, getBoundingClientRect의 경우는 렌더링 후의 크기를 반환함.
- 예를 들어, 크기 100px의 엘리먼트에 transform: scale(0.5);을 적용했을 경우, getBoundingClientRect()는 50px를, offsetWidth는 100px를 반환한다.
