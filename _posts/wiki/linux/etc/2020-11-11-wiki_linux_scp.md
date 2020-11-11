
---
title: "scp"
excerpt: "How to use scp"

categories:
  - linux
tags:
  - wiki
  - linux

toc: true
toc_sticky: true

date: 2020-11-11
last_modified_at: 2020-11-11
---
# SCP
Secure copy


## 다른 서버로 복사 (보내기)
**파일 보내기**
```
scp 파일 계정@서버주소:목적경로
```

**디렉토리 보내기**
```
scp -r 디렉토리 계정@서버주소:목적경로
```

### 예시

**파일보내기**
```
scp test.txt testuser@135.79.246.80:/home/testuser/
```
test.txt를 135.79.246.80 서버의 /home/testuser/ 폴더에 업로드

scp  libtest.so  root@192.168.40.179:/userdata/onvif/lib


## 다른 서버에서 복사 (가져오기)

**기본 포트 사용**
```
scp 계정@서버주소:원본경로 목적파일명
```

**다른 포트 사용**
```
scp -P 포트 계정@서버주소:원본경로 목적파일명
```

**폴더 복사**
```
scp -r 계정@서버주소:원본경로 목적상위폴더
scp -r testuser@135.79.246.81:/var/www/html/ /var/www/
```
