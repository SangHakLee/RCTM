# How to count every checked checkboxes in input tag using jQuery

체크된 input tag 개수
```javascript
    $('input[type="checkbox"]:checked').length
```

<br/>

체크된 input tag 리스트 순회
```javascript
    $('input:checkbox[type="checkbox"]:checked').each(function() {
        console.log( $(this) );
    })
```
