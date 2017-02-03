# [PHP/코드이그나이터] Codeigniter 세션 개발기 (session DB 저장)

## Introduction
기존에 CI에서 [쿠키][1] 기반으로 로그인 처리를 했다.
그러나 쿠키의 여러 가지 단점과 보안상 취약점 때문에 [세션][2]으로 변경해야 했고, 변경하는 과정에서 알게 된 CI 의 세션과 개발상 시행착오를 공유하는 목적으로 작성한 내용이다.

http://codeigniter-kr.org/user_guide_2.1.0/libraries/sessions.html
https://codeigniter.com/user_guide/libraries/sessions.html

1. Introduction
2. CI config.php 설명
3. sess_expiration 와 sess_time_to_update 관계
4. Session 관련 질문
5. Session.php

<br>

## CI config.php 설명
> CI를 사용하면서 가장 먼저 작업하는 파일이 아마 config.php이다.
파일명대로 설정에 대한 정보를 갖고 있는 파일이다. config.php를 잘 모르고 개발하면 나중에 힘들 수 있다.
http://www.codeigniter-kr.org/bbs/view/lecture?idx=7070 (2011년 글이지만, config.php에 대해 자세히 나와 있다. )

---

#### /application/config/config.php
```php
<?php
//...
$config['sess_cookie_name']         = 'session';
$config['sess_expiration']          = 3600;
$config['sess_expire_on_close']     = TRUE;
$config['sess_encrypt_cookie']      = TRUE;
$config['sess_use_database']        = TRUE;
$config['sess_table_name']          = 'sessions';
$config['sess_match_ip']            = TRUE;
$config['sess_match_useragent']     = TRUE;
$config['sess_time_to_update']      = 300;
//...
```

- `sess_cookie_name` : 쿠키 이름
    - **String** (default **`ci_session`**)
    - 세션이 `쿠키`와 다른 점은 정보를 **세션은 서버**에 **쿠키는 브라우저**에 저장한다는 것
    - 그러나, 세션도 서버에 저장된 정보를 가져오기 위한 키를 브라우저에 저장해야 한다.
        - 이 키는 특정 정보를 갖지 않고, 서버에 저장된 세션 정보를 가져오는 키의 역할
    - 위 코드처럼 쿠키 이름을 `'session'`으로 하면 브라우저 쿠키에 해당 이름으로 저장된다.
- `sess_expiration` : 세션 만료 기한
    - **Integer** (default **`7200`**)
    - `n`초 만큼 세션이 유효. `0`으로 설정하면 **무한대**
- `sess_expire_on_close` : 브라우저 종료시 세션 만료 여부
    - **Boolean** (default **`FALSE`**)
- `sess_encrypt_cookie` : 세션 데이터 암호화 여부
    - **Boolean** (default **`FALSE`**)
    - **TRUE** 로 한다면
        - $config['cookie_secure'] = FALSE;
        - http://m.blog.naver.com/wotjd1005/110187475545
- `sess_use_database` : 세션 정보를 DB에 저장할지 여부
    - **Boolean** (default **`FALSE`**)
    - **TRUE** 로 한다면
        - 미리 세션을 저장할 DB Table 있어야한다.
        - https://codeigniter.com/user_guide/libraries/sessions.html#database-driver
    - **FALSE** 로 한다면
        - 세션 데이터는 서버 메모리에 저장
- `sess_table_name` : 세션 DB 저장 시 DB Table 이름
    - **String** (default **`ci_sessions`**)
- `sess_match_ip` : 세션 정보 가져올 시 IP 일치 확인 사용 여부
    - **Boolean** (default **`FALSE`**)
    - sess_expiration 를 0 으로 설정해서 무한대 세션을 만드려면 **FALSE** 로 설정해야한다.
- `sess_match_useragent` : 세션 정보 가져올 시 User Agent 일치 확인 사용 여부
    - **Boolean** (default **`FALSE`**)
    - **TRUE** 로 한다면
        - 다른 브라우저 사용시 다른 로그인으로 판단
- `sess_time_to_update` : 세션 ID 갱신 주기
    - **Integer** (default **`300`**)
    - [세션 하이재킹][3] 방지
    - [CI 세션 하이재킹][4]
    - `n`초 만큼 세션 ID 유지
        - `n`초 후 DB Table의  session_id 칼럼의 값이 변동
        - 그냥 변경 X
            - 사용자의 액션이 있다면, `n`초 마다 **갱신**
            - 사용자의 액셕이 없다면, `n`초 후 **그대로**
                - `n`초 동안 계속 액션이 없고, sess_expiration 를 지나면 로그아웃
        - 브라우저 쿠키도 자동 적용되기 때문에 그냥 사용하면 됨

<br>

## sess_expiration 와 sess_time_to_update 관계
> 실제 테스트를 통해 내린 결론입니다. 사실과 다를 수 있고, 개인의 서버 환경에 따라 상이할 수 있습니다.

