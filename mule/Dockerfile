FROM anapsix/alpine-java:8_jdk_unlimited

LABEL maintainer="https://manik.magar.me"

# Define environment variables.
ENV BUILD_DATE=09292017
ENV MULE_HOME=/opt/mule
ENV MULE_VERSION=3.8.1
ENV TINI_SUBREAPER=

# SSL Cert for downloading mule zip
RUN apk --no-cache update && \
    apk --no-cache upgrade && \
    apk --no-cache add ca-certificates && \
    update-ca-certificates && \
    apk --no-cache add openssl

# For checksum, alpine linux needs two spaces between checksum and file name
RUN cd ~ && wget https://repository-master.mulesoft.org/nexus/content/repositories/releases/org/mule/distributions/mule-standalone/${MULE_VERSION}/mule-standalone-${MULE_VERSION}.tar.gz && \
    echo "db079c0fc01c534d443277cfe96ab252  mule-standalone-${MULE_VERSION}.tar.gz" | md5sum -c

RUN cd /opt && \
    tar xvzf ~/mule-standalone-${MULE_VERSION}.tar.gz && \
    rm ~/mule-standalone-${MULE_VERSION}.tar.gz && \
    ln -s /opt/mule-standalone-${MULE_VERSION} ${MULE_HOME}

RUN adduser -D -g "" mule mule

RUN chown mule:mule -R /opt/mule-standalone-3.8.1 && \
    chown mule:mule -R /opt/mule

# Define mount points.
VOLUME ["${MULE_HOME}/logs", "${MULE_HOME}/conf", "${MULE_HOME}/apps", "${MULE_HOME}/domains"]

# Define working directory.
WORKDIR ${MULE_HOME}

CMD [ "/opt/mule/bin/mule"]

# Default http port
EXPOSE 8081
