# Response
As rotas e os middlewares da aplicação recebem um objeto Response PSR 7 que representa a resposta HTTP atual 
a ser retornada ao cliente. O objeto de resposta implementa o `ResponseInterface` PSR 7 com o qual você pode inspecionar 
e manipular o [status](#status) de resposta HTTP, [headers](#header) e [body](#body).

<br>

## Como obter o objeto Response
- O objeto Response PSR 7 pode ser injetado automaticamente pelo [Container de depêndencias](container.md) em suas rotas como argumento do
callback ou nos métodos do seu controller, exemplo:

    `src/App/config/routes.php`

    ~~~php 
    use \Psr\Http\Message\ResponseInterface as Response;

    $app->get('/foo', function (Response $response) {
        // Use the PSR 7 $response object
        return $response;
    });
    ~~~

    `src/App/Controllers/DefaultController.php`

    ~~~php
    <?php

    namespace App\Controllers;

    use Gangoy\Core\Http\Controller\AbastractController;
    use Psr\Http\Message\ServerRequestInterface as Request;
    use Psr\Http\Message\ResponseInterface as Response;

    class DefaultController extends AbastractController
    {
        public function index(Request $request, Response $response, $args)
        {
            // Use the PSR 7 $response object        
            return $response;
        }
    }
    ~~~

<br>

- O objeto Response PSR 7 também é injetado automaticamente pelo [Container de depêndencias](container.md) nos middlewares como o segundo argumento. 
Veja nosso exemplo de implementação:

    `src/App/Middlewares/MyMiddleware.php`

    ~~~php 
    <?php

    namespace App\Middlewares;

    use Gangoy\Core\Http\MiddlewareInterface;
    use Psr\Http\Message\ServerRequestInterface as Request;
    use Psr\Http\Message\ResponseInterface as Response;

    class MyMiddleware implements MiddlewareInterface
    {
        /**
        * @param  Request   $request  PSR7 request
        * @param  Response  $response PSR7 response
        * @param  callable  $next     Next middleware
        *
        * @return mixed
        */
        public function __invoke(Request $request, Response $response, callable $next)
        {
            // Use the PSR 7 $response object
            
            return $next($request, $response);
        }
    }
    ~~~

<br>

## <a name="status"></a>Status
Toda resposta HTTP possui um [código de status numérico](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html) . 
O código de status identifica o tipo de resposta HTTP a ser retornado ao cliente. O código de status padrão do objeto 
de Response PSR 7 é 200(OK). Você pode obter o código de status do objeto Response PSR 7 com o método `getStatusCode()`.

~~~php 
$status = $response->getStatusCode();
~~~

Você pode pegar um objeto Response PSR 7 e atribuir um novo código de status:

~~~php 
$newResponse = $response->withStatus(302);
return $newResponse;
~~~

<br>

## <a name="header"></a>Header
Toda resposta HTTP possui cabeçalhos. Estes são metadados que descrevem a resposta HTTP, mas não são visíveis no corpo 
da resposta. O objeto Response PSR 7, fornece vários métodos para inspecionar e manipular seus cabeçalhos.

- Você pode obter todos os cabeçalhos de resposta HTTP como uma matriz associativa usando o método `getHeaders()`. 
As chaves da matriz associativa resultante são os nomes de cabeçalho e seus valores

    ~~~php 
    $headers = $response->getHeaders();
    foreach ($headers as $name => $values) {
        echo $name . ": " . implode(", ", $values);
    }
    ~~~

<br>

- Você pode obter os valores de um único cabeçalho. Isso retorna uma matriz de valores para o nome do cabeçalho dado. 
Lembre-se, um único cabeçalho HTTP pode ter mais de um valor!

    ~~~php 
    $headerValueArray = $response->getHeader('Access-Control-Allow-Origin');
    ~~~

<br>

- Você pode testar a presença de um cabeçalho com o hasHeader($name).

    ~~~php 
    if ($response->hasHeader('Access-Control-Allow-Origin')) {
        // Do something
    }
    ~~~

<br>

- Você pode definir um valor de cabeçalho com o método `withHeader($name, $value)` do objeto Response do PSR 7.

    ~~~php  
    $newResponse = $oldResponse->withHeader('Content-type', 'application/json');
    ~~~

    >**Lembrete** <br>
    >O objeto Response é imutável. Esse método retorna uma cópia do objeto Response que possui o novo valor de cabeçalho. 
    Esse método é destrutivo e ele substitui valores de cabeçalho existentes já associados ao mesmo nome de cabeçalho.

<br>

- Você pode anexar um valor de cabeçalho com o método `withAddedHeader($name, $value)` do objeto Response do PSR 7 .

    ~~~php 
    $newResponse = $oldResponse->withAddedHeader('Allow', 'PUT');
    ~~~

    >**Lembrete** <br>
    >Ao contrário do método `withHeader()`, o método  `withAddedHeader()` adiciona o novo valor ao conjunto de valores que 
    já existem para o mesmo nome de cabeçalho. O objeto Response é imutável. Esse método retorna uma cópia do objeto 
    Response que possui o valor de cabeçalho anexado.

<br>

- Você pode remover um cabeçalho com o método `withoutHeader($name)` do objeto Response PSR 7.

    ~~~php 
    $newResponse = $oldResponse->withoutHeader('Allow');
    ~~~

<br>

## <a name="body"></a>Body
Uma resposta HTTP normalmente tem um corpo. Assim como o objeto Request PSR 7, o objeto Response PSR 7 implementa o 
corpo como uma instância de \Psr\Http\Message\StreamInterface. Você pode obter a instância do StreamInterface com o corpo 
de resposta HTTP através do método `getBody()` do objeto Response PSR 7 . O método `getBody()` é preferível se o comprimento 
da sáida de resposta HTTP for desconhecido ou muito grande para a memória disponível.

~~~php 
$body = $response->getBody();
~~~

Após obter o corpo da requisição você escrever e personalizar sua resposta usando o método `write()` como no exemplo:

~~~php 
$body = $response->getBody();
$body->write('Hello');
~~~

<br>

## Retornando JSON
O objeto Response que usamos em Gangoy Framework, herda as caracterísiticas do Slim. Com isso, ele possui um método 
personalizado `withJson($data, $status, $encodingOptions)` para ajudar a simplificar o processo de retorno de dados JSON.

- $data contém a estrutura de dados que você deseja retornar como JSON.
- $status é opcional e pode ser usado para retornar um código HTTP personalizado.
- $encodingOptions é opcional e são as mesmas opções de codificação usadas em `json_encode()`.

Na forma mais simples, os dados JSON podem ser retornados com um código de status HTTP padrão de 200.

~~~php 
$data = array('name' => 'Bob', 'age' => 40);
$newResponse = $oldResponse->withJson($data);
~~~

Também podemos retornar dados JSON com um código de status HTTP personalizado.

~~~php 
$data = array('name' => 'Rob', 'age' => 40);
$newResponse = $oldResponse->withJson($data, 201);
~~~