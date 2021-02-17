###############################################################################################
#                       
# Container used to import the files and epand them. This is ephemeral
# and we will throw it away - stops the added install files being added
# to the final image (I Think)
#                       
###############################################################################################
FROM krisfoster/graal-ee-ol-base:21.0.0.2-JDK8

MAINTAINER krisfoster <me@krismade.me>

COPY gdb-7.11.1-linux-amd64.tar.gz .
RUN tar zxvf gdb-7.11.1-linux-amd64.tar.gz
RUN echo "export PATH=/gdb-7.11.1/bin/:${PATH}" >> /etc/profile
# For JDK 11
# RUN ln -s $GRAALVM_HOME/lib/svm/debug/svmhelpers.py
# For JDK 8
RUN touch ~/.gdbinit
RUN echo "add-auto-load-safe-path ./svmhelpers.py" >> ~/.gdbinit
RUN echo "set print array on" >> ~/.gdbinit
RUN echo "set print pretty on" >> ~/.gdbinit
RUN ln -s $GRAALVM_HOME/jre/lib/svm/debug/svmhelpers.py

# set the maximum size of core files to unlimited
RUN ulimit -c unlimited
# also dump memory that is mapped as read-only
RUN echo 0x7 > /proc/self/coredump_filter
# set the name of the core file
# RUN echo 'core' | tee /proc/sys/kernel/core_pattern
