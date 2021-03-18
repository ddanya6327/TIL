# HTTP

HyperText Transfer Protocol

현재, 모든 것을 HTTP 메세지에 담아 전송 할 수 있다.

- HTML 뿐만 아니라 TEXT, Image, JSON, XML 등 거의 모든 것을 HTTP로 전송 가능.

## 기반 프로토콜

- TCP: HTTP/1.1, HTTP/2
- UDP: HTTP/3

현재 HTTP/1.1 을 가장 많이 사용하지만, HTTP/2, HTTP/3도 점점 증가.

## HTTP 특징

- 클라이언트 서버 구조
- 무상태 프로토콜(스테이스리스), 비연결성
- HTTP 메시지
- 단순, 확장 가능

## 무상태 프로토콜(Stateless)

서버에서 상태를 보관하지 않으므로, Stateless의 경우 고객이 서버1과 요청을 주고 받다가 서버1이 고장나도 서버2와 통신하면 된다. 상태 유지(Stateful)의 경우는 서버1과 요청을 주고 받다가 서버1이 고장나면 다시 처음부터 서버2와 내용을 주고 받아야 함.

- 상태 유지(Stateful)
    - 항상 같은 서버가 유지되어야 한다.
- 무상태(Stateless)
    - 아무 서버나 호출해도 된다.
    - 갑자기 클라이언트 요청이 증가해도 서버를 증설할 수 있음.

상태 유지는 최소한으로 사용하는게 좋다.
- ex) 로그인

단, Stateless의 경우 데이터 량이 많을 수 밖에 없다.

## 비 연결성(connectionless)

- HTTP는 기본으로 연결을 유지하지 않음.
- 서버 입장에서 클라이언트와 연결을 유지하지 않음으로써 최소한의 자원을 사용할 수 있음.

### 단점
- TCP/IP 연결을 새로 맺어야 함. (3 way handshake 시간 추가)
- JavaScirpt, CSS, Image 등 자원이 계속 다운로드 됨.

현재는 HTTP 지속 연결(Presistent Connections)로 문제 해결

## Stateless의 활용
- 같은 시간에 동시에 발생하는 이벤트 (ex. 선착순)같읕 경우 Stateless 한 설계로 요청을 받는다면 서버만 늘리면 되므로 대응 할 수 있음.

# HTTP Message

HTTP 메세지의 구조
```
start-line (시작 라인)
header (헤더)
emptry line (공백 라인)
message body
```

HTTP 요청 메세지 Example
```
GET /search?q=hello HTTP/1.1
Host: www.google.com
(empty space) <- 필수
(body)
```


HTTP 응답 메세지 Example
```
HTTP/1.1 200 OK
Content-Type: text/html; charset=UTF-8
Content-Length: 123
(empty space) <- 필수
<html>
  ...
</html>
```

## start line

- start-line은 `request-line / status-line` 으로 이루어 짐.

## request line
- request-line
    - HTTP Method
        - ex) GET, POST, PUT, DELETE
    - request-target
        - 요청 대상
        - 절대경로(/) 로 시작하는 경로(*, http 로 시작하는 경우도 있음.)
    - HTTP Version
ex) `GET /search?q=hello&hl=ko HTTP/1.1 CRLF(Enter)`

## status line
- status-line
    - HTTP Version
    - status-code
        - ex) 200, 400, 500
    - reason-phrase
        - 이유
ex)
```
HTTP/1.1 200 OK
Content-Type: text/html; charset=UTF-8
Content-Length: 3423

<html>
    <body>...</body>
</html>
```

## HTTP Header
- HTTP Header
    - `field-name ":" field-value` 로 구성
    - field-name은 대소문자 구문 없음
    - HTTP 전송에 필요한 모든 부가정보
    - 필요시 임의의 헤더 추가 가능
ex)
```
Content-Type: text/html;charset=UTF-8
Content-Length: 123
```

## HTTP Message Body
- HTTP Message Body
    - 실제 전송할 데이터
        - HTML, Image, Video, JSON 등등

