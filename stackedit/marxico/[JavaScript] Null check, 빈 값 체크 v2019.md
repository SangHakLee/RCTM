# [JavaScript] Null check, 빈 값 체크 v2019

필자의 블로그 중 가장 조회수가 높은 글이다.

세번째 게시글이 만큼 블로그 초장기에 작성한 내용이다.

근 일년간 블로그 활동에 소홀했었다. 다시 개발 블로그를 열심히 하기로 마음을 먹고 블로그 통계를 확인했다.

어떤 게시글들이 인기 있는지 확인해보고 싶어서였다.


## isEmpty
`gist`에 공유한 `isEmpty`는 문제점이 있다.  `false` 입력 값에 대해선 의도와 다른 동작을 한다.

그래서 최초에 의도한 기능대로 동작하는 `isEmpty`를 다시 작성했다.

### 빈 값
`isEmpty`는 파라미터 값이 비었는지 확인한다. **빈 값**이라 정의한 값들은 아래와 같다.

- Number(https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Number)
	- 1
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
아래는 의심의 여지가 없는 명확하게 채워진 값들이다. 
```javascript
1
'string'
[1]
{a:1}
```

`boolean`도 값이다. 특히 `false`도 값이다.
```javascript
true
false
```

`Object`는 까다롭다. 

```javascript
new Boolean() // Boolean{false}
false // false

typeof new Boolean() // object
typeof false // boolean

typeof new Boolean() != typeof false
new Boolean() == false
```
https://stackoverflow.com/questions/17256182/what-is-the-difference-between-string-primitives-and-string-objects-in-javascrip

아래 객체들은 자명하게 값을 리턴
```javascript
new Boolean() // Boolean{false}, == false
new Number() // Number{0}, == 0
new Date() // Thu Jun 27 2019 00:59:56 GMT+0900 (한국 표준시)
new RegExp() // /(?:)/
new Error() // Error at <anonymous>
```


<!--stackedit_data:
eyJoaXN0b3J5IjpbMTY5MzA2OTUyOV19
-->