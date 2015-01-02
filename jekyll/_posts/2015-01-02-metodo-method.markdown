---
layout: post
title:  "Método Method"
date:   2015-01-02 16:00:00
categories: Javascript
---

Como falamos [anteriormente](http://blog.herebecoders.com/js-prototypes/), `prototypes` são usados para declararmos metodos e propriedades reutilizáveis em todas as instancias de uma função construtora no Javascript, a sintaxe é a seguinte:

{% highlight javascript %}
var Person = function(name) {
  this.name = name;
}

Person.prototype.getName = function() {
  return this.name;
}

Person.prototype.setName = function(name) {
  this.name = name;
}
{% endhighlight %}

Essa técnica é muito útil, e totalmente recomendável, mas alguns programadores acham meio repetitivo ter que sempre escrever `Person.prototype` toda vez q vamos criar um novo método, existe uma maneira de melhorar isso.

Podemos expandir o objeto nativo `Function`, e criar um método chamado `method`, vamos ver como funcionará a implementação antes de criar este método:

{% highlight javascript %}
var Person = function(name) {
  this.name = name;
}.method('getName', function() {
  return this.name;
}).method('setName', function(name) {
  this.name = name;
});
{% endhighlight %}

O que estamos fazendo aqui é chamado de encadeamento, muito usado no **jQuery**:

{% highlight javascript %}
$('.el').hide().show().css('display', 'none');
{% endhighlight %}

Esse encadeamento é possível, pois sempre retornamos uma referência do próprio objeto quando terminamos um método, podendo assim atribuir varios métodos um atrás do outro, escreverei sobre encadeamento em um post futuro...

Resumindo, criamos os métodos `getName` e `setName` no `prototype` de `Person`, exatamente igual a implementação anterior.

Voltando ao `method`, vamos fazer sua implementação e explica-la:

{% highlight javascript %}
if(!Function.prototype.method) {
  Function.prototype.method = function(name, implementation) {
    this.prototype[name] = implementation;
    return this;
  }
}
{% endhighlight %}

6 linhas de código, você provavelmente achou que seria mais não é mesmo? vamos ver como funciona...

Em `if(!Function.prototype.method)`, verificamos por segurança se esse método já não existe... Isso é somente uma boa prática, não interfere de fato como o `method` funciona.

Em `Function.prototype.method = function(name, implementation)`, atribuimos uma função chamada `method` ao objeto `Function`, assim todas as funções terão acesso a esse método, recebemos 2 parametros, o name que queremos atribuir ao prototype da função, e sua respectiva implementação.

Em `this.prototype[name] = implementation;` no index da variavel `name`, colocamos a implementação que vem no segundo parâmetro, seria o mesmo que fazer o seguinte: `this.prototype.getName = function() { // implementação }`

E por fim retornamos o `this`, podendo encadear um `method` atras do outro.

Para ser sincero não uso essa metodologia em meus projetos diários, mas é sempre bom descobrir jeitos diferentes de fazer implementações, pode ser que seja útil para muitos, e talvez para mim também algum dia.
