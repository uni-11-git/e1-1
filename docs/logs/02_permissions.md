# 권한 실습 및 증거

*권한을 확인/변경하는 명령을 수행하고, 변경 전/후 비교를 기술 문서에 남긴다.
*최소 요구: 파일 1개, 디렉토리 1개에 대해 권한 변경 실험을 수행한다.

## 1. 파일

### 1-1. 현재 권한 확인

 ```bash
$ ls -l renamed_test.txt
-rw-r--r-- 1 goo1902020801 1049089 7 11월 15 17:15 renamed_test.txt
 ```

### 1-2. 권한 변경 (읽기 전용)

 ```bash
$ chmod 444 renamed_test.txt
 ```

### 1-3. 권한 변경 확인

 ```bash
$ ls -l renamed_test.txt
-r--r--r-- 1 goo1902020801 1049089 7 11월 15 17:15 renamed_test.txt
 ```

 ### 1-4. 접근 거부 확인

 ```bash
 $ echo "change" > renamed_test.txt
bash: renamed_test.txt: Permission denied
 ```

### 1-5. 원상 복구

```bash
$ chmod 644 renamed_test.txt
 ```

## 2. 디렉토리

### 2-1. 폴더 생성 및 권한 확인

```bash
$ ls -ld secret_folder

$ ls -ld secret_folder
drwxr-xr-x 1 goo1902020801 1049089 0 11월 15 17:36 secret_folder/
 ```

### 2-2. 접근 권한 제거

```bash
$ chmod 000 secret_folder
 ```

### 2-3. 접근 거부 확인

```bash
$ ls secret_folder
ls: cannot open directory 'secret_folder': Permission denied

 ```

### 2-4. 폴더 권한 복구

```bash
$chmod 755 secret_folder
 ```

 ### 2-5. 복구 확인

```bash
$ ls -ld secret_folder
drwxr-xr-x 1 goo1902020801 1049089 0 11월 15 18:04 secret_folder/
  ```