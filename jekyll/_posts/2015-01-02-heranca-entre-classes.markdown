---
layout: post
title:  "Herança entre classes"
date:   2015-01-02 10:00:00
categories: Javascript
---

Muitas linguagens de programação já vem com um esquema de **Classes** e **Super Classes** pronto, com sua sintaxe já definida, como por exemplo no Ruby quando queremos criar uma herança entre classes:

{% highlight ruby %}
class Mamifero
  def respirar
    puts "Inalar e Exalar"
  end
end

class Gato < Mamifero
  def falar
    puts "Meow"
  end
end
{% endhighlight %}

Mas no Javascript não temos uma sintaxe já definida para fazer isso, precisamos simular esse comportamento com o uso de prototypes, applys e calls.

{% highlight javascript %}
function Pessoa(nome) {
  this.nome = nome;
}

Pessoa.prototype.falarNome = function() {
  return 'Olá, meu nome é ' + this.nome;
}

var mauricio = new Pessoa('Mauricio');
mauricio.falarNome(); // Olá, meu nome é Mauricio
{% endhighlight %}

Dessa maneira criamos uma classe principal chamada `Pessoa`, vamos agora criar uma classe chamada `Empregado`, que extende os métodos de pessoa.

{% highlight javascript %}
function Empregado(nome, emprego) {
  // primeiro parametro é o valor de "this"
  // o segundo são os parametros da função
  Pessoa.call(this, nome);
  this.emprego = emprego;
}

var mauricio = new Empregado('Mauricio', 'Desenvolvedor Front End');

console.log(mauricio.name); // Mauricio
console.log(mauricio.emprego); // Desenvolvedor Front End
{% endhighlight %}

Vamos entender o que aconteceu aqui.

Primeiro criamos a classe `Empregado`, e usamos `Pessoa.call` para atribuir o valor de `this.nome`. Em poucas palavras, o `call` e o `apply` são usados para chamar funções mudando o objeto de `this`, nesse caso vamos usar o da instancia de `Empregado`, portanto em `Pessoa`, quando fazemos `this.name = name`, o name que esá sendo setado é na instancia do `Empregado` (Espero não ter ficado muito confuso essa explicação).

Poderiamos ter simplesmente usado `this.nome` em `Empregado`, mas vamos imaginar que em `Pessoa`, quando atribuimos algum valor a `nome` ocorra algumas verificações, assim não precisamos duplicar códigos em `Pessoa` e `Empregado` (DRY - Don't Repeat Yourself).

Certo, temos nossa segunda classe, mas ela ainda não possui os métodos de `Pessoa`, vamos resolver isso agora.

{% highlight javascript %}
Empregado.prototype = Object.create(Pessoa.prototype);

Empregado.prototype.novoMetodo = function() {}
{% endhighlight %}

Certo, atribuimos ao prototype de `Empregado` todos os métodos de `Pessoa`, mas por que usamos `Object.create`?

Como eu expliquei em um post anterior, os [objetos são passados por referência](http://blog.herebecoders.com/js-objetos-por-referencia/), se simplesmente fizessemos isso `Empregado.prototype = Pessoa.prototype`, quando criamos `novoMetodo` em `Empregado`, automaticamente `Pessoa` também teria recebido esse método, e não é isso que queremos.

Já estamos bem evoluidos com nossas heranças, vamos ver o comportamento por enquanto:

{% highlight javascript %}
var mauricio = new Empregado('Mauricio', 'Desenvolvedor Front End');
mauricio.falarNome(); // Olá, meu nome é Mauricio
{% endhighlight %}

Nossa instancia de `Empregado` já possui métodos de `Pessoa`, mas vamos supor que queiramos fazer um método muito parecido com o de `Pessoa`, mas que só que somente incluindo um texto no final?

Existe o jeito fácil de fazer isso:

{% highlight javascript %}
Empregado.prototype.falarNomeEmprego = function() {
  return 'Olá, meu nome é ' + this.nome + ' e minha profissão é ' + this.emprego;
}
{% endhighlight %}

Notamos que o começo de nossa resposta é exatamente igual ao método `Pessoa.prototype.falarNome`, e como boa prática, não se repita :D!

Podemos refatorar o código acima para algo reutilizável:

{% highlight javascript %}
Empregado.prototype.falarNomeEmprego = function() {
  return Person.prototype.falarNome.call(this) + ' e minha profissão é ' + this.emprego;
}

var mauricio = new Empregado('Mauricio', 'Desenvolvedor Front End');
mauricio.falarNome(); // Olá, meu nome é Mauricio
mauricio.falarNomeEmprego(); // Olá, meu nome é Mauricio e minha profissão é Desenvolvedor Front End
{% endhighlight %}

Dessa maneira, usamos novamente o call para chamar o método de `Person`, mudando o valor de `this`, pegamos o resultado e concatenamos com o resto da string desejada.

Esses exemplos parecem simples, mas imaginem isso sendo aplicado em regras de negócio mais elaboradas, onde cada método tem vários tipos de verificações, e que não queremos duplicar essas verificações por todo canto da aplicação.
