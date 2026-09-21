# Tópico 1 — Preparar o ambiente Java

## Sua experiência em COBOL continua com você

Mudar de stack pode causar a sensação de que será necessário começar novamente. Não será. Você já conhece lógica, regras de negócio, sistemas corporativos, compilação, análise de erros e responsabilidade sobre dados importantes. Nada disso deixa de valer porque a linguagem mudou.

O que começa agora é a aprendizagem de uma nova forma de organizar o código e de um novo conjunto de ferramentas. Alguns nomes serão diferentes, mas várias ideias serão familiares. Você continuará escrevendo arquivos-fonte, utilizando um compilador, executando programas e interpretando mensagens de erro. A diferença é que, no Java, esses elementos aparecem com outros comandos e dentro de outro ecossistema.

Neste primeiro tópico, você preparará o computador e fará um pequeno aplicativo de **Gestão de Biblioteca** funcionar. Ele ainda não terá cadastro, empréstimo, devolução ou regras de negócio. Por enquanto, nosso objetivo é mais simples:

```text
Escrever um arquivo Java → compilar → executar → ver o resultado
```

Ao terminar, você terá percorrido o ciclo completo de um programa Java. É um primeiro passo pequeno, mas concreto.

## O que você fará neste tópico

- entenderá o que é o JDK;
- instalará o Java 21;
- conhecerá os comandos `java` e `javac`;
- configurará `JAVA_HOME` e `PATH`;
- validará o ambiente pelo terminal;
- configurará o Visual Studio Code;
- conhecerá IntelliJ IDEA e Eclipse como alternativas;
- criará um programa simples do projeto Gestão de Biblioteca;
- compilará e executará esse programa.

> Não vamos estudar lógica de programação neste tópico. O código servirá para mostrar como as ferramentas trabalham juntas.

---

## 1. Antes de instalar: o que é o JDK?

Para criar programas Java, você precisa instalar o **JDK**, sigla de *Java Development Kit*, ou Kit de Desenvolvimento Java.

Pense no JDK como uma caixa de ferramentas. Dentro dela existem programas que permitem:

- compilar código Java;
- executar aplicações Java;
- gerar documentação;
- empacotar aplicações;
- analisar e diagnosticar programas.

Neste tópico, usaremos principalmente duas ferramentas dessa caixa:

| Ferramenta | Para que serve |
|---|---|
| `javac` | Compilar um arquivo-fonte Java |
| `java` | Executar uma aplicação Java |

Se você já trabalhou com compilação em COBOL, a ideia principal não é nova. Primeiro existe um arquivo-fonte legível por pessoas. Depois, um compilador transforma esse fonte em outro formato, que poderá ser executado pelo ambiente adequado.

No Java, o fluxo será:

```text
Código-fonte                 Compilador                 Bytecode
AplicacaoBiblioteca.java  →  javac  →  AplicacaoBiblioteca.class
```

Depois, o bytecode será executado pela Máquina Virtual Java:

```text
AplicacaoBiblioteca.class  →  JVM  →  programa funcionando
```

### JDK, JVM e JRE sem complicação

Você encontrará três siglas com frequência:

- **JDK:** conjunto completo para desenvolver aplicações Java;
- **JVM:** Máquina Virtual Java, responsável por executar o bytecode;
- **JRE:** ambiente necessário para executar aplicações Java.

Neste curso, o mais importante agora é lembrar:

> Para desenvolver, precisamos do **JDK**.

O JDK já fornece as ferramentas de desenvolvimento e os componentes necessários para executar o programa. Não é preciso instalar separadamente um JRE para acompanhar o curso.

### O que é bytecode?

Em Java, o compilador normalmente não gera diretamente um executável `.exe` do Windows. Ele gera um arquivo `.class` contendo bytecode. Esse bytecode é preparado para ser entendido pela JVM.

Isso cria uma camada entre o programa e o sistema operacional:

```text
Seu código Java → bytecode → JVM → sistema operacional
```

Essa camada ajuda a explicar a portabilidade do Java. Uma aplicação compilada pode ser executada em diferentes sistemas que possuam uma JVM compatível. Existem detalhes e exceções em projetos reais, mas esse modelo é suficiente para começarmos.

