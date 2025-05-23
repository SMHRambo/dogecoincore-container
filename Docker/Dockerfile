FROM ghcr.io/linuxserver/baseimage-kasmvnc:debianbookworm

# set version label
ARG BUILD_DATE
ARG DOGECOINCORE_VERSION
ARG VERSION
LABEL build_version="Linuxserver.io version:- ${VERSION} Build-date:- ${BUILD_DATE}"
LABEL maintainer="smhrambo"

# title
ENV TITLE=DogeCoin-Core

RUN \
  echo "**** add icon ****" && \
  curl -o \
    /kclient/public/icon.png \
    https://raw.githubusercontent.com/dogecoin/dogecoin/refs/heads/master/share/pixmaps/dogecoin64.png && \
  echo "**** install packages ****" && \
  apt-get update && \
  apt-get install --no-install-recommends -y \
    libxkbcommon-x11-0 \
    xfce4-terminal && \
  echo "**** install DogeCoin-Core ****" && \
  if [ -z ${DOGECOINCORE_VERSION+x} ]; then \
    DOGECOINCORE_VERSION=$(curl -sX GET "https://api.github.com/repos/dogecoin/dogecoin/releases/latest" \
    | awk '/tag_name/{print $4;exit}' FS='[""]' | sed 's/v//g'); \
  fi && \
  curl -o \
    /tmp/DOGECOINCORE.tar.gz -L \
    "https://github.com/dogecoin/dogecoin/releases/download/v${DOGECOINCORE_VERSION}/dogecoin-${DOGECOINCORE_VERSION#release-}-x86_64-linux-gnu.tar.gz" && \
  tar -xf /tmp/DOGECOINCORE.tar.gz -C /tmp && \
  mv \
    /tmp/dogecoin-${DOGECOINCORE_VERSION}/* \
    /app/ && \
  echo "**** container tweaks ****" && \
  ln -s \
    /usr/bin/xfce4-terminal \
    /usr/bin/gnome-terminal && \
  echo "**** cleanup ****" && \
  apt-get autoclean && \
  rm -rf \
    /var/lib/apt/lists/* \
    /var/tmp/* \
    /tmp/*

# add local files
COPY /root /

# ports and volumes
EXPOSE 3000 22556

VOLUME /config
