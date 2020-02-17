# REST API 관점에서 바라보는 HTTP 상태 코드(HTTP status code) 

**TOC**
1. Introduction
2. HTTP 와 REST
3. HTTP Status Code
4. 2XX Success
	4.1. 200 OK
	4.2. 201 Created
	4.3. 202 Accepted
	4.4. 204 No Content
5. 4XX Client errors
	5.1. 400 Bad Request
	5.2. 401 Unauthorized
	5.3. 403 Forbidden
	5.4. 404 Not Found
	5.5. 405 Method Not Allowd
	5.6. 409 Conflict
	5.7. 429 Too many Requests
6. 5XX Server errors
7. Conclusion


### Introduction
[RESTful API 설계 가이드](https://sanghaklee.tistory.com/57) 심화 과정으로 설계 가이드의 **HTTP 상태 코드**에 대해서 자세하게 알아보려고 한다.

이번 주제의 핵심은 **REST API 관점에서 바라보는** 이다. 
이 주제를 정한 이유를 알기 위해선 `HTTP와 REST`의 관계에 대해서 알 필요가 있다.

## HTTP 와 REST
**HTTP**(HyperText Transfer Protocol)는 웹 환경에서 정보를 주고받기 위한  [프로토콜](http://www.ktword.co.kr/abbr_view.php?m_temp1=432)이다.
> HTTP
> - https://ko.wikipedia.org/wiki/HTTP
> - https://developer.mozilla.org/ko/docs/Web/HTTP/Overview
> - http://www.ktword.co.kr/abbr_view.php?m_temp1=648

- 클라이언트는 HTTP의 상태 코드를 확인하여 요청의 성공|실패를 확인할 수 있다.
- 이것은 HTTP를 사용하는 클라이언트와 서버 간의 약속, 프로토콜인 것이다.

**REST**(Representational State Transfer)는 분산 하이퍼미디어 시스템을 위한 소프트웨어 아키텍처이다.
> REST
> - https://ko.wikipedia.org/wiki/REST
> - https://restfulapi.net

---

**HTTP**는 웹 환경에서 정보를 송수신할 때 사용하는 약속이고, **REST**는 소프트웨어 아키텍처다.

REST에 반드시 HTTP가 필요한 것은 아니다. [WAP](https://ko.wikipedia.org/wiki/%EB%AC%B4%EC%84%A0_%EC%95%A0%ED%94%8C%EB%A6%AC%EC%BC%80%EC%9D%B4%EC%85%98_%ED%94%84%EB%A1%9C%ED%86%A0%EC%BD%9C), [WebRTC](https://ko.wikipedia.org/wiki/WebRTC), [MQTT](https://ko.wikipedia.org/wiki/MQTT) 등 다른 프로토콜로도 이용 가능하다.

REST는 소프트웨어 아키텍처(설계 지침, 원리 등등)고 REST에서 `클라이언트-서버` 간 통신 시 HTTP를 사용한 것이다.

## 그래서 REST에서 HTTP는?
REST에서 HTTP는 필수가 아니라고 했지만, 웹 환경 통신의 대부분이 HTTP를 사용한다.

만약, 어떤 API의 요청|응답 데이터 포맷이 [YAML](https://ko.wikipedia.org/wiki/YAML)이면 어떨까?
물론 YAML이 JSON 포맷의 데이터를 대체하여 표현할 수 있고 JSON이 갖는 한계를 보완한 부분도 있지만, 일반적이지 않다.
일반적이지 않은 것은 불편함을 야기한다. 이러한 불편함을 감수하면서 일반적이지 않는 것을 사용하려면 매우 설득력 있는 근거가 필요하다. 
REST의 경우엔 현재로선 없다.

REST는 HTTP를 사용하는 것이 일반적이다. 필수가 아니라고 했지만, **거의 필수다.**
REST의 개념/원리에는 HTTP가 필수는 아니지만, 그 사용에 있어 필수적으로 변한 경우라 생각하면 좋을 거 같다. ([de-facto standard](https://namu.wiki/w/%EC%82%AC%EC%8B%A4%EC%83%81%20%ED%91%9C%EC%A4%80))

누군가 만든 REST API가 HTTP를 사용하지 않는다면 이것부터 고치라고 지시할 것이다.

**따라서 REST를 이용해 API를 설계하려면 HTTP에 대해서 알고 있어야 한다.**

# HTTP Status Code (상태 코드)

[HTTP Status code, 상태 코드](https://developer.mozilla.org/ko/docs/Web/HTTP/Status)는 **HTTP** 요청이 성공했는지 실패했는지를 서버에서 알려주는 코드다.

### 기능과 목적
function  and purpose
> **기능**: 사물이 갖는 일정한 구실
> **목적**: 어떤 일을 통해 이루려고 하거나 하고자 하는 것
>
> 펜은 그리는 기능을 갖고, 어떤 이는 글을 쓰고 어떤 이는 그림을 그린다.
> 컴퓨터는 연산 기능을 갖고, 어떤 이는 게임을 하고 어떤 이는 코딩을 한다.


상태 코드의 기능은 정해져 있다. 하지만 목적은 정보 제공자와 소비자가 선택한 약속에 의해 다르게 적용될 수 있다.
예를 들면 상태 코드 `200`은 요청이 성공적으로 완료되었다는 메세지를 전달하는 기능을 갖고 이것을 이용해 클라이언트에 다음 작업을 이어 나가도 좋다는 신호의 목적으로 쓰일 수 있다.

따라서 각 상태 코드는 제공자에 따라 다른 목적으로 클라이언트에 제공될 수 있다.

앞으로 설명의 상태 코드들은 HTTP 상에서 갖는 기능에 대해서 설명하고 이것이 REST에서 어떤 목적으로 쓰이는지 위주로 설명할 것이다.

### 예제
| Methods  	| POST        	| GET                	| PUT                        	| DELETE            	|
|----------	|-------------	|--------------------	|----------------------------	|-------------------	|
| /users   	| 사용자 추가 	| 사용자 전체 조회   	| 사용자 추가 or 사용자 수정 	| 사용자 전체 삭제  	|
| /users/1 	| 405 ERROR   	| ID `1` 사용자 조회 	| ID `1`사용자 수정          	| ID `1`사용자 삭제 	|
- Collection: `/users/1` 에서 **users**, 집합
- Document: `/users/1` 에서 **1**, 집합에 속한 자원


## 2XX [Success](https://en.wikipedia.org/wiki/List_of_HTTP_status_codes#2xx_Success)
2xx 번대의 상태 코드들은 서버가 클라이언트의 요청을 성공적으로 처리했다는 의미다.

### 200 OK
> [200 OK](https://developer.mozilla.org/ko/docs/Web/HTTP/Status/200)
> 클라이언트의 요청을 서버가 정상적으로 처리했다.

성공에 대한 모든 상태 코드를 `200`으로 응답해도 크게 상관없다. (`200` 상태 코드는 클라이언트에게 요청이 성공했다는 것을 응답하는 기능을 갖기 때문에)
많은 REST API에서 `2xx` 상태 코드를 세분화하여 사용하지 않는다.

하지만,  클라이언트에게 더 정확하고 자세한 정보를 제공하기 위해선 적절한 상태 코드를 보내는 것이 좋다.(`2xx` 상태 코드들은 각각 세분화된 목적을 갖는다.)

**하지만, 아래 설계는 무조건 잘못된 것이니 수정하자!**
```http
HTTP/1.1 200 OK
{
	"result" : false
	"status" : 400
}
```
- 상태 코드는 `200`으로 성공인데 body 내용엔 실패에 관한 내용을 리턴하고 있다.
- 모든 응답을 `200`으로 처리하고 body 내용으로 성공|실패를 판단하는 구조에서 사용된다.
	- API가 아닌 HTML 웹 프로젝트에서 대부분 이렇게 사용한다.
	- 웹 프로젝트에선 크게 문제가 되지 않으나 API에선 많이 이상한 구조다.
	- 웹의 설계를 그대로 사용하는 경우 많이 하는 실수다.

### 201 Created
> [201 Created](https://developer.mozilla.org/ko/docs/Web/HTTP/Status/201)
> 클라이언트의 요청을 서버가 정상적으로 처리했고 새로운 리소스가 생겼다.

`201` 상태 코드는 `POST`, `PUT` 요청에 대한 응답에 주로 사용된다.
클라이언트의 요청이 성공적으로 이뤄졌다는 의미까지는 `200`과 동일한데, 성공과 동시에 새로운 리소스가 생성되었다는 의미를 포함한다.

```http
POST /users HTTP/1.1
Content-Type: application/json
{
	"name": "hak"
}
```
```http
HTTP/1.1 201 Created
{
	"id" : 1,
	"name" : "hak"
}
```
물론 새로운 리소스를 생성하는 `POST`, `PUT` 요청의 응답으로 `200`을 보내줘도 된다. 또한 아직 많은 API의 상태 코드가 이렇게 응답한다.

**하지만, 더 정확한 의미를 전달하기 위해 `201` 상태 코드를 쓸 것을 추천한다.**

---

**TIP!**
>  HTTP 헤더의 [Content-Location](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Content-Location)를 이용하여 만들어진 리소스 생성된 위치를 알려주면 더할 나위 없이 좋다.
```http
HTTP/1.1 201 Created
Content-Location: /users/1
{
	"id" : 1,
	"name" : "hak"
}
```

### 202 Accepted
> [202 Accepted](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/202)
> 클라이언트의 요청은 정상적이나, 서버가 아직 요청을 완료하지 못했다.

`202` 상태 코드는 [비동기](https://ko.wikipedia.org/wiki/%EB%B9%84%EB%8F%99%EA%B8%B0_%EC%9E%85%EC%B6%9C%EB%A0%A5)에 대한 개념이 없다면 생소할 수 있다.
클라이언트의 요청이 정상적이면 서버에선 작업의 `성공|실패` 응답하는 게 일반적이나, 작업 완료를 위한 일련의 작업들이 오래 걸리기 때문에 나중에 알려주겠다는 의미다.
> 커피숍의 음료 주문 프로세스로 예를 들면, (비동기를 설명하는 아주 고전적의 예시다. 마치 객체지향의 붕어빵 같은..)
> 
> **고객**은(`클라이언트`) **카운터**(`API`)의 **직원**(`URI`)에게 음료 **주문**(`요청`)을 한다.
> 그러면 주문을 받은 카운터 직원이 커피를 만드는 게 아니라 해당 주문은 음료를 만드는 직원에게 전달된다.
> 이때, 카운터 직원은 **번호표**(`자원 식별자`)를 고객에게 **주고**(`응답`) 완료되면 알려주겠다고 말한 뒤 다음 고객의 주문을 받는다.
> 
> 예시 전에 설명한 **클라이언트의 요청이 정상적이면**의 의미는 고객이 커피 주문은 제대로 했다는 것이다.
> 만약 고객이 `"책 주세요"`라고 주문하거나 `브루마블 돈을 준다면` 제대로 된 요청이 아니라 판단하고 뒤에 나올 `400` 상태 코드를 응답한다.
> 
> 고객이 `아이스-아메리카노 주세요`라고 주문하면서 3000원을 카운터의 직원에게 건넨다면, 카운터의 직원은 요청이 유효하다 판단하고 음료 만드는 직원에게 주문을 넘기고 고객은 번호표를 받는다.
> 
> 이 시점이 **서버가 아직 요청을 완료하지 못한** 상황이다.(커피를 만드는데 일정 시간이 걸리기 때문)
> 일정 시간 뒤 고객은 번호표를 카운터의 직원에게 보여주고 맛있는 **아이스-아메리카노**를 받을 수 있다.

---

`202` 상태 코드에서 중요한 것은 **작업의 확인**이다. 비동기 작업은 해당 요청이 언제 완료되는지 알 수 없다.
클라이언트가 요청의 완료 여부를 확인할 수 있는 방법을 제공해야 한다. 2가지 방법이 있다.
1. [Callback](https://ko.wikipedia.org/wiki/%EC%BD%9C%EB%B0%B1)
1. [Polling](https://ko.wikipedia.org/wiki/%ED%8F%B4%EB%A7%81_(%EC%BB%B4%ED%93%A8%ED%84%B0_%EA%B3%BC%ED%95%99))

콜백은 서버가 작업이 완료되면 클라이언트에게 알려주는 것이다. 
폴링은 클라이언트가 주기적으로 해당 작업의 상태를 조회하는 것이다.([HATEOAS](https://en.wikipedia.org/wiki/HATEOAS), [Content-Location](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Content-Location) 등으로 작업의 상태를 확인할 수 있는 URI를 응답해야 한다.)
콜백과 폴링에 대한 이야기는 길어질 수 있기 때문에 위에 설명한 개념 정도만 알고 넘어간다. 

> 커피숍의 예로 느낌만 알고 간다.
> 커피가 만들어지면 **직원**(`callback`)은 **번호**(`자원 식별자`)를 부르고 고객은 커피를 받아 간다.
> 
> 직원이 부르는 것은 1회성이고 고객이 이를 듣지 못할 수 있다. 
> 고객은 **번호표**(`자원 식별자`)를 들고 **직원**(`polling`)에게 가서 해당 번호의 주문이 완료되었는지 묻고 고객은 커피를 받아 간다.

결론은 비동기 요청(`202` 상태 코드)은 `콜백`이든 `폴링`이든 클라이언트가 요청의 완료 여부를 확인할 수 있는 방법을 제공해야 한다는 것이다.

**둘 다 제공하는 것이 좋다.**

### 204 No Content
> [204 No Content](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/204)
> 클라이언트의 요청은 정상적이다. 하지만 컨텐츠를 제공하지 않는다.

`202` 상태 코드와 유사하게 이상한 응답이라 생각할 수 있다. 자원을 제공하지 않는 경우가 뭘까?
`204` 상태 코드는 자원의 삭제 요청에 응답할 수 있다.
```http
DELETE /users/1 HTTP/1.1
```
```http
HTTP/1.1 204 No Content
```

자원 삭제 요청을 했고 이 요청이 유효하니 서버는 해당 자원을 삭제했다. 더 이상 응답할 컨텐츠가 없기 때문에 컨텐츠가 없는 `204`로 응답한다.

여기서 주의할 점은  `200`으로 응답하고 응답 body에 `null`, `{}`, `[]`, `false` 등으로 응답하는 것과 다르다는 것이다.
`204`의 경우 [HTTP Response body](https://en.wikipedia.org/wiki/HTTP_message_body)가 아예 존재하지 않는 경우다.

사실 `204`를 응답하는 API는 흔하지 않다.
> `204`로 응답하는 예시다. 절대적이진 않다.
> 
> **PUT**
>  - 자원 수정 요청의 결과가 기존의 자원 내용과 동일하여 변경된 내용이 없을 때 `204`로 응답할 수 있다.
>  - 만약 수정 요청으로 자원의 내용이 변경된다면 `201`로 응답할 것이다.
>  
> **DELETE** 
> - 삭제 요청으로 자원을 삭제하여 더 이상 존재하지 않고 그 자원을 참조하는 모든 자원도 삭제되어 더 이상 HTTP body를 응답하는 것이 무의미해졌을 때 사용한다.

## 4XX [Client errors](https://en.wikipedia.org/wiki/List_of_HTTP_status_codes#4xx_Client_errors)
`4XX`의 상태 코드들은 클라이언트의 요청이 유효하지 않아 서버가 해당 요청을 수행하지 않았다는 의미다.

### 400 Bad Request
> [400 Bad Request](https://developer.mozilla.org/ko/docs/Web/HTTP/Status/400)
> 클라이언트의 요청이 유효하지 않아 더 이상 작업을 진행하지 않는 경우

API 서버는 클라이언트 요청이 들어오면 바로 작업을 진행하지 않고 요청이 서버가 정의한 유효성에 맞는지 확인 후 진행한다.
다음과 같은 사전 유효성 검증 작업을 진행할 수 있다.
- 필수 여부
- 유효 여부
  - 범위
  - 패턴
  - ...

---

대부분의 API는 사전에 유효성 검증을 통해 `400` 상태 코드로 클라이언트에게 유효하지 않은 요청임을 응답한다.
(유효성 검증 없이 진행하면 `5xx` 서버 오류가 발생할 수 있기 때문에 대부분 사전에 막는 로직을 추가한다.)

그러나, `400` 상태 코드로 응답하는 것만으로는 부족하다. 
오류 발생 시 **파라미터의 위치(`path`, `query`, `body`), 사용자 입력 값, 에러 이유를 꼭 명시하는 것이 좋다.**

**Bad**
```http
HTTP/1.1 400 Bad Request
```

**Good case 1**
```http
HTTP/1.1 400 Bad Request
{
	"message" : "'name'(body) must be String, input 'name': 123"
}
```

**Good case 2**
```http
HTTP/1.1 400 Bad Request
{
	"errors": [
		{
		"location": "body",
		"param": "name",
		"value": 123,
		"error": "TypeError",
		"msg": "must be String"
		}
	]
}
```

**Good case 3**
```http
HTTP/1.1 400 Bad Request
{
	"errors": {
		"message": "'name'(body) must be String, input 'name': 123",
		"detail": [
			{
			"location": "body",
			"param": "name",
			"value": 123,
			"error": "TypeError",
			"msg": "must be String"
			}
		]
	}	
}
```
> 잘 만들어진 API는 대부분 문서 정리도 잘되었기 때문에 문서만으로 파라미터의 어떤 값들이 유효하고 유효하지 않은지 쉽게 알 수 있다.
> 그러나, 대부분 API는 기능 위주로 만들어지기 때문에 문서에 많은 시간을 쓸 수 없다.
> 이러한 API를 사용하면서 `4XX` 오류를 만나면 꽤 골치 아프다.
> 분명 클라이언트(요청자)의 잘못인데 어떤 부분이 잘못된 지 알려주지 않으니 유효한 요청을 하나씩 찾아야 한다.
> `400` 상태 코드는 서버에서 잘못됐다고 판단하는 응답이기 때문에 서버는 그 요청이 왜 잘못된 지도 알 수 있다.
> 서버에선 쉽게 알 수 있지만 클라이언트는 알려주지 않으면 알기 어려운 내용인 것이다.
> 
> 커피숍을 예시로 들면,
> **고객**(`클라이언트`)이 **바닐라라떼 더블샷**(`파라미터`)을 **주문**(`요청`) 한다.
> 근데 이 커피숍에 바닐라라떼는 메뉴에 있지만, 샷 추가는 불가하다. **메뉴판**(`API 문서`)엔 이 내용이 없다.
> 이때, **카운터**(`API`)의 **직원**(`URI`)이 고객에게 `해당 주문은 불가합니다`라고만 하면 고객은 어떤 반응일까?
> 바닐라라떼가 메뉴가 없나(`404 상태 코드`)? 아니면 바닐라라떼는 주문이 많이 밀려서 불가한가(`429 상태 코드`)? 등의 생각을 할 것이다.
> **카운터**(`API`)의 **직원**(`URI`)은 고객에게 `바닐라라떼에 샷 추가는 불가합니다`라고 대답해야 한다.

### 401 Unauthorized
> [401 Unauthorized](https://developer.mozilla.org/ko/docs/Web/HTTP/Status/401)
> 클라이언트가 권한이 없기 때문에 작업을 진행할 수 없는 경우
> 
> **un**-authorized 
> [un](https://en.dict.naver.com/#/entry/enko/ac6950e4ffa74e93b972466e26adf763)
> [authorized](https://en.dict.naver.com/#/entry/enko/d4a4e6e3c258483b8b9ea2bfc10e3dae): 인정받은, 권한을 부여받은

상태 코드 이름만 보면 **권한**(`authorized`)에 대한 내용처럼 보이지만, 사실 **인증**(`authenticated`)에 대한 이야기다.

`401`은 **비인증**이다.
- 비인증: [非](https://hanja.dict.naver.com/search?query=%EC%95%84%EB%8B%90%EB%B9%84), Not
	- `비인증`은 **아니다**의 뜻이 강하다. 즉, 인증이 안된 상태다.
- 미인증: [未](https://hanja.dict.naver.com/search?query=%EC%95%84%EB%8B%90%EB%AF%B8), Not enough
	- `미인증`은 **부족하다**의 뜻이 강하다. 즉, 권한이 부족한 상태다

---

**결론적으로 `401`은 인증이 안돼 자원을 이용할 수 없는 상태고, 의미상 `unauthenticated`가 더 정확하다.**

> 커피숍엔 사장과 직원이 근무를 하고 있다.
> 커피숍의 사무실은 **인증**(`authenticated`)된 사람들만 들어갈 수 있다. 
> 일반 사용자들도 커피숍 사무실의 위치는 알지만 사무실의 문을 열 때 사용하는 키, 생체 정보가 없기 때문에 들어갈 수 없다.
> 
> 사무실엔 금고 존재한다. 이것은 **승인**(`authorized`)된 사람들만 열 수 있다. (뒤에 나올 `403` 상태 코드)
> 사무실에 들어온 모든 사람이 금고을 열 수 있는 게 아니고 사장, 매니저등 일부 권한이 있는 사람만 열 수 있다.

### 403 Forbidden
> [403 Forbidden](https://developer.mozilla.org/ko/docs/Web/HTTP/Status/403)
> 클라이언트가 권한이 없기 때문에 작업을 진행할 수 없는 경우
> [forbidden](https://en.dict.naver.com/#/entry/enko/ac2fcf303b2c4cb4aa36c267ff87e70a): 금지된

`403`은 **권한**(`authorized`)에 대한 내용이다. 
`401`의 상태 코드명이 `Unauthorized`라 혼동의 여지가 있으나, **권한**에 대한 내용이다.
인증된 클라이언트가 권한이 없는 자원에 접근할 때 응답하는 상태 코드다.

> 인증과 권한은 헷갈릴 수 있으니 다시 예를 들어 설명한다.
> 
> 웹 사이트를 돌아다니다 보면, `로그인이 필요합니다`라는 경고 창을 볼 수 있다.
> 특정 파일을 다운 받으려고 게시물을 눌렀을 때라고 생각해보자.
> 이 상황은 인증이 안된 상황이다. 서버 입장에서 현재 사용자가 누군지 모르기 때문에 접근을 차단한 것이다.
> 
> 힘겹게 회원가입을 해서 해당 게시물에 들어갔다.
> 인증이 되었기 때문에 게시물을 읽을 수 있는 것이다.
> 이제 해당 게시물에 첨부된 파일을 받기 위해 다운로드 버튼을 눌렀다.
> `10 level부터 다운받을 수 있습니다.`라는 경고창이 뜬다.
> 이 상황이 권한이 없는 상태다. 서버 입장에서 현재 요청자가 누군지는 알지만, 해당 사용자는 요청한 자원에 접근할 권한이 없다고 판단하여 접근을 차단한 것이다.

### 404 Not Found
> [404 Not Found](https://developer.mozilla.org/ko/docs/Web/HTTP/Status/404)
> 클라이언트가 요청한 자원이 존재하지 않다.

IT 분야에서 일하지 않는 사람이라도 인터넷을 하다 보면 `404` 오류는 한 번쯤 만나봤을 것이다.
지금 당장 https://www.google.com/hakhak2 이렇게 존재하지 않는 페이지를 방문하면 `404` 오류가 발생한다.
![404 not found](https://user-images.githubusercontent.com/9030565/74630267-8f0e0f00-519d-11ea-87ad-b13bb9c41c64.png)


브라우저(`클라이언트`) 입장에선 자원이 웹 페이지 경로고 존재하지 않는 경로(`자원`)를 요청했기 때문에 `404` 상태 코드를 응답했다.

REST API에선 크게 두 가지 경우에서 `404` 상태 코드를 응답한다.
1. **경로**가 존재하지 않음
1. **자원**이 존재하지 않음

> REST 입장에서 생각을 해야 한다.
> 대부분 API 프레임워크에선 `경로`(라우팅)에 대한 에러 처리를 해준다.
> 즉, 존재하지 않은 `경로`는 쉽게 `404`로 응답할 수 있다.
> `GET /users/abc/def/wow` 경우 아예 존재하지 않는 `경로`다
> 
> 그러나, `자원`의 경우는 개발자가 처리해줘야 한다.
> `PUT /users/1` 경우 `/users/:id`로 존재하는 경로다. 또한 `:id`엔 숫자 값이 오고 요청도 숫자기 때문에 `400 Bad Request`도 아니다. 이때 서버는 ID `1`을 갖는 사용자가 있는지 먼저 확인을 해야 한다.
> 이것이 **자원**에 대한 존재 여부를 파악하는 것이다.
> 만약, 존재 여부를 파악하지 않고 그대로 진행을 하면 후속 작업에서 오류가 발생할 가능성이 있고 이것은 `5XX` 오류로 이어질 수 있다.
> 즉, 존재하는 `경로`에 대한 요청이라도 `자원`이 존재하는지 파악 후, 존재하지 않는다면 `404` 상태 코드로 응답해야 한다.

사실 위에서 설명한 **경로, 자원**이 정확한 용어와 의미는 아니다. 
REST에선 URI가 자원이기 때문에 경로가 곧 자원이다.

`404` 오류의 경우 두 가지 모두 확인을 해야 한다는 설명을 위한 표현이다.

### 405 Method Not Allowed
> [405 Method Not Allowed](https://developer.mozilla.org/ko/docs/Web/HTTP/Status/405)
> 클라이언트의 요청이 허용되지 않는 메소드인 경우

메소드란 `POST, GET, PUT, DELTE` 등 [HTTP Method](https://developer.mozilla.org/ko/docs/Web/HTTP/Methods)를 말한다.
즉, **자원**(`URI`)은 존재하지만 해당 자원이 지원하지 않는 메소드일 때 응답하는 상태 코드다.

---

**`405` 상태 코드는 [OPTIONS](https://developer.mozilla.org/ko/docs/Web/HTTP/Methods/OPTIONS) 메소드와 HTTP header의 [Allow](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Allow)와 연관되어있다.**

`OPTIONS`는 API가 허용하는 메소드가 어떤 것들이 있는지 확인하는 메소드다.
`405`오류를 사전에 방지하기 위한 용도에 주로 쓰인다.
이 때 응답 HTTP header의 `Allow`에  지원하는 메소드를 나열하여 응답한다.

[RESTful API 설계 가이드](https://sanghaklee.tistory.com/57)에서도 말한 내용이지만 완성도 높은 API를 위해 제공하길 추천한
다.(최근 몇몇 API 프레임워크에선 자동으로 허용되지 않는 메소드에 대해 `405` 상태 코드와 `Allow` 헤더를 응답하기도 한다.)

```http
OPTIONS /users/1 HTTP/1.1
```
```http
HTTP/1.1 200 OK
Allow: GET,PUT,DELETE,OPTIONS,HEAD
```
`/users/1` 자원은 `POST` 메소드를 제공하지 않는다는 정보를 확인할 수 있다.

---

`405` 상태 코드는 `404` 상태 코드와 혼동될 수 있기 때문에 규칙을 잘 정해야 한다.
> `POST /users/1`의 경우 API 설계에 따라 `404`, `405`로 응답할 수 있다.
> `/users/:id` 은 GET, PATCH, DELETE 메소드는 허용되고 POST은 불가한 URI이다.
> 
> GET, PATCH, DELETE의 경우, **1** 사용자가 없는 경우엔 `404`로 응답한다.
> ```http
> GET /users/1 HTTP/1.1
> ```
> ```http
> HTTP/1.1 404 Not Found
> ```
> ---
> 그러나, POST 요청의 경우 제공하지 않는 메소드기 때문에 `405`로 응답하는 것이 옳다.
> ```http
> POST /users/1 HTTP/1.1
> ```
> ```http
> HTTP/1.1 405 Method Not Allowed
> Allow: GET,PUT,DELETE,OPTIONS,HEAD
> ```

### 409 Conflict
> [409 Conflict](https://developer.mozilla.org/ko/docs/Web/HTTP/Status/409)
> 클라이언트의 요청이 서버의 상태와 충돌이 발생한 경우

**충돌**이라는 것은 매우 추상적이다. 앞에서 알아본 `400, 401, 403, 404, 405` 상태 코드들은 사용이 꽤 명확하다.
하지만, 충돌이라는 것은 정의하기 나름이다.
그렇기 때문에 필자는 `400, 401, 403, 404, 405` 상태 코드에 속하기 애매한 오류의 상황들을 `409`로 응답한다.

물론, 나름의 규칙을 갖고 `409`로 응답한다.
**해당 요청의 처리 중 [비지니스 로직](https://ko.wikipedia.org/wiki/%EB%B9%84%EC%A6%88%EB%8B%88%EC%8A%A4_%EB%A1%9C%EC%A7%81)상 불가능하거나 모순이 생긴 경우**

> 예를 들어, 다음의 경우
> ```http
> DELETE /users/1 HTTP/1.1
> X-TOKEN: password
> ```
> 
> - **자원**(URI) `/users/1`에 존재하는 메소드고 `Not 405`
> - `/users/:id`에서 **:id**가 유효한 형식이고 `Not 400`
> - **1** 사용자도 존재하고 `Not 404`
> - 헤더의 인증(X-TOKEN)도 정확하고 `Not 401`
> - 삭제 권한도 있는 경우 `Not 403`
> 
> 클라이언트의 삭제 요청은 받아들여져서 `200` 혹은 `204`로 응답해야 하지만,
> **사용자의 게시물이 존재하는 경우 사용자를 삭제할 수 없다**는 비지니스 로직이 있을 수 있다
> 
> 이렇게 API 사용에 있어 비지니스 로직상 모순이 발생하여 처리가 불가능한 경우 응답하는 상태 코드다.
> ```http
> HTTP/1.1 409 Conflict
> ```

`400` 오류 상황과 마찬가지로 응답 시 오류의 원인을 알려야 한다.
추가적으로 [HATEOAS](https://en.wikipedia.org/wiki/HATEOAS)를 이용해 클라이언트가 다음 상태로 전이될 수 있는 링크를 함께 응답하면 좋다.
```http
HTTP/1.1 409 Conflict
{
    "message" : "First, delete posts"
    "links": [
        {
            "rel": "posts.delete",
            "method": "DELETE",
            "href": "https://api.rest.com/v1/users/1/posts"
        },
    ]
}
```

### 429 Too Many Requests
> [429 Too Many Requests](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/429)
> 클라이언트가 일정 시간 동안 너무 많은 요청을 보낸 경우

비정상적인([DoS attack](https://ko.wikipedia.org/wiki/%EC%84%9C%EB%B9%84%EC%8A%A4_%EA%B1%B0%EB%B6%80_%EA%B3%B5%EA%B2%A9), [Brute-force attack](https://ko.wikipedia.org/wiki/%EB%AC%B4%EC%B0%A8%EB%B3%84_%EB%8C%80%EC%9E%85_%EA%B3%B5%EA%B2%A9)) 방법으로 자원을 요청하는 경우 응답한다. 

DoS는 [가용성](http://www.ktword.co.kr/abbr_view.php?m_temp1=1100)에 대한 공격이고 Brute-force는 [기밀성](http://www.ktword.co.kr/abbr_view.php?m_temp1=2243)에 대한 공격이다. 하지만, 서버 입장에서 두 공격 모두 가용성에 피해를 입을 수 있다.
서버가 감당하기 힘든 요청이 지속적으로 들어오면 서버는 해당 요청을 처리하기 위해 다른 작업을 처리하지 못할 수 있다.

 `429` 상태 코드는 이러한 경우 일정 시간 뒤 요청할 것을 나타내는 것이다. 따라서 다음과 같이 HTTP hearder [Retry-After](https://developer.mozilla.org/ko/docs/Web/HTTP/Headers/Retry-After)을 이용한다.
 
```http
HTTP/1.1 429 Too Many Requests
Retry-After: 3600
```
- 클라이언트는 `3600`초 후에 다시 해당 자원에 대한 작업을 요청할 수 있다.

 ---

> 두 가지 상황을 예로 설명한다.
> 
> **Case 1 기밀성에 대한 공격**
> ```http
> POST /login HTTP/1.1
> {
>	"name": "hak"
>	"password": "iwillhackU"
> }
> ```
> 해커는 사용자의 비밀번호를 알아내기 위해 `POST /login` API에 `password`를 무차별로 대입하면서 요청할 수 있다.
> 서버 입장에선 자원의 **기밀성**(Confidentiality)에 피해를 입을 수 있는 공격이면서, 이러한 무차별 요청으로 다른 요청을 처리할 수 없거나 처리가 늦을 수 있는 **가용성**(Availability)에 피해를 입을 수 있다.
> 서버는 이러한 공격에 대비해 **인증** API의 경우 각 **클라이언트**는 `n 시간 동안 n 회만 요청 가능`하다는 룰을 정하고 이것을 초과하면 `429` 상태 코드를 응답해야 한다.
> 
> ---
> 
> **Case 2 가용성에 대한 공격**
> ```http
> GET /users HTTP/1.1
> ```
> 해커는 시스템에 과부하를 주기 위해 특정 API에 지속적으로 요청을 보낼 수 있다. 
> 해커의 비정상적인 요청으로 인해 실제로 서비스를 받아야 할 정상적인 사용자가 서비스를 받지 못하는  **가용성**(Availability)에 피해를 입을 수 있다.
> 서버는 이러한 공격에 대비해 **클라이언트**의 요청에 대해 `n 시간 동안 n 회 이상 요청`한다면 그 이후의 요청은 `429` 상태 코드로 응답해야 한다.
> - **사실 `429` 상태 코드로 DoS, DDoS 같은 가용성에 대한 공격을 막을 수 있는 것은 아니다.**
>   - 일단, 서버로 지속적인 공격이 오기 때문에 가용성에 피해를 입는 것은 막을 수 없다.
>   - API 서버는 계속 `429`로 응답하겠지만, 서버 자체에 요청이 계속 오는 것을 막을 순 없다는 뜻이다.
>   - 네트워크 상단에서 해당 IP를 차단하는 등의 조치가 필요하다.
> - 그러면, 가용성에 대한 `429` 처리는 필요 없을까? 그렇지 않다. 네트워크 상단에서 차단하는 것은 시스템 엔지니어 혹은 네트워크 엔지니어가 할 일이다.
> - **개발자는 API 서버의 관점에서 생각해야 한다.** `GET /users`의 경우 요청에 대한 응답을 하기까지 비지니스 로직 중 DB를 조회할 수 있고, 서버의 자원을 사용하는 작업을 할 수도 있다.
> - API 서버가 정의한 범위에서 너무 과도한 요청이라고 생각하면 그러한 작업까지 가기 전에 `429` 상태 코드로 응답하고 끝내야 한다.
> - 또한 REST API는 화면으로만 제어하는 게 아니고 프로그램으로 제어할 수 있기 때문에 동일 요청을 매우 빠르게 보낼 수 있다. **클라이언트가 반복문 실수로 1초에 100번 요청(?)을 보내서 API 서버에 장애가 발생한 경우 클라이언트의 잘못이라고 말할 것인가?**

## 5XX [Server errors](https://en.wikipedia.org/wiki/List_of_HTTP_status_codes#5xx_Server_errors)
`5XX` 상태 코드들은 서버 오류로 인해 요청을 수행할 수 없다는 의미다.

---

클라이언트의 요청은 유효하여 작업을 진행했는데 도중에 오류가 발생한 경우다.

`404` 오류와 마찬가지로 인터넷을 하다 보면 `500, 502, 503` 등의 오류를 만나봤을 거다.

API 서버의 응답에서 `5XX`오류가 발생해서는 안된다.
보통 개발 과정에서 유효하지 않은 요청을 사전에 처리하지 않은 경우(`400`)에 많이 발생한다.

---

> 특히 `500` 오류는 개발자의 실수로 발생할 여지가 크다.
> ```http
> POST /users HTTP/1.1
> {
>     "name": 123
> }
> ```
>  요청에 대해 `4XX` 오류를 발생시킬 가능성이 있는데 사전에 확인 작업을 하지 않은 경우
>  - 파라미터 필수 값, 유효성 확인 없이 비지니스 로직 진행하는 경우
>  - 외부 API에서 받은 객체를 확인하지 않고 비지니스 로직 진행하는 경우
>  
>  ---
>  
> ```python
> # python
>  params = request.json
>  create(params['name'])
> ```
>  params에 `name`이 존재하는지 판단하지 않고 개발한 경우
>  클라이언트의 요청이 무조건 유효할 것이라 판단하고 개발하는 경우 오류를 발생시킬 수 있다.

**API를 사용하는 클라이언트에게 `5XX` 상태 코드는 나타내지 말아야 한다!**

최신의 웹 애플리케이션 프레임워크는 자체 웹서버를 내장하고 있어서 웹서버(Apache, Nginx) 없이 운영할 수 있다.

그러나, 보통 운영 레벨에서 이렇게 하는 경우는 드물고 앞에 웹서버를 두고 웹 애플리케이션을 연결해서 운영한다.

따라서, 상단의 웹서버(Apache, Nginx)에서 발생하는 어쩔 수 없는 오류를 제외하고 API에선 `5XX` 상태 코드가 응답되선 안된다.

**API 레벨에선 완벽한 예외처리를 통해 5XX 서버 오류 상태 코드를 방지해야 한다.**

### Conclusion

REST API 관점에서 바라보는 HTTP 상태 코드에 대해서 알아봤다.
상태 코드는 API 서버를 개발하면서 제대로 신경 쓰지 못하는 부분 중 하나다.
API 서버 입장에선 상태 코드는 `클라이언트의 요청에 응답한다`라는 목적을 위한 수단이기 때문에 정확하지 않아도 된다고 생각하는 것 같다.
하지만, RESTful API를 개발하고 싶다면 응답 목적에 맞는 상태 코드를 보내주는 것이 좋다.

[RESTful API 설계 가이드](https://sanghaklee.tistory.com/57)의 제목을 보면 `REST API 설계 가이드`가 아닌 **RESTful API 설계 가이드**인 것을 확인할 수 있다.
`RESTful`이란 REST 설계 원리로 구성된 시스템을 말한다. (RESTful의 공식적인 표준은 없다)
공식적인 표준이 없기 때문에 REST API를 잘 모른다면 https://restfulapi.net의 내용을 토대로 REST API를 설계 구축해보자

한 가지 더, 소프트웨어 개발과 별개로 뭐든지 중간은 하려면 **일단, 하지 말라는 것을 안 하면 된다**.
Best Practice가 없다면 Worst를 피하자.([Anti-pattern](https://en.wikipedia.org/wiki/Anti-pattern))

**[Five Clues That Your API isn't RESTful](https://dzone.com/articles/five-clues-your-api-isnt)(당신의 API가 Restful 하지 않은 5가지 증거)**