---

## 2. Instalar o Java 21

Utilizaremos a seguinte configuração:

- **Java 21 LTS**;
- distribuição **Eclipse Temurin**;
- pacote **JDK**;
- sistema **Windows 11 Enterprise de 64 bits**;
- instalador **MSI para Windows x64**.

LTS significa *Long-Term Support*, ou suporte de longo prazo. Versões LTS costumam ser escolhidas em ambientes corporativos porque permanecem como referência por mais tempo.

### Instale no computador físico

Faça a instalação no **computador físico**, não dentro da VDI. O ambiente local será utilizado durante a trilha. Se alguma tela estiver diferente, surgir uma restrição de permissão ou o resultado não corresponder ao material, procure o instrutor.

### Por que Eclipse Temurin?

Existem diferentes distribuições do JDK: Eclipse Temurin, Oracle JDK, Amazon Corretto, Microsoft Build of OpenJDK, IBM Semeru e outras. Elas fazem parte do mesmo ecossistema Java, mas são mantidas por organizações diferentes.

Usaremos o Eclipse Temurin para que todo o grupo trabalhe com a mesma distribuição e a mesma versão. Essa padronização simplifica a investigação de erros: quando todos partem da mesma configuração, existem menos diferenças escondidas entre as máquinas.

### Passos da instalação

O guia de pré-curso apresenta o processo completo com imagens. O roteiro geral é:

1. acessar o site do Eclipse Adoptium;
2. escolher **JDK 21 — LTS**;
3. selecionar **Windows**;
4. selecionar a arquitetura **x64**;
5. baixar o instalador **MSI**;
6. executar o arquivo baixado;
7. aceitar os termos da licença;
8. manter a pasta de instalação;
9. concluir o assistente.

Depois da instalação, você encontrará uma pasta semelhante a esta:

```text
C:\Program Files\Eclipse Adoptium\jdk-21.0.x.x-hotspot
```

Os números representados por `x` variam de acordo com a atualização instalada. Seu caminho não precisa ser idêntico ao do exemplo. O importante é que a pasta comece com `jdk-21` e esteja dentro de `Eclipse Adoptium`.

Dentro dela haverá uma pasta chamada `bin`:

```text
C:\Program Files\Eclipse Adoptium\jdk-21.0.x.x-hotspot\bin
```

É nessa pasta que estão `java.exe`, `javac.exe` e outras ferramentas do JDK.

Neste ponto, os arquivos existem no computador. O próximo passo é aprender a chamá-los pelo terminal.

---

## 3. Conhecer `java` e `javac`

Abra o menu Iniciar, pesquise por `cmd` e abra o **Prompt de Comando**.

Digite:

```bat
java -version
```

Esse comando pede ao executável `java` que informe sua versão. Se o Windows já souber onde ele está, você verá uma resposta contendo a versão principal `21`.

Agora digite:

```bat
javac -version
```

Esse segundo comando pede ao compilador `javac` que informe sua versão. O resultado também deve começar com `21`.

Os dois comandos verificam ferramentas diferentes:

- `java -version` verifica a ferramenta que inicia a execução;
- `javac -version` verifica o compilador.

Agora faz sentido testar ambos. Precisamos executar programas, mas também precisamos compilar os arquivos que escreveremos.

Se os comandos funcionaram, ótimo: o instalador provavelmente já configurou parte do ambiente. Mesmo assim, aprenderemos `JAVA_HOME` e `PATH`, porque essas configurações aparecem em projetos, IDEs, ferramentas de build, servidores e rotinas de suporte.

Se o Windows informou que `java` ou `javac` “não é reconhecido como um comando”, não significa que você falhou nem que será necessário reinstalar imediatamente. Na maioria das vezes, o JDK está instalado, mas o Windows ainda não sabe em qual pasta deve procurar o comando. É exatamente isso que resolveremos a seguir.

---

## 4. Configurar `JAVA_HOME`

