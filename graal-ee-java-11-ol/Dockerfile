###############################################################################################
#                       
# Container used to import the files and epand them. This is ephemeral
# and we will throw it away - stops the added install files being added
# to the final image (I Think)
#                       
###############################################################################################
FROM oraclelinux:7-slim as intermediate

ARG GRAAL_VERSION="20.2.0"
ARG JAVA_VERSION="11"
ARG USER_NAME="vscode"
ARG USER_HOME="/home/vscode"

ENV USER_NAME=${USER_NAME} \
    GRAALVM_BASE=${USER_HOME}/bin \
    INSTALLS_HOME=${USER_HOME}/installs \
    GRAALVM_HOME=${USER_HOME}/bin/graalvm-ee-java${JAVA_VERSION}-${GRAAL_VERSION}

RUN yum install -y sudo tar which \
    && useradd -u 1000 ${USER_NAME} \
    && echo "${USER_NAME} ALL=(ALL:ALL) NOPASSWD: ALL" | sudo EDITOR='tee -a' visudo

USER ${USER_NAME}
WORKDIR ${USER_HOME}

RUN mkdir -p ${USER_HOME} \
    && mkdir -p ${GRAALVM_BASE} \
    && mkdir -p ${INSTALLS_HOME}

ADD ./installs ${INSTALLS_HOME}

RUN tar zxf ${INSTALLS_HOME}/graalvm-ee-java${JAVA_VERSION}-linux-amd64-${GRAAL_VERSION}.tar.gz --directory=$GRAALVM_BASE \
    && export JAVA_HOME=${GRAALVM_HOME} \
    && export PATH=${JAVA_HOME}/bin:${PATH} \
    && echo "export JAVA_HOME=${GRAALVM_HOME}" >> ${USER_HOME}/.bashrc \
    && echo "PATH=${JAVA_HOME}/bin:${PATH}" >> ${USER_HOME}/.bashrc \
    && gu install -L ${INSTALLS_HOME}/llvm-toolchain-installable-java${JAVA_VERSION}-linux-amd64-${GRAAL_VERSION}.jar \
    && gu install -L ${INSTALLS_HOME}/llvm-toolchain-installable-java${JAVA_VERSION}-linux-amd64-${GRAAL_VERSION}.jar \
    && gu install -L ${INSTALLS_HOME}/native-image-installable-svm-svmee-java${JAVA_VERSION}-linux-amd64-${GRAAL_VERSION}.jar \
    && gu install -L ${INSTALLS_HOME}/ruby-installable-svm-svmee-java${JAVA_VERSION}-linux-amd64-${GRAAL_VERSION}.jar \
    && gu install -L ${INSTALLS_HOME}/python-installable-svm-svmee-java${JAVA_VERSION}-linux-amd64-${GRAAL_VERSION}.jar \
    && rm -rf ${INSTALLS_HOME}

###############################################################################################
#                       
# ACTUAL DEV CONTAINER
#
###############################################################################################

FROM oraclelinux:7-slim

ARG GRAAL_VERSION="20.2.0"
ARG JAVA_VERSION="11"
ARG USER_NAME="vscode"
ARG USER_HOME="/home/vscode"

ENV USER_NAME=${USER_NAME} \
    GRAALVM_BASE=${USER_HOME}/bin \
    INSTALLS_HOME=${USER_HOME}/installs \
    GRAALVM_HOME=${USER_HOME}/bin/graalvm-ee-java${JAVA_VERSION}-${GRAAL_VERSION} \
    LANG=en_US.UTF-8 \
    PATH=$PATH:/opt/rh/llvm-toolset-7/root/usr/bin \
    LD_LIBRARY_PATH=/opt/rh/llvm-toolset-7/root/usr/lib64 \
    MANPATH=/opt/rh/llvm-toolset-7/root/usr/share/man \
    PKG_CONFIG_PATH=/opt/rh/llvm-toolset-7/root/usr/lib64/pkgconfig \
    PYTHONPATH=/opt/rh/llvm-toolset-7/root/usr/lib/python2.7/site-packages \
    X_SCLS=llvm-toolset-7 \
    DISPLAY=host.docker.internal:0

RUN yum update -y oraclelinux-release-el7 \
    && yum install -y oraclelinux-developer-release-el7 oracle-softwarecollection-release-el7 \
    && yum-config-manager --enable ol7_developer \
    && yum-config-manager --enable ol7_developer_EPEL \
    && yum-config-manager --enable ol7_optional_latest \
    && yum-config-manager --enable ol7_software_collections \
    && yum install -y bzip2-devel ed gcc gcc-c++ gcc-gfortran gzip file fontconfig less libcurl-devel make openssl openssl-devel readline-devel tar vi which xz-devel zlib-devel \
    && yum install -y glibc-static libcxx libcxx-devel llvm-toolset-7 zlib-static \
    && yum install -y sudo which git zsh telnet unzip iputils curl wget rh-maven35 \
#    && yum install -y oracle-softwarecollection-release-el7 devtoolset-9 \
    && rm -rf /var/cache/yum \
    && yum clean all

RUN fc-cache -f -v \
    && useradd -u 1000 ${USER_NAME} \
    && echo "${USER_NAME} ALL=(ALL:ALL) NOPASSWD: ALL" | sudo EDITOR='tee -a' visudo \
    && ln -s ${GRAALVM_HOME} /docker-java-home

USER ${USER_NAME}
WORKDIR ${USER_HOME}

COPY --from=intermediate ${GRAALVM_BASE} ${GRAALVM_BASE}
COPY --from=intermediate ${USER_HOME}/.bashrc ${USER_HOME}/.bashrc

RUN mkdir -p ${USER_HOME} \
    && mkdir -p ${GRAALVM_BASE} \
    # Install ZSH, you can run bash as well if you like but *DON"T* install and use the FISH shell
    && wget https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh \
    && chmod u+x install.sh && ./install.sh \
    && rm install.sh \
    && echo "export JAVA_HOME=${GRAALVM_HOME}" >> ${USER_HOME}/.zshrc \
    && echo "export PATH=${GRAALVM_HOME}/bin:${PATH}"  >> ${USER_HOME}/.zshrc \
    && echo "source /opt/rh/rh-maven35/enable"  >> ${USER_HOME}/.zshrc \
    && echo "source /opt/rh/rh-maven35/enable"  >> ${USER_HOME}/.bashrc \
#   && echo "source /opt/rh/devtoolset-9/enable"  >> ${USER_HOME}/.zshrc \
#   && echo "source /opt/rh/devtoolset-9/enable"  >> ${USER_HOME}/.bashrc \
    && ${GRAALVM_HOME}/languages/ruby/lib/truffle/post_install_hook.sh
#    && ${GRAALVM_HOME}/bin/gu rebuild-images polyglot libpolyglot js llvm ruby python

EXPOSE 8080/tcp \
       3000/tcp \
       80/tcp