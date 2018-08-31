# Gangoy Framework
[![Latest Stable Version](https://poser.pugx.org/tjgazel/gangoy-framework/v/stable)](https://packagist.org/packages/tjgazel/gangoy-framework)
[![Total Downloads](https://poser.pugx.org/tjgazel/gangoy-framework/downloads)](https://packagist.org/packages/tjgazel/gangoy-framework)
[![License](https://poser.pugx.org/tjgazel/gangoy-framework/license)](https://packagist.org/packages/tjgazel/gangoy-framework)


<img src="logo-256x256.png" alt="Gangoy Framework" style="float: left;"/>

**Gangoy** é um framework PHP desenvolvido com um conjunto de bibliotecas bem conhecidas na comunidade PHP e segue as orientações das PSRs 
fornecidas pelo [**PHP-FIG**](http://www.php-fig.org/ "PHP-FIG").

Usamos o conceito de Serviços/Container, tornando fácil adicionar novas funcionalidades ou desativar alguma que já vem como padrão.

Foi projetado para que o desenvolvedor tenha em mãos recursos básicos/excenciais que são comuns em qualquer projeto Web,
ficando assim fácil e rápido iniciar um novo projeto seguindo boas práticas e padrões de desenvolvimento.

> Veja mais sobre as bibliotecas usadas na seção [**Agradecimentos**](#agradecimentos).

<br>

## Desenvolvedor
Talles Gazel<br>
Site: [http://www.tjgweb.com.br](http://www.tjgweb.com.br "TJG Web")<br>
GitHub: [https://github.com/tjgazel/gangoy-framework](https://github.com/tjgazel/gangoy-framework)

<br>

## Requisitos de sistema
- Web server com URL rewriting ativo.
- PHP 7.1 ou superior.

<br>

## Instalação
Utilizando [Composer](https://getcomposer.org/) Create-Project

```
composer create-project tjgazel/gangoy-framework nome_projeto
```

Acesse com o terminal até o diretório onde foi criado o projeto e digite o seguinte comando para iniciar o servidor de desenvolvimento.

```
php gangoy server
```

Abra o navegador e acesse `localhost:8080`

<br>

## Configurações gerais
Após a instalação você deve olhar para o arquivo `.env` , onde deve ser configurado todos os dados sensíveis 
do seu ambiente. Caso o arquivo não tenha sido criado automaticamente durante a instalação você pode copiar e renomear o arquivo `.env.example`

```apache
DETERMINE_ROUTE_BEFORE_APP_MIDDLEWARE=true
CACHE_ROUTER=false
CACHE_CONTAINER=false
CACHE_VIEWS=false
DISPLAY_ERRORS=true

AUTH_SESSION_NAME=user

DB_DRIVER=mysql
DB_HOST=localhost
DB_PORT=3306
DB_DATABASE=gangoy
DB_USERNAME=root
DB_PASSWORD=root
DB_CHARSET=utf8mb4
DB_COLLATION=utf8mb4_unicode_ci
#DB_PREFIX=
```
    
- **DETERMINE_ROUTE_BEFORE_APP_MIDDLEWARE:**  Determina as rotas antes de rodar os middlewares. Deixe como true se 
for utilizar o sistema de autenticação.
- **CACHE_ROUTER:** Ativa o sistema de cache das rotas.
- **CACHE_CONTAINER:** Ativa o sistema de cache do container.
- **CACHE_VIEWS:**  Ativa o sistema de cache para as views.
- **ERROR_DETAILS:**  Mostra erros de forma elegante, ideal para o desenvolvimento. Deixe como false no seu ambiente de produção.
- **AUTH_SESSION_NAME:** Nome da chave que será usada para a sessão de autenticação.
- **DB_:**  Configurações do seu banco de dados. Atualmente suporta `SQLite, MySQL, PostgreSQL`. 

**Nota:** Evite usar cache no ambiente de desenvolvimento para que as alterações sejam refletidas instantâneamente.

<br>

### Servidor
Todas as solicitações HTTP devem ser encaminhadas para o arquivo `index.php` que se encontra no diretório public. 

<br>

#### Apache
Certifique-se de que seus arquivos `.htaccess` e `index.php` estejam no mesmo diretório acessível ao público. 
O arquivo `.htaccess` deve conter este código:

```apache
<IfModule mod_rewrite.c>
    RewriteEngine On
    RewriteCond %{REQUEST_FILENAME} !-f
    RewriteCond %{REQUEST_FILENAME} !-d
    RewriteRule ^ index.php [QSA,L]
</IfModule>
```
 
<br>
   
#### Nginx
Você deve atualizar os `server_name`, `error_log`, `access_log`, e `root` com seus próprios valores. A diretiva `root` 
é o caminho para o diretório `public` do seu aplicativo onde está o arquivo `index.php`

```nginx
server {
    listen 80;
    server_name example.com;
    index index.php;
    error_log /path/to/example.error.log;
    access_log /path/to/example.access.log;
    root /path/to/public;

    location / {
        try_files $uri /index.php$is_args$args;
    }

    location ~ \.php {
        try_files $uri =404;
        fastcgi_split_path_info ^(.+\.php)(/.+)$;
        include fastcgi_params;
        fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
        fastcgi_param SCRIPT_NAME $fastcgi_script_name;
        fastcgi_index index.php;
        fastcgi_pass 127.0.0.1:9000;
    }
}
``` 

<br>

## <a name="agradecimentos"></a>Agradecimentos
Agradecemos a todos os desenvolvedores das bibliotecas citadas abaixo. Atravez delas foi possível chegarmos a este trabalho.

- [Slim Framework](https://www.slimframework.com/)
- [Slim Framework Flash Messages](https://github.com/slimphp/Slim-Flash)
- [Slim Framework CSRF Protection](https://github.com/slimphp/Slim-Csrf)
- [Slim Validation](https://github.com/awurth/slim-validation) / [Respect Validation](https://github.com/Respect/Validation)
- [Slim Framework Twig View](https://github.com/slimphp/Twig-View)
- [Twig](https://twig.symfony.com/doc/1.x/)
- [Laravel - Illuminate Database package](https://packagist.org/packages/illuminate/database)
- [Laravel - Mix](https://laravel.com/docs/5.6/mix)
- [Phinx](https://phinx.org/)
- [Symfony console](https://github.com/symfony/console)
- [PHP dotenv](https://github.com/vlucas/phpdotenv)
- [PHP-DI](http://php-di.org/)
- [Monolog](https://github.com/Seldaek/monolog)
- [Zend - Event Manager](https://zendframework.github.io/zend-eventmanager/)
- [Doctrine - Inflector](https://www.doctrine-project.org/projects/doctrine-inflector/en/1.3/index.html)


<br>

## License
The `Gangoy Framework` is open-sourced software licensed under the [MIT license](http://opensource.org/licenses/MIT)
