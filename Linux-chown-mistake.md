# [Linux] 리눅스 소유권 변경 chown 실수

>  숨긴 파일 포함 디렉토리의 모든 파일, 하위 디렉토리 소유권 변경
> 
> How to chown a directory recursively including hidden files or directories

## 실수
필자가 소유권을 변경할 때 사용했던 명령어다.

```bash
$ chown -R hak. ./*
$ chown -R hak. ./.* 
```
- **-R**: 재귀적으로 하위 디렉토리 모두 적용
- **{user}.**: 소유권과 동일하게 그룹 소유권도 변경
- **./***: 현재 위치에서 문자로 시작하는 모두
	-	`./`: 현재 위치
	-	`*`:  와일드카드, 모든 문자와 일치하는
-	**./.***: 현재 위치에서 `.`으로 시작하는 모두
	-	`./.`: 현재 위치의 숨긴 파일


필자의 의도는 현재 위치의 모든 디렉토리와 파일의 소유권을 변경하고, 디렉토리는 재귀적으로 하위 모든 것들의 소유권도 동일하게 변경하려는 것이었다.

**하지만, 필자의 명령어는 아주 큰 함정이 있었다.**

**./***은 현재 위치의 모든 디렉토리와 파일을 변경할 것으로 예상되지만 그렇지 않다.
리눅스에는 `.`으로 시작하는 파일이 있다. 숨긴 파일을 의미하는데 `./*`에는 적용되지 않는다. 

그래서 필자는 숨긴 파일은 `.`으로 시작하기 때문에 `./.*`을 사용하면 적용될 것이라 생각했다.
물론 필자의 의도대로 적용됐다.

**하지만, 이것은 정말 위험한 행동이다. `./.*`의 의미는 `.`으로 시작하는 모든 것이다.**

즉 `./.abc`, `./.123`, `./..` 등 `.`으로 시작하는 모든 것에 적용된다. 
**./..**!!

리눅스에서 `..`의 의미는 상위 디렉토리다. 즉, `./.*`은 현재 위치의 숨긴 파일과 **상위 디렉토리(!!)**를 모두 포함한다.
필자의 의도와 다르게 현재 위치의 상위 디렉토리까지 모두 소유권을 변경시킨 것이다.

만약, 이 명령어를 root 권한으로 실행했다면 엄청난 오류를 발생시킬 수도 있었을 것이다.

### 원래 의도
현재 위치의 숨긴 파일을 포함한 모든 디렉토리와 파일의 소유권을 수정하려면 아래 명령어를 이용한다.
```bash
$ chown -R hak .

$ chown -R hak:hak .

$ chown -R hak. .
```


## chown 사용법
> $ chown [optoin] {owner}[:{group}] file

[chown](https://www.freebsd.org/cgi/man.cgi?query=chown&sektion=8)

### 예제
```bash
$ chown hak README.md
```
- `README.md` 소유권을 `hak`으로 변경

```bash
$ chown hak:lee README.md
```
- `README.md`  소유권을 `hak`으로 그룹 소유권을 `lee`로 변경

```bash
$ chown hak. README.md
```
- `README.md`  소유권을 `hak`으로 변경하고 그룹 소유권을 소유권과 동일하게 변경

```bash
$ chown -R hak /dir
```
- `R`: --recursive, 하위 디렉토리 & 파일 모두 변경

```bash
$ chown hak .
```
- 숨긴 파일 포함 현재 경로의 모든 파일 소유권 변경

### Conclusion

> 곤경에 빠지는 건 뭔가를 몰라서가 아니다. 뭔가를 확실히 안다는 착각 때문이다.
> It ain't what you don't know that gets you into trouble. It's what you know for sure that just ain't so.
> 
> The Big Short
