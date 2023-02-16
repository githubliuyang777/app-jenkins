FROM timbru31/alpine-java-maven:latest

MAINTAINER tommylikehu@gmail.com

USER root
WORKDIR /opt/jenkins-aget/

RUN apk update && \
    apk add curl && \
    apk add jq && \
    apk add xmlstarlet

COPY entrypoint.sh /opt/jenkins-aget/

RUN ls /opt/jenkins-aget/entrypoint.sh \
    && chmod +x /opt/jenkins-aget/entrypoint.sh

ENTRYPOINT ["/opt/jenkins-aget/entrypoint.sh"]
