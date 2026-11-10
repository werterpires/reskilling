# Exercícios — Tópico 4: Colaboração, Maven e aplicação bancária em trios

## Projeto prático em trio: Sistema Bancário

Esta é a folha de tarefas da atividade em trios. Cada trio construirá, em um repositório novo e compartilhado no GitHub, uma aplicação bancária de console. O processo importa mais que o código: três pessoas combinam um contrato, trabalham em branches separadas, acompanham o trabalho umas das outras com `fetch` e `pull`, revisam-se mutuamente e integram tudo na `main`, pelo botão do Pull Request ou com um merge local.

Ao longo desta sequência, vocês irão:

- criar o repositório, convidar os colaboradores e preparar o projeto Maven;
- combinar o contrato entre `Cliente`, `Conta` e `AplicacaoBanco` e integrar um esqueleto das classes;
- desenvolver em paralelo, uma branch por integrante;
- acompanhar as branches dos colegas com `git fetch`, sem misturar com o próprio trabalho;
- abrir Pull Requests, revisar e integrar, inclusive com merge local;
- executar o cenário de teste, corrigir pelo mesmo fluxo e documentar a execução.

Os dados ficam em memória. Não haverá banco de dados, autenticação, interface gráfica, dependências, testes automatizados nem JAR: isso fica para uma etapa posterior do curso.

> Trabalhe no **computador físico**, não na VDI. Se algum comando não apresentar o resultado esperado, pare e procure o instrutor com a mensagem **completa** exibida no terminal.

> Use apenas **dados fictícios**. O repositório fica no GitHub: não digite CPFs reais, não publique código da empresa e não grave senhas ou tokens em nenhum arquivo.

Os comandos usam o **Git Bash**, com barras normais (`/`). Os botões do GitHub aparecem em inglês, como na interface padrão.

---

## Papéis e regras do trio

| Papel | Arquivo | Branches | Também faz |
|---|---|---|---|
| Integrante 1 | `Cliente.java` | `feature/esqueleto`, `feature/cliente`, `feature/transferencia` | cria o repositório e a transferência |
| Integrante 2 | `Conta.java` | `feature/conta`, `docs/readme-execucao` | escreve o `README.md` de execução |
| Integrante 3 | `AplicacaoBanco.java` | `feature/aplicacao-banco` | cuida do menu e da integração |

Regras do começo ao fim:

1. Só há um commit direto na `main`: a configuração inicial, no Exercício 51.
2. Cada integrante altera apenas o arquivo da sua responsabilidade, o que evita conflitos artificiais.
3. Toda Pull Request (PR) precisa de revisão de outra pessoa. Ninguém aprova o próprio trabalho.
4. Mudar um nome ou parâmetro do contrato exige avisar o trio **antes**.
5. Commits pequenos, com mensagem descritiva. Evite `teste`, `final` e `alterações`.
6. A regra de ouro do Tópico 3 vale para o trio: **nunca** use `reset`, `rebase` ou `push --force` na `main` nem na branch de um colega. Na sua própria branch, só reescreva o histórico antes de pedir revisão.

Cada exercício informa **Quem faz**. Os Exercícios 55, 56 e 57 acontecem ao mesmo tempo, um por integrante.

---

## Ponto de partida

Você precisa do que construiu nos tópicos anteriores: Git configurado, conta no GitHub com autenticação por HTTPS funcionando e Java 21. Ao final, o repositório terá esta estrutura (`target/` é gerada pelo Maven e fica ignorada):

```text
sistema-bancario/
├── .gitignore
├── README.md
├── pom.xml
└── src/
    └── main/
        └── java/
            └── br/
                └── com/
                    └── banco/
                        ├── AplicacaoBanco.java
                        ├── Cliente.java
                        └── Conta.java
```

---

## Exercício 49 — Formar o trio e conferir o ambiente

**Quem faz:** os três.

Definam quem é o Integrante 1, 2 e 3 e anotem o usuário do GitHub de cada um. Depois, cada pessoa executa:

```bash
git config user.name
git config user.email
java -version
javac -version
mvn -version
```

Resultado esperado: o Git mostra o seu nome e e-mail; `java` e `javac` indicam a versão `21`; `mvn -version` começa com `Apache Maven`, seguido da versão, e inclui uma linha `Java version:` com o Java 21. Se `mvn` não for reconhecido, procure o instrutor com a mensagem do terminal e não instale nada por conta própria.

O Maven padroniza a estrutura do projeto e executa tarefas como a compilação. Neste tópico você usa apenas `mvn compile`.

---

## Exercício 50 — Criar o repositório e convidar os colaboradores

**Quem faz:** Integrante 1 cria e convida. Integrantes 2 e 3 aceitam.

**Integrante 1.** No GitHub, escolha **New repository**, com nome `sistema-bancario`, visibilidade **Private** e **Add a README file** marcado (sem `.gitignore` nem licença). Depois acesse **Settings → Collaborators → Add people** e convide o usuário de cada colega. Sem o convite, os colegas não conseguem enviar branches nem abrir PRs.

**Integrantes 2 e 3.** Aceitem o convite (**Accept invitation**) pelo e-mail ou pelas notificações do GitHub.

