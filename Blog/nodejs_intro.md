# [Node.js] Node.js 개요

[Introduction](#introduction)
[Simple usage](#simple-usage)
[Best case with Node](#best-case-with-node)
[Worst case With Node](#worst-case-with-node)
[PHP Vs Node](#php-vs-node)

## Introduction

### Node.js
> `Node.js®` is a JavaScript runtime built on Chrome's `V8 JavaScript engine`.
Node.js uses an `event-driven`, `non-blocking I/O model` that makes it lightweight and efficient.

- [V8 JavaScript engine][1]
    -  Open source high-performance JavaScript engine.
    -  V8 compiles and executes JavaScript source code.
    -  C++
    -  https://github.com/v8/v8/wiki
- [event-driven][2]
    - Main loop that listens for events, and then triggers a callback function when one of those events is detected.
    - http://jasonim.me/dev/146
- [non-blocking I/O model][3]
    - Asynchronous I/O
    - The program code running in this thread is still executed synchronously but every time a system call takes place it will be delegated to the event loop along with a `callback function`.
    - `Blocking` that the program execution will stop and wait for the call to finish and return its result.
    - [10k Problem][4]

---

![event-loop-nodejs | 500x0](https://cloud.githubusercontent.com/assets/9030565/22196509/d49716b4-e190-11e6-91c1-75710cc1dbd2.jpg)
http://misclassblog.com/interactive-web-development/node-js

> Node.js는 Google의 V8 엔진에 기반한 서버 side 기술이다. Thread나 별도 Process 대신 비동기 Event 위주 I/O(Input/Output)을 사용하는 고도의 확장성을 가진 시스템으로 간단한 작업을 수행하며, 접근 빈도가 높은 웹 어플리케이션에 적합하다.

![@Blocking | ex-blocking | 300x0](https://cloud.githubusercontent.com/assets/9030565/22273362/b1407e36-e2e3-11e6-8d87-103ffd0cce54.jpg)


![@Non-nlocking | ex-non-block | 300x0](https://cloud.githubusercontent.com/assets/9030565/22273375/c0005432-e2e3-11e6-9022-84650ee24bb3.jpg)


### Misconceptions  of Node.js
1. **Not Web server / framework**
    - 위에서 말했듯이 `Node.js® is a JavaScript runtime`
    - Server side 에서 JS를 사용할 수 있게 만들어주는 JS runtime
    - [Diff JS engine & JS runtime][5]
        -  `Engine` : JavaScript로 작성된 스크립트를 기계 실행 가능하게 변경. 구문 분석 및 [JIT][6] 컴파일
        -  `Runtime` : 실행 중 프로그램에서 사용할 수있는 기본 라이브러리를 제공
        -  Chrome and Node.js therefore share the same engine (Google's V8), but they have different runtime (execution) environments.
2. **Not Mulit thread**
    - Single thread & Event loop
    - Mismatch in multi-core environment
3. **Not made by JavaScript**
    - Made by C/C++
    - Use by JavaScript
    - V8 made by C++

<br>

## Simple usage

### Setup

https://nodejs.org/ko/download/
> 현재 날짜의 LTS(Long Term Support) 버전 다운로드. `2017.01.23 v.6.9.4`

### Simple Server Code

#### ES6
```javascript
// SimpleServer.js
const http = require('http'); // Node.js 기본 내장 모듈

const hostname = '127.0.0.1'; // = localhost
const port = 3000; // 접근 포트

const server = http.createServer((req, res) => {
  res.statusCode = 200; // http 응답 코드. 200: 성공
  res.setHeader('Content-Type', 'text/plain'); // 아래 출력 내용 형태
  res.end('Hello World\n'); // Hello World 출력
});

server.listen(port, hostname, () => { // 이제 서버는 127.0.0.1 호스트의 3000 포트로 들어오는 요청을 기다린다.
  console.log(`Server running at http://${hostname}:${port}/`); // 해당 이벤트(listen) 발생시 터미널에 출력
});
```

> 코드에서 사용된 `const`, `() =>` 등 키워드는 [ES6][7] 의 키워드이다.
Node.js 는 v.4 이상 버전엔서 ES6를 지원한다. Node.js 버전이 v.4 미만이라면 다음과 같이 사용한다.

---

#### ES5
```javascript
// SimpleServer.js
var http = require('http');

var hostname = '127.0.0.1';
var port = 3000;

var server = http.createServer(function (req, res) {
  res.statusCode = 200;
  res.setHeader('Content-Type', 'text/plain');
  res.end('Hello World\n');
});

server.listen(port, hostname, function () {
  console.log('Server running at http://' + hostname + ':' + port + '/');
});
```

> https://babeljs.io/repl 이곳에서 ES6 -> ES5 변환을 테스트할 수 있다.

---

> SimpleServer.js 가 저장된 위치에서
1. $ node SimpleServer.js
1. `Server running at http://127.0.0.1:3000/` 확인
1. [http:/localhost:3000](http:/localhost:3000) 으로 접근하면 Hello World 가 표시된다.

>

<br>

## Best case with Node

- Real time web applications
    - Web socket server
    - Chat apps
    - [socket.io][8]
- API Server
    - RESTful API
    - [Express.js][9]
- Data streaming
    - Callback concept
    - I/O intensive

<br>

## Worst case with Node

- When server request is dependent on `heavy CPU` consuming algorithm/Job.
- Just as a `static` file server.
    - Use [Nginx][10] for static Object   

<br>

## PHP Vs Node

### Coding style - file read

#### PHP - Blocking
```php
<?php
    echo "started \n"; // #1
    $content = file_get_content ("myFile.txt");
    echo "myText says: " + $content; // #2
    echo "finished"; // #3
?>
```
> Before finishing up reading the file it can’t do anything else.
>
PHP 는 file_get_content() 함수가 실행되는 동안 **모든 작업이 멈춘다**.
따라서 코드의 순서대로 echo 문자가 출력된다.

#### Node - Non-blocking
```javascript
var fs = require('fs');
console.log("started \n"); // #1
fs.readFile("myFile.txt",function(error,data){
    console.log("myFile.txt says:" + data); // #3
});
console.log("finished"); // #2
```
> It starts reading the file and assigns a callback to it, and proceeds to the next line. It continues executing the other parts of the program and whenever the `fs` finished reading the myFile.txt, the callback will be called to process necessary statements.
>
Node는 readFile() 함수를 실행하라는 명령을 내리고 **다음 작업을 진행한다**.
따라서 `'finished'`(#2) 가 먼저 출력되고 readFile() 의 작업이 끝난 후 (#3)의 내용이 출력된다.
http://voidcanvas.com/describing-node-js/

<br>

### Modules
#### PHP - [Composer][11]
#### Node - [NPM][12]

<br>

### Performance
#### PHP
> PHP is relatively fast but due to its bottleneck in the file system, database and third-party requests.

#### Node
> Node.js is extremely fast due to its non-blocking I/O mechanism and Google Chrome V8 engine technology.



  [1]: https://developers.google.com/v8/
  [2]: https://en.wikipedia.org/wiki/Event-driven_programming
  [3]: https://en.wikipedia.org/wiki/Asynchronous_I/O
  [4]: https://en.wikipedia.org/wiki/C10k_problem
  [5]: https://www.quora.com/What-is-the-difference-between-javascript-engine-and-javascript-runtime
  [6]: https://ko.wikipedia.org/wiki/JIT_%EC%BB%B4%ED%8C%8C%EC%9D%BC
  [7]: http://www.ecma-international.org/ecma-262/6.0/
  [8]: http://socket.io/
  [9]: http://expressjs.com/
  [10]: https://www.nginx.com/
  [11]: https://getcomposer.org/
  [12]: https://www.npmjs.com/