### sess_expiration
- 설정된 초( n초라 지칭)가 넘어가면 **세션이 만료**된다.
- 만약, 세션을 DB에 저장해도 세션 table의 **row가 삭제되지 않는다**.
    - 해당 **row는 유지**되고, `Session.php` 의 `_sess_gc()` 함수 호출에 의해 **일정 확률**(기본 5%)로 세션 Garbage Collector 실행. GC 실행 후 **만료된 세션 모두 제거**.
        - **_sess_gc()** 는 session 객체의 **생성자**에서 **기본 호출되기 때문에** 따로 주석 처리하지 않는다면, session 객체를 사용할 때 기본 수행된다.
            - **session 객체의 사용**은 $this->session->userdata('session_id'); 같은 사용이 일반적.
            - 하지만, **autoload.php**에 session 라이브러리를 사용한다고 하면, **모든 controller**에 들어올 때 session 객체를 사용하는 효과를 준다.
            - 결론적으로, controller에 들어오는 **모든 요청 시 session 객체를 autoload 해** 생성자를 실행시키고 **_sess_gc() 함수를 실행한다**.
    - 하지만, PHP 코드에서 직접 **$this->session->sess_destroy();** 하면 **row가 삭제**된다.

### sess_time_to_update
- 설정된 초( n초라 지칭 )마다 세션을 갱신한다.
- 하지만, 조건부 갱신이다. if ( n초 동안 **사용자의 액션**이 있다 )
    - **TRUE** : n초 후 세션 DB의 `session_id`, `last_activity` 칼럼을 갱신한다.
    - **FALSE** : n초 후 세션 DB는 그대로 유지된다.
    - **사용자의 액션** : 테스트로 확인된 액션은 ajax call 같은 **서버 요청**이다.
    빈 페이지 마우스 클릭, 다른 웹 사이트로의 a 링크 이동 같은 액션에는 갱신되지 않는다.
- **NOTE : ** TRUE 로 세션이 갱신되면 그 **last_activity** 시간을 기준으로 **sess_expiration** 초가 지난 후 세션이 갱신된다.
    - **e.g. : **  sess_expiration 가 60초 sess_time_to_update 가 30초 일 때, 사용자가 30초마다 액션을 취하면  sess_time_to_update의 갱신 시간을 기준으로 60초 동안 세션이 유지되기 때문에 계속 로그인 상태일 수 있다.

#### 테스트
> **Condition**
sess_expiration : 60
sess_time_to_update : 30

|   사용자 이벤트  | 세션 만료 | 세션 갱신 |    로그인   |
|:----------------:|:---------:|:---------:|:-----------:|
| 60초 동안 가만히 |    만료   |     X     |   로그아웃  |
|  30초 마다 클릭  |    유지   |     O     | 로그인 유지 |

#### Conclusion
> CI 세션의 기본값은 **sess_expiration** : 7200초(2시간), **sess_time_to_update** : 300초(5분)이다.
기본적으로 아무것도 안 해도 2시간 동안 세션이 유지되고, 5분마다 사용자의 액션(서버와 통신)이 있다면 세션은 계속 유지된다.

<br>

## Session 관련 질문

아래의 질문들은 개발과정에서 해결 못하던 부분을 [커뮤니티][5]에 물어보려고 작성했던 질문들이다.
실제 올리기 전에 이미 올려진 질문들을 찾아서 나름 해결한 후 자문자답한 내용이다.

#### Q1.
> 세션이 만료 (3600초, 1시간) 되면 `sessions` 테이블 해당 row의 `user_data` 가 빈 값으로 update 됩니다.
2.x 버전에선 만료돼도( sess_destroy() 호출도 동일 ) row 가 삭제되지 않고 그대로 남는 것이 맞나요?
최신 질문을 보면 sess_destroy(); 시 해당 row 가 삭제된다고도 하는데 제 db에서는 그렇지 않아 질문드립니다.

#### A1.
> 해당 table row의 `session_id`와 `last_activity` 값을 확인하면 다릅니다.
session 라이브러리는 autoload 되거나 controller에서 사용하면 해당 controller에 들어오면서 자동으로 세션 객체를 만듭니다.
만약 로그인 로직을 수행해서 세션 table row에 값을 넣어주면 해당 session을 사용합니다. 그렇지 않고 `sess_time_to_update` 시간을 지나면 `session_id` 가 갱신되어 다른 세션으로 간주합니다.
그 후 session 라이브러리의 GC(만료된 세션 지우는 Garbage Collector)가 돌면 만료된 세션들을 모두 지웁니다.
위 상황은 두 가지로 예상됩니다.
1) 사실 GC는 항상 수행되지만 세션을 지우는 로직은 기본 5% 확률로 수행됩니다.
2)  5% 확률로 세션이 지워져도 autoload 되면 새로운 세션이 생성됩니다. 이 경우 새로운 session_id의 세션이 생깁니다.
http://www.codeigniter-kr.org/bbs/view/qna?idx=10810

---

