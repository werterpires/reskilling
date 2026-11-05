# Tópico 2 — Entender a mudança procedural → orientação a objetos

## Você não está começando novamente

Ao passar de COBOL para Java, a primeira diferença visível é a sintaxe. Surgem chaves, pontos, palavras em inglês, nomes escritos em outro padrão e arquivos distribuídos em pacotes. Essa camada nova pode dar a impressão de que toda a experiência anterior perdeu valor. Não perdeu.

Você já sabe interpretar processos, identificar dados relevantes, acompanhar o caminho de uma informação, analisar impacto e proteger regras de negócio. Isso continua sendo trabalho de desenvolvimento de software. Java não elimina essas competências; ele pede que você as use dentro de outra forma de organizar o programa.

Em uma abordagem procedural, é natural começar pela sequência:

```text
localizar usuário
localizar livro
validar empréstimo
registrar operação
exibir confirmação
```

Na orientação a objetos, essa sequência continua existindo, mas uma pergunta vem antes:

> Quais elementos participam desse processo e qual responsabilidade pertence a cada um?

Na Gestão de Biblioteca, os elementos mais evidentes são `Livro`, `Usuario`, `Emprestimo` e `Biblioteca`. Em Java, esses conceitos podem ser representados por classes; ocorrências concretas deles podem ser representadas por objetos; suas informações podem ser representadas por atributos; e cada informação precisa ter um tipo.

Este tópico é introdutório porque apresenta os fundamentos. Isso não significa passar rapidamente por eles. A meta é compreender o significado de cada conceito e construir pontes cuidadosas com COBOL, sem fingir que as duas linguagens são equivalentes.

Não estudaremos decisões, repetições, entrada de dados nem algoritmos. Nosso foco será a estrutura do programa.

## O que você compreenderá

- por que COBOL é um bom ponto de partida;
- por que Java não é apenas COBOL com palavras diferentes;
- como fluxo e responsabilidade se complementam;
- o que são classes, objetos, atributos e tipos;
- as diferenças entre `PIC X`, `PIC 9`, indicadores COBOL e tipos Java;
- a diferença entre tipos primitivos e tipos por referência;
- por que dinheiro e datas merecem tipos próprios;
- como pacotes organizam e identificam classes;
- o que um `import` faz — e o que ele não faz;
- como essas ideias aparecem em uma aplicação de Gestão de Biblioteca.

---

## 1. COBOL como ponto de partida

COBOL torna muito visível a separação entre a descrição dos dados e o fluxo de processamento. Em uma forma tradicional, a `DATA DIVISION` declara estruturas e a `PROCEDURE DIVISION` organiza ações.

Um livro poderia ser descrito assim:

```cobol
01 WS-LIVRO.
   05 WS-LIVRO-ISBN             PIC X(13).
   05 WS-LIVRO-TITULO           PIC X(100).
   05 WS-LIVRO-NUMERO-PAGINAS   PIC 9(5).
   05 WS-LIVRO-DISPONIVEL       PIC X(1).
      88 LIVRO-DISPONIVEL       VALUE 'S'.
      88 LIVRO-INDISPONIVEL     VALUE 'N'.
```

E um processo poderia ser organizado em parágrafos:

```cobol
PROCEDURE DIVISION.
    VALIDAR-USUARIO.
    VALIDAR-LIVRO.
    REGISTRAR-EMPRESTIMO.
    EXIBIR-CONFIRMACAO.
```

O primeiro trecho descreve dados. O segundo destaca ações em uma sequência. Essa leitura continua útil em Java: um empréstimo ainda precisa acontecer em alguma ordem, e os dados ainda precisam ser descritos.

A diferença é que Java permite aproximar os dados de um conceito e as responsabilidades relacionadas a ele. Em vez de concentrar a descrição dos dados em uma grande área e todo o processamento em outra, o sistema pode ser organizado em classes como `Livro`, `Usuario` e `Emprestimo`.

Isso não significa que COBOL seja necessariamente desorganizado, nem que todo programa procedural seja um bloco único. Sistemas COBOL podem possuir programas, subprogramas, copybooks, seções e padrões rigorosos. A mudança está no modelo principal de organização: Java orientado a objetos coloca classes e objetos no centro da estrutura.

