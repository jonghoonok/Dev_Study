# Operating System

서버에 설치되는 OS의 종류와 기본 원리





## 목차

[TOC]



## **1. Linux**





### 1.1. UNIX/Linux의 기본 구성 





리눅스와 파일

- **모든 것은 파일**이라는 철학을 따름
  - 모든 interaction은 파일을 읽고 쓰듯이 이루어 짐
  - 마우스, 키보드 등 모든 디바이스 관련 기술도 파일처럼 취급됨
- 파일 네임스페이스
  - 전역 **네임스페이스**를 제공함
    - 리눅스는 모든 파일이 root directory에 속함(`/media/floofy/jonghoon.jpg`)
    - 윈도우의 경우 A 드라이브, C 드라이브 등이 나뉘어 있음
- 파일은 inode 고유값과 자료구조에 의해 관리됨





리눅스와 프로세스

- 프로세스의 구조
  - 리눅스 실행 파일 포맷 ELF(Executable and Linkable Format)
  - 콜스택, 코드(텍스트), 데이터 및 BSS 섹션 등
- 다양한 시스템 리소스와 관련됨
  - 프로세스 내에서 **시스템 콜 호출을 통해 리소스를 처리**할 수 있도록 구성됨
  - 타이머, 시그널, 파일, 네트워크, 디바이스, IPC 등
- 가상 메모리 지원
- 각 프로세스는 pid(프로세스 ID) 고유값으로 구분됨
- 신규 프로세스의 생성
  - init 프로세스(첫 번째 프로세스)를 기반으로 `fork()` 시스템콜을 사용해 생성





리눅스와 권한

- 리눅스는 사용자/그룹으로 권한을 관리함
  - 운영체제는 사용자/리소스 권한을 관리함
  - 로그인 사용자, 사용자를 grouping한 그룹으로 관리할 수 있음
  - root는 슈퍼 관리자
- **파일마다** 권한을 관리함
  - 소유자, 소유자 그룹, 모든 사용자에 대해 실행, 읽기, 쓰기 권한 관리
  - 접근 권한 정보는 **inode 자료구조**에 저장됨





### 1.2. 쉘 사용법





Shell이란?

- 사용자와-하드웨어 or 사용자-운영체제 간 인터페이스
  - 사용자의 명령을 해석해서 **커널에 명령을 요청**
  - 관련된 시스템 콜을 사용하도록 프로그래밍 되어 있음





쉘의 종류

- Korn Shell(ksh)
  - 유닉스에서 가장 많이 사용됨
- **Bourne-Again Shell**(bash)
  - GNU 프로젝트의 일환으로 개발된 리눅스 용 쉘
  - 콘 쉘을 기반으로 개발됨
- C Shell(csh)
- Bourne Shell(sh)





#### Bash의 기본 명령어





Help

- 명령어 앞에 `man`을 치면 해당 명령어의 옵션들을 보여줌
  - 예) `man ls` 하면 ls 뒤에 올 수 있는 옵션들을 출력
  - manual이라는 뜻





다중 사용자 관련

- `whoami`
  - 로그인한 사용자 ID를 알려줌
- `passwd`
  - 로그인한 사용자 ID의 암호를 변경
  - `passwd username` 입력 → 새로운 암호를 입력해서 변경 가능
- 신규 유저 추가
  - 신규 유저/그룹는 **root**만(super user) 추가할 수 있음
    - root로 로그인하거나
    - `sudo` 명령어를 이용해 root 권한으로 실행
  - `useradd`
    - 기본 설정을 자동으로 하지 않음
    - `useradd -N -n UID username`
  - `adduser`
    - 기본 설정을 자동으로 수행함
- `su`
  - 사용자를 변경함
  - `su ID ` : 현재 사용자의 **환경설정** 기반으로 ID로 전환
    - `.profile`을 참조함
  - `su - ID` : 변경되는 사용자의 환경설정 기반으로 ID로 전환
- 유저 목록 확인
  - `grep username /etc/passwd`
  - **grep** : 입력으로 전달된 파일의 내용에서 특정 문자열을 찾고자할 때 사용하는 명령어
    - `/etc/passwd` 경로에 해당 username이 있는지 검색하는 것





그룹 관련

- 그룹 생성
  - `groupadd` : useradd와 같은 방식으로 그룹을 생성
- 그룹에 유저 추가
  - `gpasswd -a groupname username` : groupname에 username유저를 추가
- 그룹 목록 확인
  - `grep groupname /etc/group`





디렉토리/파일 관련

- `pwd`

  - 현재 디렉토리의 위치를 보여줌

