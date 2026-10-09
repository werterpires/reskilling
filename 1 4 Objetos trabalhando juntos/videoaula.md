# Roteiro de videoaula — Tópico 4: Fazer objetos trabalharem juntos

## Informações gerais

- **Tema:** relacionamentos, coleções, herança, polimorfismo, interfaces e consolidação do pensamento orientado a objetos.
- **Público:** profissionais em transição de COBOL para Java.
- **Exemplo central:** aplicação de gestão bancária.
- **Duração sugerida:** 110 a 130 minutos.
- **Formato:** exposição por tópicos + comparações COBOL/Java + refatoração progressiva no VS Code.
- **Ponto de partida:** projeto bancário do Tópico 3, com `Conta` encapsulada e métodos de depósito, saque, bloqueio e consulta.
- **Objetivo:** transformar objetos isolados em uma aplicação na qual clientes, contas, transações e banco colaboram.
- **Resultado concreto:** `Banco` administrando uma coleção polimórfica de contas; `Transacao` relacionando contas; `ContaCorrente` e `ContaPoupanca` especializando `Conta`; processamento comum sem condições baseadas no tipo.

### Fora do escopo

- persistência e banco de dados;
- JPA, Hibernate ou Spring;
- APIs REST;
- entrada pelo teclado;
- exceções personalizadas;
- testes automatizados;
- concorrência;
- transações bancárias reais;
- generics personalizados;
- detalhes internos de `ArrayList`;
- padrões de projeto formais;
- hierarquias extensas;
- recursos avançados de interfaces;
- comparação detalhada entre todas as implementações de `Collection`.

### Mensagem que deve atravessar toda a gravação

> Orientação a objetos não é dividir o código em arquivos. É distribuir responsabilidades entre objetos que se relacionam e colaboram por contratos compreensíveis.

### Resultado conceitual esperado

- Entender que objetos podem guardar referências para outros objetos.
- Distinguir relacionamento com um objeto e relacionamento com vários objetos.
- Compreender uma coleção como conjunto tipado de referências.
- Percorrer e buscar objetos em uma coleção.
- Distinguir relacionamento “tem um” de herança “é um”.
- Compreender sobrescrita e polimorfismo.
- Trabalhar com um tipo geral sem perder o comportamento específico.
- Entender interface como contrato, se houver tempo.
- Consolidar a mudança de organização procedural para orientação a objetos.

---

## 1. Abertura — uma conta isolada ainda não forma um banco

### Retomar o ponto final do Tópico 3

```java
Conta conta = new Conta();
conta.cadastrar(
        "12345-6",
        "0001",
        "Ana Martins",
        new BigDecimal("1000.00")
);

conta.depositar(new BigDecimal("200.00"));
conta.sacar(new BigDecimal("150.00"));
```

### Perguntas de abertura

- “Uma instituição bancária possui apenas uma conta?”
- “Onde ficam todas as contas administradas pelo banco?”
- “Como representar que uma conta pertence a um cliente?”
- “Como registrar uma transferência que relaciona conta de origem e conta de destino?”
- “Conta-corrente e poupança repetirão todo o código?”
- “O banco precisa perguntar o tipo de cada conta antes de chamar uma operação?”

### Pontos para comentar

- O Tópico 2 criou conceitos separados.
- O Tópico 3 deu comportamento e protegeu o estado.
- Neste tópico, os objetos deixam de trabalhar isoladamente.
- Um sistema real é uma rede de colaboração.
- A próxima pergunta é:

> Quais objetos precisam se conhecer e por qual motivo?

### Frases de condução possíveis

- “Uma classe bem feita, sozinha, ainda não é uma aplicação.”
- “O banco nasce quando contas, clientes e transações começam a colaborar.”
- “Relacionar não significa copiar todos os dados de uma classe para outra.”
- “O fluxo continua existindo; agora ele percorre e coordena objetos.”

---

## 2. Relacionamentos — um objeto conhece outro objeto

### Mostrar o modelo inicial de cliente

```java
package br.com.curso.banco.dominio;

public class Cliente {

    private String cpf;
    private String nome;

    public void cadastrar(String cpf, String nome) {
        this.cpf = cpf;
        this.nome = nome;
    }

    public String obterNome() {
        return nome;
    }
}
```

### Relacionar `Conta` com `Cliente`

Substituir o texto do titular por uma referência:

```java
private Cliente titular;
```

Alterar o cadastro atual de `Conta`:

```java
public void cadastrar(
        String numero,
        String agencia,
        Cliente titular,
        BigDecimal saldoInicial
) {
    this.numero = numero;
    this.agencia = agencia;
    this.titular = titular;
    this.saldo = saldoInicial;
    this.ativa = true;
}
```

### Explicar

- `Cliente` é o tipo do atributo.
- `titular` guarda uma referência para um objeto `Cliente`.
- A conta não precisa copiar CPF, nome e todos os dados do cliente.
- A conta pode solicitar ao cliente uma informação:

```java
public String obterNomeTitular() {
    return titular.obterNome();
}
```

### Mostrar a colaboração

```java
Cliente cliente = new Cliente();
cliente.cadastrar("123.456.789-00", "Ana Martins");

Conta conta = new Conta();
conta.cadastrar(
        "12345-6",
        "0001",
        cliente,
        new BigDecimal("1000.00")
);
```