### O que você traz do COBOL

Sua experiência ajuda diretamente a:

- perceber que `ISBN`, título e disponibilidade descrevem o livro;
- entender que matrícula e nome descrevem o usuário;
- reconhecer que datas de retirada e devolução descrevem o empréstimo;
- separar informação permanente de informação temporária;
- avaliar onde uma nova regra provoca impacto;
- desconfiar de uma estrutura que mistura assuntos demais.

Essas decisões vêm da compreensão do negócio. A sintaxe Java apenas fornece outra maneira de registrá-las.

---

## 2. Java não é “COBOL com outra sintaxe”

Uma tradução mecânica tentaria trocar cada construção COBOL por uma construção Java, mantendo a mesma forma geral do programa. O resultado pode até compilar, mas corre o risco de usar Java como se fosse apenas um executor de procedimentos escritos com chaves e ponto e vírgula.

Imagine uma única classe chamada `AplicacaoBiblioteca` responsável por:

- guardar dados de livros;
- guardar dados de usuários;
- registrar empréstimos;
- calcular atrasos;
- emitir relatórios;
- exibir mensagens;
- iniciar o programa.

Essa classe saberia detalhes de todos os assuntos. Qualquer mudança chegaria ao mesmo arquivo. O problema não seria a existência de instruções sequenciais; seria a concentração de responsabilidades.

Em Java, métodos também contêm sequências de instruções. A orientação a objetos não remove o procedimento. Ela muda o contexto no qual esse procedimento existe:

| Perspectiva predominante | Pergunta inicial |
|---|---|
| Procedural | Quais passos precisam ser executados? |
| Orientada a objetos | Quais elementos existem e pelo que cada um responde? |

As duas perguntas são necessárias. Em um empréstimo, ainda haverá uma ordem de ações. Mas o sistema também precisa representar o livro, o usuário e o próprio empréstimo.

### De verbos para substantivos — sem abandonar os verbos

No fluxo procedural, os verbos aparecem primeiro:

```text
validar → registrar → atualizar → exibir
```

Ao modelar objetos, também observamos os substantivos:

```text
Livro → Usuario → Emprestimo → Biblioteca
```

Os substantivos ajudam a encontrar candidatos a classes. Os verbos ajudam a encontrar comportamentos e processos. Nem todo substantivo vira classe e nem todo verbo vira método automaticamente; essa é apenas uma forma inicial de enxergar o domínio.

---

## 3. Classe: a definição de um conceito

Uma **classe** é uma definição criada para representar determinado tipo de elemento no sistema. Ela descreve quais informações esse elemento pode possuir e, posteriormente, quais comportamentos podem fazer parte de sua responsabilidade.

Na biblioteca, `Livro` é um conceito importante. Podemos criar:

```java
package br.com.curso.biblioteca.dominio;

public class Livro {
}
```

Leia o trecho assim:

- `package br.com.curso.biblioteca.dominio;`: informa onde a classe está organizada;
- `public class Livro`: declara uma classe chamada `Livro`;
- `{ }`: delimita o conteúdo que pertence à classe.

Mesmo vazia, a classe já registra uma decisão: livro será um conceito próprio no sistema.

### Classe e grupo de dados COBOL

Um item de grupo de nível 01 reúne campos relacionados:

```cobol
01 WS-LIVRO.
   05 WS-LIVRO-ISBN    PIC X(13).
   05 WS-LIVRO-TITULO  PIC X(100).
```

Uma classe Java também pode agrupar informações relacionadas:

```java
public class Livro {
    String isbn;
    String titulo;
}
```

Essa comparação ajuda, mas há uma diferença importante. O grupo COBOL descreve uma estrutura de dados. Uma classe pode descrever dados **e** comportamentos ligados ao conceito. Neste tópico veremos apenas os dados, mas a classe não deve ser entendida como um `01` escrito em outra sintaxe.

### Nome da classe

Classes normalmente recebem nomes no singular e que expressem um conceito:

```text
Livro
Usuario
Emprestimo
Biblioteca
```

Em Java, a convenção é iniciar cada palavra do nome com maiúscula:

