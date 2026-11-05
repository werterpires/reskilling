# Roteiro de videoaula — Tópico 2: Entender a mudança procedural → orientação a objetos

## Informações gerais

- **Tema:** mudança de organização do código: do procedural para a orientação a objetos.
- **Público:** profissionais em transição de COBOL para Java.
- **Exemplo central:** aplicação de gestão bancária.
- **Duração sugerida:** 60 a 90 minutos.
- **Formato:** exposição por tópicos + comparações COBOL/Java + demonstração no VS Code.
- **Objetivo:** compreender profundamente os fundamentos, sem avançar para lógica de programação.
- **Resultado concreto:** projeto bancário dividido em classes e pacotes, com atributos de tipos adequados, objetos criados e imports explicados.

### Fora do escopo

- decisões com `if`;
- repetições;
- entrada de dados;
- métodos próprios em profundidade;
- construtores personalizados;
- encapsulamento;
- persistência;
- cálculo de saldo;
- regras de transferência.

### Mensagem que deve atravessar toda a gravação

> Introdutório significa começar pela base. Não significa tratar a base superficialmente.

---

## 1. Abertura — o que muda e o que permanece

### Pontos para comentar

- A experiência em COBOL permanece útil.
- Continuam valendo:
  - leitura de processos;
  - identificação de dados;
  - regras de negócio;
  - análise de impacto;
  - rastreabilidade;
  - compilação;
  - cuidado com sistemas críticos.
- O primeiro choque é visual: chaves, pontos, arquivos e nomes novos.
- A mudança mais importante não é visual: é a forma de organizar o programa.

### Frases de abertura possíveis

- “Você não deixou de saber desenvolver software porque mudou de linguagem.”
- “Hoje não vamos trocar apenas palavras-chave; vamos trocar a maneira de distribuir o sistema.”
- “O procedural continua existindo dentro do Java. O que muda é o lugar que ele ocupa.”

### Apresentar a pergunta central

```text
Procedural: quais passos serão executados?
OO: quais elementos participam e pelo que cada um responde?
```

---

## 2. COBOL como ponto de partida real

### Mostrar uma estrutura bancária em COBOL

```cobol
01 WS-CONTA.
   05 WS-CONTA-NUMERO       PIC X(20).
   05 WS-CONTA-AGENCIA      PIC X(10).
   05 WS-CONTA-ATIVA        PIC X(1).
      88 CONTA-ATIVA        VALUE 'S'.
      88 CONTA-INATIVA      VALUE 'N'.
   05 WS-CONTA-SALDO        PIC S9(11)V99 COMP-3.
```

### Mostrar um fluxo procedural

```cobol
PROCEDURE DIVISION.
    VALIDAR-CLIENTE.
    VALIDAR-CONTA.
    REGISTRAR-TRANSACAO.
    EXIBIR-CONFIRMACAO.
```

### Pontos para comentar

- O nível 01 agrupa dados relacionados.
- As cláusulas `PIC` descrevem representação e capacidade.
- O nível 88 dá nomes de condição a valores.
- A `PROCEDURE DIVISION` torna o fluxo visível.
- Nada disso é conhecimento inútil ao chegar ao Java.

### Evitar caricatura

- COBOL não significa necessariamente código desorganizado.
- Programas COBOL podem possuir:
  - seções;
  - parágrafos;
  - subprogramas;
  - copybooks;
  - padrões rigorosos.
- A comparação é entre modelos predominantes de organização, não entre “linguagem antiga ruim” e “linguagem nova boa”.

---

## 3. A virada de perspectiva

### Mostrar lado a lado

| Foco procedural | Foco orientado a objetos |
|---|---|
| validar cliente | `Cliente` |
| validar conta | `Conta` |
| registrar transação | `Transacao` |
| coordenar operação | `Banco` |
| iniciar programa | `AplicacaoBanco` |

### Explicar

- Verbos ajudam a enxergar processos.
- Substantivos ajudam a identificar elementos do domínio.
- Os verbos não desaparecem.
- Os substantivos não viram classes automaticamente.
- O objetivo é encontrar responsabilidades coerentes.

### Perguntas para fazer

- “O número da agência pertence ao cliente, à conta ou à transação?”
- “O CPF deveria ficar dentro de `Conta`?”
- “Uma transação deveria copiar todos os dados da conta ou referenciar a conta?”
- “Qual classe inicia o programa? Qual classe representa o negócio?”

