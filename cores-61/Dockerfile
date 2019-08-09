#docker build -t cores-6.0.0 .
#docker run -d -p 8087:8087 --name cores-6.0.0 cores-6.0.0

#

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
#has a build of master already.
##docker build -t cores-6.0.1
##docker build -m 4g -t cores-6.0.1 .

##https://stackoverflow.com/questions/45363771/docker-build-from-dockerfile-with-more-memory
#MEMORY MEMORY MEMORY.
FROM cores-6.0.0
#
#MAINTAINER Chris Rokita version:1.0
#
##already did cmake in base
ARG CORE_VERSION=master
ARG DBUILD_AVAHI_SUPPORT=OFF
ARG DBUILD_USB_SUPPORT=OFF
ARG DBUILD_BT_SUPPORT=OFF
ARG DEXTENDED_MEDIA_MODE=OFF
ARG DENABLE_HMI_PTU_DECRYPTION=OFF

ENV CORE_NETWORK_INTERFACE eth0
ENV CORE_VERSION=${CORE_VERSION}

#WORKDIR /usr/sdl_core
#RUN git checkout $CORE_VERSION
#RUN git pull
#
#RUN mkdir /usr/build || true
#
## Work inside the build folder
WORKDIR /usr/build



#RUN git pull
#
#WORKDIR /usr/build
#
#RUN cmake -DBUILD_AVAHI_SUPPORT=$DBUILD_AVAHI_SUPPORT \
#    -DBUILD_USB_SUPPORT=$DBUILD_USB_SUPPORT \
#    -DBUILD_BT_SUPPORT=$DBUILD_BT_SUPPORT \
#    -DEXTENDED_MEDIA_MODE=$DEXTENDED_MEDIA_MODE \
#    -DENABLE_HMI_PTU_DECRYPTION=$DENABLE_HMI_PTU_DECRYPTION \
#    ../sdl_core
#
##RUN make -j 3
##RUN make
#
##vagrant@sdl-core-develop:/core/build$ make -qp | awk -F':' '/^[a-zA-Z0-9][^$#\/\t=]*:([^=]|$)/ {split($1,A,/ /);for(i in A)print A[i]}'
##jsoncpp
##MOBILE_API
##cmake_force
##connectionHandler
##list_install_components
##app_service_rpc_plugin
##copy_policy_library
##edit_cache
##HMIMessageHandler
##sdl_rpc_plugin
##install-3rd_party_logger
##liblog4cxx
##vehicle_info_plugin
##expat
##encryption
##AppServiceRpcPluginStaticLib
##pasa-tarball
##ford-tarball
##libapr-1
##LowVoltageHandlerObjLibrary
##MessageHelper
##Makefile
##smartDeviceLinkCore
##Utils
##intergen
##SmartObjects
##dbms
##rpc_base
##v4_protocol_v1_2_no_extra
##UsageStatistics
##depend
##MediaManager
##all
##ProtocolLibrary
##Policy
##policy_struct
##help
##preinstall
##SystemTimeLibrary
##AMEventEngine
##cmake_check_build_system
##default_target
##TelemetryMonitor
##ProtocolHandler
##libbson
##RCRpcPluginStaticLib
##clean
##Boost
##SecurityManager
##install-3rd_party
##HMI_API
##ConfigProfile
##apr-util
##genivi-tarball
##AMPolicyLibrary
##Resumption
##ApplicationManager
##3rd_party_logger
##appenders
##policyValidator
##rc_rpc_plugin
##TransportManager
##install
##formatters
##rebuild_cache
#
#RUN make install-3rd_party_logger
#RUN make Boost
#RUN make ConfigProfile
#RUN make Utils
#RUN make rpc_base
#RUN make policy_struct
#RUN make policyValidator
#RUN make MOBILE_API
#RUN make v4_protocol_v1_2_no_extra
RUN make ProtocolLibrary
RUN make -j 8 VERBOSE=1 HMI_API

RUN make TransportManager
RUN make Resumption
RUN make SmartObjects
RUN make formatters
RUN make libbson
RUN make ProtocolHandler
RUN make connectionHandler
RUN make dbms
RUN make SecurityManager
RUN make Policy
#Copying library libPolicy.so
RUN make copy_policy_library
RUN make UsageStatistics
RUN make AMEventEngine
RUN make SystemTimeLibrary
RUN make MessageHelper
RUN make AMPolicyLibrary
RUN make ApplicationManager
RUN make vehicle_info_plugin
RUN make sdl_rpc_plugin
RUN make rc_rpc_plugin
RUN make RCRpcPluginStaticLib
RUN make app_service_rpc_plugin
RUN make AppServiceRpcPluginStaticLib
RUN make HMIMessageHandler
RUN make MediaManager
RUN make TelemetryMonitor
RUN make LowVoltageHandlerObjLibrary
RUN make expat
RUN make smartDeviceLinkCore
RUN make appenders



