---
layout: post
title: "Criando meu primiero service"
date: 2018-05-06 17:32:51
image: '/assets/img/cover/meu-primeiro-service.png'
description: 'Aprenda o que é um Service e como utilizá-lo em seus projetos Ember.js'
category: ''
tags:
twitter_text:
introduction: 'Aprenda o que é um Service e como utilizá-lo em seus projetos Ember.js'
---


Os services do Ember são basicamente objetos que podem ser acessados de qualquer lugar do projeto enquanto sua aplicação estiver aberta. Os dados armazenados neste service
podem ser acessados facilmente nos controllers, routes, components e até mesmo em outros services.

Um bom exemplo para seu uso seria um carrinho de compras, levando em conta que o usuário da aplicação pode navegar por inumeras páginas, adicionando e removendo produtos até
finalizar, será necessário manter esses dados carregados durante a troca de página entre um produto e outro e ao fim ainda poder manipula-los quando precisar finalizar a compra.

Os services podem ser gerados através da linha de comando do Ember-cli, em seu terminal execute o seguinte comando:

```bash
$ ember generate service shopping-cart
```

Após executar o comando, será gerado um arquivos em `app/services/shopping-cart.js` contendo uma estrutura inicial do seu service.

```javascript

import Service from '@ember/service';

export default Service.extend({
});

```

Um Service assim como qualquer objeto, pode conter attributos e métodos, o qual podem estár inicializado ou não. Abaixo temos uma estrutura inicial do nosso service de carinho de compras.

```javascript

import Service from '@ember/service';
import { get, set } from '@ember/object';

export default Service.extend({
  products: null,

  init() {
    this._super(...arguments);
    set(this, 'products', []);
  },

  add(produto) {
    get(this, 'products').pushObject(produto);
  },

  remove(produto) {
    get(this, 'products').removeObject(produto);
  }
});

```

No exemplo acima, criamos um atributo chamado `products` que será inicializado com um array, assim como métodos que permitem adicionar e remover os produtos no mesmo.

### Acessando um service
Você pode injetar seu service em  components, routes, controllers e até em outros services utilizando o `inject` do `@ember/service`. Esse método pode ser usado com ou sem argumentos, o argumento opcional é uma string com o nome do service criado, enquanto que o método usado sem é carregado de acordo com o nome da variável.

Exemplo do método sem passar parâmetros:

```javascript
// app/components/cart-contents.js

import Component from '@ember/component';
import { inject as service } from '@ember/service';

export default Component.extend({
  // Baseado no nome do attributo, será carregado o service do arquivo /app/services/shopping-cart.js
  shoppingCart: service()
});
```

Caso queira injetar o service em um attributo com nome diferente do padrão, será necessário passar o nome do service como no exemplo abaixo:
```javascript
// app/components/cart-contents.js

import Component from '@ember/component';
import { inject as service } from '@ember/service';

export default Component.extend({
  // Nome do attributo pode ser fora do padrão pois o nome real do service está sendo passado manualmente.
  anyName: service('shopping-cart')
});
```

O service é carregado apenas quando for chamado de forma explícita, devido a isso, recomenda-se utilizar o método get para acessar o mesmo, caso contrário é possível receber o valor `undefined`. Os dados persistidos no service serão mantidos enquanto sua aplicação estiver aberta.

### Exemplo do uso do service

No exemplo abaixo, estamos implementando uma action chamada `remove` que utiliza o método do carrinho de compras, perceba que o carinho é acessado através do método `this.get('shoppingCart')` para evitar o problema descrito acima.

```javascript
// app/components/cart-contents.js

import Component from '@ember/component';
import { inject as service } from '@ember/service';

export default Component.extend({
  shoppingCart: service()

  actions: {
    remove(product) {
      this.get('shoppingCart').remove(product);
    }
  }
});
```
Abaxo estamos listando todos os produtos do carrinho dentro do template.

```handlebars
// app/templates/components/cart-contents.hbs

{% raw %}
<ul>
  {{#each shoppingCart.products as |product|}}
    <li>
      {{product.name}}
      <button {{action "remove" product}}>Remover Produto</button>
    </li>
  {{/each}}
</ul>
{% endraw %}
```
Além do carrinho de compras exemplificado acima, os services também podem ser útil para armazenar o usuário autenticado, fazer controle do envio e recebimento de notificações entre várias outras funcionalidades. Para mais informações sobre o Ember Service consulte a [documentação](https://guides.emberjs.com/v3.1.0/applications/services/) do Ember.js
