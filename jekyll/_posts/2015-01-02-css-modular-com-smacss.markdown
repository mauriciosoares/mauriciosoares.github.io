---
layout: post
title:  "CSS modular com SMACSS"
date:   2015-01-02 11:00:00
categories: CSS
---

Algo que faz com que muitos back-ends não sejam tao respeitosos com o Front-End é a facilidade com que as coisas fogem do controle (e elas fogem).

Com o CSS não é diferente... quantas vezes você (e eu também) não escreveu css dessa maneira:

{% highlight css %}
#header ul { /*code*/ }
#header ul li { /*code*/ }
#header ul > a { /*code*/ }
#header ul > a span { /*code*/ }
{% endhighlight %}

Parece somente um CSS inofensivo, mas para dar manutenção nisso é um inferno, imaginem que um desenvolvedor novo comemça a tralhar nesse projeto, ele nunca vai saber somente olhando para os seletores o que é o que...

Sem contar que se entrar um seletor no meio disso, provavelmente você vai ter que alterar todos os seletores anteriores para não acontecer algum erro maluco. Isso quando você não coloca outro seletor exatamente igual no final do arquivo, por que você não sabe se vc alterar aquele trecho não vai ferrar com alguma outra parte do site:

{% highlight css %}
#header ul { /*code*/ }
#header ul li { /*code*/ }
#header ul > a { /*code*/ }
#header ul > a span { /*code*/ }

/* muitas e muitas linhas de código */
#header ul > a span {
  font-size: 14px !important;
}
{% endhighlight %}

Isso resolveria por hora, mas você reza para não ter que mexer nesse código 2 vezes, e com esse problema veio uma métodologia chamada SMACSS (**S**calable and **M**odular **A**rchitecture for **CSS**).

Nesse post não vou ensinar nada extremamente avançado, mas alguns fundamentos baseados na minha experiência para que você já comece a usufruir dessa metodologia.

O SMACSS é separado em 5 partes:

* Base
* Layout
* Module
* State
* Theme

Parece muito, mas vamos ver que é o suficiente para qualquer trabalho.

