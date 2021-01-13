---
title: "grpc build"
excerpt: "How to build grpc"

categories:
  - grpc

tags:
  - grpc_build

toc: true
toc_sticky: true

date: 2020-11-26
last_modified_at: 2021-01-13
---

# how to grpc build
## build dependency

### ubuntu
```
$ [sudo] apt-get install build-essential autoconf libtool pkg-config
$ [sudo] apt-get install 
```

### centos
```
$sudo yum install kernel-devel kernel-headers autoconf libtool pkgconfig
//빌드 안되면yum groupinstall "Development Tools" 
```

## pre - build

### 인스톨 디렉토리 지정
```
$ export MY_INSTALL_DIR=$HOME/dev/grpc/install
$ mkdir -p $MY_INSTALL_DIR
//환경변수 추가
$ export PATH="$PATH:$MY_INSTALL_DIR/bin"
```
### 소스 다운
```
$ git clone --recurse-submodules -b v1.33.2 https://github.com/grpc/grpc
$ cd grpc
or
//RELEASE_TAG check https://github.com/grpc/grpc/releases 
//(2020-11-26 기준 최근 릴리즈 버젼 v1.33.2) 
$ git clone -b RELEASE_TAG https://github.com/grpc/grpc 
$ cd grpc
$ git submodule update --init
```

## build
cmake 사용 빌드
```
$ mkdir -p cmake/build
$ cd cmake/build
```

### build
```
$ cmake ../..
$ make
```

### 모든 필수 도구 빌드 및 install
```
$ cmake -DgRPC_INSTALL=ON \
      -DgRPC_BUILD_TESTS=OFF \
      -DCMAKE_INSTALL_PREFIX=$MY_INSTALL_DIR \
      ../..
$ make 
//or make -j
$ make install

ex
 cmake -DgRPC_INSTALL=ON \
       -DgRPC_BUILD_TESTS=OFF \
       -DgRPC_SSL_PROVIDER=package \
       -DOPENSSL_ROOT_DIR="/home/pdh/dev/dpro3/out/linux" \
       -DBUILD_SHARED_LIBS=ON \
       -DCMAKE_BUILD_TYPE=Release \
       -DCMAKE_INSTALL_PREFIX=$INSTALL_PATH \
       ../..
 make -j4 install
```
PROVIDER=package 일경우 설치된 패키지중에서 찾아서 사용
module 일 경우 gprc서브모듈에서 빌드해서 사용(CMakeList.txt, cmake/XXX.cmake 참고)
PROVIDER=package && ROOT_DIR 있을경우 ROOT_DIR의 include, lib사용

공유라이브러리로 빌드 할 경우
`-DBUILD_SHARED_LIBS=ON`
추가할것

## cross-compile
버그인지 사전에 x86빌드 되어 있어야 컴파일 가능
PATH에 x86 ver protoc있는 디렉토리 추가해 주고 아래 따라 진행

hisi.cmake 생성
``` shell
SET(CMAKE_SYSTEM_NAME Linux)
SET(CMAKE_SYSTEM_PROCESSOR arm)
set(CMAKE_C_COMPILER /opt/hisi-linux/x86-arm/arm-hisiv600-linux/bin/arm-hisiv600-linux-gnueabi-gcc)
set(CMAKE_CXX_COMPILER /opt/hisi-linux/x86-arm/arm-hisiv600-linux/bin/arm-hisiv600-linux-gnueabi-g++)
set(CMAKE_FIND_ROOT_PATH_MODE_PROGRAM NEVER)
set(CMAKE_FIND_ROOT_PATH_MODE_LIBRARY ONLY)
set(CMAKE_FIND_ROOT_PATH_MODE_INCLUDE ONLY)
set(CMAKE_FIND_ROOT_PATH_MODE_PACKAGE ONLY)
```

sub module package 경로 지정해서 할경우 아래 4줄은 사용하지 말것
```
ex
SET(CMAKE_SYSTEM_NAME Linux)
SET(CMAKE_SYSTEM_PROCESSOR arm)
set(tool_root /opt/hisi-linux/x86-arm)
set(CMAKE_SYSROOT ${tool_root}/arm-hisiv600-linux/target)
set(CMAKE_C_COMPILER ${tool_root}/arm-hisiv600-linux/target/bin/arm-hisiv600-linux-gcc)
set(CMAKE_CXX_COMPILER ${tool_root}/arm-hisiv600-linux/target/bin/arm-hisiv600-linux-g++)
# should make sym link libstdc++ at cross compiler dir
link_directories(${tool_root}/arm-hisiv600-linux/target/lib)
link_libraries(stdc++)
```
grpc/cmake 아래 빌드할 디렉토리 만든뒤 
그 디렉토리에서 실행
```
cmake -DCMAKE_TOOLCHAIN_FILE=/home/pdh/dev/grpc/hisi.cmake \
      -DCMAKE_BUILD_TYPE=Release \
      -DCMAKE_INSTALL_PREFIX=/home/pdh/dev/grpc/install_arm \
      ../..
```

## helloworld 예제 빌드
```
$ cd examples/cpp/helloworld
$ mkdir -p cmake/build
$ pushd cmake/build
$ cmake -DCMAKE_PREFIX_PATH=$MY_INSTALL_DIR ../..
$ make
```
(공유라이브러리로 빌드 했을경우 LD_LIBRARY_PATH 지정할것)

## etc
참고
grpc static library build 하여 나온 것들 shared library 1개로 묶어서 사용
```
arm-hisiv600-linux-g++ -shared -o libtest_grpc.so -Wl,--whole-archive \
    libabsl_bad_optional_access.a libabsl_int128.a libabsl_throw_delegate.a libabsl_bad_variant_access.a\
    libabsl_log_severity.a libabsl_time.a libabsl_base.a libabsl_malloc_internal.a libabsl_time_zone.a\
    libabsl_city.a libabsl_raw_hash_set.a libaddress_sorting.a libabsl_civil_time.a libabsl_raw_logging_internal.a\
    libabsl_cord.a libabsl_spinlock_wait.a libabsl_debugging_internal.a libabsl_stacktrace.a libgpr.a\
    libabsl_demangle_internal.a libabsl_status.a libabsl_dynamic_annotations.a libabsl_str_format_internal.a\
    libre2.a libabsl_exponential_biased.a libabsl_strings.a libupb.a libabsl_graphcycles_internal.a \
    libabsl_strings_internal.a libabsl_hash.a libabsl_symbolize.a libabsl_hashtablez_sampler.a libabsl_synchronization.a \
    libprotobuf.a libprotoc.a libgrpc++.a libgrpc.a libz.a libcares.a\
    -Wl,--no-whole-archive
```
공유라이브러리들을 묶는것은 힘들지만 아카이브는 쉽게 묶을수 있음

