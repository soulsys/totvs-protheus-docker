# TOTVS Advpl Docker

O objetivo desse repositório é ser um ponto de partida para desenvolvimento Advpl 
utilizando nossas [imagens docker](https://hub.docker.com/u/soulsys) das soluções TOTVS.

## Importante

- Esse repositório não possui qualquer relação com a TOTVS S/A
- As imagens **NÃO** devem ser usadas em ambientes de produção
- Utilize apenas como ambiente de desenvolvimento

## Usando pela primeira vez

1. Defina as variáveis de ambiente no arquivo ***.env***. 
Todas as variáveis disponíveis estão documentadas no [docker hub](https://hub.docker.com/u/soulsys).

2. Download das imagens e criação dos containers

```bash
docker-compose up
```

3. Acesse a instância do SQL Server. Recomendamos utilizar o [Azure Data Studio](https://docs.microsoft.com/pt-br/sql/azure-data-studio/download-azure-data-studio?view=sql-server-ver15).
- Servidor: localhost
- Porta: conforme definida na variável ***MSSQL_PORT***
- Usuário: sa
- Senha: conforme definida na variável ***SA_PASSWORD***

4. Crie o banco de dados do Protheus com o mesmo nome definido na variável ***DBACCESS_DB_ALIAS***

```sql
CREATE DATABASE [protheus_db] COLLATE Latin1_General_BIN;
```

5. Acesse o Protheus através do smartclient, considerando *localhost* como servidor e a porta definida 
na variável ***PROTHEUS_TCP_PORT***