Uma prática minha (que não necessáriamente precisa ser seguida), é que eu gosto de usar somente **classes** nos meus elementos do HTML, só uso **ID`s** quando vou tratar esse elemento com Javascript.

Hoje eu separo em muitos arquivos meus arquivos de CSS, para ajudar na modularização, mas separando tudo em 5 arquivos diferentes já é o suficiente.

## base.css

São basicamente os seletores puros de css, como `div`, `span`, `body` e afins...

Tudo que você precisar mexer nesses elementos, coloque diretamente neste arquivo. Outra coisa importante, não aninhe os seletores, coloque um em cada linha:

{% highlight css %}
body {
  background: blue;
}

a {
  color: red;
}

a:hover {
  color: purple;
}
{% endhighlight %}

Nesse arquivo vai as configurações básicas do seu site, que muitas vezes são até sobreescritas por outras regras dos próximos arquivos.

## layout.css

Pense nos layouts como se fossem blocos do seu site, por exemplo o Header, Footer, Sidebar e o Container do seu site são layouts... Nele configuramos larguras, alturas, floats, backgrounds, coisas mais relacionadas a posição desses blocos e a estilização básica deles:

{% highlight css %}
.header, .footer{
  width: 960px;
    margin: 0 auto;
}

.sidebar {
  width: 200px;
    background-color: blue;
}
{% endhighlight %}

Cada bloco desses contém vários modulos, e ai vem nossa terceira regra:

## module.css

Os modulos podem ser pensados como se fosse o logo do seu site, a barra de navegação, o post do seu blog, a barra de busca, basicamente todos os componentes que o usuário interage, vamos ver como fariamos uma barra de navegação:

{% highlight html %}
<div class="nav">
  <ul>
      <li class="nav-item">
          <a class="nav-item-link" href="#">Link</a>
        </li>
      <li class="nav-item">
          <a class="nav-item-link" href="#">Link</a>
        </li>
      <li class="nav-item">
          <a class="nav-item-link" href="#">Link</a>
        </li>
    </ul>
</div>
{% endhighlight %}

Vamos pensar como foi organizado essa estrutura, primeiro nosso componente se chama `nav`, pois foi nomeado assim a div principal onde ele fica... todos os subcomponentes que vão ser estilizados se chamarão `nav-*`, e os sub dos subcomponentes vão ser `nav-*-*`, e assim por diante... Notem o traço separando cada identificação.

Mas realmente é necessário colocar tantas classes assim no nosso HTML? Sim... faz com que a manutenabilidade do seu código fique mt mais fácil, e seu css mais declarativo (facilitando outros devs que forem mexer no seu código entender melhor a estrutura, sem nem precisar abrir o HTML).

Vamos ao CSS

{% highlight css %}
/**
* Module: Nav
*/
.nav {
  position: relative;
    margin-top: 10px;
}

.nav-item {
  float:left;
}

.nav-item-link {
  color: blue;
    text-decoration: underline;
}
{% endhighlight %}

Primeiro o comentário, como estamos usando um arquivo somente, seria interessante ter comentários separando cada modulo... Como uma maneira melhor de organizar seus arquivos poderiamos separar cada modulo em arquivos css, para ficar tipo: `/assets/css/modules/nav.css`, mas para começo um arquivo já é mais do que o suficiente.

Notem também que não existe aninhamento, pois não é preciso. A nomenclatura que usamos faz com que nosso modulo nunca irá conflitar com outros módulos (claro se você usar nomes únicos para seus modulos), e também faz com que você possa pegar seu módulo de qualquer parte do seu HTML, e colocar em qualquer outro lugar, pois o único css que ele precisa é o do próprio módulo, ele não está aninhado com `header` ou `footer`. Isso é ótimo!

## state.css

O state é o comportamento dos seu módulos, imagine que você queira que seu `nav` suma com o click de algum botão no seu site, poderiamos muito bem pegar o jQuery e fazer o seguinte:

{% highlight javascript %}
$('.nav').hide();
{% endhighlight %}

Isso funcionaria muito bem, mas fica dificil de debugar conforme o tempo, e como as boas práticas mandam, evite usar style inline nos seus elementos, vejamos como podermos resolver isso.

{% highlight css %}
.is-hidden {
  display: none !important;
}
{% endhighlight %}

E no JS?

{% highlight javascript %}
$('.nav').addClass('is-hidden');
{% endhighlight %}

As coisas ficam novamente, muito declarativas, uma classe objetiva para esconder elementos faz com que ela seja reutilizavel em qualquer componente.

Notemos o uso do `!important`, aqui é um dos poucos lugares que você vai precisar usa-lo, pois queremos forçar o elemento a sumir, e se ele por default tiver `display: block;` poderia atrapalhar umpouco as coisas.

States também são usados para muitas outras situações, como `.is-visible`(para mostrar elementos escondidos), `is-error`(para estilizar de vermelho alguma div com error), e assim vai, depende da sua imaginação.

Com isso dito, vamos a última regra.

## theme.css

O theme nem sempre é usado (eu particularmente usei pouquissimas vezes), seu papel é para quando nosso site tem mais de um tema principal (Tema Azul, ou Tema Vermelho) sejá facil essa transição de um para o outro.

Vamos usar como exemplo nosso módulo de `nav`.

{% highlight css %}
/* module.css */
.nav {
  border: 1px solid blue;
}
{% endhighlight %}

Se por um acaso nosso site ter 2 temas, um Azul e um Vermelho por exemplo, que são trocados de acordo com as ações do usuário, poderiamos substituir a configuração de `nav`usando o arquivo `theme.css`.

{% highlight css %}
/* theme.css */
.nav {
  border-color: red;
}
{% endhighlight %}

Notemos que só trocamos o que era necessário, a espessura da borda continuou 1px e solid, mas trocamos a cor para nosso tema vermelho.

Geralmente os arquivos de temas precisam ser mais separados, como por exemplo `/themes/red.css`, `/themes/blue.css`, e chamamos o arquivo.css que precisarmos pro momento.

O Smacss ainda tem mais particularidades, e situações muito interessantes que podem nos ajudar com organização do CSS, mas com esse tutorial você já pode começar a escrever seu CSS modular a partir de hoje!
