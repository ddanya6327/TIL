# Form

https://ko.reactjs.org/docs/forms.html

## Submit

- Form의 경우 onSubmit 속성을 사용해서 이벤트를 처리 할 수 있다.

```javascript
<form className="add-form" onSubmit={this.handleSubmit}>
  <input type="text" className="add-input" placeholder="Habit" />
  <button className="add-button">Add</button>
</form>
```

- 위의 경우 Button을 클릭하면 refresh가 발생하는데, preventDefault() 함수를 이용해서 브라우저상의 refresh를 막을 수 있다.

```javascript
handleSubmit = (event) => {
  event.preventDefault();
};
```
