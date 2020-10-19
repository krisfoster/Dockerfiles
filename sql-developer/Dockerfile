FROM krisfoster/graal-ee-java11-ubuntu:20.2.0

ARG USER_NAME="vscode"
ARG USER_HOME="/home/vscode"
ARG SQL_DEV_BIN="sqldeveloper-19.2.1.247.2212-no-jre.zip"

ENV USER_NAME=${USER_NAME} \
    INSTALLS_HOME=${USER_HOME}/installs \
    DISPLAY=host.docker.internal:0

USER vscode
WORKDIR ${USER_HOME}

ADD ./installs/${SQL_DEV_BIN} ${INSTALLS_HOME}/${SQL_DEV_BIN}
RUN sudo apt install -y unzip \
    && unzip ./installs/${SQL_DEV_BIN}

CMD ["./sqldeveloper/sqldeveloper.sh"]

