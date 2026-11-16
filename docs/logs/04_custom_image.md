# Dockerfile/이미지 기반의 커스텀 이미지

- 선택 방법 : (A) 웹 서버 베이스 이미지 활용(예: NGINX/Apache 등) + 정적 콘텐츠/설정만 교체

## 1. 수행 과정

### 1. HTML 파일 준비

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

### 2. Dockerfile 작성



```bash
$ cat <<EOF > Dockerfile (파일 생성)
> FROM nginx:latest (베이스 이미지)
> COPY index.html /usr/share/nginx/html/index.html (파일 복사)
> EXPOSE 80 (포트 설정)
> EOF (작성 종료)
 ```

 [커스텀 포인트 요약]

정적 콘텐츠 교체: 기본 NGINX 환영 페이지 대신 직접 작성한 index.html을 사용하여 서비스 목적에 맞는 화면을 구성함.
Dockerfile 활용: 베이스 이미지(nginx:latest)에 COPY 명령어를 사용하여 로컬의 파일을 이미지 내부로 자동 복사하도록 설정함.
포트 설정: EXPOSE 80을 통해 컨테이너가 80번 포트에서 서비스됨을 명시함.

### 3. 이미지 만들기

```bash
$ docker build -t my-custom-web . (이미지 빌드)
$ docker images (이미지 확인)
 ```

### 4. 실행 (컨테이너 띄우기)

```bash
$ docker run -d -p 8080:80 --name my-web my-custom-web (이미지로 컨테이너 실행)
 ```