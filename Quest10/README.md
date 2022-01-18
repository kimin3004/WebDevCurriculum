# Quest 10. 인증의 이해

## Introduction

- 이번 퀘스트에서는 웹에서의 인증에 관해 알아보겠습니다.

## Topics

- Cookie
- Session
- JWT

## Resources

- [MDN - HTTP 쿠키](https://developer.mozilla.org/ko/docs/Web/HTTP/Cookies)
- [Cookies and Sessions](https://web.stanford.edu/~ouster/cgi-bin/cs142-fall10/lecture.php?topic=cookie)
- [JWT](https://jwt.io/)

## Checklist

- 쿠키란 무엇일까요?

  - Http 쿠키, 웹 쿠키, 브라우저 쿠키로도 불리운다.
  - 서버가 사용자의 웹 브라우저에 전송하는 작은 데이터 조각이다.
  - 브라우저는 쿠키를 저장해 놓았다가, 동일한 서버에 재 요청 시 저장된 쿠키를 함께 전송한다.
  - 이를 통해 **서버는 두 요청이 동일한 브라우저에서 들어왔는지 아닌지 판단** 할 수 있다.

    - HTTP는 stateless하기 때문에, 클라이언트들의 각 요청은 독립적이다. 서버는 동일한 브라우저 또는 클라이언트로부터 2개의 요청이 왔는지 알 수 없다. 결국 쿠키는 **stateless 한 HTTP 통신에서 클라이언트에게 정보(표시)를 주어 해당 클라이언트를 식별하기 위해 만들어 진 것이다.**

    <br/>

  **쿠키가 주로 사용되는 목적**

  1. **세션 관리(Session management)**

  - 서버에 저장해야 할 로그인, 장바구니, 게임 스코어 등의 정보 관리

  2. **개인화(Personalization)**

  - 사용자 선호, 테마 등의 세팅

  3. **트래킹(Tracking)**

  - 사용자 행동을 기록하고 분석하는 용도

    <br/>

  **쿠키의 특징**

  - 쿠키에는 이름, 값, 만료날짜, 도메인 정보가 들어있다.

    - 브라우저에 따라 데이터 크기가 제한된다. (일반적으로 < 4KB)
    - 서버는 이름이 다른 여러 쿠키를 정의할 수 있지만 브라우저는 서버당 쿠키 수(약 50개)를 제한한다.

  - 기본적으로 쿠키는 웹 브라우저가 종료되면 삭제된다. ( 만료날짜를 지정해 주면 만료일이 되야 삭제된다.)

  - 웹 브라우저에 해당 도메인의 쿠키 정보가 있으면 HTTP 요청 (HTTP 헤더의 Cookie)에 무조건 담아 보낸다.
  - 해당 도메인과 일치하는 요청에만 포함 -> 서버, 포트(선택 사항), URL 접두사(선택 사항)

  <br/>

- 쿠키는 어떤 식으로 동작하나요?

  - Http 통신에서 이루어 지는 것이기 때문에, Http요청과 응답에 따라 작동하게 된다.

    ![https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fpf1N6%2Fbtqv8ZCLXgy%2FcHTFZxnBa2mOw8wPGf6pM0%2Fimg.png](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fpf1N6%2Fbtqv8ZCLXgy%2FcHTFZxnBa2mOw8wPGf6pM0%2Fimg.png)

  - 클라이언트(웹 브라우저)가 특정 서버에 처음 연결할 때에는 쿠키가 없는 상태로 요청을 보내게 된다.
  - 요청을 받은 서버에서 쿠키를 클라이언트(웹 브라우저)로 보낸다. (`쿠키를 정의하는 Set-Cookie: 헤더가 포함 됨`)
  - 클라이언트는 받은 쿠키를 도메인 서버 이름으로 정렬된 쿠키 디렉토리에 쿠키 정보를 저장하게 된다.
  - 이후 클라이언트가 동일한 서버로 HTTP 요청을 보내면 저장된 쿠키도 같이 전송된다. (`브라우저가 동일한 서버에 연결할 때마다 서버가 관련 요청을 연결하는 데 사용할 수 있는 이름과 값이 포함된 Cookie: 헤더가 포함 됨`)
  - 만약 서버에서 쿠키에 업데이트된 내용이 있으면 응답할 때 다시 업데이트된 쿠키를 보내준다.

    <br/>

- 쿠키는 어떤 식으로 서버와 클라이언트 사이에 정보를 주고받나요?

* 웹 어플리케이션의 세션이란 무엇일까요?

  - 서버에 정보를 저장하고, 세션 쿠키를 통해 클라이언트를 식별하는 방식이다.

  - 세션의 ID와 내용은 각각 어디에 저장되고 어떻게 서버와 교환되나요?

    ![https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FvaGpm%2Fbtqv8md80Kr%2FPXhH7cgDMNpl6lhmKeriSK%2Fimg.png](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FvaGpm%2Fbtqv8md80Kr%2FPXhH7cgDMNpl6lhmKeriSK%2Fimg.png)

    - 클라이언트가 서버에 요청을 보내면 서버에서는 요청헤더( Cookie)를 확인하고 세션 ID가 있는지 확인한다.

    - 만약 요청에 세션 ID가 없다면, 서버에서는 새 세션 객체를 생성하여, 응답을 보낼 때 세션 ID를 쿠키에 담아 보내게 된다.

    - 클라이언트는 받은 세션 쿠키(세선 ID 값)을 저장해 두고, 매번 해당 도메인 서버에 요청을 보낼 때 마다 세션 쿠키를 함께 보내 자신이 누구인지 인증하게 된다.

    - 세션쿠키는 브라우저가 종료되면 삭제된다.

  **장단점**

  - 쿠키를 포함한 요청이 외부에 노출되더라도 세션 ID 자체는 유의미한 개인정보를 담고 있지 않는다. 그러나 해커가 이를 중간에 탈취하여 클라이언트인척 위장할 수 있다는 한계가 존재한다.

  - 서버에서 세션 저장소를 사용하므로 요청이 많아지면 서버에 부하가 심해진다.

    <br/>

- JWT란 무엇인가요?

  - 인증에 필요한 정보들을 암호화시킨 토큰(JSON Web Token)이다.

  - 클라이언트와 서버, 서비스와 서비스 사이 통신 시 권한 인가(Authorization)를 위해 사용된다.

  - 자가 수용적 (self-contained)이다.

    - 필요한 모든 정보를 자체적으로 지니고 있다.
    - 토큰에 대한 기본정보, 전달할 정보 (로그인시스템에서는 유저 정보) 그리고 토큰이 검증됐다는것을 증명해주는 signature 를 포함하고 있다.

    `HEADER.PAYLOAD.SIGNATURE`

    <br/>

  - JWT 토큰은 어디에 저장되고 어떻게 서버와 교환되나요?

    - 쿠키/세션 방식과 유사하게 JWT 토큰(Access Token)을 HTTP 헤더에 실어 서버가 클라이언트를 식별한다.

    ```
    {
      Authorization: <type> <access-token>
    }
    ```

    1. 클라이언트에서 로그인 요청을 보내면, 서버는 검증 후 클라이언트 고유 ID 등의 정보를 Payload에 담아 보낸다.

    2. 암호화할 비밀키를 사용해 Access Token(JWT)을 발급한다.

    3. 클라이언트는 전달받은 토큰을 저장해두고, 서버에 요청할 때 마다 토큰을 요청 헤더 Authorization에 포함시켜 함께 전달한다.

    4. 서버는 토큰의 Signature를 비밀키로 복호화한 다음, 위변조 여부 및 유효 기간 등을 확인한다.

    5. 유효한 토큰이라면 요청에 응답한다.

- 세션에 비해 JWT가 가지는 장점은 무엇인가요? 또 JWT에 비해 세션이 가지는 장점은 무엇인가요?

  - JWT 방식은 인증정보 저장을 위한 별도의 저장소가 필요없다는 장점이 있다. 하지만 토큰의 길이가 늘어날 수록 네트워크 부하가 발생할 수 있으며, 한 번 Token을 발급해 주면 해당 Token에 대한 통제가 어렵다는 단점이 있다.
    마지막 단점을 보완하기 위해 Access Token과 Refresh Token을 따로 발급하여 Access Token의 만료 기간을 짧게 설정하고, Refresh Token을 통해서만 재발급이 가능하게 하는 전략을 사용할 수 있다.

    - 서버는 DB에 저장된 Refresh Token과 비교하여 유효한 경우 새로운 Access Token을 발급하고, 만료된 경우 사용자에게 로그인을 요구하게 된다.
    - 또한 서버가 강제로 Refresh Token을 만료시킬 수도 있다.
    - 검증을 위해 서버가 Refresh Token을 별도의 storage에 저장해야 한다는 비용이 발생한다.

  <br/>

  - 그에 반해 Cookie&Session 방식은 서버 쪽에서 Session을 통제하기 더 수월하며 네트워크 부하가 낮은 장점이 있으나, 서버 저장소 사용으로 인한 서버 부하가 발생한다는 단점이 있다.

## Quest

- 이번에는 메모장 시스템에 로그인 기능을 넣고자 합니다.
  - 사용자는 딱 세 명만 존재한다고 가정하고, 아이디와 비밀번호, 사용자의 닉네임은 하드코딩해도 무방합니다.
  - 로그인했을 때 해당 사용자가 이전에 작업했던 탭들과 마지막으로 활성화된 탭 등의 상태가 로딩 되어야 합니다.
  - 세션을 이용한 버전과, JWT를 이용한 버전 두 가지를 만들어 보세요!
    - 세션을 이용할 경우 세션은 서버의 메모리나 파일에 저장하면 됩니다.

## Advanced

- Web Authentication API(WebAuthn)은 무엇인가요?
