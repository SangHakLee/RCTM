# [JWT/JSON Web Token] 로그인 / 인증에서 Token 사용하기

![jwt logo|center](https://cloud.githubusercontent.com/assets/9030565/21663877/4b9f2c48-d326-11e6-9cd3-7460c85fdf45.png)

### Introduction
웹 / 앱 개발을 하면 로그인 과정에서 반드시 만나게 되는 개념이 `쿠키-세션`이다.
이미 많은 자료와 경험으로 인해 쿠키는 `나쁜 놈` 세션은 `좋은 놈`, 로그인은 일단 세션으로 해야지라는 개념이 개발자들의 머릿속에 자리 잡혀있다.
그러나, 최근 들어 IT 인프라 구성에 많은 변화가 생겼다. 웹 기반의 서비스들은 웹과 앱을 함께 서비스하는 것을 넘어 'Mobile First' 앱이 먼저라는 인식까지 생겨났다.
또한, AWS, Azure 와 같은 [IaaS][1] 클라우드 서비스가 대중화 되면서 고사양 단일 서버 아키텍쳐에서 중-저사양 다중 서버 아키텍쳐로 변화하고 있다.
이러한 상황에서 더 이상 쿠키-세션 기반 인증 아키텍쳐는 현재의 요구사항을 만족하지 못하고 있다.

현재의 요구 사항을 그나마 충족시키는 Web Token 기반 **`JWT`**에 대해서 알아보고 `Web Token`이 나타난 배경과 장단점에 대해서 알아보겠다.

<div style="text-align: center;"><iframe src="http://slides.com/sanghaklee/jwt/embed" width="576" height="420" scrolling="no" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen></iframe></div>

## Part 1. JWT's Basic Information

#### JSON Web Token
- [JSON][2] 포맷을 이용한 Web Token
- Claim based Token
- 두 개체에서 JSON 객체를 이용해 Self-contained 방식으로 정보를 안전한게 전달
- 회원 인증, 정보 전달에 주로 사용
- [RFC 7519][3]

#### Claim based ?
- `Claim` : 사용자에 대한 프로퍼티 / 속성
- 토큰 자체가 정보
- `Self-contained` : 자체 포함, 즉 토큰 자체가 정보
- key / value

<br/>

## Part 2. Web Token

#### 웹 토큰의 필요성
- CSRF, 기존 시스템의 보안 문제
- CORS, 도메인 확장시 api로서의 문제
- Web, Mobile 등 다양한 클라이언트
- Session 의 한계
- Scale out 의 한계
- REST API : REST API는 Stateless를 지향

#### 현재 우리는?
- 그런데 아직도 Cookie??
- 쿠키 기반 로그인의 문제점은 너무 많다
- Server side Session 기반으로 넘어갈 것인가?

**쿠키 기반 인증은 두말 할것 없이 변경해야 하고, 보편적 Session으로 갈것인가?**

<br/>

## Part 3. Type of Authorization

#### Cookie - Client Side Storage
> 문제점 투성이

<br/>

#### Session - Sever Side Authorization
![session](https://cloud.githubusercontent.com/assets/9030565/21638893/adb4ff6e-d2b0-11e6-86ac-cd72ddce0c68.png)
> Cookie와 차이점은 Cookie는 정보를 클라이언트에 저장하고 Session은 정보를 서버에 저장한다.

<br/>

#### Session Problem 1 - 서버 확장시 세션 정보의 동기화 문제
![session problem1](https://cloud.githubusercontent.com/assets/9030565/21638996/2ca1845a-d2b1-11e6-932b-18b164af9001.png)
> 서버가 스케일아웃 돼서 여러 개가 생기면 각 서버마다 세션 정보가 저장된다.
로그인시(서버1), 새로고침(서버2) 로 접근하면 서버는 인증이 안됐다고 판단한다.

<br/>

#### Session Problem 2 - 서버 / 세션 저장소의 부하
![session problem2](https://cloud.githubusercontent.com/assets/9030565/21639100/a4ffcc36-d2b1-11e6-8358-ea5c1c0b2f03.png)
> 세션을 각 서버에 저장하지 않고 세션 전용 서버, DB에 저장해도 문제가 생긴다.
모든 요청 시 해당 서버에 조회해야 한다. DB 부하를 야기할 수 있다.

<br/>

#### Session Problem 3 - 웹 / 앱 간의 상이한 쿠키-세션 처리 로직
![session problem3](https://cloud.githubusercontent.com/assets/9030565/21639127/ca0484ea-d2b1-11e6-8a55-8939db277e0e.png)
> 기존의 Client는 웹 브라우저가 유일했다. 그러나 이제는 모바일로 접근하는 경우도 처리해야 한다.
웹 / 앱 의 쿠키 처리 방법이 다르고 또 다른 Client 가 생겨나면 쿠키-세션에 맞게 처리해야 한다.

<br/>

#### Token - Self-contained & Stateless
![token](https://cloud.githubusercontent.com/assets/9030565/21639175/0268fde8-d2b2-11e6-8764-fc2acef3a2bd.png)
> 앞의 문제를 해결하는 최선의 방법은 토큰이다.
토큰은 서버의 상태를 저장하지 않는다. 토큰 자체로 정보를 가지고 있기 때문에 별도의 인증서버가 필요없다. 따라서 요청을 받을 서버 자체에서 인증 프로세스를 수행할 수 있다.
또한, JSON 포맷으로 통신하기 때문에 어떤 Client 에서든 Data 통신에 JSON을 이용하면 토큰을 이용할 수 있다.

<br/>

## Part 4. JWT's Architecture

#### JWT의 기본 구조

```bash
Header . Payload . Signature
```

<br/>

#### Header
> JWT 웹 토큰의 헤더 정보

- `typ` : 토큰의 타입, JWT만 존재
- `alg`  : 해싱 알고리즘.  (HMAC SHA256 or RSA). 헤더를 암호화 하는게 아니다. 토큰 검증시 사용.

```json
{
     "alg" : "HS256",
     "typ" : "JWT"
}
```
**위의 내용을 base64 로 인코딩한다.  =>   eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9 **
**[base 64][4] 는 암호화된 문자열이 아니다. 같은 문자열에 대해서는 항상 같은 인코딩 문자열을 반환한다.**

<br/>

#### Payload
> 실제 토큰으로 사용하려는 데이터가 담기는 부분. 각 데이터를 Claim이라고 하며 다음과 같이 3가지 종류가 있다.

- `Reserved claims` : 이미 예약된 Claim. 필수는 아니지만 사용하길 권장. key 는 모두 3자리 String이다.
    - iss   (String)     : issuer, 토큰 발행자 정보
    - exp (Number)  : expiration time, 만료일
    - sub  (String)     : subject, 제목
    - aud  (String)     : audience,
    - [More][5]
- `Public claims` : 사용자 정의 Claim.
    - Public 이라는 이름처럼 공개용 정보
    - 충돌 방지를 위해 [URI][6] 포맷을 이용해 저장한다.
- `Private claims` : 사용자 정의 Claim
    - Public claims 과 다르게 사용자가 임의로 정한 정보
    - 아래와 같이 일반 정보를 저장한다.

    ```json
    {
        "name" : "hak",
        "age"  : 26,
    }
    ```

<br/>

#### Signature
> Header와 Payload의 데이터 무결성과 변조 방지를 위한 서명
Header + Payload 를 합친 후, Secret 키와 함께 Header의 해싱 알고리즘으로 인코딩

```plain
HMACSHA256(
  base64UrlEncode(header) + "." +
  base64UrlEncode(payload),
  secret)
```

<br/>

#### JWT 구조
> JWT는 [Header Payload Signature] 각각 JSON 형태의 데이터를 `base 64` 인코딩 후 합친다.
아래와 같은 순서로 `.` 을 이용해 합친다.
최종적으로 만들어진 토큰은 [HTTP][7] 통신 간 이용되며, `Authorization` 이라는 `key`의 `value`로서 사용된다.

![jwt structure](https://cloud.githubusercontent.com/assets/9030565/21639513/c8fd1240-d2b3-11e6-8404-e78ccd52493f.PNG)

<br/>

#### JWT 인증 과정
![jwt process](https://cloud.githubusercontent.com/assets/9030565/21639528/e53e01ee-d2b3-11e6-86d1-6c67c16841eb.PNG)

<br/>

## Part 5. Is JWT a Silver bullet?

> 그렇다면 모든 서비스들은 기존의 쿠키-세션 기반에서 웹 토큰 기반의 인증으로 변경해야할까?

#### JWT 의 단점 &  도입시 고려사항
- `Self-contained` : 토큰 자체에 정보가 있다는 사실은 양날의 검이 될수 있다.

    - `토큰 길이` : 토큰 자체 payload 에 Claim set을 저장하기 때문에 정보가 많아질수록 토큰의 길이가 늘어나 네트워크에 부하를 줄 수 있다.

    - `payload 암호화` : payload 자체는 암호화 되지 않고 base64로 인코딩한 데이터다.
중간에 payload를 탈취하면 디코딩을 통해 테이터를 볼 수 있다.
[JWE][8] 를 통해 암호하하거나, payload에 중요 데이터를 넣지 않아야 한다.

- `Stateless` : 무상태성이 때론 불편할 수 있다. 토큰은 한번 만들면 서버에서 제어가 불가능하다.
토큰을 임의로 삭제할 수 있는 방법이 없기 때문에 토큰 만료시간을 꼭 넣어주는게 좋다.

- `tore Token` : 토큰은 클라이언트 side에서 관리해야하기 때문에 토큰을 저장해야한다.

<br/>

### Conclusion

HTTP, REST API 의 공통적인 특징은 `Stateless`(무상태)를 지향한다는 것이다.
`Stateful`, 즉 상태를 저장하는 서버는 많은 [Side-effect][9]를 발생시킬 수 있다.
또한, 서론에 말했듯이 현재의 IT 인프라 구조는 유연한 확장 가능성이 있어야 하는데 기존의 쿠키-세션 기반의 인증을 사용하면 확장 가능한 인프라를 구성하기 힘들다.
기존의 로그인 / 인증을 모두 Web Token 기반으로 변경할 수는 없지만, 앞으로 만들게 될 서비스 특히 [RESTful][10]한 API의 인증에는 JWT를 사용해보는 것이 좋을 것이라 생각한다.

### References
- [\[JWT\] 토큰(Token) 기반 인증에 대한 소개][11]
- [\[JWT\] JSON Web Token 소개 및 구조][12]
- [\[node.js\] Token 기반 인증][13]
- [REST JWT(JSON Web Token)소개 - #1 개념 소개][14]
- [JWT(JSON Web Token)에 대해서...][15]
- https://jwt.io/introduction/


  [1]: https://ko.wikipedia.org/wiki/IaaS
  [2]: http://www.json.org/json-ko.html
  [3]: https://tools.ietf.org/html/rfc7519
  [4]: https://ko.wikipedia.org/wiki/%EB%B2%A0%EC%9D%B4%EC%8A%A464
  [5]: https://tools.ietf.org/html/draft-jones-json-web-token-07#section-4.1
  [6]: https://en.wikipedia.org/wiki/Uniform_Resource_Identifier
  [7]: https://en.wikipedia.org/wiki/Hypertext_Transfer_Protocol
  [8]: https://tools.ietf.org/html/rfc7516
  [9]: https://en.wikipedia.org/wiki/Side_effect_%28computer_science%29
  [10]: https://en.wikipedia.org/wiki/Representational_state_transfer
  [11]: https://velopert.com/2350
  [12]: https://velopert.com/2389
  [13]: http://behonestar.tistory.com/37
  [14]: http://bcho.tistory.com/999
  [15]: https://blog.outsider.ne.kr/1160
