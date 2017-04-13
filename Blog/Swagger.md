# [Swagger] RESTful API 문서 만들기 feat. YAML

![swagger-icon][1]

### Introduction
[Node.js][2]를 사용해서 API 서버를 개발해야 하는 상황이 생겼다.
기존에 있는 API 서버를 Node.js 언어로 바꾸는 것이 아니고 새로운 API 서버를 만들어야 하는 상황이었다.
개발된 API 설명서를 사용자에게 제공하면서 API를 테스트할 수 있는 **Test-bed**를 제공하면 좋을 것이라 생각했다.
기존의 **Open API**들을 사용할 때, [Postman][3]을 이용해서 문서의 내용을 복-붙 후  테스트 해보고 다시 개발하는 과정이 귀찮다고 느낀 것이다.
**Open API** 문서 자체에서 테스트를 할 수 있으면 사용자의 편의성이 높아질 것이라는 생각에 찾게 된 것이 [Swagger][4]다.

## Swagger

#### THE WORLD' S MOST POPULAR API FRAMEWORK
>Swagger is a powerful open source framework backed by a large ecosystem of tools that helps you design, build, document, and consume your RESTful APIs.

**Swagger**를 사용하면 쉽게 RESTful API 서비스를 설계, 제작, 문서화할 수 있다.

<br>

## Why API Docs?
다른 서비스의 Open API를 사용하기 전에 처음으로 해야할 일이 뭘까?
사용하려는 서비스의 홈페이지에 들어가서 API 사용 설명서를 읽는 것이 먼저일 것이다.
API 사용자는 API 문서에 의존할 수 밖에 없다. 

### Test pyramid
**필자가 집중한 것은 Test-bed 의 역할을 겸한 API 문서를 작성하는 것이다.**


`Application`을 개발할 때, 테스트는 중요하다.
많은 개발자들이 시간과 노력이 많이 들어도 [TDD][5]를 지향하는데는 이유가 있다.
확실한 Test case를 통한 Unit test는 유지보수 시 큰 효과를 발휘한다.

---

![test pyramid][6]
> `Unit test`는 빠르다. 테스트를 하는데 걸리는 시간은 오래 걸리지 않는다.
Unit test는 테스트할 case가 많고 case 작성에 시간이 많이 들지 테스트가 수행되는 시간은 금방이다.(컴퓨터가 하니깐!)
그러나, `UI test` 는 테스트 과정이 느리고 비용이 많이 든다. 보통 UI test는 사람의 수작업을 필요로 하는 경우가 많기 때문이다.

---
### Test case
#### 1. Unit Test with [mocha][7], [should.js][8]
```javascript
'ab'.should.be.equalOneOf('a', 10, 'ab');
res.should.be.json();
promise.should.be.Promise();
['user_1', 'user_1000'].should.matchAny(/^(user_)+[1-9]{1}\d*$/);
```
> `Unit test`는 함수, 메소드의 결과값이 예상한 결과값과 일치하는지 즉, 개발자의 의도대로 작동하는지 검사한다.

#### 2. Black-box Test with [supertest][9]
**GET /users/{user_id}**
```javascript
request(app)
    .get('/users/user_10')
    .set('Accept', 'application/json')
    .expect(200)
    .then(response => {
        response.should.be.json();
        assert(response.body.name, 'hak')
    })
request(app)
    .get('/users/123')
    .set('Accept', 'application/json')
    .expect(400)
```
> `Black-box`는 내부 로직에 대한 이해 없이 올바른 입력과 올바르지 않은 입력을 사용해서 올바른 출력을 판별한다.

#### 3. UI Test 
- [selenium][10]
- [froglogic][11]
- [ranorex][12]

> 완벽한 Test-case를 만들 순 없지만 위의 tool들을 이용하면 UI 테스트도 어느 정도 자동화할 수 있다.

<br>

### REST API Doesn't have UI
그러나, REST API는 화면이 없다.
API 사용자가 Open API를 사용하기 전에 테스트 할 수 있는 방법이 뭐가 있을까?
1. API test tool like [Postman][13]
2. HTML/JS 로 프로토타입 만들고 테스트
3. API 제공자가 Test-bed를 제공

> **`NOTE :`** Postman은 정말 잘 만든 API test-tool 이다. 사용한 경험이 없다면, 꼭 한 번 사용해보길 권한다.
RESTful한 API를 테스트할 수 있을 뿐만 아니라, **API를 언어별 코드로 만들어주고** Test-runner를 만들어서 **전체 API를 테스트**하고 실행 결과를 분석할 수 있다.

