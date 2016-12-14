# PHP ternary operators

- PHP 에서 간단한 비교식에서 3항 연산자를 쓴다.
- 자주 쓰는 형태는 다음과 같이 특정 값의 유/무 에 따라 값을 지정할 때 사용한다.

```php
<?php  if ( ! defined('BASEPATH')) exit('No direct script access allowed');

$page = get_page() ? get_page() : 1; // get_page() 는 페이지를 가져오는 함수

```

<br>

- 위의 표현식은 get_page() 가 있으면 그것을 쓰고 없으면 1로 대체하겠다는 의미인데
더 간단하게 표현할 수 있다.

```php
<?php  if ( ! defined('BASEPATH')) exit('No direct script access allowed');

$page = get_page() ?: 1;

```

<br>

- CI 에서도 이런 표현식은 자주 사용된다.

```php
<?php  if ( ! defined('BASEPATH')) exit('No direct script access allowed');

// Bad
$page = $this->input->post('page', true) ? $this->input->post('page', true) : 1;

// Good
$page = $this->input->post('page', true) ?: 1;


```
