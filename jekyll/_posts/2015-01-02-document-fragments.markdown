---
layout: post
title:  "Document Fragments"
date:   2015-01-02 08:00:00
categories: Javascript
---

Quando trabalhando diretamente com o DOM você se depara diversas vezes em ter que incluir elementos dinamicos, e existem várias formas de fazer isso, uma delas é usando strings.

{% highlight javascript %}
var myNewElements, i = 0;

while (i < 200) {
  myNewElements += '<li>' + i + '</li>';
  i += 1;
}

document.body.innerHTML += myNewElements;
{% endhighlight %}

Mas existe uma maneira menos custoza para o browser de fazer isso, criando nodes do DOM, atribui-los a variaveis e em seguida inclui-los no DOM:

{% highlight javascript %}
var i = 0;

while (i < 200) {
  var newElement = document.createElement('li');
  newElement.innerText = i;

  document.body.appendChild(newElement);
  i += 1;
}
{% endhighlight %}

Criando um node do DOM faz com que o processo seja muito mais rápido, pois o browser não precisa parsear (transformar) a string que você passou em um elemento html real (node).

Mas dessa maneira você está acessando o DOM muitas vezes, e isso é uma má prática devido aos Repaints e Reflows que o browser precisa processar quando algo acontece no DOM. Quanto mais elementos você tiver, mais demorado vai ser esse processo, e com isso podemos usar o `fragments`!

`fragments` é basicamente um array de nodes do DOM, e com ele conseguimos usar o appendChild uma única vez, vamos ver como funciona:

{% highlight javascript %}
var i = 0, fragments = document.createDocumentFragment();

while (i < 200) {
  var newElement = document.createElement('li');
  newElement.innerText = i;

  fragments.appendChild(newElement);
  i += 1;
}

document.body.appendChild(fragments);
{% endhighlight %}

"Espera, estamos usando o appendChild no fragments, isso não da na mesma que usar o appendChild no body?"

Bem observado, mas não... O fragments seria um espaço na memória onde os elementos estão sendo incluidos, esse processo é bem mais rápido do que você incluir 200 vezes o node no DOM.

Observação: o uso de Fragments é recomendado quando você precisa mexer em muitos elementos no DOM, quando vc só vai incluir poucos elementos ele não se torna tão necessário assim.
