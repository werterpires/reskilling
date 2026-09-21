# Exercícios — Tópico 2: Entender a mudança procedural → orientação a objetos

## Continuação do projeto prático: Gestor de Tarefas

Na sequência anterior, você preparou o ambiente Java e criou a primeira versão do **Gestor de Tarefas**. O programa possuía uma classe inicial, compilava, executava e exibia mensagens no terminal.

Agora o mesmo projeto começará a adquirir uma estrutura orientada a objetos. Em vez de deixar toda a ideia do sistema concentrada em `AplicacaoGestorTarefas`, criaremos classes que representam conceitos do domínio: uma tarefa e uma pessoa responsável.

Esta etapa continuará sendo intencionalmente simples. O sistema ainda não receberá informações pelo teclado, não armazenará uma lista de tarefas e não decidirá se uma tarefa está atrasada. Também não serão usados construtores personalizados, métodos de negócio, `if`, repetições, coleções, banco de dados, getters ou setters. Esses recursos serão acrescentados somente depois de serem estudados.

O objetivo deste tópico é construir uma base conceitual correta:

- reconhecer o COBOL como ponto de partida, sem tentar traduzir cada linha literalmente;
- separar a inicialização da aplicação das classes que representam o domínio;
- criar classes, atributos e objetos;
- escolher tipos a partir do significado das informações;
- compreender a diferença entre tipo primitivo e tipo por referência;
- organizar as classes em pacotes;
- usar `import` para trabalhar com classes de outros pacotes;
- compilar e executar um projeto que agora possui vários arquivos `.java`.

> Continue trabalhando no **computador físico**, não na VDI. Se algum comando não produzir o resultado indicado, procure o instrutor e mostre a mensagem completa exibida no terminal.

---

## Ponto de partida

Use a mesma pasta criada nos exercícios do Tópico 1:

```text
gestor-tarefas/
├── out/
└── src/
    └── br/
        └── com/
            └── curso/
                └── tarefas/
                    └── AplicacaoGestorTarefas.java
```

Ao final do Tópico 1, `AplicacaoGestorTarefas.java` exibia o título do sistema, três tarefas escritas diretamente nos comandos `println` e uma mensagem de status.

Essa versão não está errada para o conteúdo estudado até aquele momento. Ela cumpriu seu papel: permitiu validar o ambiente, criar uma classe, compilar e executar. Agora vamos evoluí-la.

## Resultado final desta sequência

Ao concluir os exercícios, a estrutura deverá ser:

```text
gestor-tarefas/
├── out/
└── src/
    └── br/
        └── com/
            └── curso/
                └── tarefas/
                    ├── AplicacaoGestorTarefas.java
                    └── dominio/
                        ├── Responsavel.java
                        └── Tarefa.java
```

As responsabilidades estarão distribuídas assim:

| Classe | Responsabilidade nesta etapa |
|---|---|
| `AplicacaoGestorTarefas` | iniciar o programa e criar os primeiros objetos |
| `Tarefa` | representar uma tarefa do sistema |
| `Responsavel` | representar a pessoa associada a uma tarefa |

Ao executar o projeto, o terminal deverá mostrar:

```text
=== GESTOR DE TAREFAS ===
Modelo orientado a objetos inicializado.

Objetos criados:
- um responsável
- tarefa: revisar requisitos
- tarefa: implementar a tela inicial
- tarefa: validar a aplicação

As três tarefas são objetos distintos da classe Tarefa.
```

As descrições exibidas ainda serão mensagens fixas. Os atributos existirão nas classes, mas seu preenchimento e sua exibição serão trabalhados depois, quando os recursos necessários tiverem sido apresentados.

---

## Exercício 11 — Confirmar a versão anterior do projeto

Antes de alterar a estrutura, confirme que o projeto do Tópico 1 continua funcionando.

Abra a pasta `gestor-tarefas` no VS Code. Depois, abra o terminal integrado e execute:

```bat
javac -d out src\br\com\curso\tarefas\AplicacaoGestorTarefas.java
java -cp out br.com.curso.tarefas.AplicacaoGestorTarefas
```

