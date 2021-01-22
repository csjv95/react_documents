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
      <span>Sorry what time is now?</span>
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

## lifecycle를 클래스에 추가하기

많은 커포넌트가 있는 애플리케이션에서 컴포넌트가 삭제될때 해당 컴포넌트가 사용 중이던 리소스를 확보하는 것이 중요합니다   

### 마운트   
다음 매서드들은 컴포넌트의 인스턴스가 생성되어 DOM 상에 삽입될때 순서대로 호출됩니다

* constructor()
* static getDerivedStateFromProps()
* render()
* compoentDidMount()

### 업데이트
props 또는 state가 변경되면 갱신이 되며 다음 매서드들은 컴포넌트가 다시 렌더링될 떄 순서대로 호출됩니다

* static getDerivedStateFromProps()
* shouldComponetUpdate()
* render()
* getSnapshotBeforeUpdate()
* componetDidUpdate()

### 마운트 해제
다음 매서드는 컴포넌트가 DOM에서 제거 될떄 호출됩니다

* compoentWillUnmout

## 라이프사이클의 예시

Clock이 처음 DOM에 렌더링 될 때마다 타이머를 설정 할려고 합니다 이것은 React에서 '마운팅'이라고 합니다

Clock에서 생성된 DOM이 삭제될 때마다 타이머를 해제 할려고 합니다 이것은 React에서 '언마운팅'이라고 합니다

```
class Clock extends React.Componet {
  constructor(props) {
    super(props);
    this.state = {date : new Date()};
  }
  
  componetDidMount(){
     this.timerID = setInterval(
      () => this.tick(),
      1000
    );
  }

  componetWilUnmount() {
    clearInterval(this.timerID);
  }

  tick() {
    this.setState({
      date: new Date()
    });
  }

  render() {
    return(
    <div>
      <span>Sorry what time is now?</span>
      <sapn>It's {this.state.date.tolocalTimeString}</span>
    </div>
    );
  }
}
```

## state 올바르게 사용하기

setState()에 대해서
* 집적 State를 수정하지 마세요
* state 업데이트는 비동기적일 수도 있습니다
* state 업데이트는 병합됩니다

## 데이터는 아래로 흐릅니다
> 컴포넌트는 자신의 state를 props로 자식 컴포넌트에게 전달할 수 있습니다   
> 부모,자식 컴포넌트 모두 특정 컴포넌트가 state가 있는지 없는지 알수없고 그들이 함수나 클래스로 정의 되었는지는 신경쓸 필요가 없습니다 이러한 이유 때문에 state는 종종 로컬 또는 캡슐화라고 불립니다 state가 소유하고 설정한 컴포넌트 이외에는 어떠한 컴포넌트에도 접근할 수 없습니다
> 일반적으로 이런 데이터 흐름을 하향식, 단반향식 데이터흐름이라고 불립니다 모든 state는 항상 특정한 커포넌트가 소유하고 있으며 그 state로 부터 파생된 UI ,데이터는 오직 트리구조에서 자신의 아래에 위치한 컴포넌트에게만 영향을 미칩니다
