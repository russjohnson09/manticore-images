#docker build -t core-base-1 .


FROM buildpack-deps:stretch

MAINTAINER Chris Rokita version:1.0

ARG CORE_VERSION=master
ARG DBUILD_AVAHI_SUPPORT=OFF
ARG DBUILD_USB_SUPPORT=OFF
ARG DBUILD_BT_SUPPORT=OFF
ARG DEXTENDED_MEDIA_MODE=OFF
ARG DENABLE_HMI_PTU_DECRYPTION=OFF

ENV CORE_NETWORK_INTERFACE eth0
ENV CORE_VERSION=${CORE_VERSION}

# Install dependencies
RUN apt-get update && apt-get install -y --no-install-recommends \
        build-essential \
        cmake \
        libudev-dev \
        libsqlite3-dev \
        libssl1.0-dev \
        nginx \
        python \
        sudo \
        supervisor \
        sqlite3 \
        git

# Download SDL Core from github
WORKDIR /usr

RUN mkdir /usr/sdl_core
RUN git clone https://github.com/smartdevicelink/sdl_core.git /usr/sdl_core


WORKDIR /usr/sdl_core

RUN git checkout develop
RUN git pull



RUN mkdir /usr/build

# Work inside the build folder
WORKDIR /usr/build

# Git needs to be configured now...
RUN git config --global user.email "anon@mail.com"
RUN git config --global user.name "Anonymous"


RUN cmake -DBUILD_AVAHI_SUPPORT=$DBUILD_AVAHI_SUPPORT \
    -DBUILD_USB_SUPPORT=$DBUILD_USB_SUPPORT \
    -DBUILD_BT_SUPPORT=$DBUILD_BT_SUPPORT \
    -DEXTENDED_MEDIA_MODE=$DEXTENDED_MEDIA_MODE \
    -DENABLE_HMI_PTU_DECRYPTION=$DENABLE_HMI_PTU_DECRYPTION \
    ../sdl_core

RUN make