```java
AplicacaoBiblioteca
```

Esse padrão é conhecido como *PascalCase*. Convenções não são detalhes dispensáveis: elas permitem que quem lê reconheça rapidamente o papel de um nome.

---

## 4. Atributo: informação pertencente à classe

Um **atributo** representa uma informação que pertence a uma classe. Se título e ISBN descrevem um livro, faz sentido que sejam atributos de `Livro`.

```java
package br.com.curso.biblioteca.dominio;

public class Livro {

    String isbn;
    String titulo;
    String autor;
    int numeroPaginas;
    boolean disponivel;
}
```

Essas linhas não são etapas executadas uma depois da outra. Elas descrevem o estado que um objeto da classe `Livro` poderá possuir.

| Atributo | Significado no domínio |
|---|---|
| `isbn` | identificação editorial |
| `titulo` | nome da obra |
| `autor` | autoria da obra |
| `numeroPaginas` | quantidade de páginas |
| `disponivel` | condição para empréstimo |

### Atributo não é uma variável global com outro nome

No exemplo, os atributos pertencem à definição de `Livro`. Cada objeto criado a partir dessa classe poderá possuir seus próprios valores. Dois livros diferentes podem ter títulos, ISBNs e disponibilidades diferentes sem que seja necessário criar uma classe para cada obra.

Essa ligação entre informação e conceito é um dos primeiros ganhos da organização orientada a objetos. Ao procurar o título, você sabe que ele pertence a `Livro`. Ao procurar a data de devolução, procurará em `Emprestimo`, não em uma área genérica com todos os dados do sistema.

---

## 5. Tipo: que espécie de informação é esta?

Em Java, todo atributo possui um **tipo**. O tipo informa que conjunto de valores aquela informação pode representar e quais operações fazem sentido para ela.

No exemplo:

```java
String titulo;
int numeroPaginas;
boolean disponivel;
```

- `String` representa texto;
- `int` representa número inteiro;
- `boolean` representa `true` ou `false`.

O tipo ajuda o compilador a impedir combinações incoerentes e também comunica intenção para quem lê.

### `PIC X(n)` e `String`

Em COBOL:

```cobol
05 WS-LIVRO-TITULO PIC X(100).
```

O campo tem tamanho definido de 100 posições. Em Java:

```java
String titulo;
```

`String` representa uma sequência de caracteres, mas a declaração não estabelece o limite de 100 caracteres. Portanto:

```text
PIC X(100) ≠ String com limite automático de 100
```

A ponte correta é:

- ambos podem representar informação textual;
- `PIC X(100)` já declara um tamanho fixo;
- `String` não recebe esse limite na declaração;
- se o negócio exigir no máximo 100 caracteres, essa restrição precisará ser validada em outro ponto.

`String` começa com letra maiúscula porque não é um tipo primitivo; é uma classe da biblioteca padrão do Java.

### `PIC X(1)` e `char`

Java possui o tipo primitivo `char`, usado para uma unidade de caractere:

```java
char categoria;
```

Ele pode lembrar `PIC X(1)`, mas também não é uma equivalência perfeita. `char` representa uma unidade UTF-16; `PIC X(1)` descreve uma posição alfanumérica conforme a representação usada pelo programa COBOL.

Na prática Java, informações textuais, mesmo curtas, frequentemente são representadas por `String`. O tipo deve ser escolhido pelo significado do dado, não apenas pelo número de posições que ele ocupava antes.

### `PIC 9(n)`, `int` e `long`

Em COBOL:

```cobol
05 WS-LIVRO-NUMERO-PAGINAS PIC 9(5).
```

Em Java:

```java
int numeroPaginas;
```

Os dois podem representar uma quantidade inteira, mas descrevem capacidade de formas diferentes:

- `PIC 9(5)` declara cinco posições decimais;
- `int` é um inteiro binário de 32 bits, com valores de `-2.147.483.648` a `2.147.483.647`;
- `long` é um inteiro de 64 bits, usado quando `int` não oferece alcance suficiente.

Os inteiros Java são sinalizados por padrão. Não precisamos acrescentar um equivalente a `S` para permitir números negativos.

