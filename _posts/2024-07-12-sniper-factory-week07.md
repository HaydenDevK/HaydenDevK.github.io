---
title: "[회고] 수코딩x스나이퍼팩토리x웅진싱크빅x유데미 Next.js 프로젝트 캠프 : Project Week4"
date: 2024-07-12 19:00:00 +09:00
categories: [Dev Journal, "2024"]
image: "/assets/img/post/sniper-factory-project.png"
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

## 👍 Liked

- **RCC를 지양하고 RSC를 지향** : onClick을 formAction과 zustand로 대체하는 등 최대한 이러한 패턴을 고집해 보았다.
- **깃허브 소셜 로그인 이슈 해결** : 중복 가입이 되어버리는 이슈 해결 방법을 알았고, 필요한 유저 데이터를 session에 추가로 드리블하는 방법도 알았다.
- **몽고디비 적응 중** : `.populate`가 뭔지, `ref`로 어떻게 컬렉션 간 참조를 하는지, 복잡한 데이터 구조를 어떻게 스키마로 구현할지 등 어느 정도 "돌아가게끔" 손을 댈 수 있게 되었다.

## 🎓 Learned

### NextResponse? Response?

- Response를 사용해봤다. 타입스크립트가 5.2버전보다 낮다면 NextResponse.json()을 사용해야 하고 아니라면 Response.json()을 사용할 수 있다고 한다.

```ts
if (!newStudy)
  Response.json(null, { status: 500, statusText: "스터디 정보가 없습니다." });
```

## 💭 Longed For

이번 주가 중요했다. 셈을 해보면 해볼수록 가야 할 길은 멀지만 무너지지 않고 도전적으로 부딪혀서 목표 달성 해낸 것이 너무 뿌듯하다. 남은 3주 정말 시간이 없다는 생각만 들지만 (우승 후보의 마인드셋으로..) 최선을 탈탈 털어낼 것이다!

---

본 후기는 [유데미x스나이퍼팩토리] 프로젝트 캠프 : Next.js 1기 과정(B-log) 리뷰로 작성 되었습니다.

#유데미 #udemy #웅진씽크빅 #스나이퍼팩토리 #인사이드아웃 #미래내일일경험 #프로젝트캠프 #부트캠프 #Next.js #프론트엔드개발자양성과정 #개발자교육과정
