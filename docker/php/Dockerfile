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

# Configurações PHP
COPY php.ini /usr/local/etc/php/php.ini

# Configurações de locale
RUN apt-get update && apt-get install -y locales \
    && sed -i -e '/END LC_COLLATE/i reorder-after <U00A0>\n<U0020><CAP>;<CAP>;<CAP>;<U0020>\nreorder-end' /usr/share/i18n/locales/pt_BR \
    && localedef -i pt_BR -c -f ISO-8859-1 pt_BR \
    && sed -i 's/# pt_BR.ISO-8859-1/pt_BR.ISO-8859-1/' /etc/locale.gen \
    && sed -i 's/# pt_BR.UTF-8/pt_BR.UTF-8/' /etc/locale.gen \
    && locale-gen

ENV LANG=pt_BR.ISO-8859-1
ENV LANGUAGE=pt_BR:pt
ENV LC_ALL=pt_BR.ISO-8859-1

# Composer
COPY --from=composer:2 /usr/bin/composer /usr/bin/composer

WORKDIR /var/www/html
