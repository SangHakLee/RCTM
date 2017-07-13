# Node + Mocha + Travis + Istanbul + Coveralls: 오픈소스 프로젝트 단위 테스트 & 코드 커버리지 (Unit tests & Code coverage)

> **NOTE :** 
> 이 글은 [Node + Mocha + Travis + Istanbul + Coveralls: Unit tests & coverage for your open source project](http://dsernst.com/2015/09/02/node-mocha-travis-istanbul-coveralls-unit-tests-coverage-for-your-open-source-project/)를 한국어로 번역한 글입니다. 
블로그 배포 전에 [원작자](https://github.com/dsernst)에게 동의를 구하고 번역 & 블로깅 했습니다.
원본의 내용과 조금 다를 수 있고 필자의 정보가 추가됐습니다.
또한, 단계별 과정에 캡쳐를 추가하고  코드를 [Github](https://github.com/SangHakLee/NMTIC) 저장소에 올려 두었습니다. 

> This article is a translation of [Korean + Moccas + Travis + Istanbul + Coveralls: Unit tests & coverage for your open source project](http://dsernst.com/2015/09/02/node-mocha-travis-istanbul-coveralls-unit-tests-coverage-for-your-open-source-project/).
Before publishing the blog, I asked for the [authors](https://github.com/dsernst)' consent and translated and blogged.
The contents of the original may be slightly different, and my information has been added.
I also added capture to the step-by-step process and put the code in the [Github](https://github.com/SangHakLee/NMTIC) repository.

### Introduction
**Unit tests & code coverage metrics**은 여러분이 오픈 소스 프로젝트를 더 쉽게 작업할 수 있게 도와준다.
첫째로, 프로젝트 소유자인 당신이 프로젝트 안정성에 관심을 갖고 있음을 알려준다.
둘째로, 프로젝트의 contributors들이 패치 버전에 대한 업데이트 요청(PR)을 자신 있게 보낼 수 있게 만들어준다.
마지막으로, 프로젝트가 계속해서 오래 쉽게 개발될 수 있도록 도와준다.

## Where to put your test files
https://github.com/SangHakLee/NMTIC/tree/01-Where-to-put-your-test-files

만약 테스트 파일이 오직 1개만 필요하다면,  `./test.js`라고 파일을 생성하면 된다.
이것은 Mocha 모듈의 기본 파일 명명 규칙이다.
![01-01](https://user-images.githubusercontent.com/9030565/28170433-5ef30d0c-6820-11e7-8679-d117a00e6c18.PNG)

---

만약 테스트 파일이 여러 개로 나뉘어 있다면, `test/` 폴더를 생성하고 `index.js` 파일에서 다른 테스트 파일을 호출해서 사용한다.
![01-02](https://user-images.githubusercontent.com/9030565/28170523-a9033a98-6820-11e7-99aa-67fb109a0bf0.PNG)

---

> **NOTE :** 최근의 명명 규칙은 `test/user.model.spec.js`, `test/user.ctrl.spec.js`를 많이 사용한다. 
> 폴더 구조는 상관없으나, `{}.spec.js`로 `spec`이라는 단어를 중간에 넣어준다.
> 명명 규칙이기 때문에 강제성은 없다. 일종의 규약.

만약 테스트 파일을 `test/`가 아닌 다른 곳에 놓아도, Mocha 실행 시 해당 경로를 지정해서 실행할 수 있다.
그러나, 굳이 그럴 필요가 없기 때문에 기본 경로를 사용한다.

## Get mocha to run your tests
https://github.com/SangHakLee/NMTIC/tree/02-Get-mocha-to-run-your-tests

Mocha 모듈이 설치되어 있지 않다면, 아래의 명령어로 모듈을 설치한다.
```bash
$ npm install -g mocha
```

아래의 명령어를 사용해서 기본 `test` 경로의 테스트 코드를 실행한다.
```bash
$ npm mocha
```
![02-01](https://user-images.githubusercontent.com/9030565/28170585-d13ea394-6820-11e7-9120-6b6b0119ffb8.PNG)


## Write your tests
https://github.com/SangHakLee/NMTIC/tree/03-Write-your-tests

`Mocha`는 `describe()`와 `it()`라는 전역 변수로 테스트를 진행한다.
추가적인 의존성을 줄이려면 Node.js의 기본 [assert][1] 모듈을 사용한다.

예시로 사용할 수 있는 단위 테스트 코드: [dsernst/pythagorean-triples /test.js](https://github.com/dsernst/pythagorean-triples/blob/df68a8cdf2de7c101f199fff8027d25d3a4cbeff/test.js)
![03-01](https://user-images.githubusercontent.com/9030565/28170987-05432b0a-6822-11e7-9374-3f7edeab8f12.PNG)



## Setup $ npm test
https://github.com/SangHakLee/NMTIC/tree/04-Setup-%24-npm-test

테스트 케이스를 수행하는 일반적인 방법은 `npm`의 기본 `test` 명령어를 사용하는 것이다.
이를 위해서, `"test": "mocha"`를  **package.json**의 **"scripts"**에 추가한다.

**package.json**
```json
"scripts" : {
	"test" : "mocha"
}
```

---

`$ npm test` 를 입력하면 테스트가 진행된다.
![04-02](https://user-images.githubusercontent.com/9030565/28171179-917b9364-6822-11e7-9848-3cf3d55ca96f.PNG)

---

또한, Mocha 모듈을 `devDependency`로서 추가하여 사용자가 `$ npm install`을 실행할 때마다 설치하게 할 수 있다.

```bash
$ npm install mocha --save-dev
```

## Setup TravisCI for automatic builds
https://github.com/SangHakLee/NMTIC/tree/05-Setup-TravisCI-for-automatic-builds

![travis ci][2]
[Travis CI][3]는 저장소에 Push 요청 시 코드를 자동으로 테스트하고 문제 발생 시 알람을 준다.
Github의 Pull-Request 요청에도 작동하며 공개 저장소 사용 시 무료이다.

**사용법:** 

1. Travis CI에 로그인 후 사용을 원하는 저장소를 [Switch-on][4] 한다.
![05-01](https://user-images.githubusercontent.com/9030565/28171310-f2301298-6822-11e7-968b-4bc19aff974a.PNG)

2. `.travis.yml` 파일을 저장소에 추가한다.

    ```yaml
    language: node_js
    node_js:
    - "stable"
    ```

3. 코드를 Github에 Push한다.
![05-03](https://user-images.githubusercontent.com/9030565/28171364-252728d0-6823-11e7-8300-8a03f1cfbbd6.PNG)

Travis-CI가 Push 요청을 감지하면 자동으로 `npm test` 명령을 수행한다.



## Get code coverage with Istanbul
https://github.com/SangHakLee/NMTIC/tree/06-Get-code-coverage-with-Istanbul

다양한 종류의 code coverage 분석 툴이 있지만, [istanbul][5]을 사용한다.

> **NOTE :** Code coverage란, 테스트 케이스가 소스 코드를 얼마만큼 테스트했는지 알려주는 지표이다.
> 높은 Code coverage일수록 모든 경우의 수를 테스트했다고 신뢰할 수 있다.
> **즉, 작성한 단위 테스트 케이스들이 모든 소스 코드를 다 수행했는지 알려주는 지표다.**

---

먼저, istanbul을 `devDependency`로 설치한다.

```bash
$ npm install istanbul --save-dev
```

---

코드 커버리지를 위한 [npm run-script][6]를 작성한다.

**package.json**
```json
"cover": "node_modules/.bin/istanbul cover ./node_modules/mocha/bin/_mocha"
```
> **NOTE :** 원본 글에 `"cover": "istanbul cover _mocha"`라고 명시되어 있지만, 필자의 환경에서는 작동을 안 했다. 실행 경로가 제대로 잡히지 않았던 것 같다.
> 따라서 위와 같이 직접 node_modules이 설치된 binary 경로를 명시해서 사용했다.


---

이제 `package.json`에 두개의 실행 스크립트가 존재한다.
**package.json**
```json
"scripts": {
    "test": "mocha",
    "cover": "node_modules/.bin/istanbul cover ./node_modules/mocha/bin/_mocha",
},
```
![06-02](https://user-images.githubusercontent.com/9030565/28171864-c4b8e9d2-6824-11e7-9f5e-dc7d1eb84aba.PNG)

---

여기서 한 가지 이상한 점이 있는데, `mocha` 대신 `_mocha`를 사용했다.
`mocha` 프로세스는 child process로 실행되기 때문에 그냥 사용하면 istanbul이 아래와 같은 에러를 던진다.

> No coverage information was collected, exit without writing coverage information

그러나, `_mocha`로 사용하면 child_process 없이 실행하기 때문에 아래와 같은 코드 커버리지 결과를 얻을 수 있다.

![06-03](https://user-images.githubusercontent.com/9030565/28171952-038901f6-6825-11e7-9d25-63540ec1f8ab.PNG)
- **Statements**:  코드의 Statements(명령문)의 수행률
- **Branches**:  코드의 Branches(분기문, if-else, switch 등)의 수행률
- **Functions**:  코드의 Functions(함수)의 수행률
- **Lines**: 코드의 Lines(코드 수)의 수행률

---

코드 커버리지만 원하면 아래와 같은 명령어를 입력하면 된다.
```bash
$ npm run cover
```

## Adding automatic coverage testing with Coveralls
https://github.com/SangHakLee/NMTIC/tree/07-Adding-automatic-coverage-testing-with-Coveralls

[Coveralls][7]은 공개 저장소에 사용할 수 있는 또 다른 서비스이다.
이것은  코드 커버리지 결과를 공개적으로 보고하면서 Travis-CI를 보완한다.
또한, 커버리지 결과가 큰 폭으로 변하는 경우 사용자에게 알리도록 구성할 수 있다.

1. [Coveralls.io](https://coveralls.io/)에서 계정 생성 후 사용할 저장소를 선택한다.
![07-01](https://user-images.githubusercontent.com/9030565/28172299-27f18904-6826-11e7-8fdd-771b5718d293.PNG)

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
![07-05](https://user-images.githubusercontent.com/9030565/28172348-48b056e8-6826-11e7-9cf2-a58465f9150a.PNG)


이제 Travis-CI에서 빌드가 성공할 때마다, 업데이트된 코드 커버리지가 Coveralls로 보내진다.
`$ npm run coveralls` 명령어를 사용해서 Coveralls을 갱신할 수 있다. 물론,  [.coveralls.yml](https://github.com/nickmerwin/node-coveralls#running-locally) 파일이 해당 저장소에 있어야 한다. 이 떄는 `.gitignore`에 `.coveralls.yml`을 추가해야 안전하게 이용할 수 있다. (**Key 정보가 들어가기 때문에**)

## Success
https://github.com/SangHakLee/NMTIC/tree/08-Success

이 모든 과정을 설정했다면, 이제 프로젝트에 build status, code coverage 뱃지를 README.md에 추가할 수 있다.
![08-01](https://user-images.githubusercontent.com/9030565/28172604-f95febe8-6826-11e7-9786-201b5e497542.PNG)

이로써 저장소를 watching하고 있는 사용자들에게 프로젝트가 책임감 있게 관리되고 있음을 알려줄 수 있다.



### Conclusion
https://github.com/SangHakLee/NMTIC

처음에  Travis-CI, Coveralls에 관심을 갖게 된 이유는 단순했다. 나도 다른 오픈소스처럼 멋진 뱃지를 README.md 넣고 싶었다. 두 서비스를 쓰다 보니 자연스럽게 TDD로 개발하는 방법을 익히게 됐다.
Build status와 Code coverage report가 나와있는 오픈소스는 다른 사람들이 보기에도 신뢰를 준다.
**더 멋진 오픈소스 저장소를 위해서 써보는 것을 적극 추천한다.**


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
