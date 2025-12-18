FROM httpd:2.4

# Carrega módulos
COPY conf/modules.conf /usr/local/apache2/conf/extra/modules.conf

RUN grep -q "^IncludeOptional conf/extra/modules.conf" /usr/local/apache2/conf/httpd.conf || \
    echo "IncludeOptional conf/extra/modules.conf" >> /usr/local/apache2/conf/httpd.conf

# Configura diretórios
COPY conf/dir.conf /usr/local/apache2/conf/extra/dir.conf

RUN grep -q "^IncludeOptional conf/extra/dir.conf" /usr/local/apache2/conf/httpd.conf || \
    echo "IncludeOptional conf/extra/dir.conf" >> /usr/local/apache2/conf/httpd.conf

# Configura vhost
COPY conf/vhost.conf /usr/local/apache2/conf/extra/vhost.conf

RUN grep -q "^IncludeOptional conf/extra/vhost.conf" /usr/local/apache2/conf/httpd.conf || \
    echo "IncludeOptional conf/extra/vhost.conf" >> /usr/local/apache2/conf/httpd.conf

# Configura charset
COPY conf/charset.conf /usr/local/apache2/conf/extra/charset.conf

RUN grep -q "^IncludeOptional conf/extra/charset.conf" /usr/local/apache2/conf/httpd.conf || \
    echo "IncludeOptional conf/extra/charset.conf" >> /usr/local/apache2/conf/httpd.conf