# Estudo de GraphQL

> Repositório que reúne estudos baseados no livro **GraphQL - A revolucionária linguagem de consulta e manipulação de dados para APIs**, de Akira Hanashiro.

![Capa](https://martinsfontespaulista.vteximg.com.br/arquivos/ids/232675-320-320/882537.jpg)


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

# **Introdução ao GraphQL**

Sobre o por que de existir GraphQL, situações em que é vantajoso e noções gerais sobre GraphQL.

## **Conhecendo um novo mundo**

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

## **Conceitos básicos do GraphQL**

Definição de conceitos básicos do GraphQL, para que possamos iniciar nosso projeto na parte 2 desse repositório-tutorial.

### SDL - Schema Definition Language

É a representação de um objeto, modo pelo qual declaramos um esquema e tornamos conhecida a estrutura dos dados a serem consultados e/ou manipulados. Essa representação é usada como uma linguagem de programação ou framework, então declaramos os schemas GraphQL da mesma maneira para qualquer linguagem.

#### EXEMPLOS

**Type Aluno**

Exemplo de um schema que representa um aluno:

```gql
type Aluno {
  id: ID!
  nomeCompleto: String!
  idade: Int
}
```

O `type` é o elemento mais básico de um schema GraphQL. Representa os campos de um objeto e seus respectivos tipos. No exemplo acima estamos criando um `type` chamado `Aluno`. A estrutura de um dado de um aluno seguirá esse esquema, tendo os campos `id` e `nomeCompleto` como obrigatórios (note a `!` ao final da declaração).

**Type Curso**

Exemplo de um schema que representa um curso (que possui uma lista de alunos):

```gql
type Curso {
  id: ID!
  disciplina: String!
  alunos: [Aluno!]!
}
```

A estrutura de um `Curso` é bem semelhante ao de `Aluno`. Mas dessa vez, estamos usando o tipo `Aluno` dentro do tipo `Curso`. Os colchetes (`[]`) representam um array (e a `!` após o segundo colchete declara que o dado não pode ser `null`). E, dentro dos colchetes, a declaração `Aluno!` indica que cada item desse array deve ser do tipo `Aluno` (com `id`, `nomeCompleto` e  `idade`) e nunca pode ser `null`.

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

### Query - Consulta

É a operação de consulta. Requer que os esquemas tenham sido declarados previamente.

#### EXEMPLOS

**Query todosAlunos**

Exemplo de uma requisição de consulta (query) cujo retorno lista todos os alunos (trazendo seus `nomeCompleto`s e `curso`s, mas não o `id`):

```gql
query todosAlunos {
  nomeCompleto
  idade
}
```

No exemplo acima, `todosAlunos` seria o que chamamos de campo raiz (_rootField_). E as propriedades declaradas entre chaves (`{}`) são o _payload_. Os nomes dos campos do _payload_ (`id`, `nomeCompleto` e `curso`) são previamente definidos no back-end. A seguir, a resposta esperada (_response_) no formato `.json`:

```json
{
  "data": {
    "todosAlunos": [
      {
        "nomeCompleto": "Fulano da Silva",
        "idade": 30
      },
      {
        "nomeCompleto": "Ciclano da Silva",
        "idade": 18
      },
      {
        "nomeCompleto": "Beltrano da Silva",
        "idade": 60
      }
    ]
  }
}
```

**Query todosAlunos - com Campos Aninhados e Alias**

Um novo exemplo de query, dessa vez com campos aninhados (referentes ao curso de cada aluno - `id` e `disciplina`) e também com uso de _alias_ (dá-se um novo nome para a informação que será retornada - no caso, `disciplina` por `materia`):

```gql
todosAlunos {
  nomeCompleto
  idade
  curso {
    id
    materia: disciplina
  }
}
```

Perceba que é possível omitir a palavra `query`. Retorno esperado (`.json`):

```json
{
  "data": {
    "todosAlunos": [
      {
        "nomeCompleto": "Fulano da Silva",
        "idade": 30,
        "curso": {
          "id": 100,
          "materia": "ReactJS"
        }
      },
      {
        "nomeCompleto": "Ciclano da Silva",
        "idade": 18,
        "curso": {
          "id": 101,
          "materia": "GraphQL"
        }
      },
      {
        "nomeCompleto": "Beltrano da Silva",
        "idade": 60,
        "curso": null
      }
    ]
  }
}
```

A utilzação do _alias_ é muito útil por dar liberdade ao front-end renomear as propriedades sem demandar uma nova API ou a reestruturação do objeto retornado.

**Query todosAlunos - Query com Parâmetros**

Ainda é possível executar consultas com base em parâmetros (permitindo trabalhar dados dinâmicos no front-end). Assim como as estruturas dos dados, o back-end deve ser previamente preparado para receber essas variáveis (veremos mais adiante). Nesse caso a consulta recebe um parâmetro chamado `primeiros`, que deve receber um número (`Int`) e retornar essa quantidade de registros (alunos).

```gql
todosAlunos(primeiros: 2) {
  nomeCompleto
}
```

Veja que declaramos o parâmetro entre parênteses (`()`), com chave e valor (o valor deve corresponder ao tipo previsto no back-end). Retorno esperado (`.json`):

```json
{
  "data": {
    "todosAlunos": [
      {
        "nomeCompleto": "Fulano da Silva"
      },
      {
        "nomeCompleto": "Ciclano da Silva"
      }
    ]
  }
}
```

### Mutation - Criação, Atualização e Exclusão

É a operação de manipulação de dados. Requer que os esquemas tenham sido declarados previamente.

#### EXEMPLO

**Mutation criarAluno**

Exemplo de uma requisição de manipulação (mutation) que deve solicitar ao back-end que crie um novo aluno (conforme dados `nomeCompleto` e `idade`) cujo retorno lista todos os alunos (trazendo seus `nomeCompleto`s e `curso`s, mas não o `id`):

```gql
mutation {
  criarAluno(nomeCompleto: "Fulano de Tal", idade: 51) {
    id
  }
}
```

Perceba que além dos argumentos (`nomeCompleto` e `idade`, entre parênteses), passamos o `id` entre as chaves (`{}`). Nesse caso, o `id` não é criado por nós, mas sim pelo back-end. Declaramos o `id` como um dado que deve ser retornado. Resposta esperada (`.json`):

```json
{
  "data": {
    "criarAluno": {
      "id": "4"
    }
  }
}
```

> Uma boa prática é declarar as mutations começando com um verbo que descreve a ação realizada, no caso, `criarAluno`, `atualizarAluno` e `excluirAluno`.

Quanto às mutations `atualizarAluno` e `excluirAluno`, veremos mais adiante (mas seguem exatamente a mesma lógica e estrutura).

### Subscription - Notificação de Evento

É uma forma de mantermos nossa aplicação com dados atualizados em tempo real. Basicamente solicitamos ao servidor que nos avise - via WebSocket - quando uma determinada ação ocorrer.

#### EXEMPLO

**Subscription novoAluno**

Exemplo de uma subscription `novoAluno`, que solicita ao back-end que nos avise quando um novo aluno for criado:

```gql
subscription {
  novoAluno {
    nomeCompleto
    idade
  }
}
```

Da mesma forma que ocorre em uma query ou mutation, declaramos entre chaves (`{}`) os dados que desejamos receber. Resposta esperada (`.json`):

```json
{
  "novoAluno": {
    "nomeCompleto": "Fulano de Tal",
    "idade": 51
  }
}
```

> **WebScokets**: são uma forma de manter a conexão entre cliente e servidor ativa, permitindo a livre troca de dados entre ambos, nas duas direções.

### Schema - Esquema Completo

Da mesma forma que definimos os `type`s no nosso esquema, agora precisamos definir as operações relacionadas ao `type` (no caso, `Aluno`).

#### EXEMPLO

Considerando as operações realizadas e os tipos previamente declarados, teríamos o seguinte esquema:

```gql
# Tipo do objeto Aluno
type Aluno {
  id: ID!
  nomeCompleto: String!
  idade: Int
  curso: Curso
}

# Tipo do objeto Curso
type Curso {
  id: ID!
  disciplina: String!
  alunos: [Aluno!]!
}

# Tipo de Query (último exemplo, com parâmetro 'primeiros')
type Query {
  todosAlunos(primeiros: Int): [Aluno!]!
}

# Tipo de Mutation
type Mutation {
  criarAluno(nomeCompleto: String!, idade: Int): Aluno!
}

# Tipo de Subscription
type Subscription {
  novoAluno: Aluno!
}
```

Com isso completamos nossa visão geral sobre o que é GraphQL e seus principais conceitos.

___

# **GraphQL e Apollo Sandbox**

Hora de descer pro play! Mas antes, garanta que tem o node instalado em sua máquina.

## **Apollo Sandbox**

Acesse o repositório [CDC Livro GraphQL](https://github.com/hanashiro/cdc-livro-graphql.git) e clone para sua máquina com o seguinte comando: `git clone https://github.com/hanashiro/cdc-livro-graphql.git` (pasta `/cdc-livro-graphql-02` desse repositório).

Verá 3 pastas: `back`, `front` e `sandbox`. Acesse a pasta `sandbox` (`cd sandbox`), instale o graphql com (`npm i graphql`), instale as dependências (`npm install`), inicialize a aplicação (`npm start`) e abra seu navegador em `http://localhost:3000/graphql`.

Ao abrir o navegador, verá o sandbox do Apollo - é um ambiente onde podemos testar queries, mutations, subscriptions e esses comandos são executados no back-end. O sandbox é criado automaticamente quando se cria um servidor Apollo. Saiba mais em [apollographql.com](https://apollographql.com).

Perceba que todo o esquema do back-end está declarado no arquivo `./cdc-livro-graphql-02/sandbox/schema.js`:

```gql
const schema = `
  type Query {
    aluno(id: ID!): Aluno
    alunos(where: AlunosWhere): [Aluno]!
    curso(id: ID!): Curso
    cursos: [Curso]!
  }

  type Mutation {
    createAluno(data: AlunoInput!): Aluno
    deleteAluno(where: AlunoWhere!): Aluno
    updateAluno(where: AlunoWhere!, data: AlunoUpdate!): Aluno

    createCurso(data: CursoInput!): Curso
    deleteCurso(where: CursoWhere!): Curso
    updateCurso(where: CursoWhere!, data: CursoInput!): Curso
  }

  type Subscription {
    aluno(where: AlunoSubscriptionFilter!): AlunoSubscriptionPayload
    curso(where: CursoSubscriptionFilter!): CursoSubscriptionPayload
  }

  type Aluno{
    id: ID!
    nomeCompleto: String!
    idade: Int
    curso: Curso
  }

  type Curso{
    id: ID!
    disciplina: String!
    alunos: [Aluno!]!
  }

  input AlunoInput{
    nomeCompleto: String!
    idade: Int
    curso_create: CursoInput
    curso_connect: CursoWhere
    curso_disconnect: Boolean
  }

  input AlunoWhere{
    id: ID!
  }

  input AlunoUpdate{
    nomeCompleto: String
    idade: Int
    curso_connect: CursoWhere
    curso_disconnect: Boolean
  }

  input AlunosWhere{
    idade: Int
    idade_in: [Int!]
    idade_lt: Int
    idade_gt: Int
    idade_not: Int
    idade_lte: Int
    idade_gte: Int

    nomeCompleto: String
    nomeCompleto_ends_with: String
    nomeCompleto_starts_with: String
    nomeCompleto_not_contains: String
    nomeCompleto_not_ends_with: String
    nomeCompleto_not_starts_with: String

    first: Int
    skip: Int

    orderBy: String
  }

  input CursoInput{
    disciplina: String!
    alunos_create: [AlunoInput!]
    alunos_connect: [AlunoWhere!]
    alunos_disconnect: [AlunoWhere!]
  }

  input CursoWhere{
    id: ID!
  }

  input AlunoSubscriptionFilter {
    mutation_in: [ ModelMutationType! ]
  }

  input CursoSubscriptionFilter {
    mutation_in: [ ModelMutationType! ]
  }

  type AlunoSubscriptionPayload{
    mutation: ModelMutationType
    node: Aluno
    previousValues: Aluno
  }

  type CursoSubscriptionPayload{
    mutation: ModelMutationType
    node: Curso
    previousValues: Curso
  }

  enum ModelMutationType{
    CREATED
    UPDATED
    DELETED
  }
`;

module.exports = schema;
```

## **Busca e Alteração de Dados**

Vamos por em prática tudo o que vimos até o momento. Lembrando que nessa etapa estamos apenas executando os comandos - nosso back-end já está pronto.

Ah! Passaremos a passar os argumentos em formato de variáveis (`.json`), nos aproximando mais de como se usa o GraphQL na prática.

### Criar Aluno

Na aba `ExampleQuery` (que já aparece aberta ao acessar o Sandbox), vamos inserir nosso comando na área `Operation` e os valores, em formato de `.json` na área `Variables`. Por fim, clicamos em `ExamplQuery` (o botão azul "Run", no canto superior direito).

**Comando:**

```gql
mutation Mutation($data: AlunoInput!) {
  createAluno(data: $data) {
    id
    nomeCompleto
  }
}
```

**Variables**

```json
{
  "data": {
    "nomeCompleto": "Fulano de Tal",
    "idade": 55
  }
}
```

Isso seria o mesmo que:

```gql
mutation Mutation($data: AlunoInput!) {
  createAluno(data: {
    nomeCompleto: "Fulano de Tal",
    idade: 55
  }) {
    id
    nomeCompleto
  }
}
```

A diferença é que definimos uma variável e seu tipo (`($data: AlunoInput)`) e então atribuímos a variável ao argumento (`data: $data`). Por fim, definimos o valor para a variável (como fizemos em `Variables`). O resultado é o seguinte:

```json
{
  "data": {
    "createAluno": {
      "id": "1702945967660",
      "nomeCompleto": "Fulano de Tal"
    }
  }
}
```

### Busca Simples (Listagem)

Agora vamos listar todos os alunos.

**Comando:**

```gql
query ExampleQuery {
  alunos {
    id
    nomeCompleto
    idade
  }
}
```

**Retorno (sucesso):**

```json
{
  "data": {
    "alunos": [
      {
        "id": "1702945967660",
        "nomeCompleto": "Fulano de Tal",
        "idade": 55
      }
    ]
  }
}
```

**Retorno (erro):**

Caso não haja nenhum aluno registrado, precisamos receber um retorno - no caso, um array vazio:

```json
{
  "data": {
    "alunos": []
  }
}
```

### Atualização de Registro

Para atualizarmos um registro, precisamos antes identificá-lo. Há uma mutation preparada no nosso back-end para receber uma operação que atualiza um aluno a partir de seu `id` (seu identificador único). Vamos lá!

**Comando:**

```gql
mutation ($id: ID!, $idade: Int) {
  updateAluno(data: {idade: $idade}, where :{id: $id}) {
    id
    nomeCompleto
    idade
  }
}
```

Perceba que não demos um nome para nossa mutation, mas tipificamos nossos argumentos passados como variáveis (sendo o `id` do tipo `ID` e obrigatório e `idade` do tipo `Int`). Então definimos os valores de nossas variáveis em um `.json` (pegamos o valor do `id` ao criar o novo aluno):

```json
{
  "id": "1702945967660",
  "idade": 33
}
```

**Retorno:**

```json
{
  "data": {
    "updateAluno": {
      "id": "1702945967660",
      "nomeCompleto": "Fulano de Tal",
      "idade": 33
    }
  }
}
```

### Exclusão de Registro

Para apagarmos um registro, também precisamos antes identificá-lo - mas não há dados adicionais a serem enviados. Ainda que a gente vá apagar o registro, precisamos declarar algum retorno (nesse caso, é nossa última chance de exibir esses dados).

**Comando:**

```gql
mutation ($id: ID!) {
  deleteAluno(where: {id: $id}) {
    id
    nomeCompleto
    idade
  }
}
```

Perceba que não demos um nome para nossa mutation, mas tipificamos nossos argumentos passados como variáveis (sendo o `id` do tipo `ID` e obrigatório e `idade` do tipo `Int`). Então definimos os valores de nossas variáveis em um `.json` (pegamos o valor do `id` ao criar o novo aluno):

```json
{
  "id": "1702945967660"
}
```

**Retorno (sucesso):**

```json
{
  "data": {
    "alunos": [
      {
        "id": "1702945951841",
        "nomeCompleto": "Fulano de Tal",
        "idade": 55
      }
    ]
  }
}
```

**Retorno (erro):**

Caso não exista um registro de `id` compatível, recebemos `data: null`:

```json
{
  "data": {
    "deleteAluno": null
  }
}
```

### Relacionamento de Dados

Considerando o exemplo de alunos e cursos, há três possíveis operações no nosso projeto:

- Criar um aluno e relacionar um novo curso;
- Criar um aluno e relacionar um curso existente;
- Relacionar um curso existente a um aluno existente;
- Remover o relacionamento entre um aluno e um curso.

#### **Criar um aluno e relacionar um novo curso:**

Começaremos criando um novo Aluno e atribuindo um novo curso a ele, seguindo o mesmo padrão já visto. O único ponto de atenção é que devemos indicar que queremos criar um aluno (fazemos isso declarando a propriedade `create_aluno`):

**Operação:**

```gql
mutation CriarAlunoECriarCurso($nomeCompleto: String!, $disciplina: String!, $idade: Int) {
  createAluno(data: {
    nomeCompleto: $nomeCompleto
    idade: $idade
    curso_create: {
      disciplina: $disciplina
    }
  }) {
    id
    nomeCompleto
    idade
    curso {
      id
      disciplina
    }
  }
}
```

**Variáveis:**

```json
{
  "nomeCompleto": "Joselito Sem Noção",
  "idade": 22,
  "disciplina": "GraphQL"
}
```

**Retorno:**

```json
{
  "data": {
    "createAluno": {
      "id": "1702949884272",
      "nomeCompleto": "Joselito Sem Noção",
      "idade": 22,
      "curso": {
        "id": "1702949884272",
        "disciplina": "GraphQL"
      }
    }
  }
}
```

Podemos checar a criação do Aluno e do Curso com as seguintes queries:

**Alunos**

```gql
query ListarAlunos {
  alunos {
    id
    nomeCompleto
    idade
    curso {
      disciplina
    }
  }
}
```

**Cursos**

```json
{
  "data": {
    "alunos": [
      {
        "id": "1702949884272",
        "nomeCompleto": "Joselito Sem Noção",
        "idade": 22,
        "curso": {
          "disciplina": "GraphQL"
        }
      }
    ]
  }
}
```

```gql
query ListarCursos {
  cursos {
    id
    disciplina
    alunos {
      id
      nomeCompleto
      idade
    }
  }
}
```

```json
{
  "data": {
    "cursos": [
      {
        "id": "1702949884272",
        "disciplina": "GraphQL",
        "alunos": [
          {
            "id": "1702949884272",
            "nomeCompleto": "Joselito Sem Noção",
            "idade": 22
          }
        ]
      }
    ]
  }
}
```

#### **Criar um aluno e relacionar um curso existente:**

**Operação:**

```gql
mutation CriarAlunoEConectarCurso( $nomeCompleto: String!, $idade: Int, $disciplinaId: ID!) {
  createAluno(data: {
    nomeCompleto: $nomeCompleto
    idade: $idade
    curso_connect: {
      id: $disciplinaId
    }
  }) {
    id
    nomeCompleto
    idade
    curso {
      id
      disciplina
    }
  }
}
```

**Variáveis:**

```json
{
  "nomeCompleto": "Homer Simpson",
  "idade": 50,
  "disciplinaId": "1702949884272"
}
```

**Retorno:**

```json
{
  "data": {
    "createAluno": {
      "id": "1702950859185",
      "nomeCompleto": "Homer Simpson",
      "idade": 50,
      "curso": {
        "id": "1702949884272",
        "disciplina": "GraphQL"
      }
    }
  }
}
```

#### **Relacionar um curso existente a um aluno existente:**

**Operação 01 - Criar Aluno:**

```gql
mutation CriarAluno($nomeCompleto: String!, $idade: Int, $disciplina: String!) {
  createAluno(data: {
    nomeCompleto: $nomeCompleto
    idade: $idade
  }) {
    id
    nomeCompleto
    idade
  }
}
```

**Variáveis:**

```json
{
  "nomeCompleto": "Irmão do Jorel",
  "idade": 8
}
```

**Retorno:**

```json
{
  "data": {
    "createAluno": {
      "id": "1702951062431",
      "nomeCompleto": "Irmão do Jorel",
      "idade": 8
    }
  }
}
```

**Operação 02 - Relacionar Curso:**

```gql
mutation ConectarAlunoECurso($aluno_id: ID!, $curso_id: ID!) {
  updateAluno(
    where: {id: $aluno_id},
    data: {
      curso_connect: {
        id: $curso_id
      }
    }
  ) {
    id
    nomeCompleto
    idade
    curso {
      id
      disciplina
    }
  }
}
```

**Variáveis:**

```json
{
  "aluno_id": 1702951062431,
  "curso_id": 1702949884272
}
```

**Retorno:**

```json
{
  "data": {
    "updateAluno": {
      "id": "1702951062431",
      "nomeCompleto": "Irmão do Jorel",
      "idade": 8,
      "curso": {
        "id": "1702949884272",
        "disciplina": "GraphQL"
      }
    }
  }
}
```

#### **Remover o relacionamento entre um aluno e um curso:**

Partindo do pressuposto que só há um curso por aluno, nesse caso basta passarmos a instrução `curso_disconnect: true`, como podemos ver a seguir. Também podemos declarar o valor `true` diretamente nos argumentos, afinal é a única opção disponível.

**Operação:**

```gql
mutation DesconectarAlunoECurso($aluno_id: ID!) {
  updateAluno(
    where: {id: $aluno_id},
    data: {
      curso_disconnect: true
    }
  ) {
    id
    nomeCompleto
    idade
    curso {
      id
      disciplina
    }
  }
}
```

**Variáveis:**

```json
{
  "aluno_id": 1702951062431
}
```

**Retorno:**

```json
{
  "data": {
    "updateAluno": {
      "id": "1702951062431",
      "nomeCompleto": "Irmão do Jorel",
      "idade": 8,
      "curso": null
    }
  }
}
```

### Busca (Registro Específico)

Já vimos como buscar uma lista (`alunos`). Agora buscaremos por um registro específico.

**Operação:**

```gql
query BuscarAluno ($aluno_id: ID!) {
  aluno(id: $aluno_id) {
    nomeCompleto
    idade
  }
}
```

**Variáveis:**

```json
{
  "aluno_id": "1702951062431"
}
```

**Retorno (sucesso):**

```json
{
  "data": {
    "aluno": {
      "nomeCompleto": "Irmão do Jorel",
      "idade": 8
    }
  }
}
```

**Retorno (erro):**

```json
{
  "data": {
    "aluno": null
  }
}
```

### Variáveis

Já estamos usando as variáveis, mas vamos recapitular como funcionam. Primeiro, vamos ver uma query sem o uso de variáveis:

```gql
query BuscarAluno {
  aluno(id: "1702951062431") {
    nomeCompleto
    idade
  }
}
```
Perceba que simplesmente indicamos o ID desejado diretamente na query (como se fosse _hard coded_, "na unha").

Mas no dia a dia, o valor passado deve ser dinâmico, respondendo ao front-end. E aí entram as variáveis. Veja a diferença:

```gql
query BuscarAluno ($aluno_id: ID!) {
  aluno(id: $aluno_id) {
    nomeCompleto
    idade
  }
}
```

```json
{
  "aluno_id": "1702951062431"
}
```

Na primeira linha, indicamos que iremos usar a variável `$aluno_id` e que seu tipo deve ser `ID!` (conforme a tipagem definida para o `id`, sendo obrigatório):

`query BuscarAluno ($aluno_id: ID!) {`

Então atribuímos o valor dessa variável ao `id` (usado como parâmetro para a busca a ser feita):

`aluno(id: $aluno_id) {`

Por fim, definimos um valor para a variável (na aba Variables, simulando um dado inserido no front-end):

`{ "aluno_id": "1702951062431" }`

> Usando variáveis temos **comandos estáticos e dados dinâmicos**.

### Filtros

Definidos por parâmetros, os filtros nada mais são do que uma forma de filtrar as respostas desejadas (por exemplo, retornar uma lista de alunos que tenham mais de 21 anos). No caso do nosso Sandbox, basta usarmos o campo `where` para indicar qual o campo a ser usado como parâmetro e seu valor.

**Filtro por idade**

Listando alunos com exatamente 50 anos:

```gql
# Query
query ListarAlunosComVinteUmAnos ($where: AlunosWhere) {
  alunos (where: $where) {
    id
    nomeCompleto
    idade
    curso {
      disciplina
    }
  }
}
```

```json
// Variáveis
{
  "where": {
    "idade": 50
  }
}
```
Veja que dessa vez passamos um objeto `where` como variável. Seu valor foi previamente tipado (`AlunosWhere`). E, nosso retorno é o seguinte:

```json
// Retorno
{
  "data": {
    "alunos": [
      {
        "id": "1702950859185",
        "nomeCompleto": "Homer Simpson",
        "idade": 50,
        "curso": {
          "disciplina": "GraphQL"
        }
      }
    ]
  }
}
```

> Dica: você já deve ter percebido que o Sandbox já sugere termos. Pressione Control + Espaço para visualizar e acatar as sugestões.

**Outros Filtros**

Esse projeto possui mais uma série de outros filtros prontos (onde `gt` significa "greater than"/"maior que", `lt` significa "less than"/"menor que", `lte` é "menor ou igual a" e `gte` é "maior ou igual a").

| Campo          | Filtros                                                                                                                                                           |
| -------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `nomeCompleto` | `nomeCompleto`, `nomeCompleto_ends_with`, `nomeCompleto_starts_with`, `nomeCompleto_not_contains_with`, `nomeCompleto_not_ends_with` e `nomeCompleto_starts_with` |
| `idade`        | `idade`, `idade_in`, `idade_lt`, `idade_gt`, `idade_not`, `idade_lte` e `idade_gte`                                                                               |

Exemplos:

```gql
# Query
query ListarAlunosComFiltros ($where: AlunosWhere) {
  alunos (where: $where) {
    nomeCompleto
    idade
  }
}
```

**Menor que 50 anos**

```json
// Variáveis
{
  "where": {
    "idade_lt": 50
  }
}
```

```json
//Retorno
{
  "data": {
    "alunos": [
      {
        "nomeCompleto": "Joselito Sem Noção",
        "idade": 22
      },
      {
        "nomeCompleto": "Irmão do Jorel",
        "idade": 8
      }
    ]
  }
}
```

**Menor ou igual a 50 anos**

```json
// Variáveis
{
  "where": {
    "idade_lte": 50
  }
}
```

```json
// Retorno
{
  "data": {
    "alunos": [
      {
        "nomeCompleto": "Joselito Sem Noção",
        "idade": 22
      },
      {
        "nomeCompleto": "Homer Simpson",
        "idade": 50
      },
      {
        "nomeCompleto": "Irmão do Jorel",
        "idade": 8
      }
    ]
  }
}
```

**Nome que começa com "Jo"**

```json
// Variáveis
{
  "where": {
    "nomeCompleto_starts_with": "Jo"
  }
}
```

```json
// Retorno
{
  "data": {
    "alunos": [
      {
        "nomeCompleto": "Joselito Sem Noção",
        "idade": 22
      }
    ]
  }
}
```

**Nome que não começa com "Jo"**

```json
// Variáveis
{
  "where": {
    "nomeCompleto_not_starts_with": "Jo"
  }
}
```

```json
// Retorno
{
  "data": {
    "alunos": [
      {
        "nomeCompleto": "Homer Simpson",
        "idade": 50
      },
      {
        "nomeCompleto": "Irmão do Jorel",
        "idade": 8
      }
    ]
  }
}
```

### Paginação

Quando tratamos um volume muito grande de dados, é uma boa prática usar a paginação. Para isso, vamos usar dois argumentos dentro de `where`, o `first` (define o número de registros que devem ser retornados) e o `skip` (como um "offset", define quantos elementos devemos 'pular').

Para visualizarmos a paginação funcionando, criamos os seguintes alunos e cursos:

```json
{
  "data": {
    "alunos": [
      {
        "id": "1703025631771",
        "nomeCompleto": "Fulano da Silva",
        "idade": 20,
        "curso": {
          "disciplina": "GraphQL"
        }
      },
      {
        "id": "1703025645664",
        "nomeCompleto": "Ciclano da Silva",
        "idade": 30,
        "curso": {
          "disciplina": "ReactJS"
        }
      },
      {
        "id": "1703025657874",
        "nomeCompleto": "Beltrano da Silva",
        "idade": 40,
        "curso": {
          "disciplina": "NodeJS"
        }
      },
      {
        "id": "1703025682501",
        "nomeCompleto": "Huguinho Patinhas",
        "idade": 50,
        "curso": {
          "disciplina": "HTML"
        }
      },
      {
        "id": "1703025691810",
        "nomeCompleto": "Zezinho Patinhas",
        "idade": 55,
        "curso": {
          "disciplina": "CSS"
        }
      },
      {
        "id": "1703025702825",
        "nomeCompleto": "Luizinho Patinhas",
        "idade": 60,
        "curso": {
          "disciplina": "JS"
        }
      }
    ]
  }
}
```

**Uso do `first`**

```gql
# Query
query ListarAlunos($first: Int) {
  alunos (where: {
    first: $first
  }) {
    nomeCompleto
  }
}
```

```json
// Variáveis
{
  "first": 3
}
```

```json
// Response
{
  "data": {
    "alunos": [
      {
        "nomeCompleto": "Fulano da Silva"
      },
      {
        "nomeCompleto": "Ciclano da Silva"
      },
      {
        "nomeCompleto": "Beltrano da Silva"
      }
    ]
  }
}
```

**Uso do `first` com `skip`**

```gql
# Query
query ListarAlunos($first: Int, $skip: Int) {
  alunos (where: {
    first: $first
    skip: $skip
  }) {
    nomeCompleto
  }
}
```

```json
// Variáveis
{
  "first": 3,
  "skip": 2
}
```

```json
// Response
{
  "data": {
    "alunos": [
      {
        "nomeCompleto": "Beltrano da Silva"
      },
      {
        "nomeCompleto": "Huguinho Patinhas"
      },
      {
        "nomeCompleto": "Zezinho Patinhas"
      }
    ]
  }
}
```

### Ordenação

Mais um argumento para facilitar nossa vida - a possibilidade de ordernar os registros retornados a partir de um dos campos (no nosso exemplo, `nomeCompleto`).

**Uso do `orderBy`**

```gql
# Query
query ListarAlunos($orderBy: String) {
  alunos (where: {
    orderBy: $orderBy
  }) {
    nomeCompleto
  }
}
```

```json
// Variáveis
{
  "orderBy": "nomeCompleto"
}
```

```json
// Response
{
  "data": {
    "alunos": [
      {
        "nomeCompleto": "Beltrano da Silva"
      },
      {
        "nomeCompleto": "Ciclano da Silva"
      },
      {
        "nomeCompleto": "Fulano da Silva"
      },
      {
        "nomeCompleto": "Huguinho Patinhas"
      },
      {
        "nomeCompleto": "Luizinho Patinhas"
      },
      {
        "nomeCompleto": "Zezinho Patinhas"
      }
    ]
  }
}
```

### Alias (Renomeando Campos)

Por vezes, precisamos receber os dados no front-end com nomes diferentes dos seus nomes originais. Para isso, podemos utilizar o recurso de renomeação ("alias"). Basta inserir o nome desejado e dois pontos (`nomeNovo: `) antes do campo a ser renomeado.

```gql
# Query
query ListarAlunos {
  alumni: alunos (where: {
    first: 2
  }) {
    fullName: nomeCompleto
    age: idade
  }
}
```

```json
// Response
{
  "data": {
    "alumni": [
      {
        "fullName": "Fulano da Silva",
        "age": 20
      },
      {
        "fullName": "Ciclano da Silva",
        "age": 30
      }
    ]
  }
}
```

Repare que renomeamos todos os campos retornados (`alunos` para `alumni`, `nomeCompleto` para `fullName` e `idade` para `age`).

Outra grande vantagem de utilizarmos `alias` é podermos executar duas queries iguais (com parâmetros diferentes) de uma só vez. Veja:

```gql
# Query
query ListarAlunos {
  alunosPorNome: alunos (where: {
    orderBy: "nomeCompleto",
    first: 3
  }) {
    nomeCompleto
    idade
  }
  alunosPorIdade: alunos (where: {
    orderBy: "idade",
    first: 3
  }) {
    nomeCompleto
    idade
  }
}
```

```json
// Response
{
  "data": {
    "alunosPorNome": [
      {
        "nomeCompleto": "Beltrano da Silva",
        "idade": 40
      },
      {
        "nomeCompleto": "Ciclano da Silva",
        "idade": 30
      },
      {
        "nomeCompleto": "Fulano da Silva",
        "idade": 20
      }
    ],
    "alunosPorIdade": [
      {
        "nomeCompleto": "Fulano da Silva",
        "idade": 20
      },
      {
        "nomeCompleto": "Ciclano da Silva",
        "idade": 30
      },
      {
        "nomeCompleto": "Beltrano da Silva",
        "idade": 40
      }
    ]
  }
}
```

### Fragmentos

Você já deve ter ouvido falar sobre DRY - Don't Repeat Yourself ("Não Seja Repetitivo"). O uso de fragmentos é muito útil para evitar códigos duplicados e centralizar o controle desses campos. Os fragmentos nada mais são do que uma forma de agrupar campos. Dessa forma, não precisamos duplicar código e, em caso de alterações, não corremos o risco de esquecer de alterar o código em um local (pois centralizamos os campos no fragmento).

Tudo o que precisamos fazer é declarar o fragmento, indicar o esquema ao qual está atrelado. No exemplo a seguir criamos dois fragmentos - `dadosCadastrais` (que incluem `nomeCompleto` e `idade`) e `dadosAcademicos` (inclui `curso {disciplina, id}`).

```gql
fragment dadosCadastrais on Aluno {
  nomeCompleto
  idade
}

fragment dadosAcademicos on Aluno {
  curso {
    id
    disciplina
  }
}
```

Uma vez criado o fragmento, podemos usar a seguinte sintaxe: `...meuFragmento`, isso trará os campos definidos no seu fragmento (como o _spreadOperator_). Veja a diferença entre as duas versões da mesma query - uma com e outra sem os fragmentos.

```gql
# Sem Fragmentos
query ListarAlunos {
  alunos (where: {
    first: 3
  }) {
    nomeCompleto
    idade
    curso {
      id
      disciplina
    }
  }
}

# Com Fragmentos
query ListarAlunos {
  alunos (where: {
    first: 3
  }) {
    ...dadosCadastrais
    ...dadosAcademicos
  }
}
```

Imagine quanta repetição não economizamos usando os fragmentos! Lembre-se de um fragmento só pode ser usado dentro do esquema ao qual está atrelado.

### Notificações

A.

## **Types**

### Types

### Scalar Types

#### `Int`

#### `Float`

#### `String`

#### `Boolean`

#### `ID`

### Enumeration Types (ENUM)

### Input Types

___

# **Criando uma aplicação**

A.

## **Desenvolvimento Front-end**

A.

___

# **Programando um Servidor**

A.

## **Desenvolvimento Back-end**

A.

___

# Notas de Rodapé

A.
