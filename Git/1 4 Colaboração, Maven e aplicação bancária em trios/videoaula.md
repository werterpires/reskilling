# Aula 4 — Git colaborativo, Maven e desenvolvimento de uma aplicação bancária

- Módulo: Git Básico → Intermediário + Maven
- Aula: 4 de 4
- Tema: Desenvolvimento colaborativo com GitHub, Pull Requests, Code Review e introdução ao Maven
- Formato: Apresentação breve + atividade prática em trios
- Duração sugerida: 2h30 a 3h
- Projeto: Sistema Bancário

## 1. Objetivos da aula

Ao final da aula, os participantes deverão ser capazes de:

- Organizar um projeto Java utilizando a estrutura padrão do Maven.
- Compreender a função do arquivo pom.xml.
- Criar branches para desenvolver funcionalidades isoladamente.
- Dividir responsabilidades entre integrantes de uma equipe.
- Publicar alterações no GitHub por meio de Pull Requests.
- Analisar e revisar o código de outro desenvolvedor.
- Integrar diferentes partes de uma aplicação sem trabalhar diretamente na mesma branch principal.

O objetivo central não é construir um sistema bancário completo. É simular uma situação de desenvolvimento em equipe, na qual cada integrante assume uma responsabilidade e precisa integrar seu trabalho ao dos demais.

## 2. Abertura: como uma equipe desenvolve software?

Comece retomando o que foi aprendido na aula anterior.

Pergunte à turma:

- Se três pessoas precisarem desenvolver uma aplicação Java, como podem trabalhar simultaneamente?
- O que acontece se todos alterarem a mesma classe?
- Como garantir que uma alteração feita por um integrante não prejudique o trabalho dos demais?
- Como revisar o código antes de incorporá-lo à versão principal?

Apresente o cenário:

Uma equipe recebeu a tarefa de desenvolver uma aplicação bancária simples. O sistema deverá permitir o cadastro de clientes, a criação de contas e a realização de operações financeiras.

A equipe será dividida em trios. Cada integrante ficará responsável por uma parte da aplicação, mas todos deverão colaborar para que o sistema funcione como um conjunto.

Conceitos para explicar:

- Repositório compartilhado: local central onde o código da equipe é mantido.
- Branch: ambiente isolado para desenvolver uma funcionalidade.
- Pull Request: solicitação para incorporar alterações de uma branch a outra.
- Code Review: análise do código por outro integrante antes da integração.
- Fetch, pull e push em equipe: acompanhar o trabalho dos colegas, atualizar a main e publicar a própria branch.
- Merge local: integrar uma branch pelo terminal e publicar o resultado com push.

Destaque que o Pull Request não é apenas um botão para juntar códigos. É uma etapa de comunicação e revisão do trabalho.

## 3. Introdução ao Maven

Antes de iniciar o desenvolvimento, apresente o Maven.

Explique que o Maven é uma ferramenta de gerenciamento e automação de projetos Java. Ele permite padronizar a estrutura do projeto, administrar dependências e executar tarefas como compilação, testes e empacotamento.

Sem Maven, um desenvolvedor pode precisar organizar manualmente os arquivos compilados, configurar bibliotecas e executar comandos de compilação mais complexos.

Com Maven, boa parte dessa organização segue convenções conhecidas por outros desenvolvedores.

### 3.1. Estrutura de um projeto Maven

Apresente a estrutura:

```text
sistema-bancario/
├── pom.xml
└── src/
    ├── main/
    │   └── java/
    │       └── br/
    │           └── com/
    │               └── banco/
    │                   ├── AplicacaoBanco.java
    │                   ├── Conta.java
    │                   └── Cliente.java
    └── test/
        └── java/
```

Explique:

- pom.xml: arquivo principal de configuração do Maven.
- src/main/java: código-fonte da aplicação.
- src/test/java: código destinado aos testes automatizados.
- Pacotes: organização lógica das classes Java.

Apresente o conceito de convenção sobre configuração: o Maven estabelece uma estrutura padrão para que diferentes desenvolvedores reconheçam rapidamente a organização de um projeto.

### 3.2. O arquivo pom.xml

Apresente um exemplo mínimo:

```xml
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

Explique as coordenadas do projeto:

- groupId: identifica o grupo ou organização responsável.
- artifactId: identifica o projeto.
- version: identifica a versão do artefato.

Apresente também o conceito de versão SNAPSHOT, utilizada para indicar uma versão em desenvolvimento.

Demonstração:

- Acesse o repositório do projeto.
- Abra o VS Code.
- Crie a estrutura Maven.
- Adicione o pom.xml.
- Verifique se as classes estão dentro de src/main/java.

Execute no terminal:

```bash
mvn -version
mvn compile
```

Caso o Maven não esteja instalado ou configurado, demonstre a instalação e a configuração do PATH. Se a infraestrutura da turma já disponibilizar Maven, utilize o ambiente existente.

Explique que `mvn compile` executa a etapa de compilação do projeto. Não é necessário aprofundar o ciclo de vida nesta aula; isso fica para uma etapa posterior do curso.

## 4. Apresentação do desafio: Sistema Bancário

Agora apresente a atividade principal.

**Desafio:** desenvolver, em equipe, uma aplicação bancária executada pelo terminal, com cadastro de clientes, criação de contas e operações financeiras.

A aplicação deverá apresentar um menu enquanto estiver em execução:

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

O menu deverá continuar aparecendo até que o usuário escolha a opção de saída.

A aplicação deverá permitir:

- Cadastrar clientes.
- Criar contas vinculadas a clientes.
- Consultar todas as contas cadastradas.
- Pesquisar uma conta pelo número.
- Depositar em uma conta.
- Sacar de uma conta.
- Transferir valores entre duas contas.

Regras básicas do sistema:

- Um cliente pode possuir mais de uma conta.
- Cada conta deve ter um número identificador.
- Uma conta deve possuir um titular.
- Não será permitido depositar valores negativos ou iguais a zero.
- Não será permitido sacar mais do que o saldo disponível.
- Não será permitido transferir valores negativos ou iguais a zero.
- A conta de origem e a conta de destino devem existir.
- Uma transferência deve debitar o valor da conta de origem e creditar o valor na conta de destino.

Explique que essas regras representam requisitos funcionais. A equipe deverá transformá-las em código.

Não é necessário implementar autenticação, banco de dados, interface gráfica ou integração com sistemas externos. Os dados poderão permanecer em memória enquanto a aplicação estiver em execução.

## 5. Organização das equipes

Divida os participantes em trios.

Cada trio deverá possuir um repositório no GitHub. Se a turma tiver muitos participantes, é possível criar um repositório por trio ou utilizar um repositório central com uma organização previamente definida.

Apresente as três responsabilidades.

### Integrante 1 — Cliente.java

Responsável pela classe que representa o cliente do banco.

A classe deverá possuir:

- Identificador do cliente.
- Nome.
- CPF.
- Construtor.
- Getters e setters, quando necessários.
- Método para apresentar os dados do cliente.

Exemplo de estrutura:

```java
public class Cliente {

    private int id;
    private String nome;
    private String cpf;

    public Cliente(int id, String nome, String cpf) {
        this.id = id;
        this.nome = nome;
        this.cpf = cpf;
    }

    // Getters e demais métodos
}
```

**Tarefa do integrante:** criar a classe, implementar seus atributos e métodos e verificar se ela pode ser utilizada pelos demais componentes.

### Integrante 2 — Conta.java

Responsável pela classe que representa uma conta bancária.

A classe deverá possuir:

- Número da conta.
- Titular, representado por um objeto Cliente.
- Saldo.
- Construtor.
- Método depositar.
- Método sacar.
- Método consultarSaldo.

Exemplo de estrutura:

```java
public class Conta {

    private int numero;
    private Cliente titular;
    private BigDecimal saldo;

    public Conta(int numero, Cliente titular) {
        this.numero = numero;
        this.titular = titular;
        this.saldo = BigDecimal.ZERO;
    }

