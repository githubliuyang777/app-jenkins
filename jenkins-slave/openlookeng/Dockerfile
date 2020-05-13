FROM centos:centos7
LABEL maintainer="Hetu Community"

ARG VERSION=3.35
ARG user=jenkins
ARG group=jenkins
ARG uid=1000
ARG gid=1000
ARG AGENT_WORKDIR=/home/${user}/agent
ENV JAVA_HOME /usr
ENV LANG en_US.UTF-8
ENV M2_HOME /opt/apache-maven
ENV AGENT_WORKDIR=${AGENT_WORKDIR}
ENV PATH ${M2_HOME}/bin:${PATH}

# Install base packages
RUN yum -y -q update &&\
    yum -y -q install git java-1.8.0-openjdk-devel sudo less wget libnuma1 libaio1 &&\
    yum -q clean all &&\
    rm -rf /var/cache/yum /tmp/* /var/tmp/*

# Add jenkins user and group
RUN groupadd ${group} --gid ${uid} && \
    useradd ${user} --uid ${uid} --gid ${gid} && \
    echo 'jenkins:admin' | chpasswd &&\
    gpasswd -a ${user} wheel &&\
    echo '${user} ALL=(ALL) NOPASSWD:ALL' | tee -a /etc/sudoers

# Install maven 3.6.3
RUN wget http://mirrors.tuna.tsinghua.edu.cn/apache/maven/maven-3/3.6.3/binaries/apache-maven-3.6.3-bin.tar.gz &&\
    tar -zxvf apache-maven-3.6.3-bin.tar.gz -C /opt && mv /opt/apache-maven-3.6.3 /opt/apache-maven && rm -rf apache-maven-3.6.3-bin.tar.gz &&\
    sudo chown -R ${user}:${group} /opt/apache-maven

# Install docker (only cli will be used)
RUN curl -fsSL https://get.docker.com -o get-docker.sh && \
    sudo sh get-docker.sh && \
    sudo usermod -aG docker ${user}

# Install jenkins slave
RUN mkdir -p /usr/share/jenkins/ \
    && cd /usr/share/jenkins/ \
    && wget -O agent.jar https://repo.jenkins-ci.org/public/org/jenkins-ci/main/remoting/${VERSION}/remoting-${VERSION}.jar \
    && chmod 755 /usr/share/jenkins \
    && chmod 644 /usr/share/jenkins/agent.jar \
    && ln -sf /usr/share/jenkins/agent.jar /usr/share/jenkins/slave.jar

# Install jnlp
COPY jenkins-agent /usr/local/bin/jenkins-agent
RUN chmod 755 /usr/local/bin/jenkins-agent \
    && ln -s /usr/local/bin/jenkins-agent /usr/local/bin/jenkins-slave

# Setting up env
USER ${user}
RUN mkdir /home/${user}/.jenkins && mkdir -p ${AGENT_WORKDIR}

# Change timezone
ENV TZ=Asia/Shanghai
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

VOLUME /home/${user}/.jenkins
VOLUME ${AGENT_WORKDIR}
WORKDIR /home/${user}

ENTRYPOINT ["jenkins-agent"]
