# syntax=docker/dockerfile:1

################################################################################
# Recipe Begin
FROM ubuntu:latest

# Upgrade Ubuntu
RUN sed -i "s@http://.*archive.ubuntu.com@http://mirrors.huaweicloud.com@g" /etc/apt/sources.list.d/ubuntu.sources \
    && sed -i "s@http://.*security.ubuntu.com@http://mirrors.huaweicloud.com@g" /etc/apt/sources.list.d/ubuntu.sources
RUN apt update -y && apt upgrade -y

# Create user for convenience
RUN groupadd -r hadoop && useradd --no-log-init -r -g hadoop hadoop
RUN echo 'hadoop:hadoop' | chpasswd hadoop

# Install `gosu` to replace `sudo`.
# See https://github.com/tianon/gosu
RUN set -eux && apt update && apt install -y gosu && rm -rf /var/lib/apt/lists/* \
    # verify that the binary works
    && gosu hadoop:hadoop /bin/bash -c 'whoami && id'

# Install dependencies
RUN apt install -y openjdk-8-jdk openssh-server openssh-client
ENV JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64/

# Install Apache Hadoop v3.4.1
WORKDIR /usr/local
ADD --chown=hadoop:hadoop 'https://dlcdn.apache.org/hadoop/common/hadoop-3.4.1/hadoop-3.4.1.tar.gz' /usr/local/
WORKDIR /usr/local/hadoop-3.4.1

# Install OpenSSH Server
USER hadoop:hadoop
RUN mkdir -pv ~hadoop/.ssh/ \
    && ssh-keygen -t ed25519 -P '' -f ~hadoop/.ssh/id_ed25519 \
    && cat ~hadoop/.ssh/id_ed25519.pub > ~hadoop/.ssh/authorized_keys \
    && chmod -v 0600 ~hadoop/.ssh/authorized_keys

# Switch Apache Hadoop into pseudo-distributed mode
RUN rm -v /usr/local/hadoop-3.4.1/etc/hadoop/core-site.xml \
    && rm -v /usr/local/hadoop-3.4.1/etc/hadoop/hdfs-site.xml
COPY configs/*.xml /usr/local/hadoop-3.4.1/etc/hadoop/
RUN bin/hdfs namenode -format

EXPOSE 9000 9864 9866-9868 9870

CMD [ "/bin/bash" ]
# Recipe End
################################################################################

################################################################################
# Addtional information begin
LABEL author="Dragon1573"
LABEL name="hadoop-in-docker"
LABEL version="3.4.1"
LABEL hadoop_mode="pseudo_distributed"
LABEL license="MIT"
LABEL repo_url="https://github.com/Dragon1573/Hadoop-in-Docker.git"
# Addtional information End
################################################################################
