---
title: "[회고] 수코딩x스나이퍼팩토리x웅진싱크빅x유데미 Next.js 프로젝트 캠프 : Pre-Course Week1"
date: 2024-05-31 19:00:00 +09:00
categories: [Dev Journal, "2024"]
image: "/assets/img/post/2024-05-31-sniper-factory-week01.png"
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

1주일 만에 자바스크립트, 타입스크립트, 리액트를 훑었는데
내가 (아직도) 정확히 몰랐던 부분이 뭔지 점검하고, 채울 수 있었다.
수코딩🫡

## Learned

### 동등/부등 연산과 암시적 형변환

```js
const num = 10;
const strNum = "10";
console.log(num == strNum);
```

- 동등(`==`), 부등(`!=`) 연산은 타입 비교를 안하는데
- 그 이유는 자바스크립트가 **암시적 형변환을 한 후에 비교**하기 때문이다.
- 그리고 이 암시적 형변환은 자바스크립트가 유연한 언어라고 말해지는 이유 중 하나이다.

### 변수 값 교환

```
1. **`a`**와 **`b`**라는 두 변수를 선언하고 각각 숫자 5와 10을 할당하세요.
2. 변수의 값을 서로 교환하여 **`a`**에는 10이, **`b`**에는 5가 저장되도록 코드를 작성하세요.
```

```
// 내 풀이
const five = 5;
const ten = 10;
let a = five;
let b = ten;
a = ten;
b = five;

// 강사님 풀이
let a = 5;
let b = 10;
let c = a; // c에 a의 원래 값을 저장해놓고
a = b; // a에 b를 대입하고
b = c; // b에 원래 a를 대입

```

> 강사님의 "자연스러운 사고 확장"이 아무튼 간지난다(..)

### `<script>` 태그 작성 방식 차이

#### 비교

##### 1. `<head>` 태그 내부에 옵션 없이 작성

```html
<head>
  <script src="./index.js"></script>
</head>
```

> - [HTML 파싱] 시작 -> `<script>` 태그를 만나면 [HTML 파싱 중단] 후 [스크립트 로드] -> [스크립트 실행] -> [HTML 파싱] 재개 및 완료
> - HTML 파싱이 가장 마지막에 끝나므로 **TTFV 느림**
> - 스크립트 실행, HTML 파싱이 둘 다 끝나야 하는데 순차 실행 되므로 **TTI 느림**

##### 2. `<body>` 태그 맨 아래에 작성

```html
<body>
  <!-- content -->
  <script src="./index.js"></script>
</body>
```

> - [HTML 파싱] 시작 및 완료 -> body 맨 아래에서 `<script>` 태그를 만나면 [스크립트 로드] -> [스크립트 실행]
> - HTML 파싱이 멈추지 않고 실행되어 먼저 끝나므로 **TTFV 빠름**
> - 스크립트 실행, HTML 파싱이 둘 다 끝나야 하는데 순차 실행 되므로 **TTI 느림**

##### 3. `<head>` 태그 내부에 async 속성과 함께 작성

```html
<head>
  <script async src="./index.js" async></script>
</head>
```

> - [HTML 파싱] 시작 -> [스크립트 로드] && [HTML 파싱] 병렬 실행<br/>
>   -> ([스크립트 로드]가 먼저 완료되면 [HTML 파싱]을 멈추고) [스크립트 실행] -> [HTML 파싱] 완료<br/>
>   -> ([HTML 파싱]이 먼저 완료되면 바로) [스크립트 실행]
> - **순서 보장 X** HTML 파싱보다 스크립트 실행이 먼저 시작될 수 있다. 이러면 안되는 경우 주의
> - 스크립트 로드와 HTML 파싱이 병렬 실행되는만큼 속도 이점이 있다.

##### 4. `<head>` 태그 내부에 defer 속성과 함께 작성

```js
<head>
  <script defer src="./index.js"></script>
</head>
```

