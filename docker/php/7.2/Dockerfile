FROM daocloud.io/library/php:7.2-fpm


RUN sed -i "s/archive.ubuntu./mirrors.aliyun./g" /etc/apt/sources.list 
RUN sed -i "s/deb.debian.org/mirrors.aliyun.com/g" /etc/apt/sources.list 

RUN apt-get update && apt-get install -y \
    libpng-dev \
    libpq-dev \
    g++ \
    libicu-dev \
    libxml2-dev \
    htop \
    git \
    vim \
    libfreetype6-dev \
    libjpeg62-turbo-dev \
    libmcrypt-dev \
    zlib1g-dev \
    librabbitmq-dev

RUN docker-php-ext-configure intl \
    && docker-php-ext-install intl \
    && docker-php-ext-install mbstring \
    && docker-php-ext-install zip \
    && docker-php-ext-install pdo_mysql \
    && docker-php-ext-install pdo_pgsql \
    && docker-php-ext-install mysqli \
    && docker-php-ext-install soap \
    && docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
    && docker-php-ext-install -j$(nproc) gd \
    && docker-php-ext-install opcache 

RUN curl -fsSL 'http://pecl.php.net/get/amqp-1.9.3.tgz' -o amqp.tar.gz \
    && mkdir -p amqp \
    && tar -xf amqp.tar.gz -C amqp --strip-components=1 \
    && rm amqp.tar.gz \
    && ( \
    cd amqp \
    && phpize \
    && ./configure --enable-amqp \
    && make -j$(nproc) \
    && make install \
    ) \
    && rm -r amqp \
    && docker-php-ext-enable amqp \
    && curl -fsSL 'http://pecl.php.net/get/swoole-4.2.12.tgz' -o swoole.tar.gz \
    && mkdir -p swoole \
    && tar -xf swoole.tar.gz -C swoole --strip-components=1 \
    && rm swoole.tar.gz \
    && ( \
    cd swoole \
    && phpize \
    && ./configure --enable-swoole \
    && make -j$(nproc) \
    && make install \
    ) \
    && rm -r swoole \
    && docker-php-ext-enable swoole \
    && curl -fsSL 'http://pecl.php.net/get/redis-3.1.6.tgz' -o redis.tar.gz \
    && mkdir -p redis \
    && tar -xf redis.tar.gz -C redis --strip-components=1 \
    && rm redis.tar.gz \
    && ( \
    cd redis \
    && phpize \
    && ./configure --enable-redis \
    && make -j$(nproc) \
    && make install \
    ) \
    && rm -r redis \
    && docker-php-ext-enable redis 

