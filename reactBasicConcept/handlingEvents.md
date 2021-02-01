# 이벤트 처리

* React 이벤트는 소문자가 아닌 camelCase를 사용합니다
* JSX를 사용하면 문자열이 아닌 이벤트 핸들러로 함수를 전달합니다
* React에서는 기본동작 방지를 위해서 반드시 preventDefault()를 명시적으로 호출해야 합니다

```
//HTML
<button onclick='handleAdd()'>
Add
</button>

// In React 
(onclick ===> onClick) camelCase
('handleAdd()' ===> {handleAdd}) function
<button onClick={handleAdd}>
Add
</button>
```

```
//HTML
onSubmit = event => {
  return false;
}
<form onsubmit='onSubmit()'>

(false ===> preventDefault)
//React
onSubmit = event => {
  event.preventDefault();
}
<form onSubmit = {onSubmit}>

```

class를 사용하여 구성 요소를 정의 할 때 일반적인 패턴은 이벤트 처리기가 클래스의 메서드가되는 것입니다
```
class Event extends React.Component {
   state = {
    isLogin : false
  }

  btnClick = () => {
    this.setState(state => ({
      isLogin : !this.state.isLogin
    }))
  }
  
  render() {
    return (
      <button onClick={this.btnClick}>
        {this.state.isLogin ? 'On' : 'Off'}
      </button>
    );
  }
}
```