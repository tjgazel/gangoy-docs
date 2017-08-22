# Configurações

Após a instalação você deve olhar para o arquivo `.env` , onde deve ser configurado todos os dados sensíveis 
do seu ambiente.

```
DISPLAY_ERRORS=true
CACHE_VIEWS=false

DETERMINE_ROUTE_BEFORE_APP_MIDDLEWARE=true
AUTH_SESSION_MODEL=\App\Models\User
AUTH_SESSION_NAME=user

DB_DRIVER=sqlite
DB_HOST=localhost
DB_PORT=3306
DB_DATABASE=gangoy
DB_USERNAME=root
DB_PASSWORD=root
DB_CHARSET=utf8
DB_COLLATION=utf8_unicode_ci
DB_PREFIX=
```
    
- **DISPLAY_ERRORS**  mostra erros de forma elegante, ideal para o desenvolvimento. Deixe como false no seu ambiente de produção.
- **CACHE_VIEWS**  Ativa o sistema de cache para as views.
- **DETERMINE_ROUTE_BEFORE_APP_MIDDLEWARE**  Determina as rotas antes de rodar os middleware. Deixe como true se 
for utilizar o sistema de autenticação.
- **AUTH_SESSION_MODEL**  namespace do model usado para autenticação.
- **AUTH_SESSION_NAME**  nome da chave que será usada para a sessão de autenticação.
- **DB_**  configurações do seu banco de dados. Por padrão o framework está configurado sqlite, ideal para o 
desenvolvimento e teste locais. 

<br>

# Configurações de Servidor
Todas as solicitações HTTP devem ser encaminhadas para o arquivo `index.php` que se encontra no diretório public. 

#### Apache
Certifique-se de que seus arquivos `.htaccess` e `index.php` estejam no mesmo diretório acessível ao público. 
O arquivo `.htaccess` deve conter este código:

```
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

```
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
