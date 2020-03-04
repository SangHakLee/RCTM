
이용준(linuzle@gmail.com)

한/영:win + space

### 공인 IP 설정

```bash
$ ifconfig
$ nmtui
```

![dhcp-ip](https://user-images.githubusercontent.com/9030565/75204762-22a29980-57b5-11ea-81e6-aa0f1e6d668c.png)

Automatic: get public IP from DHCP

```bash
$ systemctl restart network
```
---



# 클라우드 환경의 이해
## 서버 가상화

### 컨테이너
> Guest OS 불필요, 환경을 격리


Docker: Cgroup, namespace, SElinux

- Cgroup: 자원의 제한
- namespace: 자원의 격리
- SELinux: 자원의 보안

## 리눅스
### 배포판
- RedHat
	- RHEL: 상업용 배포판(유료)
	- Fedora: 무료 배포판
	- CentOS: RHEL과 코드는 동일. 레드햇의 지원을 받을 순 없다.
- SuSE

[Red Hat Linux 와 CentOS](https://www.lesstif.com/pages/viewpage.action?pageId=20775405)

# 리눅스 파일 다루기
- Ctrl + Alt + F6: 터미널 접속 (6번째 터미널)
- Ctrl + F(n): 새 터미널 (터미널 접속 후)
- Ctrl + F1: GUI 터미널

#### 프롬프트
|| bash| c |
|--|--|--
|관리자|#|#|
|일반|$|%|

#### 프롬프트 변경하기
```bash
$ PS1='hak linux $ '
```

## 명령어

#### hostnamectl
CentOS 7 이상
```bash
$ hostnamectl

   Static hostname: station14.example.com
         Icon name: computer-desktop
           Chassis: desktop
        Machine ID: c0a3bfb337684ae8911711c0aa6b6a49
           Boot ID: 48097d2e920c494da5958c06056fa3ae
  Operating System: CentOS Linux 7 (Core)
       CPE OS Name: cpe:/o:centos:centos:7
            Kernel: Linux 3.10.0-514.26.1.el7.x86_64
      Architecture: x86-64
```
### root

1. 로그인
2. bash
3. /etc/profile
4. ~/.bash_profile
5. ~/.bashrc
6. /etc/bashrc


#### su
> launch a new shell as **another user**
>
> 다른 계정으로 로그인

```bash
$ su guru
```
- `guru` 계정으로 전환

```bash
$ su -
$ su -l
$ su --login
```
- `-, -l, --login` 해당 계정의 환경 설정을 가져감

#### sudo 
> run a single command with **another user's privilege**
>
> 다른 계정의 권한만 빌려서 명령어 수행

`su` 명령어로 root 계정으로 전환하는 것은 모든 일반 사용자들이 root의 패스워드를 알아야 한다는 단점이 있다.


```bash
# root
$ visudo
# /etc/sudoers
guru    ALL=(ALL)       !ALL,/usr/sbin/useradd # useradd 명령어만 부여

# ---

# guru
$ sudo useradd aaa
[sudo] password for guru:

$ sudo userdel aaa
Sorry, user guru is not allowed to execute '/sbin/userdel aaa' as root on st여ation14.example.com.
```
`유저명(%그룹명)`   `호스트명=(실행할유저)`   `명령어`

### man

- `[]` 생략 가능
- `|` 선택 사항
- `{}` 필수 입력

```bash
$ man passwd
$ man 5 passwd # 섹션 지정
$ man -k passwd # 섹션 확인
```
온라인 메뉴얼은 `name`으로 검색되는 모든 메뉴얼을 검색해서 보여준다.
passwd 검색 시 섹션을 지정하지 않으면 요구와 다른 메뉴얼이 검색될 수 있다.

```bash
$ whereis passwd
passwd: /usr/bin/passwd /etc/passwd /usr/share/man/man1/passwd.1.gz /usr/share/man/man5/passwd.5.gz
```

## 파일시스템

### 파일시스템 구조
- Data block
	- 정보를 저장
//code.jquery.com/jquery-3.2.1.min.js


### ls -l
```bash
$ ll -d /etc
drwxr-xr-x. 139 root root 8192 Feb 24 11:58 /etc
```
- 139: 서브 디렉토리 링크 수
	- 모든 디렉토리는 기본적으로 2개의 서브 디렉토리 링크를 갖고 하위에 서브 디렉토리가 추가될 때마가 1씩 증가한다.
	- 139 - 2 = 137, 즉 `/etc`는 137개의 서브 디렉토리를 갖고 있다.

### 퍼미션
|  | 파일 | 디렉토리 |
|--|--|--|
| r |  |  |
| w |  | **중요** |
| x |  |  |

디렉토리의 `w` 권한

```bash
-rw-r--r--. 1 guru wheel 0 Feb 24 15:15 3.txt
```
> 위 `3.txt` 파일을 삭제할 수 있을까?
> 현재 정보만으론 알 수 없다.
> 파일의 삭제 가능 여부는 해당 파일이 속해 있는 디렉토리의 권한에 종속적이기 때문에 디렉토리의 권한을 알아야 한다.
> 
> 만약, `/files/3.txt`이면 `files` 디렉토리에 `w` 권한이 있는 경우 `3.txt`를 삭제할 수 있다.

### 링크
#### hard link
하드 링크는 제약 사항이 많다.

```bash
$ ln /etc b.link
ln: ‘/etc’: hard link not allowed for directory
```

#### symbolic link
```bash
ln -s /etc b.link
```
**원본의 경로는 절대 경로를 사용한다.**

### find

find 검색위치 -검색옵션
- exec: 검색 후 2차 작업의 명령
	- exec 명령어 {} \;

```bash
$ find /etc -name "*.conf"
```

```bash
$ find . -name "*.cfg" -exec rm -rf {} \;
```

## 추가 구성 - VM
### 사전 준비
1. /etc/hosts
	- yum 레포 서버 설정
2. yum repolist
	- yum 확인
3. yum install
	- yum으로 가상 환경 개발을 위한 패키지 설치

```bash
$ vi /etc/hosts
59.29.224.177 server1.example.com server1
비

$ yum repolist
Loaded plugins: fastestmirror, langpacks
Loading mirror speeds from cached hostfile
repo id                              repo name                                                     status
base                                 CentOS 7 - x86_64 - base                                      3,831
classRPMs                            Custom Guru Labs Classroom RPMs                                 196
errataRPMs                           CentOS 7 - Server - x86_64 - Errata                             291
repolist: 4,318

$ yum -y install qemu-kvm libvirt virt-viewer virt-manager libvirt-daemon-kvm
...
Complete!
```

### 파티셔닝
1. lsblk
	- 디스크 확인
2. fdisk
	- 디스크 파티셔닝
3. partprobe
	- lsblk로 추가된 파티션이 나오지 않을 때, 다시 확인 시켜주기 위함
4. lsblk -f
5. mkfs.xfs /dev/sda3
6. /etc/fstabs
7. mount -a
	- fstabs 내용으로 자동 마운트
8. df
	- 마운트된 내역 확인
9. reboot
	-  
```bash
$ lsblk
$ fdisk /dev/sda
	p

	n
	  p
	  3
	  (enter)
	  +100G
	w

$ partprobe /dev/sda
$ lsblk -f

$ mkfs.xfs /dev/sda3

$ vi /etc/fstabs
/dev/sda3               /var/lib/libvirt/images xfs     defaults        0 2

$ mount -a

$ df

$ reboot
```

**fdisk 파티션 과정**
```
Command (m for help): p

Disk /dev/sda: 500.1 GB, 500107862016 bytes, 976773168 sectors
Units = sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 4096 bytes
I/O size (minimum/optimal): 4096 bytes / 4096 bytes
Disk label type: dos
Disk identifier: 0x0009ddf6

   Device Boot      Start         End      Blocks   Id  System
/dev/sda1   *        2048     1026047      512000   83  Linux
/dev/sda2         1026048    72706047    35840000   8e  Linux LVM

Command (m for help): n
Partition type:
   p   primary (2 primary, 0 extended, 2 free)
   e   extended
Select (default p): p
Partition number (3,4, default 3): 3
First sector (72706048-976773167, default 72706048): 
Using default value 72706048
Last sector, +sectors or +size{K,M,G} (72706048-976773167, default 976773167): +100G
Partition 3 of type Linux and of size 100 GiB is set

Command (m for help): p

Disk /dev/sda: 500.1 GB, 500107862016 bytes, 976773168 sectors
Units = sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 4096 bytes
I/O size (minimum/optimal): 4096 bytes / 4096 bytes
Disk label type: dos
Disk identifier: 0x0009ddf6

   Device Boot      Start         End      Blocks   Id  System
/dev/sda1   *        2048     1026047      512000   83  Linux
/dev/sda2         1026048    72706047    35840000   8e  Linux LVM
/dev/sda3        72706048   282421247   104857600   83  Linux

Command (m for help): w
The partition table has been altered!

Calling ioctl() to re-read partition table.

WARNING: Re-reading the partition table failed with error 16: Device or resource busy.
The kernel still uses the old table. The new table will be used at
the next reboot or after you run partprobe(8) or kpartx(8)
Syncing disks.
```
- p: 확인
- n: 생성

### virt-manager

```bash
$ lsmod | grep kvm

$ virt-manager
```

File - New Virtual Machine

http://59.29.224.177/CENTOS7

KDUMP: 시스템의 오류 시 덤프 파일로 오류에 대한 리포팅, 우린 `disabled` 체크 해제

![create-vm-1](https://user-images.githubusercontent.com/9030565/75204766-26362080-57b5-11ea-8a23-91dac29bbea9.png)
![create-vm-2](https://user-images.githubusercontent.com/9030565/75204767-27ffe400-57b5-11ea-8183-d51c159cfe6f.png)
![create-vm-3](https://user-images.githubusercontent.com/9030565/75204775-2e8e5b80-57b5-11ea-9b2e-7b86a6338f69.png)
![create-vm-4](https://user-images.githubusercontent.com/9030565/75204779-30581f00-57b5-11ea-8af8-52e5a06a7db0.png)
![create-vm-5](https://user-images.githubusercontent.com/9030565/75204783-3221e280-57b5-11ea-85e5-c8308d458356.png)
![create-vm-6](https://user-images.githubusercontent.com/9030565/75204784-33eba600-57b5-11ea-88c0-bae7c757418a.png)

## 명령어

### find
```bash
$ find /etc -name "*.conf"

$ find /var -user lp

$ find /var -group chrony

$ find /home -nouser

$ find /boot -size 5M # 정확하게 5MB 검색
$ find /boot -size +5M # 5MB 이상 검색

$ find /boot -mtime 1 # 1: 24 시간, -1: 24 시간 이내
$ find /boot -mtime +365 # 1년 동안 사용하지 않는 파일들 찾을 때

```
- `-name`: 파일명으로 검색
- `-user`: 소유자로 검색
- `-group`: 그룹으로 검색
- `-nouser`:  계정 정보가 없는 디렉토리 검색. userdel 옵션 없이 사용 시 정보는 /home 에 남기 때문에
- `-size`: `+ / -`  없이 사용하면 정확하게 수치에 맞는 값으로 검색
- `-mtime`: 
	- `-atime`: 접근 시간
	- `-ctime`: 파일 수정 시간
	-  `-mtime`: 파일 내용 수정 시간
	- https://www.unixtutorial.org/atime-ctime-mtime-in-unix-filesystems


#### 퍼미션 실습
```bash
$ for num in 1 2 3 4 5 6 7; do
> touch $num.txt
> chmod ${num}00 $num.txt
> done

$ find . -perm 300 # 300 정확히 검색

$ find . -perm -300 # 300 퍼미션을 갖고만 있다면 검색, wx를 갖기만 하면
우
$ find . -perm +300
find: warning: you are using `-perm +MODE'.  The interpretation of `-perm +omode' ch면anged in findutils-4.5.11.  The syntax `-perm +omode' was removed in findutils-4.5.12, in favour of `-perm /omode'.
./3.txt
$ find . -perm /300 # +는 deprecated, wx 중 하나라도 갖고 있는 경우경
```

### 리다이렉션
```bash
$ tr a-z A-Z < /etc/fstab # /etc/fstab 내용의 소문자를 대문자로 변경
```

### 파이프
```bash
$ cat /etc/passwd | grep bash | cut -d: -f1,3,6- | sort
```

#### grep
```bash
$ grep nologin /etc/passwd

$ grep ano /etc/passwd
$ grep ano /etc/passwd -i

$ grep nologin /etc/passwd -v

$ grep /etc/passwd -e asa -e ssh -e root
```
- `-i`: 대소문자 구분 X
- `-v`: not
- `-e`: 여러개 검색

## 환경 변수
- 변수
	- 로컬 변수
	- 환경 변수

변수 사용
> $ echo ${변수명}
> 
> $ env {명령어}
>
> $ set {명령어}

로컬 변수 설정, 수정
> $ 변수명=값

환경 변수 설정, 수정
> $ export 변수명=값값

https://zetawiki.com/wiki/리눅스_환경변수,_지역변수

```bash
$ age=10
$ color=red
```

### $PATH
상대 경로로 실행되는 명령어의 경로를 관리

```bash
$ echo $PATH
리
$ skipcpio
bash: skipcpio: command not found...

$ /usr/lib/dracut/skipcpio
Usage: /usr/lib/dracut/skipcpio <file>

$ PATH=$PATH:/usr/lib/dracut

$ skipcpio
Usage: /usr/lib/dracut/skipcpio <file>
```

### Nesting 명령어
```bash
$ echo today is `date`

$ echo today is $(date)
```

## bash 설정
```bash
# ~/.bash_profile
PATH=$PATH:$HOME/bin:/usr/lib/dracut
age=20
color=red

export PATH age

/usr/bin/cal
```

```bash
# ~/.bashrc
set -o ignoreeof

umask 077
```

```bash 
$ source ~/.bash_profile
$ . ~/.bashrc
```
- 설정 적용


# 리눅스 프로세스 관리하기

## 프로세스

### ps
```bash
$ ps
  PID TTY          TIME CMD
 4486 pts/0    00:00:00 bash
 6799 pts/0    00:00:00 ps

$ ps -f
UID        PID  PPID  C STIME TTY          TIME CMD
root      4486  4456  0 09:30 pts/0    00:00:00 bash
root      6779  4486  0 11:43 pts/0    00:00:00 ps -f

$ ps --forest
  PID TTY          TIME CMD
 4486 pts/0    00:00:00 bash
 6791 pts/0    00:00:00  \_ ps

$ ps -ef # e: 시스템 전체의 프로세스 확인 

$ pstree # 트리 구조 출력
```

### top
```bash
$ top
```
- shift + m :  메모리 우선 순위
- shift + p :  CPU 우선 순위

## 프로세스 Lifecycle

fork: 현재 프로세스가  자식 프로세스 생성
exec: 현재 프로세스를 덮어 씌워 프로세스 생성

### echo $?
오류 발생 시 0 이외의 양의 정수 
```bash
$ cal # no error
$ echo $? # 0

$ data # error
$ echo $? # 127

$ ls /et # error
$ echo $? # 2
```

## 시그널

단축키: 포그라운드 프로세스
명령어: 백그라운드 프로세스

### kill
```bash
$ kill -l
 1) SIGHUP	 2) SIGINT	 3) SIGQUIT	 4) SIGILL	 5) SIGTRAP
 6) SIGABRT	 7) SIGBUS	 8) SIGFPE	 9) SIGKILL	10) SIGUSR1
