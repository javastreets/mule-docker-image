FROM resin/raspberrypi3-openjdk

LABEL maintainer="https://manik.magar.me"

# Define environment variables.
ENV BUILD_DATE=09302017
ENV MULE_HOME=/opt/mule
ENV MULE_VERSION=3.8.1
ENV JAVA_WRAPPER_VERSION=3.5.34
ENV JAVA_WRAPPER=wrapper-linux-armhf-32-${JAVA_WRAPPER_VERSION}
ENV TINI_SUBREAPER=

RUN apt-get update && \
    apt-get upgrade

# For checksum, alpine linux needs two spaces between checksum and file name
RUN cd ~ && wget https://repository-master.mulesoft.org/nexus/content/repositories/releases/org/mule/distributions/mule-standalone/${MULE_VERSION}/mule-standalone-3.8.1.tar.gz && \
    echo "db079c0fc01c534d443277cfe96ab252  mule-standalone-3.8.1.tar.gz" | md5sum -c && \
    cd /opt && \
    tar xvzf ~/mule-standalone-${MULE_VERSION}.tar.gz && \
    rm ~/mule-standalone-${MULE_VERSION}.tar.gz && \
    ln -s /opt/mule-standalone-${MULE_VERSION} ${MULE_HOME} && \
    mv ${MULE_HOME}/lib/boot/wrapper-3.2.3.jar ${MULE_HOME}/lib/boot/wrapper-3.2.3.jar.original && \
    mv ${MULE_HOME}/bin/mule ${MULE_HOME}/bin/mule.original && \
    mv ${MULE_HOME}/conf/wrapper.conf ${MULE_HOME}/conf/wrapper.conf.original

COPY  java_wrapper/libwrapper-linux-armhf-32.so ${MULE_HOME}/lib/boot/
COPY java_wrapper/wrapper-3.5.34.jar ${MULE_HOME}/lib/boot/
COPY java_wrapper/wrapper-linux-armhf-32 ${MULE_HOME}/lib/boot/exec/
COPY mule ${MULE_HOME}/bin/
COPY wrapper.conf ${MULE_HOME}/conf/

RUN sudo adduser --disabled-password --gecos "" mule && \
    chown mule:mule -R /opt/mule-standalone-3.8.1 && \
    chown mule:mule -R /opt/mule && \
    chmod u+x /opt/mule-standalone-${MULE_VERSION}/lib/boot/exec/wrapper-linux-armhf-32

# Define mount points.
VOLUME ["${MULE_HOME}/logs", "${MULE_HOME}/conf", "${MULE_HOME}/apps", "${MULE_HOME}/domains"]

# Define working directory.
WORKDIR ${MULE_HOME}

CMD [ "/opt/mule/bin/mule"]

# Default http port
EXPOSE 8081
