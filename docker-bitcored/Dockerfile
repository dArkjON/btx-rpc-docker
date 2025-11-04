FROM ubuntu:18.04
LABEL maintainer="bitcore-btx"

COPY ./bin /usr/local/bin
COPY ./VERSION /tmp
COPY ./CHECKSUM /tmp

RUN apt-get update && \
    apt-get install -y --no-install-recommends \
    wget ca-certificates unzip software-properties-common \
    libboost-system1.65.1 libboost-filesystem1.65.1 \
    libboost-thread1.65.1 libboost-chrono1.65.1 \
    libssl1.1 libminiupnpc10 libevent-2.1-6 libevent-pthreads-2.1-6 \
    libzmq5
RUN VERSION=`cat /tmp/VERSION` && \
    wget https://github.com/bitcore-btx/BitCore/releases/download/${VERSION}/bitcore-x86_64-linux-gnu_no-wallet.zip -O /tmp/prebuilt.tar.xz
RUN sha256sum /tmp/prebuilt.tar.xz && \
    echo "$(cat /tmp/CHECKSUM)  /tmp/prebuilt.tar.xz" | sha256sum -c
RUN unzip -q /tmp/prebuilt.tar.xz -d /tmp/
RUN mv /tmp/bitcored /usr/local/bin && \
    mv /tmp/bitcore-cli /usr/local/bin && \
    chmod a+x /usr/local/bin/*
RUN apt-get -y autoremove && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

VOLUME ["/data"]
ENV HOME /data
ENV DATA /data
WORKDIR /data

EXPOSE 8555 8556

ENTRYPOINT ["init"]
