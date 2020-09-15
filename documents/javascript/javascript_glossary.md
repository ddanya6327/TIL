## Hoisting

- var, function declaration 선언들이 제일 위로 올라가는 것.

## DOM (Document Object Model)

- HTML, XML 문서의 프로그래밍 interface
- 페이지의 콘텐츠 및 구조, 스타일을 읽고 조작할 수 있는 API를 제공
- 트리 구조로 되어있어 DOM 트리 라고도 한다.

## CSSOM (CSS Object Model)

- CSS를 브라우저가 이해하고 처리 할 수 있는 형식으로 변환
- DOM와 마찬가지로 트리 구조로 되어있어 CSSOM 트리 라고도 부름

## Render Tree

- DOM Tree 에 CSSOM Tree를 적용하여 Render Tree 를 생성한다.
- 출력에 반영되지 않는 불필요한 노드들은 생략
  - ex) script, meta
  - `display: none` 처럼 출력에 반영되지 않는 노드들도 제외
