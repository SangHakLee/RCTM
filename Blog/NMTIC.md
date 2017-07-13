# Node + Mocha + Travis + Istanbul + Coveralls: 오픈소스 프로젝트 단위 테스트 & 코드 커버리지 (Unit tests & Code coverage)

http://dsernst.com/2015/09/02/node-mocha-travis-istanbul-coveralls-unit-tests-coverage-for-your-open-source-project/
NMTIC

### Introduction
**Unit tests & code coverage metrics**은 여러분이 오픈 소스 프로젝트를 더 쉽게 작업 할 수 있게 도와준다.
첫째로, 프로젝트 소유자인 당신이 프로젝트 안정성에 관심을 갖고 있음을 알려준다.
둘째로, 프로젝트의 contributors들이 패치 버전에 대한 업데이트 요청(PR)을 자신있게 보낼 수 있게 만들어준다.
마지막으로, 프로젝트가 계속해서 오래 쉽게 개발될 수 있도록 도와준다.

## Where to put your test files
만약 테스트 파일이 오직 1개만 필요하다면, `./test.js`라고 파일을 생성하면 된다.
이것은 Mocha 모듈의 기본 파일명명 규칙이다.

만약 테스트 파일이 여러개로 나뉘어져 있다면, `test/` 폴더를 생성하고 `index.js` 파일에서 다른 테스트 파일을 호출해서 사용한다.

> **NOTE :** 최근의 명명 규칙은 `test/user.model.spec.js`, `test/user.ctrl.spec.js`를 많이 사용한다. 
> 폴더 구조는 상관없으나, `{}.spec.js`로 `spec`이라는 단어를 중간에 넣어준다.


만약 테스트 파일을 `test/`가 아닌 다른곳에 놓아도, Mocha 실행시 해당 경로를 지정해서 실행할 수 있다.
그러나, 굳이 그럴 필요가 없기 때문에 기본 경로를 사용한다.

https://github.com/SangHakLee/NMTIC/tree/01-Where-to-put-your-test-files

## Get mocha to run your tests
Mocha 모듈이 설치되어 있지 않다면, 아래의 명령어로 모듈을 설치한다.
```bash
$ npm install -g mocha
```

아래의 명령어를 사용해서 기본 `test` 경로의 테스트 코드를 실행한다.
```bash
$ npm mocha
```

https://github.com/SangHakLee/NMTIC/tree/02-Get-mocha-to-run-your-tests

## Write your tests
`Mocha`는 `describe()`와 `it()`라는 전역 변수로 테스트를 진행한다.
추가적인 의존성을 줄이려면 Node.js의 기본 [assert][1] 모듈을 사용한다.

https://github.com/SangHakLee/NMTIC/tree/03-Write-your-tests

## Setup $ npm test
테스트 케이스를 수행하는 일반적인 방법은 `npm`의 기본 `test` 명령어를 사용하는 것이다.
**package.json**
```json
"scripts" : {
	"test" : "mocha"
}
```

`$ npm test` 를 입력하면 테스트가 진행된다.

또한, Mocha 모듈을 `devDependency`로서 추가하여 사용자가 `$ npm install`을 실행할 떄마다 설치하게 할 수 있다.

```bash
$ npm install mocha --save-dev
```

https://github.com/SangHakLee/NMTIC/tree/04-Setup-%24-npm-test

## Setup TravisCI for automatic builds
![travis ci][2]
[Travis CI][3]는 저장소에 Push 요청시 코드를 자동으로 테스트하고 문제 발생 시 알람을 준다.
Github의 Pull-Request 요청에도 작동하며 공개 저장소 사용시 무료이다.

1. Travis CI에 로그인 후 사용을 원하는 저장소를 [Switch-on][4] 한다.
2. `.travis.yml` 파일을 저장소에 추가한다.

    ```yaml
    language: node_js
    node_js:
    - "stable"
    ```
3. 코드를 Github에 Push한다.

Travis-CI가 Push 요청을 감지하면 자동으로 `npm test` 명령을 수행한다.

https://github.com/SangHakLee/NMTIC/tree/05-Setup-TravisCI-for-automatic-builds

## Get code coverage with Istanbul
다양한 종류의 code coverage 분석 툴이 있지만, [istanbul][5]을 사용한다.

> **NOTE :** Code Coverage란, 

먼저, istanbul을 `devDependency`로 설치한다.

```bash
$ npm install istanbul --save-dev
```

코드 커버리지를 위한 [npm run-script][6]를 작성한다.

