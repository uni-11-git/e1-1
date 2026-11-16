#Docker Compose 기초

docker-compose.yml의 기본 구조를 학습하고, 단일 서비스를 Compose로 실행한다.
배움 포인트: 컨테이너 실행 명령이 “문서화된 실행 설정”으로 바뀌는 이유

## 1. 파일 만들기

$ touch docker-compose.yml

## 2. 파일 실행

$ docker-compose up -d
WARN[0000] C:\Users\goo1902020801\코디세이\e1-1\bonus\docker-compose.yml: the attribute `version` is obsolete, it will be ignored, please remove it to avoid potential confusion 
[+] up 1/1
 ✔ Container bonus-my-web-1 Started 

 ## 3. 웹 브라우저 확인

 ## 4. 터미널 확인
 
 $ docker-compose ps
WARN[0000] C:\Users\goo1902020801\코디세이\e1-1\bonus\docker-compose.yml: the attribute `version` is obsolete, it will be ignored, please remove it to avoid potential confusion 
NAME             IMAGE     COMMAND                   SERVICE   CREATED              STATUS          PORTS
bonus-my-web-1   nginx     "/docker-entrypoint.…"   my-web    About a minute ago   Up About aminute   0.0.0.0:8081->80/tcp, [::]:8081->80/tcp


- 결과 : "복잡한 docker run 명령어를 docker-compose.yml 파일로 문서화하여 관리의 편의성을 높였습니다. 이제 명령어 한 줄(up)만으로 언제 어디서든 동일한 컨테이너 환경을 재현할 수 있습니다."