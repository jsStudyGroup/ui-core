# ui-core

_html, css 를 이용한 ui core 프로젝트 입니다._

[CSS-in-JS에서 CSS-in-CSS로 바꿔야 하는 이유](https://blueshw.github.io/2020/09/14/why-css-in-css/)

위 블로그 글을 많이 참조 하였습니다.

## **🤔** font-end UI 만들는 방식은 크게 두가지다. (css)

- components file 별로 UI를 나누고 추상화 하는 CSS-in-JS 방식
- style(css) file 별로 UI를 추상화 하는 CSS-in-CSS 방식

---

## **🤔** CSS-in-JS (JSS)

_css만 사용 하는 것보다 훨씬 더 강력한 추상화 방식 ( js + css )_

### 😩 css만 사용하는 경우 발생되는 문제

- **Global namespace**: 모든 스타일이 **global에 선언되어 별도의 class 명명** 규칙을 적용해야 하는 문제
- **Dependencies**: **css간의 의존관계를 관리하기 힘든 문제**
- **Dead Code Elimination:** 기능 추가, 변경, 삭제 과정에서 **불필요한 CSS를 제거하기 어려운 문제**
- **Minification: 클래스 이름의 최소화 문제**
- **Sharing Constants:** **JS의 상태 값을 공유할 수 없는 문제**
- **Non-deterministic Resolution:** **CSS 로드 순서에 따라 스타일 우선 순위가 달라지는 문제**
- **Isolation: CSS와 JS가 분리된 탓에 상속에 따른 격리가 어려운 문제**

### 😎 CSS-in-JS를 사용하는 경우 대부분의 문제는 해결

- **Global namespace:** class명이 build time에 **유니크한 해시값으로 변경**되기 때문에 별도의 명명 규칙이 필요하지 않다.
- **Dependencies:** CSS가 **컴포넌트 단위**로 추상화되기 때문에 CSS 파일(모듈)간에 의존성을 신경 쓰지 않아도 된다.
- **Dead Code Elimination: 컴포넌트와 CSS가 동일한 구조**로 관리되므로 수정 및 삭제가 용이하다.
- **Minification:** 네임스페이스 충돌을 막기위해 **BEM 같은 방법론을 사용하면 class 명이 길어질 수 있지만, CSS-in-JS는 짧은 길이의 유니크한 클래스를 자동으로 생성**한다.
- **Sharing Constants:** CSS 코드를 JS에 작성하므로 **컴포넌트의 상태에 따른 동적 코딩이 가능**하다.
- **Non-deterministic Resolution:** CSS가 컴포넌트 스코프에서만 적용되기 때문에 **우선순위에 따른 문제**가 발생하지 않는다.
- **Isolation: CSS가 JS와 결합해 있기 때문에 상속에 관한 문제를 신경 쓰지 않아도 된다.**

### 😎 module

- [styled-components](https://styled-components.com/docs) 를 많이 사용한다.

  ```jsx
  import React from "react";
  import styled from "styled-components";

  const Circle = styled.div`
    width: 5rem;
    height: 5rem;
    background: black;
    border-radius: 50%;
  `;

  function App() {
    return <Circle />;
  }

  export default App;
  ```

### 😩 CSS-in-JS 사용하는 경우 발생되는 문제

- 번들 크기가 커진다.
  - css에 비해 큰 크기, 결국 전체적으로 느려지는 문제가 있다.
  - **js가 모두 로딩된 후에 css 코드가 생성되기 때문에 느릴 수 밖에 없다.**
  - 또한 편의성을 위해 설치되는 부수 모듈이 너무 많아지는데, 모기 잡는데 대포 쏘는 격
- 인터랙션이 늦다.

---

## **🤔** CSS-in-CSS + CSS-in-JS ( webpack + css + sass 및 전처리기를 활용 )

- **Global namespace:** webpack을 활용하면 js 에서 css,sass file을 불러올 수 있고, hash 값도 생성 가능하다. ( css-module, :global, :local )
- **Dependencies**: css file을 세분화 하고 [oocss 방식을 활용.](https://mytory.net/archives/8986)
- **Dead Code Elimination:** 컴포넌트에 들어가는 css를 제외 하고는 core css file 포함하게 되기 때문에 신경 쓰지 않는다. ( core는 늘 존재해야 하는 소스를 의미한다. )
- **Minification:** [oocss 방식을 활용.](https://mytory.net/archives/8986)
- **Sharing Constants:** CSS 코드를 JS에 작성하므로 **컴포넌트의 상태에 따른 동적 코딩이 가능**하다.
- **Non-deterministic Resolution:** CSS가 컴포넌트 스코프에서만 적용되기 때문에 **우선순위에 따른 문제**가 발생하지 않는다.
- **Isolation: CSS가 JS와 결합해 있기 때문에 상속에 관한 문제를 신경 쓰지 않아도 된다.**

😎 **결론 : 좋은 점은 취하고 단점은 최대한 줄여본다!**

---

## **🤔** ui-core

- html + scss 으로 구성.
- webpack 사용.
- codemirror 를 사용하여 live previewer 를 만든다.
- npm 으로 만들고 각 플젝에서 ui를 확인 가능해야 함.
- ui-core의 core scss 가 **각 플젝 scss 와 합쳐져서 dev/build 되야 함.**
- **ui-core가 default 이고, 각 플젝에서 storyBook 을 사용한다.**

---

## **🤔** legacy browser(IE)

> 한때 인터넷 세상을 주름잡았던 웹브라우저 ‘인터넷 익스플로러(IE)’가
> **_2022년 6월로 27년간의 생을 마감한다._**

마이크로소프트(MS)는 19일(현지 시간) 공식 블로그에서 “IE 11 데스크톱 애플리케이션은 **_2022년 6월 15일부로 지원을 종료_**하게 된다”고 밝혔다.

이날 이후 **_PC에 설치된 IE는 비활성화되고, 실행하면 자동으로 MS의 다른 웹브라우저 ‘에지’로 전환된다._**

단, IE 기반으로 만든 웹사이트를 지원하는 **_에지의 ‘IE 모드’는 최소 2029년까지는 쓸 수 있게 할 방침이다._**

1995년 처음 나온 IE는 인터넷 초창기엔 웹 브라우저의 대명사로 여겨졌지만, 지금은 파이어폭스·크롬 등 경쟁 브라우저의 득세와 스마트폰 시대 도래에 밀려 점차 구시대의 유물이 돼가는 신세다.

MS도 작년 11월에는 협업 도구인 ‘팀즈’ 지원을 중단하고 올해 8월부터는 구독형 오피스 ‘마이크로소프트365(M365)’의 일부 기능을 쓸 수 없게 하는 등 차츰 IE 종료를 준비해왔다.

---

### 😎 주요 사항

- 레거시 브라우저 지원은 하지 않는다.
  - but IE 지원이 필요한 경우 이후 IE 버전을 만들어 추가 한다.
- 반응형은 코더가 원하면 on/off 가 가능해야 한다.
- dark 모드 및 theme 지원 되야함.
- SCSS로 작성.
- html + SCSS 버전 에서는 소스 뷰어가 필요.
- storyBook 버전에서의 UI는 컴포넌트 기반이기 때문에 어느정도 **ui-core 버전이 업데이트가 된 후에 작성.**
- storyBook은 react로 초안 작성 html 버전의 stroyBook 을 설치해 봤으나 JS 가 많아서 react로 해도 별반 차이가 없을 듯 하다.
