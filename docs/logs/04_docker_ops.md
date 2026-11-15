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

### 1. Hello-World 실행 결과

- 명령어: `docker run hello-world`
- 결과:

```bash
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

### 2. Ubuntu 컨테이너 진입 및 내부 확인

- 명령어: `docker run -it ubuntu bash`
- 결과: 컨테이너 내부 쉘 접속 후 리눅스 환경 확인 완료

  - `ls`: 리눅스 표준 디렉토리 구조(bin, dev, etc, root 등) 확인
```bash
bin   dev  home  lib64  mnt  proc  run   srv  tmp  var
boot  etc  lib   media  opt  root  sbin  sys  usr
```


  - `whoami`: 현재 접속 계정이 `root`임을 확인

```bash
root
```

  - `cat /etc/os-release`: `Ubuntu 24.04.1 LTS (Resolute Raccoon)` 버전 정보 확인

```bash
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
root@9906fe7f9355:/# 
```

### 3. 컨테이너 종료/유지(attach/exec 등)의 차이


## 기존 Dockerfile 기반 커스텀 이미지 제작

Dockerfile 작성 및 이미지 빌드 (4단계)
- **작성한 Dockerfile 내용**:

  ```dockerfile
  FROM nginx:latest
  # 호스트의 index.html을 컨테이너 내부의 Nginx 기본 경로로 복사
  COPY index.html /usr/share/nginx/html/index.html
  # 80번 포트 노출
  EXPOSE 80
  ```

## 포트 매핑 및 접속 증거

### 1. 컨테이너 상세 정보 확인 (`inspect`)
- 명령어: `docker inspect my-mounted-web`
- 주요 확인 사항:
  - **Status**: `running`
  - **Ports**: `8081/tcp` -> `80/tcp`
  - **Binds**: 호스트의 `step3` 폴더와 컨테이너의 `nginx/html` 폴더가 연결됨.


"PortBindings": {
    "80/tcp": [
        {
            "HostIp": "",
            "HostPort": "8081"
        }
    ]
}
```

## 바인드 마운트 + 볼륨 영속성

### 1. 바인드 마운트

### 2. Docker 볼륨 검증 (영속성)

(Volume Persistence)

볼륨을 생성하고 데이터를 저장하여, 컨테이너가 삭제되어도 데이터가 유지되는지 확인했습니다.

- **볼륨 생성 및 데이터 쓰기**: 
  `docker run --rm -v my-vol://app/data alpine sh -c "echo 'hello volume' > //app/data/result.txt"`
- **데이터 읽기 확인**:
  ```bash
  $ docker run --rm -v my-vol://app/data alpine cat //app/data/result.txt
  hello volume
  ```