---

## 4. Por que Java não é COBOL com outra sintaxe

### Situação para apresentar

```text
AplicacaoBanco.java
    ├── dados de cliente
    ├── dados de conta
    ├── dados de transação
    ├── regras bancárias
    ├── relatórios
    ├── mensagens
    └── inicialização
```

### Pontos para comentar

- Esse código pode compilar.
- Ainda assim, uma classe saberia coisas demais.
- Toda mudança chegaria ao mesmo arquivo.
- Trocar `PERFORM` por chamadas Java não produz, sozinho, orientação a objetos.
- Dividir um arquivo em muitos arquivos também não basta.
- A separação precisa acompanhar conceitos e responsabilidades.

### Formulação importante

- “Orientação a objetos não significa ausência de procedimento.”
- “Métodos continuam contendo instruções em sequência.”
- “A diferença é que essas instruções vivem em uma estrutura composta por objetos responsáveis por partes do domínio.”

---

## 5. Classe — definição de um conceito

### Criar no VS Code

Arquivo:

```text
src\br\com\curso\banco\dominio\Conta.java
```

Código:

```java
package br.com.curso.banco.dominio;

public class Conta {
}
```

### Explicar linha por linha

- `package`: identidade e organização da classe.
- `public class`: declaração de classe.
- `Conta`: nome do conceito.
- `{ }`: conteúdo pertencente à classe.

### Paralelo com COBOL

```cobol
01 WS-CONTA.
   05 WS-CONTA-NUMERO  PIC X(20).
   05 WS-CONTA-AGENCIA PIC X(10).
```

```java
public class Conta {
    String numero;
    String agencia;
}
```

### Limite do paralelo

- O nível 01 agrupa dados.
- A classe também pode agrupar dados.
- A classe poderá reunir comportamentos e participar de relações entre objetos.
- Portanto, classe não é simplesmente um nível 01 com outra sintaxe.

### Convenções

- Classes normalmente no singular.
- `PascalCase`:
  - `Conta`;
  - `Cliente`;
  - `Transacao`;
  - `AplicacaoBanco`.
- Nome deve comunicar o conceito, não uma sequência genérica.

---

## 6. Atributos — informações pertencentes à classe

### Evoluir `Conta.java`

```java
package br.com.curso.banco.dominio;

import java.math.BigDecimal;

public class Conta {
    String numero;
    String agencia;
    boolean ativa;
    BigDecimal saldo;
}
```

### Explicar

| Atributo | Pertence a | Por quê |
|---|---|---|
| `numero` | `Conta` | identifica a conta |
| `agencia` | `Conta` | localiza a conta no banco |
| `ativa` | `Conta` | descreve sua situação |
| `saldo` | `Conta` | representa seu valor financeiro |

### Pontos para comentar

- Atributos descrevem estado.
- Não são passos executados na ordem em que aparecem.
- Cada objeto `Conta` poderá ter seus próprios valores.
- Não criar `conta1`, `conta2` e `conta3` como conceitos diferentes.
- Uma classe descreve a estrutura comum; objetos representam ocorrências.

---

## 7. Tipos — comparação detalhada com `PIC`

### Introdução

- Todo atributo Java possui um tipo.
- Tipo informa:
  - valores possíveis;
  - capacidade;
  - operações coerentes;
  - intenção para quem lê.
- Tipo não é apenas formato visual.

### 7.1 `PIC X(n)` × `String`

```cobol
05 WS-CONTA-NUMERO PIC X(20).
```

```java
String numero;
```

### Pontos para comentar

- Ambos podem representar texto.
- `PIC X(20)` reserva/define 20 posições.
- `String` não impõe automaticamente limite de 20.
- Se houver limite de negócio, ele precisará ser validado.
- `String` é classe, não tipo primitivo.
- Número de conta pode ser `String`:
  - pode possuir zeros à esquerda;
  - pode possuir formatação;
  - não é usado como quantidade matemática.

### Frase útil

- “Parecer número não significa ser número.”

### 7.2 `PIC X(1)` × `char`

```cobol
05 WS-TIPO-CONTA PIC X(1).
```

```java
char tipoConta;
```

### Pontos para comentar

