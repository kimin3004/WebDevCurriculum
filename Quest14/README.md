# Quest 14. 정적 분석: 타입스크립트와 린트 시스템

## Introduction

- 이번 퀘스트에서는 타입스크립트와 린트 시스템을 통해 코드에 대한 정적분석의 장점에 대해 알아보겠습니다.

## Topics

- Lint
  - ESLint
- TypeScript

## Resources

- [ESLint](https://eslint.org/)
- [TypeScript](https://www.typescriptlang.org/)

## Checklist

- 코드를 린팅하는 것의 장점은 무엇일까요?

  - **린트(lint)** 또는 **린터(linter)**는 코드 상태 및 스타일과 관련하여 버그로 이어질 수있는 문제를 찾기 위해 코드를 프로그래밍 방식으로 스캔하는 도구이다.
  - 협업 시 팀만의 코드 규칙을 정해 균일한 코드 품질 유지에 도움을 줄 수 있다.
    - 일관된 코드 스타일 유지한다. (일관성으로 인해 코드의 구조 파악이 수월해진다.)
    - 구문 오류로 인해 코드의 버그에 플래그를 지정한다.
    - 코드가 직관적이지 않을 때 경고를 제공한다.
    - 일반적인 모범 사례에 대한 제안을 제공한다.

  <br/>

  - 린트 규칙은 어떻게 설정하는 것이 좋을까요? 너무 빡빡한 규칙과 너무 헐거운 규칙 사이에서 어떻게 밸런스를 잡아야 할까요?

    - `Prettier`를 활용해 자신이 작성한 코드를 정해진 코딩 스타일에 맞게 변환할 수 있고, `ESLint`를 활용해 자신이 작성한 코드 품질이 괜찮은지 검사할 수 있다. (코드 품질을 바로 잡은 뒤 코드를 정해진 코딩 스타일에 맞게 수정)

    - `.eslintrc.{js,yml,json}` 내부의 `rules`에 직접 규칙을 설정할 수 있으며, 설정할 수 있는 기본 규칙과 옵션은 [ESLint Rules](https://eslint.org/docs/rules/)에서 확인가능하다.
    - 설정한 규칙을 어긴 코드가 있을 때 warning 또는 error를 발생시키도록 설정할 수 있는데, error가 하나라도 발생할 경우 ESLint의 종료 코드(exit code)가 1이 되며, warning은 종료 코드에 영향을 미치지 않는다.
    - 많은 개발자가 사용중인 (대중성이 있는) Airbnb Style Guide, Google Style Guide를 토대로 팀에 맞는 규칙들을 추가하는 것도 하나의 방법이 될 수 있다.

    <br/>

- 타입스크립트는 어떤 언어인가요?

  - 마이크로소프트에서 구현한 **JavaScript의 슈퍼셋(Superset) 프로그래밍 언어**이다.
  - 컴파일을 통해 JavaScript 코드를 출력한다. 최종적으로 런타임에서는 출력된 JavaScript 코드를 구동시키게 된다.
  - ES6의 클래스, 모듈 등과 ES7의 Decorator 등을 지원한다.

  <br/>

  - 타입스크립트를 사용했을 때 얻을 수 있는 장점은 무엇인가요?

    1. **정적 타입** - 컴파일 단계에서 오류를 포착할 수 있는 장점
    2. **도구의 지원** - IDE와 같은 도구에 타입 정보를 제공함으로써 높은 수준의 코드 어시스트, 타입 체크, 리팩토링 등을 지원받을 수 있으며 이러한 도구의 지원은 대규모 프로젝트를 위한 필수 요소이다.
    3. **객체지향 프로그래밍 지원** - Java, C# 등의 클래스 기반 객체지향 언어에 익숙한 개발자가 자바스크립트 프로젝트를 수행하는데 진입 장벽을 낮추는 효과가 있다.
    4. **ES6 / ES NEXT 지원** - 현재 ES6를 완전히 지원하지 않고 있는 브라우저를 고려하여 Babel 등의 트랜스파일러를 사용해야 하는 현 상황에서 TypeScript가 새로운 스펙의 유용한 기능을 안전하게 도입하는데 유리하다.

    <br/>

  - 타입스크립트를 사용하면서 타입이 없는 라이브러리나 프레임워크를 사용해야 할 경우에는 어떻게 해야 할까요?

    - @types 라이브러리란?

      - 자바스크립트로 만들어진 써드 파티 라이브러리(jQuery, lodash, chart 등)를 타입스크립트에서 사용하려면 각 기능에 대한 타입이 정의되어 있어야 한다.
      - 자바스크립트 라이브러리는 대부분 @types라는 별칭으로 타입스크립트 추론이 가능한 보조 라이브러리를 제공한다.
        `npm i -D @types/lodash`

    1. 커스텀 패키지 타이핑

    - tsconfig.json 파일에서 compilerOptions의 typeRoots를 설정한다.

    ```
      {
        "compilerOptions": {
          "noImplicitAny": true,
          "noImplicitThis": true,
          "noImplicitReturns": true,
          "strict": true,
          "typeRoots": ["./types"], // 보통 types 폴더를 만들어 타입 정의
          "declaration": true, // lib 만들 때 .d.ts 파일을 자동으로
          "declarationDir": "./types" // 이 폴더에 만들어주는 옵션
        }
      }
    ```

    - root에서 types 라는 폴더를 생성한다.
    - (선택1) type이 없던 외부 라이브러리의 이름(example-lib)으로 폴더를 하나 생성 후 폴더 내 index.t.ts를 생성한다.
    - (선택2) 엠비언트 모듈 types/example-lib.d.ts에서 타이핑을 설정한다.

    ```
    declare module 'example-lib' {
      const exampleLib: boolean;
      export default exampleLib;
    }
    ```

    - ts파일에서 type을 import해 사용한다.

    ```
    import example from 'example-lib'
    ```

  - any 타입을 남용하는 것은 왜 좋지 않을까요?

    - any는 주로 타입 정보가 없는 외부 라이브러리에 대한 타이핑하거나 혹은 자바스크립트와 타입스크립트를 혼용할 때 유용하다.
      - 부분적이고 점진적인 마이그레이션을 위해 any 타입이 사용 될 수 있다.
    - any를 남용하면 코드 분석 시 타입체킹의 도움은 받을 수 없다.

- 린트와 빌드 등의 과정을 개발 싸이클에서 편하게 수행하려면 어떻게 하는 것이 좋을까요?

  - Prettier와 같이 별도의 확장팩을 설치하여 코드를 저장할때마다 자동으로 Lint검사를 실시하도록 설정해줄 수 있다.

- 참고 : https://365kim.tistory.com/188

## Quest

- 메모장 시스템에 린트 시스템을 적용해 보세요.
- 메모장 시스템을 타입스크립트 기반으로 수정해 보세요.
- `package.json` 파일의 `scripts` 항목을 이용하여 린트와 빌드 등의 작업을 스크립트화 해 보세요.

## Advanced

- 자바스크립트 코드에 대한 정적분석은 어떤 과정을 통해 이루어질까요?
  - 이러한 정적분석을 수행해 주는 핵심 역할을 하는 npm 패키지는 어떤 것이 있을까요?