Conferência: nos três navegadores, `https://github.com/<usuario-do-integrante-1>/sistema-bancario` abre o repositório. Um repositório privado responde `404` a quem não é colaborador: o convite não foi aceito ou o usuário foi digitado errado.

---

## Exercício 51 — Preparar o `pom.xml` e o `.gitignore`

**Quem faz:** Integrante 1. Os demais acompanham.

```bash
cd <pasta-dos-seus-repositórios>
git clone https://github.com/<usuario-do-integrante-1>/sistema-bancario.git
cd sistema-bancario
echo "target/" > .gitignore
```

Crie na raiz o arquivo `pom.xml` com o conteúdo da seção **Código final de referência** (é o único arquivo dessa seção que você usa desde já). Leia-o com o trio:

| Elemento | Função |
|---|---|
| `groupId` | grupo ou organização responsável |
| `artifactId` | identifica o projeto |
| `version` | versão do artefato; `-SNAPSHOT` marca uma versão **em desenvolvimento** |
| `maven.compiler.release` | versão da linguagem na compilação (17); roda no JDK 21 |
| `project.build.sourceEncoding` | codificação dos arquivos-fonte (UTF-8), por causa dos acentos |

Registre e publique, o único commit direto na `main`:

```bash
git status
git add pom.xml .gitignore
git commit -m "Adiciona pom.xml e .gitignore"
git push origin main
```

O `git status` deve listar `.gitignore` e `pom.xml` como não rastreados. O Git não registra pastas vazias: `src/main/java/...` só entra no repositório com o esqueleto, no Exercício 54.

---

## Exercício 52 — Clonar e compilar o projeto vazio

**Quem faz:** Integrantes 2 e 3 clonam. Os três compilam.

**Integrantes 2 e 3:** repitam o `git clone` e o `cd` do exercício anterior. `ls -a` deve listar `.git`, `.gitignore`, `README.md` e `pom.xml`.

**Os três**, na pasta do `pom.xml`:

```bash
mvn compile
git status
```

Na primeira execução o Maven baixa plugins, o que pode demorar. O final deve conter `BUILD SUCCESS`; ainda não há código-fonte, então não há classes a gerar. O `git status` deve indicar `nothing to commit, working tree clean`. Se aparecer `BUILD FAILURE`, copie a mensagem inteira e chame o instrutor.

---

## Exercício 53 — Acordar o contrato

**Quem faz:** os três, juntos, sem escrever código ainda.

Se o Integrante 2 criar `realizarSaque` e o Integrante 3 chamar `sacar`, cada classe estará correta sozinha e a integração falhará. Por isso o trio combina os nomes antes. Leiam a tabela e, se quiserem mudar algo, mudem **agora**, de comum acordo.

| Classe | Dono | Membros combinados |
|---|---|---|
| `Cliente` | Integrante 1 | `Cliente(int id, String nome, String cpf)`, `int getId()`, `String getNome()`, `String getCpf()`, `String descrever()` |
| `Conta` | Integrante 2 | `Conta(int numero, Cliente titular)`, `int getNumero()`, `Cliente getTitular()`, `boolean depositar(BigDecimal valor)`, `boolean sacar(BigDecimal valor)`, `BigDecimal consultarSaldo()`, `String descrever()` |
| `AplicacaoBanco` | Integrante 3 | `main`; atributos privados `List<Cliente> clientes` e `List<Conta> contas`; `private Conta buscarConta(int numero)`; `private boolean transferir(int contaOrigem, int contaDestino, BigDecimal valor)` |

Combinações adicionais:

- tudo no pacote `br.com.banco`, em `src/main/java/br/com/banco`;
- dinheiro em `BigDecimal`, nunca `double`;
- `depositar`, `sacar` e `transferir` devolvem `true` quando fazem a operação e `false` quando a recusam, **sem alterar nenhum saldo**;
- `descrever()` devolve, por exemplo, `Cliente 1 - Ana Souza - CPF 111.222.333-44` e `Conta 1 - titular: Ana Souza - saldo: R$ 250,00`.

Por que `boolean` e não `void`? A transferência precisa saber se o saque funcionou antes de depositar. Exceções, que resolveriam isso de outra forma, ainda não foram estudadas.

Um **construtor** é um método com o nome da classe e sem tipo de retorno; roda no `new` e grava os valores iniciais. Em `this.nome = nome;`, `this.nome` é o atributo e `nome` é o parâmetro.

### Ponte com COBOL

Um copybook compartilhado ou a lista de um `CALL ... USING` também fixa o que dois programas esperam um do outro. O contrato Java tem essa intenção; aqui o compilador acusa `cannot find symbol` se um nome divergir.

---

## Exercício 54 — Integrar o esqueleto das classes

**Quem faz:** Integrante 1 escreve e abre a PR. Integrante 2 revisa. Integrante 3 comenta. Integrante 1 integra.

O esqueleto tem as três classes com as assinaturas do contrato e corpos provisórios, para que toda branch criada depois dele compile.

**Integrante 1:**

```bash
git switch -c feature/esqueleto
mkdir -p src/main/java/br/com/banco
```

