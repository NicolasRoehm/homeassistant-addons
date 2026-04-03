# https://developers.home-assistant.io/docs/add-ons/configuration#add-on-dockerfile
ARG BUILD_FROM
FROM ${BUILD_FROM}
ENV LANG=C.UTF-8

# SomfyProtect2MQTT version
ARG VERSION=2026.3.1

# Copy root filesystem
COPY rootfs /

# Do not compile packages from source
ENV PIP_NO_BUILD_ISOLATION=1
ENV PIP_PREFER_BINARY=1

# Setup base
RUN apk update && apk add --no-cache \
    ffmpeg \
    ffmpeg-dev \
    libjxl \
    libjxl-tools \
    libsrtp \
    libsrtp-dev \
    libvpx \
    libvpx-dev \
    linux-headers \
    opus \
    opus-dev \
    py3-cffi \
    py3-cryptography \
    py3-opencv \
    py3-pip \
    python3 \
    python3-dev \
    yq

# Download source from fork branch
WORKDIR /usr/bin
ARG FORK_REPO=NicolasRoehm/SomfyProtect2MQTT-audio
ARG FORK_BRANCH=master
ADD "https://github.com/${FORK_REPO}/archive/refs/heads/${FORK_BRANCH}.tar.gz" source.tar.gz
RUN tar -xvf source.tar.gz && mv /usr/bin/SomfyProtect2MQTT-audio-${FORK_BRANCH} /usr/bin/SomfyProtect2MQTT

# Install python3 requirements
WORKDIR /usr/bin/SomfyProtect2MQTT/somfyProtect2Mqtt
RUN pip3 install --no-cache-dir -r requirements/addon.txt
