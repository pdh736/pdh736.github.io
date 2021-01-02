---


---

<hr>
<p>title: “ffmpeg build”<br>
excerpt: “How to build ffmpeg”</p>
<p>categories:</p>
<ul>
<li>study_ffmpeg<br>
tags:</li>
<li>ffmpeg</li>
<li>ffmpeg_build</li>
</ul>
<p>toc: true<br>
toc_sticky: true</p>
<h2 id="date-2020-12-12last_modified_at-2020-12-25">date: 2020-12-12<br>
last_modified_at: 2020-12-25</h2>
<h1 id="ffmpeg-build">ffmpeg build</h1>
<p>출처<br>
<a href="https://trac.ffmpeg.org/wiki/CompilationGuide/Ubuntu">https://trac.ffmpeg.org/wiki/CompilationGuide/Ubuntu</a><br>
<a href="https://trac.ffmpeg.org/wiki/CompilationGuide/Centos">https://trac.ffmpeg.org/wiki/CompilationGuide/Centos</a></p>
<p>아래 디렉토리 생성</p>
<pre><code>//소스파일 받아둘 디렉토리
$HOME/dev/ffmpeg
//빌드하며 라이브러리 설치될 디렉토리
$HOME/dev/ffmpeg/ffmpeg_sources
/실행파일 설치할 디렉토리
$HOME/dev/ffmpeg/ffmpeg_build
$HOME/dev/ffmpeg/bin
export FFMPEG_BUILD=$HOME/dev/ffmpeg/ffmpeg_build
export FFMPEG_BIN=$HOME/dev/ffmpeg/bin
</code></pre>
<p><code>$HOME/dev/ffmpeg/ffmpeg_sources</code> 에서 아래 코덱들 빌드 한 뒤 ffmpeg 빌드</p>
<h2 id="ubuntu">ubuntu</h2>
<h3 id="의존성-라이브러리-설치">의존성 라이브러리 설치</h3>
<pre><code>sudo apt-get update -qq &amp;&amp; sudo apt-get -y install \
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
</code></pre>
<h3 id="nasm">nasm</h3>
<pre><code>cd ~/ffmpeg_sources &amp;&amp; \ wget https://www.nasm.us/pub/nasm/releasebuilds/2.14.02/nasm-2.14.02.tar.bz2
tar xjvf nasm-2.14.02.tar.bz2
cd nasm-2.14.02
./autogen.sh
PATH="$FFMPEG_BIN:$PATH" ./configure --prefix="$FFMPEG_BUILD" --bindir="FFMPEG_BIN"
make &amp;&amp; \ make install
</code></pre>
<h3 id="libx264-h.264-encoder">libx264 (h.264 encoder)</h3>
<pre><code>git -C x264 pull 2&gt; /dev/null || git clone --depth 1 https://code.videolan.org/videolan/x264.git
cd x264
PATH=$FFMPEG_BIN:$PATH" PKG_CONFIG_PATH="$FFMPEG_BUILD/lib/pkgconfig" ./configure --prefix="$FFMPEG_BUILD" --bindir="$FFMPEG_BIN" --enable-static --enable-pic
PATH="$HOME/bin:$PATH" make &amp;&amp; make install
</code></pre>
<h3 id="libx265-h.265-encoder">libx265 (h.265 encoder)</h3>
<pre><code>sudo apt-get install libnuma-dev
git -C x265_git pull 2&gt; /dev/null || git clone https://bitbucket.org/multicoreware/x265_git
cd x265_git/build/linux
PATH="$HOME/bin:$PATH" cmake -G "Unix Makefiles" -DCMAKE_INSTALL_PREFIX="$HOME/ffmpeg_build" -DENABLE_SHARED=off ../../source
PATH="$HOME/bin:$PATH" make &amp;&amp; \ make install
</code></pre>
<h3 id="libvpc-vp8vp9-video-encoderdecode">libvpc (VP8/VP9 video encoder/decode)</h3>
<pre><code>git -C libvpx pull 2&gt; /dev/null || git clone --depth 1 https://chromium.googlesource.com/webm/libvpx.git
cd libvpx &amp;&amp; \ PATH="$FFMPEG_BIN:$PATH" ./configure --prefix="$FFMPEG_BUILD" --disable-examples --disable-unit-tests --enable-vp9-highbitdepth --as=yasm
PATH="$FFMPEG_BIN:$PATH" make &amp;&amp; \ make install
</code></pre>
<h3 id="libfdk-aac-aac-audio-encoder">libfdk-aac (AAC audio encoder)</h3>
<pre><code>git -C fdk-aac pull 2&gt; /dev/null || git clone --depth 1 https://github.com/mstorsjo/fdk-aac
cd fdk-aac
autoreconf -fiv
./configure --prefix="$HOME/ffmpeg_build" --disable-shared
make &amp;&amp; \ make install
</code></pre>
<h3 id="libmp3lame-mp3-audio-encoder">libmp3lame (MP3 audio encoder)</h3>
<p>ubuntu</p>
<pre><code>wget -O lame-3.100.tar.gz https://downloads.sourceforge.net/project/lame/lame/3.100/lame-3.100.tar.gz &amp;&amp; \ tar xzvf lame-3.100.tar.gz
cd lame-3.100
PATH="$HOME/bin:$PATH" ./configure --prefix="$HOME/ffmpeg_build" --bindir="$HOME/bin" --disable-shared --enable-nasm
PATH="$HOME/bin:$PATH" make &amp;&amp; \ make install
</code></pre>
<h3 id="libopus-opus-audio-decoder-and-encoder">libopus (Opus audio decoder and encoder)</h3>
<pre><code>git -C opus pull 2&gt; /dev/null || git clone --depth 1 https://github.com/xiph/opus.git
cd opus
./autogen.sh
./configure --prefix="$FFMPEG_BUILD" --disable-shared
make &amp;&amp; \ make install
</code></pre>
<h3 id="libsvtav1-av1-video-encoderdecoder">libsvtav1 (AV1 video encoder/decoder)</h3>
<p>Requires  ffmpeg  to be configured with  --enable-libsvtav1.<br>
가이드에서 ubuntu에서만 함</p>
<pre><code>git -C SVT-AV1 pull 2&gt; /dev/null || git clone https://github.com/AOMediaCodec/SVT-AV1.git
mkdir -p SVT-AV1/build
cd SVT-AV1/build
PATH="$HOME/bin:$PATH" cmake -G "Unix Makefiles" -DCMAKE_INSTALL_PREFIX="$HOME/ffmpeg_build" -DCMAKE_BUILD_TYPE=Release -DBUILD_DEC=OFF -DBUILD_SHARED_LIBS=OFF ..
PATH="$HOME/bin:$PATH" make &amp;&amp; make install
</code></pre>
<h3 id="ffmpeg">ffmpeg</h3>
<pre><code>sudo apt-get install libunistring-dev

