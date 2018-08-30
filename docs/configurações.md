# Configurações gerais

Após a instalação você deve olhar para o arquivo `.env` , onde deve ser configurado todos os dados sensíveis 
do seu ambiente. Caso o arquivo não tenha sido criado automaticamente durante a instalação você pode copiar e renomear o arquivo `.env.example`

```
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

# Servidor
Todas as solicitações HTTP devem ser encaminhadas para o arquivo `index.php` que se encontra no diretório public. 

## Apache
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
   
## Nginx
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
