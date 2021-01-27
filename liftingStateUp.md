# State 끌어 올리기

가끔 동일한 데이터에 대한 변경 사항을 여러 컴포넌트에 반영해야 할 필요가 있습니다 이럴 떄에는 가장 가까운 공통 조상으로 state를 끌어올리는게 좋습니다

주어진 온도에 값을 받아 온도가 변하면 섭씨도 그에 맞춰서 계산되는 앱을 만들어보자
```
// step 1 : 주어진 온도에서 멘트를 컨트롤하자!

// 물의 온도 측정 하여 결과값 반환
const BoilingVerdict = props => {
  const celsius = props.celsius;
  return celsius >= 100 ? <p>The water would boil</p> : <p>The water would not boil</p>;
}

const Input = props => {
  return <input type="number" value={props.celsius} onChange={props.onChange}/>;
}

class App extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      celsius : 0 // 섭씨
    }
  }
  // 받은값을 적용
  onChange = e => {
    const chageCelsius = e.target.value;
    this.setState({celsius  : chageCelsius});
  }

  render() {
    return (
      <fieldset>
        <legend>plz enter the celsius</legend>
        <Input value={this.state.celsius} onChange={this.onChange}/>
        <BoilingVerdict celsius={this.state.celsius}/>
      </fieldset>
    );
  }
}
```

## 두번째 input 추가하기

```
// step2 : 섭씨를 받아 올 두번째 input을 추가해보자!!!

const BoilingVerdict = (props) => {
  const celsius = props.celsius;
  return celsius >= 100 ? (
    <p>The water would boil</p>
  ) : (
    <p>The water would not boil</p>
  );
};

const Input = (props) => {
  return (
    <input type="number" value={props.celsius} onChange={props.onChange} />
  );
};

class App extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      celsius: 0,
    };
  }

  onChange = (e) => {
    const chageCelsius = e.target.value;
    this.setState({ celsius: chageCelsius });
  };

  render() {
    return (
      <fieldset>
        <legend>plz enter the celsius</legend>
        <Input value={this.state.celsius} onChange={this.onChange} />
        <BoilingVerdict celsius={this.state.celsius} />
      </fieldset>
    );
  }
}

const Calculator = () => {
  return (
    <div>
      <App />
      <App />
    </div>
  );
};
```

## 변환 함수 작성하기

```
//step 3 : 섭씨를 화씨로 또 화씨를 섭씨로 변환해주는 함수를 작성해보자!!!
