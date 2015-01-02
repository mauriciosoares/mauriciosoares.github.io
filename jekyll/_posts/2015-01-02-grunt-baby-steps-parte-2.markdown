---
layout: post
title:  "Grunt, baby steps, parte 2"
date:   2015-01-02 15:00:00
categories: Javascript
---

Falamos no [post anterior](http://blog.herebecoders.com/js-grunt-baby-steps-parte-1/) sobre como instalarmos o Grunt em nossa máquina, agora que passamos por todo aquele processo vamos agora configura-lo e coloca-lo em ação!

### Instalando tasks

No Grunt não temos nenhuma task pré configurada, precisamos baixar as tasks que queremos usar (o que é bom, pois mantemos somente o que precisamos), para mantermos as coisas simples, vamos usar 2 tasks que eu uso em quase todos os meus projetos...

Usaremos então a task de concatenar, e a de jshint para qualidade de código.

Para instalarmos as tasks preciamos rodar os seguintes comandos no terminal:

{% highlight html %}
npm install grunt-contrib-concat --save-dev
npm install grunt-contrib-jshint --save-dev
{% endhighlight %}

Então usamos o `npm install`, escolhemos o nome da task, e em seguinda `--save-dev`, para salvarmos a task no package.json.

Uma lista de todas as tasks (e como usa-las) pode ser achado aqui: <a href="http://gruntjs.com/plugins" target="_blank">http://gruntjs.com/plugins</a>.

Nota: Geralmente as tasks que tem **contrib** no nome, é que foi criada pela equipe do Grunt.

### Gruntfile.js

Bom, agora que temos nossas tasks instaladas, vamos iniciar a configuração de nossas tasks...

No Grunt, existe um arquivo (que fica na raiz do seu projeto) chamado `Gruntfile.js`, nele configuramos como nossas tasks irão funcionar...

Vou colocar o arquivo configurado inteiro abaixo, e em tópicos vou explicar tudo o que acontece.

{% highlight javascript %}
// #1
module.exports = function(grunt) {

  // #2
  var tasks = [
    'grunt-contrib-jshint',
    'grunt-contrib-concat'
  ];

  // #3
  var config = {};

  // =============================================
  // jshint
  // #4
  config.jshint = {
    all: ['src/**/*.js']
  };

  // =============================================
  // concat
  // #5
  config.concat = {
    options: {
      banner: '/* Minified file.js */'
    },
    dist: {
      src: [
        'assets/js/main.js',
        'assets/js/home.js'
      ],
      dest: 'assets/js/app.js'
    }
  }

  // =============================================
  // config
  // #6
  grunt.initConfig(config);

  // Load all tasks
  // #7
  tasks.forEach(grunt.loadNpmTasks);

  // Tasks
  // #8
  grunt.registerTask('default', ['jshint', 'concat']);
};
{% endhighlight %}

Bom, vamos a explicação:

1 - Usamos o `module.exports` (um método usado no Node) para exportarmos a função de configuração do Grunt... Isso é um padrão usado para fazer plugins para o Node, não vou explicar a fundo aqui como ele funciona.

Lembrando que o parametro deve ser passado (Usamos o nome padrão `grunt`), pois ele é usado no escopo da função

2 - Criamos um Array com todas as tasks que instalamos... Nós precisamos declarar manualmente todas as tasks usadas, o Grunt não consegue detectar as que nós baixamos.

3 - Criamos um Objeto onde iremos configurar nossas tasks... Por uma questão de organização, prefiro separar esse objeto em várias linhas, fica mais fácil a leitura, poderiamos fazer algo desse tipo que funcionaria também:
{% highlight javascript %}
var config = {
  concat: {
    options: {},
    dist: {}
  },
  jshint: {
    all: {}
  }
}
{% endhighlight %}

Mas dessa forma acho que dificultaria umpouco a leitura, principalmente se tivermos muitas tasks em nosso Gruntfile.

4 - Configuramos a tarefa jshint, onde o index usado é o `all`, um Array onde vai varrer todos os arquivos que quisermos, usando o jshint neles... note que podemos especificar o nome dos arquivos, ou podemos usar o sinal `**` para que ele varra todas as pastas, e `*.js` para que ele varra todos os arquivos da extensão JS.

Outras opções podem ser achados no <a href="https://github.com/gruntjs/grunt-contrib-jshint" target="_blank">repositorio do plugin</a>.

5 - Configuramos a tarefa de concat, temos um indice no objeto chamado `banner`, nele colocamos um texto que vai aparecer no topo do nosso arquivo minificado.

Em `dist`, temos 2 indices, um chamado `src`, que é um Array onde colocamos todos os arquivos que queremos concatenar, e `dest`, uma string que vai ser onde o arquivo concatenado vai ficar.

Outras opções podem ser achados no <a href="https://github.com/gruntjs/grunt-contrib-concat" target="_blank">repositorio do plugin</a>.

6 - Em seguida pegamos todo esse objeto `config` que criamos e chamamos uma função do Grunt passando ele como parâmetro.

7 - carregamos todas as tasks que definimos no começo

Essa é uma forma diferente de carregar as tasks, já vi muitos lugares fazerem da seguinte forma:

{% highlight javascript %}
grunt.loadNpmTasks('grunt-contrib-concat');
grunt.loadNpmTasks('grunt-contrib-jshint');
{% endhighlight %}

Mas prefiro usar o forEach do próprio Javascript, faz com que as coisas sejam mais dinâmicas, sem a necessidade de escrever a função `loadNpmTasks` diversas vezes.

8 - Criamos uma tarefa padrão para o Grunt.

Ao fazer isso, podemos ir no terminal e simplesmente chamar `grunt`, assim ele chamará as tarefas que estiverem configuradas neste Array, em sequencia...

Se não tivessemos feito isso, precisariamos executar as tarefas uma a uma:

{% highlight javascript %}
grunt concat
grunt jshint
{% endhighlight %}

Dessa forma, se alguma tarefa falhar, as seguintes param automáticamente.

Bom, após toda essa configuração, basta ir no seu terminal e rodar `grunt`, que ele executará essa sequência de tarefas.

Lembrando que você deve estar na pasta do seu projeto.

Bom, esse é uma pequena prévia de como funciona o Grunt... Ele pode ser útil de muitas outras maneiras, existem muitos outros plugins extremamente úteis, que vou deixar listado abaixo, cada um tem seu jeito específico de configuração, mas não foge muito desse padrão visto acima:

* <a href="https://github.com/gruntjs/grunt-contrib-concat" target="_blank">Concatenação de arquivos</a> (usado nesse tutorial)
* <a href="https://github.com/gruntjs/grunt-contrib-jshint" target="_blank">JSHint</a> (usado nesse tutorial)
* <a href="https://github.com/gruntjs/grunt-contrib-concat" target="_blank">Minificação de arquivos</a>
* <a href="https://github.com/gruntjs/grunt-contrib-watch" target="_blank">Executa tarefas automaticamente assim que um arquivo é alterado</a>
* <a href="https://github.com/gruntjs/grunt-contrib-jasmine" target="_blank">Testes em Jasmine</a>
* E novamente a lista inteira de plugins disponíveis: <a href="http://gruntjs.com/plugins" target="_blank">http://gruntjs.com/plugins</a>
