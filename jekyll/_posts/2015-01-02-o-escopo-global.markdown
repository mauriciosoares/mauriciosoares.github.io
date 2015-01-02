---
layout: post
title:  "O Escopo Global"
date:   2015-01-02 01:00:00
categories: Javascript
---

Todas as variáveis que você define no javascript que estão fora de funções são chamadas de variaveis globais, pelo fato de elas terem sido declaradas no **Escopo Global**.

É bem simples declarar uma variável no escopo global, como muitos de vocês devem saber, é só fazer o seguinte:

{% highlight javascript %}
var minhaGlobal = true;
{% endhighlight %}

O fato de sua variável ter sido de clarada no escopo global faz com que ela seja acessível em todo canto de sua aplicação.

## Como acessar variaveis do escopo global?

Para acessar uma variavel você pode simplesmente usar o nome da variavel:

{% highlight javascript %}
minhaGlobal; // true
{% endhighlight %}

ou usar o objeto `window`:

{% highlight javascript %}
window.minhaGlobal; // true
{% endhighlight %}

## O objeto **Window**

``window`` pode ser usado para acessar todas as variáveis que estão no escopo global (e também muitos objetos úteis como o `document`, que iremos falar em outro post).

Variáveis (ou outros objetos) no escopo de `window` podem ser acessados somente usando o próprio nome criado, como no tópico anterior, quando usamos `minhaGlobal` sem usar o `window`.

Outra maneira de criar variáveis globais seriam atribuindo ao próprio objeto window:

{% highlight javascript %}
window.minhaOutraGlobal = true;
{% endhighlight %}

Dessa maneira a variávei `minhaOutraGlobal` pode ser também acessada de qualquer canto de sua aplicação.

## Por que é uma má prática atribuir variaveis ao escopo global?

Como eu disse acima, variáveis globais podem ser acessadas de qualquer canto de sua aplicação, isso pode parecer bom, mas é pior do que parece...

O fato de sua variável ser acessada por toda aplicação pode implicar em erros inesperados, como por exemplo uma library parar de funcionar (como o jQuery).

Para quem conhece o jQuery, sabemos que ele pode ser usado através da variável `$` ou `jQuery`, imaginemos que por algum motivo você precise usar essa variavel para armazenar algum valor...

{% highlight javascript %}
var jQuery = 'Hello, variables';
{% endhighlight %}

Dessa forma, o jQuery pararia de funcionar sem vestígios, e isso pode render um bom tempo debugando o código para descobrir o que aconteceu.

## Como evitar variaveis globais?

Existem algumas técnicas para podermos evitar variáveis no escopo global, 2 delas das quais falarei nos próximos posts são usando **Namespaces**, e **Closures**.
