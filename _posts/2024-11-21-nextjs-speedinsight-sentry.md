---
title: "[Next.js, NextUI] Next.js 최신 버전에서 NextUI 설치 에러"
date: 2024-11-10 13:00:00 +09:00
categories: [Tech Notes, Frontend]
image: "/assets/img/post/2024-11-07-nextjs-nextui-error.gif"
tags: [Frontend, CSS, Next.js, NextUI]
---

## 문제 상황

[공식문서](https://nextui.org/docs/guide/installation#installation-1) 대로 입력한 설치 명령어는 다음과 같다.

```
npm install @nextui-org/react framer-motion
```

이 때 에러 문구는 다음과 같다.

```
npm ERR! code ERESOLVE
npm ERR! ERESOLVE unable to resolve dependency tree
npm ERR!
npm ERR! While resolving: tailwind@0.1.0
npm ERR! Found: react@19.0.0-rc-66855b96-20241106
npm ERR! node_modules/react
npm ERR!   react@"19.0.0-rc-66855b96-20241106" from the root project
npm ERR!   peer react@"19.0.0-rc-66855b96-20241106" from react-dom@19.0.0-rc-66855b96-20241106
npm ERR!   node_modules/react-dom
npm ERR!     react-dom@"19.0.0-rc-66855b96-20241106" from the root project
npm ERR!
npm ERR! Could not resolve dependency:
npm ERR! peer react@">=18" from @nextui-org/react@2.4.8
npm ERR! node_modules/@nextui-org/react
npm ERR!   @nextui-org/react@"*" from the root project
npm ERR!
npm ERR! Fix the upstream dependency conflict, or retry
npm ERR! this command with --force or --legacy-peer-deps
npm ERR! to accept an incorrect (and potentially broken) dependency resolution.
npm ERR!
npm ERR!
npm ERR! For a full report see:
npm ERR! /Users/hayden/.npm/_logs/2024-11-08T06_07_27_819Z-eresolve-report.txt
```

이는 리액트 19 버전이 호환이 안되는 문제로 보인다.

> [리액트 오류 리포트](https://github.com/shadcn-ui/ui/issues/5557)

## 해결 방법

### 1) 다운그레이드

```
"react": "^18",
"react-dom": "^18"
```

> package.json

```
npm i
```

### 2) [공식 문서](https://nextui.org/docs/frameworks/nextjs)에서 next.js 설치 안내가 따로 되어있다. 이 명령어로 설치해보니 18 버전이다.