Para quantidade de páginas, `int` é mais que suficiente. Para um identificador numérico muito longo, `long` pode ter alcance maior, mas ainda é preciso perguntar se aquele dado é realmente número. ISBN, CPF, matrícula e número de conta podem conter zeros à esquerda e não participam de cálculos; por isso, muitas vezes fazem mais sentido como `String`.

### Indicador COBOL e `boolean`

Em COBOL, uma condição pode ser representada por um campo:

```cobol
05 WS-LIVRO-DISPONIVEL PIC X(1).
   88 LIVRO-DISPONIVEL   VALUE 'S'.
   88 LIVRO-INDISPONIVEL VALUE 'N'.
```

Em Java:

```java
boolean disponivel;
```

O tipo `boolean` aceita somente:

```text
true
false
```

Essa é uma diferença importante. Um `PIC X(1)` aceita qualquer caractere se nenhuma validação impedir. Já um `boolean` não aceita `"S"`, `"N"`, `"X"` ou espaço. O próprio tipo reduz os estados possíveis.

O nível 88 dá nomes de condição a valores de um campo. Ele se aproxima conceitualmente da leitura de uma condição, mas não é o mesmo mecanismo de `boolean`. A comparação útil é a intenção: representar uma situação com dois estados claros.

### Valor monetário: `PIC 9...V99` e `BigDecimal`

Um valor monetário poderia aparecer em COBOL como:

```cobol
05 WS-MULTA PIC S9(7)V99 COMP-3.
```

Em Java, existe `double`, mas valores financeiros normalmente pedem cuidado com arredondamento e precisão. Uma escolha comum é `BigDecimal`:

```java
BigDecimal valorMulta;
```

`BigDecimal` é uma classe, não um tipo primitivo. Ela representa números decimais com precisão controlável. Para utilizá-la pelo nome simples, normalmente fazemos:

```java
import java.math.BigDecimal;
```

Neste tópico, basta guardar duas ideias:

- `PIC S9(7)V99 COMP-3` e `BigDecimal` podem atender a necessidades decimais, mas funcionam de maneiras diferentes;
- em Java, dinheiro não deve ser automaticamente tratado como `double` apenas porque possui casas decimais.

### Datas: campo formatado e `LocalDate`

Em COBOL, uma data pode estar em um campo:

```cobol
05 WS-DATA-EMPRESTIMO PIC 9(8).
```

Esse campo pode guardar `20260920`, mas continua sendo uma sequência numérica cuja interpretação depende de convenção. Java oferece uma classe que representa uma data:

```java
LocalDate dataEmprestimo;
```

Ela é importada de:

```java
import java.time.LocalDate;
```

`LocalDate` comunica que a informação é uma data, não apenas oito dígitos. A classe também possui regras próprias para trabalhar com calendário, que serão estudadas quando necessário.

### Resumo das pontes

| Intenção | Exemplo COBOL | Possibilidade em Java | Diferença principal |
|---|---|---|---|
| Texto | `PIC X(100)` | `String` | `String` não declara limite de 100 |
| Um caractere | `PIC X(1)` | `char` | representações não são idênticas |
| Inteiro | `PIC 9(5)` | `int` | dígitos decimais × inteiro binário de 32 bits |
| Inteiro maior | `PIC 9(n)` maior | `long` | alcance definido pelo tipo de 64 bits |
| Condição | `PIC X(1)` + nível 88 | `boolean` | Java aceita somente `true` ou `false` |
| Decimal monetário | `PIC S9(n)V99 COMP-3` | `BigDecimal` | classe decimal com precisão controlável |
| Data | `PIC 9(8)` | `LocalDate` | data com significado próprio, não apenas dígitos |

---

## 6. Tipos primitivos e tipos por referência

Java possui duas grandes famílias de tipos.

### Tipos primitivos

Guardam valores simples diretamente:

| Tipo | Finalidade inicial |
|---|---|
| `byte` | inteiro pequeno |
| `short` | inteiro menor que `int` |
| `int` | inteiro de uso geral |
| `long` | inteiro de alcance maior |
| `float` | ponto flutuante de precisão simples |
| `double` | ponto flutuante de precisão dupla |
| `char` | unidade de caractere |
| `boolean` | verdadeiro ou falso |