- `char` representa uma unidade UTF-16.
- `PIC X(1)` representa uma posição alfanumérica no modelo COBOL.
- Não são equivalentes binários.
- Em Java, códigos curtos também podem ser representados por `String`, conforme o domínio.

### 7.3 `PIC 9(n)` × `int` e `long`

```cobol
05 WS-QUANTIDADE-TITULARES PIC 9(3).
```

```java
int quantidadeTitulares;
```

### Explicar

- `PIC 9(3)`: três dígitos decimais.
- `int`: inteiro binário sinalizado de 32 bits.
- Intervalo de `int`: aproximadamente -2,1 bilhões a 2,1 bilhões.
- `long`: inteiro sinalizado de 64 bits.
- Java não exige equivalente a `S` para que `int` e `long` aceitem negativos.

### Tabela para mostrar

| Tipo | Tamanho | Faixa aproximada |
|---|---:|---:|
| `byte` | 8 bits | -128 a 127 |
| `short` | 16 bits | -32 mil a 32 mil |
| `int` | 32 bits | -2,1 bilhões a 2,1 bilhões |
| `long` | 64 bits | -9 quintilhões a 9 quintilhões |

### Alertas

- Não usar `int` para CPF, conta ou agência apenas porque contêm dígitos.
- Identificador não é quantidade.
- Escolher `byte` só porque o valor é pequeno raramente ajuda em aplicação de negócio.

### 7.4 Indicador/nível 88 × `boolean`

```cobol
05 WS-CONTA-ATIVA PIC X(1).
   88 CONTA-ATIVA   VALUE 'S'.
   88 CONTA-INATIVA VALUE 'N'.
```

```java
boolean ativa;
```

### Pontos para comentar

- `boolean` aceita apenas `true` ou `false`.
- Campo `PIC X(1)` pode conter outros caracteres se não houver validação.
- Nível 88 nomeia condições associadas a valores de outro campo.
- `boolean` é um tipo próprio.
- O paralelo é a intenção de representar dois estados; o mecanismo é diferente.

### Pergunta para a turma

- “Se o campo COBOL receber `X`, o que acontece? E um `boolean` pode receber `X`?”

### 7.5 `PIC S9(n)V99 COMP-3` × `BigDecimal`

```cobol
05 WS-CONTA-SALDO PIC S9(11)V99 COMP-3.
```

```java
BigDecimal saldo;
```

### Pontos para comentar

- `BigDecimal` é classe.
- Precisa de import:

```java
import java.math.BigDecimal;
```

- `float` e `double` representam ponto flutuante binário.
- Muitos valores decimais não possuem representação binária exata.
- Dinheiro normalmente pede precisão e arredondamento controlados.
- Não ensinar operações de `BigDecimal` agora.
- Fixar apenas a escolha conceitual do tipo.

### 7.6 Data em campo × `LocalDate`

```cobol
05 WS-DATA-TRANSACAO PIC 9(8).
```

```java
LocalDate data;
```

### Pontos para comentar

- `PIC 9(8)` guarda oito dígitos; o significado depende da convenção.
- `LocalDate` representa uma data sem horário.
- Precisa de:

```java
import java.time.LocalDate;
```

- O tipo comunica significado e oferece comportamento de calendário.
- Não aprofundar API de datas neste tópico.

### Quadro consolidado

| Intenção | COBOL | Java | Atenção |
|---|---|---|---|
| texto | `PIC X(n)` | `String` | limite não é automático |
| caractere | `PIC X(1)` | `char` | representações diferentes |
| inteiro | `PIC 9(n)` | `int`/`long` | dígitos × faixa binária |
| condição | campo + nível 88 | `boolean` | apenas `true`/`false` |
| dinheiro | decimal/COMP-3 | `BigDecimal` | evitar escolha automática de `double` |
| data | campo formatado | `LocalDate` | tipo carrega significado de data |

---

## 8. Tipos primitivos e tipos por referência

### Tipos primitivos

```text
byte
short
int
long
float
double
char
boolean
```

### Explicar

- nomes em minúsculas;
- representam valores básicos;
- `int`, `long`, `float` e `double` são numéricos;
- `char` representa uma unidade de caractere;
- `boolean` representa dois estados.

### Tipos por referência

```text
String
BigDecimal
LocalDate
Cliente
Conta
Transacao
Banco
```

### Explicar

