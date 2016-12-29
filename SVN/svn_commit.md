# SVN 커밋 과정

### Step 0. 코드 수정  
현재 개발 중인 서버에 ssh 접속 후 코드 수정

**주의사항**
- 개발, 운영 서버의 DB 정보도 꼭 확인한다.

<br />

### Step 1. 최신 소스 update
다른 팀원이 같은 파일을 수정했을 수도 있기 때문에 확인한다.

>$ svn update

- p  : postpone
  다음에 시도 충돌된 파일에 <<<<  >>>> 로 표시.
  자신의 파일은 .mine 으로 저장되고
  저장소에서 가져온 파일은 .r100 등 리비전 번호로 저장된다.
- df : diff-full
  변경된 부분을 모두 보여준다.
- e  : edit
  에디터로 수정한다.
- mc : mine-conflict
- tc : theirs-confict

p 키를 눌러 수동적으로 충돌을 해결한다.

<br />

### Step 2. 변경된 파일들 확인

> $ svn st

> $ svn status

```bash
[lsh@test]$ svn st
M       application/config/database.php
M       application/config/config.php
M       index.php
```

- M : Modified
- A : Added
- D : Deleted
- C : Conflicted
- G : Merged
- ? : new

<br />

#### Conflicted

만약 충돌이 생기면 충돌난 파일에 C 가 붙고 몇 개의 파일이 생긴다. ( p 누른 경우 )

```bash
?	application/config/database.php.r1002
?	application/config/database.php.r1000
?	application/config/database.php.mine
C	application/config/database.php
```
이렇게 ? 로 3개의 파일이 생기면 에디터로
application/config/database.php 열면 <<<<  >>>> 로 충돌을 확인 할수 있다.

가장 쉬운 해결법은 (내 수정본을 무시)

> $ rm -rf  application/config/database.php*

> $ svn st

> $ svn up application/config/database.php

내 수정본을 모두 지우고 svn 저장소에서 해당 파일만 update 받는 것이다.

<br />

### Step 3. 변경된 내용 확인

> $ svn diff application/config/database.php

<br />

### Step 4. 커밋

> $ svn ci

> $ svn commit

Or

> $ svn ci application/config/databases.php

OS 기본 에디터가 뜨고 상단에 commit 메시지 입력 후 :wq

<br />

### Step 5. 최신 코드 update
실 서버에 ssh 접속 후
개발 서버에서 commit 된 소스만 update

> $ svn up application/config/database.php application/config/config.php

> $ svn update application/config/database.php application/config/config.php

파일이 여러개일 경우 스페이스바로 구분
