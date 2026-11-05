# Exercícios — Tópico 3: Dar comportamento aos objetos

## Continuação do projeto prático: Gestor de Tarefas

Nos exercícios anteriores, o **Gestor de Tarefas** deixou de ser apenas uma classe com mensagens fixas. O projeto passou a possuir classes que representam conceitos do domínio: `Tarefa` e `Responsavel`. Também foram criados atributos, objetos, pacotes e imports.

Agora essas classes deixarão de apenas descrever dados. Elas passarão a executar comportamentos relacionados às próprias responsabilidades.

Ao longo desta sequência, você irá:

- criar métodos;
- enviar informações por parâmetros;
- receber resultados por retornos;
- usar `this` para distinguir atributos e parâmetros;
- proteger atributos com `private`;
- permitir operações por meio de métodos `public`;
- usar condições para aplicar regras;
- comparar `String` com `equals`;
- comparar `BigDecimal` com `compareTo`;
- usar `switch`, `for`, `while` e `do-while`;
- relacionar essas estruturas com recursos já conhecidos do COBOL.

O projeto continuará intencionalmente limitado ao conteúdo estudado. Nesta etapa, não serão usados:

- construtores personalizados;
- entrada de dados pelo teclado;
- arrays ou coleções;
- herança ou interfaces;
- exceções;
- banco de dados;
- frameworks.

> Continue trabalhando no **computador físico**, não na VDI. Se algum resultado for diferente do indicado, procure o instrutor e mostre o código e a mensagem completa exibida no terminal.

---

## Ponto de partida

Use a pasta desenvolvida nos Tópicos 1 e 2:

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

No encerramento do Tópico 2, as classes de domínio possuíam atributos sem modificador de acesso. Os objetos eram criados em `AplicacaoGestorTarefas`, mas seus dados ainda não eram preenchidos. As mensagens exibidas no terminal eram fixas.

Nesta sequência, a aplicação começará a trabalhar com os valores realmente armazenados nos objetos.

## Resultado final esperado

Ao final, o programa deverá:

1. cadastrar um responsável;
2. cadastrar três tarefas;
3. consultar dados por meio de métodos;
4. impedir o início de uma tarefa quando o responsável estiver inativo;
5. iniciar uma tarefa quando as condições forem atendidas;
6. registrar horas trabalhadas;
7. concluir automaticamente uma tarefa que atingir a estimativa;
8. comparar código e custo sem acessar diretamente os atributos;
9. executar repetições controladas;
10. exibir a situação final das tarefas.

---

## Exercício 29 — Confirmar a versão do Tópico 2

Antes de fazer alterações, compile todos os arquivos atuais:

```bat
javac -d out src\br\com\curso\tarefas\dominio\*.java src\br\com\curso\tarefas\AplicacaoGestorTarefas.java
```

Execute:

```bat
java -cp out br.com.curso.tarefas.AplicacaoGestorTarefas
```

Confirme que a versão anterior ainda funciona. Esse teste estabelece um ponto de partida conhecido.

### Conferência

- [ ] As três classes compilam.
- [ ] Os arquivos `.class` são gerados em `out`.
- [ ] A aplicação executa.
- [ ] A saída do Tópico 2 aparece no terminal.

---

## Exercício 30 — Encapsular os atributos de `Responsavel`

Abra `Responsavel.java` e acrescente `private` aos atributos:

```java
package br.com.curso.tarefas.dominio;

public class Responsavel {

    private String matricula;
    private String nome;
    private boolean ativo;
}
```

Com `private`, os atributos somente podem ser acessados diretamente dentro da própria classe `Responsavel`.

Isso não significa que os dados ficaram inutilizáveis. Significa que a classe passou a controlar como seu estado será alterado e consultado.

### Ponte com COBOL

Em COBOL, uma área da `WORKING-STORAGE SECTION` pode ser usada por diferentes parágrafos do mesmo programa. Em Java, o modificador `private` estabelece uma fronteira: outras classes não alteram o atributo diretamente; elas solicitam uma operação ao objeto.

Compile o projeto. A compilação deverá continuar funcionando porque a aplicação ainda não tenta acessar esses atributos.

---

## Exercício 31 — Cadastrar e consultar um responsável

