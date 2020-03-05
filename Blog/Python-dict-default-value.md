# [Python] 딕셔너리 기본 값
> python dict default value when you access item

파이썬에서 자바스크립의 [Object](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Object)(객체)와 유사한 자료형은 [Dictionary](https://wikidocs.net/16)(딕셔너리)다.

---

```python
dict = {
	'name': 'hak'
}
dict.get('name') # 'hak'
dict.get('age') # ''
dict.get('age', 30) # 30
dict.get('age') or 30 # 30
```
- [.get()](https://docs.python.org/3/library/stdtypes.html?highlight=get#dict.get) 메소드의 두번째 파라미터에 기본 값을 줄 수 있다.
	- 두번째 파라미터의 디폴트는 [None](https://docs.python.org/3/c-api/none.html)이고, 디폴트 값이 있기 때문에 [KeyError](https://docs.python.org/3/library/exceptions.html#KeyError)는 발생하지 않는다.
- [or](https://python-reference.readthedocs.io/en/latest/docs/operators/or.html) 연사자를 이용해 딕셔너리의 항목에 기본 값을 줄 수 있다.
	- `.get()`의 디폴트 `None`은 논리 연산 시 [False](https://docs.python.org/3/library/constants.html?highlight=false#False)로 판별되는 원리를 이용해 기본 값을 설정한다.
	- Javascipt에서 다음과 같이 기본 값을 설정하는 원리와 동일하다.
	```javascript
	var name = dict.age || 30
	```

**`KeyError`를 발생시키지 않는 `.get()`를 사용하는 것을 추천한다.**


## 딕셔너리 사용법

파이썬 딕셔너리의 Item(항목)에 접근할 때, 자바스크립트와 비슷한 방법을 사용할 수 있다.

```python
# python
dict = {
	'name': 'hak'
}
dict['name'] # hak
```

### 자바스크립트와 비교

자바스크립트를 주로 사용하면 무의식적으로 다음과 같은 방법으로 항목에 접근하려고 할 수 있는데, 오류가 발생한다.

```python
# python
dict = {
	'name': 'hak'
}
dict.name # AttributeError: 'dict' object has no attribute 'name'
```
- 문법 오류가 아니다. 파이썬의 딕셔너리는 `.` 이용하면 method를 호출한다.
	-  `.`로 접근하게 할 수 있지만, 굳이 이렇게 접근하려고 하지 말자! [How to use a dot “.” to access members of dictionary?](https://stackoverflow.com/questions/2352181/how-to-use-a-dot-to-access-members-of-dictionary)
- [Python Dictionary Methods](https://www.w3schools.com/python/python_ref_dictionary.asp), [딕셔너리 관련 함수들](https://wikidocs.net/16#_8)
- 문법 오류였다면, `SyntaxError: invalid syntax` 발생

---

```
// javascript
var name = dict.name || 'hak'

# python
name = dict.get('name') or 'hak'
```

> 파이썬이 얼마나 [고급 프로그래밍 언어](https://ko.wikipedia.org/wiki/고급_프로그래밍_언어)인지 보여준다.
> `.get()`, `or` 매우 직관적이다.


<iframe height="400px" width="100%" src="https://repl.it/@ryan0425/dict-get?lite=true" scrolling="no" frameborder="no" allowtransparency="true" allowfullscreen="true" sandbox="allow-forms allow-pointer-lock allow-popups allow-same-origin allow-scripts allow-modals"></iframe>

---

<iframe height="400px" width="100%" src="https://repl.it/@ryan0425/JS-object-default?lite=true" scrolling="no" frameborder="no" allowtransparency="true" allowfullscreen="true" sandbox="allow-forms allow-pointer-lock allow-popups allow-same-origin allow-scripts allow-modals"></iframe>
