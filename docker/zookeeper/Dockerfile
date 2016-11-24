FROM alpine:3.4

RUN apk --update add openjdk8-jre tar bash && \
    wget -q ftp://ftp.mirrorservice.org/sites/ftp.apache.org/zookeeper/zookeeper-3.4.9/zookeeper-3.4.9.tar.gz -O /tmp/zookeeper-3.4.9.tar.gz && \
    mkdir /opt && \
    tar xfz /tmp/zookeeper-3.4.9.tar.gz -C /opt && \
    rm /tmp/zookeeper-3.4.9.tar.gz && \
    rm -rf /opt/zookeeper-3.4.9/contrib && \
    rm -rf /opt/zookeeper-3.4.9/src && \
    rm -rf /opt/zookeeper-3.4.9/doc && \
    mv /opt/zookeeper-3.4.9/conf/zoo_sample.cfg /opt/zookeeper-3.4.9/conf/zoo.cfg && \
    apk del tar

EXPOSE 2181

CMD ["/opt/zookeeper-3.4.9/bin/zkServer.sh", "start-foreground"]
