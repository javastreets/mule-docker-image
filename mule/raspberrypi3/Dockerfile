FROM resin/raspberrypi3-openjdk

LABEL maintainer="https://manik.magar.me"

# Define environment variables.
ENV BUILD_DATE=11112017
ENV MULE_HOME=/opt/mule
ENV MULE_VERSION=3.9.0
ENV MULE_MD5=39b773bf20702f614faf30b2ffca4716
ENV JAVA_WRAPPER_VERSION=3.5.34
ENV JAVA_WRAPPER=wrapper-linux-armhf-32-${JAVA_WRAPPER_VERSION}
ENV TINI_SUBREAPER=
ENV TZ=America/New_York

RUN apt-get update && \
    apt-get upgrade && \
    rm -rf /var/lib/apt/lists

RUN sudo adduser --disabled-password --gecos "" mule

RUN mkdir /tmp/mule-temp && \
    mkdir /opt/mule-standalone-${MULE_VERSION} && \
    ln -s /opt/mule-standalone-${MULE_VERSION} ${MULE_HOME} && \
    chown mule:mule -R /opt/mule*

RUN echo ${TZ} > /etc/timezone && \
    dpkg-reconfigure -f noninteractive tzdata

COPY java_wrapper/libwrapper-linux-armhf-32.so /tmp/mule-temp/
COPY java_wrapper/wrapper-3.5.34.jar /tmp/mule-temp/
COPY java_wrapper/wrapper-linux-armhf-32 /tmp/mule-temp/
COPY mule /tmp/mule-temp/
COPY wrapper.conf /tmp/mule-temp/

RUN chown mule:mule -R /tmp/mule-temp

RUN chmod u+x /tmp/mule-temp/wrapper-linux-armhf-32

USER mule

RUN cd ~ && wget https://repository-master.mulesoft.org/nexus/content/repositories/releases/org/mule/distributions/mule-standalone/${MULE_VERSION}/mule-standalone-${MULE_VERSION}.tar.gz && \
    echo "${MULE_MD5}  mule-standalone-${MULE_VERSION}.tar.gz" | md5sum -c && \
    cd /opt && \
    tar xvzf ~/mule-standalone-${MULE_VERSION}.tar.gz && \
    rm ~/mule-standalone-${MULE_VERSION}.tar.gz

RUN mv ${MULE_HOME}/lib/boot/wrapper-3.2.3.jar ${MULE_HOME}/lib/boot/wrapper-3.2.3.jar.original && \
    mv ${MULE_HOME}/bin/mule ${MULE_HOME}/bin/mule.original && \
    mv ${MULE_HOME}/conf/wrapper.conf ${MULE_HOME}/conf/wrapper.conf.original && \
    mv /tmp/mule-temp/libwrapper-linux-armhf-32.so ${MULE_HOME}/lib/boot/ && \
    mv /tmp/mule-temp/wrapper-3.5.34.jar ${MULE_HOME}/lib/boot/ && \
    mv /tmp/mule-temp/wrapper-linux-armhf-32 ${MULE_HOME}/lib/boot/exec/ && \
    mv /tmp/mule-temp/mule ${MULE_HOME}/bin/ && \
    mv /tmp/mule-temp/wrapper.conf ${MULE_HOME}/conf/


# Define mount points.
VOLUME ["${MULE_HOME}/logs", "${MULE_HOME}/conf", "${MULE_HOME}/apps", "${MULE_HOME}/domains"]

# Define working directory.
WORKDIR ${MULE_HOME}

CMD [ "/opt/mule/bin/mule"]

# Default http port
EXPOSE 8081
