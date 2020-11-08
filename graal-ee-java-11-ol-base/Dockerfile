###############################################################################################
#                       
# Container used to import the files and epand them. This is ephemeral
# and we will throw it away - stops the added install files being added
# to the final image (I Think)
#                       
###############################################################################################
FROM oraclelinux:7-slim as intermediate

MAINTAINER krisfoster <me@krismade.me>

ARG GRAAL_VERSION="20.2.0"
ARG JAVA_VERSION="11"

ENV INSTALLS_HOME=${USER_HOME}/installs \
    GRAALVM_HOME=/usr/lib/graalvm-ee-java${JAVA_VERSION}-${GRAAL_VERSION}

RUN yum install -y sudo tar which \
    && yum check-update \
    && mkdir -p ${GRAALVM_HOME} \
    && mkdir -p ${INSTALLS_HOME}

ADD /installs ${INSTALLS_HOME}

RUN tar zxf ${INSTALLS_HOME}/graalvm-ee-java${JAVA_VERSION}-linux-amd64-${GRAAL_VERSION}.tar.gz \
        --directory=/usr/lib/
RUN alternatives --install /usr/bin/java java ${GRAALVM_HOME}/bin/java 300000
RUN alternatives --install /usr/bin/gu gu ${GRAALVM_HOME}/bin/gu 300000
RUN echo "export GRAALVM_HOME=/usr/lib/graalvm-ee-java11-20.2.0" >> /etc/environment
RUN echo "export JAVA_HOME=/usr/lib/graalvm-ee-java11-20.2.0" >> /etc/environment
RUN gu install -L ${INSTALLS_HOME}/llvm-toolchain-installable-java${JAVA_VERSION}-linux-amd64-${GRAAL_VERSION}.jar \
    && gu install -L ${INSTALLS_HOME}/llvm-toolchain-installable-java${JAVA_VERSION}-linux-amd64-${GRAAL_VERSION}.jar \
    && gu install -L ${INSTALLS_HOME}/native-image-installable-svm-svmee-java${JAVA_VERSION}-linux-amd64-${GRAAL_VERSION}.jar
RUN alternatives --install /usr/bin/native-image native-image ${GRAALVM_HOME}/bin/native-image 300000
RUN yum clean all
RUN rm -rf ${INSTALLS_HOME}
