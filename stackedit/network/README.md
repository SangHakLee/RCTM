# Day01

이용식 강사님 (jesuswithme@cloudshell.kr)

4차 산업 혁명
- 연결
- 공유
- 수평적 관계

---

네트워크 연결 바로가기
1. win + R
1. ncpa.cpl

---

- 스위치: 연결 시 바로 네트워크 불가, 보안
-  허브: 연결 시 바로 네트워크 가능

---

- 핫스팟: 공유기, AP, 1:n
- 테더링: 1:1

**powershell**
```powershell
PS C:\Windows\system32> Get-Command -CommandType application

PS C:\Windows\system32> cls

PS C:\Windows\system32> Get-Command -CommandType application | Where-Object {$_.Name -like "*.cpl"}

PS C:\Windows\system32> Get-Command -CommandType application | Where-Object {$_.Name -like "*.msc"}

PS C:\Windows\system32> Get-Command -CommandType application | Where-Object {$_.Name -like "*.exe"}
```
- cls : clear screen`ctrl + l`
- cpl: cpl로 끝나는 명령어 찾기

---

ping
- icpm 패킷
- layer 3까지만 확인
- 네트워크 연결 전 해당 주소까지 찾아갈 수 있는지만 확인
- TTL: 64를 기준으로 라우터를 하나씩 통과할 때마다 1씩 줄어듬
	- 64(linux server)
	- 128(window server)

tracert
- 라우터 경로 추적

---

## powershell

### cmd & powershell
> 쉘에 **PS**가 붙으면 powershell
> cmd, powershell에서 서로 전환 가능
> `> cmd`, `> powershell` 입력 시 전환

#### powershell
- 동사-명사
- opensource (확장 가능)
- script 최적화
- 프로그램 개발용
- cmd와 호환
- Linux 가능

### 명령어
#### 컴퓨터 이름 변경
```powershell
PS C:\Windows\system32> hostname
DESKTOP-LEHC97F

PS C:\Windows\system32> Rename-Computer -NewName st1 -Restart
```

#### 컴퓨터 계정 확인 & 암호 변경
```powershell
PS C:\Windows\system32> whoami
st1\wtime

PS C:\Windows\system32> net users

\\ST1에 대한 사용자 계정

-------------------------------------------------------------------------------
Administrator            DefaultAccount           Guest
WDAGUtilityAccount       wtime
명령을 잘 실행했습니다.
친

PS C:\Windows\system32> net users wtime *
사용자에 대한 암호를 입력하십시오:
암호를 확인하기 위해 다시 입력하십시오:
명령을 잘 실행했습니다.
```
- wtime * : wtime 계정의 모든 암호 바꾸기

#### 관리자 계정 활성화
```powershell
PS C:\Windows\system32> net users administrator
사용자 이름                        Administrator
전체 이름
설명                               컴퓨터 도메인을 관리하도록 기본 제공된 계정
사용자 설명
국가/지역 코드                     000 (시스템 기본값)
활성 계정                          아니요
계정 만료 날짜                     기한 없음

마지막으로 암호 설정한 날짜        2019-02-18 오후 1:35:10
암호 만료 날짜                     기한 없음
암호를 바꿀 수 있는 날짜           2019-02-18 오후 1:35:10
암호 필요                          예
사용자가 암호를 바꿀 수도 있음     예

허용된 워크스테이션                전체
로그온 스크립트
사용자 프로필
홈 디렉터리
최근 로그온                        아님

허용된 로그온 시간                 전체

로컬 그룹 구성원                   *Administrators
글로벌 그룹 구성원                 *없음
명령을 잘 실행했습니다.

PS C:\Windows\system32> net users administrator /active:yes
```
- `/active:yes` - 활성 계정 `아니오` -> `예`

#### 파일 찾기 & 복사
```powershell
PS C:\Windows\system32> Get-ChildItem -Path C:\Windows\ -Filter *.jpg -Recurse

PS C:\Windows\system32> mkdir C:\pics

PS C:\Windows\system32> Get-ChildItem -Path C:\Windows\ -Filter *.jpg -Recurse | Copy-Item -Destination C:\pics
```
- `Copy-Item`: .jpg 모두 찾아서 C:\pics에 저장

#### 도움말 업데이트
```powershell
PS C:\Windows\system32> Update-Help -Force
```

#### powershll에서 vim 사용하기
http://powershell.kr/

```powershell
PS C:\Windows\system32> Set-ExecutionPolicy Bypass -Scope Process -Force

PS C:\Windows\system32> iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))

PS C:\Windows\system32> choco install firefox -y
```
- `choco`: 초코를 사용해서 shell로 패키지 다운로드

#### 명령어 찾기
```powershell
PS C:\Windows\system32> Find-Command -Name memory

계속하려면 NuGet 공급자가 필요합니다.
NuGet 기반 리포지토리를 조작하려면 PowerShellGet에 NuGet 공급자 버전 '2.8.5.201' 이상이 필요합니다. 'C:\Program
Files\PackageManagement\ProviderAssemblies' 또는 'C:\Users\wtime\AppData\Local\PackageManagement\ProviderAssemblies'에서 NuGet
공급자를 사용할 수 있어야 합니다. 또한 'Install-PackageProvider -Name NuGet -MinimumVersion 2.8.5.201 -Force'를 실행하여 NuGet
공급자를 설치할 수 있습니다. 지금 PowerShellGet에서 NuGet 공급자를 설치하고 가져오시겠습니까?
[Y] 예(Y)  [N] 아니요(N)  [S] 일시 중단(S)  [?] 도움말 (기본값은 "Y"): Y


PS C:\Windows\system32> Find-Module -Name *memory*

Version    Name                                Repository           Description
-------    ----                                ----------           -----------
0.5.0      MemoryTools                         PSGallery            A set of functions for checking, testing and reporting memory ...
```


## Hyper-V

### Hyper-V 설치
프로그램 및 기능
1. win + R
1. appwiz.cpl

Windows 기능 켜기/끄기
- Hyper-V
- Linux용 Windows 하위 시스템

