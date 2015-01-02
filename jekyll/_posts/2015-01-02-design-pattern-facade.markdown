---
layout: post
title:  "Design pattern Facade"
date:   2015-01-02 22:00:00
categories: Javascript
---

Existem muitos patterns para javascript, e hoje falarei umpouco sobre Facade.

No javascript, como em qualquer linguagem de programação, é sempre importante manter seus métodos pequenos e simples, mas ao contrário de muitas linguagens de programação, o javascript tem que se preocupar com muitos ambientes diferentes (Chrome, IE, Firefox, Safari, e todas suas subsequentes versões...).

Muitas vezes nos deparamos fazendo coisas do tipo:

{% highlight javascript %}
var app = {
  clickHandler: function(e) {
    if(e.preventDefault) {
      e.preventDefault();
    } else {
      e.returnValue = false;
    }

    // awesome code
  }
};
{% endhighlight %}

Nesse caso estamos cancelando o handler padrão do browser no click, que seria seguir para a ação de redirecionar a página (muito usado para fazer ajax e single page apps), mas colocamos um `if` no começo do nosso método antes de fazer qualquer coisa!

E se quisermos fazer outro handler de clicks em nosso objeto, vamos repetir novamente esse trecho de código, não é muito DRY...

E ai é que (finalmente) entra nosso pattern Facade!

Ele consiste em nos ajudar a tirar esses trechos de código de nossos métodos, tornando-o reutilizável, e mais fácil de dar manutenção se necessário... vamos ver como fica:

{% highlight javascript %}
var app = {
  stop: function(e) {
    if(e.preventDefault) {
      e.preventDefault();
    } else {
      e.returnValue = false;
    }
  },

  clickHandler: function(e) {
    this.stop(e);

    // awesome code
  }
};
{% endhighlight %}

Acho que o código é auto-explicativo, mas vamos analisa-lo brevemente...

Criamos um método chamado `stop` para cuidar de toda a parte de preventDefault, tudo que fosse implementado nessa situação, iria para um método que não atrapalharia nosso clickHandler, tornando-o mais legível.

Esse é um caso extremamente simples do uso de Facade, mas consigo imaginar diversas situações onde esse padrão possa nos ajudar a tornar nosso código mais DRY e manutenível.
