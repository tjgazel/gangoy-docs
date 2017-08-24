# Request
As rotas e o middleware da aplicação recebem um objeto Request da PSR 7 que representa a 
solicitação HTTP atual recebida pelo seu servidor web. O objeto de solicitação implementa 
o [PSR 7 ServerRequestInterface](http://www.php-fig.org/psr/psr-7/#3-2-1-psr-http-message-serverrequestinterface) 
com o qual você pode inspecionar e manipular o [método](#metodo) de solicitação HTTP, [header](#header) e [body](#body).

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
<br>

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

<a name="metodo"></a>
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

- Scheme (http or https)
- Host (example.com)
- Port (80 or 443)
- Path (/users/1)
- Query string (sort=created&dir=asc)

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

Você pode obter os parâmetros da consulta como uma matriz associativa no objeto Request usando `getQueryParams()`.

Você também pode obter um único valor de parâmetro usando `getQueryParam($key)`.

<br>

<a name="header"></a>
### Headers
Toda solicitação HTTP possui cabeçalhos. Estes são metadados que descrevem a solicitação HTTP, mas não estão visíveis 
no corpo da solicitação. O objeto Request PSR 7 fornece diversos métodos para inspecionar seus cabeçalhos.

Você pode obter todos os cabeçalhos de solicitação HTTP como uma matriz associativa usando o método `getHeaders()`. 
As chaves da matriz associativa resultante são os nomes de cabeçalho e seus valores

    $headers = $request->getHeaders();
    foreach ($headers as $name => $values) {
        echo $name . ": " . implode(", ", $values);
    }

Você pode obter os valores de um único cabeçalho. Isso retorna uma matriz de valores para o nome do cabeçalho dado. 
Lembre-se, um único cabeçalho HTTP pode ter mais de um valor!

    $headerValueArray = $request->getHeader('Accept');


Você pode testar a presença de um cabeçalho com o hasHeader($name).

    if ($request->hasHeader('Accept')) {
        // Do something
    }

<br>

<a name="body"></a>
### Body
Toda solicitação HTTP possui um corpo (Body). Se você está criando um aplicativo que consome dados JSON ou XML, você 
pode usar o método getParsedBody() do objeto Request PSR 7 para analisar o corpo de solicitação HTTP em um formato 
PHP nativo. Gangoy pode analisar dados JSON, XML e URL-enconded.

    $parsedBody = $request->getParsedBody();

- Request JSON são convertidos em matrizes com json_decode($input, true).
- Request XML são convertidos em um SimpleXMLElement com simplexml_load_string($input).
- Request URL-enconded são convertidos em uma matriz PHP com parse_str($input).


Para Request URL-encoded, você também pode obter o valor de parâmetro único, com valor padrão opcional se o parâmetro 
estiver faltando usando o método `getParsedBodyParam($key, $default = null)`.

Tecnicamente falando, o objeto Request PSR 7 da aplicação Gangoy, representa o corpo de solicitação HTTP como uma instância 
de \Psr\Http\Message\StreamInterface. Você pode obter a instância StreamInterface do Body HTTP com o método `getBody()` 
do objeto Request do PSR 7 . O método getBody() é preferível se o tamanho da solicitação HTTP recebida for desconhecido 
ou muito grande para a memória disponível.

    $body = $request->getBody();

<br>

### Uploaded Files
Os carregamentos de arquivos $_FILES estão disponíveis a partir do método `getUploadedFiles()` do objeto Request . Isso 
retorna uma matriz pelo nome do elemento <input>.

    $files = $request->getUploadedFiles();

Cada objeto na matriz em $files é uma instância \Psr\Http\Message\UploadedFileInterface e é compatível com os seguintes métodos:

- getStream()
- moveTo($targetPath)
- getSize()
- getError()
- getClientFilename()
- getClientMediaType()

[Veja como fazer upload de arquivos]().

<br>

### Content Type
Você pode buscar o Content Type com o método `getContentType()`. Isso retorna o valor do Content-Type do cabeçalho fornecido pelo cliente HTTP.

    $contentType = $request->getContentType();

<br>

### Media Type
Você pode não querer o Content-Type completo . E se, em vez disso, você quer apenas o tipo de mídia? Você pode 
buscar o tipo de mídia com o método `getMediaType()`.

    $mediaType = $request->getMediaType();

<br>

### Character Set
Para recuperar o Charset de um Request use:

    $charset = $request->getContentCharset();

<br>

### Content Length
Para recuperar o tamanho de um Request use:

    $length = $request->getContentLength();
