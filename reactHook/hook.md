# Hook

Hook 은 React에 새로 추가된 기능입니다 class를 작성하지 않고도 state와 다른 React의 기능들을 사용할 수 있게 도와줍니다

Class counter app

```
class App extends Component {
  constructor(props) {
    super(props);
    this.state = {
      count : 0
    }
  }
  
  render() {
    const count = this.state.count;
    return (
      <>
        <span>{count}</span>
        <button onClick={ () => this.setState({count : count + 1})}>up</button> 
      </>
    );
  }
}
```

Hook counter app

```
const App = () => {
  // count라는 새로운 상태변수를 선언 합니다
  const [count,setCount] = useState(0); // useState는 현재의 state 값과 이 값을 업데이틑 하는 함수를 쌍으로 제공합니다

  return(
    <>
      <span>{count}</span>
      <button onClick={ () => setCount(count + 1)}>up</button>
    </>
  )
}
```

여기서 useState가 바로 Hook 입니다 Hook을 호출해 함수 컴포넌트 안에 state를 추가 했습니다 이 state는 컴포넌트가 다시 렌더링 되어도 그대로 유지될 것입니다 useState는 현재의 state 값과 이 값을 업데이틑 하는 함수를 쌍으로 제공합니다 우리는 이 함수를 이벤트 핸들러나 다른 곳에서 호출할 수 있습니다 이것은 class의 this.state와 거의 유사하지만 이전 state와 새로운 state를 합치지 않는다는 차이점이 있습니다

useState는 인자로 초기 state 값을 하나 받습니다 카운터는 대부분 0부터 시작하므로 위 예시에서는 초기값을 0으로 초기화 했습니다 this.state와는 달리 setState Hook의 state는 객체일 필요가 없습니다 (원하면 가능) 이 초기값은 첫 번쨰 렌더링에만 딱 한번 사용됩니다


## 여러 state 변수 선언하기
하나의 컴포넌트 내에서 여러개의 state hook을 사용할 수 있습니다

```
const App = () => {
  const [name,setName] = useState('SJ');
  const [age,setAge] = useState(0);
  const [hobby,setHobby] = useState('Jogging');
  const [client,setClient] = useState([{id: 1, name: Reem , age : 25, hobby : reading}])
}
```