RUN make

#
#
##already did cmake in base
##RUN cmake -DBUILD_AVAHI_SUPPORT=$DBUILD_AVAHI_SUPPORT \
##    -DBUILD_USB_SUPPORT=$DBUILD_USB_SUPPORT \
##    -DBUILD_BT_SUPPORT=$DBUILD_BT_SUPPORT \
##    -DEXTENDED_MEDIA_MODE=$DEXTENDED_MEDIA_MODE \
##    -DENABLE_HMI_PTU_DECRYPTION=$DENABLE_HMI_PTU_DECRYPTION \
##    ../sdl_core
#
## Generate our Makefile, compile it and install
## Remove everything but what's inside the bin folder of the build directory to save space
#RUN make \
#    && make install
#    && mv bin /usr/tmpbin \
#    && rm -rf * \
#    && mv /usr/tmpbin bin
#
#RUN mkdir -p /usr/web/logstream
#RUN mkdir -p /usr/web/broker
#RUN mkdir -p /usr/web/policy
#WORKDIR /usr/web/logstream
#
## Copy node and npm files
#COPY logstream /usr/web/logstream/
#COPY broker /usr/web/broker/
#COPY policy /usr/web/policy/
#
## Install nvm
#RUN curl -sL https://raw.githubusercontent.com/creationix/nvm/v0.31.0/install.sh -o install_nvm.sh
#RUN bash install_nvm.sh
#RUN /bin/bash -c "source /root/.bashrc; \
#    nvm install v8.11.1 \
#    nvm use v8.11.1 \
#    nvm alias default v8.11.1"
##
### Install dependencies
##
#WORKDIR /usr/web/logstream
#RUN /bin/bash -c "source /root/.bashrc; \
#    npm install"
#
#WORKDIR /usr/web/broker
#RUN /bin/bash -c "source /root/.bashrc; \
#    npm install"
#
#WORKDIR /usr/web/policy
#RUN /bin/bash -c "source /root/.bashrc; \
#    npm install"
#
### Directory to run commands in
WORKDIR /usr/build/bin
#
## Copy SDL Core start script into container
COPY setup.sh setup.sh
#
ENV LD_LIBRARY_PATH /usr/build/bin/
#
## Configure logger
RUN ldconfig
#
## Delete default nginx configuration
#RUN rm -v /etc/nginx/nginx.conf
#
## Copy new nginx configuration
COPY nginx.conf /etc/nginx
#
## Copy supervisor config
COPY supervisord.conf /etc/supervisor/conf.d/supervisord.conf
#
## Replace log file with a custom made one
COPY log4cxx.properties /usr/build/bin/log4cxx.properties
#
## Replace hmi capabilities file with a custom made one
COPY hmi_capabilities.json /usr/build/bin/hmi_capabilities.json


#RUN rm -Rv /usr/build/bin/storage
#
## Replace sdl_preloaded_pt file with a custom made one
COPY sdl_preloaded_pt.json /usr/build/bin/sdl_preloaded_pt.json


COPY core_config/smartDeviceLink.ini /usr/build/bin/smartDeviceLink.ini


RUN apt-get update && apt-get install nano -y

RUN apt-get update && apt-get install samba -y



#
## Expose the following ports
##   3001: Expose SDL Core's file system
##   12345: Expose SDL Core's primary port. Used to communicate with the SDL Core instance over TCP
##   5050: Expose video streaming port
##   5080: Expose audio streaming port
##   8090: Expose time testing port
##   8888: Expose websocket port for core log streaming
##   9000: Websocket connection to the broker
##   9898: Expose miniature policy server port
##   8087: Expose core websocket. SHOULD use broker in most cases.
EXPOSE 3001 12345 5050 5080 8090 8888 9000 9898 8087

#-P will expose ports from container to host but it will be random

#docker build -t cores-6.0.1 .
#docker run -d -P -p 9010:8087 -p 9011:12345 --name cores-6.0.0 -v ~/dockercorestorage:/usr/build/bin/storage cores-6.0.0

#docker run -d -P --name cores-6.0.0 cores-6.0.0

#
## Run the start script that will launch SDL Core!
CMD ["/bin/bash", "setup.sh"]

