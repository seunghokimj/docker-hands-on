# 도커(Docker) 기초

## 도커 개념
slide

## 도커 엔진(Docker Engine) 설치
* [도커 설치 페이지](https://docs.docker.com/engine/install/)
* [리눅스(ubuntu) 도커 설치](https://docs.docker.com/engine/install/ubuntu/)
```shell script
$ sudo apt-get remove docker docker-engine docker.io containerd runc
$ sudo apt-get update
$ sudo apt-get install \
    apt-transport-https \
    ca-certificates \
    curl \
    gnupg-agent \
    software-properties-common
$ curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
$ sudo apt-key fingerprint 0EBFCD88
$ sudo add-apt-repository \
   "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
   $(lsb_release -cs) \
   stable"
$ sudo apt-get update
$ sudo apt-get install docker-ce docker-ce-cli containerd.io
$ sudo docker -v
Docker version 19.03.13, build 4484c46d9d

``` 
* [맥 도커 설치](https://docs.docker.com/docker-for-mac/install/)
* [윈도우 도커 설치](https://docs.docker.com/docker-for-windows/install/)  

## 도커 이미지와 컨테이너
도커(엔진) 에서 사용하는 기본 단위는 이미지와 컨테이너 입니다. 사실상 이 둘이 도커의 핵심입니다.
- 붕어빵과 틀
- 1:N

### 도커 이미지 개념
이미지는 컨테이너를 생성할 때 필요한 요소 입니다. 컨테이너를 생성하기 위한 템플릿 입니다. 저장소(repository) 통해 이미지를 저장하고 불러올 수 있습니다.
- 붕어빵 틀
- 저장소(Repository): 이미지가 저장된 장소를 의미
- 도커 허브(Docker hub): 도커에서 공식으로 사용하는 저장소 이름

### 도커 컨테이너 개념
앞에서 설명한 도커 이미지는 우분투, CentOS 등 리눅스 운영체제부터 DB, 언어별 개발환경, 머신러닝 개발 환경 등까지 다양한 종류가 있습니다.
이러한 이미지로 컨테이너를 생성하면 해당 이미지의 모적에 맞는 파일이 들어 있는 파일 시스템과 격리된 시스템 자원 및 네트워크를 사용할 수 있는 독립된 공간이 생성되고, 이것이 바로 도커 컨테이너 입니다.
컨테이너는 이미지를 읽기 전용으로 사용하게되고 컨테이너 내부의 변경 사항은 이미지에 영향을 주지 않습니다.
또한 생성된 각 컨테이너는 각기 독립된 파일시스템을 제공 받으며 호스트와 분리돼 있으므로 특정 컨테이너에서 어떤 애플리케이션을 설치하거나 삭제해도 다른 컨테이너와 호스트는 변화가 없습니다.  
- 붕어빵

## 도커 컨테이너 실습
실습에서는 docker CLI(Command Line Interface)으로 진행합니다.

도커는 root 권한을 요구하기 때문에 `docker` 명령어는 `sudo` 명령어를 붙어야 실행할 수 있습니다.

도커 명령어를 편리하게 실행하기 위해 group을 설정합니다. 

```shell script
$ sudo groupadd docker
$ sudo usermod -aG docker $USER 
 
```
로그아웃 후 다시 로그인 합니다.

### 도커 컨테이너 생성(실행)
도커 컨테이너에 관련된 CLI 는 아래의 형태입니다.  
```shell script
$ docker container run [OPTIONS] IMAGE [COMMAND] [ARG...] 
```

#### 도커 "Hello, World"
```shell script
$ docker container run hello-world

Unable to find image 'hello-world:latest' locally
latest: Pulling from library/hello-world
0e03bdcc26d7: Pull complete 
Digest: sha256:e7c70bb24b462baa86c102610182e3efcb12a04854e8c582838d92970a09f323
Status: Downloaded newer image for hello-world:latest
Hello from Docker!
This message shows that your installation appears to be working correctly.
To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
    (amd64)
 3. The Docker daemon created a new container from that image which runs the
    executable that produces the output you are currently reading.
 4. The Docker daemon streamed that output to the Docker client, which sent it
    to your terminal.
To try something more ambitious, you can run an Ubuntu container with:
 $ docker run -it ubuntu bash
Share images, automate workflows, and more with a free Docker ID:
 https://hub.docker.com/
For more examples and ideas, visit:
 https://docs.docker.com/get-started/
```

#### 여러 버전의 ubuntu 리눅스 컨테이너 생성
* 최신 버전 ubuntu 리눅스 컨테이너 생성

우선 최신 버전 ubuntu 리눅스 컨테이너를 생성해 봅니다.

```shell script
$ docker container run ubuntu
Unable to find image 'ubuntu:latest' locally
latest: Pulling from library/ubuntu
6a5697faee43: Pull complete 
ba13d3bc422b: Pull complete 
a254829d9e55: Pull complete 
Digest: sha256:fff16eea1a8ae92867721d90c59a75652ea66d29c05294e6e2f898704bdb8cf1
Status: Downloaded newer image for ubuntu:latest
```
이미지를 잘 다운로드하였고, 무언가 실행될 것 같지만 아무런 결과가 보이지 않습니다.

```shell script
$ docker container run -it ubuntu bash
Unable to find image 'ubuntu:latest' locally
latest: Pulling from library/ubuntu
6a5697faee43: Pull complete 
ba13d3bc422b: Pull complete 
a254829d9e55: Pull complete 
Digest: sha256:fff16eea1a8ae92867721d90c59a75652ea66d29c05294e6e2f898704bdb8cf1
Status: Downloaded newer image for ubuntu:latest
root@11f87c101077:/# exit
```
* 18.04 LTS ubuntu 리눅스 생성
```shell script
$ docker container run -it ubuntu:18.04 bash
Unable to find image 'ubuntu:18.04' locally
18.04: Pulling from library/ubuntu
171857c49d0f: Pull complete 
419640447d26: Pull complete 
61e52f862619: Pull complete 
Digest: sha256:646942475da61b4ce9cc5b3fadb42642ea90e5d0de46111458e100ff2c7031e6
Status: Downloaded newer image for ubuntu:18.04
root@6fb17882bda8:/ exit
```

#### Detached(background) vs foreground
도커 `run` 명령어에 아무런 옵션을 추가 하지 않으면 default로 "foreground" 모드로 실행 됩니다.
도커 `run` 명령어에 `-d` 옵션을 입력하면 "background" 모드로 실행 됩니다.

```shell script
$ docker container run -d nginx
...
$ docker cotainer ps
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS               NAMES
a3f3ec397f3a        nginx               "/docker-entrypoint.…"   4 seconds ago       Up 3 seconds        80/tcp              clever_bose
```
 

#### 컨테이너 종료
* exit (Ctrl + D)
* 임시 빠저나오기 (Ctrl + P,Q)

#### 컨테이너 접속
도커 컨테이너에 접속하는 명령어는 `attach` 와 `exec` 가 있습니다.

* attach

`attach`는 실행되고 있는 컨테이너에 접속할 때 사용합니다.
```shell script
$ docker container run -it ubuntu:18.04 bash
root@44565bc0191f:/# (Ctrl + P,Q)

$ sudo docker container ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
44565bc0191f        ubuntu:18.04        "bash"              49 seconds ago      Up 48 seconds                           exciting_gould

$ sudo docker container attach 44565bc0191f
root@44565bc0191f:/#  
```

* exec

`exec`는 실행 중인 컨테이너에서 명령어를 실행시킬 때 사용합니다.
```shell script
$ docker container run -it ubuntu:18.04 top
top - 18:20:50 up  1:20,  0 users,  load average: 0.12, 0.03, 0.01
Tasks:   1 total,   1 running,   0 sleeping,   0 stopped,   0 zombie
%Cpu(s):  0.0 us,  0.3 sy,  0.0 ni, 99.7 id,  0.0 wa,  0.0 hi,  0.0 si,  0.0 st
KiB Mem :  7636432 total,  5774936 free,   316604 used,  1544892 buff/cache
KiB Swap:        0 total,        0 free,        0 used.  7075768 avail Mem 
  PID USER      PR  NI    VIRT    RES    SHR S  %CPU %MEM     TIME+ COMMAND                                                                                                                                                          
    1 root      20   0   36736   3172   2748 R   0.0  0.0   0:00.03 top               

(Ctrl + P,Q)
$ docker container ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
882913fbf440        ubuntu:18.04        "top"               53 seconds ago      Up 52 seconds                           zen_joliot

$ docker container exec -it 882913fbf440 /bin/bash
root@882913fbf440:/# 
```

### 컨테이너 목록 확인
* 정지되지 않은 목록 확인
```shell script
$ docker container ps
CONTAINER ID        IMAGE               COMMAND             CREATED              STATUS              PORTS               NAMES
882913fbf440        ubuntu:18.04        "top"               About a minute ago   Up About a minute                       zen_joliot
```

* 모든 목록 확인
```shell script
$ docker container ps -a
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS                         PORTS               NAMES
882913fbf440        ubuntu:18.04        "top"               3 minutes ago       Up 3 minutes                                       zen_joliot
213f9812d619        ubuntu:18.04        "/bin/bash"         6 minutes ago       Exited (0) 6 minutes ago                           epic_cartwright
...
```

#### docker container ps 출력 설명
* CONTAINER ID: 컨테이너에 자동으로 할당되는 고유 ID 
* IMAGE: 컨테이너를 생성할 때 사용된 이미지 이름
* COMMAND: 컨테이너가 시작될 때 실행될 명령어
* CREATED: 컨테이너 생성된 후 흐른 시간
* STATUS: 컨테이너의 상태
* PORTS: 컨테이너에서 오픈한 포트 정보
* NAMES: 컨테이너의 고유 이름. 사용자가 입력하지 않으면 도커 엔진이 랜덤하게 이름을 생성함 

### 컨테이너 삭제
```shell script
$ docker container ps -a
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS                         PORTS               NAMES
882913fbf440        ubuntu:18.04        "top"               9 minutes ago       Up 9 minutes                                       zen_joliot
213f9812d619        ubuntu:18.04        "/bin/bash"         12 minutes ago      Exited (0) 12 minutes ago                          epic_cartwright
...

$ docker container rm 213f9812d619
213f9812d619
```

#### 컨테이너 강제 삭제
현재 실행중인 컨테이너는 `rm` 명령어로 바로 삭제되지 않습니다. 
```shell script
$ docker container rm -f CONTAINER_ID
```

#### 여러 컨테이너 삭제
도커를 사용하다 보면 컨테이너가 너무 많이 생성되어 일일이 삭제하기 번거로울수 있습니다. 이런 경우 `prune` 명령어를 사용합니다.
```shell script
$ docker container prune
WARNING! This will remove all stopped containers.
Are you sure you want to continue? [y/N] y
Deleted Containers:
...
```

### 컨테이너 포트 개방
컨테이너를 사용하여 nginx 웹서버를 실행해 봅니다.

```shell script
$ docker container run -d nginx
...
$ docker cotainer ps
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS               NAMES
a3f3ec397f3a        nginx               "/docker-entrypoint.…"   4 seconds ago       Up 3 seconds        80/tcp              clever_bose
```

80 포트를 확인해 봅니다.
```shell script
$ curl localhost:80
curl: (7) Failed to connect to localhost port 80: Connection refused
```

컨테이너로 nginx 웹서버를 실행하였는데 80 포트로 접속이 되지 않습니다. 이번엔 컨테이너 내부에 접속하여 확인해 봅니다.
```shell script
$ docker container ps
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS               NAMES
a3f3ec397f3a        nginx               "/docker-entrypoint.…"   3 minutes ago       Up 3 minutes        80/tcp              clever_bose

$ docker container exec -it a3f3ec397f3a /bin/bash
root@a3f3ec397f3a:/# curl localhost:80
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
<style>
    body {
        width: 35em;
        margin: 0 auto;
        font-family: Tahoma, Verdana, Arial, sans-serif;
    }
</style>
</head>
<body>
<h1>Welcome to nginx!</h1>
<p>If you see this page, the nginx web server is successfully installed and
working. Further configuration is required.</p>

<p>For online documentation and support please refer to
<a href="http://nginx.org/">nginx.org</a>.<br/>
Commercial support is available at
<a href="http://nginx.com/">nginx.com</a>.</p>

<p><em>Thank you for using nginx.</em></p>
</body>
</html>
```
컨테이너 내부로 접속했을 때는 80 포트로 잘 접속되는 것을 확인할 수 있습니다. 컨테이너 내부에는 80 포트가 개방되어 있지만 컨테이너 외부에서는 포트가 열려있지 않아서 접속되지 않습니다.

`-p` 옵션으로 컨테이너 내부 포트를 외부에 연결합니다.
```shell script
$ docker container run -p 80:80 -d nginx
bb655cd139aa5b7cf4dba4c60a069f063a466cbc000e36ca8b3bcce14057d822

$ curl localhost:80
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
<style>
    body {
        width: 35em;
        margin: 0 auto;
        font-family: Tahoma, Verdana, Arial, sans-serif;
    }
</style>
</head>
<body>
<h1>Welcome to nginx!</h1>
<p>If you see this page, the nginx web server is successfully installed and
working. Further configuration is required.</p>

<p>For online documentation and support please refer to
<a href="http://nginx.org/">nginx.org</a>.<br/>
Commercial support is available at
<a href="http://nginx.com/">nginx.com</a>.</p>

<p><em>Thank you for using nginx.</em></p>
</body>
</html>
```

### docker container run vs docker run
도커에서 컨테이너 또는 이미지 관련하여 자주 사용하는 명령어들은 각 명령어의 메인 명령어를 생략 할 수 있습니다.

예를 들어 `docker container run` 명령어에서 container 를 제외하고 `docker run` 해도 동일한 기능을 합니다.

(개인견해) 처음 컨테이너와 이미지 개념을 이해할 때, `container`, `image` 명령어를 붙이는 것이 도움이 많이 되었습니다. 
이번 실습에서는 `container`, `image`을 되도록 붙입니다. 
   
## 도커 이미지 실습
### [도커 허브(Docker Hub)](https://hub.docker.com/)
도커 허브는 도커에서 제공하는 기본 이미지 저장소 입니다. 앞서 컨테이너 실습에서 이미지를 다운로드 받은 저장소 역시 도커 허브 입니다.
### 도커 이미지 검색
```shell script
$ docker search ubuntu
docker search ubuntu
NAME                                                      DESCRIPTION                                     STARS               OFFICIAL            AUTOMATED
ubuntu                                                    Ubuntu is a Debian-based Linux operating sys…   11552               [OK]                
dorowu/ubuntu-desktop-lxde-vnc                            Docker image to provide HTML5 VNC interface …   476                                     [OK]
rastasheep/ubuntu-sshd                                    Dockerized SSH service, built on top of offi…   250                                     [OK]
consol/ubuntu-xfce-vnc                                    Ubuntu container with "headless" VNC session…   228                                     [OK]
```

### 도커 이미지 다운로드
```shell script
$ sudo docker image pull debian
Using default tag: latest
latest: Pulling from library/debian
756975cb9c7e: Pull complete 
Digest: sha256:e2cc6fb403be437ef8af68bdc3a89fd58e80b4e390c58f14c77c466002391193
Status: Downloaded newer image for debian:latest
docker.io/library/debian:latest
```
### 도커 이미지 목록 확인
```shell script
$ sudo docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
debian              latest              ef05c61d5112        7 days ago          114MB
ubuntu              latest              d70eaf7277ea        4 weeks ago         72.9MB
ubuntu              18.04               56def654ec22        2 months ago        63.2MB
...
```

### 도커 이미지 생성(commit)
기존 base 이미지를 변경하여 나만의 새로운 이미지를 생성해 보겠습니다.
`commit` 명령어는 사용법은 아래 형태 입니다.
```shell script
docker commit [OPTIONS] CONTAINER [REPOSITORY[:TAG]]
```

우선 ubuntu 이미지를 가지고 컨테이너 하나를 생성합니다. 생성한 컨테이너 안에서 my_file 이라는 파일을 하나 생성하고 컨테이너를 종료합니다.
```shell script
$ docker run -it --name commit_image ubuntu:18.04
root@8136a507efcf:/# touch my_file
root@8136a507efcf:/# ls
bin  boot  dev  etc  home  lib  lib64  media  mnt  my_file  opt  proc  root  run  sbin  srv  sys  tmp  usr  var
root@8136a507efcf:/# exit 
```

위에서 변경된 컨테이너를 통해 새로운 이미지를 생성합니다. 

```shell script
docker commit -a seunghokim -m "add my_file" commit_image commit_image:first
sha256:a18a941c4b77f92299c4f351ef74991dcaaf90d48d304900599aadd72b5ec56d
$ docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
commit_image        first               a18a941c4b77        7 seconds ago       63.2MB
...
ubuntu              18.04               56def654ec22        2 months ago        63.2MB
...
```
`-a` 옵션은 author 를 의미하며, 이미지의 작성자 본인 아이디(예 seunghokim)를 입력합니다. `-m` 옵션은 commit message 를 의미합니다.

새로 변경된 이미지로 컨테이너를 생성해 봅니다. 처음부터 `my_file` 이라는 파일이 생성되 있습니다.
```shell script
$ docker container run -it commit_image:first 
root@021baf4ba4c0:/# ls     
bin  boot  dev  etc  home  lib  lib64  media  mnt  my_file  opt  proc  root  run  sbin  srv  sys  tmp  usr  var
```

### 도커 이미지 배포
git에 소스코드를 commit 후 GitHub에 `push`, `pull` 하는 것과 비슷하게 도커 이미지도 `push`, `pull`를 할 수 있습니다.

도커에서 공식적으로 제공하는 도커 허브에 push 하여 새로운 이미지를 배포해 봅니다.

#### 도커 허브 가입, 로그인
가입 링크: https://hub.docker.com/signup

도커 허브에 가입을 했으면 로그인을 합니다. 로그인 하지 않으면 도커 허브 저장소에 이미지를 push할 권한이 없습니다.
```shell script
$ $ docker login
Login with your Docker ID to push and pull images from Docker Hub. If you don't have a Docker ID, head over to https://hub.docker.com to create one.
Username: YOUR_USERNAME
Password: YOUR_PASSWORD
WARNING! Your password will be stored unencrypted in /home/shkim_kaeri_202011/.docker/config.json.
Configure a credential helper to remove this warning. See
https://docs.docker.com/engine/reference/commandline/login/#credentials-store
Login Succeeded

```

#### 이미지 태그(tag)
새로 생성한 `commit_image` 이미지에 태그를 붙여봅니다. 

내 아이디와 저장소 이름으로 태그된 이미지 파일만 도커 허브에 이미지를 push 할 수 있습니다.
```shell script
$ docker image tag f607173ae878 seunghokim/commit_image:first
shkim_kaeri_202011@docker-hands-on:~$ docker images
REPOSITORY                TAG                 IMAGE ID            CREATED             SIZE
commit_image              first               f607173ae878        22 minutes ago      63.2MB
seunghokim/commit_image   first               f607173ae878        22 minutes ago      63.2MB
``` 

#### 이미지 push, pull
태그한 이미지를 도커 허브에 push, pull 해봅니다.
```shell script
$ docker image push seunghokim/commit_image:first
The push refers to repository [docker.io/seunghokim/commit_image]
de4e96d699e0: Pushed 
7a694df0ad6c: Mounted from library/ubuntu 
3fd9df553184: Mounted from library/ubuntu 
805802706667: Mounted from library/ubuntu 
first: digest: sha256:e6d788ad39a41fa3dee59cfd977be319136a34334a78663c81ebf1c4dabe63b9 size: 1150

$ docker pull seunghokim/commit_image:first
first: Pulling from seunghokim/commit_image
Digest: sha256:e6d788ad39a41fa3dee59cfd977be319136a34334a78663c81ebf1c4dabe63b9
Status: Image is up to date for seunghokim/commit_image:first
docker.io/seunghokim/commit_image:first
```

도커 허브 웹 사이트에 접속하여 push 한 이미지가 보이는지 확인합니다.

## [Dockerfile](https://docs.docker.com/engine/reference/builder/)
### Dockerfile을 이용한 이미지 생성
`commit` 명령어를 통해 새로운 이미지를 생성하는 방법을 실습해 봤습니다. 
`commit` 명령어는 이미지를 생성하는 과정이 눈에 바로 보이지 않아 관리하기가 쉽지 않습니다.

도커는 Dockerfile 의 명령어들을 읽어 자동으로 이미지를 `build`할 수 있습니다. 
Dockerfile 은 사용자가 이미지를 조립하기 위해 커맨드라인에서 호출할 수 있는 모든 명령어를 포함하는 텍스트 문서입니다.

`commit` 을 이용한 이미지 생성 실습 과정과 동일한 결과가 나오도록 Dockerfile 을 작성하고 `build` 해보겠습니다.

#### Dockerfile 실습 내용과 동일한 이미지 생성
* Dockerfile
```dockerfile
FROM ubuntu:18.04
WORKDIR /
RUN touch my_file
```

* docker image build 명령어
```shell script
$ docker image build . -t seunghokim/build_image:first
Sending build context to Docker daemon  18.43kB
Step 1/3 : FROM ubuntu:18.04
 ---> 56def654ec22
Step 2/3 : WORKDIR /
 ---> Running in 8b41a106b6e0
Removing intermediate container 8b41a106b6e0
 ---> 1942100e32b4
Step 3/3 : RUN touch my_file
 ---> Running in e15d06295763
Removing intermediate container e15d06295763
 ---> 4369e22d43a2
Successfully built 4369e22d43a2

$ docker images
REPOSITORY                TAG                 IMAGE ID            CREATED             SIZE
seunghokim/build_image    first               4369e22d43a2        5 minutes ago       63.2MB
commit_image              first               f607173ae878        49 minutes ago      63.2MB

$ docker run -it seunghokim/build_image:first
root@fdcf6eeb8955:/# ls
bin  boot  dev  etc  home  lib  lib64  media  mnt  my_file  opt  proc  root  run  sbin  srv  sys  tmp  usr  var
root@fdcf6eeb8955:/# 
```
이미지 id가 다르지만 동일한 기능을 하는 이미지가 생성되었습니다.

### Dockerfile 명령어
Dockerfile에는 컨테이너에서 수행해야 할 작업을 명시합니다.
Dockerfile에는 도커 이미지를 빌드하기 위한 별도의 명령어가 있습니다. 
 
Dockerfile 명령어들은 소문자를 사용해도 동작하지만, 일반적으로 대문자를 사용합니다.
* FROM: 생성할 이미지의 base 이미지 이름
* RUN: 이미지를 만들기 위해 컨테이너 안에서 수행할 명령어
* CMD: 컨테이너가 실제 실행될 때, 실행되는 명령어
* EXPOSE: Dockerfile로 생성된 이미지에서 오픈할 포트 설정
* WORKDIR: 명령어를 실행한 디렉터리
* COPY: 이미지에 추가할 파일 정보  

## 도커를 활용한 자바스크립트 테트리스 배포
* [GitHub: uzyexe/javascript-tetris](https://github.com/uzyexe/javascript-tetris)
* [자바스크립트 테트리스](https://finished-javascript-tetris-ainize-team.endpoint.ainize.ai/) 

### 테트리스 실행
* git clone
```shell script
$ git clone https://github.com/uzyexe/javascript-tetris
Cloning into 'javascript-tetris'...
remote: Enumerating objects: 31, done.
remote: Total 31 (delta 0), reused 0 (delta 0), pack-reused 31
Unpacking objects: 100% (31/31), done.
```

* Dockerfile 빌드
```shell script
$ cd javascript-tetris/
$ docker image build -t seunghokim/javascript-tetris .
Sending build context to Docker daemon  228.4kB
Step 1/5 : FROM nginx:alpine
...
Successfully tagged seunghokim/javascript-tetris:latest
```

* 테트리스 컨테이너 실행
```shell script
$ $ docker container run -d -p 80:80 seunghokim/javascript-tetris 
33173eb631bcb925103958e58f4ebf7486eace47c78c362273afc77cef3d2312
$ curl localhost:80 
<!DOCTYPE html>
<html>
<head>
  <title>Javascript Tetris</title>
...
``` 

* 웹 브라우저 접속

### Google Cloud Run 으로 자바스크립트 테트리스 배포
#### Cloud Run
* 완전 관리형 서버리스 플랫폼에서 확장성이 우수한 컨테이너식 애플리케이션을 개발하고 배포합니다.
* 도커 이미지를 통해 빠른 배포
* [제한](https://cloud.google.com/run/quotas)

#### 테트리스 배포
* Google Container Registry
도커 허브가 아닌 GCP 내부의 private registry(repository)

* 도커 이미지 rename
```shell script
$ docker image tag seunghokim/javascript-tetris asia.gcr.io/[YOUR_PROJECT_ID]/javascript-tetris
```

* gcloud 인증
```shell script
$ gcloud auth login
$ gcloud auth configure-docker
```

* GCR push
```shell script
$ docker push asia.gcr.io/[YOUR_PROJECT_ID]/javascript-tetris
docker push asia.gcr.io/[YOUR_PROJECT_ID]/javascript-tetris
The push refers to repository [asia.gcr.io/[]/javascript-tetris]
358964ca7d3b: Preparing 
cb321ffc0160: Preparing 
468af79aab10: Preparing 
fbf82c12d86e: Preparing 
4dc20fbc0e8d: Preparing 
b831cc3ae47e: Waiting 
ace0eda3e3be: Waiting  
```

* Google Cloud Run 배포
