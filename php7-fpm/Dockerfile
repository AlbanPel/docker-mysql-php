# See https://github.com/docker-library/php/blob/master/7.1/fpm/Dockerfile
FROM php:8.1.5-fpm-bullseye
ARG TIMEZONE

MAINTAINER Alban Pelissier

RUN apt-get update && apt-get install -y \
    openssl \
    git \
    unzip

# Install Composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer \
&& composer --version

# Set timezone
RUN ln -snf /usr/share/zoneinfo/${TIMEZONE} /etc/localtime && echo ${TIMEZONE} > /etc/timezone \
&& printf '[PHP]\ndate.timezone = "%s"\n', ${TIMEZONE} > /usr/local/etc/php/conf.d/tzone.ini \
&& "date"

# No Memory limit
RUN cd /usr/local/etc/php/conf.d/ && \
  echo 'memory_limit = -1' >> /usr/local/etc/php/conf.d/docker-php-memlimit.ini

# No Memory limit
RUN cd /usr/local/etc/php/conf.d/ && \
  echo 'max_post_size = 32M' >> /usr/local/etc/php/conf.d/docker-php-postsize.ini

# No Memory limit
RUN cd /usr/local/etc/php/conf.d/ && \
  echo 'upload_max_filesize = 32M' >> /usr/local/etc/php/conf.d/docker-php-uploadsize.ini

# gd extension
RUN apt-get update && apt-get install -y \
  libpng-dev \
  libfreetype6-dev \
  libjpeg-dev \
  libxpm-dev \
  libxml2-dev \
  libxslt-dev \
  librabbitmq-dev \
  libssh-dev \
  libwebp-dev  # php >=7.0 (use libvpx for php <7.0)
RUN docker-php-ext-configure gd \
    --with-freetype-dir=/usr/include/ \
    --with-jpeg-dir=/usr/include/ \
    --with-xpm-dir=/usr/include \
--with-webp-dir=/usr/include/
# php >=7.0 (use libvpx for php <7.0)
RUN docker-php-ext-install gd
RUN docker-php-ext-install zip

# Type docker-php-ext-install to see available extensions
RUN docker-php-ext-install pdo pdo_mysql


# install xdebug
RUN pecl install xdebug-3.1 \
&& docker-php-ext-enable xdebug \
&& echo "error_reporting = E_ALL" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini \
&& echo "display_startup_errors = On" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini \
&& echo "display_errors = On" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini \
&& echo "xdebug.idekey=\"PHPSTORM\"" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini \
&& echo "xdebug.client_port=9001" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini \
&& echo "xdebug.discover_client_host = false" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini \
&& echo "xdebug.mode = debug" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini



RUN echo 'alias sf="php app/console"' >> ~/.bashrc \
&& echo 'alias sf6="php bin/console"' >> ~/.bashrc

WORKDIR /var/www/symfony