### Pontos para comentar

- O argumento é a referência `cliente`.
- A conta e a variável da aplicação alcançam o mesmo objeto.
- Não foi criado um segundo cliente dentro da conta.
- Relacionamentos devem representar necessidades do domínio.
- Nem toda classe precisa conhecer todas as outras.

### Ponte com COBOL

- Em COBOL, relações podem ser representadas por chaves, registros, índices e áreas de comunicação.
- Uma conta poderia guardar um código de cliente e um processo localizaria o registro correspondente.
- Em Java, uma referência pode apontar diretamente para o objeto relacionado.
- Não afirmar que referência e chave de arquivo são a mesma coisa.
- A intenção comum é conectar informações que pertencem a conceitos diferentes.

### Alerta de modelagem

Evitar colocar na conta:

```java
private String cpfTitular;
private String nomeTitular;
private boolean clienteAtivo;
```

quando esses dados já pertencem ao objeto `Cliente`.

### Frase de síntese

> Um relacionamento orientado a objetos permite que um objeto colabore com outro sem assumir a responsabilidade de representar todos os seus dados.

---

## 3. Um relacionamento pode envolver um ou vários objetos

### Relação com um objeto

```java
private Cliente titular;
```

- Uma conta possui uma referência para um titular neste modelo.
- Essa é uma relação singular.

### Relação envolvendo duas contas

Criar `Transacao`:

```java
package br.com.curso.banco.dominio;

import java.math.BigDecimal;

public class Transacao {

    private Conta contaOrigem;
    private Conta contaDestino;
    private BigDecimal valor;

    public void registrar(
            Conta contaOrigem,
            Conta contaDestino,
            BigDecimal valor
    ) {
        this.contaOrigem = contaOrigem;
        this.contaDestino = contaDestino;
        this.valor = valor;
    }
}
```

### Pontos para comentar

- O mesmo tipo `Conta` aparece em dois papéis.
- Os nomes dos atributos comunicam origem e destino.
- Uma transação relaciona objetos já existentes.
- Ela não precisa copiar saldos para representar a relação.
- A operação completa de transferência será coordenada depois.

### Relação com vários objetos

- Um banco administra várias contas.
- Um banco também pode registrar várias transações.
- Um único atributo `Conta` não resolve essa necessidade.
- Precisamos de coleções.

### Quadro para mostrar

| Classe | Relacionamento inicial |
|---|---|
| `Conta` | um `Cliente` titular |
| `Transacao` | uma conta de origem e uma de destino |
| `Banco` | várias contas e várias transações |

### Perguntas de modelagem

- “Quem precisa navegar até quem?”
- “A relação é necessária para alguma operação?”
- “É uma referência ou várias?”
- “Os dois lados realmente precisam manter a relação?”

### Evitar

- Criar relacionamentos apenas porque parecem completos.
- Fazer `Cliente` conhecer `Conta` e `Conta` conhecer `Cliente` sem necessidade.
- Manter duas coleções equivalentes e esquecer de atualizar uma delas.
- Transformar todo relacionamento em herança.

---

## 4. Coleções — o banco precisa administrar várias contas

### Mostrar a necessidade antes da sintaxe

Código que não escala:

```java
private Conta conta1;
private Conta conta2;
private Conta conta3;
```

### Perguntas

- “E quando chegar a quarta conta?”
- “Quantos atributos seriam necessários?”
- “Como percorrer todas as contas?”
- “Como procurar uma conta pelo número?”

### Introduzir `List`

```java
private List<Conta> contas;
```

### Explicar os sinais de menor e maior

- `<Conta>` indica o tipo aceito pela coleção.
- Não significa comparação matemática.
- A lista foi tipada para contas.
- O compilador impede adicionar um `Cliente` nessa lista.

### Inicializar com `ArrayList`

```java
private List<Conta> contas = new ArrayList<>();
```

### Imports

```java
import java.util.ArrayList;
import java.util.List;
```

### Explicar `List` × `ArrayList`

- `List` é o tipo geral usado na declaração.
- `ArrayList` é a implementação concreta criada.
- Não aprofundar estruturas internas, capacidade ou complexidade algorítmica.
- Antecipar apenas que essa separação está relacionada ao uso de interfaces.

### Operações básicas

```java
contas.add(conta);
int quantidade = contas.size();
Conta primeiraConta = contas.get(0);
```

### Explicar

- `add`: adiciona uma referência.
- `size`: devolve a quantidade.
- `get`: devolve o elemento de determinada posição.
- A primeira posição é zero.
- Posição inexistente gera erro em execução.
- A lista guarda referências; não transforma todas as contas em um único objeto.

### Encapsular a coleção

Em `Banco`:

```java
public void adicionarConta(Conta conta) {
    contas.add(conta);
}

public int obterQuantidadeContas() {
    return contas.size();
}
```

### Pontos para comentar

- O atributo `contas` continua privado.
- A aplicação solicita `adicionarConta`.
- `Banco` controla sua coleção.
- Não é necessário devolver a lista mutável para qualquer classe alterar.

---

## 5. Ponte entre `OCCURS` e coleções

### Mostrar uma estrutura COBOL conhecida

