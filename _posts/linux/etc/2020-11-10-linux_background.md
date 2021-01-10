---
title: "back ground 실행"
excerpt: "run as back ground"

categories:
  - linux_etc

tags:
  - linux
  - back_ground

toc: true
toc_sticky: true

date: 2020-11-10
last_modified_at: 2020-11-10
---

## 백그라운드 실행
### 백그라운드 실행 프로세스 보기
jobs


### 실행중 백그라운드로 전환
프로그램 실행중 CTRL+Z  
jobs 로 확인  
bg %번호  


### 시작시 백그라운드로 실행
프로그램 실행 시 끝에 &를 붙여 백그라운드로 실행


## 백그라운드 프로그램 포그라운드로 전환
jobs로 확인
fg %번호


## 백그라운 프로그램 죽이기
kill 명령을 통해 백그라운드에서 동작하고 있는 프로그램을 죽일 수 있음  
kill -9 %번호


