# 1. 베이스 이미지로 가벼운 nginx alpine 버전을 사용합니다.
FROM nginx:alpine

# 2. 현재 폴더의 index.html을 컨테이너 안의 웹 서버 경로로 복사합니다.
COPY index.html /usr/share/nginx/html/index.html

# 3. 80번 포트를 외부로 노출합니다.
EXPOSE 80