11) SIGSEGV	12) SIGUSR2	13) SIGPIPE	14) SIGALRM	15) SIGTERM
16) SIGSTKFLT	17) SIGCHLD	18) SIGCONT	19) SIGSTOP	20) SIGTSTP
21) SIGTTIN	22) SIGTTOU	23) SIGURG	24) SIGXCPU	25) SIGXFSZ
26) SIGVTALRM	27) SIGPROF	28) SIGWINCH	29) SIGIO	30) SIGPWR
31) SIGSYS	34) SIGRTMIN	35) SIGRTMIN+1	36) SIGRTMIN+2	37) SIGRTMIN+3
38) SIGRTMIN+4	39) SIGRTMIN+5	40) SIGRTMIN+6	41) SIGRTMIN+7	42) SIGRTMIN+8
43) SIGRTMIN+9	44) SIGRTMIN+10	45) SIGRTMIN+11	46) SIGRTMIN+12	47) SIGRTMIN+13
48) SIGRTMIN+14	49) SIGRTMIN+15	50) SIGRTMAX-14	51) SIGRTMAX-13	52) SIGRTMAX-12
53) SIGRTMAX-11	54) SIGRTMAX-10	55) SIGRTMAX-9	56) SIGRTMAX-8	57) SIGRTMAX-7
58) SIGRTMAX-6	59) SIGRTMAX-5	60) SIGRTMAX-4	61) SIGRTMAX-3	62) SIGRTMAX-2
63) SIGRTMAX-1	64) SIGRTMAX

$ kill # 시그널 없으묜 15번 SIGTERM 디폴트
```
- `ctrl + c`: SIGINT 2번
- `ctrl + d`: SIGTERM 15번
- `ctrl + z`: SIGTSTP 20번


```bash
$ sleep 3000 &
$ ps
  PID TTY          TIME CMD
 4486 pts/0    00:00:00 bash
 7448 pts/0    00:00:00 sleep
 7460 pts/0    00:00:00 ps

$ fg
sleep 3000
^Z
[2]+  Stopped                 sleep 3000

$ bg %1
$ jobs # running

$ kill -20 7448
[2]+  Stopped                 sleep 3000

$ kill -18 7448

$ kill 7448
[2]+  Terminated              sleep 3000

$ ps
  PID TTY          TIME CMD
 4486 pts/0    00:00:00 bash
 7565 pts/0    00:00:00 ps
```

#### killall
> 프로세스 이름으로 접근
```bash
$ sleep 200

$ ps

$ killall sleep
```

#### pkill
```bash
$ sleep 100

$ ps

$ pkill sleep
```

### nohup

``` bash
# user
$ nohup sleep 2020 & # pid: 7746  ppid: 5501
```

## 우선 순위
### nice
19 ~ -20: 속도 낮음 ~ 속도 높음

0: default
19 ~ 0: 일반 사용자

#### renice
```bash
$ renice -n -4 {PID}
```
프로세스의 priory 값을 재조정, -4 한다.

## 스케줄링
### at
```bash
$ at -t 202002251439
at> /usr/bin/df > /tmp/at.result # Ctrl + d

$ atq
2	Tue Feb 25 14:39:00 2020 a root
```

### crontab
```bash
$ crontab -e # 설정

$ crontab -r # 삭제
```

### anacron
```
$ cat /etc/anacrontab
```

# 리눅스 파일 편집하기

## vi
```
$ vi ~/.vimrc
: set all
```
- `set all`: 설정 리스트

`vimrc`에  설정 값을 넣어주면 vi의 기본 설정을 셋팅할 수 있다.

# 스토리지 구성 및 관리

Storage
- [DAS](https://ko.wikipedia.org/wiki/직접_연결_저장장치)
- [NAS](https://ko.wikipedia.org/wiki/네트워크_결합_스토리지)
	- cifs
	- nfs
- [SAN](https://ko.wikipedia.org/wiki/스토리지_에어리어_네트워크)
	- FC-SAN
	- IP-SAN
		- [iSCSI](https://ko.wikipedia.org/wiki/ISCSI) 
		- FCoE

> 네트워크를 통해 전송되는 단위가
> file이면 NAS
> disk면 SAN

요즘엔 파티셔닝 안함. LVM

## DAS

- `IDE`: /dev/hd`{알파벳}{숫자}`
- `SATA, SAS, SCSI`: /dev/sd`{알파벳}{숫자}`
- `virtual`: /dev/vd`{알파벳}{숫자}`

알파벳: VM이 디스크를 인식한 순서
숫자: 파티션 번호
> 숫자가 없다면 아직 파티셔닝이 안된 것


#### fdisk
고전 파티셔닝 시스템, 2TB 까지 지원 가능

#### parted
`fdisk`의 단점을 보완한 시스템
하지만, 즉시 적용이라는 특성 때문에 실수가 많이 생김

#### gdisk
2TB 이상 가능, 인터페이스가 `fdisk` 와 동일


### 실습
1. Virtual Machine Manager
	- SCSI 스토리지 3개 추가
	![create-storage](https://user-images.githubusercontent.com/9030565/75298355-9d32ee00-5875-11ea-81ec-456d4ecfd49c.png)
1. $ lsblk
	- 1G 디스크 3개 확인
	```bash
	$ lsblk
	NAME        MAJ:MIN RM SIZE RO TYPE MOUNTPOINT
	sda           8:0    0   1G  0 disk 
	sdb           8:16   0   1G  0 disk 
	sdc           8:32   0   1G  0 disk 
	vda         252:0    0  30G  0 disk 
	├─vda1      252:1    0   1G  0 part /boot
	└─vda2      252:2    0  29G  0 part 
	  ├─cl-root 253:0    0  26G  0 lvm  /
	  └─cl-swap 253:1    0   3G  0 lvm  [SWAP]\
	```것
1. fdisk 실습

#### fdisk 실습
2단계 이후 작업
```bash
$ fdisk /dev/sda
  p # 현재 파티션 확인
       Device Boot      Start         End      Blocks   Id  System
  n # 파티션 생성
    p # primary
    1 # number
      # enter, default
    +300M # 300MB 할당
  p
       Device Boot      Start         End      Blocks   Id  System
	/dev/sda1            2048      587775      292864   83  Linux
#/dev/sda 에 300M, 200M 파티션 2개 생성

  n
    e # extend
    3
      # enter, default
      # enter, default, 나머지 모두 사용
  p 
     Device Boot      Start         End      Blocks   Id  System
	/dev/sda1            2048      587775      292864   83  Linux
	/dev/sda2          587776      997375      204800   83  Linux
	/dev/sda3          997376     2097151      549888    5  Extended

  n 
    l # logical
      # enter, default
    +250M
  p
     Device Boot      Start         End      Blocks   Id  System
	/dev/sda1            2048      587775      292864   83  Linux
	/dev/sda2          587776      997375      204800   83  Linux
	/dev/sda3          997376     2097151      549888    5  Extended
	/dev/sda5          999424     1511423      256000   83  Linux

  d
    5
  d
    3

  t
    2 # 바꿀 파티션 넘버
    L # 코드 리스트 확인
    82
      Changed type of partition 'Linux' to 'Linux swap / Solaris'
  p
       Device Boot      Start         End      Blocks   Id  System
	/dev/sda1            2048      587775      292864   83  Linux
	/dev/sda2          587776      997375      204800   82  Linux swap / Solaris

  w # 파티션 적용

$ lsblk
NAME        MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
sda           8:0    0    1G  0 disk 
├─sda1        8:1    0  286M  0 part 
└─sda2        8:2    0  200M  0 part 
sdb           8:16   0    1G  0 disk 
sdc           8:32   0    1G  0 disk 
vda         252:0    0   30G  0 disk 
├─vda1      252:1    0    1G  0 part /boot
└─vda2      252:2    0   29G  0 part 
  ├─cl-root 253:0    0   26G  0 lvm  /
  └─cl-swap 253:1    0    3G  0 lvm  [SWAP]
```

#### parted 실습
이론상 무한대에 가까운 primary 파티션 생성 가능

```bash
$ parted /dev/sdb
  p
    Partition Table: unknown # 파티션 정보가 없음, 파티션 생성 불가
    
  mkpart primary 0M 400M
    Error: /dev/sdb: unrecognised disk label
    
  mklabel msdos
  p
    Partition Table: msdos # 이제 파티션 가능
    
  mklabel gpt
    Yes
  p
    Partition Table: gpt
    
  mkpart primary 0M 400M
    Ignore
  p
    Number  Start   End    Size   File system  Name     Flags
	 1      17.4kB  400MB  400MB               primary

  mkpart primary 400M 100%
  p
    Number  Start   End     Size   File system  Name     Flags
	 1      17.4kB  400MB   400MB               primary
	 2      400MB   1074MB  674MB               primary
  
  rm 2
  p
    Number  Start   End    Size   File system  Name     Flags
	 1      17.4kB  400MB  400MB  

  q # 종료
```

#### gdisk 실습
fdisk와 동일

```bash
$ gdisk /dev/sdb
```

 ##### 파티션 종류
 - primary
	 - 개수의 제약이 존재, 최대 4개
		 - MBR(512B)
 - extend
	 - logical을 묶는 용도
	 - 실제 파티션이 아님
 - logical

### 파일 시스템 만들기
위에서 파티셔닝한 디스크를 파일 시스템으로 만든다.

#### 포멧 확인
```bash
$ lsblk -f
NAME        FSTYPE      LABEL UUID                                   MOUNTPOINT
sda                                                                  
├─sda1                                                               
└─sda2                                                               
sdb                                                                  
└─sdb1                                                               
sdc                                                                  
vda                                                                  
├─vda1      xfs               cd7f06ad-533a-4ad9-a942-022975775e39   /boot
└─vda2      LVM2_member       Ykr26g-j3JV-aVah-vGom-KkhG-49z1-rTrvxN 
  ├─cl-root xfs               5a73ef7a-8544-4cbc-8ece-56bf1c6f12ea   /
  └─cl-swap swap              a6bedeea-4774-45fd-b952-ecdb36d65c8e   [SWAP]
```
- LABEL: 디스크 이름 지정. 현재는 잘 사용하지 않음
- UUID: 고유 식별자

#### mkfs
```bash
$ mkfs.xfs /dev/sda1
$ mkfs.ext4 /dev/sdb1

$ lsblk -f
NAME        FSTYPE      LABEL UUID                                   MOUNTPOINT
sda                                                                  
├─sda1      xfs               eb7da13f-0cb3-4733-a3bc-56201934f563   
└─sda2                                                               
sdb                                                                  
└─sdb1      ext4              3312fdd5-664c-4bec-befc-d2f7f9db7ccc   
sdc                                                                  
vda                                                                  
├─vda1      xfs               cd7f06ad-533a-4ad9-a942-022975775e39   /boot
└─vda2      LVM2_member       Ykr26g-j3JV-aVah-vGom-KkhG-49z1-rTrvxN 
  ├─cl-root xfs               5a73ef7a-8544-4cbc-8ece-56bf1c6f12ea   /
  └─cl-swap swap              a6bedeea-4774-45fd-b952-ecdb36d65c8e   [SWAP]
```

### 마운트
명령어로 사용한 마운트는 재시작 시 detach된다.
#### mount
```bash
$ mkdir /red
$ mkdir /blue

$ mount /dev/sda1 /red
$ mount /dev/sdb1 /blue

$ lsblk -f 
NAME        FSTYPE      LABEL UUID                                   MOUNTPOINT
sda                                                                  
├─sda1      xfs               eb7da13f-0cb3-4733-a3bc-56201934f563   /red
└─sda2                                                               
sdb                                                                  
└─sdb1      ext4              3312fdd5-664c-4bec-befc-d2f7f9db7ccc   /blue
sdc                                                                  
vda                                                                  
├─vda1      xfs               cd7f06ad-533a-4ad9-a942-022975775e39   /boot
└─vda2      LVM2_member       Ykr26g-j3JV-aVah-vGom-KkhG-49z1-rTrvxN 
  ├─cl-root xfs               5a73ef7a-8544-4cbc-8ece-56bf1c6f12ea   /
  └─cl-swap swap              a6bedeea-4774-45fd-b952-ecdb36d65c8e   [SWAP]

$ df
Filesystem          1K-blocks    Used Available Use% Mounted on
/dev/mapper/cl-root  27245572 4650660  22594912  18% /
devtmpfs              1534920       0   1534920   0% /dev
tmpfs                 1551700     176   1551524   1% /dev/shm
tmpfs                 1551700    9036   1542664   1% /run
tmpfs                 1551700       0   1551700   0% /sys/fs/cgroup
/dev/vda1             1038336  160252    878084  16% /boot
tmpfs                  310340       4    310336   1% /run/user/42
tmpfs                  310340      24    310316   1% /run/user/0
/dev/sda1              289444   14816    274628   6% /red
/dev/sdb1              370047    2062    344359   1% /blue


# --- 마운트 해제 ---
$ umount /blue # 마운트 제경로 사용
$ umount /dev/sda1 # 장치 이름 사용

$ df
Filesystem          1K-blocks    Used Available Use% Mounted on
/dev/mapper/cl-root  27245572 4650680  22594892  18% /
devtmpfs              1534920       0   1534920   0% /dev
tmpfs                 1551700     176   1551524   1% /dev/shm
tmpfs                 1551700    9036   1542664   1% /run
tmpfs                 1551700       0   1551700   0% /sys/fs/cgroup
/dev/vda1             1038336  160252    878084  16% /boot
tmpfs                  310340       4    310336   1% /run/user/42
tmpfs                  310340      24    310316   1% /run/user/0
```

#### fstab
```bash
$ vi /etc/fstab

/dev/sda1               /red                    xfs     defaults        0 2
UUID=3312fdd5-664c-4bec-befc-d2f7f9db7ccc /blue ext4    defaults        0 2

$ mount -a # 자동 마운트, fstab 내용 확인을 위해서 일단 마운트
$ df # fstab의 마운트 내용이 제대로 있다면 정상적으로 된다는 뜻

$ reboot # 부팅 시 마운트 확인
$ df # /blue, /red 확인
```
| 장치 이름, 라벨, UUID | 마운트 포인트 | 파일 시스템 | 마운트 옵션 |  덤프(restore) 백업 복구 허용 여부 | 파일 시스템 체크 여부 |
|--|--|--|--|--|--|


- 마운트 옵션
	- rw: 읽고 쓰기
	- async: 비동기
	- nouser: 일반 유저 불가
	- auto:
	- exec: 
	- dev: 장치, 특수 파일 저장 사용 가능
	- suid: 특수 퍼미션 허용

#### fuser
파일 시스템을 사용중인 사용자 확인
```bash
$ fuser -cu /blue
```

## 파일 시스템 관리

```bash
$ xfs_info /dev/sda1

$ dumpe2fs /dev/sdc1 | more
Mount count:              3
Maximum mount count:      -1
```

### 라벨 지정
```bash
$ xfs_admin -L red_disk /dev/sda1
fatal error -- couldn't initialize XFS library
$ umount /red
$ xfs_admin -L red_disk /dev/sda1
writing all SBs
new label = "red_disk"

$ umount /blue
$ e2lable /dev/sdb1 blue_disk
$ e2label /dev/sdc1 blue_disk

$ lsblk -f

