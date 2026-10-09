# Roteiro de videoaula — Tópico 3: Dar comportamento aos objetos

## Informações gerais

- **Tema:** métodos, parâmetros, retornos, encapsulamento, condições e repetições.
- **Público:** profissionais em transição de COBOL para Java.
- **Exemplo central:** aplicação de gestão bancária.
- **Duração sugerida:** 100 a 120 minutos.
- **Formato:** exposição por tópicos + comparações COBOL/Java + construção progressiva no VS Code.
- **Ponto de partida:** projeto bancário do Tópico 2, dividido em pacotes e classes.
- **Objetivo:** transformar objetos que apenas guardam dados em objetos que protegem estado e executam comportamentos.
- **Resultado concreto:** `Conta` com atributos privados e operações de depósito, saque, bloqueio e consulta; `Banco` coordenando tentativas de saque; `AplicacaoBanco` iniciando o fluxo.

### Fora do escopo

- construtores personalizados;
- sobrecarga de métodos;
- herança e polimorfismo;
- interfaces;
- exceções;
- coleções;
- entrada pelo teclado;
- persistência e banco de dados;
- APIs REST;
- testes automatizados;
- concorrência e transações bancárias reais.

### Mensagem que deve atravessar toda a gravação

> O pensamento procedural continua dentro dos métodos. A orientação a objetos define quem conhece o estado e quem deve executar cada regra.

### Resultado conceitual esperado

- Entender que classe não reúne apenas atributos.
- Ler e escrever a assinatura de um método.
- Distinguir parâmetros, argumentos e retornos.
- Compreender encapsulamento como proteção de regras, não como geração automática de getters e setters.
- Relacionar `IF`, `EVALUATE` e `PERFORM` às estruturas Java sem tratá-las como traduções exatas.

---

## 1. Abertura — os objetos deixam de ser estruturas passivas

### Retomar o ponto final do Tópico 2

```java
public class Conta {
    String numero;
    String agencia;
    boolean ativa;
    BigDecimal saldo;
}
```

### Pergunta de abertura

- “A classe já representa uma conta, mas quem garante que o saldo será alterado corretamente?”
- “Quem impede saque em conta inativa?”
- “Quem impede que outra classe coloque qualquer valor diretamente no saldo?”
- “A conta apenas conhece dados ou também deve proteger regras relacionadas a esses dados?”

### Pontos para comentar

- No Tópico 2, o ganho foi estrutural.
- `Conta`, `Cliente`, `Transacao` e `Banco` passaram a representar conceitos diferentes.
- Os atributos descreveram o estado.
- Agora serão adicionados comportamentos.
- Objeto passa a ser lido como:

```text
estado + comportamento
```

### Frases de condução possíveis

- “Até agora, nossa conta sabia o que possuía, mas não sabia o que fazer.”
- “O procedural não desaparece; ele passa a existir dentro de responsabilidades mais bem definidas.”
- “A pergunta deixa de ser apenas ‘como alterar o saldo?’ e passa a incluir ‘quem tem autoridade para alterar o saldo?’.”

---

## 2. Ponte inicial com COBOL — rotinas continuam existindo

### Mostrar um fluxo conhecido

```cobol
PROCEDURE DIVISION.
    VALIDAR-CONTA.
    VALIDAR-VALOR.
    ATUALIZAR-SALDO.
    REGISTRAR-RESULTADO.
```

### Mostrar chamadas possíveis

```cobol
PERFORM VALIDAR-CONTA
PERFORM ATUALIZAR-SALDO
```

### Pontos para comentar

- COBOL já organiza instruções em parágrafos, seções e subprogramas.
- A ideia de nomear uma rotina não é nova.
- Java também agrupa instruções em unidades com nome: métodos.
- A diferença principal é a ligação do método com uma classe e, normalmente, com um objeto.

### Mostrar a mudança de leitura

```cobol
PERFORM REALIZAR-SAQUE
```

```java
conta.sacar(valor);
```

### Explicar

- `conta`: objeto que recebe a solicitação;
- `sacar`: comportamento solicitado;
- `valor`: informação necessária para a operação.

### Evitar equivalência simplista

- Método não é apenas parágrafo COBOL com chaves.
- Método possui:
  - classe à qual pertence;
  - modificador de acesso;
  - tipo de retorno;
  - nome;
  - parâmetros;
  - corpo.
- Um método de instância pode acessar diretamente o estado do objeto atual.

---

## 3. Método — comportamento com nome

