FROM ubuntu:latest

# 更新系统
RUN sed -i "s@http://.*archive.ubuntu.com@http://mirrors.huaweicloud.com@g" /etc/apt/sources.list.d/ubuntu.sources \
    && sed -i "s@http://.*security.ubuntu.com@http://mirrors.huaweicloud.com@g" /etc/apt/sources.list.d/ubuntu.sources
RUN apt update -y && apt upgrade -y
RUN apt install -y sudo

# 创建用户
RUN useradd -G sudo -m -s /bin/bash hadoop
RUN echo 'hadoop:hadoop' | chpasswd hadoop

# 安装依赖项
RUN apt install -y openjdk-8-jdk openssh-server openssh-client wget
ENV JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64/

# 安装 Hadoop
WORKDIR /usr/local
RUN wget --verbose 'https://dlcdn.apache.org/hadoop/common/hadoop-3.4.1/hadoop-3.4.1.tar.gz' \
    && tar -zxvf 'hadoop-3.4.1.tar.gz' \
    && rm -v 'hadoop-3.4.1.tar.gz' \
    && chown -Rvf hadoop:hadoop hadoop-3.4.1
WORKDIR /usr/local/hadoop-3.4.1
RUN ["bin/hadoop", "version"]

# 安装 OpenSSH Server
USER hadoop
RUN mkdir -pv ~hadoop/.ssh/ \
    && ssh-keygen -t ed25519 -P '' -f ~hadoop/.ssh/id_ed25519 \
    && cat ~hadoop/.ssh/id_ed25519.pub > ~hadoop/.ssh/authorized_keys \
    && chmod -v 0600 ~hadoop/.ssh/authorized_keys

# 切换 Hadoop 为单节点为分布式
RUN rm -v /usr/local/hadoop-3.4.1/etc/hadoop/core-site.xml \
    && rm -v /usr/local/hadoop-3.4.1/etc/hadoop/hdfs-site.xml
COPY configs/*.xml /usr/local/hadoop-3.4.1/etc/hadoop/
RUN bin/hdfs namenode -format

# 暴露必要端口
EXPOSE 9000 9864 9866-9868 9870

# 容器保活
CMD [ "/bin/bash" ]
