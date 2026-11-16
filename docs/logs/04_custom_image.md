# Dockerfile/이미지 기반의 커스텀 이미지

- 선택 방법 : (A) 웹 서버 베이스 이미지 활용(예: NGINX/Apache 등) + 정적 콘텐츠/설정만 교체

## 1. HTML 파일 준비

```bash
$ echo '<!DOCTYPE html><html><body><h1>My Docker Web!</h1></body></html>' > index.html

$ cat <<EOF > index.html
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <title>나의 커스텀 도커 서버</title>
    <style>
        body { background-color: #f0f8ff; text-align: center; padding-top: 50px; font-family: sans-serif; }
        h1 { color: #2e8b57; }
    </style>
</head>
<body>
    <h1>도커 커스텀 이미지 빌드 성공! 🚀</h1>
    <p>이 페이지는 NGINX 베이스 이미지에 제 파일을 넣어서 만든 결과물입니다.</p>
</body>
</html>
EOF
 ```

## 2. Dockerfile 작성

```bash
$ cat <<EOF > Dockerfile (파일 생성)
> FROM nginx:latest (베이스 이미지)
> COPY index.html /usr/share/nginx/html/index.html (파일 복사)
> EXPOSE 80 (포트 설정)
> EOF (작성 종료)
 ```

## 3. 이미지 만들기

```bash
$ docker build -t my-custom-web . (이미지 빌드)
$ docker images (이미지 확인)
$ docker run -d -p 8080:80 --name my-web my-custom-web (이미지로 컨테이너 실행)
 ```

## 4. 실행 (컨테이너 띄우기)






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