### Criar o primeiro método em `Conta.java`

```java
package br.com.curso.banco.dominio;

public class Conta {

    public void exibirIdentificacao() {
        System.out.println("Conta bancária");
    }
}
```

### Explicar a assinatura

| Parte | Significado inicial |
|---|---|
| `public` | outras classes podem chamar o método |
| `void` | o método não devolve um valor |
| `exibirIdentificacao` | nome do comportamento |
| `()` | não há parâmetros |
| `{ }` | corpo com as instruções |

### Declarar não é executar

```java
Conta conta = new Conta();
conta.exibirIdentificacao();
```

### Pontos para comentar

- A classe contém a definição do método.
- O método só executa quando é chamado.
- O ponto seleciona um comportamento do objeto.
- Os parênteses fazem parte da chamada mesmo quando não há argumento.
- O nome do método normalmente começa com verbo e usa *camelCase*.

### Bons nomes para mostrar

```text
depositar
sacar
bloquear
estaAtiva
obterSaldo
```

### Nomes fracos para contrastar

```text
processar
executarCoisa
alterarFlag
metodo1
```

### Explicar o `main`

```java
public static void main(String[] args) {
}
```

- O `main` também é um método.
- Ele é o ponto de entrada usado pela JVM.
- `static` permite que seja iniciado sem um objeto de `AplicacaoBanco`.
- Não aprofundar métodos estáticos neste momento.
- Métodos como `sacar` serão chamados em objetos concretos:

```java
conta.sacar(valor);
```

---

## 4. Parâmetros — o que o método precisa receber

### Evoluir para um método com parâmetro

```java
public void depositar(BigDecimal valor) {
}
```

### Explicar

- `BigDecimal`: tipo do parâmetro;
- `valor`: nome do parâmetro;
- o parâmetro existe durante a execução daquela chamada;
- a assinatura comunica o contrato de entrada.

### Chamar o método

```java
conta.depositar(new BigDecimal("200.00"));
```

- `new BigDecimal("200.00")` cria o valor decimal a partir de texto.
- A escolha evita introduzir uma aproximação binária de `double`.
- Retomar esse cuidado na seção de comparação de `BigDecimal`.

### Parâmetro × argumento

```text
declaração: depositar(BigDecimal valor)
chamada:    depositar(new BigDecimal("200.00"))
```

- `valor` é parâmetro.
- `new BigDecimal("200.00")` é argumento.
- Parâmetros aparecem na declaração.
- Argumentos aparecem na chamada.

### Método com vários parâmetros

```java
public void cadastrar(
        String numero,
        String agencia,
        String titular,
        BigDecimal saldoInicial
) {
}
```

### Regras verificadas pelo compilador

- quantidade de argumentos;
- ordem dos argumentos;
- compatibilidade dos tipos.

### Ponte com COBOL

```cobol
CALL 'CADASTRAR-CONTA'
    USING WS-NUMERO
          WS-AGENCIA
          WS-TITULAR
          WS-SALDO-INICIAL
```

### Pontos para comentar

- `LINKAGE SECTION` e `USING` ajudam a reconhecer a intenção de receber dados.
- Em Java, cada método declara parâmetros diretamente na assinatura.
- Não afirmar que o mecanismo de memória é idêntico.
- Ordem e contrato continuam importantes.

### Introduzir `this`

```java
String numero;

public void cadastrar(String numero) {
    this.numero = numero;
}
```

### Explicar visualmente

```text
this.numero = numero;
atributo      parâmetro
do objeto     recebido
```

- `this` significa “este objeto”.
- O atributo e o parâmetro podem ter o mesmo nome.
- `this.numero` identifica o atributo.
- `numero` sozinho identifica o parâmetro mais próximo.

### Passagem por valor

- Java passa argumentos por valor.
- Tipo primitivo: o método recebe cópia do valor.
- Tipo por referência: o método recebe cópia da referência.
- O objeto inteiro não é copiado a cada chamada.
- Não aprofundar identidade e mutabilidade além do necessário.

---

## 5. Retornos — o método responde alguma coisa

### Método sem retorno

```java
public void bloquear() {
    ativa = false;
}
```

### Explicar

- `void` significa ausência de valor devolvido.
- O método executa uma ação.
- Isso não significa que ele não possa alterar o estado do objeto.

### Método com retorno

```java
public boolean estaAtiva() {
    return ativa;
}
```

### Chamada armazenando o resultado

```java
boolean contaAtiva = conta.estaAtiva();
```

