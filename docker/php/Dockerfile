FROM php:8.1.8-apache

# ENV CFLAGS="$CFLAGS -D_GNU_SOURCE"

RUN apt-get update \
    && apt-get install -y \
        curl \
        zip \
        unzip \
        git \
        supervisor \
        libpng-dev \
        libpq-dev \
        libzip-dev \
        unixodbc-dev \
    && docker-php-ext-install \
        bcmath \
        pdo_pgsql \
        gd \
        opcache \
        sockets \
        zip \
        mysqli \
        pdo \
        pdo_mysql \
    && apt-get -y autoremove \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

RUN pecl install sqlsrv pdo_sqlsrv
RUN docker-php-ext-enable sqlsrv pdo_sqlsrv

RUN apt-get update \
    # && apt-get -y install gnupg \
    # && sh -c 'echo "deb http://apt.postgresql.org/pub/repos/apt buster-pgdg main" > /etc/apt/sources.list.d/pgdg.list' \
    # && curl -fsSL https://www.postgresql.org/media/keys/ACCC4CF8.asc | apt-key add - \
    && apt-get update \
    && apt-get -y install postgresql-client-13 \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

RUN apt-get update && apt-get install -y tzdata && apt-get clean && rm -rf /var/lib/apt/lists/*
ENV TZ Asia/Ho_Chi_Minh

ENV ACCEPT_EULA=Y
RUN apt-get update && apt-get install -y gnupg2

# Install prerequisites for the sqlsrv and pdo_sqlsrv PHP extensions.
RUN curl https://packages.microsoft.com/keys/microsoft.asc | apt-key add - \
    && curl https://packages.microsoft.com/config/debian/11/prod.list > /etc/apt/sources.list.d/mssql-release.list \
    && apt-get update \
    && apt-get install -y msodbcsql17 mssql-tools unixodbc-dev \
    && rm -rf /var/lib/apt/lists/*

ENV APACHE_DOCUMENT_ROOT /var/www/html/public

RUN sed -ri -e 's!/var/www/html!${APACHE_DOCUMENT_ROOT}!g' /etc/apache2/sites-available/*.conf
#RUN sed -ri -e 's!/var/www/!${APACHE_DOCUMENT_ROOT}!g' /etc/apache2/apache2.conf /etc/apache2/conf-available/*.conf


# Install Composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer
# Install xDebug
RUN pecl install xdebug-3.1.5
RUN docker-php-ext-enable xdebug
RUN echo "xdebug.mode=debug" >> /usr/local/etc/php/php.ini

RUN a2enmod rewrite

RUN useradd -ms /bin/bash -u 1000 app

COPY start /usr/local/bin/
RUN ["chmod", "+x", "/usr/local/bin/start"]
CMD [ "start" ]