Não é preciso memorizar todos agora. Para o exemplo da biblioteca, `int` e `boolean` já são suficientes.

Os tipos inteiros existem em tamanhos diferentes:

| Tipo | Tamanho | Intervalo aproximado |
|---|---:|---:|
| `byte` | 8 bits | -128 a 127 |
| `short` | 16 bits | -32 mil a 32 mil |
| `int` | 32 bits | -2,1 bilhões a 2,1 bilhões |
| `long` | 64 bits | cerca de -9 quintilhões a 9 quintilhões |

Isso difere da leitura de `PIC 9(n)`: em COBOL, a quantidade de símbolos `9` comunica a quantidade de dígitos decimais; em Java, o nome do tipo determina uma faixa binária fixa. Para código comum, `int` costuma ser a primeira escolha para quantidades inteiras, e `long` é usado quando o alcance necessário é maior. Escolher `byte` apenas porque um valor é pequeno raramente traz vantagem para uma aplicação de negócio.

`float` e `double` representam números de ponto flutuante. Eles são úteis em muitos cálculos científicos e aproximados, mas nem todo número com casas decimais deve ser colocado neles. A representação binária pode não corresponder exatamente a determinados valores decimais. Por isso, dinheiro costuma ser representado com `BigDecimal`.

O tipo não deve ser escolhido apenas pelo formato visual do dado. Pergunte o que a informação **significa**:

- número de páginas é uma quantidade e combina com `int`;
- ISBN parece numérico, mas é identificador e combina melhor com `String`;
- disponibilidade possui dois estados e combina com `boolean`;
- data de empréstimo combina com `LocalDate`;
- valor de multa combina com `BigDecimal`.

### Tipos por referência

Classes definem tipos por referência. São exemplos:

```text
String
BigDecimal
LocalDate
Livro
Usuario
Emprestimo
```

Isso significa que as classes que criamos também podem ser usadas como tipos:

```java
Livro livro;
Usuario usuario;
```

Essa ideia é central para orientação a objetos. `Emprestimo` pode ter um atributo do tipo `Livro` e outro do tipo `Usuario`, expressando diretamente a relação do domínio:

```java
public class Emprestimo {
    Livro livro;
    Usuario usuario;
    LocalDate dataEmprestimo;
    LocalDate dataPrevistaDevolucao;
}
```

Antes de usar `LocalDate`, o arquivo precisa importá-la. `Livro` e `Usuario` não precisam de import se estiverem no mesmo pacote de `Emprestimo`.

### Valor direto e referência

Uma variável de tipo primitivo contém o próprio valor:

```java
int numeroPaginas;
boolean disponivel;
```

Uma variável cujo tipo é uma classe contém uma referência para um objeto:

```java
String titulo;
Livro livro;
Usuario usuario;
```

Essa diferença explica por que `Livro` pode ser ao mesmo tempo nome de classe e tipo de um atributo. O atributo não contém uma nova definição da classe; ele pode apontar para um objeto `Livro`.

### Valores iniciais dos atributos

Quando um objeto é criado, atributos recebem valores iniciais padrão:

| Tipo do atributo | Valor inicial |
|---|---|
| tipos inteiros | `0` |
| `float` e `double` | zero correspondente |
| `boolean` | `false` |
| `char` | caractere de valor zero |
| tipos por referência | `null` |

`null` significa que a referência não aponta para um objeto. Ele não é texto vazio, zero nem um objeto em branco.

Esses padrões pertencem ao funcionamento da linguagem; não representam necessariamente uma decisão do negócio. Um livro começar com `disponivel = false` não significa que “todo novo livro deve nascer indisponível”. Essa regra terá de ser definida conscientemente quando estudarmos a criação e a inicialização de objetos.

### Tipo não substitui regra de negócio

O tipo restringe uma parte do problema, mas não valida tudo:

- `String isbn` ainda pode receber texto com formato inválido;
- `int numeroPaginas` ainda pode representar um número negativo;
- `LocalDate dataDevolucao` ainda pode ser anterior à data do empréstimo;
- `BigDecimal valorMulta` ainda pode representar um valor inadequado.