Uma variável de ambiente é um valor nomeado que pode ser consultado pelo Windows e pelos programas executados nele. Em vez de cada ferramenta tentar adivinhar onde o JDK foi instalado, podemos registrar essa informação em uma variável chamada `JAVA_HOME`.

O nome será:

```text
JAVA_HOME
```

O valor será a pasta principal do JDK. Exemplo:

```text
C:\Program Files\Eclipse Adoptium\jdk-21.0.x.x-hotspot
```

Não coloque `\bin` no final do `JAVA_HOME`. A variável aponta para o JDK inteiro, não apenas para a pasta de executáveis.

### Configuração correta

```text
JAVA_HOME=C:\Program Files\Eclipse Adoptium\jdk-21.0.x.x-hotspot
```

### Configuração incorreta

```text
JAVA_HOME=C:\Program Files\Eclipse Adoptium\jdk-21.0.x.x-hotspot\bin
```

### Criar a variável no Windows

1. Abra o menu Iniciar.
2. Pesquise por **variáveis de ambiente**.
3. Abra **Editar as variáveis de ambiente do sistema**.
4. Na guia **Avançado**, clique em **Variáveis de Ambiente**.
5. Na área **Variáveis de usuário**, clique em **Novo**.
6. Em **Nome da variável**, informe `JAVA_HOME`.
7. Em **Valor da variável**, informe o caminho da pasta principal do JDK.
8. Confirme em **OK**.

Depois de concluir, abra um **novo** Prompt de Comando. Um terminal que já estava aberto pode continuar usando os valores antigos.

Execute:

```bat
echo %JAVA_HOME%
```

O comando `echo` exibe um valor no terminal. Em COBOL, você já encontrou a mesma intenção no comando `DISPLAY`.

Compare:

```cobol
DISPLAY "AMBIENTE CONFIGURADO"
```

```bat
echo Ambiente configurado
```

Aqui não estamos comparando linguagens de programação diretamente; o segundo exemplo é um comando do terminal. A semelhança está na finalidade: exibir uma informação.

Ao executar `echo %JAVA_HOME%`, os símbolos `%` pedem ao Windows o valor guardado na variável `JAVA_HOME`.

Resultado esperado:

```text
C:\Program Files\Eclipse Adoptium\jdk-21.0.x.x-hotspot
```

Se o terminal mostrar literalmente `%JAVA_HOME%`, a variável não está disponível nessa janela. Confirme a configuração e abra outro Prompt de Comando.

---

## 5. Configurar o `PATH`

Agora o Windows já possui uma variável que informa onde está o JDK. Falta dizer em qual pasta estão os comandos executáveis.

O `PATH` é uma lista de pastas nas quais o Windows procura programas quando você digita um comando. Ao escrever:

```bat
java -version
```

o Windows procura por `java.exe` nas pastas cadastradas no `PATH`.

Como `java.exe` e `javac.exe` estão dentro da pasta `bin`, adicionaremos esta entrada:

```text
%JAVA_HOME%\bin
```

Observe como as duas configurações trabalham juntas:

| Configuração | Informação guardada |
|---|---|
| `JAVA_HOME` | Onde está a pasta principal do JDK |
| `%JAVA_HOME%\bin` no `PATH` | Onde estão os comandos do JDK |

### Adicionar ao `PATH`

1. Volte à janela **Variáveis de Ambiente**.
2. Na área **Variáveis de usuário**, selecione `Path`.
3. Clique em **Editar**.
4. Clique em **Novo**.
5. Adicione `%JAVA_HOME%\bin`.
6. Confirme todas as janelas em **OK**.

Feche o Prompt de Comando e abra outro. Então execute novamente:

```bat
java -version
```

```bat
javac -version
```

Agora os dois devem responder com a versão `21`.

### Descobrir qual arquivo o Windows encontrou

O comando abaixo mostra o caminho do `java.exe` encontrado:

```bat
where java
```

Para localizar o compilador:

```bat
where javac
```

O resultado deve apontar para a instalação do Eclipse Adoptium. Exemplo:

```text
C:\Program Files\Eclipse Adoptium\jdk-21.0.x.x-hotspot\bin\java.exe
```