**package.json**
```json
"cover": "node_modules/.bin/istanbul cover ./node_modules/mocha/bin/_mocha"
```

이제 `package.json`에 두개의 실행 스크립트가 존재한다.
**package.json**
```json
"scripts": {
    "test": "mocha",
    "cover": "node_modules/.bin/istanbul cover ./node_modules/mocha/bin/_mocha",
},
```

여기서 한가지 이상한 점이 있는데, `mocha` 대신 `_mocha`를 사용했다.
`mocha` 프로세스는 child process로 실행되기 때문에 그냥 사용하면 istanbul이 아래와 같은 에러를 던진다.

> No coverage information was collected, exit without writing coverage information

그러나, `_mocha`로 사용하면 child_process 없이 실행하기 때문에 아래와 같은 코드 커버리지 결과를 얻을 수 있다.

코드 커버리지만 원하면 아래와 같이 명령어를 입력하면 된다.
```bash
$ npm run cover
```

https://github.com/SangHakLee/NMTIC/tree/06-Get-code-coverage-with-Istanbul

## Adding automatic coverage testing with Coveralls
[Coveralls][7]은 공개 저장소에 사용할 수 있는 또 다른 서비스이다.
이것은  코드 커버리지 결과를 공개적으로 보고하면서 Travis-CI를 보완한다.
또한, 커버리지 결과가 큰 폭으로 변하는 경우 사용자에게 알리도록 구성할 수 있다.

1. [Coveralls.io](https://coveralls.io/)에서 계정 생성 후 사용할 저장소를 선택한다.
2. 아래의 npm 모듈을 설치한다.

	```bash
	$ npm install --save-dev coveralls mocha-lcov-reporter
	```
3.  다음과 같이 `package.json`에 추가한다. istanbul의 결과를 Coveralls로 연결한다.
	
	```json
	"coveralls": "npm run cover -- --report lcovonly && cat ./coverage/lcov.info | coveralls"
	```
4. `.travis.yml`에 다음과 같이 추가한다. 빌드가 성공한 후에 coveralls 명령 스크립트를 실행한다.

	```yaml
	after_success:
	- npm run coveralls
	```

5. 소스 코드 수정 후 Push를 해보자.


이제 Travis-CI에서 빌드가 성공할 때 마다, 업데이트된 코드 커버리지가 Coveralls로 보내진다.
`$ npm run coveralls` 명령어를 사용해서 Coveralls을 갱신할 수 있다. 물론,  `.coveralls.yml` 파일이 해당 저장소에 있어야한다.NMT

https://github.com/SangHakLee/NMTIC/tree/07-Adding-automatic-coverage-testing-with-Coveralls

## Success
이 모든 과정을 설정했다면, 이제 프로젝트에 build status, code coverage 뱃지를 README.md에 추가할 수 있다.
이로써 저장소를 watching하고 있는 사용자들에게 프로젝트가 책임감 있게 관리되고 있음을 알려줄 수 있다.

https://github.com/SangHakLee/NMTIC/tree/08-Success

### Conclusion
처음에  Travis-CI, Coveralls에 관심을 갖게 된 이유는 단순했다. 나도 다른 오픈소스처럼 멋진 뱃지를 README.md 넣고 싶었다. 두 서비스를 쓰다보니 자연스럽게 TDD로 개발하는 방법을 익히게 됐다.
Build status와 Code coverage report가 나와있는 오픈소스는 다른 사람들이 보기에도 신뢰를 준다.
더 멋진 오픈소스 저장소를 위해서 써보는 것을 적극 추천한다.


### References
- [코드 커버리지에 대하여][8]
- [Istanbul로 코드 커버리지 측정하기][9]
- [Github에서 코드 커버리지를 보여주는 Coveralls][10]
- [코드 커버리지란 (CODE COVERAGE)][11]


  [1]: https://nodejs.org/api/assert.html
  [2]: http://dsernst.com/images/travis-ci-setup.png
  [3]: https://travis-ci.org/
  [4]: https://travis-ci.org/profile
  [5]: https://gotwarlost.github.io/istanbul/
  [6]: https://docs.npmjs.com/cli/run-script
  [7]: https://coveralls.io/
  [8]: http://hunj.me/code_coverage/
  [9]: http://blog.jeonghwan.net/2016/07/28/istanbul.html
  [10]: https://blog.outsider.ne.kr/954
  [11]: http://zzikjh.tistory.com/entry/%EC%BD%94%EB%93%9C-%EC%BB%A4%EB%B2%84%EB%A6%AC%EC%A7%80%EB%9E%80-code-coverage