> - [HTML 파싱] 시작 -> [스크립트 로드] && [HTML 파싱] 병렬 실행<br/>
>   -> [HTML 파싱] 선 완료 -> [스크립트 실행]<br/>
> - **순서 보장 O**<br/>
> - 가장 이상적인 방법 같지만, script가 상대적으로 무겁고 html이 상대적으로 가볍다면? <br/>
>   html 파싱 종료 -> TTFV -> 스크립트 실행 완료 -> TTI 단계 사이에 시차가 발생하는 UX 문제가 있을 수 있다.
> - 사이즈가 큰 js를 불러올 때, 스크립트 파일끼리 의존성이 있을 때 사용하면 좋다.

#### 결론

- 경우에 따라 적절한 방법을 사용하면 되겠지만, 최근에는 네트워크 속도와 브라우저 성능 발달로 왠만하면 body 맨 아래에 작성하면 될 듯 하다.
- 스크립트 사이즈가 작거나, 브라우저 및 네트워크 성능이 좋다면 어떤 방식도 문제가 되지 않을 것이다.

### Wrapper Object (래퍼 객체)

```js
const str = "string";
console.log(str.length); // length는 어떻게 존재하는가?
```

- str는 string 기본 자료형이다.
- 기본 자료형은 객체가 아니므로, 본래 속성이나 메소드를 가질 수 없다.

#### 그런데 어떻게 str.length가 존재하는가?

- 기본 자료형을 객체처럼 접근하면(메서드나 속성을 호출하면), 자바스크립트 엔진이 일시적으로 기본 자료형 데이터를 "연관된 객체"로 변환 해주기 때문이다.

#### 연관된 객체란?

- string은 new String() 생성자 함수의 결과인 인스턴트 객체이다.
- 이 때 생성되는 임시 객체를 래퍼 객체라고 한다.
- 이 코드에서도 length에 접근하는 동안만 일시적으로 래퍼 객체로 "감싸진다".<br/>

#### 감싸지면?

- 활용하는 동안, 인스턴트 객체가 가지는 String 프로토타입을 참조할 수 있다.

#### 활용이 끝나면?

- 원래의 기본 자료형으로 돌아온다.

```js
const string1 = "string";
const string2 = new String("string");
console.log(string1); // 문자열
console.log(string1); // 문자열 객체
console.log(string1 === string2); // false;
console.log(string1 === string2.toString()); // true;

const num1 = 10;
const num2 = new Number(10);
console.log(num1 instanceof Number); // false 원래 기본 자료형은 Number 생성자 함수의 인스턴스가 아니다.
console.log(num2 instanceof Number); // true Number 생성자 함수의 인스턴스가 맞다.
console.log(num1.toFixed(2)); // 문자열을 객체처럼 활용(toFixed에 접근)하는 순간 래퍼 객체로 감싸지고, 해당 객체가 가지고 있는 프로토타입 체이닝에 의해 Number.prototype.toFixed() 메소드에 접근할 수 있다.
```

### 이상한 타입오류

```ts
type TUser = {
  name: string;
  age: number;
  gender: "male" | "female";
};

const user = {
  name: "name",
  age: 10,
  gender: "female"
};

const user2: TUser = user; // 왜 에러가 날까?
```

> - 타입 추론에 의해서 gender가 string으로 추론되어서 그렇다. user는 type이 없기 때문에
> - user의 gender를 `"male" | "female" | string` 로 바꿔서 해결할 수 있다.

### 리액트 x 타입스크립트 x 나머지 매개변수 = so neat✨

```ts
const MyInput = (props: React.ComponentProps<"input">) => {
  const { ...restProps } = props;

  return <input {...restProps} />;
};

export default MyInput;
```

```ts
<MyInput type={"text"} placeholder={"Enter Todo List"} />
```

> `React.ComponentProps<"button">`가 button 태그가 원래 들어가는 속성 타입을 다 가지고 있기 때문에 onClick 타입 조차 따로 지정할 필요가 없다.
> 나머지 매개변수로 알아서 들어가기 때문에 따로 명시해주지 않아도 된다.

---

본 후기는 [유데미x스나이퍼팩토리] 프로젝트 캠프 : Next.js 1기 과정(B-log) 리뷰로 작성 되었습니다.

#유데미 #udemy #웅진씽크빅 #스나이퍼팩토리 #인사이드아웃 #미래내일일경험 #프로젝트캠프 #부트캠프 #Next.js #프론트엔드개발자양성과정 #개발자교육과정
