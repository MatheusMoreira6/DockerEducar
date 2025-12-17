# Ambiente Docker – Educar

## Requisitos

-   Docker
-   Docker Compose
-   Porta **80** e **5432** livre no host

## Estrutura do projeto

Os projetos que serão executados no ambiente Docker devem ser clonados dentro da pasta dev, criada na raiz deste repositório.
Essa pasta será montada como volume no container e utilizada como diretório de trabalho do Apache/PHP.

## Parar serviços do sistema

#### Apache (porta 80)

```bash
sudo systemctl stop apache2
```

#### PostgreSQL (porta 5432)

```bash
sudo systemctl stop postgresql
```

## Comandos Docker

### Criar diretório de trabalho (`dev`)

```bash
mkdir dev
```

### Iniciar os containers

```bash
docker compose up -d
```

### Encerrar os containers

```bash
docker compose down
```

## Rodar Composer

```bash
docker exec -it educar-php composer install -d /var/www/html/projeto
```

## Restaurar base

### Criar o banco de dados

```bash
docker exec -it educar-postgres createdb -U postgres cidade_2025-12-16
```

### Restaurar o dump no banco criado

```bash
pg_restore -h localhost -p 5432 -U postgres -d cidade_2025-12-16 -j 15 /caminho/dump_cidade.pgbkp
```

## Configuração do banco de dados

Os projetos executados dentro da pasta `dev` devem utilizar `postgres` como host do banco de dados.

Isso ocorre porque, no Docker Compose, os serviços se comunicam pela rede interna usando o nome do serviço como hostname.

Exemplo de configuração:

```env
DB_HOST = "postgres"
DB_PORT = "5432"
DB_USER = "postgres"
DB_PASS = "postgres"
DB_NAME = "cidade_2025-12-16"
```
