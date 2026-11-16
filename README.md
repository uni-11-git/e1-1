# E1_1

## 1. 프로젝트 개요

본 프로젝트는 Docker 컨테이너 기술의 핵심 개념을 이해하고, 실제 서비스 환경을 구축해보는 실습 과정입니다. 리눅스 터미널 환경에서의 기초 조작부터 Docker를 활용한 웹 서비스 배포, 네트워크 설정, 데이터 영속성 관리까지의 전 과정을 체계적으로 기록하였습니다.

## 2. 실행 환경
| 항목 | 상세 내용 |
| :--- | :--- |
| **OS** | Ubuntu 22.04 LTS (또는 Windows WSL2) |
| **Docker** | Docker Engine v24.0.x 이상 |
| **Language** | HTML, Bash Shell |
| **Tools** | Git, VS Code, Terminal |

## 3. 수행 항목 체크리스트

- [x] [터미널 조작 로그](./docs/01_terminal.md)
- [x] [권한 실습 및 증거](./docs/02_permissions.md)
- [x] [Docker 설치 및 기본 점검](./docs/03_docker_ops.md)
- [x] [Dockerfile 기반 커스텀 이미지 제작](./docs/04_custom_image.md)
- [x] [포트 매핑 및 접속 증거](./docs/05_network_and_volume.md)
- [x] [GitHub 연동 및 최종 보고서](./docs/06_git_final.md)

## 4. 검증 방법

실습 결과가 정상적으로 작동하는지 아래 방법으로 확인할 수 있습니다.

**컨테이너 상태 확인**

   ```bash
   $ docker ps -a
CONTAINER ID   IMAGE           COMMAND                   CREATED          STATUS     PORTS                                     NAMES
b6d3fab7d410   nginx           "/docker-entrypoint.…"   29 minutes ago   Up 29 minutes    80/tcp                                    vol-recovery-cnt
2731c4485102   my-custom-web   "/docker-entrypoint.…"   3 hours ago      Up 3 hours    0.0.0.0:8080->80/tcp, [::]:8080->80/tcp   my-web
d42dfb5915dc   ubuntu          "bash"                    4 hours ago      Exited (0) 4 hours ago                                             my_attach_test
e1e33f7b68ee   ubuntu          "bash"                    4 hours ago      Exited (0) 4 hours ago                                             my-ubuntu
acdc9baa174e   hello-world     "/hello"                  4 hours ago      Exited (0) 4 hours ago                                             strange_ptolemy
d4237fc988fe   my-web-site     "/docker-entrypoint.…"   5 days ago       Exited (0) 5 days ago                                              my-web-container
    ```

결과 해석: my-web 컨테이너가 Up 상태이며, 8080 포트로 정상 연결되어 서비스 중임을 확인하였습니다.

## 5. 🛠 트러블 슈팅 (Troubleshooting)

### 1. 포트 충돌 문제 (Port Conflict)
- **현상**: `docker run` 실행 시 8080 포트가 이미 사용 중이라는 에러와 함께 컨테이너 시작 실패.
- **원인**: 로컬 호스트(PC)에서 다른 서비스가 이미 8080 포트를 점유하고 있음.
- **해결**: 호스트 포트를 8081로 변경하여 실행 (`-p 8081:80`) 함으로써 문제 해결.

### 2. Git Bash 경로 인식 오류 (Path Conversion)
- **현상**: 볼륨 마운트 확인 시 `cat: 'C:/Program Files/Git/app/data/result.txt': No such file or directory` 에러 발생.
- **원인**: Windows Git Bash 환경에서 컨테이너 내부 경로(`/app/data`)를 Windows 절대 경로로 자동 변환하여 발생한 문제.
- **해결**: 경로 앞에 슬래시를 하나 더 추가(`//app/data`)하여 Git Bash의 자동 변환을 방지하고 컨테이너 경로임을 명시함.


## 6. Git 설정 및 GitHub 연동 확인

### 1. Git 설정

```bash
$ git config --global user.name "KHS" (이름)
$ git config --global user.email "goo190202@gmail.com" (이메일)

$ git config --list (확인)
diff.astextplain.textconv=astextplain
filter.lfs.clean=git-lfs clean -- %f
filter.lfs.smudge=git-lfs smudge -- %f
filter.lfs.process=git-lfs filter-process
filter.lfs.required=true
http.sslbackend=schannel
core.autocrlf=true
core.fscache=true
core.symlinks=false
pull.rebase=false
credential.helper=manager
credential.https://dev.azure.com.usehttppath=true
init.defaultbranch=master
user.email=goo190202@gmail.com
user.name=KHS
safe.directory=E:/workspace/my-project
core.repositoryformatversion=0
core.filemode=false
core.bare=false
core.logallrefupdates=true
core.symlinks=false
core.ignorecase=true
remote.origin.url=https://github.com/uni-11-git/my-project.git
remote.origin.fetch=+refs/heads/*:refs/remotes/origin/*
branch.main.remote=origin
branch.main.merge=refs/heads/main
 ```

### 2. GitHub 연동

![GitHub 연동 증거](./docs/img/github.png)
