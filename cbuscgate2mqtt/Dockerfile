ARG BUILD_FROM
FROM $BUILD_FROM

RUN apk add --no-cache openjdk8-jre

# C-Gate port 20023 to allow c-bus toolkit connection from PC - maybe should be 20123 for remote c-gate site?
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
    # Download unpack and copy CGate
    # Update to get from rootfs/usr/local/bin/cgate/config /usr/local/bin/cgate/
    cd /tmp && \
    curl -O https://updates.clipsal.com/ClipsalSoftwareDownload/mainsite/cis/technical/CGate/cgate-2.11.4_3251.zip && \
    unzip cgate-2.11.4_3251.zip && \
    cp -rp cgate /usr/local/bin && \
    rm cgate-2.11.4_3251.zip && \
    rm -rf cgate

# Copy the default c-gate config files so they are available for customisation when the add-on starts.
COPY rootfs/usr/local/bin/cgate/config /usr/local/bin/cgate/config

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

COPY rootfs/etc /etc/
# Services are started by s6-overlay scripts copied in /etc/s6-overlay/s6-rc.d/