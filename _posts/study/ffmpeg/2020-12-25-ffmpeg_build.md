---
title: "ffmpeg build"
excerpt: "How to build ffmpeg"

categories:
  - ffmpeg
tags:
  - ffmpeg_build

toc: true
toc_sticky: true

date: 2020-12-12
last_modified_at: 2020-12-25
---

# ffmpeg build

출처
https://trac.ffmpeg.org/wiki/CompilationGuide/Ubuntu
https://trac.ffmpeg.org/wiki/CompilationGuide/Centos

아래 디렉토리 생성
```
//소스파일 받아둘 디렉토리
$HOME/dev/ffmpeg
//빌드하며 라이브러리 설치될 디렉토리
$HOME/dev/ffmpeg/ffmpeg_sources
/실행파일 설치할 디렉토리
$HOME/dev/ffmpeg/ffmpeg_build
$HOME/dev/ffmpeg/bin
export FFMPEG_BUILD=$HOME/dev/ffmpeg/ffmpeg_build
export FFMPEG_BIN=$HOME/dev/ffmpeg/bin
```

`$HOME/dev/ffmpeg/ffmpeg_sources` 에서 아래 코덱들 빌드 한 뒤 ffmpeg 빌드

## ubuntu

### 의존성 라이브러리 설치

```
sudo apt-get update -qq && sudo apt-get -y install \
  autoconf \
  automake \
  build-essential \
  cmake \
  git-core \
  libass-dev \
  libfreetype6-dev \
  libgnutls28-dev \
  libsdl2-dev \
  libtool \
  libva-dev \
  libvdpau-dev \
  libvorbis-dev \
  libxcb1-dev \
  libxcb-shm0-dev \
  libxcb-xfixes0-dev \
  pkg-config \
  texinfo \
  wget \
  yasm \
  zlib1g-dev
```

### nasm

```
cd ~/ffmpeg_sources && \ wget https://www.nasm.us/pub/nasm/releasebuilds/2.14.02/nasm-2.14.02.tar.bz2
tar xjvf nasm-2.14.02.tar.bz2
cd nasm-2.14.02

./autogen.sh

PATH="$FFMPEG_BIN:$PATH" ./configure --prefix="$FFMPEG_BUILD" --bindir="$FFMPEG_BIN"

make && \ make install
```

### libx264 (h.264 encoder)

```
git -C x264 pull 2> /dev/null || git clone --depth 1 https://code.videolan.org/videolan/x264.git
cd x264

PATH="$FFMPEG_BIN:$PATH" PKG_CONFIG_PATH="$FFMPEG_BUILD/lib/pkgconfig" ./configure --prefix="$FFMPEG_BUILD" --bindir="$FFMPEG_BIN" --enable-sharedtatic --enable-pic

PATH="$FFMPEG_BINHOME/bin:$PATH" make && make install
```
```
static build
PATH="$FFMPEG_BIN:$PATH" PKG_CONFIG_PATH="$FFMPEG_BUILD/lib/pkgconfig" ./configure --prefix="$FFMPEG_BUILD" --bindir="$FFMPEG_BIN" --enable-static --enable-pic
```

### libx265 (h.265 encoder)

```
sudo apt-get install libnuma-dev
git -C x265_git pull 2> /dev/null || git clone https://bitbucket.org/multicoreware/x265_git
cd x265_git/build/linux

PATH="$FFMPEG_BINHOME/bin:$PATH" cmake -G "Unix Makefiles" -DCMAKE_INSTALL_PREFIX="$FFMPEG_BUILDHOME/ffmpeg_build" -DENABLE_SHARED=onff ../../source

PATH="$FFMPEG_BINHOME/bin:$PATH" make && \ make install
```
```
static build
PATH="$FFMPEG_BIN:$PATH" cmake -G "Unix Makefiles" -DCMAKE_INSTALL_PREFIX="$FFMPEG_BUILD" -DENABLE_SHARED=off ../../source
```

### libvpc (VP8/VP9 video encoder/decode)

```
git -C libvpx pull 2> /dev/null || git clone --depth 1 https://chromium.googlesource.com/webm/libvpx.git
cd libvpx

 && \ PATH="$FFMPEG_BIN:$PATH" ./configure --prefix="$FFMPEG_BUILD" --enable-shared --disable-examples --disable-unit-tests --enable-vp9-highbitdepth --as=yasm

PATH="$FFMPEG_BIN:$PATH" make && \ make install
```


