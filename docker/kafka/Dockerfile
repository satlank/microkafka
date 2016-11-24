FROM alpine:3.4

ENV SCALA_VERSION 2.11
ENV KAFKA_VERSION 0.10.1.0
ENV PKG_NAME kafka_"$SCALA_VERSION"-"$KAFKA_VERSION"

RUN apk --update add openjdk8-jre tar bash && \
    wget -q ftp://ftp.mirrorservice.org/sites/ftp.apache.org/kafka/"$KAFKA_VERSION"/"$PKG_NAME".tgz -O /tmp/"$PKG_NAME".tgz && \
    mkdir /opt && \
    tar xfz /tmp/"$PKG_NAME".tgz -C /opt && \
    rm /tmp/"$PKG_NAME".tgz && \
    rm -rf /opt/"$PKG_NAME"/site-docs && \
    apk del tar

EXPOSE 9092

CMD ["/opt/kafka_2.11-0.10.1.0/bin/kafka-server-start.sh", "/opt/kafka_2.11-0.10.1.0/config/server.properties"]
