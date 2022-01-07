# Quest 05. OOP 특훈

## Introduction

- 이번 퀘스트에서는 바닐라 자바스크립트의 객체지향 프로그래밍을 조금 더 훈련해 보겠습니다.

## Topics

- Separation of Concerns
- 객체지향의 설계 원칙
  - SOLID 원칙
- Local Storage

## Resources

- [Separation of concerns](https://jonbellah.com/articles/separation-of-concerns/)
- [SOLID](https://en.wikipedia.org/wiki/SOLID)
- [객체지향 설계 5원칙](https://webdoli.tistory.com/210)
- [MDN - Local Storage](https://developer.mozilla.org/ko/docs/Web/API/Window/localStorage)

## Checklist

- 관심사의 분리 원칙이란 무엇인가요? 웹에서는 이러한 원칙이 어떻게 적용되나요?

  - 소프트웨어 상에서 구조를 관심사(패턴, 역할, 기능 등)에 따라 각각 맞게 섹션 별로 분리하는 것이다. 섹션 별로 코드를 작성을 할 때는 그 특성에 맞는 하나의 역할만을 부여해서 작성을 해야한다.

  - 개별 영역들에 대한 이해가 쉽고, 코드 재사용성이 올라가며 변경과 유지보수가 수월해진다.

  - 웹에서는 HTML, CSS, JS애서 관심사의 분리 원칙이 적용된다.

    - HTML과 CSS 분리

      - HTML 파일에서는 **웹 페이지의 구조**만 신경쓰고, CSS 파일에서는 **스타일**만 신경쓸 수 있도록 구현해야 한다.

      - HTML 인라인 스타일을 이용해 스타일을 제어하면, CSS우선순위가 제일 높기 때문에 CSS파일에서 오버라이딩이 되지 않는 이슈가 발생할 수 있다. (`!important`으로 강제로 우선순위를 높일 수 있으나 유지보수 측면에서 지양되는 방식이다.)

      - 아래와 같이 Bootstrap에서 자주 사용되는 presentational class 방식은 HTML에서 해당 컴포넌트의 스타일(그리드 시스템)을 설명하고 있다. 이 역시 관심사의 분리 원칙에 위배된다고 볼 수 있다.

        ```HTML
        <div class="my-element col-lg-3 col-md-4 col-sm-6 col-xs-12"></div>
        ```

      - 아래처럼 Sass Mixin을 사용하여 CSS에서 스타일을 관리할 수 있도록 수정할 수 있다.

        ```HTML
        <div class="my-element">
        ```

        ```SCSS
        .my-element {
          @include col-lg(3);
          @include col-md(4);
          @include col-sm(6);
          @include col-xs(12);
        }
        ```

    <br/>

    - 그럼 CSS in JS는?

      - CSS in JS는 JS 구문으로 CSS를 작성할 수 있게 도와준다.
      - ClassName의 경우 해시값으로 생성되기 때문에 중복을 피하기 위해 개발자가 고민할 필요를 줄여준다.
      - BEM을 사용한 className과 달리 컴포넌트 명을 통해 더욱 직관적으로 의미를 나타낼 수 있다.

        ```jsx
        // 기존 방식
        <button className="primary">기본버튼</button>
        <button className="red">동의버튼</button>
        // CSS in JS
        <Button>기본버튼</Button>
        <AgreeButton>파란버튼</AgreeButton>
        ```

      - 관심사의 분리 원칙에 위배되는 것은 아닐까?

        ![http://www.didoo.net/wp-content/uploads/2017/02/separation-of-concerns.png](http://www.didoo.net/wp-content/uploads/2017/02/separation-of-concerns.png)

        - CSS in JS는 기존 웹에서의 **스타일** 기능보다는 현대 웹 개발에서 자주 사용되는 **컴포넌트**를 단위로 하나의 관심사로 본다고 말할 수 있다.

        - 아래처럼 컴포넌트의 로직과 style파일을 분리하여 사용할 수 있을 것이다. (style파일에서 styled 컴포넌트들을 import 해서 사용)

          ```
          ├─ Footer
          │  ├─ index.tsx
          │  └─ style.ts
          ```

  <br/>

- 객체지향의 SOLID 원칙이란 무엇인가요? 이 원칙을 구체적인 예를 들어 설명할 수 있나요?

  - **S : Single Responsibility Principle (단일 책임 원칙)**

    > _단일 모듈은 사용자 그룹 또는 이해관계자 그룹 중에 하나의 그룹을 책임져야 한다. 하나의 그룹을 책임지기 위해서는 같은 책임으로 구성된 응집된 코드를 작성되야 한다._

    - 하나의 모듈의 기능이 비대하다면 책임을 분할하여 이를 갖게하는 것을 의미한다.

    - 모듈의 의미를 하나의 클래스로도 볼 수도 있다. (클래스를 변경하는 이유는 하나여야 한다는 기준으로 기능을 나눈다.)

    <br/>

    - 또는 하나의 이상으로 작성된 자바스크립트 소스 파일로 볼 수 있다. 모듈은 함수와 데이터 구조로 구성된 응집된 집합이다.

    - 예시

      ```
      ├─ single-module.js
      ├─ group-module
      │  ├─ index.js
      │  ├─ hello.js
      │  └─ world.js
      ```

    - 자바스크립트 모듈화는 `require` 또는 `import/export` 로 구현할 수 있다.
    - 많은 파일을 묶을 때는 폴더에 `index.js` 파일을 만들어 바깥에 내보낼 수 있다.

    <br/>

  - **O : Open-Closed Principle (개방 폐쇄 원칙)**

    > _객체는 확장에는 열려 있어야 하지만 수정에는 닫혀 있어야 한다._

    - 이는 코드의 리팩토링과 관련된 원칙으로, **"새로운 기능을 추가할 때 기존 코드(함수)를 변경하는 대신, 해당 기능을 위한 코드(함수)를 새로 추가하라"** 는 의미로도 해석 가능하다.

      ```js
      // 배열을 직접 수정하는 대신, 역할군 추가를 위한 함수를 생성한다.
      // let accessibles = ['CEO', 'CSO', 'CTO', 'CFO', 'STAFF'];
      let accessibles = ['CEO', 'CTO', 'CFO', 'STAFF'];

      // 역할군 추가를 위해 생성한 함수
      function addAccessibles(role) {
        accessibles.push(role);
      }

      function isAccessible(employee) {
        if(accessibles.includes(employee.role) {
          // 해당 직원이 접근 권한을 가질 경우
          return true;
        } else {
          // 접근 권한이 없을 경우
          return false;
        }
      }

      ```

      <br/>

  - **L : Liskov Substitution Principle (리스코프 치환 원칙)**

    > _부모 클래스의 인스턴스를 자식 클래스의 인스턴스로 대체해도 프로그램의 기능은 동일해야 한다._

    - 서브 클래스가 슈퍼 클래스의 책임을 무시하거나 재정의하지 않고 확장만 수행한다는 것을 의미한다.

    ```java
      class Bag {
        private price: number;

        getPrice() {
          return this.price;
        }

        setPrice(price: number) {
          this.price = price;
        }
      }

      class DiscountedBag extends Bag {
        private discountRate: number;

        setDiscountRate(discountRate: number) {
          this.discountRate = discountRate;
        }

        // 자식클래스가 부모클래스를 오버라이딩하거나 추가적인 기능을 총해 부모의 상태를 변경시키는 것은 LSP원칙을 위반하는 것이다.
        applyDiscount(price: number) {
          super.setPrice( price - (this.discountRate * price));
        }
      }
    ```

    - 부모가 수행하고 있는 책임을 그대로 수행하면서 추가적인 필드나 기능을 제공하려는 경우에만 상속을 하는 것이 바람직하다.

    <br/>

- **I : Interface Segregation Principle (인터페이스 분리 원칙)**

  > _인터페이스는 사용하는 클라이언트 기준으로 분리해야한다._

  - 하나의 클래스가 기능이 비대하다면 책임을 분할하여 이를 갖게하는 것이 SRP(단일 책임 원칙)이고 **비대한 기능을 인터페이스로 분할하여 사용하는 것**이 ISP(인터페이스 분리 원칙)를 의미한다.

  - 여기서 클라이언트란 클라이언트 자신이 이용하지 않는 기능에는 영향을 받지 않아야 하는 뜻을 의미한다.

  - 예시

    ```js
    function EmployeeData(name, pos, hours) {
      this.name = name;
      this.pos = pos;
      this.hours = hours;
    }

    // 재정팀이 사용할 기능을 PayCalculator 모듈로 분리한다.
    function PayCalculator(employeeData) {
        this.data = employeeData;

        this.calculatePay = function() {
            ...
        }
    }

    // 인사팀이 사용할 기능을 WorkHourCalculator 모듈로 분리한다.
    function WorkHourCalculator(employeeData) {
        this.data = employeeData;

        this.calculateWorkHour = function() {
            ...
        }
    }

    // 시스템이 사용할 기능을 SystemDataUpdator 모듈로 분리한다.
    function SystemDataUpdator(employeeData) {
        this.data = employeeData;

        this.updateData = function() {
            ...
        }
    }
    ```

    - 각 모듈이 각각의 클라이언트를 위해 동작하도록 역할, 기능을 분리하여 설계한다.

- **D : Dependency Inversion Principle (의존성 역전 원칙)**

  - 객체지향 프로그래밍에서 객체는 서로 도움을 주고 받으며 의존 관계를 발생시킨다.

  - 의존 관계를 맺을 때 변화하기 쉬운 것 또는 자주 변화하는 것보다는 **변화하기 어려운 것, 거의 변화가 없는 것에 의존하라**는 가이드라인을 제공하는 원칙이다.

  <br/>

- 참고 : [peter-cho.gitbook.io](https://peter-cho.gitbook.io/book/13-1/solid/srp#undefined), [merrily-code.tistory.com](https://merrily-code.tistory.com/201) , [charming-kyu.tistory.com](https://charming-kyu.tistory.com/35?category=1023563)

  <br/>

- 로컬 스토리지란 무엇인가요? 로컬 스토리지의 내용을 개발자 도구를 이용해 확인하려면 어떻게 해야 할까요?

  - 현재 출처의 로컬 저장 공간에 접근할 수 있는 웹 스토리지 객체(web storage object)이다.
  - `Window.localStorage`를 이용해 doeument(웹페이지) 출처의 Storage 객체에 키-값 쌍을 조회, 저장, 삭제 등을 할 수 있게 접근할 수 있다.
  - localStorage는 sessionStorage와 비슷하지만, localStorage의 데이터는 만료되지 않고 sessionStorage의 데이터는 페이지 세션이 끝날 때, 즉 페이지를 닫을 때 사라지는 점이 다르다.

    ```js
    // 저장
    localStorage.setItem('myCat', 'Tom');
    // 조회
    const cat = localStorage.getItem('myCat');
    // 부분 삭제
    localStorage.removeItem('myCat');
    // 전체 삭제
    localStorage.clear();
    ```

  - 개발자 도구에서 `Application탭 - LocalStorage`에서 확인 가능하며, 도구 내에서 임의로 값을 추가, 삭제, 수정이 가능하다.

## Quest

- 외부 라이브러리나 프레임워크를 사용하지 않고, 자바스크립트를 이용하여 간단한 웹브라우저 기반의 텍스트 에디터를 만들어 보겠습니다.
  - 기본적으로 VSCode와 같이 탭을 이용해 여러 개의 파일을 동시에 편집할 수 있습니다.
  - 탭을 눌러 열려 있는 다른 파일을 편집할 수 있으며 탭을 언제든지 닫을 수 있습니다.
  - VSCode와 같이 새 파일, 로드, 저장, 다른 이름으로 저장 등의 기능을 가집니다. 저장은 웹 브라우저의 로컬 스토리지를 이용합니다.
  - VSCode와 같이 탭이 수정되었는데 저장되기 이전일 경우 이를 알려주는 인디케이터가 작동합니다.
  - 같은 이름의 파일을 저장할 경우 에러를 표시해야 합니다.
- 이번 퀘스트의 결과물은 앞으로의 많은 퀘스트에서 재사용되게 되니, 주의깊게 코드를 작성해 보세요!

## Advanced

- 웹 프론트엔드 개발에서 이러한 방식의 패턴을 더 일반화해서 정리할 수 있을까요? 이 퀘스트에서 각각의 클래스들이 공통적으로 수행하게 되는 일들에는 무엇이 있을까요?
