#docker build -t cores-develop .

FROM buildpack-deps:stretch

MAINTAINER Chris Rokita version:1.0

ARG CORE_VERSION=develop
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

RUN cd /usr && git clone https://github.com/smartdevicelink/sdl_core.git  && cd sdl_core && git checkout develop

RUN mkdir /usr/build

# Work inside the build folder
WORKDIR /usr/build

# Git needs to be configured now...
RUN git config --global user.email "anon@mail.com"
RUN git config --global user.name "Anonymous"

# Generate our Makefile, compile it and install
# Remove everything but what's inside the bin folder of the build directory to save space
RUN cmake -DBUILD_AVAHI_SUPPORT=$DBUILD_AVAHI_SUPPORT \
    -DBUILD_USB_SUPPORT=$DBUILD_USB_SUPPORT \
    -DBUILD_BT_SUPPORT=$DBUILD_BT_SUPPORT \
    -DEXTENDED_MEDIA_MODE=$DEXTENDED_MEDIA_MODE \
    -DENABLE_HMI_PTU_DECRYPTION=$DENABLE_HMI_PTU_DECRYPTION \
    ../sdl_core \
    && make

##TODO try not to make any changes above this line to speed things up.


RUN mkdir -p /usr/web/logstream
RUN mkdir -p /usr/web/broker
RUN mkdir -p /usr/web/policy
WORKDIR /usr/web/logstream

# Copy node and npm files
COPY logstream /usr/web/logstream/
COPY broker /usr/web/broker/
COPY policy /usr/web/policy/

# Install nvm
RUN curl -sL https://raw.githubusercontent.com/creationix/nvm/v0.31.0/install.sh -o install_nvm.sh
RUN bash install_nvm.sh
RUN /bin/bash -c "source /root/.bashrc; \
    nvm install v8.11.1 \
    nvm use v8.11.1 \
    nvm alias default v8.11.1"

# Install dependencies

WORKDIR /usr/web/logstream
RUN /bin/bash -c "source /root/.bashrc; \
    npm install"

WORKDIR /usr/web/broker
RUN /bin/bash -c "source /root/.bashrc; \
    npm install"

WORKDIR /usr/web/policy
RUN /bin/bash -c "source /root/.bashrc; \
    npm install"

# Directory to run commands in
WORKDIR /usr/build/bin

# Copy SDL Core start script into container
COPY setup.sh setup.sh

ENV LD_LIBRARY_PATH /usr/build/bin/

# Configure logger
RUN ldconfig

# Delete default nginx configuration
RUN rm -v /etc/nginx/nginx.conf

# Copy new nginx configuration
COPY nginx.conf /etc/nginx

# Copy supervisor config
COPY supervisord.conf /etc/supervisor/conf.d/supervisord.conf

# Replace log file with a custom made one
COPY log4cxx.properties /usr/build/bin/log4cxx.properties

# Replace hmi capabilities file with a custom made one
COPY hmi_capabilities.json /usr/build/bin/hmi_capabilities.json

# Replace sdl_preloaded_pt file with a custom made one
COPY sdl_preloaded_pt.json /usr/build/bin/sdl_preloaded_pt.json
COPY sdl_preloaded_pt.json /usr/web/policy/pt-default.json
COPY sdl_preloaded_pt.json /usr/web/policy/pt.json

# Expose the following ports
#   3001: Expose SDL Core's file system
#   12345: Expose SDL Core's primary port. Used to communicate with the SDL Core instance over TCP
#   5050: Expose video streaming port
#   5080: Expose audio streaming port
#   8090: Expose time testing port
#   8888: Expose websocket port for core log streaming
#   9000: Websocket connection to the broker
#   9898: Expose miniature policy server port
EXPOSE 3001 12345 5050 5080 8090 8888 9000 9898

# Run the start script that will launch SDL Core!

#update core build as final step before launch
#https://stackoverflow.com/questions/36996046/how-to-prevent-dockerfile-caching-git-clone
#recompile using latests
#RUN echo "last updated 2018-08-08" && cd /usr/sdl_core && ls -al
RUN echo "last updated 2018-08-09" && cd /usr/sdl_core && git pull && cd /usr/build && make && make install


RUN echo "last updated 2018-08-09" && cd /usr/sdl_core && git pull && cd /usr/build && make && make install \
&& mv bin /usr/tmpbin \
&& rm -rf * \
&& mv /usr/tmpbin bin  && rm -R /usr/sdl_core

CMD ["/bin/bash", "setup.sh"]

##docker run -d -p 12346:12345 -p 3002:3001 -p 9001:9000 --name core cores-develop:latest
