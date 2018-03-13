FROM webdevops/apache:ubuntu-16.04

MAINTAINER Eric Pfeiffer <computerfr33k@users.noreply.github.com>

ARG PHP_UPSTREAM_CONTAINER=php-fpm
ARG PHP_UPSTREAM_PORT=9000
ARG PHP_UPSTREAM_TIMEOUT=60

ENV WEB_PHP_SOCKET=${PHP_UPSTREAM_CONTAINER}:${PHP_UPSTREAM_PORT}

ENV WEB_DOCUMENT_ROOT=/var/www/

ENV WEB_PHP_TIMEOUT=${PHP_UPSTREAM_TIMEOUT}

# Install OpenSSL
RUN apt-get install openssl -y

# Create folder for storing Certs
RUN mkdir /etc/apache2/ssl 2> /dev/null

# Create Certs
RUN openssl genrsa -out "/etc/apache2/ssl/sslcert.key" 2048 && \
    openssl req -new -key "/etc/apache2/ssl/sslcert.key" -out "/etc/apache2/ssl/sslcert.csr" -subj "/CN=sslcert/O=sslcert/C=UK" && \
    openssl x509 -req -days 365 -in "/etc/apache2/ssl/sslcert.csr" -signkey "/etc/apache2/ssl/sslcert.key" -out "/etc/apache2/ssl/sslcert.crt"

# Enable Proxy
RUN a2enmod proxy && \
    a2enmod proxy_balancer && \
    a2enmod proxy_http

EXPOSE 8080 8443

WORKDIR /var/www/

COPY vhost.conf /etc/apache2/sites-enabled/vhost.conf

ENTRYPOINT ["/opt/docker/bin/entrypoint.sh"]

CMD ["supervisord"]
