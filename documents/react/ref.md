# Ref

- React에서는 DOM 요소를 직접적으로 쓰지 않기 때문에, React의 요소에 접근하고 싶으면 Ref를 쓰면 된다.

https://ko.reactjs.org/docs/refs-and-the-dom.html

```javascript
class HabitAddForm extends Component {
    // Ref 생성
  formRef = React.createRef();

  ...

    render() {
    return (
       // form 태그에 생성한 formRef 를 연결
      <form ref={this.formRef} className="add-form" onSubmit={this.onSubmit}>
        ...
      </form>
    );
  }
}
```