#### Q2.
> 만약 1번 질문에서 다른 분들도 update만 될 뿐( `user_data` 가 빈 값으로 ) row가 삭제되지 않는다면, 그냥 그대로 사용 중인가요?
로그인을 할 때마다 `sessions` 테이블에 row가 쌓이고 expired 된 row는 앞으로 쓸 일이 없는데 지우지 않고 계속 남기면 select 시 성능에 영향을 주지 않을까요?
해당 row를 지우게 되면 `session_id` 가 중복될 수도 있는 문제가 생겨서 그러나요?

#### A2.
> 1번 답변과 비슷합니다.
바로 삭제되지 않는 row들은 session의 GC `_sess_gc();` 함수가 실행되면서 만료시간이 지난 세션들을 모두 지웁니다. 바로 사라지지 않을 뿐 GC가 돌면 삭제됩니다.
`_sess_gc();` 는 항상 실행되지만 내부 함수를 보면 기본 5% 확률로 실제 DB table delete 요청합니다.
더 자주 삭제를 원하면 `var $gc_probability = 5;` 의 5를 50 등으로 변경합니다.
만약 autoload에서 session 라이브러리를 가져온다면 5%도 충분할 것입니다. autoload 때문에 항상 session 라이브러리를 가져오고 이때마다 GC를 수행하기 때문입니다.
http://codeigniter-kr.org/bbs/view/qna?idx=16579&lists_style=

---

#### Q3.
> config.php 에 $config['sess_time_to_update']	= 300; "세션이 얼마나 자주 재생성되어 새로운 세션 아이디를 만들지를 결정합니다."
이 부분이 어떤 상황에서 필요한 거죠??
일단 제가 해당 값을 30으로 변경하고 확인하니 실제로 db의 `session_id`와 브라우저 쿠키의 값이 갱신되는 것을 확인했습니다.

#### A3.
> 이 문서를 순서대로 읽었다면, `sess_expiration와 sess_time_to_update 관계`에서 sess_time_to_update 가 어떤 의미인지 알 수 있습니다.
추가적으로, 테스트할 때 **sess_expiration = 60**, **sess_time_to_update = 300**으로 두고 하지 마세요. (저도 그렇게 테스트했었습니다.)
결론적으론 sess_expiration **>** sess_time_to_update 해야 정확한 테스트가 됩니다.
만료시간이 60 초고 갱신주기가 300초이면 갱신 전에 만료되는 건 당연합니다.
하지만, 테스트하다 보면 이런 실수가 생길 수 있습니다.
http://codeigniter-kr.org/bbs/view/qna?idx=10898

<br>

#### Conclusion
> 세션을 적용할 때 공식문서대로 했는데 다른 분들과 결과가 달라 삽질을 많이 했습니다.
2.x 와 3.x 버전의 차이 때문이라고도 생각했는데 그런 이슈는 하나도 없었습니다.
정확하게 옵션에 대해 모르고 테스트했기 때문에 다른 결과가 나왔고 Session.php 자체를 수정한 부분(다른 분이 해서 몰랐던..)이 있어서 다른 결과가 나왔습니다.


<br>

## Session.php

#### /system/libraries/Session.php
```php
<?php
// session GC 수행 확률. 테스트 위해서 95로 임시변경

var $gc_probability				= 5;
var $gc_probability				= 95;


// session GC 실행 라인 주석 제거

// $this->_sess_gc();
$this->_sess_gc();
```
> 위의 방법은 Session.php Core를 건드리는 방법이기 때문에 지양한다.

<br>

#### /system/libraries/Session.php
```php
<?php
function _sess_gc()
{
    if ($this->sess_use_database != TRUE)
    { // 테이블 사용할 때만
        return;
    }

    srand(time()); // 랜덤
    if ((rand() % 100) < $this->gc_probability) // 기본 5% 확률
    { // 5% 확률로 세션 GC
        $expire = $this->now - $this->sess_expiration;

        $this->CI->db->where("last_activity < {$expire}");
        $this->CI->db->delete($this->sess_table_name);

        log_message('debug', 'Session garbage collection performed.');
    }
}
```
> _sess_gc() 함수에 의해 세션 DB table의 row가 삭제된다.

<br>

#### /application/core/MY_Controller.php

```php
<?php
class MY_Controller extends CI_Controller {
    public function __construct()
    {
        parent::__construct();

        $this->load->library('session');
        $this->session->gc_probability = 100; // 100% 확률
		$this->session->_sess_gc(); // DB GC 실행
    }
}
```
> session Core 건들지 않고 사용하는 방법
모두 public 변수, 함수이기 때문에 가능


  [1]: https://ko.wikipedia.org/wiki/HTTP_%EC%BF%A0%ED%82%A4
  [2]: https://ko.wikipedia.org/wiki/%EC%84%B8%EC%85%98
  [3]: http://marcof.tistory.com/11
  [4]: http://codeigniter-kr.org/bbs/view/qna?idx=10848
  [5]: http://codeigniter-kr.org/
