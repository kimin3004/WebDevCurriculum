# Quest 12. 보안의 기초

## Introduction

- 이번 퀘스트에서는 가장 기초적인 웹 서비스 보안에 대해 알아보겠습니다.

## Topics

- XSS, CSRF, SQL Injection
- HTTPS, TLS

## Resources

- [The Basics of Web Application Security](https://martinfowler.com/articles/web-security-basics.html)
- [Website Security 101](https://spyrestudios.com/web-security-101/)
- [Web Security Fundamentals](https://www.shopify.com.ng/partners/blog/web-security-2018)
- [OWASP Cheat Sheet Series](https://cheatsheetseries.owasp.org/)
- [Wikipedia - TLS](https://en.wikipedia.org/wiki/Transport_Layer_Security)

## Checklist

- 입력 데이터의 Validation을 웹 프론트엔드에서 했더라도 서버에서 또 해야 할까요? 그 이유는 무엇일까요?

  - validation이라는 개념은 데이터의 유효성/ 신뢰도/ 위변조 여부 등 매우 넓은 범위를 포괄하며, 이는 중요도에 따라 클라이언트 - 서버측에서 나누어 처리하도록 분할할 수 있다.

  - 또한 프론트의 검증코드는 회피할 가능성이 있기 때문에 반드시 서버측의 validation은 이루어져야 한다.

  구현 방법으로는 크게 아래 두가지가 있을 수 있다.

  1. 프론트와 백엔드 양쪽에 모두 검증 코드를 작성한다.
     - 서버에 모든 값을 바로 보내보지 않고 검증을 통해 빠른 UX를 제공 해 줄 수 있고, 서버가 값 검증을 해야하는 서버 리소스도 줄여 줄 수 있다.
  2. 백엔드에 검증 코드를 작성 후, 백엔드 결과에 따라 프론트는 메세지만 노출한다.

  <br/>

- 서버로부터 받은 HTML 내용을 그대로 검증 없이 프론트엔드에 innerHTML 등을 통해 적용하면 어떤 문제점이 있을까요?
- XSS(Cross-site scripting)이란 어떤 공격기법일까요?

  - 악의적인 사용자가 공격하려는 사이트에 스크립트를 넣는 기법이다.
  - 공격에 성공하면 사이트에 접속한 사용자는 삽입된 코드를 실행하게 되며, 보통 의도치 않은 행동을 수행시키거나 쿠키나 세션 토큰 등의 민감한 정보를 탈취한다.
  - 공격 방법에 따라 Stored XSS와 Reflected XSS로 나뉜다.

  1. Stored XSS
     ![https://media.vlpt.us/images/gyrbs22/post/d6a700bc-0e09-4898-8d31-a3b943de2418/image.png](https://media.vlpt.us/images/gyrbs22/post/d6a700bc-0e09-4898-8d31-a3b943de2418/image.png)

  - 사이트 게시판이나 댓글, 닉네임 등 스크립트가 서버에 저장되어 실행되는 방식이다.
  - server side에서 오는 data를 validate해야 하는 이유이다.

    <br/>

  2. Reflected XSS
     ![https://media.vlpt.us/images/gyrbs22/post/b49256f6-72e4-4153-976d-d0ecde687945/image.png](https://media.vlpt.us/images/gyrbs22/post/b49256f6-72e4-4153-976d-d0ecde687945/image.png)

     - URL parameter(querystring)에 행위 logic을 넣어 공격하는 방법이다.
     - 브라우저 자체에서 차단하는 경우가 많아 상대적으로 공격을 성공시키기 어렵다.
     - 보통 GET method 상에 script 코드를 넣고, 해당 URL(링크)를 접속하도록 유도하여 해당 script를 실행하도록 한다.

- CSRF(Cross-site request forgery)이란 어떤 공격기법일까요?

  ![https://www.imperva.com/learn/wp-content/uploads/sites/13/2019/01/csrf-cross-site-request-forgery.png](https://www.imperva.com/learn/wp-content/uploads/sites/13/2019/01/csrf-cross-site-request-forgery.png)

  - 사이트 간 요청 위조
  - 사용자가 자신의 의지와 무관하게 공격자가 의도한 행동을 해서 특정 웹페이지를 보안에 취약하게 한다거나 수정, 삭제 등의 작업을 하게 만드는 공격 방법이다.

    - 공격요청이 삽입된 링크를 사용자가 열 경우, 웹 서버가 이를 그대로 받아들여 악의적인 공격에 노출되는 경우 등

  - 두 공격 모두 웹브라우저를 통해 이루어진다는 점에서는 동일하다.

  - 그러나 XSS는 CSRF와 달리 server-side script를 통한 공격이 이루어지기 때문에, 사용자의 로그인없이도 그대로 피해에 노출될 수 있다.

  - 또한 CSRF는 client의 request를 이용하여 web server를 공격하는 방식이므로 공격방식이 상대적으로 어렵고, 이미 인증된 session 등을 악용한다는 점에서 차이가 있다.

- SQL Injection이란 어떤 공격기법일까요?

  ![https://ww.namu.la/s/c9f0d02cb20c312251aa14650d0cfdabb3a32e483b2526e78b1eb2c39f2b83a1da07371f351aef835a48bcd4a0c8235064d11747d19ecef994e2cf74c1415bc130d513161081977eeda75b2aea174a2aec0deb52349e8263718cd7025c3bbd7f](https://ww.namu.la/s/c9f0d02cb20c312251aa14650d0cfdabb3a32e483b2526e78b1eb2c39f2b83a1da07371f351aef835a48bcd4a0c8235064d11747d19ecef994e2cf74c1415bc130d513161081977eeda75b2aea174a2aec0deb52349e8263718cd7025c3bbd7f)

  - 코드 인젝션의 한 기법으로 클라이언트의 입력값을 조작하여 서버의 데이터베이스를 공격할 수 있는 공격방식이다.
  - 주로 사용자가 입력한 데이터를 제대로 필터링, 이스케이핑하지 못했을 경우에 발생한다.
  - 추천되는 방어법은 클라이언트 측의 입력을 받을 웹 사이트에서 자바스크립트로 폼 입력값을 한 번 검증하고, 서버 측은 클라이언트 측의 자바스크립트 필터가 없다고 가정하고한 번 더 입력값을 필터한다. 이때 정규표현식등으로 한번 걸러내고, SQL 쿼리로 넘길 때 해당 파라미터를 prepared statement로 입력받고 이를 프로시저로 처리하는게 좋다. 다음, 쿼리의 출력값을 한 번 더 필터하고(XSS 공격 방어의 목적이 강하다) 유저에게 전송한다. 이렇게 하면 해당 폼에 대해서는 SQL injection 공격이 완전히 차단된다.

  <br/>

- 대부분의 최신 브라우저에서는 HTTP 대신 HTTPS가 권장됩니다. 이유가 무엇일까요?

  - HTTPS (HTTP Secure) 는 HTTP protocol의 암호화된 버전이다.
  - 클라이언트와 서버 간의 모든 커뮤니케이션을 암호화 하기 위하여 SSL 이나 TLS을 사용한다.
    - 소켓 통신에서 일반 텍스트를 이용하는 대신 SSL이나 TLS 프로토콜을 통해 세션 데이터를 암호화
    - 클라이언트가 민감한 정보를 서버와 안전하게 주고받도록 해준다. (금융활동 이나 온라인 쇼핑 등)
  - HTTPS를 사용하는 웹페이지의 URI는 'http://'대신 'https://'로 시작한다.
  - 기본 포트는 80번이 아닌 443번을 쓴다.

  - HTTPS와 TLS는 어떤 식으로 동작하나요? HTTPS는 어떤 역사를 가지고 있나요?
    ![https://media.vlpt.us/images/gyrbs22/post/a7841e71-bb92-434a-a864-dec5e74145a2/image.png](https://media.vlpt.us/images/gyrbs22/post/a7841e71-bb92-434a-a864-dec5e74145a2/image.png)

    - TLS(Transport Layer Security) : SSL(Secure Sockets Layer)에 기반한 기술로, 인터넷에서의 정보를 암호화해서 송수신하는 프로토콜이다. - TLS는 다양한 종류의 보안 통신을 하기 위한 프로토콜이며, HTTPS는 TLS 위에 HTTP 프로토콜을 얹어 보안된 HTTP 통신을 하는 프로토콜이다.

    - SSL 통신과정
      ![https://media.vlpt.us/images/gyrbs22/post/fb608b62-50af-46db-8e5c-c0d15d575ec9/image.png](https://media.vlpt.us/images/gyrbs22/post/fb608b62-50af-46db-8e5c-c0d15d575ec9/image.png)

      1. 먼저 클라이언트에서 서버에 `ClientHello` 메시지를 보낸다. 여기에는 클라이언트에서 가능한 **TLS 버전, 서버 도메인, 세션 식별자, 암호 설정** 등의 정보가 포함된다.

      2. 클라이언트의 메시지를 받은 서버는 `ServerHello` 메시지를 클라이언트에게 보낸다. 여기에는 ClientHello 메시지의 정보 중 **서버에서 사용하기로 선택한 TLS 버전, 세션 식별자, 암호 설정** 등의 정보가 포함된다.

      3. 서버가 클라이언트에 Certificate 메시지를 보낸다. 여기에는 **서버의 인증서**가 들어간다. 이 인증서는 별도의 인증 기관에서 발급받은 것이며, 서버가 신뢰할 수 있는 자임을 인증한다. 전송이 끝나면 `ServerHelloDone` 메시지를 보내 끝났음을 알린다.

      4. 클라이언트는 **서버에서 받은 인증서를 검증**한다. 인증서의 유효 기간이 만료되지 않았는지, 그 인증서가 해당 서버에게 발급된 인증서가 맞는지 등을 확인한다. 인증서를 신뢰할 수 있다고 판단하였다면 다음 단계로 넘어간다.

      5. 클라이언트는 임의의 `pre-master secret`을 생성한 뒤, 서버가 보낸 인증서에 포함된 **공개 키를 사용해 암호화**한다. 이렇게 암호화된 `pre-master secret`을 `ClientKeyExchange` 메시지에 포함시켜 서버에 전송한다.[

      6. 서버는 전송받은 정보를 **복호화**하여 `pre-master secret`을 알아낸 뒤, 이 정보를 사용해 `master secret`을 생성한다. 그 뒤 `master secret`에서 **세션 키를 생성**해내며, 이 세션 키는 앞으로 서버와 클라이언트 간의 통신을 암호화하는데 사용될 것이다. 물론 클라이언트 역시 자신이 만들어낸 `pre-master secret`을 알고 있으므로, 같은 과정을 거쳐 **세션 키를 스스로 생성**할 수 있다.

      7. 이제 서버와 클라이언트는 **각자 동일한 세션 키**를 가지고 있으며, 이 키를 사용해 **대칭키 암호를 사용하는 통신**을 할 수 있다. 따라서 우선 서로에게 `ChangeCipherSpec` 메시지를 보내 앞으로의 모든 통신 내용은 세션 키를 사용해 암호화해 보낼 것을 알려준 뒤, `Finished` 메시지를 보내 각자의 핸드셰이킹 과정이 끝났음을 알린다.

      8. 이제 서버와 클라이언트 간에 보안 통신이 구성된다.

      → 통신이 완전히 종료된 이후엔 대칭키는 폐지한다.

  - HTTPS의 서비스 과정에서 인증서는 어떤 역할을 할까요? 인증서는 어떤 체계로 되어 있을까요?

## Quest

- 메모장의 서버와 클라이언트에 대해, 로컬에서 발행한 인증서를 통해 HTTPS 서비스를 해 보세요.

## Advanced

- TLS의 인증서에 쓰이는 암호화 알고리즘은 어떤 종류가 있을까요?
- HTTP/3은 기존 버전과 어떻게 다를까요? HTTP의 버전 3이 나오게 된 이유는 무엇일까요?
