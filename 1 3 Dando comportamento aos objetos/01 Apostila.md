# Tópico 3 — Dar comportamento aos objetos

## Os objetos começam a agir

No tópico anterior, você organizou o sistema de Gestão de Biblioteca em classes como `Livro`, `Usuario`, `Emprestimo` e `Biblioteca`. Cada classe passou a representar um conceito, e os atributos registraram informações pertencentes a esses conceitos.

Essa estrutura já foi uma mudança importante. Porém, até aquele momento, os objetos apenas possuíam estado. Um `Livro` sabia que poderia ter ISBN, título, número de páginas e disponibilidade, mas ainda não sabia fazer nada com essas informações.

Agora entra a segunda metade da ideia:

```text
objeto = estado + comportamento
```

O estado é representado pelos atributos. O comportamento é representado principalmente pelos métodos. Um livro pode informar se está disponível, aceitar um empréstimo, recusar outro empréstimo enquanto estiver indisponível, registrar uma devolução e controlar suas renovações.

Isso não significa abandonar o pensamento procedural. Dentro de um método continuam existindo instruções, condições, repetições e uma ordem de execução. A diferença é que esse procedimento passa a morar junto da responsabilidade à qual pertence.

Você já conhece a importância de identificar um processo, organizar etapas e proteger regras de negócio. O novo passo é responder:

> Qual objeto deve ser responsável por executar cada parte desse processo?

Este tópico trabalhará métodos, parâmetros, retornos, encapsulamento, condições e repetições. Cada conceito será introduzido antes de aparecer nos exemplos completos.

## O que você compreenderá

- o que é um método e como ele representa comportamento;
- como declarar e chamar métodos;
- a diferença entre método de instância e ponto de entrada `main`;
- o papel dos parâmetros e dos argumentos;
- o que significa um método retornar um valor;
- a diferença entre `void` e um tipo de retorno;
- como `private` e `public` participam do encapsulamento;
- por que encapsular não significa apenas gerar getters e setters;
- como expressar condições com `if`, `else if` e `else`;
- como comparar valores primitivos e textos;
- como relacionar `switch` com a ideia conhecida de `EVALUATE`;
- como repetir instruções com `while`, `do-while` e `for`;
- como relacionar essas estruturas a `IF`, `EVALUATE` e `PERFORM` do COBOL;
- como reunir esses recursos em uma pequena aplicação de Gestão de Biblioteca.

---

## 1. Do dado ao comportamento

Considere a classe construída anteriormente:

```java
public class Livro {
    String isbn;
    String titulo;
    String autor;
    int numeroPaginas;
    boolean disponivel;
}
```

Ela descreve informações de um livro. Entretanto, se qualquer outra parte do sistema puder alterar diretamente `disponivel`, será possível escrever:

```java
livro.disponivel = false;
```

A linha muda o estado, mas não explica por que a mudança aconteceu. Foi empréstimo? Reserva? Manutenção? Baixa do acervo? Também não existe uma regra impedindo alterações incoerentes.

Uma solução orientada a objetos é pedir ao próprio objeto que realize uma operação:

```java
livro.emprestar();
```

A leitura muda de “altere o campo disponibilidade” para “peça ao livro que realize o empréstimo”. O método pode verificar o estado atual antes de permitir a mudança.

No pensamento procedural, talvez você já tenha uma rotina como:

```cobol
PERFORM EMPRESTAR-LIVRO
```

A existência de uma operação nomeada não é novidade. A mudança está na associação explícita entre comportamento e objeto:

```java
livro.emprestar();
```

O nome antes do ponto informa quem recebe a solicitação. O nome depois do ponto informa o comportamento solicitado.

---

## 2. Método: um comportamento com nome

Um **método** é um bloco de código com nome, declarado dentro de uma classe. Ele pode executar uma ação, receber informações e devolver um resultado.

Comecemos com o formato mais simples:

```java
public void exibirIdentificacao() {
    System.out.println("Livro do acervo");
}
```

Leia cada parte:

| Parte | Significado |
|---|---|
| `public` | o método pode ser acessado por outras classes |
| `void` | o método não devolve um valor |
| `exibirIdentificacao` | nome do método |
| `()` | não há parâmetros |
| `{ }` | delimitam as instruções do método |

