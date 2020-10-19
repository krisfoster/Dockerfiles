FROM ubuntu:devel

ARG GRAAL_VERSION="20.2.0"
ARG JAVA_VERSION="11"
ARG USER_NAME="vscode"
ARG USER_HOME="/home/vscode"

ENV USER_NAME=${USER_NAME} \
    GRAALVM_BASE=${USER_HOME}/bin \
    INSTALLS_HOME=${USER_HOME}/installs \
    GRAALVM_HOME=${USER_HOME}/bin/graalvm-ee-java${JAVA_VERSION}-${GRAAL_VERSION} \
    DISPLAY=host.docker.internal:0

RUN apt-get update \
    && apt-get -y upgrade \
    && apt-get -y install build-essential \
    && apt-get -y install autoconf automake gdb git libffi-dev zlib1g-dev libssl-dev \
    && apt-get -y install curl wget telnet git zsh maven sudo vim emacs iproute2 \
    && apt-get clean \
    && apt-get autoclean \
    && apt-get autoremove \
    && useradd -u 1000 ${USER_NAME} \
    && echo "${USER_NAME} ALL=(ALL:ALL) NOPASSWD: ALL" | sudo EDITOR='tee -a' visudo \
    && mkdir -p ${USER_HOME} \
    && chown -R ${USER_NAME}:${USER_NAME} ${USER_HOME} 

USER vscode
WORKDIR ${USER_HOME}

RUN mkdir -p ${GRAALVM_BASE} \
    && mkdir -p ${INSTALLS_HOME}

ADD ./installs ${INSTALLS_HOME}

# Install ZSH, you can run bash as well if you like but *DON"T* install and use the FISH shell
RUN wget https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh \
    && chmod u+x install.sh && ./install.sh \
    && rm install.sh \
    && curl -L https://git.io/epre | sh \
    && echo "export JAVA_HOME=${GRAALVM_HOME}" >> ${USER_HOME}/.zshrc \
    && echo "export PATH=${GRAALVM_HOME}/bin:${PATH}"  >> ${USER_HOME}/.zshrc \
    && echo "export JAVA_HOME=${GRAALVM_HOME}" >> ${USER_HOME}/.bashrc \
    && echo "export PATH=${GRAALVM_HOME}/bin:${PATH}"  >> ${USER_HOME}/.bashrc

RUN tar zxf ${INSTALLS_HOME}/graalvm-ee-java${JAVA_VERSION}-linux-amd64-${GRAAL_VERSION}.tar.gz --directory=$GRAALVM_BASE \
    && export JAVA_HOME=${GRAALVM_HOME} \
    && export PATH=${JAVA_HOME}/bin:${PATH} \
    && sudo ln -s ${GRAALVM_HOME} /docker-java-home \
    && gu install -L ${INSTALLS_HOME}/llvm-toolchain-installable-java${JAVA_VERSION}-linux-amd64-${GRAAL_VERSION}.jar \
    && gu install -L ${INSTALLS_HOME}/llvm-toolchain-installable-java${JAVA_VERSION}-linux-amd64-${GRAAL_VERSION}.jar \
    && gu install -L ${INSTALLS_HOME}/native-image-installable-svm-svmee-java${JAVA_VERSION}-linux-amd64-${GRAAL_VERSION}.jar \
    && gu install -L ${INSTALLS_HOME}/ruby-installable-svm-svmee-java${JAVA_VERSION}-linux-amd64-${GRAAL_VERSION}.jar \
    && gu install -L ${INSTALLS_HOME}/python-installable-svm-svmee-java${JAVA_VERSION}-linux-amd64-${GRAAL_VERSION}.jar \
    && ${GRAALVM_HOME}/languages/ruby/lib/truffle/post_install_hook.sh \
#   && gu rebuild-images llvm polyglot libpolyglot llvm ruby python \
    && rm -rf ${INSTALLS_HOME}

EXPOSE 8080/tcp \
       3000/tcp \
       80/tcp