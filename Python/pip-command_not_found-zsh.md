# pip로 설치한 모듈이 제대로 동작하지 않는 경우

## 문제
```bash
$ pip3 install pipenv

$ pipenv
zsh: command not found: pipenv
```

**시스템 환경변수 PATH에 Python 경로가 없기 때문에**


## 해결
```bash
$ vi .zshrc

export PATH="/Users/$(whoami)/Library/Python/3.8/bin:$PATH
```
