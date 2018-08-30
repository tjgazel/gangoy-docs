# Rotas
O sistema de rotas trabalha com os verbos HTTP `GET, POST, PUT, PATCH, DELETE, OPTIONS`. 
Suas rotas devem ser definidas nos arquivos `routes.php` correspondentes aos seus respectivos módulos. Exemplo: `src/ModuloName/config/routes.php`

<br>

## Métodos disponíveis
#### get()
Lida somente com solicitações HTTP do tipo GET e aceita 2 argumentos:

1 - Pattern (caminho e parâmetros opcionais).<br>
2 - [Callback](#callback) (pode ser uma função ou um array informando o controller e sua ação).

``` 
$app->get('/books', function ($response) {
    return $response->getBody()->write('<h1>Hello World!</h1>');
});
```
``` 
$app->get('/', ['\App\Controllers\DefaultController', 'index']);
});
```

<br>
 
#### post()
Lida somente com solicitações HTTP do tipo POST e aceita 2 argumentos:

1 - Pattern (caminho e parâmetros opcionais).<br>
2 - [Callback](#callback) (pode ser uma função ou um array informando o controller e sua ação).

```    
$app->post('/books', function ($request, $response) {
    $data = $request->getParsedBody()->getAll();
    // save
    return $this->redirect($response, '/books');
});
```

<br>

#### put()
Lida somente com solicitações HTTP do tipo PUT e aceita 2 argumentos:

1 - Pattern (caminho e parâmetros opcionais).<br>
2 - [Callback](#callback) (pode ser uma função ou um array informando o controller e sua ação).

```   
$app->put('/books/{id}', function ($request, $response, $id) {
    $data = $request->getParsedBody()->getAll();
    // update books identified by id param
    return $this->redirect($response, '/books');
});
```
 
<br>
   
#### delete()
Lida somente com solicitações HTTP do tipo DELETE e aceita 2 argumentos:

1 - Pattern (caminho e parâmetros opcionais).<br>
2 - [Callback](#callback) (pode ser uma função ou um array informando o controller e sua ação).

```   
$app->delete('/books/{id}', function ($response, $id) {
    // Delete book identified by $id
});
```

<br>
    
#### patch()
Lida somente com solicitações HTTP do tipo PATCH e aceita 2 argumentos:

1 - Pattern (caminho e parâmetros opcionais).<br>
2 - [Callback](#callback) (pode ser uma função ou um array informando o controller e sua ação).

```    
$app->patch('/books/{id}', function ($response, $id) {
    // Apply changes to book identified by $id
});
```

<br>

#### options()
Lida somente com solicitações HTTP do tipo OPTIONS e aceita 2 argumentos:

1 - Pattern (caminho e parâmetros opcionais).<br>
2 - [Callback](#callback) (pode ser uma função ou um array informando o controller e sua ação).

```    
$app->options('/books/{id}', function ($response) {
    // Return response headers
});
```

<br>
    
#### any()
Você pode adicionar uma rota que manipule todos os métodos de solicitação HTTP. Ele aceita 2 argumentos:

1 - Pattern (caminho e parâmetros opcionais).<br>
2 - [Callback](#callback) (pode ser uma função ou um array informando o controller e sua ação).

```   
$app->any('/books/[{id}]', function ($request, $response, $id) {
    // Apply changes to books or book identified by $id if specified.
    // To check which method is used: $request->getMethod();
});
```
      
<br>

#### map()
Você pode adicionar uma rota que manipule os métodos de solicitação HTTP que forem declarados no primeiro argumento 
da função. Ele aceita 3 argumentos:

1 - Array de métodos HTTP.<br>
2 - Pattern (caminho e parâmetros opcionais).<br>
3 - [Callback](#callback) (pode ser uma função ou um array informando o controller e sua ação).

```   
$app->map(['GET', 'POST'], '/books', function ($request, $response) {
    // Create new book or list all books
});
```
      
<br>

## Parâmetros
Parâmetros de rotas são muito simples, devem estar entre `{}`
e podem ser recuperados como argumentos da função callback ou através do `$request->getAttribute('param_name'));`.

``` 
$app->get('/hello/{name}', function ($request, $response, $name) {
    echo "Hello, " . $name;
});
```

<br>

#### Parâmetros Opcionais
Para tornar um parâmetro de rota opcional, envolva entre colchetes:

```
$app->get('/users[/{id}]', function ($request, $response, $id) {
    // responds to both `/users` and `/users/123`
    // but not to `/users/`
});
```
       
Múltiplos parâmetros opcionais são suportados por aninhamento:

```
$app->get('/news[/{year}[/{month}]]', function ($request, $response, $year, $month) {
    // responds to `/news`, `/news/2016` and `/news/2016/03`
}); 
```    

Para parâmetros opcionais "ilimitados", você pode fazer isso:

```
$app->get('/news[/{params:.*}]', function ($request, $response) {
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
$app->get('/users/{id:[0-9]+}', function ($request, $response, $id) {
    // Find user identified by $id
});
```
    
<br>
    
## Rotas com nome
As rotas da aplicação podem ser atribuídas um nome. Isso é útil para que você gere uma URL para sua rota.

```
$app->get('/user/create', function () {
    //
})->setName('user.create');
```

Você pode gerar uma URL para esta rota em uma view evitando erros de URI relativa:

```
<a href="{{ path_for('user.create') }}">Create new user</a>
```

<br>

## Grupos de rota
Para ajudar a organizar rotas em grupos, gangoy fornece um group() método. O padrão de rota de cada grupo é precedido 
das rotas ou grupos contidos nela, e todos os parâmetros são disponibilizados para as rotas aninhadas:

```
$app->group('/users/{id:[0-9]+}', function () use ($app) {

    $app->map(['GET', 'DELETE', 'PATCH', 'PUT'], '', function ($request, $response, $id) {
        // Find, delete, patch or replace user identified by $id
    })->setName('user');
    
    $app->get('/reset-password', function ($request, $response, $id) {
        // Route for /users/{id:[0-9]+}/reset-password
        // Reset the password for user identified by $id
    })->setName('user.password-reset');
    
});
```
<br>

<a name="callback"></a>
## Callback
Cada método de roteamento descrito acima aceita uma rotina de retorno como seu argumento final. Esse argumento 
pode ser uma função como demonstrado nos exemplos, mas nós construímos Gangoy framework pensando em um modelo MVC e 
recomendamos o uso de um controller.
 
- [(Saiba como usar um controller)](controllers.md).

O callback aceita como argumentos os parâmetros de rota ou qualquer classe registrada no conteiner de dependências, mas observe que para as classes serem injetadas automaticamente deve-se passar a assinatura na declaração assim como mostrado nos exemplos o uso do Request e Response.

- **[Request](psr_7_request.md)** - Psr\Http\Message\ServerRequestInterface que representa a solicitação HTTP atual.
- **[Response](psr_7_response.md)** - Psr\Http\Message\ResponseInterface que representa a resposta HTTP atual.

[Veja aqui os conceitos PSR 7](http://www.php-fig.org/psr/psr-7/)

#### Exemplo com um controller

routes.php
```
$app->get('/', ['\App\Controllers\DefaultController', 'index'])->setName('home');
```

DefaultController.php
```
<?php

namespace App\Controllers;

use Gangoy\Core\Http\AbstractController;
use Psr\Http\Message\ServerRequestInterface as Request;
use Psr\Http\Message\ResponseInterface as Response;

class DefaultController extends AbstractController
{
    public function index(Request $request, Response $response)
    {
       return $response->getBody()->write("Hello");
    }
}
```