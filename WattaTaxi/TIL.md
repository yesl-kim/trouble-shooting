0526

1. 전반적인 스타일은 같은데 부분적으로 다른 스타일이 적용되어야 할 때

- 스타일이 부분적으로 다를 때 : **조건부 스타일** 또는 **스타일 확장**

- 스타일은 같은데 태그가 다를 때 : **'as' 속성**

```js
render(
  <div>
    <Button>Normal Button</Button>
    <TomatoAnchorButton as="a" href="/">Tomato Button</TomatoAnchorButton>
  </div>
);

const TomatoAnchorButton = styled(Button)`
  color: tomato;
  border-color: tomato;
```

구버전의 `withComponent`나 `extend`를 사용할 수도 있지만 `extend`는 삭제되었고 `withComponent`는 삭제 후보 명단에 올라와있기 때문에 권장하지 않는다. (-스타일 컴포넌트 공식문서)

0527

1. (예시) 반복되는 요소의 id값을 위해 컴포넌트 내부에 변수를 선언하는 것을 지양하고 useRef 로 선언하는 것을 지향하는 이유

- useRef는 일반적인 자바스크립트 객체입니다 즉 heap 영역에 저장됩니다
  그래서 어플리케이션이 종료되거나 가비지 컬렉팅 될 때 까지 참조할 때 마다 같은 메모리 주소를 가지게 되고
  같은 메모리 주소를 가지기 때문에 === 연산이 항상 true를 반환하고, 값이 바뀌어도 리렌더링 되지 않습니다.

하지만 함수 컴포넌트 내부에 변수를 선언한다면, 렌더링 될 때마다 값이 초기화 됩니다.
그래서 해당 방법을 지양하는 것 같습니다 :)

아래처럼 기존에 존재하는 배열에 새로운 항목을 추가할 때, id값을 지정하기 위해

```js
import React, { useRef } from "react";
import UserList from "./UserList";

function App() {
  const users = [
    {
      id: 1,
      username: "velopert",
      email: "public.velopert@gmail.com",
    },
    {
      id: 2,
      username: "tester",
      email: "tester@example.com",
    },
    {
      id: 3,
      username: "liz",
      email: "liz@example.com",
    },
  ];

  const nextId = useRef(4);
  const onCreate = () => {
    // 나중에 구현 할 배열에 항목 추가하는 로직
    // ...

    nextId.current += 1;
  };
  return <UserList users={users} />;
}

export default App;
```

````
다음과 같은 방법은 지양 ⬇️
```js
// 컴포넌트 내부에 객체를 직접선언
const nextId = { current: 4 };

// 혹은 변수 선언
let nextId = 4;
````

컴포넌트가 리렌더 될때마다 위와 같은 값은 다시 초기화되기 때문에
이런 경우 useRef를 권장

## 이벤트 버블링과 캡처링

- 이벤트 순서
  이벤트 캡처링 -> 이벤트 타깃 -> 이벤트 버블링

## 쿼리스트링을 객체로 가져오는 함수만들기

1. https://opentutorials.org/course/1376/7369

## 암호화되기 전의 문자열로 쿼리스트링 가져오기 : decodeURI()
