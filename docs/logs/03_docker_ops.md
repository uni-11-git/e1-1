# Docker

## Docker 설치 및 기본 점검

*Docker 버전 확인 결과를 기록한다. (docker --version)
*Docker 데몬 동작 여부 확인 결과를 기록한다. (docker info 또는 동등 점검)

### 1. Docker 버전 확인

- 명령어: `docker --version`
- 결과: `Docker version 29.9.2, build a82e528`

### 2. Docker 데몬 동작 여부 확인

- 명령어: `docker info`

```bash
- 결과 (실패) : Server:
failed to connect to the docker API at npipe:////./pipe/dockerDesktopLinuxEngine; check if the path is correct and if the daemon is running: open //./pipe/dockerDesktopLinuxEngine: The system cannot find the file specified.

- 결과 (성공) : Server:
 Containers: 1
  Running: 0
  Paused: 0
  Stopped: 1
 Images: 5
 Server Version: 29.9.2
 ```


## Docker 기본 운영 명령 수행


*수행 명령과 출력 결과를 기술 문서에 남긴다.

### 1. 이미지
다운로드/목록 확인

- 명령어: `docker images`
- 결과: 93.7MB 크기의 `my-web-site` 이미지를 포함하여 실습에 필요한 이미지들이 정상적으로 로드됨.

```text
IMAGE                ID             DISK USAGE   CONTENT SIZE
hello-world:latest   c567b231f656       25.9kB         9.49kB
my-web-site:latest   924595acea97       93.7MB         26.4MB
ubuntu:latest        6a06111c3b0a        163MB         45.9MB
 ```

### 2. 컨테이너
실행/중지/목록 확인 (예: docker ps, docker ps -a)

#### 2-1. 실행 중인 컨테이너 확인

- 명령어: `docker ps`
- 결과: `my-mounted-web` 컨테이너가 8081 포트에서 2시간째 정상 작동 중임을 확인.

```bash
 $ docker ps
CONTAINER ID   IMAGE     COMMAND                  CREATED       STATUS       PORTS                                     NAMES
4aa1ffd3415d   nginx     "/docker-entrypoint.…"   2 hours ago   Up 2 hours   0.0.0.0:8081->80/tcp, [::]:8081->80/tcp   my-mounted-web
```

### 3. 운영
로그 확인 (예: docker logs), 리소스 확인 (예: docker stats)

#### 3-1. 컨테이너 로그 확인

- 명령어: `docker logs my-mounted-web`
- 결과: Nginx 서버가 성공적으로 시작되었으며, worker process들이 정상적으로 구동됨을 확인.

```bash
/docker-entrypoint.sh: Configuration complete; ready for start up
2026/11/11 02:45:45 [notice] 1#1: nginx/1.31.8
2026/11/11 02:45:45 [notice] 1#1: start worker processes
... (생략) ...
2026/11/11 02:45:45 [notice] 1#1: start worker process 52
```

#### 3-2. 리소스 확인

- 명령어: `docker stats`

```bash
 $ docker stats --no-stream
CONTAINER ID   NAME             CPU %     MEM USAGE / LIMIT    MEM %     NET I/O        BLOCK I/O  PIDS
4aa1ffd3415d   my-mounted-web   0.00%     21.7MiB / 15.25GiB   0.14%     1.7kB / 126B   0B / 12.3kB  25
 ```

## 컨테이너 실행 실습

### 1. Hello-World 실행

- 명령어: `docker run hello-world`
- 결과: "Hello from Docker!" 메시지 확인 완료. 도커 엔진이 정상적으로 이미지를 가져오고 컨테이너를 생성함을 증명함.