O programa deverá exibir a versão construída anteriormente.

Essa conferência cria um ponto de partida conhecido. Se a versão anterior não compilar, resolva esse problema antes de acrescentar novas classes. Caso contrário, um erro antigo poderá ser confundido com uma alteração do Tópico 2.

### Conferência

- [ ] A pasta inteira do projeto está aberta no VS Code.
- [ ] O terminal está posicionado na pasta `gestor-tarefas`.
- [ ] `AplicacaoGestorTarefas.java` compila.
- [ ] A classe é encontrada e executada.
- [ ] As mensagens do Tópico 1 aparecem no terminal.

---

## Exercício 12 — Identificar conceitos e responsabilidades

Antes de criar arquivos, observe estas informações do Gestor de Tarefas:

```text
código da tarefa
título da tarefa
descrição da tarefa
estimativa de horas
situção de conclusão
data-limite
custo estimado
matrícula do responsável
nome do responsável
situção do responsável
```

Se todas essas informações fossem colocadas dentro de `AplicacaoGestorTarefas`, a classe inicial começaria a concentrar dados que representam conceitos diferentes.

Separe as informações em duas responsabilidades:

| Conceito | Informações que pertencem a ele |
|---|---|
| `Tarefa` | código, título, descrição, estimativa, conclusão, data-limite e custo estimado |
| `Responsavel` | matrícula, nome e situação de atividade |

Registre essa divisão em um arquivo de texto chamado `modelo-inicial.txt`, na raiz do projeto:

```text
Tarefa
- código
- título
- descrição
- estimativa de horas
- concluída
- data-limite
- custo estimado
- responsável

Responsavel
- matrícula
- nome
- ativo

AplicacaoGestorTarefas
- iniciar o programa
- criar os objetos iniciais
```

Esse arquivo não será compilado. Ele apenas registra a decisão de modelagem que orientará os próximos exercícios.

### Relação com COBOL

Em COBOL, você poderia reunir campos relacionados em itens de grupo de nível 01. Essa experiência continua útil para perceber quais dados formam um conjunto coerente. A mudança está em não tratar a aplicação inteira como uma única área de trabalho seguida por uma grande sequência de procedimentos.

Em Java, `Tarefa` e `Responsavel` serão conceitos próprios. Mais adiante, eles também poderão concentrar comportamentos relacionados às suas responsabilidades.

---

## Exercício 13 — Criar o pacote de domínio

Dentro de:

```text
src\br\com\curso\tarefas
```

crie uma pasta chamada:

```text
dominio
```

A estrutura ficará assim:

```text
src/
└── br/
    └── com/
        └── curso/
            └── tarefas/
                ├── AplicacaoGestorTarefas.java
                └── dominio/
```

Essa pasta corresponderá ao pacote:

```text
br.com.curso.tarefas.dominio
```

O pacote `br.com.curso.tarefas` continuará contendo a classe que inicia a aplicação. O pacote `br.com.curso.tarefas.dominio` conterá as classes que representam os conceitos do Gestor de Tarefas.

### Leia a correspondência

```text
src\br\com\curso\tarefas\dominio
                    ↕
package br.com.curso.tarefas.dominio;
```

O pacote não é apenas uma pasta usada para deixar o explorador organizado. Ele faz parte do nome completo de cada classe.

---

## Exercício 14 — Criar a classe `Tarefa`

Dentro da pasta `dominio`, crie:

```text
Tarefa.java
```

Escreva inicialmente:

```java
package br.com.curso.tarefas.dominio;

public class Tarefa {
}
```

Mesmo vazia, essa classe já representa uma decisão de modelagem: uma tarefa será um conceito próprio no sistema.

### Leia o código por partes

| Trecho | Significado |
|---|---|
| `package br.com.curso.tarefas.dominio;` | declara o pacote da classe |
| `public class Tarefa` | declara uma classe chamada `Tarefa` |
| `{ }` | delimita o conteúdo que pertence à classe |

O nome do arquivo e o nome da classe pública devem continuar iguais:

```text
Tarefa.java ↔ Tarefa
```

### Compile somente a nova classe

