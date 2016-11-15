# How to access jQuery tmpl datas using each statement

- 다음과 같은 데이셋이 주어진 경우

```javascript
    var info = {
        $index : 4,
        info_name   : 'hak'
        friend : [
            {
                name : 'one',
                age  : 21
            },
            {
                name : 'two',
                age  : 23
            }
        ]
    }
```

<br>

info Object는 상위에 datas에 속해 있다.
```javascript
    var datas = [
        {info},
        {info},
        ...
    ]
```

info의 바로 하위 데이터는
```html
    <script type="text/x-jquery-tmpl">
        <p> ${info_name} </p>
    </script>
```

<br>

info의 하위의 array 데이터는
info의 바로 하위 데이터는
```html
    <script type="text/x-jquery-tmpl">
        {{each friend }}
            <p> ${$value.name} </p>
            <p> ${$value.age} </p>
        {{/each}}
    </script>
```


- 가져오는 데이터가 Array 면 each 를 사용한다.
- each 문에선 ${$value.} 이라는 key로 각 object에 접근한다.
- http://skilldrick.co.uk/tmpl/#slide1
