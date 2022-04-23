# OAuth 2.0

OAuth는 인터넷 사용자들이 비밀번호를 제공하지 않고 다른 웹 사이트 상의 자신들의 정보에 대해 웹사이트나 애플리케이션의 접근 권한을 부여할 수 있는 공통적인 수단으로 사용되는 접근 위임을 위한 개방형 표준이다. (위키백과)

OAuth는 인증(Authentication)과 인가(Authorization)를 모두 포함하고 있지만 **인가**에 초점을 맞추고 있다.

- 주된 목적은 인증된 사용자에게 해당 서비스를 이용할 수 있는 권한을 제공하는 것이다.

## OAuth 2.0 구성요소

### Resource Server

- OAuth 2.0 서비스를 제공하고 자원을 관리하는 서버 
- Google, Naver, Apple 등

### Resource Owner

- Resource Server의 계정을 소유하고 있는 사용자 
- Client가 제공하는 서비스를 통해 로그인하는 실제 사용자를 말한다.

### Client

- Resouce Server에 접속해서 정보를 가져오고자 하는 클라이언트(웹/모바일 어플리케이션)이다.

### Authorization Server

- Client가 Resource Server의 서비스를 사용할 수 있게 인증하고 토큰을 발급해주는 서버

## OAuth 2.0 인증 방식

OAuth 2.0은 아래 4가지 인증 방식을 제공하지만 웹이나 앱 환경에서는 주로 Authorization Code Grant 방식을 사용한다. 

- **Authorization Code Grant**
- Implict Grant
- Resource Owner Password Credentials Grant
- Client Credentials Grant

### Authorization Code Grant

권한 부여 코드 승인 타입으로 클라이언트가 다른 사용자 대신 특정 리소스에 접근을 요청할 때 사용된다.

- 리소스 접근을 위해 Authorization Server에서 받은 권한 코드로 리소스에 대한 Access token을 받는 방식이다.

<p align="center">
  <img src="./image/oauth-authorization%20code%20grant.png" width="700"> 
  <br/>
  <small>출처: <a href="https://cheese10yun.github.io/oauth2/">https://cheese10yun.github.io/oauth2/</a></small>
</p>

---

#### 참고

- [OAuth 위키백과](https://ko.wikipedia.org/wiki/OAuth)
- [OAuth와 춤을](https://d2.naver.com/helloworld/24942)
- [OAuth2 인증 방식 정리](https://cheese10yun.github.io/oauth2/)