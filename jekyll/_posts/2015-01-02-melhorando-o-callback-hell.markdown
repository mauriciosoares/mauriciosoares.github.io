---
layout: post
title:  "Melhorando o callback hell"
date:   2015-01-02 23:00:00
categories: Javascript
---

Como eu melhorei o processo de ajax dinâmicos

Creio que todo desenvolvedor front end ja passou pelo processo de lidar com ajax + jQuery... e sabe que isso pode virar uma bagunça sem fim...

Um simples e inofensivo ajax:

{% highlight javascript %}
$.ajax({
  url: 'http://myurl.com',
  type: 'POST',
  success: function() {
    // callback
  }
})
{% endhighlight %}

Simples e objetivo, mas vamos supor que nosso código precise de mais de um ajax para validar um dado?

{% highlight javascript %}
$.ajax({
  url: 'http://myurl.com',
  type: 'POST',
  success: function() {
    $.ajax({
      url: 'http://myurl.com',
      type: 'POST',
      success: function() {
        // callback
      }
    })
  }
})
{% endhighlight %}

Mas podemos melhorar este processo usando promises... vejamos como...

{% highlight javascript %}
var req1 = $.ajax({
  // confs
});

var req2 = $.ajax({
  // confs
});

$.when(req1, req2).done(function(params1, params2) {
  // callback
});

{% endhighlight %}

Fizemos com que as variáveis `req1` e `req2` recebesse a promise de cada requisição, e com o método $.when do jQuery fazemos um callback ser executado somente quando as 2 requisições estão prontas. YAY!


Mas como meu mundo não é perfeito, eu passei por um desafio um pouco maior...

1 - Os ajaxes que eu ia fazer já estavam implementados de uma maneira que não dava suporte a promises (fuck)
2 - O número de requisições que eu ia fazer era dinâmica (mas padronizada), de maneira que poderia ser 1 ou 3 de uma vez.

Meu cenario era o seguinte:

{% highlight javascript %}
// cada metodo dentro de "service" era um ajax diferente
service.customAjax(function() {});
service.customAjax2(function() {});
service.customAjax3(function() {});

// depois que todos os ajaxes estavam prontos eu tinha que tratar os dados
{% endhighlight %}

Antes de mais nada, peguei todos os métodos ajax que eu precisava usar, e encapsulei cada um em um método meu, e criei uma estrutura que simulava as promises

{% highlight javascript %}
function metodo1() {
  var deferred = $.Deferred();

  service.customAjax(function(data) {
    deferred.resolve(data);
  });

  return deferred.promise();
}
{% endhighlight %}

Calma calma, vamos por partes...

A variável `deferred` nos ajuda a simular o efeito de uma promise... retornando `deferred.promise()` eu posso usar o `done` ou `when` do jQuery para tratar multiplos ajaxes

Quando usamos `deferred.resolve(data)` dentro do resultado do ajax, nós estamos falando pra promise que retornamos que aquela promise foi concluida, e o parâmetro passado será o valor que essa promise retornará.

Portanto a coisa ficou parecida com isso:

{% highlight javascript %}
var methods = {};
methods.metodo1() {
  var deferred = $.Deferred();

  service.customAjax(function(data) {
    deferred.resolve(data);
  });

  return deferred.promise();
}

methods.metodo2() {
  var deferred = $.Deferred();

  service.customAjax2(function(data) {
    deferred.resolve(data);
  });

  return deferred.promise();
}

methods.metodo3() {
  var deferred = $.Deferred();

  service.customAjax3(function(data) {
    deferred.resolve(data);
  });

  return deferred.promise();
}
{% endhighlight %}

Essa parte infelizmente tive que duplicar o código, mas como o numero limite de ajax era fixo, não vi muito problema.

Agora precisamos fazer uma maneira de pegar o callback tanto de 1 requisição, quanto das 3...

Primeiro criaremos um array com as promises de cada callback que quisermos usar... esse array será alimentado com alguma regra de negócio da sua aplicação...

Depois usaremos o [apply](http://mauriciosoares.co/blog/javascript/2015/01/02/como-funciona-o-call-e-o-apply.html) para chamar o método `$.when`, para termos quantas requisições forem necessárias

E no callback de `$.when` usaremos a variável [arguments](http://mauriciosoares.co/blog/javascript/2015/01/02/dicas-sobre-javascript.html) para pegarmos o retorno de cada callback.

Como ficaria então um código com essa implementação

{% highlight javascript %}
// Lógica para criar o array,
// isso vai depender de qual é sua
// regra de negocio
var methodsToCall = ['metodo1', 'metodo2', 'metodo3'],
  promises = [];

for(var index in methodsToCall) {
  // inserimos no array "promises" a promise de cada ajax
  promises.push(methods[index]());
}

// depois de alimentarmos "promises", chamamods o $.when usando apply
$.when.apply(jQuery, promises).done(callback);

function callback() {
  // essa função só é chamada quando todas as 3 requisições forem concluídas

  for(var index in arguments) {
    item = arguments[index];

    // trata o valor de "item", que é o resultado de cada requisição
  }
};
{% endhighlight %}

Dessa maneira eu posso construir um array dinâmico com promises, e executa-los de uma vez só, sem precisar aninhar callback dentro de callback.

Novamente, isso só funciona direito se seus ajaxes forem padronizados, se os valores que vc for pegar derem para tratar da mesma maneira como um todo...

No meu caso, os ajaxes eram para validar se EMAIL e CPF eram válidos, portanto a resposta dos 2 eram iguais e eu conseguia trata-los da mesma maneira.

Esse exemplo não se aplica para todas as vezes que você for usar ajax :)
