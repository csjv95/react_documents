# React 적으로 사고 하기

React는 Javascript로 규모가 크고 빠른 웹 애플리케이션을 만드는 가장 좋은 방법입니다 React는 Facebook과 Instargram을 통해 확장성을 입증했습니다

React의 가장 멋진 점 중하나는 앱을 설계하는 방식 입니다

## 상품들을 검색할 수 있는 데이터 테이블 만들기

JSON API retun
```
  {category: 'Sporting Goods', price: '$50.55', stocked: true, name: 'Football'},
  {category: 'Sporting Goods', price: '$10.00', stocked: true, name: 'Baseball'},
  {category: 'Sporting Goods', price: '$20.22', stocked: false, name: 'Basketball'},
  {category: 'Electronics', price: '$100.77', stocked: true, name: 'ipad'},
  {category: 'Electronics', price: '$150.88', stocked: false, name: 'iPhone 8'},
  {category: 'Electronics', price: '$120.99', stocked: true, name: 'Galaxy 8'}
```

### Step 1 : UI를 컨포넌트 계층 구조로 나누기

첫번째 해야 할 일은 모든 컴포넌트의 주변에 박스를 그리고 그 각각에 이름을 붙이는 것입니다

하지만 어떤 것이 컴포넌트가 되어야 할지 어떻게 알수 있을까요 바로 우리가 항상 새로운 함수나 객체를 만들때 처럼 만드시면 됩니다 비결은 단일 책임 원칙 입니다 이는 하나의 컴포넌트는 한 가지 일을 하는게 이상적이라는 원칙입니다 하나의 컴포넌트가 커지게 된다면 이는 보다 작은 하위 컴포넌트로 분리 하시는게 좋습니다

주로 JSON 데이터를 유저에게 보여주기 때문에 데이터 모델이 적적하게 만들어 졌다면 UI가 잘 연결 될것입니다 이는 UI와 데이터 모델이 같은 인포메이션 아키텍처를 가지는 경향이 있기 때문입니다 각 컴포넌트가 데이터 모델의 한 조각을 나타내도록 분리해주세요

받은 데이터를 계층구조로 나타내어 보면

* FilterableProductTable
  * InputBox
    * Input
    * CheckBox
  * Output
    * OutHead
    * OutBody


### step 2 : React로 정적인 버전 만들기

컴포넌트 게층구조가 만들어졌으니 앱을 실제로 구현 해볼 시간입니다 가장 쉬운 방법은 데이터 모델을 가지고 UI 렌더링만 되는 버전을 만들어  이처럼 과정을 나누는 것이 좋은데 정적 버전을 만드는 것은 생각은 적게 필요하지만 타이핑은 많이 필요하고 상호작용을 만드는 것은 생각은 많이 해야 하지만 타이핑은 적게 필요하기 때문입니다

#### props vs state
데이터 모델을 렌더링 하는 앱의 정적 버전을 만들기 위해 다른 컴포넌트를 재사용하는 컴포넌트를 만들고 props를 이용해 데이터를 전달해줍니다 props는 부모가 자식에게 데이터를 넘겨 줄때 사용하는 방법입니다  정적 버전을 만들기 위해 state를 사용하지 마세요 stat는 오직 상호작용을 위해 즉 시간이 지남에 따라 데이터가 바뀌는 것에 사용합니다 

#### 하향식 vs 상향식
앱을 만들때 하향식,상향식 으로 만들수 있습니다 하향식이란 계층구조에 상층부에서 하층부 순서로 만들어 나가는 것이고 상향식은 계층구조에 하층부에서 상층부 순서로 만들어 나가는 것입니다 간단한 예시는 보통 하향식으로 만들면 더 쉽지만 프로젝트가 커지면 상향식으로 개발하는 것이 더 쉽습니다