wget -O ffmpeg-snapshot.tar.bz2 https://ffmpeg.org/releases/ffmpeg-snapshot.tar.bz2
tar xjvf ffmpeg-snapshot.tar.bz2 &amp;&amp; cd ffmpeg 
PATH="$FFMPEG_BIN:$PATH" PKG_CONFIG_PATH="$FFMPEG_BUILD/lib/pkgconfig" ./configure \
  --prefix="$FFMPEG_BUILD" \
  --pkg-config-flags="--static" \
  --extra-cflags="-I$FFMPEG_BUILD/include" \
  --extra-ldflags="-L$FFMPEG_BUILD/lib" \
  --extra-libs="-lpthread -lm" \
  --bindir="$FFMPEG_BIN" \
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
  PATH="$FFMPEG_BIN:$PATH" make &amp;&amp; make install
  hash -r
</code></pre>
<h2 id="centos">centos</h2>
<h3 id="의존성-라이브러리-설치-1">의존성 라이브러리 설치</h3>
<pre><code># yum install autoconf automake bzip2 bzip2-devel cmake freetype-devel gcc gcc-c++ git libtool make mercurial pkgconfig zlib-devel
</code></pre>
<h3 id="nasm-1">nasm</h3>
<pre><code>curl -O -L https://www.nasm.us/pub/nasm/releasebuilds/2.14.02/nasm-2.14.02.tar.bz2
tar xjvf nasm-2.14.02.tar.bz2
cd nasm-2.14.02
./autogen.sh
./configure --prefix="$FFMPEG_BUILD" --bindir="$FFMPEG_BIN"
make
make install
</code></pre>
<h3 id="yasm">yasm</h3>
<p>centos 일경우 yasm 도 빌드 할것</p>
<pre><code>curl -O -L https://www.tortall.net/projects/yasm/releases/yasm-1.3.0.tar.gz
tar xzvf yasm-1.3.0.tar.gz
cd yasm-1.3.0
./configure --prefix="$FFMPGE_BUILD" --bindir="$FFMPEG_BIN"
make
make install
</code></pre>
<h3 id="libx264-h.264-encoder-1">libx264 (h.264 encoder)</h3>
<pre><code>git clone --depth 1 https://code.videolan.org/videolan/x264.git
cd x264
PKG_CONFIG_PATH="$FFMPEG_BUILD/lib/pkgconfig" ./configure --prefix="$FFMPEG_BUILD" --bindir="$FFMPEG_BIN" --enable-static
make
make install
</code></pre>
<h3 id="libx265-h.265-encoder-1">libx265 (h.265 encoder)</h3>
<pre><code>hg clone https://bitbucket.org/multicoreware/x265
cd ~/ffmpeg_sources/x265/build/linux
cmake -G "Unix Makefiles" -DCMAKE_INSTALL_PREFIX="$FFMPEG_BUILD" -DENABLE_SHARED:bool=off ../../source
make
make install
</code></pre>
<h3 id="libvpc-vp8vp9-video-encoderdecode-1">libvpc (VP8/VP9 video encoder/decode)</h3>
<pre><code>cd ~/ffmpeg_sources
git clone --depth 1 https://chromium.googlesource.com/webm/libvpx.git
cd libvpx
./configure --prefix="$FFMPEG_BUILD" --disable-examples --disable-unit-tests --enable-vp9-highbitdepth --as=yasm
make
make install
</code></pre>
<h3 id="libfdk-aac-aac-audio-encoder-1">libfdk-aac (AAC audio encoder)</h3>
<pre><code>cd ~/ffmpeg_sources
git clone --depth 1 https://github.com/mstorsjo/fdk-aac
cd fdk-aac
autoreconf -fiv
./configure --prefix="$FFMPEG_BUILD" --disable-shared
make
make install
</code></pre>
<h3 id="libmp3lame-mp3-audio-encoder-1">libmp3lame (MP3 audio encoder)</h3>
<pre><code>curl -O -L https://downloads.sourceforge.net/project/lame/lame/3.100/lame-3.100.tar.gz
tar xzvf lame-3.100.tar.gz
cd lame-3.100
./configure --prefix="$FFMPEG_BUILD" --bindir="$FFMPEG_BIN" --disable-shared --enable-nasm
make
make install
</code></pre>
<h3 id="libopus-opus-audio-decoder-and-encoder-1">libopus (Opus audio decoder and encoder)</h3>
<pre><code>cd ~/ffmpeg_sources
curl -O -L https://archive.mozilla.org/pub/opus/opus-1.3.1.tar.gz
tar xzvf opus-1.3.1.tar.gz
cd opus-1.3.1
./configure --prefix="$FFMPEG_BUILD" --disable-shared
make
make install
</code></pre>
<h3 id="ffmpeg-1">ffmpeg</h3>
<pre><code>curl -O -L https://ffmpeg.org/releases/ffmpeg-snapshot.tar.bz2
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
</code></pre>

