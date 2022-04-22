# JWT(Json Web Token)

JWT는 모바일이나 웹에서 사용자 인증을 위해 사용되는 암호화된 토큰을 말한다.

### JWT를 사용한 인증과정

1. 사용자가 아이디, 비밀번호를 입력해 로그인한다.
2. 서버는 JWT(AccessToken, RefreshToken)를 생성해 클라이언트에게 발급한다.
3. 클라이언트는 서버로부터 받은 JWT를 로컬 스토리지에 저장한다. (다른 곳에 저장할 수 있다)
   - 클라이언트는 서버에 요청 시 헤더에 AccessToken을 담아서 보낸다.
4. 서버는 클라이언트가 헤더에 담아서 보낸 AccessToken이 유효한지 확인한다.
   - 유효한 토큰이면 클라이언트가 요청한 데이터를 반환한다.
5. 만일 AccessToken이 만료되었다면 RefreshToken을 사용해 서버로부터 새로운 AccessToken을 발급받는다.


## JWT 구조

<p align="center">
  <img src="./image/jwt-structure.png">
  <small>출처: http://www.opennaru.com/opennaru-blog/jwt-json-web-token/</small>
</p>


JWT는 **HEADER, PAYLOAD, SIGNATURE**로 구분된다.

- URL에서 파라미터로 사용할 수 있도록 URL_Safe한 Base64Url로 인코딩되어 표현된다.
- 각 부분을 구분하기 위해 **.(점)** 을 사용한다.

### - HEADER

HEADER는 alg와 typ 두 가지 정보로 구성되어 있다.

```json
{
  "alg": "HS256",
  "typ": "JWT"
}
```

- **alg:** 해싱 알고리즘 지정하며 SIGNATURE 및 토큰 검증에 사용된다.
- **typ:** 토큰의 타입을 지정

### - PAYLOAD

토큰에서 사용될 Claim이 담겨있다.

- 세 가지로 나누어지며 Json 형태로 다수의 정보를 넣을 수 있다.
- 서버와 클라이언트가 주고받는 시스템에서 실제로 사용될 정보에 대한 내용을 담고 있다.

#### 등록된 클레임(Registered Claim)

토큰 정보를 표현하기 위해 이미 정해진 종류의 데이터로 선택적으로 사용 가능하다.

- **iss(issuer) - 토큰 발급자**
- **sub(subject) - 토큰 제목**
- **aud(audience) - 토큰 대상자**
- **exp(expiration) - 토큰 만료시간:** NumericDate 형식으로 지정하며, 현재 시간 이후로 설정해야한다.
- **nbf(not before) - 토큰 활성날짜:** NumericDate 형식으로 지정하며, 이 날짜가 지나기 전까지는 토큰이 활성화 되지 않는다.
- **iat(issued at) - 토큰 발급시간:** 토큰 발급 이후의 경과 시간을 알 수 있다.
- **jti(jwt id) - 토큰 고유 식별자:** 중복 방지를 위해 사용된다.

#### 공개 클레임(Public Claim)

사용자가 정의할 수 있는 클레임으로 공개용 정보 전달을 위해 사용된다.

```json
{
  "http://hello.world": "ok"
}
```

- 서로 충돌하지 않는 이름을 가져야 하기에 URL 형태로 작성한다.

#### 비공개 클레임(Private Claim)

사용자가 정의할 수 있는 클레임으로 클라이언트와 서버 간에 정보를 공유하기 위해 사용된다.

```json
{
  "token_type": "access"
}
```

### - SIGNATURE

토큰을 인코딩하거나 유효성 검증을 할 때 사용하는 고유한 암호화 코드이다.

- SIGNATURE는 HEADER와 PAYLOAD의 값을 Base64Url로 인코딩 한 후 해당 값을 비밀키를 이용해 HEADER에서 정의한 알고리즘(alg)으로 해싱을 하고 다시 Base64Url로 인코딩하여 생성한다.


- PAYLOAD는 단순히 Base64Url로만 인코딩한 데이터이기 때문에 중간에 탈취당하면 손쉽게 디코딩해 데이터를 확인할 수 있다.
- Claim에 정보가 많으면 JWT 길이가 길어지기 때문에 네트워크에 부담을 줄 수 있다.

---

#### 참고

- [JWT (JSON Web Token) 이해와 활용](http://www.opennaru.com/opennaru-blog/jwt-json-web-token/)
- [[JWT] JSON Web Token 소개 및 구조](https://velopert.com/2389)
- [JWT란 무엇인가](https://tech.toktokhan.dev/2021/04/30/JWT/)
- [[WEB] 📚 JWT(Json Web Token )란? 💯 이해하기 쉽게 정리](https://inpa.tistory.com/entry/WEB-📚-JWTjson-web-token-란-💯-정리)