---
layout: post
title:  "Grunt, baby steps, parte 1"
date:   2015-01-02 14:00:00
categories: Javascript
---

Antes de mais nada, infelizmente terei que deixar usuários de Windows um pouco na mão nesse post, esse post é destinado mais para usuários de Linux/Mac, pois vai involver o Terminal. Mas tentarei deixar referências para pelo menos guia-los como fazer os procedimentos no Windows. Sempre que eu involver o **terminal**, no Windows será equivalente ao **CMD**.

Separei este tutorial em 2 posts, pois ele vai ficar meio extenso, aqui vou ensinar como instalar tudo que precisamos para podermos rodar o Grunt.

Grunt tem sido uma ferramenta bastante "popstar" no Front-End hoje em dia, e tem seu mérito, pois é uma ferramenta fantástica.

Grunt é um automatizador de tarefas escrito em NodeJS, basicamente ele pega essas tarefas chatas de minificar, concatenar, jshint e afins, e as tornam muito mais rápidas e simples.

Vou ensinar aqui o básico de Grunt, para que você já comece a usar em seu projeto.

### Instalar o NodeJS

Como disse, Grunt foi feito com Node, portanto precisamos dele instalado para podermos usa-lo.

* Mac: Apenas baixe no site do [NodeJS](http://nodejs.org/), e instale em seu computador.

* Linux Ubuntu: Dependendo da versão do seu ubunto, seu apt-get pode ter repositórios meio antigos, portanto rode o seguinte:

{% highlight html %}
sudo add-apt-repository ppa:chris-lea/node.js
sudo apt-get update
sudo apt-get install python-software-properties python g++ make nodejs
{% endhighlight %}

Se tudo tiver dado certo, você poderá abrir seu terminal e rodar `node -v` que ele aparecerá a versão atual do NodeJS (que pode variar de acordo da data que você está vendo este post);

* Windows: Acredito que o mesmo procedimento de instalar no Mac resolva.

### Instalar o grunt-cli

Com o NodeJS instalado, temos acesso a um gerenciador de pacotes chamado **Node Package Manager** (popularmente conhecido como **npm**), com eles instalaremos o comando `grunt`, para que possamos rodar direto do terminal:

* Este procedimento é igual para ambos Mac e Linux (Para Windows também deve funcionar).

{% highlight html %}
sudo npm install -g grunt-cli
{% endhighlight %}

Com isso instalaremos Grunt de forma global na nossa máquina, podendo rodar o comando `grunt` dentro de qualquer pasta.

### package.json

Este é um arquivo de configurações padrões do Node, e preciamos dele para podermos instalar e salvar nossas tarefas do Grunt.

Para gerar esse arquivo rode `npm init` no terminal, e siga os passos que ele informar (Nome do projeto, versão, etc...)

### Instalando o Grunt no projeto

Depois de ter criado o arquivo package.json, podemos instalar o Grunt rodando:

{% highlight html %}
npm install grunt --save-dev
{% endhighlight %}

Dessa forma o modulo do Grunt é instalado, e estamos prontos para instalar nossas tasks, e configurar nosso arquivo que vai rodar essas tasks!

Continue esse tutorial [nesse post](http://blog.herebecoders.com/js-grunt-baby-steps-parte-2/)