Esses comandos serão úteis se houver mais de um Java instalado. O Windows usa a primeira ocorrência adequada encontrada no `PATH`. Se aparecer uma versão diferente de `21`, não remova pastas aleatoriamente; mostre os resultados ao instrutor.

### Validação completa do Java 21

Execute os comandos na ordem:

```bat
echo %JAVA_HOME%
java -version
javac -version
where java
where javac
```

Com isso, você confirma:

1. onde está o JDK;
2. qual versão será executada;
3. qual versão será usada para compilar;
4. quais arquivos o Windows encontrou.

Seu ambiente Java básico está pronto.

---

## 6. Preparar o Visual Studio Code

É possível escrever Java em um editor de texto simples, compilar pelo terminal e executar com a JVM. Porém, um editor preparado para Java facilita a navegação, destaca erros, completa nomes e oferece depuração.

Usaremos o **Visual Studio Code**, também chamado de VS Code.

O VS Code é um editor extensível. Isso significa que instalamos extensões para acrescentar suporte a diferentes linguagens. Para trabalhar com Java, instalaremos o pacote oficial recomendado pela Microsoft.

### Instalar o Extension Pack for Java

1. Abra o VS Code.
2. Clique no ícone **Extensions** na barra lateral.
3. Você também pode usar `Ctrl+Shift+X`.
4. Pesquise por **Extension Pack for Java**.
5. Confirme que o fornecedor é **Microsoft**.
6. Clique em **Install**.

Esse pacote reúne extensões para:

- compreender a linguagem Java;
- executar e depurar programas;
- trabalhar com testes;
- gerenciar projetos;
- integrar o editor ao Maven.

Não precisaremos usar todos esses recursos agora. Neste tópico, queremos apenas que o VS Code reconheça o Java 21 e execute nosso primeiro programa.

### Confirmar o JDK no VS Code

Abra a Paleta de Comandos com:

```text
Ctrl+Shift+P
```

Pesquise por:

```text
Java: Configure Java Runtime
```

Confirme que o JDK 21 aparece na lista. Se não aparecer, feche todas as janelas do VS Code e abra-o novamente. Isso permite que o editor carregue as variáveis de ambiente atualizadas.

### E o IntelliJ IDEA e o Eclipse?

O VS Code não é a única ferramenta para desenvolver em Java.

- **IntelliJ IDEA:** IDE muito utilizada em projetos Java, conhecida por seus recursos de navegação, análise e refatoração.
- **Eclipse IDE:** ambiente tradicional do ecossistema Java, presente há muitos anos em projetos corporativos.
- **Visual Studio Code:** editor mais leve que recebe suporte a Java por meio de extensões.

Aprender Java não significa ficar preso a uma IDE. Os fundamentos são os mesmos. Todas essas ferramentas utilizam um JDK e automatizam tarefas como compilação e execução.

Começaremos com o VS Code porque ele permite enxergar o processo aos poucos. Primeiro entenderemos os comandos; depois aproveitaremos os atalhos do editor.

---

## 7. Criar o projeto Gestão de Biblioteca

Crie uma pasta chamada:

```text
gestao-biblioteca
```

No VS Code, clique em **File > Open Folder** e abra essa pasta.

Abra a pasta inteira, não apenas um arquivo isolado. Quando você abre a pasta, o VS Code consegue tratar o conteúdo como um espaço de trabalho e organizar melhor os recursos Java.

Dentro dela, crie a seguinte estrutura:

```text
gestao-biblioteca/
└── src/
    └── br/
        └── com/
            └── curso/
                └── biblioteca/
                    └── AplicacaoBiblioteca.java
```

Parece uma sequência longa de pastas, mas ela possui uma ideia simples: o arquivo Java ficará dentro de um **pacote** chamado `br.com.curso.biblioteca`.

Você ainda não precisa dominar pacotes. Por enquanto, guarde apenas duas informações:

- pacotes ajudam a organizar as classes;
- a estrutura das pastas acompanha o nome do pacote.

Desde o início, nosso projeto não será formado por vários arquivos soltos em uma única pasta. Conforme o sistema crescer, criaremos classes e pacotes com responsabilidades diferentes.

