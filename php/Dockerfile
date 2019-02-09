FROM php:7.2-fpm
 
#~~~ INSTALL MYSQLI ~~~#
RUN docker-php-ext-install mysqli
#~~~~~~~~~~~~#

#~~~ SET TIMEZONE ~~~#
RUN ln -snf /usr/share/zoneinfo/${TIMEZONE} /etc/localtime && echo ${TIMEZONE} > /etc/timezone \
&& printf '[PHP]\ndate.timezone = "%s"\n', ${TIMEZONE} > /usr/local/etc/php/conf.d/tzone.ini \
&& "date"
#~~~~~~~~~~~~#

#~~~ Add PHP repository to apt source ~~~#
RUN apt-get update && apt-get install && apt-get install apt-utils git vim curl libzip-dev gnupg2 zip iproute2 -y
RUN docker-php-ext-configure zip --with-libzip
RUN docker-php-ext-install mysqli zip pdo_mysql
#~~~~~~~~~~~~#

#~~~ Node ~~~#
RUN curl -sL https://deb.nodesource.com/setup_11.x | bash -
RUN apt-get install -y nodejs
#~~~~~~~~~~~~#

#~~~ YARN ~~~#
RUN curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | apt-key add -
RUN echo "deb https://dl.yarnpkg.com/debian/ stable main" | tee /etc/apt/sources.list.d/yarn.list
RUN apt-get update && apt-get install yarn -y
#~~~~~~~~~~~~#

#~~~ INSTALL AND ENALBE XDEBUG ~~~#
RUN pecl install xdebug \
	&& docker-php-ext-enable xdebug \
	&& echo "error_reporting = E_ALL" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini \
	&& echo "display_startup_errors = On" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini \
	&& echo "display_errors = On" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini \
	&& echo "xdebug.remote_enable=1" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini \
	&& echo "xdebug.remote_connect_back=1" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini \
	&& echo "xdebug.idekey=\"PHPSTORM\"" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini \
	&& echo "xdebug.remote_port=9001" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini
#~~~~~~~~~~~~#


#~~~ INSTALL COMPOSER ~~~#
RUN curl -o /tmp/composer-setup.php https://getcomposer.org/installer \
&& curl -o /tmp/composer-setup.sig https://composer.github.io/installer.sig \
# Make sure we're installing what we think we're installing!
&& php -r "if (hash('SHA384', file_get_contents('/tmp/composer-setup.php')) !== trim(file_get_contents('/tmp/composer-setup.sig'))) { unlink('/tmp/composer-setup.php'); echo 'Invalid installer' . PHP_EOL; exit(1); }" \
&& php /tmp/composer-setup.php --no-ansi --install-dir=/usr/local/bin --filename=composer --snapshot \
&& rm -f /tmp/composer-setup.*
#~~~~~~~~~~~~#

#~~~ SETUP WORKDIR ~~~#
WORKDIR /var/www/symfony
