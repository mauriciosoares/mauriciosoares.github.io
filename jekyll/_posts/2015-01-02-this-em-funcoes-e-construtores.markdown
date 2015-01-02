---
layout: post
title:  "this em funções e construtores"
date:   2015-01-02 06:00:00
categories: Javascript
---

Um comportamento do Javascript é fazer com que o objeto `this` dentro de funções apontem para o objeto `window`.

{% highlight javascript %}
function teste() {
  return this;
}

teste(); // window
{% endhighlight %}

Isso acontece sempre que você chamar essa função da forma convencional, mas quando usamos funções como construtores esse comportamento é diferente...

Em muitas linguagens de programação, é possível o uso de classes construtoras, no Javascript não existem classes (pelo menos até o Ecmascript 5), mas podemos simular essa situação;

Vamos supor que tenhamos a classe Person, e queremos instanciar varias pessoas com essa classe, isso poderia ser adquirido da seguinte forma:

{% highlight javascript %}
function Person() {

}

var Weslley = new Person();
var Alan = new Person();
{% endhighlight %}

Dessa forma, **Weslley** e **Alan** receberão todos os métodos dentro de Person.

Para podermos atribuir métodos e propriedades as instancias de Person, podemos usar o objeto `this`;

{% highlight javascript %}
function Person(name) {
  this.name = name;
}

var weslley = new Person('Weslley');
weslley.name; // Weslley

var alan = new Person('Alan');
alan.name; // Alan
{% endhighlight %}

Dessa forma o objeto `this`, ao invés de apontar para o `window`, ele vai apontar para a propria variavel instanciada, no caso `alan` e `weslley`.

O mesmo pode ser feit ocom métodos.

{% highlight javascript %}
function Person(name) {
  this.name = name;
  this.sayName = function() {
    return 'Meu nome é ' + this.name;
  }
}

var weslley = new Person('Weslley');
weslley.sayName(); // Meu nome é Weslley

var alan = new Person('Alan');
alan.sayName(); // Meu nome é Alan
{% endhighlight %}

Mas temos um problema que não é bem visível nesse caso. O método `this.sayName` é exatamente igual para todas as instancias de Person, mas como ele é atribuido ao elemento instanciado (no caso `alan` e `weslley`) ele acaba sendo replicado todas as vezes que uma nova instancia é criada.

Isso não parece ser muito importante, mas imaginem uma classe que crie 1000 instancias dela mesma, e assim o método `this.sayName` é replicado 1000 vezes, ai as coisas começam a complicar...

E é ai que o `prototype` vem em ação! Leia o próximo post para saber umpouco mais sobre `prototype`.
