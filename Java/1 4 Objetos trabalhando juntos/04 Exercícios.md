# Exercícios — Tópico 4: Fazer objetos trabalharem juntos

## Continuação final do projeto prático: Gestor de Tarefas

Nos três tópicos anteriores, o projeto evoluiu em etapas:

1. o ambiente Java foi preparado e a primeira aplicação foi executada;
2. `Tarefa` e `Responsavel` passaram a representar conceitos do domínio;
3. os objetos receberam métodos, parâmetros, retornos, encapsulamento, condições e repetições.

Agora o Gestor de Tarefas será concluído com objetos trabalhando em conjunto. Uma nova classe administrará várias tarefas, tarefas diferentes compartilharão uma base comum e a aplicação processará esses objetos por meio de polimorfismo.

Ao longo desta sequência, você irá:

- revisar o relacionamento existente entre `Tarefa` e `Responsavel`;
- criar uma coleção de tarefas;
- adicionar, contar, percorrer e buscar objetos;
- tratar uma busca sem resultado;
- encapsular a coleção;
- transformar `Tarefa` em uma classe abstrata;
- criar `TarefaComum` e `TarefaUrgente`;
- sobrescrever comportamentos;
- reunir tipos diferentes em `List<Tarefa>`;
- processar os objetos polimorficamente;
- criar uma interface, se essa parte tiver sido apresentada na aula;
- consolidar a passagem do pensamento procedural para a orientação a objetos.

Esta etapa não usará banco de dados, frameworks, entrada pelo teclado, streams, construtores personalizados, exceções ou testes automatizados.

> Continue trabalhando no **computador físico**, não na VDI. Se houver erro, procure o instrutor e mostre o código e a mensagem completa do terminal.

---

## Ponto de partida

Use a pasta concluída no Tópico 3:

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

Ao final desta sequência, a estrutura principal será:

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
                        ├── GestorTarefas.java
                        ├── Priorizavel.java
                        ├── Responsavel.java
                        ├── Tarefa.java
                        ├── TarefaComum.java
                        └── TarefaUrgente.java
```

> `Priorizavel.java` será opcional. Se interfaces não forem trabalhadas por falta de tempo, mantenha o método abstrato diretamente em `Tarefa` e não crie esse arquivo.

---

## Exercício 50 — Confirmar a versão do Tópico 3

Antes de alterar o projeto, compile a versão atual:

```bat
javac -d out src\br\com\curso\tarefas\dominio\*.java src\br\com\curso\tarefas\AplicacaoGestorTarefas.java
```

Execute:

```bat
java -cp out br.com.curso.tarefas.AplicacaoGestorTarefas
```

Confirme que:

- o responsável é cadastrado;
- as tarefas são cadastradas;
- os atributos estão privados;
- métodos controlam início, registro de horas e conclusão;
- as três tarefas aparecem como concluídas ao final.

Se essa versão não funcionar, corrija-a antes de acrescentar novos arquivos. Um erro anterior não deve ser confundido com as alterações deste tópico.

---

## Exercício 51 — Identificar os relacionamentos do projeto

Abra `modelo-inicial.txt` e acrescente:

```text
Relacionamentos do Tópico 4

Tarefa tem um Responsavel.
GestorTarefas possui várias Tarefa.
TarefaComum é uma Tarefa.
TarefaUrgente é uma Tarefa.
```

Classifique cada afirmação:

| Afirmação | Tipo de relação |
|---|---|
| `Tarefa` tem um `Responsavel` | relacionamento por atributo |
| `GestorTarefas` possui várias tarefas | relacionamento por coleção |
| `TarefaComum` é uma `Tarefa` | herança |
| `TarefaUrgente` é uma `Tarefa` | herança |

Não escreva:

```text
GestorTarefas é uma Tarefa.
Responsavel é uma Tarefa.
```

Essas afirmações não descrevem especialização. O gestor possui tarefas, enquanto o responsável se relaciona com elas.

---

## Exercício 52 — Criar a classe `GestorTarefas`

Na pasta `dominio`, crie:

```text
GestorTarefas.java
```

Escreva:

```java
package br.com.curso.tarefas.dominio;

import java.util.ArrayList;
import java.util.List;

public class GestorTarefas {