- Toda classe define um tipo.
- Classes criadas no projeto também podem ser tipos de atributos.
- Uma `Transacao` pode possuir atributos do tipo `Conta`.

### Mostrar

```java
public class Transacao {
    Conta contaOrigem;
    Conta contaDestino;
    BigDecimal valor;
    LocalDate data;
}
```

### Valor e referência

- variável primitiva contém o valor;
- variável de classe contém uma referência para um objeto;
- referência pode ser `null`;
- `null` significa ausência de objeto, não texto vazio nem zero.

### Valores iniciais de atributos

| Categoria | Valor inicial |
|---|---|
| inteiro | `0` |
| ponto flutuante | zero correspondente |
| `boolean` | `false` |
| `char` | caractere de valor zero |
| referência | `null` |

### Alerta

- Valor inicial da linguagem não é automaticamente regra de negócio.
- `ativa = false` inicialmente não significa que toda nova conta deva nascer inativa.

### Tipo não substitui validação

- `String cpf` ainda pode conter CPF inválido.
- `BigDecimal saldo` ainda pode representar valor inadequado.
- `LocalDate data` ainda pode representar data incompatível com outra.
- Bom tipo reduz possibilidades, mas não completa a regra.

---

## 9. Objeto — classe, ocorrência e referência

### Mostrar

```java
Conta conta = new Conta();
```

### Separar os conceitos

| Elemento | Significado |
|---|---|
| `Conta` | classe e tipo |
| `new Conta()` | criação de objeto |
| `conta` | variável que guarda a referência |

### Pontos para comentar

- Classe não é objeto.
- Uma classe pode originar muitos objetos.
- Dois objetos podem ter atributos iguais e continuar sendo objetos diferentes.
- Objeto possui identidade e estado.

### Paralelo cuidadoso com COBOL

- Nível 01 descreve estrutura.
- Uma ocorrência na memória possui valores concretos.
- Isso ajuda a começar a entender classe e objeto.
- O modelo Java acrescenta:
  - identidade de objeto;
  - referências entre objetos;
  - comportamentos associados à classe.

### Exemplo visual para falar

```text
Classe Conta
   ├── objeto: conta corrente 001
   ├── objeto: conta corrente 002
   └── objeto: conta poupança 003
```

---

## 10. Responsabilidades no domínio bancário

### Mostrar o mapa

| Classe | O que conhece | Responsabilidade inicial |
|---|---|---|
| `Cliente` | CPF, nome | representar o cliente |
| `Conta` | número, agência, situação, saldo | representar uma conta |
| `Transacao` | contas, valor e data | representar uma movimentação |
| `Banco` | conjunto e coordenação do domínio | coordenar operações bancárias |
| `AplicacaoBanco` | ponto de entrada | iniciar o programa |

### Perguntas para guiar

- “Saldo descreve cliente ou conta?”
- “Data de transação pertence à conta ou à transação?”
- “`AplicacaoBanco` deve guardar todos os dados do sistema?”
- “A transação deve duplicar número e agência ou referenciar uma conta?”

### Reforçar

- Responsabilidade não é sinônimo de arquivo.
- Criar muitas classes não garante boa orientação a objetos.
- A classe deve ter coesão: informações e comportamentos ligados ao mesmo propósito.
- Evitar uma “classe Deus” que conhece e executa tudo.

### Fluxo ainda existe

```text
AplicacaoBanco inicia
        ↓
Banco coordena
        ↓
Transacao relaciona objetos Conta
        ↓
Conta e Cliente representam o domínio
```

---

## 11. Pacotes desde cedo

### Criar a estrutura

```text
gestao-banco/
└── src/
    └── br/
        └── com/
            └── curso/
                └── banco/
                    ├── AplicacaoBanco.java
                    └── dominio/
                        ├── Cliente.java
                        ├── Conta.java
                        ├── Transacao.java
                        └── Banco.java
```

### Explicar duas funções do pacote

1. Agrupar classes relacionadas.
2. Participar do nome completo da classe.

### Mostrar

```java
package br.com.curso.banco.dominio;
```

Nome completo:

```text
br.com.curso.banco.dominio.Conta
```

### Pontos para comentar

- `Conta` pode existir em outro pacote sem ser a mesma classe.
- O nome completo elimina ambiguidade.
- A estrutura de diretórios normalmente acompanha o pacote.
- Pacote não é apenas pasta visual do VS Code.
- Não criar arquitetura exagerada; começar com separação coerente.