### Método que devolve saldo

```java
public BigDecimal obterSaldo() {
    return saldo;
}
```

### Pontos para comentar

- O tipo antes do nome declara o tipo devolvido.
- `return` entrega o resultado a quem chamou.
- O valor precisa ser compatível com o tipo declarado.
- `return` também encerra a execução do método.

### Retornar não é imprimir

```java
return saldo;
```

```java
System.out.println(saldo);
```

- `return`: entrega um valor para outra parte do programa.
- `println`: escreve no terminal.
- Um retorno pode ser usado por tela, relatório, teste ou outra regra.
- Um método que apenas imprime fica acoplado à saída textual.

### Ação, consulta e cálculo

| Intenção | Exemplo | Retorno possível |
|---|---|---|
| executar ação | `bloquear()` | `void` |
| informar sucesso | `sacar(valor)` | `boolean` |
| consultar estado | `estaAtiva()` | `boolean` |
| obter dado | `obterSaldo()` | `BigDecimal` |
| calcular resultado | `calcularTarifa()` | `BigDecimal` |

### Ponte com COBOL

- Subprogramas COBOL podem usar `RETURNING` ou áreas definidas no contrato.
- Métodos Java declaram o tipo retornado na própria assinatura.
- A ponte é a necessidade de comunicar um resultado.
- A implementação e as regras de tipos são diferentes.

---

## 6. Encapsulamento — saldo não é uma área aberta

### Mostrar o problema

```java
conta.saldo = new BigDecimal("999999999.99");
conta.ativa = true;
```

### Perguntas para fazer

- “Qual operação bancária aconteceu?”
- “Houve depósito, estorno ou ajuste?”
- “Quem validou o valor?”
- “Qualquer classe deveria poder reativar a conta?”

### Tornar os atributos privados

```java
public class Conta {
    private String numero;
    private String agencia;
    private String titular;
    private boolean ativa;
    private BigDecimal saldo;
}
```

### Explicar

- `private`: acesso direto restrito à própria classe.
- `public`: operação oferecida às outras classes.
- A aplicação deixa de manipular o estado arbitrariamente.
- A classe passa a controlar como seu estado muda.

### Mostrar uma porta de entrada controlada

```java
public void bloquear() {
    ativa = false;
}
```

### Não resumir encapsulamento a getters e setters

Exemplo fraco:

```java
public void setSaldo(BigDecimal saldo) {
    this.saldo = saldo;
}
```

Exemplo com intenção de negócio:

```java
public boolean depositar(BigDecimal valor) {
    // validar e realizar depósito
}
```

### Pontos para comentar

- Um setter genérico muda o valor, mas não informa a operação.
- `depositar`, `sacar` e `bloquear` comunicam intenção.
- Getters podem ser necessários, mas não devem ser gerados automaticamente para tudo.
- Nem todo atributo precisa ser exposto.
- Encapsulamento reúne:
  - estado protegido;
  - operações públicas coerentes;
  - regras mantidas na classe responsável.

### Ponte com COBOL

- Sistemas COBOL também podem proteger regras em programas e subprogramas.
- Interfaces de chamada podem limitar os dados expostos.
- Em Java, o modificador de acesso integra essa fronteira à classe.
- O compilador impede outra classe de acessar diretamente um atributo `private`.

---

## 7. Condições — a conta decide se a operação pode acontecer

### Comparar `IF`

```cobol
IF CONTA-ATIVA
    PERFORM REALIZAR-DEPOSITO
ELSE
    MOVE 'N' TO WS-OPERACAO-REALIZADA
END-IF
```

```java
if (ativa) {
    saldo = saldo.add(valor);
} else {
    System.out.println("Conta inativa");
}
```

- `saldo.add(valor)` produz a soma como outro `BigDecimal`.
- A operação será retomada na construção completa de `Conta`.

### Explicar a estrutura

- `if`: verifica uma condição.
- condição entre parênteses precisa resultar em `boolean`.
- chaves delimitam o bloco.
- `else`: executado quando a condição do `if` é falsa.

### Mostrar `else if`

```java
if (!ativa) {
    System.out.println("Conta inativa");
} else if (quantidadeSaques >= 3) {
    System.out.println("Limite de saques alcançado");
} else {
    System.out.println("Saque permitido");
}
```

### Explicar a ordem

- Condições são testadas de cima para baixo.
- A primeira condição verdadeira seleciona o bloco.
- Os blocos seguintes da mesma cadeia são ignorados.

