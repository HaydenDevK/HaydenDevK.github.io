---
title: "[회고] 수코딩x스나이퍼팩토리x유데미 Next.js 프로젝트 캠프 2주차"
date: 2024-06-07 19:00:00 +09:00
categories: [Dev Journal, "2024"]
image: "/assets/img/post/2024-06-07-sniper-factory-week02.png"
tags:
  [
    수코딩,
    스나이퍼 팩토리,
    유데미,
    웅진씽크빅,
    sucoding,
    sniper factory,
    udemy,
    woongjin thinkbig,
    인사이드아웃,
    Next.js
  ]
---

## Liked

이번 주엔 React.js의 rerendering과 memoization, 상!태!관!리! 등 중요한 내용을 속도감 있게 훑었고,<br/>
node.js로 간단한 서버도 만들어볼 수 있었으며,<br/>
React-router-dom, Zustand, Next의 App Router에 대해서도 톺아보았다.<br/>
중간중간 실무 활용 예시도 알려주셨는데, 그런 게 개인적으로 너무 달달했고 꼼꼼히 흡수하려 했다.🍯<br/>

## Learned

### React.js 컴포넌트는 언제 리렌더링 되는가

- "탐조하고 있는 상태 값이 변경되었을 때"
- 복잡하게 생각했었는데 크게 보면 이것 뿐이었다.
- 리렌더링 범위 : 최하위 컴포넌트까지
- 그렇기 때문에 필연적으로 불필요한 리렌더링도 많은 것이다.
- 불필요한 리렌더링을 줄이려면?
  - state를 가지는 컴포넌트는 가장 하위에 두자!
  - 그게 안된다면? 메모이제이션을 통해 최적화 하자!

### 리스트(상위 컴포넌트)가 길 때, 아이템(하위 컴포넌트)을 메모이제이션 해야하는가?

**list가 바뀌면 item도 리프레시 되어야 하는 거 아닌가? 왜 리렌더링을 방지해야 할까**

> - 아이템 입장에서 나는 내가 가진 item이 바뀔 때만 리렌더링 하고싶다! 변화가 없는데 왜 내가 리렌더링 해야함! 나는 나의 item 데이터를 이미 가지고 있다!
>   - item 자신이 바뀌는 경우란? 내가 가진 item을 수정할 때! tem 자신이 바뀌지 않는 경우란? list에 (내가 아닌 다른) item이 추가되거나 삭제되거나 변경될 때!<br/>

**리스트가 길면 무슨 상관일까?**

>     길이가 긴 리스트라면 그 만큼 리렌더링 방지 효과가 크기 때문에 (메모이제이션 자체가 무거울지라도) 메모이제이션 할 의미나 효용이 있다.
>     **메모이제이션을 안하면 리렌더링이 왜 되는걸까?**
>
> - 아이템은 리스트의 상태(list[index])를 prop으로 전달 받고 있을 것이다.
> - 리스트의 상태가 setState 되면 -> 리스트 리렌더링 -> 하위 컴포넌트인 아이템 컴포넌트에도 리렌더링이 발생된다.<br/>

그럼 **어떻게 메모이제이션 하면 될까?**

> - 리렌더링을 방지하고 싶은 item을 메모이제이션 한다.<br/>

**구체적으로 어떻게?**

> 1. item을 React.memo()로 감싼다.
> 2. item이 받는 prop 중에 참조 자료형이 있을 경우에는, 리스트가 리렌더링 되면 함수나 객체가 실제로 재생성될 필요가 없어도 재생성되어서 불필요하게 하위 컴포넌트(리스트 아이템)가 리렌더링 된다. 즉 메모이제이션 실패! -> 이 경우에는 해당 참조 자료형도 메모이제이션을 해야한다.

### TodoList로 반복 생성되는 컴포넌트 메모이제이션 전후 차이 살펴보기

**메모이제이션 X**

```
Todo
ㄴTodoList
ㄴㄴTodoItem
```

> 위와 같은 구조에서 1번 TodoItem을 클릭하면 -> (Todo의 list가 변화 -> Todo 리렌더링 -> TodoList 리렌더링 -> TodoItem 모~두가 리렌더링 되기 때문에) 다른 2번, 3번, 4번, 5번도 리렌더링이 발생한다.