Crie nessa pasta `Cliente.java`, `Conta.java` e `AplicacaoBanco.java`, todos com `package br.com.banco;`. Em `Cliente` e `Conta`, escreva cada membro do contrato com construtor vazio e corpo provisório: `return 0;`, `return null;`, `return false;` ou `return BigDecimal.ZERO;`, conforme o tipo (`Conta` importa `java.math.BigDecimal`). `AplicacaoBanco` fica assim:

```java
package br.com.banco;

import java.math.BigDecimal;
import java.util.ArrayList;
import java.util.List;

public class AplicacaoBanco {

    private List<Cliente> clientes = new ArrayList<>();
    private List<Conta> contas = new ArrayList<>();

    public static void main(String[] args) {
        new AplicacaoBanco().executar();
    }

    private void executar() {
        System.out.println("Esqueleto: menu ainda não implementado.");
    }

    private Conta buscarConta(int numero) {
        return null;
    }

    private boolean transferir(int contaOrigem, int contaDestino, BigDecimal valor) {
        return false;
    }
}
```

Compile, execute e confira:

```bash
mvn compile
java -cp target/classes br.com.banco.AplicacaoBanco
ls target/classes/br/com/banco
```

A execução imprime `Esqueleto: menu ainda não implementado.` e a listagem contém `AplicacaoBanco.class`, `Cliente.class` e `Conta.class`. O `-cp target/classes` diz ao Java onde estão as classes compiladas pelo Maven. Se os acentos aparecerem trocados, a causa é a codificação do console e não o código: avise o instrutor e siga em frente.

```bash
git add src/main/java
git commit -m "Cria esqueleto das classes Cliente, Conta e AplicacaoBanco"
git push -u origin feature/esqueleto
```

No GitHub, use **Compare & pull request** e abra a PR para a `main` com o título `Cria esqueleto das classes conforme o contrato`. Será a PR nº 1.

**Integrante 2.** Em **Files changed**, compare cada assinatura com a tabela do Exercício 53. Se estiver tudo igual, use **Review changes → Approve**; se algo divergir, use **Comment** indicando a linha. **Integrante 3.** Deixe ao menos um comentário.

**Integrante 1.** Com a aprovação, clique em **Merge pull request**, mantenha **Create a merge commit** e depois clique em **Delete branch**.

**Os três:**

```bash
git switch main
git pull
```

O Integrante 1 também executa `git branch -d feature/esqueleto`.

---

## Exercício 55 — Integrante 1: implementar `Cliente`

**Quem faz:** Integrante 1, em paralelo com os Exercícios 56 e 57.

```bash
git switch main
git pull
git switch -c feature/cliente
```

Substitua os corpos provisórios de `Cliente.java`:

- atributos privados `id`, `nome` e `cpf`, gravados pelo construtor;
- `getId()`, `getNome()` e `getCpf()` devolvem o atributo correspondente;
- `descrever()` devolve o texto combinado.

Não altere nomes nem tipos do contrato. Faça ao menos dois commits, como `Adiciona atributos e construtor de Cliente` e `Implementa getters e descrever de Cliente`. Execute `mvn compile` (deve terminar em `BUILD SUCCESS`) e publique:

```bash
git push -u origin feature/cliente
```

Se terminar antes, releia o contrato. Não toque nos arquivos dos outros.

---

## Exercício 56 — Integrante 2: implementar `Conta`

**Quem faz:** Integrante 2, em paralelo com os Exercícios 55 e 57.

```bash
git switch main
git pull
git switch -c feature/conta
```

Requisitos para `Conta.java`:

- atributos privados `numero`, `titular` (um `Cliente`) e `saldo` (um `BigDecimal`); o saldo começa em zero;
- `depositar` recusa valor menor ou igual a zero; caso contrário, soma ao saldo;
- `sacar` recusa valor menor ou igual a zero ou maior que o saldo; caso contrário, subtrai;
- ambos devolvem `true` se alteraram o saldo e `false` se recusaram;
- `consultarSaldo()` e `descrever()` seguem o contrato;
- opcional: recuse valores com mais de duas casas decimais, com `valor.scale() > 2`.

| Necessidade | Código |
|---|---|
| criar um valor | `new BigDecimal("10.50")` |
| somar ou subtrair | `saldo = saldo.add(valor);` e `saldo = saldo.subtract(valor);` |
| comparar | `valor.compareTo(BigDecimal.ZERO) <= 0` |
| formatar | `String.format(Locale.forLanguageTag("pt-BR"), "R$ %,.2f", saldo)` |

`BigDecimal` é imutável: `saldo.add(valor)` devolve um **novo** valor, que precisa ser guardado de volta em `saldo`. Compare sempre com `compareTo`, não com `==`.

**Ponte com COBOL.** Campos como `PIC S9(7)V99` fazem aritmética decimal exata. Em Java, `double` não garante essa exatidão em valores monetários, e `BigDecimal` existe para preservar os centavos.

Faça commits pequenos, execute `mvn compile` e publique com `git push -u origin feature/conta`. Ainda não há menu para testar `Conta`; isso acontece no Exercício 61.

---

## Exercício 57 — Integrante 3: implementar `AplicacaoBanco`

**Quem faz:** Integrante 3, em paralelo com os Exercícios 55 e 56.

