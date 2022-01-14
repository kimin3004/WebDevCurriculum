# Quest 09. 서버와 클라이언트의 대화

## Introduction

- 이번 퀘스트에서는 서버와 클라이언트의 연동, 그리고 웹 API의 설계 방법론 중 하나인 REST에 대해 알아보겠습니다.

## Topics

- expressJS, fastify
- AJAX, `XMLHttpRequest`, `fetch()`
- REST, CRUD
- CORS

## Resources

- [Express Framework](http://expressjs.com/)
- [Fastify Framework](https://www.fastify.io/)
- [MDN - Fetch API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API)
- [MDN - XMLHttpRequest](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest)
- [REST API Tutorial](https://restfulapi.net/)
- [CRUD](https://en.wikipedia.org/wiki/Create,_read,_update_and_delete)
- [MDN - CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS)

## Checklist

- 비동기 프로그래밍이란 무엇인가요?

  - 현재 태스크가 완료되지 않은 상태이더라도 다음 태스크를 곧바로 실행하는 방식이다.
  - 하나의 태스크가 완료될 때까지 기다려야하는 Blocking이 발생하지 않지만, 실행순서가 보장되지는 않는다.
  - Javascript 엔진 자체는 싱글 스레드로 동작하지만, 브라우저에서 지원하는 Web API, Event Loop, Task Queue 등의 요소들을 통해 동시성을 지원받을 수 있다.
  - 웹 개발에서는 **AJAX**(Asynchronous JavaScript And XML, 비동기적 JavaScript와 XML) 즉, 서버에서 추가 정보를 비동기적으로 가져올 수 있게 해주는 프로그래밍 방식이 있다.
  - 이를 통해 웹페이지를 새로고침하지 않고 필요한 부분의 데이터만 요청, 리로드가 가능하다.
  - 웹페이지 일부가 리로드 되는 동안에도 코드가 계속 실행되어 비동기식으로 작업할 수 있다.

  <br/>

  - 콜백을 통해 비동기적 작업을 할 때의 불편한 점은 무엇인가요? 콜백지옥이란 무엇인가요?

  <br/>

  - 자바스크립트의 Promise는 어떤 객체이고 어떤 일을 하나요?

    - 비동기적으로 값을 받아오기 때문에 **생성된 시점에서는 정확히 알 수 없는 값**들의 **처리상태**와 **처리결과**를 주로 관리하는 객체이다.
    - Promise 생성자 함수의 두가지 콜백함수인 resolve, reject 내부에서 비동기 처리를 수행하게 된다.

            ```js
            const promise = new Promise((resolve, reject) => {
              const xhr = new XMLHttpRequest();
              xhr.open('GET', 'https://jsonplaceholder.typicode.com/posts/1');
              xhr.send();

              xhr.onload = () => {
                if (xhr.status === 200) {
                  // 성공
                  resolve(JSON.parse(xhr.response));
                } else {
                  // 실패
                  reject(new Error(xhr.status));
                }
              };
            });

            promise
              .then((res) => console.log(JSON.stringify(res)))
              .catch((err) => console.error(err));
            ```

    - 생성된 직후의 프로미스는 기본적으로 `pending` 상태이나, 조건에 따라 (비동기 처리 성공 등) `resolve` 또는 `reject` 함수가 호출되면 상태가 변화하게 된다.

    | 프로미스의 상태 정보    | 의미                                  | 상태 변경 조건                   |
    | ----------------------- | ------------------------------------- | -------------------------------- |
    | **pending**             | 비동기 처리가 아직 수행되지 않은 상태 | 프로미스가 생성된 직후 기본 상태 |
    | **fulfilled** (settled) | 비동기 처리가 수행된 상태(성공)       | resolve 함수 호출                |
    | **rejected** (settled)  | 비동기 처리가 수행된 상태(실패)       | reject 함수 호출                 |

  - 자바스크립트의 `async`와 `await` 키워드는 어떤 역할을 하며 그 정체는 무엇일까요?

- 브라우저 내 스크립트에서 외부 리소스를 가져오려면 어떻게 해야 할까요?

  - 브라우저의 `XMLHttpRequest` 객체는 무엇이고 어떻게 동작하나요?

    - HTTP를 통해서 브라우저와 서버가 데이터를 주고 받을 수 있게 해주는 통신 객체이다.
    - 브라우저가 제공하는 Web API이며, JavaScript로 서버로 보내는 HTTP request를 작성할 수 있다.

  - `fetch` API는 무엇이고 어떻게 동작하나요?

  <br/>

- REST는 무엇인가요?

  - **REST**(Representational State Transfer)란 HTTP을 기반으로 클라이언트가 **서버의 필요한 자원(resource)에 접근하는 방식을 규정**하는 아키텍쳐이다.

    - 웹의 기존 기술과 HTTP 프로토콜을 그대로 활용하기 때문에 웹의 장점을 최대한 활용한다.

    - 네트워크 상에서 Client와 Server 사이의 통신 방식 중 하나이다.

  - 자원을 **이름(자원의 표현)** 으로 구분하여 해당 자원의 상태(정보)를 주고 받는 방식이다.

    - HTTP URI(Uniform Resource Identifier)를 통해 **자원(Resource)** 을 명시
    - HTTP Method(POST, GET, PUT/PATCH, DELETE)를 통해 해당 자원에 대한 **행위(Verb)** 를 명시
    - Client가 자원의 상태(정보)에 대한 조작을 요청하면 Server는 이에 적절한 **응답 (표현, Representation)** 을 보냄
      - 하나의 자원은 JSON, XML, TEXT, RSS 등 여러 형태의 표현(Representation)으로 나타내어 질 수 있다.
      - 주로 JSON, XML이 사용된다.

    <br/>

  - REST API는 어떤 목적을 달성하기 위해 나왔고 어떤 장점을 가지고 있나요?

    - HTTP의 주요 저자인 로이필딩이 제안하였으며, 당시 웹이 HTTP를 제대로 사용하지 못하고 있어 HTTP의 장점을 최대한 활용할 수 있는 아키텍쳐로 REST를 소개했다.

    - HTTP 프로토콜의 인프라를 그대로 사용하므로 REST API 사용을 위한 별도의 인프라를 구출할 필요가 없다.

    - HTTP 표준 프로토콜에 따르는 모든 플랫폼에서 사용이 가능하다.

    - REST API 메시지가 의도하는 바를 명확하게 나타내므로 의도하는 바를 쉽게 파악할 수 있다.

    - 서버와 클라이언트의 역할을 명확하게 분리한다.

    <br/>

  - RESTful한 API 설계의 단점은 무엇인가요?

    - 표준이 없기 때문에, 좋은(공식화 된) API 디자인 가이드가 존재하지 않는다.

    - HTTP Method 형태가 제한적(4가지)이다.

    - 쉽게 수정 가능하고 단순한 URL구조에 비해 헤더의 구조는 복잡하다.

    <br/>

- CORS란 무엇인가요? 이러한 기능이 왜 필요할까요? CORS는 어떻게 구현될까요?

  - 기존의 웹 브라우저는 보안을 위해 다른 출처의 리소스를 사용하는 것을 제한하는 보안 정책 **SOP(Same-Origin Policy)** 을 취하고 있다.

    - URL의 `Protocol`, `Domain(Host)`, `Port` 이 세가지가 모두 같아야 동일 출처라고 브라우저가 판단한다.
    - 출처가 같아야 서로 접근이 가능하다.

  - 다른 출처간에 리소스를 공유할 수 있는 표준의 방법으로 **CORS(Cross-origin resource sharing)** 가 자리 잡게 되었다.

  ### CORS

  - 추가 HTTP 헤더를 사용하여, 한 출처의 어플리케이션에서 다른 출처의 자원에 접근할 수 있는 권한을 부여하도록 브라우저에 알려주는 체제이다.

  - CORS의 동작 방식은 **단순 요청 방법**과 **예비 요청을 먼저 보내는 방법(preflight)** 2가지 방법이 있다.

  #### 1. **Simple Request**

  ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/ad0037f8-5d3a-4a2e-883f-095c28a5cbca/Untitled.png)

  - 서버에게 바로 요청을 보내는 방법이다.
  - 클라이언트는 아래와 같은 조건을 만족해야 한다.

    - **HTTP Method** : GET, HEAD, POST

    - (수동설정 가능한) **HTTP Header** :

      - Accept
      - Accept-Language
      - Content-Language 응답 헤더
      - Content-Type

    - (가능한) **Content-Type** :
      - application/x-www-form-urlencoded
      - mulitpart/form-data
      - text/plain

  #### 2. **Preflight Request**

  - Simple Request가 아닌 경우 Preflight Request에 해당한다.
  - OPTION method를 통해 다른 도메인의 리소스에 요청이 가능한지 **사전 확인**하는 작업이다.

  - 유효한 Preflight 응답을 받은 후, **실제 요청 (Actual Request)** 를 보낸다.

  #### Why?

  - CORS 설정이 없는 서버 등의 경우 DELETE, PUT과 같은 메소드와 헤더 등 많은 요청에 의해 DB에 영향을 줄 수 있기 때문!
    (브라우저에서는 cors에러가 터지는데, 서버에서는 http response 200 ok로 요청이 받아 질 수도 있다고 한다...)

  ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/407303fe-7fe5-4bf5-8f21-77c5a4c373bd/Untitled.png)

  - 서버 설정을 통해서 **preflight** 결과의 캐시를 일정 기간 동안 저장시킬 수가 있다.

  - 이 캐시 정보가 살아있는 시간 동안은 **cross-origin 요청**에 대해서 **preflight**를 생략하고 바로 요청 전송이 가능하다.

  <br/>

  [개인 notion-CORS](https://bustling-nannyberry-195.notion.site/CORS-Cross-origin-resource-sharing-5f511d11d88a47c88aa1945a4d4c2eb6)

## Quest

- 이번 퀘스트는 Midterm에 해당하는 과제입니다. 분량이 제법 많으니 한 번 기능별로 세부 일정을 정해 보고, 과제 완수 후에 그 일정이 얼마나 지켜졌는지 스스로 한 번 돌아보세요.
  - 이번 퀘스트부터는 skeleton을 제공하지 않습니다!
- Quest 05에서 만든 메모장 시스템을 서버와 연동하는 어플리케이션으로 만들어 보겠습니다.
  - 클라이언트는 `fetch` API를 통해 서버와 통신합니다.
  - 서버는 8000번 포트에 REST API를 엔드포인트로 제공하여, 클라이언트의 요청에 응답합니다.
  - 클라이언트로부터 온 새 파일 저장, 삭제, 다른 이름으로 저장 등의 요청을 받아 서버의 로컬 파일시스템을 통해 저장되어야 합니다.
    - 서버에 어떤 식으로 저장하는 것이 좋을까요?
  - API 서버 외에, 클라이언트를 띄우기 위한 서버가 3000번 포트로 따로 떠서 API 서버와 서로 통신할 수 있어야 합니다.
  - Express나 Fastify 등의 프레임워크를 사용해도 무방합니다.
- 클라이언트 프로젝트와 서버 프로젝트 모두 `npm i`만으로 디펜던시를 설치하고 바로 실행될 수 있게 제출되어야 합니다.
- 이번 퀘스트부터는 앞의 퀘스트의 결과물에 의존적인 경우가 많습니다. 제출 폴더를 직접 만들어 제출해 보세요!

## Advanced

- `fetch` API는 구현할 수 없지만 `XMLHttpRequest`로는 구현할 수 있는 기능이 있을까요?
- REST 이전에는 HTTP API에 어떤 패러다임들이 있었을까요? REST의 대안으로는 어떤 것들이 제시되고 있을까요?
