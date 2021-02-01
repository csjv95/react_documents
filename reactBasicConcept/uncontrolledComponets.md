# 비제어 컴포넌트
대부분의 경우에 폼을 구현하는데 있어서 제어 컴포넌트를 사용하는 것이 좋습니다 제어 컴포넌트에서 폼 데이터는 React 컴포넌트에서 다루어 집니다(state) 대안인 비제어 컴포넌트는 DOM 자체 내에서 폼 데이터가 다루어 집니다   

모든 state 업데이트에 대한 이벤트 핸들러를 작성하는 대신 비제어 컴포넌트를 만들려면 ref를 사용하여 DOM에서 폼 값을 가져오면 됩니다

```
// 비제어 컴포넌트에 단일 이름을 허용 합니다(ref)
class App extends React.Component {
  constructor(props) {
    super(props);
    this.input = React.createRef(); // this.input에 ref
  }

  onSubmit = e => {
    const changeInputValue = this.input.current.value;
    alert(changeInputValue);
  }

  render() {
    return (
      <form onSubmit={this.onSubmit}>
        <input 
          ref={this.input}  // ref
          type='text' 
        />
      </form>
    );
  }
}
```

비제어 컴포넌트는 DOM에 신뢰 가능한 출처를 유지하므로 베제어 컴포넌트를 사용할때 React와 React가 아닌 코드를 통합하는것이 쉽고 지루하지 않을 수 있습니다 하지만 그 외에는 일반적으로 제어된 컴포넌트를 작성해야합니다

## 기본값

React 렌더링 생명주기에서 폼 엘리먼트의 value 어트리뷰트는 DOM value를 대체합니다 비제어 컴포넌트를 사용하면 React 초기값을 지정하지만 그 이후의 업데이트는 제어 하지 않는 것이 좋습니다 이러한 경우에 value 어트리뷰트 대신 defaultValue를 지정할 수 있습니다 컴포넌트가 마운트된 후에 defaultValue 어트리뷰트를 변경해도 DOM의 값이 업데이트 되지 않습니다 

>  `<input type='checkbox'>`와 `<input type='radio'>`는 defaultChecked를 지원하고 `<select>`와 `<textarea>`는 defaultValue를 지원합니다.

## 파일 입력 태그

HTML에서 `<input type = 'file'/>`은 사용자가 장치 저장소에서 하나 이상의 파일을 선택하여 서버에 업로드 하거나 파일 API를 사용하여 Javascript로 조작할 수 있습니다  

React에서 `<input type = 'file'/>`은 프로그래밍 적으로 값을 설정할 수 없고 사용자만이 값을 설정할 수 있기때문에 항상 비제어 컴포넌트를 사용해야 합니다   

파일 API를 사용하여 파일과 상용작용해야 합니다 
```
// 제출 핸들러에서 파일에 접근하기 위해서 DOM 노드의 ref를 만든다

class App extends React.Component {
 constructor(props) {
   super(props);
   this.value = React.createRef();  //ref
 }

 onSubmit = e => {
  alert(`Is this your choice file ? ${this.value.current.value}` )
 }

 render() {
   return (
     <form onSubmit={this.onSubmit}>
       <input 
        ref={this.value}  // ref
        type='file'
      />
      <input type='submit' value='submit'/>
     </form>
   );
 }
}

```