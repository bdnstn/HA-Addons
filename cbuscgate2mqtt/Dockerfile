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
    # settings.js will be overwritten by /etc/s6-overlay/s6-rc.d/cgateweb/run

# Install node package manager dependency for cgateweb
RUN \
    cd /usr/local/bin/cgateweb && \
    apk add --no-cache npm && \
    npm install

# Copy the c-gate config files. This is WIP as these files need to go into /data to make them persistent.
COPY rootfs/usr/local/bin/cgate/tag/ /usr/local/bin/cgate/tag/
COPY rootfs/usr/local/bin/cgate/config/ /usr/local/bin/cgate/config/

COPY rootfs/etc /etc/
# Services are started by s6-overlay scripts copied in /etc/s6-overlay/s6-rc.d/