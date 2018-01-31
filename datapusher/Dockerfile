FROM ubuntu:xenial

RUN apt-get update && apt-get -y install python-dev \
    python-virtualenv \
    python-pip \
    build-essential \
    libxslt1-dev \
    libxml2-dev \
    zlib1g-dev \
    git \
    libffi-dev

RUN mkdir -p /var/lib/ckan/default
WORKDIR /usr/lib/ckan/default

RUN git clone https://github.com/ckan/datapusher && \
    pip install --upgrade pip && \
    pip install -r datapusher/requirements.txt && \
    pip install -e datapusher/

EXPOSE 8800

CMD ["python", "/usr/lib/ckan/default/datapusher/datapusher/main.py", "/usr/lib/ckan/default/datapusher/deployment/datapusher_settings.py"]
