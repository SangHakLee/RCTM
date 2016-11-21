# Error: Fatal error: Can't use function return value in write context in

- Note: Prior to PHP 5.5, empty() only supports variables; anything else will result in a parse error. In other words, the following will not work: empty(trim($name)). Instead, use trim($name) == false.
- PHP 5.5 이전 버전에서는 empty() 함수는 변수만 지원한다.
  즉, empty() 함수 내부에 또 함수를 쓰면 parse 에러가 발생한다.

<br>


```php
<?php  if ( ! defined('BASEPATH')) exit('No direct script access allowed');

// Bad
if ( empty( trim($val) ) ) {
    //...
}

// Good
if ( trim($val) == false ) {
    //...
}
// Or
$new_val = trim($val);
if ( empty( $new_val ) ) {
    //...
}
```
