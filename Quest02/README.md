# Quest 02. CSS의 기초와 응용

## Introduction

- CSS는 Cascading StyleSheet의 약자입니다. 웹브라우저에 표시되는 HTML 문서의 스타일을 지정하는 (거의) 유일하지만 다루기 쉽지 않은 언어입니다. 이번 퀘스트를 통해 CSS의 기초적인 레이아웃 작성법을 알아볼 예정입니다.

## Topics

- CSS의 기초 문법과 적용 방법
  - Inline, `<style>`, `<link rel="stylesheet" href="...">`
- CSS 규칙의 우선순위
- 박스 모델과 레이아웃 요소
  - 박스 모델: `width`, `height`, `margin`, `padding`, `border`, `box-sizing`
  - `position`, `left`, `top`, `display`
  - CSS Flexbox와 Grid
- CSS 표준의 역사
- 브라우저별 Developer tools

## Resources

- [MDN - CSS](https://developer.mozilla.org/ko/docs/Web/CSS)
- [Centering in CSS: A Complete Guide](https://css-tricks.com/centering-css-complete-guide/)
- [A complete guide to Flexbox](https://css-tricks.com/snippets/css/a-guide-to-flexbox/)
- [그리드 레이아웃과 다른 레이아웃 방법과의 관계](https://developer.mozilla.org/ko/docs/Web/CSS/CSS_Grid_Layout/%EA%B7%B8%EB%A6%AC%EB%93%9C_%EB%A0%88%EC%9D%B4%EC%95%84%EC%9B%83%EA%B3%BC_%EB%8B%A4%EB%A5%B8_%EB%A0%88%EC%9D%B4%EC%95%84%EC%9B%83_%EB%B0%A9%EB%B2%95%EA%B3%BC%EC%9D%98_%EA%B4%80%EA%B3%84)

## Checklist

- CSS를 HTML에 적용하는 세 가지 방법은 무엇일까요?

  - 세 가지 방법 각각의 장단점은 무엇일까요?

  1. **인라인 스타일(Inline style)**

     ```jsx
     <body>
       <h2 style="color:green; text-decoration:underline">인라인 스타일</h2>
     </body>
     ```

     - HTML 태그의 style 속성에 CSS 코드를 넣는 방법
     - 해당 태그가 선택자(selector)가 되고, CSS 코드에는 속성(property)와 값만 들어 가는 형태

     > **단점**
     >
     > - 한 번 설정된 스타일을 변경하기가 어렵다.
     > - 재사용이 불가능 하다.

     <br/>

  2. **내부 스타일 시트(Internal style sheet)**

     ```html
     <head>
       <style>
         body {
           background-color: lightyellow;
         }
         h2 {
           color: red;
           text-decoration: underline;
         }
       </style>
     </head>
     ```

     - HTML 문서 안의 <style>과 </style> 안에 CSS 코드를 넣는 방법
     - `<style>`과 `</style>` 태그 안에 CSS 코드를 삽입
     - `<style>` 태그는 보통 `<head>`와 `</head>` 사이에 넣으나, HTML 문서의 어디에 넣어도 잘 적용된다.

     > **장점**
     >
     > - HTML 문서 안의 여러 요소를 한번에 꾸밀 수 있다.

     > **단점**
     >
     > - 또 다른 HTML 문서에는 적용할 수 없다.

     <br/>

  3. **외부 스타일 시트(External style sheet)**

     ```css
     h1 {
       color: red;
     }
     ```

     ```html
     <head>
       <link rel="stylesheet" href="style.css" />
     </head>
     ```

     - 별도의 CSS 파일을 만들고 HTML 문서와 연결하는 방법
     - 스타일을 적용할 웹 페이지의 `<head>`태그에 `<link>`태그를 사용하여 외부 스타일 시트를 포함해야만 스타일이 적용

     > **장점**
     >
     > - 여러 HTML 문서에 사용할 수 있다.

     <br/>

- CSS 규칙의 우선순위는 어떻게 결정될까요?

  - 위의 스타일 적용 방법들이 혼합되어 사용될 경우

    <aside>
    💡 !important와 인라인 스타일은 실무에서 사용을 제한하는 경우가 대다수

    </aside>

    1. !important (css 값 뒤에 붙여진 키워드)
    2. 인라인 스타일 (HTML 요소 내부에 위치함)
       - 내부나 외부 스타일 시트와는 상관없이 무조건 인라인 스타일이 적용
    3. 내부 / 외부 스타일 시트 (HTML 문서의 head 요소 내부에 위치함)
       - id > class, 다른 attribute, 수도클래스(:first-child같은 것) > tag element, 수도엘레먼트(::before같은 것)
    4. 웹 브라우저 기본 스타일

    - 우선순위가 같다면 개수로 순위가 정해진다.
    - 개수마저 같다면 뒤에 나오는 것이 순위가 높다.

    <br/>

- CSS의 박스모델은 무엇일까요? 박스가 화면에서 차지하는 크기는 어떻게 결정될까요?

  - 모든 HTML 요소는 박스(box) 모양으로 구성되며, 이것을 박스 모델(box model)이라고 부른다.
  - 박스 모델은 HTML 요소를 **패딩(padding)**, **테두리(border)**, **마진(margin)**, 그리고 **내용(content)** 으로 구분한다.

  ![http://www.tcpschool.com/lectures/img_css_boxmodel.png](http://www.tcpschool.com/lectures/img_css_boxmodel.png)

  1. 내용(content) : 텍스트나 이미지가 들어있는 박스의 실질적인 내용 부분
  2. 패딩(padding) : 내용과 테두리 사이의 간격. 패딩은 눈에 보이지 않는다.
  3. 테두리(border) : 내용와 패딩 주변을 감싸는 테두리
  4. 마진(margin) : 테두리와 이웃하는 요소 사이의 간격. 마진은 눈에 보이지 않는다.

  - height와 width 속성의 이해

  - CSS에서 `height`와 `width`속성의 크기가 가르키는 부분은 **내용(content) 부분**만을 대상으로 한다.
  - 패딩(padding), 테두리(border), 마진(margin)의 크기는 포함되지 않는다.

  - HTML 요소의 높이와 너비

    - HTML 요소의 전체 너비(width)
      - width + left padding + right padding + left border + right border + left margin + right margin
    - HTML 요소의 전체 높이(height)

      - height + top padding + bottom padding + top border + bottom border + top margin + bottom margin

    - 마진(margin) 영역의 크기는 눈으로 불가능 하나, HTML 요소가 차지하는 크기에는 포함된다.

    <br/>

- `float` 속성은 왜 좋지 않을까요?

  - 이미지 + 텍스트 배치 용도(ex.잡지)로 사용되기 시작한 float 속성을 **전체 레이아웃 용**으로 사용할 시 다음과 같은 문제가 발생할 수 있다.

  1. **부모 요소의 크기**
     - float 속성이 적용된 요소의 부모요소의 overflow 속성이 visible인 상태에서는 부모 요소의 크기가 자동으로 늘어나지 않는다.
  2. **요소의 높이가 제각각일 때**

     ```html
     <head>
       <style>
         .container {
         }
         .item {
           float: left;
         }
       </style>
     </head>
     <body>
       <div class="container">
         <div
           class="item"
           style="width:50%;height:100px;background-color:#000;"
         ></div>
         <div
           class="item"
           style="width:50%;height:80px;background-color:#333;"
         ></div>
         <div
           class="item"
           style="width:50%;height:100px;background-color:#666;"
         ></div>
         <div
           class="item"
           style="width:50%;height:120px;background-color:#999;"
         ></div>
       </div>
     </body>
     ```

     ![https://blog.eunsatio.io/article/images/2019.03/13/capture-1.1552453625503.JPG](https://blog.eunsatio.io/article/images/2019.03/13/capture-1.1552453625503.JPG)

     - 요소 정렬을 위해 사용할 때, 요소의 높이가 제각각이고 줄바꿈이 있는 레이아웃일 시 정렬이 깨지는 현상이 일어난다.

  3. **속성의 상속** - float는 요소의 흐름을 관리하는 속성이기 때문에 속성이 다음 요소로 상속된다. - float 속성이 적용된 요소 다음 요소에 [clear 속성](https://developer.mozilla.org/ko/docs/Web/CSS/clear)을 적용해야 한다. (float 해제)
     <br/>

- Flexbox(Flexible box)와 CSS Grid의 차이와 장단점은 무엇일까요?

  - Flexbox는 한 번에 하나의 차원(행이나 열)의 레이아웃만 다루는 1차원 레이아웃 모델이고, CSS Grid은 행과 열의 레이아웃을 함께 조절하는 2차원 레이아웃 모델이다.

  - Flexbox는 float에서 발생할 수 있는 이슈들을 해결하고 있지만, Flexbox의 CSS 코드를 읽고 시각적으로 어떻게 페이지에 배치되는지 유추하는 것이 어렵다.

  - CSS Grid는 현재 브라우저 지원이 Flexbox만큼 좋지 않다. (Flexbox는 IE 9이하, Opera 11.5 이하를 제외하고 대부분 지원한다.)
    <br/>

- CSS의 비슷한 요소들을 어떤 식으로 정리할 수 있을까요?

  - CSS의 상속 또는 SCSS의 중첩 기능을 활용한다.
  - CSS의 class 기능을 활용하여 재사용성을 높인다.
  - SCSS의 mixin 기능을 활용하여 재사용성을 높인다.

  <br/>

## Quest

- Quest 01에서 만들었던 HTML을 바탕으로, [이 그림](screen.png)의 레이아웃과 CSS를 최대한 비슷하게 흉내내 보세요. 꼭 완벽히 정확할 필요는 없으나 align 등의 속성은 일치해야 합니다.
- **주의사항: 되도록이면 원래 페이지의 CSS를 참고하지 말고 아무것도 없는 백지에서 시작해 보도록 노력해 보세요!**

## Advanced

- 왜 CSS는 어려울까요?
- CSS의 어려움을 극복하기 위해 어떤 방법들이 제시되고 나왔을까요?
- CSS가 브라우저에 의해 해석되고 적용되기까지 내부적으로 어떤 과정을 거칠까요?
- 웹 폰트의 경우에는 브라우저 엔진 별로 어떤 과정을 통해 렌더링 될까요?
