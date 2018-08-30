# Sobre Gangoy Framework
[![Latest Stable Version](https://poser.pugx.org/tjgazel/gangoy-framework/v/stable)](https://packagist.org/packages/tjgazel/gangoy-framework)
[![Total Downloads](https://poser.pugx.org/tjgazel/gangoy-framework/downloads)](https://packagist.org/packages/tjgazel/gangoy-framework)
[![License](https://poser.pugx.org/tjgazel/gangoy-framework/license)](https://packagist.org/packages/tjgazel/gangoy-framework)

**Gangoy** é um framework PHP desenvolvido com um conjunto de bibliotecas bem conhecidas na comunidade PHP e segue as orientações das PSRs 
fornecidas pelo [**PHP-FIG**](http://www.php-fig.org/ "PHP-FIG").

Usamos o conceito de Serviços/Container, tornando fácil adicionar novas funcionalidades ou desativar alguma que já vem como padrão.

Foi projetado para que o desenvolvedor tenha em mãos recursos básicos/excenciais que são comuns em qualquer projeto Web,
ficando assim fácil e rápido iniciar um novo projeto seguindo boas práticas e padrões de desenvolvimento.

> Veja mais sobre as bibliotecas usadas na seção [**Agradecimentos**](#agradecimentos).

<br>

## Site do desenvolvedor
[http://www.tjgweb.com.br](http://www.tjgweb.com.br "TJG Web")

## Requisitos de sistema
- Web server com URL rewriting ativo.
- PHP 7.1 ou superior.

<br>

## Instalação
Utilizando [Composer](https://getcomposer.org/) Create-Project

    composer create-project tjgazel/gangoy-framework nome_projeto

Acesse com o terminal até o diretório onde foi criado o projeto e digite o seguinte comando para iniciar o servidor de desenvolvimento.

	php gangoy server
	 
Abra o navegador e acesse `localhost:8080`

<br>

<a name="agradecimentos"></a>
## Agradecimentos
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
