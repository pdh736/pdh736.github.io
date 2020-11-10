---
title: "wc"
excerpt: "how to use wc"

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

## wc

### NAME 
wc - (word count) 사용자가 지정한 파일의 행, 단어, 문자수를 세는 프로그램

### SYNOPSIS
파일에 쓰여진 내용을 읽어 들여 파일의 행 수, 단어 수, 문자의 수를 세는 프로그램으로 파일에 쓰여진 내용을 파악하는데 요긴하게 사용된다.   
기본적인 사용법은 다음과 같다.
```
$ wc  [-clmw] [file ....]
기본형식 : wc -옵션 파일이름
파일이름을 입력하지 않으면 표준 입력으로 부터 정보를 받아들여 계산한다.
-l 행
-w 단어
-c 문자
```

### 사용법
```
$ wc -l  file_name
```
기본적인 사용법은 위의 형태로 많이 사용한다. wc 명령어와 옵션과 파일명을 입력한다.  
파일에 들어 있는 행의 개수만 세고 싶다면 위와 같이 명령어를 내리면 된다.
```
$ ls | wc -w
```
현재 위치 아래 파일 갯수 세기
```
find . -type f | wc -l
```
현재 위치 파일 갯수 세기
```
ls -l | grep ^- | wc -l
```
현재 위치 디렉토리 갯수 세기
```
 ls -l | grep ^d | wc -l
```

결과 라인수
```
netstat -antp | wc -l
```
