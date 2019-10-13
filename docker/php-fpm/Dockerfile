
ARG PHP_VERSION=7.3

FROM php:${PHP_VERSION}-fpm-stretch

# Dependencies and extensions
RUN apt-get update && apt-get upgrade -y \
    && apt-get install -y \
    libbz2-dev \
    libc-client-dev \
    libcurl4-gnutls-dev \
    libfreetype6-dev \
    libicu-dev \
    libjpeg62-turbo-dev \
    libmcrypt-dev \
    libreadline-dev \
    libxslt1-dev \
    libzip-dev \
    wget \
    unzip \
    && docker-php-ext-configure gd \
    --with-freetype-dir=/usr/include/ \
    --with-jpeg-dir=/usr/include/ \
    --with-png-dir=/usr/include/ \
    && docker-php-ext-install -j$(nproc) gd \
        opcache \
        intl \
    && docker-php-ext-configure intl \
    && docker-php-ext-enable \
        intl \
        opcache \
    && docker-php-ext-install -j$(nproc) intl \
    && docker-php-ext-install -j$(nproc) \
    bcmath \
    bz2 \
    calendar \
    exif \
    gettext \
    pdo_mysql \
    soap \
    sockets \
    xmlrpc \
    xsl \
    && docker-php-ext-configure zip --with-libzip \
    && docker-php-ext-install zip \
    && pecl install xdebug && docker-php-ext-enable xdebug \
    && docker-php-source delete \
    && apt-get remove -y g++ wget \
    && apt-get autoremove --purge -y && apt-get autoclean -y && apt-get clean -y \
    && rm -rf /var/lib/apt/lists/* \
    && rm -rf /tmp/* /var/tmp/*

# Composer
ENV COMPOSER_ALLOW_SUPERUSER 1
RUN php -r "readfile('https://getcomposer.org/installer');" | php -- --install-dir=/usr/local/bin --filename=composer && \
    chmod +x /usr/local/bin/composer

# NodeJS and Yarn
RUN apt-get update && \
    apt-get install -y --no-install-recommends gnupg && \
    curl -sL https://deb.nodesource.com/setup_12.x | bash - && \
    apt-get update && \
    apt-get install -y --no-install-recommends nodejs && \
    curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | apt-key add - && \
    echo "deb https://dl.yarnpkg.com/debian/ stable main" | tee /etc/apt/sources.list.d/yarn.list && \
    apt-get update && \
    apt-get install -y --no-install-recommends yarn

COPY conf.d/app.ini $PHP_INI_DIR/conf.d/app.ini

CMD ["php-fpm"]

WORKDIR /srv/www

EXPOSE 9000
