# Docker로 Sentry, Mattermost 구축하기
## 어플레케이션
1. Sentry
1. Mattermost

### Sentry
> Sentry provides self-hosted and cloud-based error monitoring that helps all software teams discover, triage, and prioritize errors in real-time.
> https://sentry.io 

#### 용도
어플리케이션에서 발생하는 오류를 모니터링하기 위핸 어플리케이션이다.

필자의 회사는 PHP CI를 사용해 웹 프로젝트를 운영한다.
PHP는 스크립트 언어로 별도의 컴파일 과정 없이 소스코드 파일 수정만으로 실제 서비스에 적용된다.

따라서, 런타임에서 발생하는 오류를 모두 잡아서 Sentry로 넘기는 작업을 진행했다.
또한, JavaScript에서 발생하는 오류도 모두 PHP 서버로 보내서 Sentry에 넘긴다.
물론, JavaScript용 SDK를 제공하지만, 하나의 도메인에 해당하는 프로젝트에 대한 모든 에러를 하나의 Sentry 프로젝트에서 확인하고 싶어서 JavaScript오류를 PHP로 보낸 것이다.

![Alt text](./sentry-new-project.png)
- 다양한 플랫폼에 적용 가능하다.



#### 이유
Sentry 서비스는 기본 기능은 무료지만, 더 많은 기능을 사용하기 위해서 유료 플랜을 선택해야한다. https://sentry.io/pricing

하지만, Sentry 서비스에서 제공하는 서버를 사용하지 않고 Self-host로 이용하면 모든 기능을 이용할 수 있다. [How much does it cost to run Sentry on-premise?](https://help.sentry.io/hc/en-us/articles/115006979128-How-much-does-it-cost-to-run-Sentry-on-premise-)


### Mattermost
> Mattermost is a flexible, open source messaging platform
that enables secure team collaboration
> https://mattermost.com

#### 이유
Slack이 Self-host 서비스를 제공하지 않기 때문에 찾은 대안 프로젝트이다.
Mattermost는 30일 무료 이용 기간이 존재하고 그 이후엔 유료로 사용해야한다.
이미 Slack이라는 더 유명하고 많이 사용되는 서비스가 있기 때문에 이 유료 플랜을 사용할 이유는 없다고 보여진다. 

Mattermost를 선택한 이유는 Self-host 서비스로 이용 시 무료고 Slack에서 Self-host 서비스를 제공하지 않기 때문에 선택한 것이다


### Gitalb

## Docker 환경 구축 시 Tip
1. 충분한 공간 확보
2. 


### Conclusion

Sentry, Gitlab, Mattermost, ELK 서비스들은 일부 제한된 기능을 제공하지만, 모두 무료로 사용할 수 있다.

각 서비스에서 제공하는 무료 플랜을 이용하면 가입만으로 쉽게 서비스를 이용할 수 있고, 서비스의 패치 시 업그레이드에 대한 부담감을 줄일 수 있다.

하지만, 일부 기능이 제한되고 해당 서비스를 운영 중에 축적되는 데이터들이 해당 서비스를 제공하는 회사의 서버에 저장된다.

스타트업 같은 작은 규모의 회사에선 위에서 설명한 서비스들을 직접 구축/운영 하기도 힘들고 추가적인 서버를 운영해야하는 부담감이 있을 수 있다.

그러나, 어느 정도 규모가 있는 기업은 자사의 데이터가 외부 서비스