### Operadores para mostrar

| Operador | Significado |
|---|---|
| `==` | igual |
| `!=` | diferente |
| `>` | maior |
| `<` | menor |
| `>=` | maior ou igual |
| `<=` | menor ou igual |
| `&&` | e |
| `||` | ou |
| `!` | negação |

### Booleano diretamente como condição

Preferir:

```java
if (ativa) {
}
```

e:

```java
if (!ativa) {
}
```

Evitar como padrão:

```java
if (ativa == true) {
}
```

### Retorno antecipado

```java
public boolean depositar(BigDecimal valor) {
    if (!ativa) {
        return false;
    }

    saldo = saldo.add(valor);
    return true;
}
```

### Pontos para comentar

- `return false` encerra o método.
- A atualização não ocorre quando a conta está inativa.
- A regra fica dentro de `Conta`, que conhece `ativa` e `saldo`.
- Quem chamou recebe apenas o resultado da tentativa.

---

## 8. Comparações que exigem cuidado

### 8.1 Comparar `String`

```java
if ("SAQUE".equals(operacao)) {
    System.out.println("Operação de saque");
}
```

### Não usar para conteúdo textual

```java
if (operacao == "SAQUE") {
}
```

### Explicar

- Para tipos primitivos, `==` compara valores.
- Para referências, `==` verifica se apontam para o mesmo objeto.
- `equals` é usado para comparar conteúdo de `String`.
- Escrever a constante antes de `.equals` evita erro se `operacao` for `null`.

### 8.2 Comparar `BigDecimal`

```java
valor.compareTo(BigDecimal.ZERO)
```

### Explicar o resultado

| Resultado | Significado |
|---:|---|
| menor que `0` | `valor` é menor que zero |
| igual a `0` | valores numericamente iguais |
| maior que `0` | `valor` é maior que zero |

### Exemplos

```java
valor.compareTo(BigDecimal.ZERO) <= 0
```

- valor é zero ou negativo.

```java
saldo.compareTo(valor) < 0
```

- saldo é menor que o valor solicitado.

### Observação importante

- Para comparar valor monetário, `compareTo` costuma ser mais apropriado.
- `BigDecimal.equals` também considera a escala.
- `10.0` e `10.00` podem ser numericamente iguais em `compareTo`, mas diferentes em `equals`.
- Criar valores monetários a partir de texto evita carregar aproximações de `double`:

```java
new BigDecimal("1000.00")
```

---

## 9. `switch` e a ponte com `EVALUATE`

### Mostrar COBOL

```cobol
EVALUATE WS-OPERACAO
    WHEN 'D'
        PERFORM DEPOSITAR
    WHEN 'S'
        PERFORM SACAR
    WHEN 'B'
        PERFORM BLOQUEAR-CONTA
    WHEN OTHER
        DISPLAY 'OPERACAO INVALIDA'
END-EVALUATE
```

### Mostrar Java

```java
switch (operacao) {
    case "D":
        System.out.println("Depósito selecionado");
        break;
    case "S":
        System.out.println("Saque selecionado");
        break;
    case "B":
        System.out.println("Bloqueio selecionado");
        break;
    default:
        System.out.println("Operação inválida");
}
```

### Pontos para comentar

- `switch` compara o mesmo valor com alternativas conhecidas.
- `case` representa uma alternativa.
- `default` recebe os casos não reconhecidos.
- Nesta forma, `break` encerra o caso atual.
- Sem `break`, a execução pode continuar no caso seguinte.
- Não aprofundar outras formas modernas de `switch` neste primeiro contato.

### Quando usar cada um

- `if`: intervalos e expressões diferentes.
- `switch`: alternativas discretas do mesmo valor.

Exemplos adequados para `if`:

```text
saldo menor que valor
conta ativa e valor positivo
quantidade de saques maior que limite
```

Exemplos adequados para `switch`:

```text
tipo de operação: D, S ou B
tipo de conta: C, P ou S
```

---

## 10. Repetições — procedimentos continuam se repetindo

### Mensagem de transição

- “Orientação a objetos não substitui estruturas de repetição.”
- “A repetição fica dentro do método responsável por coordenar aquele fluxo.”

### 10.1 `while`

```java
int tentativa = 1;

while (tentativa <= 3) {
    System.out.println("Tentativa de operação: " + tentativa);
    tentativa++;
}
```

### Explicar

- testa a condição antes do bloco;
- pode executar zero vezes;
- continua enquanto a condição for verdadeira;
- `tentativa++` aumenta uma unidade.

