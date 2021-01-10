---
title: "cron"
excerpt: "how to use wcron"

categories:
  - linux_etc
tags:
  - linux
  - cron

toc: true
toc_sticky: true

date: 2020-11-11
last_modified_at: 2020-11-11
---
# cron

## 개요

- cron, cronie, crond, cron daemon, crontab, cron job, crontab job
- 크론, 크론 데몬, 크론탭, 크론작업, 리눅스 작업 스케줄러
- /usr/sbin/crond
- /usr/bin/crontab

- 프로세스 예약 데몬
- 리눅스용 작업 스케줄러
- 특정시각에 명령어가 수행되도록 등록가능
- cronie(패키지) = crond([데몬](https://zetawiki.com/wiki/데몬)) + crontab(크론 계획표[[1\]](https://zetawiki.com/wiki/리눅스_반복_예약작업_cron,_crond,_crontab#cite_note-1))
- 로그: [/var/log/cron](https://zetawiki.com/wiki//var/log/cron)에 변경/수행 이력이 기록됨
- 런레벨, 사용자 권한 등 환경에 따라 GUI 프로그램을 실행시키거나 X윈도우에서 별도의 창을 띄우는 작업은 불가능할 수 있음

## 등록형식

```
* * * * *  수행할 명령어
┬ ┬ ┬ ┬ ┬
│ │ │ │ │
│ │ │ │ │
│ │ │ │ └───────── 요일 (0 - 6) (0:일요일, 1:월요일, 2:화요일, …, 6:토요일)
│ │ │ └───────── 월 (1 - 12)
│ │ └───────── 일 (1 - 31)
│ └───────── 시 (0 - 23)
└───────── 분 (0 - 59)
```
분,시,일,월,요일


## 예시

```
* * * * * /root/every_1min.sh
```
매 1분마다 /root/every_1min.sh 를 수행 (하루에 1440회)

```
15,45 * * * * /root/every_30min.sh
```
매시 15분, 45분에 /root/every_30min.sh 를 수행 (하루에 48회)

```
*/10 * * * * /root/every_10min.sh
```
0분마다 /root/every_10min.sh 를 수행 (하루에 144회)

```
0 2 * * * /root/backup.sh
```
매일 02:00에/root/backup.sh 를 수행 (하루에 1회) 

```
30 */6 * * * /root/every_6hours.sh
```
 매 6시간마다 수행(00:30, 06:30, 12:30, 18:30) 

```
30 1-23/6 * * * /root/every_6hours.sh
```
 1시부터 매 6시간마다 수행(01:30, 07:30, 13:30, 19:30) 

```
0 8 * * 1-5 /root/weekday.sh
```
 평일(월요일~금요일) 08:00 

```
0 8 * * 0,6 /root/weekend.sh
```
 주말(일요일, 토요일) 08:00 



## 작업목록 확인

**현재 사용자**
```
[root@zetawiki ~]# crontab -l
no crontab for root
```

**다른 사용자**
```
[root@zetawiki ~]# crontab -l -u testuser
no crontab for testuser
```


## 수동 등록 

```
crontab -e
```
vi 편집기나 Nano 에디터로 현재 사용자에 대한 cron작업의 확인/수정을 직접 할 수 있다.



## 등록 스크립트



## 삭제

현재 사용자의 예약작업을 모두 삭제
```
crontab -r
```

**실행예시**
```
[root@zetawiki ~]# crontab -l
* * * * * /root/a.sh
* * * * * /root/b.sh
* * * * * /root/c.sh
[root@zetawiki ~]# crontab -r
[root@zetawiki ~]# crontab -l
no crontab for root
```