Na raiz do projeto, execute:

```bat
javac -d out src\br\com\curso\tarefas\dominio\Tarefa.java
```

Confirme a criação de:

```text
out\br\com\curso\tarefas\dominio\Tarefa.class
```

Não tente executar `Tarefa` com o comando `java`. Ela não possui `main` e não é o ponto de entrada da aplicação. Sua responsabilidade é representar um conceito do domínio.

---

## Exercício 15 — Acrescentar os atributos básicos de `Tarefa`

Altere `Tarefa.java`:

```java
package br.com.curso.tarefas.dominio;

public class Tarefa {

    String codigo;
    String titulo;
    String descricao;
    int estimativaHoras;
    boolean concluida;
}
```

Essas linhas não representam uma sequência de comandos. Elas descrevem informações que poderão pertencer a cada objeto da classe `Tarefa`.

### Interprete os tipos

| Atributo | Tipo Java | Motivo |
|---|---|---|
| `codigo` | `String` | é identificador, não uma quantidade usada em cálculo |
| `titulo` | `String` | representa texto |
| `descricao` | `String` | representa texto |
| `estimativaHoras` | `int` | representa quantidade inteira |
| `concluida` | `boolean` | representa uma condição com dois estados |

### Ponte com COBOL

Uma estrutura semelhante poderia começar assim em COBOL:

```cobol
01 WS-TAREFA.
   05 WS-TAREFA-CODIGO       PIC X(10).
   05 WS-TAREFA-TITULO       PIC X(100).
   05 WS-TAREFA-DESCRICAO    PIC X(250).
   05 WS-TAREFA-ESTIMATIVA   PIC 9(3).
   05 WS-TAREFA-CONCLUIDA    PIC X(1).
      88 TAREFA-CONCLUIDA    VALUE 'S'.
      88 TAREFA-PENDENTE     VALUE 'N'.
```

Não trate o código Java como uma tradução linha a linha:

- `String` não recebe automaticamente o limite declarado em `PIC X(n)`;
- `int` possui uma faixa binária fixa, não uma quantidade declarada de dígitos;
- `boolean` aceita apenas `true` ou `false`, enquanto `PIC X(1)` pode armazenar outros caracteres;
- a classe `Tarefa` representa um conceito que poderá reunir estado e comportamento.

Compile novamente:

```bat
javac -d out src\br\com\curso\tarefas\dominio\Tarefa.java
```

---

## Exercício 16 — Revisar os tipos pelo significado

Faça uma conferência dos tipos escolhidos. Para cada afirmação, marque verdadeiro ou falso no arquivo `modelo-inicial.txt`.

```text
[ ] codigo deve ser int porque contém algarismos.
[ ] codigo pode ser String porque é um identificador.
[ ] estimativaHoras pode ser int porque representa uma quantidade inteira.
[ ] concluida pode ser boolean porque possui os estados true e false.
[ ] String titulo cria automaticamente um limite de 100 caracteres.
```

O resultado correto é:

```text
[F] codigo deve ser int porque contém algarismos.
[V] codigo pode ser String porque é um identificador.
[V] estimativaHoras pode ser int porque representa uma quantidade inteira.
[V] concluida pode ser boolean porque possui os estados true e false.
[F] String titulo cria automaticamente um limite de 100 caracteres.
```

Depois, acrescente ao final do arquivo:

```text
Decisão: os tipos foram escolhidos pelo significado das informações,
e não apenas pela aparência do valor ou pelo formato usado em COBOL.
```

Esse exercício evita dois erros frequentes na mudança de stack:

1. transformar todo campo composto por algarismos em `int`;
2. imaginar que `String` reproduz automaticamente o tamanho de `PIC X(n)`.

---

## Exercício 17 — Criar a classe `Responsavel`

Na pasta `dominio`, crie:

```text
Responsavel.java
```

Escreva:

```java
package br.com.curso.tarefas.dominio;

public class Responsavel {

    String matricula;
    String nome;
    boolean ativo;
}
```

Agora o projeto possui duas classes de domínio:

```text
Tarefa
Responsavel
```