```cobol
01 WS-CONTAS.
   05 WS-CONTA OCCURS 100 TIMES.
      10 WS-CONTA-NUMERO PIC X(10).
      10 WS-CONTA-SALDO  PIC S9(9)V99 COMP-3.
```

### Mostrar Java

```java
List<Conta> contas = new ArrayList<>();
```

### Semelhanças de intenção

- reunir várias ocorrências;
- percorrer elementos;
- localizar uma ocorrência;
- executar um processamento para cada item.

### Diferenças que precisam ser explicitadas

| `OCCURS` | `List<Conta>` |
|---|---|
| estrutura declarada com ocorrências | coleção criada como objeto |
| quantidade frequentemente definida | tamanho pode crescer durante a execução |
| campos descritos dentro da tabela | elementos são objetos `Conta` |
| acesso por posição ou índice | métodos como `add`, `get`, `size` e `remove` |
| ocorrência contém os campos | elemento da lista é uma referência |

### Frase de cuidado

- “A experiência com tabelas ajuda, mas `List` não é apenas um `OCCURS` escrito de outra forma.”
- “Cada elemento da lista continua sendo um objeto com estado e comportamento próprios.”

---

## 6. Percorrer uma coleção com `for` aprimorado

### Mostrar a forma com índice primeiro

```java
for (int indice = 0; indice < contas.size(); indice++) {
    Conta conta = contas.get(indice);
    System.out.println(conta.obterNumero());
}
```

### Explicar

- `indice` começa em zero.
- A condição evita ultrapassar o tamanho.
- `get(indice)` devolve a referência da posição atual.
- Essa forma é útil quando a posição realmente importa.

### Mostrar o `for` aprimorado

```java
for (Conta conta : contas) {
    System.out.println(conta.obterNumero());
}
```

### Leitura sugerida

> Para cada `Conta` da coleção `contas`, use a referência atual na variável `conta`.

### Pontos para comentar

- Não há contador explícito.
- O foco está no elemento.
- Continua sendo uma repetição.
- A variável `conta` recebe cada referência, uma por vez.
- Não cria cópias completas das contas.

### Ponte com `PERFORM VARYING`

```cobol
PERFORM VARYING WS-INDICE FROM 1 BY 1
        UNTIL WS-INDICE > WS-QUANTIDADE-CONTAS
    DISPLAY WS-CONTA-NUMERO(WS-INDICE)
END-PERFORM
```

### Explicar a diferença

- COBOL mostra explicitamente o índice.
- O `for` aprimorado esconde a posição quando ela não é relevante.
- A intenção comum é processar todas as ocorrências.

---

## 7. Buscar uma conta na coleção

### Criar método em `Banco`

```java
public Conta buscarConta(String numero) {
    for (Conta conta : contas) {
        if (conta.obterNumero().equals(numero)) {
            return conta;
        }
    }

    return null;
}
```

### Explicar o fluxo

1. Percorrer cada conta.
2. Obter o número da conta atual.
3. Comparar conteúdo textual com `equals`.
4. Retornar imediatamente quando encontrar.
5. Retornar `null` quando terminar sem resultado.

### Introduzir `null` com cuidado

- `null` representa ausência de referência.
- Não é texto vazio.
- Não é número zero.
- Não é um objeto `Conta` vazio.
- Não é possível chamar método em `null`.

### Verificação na aplicação

```java
Conta contaEncontrada = banco.buscarConta("12345-6");

if (contaEncontrada != null) {
    System.out.println(contaEncontrada.obterSaldo());
} else {
    System.out.println("Conta não encontrada");
}
```

### Alerta

Este código falha se o resultado for `null`:

```java
banco.buscarConta("99999-9").obterSaldo();
```

### Limite

- Não introduzir `Optional` nesta aula.
- Não introduzir streams para busca.
- Primeiro consolidar coleção, repetição, condição e retorno.

---

## 8. Demonstração progressiva — criar `Banco.java`

### Etapa 1 — pacote e imports

```java
package br.com.curso.banco.dominio;

import java.util.ArrayList;
import java.util.List;
```

### Etapa 2 — coleção privada

```java
public class Banco {

    private List<Conta> contas = new ArrayList<>();
}
```

### Etapa 3 — adicionar conta

```java
public void adicionarConta(Conta conta) {
    contas.add(conta);
}
```

### Etapa 4 — quantidade

```java
public int obterQuantidadeContas() {
    return contas.size();
}
```

### Etapa 5 — busca

```java
public Conta buscarConta(String numero) {
    for (Conta conta : contas) {
        if (conta.obterNumero().equals(numero)) {
            return conta;
        }
    }

    return null;
}
```

### Etapa 6 — exibir resumo

```java
public void exibirResumoContas() {
    for (Conta conta : contas) {
        System.out.println(
                conta.obterNumero()
                + " — "
                + conta.obterNomeTitular()
                + " — R$ "
                + conta.obterSaldo()
        );
    }
}
```

### Pausas para perguntas

- “Por que `contas` é privado?”
- “Quem controla a inclusão de contas?”
- “O `for` percorre objetos ou campos soltos?”
- “Por que usamos `equals` no número?”
- “O que significa o retorno `null`?”

### Compilar antes da herança

```bat
javac -d out src\br\com\curso\banco\dominio\*.java src\br\com\curso\banco\AplicacaoBanco.java
```