    private List<Tarefa> tarefas = new ArrayList<>();
}
```

### Leia a declaração

| Trecho | Significado |
|---|---|
| `List<Tarefa>` | lista que aceita referências para tarefas |
| `tarefas` | nome do atributo |
| `new ArrayList<>()` | objeto concreto usado como lista |

`List` e `ArrayList` pertencem ao pacote `java.util`, por isso foram importados.

### Ponte com COBOL

Uma tabela declarada com `OCCURS` também reúne várias ocorrências. Porém, `List<Tarefa>` não é apenas um `OCCURS` sem limite:

- a lista é um objeto;
- possui métodos como `add`, `get` e `size`;
- seus elementos são referências para objetos `Tarefa`;
- o tamanho pode crescer durante a execução;
- cada tarefa preserva sua própria identidade e comportamento.

Compile somente o pacote de domínio:

```bat
javac -d out src\br\com\curso\tarefas\dominio\*.java
```

---

## Exercício 53 — Adicionar tarefas e consultar a quantidade

Acrescente dois métodos em `GestorTarefas`:

```java
public void adicionarTarefa(Tarefa tarefa) {
    tarefas.add(tarefa);
}

public int obterQuantidadeTarefas() {
    return tarefas.size();
}
```

### Interprete

- `add` inclui a referência na coleção;
- `size` devolve a quantidade de elementos;
- a coleção continua privada;
- a aplicação pede ao gestor para adicionar a tarefa;
- o gestor controla sua coleção.

Não crie um método que devolva a lista apenas para permitir alterações externas:

```java
public List<Tarefa> obterTarefas() {
    return tarefas;
}
```

Nesta etapa, serão oferecidas operações específicas: adicionar, listar, buscar e processar.

---

## Exercício 54 — Colocar as tarefas existentes na coleção

Em `AplicacaoGestorTarefas.java`, importe:

```java
import br.com.curso.tarefas.dominio.GestorTarefas;
```

Depois do cadastro das três tarefas, crie o gestor:

```java
GestorTarefas gestor = new GestorTarefas();

gestor.adicionarTarefa(tarefaRevisarRequisitos);
gestor.adicionarTarefa(tarefaImplementarTela);
gestor.adicionarTarefa(tarefaValidarAplicacao);
```

Exiba:

```java
System.out.println(
        "Quantidade de tarefas: " + gestor.obterQuantidadeTarefas()
);
```

Resultado esperado:

```text
Quantidade de tarefas: 3
```

As variáveis da aplicação continuam apontando para os mesmos objetos que foram adicionados à lista. `add` não cria uma cópia completa de cada tarefa.

---

## Exercício 55 — Percorrer a coleção com `for` aprimorado

Acrescente em `GestorTarefas`:

```java
public void exibirResumo() {
    for (Tarefa tarefa : tarefas) {
        System.out.println(
                tarefa.obterCodigo()
                + " - "
                + tarefa.obterTitulo()
                + " - "
                + tarefa.obterSituacao()
        );
    }
}
```

Leia o cabeçalho:

```java
for (Tarefa tarefa : tarefas)
```

como:

> Para cada `Tarefa` da coleção `tarefas`, use a referência atual na variável `tarefa`.

Na aplicação, chame:

```java
gestor.exibirResumo();
```

### Ponte com COBOL

Uma tabela poderia ser percorrida com `PERFORM VARYING` e um índice. O `for` aprimorado esconde a posição quando ela não é importante. O objetivo aqui é visitar cada objeto.

---

## Exercício 56 — Buscar uma tarefa por código

Acrescente:

```java
public Tarefa buscarPorCodigo(String codigo) {
    for (Tarefa tarefa : tarefas) {
        if (tarefa.possuiCodigo(codigo)) {
            return tarefa;
        }
    }

    return null;
}
```

O método:

1. percorre a coleção;
2. pede a cada tarefa que compare o próprio código;
3. retorna a primeira tarefa encontrada;
4. retorna `null` se nenhuma corresponder.

Teste na aplicação:

```java
Tarefa encontrada = gestor.buscarPorCodigo("TAR-002");

