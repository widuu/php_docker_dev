FROM php:8.2.11-fpm

COPY ./etc/swoole.ini /usr/local/etc/php/conf.d/swoole.ini
# 更改为阿里云镜像
RUN sed -i 's/deb.debian.org/mirrors.aliyun.com/g' /etc/apt/sources.list.d/debian.sources \
    && apt-get update && apt-get install -y \
    openssl \
    libssl-dev \
    libfreetype6-dev \
    libpng-dev \
    libjpeg-dev \
    libzip-dev \
    libonig-dev \ 
    zip \
    wget

# 需要openssl扩展取消以下注释-begin
RUN rm -rf /var/lib/apt/lists/* \
    && wget https://www.openssl.org/source/openssl-1.1.1f.tar.gz \
    && tar -xzvf ./openssl-1.1.1f.tar.gz \
    && cd openssl-1.1.1f \
    && ./config --prefix=/usr/local/ssl --openssldir=/usr/local/ssl \
    && make \
    && make install \
    && rm -rf /var/www/openssl-1.1.1f.tar.gz \
    && rm -rf /var/www/openssl-1.1.1f \
    #php7.0-7.3 gd库安装
    # && docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-png-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
    # && docker-php-ext-install gd \
    #php7.4 gd库安装
    && docker-php-ext-configure gd --with-freetype --with-jpeg \
    && docker-php-ext-install gd \
    && docker-php-ext-install mysqli \
    && docker-php-ext-install pdo_mysql \
    && docker-php-ext-install bcmath \
    && docker-php-ext-install mbstring \
    && pecl install protobuf \
    && docker-php-ext-enable protobuf

RUN pecl install redis \
    && docker-php-ext-enable redis \
    && pecl install zip \
    && docker-php-ext-enable zip \
    && pecl install mongodb \
    && docker-php-ext-enable mongodb

RUN mkdir -p ~/build \
    && cd ~/build \
    && rm -rf ./swoole-src \
    && wget https://github.com/swoole/swoole-src/archive/master.tar.gz -O swoole.tar.gz \
    && mkdir -p swoole \
    && tar -xzvf swoole.tar.gz -C swoole --strip-components=1 \
    && rm swoole.tar.gz \
    &&  cd swoole \
    && phpize \
    && ./configure --with-openssl-dir=/usr/local/ssl --enable-mysqlnd --enable-openssl --enable-http2 \
    && make -j$(nproc) \
    && make install \
    && docker-php-ext-enable swoole \
    && cd ~ \
    && rm -rf ~/build

RUN php -r "copy('https://install.phpcomposer.com/installer', 'composer-setup.php');" \
    && php composer-setup.php \
    && rm -rf composer-setup.php \
    && mv composer.phar /usr/bin/composer