---

## 12. Imports desde cedo

### Mostrar em `AplicacaoBanco.java`

```java
package br.com.curso.banco;

import br.com.curso.banco.dominio.Conta;

public class AplicacaoBanco {

    public static void main(String[] args) {
        Conta conta = new Conta();
        System.out.println("Aplicação bancária iniciada.");
    }
}
```

### Explicar

- `AplicacaoBanco` e `Conta` estão em pacotes diferentes.
- `import` associa o nome simples `Conta` ao nome completo.
- Sem import:

```java
br.com.curso.banco.dominio.Conta conta =
        new br.com.curso.banco.dominio.Conta();
```

### O que o `import` não faz

- não copia a classe;
- não inclui texto no arquivo;
- não baixa dependência;
- não cria objeto;
- não executa código.

### Comparar com `COPY`

- `COPY` insere conteúdo de copybook durante preparação/compilação.
- `import` resolve nome de classe.
- A intenção ampla de organização/reutilização se aproxima.
- O mecanismo é diferente.

### Quando não precisa

- classes do mesmo pacote;
- classes de `java.lang`, como:
  - `String`;
  - `System`.

### Quando normalmente precisa

```java
import java.math.BigDecimal;
import java.time.LocalDate;
```

---

## 13. Demonstração completa no VS Code

### Etapa 1 — mostrar o projeto vindo do Tópico 1

```text
gestao-banco
└── src\br\com\curso\banco\AplicacaoBanco.java
```

### Etapa 2 — criar o pacote de domínio

```text
src\br\com\curso\banco\dominio
```

### Etapa 3 — criar `Cliente.java`

```java
package br.com.curso.banco.dominio;

public class Cliente {
    String cpf;
    String nome;
}
```

### Etapa 4 — criar `Conta.java`

```java
package br.com.curso.banco.dominio;

import java.math.BigDecimal;

public class Conta {
    String numero;
    String agencia;
    boolean ativa;
    BigDecimal saldo;
    Cliente titular;
}
```

### Pontos para pausar

- `String`: texto, mas sem limite automático de `PIC X`.
- `boolean`: dois estados.
- `BigDecimal`: tipo por referência e import.
- `Cliente`: classe do mesmo pacote usada como tipo, sem import.

### Etapa 5 — criar `Transacao.java`

```java
package br.com.curso.banco.dominio;

import java.math.BigDecimal;
import java.time.LocalDate;

public class Transacao {
    Conta contaOrigem;
    Conta contaDestino;
    BigDecimal valor;
    LocalDate data;
}
```

### Pontos para pausar

- `Conta` está no mesmo pacote: não precisa de import.
- `BigDecimal` e `LocalDate` estão em outros pacotes.
- Uma transação referencia objetos `Conta`.
- Não duplicar todos os atributos das contas na transação.

### Etapa 6 — atualizar `AplicacaoBanco.java`

```java
package br.com.curso.banco;

import br.com.curso.banco.dominio.Cliente;
import br.com.curso.banco.dominio.Conta;

public class AplicacaoBanco {

    public static void main(String[] args) {
        Cliente cliente = new Cliente();
        Conta conta = new Conta();

        System.out.println("Aplicação bancária organizada por domínio.");
    }
}
```

### Explicar sem avançar o conteúdo

- `Cliente` e `Conta`: classes e tipos.
- `cliente` e `conta`: referências.
- `new Cliente()` e `new Conta()`: criação de objetos.
- Imports necessários porque as classes estão em `dominio`.
- Não atribuir valores ainda.
- Não criar regra de conta ainda.

---

## 14. Compilar e executar a estrutura

### Criar `out`, se necessário

```bat
mkdir out
```

### Compilar

```bat
javac -d out src\br\com\curso\banco\dominio\Cliente.java src\br\com\curso\banco\dominio\Conta.java src\br\com\curso\banco\dominio\Transacao.java src\br\com\curso\banco\AplicacaoBanco.java
```

### Executar

```bat
java -cp out br.com.curso.banco.AplicacaoBanco
```

### Resultado esperado

```text
Aplicação bancária organizada por domínio.
```

### Pontos para comentar

