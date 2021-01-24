# 조건 렌더링

React에서는 원하는 동작을 하는 컴포넌트를 애플리케이션에 상태에 따라서 랜더링 할수 있습니다 즉 원하는 것만 렌더링을 할수 있습니다

예를 들어 아래와 같이 Welcom 이란 함수를 렌더링 할건지 Bye를 렌더링 할껀지 상태에 따라 렌더링을 할수 있습니다

```
function Welcom() {
  return <h1>Welcom </h1>
}

function Bye() {
  return <h1>Bye</h1>
}

function Greeting(props) {
  const isLogin = props.isLogin;
  return isLogin === true ? <Welcom /> : <Bye />;
}

class App extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      // 다른 결과를 렌더링 하고 싶다면 isLogin을 바꿔 주세요
      isLogin : false
    };
  }

  render() {
    return (
      <Greeting isLogin={this.state.isLogin}/>
    );
  }
}
```

## 엘리먼트 변수

엘리먼트를 변수에 저장하여 사용할수 있습니다 출력의 다른 부분은 변하지 않은 채로 컴포넌트의 일부를 조건부로 렌더링 할 수 있습니다

```
function Welcom() {
  return <h1>Plz logout</h1>
}

function Bye() {
  return <h1>Plz login</h1>
}

function Greeting(props) {
  const isLogin = props.isLogin;
  return isLogin === true ? <Welcom /> : <Bye />;
}

function LogInBtn(props) {
  return <button onClick={props.onClick}>logIn</button>;
}

function LogOutBtn(props) {
  return <button onClick={props.onClick}>logOut</button>;
}

class App extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      isLogin : false
    };
  }

  logInBtnClick = () => {
    const isLogin = !this.state.isLogin;
    this.setState({isLogin})
  }
  
  logOutBtnClick = () => {
    const isLogin = !this.state.isLogin;
    this.setState({isLogin})
  }
  
  render() {
    const isLogin = this.state.isLogin;
    let btn; // btn 이라는 변수 선언 (엘리먼트를 저장하기 위해)
    isLogin ? btn = <LogOutBtn onClick={this.logOutBtnClick}/> : btn = <LogInBtn onClick={this.logInBtnClick}/>; // 조건 렌더링
  
    return (
      <>
        <Greeting isLogin={this.state.isLogin}/>
        {btn} //let btn
      </>
    );
  }
}
```

## 논리 연산자 & 삼항 연산자 조건부 렌더링
if로 조건을 주어서 렌더링 하는것도 좋지만 더욱 짧고 간편하게 inline 라인으로 처리 할수 있습니다

```
//if문
  if(isLogin){
    btn = <LogOutBtn onClick={this.logOutBtnClick}/>;
  }else {
    btn = <LogInBtn onClick={this.logInBtnClick}/>;
  }

  // 삼항연산자 (if-else)
  isLogin ? btn = <LogOutBtn onClick={this.logOutBtnClick}/> : btn = <LogInBtn onClick={this.logInBtnClick}/>; 

  //논리 연산자 (&&, ||)
  isLogin &&  btn = <LogOutBtn onClick={this.logOutBtnClick}/>; //JS에선 true && 표현식이면 항상 표현식으로 평가되며 false &&표현식이면 false입니다
  
```

> 논리 연산자 주의할점
>> && 연산자는 둘다 참일때만 결과 값을 반환하며 false이면 React는 무시합니다  
>> 데이터 타입에서 boolean 타입이 아니어도 ture 또는 false로 인식할수 있는 타입의 결과가 있습니다

```
// 만약 아래와 같이 count = 0 일때 렌더링이 된다고 생각한다면, 0 = false이기 때문에 렌더링이 되지않습니다
// 0 = false, 1 = true
render() {
  const count = 0;
  return (
    <div>
      { count && <h1>Messages: {count}</h1>}
    </div>
  );
}
```

## 컴포넌트 렌더링 막기
만약 컴포넌트가 렌더링이 되는것을 막고싶다면 렌더링 결과 대신 null을 반환한다면 해당 컴포넌트의 렌더링을 막을 수 있습니다   
컴포는트의 render 메서드로 부터 null을 반환하는것은 생명주기 메서드 호출에 전형 영향을 주시 않습니다
