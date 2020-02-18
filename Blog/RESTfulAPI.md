# 1. RESTful API 설계 가이드

본 문서는 REST API를 좀 더 RESTful 하게 설계하도록 가이드할 목적으로 만들어졌다.

일부 규칙들은 기존에 존재하는 회사 규칙 때문에 보편적인 REST API의 철학과 다를 수 있다.

따라서, 기본적인 REST API 개념 설명은 아래의 링크로 대신한다.

> - [REST API 제대로 알고 사용하기](http://meetup.toast.com/posts/92)
> - [REST 아키텍처를 훌륭하게 적용하기 위한 몇 가지 디자인 팁](https://spoqa.github.io/2012/02/27/rest-introduction.html)


**TOC**
<!-- TOC -->

- [1. RESTful API 설계 가이드](#1-restful-api-설계-가이드)
- [2. URL Rules](#2-url-rules)
    - [2.1. 마지막에 `/` 포함하지 않는다.](#21-마지막에--포함하지-않는다)
    - [2.2. _(underbar) 대신 -(dash)를 사용한다.](#22-_underbar-대신--dash를-사용한다)
    - [2.3. 소문자를 사용한다.](#23-소문자를-사용한다)
    - [2.4. 행위(method)는 URL에 포함하지 않는다.](#24-행위method는-url에-포함하지-않는다)
    - [2.5. 컨트롤 자원을 의미하는 URL 예외적으로 동사를 허용한다.](#25-컨트롤-자원을-의미하는-url-예외적으로-동사를-허용한다)
- [3. Set HTTP Headers](#3-set-http-headers)
    - [3.1. Content-Location](#31-content-location)
    - [3.2. Content-Type](#32-content-type)
    - [3.3. Retry-After](#33-retry-after)
        - [3.3.1. Case 1. 인증](#331-case-1-인증)
        - [3.3.2. Case 2. 자원 요청](#332-case-2-자원-요청)
    - [3.4. Link](#34-link)
- [4. Use HTTP methods](#4-use-http-methods)
    - [4.1. POST, GET, PUT, DELETE 4가지 methods는 반드시 제공한다.](#41-post-get-put-delete-4가지-methods는-반드시-제공한다)
    - [4.2. OPTIONS, HEAD, PATCH를 사용하여 완성도 높은 API를 만든다.](#42-options-head-patch를-사용하여-완성도-높은-api를-만든다)
        - [4.2.1. OPTIONS](#421-options)
        - [4.2.2. HEAD](#422-head)
        - [4.2.3. PATCH](#423-patch)
            - [4.2.3.1. Example](#4231-example)
- [5. Use HTTP status](#5-use-http-status)
    - [5.1. 의미에 맞는 HTTP status를 리턴한다](#51-의미에-맞는-http-status를-리턴한다)
    - [5.2. HTTP status만으로 상태 에러를 나타낸다](#52-http-status만으로-상태-에러를-나타낸다)
- [6. Use the correct HTTP status code.](#6-use-the-correct-http-status-code)
    - [6.1. 성공 응답은 `2XX`로 응답한다.](#61-성공-응답은-2xx로-응답한다)
    - [6.2. 실패 응답은 `4XX`로 응답한다.](#62-실패-응답은-4xx로-응답한다)
    - [6.3. `5XX` 에러는 절대 사용자에게 나타내지 마라!](#63-5xx-에러는-절대-사용자에게-나타내지-마라)
- [7. Use HATEOAS](#7-use-hateoas)
    - [7.1. 구성 요소](#71-구성-요소)
    - [7.2. 응답 예제](#72-응답-예제)
- [8. Paging, Ordering, Filtering, Field-Selecting](#8-paging-ordering-filtering-field-selecting)
    - [8.1. Paging](#81-paging)
        - [8.1.1. Paging Key](#811-paging-key)
        - [8.1.2. 응답 예제](#812-응답-예제)
            - [8.1.2.1. HTTP Header의 Link 속성을 이용한다.](#8121-http-header의-link-속성을-이용한다)
            - [8.1.2.2. HATEOAS로 응답한다.](#8122-hateoas로-응답한다)
            - [8.1.2.3. Link, HATEOAS 모두 사용한다.](#8123-link-hateoas-모두-사용한다)
    - [8.2. Ordering](#82-ordering)
        - [8.2.1. 요청 샘플](#821-요청-샘플)
    - [8.3. Filtering](#83-filtering)
    - [8.4. Field-Selecting](#84-field-selecting)
        - [8.4.1. 요청 샘플](#841-요청-샘플)
- [9. Versioning](#9-versioning)
    - [9.1. 종류](#91-종류)
        - [9.1.1. 예외적으로 서비스의 기본 도메인이 3차인 경우 `path level`에 모두 명시한다.](#911-예외적으로-서비스의-기본-도메인이-3차인-경우-path-level에-모두-명시한다)
    - [9.2. URI Versioning 개발 가이드](#92-uri-versioning-개발-가이드)

<!-- /TOC -->

# 2. URL Rules

## 2.1. 마지막에 `/` 포함하지 않는다.
**Bad**
```
http://api.test.com/users/
```
**Good**
```
http://api.test.com/users
```

## 2.2. _(underbar) 대신 -(dash)를 사용한다.
-(dash)의 사용도 최소한으로 설계한다. 정확한 의미나 표현을 위해 단어의 결합이 불가피한 경우 반드시 -(dash) 사용한다.

**Bad**
```
http://api.test.com/users/post_commnets
```
**Good**
```
http://api.test.com/users/post-commnets
```

## 2.3. 소문자를 사용한다.
**Bad**
```
http://api.test.com/users/postCommnets
```
**Good**
```
http://api.test.com/users/post-commnets
```

## 2.4. 행위(method)는 URL에 포함하지 않는다.
**Bad**
```
POST http://api.test.com/users/1/delete-post/1
```
**Good**
```
DELETE http://api.test.com/users/1/posts/1
```

## 2.5. 컨트롤 자원을 의미하는 URL 예외적으로 동사를 허용한다.
함수처럼, 컨트롤 리소스를 나타내는 URL은 동작을 포함하는 이름을 짓는다.

**Bad**
```
http://api.test.com/posts/duplicating
```
**Good**
```
http://api.test.com/posts/duplicate
```

# 3. Set HTTP Headers

## 3.1. Content-Location
`POST` 요청은 대부분 idempotent(멱등, `f(f(x))=f(x)`) 하지 않다. (멱등, 반환되는 응답 리소스의 결과가 항상 동일하다)

> Location and Content-Location are different. Location indicates the URL of a redirect, while Content-Location indicates the direct URL to use to access the resource, without further content negotiation in the future.
https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Content-Location

```
POST /users
{
    "name": "hak"
}
```
위와 같은 요청은 매번 다른 리소스를 반환한다.
첫 번째는 `/users/1` 두 번째는 `/users/2`  ...  n번째는 `/users/n`

따라서 요청의 응답 헤더에 새로 생성된 리소스를 식별할 수 있는 `Content-Location` 속성을 이용한다.

```http
HTTP/1.1 200 OK
Content-Location: /users/1
```

> GET, PUT 등의 요청은 `idempotent` 하다
>
> `GET /users/1`의 경우 언제나 같은 결과로 응답한다.
>
> PUT을 POST처럼 쓰는 경우엔 idempotent하지 않을 수 있다.

**HATEOAS로 `Content-Location`를 대체할 수 있다.**

## 3.2. Content-Type
**application/json**을 우선(될 수 있다면 이것만!)으로 제공한다.

2018년이다. json을 이용하자.

application/xml 등을 제공해서 응답 포맷을 이원화하지 말자!

응답 포맷을 여러 개로 나누면 요청 포맷도 나눠야 한다.

## 3.3. Retry-After
비정상적인 방법(DoS, Brute-force attack)으로 API 서버를 이용하려는 경우 `429 Too Many Requests` 오류 응답과 함께 일정 시간 뒤 요청할 것을 나타낸다.
```http
HTTP/1.1 429 Too Many Requests
Retry-After: 3600
```

### 3.3.1. Case 1. 인증
- `/auth` OAuth, JWT 같은 인증 관련 리소스를 요청하는 작업
- `/login` Id, Password를 이용한 로그인 작업

비정상적인 요청(401) 일 때 두 가지 응답 방안
1. n 시간 동안 n 회만 요청 가능
    - `429` 응답과 함께 Retry-After: n
1. n 회만 요청 가능
    - `401` 응답과 함께 해당 사용자(IP)는 더 이상 인증 관련 API를 사용할 수 없고 다시 요청하려면 특수한 절차가 필요하다는 메세지 응답
        - **Retry-After**와 관계없음

### 3.3.2. Case 2. 자원 요청
- `/posts` 특정 사용자가 의도적으로 서버 과부화를 목적으로 반복 요청하는 경우

n 시간 동안 n 회 이상 요청한 경우 `429`

## 3.4. Link
페이징 처리를 위해 사용한다.

github 방법을 따른다. https://developer.github.com/v3/#pagination

```http
HTTP/1.1 200 OK
Link: <https://api.test.com/users?page=3&per_page=100>; rel="next",
  <https://api.test.com/users?page=50&per_page=100>; rel="last"
```
- `rel`

| Name  	| Description                                                   	|
|-------	|---------------------------------------------------------------	|
| next  	| The link relation for the immediate next page of results.     	|
| last  	| The link relation for the last page of results.               	|
| first 	| The link relation for the first page of results.              	|
| prev  	| The link relation for the immediate previous page of results. 	|

- query parameter의 `page`, `per_page` 이름은 알맞게 변경한다.

# 4. Use HTTP methods

## 4.1. POST, GET, PUT, DELETE 4가지 methods는 반드시 제공한다.

| methods    	| POST        	| GET               	| PUT                   	| DELETE            	|
|------------	|-------------	|-------------------	|-----------------------	|-------------------	|
| /users     	| 사용자 추가 	| 사용자 전체 조회  	| 사용자 추가 or 사용자 수정 	| 사용자 전체 삭제  	|
| /users/hak 	| 405 ERROR   	| 사용자 'hak' 조회 	| 사용자 'hak' 수정      	| 사용자 'hak' 삭제 	|
- `PUT /users` 경우 표에선 사용자 추가 or 사용자 수정으로 했지만, 보통 `Collection`에 PUT 요청은 지원하지 않으므로 **405 ERROR** 응답하기도 한다.
    - Collection: `/users/hak` 에서 **users**, 집합
    - Document: `/users/hak` 에서 **hak**, 집합에 속한 자원

## 4.2. OPTIONS, HEAD, PATCH를 사용하여 완성도 높은 API를 만든다.

### 4.2.1. OPTIONS
https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods/OPTIONS

현재 End-point가 제공 가능한 API method를 응답한다.
```
OPTIONS /users/hak
```
```http
HTTP/1.1 200 OK
Allow: GET,PUT,DELETE,OPTIONS,HEAD
```

### 4.2.2. HEAD
https://developer.mozilla.org/ko/docs/Web/HTTP/Methods/HEAD

요청에 대한 Header 정보만 응답한다. **body**가 없다.

```
HEAD /users
```
```http
HTTP/1.1 200 OK
Content-Type: application/json
Content-Length: 120
```

### 4.2.3. PATCH
https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods/PATCH

PUT 대신 PATCH method를 사용한다.

자원의 일부를 수정할 때는 `PATCH`가 목적에 맞는 method다.

- **[PATCH](https://tools.ietf.org/html/rfc5789)**, which is used to apply partial modifications to a resource.
- **[PUT](https://tools.ietf.org/html/rfc7231#section-4.3.4)** method requests that the state of the target resource be created or replaced with the state defined by the representation enclosed in the request message payload


#### 4.2.3.1. Example
| id 	| name 	| level 	|
|----	|------	|-------	|
| 1  	| hak  	| 10    	|
| 2  	| lee  	| 5     	|

**PUT 요청 시 요청을 일부분만 보낸 경우 나머지는 default 값으로 수정되는 게 원칙이다. 그러나, 대부분 PUT 요청에서 이와 같이 개발하진 않는다.**
```
PUT /users/1
{
    "level": 11
}
```
```http
HTTP/1.1 200 OK
{
    "name": null,
    "level": 11
}
```

**PUT은 다음과 같이 바뀌지 않는 속성도 보내야 한다.**
```
PUT /users/1
{
    "name": "hak"
    "level": 11
}
```
```http
HTTP/1.1 200 OK
{
    "name": "hak",
    "level": 11
}
```

**PATCH를 이용하여 원래의 목적대로 'level'만 변경하는 요청을 보낸다.**
```
PATCH /users/1
{
    "level": 11
}
```
```http
HTTP/1.1 200 OK
{
    "name": "hak",
    "level": 11
}
```

# 5. Use HTTP status

## 5.1. 의미에 맞는 HTTP status를 리턴한다

**Bad**
```http
HTTP/1.1 200 OK
{
    "result" : false
    "status" : 400
}
```
- status는 `200`으로 성공인데 body 내용엔 실패에 관한 내용을 리턴하고 있다.
    - 모든 응답을 `200`으로 처리하고 body 내용으로 `성공|실패`를 판단하는 구조에서 사용된다. 잘못된 설계다.

**Good**
```http
HTTP/1.1 400 Bad Request
{
    "msg" : "check your parameter"
}
```

## 5.2. HTTP status만으로 상태 에러를 나타낸다
세부 에러 사항은 응답 객체에 표시하거나, 해당 에러를 확인할 수 있는 link를 표시한다.

**http 상태 코드를 응답 객체에 중복으로 표시할 필요 없다.**

**Bad**
```http
HTTP/1.1 404 Not Found
{
    "code" : 404,
    "error_code": -765
}
```

**Good**
```http
HTTP/1.1 404 Not Found
{
    "code" : -765,
    "more_info" : "https://api.test.com/errors/-765"
}
```

# 6. Use the correct HTTP status code.

## 6.1. 성공 응답은 `2XX`로 응답한다.
- 200 : [OK] 
- 201 : [Created]
    - 200과 달리 요청에 성공하고 새로운 리소스를 만든 경우에 응답한다.
        - POST, PUT에 사용한다.
- 202 : [Accepted]
    - 클라이언트 요청을 받은 후, 요청은 유효하나 서버가 아직 처리하지 않은 경우에 응답한다. `비동기 작업`
        - 요청에 대한 응답이 일정 시간 후 완료되는 작업의 경우 
        - 작업 완료 후 클라이언트에 알릴 수 있는 server push 작업을 하거나, 클라이언트가 해당 작업의 진행 상황을 조회할 수 있는 URL을 응답해야 한다.
        
            ```http
            HTTP/1.1 202 Accepted
            {
                "links": [
                    {
                        "rel": "self",
                        "method": "GET",
                        "href":  "https://api.test.com/v1/users/3"
                    }
                ] 
            }
            ```
- 204 : [No Content]
	- 응답 body가 필요 없는 자원 삭제 요청(DELETE) 같은 경우 응답한다.
	- 200 응답 후 body에 `null`, `{}`, `[]`, `false`로 응답하는 것과 다르다.
		- 204의 경우 HTTP body가 아예 없는 경우

## 6.2. 실패 응답은 `4XX`로 응답한다.
- 400 : [Bad Request]
	- 클라이언트 요청이 미리 정의된 파라미터 요구사항을 위반한 경우
	- 파라미터의 위치(`path`, `query`, `body`), 사용자 입력 값, 에러 이유 등을 **반드시** 알린다 
        - case 1

		```
		{
			"message" : "'name'(body) must be Number, input 'name': test123"
		}
		```
        - case 2

		```
		{
			"errors": [
				{
					"location": "body",
					"param": "name",
					"value": "test123",
					"msg": "must be Number"
				}
			]
		}
		```
- 401 : [Unauthorized]
- 403 : [Forbidden]
    - 해당 요청은 유효하나 서버 작업 중 접근이 허용되지 않은 자원을 조회하려는 경우
    - 접근 권한이 전체가 아닌 일부만 허용되어 요청자의 접근이 불가한 자원에 접근 시도한 경우 응답한다.
- 404 : [Not Found]
- 405 : [Method Not Allowed]
	- 405 code는 404 code와 혼동될 수 있기 때문에 룰을 잘 정하고 시작한다.
	- `POST /users/1`의 경우 404로 응답한다고 생각할 수 있지만, 경우에 따라 **405**로 응답할 수 있다.
	`/users/:id` URL은 GET, PATCH, DELETE method는 허용되고 POST는 불가한 URL이다.
		- 만약 id가 `1`인 사용자가 없는 경우엔 404로 응답하지만(GET, PATCH, DELETE의 경우), `POST /users/1`는 `/users/:id` URL이 POST method를 제공하지 않기 때문에 405로 응답하는 게 옳다.
    - `Allow: GET, PATCH, DELETE` HTTP header에 허용 가능한 method를 표시한다.
- 409 : [Conflict]
    - 해당 요청의 처리가 비지니스 로직상 불가능하거나 모순이 생긴 경우
    - e.g.) `DELETE /users/hak`의 경우, 비지니스 로직상 사용자의 모든 자원이 비어있을 때만 사용자를 삭제할 수 있는 규칙이 있을 때 409로 응답한다.
    
        ```
        409 Conflict
        {   
            "message" : "first, delete connected resources."
            "links": [
                {
                    "rel": "posts.delete",
                    "method": "DELETE",
                    "href":  "https://api.test.com/v1/users/hak/posts"
                },
                {
                    "rel": "comments.delete",
                    "method": "DELETE",
                    "href":  "https://api.test.com/v1/users/hak/comments"
                }
            ]
        }
        ```
- 429 : [Too Many Requests]
	- `Retry-After: 3600`(HTTP Headers)
    - DoS, Brute-force attack 같은 비정상적인 접근을 막기 위해 요청의 수를 제한한다.


## 6.3. `5XX` 에러는 절대 사용자에게 나타내지 마라!
API Server level에선 **500** 에러가 나선 안된다.
**이건 서비스 장애!**

즉, API Server는 모든 발생 가능한 에러를 핸들링해야 한다.

만약 API Server를 서빙하는 웹서버(apache, nginx)가 오류일 때는 500 가능

# 7. Use HATEOAS
Hypermedia as the Engine of Application State

> A hypermedia-driven site provides information to navigate the site's REST interfaces dynamically by including hypermedia links with the responses. 
> https://spring.io/understanding/HATEOAS

REST API는 `요청-응답`이라는 간단한 구조로 이루어져 있다.
또한 응답의 내용도 단순하다.

만약, `POST /users {"name": "hak"}`이라는 요청을 보내면
서버는 응답으로 `201 Created {"id": 1, "name":"hak"}`와 같이 응답할 것이다.

여기서 문제는 이 응답만으론 사용자 리스소의 상태가 전이되기엔 정보가 부족하다는 것이다.

REST API가 아닌 HTML 환경에선 눈에 보이는 화면이 있기 때문에 `POST /users` 후에 사용자의 상태가 전이될 수 있는 link를 화면에서 제공할 수 있다.

이 문제를 해결하기 위해 응답 객체에 해당 리소스의 상태가 전이될 수 있는 link들을 함께 제공한다. link들을 통해 리소스의 다음 상태 전이 정보를 동적으로 제공한다.

## 7.1. 구성 요소

- 변경될 리소스의 상태 관계 `rel`
    - `self`: 현재 URL 자신, 예약어처럼 쓰임
- 요청 URL `href`
- 요청 Method `method`
- ...(그 외 추가사항)

```json
{
    "rel": "self",
    "href": "http://api.test.com/users/1",
    "method": "GET"
}
```


## 7.2. 응답 예제
```
201 Created
{
    "id": 1,
    "name": "hak",
    "createdAt": "2018-07-04 14:00:00"
    "links": [
        {
            "rel": "self",
            "href": "http://api.test.com/users/1",
            "method": "GET"
        },
        {
            "rel": "delete",
            "href": "http://api.test.com/users/1",
            "method": "DELETE"
        },
        {
            "rel": "update",
            "href": "http://api.test.com/users/1",
            "method": "PATCH",
            "more_info": "http://api.test.com/docs/user-update"
            "body": {
                "name": "{The value to be modified}"
            }
        },
        {
            "rel": "user.posts",
            "href": "http://api.test.com/users/1/posts",
            "method": "GET"
        }
    ]
}
```

- **rel**의 값은 `self`를 제외하고 내부 규칙을 정해서 따른다.
    의미만 정확히 드러나면 된다.
- `more_info`, `body`와 같이 내부 정의된 key를 사용해도 된다.
    - `more_info`: 해당 link의 세부 정보를 알고 싶은 경우 탐색할 웹 페이지 주소
    - `body`: 해당 link가 `POST`, `PUT`인 경우 파라미터를 body에 넣어 보내기 때문에 보낼 수 있는 body 파라미터 샘플. 파라미터가 너무 많거나 제약사항이 엄격한 경우 `more_info`로 해당 link의 상세 정보를 확인할 수 있게 한다.



# 8. Paging, Ordering, Filtering, Field-Selecting
## 8.1. Paging
Collection(리스트)에 대한 GET 요청의 경우(`GET /users`) 한 번에 모든 결과를 응답하지 않고 적당한 크기로 데이터 셋을 나눠서 응답한다.

### 8.1.1. Paging Key
- [Github](https://developer.github.com/v3/?#pagination)
    - `page`
    - `per_page`
- [Atlassian](https://developer.atlassian.com/server/confluence/pagination-in-the-rest-api/)
    - `start`
    - `limit`
- [Digitalocean](https://developers.digitalocean.com/documentation/v2/#links)
    - `page`
    - `per_page`
- 개발자 관점
    - `offset`
    - `limit`

**어떤 key로 paging을 처리할지 변경될 수 있으니 개발자는 코드의 설정 값으로 언제든 key 이름을 변경할 수 있게 구현한다.**

### 8.1.2. 응답 예제
```
GET /users
```

#### 8.1.2.1. HTTP Header의 Link 속성을 이용한다.
```http
HTTP/1.1 200 OK
Link: 
 <https://api.test.com/users?offset=10&limit=10>; rel="next",
 <https://api.test.com/users?offset=50&limit=10>; rel="last",
 <https://api.test.com/users?offset=0&limit=10>; rel="first",
 <https://api.test.com/users?offset=0&limit=0>; rel="prev",
[
    {1, ...},
    {2, ...},
    ...
    {10,...},
]
```

#### 8.1.2.2. HATEOAS로 응답한다.
```http
HTTP/1.1 200 OK
[
    {1, ...},
    {2, ...},
    ...
    {10,...},
    "links": [
        {
            "rel": "next",
            "method": "GET",
            "link": "https://api.test.com/users?offset=10&limit=10
        },
        {
            "rel": "last",
            "method": "GET",
            "link": "https://api.test.com/users?offset=50&limit=10
        },
        {
            "rel": "first",
            "method": "GET",
            "link": "https://api.test.com/users?offset=0&limit=10
        },
        {
            "rel": "prev",
            "method": "GET",
            "link": "https://api.test.com/users?offset=0&limit=0
        },
    ]
]
```

#### 8.1.2.3. Link, HATEOAS 모두 사용한다.
```http
HTTP/1.1 200 OK
Link: 
 <https://api.test.com/users?offset=10&limit=10>; rel="next",
 <https://api.test.com/users?offset=50&limit=10>; rel="last",
 <https://api.test.com/users?offset=0&limit=10>; rel="first",
 <https://api.test.com/users?offset=0&limit=0>; rel="prev",
[
    {1, ...},
    {2, ...},
    ...
    {10,...},
    "links": [
        {
            "rel": "next",
            "method": "GET",
            "link": "https://api.test.com/users?offset=10&limit=10
        },
        {
            "rel": "last",
            "method": "GET",
            "link": "https://api.test.com/users?offset=50&limit=10
        },
        {
            "rel": "first",
            "method": "GET",
            "link": "https://api.test.com/users?offset=0&limit=10
        },
        {
            "rel": "prev",
            "method": "GET",
            "link": "https://api.test.com/users?offset=0&limit=0
        },
    ]
]
```

## 8.2. Ordering
Collection(리스트)에 대한 GET 요청의 경우(`GET /users`) 리스트를 클라이언트의 요청에 맞게 정렬해 응답한다.

`order`라는 key를 사용한다.
- 오름차순: key
- 내림차순: -key

### 8.2.1. 요청 샘플
```
GET /users?order=name
```

- `?order=-name`: name 내림차순 `name desc`
- `?order=-name,level`: name 내림차순, level 오름차순 `name desc, level asc`

## 8.3. Filtering
Collection(리스트)에 대한 GET 요청의 경우(`GET /users`) 리스트 검색 조건을 요청할 수 있다.

- `AND`, `OR`
- `=`, `!=` 
- `>`, `>=`
- `<`, `>=`
- `IN`(OR), `NOT IN`
- `LIKE`(include)

type = ssd or (cpu=1 or memory>=2) and staus != 04 


## 8.4. Field-Selecting
Collection(리스트)에 대한 GET 요청의 경우(`GET /users`) 리스트 결과의 일부분만 선택해서 응답받을 수 있다.

### 8.4.1. 요청 샘플
```
GET /users?fields=level
```
```http
HTTP/1.1 200 OK
{
    "level": 10
}
```

- include: **?fields=id,name**
    - `id, name` 만 반환

        ```http
        HTTP/1.1 200 OK
        {
            "id": 1,
            "name": "hak"
        }
        ```
- exclude: **?-fields=level**
    - `level` 제외 모두 반환

         ```http
        HTTP/1.1 200 OK
        {
            "id": 1,
            "name": "hak",
            "createdAt": "2018-07-04 14:00:00"
        }
        ```
- 존재하지 않는 key
    - `fields`에 존재하는 key가 하나도 없는 경우, fields 모두 무시
    - (?fields=aaaaaa  혹은  ?fields=aaaaaa,bbbb)

        ```http
        HTTP/1.1 200 OK
        {
            "id": 1,
            "name": "hak",
            "level" 10,
            "createdAt": "2018-07-04 14:00:00"
        }
        ```
    - `fields`에 key가 일부분만 존재하는 경우, 존재하는 key로만 selecting
    - (?fields=aaaaaa,name)
        
        ```http
        HTTP/1.1 200 OK
        {
            "name": "hak"
        }
        ```

# 9. Versioning

## 9.1. 종류

- URI Versioning

    ```
    http://api.test.com/v1
    http://apiv1.test.com
    ```
- Accept header

    ```
    Accept: application/vnd.example.v1+json
    Accept: application/vnd.example+json;version=1.0
    ```

URI Versioning을 채택하고, 버저닝 정보는 host레벨이 아닌 path레벨에 명시한다.

**Do**
```
http://api.test.com/v1
``` 
**Don't**
```
http://apiv1.test.com
```

### 9.1.1. 예외적으로 서비스의 기본 도메인이 3차인 경우 `path level`에 모두 명시한다.

**Domain**
```
http://www.test.com
http://service1.test.com
http://service2.test.com
```

**Your Service API URI**
```
http://service3.test.com
```

**Allow**
```
http://service3.test.com/api/v1
```
- **Don't** `http://api.service3.test.com/v1`

## 9.2. URI Versioning 개발 가이드
- 개발 코드에서 버저닝 정보를 관리하지 않는다.
- 개발 프로젝트 폴더의 버저닝은 VCS(e.g. git)를 이용한다.
    v1(v1 branch), v2(master branch)
- 웹 서버의 reverse-proxy 기능을 활용한다
    - 웹 APP 서버의 라우팅은 버저닝을 제외하고 개발한다. /users, /posts
    - http://api.test.com/v1 -> (reverse-proxy) -> 웹 APP /
    - `시나리오 case`(Node.js express server)
        - v1 process
            - app-name: v1
            - port: 3001
            - dir: /home/v1
            - Apache **ProxyPassReverse "/v1" "http://127.0.0.1:3001"**
            - Nginx **location /v1 {proxy_pass http://127.0.0.1:3001}**
        - v2 process
            - app-name: v2
            - port: 3002
            - dir: /home/v2
            - Apache **ProxyPassReverse "/v2" "http://127.0.0.1:3002"**
            - Nginx **location /v2 {proxy_pass http://127.0.0.1:3002}**
    - API 버저닝 여부에 관계없이 프로젝트 구조가 변경되지 않는다.
        



**Bad**
```
- routes
    - /v1
        - /users
        - /posts
    - /v2
        - /users
        - /posts
    - ...
```

**Good**
```
- routes
    - /users
    - /posts
```