O método é declarado dentro da classe, mas fora de outros métodos:

```java
public class Livro {

    public void exibirIdentificacao() {
        System.out.println("Livro do acervo");
    }
}
```

### Declarar não é executar

Declarar o método define o comportamento. Para executá-lo, é preciso chamá-lo por meio de um objeto:

```java
Livro livro = new Livro();
livro.exibirIdentificacao();
```

Na segunda linha:

- `livro` é a referência;
- o ponto seleciona algo pertencente ao objeto;
- `exibirIdentificacao` é o método;
- `()` indica a chamada.

Em COBOL, um parágrafo pode agrupar instruções e ser executado com `PERFORM`. Essa é uma ponte útil, mas não uma equivalência completa. Um método Java pertence a uma classe, pode operar diretamente sobre o estado do objeto, possui regras de acesso e pode receber e retornar valores de forma declarada em sua assinatura.

### O método `main`

Você já utilizou um método desde o primeiro tópico:

```java
public static void main(String[] args) {
}
```

O `main` é especial porque serve como ponto de entrada da aplicação. A JVM pode iniciá-lo sem criar antes um objeto de `AplicacaoBiblioteca`, por isso aparece `static`.

Os comportamentos de domínio que veremos serão métodos de instância. Para chamar `emprestar`, será necessário possuir uma referência para um objeto `Livro`:

```java
livro.emprestar();
```

Não é necessário aprofundar `static` neste momento. Basta distinguir:

- `main`: inicia a aplicação;
- métodos do objeto: representam comportamentos associados àquele objeto.

### Nomes de métodos

Métodos normalmente recebem nomes que começam com verbo e usam *camelCase*:

```text
emprestar
devolver
renovar
estaDisponivel
obterTitulo
```

O nome deve comunicar intenção. `alterarFlag()` diz pouco sobre o negócio. `emprestar()` mostra o que está acontecendo.

---

## 3. Parâmetros: informações necessárias para o método

Alguns comportamentos precisam receber informações. Para cadastrar os dados iniciais de um livro, podemos declarar:

```java
public void cadastrar(String isbn, String titulo, String autor, int numeroPaginas) {
}
```

Os itens entre parênteses são **parâmetros**. Cada parâmetro possui tipo e nome:

| Parâmetro | Tipo | Informação recebida |
|---|---|---|
| `isbn` | `String` | identificador editorial |
| `titulo` | `String` | título do livro |
| `autor` | `String` | autoria |
| `numeroPaginas` | `int` | quantidade de páginas |

Ao chamar o método, fornecemos **argumentos**:

```java
livro.cadastrar(
        "9788535914849",
        "Dom Casmurro",
        "Machado de Assis",
        256
);
```

Os parâmetros aparecem na declaração. Os argumentos aparecem na chamada.

```text
declaração: cadastrar(String isbn, String titulo, String autor, int numeroPaginas)
chamada:    cadastrar("978...", "Dom Casmurro", "Machado de Assis", 256)
```

O compilador verifica a quantidade, a ordem e os tipos. Se o método espera quatro parâmetros, a chamada precisa fornecer quatro argumentos compatíveis.

### Parâmetros e COBOL

Em COBOL, programas e subprogramas podem receber dados por meio da `LINKAGE SECTION` e de `USING`. Métodos Java também recebem informações declaradas, mas cada método define seus parâmetros diretamente na assinatura.

Uma aproximação conceitual é:

| Intenção | COBOL | Java |
|---|---|---|
| declarar dados recebidos | itens na `LINKAGE SECTION` | parâmetros na assinatura |
| informar dados na chamada | `CALL ... USING` | argumentos entre parênteses |
| identificar o contrato | ordem e definição dos itens | tipos, ordem e quantidade dos parâmetros |

O mecanismo de memória não deve ser tratado como idêntico. A comparação serve para reconhecer a intenção: uma rotina precisa saber quais informações recebe.

### A palavra `this`

Dentro do método, o parâmetro `titulo` possui o mesmo nome do atributo `titulo`. Para indicar explicitamente o atributo do objeto atual, usamos `this`:

```java
public class Livro {

    String titulo;

    public void cadastrar(String titulo) {
        this.titulo = titulo;
    }
}
```

