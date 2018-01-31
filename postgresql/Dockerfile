FROM ubuntu:xenial

RUN apt-key adv --keyserver hkp://p80.pool.sks-keyservers.net:80 --recv-keys B97B0AFCAA1A47F044F244A07FCC7D46ACCC4CF8 && \
    echo "deb http://apt.postgresql.org/pub/repos/apt/ precise-pgdg main" > /etc/apt/sources.list.d/pgdg.list && \
    apt-get update && \
    apt-get install -y  python-software-properties \
      software-properties-common postgresql-9.3 \
      postgresql-client-9.3 postgresql-contrib-9.3

USER postgres

COPY set_permissions.sql /var/lib/postgresql/

RUN /etc/init.d/postgresql start && \
    createuser -S -D -R ckan_default && \
    psql -c "ALTER USER ckan_default WITH PASSWORD 'ckan'" && \
    createdb -O ckan_default ckan_default -E utf-8 && \
    createuser -S -D -R -l datastore_default && \
    psql -c "ALTER USER datastore_default WITH PASSWORD 'ckan'" && \
    createdb -O ckan_default datastore_default -E utf-8 && \
    cat /var/lib/postgresql/set_permissions.sql | psql

RUN echo "host all all 0.0.0.0/0 md5" >> /etc/postgresql/9.3/main/pg_hba.conf && \
    echo "listen_addresses='*'" >> /etc/postgresql/9.3/main/postgresql.conf

#VOLUME ["/var/lib/postgresql/data"]

EXPOSE 5432

CMD ["/usr/lib/postgresql/9.3/bin/postgres", "-D", "/var/lib/postgresql/9.3/main", "-c", "config_file=/etc/postgresql/9.3/main/postgresql.conf"]
