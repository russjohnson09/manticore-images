#build with --no-cache to get the most recent develop

#docker build --no-cache -t manticore-core-develop .
#docker tag manticore-core-develop russjohnson09/mantiore-core-develop
#docker push russjohnson09/manticore-core-develop:latest

#-P will expose ports from container to host but it will be random
#docker run -d -P -p 9010:8087 -p 9011:12345 --name manticore-core-develop -v ~/dockercorestorage:/usr/build/bin/storage cores-6.0.0

FROM russjohnson09/manticore-core-develop:latest

ARG CORE_VERSION=develop
ARG DBUILD_AVAHI_SUPPORT=OFF
ARG DBUILD_USB_SUPPORT=OFF
ARG DBUILD_BT_SUPPORT=OFF
ARG DEXTENDED_MEDIA_MODE=OFF
ARG DENABLE_HMI_PTU_DECRYPTION=OFF

ENV CORE_NETWORK_INTERFACE eth0
ENV CORE_VERSION=${CORE_VERSION}

WORKDIR /usr/sdl_core
RUN git status
RUN git pull

WORKDIR /usr/build
RUN make

RUN mkdir -p /usr/web/logstream
RUN mkdir -p /usr/web/broker
RUN mkdir -p /usr/web/policy
WORKDIR /usr/web/logstream

#
## Copy node and npm files
COPY logstream /usr/web/logstream/
COPY broker /usr/web/broker/
COPY policy /usr/web/policy/
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
WORKDIR /usr/web/logstream
RUN /bin/bash -c "source /root/.bashrc; \
    npm install"
#
WORKDIR /usr/web/broker
RUN /bin/bash -c "source /root/.bashrc; \
    npm install"
#
WORKDIR /usr/web/policy
RUN /bin/bash -c "source /root/.bashrc; \
    npm install"
#

### Directory to run commands in
WORKDIR /usr/build/bin
#
## Copy SDL Core start script into container
#
ENV LD_LIBRARY_PATH /usr/build/bin/
RUN ldconfig

COPY log4cxx.properties /usr/build/bin/log4cxx.properties
COPY hmi_capabilities.json /usr/build/bin/hmi_capabilities.json
COPY sdl_preloaded_pt.json /usr/build/bin/sdl_preloaded_pt.json
COPY core_config/smartDeviceLink.ini /usr/build/bin/smartDeviceLink.ini

## Copy supervisor config
COPY supervisord.conf /etc/supervisor/conf.d/supervisord.conf
COPY nginx.conf /etc/nginx

COPY setup.sh setup.sh

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

## Run the start script that will launch SDL Core!
CMD ["/bin/bash", "setup.sh"]
