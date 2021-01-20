# Componets and Props

개념적으로 컴포넌트는 Javascript 함수와 유사합니다 props 임의의 입력을 받은 후 화면에 어떻게 표시되는지를 기술하는 React엘리먼트를 반환합니다

## Fucntion Componets

```
Todo = props => {
  return <li>{this.props.Something}</li>;
}
```
props라는 인자를 받은 후 React 엘리먼트를 반환하는 "함수 컨포넌트" 입니다

## Class Componets

```
class Todo extends React.Componet {
  render() {
    return <li>{this.props.Something}</li>;
  }
}
```


## 컴포넌트 렌더링

React 엘리먼트는 사용자가 정의한 컴포넌트로도 나타낼수 있습니다

```
Todo = props => {
  return li<li>{this.props.todo}</li>;
}

const element = <Todo todo='reading'/>
ReactDOM.reder(
  element,
  document,getElementById('rood')
);
```

> 컴포넌트의 이름은 항상 대문자로 시작 해야 합니다 React는 소문자로 지작하는 컴포넌트를 DOM 태그로 인식하여 처리합니다 
>> `<li />`는 HTML 태그를 나타내지만 `<Todo />`는 사용자가 정의한 컴포넌트를 나타냅니다