**메모이제이션 방법**

1. TodoItem 컴포넌트를 React.memo로 감싼다.

```ts
export default React.memo(TodoListItem);
```

2. (TodoList가 리렌더링 되면서 TodoItem에 props로 내려주는 함수들도 재생성 되어 버려서) TodoItem 메모이제이션이 실패하기 때문에, props로 내려주는 함수들도 useCallback으로 메모이제이션을 해준다.

### Context API의 단점과 보완 방법

#### Context가 바뀌면 (구독하든 하지 않든) 래핑하고 있는 모든 하위 컴포넌트가 리렌더링 된다.

**보완 방법**

```ts
import Home from "./components/Home";
import Navbar from "./components/Navbar";
import { CounterProvider } from "./components/CountContext";

function App() {
  console.log("App");

  return (
    <>
      <CounterProvider>
        <Navbar />
        <Home />
      </CounterProvider>
    </>
  );
}

export default App;
```

```ts
import { createContext, useState } from "react";

type CountContextType = {
  count: number;
  setCount: React.Dispatch<React.SetStateAction<number>>;
};

const initialCount = {
  count: 0,
  setCount: () => {}
};

export const CountContext = createContext<CountContextType>(initialCount); // 초기 값

export const CounterProvider = ({
  children
}: {
  children: React.ReactNode;
}) => {
  const [count, setCount] = useState(0);

  return (
    <CountContext.Provider value={{ count, setCount }}>
      {children}
    </CountContext.Provider>
  );
};
```

```ts
import { useContext } from "react";
import { CountContext } from "./CountContext"; // 변경

const Child1 = () => {
  const countContext = useContext(CountContext);
  return <h1>Child1 Count: {countContext.count}</h1>;
};

export default Child1;
```

> 앱 전체를 감싸지 않고, 해당 컨텍스트가 필요한 컴포넌트만 감싸도록 하면 컨텍스트를 구독하지 않는 컴포넌트는 리렌더링되지 않는다. <br>

### React-router-dom의 해시 라우터 vs 브라우저 라우터

- 해시 라우터: URL에 (#)가 붙어서 구성되는 형식(https://www.example.com/#/login)
- 브라우저 라우터: URL에 슬래시(/)가 붙어서 구성되는 형식(https://www.example.com/login)
- 우리에게 익숙한 라우터 방식은 브라우저 라우터 방식이다.

### Next.js의 Params

- params Props는 디폴트로 전달된다.

```js
export default function Page({ params, searchParams }) {
  const { id } = params;
  const { option1, option2 } = searchParams;
  return (
    <>
      <h1>id: {id}</h1> // 1<p>option1 : {option1} </p> // hi
      <p>option2 : {option2}</p> // bye
    </>
  );
}
```

> `/1?option1=hi&option2=bye`

### Next.js의 포괄적 경로 (catch all segments)

- 구조

```
page1
ㄴpage2
ㄴpage3
ㄴ[...slug]
```

> 이런 구조라면 `/page1/page2`, `/page1/page3` 외에 `/parent/anything`으로 접근 시 anything이 뭐든 다 `/[...slug]/pages.tsx`가 렌더링된다.

- 코드

```ts
const Slug = ({
  params
}: {
  params: {
    slug: string[];
  };
}) => {
  // page1 이후의 경로가 slug에 배열 타입으로 들어온다.
  // ex) /page1/1/user/1 -> [1, user, 1]
  return (
    <>
      <h1>Slug Component</h1>
    </>
  );
};
export default Slug;
```

- 어떤 경우에 유용하게 사용할 수 있을지는 아직 잘 모르겠다!

---

본 후기는 [유데미x스나이퍼팩토리] 프로젝트 캠프 : Next.js 1기 과정(B-log) 리뷰로 작성 되었습니다.

#유데미 #udemy #웅진씽크빅 #스나이퍼팩토리 #인사이드아웃 #미래내일일경험 #프로젝트캠프 #부트캠프 #Next.js #프론트엔드개발자양성과정 #개발자교육과정