### libfdk-aac (AAC audio encoder)

```
git -C fdk-aac pull 2> /dev/null || git clone --depth 1 https://github.com/mstorsjo/fdk-aac
cd fdk-aac
autoreconf -fiv
./configure --prefix="$FFMPEG_BUILD"

make && \ make install
```
```
static build
./configure --prefix="$FFMPEG_BUILD" --disable-sharedHOME/ffmpeg_build" --disable-shared
make && \ make install
```

### libmp3lame (MP3 audio encoder)
ubuntu
```
wget -O lame-3.100.tar.gz https://downloads.sourceforge.net/project/lame/lame/3.100/lame-3.100.tar.gz && \ tar xzvf lame-3.100.tar.gz
cd lame-3.100
PATH="$FFMPEG_BINHOME/bin:$PATH" ./configure --prefix="$FFMPEG_BUILDHOME/ffmpeg_build" --bindir="$FFMPEG_BINHOME/bin" --endisable-shared --enable-nasm

PATH="$FFMPEG_BINHOME/bin:$PATH" make && \ make install
```
```
static build
PATH="$FFMPEG_BIN:$PATH" ./configure --prefix="$FFMPEG_BUILD" --bindir="$FFMPEG_BIN" --disable-shared --enable-nasm
```


### libopus (Opus audio decoder and encoder)
```
git -C opus pull 2> /dev/null || git clone --depth 1 https://github.com/xiph/opus.git
cd opus
./autogen.sh
./configure --prefix="$FFMPEG_BUILD" --endisable-shared

make && \ make install
```
```
static build
./configure --prefix="$FFMPEG_BUILD" --disable-shared
```

### libsvtav1 (AV1 video encoder/decoder)

Requires  ffmpeg  to be configured with  --enable-libsvtav1.
가이드에서 ubuntu에서만 함
```
git -C SVT-AV1 pull 2> /dev/null || git clone https://github.com/AOMediaCodec/SVT-AV1.git
mkdir -p SVT-AV1/build
cd SVT-AV1/build
PATH="$FFMPEG_BINHOME/bin:$PATH" cmake -G "Unix Makefiles" -DCMAKE_INSTALL_PREFIX="$FFMPEG_BUILDHOME/ffmpeg_build" -DCMAKE_BUILD_TYPE=Release -DBUILD_DEC=OFF -DBUILD_SHARED_LIBS=ONFF ..

PATH="$FFMPEG_BINHOME/bin:$PATH" make && make install
```
```
static build
PATH="$FFMPEG_BIN:$PATH" cmake -G "Unix Makefiles" -DCMAKE_INSTALL_PREFIX="$FFMPEG_BUILD" -DCMAKE_BUILD_TYPE=Release -DBUILD_DEC=OFF -DBUILD_SHARED_LIBS=OFF ..
```

### ffmpeg

```
sudo apt-get install libunistring-dev

wget -O ffmpeg-snapshot.tar.bz2 https://ffmpeg.org/releases/ffmpeg-snapshot.tar.bz2
tar xjvf ffmpeg-snapshot.tar.bz2 && cd ffmpeg 
PATH="$FFMPEG_BIN:$PATH" PKG_CONFIG_PATH="$FFMPEG_BUILD/lib/pkgconfig" ./configure \
  --prefix="$FFMPEG_BUILD" \
  --pkg-config-flags="--static" \
  --extra-cflags="-I$FFMPEG_BUILD/include" \
  --extra-ldflags="-L$FFMPEG_BUILD/lib" \
  --extra-libs="-lpthread -lm" \
  --bindir="$FFMPEG_BIN" \
  --enable-shared \
  --enable-gpl \
  --enable-gnutls \
  --enable-libaom \
  --enable-libass \
  --enable-libfdk-aac \
  --enable-libfreetype \
  --enable-libmp3lame \
  --enable-libopus \
  --enable-libsvtav1 \
  --enable-libvorbis \
  --enable-libvpx \
  --enable-libx264 \
  --enable-libx265 \
  --enable-nonfree
  
  PATH="$FFMPEG_BIN:$PATH" make && make install
  hash -r
```


