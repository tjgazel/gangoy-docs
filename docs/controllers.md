# Controllers
Controllers podem ser criados rapidamente através dos [Comandos de console](console.md).<br>
Os controllers da aplicação devem ser criados dentro do diretório `Controllers` dos seus respectivos módulos e extender a classe 
`Gangoy\Core\Http\Controller\AbstractController` ou `Gangoy\Core\Http\Controller\AbstractApiController` que são mais indicadas para criação de API Restful. 

```
<?php

namespace App\Controllers;

use Gangoy\Core\Http\Controller\AbstractController;

class DefaultController extends AbstractController
{
   public function index()
   {
       //
   }
}
```

Então configure suas rotas para trabalhar com seus controllers.

```
$app->get('/', ['\App\Controllers\DefaultController', 'index'])->name('home');
```

Note que seu controller deve ser passado como segundo argumento do método da rota em formato de array usando o namespace completo do controller e a ação correspondente 
que ele deve tomar para a referida rota.

<br>

## Injeção de dependências
O [Container de Dependências](container.md) resolve automaticamente qualquer dependência que você precise utilizar nos métodos do controller desde que essa dependência tenha sido registrada no arquivo `config/dependencies.php` de qualquer módulo ativo e que seja passado sua assinatura. Veja o exemplo de como obter a instância da classe [Response](psr_7_response.md):

```
<?php

namespace App\Controllers;

use Gangoy\Core\Http\Controller\AbstractController;
use Psr\Http\Message\ResponseInterface as Response;

class DefaultController extends AbstractController
{
   public function index(Response $response)
   {
        return $response->getBody()->write('<h1>Hello  World!</h1>');
   }
}
```

<br>

## Métodos e atributos de herança das classes extendidas.
Seus controllers recebem como herança alguns métodos e atributos da classe extendida `AbstractController` ou `AbstractApiContoller` para facilitar acesso aos recursos/serviços 
disponíveis no [Container de Dependências](container.md). 

<br>

### **__construct**
Por padrão a aplicação injeta automaticamente o [Container de Dependências](container.md) no contrutor de todo controller. Caso você precise implementar alguma lógica no construtor deve-se lembrar de adicionar o mesmo como atributo e passar para a classe pai. Exemplo:

```
<?php

namespace App\Controllers;

use Gangoy\Core\Http\Controller\AbstractController;
use Psr\Container\ContainerInterface;

class DefaultController extends AbstractController
{
    public function __construct(ContainerInterface $container)
    {
        parent::__construct($container);
        // 
    }
}
```

<br>

### **Atributos**

- **$this->container** - Instância do [Container de Dependências](container.md).

- **$this->logger** - Instância do Monolog\Logger para gravar [logs](logs.md) das atividades do sistema.

- **$this->validator** - Instância do serviço de [validação](validação.md) de dados. 

- **$this->db** - Instância do Illuminate Database responsável pela manipulação do [banco de dados](database.md).

- **$this->events** - Instância do Zend EventManager, responsável pela manipulação de [eventos](eventos.md).

- **$this->twig** - Instância do Twig, responsável por renderizar as [Views](views.md).

- **$this->flash** - Instância do servico de [Flash Message](flash_messages.md).


**Nota:** em `AbstractApiController` não existe os atributos `$this->twig` e `$this->flash` por se tratar de um controller específico para Apis Restful.

<br>

### **Métodos**

- **$this->redirect(Response $response, string $url, int $status = 307)**<br>
Este método abstrai a forma um pouco verbosa de redirecionamento da [Response](psr_7_response.md) PSR 7.
    - 1º argumento é o objeto de resposta PSR 7.<br>
    - 2ª argumento é a url de redirecionamento.<br>
    - 3º argumento é o http status code opcional.

- **$this->pathFor(string $name, array $data = [], array $queryParams = [])**<br>
Método que retorna a url da rota de acordo com o seu nome. Veja como dar nome às [rotas](rotas.md).
    - 1º argumento é o nome da rota. Ex: 'auth.getLogin'.<br>
    - 2º argumento opcional. Array com as chaves e valores dos parâmetros da rota caso exista.
    - 3º argumento opcional. Array com chaves e valores caso queira passar uma query params.