- `cd`

  - 디렉토리 이동
  - 예) `cd /etc/` : etc 디렉토리로 이동하기
  - `cd ~` : 현재 로그인한 아이디의 home directory로 이동

- `ls` or `dir`

  - 현재 디렉토리에 있는 파일 목록을 출력

  - 뒤에 `-al`을 붙이면 숨김 파일, 시스템 파일(`.`으로 시작)까지 모두 볼 수 있음

    - **inode 정보도 출력**함

    - 파일 별 소유자, 소유자 그룹, 모든 사용자에 대해 읽고 쓰고 실행하는 권한
    - `-rwxr-xr-x 1 root root 120 Jul 19 19:28 debian-start`
    - 소유자의 권한+그룹의 권한+기타 사용자의 권한
    - r : 읽기, w : 쓰기, x : 실행
    - 맨 앞의 `-`는 파일임을 나타냄(`d`면 디렉토리라는 뜻)

  - 와일드 카드

    - 정규 표현식과 같음
    - `*`는 임의 문자열
    - `?`는 문자 하나
    - `ls host*` 하면 host로 시작하는 파일 목록을 출력, `ls host?`하면 host(한글자) 파일 목록 출력

- 파일 보기

  - `cat file` : 터미널에서 바로 파일 내용을 **볼 수만 있음**
    - `vi file` 하게 되면 vi에서 열고(**편집 가능**) cat을 사용하면 터미널에서 열게 됨
  - `head file` : 파일의 앞 부분을 볼 수 있음(default 10줄)
  - `tail file` : 파일의 끝 부분을 볼 수 있음(default 10줄)

- `rm file`

  - 해당 파일을 삭제
  - `rm -rf` 하게되면 하위 디렉토리를 포함한 모든 파일을(r) 강제로(f) 삭제함





권한 관련

- `sudo`
  - root 계정으로 로그인하지 않은 상태에서 **root 권한이 필요한 명령어를 실행**할 수 있게 함
  - 어떤 아이디에서 sudo를 사용하려면 해당 아이디가 **sudo를 사용할 수 있게 설정**되어 있어야 함
    - `/etc/sudoers` 설정 파일에서 설정을 변경 가능
  - 시스템에 영향이 가는 중요한 명령을 수행할 때 root로 로그인하기보다 sudo를 사용하는 것이 일반적
    - 주의를 환기하기 위해서?
- `chmod`
  - **파일 권한을 변경**함
  - 누구에게 + 주는지/뺏는지 + 무슨 권한을
    - 문자를 사용하는 방법
    - 숫자를 사용하는 방법 : r=4, w=2, x=1로 변환하여 부여할 수 있음
    - `chmod ug+rw test.c`  : test.c 파일에 대해 user와 group에게 읽기/쓰기 권한을 추가(+)
    - `chmod u=rwx, g=rw, o=rx` : user는 rwx로, group은 rw로, 기타 사용자는 rx로 설정
    - `chmod 400 mysecurity.pem`  : `r--------`로 설정함(소유자만 읽을 수 있음)
    - `chmod -R 777 directory` : 디렉토리 내 모든 파일에 대해(-Recursive) 전부 rwx 가능하게 변경
- 소유자 변경
  - `chown` : 소유하는 사용자를 변경
    - `chown [option][user:group][file]`
  - `chgrp` : 소유하는 그룹을 변경





#### Linux Redirection, Pipe





Standard Stream(표준 입출력)

- command로 실행되는 프로세스는 3가지 스트림을 갖고 있음
  - Standard Input Stream - **`stdin`**
  - Standard Output Stream - **`stdout`**
  - Standard Error Stream - **`stderr`**
- 모든 스트림은 일반적인 plain text로 console에 출력하도록 되어 있음





리다이렉션이란?

- 표준 스트림의 흐름을 다른 쪽으로 바꿀 수 있음
  - `>, <` 기호를 사용함
  - 주로 **명령어 표준 출력을 화면이 아닌 파일에 쓸 때** 사용
- 예시
  - `ls > files.txt`
    - ls로 출력되는 표준 **출력 스트림의 방향을 파일로** 바꿈
    - 파일에 ls로 출력되는 결과가 저장됨
  - `head < files.txt`
    - files 내용이 head라는 파일의 처음부터 10라인까지 출력해주는 명령으로 넣어짐
    - files.txt의 앞 10라인이 console에 출력되는 것
  - `head < files.txt > files2.txt`
    - files.txt의 내용이 head로 들어가서 첫 10라인이 출력
    - 되어야 하는데 그 출력 스트림이 다시 files2.txt로 들어가서 저장됨
    - **리다이렉션은 앞에서부터 순서대로** 이루어짐
