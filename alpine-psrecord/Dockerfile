FROM alpine:latest
MAINTAINER krisfoster <me@krismade.me>

# Alpine image with psrecord installed, for profiling mem / cpu usage
# Use as a base build for apps that we want to profile

RUN apk add --update alpine-sdk openssl-dev build-base

# Install python/pip
ENV PYTHONUNBUFFERED=1
RUN apk add --update --no-cache python3 && ln -sf python3 /usr/bin/python
RUN python3 -m ensurepip
RUN pip3 install --no-cache --upgrade pip setuptools
RUN apk add --update --no-cache py3-psutil
RUN rm -rf /var/cache/apk/*
RUN pip install --upgrade pip
RUN pip install psrecord