```
const Data = [
  {category: 'Sporting Goods', price: '$50.55', stocked: true, name: 'Football'},
  {category: 'Sporting Goods', price: '$10.00', stocked: true, name: 'Baseball'},
  {category: 'Sporting Goods', price: '$20.22', stocked: false, name: 'Basketball'},
  {category: 'Electronics', price: '$100.77', stocked: true, name: 'ipad'},
  {category: 'Electronics', price: '$150.88', stocked: false, name: 'iPhone 8'},
  {category: 'Electronics', price: '$120.99', stocked: true, name: 'Galaxy 8'}
];

const InputBox = () => {
  return (
    <header>
      <Input />
      <CheckBox />
    </header>
  );
};

const Input = () => {
  return <input type='text' />;
};

const CheckBox = () => {
  return (
    <label>
      Only show products in stock
      <input type='checkbox' />
    </label>
  );
};

const OutputHead = () => {
  return (
    <thead>
      <tr>
        <th>Name</th>
        <th>Price</th>
      </tr>
    </thead>
  );
};

const OutputBody = (props) => {
  const data = props.data;
  return (
    <tbody>
      
      <tr>
        <th colSpan='2'>Sporting Goods</th>
      </tr>

      {data.map((item) =>
        item.category === 'Sporting Goods' ? (
          <tr>
            <td>
              {item.stocked ? (
                item.name
              ) : (
                <td style={{ color: 'red' }}>{item.name}</td>
              )}
            </td>
            <td>{item.price}</td>
          </tr>
        ) : null
      )}

      <tr>
        <th colSpan='2'>Electronics</th>
      </tr>

      {data.map((item) =>
        item.category === 'Electronics' ? (
          <tr>
            <td>{item.name}</td>
            <td>{item.price}</td>
          </tr>
        ) : null
      )}
    </tbody>
  );
};

const Output = (props) => {
  return (
    <table>
      <OutputHead />
      <OutputBody data={Data}/>
    </table>
  );
};

const FilterableProductTable = () => {
  return (
    <main>
      <InputBox />
      <Output data={Data} />
    </main>
  );
};
```

### step 3: UI state에 대한 최소한의 표현 찾아내기

UI를 상호작용하게 만들려면 기반 데이터 모델을 변경할 수 있는 방법이 있어야 합니다 이를 state를 통해 변경합니다

애플리케이션을 올바르게 만들기 위해서는 애플리케이션에서 필요하는 변경 가능한 state를 최소 집합을 생각해야 합니다 여기서 핵심은 중복배체 원칙입니다 애플리케이션이 필요로 하는 가장 최소한의 state를 찾고 이를 통해 나머지 모든 것들이 필요에 따라 그때그때 계산 되도록 만드세요 예를들어 TODO 리스트를 만든다고 하면 TODO 아이템을 저장하는 배열만 유지하고 TODO 아이템의 개수를 표현하는 state를 별도로 만들지 마세요 TODO갯수를 렌더링해야 한다면 TODO 아이템 배열의 길이를 가져오면 됩니다

예시 애플리케이션 내 데이터를 생각해보면 다음과 같은 데이터를 가지고 있습니다

* 제품의 원본 목록
* 유저가 입력한 검색어
* 체크박스의 값
* 필터링된 제품들의 목록

각각 살펴보고 어떤게 state가 되어야 하는지 살펴봅시다 이는 각 데이터에 대해 아래의 세가지 질문을 통해 결정할 수 있습니다

* 부모로부터 props를 통해 전달 됩니다 not state
* 시간이 지나도 변하지 않습니다 // not state
* 컴포넌트 안의 다른 state나 props를 가지고 계산이 가능합니다 //not state

제품의 원본 목록은 props를 통해 전달되므로 state가 아닙니다 검색과 체크박스는 시간이 지남에 따라 변하기도 하고 다른 것들로 부터 계산될 수 없기 떄문에 state로 볼수 있습니다 마지막으로 필터링 항목은 젬품의 원본 목록과 검색어,체크박스의 값을 조합해서 계산해야 함으로 state가 아닙니다

결과적으로 이 애플리케이션 state는

* 유저가 입력한 검색어 (input)
* 유저가 입력하는 체크박스 (input)

### step 4 : state가 어디에 있어야 할지 찾기

어떤 컴포넌트가 state를 변경하거나 소유할지 찾아야 합니다