Portanto, escolher bons tipos melhora o modelo, mas regras do domínio continuarão necessárias. Elas serão acrescentadas quando os recursos usados para expressá-las já tiverem sido apresentados.

---

## 7. Objeto: uma ocorrência concreta da classe

Se a classe é a definição, o **objeto** é uma ocorrência criada a partir dela.

```java
Livro livro = new Livro();
```

Leia a linha por partes:

| Parte | Significado inicial |
|---|---|
| `Livro` | tipo da referência |
| `livro` | nome usado para acessar essa referência |
| `new Livro()` | criação de um novo objeto da classe `Livro` |

É útil separar três ideias:

1. `Livro` é a classe e também o tipo.
2. O objeto é a ocorrência criada por `new Livro()`.
3. `livro` é a referência usada para chegar a esse objeto.

Em uma explicação inicial, é comum dizer apenas “o objeto `livro`”. Essa simplificação ajuda, mas tecnicamente a variável `livro` guarda uma referência ao objeto.

### Objeto e ocorrência de registro

Um grupo de dados COBOL descreve uma estrutura, e uma área de memória contém valores concretos daquela estrutura. Isso oferece uma ponte útil com classe e objeto.

A diferença é que objetos possuem identidade própria e pertencem a um modelo em que dados e comportamentos podem ficar reunidos. Dois objetos `Livro` podem ter atributos iguais e ainda serem dois objetos distintos.

Uma classe não é um objeto:

```text
Classe Livro
   ├── objeto: exemplar de “Dom Casmurro”
   ├── objeto: exemplar de “Grande Sertão: Veredas”
   └── objeto: exemplar de “Capitães da Areia”
```

Uma única classe permite representar muitos livros concretos.

---

## 8. Programa organizado por responsabilidades e objetos

Depois de compreender classe, atributo, tipo e objeto, podemos distribuir melhor o domínio:

| Classe | Informações associadas | Responsabilidade inicial |
|---|---|---|
| `Livro` | ISBN, título, autor, páginas, disponibilidade | representar uma obra ou exemplar do acervo |
| `Usuario` | matrícula, nome, contato | representar quem utiliza a biblioteca |
| `Emprestimo` | livro, usuário e datas | representar a relação de empréstimo |
| `Biblioteca` | organização do acervo e operações | coordenar o domínio |
| `AplicacaoBiblioteca` | ponto de entrada | iniciar a aplicação |

Responsabilidade é aquilo que uma parte do sistema precisa conhecer ou realizar porque combina com seu papel.

Seria estranho colocar `titulo` em `Emprestimo`, porque título descreve livro. Também seria estranho colocar `matriculaUsuario` em `Livro`, porque matrícula descreve usuário. `Emprestimo` não precisa duplicar todos esses dados; ele pode se relacionar com objetos dos tipos `Livro` e `Usuario`.

### O fluxo não desaparece

A aplicação ainda terá processos:

```text
AplicacaoBiblioteca inicia
          ↓
Biblioteca coordena uma operação
          ↓
Emprestimo relaciona Usuario e Livro
```

Existem setas e ordem. A diferença é que o fluxo trabalha com partes que possuem significado próprio.

### Nem todo substantivo vira classe

Localizar substantivos é apenas um começo. “Tela”, “confirmação” e “mensagem” aparecem nos requisitos, mas não precisam automaticamente virar classes. Uma classe deve ajudar a representar o domínio ou organizar uma responsabilidade real.

Criar muitas classes sem critério não é orientação a objetos bem aplicada. O objetivo é coerência, não quantidade de arquivos.

---

## 9. Pacotes desde o início

Um **pacote** agrupa classes relacionadas e participa do nome completo delas. Ele funciona como organização e como espaço de nomes.

Podemos organizar o projeto assim:

```text
gestao-biblioteca/
└── src/
    └── br/
        └── com/
            └── curso/
                └── biblioteca/
                    ├── AplicacaoBiblioteca.java
                    └── dominio/
                        ├── Livro.java
                        ├── Usuario.java
                        ├── Emprestimo.java
                        └── Biblioteca.java
```

