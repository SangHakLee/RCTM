# [Git] Git ecosystem with. Github Gist Pages Gitbook

### Introduction
개발자들은, 자신의 프로그램 소스 코드를 효율적으로 관리하기 위해 버전 관리 툴을 이용한다.
과거에는 [SVN][1]을 많이 사용했지만, 최근엔 [Git][2]을 대부분 선택한다.
Git과 함께 사용할 수 있는 Git 생태계에 대해서 알아본다.

## Git
> **Git** is a free and open source distributed version control system designed to handle everything from small to very large projects with speed and efficiency.
https://git-scm.com/

프로그램의 소스 코드를 관리하는 **[DVCS][3]**(**D**istributed **V**ersion **C**ontrol **S**ystems)

### Version Control

### Git vs SVN

### Git References
- [git - 간편 안내서][4]
- [svn 능력자를 위한 git 개념 가이드][5]
___

## Github
> GitHub is a development platform inspired by the way you work. From open source to business, you can host and review code, manage projects, and build software alongside millions of other developers.
https://github.com/

Git으로 관리되는 프로젝트를 저장하는 원격 저장소

### Make Public Repository

### Try Pull Request

### Github References
- [Github를 이용하는 전체 흐름 이해하기 #1][6]

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
https://staticsitegenerators.net/
 Ruby 기반의 Jekyll https://jekyllrb-ko.github.io/
 Node.js 기반의 Hexo https://hexo.io/ko/

### Pages References
- [kakao 기술 블로그가 GitHub Pages로 간 까닭은][7]
- [Github pages와 Hexo를 이용하여 블로그 만들기][8]
- https://nolboo.kim/blog/2013/10/15/free-blog-with-github-jekyll/
- https://hyunseob.github.io/2016/02/23/start-hexo/

---

## Gist
> Instantly share code, notes, and snippets.
https://gist.github.com/

짧고 간단한 코드를 저장, 버전관리, 웹에 삽입하는 서비스

### Embed Your Gist Code
<script src="https://gist.github.com/SangHakLee/f3bdee00e6bcbb1965c235840eb43034.js"></script>

### More...
#### LEPTON
http://hackjutsu.com/Lepton

#### Colorscripter
`script` 태그 없이  코드 Syntax HighLighting 해주는 서비스
https://colorscripter.com/info#e

### Gist References
- [Gist를 이용한 소스관리][9]
- [블로그 등에 소스 코드 Snippet 붙여넣기 - GitHub Gist][10]

---

## Gitbook
> Documentation made easy
GitBook helps your team write, collaborate and publish content online.
https://www.gitbook.com/

[Markdown][11] 포맷을 이용해서 e-book을 작성하고 버전관리, 배포해주는 서비스

### Make RESTApi Guide Page

### Make Tutorial Page

#### Example
https://sanghaklee.gitbooks.io/elk/content/

### Gitbook References


  [1]: https://subversion.apache.org/
  [2]: https://git-scm.com/
  [3]: https://ko.wikipedia.org/wiki/%EB%B6%84%EC%82%B0_%EB%B2%84%EC%A0%84_%EA%B4%80%EB%A6%AC
  [4]: https://rogerdudler.github.io/git-guide/index.ko.html
  [5]: https://www.slideshare.net/einsub/svn-git-17386752
  [6]: https://blog.outsider.ne.kr/865
  [7]: http://tech.kakao.com/2016/07/07/tech-blog-story/
  [8]: http://blog.lattecom.xyz/2016/06/28/hexo-blog-github-pages/
  [9]: https://gist.github.com/safe1981/2041116
  [10]: http://hanmomhanda.tistory.com/entry/%EB%B8%94%EB%A1%9C%EA%B7%B8-%EB%93%B1%EC%97%90-%EC%86%8C%EC%8A%A4-%EC%BD%94%EB%93%9C-Snippet-%EB%B6%99%EC%97%AC%EB%84%A3%EA%B8%B0-GitHub-Gist
  [11]: https://ko.wikipedia.org/wiki/%EB%A7%88%ED%81%AC%EB%8B%A4%EC%9A%B4