---

## 8. Escrever o primeiro programa Java

Abra `AplicacaoBiblioteca.java` e escreva:

```java
package br.com.curso.biblioteca;

public class AplicacaoBiblioteca {

    public static void main(String[] args) {
        System.out.println("Sistema de Gestão de Biblioteca");
        System.out.println("Ambiente Java configurado com sucesso.");
    }
}
```

Não tente memorizar tudo agora. Vamos observar somente o necessário para executar o programa.

### `package`

```java
package br.com.curso.biblioteca;
```

Essa linha declara o pacote da classe. Ela corresponde às pastas:

```text
br\com\curso\biblioteca
```

### A classe

```java
public class AplicacaoBiblioteca {
```

Essa linha cria a classe `AplicacaoBiblioteca`. O arquivo possui exatamente o mesmo nome da classe pública:

```text
AplicacaoBiblioteca.java
```

Em Java, letras maiúsculas e minúsculas fazem diferença. `AplicacaoBiblioteca`, `aplicacaoBiblioteca` e `APLICACAOBIBLIOTECA` são nomes diferentes.

### O ponto de entrada

```java
public static void main(String[] args) {
```

O método `main` é o ponto pelo qual iniciaremos o programa. Ele cumpre uma função comparável ao ponto inicial de execução de um programa COBOL. Não precisamos desmontar cada palavra da assinatura agora. Neste momento, basta reconhecer: quando a JVM inicia essa classe, é o `main` que ela procura.

### Exibir uma mensagem: `DISPLAY` e `println`

Em COBOL, uma mensagem poderia ser exibida assim:

```cobol
DISPLAY "SISTEMA DE GESTAO DE BIBLIOTECA"
```

Em Java, usaremos:

```java
System.out.println("Sistema de Gestão de Biblioteca");
```

Os dois comandos cumprem, neste exemplo, a mesma finalidade: enviar uma mensagem para a saída padrão.

Você pode ler `System.out.println` inicialmente como “exiba esta linha”. Depois estudaremos o significado de `System`, `out`, o ponto e a chamada de método. Não precisamos abrir todos esses conceitos antes de ver o programa funcionar.

Essa forma de avançar será importante durante a mudança de stack: primeiro você reconhece a finalidade; depois aprende como o Java organiza e nomeia cada parte.

### Uma comparação completa

Um programa COBOL mínimo poderia ser apresentado assim:

```cobol
IDENTIFICATION DIVISION.
PROGRAM-ID. APLICACAO-BIBLIOTECA.

PROCEDURE DIVISION.
    DISPLAY "SISTEMA DE GESTAO DE BIBLIOTECA".
    DISPLAY "AMBIENTE CONFIGURADO COM SUCESSO".
    STOP RUN.
```

Nosso programa Java é:

```java
package br.com.curso.biblioteca;

public class AplicacaoBiblioteca {

    public static void main(String[] args) {
        System.out.println("Sistema de Gestão de Biblioteca");
        System.out.println("Ambiente Java configurado com sucesso.");
    }
}
```

Não são traduções perfeitas linha por linha. A comparação serve para mostrar que você já conhece a intenção do programa: iniciar, exibir duas mensagens e terminar. A novidade está na estrutura usada para expressar essa intenção.

---

## 9. Compilar com `javac`

Agora transformaremos o arquivo `.java` em um arquivo `.class`.

No VS Code, abra **Terminal > New Terminal**. O terminal deve estar na pasta `gestao-biblioteca`.

Crie uma pasta para receber o resultado da compilação:

```bat
mkdir out
```

`mkdir` significa *make directory*: criar diretório. A pasta `out` guardará o código compilado.

Execute:

```bat
javac -d out src\br\com\curso\biblioteca\AplicacaoBiblioteca.java
```

Leia o comando em partes:

| Parte | Significado |
|---|---|
| `javac` | Chama o compilador Java |
| `-d out` | Envia o resultado para a pasta `out` |
| caminho terminado em `.java` | Indica o arquivo que será compilado |

Se o terminal não exibir erros, a compilação foi concluída. Dentro de `out`, aparecerá:

