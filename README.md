## Objetivo

O objetivo deste guia é descrever um padrão de desenvolvimento de código `CSS` cujo o processo de estilização fique mais simples e produtivo, mantendo a organização, boas práticas e performance na aplicação.

---

## Design Atômico

O Design Atômico é uma metodologia de desenvolvimento de estilos css onde a estrutura da página ou site é encarada de forma semelhante ao conceito químico de átomos e moléculas. Ou seja, componentes menores como botões por exemplo, são vistos como átomos dentro da página, enquanto items maiores como o rodapé do site, são tratados como organismos.

Dessa forma os elementos mais complexos (organismos) são compostos por unidades menores (moléculas), até chegarmos nas menores partes da hierarquia (átomos).

<p style="background-color: #fff; padding: 25px;"><img src="./paginas.png" alt="Design Atômico - Esquema" title="Design Atômico"></p>

### **Átomos**
Átomos são as menores partes dentro da estrutura do layout. São elementos que devem ser o mais abstratos possíveis, para que possam se adequar a praticamante qualquer parte do código em que forem utilizados. Botões, links e logos por exemplo, geralmente tem esse comportamento.

**Exemplo**

> HTML
```
<a class="a-link">Link</a>
```

> CSS
```
a-link {
  color: #f00;
  text-align: center;
  font-size: 12px;
  border: 1px solid #f00;
}
```


### **Moléculas**
Moléculas são o segundo nível da hierarquia de elementos. As moléculas são basicamente a união de átomos dando alguma função e estrutura a eles. Exemplos de moléculas podem ser os menus e listas.

**Exemplo**

> HTML
```
<ul class="m-list">
  <li class="m-list-item">
    <a class="a-link">Item 1</a>
  </li>
  <li class="m-list-item">
    <a class="a-link">Item 2</a>
  </li>
</ul>
```

> CSS
```
m-list {
  margin-left: auto;
  margin-right: auto;
  max-width: 1200px;
}

m-list-item {
  padding: 10px;
}
```

### **Organismos**
Os Organismos assim como as moléculas são compostos por suas partes menores (átomos e moléculas). Esses elementos de forma geral, já são capazes de trazer uma informação ou funcionaidade completa. Alguns exemplos são o rodapé, cabeçalho e artigos;

**Exemplo**

> HTML
```
<footer class="o-footer">
  <ul class="m-list">
    <li class="m-list-item">
      <a class="a-link">Item 1</a>
    </li>
    <li class="m-list-item">
      <a class="a-link">Item 2</a>
    </li>
  </ul>
  <ul class="m-list">
    <li class="m-list-item">
      <a class="a-link">Item 3</a>
    </li>
    <li class="m-list-item">
      <a class="a-link">Item 4</a>
    </li>
  </ul>
</footer>
```

> CSS
```
o-footer {
  width: 100%;
  background-color: #000;
  position: relative;
}
```

### **Templates**
Os templates são componentes que estilizam características gerais de maneria que as estruturas das páginas possam ser reaproveitadas, ou seja, uma espécie de "esqueleto" do layout.

**Exemplo**

> HTML
```
<main class="t-brand">
  /* Conteúdo */
</main>
```

> CSS
```
t-brand {
  width: 100%;
  min-height: 950px;
  position: relative;
  background-color: #fff;
}
```

### **Páginas**
As páginas são o último nível da hierarquia. Nelas temos estilos que abrangem comportamentos específicos de um determinado template, uma vez que os demais estilos já foram determinados pelas suas partes.

**Exemplo**

> HTML
```
<body class="p-main-brand">
  /* Conteúdo */
</body>
```

> CSS
```
p-main-brand {
  background-color: #00f;
}
```

---

## Manipulando a estrutura

Um ponto importante para ser considerado a respeito do Design Atômico é o escopo dos seus componentes e como estilizá-los da melhor forma possível para que eles realmente tenham as características recomendadas na estrutura.

### **Adicionando ou sobescrevendo regras**
Em casos em que um componente filho (átomo) precisa de ser estilizado segundo o seu componente pai (molécula) a prática adotada é adicionar uma classe de marcação do elemento pai no filho.


**Exemplo**
> HTML
```
<div class="m-holder">
  <button class="a-btn m-holder-btn">Botão 1</button>
</div>
```

> CSS
```
a-btn {
  font-size: 12px;
  text-align: center;
  background-color: #ff0;
}

m-holder {
  background-color: #00f;
}

m-holder-btn {
  display: block;
  margin-left: auto;
  margin-right: auto;
  width: 200px;
}

```

Com essa prática o Design Atômico se torma mais eficiente, pois regras específicas para um átomo em uma molécula por exemplo, não vão interferir no comportamento do mesmo átomo em outros lugares em que for utilizado. Isso também deve ser adotado para os outros tipos de elementos (moléculas, organismos, templates e páginas). 

No caso dos átomos, é interessante pensar que regras de posicionamento e espaçamento nesses elementos não é o ideal, uma vez que essas propriedades devem ser definidas pelo contexto em que estiverem insreidos (na molécula nessa situação).

E aqui encontramos uma das principais vantagens de se utilizar esta metodologia. Como os estilos dos seus componentes são escritos pensando em seu comportamento de forma independente do seu contexto, é possível ter uma **reutilização de código** muito alta, melhorando a **produtividade** e **organização** da estrutura de estilos.


