---
title: "[회고] 수코딩x스나이퍼팩토리x웅진싱크빅x유데미 Next.js 프로젝트 캠프 : Project Week5"
date: 2024-07-19 19:00:00 +09:00
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

- **테마는 일잘알** : "일을 잘하는 실무자"는 어떻게 일을 할까? 그간의 경험을 통한 내 나름의 인사이트는 다음과 같고, 이를 훈련 중이다.
  - 작업자가 하고 싶은 것을 하고 싶은 대로 하는 것이 아니라, 요구사항을 완수할 것. 즉, 기한 내에 완성할 것.
  - 어떤 부분에 얼마큼의 공수를 투여할지의 판단을 잘할 것.
  - 이슈는 바로 공유하고, 기록하고, 언제 논의하고 누가 언제 어떻게 처리할 것인지 트래킹할 것
- **무시무시한 캐싱 & 상태 이슈 해결** : 패러렐 라우트로 구현한 탭 메뉴에서 요상한 리렌더링이 발견되었고, 지난주 부터 시도 중인 "RCC 지양 RSC 지향" 패턴도 벽에 부딪혔다. 미니 프로젝트를 통해 외부 요소를 제거한 환경에서 문제를 제대로 파악하고자 했고, 그 덕분에 문제 파악 및 해결이 가능했다. 어떻게든 추구미를 유지하며 구현해 낸 것은 진짜 뿌듯하지만, 방향성을 궤도 수정해야 하는 게 아닌지 심히 의구심이 들고 있다. 그래도 성장을 체감하기에 굿
- **카카오톡 공유하기, 카카오 소셜 로그인** : 이런 건 이제 제법 쉽게 하는 것 같다. 물론 익숙한 환경이라 더 그랬겠지만

## 🎓 Learned

### 이 에러는 무시해도 된다고 한다.

```
app-index.js:33 Warning: Extra attributes from the server: class
    at body
    at html
    at RootLayout (Server)
    at RedirectErrorBoundary
```

- [참고자료1](https://www.inflearn.com/questions/1197812/처음에-패키지-생성만-해도-콘솔에-뜨는-오류가-있던데-이게-뭔가요-extra-attributes-from-the-server)
- [참고자료2](https://velog.io/@brgndy/Next.js-13-다크-모드-구현시-Warning-Extra-attributes-from-the-server-data-themestyle-at-body-at-html-at-RedirectErrorBoundary-에러)

## 💭 Longed For

슬슬 미완성 태스크 리스트를 작성할 수 있는 단계에는 돌입했지만, 중요한 시기에 팀원 이탈이 발생하면서 팀장님 조차 "저도 하차하고 싶어요" 발언하는 지경에 이르렀던(..) 대단히 사기 저하될 뻔한 한 주였다.<br/>
일단 나는 포기할 생각이 저은혀 없었기에, '저희 생각보다 잘했는데요? 많이 했는데요? 거의 다 하겠는데요?' 와 같은 말을 뻐끔뻐끔 거리며 전력 질주했다. 다행히도 추가 이탈은 없었고 나의 자칭 우승 후보 마인드 셋도 아직 유지 중이다(..)<br/>
중간발표도 잘해서 최종 발표팀 가자~!

---

본 후기는 [유데미x스나이퍼팩토리] 프로젝트 캠프 : Next.js 1기 과정(B-log) 리뷰로 작성 되었습니다.

#유데미 #udemy #웅진씽크빅 #스나이퍼팩토리 #인사이드아웃 #미래내일일경험 #프로젝트캠프 #부트캠프 #Next.js #프론트엔드개발자양성과정 #개발자교육과정
