## Criar uma aplicação em adonisjs utilizando o docker ##

### 1. Crie uma pasta para armazenar a aplicação ###
```
mkdir src
```
### 2. Executar o comando para acessar uma imagem do nodejs ###

```
docker run -it --name adonisapp -v $(pwd)/src:/app:Z -w /app --user $(id -u):$(id -g) --rm node:17 npm init adonis-ts-app@latest .
```

Explicação do comando:
 * **docker run**: utilizado para executar um container
 * **-it**: indica que queremos interagir com o container criado
 * **--name adonisapp**: nome do container
 * **-v $(pwd)/src:/app:Z**: mapeia o diretório src para a pasta /app do container. O :Z é necessário por conta das permissões do mapeamento
* **-w /app**: indica que a pasta de trabalho dentro do container é a /app
* **--user $(id -u):$(id -g)**: executa os comandos como o usuário node dentro do container
* **node:17**: imagem base da qual será criado o container
* **npm init adonis-ts-app@latest .**: comando utilizado para criar uma aplicação adonisjs

### 3. Executar o comando para rodar o adonijs ###

```
docker run -it --name adonisapp -v $(pwd)/src:/app:Z -w /app -p 3333:3333 --user $(id -u):$(id -g) --rm node:17 node ace serve --watch
```

Explicação do comando:
 * **docker run**: utilizado para executar um container
 * **-it**: indica que queremos interagir com o container criado
 * **--name adonisapp**: nome do container
 * **-v $(pwd)/src:/app:Z**: mapeia o diretório src para a pasta /app do container. O :Z é necessário por conta das permissões do mapeamento
 * **-p 3333:3333**: mapeia a porta 3333 do host para a porta 3333 do container
 * **-w /app**: indica que a pasta de trabalho dentro do container é a /app
* **--user $(id -u):$(id -g)**: executa os comandos como o usuário atual dentro do container
* **node:17**: imagem base da qual será criado o container
* **node ace serve --watch**: comando para rodar a aplicação adonisjs


### 5. Acesse a aplicação no seu browser ###
```
<http://localhost:3333>
```

### 6. Caso você precise rodar um comando dentro do container ###

```
docker exec -it adonisapp /bin/bash
```

Exeplicação do comando:
docker exec -> executa uma ação no container
-it -> indica que a ação será interativa
adonisapp -> nome do container onde a ação será executada
/bin/bash -> comando a ser executado, neste caso o terminal de comando

## Utilizando o Docker-Compose ##

### 1. Inicializar os serviços com o docker-compose ###

```
docker-compose up
```

Inicializa todos os serviços definidos no arquivo docker-compose.yaml

### 2. Abrir um novo terminal e executar o seguinte comando ###

```
docker-compose exec web bash
```

Abre um terminal no container web.


### 3. Instalar e configurar o lucid ###

<https://docs.adonisjs.com/guides/database/introduction>

Configurar as variáveis de ambiente

Arquivo *.env*

```
MYSQL_HOST=db
MYSQL_PORT=3306
MYSQL_USER=user
MYSQL_PASSWORD=user12345
MYSQL_DB_NAME=app
```

Arquivo *env.ts*

```
DB_CONNECTION: Env.schema.string(),
MYSQL_HOST: Env.schema.string({ format: 'host' }),
MYSQL_PORT: Env.schema.number(),
MYSQL_USER: Env.schema.string(),
MYSQL_PASSWORD: Env.schema.string.optional(),
MYSQL_DB_NAME: Env.schema.string(),
```

### 4. Criar rota para verificar as configurações ###

Insira o código abaixo no arquivo  *start/route.ts*

```
import HealthCheck from '@ioc:Adonis/Core/HealthCheck'

// check db connection
Route.get('health', async ({ response }) => {
  const report = await HealthCheck.getReport()

  return report.healthy ? response.ok(report) : response.badRequest(report)
})
```

### 5. Caso de erro de autenticação ###

Executar no terminal o seguinte comando para acessar o container do banco de dados:
```
docker-compose exec db bash
```

Dentro do terminal executar este comando para acessar o terminal do mysql:

```
mysql -u root -p
```
Dentro do terminal do mysql, executar estes comandos para mudar o tipo de autenticação:

```
ALTER USER 'user'@'%' IDENTIFIED WITH mysql_native_password BY 'user12345';
flush privileges;
```