Não coloque `matricula`, `nome` e `ativo` dentro de `Tarefa`. Essas informações descrevem a pessoa responsável, não a tarefa.

### Compile as duas classes do pacote

No Prompt de Comando do Windows, o caractere `*` permite selecionar os arquivos `.java` da pasta:

```bat
javac -d out src\br\com\curso\tarefas\dominio\*.java
```

Confirme a existência de:

```text
out\br\com\curso\tarefas\dominio\Tarefa.class
out\br\com\curso\tarefas\dominio\Responsavel.class
```

As duas classes pertencem ao mesmo pacote porque ambas declaram:

```java
package br.com.curso.tarefas.dominio;
```

---

## Exercício 18 — Relacionar `Tarefa` e `Responsavel`

Uma tarefa pode estar associada a uma pessoa responsável. Em vez de copiar matrícula e nome para dentro de `Tarefa`, use a própria classe `Responsavel` como tipo de atributo.

Acrescente esta linha em `Tarefa.java`:

```java
Responsavel responsavel;
```

O arquivo ficará assim:

```java
package br.com.curso.tarefas.dominio;

public class Tarefa {

    String codigo;
    String titulo;
    String descricao;
    int estimativaHoras;
    boolean concluida;
    Responsavel responsavel;
}
```

Leia a declaração:

| Parte | Significado |
|---|---|
| `Responsavel` | tipo do atributo |
| `responsavel` | nome do atributo |

`Responsavel` é uma classe criada no próprio projeto e também pode ser usada como tipo. O atributo poderá guardar uma referência para um objeto `Responsavel`.

### É necessário importar?

Não. `Tarefa` e `Responsavel` estão no mesmo pacote:

```text
br.com.curso.tarefas.dominio
```

Classes do mesmo pacote podem ser usadas pelo nome simples sem `import`.

Compile novamente:

```bat
javac -d out src\br\com\curso\tarefas\dominio\*.java
```

---

## Exercício 19 — Acrescentar data e valor decimal

Uma tarefa poderá ter data-limite e custo estimado. Para representar essas informações, use `LocalDate` e `BigDecimal`.

Altere `Tarefa.java`:

```java
package br.com.curso.tarefas.dominio;

import java.math.BigDecimal;
import java.time.LocalDate;

public class Tarefa {

    String codigo;
    String titulo;
    String descricao;
    int estimativaHoras;
    boolean concluida;
    LocalDate dataLimite;
    BigDecimal custoEstimado;
    Responsavel responsavel;
}
```

### Observe a ordem do arquivo

```text
declaração do package
imports
declaração da classe
atributos
```

### Interprete as escolhas

| Informação | Possível representação COBOL | Tipo escolhido em Java |
|---|---|---|
| data-limite | `PIC 9(8)` com convenção de formato | `LocalDate` |
| custo estimado | `PIC S9(n)V99 COMP-3` | `BigDecimal` |

`LocalDate` comunica que o dado possui significado de calendário. `BigDecimal` é apropriado quando a precisão decimal importa. Os dois são classes da biblioteca padrão do Java e pertencem a pacotes diferentes do pacote de `Tarefa`.

Por isso, foram usados:

```java
import java.math.BigDecimal;
import java.time.LocalDate;
```

Os imports não criam objetos e não copiam o conteúdo dessas classes para `Tarefa.java`. Eles permitem usar os nomes simples `BigDecimal` e `LocalDate` no arquivo.

### Teste de compilação

```bat
javac -d out src\br\com\curso\tarefas\dominio\*.java
```

Se você retirar um dos imports e mantiver o nome simples correspondente, o compilador não conseguirá localizar o tipo. Depois do teste, restaure o import e compile novamente.

---

## Exercício 20 — Importar as classes do domínio na aplicação

`AplicacaoGestorTarefas` está no pacote:

```text
br.com.curso.tarefas
```

`Tarefa` e `Responsavel` estão no pacote:

```text
br.com.curso.tarefas.dominio
```

Como são pacotes diferentes, a aplicação deverá importar as classes do domínio. Substitua o conteúdo de `AplicacaoGestorTarefas.java` por:

```java
package br.com.curso.tarefas;

import br.com.curso.tarefas.dominio.Responsavel;
import br.com.curso.tarefas.dominio.Tarefa;

public class AplicacaoGestorTarefas {

    public static void main(String[] args) {
        System.out.println("=== GESTOR DE TAREFAS ===");
        System.out.println("Modelo orientado a objetos inicializado.");
    }
}
```

Ainda não criamos objetos. Primeiro, apenas informamos ao compilador quais classes serão usadas pelo nome simples neste arquivo.

### Compare os nomes

| Forma | Exemplo |
|---|---|
| nome simples | `Tarefa` |
| nome completo | `br.com.curso.tarefas.dominio.Tarefa` |

O import permite escrever `Tarefa` em vez de repetir o nome completo em cada uso.

### `import` não é `COPY`

Em COBOL, `COPY` insere o conteúdo de um copybook no programa durante a preparação ou compilação. Em Java, `import` apenas resolve o nome da classe. `Tarefa.java` e `Responsavel.java` continuam existindo como arquivos e classes separados.

---

## Exercício 21 — Criar o primeiro objeto de cada classe

Dentro do método `main`, antes dos comandos `println`, crie um objeto `Responsavel` e um objeto `Tarefa`:

```java
Responsavel responsavel = new Responsavel();
Tarefa tarefaRevisarRequisitos = new Tarefa();
```

O arquivo ficará assim:

```java
package br.com.curso.tarefas;

import br.com.curso.tarefas.dominio.Responsavel;
import br.com.curso.tarefas.dominio.Tarefa;

public class AplicacaoGestorTarefas {

    public static void main(String[] args) {
        Responsavel responsavel = new Responsavel();
        Tarefa tarefaRevisarRequisitos = new Tarefa();

        System.out.println("=== GESTOR DE TAREFAS ===");
        System.out.println("Modelo orientado a objetos inicializado.");
    }
}
```

### Leia cada linha

Na declaração:

```java
Tarefa tarefaRevisarRequisitos = new Tarefa();
```

| Parte | Significado |
|---|---|
| `Tarefa` | classe e tipo da referência |
| `tarefaRevisarRequisitos` | variável que guarda uma referência |
| `new Tarefa()` | criação de um novo objeto |

Não confunda os três elementos. A classe é a definição; o objeto é a ocorrência criada; a variável é a referência usada para alcançar esse objeto.

### Compile todos os fontes necessários

Execute a partir da pasta `gestor-tarefas`:

```bat
javac -d out src\br\com\curso\tarefas\dominio\*.java src\br\com\curso\tarefas\AplicacaoGestorTarefas.java
```

Depois execute:

```bat
java -cp out br.com.curso.tarefas.AplicacaoGestorTarefas
```

Resultado esperado:

```text
=== GESTOR DE TAREFAS ===
Modelo orientado a objetos inicializado.
```

Os objetos foram criados, mas a criação deles não produz automaticamente uma mensagem no terminal. A saída ainda depende dos comandos `System.out.println`.

---

## Exercício 22 — Criar vários objetos da mesma classe

Uma classe pode originar vários objetos. Mantenha `tarefaRevisarRequisitos` e crie mais duas referências, cada uma recebendo um novo objeto:

```java
Tarefa tarefaImplementarTela = new Tarefa();
Tarefa tarefaValidarAplicacao = new Tarefa();
```

O início do método `main` ficará assim:

```java
public static void main(String[] args) {
    Responsavel responsavel = new Responsavel();

    Tarefa tarefaRevisarRequisitos = new Tarefa();
    Tarefa tarefaImplementarTela = new Tarefa();
    Tarefa tarefaValidarAplicacao = new Tarefa();

    System.out.println("=== GESTOR DE TAREFAS ===");
    System.out.println("Modelo orientado a objetos inicializado.");
}
```

Agora existem três objetos distintos criados a partir da mesma classe `Tarefa`.

Em COBOL, uma estrutura poderia ser repetida por meio de ocorrências. Essa é uma ponte inicial útil, mas não uma equivalência completa. Em Java, cada execução de `new Tarefa()` cria um objeto com identidade própria.

