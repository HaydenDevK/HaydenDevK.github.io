---
title: "[Next.js 14] SEO 챙기기 (feat. 라이트하우스 SEO 100점) | Search Engine Optimization"
date: 2024-02-26 12:00:00 +09:00
categories: [Tech Notes, Frontend]
image: "/assets/img/post/2024-02-26-seo-in-next-js.gif"
tags: [Next14, SEO, LightHouse]
---

## 결론부터

---

1. (에러가 없다면) title, description만 넣으면 된다. (통신 에러 있는 페이지에서는 `Page has unsuccessful HTTP status code` 사유로 감점이 되는 것을 확인된다.)
2. 실제 SEO(검색 엔진 최적화)가 목적이라면 라이트하우스 100점은 큰 의미가 없을 듯 하다.

## 라이트하우스 페이지 분석 결과

---

### CONTENT BEST PRACTICES

#### 항목들

```
Document doesn't have a <title> element
```

> index.html의 title 태그가 없다는 의미이다.

```
Document doesn't have a meta description
```

> description 메타 태그가 없다는 의미이다.

#### 작성할 코드

```jsx
export const metadata: Metadata = {
  title: "마이펫로그",
  description: "반려동물의 일상을 기록하고 공유하세요"
};
```

> layout.tsx에 작성하면 된다.

### NOT APPLICABLE

#### 무엇인가?

"적용할 수 없는 감사" _확실하지 않지만 적용되지 않은, 점수에 반영되지 않은 항목인 듯하다._

#### 예시

```
robots.txt is valid
```

> - 크롤링 관련 설정 파일이 없거나 잘못 작성되어있다는 의미이다.
> - [구글 검색 센터 공식 문서](https://developers.google.com/search/docs/crawling-indexing/robots/intro?hl=ko)

```
Document has a valid rel=canonical
```

> - 중복 페이지에 대한 대표 페이지 설정이 되어있지않다는 의미이다.
> - ex) `/page1?query=something` 이렇게 중복 페이지가 존재하는 경우 `/page` 페이지가 canonical로 설정되어야 한다.

## 100점 완성! 그러나

---

### 점수

<img src='/assets/img/post/2024-02-26-seo-in-next-js-img-1.png' width='100%' alt='seo 적용 전 이미지'>
> 전

<img src='/assets/img/post/2024-02-26-seo-in-next-js-img-2.png' width='100%' alt='seo 적용 전 이미지'>
> 후

### 100점 만들면 끝일까?

사실 라이트하우스 100점은 큰 의미가 없다고 한다. 실제 SEO를 개선하기 위해서는 앱 전반적으로 리팩토링이 필요할 것이다. _(이를테면 앱 로딩 속도를 2~3초 이하로 줄인다던지)_

## 개인적으로 챙기는 것들

---

### 1. sitemap.xml

#### 설정 방법

