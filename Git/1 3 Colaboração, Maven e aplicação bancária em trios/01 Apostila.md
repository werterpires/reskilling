# Tópico 3 — Trabalhar em equipe com Pull Requests e começar com Maven

## Do seu repositório para o repositório da equipe

Nos tópicos anteriores, você aprendeu a registrar o histórico de um projeto com Git, publicá-lo no GitHub, criar branches, integrar alterações com `merge` e resolver conflitos. Até aqui, porém, o repositório tinha basicamente uma pessoa decidindo o que entrava nele.

Em uma equipe real, essa situação muda. Várias pessoas alteram o mesmo projeto ao mesmo tempo, e surgem perguntas que um repositório pessoal não obriga a responder:

- quem pode enviar alterações para o repositório?
- como duas pessoas trabalham sem sobrescrever o trabalho uma da outra?
- como combinar antes de programar para que as partes se encaixem depois?
- como outra pessoa examina uma alteração antes de ela chegar à `main`?
- como registrar o que precisa ser feito e quem está fazendo?

Neste tópico, usaremos a aplicação de **Gestão de Biblioteca** como exemplo. Imagine um trio que precisa construir uma versão em console desse sistema, com as classes `Livro`, `Usuario` e `AplicacaoBiblioteca`. Cada pessoa assume uma classe. O desafio não é a quantidade de código, mas a coordenação.

Ao final do tópico, você também terá uma primeira visão do **Maven**, ferramenta que padroniza a estrutura e a compilação de projetos Java. Ele aparece neste ponto porque o projeto da equipe precisa de uma organização que todos reconheçam ao abrir o repositório.

Você fará, em ordem:

- dar acesso ao repositório às pessoas da equipe;
- combinar contratos entre as classes antes de programar;
- registrar tarefas como Issues;
- trabalhar em uma branch por responsabilidade;
- abrir Pull Requests ligados às Issues;
- revisar o código de outra pessoa;
- integrar na `main` e verificar o conjunto;
- conhecer a estrutura Maven, o `pom.xml` e o comando `mvn compile`.

Sua experiência em COBOL ajuda bastante aqui. Quem já trabalhou em sistemas corporativos conhece a importância de combinar layouts, controlar mudanças e revisar antes de colocar em produção. O que muda é o mecanismo: no ecossistema Git, boa parte desse processo acontece em branches, Pull Requests e revisões feitas no GitHub.

---

## 1. Um repositório, várias pessoas

Quando uma pessoa cria um repositório no GitHub, só ela pode enviar alterações para ele. Para que o trio trabalhe junto, a pessoa dona do repositório precisa convidar as demais como **colaboradoras**.

No GitHub, o caminho costuma ser:

```text
Repositório → Settings → Collaborators → Add people
```

Informe o usuário ou o e-mail de cada integrante. A pessoa convidada recebe um convite e precisa **aceitá-lo** antes de conseguir enviar branches. Sem aceitar, o `git push` é recusado, e esse é um dos primeiros problemas que aparecem em trabalhos em grupo.

Cada integrante então clona o repositório compartilhado no próprio computador:

```bash
git clone https://github.com/SEU-USUARIO/gestao-biblioteca.git
cd gestao-biblioteca
```

Se o repositório for privado, o Git precisa de uma credencial. Com o endereço HTTPS, o **Git Credential Manager**, instalado junto com o Git no Windows, abre uma janela do navegador para você entrar na sua conta do GitHub e autorizar o acesso. Não existe digitação de senha da conta no terminal, e essa senha não deve ser guardada em arquivo algum do projeto.

### Permissão de escrita não significa liberdade para alterar a `main`

Ter permissão para enviar alterações ao repositório não quer dizer que todo trabalho deva ir direto para a `main`. A combinação de equipe é: **ninguém desenvolve na `main`**. Cada alteração nasce em uma branch e chega à `main` por Pull Request.

O GitHub também permite transformar essa combinação em regra, com proteção de branch, exigindo Pull Request e aprovação antes da integração. Os recursos disponíveis variam conforme o tipo de repositório e o plano da conta. Neste tópico, a regra é apenas um acordo da equipe.

---

## 2. Combinar o contrato antes de programar

Imagine que três pessoas dividam a biblioteca assim:

| Integrante | Responsabilidade |
|---|---|
| 1 | `Usuario` |
| 2 | `Livro` |
| 3 | `AplicacaoBiblioteca` |

