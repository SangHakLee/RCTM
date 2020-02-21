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
dict.get('age') or 30 # 30
```

`or` 연사자를 이용해 딕셔너리의 항목에 기본값을 줄 수 있다.


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


<iframe height="500px" width="100%" src="https://repl.it/@ryan0425/dict-get?lite=true" scrolling="no" frameborder="no" allowtransparency="true" allowfullscreen="true" sandbox="allow-forms allow-pointer-lock allow-popups allow-same-origin allow-scripts allow-modals"></iframe>

---

<iframe height="500px" width="100%" src="https://repl.it/@ryan0425/JS-object-default?lite=true" scrolling="no" frameborder="no" allowtransparency="true" allowfullscreen="true" sandbox="allow-forms allow-pointer-lock allow-popups allow-same-origin allow-scripts allow-modals"></iframe>