### Comparar com COBOL

```cobol
MOVE 1 TO WS-TENTATIVA

PERFORM UNTIL WS-TENTATIVA > 3
    DISPLAY 'TENTATIVA: ' WS-TENTATIVA
    ADD 1 TO WS-TENTATIVA
END-PERFORM
```

### Destacar a inversão de leitura

- `while (tentativa <= 3)`: condição para continuar.
- `PERFORM UNTIL tentativa > 3`: condição para encerrar.
- A intenção pode ser equivalente, mas a condição aparece invertida.

### 10.2 `do-while`

```java
int tentativa = 1;

do {
    System.out.println("Tentativa de operação: " + tentativa);
    tentativa++;
} while (tentativa <= 3);
```

### Explicar

- executa o bloco antes de testar;
- sempre executa ao menos uma vez;
- termina com ponto e vírgula depois da condição;
- aproxima-se da ideia de `PERFORM ... WITH TEST AFTER`.

### 10.3 `for`

```java
for (int tentativa = 1; tentativa <= 3; tentativa++) {
    System.out.println("Tentativa de saque: " + tentativa);
}
```

### Separar o cabeçalho

```text
int tentativa = 1  → inicialização
tentativa <= 3     → condição de continuidade
tentativa++        → atualização
```

### Comparar com COBOL

```cobol
PERFORM VARYING WS-TENTATIVA FROM 1 BY 1
        UNTIL WS-TENTATIVA > 3
    DISPLAY 'TENTATIVA DE SAQUE: ' WS-TENTATIVA
END-PERFORM
```

### Quadro de escolha

| Estrutura | Leitura principal |
|---|---|
| `while` | repetir enquanto a condição continuar verdadeira |
| `do-while` | executar pelo menos uma vez e depois testar |
| `for` | repetir com controle concentrado |

### Alertar sobre laço infinito

```java
int tentativa = 1;

while (tentativa <= 3) {
    System.out.println("Tentativa");
}
```

- `tentativa` nunca muda.
- A condição nunca se torna falsa.
- Sempre identificar:
  - condição de continuidade;
  - variável modificada;
  - situação de encerramento.

### Limite deste tópico

- Ainda não percorrer lista de contas.
- Ainda não introduzir `List`, arrays ou `for-each`.
- Usar repetição controlada por contador.
- Coleções terão um momento próprio.

---

## 11. Demonstração progressiva — construir `Conta.java`

### Etapa 1 — manter pacote e import

```java
package br.com.curso.banco.dominio;

import java.math.BigDecimal;
```

### Etapa 2 — tornar o estado privado

```java
public class Conta {

    private String numero;
    private String agencia;
    private String titular;
    private boolean ativa;
    private BigDecimal saldo;
    private int quantidadeSaques;
}
```

### Pausar e perguntar

- “Qual classe conhece o saldo?”
- “Qual classe deve decidir se o saldo é suficiente?”
- “`AplicacaoBanco` deveria modificar `saldo` diretamente?”

### Etapa 3 — cadastrar dados iniciais

```java
public void cadastrar(
        String numero,
        String agencia,
        String titular,
        BigDecimal saldoInicial
) {
    this.numero = numero;
    this.agencia = agencia;
    this.titular = titular;
    this.saldo = saldoInicial;
    this.ativa = true;
    this.quantidadeSaques = 0;
}
```

### Explicar

- Método usado didaticamente no lugar de construtor personalizado.
- Parâmetros fornecem os dados iniciais.
- `this` distingue atributos e parâmetros.
- Conta inicia ativa no exemplo.
- Quantidade de saques começa em zero.
- Validações completas de cadastro não são o foco.

### Etapa 4 — depositar

```java
public boolean depositar(BigDecimal valor) {
    if (!ativa || valor.compareTo(BigDecimal.ZERO) <= 0) {
        return false;
    }

    saldo = saldo.add(valor);
    return true;
}
```

### Ler a condição

```text
conta não está ativa
OU
valor é menor ou igual a zero
```

### Pontos para comentar

- Se qualquer impedimento for verdadeiro, retorna `false`.
- `return` antecipado evita executar a atualização.
- `BigDecimal` é imutável: `add` produz outro valor.
- O novo resultado é atribuído novamente a `saldo`.

### Etapa 5 — sacar