Acrescente os métodos abaixo em `Responsavel`, depois dos atributos:

```java
public void cadastrar(String matricula, String nome) {
    this.matricula = matricula;
    this.nome = nome;
    this.ativo = true;
}

public String obterMatricula() {
    return matricula;
}

public String obterNome() {
    return nome;
}

public boolean estaAtivo() {
    return ativo;
}

public void ativar() {
    ativo = true;
}

public void desativar() {
    ativo = false;
}
```

### Interprete os métodos

| Método | Parâmetros | Retorno | Responsabilidade |
|---|---|---|---|
| `cadastrar` | matrícula e nome | `void` | preencher os dados iniciais e ativar o responsável |
| `obterMatricula` | nenhum | `String` | devolver a matrícula |
| `obterNome` | nenhum | `String` | devolver o nome |
| `estaAtivo` | nenhum | `boolean` | informar se o responsável está ativo |
| `ativar` | nenhum | `void` | alterar a situação para ativa |
| `desativar` | nenhum | `void` | alterar a situação para inativa |

No método `cadastrar`, compare:

```java
this.nome = nome;
```

- `this.nome` é o atributo do objeto atual;
- `nome` é o parâmetro recebido pelo método.

Em COBOL, uma rotina poderia receber informações por `USING`. Em Java, os parâmetros fazem parte da assinatura do método. A intenção é semelhante — fornecer dados a uma operação —, mas o método pertence à classe e atua sobre o objeto.

---

## Exercício 32 — Usar os métodos de `Responsavel`

Em `AplicacaoGestorTarefas.java`, mantenha a criação do responsável e chame o método `cadastrar`:

```java
Responsavel responsavel = new Responsavel();
responsavel.cadastrar("F12345", "Marina Souza");
```

Depois, exiba informações usando os métodos de consulta:

```java
System.out.println("Responsável: " + responsavel.obterNome());
System.out.println("Matrícula: " + responsavel.obterMatricula());
System.out.println("Ativo: " + responsavel.estaAtivo());
```

Observe a diferença entre declarar e chamar:

```java
public String obterNome()       // declaração na classe
responsavel.obterNome()         // chamada na aplicação
```

O `return` entrega o valor para quem chamou o método. Ele não exibe nada sozinho. A exibição continua sendo feita por `System.out.println`.

---

## Exercício 33 — Encapsular os atributos de `Tarefa`

Abra `Tarefa.java` e transforme todos os atributos em `private`. Acrescente também `emAndamento` e `horasRealizadas`:

```java
private String codigo;
private String titulo;
private String descricao;
private int estimativaHoras;
private int horasRealizadas;
private boolean emAndamento;
private boolean concluida;
private LocalDate dataLimite;
private BigDecimal custoEstimado;
private Responsavel responsavel;
```

Os dois novos atributos representam o avanço da tarefa:

| Atributo | Significado |
|---|---|
| `horasRealizadas` | quantidade de horas já registradas |
| `emAndamento` | indica se a tarefa foi iniciada |

Não crie métodos como `setConcluida` ou `setHorasRealizadas`. Esses nomes permitiriam alterações genéricas. Nos próximos exercícios, criaremos operações que expressem intenções do domínio: `iniciar`, `registrarHoraTrabalhada` e `concluir`.

---

## Exercício 34 — Criar o método `cadastrar` de `Tarefa`

Acrescente este método em `Tarefa`:

```java
public void cadastrar(
        String codigo,
        String titulo,
        String descricao,
        int estimativaHoras,
        LocalDate dataLimite,
        BigDecimal custoEstimado,
        Responsavel responsavel) {

    this.codigo = codigo;
    this.titulo = titulo;
    this.descricao = descricao;
    this.estimativaHoras = estimativaHoras;
    this.dataLimite = dataLimite;
    this.custoEstimado = custoEstimado;
    this.responsavel = responsavel;
    this.horasRealizadas = 0;
    this.emAndamento = false;
    this.concluida = false;
}
```

O método possui vários parâmetros porque recebe as informações necessárias para cadastrar uma tarefa. Ainda não usamos um construtor personalizado; o objeto continua sendo criado com `new Tarefa()` e configurado por uma chamada separada.

### Leia a chamada antes de escrevê-la

