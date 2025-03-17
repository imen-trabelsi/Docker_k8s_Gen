FROM python
WORKDIR /usr/src/app
COPY . .

ENV \
POSTGRES_PASSWORD=postgres

RUN \
pip install paho-mqtt && \
pip install psycopg2

CMD ["python", "-m", "app"]
