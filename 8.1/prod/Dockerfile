FROM php:8.1-fpm-bullseye

RUN apt-get update && apt-get install -y --fix-missing \
    default-mysql-client \
    imagemagick \
    graphviz \
    git \
    libpng-dev \
    libfreetype6-dev \
    libjpeg62-turbo-dev \
    libwebp-dev \
    libmcrypt-dev \
    libxml2-dev \
    libxslt1-dev \
    wget \
    linux-libc-dev \
    libyaml-dev \
    libzip-dev \
    libicu-dev \
    libpq-dev \
    libssl-dev \
    nginx \
    sendmail \
    supervisor && \
    rm -r /var/lib/apt/lists/*

RUN docker-php-ext-configure gd --with-jpeg=/usr/include --with-webp --with-freetype 
RUN docker-php-ext-configure intl
RUN docker-php-ext-install \
    pdo_mysql \
    gd \
    opcache \
    zip \
    intl \
    simplexml

RUN cd /usr/src && \
    curl -sS http://getcomposer.org/installer | php -- --install-dir=/usr/bin --filename=composer

ADD ./server.conf /etc/nginx/nginx.conf
COPY ./php-fpm.conf /usr/local/etc/php-fpm.conf
COPY ./www.conf /usr/local/etc/php-fpm.d/www.conf

RUN chown -R www-data:www-data $HOME/.composer/

#USER www-data
WORKDIR /var/www/html

COPY ./supervisord.conf /etc/supervisord.conf

ENTRYPOINT []
CMD ["/usr/bin/supervisord", "-c", "/etc/supervisord.conf"]
