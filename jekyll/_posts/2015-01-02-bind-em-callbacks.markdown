---
layout: post
title:  "Bind em callbacks"
date:   2015-01-02 13:00:00
categories: Javascript
---

Recentemente falamos sobre o [this](http://blog.herebecoders.com/js-this-em-funcoes-e-construtores/), de forma bem simples expliquei como esse objeto funciona, mas existe uma situação em que o `this` pode não se comportar da forma que você espera.

Antes de mais nada, precisamos entender um conceito da linguagem Javascript chamado de `callback`, que consiste em ser uma função que vai ser chamada assim que determinada ação é executada, por exemplo:

{% highlight javascript %}
[1, 2, 3].forEach(function(item) {
  console.log(item);
});

// log: 1
// log: 2
// log: 3
{% endhighlight %}

O construtor `Array` tem um método bem famoso chamado `forEach` (funciona da mesma forma que o $.forEach do jQuery), que itera sob o array, e retorna todos os valores do mesmo.

Notemos que ao usar o `forEach`, passamos uma função como primeiro argumento, esse argumento é o **callback** desse método do Array, que é executado em cada item.

Por um momento vamos voltar ao `this`.

Como sabemos, o `this` dentro de um método de um objeto, ou em uma instancia, aponta para o próprio objeto.

{% highlight javascript %}
var obj = {};

obj.metodo = function() {
  return this;
}

obj.metodo() // retorna o próprio "obj"
{% endhighlight %}

Mas esse comportamento é diferente dentro de callbacks... Quando usamos um callback, o `this` passa a apontar para o objeto `window`, e isso pode trazer alguams complicações, vamos usar como exemplo novamente o `forEach`.

{% highlight javascript %}
var obj = {};

obj.metodo = function() {
  [1, 2, 3].forEach(function() {
    console.log(this);
  });

  return this;
}

obj.metodo() // log 3 vezes "window", e retorna 1 vez o próprio objeto
{% endhighlight %}

Dentro do `callback` do `forEach`, o `this` retornou o objeto window, mas não necessariamente sobreescreveu o valor de `this`, quando acabou essa iteração, o `this` voltou a ser o próprio `obj`.

Por que isso pode ser um problema?

Suponhamos que queremos usar esse callback, mas ao mesmo tempo usar algum método do próprio objeto em que estamos?

{% highlight javascript %}
var obj = {};
obj.meuMetodo = function() {
  // some code
}

obj.metodo = function() {
  [1, 2, 3].forEach(function() {
    this.meuMetodo();
  });
}

obj.metodo() // Error, undefined is not a function
{% endhighlight %}

Teremos um erro pois o objeto `window` (o `this` dentro do callback de `forEach`) não tem o método `meuMetodo`.

Como podemos contornar esse erro?

Existe uma maneira muito usado entre programadores, que consiste em atribuir o valor de `this` a uma outra variável, comumente chamada de `that` ou `_this`.

{% highlight javascript %}
var obj = {};
obj.meuMetodo = function() {
  // some code
}

obj.metodo = function() {
  var that = this;
  [1, 2, 3].forEach(function() {
    that.meuMetodo();
  });
}

obj.metodo() // Executa normalmente
{% endhighlight %}

Funciona muito bem assim, mas temos uma variável declarada desnecessariamente, e temos que seguir um padrão de nomenclatura ai, pois um programador pode usar o `that`, mas outro programador começa a mexer no projeto e começa a usar o `_this`, ai teremos uma inconsistência no nosso código.

E finalmente (antes tarde do que nunca), podemos usar o `bind`!

O `bind` funciona de forma semelhante ao [call e o apply](http://blog.herebecoders.com/js-como-funciona-o-call-e-o-apply/), alterando o valor de `this` dentro de alguma função, mas ao invés de executar a função na hora em que o usamos, como faz o `call` e o `apply`, ele espera que a função em que o colocamos como callback o faça, como por exemplo:

{% highlight javascript %}
var obj = {};
obj.meuMetodo = function() {
  // some code
}

obj.metodo = function() {
  [1, 2, 3].forEach(function() {
    this.meuMetodo();
  }.bind(this));
}

obj.metodo() // Executa normalmente
{% endhighlight %}

Dessa maneira, de forma elegante, mantivemos o valor de `this` no callback de `forEach`, e a leitura do código ficou mais simples também.

Podemos usar o `bind` em funções já definidas também, vamos analisar o seguinte exemplo:

{% highlight javascript %}
var obj = {};
obj.meuMetodo = function() {
  console.log(this);
}

obj.metodo = function() {
  [1, 2, 3].forEach(this.meuMetodo);
}

obj.metodo() // log 3 vezes em window
{% endhighlight %}

Nesse caso, usamos o próprio metodo de `obj` como callback, mas como comportamento padrão, mesmo assim o valor de `this` foi mudado para `window`.

Isso foi algo que me confundio bastante, vamos analisar o seguite código:

{% highlight javascript %}
obj.meuMetodo(); // retorna o próprio obj

obj.metodo(); // retorna window toda vez que meuMetodo é chamado
{% endhighlight %}

Se chamamos o `meuMetodo` sem usa-lo como callback, o valor de `this` mantém, mas como vimos antes, dentro de um callback o valor de `this` muda.

Vamos consertar isso.

{% highlight javascript %}
var obj = {};
obj.meuMetodo = function() {
  console.log(this);
}

obj.metodo = function() {
  [1, 2, 3].forEach(this.meuMetodo.bind(this));
}

obj.metodo() // log no obj 3 vezes
{% endhighlight %}

Como vimos, o bind pode parecer um pouco confuso, mas nos ajuda muito quando queremos manter o valor de `this` em todos nossos métodos, mesmo em callbacks.
