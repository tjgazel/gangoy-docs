# Rotas
O sistema de rotas trabalha com os verbos HTTP `GET, POST, PUT, PATCH, DELETE, OPTIONS`. 
Suas rotas devem ser definidas no arquivo `routes.php` que se encontra no diretŕio app (app/routes.php).

<br>

## Métodos disponíveis
#### get()
Lida somente com solicitações HTTP do tipo GET e aceita 2 argumentos:

1 - Pattern (caminho e parâmetros opcionais).<br>
2 - [Callback](#callback) (poder uma função ou um controller).

``` 
$app->get('/books/{id}', function ($request, $response, $args) {
    // Show book identified by $args['id']
});
```

<br>
 
#### post()
Lida somente com solicitações HTTP do tipo POST e aceita 2 argumentos:

1 - Pattern (caminho e parâmetros opcionais).<br>
2 - [Callback](#callback) (poder uma função ou um controller).

```    
$app->post('/books', function ($request, $response, $args) {
    // Create new book
});
```

<br>

#### put()
Lida somente com solicitações HTTP do tipo PUT e aceita 2 argumentos:

1 - Pattern (caminho e parâmetros opcionais).<br>
2 - [Callback](#callback) (poder uma função ou um controller).

```   
$app->put('/books/{id}', function ($request, $response, $args) {
    // Update book identified by $args['id']
});
```
 
<br>
   
#### delete()
Lida somente com solicitações HTTP do tipo DELETE e aceita 2 argumentos:

1 - Pattern (caminho e parâmetros opcionais).<br>
2 - [Callback](#callback) (poder uma função ou um controller).

```   
$app->delete('/books/{id}', function ($request, $response, $args) {
    // Delete book identified by $args['id']
});
```

<br>
    
#### patch()
Lida somente com solicitações HTTP do tipo PATCH e aceita 2 argumentos:

1 - Pattern (caminho e parâmetros opcionais).<br>
2 - [Callback](#callback) (poder uma função ou um controller).

```    
$app->patch('/books/{id}', function ($request, $response, $args) {
    // Apply changes to book identified by $args['id']
});
```

<br>

#### options()
Lida somente com solicitações HTTP do tipo OPTIONS e aceita 2 argumentos:

1 - Pattern (caminho e parâmetros opcionais).<br>
2 - [Callback](#callback) (poder uma função ou um controller).

```    
$app->options('/books/{id}', function ($request, $response, $args) {
    // Return response headers
});
```

<br>
    
#### any()
Você pode adicionar uma rota que manipule todos os métodos de solicitação HTTP. Ele aceita 2 argumentos:

1 - Pattern (caminho e parâmetros opcionais).<br>
2 - [Callback](#callback) (poder uma função ou um controller).

```   
$app->any('/books/[{id}]', function ($request, $response, $args) {
    // Apply changes to books or book identified by $args['id'] if specified.
    // To check which method is used: $request->getMethod();
});
```
      
<br>

#### map()
Você pode adicionar uma rota que manipule os métodos de solicitação HTTP que forem declarados no primeiro argumento 
da função. Ele aceita 3 argumentos:

1 - Array de métodos HTTP.<br>
2 - Pattern (caminho e parâmetros opcionais).<br>
3 - [Callback](#callback) (poder uma função ou um controller).

```   
$app->map(['GET', 'POST'], '/books', function ($request, $response, $args) {
    // Create new book or list all books
});
```
      
<br>

## Parâmetros
Parâmetros de rotas são muito simples, devem estar entre `{}`
e podem ser recuperados através da matrís `$args`.

``` 
$app->get('/hello/{name}', function ($request, $response, $args) {
    echo "Hello, " . $args['name'];
});
```

<br>

#### Parâmetros Opcionais
Para tornar um parâmetro de rota opcional, envolva entre colchetes:

```
$app->get('/users[/{id}]', function ($request, $response, $args) {
    // responds to both `/users` and `/users/123`
    // but not to `/users/`
});
```
       
Múltiplos parâmetros opcionais são suportados por aninhamento:

```
$app->get('/news[/{year}[/{month}]]', function ($request, $response, $args) {
    // reponds to `/news`, `/news/2016` and `/news/2016/03`
}); 
```    

Para parâmetros opcionais "ilimitados", você pode fazer isso:

```
$app->get('/news[/{params:.*}]', function ($request, $response, $args) {
    $params = explode('/', $request->getAttribute('params'));

    // $params is an array of all the optional segments
});
```

<br>
    
#### Parâmetros com expressões regulares
Por padrão os parâmetros são escritos detro de {} e podem aceitar quaisquer valores. No entanto, também podem exigir que 
uma URI coincida com uma expressão regular, caso contrário ela não é invocada. Este exemplo a rota requer um parâmetro
com 1 ou mais dígitos:

```
$app->get('/users/{id:[0-9]+}', function ($request, $response, $args) {
    // Find user identified by $args['id']
});
```
    
<br>
    
## Rotas com nome
As rotas da aplicação podem ser atribuídas um nome. Isso é útil para que você gere uma URL para sua rota.

```
$app->get('/user/create', function ($request, $response, $args) {
    //
})->setName('user.create');
```

Você pode gerar uma URL para esta rota em uma view como no exemplo, evitando erros de URI relativa:

```
<a href="{{ path_for('user.create') }}">Create new user</a>
```

<br>

## Grupos de rota
Para ajudar a organizar rotas em grupos, gangoy fornece um group() método. O padrão de rota de cada grupo é precedido 
das rotas ou grupos contidos nela, e todos os parâmetros são disponibilizados para as rotas aninhadas:

```
$app->group('/users/{id:[0-9]+}', function () use($app) {

    $app->map(['GET', 'DELETE', 'PATCH', 'PUT'], '', function ($request, $response, $args) {
        // Find, delete, patch or replace user identified by $args['id']
    })->setName('user');
    
    $app->get('/reset-password', function ($request, $response, $args) {
        // Route for /users/{id:[0-9]+}/reset-password
        // Reset the password for user identified by $args['id']
    })->setName('user-password-reset');
    
});
```
<br>

<a name="callback"></a>
## Callback
Cada método de roteamento descrito acima aceita uma rotina de retorno como seu argumento final. Esse argumento 
pode ser uma função como demonstrado nos exemplos, mas nós construímos Gangoy framework pensando em um modelo MVC e 
recomendamos o uso de um controller [(Saiba como usar um controller)](Controllers.md).

Por padrão, ele aceita três argumentos.

- **[Request](Request_e_Response.md)** O primeiro argumento é um objeto Psr\Http\Message\ServerRequestInterface que representa a solicitação HTTP atual.
- **[Response](Request_e_Response.md)** O segundo argumento é um objeto Psr\Http\Message\ResponseInterface que representa a resposta HTTP atual.
- **Arguments** O terceiro argumento é uma matriz assiciativa que contém os valores dos parâmetros de rota.

[Veja aqui os conceitos PSR 7](http://www.php-fig.org/psr/psr-7/)

#### Exemplo com um controller

`route.php`
```
$app->get('/', '\App\Controllers\HomeController:index')->setName('home');
```

`HomeController.php`
```
<?php

namespace App\Controllers;

use TJG\Gangoy\Http\Controller\BaseController;
use \Psr\Http\Message\ServerRequestInterface as Request;
use \Psr\Http\Message\ResponseInterface as Response;

class HomeController extends BaseController
{
    public function index(Request $req, Response $res, $args)
    {
       return $response->getBody()->write("Hello");
    }
}
```