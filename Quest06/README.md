# Quest 06. 인터넷의 이해

## Introduction

- 이번 퀘스트에서는 인터넷이 어떻게 동작하며, 서버와 클라이언트, 웹 브라우저 등의 역할은 무엇인지 알아보겠습니다.

## Topics

- 서버와 클라이언트, 그리고 웹 브라우저
- 인터넷을 구성하는 여러 가지 프로토콜
  - IP
  - TCP
  - HTTP
- DNS

## Resources

- [OSI 모형](https://ko.wikipedia.org/wiki/OSI_%EB%AA%A8%ED%98%95)
- [IP](https://ko.wikipedia.org/wiki/%EC%9D%B8%ED%84%B0%EB%84%B7_%ED%94%84%EB%A1%9C%ED%86%A0%EC%BD%9C)
  - [Online service Traceroute](http://ping.eu/traceroute/)
- [TCP](https://ko.wikipedia.org/wiki/%EC%A0%84%EC%86%A1_%EC%A0%9C%EC%96%B4_%ED%94%84%EB%A1%9C%ED%86%A0%EC%BD%9C)
  - [Wireshark](https://www.wireshark.org/download.html)
- [HTTP](https://ko.wikipedia.org/wiki/HTTP)
  - Chrome developer tool, Network tab
- [DNS](https://ko.wikipedia.org/wiki/%EB%8F%84%EB%A9%94%EC%9D%B8_%EB%84%A4%EC%9E%84_%EC%8B%9C%EC%8A%A4%ED%85%9C)
  - [Web-based Dig](http://networking.ringofsaturn.com/Tools/dig.php)

## Checklist

- 인터넷은 어떻게 동작하나요? Internet Protocol Suite의 레이어 모델에 입각하여 설명해 보세요.
  - 근거리에서 서로 떨어진 두 전자기기가 유선/무선으로 서로 통신하는 프로토콜은 어떻게 동작할까요?
  - 근거리에 있는 여러 대의 전자기기가 서로 통신하는 프로토콜은 어떻게 동작할까요?
  - 아주 멀리 떨어져 있는 두 전자기기가 유선/무선으로 서로 통신하는 프로토콜은 어떻게 동작할까요?
  - 두 전자기기가 신뢰성을 가지고 통신할 수 있도록 하기 위한 프로토콜은 어떻게 동작할까요?
  - HTTP는 어떻게 동작할까요?
- 우리가 브라우저의 주소 창에 www.knowre.com 을 쳤을 때, 어떤 과정을 통해 서버의 IP 주소를 알게 될까요?

  **1. 브라우저의 URL 파싱**

  - 어떤 프로토콜을 통해 해당 URL에 요청할 것인지
  - 어떤 URL로 요청할 것인지
  - 어떤 포트로 요청할 것인지

    - HTTP라면 80 포트를, HTTPS라면 443 포트를 기본 값으로 요청

    <br/>

  **2. HSTS(HTTP Strict transport security) 목록 조회**

  - HSTS : HTTP를 허용하지 않고 HTTPS를 사용하는 연결만 허용하는 기능
  - 만약 HTTP로 요청이 왔다면 HTTP 응답 헤더에 "Strict Transport Security"라는 필드를 포함하여 응답하고 이를 확인한 브라우저는 해당 서버에 요청할 때 HTTPS만을 통해 통신
  - 자신의 HSTS캐시에 해당 URL을 저장
  - HSTS목록에 해당 URL이 존재한다면 명시적으로 HTTP를 통해 요청한다 해도 브라우저가 이를 HTTPS로 요청

    <br/>

  **3. URL을 IP주소로 변환**

  - 우선 브라우저에서는 자신의 로컬 hosts 파일과 브라우저 캐시에 해당 URL이 존재하는지 확인
  - 존재하지 않을 시 도메인 주소를 IP주소로 변환해주는 DNS(Domain Name System) 서버에 요청하여 해당 URL을 IP주소로 변환

    <br/>

  **4. 라우터를 통해 해당 서버의 게이트웨이까지 이동**

  - DNS서버에게 받은 IP주소를 가지고 해당 서버로 요청 전송
  - 전송 시 해당 요청이 목적지인 IP주소까지 도착하는 네트워크 상 경로는 알 수 없으며, 라우터의 라우팅을 통해 해당 요청이 어떤 경로로 가야할 지 지정 됨

  <br/>

  **5. ARP를 통해 IP주소를 MAC주소로 변환**

  - 논리 주소인 IP주소를 물리 주소인 MAC 주소로 변환해야 실질적인 통신이 가능
  - 해당 IP주소를 가지고 있는 노드는 자신의 MAC 주소를 응답

  <br/>

  **6. 대상 서버와 TCP 소켓 연결**

  - 대상 서버와 통신하기 위해 **TCP 소켓 연결**을 진행
  - 소켓 연결은 **3-way-handshake**라는 과정을 통해 이루어짐
  - 현재는 HTTPS 요청이므로, 서로 암호화 통신을 위한 TLS 핸드쉐이킹이 추가 됨
  - 이를 통해 서버와 클라이언트는 암호화 통신이 가능

  <br/>

  **7. HTTP(HTTPS) 프로토콜로 요청, 응답**

  - 연결이 확정되었으니, www.knowre.com 해당 페이지에 대한 데이터를 요청
  - 서버에서 해당 요청을 받은 후, 요청을 수락할 수 있는지 확인, 요청에 대한 응답을 생성하여 브라우저에게 전달

  <br/>

  **8. 브라우저에서 응답을 해석**

  - 응답 받은 HTML, CSS, JS 등의 파일들을 브라우저 엔진을 통해 해석되고, 화면에 렌더링되어 나타나게됨  
    <br/>

## Quest

- tracert(Windows가 아닌 경우 traceroute) 명령을 통해 www.google.com 까지 가는 경로를 찾아 보세요.
  - 어떤 IP주소들이 있나요?
  - 그 IP주소들은 어디에 위치해 있나요?
- Wireshark를 통해 www.google.com 으로 요청을 날렸을 떄 어떤 TCP 패킷이 오가는지 확인해 보세요
  - TCP 패킷을 주고받는 과정은 어떻게 되나요?
  - 각각의 패킷에 어떤 정보들이 담겨 있나요?
- telnet 명령을 통해 http://www.google.com/ URL에 HTTP 요청을 날려 보세요.
  - 어떤 헤더들이 있나요?
  - 그 헤더들은 어떤 역할을 하나요?

## Advanced

- HTTP의 최신 버전인 HTTP/3는 어떤 식으로 구성되어 있을까요?
- TCP/IP 외에 전세계적인 네트워크를 구성하기 위한 다른 방식도 제안된 바 있을까요?
