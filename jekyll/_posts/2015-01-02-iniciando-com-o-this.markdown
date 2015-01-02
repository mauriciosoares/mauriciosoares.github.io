---
layout: post
title:  "Iniciando com this"
date:   2015-01-02 04:00:00
categories: Javascript
---

Algo que confude muitos desenvolvedores ao iniciar com Javascript é entender o conceito do `this`.

O `this` é usado para referenciar ao próprio objeto que está sendo usado no momento, facilitando assim passar valores dentre métodos do objeto, mas nada melhor do que ver na prática como funciona:

{% highlight javascript %}
var obj = {
  name: 'Mauricio',
  sayName: function() {
    return 'My name is ' + this.name;
  }
}

obj.sayName(); // My name is Mauricio
{% endhighlight %}

O uso de `this.name` seria semelhante a `obj.name` dentro desse próprio objeto.

Com o uso de this podemos criar propriedades para este objeto de forma dinâmica.

{% highlight javascript %}
var obj = {
  addName: function(newName) {
    this.name = newName;
  }
}

obj.addName('Mauricio');

console.log(obj.name); // Mauricio
{% endhighlight %}

O this também pode ser usado para referências do DOM, principalmente quando usado junto com eventos de click, mouseenter e afins...

{% highlight javascript %}
// suponhamos que exista uma div com um id #teste no DOM

var el = document.getElementById('teste');

el.addEventListener('click', function() {
  console.log(this)
});
{% endhighlight %}

Assim, sempre que clicarmos nessa div, nosso log retornara uma referência a própria div.

O motivo de isso ser útil é você poder trabalhar com essa referência sem precisar usar o `getElementById` toda vez que você precisar fazer algo nessa div, por exemplo:

{% highlight javascript %}
// suponhamos que exista uma div com um id #teste no DOM

var el = document.getElementById('teste');

el.addEventListener('click', function() {
  this.style.display = 'none';
});
{% endhighlight %}

Quando clicarmos nessa div novamente ela sumirá, pois temos a referência desse elemento dentro de `this`.

Em breve explicarei de forma simples como funciona o `this` dentro de funções.
