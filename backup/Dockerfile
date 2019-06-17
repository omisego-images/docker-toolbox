FROM alpine:3.9

LABEL maintainer="OmiseGO Team <omg@omise.co>"
LABEL description="Backup utilities for OmiseGO"

RUN set -xe \
 && apk add --update --no-cache --virtual .utils \
        bash \
        ca-certificates \
        curl \
        git \
        gnupg \
        libressl \
        libressl-dev \
        openssh-client

ARG CLOUD_SDK_VERSION=250.0.0
ARG CLOUD_SDK_DOWNLOAD_SHA256="fe59b988c6a8a40ae98a3b9d0ea98b98e55e5061e8cec14d71e93b7d198c133e"

RUN set -xe \
 && apk add --update --no-cache --virtual .gcloud-runtime \
        python \
        py-crcmod \
        libc6-compat \
 && CLOUD_SDK_DOWNLOAD_URL="https://dl.google.com/dl/cloudsdk/channels/rapid/downloads/google-cloud-sdk-${CLOUD_SDK_VERSION}-linux-x86_64.tar.gz" \
 && curl -fsL -o cloud-sdk-src.tar.gz "${CLOUD_SDK_DOWNLOAD_URL}" \
 && echo "${CLOUD_SDK_DOWNLOAD_SHA256}  cloud-sdk-src.tar.gz" |sha256sum -c - \
 && tar xzf cloud-sdk-src.tar.gz \
 && tar -xzC /usr/local --strip-components=1 -f cloud-sdk-src.tar.gz \
 && rm cloud-sdk-src.tar.gz \
 && ln -s /lib /lib64 \
 && gcloud config set core/disable_usage_reporting true \
 && gcloud config set component_manager/disable_update_check true \
 && gcloud config set metrics/environment github_docker_image \
 && gcloud --version

RUN set -xe \
 && apk add --update --no-cache --virtual .duplicity-runtime \
        duplicity \
        gnupg \
 && duplicity --version \
 && gpg --version
