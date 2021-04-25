# PHP Docker image for Yii 2.0 Framework runtime
# ==============================================

FROM php:7.3-fpm

# Install system packages for PHP extensions recommended for Yii 2.0 Framework
ENV DEBIAN_FRONTEND=noninteractive
RUN apt-get update && \
    apt-get -y install \
        gnupg2 && \
    apt-key update && \
    apt-get update && \
    apt-get -y install \
            g++ \
            git \
            curl \
            imagemagick \
            libcurl3-dev \
            libicu-dev \
            libfreetype6-dev \
            libjpeg-dev \
            libjpeg62-turbo-dev \
            libmagickwand-dev \
            libpq-dev \
            libpng-dev \
            libxml2-dev \
            libzip-dev \
            zlib1g-dev \
            default-mysql-client \
            openssh-client \
            unzip \
            libcurl4-openssl-dev \
            libssl-dev \
            vim \
            gettext \
            net-tools \
            openssh-server \
            libmcrypt-dev \
        --no-install-recommends && \
        apt-get clean && \
        rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

# Install PHP extensions required for Yii 2.0 Framework
RUN docker-php-ext-configure gd \
        --with-freetype-dir=/usr/include/ \
        --with-png-dir=/usr/include/ \
        --with-jpeg-dir=/usr/include/ && \
    docker-php-ext-configure bcmath && \
    docker-php-ext-install \
        soap \
        zip \
        curl \
        bcmath \
        exif \
        gd \
        iconv \
        intl \
        mbstring \
        opcache \
        pdo_mysql \
        pdo_pgsql \
        pcntl \
        json \
        xml
    

# Install PECL extensions
# see http://stackoverflow.com/a/8154466/291573) for usage of `printf`
RUN printf "\n" | pecl install \
        imagick \
        redis && \
    docker-php-ext-enable \
        imagick \
        redis

RUN pecl install mcrypt-1.0.2 \
    && docker-php-ext-enable mcrypt;

# Check if Xdebug extension need to be compiled
RUN cd /tmp && \
    git clone git://github.com/xdebug/xdebug.git && \
    cd xdebug && \
    git checkout 2.9.4 && \
    phpize && \
    ./configure --enable-xdebug && \
    make && \
    make install

RUN rm -rf /tmp/xdebug
RUN mkdir -p /tmp/xdebug
RUN chmod -R 2777 /tmp/xdebug

# Environment settings
ENV PHP_USER_ID=33 \
    PHP_ENABLE_XDEBUG=0 \
    PATH=/app:/app/vendor/bin:/root/.composer/vendor/bin:$PATH \
    TERM=linux \
    VERSION_PRESTISSIMO_PLUGIN=^0.3.10 \
    COMPOSER_ALLOW_SUPERUSER=1

# Add configuration files
COPY image-files/ /

RUN chmod 700 \
        /usr/local/bin/docker-php-entrypoint \
        /usr/local/bin/composer

# Install composer
RUN curl -sS https://getcomposer.org/installer | php -- \
        --filename=composer.phar \
        --install-dir=/usr/local/bin && \
    composer clear-cache

# Install Yii framework bash autocompletion
RUN curl -L https://raw.githubusercontent.com/yiisoft/yii2/master/contrib/completion/bash/yii \
        -o /etc/bash_completion.d/yii

WORKDIR /var/www
