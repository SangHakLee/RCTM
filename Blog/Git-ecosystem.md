# [Git] Git ecosystem with. Github Gist Gitbook Pages Travis

@(Marxico)


### Introduction
개발자들은, 자신의 프로그램 소스 코드를 효율적으로 관리하기 위해 버전 관리 툴을 이용한다.
과거에는 [SVN][1]을 많이 사용했지만, 최근엔 [Git][2]을 대부분 선택한다.
Git과 함께 사용할 수 있는 Git 생태계에 대해서 알아본다.

## Git
> **Git** is a free and open source distributed version control system designed to handle everything from small to very large projects with speed and efficiency.
https://git-scm.com

프로그램의 소스 코드를 관리하는 **[DVCS][3]**(**D**istributed **V**ersion **C**ontrol **S**ystems)

### Version Control
![Version Control | 300x0][4]
> 버전 관리 시스템은 파일의 변화를 시간에 따라 기록하여 과거 특정 시점의 버전을 다시 불러올 수 있는 시스템이다. 
[로컬, 중앙 집중, 분산 버전 관리][5]

### SVN vs Git
> SVN : 파일을 관리하는 중앙 서버가 존재하고, 각 클라이언트들은 중앙 서버에서 파일을 Checkout 받아 사용한다. 즉, 중앙 서버에 의존적인 파일 관리 체계이다.
Git: 파일을 관리를 개인 컴퓨터에서 한다. 각 클라이언트들은 자신의 파일 관리 서버에서 버전 관리를 하고, 협업을 위해 원격 저장소를 이용한다.

![git vs svn | 500x0 ][6]

### Git References
- [Version Control Visualization][7]
- [Version control concepts and best practices][8]
- [Learn Version Control with Git][9]
- [Git, 분산식 버젼관리시스템(DVCS)][10]
- [git - 간편 안내서][11]
- [svn 능력자를 위한 git 개념 가이드][12]
- [Git을 이용한 협업 워크플로우 배우기][13]

___

## Github
> GitHub is a development platform inspired by the way you work. From open source to business, you can host and review code, manage projects, and build software alongside millions of other developers.
https://github.com

Git으로 관리되는 프로젝트를 저장하는 원격 저장소

