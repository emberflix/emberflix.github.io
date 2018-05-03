---
layout: post
title: "O que são componentes?"
date: 2018-04-25 23:00:59
image: '/assets/img/cover/ember-components.png'
description: 'Um guia rápido e prático de como criar e utilizar os componentes no Ember.js'
category: ''
tags:
twitter_text:
introduction: 'Um guia rápido e prático de como criar e utilizar os componentes no Ember.js'
---
Neste post vamos falar sobre um dos principais recursos do Ember.js, os componentes permitem separar uma funcionalidade tornando a independente do restante do projeto e permitindo que o seu código seja reutilizado em outros lugares.

Para criar um componente  basta executar o comando abaixo:

```bash
ember generate component meu-primeiro-componente
```
Após executar o comando em seu terminal, o componente será criado gerando assim dois arquivos, um `.hbs` onde vai ficar a parte HTML/Handlebars  e um arquivo `.js` onde ficará o código javascript.

Exemplo do uso de um componente.

```handlebars
// app/templates/index.hbs
{% raw %}
<div class="row">

  <div class="col-md-6 offset-md-3 mt-5">
    {{#each model as |user|}}
      <div class="col-md-12 mt-1">
        <div class="card">
          <div class="card-header">
            <img src={{user.avatar}} class="rounded-circle float-left" width="80" height="80">
            <h5 class="float-left mt-4">{{user.firstName}} {{user.lastName}}</h5>
          </div>
        </div>
      </div>
    {{/each}}
  </div>
{% endraw %}
</div>
```

Neste exemplo, está sendo listado os usuários em uma rota qualquer, perceba que dentro do loop `EACH` há um bloco que vai exibir as informações da pessoa. O fato é que toda vez que essas informações precisarem ser exibidas em algum lugar do projeto, o código terá que ser repetido.

Neste caso podemos criar um componente para isolar essa funcionalidade.

```bash
ember generate component user-profile
```

No arquivo `app/templates/components/user-profile.hbs` que foi gerado, vamos adiconar o HTML que está dentro do `EACH`, ficando assim:

```handlebars
{% raw %}
<div class="col-md-12 mt-1">
  <div class="card">
    <div class="card-header">
      <img src={{user.avatar}} class="rounded-circle float-left" width="80" height="80">
      <h5 class="float-left mt-4">{{user.name}}</h5>
    </div>
  </div>
</div>
{% endraw %}
```
Agora basta chamar o componente dentro do template:

```handlebars
app/templates/index.hbs
{% raw %}
<div class="row">

  <div class="col-md-6 offset-md-3 mt-5">
    {{#each model as |user|}}
      {{user-profile user=user}}
    {{/each}}
  </div>
{% endraw %}
</div>
```

No exemplo acima, o componente recebe uma propriedade com o nome `user` que contém o objeto com os dados do usuário. O componente pode receber
outros dados, todos seguindo o padrão: `nomeDaPropriedade=Valor`, sendo assim, dentro do componente os valores poderão ser acessados pelo nome da propriedade.

### Ciclo de vida do componente

Os componentes Ember disponibilizam Hooks que são executados em momentos específicos durante seu ciclo de vida.

```js
// app/components/user-profile.js

didReceiveAttrs() {
	let user = this.get('user')ç
	console.log(user);
}
```

O `didReceiveAttrs` utilizado acima é acionado quando o componente receber os dados passados por parâmetro, desta forma todo código dentro deste contexto pode
considerar que os valores já estão carregados. Segue abaixo outros exemplos:

```js
// app/components/user-profile.js

didInsertElement() {
	// É chamado quando todos os elementos HTML do componente estiverem inseridos no DOM.
  console.log('Componente carregados da tela');
},

didDestroyElement() {
	// É chamado quando o componente for removido do DOM.
  console.log('Componente foi removido da tela');
}
```
Mais detalhes sobre o ciclo de vida dos componentes podem ser encontrados na [documentação](https://guides.emberjs.com/v2.11.0/components/the-component-lifecycle) oficial do Ember.js.


### Customizando o componente
O componente também disponibiliza algumas propriedades para a customização do mesmo:

```js
classNames: 'btn btn-succes',
tagName: 'button'
```

A propriedade `classNames` pode receber uma string ou um array de class a qual será aplicado no componente. A class também pode ser definida informando um parâmetro class quando chamar o component em seu template.

Já a propriedade `tagName` serve para definir qual tag o componente terá, por padrão o mesmo utiliza a tag div. No exemplo acima, será renderizado um botão com os estilos do bootstrap.

### Actions
É possível criar action no componente e chamá las da mesma forma que é feito nas routes e controllers.
Também é possível chamar uma action de um controller estando dentro de um componente utilizando o método `sendAction`:

```js
// app/components/user-profile.js

actionName: 'removeUserInController',

actions: {
  removeUser(user) {
    this.sendAction('actionName', user);
  }
}
```

```js
// app/controller/index.js

actions: {
  removeUserInController(user) {
    user.destroyRecord();
  }
}
```

No exemplo acima, a action `removeUser` do componente vai invocar a action `removeUserInController` do controller passando o `user` por parâmetro, onde então o mesmo será removido.

### Usando parâmetros de bloco

Os componentes podem ser utilizados em forma de bloco, usando o mesmo exemplo do user-profile ficaria da seguinte maneira:

```handlebars
app/templates/components/user-profile.hbs

{% raw %}
<div class="col-md-12 mt-1">
  <div class="card">
    <div class="card-header">
      <img src={{user.avatar}} class="rounded-circle float-left" width="80" height="80">
      <h5 class="float-left mt-4">{{user.name}}</h5>
      {{yield}}
    </div>
  </div>
</div>
{% endraw %}
```

```handlebars
app/templates/index.hbs

{% raw %}
<div class="row">

  <div class="col-md-6 offset-md-3 mt-5">
    {{#each model as |user|}}
      {{#user-profile user=user}}
        <h6>
          Outras informações do usuário
        </h6>      
      {{/user-profile}}
    {{/each}}
  </div>
{% endraw %}
</div>
```

Utilizando o componente dessa forma, todo o conteúdo dentro do bloco será renderizado no lugar do `yield`, permitindo criar componentes mais dinâmicos. No exemplo acima, além do nome do usuário que sempre será exibido, pode-se passar outras informações sem precisar alterar o template do componente.

Para obter mais informações sobre os componentes, acesse a [documentação](https://guides.emberjs.com/v3.1.0/components/defining-a-component/) oficial do Ember.js.
