###############################################################################################
#                       
# Container used to import the files and epand them. This is ephemeral
# and we will throw it away - stops the added install files being added
# to the final image (I Think)
#                       
###############################################################################################
FROM oraclelinux:7-slim

MAINTAINER krisfoster <me@krismade.me>

ARG GRAAL_VERSION="20.3.0"
ARG JAVA_VERSION="11"

ENV GRAALVM_HOME=/usr/lib/graalvm-ee-java${JAVA_VERSION}-${GRAAL_VERSION} \
    JAVA_HOME=/usr/lib/graalvm-ee-java${JAVA_VERSION}-${GRAAL_VERSION} \
    TAR_FILE=graalvm-ee-java${JAVA_VERSION}-linux-amd64-${GRAAL_VERSION}.tar.gz \
    LLVM_JAR=llvm-toolchain-installable-java${JAVA_VERSION}-linux-amd64-${GRAAL_VERSION}.jar \
    NATIVE_IMAGE_JAR=native-image-installable-svm-svmee-java${JAVA_VERSION}-linux-amd64-${GRAAL_VERSION}.jar \
    PATH=$PATH:/opt/rh/llvm-toolset-7/root/usr/bin \
    LD_LIBRARY_PATH=/opt/rh/llvm-toolset-7/root/usr/lib64 \
    MANPATH=/opt/rh/llvm-toolset-7/root/usr/share/man \
    PKG_CONFIG_PATH=/opt/rh/llvm-toolset-7/root/usr/lib64/pkgconfig \
    PYTHONPATH=/opt/rh/llvm-toolset-7/root/usr/lib/python2.7/site-packages \
    X_SCLS=llvm-toolset-7

COPY installs/${TAR_FILE} .
COPY installs/${LLVM_JAR} .
COPY installs/${NATIVE_IMAGE_JAR} .

RUN yum update -y oraclelinux-release-el7 \
    && yum install -y oraclelinux-developer-release-el7 oracle-softwarecollection-release-el7 \
    && yum-config-manager --enable ol7_developer \
    && yum-config-manager --enable ol7_developer_EPEL \
    && yum-config-manager --enable ol7_optional_latest \
    && yum-config-manager --enable ol7_software_collections
RUN yum install -y bzip2-devel ed gcc gcc-c++ gcc-gfortran gzip file fontconfig less libcurl-devel make openssl \
        openssl-devel readline-devel tar vi which xz-devel zlib-devel
RUN yum install -y glibc-static libcxx libcxx-devel llvm-toolset-7 zlib-static
RUN fc-cache -f -v
RUN set -eux \
    && mkdir -p ${GRAALVM_HOME} \
    && tar zxf ${TAR_FILE} --directory=/usr/lib/ \
    && mkdir -p "/usr/java" \
    && ln -sfT "$GRAALVM_HOME" /usr/java/default \
    && ln -sfT "$GRAALVM_HOME" /usr/java/latest
RUN for bin in "$GRAALVM_HOME/bin/"*; do \
        base="$(basename "$bin")"; \
        [ ! -e "/usr/bin/$base" ]; \
        alternatives --install "/usr/bin/$base" "$base" "$bin" 20000; \
    done
RUN echo "export GRAALVM_HOME=/usr/lib/graalvm-ee-java11-20.2.0" >> /etc/environment \
    && echo "export JAVA_HOME=/usr/lib/graalvm-ee-java11-20.2.0" >> /etc/environment
RUN gu install -L  ${LLVM_JAR}
RUN gu install -L ${NATIVE_IMAGE_JAR}
RUN alternatives --install /usr/bin/native-image native-image ${GRAALVM_HOME}/bin/native-image 300000
RUN yum clean all
RUN rm -rf /var/cache/yum
RUN rm -f ${LLVM_JAR} \
    && rm -f ${NATIVE_IMAGE_JAR} \
    && rm -f ${TAR_FILE}
