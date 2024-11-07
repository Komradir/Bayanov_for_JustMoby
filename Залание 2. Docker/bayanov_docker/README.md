файлы kafka.service и zookeeper.service создержат базовые конфигурации для одноимённых программ


коментарии к Docker file
===================================================================================================================================

FROM ubuntu:22.04                                                     # выбор базовго образа
                                                                      
MAINTAINER Bayanov Dmitryi                                            # Указание авторства
                                                                      
RUN apt-get update                                                    # обновление пакетов
RUN apt-get install -y curl tar wget default-jdk                      # установка базовых пакетов и jdk
RUN wget https://downloads.apache.org/kafka/3.9.0/kafka_2.12-3.9.0.tgz # скачивание архива с искомой программой 
RUN mkdir /opt/kafka                                                  # создание директории
RUN tar zxf kafka_*.tgz -C /opt/kafka --strip 1                       # распаковка ранее скачанного архива
RUN useradd -r -c 'service user for Kafka' kafka_U                    # создание служебного пользователя
RUN chown -R kafka_U:kafka_U /opt/kafka                               # выдача прав на деррикторию
EXPOSE 9092                                                           # открытие порта 9092 для связи с kafka
COPY zookeeper.service /etc/systemd/system/                           # копирование конфигурации из нашего ПК в имедж         
COPY kafka.service /etc/systemd/system/                               # копирование конфигурации из нашего ПК в имедж
RUN systemctl enable zookeeper kafka                                  # команда для включения автозапуска служб zookeeper и kafka
CMD ['bash /opt/kafka/bin/kafka-server-start.sh']                     # команда выполняемая в уже запущеном контейнере, в данном 
                                                                      # случае это запуск скрипта, который шел в комплекте с kafka.
                                                                       
======================================================================================================================================