Uma chamada deverá fornecer argumentos na mesma ordem e com tipos compatíveis:

```java
tarefa.cadastrar(
        "TAR-001",
        "Revisar requisitos",
        "Conferir os requisitos do projeto",
        3,
        LocalDate.of(2026, 10, 10),
        new BigDecimal("150.00"),
        responsavel);
```

O método declara **parâmetros**; a chamada fornece **argumentos**.

---

## Exercício 35 — Criar métodos de consulta em `Tarefa`

Acrescente:

```java
public String obterCodigo() {
    return codigo;
}

public String obterTitulo() {
    return titulo;
}

public int obterEstimativaHoras() {
    return estimativaHoras;
}

public int obterHorasRealizadas() {
    return horasRealizadas;
}

public boolean estaEmAndamento() {
    return emAndamento;
}

public boolean estaConcluida() {
    return concluida;
}

public String obterNomeResponsavel() {
    return responsavel.obterNome();
}
```

Esses métodos não alteram a tarefa. Eles consultam o estado atual e devolvem uma informação.

Não é necessário expor todos os atributos. `descricao`, `dataLimite` e `custoEstimado`, por exemplo, somente precisarão de métodos de consulta quando a aplicação realmente tiver uma razão para usar esses valores.

---

## Exercício 36 — Aplicar condições ao iniciar uma tarefa

Uma tarefa somente poderá ser iniciada quando:

- ainda não estiver concluída;
- ainda não estiver em andamento;
- o responsável estiver ativo.

Crie:

```java
public boolean iniciar() {
    if (concluida || emAndamento || !responsavel.estaAtivo()) {
        return false;
    }

    emAndamento = true;
    return true;
}
```

O método usa retorno antecipado. Quando alguma regra impede a operação, `return false` encerra o método. A alteração somente ocorre depois da validação.

### Relação com COBOL

Uma regra parecida poderia usar `IF`, `OR` e uma condição de nível 88. Em Java:

- `||` representa “ou”;
- `!` inverte um valor boolean;
- `return false` informa que a operação não foi realizada.

O ponto principal não é trocar palavras-chave. A regra de início foi colocada dentro de `Tarefa`, junto do estado que ela protege.

---

## Exercício 37 — Registrar horas e concluir automaticamente

Crie este método:

```java
public boolean registrarHoraTrabalhada() {
    if (!emAndamento || concluida) {
        return false;
    }

    horasRealizadas++;

    if (horasRealizadas >= estimativaHoras) {
        concluida = true;
        emAndamento = false;
    }

    return true;
}
```

O primeiro `if` protege a operação. Não é possível registrar trabalho em uma tarefa que não foi iniciada ou que já foi concluída.

Depois do incremento, o segundo `if` verifica se a quantidade realizada alcançou a estimativa. Quando isso acontece, a tarefa é concluída.

### Interprete `horasRealizadas++`

```java
horasRealizadas++;
```

equivale, neste caso, a:

```java
horasRealizadas = horasRealizadas + 1;
```

Em COBOL, uma intenção semelhante poderia ser expressa com:

```cobol
ADD 1 TO WS-HORAS-REALIZADAS
```

---

## Exercício 38 — Criar uma conclusão explícita

Além da conclusão automática, crie uma operação para concluir antecipadamente uma tarefa em andamento:

```java
public boolean concluir() {
    if (!emAndamento || concluida) {
        return false;
    }

    concluida = true;
    emAndamento = false;
    return true;
}
```

O retorno permite que a aplicação saiba se a solicitação foi aceita.

Compare:

```java
boolean resultado = tarefa.concluir();
```

com uma chamada `PERFORM` ou `CALL` que também precisa comunicar o resultado de uma rotina. Em Java, o tipo `boolean` já torna explícito que a resposta somente pode ser `true` ou `false`.

---

## Exercício 39 — Comparar códigos e valores corretamente

Acrescente dois métodos:

```java
public boolean possuiCodigo(String codigoInformado) {
    return codigo.equals(codigoInformado);
}

public boolean possuiCustoAcimaDe(BigDecimal limite) {
    return custoEstimado.compareTo(limite) > 0;
}
```

### Comparação de `String`

Não escreva:

```java
codigo == codigoInformado
```