Leia a atribuição:

```text
this.titulo = titulo;
atributo       parâmetro
do objeto      recebido
```

`this` significa “este objeto”. Se a chamada for `livro.cadastrar("Dom Casmurro")`, o atributo do objeto referenciado por `livro` receberá o argumento informado.

### O que é passado ao método

Java passa argumentos **por valor**. Para tipos primitivos, o método recebe uma cópia do valor. Para objetos, recebe uma cópia da referência. Isso não significa copiar o objeto inteiro.

Neste tópico, guarde principalmente que o parâmetro é uma variável local do método. Ele existe durante aquela chamada e permite que o método trabalhe com a informação recebida.

---

## 4. Retornos: o método responde alguma coisa

Um método pode apenas executar uma ação:

```java
public void exibirIdentificacao() {
    System.out.println("Livro do acervo");
}
```

Como o tipo de retorno é `void`, nenhuma informação é devolvida à chamada.

Outro método pode responder uma pergunta:

```java
public boolean estaDisponivel() {
    return disponivel;
}
```

Agora, `boolean` aparece antes do nome do método. Isso declara que o resultado será `true` ou `false`. A palavra `return` encerra a execução do método e entrega o valor.

```java
boolean podeEmprestar = livro.estaDisponivel();
```

Também é possível usar diretamente o resultado:

```java
System.out.println(livro.estaDisponivel());
```

### Ação e consulta

Uma distinção útil é:

| Intenção | Exemplo | Retorno |
|---|---|---|
| executar uma ação | `livro.devolver()` | pode ser `void` ou informar sucesso |
| responder uma pergunta | `livro.estaDisponivel()` | `boolean` |
| obter uma informação | `livro.obterTitulo()` | `String` |
| calcular um resultado | `emprestimo.calcularDiasAtraso()` | `int` |

Exemplo com texto:

```java
public String obterTitulo() {
    return titulo;
}
```

Exemplo com inteiro:

```java
public int obterNumeroRenovacoes() {
    return numeroRenovacoes;
}
```

O valor retornado precisa ser compatível com o tipo declarado. Um método `boolean` não pode retornar uma `String`.

### Retornar não é exibir

Estas operações são diferentes:

```java
return titulo;
```

```java
System.out.println(titulo);
```

`return` entrega um valor a quem chamou o método. `println` escreve uma representação no terminal. Um método que retorna o título pode ser usado por uma tela, um relatório, um teste ou outra regra. Um método que apenas imprime fica preso à saída do terminal.

---

## 5. Encapsulamento: proteger o estado do objeto

**Encapsulamento** é o princípio de manter o estado e as regras relacionadas sob controle da classe. O objeto não deve ser apenas uma caixa aberta que qualquer parte do sistema altera livremente.

No tópico anterior, usamos atributos sem modificador para apresentar o modelo. Agora vamos torná-los privados:

```java
public class Livro {

    private String isbn;
    private String titulo;
    private String autor;
    private int numeroPaginas;
    private boolean disponivel;
}
```

`private` informa que o atributo pode ser acessado diretamente apenas dentro da própria classe `Livro`.

Em outra classe, isto deixa de ser permitido:

```java
livro.disponivel = false;
```

Em vez disso, a mudança acontece por um comportamento público:

```java
boolean realizado = livro.emprestar("U0001");
```

O método público funciona como uma porta de entrada controlada. Ele pode conferir o estado, aplicar a regra, realizar a alteração e comunicar o resultado.

### `public` e `private`

Neste momento, pense nos modificadores assim:

| Modificador | Uso inicial |
|---|---|
| `private` | detalhe interno protegido pela classe |
| `public` | operação oferecida às outras partes do sistema |

Encapsular não significa esconder tudo sem permitir uso. Significa oferecer uma interface coerente e proteger detalhes internos.

### Encapsulamento não é apenas getter e setter

Um código pode ter atributos privados e ainda expor alterações sem regra:

```java
public void setDisponivel(boolean disponivel) {
    this.disponivel = disponivel;
}
```

Esse setter permite que qualquer parte diga arbitrariamente `true` ou `false`. Ele protege a sintaxe de acesso, mas não expressa a regra do domínio.

Compare com:

```java
public void emprestar(String matriculaUsuario) {
    disponivel = false;
    matriculaUsuarioAtual = matriculaUsuario;
}
```

Agora a alteração possui significado: aconteceu um empréstimo para determinada matrícula. Na próxima seção, acrescentaremos uma condição para impedir um novo empréstimo quando o livro já estiver indisponível.

Métodos de consulta, como `obterTitulo()`, podem ser úteis. Métodos de alteração genéricos não devem ser criados automaticamente para todos os atributos. A pergunta não é “qual setter falta?”, mas “qual comportamento o objeto precisa oferecer?”.

### Relação com COBOL

Programas COBOL bem estruturados também podem proteger regras dentro de módulos e controlar quais dados atravessam uma interface de chamada. Portanto, a preocupação com limites não nasce em Java.

Em Java, porém, a classe e os modificadores de acesso tornam essa fronteira parte direta do modelo. O compilador impede o acesso direto a um atributo `private` feito por outra classe.

---

## 6. Condições: decisões dentro dos métodos

Uma condição permite executar um bloco apenas quando determinada expressão é verdadeira.

Em COBOL:

```cobol
IF LIVRO-DISPONIVEL
    MOVE 'N' TO WS-LIVRO-DISPONIVEL
ELSE
    DISPLAY 'EMPRESTIMO NAO PERMITIDO'
END-IF
```

Em Java:

```java
if (disponivel) {
    disponivel = false;
} else {
    System.out.println("Empréstimo não permitido");
}
```

As chaves delimitam os blocos. A condição entre parênteses precisa resultar em `boolean`.

### `if`, `else if` e `else`

Quando há mais de duas possibilidades:

```java
if (numeroRenovacoes == 0) {
    System.out.println("Nenhuma renovação realizada");
} else if (numeroRenovacoes == 1) {
    System.out.println("Uma renovação realizada");
} else {
    System.out.println("Limite de renovações alcançado");
}
```

O Java avalia as condições de cima para baixo. Quando encontra a primeira verdadeira, executa o bloco correspondente e ignora os demais da cadeia.

### Operadores de comparação

| Operador | Significado |
|---|---|
| `==` | igual |
| `!=` | diferente |
| `>` | maior |
| `<` | menor |
| `>=` | maior ou igual |
| `<=` | menor ou igual |

Operadores lógicos permitem combinar ou inverter condições:

| Operador | Significado |
|---|---|
| `&&` | e |
| `||` | ou |
| `!` | negação |

O operador `++` aumenta uma variável numérica em uma unidade:

```java
numeroRenovacoes++;
```

Neste uso, ele produz o mesmo novo valor de:

```java
numeroRenovacoes = numeroRenovacoes + 1;
```

Em COBOL, a intenção poderia ser expressa por `ADD 1 TO WS-NUMERO-RENOVACOES`.

Exemplo:

```java
if (!disponivel && numeroRenovacoes < 2) {
    numeroRenovacoes++;
}
```

A condição significa: o livro não está disponível **e** o número de renovações é menor que dois.

### Condição com retorno antecipado

Agora podemos completar a regra que começou na seção de encapsulamento:

```java
public boolean emprestar(String matriculaUsuario) {
    if (!disponivel) {
        return false;
    }

    disponivel = false;
    matriculaUsuarioAtual = matriculaUsuario;
    return true;
}
```

Se o livro estiver indisponível, o método termina em `return false`. As instruções seguintes não são executadas. Se estiver disponível, o estado é alterado e o método retorna `true`.

### Booleanos não precisam ser comparados com `true`

Prefira:

```java
if (disponivel) {
}
```

em vez de:

```java
if (disponivel == true) {
}
```

E prefira:

```java
if (!disponivel) {
}
```

em vez de:

```java
if (disponivel == false) {
}
```

O próprio `boolean` já é a condição.

### Comparação de `String`

Para valores primitivos, `==` compara valores. Para textos, use `equals` quando quiser comparar o conteúdo:

```java
if ("EMPRESTAR".equals(operacao)) {
    System.out.println("Operação de empréstimo selecionada");
}
```

Não use como regra geral:

```java
if (operacao == "EMPRESTAR") {
}
```