    // Métodos de operação e consulta
}
```

Explique que o saldo deve utilizar BigDecimal, e não double, para evitar problemas de precisão em operações monetárias.

**Tarefa do integrante:** implementar os métodos de depósito, saque e consulta de saldo, respeitando as regras do sistema.

### Integrante 3 — AplicacaoBanco.java

Responsável pela classe principal, que controla a execução da aplicação.

Deverá implementar:

- Método main.
- Menu de opções.
- Laço de repetição.
- Leitura das opções digitadas pelo usuário.
- Cadastro de clientes.
- Criação e armazenamento de contas.
- Pesquisa de contas.
- Execução das operações.
- Transferências entre contas.

O integrante deverá utilizar as classes Cliente e Conta, sem duplicar suas responsabilidades.

**Tarefa do integrante:** construir o fluxo da aplicação e integrar os componentes desenvolvidos pelos outros integrantes.

## 6. Definição dos contratos entre os integrantes

Antes de liberar o desenvolvimento, faça uma pausa para alinhar as classes.

Explique que equipes não devem começar a programar sem combinar como os componentes vão se comunicar.

Defina previamente:

- A classe Conta receberá um objeto Cliente no construtor.
- O método depositar receberá um BigDecimal.
- O método sacar receberá um BigDecimal.
- O método consultarSaldo retornará um BigDecimal.
- A classe AplicacaoBanco será responsável por localizar as contas e coordenar as transferências.
- A transferência utilizará os métodos de saque e depósito das contas.

Uma assinatura possível para os métodos da classe Conta:

```java
public void depositar(BigDecimal valor);

public void sacar(BigDecimal valor);

public BigDecimal consultarSaldo();
```

A classe AplicacaoBanco poderá manter uma coleção de contas:

```java
private List<Conta> contas = new ArrayList<>();
```

E deverá implementar métodos auxiliares, como:

```java
private Conta buscarConta(int numero);

private void transferir(
        int contaOrigem,
        int contaDestino,
        BigDecimal valor
);
```

**Ponto de atenção:** essas assinaturas são o contrato inicial da equipe. Se alguém decidir alterar um nome ou parâmetro, deverá comunicar os demais antes de realizar a mudança.

## 7. Preparação do trabalho colaborativo no GitHub

Cada trio deverá organizar o repositório.

Oriente os participantes a:

- Clonar o repositório no computador.
- Abrir o projeto no VS Code.
- Conferir a estrutura Maven.
- Verificar se o projeto compila.

Cada integrante deverá trabalhar em sua própria branch.

Exemplos:

```bash
git switch -c feature/cliente
git switch -c feature/conta
git switch -c feature/aplicacao-banco
```

Os nomes são apenas exemplos. Cada integrante deverá criar sua branch a partir da versão atualizada de main.

Oriente a turma a não trabalhar diretamente na branch main.

### 7.1. O ciclo de sincronização

Apresente a rotina de cada integrante:

| Momento | Comando |
|---|---|
| antes de começar algo novo | `git switch main` e `git pull` |
| durante o trabalho | `git push` da própria branch |
| a qualquer momento | `git fetch`, para ver o que os colegas enviaram |

Demonstre como examinar a branch de um colega sem sair da sua, usando as ferramentas da aula 3:

```bash
git fetch
git branch -a
git log --oneline main..origin/feature/conta
git diff --stat main...origin/feature/conta
```

**Ponto importante:** publicar não é integrar. Quando um colega diz "já subi", a branch dele está no GitHub, mas o trabalho só chega à main quando alguém o integra.

**Regra de ouro em equipe:** a main e as branches dos colegas nunca recebem `reset`, `rebase` ou `push --force`. Cada integrante só reorganiza a própria branch antes de pedir revisão.

## 8. Desenvolvimento prático — primeira etapa

**Tempo sugerido: 35 a 45 minutos.**

Libere o desenvolvimento das três classes.

Enquanto os integrantes trabalham, circule entre os trios e observe:

- Se cada integrante está trabalhando na branch correta.
- Se as responsabilidades estão sendo respeitadas.
- Se os contratos definidos estão sendo seguidos.
- Se as classes estão utilizando o mesmo pacote.
- Se o projeto continua compilando.
- Se as alterações estão sendo registradas com commits.

Cada integrante deverá realizar commits pequenos e objetivos.

Exemplos:

```bash
git add src/main/java
git commit -m "Cria classe Cliente"
git add src/main/java
git commit -m "Implementa operacoes da conta"
git add src/main/java
git commit -m "Cria menu principal da aplicacao"
```

Os commits devem representar alterações compreensíveis. Evite mensagens genéricas como alterações, teste ou final.

**Intervenção do instrutor:** se um integrante terminar antes dos demais, peça que revise sua implementação, identifique casos inválidos e prepare exemplos de utilização para os colegas.

## 9. Integração com Pull Requests

**Tempo sugerido: 25 a 35 minutos.**

Quando os integrantes terminarem suas primeiras implementações, inicie a integração.

Cada participante deverá:

- Publicar sua branch no GitHub.
- Abrir um Pull Request para main.
- Informar o objetivo da alteração.
- Descrever o que foi implementado.
- Indicar como testar.

Exemplo de título:

Implementa classe Conta

Exemplo de descrição:

```markdown
## Alterações
- Criada a classe Conta.
- Adicionado número e titular.
- Implementado depósito.
- Implementado saque.
- Implementada consulta de saldo.

