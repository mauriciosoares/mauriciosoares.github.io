---
layout: post
title:  "Bind crossbrowser"
date:   2015-01-02 19:00:00
categories: Javascript
---

Em um dos posts anteriores falamos sobre o [bind](http://blog.herebecoders.com/js-bind-em-callbacks/), e quão útil ele pode ser em nossa aplicação... Mas infelizmente o `bind` é uma feature do Ecmascript 5, e sua compatibilidade com o IE é a partir do 9 (e todos sabemos que o cliente sempre quer apps que funcionem no IE 8 e muitas vezes no IE 7).

Contudo, existe uma maneira bem simples de implementa-lo em ambientes antigos (isso é uma técnica chamada **polyfill**), que com ela nosso bind funcionará da mesma maneira que deveria funcionar (e com pouquíssimas linhas de código).

Vamos ver como implementa-la, e explico abaixo como funciona cada linha.

{% highlight javascript %}
if(typeof Function.prototype.bind === 'undefined') { // 1
  Function.prototype.bind = function(newThis) { // 2
    var slice = Array.prototype.slice,
      args = slice.call(arguments, 1),
      fn = this; // 3

    return function() { // 4
      return fn.apply(thisArgs, args.concat(slice.call(arguments))); // 5
    }
  }
}
{% endhighlight %}

Vamos analisar então o código:

1 - Antes de mais nada precisamos saber se o método bind existe. Não queremos substituir a versão nativa caso nosso ambiente dê suporte ao Ecmascript, o intuito do polyfill é somente implementar a feature se ela não tiver implementada nativamente.

2 - Em `Function.prototype.bind`, que no caso seria o constructor de todas as functions, receberemos uma função com argumentos... (Se precisar refrescar a memória sobre como funciona o prototype, acesse [aqui](http://blog.herebecoders.com/js-prototypes/)). E recebemos um parâmetro que seria o novo `this` que será chamado no seu callback.

3 - Declaramos algumas variáveis
`slice`, é apenas um **alias** para o `Array.prototype.slice`, já que o usaremos mais de uma vez sempre é bom cachearmos seu acesso em algo mais simples de ler.

`args`, Nós usamos o `slice` declarado acima, assim recebemos todos os items de `arguments` como um Array, e tiramos o primeiro argumento, que já esta no parâmetro `newThis` de nossa função. Se não souber o porque de `arguments`, você pode dar uma lida [aqui](http://blog.herebecoders.com/js-dicas-sobre-javascript/)

`fn`, recebemos a função em sí que está sendo chamado o `bind`. Nesse caso `this` aponta para a própria função que estamos usando o `bind`.

4 - Em seguida retornamos uma função, pois nosso callback espera o escopo de uma função, e não uma função já executada.

5 - E finalmente usamos o [apply](http://blog.herebecoders.com/js-como-funciona-o-call-e-o-apply/), para executarmos nossa função com o this que nós passamos como parâmetro no bind (`newThis`). Em seguida fazemos uma pequena mutreta, passamos nossos `args` como parâmetros que receberemos no nosso callback, e concatenamos `arguments` novamente, pois o callback pode querer passar parâmetros também a sua função que você esta usando.

O bind é algo extremamente útil, que com certeza me ajuda muito em meu dia a dia, e seu polyfill é tão útil quanto.

Outra maneira de usar o bind seria com o jQuery, usando [$.proxy](http://api.jquery.com/jquery.proxy/), ele também implementa o polyfill no bind.
