FROM jupyter/scipy-notebook

USER root

RUN set -eux; \
    apt-get -y update; \
    apt-get install --no-install-recommends -y \
        openjdk-8-jre-headless \
        gnupg ca-certificates-java;  \
    apt-get clean; rm -rf /var/lib/apt/lists/*; \
    usermod -aG sudo ${NB_USER}; echo "${NB_USER}:juno" | chpasswd

ENV SPARK_VERSION=2.3.2  \
    HADOOP_VERSION=2.8.5 \
    SPARK_HOME=/usr/local/spark   \
    HADOOP_HOME=/usr/local/hadoop \
    JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64/

# Main Apache mirrors:
#   * https://apache.org/dist
#   * https://archive.apache.org/dist
#   * https://dist.apache.org/repos/dist/release
# List all Apache mirrors:
#   * https://apache.org/mirrors
ARG APACHE_DIST=https://apache.org/dist
ARG APACHE_MIRROR=https://dist.apache.org/repos/dist/release

RUN set -eux; \
    cd /tmp;  \
    wget -qO - "${APACHE_DIST}/hadoop/common/KEYS" | gpg --import -; \
    wget       "${APACHE_MIRROR}/hadoop/common/hadoop-${HADOOP_VERSION}/hadoop-${HADOOP_VERSION}.tar.gz" --progress=bar:force; \
    wget -qO - "${APACHE_DIST}/hadoop/common/hadoop-${HADOOP_VERSION}/hadoop-${HADOOP_VERSION}.tar.gz.asc" | gpg --verify - "hadoop-${HADOOP_VERSION}.tar.gz"; \
    tar -xzf "hadoop-${HADOOP_VERSION}.tar.gz" -C /usr/local --owner root --group root --no-same-owner; \
    rm  -rf  "hadoop-${HADOOP_VERSION}.tar.gz" "$HOME/.gnupg"; \
    cd /usr/local; \
    ln  -s   "hadoop-${HADOOP_VERSION}" hadoop

RUN set -eux; \
    cd /tmp;  \
    wget -qO - "${APACHE_DIST}/spark/KEYS" | gpg --import -; \
    wget       "${APACHE_MIRROR}/spark/spark-${SPARK_VERSION}/spark-${SPARK_VERSION}-bin-without-hadoop.tgz" --progress=bar:force; \
    wget -qO - "${APACHE_DIST}/spark/spark-${SPARK_VERSION}/spark-${SPARK_VERSION}-bin-without-hadoop.tgz.asc" | gpg --verify - "spark-${SPARK_VERSION}-bin-without-hadoop.tgz"; \
    tar -xzf "spark-${SPARK_VERSION}-bin-without-hadoop.tgz" -C /usr/local --owner root --group root --no-same-owner; \
    rm  -rf  "spark-${SPARK_VERSION}-bin-without-hadoop.tgz" "$HOME/.gnupg"; \
    cd /usr/local; \
    ln  -s   "spark-${SPARK_VERSION}-bin-without-hadoop" spark; \
    echo 'SPARK_DIST_CLASSPATH=$(${HADOOP_HOME}/bin/hadoop classpath)' > "${SPARK_HOME}/conf/spark-env.sh"; \
    chmod +x "${SPARK_HOME}/conf/spark-env.sh"

RUN set -eux; \
    cd ${SPARK_HOME}/jars; \
    wget http://central.maven.org/maven2/org/apache/hadoop/hadoop-aws/2.8.5/hadoop-aws-2.8.5.jar; \
    wget http://central.maven.org/maven2/org/apache/httpcomponents/httpclient/4.5.3/httpclient-4.5.3.jar; \
    wget http://central.maven.org/maven2/com/amazonaws/aws-java-sdk-core/1.11.234/aws-java-sdk-core-1.11.234.jar; \
    wget http://central.maven.org/maven2/com/amazonaws/aws-java-sdk/1.11.234/aws-java-sdk-1.11.234.jar; \
    wget http://central.maven.org/maven2/com/amazonaws/aws-java-sdk-kms/1.11.234/aws-java-sdk-kms-1.11.234.jar; \
    wget http://central.maven.org/maven2/com/amazonaws/aws-java-sdk-s3/1.11.234/aws-java-sdk-s3-1.11.234.jar

ENV SPARK_OPTS="--driver-java-options=-Xms1024M --driver-java-options=-Xmx4096M --driver-java-options=-Dlog4j.logLevel=info" \
    PYTHONPATH="${SPARK_HOME}/python:${SPARK_HOME}/python/lib/py4j-0.10.7-src.zip" \
    PATH="${SPARK_HOME}/bin:${HADOOP_HOME}/bin:${PATH}"

USER $NB_UID

RUN set -eux; \
    conda install -y pyarrow; \
    conda clean   -tipsy; \
    fix-permissions $CONDA_DIR; \
    fix-permissions /home/$NB_USER
