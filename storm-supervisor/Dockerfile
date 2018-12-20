FROM ubuntu

# Install required packages

RUN apt-get update && \
    apt-get install -y build-essential && \
    apt-get install -y software-properties-common && \
    apt-get install -y python

# Install Java.

RUN \
  echo oracle-java8-installer shared/accepted-oracle-license-v1-1 select true | debconf-set-selections && \
  add-apt-repository -y ppa:webupd8team/java && \
  apt-get update && \
  apt-get install -y oracle-java8-installer && \
  rm -rf /var/lib/apt/lists/* && \
  rm -rf /var/cache/oracle-jdk8-installer

ENV JAVA_HOME /usr/lib/jvm/java-8-oracle


# Install Storm.

ENV STORM_USER=usama \
    STORM_CONF_DIR=/conf \
    STORM_DATA_DIR=/data \
    STORM_LOG_DIR=/logs

# Add a user and make dirs
RUN set -ex; \
    adduser "$STORM_USER"; \
    mkdir -p "$STORM_CONF_DIR" "$STORM_DATA_DIR" "$STORM_LOG_DIR"; \
    chown -R "$STORM_USER:$STORM_USER" "$STORM_CONF_DIR" "$STORM_DATA_DIR" "$STORM_LOG_DIR"``

ARG DISTRO_NAME=apache-storm-0.9.2-incubating

# Download Apache Storm.
RUN set -ex; \
    wget -q "https://archive.apache.org/dist/storm/${DISTRO_NAME}/${DISTRO_NAME}.tar.gz"; \
    export GNUPGHOME="$(mktemp -d)"; \
    tar -xzf "${DISTRO_NAME}.tar.gz"; \
    chown -R "$STORM_USER:$STORM_USER" "${DISTRO_NAME}"; \
    rm "${DISTRO_NAME}.tar.gz";

ADD storm.yaml "${DISTRO_NAME}/conf/"

ENV PATH $PATH:/${DISTRO_NAME}/bin

RUN chown -R "$STORM_USER" "$STORM_CONF_DIR" "$STORM_DATA_DIR" "$STORM_LOG_DIR"


# Install Maven.

ENV MAVEN_VERSION="3.6.0" \
    M2_HOME=/usr/lib/mvn

RUN cd /tmp && \
    wget "http://apache.mirrors.hoobly.com/maven/maven-3/3.6.0/binaries/apache-maven-3.6.0-bin.tar.gz" && \
    tar -zxvf "apache-maven-3.6.0-bin.tar.gz" && \
    mv "apache-maven-3.6.0" "$M2_HOME" && \
    ln -s "$M2_HOME/bin/mvn" /usr/bin/mvn && \
    rm -R /tmp/*