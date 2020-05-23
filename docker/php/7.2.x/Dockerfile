ARG PHP_VERSION=7.2
FROM daocloud.io/library/php:${PHP_VERSION}-fpm



RUN sed -i "s/archive.ubuntu./mirrors.aliyun./g" /etc/apt/sources.list 
RUN sed -i "s/deb.debian.org/mirrors.aliyun.com/g" /etc/apt/sources.list 

RUN apt-get clean && apt-get update && apt-get install -y \
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
    && docker-php-ext-install opcache \
    && docker-php-ext-install pcntl \
    && docker-php-ext-install bcmath \
    && docker-php-ext-install posix \
    && docker-php-ext-install sysvsem \
    && docker-php-ext-install sysvshm \
    && docker-php-ext-install sysvmsg 

# 压缩 tar -czvf ***.tar.gz
# 解压 tar -xzvf ***.tar.gz

# RUN mkdir -p /tmp/extension

RUN mkdir -p /tmp/extension 
ADD extension /tmp/extension

ENV AMQP_VERSION=amqp-1.9.3
RUN mkdir -p amqp \
    && tar -xzvf /tmp/extension/${AMQP_VERSION}.tar.gz -C amqp --strip-components=1 \
    && ( \
    cd amqp \
    && phpize \
    && ./configure --enable-amqp \
    && make -j$(nproc) \
    && make install \
    ) \
    && rm -r amqp \
    && docker-php-ext-enable amqp 

# ENV HIREDIS_VERSION=hiredis-master
# RUN mkdir -p hiredis \
#     && tar -xzvf /tmp/extension/${HIREDIS_VERSION}.tar.gz -C hiredis --strip-components=1 \
#     && ( \
#     cd hiredis \
#     && make -j$(nproc) \
#     && make install \
#     ) \
#     && rm -r hiredis \
#     && ldconfig

ENV SWOOLE_VERSION=swoole-4.4.5
RUN mkdir -p swoole \
    && tar -xzvf /tmp/extension/${SWOOLE_VERSION}.tar.gz -C swoole --strip-components=1 \
    && ( \
    cd swoole \
    && phpize \
    && ./configure --enable-swoole \
    && make -j$(nproc) \
    && make install \
    ) \
    && rm -r swoole \
    && docker-php-ext-enable swoole

ENV REDIS_VERSION=redis-3.1.6
RUN mkdir -p redis \
    && tar -xzvf /tmp/extension/${REDIS_VERSION}.tar.gz -C redis --strip-components=1 \
    && ( \
    cd redis \
    && phpize \
    && ./configure --enable-redis \
    && make -j$(nproc) \
    && make install \
    ) \
    && rm -r redis \
    && docker-php-ext-enable redis

ENV MCRYPT_VERSION=mcrypt-1.0.3
RUN mkdir -p mcrypt \
    && tar -xzvf /tmp/extension/${MCRYPT_VERSION}.tar.gz -C mcrypt --strip-components=1 \
    && ( \
    cd mcrypt \
    && phpize \
    && ./configure --enable-mcrypt \
    && make -j$(nproc) \
    && make install \
    ) \
    && rm -r mcrypt \
    && docker-php-ext-enable mcrypt

# ENV PHP_PSR_VERSION=php-psr-0.7.0
# # phalcon 4依赖psr扩展
# RUN mkdir -p phppsr \
#     && tar -xzvf /tmp/extension/${PHP_PSR_VERSION}.tar.gz -C phppsr --strip-components=1 \
#     && ( \
#     cd phppsr \
#     && phpize \
#     && ./configure \
#     && make -j$(nproc) \
#     && make install \
#     ) \
#     && rm -r phppsr \
#     && echo "extension=psr.so" > "$PHP_INI_DIR/conf.d/psr.ini" 


ENV PHALCON_VERSION=phalcon-v3.2.4
RUN mkdir -p phalcon \
    && tar -xzvf /tmp/extension/${PHALCON_VERSION}.tar.gz -C phalcon --strip-components=1 \
    && cd phalcon/build \
    && ./install \
    && cd ../../ \
    && rm -r phalcon \
    && echo "extension=phalcon.so" > "$PHP_INI_DIR/conf.d/phalcon.ini" 




ENV MONGODB_VERSION=mongodb-1.6.0
RUN mkdir -p mongodb \
    && tar -xzvf /tmp/extension/${MONGODB_VERSION}.tar.gz -C mongodb --strip-components=1 \
    && ( \
    cd mongodb \
    && phpize \
    && ./configure  --enable-mongodb \
    && make all \
    && make install \
    ) \
    && rm -r mongodb \
    && echo "extension=mongodb.so" > "$PHP_INI_DIR/conf.d/mongodb.ini"



#composer安装
RUN echo "memory_limit=-1" > "$PHP_INI_DIR/conf.d/memory-limit.ini" \
    && echo "date.timezone=${PHP_TIMEZONE:-UTC}" > "$PHP_INI_DIR/conf.d/date_timezone.ini"
ENV COMPOSER_ALLOW_SUPERUSER 1
ENV COMPOSER_HOME /tmp
ENV COMPOSER_VERSION 1.6.5

RUN cp /tmp/extension/composer.phar /usr/bin/composer \
    && chmod u+x /usr/bin/composer


ENV XHPROF_VERSION=xhprof-4.x
RUN mkdir -p xhprof \
    && tar -xzvf /tmp/extension/${XHPROF_VERSION}.tar.gz -C xhprof --strip-components=1 \
    && ( \
    cd xhprof \
    && phpize \
    && ./configure \
    && make -j$(nproc) \
    && make install \
    ) \
    && rm -r xhprof \
    && echo "extension=tideways.so" > "$PHP_INI_DIR/conf.d/tideways.ini" \
    && echo "tideways.auto_prepend_library=0" >> "$PHP_INI_DIR/conf.d/tideways.ini" 





RUN rm -r /tmp/extension && php -m




ADD gitconfig /etc/
ADD php.ini /usr/local/etc/php/


# WORKDIR /var/www/html/queue.yunkezan.com

# CMD ["php", "./easyswoole", "start"]
