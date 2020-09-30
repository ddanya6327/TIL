# Single Page Application

- 페이지 전환이 필요할 때 마다 서버에 요청해서 새로운 HTML 을 받는 형식이지만, 싱글 페이지 어플리케이션의 경우 단일 HTML 을 내려 받아서 필요시 데이터를 요청하고 바인딩 하는 방식.

## SAP가 가능하기 위한 조건

- 자바스크립트에서 브라우저로 페이지 전환 요청을 보낼 수 있다.
  - 단, 브라우저는 서버로 요청을 보내지 않아야 함.
- 브라우저의 뒤로 가기와 같은 사용자의 페이지 전환 요청을 자바스크립트에서 처리할 수 있어야 한다.
  - 브라우저는 서버로 요청을 보내지 않아야 함.

즉, 브라우저와 JS는 서로 페이지 전환 사실을 알아야 함.

### 위 조건을 만족하는 브라우저 API

- pushState(), replaceState()

  - pushState
    - history 엔트리에 새로운 엔트리를 추가
    - window.history.pushState({data: '주소와 저장 될 데이터'}, 'title', '/URI');
  - replaceState
    - 현재 히스토리 엔트리를 변경

- popstate 이벤트
  - https://developer.mozilla.org/ko/docs/Web/API/Window/popstate_event
