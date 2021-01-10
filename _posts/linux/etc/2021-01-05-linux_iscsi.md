---
title: "iscsi"
excerpt: "How to use iscsi"

categories:
  - linux_etc

tags:
  - linux
  - iscsi

toc: true
toc_sticky: true

date: 2020-01-05
last_modified_at: 2020-01-05
---

# iscsi
## iscsi 란
TCP/IP 네트워크를 통해 블록 중심형 스토리지 데이터를 맵핑하기 위한 scsi 전송프로토콜

target : 스토리지를 제공하는 장비
initiator : 스토리지를 이용하는 장비

```
/etc/iscsi/iscsid.conf : iscsiadm 이 읽는 설정파일
/sbin/iscsid : open iscsi 데몬
/sbin/iscsiadm : open iscsi 관리도구
/sbin/iscsi-iname : iscsi name 생성 툴

/var/lib/iscsi : 설정파일들 저장되는 위치
```

## iscsi client 사용
centos 기준
### 설치및 서비스 시작&등록
```
yum install iscsi-initiator-utilㄴs

vi /etc/iscsi/initiatorname.iscsi //iscsi name 수정
vi /etc/iscsi/iscsid.conf  //iscsi 설정 수정

systemctl enable iscsi
systemctl enable iscsid
systemctl enable start iscsi
systemctl enable start iscsid
```

### discovery
```
iscsiadm -m discovery -t -st -p [ip address]
or
iscsiadm --mode discovery --type -sendtargets --portal [ip address]

//결과 확인
iscsiadm -m node -o show
or
ls /var/lib/iscsi/nodes
```

### login
LUN login
```
iscsiadm -m session
```
으로 세션 확인, 로그인 전에는 연결된 세션 없음
```
iscsiadm -m node -l
or
iscsiadm -m node --login

개별 디스크 추가시
iscsiadm -m node --targetname [iqn name] --login
or
iscsiadm -m node -T [iqn name] -l
//iscsiadm -m node -T [iqn name] -p [ip address(:port,1)] -l
//typically TCP ports 860 and 3260 (:port, 1) 생략가능
//issiadm -m node -T iqn.2000-01.com.synology:DiskStation.Target-11.1c6f1153e3 -p 192.168.40.55 -l
```
확인
```
세션 확인
iscsiadm -m session -o show
or
iscsiadm -m session -p 1 or 3 //더 자세하게 확인

연결 확인
lsiscsi
or
fdisk -l
```
이후 포맷및 마운트

### 타겟 제거
```
//logout
iscsiadm -m node -T [iqn name]  -p [ip address]  -u
or
iscsiadm -m node --targetname [iqn name] --logout

//delete
iscsiadm -m node -o delete -T [iqn name]  -p [ip address]
or
iscsiadm -m --targetname [iqn name] --op=delete
```


