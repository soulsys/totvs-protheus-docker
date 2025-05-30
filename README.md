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

1. Defina as variáveis de ambiente no arquivo **_.env_**.
   Todas as variáveis disponíveis estão documentadas no [docker hub](https://hub.docker.com/u/soulsys).

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
- Porta: conforme a variável **_MSSQL_PORT_**
- Usuário: sa
- Senha: conforme a variável **_SA_PASSWORD_**

5. Crie o banco de dados do Protheus com o mesmo nome definido na variável **_DBACCESS_DB_ALIAS_**

```sql
CREATE DATABASE [protheus_db] COLLATE Latin1_General_BIN;
```

6. Reinicie os containers

```bash
docker-compose stop && docker-compose up --no-recreate
```

7. Acesse o Protheus através do smartclient ou webapp, considerando _localhost_ como servidor e a porta definida
   na variável **_PROTHEUS_TCP_PORT_** ou **_PROTHEUS_WEBAPP_PORT_**

## 12.1.2410 - Apple Silicon 💻

- Se estiver utilizando um Mac com chip Apple Silicon, utilize a tag [12.1.2410-aarch64](https://hub.docker.com/r/soulsys/totvs_protheus/tags) para o release 12.1.2410 do Protheus

- A técnica consiste em executar o appserver em um ambiente x86_64 através do comando [chroot](<https://wiki.archlinux.org/title/Chroot*(Portugu%C3%AAs)>). Para tal, o diretório **_/totvs_** é copiado para **_/rootfs_** durante a inicialização. Criamos links simbólicos para facilitar o gerenciamento do container.

- As demais imagens não precisam de tratamentos específicos e funcionam normalmente com a emulação padrão do Rosetta

- Os testes foram feitos utilizando o [OrbStack](https://orbstack.dev/) em um Macbook Pro M4

## Dicas 💡

- Optamos por não compartilhar as pastas _apo_, _appserver_ e _protheus_data_ com o host por questões de performance

- Após o primeiro uso, sempre utilize o comando **_docker-compose up --no-recreate_** para não recriar o container
  do Protheus e perder o estado do seu _apo_, _appserver_ e _protheus_data_

- Utilize periodicamente o script **_sh backup.sh_** no container do Protheus para salvar seus dados na pasta de volume

- Instale o [Node.js](https://nodejs.org/en/download/) e utilize os scripts NPM para subir e parar os containers
  de forma visual através do VS Code

- Acesse a página das [imagens no Docker Hub](https://hub.docker.com/u/soulsys) para conhecer todas
  as variáveis de ambiente e scripts disponíveis.

## Dúvidas e sugestões ❓

Caso encontre alguma dificuldade ou tenha sugestões de melhorias, não deixe de compartilhar conosco através da seção de [issues](https://github.com/soulsys/totvs-protheus-docker/issues).