Em tipos por referência, `==` verifica se as referências apontam para o mesmo objeto, não se dois objetos possuem conteúdo equivalente. Essa diferença é especialmente importante com `String`.

---

## 7. `switch` e a ponte com `EVALUATE`

Quando uma informação pode assumir alternativas conhecidas, `switch` pode deixar a seleção mais clara.

Em COBOL:

```cobol
EVALUATE WS-OPERACAO
    WHEN 'E'
        PERFORM EMPRESTAR-LIVRO
    WHEN 'D'
        PERFORM DEVOLVER-LIVRO
    WHEN 'R'
        PERFORM RENOVAR-EMPRESTIMO
    WHEN OTHER
        DISPLAY 'OPERACAO INVALIDA'
END-EVALUATE
```

Em Java:

```java
switch (operacao) {
    case "E":
        System.out.println("Emprestar livro");
        break;
    case "D":
        System.out.println("Devolver livro");
        break;
    case "R":
        System.out.println("Renovar empréstimo");
        break;
    default:
        System.out.println("Operação inválida");
}
```

Nesta forma do `switch`, `break` encerra o caso atual. Sem ele, a execução pode continuar nos casos seguintes. `default` corresponde à alternativa usada quando nenhum caso combina.

`switch` não substitui todo `if`. Use `if` quando a decisão envolve intervalos ou expressões diferentes, como `diasAtraso > 0 && usuarioAtivo`. Use `switch` quando o mesmo valor é comparado com alternativas bem definidas.

---

## 8. Repetições: executar um bloco mais de uma vez

Repetições não desaparecem na orientação a objetos. Elas continuam úteis quando uma operação precisa ocorrer várias vezes. O cuidado é colocá-las no método cuja responsabilidade combina com o processo.

### `while`: repetir enquanto a condição for verdadeira

No exemplo, o operador `+` juntará o texto ao valor numérico para formar a mensagem exibida.

```java
int numeroAviso = 1;

while (numeroAviso <= 3) {
    System.out.println("Enviando aviso " + numeroAviso);
    numeroAviso++;
}
```

O fluxo é:

1. verificar a condição;
2. executar o bloco se ela for verdadeira;
3. atualizar a variável;
4. voltar à condição.

Uma aproximação em COBOL seria:

```cobol
MOVE 1 TO WS-NUMERO-AVISO

PERFORM UNTIL WS-NUMERO-AVISO > 3
    DISPLAY 'ENVIANDO AVISO ' WS-NUMERO-AVISO
    ADD 1 TO WS-NUMERO-AVISO
END-PERFORM
```

Observe uma diferença de leitura: `while` continua **enquanto** sua condição é verdadeira; `PERFORM UNTIL` continua **até que** sua condição de término se torne verdadeira. Por isso, condições equivalentes frequentemente aparecem invertidas.

O `while` pode executar zero vezes, pois testa a condição antes do bloco.

### `do-while`: executar antes de testar

```java
int tentativa = 1;

do {
    System.out.println("Tentativa " + tentativa);
    tentativa++;
} while (tentativa <= 3);
```

O bloco executa pelo menos uma vez. Em COBOL, a ideia se aproxima de um `PERFORM ... WITH TEST AFTER`.

Observe o ponto e vírgula depois da condição do `do-while`:

```java
} while (tentativa <= 3);
```

### `for`: repetição com controle concentrado

Quando início, condição e atualização pertencem ao mesmo controle, `for` costuma ser claro:

```java
for (int numeroExemplar = 1; numeroExemplar <= 3; numeroExemplar++) {
    System.out.println("Conferindo exemplar " + numeroExemplar);
}
```

Leia o cabeçalho em três partes:

```text
int numeroExemplar = 1   → inicialização
numeroExemplar <= 3      → condição
numeroExemplar++         → atualização
```

Uma ponte COBOL seria:

```cobol
PERFORM VARYING WS-NUMERO-EXEMPLAR FROM 1 BY 1
        UNTIL WS-NUMERO-EXEMPLAR > 3
    DISPLAY 'CONFERINDO EXEMPLAR ' WS-NUMERO-EXEMPLAR
END-PERFORM
```

Novamente, o `for` declara a condição de continuidade (`numeroExemplar <= 3`), enquanto o `UNTIL` COBOL declara a condição de encerramento (`WS-NUMERO-EXEMPLAR > 3`).

