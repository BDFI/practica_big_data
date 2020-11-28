FROM ubuntu:18.04
RUN apt-get update && apt-get install python3.6 -y && apt install python3-venv python3-pip -y
WORKDIR /
RUN apt-get update
RUN apt install git-all
RUN git clone https://github.com/ging/practica_big_data_2019
RUN cd practica_big_data_2019/
RUN pip3 install -r /practica_big_data_2019/requirements.txt
ENV PROJECT_HOME=/practica_big_data_2019/
EXPOSE 5000 9200
CMD ["python3", "/practica_big_data_2019/resources/web/predict_flask.py"]
