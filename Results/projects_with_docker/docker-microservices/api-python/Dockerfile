FROM tiangolo/meinheld-gunicorn-flask:python3.8-alpine3.11

RUN apk --no-cache add build-base bash postgresql-dev

WORKDIR /app

# set environment variables
ENV PYTHONDONTWRITEBYTECODE 1
ENV PYTHONUNBUFFERED 1

COPY ./app/requirements.txt ./

RUN pip install -U pip && pip install -r requirements.txt
