---
title: 2023-Winter-Bootcamp-Team K
date: 2023-12-27 22:08:15 +09:00
categories: [2024 겨울 부트캠프 K, 공부중]
tags: [부트캠프, 프론트엔드, 백엔드]
pin: true
---

> 부트캠프 시작
> 나는 사실상 지금까지 프론트엔드나 백엔드에 관한 것을 하나도 공부해 본 것이 없었기에 이번이 처음이다. 그렇다면 당연히 처음 배우는 것들을 공부해야겠지?

# 날짜별 일지

## 2일차 2023-12-28 (Git & Github)

git과 github에 관하여 배웠다.

github.io페이지 넘길 때에는 main브랜치에 바로 push pull했고 issue나 action에 관한 사용방법을 몰랐었는데 나중에 git에 관해 한번 다 정리해봐야겠다.

## 3일차 2023-12-29 (Chat Gpt를 사용하여 개발하기)

chat Gpt를 이용한 서비스를 만들게 될 것이다보니 아무래도 좀 많이 집중해서 들어야겠다. 우리는 GPT를 이용해서 캐릭터를 만들고 그 캐릭터와 대화하면서 영어실력을 향상시킬 수 있는 서비스를 만들 것이다.

> 다른 아이디어를 생각해 보라는....

사용할 것들 : chat GPT, RVS-TTS, STT, react, typescript 등등

## 4일차 2023-12-30 (프로젝트 관리와 커뮤니케이션)

> 아직도 아이디어 개발중... 으엑

