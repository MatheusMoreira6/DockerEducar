FROM httpd:2.4

# Configurações globais
COPY conf/httpd.conf /usr/local/apache2/conf/extra/httpd.conf

RUN grep -q "^IncludeOptional conf/extra/httpd.conf" /usr/local/apache2/conf/httpd.conf || \
    echo "IncludeOptional conf/extra/httpd.conf" >> /usr/local/apache2/conf/httpd.conf

# Carrega módulos
COPY conf/modules.conf /usr/local/apache2/conf/extra/modules.conf

RUN grep -q "^IncludeOptional conf/extra/modules.conf" /usr/local/apache2/conf/httpd.conf || \
    echo "IncludeOptional conf/extra/modules.conf" >> /usr/local/apache2/conf/httpd.conf

# Configura vhost
COPY conf/vhost.conf /usr/local/apache2/conf/extra/vhost.conf

RUN grep -q "^IncludeOptional conf/extra/vhost.conf" /usr/local/apache2/conf/httpd.conf || \
    echo "IncludeOptional conf/extra/vhost.conf" >> /usr/local/apache2/conf/httpd.conf
