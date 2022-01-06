# Quest 04. OOP의 기본

## Introduction

- 이번 퀘스트에서는 바닐라 자바스크립트의 객체지향 프로그래밍에 대해 알아볼 예정입니다.

## Topics

- 객체지향 프로그래밍
  - 프로토타입 기반 객체지향 프로그래밍
  - 자바스크립트 클래스
    - 생성자
    - 멤버 함수
    - 멤버 변수
  - 정보의 은폐
  - 다형성
- 코드의 재사용

## Resources

- [MDN - Classes](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Classes)
- [MDN - Inheritance and the prototype chain](https://developer.mozilla.org/ko/docs/Web/JavaScript/Inheritance_and_the_prototype_chain)
- [MDN - Inheritance](https://developer.mozilla.org/ko/docs/Learn/JavaScript/Objects/Inheritance)
- [Polymorphism](https://medium.com/@viktor.kukurba/object-oriented-programming-in-javascript-3-polymorphism-fb564c9f1ce8)
- [Class Composition](https://alligator.io/js/class-composition/)
- [Inheritance vs Composition](https://woowacourse.github.io/javable/post/2020-05-18-inheritance-vs-composition/)

## Checklist

- 객체지향 프로그래밍은 무엇일까요?

  - `#`로 시작하는 프라이빗 필드는 왜 필요한 것일까요? 정보를 은폐(encapsulation)하면 어떤 장점이 있을까요?

    - 캡슐화(encapsulation)란

      1. 객체의 속성(data fields)과 행위(메서드, methods)를 하나로 묶고,
      2. 실제 구현 내용 일부를 외부에 감추어 은닉

      하는 것을 의미한다.

    - 캡슐화를 하는 중요한 목적은 바로 정보은닉이다. 유저 정보를 가지고 있는 User라는 객체에서 유저의 정보가 public으로 선언되어 있다면, 누구든 접근해서 유저 정보를 변경할 수 있다. 그렇기 때문에 private로 해서 데이터를 보호해서 접근을 제한해야한다.

    - 이렇게 보호된 변수는 getter나 setter 등의 메서드를 통해서만 간접적으로 접근이 가능하도록 하는 것이 캡슐화의 중요한 목적이다.

- 다형성이란 무엇인가요? 다형성은 어떻게 코드 구조의 정리를 도와주나요?

  - 프로그램 언어 각 요소들(상수, 변수, 식, 객체, 메소드 등)이 다양한 자료형(type)에 속하는 것이 허가되는 성질을 의미한다.

  - 같은 동작이지만 다른 결과물이 나올 때를 생각하면 된다.

  - 같은 이름의 속성을 유지함으로서, 속성을 사용하기 위한 인터페이스를 유지하고, 메서드 이름을 낭비하지 않는다는 것이다.
    예를 들어, 고양이와 사자의 울음소리를 호출하기 위해서 각 객체에서 roar() 메서드를 호출하면 된다.

  (추가로 찾아보기)

    <br/>

- 상속이란 무엇인가요? 상속을 할 때의 장점과 단점은 무엇인가요?

  - 자식 클래스가 부모 클래스의 기능을 받아 쓰는 것이며, 이때 자식 클래스는 부모 클래스의 기능을 받았으므로 부모의 역할도 할 수 있게 된다.
  - 기존 클래스에 새로운 기능을 얼마든지 덧붙이거나 심지어 기존 클래스까지 새롭게 정의할 수 있다.(재정의)
  - is-a 관계

    - laptop is a computer (o)
    - laptop is a desktop (x)

    <br/>

  > 장점

  - 클래스나 상속을 쓰지 않았다면 사소한 수정 사항이라도 발생할 경우 모든 코드들을 일일히 수정해야 하지만, 상위 클래스에서 공통되는 코드들을 상속으로 하위클래스에 넘길 시 코드 중복을 피할 수 있다.
  - 재정의 같은 기능을 이용해 쉽게 기존 클래스도 수정할 수 있다.

  <br/>

  > 단점

  - 취약한 기반 클래스 문제(Fragile Base Class Problem)

    - 상위 계층에 있는 클래스의 작은 변경만으로도 해당 클래스의 하위 계층의 모든 클래스가 영향을 받아 취약해 질 수 있는 문제

    - 상속 구조가 복잡해지면 영향도를 예측하기가 어려워 상위 클래스 변경 시 하위 클래스가 정상 동작할 지 예측이 어렵다.

  - 정보 은닉의 파괴

    - 하위 클래스가 상위 클래스의 정보를 뜯어내 보안상 허점을 만들어낼 수 있다.

    <br/>

- OOP의 합성(Composition)이란 무엇인가요? 합성이 상속에 비해 가지는 장점은 무엇일까요?

  - 위의 글처럼 상속은 부모 클래스와 자식 클래스 간에 높은 결합도 (High Coupling)를 가지게 된다. (자식 클래스가 부모 클래스에게 강하게 결합된 구조)

  - 합성은 다른 객체의 인스턴스를 자신의 인스턴스 변수로 포함해서 재사용하는 방법을 의미한다.

  - 상속 관계는 클래스 사이의 정적인 관계인데 비해 합성 관계는 객체 사이의 동적인 관계이다. 코드 작성 시점에 결정한 상속 관계는 변경이 불가능하지만 합성 관계는 실행 시점에 동적으로 변경할 수 있다. 따라서, 상속 대신 합성을 사용하면 변경하기 쉽고 유연한 설계를 얻을 수 있다.

  - has-a 관계
    - Person has a name (o)

  <br/>

- 자바스크립트의 클래스는 어떻게 정의할까요?

  - 프로토타입 기반의 객체지향 프로그래밍은 무엇일까요?

    - 객체지향 프로그래밍의 한 종류로, 클래스가 없고 클래스 기반 언어에서 상속을 사용하는 것과는 다르게, 객체를 원형(프로토타입)으로 하여 복제의 과정을 통하여 객체의 동작 방식을 다시 사용할 수 있게 한다.

    - JS는 프로토타입을 기반으로 상속을 구현하여 코드의 중복을 제거한다.

    - 생성자 함수가 생성할 모든 인스턴스가 공통적으로 가져야할 프로퍼티나 메서드를 프로토타입으로 미리 구현해 두면, 모든 인스턴스는 상위 객체의 프로토타입을 공유하여 사용할 수 있다.

      ```js
      function Person(name) {
        this.name = name;
      }

      Person.prototype.sayName = function () {
        return `My name is ${this.name}`;
      };

      const mini = new Person('mini');
      const mickey = new Person('mickey');

      // 생성자 함수가 생성하는 모든 인스턴스는 하나의 sayName 메서드를 공유한다.
      console.log(mini.sayName === mickey.sayName); // true

      console.log(mini.sayName()); // My name is mini
      console.log(mickey.sayName()); // My name is mickey
      ```

      <br/>

  - 자바스크립트의 클래스는 이전의 프로토타입 기반의 객체지향 구현과 어떤 관계를 가지고 있나요?

    - ES6에서 클래스 문법이 추가되었는데, 단지 생성자-프로토타입 패턴을 숨기기 위한 Syntactic Sugar에 불과하다.

      ```js
      class Person {
        constructor(name) {
          this.name = name;
        }

        sayName() {
          return `My name is ${this.name}`;
        }
      }

      const mini = new Person('mini');
      const mickey = new Person('mickey');

      console.log(mini.sayName === mickey.sayName); // true

      console.log(mini.sayName()); // My name is mini
      console.log(mickey.sayName()); // My name is mickey
      ```

      - 위에서 생성자-프로토타입으로 작성한 코드와 결과가 동일하며, 동작 방식에도 큰 차이가 없는 것을 확인할 수 있다.

## Quest

- 웹 상에서 동작하는 간단한 바탕화면 시스템을 만들 예정입니다.
- 요구사항은 다음과 같습니다:
  - 아이콘은 폴더와 일반 아이콘, 두 가지의 종류가 있습니다.
  - 아이콘들을 드래그를 통해 움직일 수 있어야 합니다.
  - 폴더 아이콘은 더블클릭하면 해당 폴더가 창으로 열리며, 열린 폴더의 창 역시 드래그를 통해 움직일 수 있어야 합니다.
  - 바탕화면의 생성자를 통해 처음에 생겨날 아이콘과 폴더의 개수를 받을 수 있습니다.
  - 여러 개의 바탕화면을 각각 다른 DOM 엘리먼트에서 동시에 운영할 수 있습니다.
  - Drag & Drop API를 사용하지 말고, 실제 마우스 이벤트(mouseover, mousedown, mouseout 등)를 사용하여 구현해 보세요!

## Advanced

- 객체지향의 역사는 어떻게 될까요?
- Smalltalk, Java, Go, Kotlin 등의 언어들로 넘어오면서 객체지향 패러다임 측면에서 어떤 발전이 있었을까요?
