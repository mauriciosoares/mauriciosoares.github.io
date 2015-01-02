---
layout: post
title:  "Combinando Objetos com Mixins"
date:   2015-01-02 18:00:00
categories: Javascript
---

Uma maneira interessante de combinar um número indefinido de objetos é usando um método chamado `mixin`.

Com o `mixin` conseguimos pegar um número qualquer de objetos, e transforma-los em uma única variável. Vamos ver como ele é usado:

{% highlight javascript %}
var mixed = mixin({
  a: 'a',
  b: 'b'
}, {
  c: 'cas',
}, {
  d: 'das',
  e: 'es'
});

mixed; // {a: 'a', b: 'b', c: 'cas', d: 'das', e: 'es'};
{% endhighlight %}

Vamos ver como o `mixin` é implementado:

{% highlight javascript %}
var mixin = function() { //1
  var args,
    props,
    child = {},
    argsLentgh = arguments.length; // 2

  for(args = 0; args < argsLentgh; args += 1) { // 3
    for(props in arguments[args]) { // 4
      if(arguments.hasOwnProperty(args)) { // 5
        child[props] = arguments[args][props]; // 6
      }
    }
  }

  return child; // 7
};
{% endhighlight %}

Antes de mais nada, funções tem uma variável que é automáticamente gerada chamada `arguments`, você pode ler umpouco sobre isso no primeiro item [desse post](http://mauriciosoares.co/blog/javascript/2015/01/02/dicas-sobre-javascript.html).

Ele funciona de forma semelhante ao método [extend](http://mauriciosoares.co/blog/javascript/2015/01/02/metodo-extend.html).

1 - Criamos uma função sem os parâmetros definidos, pois eles podem ser um número desconhecido.

2 - Declaramos algumas variáveis
`args`: Usado para armazenar o index dos argumentos no `for`.
`props`: Usado para armazenar o nome da propriedade no `for in`.
`child`: Um objeto o qual armazenaremos o mix de objetos
`argsLentgh`: Cacheamos o length de `arguments`, para podermos otimizar a velocidade de nosso `for`

3 - Iteramos sob todos os `arguments` de nossa função.

4 - Iteramos sobre todas as propriedades do `arguments` que estamos no `for`.

5 - Por segurança, verificamos se essa propriedade eh realmente desse objeto.

6 - Atribuimos no índice do nosso novo objeto, o indice desse argumento que estamos lendo.

7 - E por fim retornamos nosso novo objeto `child` com todas as novas propriedades.

Nessa implementação, se tivermos indices iguais, o último sempre prevalererá:

{% highlight javascript %}
var mixed = mixin({
  indice1: true,
  indice2: true
}, {
  indice1: false
});

mixed.indice1; // false
mixed.indice2; // true
{% endhighlight %}

Nota que essa técnica passa os objetos [por referência](http://mauriciosoares.co/blog/javascript/2015/01/02/objetos-por-referencia.html).
