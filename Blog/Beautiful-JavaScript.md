# [ECMAScript6 / ES6] 아름다운 JavaScript를 위한 ES6 

### Introduction
그럼에도 불구하고, ES6를 써야하는 이유?

![enter image description here](http://cfile235.uf.daum.net/R400x0/237ACD3D5666ACF13155EA)
"아름답지 않으면 살 의미가 없어" - 하울의 움직이는 성


**아름다운 코드에 대한 열망**

> Node.js를 통해 JavaScript를 배웠지만, JavaScript의 복잡한 Callback 구조는 코드를 보기 힘들게 했다.
> 다른 스크립트 언어에서 제공하는 깔끔한 문법이 있으면 좋다고 생각했는데, ES6가 이런 요구사항을 만족 시켜줄거라 생각해서 새로운 Node.js 프로젝트에 ES6를 도입했다. (당시 Node.js v6.10.x 의 ES6 coverage가 99%였기 때문에 사용가능했다.)

## Background
ES5, ES6의 대한 설명을 바로 하는 것보다 JavaScript의 동작 원리를 알고 ES5, ES6를 알면 좋을 것 같다.

### JavaScript Engine

- **V8** - Google, Opera
- Rhino - Mozilla
- JavascriptCore - Safari
- [List of ECMAScript engines](https://en.wikipedia.org/wiki/List_of_ECMAScript_engines)

> Javascript Engine은 JavaScript로 작성된 코드를 해석하고 실행하는 인터프리터

#### Interpereter
프로그래밍 언어의 소스 코드를 바로 실행하는 컴퓨터 프로그램 또는 환경을 말한다. 

#### JIT Compile
JIT 컴파일(just-in-time compilation) 또는 동적 번역(dynamic translation)은 프로그램을 실제 실행하는 시점에 기계어로 번역하는 컴파일 기법이다. 
실행 시점에서 기계어 코드를 생성하면서 그 코드를 캐싱하여, 같은 함수가 여러 번 불릴 때 매번 기계어 코드를 생성하는 것을 방지한다.

> 우리가 JavaScript 코드를 웹에 올리면 브라우저의 엔진(e.g. V8)이 코드를 해석하고 실행한다. 
> 각 엔진마다 JavaScript 코드를 해석하는 방법과 능력 등이 다르다.
> 그래서 ES6, ES7이 발표되도 IE 같은 좋은 브라우저에선 바로 사용할 수 없는 것이다.
> ES6에 새로 추가된 문법을 해석할 능력이 없는 엔진인 것이다.
> Python 2.x 와 3.x의 문법이 조금씩 다른 것과 비슷하다.

## Beautiful Code
> 아래의 ES6부터 추가된 문법은 JavaScript 코딩 중 가장 많이 사용하는 문법이다.
> 또한, 이러한 sugar-code 스러운 느낌이 마음에 들어 ES6를 도입했다.

## Destructuring Assignment
비구조화 할당
https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment

### ES5
```javascript
var name = req.body.name
var age = req.body.age
var email = req.body.email
```

### ES6
```javascript
const {name, age, email} = req.body
```

---
<br>

##Object Initialize - Property Shorthand
객체 초기자
https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Operators/Object_initializer

### ES5
```javascript
var name = 'hak'
var age = 27
var email = 'code.ryan.lee@gmail.com'

var datas = {
	name: name,
	age: age,
	email: email
}
```

### ES6
```javascript
let name = 'hak'
let age = 27
let email = 'code.ryan.lee@gmail.com'

let datas = {name, age, email}
let datas2 = {username: name, age, email}
```

---
<br>

##Template Literals

https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Template_literals

### ES5
```javascript
var username = req.body.username
if ( !username ) {
	throw "'username' must required. Your input: " + username  + "."
}
```

### ES6
```javascript
let {username} = req.body
if ( !username ) {
	throw `'username' must required. Your input: ${username}.`
}
```

---
<br>

##Default Parameters
기본 매개변수
https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Functions/Default_parameters

### ES5
```javascript
var greeting = function(username, date, message) {
	var username = typeof username !== 'undefined' ? username : 'anonymous'
	var date     = typeof date     !== 'undefined' ? date     : new Date()
	var message  = typeof message  !== 'undefined' ? message  : 'hello'
	
	return message + ' ' +  username + ' at ' + date
}
```

### ES6
```javascript
const greeting = (username='anonymous', date=new Date(), message='hello') => {
	return `${message} ${username} at ${date}`
}
```

---
<br>

## Promise - Co - async/await

#### Promise
ECMAScript 2015/  ES6 에 Promise가 추가되기 전에 ES5 환경에서는 아래의 모듈들을 사용해서 Promise를 사용했다.
[bluebird](http://bluebirdjs.com/docs/getting-started.html), [Q](https://github.com/kriskowal/q)

그러나, ECMAScript 2015에서는 [Promise](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Promise)가 표준 내장 객체로 추가되서 외부 모듈 없이 사용할 수 있다.
필자는 표준 내장 객체를 사용하는 것을 추천한다. 물론 다른 모듈들을 사용하면 사용상 편의성이 있다.
**튜닝의 끝은 순정이라고..**

Promise를 설명하고자 하는게 아니다.  ( [자바스크립트 프라미스: 소개](https://developers.google.com/web/fundamentals/getting-started/primers/promises?hl=ko) )
모든 비동기 함수는 Promise를 반환하게 만드는게 핵심이다.

```javascript
const getName = (user_id) => {
    return new Promise( (resolve, reject) => {
        if ( !user_id ) {
            return reject('user_id must be required.')
        }

        Model.select('table_users', user_id)
        .then( result => {
            resolve( result.username )
        })
        .catch( err => {
            reject( err )
        })
    });
}
```

---

#### Co
##### Generator
[function*](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Statements/function*)
ECMAScript 2015 / ES6 에 추가된 generator function

##### 비동기 코드를 아름답게 제어하는 단계
Callback - Promise - Generator(Co) - async/await

![enter image description here](https://cdn-images-1.medium.com/max/800/1*hkb9fD1Q-LFVRkZnPdFhXA.png)
[ES 5-6-7: From Callbacks to Promises to Generators to Async/await](https://medium.com/@rdsubhas/es6-from-callbacks-to-promises-to-generators-87f1c0cd8f2e)



현재 필자는 Co 단계에서 비동기 코드를 제어하고 있다.

아름다운 비동기 코드를 만들기 위해 검색을 많이 했는데, [ES6의 제너레이터를 사용한 비동기 프로그래밍](http://meetup.toast.com/posts/73)  블로그를 보고 현재의 비동기 코드가 만들어졌다.

#####  NPM Co
[co](https://www.npmjs.com/package/co)

Co 모듈을 처음 사용할 때는 거의 신세계였다.
JavaScript 코드를 Java, Python, PHP 처럼 동기식으로 표현할 수 있었다.
가장 좋은 점은 try-catch를 아름답게 사용할 수 있었다.

Co 모듈을 알게된 후로 모든 비동기 함수는 Co를 활용한 Generator 함수로 만들었다.

Co 모듈은 Promise, Array, Object, Generator, ... 을 반환하는 함수에 yield 구문을 사용할 수 있다고 나와있다.
> The yieldable objects currently supported are:
> - promises
> - thunks (functions)
> - array (parallel execution)
> - objects (parallel execution)
> - generators (delegation)
> - generator functions (delegation)
> 
> https://www.npmjs.com/package/co#yieldables


그래서 위의 Promise 설명에서 모든 비동기 함수는 Promise를 반환한다는 규칙을 정한 것이다.

Co 모듈의 반환 규칙 한 가지 때문에 이런 규칙을 정한 것은 아니다.
바로 끝판왕 async/await 함수로 리팩토링 할 때 쉽게하기 위한 큰 그림이었다.


#### async/await
[async function](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Statements/async_function)

링크의 Description에 다음과 같은 설명이 있다.
> async함수가 호출되어 졌을때, 이 함수는 Promise을 반환한다. 

이래서 처음 Promise 설명에서 필자가 만드는 모든 비동기 함수는 Promise를 리턴한다는 규칙을 만든 것이다.

필자가 개발하던 당시에는 ECMAScript 2016 / ES7이 가장 최신 스펙이었다. ( 2017.04)

위에서 설명한 큰 그림은 ECMAScript 2017이  Release 되면 모든 Co-Generator 함수를 async/await로 바꾸려는 계획이었다.

```javascript
// ES6 - Generator
const bigPicture = function* () {
	let awesome = yield beautiful()
}

// ES8 - async/await
const bigPicture = async function() {
	let awesome = await beautiful()
}
```
`*` -> `async` 
`yield` -> `await`

그리고 2017.06에 ECMAScript 2017 / ES8 이 나왔고, 여기에 async function이 추가 되었다.

이제 Node.js를 v8.x.x로 올리고 ES8을 아름답게 사용할 수 있다.
http://node.green/#ES2017

---
<br>


## if-else
Node.js의 기본 내장 함수는 [Error-First Callback](http://fredkschott.com/post/2014/03/understanding-error-first-callbacks-in-node-js/) 규칙을 따른다.
비동기/동기 함수든 이 규칙으로 로직을 만들면 **아름다운 코드**를 만들 수 있다.
아래의 예가 당연하게 보이지만, legacy 코드들을 보면 if-else에서 Error-First 규칙을 적용하지 않아 보기 힘든 코드들이 많다.

### Bad case - Non Error First
```javascript
const error = (name, age, email) => {
    if ( name && name.length > 3 ) {
        // something
        // ...
        if ( age < 30 ) {
            // something
            // ...
            if ( /이메일정규식/.test(email) ) {
                // something
                // ...
                return true
            } else {
                return false
            }
        } else {
            return false
        }
    } else {
        return false
    }
}
```

### Good case - Error First
```javascript
const error = (name, age, email) => {
    if ( !name || name.length < 4 ) {
        return false
    }
    // something
    // ...

    if ( age >= 30 ) {
        return false
    }
    // something
    // ...

    if ( !/이메일정규식/.test(email) ) {
        return false
    }
    // something
    // ...
    
    return true
}
```

기존 코드를 바꿀 때 변경되는 조건문은 이전 조건문과 상호배타적 이어야한다!!!

$$A \cap B = 0, 	(A, B) \subset C$$
```javascript
set = [1, 2, 3, 4];
if ( set > 2 ) // 3,4 
if ( set <= 2) // 1,2
```


<br>

## [underscore.js](http://underscorejs.org/) 
**or [lodash.js](https://lodash.com/)**

<br>


### Conclusion

> 태양처럼 너는 밝고 아리따워
> 여름처럼 너는 밝고 아리따워
> beautiful beautiful
> 찾아봐요 당신만의 아리따움

>  아리따움 - Supreme Team


### References
- [Interpreter and JavaScript Engine](http://huns.me/development/360)
- [ECMAScript® 2015 Language Specification](http://www.ecma-international.org/ecma-262/6.0/)
- [ES6시대의 JavaScript](https://gist.github.com/marocchino/841e2ff62f59f420f9d9)
- [ES6의 제너레이터를 사용한 비동기 프로그래밍](http://meetup.toast.com/posts/73)
- [ECMAScript 6의 generator](https://blog.outsider.ne.kr/979)
- [JavaScript Generators와 Co](https://medium.com/@pitzcarraldo/javascript-generators%EC%99%80-co-5316ee9266f9)
- [ES 5-6-7: From Callbacks to Promises to Generators to Async/await](https://medium.com/@rdsubhas/es6-from-callbacks-to-promises-to-generators-87f1c0cd8f2e)
- [The Node.js Way - Understanding Error-First Callbacks](http://fredkschott.com/post/2014/03/understanding-error-first-callbacks-in-node-js/)
- [Book] -  [ECMAScript 6 두고두고 보는 자바스크립트 표준 레퍼런스 - 김영보](http://www.yes24.com/24/goods/35254611?scode=032&OzSrank=2)