### Make Public Repository
![create a new repository 2017-06-06 22-08-56](https://cloud.githubusercontent.com/assets/9030565/26830847/29ce02a4-4b05-11e7-9745-05e75af518d4.jpg)
- **Add .gitignore** : Git 원격 저장소에 올리지 않은 무시될 파일/폴더를 명시하는 파일
	- https://git-scm.com/docs/gitignore
	- 언어별로 기본 .gitignore 파일을 제공하니 선택해서 사용해도 됨. 필자는 `Node` 선택
- **Add a license** : 해당 프로젝트의 라이센스 정보를 명시하는 파일

![sanghaklee php-js-function pollyfill php function for javascript 2017-06-06 22-11-05](https://cloud.githubusercontent.com/assets/9030565/26830859/3fe2cd2c-4b05-11e7-82b1-861051373f63.jpg)


### Try Pull Request
> Pull requests let you tell others about changes you've pushed to a repository on GitHub.

Github 원격 저장소의 Push 된 변경 사항을 다른 사용자들에게 알려서 코드 수정 사항에 대해 같이 토론하는 과정 

### More
#### Bitbucket
> Code, Manage, Collaborate
Bitbucket is the Git solution for professional teams
https://bitbucket.org

Free plan에서 `개인` 혹은 `5명 이하의 팀`은 private 저장소가 무제한 무료.
[Atlassian][14] support.

#### Gitlab
> The platform for modern developers
GitLab unifies issues, code review, CI and CD into a single UI
https://about.gitlab.com

설치형 Git 저장소

`Gitlab 장애` [GitLab.com Database Incident][15] ([한글][16])

### Github References
- [Github를 이용하는 전체 흐름 이해하기 #1][17]
- [Git과 GitHub 사용하기][18]
- [GitHub vs. Bitbucket vs. GitLab vs. Coding][19]

---

## Gist
> Instantly share code, notes, and snippets.
https://gist.github.com

짧고 간단한 코드를 저장, 버전관리, 웹에 삽입하는 서비스

### Embed Your Gist Code
```javascript
<script src="https://gist.github.com/SangHakLee/f3bdee00e6bcbb1965c235840eb43034.js"></script>
```
<script src="https://gist.github.com/SangHakLee/f3bdee00e6bcbb1965c235840eb43034.js"></script>

### More
#### Lepton
> Gist를 효율적으로 관리하는 툴
http://hackjutsu.com/Lepton

![LEPTON](https://cloud.githubusercontent.com/assets/9030565/26362933/6b906c7c-401a-11e7-86e0-3e3244a66cd8.png)


#### Colorscripter
> `<script>` 태그 없이  코드 Syntax HighLighting 해주는 서비스
https://colorscripter.com

### Gist References
- [What is GitHub?][20]
- [Gist를 이용한 소스관리][21]
- [블로그 등에 소스 코드 Snippet 붙여넣기 - GitHub Gist][22]

---

## Gitbook
> Documentation made easy
GitBook helps your team write, collaborate and publish content online.
https://www.gitbook.com

[Markdown][23] 포맷을 이용해서 e-book을 작성하고 버전관리, 배포해주는 서비스

### Make REST Api Guide Page

### Make Tutorial Page

#### Example
https://sanghaklee.gitbooks.io/elk/content
![introduction elk 2017-06-06 23-43-14](https://cloud.githubusercontent.com/assets/9030565/26834991/04b2f9b8-4b12-11e7-9655-e1c91afa8cbe.jpg)


### Gitbook References
- [Gitbook 과 Pandoc 을 이용한 전자 출판][24]

---


## Pages
> Websites for you and your projects.
Hosted directly from your GitHub repository. Just edit, push, and your changes are live.
https://pages.github.com

Github 저장소를 웹 페이지로 만들어주는 서비스.
정적 컨텐츠 블로그로 주로 이용된다.

### github.io
Github를 계정이 있다면 `{username}.github.io` 로 접근할 수 있는 호스팅 도메인을 1개씩 사용가능하다.
https://sanghaklee.github.io

Github Repo를 {username}/{username}.github.io 로 만들면 바로 이용할 수 있다.
https://github.com/SangHakLee/sanghaklee.github.io

Pages는 PHP, JSP, ASP 같은 서버사이드 스크립트를 읽는 서버가 없다.
HTML, CSS, JavaScript를 포함한 정적 컨텐츠만 이용할 수 있다.

### Static Site Generator
https://www.staticgen.com
https://staticsitegenerators.net

### Jekyll vs Hexo  
정적 컨텐츠만 올릴 수 있지만, 이 과정을 쉽고 간편하게 해주는 서비스가 있다.
Ruby 기반의 [Jekyll][25]
Node.js 기반의 [Hexo][26] 

... Go 기반의 [Hugo][27]

### Pages References
- [kakao 기술 블로그가 GitHub Pages로 간 까닭은][28]
- [Github pages와 Hexo를 이용하여 블로그 만들기][29]
- [지킬로 깃허브에 무료 블로그 만들기][30]
- [Hexo 시작하기][31]

---

## Travis CI
> Test and Deploy with Confidence
Easily sync your GitHub projects with Travis CI and you’ll be testing your code in minutes!
https://travis-ci.org/

Github Repo와 연동하여 [지속적 통합][32](continuous integration)을 자동화하는 서비스

### .travis.yml
#### PHP
```yaml
language: php
php:
  - '5.4'
  - '5.5'
```

#### Node.js
```yaml
language: node_js
node_js:
  - '6'
```

### Do it!
#### Step 0
아래의 과정은 텍스트로 대체
1. Github Services **Travis CI** 활성화 (Repo - Settings - Intergrations & services)
1. Travis CI  가입 (Github 계정 연동)
1. Travis CI 사용할 Github Repo 추가
1. Github Repo에 프로젝트에 맞는 `.travis.yml` 파일 추가

#### Step 1
Travis CI 를 사용할 Repo만 선택한 상태.
**No bulids for this repository**
![travis ci 1 2017-06-06 23-01-51](https://cloud.githubusercontent.com/assets/9030565/26833672/17579b22-4b0e-11e7-9559-db7d63e7f748.jpg)

#### Step 2
Github에 Push 요청이 오고 이를 감지한 Travis CI 가 해당 프로젝트를 빌드한 상태
![travis ci 2 2017-06-06 23-12-01](https://cloud.githubusercontent.com/assets/9030565/26833754/52d3a4ca-4b0e-11e7-9b70-38420e34a2ca.jpg)
![travis ci 3 2017-06-06 23-12-20](https://cloud.githubusercontent.com/assets/9030565/26833813/71f1a956-4b0e-11e7-9f8d-26922c3565c4.jpg)

#### Step 3
Github Repo README.md 에 Travis CI 에서 제공하는 build status 아이콘을 넣을 수 있다.
![sanghaklee php-js-function pollyfill php function for javascript 2017-06-06 23-26-26](https://cloud.githubusercontent.com/assets/9030565/26834201/a5066cae-4b0f-11e7-8409-3907e8314485.jpg)

### Travis CI References
- [Travis CI 소개 #1][33]
- [Travis CI 의 연동과 사용][34]

---

### Conclusion
Git은 코드를 관리하기 위한 DVCS이다. Git을 이런 용도로만 제대로 사용해도 성공이지만, Git 과 함께 사용할 수 있는 서비스는 소개한 서비스 말고도 굉장히 많다.
Git 보다는  Github과 함께 사용할 수 있는 서비스라고 명하는 것이 더 정확할 수 있으나, Github의 root는 Git이니 Git 생태계라 정했다.
안 쓸 이유가 없는 서비스들이니 한 번쯤 사용해보는 것을 적극 추천한다. 


  [1]: https://subversion.apache.org/
  [2]: https://git-scm.com/
  [3]: https://ko.wikipedia.org/wiki/%EB%B6%84%EC%82%B0_%EB%B2%84%EC%A0%84_%EA%B4%80%EB%A6%AC
  [4]: http://www.martinhofmann.com/wp-content/uploads/2015/05/version-control-825x510.png
  [5]: https://git-scm.com/book/ko/v2/%EC%8B%9C%EC%9E%91%ED%95%98%EA%B8%B0-%EB%B2%84%EC%A0%84-%EA%B4%80%EB%A6%AC%EB%9E%80%3F
  [6]: https://www.git-tower.com/learn/content/01-git/01-ebook/en/02-desktop-gui/07-appendix/02-from-subversion-to-git/centralized-vs-distributed.png
  [7]: http://www.martinhofmann.com/version-control-visualization/
  [8]: https://homes.cs.washington.edu/~mernst/advice/version-control.html
  [9]: https://www.git-tower.com/learn/git/ebook/en/desktop-gui/appendix/from-subversion-to-git
  [10]: http://skyfall.tistory.com/1
  [11]: https://rogerdudler.github.io/git-guide/index.ko.html
  [12]: https://www.slideshare.net/einsub/svn-git-17386752
  [13]: http://blog.appkr.kr/learn-n-think/comparing-workflows/
  [14]: https://www.atlassian.com/
  [15]: https://about.gitlab.com/2017/02/01/gitlab-dot-com-database-incident/
  [16]: https://kyupokaws.wordpress.com/2017/02/02/2117-gitlab-com-database-incident%ED%95%9C%EA%B8%80%EB%B2%88%EC%97%AD/
  [17]: https://blog.outsider.ne.kr/865
  [18]: http://kr.discovermeteor.com/chapters/github/
  [19]: https://medium.com/flow-ci/github-vs-bitbucket-vs-gitlab-vs-coding-7cf2b43888a1
  [20]: https://guides.github.com/activities/hello-world/
  [21]: https://gist.github.com/safe1981/2041116
  [22]: http://hanmomhanda.tistory.com/entry/%EB%B8%94%EB%A1%9C%EA%B7%B8-%EB%93%B1%EC%97%90-%EC%86%8C%EC%8A%A4-%EC%BD%94%EB%93%9C-Snippet-%EB%B6%99%EC%97%AC%EB%84%A3%EA%B8%B0-GitHub-Gist
  [23]: https://ko.wikipedia.org/wiki/%EB%A7%88%ED%81%AC%EB%8B%A4%EC%9A%B4
  [24]: http://blog.appkr.kr/work-n-play/pandoc-gitbook-%EC%A0%84%EC%9E%90%EC%B6%9C%ED%8C%90/
  [25]: https://jekyllrb-ko.github.io/
  [26]: https://hexo.io/ko/
  [27]: https://gohugo.io/
  [28]: http://tech.kakao.com/2016/07/07/tech-blog-story/
  [29]: http://blog.lattecom.xyz/2016/06/28/hexo-blog-github-pages/
  [30]: https://nolboo.kim/blog/2013/10/15/free-blog-with-github-jekyll/
  [31]: https://hyunseob.github.io/2016/02/23/start-hexo/
  [32]: https://ko.wikipedia.org/wiki/%EC%A7%80%EC%86%8D%EC%A0%81_%ED%86%B5%ED%95%A9
  [33]: https://blog.outsider.ne.kr/779
  [34]: http://judelee19.github.io/etc/travis_CI/
