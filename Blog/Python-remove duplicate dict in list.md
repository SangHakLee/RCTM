# Python 리스트 딕셔너리 중복 제거 
> python how to remove duplicate dict in list
> 
> https://stackoverflow.com/questions/11092511/python-list-of-unique-dictionaries

## dict 전체 중복

```python
list(map(dict, set(tuple(sorted(d.items())) for d in data)))
```


<iframe height="400px" width="100%" src="https://repl.it/@ryan0425/how-to-remove-duplicate-dict-in-list?lite=true" scrolling="no" frameborder="no" allowtransparency="true" allowfullscreen="true" sandbox="allow-forms allow-pointer-lock allow-popups allow-same-origin allow-scripts allow-modals"></iframe>

```python
data = [
	{'id': 1, 'name': 'hak', 'age': 30},
	{'id': 2, 'name': 'woo', 'age': 31},
	{'id': 3, 'name': 'ho', 'age': 29},
	{'id': 1, 'name': 'hak', 'age': 30},
]

data = list(map(dict, set(tuple(sorted(d.items())) for d in data)))

print(data)
```

## dict 특정 key 중복

```python
list({v['id']:v for v in data}.values())
```
- 이 방법은 특정 `key`에 대한 중복 제거기 때문에 사용하지 않는다.
- 만약, 특정 key가 중복인 경우를 중복으로 바라본다면 이 방법을 사용한다.

