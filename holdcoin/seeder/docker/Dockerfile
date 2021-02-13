FROM alpine:3.13 AS builder

RUN apk add --no-cache \
    autoconf \
    automake \
    boost-dev \
    build-base \
    gcc \
    git \
    libevent-dev \
    libtool

WORKDIR '/root'
RUN git clone --depth 1 https://github.com/efokschaner/holdcoin.git -b v0.21.0-holdcoin.0.0.2
WORKDIR '/root/holdcoin'
RUN ./autogen.sh
RUN ./configure --disable-wallet --without-gui --without-miniupnpc --disable-bench --disable-tests
RUN make install DESTDIR=/opt/holdcoin -j "$(($(nproc)+1))"

FROM alpine:3.13

RUN apk add --no-cache libevent boost boost-system boost-filesystem boost-thread musl

COPY bitcoin.conf /root/.holdcoin/bitcoin.conf
COPY --from=builder /opt/holdcoin/usr/local /opt/holdcoin
EXPOSE 8565
CMD ["/opt/holdcoin/bin/bitcoind"]