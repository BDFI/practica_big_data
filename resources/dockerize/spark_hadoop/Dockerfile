FROM bde2020/spark-base:2.4.4-hadoop2.7

LABEL maintainer="Gezim Sejdiu <g.sejdiu@gmail.com>, Giannis Mouchakis <gmouchakis@gmail.com>"

COPY master.sh /

ENV SPARK_HOME /spark
ENV SPARK_MASTER_PORT 7077
ENV SPARK_MASTER_WEBUI_PORT 8080
ENV SPARK_MASTER_LOG /spark/logs

RUN apk add git
RUN git clone https://github.com/BDFI/practica_big_data
WORKDIR /practica_big_data/flight_prediction
EXPOSE 8080 7077 6066

CMD ["/bin/bash", "/master.sh"]
