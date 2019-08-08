# Copyright (c) 2018 Livio, Inc.
# All rights reserved.
#
# Redistribution and use in source and binary forms, with or without
# modification, are permitted provided that the following conditions are met:
#
# Redistributions of source code must retain the above copyright notice, this
# list of conditions and the following disclaimer.
#
# Redistributions in binary form must reproduce the above copyright notice,
# this list of conditions and the following
# disclaimer in the documentation and/or other materials provided with the
# distribution.
#
# Neither the name of the Livio Inc. nor the names of its contributors
# may be used to endorse or promote products derived from this software
# without specific prior written permission.
#
# THIS SOFTWARE IS PROVIDED BY THE COPYRIGHT HOLDERS AND CONTRIBUTORS "AS IS"
# AND ANY EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE
# IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE
# ARE DISCLAIMED. IN NO EVENT SHALL THE COPYRIGHT HOLDER OR CONTRIBUTORS BE
# LIABLE FOR ANY DIRECT, INDIRECT, INCIDENTAL, SPECIAL, EXEMPLARY, OR
# CONSEQUENTIAL DAMAGES (INCLUDING, BUT NOT LIMITED TO, PROCUREMENT OF
# SUBSTITUTE GOODS OR SERVICES; LOSS OF USE, DATA, OR PROFITS; OR BUSINESS
# INTERRUPTION) HOWEVER CAUSED AND ON ANY THEORY OF LIABILITY, WHETHER IN
# CONTRACT, STRICT LIABILITY, OR TORT (INCLUDING NEGLIGENCE OR OTHERWISE)
# ARISING IN ANY WAY OUT OF THE USE OF THIS SOFTWARE, EVEN IF ADVISED OF THE
# POSSIBILITY OF SUCH DAMAGE.
#

# ##################################################################################################
# Basic docker build of SDL Core for use with Manticore:  https://github.com/smartdevicelink/sdl_core
# ##################################################################################################

# This image is based of debian
FROM buildpack-deps:stretch

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


ARG CORE_VERSION=master
ARG DBUILD_AVAHI_SUPPORT=OFF
ARG DBUILD_USB_SUPPORT=OFF
ARG DBUILD_BT_SUPPORT=OFF
ARG DEXTENDED_MEDIA_MODE=OFF
ARG DENABLE_HMI_PTU_DECRYPTION=OFF

ENV CORE_NETWORK_INTERFACE eth0
ENV CORE_VERSION=${CORE_VERSION}

# Download SDL Core from github
WORKDIR /usr

WORKDIR /usr

RUN echo 1

RUN git clone https://github.com/smartdevicelink/sdl_core.git
WORKDIR /usr/sdl_core
RUN git checkout master
RUN git pull

RUN mkdir /usr/build

# Git needs to be configured now...
RUN git config --global user.email "anon@mail.com"
RUN git config --global user.name "Anonymous"

#RUN cmake -DBUILD_AVAHI_SUPPORT=$DBUILD_AVAHI_SUPPORT \
#    -DBUILD_USB_SUPPORT=$DBUILD_USB_SUPPORT \
#    -DBUILD_BT_SUPPORT=$DBUILD_BT_SUPPORT \
#    -DEXTENDED_MEDIA_MODE=$DEXTENDED_MEDIA_MODE \
#    -DENABLE_HMI_PTU_DECRYPTION=$DENABLE_HMI_PTU_DECRYPTION \
#    ../sdl_core \
#    && make \
#    && make install


#ADD go /usr/local/go


WORKDIR /

COPY .builds/test.txt test.txt

COPY .builds/develop-2019-08-08.tar.gz develop-2019-08-08.tar.gz


RUN ls -al

