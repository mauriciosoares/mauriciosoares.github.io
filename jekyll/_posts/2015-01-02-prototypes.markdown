---
layout: post
title:  "Prototypes!"
date:   2015-01-02 07:00:00
categories: Javascript
---

Prototype são muito utilizados para criar métodos em funções construtoras, vamos dar o seguinte exemplo.

{% highlight javascript %}
function Person(name) {
  this.name = name;
  this.sayName = function() {
    return 'My name is ' + this.name;
  };
}
{% endhighlight %}

Dessa maneira, todas as instancias de Person vao replicar o método `sayName`, mas isso não é necessário, pois o comportamento desse método é igual para todos, podemos reescrever esse código usando `prototype`.

{% highlight javascript %}
function Person(name) {
  this.name = name;
}

Person.prototype.sayName = function() {
  return 'My name is ' + this.name;
}
{% endhighlight %}

Aumentamos umpouco as linhas de código, mas vamos interpretar o que aconteceu aqui.

Toda função tem um objeto `prototype` nela... Esse objeto pode ser acessado por todas as instancias dessa função (ou dessa constructor), e o `prototype` tem acesso ao objeto `this` da instancia, por esse motivo podemos acessar `this.name` em `Person.prototype.sayName`.

A maior vantagem disso é que tudo dentro de `prototype` pertence ao constructor `Person`, portanto ele não é replicado toda vez que uma instancia é criada. Mas sim a instancia acessa o `prototype` da constructor `Person`.
