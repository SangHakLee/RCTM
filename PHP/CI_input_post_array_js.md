# How to parse array value in CI input post when using ajax call

- passing array value using ajax
```javascript
var array = ['abc', '123'];

// ajax call
var options = {
    url      : 'domain/mycontroller',
    dataType : 'json',
    type     : 'post',
    data     : array
}
```

- receive post value
```php
<?php defined('BASEPATH') OR exit('No direct script access allowed');
// Bad
$array = $this->input->post('array', true); // ['abc', '123']  String

// Good
$array = $this->input->post('array', true); 
$array = (array) json_decode($array); // Array( [0] => 'abc', [1] => '123' )

```