Cada uma trabalha isolada e escreve um código correto. Na hora de juntar, a aplicação chama `livro.emprestarLivro()`, mas a classe `Livro` oferece `emprestar()`. Nenhuma das duas está errada de forma isolada, mas o conjunto não compila.

A causa não é falta de habilidade. É a ausência de um **contrato**: um acordo escrito sobre os nomes, parâmetros e retornos que uma parte oferece para as outras.

### O que entra no contrato

Para este exemplo, o trio combina:

| Classe | Assinatura combinada | Significado |
|---|---|---|
| `Usuario` | `Usuario(String matricula, String nome)` | criação de um usuário |
| `Usuario` | `String obterMatricula()` | devolve a matrícula |
| `Usuario` | `String obterNome()` | devolve o nome |
| `Livro` | `Livro(String codigo, String titulo)` | criação de um livro disponível |
| `Livro` | `String obterCodigo()` | devolve o código |
| `Livro` | `String obterTitulo()` | devolve o título |
| `Livro` | `boolean estaDisponivel()` | informa se pode ser emprestado |
| `Livro` | `boolean emprestar()` | `true` se emprestou; `false` se já estava emprestado |
| `Livro` | `boolean devolver()` | `true` se devolveu; `false` se não estava emprestado |
| `AplicacaoBiblioteca` | `Livro buscarLivro(String codigo)` | devolve o livro ou `null` |
| `AplicacaoBiblioteca` | `boolean emprestar(String codigo, Usuario usuario)` | coordena o empréstimo |

Observe que `emprestar` e `devolver` retornam `boolean`. Como você viu nos tópicos de Java, o retorno permite que quem chama saiba se a operação foi realizada. Sem isso, a aplicação não teria como avisar que o livro já estava emprestado.

### Construtores em poucas palavras

A tabela usa `Usuario(String matricula, String nome)`, que é um **construtor**. Um construtor tem o nome da classe, não declara tipo de retorno e roda quando o objeto é criado com `new`:

```java
Usuario ana = new Usuario("U001", "Ana Martins");
```

Ele existe para que o objeto já nasça com seus dados. Nos tópicos anteriores você usou métodos como `cadastrar` para preencher o objeto depois de criado. O construtor faz esse preenchimento no momento da criação, o que torna o contrato mais curto.

### Um esqueleto que já compila

Combinar assinaturas em uma conversa ou em uma tabela não é suficiente. A forma mais segura é transformar o contrato em **código que compila**. Uma pessoa do trio cria os três arquivos com as assinaturas e corpos provisórios, junto com o `pom.xml` e o `.gitignore` do projeto, e os integra na `main` por Pull Request antes de qualquer trabalho paralelo.

Para `Livro`, o esqueleto fica assim:

```java
package br.com.biblioteca;

public class Livro {

    public Livro(String codigo, String titulo) {
        // TODO: Integrante 2
    }

    public String obterCodigo() {
        return null; // TODO: Integrante 2
    }

    public String obterTitulo() {
        return null; // TODO: Integrante 2
    }

    public boolean estaDisponivel() {
        return false; // TODO: Integrante 2
    }

    public boolean emprestar() {
        return false; // TODO: Integrante 2
    }

    public boolean devolver() {
        return false; // TODO: Integrante 2
    }
}
```

Os corpos não funcionam ainda, e isso é intencional. O objetivo é que `AplicacaoBiblioteca` consiga **compilar** contra `Livro` e `Usuario` desde o primeiro minuto. Cada integrante então troca os corpos provisórios por implementações reais na sua branch.

Esse esqueleto também resolve um problema de dependência. `AplicacaoBiblioteca` usa `Livro` e `Usuario`. Se cada integrante partisse da `main` vazia, a aplicação de quem faz a classe principal não compilaria até as outras duas chegarem. Com o esqueleto na `main`, as três branches compilam desde o início.

### Mudou o contrato? Avise antes

O contrato é um acordo. Se alguém concluir que `emprestar` precisa de um parâmetro novo, a mudança afeta as outras pessoas. A regra é comunicar primeiro e alterar depois, de preferência em uma Issue ou Pull Request pequeno, para que todos vejam.

### Ponte com COBOL

