# 터미널 조작 로그

## 1. 현재 위치 확인 (pwd)

 ```bash
$ pwd
/c/Users/goo1902020801/코디세이/e1-1
 ```

## 2. 목록 확인 - 숨김 파일 포함 (ls -la)

```bash
$ ls -la
total 44
drwxr-xr-x 1 goo1902020801 1049089     0 11월 15 17:06 ./
drwxr-xr-x 1 goo1902020801 1049089     0 11월 15 12:45 ../
drwxr-xr-x 1 goo1902020801 1049089     0 11월 15 16:36 .git/
drwxr-xr-x 1 goo1902020801 1049089     0 11월 15 17:06 docker-mission/
drwxr-xr-x 1 goo1902020801 1049089     0 11월 15 16:34 docs/
-rw-r--r-- 1 goo1902020801 1049089 12955 11월 11 10:28 mission.txt
drwxr-xr-x 1 goo1902020801 1049089     0 11월 15 16:26 mount-demo/
-rw-r--r-- 1 goo1902020801 1049089 14221 11월 11 16:53 README.md
drwxr-xr-x 1 goo1902020801 1049089     0 11월 15 16:36 site/
 ```

## 3. 실습용 폴더 생성 (mkdir)

```bash
$ mkdir practice
 ```

## 4. 폴더 이동 (cd)

```bash
$ cd practice
 ```

## 5. 빈 파일 생성 (touch)

```bash
$ touch test.txt
 ```

## 6. 파일 내용 작성 (echo를 사용해 내용을 넣습니다)

```bash
$ echo "Hello!" > test.txt
 ```

## 7. 파일 내용 확인 (cat)

```bash
$ cat test.txt
Hello!
 ```

## 8. 파일 복사 (cp)

```bash
$ cp test.txt test_copy.txt
 ```

## 9. 파일 이름 변경/이동 (mv)

```bash
$ mv test_copy.txt renamed_test.txt
 ```

## 10. 파일 삭제 (rm)

```bash
$ rm test.txt
 ```