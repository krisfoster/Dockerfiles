FROM alpine:latest

ENV GLIBC_REPO=https://github.com/sgerrand/alpine-pkg-glibc \
    GLIBC_VERSION=2.32-r0 \
    LANG=C.UTF-8

RUN apk -U upgrade \
        && apk add --no-cache libstdc++ bash curl \
        && for pkg in glibc-${GLIBC_VERSION} glibc-bin-${GLIBC_VERSION} glibc-i18n-${GLIBC_VERSION}; \
            do curl -sSL ${GLIBC_REPO}/releases/download/${GLIBC_VERSION}/${pkg}.apk -o /tmp/${pkg}.apk; \
            done \
        && apk add --no-cache --allow-untrusted /tmp/*.apk \
        && rm -v /tmp/*.apk \
        && echo "export LANG=C.UTF-8" > /etc/profile.d/locale.sh  \
        && /usr/glibc-compat/sbin/ldconfig /lib /usr/glibc-compat/lib \
        # Install psrecord
        && apk add --no-cache --update alpine-sdk openssl-dev build-base \
        #&& ENV PYTHONUNBUFFERED=1 \
        && apk add --update --no-cache python3 && ln -sf python3 /usr/bin/python \
        && python3 -m ensurepip \
        && pip3 install --no-cache --upgrade pip setuptools \
        && apk add --update --no-cache py3-psutil \
        && rm -rf /var/cache/apk/* \
        && pip install --upgrade pip \
        && pip install psrecord \
        # Clean up
        && rm -rf /var/lib/apt/lists/*

