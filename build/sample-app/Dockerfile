FROM ubuntu:18.04

MAINTAINER Toshiki Tsuboi <t.tsubo2000@gmail.com>


ENV GO_VERSION 1.21.3
ENV PATH=$PATH:/usr/local/go/bin

# Install go
ADD https://storage.googleapis.com/golang/go$GO_VERSION.linux-amd64.tar.gz /
RUN tar xzvf go$GO_VERSION.linux-amd64.tar.gz
RUN mv /go /usr/local/go

# Install dependencies for go-get
RUN apt-get update \
 && apt-get install -y \
                       bzr \
                       curl \
                       git \
                       mercurial \
                       build-essential \
                       vim \
                       net-tools \
                       iproute2 \
                       iputils-ping \
                       procps \
                       netbase \
 && apt-get clean \
 && rm -rf /var/lib/apt/lists/*

ADD go-intermediate /root/go-intermediate
WORKDIR /root/go-intermediate
RUN go mod tidy