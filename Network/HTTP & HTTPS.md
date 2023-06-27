# HTTP & HTTPS

## HTTP

### HTTP(Hypertext Transfer Protocol)란?
> 서로 다른 시스템들 사이에 통신을 주고 받게 해주는 가장 기본적인 프로토콜
- 웹 서핑을 할 때 서버에서 자신의 브라우저(클라이언트)로 데이터를 전송해주는 용도로 많이 사용되며, 서버-클라이언트 모델에 맞춰 데이터를 주고 받기 위한 프로토콜이다.
- 인터넷 초기에 모든 웹 사이트에서 기본적으로 사용되었던 프로토콜이었다.
- 상태를 가지고 있지 않은 `Stateless` 프로토콜이다.
- `80번 포트`를 기본적으로 사용하고 있다.
- HTTP 기본적으로 `요청/응답(request/response)` 구조로 되어있다.
.

### HTTP의 기본 구조
<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FSBdjE%2FbtqRjfzXczu%2F7JmzAWw6i0BdlWKTn0k9g1%2Fimg.png" weight=500px height=250px>

.

### HTTP Request 구조
<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fx30TZ%2FbtqRrPzkJe5%2FLEOl80r5wrXA1cr0E62CRK%2Fimg.png" weight=500px height=250px>

#### startline
<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FceDXqo%2FbtqRnNpsXqq%2FEZaWTj2BejpS4AmNx0Qixk%2Fimg.png" weight=500px height=250px>

- HTTP Method - 요청시 보내는 HTTP 메소드 형태. (GET, POST, PUT, PATCH, DELETE, 기타)
- Request Target - 어디로 보내는지에 대한 URI.
- HTTP Version - HTTP 버전으로 현재까지는 대부분이 HTTP/1.1이고 HTTP/2, HTTP/3 까지도 있다.

#### header
<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FckWqjf%2FbtqRjf7T8bf%2F9HkmlXCiX4kyS2TKoudTj0%2Fimg.png" weight=500px height=250px>

- Host - 호스트 URL.
- User-Agent - 클라이언트 정보.
- Accept - 서버에서 해당 타입에 데이터를 보내달라고 요청하는 헤더.
- Authorization - JWT 같은 인증 토큰을 서버로 보낼 때 사용하는 헤더.

위의 HTTP Method는 GET 방식이라 Body 부분은 따로 존재 하지 않지만 GET이 아닌 다른 메소드의 경우 Body가 존재한다.

.

### HTTP Request 구조
<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FSGfdC%2FbtqRxZWq8iF%2Fn3TA0JZZVj58bei0lfDtbk%2Fimg.png" weight=500px height=250px>


#### startline
<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fm3GjD%2FbtqRqP8qnAh%2FoLLYFKNxZoJopNIzqaD5x0%2Fimg.png" weight=500px height=250px>

- HTTP Version - 응답온 메세지의 HTTP 버전 정보.
- Status Code - 응답 코드.
- Status text - 응답 상태.

#### header
<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FBNiRt%2FbtqRqcJv0Iv%2FvPav8McqfhGSjOp0ngqxFK%2Fimg.png" weight=500px height=250px>

- Date - 응답온 일시.
- Content-Type - 응답 데이터의 타입.
- Cache-Control - 캐시용 헤더.
- 기타 등등

#### body
<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fbiac6B%2FbtqRpuDFbql%2F4kWFxEtBvc919CiVYPyCFK%2Fimg.png" weight=600px height=150px>

- 요청에 대한 응답 값

.

### HTTP는 보안적으로 안전한가?
HTTP 통신은 클라이언트와 서버간의 통신에 있어서 별다른 보안 조치가 없기 때문에 만약 누군가 네트워크 신호를 가로챈다면 HTTP의 내용은 그대로 외부에 노출된다.

중요 정보가 없는 소규모의 프로젝트라면 문제가 되지 않겠지만 고객의 개인정보나 비밀을 취급하는 대규모 서비스라면 큰 보안적 허점이 될 것.

이런 문제를 해결하기 위해 등장한 것이 HTTPS이다.

.

#### 자주 사용하는 HTTP Status Code
https://velog.io/@geeneve/%EC%9E%90%EC%A3%BC-%EC%82%AC%EC%9A%A9%ED%95%98%EB%8A%94-HTTP-%EC%83%81%ED%83%9C-%EC%BD%94%EB%93%9C

.

## HTTPS

### HTTPS(Hyper Text Transfer Protocol Secure)란?
> HTTP와 동일한 방식으로 동작하며 HTTP에 데이터 암호화가 추가된 프로토콜

- 네트워크 상에서 중간에 제3자가 정보를 볼 수 없도록 `암호화`를 지원하고 있다.
- SSL(Secure Sockets Layer) 위에 HTTP를 얹어서 보안이 보장된 통신을 하는 프로토콜입니다.
- `443번 포트`를 사용한다.

.

### HTTPS 동작 방식

<img src="https://i.imgur.com/4GHgl0T.png" weight=400px height=400px>

기존의 HTTP 프로토콜은 전송계층의 TCP위에서 동작한다.

여기서 SSL(Secure Sockets Layer)이라는 보안계층이 전송계층 위에 올라간다. SSL 위에 HTTP를 얹어서 보안이 보장된 통신을 수행한다.

SSL 암호화 통신은 `대칭키 + 공개키 암호화` 방식의 알고리즘을 통해 구현된다.

p.s - 현재의 표준은 TLS (Transport Layer Security)

.

### 대칭키 암호화와 공개키 암호화

#### 대칭키 암호화
- 클라이언트와 서버가 동일한 키를 사용해 암호화/복호화를 진행함
- 키가 노출되면 매우 위험하지만 연산 속도가 빠름

#### 공개키(비대칭키) 암호화
- 1개의 쌍으로 구성된 공개키와 개인키를 암호화/복호화 하는데 사용함
- 키가 노출되어도 비교적 안전하지만 연산 속도가 느림

SSL/TLS 프로토콜은 초기 연결 설정 단계에서 공개키 암호화 방식을 사용하여 상호 인증과 세션 키 교환을 수행한 후, 데이터 전송 단계에서 대칭키 암호화 방식을 사용하여 데이터를 암호화한다.

.

### HTTPS 동작 순서
<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FcCodLU%2FbtrqRZnoOFq%2Fe6kFHjADoVby70466Jkq51%2Fimg.png" weight=400px height=1000px>

1. 클라이언트(브라우저)가 서버로 최초 연결 시도를 함
2. 서버는 공개키(엄밀히는 인증서)를 브라우저에게 넘겨줌
3. 브라우저는 인증서의 유효성을 검사하고 세션키를 발급함
4. 브라우저는 세션키를 보관하며 추가로 서버의 공개키로 세션키를 암호화하여 서버로 전송함
5. 서버는 개인키로 암호화된 세션키를 개인키를 통해 복호화하여 세션키를 얻음
6. 클라이언트와 서버는 동일한 세션키(대칭)를 공유하므로 데이터를 전달할 때 세션키로 암호화/복호화를 진행함


