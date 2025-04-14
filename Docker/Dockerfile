FROM golang:1.14-buster AS easy-novnc-build
WORKDIR /src
RUN go mod init build && \
    go get github.com/geek1011/easy-novnc@v1.1.0 && \
    go build -o /bin/easy-novnc github.com/geek1011/easy-novnc

#FROM ubuntu:latest AS dogecoincore-prep
FROM debian:stable-slim AS dogecoincore-prep

RUN apt-get update -y && apt-get install -y --no-install-recommends wget ca-certificates \
    && mkdir -p /opt/dcc_dl \
    && mkdir -p /opt/dogecoincore \
    && cd /opt/dcc_dl \
    && wget -q https://github.com/dogecoin/dogecoin/releases/download/v1.14.9/dogecoin-1.14.9-x86_64-linux-gnu.tar.gz \
    && tar -xf dogecoin-1.14.9-x86_64-linux-gnu.tar.gz \
    && mv dogecoin-1.14.9/* /opt/dogecoincore/

#FROM debian:buster
FROM debian:stable-slim

RUN apt-get update -y && \
    apt-get install -y --no-install-recommends openbox tigervnc-standalone-server supervisor gosu && \
    rm -rf /var/lib/apt/lists && \
    mkdir -p /usr/share/desktop-directories

RUN apt-get update -y && \
    apt-get install -y --no-install-recommends lxterminal nano wget libxkbcommon-x11-0 && \
    rm -rf /var/lib/apt/lists

COPY --from=easy-novnc-build /bin/easy-novnc /usr/local/bin/
COPY --from=dogecoincore-prep /opt/dogecoincore /opt/dogecoincore
COPY menu.xml /etc/xdg/openbox/
COPY supervisord.conf /etc/

EXPOSE 8080

VOLUME /config
VOLUME /data

CMD ["sh", "-c", "chown 0:0 /dev/stdout /config /data && exec gosu root supervisord"]