```text
out/
└── br/
    └── com/
        └── curso/
            └── biblioteca/
                └── AplicacaoBiblioteca.class
```

Agora existem dois arquivos com papéis diferentes:

| Arquivo | Papel |
|---|---|
| `AplicacaoBiblioteca.java` | Código-fonte que você lê e altera |
| `AplicacaoBiblioteca.class` | Bytecode produzido pelo compilador |

Em COBOL, você também distingue o fonte do artefato produzido pela compilação. Os nomes e formatos mudaram, mas a separação continua familiar.

---

## 10. Executar com `java`

Com o arquivo compilado, execute:

```bat
java -cp out br.com.curso.biblioteca.AplicacaoBiblioteca
```

Resultado esperado:

```text
Sistema de Gestão de Biblioteca
Ambiente Java configurado com sucesso.
```

Leia o comando por partes:

| Parte | Significado |
|---|---|
| `java` | Inicia a execução pela JVM |
| `-cp out` | Informa onde estão as classes compiladas |
| `br.com.curso.biblioteca.AplicacaoBiblioteca` | Nome completo da classe que será iniciada |

Na execução, não escrevemos `.class`. Também trocamos as barras do caminho por pontos porque estamos informando o nome completo da classe e de seu pacote.

Você acabou de completar o ciclo:

```text
AplicacaoBiblioteca.java
        ↓ javac
AplicacaoBiblioteca.class
        ↓ java / JVM
Mensagens exibidas no terminal
```

Esse é o resultado principal do Tópico 1: o ambiente está instalado, configurado e comprovadamente funcional.

---

## 11. Executar pelo VS Code

Com o Extension Pack for Java funcionando, o VS Code exibirá a opção **Run** próxima ao método `main`.

Clique em **Run**. O resultado deverá aparecer no terminal integrado:

```text
Sistema de Gestão de Biblioteca
Ambiente Java configurado com sucesso.
```

O botão **Run** não cria outro tipo de Java. Ele apenas automatiza etapas que você acabou de realizar manualmente: localizar o JDK, preparar a compilação e iniciar a JVM.

Entender o terminal antes do botão traz uma vantagem importante. Se a execução automática falhar no futuro, você terá uma ideia melhor de onde investigar.

O Java 21 também consegue executar diretamente determinados arquivos `.java` em modo de arquivo-fonte. Porém, utilizamos separadamente `javac` e `java` para deixar clara a diferença entre **compilar** e **executar**.

---

## 12. Primeira aproximação da orientação a objetos

O programa ainda é pequeno, mas já está dentro de uma classe chamada `AplicacaoBiblioteca`.

Durante a trilha, o sistema crescerá com elementos do domínio da biblioteca:

- `Livro`;
- `Usuario`;
- `Emprestimo`;
- `Biblioteca`.

Em vez de colocar todo o sistema dentro de uma única sequência enorme de instruções, distribuiremos responsabilidades entre classes e objetos.

Essa será uma das principais mudanças em relação a uma abordagem predominantemente procedural. No procedural, a pergunta central costuma ser:

> Quais passos o programa deve executar?

Na orientação a objetos, também começaremos a perguntar:

> Quais elementos existem neste domínio e qual responsabilidade pertence a cada um?

Não é necessário dominar essa mudança agora. A transição será construída aos poucos, sempre usando o mesmo aplicativo de Gestão de Biblioteca. Sua experiência em compreender processos continuará sendo útil; apenas acrescentaremos uma nova forma de modelar quem participa desses processos.

---

## 13. Problemas comuns e soluções simples

### `java` não é reconhecido

Confira se `%JAVA_HOME%\bin` foi incluído no `PATH`. Depois, feche o terminal e abra outro.

### `javac` não é reconhecido

Execute:

```bat
where java
where javac
```

Se `java` aparece, mas `javac` não, o terminal pode estar encontrando outra instalação Java. Mostre o resultado ao instrutor.

### A versão não é 21

Existe provavelmente outra versão antes do Temurin 21 no `PATH`. Não exclua nada sem verificar. Registre os resultados de `where java` e `where javac`.

