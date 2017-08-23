# Request
As rotas da aplicação recebem um objeto Request PSR 7 que representa a 
solicitação HTTP atual recebida pelo seu servidor web. O objeto de solicitação implementa 
o [PSR 7 ServerRequestInterface](http://www.php-fig.org/psr/psr-7/#3-2-1-psr-http-message-serverrequestinterface) 
com o qual você pode inspecionar e manipular o método de solicitação HTTP, cabeçalho e corpo.

<br>

### Como obter o objeto Request
O objeto Request PSR 7 é injetado em suas rotas como o primeiro argumento para o 
retorno de chamada da rota (callback) ou no método do seu controller, exemplo:

```
$app->get('/foo', function (ServerRequestInterface $request, ResponseInterface $response) {
    // Use the PSR 7 $request object
    return $response;
});
```

```
<?php

namespace App\Controllers;

use TJG\Gangoy\Http\Controller\BaseController;
use \Psr\Http\Message\ServerRequestInterface as Request;
use \Psr\Http\Message\ResponseInterface as Response;

class HomeController extends BaseController
{
    public function index(Request $request, Response $response, $args)
    {
       // Use the PSR 7 $request object
       
       return $response;
    }
}
```

O objeto Request PSR 7 também é injetado nos middleware como o primeiro argumento. 
Veja nosso exemplo de implementação:

```
<?php

namespace App\Middleware;

use TJG\Gangoy\Http\Middleware\MiddlewareInterface;
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
        // Use the PSR 7 $request object
        
        return $next($request, $response);
    }
}
``` 

<br>

### Métodos
Toda solicitação HTTP possui um método que normalmente é um dos abaixo:

- GET
- POST
- PUT
- DELETE
- HEAD
- PATCH
- OPTIONS

Você pode obter o método da requisição HTTP fazendo o seguinte: 
    
    $method = $request->getMethod();
    
A implementação do PSR 7 também fornece esses métodos que retornam true ou false.

- $request->isGet()
- $request->isPost()
- $request->isPut()
- $request->isDelete()
- $request->isHead()
- $request->isPatch()
- $request->isOptions()

É possível substituir o método de solicitação HTTP. Isso é útil se, por exemplo, você 
precisa enviar uma requisição do tipo PUT usando um navegador da web tradicional que só 
suporta GET ou POST. Veja o exemplo:

```
<form method="POST" action="">
    <input type="hidden" name="_METHOD" value="PUT">
</form>
```

Você pode buscar o método HTTP original (não anulado) com o método do objeto PSR 7 Request **getOriginalMethod()**.

<br>

### URI
Toda solicitação HTTP possui um URI que identifica a rota solicitada ao aplicativo. O URI tem várias partes:

- Scheme (e.g. http or https)
- Host (e.g. example.com)
- Port (e.g. 80 or 443)
- Path (e.g. /users/1)
- Query string (e.g. sort=created&dir=asc)

Você pode capturar o objeto URI do objeto Request PSR 7 da seguinte forma:

    $uri = $request->getUri();
    
O objeto URI é em sí um objeto que fornece os seguintes métodos para inspecionar as partes de uma URL HTTP:

- getScheme()
- getAuthority()
- getUserInfo()
- getHost()
- getPort()
- getPath()
- getBasePath()
- getQuery() (Retorna a string de consulta completa, por exemplo a=1&b=2)
- getFragment()
- getBaseUrl()

Você pode obter os parâmetros da consulta como uma matriz associativa no objeto Request usando getQueryParams().

Você também pode obter um único valor de parâmetro usando getQueryParam($key).

