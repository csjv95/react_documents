# 합성 vs 상속

React는 강력한 합성 모델을 가지고 있으며 상속 대신 합성을 사용 하여 컴포넌트 간에 코드를 재사용하는 것이 좋습니다(합성 Win)

## 컴포넌트에 다른 컴포넌트 담기 (합성)

몇몇 컴포넌트들은 어떤 자식 엘리먼트가 들어올지 미리 예상 할수 없는 경우가 있습니다 범용적인 'Box' 역활을 하는 Sidebar 또는 Dialog와 같은 컴포넌트에서 특히 자주 볼수 있습니다  

## props.children
이런한 컴포넌트에서 특수한 children prop을 사용하여 자식 엘리먼트를 출력에 그대로 전달하는 것이 좋습니다

```
const List = props => {
  return (
    <ul>
      {props.children}  // props.children 으로 자식을 전달
    </ul>
  );
}

// 밑의 방식으로 다른 컴포넌트에서 JSX를 중첩하여 자식을 전달 가능
const Item = props => {
  return (
    <List>
      <li>
        {props.children}  // props.children 으로 자식을 전달
      </li>
    </List>
  );
}

// 밑의 방식으로 다른 컴포넌트에서 JSX를 중첩하여 자식을 전달 가능
const App = () => {
  return (
    <main>
      <Item >
        <h1>title</h1>
        <p>description</p>
      </Item>
    </main>
  );
}
```

## props.자기만의 방식

흔하진 않지만 가끔 컴포넌트에 여러 개의 구멍이 필요할수 있습니다 이런 경우 children 대신 자기만의 고유한 방식으로 적용할 수 있습니다

```
const Title = () => {
  return <h1>title</h1>;
};

const Description = () => {
  return <p>description</p>;
};

const List = (props) => {
  return <ul>{props.children}</ul>;
};

const Item = (props) => {
  return (
    <List>
      <li>{props.children}</li>
    </List>
  );
};

//  아래처럼 여러개의 구멍이 필요할 수 있습니다
const ListItem = (props) => {
  return (
    <Item>
      <h1>{props.title}</h1> // 자신만의 방식으로
      <p>{props.description}</p>
    </Item>
  );
};

const App = () => {
  return (
    <main>
      <ListItem title={<Title />} description={<Description />} />
    </main>
  );
};

export default App;

```

'<Title />','<Description />'이와 같은 엘리먼트들은 단지 React 객체이기 때문에 다른 데이터 처럼 prop로 전달할 수 있습니다 이러한 접근은 다른 라이브러리의 slots과 비슷해 보이지만 React에서 props로 전달할 수 있는 것에는 제한이 없습니다


## 특수화
때로는 어떤 컴포넌트의 특수한 경우인 컴포넌트를 고려해야 하는 경우가 있습니다 예를 들어 ListItem은  App의 특수한 경우라고 할 수 있습니다   

React에서는 이것 또한 합성을 통해 해결할 수 있습니다 더 구체적인 컴포넌트가 일반적인 컴포넌트를 렌더링하고 prop를 통해 내용을 구성합니다

```
// 없는 코드는 위 내용과 동일

const ListItem = (props) => {
  return (
    <Item>
      <h1>{props.title}</h1>
      <p>{props.description}</p>
      {props.children}  
    </Item>
  );
};
//  합성은 클래스로 정의된 컴포넌트에도 동일하게 적용됩니다
class App extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      value : ''
    }
  }

  onChange = e => {
    const changeName = e.target.value
    this.setState({value : changeName})
  }

  submit = ()=> {
    alert(this.state.value)
  }
  
  render() {
    return (
      <main>
        <ListItem title={<Title />} description={<Description />} >
        <input type="text" onChange={this.onChange} placeholder='plz input & click the submit'/>
        <button onClick={this.submit}>submit</button>
       </ListItem>
      </main>
    );
  }
};
```

## 상속 보단 합성
Facebook에서는 수천개의 React 컴포넌트를 사용하지만 컴포넌트를 상속 계층 구조로 작성을 권장할만한 사례를 아직 찾지 못했습니다   

props와 합성은 명시적이고 안전한 방법으로 컴포넌트의 모양과 동작을 커스터마이징 하는데 필요한 모든 유연성을 제공합니다 컴포넌트가 원시 타입의 값 React 엘리먼트 혹은 함수 등 어떠한 props도 받을수 있다는 것을 기억하세요   

UI가 아닌 기능을 여러 컴포넌트에서 재사용 하기를 원한다면 별로의 Javascript 모듈로 분리하는 것이 좋습니다 컴포넌트에서 해당 함수,객체,클래스 등을 import하여 사용할 수 있습니다 상속받을 필요 없이 가능합니다