```bash
git switch main
git pull
git switch -c feature/aplicacao-banco
```

Requisitos para `AplicacaoBanco.java`:

- um menu em laço que só termina na opção `0`; opção desconhecida mostra `Opção inválida.`;
- o menu tem este formato:

```text
================================
SISTEMA BANCÁRIO
================================

1 - Cadastrar cliente
2 - Criar conta
3 - Consultar contas
4 - Consultar conta
5 - Depositar
6 - Sacar
7 - Transferir
0 - Sair

Escolha uma opção:
```

- `1`: pede nome e CPF e guarda o cliente em `clientes`, com `id` sequencial;
- `2`: pede o `id` do cliente e cria uma conta de número sequencial em `contas`; avisa se o cliente não existir;
- `3`: lista as contas com `descrever()`;
- `4`, `5` e `6`: pedem o número da conta, localizam com `buscarConta` e avisam se ela não existir;
- `5` e `6`: pedem o valor, chamam `depositar` ou `sacar` e informam, pelo `boolean`, se foi realizado;
- `7`: pede origem, destino e valor, chama `transferir` e informa o resultado.

**Não implemente o corpo de `transferir`:** ele é tarefa do Integrante 1, no Exercício 62. Implemente apenas `buscarConta`.

### Recursos novos de Java

- **`Scanner`** lê o teclado: `new Scanner(System.in)`, com `nextLine()` para texto, `nextInt()` e `nextBigDecimal()` para números. Importe `java.util.Scanner`.
- Depois de `nextInt()` ou `nextBigDecimal()`, chame `entrada.nextLine()` para consumir o fim da linha; senão, a próxima leitura de texto vem vazia.
- Para o programa não cair se alguém digitar letras, teste antes de ler, sem exceções: `while (!entrada.hasNextInt()) { entrada.next(); }`. Para valores, use `hasNextBigDecimal()`.
- Para aceitar `500,00` em qualquer máquina, crie o scanner com `new Scanner(System.in).useLocale(Locale.forLanguageTag("pt-BR"))`.
- `while (opcao != 0) { ... }` repete o menu e um `switch` escolhe a ação.

**Ponte com COBOL.** O laço lembra um `PERFORM UNTIL OPCAO = 0` em torno de um `ACCEPT`, com um `EVALUATE` escolhendo a ação. É a mesma ideia de repetição e decisão; o `Scanner` cumpre o papel de ler a entrada.

Por enquanto, `Cliente` e `Conta` só têm o esqueleto: o menu compila e aparece, mas não opera de verdade. O teste funcional acontece no Exercício 60, com as classes dos colegas na `main`.

Faça commits pequenos (`Cria menu principal da aplicacao`, `Implementa operacoes de deposito e saque`), execute `mvn compile` e publique com `git push -u origin feature/aplicacao-banco`.

---

## Exercício 58 — Acompanhar o trabalho do trio com `fetch`

**Quem faz:** os três, depois que as três branches foram publicadas.

Cada um continua na própria branch. Busque o que os colegas enviaram:

```bash
git fetch
git branch -a
git status -sb
```

`git branch -a` lista as branches remotas, como `remotes/origin/feature/conta`. O `git status -sb` não muda: o `fetch` só atualiza as referências `origin/...` e não mexe na sua branch nem nos seus arquivos.

Escolha a branch de um colega e examine-a **sem sair da sua**:

```bash
git log --oneline main..origin/feature/conta
git diff --stat main...origin/feature/conta
git show origin/feature/conta:src/main/java/br/com/banco/Conta.java
```

O primeiro comando lista os commits do colega que ainda não estão na `main`. O segundo resume o que ele mudou desde que a branch nasceu, com os três pontos do Tópico 3. O terceiro mostra o arquivo como está na branch dele. Troque `feature/conta` pela branch que quiser examinar.

Veja o desenho das três linhas de trabalho:

```bash
git log --oneline --graph --all -12
```

As três branches saem do mesmo commit da `main`, o da integração do esqueleto, e seguem separadas. Agora atualize a `main`:

```bash
git switch main
git pull
```

O resultado é `Already up to date.`: as branches foram publicadas, mas nenhuma foi integrada. **Publicar não é integrar.** O trabalho só chega à `main` por PR ou merge, no Exercício 60. Volte à sua branch com `git switch` antes de continuar.

---

## Exercício 59 — Abrir as Pull Requests

**Quem faz:** cada integrante abre a PR da sua branch.

No GitHub, use **Compare & pull request**, com base na `main`, e este modelo:

```markdown
## Alterações
- Criada a classe Conta.
- Implementados depósito e saque com BigDecimal.
- Implementada a consulta de saldo.

## Como testar
- `mvn compile` termina em BUILD SUCCESS.
- Depósito de valor negativo e saque acima do saldo são recusados.
```

A seção **Como testar** diz ao revisor o que conferir. Anote o número que o GitHub dá a cada PR (`#2`, `#3`...): ele faz parte da entrega.

Defina o revisor em **Reviewers**, em rodízio:

| PR de | Revisor |
|---|---|
| Integrante 1 | Integrante 2 |
| Integrante 2 | Integrante 3 |
| Integrante 3 | Integrante 1 |

