# How to called ajax synchronous with beforeSend() option ?
http://stackoverflow.com/questions/16046387/jquery-ajax-beforesend

## Error Case

```javascript
function showLoading () {
	// show loading
}
function hideLoading() {
	// hode loading
}

$.ajax({
	type: 'POST',
	url: 'localhost/publish'
	data: {'name' : 'hak'},
	async: false,
	beforeSend: function() {
		showLoading();
	},
	success: function() {
		// Do when ajax call success
	},
	error: function() {
		// Do when ajax call fail
	},
	complete: function() {
		hideLoading();
	}
})
```

<br>

### Why?
> This is most probably because of async : false. As your call is synchronous, after your call to the $.ajax() function begins, nothing happens until the response is received, and the next thing as far as your code goes will be the success handler

> async : '`false`' 때문에 발생한 가능성이 크다. ajax 옵션의 `sync` 는 ajax 통신의 동기 / 비동기 여부에 대한 옵션이다.
`default` 값은 `true` 이고 이 경우 ajax call을 비동기로 처리한다.
만약, `async`의 값에 `false`를 넣으면 해당 ajax call을 동기로 처리해서 해당 요청에 대한 응답이 있을 때까지 모든 처리가 중단된다.
따라서,  `showLoading()` 의 함수 처리가 async : '`false`' 때문에 정지될 가능성이 높다.

<br>

## Solution

### 1. Remove `async: false` Option

```javascript
//...

// Remove async option. default true
$.ajax({
	type: 'POST',
	url: 'localhost/publish'
	data: {'name' : 'hak'},
	beforeSend: function() {
		showLoading();
	},
	//...
	complete: function() {
		hideLoading();
	}
})

// Or

//...

// Set async option true
$.ajax({
	type: 'POST',
	url: 'localhost/publish'
	data: {'name' : 'hak'},
	async: true,
	beforeSend: function() {
		showLoading();
	},
	//...
	complete: function() {
		hideLoading();
	}
})
```

<br>

### 2. Remove `beforeSend` function. Make synchronous!

```javascript
//...

showLoading(); // synchronous
$.ajax({
	type: 'POST',
	url: 'localhost/publish'
	data: {'name' : 'hak'},
	async: false,
	//...
	complete: function() {
		hideLoading();
	}
})
```

<br>

### Why?
> 해결책은 간단하게 생각하면 된다. ajax를 `async`(비동기) 하게 요청하면 다른 부분도 async 하게 만들고
ajax를 `sync`(동기) 하게 요청하면 다른 부분도 sync 하게 만든다.