- 기존 파일에 추가할 경우
  - `>>, <<` 를 사용함
  - `ls >> files.txt` 기존에 있는 files.txt **파일 뒤에 ls의 출력 결과가 추가**됨





파이프란?

- 두 프로세스 사이에서 **한 프로세스의 출력 스트림을 또 다른 프로세스의 입력 스트림으로** 사용할 때
  - `|` 기호를 사용함
  - UNIX 철학이 반영된 것
    - **프로세스는 단순하게** 하고 이를 **여러 개 엮어서 원하는 기능을 실행**하는 것
- 예시
  - `ls | grep files`
  - ls 명령의 출력 내용이 **grep 명령**의(찾기 명령) 입력 스트림으로 들어감
    - grep files는 grep 명령의 입력 스트림을 검색해 files가 들어있는 입력 내용만 출력함
    - 따라서 ls 명령으로 나타낼 파일/디렉토리 중에 files가 포함된 것을 출력함





#### 프로세스 관련





프로세스 vs 바이너리

- 바이너리는(코드 이미지) **실행 파일**
- 프로세스는 **실행 중인 프로그램**
  - 실행 파일에 대한 정보에 추가로 실행 관련 정보를 가짐
    - 가상 메모리 및 물리 메모리 정보
    - 시스템 리소스 관련 정보
    - 스케줄링 단위





리눅스의 프로세스 실행 환경

- UNIX 철학
  - 한 가지 프로그램은 한 가지 기능에만 충실할 것
  - 여러 프로그램이 각자의 일을 수행해 전체 시스템이 동작하여 사용자가 원하는 기능을 제공
- 리눅스에는 기본적으로 다양한 프로세스가 실행됨
  - **프로세스를 제어/관리하는 명령어가 발달**되어 있음





foreground, background 프로세스

- foreground process
  - shell에서 실행을 명령한 후 해당 **프로세스 수행 종료까지 사용자가 다른 입력을 하지 못함**
- background process
  - 사용자 입력과 상관 없이 실행되는 프로세스(**뒤에서 실행**)
  - shell에서 해당 프로세스 실행 시 맨 뒤에 `&`를 붙여줌
  - 예) `find / -name '*.py' > list.txt &`
    - .py로 끝나는 파일을 찾아서 그 결과를 list.txt에 저장해라
    - 일반적으로 탐색은 오래걸리기 때문에 background process로 실행
    - 실행하면 job number와 pid가 표시됨





foreground 프로세스의 제어

- 프로세스 **일시 중지**
  - Ctrl + Z : (suspended 상태)로 만듦
    - 콘솔에 job number + 상태 가 출력됨
  - `bg` 명령어
    - 직전에 Stop된 프로세스를 실행시킴
    - bg + job number로 해당 프로세스를 다시 실행할 수 있음
  - `jobs` 명령어
    - 백그라운드로 진행 또는 중지된 상태로 있는 프로세스를 보여줌
- 프로세스 **강제 종료**
  - Ctrl + C : 해당 프로세스를 완전히 종료
  - 운영체제 **소프트웨어 인터럽트**가 해당 프로세스에 보내짐





프로세스 상태 확인

- `ps` 명령어로 확인 가능
- option
  - `-a` : 모든 사용자의 프로세스 출력
  - `-u` : 프로세스 소유자에 대한 상세 정보 출력
  - `-l` : 프로세스 관련 상세 정보 출력
  - `-x` : 터미널에 로그인한 후 실행한 프로세스가 아닌 프로세스도 출력
    - 주로 **daemon process**를 확인하기 위해 사용됨
    - 기본적으로 ps 명령은 현재 shell에서 실행한 프로세스들만 보여주기 때문
  - `-e` : 해당 프로세스와 관련된 환경 변수 정보를 함께 출력
  - `-f` : 프로세스 간 관계 정보도 함께 출력
  - 이 외에도 있지만 생략
- Daemon process
  - 사용자 모르게 **시스템 관리를 위해 실행되는 프로세스**
  - 보통 시스템이 부팅될 때 자동 실행
  - 예) ftpd, inetd
- ps 출력 정보 항목
  - USER : 프로세스를 실행시킨 사용자 ID
  - PID : 프로세스 ID
  - %CPU : 마지막 1분간 프로세스가 사용한 CPU burst time의 점유율
  - %MEM : 마지막 1분간 프로세스가 사용한 메모리 점유율
  - VSZ : 프로세스가 사용하는 가상 메모리 사이즈
  - RSS : 프로세스가 사용하는 실제 메모리 사이즈
  - STAT : 프로세스 상태
  - START : 프로세스가 시작된 시간
  - TIME : 현재까지 사용된 CPU 시간
  - COMMAND : 명령어