<br>

### RESTful APIs

#### Just Docs
아래 3가지 Open API들은 문서가 정적으로 제공된다는 공통점을 갖고 있다.
즉, 문서 자체에서 Test를 해볼 수 없다.
- [Facebook][14]
    - `GET` **graph.facebook.com/v2.5/me** HTTP/1.1 
- [Github][15]
    - `GET` **api.github.com/users/sanghaklee** HTTP/1.1 
- [Kakao][16]
    - `POST` **kauth.kakao.com/oauth/token** HTTP/1.1

#### With Test-bed
문서만 제공하는 API 달리 몇몇 API 문서들은 Test-bed를 함께 제공한다.
주관적이지만, Iamport의 API 문서가 개발자 친화적이라 생각한다.
- [Iamport][17]
- [gAPI][18]

<br>

### Swagger
> Swagger의 주된 목적은 RESTful API를 **`문서화 시키고 관리`**하는 것이다.
API 문서를 일반 Document로 작성하면 API 변경시 마다 문서를 수정해야하는 불편함이 있는데, Swagger 같은 Framework를 이용하면 이를 자동화할 수 있다.

> Swagger의 주된 목적은 API 문서의 효율적 작성이지만, **`필자가 중점을 둔 것은 테스트 환경의 제공이다.`**
Swaager로 API 문서를 만들면 문서 자체가 API에 대한 설명이면서 Test-bed 이다.
사용자는 API 문서를 읽으면서 바로 해당 API에 대해 테스트를 해볼 수 있다.

> Unit -> Black-box 테스트 과정을 통과 후 배포해도 실제 Service-level에서 오류가 발생할 수 있다.
이러한 이유가 우리가 Open API를 사용 전 Postman으로 테스트를 해보는 이유이다.
Swagger를 이용하면 이러한 API Test 환경을 문서와 함께 제공하기 때문에 Postman 같은 tool을 이용할 필요가 적어진다.

> **`이것이 Swagger를 포함한 API Framework를 사용하는 이유이다`.**

#### UI
아래의 링크로 이동하면 Swagger 예쩨 API Docs를 볼 수 있다.
- http://swagger.io/swagger-ui/
- http://petstore.swagger.io/

#### Editor
Swagger Editor를 이용하면 쉽게 Swagger 문서 모델을 만들 수 있다.
http://editor.swagger.io/#!/
```json
---
swagger: '2.0'
info:
  version: 1.0.0
  title: Echo
  description: |
    #### Echos back every URL, method, parameter and header
    Feel free to make a path or an operation and use **Try Operation** to test it. The echo server will
    render back everything.
schemes:
  - http
host: mazimi-prod.apigee.net
basePath: /echo
paths:
  /:
    get:
      responses:
        200:
          description: Echo GET
    post:
      responses:
        200:
          description: Echo POST
      parameters:
        - name: name
          in: formData
          description: name
          type: string
        - name: year
          in: formData
          description: year
          type: string
```

<br>

### Other
Swagger와 비슷하게 API Docs를 만들어주는 서비스가 있다.

- http://apidocjs.com/
- https://github.com/mashery/iodocs

<br>

## YAML
> Swagger는 기본적으로 [YAML][19] 포맷을 이용해 Docs 문서를 작성한다.
물론 JSON 포맷으로도 가능하지만, Swagger가 기본으로 채택한 YAML도 살펴볼 필요가 있다.

- XML, C, 파이썬, 펄, [RFC2822][20]에서 정의된 e-mail 양식에서 개념을 얻어 만들어진 `사람이 쉽게 읽을 수 있는` **데이터 직렬화** 양식이다.
- **Y**AML **A**in't **M**arkup **L**anguage
    - YAML은 [마크업][21]이 아닌 **데이터 자체**를 중요시 여긴다는 뜻에서 지어진 이름이다.


### 문법
#### 주석 : **`#`**
```
# 이 라인은 주석입니다. 
```
--- 
#### 배열(list) : **`-`**
```plain
- Lil Dicky
- Rae sremmurd
- R.City
```

```
[Lil Dicky, Rae sremmurd, R.City]
```
> **`-`** 을 사용하지 않고 JSON의 **`[]`** 을 사용해서 **한 줄로** 배열을 나타낼 수 있다.