- Executar e confirmar a coleção funcionando.
- Criar duas contas ainda do tipo atual.
- Adicionar ambas ao banco.
- Exibir quantidade e resumo.
- Só depois avançar para tipos diferentes de conta.

---

## 9. Herança — conta-corrente e poupança compartilham uma base

### Apresentar a necessidade

- Conta-corrente e poupança possuem número, agência, titular, saldo e situação.
- Ambas depositam, sacam, bloqueiam e consultam saldo.
- Algumas regras variam, como tarifa mensal.
- Copiar a classe inteira cria duplicação.

### Desenhar verbalmente

```text
Conta
├── ContaCorrente
└── ContaPoupanca
```

### Explicar a relação “é um”

- Conta-corrente **é uma** conta.
- Conta-poupança **é uma** conta.
- Cliente não é uma conta.
- Banco não é uma conta.
- Transação não é uma conta.

### Contrastar “é um” e “tem um”

| Afirmação | Modelo |
|---|---|
| conta tem um titular | relacionamento com `Cliente` |
| banco possui várias contas | coleção de `Conta` |
| conta-corrente é uma conta | herança |

### Mensagem importante

- Herança não representa qualquer ligação.
- Herança não deve ser usada apenas para reutilizar código.
- O conceito específico precisa poder ser usado onde o geral é esperado.

---

## 10. Refatorar `Conta` para uma classe abstrata

### Alterar a declaração

```java
public abstract class Conta {
```

### Explicar `abstract`

- `Conta` passa a representar a base geral.
- Não criaremos `new Conta()`.
- Criaremos objetos `ContaCorrente` e `ContaPoupanca`.
- A classe abstrata pode manter atributos e métodos implementados.
- Ela também pode exigir comportamentos das subclasses.

### Manter atributos comuns privados

```java
private String numero;
private String agencia;
private Cliente titular;
private boolean ativa;
private BigDecimal saldo;
```

### Método protegido de cadastro

```java
protected void cadastrarConta(
        String numero,
        String agencia,
        Cliente titular,
        BigDecimal saldoInicial
) {
    this.numero = numero;
    this.agencia = agencia;
    this.titular = titular;
    this.saldo = saldoInicial;
    this.ativa = true;
}
```

### Explicar `protected`

- Pode ser usado pela classe e por subclasses.
- Não precisa ser a porta pública principal da aplicação.
- Não transformar automaticamente todos os atributos em `protected`.
- Atributos continuam privados.
- Subclasses usam comportamentos oferecidos pela classe base.

### Manter métodos comuns

```java
public boolean depositar(BigDecimal valor) {
    if (!ativa || valor.compareTo(BigDecimal.ZERO) <= 0) {
        return false;
    }

    saldo = saldo.add(valor);
    return true;
}
```

```java
public boolean sacar(BigDecimal valor) {
    if (!ativa || valor.compareTo(BigDecimal.ZERO) <= 0) {
        return false;
    }

    if (saldo.compareTo(valor) < 0) {
        return false;
    }

    saldo = saldo.subtract(valor);
    return true;
}
```

### Declarar comportamento variável

```java
public abstract BigDecimal calcularTarifaMensal();
```

### Explicar método abstrato

- Possui assinatura.
- Não possui corpo na classe base.
- Toda subclasse concreta precisa fornecer implementação.
- A base garante que toda conta responde à operação.
- O valor da tarifa pode variar por tipo.

---

## 11. Criar `ContaCorrente`

### Código progressivo

```java
package br.com.curso.banco.dominio;

import java.math.BigDecimal;

public class ContaCorrente extends Conta {

    private BigDecimal tarifaMensal;

    public void cadastrarContaCorrente(
            String numero,
            String agencia,
            Cliente titular,
            BigDecimal saldoInicial,
            BigDecimal tarifaMensal
    ) {
        cadastrarConta(numero, agencia, titular, saldoInicial);
        this.tarifaMensal = tarifaMensal;
    }

    @Override
    public BigDecimal calcularTarifaMensal() {
        return tarifaMensal;
    }
}
```

### Explicar `extends`

- `ContaCorrente` especializa `Conta`.
- Recebe os métodos públicos da base.
- Pode usar o método protegido `cadastrarConta`.
- Acrescenta `tarifaMensal`.

### Explicar `@Override`

- Indica intenção de sobrescrever método herdado.
- O compilador verifica compatibilidade.
- O nome, parâmetros e tipo de retorno devem corresponder ao contrato.
- Não é apenas comentário.

### Criar objeto

```java
ContaCorrente contaCorrente = new ContaCorrente();
contaCorrente.cadastrarContaCorrente(
        "10001-1",
        "0001",
        cliente,
        new BigDecimal("1000.00"),
        new BigDecimal("19.90")
);
```

### Mostrar métodos herdados

```java
contaCorrente.depositar(new BigDecimal("100.00"));
contaCorrente.sacar(new BigDecimal("50.00"));
contaCorrente.obterSaldo();
```

---

## 12. Criar `ContaPoupanca`

```java
package br.com.curso.banco.dominio;

import java.math.BigDecimal;

public class ContaPoupanca extends Conta {

    public void cadastrarContaPoupanca(
            String numero,
            String agencia,
            Cliente titular,
            BigDecimal saldoInicial
    ) {
        cadastrarConta(numero, agencia, titular, saldoInicial);
    }

    @Override
    public BigDecimal calcularTarifaMensal() {
        return BigDecimal.ZERO;
    }
}
```