### Qual repetição escolher?

| Estrutura | Uso inicial |
|---|---|
| `while` | repetir enquanto uma condição permanecer verdadeira |
| `do-while` | executar ao menos uma vez e depois decidir se continua |
| `for` | repetir com contador ou controle bem definido |

Não escolha pela quantidade de caracteres. Escolha pela intenção que a estrutura comunica.

### Cuidado com repetição infinita

Neste código, `numeroAviso` nunca muda:

```java
int numeroAviso = 1;

while (numeroAviso <= 3) {
    System.out.println("Enviando aviso");
}
```

Como a condição continua verdadeira, o laço não termina. Sempre identifique:

- qual condição mantém a repetição;
- o que muda durante cada passagem;
- em que momento a condição se tornará falsa.

Ainda não usaremos coleções para percorrer o acervo. Nesta etapa, as repetições trabalharão com controles simples. Listas e outras coleções serão apresentadas em seu tópico próprio.

---

## 9. Colocando comportamento em `Livro`

Agora podemos reunir estado, métodos, parâmetros, retornos, encapsulamento e condições.

```java
package br.com.curso.biblioteca.dominio;

public class Livro {

    private String isbn;
    private String titulo;
    private String autor;
    private int numeroPaginas;
    private boolean disponivel;
    private String matriculaUsuarioAtual;
    private int numeroRenovacoes;

    public void cadastrar(
            String isbn,
            String titulo,
            String autor,
            int numeroPaginas
    ) {
        this.isbn = isbn;
        this.titulo = titulo;
        this.autor = autor;
        this.numeroPaginas = numeroPaginas;
        this.disponivel = true;
        this.matriculaUsuarioAtual = null;
        this.numeroRenovacoes = 0;
    }

    public boolean emprestar(String matriculaUsuario) {
        if (!disponivel) {
            return false;
        }

        disponivel = false;
        matriculaUsuarioAtual = matriculaUsuario;
        numeroRenovacoes = 0;
        return true;
    }

    public boolean renovar() {
        if (disponivel || numeroRenovacoes >= 2) {
            return false;
        }

        numeroRenovacoes++;
        return true;
    }

    public boolean devolver() {
        if (disponivel) {
            return false;
        }

        disponivel = true;
        matriculaUsuarioAtual = null;
        numeroRenovacoes = 0;
        return true;
    }

    public String obterTitulo() {
        return titulo;
    }

    public boolean estaDisponivel() {
        return disponivel;
    }

    public int obterNumeroRenovacoes() {
        return numeroRenovacoes;
    }
}
```

### O que cada método protege

| Método | Responsabilidade |
|---|---|
| `cadastrar` | registrar os dados iniciais usados neste exemplo |
| `emprestar` | impedir empréstimo quando o livro está indisponível |
| `renovar` | exigir empréstimo ativo e limitar a duas renovações |
| `devolver` | impedir devolução de livro já disponível e limpar o estado |
| `obterTitulo` | fornecer o título sem permitir alteração direta |
| `estaDisponivel` | responder a situação atual |
| `obterNumeroRenovacoes` | informar quantas renovações estão registradas |

O método `cadastrar` é uma solução didática temporária para inicializar o objeto sem introduzir construtores personalizados neste tópico. Em projetos reais, a criação consistente de objetos merece uma estratégia própria.

Observe também que `isbn`, `titulo`, `disponivel` e os demais atributos não são alterados diretamente pela aplicação. A classe controla seu estado.

---

## 10. Um objeto coordenador e a repetição

`Livro` protege as regras que dependem do próprio estado. `Biblioteca` pode coordenar um pequeno fluxo e comunicar o resultado:

```java
package br.com.curso.biblioteca.dominio;

public class Biblioteca {

    public void processarEmprestimo(Livro livro, String matriculaUsuario) {
        boolean realizado = livro.emprestar(matriculaUsuario);

        if (realizado) {
            System.out.println("Empréstimo realizado: " + livro.obterTitulo());
        } else {
            System.out.println("Empréstimo recusado: livro indisponível");
        }
    }

    public void tentarRenovacoes(Livro livro, int quantidadeTentativas) {
        for (int tentativa = 1; tentativa <= quantidadeTentativas; tentativa++) {
            boolean renovado = livro.renovar();

            if (renovado) {
                System.out.println("Renovação " + tentativa + " autorizada");
            } else {
                System.out.println("Renovação " + tentativa + " recusada");
            }
        }
    }
}
```