```java
public boolean sacar(BigDecimal valor) {
    if (!ativa) {
        return false;
    }

    if (valor.compareTo(BigDecimal.ZERO) <= 0) {
        return false;
    }

    if (saldo.compareTo(valor) < 0) {
        return false;
    }

    saldo = saldo.subtract(valor);
    quantidadeSaques++;
    return true;
}
```

### Explicar a sequência

1. Recusar conta inativa.
2. Recusar valor zero ou negativo.
3. Recusar saldo insuficiente.
4. Subtrair o valor.
5. Incrementar a quantidade de saques.
6. Retornar sucesso.

### Paralelo procedural

- O método ainda possui fluxo sequencial.
- Cada `if` protege uma pré-condição.
- O fluxo está dentro de `Conta` porque depende do estado da conta.
- Outra classe não precisa conhecer como saldo e status são validados.

### Etapa 6 — bloquear e consultar

```java
public void bloquear() {
    ativa = false;
}

public boolean estaAtiva() {
    return ativa;
}

public BigDecimal obterSaldo() {
    return saldo;
}

public String obterNumero() {
    return numero;
}

public int obterQuantidadeSaques() {
    return quantidadeSaques;
}
```

### Código consolidado de `Conta.java`

```java
package br.com.curso.banco.dominio;

import java.math.BigDecimal;

public class Conta {

    private String numero;
    private String agencia;
    private String titular;
    private boolean ativa;
    private BigDecimal saldo;
    private int quantidadeSaques;

    public void cadastrar(
            String numero,
            String agencia,
            String titular,
            BigDecimal saldoInicial
    ) {
        this.numero = numero;
        this.agencia = agencia;
        this.titular = titular;
        this.saldo = saldoInicial;
        this.ativa = true;
        this.quantidadeSaques = 0;
    }

    public boolean depositar(BigDecimal valor) {
        if (!ativa || valor.compareTo(BigDecimal.ZERO) <= 0) {
            return false;
        }

        saldo = saldo.add(valor);
        return true;
    }

    public boolean sacar(BigDecimal valor) {
        if (!ativa) {
            return false;
        }

        if (valor.compareTo(BigDecimal.ZERO) <= 0) {
            return false;
        }

        if (saldo.compareTo(valor) < 0) {
            return false;
        }

        saldo = saldo.subtract(valor);
        quantidadeSaques++;
        return true;
    }

    public void bloquear() {
        ativa = false;
    }

    public boolean estaAtiva() {
        return ativa;
    }

    public BigDecimal obterSaldo() {
        return saldo;
    }

    public String obterNumero() {
        return numero;
    }

    public int obterQuantidadeSaques() {
        return quantidadeSaques;
    }
}
```

---

## 12. Criar `Banco.java` como coordenador

### Criar no mesmo pacote de domínio

Arquivo:

```text
src\br\com\curso\banco\dominio\Banco.java
```

### Código

```java
package br.com.curso.banco.dominio;

import java.math.BigDecimal;

public class Banco {

    public void tentarSaques(
            Conta conta,
            BigDecimal valor,
            int quantidadeTentativas
    ) {
        for (int tentativa = 1; tentativa <= quantidadeTentativas; tentativa++) {
            boolean realizado = conta.sacar(valor);

            if (realizado) {
                System.out.println(
                        "Saque " + tentativa
                                + " aprovado. Saldo: "
                                + conta.obterSaldo()
                );
            } else {
                System.out.println(
                        "Saque " + tentativa
                                + " recusado. Saldo: "
                                + conta.obterSaldo()
                );
            }
        }
    }
}
```

### Pontos para comentar

- `Banco` coordena várias tentativas.
- `Conta` continua decidindo se cada saque é permitido.
- `Banco` não acessa `saldo` diretamente.
- `Conta` devolve `boolean` e permite consultar o saldo.
- O `for` controla a quantidade de tentativas.
- O `if` interpreta o resultado de cada chamada.

### Pergunta para a turma

- “Por que a condição de saldo não foi colocada dentro de `Banco`?”

### Resposta esperada

- Porque `Conta` conhece e protege o próprio saldo.
- Centralizar a regra em `Conta` evita duplicação.
- Qualquer fluxo que solicitar saque receberá a mesma validação.

---

## 13. Atualizar `AplicacaoBanco.java`

### Código completo

