# TOTVS Protheus Docker

O objetivo desse repositório é ser um ponto de partida para criação de ambientes de
desenvolvimento Protheus utilizando nossas [imagens docker](https://hub.docker.com/u/soulsys) das soluções TOTVS.

## Importante

- Esse repositório não possui qualquer relação com a TOTVS S/A
- As imagens **NÃO** devem ser usadas em ambientes de produção
- Utilize apenas como ambiente de desenvolvimento
- Ao utilizar você concorda com os termos da licença MIT

## Pré-requisitos

### Windows

- Habilitar o [WSL2](https://www.omgubuntu.co.uk/how-to-install-wsl2-on-windows-10)
- Instalar o [Docker Desktop](https://docs.docker.com/desktop/windows/install) ou configurar manualmente o docker na distribuição linux do WSL2

### Linux

- Instalar e configurar as últimas versões do docker e docker compose

### Mac

- Instalar o [OrbStack](https://orbstack.dev/)

## Mémoria

- Os containers precisam de pelo ao menos 4 GB de RAM para execução sem erros
- Se estiver usando o Docker Desktop, habilite a memória na área de recursos

## Softwares recomendados

- [VS Code](https://code.visualstudio.com/download)
  (instalar plugins [TOTVS Developer Studio](https://marketplace.visualstudio.com/items?itemName=totvs.tds-vscode) e
  [Docker](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-docker))
- [Node.js](https://nodejs.org/en/download/)
- [Azure Data Studio](https://docs.microsoft.com/pt-br/sql/azure-data-studio/download-azure-data-studio?view=sql-server-ver15)

## Usando pela primeira vez

1. Crie um arquivo `.env` na raiz do repositório e defina as [variáveis de ambiente](https://hub.docker.com/u/soulsys) conforme abaixo:

````bash
# SQL Server
SA_PASSWORD=Root@mssqlpsw
ACCEPT_EULA=Y
MSSQL_CONTAINER_NAME=mssql
MSSQL_VOLUME=./docker/mssql
MSSQL_PORT=1433

# License Server
LICENSE_CONTAINER_NAME=license
LICENSE_=license
LICENSE_WEBAPP_PORT=8020

# DBAccess
DBACCESS_CONTAINER_NAME=dbaccess
DBACCESS_DB_ALIAS=protheus_db

# Protheus
PROTHEUS_ENVIRONMENT=environment
PROTHEUS_CONTAINER_NAME=protheus
PROTHEUS_VOLUME=./docker/protheus
PROTHEUS_TCP_PORT=2034
PROTHEUS_WEBAPP_PORT=8097
PROTHEUS_REST_PORT=8098
````

2. Criação de uma rede para os containers

```bash
docker network create sample-docker-network
```

3. Download das imagens e criação dos containers

```bash
docker-compose up --no-recreate
```

4. Acesse a instância do SQL Server. Recomendamos utilizar o [Azure Data Studio](https://docs.microsoft.com/pt-br/sql/azure-data-studio/download-azure-data-studio?view=sql-server-ver15).

- Servidor: localhost
- Porta: conforme a variável `MSSQL_PORT`
- Usuário: sa
- Senha: conforme a variável `SA_PASSWORD`

5. Crie o banco de dados do Protheus com o mesmo nome definido na variável `DBACCESS_DB_ALIAS`

```sql
CREATE DATABASE [protheus_db] COLLATE Latin1_General_BIN;
```

6. Reinicie os containers

```bash
docker-compose stop && docker-compose up --no-recreate
```

7. Acesse o Protheus através do endereço `http://localhost:8097` (considere a porta definida na variável `PROTHEUS_WEBAPP_PORT`)

## Apple Silicon 💻

- Se estiver utilizando um Mac com chip Apple Silicon, utilize as [tags com sufixo `-aarch64`](https://hub.docker.com/r/soulsys/totvs_protheus/tags) para os releases do Protheus a partir de **12.1.2410**.

- A técnica consiste em executar o appserver em um ambiente x86_64 através do comando [chroot](<https://wiki.archlinux.org/title/Chroot*(Portugu%C3%AAs)>). Para tal, o diretório **_/totvs_** é copiado para **_/rootfs_** durante a inicialização. Criamos links simbólicos para facilitar o gerenciamento do container.

- As demais imagens não precisam de tratamentos específicos pois funcionam normalmente com a emulação padrão do Rosetta

- Os testes foram feitos utilizando o [OrbStack](https://orbstack.dev/) em um Macbook Pro M4

## Dicas 💡

- Optamos por não compartilhar as pastas `apo`, `appserver` e `protheus_data` com o host por questões de performance

- Após o primeiro uso, sempre utilize o comando `docker-compose up --no-recreate` para não recriar o container do Protheus e perder o estado do seu `apo`, `appserver` e `protheus_data`

- Utilize periodicamente o script `sh backup.sh` no container do Protheus para salvar seus dados na pasta de volume

- Instale o [Node.js](https://nodejs.org/en/download/) e utilize os scripts NPM para subir e parar os containers de forma visual através do VS Code

- Acesse a página das [imagens no Docker Hub](https://hub.docker.com/u/soulsys) para conhecer todas as variáveis de ambiente e scripts disponíveis.

## Dúvidas e sugestões ❓

Caso encontre alguma dificuldade ou tenha sugestões de melhorias, não deixe de compartilhar conosco através da seção de [issues](https://github.com/soulsys/totvs-protheus-docker/issues).