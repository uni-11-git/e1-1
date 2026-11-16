# E1_1

## 1. 프로젝트 개요

### 1. 실습 내용
- Dockerfile을 이용한 Nginx 웹 서버 이미지 빌드
- 포트 포워딩(8081:80)을 이용한 컨테이너 실행
- 바인드 마운트를 이용한 소스 코드 실시간 반영 실습

## 2. 실행 환경

## 3. 수행 항목 체크리스트

### 1. 터미널 조작 로그

### 2. 권한 실습 및 증거

### 3. Docker 설치 및 기본 점검 & Docker 기본 운영 명령 수행

## 4. 검증 방법

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