1. [https://www.xml-sitemaps.com](https://www.xml-sitemaps.com) 주소 입력
2. view > download
3. 다운로드 받은 `sitemap.xml` 파일을 맞는 경로(Next.js 14버전 기준 /app 하위)에 추가
4. 배포

#### 참고 자료

- [Next.js 공식 문서](https://nextjs.org/docs/app/api-reference/file-conventions/metadata/sitemap)

### 2. robots.txt

#### 작성 방법

```
User-Agent: *
Allow: /
Sitemap: http://54.180.96.220:3000/sitemap.xml
```

> - `User-Agent` : 대상 크롤러 (`*` 모든 크롤러가 대상)
> - `Allow` : 크롤링을 허용할 경로 (`/` 의 상대 경로)
> - `Disallow` : 크롤링을 제한할 경로 (`/` 의 상대 경로) _해당이 없으므로 미작성_
> - `Sitemap` : sitemap.xml 파일 경로

#### 참고 자료

- [Next.js 공식 문서](https://nextjs.org/docs/app/api-reference/file-conventions/metadata/robots)
- [https://seo.tbwakorea.com/blog/robots-txt-complete-guide/](https://seo.tbwakorea.com/blog/robots-txt-complete-guide/)

### 3. 네이버 웹 마스터 도구 체크

1. https://searchadvisor.naver.com/console/board
2. 사이트 등록 > 주소 입력 > 등록 아이콘 버튼 클릭 > HTML 태그 선택 > `index.html` 에 메타 태그 추가 > 배포 > `소유확인` 클릭
3. (robots.txt 설정 안 했으면 여기서 해도 된다) 사이트 관리 > 검증 > robot.txt > 간단 생성 > `public/robots.txt`로 추가 > 배포
4. robots.txt 정보 > `수집 요청` 클릭
5. 사이트 관리 > 요청 > 사이트맵 제출 > `웹사이트주소/sitemap.xml`
6. https://searchadvisor.naver.com/diagnose > 사이트 간단 체크

### 4. 상세한 메타 데이터

#### layout.tsx 에서 디폴트 메타 데이터 설정

```jsx
NEXT_PUBLIC_NAVER_SITE_VERIFICATION=
NEXT_PUBLIC_GOOGLE_SITE_VERIFICATION=
```

```jsx
import logoImageUrl from "@/public/icons/logo.svg";
import { OpenGraphType } from "next/dist/lib/metadata/types/opengraph-types";

export const BASE_URL = "http://54.180.96.220:3000";

export const METADATA = {
  title: "마이펫로그",
  description: "반려동물의 일상을 기록하고 공유하세요",
  googleSiteVerificationVerification: `${process.env.NEXT_PUBLIC_GOOGLE_SITE_VERIFICATION}`,
  naverSiteVerificationVerification: `${process.env.NEXT_PUBLIC_NAVER_SITE_VERIFICATION}`,
  imageUrl: logoImageUrl,
  url: BASE_URL,
  locale: "ko_KR",
  type: "website" as OpenGraphType,
  creator: "PPP",
  generator: "Next.js",
  publisher: "PPP",
  applicationName: "마이펫로그",
  keywords: ["마이펫로그", "PPP", "코드잇", "CodeIt"],
};
```

```jsx
export const metadata: Metadata = {
  title: METADATA.title,
  description: METADATA.description,
  verification: {
    google: METADATA.googleSiteVerificationVerification,
    other: {
      naverSiteVerification: METADATA.naverSiteVerificationVerification
    }
  },
  openGraph: {
    title: METADATA.title,
    description: METADATA.description,
    url: METADATA.url,
    siteName: METADATA.title,
    images: [
      {
        url: METADATA.imageUrl,
        width: 800,
        height: 600
      }
    ],
    locale: METADATA.locale,
    type: "website"
  },
  creator: METADATA.creator,
  generator: METADATA.generator,
  publisher: METADATA.publisher,
  applicationName: METADATA.publisher,
  keywords: METADATA.keywords,
  icons: {
    icon: METADATA.imageUrl
  }
};
```

> - `keywords` : 정의해도 큰 의미가 없다고 하지만 일단은 작성했다.
> - `metadataBase` : 완전한 URL을 필요로 하는 메타데이터 필드를 위한 기본 URL 접두사를 설정하는 옵션. 설정 시 상대

#### 각 페이지 별 메타 데이터 \_ 정적

```jsx
export const metadata: Metadata = {
  title: "홈 | 마이펫로그",
  description: "반려동물 프로필과 건강 수첩을 빠르게 확인하세요"
};
```

#### 각 페이지 별 메타 데이터 \_ 동적

```jsx
import { Metadata, ResolvingMetadata } from 'next'

type Props = {
  params: { id: string }
  searchParams: { [key: string]: string | string[] | undefined }
}

export async function generateMetadata(
  { params, searchParams }: Props,
  parent: ResolvingMetadata
): Promise<Metadata> {
  // read route params
  const id = params.id

  // fetch data
  const product = await fetch(`https://.../${id}`).then((res) => res.json())

  // optionally access and extend (rather than replace) parent metadata
  const previousImages = (await parent).openGraph?.images || []

  return {
    title: product.title,
    openGraph: {
      images: ['/some-specific-page-image.jpg', ...previousImages],
    },
  }
}

export default function Page({ params, searchParams }: Props) {}
```

> 예시 코드만 작성해둔다.