$ mount LABEL=red_disk /red
```

### 파일 시스템 관리
#### xfs_repair
```bash
$ cp /etc/*.conf /red

$ xfs_repaire /dev/sda1
xfs_repair: /dev/sda1 contains a mounted filesystem
xfs_repair: /dev/sda1 contains a mounted and writable filesystem

fatal error -- couldn't initialize XFS library

$ umount /red
$ xfs_repaire /dev/sda1
...
done
```

#### xfs_copy
```bash
$ xfs_copy /dev/sda1 /tmp/red.bak
Creating file /tmp/red.bak
 0%  ... 10%  ... 20%  ... 30%  ... 40%  ... 50%  ... 60%  ... 70%  ... 80%  ... 90%  ... 100%

All copies completed.

$ ll -h /tmp/red.bak # 파티션이 카피된 것이기 때문에 용량이 큼
-rw-------. 1 root root 286M Feb 25 17:49 /tmp/red.bak
```

#### xfs_freeze
```bash
$ mount -a # 위에서 umount된 모든 디스크 자동 마운트

$ xfs_freeze -f /red

# 새 터미널에서 작업
$ rm -f /red/*
 # 프롬프트 멈춤, freeze 했기 때문에 

# 기존 터미널
$ xfs_freeze -u /red # 새 터미널에서 프롬프트 다시 진행됨
```

### swap
```bash
$ swapon -s
```
#### swap 만들기
```bash
$ lsblk -f
NAME     FSTYPE      LABEL     UUID                                   MOUNTPOINT
sda                                                                   
├─sda1   xfs         red_disk  eb7da13f-0cb3-4733-a3bc-56201934f563   /red
└─sda2                                                                

$ mkswap /dev/sda2
Setting up swapspace version 1, size = 204796 KiB
no label, UUID=68f82a7d-dbdf-4da0-bdac-7b9679714f5d

$ lsblk -f
NAME     FSTYPE      LABEL     UUID                                   MOUNTPOINT
sda                                                                   
├─sda1   xfs         red_disk  eb7da13f-0cb3-4733-a3bc-56201934f563   /red
└─sda2   swap                  68f82a7d-dbdf-4da0-bdac-7b9679714f5d   

$ swapon -s
Filename				Type		Size	Used	Priority
/dev/dm-1                              	partition	3145724	0	-1

$ swapon /dev/sda2 # swap 활성화

$ swapon -s
Filename				Type		Size	Used	Priority
/dev/dm-1                              	partition	3145724	0	-1
/dev/sda2                              	partition	204796	0	-2

$ swapoff /dev/sda2 # 우선 순위 지정을 위해 비활성화

$ vi /etc/fstab # 
	/dev/sda2               swap                    swap    defaults,pri=3  0 0

$ mount -a 
$ swapon -s # mount -a로 swap이 안되는 것을 확인
Filename				Type		Size	Used	Priority
/dev/dm-1                              	partition	3145724	0	-1

$ swapon -a
$ swapon -s
Filename				Type		Size	Used	Priority
/dev/dm-1                              	partition	3145724	0	-1
/dev/sda2                              	partition	204796	0	3

```
- Priority: 양의 정수에 가까울수록 우선 순위가 높음

### LVM

1TB  볼륨 100개를 합쳐 100TB의 볼륨 그룹을 만든다.

- Physical Volumes: 1TB의 물리적인 볼륨
- Volume Groups: 물리 볼륨 여러개를 모아 둔 것
- Logical Volume: 볼륨 그룹을 논리적으로 나눈것
- Extend: 용량,가 관리의 최소 단위, 볼륨 그룹을 만들 때 결정됨(default: 4MB)
	- 7MB를 원하면 extend 2개를 사용하면 됨

#### LVM 실습
1. vm 강제 종료 `Force Off`(VMM)
2. 이전에 찍어둔 스냅샷으로 롤백 (컴퓨터 2개 합친 아이콘)
3. 디스크 3개 추가 (이전과 동일,  Virtual Machine Manager)
4. LVM 실습, bash

```bash
$ pvs # 물리 볼륨 확인, LVM 전이라 추가한 3개 볼륨이 보이지 않음음
  PV         VG Fmt  Attr PSize  PFree
  /dev/vda2  cl lvm2 a--  29.00g    0

$ pvcreate /dev/sda /dev/sdb
  Physical volume "/dev/sda" successfully created.
  Physical volume "/dev/sdb" successfully created.

$ pvs
 PV         VG Fmt  Attr PSize  PFree
  /dev/sda      lvm2 ---   1.00g 1.00g
  /dev/sdb      lvm2 ---   1.00g 1.00g
  /dev/vda2  cl lvm2 a--  29.00g    0 

$ pvdisplay /dev/sda
  "/dev/sda" is a new physical volume of "1.00 GiB"
  --- NEW Physical volume ---
  PV Name               /dev/sda
  VG Name               
  PV Size               1.00 GiB
  Allocatable           NO
  PE Size               0   
  Total PE              0
  Free PE               0
  Allocated PE          0
  PV UUID               fV1zuy-sOLb-0gOA-9B0y-2gzB-kzAQ-MKp9Qb
   
$ vgs # 볼륨 그룹 확인
  VG #PV #LV #SN Attr   VSize  VFree
  cl   1   2   0 wz--n- 29.00g    0

$ vgcreate vgtest /dev/sda /dev/sdb
  Volume group "vgtest" successfully created 

$ vgs
  VG     #PV #LV #SN Attr   VSize  VFree
  cl       1   2   0 wz--n- 29.00g    0 
  vgtest   2   0   0 wz--n-  1.99g 1.99g

$ vgdisplay vgtest
  --- Volume group ---
  VG Name               vgtest
  System ID             
  Format                lvm2
  Metadata Areas        2
  Metadata Sequence No  1
  VG Access             read/write
  VG Status             resizable
  MAX LV                0
  Cur LV                0
  Open LV               0
  Max PV                0
  Cur PV                2
  Act PV                2
  VG Size               1.99 GiB
  PE Size               4.00 MiB
  Total PE              510
  Alloc PE / Size       0 / 0   
  Free  PE / Size       510 / 1.99 GiB
  VG UUID               K2nsVP-0Zcy-AGjY-topt-AnkH-XvQ0-Ywoz1U

$ vgdisplay vgtest -v # 물리 볼륨, 논리 볼륨 모두 확인
  --- Volume group ---
  VG Name               vgtest
  System ID             
  Format                lvm2
  Metadata Areas        2
  Metadata Sequence No  1
  VG Access             read/write
  VG Status             resizable
  MAX LV                0
  Cur LV                0
  Open LV               0
  Max PV                0
  Cur PV                2
  Act PV                2
  VG Size               1.99 GiB
  PE Size               4.00 MiB
  Total PE              510
  Alloc PE / Size       0 / 0   
  Free  PE / Size       510 / 1.99 GiB
  VG UUID               K2nsVP-0Zcy-AGjY-topt-AnkH-XvQ0-Ywoz1U
   
  --- Physical volumes ---
  PV Name               /dev/sda     
  PV UUID               fV1zuy-sOLb-0gOA-9B0y-2gzB-kzAQ-MKp9Qb
  PV Status             allocatable
  Total PE / Free PE    255 / 255
   
  PV Name               /dev/sdb     
  PV UUID               hjIrgd-5MLd-f118-4Mxr-Habl-OYn3-V4FS0X
  PV Status             allocatable
  Total PE / Free PE    255 / 255

$ pvdisplay /dev/sda # 볼륨 그룹에 추가된 것을 확인
  --- Physical volume ---
  PV Name               /dev/sda
  VG Name               vgtest
  PV Size               1.00 GiB / not usable 4.00 MiB
  Allocatable           yes 
  PE Size               4.00 MiB
  Total PE              255
  Free PE               255
  Allocated PE          0
  PV UUID               fV1zuy-sOLb-0gOA-9B0y-2gzB-kzAQ-MKp9Qb

$ vgremove vgtest # 볼륨 그룹 삭제
  Volume group "vgtest" successfully removed

$ vgs
  VG #PV #LV #SN Attr   VSize  VFree
  cl   1   2   0 wz--n- 29.00g    0

$ vgcreate -s 16M vgcolor /dev/sda /dev/sdb # extend가 16M인 볼륩 그룹 생성
  Volume group "vgcolor" successfully created

$ vgdisplay vgcolor
  --- Volume group ---
  VG Name               vgcolor
  System ID             
  Format                lvm2
  Metadata Areas        2
  Metadata Sequence No  1
  VG Access             read/write
  VG Status             resizable
  MAX LV                0
  Cur LV                0
  Open LV               0
  Max PV                0
  Cur PV                2
  Act PV                2
  VG Size               1.97 GiB
  PE Size               16.00 MiB
  Total PE              126
  Alloc PE / Size       0 / 0   
  Free  PE / Size       126 / 1.97 GiB
  VG UUID               x3hO9Q-rIoL-BcQf-qcKP-LtqF-ysu0-7d0gQG
```
-  vgdisplay
	- `PE Size               4.00 MiB` 설정하지 않으면, 기본 값이 4MB인 것을 확인

#### LVM 실습 - 논리 볼륨 
이어서 계속
```bash
$ lvs
  LV   VG Attr       LSize  Pool Origin Data%  Meta%  Move Log Cpy%Sync Convert
  root cl -wi-ao---- 26.00g                                                    
  swap cl -wi-ao----  3.00g

$ lvcreate -L 150M -n lvred vgcolor # extend는 16MB고 150MB 를 원하니 그보다 좀 큰 160MB가 만들어진다.
  Rounding up size to full physical extent 160.00 MiB
  Logical volume "lvred" created.

$ lvcreate -l 10 -n lvblue vgcolor
  Logical volume "lvblue" created.

$ lvcreate -L 320M vgcolor
  Logical volume "lvol0" created.

$ lvcreate -l 50%FREE vgcolor
  Logical volume "lvol1" created.
  정
$ lvs
  LV     VG      Attr       LSize   Pool Origin Data%  Meta%  Move Log Cpy%Sync Convert
  root   cl      -wi-ao----  26.00g                                                    
  swap   cl      -wi-ao----   3.00g                                                    
  lvblue vgcolor -wi-a----- 160.00m                                                    
  lvol0  vgcolor -wi-a----- 320.00m                                                    
  lvol1  vgcolor -wi-a----- 688.00m                                                    
  lvred  vgcolor -wi-a----- 160.00m 
```
- lvcreate
	- `-L`: 용량 지정
	- `-l`: 개수 지정


#### LVM 실습 - 마운트

```bash
$ mkfs.xfs /dev/vgcolor/lvred
$ mkfs.ext4 /dev/vgcolor/lvblue
$ mkswap /dev/vgcolor/lvol1

$ mkdir /red
$ mkdir /blue

$ vi /etc/fstab
/dev/vgcolor/lvred      /red    xfs     defaults        0 2
/dev/vgcolor/lvblue     /blue   ext4    defaults        0 2
/dev/vgcolor/lvol1      swap    swap    defaults        0 0

$ mount -a # 자동 마운트
$ swapon -a # 자동 스왑

$ df # 마운트 확인
$ swapon -s # 스왑 확인
```
- /dev/{lv 그룹명}/{lv 이름}

#### LVM 실습 - 볼륨 그룹 추가
```bash
$ pvs
  PV         VG      Fmt  Attr PSize    PFree  
  /dev/sda   vgcolor lvm2 a--  1008.00m 368.00m
  /dev/sdb   vgcolor lvm2 a--  1008.00m 320.00m
  /dev/vda2  cl      lvm2 a--    29.00g      0 

$ pvcreate /dev/sdc
  Physical volume "/dev/sdc" successfully created.
$ pvs
  PV         VG      Fmt  Attr PSize    PFree  
  /dev/sda   vgcolor lvm2 a--  1008.00m 368.00m
  /dev/sdb   vgcolor lvm2 a--  1008.00m 320.00m
  /dev/sdc           lvm2 ---     1.00g   1.00g
  /dev/vda2  cl      lvm2 a--    29.00g      0 

$ vgextend vgcolor /dev/sdc
  Volume group "vgcolor" successfully extended

$ vgs
  VG      #PV #LV #SN Attr   VSize  VFree
  cl        1   2   0 wz--n- 29.00g    0 
  vgcolor   3   4   0 wz--n-  2.95g 1.66g

$ lvs
  LV     VG      Attr       LSize   Pool Origin Data%  Meta%  Move Log Cpy%Sync Convert
  root   cl      -wi-ao----  26.00g                                                    
  swap   cl      -wi-ao----   3.00g                                                    
  lvblue vgcolor -wi-ao---- 160.00m                                                    
  lvol0  vgcolor -wi-a----- 320.00m                                                    
  lvol1  vgcolor -wi-ao---- 688.00m                                                    
  lvred  vgcolor -wi-ao---- 160.00m

$ lvextend -L 480M /dev/vgcolor/lvred
  Size of logical volume vgcolor/lvred changed from 160.00 MiB (10 extents) to 480.00 MiB (30 extents).
  Logical volume vgcolor/lvred successfully resized.

$ lvextend -l 30 /dev/vgcolor/lvblue
  Size of logical volume vgcolor/lvblue changed from 160.00 MiB (10 extents) to 480.00 MiB (30 extents).
  Logical volume vgcolor/lvblue successfully resized.

$ lvs
  LV     VG      Attr       LSize   Pool Origin Data%  Meta%  Move Log Cpy%Sync Convert
  root   cl      -wi-ao----  26.00g                                                    
  swap   cl      -wi-ao----   3.00g                                                    
  lvblue vgcolor -wi-ao---- 480.00m                                                    
  lvol0  vgcolor -wi-a----- 320.00m                                                    
  lvol1  vgcolor -wi-ao---- 688.00m                                                    
  lvred  vgcolor -wi-ao---- 480.00m

$ df -h # lv의 용량을 늘려도 파일시스템의 용량이 이에 맞게 자동으로 증가하지는 않는다.
Filesystem                  Size  Used Avail Use% Mounted on
/dev/mapper/cl-root          26G  4.5G   22G  18% /
devtmpfs                    1.5G     0  1.5G   0% /dev
tmpfs                       1.5G  140K  1.5G   1% /dev/shm
tmpfs                       1.5G  8.9M  1.5G   1% /run
tmpfs                       1.5G     0  1.5G   0% /sys/fs/cgroup
/dev/vda1                  1014M  157M  858M  16% /boot
tmpfs                       304M  4.0K  304M   1% /run/user/42
tmpfs                       304M   28K  304M   1% /run/user/0
/dev/mapper/vgcolor-lvred   157M  8.2M  149M   6% /red
/dev/mapper/vgcolor-lvblue  151M  1.6M  139M   2% /blue

```
- **lvextend로  lv의 용량을 증가시켜도 파일 시스템의 용량이 자동으로 이에 맞게 증가하지 않는다.**
- lvextend
	- `-L`: 증가 후 최종 용량, 증가분이 아님

##### 파일 시스템 증가
```bash
$ xfs_growfs /dev/vgcolor/lvred
meta-data=/dev/mapper/vgcolor-lvred isize=512    agcount=4, agsize=10240 blks
         =                       sectsz=512   attr=2, projid32bit=1
         =                       crc=1        finobt=0 spinodes=0
data     =                       bsize=4096   blocks=40960, imaxpct=25
         =                       sunit=0      swidth=0 blks
naming   =version 2              bsize=4096   ascii-ci=0 ftype=1
log      =internal               bsize=4096   blocks=855, version=2
         =                       sectsz=512   sunit=0 blks, lazy-count=1
realtime =none                   extsz=4096   blocks=0, rtextents=0
data blocks changed from 40960 to 122880

$ resize2fs /dev/vgcolor/lvblue
resize2fs 1.42.9 (28-Dec-2013)
Filesystem at /dev/vgcolor/lvblue is mounted on /blue; on-line resizing required
old_desc_blocks = 2, new_desc_blocks = 4
The filesystem on /dev/vgcolor/lvblue is now 491520 blocks long.

$ df -h
Filesystem                  Size  Used Avail Use% Mounted on
/dev/mapper/cl-root          26G  4.5G   22G  18% /
devtmpfs                    1.5G     0  1.5G   0% /dev
tmpfs                       1.5G  140K  1.5G   1% /dev/shm
tmpfs                       1.5G  8.9M  1.5G   1% /run
tmpfs                       1.5G     0  1.5G   0% /sys/fs/cgroup
/dev/vda1                  1014M  157M  858M  16% /boot
tmpfs                       304M  4.0K  304M   1% /run/user/42
tmpfs                       304M   28K  304M   1% /run/user/0
/dev/mapper/vgcolor-lvred   477M  8.5M  469M   2% /red
/dev/mapper/vgcolor-lvblue  461M  2.3M  435M   1% /blue
```
- `xfs_growfs`: xfs 파일 시스템에서 사용
- `resize2fs` ext4 파일 시스템에서 사용 

##### 파일 시스템 축소
```bash
$ umount /blue # 축소 시키기 전에 마운트 해제 필요

$ resize2fs /dev/vgcolor/lvblue 320M # 줄이는 건 바로 가능하지 않고 유실 가능성을 먼저 체크해야한다
resize2fs 1.42.9 (28-Dec-2013)
Please run 'e2fsck -f /dev/vgcolor/lvblue' first.

$ e2fsck -f /dev/vgcolor/lvblue
e2fsck 1.42.9 (28-Dec-2013)
Pass 1: Checking inodes, blocks, and sizes
Pass 2: Checking directory structure
Pass 3: Checking directory connectivity
Pass 4: Checking reference counts
Pass 5: Checking group summary information
/dev/vgcolor/lvblue: 11/122880 files (0.0% non-contiguous), 21922/491520 blocks

resize2fs /dev/vgcolor/lvblue 320M
resize2fs 1.42.9 (28-Dec-2013)
Resizing the filesystem on /dev/vgcolor/lvblue to 327680 (1k) blocks.
The filesystem on /dev/vgcolor/lvblue is now 327680 blocks long.
```
- xfs 파일 시스템은 아직 줄이는 것이 존재하지 않다.

##### lv 축소
파일 시스템을 줄였으니 논리 볼륨의 사이즈도 맞춰 줄여준다.
``` bash
lvs
  LV     VG      Attr       LSize   Pool Origin Data%  Meta%  Move Log Cpy%Sync Convert
  root   cl      -wi-ao----  26.00g                                                    
  swap   cl      -wi-ao----   3.00g                                                    
  lvblue vgcolor -wi-a----- 480.00m                                                    
  lvol0  vgcolor -wi-a----- 320.00m                                                    
  lvol1  vgcolor -wi-ao---- 688.00m                                                    
  lvred  vgcolor -wi-ao---- 480.00m 
  
$ lvreduce -L 320M /dev/vgcolor/lvblue
  WARNING: Reducing active logical volume to 320.00 MiB.
  THIS MAY DESTROY YOUR DATA (filesystem etc.)
Do you really want to reduce vgcolor/lvblue? [y/n]: y
  Size of logical volume vgcolor/lvblue changed from 480.00 MiB (30 extents) to 320.00 MiB (20 extents).
  Logical volume vgcolor/lvblue successfully resized.

$ lvs
  LV     VG      Attr       LSize   Pool Origin Data%  Meta%  Move Log Cpy%Sync Convert
  root   cl      -wi-ao----  26.00g                                                    
  swap   cl      -wi-ao----   3.00g                                                    
  lvblue vgcolor -wi-a----- 320.00m                                                    
  lvol0  vgcolor -wi-a----- 320.00m                                                    
  lvol1  vgcolor -wi-ao---- 688.00m                                                    
  lvred  vgcolor -wi-ao---- 480.00m

$ mount -a
$ df -h
Filesystem                  Size  Used Avail Use% Mounted on
/dev/mapper/cl-root          26G  4.5G   22G  18% /
devtmpfs                    1.5G     0  1.5G   0% /dev
tmpfs                       1.5G  140K  1.5G   1% /dev/shm
tmpfs                       1.5G  8.9M  1.5G   1% /run
tmpfs                       1.5G     0  1.5G   0% /sys/fs/cgroup
/dev/vda1                  1014M  157M  858M  16% /boot
tmpfs                       304M  4.0K  304M   1% /run/user/42
tmpfs                       304M   28K  304M   1% /run/user/0
/dev/mapper/vgcolor-lvred   477M  8.5M  469M   2% /red
/dev/mapper/vgcolor-lvblue  306M  2.1M  287M   1% /blue
```
#### LVM 실습 - 볼륨 그룹 삭제
1. v
```bash
# 스왑된 파일시스템 off
$ swapoff /dev/vgcolor/lvol1
$ swapon -s

# 마운트된 파일시스템 해제
$ umount /red
$ umount /blue

# 논리 볼륨 제거
$ lvremove /dev/vgcolor/lvol0
Do you really want to remove active logical volume vgcolor/lvol0? [y/n]: y
  Logical volume "lvol0" successfully removed

$ lvs
  LV     VG      Attr       LSize   Pool Origin Data%  Meta%  Move Log Cpy%Sync Convert
  root   cl      -wi-ao----  26.00g                                                    
  swap   cl      -wi-ao----   3.00g                                                    
  lvblue vgcolor -wi-a----- 320.00m                                                    
  lvol1  vgcolor -wi-a----- 688.00m                                                    
  lvred  vgcolor -wi-a----- 480.00m

# 논리 그룹 제거 (논리 볼륨 한번에 제거하는 방법)
$ vgremove vgcolor
Do you really want to remove volume group "vgcolor" containing 3 logical volumes? [y/n]: y
Do you really want to remove active logical volume vgcolor/lvred? [y/n]: y
  Logical volume "lvred" successfully removed
Do you really want to remove active logical volume vgcolor/lvblue? [y/n]: y
  Logical volume "lvblue" successfully removed
Do you really want to remove active logical volume vgcolor/lvol1? [y/n]: y
  Logical volume "lvol1" successfully removed
  Volume group "vgcolor" successfully removed

$ lvs
lvs
  LV   VG Attr       LSize  Pool Origin Data%  Meta%  Move Log Cpy%Sync Convert
  root cl -wi-ao---- 26.00g                                                    
  swap cl -wi-ao----  3.00g  

$ vgs
  VG #PV #LV #SN Attr   VSize  VFree
  cl   1   2   0 wz--n- 29.00g    0


# 물리 볼륨 제거
$ pvs
  PV         VG Fmt  Attr PSize  PFree
  /dev/sda      lvm2 ---   1.00g 1.00g
  /dev/sdb      lvm2 ---   1.00g 1.00g
  /dev/sdc      lvm2 ---   1.00g 1.00g
  /dev/vda2  cl lvm2 a--  29.00g    0 

$ pvremove /dev/sdc # 디스크를 물리적으로 제거하기 전에 시스템에서 삭제(물리적 제거란 현재 VMM 시스템에서 하드웨어 제거하는 행위)
  Labels on physical volume "/dev/sdc" successfully wiped.
$ pvs
  PV         VG Fmt  Attr PSize  PFree
  /dev/sda      lvm2 ---   1.00g 1.00g
  /dev/sdb      lvm2 ---   1.00g 1.00g
  /dev/vda2  cl lvm2 a--  29.00g    0 
```


#### LVM 활성화
SSHD: SSD + HDD,  OS가 설치될 일부 영역만 SSD로 구성되고 나머지는 HDD 구성됨

lv에서도 이러한 기능이 가능

|Data [캐시]|
|--|
|Data LV [캐시]|
|VG|
|HDD + HDD + SSD [캐시]|
- write-back
	- 시스템 다운 시 데이터 유실 가능성
- write-through(default)


```bash
$ pvcreate /dev/sdc # 위에서 삭제한 내용 다시 생성
  Physical volume "/dev/sdc" successfully created.

$ pvs
  PV         VG Fmt  Attr PSize  PFree
  /dev/sda      lvm2 ---   1.00g 1.00g
  /dev/sdb      lvm2 ---   1.00g 1.00g
  /dev/sdc      lvm2 ---   1.00g 1.00g
  /dev/vda2  cl lvm2 a--  29.00g    0 

$ vgcreate vgcloud /dev/sda /dev/sdb /dev/sdc
  Volume group "vgcloud" successfully created

$ pvs -o +tags # 태그 확인
  PV         VG      Fmt  Attr PSize    PFree    PV Tags
  /dev/sda   vgcloud lvm2 a--  1020.00m 1020.00m        
  /dev/sdb   vgcloud lvm2 a--  1020.00m 1020.00m        
  /dev/sdc   vgcloud lvm2 a--  1020.00m 1020.00m        
  /dev/vda2  cl      lvm2 a--    29.00g       0  

# 태그 생성
$ pvchange --addtag slowhdd /dev/sda /dev/sdb
  Physical volume "/dev/sda" changed
  Physical volume "/dev/sdb" changed
  2 physical volumes changed / 0 physical volumes not changed
$ pvchange --addtag ssd /dev/sdc
  Physical volume "/dev/sdc" changed
  1 physical volume changed / 0 physical volumes not changed
  
$ pvs -o +tags
  PV         VG      Fmt  Attr PSize    PFree    PV Tags
  /dev/sda   vgcloud lvm2 a--  1020.00m 1020.00m slowhdd
  /dev/sdb   vgcloud lvm2 a--  1020.00m 1020.00m slowhdd
  /dev/sdc   vgcloud lvm2 a--  1020.00m 1020.00m ssd    
  /dev/vda2  cl      lvm2 a--    29.00g       0    


$ lvcreate -l 100%FREE -n lvdata vgcloud @slowhdd # 데이터 영역
WARNING: xfs signature detected on /dev/vgcloud/lvdata at offset 0. Wipe it? [y/n]: y
  Wiping xfs signature on /dev/vgcloud/lvdata.
  Logical volume "lvdata" created.

$ lvcreate --type cache-pool -l 100%FREE -n 역lvcache vgcloud @ssd # 캐시 영역 
  Using default stripesize 64.00 KiB.
  Logical volume "lvcache" created.

$ lvs # 현 시점에서 분리된 것으로 보임
  LV      VG      Attr       LSize    Pool Origin Data%  Meta%  Move Log Cpy%Sync Convert
  root    cl      -wi-ao----   26.00g                                                    
  swap    cl      -wi-ao----    3.00g                                                    
  lvcache vgcloud Cwi---C--- 1004.00m                                                    
  lvdata  vgcloud -wi-a-----    1.99g 

$ vgdisplay -v vgcloud # cache는 단독으로 사용할 수 없기 때문에 not available
# lvdata
#  LV Status              availabl
# lvcache
#  LV Status              NOT available


$ lvconvert --type cache --cachepool vgcloud/lvcache vgcloud/lvdata # 두개를 합쳐 하나로 사용
Do you want wipe existing metadata of cache pool volume vgcloud/lvcache? [y/n]: y
  Logical volume vgcloud/lvdata is now cached.

$ lvs # lv가 하나로 합쳐진 것을 확인
  LV     VG      Attr       LSize  Pool      Origin         Data%  Meta%  Move Log Cpy%Sync Convert
  root   cl      -wi-ao---- 26.00g                                                                 
  swap   cl      -wi-ao----  3.00g                                                                 
  lvdata vgcloud Cwi-a-C---  1.99g [lvcache] [lvdata_corig] 0.00   1.86            0.00       

# 읽기 성능 향상, SSD의 캐시를 이용하기 때문에
$ mkfs.xfs /dev/vgcloud/lvdata
$ mkdir /test에
$ mount /dev/vgcloud/lvdata /test
$ cp /etc/*.conf /test

$ lvs -o lv_name,cache_mode,cache_read,hits,cache_write_hits
```

## NAS

### NFS
#### NFSv4
`2049` 포트 단일화

제약 조건
- kernel 2.6.33+
- nfs-utils 1.2.2+

#### NFS Client
**host 컴퓨터에서 진행**

`server1`은 day01에 /etc/hosts 에 설정한 서버
`59.29.224.177 server1.example.com server1`

```bash
$ showmount -e server1
Export list for server1:
/export/courserepos *
/export/autoyast    *
/export/home        *
/export/tmp         *
/export/netinstall  *
/export/kickstart   *

$ mkdir /aaa
$ mount -t nfs server1:/export/home /aaa

$ df
...
server1:/export/home 487108608 21967872 465140736   5% /aaa
$ ls /aaa
...

$ mount | grep /aaa
server1:/export/home on /aaa type nfs4 (rw,relatime,vers=4.0,rsize=1048576,wsize=1048576,namlen=255,hard,proto=tcp,port=0,timeo=600,retrans=2,sec=sys,clientaddr=59.29.224.192,local_lock=none,addr=59.29.224.177)

$ umount /aaa

$ mount -t nfs -o vers=3 server1:/export/home /aaa
$ mount | grep /aaa
server1:/export/home on /aaa type nfs (rw,relatime,vers=3,rsize=1048576,wsize=1048576,namlen=255,hard,proto=tcp,timeo=600,retrans=2,sec=sys,mountaddr=59.29.224.177,mountvers=3,mountport=20048,mountproto=udp,local_lock=none,addr=59.29.224.177)
```

### NFS Server
1. host, VM에 같은 세팅
	- setenforce
	- systemctl
2. nfs-server (VM)
3. client(host)
	- mount


#### host
```bash
# host
$ setenforce 0
$ systemctl stop firewalld
```

####  VM
nfs-server
192.168.122.149
```bash
# VM
$ setenforce 0
$ systemctl stop firewalld

$ mkdir /nfsdata
$ touch /nfsdata/sample
$ chmod 1777 /nfsdata

$ vi /etc/exports # 이 정보를 기반으로 nfs-server 구동
/nfsdata        192.168.122.0/24(rw,sync)  59.29.224.5(ro)

$ systemctl status nfs-server
● nfs-server.service - NFS server and services
   Loaded: loaded (/usr/lib/systemd/system/nfs-server.service; disabled; vendor preset: disabled)
   Active: inactive (dead)

$ systemctl start nfs-server

$ systemctl status nfs-server
● nfs-server.service - NFS server and services
   Loaded: loaded (/usr/lib/systemd/system/nfs-server.service; disabled; vendor preset: disabled)
   Active: active (exited) since Wed 2020-02-26 14:31:25 KST; 14s ago
  Process: 9060 ExecStart=/usr/sbin/rpc.nfsd $RPCNFSDARGS (code=exited, status=0/SUCCESS)
  Process: 9059 ExecStartPre=/usr/sbin/exportfs -r (code=exited, status=0/SUCCESS)
 Main PID: 9060 (code=exited, status=0/SUCCESS)
   CGroup: /system.slice/nfs-server.service

Feb 26 14:31:25 localhost.localdomain systemd[1]: Starting NFS server and services...
Feb 26 14:31:25 localhost.localdomain systemd[1]: Started NFS server and services.
```

```bash
# host
$ showmount -e 192.168.122.149
Export list for 192.168.122.149:
/nfsdata 192.168.122.0/24,59.29.224.5

$ mount -t nfs 192.168.122.149:/nfsdata /mnt

$ ll /mnt
-rw-r--r--. 1 root root 0 Feb 26 14:27 sample

$ whoami
root
$ touch /mnt/1.txt
$ ll /mnt # 새로 만든 파일의 소유권은 root가 아니다.
total 0
-rw-r--r--. 1 nfsnobody nfsnobody 0 Feb 26 14:35 1.txt
-rw-r--r--. 1 root      root      0 Feb 26 14:27 sample
```

```bash
# VM
$ vi /etc/exports # 설정 수정
nfsdata        192.168.122.0/24(rw,sync,no_root_squash)  59.29.224.5(ro)

$ exportfs -vra
exporting 59.29.224.5:/nfsdata
exporting 192.168.122.0/24:/nfsdata
```
- 설정을 바꾼 뒤 `nfs-server` 서버를 다시 시작하지 않고 `exportfs -vra`를 이용하여 설정을 다시 불러온다.

```bash
# host
$ touch /mnt/2.txt
$ ll /mnt
total 0
-rw-r--r--. 1 nfsnobody nfsnobody 0 Feb 26 14:35 1.txt
-rw-r--r--. 1 root      root      0 Feb 26 14:38 2.txt
-rw-r--r--. 1 root      root      0 Feb 26 14:27 sample
```
- `2.txt`는 `root` 소유로 생성된다.


#### AutoFS
> 클라이언트의 설정으로
> 사용할 때 moun
> 사용하지 않으면 umount


1. 이전 실습 모두 umount
1. `/etc/auto.master` 수정

```bash
$ df
...
server1:/export/home     487108608 21967872 465140736   5% /aaa
192.168.122.149:/nfsdata  27246080  4650496  22595584  18% /mnt


$ umount /aaa
$ umount /mnt

$ cat -n /etc/auto.master
...
/misc	/etc/auto.misc

$ cat -n /etc/auto.misc
...
cd		-fstype=iso9660,ro,nosuid,nodev	:/dev/cdrom
```

##### /etc/auto.master
|접근 디렉토리| 설정 파일|
|--|--|
|/misc|/etc/auto.misc|
- `/misc`: 클라이언트 PC에 접근할 디렉토리
- `/etc/auto.misc`: 해당 설정 파일로 자동으로 마운트

##### /etc/auto.misc
- `cd`: `/etc/auto.master`의 접근 디렉토리인 `/misc` 의 `상대 경로`
- `:dev/cdrom`: 현재 PC의 `/dev/cdrom`


##### AutoFS Direct vs Indirect
- Direct: `/etc/auto.master` 첫 컬럼 `/-`로 시작
- Indirect: `/etc/auto.master` 첫 컬럼 `/`로 시작

```bash
$ vi /etc/auto.master
...
/-      /etc/dir.auto
/hp     /etc/indir.auto

$ vi /etc/aaa # 새로 만든 파일
/ibm    -rw,sync,soft   192.168.122.149:/nfsdata

$ vi /etc/indir.auto
sun     -rw,sync        server1:/export/courserepos
google  -rw,sync        server1:/export/netinstall
amazon  -rw,sync        server1:/export/kickstart


$ systemctl status autofs
...

$ systemctl restart autofs # autofs

$ ls /ibm # host에서 만들지 않았지만 autofs의 설정으로 192.168.122.149:/nfsdata을 자동으로 마운트했다.
1.txt  2.txt  sample

$ ls /hp # 최초엔 아무것도 없다
$ ls /hp/sun # 첫번째 접근 시 없으면 auto 마운트로 nfs로 가져온다.
$ ls /hp/amazone
$ ls /hp/google
$ ls /hp
amazon  google  sun
```

## iSCSI
IQN: iSCSI 식별자

iSCSI 설정은 명령어로 입력한 것들이 영구적으로 저장된다. 만약, 명령어를 잘못 입력한다면 해당 데이터를 지워야한다. (`/var/lib/iscsi`)

```bash
# host

$ cat /etc/iscsi/initiatorname.iscsi
InitiatorName=iqn.1994-05.com.redhat:c68cb971b3ba
```

```bash
# host

$ rpm -qa  | grep iscsi
iscsi-initiator-utils-iscsiuio-6.2.0.873-35.el7.x86_64
iscsi-initiator-utils-6.2.0.873-35.el7.x86_64
libiscsi-1.9.0-7.el7.x86_64

$ vi /etc/iscsi/iscsid.conf # 57,61,62,66,67 주석 제거 / 61,62 수정 / 66, 67 수정
...
node.session.auth.authmethod = CHAP
...
node.session.auth.username = stationX 
node.session.auth.password = letmein
...
node.session.auth.username_in = server1
node.session.auth.password_in = itsreallyme


$ ls /var/lib/iscsi/nodes/ # 아무것도 없음
$ ls /var/lib/iscsi/send_targets/ # 아무것도 없음

$ iscsiadm -m discovery -t st -p server1
...
...
$ ls /var/lib/iscsi/nodes/ # 매우 많음
$ ls /var/lib/iscsi/send_targets/ # 매우 많음

# 제거
$ iscsiadm -m discovery -p server1 -o delete
$ ls /var/lib/iscsi/nodes/ # 전체 삭제됨
$ ls /var/lib/iscsi/send_targets/ # 전체 삭제됨

# 다시 복귀
$ iscsiadm -m discovery -t st -p server1

###

$ lsblk

$ iscsiadm -m node -T iqn.1999-07.com.example:station28 -l # 오류 발생

### 오류 발생으로 일단 방법만 
$ fdisk # 파티셔닝
 # n p 1 () () p w

$ lsblk
$ mkfs.xfs /dev/sdb1
$ mkdir /san
$ vi /etc/fstab
/dev/sdb1               /san                    xfs     _netdev         0 2
$ mount -a # 마운트 설정 확인
$ systemctl is-enabled iscsid
disabled
$ systemctl enable iscsid
Created symlink from /etc/systemd/system/multi-user.target.wants/iscsid.service to /usr/lib/systemd/system/iscsid.service.
$ reboot

$ umount /san
$ systemctl disable iscsid
...


```
- `iscsi-initiator-utils` 필요, 없으면 설치

iqn.1999-07.com.example:station

# 네트워크 연결 구성하기

- NAT
- NAPT: 포트까지 변경

---

### 랜카드
centos7+ 에선 `eth`란 명칭을 사용하지 않는다.

- en:유선,
	- en{o}: `o`nboard, 내장된
	- en{s}: plug, 탈부착 가능
	- p{}s{}: 
- wl: 무선
- ww:  무선

```bash
$ ifconfig
enp2s0: ...
```
- en`p`2`s`0


## 실습
`snapshot1`로 돌아가기
```bash
# VM
$ cat /etc/default/grub
GRUB_TIMEOUT=9
GRUB_DISTRIBUTOR="$(sed 's, release .*$,,g' /etc/system-release)"
GRUB_DEFAULT=saved
GRUB_DISABLE_SUBMENU=true
GRUB_TERMINAL_OUTPUT="console"
GRUB_CMDLINE_LINUX="rd.lvm.lv=vg0/root rd.lvm.lv=vg0/swap rhgb quiet" # quiet: 부팅 시 메세지 보지 않겠다
GRUB_DISABLE_RECOVERY="true"

$ vi /etc/default/grub # 부팅 설정 변경
GRUB_CMDLINE_LINUX="rd.lvm.lv=vg0/root rd.lvm.lv=vg0/swap ipv6.disable=1"

$ grub2-mkconfig -o /boot/grub2/grub.cfg
Generating grub configuration file ...
Found linux image: /boot/vmlinuz-3.10.0-514.26.1.el7.x86_64
Found initrd image: /boot/initramfs-3.10.0-514.26.1.el7.x86_64.img
Found linux image: /boot/vmlinuz-0-rescue-c0a3bfb337684ae8911711c0aa6b6a49
Found initrd image: /boot/initramfs-0-rescue-c0a3bfb337684ae8911711c0aa6b6a49.img
done

$ reboot # 부팅 시 

$ ifconfig # ipv6 내용 없는 거 확인
$ cat /proc/cmdline
```
- GRUB_TIMEOUT: 부팅 대기 시간
- GRUB_CMDLINE_LINUX: 실제 커널에 전달되는 값

**quiet 제거 시**
![quiet-boot](https://user-images.githubusercontent.com/9030565/75327639-1a835080-58c0-11ea-93a6-f6d37fe3fa0a.png)


```bash
# host
$ ifconfig
enp2s0:

$ ethtool enp2s0
...
	Speed: 1000Mb/s
	Duplex: Full
	Port: MII
	PHYAD: 0
	Transceiver: internal
	Auto-negotiation: on
...

$ ethtool -s enp2s0 autoneg off speed 100
$ ethtool enp2s0
...
	Speed: 100Mb/s
	Duplex: Full
	Port: MII
	PHYAD: 0
	Transceiver: internal
	Auto-negotiation: off
...

$ ethtool -s enp2s0 autoneg on speed 1000 # 롤백
```
- `enp2s0`: PC 환경에 따라 바뀜


## ip

아래 명령어들은 쓰지 않는 것을 권고하지만, 아직 많이 쓰임
Docker에서도 최소 설치 시 제외됨(`net-tools` 설치 필요)
- ifconfig
- route

`ip`로 가능
```bash
$ ip addr
$ ip route
$ ip neigh
```
- 하지만, 실제로 `ip` 명령어 보단 전통적인 명령어를 주로 사용한다.

## DNS Client
FQDN: `호스트` + `도메인명`

## NetwokManager
CentOS5 버전부터 존재
7버전에서 개선

### nmcli
- `nmtui`: gui 기반
- `nmcli`: 명령어 기반

#### nmcli로 네트워크 추가/수정 하기
```bash
# VM
$ nmcli dev # 인식된 네트워크 디바이스 확인
DEVICE      TYPE      STATE      CONNECTION 
virbr0      bridge    connected  virbr0     
enp2s0      ethernet  connected  enp2s0     
vnet0       tun       connected  vnet0      
lo          loopback  unmanaged  --         
virbr0-nic  tun       unmanaged  --  

$ nmcli connection 
NAME    UUID                                  TYPE            DEVICE 
eth0    72df41d4-f64a-47cd-8e08-0913b0e575f0  eth0   
virbr0  b0f9fc63-ce98-4c74-b36b-2886b9c61bea  bridge          virbr0 

$ ls /etc/sysconfig/network-scripts/ # 조회된 리스트들이 현재 설정된 네트워크들
... ifcfg-enp2s0 ifcfg-eth0 ...
$ nmcli con delete eth0 # 설정 파일 제거
Connection 'eth0' (72df41d4-f64a-47cd-8e08-0913b0e575f0) successfully deleted.
$ nmcli con delete 'Wired connection 1' # network-scripts 에서 아직 조회되기 때문에 삭제, eth0은 설정 파일만 지운 것이고 이 명령어도 추가하여 완전히 제거
Connection 'Wired connection 1' (241d59dd-6cb8-3957-ad76-979b67348c55) successfully deleted.

$ nmcli con add type ethernet con-name eth0 ifname eth0
Connection 'eth0' (fe91efcb-a949-4ae4-aac9-cbd0e1f9deec) successfully added.

## 설정 파일 만들기
$ nmcli con modify eth0 ipv4.addresses 192.168.122.149/24 ipv4.gateway 192.168.122.1 ipv4.dns 192.168.122.1 ipv4.method manual ipv4.method manual autoconnect yes

$ nmcli con show eth0 | grep ipv4

$ systemctl restart network # 수정 적용

$ ifconfig eth0 # 확인
```
- `nmlci con modify`
	- DHCP로 자동으로 받아오려는 경우  address, gateway, dns 모두 필요 없고 method을 `auto`로 해주면 된다.
	- method: `manual|auto` DHCP
	- autoconnect: 자동으로 설정 적용할 지 

#### nmtui로 추가한 네트워크 설정 확인하기

ping 확인
1. 자기 자신
2. 게이트웨이
3. 외부 네트워크(kornet)
4. DNS
```bash
# VM
$ ping -c 3 localhost # 자기 자신
PING localhost (127.0.0.1) 56(84) bytes of data.
64 bytes from localhost (127.0.0.1): icmp_seq=1 ttl=64 time=0.031 ms
64 bytes from localhost (127.0.0.1): icmp_seq=2 ttl=64 time=0.054 ms
64 bytes from localhost (127.0.0.1): icmp_seq=3 ttl=64 time=0.054 ms

--- localhost ping statistics ---
3 packets transmitted, 3 received, 0% packet loss, time 1999ms
rtt min/avg/max/mdev = 0.031/0.046/0.054/0.012 ms

$ ping -c 3 192.168.122.1 # 게이트웨이
... 생략

$ ping -c 3 168.126.63.1 # 외부 네트워크
... 생략

$ ping -c 3 www.google.co.kr # DNS
... 생략

```

##### nslookup
```bash
$ nslookup www.daum.net # non-interactive, 결과가 나오고 프롬프트 끝
Server:		192.168.122.1
Address:	192.168.122.1#53

Non-authoritative answer:
www.daum.net	canonical name = www.g.daum.net.
Name:	www.g.daum.net
Address: 203.133.167.16
Name:	www.g.daum.net
Address: 203.133.167.81

$ nslookp # interactive, 대화형 (결과는 길기 때문에 생략, 직접 써보길)
> www.naver.com
> set type=mx
> naver.com
> set type=ns
> naver.com
> set type=a
> www.naver.com
```
- set
	- mx: 메일 서버 확인
	- ns: 네임 서버 확인

##### dig
```bash
$ dig www.naver.com

; <<>> DiG 9.9.4-RedHat-9.9.4-38.el7_3.3 <<>> www.naver.com
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 8264
;; flags: qr rd ra; QUERY: 1, ANSWER: 3, AUTHORITY: 3, ADDITIONAL: 4

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 4096
;; QUESTION SECTION:
;www.naver.com.			IN	A

;; ANSWER SECTION:
www.naver.com.		13547	IN	CNAME	www.naver.com.nheos.com.
www.naver.com.nheos.com. 71	IN	A	210.89.160.88
www.naver.com.nheos.com. 71	IN	A	125.209.222.142

;; AUTHORITY SECTION:
nheos.com.		166525	IN	NS	gns2.nheos.com.
nheos.com.		166525	IN	NS	gns1.nheos.com.
nheos.com.		166525	IN	NS	gns3.nheos.com.

;; ADDITIONAL SECTION:
gns1.nheos.com.		14725	IN	A	103.6.174.86
gns2.nheos.com.		14113	IN	A	210.89.165.22
gns3.nheos.com.		14304	IN	A	125.209.246.230

;; Query time: 1 msec
;; SERVER: 192.168.122.1#53(192.168.122.1)
;; WHEN: Thu Feb 27 10:19:10 KST 2020
;; MSG SIZE  rcvd: 213
```

### netstat
- `netstat -nr` = route
- `netstat -ntulp`
	- `n`: 숫자
	-  `t`: TCP
	- `u`: UDP
	- `l`: Listen
	- `p`: Program
- `netstat -an`

## Interface Aggregation
https://ko.wikipedia.org/wiki/링크_애그리게이션

### Network Teaming
인터페이스 1개에 물리 랜카드 2개를 합침 (bonding, teaming)

1. 랜카드 추가
2. 인터페이스 만들기
3. 랜카드를 인터페이스에 연결

####  랜카드 추가
VM 에  랜카드 추가 
![create-network](https://user-images.githubusercontent.com/9030565/75405740-df316200-5951-11ea-8900-50961e39b784.png)

```bash
# VM
$ nmcli dev # ens9 ens10가 추가됨
DEVICE      TYPE      STATE      CONNECTION         
virbr0      bridge    connected  virbr0             
ens10       ethernet  connected  Wired connection 2 
ens9        ethernet  connected  Wired connection 1 
eth0        ethernet  connected  eth0               
lo          loopback  unmanaged  --                 
virbr0-nic  tun       unmanaged  --     

## 실제로 존재하지 않는 내용이기 때문에 제거 후 설정 파일부터 다시 생성
$ nmcli connection delete 'Wired connection 1'
Connection 'Wired connection 1' (be9f8817-b152-3150-88de-8f710b7965b1) successfully deleted.
$ nmcli connection delete 'Wired connection 2'
Connection 'Wired connection 2' (bae93ce6-08e5-349f-aac5-99f783f544c0) successfully deleted.

$ nmcli dev
DEVICE      TYPE      STATE         CONNECTION 
virbr0      bridge    connected     virbr0     
eth0        ethernet  connected     eth0       
ens10       ethernet  disconnected  --         
ens9        ethernet  disconnected  --         
lo          loopback  unmanaged     --         
virbr0-nic  tun       unmanaged     --         
```

#### teaming
기존 linux bonding울 보완하여 CentOS7 부터 지원하는 네트워크 이중화 구성

##### teaming - 인터페이스 만들기
```bash
$ nmcli connection add type team con-name team0 ifname team0 autoconnect yes config '{"runner": {"name": "activebackup"}}'
Connection 'team0' (4fe0aaff-1388-48d2-b66f-159eb8b99dc2) successfully added.

$ ls /etc/sysconfig/network-scripts/
... ifcfg-team0 ...

$ nmcli connection modify team0 ipv4.addresses 192.168.122.22/24 ipv4.method manual
```

##### teaming - 인터페이스에 랜카드 연결
```bash
$ nmcli connection add type team-slave con-name team0-port1 ifname ens9 master team0
Connection 'team0-port1' (a1976d18-1769-46b4-adb6-8ff18f385325) successfully added.

$ nmcli connection add type team-slave con-name team0-port2 ifname ens10 master team0
Connection 'team0-port2' (29db48ef-dfc3-4632-ab17-4a18111a1aaa) successfully added.

$ ls /etc/sysconfig/network-scripts/
... ifcfg-team0 ifcfg-team0-port1 ifcfg-team0-port2 ...

## 위까진 설정 파일만 만든것
$ systemctl restart network # 설정 파일 적용

$ ifconfig
...
team0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 192.168.122.22  netmask 255.255.255.0  broadcast 192.168.122.255
        ether 52:54:00:8d:c2:50  txqueuelen 1000  (Ethernet)
        RX packets 10  bytes 1381 (1.3 KiB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 17  bytes 1999 (1.9 KiB)
        TX errors 0  dropped 2 overruns 0  carrier 0  collisions 0
...

$ eamdctl team0 state
setup:
  runner: activebackup
ports:
  ens10
    link watches:
      link summary: up
      instance[link_watch_0]:
        name: ethtool
        link: up
        down count: 0
  ens9
    link watches:
      link summary: up
      instance[link_watch_0]:
        name: ethtool
        link: up
        down count: 0
runner:
  active port: ens9
```
- ifname: 추가한 랜카드 이름, `ens`{}
- master: 인터페이스 이름

##### teaming - team 관리
``` bash
$ teamdctl team0 state item set runner.active_port ens10 # active port 변경

$ teamdctl team0 state
setup:
  runner: activebackup
ports:
  ens10
    link watches:
      link summary: up
      instance[link_watch_0]:
        name: ethtool
        link: up
        down count: 0
  ens9
    link watches:
      link summary: up
      instance[link_watch_0]:
        name: ethtool
        link: up
        down count: 0
runner:
  active port: ens10
```

##### teaming - 장애 발생 시나리오
```bash
$ nmcli device disconnect ens10 # 장애 상황으로 active가 down이라 가정
Device 'ens10' successfully disconnected.

$ teamdctl team0 state
setup:
  runner: activebackup
ports:
  ens9
    link watches:
      link summary: up
      instance[link_watch_0]:
        name: ethtool
        link: up
        down count: 0
runner:
  active port: ens9

$ nmcli connection up team0-port2 # 장애 해결로 up이라 가정
Connection successfully activated (D-Bus active path: /org/freedesktop/NetworkManager/ActiveConnection/15)

$ teamdctl team0 state
setup:
  runner: activebackup
ports:
  ens10
    link watches:
      link summary: up
      instance[link_watch_0]:
        name: ethtool
        link: up
        down count: 0
  ens9
    link watches:
      link summary: up
      instance[link_watch_0]:
        name: ethtool
        link: up
        down count: 0
runner:
  active port: ens9
```

##### teaming - 설정 파일로 관리하기
```bash
$ vi /etc/sysconfig/network-scripts/ifcfg-team0 # runner 변경
TEAM_CONFIG="{\"runner\": {\"name\": \"loadbalance\"}}"
	# nmcli connection modify config '{"runner": {"name": "loadbalance"}}'

$ systemctl restart network

$ teamdctl team0 state
setup:
  runner: loadbalance
ports:
  ens10
    link watches:
      link summary: up
      instance[link_watch_0]:
        name: ethtool
        link: up
        down count: 0
  ens9
    link watches:
      link summary: up
      instance[link_watch_0]:
        name: ethtool
        link: up
        down count: 0
```
- runner
	- `activebackup` : 하나가 동작, 나머지 대기
	- `loadbalance`: 밸런싱

### Interface Bridging

#### 브릿지
1. bridge 생성
2. bridge-slave 생성
3. vlan 생성
4. 


#####  team 정보 모두 제거
```bash
# VM
$ nmcli device disconnect ens9
$ nmcli device disconnect ens10
$ nmcli connection delete team0
$ nmcli connection delete team0-port1
$ nmcli connection delete team0-port2

$ systemctl restart network

$ ifconfig
```
#####  브릿지 만들기
```bash
$ nmcli connection add type bridge con-name br0 ifname br0 autoconnect yes
Connection 'br0' (c3c0cf27-5c8e-4b0c-91c9-4ad7ede92334) successfully added.

$ nmcli connection modify br0 ipv4.addresses 192.168.122.33/24 ipv4.method manual
```

##### 브릿지에 랜카드 연결
``` bash
$ nmcli connection add type bridge-slave con-name br0-port1 ifname ens9 master br0
Connection 'br0-port1' (e40dbe49-5318-45aa-b0e3-dc6cf5ab6965) successfully added.

$ systemctl restart network
$ ifconfig
```
##### 브릿지 관리
```bash
$ brctl show
bridge name	bridge id		STP enabled	interfaces
br0		8000.5254008dc250	yes		ens9
virbr0		8000.5254004f18fc	yes		virbr0-nic
```

#####  VLAN
```bash
$ nmcli dev # VLAN으로 사용할 랜카드가 연결되지 않았는지 확인
DEVICE      TYPE      STATE         CONNECTION 
br0         bridge    connected     br0        
virbr0      bridge    connected     virbr0     
ens9        ethernet  connected     br0-port1  
eth0        ethernet  connected     eth0       
ens10       ethernet  disconnected  --         
lo          loopback  unmanaged     --         
virbr0-nic  tun       unmanaged     --       

$ nmcli connection add type vlan con-name vlan10 dev ens10 id 10 # vlan 추가
Connection 'vlan10' (7af6c451-07b7-4c17-8c49-2467f23cc721) successfully added.

$ nmcli connection modify vlan10 ipv4.addresses 192.168.122.44/24 ipv4.gateway 192.168.122.1 ipv4.dns 192.168.122.1 ipv4.method manual # vlan 정보 수정

$ nmcli connection up vlan10 # network 재시작하지 않고 vlan 정보 설정하기
Connection successfully activated (D-Bus active path: /org/freedesktop/NetworkManager/ActiveConnection/29)

$ ifconfig
...
ens10.10: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 192.168.122.44  netmask 255.255.255.0  broadcast 192.168.122.255
        ether 52:54:00:9e:81:53  txqueuelen 1000  (Ethernet)
        RX packets 0  bytes 0 (0.0 B)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 27  bytes 4323 (4.2 KiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
...

$ lsmod | grep 802 # 브릿지 설정 모듈 존재하는지 확인
8021q                  33104  0
```


### Tuning Kernel Network
```bash
# host
# VM에 ping 테스트
$ ping -c 3 192.168.122.149
PING 192.168.122.149 (192.168.122.149) 56(84) bytes of data.
64 bytes from 192.168.122.149: icmp_seq=1 ttl=64 time=0.178 ms
64 bytes from 192.168.122.149: icmp_seq=2 ttl=64 time=0.182 ms
64 bytes from 192.168.122.149: icmp_seq=3 ttl=64 time=0.341 ms

--- 192.168.122.149 ping statistics ---
3 packets transmitted, 3 received, 0% packet loss, time 2000ms
rtt min/avg/max/mdev = 0.178/0.233/0.341/0.077 ms
[root@station14 /]# ping -c 3 192.168.122.149
PING 192.168.122.149 (192.168.122.149) 56(84) bytes of data.
```
- ping 제대로 동작

```bash
# VM
$ vi /etc/sysctl.conf # ICMP 무시 설정 추가
net.ipv4.icmp_echo_ignore_all = 1

$ sysctl -p # reboot 없이 설정 적용
```
- ping  설정으로 무시

```bash
# host
ping -c 3 192.168.122.149
PING 192.168.122.149 (192.168.122.149) 56(84) bytes of data.

--- 192.168.122.149 ping statistics ---
3 packets transmitted, 0 received, 100% packet loss, time 1999ms
```
- ping 동작하지 않음

# 컨테이너를 위한 보안 설정 SELinux



커널
- File system
- H/W driver
- SELinux
	- [LSM](https://ko.wikipedia.org/wiki/리눅스_보안_모듈)
	- Network

접근 제어 모델
- DAC: 임의 접근 제어
	- unix 의 퍼미션
	- 소유자가 잘못된 세팅을 할 수 있는 단점이 있음
- MAC: 강제 접근 제어
	- `policy`가 핵심
	- 정책을 위반한 접근은 차단 (퍼미션이 있다고 하더라도)
	- SELinux
	- 중앙 집중화된 관리가 가능
- RAC: 역할 기반 접근 제어


```bash
# host
# Security Context 확인

$ id
uid=0(root) gid=0(root) groups=0(root) context=unconfined_u:unconfined_r:unconfined_t:s0-s0:c0.c1023

$ ll -Z # Security Context 확인

$ ps -eZ # Security Context 확인
```

## SELinux

SELinx가 보안 정책을 가져옴 
1.퍼미션 확인 2.라벨 확인 모두 OK -> 접근 허가가

### SELinux 동작 확인
1. 웹 서버로 SELinux 동작 확인
2. 모드 변경

``` bash
# VM
$ getenforce # Enforcing 가 나와야 한다.
Enforcing
```

#### 웹 서버로 SELinux 동작 확인

**퍼미션이 같은 파일이라도 `SELinux`의 라벨 설정에 따라 접근이 가능 여부가 달라진다.**

```bash
$ yum -y install httpd

$ cd /root
$ cp initial-setup-ks.cfg /var/www/html/1.html
$ mv initial-setup-ks.cfg /var/www/html/2.html

$ ll /var/www/html/
total 8
-rw-r--r--. 1 root root 1546 Feb 27 12:37 1.html
-rw-r--r--. 1 root root 1546 Feb 25 09:09 2.html


$ systemctl start httpd # 웹 서버 실행

## 브라우저에서 http://127.0.0.1/
## 1.html 2.html 접근 가능 여부가 다름

$ ll -Z /var/www/html/
-rw-r--r--. root root unconfined_u:object_r:httpd_sys_content_t:s0 1.html
-rw-r--r--. root root system_u:object_r:admin_home_t:s0 2.html
```
- http://127.0.0.1/1.html : 접근 가능
- http://127.0.0.1/2.html : 접근 불가 (403)

### SELinux 모드 enforcing permissive disable
##### enforcing
강제 모드
접근을 위반할 시 `차단, 로깅`

##### permissive
접근을 위반할 시 `로깅`

##### disable
접근을 위반할 시 `아무것도 하지 않음`


#### 모드 변경
##### SELinux change
- `set enforce 1`: permissive -> enforcing
- `set enforce 0`: enforcing -> permissive
> 부팅 상태에서만 가능

##### SELinux on/off
 disable <-> permissive/enforcing
 > 명령어로 불가
 > 부팅 시 결정됨
 >  `/etc/sysconfig/selinux`의 설정으로 결정됨


```bash
# VM
$ getenforce
Enforcing

$ setenforce 0
$ getenforce
Permissive
# http://127.0.0.1/2.html 접근 가능

$ setenforce 1
$ getenforce
Enforcing
# http://127.0.0.1/2.html 접근 불가(403)
```

```bash
# VM
$ vi /etc/sysconfig/selinux
SELINUX=disabled

$ reboot

$ getenforce
Disabled

# VM 종료
# `snapshot1`로 롤백
```

### SELinux 명령어
`snapshot1`로 돌아간 뒤 실습

```bash
$ matchpathcon /tmp /etc /root /boot

$ll -Z
-rw-------. root root system_u:object_r:admin_home_t:s0 anaconda-ks.cfg
...

$ ll -Z
-rw-------. root root system_u:object_r:etc_t:s0       anaconda-ks.cfg

$ restorecon -vvFR /root # matchpathcon 내용으로 복구

$ ll -Z
-rw-------. root root system_u:object_r:admin_home_t:s0 anaconda-ks.cfg
...

```
- matchpathcon: security context 확인
- chcon
	- `-t` type
- restorecon: 각 디렉토리마다 초기에 정의된 security context가 존재. 이것을 기반으로 복구

### SELinux Booleans
SELinux 정책 확인

```bash천
# VM
# 아래 모두 같은 기능
$ getsebool -a # 강사님 추천
$ sestatus -b
```

#### 실습
1. host 에서 vm 의 /tmp 로 파일 전송 (ftp)

```bash
# VM
$ yum -y install vsftpd ftp

$ systemctl start vsftpd
$ systemctl status vsftpd

$ systemctl stop firewalld

$ ll -d /tmp # 777
drwxrwxrwt. 16 root root 4096 Feb 27 14:40 /tmp
```

```bash
# host
$ yum -y install ftp

$ cp /etc/fstab /tmp/se.txt

$ ftp 192.168.122.149
Name:
Password:
230 Login successful.
Remote system type is UNIX.
Using binary mode to transfer files.
ftp> cd /tmp
250 Directory successfully changed.
ftp> put se.txt 
local: se.txt remote: se.txt
227 Entering Passive Mode (192,168,122,149,92,58).
553 Could not create file.
```
- 업로드 불가, `777`이지만 SELinux 설정 때문에

```bash
# VM
$ rpm -qa | grep setroubleshoot # 아래 패키지가 있어야 SELinux 관련 문제 발생 시 로깅. 만약 패키지가 존재하지 않다면 설치 후 재부팅 필요
setroubleshoot-plugins-3.0.64-2.1.el7.noarch
setroubleshoot-3.2.27.2-3.el7.x86_64
setroubleshoot-server-3.2.27.2-3.el7.x86_64

$ cat /var/log/messages
...
Feb 27 14:49:50 localhost setroubleshoot: SELinux is preventing vsftpd from create access on the file se.txt. For complete SELinux messages. run sealert -l 5777e6a8-49a4-4d00-a41b-d2128aee3a2f
...

$ sealert -l 5777e6a8-49a4-4d00-a41b-d2128aee3a2f
...
setsebool -P ftpd_full_access 1
...

$ getsebool -a | grep ftp
ftpd_anon_write --> off
ftpd_connect_all_unreserved --> off
ftpd_connect_db --> off
ftpd_full_access --> off
ftpd_use_cifs --> off
ftpd_use_fusefs --> off
ftpd_use_nfs --> off
ftpd_use_passive_mode --> off
httpd_can_connect_ftp --> off
httpd_enable_ftp_server --> off
tftp_anon_write --> off
tftp_home_dir --> off

$ setsebool -P ftpd_full_access 1
```
- setsebool
	- `-P`: persistent, 재부팅 시도 적용되게 설정

```bash
# host
$ ftp 192.168.122.149
ftp> cd /tmp
ftp> put se.txt 
local: se.txt remote: se.txt
227 Entering Passive Mode (192,168,122,149,127,146).
150 Ok to send data.
226 Transfer complete.
713 bytes sent in 2.2e-05 secs (32409.09 Kbytes/sec)
```
- SELinux 설정 변경으로 업로드 가능해짐

```bash
# VM
$ vi /etc/ssh/sshd_config # 17행 주석 제거 후 변경
Port 222

$ systemctl restart sshd
$ systemctl status sshd
● sshd.service - OpenSSH server daemon
   Loaded: loaded (/usr/lib/systemd/system/sshd.service; enabled; vendor preset: enabled)
   Active: failed (Result: exit-code) since Thu 2020-02-27 15:29:19 KST; 2s ago
...

$ cat /var/log/messages
Feb 27 15:25:07 localhost setroubleshoot: SELinux is preventing /usr/sbin/sshd from name_bind access on the tcp_socket port 222. For complete SELinux messages. run sealert -l 98bc8c51-770d-4a89-b78d-018e65050f57

$ sealert -l 98bc8c51-770d-4a89-b78d-018e65050f57 # 대충 SELinux에서 서비스별로 정한 포트가 있는데 그걸 벗어났다.
SELinux is preventing /usr/sbin/sshd from name_bind access on the tcp_socket port 222.
...

$ semanage port -l | grep ssh
ssh_port_t                     tcp      22

$ semanage port -a -t ssh_port_t -p tcp 222 # 위의 sealert 내용을 자세히 보면 이렇게 수정할 수 있다고 알려주고 있었다.

$ semanage port -l | grep ssh # 222가 추가됨
ssh_port_t                     tcp      222, 22

$ systemctl restart sshd
$ systemctl status  sshd # 정상 동작됨
● sshd.service - OpenSSH server daemon
   Loaded: loaded (/usr/lib/systemd/system/sshd.service; enabled; vendor preset: enabled)
   Active: active (running) since Thu 2020-02-27 15:34:03 KST; 6s ago
...
```

###  Permissive Domains
예외 처리
모든건 `enforcing`하고 몇가지만 `permissive` 등록

**whitelist**

```bash
$ ps -eZ | grep vsftpd
system_u:system_r:ftpd_t:s0-s0:c0.c1023 4565 ? 00:00:00 vsftpd

$ semodule -l | grep permiss
permissivedomains	(null)

$ semanage permissive -a ftpd_t
$ semodule -l | grep permiss
permissive_ftpd_t	(null)
permissivedomains	(null)
```

## Firewall

### 패키지 매니저
차후 실습에서 필요한 모듈을 다운 받기 위해 패키지 매니저에 대해서 알아본다.

#### rpm
```bash
# host

$ wget http://server1/amanda-3.3.3-17.el7.x86_64.rpm # server1은 호스트에 등록된 교육센터 공유 PC

$ rpm -Uvh amanda-3.3.3-17.el7.x86_64.rpm # 의존성 문제로 설치 불가
warning: amanda-3.3.3-17.el7.x86_64.rpm: Header V3 RSA/SHA256 Signature, key ID f4a80eb5: NOKEY
error: Failed dependencies:
	amanda-libs(x86-64) = 3.3.3-17.el7 is needed by amanda-3.3.3-17.el7.x86_64
	libamanda-3.3.3.so()(64bit) is needed by amanda-3.3.3-17.el7.x86_64
	libamar-3.3.3.so()(64bit) is needed by amanda-3.3.3-17.el7.x86_64
	perl(Amanda::Config) is needed by amanda-3.3.3-17.el7.x86_64
	perl(Amanda::Constants) is needed by amanda-3.3.3-17.el7.x86_64
	perl(Amanda::Debug) is needed by amanda-3.3.3-17.el7.x86_64
	perl(Amanda::Paths) is needed by amanda-3.3.3-17.el7.x86_64
	perl(Amanda::Util) is needed by amanda-3.3.3-17.el7.x86_64

$ wget http://server1/amanda-libs-3.3.3-17.el7.x86_64.rpm # 의존성 걸린 패키지 추가 다운

$ rpm -Uvh amanda-libs-3.3.3-17.el7.x86_64.rpm 
warning: amanda-libs-3.3.3-17.el7.x86_64.rpm: Header V3 RSA/SHA256 Signature, key ID f4a80eb5: NOKEY
Preparing...                          ################################# [100%]
Updating / installing...
   1:amanda-libs-3.3.3-17.el7         ################################# [100%]
$ rpm -Uvh amanda-3.3.3-17.el7.x86_64.rpm 
warning: amanda-3.3.3-17.el7.x86_64.rpm: Header V3 RSA/SHA256 Signature, key ID f4a80eb5: NOKEY
Preparing...                          ################################# [100%]
Updating / installing...
   1:amanda-3.3.3-17.el7              ################################# [100%]

$ rpm -qa | grep -i amanda # 설치 확인
amanda-3.3.3-17.el7.x86_64
amanda-libs-3.3.3-17.el7.x86_64

$ rpm -e amanda # 패키지 제거
$ rpm -qa | grep -i amanda # 제거 확인
amanda-libs-3.3.3-17.el7.x86_64
```

#### yum
```bash
$ yum search ssl
$ yum install mod_ssl # d:  다운만 받고 설치는 안 하는 것
Is this ok [y/d/N]: 
...

$ yum repolist # 레포 확인, status: 패키지 수
repo id                                   repo name                                                status
base                                      CentOS 7 - x86_64 - base                                 3,831

$ yum clean all # 캐싱된 레포의 패키지들 제거, 설치하지 않고 다운만 받은 패키지도 제거
$ yum repolist # 이전과 결과가 다름. 뭔가를 다운 받음

$ yum grouplist # 패키지들을 모은 그룹 확인
...
Available Environment Groups:
   Minimal Install
   Compute Node
   Infrastructure Server
   File and Print Server
   Basic Web Server
...
$ yum -y groupinstall 'File and Print Server'
Installed:
  foomatic.x86_64 0:4.0.9-8.el7  foomatic-filters.x86_64 0:4.0.9-8.el7  iprutils.x86_64 0:2.4.13.1-1.el7 
  samba.x86_64 0:4.4.4-14.el7_3  targetd.noarch 0:0.7.1-1.el7          

Dependency Installed:
  PyYAML.x86_64 0:3.10-11.el7                            foomatic-db.noarch 0:4.0-40.20130911.el7        
  foomatic-db-filesystem.noarch 0:4.0-40.20130911.el7    foomatic-db-ppds.noarch 0:4.0-40.20130911.el7   
  libyaml.x86_64 0:0.1.4-11.el7_0                        lsscsi.x86_64 0:0.27-4.el7                      
  lvm2-python-libs.x86_64 7:2.02.166-1.el7_3.5           pytalloc.x86_64 0:2.1.6-1.el7                   
  python-setproctitle.x86_64 0:1.1.6-5.el7               samba-common-tools.x86_64 0:4.4.4-14.el7_3      
  samba-libs.x86_64 0:4.4.4-14.el7_3                    

Complete!

$ yum -y remove mod_ssl

$ yum history
Loaded plugins: fastestmirror, langpacks
ID     | Login user               | Date and time    | Action(s)      | Altered
-------------------------------------------------------------------------------
     7 | root <root>              | 2020-02-27 16:27 | Erase          |    1   
     6 | root <root>              | 2020-02-27 16:26 | Install        |   16   
     5 | root <root>              | 2020-02-27 16:17 | Install        |    5  <
     4 | root <root>              | 2020-02-27 14:42 | Install        |    1 > 
     3 | root <root>              | 2020-02-25 09:41 | Install        |    2 EE
     2 | root <root>              | 2020-02-24 17:23 | Install        |   44  <
     1 | System <unset>           | 2020-02-22 11:33 | Install        | 1362 > 
history list

$ yum history info 7
Loaded plugins: fastestmirror, langpacks
Transaction ID : 7
Begin time     : Thu Feb 27 16:27:33 2020
Begin rpmdb    : 1436:c29b8b6b6f4f6f0593b54ed0218e9a6fc01f1cc6
End time       :            16:27:34 2020 (1 seconds)
End rpmdb      : 1435:713ba9e64fc9fc7f9eb26d7dff6300035351cdd3
User           : root <root>
Return-Code    : Success
Command Line   : remove mod_ssl
Transaction performed with:
    Installed     rpm-4.11.3-21.el7.x86_64                      @anaconda
    Installed     yum-3.4.3-150.el7.centos.noarch               @anaconda
    Installed     yum-plugin-fastestmirror-1.1.31-40.el7.noarch @anaconda
Packages Altered:
    Erase mod_ssl-1:2.4.6-45.el7.centos.4.x86_64 @base
history info

$ yum history undo 7 # 되돌리기
```

## Netfilter

```bash
# VM
$ systemctl stop firewalld
$ systemctl disable firewalld
Removed symlink /etc/systemd/system/dbus-org.fedoraproject.FirewallD1.service.
Removed symlink /etc/systemd/system/basic.target.wants/firewalld.service.

$ systemctl mask firewalld # firewalld 수동 시작 금지
Created symlink from /etc/systemd/system/firewalld.service to /dev/null.

$ systemctl start firewalld
Failed to start firewalld.service: Unit is masked.

$ yum -y install iptables-services

$ systemctl start iptables
ystemctl stauts iptables
Unknown operation 'stauts'.
$ systemctl status iptables
● iptables.service - IPv4 firewall with iptables
   Loaded: loaded (/usr/lib/systemd/system/iptables.service; enabled; vendor preset: disabled)
   Active: active (exited) since Thu 2020-02-27 16:40:19 KST; 18s ago
...

$ systemctl enable iptables # 부팅 시 자동 시작
Created symlink from /etc/systemd/system/basic.target.wants/iptables.service to /usr/lib/systemd/system/iptables.service.
```

### iptables
명령어
- `A`: append, 맨 밑에 추가
- `I`: input, 특정 위치에 룰을 추가
- `D`: delete,

```bash
# VM
$ iptables -nL
Chain INPUT (policy ACCEPT)
target     prot opt source               destination         
ACCEPT     all  --  0.0.0.0/0            0.0.0.0/0            state RELATED,ESTABLISHED
ACCEPT     icmp --  0.0.0.0/0            0.0.0.0/0           
ACCEPT     all  --  0.0.0.0/0            0.0.0.0/0           
ACCEPT     tcp  --  0.0.0.0/0            0.0.0.0/0            state NEW tcp dpt:22
REJECT     all  --  0.0.0.0/0            0.0.0.0/0            reject-with icmp-host-prohibited

Chain FORWARD (policy ACCEPT)
target     prot opt source               destination         
REJECT     all  --  0.0.0.0/0            0.0.0.0/0            reject-with icmp-host-prohibited

Chain OUTPUT (policy ACCEPT)
target     prot opt source               destination         
```
- INPUT: **중요**
- OUTPUT: 대부분 all open
- FORWARD: 네트워트 서버가 아닌 이상 중요하지 않음

```bash
$ iptables -nL --line-number # 룰은 순서가 중요하기 때문에 순서 확인
$ iptables -D INPUT 4 # 4번째 룰 삭제
$ iptables -F INPUT # {}체인의 모든 룰 삭제
# $ iptables -F # 체인 상관없이 모든 룰 삭제
```

#### Targets
- `DROP`: 차단 후 아무것도 안함
- `REJECT`: 차단 후 메세지를 보냄 (잘 안씀, 굳이 메세지를 보낼 필요가 없음)
- `ACCEPT` : 허용


```bash
# VM
$ iptables -nL
Chain INPUT (policy DROP)
target     prot opt source               destination      
```
```bash
# host
$ ping -c 3 192.168.122.149 # ping 불가 VM의 방화벽 INPUT이 DROP이기 때문
PING 192.168.122.149 (192.168.122.149) 56(84) bytes of data.
3 packets transmitted, 0 received, 100% packet loss, time 1999ms
```
```bash
# VM
$ iptables -A INPUT -p icmp --icmp-type echo-request -j ACCEPT # icmp 허용
$ iptables -nL
Chain INPUT (policy DROP)
target     prot opt source               destination         
ACCEPT     icmp --  0.0.0.0/0            0.0.0.0/0            icmptype 8
```
```bash
# host
$ ping -c 3 192.168.122.149 # 위에서 icmp 예외 처리를 했디 때문에 통신가능
3 packets transmitted, 3 received, 0% packet loss, time 1999ms
```
```bash
# VM
# OUTPUT 체인은 전체 허용이라 나가는 것이 가능해서 나갔는데 돌아온 응답은 icmp 만 허용하기 때문에 DROP해서 통신 불가
# 이러한 경우 일일이 INPUT 체인을 넣어주긴 힘들다다
$ ping -c 3 192.168.122.1 # 불가
PING 192.168.122.1 (192.168.122.1) 56(84) bytes of data.
$ ssh root@192.168.122.1 # 불가

## 해결법, connection tracking 사용
$ iptables -I INPUT 1 -m state --state ESTABLISHED,RELATED -j ACCEPT
$ iptables -nL
Chain INPUT (policy DROP)
target     prot opt source               destination         
ACCEPT     all  --  0.0.0.0/0            0.0.0.0/0            state RELATED,ESTABLISHED
ACCEPT     icmp --  0.0.0.0/0            0.0.0.0/0            icmptype 8

$ ping -c 3 192.168.122.1 # 가능
$ ssh root@192.168.122.1 # 가능

$ iptables -A INPUT -p tcp --dport 80 -j ACCEPT
```
- connection tracking, 상태 추적 기능이 필요
	- INPUT으로 나갔는데 돌아올 때 못 들어오는 경우를 방지하기 위한 기능
	- `NEW`: 새롭게 연결을 맺기 위해 들어오는
	- `INVALID`: 상태를 알 수 없는
	- `ESTABLISHED`: **(중요)** 밖으로 나갔다가 들어오는 
	- `RELATED`: **(중요)** 2개 이상의 포트를 사용하는 서비스 (ftp 20,21)에서 INPUT, OUTPUT 포트가 다른 경우

``` bash
$ yum search xtable # 현재 레포에는 해당 패키지가 없다.
$ yum search dbench # 동일하게 없다.

# /etc/yum.repos.d/epel 에 epel 이 추가된다.
$ yum install https://dl.fedoraproject.org
/pub/epel/epel-release-latest-7.noarch.rpm

$ yum clean all # 캐시 제거
$ yum repolist # 레포 설정 파일로 레포 등록

$ yum search dbench # epel에서 제공하기 때문에 나온다
$ yum search xtable # epel에서도 제공하지 않기 때문에 안 나온다.

$ yum install repo.iotti.biz/CentOS/7/noarch/lux-release-7-1.noarch.rpm
$ yum clean all
$ yum repolist
$ yum search xtable # 이제 검색된다? 안된다!

```
- `/etc/yum.repos.d/` 디렉토리의 `.repo` 확장자를 갖는 파일이 레포의 설정 파일이다.
- https://fedoraproject.org/wiki/EPEL
- http://repo.iotti.biz/CentOS/7/noarch/


# 컨테이너를 위반 기반 기술

https://hub.docker.com

### Dockerfile
https://github.com/docker-library/httpd/blob/4faace97468d7bced1ac4a072f89f151359ee9fa/2.4/Dockerfile

## chroot
> 특정 디렉토리를 최상위 `/`처럼 보이게 변경

```bash
# host
$ mkdir -p ~/newroot/bin
$ mkdir -p ~/newroot/lib64

$ chroot ~/newroot /bin/bash # 참조 라이브러리가 없어서 오류 발생
chroot: failed to run command ‘/bin/bash’: No such file or directory

$ ldd /bin/bash # 명령어가 참조하는 라이브러리 확인
	linux-vdso.so.1 =>  (0x00007ffc1ffdc000)
	libtinfo.so.5 => /lib64/libtinfo.so.5 (0x00007fa702bd9000)
	libdl.so.2 => /lib64/libdl.so.2 (0x00007fa7029d5000)
	libc.so.6 => /lib64/libc.so.6 (0x00007fa702613000)
	/lib64/ld-linux-x86-64.so.2 (0x00007fa702e17000)

# bash 명령에 필요한 라이브러리들 복사
$ cp /lib64/libtinfo.so.5 ~/newroot/lib64
$ cp /lib64/libdl.so.2  ~/newroot/lib64
$ cp /lib64/libc.so.6 ~/newroot/lib64
$ cp /lib64/ld-linux-x86-64.so.2  ~/newroot/lib64
$ cp /bin/bash ~/newroot/bin

$ chroot ~/newroot /bin/bash
bash-4.2# pwd
/
bash-4.2# ls
bash: ls: command not found # ls에 관련된 라이브러리는 복사하지 않았기 때문에 명령어 오류
```

```bash
# host
$ mkdir /tmp/ram
$ cd /tmp/ram

$ /lib/dracut/skipcpio /boot/initramfs-3.10.0-514.26.1.el7.x86_64.img  | zcat | cpio -icdmu # initramfs-3.10.0-514.26.1.el7.x86_64.img = 램디스크
145044 blocks

$ ls /tmp/ram
bin  dev  etc  init  lib  lib64  proc  root  run  sbin  shutdown  sys  sysroot  tmp  usr  var
```
- 복구 모드에서 하드 디스크 대신 램 디스트로 부팅됨
- `sysroot`: 실제 하드 디스크가 마운트되는 위치
	- `chroot sysroot` : 램 디스크 부팅 환경에서 오류 해결을 위해 실제 하드 디스크를 `root`로 보이게 한다

#### 장애 상황 구현으로 chroot
1.  VM 재부팅
2. e 누름
![부팅시 e](https://user-images.githubusercontent.com/9030565/75500970-e5871300-5a11-11ea-9695-c63003868968.png)
3. `rd.break` 추가 
UTF-8 뒤에 한 칸 띄고 추가
![rd break 추가](https://user-images.githubusercontent.com/9030565/75500972-e750d680-5a11-11ea-8e85-236204913d80.png)
5. ctrl + x
![램디스크로 부팅](https://user-images.githubusercontent.com/9030565/75500981-ed46b780-5a11-11ea-919f-27c4453b9d59.png)
6. `switch_root>` 진입
```bash
# 장애 상황
# VM

VM 재부팅
e 누름
`linux16 ~ UTF-8` rd.break 
ctrl + x 누름

switch_root> pwd
/
switch_root> ls

switch_root> mount -o remount,rw /sysroot # /sysroot 변경 가능하게 

switch_root> chroot /sysroot # 실제 하드디스크로 root 변경
sh-4.2> ls
sh-4.2> passwd # hak
sh-4.2> touch /.autorelabel # 반드시 해줘야함. 뭔가 바뀌었다는 것을 알려주는 용도
sh-4.2> exit # chroot 빠져나옴
switch_root> exit # 램 디스트 빠져나옴

# 다시 부팅되고 root 비밀번호 바뀐 것을 확인할 수 있다
```
- `rd.break`: 램 디스크에서 정지 시키고 부팅
	- 검은 화면으로 나온다 (스샷)


## 네임스페이스
> 프로세스 격리

- `clone()`
- `unshare()`
- `setna()`

```bash
$ unshare -f -p --mount-proc /bin/sh
sh-4.2> ps -ef # 프로세스를 격리했기 때문에 아래 두개만 나옴. 또한 pid 1이 /bin/sh
UID        PID  PPID  C STIME TTY          TIME CMD
root         1     0  0 10:32 pts/0    00:00:00 /bin/sh
root         3     1  0 10:33 pts/0    00:00:00 ps -ef
sh-4.2> exit

unshare -f /bin/sh
sh-4.2> ps -ef # 위에서의 결과와 다름, 전체가 나옴
UID        PID  PPID  C STIME TTY          TIME CMD
root         1     0  0 10:01 ?        00:00:01 /usr/lib/systemd/systemd --switched-root --system --deserialize 
root         2     0  0 10:01 ?        00:00:00 [kthreadd]
root         3     2  0 10:01 ?        00:00:00 [ksoftirqd/0]
root         6     2  0 10:01 ?        00:00:00 [kworker/u2:0]
root         7     2  0 10:01 ?        00:00:00 [migration/0]
root         8     2  0 10:01 ?        00:00:00 [rcu_bh]
root         9     2  0 10:01 ?        00:00:00 [rcu_sched]
root        10     2  0 10:01 ?        00:00:00 [watchdog/0]
...

```

### PID Namespace
```bash
$ pstree
```
- kernel: pid 0
- systemd: pid 1 (CentOS 7 이전엔 `init`)


### MNT Namespace
Persistent Volumes(PV), https://kubernetes.io/docs/concepts/storage/persistent-volumes


### UTS Namespace
```bash
# host

$ unshare -u /bin/bash

$ echo $$ # bash pid 확인
10511

$ hostname # 현재 호스트
station14.example.com

$ ll /proc/$$/ns/uts
lrwxrwxrwx. 1 root root 0 Feb 28 10:49 /proc/10511/ns/uts -> uts:[4026532463]

$ hostname happy.test.com # 현재 별도의 격리된 쉘
$ hostname
happy.test.com

$ exit # 격리된 쉘에서 나오기
exit

$ hostname
station14.example.com
```
- unshare
	- `-u`: UTS

### IPC Namespace

```bash
# host
$ top
  PID USER      PR  NI    VIRT    RES    SHR S  %CPU %MEM     TIME+ COMMAND             

$ unshare -i /bin/bash # bash 쉘이 격리됨
$ ipcmk -M 100 # 100byte 공유 메모리 생성
Shared memory id: 0

$ vipcs -m # 격리되었기 때문에 1개의 공유 메모리만 나온다.
------ Shared Memory Segments --------
key        shmid      owner      perms      bytes      nattch     status      
0x3882bc4a 0          root       644        100        0

## 새 터미널 생성
$ ipcmk -M 200
Shared memory id: 15433771
$ ipcs -m # 격리하지 않았기 때문에 모든 공유 메모리가 조회된다.
------ Shared Memory Segments --------
key        shmid      owner      perms      bytes      nattch     status      
0x00000000 196608     root       600        524288     2          dest         
0x00000000 229377     root       600        4194304    2          dest         
...

```
- top
	- `SHR`: 여러 프로세스들이 공유해서 사용하는 영역


### USER Namespace
```bash
# host

$ grep CONFIG_USER_NS /boot/config-$(uname -r)
CONFIG_USER_NS=y
```

### NET Namespace

```bash
# 실습이 아님 
docker run -d -p 3000:80 도커이미지
```

```bash
# host
$ ip netns add guestnet
$ ip netns exec guestnet ip link
1: lo: <LOOPBACK> mtu 65536 qdisc noop state DOWN mode DEFAULT qlen 1
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
$ ip netns exec guestnet ip link set lo up
$ ip netns exec guestnet ip link
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN mode DEFAULT qlen 1
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00

$ ip link add host type veth peer name guest
$ ip link
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN mode DEFAULT qlen 1
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
2: enp2s0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP mode DEFAULT qlen 1000
    link/ether 34:64:a9:0d:81:e7 brd ff:ff:ff:ff:ff:ff
3: virbr0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP mode DEFAULT qlen 1000
    link/ether 52:54:00:cf:90:84 brd ff:ff:ff:ff:ff:ff
4: virbr0-nic: <BROADCAST,MULTICAST> mtu 1500 qdisc pfifo_fast master virbr0 state DOWN mode DEFAULT qlen 1000
    link/ether 52:54:00:cf:90:84 brd ff:ff:ff:ff:ff:ff
13: vnet0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast master virbr0 state UNKNOWN mode DEFAULT qlen 1000
    link/ether fe:54:00:a4:a9:a4 brd ff:ff:ff:ff:ff:ff
14: guest@host: <BROADCAST,MULTICAST,M-DOWN> mtu 1500 qdisc noop state DOWN mode DEFAULT qlen 1000
    link/ether da:4a:16:80:c4:0e brd ff:ff:ff:ff:ff:ff
15: host@guest: <BROADCAST,MULTICAST,M-DOWN> mtu 1500 qdisc noop state DOWN mode DEFAULT qlen 1000
    link/ether 36:a4:88:1c:fb:ab brd ff:ff:ff:ff:ff:ff
$ ip link set guest netns guestnet
$ ip link
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN mode DEFAULT qlen 1
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
2: enp2s0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP mode DEFAULT qlen 1000
    link/ether 34:64:a9:0d:81:e7 brd ff:ff:ff:ff:ff:ff
3: virbr0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP mode DEFAULT qlen 1000
    link/ether 52:54:00:cf:90:84 brd ff:ff:ff:ff:ff:ff
4: virbr0-nic: <BROADCAST,MULTICAST> mtu 1500 qdisc pfifo_fast master virbr0 state DOWN mode DEFAULT qlen 1000
    link/ether 52:54:00:cf:90:84 brd ff:ff:ff:ff:ff:ff
13: vnet0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast master virbr0 state UNKNOWN mode DEFAULT qlen 1000
    link/ether fe:54:00:a4:a9:a4 brd ff:ff:ff:ff:ff:ff
15: host@if14: <BROADCAST,MULTICAST> mtu 1500 qdisc noop state DOWN mode DEFAULT qlen 1000
    link/ether 36:a4:88:1c:fb:ab brd ff:ff:ff:ff:ff:ff link-netnsid 0
$ ip netns exec guestnet ip link
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN mode DEFAULT qlen 1
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
14: guest@if15: <BROADCAST,MULTICAST> mtu 1500 qdisc noop state DOWN mode DEFAULT qlen 1000
    link/ether da:4a:16:80:c4:0e brd ff:ff:ff:ff:ff:ff link-netnsid 0

$ ip link set host up
$ ip address add 1.1.1.1/24 dev host
$ ip address
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN qlen 1
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host 
       valid_lft forever preferred_lft forever
2: enp2s0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP qlen 1000
    link/ether 34:64:a9:0d:81:e7 brd ff:ff:ff:ff:ff:ff
    inet 59.29.224.192/24 brd 59.29.224.255 scope global dynamic enp2s0
       valid_lft 282151sec preferred_lft 282151sec
    inet6 fe80::3664:a9ff:fe0d:81e7/64 scope link 
       valid_lft forever preferred_lft forever
3: virbr0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP qlen 1000
    link/ether 52:54:00:cf:90:84 brd ff:ff:ff:ff:ff:ff
    inet 192.168.122.1/24 brd 192.168.122.255 scope global virbr0
       valid_lft forever preferred_lft forever
4: virbr0-nic: <BROADCAST,MULTICAST> mtu 1500 qdisc pfifo_fast master virbr0 state DOWN qlen 1000
    link/ether 52:54:00:cf:90:84 brd ff:ff:ff:ff:ff:ff
13: vnet0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast master virbr0 state UNKNOWN qlen 1000
    link/ether fe:54:00:a4:a9:a4 brd ff:ff:ff:ff:ff:ff
    inet6 fe80::fc54:ff:fea4:a9a4/64 scope link 
       valid_lft forever preferred_lft forever
15: host@if14: <NO-CARRIER,BROADCAST,MULTICAST,UP> mtu 1500 qdisc noqueue state LOWERLAYERDOWN qlen 1000
    link/ether 36:a4:88:1c:fb:ab brd ff:ff:ff:ff:ff:ff link-netnsid 0
    inet 1.1.1.1/24 scope global host
       valid_lft forever preferred_lft forever

$ ip netns exec guestnet ip link set guest up
$ ip netns exec guestnet ip address add 1.1.1.1/24 dev guest
$ ip netns exec guestnet ip add show dev guest
14: guest@if15: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP qlen 1000
    link/ether da:4a:16:80:c4:0e brd ff:ff:ff:ff:ff:ff link-netnsid 0
    inet 1.1.1.1/24 scope global guest
       valid_lft forever preferred_lft forever
    inet6 fe80::d84a:16ff:fe80:c40e/64 scope link 
       valid_lft forever preferred_lft forever

$ ping 1.1.1.2 # 통신 되야함. 실패

## 실습 실패
```

## cgroup
```bash
cat /proc/cgroups 
#subsys_name	hierarchy	num_cgroups	enabled
cpuset	2	5	1
cpu	5	7	1
cpuacct	5	7	1
memory	7	5	1
devices	4	101	1
freezer	8	3	1
net_cls	3	3	1
blkio	6	5	1
perf_event	10	3	1
hugetlb	11	1	1
pids	9	1	1
net_prio	3	3	1

$ mkdir -p /cgroup/cpu
$ mount -t cgroup -o cpu,cpuacct cpu,cpuacct, /cgroup/cpu/
$ ls -l /cgroup/cpu/
...
-rw-r--r--. 1 root root 0 Feb 24 17:42 tasks
...

## 실습 중단.
```

## AUFS
> 하이퍼바이저에 의해 가상화된 VM은 OS 뿐만 아니라 VM에 필요한 서비스들까지 모두 포함
> AUFS는 레이어 개념을 통해서 다양한 OS 및 서비스들응 각각 읽기 전용 이미지 레이어로 구성하며, 새로운 파일이 추가되거나 수정되면 레이어를 얻는 방식

> 도커 명령어 하나 마다 레이어가 생성됨
> ```bash
> FROM debian~
> RUN groupadd~
> RUN apt-get~
> RUN set -x  \
>   && ~
>   && ~ 
> ```
>  - FROM, RUN, RUN 하나 마다 각각의 레이어가 추가됨
>  -  따라서 레이어가 많이 추가되는 것을 방지하기 위해 `&&`로 하나의 명령어로 사용한다.

### OverlayFS
`/var/lib/docker/overlay2`
> 커널 3.18 부터 지원
> 두 개의 레이어만 갖고 있어 AUFS 보단 단순

# 리눅스 자원 모니터링

#### vmstat
https://zetawiki.com/wiki/리눅스_vmstat
```bash
$ vmstat
procs -----------memory---------- ---swap-- -----io---- -system-- ------cpu-----
 r  b   swpd   free   buff  cache   si   so    bi    bo   in   cs us sy id wa st
 0  0 523828 986544      0 1229548    0    0     8    22   27   10  2  0 97  0  0
 
$ vmstat 3 5 # 3초마다 5번 출력
```
- `r`: 실행 대기 중인 프로세스의 평균 개수
- `b`: 블럭된 프로세스의 개수
- `wa`:

#### iostat
```bash
$ iostat
Linux 3.10.0-514.26.1.el7.x86_64 (station14.example.com) 	02/28/2020 	_x86_64_	(4 CPU)

avg-cpu:  %user   %nice %system %iowait  %steal   %idle
           2.05    0.05    0.34    0.29    0.00   97.27

Device:            tps    kB_read/s    kB_wrtn/s    kB_read    kB_wrtn
sda               4.25        31.94        89.22   10393734   29030024
dm-0              2.19         7.26        30.30    2363600    9858052
dm-1              0.59         0.45         1.89     147608     615316
dm-2              0.10         0.02         1.26       5560     409944
dm-3              0.23         1.08         6.66     352180    2167688

$ iostat -xt 3 2 # 3초 간격 2번
```

#### mpstat
```bash
$ mpstat
Linux 3.10.0-514.26.1.el7.x86_64 (station14.example.com) 	02/28/2020 	_x86_64_	(4 CPU)

12:30:54 PM  CPU    %usr   %nice    %sys %iowait    %irq   %soft  %steal  %guest  %gnice   %idle
12:30:54 PM  all    1.84    0.05    0.34    0.29    0.00    0.00    0.00    0.23    0.00   97.25

$ mpstat -P ALL 5 2 # 5초 간격으로 2번
```

#### sar
```bash
$ ll /var/log/sa
$ file /var/log/sa/*
/var/log/sa/sa21:  data
/var/log/sa/sa23:  data
/var/log/sa/sa24:  data
/var/log/sa/sa25:  data
/var/log/sa/sa26:  data
/var/log/sa/sa27:  data
/var/log/sa/sa28:  data
/var/log/sa/sar24: ASCII text
/var/log/sa/sar25: ASCII text
/var/log/sa/sar26: ASCII text
/var/log/sa/sar27: ASCII text

$ sar -u 1 3 # ALL
Linux 3.10.0-514.26.1.el7.x86_64 (station14.example.com) 	02/28/2020 	_x86_64_	(4 CPU)

12:41:22 PM     CPU     %user     %nice   %system   %iowait    %steal     %idle
12:41:23 PM     all      1.26      0.00      0.00      0.00      0.00     98.74
12:41:24 PM     all      2.78      0.00      0.25      0.25      0.00     96.71
12:41:25 PM     all      4.51      0.00      0.50      0.00      0.00     94.99
Average:        all      2.86      0.00      0.25      0.08      0.00     96.81

$ sar -q 1 3 # 큐 로드 평균
Linux 3.10.0-514.26.1.el7.x86_64 (station14.example.com) 	02/28/2020 	_x86_64_	(4 CPU)

12:41:39 PM   runq-sz  plist-sz   ldavg-1   ldavg-5  ldavg-15   blocked
12:41:40 PM         0       737      0.45      0.56      0.42         0
12:41:41 PM         0       737      0.45      0.56      0.42         0
12:41:42 PM         0       737      0.45      0.56      0.42         0
Average:            0       737      0.45      0.56      0.42         0

$ sar -r 1 3 # 메모리
Linux 3.10.0-514.26.1.el7.x86_64 (station14.example.com) 	02/28/2020 	_x86_64_	(4 CPU)

12:42:50 PM kbmemfree kbmemused  %memused kbbuffers  kbcached  kbcommit   %commit  kbactive   kbinact   kbdirty
12:42:51 PM   1481568   6620988     81.71         0   1021200  11951652    138.54   4212952   1979528       844
12:42:52 PM   1482044   6620512     81.71         0   1021368  11951652    138.54   4212840   1979724       844
12:42:53 PM   1482292   6620264     81.71         0   1021128  11951640    138.54   4212520   1979484       844
Average:      1481968   6620588     81.71         0   1021232  11951648    138.54   4212771   1979579       844

```
- `-q`
	- `runq-sz`: 실행을 위해 대기 큐에 쌓인 프로세스


# 쉘 프로그램
```bash
$ vi 1.sh
$ ./1.sh # 실행 권한이 없어서 불가
bash: ./1.sh: Permission denied

$ ./1.sh
```

쉘 스크립트에 데이터를 전달하는 방법
1. 환경 변수 이용 (로컬 변수 X)
2. argument
3. 입력(직접)

```bash
$ vi 2.sh
echo my age is $myage
echo my hair color is $myhaircolor

$ chmod a+x 2.sh
$ myage=10
$ myhaircolor=red
$ ./2.sh # 로컬 변수는 쉘 스크립트에 전달되지 않음
my age is
my hair color is

$ export myage=10
$ ./2.sh 
my age is 10
my hair color is
```

```bash

$ variable.sh
#!/bin/sh
echo "The script name is: $0"
echo "The 1 argument passed is: $1"
echo "The 2 argument passed is: $2"
echo "The 3 argument passed is: $#"
echo "The 4 argument passed is: $@"

$ chmod +x variable.sh

$ ./variable.sh 1.txt aaa hp ibm rain final
The script name is: ./variable.sh
The 1 argument passed is: 1.txt
The 2 argument passed is: aaa
The 3 argument passed is: 6
The 4 argument passed is: 1.txt aaa hp ibm rain final

```
- `#!/bin/sh`: 쉘 스크립트의 첫줄의 2byte가 `#!`이면 뒤에 나오는 쉘로 명령어 실행. 없다면 터미널 기본 쉘로 실행
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTI4MDcwODI0Nl19
-->