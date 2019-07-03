
# [JavaScript] Null check, 빈 값 체크 v2019

### Introduction

필자의 블로그 중 가장 조회수가 높은 글이다.

세번째 게시글인 만큼 블로그 초장기에 작성한 내용이다.

근 일년간 블로그 활동에 소홀했었다. 다시 개발 블로그를 열심히 하기로 마음을 먹고 블로그 통계를 확인했다.  
  
어떤 게시글들이 인기 있는지 확인해보고 싶어서였다.

## isEmpty

`gist`에 공유한

## isEmpty

`gist`에 공유한 [isEmpty](https://gist.github.com/SangHakLee/4da6159a7a08cdd12132">isEmpty는 문제점이 있다.  `false`)는 문제점이 있다.  `false`  입력 값에 대해선 의도와 다른 동작을 한다.

그래서 최초에 의도한 기능대로 동작하는  `` `isEmpty</code>`를 다시 작성했다.``

`is-`

`is-`로 시작하는 함수 관리하기 위해  `is``is`라는 NPM 모듈로 만들었다.

### is

### is

[https://sanghaklee.github.io/is">](https://sanghaklee.github.io/is

```javascript
is.empty('') // true

```

### 빈 값

`isEmpty`는 파라미터 값이 비었는지 확인한다.  **빈 값**이라 정의한 값들은 아래와 같다.

-   [String](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/String)
    -   `''`
    -   `new String()`
    -   **``**

### 빈 값

`isEmpty`는 파라미터 값이 비었는지 확인한다.  **빈 값**이라 정의한 값들은 아래와 같다.

-   [String](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/String)
    -   `''`
    -   `new String()`
    -   **``**  [Template literals  
        ](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Template_literals%22%3E)
    -   `${''}`  [Template literals  
          
        

-   `${''}`  [Template literals  
    ](https://stackedit.io/](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Template_literals)
-   [Array](https://stackedit.io/)-%20[Array](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array)
    -   `[]`
    -   `new Array()`
-   [Object](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Object)
    -   `{}`
    -   `new Object()`
    -   `new Proxy({}, {})`  [Proxy](https://stackedit.io/)-%20%60[]%60-%20%60new%20Array()%60-%20[Object](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Object)-%20%60%7B%7D%60-%20%60new%20Object()%60-%20%60new%20Proxy(%7B%7D,%20%7B%7D)%60%20[Proxy](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Proxy)
-   [null](https://stackedit.io/)-%20[null](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/null)
    -   `null`
-   [undefined](https://stackedit.io/)-%20%60null%60-%20[undefined](https://developer.mozilla.org/ko/docs/Web/JavaSReference/Global_Objects/undefined)
    -   `undefined`

### 채워진 값

```javascript
1
'string'
[1]
{a:1}

```

-   의심의 여지가 없는 명백하게 채워진 값들이다.

```javascript
true
false

```

-   `boolean`도 값이다. 특히  `false`도 값이다.

#### Object

`Object`는 까다롭다.

```javascript
new Boolean() // Boolean{false}
false // false

```

typeof  new  Boolean()  // object  
typeof  false  // boolean

typeof  new  Boolean()  !=  typeof  false  
new  Boolean()  ==  false  

[https://stackoverflow.com/questions/17256182/what-is-the-difference-between-string-primitives-and-string-objects-in-javascrip](https://stackoverflow.com/questions/17256182/what-is-the-difference-between-string-primitives-and-string-objects-in-javascrip)

```javascript
new Boolean() // Boolean{false}, == false
new Number() // Number{0}, == 0
new Date() // Thu Jun 27 2019 00:59:56 GMT+0900 (한국 표준시)
new RegExp() // /(?:)/
new Error() )
	- `undefined`

```

### 채워진 값

```javascript
1
'string'
[1]
{a:1}

```

-   의심의 여지가 없는 명백하게 채워진 값들이다.

```javascript
true
false

```

-   `boolean`도 값이다. 특히  `false`도 값이다.

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

[https://stackoverflow.com/questions/17256182/what-is-the-difference-between-string-primitives-and-string-objects-in-javascrip](https://stackoverflow.com/questions/17256182/what-is-the-difference-between-string-primitives-and-string-objects-in-javascrip)

```javascript
new Boolean() // Boolean{false}, == false
new Number() // Number{0}, == 0
new Date() // Thu Jun 27 2019 00:59:56 GMT+0900 (한국 표준시)
new RegExp() // /(?:)/
new Error() // Error at &lt;<anonymous&gt;</span>
</code></pre>
<ul>
<li>위 객체들은 자명하게 값을 리턴</li>
</ul>
<hr>
<p><a href="https://github.com/SangHakLee/is/blob/master/test/datas/empty.js">Test Data-set</a>을 보면 필자가 어떤 값들을 <strong>빈 값</strong>이라 보고 어떤 값들을 <strong>채워진 값</strong>이라 보는지 알 수 있다.</p>
<h2 id="u>

```

-   위 객체들은 자명하게 값을 리턴

----------

[Test Data-set](https://github.com/SangHakLee/is/blob/master/test/datas/empty.js)을 보면 필자가 어떤 값들을  **빈 값**이라 보고 어떤 값들을  **채워진 값**이라 보는지 알 수 있다.

## Underscore.js–l & Lodash">

[Underscore.js & Lodash

[Underscore.js](https://underscorejs.org/).  [Lodash](https://lodash.com/)에  `isEmpty`가 이미 존재한다.

[map](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/map">map 을 두고  [_.map_](https://stackedit.io/)%20%EC%9D%84%20%EB%91%90%EA%B3%A0%20[_.map](https://underscorejs.org/#map)) 을 쓸 정도로 Undederscore.js 성애자인 필자가  [.isEmpty](https://stackedit.io/[_.isEmpty](https://underscorejs.org/#isEmpty)) 를 사용하지 않는 이유가 있다.

필자가 원하는 것은  ****파라미터의 값이 비었는지 확인****하는 것이다. 하지만,  `` `_.isEmpty</code>`는 필자의 의도와 다르게 동작한다.``

-   `boolean`  return  **true**
    -   _.isEmpty(true)
-   `number`  return  **true**
    -   _.isEmpty(1)
-   `object created with new, except new Object()`  **true**
    -   _.isEmpty(new Date())

-   `boolean`  return  **true**
    -   _.isEmpty(true)
-   `number`  return  **true**
    -   _.isEmpty(1)
-   `object created with new, except new Object()`  **true**
    -   _.isEmpty(new Date())
-   `function</code>`  return  **true**

-   _.isEmpty(function() {})

필자는 앞에서  **빈 값****true** - _.isEmpty(function() {})

필자는 앞에서  **빈 값**이라 정의한 파라미터를 제외한 모든 경우를  **채워진 값****채워진 값**으로 판단하고 싶었다.  
  
JavaScript로 데이터를 다루다 보면 이렇게 처리해야 하는 경우가 꽤 생긴다.
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTMwMTU4NzU3Ml19
-->