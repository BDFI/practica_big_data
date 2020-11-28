FROM mongo

WORKDIR /
RUN git clone https://github.com/ging/practica_big_data_2019
COPY practica_big_data_2019/ /home
WORKDIR /home
CMD resources/import_distances.sh
