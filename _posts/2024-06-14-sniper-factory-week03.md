---
title: "[회고] 수코딩x스나이퍼팩토리x웅진싱크빅x유데미 Next.js 프로젝트 캠프 : Pre-Course Week3"
date: 2024-06-14 19:00:00 +09:00
categories: [Dev Journal, "2024"]
image: "/assets/img/post/2024-06-14-sniper-factory-week03.png"
tags:
  [
    Frontend,
    Javascript,
    JS,
    React,
    Next.js,
    App Router,
    수코딩,
    sucoding,
    스나이퍼 팩토리,
    sniper factory,
    유데미,
    udemy,
    웅진씽크빅,
    woongjin thinkbig,
    인사이드아웃,
    부트캠프,
    bootcamp
  ]
---

## Liked

Next.js에서 가장 중요하다고 할 수 있는 RSC/RSC, Hydration에 대해 보다 깊게 이해할 수 있었고<br>
Streaming, Suspense, Route Handler, Server Action, Cache도 상세히 다뤄주셔서 개인적으로 짱짱 유익했다!<br>
더불어 몽고디비와 Auth.js를 사용한 실습은 재밌으면서 어마어마하게 실용적이었다...🫶<br>
무엇보다 RQ 없이 Nest.js 14 버전의 Cache 시스템과 Zustand만을 사용해서 RSC에서 상태를 관리하는 패턴을 시도해보고 싶다고 생각하게 되었는데, 나름의 차별점이 될 것 같다!❤️‍🔥

## Learned

### VSCode에서 Next.js 컴포넌트 스니펫 설정 방법

```js
{
  "Next Function Component": {
  "prefix": "nfce",
  "body": [
    "export default function ${1:$TM_FILENAME_BASE} (){",
    "  return (",
    "    <>",
    "      <h1>${1:$TM_FILENAME_BASE} Component</h1>",
    "    </>",
    "  );",
    "}"
  ],
  "description": "Create an Next function component"
}
```

> VSCode > setting > user snippets > typescriptreact.json >

### Streaming과 Suspense

#### 1. 상위 컴포넌트가 클라이언트 컴포넌트이고, 하위 컴포넌트가 서버 컴포넌트일 때?

- 하위 컴포넌트는 모두 클라이언트 컴포넌트가 된다.
- 따라서 서버 컴포넌트인 하위 컴포넌트에서 아래와 같은 에러가 난다.

```
Warning: SOMETHING is not yet supported in Client Components, only Server Components. This error is often caused by accidentally adding `'use client'` to a module that was originally written for the server.
```

#### 2. 서버 컴포넌트는 동기적으로 실행된다. 따라서 async await로 지연이 발생하면 그만큼 딜레이가 발생한다.

```ts
import ServerComp1 from "@/components/ServerComp1";
import ServerComp2 from "@/components/ServerComp2";

export default function AboutPage() {
  return (
    <>
      <h1>AboutPage Component</h1>
      <ServerComp1 /> {/* 2초 로딩 */}
      <ServerComp2 /> {/* 6초 로딩 => 이 로딩이 끝날 때까지 지연 */}
    </>
  );
}
```

```ts
// 1
export default async function ServerComp1() {
  await new Promise((resolve) => setTimeout(resolve, 6000));
  return (
    <>
      <h1>ServerComp1 Component</h1>
    </>
  );
}

// 2
export default async function ServerComp2() {
  await new Promise((resolve) => setTimeout(resolve, 2000));
  return (
    <>
      <h1>ServerComp2 Component</h1>
    </>
  );
}
```

> - 가장 늦게 끝나는 ServerComp2 로딩 6초가 끝난 후에야 렌더링된다. (컴포넌트는 병렬적으로 렌더링되므로 6+2초는 아니다.)
> - 하지만..서버 컴포넌트에서 지연이 발생하는 동안 화면이 멈춘다??
> - 그래서 나온 개념이 **Streaming**이다.

#### 3. Streaming 이란