Para objetos `String`, `==` compara referências. O método `equals` compara o conteúdo textual.

### Comparação de `BigDecimal`

`BigDecimal` também é um objeto. Use `compareTo`:

| Resultado | Significado |
|---|---|
| menor que zero | `custoEstimado` é menor que o limite |
| igual a zero | os valores são equivalentes |
| maior que zero | `custoEstimado` é maior que o limite |

O método pergunta se o custo é maior; por isso usa:

```java
compareTo(limite) > 0
```

---

## Exercício 40 — Informar a situação atual

Crie um método com `if`, `else if` e `else`:

```java
public String obterSituacao() {
    if (concluida) {
        return "CONCLUÍDA";
    } else if (emAndamento) {
        return "EM ANDAMENTO";
    } else {
        return "PENDENTE";
    }
}
```

A ordem das condições importa. Uma tarefa concluída não está mais em andamento, mas consultar primeiro `concluida` torna a intenção explícita.

Em COBOL, uma seleção com caminhos mutuamente exclusivos poderia usar `IF`/`ELSE` ou `EVALUATE`. Em Java, `if` é apropriado quando a decisão depende de expressões booleanas.

---

## Exercício 41 — Cadastrar as três tarefas na aplicação

Em `AplicacaoGestorTarefas`, acrescente os imports:

```java
import java.math.BigDecimal;
import java.time.LocalDate;
```

Depois de cadastrar o responsável, crie e cadastre três tarefas:

```java
Tarefa tarefaRevisarRequisitos = new Tarefa();
tarefaRevisarRequisitos.cadastrar(
        "TAR-001",
        "Revisar requisitos",
        "Conferir os requisitos do projeto",
        3,
        LocalDate.of(2026, 10, 10),
        new BigDecimal("150.00"),
        responsavel);

Tarefa tarefaImplementarTela = new Tarefa();
tarefaImplementarTela.cadastrar(
        "TAR-002",
        "Implementar a tela inicial",
        "Criar a estrutura visual inicial",
        2,
        LocalDate.of(2026, 10, 12),
        new BigDecimal("300.00"),
        responsavel);

Tarefa tarefaValidarAplicacao = new Tarefa();
tarefaValidarAplicacao.cadastrar(
        "TAR-003",
        "Validar a aplicação",
        "Executar os testes previstos",
        1,
        LocalDate.of(2026, 10, 13),
        new BigDecimal("80.00"),
        responsavel);
```

Exiba os títulos pelos métodos de consulta. As mensagens deixam de ser fixas:

```java
System.out.println("- " + tarefaRevisarRequisitos.obterTitulo());
System.out.println("- " + tarefaImplementarTela.obterTitulo());
System.out.println("- " + tarefaValidarAplicacao.obterTitulo());
```

---

## Exercício 42 — Testar uma regra com `if` e retorno

Desative o responsável e tente iniciar a primeira tarefa:

```java
responsavel.desativar();

boolean iniciou = tarefaRevisarRequisitos.iniciar();

if (iniciou) {
    System.out.println("Tarefa iniciada.");
} else {
    System.out.println("Tarefa não iniciada: responsável inativo.");
}
```

O resultado esperado é:

```text
Tarefa não iniciada: responsável inativo.
```

Ative o responsável e tente novamente:

```java
responsavel.ativar();
iniciou = tarefaRevisarRequisitos.iniciar();
```

Agora `iniciou` deverá receber `true`.

Esse teste demonstra duas responsabilidades:

- `Tarefa` decide se pode iniciar;
- `AplicacaoGestorTarefas` decide qual mensagem exibir com base no retorno.

---

## Exercício 43 — Usar `for` para registrar as horas

A primeira tarefa possui estimativa de três horas. Use uma repetição:

```java
for (int hora = 1; hora <= 3; hora++) {
    boolean registrou = tarefaRevisarRequisitos.registrarHoraTrabalhada();

    if (registrou) {
        System.out.println(
                "Hora registrada: "
                + tarefaRevisarRequisitos.obterHorasRealizadas());
    }
}
```

O `for` concentra três partes do controle:

```java
for (inicialização; condição; atualização)
```

| Java | Ideia aproximada em `PERFORM VARYING` |
|---|---|
| `int hora = 1` | `FROM 1` |
| `hora <= 3` | repetir enquanto ainda não passou de 3 |
| `hora++` | `BY 1` |