프로세스 중지

- `kill PID` 명령어로 죽일 수 있음
- 강제 종료하고 싶은 경우 `kill -9`





#### 리눅스 파일 시스템





파일 인터페이스

- 리눅스에서는 **파일 추상화 인터페이스**로 모든 데이터 및 디바이스에 접근
  - **모든 것은 파일**이라는 철학
  - 모든 자원에 대한 추상화 인터페이스로 파일 인터페이스를 활용하는 것
    - 파일을 처리하는 4가지 인터페이스로(open, read, write, close) 다른 작업도 처리하자





파일 시스템의 체계

- **슈퍼블록** : 파일 시스템의 전체에 대한 정보
- 파일은 **inode** 고유값과 자료구조에 의해 주요 정보가 관리됨
  - inode에는 파일 **메타 데이터**와 disk block이 저장됨
  - 메타 데이터 : 파일 권한, 소유자 정보, 파일 사이즈, 생성시간, 데이터 저장 위치 등
  - inode 번호가 파일이름에 매칭됨
    - 각각의 파일은 inode를 기반으로 액세스





파일 탐색

예) `/home/ubuntu/link.txt` 를 어떻게 찾아갈까?

1. 먼저 각 디렉토리의 엔트리(**dentry**)를 탐색
   - 각 엔트리는 해당 디렉토리에 대한 파일/디렉토리의 정보를 갖고 있음
   - 여기서 파일/디렉토리 정보는 **inode로 관리**됨
     - 모든 것은 파일이라는 것
2. root dentry에서 'home'을, 'home'에서 'ubuntu'를, 'ubuntu'에서 'link.txt'을 찾아 해당하는 inode를 얻음





파일 복사

- `cp`
  - `cp file1 file2` 명령어로 file1을 file2로 복사함
  - 원본 파일 **inode 구조 내의 모든 데이터를 복사**해서 새로운 파일을 생성함
  - deep copy라고 할 수 있음
    - 원본을 수정해도 복사본의 내용은 변경되지 않음
    - 원본을 삭제해도 복사본은 남음
- 하드 링크
  - `ln file1 file2` 명령어로 file1을 file2로 복사함
  - 이렇게 복사된 파일의 **inode 번호는 원본 파일과 동일**
  - shallow copy라고 할 수 있음
    - 원본을 수정하면 복사본의 내용도 변경됨
    - **원본을 삭제해도 복사본은 남음** : inode 구조는 아직 남아있기 때문
  - 대용량의 파일을 다른 디렉토리에 복사하는 경우에 유용함
- 소프트 링크
  - `ln -s file1 file2` 명령어로 file1을 file2로 복사함
  - 복사된 파일의 **inode 번호는 원본 파일과 다름**
    - 새로 생성된 inode 구조의 direct block 자리에 원본 파일 inode 구조의 주소가 저장됨
  - 윈도우의 바로가기와 동일하다고 볼 수 있음
    - 원본을 수정하면 복사본의 내용도 변경됨
    - 원본을 삭제하면 복사본도 삭제됨
    - `ls -al`을 통해 해당 파일이 어느 파일을 가리키는지 확인 가능
    - 링크 파일임을 알려주기 위해 권한정보 첫 글자에 `l`이 표시됨





특수 파일(디바이스)

- 블록 디바이스(block device)
  - 블록, 섹터 단위로 데이터를 전송하는 디바이스
  - HDD, SSD 등 저장매체
  - I/O 송수신 속도가 높음
  - 권한정보 첫 글자가 `b`로 표시됨
- 캐릭터 디바이스(character device)
  - 바이트 단위로 데이터를 전송하는 디바이스
  - 키보드, 마우스 등 
  - I/O 송수신 속도가 낮음
  - 권한정보 첫 글자는 `c`
- `/dev` 디렉토리로 이동해서 다양한 디바이스들의 파일을 확인 가능





### 1.3. Ubuntu





특징





### 1.4. CentOS





특징

- 





우분투와의 비교

- 안정성
  - 우분투가 더 안정적이고 업데이트가 빠름
- 점유율
  - 세계적으로 3~4배정도 우분투가 더 많이 사용됨
  - 한국에서는 우분투가 약간 우세



### 1.5. RHEL







## **2. Unix**



### 2.1. FreeBSD
