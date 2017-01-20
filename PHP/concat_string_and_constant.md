# How to concatenate  string value and constant value in a class?
http://stackoverflow.com/questions/3288495/parse-error-when-concating-a-defined-variable-and-string-in-a-class

## Error Case

```php
<?php

define('PREFIX', 'hello');

class Hak {
	private $_msg = PREFIX . ' world';
}
```

> Parse error: parse error, expecting `','` or `';'`

<br>

### Why?

> You can use constants, but not functions or other variables when pre-defining class variables. Also you cannot use operators like the `.` dot string concatenation operator.
Class definitions are like blueprints, they must be completely independent from anything else going on in the script (except for dependencies on other classes of course.)

> 상수는 함수나 다른 미리 선언된 클래스에서는 사용할 수 없다.
클래스 정의는 `청사진`과 비슷하다. 클래스 정의는 반드시 독립적이어야 한다.

<br>

## Solution

```php
<?php

define('PREFIX', 'hello');

class Hak {
    private $_msg;

	function __construct()
	{
		$this->_msg = PREFIX . ' world';
	}
}
```
> `hello world`

<br>

### Why?
> in the constructor

> 변수의 정의를 클래스 생성자 레벨에서 하면 상수를 사용할 수 있다.
이 내용은 PHP OOP 스타일이기 때문에 이렇게 해야한다.
http://php.net/manual/kr/language.oop5.php
