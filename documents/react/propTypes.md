# PropTypes

- 속성 값의 type을 정할 수 있는 React 패키지

```javascript
import PropTypes from "prop-types";

User.propTypes = {
  male: PropTypes.bool.isRequired,
  age: PropTypes.number,
  type: PropTypes.oneOf(["gold", "silver", "bronze"]),
  onChangeName: PropTypes.func, // (name: string) => void 같은 형태로 매개변수나 반환 값을 적어주면 좋다.
  onChangeTitle: propTypes.func.isRequired,
};
```

- https://ko.reactjs.org/docs/typechecking-with-proptypes.html
