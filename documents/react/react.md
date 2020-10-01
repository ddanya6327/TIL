# React

- 컴포넌트 단위로 이루어진 UI(User Interfaces)를 만들 수 있는 라이브러리

  - 사용자에게 UI를 보여주고 Event를 처리하는 일을 할 수 있다.

- React 의 주요 키워드
  - _Components_ \*
  - _Re-render_ \*
  - Virtual DOM

https://reactjs.org/

## Components

- 한 가지의 기능을 수행하는 UI의 단위
- 특징
  - 독립적
  - 고립적?
  - 재사용성이 높다
    => 즉 유닛별로 테스트 하기 쉽다.
- Components는 DOM과 같은 tree 구조
- Component는 컴포넌트의 데이터를 관리하는 state, UI를 구현하는 render 함수가 있다.
  - 상태(state)가 변경되면 render 함수가 호출된다.
    - tree 구조 이므로 부모 요소의 state가 변경되면 자식도 같이 render 된다.
    - 다만, virtual DOM 을 통해 실질적으로 변경 된 요소만 업데이트 하므로 성능적으로 이점이 있다.

## React의 장점

- State가 변경되면 자동으로 re-render 해주기 때문에 개발자가 신경 쓸 것이 줄어든다.
- Virtual DOM 을 이용해서 이전의 Tree와 비교해서 업데이트 된 부분을 찾고, 그 부분을 모아서 한 번에 업데이트 하기 때문에 성능적으로 빠르다.

## Tip

- render 함수는 순수 함수로 작성
  - 랜덤 함수 사용 X, 외부 상태 변경 X
  - input이 같다면 output도 같아야 한다.
- state는 불변 변수로 관리한다.
  - 직접 수정을 할 경우 render가 되지 않을 수 있다.
