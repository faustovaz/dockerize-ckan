from ubuntu:xenial

RUN apt-get update && apt-get install -y python-dev \
    postgresql \
    libpq-dev \
    python-pip \
    python-virtualenv \
    git-core \
    git \
    solr-jetty \
    openjdk-8-jdk \
    wget \
    vim \
    build-essential \
    libxslt1-dev \
    libxml2-dev \
    zlib1g-dev \
    libffi-dev \
    expect

RUN mkdir -p /usr/lib/ckan/default
RUN mkdir -p /var/lib/ckan/default

ENV CKAN_CONFIG /etc/ckan/default/development.ini

WORKDIR /usr/lib/ckan/default

RUN wget https://raw.githubusercontent.com/ckan/ckan/master/requirement-setuptools.txt && \
    pip install --upgrade pip && \
    pip install -r requirement-setuptools.txt && \
    pip install -e 'git+https://github.com/ckan/ckan.git@ckan-2.7.2#egg=ckan' && \
    pip install -r /usr/lib/ckan/default/src/ckan/requirements.txt && \
    git clone https://github.com/ckan/datapusher && \
    pip install -r datapusher/requirements.txt && \
    pip install -e datapusher/

RUN paster make-config ckan "$CKAN_CONFIG" && \
    paster --plugin=ckan config-tool "$CKAN_CONFIG" -e \
      "ckan.site_title = Ckan" \
      "ckan.storage_path = /var/lib/ckan/default" \
      "ckan.plugins = datastore datapusher recline_view" \
      "ckan.views.default_views = recline_view" \
      "ckan.datastore.write_url = postgresql://ckan_default:ckan@db:5432/datastore_default" \
      "ckan.datastore.read_url = postgresql://datastore_default:ckan@db:5432/datastore_default" \
      "sqlalchemy.url = postgresql://ckan_default:ckan@db:5432/ckan_default" \
      "solr_url = http://solr:8983/solr/ckan" \
      "ckan.redis.url = redis://redis:6379/1" \
      "ckan.site_url = http://localhost:5000" \
      "ckan.datapusher.formats = csv xls xlsx tsv application/csv application/vnd.ms-excel application/vnd.openxmlformats-officedocument.spreadsheetml.sheet" \
      "ckan.datapusher.url = http://localhost:8800/" \
      "ckan.datapusher.assume_task_stale_after = 3600"

RUN ln -s /usr/lib/ckan/default/src/ckan/who.ini /etc/ckan/default/who.ini

COPY ckan_start /bin/
COPY add_admin /bin/

RUN chmod +x /bin/add_admin
RUN chmod +x /bin/ckan_start

CMD ["/bin/ckan_start"]
