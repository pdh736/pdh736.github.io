---
title: "package download"
excerpt: "How to download package"

categories:
  - linux_etc

tags:
  - linux
  - package
  - download

toc: true
toc_sticky: true

date: 2020-12-22
last_modified_at: 2020-12-22
---
# package download

## centos
yumdownloader 사용
설치
`yum install yum-utils`
 
### option
--resolve 의존성 패키지 다운
--destdir 저장되는 디렉토리 지정

### 사용
```
yumdownloader --resolve [package name]
```

## ubuntu
```
apt-get download $(apt-rdepends "${package}" | grep -v ^\ )
```