- 시각적으로 페이지가 통째로 멈추지 않도록, 지연 상태가 아닌 준비된 컴포넌트는 바로 보여주는 기술
- HTTP 요청을 끊지 않고, 전체가 로딩 되지 않더라도, 일부 로딩된 것들을 청크 단위로 잘라서 전송해서 **부분 렌더링**
- 위 코드에서 `/about/loading.tsx`를 생성하면 로딩하는 동안 로딩 UI를 보여줄 수 있다.
- 하지만 여전히 "ServerComp2"가 먼저 로딩이 되는데 같이 기다린다는 문제가 있다.
- 이 때 활용할 수 있는 게 **Suspense**

#### 4. Suspense 란

```ts
import ServerComp1 from "@/components/ServerComp1";
import ServerComp2 from "@/components/ServerComp2";
import { Suspense } from "react";

export default function AboutPage() {
  return (
    <>
      <h1>AboutPage Component</h1>
      <Suspense
        fallback={<h1 className="text-rose-500">ServerComp1 Loading...</h1>}
      >
        <ServerComp1 />
      </Suspense>
      <Suspense
        fallback={<h1 className="text-blue-500">ServerComp1 Loading...</h1>}
      >
        <ServerComp2 />
      </Suspense>
    </>
  );
}
```

> - 위와 같이 컴포넌트 별로 로딩 UI 처리가 가능하다.
> - 서버 컴포넌트에서만 사용할 수 있다.
> - 위와 같이 Suspense로 묶으면 하위 컴포넌트들 각각의 Promise를 병렬적으로(개별적으로) 로딩 처리(로딩 UI 렌더링) 할 수 있다.
> - 성공한 상태만 다루고, 로딩 상태와 에러 상태는 외부에 위임할 수 있다. (로딩 상태는 Suspense, 에러 상태는 ErrorBoundary로 관리할 수 있다는 의미)
> - 시각적으로 덜컥 거리지 않도록 실제 렌더링될 컴포넌트와 비슷한 틀의 로딩 컴포넌트를 만들고 싶어진다면 그게 바로 **스켈레톤 UI**

#### 5. 비고

- 컴포넌트 분리 + 각 컴포넌트 서스펜스로 묶기 = 각각의 로딩 처리 + 병렬 통신?! 쩔었다😳
- '비동기를 동기적으로 바꿔준다', '비동기를 처리하면서 간단하고 읽기편한 컴포넌트 구현을 위한 도구다' 라는 설명은 좀 더 생각해봐야 이해가 될 것 같다.
- loading.tsx 가 같이 존재한다면? 뭐가 우선일까?
- 스켈레톤 UI 라이브러리 좋은 거 찾아봐야지

## 클라이언트 컴포넌트 내부에 서버 컴포넌트를 둘 수 없을까?

1. [공식문서](https://nextjs.org/docs/app/building-your-application/rendering/composition-patterns#unsupported-pattern-importing-server-components-into-client-components) 보면 클라이언트 컴포넌트를 서버 컴포넌트에 import 할 수는 없다.

2. 하지만 [이 공식문서](https://nextjs.org/docs/app/building-your-application/rendering/composition-patterns#supported-pattern-passing-server-components-to-client-components-as-props)를 보면 서버 컴포넌트를 클라이언트 컴포넌트에 child prop 으로 넘겨주는 방법은 가능하다는 것을 알 수 있다.

3. 실제 이전 MyPetLog 프로젝트에서 페이지 별 레이아웃 조건부 렌더링을 위해 Next.js의 `template`을 2번 방법으로 사용했었는데, 그 땐 제대로 이해하고 사용한 것이 아니라 "template" 때문에 하위 컴포넌트가 다 클라이언트 컴포넌트화 되었을까봐 검토 후 리팩토링을 할 예정이었다. 하지만 그럴 필요가 없다는 걸 알게 되었다!

---

본 후기는 [유데미x스나이퍼팩토리] 프로젝트 캠프 : Next.js 1기 과정(B-log) 리뷰로 작성 되었습니다.

#유데미 #udemy #웅진씽크빅 #스나이퍼팩토리 #인사이드아웃 #미래내일일경험 #프로젝트캠프 #부트캠프 #Next.js #프론트엔드개발자양성과정 #개발자교육과정