**Ponte com COBOL.** Equipes de mainframe também conferem mudanças antes de levá-las a outro ambiente. A PR tem essa intenção: deixar a mudança visível, discutida e registrada junto do código.

---

## Exercício 60 — Revisar, ajustar e integrar

**Quem faz:** revisores e autores, na ordem do Exercício 59.

**Revisor.** Compile a branch do colega antes de opinar:

```bash
git fetch origin
git switch feature/<branch-do-colega>
mvn compile
```

Em **Files changed**, avalie: nomes claros? Uma responsabilidade por método? Parâmetros e retornos conforme o contrato? Validação de valores inválidos? Erro evidente de lógica? Clique no `+` ao lado de uma linha para comentá-la e finalize com **Review changes**:

| Opção | Quando usar |
|---|---|
| **Comment** | dúvida ou observação, sem decidir |
| **Approve** | a implementação está adequada para integrar |
| **Request changes** | algo precisa mudar antes |

Cada revisão deve ter ao menos um comentário. Aprovar não substitui testar. O GitHub impede aprovar a própria PR, e a regra do trio é a mesma.

**Autor.** Se houver pedido de mudança, corrija na mesma branch, faça `commit` e `push`: a PR se atualiza e o revisor avalia de novo.

**Integração pelo GitHub.** Integrem na ordem `Cliente`, `Conta`, `AplicacaoBanco`. Nas PRs aprovadas de `Cliente` e `Conta`, o autor clica em **Merge pull request** (**Create a merge commit**) e em **Delete branch**.

**Integrante 3: trazer a `main` para a branch.** Antes de integrar, traga as classes dos colegas e teste o menu de verdade:

```bash
git switch main
git pull
git switch feature/aplicacao-banco
git merge main -m "Atualiza a branch com a main"
mvn compile
java -cp target/classes br.com.banco.AplicacaoBanco
```

Cadastre um cliente, crie uma conta, deposite `100,00` e consulte as contas. Se achar defeito, corrija na mesma branch. Depois, publique com `git push`. É um `push` comum: o merge acrescentou um commit e não reescreveu nada. **Não** use `rebase` aqui, porque a branch já está em revisão e o revisor tem uma cópia dela.

**Integrante 3: integrar com merge local.** Com a PR aprovada, integre pelo terminal em vez do botão:

```bash
git switch main
git pull
git merge --no-ff feature/aplicacao-banco -m "Integra feature/aplicacao-banco"
mvn compile
git push
git push origin --delete feature/aplicacao-banco
```

O `--no-ff` cria um commit de merge mesmo quando um fast-forward seria possível, como faz o botão **Create a merge commit**. Assim o histórico registra o momento da integração. Abra a PR no GitHub: ela aparece como **Merged** sozinha, porque os commits dela chegaram à `main`.

Se o `git push` for recusado, alguém integrou algo na `main` nesse meio-tempo. Faça `git pull`, compile de novo e repita o `git push`. **Nunca** force o envio da `main`.

**Os três** atualizam a `main` e removem as branches locais já integradas, inclusive as baixadas para revisar (`git branch` as lista):

```bash
git switch main
git pull
git branch -d feature/<branch-integrada>
```

---

## Exercício 61 — Verificar a integração na `main`

**Quem faz:** os três, cada um no seu computador.

```bash
git switch main
git pull
git log --oneline
mvn compile
java -cp target/classes br.com.banco.AplicacaoBanco
```

O `git log --oneline` mostra os commits de cada integrante e os das integrações. `mvn compile` termina em `BUILD SUCCESS` e o menu permanece até a opção `0`.

Uma classe correta sozinha pode falhar quando integrada. Confira se as assinaturas chamadas são as do contrato, se tudo está em `br.com.banco` e se não há classe duplicada.

**Experimento local, sem commit.** Em `Conta.java`, troque `sacar` por `realizarSaque` e execute `mvn compile`: o resultado é `BUILD FAILURE` com `cannot find symbol` para `sacar`. Desfaça com `git restore src/main/java/br/com/banco/Conta.java`, que descarta a alteração ainda não commitada; o `git status` volta a indicar `working tree clean`.

---

## Exercício 62 — Implementar a transferência pelo mesmo fluxo

**Quem faz:** Integrante 1 implementa. Integrante 2 revisa. Integrante 3 prepara o Exercício 63.

**Integrante 1:** crie `feature/transferencia` a partir da `main` atualizada e implemente o corpo de `transferir`, sem alterar o resto do arquivo:

1. localize origem e destino com `buscarConta`; recuse se algum não existir;
2. recuse se forem a mesma conta;
3. peça a `sacar` da origem **primeiro**; se recusar, recuse sem tocar no destino;
4. só então `depositar` no destino e devolva `true`.

Sacar primeiro garante que, em qualquer falha, nenhum saldo mudou. Depositar primeiro deixaria o destino com dinheiro que não saiu da origem se o saque falhasse. Escreva uma mensagem para cada motivo de recusa.

