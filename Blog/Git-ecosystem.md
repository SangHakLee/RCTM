# [Git] Git ecosystem with. Github Gist Pages Gitbook Travis

@(Marxico)


### Introduction
개발자들은, 자신의 프로그램 소스 코드를 효율적으로 관리하기 위해 버전 관리 툴을 이용한다.
과거에는 [SVN][1]을 많이 사용했지만, 최근엔 [Git][2]을 대부분 선택한다.
Git과 함께 사용할 수 있는 Git 생태계에 대해서 알아본다.

## Git
> **Git** is a free and open source distributed version control system designed to handle everything from small to very large projects with speed and efficiency.
https://git-scm.com/

프로그램의 소스 코드를 관리하는 **[DVCS][3]**(**D**istributed **V**ersion **C**ontrol **S**ystems)

### Version Control
![Version Control | 300x0][4]
> 버전 관리 시스템은 파일의 변화를 시간에 따라 기록하여 과거 특정 시점의 버전을 다시 불러올 수 있는 시스템이다. 
[로컬, 중앙 집중, 분산 버전 관리][5]

### Git vs SVN
![git vs svn | 500x0 ][6]

### Git References
- [Version Control Visualization][7]
- [Version control concepts and best practices][8]
- [Learn Version Control with Git][9]
- [git - 간편 안내서][10]
- [svn 능력자를 위한 git 개념 가이드][11]
- [Git을 이용한 협업 워크플로우 배우기][12]
___

## Github
> GitHub is a development platform inspired by the way you work. From open source to business, you can host and review code, manage projects, and build software alongside millions of other developers.
https://github.com/

Git으로 관리되는 프로젝트를 저장하는 원격 저장소

### Make Public Repository

### Try Pull Request

### More
#### Bitbucket
> Code, Manage, Collaborate
Bitbucket is the Git solution for professional teams
https://bitbucket.org/

Free plan에서 `개인` 혹은 `5명 이하의 팀`은 private 저장소가 무제한 무료.
[Atlassian][13] support.

#### Gitlab
> The platform for modern developers
GitLab unifies issues, code review, CI and CD into a single UI
https://about.gitlab.com

설치형 Git 저장소

`Gitlab 장애` [GitLab.com Database Incident][14] ([한글][15])