```bash
$ docker run hello-world

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

### 2. Ubuntu 컨테이너

#### 1. 우분투 컨테이너 실행 및 터미널 진입

- 명령어: `docker run -it --name my-ubuntu ubuntu bash`
- 결과: 컨테이너 내부 쉘 접속 후 리눅스 환경 확인 완료

#### 2. 명령어 실습

##### 1. 파일 목록 확인

```bash
root@e1e33f7b68ee:/# ls
bin   dev  home  lib64  mnt  proc  run   srv  tmp  var
boot  etc  lib   media  opt  root  sbin  sys  usr
 ```

##### 2. 메시지 출력

```bash
root@e1e33f7b68ee:/# echo "hello, ubuntu!"
hello, ubuntu!
```

##### 3. OS 정보 확인

```bash
root@e1e33f7b68ee:/# cat /etc/os-release
PRETTY_NAME="Ubuntu 26.04.1 LTS"
NAME="Ubuntu"
VERSION_ID="26.04"
VERSION="26.04.1 LTS (Resolute Raccoon)"
VERSION_CODENAME=resolute
ID=ubuntu
ID_LIKE=debian
HOME_URL="https://www.ubuntu.com/"
SUPPORT_URL="https://help.ubuntu.com/"
BUG_REPORT_URL="https://bugs.launchpad.net/ubuntu/"
PRIVACY_POLICY_URL="https://www.ubuntu.com/legal/terms-and-policies/privacy-policy"
UBUNTU_CODENAME=resolute
LOGO=ubuntu-logo
root@e1e33f7b68ee:/# S
```

##### 4. 나가기

```bash
root@e1e33f7b68ee:/# exit
exit
```

### 3. 컨테이너 종료/유지의 차이

#### 1. docker exec (추천 방식)
- **특징**: 실행 중인 컨테이너에 새로운 프로세스(bash 등)를 추가로 실행해서 진입함.
- **관찰 결과**: `exit`로 빠져나와도 컨테이너는 종료되지 않고 계속 실행됨.
- **용도**: 운영 중인 컨테이너 내부 상태를 확인하거나 디버깅할 때 주로 사용.

- **실험 로그**

```bash

# 1. exec 진입

$ docker exec -it my_test //bin/bash

# 2. 프로세스 확인

root@77d718fc08f8:/# ps -ef
UID          PID    PPID  C STIME TTY          TIME CMD
root           1       0  0 03:53 ?        00:00:00 sleep 1000
root          13       0  0 03:57 pts/0    00:00:00 //bin/bash
root          20      13  0 03:58 pts/0    00:00:00 ps -ef

# 3. 종료 및 탈출

root@77d718fc08f8:/# exit
exit

What's next:
    Try Docker Debug for seamless, persistent debugging tools in any container or image → docker debug my_test
    Learn more at https://docs.docker.com/go/debug-cli/

# 4. 컨테이너 상태 확인

$ docker ps
CONTAINER ID   IMAGE     COMMAND        CREATED         STATUS         PORTS     NAMES
77d718fc08f8   ubuntu    "sleep 1000"   4 minutes ago   Up 4 minutes             my_test

```

#### 2. docker attach
- **특징**: 컨테이너가 처음 실행될 때의 메인 프로세스(PID 1)에 터미널을 연결함.
- **관찰 결과**: `exit`로 빠져나오면 메인 프로세스가 종료되면서 컨테이너도 함께 멈춤.
- **주의**: 컨테이너를 끄지 않고 빠져나오려면 `Ctrl + P, Q`를 순서대로 눌러야 함.

- **실험 로그**

```bash

# 1. attach 진입

$ docker attach my_attach_test

# 2. 프로세스 확인

root@d42dfb5915dc:/# ps -ef
UID          PID    PPID  C STIME TTY          TIME CMD
root           1       0  0 04:02 pts/0    00:00:00 bash
root           8       1  0 04:03 pts/0    00:00:00 ps -ef
root@d42dfb5915dc:/# exit
exit

# 3. 종료 및 탈출

root@d42dfb5915dc:/# exit
exit

# 4. 컨테이너 상태 확인

CONTAINER ID   IMAGE         COMMAND                   CREATED          STATUS      PORTS     NAMES
d42dfb5915dc   ubuntu        "bash"                    32 seconds ago   Exited (0) 4 seconds ago              my_attach_test

```