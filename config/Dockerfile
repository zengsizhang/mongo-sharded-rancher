FROM alpine:3.7@sha256:ccba511b1d6b5f1d83825a94f9d5b05528db456d9cf14a1ea1db892c939cda64

LABEL maintainer "Leonardo Gatica <lgatica@protonmail.com>"

ENV GIDDYUP_VERSION=0.19.0 CURL_VERSION=7.56.1

RUN mkdir -p /opt/rancher/bin
RUN apk add --no-cache curl && \
  mkdir -p /usr/local/etc/redis && \
  curl -sL -o /opt/rancher/bin/giddyup https://github.com/rancher/giddyup/releases/download/v${GIDDYUP_VERSION}/giddyup && \
  apk add --update --no-cache --virtual curldeps g++ make perl openssl openssl-dev ca-certificates && \
  wget https://curl.haxx.se/download/curl-$CURL_VERSION.tar.bz2 && \
  tar xjvf curl-$CURL_VERSION.tar.bz2 && \
  rm curl-$CURL_VERSION.tar.bz2 && \
  cd curl-$CURL_VERSION && \
  ./configure \
      --prefix=/tmp/curl \
      --with-ssl \
      --enable-ipv6 \
      --enable-unix-sockets \
      --without-libidn \
      --disable-shared \
      --enable-static \
      --disable-ldap \
      --with-pic && \
  make && \
  rm src/curl && \
  make LDFLAGS=-all-static && \
  make install && \
  cp /tmp/curl/bin/curl /opt/rancher/bin/curl && \
  rm -rf /tmp/curl && \
  apk del curl curldeps
COPY entrypoint.sh /entrypoint.sh
COPY scripts/* /opt/rancher/bin/
RUN  chmod +x /entrypoint.sh /opt/rancher/bin/*

ENTRYPOINT ["/entrypoint.sh"]
