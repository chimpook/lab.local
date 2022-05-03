ARG PHP_VERSION

FROM php:${PHP_VERSION}-apache

ENV TZ=Europe/Warsaw

RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && \
	echo $TZ > /etc/timezone && \
	echo "date.timezone = $TZ" > $PHP_INI_DIR/conf.d/timezone.ini

RUN apt-get update \
	&& apt-get install -y \
	apt-utils \
	libicu-dev \
	zlib1g-dev libpng-dev libjpeg-dev libfreetype6-dev \
	libpq-dev libxslt-dev \
	libzip-dev git wget \
	librabbitmq-dev \
	--no-install-recommends


RUN pecl install -o -f xdebug && \
	rm -rf /tmp/pear
RUN docker-php-ext-enable xdebug
RUN docker-php-ext-configure pgsql --with-pgsql=/usr/local/pgsql
RUN docker-php-ext-install -j$(nproc) pdo mysqli pdo_mysql zip gd pdo_pgsql xsl intl
RUN pecl install redis amqp
RUN docker-php-ext-enable redis amqp

COPY xdebug.ini /usr/local/etc/php/conf.d/xdebug.ini
COPY apache.conf /etc/apache2/sites-enabled/000-default.conf

WORKDIR /var/www

RUN a2enmod proxy && \ 
	a2enmod proxy_http && \
	a2enmod rewrite

#RUN echo 'deb [trusted=yes] https://repo.symfony.com/apt/ /' | tee /etc/apt/sources.list.d/symfony-cli.list
#RUN apt-get update
#RUN apt-get install symfony-cli

#RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

#RUN curl -fsSL https://deb.nodesource.com/setup_18.x | bash -
#RUN apt-get update
#RUN apt-get install -y nodejs

#RUN npm install -g yarn

#RUN git config --global user.email "chimpook@gmail.com"
#RUN git config --global user.name "Chimpook Virtuch"

CMD ["apache2-foreground"]