## Como testar
- mvn compile termina em BUILD SUCCESS.
- Saque acima do saldo é recusado.
```

Explique que a seção de teste orienta o revisor e evita uma aprovação feita só pela leitura do código.

### 9.1. Code Review

Antes de aprovar o Pull Request, outro integrante deverá revisar o código.

Oriente os revisores a observar:

- Os atributos possuem nomes claros?
- Os métodos possuem responsabilidades bem definidas?
- Os parâmetros estão adequados?
- Há validações para valores inválidos?
- O código respeita os contratos definidos?
- A implementação está coerente com o requisito?
- Existem problemas evidentes de lógica?

O revisor poderá fazer comentários e solicitar alterações.

Explique a diferença entre comentar e aprovar. A aprovação indica que o revisor considera a implementação adequada para integração, mas não substitui a execução dos testes.

Após a revisão, o autor deverá corrigir o que for necessário, publicar as alterações e aguardar nova avaliação.

O Pull Request poderá então ser integrado à main.

**Regra para o exercício:** cada integrante deverá revisar pelo menos um Pull Request de outro integrante. Ninguém deverá aprovar automaticamente o próprio trabalho.

## 10. Integração da aplicação

### 10.1. Integrar pelo botão ou por merge local

Mostre as duas formas de integrar um Pull Request aprovado. A primeira é o botão **Merge pull request**, com **Create a merge commit**. A segunda é o merge local:

```bash
git switch main
git pull
git merge --no-ff feature/aplicacao-banco -m "Integra feature/aplicacao-banco"
mvn compile
git push
```

Explique:

- o `--no-ff` cria o commit de merge, como o botão;
- o `mvn compile` antes do `push` confere o conjunto integrado antes de publicá-lo;
- o GitHub marca o Pull Request como Merged sozinho, quando os commits chegam à main;
- se o `push` for recusado, alguém integrou algo antes: `git pull`, compilar de novo e repetir o `push`, nunca forçar.

Mostre também a direção contrária, usada pelo Integrante 3 antes da revisão final: trazer a main para a própria branch com `git merge main`, para testar a aplicação com as classes dos colegas.

### 10.2. Verificar o conjunto

Depois que as três classes forem integradas, o trio deverá atualizar sua branch main e verificar se o projeto compila.

```bash
git switch main
git pull origin main
mvn compile
```

Se houver problemas, a equipe deverá identificar se são erros de compilação, incompatibilidade entre assinaturas ou problemas de lógica.

Peça que observem especialmente:

- Se AplicacaoBanco consegue instanciar Cliente.
- Se Conta recebe corretamente o titular.
- Se os métodos utilizados na aplicação possuem as mesmas assinaturas definidas.
- Se todos os arquivos estão no pacote correto.
- Se não existem classes duplicadas ou versões conflitantes.

Explique que uma classe pode estar correta isoladamente e ainda assim apresentar problemas quando integrada às demais. Por isso, a integração precisa ser verificada.

## 11. Segunda etapa: fazer a aplicação funcionar

**Tempo sugerido: 25 a 35 minutos.**

Agora o trio deverá executar a aplicação e completar o fluxo de uso.

Solicite que implementem e testem, no mínimo:

- Cadastrar dois clientes.
- Criar uma conta para cada cliente.
- Consultar as contas cadastradas.
- Consultar uma conta pelo número.
- Depositar R$ 500,00 na primeira conta.
- Sacar R$ 100,00 da primeira conta.
- Transferir R$ 150,00 da primeira conta para a segunda.
- Consultar os saldos das duas contas.
- Tentar sacar um valor superior ao saldo.
- Tentar transferir para uma conta inexistente.

Resultado esperado para o cenário principal:

- Primeira conta: R$ 250,00.
- Segunda conta: R$ 150,00.

Os participantes deverão verificar se os valores finais correspondem às operações realizadas.

A equipe poderá dividir as correções entre os integrantes, mas as mudanças também deverão seguir o fluxo de branches e Pull Requests.

Não aceite como concluída uma funcionalidade que apenas aparece no menu. Ela precisa executar a operação e apresentar um resultado coerente.

## 12. Critérios de conclusão

O trio deverá demonstrar que:

- O projeto possui estrutura Maven.
- O pom.xml está configurado corretamente.
- As três classes foram implementadas.
- O menu permanece em execução até a opção de saída.
- É possível cadastrar clientes.
- É possível criar e consultar contas.
- É possível depositar e sacar.
- É possível transferir valores.
- As validações básicas funcionam.
- Cada integrante utilizou sua própria branch.
- As alterações foram publicadas no GitHub.
- Foram abertos Pull Requests.
- Houve revisão de código por outro integrante.
- A integração foi realizada na main.
- A aplicação compila e executa.

Não é necessário que todos os trios concluam todas as funcionalidades para que a aula tenha sido produtiva. Entretanto, todos devem ter vivenciado o processo de desenvolvimento em equipe e integração por Pull Request.

## 13. Encerramento e revisão

Finalize perguntando:

- Qual é a diferença entre uma branch e um repositório?
- Por que não devemos desenvolver diretamente na main?
- Qual é a função de um Pull Request?
- O que acontece durante um Code Review?
- Qual a diferença entre `git fetch` e `git pull` quando várias pessoas usam o mesmo repositório?
- Por que publicar uma branch não é o mesmo que integrá-la?
- Como integrar um Pull Request com merge local?
- Qual é a função do pom.xml?
- Por que o Maven estabelece uma estrutura padrão de diretórios?
- Por que uma aplicação pode compilar individualmente e apresentar problemas depois da integração?

Retome a ideia principal: desenvolver software em equipe exige mais do que escrever código. É necessário organizar responsabilidades, estabelecer contratos, comunicar mudanças, revisar implementações e integrar o trabalho de maneira controlada.

## Próximos passos depois do módulo

Na continuação do curso, o foco será aprofundar o Maven.

A turma utilizará o mesmo projeto para trabalhar:

- Dependências externas.
- Maven Central.
- Repositório local.
- Escopos de dependências.
- Ciclo de vida do Maven.
- Testes automatizados.
- Empacotamento da aplicação em JAR.
- Execução e integração com o VS Code.

A aplicação bancária será a base prática para demonstrar como um projeto Java deixa de ser apenas um conjunto de classes e passa a ter uma estrutura de construção, dependências e testes mais próxima da utilizada em projetos profissionais.

## Notas para a condução

**Minha recomendação para conduzir a atividade**

Eu faria uma alteração na dinâmica: não deixe os três integrantes trabalharem isoladamente até o final para só então juntar tudo.

O ideal é estabelecer três momentos de integração:

- **Contrato:** os três definem juntos os atributos e as assinaturas dos métodos.
- **Desenvolvimento:** cada um implementa sua classe em uma branch.
- **Integração:** fazem Pull Requests, revisam e testam o conjunto.

Isso permite observar um problema muito comum em equipes: o desenvolvedor A implementa um método chamado sacar, enquanto o desenvolvedor B espera encontrar um método chamado realizarSaque. O código de ambos pode estar correto isoladamente, mas a integração falha.

Também recomendo que você forneça aos trios o pom.xml e a estrutura inicial do projeto já preparados. A aula 4 tem bastante conteúdo; gastar tempo excessivo com configuração inicial reduziria o tempo de desenvolvimento colaborativo. O foco deve ser GitHub, revisão e integração.
