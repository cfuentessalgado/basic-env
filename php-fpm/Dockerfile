FROM php:7.4-fpm


WORKDIR /var/www

RUN apt-get update && apt-get install -y \
    libpq-dev \
    build-essential \
    libpng-dev \
    libjpeg62-turbo-dev \
    libfreetype6-dev \
    locales \
    libzip-dev \
    zip \
    jpegoptim optipng pngquant gifsicle \
    vim \
    unzip \
    git \
    curl \
    libxml2-dev \
    python3 \
    python3-venv \
    python3-pip \
    cups \
    cron \
    cups-bsd \
    iputils-ping

RUN apt-get clean && rm -rf /var/lib/apt/lists/*

RUN docker-php-ext-install pdo_pgsql exif pcntl
#RUN docker-php-ext-configure gd --with-gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ --with-png-dir=/usr/include/
RUN docker-php-ext-install gd zip soap
RUN pear config-set php_ini "$PHP_INI_DIR"
RUN pecl install -o -f redis \
&&  rm -rf /tmp/pear \
&&  docker-php-ext-enable redis

# Install composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

# Add user for laravel application
RUN groupadd -g 1000 www
RUN useradd -u 1000 -ms /bin/bash -g www www

# Change current user to www
USER www
#RUN chmod 0644 /etc/cron.d/cron
#RUN touch /var/log/cron.log
CMD printenv > /etc/environment && echo “cron starting…” && (cron) && : > /var/log/cron.log && tail -f /var/log/cron.log
# Expose port 9000 and start php-fpm server
EXPOSE 9000
CMD ["php-fpm"]
