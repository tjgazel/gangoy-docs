# Frontend
Gangoy oferece uma estrutura bem fácil para desenvolvimento de seu frontend. Por padrão nós encorajamos o uso de templete views com Twig que já está previamente configurado nas estruturas de módulos, mas como tudo em nosso sistema você consegue alterar e usar facilmente qualquer outro recurso de sua preferência.<br>

O uso de componentes Vue.js também estão presentes em nossa estrutura  e previamente configurados graças aos recursos do [Laravel Mix](#assets) que fornece uma API fluente para definir a criação do [Webpack](https://webpack.js.org/) para seu aplicativo usando vários pré-processadores CSS e JavaScript comuns sem nenhuma dor de cabeça.

<br>

## Twig Templates


<br>

## <a name="assets"></a>Compilando Assets
Podemos facilmente gerenciar as dependências do frontend usando [Laravel Mix](https://laravel.com/docs/5.6/mix) que fornece uma API fluente para definir a criação do [Webpack](https://webpack.js.org/) para seu aplicativo usando vários pré-processadores CSS e JavaScript comuns. Exemplo:

```javascript
// webpack.mix.js
let mix = require('laravel-mix');

mix.js('resources/js/app.js', 'public/js');
   .sass('resources/sass/app.scss', 'public/css');
   .copyDirectory('resources/images', 'public/images');

```
Se você já se sentiu confuso e sobrecarregado sobre começar a usar o Webpack e a compilação de recursos, vai adorar o Laravel Mix. No entanto, você não é obrigado a usá-lo durante o desenvolvimento de seu aplicativo. Naturalmente, você está livre para usar qualquer ferramenta de pipeline para Assets que desejar ou até mesmo nenhuma. O arquivo de configuração do Laravel Mix está dentro do diretório raiz do seu projeto `webpack.mix.js`.

<br>

### Instalação e Configuração Laravel Mix
- Antes de usar o Laravel Mix, primeiro você deve garantir que o [Node.js]() e o NPM estejam instalados em sua máquina. Digite os comandos abaixo, se houver retorno da versão sua instalação está ok, caso contrário você pode usar os instaladores do Node.js em sua [página de downloads](https://nodejs.org/en/download/).

    ```
    node -v
    npm -v
    ```

- O último passo é instalar o Laravel Mix. Dentro do diretório raiz do projeto você vai encontrar o arquivo `package.json`. O arquivo padrão inclui tudo que você precisa para começar desenvolver seu frontend, incluindo o Laravel Mix. Pense nisso como seu arquivo que define as dependências do frontend assim como o composer define suas depêndencia do PHP. Fique livre para remover ou adicionar a seu gosto.

    Para instalar use o comando:

    ```
    npm install
    ```

<br>

### Utilização do Laravel Mix
Laravel Mix é uma camada de configuração do Webpack , portanto, para executar suas tarefas, você só precisa executar um dos scripts do NPM incluído no arquivo `package.json` padrão da instalação Gangoy.

```
// Os arquivos serão minificados/otimizados.
npm run prod

// Os arquivos não serão minificados/otimizados para depuração no desenvolvimento
npm run dev

```
**Escutando alterações nos arquivos** <br>
O `watch` comando continuará em execução no seu terminal e observará todos os arquivos relevantes para alterações. O Webpack recompilará automaticamente seus assets quando detectar uma alteração:

```
npm run watch
```
Por vezes, em certos ambientes, o Webpack não consegue atualizar automaticamente quando seus arquivos são alterados. Se este for o caso use o comando alternativo `watch-poll`.
```
npm run watch-poll
```

<br>

>Essas são apenas informações básicas sobre essa fantástica ferramenta. Veja todos os detalhes na [documentação oficial do Laravel Mix](https://laravel.com/docs/5.6/mix).