### Não crie uma classe para cada tarefa

Evite arquivos como:

```text
TarefaRevisarRequisitos.java
TarefaImplementarTela.java
TarefaValidarAplicacao.java
```

Esses nomes representam ocorrências, não novos conceitos. Uma única classe `Tarefa` é a definição usada para criar todas elas.

---

## Exercício 23 — Diferenciar um novo objeto de uma nova referência

Depois da criação das três tarefas, escreva:

```java
Tarefa tarefaSelecionada = tarefaRevisarRequisitos;
```

Observe que essa linha não possui `new`.

```java
Tarefa tarefaRevisarRequisitos = new Tarefa();
Tarefa tarefaSelecionada = tarefaRevisarRequisitos;
```

As duas variáveis podem guardar referência para o mesmo objeto. A segunda linha não cria uma quarta tarefa.

Registre em `modelo-inicial.txt`:

```text
new Tarefa() cria um novo objeto.
tarefaSelecionada = tarefaRevisarRequisitos copia a referência.
```

Não é necessário exibir nem comparar essas referências neste exercício. A finalidade é apenas praticar a leitura das declarações.

---

## Exercício 24 — Atualizar a saída da aplicação

Mantenha os objetos criados e substitua as mensagens antigas pela saída desta etapa:

```java
System.out.println("=== GESTOR DE TAREFAS ===");
System.out.println("Modelo orientado a objetos inicializado.");
System.out.println();
System.out.println("Objetos criados:");
System.out.println("- um responsável");
System.out.println("- tarefa: revisar requisitos");
System.out.println("- tarefa: implementar a tela inicial");
System.out.println("- tarefa: validar a aplicação");
System.out.println();
System.out.println("As três tarefas são objetos distintos da classe Tarefa.");
```

O arquivo completo será:

```java
package br.com.curso.tarefas;

import br.com.curso.tarefas.dominio.Responsavel;
import br.com.curso.tarefas.dominio.Tarefa;

public class AplicacaoGestorTarefas {

    public static void main(String[] args) {
        Responsavel responsavel = new Responsavel();

        Tarefa tarefaRevisarRequisitos = new Tarefa();
        Tarefa tarefaImplementarTela = new Tarefa();
        Tarefa tarefaValidarAplicacao = new Tarefa();

        Tarefa tarefaSelecionada = tarefaRevisarRequisitos;

        System.out.println("=== GESTOR DE TAREFAS ===");
        System.out.println("Modelo orientado a objetos inicializado.");
        System.out.println();
        System.out.println("Objetos criados:");
        System.out.println("- um responsável");
        System.out.println("- tarefa: revisar requisitos");
        System.out.println("- tarefa: implementar a tela inicial");
        System.out.println("- tarefa: validar a aplicação");
        System.out.println();
        System.out.println("As três tarefas são objetos distintos da classe Tarefa.");
    }
}
```

As mensagens continuam fixas. Não conclua que o texto `revisar requisitos` já foi colocado no atributo `titulo`. O objeto existe e a classe descreve esse atributo, mas o preenchimento dos objetos será estudado em outro tópico.

Essa separação é importante: não devemos usar um recurso antes de ele ser devidamente apresentado apenas para fazer a saída parecer mais avançada.

---

## Exercício 25 — Compilar e executar o projeto completo

Compile as classes do domínio e a classe de aplicação:

```bat
javac -d out src\br\com\curso\tarefas\dominio\*.java src\br\com\curso\tarefas\AplicacaoGestorTarefas.java
```

Execute:

```bat
java -cp out br.com.curso.tarefas.AplicacaoGestorTarefas
```

Resultado esperado:

```text
=== GESTOR DE TAREFAS ===
Modelo orientado a objetos inicializado.

Objetos criados:
- um responsável
- tarefa: revisar requisitos
- tarefa: implementar a tela inicial
- tarefa: validar a aplicação

As três tarefas são objetos distintos da classe Tarefa.
```

### O que mudou desde o Tópico 1

No Tópico 1, toda a aplicação estava representada por uma única classe com mensagens. Agora:

- `AplicacaoGestorTarefas` continua iniciando o programa;
- `Tarefa` representa o conceito de tarefa;
- `Responsavel` representa a pessoa associada;
- os atributos foram colocados nas classes às quais pertencem;
- os tipos comunicam o significado de cada informação;
- `Tarefa` usa `Responsavel` como um tipo por referência;
- as classes do domínio estão em um pacote próprio;
- a aplicação usa imports para acessar as classes desse pacote;
- vários objetos podem ser criados a partir da mesma classe.

---

## Exercício 26 — Testar os imports conscientemente

Faça os testes abaixo um por vez. Depois de cada teste, desfaça a alteração e restaure o código correto.

### Teste A — remover o import de `Tarefa`

Remova temporariamente:

```java
import br.com.curso.tarefas.dominio.Tarefa;
```

Tente compilar o projeto. O compilador deverá informar que não consegue encontrar o símbolo `Tarefa` dentro de `AplicacaoGestorTarefas`.

Restaure o import.

### Teste B — remover o import de `Responsavel`

Remova temporariamente:

```java
import br.com.curso.tarefas.dominio.Responsavel;
```

Compile novamente e observe o erro relacionado a `Responsavel`.

Restaure o import.

### Teste C — procurar um import inexistente entre classes do mesmo pacote

Abra `Tarefa.java`. Confirme que existe:

```java
Responsavel responsavel;
```

e que não foi necessário escrever:

```java
import br.com.curso.tarefas.dominio.Responsavel;
```

Isso acontece porque as duas classes pertencem ao pacote `br.com.curso.tarefas.dominio`.

### Teste D — confirmar os imports da biblioteca padrão

Em `Tarefa.java`, confirme:

```java
import java.math.BigDecimal;
import java.time.LocalDate;
```

Esses imports são necessários porque as classes pertencem a outros pacotes.

---

## Exercício 27 — Explicar a organização do projeto

Sem alterar o código, complete a tabela em `modelo-inicial.txt`:

```text
Classe: AplicacaoGestorTarefas
Pacote:
Responsabilidade:

Classe: Tarefa
Pacote:
Responsabilidade:

Classe: Responsavel
Pacote:
Responsabilidade:
```

Resposta esperada:

```text
Classe: AplicacaoGestorTarefas
Pacote: br.com.curso.tarefas
Responsabilidade: iniciar o programa e criar os objetos iniciais

Classe: Tarefa
Pacote: br.com.curso.tarefas.dominio
Responsabilidade: representar uma tarefa

Classe: Responsavel
Pacote: br.com.curso.tarefas.dominio
Responsabilidade: representar uma pessoa responsável
```

Depois, escreva o nome completo de cada classe:

```text
br.com.curso.tarefas.AplicacaoGestorTarefas
br.com.curso.tarefas.dominio.Tarefa
br.com.curso.tarefas.dominio.Responsavel
```

---

## Exercício 28 — Conferência final do Tópico 2

Confira o projeto completo.

### Estrutura

- [ ] O projeto continua na pasta `gestor-tarefas`.
- [ ] `AplicacaoGestorTarefas.java` está em `src\br\com\curso\tarefas`.
- [ ] A pasta `dominio` existe dentro de `src\br\com\curso\tarefas`.
- [ ] `Tarefa.java` está dentro de `dominio`.
- [ ] `Responsavel.java` está dentro de `dominio`.
- [ ] O arquivo `modelo-inicial.txt` está na raiz do projeto.

### Pacotes e imports

- [ ] `AplicacaoGestorTarefas` declara `package br.com.curso.tarefas;`.
- [ ] `Tarefa` declara `package br.com.curso.tarefas.dominio;`.
- [ ] `Responsavel` declara `package br.com.curso.tarefas.dominio;`.
- [ ] A aplicação importa `Tarefa` e `Responsavel`.
- [ ] `Tarefa` importa `BigDecimal` e `LocalDate`.
- [ ] `Tarefa` não precisa importar `Responsavel`, pois estão no mesmo pacote.

### Classes, atributos e tipos

