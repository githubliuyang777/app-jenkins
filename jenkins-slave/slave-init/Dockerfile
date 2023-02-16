FROM timbru31/alpine-java-maven:latest

MAINTAINER tommylikehu@gmail.com

USER root

RUN apk update && \
    apk add curl && \
    apk add jq && \
    apk add xmlstarlet

COPY entrypoint.sh /usr/local/bin/

RUN ls /usr/local/bin/entrypoint.sh

ENTRYPOINT ["/usr/local/bin/entrypoint.sh"]
