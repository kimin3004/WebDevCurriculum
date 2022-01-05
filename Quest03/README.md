# Quest 03. 자바스크립트와 DOM

## Introduction

- 자바스크립트는 현재 웹 생태계의 근간인 프로그래밍 언어입니다. 이번 퀘스트에서는 자바스크립트의 기본적인 문법과, 자바스크립트를 통해 브라우저의 실제 DOM 노드를 조작하는 법에 대하여 알아볼 예정입니다.

## Topics

- 자바스크립트의 역사
- 기본 자바스크립트 문법
- DOM API
  - `document` 객체
  - `document.getElementById()`, `document.querySelector()`, `document.querySelectorAll()` 함수들
  - 기타 DOM 조작을 위한 함수와 속성들
- 변수의 스코프
  - `var`, `let`, `const`

## Resources

- [자바스크립트 첫걸음](https://developer.mozilla.org/ko/docs/Learn/JavaScript/First_steps)
- [자바스크립트 구성요소](https://developer.mozilla.org/ko/docs/Learn/JavaScript/Building_blocks)
- [Just JavaScript](https://justjavascript.com/)

## Checklist

- 자바스크립트는 버전별로 어떻게 변화하고 발전해 왔을까요?

  - 자바스크립트 초기 버전은 Brendan Eich가 초기 브라우저인 네스케이프에 탑재하기 위해 만들었고, 인터넷 익스플로러도 상표권 이슈를 피하기 위해 JScript라 이름 붙인 자바스크립트의 방언을 개발했다.
  - 두 언어가 분화되는 것을 막기 위해 네스케이프는 자바스크립트 표준화를 ECMA International라는 국제 표준 단체로 넘기게 된다.

  <br/>

  - 자바스크립트의 버전들을 가리키는 ES5, ES6, ES2016, ES2017 등은 무엇을 이야기할까요?

    - **ECMAScript**는 ES라는 약어로도 사용되어지며 정보 통신 시스템의 표준화하는 조직인 ECMA International에서 ECMA-262 기술 규격에 따라 정의하고 있는 표준화된 스크립트 프로그래밍 언어이다.
    - 자바스크립트를 표준화하기 위해 만들어 졌다.
    - ECMA에서 해마다 업데이트를 하기로 결정하며, ECMAScript의 초기 버전(ES1, ES2, ES3, ES4, ES5)은 숫자로 이름이 지정되었으며 1씩 증가하는 형태였다.
    - ECMAScript 6(ES6)인 경우 ES6가 먼저 대중화된 이름으로 알려졌으나, 나중에 ECMAScript 2015 (ES2015)로 공개연도를 반영한 이름으로 변경되었고 이후부터는 공개 될 연도에 따라 이름이 지정되게 된다.
    - ES2016 (ES7), ES2017 (ES8), ES2018 (ES9)...

  - 자바스크립트의 표준은 어떻게 제정될까요?

    - Ecma 내에는 표준별로 기술 위원회(Technical Committee, TC)가 존재하는데 EcmaScript는 **TC39** 라는 위원회가 명세를 관리한다.
    - TC39의 구성원 목록은 Mozilla, Google, Apple, Microsoft 등의 메이저 브라우저 벤더를 비롯해 Facebook, Twitter등 의 인터넷 대기업들이 있으며, 필요에 따라 프로그래밍 언어를 전공한 대학 교수나 전문가들이 초청되기도 한다.
    - TC39 미팅은 보통 2-3달에 한 번 이루어지며, 회의록은 모두 [웹상](https://github.com/tc39/notes)에 공개된다.

    <br/>

- 자바스크립트의 문법은 다른 언어들과 비교해 어떤 특징이 있을까요?

  - **프로토 타입 기반의 언어**

    - JavaScript에서 class는 존재하지만 C++의 class와는 완전히 다른 개념이며, JavaScript의 객체지향 프로그래밍은 함수 프로토타입에 기반한 객체지향 프로그래밍이다.

  - **동적 타이핑, 약타입 언어**

    - 정적으로 사용하기 위해 JavaScript 슈퍼셋 언어인 **TypeScript**가 나오게 된다.

  - **객체(Object)의 상수(const) 개념**

    - 객체의 경우 `const`로 선언해도 메모리값만 상수일 뿐 객체 안의 내용은 변경이 가능하다.
    - 객체를 복사하여 사용할 때도 Deep clone하지 않으면 의도치 않게 원본 객체가 변경되어버리는 현상이 발생한다.
    - 이렇게 상수로 선언된 객체의 Immutability를 보장하기 위해 여러 테크닉이 쓰이게 되는데 주로 ES6에서 도입된 **Spread Operator**를 사용하는 것이 일반적이다.

    <br/>

  - 자바스크립트에서 반복문을 돌리는 방법은 어떤 것들이 있을까요?

    - for 문
      ```
      for ([초기문]; [조건문]; [증감문])
        문장
      ```
    - do...while 문

      ```
      do
        문장
      while (조건문);
      ```

      ```js
      do {
        i += 1;
        console.log(i);
      } while (i < 5);
      ```

    - while 문

      ```
      while (조건문)
        문장
      ```

    - 레이블 문

      - 다른 곳으로 참조할 수 있도록 식별자로 문을 제공한다.

      ```
      label :
        statement
      ```

      ```js
      markLoop: while (theMark == true) {
        doSomething();
      }
      ```

    - break 문

      - 반복문, switch문, 레이블 문과 결합한 문장을 빠져나올 때 사용한다.
        - 레이블 없이 break문을 쓸 때: 가장 가까운 while, do-while, for, 또는 switch문을 종료하고 다음 명령어로 넘어간다.
        - 레이블 문을 쓸 때: 특정 레이블 문에서 끝난다.

      ```js
      let x = (y = 0);
      OuterLoop: while (true) {
        console.log('Outer loop: ' + x);
        x += 1;
        y = 1;

        while (true) {
          console.log('Inner loop: ' + y);
          y += 1;
          if (y === 5 && x === 5) {
            break OuterLoop;
          } else if (y === 5) {
            break;
          }
        }
      }
      ```

    - continue 문
      - continue 문은 while, do-while, for, 레이블 문을 다시 시작하기 위해 사용한다.
        - 레이블 없이 continue문을 쓸 때: 가장 안쪽의 while, do-while, for 문을 둘러싼 현재 반복을 종료하고, 다음 반복으로 실행을 계속한다. break문과 달리, continue 문은 전체 반복문의 실행을 종료하지 않는다. while 루프에서는 다시 조건으로 이동하고, for 루프에서는 증가 표현으로 이동하게 된다.
    - for...in 문

      - 객체의 열거 속성을 통해 지정된 변수를 반복한다.
      - 객체의 key 값에 접근할 수 있지만, value 값에 접근하는 방법은 제공하지 않는다.

      ```js
      for (variable in object) {
        statements;
      }
      ```

      ```js
      const obj = {
        a: 1,
        b: 2,
        c: 3,
      };

      for (const prop in obj) {
        console.log(prop, obj[prop]); // a 1, b 2, c 3
      }
      ```

    - for...of 문

      - [Symbol.iterator] 속성을 가지는 반복 가능한 객체 (배열, Map, Set, 인수 객체 등을 포함) 전용이다.

      ```js
      const iterable = [10, 20, 30];

      for (const value of iterable) {
        console.log(value); // 10, 20, 30
      }
      ```

- 자바스크립트를 통해 DOM 객체에 CSS Class를 주거나 없애려면 어떻게 해야 하나요?

  1. 클래스 추가

  - 단일 클래스 추가

    - `elementNode.className = '클래스명';`
    - `elementNode.setAttribute('class', '클래스명');`
    - `elementNode.classList.add('클래스명');`

  - 여러 개의 클래스 추가

    - `elementNode.className = '클래스명1 클래스명2`
    - `elementNode.setAttribute('class', '클래스명1 클래스명2');`
    - `elementNode.classList.add('클래스명1', '클래스명2', ...classNameArray);`

  - 기존 클래스에 클래스 추가

    - `elementNode.className += '클래스명3';`
    - `parent.setAttribute('class', parent.getAttribute('class')+' 클래스명3');`
    - `elementNode.classList.add('클래스명3');`

    <br/>

  2. 클래스 삭제

  - 클래스 속성 자체 삭제

    - `elementNode.removeAttribute('클래스명');`

  - 클래스 속성은 남기고 클래스만 삭제

    - `elementNode.className = '';`
    - `elementNode.setAttribute('class', '');`
    - `elementNode.classList.remove('클래스1', '클래스2', ...classNameArray);`

  - IE9나 그 이전의 옛날 브라우저들에서는 어떻게 해야 하나요?
    - 위의 방법들 중 `classList`를 사용한 클래스 관리방법은 현재 IE9나 그 이전의 옛날 브라우저들은 지원이 되지 않으므로, 그 외 관리방법들을 사용하자

  <br/>

- 자바스크립트의 변수가 유효한 범위는 어떻게 결정되나요?

  - 변수를 포함한 모든 식별자는 **자신이 선언된 위치**에 의해 다른 코드가 자신을 참조할 수 있는 유효 범위가 결정된다.
  - 크게 전역변수와 지역변수로 나뉘게 되는데, 지역변수는 함수 몸체 내부에서 선언된 변수를 뜻한다.
  - 전역변수는 어디서든 참조가 가능하며, 지역 변수는 자신의 지역 스코프와 하위 지역 스코프에서 유효하다.

  - `var`과 `let`으로 변수를 정의하는 방법들은 어떻게 다르게 동작하나요?

    - var로 정의된 변수는 함수의 코드블록만을, let으로 정의된 변수는 블록 레벨 스코프를 지역 스코프로 인정한다.
    - 변수 중복 선언이 가능한 var과는 다르게 let은 변수 중복 선언이 불가능 하다.
    - 둘 다 변수 호이스팅이 발생하나, var은 평가단계에서 선언댠계와 초기화단계가 동시에 진행되어 undefined가 할당되게 된다. 변수 선언 이전에 접근해도, 스코프에 변수가 존재하기 때문에 에러가 발생하지 않는다.
    - 하지만 let은 선언단계와 초기화 단계가 분리되어 진행되며, 런타임에 실행흐름이 변수 선언문에 도달하기 전까지 일시적 사각지대(TDZ)에 빠지게 된다. 이때 참조가 불가능하여 ReferenceError가 발생하게 된다.

- 자바스크립트의 익명 함수는 무엇인가요?
  - 자바스크립트의 Arrow function은 무엇일까요?

## Quest

- (Quest 03-1) 초보 프로그래머의 영원한 친구, 별찍기 프로그램입니다.

  - [이 그림](jsStars.png)과 같이, 입력한 숫자만큼 삼각형 모양으로 콘솔에 별을 그리는 퀘스트 입니다.
    - 줄 수를 입력받고 그 줄 수만큼 별을 그리면 됩니다. 위의 그림은 5를 입력받았을 때의 결과입니다.
  - `if`와 `for`와 `function`을 다양하게 써서 프로그래밍 하면 더 좋은 코드가 나올 수 있을까요?
  - 입력은 `prompt()` 함수를 통해 받을 수 있습니다.
  - 출력은 `console.log()` 함수를 통해 할 수 있습니다.

- (Quest 03-2) skeleton 디렉토리에 주어진 HTML을 조작하는 스크립트를 완성해 보세요.
  - 첫째 줄에 있는 사각형의 박스들을 클릭할 때마다 배경색이 노란색↔흰색으로 토글되어야 합니다.
  - 둘째 줄에 있는 사각형의 박스들을 클릭할 때마다 `enabled`라는 이름의 CSS Class가 클릭된 DOM 노드에 추가되거나 제거되어야 합니다.
- 구현에는 여러 가지 방법이 있으나, 다른 곳은 건드리지 않고 TODO 부분만 고치는 방향으로 하시는 것을 권장해 드립니다.

## Advanced

- Quest 03-1의 코드를 더 구조화하고, 중복을 제거하고, 각각의 코드 블록이 한 가지 일을 전문적으로 잘하게 하려면 어떻게 해야 할까요?
- Quest 03-2의 스켈레톤 코드에서 `let` 대신 `var`로 바뀐다면 어떤 식으로 구현할 수 있을까요?
