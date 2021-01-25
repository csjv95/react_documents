# 리스트와 키

## 리스트 렌더링 (여러 컴포넌트 랜더링)
Javascript에서 map()함수를 사용하여 반환하는 방법과 유사합니다 

```
const App = () => {
  const numbers =[1,2,3,4,5]; 
  const list = numbers.map(item => <li>{item}</li>);  //배열을 엘리먼트 리스트로 만든다
  
  return (
    <ul>
      {list}
    </ul>
  );
} 
  // 이 코드를 실행 했다면
  //'Warning: Each child in a list should have a unique "key" prop '라는 경고문이 표시됩니다
  //다음 key 파트에서 설명
```

## key
key는 React가 어떤 항목 변경,추가 또는 삭제할지 식별하는 것을 돕습니다 key는 엘리먼트에 안정적인 고유성을 부여하기 위해 배열 내부의 엘리먼트에 지정해야 합니다   
앞에서 본 코드를 실행을 할때에도 React가 리스트에서 어떤 항목을 변경,추가,제거 식별을 돕기 위해서 key를 넣어주면 더욱 좋습니다 ( 만약 리스트 항목에 명시적으로 key를 지정하지 않으면 React는 기본적으로 인덱스를 key로 사용합니다 )

```
const App = () => {
  const numbers =[1,2,3,4,5]; 
  const list = numbers.map(item => <li key={item.tostring}>{item}</li>); //key 값을 넣어 식별을 돕는다
  
  return (
    <ul>
      {list}
    </ul>
  );
} 
```
## key 선택하는 가장 좋은 방법
리스트의 다른 항목들 사이에서 해당 항목을 고유하게 식별할 수 있는 문자열을 사용하는 것입니다. 대부분의 경우 데이터의 ID를 key로 사용합니다.

```
const App = () => {
  const list = [
    {
      id : 1,
      Title: 'movie',
      descriptioin : 'I love movie'
    },

    {
      id : 2,
      Title : 'animal',
      descriptioin : 'I love animal'
    }
  ]

  return (
    <ul>
      {list.map(item => <li key ={item.id}>
        <h1>{item.title}</h1>
        <p>{item.descriptioin}</p>
      </li>)}
    </ul>
    
  );
}
```

> 렌더링 한 항목에 대한 알맞은 ID가 없다면 마지막 수단으로 항목의 인데스를 key로 사용할 수 있습니다 그러나 항목의 순서가 바뀔수 있는경우 key에 인덱스를 사용하는 것은 권장하지 않습니다
>> 특히 map() 함수 내부에 있는 엘리먼트에 key를 넣어주는게 좋습니다

## key의 값
Key는 배열 안에서 형제 사이에서 고유해야 하고 전체 범위에서 고유할 필요는 없습니다 다른 배열을 만들 때 동일한 key를 사용할 수 있습니다

## JSX map()
JSX를 사용해서 {}(중괄호) 안에 표현식을 포함 시킬수 있으므로 map()의 결과를 인라인으로 처리해서 코드를 깔끔하게 할수있습니다 하지만 이 방식을 남발하기 보다는 개발자가 무었이 더 깔끔한지 판단해야 하며 많이 중첩이 된다면 차라리 컴포넌트 단위로 나누는것이 더 좋습니다