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

// 섭씨를 화씨로
const toCelsius = fahrenheit => {
  return (fahrenheit - 32) * 5 / 9;
}

// 화씨를 섭씨로
const toFahrenheit = celsius => {
  return (celsius * 9 / 5) + 32;
}

// teamperate,와 변환 함수를 인수로 가져와 문자열로 반환
const tryConvert= (temperature, convert) => {
  const input = parseFloat(temperature);
  const output = convert(input);
  const rounded = Math.round(output * 1000) / 1000; // 소수점 3자리 까지 반올림
  return Number.isNaN(input) ? '' : rounded.toString();
} 
```
## State 끌어올리기
React 에서 state를 공유하는 일은 그 값을 필요로 하는 컴포넌트 간의 가장 가까운 공통 조상으로 state를 끌어 올림 으로써 이뤄낼 수 있습니다 이렇게 하는 방법을 'state 끌어 올리기' 라고 부릅니다

```
//step 3 : 서로 분리된 TemperatureInput에서 state를 하나의 가장 가까운 공통 조상인 Calculator에 합친다

const scaleName = {
  c: "Celsius",
  f: "Fahrenheit",
};

const toCelsius = (fahrenheit) => {
  return ((fahrenheit - 32) * 5) / 9;
};

const toFahrenheit = (celsius) => {
  return (celsius * 9) / 5 + 32;
};

const tryConvert = (temperature, convert) => {
  const input = parseFloat(temperature);
  const output = convert(input);
  const rounded = Math.round(output * 1000) / 1000;
  return Number.isNaN(input) ? "" : rounded.toString();
};

const BoilingVerdict = (props) => {
  return props.celsius >= 100 ? (
    <p>The water would boil</p>
  ) : (
    <p>The water would not boil</p>
  );
};

const TemperatureInput = (props) => {
  const onChange = (e) => {
    //  const ChangeTemperature = e.target.value;
    //  this.setState({ temperature: ChangeTemperature });
    props.onTemperatureChange(e.target.value);
  };

  //const temperature = this.state.temperature;
  const temperature = props.temperature; //this.state => props
  const scale = props.scale;
  return (
    <fieldset>
      <legend>Plz Enter the temperature {scaleName[scale]}</legend>
      <input type="number" value={temperature} onChange={onChange} />
      {/* <BoilingVerdict celsius={temperature} /> */}
    </fieldset>
  );
};

class Calculator extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      temperature: 0,
      sclae: 'c',
    };
  }

  handleCelsius = temperature => {
    this.setState({ temperature, scale : 'f' });
  };

  handleFahrenheit = temperature => {
    this.setState({ temperature, scale : 'c' });
  }

  render() {
    const scale = this.state.sclae;
    const temperature = this.state.temperature;
    const  fahrenheit = scale === 'c' ? tryConvert(temperature,toFahrenheit) : temperature;
    const celsius = scale === 'f' ? tryConvert(temperature,toCelsius) : temperature;

    return (
      <main>
        <TemperatureInput
          scale={"c"}
          temperature={celsius}
          onTemperatureChange={this.handleCelsius}
        />
        <TemperatureInput 
          scale={"f"}
          temperature={fahrenheit}
          onTemperatureChange={this.handleFahrenheit}
        />
        <BoilingVerdict celsius={temperature} />
      </main>
    );
  }
}
```

## 교훈

React 애플리케이션 안에서 변경이 일어나는 데이터에 대해서는 하나의 state를 두워야 합니다 보통의 경우에는 state 렌더링에 그 값을 필요로 하는 컴포넌트에 먼저 추가 됩니다 그리고 나서 다른 컴포넌트도 역시 그값이 필요하게 되면 그값을 그들의 가장 가까운 공통 조상으로 끌어 올리면 됩니다 다른 컴포넌트 간에 존재하는 state를 동기화 시키려고 노력하는 대신 하향식 데이터 흐름에 기대는 걸 추천합니다   

state를 끌어올리는 작업은 양방향 바이딩 접근 방식보다 더 많은 '보일러 플레이트' 코드를 유발 하지만 버그를 찾고 격리하기 더 쉽게 만든다는 장점이 있습니다 어떤 state 간에 특정 컴포넌트 안에서 존재하기 마련이고 그 컴포넌트가 자신의 state를 스스로 변경할 수 있으므로 버그가 존재할 수 있는 범위가 크게 줄어들게 됩니다 또한 사용자의 입력을 거부하거나 변경 하는 자체 로직을 또한 쉽게 구현 할수 있습니다   

만약 어떤 값이 props,state로 부터 계산될 수 있다면 그 값을 state에 두어서는 안됩니다 예를 들어 celsius 와 fahrenheit를 둘다 저장하는 대신 최근에 변경된 temperature 과 scale만 저장 하면 됩니다 그 값들을 기반으로 render() 메서드 안에서 계산될 수 있습니다 이를 통해 사용자 입력값이 정밀도를 유지한 채 다른 필드의 입력값에 지우거나 적용 할수 있습니다 

> '보일러 플레이트'는 변경 없이 계속하여 재 사용할 수 있는 걸 말하며, 재사용 가능한 프로그램을 가리키는 데 사용되기도 한다.
