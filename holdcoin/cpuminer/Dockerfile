# Dockerfile for cross compiling https://github.com/pooler/cpuminer for windows
# Compile:
# docker build . -t efokschaner/compile-cpuminer-win
# Extract built files:
# $con_id = docker create efokschaner/compile-cpuminer-win
# docker cp ${con_id}:/opt/cpuminer-win-static ./cpuminer-win-static
# docker rm ${con_id}
# Try the miner with eg.
# .\minerd.exe --algo=sha256d --url=127.0.0.1:8332 --userpass=$(Get-Content ${env:APPDATA}\Holdcoin\.cookie) --coinbase-addr=hc1qs8msnclrzyej7wdakh0jdekk4wu6s9qt3s4njn

FROM ubuntu:20.04

# workaround for interactive TZ installation (see https://grigorkh.medium.com/fix-tzdata-hangs-docker-image-build-cdb52cc3360d)
ENV TZ=Etc/UTC
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

RUN apt-get update -qq && \
    apt-get install -qqy \
    automake \
    gcc \
    gcc-mingw-w64-i686 \
    git \
    libcurl4-openssl-dev \
    libtool \
    make \
    pkg-config
    

# TODO we should probably not use bleeding edge curl
RUN git clone https://github.com/curl/curl.git

RUN cd curl && \
    ./buildconf && \
    ./configure CFLAGS="-O3" --build x86_64-pc-linux-gnu --host i686-w64-mingw32 --prefix=/usr/i686-w64-mingw32 --disable-ldap --disable-ldaps && \
    make install

RUN git clone https://github.com/pooler/cpuminer

RUN cd cpuminer && \
    ./autogen.sh && \
    ./configure CFLAGS="-O3" --build x86_64-pc-linux-gnu --host i686-w64-mingw32 --prefix=/usr/i686-w64-mingw32 && \
    make install CFLAGS="-O3 -static -DCURL_STATICLIB" DESTDIR=/opt/cpuminer-win-static