if (encontrada != null) {
    System.out.println("Encontrada: " + encontrada.obterTitulo());
}
```

Resultado esperado:

```text
Encontrada: Implementar a tela inicial
```

---

## Exercício 57 — Tratar uma busca sem resultado

Agora procure:

```java
Tarefa inexistente = gestor.buscarPorCodigo("TAR-999");
```

Verifique:

```java
if (inexistente == null) {
    System.out.println("Tarefa TAR-999 não encontrada.");
}
```

`null` representa ausência de referência. Não é uma tarefa vazia, não é uma `String` vazia e não é o número zero.

Não faça:

```java
gestor.buscarPorCodigo("TAR-999").obterTitulo();
```

Como não há objeto retornado, tentar chamar `obterTitulo()` causaria erro durante a execução.

---

## Exercício 58 — Listar tarefas de um responsável

Primeiro, acrescente em `Tarefa`:

```java
public boolean possuiResponsavel(String matricula) {
    return responsavel.obterMatricula().equals(matricula);
}
```

Depois, em `GestorTarefas`:

```java
public void exibirTarefasDoResponsavel(String matricula) {
    for (Tarefa tarefa : tarefas) {
        if (tarefa.possuiResponsavel(matricula)) {
            System.out.println(tarefa.obterTitulo());
        }
    }
}
```

Teste:

```java
gestor.exibirTarefasDoResponsavel("F12345");
```

### Observe a colaboração

- `GestorTarefas` percorre a coleção;
- `Tarefa` sabe qual responsável possui;
- `Responsavel` fornece a matrícula;
- nenhuma classe acessa diretamente atributos privados das outras.

---

## Exercício 59 — Planejar a especialização das tarefas

Antes de alterar `Tarefa`, registre em `modelo-inicial.txt`:

```text
Hierarquia do Tópico 4

Tarefa
|- TarefaComum
|- TarefaUrgente

Comportamentos comuns:
- cadastrar dados básicos
- iniciar
- registrar hora trabalhada
- concluir
- consultar código, título e situação