### Pontos para comentar

- Poupança compartilha estado e operações de `Conta`.
- Neste modelo didático, sua tarifa é zero.
- Não discutir regras bancárias reais.
- A diferença aparece na implementação do mesmo método.

### Criar objeto

```java
ContaPoupanca contaPoupanca = new ContaPoupanca();
contaPoupanca.cadastrarContaPoupanca(
        "20001-2",
        "0001",
        cliente,
        new BigDecimal("500.00")
);
```

### Comparar chamadas

```java
contaCorrente.calcularTarifaMensal();
contaPoupanca.calcularTarifaMensal();
```

- Mesmo nome de método.
- Mesma intenção.
- Implementações diferentes.

---

## 13. Sobrescrita não é sobrecarga

### Sobrescrita usada na aula

```java
@Override
public BigDecimal calcularTarifaMensal() {
    return BigDecimal.ZERO;
}
```

- Método definido pela classe base.
- Subclasse fornece implementação específica.
- Assinatura compatível.

### Sobrecarga apenas para contrastar

```java
depositar(BigDecimal valor)
depositar(BigDecimal valor, String descricao)
```

- Mesmo nome.
- Lista de parâmetros diferente.
- Não é o foco deste tópico.

### Mensagem para repetir

- “Sobrescrita permite que o objeto específico responda à operação definida pelo tipo geral.”
- “É essa sobrescrita que aparecerá no polimorfismo.”

---

## 14. Polimorfismo — uma referência geral, objetos específicos

### Mostrar atribuições

```java
Conta conta1 = new ContaCorrente();
Conta conta2 = new ContaPoupanca();
```

### Explicar cuidadosamente

- O tipo das variáveis é `Conta`.
- Os objetos reais são `ContaCorrente` e `ContaPoupanca`.
- A referência geral pode apontar para um objeto específico.
- O contrário não funciona automaticamente.
- O objeto não deixa de ser do tipo concreto.

### Mostrar chamadas

```java
System.out.println(conta1.calcularTarifaMensal());
System.out.println(conta2.calcularTarifaMensal());
```

### Resultado esperado

```text
19.90
0
```

### Explicar o despacho do método

- O compilador permite a chamada porque `Conta` declara o método.
- Em execução, Java observa o objeto real.
- Para `ContaCorrente`, executa a versão da conta-corrente.
- Para `ContaPoupanca`, executa a versão da poupança.
- Isso é comportamento polimórfico.

### Definição didática

> Polimorfismo permite tratar objetos diferentes por um tipo comum e obter de cada um a resposta correspondente à sua implementação.

### Evitar definição vazia

- Não resumir a “muitas formas” sem mostrar o mecanismo.
- Sempre ligar:
  - tipo geral;
  - objeto concreto;
  - método sobrescrito;
  - seleção em execução.

---

## 15. Coleção polimórfica

### Retomar `Banco`

```java
private List<Conta> contas = new ArrayList<>();
```

### Adicionar tipos diferentes

```java
banco.adicionarConta(contaCorrente);
banco.adicionarConta(contaPoupanca);
```

### Percorrer e calcular tarifas

```java
public void exibirTarifas() {
    for (Conta conta : contas) {
        System.out.println(
                conta.obterNumero()
                + " — tarifa: R$ "
                + conta.calcularTarifaMensal()
        );
    }
}
```

### Pergunta central

- “Onde está o `if` perguntando se é corrente ou poupança?”

### Explicar

- Não é necessário perguntar o tipo.
- `Banco` trabalha com `Conta`.
- Cada objeto responde à operação.
- A coleção aceita subclasses de `Conta`.
- O método correto é selecionado em execução.

### Comparar com decisão centralizada

Pseudocódigo procedural:

```text
SE TIPO-CONTA = "C"
    TARIFA = 19,90
SENÃO SE TIPO-CONTA = "P"
    TARIFA = 0
```

### Ponte com `EVALUATE`

```cobol
EVALUATE WS-TIPO-CONTA
    WHEN 'C'
        MOVE 19,90 TO WS-TARIFA
    WHEN 'P'
        MOVE 0 TO WS-TARIFA
END-EVALUATE
```

### Cuidado na comparação

- O `EVALUATE` não é “errado”.
- Há decisões em que ele continua apropriado.
- O problema aparece quando uma classe central precisa conhecer todos os tipos e todas as regras específicas.
- Com polimorfismo, a variação fica nas classes que representam os tipos.
- Um novo tipo de conta pode implementar o método sem aumentar esse fluxo de cálculo.

### Frase de síntese

> O banco pede a tarifa. A conta concreta sabe responder.

---

## 16. Transferência — vários objetos colaborando

### Criar método em `Banco`

```java
public boolean transferir(
        String numeroOrigem,
        String numeroDestino,
        BigDecimal valor
) {
    Conta origem = buscarConta(numeroOrigem);
    Conta destino = buscarConta(numeroDestino);

    if (origem == null || destino == null) {
        return false;
    }

    boolean retirado = origem.sacar(valor);

    if (!retirado) {
        return false;
    }

    boolean depositado = destino.depositar(valor);

    if (!depositado) {
        origem.depositar(valor);
        return false;
    }

    Transacao transacao = new Transacao();
    transacao.registrar(origem, destino, valor);
    transacoes.add(transacao);
    return true;
}
```

