FROM ubuntu:22.04

MAINTAINER Bayanov Dmitryi

RUN apt-get update 
RUN apt-get install -y curl tar wget default-jdk
RUN wget https://downloads.apache.org/kafka/3.9.0/kafka_2.12-3.9.0.tgz
RUN mkdir /opt/kafka
RUN tar zxf kafka_*.tgz -C /opt/kafka --strip 1  
RUN useradd -r -c 'service user for Kafka' kafka_U
RUN chown -R kafka_U:kafka_U /opt/kafka
EXPOSE 9092 
COPY zookeeper.service /etc/systemd/system/
COPY kafka.service /etc/systemd/system/
RUN systemctl enable zookeeper kafka
CMD ['bash /opt/kafka/bin/kafka-server-start.sh']
