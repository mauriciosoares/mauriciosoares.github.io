---
layout: post
title:  "Uma palinha sobre Backbone - sem código"
date:   2015-01-02 21:00:00
categories: Javascript
---

Fala galera, passei um tempo sem escrever aqui no blog pois estava trabalhando em um projeto pessoal bem bacana, e provavelmente a frequência de posts novos ira diminuir umpouco (um por dia estava meio puxado hahaha).

Nesse post não teremos código, mas vou explicar umpouco o conceito de um excelente (e popular) framework chamado [Backbone](backbonejs.org). O backbone tem o jQuery e o Underscore como dependência.

Basicamente o Backbone tem 3 conceitos extremamente importantes, que é a base de todas as aplicações escritas com ele, seriam os **models**, as **collections** e as **views**.

Para usar uma aplicação real como exemplo, falaremos de cada um desses conceitos usando um app ToDo como exemplo (clichê?).

###Models

Os models são dados únicos dentro de uma aplicação, imaginemos nosso app, teremos uma lista de afazeres nele, cada item dessa lista seria equivalente a um Model. Simples assim.

Nos models podemos ter configurações como valores default (um item da nossa todo list poderia ser o título e se ela ja está checada), podemos ter verificações na hora que um model é criado, para sabermos se os valores condizem com o esperado.

Fora tudo isso, os models tem diversos métodos para nos ajudar a manipula-lo, como métodos se o model tem determinado valor, para setar um valor diferente, para destrui-lo, se ele foi alterado, e assim vai...

Mas os models a princípio, são espaços alocados na memória, o fato de você criar um model, não faz com que ele apareça **automaticamente** no DOM. Com a ajuda das **views** conseguiremos isso, que falaremos logo após as collections.

### Collections

Pense nas collections como um conjunto de models. Pronto. Basicamente sua lista de Todos como um todo.

Sério, uma collection seria um lugar onde vários models (ou itens do seu todo) estão alocados, onde você pode fazer ações que interagem naquele conjunto de forma geral, não tendo que iterar em todos os models um por um (não que por trás dos panos não seja feito isso, mas é a convenção do framework).

Em uma collection você fala para qual model essa collection se referencia, portando você também pode criar models usando métodos da collection, fazendo com que esse model já seja incluido na collection automaticamente.

Mas como os models anteriormente, as collections não passam de espaços na memória com seus respectivos models, usaremos em conjunto as **views** para que tudo isso apareça no DOM.

### Views

Com a ajuda das views, podemos colocar tudo que criamos no DOM.

Quando usamos as views, nós podemos pegar as collections ou os models, e com a ajuda de templates, criar estruturas HTML para podermos jogar no DOM.

Podemos usar os próprios templates do underscore para definirmos nossa estrutura que virão os dados.

Nas Views podemos colocar eventos de `click`, `keyup`, e vários outros, fazendo com que quando algum desses eventos ocorrer, podemos trocar informações dentro dos models, e em seguida atualizar nossas views com as informações novas. Em comparação ao Todo List, seriam os cliques para deletar uma todo específica, ou para limpar todos que já foram concluidas de seus dados...

Diferente do **AngularJS** (que falarei em um post futuro), o Backbone não tem por default o famoso **two way databind**, que quando algo é alterado no model, automaticamente é alterado na view. Tudo que alteramos em nossos models, temos que mandar nossa view se atualizar para que os dados persistam.

Fique claro que não precisamos referenciar nem models nem collections nas views para podermos usa-las... Você pode usar uma View para controlar sua aplicação de forma geral, adicionando eventos em lugares que não influenciarão em um model. Como uma view para startar todas as outras views necessárias.

#### Conclusão

Por mais chato que pareça esse post, é muito importante entendermos o básico de como o framework funciona, na hora que virmos código tudo ficara umpouco mais claro para entendermos por que estamos codando daquela maneira.

Na minha opnião, backbone é um framework muito bom, nos dá bastante controle sob a aplicação, pois temos que fazer muita coisa na mão. Isso no ponto de vista de alguns desenvolvedores é ruim, pois resulta em mais código, mas acho particularmente divertido fazer com que cada ação de sua aplicação aconteça.

Não vou comparar AngularJS com Backbone, já trabalhei com ambos, e ambos são muito fodas... Acredito que o propósito de ambos seja semelhante, ajudar os desenvolvedores a atingir seus objetivos com a aplicação, e portanto qualquer um que você usar já será algo interessante.

Massss, uma dica para muitos desenvolvedores que estão se aventurando no vasto mundo do Javascript, não aprendam um framework antes de aprender conceitos básicos do Javascript, isso te torna muito dependente, e faz com que erros que são relativamente simples, se tornem um inferno para debugar.

Aprenda a linguagem antes, depois suas ferramentas.
