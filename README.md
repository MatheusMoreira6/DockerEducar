# Ambiente Docker – Educar

## Requisitos

-   Docker
-   Docker Compose
-   Porta **80** livre no host

## Estrutura do projeto

Os projetos que serão executados no ambiente Docker devem ser clonados dentro da pasta dev, localizada na raiz deste repositório.
Essa pasta será montada como volume no container e utilizada como diretório de trabalho do Apache/PHP.

## Comandos

### Build da imagem

```bash
docker compose build --no-cache
```

### Subir os containers
```bash
docker compose up -d
```

### Parar os containers
```bash
docker compose down
```
