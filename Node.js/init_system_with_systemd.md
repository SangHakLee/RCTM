# Node.js Service with systemd

## Intro

### 문제점
> 시스템이 다시 시작될 때 Node.js 앱을 자동으로 시작해야함 (윈도우의 자동 시작 프로그램 개념)
`pm2`(혹은 `forever`) 가 Node.js 프로세스를 관리해주지만, 서버 자체의 재시작이나 오토스케일 된 서버에서 Node 프로세스를 관리하기 어렵다.

### 해결책
> 프로세스 관리자에서 앱을 실행한 후, init 시스템을 통해 프로세스 관리자를 서비스로서 설치하십시오. 프로세스 관리자는 앱에서 충돌이 발생할 때 앱을 자동으로 시작하고, init 시스템은 OS가 다시 시작될 때 프로세스 관리자를 다시 시작할 것입니다. 이러한 접근법은 권장되는 접근법입니다.
출처 : http://expressjs.com/ko/advanced/best-practice-performance.html

여기서 이야기하는 프로세스 관리자는 [pm2](https://github.com/Unitech/pm2), [forever](https://github.com/foreverjs/forever) 같은 모듈을 말한다.

<br>

## How to use ( with [systemd](https://ko.wikipedia.org/wiki/Systemd) )
[Run node.js service with systemd](https://www.axllent.org/docs/view/nodejs-service-with-systemd/)

`$ vi /etc/systemd/system/nodeserver.service`
```bash
[Unit]
Description=Node.js Example Server
#Requires=After=mysql.service       # Requires the mysql service to run first

[Service]
ExecStart=/root/.nvm/versions/node/v4.4.7/bin/pm2 start /home/lsh/simple-server.js
Restart=always
RestartSec=10                       # Restart service after 10 seconds if node service crashes
StandardOutput=syslog               # Output to syslog
StandardError=syslog                # Output to syslog
SyslogIdentifier=nodejs-example
#User=<alternate user>
#Group=<alternate group>
Environment=PATH=/root/.nvm/versions/node/v4.4.7/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
Environment=NODE_ENV=production PORT=1337

[Install]
WantedBy=multi-user.target

```

### 주의사항

- 실핼 스크립트는 절대 경로 사용
	> $ node server.js **(X)**

	> $ /usr/local/bin/node server.js **(O)**

- Environment 에 PATH 지정
	> 정확한 이유는 모르나, 아래의 내용 없으면 에러
	> PATH=/root/.nvm/versions/node/v4.4.7/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
	> Node.js path는 각자 설치된 경로로 사용할 것.