Em COBOL, programas que trocam dados costumam combinar o layout em um *copybook* ou na área de comunicação da chamada. Se um lado altera o tamanho de um campo e o outro não, a falha aparece na integração.

O contrato de classes cumpre uma intenção parecida: fixar o formato da conversa entre as partes antes de cada uma avançar. A comparação é de propósito, não de equivalência. Em Java, o compilador verifica os nomes, os tipos dos parâmetros e o retorno, mas não verifica o **significado** combinado (por exemplo, que `false` quer dizer "já estava emprestado"). Esse significado precisa estar escrito no contrato.

---

## 3. Issues: registrar o que precisa ser feito

Uma **Issue** é um registro de tarefa, melhoria ou problema dentro do repositório. Ela tem título, descrição, responsáveis e etiquetas (*labels*), além de um número, como `#3`.

Para a biblioteca do trio, bons exemplos de Issues são:

| Número | Título | Responsável |
|---|---|---|
| `#1` | Criar a classe `Usuario` | Integrante 1 |
| `#2` | Criar a classe `Livro` com regras de empréstimo | Integrante 2 |
| `#3` | Criar a `AplicacaoBiblioteca` com busca e empréstimo | Integrante 3 |

Uma boa Issue responde:

- o que deve ser feito;
- como saber que está pronto;
- quem assumiu a tarefa.

Por exemplo, na Issue `#2`:

```markdown
Implementar Livro conforme o contrato combinado.

Critérios:
- novo livro nasce disponível;
- emprestar() devolve false se já estiver emprestado;
- devolver() devolve false se não estiver emprestado.
```

Atribuir a Issue a uma pessoa evita que duas comecem a mesma tarefa. Uma etiqueta como `Integrante 2` também ajuda a filtrar o que pertence a cada um.

As Issues têm uma segunda utilidade: quando o Pull Request que as resolve for integrado, elas podem ser fechadas automaticamente. Isso aparece na seção 5.

---

## 4. Uma branch por responsabilidade

No Tópico 2 do Git, você criou branches para desenvolver sem afetar a `main`. Em equipe, a prática continua, com uma regra de organização: **uma branch por responsabilidade**, criada a partir da `main` atualizada.

Antes de criar a branch, confirme que a `main` local está em dia:

```bash
git switch main
git pull
```

Depois, crie a branch da sua tarefa. O Integrante 2, por exemplo:

```bash
git switch -c feature/livro
```

```text
Switched to a new branch 'feature/livro'
```

O nome da branch segue um padrão que todos entendem. Alguns exemplos para o trio:

| Integrante | Branch |
|---|---|
| 1 | `feature/usuario` |
| 2 | `feature/livro` |
| 3 | `feature/aplicacao-biblioteca` |

### Por que dividir por classe e não por operação

Seria tentador dividir assim: uma pessoa faz "emprestar", outra faz "devolver" e outra faz "buscar". O problema é que todas alterariam o mesmo arquivo, e os conflitos viriam por motivos artificiais.

Dividir por responsabilidade, com um arquivo principal por pessoa, reduz a chance de duas pessoas editarem as mesmas linhas. Os conflitos que sobram são normalmente os de integração, e esses precisam ser enfrentados mesmo.

### Commits pequenos e descritivos

Trabalhando na sua branch, faça commits que representem passos compreensíveis:

```bash
git add src/main/java
git commit -m "Implementa estado e construtor de Livro"
git add src/main/java
git commit -m "Implementa emprestar e devolver"
```

Mensagens como `alteracoes`, `teste` ou `final` não ajudam quem revisa. Um commit bem nomeado diz o que mudou, e uma sequência deles conta a história da funcionalidade.

### Publicar a branch

Quando houver algo para compartilhar, envie a branch para o GitHub:

```bash
git push -u origin feature/livro
```

A opção `-u` associa a branch local à remota. Nos pushes seguintes, `git push` basta.

### E se a `main` mudar enquanto você trabalha?

Enquanto você implementa `Livro`, outra pessoa pode integrar `Usuario`. Para trazer essas mudanças para a sua branch, atualize a `main` local e integre-a:

```bash
git switch main
git pull
git switch feature/livro
git merge main
```

Esse é o mesmo `merge` que você usou no Tópico 2 do Git, agora na direção contrária: trazendo a `main` para a sua branch. Fazer isso com frequência evita que as diferenças se acumulem.

---