### Antes, acrescentar a coleção

```java
private List<Transacao> transacoes = new ArrayList<>();
```

### Ler o fluxo

1. Buscar origem.
2. Buscar destino.
3. Recusar se alguma não existir.
4. Solicitar saque à origem.
5. Recusar se o saque não ocorrer.
6. Solicitar depósito ao destino.
7. Restaurar o valor na origem se o depósito falhar, apenas para manter coerência no exemplo didático.
8. Criar a transação.
9. Relacionar origem, destino e valor.
10. Guardar a transação na coleção.

### Pontos para comentar

- `Banco` coordena o fluxo.
- `Conta` continua protegendo saque e depósito.
- `Transacao` representa o fato registrado.
- A coleção mantém várias transações.
- Os objetos colaboram sem abrir seus atributos.

### Alerta obrigatório

- O exemplo não representa uma transação bancária real.
- Não há concorrência, persistência, rollback seguro ou controle transacional.
- A finalidade é visualizar colaboração entre objetos.

### Relação com COBOL

- O fluxo ainda é perfeitamente reconhecível como processo.
- Existem busca, validação, débito, crédito e registro.
- A diferença é que cada objeto recebe uma parte coerente da responsabilidade.
- A coordenação não manipula diretamente o saldo.

---

## 17. Interfaces — se o tempo permitir

### Introduzir pela necessidade de contrato

- Herança define uma base comum para tipos relacionados.
- Interface declara uma capacidade que uma classe assume.
- Uma classe pode implementar mais de uma interface.
- Interface não deve ser criada apenas para aumentar a quantidade de arquivos.

### Criar `Movimentavel`

```java
package br.com.curso.banco.dominio;

import java.math.BigDecimal;

public interface Movimentavel {

    boolean depositar(BigDecimal valor);

    boolean sacar(BigDecimal valor);
}
```

### Fazer `Conta` implementar

```java
public abstract class Conta implements Movimentavel {
```

### Explicar

- `implements` indica que a classe assume o contrato.
- `Conta` já possui `depositar` e `sacar`.
- As assinaturas precisam ser compatíveis.
- Uma variável pode usar o tipo da interface:

```java
Movimentavel origem = contaCorrente;
```

### Possibilidade futura

- Uma `CarteiraDigital` poderia implementar `Movimentavel` sem estender `Conta`.
- Ela não seria uma conta na hierarquia criada.
- Ainda assim, ofereceria depósito e saque.
- Não implementar `CarteiraDigital` se o tempo estiver curto.

### Comparar classe abstrata e interface

| Classe abstrata `Conta` | Interface `Movimentavel` |
|---|---|
| representa uma base conceitual | representa capacidade ou contrato |
| mantém estado comum | não representa os atributos da conta |
| fornece implementações compartilhadas | define operações que devem estar disponíveis |
| usada com `extends` | usada com `implements` |
| uma classe estende uma classe | uma classe pode implementar várias interfaces |

### Ponte com COBOL

- Um contrato de chamada também define operações e dados esperados.
- A interface Java participa do sistema de tipos.
- O compilador verifica se a classe concreta cumpre o contrato.
- Não comparar interface com copybook como se fossem equivalentes.

### Decisão de tempo

- Se restarem menos de 10 minutos, apenas explicar o conceito e mostrar o código.
- Não comprometer a consolidação de relacionamento, coleção e polimorfismo.
- Interfaces são a extensão opcional prevista para o tópico.

---

## 18. Demonstração final no VS Code

### Estrutura esperada

```text
gestao-banco/
└── src/
    └── br/
        └── com/
            └── curso/
                └── banco/
                    ├── AplicacaoBanco.java
                    └── dominio/
                        ├── Banco.java
                        ├── Cliente.java
                        ├── Conta.java
                        ├── ContaCorrente.java
                        ├── ContaPoupanca.java
                        ├── Movimentavel.java
                        └── Transacao.java
```

> Se a parte de interface não for realizada, `Movimentavel.java` não precisa ser criado.

### Ordem da demonstração

1. Confirmar a compilação do projeto do Tópico 3.
2. Criar `Cliente` ou atualizar a classe existente.
3. Trocar o titular textual de `Conta` por `Cliente`.
4. Criar `Banco` com `List<Conta>`.
5. Adicionar contas e mostrar `size`.
6. Percorrer com `for` aprimorado.
7. Criar busca por número.
8. Testar busca existente e inexistente.
9. Transformar `Conta` em classe abstrata.
10. Criar método abstrato de tarifa.
11. Criar `ContaCorrente`.
12. Criar `ContaPoupanca`.
13. Adicionar os dois tipos na mesma lista.
14. Exibir tarifas polimorficamente.
15. Criar `Transacao` relacionando contas.
16. Criar transferência coordenada por `Banco`.
17. Executar a transferência.
18. Exibir saldos finais.
19. Se houver tempo, criar `Movimentavel`.
20. Compilar e executar novamente.

### Cenário sugerido