Ao registrar a terceira hora, `Tarefa` deverá alterar sua situação para concluída.

Exiba:

```java
System.out.println(tarefaRevisarRequisitos.obterSituacao());
```

Resultado esperado:

```text
CONCLUÍDA
```

---

## Exercício 44 — Usar `while` até a conclusão

Inicie `tarefaImplementarTela`:

```java
tarefaImplementarTela.iniciar();
```

Depois, registre horas enquanto ela não estiver concluída:

```java
while (!tarefaImplementarTela.estaConcluida()) {
    tarefaImplementarTela.registrarHoraTrabalhada();

    System.out.println(
            tarefaImplementarTela.obterTitulo()
            + ": "
            + tarefaImplementarTela.obterHorasRealizadas()
            + " hora(s)");
}
```

`while` testa a condição antes de cada repetição. Ele se aproxima de um `PERFORM UNTIL`, mas existe uma diferença de leitura:

- `PERFORM UNTIL concluída`: repita até ficar concluída;
- `while (!estaConcluida())`: repita enquanto não estiver concluída.

Verifique sempre se alguma instrução dentro da repetição modifica a condição. Neste exercício, `registrarHoraTrabalhada` aumenta as horas e poderá concluir a tarefa. Sem essa mudança, a repetição poderia nunca terminar.

---

## Exercício 45 — Usar `do-while` em uma tentativa controlada

Para praticar uma repetição que executa pelo menos uma vez, use a terceira tarefa:

```java
int tentativa = 1;
boolean tarefaIniciada;

do {
    tarefaIniciada = tarefaValidarAplicacao.iniciar();
    System.out.println("Tentativa de início: " + tentativa);
    tentativa++;
} while (!tarefaIniciada && tentativa <= 3);
```

O bloco do `do` é executado antes do teste. Como o responsável está ativo e a tarefa está pendente, a primeira tentativa deverá funcionar e a repetição será encerrada.

Depois, conclua a tarefa explicitamente:

```java
boolean concluiu = tarefaValidarAplicacao.concluir();

if (concluiu) {
    System.out.println("Validação concluída.");
}
```

---

## Exercício 46 — Selecionar uma operação com `switch`

Crie uma variável dentro de `main`:

```java
String operacao = "CONSULTAR";
```

Use `switch`:

```java
switch (operacao) {
    case "INICIAR":
        System.out.println("Operação escolhida: iniciar tarefa.");
        break;
    case "CONCLUIR":
        System.out.println("Operação escolhida: concluir tarefa.");
        break;
    case "CONSULTAR":
        System.out.println("Operação escolhida: consultar tarefa.");
        break;
    default:
        System.out.println("Operação desconhecida.");
}
```

Em COBOL, uma estrutura semelhante poderia usar:

```cobol
EVALUATE WS-OPERACAO
    WHEN "INICIAR"
        DISPLAY "OPERACAO ESCOLHIDA: INICIAR TAREFA"
    WHEN "CONCLUIR"
        DISPLAY "OPERACAO ESCOLHIDA: CONCLUIR TAREFA"
    WHEN "CONSULTAR"
        DISPLAY "OPERACAO ESCOLHIDA: CONSULTAR TAREFA"
    WHEN OTHER
        DISPLAY "OPERACAO DESCONHECIDA"
END-EVALUATE
```

Não conclua que `switch` substitui todo `EVALUATE`. Aqui os dois são comparados porque selecionam um caminho entre valores conhecidos.

---

## Exercício 47 — Testar as comparações

Use os métodos criados no Exercício 39:

```java
boolean codigoEncontrado = tarefaRevisarRequisitos.possuiCodigo("TAR-001");

if (codigoEncontrado) {
    System.out.println("Tarefa TAR-001 encontrada.");
}

BigDecimal limite = new BigDecimal("100.00");

if (tarefaRevisarRequisitos.possuiCustoAcimaDe(limite)) {
    System.out.println("A tarefa possui custo acima de R$ 100,00.");
}
```

Faça mais dois testes:

1. procure o código `TAR-999` e confirme que o retorno é `false`;
2. verifique se `tarefaValidarAplicacao` possui custo acima de `100.00` e confirme que o retorno é `false`.