많은 React 초보자분들이 어떤 컴포넌트가 state를 가져야 하는지 어려워 합니다 React는 항상 컴포넌트 계층구조를 따라 알로 내려가는 단방향 데이터 흐름을 따릅니다

애플리케이션이 가지는 각각의 state에 대해서

* state를 기반으로 렌더링 하는 모든 컴포넌트를 찾으세요
* 공통 소유 컴포넌트를 찾으세요
* 공통 혹은 더 상위의 컴포넌트가 state를 가져야 합니다
* state를 소유할 적절한 컴포넌트를 찾지 못했다면 state를 소유하는 컴포넌트를 하나 만들어서 공통의 오너 컴포넌트를 상위 계층에 추가하는 방법도 있습니다

이 전략을 애플리케이션에 적용한다면 

* InputBox와 OutputBox는 검색,체크,상품리스트를 필터링 하여 표시해주어야 합니다
* 공통 소유컴퍼넌트는 FilterableProductTable 입니다
* 공통 혹은 최상위 컴포넌트인 FilterableProductTable 가 state를 가지게 합니다

```

```

### step 5 : 역방향 데이터 흐름 추가하기

```
const Data = [
  {
    category: 'Sporting Goods',
    price: '$50.55',
    stocked: true,
    name: 'Football',
  },
  {
    category: 'Sporting Goods',
    price: '$10.00',
    stocked: true,
    name: 'Baseball',
  },
  {
    category: 'Sporting Goods',
    price: '$20.22',
    stocked: false,
    name: 'Basketball',
  },
  { category: 'Electronics', price: '$100.77', stocked: true, name: 'ipad' },
  {
    category: 'Electronics',
    price: '$150.88',
    stocked: false,
    name: 'iPhone 8',
  },
  {
    category: 'Electronics',
    price: '$120.99',
    stocked: true,
    name: 'Galaxy 8',
  },
];

const InputBox = (props) => {
  return (
    <header>
      <Input inputText={props.inputText} changeText={props.changeText} />
      <Check isStock={props.isStock} changeChek={props.changeChek} />
    </header>
  );
};

const Input = (props) => {
  const onChange = (e) => {
    const targetValue = e.target.value;
    return props.changeText(targetValue);
  };
  
  return <input type='text' value={props.inputText} onChange={onChange} />;
};

const Check = (props) => {
  const onChange = (e) => {
    const targetValue = e.target.checked;
    return props.changeChek(targetValue);
  };
  
  return (
    <label>
      Only show products in stock
      <input type='checkbox' checked={props.isStock} onChange={onChange} />
    </label>
  );
};

const OutputBox = (props) => {
  return (
    <table>
      <OutputHead />
      <OutputBody data={Data} />
    </table>
  );
};

const OutputHead = () => {
  return (
    <thead>
      <tr>
        <th>Name</th>
        <th>Price</th>
      </tr>
    </thead>
  );
};

const OutputBody = (props) => {
  const data = props.data;

  return (
    <tbody>
      <tr>
        <th colSpan='2'>Sporting Goods</th>
      </tr>

      {data.map((item) =>
        item.category === 'Sporting Goods' ? (
          <tr>
            <td>
              {item.stocked ? (
                item.name
              ) : (
                <td style={{ color: 'red' }}> {item.name}</td>
                )}
            </td>
            <td>{item.price}</td>
          </tr>
        ) : null
        )}

      <tr>
        <th colSpan='2'>Electronics</th>
      </tr>

      {data.map((item) =>
        item.category === 'Electronics' ? (
          <tr>
            <td>{item.name}</td>
            <td>{item.price}</td>
          </tr>
        ) : null
      )}
    </tbody>
  );
};

class FilterableProductTable extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      inputText: '',
      isStock: false,
    };
  }

  changeText = (inputText) => {
    console.log(inputText);
    this.setState({ inputText: inputText });
  };

  changeChek = (isStock) => {
    this.setState({ isStock: isStock });
  };

  render() {
    return (
      <main>
        <InputBox
          inputText={this.state.inputText}
          isStock={this.state.isStock}
          changeText={this.changeText}
          changeChek={this.changeChek}
        />
        <OutputBox />
      </main>
    );
  }
}
```