---
#### 객체(hash) : **`:`**
```plain
name: Lil Dicky
birth: 1988
born: United States
```

```plain
{name:Lil Dicky, birth: 1988, born: United States}
```
> JSON의 **`{}`** 을 사용해서 **한 줄로** 객체를 나타낼 수 있다.

---
#### 분류 - 공백(space)을 이용해 각 데이터를 구분
```plain
---
intro: Yeah bitch, check my profile. Perfect, but you are not.
profile:
  name: beenzino
  birth: 1987
  sns:
    youtube: https://www.youtube.com/channel/UCyFkumV0dfLxSY602sIYNvw
    insta: https://www.instagram.com/realisshoman/
  song:
    single: [Nike Shoes, Aqua Man, Up All Night]
    crew: [연결고리, '11:11']
```
> **tab** 은 사용하지 못한다. 시스템마다 tab을 표현하는 방법이 다르기 때문에 공백만 사용.

[More][22]

<br>

### JSON vs YAML

#### JSON
```json
{
  "name": "hak",
  "age": 27,
  "programmer": true,
  "money": null,
  "blog": "http://sanghaklee.tistory.com/",
  "lang": ["JavaScript", "PHP", "Java"]
}
```

#### YAML
```plain
name: hak
age: 27
programmer: true
money: 
blog: http://sanghaklee.tistory.com/
lang:
- JavaScript
- PHP
- Java
```
[JSON -> YAML][23]
[YAML -> JSON][24]



### Conclusion
`There is no silver bullet.`
모든 문제에 만능 해결 **key**는 없다. SW 공학에서 여러가지 방법론을 평가할 때 사용하는 말이다.
원론적으로 `RESTful API`에서 API Docs가 필요하지 않을 수 있다.
그러나, 높은 완성도와 사용자 편의성을 위해 Docs를 만드는 것이 좋다.
위에서 본 Facebook, Github와 같이 오직 문서로만 Docs를 만들 수 있고, 좀 더 나아가 간단한 테스트 환경을 만들어 줄 수 있다.
만약, API Docs를 만들면서 Test 환경을 같이 제공해 주고 싶다면 `Swagger` 가 좋은 선택이 될 수 있다.

**Swagger의 [사전적 의미][25]처럼 멋지고 으스댈 수 있는 API 문서를 만들어 보자!**

### References
- [swagger를 이용한 nodeJS API Document 만들기][26]
- [Swagger로 API 문서 자동화하기][27]
- [Swagger tutorial][28]
- [API 문서화 도구 Swagger v2 사용법][29]


  [1]: https://encrypted-tbn1.gstatic.com/images?q=tbn:ANd9GcQoYYweNDfsYwsQ_QlOD5tFNIEE-JL3tqE2fyIZqjMQwntXnIeFzQ
  [2]: https://nodejs.org
  [3]: https://www.getpostman.com/
  [4]: http://swagger.io/
  [5]: https://en.wikipedia.org/wiki/Test-driven_development
  [6]: https://martinfowler.com/bliki/images/testPyramid/test-pyramid.png
  [7]: https://mochajs.org/
  [8]: https://shouldjs.github.io/
  [9]: https://github.com/visionmedia/supertest
  [10]: http://www.seleniumhq.org/
  [11]: www.froglogic.com/
  [12]: www.ranorex.com/
  [13]: https://www.getpostman.com/
  [14]: https://developers.facebook.com/docs/graph-api/using-graph-api
  [15]: https://developer.github.com/v3/
  [16]: https://developers.kakao.com/docs/restapi
  [17]: https://api.iamport.kr/
  [18]: http://gapi.gabia.com/apiexplorer#none
  [19]: http://yaml.org/
  [20]: https://tools.ietf.org/html/rfc2822
  [21]: https://namu.wiki/w/%EB%A7%88%ED%81%AC%EC%97%85%20%EC%96%B8%EC%96%B4
  [22]: https://ko.wikipedia.org/wiki/YAM
  [23]: https://www.json2yaml.com/
  [24]: https://www.json2yaml.com/convert-yaml-to-json
  [25]: https://translate.google.com/?source=gtx#en/ko/swagger
  [26]: http://rabbit87min.tistory.com/4
  [27]: http://jojoldu.tistory.com/31
  [28]: http://idratherbewriting.com/learnapidoc/pubapis_swagger.html
  [29]: http://itzone.tistory.com/681