## 5. Pull Request: pedir para integrar

Um **Pull Request**, abreviado como PR, é um pedido para que as alterações de uma branch sejam incorporadas a outra, normalmente a `main`. Ele fica no GitHub, ao lado do código, e reúne tudo em um só lugar:

- as diferenças entre as duas branches;
- os commits incluídos;
- a descrição do que foi feito;
- os comentários e as revisões;
- o resultado da integração.

O PR não é apenas um botão para juntar código. É um ponto de **comunicação**: quem abre explica o que fez, e quem revisa pode perguntar, sugerir e aprovar.

### Base e comparação

Todo PR tem duas branches:

| Campo | Significado | Exemplo |
|---|---|---|
| `base` | destino, onde as alterações vão entrar | `main` |
| `compare` | origem, de onde as alterações vêm | `feature/livro` |

Depois do `git push`, o GitHub costuma oferecer um botão para abrir o PR da branch recém-publicada. Também é possível abrir pela aba **Pull requests**, escolhendo as duas branches.

### Um título e uma descrição úteis

Exemplo de título:

```text
Implementa a classe Livro
```

Exemplo de descrição:

```markdown
## Alterações
- Atributos e construtor de Livro.
- emprestar() e devolver() retornando boolean.
- estaDisponivel() e getters.

## Issue relacionada
Closes #2
```

### Ligar o PR à Issue

A linha `Closes #2` tem efeito prático. Quando o PR for integrado à branch padrão do repositório, o GitHub fecha automaticamente a Issue `#2`. O mesmo vale para palavras como `Fixes` e `Resolves`.

### Integrando o PR

Depois da revisão, alguém aciona o botão de integração. O GitHub oferece *merge commit*, *squash* (junta os commits da branch em um) e *rebase*; para o trio, o *merge commit* é suficiente.

Depois da integração, o GitHub oferece apagar a branch remota, o que costuma ser uma boa limpeza. Localmente, atualize a `main` e remova a branch que já cumpriu o papel:

```bash
git switch main
git pull
git branch -d feature/livro
git fetch --prune
```

```text
Deleted branch feature/livro (was bbf4406).
```

O `git fetch --prune` remove da sua cópia as referências a branches remotas que já não existem. O identificador do commit na sua tela será diferente.

---

## 6. Code Review: outra pessoa examina antes

**Code review**, ou revisão de código, é a análise de uma alteração por alguém que não a escreveu. O Pull Request é o lugar onde isso acontece.

Quem escreve tende a ver o que pretendia fazer, não o que escreveu. Uma segunda pessoa enxerga nomes confusos, casos esquecidos e desvios do contrato.

### O que olhar numa revisão

Para o PR de `Livro`, o revisor pode perguntar:

- os nomes seguem o contrato combinado?
- `emprestar()` retorna `false` quando o livro já está emprestado?
- `devolver()` trata o caso de um livro que não foi emprestado?
- algum atributo ficou público sem necessidade?
- o código está no pacote correto?
- a mensagem dos commits explica a mudança?

### Ler a diferença e também executar

O GitHub mostra a aba **Files changed**, em que linhas adicionadas e removidas aparecem destacadas. Dá para comentar em uma linha específica.

Ler a diferença nem sempre basta. Para conferir se o código compila e se comporta como esperado, traga a branch para o seu computador:

```bash
git fetch
git switch feature/livro
```

Como a branch existe no `origin`, o `git switch` cria a cópia local que acompanha a remota. Depois, compile e execute como faria com o seu próprio código.

### Comentar, pedir mudanças ou aprovar

Ao finalizar a revisão, o GitHub oferece três caminhos:

| Opção | Quando usar |
|---|---|
| Comment | observações sem decidir sobre a integração |
| Approve | você considera a alteração adequada para entrar |
| Request changes | há algo a corrigir antes de integrar |

Aprovar não é garantia de que o código está livre de defeitos. A aprovação indica que a revisão não encontrou impedimentos, mas **não substitui** compilar e testar.

### O ciclo de correção

Se houver pedidos de mudança, o autor corrige na **mesma branch**, faz novos commits e envia:

```bash
git add src/main/java
git commit -m "Trata devolucao de livro nao emprestado"
git push
```

O PR é atualizado automaticamente com os novos commits, e o revisor reavalia. Não é necessário abrir outro PR.

### Regras de convivência

