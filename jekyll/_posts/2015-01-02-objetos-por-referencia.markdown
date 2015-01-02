---
layout: post
title:  "Objetos por referência"
date:   2015-01-02 09:00:00
categories: Javascript
---

Quando trabalhamos com variáveis temos o abto de passar valores para outras variáveis, e assim trata-las, como abaixo:

{% highlight javascript %}
var a = 1;
var b = a;
b += 1;

console.log(a); // 1
console.log(b); // 2
{% endhighlight %}

Esse comportamento é bem simples, o valor de `a` não muda pois ele foi "duplicado" para o `b`, e tudo que você fizer em `b` não afetará em `a`.

Comparações entre variáveis também são bem simples:

{% highlight javascript %}
var a = 1;
var b = 1;

a == b; // true
a === b; // true
{% endhighlight %}

Só para esclarecer, o sinal `==` compara os valores das variáveis, e `===` compara os valores e os tipos (`2 === '2' // false`).

Mas quando trabalhamos com objetos esse comportamento é diferente (e muitas vezes trazem bugs inesperádos). Vamos ver o seguinte exemplo:

{% highlight javascript %}
({} == {}) // false
({} === {}) // false
{% endhighlight %}

Vemos que a comparação entre esses 2 objetos sempre retornam false, isso ocorre por que no Javascript um objeto nunca é igual a outro objeto.

O javascript trabalha com objetos passando eles por referência, isso faz com que ao atribuir um objeto a uma variável, e em seguida atribuir essa mesma variável a uma nova variável, faz com que se a segunda for alterada, a primeira também será, com um exemplo é mais fácil de entender essa confusão:

{% highlight javascript %}
var a = {
  propriedadeDeA: true
};

var b = a;

b.propriedadeDeA // true
b.propriedadeDeB = true;

a.propriedadeDeB // true
{% endhighlight %}

Como o exemplo acima mostra, quando atribuimos `b.propriedadeDeB = true`, automaticamente `a` também recebe a propriedade `propriedadeDeB`, isso acontece porque `b` é somente uma referência de `a`, e tudo que for alterado em um, será alterado no outro.

Essa também é a única situação em que um objeto será igual a outro (pois eles são o mesmo, somente uma referência):

{% highlight javascript %}
var a = {};
b = a;

a == b; // true
a === b; // true
{% endhighlight %}

Isso pode ser um problema quando queremos criar um objeto novo a partir de um já existente, e não queremos que o antigo mude, mas pode ser facilmente contornado com `Object.create`, segue:

{% highlight javascript %}
var a = {hello: 'world'};
// criamos um objeto igual ao "a", mas não
// sendo uma referência, e sim um objeto novo
var b = Object.create(a);

b.hello; // world

a == b; // false
a === b; // false

b.novaPropriedade = true;

a.novaPropriedade; // undefined
{% endhighlight %}

No exemplo acima eu criei um objeto chamado `a`, e criei um novo objeto chamado `b` usando o `Object.create`, notamos que `b` não é uma referência de `a` quando comparamos os 2 objetos com `==` e `===`, e todas as propriedades novas que eu adicionar em `b` não afetarão em nada o objeto `a`.

Essa técnica pode ser muito útil quando trabalhamos com **herança de classes** no javascript, falarei em breve sobre esse assunto.
