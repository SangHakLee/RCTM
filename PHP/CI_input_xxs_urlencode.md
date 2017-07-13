# How to use Special Characters when using CI xxs filtering.

CI에서 서버로 넘어온 데이터에 대한 [xxs 공격](https://namu.wiki/w/XSS)을 막기 위해선 아래와 같이 사용한.
```php
<?php
$post_data = $this->input->post(null, true);
```

**그러나,** 여기서 한가지 문제점이 발생할 수 있는데 xxs 필터링 옵션을 주면
특수문자를 원본과 다르게 변경하기 때문에 문제가 발생할 수 있다.

<br>

따라서 다음과 같이 JavaScript에서 [encodeURIComponent()](encodeURIComponent) 한 후 데이터를 보낸다.

**web.js**
```JavaScript
$.ajax({
    type: 'POST',
    url: '/request',
    data: {
        name: $('#name').val(),
        pw:  encodeURIComponent( $('#pw').val() ),
        msg:  encodeURIComponent( $('#msg').val() )
    }
})
```
- 이렇게 보내면, `pw`와 `msg`의 내용이 그대로 보내지지 않고 URI 인코딩 형태로 보내지기 때문에
CI에서 거르지 않는다.

**server.php**
```php
<?php
public funtion request()
{
    $name = $this->input->post('name', true);
    $pw = $this->input->post('pw', true);
    $msg = $this->input->post('msg', true);
}
```
- `$pw`, `$msg`는 온전한 형태로 받아진다.

<br>

**추가 정보**
CI 서버에서 받은 데이터를 다시 다른 CI 서버로 보낼 때도 그냥 보내면 xxs 필터링에 걸리기 때문에,
다음과 같이 [rawurlencode()](http://php.net/manual/kr/function.rawurlencode.php)로 보낸다.
```php
<?php
$params = array(
    'name' => $name,
    'pw' => rawurlencode($pw),
    'msg' => rawurlencode($msg),
);
```
