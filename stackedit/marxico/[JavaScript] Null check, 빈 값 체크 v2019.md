
# [JavaScript] Null check, 빈 값 체크 v2019

###  Introduction
필자의 블로그 중 가장 조회수가 높은 글이다.

세번째 게시글인 만큼 블로그 초장기에 작성한 내용이다.

근 일년간 블로그 활동에 소홀했었다. 다시 개발 블로그를 열심히 하기로 마음을 먹고 블로그 통계를 확인했다.

어떤 게시글들이 인기 있는지 확인해보고 싶어서였다.


## isEmpty
`gist`에 공유한 [isEmpty](https://gist.github.com/SangHakLee/4da6159a7a08cdd12132)는 문제점이 있다.  `false` 입력 값에 대해선 의도와 다른 동작을 한다.

그래서 최초에 의도한 기능대로 동작하는 `isEmpty`를 다시 작성했다.

`is-`로 시작하는 함수 관리하기 위해 `is`라는 NPM 모듈로 만들었다.

### is
[https://sanghaklee.github.io/is](https://sanghaklee.github.io/is)
```javascript
const empty = (value) => {
	if (value === null) return true
	if (typeof value === 'undefined') return true
	if (typeof value === 'string' && value === '') return true
	if (Array.isArray(value) && value.length < 1) return true
	if (typeof value === 'object' && value.constructor.name === 'Object' && Object.keys(value).length < 1 && Object.getOwnPropertyNames(value) < 1) return true
	if (typeof value === 'object' && value.constructor.name === 'String' && Object.keys(value).length < 1) return true // new String()

	return false
}
```

### 빈 값
`isEmpty`는 파라미터 값이 비었는지 확인한다. **빈 값**이라 정의한 값들은 아래와 같다.

- [String](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/String)
	- `''`
	- `new String()`
	- **``** [Template literals
](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Template_literals)
	- `${''}` [Template literals
](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Template_literals)
- [Array](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array)
	- `[]`
	- `new Array()`
- [Object](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Object)
	- `{}`
	- `new Object()`
	- `new Proxy({}, {})` [Proxy](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Proxy)
- [null](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/null)
	- `null`
- [undefined](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/undefined)
	- `undefined`


### 채워진 값
```javascript
1
'string'
[1]
{a:1}
```
- 의심의 여지가 없는 명확하게 채워진 값들이다. 

```javascript
true
false
```
- `boolean`도 값이다. 특히 `false`도 값이다.

#### Object
`Object`는 까다롭다. 

```javascript
new Boolean() // Boolean{false}
false // false

typeof new Boolean() // object
typeof false // boolean

typeof new Boolean() != typeof false
new Boolean() == false
```
- `new` 로  생성하면 타입은 `object`지만 값으로 비교할 땐 false이다.
	-  `new`로 생성하면 Object
	-  `false` 등 리터럴로 생성하면 Primitive
- [자바스크립트의 원시 타입(Primitive Type)](https://velog.io/@jakeseo_me/%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8-%EA%B0%9C%EB%B0%9C%EC%9E%90%EB%9D%BC%EB%A9%B4-%EC%95%8C%EC%95%84%EC%95%BC-%ED%95%A0-33%EA%B0%80%EC%A7%80-%EA%B0%9C%EB%85%90-2-%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8%EC%9D%98-%EC%9B%90%EC%8B%9C-%ED%83%80%EC%9E%85Primitive-Type-%EB%B2%88%EC%97%AD)
- https://stackoverflow.com/questions/17256182/what-is-the-difference-between-string-primitives-and-string-objects-in-javascript

```javascript
new Boolean() // Boolean{false}, == false
new Number() // Number{0}, == 0
new Date() // Thu Jun 27 2019 00:59:56 GMT+0900 (한국 표준시)
new RegExp() // /(?:)/
new Error() // Error at <anonymous>
```
- 위 객체들은 자명하게 값을 리턴
- JavaScript는 6가지 Primitive를 제외하면 모두 Object
	- `new Object()`를 제외한 모든 Object는 값이 채워졌다고 판단
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTUzMDczNDIwNCwxMjM0NDI2ODk1LDE1Nz
MxMjY3MDksLTMwMTU4NzU3Ml19
-->