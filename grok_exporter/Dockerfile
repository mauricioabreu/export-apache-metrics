FROM ubuntu:20.04

ARG DEBIAN_FRONTEND=noninteractive

RUN apt-get update && apt-get install -y curl libonig-dev gcc make git

RUN curl -sLO https://github.com/kkos/oniguruma/releases/download/v6.9.5_rev1/onig-6.9.5-rev1.tar.gz && \
    tar xfz onig-6.9.5-rev1.tar.gz && \
    cd onig-6.9.5 && \
    ./configure && \
    make && \
    make install

RUN apt-get install -y golang
RUN git clone https://github.com/fstab/grok_exporter
RUN cd grok_exporter && \
    git submodule update --init --recursive && \
    go install .