```java
Cliente ana = new Cliente();
ana.cadastrar("123.456.789-00", "Ana Martins");

Cliente bruno = new Cliente();
bruno.cadastrar("987.654.321-00", "Bruno Lima");

ContaCorrente contaCorrente = new ContaCorrente();
contaCorrente.cadastrarContaCorrente(
        "10001-1",
        "0001",
        ana,
        new BigDecimal("1000.00"),
        new BigDecimal("19.90")
);

ContaPoupanca contaPoupanca = new ContaPoupanca();
contaPoupanca.cadastrarContaPoupanca(
        "20001-2",
        "0001",
        bruno,
        new BigDecimal("500.00")
);

Banco banco = new Banco();
banco.adicionarConta(contaCorrente);
banco.adicionarConta(contaPoupanca);

banco.exibirResumoContas();
banco.exibirTarifas();

boolean transferida = banco.transferir(
        "10001-1",
        "20001-2",
        new BigDecimal("200.00")
);

System.out.println("Transferência realizada: " + transferida);
banco.exibirResumoContas();
```

### Saída aproximada

```text
10001-1 — Ana Martins — R$ 1000.00
20001-2 — Bruno Lima — R$ 500.00
10001-1 — tarifa: R$ 19.90
20001-2 — tarifa: R$ 0
Transferência realizada: true
10001-1 — Ana Martins — R$ 800.00
20001-2 — Bruno Lima — R$ 700.00
```

### Comandos

```bat
javac -d out src\br\com\curso\banco\dominio\*.java src\br\com\curso\banco\AplicacaoBanco.java
java -cp out br.com.curso.banco.AplicacaoBanco
```

### Também executar pelo VS Code

- Abrir `AplicacaoBanco.java`.
- Usar **Run** acima do `main`.
- Confirmar que a saída é equivalente.
- Reforçar que VS Code automatiza comandos, mas o projeto ainda depende do JDK e da estrutura correta de pacotes.

---

## 19. Consolidação — o que mudou nos quatro tópicos

### Tópico 1 — ambiente e primeira execução

- JDK instalado.
- `JAVA_HOME` e `PATH` configurados.
- `java` e `javac` validados.
- VS Code configurado.
- Primeira classe executada.

### Tópico 2 — domínio representado por objetos

- classes;
- objetos;
- atributos;
- tipos;
- pacotes;
- imports;
- responsabilidades iniciais.

### Tópico 3 — objetos com comportamento

- métodos;
- parâmetros;
- retornos;
- `this`;
- encapsulamento;
- condições;
- repetições.

### Tópico 4 — objetos colaborando

- relacionamentos;
- coleções;
- percurso e busca;
- herança;
- sobrescrita;
- polimorfismo;
- interfaces como contrato.

### Mostrar a evolução da pergunta

```text
1. Como compilar e executar?
2. Quais conceitos existem?
3. Qual objeto protege cada regra?
4. Como os objetos colaboram e variam por um tipo comum?
```

### Mensagem sobre COBOL

- O conhecimento procedural não foi descartado.
- Sequência, decisão, repetição, tabelas e chamadas continuam presentes.
- A experiência com regras de negócio continua valiosa.
- O novo aprendizado é a organização dessas regras em objetos colaboradores.
- Java não deve ser tratado como COBOL com palavras-chave diferentes.

### Comparação final

| Necessidade | Referência COBOL | Organização Java |
|---|---|---|
| agrupar dados | grupo de nível 01 | classe e atributos |
| representar ocorrências | `OCCURS` | coleção de objetos |
| executar rotina | `PERFORM` / `CALL` | chamada de método |
| decidir | `IF` / `EVALUATE` | `if` / `switch` ou comportamento polimórfico |
| percorrer ocorrências | `PERFORM VARYING` | `for` ou `for` aprimorado |
| relacionar registros | chaves e estruturas | referências entre objetos |
| variar regra por tipo | código de tipo + seleção | sobrescrita e polimorfismo |
| definir contrato | interface de chamada | interface Java |

### Cuidado ao apresentar a tabela

- As colunas não representam traduções exatas.
- São pontes de intenção.
- Cada linguagem possui modelo, sistema de tipos e mecanismos próprios.

---

## 20. Erros conceituais para antecipar durante a aula

### “Relacionamento significa colocar todos os campos em uma classe”

- Mostrar `Conta` com atributo `Cliente`.
- Explicar referência para o objeto relacionado.

### “Lista é apenas um array sem limite”

- Reforçar que `List` oferece contrato e operações.
- Os elementos são referências tipadas.
- Não aprofundar todas as implementações.

### “Posição começa em um, como em muitos exemplos COBOL”

- Reforçar `get(0)` como primeiro elemento.
- Usar `for` aprimorado quando a posição não importa.

### “Se a busca não encontra, ela devolve uma conta vazia”

- Explicar `null` como ausência de referência.
- Verificar antes da chamada.

### “Herança serve para qualquer relação”

- Repetir “é um” versus “tem um”.
- Cliente não estende Conta.
- Banco não estende Conta.

### “Herança exige atributos protegidos”

- Manter atributos privados.
- Oferecer métodos coerentes à subclasse.

### “Sobrescrita é criar método com mesmo nome e parâmetros diferentes”

- Isso seria sobrecarga.
- Sobrescrita redefine contrato herdado.

### “Polimorfismo é apenas misturar objetos numa lista”

