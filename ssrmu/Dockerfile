FROM python:alpine3.7
MAINTAINER FAN VINGA<fanalcest@gmail.com>

ENV DOMAIN=your_domain \
    MUKEY=your_mukey \
    NODEID=3 

RUN  apk update && apk add --virtual .build-deps \
                        git \
                        tar \          
                        autoconf \
                        automake \
                        make \
                        build-base \
                        linux-headers && \
     apk add --virtual runtime-tools \
                        curl \
                        libsodium-dev \
                        openssl-dev \
                        udns-dev \
                        mbedtls-dev \
                        pcre-dev \
                        libev-dev \
                        libtool \
                        supervisor \
                        libffi-dev && \
     git clone -b manyuser https://github.com/glzjin/shadowsocks.git "/root/shadowsocks" --depth 1 && cd /root/shadowsocks && \
     pip install --no-cache-dir -r requirements.txt && \
     cp apiconfig.py userapiconfig.py && \
     cp config.json user-config.json && \
     apk del .build-deps && rm -rf /var/cache/apk/*

WORKDIR /root/shadowsocks
ADD supervisord.conf /etc/supervisord.conf

CMD sed -i "s#https://zhaoj.in#https://${DOMAIN}#" /root/shadowsocks/userapiconfig.py && \
    sed -i "s#glzjin#${MUKEY}#" /root/shadowsocks/userapiconfig.py && \
    sed -i "s#NODE_ID\ =\ 1#NODE_ID\ =\ ${NODEID}#" /root/shadowsocks/userapiconfig.py && \
    supervisord -c /etc/supervisord.conf
