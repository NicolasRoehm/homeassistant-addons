# https://developers.home-assistant.io/docs/add-ons/configuration#add-on-dockerfile
ARG BUILD_FROM
FROM ${BUILD_FROM}
ENV LANG=C.UTF-8

ARG VERSION=0.1.0

# Copy root filesystem (s6-overlay scripts)
COPY rootfs /

# Install Node.js 20 LTS, npm and runtime tools
# Note: Alpine 3.20 ships Node.js 20.x — compatible with all project dependencies
RUN apk update && apk add --no-cache \
    nodejs \
    npm \
    curl \
    jq \
    tar \
    git