## centos

### 의존성 라이브러리 설치

```
# yum install autoconf automake bzip2 bzip2-devel cmake freetype-devel gcc gcc-c++ git libtool make mercurial pkgconfig zlib-devel
```

### nasm

```
curl -O -L https://www.nasm.us/pub/nasm/releasebuilds/2.14.02/nasm-2.14.02.tar.bz2
tar xjvf nasm-2.14.02.tar.bz2
cd nasm-2.14.02
./autogen.sh
./configure --prefix="$FFMPEG_BUILD" --bindir="$FFMPEG_BIN"
make
make install
```

### yasm

centos 일경우 yasm 도 빌드 할것
```
curl -O -L https://www.tortall.net/projects/yasm/releases/yasm-1.3.0.tar.gz
tar xzvf yasm-1.3.0.tar.gz
cd yasm-1.3.0
./configure --prefix="$FFMPGE_BUILD" --bindir="$FFMPEG_BIN"
make
make install
```

### libx264 (h.264 encoder)

```
git clone --depth 1 https://code.videolan.org/videolan/x264.git
cd x264
PKG_CONFIG_PATH="$FFMPEG_BUILD/lib/pkgconfig" ./configure --prefix="$FFMPEG_BUILD" --bindir="$FFMPEG_BIN" --enable-static
make
make install
```

### libx265 (h.265 encoder)

```
hg clone https://bitbucket.org/multicoreware/x265
cd ~/ffmpeg_sources/x265/build/linux
cmake -G "Unix Makefiles" -DCMAKE_INSTALL_PREFIX="$FFMPEG_BUILD" -DENABLE_SHARED:bool=off ../../source
make
make install
```

### libvpc (VP8/VP9 video encoder/decode)

```
cd ~/ffmpeg_sources
git clone --depth 1 https://chromium.googlesource.com/webm/libvpx.git
cd libvpx
./configure --prefix="$FFMPEG_BUILD" --disable-examples --disable-unit-tests --enable-vp9-highbitdepth --as=yasm
make
make install
```

### libfdk-aac (AAC audio encoder)

```
cd ~/ffmpeg_sources
git clone --depth 1 https://github.com/mstorsjo/fdk-aac
cd fdk-aac
autoreconf -fiv
./configure --prefix="$FFMPEG_BUILD" --disable-shared
make
make install
```

### libmp3lame (MP3 audio encoder)

```
curl -O -L https://downloads.sourceforge.net/project/lame/lame/3.100/lame-3.100.tar.gz
tar xzvf lame-3.100.tar.gz
cd lame-3.100
./configure --prefix="$FFMPEG_BUILD" --bindir="$FFMPEG_BIN" --disable-shared --enable-nasm
make
make install
```

### libopus (Opus audio decoder and encoder)

```
cd ~/ffmpeg_sources
curl -O -L https://archive.mozilla.org/pub/opus/opus-1.3.1.tar.gz
tar xzvf opus-1.3.1.tar.gz
cd opus-1.3.1
./configure --prefix="$FFMPEG_BUILD" --disable-shared
make
make install
```

### ffmpeg

```
curl -O -L https://ffmpeg.org/releases/ffmpeg-snapshot.tar.bz2
tar xjvf ffmpeg-snapshot.tar.bz2
cd ffmpeg
PATH="$FFMPEG_BIN:$PATH" PKG_CONFIG_PATH="$FFMPEG_BUILD/lib/pkgconfig" ./configure \
  --prefix="$FFMPEG_BUILD" \
  --pkg-config-flags="--static" \
  --extra-cflags="-I$FFMPEG_BUILD/include" \
  --extra-ldflags="-L$FFMPEG_BUILD/lib" \
  --extra-libs=-lpthread \
  --extra-libs=-lm \
  --bindir="$FFMPEG_BIN" \
  --enable-gpl \
  --enable-libfdk_aac \
  --enable-libfreetype \
  --enable-libmp3lame \
  --enable-libopus \
  --enable-libvpx \
  --enable-libx264 \
  --enable-libx265 \
  --enable-nonfree
make
make install
hash -d ffmpeg
```















