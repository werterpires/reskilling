# Exercícios — Tópico 1: Preparar o ambiente Java

## Projeto prático: Gestor de Tarefas

Nesta sequência, você montará a base de uma aplicação de **Gestão de Tarefas**. Ao final, ela ainda não cadastrará tarefas, não pedirá dados no teclado e não decidirá o que fazer com cada item. Esses assuntos dependem de conteúdos que ainda serão estudados.

Mesmo assim, o projeto será real: haverá uma estrutura organizada de pastas, um pacote Java, uma classe inicial, mensagens do sistema, compilação e execução. Cada exercício acrescenta uma parte do mesmo programa.

> Trabalhe no **computador físico**, não na VDI. Se algum comando não apresentar o resultado esperado, pare e procure o instrutor com a mensagem exibida no terminal.

## Resultado final esperado

Ao concluir todos os exercícios, o terminal deverá exibir uma tela semelhante a esta:

```text
=== GESTOR DE TAREFAS ===
Ambiente Java configurado com sucesso.

Tarefas planejadas para hoje:
1. Revisar requisitos do projeto
2. Implementar a tela inicial
3. Validar a execução da aplicação

Status inicial: tarefas aguardando evolução do sistema.
```

Por enquanto, essa lista é fixa, escrita diretamente no código. Ela serve para praticar o ciclo completo do Java:

```text
editar o arquivo .java → compilar com javac → executar com java
```

Em COBOL, você já conhece a ideia: alterar o programa-fonte, compilar e executar novamente. Em Java, a intenção é a mesma; mudam a estrutura do código e os comandos usados.

---

## Antes de começar

Abra o **Prompt de Comando** e execute:

```bat
java -version
javac -version
```

Os dois resultados devem indicar a versão principal `21`.

Em seguida, confirme a instalação encontrada pelo Windows:

```bat
echo %JAVA_HOME%
where java
where javac
```

Use esta tabela para interpretar os comandos:

| Comando | O que confirma |
|---|---|
| `java -version` | A ferramenta usada para executar programas Java |
| `javac -version` | O compilador Java |
| `echo %JAVA_HOME%` | A pasta principal configurada para o JDK |
| `where java` | O arquivo `java.exe` escolhido pelo Windows |
| `where javac` | O arquivo `javac.exe` escolhido pelo Windows |

Não avance se `java` ou `javac` não forem reconhecidos. A configuração do ambiente faz parte da prática.

---

## Exercício 1 — Criar o espaço do projeto

Crie uma pasta chamada:

```text
gestor-tarefas
```

Abra o VS Code e use **File > Open Folder** para abrir essa pasta inteira. Não abra apenas um arquivo isolado: a pasta será o espaço de trabalho do projeto.

Dentro de `gestor-tarefas`, crie a seguinte estrutura:

```text
gestor-tarefas/
└── src/
    └── br/
        └── com/
            └── curso/
                └── tarefas/
```

### O que essa estrutura representa

As pastas `br/com/curso/tarefas` formarão o pacote Java `br.com.curso.tarefas`. Ainda não é necessário dominar pacotes; por enquanto, basta preservar a correspondência:

```text
br\com\curso\tarefas  ↔  br.com.curso.tarefas
```

As barras são usadas para representar pastas. Os pontos serão usados no nome do pacote dentro do Java.

### Verificação

No explorador de arquivos do VS Code, confirme que a pasta `src` existe e que, dentro dela, você consegue chegar até `tarefas`.

---

## Exercício 2 — Criar a classe que inicia a aplicação

Dentro da pasta `tarefas`, crie o arquivo:

```text
AplicacaoGestorTarefas.java
```

Escreva o código abaixo:

```java
package br.com.curso.tarefas;

public class AplicacaoGestorTarefas {

    public static void main(String[] args) {
    }
}
```

### O que acabou de ser criado

| Parte | Papel neste momento |
|---|---|
| `package br.com.curso.tarefas;` | Declara a organização da classe dentro do pacote |
| `public class AplicacaoGestorTarefas` | Cria a classe inicial da aplicação |
| `main` | Indica o ponto pelo qual a JVM iniciará o programa |

O nome da classe pública e o nome do arquivo devem ser idênticos:

```text
AplicacaoGestorTarefas.java
AplicacaoGestorTarefas
```

