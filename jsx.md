# JSX

## JSX의 유래
JSX는 Javascript + XML가 합쳐 탄생한 Javascript 확장 문법이며 의 모든 기능이 포함 되어 있습니다

## JSX란

아래와 같이 Javascript에 마크업을 넣어서 사용한다

```
  const element = <h1>Hello world</h1>;
```

JSX에 표현식 포함하기

```
//JSX의 중괄호 안에는 유효한 모든 Javascript 표현식을 넣을수 있다
//변수 

const name = 'SJ';
const element = <h1>What's up {name}</h1>;  

ReactDOM.render (
  element,
  document.getElementById('root');
);
```

```
//함수

fucntion useName(user) {
  return `${user.firstName} + ${user.lastName}`;

  const user = {
    firstName : 'Choi'.
    lastName : 'SeokJin'
  }

  const element = (
    <h1>
      What's up {useName(user)}
    </h1>
  );

  ReactDOM.render (
    element,
    document.getElementById('root');
  );
}
```

## JSX도 표현식
컴파일이 끝나면 JSX에 작성된 표현식들이 정규 Javascript 함수 호출이 되고 Javascript객체로 인식이됩니다
JSX는 HTML보다는 JS에 더 가까워 React DOM은 HTML 속성 이름 대신에 camelCase로 정한 속성명 규칙을 사용합니다 
>> 예를 들어, HTML에서는 class이름명 속성이 class였고 JSX에서는 camelCase규칙으로 정해진 className이 됩니다

## JSX 속성 정의
속성에 "" (따옴표)를 사용하여 문자열 리터럴을 정의 할수있습니다 
```
const element = <div tabIndex="0" />;
```

속성에 {} (중괄호)를 사용하여 Javascript 표현식을 삽입할수도 있습니다
```
const element = <img src={user.avatarUrl} alt={user.avatar} />
```

## JSX로 자식의 정의

태그가 비었다면 XML처럼 /를 이용해 닫아주어야 합니다
```
const meet = <h1>Hi</h1> 

const userAvatar = <img src={user.avataUrl} alt={user.avatar} />
```

JSX태그는 자식을 포함할 수 있습니다

```
const meet = (
  <ul>
    <li>name : SJ</li>
    <li>age : 26 </li>
  <ul>
)
```
JSX 태그는 항상 2개이상이 되면 그 태그들을 감싸줘야 합니다 

```
<>
  <Header />
  <Main />
</>

<ul className='food__list'>
  <li className='food'>Apple</li>
  <li className='food'>Orange</li>
</ul>
```

## JSX 주입공격 방지

JSX에 사용자 입력을 포함하는 것이 안전합니다  
기본적을 ReactDOM은 JSX에 포함된 갑을 렌더링 하기전에 나갑니다 따라서 앱에서 명시적으로 작성되지 않은 것은 주입이 불가능 합니다 모든 것이 렌더링이 되기전에 문자로 변환되는데 이는 사이트간에 스크립팅 공격을 방지하는데 도움이 됩니다

## JSX는 객체를 나타냅니다

Babel은 JSX를 컴파일하여 React.createElement()를 호출합니다.

아래 두가지 예들은 동일 합니다
```
const element = (
  <h1 className='greeting'>
    Nice meet you~!
  </h1> 
);
```

```
const element = React.createElement(
  'h1'
  {className: 'greeting'},
  'Nice meet you~!'
);
```

React.createElement() 버그없는 코드를 작성하는데 도움이 되는 몇가지 검사를 수행하지만 기본적으로 다음과 같은 객체를 생성합니다

```
//심플하게 작성되었습니다
const element = {
  type : 'h1',
  props {
    className : 'greeting',
    children : 'Nice meet you~!'
  }
};
```
이러한 개체를 반응 요소라고 합니다 화면에서 보고싶은 내용에 대한 설명으로 생각하고 React는 이러한 객체의 내용의 설명을 읽고 DOM를 구성하고 유지합니다