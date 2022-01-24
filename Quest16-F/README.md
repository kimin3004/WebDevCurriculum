# Quest 16-F. 컴포넌트 기반 개발

## Introduction

- 이번 퀘스트에서는 Vue.js 프레임워크를 통해 컴포넌트 기반의 웹 클라이언트 개발 방법론을 더 자세히 알아보겠습니다.

## Topics

- Vue.js framework
- vuex
- Virtual DOM

## Resources

- [Vue.js](https://vuejs.org)
  - [Lifecycle Hooks](https://v3.vuejs.org/guide/composition-api-lifecycle-hooks.html)
  - [State Management](https://v3.vuejs.org/guide/state-management.html)
  - [Virtual DOM](https://v3.vuejs.org/guide/optimizations.html#virtual-dom)

## Checklist

- Vue.js는 어떤 특징을 가지고 있는 웹 프레임워크인가요?

  - ![https://media.vlpt.us/images/rimo09/post/d73a99ba-3e22-46a4-92fa-61843a4b7fe0/image.png](https://media.vlpt.us/images/rimo09/post/d73a99ba-3e22-46a4-92fa-61843a4b7fe0/image.png)

  - UI 구현을 위한 **프로그레시브 프레임워크**이다.

    - 프레임워크는 애플리케이션 개발의 복잡성을 해소해주는 도구다. 그러나 애플리케이션과 마찬가지로 프레임워크에도 그 자체의 복잡성이 있다. 프레임워크라는 도구를 사용하려면 도구 자체의 복잡성에서 오는 비용과 애플리케이션 개발의 복잡성에서 오는 비용이 균형을 이루도록 적합한 프레임워크를 선택하는 것이 중요하다.

  - 프로그레시브 프레임워크에서 제공하는 단계적 영역

  1. 선언적 렌더링
  2. 컴포넌트 시스템
  3. 클라이언트 사이드 라우팅
  4. 대규모 상태관리
  5. 빌드 시스템
  6. 클라이언트-서버데이터 퍼시스턴스

  <br/>

  - Vue.js

    - 다른 단일형 프레임워크와 달리 Vue는 점진적으로 채택할 수 있도록 설계되었으며, 핵심 라이브러리는 뷰 레이어만 초점을 맞추어 **다른 라이브러리나 기존 프로젝트와의 통합이 매우 쉽다.**
    - 라우터, 상태 관리, 테스팅 등을 쉽게 결합할 수 있는 형태로 제공 -> 라이브러리 역할뿐만 아니라 프레임워크 역할도 할 수 있다는 의미로 Vue를 **점진적인 프레임워크**라고 표현한다.
    - vue도 react처럼 프로젝트에 부분적용이 가능하고, 점진적으로 도입한 후기글도 많이 찾아볼 수 있다.

  <br/>

  - Vue.js는 내부적으로 어떤 식으로 동작하나요?

  ![https://github.com/namjunemy/TIL/blob/master/Vue/img/01.PNG?raw=true](https://github.com/namjunemy/TIL/blob/master/Vue/img/01.PNG?raw=true)

  - MVVM 패턴의 ViewModel 레이어에 해당하는 View 단을 담당한다.

    - 모델과 뷰 사이에 뷰모델이 위치하는 구조
    - DOM이 변경됐을 때, 뷰모델의 DOM Listeners를 거쳐서 모델로 신호가 간다.
    - 모델에서 변경된 데이터를 뷰모델을 거쳐서 뷰로 보냈을 때, 화면이 reactive하게 반응이 일어난다.
    - Vue.js 는 이와 같이 reactive한 프로그래밍이 가능하게 끔 뷰단에서 모든 것을 제어하는 뷰모델 라이브러리이다.
    - 데이터 바인딩과 화면 단위를 컴포넌트 형태로 제공하며, 관련 API를 지원하는데에 궁극적인 목적이 있다.

- Vue.js에서의 컴포넌트란 무엇인가요?
  - Vue 컴포넌트의 라이프사이클은 어떤 식으로 호출되나요?
- 컴포넌트 간에 데이터를 주고받을 때 단방향 바인딩과 양방향 바인딩 방식이 어떻게 다르고, 어떤 장단점을 가지고 있나요?
- Vue.js 기반의 웹 어플리케이션을 위한 상태관리 라이브러리에는 어떤 것이 있을까요? 이러한 상태관리 툴을 사용하는 것에는 어떤 장단점이 있을까요?

참고 : https://goodteacher.tistory.com/195

## Quest

- Vue.js를 통해 메모장 시스템을 다시 한 번 만들어 보세요.
  - 어떤 컴포넌트가 필요한지 생각해 보세요.
  - 각 컴포넌트별로 해당하는 CSS와 자바스크립트를 어떤 식으로 붙여야 할까요?
  - Vue.js 시스템에 타입스크립트는 어떤 식으로 적용할 수 있을까요?
  - 컴포넌트간에 데이터를 주고받으려면 어떤 식으로 하는 것이 좋을까요?
  - `vue-cli`와 같은 Vue의 Boilterplating 기능을 이용하셔서 세팅하시면 됩니다.

## Advanced

- React와 Angular는 어떤 프레임워크이고 어떤 특징을 가지고 있을까요? Vue와는 어떤 점이 다를까요?

  - React VS Vue

  <br/>

  1. React

  - **함수형(immutable)**

    - setState, pure, mapStateToProps(커링 가능)
    - State(상태)의 불변성을 지켜야한다.
      - Vue에서는 상태를 직접 참조하여 값을 업데이트 가능하지만 React에서는 상태를 직접 변경하면 안된다. (setState를 이용하여 상태의 불변성을 지켜야 한다.)
      - **Why?** React에서는 state의 변경을 감지하여 특정 라이프 사이클 훅, componentWillReceiveProps, shouldComponentUpdate, componentWillUpdate, render, componentDidUpdate들을 통해 해당 컴포넌트를 다시 실행하는 구조이기 때문이다. => 컴포넌트 재실행, 재평가 (리렌더링은 Virtual DOM에의해 선택)

  - **단방향 바인딩**
    - 적절한 Event를 통해서만 코드 상 변수에 데이터 값이 담긴다.

  <br/>

  2. Vue

  - **반응형(watch, computed)**

  - **양방향 바인딩**
    - v-model를 이용한 양방향 바인딩
    - v-model과 data 객체와 연결 (사용자의 입력값이 곧바로 코드 상의 변수에 바인딩, 이벤트와 이벤트 함수 없이 바로 사용자가 입력한 값을 화면에 반영)

  <br/>

- Web Component는 어떤 개념인가요? 이 개념이 Vue나 React를 대체하게 될까요?

  - Internet Explorer에서 지원하던 HTC(HTML Component)와 유사한 방식으로 개발자가 자체적으로 HTML 엘리먼트를 만드는 기술이다.
  - 웹 컴포넌트는 **템플릿(Templates)**, **데코레이터(Decorators)**, **커스텀 엘리먼트(Custom Element)**, **섀도 DOM(Shadow DOM)**으로 구성되어 있다.

    > 장점

    1. 컴포넌트를 캡슐화(encapsulation)하여 쉽게 적용할 수 있다.
    2. 네이티브 엘리먼트로 동작하기 때문에 성능이 좋다.

    > 단점

    - 현재 웹 컴포넌트 명세는 굉장히 미흡하며, 완벽하게 지원하는 브라우저도 없다. Chrome 21 이상에서만 일부 기능이 동작한다.
    - 현재 기능의 명세들에 대한 논란이 많아 명세 내용은 언제든지 변경될 여지가 있다.

  <br/>

- Reactive Programming이란 무엇일까요?

  - 데이터 흐름(data flows)과 변화 전파에 중점을 둔 프로그래밍 패러다임(programming paradigm)이다.
  - 프로그래밍 언어로 정적 또는 동적인 데이터 흐름을 쉽게 표현할 수 있어야하며, 데이터 흐름을 통해 하부 실행 모델이 자동으로 변화를 전파할 수 있는 것을 의미한다.

<br/>

- 참고 : https://dev-daddy.tistory.com/25 [https://erwinousy.medium.com](https://erwinousy.medium.com/%EB%82%9C-react%EC%99%80-vue%EC%97%90%EC%84%9C-%EC%99%84%EC%A0%84%ED%9E%88-%EA%B0%99%EC%9D%80-%EC%95%B1%EC%9D%84-%EB%A7%8C%EB%93%A4%EC%97%88%EB%8B%A4-%EC%9D%B4%EA%B2%83%EC%9D%80-%EA%B7%B8-%EC%B0%A8%EC%9D%B4%EC%A0%90%EC%9D%B4%EB%8B%A4-5cffcbfe287f)