- A coleção ajuda a visualizar.
- O ponto central é a mesma chamada executar implementações específicas.

### “Polimorfismo elimina todas as condições”

- Condições continuam necessárias.
- Polimorfismo reduz decisões baseadas apenas no tipo.

### “Interface e classe abstrata são iguais”

- Classe abstrata compartilha base, estado e implementação.
- Interface expressa contrato ou capacidade.

### “Quanto mais herança e interface, mais OO”

- Complexidade adicional precisa resolver uma necessidade real.
- Objetos simples colaborando são preferíveis a hierarquias artificiais.

---

## 21. Encerramento sugerido

### Recapitulação oral

- Conta passou a se relacionar com Cliente.
- Banco passou a reunir contas em uma coleção.
- A coleção foi percorrida e pesquisada.
- Conta tornou-se uma base para tipos específicos.
- Conta-corrente e poupança sobrescreveram o cálculo de tarifa.
- Banco processou ambas pelo tipo geral `Conta`.
- Transação relacionou origem, destino e valor.
- Interface mostrou como declarar uma capacidade comum.

### Mensagem final

> O fluxo procedural continua dentro da aplicação, mas já não carrega sozinho todas as regras. Cada objeto conhece seu estado, protege seu comportamento e colabora com os demais por relações e contratos claros.

### Reforçar a mudança de perspectiva

- Não perguntar apenas “qual comando vem depois?”.
- Perguntar “qual objeto deve responder por isso?”.
- Não copiar dados sem necessidade.
- Relacionar objetos pelos tipos do domínio.
- Usar coleções para administrar conjuntos.
- Usar herança somente quando houver especialização real.
- Usar polimorfismo para tratar variações por um contrato comum.

### Gancho para os exercícios

- Na aula, o exemplo foi bancário.
- Nos exercícios, a turma continuará o Gestor de Tarefas.
- `Responsavel` poderá se relacionar com várias tarefas.
- Um `GestorTarefas` poderá manter uma coleção.
- Tarefas comuns e tarefas urgentes poderão compartilhar uma base.
- O processamento deverá usar polimorfismo, sem repetir uma grande seleção de tipos.
- A interface será opcional, acompanhando o tempo disponível na aula.

---

## Checklist de gravação

- [ ] Retomar o resultado do Tópico 3.
- [ ] Explicar por que uma conta isolada ainda não forma um banco.
- [ ] Relacionar `Conta` e `Cliente`.
- [ ] Mostrar que relacionamento não é cópia de campos.
- [ ] Criar `Transacao` com origem e destino.
- [ ] Distinguir relação singular e relação com vários objetos.
- [ ] Introduzir `List<Conta>` a partir da necessidade.
- [ ] Explicar `List` e `ArrayList`.
- [ ] Mostrar imports de `java.util`.
- [ ] Demonstrar `add`, `size` e `get`.
- [ ] Comparar coleção com `OCCURS` sem declarar equivalência.
- [ ] Mostrar `for` com índice.
- [ ] Mostrar `for` aprimorado.
- [ ] Criar busca por número.
- [ ] Explicar retorno `null`.
- [ ] Verificar `null` antes de usar a referência.
- [ ] Manter a coleção encapsulada em `Banco`.
- [ ] Introduzir relação “é um”.
- [ ] Contrastar “é um” e “tem um”.
- [ ] Transformar `Conta` em classe abstrata.
- [ ] Explicar método abstrato.
- [ ] Explicar `protected` sem abrir os atributos.
- [ ] Criar `ContaCorrente` com `extends`.
- [ ] Criar `ContaPoupanca` com `extends`.
- [ ] Demonstrar `@Override`.
- [ ] Distinguir sobrescrita e sobrecarga.
- [ ] Usar referência do tipo geral `Conta`.
- [ ] Explicar seleção da implementação em execução.
- [ ] Adicionar tipos diferentes à mesma lista.
- [ ] Processar tarifas polimorficamente.
- [ ] Comparar polimorfismo e seleção por código de tipo.
- [ ] Criar transferência coordenada por `Banco`.
- [ ] Registrar a transação na coleção.
- [ ] Alertar que o exemplo não é uma transação bancária real.
- [ ] Apresentar interface somente se houver tempo.
- [ ] Distinguir classe abstrata e interface.
- [ ] Compilar pelo terminal.
- [ ] Executar pelo terminal.
- [ ] Executar pelo VS Code.
- [ ] Consolidar os quatro tópicos do curso.
- [ ] Encerrar reforçando que a experiência em COBOL continua útil.

---

## Resumo de tempo sugerido

| Bloco | Tempo aproximado |
|---|---:|
| retomada e relacionamentos | 15 minutos |
| coleções e ponte com `OCCURS` | 20 minutos |
| percurso, busca e `null` | 15 minutos |
| herança e classe abstrata | 20 minutos |
| sobrescrita e polimorfismo | 20 minutos |
| transferência entre objetos | 15 minutos |
| interfaces, se houver tempo | 10 minutos |
| consolidação e encerramento | 10 minutos |

O total pode variar conforme as perguntas e o ritmo da demonstração. Se for necessário reduzir, preserve relacionamentos, coleções e polimorfismo. A seção de interfaces é a primeira candidata a ser apenas apresentada conceitualmente.
