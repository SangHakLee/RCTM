# PHP strpos()
http://php.net/manual/kr/function.strpos.php

> 위치를 정수로 반환합니다. needle을 발견하지 못하면, strpos()는 boolean FALSE를 반환합니다.
> **Warning**
이 함수는 논리 FALSE를 반환하지만, 논리 FALSE로 취급할 수 있는 다른 값을 반환할 수 있습니다. 자세한 정보는 논리형 섹션을 참고하십시오. 이 함수의 반환값을 확인하려면 === 연산자를 이용하십시오.

**찾으면 TRUE를 반환하는 게 아니라, 찾지 못했을 때 FALSE를 반환한다.**
왜 이렇게 만들었는지 정말 이해가 안가지만, 어쩔 수 없이 사용한다.
반드시! **`비교 연산에 FALSE를 쓴다.`** 또한 반드시 **`=== 연산자를 사용한다.`**


https://repl.it/ODgv/3

```php
$haystack = "안녕하세요.";   
$needle = "안녕";   

// 옳은 사용법
if(strpos($haystack, $needle) !== FALSE) {   
    echo "O";   
} else {   
    echo "X";   
}

// 옳은 사용법
if(strpos($haystack, $needle) === FALSE) {   
    echo "X";   
} else {   
    echo "O";
}

// 옳지않은 사용법: == 연산자 사용, === 사용할 것
if(strpos($haystack, $needle) == FALSE) {   
    echo "X";   
} else {   
    echo "O";
}

// 옳지않은 사용법: TRUE로 비교, FALSE 사용할 것
if(strpos($haystack, $needle) === TRUE) {   
    echo "O";   
} else {   
    echo "X";   
}
```