```java
package br.com.curso.banco;

import java.math.BigDecimal;

import br.com.curso.banco.dominio.Banco;
import br.com.curso.banco.dominio.Conta;

public class AplicacaoBanco {

    public static void main(String[] args) {
        Conta conta = new Conta();
        conta.cadastrar(
                "000123-4",
                "1234",
                "Ana Souza",
                new BigDecimal("1000.00")
        );

        System.out.println(
                "Conta " + conta.obterNumero()
                        + " cadastrada. Saldo: "
                        + conta.obterSaldo()
        );

        boolean depositoRealizado =
                conta.depositar(new BigDecimal("200.00"));

        if (depositoRealizado) {
            System.out.println(
                    "Depósito realizado. Saldo: " + conta.obterSaldo()
            );
        } else {
            System.out.println("Depósito recusado");
        }

        Banco banco = new Banco();
        banco.tentarSaques(
                conta,
                new BigDecimal("350.00"),
                4
        );

        System.out.println(
                "Saques realizados: " + conta.obterQuantidadeSaques()
        );
    }
}
```

### Ler o fluxo em voz alta

1. Criar a conta.
2. Cadastrar dados e saldo inicial.
3. Solicitar depósito de duzentos reais.
4. Verificar o retorno do depósito.
5. Criar o coordenador `Banco`.
6. Solicitar quatro tentativas de saque de trezentos e cinquenta reais.
7. Exibir a quantidade de saques aprovados.

### Resultado esperado

```text
Conta 000123-4 cadastrada. Saldo: 1000.00
Depósito realizado. Saldo: 1200.00
Saque 1 aprovado. Saldo: 850.00
Saque 2 aprovado. Saldo: 500.00
Saque 3 aprovado. Saldo: 150.00
Saque 4 recusado. Saldo: 150.00
Saques realizados: 3
```

### Pontos para pausar

- O quarto saque não precisa de tratamento especial na aplicação.
- A mesma chamada é realizada quatro vezes.
- O estado atualizado influencia a próxima execução.
- `Conta` preserva a regra de saldo suficiente.
- `Banco` preserva a responsabilidade de coordenar tentativas.
- `AplicacaoBanco` apenas monta e inicia o cenário.

---

## 14. Compilar e executar no terminal

### Compilar

```bat
javac -d out src\br\com\curso\banco\dominio\Conta.java src\br\com\curso\banco\dominio\Banco.java src\br\com\curso\banco\AplicacaoBanco.java
```

### Executar

```bat
java -cp out br.com.curso.banco.AplicacaoBanco
```

### Se ocorrer erro, verificar

- pacote declarado em cada arquivo;
- posição dos arquivos nas pastas;
- imports de `BigDecimal`, `Conta` e `Banco`;
- nome da classe pública igual ao nome do arquivo;
- chaves abertas e fechadas;
- ponto e vírgula;
- tipos e quantidade dos argumentos;
- compilação de todos os arquivos usados.

### Mostrar também pelo VS Code

- Executar `AplicacaoBanco` pelo botão **Run**.
- Comparar com a execução manual.
- Reforçar que o VS Code automatiza o processo, mas o JDK continua compilando e executando.

---

## 15. Quadro de síntese COBOL → Java

| Experiência conhecida | Ponte em Java | Limite da comparação |
|---|---|---|
| parágrafo ou seção | método | método pertence a uma classe |
| `PERFORM` | chamada de método | chamada pode ser dirigida a um objeto |
| `CALL ... USING` | argumentos | assinatura Java declara tipos e retorno |
| `RETURNING`/área de resposta | `return` | retorno integra a assinatura do método |
| fronteira de programas | encapsulamento | classe usa modificadores de acesso |
| `IF`/`ELSE` | `if`/`else` | condição Java resulta em `boolean` |
| `EVALUATE` | `switch` | regras próprias de casos e `break` |
| `PERFORM UNTIL` | `while` | `while` declara continuidade; `UNTIL`, término |
| `PERFORM ... TEST AFTER` | `do-while` | Java termina a estrutura com `;` |
| `PERFORM VARYING` | `for` | controle aparece no cabeçalho |
| `ADD 1 TO` | `++` | `++` incrementa exatamente uma unidade |

### Pergunta de verificação

- “Qual dessas linhas é uma tradução perfeita?”
- Resposta esperada: nenhuma.
- São pontes para compreender intenção e fluxo.

---

## 16. Erros conceituais para antecipar

### “Método é só uma função solta dentro do arquivo”

- Método é declarado dentro de uma classe.
- Método de instância opera no contexto de um objeto.

### “Parâmetro e argumento são a mesma posição no código”

- Parâmetro: declaração.
- Argumento: chamada.

