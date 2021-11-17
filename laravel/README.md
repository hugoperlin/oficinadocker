## Criar uma Aplicação com Laravel ##

### 1. Crie uma pasta para o aplicativo ###

```
mkdir src
```

### 2. Execute este comando para criar a aplicação ###

```
docker run -it --rm -v $(pwd)/src:/app -w /app --user $(id -u):$(id -g) composer composer create-project laravel/laravel .
```

Explicação do comando

* **docker run** : executa um container docker
* **-it** : indica que iremos interagir com o container
* **--rm** : remove o container depois da utilização
* **-v $(pwd)/src:/app** : mapeia a pasta src para a pasta /app no container
* **-w /app** : indica que o diretório de trabalho do containeir será /app
* **--user $(id -u):$(id -g)** : indica que o container será executado como o usuário atual
* **composer** : nome da imagem que estamos utilizando para criar o container
* **composer create-project laravel/laravel .** : comando do composer para criar a aplicação


## Utilizando o Docker-Compose ##

### 1. Inicialize os serviços com o docker-compose ###

```
docker-compose up
```

### 2. Acesse o conteiner do laravel ###
```
docker-compose exec web docker-php-ext-install pdo pdo_mysql
```
Este comando instala no container os drivers necessários para o Laravel acessar
o mysql.

### 3. Configure as variáveis de ambiente ###

Altere o arquivo *.env*:

DB_CONNECTION=mysql

DB_HOST=db

DB_PORT=3306

DB_DATABASE=app

DB_USERNAME=user

DB_PASSWORD=user12345

### 4. Crie uma rota para verificar o funcionamento ###

Insira o código abaixo no arquivo *routes/web.php*

```
use Illuminate\Database\Connection;
Route::get('/', function () {
    try {
        DB::connection()->getPdo();
        echo "Connected successfully to: " . DB::connection()->getDatabaseName();
    } catch (\Exception $e) {
        die("Could not connect to the database. Please check your configuration. error:" . $e );
    }

    return view('welcome');
})
```
