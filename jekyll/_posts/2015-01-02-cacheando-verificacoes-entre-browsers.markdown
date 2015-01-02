---
layout: post
title:  "Cacheando verificações entre browsers"
date:   2015-01-02 05:00:00
categories: Javascript
---

Uma técina extremamente interessante que li no livro [Padrões Javascript](http://novatec.com.br/livros/padroesjavascript/) do Stoyan Stefanov é chamada de Sniffing.

Ela consiste em cachear verificações entre diferentes browsers, para que essas verificações não ocorram toda vez que um determinado método é chamado... vamos ver o exemplo:

Vamos supor que eu quero atribuir um evento em algum elemento (sem uso do jQuery), o problema é que a chamada pode mudar de browser para browser, portanto fariamos a seguinte verificação:

{% highlight javascript %}
function addEventListener(el, type, fn) {
  if(typeof window.addEventListener === 'function') {
    // browsers novos
      el.addEventListener(type, fn, false);
      el.removeEventListener(type, fn, false);

  } else if(typeof document.attachEvent === 'function') {
    // old IE
      el.attachEvent('on' + type, fn);
      el.detachEvent('on' + type, fn);

  } else {
    // super old browsers
      el['on' + type] = fn;
      el['on' + type] = null;
  }
}

addEventListener(getElementById('#myElement'), 'click', function() {
  // callback
});
{% endhighlight %}

Dessa maneira, sempre que você quiser atribuir algum evento a um elemento, ele sempre fará essa verificação para saber qual método é compativel com o browser atual.

Uma forma simples de contornar essas verificações desnecessárias, seria guardar o método certo em alguma variável, e a partir daí usar essa variavel, vamos ver o exemplo:

{% highlight javascript %}
  var Helpers = {
    addListener: null,
    removeListener: null
  };
  if(typeof window.addEventListener === 'function') {
    // actually browsers
    Helpers.addListener = function(el, type, fn) {
      el.addEventListener(type, fn, false);
    };

    Helpers.removeListener = function(el, type, fn) {
      el.removeEventListener(type, fn, false);
    };

  } else if(typeof document.attachEvent === 'function') {
    // old IE
    Helpers.addListener = function(el, type, fn) {
      el.attachEvent('on' + type, fn);
    };

    Helpers.removeListener = function(el, type, fn) {
      el.detachEvent('on' + type, fn);
    };

  } else {
    // damn that's old
    Helpers.addListener = function(el, type, fn) {
      el['on' + type] = fn;
    };

    Helpers.removeListener = function(el, type, fn) {
      el['on' + type] = null;
    };
  }

  Helpers.addListener(document.getElementById('#myElement'), 'click', function() {
    // callback
  });
{% endhighlight %}

A segunda maneira, mesmo sendo mais extensa, é muito menos custoza pra o browser, pois é verificado somente 1 vez qual método usar, e a partir daí usar o `Helpers.addListener` e o `Helpers.removeListener`.
