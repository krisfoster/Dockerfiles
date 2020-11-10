###############################################################################################
#                       
# Container used to import the files and epand them. This is ephemeral
# and we will throw it away - stops the added install files being added
# to the final image (I Think)
#                       
###############################################################################################
FROM oraclelinux:7-slim

MAINTAINER krisfoster <me@krismade.me>

ARG GRAAL_VERSION="20.2.0"
ARG JAVA_VERSION="11"

ENV GRAALVM_HOME=/usr/lib/graalvm-ee-java${JAVA_VERSION}-${GRAAL_VERSION} \
    JAVA_HOME=/usr/lib/graalvm-ee-java${JAVA_VERSION}-${GRAAL_VERSION} \
    TAR_FILE=graalvm-ee-java${JAVA_VERSION}-linux-amd64-${GRAAL_VERSION}.tar.gz \
    LLVM_JAR=llvm-toolchain-installable-java${JAVA_VERSION}-linux-amd64-${GRAAL_VERSION}.jar \
    NATIVE_IMAGE_JAR=native-image-installable-svm-svmee-java${JAVA_VERSION}-linux-amd64-${GRAAL_VERSION}.jar

COPY installs/${TAR_FILE} .
COPY installs/${LLVM_JAR} .
COPY installs/${NATIVE_IMAGE_JAR} .

RUN yum-config-manager --enable ol7_optional_latest \
    && yum check-update \
    && yum install -y tar gzip gcc glibc-devel zlib-devel libstdc++-static \
    && mkdir -p ${GRAALVM_HOME} \
    && tar zxf ${TAR_FILE} --directory=/usr/lib/ \
    && alternatives --install /usr/bin/gu gu ${GRAALVM_HOME}/bin/gu 300000 \
    && echo "export GRAALVM_HOME=/usr/lib/graalvm-ee-java11-20.2.0" >> /etc/environment \
    && echo "export JAVA_HOME=/usr/lib/graalvm-ee-java11-20.2.0" >> /etc/environment \
    && gu install -L ${LLVM_JAR} \
    && gu install -L ${NATIVE_IMAGE_JAR} \
    && alternatives --install /usr/bin/java java ${GRAALVM_HOME}/bin/java 300000 \
    && alternatives --install /usr/bin/javac javac ${GRAALVM_HOME}/bin/javac 300000 \
    && alternatives --install /usr/bin/jar jar ${GRAALVM_HOME}/bin/jar 300000 \
    && alternatives --install /usr/bin/native-image native-image ${GRAALVM_HOME}/bin/native-image 300000 \
    && yum autoremove -y tar gzip \
    && yum clean all \
    && rm -rf /var/cache/yum \
    && rm -f ${LLVM_JAR} \
    && rm -f ${NATIVE_IMAGE_JAR} \
    && rm -f ${TAR_FILE}
