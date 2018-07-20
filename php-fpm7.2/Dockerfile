FROM php:7.2-fpm

RUN apt-get update \
  && DEBIAN_FRONTEND=noninteractive apt-get install -y --no-install-recommends \
    curl \
    libmemcached-dev \
    libz-dev \
    libpq-dev \
    libjpeg-dev \
    libpng-dev \
    libfreetype6-dev \
    libssl-dev \
    libmcrypt-dev \
    libxml2-dev \
    libxslt1-dev \
    zlib1g-dev \
    libzip-dev \
    zip \
    postfix \
    supervisor \
    rsyslog \
  && rm -rf /var/lib/apt/lists/*

RUN docker-php-ext-install pdo_mysql \
  && docker-php-ext-install pdo_pgsql \
  && docker-php-ext-configure gd \
    --enable-gd-native-ttf \
    --with-jpeg-dir=/usr/lib \
    --with-freetype-dir=/usr/include/freetype2 && \
     docker-php-ext-configure zip --with-libzip && \
#  && docker-php-ext-install zip
     pecl install memcached-3.0.3 && \
     pecl install apcu-5.1.5 && \
     pecl install redis && \
     docker-php-ext-enable memcached && \
     docker-php-ext-enable redis && \
     docker-php-ext-enable apcu && \
     docker-php-ext-install gd \
	exif \
	opcache \
	calendar \
	mysqli \
	pdo \
	bcmath \
  	gettext \
	pcntl \
	xsl \
        zip && \
     docker-php-source delete

RUN set -x \
    && rm /usr/local/etc/php-fpm.d/www.conf.default \
    && rm /usr/local/etc/php-fpm.conf.default \
    && rm /usr/local/etc/php-fpm.d/www.conf \
    && /bin/mkdir /run/php-fpm \
    && rm -r /etc/postfix \
    && groupadd bitrix -g 1003 \
    && useradd -u 1003 --home /home/bitrix --create-home --shell /bin/bash --no-user-group bitrix \
    && usermod -g bitrix bitrix 

WORKDIR /home/bitrix/public-html


COPY ./conf/php.ini /usr/local/etc/php/conf.d/php.ini
COPY ./conf/php-fpm.conf /usr/local/etc/php-fpm.conf
COPY ./conf/bitrix.conf /usr/local/etc/php-fpm.d/bitrix.conf
COPY ./postfix /etc/postfix
COPY ./resources/supervisord.conf /etc/supervisor/supervisord.conf
COPY ./resources/rsyslog.conf /etc/rsyslog.conf
COPY ./resources/listen.conf /etc/rsyslog.d/listen.conf
COPY ./conf/aliases /etc/aliases

EXPOSE 9201 25 3306

CMD /usr/bin/supervisord -c /etc/supervisor/supervisord.conf
