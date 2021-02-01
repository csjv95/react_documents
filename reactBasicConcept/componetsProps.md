# Componets & Props

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

## 컴포넌트 합성
컴포넌트는 자기 자신의 출력에 다른 컴포넌트를 참조할 수 있습니다 

```
// Meet를 여러번 렌더링 할수 있습니다

Meet = props => {
  return <h1>Hello {props.name} nice meet you </h1>;
}

function App() {
  return (
    <main>
      <Meet  name={'SJ'} />
      <Meet name={'Reem'} />
    </main>
  );
}
```

## 컴포넌트 추출
컴포넌트를 여러 개의 작은 컴포넌트 단위로 나누면 앱이 커질수록 재사용이 가능한 컴포넌트들이 많아지면서 작업하기가 훨씬 수월할 것입니다 UI 일부가 여러번 사용되거나 UI가 복잡한 경우는 별도의 컴포넌트로 만드는것이 좋습니다

```
const comment = {
  date: new Date(),
  text: 'Hello I like animal',
  author: {
    name: 'Puppy',
    avatarUrl: 'https://animal.com/puppy'
  }
};

function Comment(props) {
  function formatDate(date) {
    return date.toLocaleDateString();
  }

function Comment(props) {
  return (
    <div className="comment">
      <div className="userInfo">
        <img className="avatar"
          src={props.author.avatarUrl}
          alt={props.author.name}
        />
        <div className="userInfo-name">
          {props.author.name}
        </div>
      </div>
      <div className="comment-text">
        {props.text}
      </div>
      <div className="comment-date">
        {formatDate(props.date)}
      </div>
    </div>
  );
}

ReactDOM.render(
  <Comment
    date={comment.date}
    text={comment.text}
    author={comment.author}
  />,
  document.getElementById('root')
);

```
class avatar를 컴포넌트로 추출
```
Avatar = props => {
  return (
   <img className="avatar"
      src={props.author.avatarUrl}
      alt={props.author.name}
   />
  );
}

//간결해진 모습
function Comment(props) {
  return (
    <div className="comment">
      <div className="userInfo">
        <Avatar author={props.author} />
        <div className="userInfo-name">
          {props.author.name}
        </div>
      </div>
      <div className="comment-text">
        {props.text}
      </div>
      <div className="comment-date">
        {formatDate(props.date)}
      </div>
    </div>
  );
}
```
userInfo를 컴포넌트로 추출
```
UserInfo = props => {
  <div className="userInfo">
    <Avatar author={props.author} />
    <div className="userInfo-name">
      {props.author.name}
    </div>
  </div>
}

//간결해진 모습
function Comment(props) {
  return (
    <div className="comment">
      <UserInfo author={props.author} />  
      <div className="comment-text">
        {props.text}
      </div>
      <div className="comment-date">
        {formatDate(props.date)}
      </div>
    </div>
  );
}
```

## porps는 읽기 전용입니다
> 클래스,함수 컴포넌트 모두 컴포넌트의 자체 prop를 수정해서는 안됩니다   
> React의 모든 컴포넌트들은 props를 다룰떄 반드시 순수함수 처럼 동작해야 합니다