Em um trio, valem duas regras simples:

- cada integrante revisa pelo menos um PR de outro integrante;
- ninguém aprova o próprio PR.

O GitHub não permite que o autor aprove seu próprio PR, o que reforça a regra. Um comentário bom é específico e respeitoso: aponta a linha, explica o motivo e, quando possível, sugere uma alternativa. A crítica é ao código, não à pessoa.

### Ponte com COBOL

Em muitos ambientes mainframe existem controle de mudança, revisão por pares e etapas de aprovação antes de promover um programa. O Pull Request organiza uma intenção semelhante no fluxo do Git: registrar o que mudou, permitir que outra pessoa examine e deixar a decisão documentada. Ele não substitui os processos da sua organização. É uma forma de executar esse tipo de cuidado em um repositório Git.

---

## 7. Integrar e verificar o conjunto

Depois que os três PRs entram na `main`, cada parte pode estar correta e, mesmo assim, o conjunto falhar. Por isso, a integração tem uma etapa própria de verificação.

Cada integrante atualiza a `main` e compila:

```bash
git switch main
git pull
mvn compile
```

Se algo falhar, a mensagem ajuda a classificar o problema:

| Sintoma | Causa provável |
|---|---|
| `cannot find symbol` | nome de método ou classe diferente do contrato |
| `incompatible types` | tipo de parâmetro ou retorno diferente do combinado |
| `package ... does not exist` | classe em pacote errado ou import ausente |
| `duplicate class` | duas versões da mesma classe no projeto |

Compilar não esgota a verificação. Também é preciso **executar** e conferir o comportamento. Um método que existe, mas ainda tem corpo provisório, não é uma funcionalidade pronta.

As correções seguem o mesmo fluxo: Issue, branch, PR, revisão.

---

## 8. Maven: um jeito padrão de organizar projetos Java

No Tópico 1 de Java, você compilou com `javac` e executou com `java`. Isso funciona bem para um arquivo ou para poucos arquivos. Conforme o projeto cresce e passa a ter várias pessoas, surgem perguntas:

- onde ficam os arquivos-fonte?
- para onde vão os arquivos compilados?
- qual versão do Java o projeto exige?
- como outra pessoa compila o projeto sem aprender o seu comando?

O **Maven** é uma ferramenta de gerenciamento e automação de projetos Java. Ele padroniza a estrutura do projeto, descreve o projeto em um arquivo e executa tarefas como a compilação. Mais adiante ele também cuida de bibliotecas externas, testes e empacotamento, assuntos do próximo tópico.

### Convenção sobre configuração

O Maven parte da ideia de que projetos parecidos devem ter organização parecida. Em vez de cada equipe inventar suas pastas, ele define uma estrutura padrão. Quem conhece Maven reconhece o projeto ao abri-lo.

Para o nosso exemplo:

```text
gestao-biblioteca/
├── pom.xml
└── src/
    ├── main/
    │   └── java/
    │       └── br/
    │           └── com/
    │               └── biblioteca/
    │                   ├── AplicacaoBiblioteca.java
    │                   ├── Livro.java
    │                   └── Usuario.java
    └── test/
        └── java/
```

| Caminho | Função |
|---|---|
| `pom.xml` | arquivo principal de configuração do projeto |
| `src/main/java` | código-fonte da aplicação |
| `src/test/java` | código dos testes automatizados, tema do próximo tópico |
| `br/com/biblioteca` | pastas que correspondem ao `package br.com.biblioteca` |

O pacote declarado no arquivo precisa corresponder à pasta. Um arquivo com `package br.com.biblioteca;` deve estar em `br/com/biblioteca`.

### O `pom.xml`

POM significa *Project Object Model*. É um arquivo XML que descreve o projeto. Uma versão mínima para a biblioteca é:

```xml
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0
                             https://maven.apache.org/xsd/maven-4.0.0.xsd">

    <modelVersion>4.0.0</modelVersion>

    <groupId>br.com.biblioteca</groupId>
    <artifactId>gestao-biblioteca</artifactId>
    <version>1.0-SNAPSHOT</version>

    <properties>
        <maven.compiler.release>17</maven.compiler.release>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    </properties>

</project>
```

Leia por blocos:

