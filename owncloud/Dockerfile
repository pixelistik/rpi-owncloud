FROM sdhibit/rpi-raspbian

RUN apt-get update && apt-get install -y \
	curl \
	apache2 \
	libapache2-mod-php5 \
	php5-gd \
	php5-json \
	php5-curl \
	php5-intl \
	php5-mcrypt \
	php5-imagick \
	php5-sqlite \
	bzip2 \
	libcurl4-openssl-dev \
	libfreetype6-dev \
	libicu-dev \
	libjpeg-dev \
	libmcrypt-dev \
	libpng12-dev \
	libpq-dev \
	libxml2-dev \
	&& rm -rf /var/lib/apt/lists/*

#gpg key from https://owncloud.org/owncloud.asc
RUN gpg --keyserver ha.pool.sks-keyservers.net --recv-keys E3036906AD9F30807351FAC32D5D5E97F6978A26

RUN mkdir -p /etc/apache2/logs

ENV OWNCLOUD_VERSION=8.1.1 \
    APACHE_RUN_USER=www-data \
    APACHE_RUN_GROUP=www-data \
    APACHE_LOG_DIR=logs \
    APACHE_LOCK_DIR=/var/lock/apache2 \
    APACHE_PID_FILE=/var/run/apache2/apache

RUN curl -fsSL -o owncloud.tar.bz2 \
		"https://download.owncloud.org/community/owncloud-${OWNCLOUD_VERSION}.tar.bz2" \
	&& curl -fsSL -o owncloud.tar.bz2.asc \
		"https://download.owncloud.org/community/owncloud-${OWNCLOUD_VERSION}.tar.bz2.asc" \
	&& gpg --verify owncloud.tar.bz2.asc \
		&& tar -xjf owncloud.tar.bz2 -C /var/ \
		&& mv /var/owncloud /var/www \
		&& rm owncloud.tar.bz2 owncloud.tar.bz2.asc

COPY config.php /var/www/owncloud/config/config.php

RUN chown -R www-data:www-data /var/www

COPY owncloud.conf /etc/apache2/sites-available/owncloud.conf
RUN a2dissite 000-default \
	&& a2ensite owncloud

RUN a2enmod rewrite \
	&& a2enmod headers

ENTRYPOINT ["apache2","-DFOREGROUND"]
