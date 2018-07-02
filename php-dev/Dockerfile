# debian:jessie (8)
# php:7.1
FROM  php:7.1
LABEL maintainer="Dũng Đặng Minh <dungdm93@gmail.com>"

RUN set -eux; \
    echo "===> Install composer..."; \
    curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer; \
    \
    echo "===> Install php-dev & composer requirements..."; \
    apt-get update; \
    # php-dev aviable in php:7.2 => debian:stretch
    # apt-get install --no-install-recommends -y php-dev; \
    # Reference: Composer https://github.com/composer/docker/blob/master/1.4/Dockerfile
    apt-get install -y --no-install-recommends openssl git subversion mercurial; \
    \
    echo "===> Install envsubst..."; \
    apt-get install -y --no-install-recommends gettext-base; \
    cp "$(which envsubst)" /usr/local/bin/; \
    apt-get purge -y --auto-remove gettext-base; \
    \
    echo "===> Clean up..."; \
    rm -rf /var/lib/apt/lists/* /var/cache/apt/*;

RUN set -eux; \
    apt-get update; \
    \
    echo "===> Install php-zip..."; \
    apt-get install -y --no-install-recommends zlib1g-dev; \
        docker-php-ext-install zip; \
    \
    echo "===> Install php-xdebug..."; \
    pecl install xdebug; \
        docker-php-ext-enable xdebug; \
    \
    echo "===> Install php-pdo..."; \
    apt-get install -y --no-install-recommends sqlite3 libsqlite3-dev libmysqlclient-dev libpq-dev; \
        docker-php-ext-install pdo pdo_sqlite pdo_mysql pdo_pgsql; \
    \
    echo "===> Clean up..."; \
    rm -rf /var/lib/apt/lists/* /var/cache/apt/*;

####### INSTRUCTIONS #######
# Build image:
# $ docker build -t dungdm93/php-dev:7.1-debian .
# Run image:
# $ docker run -it --rm --name php-dev dungdm93/php-dev:7.1-debian bash
############################
