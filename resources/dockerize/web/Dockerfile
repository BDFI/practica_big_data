FROM ubuntu:18.04
ENV TZ=Europe
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone
RUN apt-get update && apt-get install python3.6 -y && apt install python3-venv python3-pip -y
WORKDIR /
RUN apt-get update -y
RUN apt install git-all -y
RUN git clone https://github.com/BDFI/practica_big_data.git
RUN cd practica_big_data/
RUN pip3 install -r /practica_big_data/requirements.txt
ENV PROJECT_HOME=/practica_big_data/
EXPOSE 5000 9200
CMD ["python3", "/practica_big_data/resources/web/predict_flask.py"]