No arquivo `Livro.java`:

```java
package br.com.curso.biblioteca.dominio;
```

O nome completo da classe é:

```text
br.com.curso.biblioteca.dominio.Livro
```

Podem existir classes chamadas `Livro` em projetos ou pacotes diferentes. O nome completo evita ambiguidade.

### Pacote não é apenas uma pasta decorativa

No código-fonte, o pacote declara a identidade da classe. Na organização usual do projeto, a estrutura de diretórios acompanha essa declaração:

```text
package br.com.curso.biblioteca.dominio;
              ↕
src\br\com\curso\biblioteca\dominio
```

Começar com pacotes desde cedo evita que todas as classes acabem na mesma pasta. Não precisamos criar dezenas de camadas. Uma separação inicial entre a aplicação e o domínio já comunica intenção.

---

## 10. `import`: usar o nome simples de outra classe

Considere `AplicacaoBiblioteca` no pacote:

```java
package br.com.curso.biblioteca;
```

E `Livro` em:

```java
package br.com.curso.biblioteca.dominio;
```

Para usar `Livro` pelo nome simples, fazemos:

```java
package br.com.curso.biblioteca;

import br.com.curso.biblioteca.dominio.Livro;

public class AplicacaoBiblioteca {

    public static void main(String[] args) {
        Livro livro = new Livro();
        System.out.println("Sistema de Gestão de Biblioteca iniciado.");
    }
}
```

Sem o `import`, seria possível usar o nome completo:

```java
br.com.curso.biblioteca.dominio.Livro livro =
        new br.com.curso.biblioteca.dominio.Livro();
```

O `import` evita repetir esse nome completo.

### O que `import` não faz

O `import`:

- não copia o conteúdo da classe;
- não baixa uma biblioteca;
- não cria um objeto;
- não executa a classe;
- não torna todas as classes disponíveis automaticamente.

Ele permite que o compilador associe o nome simples `Livro` ao nome completo `br.com.curso.biblioteca.dominio.Livro`.

### `import` e `COPY` não são equivalentes

Um `COPY` COBOL insere o conteúdo de um copybook no fonte durante a preparação/compilação. Um `import` Java não insere o texto de `Livro.java` em `AplicacaoBiblioteca.java`. `Livro` continua sendo uma classe separada.

A semelhança está apenas na intenção ampla de organizar e reutilizar definições. O mecanismo é diferente.

### Quando o `import` não é necessário

- Classes do mesmo pacote podem se referir umas às outras sem `import`.
- Classes de `java.lang`, como `String` e `System`, são disponibilizadas automaticamente.
- Tipos de outros pacotes, como `LocalDate` e `BigDecimal`, normalmente precisam de import.

Exemplos:

```java
import java.math.BigDecimal;
import java.time.LocalDate;
```

---

## 11. As peças reunidas

### `Livro.java`

```java
package br.com.curso.biblioteca.dominio;

public class Livro {
    String isbn;
    String titulo;
    String autor;
    int numeroPaginas;
    boolean disponivel;
}
```

### `Usuario.java`

```java
package br.com.curso.biblioteca.dominio;

public class Usuario {
    String matricula;
    String nome;
}
```

### `Emprestimo.java`

```java
package br.com.curso.biblioteca.dominio;

import java.time.LocalDate;

public class Emprestimo {
    Livro livro;
    Usuario usuario;
    LocalDate dataEmprestimo;
    LocalDate dataPrevistaDevolucao;
}
```

### `AplicacaoBiblioteca.java`

```java
package br.com.curso.biblioteca;

import br.com.curso.biblioteca.dominio.Livro;

public class AplicacaoBiblioteca {

    public static void main(String[] args) {
        Livro livro = new Livro();
        System.out.println("Sistema de Gestão de Biblioteca iniciado.");
    }
}
```

Observe a construção:

- `Livro`, `Usuario` e `Emprestimo` representam conceitos;
- os atributos registram informações pertencentes a esses conceitos;
- tipos primitivos descrevem valores simples;
- classes também são usadas como tipos;
- `Emprestimo` se relaciona com `Livro` e `Usuario`;
- classes do mesmo pacote dispensam import entre si;
- `LocalDate`, de outro pacote, precisa de import;
- a aplicação importa `Livro` porque está em outro pacote;
- um objeto `Livro` é criado com `new Livro()`.

