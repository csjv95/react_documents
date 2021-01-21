# state & lifecycle

## state 와 porps의 차이점
state는 일반 Javascript객체 입니다 props는 properties입니다
두객체 모두 렌더링 결과물에 영향을 주는 정보를 가지고 있는데 state는 정의된 컴포넌트 안에서 관리되며 props는 매개변수 처럼 컴포넌트에 전달되는 방식으로 결과물에 영향을 줍니다

## state 사용
state를 이용하여 컴포넌트를 완전히 재사용이 가능하며 캡슐화해서 사용할 수 있는 장점이 있습니다   
다음은 시간을 나타내는 어플리케이션입니다 

```
tick () => {
  const element = (
    <div>
      <span>Sorry what tiem is now?</span>
      <sapn>It's {new Date().toLocaleTimeString()}</span>
    </div>
  );

  ReactDOM.render(
    element,
    document.getElemntById('root') 
  );
}

 setInterval(tick,1000);
```

<strong>props</strong> 
```
function Clock(props) {
  return (
    <div>
      <h1>Hello, world!</h1>
      <h2>It is {props.date.toLocaleTimeString()}.</h2>
    </div>
  );
}

function tick() {
  ReactDOM.render(
    <Clock date={new Date()} />,
    document.getElementById('root')
  );
}

setInterval(tick, 1000);
```

<strong>state</strong>를 이용할려면 class 로 변환 해주어야 합니다

```
class Clock extends React.Component {
  constructor(props) {
    super(props);
    this.state = {date : new Date()};
  }

  render(
    <div>
      <span>Sorry what tiem is now?</span>
      <sapn>It's {this.state.date.toLocaleTimeString()}</span>
    </div>
  );
}

function tick() {
  ReactDOM.render(
    <Clock />,
    document.getElementById('root')
  );
}
```