### “`void` significa que o método não faz nada”

- Significa apenas que não devolve valor.
- Pode alterar estado ou executar outras ações.

### “`return` imprime o valor”

- Não imprime.
- Devolve o valor à chamada.

### “Colocar `private` já resolve o encapsulamento”

- `private` impede acesso direto.
- Métodos ainda precisam expressar operações coerentes.

### “Todo atributo privado precisa de getter e setter”

- Não.
- Expor alteração irrestrita pode destruir a regra do objeto.
- Preferir comportamentos do domínio.

### “`==` compara textos”

- Para `String`, usar `equals` para comparar conteúdo.

### “Dinheiro pode ser comparado com `<` normalmente”

- `BigDecimal` é objeto.
- Usar `compareTo` para comparação numérica.

### “OO substitui condição e repetição”

- Não substitui.
- Métodos continuam usando fluxo procedural.

### “A regra de saque deve ficar em `Banco` porque banco coordena tudo”

- `Banco` coordena o fluxo.
- `Conta` protege o saldo e sua situação.
- Coordenação não significa apropriação de todas as regras.

---

## 17. Encerramento

### Recapitular

- Objeto reúne estado e comportamento.
- Método representa comportamento dentro de uma classe.
- Parâmetro declara informação recebida.
- Argumento fornece o valor na chamada.
- Retorno comunica um resultado.
- `void` indica ausência de valor devolvido.
- `private` protege o estado.
- Métodos públicos oferecem operações controladas.
- Encapsulamento não é sinônimo de getters e setters.
- `if` representa decisões.
- `switch` seleciona alternativas conhecidas.
- `while`, `do-while` e `for` repetem instruções.
- `Conta` protege saldo e status.
- `Banco` coordena o fluxo.
- `AplicacaoBanco` inicia o cenário.

### Mensagens finais sugeridas

- “A experiência procedural continua dentro de cada método.”
- “A novidade é colocar a regra perto do estado que ela protege.”
- “Sacar não é o mesmo que atribuir um novo saldo.”
- “Encapsulamento troca acesso irrestrito por operações com significado.”
- “O objeto começa a ser realmente útil quando ele deixa de ser apenas uma estrutura de dados.”

### Preparar a transição para a prática

- Na atividade, o domínio será um Gestor de Tarefas.
- A turma deverá transferir a ideia, não copiar nomes bancários.
- `Conta.sacar()` poderá inspirar um comportamento como `Tarefa.concluir()`.
- `Conta.estaAtiva()` poderá inspirar `Tarefa.estaConcluida()`.
- As regras precisarão permanecer dentro da classe responsável.

---

## Checklist de gravação

- [ ] Retomar as classes do Tópico 2.
- [ ] Mostrar a diferença entre estado e comportamento.
- [ ] Comparar método com rotinas COBOL sem declarar equivalência.
- [ ] Explicar a assinatura completa de um método.
- [ ] Diferenciar declaração e chamada.
- [ ] Explicar o papel especial do `main`.
- [ ] Diferenciar parâmetro e argumento.
- [ ] Mostrar método com vários parâmetros.
- [ ] Explicar `this`.
- [ ] Mencionar passagem por valor.
- [ ] Diferenciar `void` e método com retorno.
- [ ] Diferenciar `return` e `println`.
- [ ] Tornar os atributos de `Conta` privados.
- [ ] Explicar por que setter genérico não representa regra bancária.
- [ ] Comparar `IF`/`ELSE` com `if`/`else`.
- [ ] Apresentar operadores de comparação e operadores lógicos.
- [ ] Mostrar retorno antecipado.
- [ ] Explicar comparação de `String` com `equals`.
- [ ] Explicar comparação de `BigDecimal` com `compareTo`.
- [ ] Comparar `EVALUATE` e `switch`.
- [ ] Comparar `PERFORM UNTIL` e `while`.
- [ ] Apresentar `do-while` e teste posterior.
- [ ] Comparar `PERFORM VARYING` e `for`.
- [ ] Alertar sobre repetição infinita.
- [ ] Construir `Conta.java` progressivamente.
- [ ] Criar `Banco.java` como coordenador.
- [ ] Atualizar `AplicacaoBanco.java`.
- [ ] Executar depósito e quatro tentativas de saque.
- [ ] Mostrar o quarto saque recusado.
- [ ] Compilar pelo terminal.
- [ ] Executar pelo terminal e pelo VS Code.
- [ ] Encerrar antes de coleções, construtores e exceções.