Não altere maiúsculas, minúsculas nem a grafia. Em Java, esses nomes são diferentes caso a capitalização mude.

---

## Exercício 3 — Compilar a estrutura inicial

Abra **Terminal > New Terminal** no VS Code. Confirme que o terminal está na pasta `gestor-tarefas`.

Crie a pasta que receberá o resultado da compilação:

```bat
mkdir out
```

Agora compile o arquivo:

```bat
javac -d out src\br\com\curso\tarefas\AplicacaoGestorTarefas.java
```

Se nenhum erro aparecer, a compilação foi concluída.

### O que o comando fez

| Parte | Significado |
|---|---|
| `javac` | Chama o compilador Java |
| `-d out` | Indica que o resultado será colocado na pasta `out` |
| caminho terminado em `.java` | Informa qual código-fonte será compilado |

Verifique se a seguinte estrutura foi criada:

```text
out/
└── br/
    └── com/
        └── curso/
            └── tarefas/
                └── AplicacaoGestorTarefas.class
```

O arquivo `.java` é o fonte que você edita. O arquivo `.class` é o bytecode produzido pelo compilador para ser executado pela JVM.

---

## Exercício 4 — Executar uma aplicação ainda sem mensagens

Execute a classe compilada:

```bat
java -cp out br.com.curso.tarefas.AplicacaoGestorTarefas
```

Não haverá mensagem no terminal, e isso é esperado. O `main` existe, a JVM encontrou a classe e a execução terminou sem realizar nenhuma ação visível.

Esse primeiro teste confirma duas coisas importantes:

1. o arquivo foi compilado;
2. a JVM consegue localizar e iniciar a classe pelo nome completo do pacote.

No comando de execução, não escrevemos `.class`. Também usamos pontos no nome da classe:

```text
br.com.curso.tarefas.AplicacaoGestorTarefas
```

---

## Exercício 5 — Exibir a abertura do gestor de tarefas

Agora transforme a aplicação silenciosa na tela inicial do gestor. Dentro do método `main`, escreva duas linhas:

```java
package br.com.curso.tarefas;

public class AplicacaoGestorTarefas {

    public static void main(String[] args) {
        System.out.println("=== GESTOR DE TAREFAS ===");
        System.out.println("Ambiente Java configurado com sucesso.");
    }
}
```

Em COBOL, a primeira intenção poderia ser escrita assim:

```cobol
DISPLAY "=== GESTOR DE TAREFAS ==="
```

Em Java, usamos:

```java
System.out.println("=== GESTOR DE TAREFAS ===");
```

Nos dois casos, a finalidade é exibir uma mensagem. Neste tópico, leia `System.out.println` como “exiba esta linha”.

Compile novamente:

```bat
javac -d out src\br\com\curso\tarefas\AplicacaoGestorTarefas.java
```

Depois execute:

```bat
java -cp out br.com.curso.tarefas.AplicacaoGestorTarefas
```

Resultado esperado:

```text
=== GESTOR DE TAREFAS ===
Ambiente Java configurado com sucesso.
```

> Sempre que alterar o arquivo `.java`, compile antes de executar pelo terminal. Salvar o arquivo não atualiza automaticamente o `.class`.

---

## Exercício 6 — Mostrar tarefas planejadas

Acrescente uma linha em branco e três tarefas planejadas. O código deve ficar assim:

```java
package br.com.curso.tarefas;

public class AplicacaoGestorTarefas {

    public static void main(String[] args) {
        System.out.println("=== GESTOR DE TAREFAS ===");
        System.out.println("Ambiente Java configurado com sucesso.");
        System.out.println();
        System.out.println("Tarefas planejadas para hoje:");
        System.out.println("1. Revisar requisitos do projeto");
        System.out.println("2. Implementar a tela inicial");
        System.out.println("3. Validar a execução da aplicação");
    }
}
```

`System.out.println();`, sem texto entre parênteses, apenas pula uma linha no terminal. Ele deixa a saída mais organizada.

Compile e execute novamente. A saída esperada agora é:

```text
=== GESTOR DE TAREFAS ===
Ambiente Java configurado com sucesso.

Tarefas planejadas para hoje:
1. Revisar requisitos do projeto
2. Implementar a tela inicial
3. Validar a execução da aplicação
```