| Elemento | Significado |
|---|---|
| `modelVersion` | versão do formato do POM; para o Maven 3, use `4.0.0` |
| `groupId` | identifica o grupo ou a organização responsável |
| `artifactId` | identifica o projeto dentro do grupo |
| `version` | identifica a versão do artefato |
| `maven.compiler.release` | versão do Java usada na compilação |
| `project.build.sourceEncoding` | codificação dos arquivos-fonte |

### Coordenadas e SNAPSHOT

`groupId`, `artifactId` e `version` formam as **coordenadas** do projeto. Elas funcionam como um endereço: qualquer projeto Maven, em qualquer lugar, é identificado por esses três valores. Por isso o `groupId` costuma seguir o padrão de domínio invertido, como `br.com.biblioteca`.

O sufixo `-SNAPSHOT` em `1.0-SNAPSHOT` indica uma versão **em desenvolvimento**, que ainda muda. Uma versão sem o sufixo, como `1.0`, indica uma versão fechada. Por enquanto, mantenha `SNAPSHOT`: o projeto está em construção.

Sobre a linha `maven.compiler.release`, o valor `17` determina que o código seja compilado com o nível de linguagem do Java 17. O JDK 21 instalado no Tópico 1 de Java consegue compilar para esse nível, então não há conflito.

### Verificar e compilar

O Maven precisa estar instalado e acessível no `PATH`, assim como o JDK. Ele também usa o Java instalado, portanto o `JAVA_HOME` configurado no Tópico 1 de Java continua importante. Para conferir:

```bash
mvn -version
```

A primeira linha da saída informa a versão do Maven. As linhas seguintes mostram o Java que ele encontrou, e vale conferir se é o Java 21.

Dentro da pasta que contém o `pom.xml`, compile:

```bash
mvn compile
```

Ao final, a mensagem `BUILD SUCCESS` indica que a compilação terminou sem erros. Os arquivos compilados são colocados em `target/classes`:

```text
target/classes/br/com/biblioteca/AplicacaoBiblioteca.class
target/classes/br/com/biblioteca/Livro.class
target/classes/br/com/biblioteca/Usuario.class
```

A pasta `target` é gerada pelo Maven. Ela é resultado de construção, não código-fonte, e não deve ir para o Git. Acrescente ao `.gitignore`, como você fez no Tópico 1 do Git:

```text
target/
```

### Executar o programa compilado

O comando `mvn compile` compila, mas não executa. Para rodar a aplicação, use o `java` indicando onde estão as classes e qual classe contém o `main`:

```bash
java -cp target/classes br.com.biblioteca.AplicacaoBiblioteca
```

A opção `-cp` é o *classpath*, a lista de lugares onde o Java procura classes. Aqui, `target/classes` é onde o Maven colocou o resultado. Com o exemplo da biblioteca, a execução produz:

```text
Ana Martins pegou Dom Casmurro
Dom Casmurro já está emprestado
Livro não encontrado: L999
```

Você já usou `javac -d` para escolher onde gravar os `.class`. O Maven assume essa decisão por convenção: o fonte fica em `src/main/java` e o resultado em `target/classes`.

### O que fica para o próximo tópico

O Maven faz bem mais do que compilar: bibliotecas externas, ciclo de vida, testes e empacotamento ficam para o tópico seguinte. Aqui, basta entender a estrutura, o `pom.xml`, as coordenadas, o `SNAPSHOT` e o `mvn compile`.

### Ponte com COBOL

Em ambientes mainframe, a compilação e a ligação costumam ser descritas por procedimentos e JCL, que indicam passos, bibliotecas e destinos. O `pom.xml` cumpre uma intenção comparável: descrever como o projeto é construído para que qualquer pessoa da equipe consiga repetir o processo. A diferença está no estilo. O JCL tende a listar os passos explicitamente, enquanto o Maven assume convenções e permite configurar apenas o que foge delas.

---

## 9. Erros de compreensão que vale evitar

### “Pull Request é só o botão de juntar o código”

O PR é também o lugar da explicação e da revisão. Se ele for aberto sem descrição e integrado sem olhar, o ganho da ferramenta se perde.

### “Se compila na minha branch, está pronto”

Sua branch compila com o que existia quando você a criou. Depois da integração, o conjunto pode falhar. Atualize a `main` e compile de novo.

### “Posso mudar o nome do método, pois está melhor”

O nome pode estar melhor, mas outras pessoas dependem do contrato. Alterações combinadas e comunicadas valem, mas alterações silenciosas quebram o trabalho dos demais.

