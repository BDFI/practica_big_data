# 0. Descargar spark, python y sbt
# 1,2, ejecutar entrenamiento y predicción

# 0
FROM ubuntu:18.04

# COPY master.sh /

ENV SPARK_HOME /spark-2.4.0-bin-hadoop2.6
ENV SPARK_MASTER_PORT 7077
ENV SPARK_MASTER_WEBUI_PORT 8080
ENV SPARK_MASTER_LOG /spark/logs

# Instalar spark 2.4.0 (para entrenar y realizar las predicciones). Se despliega un nodo máster local [https://phoenixnap.com/kb/install-spark-on-ubuntu]
RUN apt-get update
RUN apt install openjdk-8-jre openjdk-8-jdk-headless scala git wget curl -y
RUN java -version; javac -version; scala -version; git --version
WORKDIR /home
# wget https://ftp.cixug.es/apache/spark/spark-2.4.7/spark-2.4.7-bin-hadoop2.7.tgz && pwd && tar -xvf spark-2.4.7-bin-hadoop2.7.tgz
RUN wget https://archive.apache.org/dist/spark/spark-2.4.0/spark-2.4.0-bin-hadoop2.6.tgz && pwd && tar -xvf spark-2.4.0-bin-hadoop2.6.tgz 
# 2.4.4
# https://archive.apache.org/dist/spark/spark-2.4.4/spark-2.4.4-bin-hadoop2.6.tgz
WORKDIR /home/spark-2.4.0-bin-hadoop2.6/sbin/
RUN ./start-master.sh

# Instalar pip
WORKDIR /home
RUN sudo apt update
RUN sudo apt install python3-pip
RUN pip3 --version
RUN python3 --version

ARG SBT_VERSION=1.1.5
ARG SBT_HOME=/usr/local/sbt
RUN curl -sL "https://github.com/sbt/sbt/releases/download/v$SBT_VERSION/sbt-$SBT_VERSION.tgz" | tar -xz -C /usr/local
ENV PATH $SBT_HOME/bin:$PATH

# 1,2
WORKDIR /
RUN git clone https://github.com/BDFI/practica_big_data.git
WORKDIR /practica_big_data
RUN resources/download_data.sh

RUN pip3 install -r requirements.txt
RUN python3 resources/train_spark_mllib_model.py .
WORKDIR /practica_big_data_2019/flight_prediction/src/main/scala/es/upm/dit/ging/predictor/
# COPY practica_big_data_2019/flight_prediction/src/main/scala/es/upm/dit/ging/predictor/MakePrediction.scala /practica_big_data_2019/flight_prediction/src/main/scala/es/upm/dit/ging/predictor/MakePrediction.scala
RUN sbt compile
RUN sbt package

EXPOSE 8080 7077 6066

CMD ["/bin/bash", "/master.sh"]