---

## Exercício 48 — Produzir o resumo final

Ao final de `main`, exiba:

```java
System.out.println();
System.out.println("=== RESUMO FINAL ===");
System.out.println(
        tarefaRevisarRequisitos.obterCodigo()
        + " - "
        + tarefaRevisarRequisitos.obterTitulo()
        + " - "
        + tarefaRevisarRequisitos.obterSituacao());

System.out.println(
        tarefaImplementarTela.obterCodigo()
        + " - "
        + tarefaImplementarTela.obterTitulo()
        + " - "
        + tarefaImplementarTela.obterSituacao());

System.out.println(
        tarefaValidarAplicacao.obterCodigo()
        + " - "
        + tarefaValidarAplicacao.obterTitulo()
        + " - "
        + tarefaValidarAplicacao.obterSituacao());
```

As três tarefas deverão aparecer como concluídas.

---

## Exercício 49 — Conferência final do Tópico 3

### Encapsulamento

- [ ] Todos os atributos de `Tarefa` são `private`.
- [ ] Todos os atributos de `Responsavel` são `private`.
- [ ] A aplicação não acessa atributos diretamente.
- [ ] Alterações acontecem por métodos que expressam uma intenção.
- [ ] Não foram criados setters genéricos para conclusão ou horas realizadas.

### Métodos, parâmetros e retornos

- [ ] `Responsavel` possui métodos de cadastro, consulta, ativação e desativação.
- [ ] `Tarefa` possui métodos de cadastro, consulta e alteração de situação.
- [ ] `this` é usado quando parâmetros e atributos têm o mesmo nome.
- [ ] Métodos de ação usam `void` ou `boolean` conforme sua necessidade.
- [ ] Métodos de consulta devolvem valores com `return`.
- [ ] `return` não foi confundido com `System.out.println`.

### Condições e comparações

- [ ] `iniciar` verifica as condições antes de alterar a tarefa.
- [ ] `registrarHoraTrabalhada` impede operações inválidas.
- [ ] A tarefa é concluída ao atingir a estimativa.
- [ ] Códigos são comparados com `equals`.
- [ ] Valores `BigDecimal` são comparados com `compareTo`.
- [ ] `if`, `else if`, `else`, operadores lógicos e negação foram usados.

### Repetições

- [ ] `for` registra uma quantidade conhecida de horas.
- [ ] `while` repete enquanto a tarefa não está concluída.
- [ ] `do-while` executa uma tentativa antes de testar a condição.
- [ ] As condições das repetições são modificadas dentro do fluxo.
- [ ] Nenhuma repetição fica infinita.

### Compilação e execução

- [ ] Todos os arquivos compilam sem erros.
- [ ] A aplicação executa pelo terminal.
- [ ] A aplicação executa pelo VS Code.
- [ ] A regra do responsável inativo é respeitada.
- [ ] O resumo final mostra as três tarefas concluídas.

---

## Entrega

Entregue a pasta inteira `gestor-tarefas`, preservando a estrutura:

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

## Comandos de referência

Execute a partir da pasta `gestor-tarefas`:

```bat
javac -d out src\br\com\curso\tarefas\dominio\*.java src\br\com\curso\tarefas\AplicacaoGestorTarefas.java
java -cp out br.com.curso.tarefas.AplicacaoGestorTarefas
```

## Síntese da evolução

No Tópico 1, o projeto validava o ambiente e exibia mensagens. No Tópico 2, passou a representar conceitos por classes, atributos e objetos. No Tópico 3, esses objetos adquiriram comportamento.

Agora `Tarefa` não é apenas um conjunto de campos. Ela controla quando pode ser iniciada, quando aceita horas trabalhadas, quando é concluída e como seus valores são consultados. `Responsavel` também protege seu estado e oferece operações coerentes com sua responsabilidade.

As estruturas conhecidas do COBOL continuam servindo como referência: `IF`, `EVALUATE`, `PERFORM UNTIL`, `PERFORM VARYING`, parâmetros e resultados de rotinas ainda ajudam a compreender o fluxo. A mudança principal está na organização: as regras relacionadas a uma tarefa ficam dentro da classe `Tarefa`, próximas do estado que precisam proteger.