### “Aprovar um PR garante que funciona”

A aprovação diz que a revisão não encontrou impedimentos. Ela não executa o programa por você. Compile e teste.

### “Posso aprovar meu próprio PR para ganhar tempo”

Isso anula a revisão. O objetivo é que alguém diferente do autor examine a alteração.

### “Basta criar o convite para a pessoa conseguir enviar código”

O convite precisa ser aceito. Sem isso, o `git push` é recusado.

### “O `mvn compile` executa a aplicação”

Ele apenas compila. A execução é feita com `java -cp target/classes` e o nome completo da classe.

---

## 10. Checklist do Tópico 3

- [ ] Sei dar acesso ao repositório a outras pessoas e lembro que o convite precisa ser aceito.
- [ ] Sei que o push por HTTPS autentica pelo Git Credential Manager, no navegador.
- [ ] Entendo por que o trabalho em equipe evita alterações diretas na `main`.
- [ ] Sei explicar o que é um contrato entre integrantes.
- [ ] Consigo listar assinaturas de métodos como parte de um contrato.
- [ ] Entendo o que é um construtor e como ele é chamado com `new`.
- [ ] Sei por que um esqueleto compilável vai para a `main` antes do trabalho paralelo.
- [ ] Sei criar uma Issue com título, descrição e responsável.
- [ ] Sei criar uma branch com `git switch -c` a partir da `main` atualizada.
- [ ] Sei publicar a branch com `git push -u origin`.
- [ ] Sei trazer a `main` para a minha branch com `git merge main`.
- [ ] Sei abrir um Pull Request com `base` e `compare` corretos.
- [ ] Sei ligar o PR a uma Issue com `Closes #N`.
- [ ] Sei o que olhar em um code review.
- [ ] Sei trazer a branch de outra pessoa para executá-la localmente.
- [ ] Distingo Comment, Approve e Request changes.
- [ ] Sei corrigir um PR pedindo mudanças sem abrir outro.
- [ ] Não aprovo meu próprio PR.
- [ ] Sei atualizar a `main`, compilar e executar depois da integração.
- [ ] Reconheço a estrutura de um projeto Maven.
- [ ] Sei explicar `groupId`, `artifactId` e `version`.
- [ ] Entendo o que significa `SNAPSHOT`.
- [ ] Sei usar `mvn -version` e `mvn compile`.
- [ ] Sei executar a aplicação com `java -cp target/classes`.
- [ ] Sei por que `target/` fica no `.gitignore`.
- [ ] Consigo comparar, sem confundir, contrato, controle de mudança e o `pom.xml` com o que conheço de COBOL.

---

## Conclusão

Neste tópico, você passou do uso individual do Git para o trabalho coordenado. O código continua importante, mas a qualidade do resultado depende também de como a equipe combina, divide, revisa e integra.

Primeiro, a equipe fixa um contrato e o transforma em um esqueleto que compila. Depois, as tarefas viram Issues, cada pessoa trabalha em sua branch, e as alterações chegam à `main` por Pull Requests revisados. Ao final, todos atualizam a `main`, compilam e executam o conjunto para ter certeza de que as partes realmente se encaixam.

O Maven entrou como uma base comum. Com uma estrutura padrão, um `pom.xml` e o comando `mvn compile`, qualquer integrante clona o projeto e o constrói da mesma forma. Os assuntos que ainda faltam, como dependências, ciclo de vida, testes e empacotamento, ficam para o próximo tópico, usando o mesmo projeto.

Sua experiência com COBOL e mainframe continua útil: a atenção a layouts, a disciplina de mudança e o cuidado com o que vai para produção são a mesma preocupação, expressa agora em branches, Pull Requests e revisões.

Desenvolver em equipe é, antes de tudo, comunicar. O resultado depende de combinar claramente, mudar com aviso e revisar com atenção.

---

## Referências oficiais

- Documentação do GitHub sobre Pull Requests: <https://docs.github.com/pt/pull-requests>
- Documentação do GitHub sobre Issues: <https://docs.github.com/pt/issues>
- Apache Maven, introdução: <https://maven.apache.org/guides/getting-started/>
- Apache Maven, estrutura de diretórios padrão: <https://maven.apache.org/guides/introduction/introduction-to-the-standard-directory-layout.html>
