# Quest 08. 웹 API의 기초

## Introduction

- 이번 퀘스트에서는 웹 API 서버의 기초를 알아보겠습니다.

## Topics

- HTTP Method
- node.js `http` module
  - `req`와 `res` 객체

## Resources

- [MDN - Content-Type Header](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Content-Type)
- [MDN - HTTP Methods](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods)
- [MDN - MIME Type](https://developer.mozilla.org/en-US/docs/Glossary/MIME_type)
- [Postman](https://chrome.google.com/webstore/detail/postman/fhbjgbiflinjbdggehcddcbncdddomop)
- [HTTP Node.js Manual & Documentation](https://nodejs.org/api/http.html)

## Checklist

- HTTP의 GET과 POST 메소드는 어떻게 다른가요?

  - 다른 HTTP 메소드에는 무엇이 있나요?

    - **GET**
      - 특정 리소스의 표시를 요청한다.
      - 오직 데이터를 받기만 한다.
      - 멱등성 O
    - **POST**
      - 특정 리소스에 엔티티를 제출할 때 쓰인다.
      - 멱등성 X
    - **PUT**
      - 목적 리소스를 요청 payload로 바꾼다.
      - 멱등성 O
    - **PATCH**
      - 리소스의 부분만을 수정한다.
      - 멱등성 O
    - **DELETE**
      - 특정 리소스를 삭제한다.
      - 멱등성 O
    - HEAD
      - GET과 유사하나 응답본문없이 응답 헤더만을 전달 받는다.
      - 요청한 URI 컨텐츠가 웹서버에 존재하는지 여부를 확인할 때 사용된다. (응답헤더의 상태코드 참조)
    - CONNECT

      - 요청한 리소스에 대해 양방향 연결을 시작하는 메소드이다.
      - SSL (HTTPS)를 사용하는 웹사이트에 접속하는데 사용될 수있다.

      - 클라이언트는 원하는 목적지와의 TCP 연결을 HTTP 프록시 서버에 요청 -> 서버는 클라이언트를 대신하여 연결 생성 -> 한번 서버에 의해 연결이 수립되면, 프록시 서버는 클라이언트에 오고가는 TCP 스트림을 계속해서 프록시

    - OPTIONS
      - 웹서버에서 지원하는 HTTP 요청방식을 확인하고자 할 때 사용한다.
      - OPTIONS 요청 메소드와 URI를 웹서버로 전달하면, 웹서버는 Allow 응답헤더에 지원하는 Method들을 나열해 반환한다.
    - TRACE
      - 목적 리소스가 위치한 웹서버로 가는 네트워크의 경로를 체크한다.
      - 웹서버와 클라이언트 사이에 중간 서버(웹 프록시 or 웹 캐시서버)의 존재 여부를 확인할 수 있다.

    <br/>

    - \*멱등성 : 동일한 요청을 한 번 보내는 것과 여러 번 연속으로 보내는 것이 같은 효과를 지니고, 서버의 상태도 동일하게 남을 때 멱등성을 가진다고 한다.

    <br/>

- HTTP 서버에 GET과 POST를 통해 데이터를 보내려면 어떻게 해야 하나요?

  - HTTP 요청의 `Content-Type` 헤더는 무엇인가요?

    - 리소스의 media type을 나타내기 위해 사용된다.

    <br/>

  - Postman에서 POST 요청을 보내는 여러 가지 방법(`form-data`, `x-www-form-urlencoded`, `raw`, `binary`) 각각은 어떤 용도를 가지고 있나요?

    - `form-data`

      - multipart/form-data
      - `key1=value1&key2=value2`의 형태로 전송된다.
      - 파일 첨부가 가능하다.

    - `x-www-form-urlencoded`

      - `key1=value1&key2=value2`의 형태로 전송된다.
      - 양식데이터인 `form-data`와 유사하나 URL이 x-www-form-urlencode를 통해 전송 될 때 인코딩 되는 차이가 있다.
      - 전송하는 데이터가 다른 문자로 인코딩 되면 중간에서 공격을 받아도 인식 할 수 없다는 장점이 있다.

    - `raw`

      - 본문 데이터를 사용하여 텍스트로 입력할 수 있는 모든 것을 보낼 수 있다.
      - text , javascript , JSON , HTML 또는 XML을 선택히여 작성한다.

    - `binary`
      - 바이너리 데이터를 사용 하여 이미지, 오디오 및 비디오 파일(텍스트 파일도 보낼 수 있음)과 같이 요청 본문과 함께 Postman 편집기에 수동으로 입력할 수 없는 정보를 보낼 수 있다.

- node.js의 `http` 모듈을 통해 HTTP 요청을 처리할 때,

  - `req`와 `res` 객체에는 어떤 정보가 담겨있을까요?

    - **req 객체**

      - req.body : POST 정보를 가진다. 요청 정보가 url에 들어있는 것이 아니라 Request의 본문에 들어있는 형태이다.

      - req.query : GET 정보를 가진다. 즉, url로 전송된 쿼리 스트링 파라미터를 담고 있다.

      - req.params : 내가 이름 붙인 라우트 파라미터 정보를 가진다.

      - req.headers : HTTP의 Header 정보를 가진다.

      * 이 외에도 req.route, req.cookies, req.acceptedLanguages, req.url, req.protocol, req.host, req.ip 등이 존재

      <br/>

    - **res 객체**

      - res.send : 다양한 유형의 응답을 전송한다.

      - res.redirect : 브라우저를 리다이렉트 한다.

      - res.render : 설정된 템플릿 엔진을 사용해서 views를 렌더링한다.

      - res.json : JSON 응답을 전송한다.

      - res.end : 응답 프로세스를 종료한다.

      * 이 외에도 res.set, res.status, res.type, res.sendFile, res.links, res.cookie 등이 존재한다.

  - GET과 POST에 대한 처리 형태가 달라지는 이유는 무엇인가요?

- 만약 API 엔드포인트(URL)가 아주 많다고 한다면, HTTP POST 요청의 `Content-Type` 헤더에 따라 다른 방식으로 동작하는 서버를 어떻게 정리하면 좋을까요?
  - 그 밖에 서버가 요청들에 따라 공통적으로 처리하는 일에는 무엇이 있을까요? 이를 어떻게 정리하면 좋을까요?

## Quest

- 다음의 동작을 하는 서버를 만들어 보세요.
  - 브라우저의 주소창에 `http://localhost:8080`을 치면 `Hello World!`를 응답하여 브라우저에 출력합니다.
  - 서버의 `/foo` URL에 `bar` 변수로 임의의 문자열을 GET 메소드로 보내면, `Hello, [문자열]`을 출력합니다.
  - 서버의 `/foo` URL에 `bar` 키에 임의의 문자열 값을 갖는 JSON 객체를 POST 메소드로 보내면, `Hello, [문자열]`을 출력합니다.
  - 서버의 `/pic/upload` URL에 그림 파일을 POST 하면 서버에 보안상 적절한 방법으로 파일이 업로드 됩니다.
  - 서버의 `/pic/show` URL을 GET 하면 브라우저에 위에 업로드한 그림이 뜹니다.
  - 서버의 `/pic/download` URL을 GET 하면 브라우저에 위에 업로드한 그림이 `pic.jpg`라는 이름으로 다운로드 됩니다.
- expressJS와 같은 외부 프레임워크를 사용하지 않고, node.js의 기본 모듈만을 사용해서 만들어 보세요.
- 처리하는 요청의 종류에 따라 공통적으로 나타나는 코드를 정리해 보세요.

## Advanced

- 서버가 파일 업로드를 지원할 때 보안상 주의할 점에는 무엇이 있을까요?
