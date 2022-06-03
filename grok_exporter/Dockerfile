FROM alpine:3.16 as builder

RUN apk update && apk add --no-cache curl build-base make git 

RUN curl -sLO https://github.com/kkos/oniguruma/releases/download/v6.9.5_rev1/onig-6.9.5-rev1.tar.gz && \
    tar xfz onig-6.9.5-rev1.tar.gz && \
    cd onig-6.9.5 && \
    ./configure && \
    make && \
    make install

COPY --from=golang:1.18-alpine /usr/local/go/ /usr/local/go/
ENV PATH="/usr/local/go/bin:${PATH}"

RUN git clone https://github.com/fstab/grok_exporter
RUN cd grok_exporter && \
    git submodule update --init --recursive && \
    git -C /grok_exporter/logstash-patterns-core checkout main && \
    GOBIN=/usr/local/bin/ go install .

FROM alpine:3.16

COPY --from=builder /usr/local/include/onig* /usr/local/include/
COPY --from=builder /usr/local/lib/libonig* /usr/local/lib/
COPY --from=builder /usr/local/bin/grok_exporter /usr/local/bin/
COPY --from=builder /grok_exporter /grok_exporter

COPY patterns/httpd /grok_exporter/logstash-patterns-core/patterns/legacy
