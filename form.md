# Form

HTML의 폼은 자체가 내부 상태를 가지고 있기 때문에 React의 다른 DOM 엘리먼트와 조금 다르게 동작합니다

```
// age를 입력받아 전달
// form의 기본동작으로 제출을 하면 정해진 새로운 페이지로 이동
const App = () => {
  return (
    <label>
      <input type="text" age='age'/>
      <input type="submit" value='submit'/>
    </label>
  );
}
```

위와 같이 HTML의 form의 기본동작을 원하면 그대로 사용하면 되지만 Javascript 함수로 form의 제출을 처리하고 사용자가 입력한 데이터에 접근하도록 하는것이 편리합니다 이를 위한 표준 방식은 '제어 컴포넌트(Controlled Component)' 라고 불리는 기술을 이용하는 것입니다

## 제어 컴포넌트
HTML에서 input, textarea, select와 같은 폼 엘리먼트는 일반적으로 사용자의 입력을 기반으로 자신의 state를 관리하고 업데이트 합니다, React에서 변경할 수 있는 state가 일반적으로 컴포넌트의 state 속성에 유지되며 setState()에 의해 업데이트 됩니다

우리는 React state를 '신뢰 가능한 단일 출처(single source of truth)'로 만들어 두 요소를 결합할 수 있습니다 그러면 폼을 렌더링하는 React 컴포넌트는 폼에 발생하는 사용자 입력값을 제어 합니다 이러한 방식으로 React에 의해 값이 제어되는 입력 폼 엘리먼트를 '제어 컴포넌트 (controlled component)'라고 합니다.

```
// 전송된 이름을 기록하기 원한다면 폼을 제어 컴포넌트로 작성 할수 있습니다
class App extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      value : ''
    }
  }

  onSubmit = () => {
    alert(this.state.value)
  }

  onChange= (e) => {
    const value = e.target.value;
    this.setState({value})
  }

  render() {
    return (
      //제어 컴포넌트로 작성 예시
      // value는 폼 엘리먼트에 설정되므로 표시 되는 값은 항상 this.state.value이고 
      // onChange로인해 키입력이 onChange()를 실행시킨다
      // 제어 컴포넌트로 사용하면, input의 값은 항상 React state에 의해 결정됩니다
      <form onSubmit={this.onSubmit}>
        <label>Name :
          <input type="text" value={this.state.value} onChange={this.onChange} /> //  value는 신뢰가능한 단일 출처
        </label>
        <input type="submit" value='submit'/>
      </form>
    );
  }
}
```

