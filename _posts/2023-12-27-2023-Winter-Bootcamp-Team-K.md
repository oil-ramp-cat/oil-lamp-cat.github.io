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

## 2024-01-09

> 오늘은 (로그인 안했을 시) 메인 페이지, 로그인 페이지, 회원가입 페이지를 만들었다.

![main page](https://github.com/2023-Winter-Bootcamp-Team-K/Front/assets/103806022/35049a1a-007f-4c65-9dd7-859864501727)
-> 메인 페이지

![login page](https://github.com/2023-Winter-Bootcamp-Team-K/Front/assets/103806022/94cbd5a0-9b6a-4b18-9c02-2e2aea80c0b2)
-> 로그인 페이지

![register page](https://github.com/2023-Winter-Bootcamp-Team-K/Front/assets/103806022/2be7ec1f-0ed9-4e42-aef2-e6e099dd6c0a)
-> 회원가입(아이디 비밀번호) 페이지

![register page2](https://github.com/2023-Winter-Bootcamp-Team-K/Front/assets/103806022/5cedd0e4-4fe6-4497-9b89-c3c211edb3fd)
-> 회원가입(개인정보) 페이지

styled component를 사용하게 되면 딱 css나 html문법만 사용하게 될 줄 알았는데 생각보다 하면서 뭔가 애니메이션도 넣고 싶은 생각이 들어 typescript를 좀 더 파봐야겠다. 일단 오늘 만든 것은 위 3페이지로 추가적으로 나중에 시간이 난다면 구름이나 꽃 등의 요소가 떠다니는 애니메이션을 넣고 싶다. 코드를 짜면서 오류가 나던 것들은 계속 캡쳐를 하고 있으니 나중에 모아서 오류들에 관한 이야기를 작성해 봐야겠다.

컴포넌트들(btn, id/pwinput)을 우리 입맛대로 조율할 수 있게 하려고 props를 추가하여 넣어주었다.

[github](https://github.com/2023-Winter-Bootcamp-Team-K/Front/pull/27)

> git 자주 쓰는 명령어

```git
git fetch
git switch 브랜치 이름(design/#1)

git add .
git commit -m '폴더'
git push origin main

git pull
```

### 오후

> 스크롤 바꾸미는 방법을 찾아 채팅창을 구현하는 중이다.

![scrollB](https://github.com/oil-lamp-cat/oil-lamp-cat.github.io/assets/103806022/07bd4b23-390a-4f8b-99a2-6fc3907921f0)

-> 꾸미기 전

음... 브라우저마다 규칙이 달라서 이건 나중에 혼자 해보는 걸로....

## 2024-01-11

> 어제 하던 로그인 페이지, 회원가입 페이지를 완성했다. 추가로 카메라(react-webcam) 반응형을 구현하였다.

### 오늘의 문제

분명 같은 컴포넌트인데 자꾸 input창이 밖으로 나온다.

![비밀번호_box_layout](https://github.com/oil-lamp-cat/oil-lamp-cat.github.io/assets/103806022/e1c291b4-785f-49a3-b0f3-c5d9b1176683)
![비밀번호_input_layout](https://github.com/oil-lamp-cat/oil-lamp-cat.github.io/assets/103806022/37017faa-3375-41b4-88d3-4178554fc3f8)
![비밀번호_layout](https://github.com/oil-lamp-cat/oil-lamp-cat.github.io/assets/103806022/4f3833f6-e38a-49b2-805a-8f37cb7ced07)

-> 비밀번호 layout

![아이디_box_layout](https://github.com/oil-lamp-cat/oil-lamp-cat.github.io/assets/103806022/adfc106f-675c-452b-9c75-988393927fc1)
![아이디_input_layout](https://github.com/oil-lamp-cat/oil-lamp-cat.github.io/assets/103806022/949588e2-fccc-4f0c-9542-4266c0428ee2)
![아이디_layout](https://github.com/oil-lamp-cat/oil-lamp-cat.github.io/assets/103806022/5e7f6e29-78bf-4c9f-8ef0-07ff3abc2c46)

-> 아이디 layout

### 해결

일단 결론만 말하자면 width 설정을 해줄 때에 오류가 생긴 코드에서는 outbox layer와 input layer의 값이 동일하기에 margin을 주었을 때에 옆으로 삐져나가는 오류가 생긴 것이다. 해결하기위해 width 를 props로 주지 않고 100%로 설정하여 해결하였다.

```typescript
    @media all and (min-width: 391px) {
      width: ${(props) => (props.width === 'normal' ? '29.55rem' : props.width)};;
      height: 1.84725rem;
      font-size: 1.25rem;
      margin-left: 0.94rem;
    }
    @media all and (min-width: 390px) and (max-width: 790px) {
      width: ${(props) => (props.width === 'normal' ? '29.55rem' : props.width)};;
      height: 1.1425rem;
      font-size: 1rem;
      margin-left: 0.51rem;
    }
```

```typescript
    @media all and (min-width: 391px) {
      width: 100%;
      height: 1.84725rem;
      font-size: 1.25rem;
      margin-left: 0.94rem;
    }
    @media all and (min-width: 390px) and (max-width: 790px) {
      width: 100%;
      height: 1.1425rem;
      font-size: 1rem;
      margin-left: 0.51rem;
    }
```

### 지금까지의 결과물

![2024-01-11-18-07-26-Trim](https://github.com/oil-lamp-cat/oil-lamp-cat.github.io/assets/103806022/720e2dcb-99ef-40a9-8468-db6ffff6de3f)

[[design] 회원가입 페이지 반응형 설정 #44](https://github.com/2023-Winter-Bootcamp-Team-K/Front/issues/44)

[[design] 로그인 페이지 반응형 설정 #38](https://github.com/2023-Winter-Bootcamp-Team-K/Front/issues/38)

[[design] 카메라 반응형 구현 #50](https://github.com/2023-Winter-Bootcamp-Team-K/Front/issues/50)

## 2023-01-12

오늘은 어제에 이어서 일기장 페이지 반응형을 설정하였고, webcam 모달을 퍼블리싱 하였다.

### 오늘의 실수

부모 요소를 자식요소한테 전해주려면 (width, height) display: flex;를 이용해야 한다. 이걸 해깔려서 분명 반응형 설정을 했는데 불구하고 카메라 캡쳐가 혼자 본래의 크기를 유지하고 있는 일이 생겼다. 그리고 \<webcam>에 width를 줄 때에 부모 크기에 맞출 수 있게 rem이나 px값이 아닌 100%로 줘야한다는 요령이 생겼다. 이러한 관계가 익숙해지면 훨씬 더 간단하고 효율적으로 코드를 짤 수 있지 않을까 생각해본다.

### 결과물

음.. 이미지가 너무 커서 github 호스팅이 안된다.

```
![df](../assets/img/post/winter_bootcamp/2024-01-12/2024-01-12%2022-09-48.gif)
```

생각보다 자잘한 문제들이 많아서 아무래도 조금 시간이 걸렸다. 확실히 만들기 전에 어떻게 코들를 짤지 박스로 그림을 그려놓고 만드는 것이 좋은 것 같다.

그리고 확실히 나는 적어도 6시간은 자야겠다. 아무래도 효율이 좋지 않다.

- [[design] CameraModal.tsx 퍼블리싱 ](https://github.com/2023-Winter-Bootcamp-Team-K/Front/issues/61)
- [[design] result 페이지, chattingResult 컴포넌트(반응형x) 생성 ](https://github.com/2023-Winter-Bootcamp-Team-K/Front/issues/59)
- [[design] 일기장 페이지 반응형 설정](https://github.com/2023-Winter-Bootcamp-Team-K/Front/issues/55)

이제 백엔드에서 만든 api를 연동해야 하는데.. 백엔드에서 작업을 끝낼 때 까지 어떻게 연동해야하는지 그리고 Zustand, useEffect 함수에 관해 공부하고 있어야겠다.

### 2024-01-13 02/37

추가적으로 변경사항이 생겼다.

[[design/#69] chatInfo 컴포넌트 생성 및 chatpage 레이아웃 조정 ](https://github.com/2023-Winter-Bootcamp-Team-K/Front/pull/70)

![스크린샷(210)](https://github.com/oil-lamp-cat/oil-lamp-cat.github.io/assets/103806022/da3e4c21-f42b-4920-b0a5-2e1f369106a1)

## 2024-01-13

시작부터 오류를 발견했다..

> chunk-GZ55BCQ2.js?v=bed9aa86:521 Warning: React does not recognize the `marginBottomPTT` prop on a DOM element. If you intentionally want it to appear in the DOM as a custom attribute, spell it as lowercase `marginbottomptt` instead. If you accidentally passed it from a parent component, remove it from the DOM element.

콘솔창이 말하길 pops 명에 대문자가 포함되어 있어 문제가 생기고 있다 한다.

아.. 미리 체크 해볼걸.. 또 새로운 것을 알아간다..

![스크린샷(216)](https://github.com/oil-lamp-cat/oil-lamp-cat.github.io/assets/103806022/088ad40d-0792-4fa7-84b8-9b284a9f2ce9)

세상에 이걸 다 바꿔야한다니... 연결되어있는 page가 좀 많이 많은데..

![스크린샷(215)](https://github.com/oil-lamp-cat/oil-lamp-cat.github.io/assets/103806022/6d8c25f0-961b-4798-913a-56362c1141e7)

### 저녁

![스크린샷(219)](https://github.com/oil-lamp-cat/oil-lamp-cat.github.io/assets/103806022/9fa8a7fa-d98b-4282-b938-9b68e897af7a)

해결을 이해 marginbottomptt에 값을 넣어주었다.

## 2024-01-14

### 오전

정규식표현을 적용하여

- 비밀번호 : 6자리 이상 영어, 숫자 포함
- 이름 : only 한글
- 생년월일 : YY-MM-DD 형식으로 YY 4, MM 2, DD 2자씩 입력해야 통과할 수 있게 만들었다.

```typescript
/// 비밀번호
/^(?=.*[a-zA-Z])(?=.*[0-9]).{6,25}$/

/// 생일
/[0-9]{4}-[0-9]{2}-[0-9]{2}/i

/// 이름
/^[ㄱ-ㅎ|가-힣]+$/

```

![2024-01-14 14-46-13 - Trim](https://github.com/oil-lamp-cat/oil-lamp-cat.github.io/assets/103806022/9d4abfc9-fe31-4d33-9488-c1df7616be58)

## 2024-01-16

일기장 생성 api연동, diarypage에서 메인으로 가는 버튼 추가, result 페이지 퍼블리싱을 하였다. 사실 이번에는 백엔드와 axios로 api연동하는 작업을 많이 했다.

![2024-01-16 00-10-05 - Trim](https://github.com/oil-lamp-cat/oil-lamp-cat.github.io/assets/103806022/62928d2f-5f74-4712-8946-287c1f7fe0e9)

[[feat/#77] 일기장 생성 및 diarypage 메인으로 가기 버튼 추가](https://github.com/2023-Winter-Bootcamp-Team-K/Front/pull/82)

[[design/#83] result 페이지 퍼블리(채팅기록 페이지 퍼블리싱)](https://github.com/2023-Winter-Bootcamp-Team-K/Front/pull/84)

## 2024-01-17

### 새벽

일단 어제, 오늘은 swagger연동과 웹소켓 연결을 하였고 추가적으로 웹소켓 시작 세팅(첫 대화 시작 연동을 하였다)

웹소켓 연결을 할 때에 다른 라이브러리를 사용하지 않고 그냥 WebSocket을 사용하였다.

[[design] 채팅기록 페이지 퍼블리싱](https://github.com/2023-Winter-Bootcamp-Team-K/Front/issues/83)

[[feat] 채팅 페이지 웹소켓 연결](https://github.com/2023-Winter-Bootcamp-Team-K/Front/issues/85)

[[feat] 일기장 생성 ](https://github.com/2023-Winter-Bootcamp-Team-K/Front/issues/77)

[[feat] 웹소켓 기분설정, 첫 대화 시작](https://github.com/2023-Winter-Bootcamp-Team-K/Front/issues/90)

### 저녁

웹소켓 연결, 연결 해제, 웹소켓 연결하면 연결 되었다고 백엔드에 요청 보낸 후 요청을 받아서 character별로 분류하여 리스트에 넣고 tts는 blob형태를 다시 base64 디코딩하여 소리로 출력할 수 있게 만들었다.

```javascript
const audioBlob = messageReceived.data.audioBlob;
let snd = new Audio(`data:audio/x-wav;base64, ${audioBlob}`);
snd.play();
```

- javascript에서 blob형태를 base64 디코딩하여 소리로 변환하는 코드

[[Feat/#90] 웹소켓 연결 및 연결 해제](https://github.com/2023-Winter-Bootcamp-Team-K/Front/pull/97)

## 2024-01-18

아래와 같은 오류가 발생하는 이유는 컴포넌트에서 페이지 컴포넌트로 옮겨 오는 과정에서 styled-components 콘솔 오류가 발생하는 듯 하다.

```
styled-components: it looks like an unknown prop "prop이름" is being sent through to the DOM, which will likely trigger a React console error. If you would like automatic filtering of unknown props, you can opt-into that behavior via <StyleSheetManager shouldForwardProp={...}> (connect an API like @emotion/is-prop-valid) or consider using transient props ($ prefix for automatic filtering.)
```

이 오류를 해결하기 위해 자식 컴포넌트에 $를 붙여 해결하였다.

![props 오류](https://github.com/oil-lamp-cat/oil-lamp-cat.github.io/assets/103806022/d8872ae6-e9a0-4a64-9fa7-af402f744648)

![props 오류 2](https://github.com/oil-lamp-cat/oil-lamp-cat.github.io/assets/103806022/f9ab4c55-57ab-4c35-b74c-e84fb7e10d5e)

![props 오류 3](https://github.com/oil-lamp-cat/oil-lamp-cat.github.io/assets/103806022/e3aee7e7-bbe1-45cd-ad2c-952e79d6a131)

# 공부할 때에 도움이 된 것들

> 이기는 한데 코드를 짤 때에 이미 너무 많은 것들을 찾아봐서 저장하기 어려울지도?

css

- [스크롤 꾸미기!](https://inpa.tistory.com/entry/CSS-%F0%9F%8C%9F-%EC%8A%A4%ED%81%AC%EB%A1%A4-%EB%B0%94Scrollbar-%EA%BE%B8%EB%AF%B8%EA%B8%B0-%EC%86%8D%EC%84%B1-%EC%B4%9D%EC%A0%95%EB%A6%AC)
- [FlexboxFroggy](https://flexboxfroggy.com/#ko)
- [WEB2-CSS](https://youtube.com/playlist?list=PLuHgQVnccGMAnWgUYiAW2cTzSBywFO75B&si=LZxR-ue8zYp82oOh)

javascript

- [WEB2-JavaScript](https://youtube.com/playlist?list=PLuHgQVnccGMBB348PWRN0fREzYcYgFybf&si=5-6IckN6-OTd5cwx)

react

- [2022 코딩애플 리액트 강의](https://www.youtube.com/playlist?list=PLfLgtT94nNq0qTRunX9OEmUzQv4lI4pnP)
- [react-webcam + TypeScript](https://dev.to/sababg/react-webcam-typescript-gh2)

git

- [Learning git branch](https://learngitbranching.js.org/?locale=ko)

색상 추천

- [color space](https://mycolor.space/?hex=%23FEC479&sub=1)
