# Ambiente Docker – Educar

## Requisitos

- Docker
- Docker Compose
- Porta **80** e **5432** livre no host
- PostgreSQL Client (`pg_restore`, `psql`, `pg_dump`)
    - Ubuntu: `sudo apt install postgresql-client`
    - Fedora: `sudo dnf install postgresql`
    - Arch Linux: `sudo pacman -S postgresql-libs`

## Compatibilidade com Windows

O projeto Educar **não funciona diretamente no Windows**, devido à existência de pastas e arquivos com nomes iguais, diferenciados apenas por maiúsculas/minúsculas (ex.: `teste` e `Teste`).
O Windows não distingue essa diferença, o que causa conflitos no Git.

**No Windows, o uso deve ser feito obrigatoriamente via WSL (Windows Subsystem for Linux).**

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

### Criar diretório de projetos (`dev`)

```bash
mkdir dev
```

### Criar diretório de arquivos (`storage`)

```bash
mkdir storage
sudo chown -R seu_usuario:www-data storage
chmod 775 storage
```

### Iniciar os containers

```bash
docker compose up -d
```

### Encerrar os containers

```bash
docker compose down
```

## Executar Comandos

### Composer

```bash
docker exec -it educar-php composer install -d /var/www/html/projeto
```

### Scripts PHP

```bash
docker exec -it educar-php php /var/www/html/projeto/script.php
```

> **Observação:** no container, o caminho `/var/www/html` é um volume espelhado com a pasta `dev` do projeto no host.  
> Portanto, substitua `projeto` pelo nome da pasta que está dentro de `dev` onde você deseja executar o Composer.  
> Exemplo: se o projeto estiver em `dev/meu-projeto`, utilize `-d /var/www/html/meu-projeto`.

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

## Permissões de Pasta (Usuário + Apache)

```bash
sudo chown -R seu_usuario:www-data dev/
```

> Execute este comando **somente após** os projetos já estarem na pasta `dev` e configurados (ex.: branch correta), para evitar problemas de permissão entre usuário e Apache.