react를 공부할 때에는 [생활코딩의 react강의](https://www.youtube.com/watch?v=t9e3hMJ_s-c&list=PLuHgQVnccGMCOGstdDZvH41x0Vtvwyxu7&index=5&t=233s)를 보면서 공부하고 있다.

추가로 아래 영상들을 보며 공부하고 있다.
![생활코딩](https://github.com/oil-lamp-cat/oil-lamp-cat.github.io/assets/103806022/13daafcc-cf7e-4365-b29f-80fda6246d6d)

## 5-1일차 2023-12-31 (2023년의 마지막 날)

> 또 다른 아이디어!

택배 경로 최적화 서비스

### 기획 의도

---

전통적인 택배 배달 방식의 비효율적인 시간문제 환경 문제를 해결하기 위해 지하철을 활용한 배달 시스템 고안

### 기대 효과

---

- 지하철을 활용한 택배 어플리케이션을 제공함으로써 택배 배송의 문제점인 교통체증, 대기오염 등의 문제 해결
- 기존 물류 시스템보다 빠른 시간 내에 배송이 가능
- 택배 기사님들의 과도한 업무량 감소
- 누구나 택배기사가 될 수 있으므로 경제적인 효과?

> 그리고 파기

## 5-2일차 2023-12-31 (2023년의 마지막 날)

> 돌고돌아 음성 채팅이 되었다.

### 기획의도

---

유튜브에서 본인이 좋아하는 캐릭터의 성우와 이야기하면서 실제 캐릭터와 대화하는 느낌을 받으며 기뻐하는 컨텐츠를 보고 생각하게 되었다. 이를 보고 캐릭터의 성격이나 정보를 GPT에게 입력한 후 TTS를 기반으로 사용자가 해당 캐릭터와 대화를 하는 서비스가 있다면 좋을 것 같다는 생각에서 기획하게 되었다.

## 6, 7일차 2024-01-03 (UI/UX 설계 및 적용, 도커 & 도커 컴포즈)

[CSS 공부 시작](https://www.youtube.com/)

[HTML 공부 이어서](https://oil-lamp-cat.github.io/posts/html/)

[ERD cloud](https://shuu.tistory.com/64)

- npm 대신 Yarn을 사용해보자!
  [참고](https://velog.io/@nxnaxx/React-%EA%B0%9C%EB%B0%9C-%ED%99%98%EA%B2%BD-%EA%B5%AC%EC%B6%95-yarn%EA%B3%BC-CRA-%EC%84%A4%EC%B9%98)

api 설계(api 명세서 작성)

API 설계란?

애플리케이션(스마트폰, 카카오톡 서버 등 서로 다른 프로그램)이 인터페이싱하는(요청과 응답을 주고받는) 체계
[추가적인 내용](https://enjoyinjoanne.tistory.com/56)

[설계할 때 참고한 블로그](https://hackmd.io/@boostgroup3/S152Qkn_P)

## 8일차 2024-01-04 (DB, 프론트엔드 초기 세팅)

수업을 듣고나서 세팅을 해보려 했는데 자꾸 해깔려서 기록

- `npm install --global yarn vite`
- `yarn create vite [프로젝트 이름] --template react-ts`
- `yarn install`
- `yarn add  -D eslint @typescript-eslint/eslint-plugin @typescript-eslint/parser`
- `yarn add -D eslint-config-airbnb-base eslint-plugin-import`
- `yarn add -D prettier`
- `yarn add -D tailwindcss postcss autoprefixer`
- `yarn add react-router-dom`
- `yarn dev`로 실행

## 2024-01-09

이제는 Typescript를 이용해서 웹사이트 퍼블리싱을 하기로 함. 타입스크립트관련 내용은 추후에 추가로 쓰겠다.
![2024-01-09](https://github.com/2023-Summer-Bootcamp-Team-G/frontend/assets/103806022/482ea619-7cc2-4aad-8e87-11c5937a140b)

타입스크립트에서 props사용하는 법을 잘 알지 못해 아무래도 여기 저기 찾아다녔고 결국 찾았다!
![스크린샷(187)](https://github.com/2023-Summer-Bootcamp-Team-G/frontend/assets/103806022/fdd2c37f-ba5c-4521-afff-8f90f6206a8c)

```escripttyp
type [이름] = {
  변수명?: [변수 타입];
}
```

변수명 뒤에 ?를 붙여 기본 설정하고 변수 타입을 설정할 수 있게 만들어준다.

![스크린샷(188)](https://github.com/2023-Summer-Bootcamp-Team-G/frontend/assets/103806022/bc19230e-52fc-43bb-afec-435ee7d6193b)

```typescript
export default function [함수명]({
  변수명 = '기본 설정 값',
  변수명,
} : [이름]){
  return(
    <div
    변수명1 = {변수명1}
    변수명2 = {변수명2}
    >{변수명3}</div>
  );
}
```

변수설정 할 때에 기본 설정을 해 놓으면 컴포넌트 사용할 때에 필수가 아닌 선택을 할 수 있게 된다.

![스크린샷(189)](https://github.com/2023-Summer-Bootcamp-Team-G/frontend/assets/103806022/638d898b-71cd-4f19-9dcd-1c233c8e2b40)

이 부분에서 중요한 곳은

```typescript
[css 변수]: ${(props) => (props.[css 변수] === '원하는 기본 명령어' ? '기본 명령어일 때 값' : props.[css 변수])}
```

이거를 이해하기 위해 거진 6시간동안 찾았고 역시 나는 이런거에 희열을 느끼는 사람인가보다. 다음 문제는 뭘까?

# 공부할 때에 도움이 된 것들

css

- [FlexboxFroggy](https://flexboxfroggy.com/#ko)
- [WEB2-CSS](https://youtube.com/playlist?list=PLuHgQVnccGMAnWgUYiAW2cTzSBywFO75B&si=LZxR-ue8zYp82oOh)

javascript

- [WEB2-JavaScript](https://youtube.com/playlist?list=PLuHgQVnccGMBB348PWRN0fREzYcYgFybf&si=5-6IckN6-OTd5cwx)

react

- [2022 코딩애플 리액트 강의](https://www.youtube.com/playlist?list=PLfLgtT94nNq0qTRunX9OEmUzQv4lI4pnP)

git

- [Learning git branch](https://learngitbranching.js.org/?locale=ko)
