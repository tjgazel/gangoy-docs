# Sobre Gangoy Framework
[![Latest Stable Version](https://poser.pugx.org/tjg/microframework/v/stable)](https://packagist.org/packages/tjg/microframework)
[![Latest Unstable Version](https://poser.pugx.org/tjg/microframework/v/unstable)](https://packagist.org/packages/tjg/microframework)
[![Total Downloads](https://poser.pugx.org/tjg/microframework/downloads)](https://packagist.org/packages/tjg/microframework)
[![License](https://poser.pugx.org/tjg/microframework/license)](https://packagist.org/packages/tjg/microframework)

**Gangoy** é um framework PHP desenvolvido sobre um conjunto de bibliotecas bem conhecidas na comunidade PHP. Sua base foi feita sobre o 
esqueleto do [**Slim 3**](https://www.slimframework.com/ "Slim Framework") e segue as orientações das PSRs 
fornecidas pelo [**PHP-FIG**](http://www.php-fig.org/ "PHP-FIG").

Usamos o conceito de Serviços/Container, tornando fácil adicionar novas funcionalidades ou desativar alguma que já vem como padrão.

Foi projetado para que o desenvolvedor tenha em mãos recursos básicos/excenciais que são comuns em qualquer projeto Web,
ficando assim fácil e rápido iniciar um novo projeto seguindo boas práticas e padrões de desenvolvimento.

> Veja mais sobre as bibliotecas usadas na seção [**Agradecimentos**](#agradecimentos).

<br>

## Site do desenvolvedor
[http://www.tjgweb.com.br](http://www.tjgweb.com.br "TJG Web")

## Vídeo-aulas sobre o Framework
[Acesse nosso canal no YouTube](https://www.youtube.com/tjgweb)

<br>

# Requisitos de sistema
- Web server with URL rewriting
- PHP 5.6.9 or newer

<br>

# Instalação
Utilizando [Composer](https://getcomposer.org/) Create-Project

    composer create-project tjg/gangoy seu_projeto

Navegue com o terminal até o diretório raiz do projeto e digite o seguinte comando para iniciar o servidor.

	php gangoy server
	 
Abra o navegador e acesse `localhost:8080`

<br>

<a name="agradecimentos"></a>
# Agradecimentos
Agradecemos a todos os desenvolvedores das bibliotecas citadas abaixo. Atravez delas foi possível chegarmos a este trabalho.

- [Slim Framework](https://www.slimframework.com/)
- [Slim Framework Flash Messages](https://github.com/slimphp/Slim-Flash)
- [Slim Framework CSRF Protection](https://github.com/slimphp/Slim-Csrf)
- [Slim Validation](https://github.com/awurth/slim-validation) / [Respect Validation](https://github.com/Respect/Validation)
- [Slim Framework Twig View](https://github.com/slimphp/Twig-View)
- [Twig](https://twig.symfony.com/doc/1.x/)
- [Laravel - Illuminate Database package](https://packagist.org/packages/illuminate/database)
- [Phinx](https://phinx.org/)
- [Symfony console](https://github.com/symfony/console)
- [PHP dotenv](https://github.com/vlucas/phpdotenv)


<br>

# License
The `Gangoy Framework` is open-sourced software licensed under the [MIT license](http://opensource.org/licenses/MIT)