Não implementamos regras de empréstimo. Mesmo assim, já existe um modelo inicial do domínio, e ele comunica mais que uma lista de campos soltos.

---

## 12. Erros de compreensão que vale evitar

### “Classe é um registro COBOL com chaves”

Uma classe pode agrupar dados como um registro, mas também pode reunir comportamentos e participar de relações entre objetos.

### “Objeto é o mesmo que classe”

A classe é a definição. O objeto é uma ocorrência criada a partir dela.

### “A variável contém o objeto inteiro”

Para tipos por referência, a variável guarda uma referência ao objeto. Essa distinção ficará mais importante quando trabalharmos com vários objetos.

### “`String` é `PIC X` sem tamanho”

Ambos representam texto, mas possuem modelos diferentes. O limite de tamanho de `PIC X(n)` não acompanha automaticamente a `String`.

### “`boolean` é apenas um `S/N`”

`boolean` aceita somente `true` e `false`. Um campo textual pode aceitar outros valores se não houver validação.

### “Todo número deve virar `int`”

Identificadores podem ser textuais; inteiros possuem alcances específicos; valores monetários pedem precisão decimal; datas merecem tipos de data.

### “`import` é o `COPY` do Java”

Os dois auxiliam a organização, mas `COPY` insere conteúdo e `import` resolve o nome de uma classe.

### “Orientação a objetos elimina procedimentos”

Métodos ainda contêm instruções e processos ainda possuem ordem. A mudança está na distribuição das responsabilidades.

---

## 13. Checklist do Tópico 2

- [ ] Consigo explicar o que permanece útil da experiência em COBOL.
- [ ] Entendi por que Java não deve ser tratado como tradução sintática.
- [ ] Sei que classe é a definição de um conceito.
- [ ] Sei que objeto é uma ocorrência concreta de uma classe.
- [ ] Consigo distinguir objeto, classe e referência.
- [ ] Entendo atributo como informação pertencente a uma classe.
- [ ] Sei que todo atributo possui um tipo.
- [ ] Reconheço os oito tipos primitivos do Java.
- [ ] Sei que `String` é uma classe, não um tipo primitivo.
- [ ] Entendo a diferença entre `PIC X(n)` e `String`.
- [ ] Entendo a diferença entre `PIC 9(n)`, `int` e `long`.
- [ ] Entendo a ponte entre indicadores/níveis 88 e `boolean`.
- [ ] Sei por que `BigDecimal` é relevante para dinheiro.
- [ ] Sei por que `LocalDate` comunica mais que um campo `PIC 9(8)`.
- [ ] Entendo que classes criadas no projeto também são tipos.
- [ ] Consigo associar `Livro`, `Usuario` e `Emprestimo` a responsabilidades diferentes.
- [ ] Sei que pacotes organizam e identificam classes.
- [ ] Sei o que `import` faz e por que ele não é igual a `COPY`.

## Fechamento

A mudança do procedural para a orientação a objetos não exige abandonar o raciocínio de processos. Ela exige ampliar esse raciocínio.

Você continuará perguntando quais etapas precisam acontecer. Ao mesmo tempo, passará a perguntar quais elementos participam do processo, quais informações pertencem a cada um e onde cada responsabilidade deve ficar.

COBOL oferece pontos de comparação valiosos: grupos de dados ajudam a compreender atributos; campos `PIC` ajudam a discutir tipos; níveis 88 ajudam a introduzir condições booleanas; copybooks ajudam a perceber que sistemas precisam de organização e reutilização. Mas essas pontes têm limites. Java possui seu próprio modelo de tipos, classes, objetos, pacotes e imports.

Na Gestão de Biblioteca, o programa já deixou de ser apenas uma sequência abstrata de passos. Agora conseguimos enxergar `Livro`, `Usuario`, `Emprestimo`, `Biblioteca` e `AplicacaoBiblioteca` como partes diferentes de um mesmo sistema. Essa é a base sobre a qual o restante da orientação a objetos será construído.