### **Átomos Virtuais**

Átomos virtuais são aqueles que apesar de possuirem o comportamento de um átomo estão diretamente ligados á uma molécula e não existiriam sem ela. Neste caso específico os estilos desse átomo ficam na sua molécula. O item de um menu único de navegação por exemplo, possui essa característica.


**Exemplo**

> HTML
```
<ul class="m-list">
  <li class="m-list-item">
    <a class="a-link">Item 1</a>
  </li>
  <li class="m-list-item">
    <a class="a-link">Item 2</a>
  </li>
</ul>
```

> CSS
```
.m-list {
  list-style: none;
}

.m-list-item {
  padding: 10px;
}
```

* `m-list-item` neste caso pode ser classificado como um átomo virtual, pois não existiria sem o seu elemento pai (`m-list`).
---


## Nomenclatura de classes

### **Lógica de nomenclatura**
Para manter um padrão de estilização dos componetes da página ou site, o ideal é priorizar o uso de classes para estilizar os elementos e não outros seletores.

O nome das classes deve sempre buscar elicitar o que o elemento em questão representa dentro da estrutura da página e não o seu conteúdo. Por exemplo, se houver um elemento `div` com um banner de um refrigerante, o ideal é nomeá-lo de acordo com seu papel na estrutura e não seu conteúdo (refrigerante). Um possível nome para este componente então seria `m-banner` que está relaciionado ao seu papel, diferente de `m-soda` que se refere ao conteúdo.

### **Linguagem**
Os nomes dos seletores sempre devem estar na língua inglesa.

### **Dashed Case**
Quando o nome de um componente é composto, utiliza-se o padrão `dashed case` para separar os nomes.

**Exemplos**
```
  .a-main-link
  .m-list-item
  .o-footer-conteiner
```

### **Prefixos**
Para ser evitado a sobescrita de regras de estilos de maneira involuntária, os seguintes prefixos são utilizados para cada tipo de componente:

- Átomos: `a-`
```
  .a-atom-name ...
```

- Moléculas: `m-`
```
  .m-molecule-name ...
```

- Organismos: `o-`
```
  .o-organism-name ...
```

- Templates: `t-`
```
  .t-template-name ...
```

- Páginas: `p-`
```
  .p-page-name ...
```

### **Modificadores**
Quando for necessário criar uma variação de estilos para um determinado elemento, o padrão utilizado é adicionar o nome da classe modificadora juntamente a classe do elemento que vai ser modificado.

**Exemplo**

> HTML
```
<div class="m-grid">
  <p class="m-grid-text">Lorem ipsum</p>
</div>
<div class="m-grid active">
  <p class="m-grid-text">Lorem ipsum</p>
</div>
```

> SASS, SCSS e Stylus 
```
.m-grid {
  display: none;
  width: 500px;
  height: 250px;
  background-color: #f00;

  &.active {
    display: block;
  }
}

```
> CSS
```
.m-grid {
  display: none;
  width: 500px;
  height: 250px;
  background-color: #f00;
}

.m-grid.active {
  display: block;
}
```

### **Abreviações**

No desenvolvimento dos estilos dos componentes é interessante maximizar o uso de nomes curtos e abreviações nos seletores CSS, para que dessa maneira a legibilidade e também performance do código sejam aperfeiçoadas.

Outro fator muito importante a ser considerado é que os mecanismos de busca na web analisam a proporção código/conteúdo nas páginas. Isso significa que quanto menos código de marcação e estilo a página possuir em relação ao seu conteúdo melhor será o seu posicionamento, características que são implementadas com estas práticas.

**Exemplo**

> HTML
```
<div class="m-banner-hld">
  /* Content */
</div>
```

> CSS
```
.m-banner-hld {
  width: 500px;
}
```

* `hld` por exemplo, pode ser uma abreviação de "holder";
* Abreviações devem sempre ter 3 caracteres;
* No caso das abreviações, o ideal é que exista um controle das nomeclaturas.

### **Uso do "&" nos seletores**

O seletor "&" que nos pré-processadores de CSS como SASS e Stylus representa o elemento selecionado anteriormente, não deve ser utilizado frequentemente ou sem uma razão específica.

O propósito de adotar está prática é que quando esse seletor é utilizado da maneira displicente, pode gerar códigos CSS que não entregam as melhores práticas de desenvolvimento. Outro ponto a ser considerado é que a legibilidade do código escrito nos pré-processadores é comprometida, uma vez que, se houver muitos aninhamentos de classes, a identificação do nome real do seletor é dificultada.

Visto isso, os casos a seguir são aqueles em que o uso deste seletor é recomendado, pois o código permance legível e o CSS final é claro e performático.

* Adição de classes modificadoras

**Exemplo**

> HTML
```
<div class="m-banner-hld active">
  /* Content */
</div>
```

> CSS
```
.m-banner-hld {
  display: none
  width: 500px;

  &.active {
    display: block;
  }
}
```

* Uso de pseudo elementos

> HTML
```
<div class="m-banner-hld">
  /* Content */
</div>
```

> CSS
```
.m-banner-hld {
  display: none
  width: 500px;

  &::before {
    content: '';
    display: block;
    border: 1px solid #f00;
    height: 20px;
    width: 20px;
  }
}
```

---


## Boas práticas