Faça `commit`, `mvn compile`, publique e abra a PR. Antes de publicar, se a `main` tiver avançado, você pode reaplicar a branch sobre ela com `git rebase main`, como no Tópico 3: a branch ainda é só sua. Depois de abrir a PR, não reescreva mais. **Integrante 2** revisa com o checklist do Exercício 60. **Integrante 3** anota em papel os saldos esperados em cada passo do Exercício 63. Depois da integração, os três executam `git switch main` e `git pull`.

---

## Exercício 63 — Executar o cenário principal

**Quem faz:** os três, cada um no seu computador, comparando resultados.

```bash
mvn compile
java -cp target/classes br.com.banco.AplicacaoBanco
```

| Passo | Opção e dados digitados | Resultado esperado |
|---|---|---|
| 1 | `1`, `Ana Souza`, `111.222.333-44` | cliente 1 cadastrado |
| 2 | `1`, `Bruno Lima`, `555.666.777-88` | cliente 2 cadastrado |
| 3 | `2`, `1` | conta 1 de Ana, saldo R$ 0,00 |
| 4 | `2`, `2` | conta 2 de Bruno, saldo R$ 0,00 |
| 5 | `3`, depois `4` e `1` | as duas contas; depois só a conta 1 |
| 6 | `5`, conta `1`, valor `500,00` | depósito realizado; saldo R$ 500,00 |
| 7 | `6`, conta `1`, valor `100,00` | saque realizado; saldo R$ 400,00 |
| 8 | `7`, origem `1`, destino `2`, valor `150,00` | transferência realizada |
| 9 | `3` | conta 1 com **R$ 250,00** e conta 2 com **R$ 150,00** |
| 10 | `6`, conta `1`, valor `1000,00` | saque **recusado**; saldos inalterados |
| 11 | `7`, origem `1`, destino `99`, valor `10,00` | transferência **recusada**; saldos inalterados |
| 12 | `3` | R$ 250,00 e R$ 150,00 |
| 13 | `0` | o programa encerra |

Saída de referência no passo 9:

```text
Escolha uma opção: 3

Conta 1 - titular: Ana Souza - saldo: R$ 250,00
Conta 2 - titular: Bruno Lima - saldo: R$ 150,00
```

As **mensagens** podem ser diferentes; os **saldos** não. Confira também que depósito de `-5,00` e transferência de `0,00` são recusados e que digitar `abc` no menu não derruba o programa. Uma funcionalidade que só aparece no menu, sem executar a operação, não está concluída.

Se algo divergir, anote o passo, o esperado e o obtido para o próximo exercício.

---

## Exercício 64 — Corrigir e documentar pelo mesmo fluxo

**Quem faz:** os três. Quem corrige e quem revisa são pessoas diferentes.

**Parte A — correções.** Para cada divergência do Exercício 63, o trio escolhe quem corrige e segue o fluxo completo: branch `fix/<descrição-curta>` a partir da `main` atualizada; commit e publicação; PR descrevendo o defeito e a correção; revisão e aprovação por outra pessoa; integração, pelo botão ou por merge local; e atualização da `main` de todos. Sem defeitos, avise o instrutor e siga para a Parte B.

**Parte B — documentação (obrigatória).** O Integrante 2 cria a branch `docs/readme-execucao` e acrescenta ao `README.md`:

```markdown
## Como compilar
mvn compile

## Como executar
java -cp target/classes br.com.banco.AplicacaoBanco

## Integrantes
- Integrante 1: Cliente e transferência
- Integrante 2: Conta
- Integrante 3: AplicacaoBanco
```

Troque os papéis pelos nomes reais. O Integrante 3 revisa e o Integrante 2 integra. Só uma pessoa edita o `README.md` por vez; se duas alterarem a mesma linha, o conflito é o do Tópico 2 e se resolve no VS Code.

---

## Exercício 65 — Conferência final

**Ambiente e Maven**

- [ ] `java -version` e `mvn -version` funcionam nos três computadores.
- [ ] O `pom.xml` está na raiz, com as coordenadas `br.com.banco`, `sistema-bancario` e `1.0-SNAPSHOT`.
- [ ] `mvn compile` termina em `BUILD SUCCESS` na `main` atualizada.
- [ ] `target/` está no `.gitignore` e não foi versionada.

**Repositório e colaboração**

- [ ] O repositório é privado e os dois colegas são colaboradores.
- [ ] Só a configuração inicial foi commitada direto na `main`.
- [ ] Cada integrante usou a própria branch, e os commits têm mensagens descritivas.

**Contrato e código**

- [ ] O esqueleto foi integrado por PR antes do desenvolvimento.
- [ ] `Conta` usa `BigDecimal`; `depositar`, `sacar` e `transferir` devolvem `boolean`.
- [ ] `AplicacaoBanco` guarda `List<Conta>` e tem `buscarConta` e `transferir`.
- [ ] O menu permanece até a opção `0`.

**Sincronização, PRs e revisão**

- [ ] Cada integrante examinou a branch de um colega com `git fetch`, sem sair da própria branch.
- [ ] Toda PR foi revisada por outra pessoa e cada integrante revisou ao menos uma.
- [ ] Ninguém aprovou o próprio trabalho.
- [ ] A PR de `AplicacaoBanco` foi integrada por merge local e aparece como **Merged** no GitHub.
- [ ] Ninguém usou `reset`, `rebase` ou `push --force` na `main`.
- [ ] `git log --oneline --graph` mostra os commits de merge das integrações.

