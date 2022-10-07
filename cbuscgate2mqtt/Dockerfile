ARG BUILD_FROM
FROM $BUILD_FROM

RUN apk add --no-cache openjdk8-jre

# C-Gate port 20023 to allow c-bus toolkit connection from PC
EXPOSE 20023

# Build and install the ser2sock application
RUN \
    apk add --no-cache build-base git libressl-dev jq && \
    cd /tmp && \
    git clone https://github.com/nutechsoftware/ser2sock.git && \
    cd ser2sock && \
    ./configure && \
    make && \
    mkdir /usr/local/bin/ser2sock && \
    cp ser2sock /usr/local/bin/ser2sock && \
    chmod a+x /usr/local/bin/ser2sock/ser2sock && \
    cd .. && \
    rm -rf ser2sock

RUN \
    # mkdir /usr/local/bin/cgate && \
    cd /tmp && \
    curl -O https://updates.clipsal.com/ClipsalSoftwareDownload/mainsite/cis/technical/CGate/cgate-2.11.4_3251.zip && \
    unzip cgate-2.11.4_3251.zip && \
    cp -rp cgate /usr/local/bin && \
    rm cgate-2.11.4_3251.zip && \
    rm -rf cgate

# Clone cgateweb
RUN \
    cd /usr/local/bin && \
    git clone https://github.com/bdnstn/cgateweb.git && \
    cd /usr/local/bin/cgateweb && \
    chmod a+x index.js 

RUN \
    cd /usr/local/bin/cgateweb && \
    apk add --no-cache npm && \
    npm install

# Configure cgateweb
RUN \
     cd /usr/local/bin/cgateweb && \
     rm settings.js 

# COPY rootfs/usr/local/bin/cgateweb/settings.js /usr/local/bin/cgateweb/settings.js
# RUN \
#     chmod a+x /usr/local/bin/cgateweb/settings.js && \
#     chmod a+x /usr/local/bin/cgateweb/index.js

# TODO - get the tag file from somewhere else
COPY rootfs/usr/local/bin/cgate/tag/ /usr/local/bin/cgate/tag/
COPY rootfs/usr/local/bin/cgate/config/ /usr/local/bin/cgate/config/

# CGate logs a lot of events, so drect to null to save the SD.
RUN \
    mkdir /usr/local/bin/cgate/logs && \
    cd /usr/local/bin/cgate/logs && \
    ln -s /dev/null event.txt

COPY rootfs/etc /etc/