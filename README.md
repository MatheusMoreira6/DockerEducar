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

## Parar serviços do sistema (`Linux`)

#### Apache (porta 80)

```bash
# Para parar temporariamente o Apache
sudo systemctl stop apache2

# Para desabilitar completamente a inicialização automática do Apache
sudo systemctl disable apache2
```

#### PostgreSQL (porta 5432)

```bash
# Para parar temporariamente o PostgreSQL
sudo systemctl stop postgresql

# Para desabilitar completamente a inicialização automática do PostgreSQL
sudo systemctl disable postgresql
```

## Instalação do Projeto via Docker

### Clonar o repositório

```bash
git clone git@gitlab.com:equipe-de-desenvolvimento/docker-educar.git
cd docker-educar
```

### Criar diretórios de projetos (`dev`) e arquivos (`storage`)

```bash
mkdir dev storage
```

### Configurar variáveis de ambiente

```bash
cp .env.example .env
```

```bash
# Verifique os IDs do seu usuário e do grupo www-data:
id -u
getent group www-data | cut -d: -f3
```

> Valores comuns: UID=1000 e GID=33. Se forem diferentes no seu sistema, ajuste no .env.

### Clonar os projetos no diretório (`dev`)

```bash
cd dev
git clone URL_DO_REPOSITORIO
```

### Permissões de grupo (`Apache` + `herança`)

> Execute este comando **somente após** os projetos já estarem na pasta `dev` e configurados (ex.: branch correta), para evitar problemas de permissão entre usuário e Apache.

```bash
sudo chgrp -R www-data dev storage
sudo chmod -R 2770 dev storage
```

## Comandos Docker

```bash
# Iniciar os containers
docker compose up -d

# Encerrar os containers
docker compose down
```

## Executar Comandos (`Composer` | `Scripts`)

### Composer

```bash
docker exec -it educar-php composer install -d ./projeto
```

### Scripts PHP

```bash
docker exec -it educar-php php ./projeto/script.php
```

> **Observação:** no container, o caminho `/var/www/html` é um volume espelhado com a pasta `dev` do projeto no host.  
> Portanto, substitua `projeto` pelo nome da pasta que está dentro de `dev` onde você deseja executar o Composer.  
> Exemplo: se o projeto estiver em `dev/meu-projeto`, utilize `-d ./meu-projeto`.

## Restaurar base

### Criar o banco de dados

```bash
docker exec -it educar-postgres createdb -U postgres cidade_2026-01-01
```

### Restaurar o dump no banco criado

```bash
pg_restore -h localhost -p 5432 -U postgres -d cidade_2026-01-01 -j 5 /caminho/dump_cidade.pgbkp
```

> **Observação:** o caminho `/caminho/dump_cidade.pgbkp` deve ser o caminho do arquivo na sua máquina. Ex: Se o dump estiver em Downloads utilize: `~/Downloads/dump_cidade.pgbkp`.

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

## Limpeza de espaço no Docker (PostgreSQL + Volumes)

### Linux e Windows

```bash
docker-compose down -v
```

> **Observação:** esse comando remove os containers, redes e volumes definidos no docker-compose.yml do projeto, apagando os dados persistidos (ex: banco de dados).

### Somente no Windows

#### Parar o WSL

```powershell
wsl --shutdown
```

#### Compactar disco do Docker (VHDX)

```powershell
Optimize-VHD -Path "C:\Users\SEU_USER\AppData\Local\Docker\wsl\data\ext4.vhdx" -Mode Full
```
