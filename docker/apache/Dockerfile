FROM httpd:2.4

# Carrega módulos
COPY modules.conf /usr/local/apache2/conf/extra/modules.conf

RUN grep -q "^IncludeOptional conf/extra/modules.conf" /usr/local/apache2/conf/httpd.conf || \
    echo "IncludeOptional conf/extra/modules.conf" >> /usr/local/apache2/conf/httpd.conf

# Configura vhost
COPY vhost.conf /usr/local/apache2/conf/extra/vhost.conf

RUN grep -q "^IncludeOptional conf/extra/vhost.conf" /usr/local/apache2/conf/httpd.conf || \
    echo "IncludeOptional conf/extra/vhost.conf" >> /usr/local/apache2/conf/httpd.conf