### Github References
- [Github를 이용하는 전체 흐름 이해하기 #1][16]
- [Git과 GitHub 사용하기][17]
- [GitHub vs. Bitbucket vs. GitLab vs. Coding][18]

---

## Pages
> Websites for you and your projects.
Hosted directly from your GitHub repository. Just edit, push, and your changes are live.
https://pages.github.com/

Github 저장소를 웹 페이지로 만들어주는 서비스.
정적 컨텐츠 블로그로 주로 이용된다.

### github.io
Github를 계정이 있다면 `{username}.github.io` 로 접근할 수 있는 호스팅 도메인을 1개씩 사용가능하다.
https://sanghaklee.github.io/

Github Repo를 {username}/{username}.github.io 로 만들면 바로 이용할 수 있다.
https://github.com/SangHakLee/sanghaklee.github.io

Pages는 PHP, JSP, ASP 같은 서버사이드 스크립트를 읽는 서버가 없다.
HTML, CSS, JavaScript를 포함한 정적 컨텐츠만 이용할 수 있다.

### Jekyll vs Hexo  
정적 컨텐츠만 올릴 수 있지만, 이 과정을 쉽고 간편하게 해주는 서비스가 있다.
https://www.staticgen.com/
https://staticsitegenerators.net/
 Ruby 기반의 [Jekyll][19]
 Node.js 기반의 [Hexo][20] 

### Pages References
- [kakao 기술 블로그가 GitHub Pages로 간 까닭은][21]
- [Github pages와 Hexo를 이용하여 블로그 만들기][22]
- [지킬로 깃허브에 무료 블로그 만들기][23]
- [Hexo 시작하기][24]

---

## Gist
> Instantly share code, notes, and snippets.
https://gist.github.com/

짧고 간단한 코드를 저장, 버전관리, 웹에 삽입하는 서비스

### Embed Your Gist Code
<script src="https://gist.github.com/SangHakLee/f3bdee00e6bcbb1965c235840eb43034.js"></script>

### More
#### LEPTON
> Gist를 효율적으로 관리하는 툴
http://hackjutsu.com/Lepton

![LEPTON](https://cloud.githubusercontent.com/assets/9030565/26362933/6b906c7c-401a-11e7-86e0-3e3244a66cd8.png)


#### Colorscripter
`script` 태그 없이  코드 Syntax HighLighting 해주는 서비스
https://colorscripter.com/info#e

### Gist References
- [Gist를 이용한 소스관리][25]
- [블로그 등에 소스 코드 Snippet 붙여넣기 - GitHub Gist][26]

---

## Gitbook
> Documentation made easy
GitBook helps your team write, collaborate and publish content online.
https://www.gitbook.com/

[Markdown][27] 포맷을 이용해서 e-book을 작성하고 버전관리, 배포해주는 서비스

### Make RESTApi Guide Page

### Make Tutorial Page

#### Example
https://sanghaklee.gitbooks.io/elk/content/

### Gitbook References
- [Gitbook 과 Pandoc 을 이용한 전자 출판][28]

## Travis CI
> Test and Deploy with Confidence
Easily sync your GitHub projects with Travis CI and you’ll be testing your code in minutes!
https://travis-ci.org/

Github Repo와 연동하여 [지속적 통합][29](continuous integration)을 자동화하는 서비스

### Travis CI References
- [Travis CI 소개 #1][30]


  [1]: https://subversion.apache.org/
  [2]: https://git-scm.com/
  [3]: https://ko.wikipedia.org/wiki/%EB%B6%84%EC%82%B0_%EB%B2%84%EC%A0%84_%EA%B4%80%EB%A6%AC
  [4]: http://www.martinhofmann.com/wp-content/uploads/2015/05/version-control-825x510.png
  [5]: https://git-scm.com/book/ko/v1/%EC%8B%9C%EC%9E%91%ED%95%98%EA%B8%B0-%EB%B2%84%EC%A0%84-%EA%B4%80%EB%A6%AC%EB%9E%80?
  [6]: https://www.git-tower.com/learn/content/01-git/01-ebook/en/02-desktop-gui/07-appendix/02-from-subversion-to-git/centralized-vs-distributed.png
  [7]: http://www.martinhofmann.com/version-control-visualization/
  [8]: https://homes.cs.washington.edu/~mernst/advice/version-control.html
  [9]: https://www.git-tower.com/learn/git/ebook/en/desktop-gui/appendix/from-subversion-to-git
  [10]: https://rogerdudler.github.io/git-guide/index.ko.html
  [11]: https://www.slideshare.net/einsub/svn-git-17386752
  [12]: http://blog.appkr.kr/learn-n-think/comparing-workflows/
  [13]: https://www.atlassian.com/
  [14]: https://about.gitlab.com/2017/02/01/gitlab-dot-com-database-incident/
  [15]: https://kyupokaws.wordpress.com/2017/02/02/2117-gitlab-com-database-incident%ED%95%9C%EA%B8%80%EB%B2%88%EC%97%AD/
  [16]: https://blog.outsider.ne.kr/865
  [17]: http://kr.discovermeteor.com/chapters/github/
  [18]: https://medium.com/flow-ci/github-vs-bitbucket-vs-gitlab-vs-coding-7cf2b43888a1
  [19]: https://jekyllrb-ko.github.io/
  [20]: https://hexo.io/ko/
  [21]: http://tech.kakao.com/2016/07/07/tech-blog-story/
  [22]: http://blog.lattecom.xyz/2016/06/28/hexo-blog-github-pages/
  [23]: https://nolboo.kim/blog/2013/10/15/free-blog-with-github-jekyll/
  [24]: https://hyunseob.github.io/2016/02/23/start-hexo/
  [25]: https://gist.github.com/safe1981/2041116
  [26]: http://hanmomhanda.tistory.com/entry/%EB%B8%94%EB%A1%9C%EA%B7%B8-%EB%93%B1%EC%97%90-%EC%86%8C%EC%8A%A4-%EC%BD%94%EB%93%9C-Snippet-%EB%B6%99%EC%97%AC%EB%84%A3%EA%B8%B0-GitHub-Gist
  [27]: https://ko.wikipedia.org/wiki/%EB%A7%88%ED%81%AC%EB%8B%A4%EC%9A%B4
  [28]: http://blog.appkr.kr/work-n-play/pandoc-gitbook-%EC%A0%84%EC%9E%90%EC%B6%9C%ED%8C%90/
  [29]: https://ko.wikipedia.org/wiki/%EC%A7%80%EC%86%8D%EC%A0%81_%ED%86%B5%ED%95%A9
  [30]: https://blog.outsider.ne.kr/779