### `%JAVA_HOME%` aparece na tela em vez do caminho

A variável não foi encontrada naquela janela. Confira se o nome foi escrito exatamente como `JAVA_HOME` e abra um novo terminal.

### O compilador não encontra o arquivo

Confira se o terminal está na pasta `gestao-biblioteca` e se a estrutura foi criada corretamente:

```text
src\br\com\curso\biblioteca\AplicacaoBiblioteca.java
```

### O nome da classe está errado

O arquivo e a classe precisam usar o mesmo nome:

```text
AplicacaoBiblioteca.java
```

```java
public class AplicacaoBiblioteca
```

### O programa continua exibindo a mensagem antiga

Se você alterou o `.java`, compile novamente antes de executar pelo terminal. O arquivo `.class` não é atualizado apenas porque o fonte foi salvo.

### O VS Code não reconhece o Java

Confira:

1. se `java -version` funciona;
2. se `javac -version` funciona;
3. se o Extension Pack for Java está instalado;
4. se a pasta `gestao-biblioteca` foi aberta;
5. se o JDK 21 aparece em **Java: Configure Java Runtime**;
6. se o VS Code foi reiniciado após a configuração.

Se o problema continuar, procure o instrutor levando o resultado destes comandos:

```bat
echo %JAVA_HOME%
java -version
javac -version
where java
where javac
```

---

## 14. Prática de consolidação

Faça o ciclo mais uma vez para fixar o funcionamento do ambiente:

1. execute `java -version`;
2. execute `javac -version`;
3. confira `JAVA_HOME`;
4. abra `gestao-biblioteca` no VS Code;
5. abra `AplicacaoBiblioteca.java`;
6. troque a segunda mensagem por:

```java
System.out.println("Aplicação pronta para receber novos módulos.");
```

7. compile novamente com `javac`;
8. execute novamente com `java`;
9. confirme que a nova mensagem apareceu;
10. execute também pelo botão **Run**.

Em COBOL, a alteração equivalente seria trocar o conteúdo de um `DISPLAY`, recompilar e executar novamente. Em Java, fizemos o mesmo ciclo usando `System.out.println`, `javac` e `java`.

---

## 15. Checklist do Tópico 1

- [ ] Instalei o JDK 21 no computador físico.
- [ ] Entendi que o JDK contém ferramentas para desenvolver em Java.
- [ ] Sei que `javac` compila e `java` executa.
- [ ] Configurei `JAVA_HOME` com a pasta principal do JDK.
- [ ] Adicionei `%JAVA_HOME%\bin` ao `PATH`.
- [ ] `java -version` apresenta a versão 21.
- [ ] `javac -version` apresenta a versão 21.
- [ ] `where java` aponta para a instalação esperada.
- [ ] `where javac` aponta para a instalação esperada.
- [ ] Instalei o Extension Pack for Java no VS Code.
- [ ] O VS Code reconhece o JDK 21.
- [ ] Criei a estrutura do projeto `gestao-biblioteca`.
- [ ] Criei `AplicacaoBiblioteca.java`.
- [ ] Compilei o programa e gerei um arquivo `.class`.
- [ ] Executei o programa pelo terminal.
- [ ] Executei o programa pelo VS Code.

Você não começou novamente do zero. Neste tópico, apenas conectou sua experiência de desenvolvimento a uma nova plataforma. O ambiente está pronto; a partir daqui, o aplicativo de Gestão de Biblioteca crescerá passo a passo enquanto você aprende a forma Java de organizar o código.

---

## Referências oficiais

- [Java no Visual Studio Code](https://code.visualstudio.com/docs/languages/java)
- [Primeiros passos com Java no VS Code](https://code.visualstudio.com/docs/java/java-tutorial)
- [Instaladores MSI do Eclipse Temurin para Windows](https://adoptium.net/installation/windows/)
- [Documentação do comando `java` no JDK 21](https://docs.oracle.com/en/java/javase/21/docs/specs/man/java.html)
- [Documentação do comando `javac` no JDK 21](https://docs.oracle.com/en/java/javase/21/docs/specs/man/javac.html)
