FROM php:8.2-fpm

# Dependências básicas
RUN apt update && apt install -y \
    autoconf \
    build-essential \
    pkg-config \
    libpq-dev \
    libxml2-dev \
    libzip-dev \
    libpng-dev \
    libjpeg-dev \
    libfreetype6-dev \
    libonig-dev \
    libicu-dev \
    libbz2-dev \
    libcurl4-openssl-dev \
    libxslt1-dev \
    libffi-dev \
    libmcrypt-dev \
    unzip \
    git \
    curl \
    # Extensões PHP
    && docker-php-ext-configure gd --with-freetype --with-jpeg \
    && docker-php-ext-install \
    bcmath \
    bz2 \
    curl \
    gd \
    intl \
    mbstring \
    pdo \
    pgsql \
    pdo_pgsql \
    soap \
    xml \
    xsl \
    zip \
    ffi \
    && pecl install mcrypt \
    && docker-php-ext-enable mcrypt \
    # Limpeza
    && rm -rf /var/lib/apt/lists/*

# PHP sessions
RUN mkdir -p /var/lib/php/8.2/sessions && \
    chown -R www-data:www-data /var/lib/php && \
    chmod 755 /var/lib/php/8.2/sessions

# Configurações
COPY php.ini /usr/local/etc/php/php.ini

# Composer
COPY --from=composer:2 /usr/bin/composer /usr/bin/composer

WORKDIR /var/www/html
