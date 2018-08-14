# [NPM] Make Private NPM Registry with sinopia -  NPM 사설 저장소 만들기

### Introduction
회사의 VCS가 SVN에서 Git으로 변경됐다. Github 기업 버전을 사용하지 않고 사내 서버에 Gitlab을 설치해서 사용한다.
최근 업무의 대부분은 Node.js로 작업한다. 어느 정도 JavaScirpt와 Node.js로 프로젝트를 진행하다 보면 Node.js 프로젝트들을 모듈화 하고 싶은 욕망이 생긴다.

기존의 만들어진 프로젝트를 모듈화 하면서 생긴 문제가 있었다. 만들어진 모듈들은 어디다 배포할까? 
NPM 저장소(https://www.npmjs.com)에 올리면 모든 코드가 공개된다. 물론 NPM의 유료 버전을 이용하면 비공개 저장소를 만들 수 있다. 
하지만, 이 또한 내 코드가 NPM 저장소에 올라간다. 

Gitlab과 유사하게 설치형 사설 NPM 저장소를 만들어서 이용하고 싶었고 열심히 검색을 했다.
역시나, 이와 비슷한 기능이 필요한 개발자들이 있었고 이([sinopia](https://github.com/rlidwka/sinopia))를 구현한 개발자들이 있었다.

sinopia를 이용해 사설 NPM registry를 구축하는 방법에 대해 알아본다.

> 이 포스팅은 Make Graceful Node.js Module 의 첫 번째 포스팅으로 아래의 순서로 연재된다.
> 1. [NPM] Make Private NPM Registry with sinopia - NPM 사설 저장소 만들기
> 1. [Gitlab] CI / CD with Gitlab Runner - Gitlab에서 지속적 통합-배포하기
> 1. [NPM / Gitlab] Build, Test and Deploy Private Node Package with Gitlab - Gitlab에서 Node.js 프로젝트 관리하기

## Package manager
패키지를 관리해주는 일련의 S/W 패키지를 사용하려면 설치, 버전 관리, 삭제 등 기본적인 설치 과정이 필수적으로 필요하다.
추가적으로 테스트-빌드, 의존성 관리 등 관리 포인트가 많다.
이러한 복잡한 패키지의 사용을 쉽게 관리해주는 것 이 package manager


### 언어별 패키지 매니저
- [npm](https://www.npmjs.com/) - npm is the package manager for JavaScript and the world’s largest software registry.
- [pip](https://pip.pypa.io/en/stable/) - The PyPA recommended tool for installing Python packages.
- [composer](https://getcomposer.org/) - Dependency Manager for PHP.
- [gem](https://rubygems.org/) - Find, install, and publish RubyGems.
- [bower](https://bower.io/) - A package manager for the web.

## sinopia

### Use cases
https://github.com/rlidwka/sinopia#use-cases

1. [사설 저장소로 이용](https://github.com/rlidwka/sinopia#using-private-packages)
비공개 NPM 사설 저장소로 이용할 수 있다. 
모듈들의 코드들은 저장소로 이용되는 서버에 올라가기 때문에 기업의 코드들이 외부 서비스에 올라가는 걱정을 하지 않아도 된다.
1. [NPM 저장소 캐시](https://github.com/rlidwka/sinopia#using-public-packages-from-npmjsorg)
1. [NPM 저장소에 모듈 덮어쓰기](https://github.com/rlidwka/sinopia#override-public-packages)


### Install
sinopia 서버를 사용하기 위해 전역으로 sinopia 모듈을 설치한다.
```bash
$ npm i -g sinopia

$ sudo npm i -g sinopia
```

### Usage

#### Run sinopia Server

바로 sinopia를 이용할 수 있다.

```bash
$ sinopia
```

**output:**
```
 warn  --- config file  - /home/lsh/.config/sinopia/config.yaml
 warn  --- http address - http://localhost:4873/
```

![1-run sinopia](https://user-images.githubusercontent.com/9030565/38304829-ace7a856-3845-11e8-8ad2-5b05abb48113.PNG)


> http://localhost:4873로 접속하면 사설 NPM 저장소의 웹페이지를 확인할 수 있다.
> 현재 구동된 sinopia 서버는 기본적으로 제공하는 설정대로 작동된다.
> 이렇게 구동된 서버는 관리상 불편함이 생긴다. 
> 설정을 변경해서 좀 더 편하게 sinopia 서버를 관리하는 방법을 알아본다.

### Run sinopia Server with custom config

#### 1. 폴더 생성
```bash
$ mkdir ~/npm-registry & cd ~/npm-registry
```
앞으로 사설 NPM 저장소는 `npm-registry/`에서 관리된다.

#### 2. 설정 파일 복사
```bash 
$ cp ~/.config/sinopia/config.yaml ./config.yaml
```
기본 설정 파일 `config.yaml` 을 현재 위치로 복사한다.

#### 3. config.yml로 서버 구동
```bash
$ sinopia ./config.yaml
```
**output:**
```
 warn  --- config file  - /home/lsh/npm-registry/config.yaml
 warn  --- http address - http://localhost:4873/
```

### Edit custom conifg.yaml
설정 파일의 내용을 수정해서 자신의 환경에 맞는 서버를 구축한다.

#### Host
**config.yml**
```yaml
# ...
listen: 0.0.0.0:4873
```
- `(optional)` 기본 포트인 `4873`을 변경할 때 사용한다.
- `(optional)` 만약 localhost로 접근이 안되면 **0.0.0.0**으로 변경
	- 클라우드 서비스 이용 시

> **중요**
> 필자의 개발환경은 개인 PC 환경이 아닌 클라우드 서버를 이용한다.
> 클라우드 VM의 주소가 10.222.222.227이기 때문에 앞으로 접근 주소는 http://10.222.222.227:4873이 된다.
> 만약, 로컬 환경에서 테스트를 하면 http://localhost:4873을 그대로 사용하면 된다.

#### User

`$ npm publish` 시 인증을 위한 사용자를 추가한다.
또한 http://localhost:4873에 접근 후 로그인할 사용자이기도 하다.

**Generate password using Node:**
```bash
$ node

> crypto.createHash('sha1').update('password').digest('hex')
'5baa61e4c9b93f3f0682250b6cf8331b7ee68fd8'
```
- Node.js를 이용해서 sh1 암호화된 문자열을 얻는다.
- .update('mypass')에서 **`mypass`**를 사용하려는 패스워드로 변경한다.
	- e.g.)  `.update('Admin123@')`

**config.yml**
```yaml
users:
  admin:
    password: 5baa61e4c9b93f3f0682250b6cf8331b7ee68fd8
```


### Run sinopia Server
수정한 config.yaml 파일로 sinopia 서버를 구동한다.
[config.yaml](https://github.com/SangHakLee/npm-registry/blob/master/config.yaml)
```bash
$ nohup sionpia ./config.yaml &
```
nohup & 옵션을 이용해 백그라운드 프로세스를 구동하고 터미널을 닫아도 서버가 돌아가게 한다.

![4-nohup](https://user-images.githubusercontent.com/9030565/38304886-dfab5620-3845-11e8-8695-bd76267c13ad.PNG)



## Private NPM Registry 

### adduser

```bash
$ npm adduser --registry http://10.222.222.227:4873
```
- Username: `admin`
	- config.yml에서 설정한 사용자
- Password: `password`
	- - config.yml에서 설정한 사용자의 패스워드
- 로컬 환경이면, ```$ npm adduser --registry http://localhost:4873```

### Create module
```bash
$ mkdir ~/my-module 
$ cd ~/my-module

$ wget https://gist.githubusercontent.com/SangHakLee/9513672e133c1027bc06a80c993d7025/raw/e5c0b7508702f974ebcb1dbfeacf205b8ba3814c/package.json

$ echo "console.log('my-module')" > index.js
```

> package.json에서 `"publishConfig": {"registry": "http://10.222.222.227:4873"}` 옵션이 있기 때문에
> `$ npm publish` 요청 시 `http://registry.npmjs.org`가 아닌 `http://10.222.222.227:4873`으로 publish 한다.
> 
> 만약, package.json에 해당 옵션을 주지 않으면 `npm set registry http://10.222.222.227:4873` 명령으로 registry를 변경 후 publish 한다. 
> 
> 이 경우 앞으로 모든 npm 요청이 set registry로 설정한 서버로 가기 때문에 public 모듈을 설치할 때 다시 `npm set http://registry.npmjs.org` 해줘야 하는 번거로움이 있다.
>
> https://gist.github.com/SangHakLee/9513672e133c1027bc06a80c993d7025

### Pubish

```bash
$ npm publish
+ my-module@1.1.0
```

![10-web](https://user-images.githubusercontent.com/9030565/38305090-7ecb40e4-3846-11e8-8ed1-e28041bd7f6d.PNG)


### Install module 

**Set registry and install:**
```bash
$ npm set registry http://10.222.222.227:4873

$ npm i my-module
```

![11-install-module](https://user-images.githubusercontent.com/9030565/38305107-937702c6-3846-11e8-80ab-b9df48dc3548.PNG)


### Conclusion
sinopia를 이용해서 private 한 NPM 저장소를 만들어봤다.
사실 sinopia가 private NPM registry를 위한 프로젝트는 아니다. 
처음 설명에서 말한 대로 크게 3가지의 Use cases가 있고 그중 하나가 사설 저장소로 이용하는 것이다.
위의 방법대로 만들어진 NPM registry를 외부에 공개하면 공개된 registry가 된다.

config.yaml의 옵션들을 잘 사용하면 꽤 근사한 저장소를 만들 수 있다. 

Node.js를 이용해서 회사의 시스템을 구성한다면 http://registry.npmjs.org 저장소 대신 사내 NPM 서버를 구축해서 사용해보는 것을 강력 추천한다.


### References
-  [Maintaining a Private NPM registry for your Organization with Sinopia](http://thejackalofjavascript.com/maintaining-a-private-npm-registry/)
<!--stackedit_data:
eyJwcm9wZXJ0aWVzIjoiYXV0aG9yOiBzYW5naGFrbGVlXG4iLC
JoaXN0b3J5IjpbMTgyMDA4NDQzMywtMTg2NDU2MDIyNSwtNzU0
MTU1OTYxXX0=
-->