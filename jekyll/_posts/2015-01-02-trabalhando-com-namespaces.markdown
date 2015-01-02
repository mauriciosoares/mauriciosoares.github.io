---
layout: post
title:  "Trabalhando com namespaces"
date:   2015-01-02 02:00:00
categories: Javascript
---

Como disse em um post anterior, uma má prática é atribuir muitas variáveis ao escopo global, como por exemplo:

{% highlight javascript %}
var contador = 0,
  checker = false,
  button1 = $('button.bt1'),
  button2 = $('button.bt2');
{% endhighlight %}

Nesse caso todas essas variáveis são globais, e uma maneira de diminuir isso é usando um conceito famoso em várias linguagens chamado de **namespaces**.

Esse conceito no Javascript é aplicado com a atribuição de vários objetos dentro de um único objeto, por exemplo:

{% highlight javascript %}
var App = {};

App.contador = 0;
App.checker = false;
App.button1 = $('button.bt1');
App.button2 = $('button.bt2');
{% endhighlight %}

Dessa maneira a única variável declarada no escopo global (ou também podemos dizer no objeto `window`) seria a `App`.

Dessa maneira é muito fácil manter um padrão em sua aplicação, como por exemplo atribuir determinadas variáveis em um objetos relativos a página que você estiver mexendo:

{% highlight javascript %}
App.Login = {};
App.Login.form = $('.login form');

App.Contato = {};
App.Contato.form = $('.contato form');
{% endhighlight %}

Assim não precisamos criar variáveis como `loginForm` e `contatoForm`, deixando o escopo global sempre limpo com uma única variavel chamada `App`.

Um problema (facilmente contornado) com namespaces no Javascript é que você também pode sem querer deletar tudo o que você fez em uma variável, principalmente se você estiver trabalhando com multiplos arquivos.

{% highlight javascript %}
// login.js
var App = {};
App.Login = {};

// contato.js
// var App foi declarada novamente
var App = {};
App.Contato = {};
{% endhighlight %}

Dessa forma, deletamos sem querer tudo o que foi feito em `App.Login`, mas uma maneira bem simples de evitar esse erro seria verificando se a variável `App` já existe, caso contrário, você cria um objeto vazio:

{% highlight javascript %}
// login.js
var App = App || {};
App.Login = {};

// contato.js
var App = App || {};
App.Contato = {};
{% endhighlight %}

Com um simples <a href="https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Conditional_Operator" target="_blank">ternário</a> podemos verificar se a variável `App` já existe, se existir, atribuimos tudo o que já foi feito nela, caso contrário criamos um objeto vazio.

Parece desnecessário fazer esse tipo de verificação, principalmente quando você controla a ordem em que os arquivos serão chamados no seu html, mas quando você não sabe qual vai ser a ordem que os arquivos serão chamados (quando usamos alguma ferramenta para concatenar os arquivos), essa verificação se torna extremamente útil.