Neste momento, as tarefas ainda não são registros armazenados pelo programa. São mensagens fixas que representam a tela inicial do sistema. Isso é intencional: cadastro, armazenamento e regras virão quando esses recursos forem estudados.

---

## Exercício 7 — Acrescentar o status inicial do sistema

Depois da terceira tarefa, inclua uma linha em branco e a mensagem de status:

```java
System.out.println();
System.out.println("Status inicial: tarefas aguardando evolução do sistema.");
```

O método `main` deverá conter todas as mensagens abaixo, na ordem:

```java
public static void main(String[] args) {
    System.out.println("=== GESTOR DE TAREFAS ===");
    System.out.println("Ambiente Java configurado com sucesso.");
    System.out.println();
    System.out.println("Tarefas planejadas para hoje:");
    System.out.println("1. Revisar requisitos do projeto");
    System.out.println("2. Implementar a tela inicial");
    System.out.println("3. Validar a execução da aplicação");
    System.out.println();
    System.out.println("Status inicial: tarefas aguardando evolução do sistema.");
}
```

Compile e execute outra vez. Compare cada linha exibida com o texto escrito dentro de cada `println`.

Essa comparação ajuda a interpretar o programa: por enquanto, a execução segue a sequência em que as mensagens aparecem no `main`.

---

## Exercício 8 — Executar pelo VS Code

Com o **Extension Pack for Java** instalado, abra `AplicacaoGestorTarefas.java` no VS Code. Próximo ao método `main`, deve aparecer a opção **Run**.

1. Clique em **Run**.
2. Observe a saída no terminal integrado.
3. Compare o resultado com a execução manual feita pelo comando `java -cp out ...`.

O resultado deve ser equivalente: o VS Code automatiza a compilação e a execução, mas continua usando o JDK configurado no computador.

Se a opção **Run** não aparecer, confirme:

- se `java -version` funciona no terminal;
- se `javac -version` funciona no terminal;
- se o Extension Pack for Java está instalado;
- se a pasta `gestor-tarefas` está aberta no VS Code;
- se o JDK 21 aparece em **Java: Configure Java Runtime**.

---

## Exercício 9 — Personalizar sem mudar a estrutura

Faça uma pequena personalização, mantendo a estrutura já construída.

1. Troque uma das três tarefas por uma atividade que faria sentido no seu trabalho.
2. Troque a segunda mensagem de abertura por uma frase curta que identifique a sua versão do gestor.
3. Compile novamente com `javac`.
4. Execute pelo terminal com `java`.
5. Execute também pelo botão **Run** do VS Code.

Exemplo de alteração possível:

```java
System.out.println("2. Conferir pendências da equipe");
```

O objetivo não é criar uma nova regra. É praticar a alteração de um arquivo-fonte e comprovar que a nova versão foi compilada e executada.

---

## Exercício 10 — Conferência final do projeto

Antes de encerrar, confirme todos os itens abaixo.

- [ ] A pasta principal se chama `gestor-tarefas`.
- [ ] O arquivo está em `src\br\com\curso\tarefas\AplicacaoGestorTarefas.java`.
- [ ] A primeira linha do arquivo declara `package br.com.curso.tarefas;`.
- [ ] A classe pública se chama `AplicacaoGestorTarefas`.
- [ ] A classe possui o método `main`.
- [ ] O programa exibe o título do gestor de tarefas.
- [ ] O programa exibe três tarefas planejadas.
- [ ] O programa exibe a mensagem de status inicial.
- [ ] A compilação gera `AplicacaoGestorTarefas.class` dentro da pasta `out`.
- [ ] O programa funciona pelo terminal.
- [ ] O programa funciona pelo VS Code.

## Entrega

Entregue a pasta inteira `gestor-tarefas`, preservando `src` e `out`.

O resultado desta etapa é a **estrutura inicial** do gestor de tarefas. O sistema ainda não administra tarefas de verdade porque, até aqui, vimos apenas preparação do ambiente, classes, pacotes, mensagens, compilação e execução. Nos próximos tópicos, a aplicação poderá crescer sem abandonar essa base.

## Comandos de referência

Use estes comandos a partir da pasta `gestor-tarefas`:

```bat
javac -d out src\br\com\curso\tarefas\AplicacaoGestorTarefas.java
java -cp out br.com.curso.tarefas.AplicacaoGestorTarefas
```

