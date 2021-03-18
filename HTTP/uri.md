# URI

Uniform Resource Identifier

## URI? URL? URN?

- URI는 로케이터(locator), 이름(name) 또는 둘 다 추가로 분류될 수 있다.

URL = URL(Resource Locator) + URN(Resource Name)

### URI

Uniform: 리소스 식별하는 통일된 방식
Resource: 자원, URI로 식별할 수 있는 모든 것(제한 없음)
Identifier: 다른 항목과 구분하는데 필요한 정보

### URL, URN

URL - Locator : 리소스가 있는 위치를 지정
URN - Name : 리소스에 이름을 부여

- 위치는 변할 수 있지만, 이름은 변하지 않는다.

## Example

https://www.google.com/search?q=hello

- 프로토콜(https)
    - 어떤 방식으로 접근할건지 ex) http, https, ftp
- 호스트명(www.google.com)
    - 도메인명, ip 주소를 입력할 수 있음
- port(443)
    - 생략 가능. 생략시 기본 포트는 http(80), https(443)
- 패스(/search)
- query parameter(q=hello&hl=ko)
    - key=value 형태
    - ?로 시작, &로 추가 가능
