# SearchBox

1. flexbox의 justify-content: space-between 해도 양옆 여백이 남아있는 경우

```js
<form>
  <Item size="40%" />
  <Item size="30%" />
  <Item size="21%" />
  <Item size="7%" />
</form>;

const Form = styled.form`
  display: flex;
  justify-content: space-between;
  align-items: stretch;
`;
const Item = styled.div`
  flex-basis: ${({ size }) => size};
`;
```

상황

- 부모에 {justify-content: space-between;} 속성을 주고, 자식에는 `flex-basis` 속성으로 기본 너비를 지정해준 상태
- 나머지 2%가 여백이 되게 하기 위해 자식 요소 너비의 총값이 100%가 되지 않는다 (총 98%).

문제

- 가장 첫번째 요소의 왼쪽에도 여백이 생긴다.

해결

- 자식 요소 너비의 총값이 100%가 되게 하고 {margin-left} 값을 주어야 한다.
- 첫번째 자식요소의 margin-left는 따로 제거해주어야 한다.

~~왜 안될까. 왜~~

2. input 스타일 컴포넌트(`styled.input`)의 readonly 속성이 적용되지 않는 이유

```
// html
<input readonly />

// JSX
const Input = style.input`...`;

<Input readOnly={true} />
```

`readonly` 속성을 `readOnly` 처럼 카멜케이스로 작성해주어야 한다.

리액트에서는 이벤트 핸들러나 css 속성 등을 모두 카멜케이스로 작성해야하는 것처럼.

## 모달창 html 문서상의 위치

상황

- 위치, 날짜, 인원수 3개의 검색메뉴와 검색메뉴를 클릭했을 때 나타나는 모달창
- 검색메뉴를 클릭했을 때 모달창이 화면 상에서는 클릭된 검색메뉴 기준으로 위치하는데, (검색메뉴의 바로 아래, 왼쪽 정렬) html 상에서는 다른 부모 요소 아래 있기 때문에

해결

- 검색메뉴와 모달창은 같은 부모 요소 아래 있도록 하자
- 검색 메뉴 컴포넌트 안에

## state 끌어올리기

인자로 전달
함수를 prop으로 전달할 때 () => ㅁㄴㅇㄹㅁ()
처럼 함수 형태로 전달해야함

## input checked error

Warning: A component is changing an uncontrolled input to be controlled.

상황
- 라디오박스의 checked 속성을 컴포넌트의 state로 제어
- 에러 발생  
`Warning: A component is changing an uncontrolled input to be controlled.`

과정
- value 대신 defaultValue로 값 지정

- 이건 uncontrolled component로 만든 것 같은데 내 상황은 분명히 input의 value를 컴포넌트에서 제어해야 하는 상황이므로 맞지 않는다.
역시 defaultValue로 수정해도 해결은 되지 않았다.

- **uncontrolled component**란,  
초깃값은 지정하지만 그 후에 업데이트에 대해서는 제어하지 않는 컴포넌트  
~~그런데 그걸 꼭 defaultValue로 지정해줘야 하는 이유가 있나, 그냥 상수로 전달하면 되지 않나~~




## input checked 동적으로 제어하는 문제

```js
{
  seats.map((seat) => {
    console.log(seat.name, seat.name === selectedSeat);
    return (
      <li key={seat.id}>
        <input
          type="radio"
          name="seat"
          id={`seat${seat.id}`}
          defaultValue={seat.name}
          onClick={(e) => handleSeat(e)} // <----
          checked={seat.name === selectedSeat}
        />
        <label htmlFor={`seat${seat.id}`}>{seat.name}</label>
      </li>
    );
  });
}
```

- defaultValue 속성 필요없고
- 그냥 잘못된 이벤트를 걸어서 문제  
checked 속성을 제어하고 싶을 때는 onChage 이벤트 핸들러를 걸어주어야 한다.
```
Warning: You provided a `checked` prop to a form field without an `onChange` handler. This will render a read-only field. If the field should be mutable use `defaultChecked`. Otherwise, set either `onChange` or `readOnly`.
```

## useState() 인자로 값을 넣어주는 것과 함수를 넣어주는 것의 차이

정확히 차이점이 뭘까, 똑같은거아닌가?
헷갈린다 헷갈려

## 이벤트 버블링. 어디에 event.preventDefault를 걸어야할까

form 요소를 form 태그로 감싸다 보니 모든 요소에 preventDefualt()를 걸어야하는문제가 있다. (x)

`<button> type`속성의 기본값은 submit.  
submit 버튼의 경우 화면을 새로고침하기 때문에 e.preventDefault()가 필요하다. form 태그로 감싸서가 아니라 button 을 클릭했을 때 '제출'이 되기 때문에 새로고침 되었던 것이다. 버튼을 버튼 용도로 쓰고 싶다면 type="button"을 지정해주어야한다.

## e.preventDefault() vs e.stopPropagation()

## Styled component 사용팁!

- 편도/ 왕복에 따른 탭 버튼 스타일링 리뷰

이 코드의 의미는 결국 왕복인지, 편도인지 둘 중 하나를 선택하는 것이네요. boolean 값 하나에 따라 동일한 컴포넌트가 다른 모양을 보여주면 되는 것이기 때문에 다른 컴포넌트로 바꿔주지 말고, 하나의 컴포넌트가 두 가지 스타일을 가지도록 하는게 코드만 보고 형태를 파악하기에 좋겠습니다.

```js
function SearchMenu() {
  const [isRoundTrip, setIsRoundTrip] = useState(true);

  const tabButtonProps = {
    roundTrip: {
      isSelected: isRoundTrip,
      onClick: () => setIsRoundTrip(true),
    },
    oneWayTrip: {
      isSelected: !isRoundTrip,
      onClick: () => setIsRoundTrip(false),
    },
  };

  return (
    <Container>
      <Tab>
        <TabBtn {...tabButtonProps.roundTrip}>왕복</TabBtn>
        <TabBtn {...tabButtonProps.oneWayTrip}>편도</TabBtn>
      </Tab>
      <SearchBar isRoundTrip={isRoundTrip} />
    </Container>
  );
}

const TabBtn = styled.li`
  margin-right: 8px;
  padding-bottom: 8px;
  width: 64px;
  font-size: 16px;
  text-align: center;
  color: #fff;
  cursor: pointer;
  opacity: 0.5;

  ${({ isSelected }) => {
    return (
      isSelected &&
      css`
        border-bottom: 2px solid #fff;
        font-weight: normal;
        opacity: 1;
      `
    );
  }}

  &:hover {
    opacity: 0.9;
  }
`;
```

1. 출발지 초기값을 정하는 것처럼, 초기값을 정하는 건 프론트에서 하는 게 좋을까 백엔드에서 하는 게 좋을까.

- 프론트. 이런 데이터가 그렇게 중요해 보이진 않아서, db에까지 추가할 필요는 없어보인다.