- Ainda não há operação bancária.
- O ganho deste tópico é estrutural.
- O projeto contém classes com responsabilidades diferentes.
- Tipos expressam o significado inicial dos atributos.
- Objetos foram criados.
- Pacotes e imports conectam a estrutura.

---

## 15. Quadro de síntese COBOL → Java

| Experiência conhecida | Ponte em Java | Limite da comparação |
|---|---|---|
| grupo nível 01 | classe com atributos | classe também pode possuir comportamento |
| ocorrência da área de dados | objeto | objeto possui identidade e referências |
| `PIC X(n)` | `String` | `String` não recebe limite fixo |
| `PIC 9(n)` | `int`/`long` | dígitos decimais × faixas binárias |
| campo + nível 88 | `boolean` | nível 88 nomeia condição; `boolean` é tipo |
| decimal/COMP-3 | `BigDecimal` | representações e operações diferentes |
| data em `PIC 9(8)` | `LocalDate` | data é tipo com semântica própria |
| copybook | organização/reuso de definições | `import` não copia conteúdo |
| parágrafos e fluxo | instruções em métodos | métodos pertencem a classes/objetos |

### Pergunta de verificação

- “Qual dessas linhas é uma equivalência exata?”
- Resposta esperada: nenhuma; são pontes conceituais com diferenças importantes.

---

## 16. Erros conceituais para antecipar

### “Classe e objeto são sinônimos”

- Classe: definição.
- Objeto: ocorrência.
- Referência: forma de chegar ao objeto.

### “Todo campo numérico COBOL vira `int`”

- Identificadores podem ser `String`.
- Inteiros dependem de alcance.
- Dinheiro pede tipo decimal adequado.
- Datas não devem ser tratadas apenas como números.

### “`String` já respeita o tamanho do antigo `PIC X`”

- Não respeita.
- Tamanho de negócio será validado depois.

### “`boolean` guarda `S` e `N`”

- Não guarda.
- Guarda `true` ou `false`.

### “`import` baixa ou copia a classe”

- Não.
- Resolve o nome simples.

### “Criar uma classe para cada substantivo é OO”

- Não.
- Classe precisa de significado e responsabilidade coerentes.

### “OO elimina sequência”

- Não.
- Processos continuam ordenados.
- A estrutura distribui o conhecimento entre objetos.

---

## 17. Encerramento

### Recapitular

- COBOL é uma base de experiência, não um peso.
- Java não deve ser usado como tradução mecânica.
- Classe define um conceito.
- Atributo descreve estado.
- Tipo comunica natureza e limita possibilidades.
- Tipos Java não são traduções exatas de cláusulas `PIC`.
- Objeto é ocorrência concreta.
- Variável de classe guarda referência.
- Responsabilidades organizam o domínio.
- Pacotes agrupam e identificam classes.
- Imports permitem usar nomes simples de outros pacotes.

### Mensagens finais sugeridas

- “Você continuará pensando em processos; agora também pensará em participantes.”
- “O domínio bancário já não está todo dentro de `AplicacaoBanco`.”
- “Ainda não ensinamos regras. Construímos o lugar correto onde elas poderão existir.”
- “Essa base parece simples porque é fundamental, não porque seja superficial.”

---

## Checklist de gravação

- [ ] Mostrar dados e fluxo em COBOL.
- [ ] Explicar sem caricaturar o procedural.
- [ ] Contrastar fluxo e responsabilidade.
- [ ] Criar `Cliente`, `Conta` e `Transacao`.
- [ ] Diferenciar classe, objeto e referência.
- [ ] Comparar `PIC X(n)` e `String`.
- [ ] Comparar `PIC 9(n)`, `int` e `long`.
- [ ] Comparar nível 88 e `boolean`.
- [ ] Explicar `BigDecimal` para valores monetários.
- [ ] Explicar `LocalDate` para datas.
- [ ] Apresentar todos os tipos primitivos.
- [ ] Diferenciar tipos primitivos e tipos por referência.
- [ ] Explicar valores iniciais e `null`.
- [ ] Mostrar que tipo não substitui regra de negócio.
- [ ] Criar o pacote `dominio`.
- [ ] Explicar nome completo de classe.
- [ ] Explicar o que `import` faz.
- [ ] Diferenciar `import` de `COPY`.
- [ ] Compilar todas as classes.
- [ ] Executar `AplicacaoBanco`.
- [ ] Encerrar antes de decisões, repetições e regras bancárias.
