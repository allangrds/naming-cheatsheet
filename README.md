<p align="center">
  <a href="https://github.com/kettanaito/naming-cheatsheet">
    <img src="./naming-cheatsheet.png" alt="Naming cheatsheet" />
  </a>
</p>

# Cheatsheet de nomeclatura

- [Escreva em inglês](#escreva-em-inglês)
- [Padrão de nomenclatura](#padrão-de-nomenclatura)
- [C-I-D](#c-i-d)
- [Evite contrações](#evite-contrações)
- [Evite duplicar o contexto](#evite-duplicar-o-contexto)
- [Reflita o resultado esperado](#reflita-o-resultado-esperado)
- [Nomeando funções](#nomeando-funções)
  - [Padrão A/HC/LC](#padrao-ahclc)
    - [Ações](#ações)
    - [Contexto](#contexto)
    - [Prefixos](#prefixos)
- [Singular e Plural](#singular-e-plural)

---

Dar nome às coisas é uma tarefa difícil. Este documenta tenta tornar esta tarefa mais fácil.

Embora essas sugestões possam ser aplicadas a qualquer linguagem de programação, usarei JavaScript para ilustrá-las na prática.

## Escreva em inglês

Nomeie suas variáveis e funções em inglês.

```js
/* Ruim */
const primeiroNome = 'Gustavo'
const amigos = ['João', 'Raul']

/* Bom */
const firstName = 'Gustavo'
const friends = ['João', 'Raul']
```

> Goste ou não, o Inglês é a língua dominante na programação: a sintaxe de todas as linguagens de programação é escrita em Inglês, assim como inúmeras documentações e materiais educacionais. Ao escrever seu código em Inglês, você aumenta drasticamente sua coesão.

## Padrão de nomenclatura

Escolha **um** padrão de nomenclatura e siga o mesmo. Podendo ser `camelCase`, `PascalCase`, `snake_case`, ou qualquer outro que te agrade ou agrade a equipe, contando que seja consistente. Algumas linguaguens de programação tem seu próprio contrato sobre padrão de nomenclaturas; para isso, olhe a documentação da linguaguem e verifique repositórios populares no Github!

```js
/* Ruim */
const page_count = 5
const shouldUpdate = true

/* Bom */
const pageCount = 5
const shouldUpdate = true

/* Bom também */
const page_count = 5
const should_update = true
```

## C-I-D

Um nome deve ser _curto_, _intuitivo_ e _descritivo_:

- **Curto**. Um nome não deve demorar para ser digitado e, portanto, para ser lembrado;

- **Intuitivo**. Um nome deve ser lido de forma natural, o mais próximo do que se é usado comumente no idioma;

- **Descritivo**. Um nome deve refletir o que faz/possui da maneira mais eficiente.

```js
/* Ruim */
const a = 5 // "a" pode ser significar qualquer coisa
const isPaginatable = a > 10 // "Paginatable" não tem um som legal
const shouldPaginatize = a > 10 // Verbos compostos são muito divertidos!

/* Bom */
const postCount = 5
const hasPagination = postCount > 10
const shouldPaginate = postCount > 10 // alternativa
```

## Evite contrações

**Não** use contrações. Elas não contribuem para nada além de diminuir a legibilidade do código. Encontrar um nome curto e descritivo pode ser difícil, mas a contração não é desculpa para não fazê-lo.

```js
/* Ruim */
const onItmClk = () => {}

/* Bom */
const onItemClick = () => {}
```

## Evite duplicar o contexto

Um nome não deve duplicar o contexto em que está definido. Sempre remova o contexto de um nome se isso não diminuir sua legibilidade.

```js
class MenuItem {
  /* O nome do método duplica o contexto (que é "MenuItem") */
  handleMenuItemClick = (event) => { ... }

  /* Lemos tranquilamente como `MenuItem.handleClick()` */
  handleClick = (event) => { ... }
}
```

## Reflita o resultado esperado

Um nome deve refletir o resultado esperado.

```jsx
/* Ruim */
const isEnabled = itemCount > 3
return <Button disabled={!isEnabled} /> //estou verificando a negação da condição acima

/* Bom */
const isDisabled = itemCount <= 3
return <Button disabled={isDisabled} /> //estou verificando a condição acima
```

---

# Nomeando funções

## Padrão A/HC/LC

Existe um padrão muito útil pra se seguir quando precisar nomear funções:

```
prefixo? + ação (A) + alto (high) contexto (HC) + baixo (low) contexto? (LC)
```

Dê uma olhada em como este padrão pode ser aplicado na tabela abaixo.

| Nome                   | Prefixo  | Ação (A)   | Alto contexto(HC) | Baixo contexto(LC)|
| ---------------------- | -------- | ---------- | ----------------- | ----------------- |
| `getUser`              |          | `get`      | `User`            |                   |
| `getUserMessages`      |          | `get`      | `User`            | `Messages`        |
| `handleClickOutside`   |          | `handle`   | `Click`           | `Outside`         |
| `shouldDisplayMessage` | `should` | `Display`  | `Message`         |                   |

> **Nota:** A ordem do contexto afeta o significado de uma variável. Por exemplo, `shouldUpdateComponent` significa que _você_ está prestes a atualizar um componente, enquanto `shouldComponentUpdate` diz que o _componente_ será atualizado por si só, e você apenas está contrlando quando isto deve acontecer.
> Em outras palavras, **alto contexto enfatiza o significado de uma variável**.

---

## Ações

A parte verbal do nome de sua função. A parte mais importante responsável por descrever o que a função _faz_.

### `get`

Acessa o dado imediatamente (i.e. getter de dados internos abreviado).

```js
function getFruitCount() {
  return this.fruits.length
}
```

> Veja também [compose](#compose).

Você pode usar `get` quando disparar operações assíncronas tais como:

```js
async function getUser(id) {
  const user = await fetch(`/api/user/${id}`)
  return user
}
```

### `set`

Seta uma variável de forma declarativa, com valor `A` pro valor `B`.

```js
let fruits = 0

function setFruits(nextFruits) {
  fruits = nextFruits
}

setFruits(5)
console.log(fruits) // 5
```

### `reset`

Seta uma variável de volta ao seu valor ou estado iniciais.

```js
const initialFruits = 5
let fruits = initialFruits
setFruits(10)
console.log(fruits) // 10

function resetFruits() {
  fruits = initialFruits
}

resetFruits()
console.log(fruits) // 5
```

### `remove`

Remove algo _de_ algum lugar.

Por exemplo, se vc tem uma coleção de filtros selecionados em uma página de busca, remove um deles da coleção seria `removeFilter`, **não** `deleteFilter` (e isto é como naturalmente você diria em Inglês):

```js
function removeFilter(filterName, filters) {
  return filters.filter((name) => name !== filterName)
}

const selectedFilters = ['price', 'availability', 'size']
removeFilter('price', selectedFilters)
```

> Veja também [delete](#delete).

### `delete`

Apaga completamente algo dos reinos da existência.

Imagine que você é um editor de conteúdo, e existe um post notório que você quer se livrar. Uma vez que você clicar no botão brilhante "Deletar post", o CMS dispara uma ação de `deletePost`, **não** `removePost`.

```js
function deletePost(id) {
  return database.find({ id }).delete()
}
```

> Veja também [remove](#remove).

> **`remove` ou `delete`?**
>
> Quando a diferença entre `remove` e `delete` não é tão óbvia pra você, Sugiro que olhe pra suas ações opostas - `add` e `create`.
> A diferença chave entre `add` e `create` é que `add` precisa de um destino enquanto `create` **não precisa de um destino**. Você `add` (adiciona) um item _pra algum lugar_, mas você não cria "`create` em _algum lugar_".
> Simplesmente pareie `remove` com `add` e `delete` com `create`.
>
> Explicado em detalhes [aqui](https://github.com/kettanaito/naming-cheatsheet/issues/74#issue-1174942962).

### `compose`

Cria novo dado a partir de um existente. Aplicado na maioria das vezes pra strings, objetos, ou funções.

```js
function composePageUrl(pageName, pageId) {
  return pageName.toLowerCase() + '-' + pageId
}
```

> Veja também [get](#get).

### `handle`

Lida com uma ação. Muitas vezes usado quando precisamos nomear um método de callback (retorno).

```js
function handleLinkClick() {
  console.log('Clicked a link!')
}

link.addEventListener('click', handleLinkClick)
```

---

## Contexto

Um domínio no qual a função opera.

Uma função muitas das vezes é uma ação sobre _algo_. É importante declarar qual domínio operável é, ou ao menos um tipo de dado esperado.

```js
/* Uma função pura operando com primitivos */
function filter(list, predicate) {
  return list.filter(predicate)
}

/* Função operando exatamente com posts */
function getRecentPosts(posts) {
  return filter(posts, (post) => post.date === Date.now())
}
```

> Algumas suposições específicas de linguagens podem permitir omitir o contexto. Por exemplo, em JavaScript, é comum que um `filtro` opere em Arrays (vetores). Adicionar explicitamente `filterArray` seria desnecessário.

---

## Prefixos

Prefixar realça o significado de uma variável. É raramente usado em nomes de função.

### `is`

Descreve uma característica ou estado do contexto atual (usualmente `boolean`).

```js
const color = 'blue'
const isBlue = color === 'blue' // característica
const isPresent = true // estado

if (isBlue && isPresent) {
  console.log('Blue is present!')
}
```

### `has`

Descreve se o contexto atual possui um certo valor ou estado (usualmente `boolean`).

```js
/* Ruim */
const isProductsExist = productsCount > 0
const areProductsPresent = productsCount > 0

/* Bom */
const hasProducts = productsCount > 0
```

### `should`

Reflete uma declaração condicional positiva (usualmente `boolean`) casada com uma certa ação.

```js
function shouldUpdateUrl(url, expectedUrl) {
  return url !== expectedUrl
}
```

### `min`/`max`

Representa o valor mínimo ou máximo. Usado ao descrever limites.

```js
/**
 * Renderiza um valor aleatório de posts dentro de
 * um dado limite mínimo/máximo.
 */
function renderPosts(posts, minPosts, maxPosts) {
  return posts.slice(0, randomBetween(minPosts, maxPosts))
}
```

### `prev`/`next`

Indica o estado prévio ou posterior de uma variável no contexto atual. Usado ao descrever transição de estado.

```jsx
async function getPosts() {
  const prevPosts = this.state.posts

  const latestPosts = await fetch('...')
  const nextPosts = concat(prevPosts, latestPosts)

  this.setState({ posts: nextPosts })
}
```

## Singular e Plural

Tal qual um prefixo, nomes de variávels podem estar no singular ou no plural dependendo se elas contém um valor único ou múltiplos valores.

```js
/* Ruim */
const friends = 'Bob'
const friend = ['Bob', 'Tony', 'Tanya']

/* Bom */
const friend = 'Bob'
const friends = ['Bob', 'Tony', 'Tanya']
```
