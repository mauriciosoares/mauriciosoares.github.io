---
layout: post
title:  "Método extend"
date:   2015-01-02 17:00:00
categories: Javascript
---

A alguns posts atras, nós falamos que objetos são [passados por referência](http://mauriciosoares.co/blog/javascript/2015/01/02/objetos-por-referencia.html), e descobrimos que quando atribuimos um objeto a outra variável, ele é somente uma referência deste primeiro objeto, e tudo que for alterado no primeiro, será alterado no segundo também.

Resolvemos este problema usando `Object.create`, que cria uma cópia e não uma referência do primeiro objeto, vamos ver um exemplo:

{% highlight javascript %}
  // Copiar objetos sem object.create
  var a = {},
    b = a;
  b.teste = true;
  a.teste; // true

  // Copiar objetos com object.create
  var a = {},
    b = Object.create(a);

  b.teste = true;
  a.teste; // undefined
{% endhighlight %}

Isso resolve muitos possíveis bugs que aconteceriam inesperadamente em nossa aplicação, mas temos 2 problemas com essa técnica:

1 - Object.create só funciona a partir do Ecmascript 5, portanto IE8 para baixo não funciona

2 - Object.create só cria uma cópia da raiz do objeto, se tivermos outros objetos aninhados, ainda assim eles serão referências do primeiro, exemplo:

{% highlight javascript %}
var a = {
  objetoAninhado: {}
},

b = Object.create(a);

// criamos um indice a mais na raiz (onde Object.create foi usado)
b.teste = true;
a.teste; // undefined

// criamos um indice a mais em um elemento aninhado (onde Object.create não foi usado)
b.objetoAninhado.teste = true;
a.objetoAninhado.teste; // true
{% endhighlight %}

É... Javascript tem suas particularidades, e esse é um comportamento não muito esperado (para ser sincero descobri isso a não muito tempo antes de escrever esse post).

Ao ler o livro <a href="http://shop.oreilly.com/product/9780596806767.do" target="_blank">Javascript Patterns</a>, do Stoyan Stefanov (que por sinal é um excelente livro), ele ensina uma técnica muito boa de contornar esse error, e criar todos os elementos aninhados de forma que eles sejam uma cópia, e não uma referência (YAY!).

E assim damos vida ao método `extend`.

Vamos ver a utilização de `extend` antes de implementa-lo:

{% highlight javascript %}
var a = {
  objetoAninhado: {}
};

var b = extend(a);

// poderiamos fazer da seguinte forma também:
// var b = {};
// extend(a, b);

b.teste = true;
a.teste; // undefined

b.objetoAninhado.teste = true;
a.objetoAninhado.teste; // undefined
{% endhighlight %}

Ao usarmos o extend, o item aninhado `b.objetoAninhado` não é uma referência de `a.objetoAninhado`, e isso funciona de forma que indexes mais aninhados continuem a não ser uma referência do objeto copiado.

Vamos ver a implementação desse método, e explica-lo em seguida:

{% highlight javascript %}
var extend = function(parent, child) { // 1
  var i = 0,
    child = child || {},
    toStr = Object.prototype.toString,
    arrayRef = '[object Array]'; // 2

  for(i in parent) { // 3
    if(parent.hasOwnProperty(i)) { // 4
      if(typeof parent[i] === 'object') { // 5
        child[i] = (toStr.call(parent[i]) === arrayRef) ? [] : {}; // 6
        extend(parent[i], child[i]); // 7
      } else { // 8
        child[i] = parent[i]; // 9
      }
    }
  }

  return child; // 10
}
{% endhighlight %}

Vou explicar de forma geral o que o método faz, e depois detalhadamente...

O método pega o objeto que já existe, e varre entre todos os seus índices, e verifica se esse indice é um Array/Object, ou qualquer outro valor (String, Boolean, Null, Number), se ele for qualquer outro valor (esses valores não são passados por referência) ele já atribui no index no novo objeto, se for um Array/Object, ele chama a msm função de forma recursiva, até achar os valores que não são passados por referência, e popular os recém criados objetos e arrays.

1 - Criamos nossa função, e colocamos 2 parâmetros, o objeto `parent` (obrigatório), do qual queremos copiar os indices, e o objeto `child` (opcional).

2 - Declaramos algumas variáveis:
`i`: para loopins.
`child`: onde recebemos opcionalmente o objeto em que queremos fazer o extend, caso contrário ele retorna um objeto recém criado.
`toStr`: apenas uma referência para podermos verificar se um objeto é um Array ou um Object (No Javascript, `typeof []` e `typeof {}` retornam "object").
`arrayRef`: um facilitador para verificar o tipo do objeto, e deixarmos nosso código umpouco mais claro (veremos onde é usado umpouco abaixo).

3 - Usamos o **for in** do Javascript, para iterarmos entre todos os indices do objeto `parent`, o qual estamos replicando.

4 - Verificamos se o índice de `parent` é uma propriedade dele mesmo (Poderia ter vindo no objeto prototype, se esse objeto fosse uma instancia).

5 - Verificamos se o índice é um `object`, como disse anteriormente, tanto objetos quanto arrays no javascript retornam `object`, e são esses casos que queremos que sejam duplicados e não sejam passados como referência para o outro objeto.

6 - Fazemos um pequeno ternário, vamos ver como ficaria isso de forma mais clara:

{% highlight javascript %}
if(toStr.call(parent[i]) === arrayRef) { // se o retorno desse call for igual a [object Array] (a variável que declaramos no começo)
  child[i] = []; // child recebe um array novo, e não uma referência
} else {
  child[i] = {}; // child recebe um objeto novo, e não uma referência
}
{% endhighlight %}

7 - Essa parte pode ser a mais complicada, na programação existe uma técnica chamada <a href="http://pt.wikipedia.org/wiki/Recurs%C3%A3o_(ci%C3%AAncia_da_computa%C3%A7%C3%A3o)" target="_blank">Recursividade</a>, essa técnica consiste em chamar uma função dentro dela mesma. Isso pode ser muito útil para items aninhados que tem uma quantidade dinâmica, não precisando fazer IFs dentro de IFs para verificar todas as possibilidades.

Usamos a recursão aqui para rodar novamente nosso extend, e assim criar indices novos dentro de nossos recém criados Arrays e Objects, junto com seus respectivos valores.

8 - Se não for um objeto

9 - Simplesmente atribui o valor que não é passado por referência, para o index do novo objeto.

10 - E por último ele retorna o próprio objeto recém criado.

Pode parecer complicado, mas é mais simples do que parece, sua útilidade é bem grande, e pode ajudar em muitos projetos em que você não estiver trabalhando com Constructors, e quer fazer heranças.
