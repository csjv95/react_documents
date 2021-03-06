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

## textarea 테그

HTML에서 textarea 엘리먼트는 텍스트를 자식으로 정의 합니다  
React에서 textarea는 value 어트리뷰트를 대신 사용합니다 이를 이용한면 textarea를 사용하는 폼은 한 줄 입력을 사용하는 폼과 비슷하게 작성할 수 있습니다

```
class App extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      value: '',
    };
  }

  handleChange = e => {
   this.setState({value : e.target.value});
  }
  
  handleSubmit = () => {
    alert(this.state.value);
  }

  render() {
    return (
      <form onSubmit={this.handleSubmit}>
        <textarea value={this.state.value} onChange={this.handleChange}/>
        <input type='submit' value='submit'/>
      </form>
    );
  }
}
```

## select 태그

HTML에서 select는 드롭 다운 목록을 만듭니다   
select는 selected 옵션이 있으므로 state에 정해준 값이 초기값이 됩니다  
React에서 selected 어트리뷰트를 사용하는 대신 최상단 select 태그에 value 어트리뷰트를 사용합니다 한 곳에서 업데이트만 하면 되기 때문에 제어 컴포넌트에서 사용하기 편합니다

```
class App extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      value: "select",
    };
  }

  onSubmit = () => {
    alert(this.state.value);
  };

  onChange = (e) => {
    this.setState({ value: e.target.value });
  };

  render() {
    return (
      <form onSubmit={this.onSubmit}>
        <label>
          Animal
          <select value={this.state.value} onChange={this.onChange}>
            <option value="select">----</option>
            <option value="dog">Dog</option>
            <option value="cat">Cat</option>
            <option value="tiger">Tiger</option>
            <option value="bear">Bear</option>
          </select>
        </label>
      </form>
    );
  }
}
// selcte 태그에 multiple 옵셥을 허용하면 value 어트리뷰트에 배열을 전달할 수 있습니다
// <select mutiple={true} value={[1,2,3,4,5]}>
```

> 전반적으로 input, textarea, select는 매우 비슷하게 동작합니다 모두 제어 컴포넌트를 구현 하는데 value 속성을 허용합니다

## 다중입력 제어하기

여러 input 엘리먼트를 제어 해야할때 각 엘리먼트에 name 어트리뷰트를 추가하고 event.target.name 값을 통해 핸들러가 어떤 작업을 해야할지 선택할 수 있게 해줍니다 

```
class App extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      greeting : false,
      get : 0
    };
  }

  onChange = e => {
    const target = e.target;
    const value = target.type === 'checkbox' ? target.checked : target.value;
    const name = target.name; // name 정의 => name에 따라 다른 동작을 할 수 있게 만들수 있음

    this.setState({[name] : value}); // ES6문법 계산된 속성이름, 주어진 입력 이름에 상태키를 업데이트
  }
  
  render() {
    return (
      <form>
        <label>Can I get your phone number?
          <input 
          type = 'checkbox'
          name = 'greeting' // name 추가
          checked = {this.state.greeting}
          onChange = {this.onChange}
          />
        </label>

        <label>How many get phone number
          <input 
          type = 'number'
          name = 'get'  // name 추가
          value = {this.state.get}
          onChange = {this.onChange}
          />
        </label>
      </form>
    );
  }
}
```

## 제어된 입력 Null 값
제어 컴포넌트에 value prop을 지정하면 의도하지 않는한 사용자가 변경할 수 없습니다 value를 설정했는데 여저히 수정할 수 있다면 실수로 value를 undifined나 null로 설정했을 수 있습니다

## 제어 컴포넌트의 대안
데이터를 변경할 수 있는 모든 방법에 대해 이벤트 핸들러를 작성하고 React 컴포넌트를 통해 모든 입력 상태를 연결해야 하기 때문에 때로는 제어 컴포넌트를 사용하는게 지루할 수 있습니다 특히 지존의 코드 베이스를 React로 변경하고자 할 때나 React가 아닌 라이버리와 React 어플리케이션을 통합하고자 할떄 짜증이 날수도 있습니다 이러한 경우에 입력 폼을 구현하기 위한 대체 기술이 있습니다 이것을 비제어 컴포넌트라고 합니다