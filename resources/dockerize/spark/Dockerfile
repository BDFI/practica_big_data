# 0. Descargar spark y sbt
# 1,2, ejecutar entrenamiento y predicción

# 0
FROM bde2020/spark-base:2.4.4-hadoop2.7

LABEL maintainer="Gezim Sejdiu <g.sejdiu@gmail.com>, Giannis Mouchakis <gmouchakis@gmail.com>"

COPY master.sh /

ENV SPARK_HOME /spark
ENV SPARK_MASTER_PORT 7077
ENV SPARK_MASTER_WEBUI_PORT 8080
ENV SPARK_MASTER_LOG /spark/logs

RUN apt-get update && apt-get install curl tar wget git -y && apt install python3-venv python3-pip -y &&\
    rm -rf /var/lib/apt/lists/*
RUN wget https://www.python.org/ftp/python/3.6.5/Python-3.6.5.tgz
RUN apt-get update && apt-get upgrade && apt-get install -y make build-essential libssl-dev zlib1g-dev libbz2-dev libreadline-dev libsqlite3-dev wget curl llvm libncurses5-dev  libncursesw5-dev xz-utils tk-dev
RUN tar xvf Python-3.6.5.tgz
WORKDIR Python-3.6.5
RUN ./configure --enable-optimizations --with-ensurepip=install && make -j 8 && make altinstall
RUN apt-get update && apt install python3-venv python3-pip -y &&\
    rm -rf /var/lib/apt/lists/*


ARG SBT_VERSION=1.1.5
ARG SBT_HOME=/usr/local/sbt
RUN curl -sL "https://github.com/sbt/sbt/releases/download/v$SBT_VERSION/sbt-$SBT_VERSION.tgz" | tar -xz -C /usr/local
ENV PATH $SBT_HOME/bin:$PATH

# 1,2
WORKDIR /
RUN git clone https://github.com/ging/practica_big_data_2019
WORKDIR /practica_big_data_2019
RUN resources/download_data.sh && mkdir data && mv simple_flight_delay_features.jsonl.bz2 data/simple_flight_delay_features.jsonl.bz2 && mv origin_dest_distances.jsonl data/origin_dest_distances.jsonl
RUN pip3 install -r requirements.txt
RUN python3 resources/train_spark_mllib_model.py .
WORKDIR /practica_big_data_2019/flight_prediction
COPY practica_big_data_2019/flight_prediction/src/main/scala/es/upm/dit/ging/predictor/MakePrediction.scala /practica_big_data_2019/flight_prediction/src/main/scala/es/upm/dit/ging/predictor/MakePrediction.scala
RUN sbt package

EXPOSE 8080 7077 6066

CMD ["/bin/bash", "/master.sh"]