**Cenário**

- [ ] Saldos finais: R$ 250,00 e R$ 150,00.
- [ ] Saque acima do saldo e transferência para conta inexistente são recusados, com saldos coerentes.
- [ ] O `README.md` explica como compilar e executar.

---

## Entrega

Entregue o endereço do repositório, os nomes dos integrantes e os números das PRs:

```text
https://github.com/<usuario-do-integrante-1>/sistema-bancario
```

Como o repositório é privado, adicione o usuário GitHub do instrutor como colaborador, pelo procedimento do Exercício 50. A `main` deve ter a estrutura do **Ponto de partida**.

---

## Código final de referência

Use esta seção somente depois de concluir a sequência (exceto o `pom.xml`, usado no Exercício 51). O seu código pode usar mensagens diferentes, desde que respeite o contrato.

### `pom.xml`

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0
                             https://maven.apache.org/xsd/maven-4.0.0.xsd">

    <modelVersion>4.0.0</modelVersion>

    <groupId>br.com.banco</groupId>
    <artifactId>sistema-bancario</artifactId>
    <version>1.0-SNAPSHOT</version>

    <properties>
        <maven.compiler.release>17</maven.compiler.release>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    </properties>

</project>
```

### `Cliente.java`

```java
package br.com.banco;

public class Cliente {

    private int id;
    private String nome;
    private String cpf;

    public Cliente(int id, String nome, String cpf) {
        this.id = id;
        this.nome = nome;
        this.cpf = cpf;
    }

    public int getId() {
        return id;
    }

    public String getNome() {
        return nome;
    }

    public String getCpf() {
        return cpf;
    }

    public String descrever() {
        return "Cliente " + id + " - " + nome + " - CPF " + cpf;
    }
}
```

### `Conta.java`

```java
package br.com.banco;

import java.math.BigDecimal;
import java.util.Locale;

public class Conta {

    private int numero;
    private Cliente titular;
    private BigDecimal saldo;

    public Conta(int numero, Cliente titular) {
        this.numero = numero;
        this.titular = titular;
        this.saldo = BigDecimal.ZERO;
    }

    public int getNumero() {
        return numero;
    }

    public Cliente getTitular() {
        return titular;
    }

    public boolean depositar(BigDecimal valor) {
        if (!valorValido(valor)) {
            return false;
        }
        saldo = saldo.add(valor);
        return true;
    }

    public boolean sacar(BigDecimal valor) {
        if (!valorValido(valor) || valor.compareTo(saldo) > 0) {
            return false;
        }
        saldo = saldo.subtract(valor);
        return true;
    }

    public BigDecimal consultarSaldo() {
        return saldo;
    }

    public String descrever() {
        return "Conta " + numero + " - titular: " + titular.getNome()
                + " - saldo: " + formatar(saldo);
    }

    private String formatar(BigDecimal valor) {
        return String.format(Locale.forLanguageTag("pt-BR"), "R$ %,.2f", valor);
    }

    // Maior que zero e com no máximo duas casas decimais.
    private boolean valorValido(BigDecimal valor) {
        return valor != null
                && valor.compareTo(BigDecimal.ZERO) > 0
                && valor.scale() <= 2;
    }
}
```

### `AplicacaoBanco.java`

```java
package br.com.banco;

import java.math.BigDecimal;
import java.util.ArrayList;
import java.util.List;
import java.util.Locale;
import java.util.Scanner;

public class AplicacaoBanco {

    private List<Cliente> clientes = new ArrayList<>();
    private List<Conta> contas = new ArrayList<>();
    private Scanner entrada = new Scanner(System.in)
            .useLocale(Locale.forLanguageTag("pt-BR"));

    public static void main(String[] args) {
        new AplicacaoBanco().executar();
    }

    private void executar() {
        int opcao = -1;
        while (opcao != 0) {
            exibirMenu();
            opcao = lerInteiro("Escolha uma opção: ");
            System.out.println();
            switch (opcao) {
                case 1 -> cadastrarCliente();
                case 2 -> criarConta();
                case 3 -> consultarContas();
                case 4 -> consultarConta();
                case 5 -> depositar();
                case 6 -> sacar();
                case 7 -> transferirPeloMenu();
                case 0 -> System.out.println("Encerrando o sistema.");
                default -> System.out.println("Opção inválida.");
            }
        }
    }

    private void exibirMenu() {
        System.out.println();
        System.out.println("================================");
        System.out.println("SISTEMA BANCÁRIO");
        System.out.println("================================");
        System.out.println();
        System.out.println("1 - Cadastrar cliente");
        System.out.println("2 - Criar conta");
        System.out.println("3 - Consultar contas");
        System.out.println("4 - Consultar conta");
        System.out.println("5 - Depositar");
        System.out.println("6 - Sacar");
        System.out.println("7 - Transferir");
        System.out.println("0 - Sair");
        System.out.println();
    }

    private void cadastrarCliente() {
        String nome = lerTexto("Nome: ");
        String cpf = lerTexto("CPF: ");
        Cliente cliente = new Cliente(clientes.size() + 1, nome, cpf);
        clientes.add(cliente);
        System.out.println("Cliente cadastrado: " + cliente.descrever());
    }