A distribuição é intencional:

- `Livro` decide se seu estado permite empréstimo ou renovação;
- `Biblioteca` coordena chamadas e apresenta o resultado do fluxo;
- a repetição não modifica diretamente os atributos de `Livro`;
- cada tentativa chama um comportamento público do objeto.

Se `Biblioteca` alterasse `numeroRenovacoes` diretamente, a regra ficaria espalhada. Como o atributo é `private`, isso também seria impedido pelo compilador.

---

## 11. A aplicação iniciando o fluxo

```java
package br.com.curso.biblioteca;

import br.com.curso.biblioteca.dominio.Biblioteca;
import br.com.curso.biblioteca.dominio.Livro;

public class AplicacaoBiblioteca {

    public static void main(String[] args) {
        Livro livro = new Livro();
        livro.cadastrar(
                "9788535914849",
                "Dom Casmurro",
                "Machado de Assis",
                256
        );

        Biblioteca biblioteca = new Biblioteca();
        biblioteca.processarEmprestimo(livro, "U0001");
        biblioteca.tentarRenovacoes(livro, 3);

        System.out.println(
                "Renovações registradas: " + livro.obterNumeroRenovacoes()
        );
    }
}
```

Saída esperada:

```text
Empréstimo realizado: Dom Casmurro
Renovação 1 autorizada
Renovação 2 autorizada
Renovação 3 recusada
Renovações registradas: 2
```

Leia o fluxo:

1. `AplicacaoBiblioteca` cria um objeto `Livro`.
2. O método `cadastrar` recebe os dados iniciais.
3. A aplicação cria um objeto `Biblioteca`.
4. `Biblioteca` solicita o empréstimo ao `Livro`.
5. `Livro` verifica a própria disponibilidade e responde `true`.
6. `Biblioteca` repete três tentativas de renovação.
7. `Livro` autoriza duas e recusa a terceira.
8. A aplicação consulta o total final sem acessar o atributo diretamente.

Existe sequência, decisão e repetição. O programa não deixou de ser procedural internamente. A diferença é que cada procedimento foi colocado próximo da responsabilidade que conhece e protege.

---

## 12. Pontes cuidadosas entre COBOL e Java

| Necessidade | Estrutura conhecida em COBOL | Estrutura usada em Java | Cuidado |
|---|---|---|---|
| nomear uma rotina | parágrafo, seção ou subprograma | método | método pertence a uma classe |
| executar uma rotina | `PERFORM` ou `CALL` | chamada com `objeto.metodo()` | a chamada é direcionada a um objeto |
| receber informações | `USING` e `LINKAGE SECTION` | parâmetros | os mecanismos não são idênticos |
| devolver resultado | `RETURNING` ou área compartilhada | tipo de retorno e `return` | Java declara o retorno na assinatura |
| decidir | `IF` / `ELSE` | `if` / `else` | a condição Java resulta em `boolean` |
| selecionar alternativas | `EVALUATE` | `switch` | cada estrutura possui regras próprias |
| repetir por condição | `PERFORM UNTIL` | `while` | observe quando a condição é testada |
| repetir com contador | `PERFORM VARYING` | `for` | inicialização, condição e atualização ficam juntas |
| proteger detalhes | fronteira de programas e módulos | `private` e métodos públicos | o compilador aplica o acesso da classe |

As comparações reduzem o estranhamento, mas não substituem o aprendizado do modelo Java. A meta não é descobrir “qual palavra Java traduz cada palavra COBOL”. A meta é aproveitar uma experiência real para compreender uma nova forma de expressar responsabilidades.

---

## 13. Erros de compreensão que vale evitar

### “Método é apenas um parágrafo COBOL com chaves”

Um método contém instruções como uma rotina procedural, mas pertence a uma classe, opera sobre o estado do objeto e possui assinatura, retorno e regras de acesso.

### “Todo método deve ser `void`”

