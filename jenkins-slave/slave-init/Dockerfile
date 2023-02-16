FROM smartthingsoss/jenkins-agent-jdk11:latest

MAINTAINER tommylikehu@gmail.com

USER root
WORKDIR /opt/jenkins-aget/

COPY entrypoint.sh /opt/jenkins-aget/

RUN apt-get update \
    && apt-get upgrade \
    && apt-get install -y xmlstarlet \
    && apt-get install -y curl \
    && apt-get install -y jq
    
RUN ls /opt/jenkins-aget/entrypoint.sh \
    && chmod +x /opt/jenkins-aget/entrypoint.sh

ENTRYPOINT ["/opt/jenkins-aget/entrypoint.sh"]