    private void criarConta() {
        Cliente titular = buscarCliente(lerInteiro("ID do cliente: "));
        if (titular == null) {
            System.out.println("Cliente não encontrado.");
            return;
        }
        Conta conta = new Conta(contas.size() + 1, titular);
        contas.add(conta);
        System.out.println("Conta criada: " + conta.descrever());
    }

    private void consultarContas() {
        if (contas.isEmpty()) {
            System.out.println("Nenhuma conta cadastrada.");
            return;
        }
        for (Conta conta : contas) {
            System.out.println(conta.descrever());
        }
    }

    private void consultarConta() {
        Conta conta = buscarConta(lerInteiro("Número da conta: "));
        if (conta == null) {
            System.out.println("Conta não encontrada.");
            return;
        }
        System.out.println(conta.descrever());
    }

    private void depositar() {
        Conta conta = buscarConta(lerInteiro("Número da conta: "));
        BigDecimal valor = lerValor("Valor: ");
        if (conta == null) {
            System.out.println("Conta não encontrada.");
        } else if (conta.depositar(valor)) {
            System.out.println("Depósito realizado. " + conta.descrever());
        } else {
            System.out.println("Depósito não realizado. Valor inválido.");
        }
    }

    private void sacar() {
        Conta conta = buscarConta(lerInteiro("Número da conta: "));
        BigDecimal valor = lerValor("Valor: ");
        if (conta == null) {
            System.out.println("Conta não encontrada.");
        } else if (conta.sacar(valor)) {
            System.out.println("Saque realizado. " + conta.descrever());
        } else {
            System.out.println("Saque não realizado. Valor inválido ou saldo insuficiente.");
        }
    }

    private void transferirPeloMenu() {
        int origem = lerInteiro("Conta de origem: ");
        int destino = lerInteiro("Conta de destino: ");
        BigDecimal valor = lerValor("Valor: ");
        if (transferir(origem, destino, valor)) {
            System.out.println("Transferência realizada.");
        } else {
            System.out.println("Transferência não realizada.");
        }
    }

    private Cliente buscarCliente(int id) {
        for (Cliente cliente : clientes) {
            if (cliente.getId() == id) {
                return cliente;
            }
        }
        return null;
    }

    private Conta buscarConta(int numero) {
        for (Conta conta : contas) {
            if (conta.getNumero() == numero) {
                return conta;
            }
        }
        return null;
    }

    private boolean transferir(int contaOrigem, int contaDestino, BigDecimal valor) {
        Conta origem = buscarConta(contaOrigem);
        Conta destino = buscarConta(contaDestino);
        if (origem == null || destino == null) {
            System.out.println("Conta de origem ou de destino não encontrada.");
            return false;
        }
        if (origem == destino) {
            System.out.println("Origem e destino devem ser contas diferentes.");
            return false;
        }
        // Saca primeiro: se falhar, nada foi alterado em nenhuma das contas.
        if (!origem.sacar(valor)) {
            System.out.println("Valor inválido ou saldo insuficiente na origem.");
            return false;
        }
        destino.depositar(valor);
        return true;
    }

    private String lerTexto(String rotulo) {
        System.out.print(rotulo);
        return entrada.nextLine().trim();
    }

    private int lerInteiro(String rotulo) {
        System.out.print(rotulo);
        while (!entrada.hasNextInt()) {
            entrada.next();
            System.out.print("Valor inválido. " + rotulo);
        }
        int numero = entrada.nextInt();
        entrada.nextLine();
        return numero;
    }

    private BigDecimal lerValor(String rotulo) {
        System.out.print(rotulo);
        while (!entrada.hasNextBigDecimal()) {
            entrada.next();
            System.out.print("Valor inválido. " + rotulo);
        }
        BigDecimal valor = entrada.nextBigDecimal();
        entrada.nextLine();
        return valor;
    }
}
```

---

## Síntese final do projeto

No Tópico 1, você guardou o histórico de um projeto local. No Tópico 2, publicou-o no GitHub e aprendeu branches e conflitos. No Tópico 3, aprendeu a comparar, desfazer e reorganizar o histórico. Agora, três pessoas trabalharam no mesmo repositório sem pisar umas nas outras.

- O Maven padroniza a estrutura e a compilação, e o `pom.xml` descreve o projeto por suas coordenadas.
- Um contrato combinado antes do código, fixado por um esqueleto que compila, evita que classes corretas sozinhas falhem juntas.
- `fetch` mostra o trabalho dos colegas sem misturá-lo ao seu; `pull` traz a `main` atualizada; `push` publica, mas publicar não é integrar.
- A PR é um espaço de comunicação e revisão, e não só um botão para juntar código. A integração pode ser feita pelo botão ou por um merge local seguido de `push`.
- Em equipe, a regra de ouro deixa de ser teoria: a `main` e as branches dos colegas nunca são reescritas.
- Integrar é verificar: compilar, executar e conferir o cenário depois que as peças se encontram.

Dependências, ciclo de vida, testes automatizados e JAR ficam para uma etapa posterior do curso, sobre o mesmo projeto.
