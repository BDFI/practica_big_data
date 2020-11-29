FROM mongo

ENV TZ=Europe
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone
RUN apt-get update -y
RUN apt install git-all -y
WORKDIR /
RUN git clone https://github.com/BDFI/practica_big_data
#COPY practica_big_data/ /home
#WORKDIR /home
WORKDIR /practica_big_data
# CMD resources/import_distances.sh
