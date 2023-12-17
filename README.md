# Estudo de GraphQL

![Capa](https://martinsfontespaulista.vteximg.com.br/arquivos/ids/232675-320-320/882537.jpg)

Repositório que reúne estudos baseados no livro **GraphQL - A revolucionária linguagem de consulta e manipulação de dados para APIs**, de Akira Hanashiro.

| Título  | **GraphQL - A revolucionária linguagem de consulta e manipulação de dados para APIs** |
| ------- | ------------------------------------------------------------------------------------- |
| Autoria | Akira Hanashiro                                                                       |
| Edição  | Vivian Matsui                                                                         |
| Revisão | Antonio Pedro Loureiro                                                                |
| Editora | Casa do Código                                                                        |
| Ano     | 2023                                                                                  |
| ISBN    | 978-85-7254-011-7                                                                     |
| EPUB    | 978-85-7254-012-4                                                                     |
| MOBI    | 978-85-7254-013-1                                                                     |

## Sumário

- Parte 1 - **Introdução ao GraphQL**
  - Tópico 1: **Conhecendo um novo mundo**
  - Tópico 2: **Conceitos básicos do GraphQL**
- Parte 2 - **GraphQL e Apollo Sandbox**
  - Tópico 3: **Apollo Sandbox**
  - Tópico 4: **Busca e Alteração de Dados**
  - Tópico 5: **Types**
- Parte 3 - **Criando uma aplicação**
  - Tópico 6: **Desenvolvimento Front-end**
- Parte 4 - **Programando um Servidor**
  - Tópico 7: **Desenvolvimento Back-end**

___

# Parte 1 - **Introdução ao GraphQL**

Sobre o por que de existir GraphQL, situações em que é vantajoso e noções gerais sobre GraphQL.

## Tópico 1: **Conhecendo um novo mundo**

Criado pelo Facebook (atual Meta) em 2012, o GraphQL existe para resolver problemas que exigem muitas 'voltas' para serem resolvidos com REST (prejudicando a performance das aplicações). Trata-se de uma linguagem de consulta e manipulação de dados, assim como o SQL.

Mas utilizar GraphQL nos permite receber somente dados que realmente precisamos naquele contexto, etapa e cenário. Além disso, o uso do GraphQL pode ser útil em aplicações que consomem diferentes tipos de bancos de dados (SQL, MongoDB e Redis).

Atualmente o GraphQL é um projeto da GraphQL Foundation hospedado pela Linux Foundation e mantido por empresas como Meta (Facebook), Atlassian, AWS, IBM, Microsoft, Spotify, Airbnb, PayPal, Prisma, entre outras.

É possível usar GraphQL em qualquer cenário onde haja comunicação entre cliente e servidor via API - seja web, desktop ou mobile. O GraphQL possibilita um uso versátil, com nomeação de campos e seleção de dados a receber. Mas devemos saber quando e para quê utilizar o GraphQL.

**Pontos Fortes**

- **Schema** | Ao declararmos a estrutura do dado, o cliente sempre sabe o que deve esperar receber - além de gerar automaticamente uma documentação e a validação de tipos.

- **Tudo em uma única requisição** | É possível receber todos os dados necessários através de uma única requisição.

- **Nem mais, nem menos** | O back-end deixa todos os dados estruturados e o front-end consome esses dados 'à la carte' (escolhe exatamente que dados deve receber).

- **Desenvolvimento rápido** | Há uma série de bibliotecas prontas com recursos como Cache, Atualização da Interface e Atualização em Tempo Real com WebSockets.

- **Criação de APIs de forma rápida e simples** | Uma única API pode entregas dados ao cliente com bem pouco código (como veremos nesse repositório-tutorial).

Mas... a depender da complexidade e níveis da consulta, pode haver um grande impacto no servidor.

**Pontos de Atenção**

Pontos que estão sendo estudados mas não foram resolvidos até o momento.

- **Avaliação de Limites** | Limitar, restringir a quantidade de requisições pode ser um desafio no GraphQL, pois uma única requisição pode ser leve ou pesada.

- **Cache** | Por termos uma única API no GraphQL, não conseguimos diferenciar cache por URL.

## Tópico 2: **Conceitos básicos do GraphQL**

Definição de conceitos básicos do GraphQL, para que possamos iniciar nosso projeto na parte 2 desse repositório-tutorial.

### SDL - Schema Definition Language

É a representação de um objeto, modo pelo qual declaramos um esquema e tornamos conhecida a estrutura dos dados a serem consultados e/ou manipulados. Essa representação é usada como uma linguagem de programação ou framework, então declaramos os schemas GraphQL da mesma maneira para qualquer linguagem.

#### EXEMPLO

**Type Aluno**

Exemplo de um schema que representa um aluno:

```gql
type Aluno {
  id: ID!
  nomeCompleto: String!
  idade: Int
}
```

O `type` é o elemento mais básico de um schema GraphQL. Representa os campos de um objeto e seus respectivos tipos.

No exemplo acima estamos criando um `type` chamado `Aluno`. A estrutura de um dado de um aluno seguirá esse esquema, tendo os campos `id` e `nomeCompleto` como obrigatórios (note a `!` ao final da declaração).

**Type Curso**

Exemplo de um schema que representa um curso (que possui uma lista de alunos):

```gql
type Curso {
  id: ID!
  disciplina: String!
  alunos: [Aluno!]!
}
```

A estrutura de um `Curso` é bem semelhante ao de `Aluno`. Mas dessa vez, estamos usando o tipo `Aluno` dentro do tipo `Curso`.

Os colchetes (`[]`) representam um array (e a `!` após o segundo colchete declara que o dado não pode ser `null`).

E, dentro dos colchetes, a declaração `Aluno!` indica que cada item desse array deve ser do tipo `Aluno` (com `id`, `nomeCompleto` e  `idade`) e nunca pode ser `null`.

**Vinculando um Curso a um Aluno**

A seguir, declaramos que um aluno pode estar matriculado em até um (1) curso (note que o aluno também pode não estar matriculado em nenhum curso).

```gql
type Aluno {
  id: ID!
  nomeCompleto: String!
  idade: Int
  curso: Curso
}
```

___

# Parte 2 - **GraphQL e Apollo Sandbox**

A.

## Tópico 3: **Apollo Sandbox**

A.

## Tópico 4: **Busca e Alteração de Dados**

A.

## Tópico 5: **Types**

A.

___

# Parte 3 - **Criando uma aplicação**

A.

## Tópico 6: **Desenvolvimento Front-end**

A.

___

# Parte 4 - **Programando um Servidor**

A.

## Tópico 7: **Desenvolvimento Back-end**

A.

___

# Notas de Rodapé

A.
