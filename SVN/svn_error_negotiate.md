# svn: Cannot negotiate authentication mechanism

## Problem

- svn: Cannot negotiate authentication mechanism
- svn: 인증 방법 협상 불가능

<br>

## Solution

SASL 인증 문제
- https://en.wikipedia.org/wiki/Simple_Authentication_and_Security_Layer
- http://linuxism.tistory.com/368

`cyrus-sasl-md5`

### CentOS
```
$ rpm -qa | grep cyrus-sasl-md5

$ yum install cyrus-sasl-md5
```
