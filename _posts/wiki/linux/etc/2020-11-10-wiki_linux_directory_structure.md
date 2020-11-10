---
title: "Directory Structure"
excerpt: "Linux Directory Structure"

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

[toc]



## 디렉토리 구조

/ : 최상위 디렉터리(root)


/bin : Binary의 약자로서 명령어가 들어있는 디렉터리라고 이해


/boot : 부팅 시에 필요한 파일이 들어있습니다. 흔히 아는 grub 부트로더 관련 파일도 여기에 들어 있음

/dev : 물리적 장치(하드디스크, CD-ROM 등)를 파일화 하여 관리하는 디렉터리

/etc : 시스템 환경 설정 및 부팅관련 스크립트 파일 등이 들어있는 디렉터리입니다.

/home : 홈 디렉터리가 위치합니다. 홈 디렉터리는 개인 사용자들이 파일이나 디렉터리를 만들어 사용할 수 있는 공간으로 개인사용자의 경우 /home/사용자명 으로 생성

/lib : 각종 라이브러리가 저장된 디렉터리

/lost+found : fsck 명령을 통해 시스템을 복구할때 사용하는 디렉터리

/mnt :마운트할때 포인터가 되는 디렉터리

/misc : autofs(자동마운트) 에 의해 사용되는 디렉터리

/opt : 응용프로그램의 설치를 위한 디렉터리

/proc : 가상 파일 시스템으로서 프로세스, 하드웨어 등의 시스템 정보를 담은 디렉터리
현재 실행중인 프로세스에 대한 정보, 하드웨어에 대한 정보가 덤프형식으로 생성되어 보여줌
메모리에 실행 되어있는 실제 정보를 수집

/sys :  커널이 하드웨어 정보를 기록함

udev가 동작할때 필요한 하드웨어 정보를 제공함

block : 시스템에 있는 각 block device에 대한 정보

 bus : 각 device에 어떤 device driver가 할당되어있는지의 정보

class : device를 역할에 따라 구분

예를들어 /sys/class/net에는 network interface들에 대한 정보가 있고

/sys/class/input에는 키보드나 마우스와 같은 input 장치에 대한 정보가 있다

 devices  : 시스템의 장치에 대하여 어떻게 연결되어있는지에 대한 종합 hierarchy 정보

 module   : 각 로드 되어있는 커널 모듈에 대한 정보를 모듈 파라메터 값 등의 디렉토리로 저장하고 있음
 
/root : root 사용자의 홈디렉터리 입니다.

/sbin : System Binary 약자로서, 시스템 관리에 대한 명령어들이 들어있습니다. 주로 root가 사용하는 명령어들 입니다.

/tmp : 임시 저장 디렉터리 입니다. 프로그램, 프로세스 등의 작업을 할 때 임시로 생성되는 파일을 저장하는 공간 입니다.

/usr : 시스템 운영에 관련된 명령과 각종 응용프로그램들이 위치하는 디렉터리 입니다. 대표적으로 apache, mysql, php 뿐만 아니라 각종 개발도구 커널소스등도 포함됩니다.

/var : 시스템 운영로그 파일과 스풀링을 보관하는 디렉터리 입니다. 메일을 운영할 경우에는 수신메일이 이 디렉터리의 하위 디렉터리에 저장됩니다.




