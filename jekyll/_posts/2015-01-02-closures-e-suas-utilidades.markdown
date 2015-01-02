---
layout: post
title:  "Closures e suas utilidades"
date:   2015-01-02 03:00:00
categories: Javascript
---

Existem algumas maneiras de evitar que variáveis sejam declaradas no escopo global (ou no objeto `window`), uma delas é o uso de closures.

No javascript, uma closure é criada quando você cria uma função, tudo dentro dessa função está privado para uso nessa função, por exemplo:

{% highlight javascript %}
function closure() {
  var minhaLocal = true;
}

console.log(minhaLocal); // ReferenceError: minhaLocal is not defined
{% endhighlight %}

Algo interessante sobre as closures é que ela tem acesso a todos os ambientes criados **acima** dela, por exemplo, se uma função tiver dentro de outra função, a função de dentro vai ter acesso a todas as variáveis criadas na função de fora, por exemplo:

{% highlight javascript %}
function closure() {
  var var1 = true;
  function closure2() {
    console.log(var1); // true

    var var2 = true;
  }

  console.log(var2); // ReferenceError: var2 is not defined
}
{% endhighlight %}

E esse comportamento é o mesmo para o escopo global, tudo dentro de `window` pode ser acessado dentro de qualquer função.

É muito útil você usar este tipo de técnica quando você quer fazer algum tipo de lógica que involva uma variável temporária, que será usada uma única vez.

É muito importante que você sempre declare suas variáveis com a palavra-chave `var`, pois todas as variáveis declaradas sem o uso de `var` vao automaticamente para o escopo global:

{% highlight javascript %}
function closure() {
  minhaGlobal = true;
}

console.log(minhaGlobal); // true
{% endhighlight %}

Em breve ensinarei técnicas para modularizar sua aplicação que usaremos muito o conceito de closures, mantendo seu cógido bem organizado.
