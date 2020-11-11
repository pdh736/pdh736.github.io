---
title: "nbtstat"
excerpt: "How to use nbtstat"

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

## nbtstat (NetBIOS over Tcp/ip state)
TCP/IP의 NETBIOS가 사용하여 상대방의 mac, 사용자 이름등을 표시  
주로 IP충돌이 났을 경우 상대방이 누군지 확인하는 명령어, 상대방 맥주소 확인할 때 사용
ex) `netstat -antp | grep 149 | wc -l`
 
 
### 옵션
|              |               |
|-------------|-----------------|
|-a 컴퓨터이름 	|원격 컴퓨터의 NetBIOS이름 테이블을 표시			|
|-A ip주소		|원격 컴퓨터의 ip주소로 NetBIOS 이름 테이블을 표시	|
|-c				|NetBIOS 이름 캐시의 내용 						|
|-n				|로컬 컴퓨터의 NetBIOS 이름 테이블을 표시			|
|-r				|NetBIOS 이름 확인 통계를 표시					|
|-R				|NetBIOS 이름 캐시의 내용을 비움					|


