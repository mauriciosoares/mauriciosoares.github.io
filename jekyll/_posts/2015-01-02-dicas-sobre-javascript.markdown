---
layout: post
title:  "Dicas sobre Javascript"
date:   2015-01-02 14:00:00
categories: Javascript
---

Nesse Post vou colocar algumas dicas sobre Javascript, vou atualizar este post conforme vou me lembrando (ou conhecendo) dicas novas que possam ajudar no seu dia a dia.

### Parametros em funções

Existe uma variável implicita em todas as funções no Javascript chamada `arguments`, essa variável é um **ArrayLike** (Se parece com um Array, mas não tem seus métodos), e contém todos os argumentos da sua função quando ela é chamada... até mesmo argumentos que não estão declarados na hora que a função é criada, segue o exemplo:

{% highlight javascript %}
function teste(a, b) {
  return arguments;
}

teste(1, 2, 3); // [1, 2, 3]
{% endhighlight %}

Notemos que o terceiro argumento quando chamamos `teste()` não está declarado na função, mas msm assim existe dentro da variável arguments...

Nenhum parâmetro precisa ser definido para a criação da função para que a variável `arguments` contenha os argumentos passados.

{% highlight javascript %}
function teste() {
  return arguments;
}

teste(1, 2, 3); // [1, 2, 3]
{% endhighlight %}

### Elementos com ID's viram variávels

Um comportamento que eu achei muito estranho quando descobri, foi que quando criamos um elemento no HTML que contenha um **ID**, ele vira automáticamente uma variável no Javascript, com referência para o próprio elemento:

{% highlight html %}
<div id="teste">:D</div>
{% endhighlight %}

Criamos uma `div` com o id `teste`, vamos ver a variável teste no Javascript.

{% highlight javascript %}
typeof teste; // object
teste.tagName; // div
teste.innerText; // :D
{% endhighlight %}

Nós nao declaramos a variável teste, ela simplesmente existe no objeto `window`.

Não acho muito seguro usar isso em nosso dia a dia, nunca sabemos quando uma library que estamos usando possa sobreescrever essa nossa variável, trazendo comportamentos e bugs inesperados. Por isso sempre declaro minhas variáveis e atribuo os elementos a ela usando `getElementById` ou algum método do tipo.

### Use Strict

No Ecmascript 3 existiam alguams features que não eram muito bem compreendidas, e algumas más práticas que podiam ser feitas (que continuam em vigor no Ecmascript 5), o `use strict` veio para fazer com que essas práticas sejam eliminadas, garantindo assim uma qualidade umpouco mais alta no seu código, como podemos usa-lo?

{% highlight javascript %}
// código não estrito

'use strict';

// código estrito
{% endhighlight %}

Simplesmente use a string `'use strict'` dentro do seu código, tudo abaixo dela será estrito.

Mas se declararmos o `'use strict'` dentro de uma [closure](http://mauriciosoares.co/blog/javascript/2015/01/02/closures-e-suas-utilidades.html), somente sua closure estará estrita, e nao o que está fora dela, exemplo:

{% highlight javascript %}
// codigo não estrito

function teste() {
  'use strict';
    // codigo estrito
}

// codigo não estrito
{% endhighlight %}

Aqui são algumas vantagens do uso do `'use strict;'`

* Você não pode usar uma variável sem declara-la:
{% highlight javascript %}
'use strict';
a = true; // Error
{% endhighlight %}

* Não é possível deletar uma variável, função ou argumento:
{% highlight javascript %}
'use strict';
var a = true;
delete a; // Error
{% endhighlight %}

* Não é possível declarar duas propriedades quando estamos criando um objeto:
{% highlight javascript %}
'use strict';

var a = {
  index1: true;
    index1: false
}; // Error
{% endhighlight %}

Lembrando que esse erro ocorre se estamos declarando o objeto de forma literal, se nós criarmos um objeto e em seguida atribuir valores para suas propriedades, funcionará corretamente, exemplo:

{% highlight javascript %}
'use strict';

var a = {};
a.index1 = true;
a.index1 = false;

a.index; // false
{% endhighlight %}

* Parametros duplicados em uma função também retorna um erro:

{% highlight javascript %}
'use strict';

function teste(param1, param1) {

} // Error
{% endhighlight %}

* A string `arguments` (a qual falamos anteriormente) não pode ser usada para atribuir outros valores:

{% highlight javascript %}
'use strict';

var arguments = false; // Error
{% endhighlight %}

* O método `with` não pode ser usado (Isso pois ele pode retornar bugs inesperados, e alguns erros de compatibilidade)

{% highlight javascript %}
'use strict';

with(Math) {

} // Error
{% endhighlight %}

Existem algumas outras restrições no Javascript quando usamso o `use strict`, mas essas são algumas das bem importantes, que é bom você conhecer.

### Chrome dev tools e $0

Algo extremamente útil no chrome dev tools, é o `$0`, se estivermos navegando na aba **Elements**, clicando em todos os elementos que queremos debugar, o último elemento que clicarmos vai ficar salvo em uma variável chamada $0 (e o penúltimo em $1, antepenultimo em $2 e assim vai).

Essa variável pode ser usada na aba **Console**, que apontará para o elemento que você clicou anteriormente em **Elements**.

Portanto suponhamos que você esteja debugando sua aplicação, e quer atribuir algum evento a esse elemento sem precisar fazer algum `querySelector`, basta fazer o seguinte:

{% highlight javascript %}
$($0).on('click', callback);

// ou

$0.addEventListener('click', callback);
{% endhighlight %}

Muito simples, não precisamos usar o seletor do jquery, nem usar `getElementById` ou algo do tipo para termos esse elemento preparado para  ser usado.
