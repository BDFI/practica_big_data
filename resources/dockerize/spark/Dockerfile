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
WORKDIR /
# wget https://ftp.cixug.es/apache/spark/spark-2.4.7/spark-2.4.7-bin-hadoop2.7.tgz && pwd && tar -xvf spark-2.4.7-bin-hadoop2.7.tgz
RUN wget https://archive.apache.org/dist/spark/spark-2.4.0/spark-2.4.0-bin-hadoop2.6.tgz && pwd && tar -xvf spark-2.4.0-bin-hadoop2.6.tgz 
# 2.4.4
# https://archive.apache.org/dist/spark/spark-2.4.4/spark-2.4.4-bin-hadoop2.6.tgz
WORKDIR /spark-2.4.0-bin-hadoop2.6/sbin/
RUN ls
#RUN /start-master.sh

RUN export SPARK_MASTER_HOST=`hostname`

RUN . "/spark-2.4.0-bin-hadoop2.6/sbin/spark-config.sh"

RUN . "/spark-2.4.0-bin-hadoop2.6/bin/load-spark-env.sh"

RUN mkdir -p $SPARK_MASTER_LOG

RUN ln -sf /dev/stdout $SPARK_MASTER_LOG/spark-master.out

WORKDIR cd /spark/bin 

RUN /spark/sbin/../bin/spark-class org.apache.spark.deploy.master.Master \
    --ip $SPARK_MASTER_HOST --port $SPARK_MASTER_PORT --webui-port $SPARK_MASTER_WEBUI_PORT >> $SPARK_MASTER_LOG/spark-master.out

WORKDIR /spark-2.4.0-bin-hadoop2.6/bin
RUN ls
RUN ./spark-submit --version


ARG SBT_VERSION=1.1.5
ARG SBT_HOME=/usr/local/sbt
RUN curl -sL "https://github.com/sbt/sbt/releases/download/v$SBT_VERSION/sbt-$SBT_VERSION.tgz" | tar -xz -C /usr/local
ENV PATH $SBT_HOME/bin:$PATH

# 1,2
WORKDIR /
RUN git clone https://github.com/BDFI/practica_big_data.git
WORKDIR /practica_big_data
RUN resources/download_data.sh

#RUN pip3 install -r requirements.txt
#RUN python3 resources/train_spark_mllib_model.py .
WORKDIR /practica_big_data_2019/flight_prediction/src/main/scala/es/upm/dit/ging/predictor/
# COPY practica_big_data_2019/flight_prediction/src/main/scala/es/upm/dit/ging/predictor/MakePrediction.scala /practica_big_data_2019/flight_prediction/src/main/scala/es/upm/dit/ging/predictor/MakePrediction.scala
RUN sbt compile
RUN sbt package

EXPOSE 8080 7077 6066

WORKDIR /spark-2.4.0-bin-hadoop2.6/bin
RUN ./spark-submit --master local \
--class es.upm.dit.ging.predictor.MakePrediction \
 --packages org.mongodb.spark:mongo-spark-connector_2.11:2.3.2,org.apache.spark:spark-sql-kafka-0-10_2.11:2.4.0 \
~/practica_big_data/flight_prediction/target/scala-2.11/flight_prediction_2.11-0.1.jar 

# CMD ["/bin/bash", "/master.sh"]
