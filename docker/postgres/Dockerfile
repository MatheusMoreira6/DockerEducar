# Imagem oficial do PostgreSQL 15
FROM postgres:17

# Define locale padrão do sistema como pt_BR em ISO-8859-1
ENV LANG=pt_BR.ISO8859-1
ENV LC_ALL=pt_BR.ISO8859-1

# Configura o initdb para criar o cluster em LATIN1 com locale pt_BR.ISO8859-1
ENV POSTGRES_INITDB_ARGS="--encoding=LATIN1 --locale=pt_BR.ISO8859-1"

# Gera o locale pt_BR.ISO8859-1 no sistema do container
RUN localedef -i pt_BR -c -f ISO-8859-1 pt_BR.ISO8859-1

# Adiciona scripts que rodam no primeiro start do PostgreSQL
COPY initdb /docker-entrypoint-initdb.d
