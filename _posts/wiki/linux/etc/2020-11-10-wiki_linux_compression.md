---
title: "압축"
excerpt: "how to use tar and zip"

categories:
  - linux
tags:
  - wiki
  - linux

toc: true
toc_sticky: true

date: 2020-11-10
last_modified_at: 2020-11-10
---
## tar

### tar 압축 옵션

| 옵션      | 설명                                    |
| -------- | ----------------------------------------|
| -c       | 파일을 tar로 묶음                        |
| -p       | 파일 권한을 저장                         |
| -v       | 묶거나 파일을 풀 때 과정을 화면으로 출력    |
| -f       | 파일 이름을 지정                         |
| -C       | 경로를 지정                              |
| -x       | tar 압축을 풂                            |
| -z       | gzip으로 압축하거나 해제함               |
|   -j     | bzip2방식                              |


### 압축하기
```
tar -czvf 생성할이름 경로
```

### 압축풀기
```
tar -xzvf 파일이름
tar -xzvf 파일이름 -C 경로
```

### 내용보기
```
tar tvf 파일명.tar
```


## zip
### 압축하기
```
zip 파일명
zip -r name.zip dir
```

### 압축풀기
```
unzip 파일명
```
