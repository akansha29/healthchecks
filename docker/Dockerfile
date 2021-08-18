FROM ubuntu:latest

RUN apt-get -y update
RUN apt-get install -y python3-dev default-libmysqlclient-dev build-essential

FROM python:3.9

RUN useradd --system hc
ENV PYTHONUNBUFFERED=1
ENV CRYPTOGRAPHY_DONT_BUILD_RUST=1
WORKDIR /hlthchk/healthchecks

COPY requirements.txt /tmp
RUN \
    pip install --no-cache-dir -r /tmp/requirements.txt && \
    pip install uwsgi

RUN pip install mysqlclient

COPY . /hlthchk/healthchecks/

RUN \
    rm -f /hlthchk/healthchecks/hc/local_settings.py && \
    DEBUG=False SECRET_KEY=build-key ./manage.py collectstatic --noinput && \
    DEBUG=False SECRET_KEY=build-key ./manage.py compress

USER hc

CMD [ "uwsgi", "/hlthchk/healthchecks/docker/uwsgi.ini"]