`void` serve quando não há valor devolvido. Consultas e cálculos normalmente precisam retornar um resultado adequado.

### “Retornar e imprimir são a mesma coisa”

`return` entrega um valor a quem chamou. `println` escreve no terminal.

### “Parâmetro e argumento são sinônimos perfeitos”

Parâmetro está na declaração do método. Argumento é o valor fornecido na chamada.

### “Atributo privado precisa obrigatoriamente de setter”

Um método de negócio como `emprestar` protege melhor a intenção do que `setDisponivel(false)`.

### “Encapsular é apenas colocar `private`”

`private` é uma ferramenta. Encapsulamento também exige oferecer operações coerentes e manter regras dentro da classe responsável.

### “`==` compara o conteúdo de qualquer coisa”

Para objetos, `==` compara referências. Para conteúdo textual, use `equals`.

### “Orientação a objetos elimina `if` e repetição”

Condições e repetições continuam existindo dentro dos métodos. O que muda é onde o comportamento fica e qual objeto protege a regra.

### “Quanto mais métodos, mais orientado a objetos”

Quantidade não garante boa modelagem. Métodos precisam representar responsabilidades coerentes.

---

## 14. Checklist do Tópico 3

- [ ] Entendo que objetos reúnem estado e comportamento.
- [ ] Sei identificar a declaração de um método.
- [ ] Sei chamar um método por meio de um objeto.
- [ ] Distingo declaração de método e execução do método.
- [ ] Entendo a finalidade inicial de `public`, `private` e `void`.
- [ ] Distingo parâmetro e argumento.
- [ ] Sei que quantidade, ordem e tipos dos argumentos precisam ser compatíveis.
- [ ] Compreendo o uso de `this` para indicar o objeto atual.
- [ ] Sei que Java passa argumentos por valor.
- [ ] Entendo que uma referência copiada não representa a cópia do objeto inteiro.
- [ ] Distingo método de ação, consulta e cálculo.
- [ ] Sei que `return` entrega um valor e encerra o método.
- [ ] Não confundo `return` com `System.out.println`.
- [ ] Entendo encapsulamento como proteção do estado e das regras.
- [ ] Sei por que um método de negócio pode ser melhor que um setter genérico.
- [ ] Consigo ler condições com `if`, `else if` e `else`.
- [ ] Reconheço operadores de comparação e operadores lógicos.
- [ ] Sei usar `equals` para comparar conteúdo de `String`.
- [ ] Entendo a aproximação entre `EVALUATE` e `switch`.
- [ ] Distingo `while`, `do-while` e `for`.
- [ ] Consigo relacionar `PERFORM UNTIL` a `while`.
- [ ] Consigo relacionar `PERFORM VARYING` a `for`.
- [ ] Sei identificar a condição de encerramento de uma repetição.
- [ ] Entendo por que `Livro` protege a regra e `Biblioteca` coordena o fluxo.

## Fechamento

No tópico anterior, as classes deram nomes aos elementos do domínio. Neste, esses elementos começaram a agir.

Métodos transformam responsabilidades em operações. Parâmetros permitem que as operações recebam informações. Retornos permitem que respondam resultados. O encapsulamento mantém o estado sob controle. Condições expressam decisões. Repetições permitem executar uma parte do processo quantas vezes forem necessárias.

Nada disso invalida o conhecimento construído em COBOL. `IF`, `EVALUATE`, `PERFORM`, rotinas e contratos de chamada continuam oferecendo pontos de referência. Você ainda acompanha fluxo, dados, decisões e efeitos. A mudança é que, em Java orientado a objetos, essas estruturas passam a colaborar com classes que representam o domínio.

Na Gestão de Biblioteca, `Livro` deixou de ser apenas um conjunto de campos. Ele pode proteger sua disponibilidade, controlar renovações e responder informações. `Biblioteca` pode coordenar o processo sem invadir o estado de `Livro`. `AplicacaoBiblioteca` pode iniciar o fluxo sem carregar todas as regras.

Essa distribuição reduz a concentração de responsabilidades e prepara o sistema para crescer. A orientação a objetos começa a ficar concreta quando você deixa de perguntar apenas “qual sequência devo executar?” e passa também a perguntar “quem deve conhecer e executar esta regra?”.
