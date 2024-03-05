---
title: "기술 블로그 구축 | How I Built This Blog"
date: 2024-02-09 16:00:00 +09:00
categories: [Tech Notes, ETC]
image: "/assets/img/post/2024-02-09-how-i-built-blog.gif"
tags: [github blog, chirpy]
---

## 개요

---

### 기본 정보

- 2024년 2월 9일 기준
- Mac M1 (2020) Sonoma 14.3.1

### 목적

- 기존에 깃허브, 노션, 슬랙, 디스코드 등에 산란해있던 Tech Notes 취합해서 관리할 용도
- 모종의 이유로 재구축
- 기존 게시글 일괄적으로 옮겨올 예정

## Step by step

---

### 1. Ruby 설치

```
brew install ruby
brew install rbenv ruby-build
rbenv global 3.0.5
vim ~/.zshrc # 파일 열어서 아래 내용 넣고 저장

[[ -d ~/.rbenv  ]] && \
  export PATH=${HOME}/.rbenv/bin:${PATH} && \
  eval "$(rbenv init -)"

source ~/.zshrc
```

### 2. bundler, jekyll, github-pages 설치

```
gem install bundler
gem install jekyll
gem install github-pages
```

### 3. chirpy 레포지터리 Fork

```
https://github.com/cotes2020/jekyll-theme-chirpy >> Fork >> `깃허브유저네임.github.io` 레포지터리명으로 설정 >> (master only copy 선택)
```

### 4. 본인 깃허브 레포지터리 배포 설정

```
Repository >> Settings >> Pages >> Build and deployment >> `GitHub Actions`으로 설정 >> configure >> commit (`Create jekyll.yml`)
```

### 5. 본인 로컬에 클론

```
(로컬에서 원하는 디렉토리 이동) >> `git clone 유저네임.github.io.git`
```

### 6. 패키지 설치

```
npm i
```

### 7. 환경변수 수정

```
NODE_ENV=production npx rollup -c --bundleConfigAsCjs
```

### 9. `./github/workflows/pages-deploy.yml` 수정

```
branches:
  - main
  - master
```

> `- master` 제거

```
pages-deploy.yml.hook => pages-deploy.yml
```

> 파일 확장자 수정

### 10. `./_config.yml` 수정

```
lang: en
timezone: Asia/Seoul
title: # 블로그 타이틀
tagline: # 블로그 서브타이틀
description: # 블로그 설명
url: # 블로그 URL
github:
  username: # 깃허브 유저 네임
social:
  name: # 원하는 이름
  email: # 이메일 주소
  links:
    # 본인 깃허브 페이지 주소
    # 그 외 SNS 등 주소
```

### 11. `./gitignore` 수정

```
# assets/js/dist
```

> 주석처리

### 12. Commit, Push 후 자동 배포 확인

### 13. 로컬에서 서버 설치, 실행

```
bundle install
bundle exec jekyll serve
```
