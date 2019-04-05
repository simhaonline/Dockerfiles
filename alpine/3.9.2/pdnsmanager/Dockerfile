FROM alpine:3.9

ENV  PDNSMANAGER_VERSION 2.0.1
ENV  PDNSMANAGER_ARCHIVE=pdnsmanager-${PDNSMANAGER_VERSION}.tar.gz
ENV  PDNSMANAGER_DOWNLOAD_URL=https://dl.pdnsmanager.org/${PDNSMANAGER_ARCHIVE}

RUN apk add curl apache2 apache2-ctl php7-apache2 php7 php7-mysqli php7-pdo php7-pdo_mysql php7-json php7-pecl-apcu mariadb-client bash && \
    mkdir /var/www/html && 

COPY $PDNSMANAGER_ARCHIVE /tmp
COPY conf/apache-vhost.conf /etc/apache2/conf.d/pdnsmanager.conf
COPY conf/ConfigUser.php /var/www/html/backend/config
COPY scripts/pdnsmanager_schema_v6_extensions.sql /tmp/pdnsmanager_schema_v6_extensions.sql
COPY scripts/app_start /usr/local/bin/app_start
COPY scripts/dbsetup /usr/local/bin/dbsetup

RUN tar -C /var/www/html --strip-components=1 -xzf /tmp/$PDNSMANAGER_ARCHIVE pdnsmanager-${PDNSMANAGER_VERSION}/backend  pdnsmanager-${PDNSMANAGER_VERSION}/frontend && \
    chown apache:apache -R /var/www/html && \
    rm -f /tmp/$PDNSMANAGER_ARCHIVE && \
    sed -i '/LoadModule rewrite_module/s/^#//g' /etc/apache2/httpd.conf

RUN chown apache:apache -R /var/www/html

WORKDIR /etc/apache2
ENTRYPOINT [ "/usr/local/bin/app_start" ]
CMD [ "apachectl", "-d", ".", "-f", "httpd.conf", "-e", "info", "-DFOREGROUND" ]