- [ ] `Tarefa` representa um conceito, não uma tarefa específica.
- [ ] `Responsavel` representa um conceito separado.
- [ ] Os atributos foram colocados na classe que melhor os representa.
- [ ] Identificadores foram tratados como `String`.
- [ ] A quantidade de horas foi representada por `int`.
- [ ] A situação de conclusão foi representada por `boolean`.
- [ ] A data-limite foi representada por `LocalDate`.
- [ ] O custo estimado foi representado por `BigDecimal`.
- [ ] `Responsavel` foi usado como tipo de um atributo de `Tarefa`.

### Objetos e referências

- [ ] Um objeto `Responsavel` é criado no método `main`.
- [ ] Três objetos `Tarefa` são criados com três chamadas a `new Tarefa()`.
- [ ] A variável `tarefaSelecionada` recebe uma referência já existente.
- [ ] Você consegue explicar que `tarefaSelecionada` não cria uma quarta tarefa.

### Compilação e execução

- [ ] Todos os arquivos compilam sem erro.
- [ ] Os arquivos `.class` aparecem abaixo de `out` nos pacotes correspondentes.
- [ ] O programa executa pelo terminal.
- [ ] O programa executa pelo botão **Run** do VS Code.
- [ ] A saída corresponde ao resultado esperado.

---

## Entrega

Entregue a pasta inteira `gestor-tarefas`, contendo:

```text
gestor-tarefas/
├── modelo-inicial.txt
├── out/
└── src/
    └── br/
        └── com/
            └── curso/
                └── tarefas/
                    ├── AplicacaoGestorTarefas.java
                    └── dominio/
                        ├── Responsavel.java
                        └── Tarefa.java
```

## Código final de referência

Use esta seção apenas para conferir o resultado depois de concluir a sequência.

### `Responsavel.java`

```java
package br.com.curso.tarefas.dominio;

public class Responsavel {

    String matricula;
    String nome;
    boolean ativo;
}
```

### `Tarefa.java`

```java
package br.com.curso.tarefas.dominio;

import java.math.BigDecimal;
import java.time.LocalDate;

public class Tarefa {

    String codigo;
    String titulo;
    String descricao;
    int estimativaHoras;
    boolean concluida;
    LocalDate dataLimite;
    BigDecimal custoEstimado;
    Responsavel responsavel;
}
```

### `AplicacaoGestorTarefas.java`

```java
package br.com.curso.tarefas;

import br.com.curso.tarefas.dominio.Responsavel;
import br.com.curso.tarefas.dominio.Tarefa;

public class AplicacaoGestorTarefas {

    public static void main(String[] args) {
        Responsavel responsavel = new Responsavel();

        Tarefa tarefaRevisarRequisitos = new Tarefa();
        Tarefa tarefaImplementarTela = new Tarefa();
        Tarefa tarefaValidarAplicacao = new Tarefa();

        Tarefa tarefaSelecionada = tarefaRevisarRequisitos;

        System.out.println("=== GESTOR DE TAREFAS ===");
        System.out.println("Modelo orientado a objetos inicializado.");
        System.out.println();
        System.out.println("Objetos criados:");
        System.out.println("- um responsável");
        System.out.println("- tarefa: revisar requisitos");
        System.out.println("- tarefa: implementar a tela inicial");
        System.out.println("- tarefa: validar a aplicação");
        System.out.println();
        System.out.println("As três tarefas são objetos distintos da classe Tarefa.");
    }
}
```

## Comandos de referência

Execute a partir da pasta `gestor-tarefas`:

```bat
javac -d out src\br\com\curso\tarefas\dominio\*.java src\br\com\curso\tarefas\AplicacaoGestorTarefas.java
java -cp out br.com.curso.tarefas.AplicacaoGestorTarefas
```

Ao concluir esta etapa, o Gestor de Tarefas ainda é pequeno, mas sua organização mudou de maneira importante. O programa agora possui conceitos próprios, responsabilidades separadas, tipos escolhidos pelo significado, objetos concretos e pacotes coerentes. A experiência com processos continua válida; a diferença é que o fluxo futuro trabalhará com elementos do domínio em vez de depender de um único bloco concentrando tudo.
