FROM openjdk:8-jre-alpine

ARG JMETER_VERSION="4.0"
ARG JMETER_DOWNLOAD_URL=http://mirror.downloadvn.com/apache//jmeter/binaries/apache-jmeter-${JMETER_VERSION}.tgz

RUN set -eux; \
    apk add -U curl tar; \
    mkdir /opt; cd /opt; \
    curl -O ${JMETER_DOWNLOAD_URL}; \
    tar -xzvf apache-jmeter-${JMETER_VERSION}.tgz; \
    mv apache-jmeter-${JMETER_VERSION} apache-jmeter; \
    rm -rf apache-jmeter-${JMETER_VERSION}.tgz

ENV JMETER_HOME=/opt/apache-jmeter
ENV PATH=${PATH}:${JMETER_HOME}/bin
