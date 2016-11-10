# Dom change listener

- jQuery
```javascript
    $(document).on('DOMSubtreeModified', 'table', function(){
        console.log('changed!!')
    })
```

<br>

- JavaScript
```javascript
    document.body.addEventListener('DOMSubtreeModified', function () {
        console.log('changed!!')
    }, false);
```