Comportamentos que variam:
- calcular prioridade
- informar o tipo da tarefa
```

### Confirme a relação

- uma `TarefaComum` é uma `Tarefa`;
- uma `TarefaUrgente` é uma `Tarefa`;
- ambas podem ocupar um lugar em `List<Tarefa>`;
- cada uma responderá à prioridade de forma diferente.

Não use herança para dizer que `Responsavel` é uma tarefa ou que `GestorTarefas` é uma tarefa.

---

## Exercício 60 — Transformar `Tarefa` em classe abstrata

Altere a declaração:

```java
public abstract class Tarefa {
```

Renomeie o método `cadastrar` para `cadastrarTarefa` e torne-o `protected`:

```java
protected void cadastrarTarefa(
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

Acrescente no final da classe:

```java
public abstract int calcularPrioridade();

public abstract String obterTipo();
```

### O que mudou

- `Tarefa` continua possuindo estado e métodos comuns;
- não será mais criada diretamente com `new Tarefa()`;
- subclasses concretas usarão `cadastrarTarefa`;
- cada subclasse precisa implementar prioridade e tipo.

Neste momento, `AplicacaoGestorTarefas` deixará de compilar porque ainda usa:

```java
new Tarefa()
```

Esse erro é esperado. Ele será resolvido ao criar as subclasses.

---

## Exercício 61 — Acrescentar uma consulta de pendência

Antes de criar as subclasses, acrescente em `Tarefa`:

```java
public boolean estaPendente() {
    return !emAndamento && !concluida;
}
```

Esse método permitirá que o gestor solicite o início apenas de tarefas pendentes sem precisar interpretar o texto devolvido por `obterSituacao()`.

Compare:

```java
"PENDENTE".equals(tarefa.obterSituacao())
```

com:

```java
tarefa.estaPendente()
```

A segunda forma comunica diretamente a pergunta de negócio.

---

## Exercício 62 — Criar `TarefaComum`

Na pasta `dominio`, crie:

```text
TarefaComum.java
```

Escreva:

```java
package br.com.curso.tarefas.dominio;

import java.math.BigDecimal;
import java.time.LocalDate;

public class TarefaComum extends Tarefa {

    public void cadastrarTarefaComum(
            String codigo,
            String titulo,
            String descricao,
            int estimativaHoras,
            LocalDate dataLimite,
            BigDecimal custoEstimado,
            Responsavel responsavel) {

        cadastrarTarefa(
                codigo,
                titulo,
                descricao,
                estimativaHoras,
                dataLimite,
                custoEstimado,
                responsavel
        );
    }

    @Override
    public int calcularPrioridade() {
        return 1;
    }

    @Override
    public String obterTipo() {
        return "COMUM";
    }
}
```

### Interprete

- `extends Tarefa`: tarefa comum é uma tarefa;
- `cadastrarTarefa`: método protegido fornecido pela classe base;
- `@Override`: indica sobrescrita;
- prioridade `1`: regra didática usada no projeto;
- tipo `COMUM`: descrição específica.

---

## Exercício 63 — Criar `TarefaUrgente`

Crie:

```text
TarefaUrgente.java
```

Escreva:

```java
package br.com.curso.tarefas.dominio;

import java.math.BigDecimal;
import java.time.LocalDate;

public class TarefaUrgente extends Tarefa {

    private String motivoUrgencia;

    public void cadastrarTarefaUrgente(
            String codigo,
            String titulo,
            String descricao,
            int estimativaHoras,
            LocalDate dataLimite,
            BigDecimal custoEstimado,
            Responsavel responsavel,
            String motivoUrgencia) {

        cadastrarTarefa(
                codigo,
                titulo,
                descricao,
                estimativaHoras,
                dataLimite,
                custoEstimado,
                responsavel
        );

        this.motivoUrgencia = motivoUrgencia;
    }

    @Override
    public int calcularPrioridade() {
        return 3;
    }

    @Override
    public String obterTipo() {
        return "URGENTE";
    }

    public String obterMotivoUrgencia() {
        return motivoUrgencia;
    }
}
```

`TarefaUrgente` compartilha a estrutura e os comportamentos gerais de `Tarefa`, mas acrescenta `motivoUrgencia` e sobrescreve prioridade e tipo.

---

## Exercício 64 — Atualizar a criação dos objetos

Em `AplicacaoGestorTarefas`, substitua o import de `Tarefa` para criação de objetos por:

```java
import br.com.curso.tarefas.dominio.Tarefa;
import br.com.curso.tarefas.dominio.TarefaComum;
import br.com.curso.tarefas.dominio.TarefaUrgente;
```

Crie duas tarefas comuns:

```java
TarefaComum tarefaRevisarRequisitos = new TarefaComum();
tarefaRevisarRequisitos.cadastrarTarefaComum(
        "TAR-001",
        "Revisar requisitos",
        "Conferir os requisitos do projeto",
        3,
        LocalDate.of(2026, 10, 10),
        new BigDecimal("150.00"),
        responsavel
);

TarefaComum tarefaImplementarTela = new TarefaComum();
tarefaImplementarTela.cadastrarTarefaComum(
        "TAR-002",
        "Implementar a tela inicial",
        "Criar a estrutura visual inicial",
        2,
        LocalDate.of(2026, 10, 12),
        new BigDecimal("300.00"),
        responsavel
);
```

Crie uma tarefa urgente:

```java
TarefaUrgente tarefaValidarAplicacao = new TarefaUrgente();
tarefaValidarAplicacao.cadastrarTarefaUrgente(
        "TAR-003",
        "Corrigir falha de validação",
        "Corrigir a falha antes da publicação",
        1,
        LocalDate.of(2026, 10, 8),
        new BigDecimal("500.00"),
        responsavel,
        "Publicação bloqueada"
);
```

As três variáveis possuem tipos concretos, mas todos os objetos também são tarefas.

---

## Exercício 65 — Reunir subclasses na mesma coleção

O método abaixo já aceita as três referências:

```java
gestor.adicionarTarefa(tarefaRevisarRequisitos);
gestor.adicionarTarefa(tarefaImplementarTela);
gestor.adicionarTarefa(tarefaValidarAplicacao);
```

Isso funciona porque o parâmetro é:

```java
Tarefa tarefa
```

e `TarefaComum` e `TarefaUrgente` estendem `Tarefa`.

O atributo continua:

```java
private List<Tarefa> tarefas = new ArrayList<>();
```

A lista pode reunir objetos concretos diferentes por meio do tipo geral.

---

## Exercício 66 — Exibir tipo e prioridade polimorficamente

Atualize `exibirResumo()`:

```java
public void exibirResumo() {
    for (Tarefa tarefa : tarefas) {
        System.out.println(
                tarefa.obterCodigo()
                + " - "
                + tarefa.obterTitulo()
                + " - tipo: "
                + tarefa.obterTipo()
                + " - prioridade: "
                + tarefa.calcularPrioridade()
                + " - situação: "
                + tarefa.obterSituacao()
        );
    }
}
```

Não foi necessário escrever:

```java
if (tarefa é comum) {
    prioridade = 1;
} else if (tarefa é urgente) {
    prioridade = 3;
}
```

O gestor chama:

```java
tarefa.calcularPrioridade()
```

Cada objeto executa sua implementação. Isso é polimorfismo.

### Ponte com COBOL

Um programa procedural poderia guardar um código de tipo e usar `EVALUATE`. Essa organização pode ser adequada em vários contextos. Aqui, porém, a regra que varia foi colocada na classe específica. O gestor não precisa conhecer cada tipo para perguntar a prioridade.

---

## Exercício 67 — Buscar a tarefa de maior prioridade

Acrescente em `GestorTarefas`:

```java
public Tarefa buscarMaiorPrioridade() {
    if (tarefas.size() == 0) {
        return null;
    }

    Tarefa maiorPrioridade = tarefas.get(0);

    for (Tarefa tarefa : tarefas) {
        if (tarefa.calcularPrioridade()
                > maiorPrioridade.calcularPrioridade()) {
            maiorPrioridade = tarefa;
        }
    }

    return maiorPrioridade;
}
```

### Leia o algoritmo

1. Se a lista estiver vazia, não há tarefa para devolver.
2. A primeira tarefa começa como maior prioridade conhecida.
3. Cada tarefa é comparada com a atual.
4. A referência é substituída quando uma prioridade maior aparece.
5. O objeto selecionado é retornado.

Na aplicação:

```java
Tarefa prioritaria = gestor.buscarMaiorPrioridade();

if (prioritaria != null) {
    System.out.println(
            "Próxima tarefa: " + prioritaria.obterTitulo()
    );
}
```

Resultado esperado:

```text
Próxima tarefa: Corrigir falha de validação
```

---

## Exercício 68 — Iniciar todas as tarefas pendentes

Acrescente:

```java
public void iniciarTarefasPendentes() {
    for (Tarefa tarefa : tarefas) {
        if (tarefa.estaPendente()) {
            tarefa.iniciar();
        }
    }
}
```

Chame:

```java
gestor.iniciarTarefasPendentes();
gestor.exibirResumo();
```

Todas as tarefas pendentes deverão aparecer como `EM ANDAMENTO`, desde que o responsável permaneça ativo.

Observe que o gestor coordena a repetição, enquanto cada tarefa protege a própria regra de início.

---

## Exercício 69 — Interface `Priorizavel` — parte opcional

Faça este exercício somente se interfaces tiverem sido apresentadas.

Crie:

```text
Priorizavel.java
```

Escreva:

```java
package br.com.curso.tarefas.dominio;

public interface Priorizavel {

    int calcularPrioridade();
}
```

Altere a declaração de `Tarefa`:

```java
public abstract class Tarefa implements Priorizavel {
```

Mantenha em `Tarefa`:

```java
@Override
public abstract int calcularPrioridade();
```

### Interprete

- `Priorizavel` declara o contrato;
- qualquer classe que implemente a interface deve oferecer `calcularPrioridade`;
- `Tarefa` assume o contrato;
- as subclasses concretas fornecem a implementação;
- a interface representa a capacidade de ser priorizada;
- a classe abstrata continua concentrando estado e comportamento comuns das tarefas.

Não confunda a interface com herança de implementação. `Priorizavel` não guarda código, título, horas ou responsável.

---

## Exercício 70 — Executar o fluxo consolidado

Organize o `main` nesta ordem:

1. cadastrar o responsável;
2. criar duas tarefas comuns;
3. criar uma tarefa urgente;
4. criar `GestorTarefas`;
5. adicionar as três tarefas;
6. exibir a quantidade;
7. exibir o resumo inicial;
8. buscar uma tarefa por código;
9. buscar código inexistente;
10. buscar a maior prioridade;
11. iniciar todas as pendentes;
12. exibir o resumo final.

Saída aproximada:

```text
=== GESTOR DE TAREFAS ===
Quantidade de tarefas: 3

TAR-001 - Revisar requisitos - tipo: COMUM - prioridade: 1 - situação: PENDENTE
TAR-002 - Implementar a tela inicial - tipo: COMUM - prioridade: 1 - situação: PENDENTE
TAR-003 - Corrigir falha de validação - tipo: URGENTE - prioridade: 3 - situação: PENDENTE

Encontrada: Implementar a tela inicial
Tarefa TAR-999 não encontrada.
Próxima tarefa: Corrigir falha de validação

TAR-001 - Revisar requisitos - tipo: COMUM - prioridade: 1 - situação: EM ANDAMENTO
TAR-002 - Implementar a tela inicial - tipo: COMUM - prioridade: 1 - situação: EM ANDAMENTO
TAR-003 - Corrigir falha de validação - tipo: URGENTE - prioridade: 3 - situação: EM ANDAMENTO
```

Compile:

```bat
javac -d out src\br\com\curso\tarefas\dominio\*.java src\br\com\curso\tarefas\AplicacaoGestorTarefas.java
```

Execute:

```bat
java -cp out br.com.curso.tarefas.AplicacaoGestorTarefas
```

Execute também pelo botão **Run** do VS Code.

---

## Exercício 71 — Conferência final do projeto

### Relacionamentos

- [ ] `Tarefa` possui uma referência para `Responsavel`.
- [ ] `GestorTarefas` possui uma coleção de tarefas.
- [ ] Os dados do responsável não foram copiados para vários atributos em `Tarefa`.
- [ ] As classes colaboram por métodos públicos.

### Coleções

- [ ] `GestorTarefas` importa `List` e `ArrayList`.
- [ ] A coleção é `private`.
- [ ] O tipo da coleção é `List<Tarefa>`.
- [ ] Tarefas são adicionadas com um método do gestor.
- [ ] A quantidade é consultada com `size()`.
- [ ] O percurso usa `for` aprimorado.
- [ ] A busca retorna a tarefa encontrada ou `null`.
- [ ] A aplicação verifica `null` antes de chamar métodos.

### Herança

- [ ] `Tarefa` é abstrata.
- [ ] `TarefaComum` usa `extends Tarefa`.
- [ ] `TarefaUrgente` usa `extends Tarefa`.
- [ ] O estado comum permanece em `Tarefa`.
- [ ] O cadastro comum é reutilizado pelas subclasses.
- [ ] `TarefaUrgente` acrescenta o motivo da urgência.
- [ ] A hierarquia representa uma relação “é uma”.

### Polimorfismo

- [ ] `Tarefa` declara operações que variam.
- [ ] As subclasses usam `@Override`.
- [ ] Tarefa comum devolve prioridade `1`.
- [ ] Tarefa urgente devolve prioridade `3`.
- [ ] Os dois tipos são adicionados a `List<Tarefa>`.
- [ ] O gestor chama os mesmos métodos em todos os objetos.
- [ ] Não existe uma grande cadeia de condições verificando o tipo.

### Interface opcional

- [ ] `Priorizavel` foi criada somente se a seção foi estudada.
- [ ] A interface declara `calcularPrioridade`.
- [ ] `Tarefa` usa `implements Priorizavel`.
- [ ] A interface não foi confundida com a classe abstrata.

### Compilação e execução

- [ ] Todos os arquivos compilam.
- [ ] Os `.class` aparecem nos pacotes corretos em `out`.
- [ ] O programa executa pelo terminal.
- [ ] O programa executa pelo VS Code.
- [ ] A quantidade final é três.
- [ ] A tarefa urgente é escolhida como maior prioridade.
- [ ] Todas as tarefas pendentes são iniciadas.

---

## Entrega

Entregue a pasta inteira:

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
                        ├── GestorTarefas.java
                        ├── Priorizavel.java
                        ├── Responsavel.java
                        ├── Tarefa.java
                        ├── TarefaComum.java
                        └── TarefaUrgente.java
```

Se a interface não tiver sido trabalhada, retire `Priorizavel.java` da entrega, remova `implements Priorizavel` da declaração de `Tarefa` e retire o `@Override` colocado sobre a declaração de `calcularPrioridade()` nessa classe. O método continua existindo como método abstrato de `Tarefa`.

---

## Código final de referência

Use esta seção somente depois de concluir a sequência.

### `Priorizavel.java` — opcional

```java
package br.com.curso.tarefas.dominio;

public interface Priorizavel {

    int calcularPrioridade();
}
```

### `Responsavel.java`

```java
package br.com.curso.tarefas.dominio;

public class Responsavel {

    private String matricula;
    private String nome;
    private boolean ativo;

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
}
```

### `Tarefa.java`

```java
package br.com.curso.tarefas.dominio;

import java.math.BigDecimal;
import java.time.LocalDate;

public abstract class Tarefa implements Priorizavel {

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

    protected void cadastrarTarefa(
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

    public boolean iniciar() {
        if (concluida || emAndamento || !responsavel.estaAtivo()) {
            return false;
        }

        emAndamento = true;
        return true;
    }

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

    public boolean concluir() {
        if (!emAndamento || concluida) {
            return false;
        }

        concluida = true;
        emAndamento = false;
        return true;
    }

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

    public boolean estaPendente() {
        return !emAndamento && !concluida;
    }

    public String obterNomeResponsavel() {
        return responsavel.obterNome();
    }

    public boolean possuiResponsavel(String matricula) {
        return responsavel.obterMatricula().equals(matricula);
    }

    public boolean possuiCodigo(String codigoInformado) {
        return codigo.equals(codigoInformado);
    }

    public boolean possuiCustoAcimaDe(BigDecimal limite) {
        return custoEstimado.compareTo(limite) > 0;
    }

    public String obterSituacao() {
        if (concluida) {
            return "CONCLUÍDA";
        } else if (emAndamento) {
            return "EM ANDAMENTO";
        } else {
            return "PENDENTE";
        }
    }

    @Override
    public abstract int calcularPrioridade();

    public abstract String obterTipo();
}
```

### `TarefaComum.java`

```java
package br.com.curso.tarefas.dominio;

import java.math.BigDecimal;
import java.time.LocalDate;

public class TarefaComum extends Tarefa {

    public void cadastrarTarefaComum(
            String codigo,
            String titulo,
            String descricao,
            int estimativaHoras,
            LocalDate dataLimite,
            BigDecimal custoEstimado,
            Responsavel responsavel) {

        cadastrarTarefa(
                codigo,
                titulo,
                descricao,
                estimativaHoras,
                dataLimite,
                custoEstimado,
                responsavel
        );
    }

    @Override
    public int calcularPrioridade() {
        return 1;
    }

    @Override
    public String obterTipo() {
        return "COMUM";
    }
}
```

### `TarefaUrgente.java`

```java
package br.com.curso.tarefas.dominio;

import java.math.BigDecimal;
import java.time.LocalDate;

public class TarefaUrgente extends Tarefa {

    private String motivoUrgencia;

    public void cadastrarTarefaUrgente(
            String codigo,
            String titulo,
            String descricao,
            int estimativaHoras,
            LocalDate dataLimite,
            BigDecimal custoEstimado,
            Responsavel responsavel,
            String motivoUrgencia) {

        cadastrarTarefa(
                codigo,
                titulo,
                descricao,
                estimativaHoras,
                dataLimite,
                custoEstimado,
                responsavel
        );

        this.motivoUrgencia = motivoUrgencia;
    }

    @Override
    public int calcularPrioridade() {
        return 3;
    }

    @Override
    public String obterTipo() {
        return "URGENTE";
    }

    public String obterMotivoUrgencia() {
        return motivoUrgencia;
    }
}
```

### `GestorTarefas.java`

```java
package br.com.curso.tarefas.dominio;

import java.util.ArrayList;
import java.util.List;

public class GestorTarefas {

    private List<Tarefa> tarefas = new ArrayList<>();

    public void adicionarTarefa(Tarefa tarefa) {
        tarefas.add(tarefa);
    }

    public int obterQuantidadeTarefas() {
        return tarefas.size();
    }

    public Tarefa buscarPorCodigo(String codigo) {
        for (Tarefa tarefa : tarefas) {
            if (tarefa.possuiCodigo(codigo)) {
                return tarefa;
            }
        }

        return null;
    }

    public Tarefa buscarMaiorPrioridade() {
        if (tarefas.size() == 0) {
            return null;
        }

        Tarefa maiorPrioridade = tarefas.get(0);

        for (Tarefa tarefa : tarefas) {
            if (tarefa.calcularPrioridade()
                    > maiorPrioridade.calcularPrioridade()) {
                maiorPrioridade = tarefa;
            }
        }

        return maiorPrioridade;
    }

    public void iniciarTarefasPendentes() {
        for (Tarefa tarefa : tarefas) {
            if (tarefa.estaPendente()) {
                tarefa.iniciar();
            }
        }
    }

    public void exibirTarefasDoResponsavel(String matricula) {
        for (Tarefa tarefa : tarefas) {
            if (tarefa.possuiResponsavel(matricula)) {
                System.out.println(tarefa.obterTitulo());
            }
        }
    }

    public void exibirResumo() {
        for (Tarefa tarefa : tarefas) {
            System.out.println(
                    tarefa.obterCodigo()
                    + " - "
                    + tarefa.obterTitulo()
                    + " - tipo: "
                    + tarefa.obterTipo()
                    + " - prioridade: "
                    + tarefa.calcularPrioridade()
                    + " - situação: "
                    + tarefa.obterSituacao()
            );
        }
    }
}
```

### `AplicacaoGestorTarefas.java`

```java
package br.com.curso.tarefas;

import java.math.BigDecimal;
import java.time.LocalDate;

import br.com.curso.tarefas.dominio.GestorTarefas;
import br.com.curso.tarefas.dominio.Responsavel;
import br.com.curso.tarefas.dominio.Tarefa;
import br.com.curso.tarefas.dominio.TarefaComum;
import br.com.curso.tarefas.dominio.TarefaUrgente;

public class AplicacaoGestorTarefas {

    public static void main(String[] args) {
        Responsavel responsavel = new Responsavel();
        responsavel.cadastrar("F12345", "Marina Souza");

        TarefaComum tarefaRevisarRequisitos = new TarefaComum();
        tarefaRevisarRequisitos.cadastrarTarefaComum(
                "TAR-001",
                "Revisar requisitos",
                "Conferir os requisitos do projeto",
                3,
                LocalDate.of(2026, 10, 10),
                new BigDecimal("150.00"),
                responsavel
        );

        TarefaComum tarefaImplementarTela = new TarefaComum();
        tarefaImplementarTela.cadastrarTarefaComum(
                "TAR-002",
                "Implementar a tela inicial",
                "Criar a estrutura visual inicial",
                2,
                LocalDate.of(2026, 10, 12),
                new BigDecimal("300.00"),
                responsavel
        );

        TarefaUrgente tarefaValidarAplicacao = new TarefaUrgente();
        tarefaValidarAplicacao.cadastrarTarefaUrgente(
                "TAR-003",
                "Corrigir falha de validação",
                "Corrigir a falha antes da publicação",
                1,
                LocalDate.of(2026, 10, 8),
                new BigDecimal("500.00"),
                responsavel,
                "Publicação bloqueada"
        );

        GestorTarefas gestor = new GestorTarefas();
        gestor.adicionarTarefa(tarefaRevisarRequisitos);
        gestor.adicionarTarefa(tarefaImplementarTela);
        gestor.adicionarTarefa(tarefaValidarAplicacao);

        System.out.println("=== GESTOR DE TAREFAS ===");
        System.out.println(
                "Quantidade de tarefas: "
                + gestor.obterQuantidadeTarefas()
        );
        System.out.println();

        gestor.exibirResumo();
        System.out.println();

        Tarefa encontrada = gestor.buscarPorCodigo("TAR-002");

        if (encontrada != null) {
            System.out.println("Encontrada: " + encontrada.obterTitulo());
        }

        Tarefa inexistente = gestor.buscarPorCodigo("TAR-999");

        if (inexistente == null) {
            System.out.println("Tarefa TAR-999 não encontrada.");
        }

        Tarefa prioritaria = gestor.buscarMaiorPrioridade();

        if (prioritaria != null) {
            System.out.println(
                    "Próxima tarefa: " + prioritaria.obterTitulo()
            );
        }

        System.out.println();
        gestor.iniciarTarefasPendentes();
        gestor.exibirResumo();
    }
}
```

---

## Síntese final do projeto

No primeiro tópico, o Gestor de Tarefas apenas validava o ambiente e exibia mensagens. No segundo, passou a possuir classes, atributos, objetos e pacotes. No terceiro, os objetos receberam métodos e passaram a proteger regras. Neste último tópico, eles começaram a colaborar.

Agora:

- `Tarefa` se relaciona com `Responsavel`;
- `GestorTarefas` administra uma coleção;
- o gestor percorre e busca objetos;
- `TarefaComum` e `TarefaUrgente` especializam `Tarefa`;
- a lista aceita os dois tipos pelo tipo geral;
- a mesma chamada produz respostas específicas;
- a interface opcional registra a capacidade de calcular prioridade.

O pensamento procedural continua útil dentro dos métodos: ainda existem sequência, condições, repetições e busca. A mudança está na organização. O fluxo deixa de carregar sozinho todos os detalhes e passa a coordenar objetos que conhecem e protegem suas próprias responsabilidades.
