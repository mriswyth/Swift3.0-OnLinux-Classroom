FROM ubuntu:15.10

# Install dependencies for running Swift 3.0 on ubuntu:15.10 image
# swift.org Downloads page lists clang and libicu-dev as the required packages
# had to add libxml2 to start Swift
# wget is for Swift package and signature download
# vim is for editing
RUN apt-get update && \
    apt-get install -y \
            clang \
            libxml2 \
            vim \
            wget \
            make && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

# Install Swift keys
RUN wget -q -O - https://swift.org/keys/all-keys.asc | gpg --import - && \
    gpg --keyserver hkp://pool.sks-keyservers.net --refresh-keys Swift

# Download and install Swift
# Swift Trunk Snapshot for ubuntu:15.10
# Includes making /projects directory to map to host filesystem
# Includes creation of swift user
RUN SWIFT_PKG=swift-DEVELOPMENT-SNAPSHOT-2016-04-25-a && \
    SWIFT_HOST=ubuntu15.10 && \
    SWIFT_URL=https://swift.org/builds/development && \
    SWIFT_BASE_FILE=${SWIFT_PKG}-${SWIFT_HOST} && \
    SWIFT_LINK=${SWIFT_URL}/$(echo "${SWIFT_HOST}" | tr -d .)/${SWIFT_PKG}/${SWIFT_BASE_FILE} && \
    wget ${SWIFT_LINK}.tar.gz && \
    wget ${SWIFT_LINK}.tar.gz.sig && \
    gpg --verify ${SWIFT_BASE_FILE}.tar.gz.sig && \
    tar -xvzf ${SWIFT_BASE_FILE}.tar.gz -C / --strip 1 && \
    rm -rf ${SWIFT_BASE_FILE}* /tmp/* /var/tmp/* && \
    mkdir /projects && \
    useradd -m -u 1000 swift -g users -s /bin/bash

# workarounds 
# "runtime does not come first" error 
# CoreFoundation module.modulemap permission error
RUN mv /usr/lib/lldb /usr/lib/lldb-disabled && \
    chmod o+r /usr/lib/swift/CoreFoundation/*

USER swift
