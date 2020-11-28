FROM ubuntu:18.04
RUN apt-get update && apt-get install python3.6 -y && apt install python3-venv python3-pip -y
RUN mkdir /practica
COPY practica_big_data_2019/ /practica/
RUN pip3 install -r /practica/requirements.txt
ENV PROJECT_HOME=/practica/
EXPOSE 5000 9200
CMD ["python3", "/practica/resources/web/predict_flask.py"]
