---
layout: post
title:  "Como funciona o call e o apply"
date:   2015-01-02 12:00:00
categories: Javascript
---

No javascript existem 2 métodos mt semelhates, são o `call` e o `apply`.

Eles fazem basicamente a mesma coisa, só que o `call` aceita um número qualquer de parametros, e o apply só aceita 2, um **objeto** e um **array**.

Todas as funções tem uma variável chamada `this`, que a ponta para o próprio objeto da função, ou no caso de uma constructor, aponta para sua instancia, o `apply` e o `call` servem para você chamar estas funções mudando o valor do `this`.

O primeiro parâmetro de `call` e `apply` muda o valor do `this`, no caso do `call`, os próximos parâmetros são os parâmetros da função que você está chamando `call(novoThis, param1, param2)`, e no caso do apply, o primeiro parâmetro também muda o valor de `this`, e o segundo é um array, que são os parâmetros da função que você está chamando `apply(novoThis, [param1, param2])`

Vamos ver um exemplo:

{% highlight javascript %}
var obj = {
  metodo: function() {
    return this;
  }
}

obj.metodo(); // retorna o próprio "obj"

var novoObj = {};
obj.metodo.call(novoObj); // retorna o "novoObj"
{% endhighlight %}

Então quando usamos o `call` no `obj.metodo`, nós trocamos o valor de this para um objeto diferente.

Isso pode ser útil para muitas coisas:

Transformar a variavel `arguments` em um Array

Toda função tem uma variável (como o `this`) chamada `arguments`, ela é um ArrayLike, significa que ela tem a estrutura de um Array, se parece com um Array, mas não tem os métodos do Array...

Muitas vezes precisamos tratar essa variável com métodos como push ou pop, mas por ser um ArrayLike ela não possui esses métodos, podemos transforma-la em um Array da seguinte forma:

{% highlight javascript %}
function teste() {
  var args = Array.prototype.slice.call(arguments);

  args.push('yeah');

  return args;
}

teste(1, 2, 3); // [1, 2, 3, 'yeah']
{% endhighlight %}

Basicamente nós usamos um método do objeto `Array` (nativo do javascript), e chamamos ele mudando o valor de `this`, o slice quando usado sem passar parâmetros, retorna o próprio Array, e internamente o javascript converteu o ArrayLike para um Array mesmo, assim podendo usar os métodos de Arrays na variavel `args`

Outra grande utilidade é reaproveitar métodos de outros objetos, segue o exemplo:

{% highlight javascript %}
var obj = {
  title: 'Título maneiro',
  sayTitle: function() {
    return 'Este é o título: ' + this.title
  }
};

obj.sayTitle(); // Este é o título: Título maneiro

novoObj = {
  title: 'Outro título maneiro'
}

obj.sayTitle.call(novoObj); // Este é o título: Outro título maneiro
{% endhighlight %}

Nesse exemplo usamos o método `sayTitle` de `obj`, passando o `novoObj` como o valor de `this`, reaproveitando assim o método que já existia.