![프로그램 및 기능](https://user-images.githubusercontent.com/9030565/52929463-dcd41980-3387-11e9-8e88-a98468f98844.png)

### Hyper-V 관리자 실행
1. win + S
1. `Hyper-V 관리자` 검색

![Hyper-V 관리자](https://user-images.githubusercontent.com/9030565/52929668-b2369080-3388-11e9-8607-c53704209888.png)

#### 가상 스위치 만들기
1. 외부 네트워크
	-  이름: `External Network`
1. 내부 네트워크
	- 이름: `Private Network`

![가상 스위치 만들기](https://user-images.githubusercontent.com/9030565/52929813-40ab1200-3389-11e9-9710-77fbd9ce9d1b.png)

#### 공유 폴더 접근

> 공유 폴더 설정은 `c:\` 아래에서만 가능

강사님 컴퓨터 이름이 teacher이기 때문에 접근 가능

![공유 폴더](https://user-images.githubusercontent.com/9030565/52930287-31c55f00-338b-11e9-8cfd-876504234f49.png)
- 두 파일 복사
	- Windows Server 2012 원본 모음
	- Base14A-WS12R2

### 가상 컴퓨터 만들기

![가상 컴퓨터 만들기1](https://user-images.githubusercontent.com/9030565/52930683-bbc1f780-338c-11e9-94cf-d491bccdf3de.png)

![가상 컴퓨터 만들기2](https://user-images.githubusercontent.com/9030565/52930722-f035b380-338c-11e9-842a-e43985d6a348.png)

![가상 컴퓨터 만들기3](https://user-images.githubusercontent.com/9030565/52930729-f461d100-338c-11e9-8612-c0126b3c44a8.png)

이름: vm1
1세대 선택
이 가상 컴퓨터에 동적 ~ 해제
external network 석택
기존 가상 하드 디스크 사용 선택
	`Base14A-WS12R2`

P@ssw0rd

### 가상 컴퓨터 설정

![vm1 가상 컴퓨터 접속](https://user-images.githubusercontent.com/9030565/52930849-794cea80-338d-11e9-9fbc-51bff7e18e63.png)

![vm1 이름 변경](https://user-images.githubusercontent.com/9030565/52930893-9ed9f400-338d-11e9-8d7f-e1df15d4e304.png)


### vm2 만들기

iso `9600.16384.WINBLUE_RTM.130821-1623_X64FRE_SERVER_EVAL_EN-US-IRM_SSS_X64FREE_EN-US_DV5`

![vm2 위치](https://user-images.githubusercontent.com/9030565/52931100-7a324c00-338e-11e9-9b56-df588e48bf38.png)

![vm2 iso 파일 설정](https://user-images.githubusercontent.com/9030565/52931120-8c13ef00-338e-11e9-9d81-7349ea6afa46.png)

![vm2](https://user-images.githubusercontent.com/9030565/52931129-93d39380-338e-11e9-9c06-d41de7909e9f.png)

![vm2 윈도우 설치](https://user-images.githubusercontent.com/9030565/52931174-c7162280-338e-11e9-97db-60d5feb5063b.png)
- 4번째 선택
- 2번째 custom

암호는 `P@ssw0rd` 로  동일
설치 완료 후 컴퓨터 명 `vm2`로 변경 vm1과 방법 동일

### Router1, 2  만들기
![Router1 위치](https://user-images.githubusercontent.com/9030565/52931206-e6ad4b00-338e-11e9-82a1-28f4d59c60c0.png)

![Router1](https://user-images.githubusercontent.com/9030565/52931220-f462d080-338e-11e9-8ea8-5cf2089c5f20.png)

![Router2](https://user-images.githubusercontent.com/9030565/52931285-296f2300-338f-11e9-8511-e7fc4cd2b2a2.png)

#### 설치 후 이름 변경
```powershell
PS C:\Users\Administrator> Rename-Computer -NewName router1 -Restart

PS C:\Users\Administrator> Rename-Computer -NewName router2 -Restart
```

### Windows 10 Linux 설치
1. Microsoft store 실행
1. linux 검색
1. Ubuntu 18.04 LTS 설치

![윈도우10 리눅스](https://user-images.githubusercontent.com/9030565/52931819-22e1ab00-3391-11e9-923a-ab964228e57b.png)
- UNIX username: adminuser
- UNIX password: 1

#### powershell에서 bash 사용

```powershell
PS C:\Windows\system32> bash
To run a command as administrator (user "root"), use "sudo <command>".
See "man sudo_root" for details.

adminuser@st1:/mnt/c/Windows/System32$
```


### CentOS Download
http://isoredirect.centos.org/centos/7/isos/x86_64/CentOS-7-x86_64-Minimal-1810.iso

#### 가상 머신 만들기

![centos1 설치 경로](https://user-images.githubusercontent.com/9030565/52932152-380b0980-3392-11e9-88c2-0b304efc8af0.png)

![centos1 iso 위치](https://user-images.githubusercontent.com/9030565/52932195-55d86e80-3392-11e9-88ca-6e106ab89abb.png)

![centos1](https://user-images.githubusercontent.com/9030565/52932204-5d981300-3392-11e9-87d8-25199fc2e6e5.png)

#### centos1 설치

![centos1 설치하기1](https://user-images.githubusercontent.com/9030565/52932286-9637ec80-3392-11e9-8573-45e68fef71ef.png)
- `Install CentOS 7` 엔터 


![centos1  Installation destination](https://user-images.githubusercontent.com/9030565/52932463-1fe7ba00-3393-11e9-87ff-8b27f371c896.png)
- INSTALLATION DESTINATION 클릭하고 `Done` 누르면 자동으로 빨간색이 없어짐

![centos1  network & hostname](https://user-images.githubusercontent.com/9030565/52932477-2bd37c00-3393-11e9-8b70-29325d490dfc.png)
- NETWORK & HOSTNAME 
	- Ethernet **ON**
	- Host name: centos1

![centos1 USER CREATION](https://user-images.githubusercontent.com/9030565/52932557-70f7ae00-3393-11e9-883c-4d770718050f.png)
- `ROOT PASSWORD` : 1
- `USER CREATION`: **adminuser** 생성


#### centos1 

![centos1  root 로그인](https://user-images.githubusercontent.com/9030565/52932803-58d45e80-3394-11e9-85c3-01588bf542ad.png)
- centos1의 아이피 확인

![window10 ubuntu에서 ssh로 centos1 연결](https://user-images.githubusercontent.com/9030565/52932850-77d2f080-3394-11e9-9bf0-0f7fef65c67a.png)
- ssh로 centos1에 접근
- `$ yum update -y`로 패키지 업데이트


---

> BSD UNIX는 오픈소스
> BSD UNIX는 수정 재배포가 가능하면 수정한 코드를 공개하지 않아도 된다.
> apple mac PC는 BSD 라이센스 기반이기 때문에 UNIX를 사용하면서 소스 코드를 공개하지 않는다.

####  Day01 Hyper-V 관리자 
![day01 Hyper-V 관리자](https://user-images.githubusercontent.com/9030565/52936737-70194900-33a0-11e9-811d-a22bdbc595d8.png)


### Docker

#### docker 설치
```bash
[root@centos1 ~]# curl -sSL http://get.docker.com | sh

[root@centos1 ~]# yum install epel-release -y
[root@centos1 ~]# yum install net-tools wget -y
```

#### selinux 수정
1. selinux 수정
1. 재부팅

```bash
[root@centos1 ~]# vi /etc/sysconfig/selinux
```
```bash
#SELINUX=enforcing
SELINUX=disabled
```
- `disabled`로 변경
- shift + z + z (:wq와 동일)

```bash
[root@centos1 ~]# reboot
```

#### docker 설정 & 이미지 다운로드
```bash
[root@centos1 ~]# systemctl start docker
[root@centos1 ~]# systemctl enable docker
Created symlink from /etc/systemd/system/multi-user.target.wants/docker.service to /usr/lib/systemd/system/docker.service.

[root@centos1 ~]# docker pull ubuntu:16.04
[root@centos1 ~]# docker pull ubuntu:18.04
[root@centos1 ~]# docker pull centos
[root@centos1 ~]# docker pull nginx
[root@centos1 ~]# docker pull httpd
[root@centos1 ~]# docker pull mysql
[root@centos1 ~]# docker pull alpine
```

#### docker 명령어
```bash
[root@centos1 ~]# docker run -it ubuntu:16.04 bash
[root@centos1 ~]# docker run -it centos bash
[root@centos1 ~]# docker run -it alpine sh
```

---

```bash
[root@centos1 ~]# docker run -d -p 80:80 nginx
[root@centos1 ~]# docker run -d -p 8080:80 httpd
```
- `-d`: 백 그라운드 실행

http://192.168.1.95

---

```bash
[root@centos1 ~]# docker search pengbai
[root@centos1 ~]# docker pull pengbai/docker-supermario
[root@centos1 ~]# docker run -d -p 8888:8080 pengbai/docker-supermario
```

<!--stackedit_data:
eyJoaXN0b3J5IjpbMzUzNjc0ODg4LDU2ODE5OTc3MywtNjc0Mj
g2MTk4LC0xNjg3NjM0NTk2LC0xMjkxNzY5OTgyLC0yMTQxODgx
NDAzLC02MzY1MTcyNzgsMjAyNzIwNjIzOCwtMTI4Mzk5MjEyMi
wtMTIwOTM2Njg3NiwtNTk2NzM4NTE3LDE4OTM3MjY0NjgsMjEw
MDI2MDExNywtNTk3OTIyNTE1LDExMjQyOTk3NTMsLTQzMTMzNj
UzMywtMTIxMzc5MTI0MywyMTA4MzE5NTU0LC04MjQ5MTU0MjIs
MTc2OTIwNzIxNV19
-->

# Day02

## QA
1. g클라우드 시스템의 스위치는 몇 레벨?
2. 부하분산은 물리적으로? 논리적으로?

- Windows: NetBEUI
- Apple: apple talk

각 제조사마다 다른 프로토콜을 사용했기 때문에 이기종에 대한 통신 시 문제 발생
그래서 TCP/IP 표준 생성

## NETWORK

### 통신방식에 따른 네트워크 분류
1. 유니캐스트 (Unicast)
1. 브로드캐스트 (Broadcast)
1. 멀티캐스트 (Multicast)


### 네트워크의 주소 체계

1.  MAC address : 물리적 주소 
	- 00-19-D1-F0-09-FF
	- 00:19:D1:F0:09:FF
	- 0019.D1F0.09FF
1. IP address : 논리적 주소 
	- 192.168.21.1 

#### 2계층 MAC 관련 명령어

```powershell
PS C:\Windows\system32> arp -a
PS C:\Windows\system32> arp -d
```
- `-a`: 전체 정보 보기
- `-d`: 캐시 지우기

#### ARP
Address Resolution Protocol

IP 주소로 MAC 주소 알아오는 것

#### ping
3계층 통신 (IP 주소)
3/2/1(출발지) -> 1/2/3(목적지)

1. `> ping 192.168.1.21` (3계층)
2. 3/2/1 계층 순으로 전달
3. 목적지 192.168.1.21 수신
4. 1/2/3 계층으로 전달
	1. 2계층에서 192.168.1.21의 MAC 주소 알아 냄
5.  192.168.1.21 수신자는 잘 받았다는 정보를 다신 회신(자신의 MAC 주소를 포함해서)
6. ping 보낸 처음 요청자는 이제 192.168.1.21의 MAC 주소를 알 수 있다.
	1. IP 주소에 해당하는 MAC 주소를 캐싱하고 `> arp -a`로 조회 시 이제 MAC 주소를 알 수 있다.


#### MAC 주소 변경으로 해킹
파일 공유 서버를 예로 설명
- 파일 공유 서버 MAC `00:19:D1:F0:09:FF`
- 해커 가짜 서버 MAC `00:60:97:8F:4F:86`

1. 해커는 전체 네트워크를 돌면서 mac 주소 수집
2. 각 개인 PC의 mac 주소를 변경
	1. 실제 파일 공유 서버 대신 `00:60:97:8F:4F:86`로 변경
3. 사용자는 파일을 업로드 하면 실제 `00:19:D1:F0:09:FF` 대신 `00:60:97:8F:4F:86`로 파일이 업로드 됨

> 사내 네트워크 관리자는 각 IP 주소에 대응하는 MAC 주소를 다 알고 있어야하고 이것이 임의로 변경되는 지 항상 확인해야한다.
>
> 스위치는 모든 네트워크의 MAC 주소를 알기 때문에 스위치를 이용해서 MAC 주소를 관리한다. 

#### 3계층 IP 주소 캐시 확인/삭제

```cmd
C:\Windows\system32> ipconfig/displaydns

C:\Windows\system32> ipconfig/flushdns
```
- `displaydns`: 캐시 확인
- `flushdns`: 캐시 지우기

#### IP 주소
IP = 네트워크 주소 + 호스트 주소

네트워크 주소가 같으면 라우터를 거치지 않는 같은 네트워크

> 서브넷 마스크: 2555.255.255.0
> 11111111.11111111.11111111.00000000
> - 192.168.1.21
> - 192.168.1.19
> xxx.xxx.xxx.21 3번째 까지가 네트워크 주소부

## OSI 7 Layer

### Hub & Switch
둘 다 모두 통신 간 패킷을 송-수신, 증폭하는 기능을 갖고 있다.

그러나, Switch가 Hub에 비해 추가적으로 제공하는 기능이 많다.

통신 방법
- Hub: 반이중(half-duplex)
- Switch: 전이중(full-duplex)

---

제어
- Hub: 개별적 포트 제어 불가
- Switch: 개별적 포트 제어 가능

---

VLAN
- Hub: 불가
- Switch: 가능

구글 드로잉

---

인증
- Hub: 인증 기능 X
- Switch: 인증 기능 O(스위치와 인증서버)

---

**무선은 무조건 Switch, 무선은 MAC 주소를 사용하기 때문에 Hub로 불가**

#### Hub
멀티포트(Multiport), 리피터(Repeater)

### OSI 7 Layer 모델과 TCP/IP 모델 

- 1계층: Hub
- 2계층: Switch
- 3계층: Router

#### L3 Switch
라우터 기능이 포함된 스위치
	- 라우터 기능을 하는 스위치
	- 일반적으로 라우터를 거치면 속도가 느려지기 때문에 속도 증가를 위해 라우터 기능이 있는 스위치를 사용
	- 라우터 기능을 S/W로 처리
![vlan-p260](https://user-images.githubusercontent.com/9030565/53076533-3462b880-3533-11e9-963d-72ae9fc3aae0.png)

- VLAN:  L3 Switch (p260)
	- 한 네트워크에서 논리적으로 네트워크를 분리하기 위한 방법
	- 물리적으로 스위치를 개별로 둬서 나누는 게 좋으나, 비용적인 문제로 하나의 스위치를 논리적으로 나누는 방법
	- 분리된 
- VPN: L2 Switch
	- 논리적으로 내부 네트워크에 가상망을 통해서 접속

#### L4 Switch
부하 분산 기능이 포함된 스위치

#### L7 Switch

> www.naver.com/img
> www.naver.com/video
> 위와 같이 표면적으로 같은 서버 www.naver.com이지만, 실제로 `/img`, `/video`를 다른 서버로 연계해줄 수 있다.
> reverse-proxy

#### Router 와 Firewall
라우터의 기능
- packet filtering
- VPN
- proxy

라우터의 기능 중 packet filtering 기능만 특화 시켜 만든 장비가 `firewall`

### 7계층 : Application Layer
- package: 원본, 코드
- program: HDD에 저장된 실행 가능한 객체
- process: 메모리 위에 올라가서 실행 중인 프로그램
	- service(demon): 자동으로 실행
	- desktop app: 수동으로 실행


### 실습

vm1, vm2 통신

vm1: 192.168.1.57
vm2 : 192.168.1.71
Router1: 192.168.1.77
Router2: 192.168.1.83
centos1: 192.168.1.95

#### 방화벽 
**vm1, vm2 간 ping 통신은 방화벽을 해제하지 않으면 불가**

1. 방화벽 설정 해제
2. 공유 폴더 생성
3. ICMP 규칙 추가

```powershell
> firewall.cpl
```
- 방화벽 설정 확인
	- 만약 방화벽이 켜져 있으면 private, public 모두 `사용 안함`으로 변경	
```powershell
> Test-NetConnection -ComputerName 192.168.1.71 -Port 22
```

**Router1 설정**
만약, 방화벽 설정을 수정하지 않고 ping 통신을 가능하게 하려면 아무 폴더나 공유하면 된다.
폴더 공유를 하게 되면 ICMP 패킷은 허용해서 ping 통신이 가능하다.

**Router2 설정**
```powershell
> wf.msc
```

1. Inbound Rules
2. New Rule
	1. Rule Type: Custom
	2. Program: All programs
	3. Protocol and Ports: Any
	4. Scope: Any IP address
	5. Action: Allow the connection
	6. Profile: all check

![방화벽 룰 추가-ruleType](https://user-images.githubusercontent.com/9030565/52993573-08bed000-3458-11e9-838e-8c0bdde99b99.png)

![방화벽 룰 추가-program](https://user-images.githubusercontent.com/9030565/52993586-12483800-3458-11e9-9e77-879284d0e472.png)

![방화벽 룰 추가-protocolAndPorts](https://user-images.githubusercontent.com/9030565/52993610-255b0800-3458-11e9-852f-108619385dd4.png)

![방화벽 룰 추가-action](https://user-images.githubusercontent.com/9030565/52993631-3146ca00-3458-11e9-8753-635b5b0da6ac.png)



#### telnet client 켜기
1. WIN + R
2. appwiz.cpl
3. windows 기능 켜기/끄기
4. telnet client

```powershell
> telnet 192.168.1.21 445
```
- 원격지의 445번 포트가 열려있나 확인

#### 다른 네트워크 간 연결 
1. vm2, Router2의 네트워크 어댑터를 `Private Network` 로 변경
2. vm2에서 vm1으로 ping 시도

![네트워크 변경 - private](https://user-images.githubusercontent.com/9030565/52993848-f8f3bb80-3458-11e9-98fc-aea58c62b865.png)
- Private Network로 변경
	- 기존엔 External Network

![vm2 네트워크 변경 후 ping 불가](https://user-images.githubusercontent.com/9030565/52993900-1fb1f200-3459-11e9-9d79-5cddb27e3a01.png)
- 같은 네트워크일 때
	- `Reply from 192.168.1.57: bytes=32 time=3ms TTL=128`
- 다른 네트워크일 때
	- `Reply from 192.168.1.71: Destination host unreachable.`

#### VLAN
모든 스위치는 기본적으로 설정하지 않으면 VLAN:1을 갖고 있다.

vm1, vm2의 설정을 `VLAN:10`으로 수정

설정 - 네트워크 어댑터
- VLAN 설정 
- ID: 10

![vlan 설정](https://user-images.githubusercontent.com/9030565/52994362-a6b39a00-345a-11e9-8f1d-0096aeabfc08.png)


![vlan이 다르면 연결 불가](https://user-images.githubusercontent.com/9030565/52994542-4709be80-345b-11e9-98ad-5babf1cbf811.png)
- vm1, vm2는 VLAN: 10
- centos1은 VLAN: 1(설정하지 않으면 기본 1)
- VLAN이 다르기 때문에 연결 불가

#### 네트워크 주소 변경
vm2의 네트워크 주소 수동으로 변경

192.168.1.71 -> 192.168.2.71
- 255.255.255.0 서브넷 마스크를 갖으면 3번째 옥택까지 네트워크 주소이다.
	- 192.168.1.x 와 192.168.2.x는 다른 네트워크이기 때문에 통신 불가(라우터 필요)

---

```powershell
> ncpa.cpl
```
- TCP/IP v4 수동 변경

![vm2  네트워크 주소 변경](https://user-images.githubusercontent.com/9030565/52995860-30fdfd00-345f-11e9-99cb-11af78fb2fef.png)
- vm2에서 vm1 ping 불가
	- 다른 네트워크이기 때문에

## TCP/IP protocol
[TFTP](https://ko.wikipedia.org/wiki/TFTP)
	- 라우터에 주로 사용

https://myip.is


## Ethernet 
PPT - 2장 이더넷, 계층별 장비, Cable

### 이더넷 (Ethernet) 
- LAN 구간에서 사용되는 네트워킹 방식 중 하나 
	- 2계층

### CSMA/CD 
Ethernet의 가장 큰 특징은 CSMA/CD 방식으로 통신한다는 것
- Carrier Sense Multiple Access/Collision Detection
- 충돌이 발생하는 영역을 Collision Domain이라고 한다. 

>  full duplex로 동작하는 링크는 Frame의 송신과 수신이 서로 다른 채널을 통해 이루어지기 때문에 충돌이 일어나지 않는다.
>  **때문에 충돌 감지도 하지 않는다.**
>  
>   즉, full duplex 모드에서 Ethernet 동작 방식이 CSMA/CD가 아니다.

## 계층 별 사용 장비

### Physical Layer (물리계층)
1. 리피터
	- cable 전송으로 약화된 신호를 초기화, 증폭, 재전송의 기능을 수행 
2. 허브
	- 리피터와 마찬가지로 전기적 신호를 증폭
	- 100Mbps 허브에 20대의 PC를 연결하면 실제 속도는 각 PC가 5Mbps의 속도를 사용하는 것
	- 허브는 한 장비에서 전송된 데이터 프레임을 허브에 연결된 모든 장비에 전송 (flooding)
	- 충돌이 많이 발생하여 하나의 허브에 많은 장비를 연결할 수 없다.
	- 허브에 연결된 장비들은 하나의 Collision domain 안에 있다.

### Data link Layer (데이터 링크 계층)
MAC address를 사용하는 계층
1. LLC (Logical Link Control)
2. MAC (Media Access Control)

MAC 주소 테이블 관리

---

1. 브리지
	- 허브와는 달리 Layer 2 주소인 MAC 주소를 보고 Frame 전송 포트를 결정
	- 자체적으로 주변 네트워크에 대한 MAC 주소를 가진 테이블을 관리하고 이것을 기반으로 네트워킹한다.
2. 스위치
	- 스위치는 한 포트에서 전송되는 Frame이 MAC address table에 있는 특정 포트로만 전송하기 때문에 다른 포트가 전송하는 Frame과 충돌이 발생하지 않는다.


### Network Layer (네트워크 계층)
IP 주소(논리적인 주소)를 사용하는 계층
1. Router

IP 주소 테이블 관리


# Day03
## 실습
### 라우터
1. 두 개 이상 필요 (Router1, Router2)
  - 정지된 상태에서 네트워크 어댑터 추가
2. 
#### 가상 스위치 유형
- Private(개인)
  - 개인은 내부 네트워크끼리만 가능 밖으로 못 나감
- Internal(내부)
  - Hyper-v 호스트(윈도우)와 네트워크)
- External(외부)
  - 외부까지 가능, 공인망
### Hyper-V 설정
![p307](https://user-images.githubusercontent.com/9030565/53076336-cae2aa00-3532-11e9-963d-c913d175a9e4.png)
1. Router1, Router2 네트워크 어댑터 추가
2. Private Network 이름 변경
  - Private Network-1
  - Router1의 스위치
3. Private Network-2 추가
  - Router2의 스위치
4. vm1
  - Private Network-1
5. vm2 
  - Private Network-2
![Router1 private 어댑터 추가](https://user-images.githubusercontent.com/9030565/53057725-dd89be80-34f3-11e9-8e06-3871d6e16475.png)
- Router1에 Private Network 추가
![Router2 private 어댑터 추가](https://user-images.githubusercontent.com/9030565/53057746-f1cdbb80-34f3-11e9-97a3-5ef4feadef46.png)
- Router2에 Private Network 추가
![private network 이름 변경](https://user-images.githubusercontent.com/9030565/53057826-3ce7ce80-34f4-11e9-8506-e3af6a00f60a.png)
- Private Network 이름 변경
  - `Private Network-1`
![Private network-2 생성](https://user-images.githubusercontent.com/9030565/53057858-4a9d5400-34f4-11e9-8ae3-9261fd316be6.png)
- Private Network-2 생성
![vm2 - private network2에 연결](https://user-images.githubusercontent.com/9030565/53057994-c0092480-34f4-11e9-87b4-6d5071b93618.png)
- Router2에 `Private Network-2` 연결
- 이 상태에서 vm1는 외부 통신 불가
  - Router1 와는 가능, Router1에 Private 있기 때문에
### 가상 머신 설정
```powershell
> ncpa.cpl
```
#### Router1
ethernet2
- IP address: 10.10.1.1
- Subnet mask: 255.255.255.0
![Router1 설정](https://user-images.githubusercontent.com/9030565/53058266-bd5aff00-34f5-11e9-8879-39256929aa11.png)
#### Router2
ethernet2
- IP address: 10.10.2.1
- Subnet mask: 255.255.255.0
#### vm1
- IP address: 10.10.1.2
- Subnet mask: 255.255.255.0
- Default gateway: 10.10.1.1
- Preferred DNS server: 8.8.8.8
#### vm2
- IP address: 10.10.2.2
- Subnet mask: 255.255.255.0
- Default gateway: 10.10.2.1
- Preferred DNS server: 8.8.8.8
![vm2 설정](https://user-images.githubusercontent.com/9030565/53058342-00b56d80-34f6-11e9-8dd2-25df206047a4.png)
---
> Router1과 Router2에 공유 폴더를 설정하면 각 연결된 vm1,vm2에서  Router1,Router2로 ping이 나간다.
> 하지만, vm1, vm2에서 ping이 외부로 나갈 순 없다.(NAT를 사용하면 가능)
> 
> Router1
> - 공유 폴더 설정
> - 네트워크 어댑터 2개
>   - External
>   - Private Network-1
> 
> vm1
> - 네트워크 어댑터 Private Network-1로 설정
> 
>  ---
> 
> Router2
> - 공유 폴더 설정
> - 네트워크 어댑터 2개
>   - External
>   - Private Network-2
> 
> vm2
> - 네트워크 어댑터 Private Network-2로 설정
#### 라우터 NAT 설정
[Manage] - [Add Roles and Features]
1. Server Roles
  -  Remote Access
2. Remote Access
  -  Role Services
    -  **Routing 클릭**
나머지는 Next 클릭
![NAT -Manage - Add Roles and Features Router1 라우터 설정](https://user-images.githubusercontent.com/9030565/53061022-d49eea00-34ff-11e9-90fe-5eba61ff03db.png)
![NAT -Manage - Add Roles and Features Router1 라우터 Routing 클릭 ](https://user-images.githubusercontent.com/9030565/53061096-129c0e00-3500-11e9-8850-e7aaa2625ba8.png)
[Tools] - [Routing and Remote Access]
1. ROUTER1(local) 오른쪽 클릭
2. Configure and Enable Routing and Remote Access
3. **NAT** 선택
나머지는 Next 클릭
![NAT - Tools - Routing and Remote Access](https://user-images.githubusercontent.com/9030565/53061234-8ccc9280-3500-11e9-99c5-87841333fa56.png)
![NAT - Routing and Remote Access 설정하기](https://user-images.githubusercontent.com/9030565/53061288-abcb2480-3500-11e9-9b0d-e756590ded87.png)
![NAT - Routing and Remote Access NAT 선택](https://user-images.githubusercontent.com/9030565/53061314-b84f7d00-3500-11e9-9869-283e49a72a1c.png)
![NAT - Routing and Remote Access Ethernet 선택](https://user-images.githubusercontent.com/9030565/53061332-c6050280-3500-11e9-8dc1-2fc9a01e2e0c.png)
![NAT - Routing and Remote Access Finish 전](https://user-images.githubusercontent.com/9030565/53061371-da48ff80-3500-11e9-9379-2bd0c664a8ab.png)
![NAT - Routing and Remote Access 설치 후 ](https://user-images.githubusercontent.com/9030565/53061416-f351b080-3500-11e9-804e-f49d65c7c99c.png)
![NAT - ping 외부 확인](https://user-images.githubusercontent.com/9030565/53061523-40358700-3501-11e9-9741-58f4f00edf11.png)
![NAT 테이블 확인](https://user-images.githubusercontent.com/9030565/53062543-8213fc80-3504-11e9-81a1-f0db9f13c5a3.png)
#### ping 8.8.8.8
1. vm1에서 ping 8.8.8.8
  - 출발지: 10.10.1.2
  - 목적지: 8.8.8.8
2. Router1에 도착시
  - NAT에 의해 출발지 `192.168.1.77`(External Network)로 변경
#### vm2에서 vm1로 ping
현재 vm2에서 NAT 설정으로 8.8.8.8 공인망 연결은 가능한 상태
vm2 -> vm1 불가능 이유
> 10.10.2.1의 네트워크에서 10.10.1.x는 다른 네트워크 이기 때문에 Router2로 간다.
>
> Router2는 External 외부망과 연결되어 있기 때문에 공인 IP(webtime)로 가지만 webtime에선 10.10.1.x(사설)에 대한 정보는 없기 때문에 불가
> 
> 8.8.8.8은 알려진 외부 네트워크기 때문에 webtime이 정보를 알아서 통신 가능했다.
>
> NAT는 기본적으로 목적지도 NAT이면 불가(단방향)
>  - 사설 IP -> 공인 IP (가능)
>  - 공인 IP -> 사설 IP (불가)
> 
> **해결책**
> - 목적지를 최종 사설 IP로 하지 않고 목적지의 라우터로 한다.
>   - Router2가 10.10.1.x가 목적지인 데이터가 오면 default route가 아닌 Router1로 가게 설정해준다.
> - 목적지 라우터까지 왔다고 하더라도 NAT는 단방향이기 때문에 10.10.1.x로 들어갈 수 없다.
>   - 따라서 10.10.1.x의 Router1에서 port-forwarding을 해서 10.10.1.2까지 갈 수 있게 설정한다.
**Router1 NAT 설정**
IPv4 - NAT - Properties - Services and Ports
1. vm1, vm2 폴더 공유 설정
  - 공유 확인 명령어
  ```powershell
  > net share
  ```
2. ping 10.10.1.1 불가  
  - 목적지도 NAT기 때문에
3. Router1 445 port forwarding
  - vm2에서 Router1 `\\192.168.1.77`로 접근하면 vm1의 공유 폴더 확인 가능
  - 445 공유 폴더 포트를 포워딩했기 때문에
![Router1 port forwading](https://user-images.githubusercontent.com/9030565/53063776-b5588a80-3508-11e9-8098-ec743fb05172.png)
![vm2에서 Router1의 smb로 붙어서 vm1의 공유된 폴더 접근](https://user-images.githubusercontent.com/9030565/53064249-7aefed00-350a-11e9-814d-980235251a0c.png)
### 원격 접속
#### Router1 예제
![dashboard - Local Server - Remote Desktop - Enabled](https://user-images.githubusercontent.com/9030565/53067048-da9fc580-3515-11e9-9ecd-7dbc6b5e3eb2.png)
```powershell
> mstsc
```
![Router1에 원격 접속](https://user-images.githubusercontent.com/9030565/53067149-51d55980-3516-11e9-9929-d8479c4bb2dd.png)
`3389` remote terminal
#### vm2 원격 접속
1. vm2 원격 접속 허용
2. Router2 라우터 NAT 설정에서 3389 Remote Desktop 허용
3. mstsc 
  - 192.168.1.83 (Router2)
  - `administrator`
![vm2 원격 접속 허용](https://user-images.githubusercontent.com/9030565/53067270-d627dc80-3516-11e9-970c-7347c6c2b728.png)
![Router2 - Routing and Remote Access - Remote Descktop 추가](https://user-images.githubusercontent.com/9030565/53067358-3880dd00-3517-11e9-80fe-c2c7b8c4f466.png)
![원격 접속 로그인 Router2의 IP해야함 포트 포워딩](https://user-images.githubusercontent.com/9030565/53067445-8e558500-3517-11e9-9718-cd15b26b90b5.png)
![원격 접속 확인](https://user-images.githubusercontent.com/9030565/53067455-97465680-3517-11e9-88a3-11c9bd343a48.png)
![원격 접속 완료](https://user-images.githubusercontent.com/9030565/53067484-acbb8080-3517-11e9-9b0b-3a1f72f539b5.png)
#### 포트 포워딩 약점
한 라우터에 다수의 서버가 연결된 경우 vm1에 포트 포워딩을 80으로 했다면, vm2는 할 수 없다.
즉, 같은 포트에 대해서 불가하다.(라우터에 여러 공인 IP가 붙어야 가능하다)
#### 라우터의 기능
- **라우팅**
- 패킷 필터링
  - Firewall
- VPN
  - 장비
- NAT
  - Reverse proxy
#### 방화벽 & Proxy
- Firewall
  - 외부에서 내부 차단(원래 방화벽)
  - 요즘 방화벽은 Inbound, Outbound 모두 차단 가능
- Proxy Server
  - 내부에서 외부 차단
  - Fast Access(캐시)
  - 특정 사이트 접근 차단
- Reverse Proxy
  - 외부에서 내부로 접속하게 도와줌
  - NAT와 유사
    - NAT는 포트를 포워딩
    - RP는 포트&경로를 포워딩
### 접속 방법
- Console
  - 노트북과 라우터를 케이블로 연결해서 콘솔로 작업
  - 초기 작업
- Network
---
operation
  - 작업, 수술, 운영, 작전, 연산
console
  - consul(로마집정관), 명령을 내리는 사람
### 공인 IP & 사설 IP
- 공인 IP: 인터넷에 연결할 수 있는 공식 IP
- 사설 IP: 내부 네트워크에서만 사용가능한 공식 IP
  - A 10.x.x.x
  - B 172.16.x.x ~ 172.31.x.x
  - C 192.168.x.x
## 계층 별 사용 장비
pdf 2장
### Network Media
#### 장비 별 UTP Cabling
- 이기종 : Straight-through cable
  - Switch와 Router 
  - Switch와 PC 
  - Switch와 Server 
  - Hub와 PC 
  - Hub와 Server
- 동일기종: Crossover cable 
  - Switch와 Switch 
  - Switch와 Hub 
  - Hub와 Hub 
  - Router와 Router 
  - PC와 PC 
  - Router와 PC
### 라우트 경로 실습
**라우트 경로 삭제/생성**
```powershell
PS C:\Windows\system32> route print
PS C:\Windows\system32> route delete 0.0.0.0
PS C:\Windows\system32> route add 0.0.0.0 mask 0.0.0.0 192.168.1.1
```
- route delete 0.0.0.0
  - 모든 통신 막힘
### Router1에서 vm1 route table 지우기
#### 원격 접속
```powershell
PS C:\Users\Administrator> Enter-PSSession -ComputerName 10.10.1.2
Enter-PSSession : Connecting to remote server 10.10.1.2 failed with the following error message : The WinRM client
cannot process the request. If the authentication scheme is different from Kerberos, or if the client computer is not
joined to a domain, then HTTPS transport must be used or the destination machine must be added to the TrustedHosts
configuration setting. Use winrm.cmd to configure TrustedHosts. Note that computers in the TrustedHosts list might not
be authenticated. You can get more information about that by running the following command: winrm help config. For
more information, see the about_Remote_Troubleshooting Help topic.
At line:1 char:1
+ Enter-PSSession -ComputerName 10.10.1.2
+ ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo          : InvalidArgument: (10.10.1.2:String) [Enter-PSSession], PSRemotingTransportException
    + FullyQualifiedErrorId : CreateRemoteRunspaceFailed
PS C:\Users\Administrator> Set-Item WSMan:\localhost\Client\TrustedHosts -Value *
WinRM Security Configuration.
This command modifies the TrustedHosts list for the WinRM client. The computers in the TrustedHosts list might not be
authenticated. The client might send credential information to these computers. Are you sure that you want to modify
this list?
[Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"): Y
PS C:\Users\Administrator> Enter-PSSession -ComputerName 10.10.1.2
```
- Set-Item: 원격 접속을 신뢰한다.
  - `-Value`: 컴퓨터 이름. `*`은 모든 컴퓨터
- Enter-PSSession: 원격 접속
#### 원격 접속으로 vm1의 route table 제거 
현재 vm1에 원격 접속한 상태
```powelshell
[vm1]: PS C:\> route delete 0.0.0.0
 OK!
[vm1]: PS C:\> route add 0.0.0.0 mask 0.0.0.0 10.10.1.1
 OK!
[vm1]: PS C:\>
```
#### Router1로서 원격으로 vm1 컨트롤하기
현재 Router1에 접속한 상태
```powershell
PS C:\Users\Administrator> Invoke-Command -ComputerName vm1 {ping 8.8.8.8}
```
- Invoke-Command: 현재 터미널 상태에서 원격으로 명령어 실행
  > **Router1에서 명령어 실행**
  > PS C:\Users\Administrator> Invoke-Command - ComputerName vm1 {route delete 0.0.0.0}
  > ```
  >
  > **Router1에서 vm에 접속 후 명령어 실행**
  > ```powershell
  > PS C:\Users\Administrator> Enter-PSSession -ComputerName 10.10.1.2
  > [vm1]: PS C:\> route delete 0.0.0.0
  >
## IP addressing

# Day04
## Day03 복습
### Subnet mask
#### Class A
5.1.1.0/8
  - 호스트 2^24 - 2 
### Router
- Routing & Switching
- Packet filtering
  - Firewall
- VPN
- NAT
  - Reverse proxy
- DHCP: IP Address 자동 할당
- Load Balancer: 부하 분산
### Windows Firewall 설정
- `firewall.cpl`
  - Inbound 설정만 가능, 단순한 기능
- `wf.msc`
  - Inbound, Outbound 모두 설정, 다양한 기능
### 용어 정리
- Host
  - 주인
- Guest
---
- Physical
- Virtual
---
- Proxy
- Redirection
- Forward
## 실습
### vm1, vm2 통신
Day03의 NAT, 포트 포워딩의 문제점을 해결한 방법을 실습
현재 포트 포워딩으로 445(SMB) 포트를 연결하여 폴더 공유는 가능하나 다른 포트에 대한 vm1 -> vm2는 불가능
1. Static Route 추가
  - Router1, Router2 모두 라우팅 경로가 필요하다
    - 통신의 기본은 **주고 받는** 것이기 때문에
2. NAT 제거
  - NAT는 사설 주소를 공인 주소로 변환해서 보내기 때문에 이것이 설정되어 있으면 제대로 통신이 안된다.
#### 현재 구성도
![p307](https://user-images.githubusercontent.com/9030565/53076336-cae2aa00-3532-11e9-963d-c913d175a9e4.png)
#### 이더넷 이름 변경
![이더넷 이름 변경](https://user-images.githubusercontent.com/9030565/53138426-97069380-35c9-11e9-89f3-a744e3c13249.png)
- Router1, Router2
  - Ethernet -> Public
  - Etherner2 -> Private
- vm1, vm2
  - Etherner -> Private
####  Static Route
![Router1의 Static Route](https://user-images.githubusercontent.com/9030565/53139100-e352d300-35cb-11e9-9161-c5f02d485b2a.png)
- Router1의 Static Route
  - Destination: `10.10.2.0`
    - Router2의 네트워크 대역을 적는다.
    - `10.10.2.2` host 주소를 적으면 해당 호스트만 통신 가능하기 때문에 network 주소를 적는다.
  - Network mask: `255.255.255.0`
  - Gateway: `192.168.1.83`
    - Router2의 공인망 게이트웨이 주소를 적는다
![Router2의 Static Route](https://user-images.githubusercontent.com/9030565/53139166-201eca00-35cc-11e9-97f6-d452a1a2e82b.png)
- Router2의 Static Route
  - Destination: `10.10.1.0`
  - Network mask: `255.255.255.0`
  - Gateway: `192.168.1.77`
####  NAT 제거
NAT 설정이 있으면 안된다.
![NAT 제거](https://user-images.githubusercontent.com/9030565/53139239-5eb48480-35cc-11e9-93e5-c337f5a643ad.png)
> 하지만, NAT 설정을 지우면 공인망 8.8.8.8과 통신이 안된다.(vm1 -> 8.8.8.8)
> 
> 현재 NAT가 지워진 경우 (vm1-> Router1 -> webtime -> internet)
> 1. vm1 -> Router1
>   - 출발지 `10.10.1.2`
>   - 목적지 `8.8.8.8`
> 2. Router1 -> webtime
>   - 출발지 `10.10.1.2`(NAT가 있었다면, 192.168.1.77로 변환)
>   - 목적지 `8.8.8.8`
> 3. webtime -> internet
>   - 출발지 `122.220.31.218`(webtime의 NAT가 자동으로 변환, 사설 IP는 그대로 나갈 수 없기 때문에 default)
>   - 목적지 `8.8.8.8`
>
> **vm1 -> 8.8.8.8 까지는 도달할 수 있으나, 통신은 `송수신`이기 때문에 응답이 vm1까지 다시와야 한다.**
>
> 4. internet -> webtime
>   - 출발지 `8.8.8.8`
>   - 목적지 `122.220.31.218`(webtime -> internet에서 나갈 때 NAT 정보가 없어 default로 나갔기 때문에 다시 들어갈 주소를 모른다)
> 
> **webtime까지는 왔으나 더 이상 갈 곳이 없기 때문에 패킷을 버린다.**
![p383](https://user-images.githubusercontent.com/9030565/53149938-8d455600-35f2-11e9-903f-a953ad224557.png)
### 자동 Route
#### Static Route 제거
Router1, Router2 
위 과정에서 생성한 Static Route 모두 제거
Router1, Router2
1. Routing and Remote Access
3. IPv4 - General - New Routing Protocol 클릭
  - RIP 선택
4. RIP - New Interface
  - Public 선택
  - **Activate authentication** 선택
    - password: 1111
#### New Routing Protocol  - RIP
![New Routing Protocol](https://user-images.githubusercontent.com/9030565/53140806-3c713580-35d1-11e9-8cbf-6e487ff7e6ba.png)
#### New Interface - Public
![RIP Public으로 수정 1](https://user-images.githubusercontent.com/9030565/53140812-42ffad00-35d1-11e9-8cef-e507af37b391.png)
- NAT가 삭제된 상태
![RIP Public으로 수정 2](https://user-images.githubusercontent.com/9030565/53141208-a0e0c480-35d2-11e9-8978-536b6de53e84.png)
- **Activate authentication** 선택
  - 현재 실습 환경에선 같은 10.10.1.x를 사설 네트워크로 다 같이 사용하고 있고 Router1의 External 네트워크는 192.168.1.x를 공유하기 때문에 5명 수강생이 모두 같은 네트워크에 있는 것처럼 통신되기 때문에 불가
    - 10.10.1.x, 10.10.11.x, 10.10.12.x 으로 각 수강생의 사설 네트워크를 변경하든지
    - Activate authentication을 선택해서 같은 비밀번호는 사용하는 네트워크끼리만 통신하게 변경한다.
    - 이 때 수강생마다 비밀번호는 모두 다르게 설정한다.
---
결과적으로 Static, Auto Route 모두 vm1 -> 8.8.8.8은 불가
공인망과 통신하려면 NAT가 필요하다.
## Centos 실습
nginx를 사용하여 Load Balancer와  Reverse proxy 구성하기
### 사전 준비
#### centos1 복사
1. 정지 centos1 복사 후 centos2 centos3 이름 변경
2. 가상 컴퓨터 생성
  3. C\VMs에 복사된 파일 사용
3. hostname 수정
![centos 파일 복사](https://user-images.githubusercontent.com/9030565/53141528-d1752e00-35d3-11e9-938e-364d59f573f0.png)
- centos1 192.168.1.95
- centos2 192.168.1.29
- centos3 192.168.1.31
### mobaxterm 터미널 접속
https://mobaxterm.mobatek.net
centos1, centos2, centos3에 모두 접속
####  방화벽 해제 
![방화벽 확인](https://user-images.githubusercontent.com/9030565/53146694-887ba480-35e8-11e9-9f2b-c95d6cf3290e.png)
```bash
$ systemctl status firewalld
$ systemctl stop firewalld
$ systemctl disable firewalld
```
- `stop`: 방화벽 정지
- `disable`: 재시작 시 방화벽 안 올라오게 설정
### Load Balancer 구성하기 
#### centos1 아파치 설치
```bash
[root@centos1 ~]$ yum install httpd -y
[root@centos1 ~]$ systemctl status httpd
[root@centos1 ~]$ systemctl start httpd
[root@centos1 ~]$ systemctl enable httpd
[root@centos1 ~]$ cd /var/www/html && echo "VM1" > index.html
```
- `enable`: 재시작 시 방화벽 자동으로 올라오게 설정
#### centos2 아파치 설치
centos1의 설정과 동일 `echo "VM2" > index.html`
#### centos3 nginx 설치
```bash
[root@centos3 ~]$ yum install nginx -y
[root@centos3 ~]$ systemctl status nginx
[root@centos3 ~]$ systemctl start nginx
[root@centos3 ~]$ systemctl enable nginx
```
#### centos3 nginx로 LB 구성하기 
**centos3**
**vi /etc/nginx/nginx.conf**
```bash
# For more information on configuration, see:
#   * Official English Documentation: http://nginx.org/en/docs/
#   * Official Russian Documentation: http://nginx.org/ru/docs/
user nginx;
worker_processes auto;
error_log /var/log/nginx/error.log;
pid /run/nginx.pid;
# Load dynamic modules. See /usr/share/nginx/README.dynamic.
include /usr/share/nginx/modules/*.conf;
events {
    worker_connections 1024;
}
http {
    upstream www.cloudshell.local {
        server vm1.cloudshell.local;
        server vm2.cloudshell.local;
    }
    server {
        location / {
            proxy_apss http://www.cloudshell.local;
        }
    }
}
```
- `http {` 아래 모든 내용을 지우고 위와 같이 수정한다.
---
**nginx 문법 검사**
```bash
[root@centos3 nginx]$ nginx -t
nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
nginx: configuration file /etc/nginx/nginx.conf test is successful
```
---
**nginx 설정 다시 로드**
```bash
[root@centos3 nginx]$ systemctl reload nginx
```
---
**vi /etc/hosts**
```bash
127.0.0.1   localhost localhost.localdomain localhost4 localhost4.localdomain4
::1         localhost localhost.localdomain localhost6 localhost6.localdomain6
192.168.1.95    vm1.cloudshell.local #centos1
192.168.1.29    vm2.cloudshell.local #centos2
192.168.1.31    www.cloudshell.local #centos3
```
- 설정후 ping으로 제대로 통신되는지 확인
  - `$ ping vm1.cloudshell.local`
--- 
**/ect/hosts scp 이용해 centos1, centos2로 보내기**
```bash
[root@centos3 nginx]$ scp /etc/hosts vm1.cloudshell.local:/etc/hosts
[root@centos3 nginx]$ scp /etc/hosts vm2.cloudshell.local:/etc/hosts
```
- `$ systemctl reload nginx`
---
![부하 분산확인 centos3에서](https://user-images.githubusercontent.com/9030565/53148846-51f55800-35ef-11e9-95b8-425a9203e016.png)
![부하 분산확인 centos1에서](https://user-images.githubusercontent.com/9030565/53149081-05f6e300-35f0-11e9-83ac-a45d811fc9f8.png)
### Reverse Proxy 서버 구성
#### centos3 nginx 삭제
```bash
[root@centos3 ~]$ yum remove nginx -y
```
---
```bash
[root@centos3 ~]$ systemctl is-active firewalld
inactive
```
- 방화벽 확인
  - 만약, `active`면 아래 명령어로 중지
    - `$ systemctl  stop firewalld`
    - `$ systemctl disable firewalld`
#### centos3 nginx 재설치
```bash
[root@centos3 ~]$ yum install nginx -y
[root@centos3 ~]$ systemctl start nginx
[root@centos3 ~]$ systemctl enable nginx
```
#### centos3 nginx.conf 수정
**vi /etc/nginx/nginx.conf**
```bash
# For more information on configuration, see:
#   * Official English Documentation: http://nginx.org/en/docs/
#   * Official Russian Documentation: http://nginx.org/ru/docs/
user nginx;
worker_processes auto;
error_log /var/log/nginx/error.log;
pid /run/nginx.pid;
# Load dynamic modules. See /usr/share/nginx/README.dynamic.
include /usr/share/nginx/modules/*.conf;
events {
    worker_connections 1024;
}
http {
    log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                      '$status $body_bytes_sent "$http_referer" '
                      '"$http_user_agent" "$http_x_forwarded_for"';
    access_log  /var/log/nginx/access.log  main;
    sendfile            on;
    tcp_nopush          on;
    tcp_nodelay         on;
    keepalive_timeout   65;
    types_hash_max_size 2048;
    include             /etc/nginx/mime.types;
    default_type        application/octet-stream;
    # Load modular configuration files from the /etc/nginx/conf.d directory.
    # See http://nginx.org/en/docs/ngx_core_module.html#include
    # for more information.
    include /etc/nginx/conf.d/*.conf;
}
```
#### centos3 /conf.d/ 추가
**vi /etc/nginx/conf.d/www.cloudshell.local.conf**
```bash
server {
    listen 80;
    server_name www.cloudshell.local;
    location / {
        proxy_pass http://vm1.cloudshell.local:80;
    }
}
```
---
**powershell 만들기**
```bash
[root@centos3 /etc/nginx/conf.d]$ cp www.cloudshell.local.conf www.powershell.local.conf
```
---
**vi /etc/nginx/conf.d/www.powershell.local.conf**
```bash
server {
  listen 80;
    server_name www.powershell.local;
    location / {
        proxy_pass http://vm2.cloudshell.local:80;
    }
}
```
####  vi /etc/hosts 수정
```bash
127.0.0.1   localhost localhost.localdomain localhost4 localhost4.localdomain4
::1         localhost localhost.localdomain localhost6 localhost6.localdomain6
192.168.1.95    vm1.cloudshell.local #centos1
192.168.1.29    vm2.cloudshell.local #centos2
192.168.1.31    www.cloudshell.local #centos3
192.168.1.31    www.powershell.local #centos3
```
#### /etc/hosts 파일 복사
```bash
[root@centos3 /etc/nginx/conf.d]$ scp /etc/hosts vm1.cloudshell.local:/etc/hosts
[root@centos3 /etc/nginx/conf.d]$ scp /etc/hosts vm2.cloudshell.local:/etc/hosts
```
#### cloudshell.local, powershell.local 확인
```bash
[root@centos3 /etc/nginx/conf.d]$ systemctl reload nginx
```
- http://www.cloudshell.local
  - vm1
- http://www.powershell.local
  - vm2
## Kubernetes
minikube
- https://kubernetes.io/ko/docs/tasks/tools/install-minikube/
- https://kubernetes.io/ko/docs/setup/minikube
- https://medium.com/@JockDaRock/minikube-on-windows-10-with-hyper-v-6ef0f4dc158c
docker에서 부족한 점: orchestration
Kubernetes: 컨테이너 관리에 특화된 솔류션


# Day05
## Router
### Router의 역할
- Router는 Layer 3 (Network 계층) 장비이다.
- 서로 다른 Network를 연결하고 Broadcast Domain을 나눈다.
- 라우터는 특정 인터페이스를 통하여 수신한 packet의 목적지 IP 주소를 보고 목적지와 연결된 인터페이스를 통하여 전송.
이를 Routing이라고 한다.
- 경로결정 : packet이 목적지로 갈 수 있는 경로를 확인하고 어느 경로가 가장 최적경로(Best path)인지 결정
- 스위칭 : 결정된 경로대로 packet을 전송해주는 것
- Routing protocol에 따라 Routing table을 작성한다.
### Router에 사용되는 Cable
1. V.35  WAN 구간(Serial Interface)에 사용되는 케이블 중 하나
2. UTP  LAN 구간(Ethernet Interface)에 사용되는 케이
### Router
- Router에서 사용되는 Software를 IOS(Internetworking Operating System)라고 한다
### Router 접속 방법
1. Console cable
  - console cable이 가장 일반적이고 편리한 방법. Router에 PC를 직접 연결해야 하고 console cable이 필요하다는 불편함이 있다.  telnet을 주로 사용
2. Telnet
  - Router의 IP 주소를 알고 네트워크에 접속만 되어 있다면 장소와 상관없이 접속이 가능하다. 
## Routing Protocol
Routing : Packet을 수신했을때 Best Path(최적 경로)를 찾아서 어느 경로로 전송할지 결정하는 것
### Routing Protocol
- 목적지 네트워크로 가는 경로를 알아내기 위해 사용하는 Protocol.
- Router는 기본적으로 자신과 연결된 네트워크의 정보만을 Routing table에 가지고 있다. 
### Routing Protocol의 종류
1. Static Routing Protocol
  - 관리자가 직접적으로 목적지 네트워크의 정보를 입력하는 프로토콜
2. Dynamic Routing Protocol
  - Router와 Router가 자동으로 서로의 네트워크 정보를 주고 받으며 네트워크 정보를 업데이트하는 프로토콜
## Switch
- Switch는 Layer 2 Switch와 Multi Layer Switch로 구분된다.
- Router와 차이점
  1. Router는 CPU-base (policy-base) Switch는ASIC(ApplicationSpecific Integrated Circuit)칩 기반이다.
  2. Router는 Routing table, ARP-table을 확인 (IP address) Switch는 MAC address table을 확인 (MAC address)
  3. Router는 자신이 모르는 목적지를 가진 packet과 Broadcast를 Drop. Switch는 자신이 모르는 목적지를 가진 frame과 Broadcast를 Flooding.
### Transparent Bridging
- Ethernet Switch가 Frame을 수신하여 목적지로 전송하는 방식과 절차를 정의.
### Spanning-Tree Protocol
단일 경로로 구성할 경우 경로에 이상이 발생하면 통신이 이뤄지지 않는다. 
이러한 문제들을 해결하기 위해 Spanning-Tree Protocol이 enable되어 있다.
>  다중화로 구성된 스위치에서 Looping 발생을 방지하기 위해 하나의 경로를 제외하고 나머지 경로들을 차단했다가 사용되던 경로에 이상이 발생했을 경우 차단됐던 경로를 사용하는 알고리즘.
## VLAN
- 연결된 장비가 많아질 수록 Broadcast의 발생이 많아지기 때문에 Router를 사용해 물리적으로 Network 영역을 구분.
  - 하지만 Router를 사용해 물리적으로 Network 영역을 구분하는 대신 VLAN 기술을 사용하면 논리적으로 Network (즉, Broadcast Domain)를 나눌 수 있다.
- 하나의 Switch에 연결된 장비들의 Network(Broadcast Domain)를 나눌 수 있다.
- VLAN 설정을 하기 전에 모든 포트들은 default VLAN인 VLAN 1에 속해 있다.
### VLAN의 장점
- VLAN을 사용하면 Network의 보안성 강화된다.
  - 장비들을 서로 다른 VLAN으로 구분했을 경우 Router를 통해야만 통신이 이뤄지기 때문에 Router에 다양한 보안 정책을 적용해서 보안성을 강화시킬 수 있다.
- VLAN을 사용하면 Switch Network에서 Load balancing이 가능.
### VLAN의 번호
- VLAN은 서로 번호(ID)로 구분. 
- 사용 가능한 VLAN 번호는 1 ~ 4094
### VLAN Port 종류
1. Access Port
  - 하나의 Port가 하나의 VLAN에 속하는 경우.
2. Trunk Port
  - 하나의 Port에 여러 개의 VLAN Frame이 흘러다닐 수 있도록 하는 경우.
  - Switch와 Switch가 하나의 port로 연결돼 있는 경우 해당 port에 Switch에 설정된 여러 개의 VLAN Frame이 모두 흘러다녀야 두 대의 Switch에 설정된 각 VLAN별로 통신이 가능 .
  - 즉, 다수의 같은 VLAN이 여러개의 Switch에 존재할 경우 trunk port를 통해 각 Switch에 연결된 같은 VLAN에 속한 장비끼리 서로 통신이 가능.
  - 이러한 통신은 같은 VLAN에 연결된 장비들 사이에만 통신만 가능하다. 서로 다른 VLAN에 속한 장비는 L3장비(ex. Router)를 통해야만 통신이 가능하다.
## VTP - VLAN Trunking Protocol
- Switch간 VLAN정보를 서로 교환하여 Switch들이 가지고 있는 VLAN 정보를 동기화 시켜주기 위한 Protocol.
- VTP를 설정하지 않으면 각각의 Switch들마다 VLAN을 구성하고 만약 VLAN에 변경이 생겼을 경우 각 Switch마다 하나씩 VLAN의 변경 내용.

