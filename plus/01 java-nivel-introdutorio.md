# Java - Nível Introdutório

# Módulo 1 — Conhecendo Java e o JDK

Este módulo é o ponto de partida de todo o livro: antes de aprender a escrever qualquer linha de código Java, é preciso entender o que é essa linguagem, como ela se relaciona com a plataforma e as ferramentas que a cercam, e como colocar um ambiente de trabalho funcionando na própria máquina. Os cinco capítulos aqui reunidos seguem uma ordem deliberada — da teoria mínima necessária até a prática de rodar um programa de verdade — porque não faz sentido instalar ferramentas sem saber o que elas fazem, nem executar um programa sem entender, ainda que superficialmente, o caminho que o código percorre até virar algo em execução.

O módulo começa apresentando a linguagem Java e o ecossistema ao seu redor (Java SE, a plataforma Java, suas características e a promessa de portabilidade), segue explicando o papel da JVM, do JDK e do bytecode nesse processo, passa pela instalação prática do Java 25 na máquina do leitor, chega ao primeiro programa efetivamente escrito, compilado e executado, e termina mostrando formas mais modernas e simplificadas de iniciar um programa Java, incluindo o JShell. Ao final deste módulo, o leitor terá o ambiente de desenvolvimento pronto e terá escrito e executado seu primeiro código, além de entender por que os comandos que digitou fazem o que fazem — uma base indispensável para tudo que vem depois no livro.

## O que é Java?

Antes de escrever qualquer código, faz sentido parar e entender o que exatamente é "Java" — porque o nome é usado, no dia a dia, para se referir a várias coisas relacionadas, mas distintas: a linguagem de programação em si, a edição padrão de tecnologias que a acompanham (Java SE) e a plataforma de execução que roda o código já compilado. Confundir esses termos é uma fonte comum de confusão para quem está começando, especialmente ao ler vagas de emprego, documentações ou tutoriais que usam essas palavras de forma intercambiável sem explicar a diferença.

Este capítulo separa esses conceitos um a um: o que caracteriza Java como linguagem, o que é o Java SE, o que é a "plataforma Java" como um todo, quais características tornaram essa linguagem tão adotada em ambientes corporativos, e por que ela é descrita como portátil — capaz de rodar, sem alterações, em sistemas operacionais diferentes. Esses conceitos não são só teoria desconectada: eles explicam decisões práticas que o leitor vai encontrar nos próximos capítulos, como por que é preciso instalar um "JDK" e não apenas um compilador, e por que um mesmo programa compilado funciona tanto em Windows quanto em Linux.

### Linguagem Java

Java é uma linguagem de programação orientada a objetos, criada pela Sun Microsystems (hoje parte da Oracle) em meados dos anos 1990 e usada até hoje para construir desde aplicativos de celular até sistemas bancários de grande porte. Quando dizemos "linguagem de programação", falamos de um conjunto de regras de sintaxe e de comportamento que permite a uma pessoa escrever instruções que o computador consegue transformar em ações concretas: somar números, gravar um arquivo, responder a um clique, consultar um banco de dados. Java se encaixa nesse universo como uma linguagem de propósito geral, ou seja, não foi desenhada para resolver um único tipo de problema (como acontece com linguagens voltadas exclusivamente para estatística ou para páginas web), mas para servir a praticamente qualquer domínio.

Antes de linguagens como Java se popularizarem, quem programava em larga escala geralmente usava C ou C++. Essas linguagens são poderosas e rápidas, mas exigem que o programador administre manualmente detalhes delicados, como a alocação e a liberação de memória. Um erro nessa administração podia travar o programa ou abrir brechas de segurança. Java nasceu, entre outras razões, para reduzir esse tipo de problema: ela introduz um modelo de gerenciamento automático de memória (o "coletor de lixo", ou garbage collector) e uma série de verificações em tempo de execução que tornam o código mais seguro e mais previsível, ainda que isso tenha um custo de desempenho comparado ao C++.

Na prática, aprender "a linguagem Java" significa aprender sua sintaxe (como declarar uma variável, como escrever uma condição, como definir uma classe) e seus conceitos centrais, como orientação a objetos, tipagem estática (o tipo de cada dado é conhecido antes mesmo do programa rodar) e um modelo de execução baseado em uma máquina virtual, que será detalhado no próximo capítulo. É importante já não confundir "linguagem Java" com "Java SE" ou "plataforma Java": a linguagem é apenas a gramática e o vocabulário; SE e plataforma dizem respeito a como esse código roda e a quais ferramentas o acompanham, temas dos dois conceitos seguintes.

Um exemplo simples do que "escrever em Java" significa, mesmo antes de entender cada detalhe:

```java
int idade = 25;
System.out.println("Idade: " + idade);
```

Essas duas linhas já mostram traços típicos da linguagem: um tipo explícito (`int`), um ponto e vírgula encerrando cada instrução, e uma chamada de método (`System.out.println`) para produzir uma saída. Dominar Java começa por entender essas convenções e vai, ao longo do livro, se expandindo para conceitos mais ricos, como classes, herança e tratamento de erros.

### Java SE

Java SE, sigla para "Java Standard Edition" (Edição Padrão do Java), é o conjunto básico e mais comum de tecnologias Java: a linguagem em si, mais uma biblioteca padrão enorme (coleções, manipulação de texto, entrada e saída de dados, threads, entre muitas outras coisas) e as ferramentas necessárias para compilar e rodar programas. É o "Java do dia a dia", usado para aprender a linguagem, construir aplicações de desktop, utilitários de linha de comando e a base sobre a qual outras edições são construídas.

Sem essa distinção, um iniciante pode se confundir ao ler sobre "Java Enterprise Edition" (Jakarta EE, focada em aplicações corporativas e servidores web) ou "Java Micro Edition" (voltada a dispositivos com poucos recursos, hoje bem menos usada). Essas outras edições não substituem o Java SE — elas se apoiam nele e adicionam bibliotecas e especificações extras para contextos específicos, como um servidor de aplicações corporativo. Sem entender que o SE é a base, seria fácil achar que Java é uma coisa só e se perder ao encontrar siglas diferentes em vagas de emprego, documentações ou tutoriais.

O Java SE resolve isso concentrando, num único conjunto coerente, tudo que é necessário para programar em Java de forma independente: a especificação da linguagem, a biblioteca padrão (chamada de API Java SE) e a implementação de referência, hoje mantida pela Oracle através do OpenJDK. Quando você instala "o JDK" (assunto do próximo capítulo), está instalando justamente uma implementação do Java SE.

Um exemplo prático: ao escrever `import java.util.List;` num programa, você está usando uma classe que faz parte da biblioteca padrão do Java SE. Não é preciso baixar nada além do próprio Java SE para isso — diferente de bibliotecas de terceiros, que exigem downloads e configurações adicionais.

Este livro se concentra inteiramente no Java SE. Módulos mais avançados de Java corporativo (Jakarta EE, frameworks como Spring) ficam fora do escopo introdutório e são construídos justamente sobre os fundamentos ensinados aqui.

### Plataforma Java

"Plataforma Java" é um termo mais amplo do que "linguagem Java": ele engloba tanto a linguagem quanto o ambiente de execução que a sustenta — a Java Virtual Machine (JVM) e o conjunto de bibliotecas que dão suporte ao código em tempo de execução. Enquanto a linguagem define como o código é escrito, a plataforma define como esse código, depois de compilado, é efetivamente executado em um computador.

Isso é uma diferença importante porque, em muitas linguagens tradicionais (como C), o código-fonte é traduzido diretamente para instruções do processador específico da máquina onde ele vai rodar. Um programa compilado para Windows em um processador Intel não roda, sem recompilação, em um Mac com processador ARM. A plataforma Java resolve esse problema inserindo uma camada intermediária: o código Java não é compilado diretamente para instruções de uma máquina física, mas para um formato intermediário (bytecode) que roda sobre a JVM, um "computador virtual" que existe em versões específicas para cada sistema operacional e arquitetura.

Dessa forma, a plataforma Java entrega duas coisas ao mesmo tempo: um ambiente de execução (JVM) que abstrai as diferenças entre sistemas operacionais, e uma base de bibliotecas essenciais (como manipulação de arquivos, redes e coleções de dados) disponível de forma consistente em qualquer lugar onde a plataforma exista. É essa combinação — máquina virtual mais bibliotecas — que permite a portabilidade discutida no próximo conceito.

Uma boa analogia é pensar na plataforma Java como um teatro com um palco padronizado, presente em várias cidades do mundo, todos seguindo exatamente as mesmas medidas e o mesmo equipamento de som e luz. Uma peça (o programa) pode ser ensaiada uma única vez e apresentada em qualquer um desses palcos sem alterações, porque o "palco" (a plataforma) é sempre o mesmo, mesmo que o prédio ao redor (o sistema operacional, o hardware) mude de cidade para cidade.

Na prática, quando alguém diz "isso roda na plataforma Java", está afirmando que o programa foi compilado para bytecode e pode ser executado por qualquer JVM compatível, dando a ele grande alcance sem exigir versões separadas de código para cada sistema operacional.

### Características da linguagem

Java reúne um conjunto de características que, juntas, explicam por que ela se tornou tão usada em ambientes corporativos e educacionais. A primeira é ser orientada a objetos: praticamente todo código Java é organizado em classes, que agrupam dados e comportamentos relacionados. Isso favorece a organização de sistemas grandes, já que cada parte do problema pode ser modelada como um objeto com responsabilidades claras.

A segunda característica é a tipagem estática e forte: toda variável tem um tipo definido no momento em que é declarada (um `int`, uma `String`, etc.), e esse tipo é checado pelo compilador antes mesmo do programa rodar. Isso evita uma classe inteira de erros — como tentar somar um texto com um número sem conversão — que só apareceriam em tempo de execução em linguagens de tipagem dinâmica, como Python ou JavaScript.

A terceira é a gestão automática de memória: como já vimos ao apresentar a linguagem, o programador não precisa liberar manualmente a memória de um objeto que não é mais necessário — o coletor de lixo da JVM cuida disso, reduzindo erros comuns em linguagens como C.

A quarta é a robustez em relação a erros: Java obriga o tratamento (ou pelo menos a sinalização) de determinados tipos de falha através do mecanismo de exceções, que será estudado em profundidade mais adiante no livro. Isso força o programador a pensar, desde cedo, em cenários de erro.

Por fim, há a portabilidade, tema do próximo conceito, e uma sintaxe relativamente verbosa se comparada a linguagens mais modernas, mas que prioriza clareza: um trecho de código Java tende a deixar explícito o tipo de cada dado e a estrutura de cada bloco, o que ajuda times grandes a lerem e manterem código escrito por outras pessoas.

Um exemplo que ilustra a tipagem forte:

```java
int quantidade = 10;
// quantidade = "dez"; // erro de compilação: tipos incompatíveis
```

Essa linha comentada não compilaria, porque o compilador Java rejeita atribuir um texto a uma variável declarada como `int`. Esse tipo de verificação antecipada é uma das características mais marcantes da linguagem e um dos motivos pelos quais ela é vista como confiável para sistemas críticos, como aplicações bancárias e de grande escala.

### Portabilidade

Portabilidade é a capacidade de um programa rodar em diferentes sistemas operacionais e arquiteturas de hardware sem precisar ser reescrito ou recompilado especificamente para cada um deles. Esse é, historicamente, um dos maiores diferenciais do Java em relação a linguagens como C ou C++, cujos programas compilados são específicos para o sistema e o processador em que foram gerados.

Como vimos ao falar da plataforma Java, foi justamente para evitar esse retrabalho que ela introduziu a camada intermediária de bytecode e JVM: sem isso, uma empresa que precisasse rodar o mesmo software em um servidor Linux, em uma estação Windows e em um Mac teria que manter versões separadas do código ou recompilar e testar em cada ambiente-alvo, com risco de comportamentos divergentes entre eles. É essa mesma combinação — bytecode compilado uma única vez, executado por qualquer JVM compatível — que garante a portabilidade na prática: o mesmo arquivo compilado (um `.class`, ou um `.jar` contendo vários deles) pode ser levado, sem alteração, de um Windows para um Linux ou para um Mac, desde que exista uma JVM compatível instalada em cada um. Os detalhes de como a JVM interpreta esse bytecode ficam para o capítulo seguinte.

Um exemplo real dessa portabilidade: uma equipe de desenvolvimento pode programar e testar em notebooks com Windows ou macOS, e implantar a mesma aplicação, sem recompilar, em servidores Linux em produção — cenário extremamente comum no mercado. Isso reduz custos de manutenção e amplia o público-alvo de um software, já que não é preciso escolher entre suportar Windows ou Linux: basta que exista uma JVM para o sistema de destino, o que hoje é praticamente universal.

Vale notar que portabilidade não é sinônimo de desempenho idêntico em todo lugar: diferenças de hardware, de sistema operacional e de configuração da própria JVM ainda podem afetar a velocidade de execução. O que a portabilidade garante é a possibilidade de execução correta do mesmo código compilado, não que o desempenho seja idêntico em todas as máquinas. Ainda assim, essa garantia de "não precisar reescrever o programa para cada plataforma" foi um fator decisivo para a adoção de Java em ambientes corporativos com infraestrutura heterogênea.

### "Write once, run anywhere"

"Write once, run anywhere" (escreva uma vez, rode em qualquer lugar), frequentemente abreviado como WORA, é o lema que a Sun Microsystems usou para resumir a proposta de portabilidade do Java desde o seu lançamento. É mais um slogan de marketing do que um termo técnico, mas carrega uma promessa concreta: o mesmo código-fonte, compilado uma única vez, deve poder ser executado em qualquer sistema que tenha uma JVM compatível, sem a necessidade de recompilação para cada plataforma-alvo.

Antes desse lema fazer sentido como promessa realista, a experiência comum na programação era resumida na brincadeira "escreva uma vez, compile para cada lugar" (ou pior, "escreva de novo para cada lugar"): um desenvolvedor de C que quisesse distribuir um programa para Windows, Linux e Mac normalmente precisava manter builds (versões compiladas) separadas, cada uma gerada a partir de uma compilação específica para aquele ambiente. O WORA é justamente o nome popular para a solução que já vimos ao falar de portabilidade e da plataforma Java — bytecode compilado uma única vez e executado por qualquer JVM instalada no destino —, só que sob a ótica do slogan que a Sun usou para vender essa ideia ao mercado.

Um exemplo prático: uma empresa desenvolve um sistema de gestão em Java, compila-o em um único arquivo `.jar`, e distribui esse mesmo arquivo para clientes que usam Windows, Linux ou macOS. Nenhum desses clientes precisa de uma versão diferente do arquivo — cada um só precisa ter o Java instalado (ou seja, uma JVM compatível) em sua própria máquina.

É importante ter uma expectativa realista sobre esse lema: ele descreve bem a portabilidade do bytecode entre sistemas operacionais e arquiteturas de hardware, mas não elimina completamente diferenças de comportamento — por exemplo, um programa que manipula caminhos de arquivo de forma específica do Windows (usando barra invertida) pode se comportar de modo inesperado em Linux, que usa barra normal, a menos que o código utilize as abstrações apropriadas da própria biblioteca padrão para lidar com esse tipo de diferença. Ainda assim, para a vasta maioria dos programas Java, "write once, run anywhere" continua sendo uma descrição precisa da experiência de portabilidade oferecida pela plataforma.

## JDK, JVM e bytecode

O capítulo anterior explicou que Java é portátil e roda sobre uma plataforma, mas deixou de propósito para este capítulo o "como": qual peça de software é responsável por executar o código, o que essa peça recebe como entrada, e o que é preciso instalar na máquina para poder programar e rodar aplicações Java. Este capítulo apresenta as três peças que respondem a essas perguntas — a JVM, o JDK e o bytecode — e como elas se encaixam entre si.

Esses três termos aparecem juntos com tanta frequência que é fácil tratá-los como sinônimos, mas cada um tem um papel distinto: a JVM executa, o JDK contém as ferramentas para desenvolver (incluindo o compilador e uma JVM embutida), e o bytecode é o formato intermediário que sai da compilação e entra na execução. Entender essa cadeia — código-fonte, compilador, bytecode, JVM — é o que torna compreensível, e não mágico, o processo de "escrever, compilar e rodar" que será praticado logo em seguida, no capítulo sobre o primeiro programa.

### JVM

JVM é a sigla para Java Virtual Machine (Máquina Virtual Java), o componente responsável por executar programas Java. Ela não é uma máquina física, mas um programa que simula um processador: recebe instruções em um formato específico (o bytecode, tema de um conceito adiante) e as executa, traduzindo-as, internamente, para as instruções reais do processador e do sistema operacional onde está instalada. É a JVM que torna possível a portabilidade discutida no capítulo anterior — ela é a peça que existe "em cada palco" para que a mesma "peça de teatro" (o programa compilado) possa ser apresentada em qualquer lugar.

Sem uma máquina virtual como a JVM, cada programa Java precisaria ser compilado diretamente para as instruções do processador específico de cada máquina, exatamente como acontece em C ou C++. Isso quebraria a promessa central da plataforma: a mesma aplicação teria que existir em várias versões compiladas, uma para cada combinação de sistema operacional e arquitetura de hardware.

A JVM resolve isso funcionando como uma camada intermediária. Quando você executa um programa Java com o comando `java`, é a JVM que é efetivamente iniciada; ela carrega o bytecode do programa, verifica sua integridade, aloca memória para os objetos que serão criados, gerencia essa memória automaticamente através do coletor de lixo, e vai interpretando e otimizando a execução das instruções conforme o programa roda. JVMs modernas usam uma técnica chamada compilação just-in-time (JIT), que identifica trechos de código executados com frequência e os traduz diretamente para instruções nativas do processador, ganhando desempenho ao longo da execução — diferente de uma interpretação pura, instrução por instrução, que seria mais lenta.

Um exemplo concreto: ao rodar `java MinhaClasse` no terminal, o que acontece por trás dos panos é a JVM sendo iniciada, localizando o arquivo `MinhaClasse.class` (o bytecode compilado), carregando-o na memória e começando a executar o método `main` presente nele. Se você tivesse apenas o bytecode, mas nenhuma JVM instalada, o arquivo `.class` seria inútil — não haveria nada capaz de interpretá-lo.

Vale destacar que existem diferentes implementações de JVM (a da Oracle, a OpenJ9 da IBM, entre outras), todas seguindo a mesma especificação técnica, o que garante que bytecode gerado por um compilador Java padrão funcione em qualquer uma delas. Essa separação entre "especificação" (o que a JVM deve fazer) e "implementação" (como um fabricante específico faz isso) é parte do que sustenta o ecossistema Java, permitindo inclusive otimizações e melhorias de desempenho diferentes entre fornecedores, sem quebrar a compatibilidade do código.

### JDK

JDK é a sigla para Java Development Kit (Kit de Desenvolvimento Java), o pacote de ferramentas que um programador instala em sua máquina para escrever, compilar, testar e executar programas Java. Ele inclui, entre outras coisas, o compilador (`javac`), a própria JVM (necessária para rodar o que foi compilado), ferramentas de depuração, geração de documentação e a biblioteca padrão do Java SE.

É comum, principalmente para quem está começando, confundir JDK com JVM ou pensar que baixar "o Java" é uma coisa só e sem nuances. Sem entender a diferença, um iniciante pode instalar apenas um ambiente de execução (JRE, um pacote mais restrito, hoje pouco distribuído separadamente) e descobrir, na hora de compilar seu primeiro programa, que o comando `javac` simplesmente não existe em sua máquina — porque o compilador é parte do JDK, não de um ambiente de execução isolado.

O JDK resolve o problema de reunir, num único instalador, tudo que é necessário para o ciclo completo de desenvolvimento: você escreve o código-fonte em um editor qualquer, usa o `javac` (que vem no JDK) para compilá-lo em bytecode, e usa o `java` (também no JDK, que internamente aciona a JVM) para executá-lo. Sem o JDK, seria preciso reunir manualmente ferramentas separadas — algo que, na prática, nenhuma linguagem madura obriga o desenvolvedor a fazer.

Um exemplo prático do dia a dia: ao seguir o capítulo de instalação mais adiante neste módulo, você vai baixar e instalar o JDK (na versão 25, a mais recente quando este livro foi escrito), e verificar sua instalação rodando `java --version` e `javac --version` no terminal. Se apenas o primeiro comando funcionar, é sinal de que você tem um ambiente de execução, mas não o kit completo de desenvolvimento — um problema comum que a instalação correta do JDK evita.

É importante frisar que, embora o JDK inclua uma JVM internamente (para rodar programas durante o desenvolvimento), a JVM em si é um conceito mais amplo: existem distribuições dela usadas apenas para executar aplicações já prontas, sem as ferramentas de desenvolvimento do JDK. Para quem programa, no entanto, instalar o JDK é sempre o caminho certo, já que ele contém tudo que é necessário, inclusive a capacidade de executar o que foi compilado.

### Compilador

O compilador é o programa responsável por traduzir código-fonte, escrito por uma pessoa em uma linguagem legível como Java, para um formato que a máquina (ou, no caso do Java, a JVM) consiga executar. No ecossistema Java, essa ferramenta é o `javac` (Java Compiler), que faz parte do JDK e transforma arquivos `.java` em arquivos `.class` contendo bytecode.

Sem um compilador, o código-fonte escrito por um desenvolvedor seria apenas texto: um processador não sabe interpretar diretamente `int idade = 25;`. É preciso um programa que leia essas instruções, verifique se elas seguem as regras gramaticais da linguagem (a chamada análise sintática), confira se os tipos são compatíveis entre si (a checagem de tipos, uma das etapas mais importantes em Java, dada sua tipagem forte) e, só então, gere uma representação que possa ser executada.

O `javac` resolve esse processo em uma única etapa do ponto de vista do usuário: ao rodar `javac MinhaClasse.java`, o compilador lê o arquivo, verifica erros de sintaxe e de tipo, e, se tudo estiver correto, gera um arquivo `MinhaClasse.class` com o bytecode correspondente. Se houver qualquer erro — uma variável usada sem ser declarada, uma chave sem fechamento, uma atribuição de tipos incompatíveis — o compilador interrompe o processo e relata o problema, sem gerar o `.class`. Essa etapa de checagem antecipada é uma das razões pelas quais erros básicos em Java costumam ser pegos antes mesmo do programa rodar, diferente de linguagens interpretadas, onde certos erros só aparecem durante a execução.

Um exemplo direto:

```
javac Ola.java
```

Esse comando, executado no terminal dentro da pasta onde está o arquivo `Ola.java`, gera um arquivo `Ola.class` na mesma pasta, caso o código esteja correto. Esse `.class` é o bytecode que a JVM, posteriormente, vai executar através do comando `java Ola`.

Vale destacar que "compilador", em Java, tem um papel um pouco diferente do que em C ou C++: nessas linguagens, o compilador gera diretamente código de máquina específico para um processador; em Java, o `javac` gera bytecode, um formato intermediário e independente de plataforma, que só se torna instruções de máquina de fato quando a JVM o executa — muitas vezes através da compilação just-in-time mencionada no conceito sobre a JVM.

### Bytecode

Bytecode é o formato intermediário para o qual o compilador Java traduz o código-fonte. Ele não é o texto legível que o programador escreveu (como `int idade = 25;`), nem é diretamente as instruções específicas de um processador Intel ou ARM: é uma representação intermediária, organizada em instruções compactas, pensada para ser interpretada (ou compilada, via JIT) eficientemente por qualquer JVM, independentemente do sistema operacional ou hardware.

Sem esse formato intermediário, o Java teria que escolher entre duas alternativas menos vantajosas: compilar diretamente para código de máquina específico de cada plataforma (perdendo a portabilidade que discutimos anteriormente), ou interpretar o código-fonte texto diretamente a cada execução (o que seria muito mais lento, já que interpretar texto bruto exige muito mais processamento do que interpretar instruções já compactadas e estruturadas).

O bytecode resolve esse impasse funcionando como um meio-termo: ele já está numa forma compacta, estruturada e "pré-digerida" pelo compilador — toda a análise de sintaxe e checagem de tipos já foi feita durante a compilação —, mas ainda não é específico de nenhuma plataforma de hardware. Cada instrução de bytecode corresponde a uma operação simples (carregar um valor, somar dois números, chamar um método), e é a JVM, no momento da execução, que traduz essas instruções para o que o processador local realmente entende, seja via interpretação direta, seja via compilação just-in-time.

Retomando o exemplo do conceito anterior: o `Ola.class` gerado por `javac Ola.java` é justamente esse bytecode. Se você tentar abrir esse arquivo em um editor de texto comum, verá um conteúdo binário, ilegível para humanos. Ferramentas específicas, como o `javap` (desmontador de bytecode, incluído no JDK), conseguem exibir esse conteúdo de forma legível, mostrando instruções como `iconst_5` (carregar a constante inteira 5) ou `invokevirtual` (chamar um método de instância) — um nível de detalhe que o programador raramente precisa examinar diretamente, mas que existe por trás de cada `.class` gerado.

Uma boa analogia: pense no bytecode como uma partitura musical. Um compositor escreve a partitura (o código-fonte) seguindo regras de notação musical; a partitura em si não faz som algum, mas é um formato universal que qualquer músico (a JVM), treinado para ler aquela notação, consegue interpretar e transformar em som real, independentemente do instrumento específico que ele tocar. A partitura não muda de país para país; o que muda é apenas quem a interpreta.

Esse formato intermediário é a peça central que torna possível tanto a portabilidade quanto a segurança de Java: antes de executar qualquer bytecode, a JVM realiza uma etapa de verificação, checando se aquele bytecode obedece às regras da máquina virtual, o que ajuda a prevenir a execução de código malformado ou malicioso.

### Código-fonte × código compilado

Código-fonte é o texto que o programador efetivamente escreve, em um arquivo com extensão `.java`, seguindo a sintaxe da linguagem Java — algo como `System.out.println("Olá, mundo!");`. Código compilado, no contexto de Java, é o resultado do processo de compilação: o bytecode contido em um arquivo `.class`, que não é mais texto legível por humanos, mas sim instruções que a JVM sabe interpretar e executar.

A distinção entre essas duas formas do "mesmo programa" costuma confundir quem está começando, principalmente porque, em algumas linguagens (como Python, em seu uso mais comum), o mesmo arquivo que o programador escreve é, na prática, o que é executado diretamente por um interpretador, sem uma etapa de compilação explícita e visível para o usuário. Em Java, essas duas formas são fisicamente arquivos diferentes: um `.java` e um `.class`, e entender que ambos coexistem, com papéis distintos, é essencial para não se perder ao ver mensagens de erro ou ao organizar um projeto.

Como já vimos nos conceitos sobre o compilador e o bytecode, Java separa essas duas etapas exatamente para conseguir checagem antecipada de erros e, ao mesmo tempo, um formato de execução compacto e portátil. O que vale destacar aqui é a consequência prática dessa separação: cada `.java` compilado gera, em geral, um ou mais arquivos `.class` — uma classe pública principal, mais eventuais classes auxiliares definidas no mesmo arquivo.

Um exemplo direto do fluxo completo:

```
// Arquivo: Ola.java (código-fonte)
public class Ola {
    public static void main(String[] args) {
        System.out.println("Olá, mundo!");
    }
}
```

Esse é o código-fonte que, como vimos, o `javac` transforma em `Ola.class`. O arquivo `.java` continua existindo, inalterado, na mesma pasta; o `.class` é um arquivo novo, gerado a partir dele — e é esse `.class`, não o `.java`, que o comando `java Ola` efetivamente executa. É por isso que, se você editar o `.java` mas esquecer de recompilar, a execução continuará usando a versão antiga do `.class`, um erro comum entre iniciantes.

Essa separação também explica por que é possível distribuir um programa Java sem compartilhar seu código-fonte: basta entregar os arquivos `.class` (ou um `.jar`, que os agrupa), preservando, até certo ponto, a confidencialidade da implementação original, já que o bytecode é bem mais difícil de ler do que o código-fonte, embora não seja impossível de ser decompilado.

### Execução multiplataforma

Execução multiplataforma é a capacidade prática de pegar um mesmo arquivo compilado (bytecode) e rodá-lo, sem qualquer alteração, em sistemas operacionais e arquiteturas de hardware diferentes — é o resultado concreto de tudo que foi discutido neste capítulo: JVM, JDK, compilador e bytecode trabalhando juntos.

Sem essa capacidade, distribuir software seria significativamente mais trabalhoso. Um programa compilado para Windows em arquitetura x86, por exemplo, simplesmente não roda em um Linux com processador ARM, a menos que seja recompilado especificamente para esse alvo — e recompilar exige, no mínimo, ter acesso ao código-fonte, às ferramentas de compilação corretas para aquele destino, e testar novamente o resultado. Empresas que precisam suportar vários sistemas operacionais historicamente lidam com esse fardo mantendo múltiplos pipelines de build, um para cada combinação de sistema e arquitetura.

A execução multiplataforma em Java resolve isso concentrando toda a variação específica de plataforma dentro da própria JVM, e não no bytecode. O bytecode gerado pelo `javac` é sempre o mesmo, independentemente de onde a compilação aconteceu; o que muda, de máquina para máquina, é apenas qual JVM está instalada e como ela traduz aquele bytecode para instruções nativas daquele sistema específico. Do ponto de vista de quem distribui o software, o trabalho de "adaptar para cada plataforma" já foi feito, de uma vez por todas, pelos mantenedores da própria JVM — não é uma responsabilidade que recai sobre cada equipe de desenvolvimento.

Um exemplo real e comum no mercado: uma equipe desenvolve uma aplicação Java em notebooks com Windows, versiona o código em um repositório compartilhado, e um pipeline de integração contínua compila e testa essa aplicação automaticamente em um servidor Linux, gerando um `.jar` que, mais tarde, é implantado em produção — possivelmente em um terceiro ambiente, como um contêiner baseado em uma distribuição específica de Linux. Em nenhum momento desse processo é necessário reescrever ou recompilar manualmente o código para cada sistema; o mesmo `.jar` funciona em todos, desde que cada ambiente tenha sua própria JVM compatível instalada.

É importante notar que essa multiplataforma se refere à execução do bytecode, não necessariamente a todo comportamento do programa: código que depende de recursos específicos de um sistema operacional (por exemplo, caminhos de arquivo formatados de um jeito particular, ou bibliotecas nativas externas ao Java) pode exigir cuidados extras para funcionar igualmente bem em todos os ambientes. Ainda assim, para a grande maioria das aplicações Java, que usam apenas a linguagem e a biblioteca padrão, a execução multiplataforma é uma garantia sólida e um dos motivos centrais da adoção da tecnologia em ambientes corporativos heterogêneos.

## Instalando o Java 25

Com a teoria de linguagem, plataforma, JVM, JDK e bytecode já apresentada, este capítulo sai do campo conceitual e vai para a prática: colocar o Java 25 funcionando na máquina do leitor. Sem essa etapa, nenhum dos comandos e programas discutidos no restante do livro pode ser efetivamente testado — é o requisito prático para tudo que vem depois.

O capítulo cobre a instalação do JDK propriamente dita, a configuração de duas variáveis de ambiente que costumam confundir iniciantes (`JAVA_HOME` e `PATH`), e como verificar se tudo funcionou usando os comandos `java --version` e `javac --version`. Também aborda, de forma breve, a escolha de uma IDE (ambiente de desenvolvimento integrado) para escrever código com mais conforto do que um editor de texto simples. Ao final deste capítulo, o leitor terá um ambiente pronto para compilar e executar os primeiros programas Java, tema do capítulo seguinte.

### Instalação do JDK

Instalar o JDK é o primeiro passo prático para programar em Java: é o processo de baixar e configurar, na própria máquina, o conjunto de ferramentas descrito no capítulo anterior — compilador, JVM e biblioteca padrão — de modo que os comandos `javac` e `java` fiquem disponíveis no terminal. Neste livro, a versão usada é o JDK 25, a versão mais recente e com suporte ativo no momento em que este material foi escrito.

Sem essa instalação, nenhum dos comandos discutidos até aqui funciona: tentar rodar `javac` ou `java` num terminal sem o JDK instalado resulta em uma mensagem de erro do próprio sistema operacional, informando que o comando não foi encontrado. É um passo que parece trivial, mas costuma ser onde iniciantes mais travam, geralmente por baixar a versão errada (por exemplo, apenas um JRE, sem o compilador) ou por não configurar corretamente as variáveis de ambiente, assunto dos próximos dois conceitos.

A forma recomendada de instalar o JDK 25 é baixar um instalador de uma distribuição confiável — a própria Oracle oferece o seu, e existem distribuições OpenJDK igualmente compatíveis e gratuitas, como as mantidas por Eclipse Adoptium, Amazon (Corretto) ou Azul (Zulu). Todas seguem a mesma especificação da linguagem e da JVM, de modo que o código compilado se comporta da mesma forma independentemente de qual distribuição foi escolhida; a diferença está em detalhes de suporte, licenciamento e frequência de atualizações.

Em sistemas como Windows e macOS, o instalador normalmente já cuida de colocar os executáveis em um local padrão e, em muitos casos, já ajusta o PATH automaticamente. Em distribuições Linux, é comum instalar o JDK via gerenciador de pacotes (como `apt` em sistemas baseados em Debian/Ubuntu) ou extraindo manualmente um arquivo compactado para uma pasta específica, caso em que a configuração de `JAVA_HOME` e PATH precisa ser feita manualmente — passos detalhados nos dois próximos conceitos.

Um exemplo do processo em um sistema Linux baseado em Debian:

```
sudo apt update
sudo apt install openjdk-25-jdk
```

Após a instalação, seja qual for o sistema operacional, o passo seguinte é sempre o mesmo: verificar, no terminal, se os comandos `java --version` e `javac --version` respondem corretamente, confirmando que a instalação foi bem-sucedida e que a versão instalada é de fato a 25 — verificação detalhada em conceito próprio mais adiante neste capítulo.

Vale mencionar que é possível ter mais de uma versão do JDK instalada na mesma máquina simultaneamente (por exemplo, um JDK 17 para manter um projeto legado funcionando e um JDK 25 para desenvolvimento novo). Nesse cenário, ferramentas de gerenciamento de versões (como o SDKMAN em Linux/macOS) facilitam alternar entre elas sem reconfigurar manualmente as variáveis de ambiente a cada troca.

### `JAVA_HOME`

`JAVA_HOME` é uma variável de ambiente que aponta para a pasta onde o JDK está instalado no sistema. Variáveis de ambiente, de forma geral, são valores configurados no sistema operacional que ficam disponíveis para qualquer programa que rode naquela máquina; `JAVA_HOME`, especificamente, é uma convenção amplamente adotada no ecossistema Java (e em várias outras ferramentas que dependem dele, como Maven, Gradle e diversas IDEs) para localizar de forma confiável onde está a instalação do JDK.

Sem essa variável configurada corretamente, ferramentas que dependem de Java para funcionar — servidores de aplicação, ferramentas de build, plugins de IDE — podem falhar ao tentar localizar automaticamente o JDK, mesmo que os comandos `java` e `javac` já funcionem manualmente no terminal graças ao PATH (conceito seguinte). Isso acontece porque `JAVA_HOME` e PATH resolvem problemas relacionados, mas distintos: PATH diz ao sistema onde encontrar executáveis para rodar por nome direto no terminal; `JAVA_HOME` diz a outros programas onde está a raiz completa da instalação do JDK, incluindo suas bibliotecas internas, não apenas os executáveis.

Configurar `JAVA_HOME` resolve esse problema apontando explicitamente, de uma vez, para a pasta raiz de instalação — por exemplo, `/usr/lib/jvm/jdk-25` em um Linux típico, ou algo como `C:\Program Files\Java\jdk-25` no Windows. A partir daí, qualquer ferramenta que siga a convenção sabe procurar, dentro daquela pasta, os subdiretórios `bin` (executáveis), `lib` (bibliotecas) e demais recursos do JDK.

Um exemplo de configuração em um sistema Linux/macOS, adicionando a linha ao arquivo de configuração do shell (como `.bashrc` ou `.zshrc`):

```
export JAVA_HOME=/usr/lib/jvm/jdk-25
export PATH=$JAVA_HOME/bin:$PATH
```

Note que a segunda linha já usa `JAVA_HOME` para compor o PATH, evitando repetir o caminho completo duas vezes — uma prática comum e recomendada, já que garante que ambas as variáveis fiquem sempre consistentes entre si, apontando para a mesma instalação.

No Windows, a configuração é feita através do painel de variáveis de ambiente do sistema, criando uma variável chamada `JAVA_HOME` com o caminho da instalação, e depois editando a variável `Path` para incluir `%JAVA_HOME%\bin`.

Um erro comum é instalar uma nova versão do JDK e esquecer de atualizar `JAVA_HOME` para apontar para a pasta nova, fazendo com que ferramentas continuem enxergando a versão antiga mesmo depois da atualização — por isso, sempre que uma nova versão é instalada, vale conferir se essa variável foi atualizada de acordo.

### PATH

Imagine que o JDK foi instalado corretamente e `JAVA_HOME` está configurado, mas mesmo assim, digitar `java --version` no terminal devolve uma mensagem dizendo que o comando não foi reconhecido. Na maioria das vezes, a causa é a pasta `bin` do JDK — onde ficam os executáveis `java`, `javac` e outros — não estar listada no PATH, a variável de ambiente do sistema operacional responsável por dizer ao terminal onde procurar quando um comando é digitado apenas pelo nome, sem o caminho completo.

PATH lista, em ordem, as pastas em que o terminal (ou qualquer programa que precise localizar executáveis) deve procurar um comando digitado dessa forma. Não é uma variável exclusiva do Java — é um mecanismo geral do sistema operacional, usado por praticamente qualquer ferramenta de linha de comando instalada em uma máquina. Sem a pasta correta listada, seria necessário digitar o caminho completo até o executável toda vez, algo como `/usr/lib/jvm/jdk-25/bin/java --version`, o que é claramente inviável no dia a dia.

Adicionar a pasta `bin` do JDK ao PATH resolve exatamente isso: a partir dessa configuração, o sistema passa a reconhecer `java` e `javac` como comandos válidos em qualquer lugar do terminal, sem precisar informar o caminho completo. Como mostrado no conceito anterior, a prática recomendada é compor essa entrada a partir de `JAVA_HOME`, garantindo que as duas variáveis fiquem sempre alinhadas.

Um exemplo do efeito prático: depois de configurar corretamente o PATH, abrir um terminal em qualquer pasta do sistema e digitar `javac MinhaClasse.java` funciona normalmente, porque o sistema, ao procurar por `javac`, percorre cada pasta listada no PATH até encontrar o executável correspondente — que estará na pasta `bin` do JDK, desde que ela tenha sido corretamente adicionada.

Um problema comum entre iniciantes é ter mais de uma versão de Java instalada, com pastas diferentes de cada uma listadas no PATH, mas em uma ordem que faz o sistema encontrar primeiro uma versão antiga ao invés da recém-instalada. Como o PATH é percorrido na ordem em que as pastas aparecem, a primeira ocorrência de um executável `java` encontrada é a que efetivamente será usada, mesmo que uma versão mais nova esteja instalada em outra pasta também listada, porém mais abaixo na lista. Por isso, ao instalar uma nova versão do JDK, é importante verificar a ordem das entradas no PATH, e não apenas se elas existem.

### `java --version`

`java --version` é o comando usado para verificar, diretamente no terminal, se o comando `java` está disponível e qual versão da JVM ele está executando. É, na prática, o teste mais simples e direto para confirmar que a instalação do JDK e a configuração do PATH funcionaram corretamente.

Sem esse tipo de verificação, um problema de instalação ou de configuração de variáveis de ambiente só apareceria mais tarde, no momento de tentar rodar um programa Java de verdade — um cenário em que seria mais difícil identificar se o erro está na instalação, na configuração do sistema, ou no próprio código escrito. Rodar `java --version` logo após a instalação isola essa verificação, permitindo confirmar (ou descartar) problemas de ambiente antes de escrever qualquer linha de código.

O comando resolve essa necessidade de forma direta: ao ser executado, ele imprime no terminal informações como o número da versão instalada, a distribuição usada (por exemplo, OpenJDK ou a versão da Oracle) e, em algumas distribuições, detalhes adicionais como a data de build. Se o comando não for reconhecido pelo sistema, o problema está no PATH (o executável `java` não foi encontrado); se for reconhecido, mas mostrar uma versão diferente da esperada, o problema costuma estar na ordem das entradas do PATH, como discutido no conceito anterior, ou em uma instalação anterior não removida.

Um exemplo de saída esperada, ao instalar corretamente o JDK 25:

```
$ java --version
openjdk 25 2025-09-16
OpenJDK Runtime Environment (build 25+36)
OpenJDK 64-Bit Server VM (build 25+36, mixed mode, sharing)
```

O número exato de build pode variar conforme a distribuição e a data específica de lançamento, mas a primeira linha deve sempre indicar a versão principal — neste livro, `25` — confirmando que a instalação está correta e alinhada com o que será usado ao longo do material.

Vale notar que `java --version` verifica o ambiente de execução, ou seja, a JVM que o comando `java` aciona — não confirma, por si só, que o compilador (`javac`) também está corretamente instalado e acessível. Essa segunda verificação é o papel do próximo conceito.

### `javac --version`

`javac --version` é o comando análogo ao anterior, mas voltado especificamente para o compilador: ele verifica se o `javac` está disponível no terminal e informa a versão da linguagem Java que aquele compilador suporta. É uma verificação complementar e igualmente necessária, já que ter a JVM funcionando (confirmada por `java --version`) não garante, por si só, que o compilador também esteja instalado e acessível.

Essa distinção existe porque é tecnicamente possível ter na máquina apenas um ambiente de execução Java (um JRE, historicamente distribuído separadamente do JDK, embora hoje seja incomum encontrá-lo isolado) sem o kit de desenvolvimento completo. Nesse cenário, `java --version` funcionaria normalmente — afinal, a JVM está presente —, mas `javac --version` falharia, revelando que, apesar de ser possível rodar programas Java já compilados, não é possível compilar código-fonte novo naquela máquina. Sem essa segunda checagem, alguém poderia concluir erroneamente que "o Java está instalado e funcionando", só para descobrir a falta do compilador exatamente no momento de tentar compilar o primeiro programa.

O comando resolve isso da mesma forma direta que `java --version`: executado no terminal, ele imprime a versão do compilador instalado, permitindo confirmar rapidamente que o JDK completo (não apenas um ambiente de execução) está corretamente configurado.

Um exemplo de saída esperada, após uma instalação correta do JDK 25:

```
$ javac --version
javac 25
```

Diferente da saída de `java --version`, a de `javac --version` costuma ser mais enxuta, mostrando apenas o número da versão do compilador — que deve corresponder à mesma versão principal reportada por `java --version`, já que ambos os executáveis fazem parte da mesma instalação do JDK.

Se `javac --version` reportar uma versão diferente da reportada por `java --version`, isso é um forte indício de que existem múltiplas instalações de Java na máquina, com o PATH apontando para pastas `bin` de instalações diferentes para cada um dos dois comandos — um problema de configuração que vale a pena resolver antes de avançar na leitura, revisando a ordem das entradas do PATH e a configuração de `JAVA_HOME`, para garantir que ambos os comandos apontem consistentemente para a mesma versão 25 do JDK usada neste material.

### IDE

Escrever Java usando apenas um editor de texto simples e o terminal para rodar `javac` e `java` manualmente, como mostrado nos conceitos anteriores, é perfeitamente possível. Esse caminho, no entanto, exige mais trabalho manual: encontrar erros de sintaxe só depois de compilar, não ter sugestões automáticas de métodos disponíveis em uma classe, e não contar com um depurador visual para investigar o comportamento do programa em tempo real. Para programas pequenos isso é gerenciável, mas conforme os projetos crescem esse trabalho manual se torna cada vez mais custoso — e é exatamente esse ganho de produtividade que uma IDE (sigla para Integrated Development Environment, Ambiente de Desenvolvimento Integrado) se propõe a entregar.

Uma IDE é um programa que reúne, numa única interface, as principais ferramentas usadas para escrever software: um editor de texto com recursos específicos para código (como destaque de sintaxe e autocompletar), integração com o compilador e com a execução do programa, um depurador (para inspecionar o programa passo a passo durante a execução) e, frequentemente, integração com sistemas de controle de versão como o Git.

Uma IDE resolve isso integrando tudo em um único ambiente: o editor já sinaliza erros de sintaxe e de tipo enquanto o código é digitado, sem esperar uma compilação explícita; oferece autocompletar, mostrando quais métodos e atributos estão disponíveis para um objeto de determinado tipo, o que ajuda especialmente quem ainda está aprendendo a biblioteca padrão do Java; e permite compilar e executar o programa com um único clique ou atalho, sem precisar digitar os comandos manualmente no terminal a cada teste.

As IDEs mais usadas para Java incluem o IntelliJ IDEA (em sua versão Community, gratuita, ou Ultimate, paga), o Eclipse e o Visual Studio Code com extensões específicas para Java. Todas elas, internamente, ainda dependem do JDK instalado na máquina — a IDE não substitui o JDK, apenas oferece uma interface mais produtiva para usá-lo. Por isso, a instalação do JDK, discutida no primeiro conceito deste capítulo, continua sendo um pré-requisito mesmo para quem pretende programar exclusivamente através de uma IDE.

Um exemplo prático dessa integração: ao criar um novo projeto Java no IntelliJ IDEA, a própria IDE pergunta (ou detecta automaticamente) qual JDK instalado na máquina deve ser usado para aquele projeto, e a partir daí passa a chamar `javac` e `java` internamente, sempre que você pede para compilar ou rodar o código, sem que seja necessário abrir o terminal manualmente.

Para quem está começando, é comum a recomendação de aprender antes a compilar e rodar programas manualmente pelo terminal — como foi feito nos capítulos anteriores deste livro — antes de migrar para uma IDE, justamente para entender o que a IDE está automatizando por trás da interface gráfica, evitando que ela vire uma "caixa-preta" incompreendida.

## Primeiro programa

Com o JDK instalado e configurado, chegou o momento de escrever, compilar e executar o primeiro programa Java de verdade. Este capítulo pega o clássico exemplo de exibir uma mensagem na tela e usa esse exemplo mínimo para apresentar, na prática, cada peça que compõe um programa Java: o arquivo-fonte, a classe que o contém, o método especial que serve de ponto de entrada e a instrução usada para imprimir texto no console.

Mais importante do que decorar a sintaxe exata é entender o papel de cada elemento e o caminho que o código percorre: primeiro ele é escrito em um arquivo `.java`, depois compilado pelo `javac` (transformando-se em bytecode), e por fim executado pela JVM através do comando `java` — o mesmo processo explicado de forma abstrata no capítulo sobre JDK, JVM e bytecode, agora visto na prática, comando por comando. Ao terminar este capítulo, o leitor terá escrito, compilado e rodado seu primeiro programa Java com entendimento de cada etapa envolvida.

### Arquivo `.java`

Um arquivo `.java` é o arquivo de texto onde o código-fonte de um programa Java é escrito, usando a extensão `.java` no nome. É a unidade básica de trabalho de qualquer desenvolvedor Java: cada arquivo desse tipo contém, tipicamente, a definição de uma classe (conceito detalhado a seguir), escrita seguindo a sintaxe e as regras gramaticais da linguagem.

Sem essa convenção de extensão e formato, o compilador não teria como saber quais arquivos, dentro de uma pasta cheia de outros tipos de arquivo, deveriam ser tratados como código-fonte Java a ser compilado. A extensão `.java` funciona como um sinal claro, tanto para ferramentas (como o `javac`) quanto para o próprio programador e para IDEs, de que aquele arquivo contém código Java e deve ser tratado de acordo — com destaque de sintaxe apropriado, checagem de erros específica da linguagem, e assim por diante.

Java impõe uma regra importante sobre esses arquivos: se uma classe é declarada como `public` (pública, acessível de fora do próprio arquivo), o nome do arquivo `.java` precisa ser exatamente igual ao nome dessa classe pública, incluindo maiúsculas e minúsculas. Essa regra resolve um problema de organização: ao olhar para o sistema de arquivos, qualquer pessoa consegue prever exatamente onde encontrar o código de uma classe pública específica, sem precisar abrir cada arquivo para descobrir seu conteúdo.

Um exemplo direto: se você declara uma classe pública chamada `Calculadora`, o arquivo precisa se chamar exatamente `Calculadora.java`:

```java
public class Calculadora {
    // conteúdo da classe
}
```

Se o arquivo fosse salvo como `calc.java` ou `Calculadora2.java`, o compilador rejeitaria a compilação, apontando a incompatibilidade entre o nome da classe pública e o nome do arquivo. Essa restrição não se aplica a classes que não são `public` (chamadas de classes "package-private"): é possível ter, dentro de um mesmo arquivo `.java`, uma classe pública que dá nome ao arquivo e outras classes auxiliares, sem essa mesma exigência de nome — desde que exista, no máximo, uma classe pública por arquivo.

### Classe

Classe é o conceito central da orientação a objetos em Java: é o molde, ou modelo, a partir do qual objetos são criados, agrupando dados (chamados de atributos ou campos) e comportamentos (chamados de métodos) relacionados a uma mesma entidade ou responsabilidade. Praticamente todo código Java precisa estar dentro de uma classe — diferente de linguagens como Python ou C, onde é possível escrever instruções soltas diretamente em um arquivo, Java exige que tudo esteja organizado dentro da estrutura de uma classe (com exceções pontuais e modernas, discutidas no próximo capítulo).

Programas maiores, sem esse conceito organizador, rapidamente se tornariam confusos: variáveis e funções soltas, sem nenhuma estrutura que agrupe o que pertence logicamente junto, dificultam tanto a leitura quanto a manutenção do código, especialmente à medida que um projeto cresce e passa a ser mantido por várias pessoas. A classe resolve isso oferecendo uma unidade de organização natural: tudo relacionado a "um cliente", por exemplo, pode viver dentro de uma classe `Cliente`, com seus atributos (nome, e-mail) e métodos (validar dados, calcular algo específico daquele cliente) reunidos em um único lugar coerente.

Para o programa mais simples possível — o "primeiro programa" deste capítulo — a classe funciona apenas como um recipiente obrigatório: mesmo sem atributos nem múltiplos métodos, ela é a estrutura mínima dentro da qual o método `main` (assunto do próximo conceito) precisa estar declarado.

Um exemplo mínimo:

```java
public class Ola {
    public static void main(String[] args) {
        System.out.println("Olá, mundo!");
    }
}
```

Aqui, `Ola` é o nome da classe, e tudo o que o programa faz está contido dentro dela, delimitado pelas chaves `{` e `}`. Ao longo do livro, as classes vão ganhar atributos próprios e vários métodos, representando entidades reais de um problema (como `ContaBancaria`, `Produto`, `Pedido`), mas a estrutura sintática básica — a palavra `class`, um nome e um par de chaves delimitando seu conteúdo — permanece a mesma vista aqui.

### `main`

O método `main` é o ponto de entrada de um programa Java: é o método que a JVM procura e executa automaticamente ao iniciar a execução de um programa através do comando `java`. Sem um método `main` (ou uma das alternativas modernas discutidas no próximo capítulo), a JVM não sabe por onde começar a executar o código, mesmo que o arquivo contenha uma classe válida e corretamente compilada.

Antes de existir essa convenção clara, seria ambíguo determinar qual parte do código deveria rodar primeiro em um programa com várias classes e métodos diferentes. O `main` resolve isso estabelecendo um contrato fixo: a JVM sempre procura, na classe indicada ao comando `java`, um método com a assinatura exata `public static void main(String[] args)`. Cada palavra dessa assinatura tem um papel: `public` permite que a JVM, de fora da classe, consiga acessar e chamar esse método; `static` permite chamá-lo sem antes precisar criar um objeto daquela classe (um passo que, para o método de entrada do programa, ainda não faria sentido exigir); `void` indica que o método não devolve nenhum valor de volta para quem o chamou; e `String[] args` é um parâmetro que recebe argumentos de linha de comando passados ao rodar o programa, mesmo que, na maioria dos programas simples, esse parâmetro não seja usado.

Um exemplo direto, retomando o programa do conceito anterior:

```java
public class Ola {
    public static void main(String[] args) {
        System.out.println("Olá, mundo!");
    }
}
```

Ao rodar `java Ola` no terminal, é exatamente esse método `main` que a JVM localiza e executa primeiro; tudo que está escrito dentro dele, entre as chaves, roda em sequência, de cima para baixo (seguindo, é claro, o fluxo de controle específico de cada instrução — laços e condições, temas de módulos futuros, podem alterar essa ordem linear).

Um programa Java pode ter várias classes, e mais de uma delas pode, tecnicamente, conter um método `main`. Isso não é um erro — significa apenas que existem vários pontos de entrada possíveis dentro do mesmo projeto, e cabe a quem executa o programa escolher, através do comando `java NomeDaClasse`, qual dessas classes (e, portanto, qual `main`) deve ser efetivamente executada naquela chamada.

### `System.out.println`

`System.out.println` é a forma mais comum de exibir uma mensagem no terminal a partir de um programa Java — é, na prática, o primeiro comando que a maioria das pessoas aprende ao começar a programar em qualquer linguagem, geralmente através de um programa que imprime "Olá, mundo!" na tela, como visto nos exemplos deste capítulo.

Sem uma forma de produzir saída visível, um programa rodaria de forma completamente silenciosa, sem nenhuma maneira de confirmar, durante o aprendizado, que ele está de fato fazendo o que deveria. `System.out.println` resolve isso oferecendo um canal simples de comunicação entre o programa e quem o executa, imprimindo texto diretamente no terminal (mais precisamente, na saída padrão do sistema operacional).

Decompondo a expressão: `System` é uma classe da biblioteca padrão do Java, que reúne funcionalidades relacionadas ao sistema em que o programa está rodando; `out` é um atributo dessa classe, que representa o fluxo de saída padrão (a tela do terminal, tipicamente); e `println` é um método chamado sobre esse fluxo de saída, responsável por imprimir o texto recebido como argumento, seguido automaticamente de uma quebra de linha — daí o nome, abreviação de "print line" (imprimir linha).

Um exemplo já visto, e outro com uma variável:

```java
System.out.println("Olá, mundo!");

int idade = 25;
System.out.println("Idade: " + idade);
```

No segundo exemplo, o operador `+` está sendo usado para concatenar (juntar) um texto com o valor de uma variável numérica, resultando na impressão de `Idade: 25`. Esse uso do `+` para combinar texto com outros tipos de dado é um recurso muito comum em Java e será revisitado com mais detalhes no módulo sobre operadores.

Existe também o método `System.out.print` (sem o "ln" no final), que imprime o texto sem adicionar a quebra de linha automática ao final — útil quando se quer que a próxima impressão continue na mesma linha da anterior. A escolha entre `print` e `println` depende exclusivamente de como o programador quer organizar visualmente a saída no terminal.

### `javac`

`javac` é o executável, incluído no JDK, que representa o compilador Java na prática — é o comando que efetivamente traduz um arquivo `.java` (código-fonte) em um arquivo `.class` (bytecode), como discutido em profundidade no capítulo anterior sobre JDK, JVM e bytecode. Neste capítulo, o foco está em como usá-lo concretamente para transformar o primeiro programa escrito em algo executável.

Um arquivo `.java`, por mais correto que esteja, não passa de texto até que algo o transforme: a JVM não consegue executar código-fonte diretamente, apenas bytecode. É `javac` quem faz essa ponte entre o que o programador escreveu e o que a JVM consegue rodar.

O uso mais simples do comando é passar o nome do arquivo `.java` a ser compilado:

```
javac Ola.java
```

Executado dentro da pasta onde está o arquivo `Ola.java`, esse comando gera, na mesma pasta, um arquivo `Ola.class` — desde que o código não contenha erros de sintaxe ou de tipo. Se houver algum problema, o `javac` imprime no terminal uma mensagem indicando o arquivo, a linha e a natureza do erro, e nenhum `.class` é gerado até que o problema seja corrigido e o comando seja rodado novamente.

É importante notar que `javac` só realiza a compilação — ele não executa o programa. Depois de rodar `javac Ola.java` com sucesso, o arquivo `Ola.class` existe na pasta, mas nada foi impresso no terminal ainda; a execução propriamente dita é responsabilidade do comando `java`, tema do próximo conceito. Esse é um ponto de confusão comum para quem está começando: esperar que `javac` já rode o programa, quando na verdade ele apenas prepara o bytecode para ser executado em um passo seguinte e separado.

Para projetos com múltiplos arquivos `.java` que dependem uns dos outros, é possível passar vários nomes de arquivo ao `javac` de uma vez, ou usar um padrão de pastas (curinga) para compilar todos de uma vez — um cenário mais comum à medida que os programas crescem além de uma única classe, mas que não muda o princípio básico: `javac` sempre traduz código-fonte em bytecode, arquivo por arquivo.

### `java`

`java` é o executável, também incluído no JDK, que inicia a JVM e executa um programa Java já compilado — é o comando complementar ao `javac`: enquanto este traduz código-fonte em bytecode, aquele efetivamente roda esse bytecode, produzindo o comportamento e a saída esperados do programa.

O bytecode gerado pela compilação, sozinho, ficaria parado, sem nenhuma forma prática de ser executado a partir do terminal. `java` resolve isso iniciando uma instância da JVM, carregando o arquivo `.class` indicado (e quaisquer outras classes de que ele dependa), localizando dentro dele o método `main` (o ponto de entrada discutido anteriormente neste capítulo) e começando a executar as instruções presentes ali, em sequência.

O uso básico do comando, para o programa deste capítulo, depois de já ter rodado `javac Ola.java` com sucesso:

```
java Ola
```

Um detalhe que costuma confundir iniciantes: ao contrário de `javac`, que recebe o nome do arquivo incluindo a extensão (`Ola.java`), o comando `java` recebe apenas o nome da classe, sem a extensão `.class` (`Ola`, não `Ola.class`). Isso acontece porque, tecnicamente, `java` não está "abrindo um arquivo" da mesma forma que `javac` — ele está pedindo à JVM para carregar e executar uma classe pelo nome, e é a própria JVM, internamente, que localiza o arquivo `.class` correspondente na pasta atual (ou em outros locais configurados, para projetos mais complexos).

Executando o comando acima, o resultado esperado no terminal é a saída produzida pelo `System.out.println` dentro do `main` da classe `Ola`:

```
$ java Ola
Olá, mundo!
```

Se o arquivo `Ola.class` não existir na pasta (por exemplo, porque a compilação com `javac` ainda não foi feita, ou porque falhou por algum erro), o comando `java` reporta um erro dizendo que não conseguiu encontrar ou carregar a classe indicada. Esse ciclo — editar o `.java`, compilar com `javac`, executar com `java` — é o fluxo de trabalho fundamental de qualquer desenvolvimento Java tradicional, e será repetido, ainda que muitas vezes automatizado por uma IDE, ao longo de todo o livro.

## Formas modernas de iniciar programas

O capítulo anterior mostrou a forma tradicional e completa de escrever um programa Java, com sua classe pública, seu método `main` com assinatura fixa e os comandos separados de compilação e execução. Essa forma é a que o leitor vai encontrar na esmagadora maioria do código Java profissional e é por isso que foi ensinada primeiro, mas versões recentes da linguagem introduziram alternativas mais simples, pensadas justamente para reduzir a barreira de entrada de quem está aprendendo.

Este capítulo revisita o método `main` tradicional para comparar com essas novidades: os chamados "Compact Source Files" (arquivos-fonte compactos, que dispensam parte da cerimônia inicial), os "Instance Main Methods" (métodos `main` de instância, ainda mais simplificados) e o JShell, uma ferramenta interativa que permite testar trechos de código Java sem precisar criar um arquivo completo. O capítulo fecha discutindo quando faz sentido usar cada uma dessas abordagens, encerrando o módulo com o leitor equipado tanto para escrever programas "de verdade" quanto para experimentar rapidamente ideias soltas de código.

### `public static void main`

`public static void main(String[] args)` é a assinatura tradicional e completa do método de entrada de um programa Java, já apresentada no capítulo anterior. Ela reúne, em uma única linha, quatro elementos obrigatórios historicamente: o modificador de acesso `public`, o modificador `static`, o tipo de retorno `void` e a lista de parâmetros `String[] args`. Este conceito revisita essa assinatura especificamente para contrastá-la com as formas mais novas e simplificadas de iniciar um programa, introduzidas nas versões recentes do Java e discutidas nos próximos conceitos deste capítulo.

Historicamente, essa assinatura completa era a única forma válida de declarar um ponto de entrada em Java, e ela carrega uma exigência que sempre incomodou quem está aprendendo a linguagem: para escrever até o programa mais simples possível, um iniciante precisa entender (ou pelo menos digitar corretamente, sem entender ainda) conceitos avançados como modificadores de acesso, métodos estáticos e arrays de strings — nenhum dos quais é, de fato, necessário para o problema mais básico de "imprimir uma mensagem na tela".

Java resolveu esse atrito pedagógico, em versões recentes da linguagem, introduzindo alternativas mais enxutas: os Compact Source Files e os Instance Main Methods, tratados nos dois próximos conceitos. Essas alternativas permitem que um primeiro programa seja escrito com uma sintaxe muito mais curta, adiando a necessidade de entender `public`, `static` e `String[] args` para um momento mais avançado do aprendizado, sem abrir mão, mais tarde, da forma completa e tradicional — que continua sendo a única forma válida em código de produção real e a que aparece na esmagadora maioria dos exemplos e materiais já publicados sobre Java.

Um exemplo lado a lado, apenas para fixar a comparação (o programa faz exatamente a mesma coisa):

```java
// Forma tradicional e completa
public class Ola {
    public static void main(String[] args) {
        System.out.println("Olá, mundo!");
    }
}
```

Neste livro, a forma tradicional é ensinada primeiro — como já foi feito no capítulo anterior — justamente porque ela é a que aparece universalmente em qualquer projeto Java real, em qualquer material de estudo mais antigo, e em qualquer entrevista técnica. As formas modernas, vistas a seguir, são um recurso complementar, pensado especialmente para o aprendizado inicial e para scripts pequenos, não um substituto definitivo da assinatura completa.

### Compact Source Files

Compact Source Files ("arquivos-fonte compactos") é um recurso das versões recentes do Java que permite escrever um programa simples sem a necessidade de declarar explicitamente uma classe envolvendo o método principal. Introduzido como parte de um esforço para tornar o primeiro contato com Java mais direto, ele reduz o "código de cerimônia" (boilerplate) que um iniciante precisa escrever antes mesmo de chegar à lógica que de fato importa.

Sem esse recurso, como visto no capítulo anterior, mesmo o programa mais simples exige a declaração explícita de uma classe pública, com nome coincidindo com o do arquivo, além da assinatura completa do `main`. Para quem está apenas testando um trecho pequeno de código ou aprendendo os primeiros passos da linguagem, essa exigência estrutural pode ser uma barreira desnecessária antes de qualquer lógica real aparecer.

Os Compact Source Files resolvem isso permitindo omitir a declaração da classe: o arquivo pode conter diretamente um método `main` solto, sem estar explicitamente dentro de um `class { }`. Por trás dos panos, o compilador ainda gera uma classe implícita para tornar isso executável pela JVM — o recurso não muda o modelo de execução do Java, apenas dispensa o programador de escrever manualmente essa camada estrutural nos casos mais simples.

Um exemplo de Compact Source File equivalente ao programa "Olá, mundo!" já mostrado:

```java
void main() {
    System.out.println("Olá, mundo!");
}
```

Note que, além de dispensar a classe, esse formato também permite uma assinatura simplificada do próprio `main`, sem os modificadores `public static` e sem o parâmetro `String[] args` — combinação que só é válida dentro de um Compact Source File; fora desse contexto, a assinatura completa continua sendo obrigatória.

Esse recurso é voltado principalmente para aprendizado, scripts pequenos e experimentação rápida — não para projetos maiores, com múltiplas classes que precisam se relacionar entre si, cenário em que a organização explícita em classes (como já ensinada nos capítulos anteriores) continua sendo necessária e recomendada. À medida que um programa cresce além de um único arquivo simples, a migração para a estrutura tradicional de classe é natural e, na maioria dos casos, inevitável.

### Instance Main Methods

Instance Main Methods ("métodos main de instância") é outro recurso moderno relacionado ao anterior, que permite declarar o método `main` sem o modificador `static`, tratando-o como um método de instância comum — ou seja, um método que, em circunstâncias normais, só poderia ser chamado a partir de um objeto já criado daquela classe, e não diretamente pela classe em si.

O motivo de existir é o mesmo `static` que sobrou até aqui: mesmo usando um Compact Source File, essa palavra-chave ainda representa uma ideia que um iniciante normalmente só entenderia depois de estudar orientação a objetos com mais profundidade — afinal, `static` é definido, por contraste, em relação ao que é um método de instância, tema que naturalmente aparece mais adiante na progressão didática deste livro. Exigir que o primeiro programa já use um conceito que só fará sentido pleno mais adiante é outra fonte do atrito pedagógico mencionado no primeiro conceito deste capítulo.

O recurso resolve isso permitindo que a JVM, ao localizar um método `main` sem `static` em uma classe (ou em um Compact Source File), crie automaticamente uma instância daquela classe e chame o `main` sobre esse objeto recém-criado, como se fosse qualquer outro método de instância. Isso dispensa o entendimento prévio de `static` para o primeiro contato com a linguagem, sem, no entanto, mudar o comportamento visível do programa mais simples.

Um exemplo, combinando esse recurso com o Compact Source File do conceito anterior:

```java
void main() {
    System.out.println("Olá, mundo!");
}
```

Esse mesmo trecho de código já exemplifica um Instance Main Method: repare que não há `static` na assinatura. Por trás dos panos, a JVM cria uma instância da classe implícita gerada pelo Compact Source File e chama `main` sobre ela — um detalhe que o programador, neste estágio do aprendizado, não precisa administrar manualmente.

Assim como os Compact Source Files, esse recurso foi pensado para reduzir a barreira inicial de aprendizado, não para substituir o `static void main` tradicional em programas reais — a mesma migração para a estrutura convencional conforme o projeto cresce, discutida no conceito anterior, vale aqui.

### JShell

JShell é uma ferramenta interativa incluída no JDK que permite digitar e executar trechos de código Java diretamente no terminal, linha por linha, sem precisar criar um arquivo `.java`, compilá-lo com `javac` e executá-lo com `java` para cada teste. É o que se costuma chamar de um REPL (Read-Eval-Print Loop, ou "ciclo de ler, avaliar e imprimir"), um recurso já comum em outras linguagens, como Python, e que Java passou a oferecer nativamente a partir da versão 9.

Sem uma ferramenta como o JShell, testar um trecho pequeno de código — como verificar rapidamente como um método da biblioteca padrão se comporta, ou conferir o resultado de uma expressão aritmética específica — exigia todo o ciclo completo: criar um arquivo, escrever uma classe inteira com um `main`, compilar e executar, só para descartar tudo em seguida. Para experimentos rápidos e para aprendizado, esse ciclo completo é desproporcional ao tamanho real do que se quer testar.

O JShell resolve isso oferecendo um ambiente onde cada linha digitada é imediatamente avaliada e seu resultado é mostrado na hora, sem exigir a estrutura completa de classe e método `main`. Ele é iniciado digitando `jshell` no terminal (o executável também faz parte do JDK), e a partir daí abre um prompt interativo próprio.

Um exemplo de sessão de JShell:

```
$ jshell
jshell> int idade = 25;
idade ==> 25

jshell> idade + 5
$2 ==> 30

jshell> System.out.println("Olá, mundo!");
Olá, mundo!
```

Repare que o JShell não apenas executa cada linha, mas também mostra automaticamente o valor resultante de expressões, mesmo sem um `System.out.println` explícito — um recurso pensado exatamente para exploração rápida e aprendizado, poupando digitação repetitiva.

O JShell é uma ferramenta de experimentação e aprendizado, não uma forma de desenvolver e organizar programas reais: o código digitado nele não fica salvo automaticamente em arquivos reutilizáveis (embora seja possível salvar e carregar sessões manualmente), e não há, nesse ambiente, o mesmo controle de organização em classes e pacotes que um projeto real exige. Seu papel principal, especialmente para quem está aprendendo, é permitir testar rapidamente uma dúvida pontual sobre a linguagem sem o peso de montar um projeto completo só para isso.

### Quando usar cada abordagem

Depois de conhecer a assinatura tradicional `public static void main`, os Compact Source Files, os Instance Main Methods e o JShell, a pergunta natural é: qual dessas formas usar em cada situação? Este conceito fecha o capítulo amarrando essa decisão prática, já que cada uma das abordagens vistas resolve um problema ligeiramente diferente, e nenhuma delas substitui completamente as outras.

O JShell é a ferramenta certa para dúvidas pontuais e exploração imediata: testar como um método específico se comporta, confirmar o resultado de uma expressão, ou simplesmente experimentar uma ideia antes de decidir como incorporá-la a um programa real. Seu ponto forte é a velocidade de feedback, sem qualquer estrutura de arquivo envolvida; seu limite é justamente não produzir um programa persistente e organizado.

Os Compact Source Files, combinados com Instance Main Methods, são adequados para scripts pequenos e independentes — um utilitário de poucas linhas, um exercício de aprendizado, ou um teste isolado que ainda assim precisa existir como um arquivo executável, mas sem a necessidade de se relacionar com outras classes. Essa abordagem também é especialmente recomendada nas primeiras semanas de aprendizado de Java, exatamente para adiar conceitos como `static`, modificadores de acesso e arrays de argumentos, até que a base da linguagem (variáveis, tipos, operadores, estruturas de controle) já esteja mais consolidada.

A assinatura tradicional e completa, `public static void main(String[] args)`, dentro de uma classe explicitamente declarada, é a forma exigida para qualquer programa Java real de porte mínimo que seja: assim que um projeto passa a ter mais de uma classe colaborando entre si — o que acontece muito rapidamente à medida que um sistema cresce além de um exercício isolado —, essa é a única estrutura válida e amplamente reconhecida em qualquer ambiente profissional, em qualquer ferramenta de build (Maven, Gradle) e em praticamente toda a documentação e literatura já publicada sobre a linguagem.

Este livro, especificamente, prioriza ensinar a forma tradicional desde o início, como já visto no capítulo anterior, justamente para que o aprendizado fique alinhado com o que você vai efetivamente encontrar no mercado e em qualquer projeto Java real daqui em diante. As formas modernas apresentadas neste capítulo são apresentadas como conhecimento complementar — úteis para reconhecer ao encontrá-las em versões recentes de tutoriais e documentações, e ocasionalmente convenientes para testes rápidos —, mas não substituem a necessidade de dominar a estrutura tradicional, que continuará sendo usada em praticamente todo o restante deste livro.

# Módulo 2 — Variáveis, tipos e expressões

Depois de ver um programa Java completo rodar pela primeira vez no módulo anterior, chegou a hora de entender os blocos mais elementares com que qualquer programa é construído: os dados que ele manipula e as operações que faz sobre eles. Este módulo é o alicerce de tudo que envolve manipulação de informação em Java — sem ele, não é possível entender métodos, decisões, repetições ou classes, porque todos esses recursos, em algum momento, armazenam, transformam ou comparam valores.

Os cinco capítulos seguem uma progressão natural: primeiro como declarar e usar variáveis, depois os tipos primitivos que essas variáveis podem assumir, em seguida como escrever valores literais e convertê-los entre tipos, depois os operadores que combinam esses valores em operações, e por fim como esses operadores se combinam em expressões mais complexas, respeitando regras de precedência e associatividade. Ao final deste módulo, o leitor será capaz de armazenar dados de diferentes naturezas (números inteiros, decimais, texto, valores verdadeiro/falso), combiná-los em cálculos e comparações, e entender exatamente como o Java avalia uma expressão com vários operadores — uma competência que sustenta praticamente todo o código escrito dali em diante.

## Variáveis

Todo programa precisa guardar informação temporariamente enquanto está rodando — a idade digitada por um usuário, o resultado de um cálculo, o texto que será exibido na tela. Em Java, esse armazenamento é feito através de variáveis: espaços nomeados na memória que guardam um valor de um tipo específico. Este capítulo abre o módulo justamente por aqui, porque toda a discussão de tipos, operadores e expressões que vem a seguir só faz sentido quando existe algo — uma variável — para guardar o resultado dessas operações.

O capítulo percorre como declarar uma variável, como inicializá-la com um valor, como atribuir um novo valor a ela depois de declarada, e as regras que Java impõe para nomear variáveis (identificadores), incluindo as convenções de nomenclatura amplamente adotadas pela comunidade. São conceitos simples individualmente, mas que formam o vocabulário básico usado em absolutamente todo código Java daqui em diante.

### Declaração

Declaração é o ato de anunciar ao compilador que uma variável vai existir, informando seu tipo e o nome pelo qual ela será referenciada ao longo do código. Em Java, toda variável precisa ser declarada antes de ser usada, e essa declaração sempre inclui um tipo explícito — diferente de linguagens de tipagem dinâmica, onde uma variável pode simplesmente surgir na primeira atribuição, sem anúncio prévio de que tipo de dado ela vai guardar.

Sem uma declaração prévia, o compilador não teria como saber, ao encontrar o nome de uma variável em qualquer ponto do código, que tipo de valor ela deveria conter, nem quanto espaço de memória reservar para ela. Essa exigência pode parecer burocrática à primeira vista, mas é exatamente o que sustenta a tipagem forte de Java: ao declarar `int idade;`, o compilador passa a saber, dali em diante, que qualquer valor atribuído a `idade` precisa ser compatível com números inteiros, e pode sinalizar erro imediatamente caso o código tente atribuir algo incompatível, como um texto.

A sintaxe básica de uma declaração é `tipo nomeDaVariavel;`, opcionalmente seguida de um valor inicial na mesma linha (assunto do próximo conceito). É possível também declarar várias variáveis do mesmo tipo em uma única linha, separando os nomes por vírgula:

```java
int idade;
int quantidade, total;
```

Depois de declarada, uma variável fica disponível para uso dentro do escopo (a região do código) em que foi declarada — um conceito que será aprofundado em módulos futuros sobre métodos e blocos de código, mas que, por ora, basta entender como "a partir da linha em que a variável foi declarada, e até o fechamento do bloco de chaves em que ela está, dentro daquele mesmo bloco".

Um erro comum entre iniciantes é tentar usar uma variável antes de declará-la, ou declarar duas variáveis com o mesmo nome dentro do mesmo escopo — em ambos os casos, o compilador Java rejeita a compilação e aponta o problema, evitando que ambiguidades desse tipo cheguem a rodar. Essa checagem antecipada, novamente, é parte do que torna Java uma linguagem mais previsível para projetos grandes: qualquer variável usada em qualquer ponto do código precisa ter sido declarada de forma clara e única antes daquele ponto.

### Inicialização

Inicialização é o ato de atribuir um primeiro valor a uma variável, seja no mesmo momento em que ela é declarada, seja em um momento posterior, mas antes de qualquer tentativa de uso dela. É um conceito próximo, mas distinto, de declaração (o conceito anterior): declarar cria a variável e define seu tipo; inicializar dá a ela um valor concreto para trabalhar.

Sem inicialização, uma variável local (declarada dentro de um método) existe apenas como um espaço reservado, sem valor definido, e Java trata isso como um problema real: tentar usar uma variável local que ainda não foi inicializada é um erro de compilação, não apenas um comportamento indefinido silencioso, como aconteceria em outras linguagens menos rigorosas. Essa exigência evita uma classe inteira de bugs sutis, onde um programa usaria acidentalmente um valor "lixo" (não definido) por esquecimento de inicialização.

O compilador Java barra esse risco na raiz, exigindo que toda variável local seja inicializada antes do primeiro uso — o que, na prática, costuma levar o programador a já inicializar a variável no mesmo momento da declaração, combinando os dois passos em uma única linha:

```java
int idade = 25;
```

Aqui, `idade` é declarada como `int` e imediatamente inicializada com o valor `25`, tudo em uma única instrução. Também é possível separar os dois passos:

```java
int idade;
idade = 25;
```

Esse segundo formato é válido e às vezes necessário — por exemplo, quando o valor inicial depende de alguma lógica condicional que só é decidida depois da declaração —, mas exige atenção: o compilador só permite o uso da variável depois que alguma inicialização de fato aconteceu em todos os caminhos possíveis de execução até aquele ponto do código, algo que os módulos sobre estruturas de decisão vão explorar com mais profundidade.

Vale notar uma exceção importante a essa regra: atributos de uma classe (variáveis declaradas dentro de uma classe, mas fora de qualquer método, chamadas de campos) recebem automaticamente um valor padrão caso não sejam explicitamente inicializados — zero para tipos numéricos, `false` para booleanos, e assim por diante. Essa regra de valores padrão, no entanto, só vale para campos de classe, não para variáveis locais dentro de métodos, que continuam exigindo inicialização explícita antes do primeiro uso, como descrito acima.

### Atribuição

Atribuição é a operação de dar (ou trocar) o valor guardado em uma variável já existente, usando o operador `=`. É diferente de declaração e de inicialização em um ponto importante: enquanto declaração e inicialização acontecem, tipicamente, uma única vez, no início da vida de uma variável, atribuição pode acontecer quantas vezes forem necessárias ao longo da execução de um programa, sempre que for preciso mudar o valor guardado naquela variável.

Se não fosse possível atribuir novos valores a uma variável já existente, cada variável seria, na prática, uma constante — um valor fixo que nunca muda depois de criado. Isso limitaria fortemente o que um programa pode fazer: contadores que aumentam em um laço de repetição, um total que vai acumulando valores, ou uma variável que armazena temporariamente o resultado mais recente de um cálculo — todos esses cenários, extremamente comuns em qualquer programa real, dependem da capacidade de reatribuir valores a uma mesma variável ao longo do tempo.

Por isso a linguagem permite, a qualquer momento dentro do escopo de uma variável já declarada, trocar o valor que ela guarda, desde que o novo valor seja compatível com o tipo daquela variável:

```java
int idade = 25;
idade = 26; // atribuição: o valor de idade muda de 25 para 26
```

Um detalhe importante do operador `=` em Java: ele não representa uma comparação de igualdade (isso é feito pelo operador `==`, estudado no capítulo sobre operadores) nem uma equação matemática, no sentido de "os dois lados são sempre iguais". Ele representa uma ação: "pegue o valor do lado direito e guarde-o na variável do lado esquerdo". Por isso, uma instrução como `idade = idade + 1;` faz perfeito sentido em Java, mesmo que pareça estranha do ponto de vista de uma equação matemática tradicional: ela lê o valor atual de `idade`, soma 1, e guarda o resultado de volta na mesma variável, efetivamente incrementando-a.

Java também oferece operadores de atribuição compostos, que combinam uma operação aritmética com a atribuição em um único símbolo, como `+=`, `-=`, `*=` e `/=` — por exemplo, `idade += 1;` é equivalente a `idade = idade + 1;`, mas mais compacto. Esses operadores compostos serão detalhados no capítulo sobre operadores, mas já valem a menção aqui como uma forma abreviada de atribuição frequentemente usada no dia a dia.

### Identificadores

Identificador é o nome dado a uma variável (ou a uma classe, método, ou outros elementos do código) para que ela possa ser referenciada em outras partes do programa. Java impõe um conjunto claro de regras sobre o que é ou não um identificador válido, e entender essas regras evita erros de compilação triviais, mas frustrantes, para quem está começando.

Sem regras claras sobre o que constitui um nome válido, o compilador teria dificuldade em distinguir, de forma não ambígua, onde um nome de variável termina e onde começa outro elemento da sintaxe da linguagem — por exemplo, um espaço, um número solto, ou uma palavra reservada usada em outro contexto. As regras de identificador resolvem essa ambiguidade de forma objetiva e mecânica.

Em Java, um identificador válido pode conter letras, dígitos, o cifrão (`$`) e o sublinhado (`_`), mas precisa necessariamente começar com uma letra, um cifrão ou um sublinhado — nunca com um dígito. Identificadores também são sensíveis a maiúsculas e minúsculas (`idade` e `Idade` são dois nomes diferentes para o compilador), e não podem coincidir com nenhuma das palavras reservadas da linguagem, como `class`, `int`, `public` ou `static`, já que essas palavras têm significado fixo e especial na gramática de Java.

Alguns exemplos de identificadores válidos e inválidos:

```java
int idade;       // válido
int _total;      // válido
int $valor;      // válido, mas pouco comum na prática
int 2idade;      // inválido: começa com dígito
int int;         // inválido: "int" é palavra reservada
```

Vale notar que identificadores válidos em Java podem, tecnicamente, incluir caracteres acentuados e outros caracteres Unicode, mas a convenção amplamente adotada — e praticamente universal no código profissional — é restringir nomes a letras sem acento do alfabeto latino, dígitos e sublinhado, evitando problemas de compatibilidade entre diferentes editores, terminais e configurações de codificação de texto.

Escolher bons identificadores é uma prática importante, ainda que o compilador não exija nada além das regras mecânicas acima: um nome como `idade` comunica imediatamente o propósito da variável, enquanto um nome como `x` ou `a1` exige que quem lê o código descubra, pelo contexto, o que aquela variável representa. Esse cuidado com nomes significativos é retomado, de forma mais sistemática, no próximo conceito, sobre convenções de nomes.

### Convenções de nomes

Convenções de nomes são um conjunto de práticas amplamente adotadas pela comunidade Java (documentadas oficialmente pela Oracle) para escolher identificadores de forma consistente, mesmo quando a linguagem, por si só, não obriga seguir esse padrão. Diferente das regras de identificador vistas no conceito anterior — que são exigências do compilador, e cujo desrespeito impede a compilação —, convenções de nomes são acordos sociais dentro da comunidade de desenvolvedores: o código compila normalmente mesmo se não forem seguidas, mas ignorá-las torna o código mais difícil de ler e destoante do que qualquer outro programador Java experiente esperaria encontrar.

Cada desenvolvedor (ou cada equipe), sem um padrão compartilhado, acabaria inventando seu próprio estilo de nomeação, tornando qualquer código-fonte de terceiros mais difícil de ler rapidamente — cada arquivo exigiria que quem o lê primeiro "aprendesse" a convenção específica usada ali antes de conseguir entender o código com fluidez. Isso é particularmente custoso em Java, uma linguagem historicamente usada em projetos grandes, mantidos por muitas pessoas ao longo de anos.

A convenção mais importante em Java é o uso do chamado camelCase para nomes de variáveis e métodos: a primeira palavra começa com letra minúscula, e cada palavra seguinte, dentro do mesmo nome composto, começa com letra maiúscula, sem espaços ou sublinhados separando-as — por exemplo, `nomeCompleto`, `calcularTotal`, `quantidadeDeItens`. Para nomes de classes, a convenção é o PascalCase (também chamado de UpperCamelCase), em que até a primeira palavra começa com maiúscula — por exemplo, `ContaBancaria`, `Calculadora`. Já constantes (variáveis cujo valor nunca muda depois de definido, um conceito formalizado através da palavra-chave `final`, estudada em módulos futuros) seguem a convenção de letras maiúsculas separadas por sublinhado, como `TAXA_MAXIMA` ou `LIMITE_TENTATIVAS`.

Um exemplo reunindo essas convenções:

```java
public class ContaBancaria {
    static final double TAXA_JUROS = 0.02;

    public void calcularSaldoFinal() {
        double saldoAtual = 1000.0;
        // ...
    }
}
```

Além do estilo de capitalização, boas convenções de nomes também recomendam escolher nomes descritivos e pronunciáveis, evitando abreviações obscuras (`qtdIt` em vez de `quantidadeDeItens`, por exemplo) e evitando nomes de uma única letra, exceto em contextos muito específicos e universalmente reconhecidos, como `i` em contadores de laços de repetição simples — tema que será revisitado mais adiante, quando chegarmos às estruturas de repetição. Seguir essas convenções, desde o início do aprendizado, cria o hábito que vai tornar qualquer código futuro mais fácil de ler, tanto para outras pessoas quanto para o próprio programador, ao revisitar seu código meses depois.

## Tipos primitivos

No capítulo anterior, toda variável foi declarada com um tipo, mas sem que esse tipo fosse explicado em detalhe. Este capítulo preenche essa lacuna: Java é uma linguagem de tipagem estática, o que significa que toda variável precisa ter um tipo definido no momento em que é declarada, e esse tipo determina que espécie de valor ela pode guardar, quanto espaço de memória ocupa e quais operações são válidas sobre ela. Os tipos primitivos são os tipos mais básicos e eficientes da linguagem — não representam objetos complexos, apenas valores simples armazenados diretamente na memória.

A razão de existirem vários tipos primitivos, e não um único tipo numérico genérico, é uma questão de precisão e de uso de memória: um contador que nunca passa de 100 não precisa do mesmo espaço que um valor que representa a população de um país, e um cálculo financeiro que exige casas decimais tem necessidades diferentes de um simples "verdadeiro ou falso". Este capítulo apresenta, um a um, os oito tipos primitivos de Java — `byte`, `short`, `int`, `long`, `float`, `double`, `char` e `boolean` — cobrindo faixa de valores, uso típico e tamanho ocupado por cada um, para que o leitor saiba escolher o tipo adequado a cada situação em vez de usar sempre o mesmo por hábito.

### `byte`

`byte` é o menor dos tipos primitivos numéricos inteiros de Java, ocupando 8 bits (1 byte) de memória e capaz de representar valores inteiros entre -128 e 127. Ele faz parte do grupo de tipos primitivos — os tipos de dado mais básicos da linguagem, que armazenam diretamente um valor (e não uma referência a um objeto, diferente de tipos como `String`), e que serão apresentados neste capítulo em ordem crescente de capacidade.

Sem um tipo especificamente pequeno como `byte`, qualquer número inteiro, por menor que fosse, precisaria ser guardado em um tipo maior (como `int`, de 32 bits), desperdiçando memória em situações onde se sabe, de antemão, que os valores envolvidos são pequenos — por exemplo, ao processar dados brutos vindos de um arquivo binário, onde cada unidade de dado já vem naturalmente do tamanho de um byte, ou ao armazenar grandes coleções de números pequenos, como componentes de cor RGB (cada um variando de 0 a 255, ainda que, na prática, `byte` sendo `signed` — indo até 127 — leve muitos códigos a usar `int` mesmo nesses casos por simplicidade).

O tipo `byte` resolve o problema de economia de memória em cenários de grande volume de dados pequenos: ao declarar um array com milhões de elementos do tipo `byte` em vez de `int`, a economia de memória é de quatro vezes, já que `int` ocupa 32 bits contra os 8 bits de `byte`. Isso é relevante especialmente em processamento de arquivos binários, comunicação em rede de baixo nível, e leitura de imagens ou áudio, onde o dado já chega naturalmente na granularidade de um byte.

Um exemplo:

```java
byte idade = 25;
byte temperaturaMinima = -10;
```

Um cuidado importante: como o intervalo de `byte` é bastante restrito (apenas 256 valores possíveis, de -128 a 127), operações aritméticas simples podem facilmente ultrapassar esse limite, gerando o que se chama de overflow — um comportamento detalhado em conceito próprio mais adiante neste módulo. Por essa limitação, `byte` é usado com menos frequência do que `int` no dia a dia de quem está aprendendo Java; seu uso mais comum e justificado é justamente em cenários de manipulação de dados binários brutos, não em variáveis comuns do tipo "idade de uma pessoa" ou "quantidade de itens", onde `int` é a escolha padrão, mesmo que tecnicamente um `byte` já fosse suficiente.

### `short`

`short` é um tipo primitivo numérico inteiro intermediário entre `byte` e `int`, ocupando 16 bits (2 bytes) de memória e capaz de representar valores inteiros entre -32.768 e 32.767. Ele resolve uma faixa de necessidade que fica entre a economia extrema de `byte` e a capacidade mais ampla de `int`: situações em que os valores envolvidos são um pouco maiores do que cabem em um `byte`, mas ainda claramente menores do que a faixa completa de um `int` seria capaz de comportar.

Sem esse tipo intermediário, o programador teria que escolher entre `byte` (arriscando overflow frequente para valores moderadamente grandes) ou `int` (usando o dobro da memória necessária) em cenários onde nenhuma das duas opções é um ajuste perfeito. `short` preenche essa lacuna, embora, na prática do dia a dia de programação em Java, seja o tipo primitivo menos usado de todos: a maioria dos desenvolvedores, mesmo sabendo que um valor caberia em um `short`, prefere usar `int` por padrão, reservando `short` para situações bem específicas de otimização de memória em grandes volumes de dados, ou para interoperar com formatos de arquivo e protocolos de rede que definem explicitamente campos de 16 bits.

Um exemplo:

```java
short anoNascimento = 1998;
short quantidadeMaxima = 30000;
```

Como acontece com `byte`, o intervalo limitado de `short` significa que ele está mais sujeito a overflow do que `int` ou `long` ao lidar com valores que crescem durante a execução de um programa — por exemplo, um contador que começa pequeno mas pode, dependendo da lógica do programa, ultrapassar 32.767 ao longo do tempo. Por essa razão — e pelos mesmos motivos que já valem para `byte` —, a recomendação é reservar `short` para os cenários específicos de economia de memória ou interoperabilidade com formatos binários mencionados acima, e usar `int` como padrão no restante do código, como o próximo conceito explica em detalhe.

### `int`

`int` é o tipo primitivo numérico inteiro mais usado em Java, ocupando 32 bits (4 bytes) de memória e capaz de representar valores inteiros entre aproximadamente -2,1 bilhões e 2,1 bilhões (mais precisamente, de -2.147.483.648 a 2.147.483.647). É, na prática, o tipo padrão para representar quantidades inteiras em Java — idades, contagens, índices de posições em uma lista, e uma enorme variedade de outros usos cotidianos.

O motivo de `int` ser tão dominante, mesmo havendo `byte`, `short` e `long` disponíveis, é um equilíbrio prático: sua faixa de valores é ampla o suficiente para cobrir a esmagadora maioria das necessidades do dia a dia (poucos programas comuns lidam com contagens acima de 2 bilhões), e seu tamanho de 32 bits é bem alinhado ao processamento nativo da maioria dos processadores modernos, tornando operações com `int` tipicamente as mais eficientes entre os tipos inteiros disponíveis. Antes de decidir entre `byte`, `short`, `int` ou `long` para uma variável nova, a prática recomendada em Java é justamente começar assumindo `int`, e só considerar outro tipo se houver uma razão concreta — necessidade de valores maiores (então `long`) ou economia de memória comprovadamente relevante (então `byte` ou `short`).

Um exemplo típico de uso:

```java
int idade = 25;
int quantidadeDeAlunos = 150;
int anoAtual = 2026;
```

Todos os exemplos de código com variáveis inteiras usados nos capítulos anteriores deste livro já utilizaram `int` justamente por essa razão: é o tipo natural para representar um número inteiro comum, sem necessidade de justificativa adicional.

Assim como `byte` e `short`, `int` também está sujeito a overflow ao ultrapassar sua faixa de valores — um risco menos frequente no dia a dia por conta de sua faixa ampla, mas ainda relevante em cálculos que envolvem multiplicações grandes ou acumulações ao longo de muitas repetições, tema aprofundado em conceito específico deste módulo. Quando há certeza de que um valor pode ultrapassar a faixa de `int` — por exemplo, ao contar visualizações de um vídeo muito popular, ou ao calcular fatoriais de números moderadamente grandes —, a escolha correta passa a ser `long`, tipo apresentado no próximo conceito.

### `long`

`long` é o maior tipo primitivo numérico inteiro de Java, ocupando 64 bits (8 bytes) de memória e capaz de representar valores inteiros em uma faixa muito mais ampla que `int` — de aproximadamente -9,2 quintilhões a 9,2 quintilhões (mais precisamente, de -9.223.372.036.854.775.808 a 9.223.372.036.854.775.807). Ele existe para cobrir os casos em que a faixa de `int`, apesar de já ser ampla, não é suficiente.

Sem `long`, qualquer cálculo que envolvesse números muito grandes — contagem de milissegundos desde uma data de referência (uma necessidade extremamente comum em programação, usada, por exemplo, para representar instantes de tempo), somatórios acumulados de grandes volumes de transações financeiras, ou identificadores únicos gerados sequencialmente em sistemas de grande escala — correria risco constante de overflow se restrito a um `int`, produzindo resultados incorretos e silenciosos (o overflow em tipos inteiros primitivos de Java não gera erro, apenas "dá a volta" no valor, como será detalhado no conceito sobre overflow).

`long` resolve isso oferecendo uma faixa de valores drasticamente maior, ao custo de usar o dobro da memória de um `int` para cada valor armazenado. Uma peculiaridade importante da sintaxe de Java: um valor literal inteiro (escrito diretamente no código, como `100`) é interpretado, por padrão, como `int`; para que um literal seja tratado como `long`, é necessário adicionar o sufixo `L` (ou `l`, embora `L` maiúsculo seja fortemente recomendado, já que `l` minúsculo é visualmente quase idêntico ao número `1`) ao final do número — assunto que será revisitado com mais detalhes no próximo capítulo, sobre literais e conversões.

Um exemplo comum, envolvendo um valor que ultrapassaria a faixa de `int`:

```java
long populacaoMundial = 8_100_000_000L;
long timestampAtual = System.currentTimeMillis();
```

O segundo exemplo usa um método real e muito comum da biblioteca padrão, `System.currentTimeMillis()`, que devolve o número de milissegundos passados desde uma data de referência fixa (1º de janeiro de 1970) — um valor que já ultrapassa a faixa de um `int` há muitos anos, por isso esse método sempre devolve um `long`.

A recomendação prática é semelhante à de `short`: usar `int` como padrão, e migrar para `long` apenas quando há indício claro (ou certeza matemática) de que os valores envolvidos podem ultrapassar a faixa de um `int` — seja por lidar com contagens muito grandes, seja por trabalhar diretamente com timestamps e identificadores de sistemas de larga escala, contextos em que `long` é a escolha padrão e esperada no ecossistema Java.

### `float`

`float` é um tipo primitivo numérico usado para representar números com casas decimais (números de ponto flutuante), ocupando 32 bits de memória. Ele é o primeiro tipo de ponto flutuante apresentado neste capítulo, e sua função é resolver uma limitação fundamental dos tipos inteiros vistos até aqui: nenhum deles (`byte`, `short`, `int`, `long`) é capaz de representar valores fracionários, como `3.14` ou `0.5`.

Sem um tipo de ponto flutuante, seria impossível representar diretamente medidas contínuas — preços com centavos, médias, porcentagens, resultados de divisões que não sejam exatas — sem recorrer a soluções artificiais, como representar centavos como um número inteiro separado (uma técnica, aliás, ainda usada em alguns contextos financeiros específicos, exatamente para evitar as imprecisões de ponto flutuante mencionadas a seguir). `float` resolve o problema mais geral de representar esses valores fracionários diretamente na linguagem.

Um ponto importante sobre `float` (e, de forma equivalente, sobre `double`, tratado no próximo conceito): números de ponto flutuante não representam valores decimais com precisão perfeita internamente — eles usam uma representação binária aproximada, definida por um padrão técnico chamado IEEE 754. Isso significa que operações aparentemente simples, como somar `0.1` e `0.2`, podem produzir um resultado ligeiramente diferente de `0.3` exato, devido a arredondamentos internos dessa representação binária. Esse comportamento não é um defeito de Java especificamente — é uma característica de como praticamente toda linguagem de programação moderna representa números de ponto flutuante em hardware.

Um exemplo de uso, já mostrando a exigência sintática de `float`:

```java
float preco = 19.99f;
```

Note o sufixo `f` (ou `F`) ao final do literal `19.99f`. Isso é necessário porque, assim como um literal inteiro é `int` por padrão, um literal com casas decimais é `double` por padrão em Java (o tipo do próximo conceito, com o dobro da precisão de `float`); sem o sufixo `f`, o compilador tentaria atribuir um valor `double` a uma variável `float`, o que gera um erro de compilação por perda potencial de precisão — tema também revisitado no próximo capítulo, sobre conversões.

Na prática do dia a dia em Java, `float` é usado com bem menos frequência do que `double`: sua menor precisão só compensa em cenários muito específicos de economia de memória em larga escala (como processamento gráfico, onde grandes quantidades de números de ponto flutuante precisam ser armazenadas com o menor custo possível), e a recomendação geral para a maioria dos programas comuns é usar `double`, que resolve praticamente as mesmas necessidades com mais precisão e é, de fato, o tipo padrão que o próprio Java assume para literais decimais.

### `double`

`double` é o tipo primitivo de ponto flutuante mais usado em Java, ocupando 64 bits de memória (o dobro de `float`, daí o nome "double" — duplo) e oferecendo, consequentemente, uma precisão bem maior para representar números com casas decimais. É o tipo padrão para valores fracionários em Java, da mesma forma que `int` é o padrão para valores inteiros.

A razão de `double` ser preferido a `float` na grande maioria dos casos é semelhante à razão de `int` ser preferido a `byte` ou `short`: sua precisão mais alta reduz significativamente o acúmulo de erros de arredondamento em cálculos sucessivos, um problema que se torna mais perceptível quanto mais operações são encadeadas sobre o mesmo valor. Como `double` já é, por padrão, o tipo assumido para qualquer literal decimal escrito no código (como `19.99`, sem sufixo algum), usá-lo elimina a necessidade do sufixo `f` exigido por `float`, tornando o código ligeiramente mais direto de escrever também.

Um exemplo:

```java
double preco = 19.99;
double media = (85.5 + 92.0 + 78.3) / 3;
```

Assim como `float`, `double` compartilha a limitação de representação aproximada baseada no padrão IEEE 754: cálculos com `double` podem produzir pequenas imprecisões, especialmente perceptíveis em somas repetidas de valores fracionários. Por isso, em contextos onde a precisão decimal exata é absolutamente crítica — cálculos financeiros que envolvem centavos, por exemplo, onde um erro de arredondamento pode ter implicações contábeis ou legais reais — Java oferece uma alternativa mais robusta, a classe `BigDecimal`, que não é um tipo primitivo, mas um objeto especializado que realiza aritmética decimal com precisão exata, ao custo de maior complexidade de uso e menor desempenho. `BigDecimal` está fora do escopo deste módulo introdutório, mas vale já saber que ela existe como alternativa quando a imprecisão de `double` se torna inaceitável.

Para a grande maioria dos programas comuns — cálculos científicos, médias, medidas físicas, e qualquer contexto onde uma pequena imprecisão de arredondamento não tem consequências sérias —, `double` é a escolha padrão e recomendada em Java sempre que um valor fracionário precisa ser representado, exatamente da mesma forma que `int` é a escolha padrão para valores inteiros comuns.

### `char`

`char` é o tipo primitivo usado para representar um único caractere em Java, ocupando 16 bits de memória. Diferente do que se poderia imaginar por comparação com outras linguagens (como C, onde `char` costuma ocupar apenas 8 bits e representa basicamente um byte), o `char` de Java usa 16 bits especificamente para conseguir representar, nativamente, qualquer caractere do padrão Unicode — incluindo letras acentuadas, caracteres de alfabetos não latinos, e muitos símbolos, não apenas o conjunto básico de caracteres ASCII (letras sem acento, dígitos e alguns símbolos comuns).

Sem um tipo dedicado a caracteres individuais, representar texto exigiria sempre recorrer a um tipo mais complexo, como uma sequência de caracteres (uma `String`, o tipo usado para texto composto por várias letras, que será estudado em módulo futuro). Para situações em que se precisa lidar com um único caractere isolado — verificar se uma letra digitada é uma vogal, processar um texto caractere por caractere, ou comparar um caractere específico dentro de uma posição de uma palavra — ter um tipo primitivo dedicado a essa unidade simplifica o código e evita a sobrecarga de trabalhar com uma `String` de tamanho um só para representar um único símbolo.

Um caractere literal em Java é escrito entre aspas simples (diferente de uma `String`, que usa aspas duplas):

```java
char letraInicial = 'J';
char digito = '7';
```

Um detalhe interessante e às vezes surpreendente para quem está começando: internamente, `char` é armazenado como um número — mais especificamente, o código numérico daquele caractere na tabela Unicode. Isso significa que é possível realizar operações aritméticas diretamente sobre valores `char`, já que, por trás dos panos, eles são tratados como números inteiros de 16 bits sem sinal:

```java
char letra = 'A';
char proximaLetra = (char) (letra + 1); // 'B'
```

Esse exemplo soma 1 ao código numérico de `'A'`, obtendo o código correspondente a `'B'` na tabela de caracteres, e depois converte o resultado de volta para `char` explicitamente (usando um casting, técnica detalhada no próximo capítulo) — Java não faz essa conversão de volta automaticamente, já que somar um `char` com um número inteiro produz, por padrão, um resultado do tipo `int`.

É importante não confundir `char` (um único caractere) com `String` (uma sequência de zero ou mais caracteres): `'A'` é um `char` válido, enquanto `"A"`, com aspas duplas, já é uma `String` contendo um único caractere — dois tipos diferentes, com comportamentos e usos distintos, mesmo representando, à primeira vista, "a mesma coisa" visualmente no código.

### `boolean`

`boolean` é o tipo primitivo usado para representar valores lógicos, capazes de assumir apenas dois estados possíveis: `true` (verdadeiro) ou `false` (falso). É, conceitualmente, o mais simples dos tipos primitivos de Java, mas também um dos mais usados na prática, já que sustenta toda a lógica de decisão de um programa — comparações, condições e controle de fluxo, temas que serão explorados em profundidade em módulos futuros sobre estruturas de decisão.

Antes de existir um tipo dedicado a valores lógicos, um programa só conseguia representar "verdadeiro" e "falso" por meio de convenções artificiais sobre outros tipos — como usar o número `1` para verdadeiro e `0` para falso, uma prática comum em linguagens mais antigas, como as primeiras versões de C. Essa convenção, apesar de funcional, abre espaço para ambiguidade: um valor numérico qualquer, como `5` ou `-3`, poderia ser interpretado de forma inconsistente como "verdadeiro" dependendo do contexto ou da implementação específica, tornando o código menos claro e mais propenso a erros de interpretação.

`boolean` resolve isso oferecendo um tipo exclusivo, com apenas dois valores possíveis e sem qualquer conversão implícita para ou a partir de tipos numéricos — diferente de linguagens como C, em Java não é possível atribuir `1` ou `0` a uma variável `boolean`, nem usar um `int` diretamente onde um `boolean` é esperado. Essa rigidez elimina a ambiguidade mencionada acima: um valor `boolean` em Java é sempre, de forma inequívoca, `true` ou `false`, nunca um número disfarçado de valor lógico.

Um exemplo de uso:

```java
boolean ativo = true;
boolean maiorDeIdade = false;
```

O uso mais comum de `boolean`, no entanto, não é atribuir diretamente `true` ou `false` como valores literais, mas guardar o resultado de uma expressão relacional ou lógica — como uma comparação entre dois números, tema do capítulo seguinte, sobre operadores:

```java
int idade = 20;
boolean maiorDeIdade = idade >= 18;
```

Aqui, `maiorDeIdade` recebe o resultado da comparação `idade >= 18`, que a JVM avalia e resolve diretamente para `true` ou `false`, dependendo do valor de `idade` naquele momento. É esse tipo de uso — guardar e combinar o resultado de comparações — que torna `boolean` a base de praticamente toda tomada de decisão em um programa Java, preparando o terreno para as estruturas condicionais (`if`, `else`) que serão estudadas em módulos posteriores deste livro.

## Literais e conversões

Depois de conhecer os tipos primitivos, a pergunta natural é: como escrever, diretamente no código, um valor de cada um desses tipos, e o que acontece quando um valor de um tipo precisa ser usado onde outro tipo é esperado? Este capítulo responde às duas perguntas. A primeira parte trata dos literais — a forma textual como um valor fixo aparece no código-fonte, como `42`, `3.14` ou `'a'` — e dos sufixos usados para indicar explicitamente o tipo de alguns desses literais.

A segunda parte do capítulo trata de conversões entre tipos, um tema que gera bastante confusão em quem está começando: quando o Java converte um valor automaticamente (conversão implícita), quando é preciso pedir a conversão explicitamente (casting), como funciona a promoção numérica em expressões com tipos mistos, e o que é overflow — quando um valor ultrapassa a capacidade do tipo que deveria armazená-lo. Entender essas regras evita erros sutis de compilação e de comportamento que aparecem justamente na fronteira entre tipos diferentes.

### Literais

Literal é qualquer valor escrito diretamente no código-fonte, representando a si mesmo, sem depender de uma variável ou de um cálculo — como `25`, `19.99`, `'A'`, `"texto"` ou `true`. É a forma mais direta de introduzir um valor concreto dentro de um programa, e todos os exemplos de código usados até aqui neste livro já continham literais, ainda que o termo não tivesse sido formalmente apresentado.

Sem literais, seria impossível dar a uma variável um valor inicial concreto sem antes calculá-lo a partir de outra fonte de dado — toda inicialização, em algum ponto da cadeia, remonta a um valor literal escrito em algum lugar do código, mesmo que indiretamente (por exemplo, um valor lido de um arquivo, em algum momento, precisou ser digitado por alguém como texto literal naquele arquivo). Literais são, portanto, o ponto de partida mais básico de qualquer dado dentro de um programa.

Java reconhece diferentes tipos de literais, cada um com sua própria sintaxe: literais inteiros (`25`, `100`), literais de ponto flutuante (`19.99`), literais de caractere, sempre entre aspas simples (`'A'`), literais de texto (`String`), sempre entre aspas duplas (`"Olá"`), e literais booleanos (`true` e `false`). Cada tipo de literal, por padrão, é interpretado pelo compilador como um tipo primitivo específico — um literal inteiro sem sufixo é sempre `int`, e um literal com casas decimais sem sufixo é sempre `double`, como já mencionado no capítulo anterior.

Java também oferece literais inteiros em bases numéricas diferentes da decimal usual, através de prefixos específicos: `0x` para hexadecimal (`0xFF` equivale a 255 em decimal), `0` seguido de dígitos para octal (menos comum na prática atual), e `0b` para binário (`0b1010` equivale a 10 em decimal) — um recurso útil especialmente ao trabalhar com manipulação de bits ou representações que naturalmente fazem mais sentido em outra base numérica.

Um exemplo reunindo diferentes tipos de literais:

```java
int quantidade = 25;
double preco = 19.99;
char inicial = 'J';
String nome = "Java";
boolean ativo = true;
int corHex = 0xFF0000; // vermelho, em hexadecimal
```

Um recurso adicional, disponível para literais numéricos em Java, é o uso do sublinhado (`_`) como separador visual dentro do próprio número, sem afetar seu valor — por exemplo, `1_000_000` é exatamente igual a `1000000`, mas mais fácil de ler rapidamente à primeira vista, algo especialmente útil em números grandes. Esse recurso será revisitado, com mais um exemplo prático, no conceito sobre sufixos, a seguir.

### Sufixos

Sufixo, no contexto de literais numéricos em Java, é uma letra adicionada ao final de um valor literal para indicar explicitamente qual tipo primitivo aquele valor deve assumir, sobrescrevendo o tipo padrão que o compilador assumiria automaticamente. Já foram mencionados, em conceitos anteriores deste módulo, dois sufixos específicos: `L` (para indicar que um literal inteiro deve ser tratado como `long`, não `int`) e `f` (para indicar que um literal decimal deve ser tratado como `float`, não `double`).

Sem sufixos, o programador ficaria restrito aos tipos padrão que Java assume automaticamente para cada categoria de literal — sempre `int` para inteiros, sempre `double` para decimais —, mesmo em situações em que o tipo de destino desejado é outro. Isso geraria erros de compilação frequentes ao tentar, por exemplo, atribuir um número grande diretamente a uma variável `long`, ou um valor decimal diretamente a uma variável `float`, já que o literal, por padrão, teria um tipo diferente do esperado pela variável.

Os sufixos resolvem isso permitindo que o próprio literal já "declare" seu tipo pretendido, no momento em que é escrito, eliminando a necessidade de conversões explícitas adicionais nesses casos comuns. Além de `L` e `f`, Java também reconhece o sufixo `d` (ou `D`) para reforçar explicitamente que um literal é `double` — embora, como `double` já seja o padrão, esse sufixo raramente seja necessário na prática, exceto por clareza deliberada em algum contexto específico.

Alguns exemplos:

```java
long populacao = 8_100_000_000L;   // sufixo L: literal tratado como long
float preco = 19.99f;               // sufixo f: literal tratado como float
double media = 87.5d;               // sufixo d: opcional, double já é o padrão
```

Repare, no primeiro exemplo, o uso combinado do separador visual `_` mencionado no conceito anterior junto com o sufixo `L`: `8_100_000_000L` é lido com muito mais facilidade do que `8100000000L`, e ambos representam exatamente o mesmo valor numérico — o sublinhado é apenas um auxílio visual para quem lê o código, completamente ignorado pelo compilador ao interpretar o valor.

Um erro comum entre iniciantes é escrever um valor claramente destinado a `long` sem o sufixo `L`, como em `long grande = 9999999999;`. Como esse literal, sem sufixo, é interpretado como `int` por padrão, e o valor `9999999999` ultrapassa a faixa máxima de um `int`, o compilador rejeita essa linha com um erro, mesmo que a variável de destino seja `long` — o problema está no literal em si, avaliado isoladamente antes mesmo de qualquer atribuição, e a correção é simplesmente adicionar o sufixo `L` ao final do número.

### Conversões implícitas

Conversão implícita (também chamada de widening, ou "alargamento") é a transformação automática de um valor de um tipo primitivo menor para um tipo primitivo maior, realizada pelo próprio compilador sem exigir nenhuma instrução explícita do programador, sempre que essa conversão não corre risco de perda de informação. É o tipo de conversão mais simples e seguro entre tipos primitivos numéricos em Java.

Imagine ter que converter manualmente, a cada operação, valores de tipos diferentes antes de combiná-los — somar um `int` com um `long`, por exemplo, exigiria transformar explicitamente o `int` em `long` antes da soma, mesmo sabendo que essa conversão específica nunca causa perda de dado (já que a faixa de `long` engloba completamente a faixa de `int`). Exigir essa conversão manual em todo caso seguro tornaria o código desnecessariamente verboso para operações extremamente comuns.

Por isso o compilador realiza automaticamente conversões entre tipos numéricos sempre que o tipo de destino consegue representar, sem perda, qualquer valor do tipo de origem. A ordem crescente de "abrangência" entre os tipos numéricos primitivos é, aproximadamente: `byte` → `short` → `int` → `long` → `float` → `double` (com uma ressalva sobre `char`, que converte implicitamente para `int` e tipos maiores, mas não participa diretamente dessa cadeia a partir de `byte` ou `short`). Um valor de um tipo mais à esquerda nessa cadeia pode sempre ser atribuído a uma variável de um tipo mais à direita, sem necessidade de conversão explícita.

Um exemplo:

```java
int quantidade = 100;
long quantidadeGrande = quantidade; // conversão implícita: int para long
double media = quantidadeGrande;    // conversão implícita: long para double
```

Nenhuma dessas duas atribuições exige sintaxe especial — o compilador realiza a conversão automaticamente, porque em ambos os casos o tipo de destino consegue representar fielmente qualquer valor que o tipo de origem poderia ter.

Vale uma ressalva sobre a conversão de tipos inteiros (`long`) para tipos de ponto flutuante (`float`, `double`): embora tecnicamente permitida como implícita, ela pode, em casos extremos com números muito grandes, envolver uma pequena perda de precisão, já que nem todo valor `long` tem uma representação exata em ponto flutuante. Ainda assim, Java classifica essa conversão como implícita porque, na prática, a perda (quando ocorre) é considerada aceitável e o caso mais comum não gera surpresas perceptíveis — diferente das conversões na direção oposta, que exigem instrução explícita do programador, tema do próximo conceito.

### Casting

Casting (também chamado de conversão explícita, ou narrowing quando reduz a capacidade do tipo) é a instrução explícita, escrita pelo programador, para converter um valor de um tipo para outro quando essa conversão não é automática — tipicamente porque ela pode causar perda de informação, e Java, propositalmente, não realiza esse tipo de conversão sem que o programador declare, de forma explícita, que está ciente do risco.

Imagine guardar em uma variável `int` um valor que está atualmente armazenado em uma variável `double`, mesmo quando o programador sabe, no contexto específico daquele programa, que a perda das casas decimais é aceitável (ou até intencional, como ao truncar um valor para exibir apenas a parte inteira). Sem uma forma de autorizar explicitamente essa conversão, o compilador simplesmente rejeitaria a atribuição, por segurança, já que essa direção de conversão pode perder informação de forma silenciosa se não for cuidadosamente intencional.

A saída de Java para esse impasse é uma sintaxe específica — o nome do tipo de destino entre parênteses, imediatamente antes do valor a ser convertido — que funciona como uma declaração explícita: "eu, programador, autorizo essa conversão, mesmo sabendo que ela pode perder informação".

```java
double media = 87.9;
int mediaArredondada = (int) media; // casting: double para int
System.out.println(mediaArredondada); // imprime 87, não 88
```

Repare, nesse exemplo, que o casting de `double` para `int` não arredonda o valor para o inteiro mais próximo — ele simplesmente descarta a parte decimal (trunca o valor), independentemente de qual seria o arredondamento matematicamente mais correto. Esse comportamento de truncamento, e não arredondamento, é uma fonte comum de confusão para quem está começando, e vale ser memorizado: `(int) 87.9` resulta em `87`, não em `88`.

O casting também é necessário em conversões entre tipos inteiros de tamanhos diferentes quando a direção é do maior para o menor — por exemplo, de `long` para `int`, ou de `int` para `short` — casos em que, se o valor de origem for maior do que a faixa do tipo de destino comporta, o resultado após o casting pode ser um valor completamente diferente e inesperado, um comportamento relacionado ao overflow discutido no último conceito deste capítulo. Por isso, o uso do casting deve ser sempre uma decisão deliberada, feita com a certeza (ou pelo menos a expectativa razoável) de que o valor de origem cabe corretamente no tipo de destino, e não uma forma automática de "silenciar" um erro de compilação sem entender suas implicações.

### Promoção numérica

Promoção numérica é a conversão automática que Java aplica aos operandos de uma operação aritmética (como soma, subtração, multiplicação) quando eles não são do mesmo tipo, ou quando são de tipos menores que `int`, garantindo que a operação aconteça de forma consistente e sem perda desnecessária de informação durante o próprio cálculo. É um mecanismo relacionado às conversões implícitas já vistas, mas com um propósito específico: unificar os tipos dos operandos antes de uma operação, não apenas ao atribuir um valor a uma variável.

Somar, por exemplo, um `byte` com um `short`, ou um `int` com um `double`, exigiria conversões manuais explícitas antes de cada operação envolvendo tipos diferentes, se esse mecanismo não existisse — mesmo em cálculos triviais e extremamente comuns no dia a dia de qualquer programa. Isso tornaria expressões aritméticas simples desnecessariamente verbosas e sujeitas a erro.

Duas regras específicas de promoção evitam esse retrabalho. A primeira: qualquer operando do tipo `byte`, `short` ou `char` envolvido em uma operação aritmética é automaticamente promovido para `int`, mesmo que o outro operando também seja pequeno — por isso, somar dois valores `byte` em Java já produz, como resultado, um valor do tipo `int`, não `byte`, um detalhe que surpreende bastante gente ao começar a programar na linguagem. A segunda regra: quando os dois operandos de uma operação são de tipos numéricos diferentes (por exemplo, `int` e `double`), o operando de tipo "menor" é promovido para o tipo "maior" antes da operação acontecer, seguindo a mesma ordem de abrangência mencionada no conceito sobre conversões implícitas.

Um exemplo que ilustra a primeira regra, e que costuma surpreender iniciantes:

```java
byte a = 10;
byte b = 20;
// byte soma = a + b; // erro de compilação!
int soma = a + b; // correto: o resultado de "byte + byte" já é int
```

A linha comentada geraria erro de compilação, mesmo que `10 + 20` caiba perfeitamente em um `byte` (o resultado, 30, está dentro da faixa de -128 a 127): o compilador não avalia o valor específico do resultado antes de decidir o tipo — ele aplica a regra de promoção de forma mecânica, sempre elevando `byte` e `short` para `int` em operações aritméticas, independentemente do resultado real daquela operação específica.

Um exemplo da segunda regra, envolvendo tipos diferentes:

```java
int quantidade = 3;
double preco = 19.99;
double total = quantidade * preco; // int promovido para double antes da multiplicação
```

Entender a promoção numérica evita erros de compilação inesperados (como no primeiro exemplo) e também ajuda a prever corretamente o tipo do resultado de uma expressão aritmética qualquer, um conhecimento que se conecta diretamente ao capítulo seguinte, sobre operadores, e ao capítulo final deste módulo, sobre expressões.

### Overflow

Overflow é o que acontece quando o resultado de uma operação aritmética ultrapassa a faixa máxima (ou mínima) de valores que o tipo envolvido consegue representar. Diferente do que se poderia esperar, Java não interrompe a execução do programa nem lança um erro automaticamente quando isso acontece com tipos primitivos inteiros — o valor simplesmente "dá a volta", recomeçando do outro extremo da faixa daquele tipo, um comportamento silencioso que pode passar despercebido se o programador não estiver atento a essa possibilidade.

Sem entender overflow, um programador pode escrever um código aparentemente correto — como um contador incrementado repetidamente dentro de um laço muito longo, ou uma multiplicação entre dois números grandes — e obter, sem qualquer aviso do compilador ou da JVM, um resultado completamente errado e sem sentido aparente, difícil de depurar justamente porque nenhum erro explícito é lançado no momento em que o problema ocorre.

O comportamento de overflow em Java segue a aritmética modular: para um `int`, por exemplo, se um cálculo ultrapassar o valor máximo (2.147.483.647), o resultado "volta" para o valor mínimo (-2.147.483.648) e continua contando a partir dali, como se a faixa de valores fosse um relógio circular, e não uma linha reta que termina.

Um exemplo direto:

```java
int maximo = Integer.MAX_VALUE; // 2.147.483.647
int resultado = maximo + 1;
System.out.println(resultado); // imprime -2147483648
```

`Integer.MAX_VALUE` é uma constante da própria biblioteca padrão de Java, que guarda exatamente o maior valor possível para um `int` — um recurso útil justamente para checar, no próprio código, se um cálculo está se aproximando perigosamente desse limite. Somar `1` a esse valor máximo não gera erro algum: o resultado simplesmente "dá a volta" para o menor valor possível de um `int`, um comportamento que, sem o conhecimento deste conceito, pareceria um bug incompreensível.

A forma mais comum de evitar overflow, ao lidar com valores que podem crescer além do esperado, é escolher desde o início um tipo com faixa suficientemente ampla — `long` em vez de `int`, por exemplo, quando há qualquer possibilidade razoável de os valores envolvidos crescerem além da faixa de um `int`, como discutido no conceito sobre o tipo `long` anteriormente neste módulo. Para cenários onde nem `long` seria suficiente (cálculos matemáticos com números extremamente grandes, como em criptografia), Java oferece a classe `BigInteger`, que, assim como `BigDecimal` mencionado no conceito sobre `double`, não é um tipo primitivo e está fora do escopo deste módulo introdutório, mas vale saber que existe como recurso para os casos mais extremos.

## Operadores

Com variáveis e tipos já estabelecidos, este capítulo apresenta as ferramentas que efetivamente fazem algo com esses valores: os operadores. Um operador é um símbolo que representa uma operação a ser executada sobre um ou mais valores (chamados de operandos), como somar dois números, comparar se um é maior que o outro, ou combinar duas condições verdadeiro/falso.

O capítulo organiza os operadores por categoria: aritméticos (soma, subtração, multiplicação, divisão e resto), relacionais (comparações de maior, menor, etc.), de igualdade, lógicos (para combinar condições booleanas) e de atribuição (incluindo as formas compostas, como somar e atribuir em um único passo). Cada categoria resolve um tipo diferente de necessidade dentro de um programa, e conhecer todas elas é o que torna possível, no capítulo seguinte, montar expressões mais elaboradas combinando vários operadores em uma única linha.

### Aritméticos

Operadores aritméticos são os símbolos usados para realizar cálculos matemáticos básicos entre valores numéricos em Java: `+` (soma), `-` (subtração), `*` (multiplicação), `/` (divisão) e `%` (resto da divisão, também chamado de operador módulo). Eles são a base de qualquer cálculo dentro de um programa, desde os mais simples, como somar dois números, até expressões mais elaboradas combinando vários deles.

Sem operadores aritméticos nativos na linguagem, qualquer cálculo — mesmo uma soma simples — precisaria ser implementado manualmente através de outra abordagem, algo impensável para uma linguagem de propósito geral. Java, como praticamente qualquer linguagem de programação, oferece esses operadores diretamente na sintaxe, com um comportamento próximo ao da matemática tradicional, mas com nuances específicas que vale conhecer.

A maioria dos operadores aritméticos se comporta de forma previsível, mas o operador `/` (divisão) merece atenção especial: quando os dois operandos envolvidos são de tipos inteiros (como `int` ou `long`), a divisão em Java descarta qualquer parte decimal do resultado, retornando apenas a parte inteira — um comportamento diferente do que se poderia esperar de uma divisão matemática comum.

```java
int resultado = 7 / 2;
System.out.println(resultado); // imprime 3, não 3.5
```

Para obter o resultado decimal completo de uma divisão, pelo menos um dos operandos precisa ser de um tipo de ponto flutuante (`float` ou `double`), o que aciona a promoção numérica discutida no capítulo anterior:

```java
double resultado = 7.0 / 2;
System.out.println(resultado); // imprime 3.5
```

O operador `%` (módulo) devolve o resto de uma divisão inteira, não o quociente — `7 % 2` resulta em `1`, já que 7 dividido por 2 dá quociente 3 e resto 1. Esse operador é extremamente útil em situações como verificar se um número é par ou ímpar (`numero % 2 == 0` indica número par), ou distribuir itens em ciclos repetidos, um padrão que reaparecerá com frequência em módulos futuros sobre laços de repetição.

Um cuidado importante, relacionado ao operador de divisão: dividir um número inteiro por zero (`10 / 0`, com ambos operandos inteiros) não retorna um valor especial — lança uma exceção em tempo de execução (`ArithmeticException`), interrompendo o programa se não for tratada, um tópico que será revisitado no módulo sobre tratamento de erros. Já a divisão de ponto flutuante por zero (`10.0 / 0`) não lança exceção, mas produz valores especiais como `Infinity` ou `NaN` ("Not a Number"), seguindo o padrão IEEE 754 já mencionado no capítulo anterior sobre `float` e `double`.

### Relacionais

Operadores relacionais são usados para comparar dois valores entre si, produzindo sempre um resultado do tipo `boolean` (`true` ou `false`) como resposta à comparação. Java oferece quatro operadores relacionais de ordem: `>` (maior que), `<` (menor que), `>=` (maior ou igual a) e `<=` (menor ou igual a), todos aplicáveis a tipos numéricos (e, indiretamente, a `char`, já que ele é internamente representado como número, como visto no capítulo anterior).

Sem operadores relacionais, não haveria como um programa comparar dois valores numéricos e tomar decisões baseadas nessa comparação — e comparar valores é a base de praticamente toda lógica condicional em qualquer programa, desde verificar se uma idade é suficiente para uma ação específica, até ordenar uma lista de valores. Esses operadores resolvem exatamente essa necessidade, produzindo um `boolean` que pode, então, ser usado diretamente em estruturas de decisão (o módulo de estruturas condicionais, mais adiante no livro, vai construir diretamente sobre esse resultado).

Um exemplo prático, já conectando com o tipo `boolean` estudado no capítulo anterior:

```java
int idade = 20;
boolean maiorDeIdade = idade >= 18;
System.out.println(maiorDeIdade); // imprime true
```

Cada operador relacional avalia a relação entre os dois operandos de forma direta: `idade >= 18` é lido como "idade é maior ou igual a 18", e o resultado dessa avaliação, `true` ou `false`, é o que fica armazenado na variável `maiorDeIdade`.

Um erro comum entre iniciantes vindos de outras linguagens é confundir os operadores relacionais de ordem com o operador de igualdade — `>=` e `<=` comparam ordem (um valor é maior/menor ou igual a outro), enquanto a comparação de igualdade estrita usa um operador diferente, `==`, tratado no próximo conceito. Essa distinção é importante porque `=` (um único sinal de igual) já tem um significado totalmente diferente em Java — é o operador de atribuição, estudado no primeiro capítulo deste módulo —, e confundir os dois é uma fonte frequente de erros de compilação (ou, em certos casos mais raros, de bugs lógicos) para quem está começando a programar.

Os operadores relacionais funcionam de forma consistente com a promoção numérica já discutida: é possível comparar diretamente um `int` com um `double`, por exemplo, já que o operando de tipo menor é promovido automaticamente antes da comparação acontecer, seguindo exatamente a mesma lógica já vista nas operações aritméticas.

### Igualdade

Operadores de igualdade são usados para verificar se dois valores são exatamente iguais (`==`) ou diferentes (`!=`) entre si, também produzindo um resultado `boolean`. Eles formam, junto com os operadores relacionais do conceito anterior, o conjunto completo de comparações que Java oferece nativamente entre valores.

A distinção entre `==` (comparação de igualdade) e `=` (atribuição, já visto no primeiro capítulo deste módulo) é um dos pontos mais importantes a fixar bem neste estágio do aprendizado: usar `=` onde se pretendia usar `==` é um erro de lógica clássico em muitas linguagens de programação, embora em Java, felizmente, esse erro específico costuma ser pego pelo próprio compilador na maioria dos casos, já que uma atribuição dentro de uma condição (assunto de módulos futuros) normalmente não resulta em um valor `boolean` compatível, gerando erro de compilação em vez de um bug silencioso.

Um exemplo direto, aplicado a tipos primitivos:

```java
int idade = 25;
boolean ehVinteECinco = idade == 25;
boolean naoEhTrinta = idade != 30;
System.out.println(ehVinteECinco); // imprime true
System.out.println(naoEhTrinta);   // imprime true
```

Para tipos primitivos, `==` e `!=` comparam diretamente os valores numéricos envolvidos, funcionando exatamente como seria de se esperar: `idade == 25` é `true` porque o valor guardado em `idade` é, de fato, 25. Esse comportamento simples e direto vale para todos os tipos primitivos vistos neste módulo — números inteiros, números de ponto flutuante, `char` e `boolean`.

É importante, no entanto, já registrar uma ressalva que será aprofundada em módulos futuros, quando forem tratadas classes e objetos: `==` se comporta de forma diferente ao comparar objetos (como duas `String`, por exemplo) em vez de tipos primitivos — nesse caso, `==` compara se as duas variáveis apontam para o mesmo objeto na memória, não necessariamente se os conteúdos são "iguais" no sentido intuitivo. Essa distinção, embora relevante, foge do escopo deste módulo, focado em tipos primitivos; por ora, basta saber que os exemplos e explicações aqui apresentados valem plenamente para comparações entre valores primitivos, o uso mais comum e mais simples desses operadores.

Comparar valores de ponto flutuante (`float`, `double`) com `==` merece um cuidado adicional: como discutido no capítulo anterior, cálculos com esses tipos podem sofrer pequenas imprecisões de arredondamento, de forma que dois valores matematicamente equivalentes, mas calculados por caminhos diferentes, podem não ser considerados exatamente iguais por `==`. Por essa razão, comparar valores `double` ou `float` por igualdade exata é, em geral, desaconselhado; a prática recomendada, em cálculos que envolvem ponto flutuante, é verificar se a diferença entre os dois valores é menor que uma margem de tolerância aceitável, em vez de exigir igualdade perfeita.

### Lógicos

Operadores lógicos combinam dois ou mais valores `boolean` (ou expressões que resultam em `boolean`) em uma única expressão lógica, também resultando em um `boolean`. Java oferece três operadores lógicos principais: `&&` (E lógico, "and"), `||` (OU lógico, "or") e `!` (negação, "not"), cada um representando uma operação lógica clássica, presente também na lógica matemática formal.

Sem operadores lógicos, seria impossível expressar condições compostas — situações do tipo "só faça isso se A e B forem verdadeiros ao mesmo tempo", ou "faça isso se A ou B for verdadeiro" — obrigando o programador a decompor manualmente qualquer lógica de decisão mais elaborada em vários passos separados, tornando o código mais longo e menos direto de ler.

O operador `&&` resulta em `true` apenas quando ambos os operandos são `true`; `||` resulta em `true` quando pelo menos um dos operandos é `true`; e `!` inverte o valor de um único operando `boolean`, transformando `true` em `false` e vice-versa.

```java
int idade = 20;
boolean temCarteira = true;
boolean podeDirigir = idade >= 18 && temCarteira;
System.out.println(podeDirigir); // imprime true

boolean feriadoOuFimDeSemana = false || true;
System.out.println(feriadoOuFimDeSemana); // imprime true

boolean naoEstaAtivo = !true;
System.out.println(naoEstaAtivo); // imprime false
```

Um comportamento importante de `&&` e `||` em Java é a chamada avaliação de curto-circuito (short-circuit evaluation): em uma expressão `a && b`, se `a` já for `false`, Java nem chega a avaliar `b`, já que o resultado da expressão inteira já está determinado como `false`, independentemente do valor de `b`. Da mesma forma, em `a || b`, se `a` já for `true`, `b` não é avaliado, pois o resultado já é `true`. Esse comportamento não é apenas uma otimização de desempenho — ele é frequentemente usado deliberadamente para evitar erros, como em `lista != null && lista.tamanho() > 0`, onde `lista.tamanho()` só é chamado se `lista` já tiver sido confirmada como não nula pela primeira parte da expressão, evitando um erro que aconteceria ao tentar usar uma referência nula (conceito que será formalizado em módulos futuros sobre objetos).

Java também oferece versões sem curto-circuito desses operadores, `&` e `|` (um único símbolo, em vez de dois), que sempre avaliam ambos os operandos, mesmo quando o resultado já poderia ser determinado apenas pelo primeiro. Seu uso é bem menos comum no dia a dia — geralmente reservado a contextos específicos onde os efeitos colaterais de avaliar ambos os lados são deliberadamente desejados —, e para a grande maioria das situações práticas, `&&` e `||`, com curto-circuito, são a escolha padrão e recomendada.

### Atribuição

Operadores de atribuição vão além do simples `=` (atribuição básica, já estudado no primeiro capítulo deste módulo): Java oferece um conjunto de operadores de atribuição compostos, que combinam uma operação aritmética (ou de outro tipo) com uma atribuição em um único símbolo, tornando expressões comuns mais curtas de escrever. Os mais usados são `+=`, `-=`, `*=`, `/=` e `%=`, cada um correspondente a um dos operadores aritméticos vistos no primeiro conceito deste capítulo.

Sem operadores de atribuição compostos, uma operação extremamente comum — atualizar o valor de uma variável com base em seu próprio valor atual, como incrementar um total acumulado — exigiria sempre repetir o nome da variável duas vezes na mesma linha, uma como referência ao valor atual e outra como destino do novo valor:

```java
int total = 100;
total = total + 50; // forma longa
```

Os operadores de atribuição compostos resolvem isso condensando essa repetição em um único símbolo:

```java
int total = 100;
total += 50; // equivalente a "total = total + 50"
System.out.println(total); // imprime 150
```

Cada operador composto segue o mesmo padrão: `total -= 20` equivale a `total = total - 20`; `total *= 2` equivale a `total = total * 2`; `total /= 4` equivale a `total = total / 4`; e `total %= 3` equivale a `total = total % 3`. Além de mais compactos, esses operadores tornam a intenção do código mais clara à primeira leitura: `total += 50` comunica diretamente "incremente total em 50", sem exigir que quem lê o código confira se o nome da variável nos dois lados do `=` é realmente o mesmo.

Um detalhe técnico relevante: os operadores de atribuição compostos realizam, internamente, um casting implícito de volta para o tipo original da variável, mesmo quando a operação intermediária, isoladamente, geraria um tipo diferente (por conta da promoção numérica vista no capítulo anterior). Por exemplo:

```java
byte contador = 10;
contador += 5; // funciona, mesmo "contador + 5" sendo tecnicamente int
// contador = contador + 5; // isso, por outro lado, exigiria casting explícito!
```

Essa diferença é sutil, mas importante: a forma longa, `contador = contador + 5`, geraria erro de compilação sem um casting explícito, já que `contador + 5` é promovido para `int`, e atribuir um `int` diretamente a uma variável `byte` não é uma conversão implícita válida, como discutido no capítulo anterior. Já a forma composta, `contador += 5`, já inclui esse casting de volta automaticamente como parte de sua própria definição na linguagem, tornando-a não apenas mais curta, mas também mais conveniente nesses casos específicos envolvendo tipos menores que `int`.

## Expressões

Os operadores apresentados no capítulo anterior raramente aparecem sozinhos: no código real, é comum combinar vários deles em uma única linha, formando o que se chama de expressão — uma combinação de valores, variáveis e operadores que, ao ser avaliada, produz um resultado. Este capítulo fecha o módulo explicando as regras que determinam como o Java avalia essas combinações quando há mais de um operador envolvido.

O capítulo cobre precedência (qual operador é avaliado primeiro quando há vários em uma expressão, como multiplicação sendo calculada antes de soma), associatividade (a ordem de avaliação entre operadores de mesma precedência), e os operadores de incremento e decremento, que têm um comportamento particular dependendo de aparecerem antes ou depois da variável. O capítulo termina mostrando expressões compostas, juntando tudo que foi visto no módulo em exemplos mais próximos do código do dia a dia. Sem entender essas regras, é fácil escrever uma expressão que calcula um resultado diferente do esperado, mesmo usando os operadores corretos.

### Precedência

Precedência de operadores é a regra que determina qual operação, dentro de uma expressão com múltiplos operadores, é avaliada primeiro. Assim como na matemática tradicional (onde multiplicação e divisão são resolvidas antes de soma e subtração), Java segue uma ordem bem definida de precedência entre seus diversos operadores, garantindo que uma expressão com vários símbolos diferentes tenha sempre um único resultado previsível, e não uma ambiguidade que dependeria da interpretação de quem lê o código.

Sem regras de precedência bem definidas, uma expressão como `2 + 3 * 4` poderia ser interpretada de duas formas diferentes — primeiro somando `2 + 3` (resultando em `20`) ou primeiro multiplicando `3 * 4` (resultando em `14`) —, e essa ambiguidade tornaria impossível prever, com certeza, o comportamento de qualquer expressão minimamente composta. A precedência resolve isso estabelecendo uma hierarquia fixa: operadores multiplicativos (`*`, `/`, `%`) têm precedência mais alta que operadores aditivos (`+`, `-`), então, na expressão `2 + 3 * 4`, a multiplicação é resolvida primeiro, resultando em `14`.

```java
int resultado = 2 + 3 * 4;
System.out.println(resultado); // imprime 14, não 20
```

De forma geral, a ordem de precedência em Java (da mais alta para a mais baixa, entre os operadores já vistos neste módulo) é aproximadamente: operadores unários (como `!` e o `-` de sinal negativo) primeiro, depois multiplicativos (`*`, `/`, `%`), depois aditivos (`+`, `-`), depois relacionais (`<`, `>`, `<=`, `>=`), depois de igualdade (`==`, `!=`), depois o `&&`, depois o `||`, e por último os operadores de atribuição (`=`, `+=`, e os demais), que têm a precedência mais baixa de todos.

Quando a ordem natural de precedência não corresponde ao que se deseja calcular, é possível (e recomendado) usar parênteses para forçar explicitamente uma ordem diferente, já que expressões entre parênteses são sempre avaliadas antes de qualquer coisa ao seu redor — a mesma regra usada na matemática tradicional:

```java
int resultado = (2 + 3) * 4;
System.out.println(resultado); // imprime 20
```

Mesmo quando a precedência padrão já produziria o resultado desejado, é uma prática comum e recomendada usar parênteses para deixar a intenção explícita em expressões mais complexas, envolvendo vários operadores diferentes — não por necessidade estrita do compilador, mas para tornar o código mais fácil de ler rapidamente por outra pessoa (ou por você mesmo, meses depois), sem exigir que quem lê memorize a tabela completa de precedência de Java para entender corretamente cada expressão.

### Associatividade

Associatividade é a regra que determina a ordem de avaliação entre operadores de mesma precedência, quando eles aparecem lado a lado em uma mesma expressão. Enquanto a precedência (conceito anterior) resolve a ordem entre operadores diferentes, a associatividade resolve o caso em que dois ou mais operadores de mesmo nível de precedência aparecem juntos, e ainda assim é preciso decidir qual deles "age" primeiro.

Sem regras de associatividade, uma expressão como `20 - 5 - 3`, que envolve dois operadores de mesma precedência (`-`), ainda seria ambígua: calculada da esquerda para a direita, o resultado seria `(20 - 5) - 3 = 12`; calculada da direita para a esquerda, seria `20 - (5 - 3) = 18` — dois resultados bem diferentes para a mesma expressão escrita. A associatividade resolve essa ambiguidade remanescente, definindo, para cada operador, uma direção fixa de avaliação.

A maioria dos operadores binários em Java — incluindo os aritméticos (`+`, `-`, `*`, `/`, `%`), os relacionais e os de igualdade — tem associatividade da esquerda para a direita: entre operadores de mesma precedência, o que aparece mais à esquerda é avaliado primeiro.

```java
int resultado = 20 - 5 - 3;
System.out.println(resultado); // imprime 12: (20 - 5) - 3
```

Já os operadores de atribuição (`=`, `+=`, e os demais vistos no capítulo anterior) têm associatividade da direita para a esquerda — um detalhe que se torna visível ao encadear várias atribuições na mesma linha, um recurso pouco comum no dia a dia, mas que ilustra bem a regra:

```java
int a, b, c;
a = b = c = 10; // associatividade direita-para-esquerda: c=10, depois b=c, depois a=b
System.out.println(a + " " + b + " " + c); // imprime 10 10 10
```

Nesse exemplo, a atribuição mais à direita (`c = 10`) é resolvida primeiro, e o resultado dessa atribuição (o próprio valor `10`) é então usado para a atribuição seguinte à esquerda (`b = c`), e assim sucessivamente, até `a`. Isso só faz sentido porque a associatividade de `=` é da direita para a esquerda; se fosse da esquerda para a direita, como a maioria dos outros operadores, essa cadeia de atribuições sequenciais nem seria possível da mesma forma.

Assim como a precedência, associatividade é uma regra que o compilador segue de forma totalmente mecânica e previsível — não há ambiguidade real na linguagem, apenas a necessidade de o programador conhecer essas regras para prever corretamente o resultado de expressões mais compostas, e usar parênteses sempre que preferir deixar essa ordem explícita, em vez de depender da memorização exata dessas regras por quem for ler o código depois.

### Incremento

O operador de incremento (`++`) é um atalho sintático para aumentar o valor de uma variável numérica em exatamente uma unidade, substituindo a forma mais longa `variavel = variavel + 1` (ou sua versão composta, `variavel += 1`) por uma notação ainda mais compacta. É um dos operadores mais usados em Java, especialmente em laços de repetição, tema de um módulo futuro deste livro.

Sem um operador dedicado a essa operação tão comum — aumentar uma variável em 1 —, o código precisaria repetir sempre a forma mais longa de atribuição, mesmo sendo essa uma das operações mais frequentes em qualquer programa que precise contar, iterar ou acumular repetições. O operador `++` resolve isso com uma notação mínima, de apenas dois caracteres.

Uma particularidade importante do `++` em Java é que ele existe em duas formas, com comportamentos diferentes dependendo da posição em relação à variável: pré-incremento (`++variavel`, o operador antes da variável) e pós-incremento (`variavel++`, o operador depois da variável). Ambas as formas incrementam a variável em 1, mas diferem no valor que a própria expressão produz quando usada dentro de uma expressão maior: o pré-incremento produz o valor já incrementado; o pós-incremento produz o valor original, antes do incremento, e só depois disso aplica o incremento à variável.

```java
int a = 5;
int b = ++a; // pré-incremento: a vira 6, e b recebe 6
System.out.println(a + " " + b); // imprime 6 6

int x = 5;
int y = x++; // pós-incremento: y recebe 5 (valor original), e só depois x vira 6
System.out.println(x + " " + y); // imprime 6 5
```

Esse comportamento diferenciado entre as duas formas é uma fonte comum de confusão para quem está começando, especialmente quando o `++` é usado dentro de uma expressão maior, e não isoladamente em sua própria linha. Quando o operador é usado sozinho, em sua própria instrução (como `contador++;`, sem atribuir o resultado a nada), a diferença entre pré e pós-incremento é irrelevante — ambas as formas produzem exatamente o mesmo efeito final sobre a variável, apenas o valor "devolvido" pela expressão em si é que muda, e esse valor só importa quando é efetivamente usado em outro lugar, como em uma atribuição.

Por essa razão, uma boa prática para quem está aprendendo é evitar, inicialmente, misturar `++` dentro de expressões mais complexas junto com outros operadores, e usá-lo isoladamente, em sua própria linha, até se sentir confortável com a diferença entre as duas formas — um cuidado que reduz bastante a chance de introduzir bugs sutis relacionados à ordem exata de avaliação.

### Decremento

O operador de decremento (`--`) é o equivalente ao operador de incremento do conceito anterior, mas na direção oposta: reduz o valor de uma variável numérica em exatamente uma unidade, funcionando como um atalho para `variavel = variavel - 1` (ou `variavel -= 1`). Assim como o incremento, é amplamente usado em laços de repetição, especialmente naqueles que contam de forma decrescente, e segue exatamente as mesmas regras de posicionamento e comportamento do `++`, já detalhadas no conceito anterior.

A motivação para sua existência é a mesma já discutida para o `++`: evitar repetir, a cada redução, a forma mais longa de atribuição para uma operação tão frequente em programas que contam ou percorrem sequências — só que aqui aplicada à direção inversa, subtraindo em vez de somar.

Como já visto no incremento, o `--` também existe em duas formas com comportamentos distintos: pré-decremento (`--variavel`), que produz, como valor da própria expressão, o resultado já decrementado; e pós-decremento (`variavel--`), que produz o valor original, aplicando a redução à variável só depois.

```java
int a = 5;
int b = --a; // pré-decremento: a vira 4, e b recebe 4
System.out.println(a + " " + b); // imprime 4 4

int x = 5;
int y = x--; // pós-decremento: y recebe 5 (valor original), e só depois x vira 4
System.out.println(x + " " + y); // imprime 4 5
```

Um exemplo de uso comum, mesmo antes do módulo sobre estruturas de repetição, é a ideia de uma contagem regressiva: uma variável que começa em um valor e vai sendo decrementada repetidamente até atingir zero (ou outro valor de parada), um padrão que reaparecerá com muita frequência, formalizado através de laços como `for` e `while` em módulos futuros deste livro.

A mesma recomendação já dada para o incremento vale aqui: evite misturar `--` dentro de expressões maiores enquanto a diferença entre pré e pós-decremento não estiver bem consolidada, preferindo usá-lo isoladamente, em sua própria linha, até ganhar confiança com o padrão — o mesmo cuidado que evita aquele tipo de bug sutil em que o programa roda sem erros, mas produz um resultado ligeiramente diferente do esperado.

### Expressões compostas

Expressão composta é qualquer expressão que combina múltiplos operadores (aritméticos, relacionais, lógicos, ou uma mistura deles) em uma única instrução, exigindo que precedência e associatividade — os dois primeiros conceitos deste capítulo — sejam aplicadas em conjunto para determinar o resultado final. É o tipo de expressão mais próximo do que se encontra na prática real de programação, já que raramente um cálculo útil se resume a um único operador isolado.

Sem entender como combinar corretamente vários operadores em uma mesma expressão, o programador ficaria limitado a instruções extremamente simples, uma operação por vez, dividindo em várias linhas separadas qualquer cálculo que naturalmente envolveria mais de um passo — um estilo de código bem menos comum e mais verboso do que o que se pratica normalmente em Java (e em praticamente qualquer linguagem de programação madura).

Expressões compostas resolvem isso permitindo reunir, em uma única linha, toda a lógica de um cálculo ou de uma decisão, desde que o programador tenha clareza sobre a ordem em que as partes serão avaliadas — daí a importância de já ter estudado precedência e associatividade antes de compor expressões mais elaboradas.

Um exemplo que combina operadores aritméticos, relacionais e lógicos em uma única expressão:

```java
int idade = 20;
double renda = 2500.0;
boolean temHistoricoLimpo = true;

boolean aprovaCredito = idade >= 18 && renda > 2000.0 && temHistoricoLimpo;
System.out.println(aprovaCredito); // imprime true
```

Nessa expressão, três sub-condições diferentes são combinadas com `&&`: a idade mínima, a renda mínima e o histórico limpo. Cada uma delas é avaliada individualmente (seguindo a precedência, que resolve `>=` e `>` antes de `&&`), e o resultado de cada uma é então combinado através da avaliação de curto-circuito discutida no capítulo anterior — se `idade >= 18` já for `false`, Java nem chega a avaliar as duas condições seguintes.

Um exemplo aritmético mais elaborado, misturando parênteses, multiplicação e divisão:

```java
double precoComDesconto = (100.0 - (100.0 * 0.15)) / 2;
System.out.println(precoComDesconto); // imprime 42.5
```

Ao escrever expressões compostas como essas, a recomendação prática mais importante — já mencionada no primeiro conceito deste capítulo — é usar parênteses de forma generosa sempre que a ordem de avaliação não for absolutamente óbvia à primeira leitura, mesmo quando a precedência padrão do Java já produziria o resultado correto sem eles. Expressões compostas muito longas e sem nenhum parêntese explícito, mesmo que tecnicamente corretas, tendem a ser mais difíceis de revisar e mais propensas a esconder um erro de lógica que passaria despercebido — um cuidado que se torna ainda mais relevante à medida que os programas deste livro forem crescendo em complexidade nos módulos seguintes.

# Módulo 3 — Métodos

Até aqui, o livro tratou de dados isolados — variáveis, tipos, operadores e expressões — mas todo o código escrito ficava dentro de um único bloco, sem organização em partes reutilizáveis. Este módulo introduz os métodos, o principal mecanismo que Java oferece para dividir um programa em blocos nomeados e reutilizáveis de código, cada um responsável por uma tarefa específica. É um passo essencial rumo à organização de programas maiores, que será aprofundada mais adiante com classes e objetos.

Os cinco capítulos deste módulo cobrem, em ordem, como declarar um método e entender suas partes constituintes, como ele recebe dados de fora através de parâmetros e argumentos, como ele devolve um resultado através do `return`, como um mesmo nome de método pode ter várias formas através de sobrecarga e varargs, e por fim como funcionam escopo (onde uma variável é visível) e recursividade (um método que chama a si mesmo). Ao final deste módulo, o leitor será capaz de estruturar um programa em métodos coesos e reutilizáveis, entendendo exatamente como dados entram, circulam e saem de cada um deles.

## Criando métodos

Até este ponto do livro, todo código de exemplo viveu dentro de um único método `main`, o que funciona para programas pequenos, mas rapidamente se torna difícil de ler e manter conforme um programa cresce. Este capítulo introduz o conceito central do módulo: o método, um bloco de código nomeado que agrupa uma sequência de instruções para realizar uma tarefa específica e pode ser chamado (invocado) de outros pontos do programa sempre que essa tarefa for necessária.

O capítulo apresenta como declarar um método, o que compõe sua assinatura (nome, parâmetros e tipo de retorno), como seu corpo é definido, como ele é efetivamente invocado a partir de outro trecho de código, e por que dividir um programa em métodos favorece a separação de responsabilidades — cada método cuidando de uma única tarefa bem definida, em vez de um bloco único fazendo tudo. Esse princípio de organização é a motivação que percorre todo o restante do módulo.

### Declaração

Declarar um método é o ato de definir, dentro de uma classe, um bloco de código nomeado que pode ser executado sempre que necessário, em vez de escrito repetidamente em todos os lugares onde aquela lógica é usada. Uma declaração de método reúne, em uma única estrutura, tudo o que é preciso saber sobre ele antes mesmo de olhar para dentro: seu nome, o que ele recebe, o que ele devolve e o código que ele executa.

Antes de existir a possibilidade de declarar métodos próprios, todo o código de um programa ficaria concentrado em um único bloco, como o `main` visto nos primeiros programas deste livro. Qualquer lógica que precisasse se repetir — validar uma idade, calcular uma média, formatar um texto — teria que ser copiada e colada em cada ponto do programa onde fosse necessária, tornando o código maior, mais difícil de revisar e extremamente propenso a erros: uma correção feita em uma cópia da lógica poderia facilmente ser esquecida em outra.

A declaração de métodos resolve isso permitindo isolar um trecho de lógica sob um nome único, que pode então ser chamado (invocado, tema de um conceito adiante) quantas vezes forem necessárias, de qualquer lugar dentro da classe. Isso separa a definição do comportamento (o que o método faz, escrito uma única vez) do seu uso (onde e quando esse comportamento é acionado).

Um exemplo simples de declaração:

```java
public static int somar(int a, int b) {
    int resultado = a + b;
    return resultado;
}
```

Aqui, `somar` é declarado uma única vez, mas pode ser usado dezenas de vezes ao longo do programa, sempre produzindo o mesmo comportamento consistente para quaisquer valores de `a` e `b` que sejam fornecidos.

Uma boa analogia é pensar em um método como a receita de um prato em um livro de culinária: a receita é escrita uma única vez, com um nome (o título do prato) e uma sequência de passos, e pode ser seguida (invocada) sempre que alguém quiser preparar aquele prato, sem precisar reescrever os passos a cada vez. Assim como a receita, a declaração do método concentra o "como fazer" em um único lugar, deixando quem for usá-lo livre para simplesmente pedir que ele seja executado.

Vale notar que declarar um método não o executa imediatamente: a declaração apenas ensina ao compilador que aquele método existe e como ele deve se comportar quando chamado. A execução de fato só acontece quando o método é invocado — algo detalhado em um conceito mais adiante neste mesmo capítulo.

### Assinatura

A assinatura de um método é o conjunto formado pelo seu nome e pela lista de tipos dos parâmetros que ele recebe, na ordem em que aparecem — é, em outras palavras, a "identidade" do método do ponto de vista do compilador, o que permite distinguir métodos diferentes mesmo quando compartilham o mesmo nome (situação explorada mais a fundo no capítulo sobre sobrecarga).

Sem um conceito claro de assinatura, o compilador não teria como decidir, diante de uma chamada de método, exatamente qual código deveria ser executado quando existisse mais de uma declaração com o mesmo nome na classe — ficaria ambíguo se `calcular(int, int)` e `calcular(double, double)` seriam a mesma coisa ou métodos completamente distintos.

A assinatura resolve isso definindo que dois métodos são considerados diferentes sempre que diferem no nome, na quantidade de parâmetros ou nos tipos desses parâmetros — mesmo que o tipo de retorno seja igual, e mesmo que os nomes dos parâmetros sejam diferentes (nomes de parâmetros não fazem parte da assinatura, apenas seus tipos e ordem).

```java
public static int calcular(int a, int b) { return a + b; }
public static double calcular(double a, double b) { return a + b; }
```

Nesse par de declarações, ambas se chamam `calcular`, mas têm assinaturas diferentes — `calcular(int, int)` e `calcular(double, double)` — porque os tipos dos parâmetros diferem. O compilador usa exatamente essa assinatura para decidir, em cada chamada, qual das duas versões deve ser executada, com base nos tipos dos valores fornecidos.

Uma forma útil de visualizar a assinatura é como o "endereço" de um método dentro da classe: assim como duas casas na mesma rua precisam de números diferentes para serem distinguidas pelo carteiro, dois métodos com o mesmo nome numa classe precisam de assinaturas diferentes para que o compilador saiba, sem ambiguidade, para qual deles uma chamada deve ser direcionada.

É importante não confundir assinatura com a declaração completa do método: o tipo de retorno, os modificadores (como `public` ou `static`) e o corpo do método não fazem parte da assinatura — apenas nome, quantidade, tipo e ordem dos parâmetros.

### Corpo

O corpo de um método é o bloco de código, delimitado por chaves `{ }`, que contém as instruções efetivamente executadas toda vez que o método é invocado. É onde a lógica prometida pelo nome do método realmente acontece — a declaração e a assinatura descrevem "o que" o método é, mas é o corpo que descreve "como" ele faz aquilo que promete fazer.

Imagine uma declaração e uma assinatura sem corpo: seria apenas uma promessa vazia — um nome que existe no papel, mas incapaz de realizar qualquer tarefa real quando chamado. É justamente o corpo que transforma essa promessa em algo funcional.

É esse bloco entre chaves que hospeda qualquer sequência válida de instruções Java: declarações de variáveis locais, chamadas a outros métodos, operações aritméticas, estruturas de decisão e repetição (vistas em módulos seguintes) e, quando aplicável, uma instrução `return` para devolver um resultado.

```java
public static int quadrado(int numero) {
    int resultado = numero * numero; // início do corpo
    return resultado;                // fim do corpo
}
```

Nesse exemplo, tudo o que está entre as chaves — a declaração da variável `resultado` e o `return` — compõe o corpo do método `quadrado`. Cada vez que esse método é chamado, é exatamente essa sequência de instruções que é executada, do início ao fim (ou até um eventual `return` antecipado).

Uma analogia simples: se a assinatura de um método é como o título e os ingredientes listados no topo de uma receita, o corpo é a sequência numerada de passos logo abaixo — é ali que o preparo de fato acontece, instrução por instrução, na ordem em que estão escritas.

Vale destacar que o corpo de um método tem seu próprio escopo: variáveis declaradas dentro dele só existem enquanto aquela execução específica do método estiver em andamento, um comportamento detalhado no conceito de escopo local, mais adiante neste módulo. Também é dentro do corpo que a instrução `return` (tema de um capítulo próprio) encerra a execução do método, com ou sem um valor associado, dependendo de o método ser `void` ou não.

### Invocação

Invocar um método é o ato de efetivamente chamá-lo para execução, usando seu nome seguido de parênteses (contendo, se necessário, os argumentos exigidos pela sua assinatura). É a invocação que faz o corpo do método, discutido no conceito anterior, realmente rodar — sem ela, o método declarado permanece apenas como uma definição inerte, nunca executada.

Declarar um método sem poder invocá-lo esvaziaria todo o propósito de isolar uma lógica sob um nome único: de nada adiantaria ter esse nome se não houvesse uma forma simples e repetível de acioná-lo sempre que necessário, a partir de qualquer ponto do programa.

É isso que a invocação viabiliza: "chamar de volta" um método já declarado, quantas vezes forem necessárias, sempre com o mesmo comportamento consistente (dado os mesmos argumentos), passando os valores concretos que preenchem os parâmetros esperados.

```java
public static int dobro(int numero) {
    return numero * 2;
}

public static void main(String[] args) {
    int resultado = dobro(5); // invocação: aciona o corpo de dobro com numero = 5
    System.out.println(resultado); // imprime 10

    System.out.println(dobro(10)); // outra invocação, com outro argumento
}
```

Nesse exemplo, `dobro` é declarado uma única vez, mas invocado duas vezes, com argumentos diferentes (`5` e `10`) — cada invocação executa o mesmo corpo do método, mas produz um resultado próprio, de acordo com o valor fornecido naquela chamada específica.

Uma boa forma de entender a invocação é compará-la a discar um número de telefone: o número (o nome do método) sempre leva à mesma pessoa (o mesmo corpo de código), mas cada ligação (cada invocação) é um evento independente, que pode acontecer quantas vezes forem necessárias, em momentos diferentes, e cada uma delas terá seu próprio conteúdo (os argumentos passados e o resultado obtido).

Quando um método é invocado, a execução do programa "pausa" no ponto da chamada, desvia para o corpo do método invocado, executa-o do início ao fim (ou até um `return`), e só então retorna ao ponto exato de onde a chamada partiu, continuando a execução normalmente a partir dali — um comportamento relacionado ao conceito de pilha de chamadas, explorado com mais profundidade no capítulo sobre escopo e recursividade.

### Separação de responsabilidades

Separação de responsabilidades é o princípio de organizar um programa em métodos distintos, cada um cuidando de uma única tarefa bem definida, em vez de concentrar toda a lógica em um único bloco extenso e monolítico, como um `main` gigante. Não é um recurso da linguagem em si, mas uma prática de organização que os conceitos anteriores deste capítulo — declaração, assinatura, corpo e invocação — tornam possível.

Um programa que ignora essa separação tende a crescer como um único bloco de instruções sequenciais, misturando leitura de dados, cálculos, validações e formatação de saída tudo junto, sem fronteiras claras entre uma etapa e outra. Esse tipo de código costuma ser difícil de ler (é preciso entender o todo para entender qualquer parte), difícil de testar isoladamente e difícil de reaproveitar em outro contexto.

Quebrar o problema maior em pedaços menores resolve isso: cada pedaço vira um método com uma única finalidade clara, expressa já pelo seu nome. Um bom método costuma responder bem à pergunta "o que este método faz?" com uma frase curta e específica — se a resposta exigir "e também", provavelmente esse método está fazendo mais do que deveria.

```java
public static double calcularMedia(double nota1, double nota2) {
    return (nota1 + nota2) / 2;
}

public static boolean aprovado(double media) {
    return media >= 6.0;
}

public static void main(String[] args) {
    double media = calcularMedia(7.5, 8.0);
    if (aprovado(media)) {
        System.out.println("Aluno aprovado.");
    }
}
```

Nesse exemplo, em vez de calcular a média e verificar a aprovação em um único bloco misturado dentro do `main`, cada responsabilidade foi isolada em seu próprio método: `calcularMedia` só calcula, `aprovado` só decide. Isso torna cada parte mais fácil de entender isoladamente, e também mais fácil de reaproveitar — `aprovado` poderia ser chamado de vários lugares diferentes do programa sem repetir a lógica da nota de corte.

Uma analogia útil é a de uma equipe de trabalho bem organizada, em que cada pessoa tem uma função clara — uma cuida da recepção, outra da cozinha, outra do caixa — em vez de todos tentarem fazer um pouco de tudo ao mesmo tempo. Assim como essa divisão de tarefas facilita encontrar quem é responsável por resolver um problema específico, dividir um programa em métodos com responsabilidades bem definidas facilita localizar exatamente onde uma lógica específica está implementada, e corrigi-la sem risco de afetar partes não relacionadas do código.

## Parâmetros e argumentos

Um método que sempre faz exatamente a mesma coisa, sem receber nenhuma informação de fora, tem utilidade limitada. Este capítulo trata de como métodos recebem dados externos para trabalhar com eles: os parâmetros, declarados junto com o método, e os argumentos, os valores concretos passados em cada chamada. É essa capacidade de receber dados variáveis que transforma um método de um bloco fixo de instruções em uma ferramenta genérica, reutilizável em situações diferentes.

O capítulo esclarece a diferença exata entre parâmetro e argumento (um termo comum de se confundir), a tipagem obrigatória de cada parâmetro, e um dos pontos mais importantes — e mais mal compreendidos — de Java: a passagem de parâmetros é sempre por valor, inclusive quando o que está sendo passado é uma referência a um objeto. Entender essa distinção evita um tipo clássico de confusão sobre por que alterar um objeto dentro de um método afeta o objeto original, mas reatribuir a variável dentro do método, não.

### Parâmetro × argumento

Parâmetro e argumento são dois termos frequentemente confundidos por quem está começando, mas que designam coisas diferentes e complementares: parâmetro é a variável declarada na assinatura do método, que descreve o tipo e o nome de um valor que ele espera receber; argumento é o valor concreto, fornecido de fato no momento da invocação, que preenche aquele parâmetro.

Sem essa distinção clara, fica difícil comunicar com precisão sobre o comportamento de um método: dizer apenas "o valor" tanto poderia se referir ao que o método espera receber (o parâmetro, fixo na declaração) quanto ao que foi efetivamente enviado numa chamada específica (o argumento, que muda a cada invocação).

A distinção resolve essa ambiguidade atribuindo um papel a cada momento: o parâmetro existe uma única vez, na declaração do método, como parte de sua assinatura; o argumento existe uma vez para cada invocação, como o valor real usado naquela chamada específica.

```java
public static int somar(int a, int b) { // "a" e "b" são parâmetros
    return a + b;
}

public static void main(String[] args) {
    int resultado = somar(3, 4); // "3" e "4" são argumentos
}
```

Nesse exemplo, `a` e `b` são parâmetros — eles existem apenas uma vez, fixados na declaração de `somar`, e descrevem que o método espera dois valores inteiros. Já `3` e `4` são argumentos — os valores concretos fornecidos nesta invocação específica, que serão atribuídos a `a` e `b` durante a execução daquela chamada.

Uma analogia simples é pensar em um formulário em branco e um formulário preenchido: o formulário em branco, com seus campos nomeados ("Nome:", "Idade:"), é como o parâmetro — a estrutura que descreve o que é esperado. Cada vez que alguém preenche esse formulário com valores reais ("Nome: Ana", "Idade: 30"), esses valores preenchidos são como os argumentos — específicos de cada preenchimento, ainda que a estrutura do formulário (os parâmetros) permaneça sempre a mesma.

Essa distinção se torna especialmente relevante quando se discute passagem por valor (conceito adiante neste mesmo capítulo): é sempre o valor do argumento que é copiado para o parâmetro no momento da chamada — entender exatamente o que é copiado, e quando, depende de já ter claro qual dos dois termos está sendo usado em cada situação.

### Tipagem

Tipagem de parâmetros é a exigência de que cada parâmetro declarado na assinatura de um método tenha um tipo explícito (como `int`, `double`, `String` ou qualquer outro tipo válido em Java), e de que todo argumento fornecido numa invocação seja compatível com esse tipo declarado — reflexo direto da tipagem estática já estudada no módulo anterior deste livro, agora aplicada especificamente aos parâmetros de métodos.

Um método sem essa exigência poderia ser chamado com qualquer tipo de valor, sem garantia nenhuma de que o corpo do método fosse capaz de operar corretamente sobre aquele valor — passar um texto para um método que espera somar números, por exemplo, levaria a um erro percebido apenas durante a execução, e não antecipadamente.

A tipagem de parâmetros resolve isso obrigando o compilador a verificar, em tempo de compilação, se cada argumento fornecido em uma chamada é compatível com o tipo do parâmetro correspondente na assinatura do método — se não for, o programa sequer chega a compilar, evitando que esse tipo de erro alcance a execução.

```java
public static int dobro(int numero) {
    return numero * 2;
}

public static void main(String[] args) {
    System.out.println(dobro(5));      // válido: 5 é int
    // System.out.println(dobro("5")); // erro de compilação: String não é int
}
```

Nesse exemplo, o parâmetro `numero` é declarado como `int`, então qualquer chamada a `dobro` precisa fornecer um argumento compatível com `int` — um valor `int` diretamente, ou algo que possa ser convertido implicitamente (como um `byte` ou `short`, seguindo as mesmas regras de conversão vistas no módulo anterior). Uma `String`, por não ser compatível, gera erro de compilação antes mesmo do programa rodar.

Essa verificação antecipada é uma das vantagens práticas mais evidentes de trabalhar com uma linguagem estaticamente tipada como Java: erros de incompatibilidade de tipo em chamadas de método são pegos cedo, no momento da compilação, em vez de aparecerem como falhas inesperadas depois que o programa já está em produção, sendo usado por pessoas reais.

### Passagem por valor

Passagem por valor é o mecanismo que Java usa, de forma exclusiva, para transferir argumentos para os parâmetros de um método: em toda invocação, o que é efetivamente copiado para o parâmetro é o valor do argumento no momento da chamada — nunca uma referência viva à variável original de quem fez a chamada.

É comum, especialmente vindo de experiências com outras linguagens ou de intuições incorretas, supor que alterar um parâmetro dentro de um método também alteraria a variável original passada como argumento — uma suposição que leva a bugs difíceis de diagnosticar, já que o comportamento observado não corresponde ao esperado.

A passagem por valor resolve essa questão de forma bem definida: como apenas uma cópia do valor é enviada ao método, qualquer alteração feita sobre o parâmetro dentro do corpo do método afeta apenas essa cópia local, sem nenhum efeito sobre a variável original, que permanece intacta no local de onde a chamada partiu.

```java
public static void dobrar(int numero) {
    numero = numero * 2; // altera apenas a cópia local
}

public static void main(String[] args) {
    int valor = 10;
    dobrar(valor);
    System.out.println(valor); // ainda imprime 10, não 20
}
```

Nesse exemplo, `valor` permanece `10` depois da chamada a `dobrar`, mesmo que o parâmetro `numero`, dentro do método, tenha sido multiplicado por dois — a alteração aconteceu apenas sobre a cópia recebida por `numero`, sem nenhum reflexo sobre `valor`.

Uma boa analogia é a de fotocopiar um documento antes de entregá-lo a alguém para que essa pessoa faça anotações: qualquer rabisco feito na cópia não altera em nada o documento original, que continua limpo nas mãos de quem o entregou. Da mesma forma, o método recebe uma "fotocópia" do valor do argumento, e pode fazer o que quiser com ela, sem que isso volte a afetar o valor original de quem chamou.

Esse comportamento vale igualmente para todos os tipos primitivos vistos até aqui. O caso de variáveis que armazenam objetos — como arrays ou instâncias de classes — introduz uma nuance importante sobre o que exatamente é copiado, tratada com cuidado no próximo conceito.

### Referências passadas por valor

Referências passadas por valor é o nome dado ao comportamento específico da passagem por valor (conceito anterior) quando o argumento envolvido é uma variável de tipo referência — como um array ou um objeto — em vez de um tipo primitivo. A regra geral não muda: o que é copiado para o parâmetro continua sendo apenas um valor. A sutileza está em que, nesses casos, o valor armazenado na variável não é o próprio objeto, mas sim uma referência (algo como um "endereço") que aponta para onde esse objeto vive na memória.

É comum, por isso, concluir de forma equivocada que arrays e objetos são passados "por referência" em Java, no sentido de que o método receberia acesso direto à variável original — confusão alimentada justamente pelo fato de que alterações no conteúdo de um array ou objeto passado como argumento são, sim, visíveis fora do método, o que parece contradizer a regra da passagem por valor.

A explicação, no entanto, é consistente com tudo o que já foi visto: como o que é copiado é a referência (o "endereço"), e não o objeto em si, o parâmetro dentro do método aponta para exatamente o mesmo objeto na memória que a variável original — por isso, alterar o conteúdo desse objeto através do parâmetro é visível de fora, já que não existem dois objetos, apenas duas referências (a original e a cópia) apontando para o mesmo lugar.

```java
public static void alterarPrimeiro(int[] array) {
    array[0] = 100; // altera o conteúdo do array original
}

public static void reatribuir(int[] array) {
    array = new int[]{9, 9, 9}; // troca apenas a cópia local da referência
}

public static void main(String[] args) {
    int[] numeros = {1, 2, 3};
    alterarPrimeiro(numeros);
    System.out.println(numeros[0]); // imprime 100: o conteúdo mudou

    reatribuir(numeros);
    System.out.println(numeros[0]); // ainda imprime 100: a variável original não mudou
}
```

Esse exemplo distingue os dois casos com clareza: `alterarPrimeiro` modifica o conteúdo do array através da referência copiada, e essa mudança é visível fora do método, porque ambas as referências (a original em `numeros` e a cópia em `array`) apontam para o mesmo array na memória. Já `reatribuir` troca para qual array a cópia local aponta, mas isso não afeta a variável `numeros` do `main`, que continua apontando para o array original — reforçando que apenas a referência foi copiada, não a variável em si.

A analogia mais precisa aqui é a de um controle remoto e uma televisão: passar o array como argumento é como entregar uma cópia idêntica do controle remoto (a referência), não a televisão em si (o objeto). Apertar um botão nesse controle copiado (alterar o conteúdo do array) muda a mesma televisão, já que ambos os controles operam o mesmo aparelho — mas trocar o controle copiado por um controle totalmente diferente (reatribuir a variável local) não faz com que o controle original, nas mãos de quem chamou o método, também mude.

## Retorno

Um método pode não só receber dados, como também devolver um resultado para quem o chamou — é isso que permite, por exemplo, calcular um valor dentro de um método e usar esse valor em outro lugar do programa. Este capítulo trata do mecanismo de retorno em Java: a palavra-chave `return`, o tipo especial `void` usado quando um método não devolve nada, e como estruturar métodos que efetivamente retornam valores de tipos variados.

O capítulo também mostra como métodos que retornam valores podem ser combinados entre si — a composição de métodos, em que o resultado de um alimenta a chamada de outro — reforçando a ideia, já apresentada no capítulo sobre criação de métodos, de que programas bem organizados são construídos a partir de peças pequenas e previsíveis que se encaixam. Compreender bem o `return` é também pré-requisito para os próximos capítulos, em que um mesmo método pode ter várias formas ou chamar a si mesmo.

### `return`

A instrução `return` é o comando usado dentro do corpo de um método para encerrar sua execução imediatamente, devolvendo o controle do programa para o ponto exato de onde o método foi invocado — opcionalmente, levando consigo um valor a ser entregue a quem fez a chamada, dependendo de o método ser `void` ou de retorno não vazio, tema dos dois próximos conceitos.

Sem uma instrução como o `return`, um método sempre executaria da primeira à última instrução do seu corpo, sem nenhuma forma de interromper esse fluxo antecipadamente diante de alguma condição já identificada dentro do próprio método, nem de comunicar um resultado de volta para quem o chamou.

O `return` resolve os dois problemas de uma vez: ele permite sair do método a qualquer ponto do seu corpo (não apenas na última linha), e, quando o método não é `void`, ele é a única forma de entregar um valor de volta ao ponto de chamada.

```java
public static int primeiroPositivo(int a, int b) {
    if (a > 0) {
        return a; // encerra o método aqui, se a condição for verdadeira
    }
    return b; // só é alcançado se o return anterior não tiver sido executado
}
```

Nesse exemplo, se `a` for positivo, o método é encerrado logo na primeira instrução `return`, e a segunda linha (`return b`) nunca chega a ser executada para essa chamada específica — uma ilustração de como o `return` interrompe o fluxo normal do corpo do método assim que é alcançado, independentemente de haver mais instruções depois dele.

Um método pode ter várias instruções `return` espalhadas pelo seu corpo, geralmente associadas a diferentes condições (como no exemplo acima), mas apenas uma delas será de fato executada em cada chamada — assim que a primeira `return` alcançada é executada, o restante do corpo do método é ignorado para aquela invocação específica.

Vale notar que todo caminho possível dentro de um método de retorno não vazio precisa, obrigatoriamente, terminar em algum `return` — o compilador Java verifica essa exigência e recusa compilar um método que tenha algum caminho de execução sem um `return` correspondente, um cuidado que evita que um método prometa um valor de retorno e, em alguma situação, simplesmente não o entregue.

### `void`

`void` é a palavra-chave usada no lugar do tipo de retorno na declaração de um método para indicar que ele não devolve nenhum valor para quem o invoca — ele realiza uma ação (como imprimir algo na tela, ou alterar o conteúdo de um array, como visto no conceito de referências passadas por valor), mas não produz um resultado que possa ser armazenado em uma variável ou usado dentro de uma expressão maior.

Todo método, se não existisse essa palavra-chave, seria obrigado a devolver algo — mesmo quando sua finalidade fosse puramente executar uma ação, como imprimir uma mensagem, sem que houvesse, de fato, um resultado natural para se devolver.

A solução é a própria palavra-chave: declarado com `void` no lugar do tipo de retorno, o método fica isento da exigência (vista no conceito anterior) de terminar todo caminho possível em um `return` com valor — nesses métodos, o `return` é opcional, e, quando usado, aparece sozinho, sem nenhum valor associado a ele.

```java
public static void imprimirSaudacao(String nome) {
    System.out.println("Olá, " + nome + "!");
    // não há necessidade de return aqui
}

public static void main(String[] args) {
    imprimirSaudacao("Maria"); // não é possível fazer: String s = imprimirSaudacao("Maria");
}
```

Nesse exemplo, `imprimirSaudacao` é declarado como `void` porque sua única finalidade é imprimir uma mensagem na tela — não existe um valor natural que faça sentido devolver dali. Por isso, tentar atribuir o resultado dessa chamada a uma variável seria um erro de compilação: métodos `void` simplesmente não produzem um valor utilizável dessa forma.

Dentro de um método `void`, ainda é possível usar `return` (sem nenhum valor após ele) para encerrar a execução antecipadamente, de forma parecida com o que foi visto no conceito anterior, só que sem devolver nada:

```java
public static void imprimirSePositivo(int numero) {
    if (numero <= 0) {
        return; // encerra o método sem imprimir nada
    }
    System.out.println(numero);
}
```

Métodos `void` são especialmente comuns quando a finalidade do método é produzir um efeito observável — imprimir algo, alterar o conteúdo de um array recebido como argumento, gravar um valor em algum lugar — em vez de calcular e devolver um resultado que será usado por quem chamou.

### Retorno de valores

Retorno de valores é o comportamento de um método declarado com um tipo diferente de `void` (como `int`, `double`, `boolean`, `String`, entre outros), que é obrigado a devolver, através do `return`, um valor compatível com esse tipo declarado, sempre que for invocado — é o mecanismo que permite que um método funcione como uma expressão, produzindo um resultado utilizável por quem o chamou.

Imagine tentar escrever `int total = calcularTotal(precos)` em uma linguagem onde métodos só pudessem ser usados pelo efeito colateral que produzem, como imprimir algo na tela: seria impossível, porque não haveria como um método "devolver" o resultado de um cálculo para ser armazenado ou reutilizado.

É exatamente isso que o retorno de valores viabiliza: o próprio tipo declarado na assinatura do método (no lugar onde `void` apareceria, caso não houvesse retorno) determina o tipo do valor que o `return` deve entregar — e esse valor, uma vez devolvido, pode ser atribuído a uma variável, usado diretamente dentro de uma expressão maior, ou passado como argumento para outro método.

```java
public static double calcularMedia(double nota1, double nota2, double nota3) {
    return (nota1 + nota2 + nota3) / 3;
}

public static void main(String[] args) {
    double media = calcularMedia(7.0, 8.0, 6.5); // o valor devolvido é armazenado
    System.out.println("Média: " + media);

    if (calcularMedia(9.0, 9.0, 9.0) >= 6.0) { // o valor devolvido é usado direto numa condição
        System.out.println("Aprovado com média alta.");
    }
}
```

Nesse exemplo, `calcularMedia` devolve um `double`, e esse valor pode ser tratado exatamente como qualquer outro valor `double` do programa: armazenado em uma variável, comparado com `>=`, ou usado em qualquer outra expressão válida para esse tipo.

O tipo do valor devolvido pelo `return` precisa ser compatível com o tipo declarado na assinatura do método, seguindo as mesmas regras de conversão implícita já estudadas no módulo anterior — um método declarado como `double` pode devolver um valor `int` (que será promovido automaticamente), mas um método declarado como `int` não pode devolver diretamente um `double` sem um casting explícito, pelo mesmo motivo de perda de precisão discutido naquele módulo.

### Composição de métodos

Composição de métodos é a prática de usar o valor devolvido por um método como argumento de outro, ou dentro do corpo de um terceiro método, encadeando várias operações menores para formar um cálculo ou comportamento mais complexo — uma extensão natural de tudo o que já foi visto neste capítulo sobre `return` e retorno de valores.

Cada resultado intermediário produzido por um método, sem essa possibilidade, precisaria ser obrigatoriamente armazenado em uma variável antes de ser reutilizado em outro cálculo — mesmo quando esse valor não tivesse nenhuma utilidade própria além de servir de ponte para o próximo passo, o que tornaria o código mais longo, com variáveis criadas apenas para transporte momentâneo de um valor.

Para evitar isso, a composição permite que a chamada a um método apareça diretamente onde um valor daquele tipo é esperado — inclusive como argumento de outra chamada de método — dispensando a criação de uma variável intermediária quando ela não agrega clareza ao código.

```java
public static double calcularMedia(double a, double b) {
    return (a + b) / 2;
}

public static boolean aprovado(double media) {
    return media >= 6.0;
}

public static void main(String[] args) {
    // composição: o retorno de calcularMedia vira argumento de aprovado, direto
    boolean resultado = aprovado(calcularMedia(7.0, 8.0));
    System.out.println(resultado); // imprime true
}
```

Nesse exemplo, em vez de calcular a média, guardá-la em uma variável, e só depois chamar `aprovado` com essa variável, a chamada a `calcularMedia` é usada diretamente como argumento de `aprovado`, compondo as duas operações em uma única expressão.

É importante usar a composição com moderação: encadear chamadas de métodos demais em uma única linha pode tornar o código mais difícil de ler e de depurar (já que não há um nome de variável intermediário indicando o que cada resultado parcial representa) — como regra prática, compor duas ou três chamadas relacionadas costuma deixar o código mais enxuto sem prejudicar a leitura, mas encadeamentos muito mais longos que isso geralmente se beneficiam de serem quebrados de volta em variáveis intermediárias, com nomes que expliquem o que cada resultado parcial significa.

## Sobrecarga e varargs

Às vezes faz sentido que um mesmo nome de método sirva para variações ligeiramente diferentes de uma mesma operação — por exemplo, somar dois números inteiros ou somar dois números decimais. Este capítulo mostra como Java permite isso através da sobrecarga (overloading): várias versões de um método com o mesmo nome, mas com listas de parâmetros diferentes, deixando o próprio compilador decidir qual versão usar em cada chamada.

O capítulo também aborda os varargs (`String...` e a sintaxe equivalente para outros tipos), um recurso que permite a um método aceitar uma quantidade variável de argumentos do mesmo tipo, e como isso se relaciona com o uso de arrays como argumentos. Por fim, trata de como o compilador resolve qual versão sobrecarregada chamar (resolução de sobrecarga) e dos casos em que essa escolha se torna ambígua, gerando erro de compilação. São recursos que dão mais flexibilidade na hora de projetar a interface de um método, sem abrir mão da clareza do nome escolhido.

### Overloading

Overloading (sobrecarga de métodos) é a possibilidade de declarar, dentro da mesma classe, mais de um método com o mesmo nome, desde que suas assinaturas — como visto no primeiro capítulo deste módulo — sejam diferentes entre si, seja na quantidade de parâmetros, seja nos tipos desses parâmetros. É um recurso que permite oferecer variações do "mesmo" comportamento, adaptadas a diferentes tipos ou quantidades de entrada.

Sem overloading, cada variação de comportamento de um método precisaria de um nome próprio e distinto — como `somarInteiros`, `somarDecimais`, `somarTresValores` — mesmo quando a ideia central de todos eles fosse essencialmente a mesma (somar valores). Isso obriga quem usa esses métodos a memorizar vários nomes diferentes para operações conceitualmente equivalentes, e frequentemente força escolhas de nomenclatura pouco naturais.

O overloading resolve isso permitindo reunir todas essas variações sob um único nome compartilhado, deixando o compilador decidir automaticamente, no momento de cada chamada, qual das versões declaradas deve ser executada, com base nos tipos e na quantidade dos argumentos fornecidos — um processo detalhado no conceito de resolução de sobrecarga, mais adiante.

```java
public static int somar(int a, int b) {
    return a + b;
}

public static double somar(double a, double b) {
    return a + b;
}

public static int somar(int a, int b, int c) {
    return a + b + c;
}
```

Nesse conjunto de declarações, os três métodos compartilham o nome `somar`, mas têm assinaturas diferentes entre si — os dois primeiros diferem no tipo dos parâmetros (`int` versus `double`), e o terceiro difere na quantidade de parâmetros. Isso é uma sobrecarga válida, e o Java permite que as três declarações coexistam na mesma classe sem conflito.

Uma boa analogia é pensar em um mesmo verbo em português usado com objetos diferentes, como "abrir a porta" e "abrir uma exceção" — o verbo é o mesmo, mas o contexto (os "argumentos" da frase) deixa claro qual sentido se aplica. Da mesma forma, `somar(int, int)` e `somar(double, double)` compartilham o nome porque representam a mesma ideia conceitual, ainda que operem sobre tipos diferentes.

É importante notar que apenas mudar o tipo de retorno, sem alterar a lista de parâmetros, não é suficiente para configurar uma sobrecarga válida — como já visto no conceito de assinatura, o tipo de retorno não faz parte dela, então dois métodos com mesmo nome, mesmos parâmetros e retornos diferentes gerariam um erro de compilação, não uma sobrecarga.

### `String...`

`String...` é a notação usada para declarar um parâmetro do tipo varargs — abreviação de "variable arguments", mecanismo geral que o próximo conceito detalha por completo — especificamente para argumentos do tipo `String`. Ela permite que um método aceite zero, um, ou qualquer quantidade de textos como argumentos, sem que o programador precise saber de antemão quantos serão fornecidos em cada chamada.

Textos são provavelmente o caso de uso mais comum de varargs no dia a dia: mensagens compostas a partir de vários nomes, linhas de um relatório, ou argumentos recebidos por linha de comando (como o próprio `args` do `main`, ainda que ali a notação usada seja `String[]`, e não `String...`). Por isso vale conhecer essa forma específica antes de ver o mecanismo geral — é, de longe, a mais encontrada em código real.

```java
public static void imprimirTodos(String... nomes) {
    for (String nome : nomes) {
        System.out.println(nome);
    }
}

public static void main(String[] args) {
    imprimirTodos("Ana");
    imprimirTodos("Ana", "Bruno", "Carla");
    imprimirTodos(); // também é válido: nenhum argumento
}
```

Nesse exemplo, o mesmo método `imprimirTodos` aceita qualquer quantidade de nomes, incluindo nenhum, sem que seja preciso declarar várias versões sobrecarregadas para cada quantidade possível de argumentos — o compilador trata `nomes`, internamente, como um array de `String`, percorrido normalmente com `for`.

A sintaxe `String...` é apenas uma aplicação específica do mecanismo mais geral de varargs, que funciona da mesma forma para qualquer tipo (`int...`, `double...`, e assim por diante). O próximo conceito explica por que esse mecanismo existe, como ele resolve o problema de aceitar uma quantidade não fixa de argumentos, e detalha suas regras de posicionamento na assinatura do método.

### Varargs

Varargs é o mecanismo geral (do qual `String...`, visto no conceito anterior, é apenas um caso específico) que permite declarar um parâmetro de método capaz de aceitar uma quantidade variável de argumentos do mesmo tipo, usando a notação `Tipo... nome` na assinatura do método — reticências logo após o tipo do parâmetro.

Sem varargs, qualquer método que precisasse aceitar uma quantidade não fixa de argumentos do mesmo tipo teria duas alternativas pouco satisfatórias: sobrecarregar manualmente uma versão do método para cada quantidade possível de argumentos (algo inviável quando essa quantidade pode variar amplamente), ou exigir que o próprio chamador monte um array antes de cada chamada, adicionando um passo extra sempre que o método for usado.

Varargs resolve isso permitindo declarar um único parâmetro especial, que internamente é tratado como um array do tipo declarado, mas que, do ponto de vista de quem chama o método, pode ser preenchido simplesmente listando os valores separados por vírgula — o compilador se encarrega de empacotar esses valores em um array automaticamente, nos bastidores.

```java
public static int somarTodos(int... numeros) {
    int total = 0;
    for (int n : numeros) {
        total += n;
    }
    return total;
}

public static void main(String[] args) {
    System.out.println(somarTodos(1, 2));          // imprime 3
    System.out.println(somarTodos(1, 2, 3, 4, 5));  // imprime 15
    System.out.println(somarTodos());               // imprime 0
}
```

Nesse exemplo, `somarTodos` aceita qualquer quantidade de números inteiros, sem que seja necessário declarar uma versão distinta do método para cada quantidade possível de argumentos — o mesmo método atende a duas, cinco, ou nenhuma chamada, tratando internamente `numeros` como um array comum, percorrido com o mesmo `for` já familiar de arrays.

Uma regra importante de sintaxe é que um método só pode ter um único parâmetro varargs, e ele precisa obrigatoriamente ser o último da lista de parâmetros — não é possível ter parâmetros comuns depois de um varargs, já que o compilador não teria como saber onde os argumentos variáveis terminam e os parâmetros seguintes começam.

```java
public static void exemplo(String prefixo, int... numeros) { // válido: varargs por último
    // ...
}
```

Como visto no exemplo anterior deste mesmo conceito, é perfeitamente válido chamar um método varargs sem fornecer nenhum argumento para esse parâmetro — nesse caso, o array recebido internamente simplesmente tem tamanho zero, sem gerar nenhum erro.

### Arrays como argumentos

Como visto no conceito anterior, um parâmetro varargs é internamente tratado como um array — e essa relação direta permite que, além de listar os valores separados por vírgula, seja possível também passar um array já pronto diretamente como argumento para um parâmetro varargs, sem precisar reescrever seus elementos individualmente.

Sem essa flexibilidade, um programa que já tivesse seus dados organizados em um array (por já terem sido lidos de algum lugar, por exemplo) seria forçado a "desmontar" esse array em valores individuais separados por vírgula antes de poder chamar um método varargs, o que seria trabalhoso e, em muitos casos, nem seria possível fazer de forma prática (especialmente se o tamanho do array só fosse conhecido em tempo de execução).

Java resolve isso permitindo que, sempre que um método espera um parâmetro varargs, um array já existente e compatível com o tipo declarado possa ser passado diretamente como argumento único, no lugar de vários valores separados por vírgula — o compilador reconhece esse caso e usa o array fornecido exatamente como ele é, sem envolvê-lo em outro array.

```java
public static int somarTodos(int... numeros) {
    int total = 0;
    for (int n : numeros) {
        total += n;
    }
    return total;
}

public static void main(String[] args) {
    int[] valores = {10, 20, 30};
    System.out.println(somarTodos(valores)); // passa o array direto, sem listar elementos
    System.out.println(somarTodos(10, 20, 30)); // forma equivalente, valor a valor
}
```

Nesse exemplo, as duas chamadas a `somarTodos` produzem exatamente o mesmo resultado (`60`) — a primeira passando um array já pronto, a segunda listando os mesmos valores individualmente. Isso confirma, na prática, que o mecanismo de varargs e um array comum do mesmo tipo são, por baixo dos panos, a mesma coisa: apenas duas formas diferentes de preencher o mesmo parâmetro.

Essa equivalência é especialmente útil quando o método varargs é chamado a partir de outro método que já recebeu os dados como array (como o próprio `args` do `main`, que é um `String[]`) — nesses casos, não há necessidade de nenhuma conversão manual, o array pode simplesmente ser repassado como argumento direto.

### Resolução de sobrecarga

Resolução de sobrecarga é o processo que o compilador Java executa, em toda chamada de método, para decidir exatamente qual, entre as várias versões sobrecarregadas (conceito visto no início deste capítulo) de um método com o mesmo nome, deve ser efetivamente executada — decisão tomada com base nos tipos e na quantidade dos argumentos fornecidos naquela chamada específica.

Sem um processo bem definido de resolução, a existência de várias declarações com o mesmo nome (permitida pelo overloading) seria inútil, ou pior, ambígua: o compilador precisaria de alguma forma sistemática de escolher entre as opções disponíveis, e essa escolha precisa ser previsível e consistente, para que o comportamento de um programa não dependa de acaso.

A resolução resolve isso seguindo uma ordem de preferência bem definida: primeiro, o compilador procura por uma assinatura que corresponda exatamente aos tipos dos argumentos, sem nenhuma conversão; se não encontrar, procura por uma correspondência possível através de conversões implícitas (como as de tipos primitivos vistas no módulo anterior, ou entre tipos e seus wrappers); e, por fim, considera versões varargs, que costumam ser a última alternativa avaliada, justamente por serem as mais "flexíveis" na quantidade de argumentos aceitos.

```java
public static void mostrar(int numero) {
    System.out.println("versão int: " + numero);
}

public static void mostrar(double numero) {
    System.out.println("versão double: " + numero);
}

public static void main(String[] args) {
    mostrar(5);    // escolhe a versão int: correspondência exata
    mostrar(5.0);  // escolhe a versão double: correspondência exata
    mostrar((byte) 5); // escolhe a versão int: byte é promovido para int implicitamente
}
```

Nesse exemplo, a chamada `mostrar(5)` escolhe diretamente a versão com `int`, por ser uma correspondência exata de tipo — mesmo existindo uma versão com `double` que também seria tecnicamente compatível (já que `int` pode ser promovido implicitamente para `double`), o compilador prioriza sempre a correspondência mais exata disponível antes de considerar qualquer conversão.

Entender esse processo é importante para prever, com segurança, qual versão de um método sobrecarregado será realmente executada em cada chamada — especialmente em programas com várias sobrecargas parecidas, onde a diferença entre elas pode não ser óbvia à primeira vista, sem conhecer essa ordem de prioridade que o compilador segue de forma consistente.

### Ambiguidades

Ambiguidade de sobrecarga é a situação em que o compilador Java, durante o processo de resolução descrito no conceito anterior, encontra mais de uma versão sobrecarregada de um método igualmente válida para uma determinada chamada, sem conseguir decidir com segurança qual delas deveria ser escolhida — nesses casos, em vez de escolher arbitrariamente uma das opções, o compilador recusa compilar o programa, sinalizando um erro.

Sem essa recusa explícita, o compilador poderia ser forçado a "adivinhar" entre duas opções igualmente plausíveis, e essa escolha poderia até variar entre diferentes versões do compilador ou situações sutilmente diferentes — um comportamento imprevisível e potencialmente perigoso, já que o programador não teria garantia nenhuma de qual código realmente seria executado.

Java resolve isso de forma conservadora: sempre que mais de uma sobrecarga é igualmente aplicável a uma chamada, sem que nenhuma delas seja claramente mais específica que as outras, o compilador aponta um erro de ambiguidade e recusa compilar, forçando o programador a resolver a situação — seja ajustando os tipos dos argumentos, seja fazendo um casting explícito para indicar exatamente qual versão deve ser usada.

```java
public static void mostrar(long numero) {
    System.out.println("versão long: " + numero);
}

public static void mostrar(double numero) {
    System.out.println("versão double: " + numero);
}

public static void main(String[] args) {
    // mostrar(5); // erro de compilação: ambíguo entre long e double,
                    // já que int pode ser promovido para os dois igualmente bem
    mostrar(5L);    // resolve a ambiguidade: 5L já é long, sem precisar de promoção
}
```

Nesse exemplo, chamar `mostrar(5)` seria ambíguo, porque um `int` pode ser promovido implicitamente tanto para `long` quanto para `double`, e nenhuma das duas conversões é "mais natural" que a outra do ponto de vista do compilador — não há como decidir automaticamente entre elas. Fornecer o argumento já com o tipo `long` explícito (`5L`) remove essa ambiguidade, já que passa a existir uma correspondência exata com uma das duas versões.

Ambiguidades costumam ser mais comuns em sobrecargas que combinam varargs com métodos de parâmetros fixos, ou que envolvem várias conversões implícitas possíveis ao mesmo tempo — uma boa prática, ao projetar sobrecargas próprias, é evitar combinações de tipos que poderiam gerar esse tipo de situação, preferindo assinaturas cujas diferenças sejam claras e não deixem margem para mais de uma interpretação igualmente válida.

## Escopo e recursividade

Este último capítulo do módulo fecha dois temas que ficam mais claros depois de já se conhecer bem como métodos são declarados, recebem parâmetros e retornam valores: onde exatamente uma variável existe e pode ser usada (escopo), e o que acontece quando um método chama a si mesmo (recursividade). Ambos os temas dependem de entender a pilha de chamadas — a estrutura que o Java usa internamente para controlar quais métodos estão em execução e quais variáveis pertencem a cada um.

O capítulo explica o escopo local (variáveis que só existem dentro do bloco onde foram declaradas), o shadowing (quando uma variável de escopo mais interno "esconde" uma de escopo mais externo com o mesmo nome), e como a pilha de chamadas organiza a execução de métodos aninhados. A partir daí, apresenta a recursão como técnica — um método que resolve um problema chamando a si mesmo com uma versão menor do problema —, a importância de um caso-base para essa chamada não se repetir infinitamente, e uma comparação entre recursão e iteração (usar um loop) como abordagens alternativas para o mesmo tipo de problema.

### Escopo local

Escopo local é a região do código, delimitada pelo corpo de um método (ou por um bloco dentro dele, como será visto em módulos futuros sobre estruturas de controle), dentro da qual uma variável declarada existe e pode ser usada — fora dessa região, a variável simplesmente não existe e qualquer tentativa de referenciá-la resulta em erro de compilação.

Sem um conceito de escopo bem definido, todas as variáveis declaradas em qualquer método de um programa estariam potencialmente visíveis e acessíveis de qualquer outro lugar do código, o que tornaria praticamente impossível reutilizar nomes de variáveis simples e comuns (como `i`, `total` ou `resultado`) em métodos diferentes sem que eles colidissem e interferissem uns nos outros.

O escopo local resolve isso confinando cada variável declarada dentro de um método à existência exclusiva daquela execução específica: ela é criada quando sua declaração é alcançada durante a execução, e deixa de existir assim que o método termina (por um `return` ou por chegar ao fim do corpo), sem deixar nenhum rastro acessível de fora.

```java
public static void metodoA() {
    int total = 10; // total existe apenas dentro de metodoA
    System.out.println(total);
}

public static void metodoB() {
    int total = 999; // outro "total", completamente independente do de metodoA
    System.out.println(total);
}
```

Nesse exemplo, os dois métodos declaram uma variável chamada `total`, mas cada uma delas é local ao seu próprio método — não há conflito nenhum entre elas, e alterar o valor de `total` em `metodoA` não tem absolutamente nenhum efeito sobre o `total` de `metodoB`, já que se trata de duas variáveis completamente distintas, que apenas compartilham o mesmo nome por coincidência.

Uma boa analogia é pensar em cada chamada de método como uma sala própria e isolada: variáveis declaradas "dentro da sala" (no corpo do método) só existem enquanto aquela sala estiver em uso, e não são visíveis nem acessíveis de nenhuma outra sala, mesmo que móveis com o mesmo nome (variáveis com o mesmo identificador) existam em salas diferentes ao mesmo tempo.

Esse comportamento tem uma relação direta com o conceito de pilha de chamadas, tratado adiante neste capítulo: cada chamada de método, mesmo que seja uma chamada repetida ao mesmo método (como acontece na recursão), recebe seu próprio escopo local independente, com suas próprias variáveis, isoladas das variáveis de qualquer outra chamada em andamento.

### Shadowing

Shadowing (sombreamento) é o que acontece quando uma variável declarada em um escopo mais interno tem o mesmo nome de uma variável já existente em um escopo mais externo (como um parâmetro de método e uma variável de mesmo nome declarada dentro de um bloco aninhado) — a variável mais interna "esconde" temporariamente a mais externa, tornando-a inacessível por aquele nome enquanto o escopo interno estiver ativo.

Sem esse conceito bem definido, o comportamento de nomes repetidos em escopos aninhados seria imprevisível: não ficaria claro se uma referência a um nome dentro de um bloco mais interno se refere à variável recém-declarada ali, ou a uma variável de mesmo nome vinda de um escopo mais externo — uma ambiguidade que o compilador precisa resolver de forma consistente.

Java resolve isso com uma regra simples: dentro de um escopo mais interno, uma referência a um nome sempre aponta para a declaração mais próxima (a mais interna) daquele nome — enquanto esse escopo interno estiver ativo, não há forma de acessar a variável externa de mesmo nome por esse identificador, ela fica "sombreada". Importante notar que Java proíbe, em muitos casos, redeclarar dentro de um bloco uma variável com o mesmo nome de um parâmetro ou variável local já visível naquele mesmo escopo — o shadowing costuma ser permitido principalmente entre atributos de classe (tema de módulos futuros) e variáveis locais ou parâmetros que compartilham o nome.

```java
public class Exemplo {
    static int numero = 100; // atributo de classe

    public static void mostrar(int numero) { // parâmetro "numero" sombreia o atributo
        System.out.println(numero); // refere-se ao parâmetro, não ao atributo
    }

    public static void main(String[] args) {
        mostrar(5); // imprime 5, o valor do parâmetro, não 100
    }
}
```

Nesse exemplo, o parâmetro `numero` do método `mostrar` sombreia o atributo `numero` da classe: dentro do corpo desse método, qualquer referência simples a `numero` aponta para o parâmetro (o escopo mais interno e mais próximo), tornando o atributo de classe inacessível por esse nome ali dentro (ele ainda poderia ser acessado explicitamente, com uma sintaxe própria estudada em módulos futuros sobre classes).

Embora o shadowing seja permitido pela linguagem nessas situações, ele costuma ser considerado uma prática arriscada quando usado sem necessidade real, já que pode confundir quem lê o código, fazendo-o supor, por engano, que uma referência aponta para uma variável quando na verdade aponta para outra, de escopo diferente mas mesmo nome — por isso, escolher nomes distintos para variáveis em escopos aninhados costuma ser uma prática mais segura e mais clara do que depender do sombreamento.

### Pilha de chamadas

A pilha de chamadas (call stack) é a estrutura que a JVM usa internamente para controlar a sequência de métodos em execução em um determinado momento, registrando, para cada chamada ainda não concluída, o ponto exato de onde ela partiu e o escopo local (visto no primeiro conceito deste capítulo) daquela execução específica — é o mecanismo que permite que, ao final de um método, a execução saiba exatamente para onde voltar.

Sem uma estrutura assim, o programa não teria como saber, quando um método termina, exatamente para onde a execução deveria retornar — especialmente em cenários com vários métodos chamando uns aos outros em sequência, ou chamando a si mesmos repetidamente, como acontece na recursão, tema do próximo conceito.

A pilha resolve isso funcionando exatamente como o nome sugere: uma pilha, no sentido literal de objetos empilhados uns sobre os outros, em que cada chamada de método é "empilhada" no topo assim que começa, e "desempilhada" assim que termina, revelando novamente, logo abaixo, a chamada anterior — que retoma sua execução exatamente de onde havia parado.

```java
public static void metodoC() {
    System.out.println("Executando C");
}

public static void metodoB() {
    System.out.println("Início de B");
    metodoC(); // B empilha C
    System.out.println("Fim de B");
}

public static void metodoA() {
    System.out.println("Início de A");
    metodoB(); // A empilha B
    System.out.println("Fim de A");
}
```

Ao chamar `metodoA()`, a pilha recebe primeiro `metodoA`; dentro dele, a chamada a `metodoB` empilha `metodoB` por cima; dentro de `metodoB`, a chamada a `metodoC` empilha `metodoC` no topo. Quando `metodoC` termina, ele é removido do topo da pilha, e a execução retoma exatamente onde `metodoB` havia parado (a linha logo após a chamada a `metodoC`) — e assim sucessivamente, até que a pilha volte a ficar vazia.

Uma analogia útil é a de uma pilha de pratos empilhados: o último prato colocado é sempre o primeiro a ser retirado (um comportamento conhecido em outras áreas da programação como LIFO, "last in, first out"). Da mesma forma, o último método chamado é sempre o primeiro a terminar e ser removido da pilha, revelando o método anterior logo abaixo, que retoma seu trabalho de onde havia parado.

A pilha de chamadas também tem um limite de tamanho: se métodos continuarem se chamando sem nunca terminar (por exemplo, uma recursão sem um caso de parada adequado, tema tratado nos próximos dois conceitos), a pilha eventualmente esgota seu espaço disponível, e a JVM interrompe o programa com um erro conhecido como `StackOverflowError`.

### Recursão

Recursão é a técnica em que um método chama a si mesmo, direta ou indiretamente, como parte de sua própria execução — uma aplicação direta do conceito de pilha de chamadas visto no conceito anterior, já que cada chamada recursiva empilha uma nova execução do mesmo método, com seu próprio escopo local independente das chamadas anteriores.

Sem a recursão, certos problemas que são naturalmente definidos "em termos de si mesmos" — como calcular o fatorial de um número, ou percorrer uma estrutura que contém outras estruturas do mesmo tipo dentro dela — precisariam ser resolvidos exclusivamente com estruturas de repetição explícitas (tema de um módulo futuro), o que, para alguns desses problemas, resulta em um código bem menos direto de escrever e de entender do que a versão recursiva equivalente.

A recursão resolve isso permitindo que um método expresse sua solução como uma combinação entre um caso simples, resolvido diretamente (o caso-base, tema do próximo conceito), e uma chamada a si mesmo para resolver uma versão "menor" do mesmo problema, aproximando-se progressivamente do caso-base a cada nova chamada.

```java
public static int fatorial(int n) {
    if (n <= 1) {
        return 1; // caso-base
    }
    return n * fatorial(n - 1); // chamada recursiva, com um problema "menor"
}

public static void main(String[] args) {
    System.out.println(fatorial(5)); // imprime 120
}
```

Nesse exemplo, `fatorial(5)` chama `fatorial(4)`, que chama `fatorial(3)`, e assim sucessivamente, até `fatorial(1)`, que resolve o caso-base diretamente, sem mais chamadas. A partir daí, cada chamada empilhada devolve seu resultado para a chamada anterior, que multiplica esse resultado pelo seu próprio `n`, até que a primeira chamada, `fatorial(5)`, finalmente receba e devolva o resultado completo (`120`).

Uma boa analogia para a recursão é a de um conjunto de bonecas russas (matrioscas): para saber o que há dentro da maior boneca, é preciso abri-la e olhar dentro da boneca seguinte, um pouco menor, repetindo esse processo até chegar à menor boneca de todas, que não contém mais nenhuma outra dentro (o equivalente ao caso-base) — só então é possível "montar de volta" a resposta, boneca por boneca, na ordem inversa em que foram abertas.

Como já vimos ao tratar da pilha de chamadas, cada chamada de método recebe seu próprio espaço isolado na pilha; na recursão, isso significa que cada `fatorial(n)` guarda seu próprio `n`, sem qualquer risco de uma chamada embaralhar a variável de outra que ainda está pendente — é justamente esse isolamento que garante que a multiplicação final (`n * fatorial(n - 1)`) use sempre o valor correto de `n` de cada nível da cadeia.

### Caso-base

Caso-base é a condição, dentro de um método recursivo (conceito anterior), que interrompe a cadeia de chamadas a si mesmo, resolvendo o problema diretamente, sem gerar nenhuma nova chamada recursiva — é o elemento que garante que uma recursão eventualmente termine, em vez de continuar chamando a si mesma indefinidamente.

Um método recursivo sem um caso-base bem definido — ou, pior, sem nenhum caso-base — continuaria chamando a si mesmo indefinidamente, empilhando uma nova execução a cada chamada, sem nunca alcançar uma condição de parada; como já vimos ao falar da pilha de chamadas, isso eventualmente esgota o espaço disponível nela, e a JVM interrompe o programa com um `StackOverflowError`.

Por isso, o caso-base funciona como uma "saída de emergência" dentro da lógica recursiva: uma condição, verificada no início do método (geralmente através de um `if`, tema do próximo módulo deste livro), que identifica quando o problema já está simples o suficiente para ser resolvido diretamente, sem necessidade de mais nenhuma chamada recursiva.

```java
public static int somarAte(int n) {
    if (n <= 0) {
        return 0; // caso-base: não há mais nada a somar
    }
    return n + somarAte(n - 1); // aproxima-se do caso-base a cada chamada
}
```

Nesse exemplo, `n <= 0` é o caso-base: assim que `n` chega a zero (ou menos), o método devolve `0` diretamente, sem fazer nenhuma nova chamada recursiva, interrompendo ali a cadeia de chamadas que vinha sendo empilhada.

Um erro comum ao escrever recursão é esquecer o caso-base, ou escrevê-lo de forma que ele nunca seja de fato alcançado — por exemplo, se a chamada recursiva de `somarAte` fosse escrita como `somarAte(n + 1)` em vez de `somarAte(n - 1)`, o valor de `n` nunca se aproximaria de `0`, e o caso-base jamais seria atingido, levando inevitavelmente a um `StackOverflowError`.

Por isso, toda recursão bem escrita precisa garantir duas coisas ao mesmo tempo: a existência de pelo menos um caso-base alcançável, e a garantia de que cada chamada recursiva se aproxima progressivamente desse caso-base (reduzindo `n`, encurtando uma estrutura, ou qualquer medida equivalente de "progresso" em direção à condição de parada) — sem essas duas garantias, mesmo uma recursão sintaticamente correta pode nunca terminar na prática.

### Recursão × iteração

Recursão e iteração são duas abordagens diferentes para resolver o mesmo tipo de problema: repetir uma operação múltiplas vezes até alcançar um resultado. A recursão, vista nos conceitos anteriores, resolve isso através de um método chamando a si mesmo; a iteração (tema formal de um módulo futuro deste livro, com estruturas como `for` e `while`) resolve o mesmo tipo de problema repetindo um bloco de código dentro de um único método, sem novas chamadas empilhadas.

Sem entender as diferenças práticas entre as duas abordagens, é fácil escolher recursão para problemas em que a iteração seria mais simples e mais eficiente, ou vice-versa — cada uma tem vantagens que a tornam mais adequada a diferentes tipos de situação, e boa parte da experiência de programar envolve reconhecer qual delas se encaixa melhor em cada caso.

A comparação entre as duas resolve essa dúvida ao evidenciar os pontos fortes de cada abordagem: a recursão tende a expressar de forma mais direta e legível problemas que já são naturalmente definidos em termos de si mesmos (como percorrer estruturas aninhadas), enquanto a iteração costuma ser mais eficiente em termos de memória, já que não empilha uma nova execução a cada repetição — como visto no conceito de pilha de chamadas, cada chamada recursiva ocupa espaço próprio na pilha, o que a iteração simplesmente não precisa fazer.

```java
// versão recursiva
public static int fatorialRecursivo(int n) {
    if (n <= 1) return 1;
    return n * fatorialRecursivo(n - 1);
}

// versão iterativa (adiantando a sintaxe de "for", vista em módulo futuro)
public static int fatorialIterativo(int n) {
    int resultado = 1;
    for (int i = 2; i <= n; i++) {
        resultado *= i;
    }
    return resultado;
}
```

Ambas as versões calculam exatamente o mesmo resultado para qualquer valor de `n`, mas de formas estruturalmente diferentes: a recursiva empilha uma chamada por vez até o caso-base, enquanto a iterativa mantém uma única execução do método, atualizando `resultado` repetidamente dentro do laço.

Na prática, para problemas simples como o fatorial, a versão iterativa costuma ser preferida justamente por evitar o custo extra da pilha de chamadas e o risco de `StackOverflowError` para valores muito grandes de `n` — a recursão tende a se justificar mais claramente em problemas cuja estrutura já é naturalmente recursiva (como percorrer diretórios que contêm subdiretórios, ou certas estruturas de dados que serão vistas em módulos futuros), onde escrever a versão iterativa equivalente seria significativamente mais complexo do que a versão recursiva correspondente.
# Módulo 4 — Decisões

Todo programa mais interessante do que uma sequência fixa de instruções precisa, em algum momento, tomar decisões: fazer uma coisa se uma condição for verdadeira, outra coisa se for falsa. Este módulo apresenta os mecanismos de controle de fluxo condicional de Java, que permitem que um programa reaja de forma diferente dependendo dos dados que recebe, em vez de sempre executar exatamente a mesma sequência de instruções.

O módulo começa pela base lógica de qualquer decisão — as expressões booleanas, que produzem um valor verdadeiro ou falso — e a partir daí constrói progressivamente as estruturas que usam esse valor para desviar o fluxo do programa: o `if` simples, as variações com `else` e `else if` para decisões múltiplas, e o `switch`, tanto em sua forma tradicional quanto na forma moderna de expressão (`switch` expressions). Ao final deste módulo, o leitor será capaz de escrever programas que se comportam de maneiras diferentes conforme as condições encontradas, escolhendo a estrutura de decisão mais adequada para cada situação.

## Expressões booleanas

Antes de aprender qualquer estrutura de decisão, é preciso entender a matéria-prima com que essas estruturas trabalham: expressões que resultam em verdadeiro ou falso. O tipo `boolean` já apareceu brevemente no módulo sobre tipos primitivos, mas este capítulo aprofunda como valores e expressões booleanas são construídos e combinados — a base sobre a qual todo o restante do módulo se apoia.

O capítulo cobre como comparações (usando os operadores relacionais já vistos) produzem um resultado booleano, e como os operadores lógicos `&&` (E), `||` (OU) e `!` (negação) permitem combinar várias condições em uma única expressão mais complexa. Por fim, explica o comportamento de "short circuit" (curto-circuito), em que o Java, em certas condições, nem chega a avaliar o segundo operando de uma expressão lógica — um detalhe que tem implicações práticas importantes, como evitar erros ao verificar se um valor existe antes de acessá-lo.

### `boolean`

`boolean` é o tipo primitivo de Java usado para representar valores lógicos, capaz de armazenar exatamente dois estados possíveis: `true` (verdadeiro) ou `false` (falso) — nenhum outro valor é aceito para uma variável desse tipo. É o tipo fundamental sobre o qual toda a tomada de decisões em Java se apoia, incluindo o `if`, tratado no próximo capítulo.

Sem um tipo dedicado a representar apenas verdadeiro ou falso, um programa precisaria recorrer a convenções improvisadas para representar esses dois estados — como usar `0` e `1`, prática comum em linguagens mais antigas —, o que abre espaço para ambiguidade (o que significaria um `2`, por exemplo?) e torna o código menos expressivo sobre sua real intenção.

O tipo `boolean` resolve isso oferecendo, na própria linguagem, um tipo restrito exatamente aos dois valores que uma decisão lógica pode assumir, tornando explícito, só pelo tipo declarado, que uma variável representa uma condição, e não um número ou texto qualquer.

```java
boolean maiorDeIdade = true;
boolean chovendo = false;

System.out.println(maiorDeIdade); // imprime true
System.out.println(chovendo);     // imprime false
```

Diferente de tipos numéricos como `int`, o `boolean` não participa de conversões implícitas ou explícitas com outros tipos primitivos em Java — não é possível converter um `int` em `boolean` (como seria em outras linguagens, tratando `0` como falso e qualquer outro número como verdadeiro), nem o inverso. Um valor `boolean` só pode vir diretamente dos literais `true` e `false`, ou como resultado de uma expressão lógica ou comparativa (temas dos próximos conceitos deste capítulo).

Uma variável `boolean` costuma ser nomeada de forma que sua leitura já soe como uma pergunta de sim ou não — `maiorDeIdade`, `chovendo`, `aprovado` — uma convenção que torna o código mais legível quando essa variável é usada dentro de uma condição, como será visto já no próximo capítulo deste módulo, sobre o comando `if`.

### Comparações

Operadores de comparação (também chamados de operadores relacionais) são os símbolos usados para comparar dois valores entre si, produzindo sempre um resultado do tipo `boolean` — `true` se a comparação for verdadeira, `false` caso contrário. Java oferece seis deles: `==` (igual a), `!=` (diferente de), `>` (maior que), `<` (menor que), `>=` (maior ou igual a) e `<=` (menor ou igual a).

Sem operadores de comparação, não haveria como transformar uma relação entre dois valores numéricos (ou de outros tipos comparáveis) em uma condição lógica utilizável — não existiria uma forma direta de perguntar, por exemplo, "essa idade é maior ou igual a 18?" e obter uma resposta que o programa pudesse usar para decidir o que fazer em seguida.

As comparações resolvem isso oferecendo uma ponte natural entre valores concretos (números, principalmente) e o mundo lógico do `boolean`: cada operador relacional recebe dois valores comparáveis e devolve um único resultado verdadeiro ou falso, que pode então ser armazenado em uma variável `boolean` ou usado diretamente dentro de uma condição.

```java
int idade = 20;
boolean maiorDeIdade = idade >= 18;
System.out.println(maiorDeIdade); // imprime true

int a = 10;
int b = 10;
System.out.println(a == b); // imprime true: mesmo valor
System.out.println(a != b); // imprime false: não são diferentes
```

É importante não confundir o operador de comparação `==` com o operador de atribuição `=`, estudado no módulo anterior — são símbolos visualmente parecidos, mas com funções completamente diferentes: `=` atribui um valor a uma variável, enquanto `==` compara dois valores e devolve `true` ou `false`, sem alterar nenhum dos dois. Trocar um pelo outro por engano é um erro comum entre iniciantes, e costuma gerar comportamento incorreto sem necessariamente impedir a compilação, dependendo do contexto.

Vale destacar também que, para tipos primitivos numéricos (como `int` e `double`), o `==` compara diretamente os valores armazenados — mas, ao lidar com tipos mais complexos (como `String` e outros objetos, temas de módulos futuros), essa comparação direta pode não se comportar da forma esperada, um cuidado a ser retomado quando esses tipos forem apresentados com mais profundidade.

### `&&`

O operador `&&` (E lógico) combina duas expressões booleanas, produzindo `true` somente quando ambas as expressões avaliadas são verdadeiras — se qualquer uma das duas for falsa, o resultado combinado também será falso. É um dos operadores lógicos mais usados para expressar condições que dependem de múltiplos requisitos sendo satisfeitos simultaneamente.

Sem um operador para combinar condições dessa forma, cada requisito precisaria ser verificado separadamente, em condições aninhadas distintas (um recurso apresentado com mais detalhe no capítulo sobre `if`, `else` e `else if` mais adiante), o que tende a tornar o código mais longo e mais profundamente aninhado do que o necessário quando os requisitos são conceitualmente parte de uma única decisão.

O `&&` resolve isso permitindo expressar, em uma única linha, uma exigência composta por vários requisitos que precisam ser todos verdadeiros ao mesmo tempo, sem necessidade de estruturas aninhadas separadas para cada um deles.

```java
int idade = 20;
boolean temCarteira = true;

boolean podeDirigir = idade >= 18 && temCarteira;
System.out.println(podeDirigir); // imprime true, porque ambas as condições são verdadeiras

boolean exemplo = true && false;
System.out.println(exemplo); // imprime false: uma das duas é falsa
```

Nesse exemplo, `podeDirigir` só é `true` porque as duas condições — ser maior de idade e ter carteira — são simultaneamente verdadeiras; se qualquer uma delas fosse `false`, o resultado combinado também seria `false`, independentemente do valor da outra.

Uma boa analogia é a de uma porta com duas fechaduras diferentes, que só abre quando ambas as chaves são giradas ao mesmo tempo: ter apenas uma das chaves certas não é suficiente para abrir a porta — é preciso que as duas condições, simultaneamente, sejam satisfeitas, exatamente como o `&&` exige que ambos os lados sejam verdadeiros para que o resultado combinado também seja.

O `&&` pode encadear mais de duas condições ao mesmo tempo (`a && b && c`), e, como visto no módulo anterior sobre precedência e associatividade, ele tem uma precedência mais baixa que os operadores de comparação, mas mais alta que o `||`, tratado no próximo conceito — uma ordem que garante que expressões comparativas sejam resolvidas antes de serem combinadas logicamente.

### `||`

O operador `||` (OU lógico) é o complemento natural do `&&`, visto no conceito anterior: em vez de exigir que todas as expressões booleanas combinadas sejam verdadeiras, basta que pelo menos uma delas seja — o resultado só é `false` quando ambas as expressões avaliadas são falsas ao mesmo tempo. É usado quando basta que um entre vários requisitos seja atendido, em vez de todos ao mesmo tempo.

Sem um operador para esse tipo de combinação, expressar uma condição do tipo "qualquer uma destas situações é suficiente" exigiria verificações separadas e repetidas, uma para cada alternativa aceitável, tornando difícil condensar em uma única expressão a ideia de que múltiplos caminhos distintos levam ao mesmo resultado desejado.

O `||` resolve isso permitindo reunir, em uma única expressão, todas as alternativas que tornariam a condição geral verdadeira, bastando que qualquer uma delas, isoladamente, já seja suficiente para satisfazer a condição combinada.

```java
boolean feriado = false;
boolean fimDeSemana = true;

boolean folga = feriado || fimDeSemana;
System.out.println(folga); // imprime true: pelo menos uma das duas é verdadeira

boolean exemplo = false || false;
System.out.println(exemplo); // imprime false: nenhuma das duas é verdadeira
```

Nesse exemplo, `folga` é `true` porque pelo menos uma das duas condições (`fimDeSemana`) é verdadeira, mesmo que a outra (`feriado`) seja falsa — basta uma das alternativas para que o resultado combinado também seja verdadeiro.

Uma analogia útil é a de duas chaves diferentes que abrem a mesma porta, sendo suficiente possuir qualquer uma delas para conseguir entrar: não é preciso ter as duas chaves ao mesmo tempo, como aconteceria com o `&&` — basta uma, entre as opções disponíveis, para que o acesso (o resultado `true`) seja concedido.

Assim como o `&&`, o `||` tem uma precedência bem definida em relação aos demais operadores (mais baixa que o `&&`, como visto no conceito anterior), e pode encadear mais de duas condições em uma única expressão (`a || b || c`), sendo suficiente que qualquer uma delas, isoladamente, seja verdadeira para que toda a expressão combinada resulte em `true`.

### `!`

O operador `!` (NÃO lógico, ou negação) é um operador unário que inverte o valor de uma expressão booleana: aplicado a `true`, produz `false`; aplicado a `false`, produz `true`. Diferente de `&&` e `||`, que combinam duas expressões, o `!` opera sobre uma única expressão, invertendo diretamente seu resultado.

Sem um operador de negação, expressar o oposto de uma condição exigiria reescrever completamente a lógica da comparação original de forma invertida — em vez de simplesmente negar `estaLogado`, seria preciso reescrever toda a expressão que produz esse valor de forma que ela já resultasse no oposto, o que nem sempre é simples ou direto, especialmente para condições mais elaboradas.

O `!` resolve isso oferecendo uma forma direta e compacta de inverter qualquer expressão booleana, por mais complexa que ela seja, sem precisar reescrever sua lógica interna — basta prefixá-la com o símbolo `!`.

```java
boolean estaLogado = false;
System.out.println(!estaLogado); // imprime true: inverte o valor de estaLogado

int idade = 15;
boolean maiorDeIdade = idade >= 18;
System.out.println(!maiorDeIdade); // imprime true: nega o resultado da comparação
```

Nesse exemplo, `!estaLogado` inverte diretamente o valor armazenado, e `!maiorDeIdade` inverte o resultado de uma comparação inteira, sem que seja necessário reescrever essa comparação de outra forma (como trocar `>=` por `<`, o que exigiria reescrever a lógica em vez de simplesmente negá-la).

O `!` é especialmente comum combinado com nomes de variáveis booleanas que já soam como perguntas afirmativas — `!ativo`, `!aprovado`, `!disponivel` — produzindo uma leitura natural equivalente a "não ativo", "não aprovado", "não disponível", o que costuma manter o código legível mesmo depois da negação.

Um cuidado prático com o `!` é evitar aninhá-lo excessivamente ou combiná-lo com expressões já negativas por natureza (como negar uma condição que já usa `!=`), já que isso tende a produzir frases lógicas difíceis de interpretar mentalmente — quando a negação de uma condição composta se torna confusa, geralmente vale a pena reescrever a lógica original de forma mais direta, em vez de simplesmente prefixá-la com `!`.

### Short circuit

Short circuit (avaliação de curto-circuito) é o comportamento pelo qual os operadores `&&` e `||`, vistos nos conceitos anteriores, evitam avaliar o lado direito de uma expressão sempre que o resultado final já pode ser determinado apenas pelo valor do lado esquerdo — no `&&`, se o lado esquerdo já é `false`, o resultado combinado já é `false`, então o lado direito nem chega a ser avaliado; no `||`, se o lado esquerdo já é `true`, o mesmo raciocínio se aplica, e o lado direito também é ignorado.

Sem esse comportamento, toda expressão lógica composta precisaria avaliar obrigatoriamente os dois lados, mesmo quando o resultado já estivesse decidido apenas pelo primeiro — o que, além de representar um trabalho desnecessário em alguns casos, se tornaria um problema real sempre que o lado direito de uma expressão dependesse de uma condição que só é segura de avaliar quando o lado esquerdo já garantiu isso.

O curto-circuito resolve exatamente esse tipo de situação, tornando seguro escrever expressões em que o lado direito só faz sentido (ou só é seguro de calcular) quando o lado esquerdo já validou uma pré-condição necessária.

```java
int[] numeros = {1, 2, 3};
int indice = 5;

// sem curto-circuito, acessar numeros[indice] geraria erro (índice fora dos limites)
if (indice < numeros.length && numeros[indice] > 0) {
    System.out.println("Positivo");
} else {
    System.out.println("Condição não satisfeita, sem erro de índice");
}
```

Nesse exemplo, como `indice < numeros.length` já é `false` (`5` não é menor que `3`, o tamanho do array), o `&&` nem chega a avaliar `numeros[indice] > 0` — o que é essencial aqui, já que tentar acessar a posição `5` de um array de tamanho `3` geraria um erro em tempo de execução. Graças ao curto-circuito, essa segunda parte da expressão simplesmente nunca é executada quando a primeira já é suficiente para decidir o resultado.

Esse padrão — verificar primeiro uma condição de segurança, e só depois, à direita do `&&`, uma operação que depende dela ser verdadeira — é extremamente comum na prática, especialmente ao lidar com posições de arrays (como no exemplo) ou, em módulos futuros deste livro, com referências que podem não existir. Java também oferece versões não curto-circuito desses operadores (`&` e `|`, que sempre avaliam os dois lados), mas seu uso para lógica booleana é raro — `&&` e `||` são, de longe, a escolha padrão e recomendada para combinar condições no dia a dia.

## `if`

Com expressões booleanas já dominadas, este capítulo apresenta a estrutura de decisão mais básica e mais usada de Java: o `if`, que executa um bloco de código apenas quando uma condição booleana é verdadeira. É a estrutura de controle de fluxo mais simples possível, e serve de base conceitual para todas as variações mais elaboradas que vêm nos capítulos seguintes.

O capítulo mostra como escrever condições simples com `if`, como delimitar corretamente o bloco de código que deve ser executado (usando chaves, e o que acontece quando elas são omitidas), e como visualizar o fluxo de execução de um programa que contém uma decisão — ou seja, os diferentes caminhos que o programa pode seguir dependendo do valor da condição avaliada.

### Condições simples

Uma condição simples, no contexto do comando `if`, é uma única expressão booleana (como as vistas no capítulo anterior) que determina se um bloco específico de código deve ou não ser executado — o `if` avalia essa expressão, e executa o bloco associado apenas quando o resultado for `true`, ignorando-o completamente quando for `false`.

Sem uma estrutura como o `if`, um programa seguiria sempre a mesma sequência fixa de instruções, do início ao fim, sem nenhuma capacidade de reagir de forma diferente a situações diferentes — todo o código escrito até este módulo do livro é executado de forma linear e previsível, sem nenhum desvio condicional baseado em dados que só são conhecidos durante a execução.

O `if` resolve isso introduzindo a primeira estrutura de controle de fluxo deste livro: a possibilidade de tornar a execução de um trecho de código condicional a uma expressão booleana avaliada em tempo real, permitindo que o programa se comporte de forma diferente dependendo dos valores das suas variáveis naquele momento específico.

```java
int idade = 20;

if (idade >= 18) {
    System.out.println("Maior de idade");
}

System.out.println("Fim do programa");
```

Nesse exemplo, a mensagem "Maior de idade" só é impressa porque a condição `idade >= 18` é avaliada como `true` para o valor de `idade` usado — se `idade` fosse `15`, essa linha simplesmente seria ignorada, mas "Fim do programa" ainda seria impressa normalmente, já que ela está fora do bloco condicional do `if`.

A sintaxe básica do `if` sempre segue o mesmo formato: a palavra-chave `if`, seguida de uma expressão booleana entre parênteses, seguida do bloco de código (entre chaves) a ser executado quando essa expressão for `true`. Essa expressão entre parênteses precisa obrigatoriamente ser do tipo `boolean` — diferente de outras linguagens, Java não aceita números ou textos diretamente como condição de um `if`, reforçando a separação estrita entre `boolean` e outros tipos já discutida no capítulo anterior.

Condições simples são a base sobre a qual as estruturas mais elaboradas deste módulo — `else`, `else if`, e os diferentes formatos de `switch` — se apoiam, cada uma delas expandindo, de formas diferentes, essa ideia central de executar código de forma condicional a uma expressão booleana.

### Blocos

Um bloco, no contexto do `if` (e de estruturas de controle em geral, tema recorrente dos próximos módulos deste livro), é o conjunto de instruções delimitado por chaves `{ }` que é executado (ou não) dependendo do resultado da condição avaliada — é a região de código que o `if` efetivamente controla.

Sem blocos delimitados por chaves, ficaria ambíguo determinar exatamente até onde vai o efeito de uma condição: seria a próxima instrução apenas, ou várias instruções seguintes? Sem uma marcação clara de início e fim, cada linha adicional após um `if` poderia ser mal interpretada como parte ou não daquela condição.

Os blocos resolvem isso demarcando explicitamente, com `{` e `}`, exatamente quais instruções pertencem ao `if` — tudo o que está entre essas chaves só é executado quando a condição é verdadeira; tudo o que está fora delas segue seu curso normal, independentemente do resultado da condição.

```java
int idade = 15;

if (idade >= 18) {
    System.out.println("Maior de idade");
    System.out.println("Pode dirigir");
}
System.out.println("Este texto sempre aparece");
```

Nesse exemplo, as duas primeiras mensagens estão dentro do bloco do `if`, e só seriam impressas se `idade >= 18` fosse `true` — como não é o caso (`idade` é `15`), nenhuma das duas aparece. Já a terceira mensagem está fora do bloco, então é sempre impressa, independentemente do valor de `idade`.

Java permite, tecnicamente, omitir as chaves quando o bloco de um `if` contém apenas uma única instrução — nesse caso, apenas a linha imediatamente seguinte é considerada parte da condição:

```java
if (idade >= 18) System.out.println("Maior de idade"); // válido, sem chaves
```

Apesar de válida, essa forma sem chaves é considerada uma prática arriscada e geralmente desencorajada, mesmo entre programadores experientes: é fácil, ao editar o código depois, adicionar uma segunda linha logo abaixo supondo (erroneamente) que ela também faz parte da condição, quando na verdade só a primeira linha pertence ao `if`. Por essa razão, usar sempre as chaves — mesmo para um bloco de uma única instrução — é a convenção recomendada e seguida ao longo deste livro, priorizando clareza e prevenção de erros sobre a economia de duas linhas de código.

### Fluxo de execução

Fluxo de execução é o caminho real, instrução por instrução, que o programa efetivamente percorre durante uma execução específica. Como já vimos ao apresentar o `if`, sem uma estrutura condicional esse caminho seria sempre o mesmo, execução após execução, independentemente dos dados envolvidos; o `if` é justamente o que introduz a possibilidade desse caminho variar, dependendo dos valores das variáveis presentes na condição avaliada.

Olhando especificamente para o fluxo, o `if` funciona como um ponto de bifurcação: ao alcançá-lo, o programa avalia a condição e, dependendo do resultado, ou desvia para dentro do bloco associado (executando-o) e depois retoma o fluxo normal logo após o bloco, ou pula diretamente para depois do bloco, sem nunca ter executado seu conteúdo — as duas rotas se reencontram no mesmo lugar, mas passando por caminhos diferentes.

```java
int nota = 4;
System.out.println("Início da avaliação");

if (nota >= 6) {
    System.out.println("Aprovado");
}

System.out.println("Avaliação encerrada");
```

Nesse exemplo, com `nota` igual a `4`, o fluxo de execução visita "Início da avaliação", em seguida avalia a condição do `if` (que resulta em `false`, já que `4` não é maior ou igual a `6`), pula todo o bloco associado sem executá-lo, e segue direto para "Avaliação encerrada" — o programa nunca "vê" a linha que imprimiria "Aprovado" nessa execução específica.

É importante entender que o fluxo de execução, nesse caso, é decidido em tempo real, durante a execução do programa, com base no valor concreto de `nota` naquele momento — o mesmo trecho de código, executado outra vez com `nota` igual a `8`, seguiria um caminho ligeiramente diferente, agora passando também pela linha "Aprovado" antes de chegar a "Avaliação encerrada".

Visualizar o fluxo de execução como um caminho que se bifurca a cada condição — seguindo por um ramo ou por outro, dependendo do resultado avaliado — é uma habilidade fundamental para entender corretamente o comportamento de qualquer programa a partir daqui, e só ganha importância nos próximos capítulos deste módulo, conforme novas bifurcações forem se somando a essa primeira.

## `if`, `else` e `else if`

O `if` isolado, visto no capítulo anterior, só cobre um caminho: o que fazer quando a condição é verdadeira, deixando implícito "não fazer nada" quando é falsa. Este capítulo completa essa estrutura mostrando como lidar com o caminho alternativo e com decisões que envolvem mais de duas possibilidades, algo extremamente comum em programas reais.

O capítulo apresenta o `else` (o que fazer quando a condição do `if` é falsa), o encadeamento com `else if` para representar decisões com várias alternativas mutuamente exclusivas, condições aninhadas (um `if` dentro de outro) e boas práticas de organização de condições, para evitar blocos de decisão profundamente aninhados e difíceis de ler. É o tipo de estrutura que aparece o tempo todo em código real, sempre que há mais de dois caminhos possíveis a considerar.

### Decisões múltiplas

Decisões múltiplas são situações em que existem duas ou mais alternativas de comportamento, mutuamente exclusivas, dependendo do resultado de uma ou mais condições — diferente do `if` simples (capítulo anterior), que só decide entre "executar" ou "não executar" um único bloco, decisões múltiplas envolvem escolher entre caminhos alternativos, cada um associado a uma situação diferente.

Sem uma forma dedicada de expressar essas alternativas, seria necessário simular o comportamento de "senão" usando apenas `if`s simples e independentes, negando manualmente a condição anterior em cada `if` seguinte — uma abordagem que funciona, mas é repetitiva e mais propensa a erros, já que a relação de exclusividade entre os blocos fica implícita, dependendo inteiramente de o programador negar corretamente cada condição anterior.

A cláusula `else`, associada a um `if`, resolve isso oferecendo um segundo bloco, executado exatamente quando a condição do `if` é `false` — as duas alternativas juntas cobrem todas as possibilidades, e apenas uma delas é executada em cada avaliação, nunca as duas.

```java
int idade = 15;

if (idade >= 18) {
    System.out.println("Maior de idade");
} else {
    System.out.println("Menor de idade");
}
```

Nesse exemplo, exatamente uma das duas mensagens é impressa para qualquer valor de `idade`: se a condição do `if` for `true`, o bloco do `if` executa e o do `else` é ignorado; se for `false`, o inverso acontece — o `else` cobre automaticamente todos os casos em que a condição não é satisfeita, sem que seja necessário escrever essa negação explicitamente.

Essa relação de exclusividade mútua é a principal vantagem do `else` sobre dois `if`s independentes: com `if`/`else`, a linguagem garante estruturalmente que os dois blocos nunca executam juntos na mesma avaliação, enquanto dois `if`s separados, tecnicamente, poderiam ambos ser verdadeiros ao mesmo tempo (caso as condições não sejam escritas como exatamente opostas uma da outra), levando a comportamentos não previstos.

Quando existem mais de duas alternativas possíveis, a combinação `else if` estende essa mesma ideia para permitir uma cadeia de condições, cada uma testada apenas se todas as anteriores já tiverem sido descartadas — um recurso que aparece com frequência crescente nos exemplos deste capítulo, à medida que os cenários avaliados ganham mais alternativas.

### Condições aninhadas

Condições aninhadas são estruturas `if` (com ou sem `else`) declaradas dentro do bloco de outra estrutura condicional, formando uma hierarquia de decisões em que uma condição só chega a ser avaliada se uma condição mais externa já tiver sido satisfeita antes. É uma forma de expressar decisões que dependem, em sequência, de mais de um critério relacionado.

Sem a possibilidade de aninhar condições, seria necessário combinar todos os critérios relevantes em uma única expressão booleana enorme, usando `&&` e `||` (vistos no capítulo anterior) para reunir tudo de uma vez — o que, para decisões com vários níveis de dependência entre si, tende a resultar em expressões difíceis de ler e de revisar corretamente.

O aninhamento resolve isso permitindo quebrar uma decisão complexa em etapas sucessivas, cada uma verificando um critério específico apenas depois que os critérios anteriores, mais externos, já tiverem sido confirmados — deixando claro, pela própria estrutura visual do código (a indentação), quais condições dependem de quais.

```java
int idade = 20;
boolean temCarteira = true;

if (idade >= 18) {
    if (temCarteira) {
        System.out.println("Pode dirigir sozinho");
    } else {
        System.out.println("Maior de idade, mas sem carteira");
    }
} else {
    System.out.println("Menor de idade, não pode dirigir");
}
```

Nesse exemplo, a verificação de `temCarteira` só acontece se `idade >= 18` já tiver sido confirmada como verdadeira — ela está aninhada dentro do bloco do primeiro `if`, o que reflete corretamente a ideia de que ter carteira só é relevante depois de já se saber que a pessoa é maior de idade.

Uma comparação útil é com o mesmo cenário resolvido usando apenas `&&`, sem aninhamento: `if (idade >= 18 && temCarteira)` — funcional para esse caso simples, mas incapaz de expressar, como o aninhamento consegue, uma resposta diferenciada para a situação intermediária ("maior de idade, mas sem carteira"), já que o `&&` sozinho só distingue entre "as duas condições são verdadeiras" e "não são".

O aninhamento excessivo, no entanto, tem um custo prático: cada nível adicional de `if` dentro de `if` aumenta a indentação do código e a carga mental necessária para acompanhar em qual ramo da lógica se está, a cada momento — um cuidado retomado no próximo conceito, sobre como organizar condições para manter esse tipo de estrutura legível mesmo quando os critérios se multiplicam.

### Organização de condições

Organização de condições é o cuidado de estruturar blocos `if`/`else`/`else if`, incluindo os aninhados (conceito anterior), de forma que permaneçam legíveis e fáceis de acompanhar mesmo quando o número de critérios avaliados cresce — não é um recurso novo da linguagem, mas um conjunto de práticas que se apoia em tudo o que já foi visto neste capítulo.

Decisões que começam simples, com um ou dois critérios, costumam crescer ao longo do tempo — à medida que novos requisitos são adicionados ao programa — sem que ninguém decida deliberadamente complicar o código; quando esse crescimento não é acompanhado de nenhum cuidado extra, o resultado é uma estrutura profundamente aninhada e difícil de seguir, em que é preciso rastrear vários níveis de indentação simultaneamente só para entender sob quais condições um trecho específico de código é alcançado.

Um conjunto de práticas ajuda a manter esse crescimento sob controle: preferir `else if` encadeado a `if`s aninhados sempre que as condições forem realmente alternativas mutuamente exclusivas (em vez de dependências reais entre critérios); extrair condições compostas complexas para uma variável `boolean` nomeada de forma descritiva, tornando a intenção explícita antes mesmo de olhar para dentro do `if`; e, quando um aninhamento fica com muitos níveis, considerar se parte da lógica não deveria ser isolada em um método próprio, retomando a separação de responsabilidades já discutida no módulo anterior.

```java
int idade = 25;
double renda = 3000.0;
boolean temHistoricoLimpo = true;

// menos legível: condição composta direto no if
if (idade >= 18 && renda >= 2000.0 && temHistoricoLimpo) {
    System.out.println("Crédito aprovado");
}

// mais legível: condição nomeada explica a intenção
boolean elegivelParaCredito = idade >= 18 && renda >= 2000.0 && temHistoricoLimpo;
if (elegivelParaCredito) {
    System.out.println("Crédito aprovado");
}
```

Nesse exemplo, as duas versões produzem exatamente o mesmo comportamento, mas a segunda deixa explícito, através do nome `elegivelParaCredito`, o que aquela combinação de critérios representa — uma vantagem que cresce proporcionalmente à complexidade da condição, já que quem lê o `if` não precisa decifrar a expressão completa para entender a que ela se refere.

Da mesma forma, uma cadeia de `if`/`else if`/`else if`/`else` costuma comunicar melhor a ideia de "escolher entre várias alternativas relacionadas" do que uma sequência de `if`s aninhados um dentro do outro representando as mesmas alternativas — reservar o aninhamento para os casos em que existe, de fato, uma dependência real entre critérios (como no exemplo do conceito anterior, em que verificar a carteira só faz sentido depois de confirmar a maioridade) tende a manter o código mais alinhado à lógica que ele pretende representar.

## Switch tradicional

Quando uma decisão envolve comparar uma mesma variável contra vários valores possíveis, uma longa cadeia de `else if` pode se tornar repetitiva e difícil de ler. Este capítulo apresenta uma alternativa pensada exatamente para esse cenário: o `switch`, uma estrutura que compara um valor contra vários casos possíveis de forma mais organizada.

O capítulo cobre a sintaxe tradicional do `switch` — a palavra-chave em si, os blocos `case` que representam cada valor possível, a necessidade do `break` para evitar que a execução "caia" para o próximo caso (o chamado fall-through) e o bloco `default` para tratar valores não previstos nos casos anteriores. Essa forma tradicional, mais verbosa e com armadilhas conhecidas como o fall-through acidental, é a base histórica da linguagem; o capítulo seguinte mostra como a forma moderna de `switch` resolve boa parte dessas armadilhas.

### `switch`

`switch` é uma estrutura de controle de fluxo alternativa ao `if`/`else if`, especializada em testar uma única variável (ou expressão) contra uma lista de valores possíveis específicos, executando o bloco correspondente ao valor que efetivamente corresponder. É especialmente útil quando existem várias alternativas distintas, todas comparadas contra a mesma variável, um cenário em que uma longa cadeia de `else if` tenderia a ficar repetitiva.

Sem uma estrutura como o `switch`, testar uma mesma variável contra várias possibilidades diferentes exigiria uma cadeia de `else if`, repetindo o nome da variável e o operador de comparação (`==`) em cada uma das condições — funcional, mas visualmente repetitivo quando o número de alternativas cresce, já que a única coisa que realmente muda entre elas é o valor comparado.

O `switch` resolve isso concentrando a variável (ou expressão) testada em um único lugar, no topo da estrutura, e listando depois, de forma mais enxuta, apenas os valores possíveis e o bloco de código associado a cada um — dispensando a repetição da comparação completa a cada alternativa.

```java
int diaDaSemana = 3;
String nome;

switch (diaDaSemana) {
    case 1:
        nome = "Domingo";
        break;
    case 2:
        nome = "Segunda";
        break;
    case 3:
        nome = "Terça";
        break;
    default:
        nome = "Dia inválido";
        break;
}

System.out.println(nome); // imprime "Terça"
```

Nesse exemplo, `diaDaSemana` é avaliado uma única vez, no topo do `switch`, e comparado, em sequência, contra os valores `1`, `2` e `3`, cada um associado a um bloco (`case`) próprio — quando `diaDaSemana` é `3`, o bloco correspondente ao `case 3` é executado, atribuindo `"Terça"` a `nome`.

Os elementos internos dessa estrutura — `case`, `break` e `default` — são detalhados individualmente nos próximos três conceitos deste capítulo, assim como o comportamento de fall-through, uma particularidade importante do `switch` tradicional que precisa ser bem compreendida antes de usá-lo com segurança.

Uma limitação importante do `switch` tradicional é que ele só aceita testar a variável contra valores constantes específicos (como números inteiros, caracteres, `String`s e alguns outros tipos), nunca contra faixas de valores ou condições mais elaboradas (como `idade >= 18`) — para esses casos, `if`/`else if` continua sendo a estrutura apropriada, já que o `switch` foi projetado especificamente para comparações de igualdade contra valores fixos e conhecidos de antemão.

### `case`

`case` é a palavra-chave usada dentro de um `switch` (conceito anterior) para declarar cada um dos valores possíveis contra os quais a variável testada pode corresponder, seguida do bloco de código a ser executado quando essa correspondência específica ocorrer. Cada `switch` pode conter quantos `case`s forem necessários, um para cada valor distinto que precisa de um tratamento próprio.

Sem uma forma de declarar múltiplas alternativas dentro da mesma estrutura, o `switch` não teria como associar valores diferentes a comportamentos diferentes — seria apenas uma comparação única, sem a capacidade de ramificar entre várias possibilidades distintas testadas contra a mesma variável.

O `case` resolve isso funcionando como um rótulo dentro do `switch`: a execução "salta" diretamente para o `case` cujo valor corresponde ao valor da variável testada, ignorando os `case`s anteriores, e começa a executar o código a partir dali.

```java
char nota = 'B';

switch (nota) {
    case 'A':
        System.out.println("Excelente");
        break;
    case 'B':
        System.out.println("Bom");
        break;
    case 'C':
        System.out.println("Regular");
        break;
}
```

Nesse exemplo, existem três `case`s diferentes, um para cada letra de nota possível — como `nota` vale `'B'`, a execução salta diretamente para `case 'B'`, ignorando `case 'A'` (que aparece antes) e nunca avaliando `case 'C'` (que aparece depois), imprimindo "Bom".

O valor associado a cada `case` precisa ser um valor constante, conhecido em tempo de compilação — não pode ser uma variável, nem uma expressão que dependa de cálculos feitos durante a execução do programa, uma restrição que reforça o propósito do `switch` como uma estrutura voltada especificamente para comparações contra valores fixos e previamente conhecidos.

É perfeitamente possível que o `switch` termine sua execução sem que nenhum `case` corresponda ao valor testado — nesse caso, se existir um `default` (tema de um conceito adiante neste capítulo), ele é executado; se não existir nenhum `default`, o `switch` inteiro simplesmente termina sem executar nenhum bloco, e a execução do programa segue normalmente para depois da estrutura.

### `break`

`break`, no contexto de um `switch` tradicional, é a instrução usada ao final de cada `case` para interromper a execução da estrutura naquele ponto, impedindo que o fluxo continue avançando para os `case`s seguintes.

Sem uma forma de interromper a execução ao final de um `case`, esse avanço automático pelos `case`s seguintes — o fall-through, detalhado no último conceito deste capítulo — tornaria praticamente todo `switch` sujeito a executar mais código do que o pretendido, a menos que a continuação fosse explicitamente interrompida em cada bloco.

O `break` resolve isso oferecendo um ponto de saída explícito: ao ser alcançado, ele encerra imediatamente a execução do `switch` inteiro, e o fluxo do programa segue para a primeira instrução logo depois da estrutura, sem visitar nenhum dos `case`s restantes.

```java
int mes = 2;
String estacao;

switch (mes) {
    case 12:
    case 1:
    case 2:
        estacao = "Verão";
        break; // impede que a execução avance para o case 3
    case 3:
    case 4:
    case 5:
        estacao = "Outono";
        break;
    default:
        estacao = "Desconhecida";
        break;
}

System.out.println(estacao); // imprime "Verão"
```

Nesse exemplo, o `break` ao final do bloco de `"Verão"` garante que, uma vez atribuído o valor correto a `estacao`, a execução do `switch` pare ali — sem esse `break`, o programa continuaria "caindo" para o bloco seguinte (`case 3`, `4`, `5`), sobrescrevendo `estacao` para `"Outono"` de forma indevida, mesmo o mês sendo de verão.

Vale notar também, nesse exemplo, que vários `case`s podem ser empilhados sem código entre eles (`case 12:`, `case 1:`, `case 2:` seguidos direto do mesmo bloco) — isso é uma forma intencional e válida de associar múltiplos valores ao mesmo tratamento, diferente do fall-through acidental que acontece quando um `break` é simplesmente esquecido, tema aprofundado no último conceito deste capítulo.

Por essa importância em delimitar corretamente onde cada `case` termina, esquecer um `break` é um dos erros mais comuns e menos óbvios ao escrever um `switch` tradicional — o programa continua compilando e rodando normalmente, mas produz um resultado inesperado, silenciosamente incorreto, sem nenhum erro que aponte diretamente para a causa.

### `default`

Assim como o `else` funciona como alternativa "coringa" ao final de uma cadeia `if`/`else if` (visto no capítulo anterior), o `default` cumpre esse mesmo papel dentro de um `switch` tradicional: é o rótulo especial que agrupa o código a ser executado quando o valor testado não corresponde a nenhum dos `case`s declarados explicitamente.

Sem um `default`, um `switch` que não encontrasse nenhum `case` correspondente simplesmente terminaria sua execução sem fazer nada, o que nem sempre é o comportamento desejado — muitas vezes é importante tratar explicitamente a possibilidade de um valor inesperado, seja registrando um erro, seja aplicando algum comportamento padrão razoável para essa situação.

O `default` resolve isso oferecendo um ponto de tratamento garantido para qualquer valor que não se encaixe em nenhum dos `case`s previstos, tornando explícita a decisão sobre o que fazer nesses casos, em vez de deixar esse cenário simplesmente ser ignorado silenciosamente.

```java
int diaDaSemana = 9; // valor inválido, fora do intervalo esperado (1 a 7)
String nome;

switch (diaDaSemana) {
    case 1:
        nome = "Domingo";
        break;
    case 2:
        nome = "Segunda";
        break;
    default:
        nome = "Dia inválido";
        break;
}

System.out.println(nome); // imprime "Dia inválido"
```

Nesse exemplo, como `9` não corresponde a nenhum dos `case`s declarados, a execução cai diretamente no `default`, atribuindo `"Dia inválido"` a `nome` — sem esse `default`, `nome` simplesmente não seria atribuído em nenhum ramo, o que geraria inclusive um erro de compilação nesse exemplo específico, já que Java exige que toda variável seja definitivamente atribuída antes de ser usada.

Diferente do que o nome poderia sugerir, o `default` não precisa obrigatoriamente aparecer no final do `switch` — ele pode, tecnicamente, ser posicionado em qualquer lugar entre os `case`s —, mas a convenção quase universal, seguida também neste livro, é sempre colocá-lo por último, já que essa posição reflete melhor seu papel de "alternativa restante", tornando o código mais previsível para quem o lê.

Assim como qualquer outro bloco de `case`, o `default` também deve terminar com `break` (ou ser, de fato, o último bloco do `switch`) para evitar o comportamento de fall-through descrito no próximo conceito, especialmente relevante quando o `default`, por algum motivo, não é o último rótulo declarado na estrutura.

### Fall-through

Fall-through é o comportamento padrão do `switch` tradicional de continuar executando os blocos de código dos `case`s seguintes, em sequência, após encontrar o `case` correspondente, até encontrar um `break` (visto anteriormente) ou até alcançar o fim da estrutura — mesmo que esses `case`s seguintes não correspondam ao valor originalmente testado.

Sem entender esse comportamento, é fácil escrever um `switch` supondo, erroneamente, que a execução de um `case` termina automaticamente assim que aquele bloco específico é concluído — uma suposição razoável para quem vem de estruturas como o `if`, mas que não se aplica ao `switch` tradicional, cujo comportamento padrão é justamente o oposto: continuar "caindo" pelos `case`s seguintes, a menos que isso seja interrompido explicitamente.

O `break`, já discutido, é a ferramenta que existe justamente para controlar esse comportamento — mas entender o fall-through em si, mesmo quando ele é sempre evitado com `break`s bem posicionados, é essencial para prever corretamente o que acontece nos casos em que um `break` é esquecido por engano, e também para reconhecer os casos (mais raros, mas válidos) em que o fall-through é usado de forma intencional.

```java
int nivel = 2;

switch (nivel) {
    case 1:
        System.out.println("Nível básico");
        // sem break: cai para o case seguinte de propósito
    case 2:
        System.out.println("Nível intermediário");
        // sem break: cai para o case seguinte de propósito
    case 3:
        System.out.println("Nível avançado");
        break;
    default:
        System.out.println("Nível desconhecido");
}
```

Nesse exemplo, com `nivel` igual a `2`, a execução começa em `case 2`, imprime "Nível intermediário", e — como não há `break` ali — continua "caindo" para `case 3`, imprimindo também "Nível avançado", antes de finalmente encontrar o `break` e encerrar. O resultado são duas linhas impressas, não apenas uma, um comportamento que só faz sentido ao entender o fall-through como o padrão da estrutura.

O uso intencional do fall-through — como visto também no exemplo do conceito de `break`, ao agrupar vários `case`s para o mesmo bloco de código — costuma ser considerado aceitável quando a intenção fica clara pelo contexto (geralmente através de `case`s vazios empilhados). Já o fall-through causado por um `break` esquecido por descuido é uma das fontes mais comuns de bugs sutis em código Java que usa `switch` tradicional — um dos motivos que motivou a criação das switch expressions, tema do próximo e último capítulo deste módulo, que eliminam esse comportamento por padrão.

## Switch expressions

O capítulo anterior mostrou o `switch` tradicional e uma de suas armadilhas mais conhecidas: esquecer o `break` e deixar a execução "cair" acidentalmente para o caso seguinte. Este capítulo, que fecha o módulo, apresenta a evolução moderna dessa estrutura: as switch expressions, introduzidas para tornar o `switch` mais seguro e, além disso, capaz de produzir diretamente um valor, funcionando como uma expressão e não apenas como um comando.

O capítulo mostra a nova sintaxe com seta (`case ->`), que dispensa o `break` e elimina o fall-through por padrão, como escrever expressões `switch` que retornam um valor a ser usado diretamente (por exemplo, atribuído a uma variável), o uso da palavra-chave `yield` quando um bloco mais complexo precisa produzir esse valor, e como isso simplifica cenários em que, na forma tradicional, seria preciso declarar uma variável fora do `switch` só para receber um resultado calculado dentro dele.

### `case ->`

`case ->` é a sintaxe usada nas switch expressions (a forma moderna do `switch`, introduzida em versões mais recentes de Java) para associar um valor testado diretamente ao código a ser executado, usando uma seta (`->`) no lugar dos dois-pontos (`:`) do `switch` tradicional — e, com essa mudança de sintaxe, eliminando por completo o comportamento de fall-through visto no capítulo anterior.

Sem essa nova sintaxe, todo `switch` em Java carregaria consigo o risco de fall-through acidental discutido no capítulo anterior, exigindo disciplina constante para lembrar de um `break` ao final de cada `case` — um requisito fácil de esquecer, especialmente para quem está começando, e responsável por uma categoria inteira de bugs sutis.

O `case ->` resolve isso mudando a regra padrão: cada `case` associado com seta executa apenas o código imediatamente associado a ele, e a execução do `switch` termina automaticamente ali, sem nenhuma possibilidade de "cair" para o `case` seguinte — não existe `break` nas switch expressions porque, simplesmente, não é mais necessário.

```java
int diaDaSemana = 3;
String nome;

switch (diaDaSemana) {
    case 1 -> nome = "Domingo";
    case 2 -> nome = "Segunda";
    case 3 -> nome = "Terça";
    default -> nome = "Dia inválido";
}

System.out.println(nome); // imprime "Terça"
```

Nesse exemplo, equivalente em resultado ao exemplo visto no conceito de `switch` do capítulo anterior, cada `case` executa apenas a instrução à direita da seta, sem qualquer risco de continuar executando o `case` seguinte — o fall-through simplesmente não existe mais nessa sintaxe, tornando cada `case` uma alternativa verdadeiramente isolada das demais.

Também é possível associar múltiplos valores a um único `case ->`, separados por vírgula, substituindo de forma mais direta o padrão de "case vazio empilhado" usado intencionalmente no `switch` tradicional:

```java
switch (mes) {
    case 12, 1, 2 -> System.out.println("Verão");
    case 3, 4, 5 -> System.out.println("Outono");
    default -> System.out.println("Outra estação");
}
```

Essa forma de listar vários valores no mesmo `case`, separados por vírgula, é mais direta e menos propensa a erro do que empilhar vários `case`s vazios um após o outro, como era necessário na sintaxe tradicional — outra vantagem prática trazida junto com essa nova notação.

### Expressões `switch`

Expressões `switch` são a capacidade das switch expressions de serem usadas não apenas como uma estrutura de controle de fluxo (executando blocos de código condicionalmente, como o `switch` tradicional), mas também como uma expressão que produz diretamente um valor, podendo ser atribuída a uma variável ou usada dentro de outra expressão maior — uma diferença fundamental em relação ao `switch` tradicional, que nunca produz um valor por si só.

Sem essa capacidade, mesmo usando a sintaxe `case ->` do conceito anterior, ainda seria necessário declarar uma variável antes do `switch` e atribuir um valor a ela dentro de cada `case`, como visto nos exemplos anteriores — um padrão funcional, mas que separa desnecessariamente a declaração da variável do próprio cálculo do seu valor.

As expressões `switch` resolvem isso permitindo que o `switch` inteiro apareça diretamente do lado direito de uma atribuição (ou em qualquer outro lugar onde um valor seja esperado), com cada `case` fornecendo o valor correspondente àquela alternativa, e o resultado de todo o `switch` sendo exatamente o valor do `case` que correspondeu.

```java
int diaDaSemana = 3;

String nome = switch (diaDaSemana) {
    case 1 -> "Domingo";
    case 2 -> "Segunda";
    case 3 -> "Terça";
    default -> "Dia inválido";
};

System.out.println(nome); // imprime "Terça"
```

Nesse exemplo, `nome` é declarado e atribuído em uma única instrução, com o `switch` inteiro funcionando como uma expressão que produz diretamente o valor correspondente ao `case` que casou com `diaDaSemana` — repare que, diferente do `switch` tradicional, não há mais uma atribuição repetida (`nome = ...`) dentro de cada `case`; cada `case` simplesmente fornece o valor, e é o próprio `switch`, como um todo, que é atribuído a `nome` no final, com um `;` fechando a expressão inteira.

Essa forma também tende a ser mais segura, além de mais concisa: como o `switch` precisa produzir um valor para a atribuição funcionar, o compilador passa a exigir que essa produção seja garantida para qualquer entrada possível — uma exigência (a exaustividade) que o conceito de retorno de valores, mais adiante neste capítulo, explica em detalhe.

Quando o valor de cada `case` não pode ser expresso em uma única instrução simples (exigindo, por exemplo, mais de uma linha de lógica antes de determinar o valor final), é necessário um bloco com chaves e a palavra-chave `yield`, tema do próximo conceito deste capítulo.

### `yield`

`yield` é a palavra-chave usada dentro de um `case` de switch expression (vistas nos dois conceitos anteriores) quando o valor daquele `case` precisa ser calculado através de um bloco de código com mais de uma instrução, em vez de uma única expressão simples logo após a seta `->` — o `yield` indica exatamente qual valor, calculado dentro desse bloco, deve ser o resultado daquele `case` específico.

Sem o `yield`, a sintaxe `case ->` (vista no primeiro conceito deste capítulo) ficaria limitada a `case`s cujo valor pudesse ser expresso em uma única instrução direta — qualquer lógica que exigisse passos intermediários, como uma variável auxiliar ou um cálculo em múltiplas etapas, não teria como indicar claramente qual valor final deveria ser "devolvido" pelo `case` como um todo.

O `yield` resolve isso permitindo que um `case` seja associado a um bloco de código entre chaves (em vez de uma única expressão), dentro do qual qualquer lógica necessária pode ser executada, encerrando com um `yield valor;` que determina explicitamente qual será o resultado daquele `case` para a expressão `switch` inteira.

```java
int nota = 85;

String conceito = switch (nota / 10) {
    case 10, 9 -> "A";
    case 8 -> "B";
    case 7 -> {
        System.out.println("Nota na faixa de aprovação mínima");
        yield "C"; // valor final deste case, após a lógica intermediária
    }
    default -> "Reprovado";
};

System.out.println(conceito); // imprime "B"
```

Nesse exemplo, o `case 7` precisa de uma instrução extra (uma mensagem informativa) antes de determinar seu valor final, o que exige um bloco entre chaves — e é o `yield "C"` que informa ao compilador qual valor esse `case`, como um todo, deve produzir para a expressão `switch`, mesmo tendo executado mais de uma instrução internamente. Como `nota / 10` resulta em `8` nesse caso concreto, é o `case 8` que efetivamente executa, produzindo `"B"` diretamente, já que esse `case` usa a forma simples com seta, sem precisar de bloco nem de `yield`.

É importante não confundir `yield` com `return`: `yield` devolve um valor especificamente para a expressão `switch` que o contém, sem encerrar o método inteiro em que esse `switch` está inserido — depois do `yield`, a execução continua normalmente a partir de onde a expressão `switch` foi usada, diferente do `return`, que encerra todo o método imediatamente.

Um `case` com seta simples (`case 8 -> "B"`) e um `case` com bloco e `yield` (`case 7 -> { ...; yield "C"; }`) podem coexistir livremente dentro da mesma expressão `switch`, como visto no exemplo — a escolha entre uma forma e outra depende exclusivamente de o valor daquele `case` específico exigir, ou não, alguma lógica intermediária antes de ser determinado.

### Retorno de valores

Retorno de valores, no contexto das switch expressions, é a garantia, verificada pelo próprio compilador, de que toda expressão `switch` usada como valor (conceito de "expressões `switch`", visto anteriormente neste capítulo) produz obrigatoriamente um resultado para qualquer entrada possível — diferente do `switch` tradicional, em que era perfeitamente possível uma variável ficar sem valor atribuído se nenhum `case` correspondesse e não houvesse `default`.

Sem essa garantia, usar um `switch` diretamente como expressão (atribuindo seu resultado a uma variável, como visto anteriormente) seria arriscado: o que aconteceria se, em tempo de execução, nenhum `case` correspondesse ao valor testado, e não houvesse um `default` para cobrir essa lacuna? A variável de destino ficaria sem um valor definido, uma situação que o compilador precisa poder descartar com segurança antes mesmo do programa rodar.

Java resolve isso exigindo, em tempo de compilação, que toda expressão `switch` seja exaustiva — ou seja, que cubra todos os valores possíveis da variável testada. Para a maioria dos tipos, isso significa exigir a presença de um `default`, garantindo que sempre exista um valor de resultado, mesmo para entradas não previstas explicitamente pelos demais `case`s.

```java
int nota = 6;

String faixa = switch (nota) {
    case 10, 9, 8 -> "Alta";
    case 7, 6 -> "Média";
    case 5, 4 -> "Baixa";
    default -> "Fora da faixa esperada";
};

System.out.println(faixa); // imprime "Média"

// o código abaixo não compilaria, por não ser exaustivo:
// String faixaIncompleta = switch (nota) {
//     case 10, 9, 8 -> "Alta";
//     case 7, 6 -> "Média";
// }; // erro: switch expression não coberto para todos os valores possíveis de int
```

Nesse exemplo, o primeiro `switch` compila normalmente porque inclui um `default`, cobrindo qualquer valor de `nota` não contemplado explicitamente pelos demais `case`s. Já a versão comentada, sem `default`, não compilaria, porque o compilador não consegue garantir que `faixaIncompleta` sempre receberá algum valor — existem infinitos valores possíveis de `int` (como `100`, ou `-5`) que não correspondem a nenhum dos `case`s listados.

Essa exigência de exaustividade é uma das vantagens práticas mais importantes das switch expressions sobre o `switch` tradicional: um erro que, na forma antiga, só seria percebido durante a execução (uma variável nunca atribuída, gerando comportamento incorreto silencioso ou um erro de compilação em contextos específicos) é antecipado para o momento da compilação, de forma consistente, sempre que o `switch` for usado como expressão que produz um valor — reforçando, mais uma vez, o benefício de detectar problemas o mais cedo possível no ciclo de desenvolvimento, um tema recorrente ao longo de todo este livro.

# Módulo 5 — Repetições

Depois de aprender a tomar decisões, este módulo apresenta o outro grande mecanismo de controle de fluxo: repetir um bloco de código várias vezes sem precisar copiá-lo manualmente. Repetições (ou loops) são indispensáveis para tarefas como processar uma lista de itens, repetir uma operação até uma condição ser satisfeita, ou validar uma entrada até que o usuário forneça um valor aceitável — situações extremamente comuns em qualquer programa real.

O módulo apresenta as três estruturas de repetição de Java — `while`, `do while` e `for` — explicando quando cada uma é mais adequada, seguido de um capítulo sobre como controlar o comportamento de um loop em execução (interrompendo-o antecipadamente ou pulando uma iteração) e um capítulo final que aplica tudo isso a problemas clássicos de repetição, como contagens, somatórios, buscas e validações. Ao final deste módulo, o leitor será capaz de automatizar tarefas repetitivas escolhendo a estrutura de loop mais apropriada para cada situação, evitando armadilhas comuns como loops infinitos.

## while

Este capítulo abre o módulo de repetições com a estrutura de loop mais simples de entender conceitualmente: o `while`, que repete um bloco de código enquanto uma condição booleana permanecer verdadeira — a mesma ideia de condição já usada no `if`, só que agora reavaliada a cada repetição, e não apenas uma vez.

O capítulo mostra como essa condição controla quando o loop para, como usar contadores (variáveis que mudam a cada repetição, geralmente para controlar quantas vezes o loop roda) e acumuladores (variáveis que vão somando ou combinando valores ao longo das repetições), e alerta para um erro comum entre iniciantes: o loop infinito, que ocorre quando a condição nunca se torna falsa, geralmente porque o programador esqueceu de atualizar a variável que a condição depende.

### Condição

O `while` é a estrutura de repetição mais simples de Java: ele executa um bloco de código repetidamente enquanto uma condição booleana permanecer verdadeira. A condição é escrita entre parênteses logo após a palavra-chave `while`, e é avaliada antes de cada execução do bloco — inclusive antes da primeira. Se, na primeira avaliação, a condição já for falsa, o bloco nunca chega a executar nenhuma vez.

Antes de existir uma estrutura dedicada a isso, repetir um trecho de código um número de vezes que só se sabe em tempo de execução (por exemplo, "enquanto o usuário não digitar 'sair'") não tinha solução direta — seria necessário duplicar manualmente o mesmo bloco de código várias vezes, o que só funciona quando o número de repetições é conhecido antecipadamente, e mesmo assim de forma trabalhosa e propensa a erros.

É exatamente isso que o `while` oferece: a repetição continua por quantas vezes forem necessárias, sem que o programador precise saber esse número de antemão — quem decide quando parar é a própria condição, reavaliada a cada volta do laço (cada repetição costuma ser chamada de "iteração").

```java
int senha = -1;

while (senha != 1234) {
    System.out.println("Digite a senha:");
    senha = new java.util.Scanner(System.in).nextInt();
}

System.out.println("Acesso liberado!");
```

Nesse exemplo, o bloco dentro do `while` continua pedindo a senha repetidamente enquanto o valor digitado for diferente de `1234`. Não há como saber, ao escrever o código, quantas vezes o usuário vai errar — a condição é o único critério de parada, avaliada a cada nova tentativa.

É essencial que algo dentro do bloco, em algum momento, tenha potencial de tornar a condição falsa — no exemplo, é a leitura de um novo valor para `senha` a cada iteração. Sem isso, a condição nunca muda, e o laço se repete indefinidamente (essa situação, os loops infinitos, é tratada em um conceito à parte adiante neste capítulo).

O `while` costuma ser a escolha mais natural quando o número de repetições depende de uma condição externa e imprevisível — entrada do usuário, leitura de um arquivo até seu fim, ou espera por uma resposta de rede — diferente de situações em que se sabe, de antemão, exatamente quantas vezes repetir, caso em que o `for` (visto em um capítulo posterior) costuma ser mais direto.

### Contadores

Um contador é uma variável usada dentro de um laço de repetição para registrar quantas vezes algo aconteceu — tipicamente quantas iterações já foram executadas, ou quantas vezes uma condição específica se verificou dentro do laço. Diferente da condição do `while` (vista no conceito anterior), que decide se o laço continua, o contador é apenas uma variável comum, declarada antes do laço e alterada dentro dele, sem nenhuma sintaxe especial associada.

Sem um contador, um laço `while` sabe apenas repetir enquanto sua condição for verdadeira, mas não tem, por si só, nenhuma noção de "quantas vezes já rodei" ou "quantos itens já processei" — se essa informação for necessária depois do laço (por exemplo, para exibir "você teve 3 tentativas erradas"), não existe como recuperá-la sem guardá-la em algum lugar ao longo do caminho.

A solução é simples: declarar uma variável antes do `while` com um valor inicial (geralmente `0`), e incrementá-la a cada iteração em que o evento de interesse ocorre. Essa variável pode, inclusive, participar da própria condição do `while`, quando o objetivo for repetir um número fixo de vezes.

```java
int tentativas = 0;
int senha = -1;

while (senha != 1234 && tentativas < 3) {
    System.out.println("Digite a senha:");
    senha = new java.util.Scanner(System.in).nextInt();
    tentativas++;
}

if (senha == 1234) {
    System.out.println("Acesso liberado!");
} else {
    System.out.println("Número de tentativas excedido.");
}
```

Aqui, `tentativas` começa em `0` e é incrementada a cada volta do laço, com `tentativas++` (equivalente a `tentativas = tentativas + 1`). Como `tentativas` também aparece na condição do `while`, ela passa a limitar o número de repetições — depois de três tentativas erradas, a condição `tentativas < 3` se torna falsa e o laço para, mesmo que a senha ainda esteja incorreta.

É importante que o incremento do contador esteja dentro do bloco do laço, e em um ponto que realmente será alcançado em toda iteração — um contador que só é incrementado dentro de um `if` que nem sempre executa, por exemplo, não contará corretamente todas as iterações, apenas as que satisfizerem aquela condição extra, o que pode ser intencional (contar só ocorrências específicas) ou um erro, dependendo do que se pretende medir.

Um contador que simplesmente soma `1` a cada iteração, sem qualquer condição, é o padrão mais comum e costuma vir acompanhado da inicialização em `0`, já que a contagem sempre começa de "nenhuma ocorrência ainda".

### Acumuladores

```java
java.util.Scanner leitor = new java.util.Scanner(System.in);
int soma = 0;
int numero = -1;

while (numero != 0) {
    System.out.println("Digite um número (0 para parar):");
    numero = leitor.nextInt();
    soma += numero;
}

System.out.println("Soma total: " + soma);
```

Repare que `soma` se parece muito com o contador visto no conceito anterior: é declarada antes do laço, começa em `0` e é atualizada a cada iteração. A diferença está no que ela recebe — em vez de somar sempre `1`, `soma` soma o valor de `numero`, que muda a cada volta conforme o que o usuário digita. Essa variável que vai combinando valores ao longo das iterações, em vez de apenas contá-las, é chamada de acumulador: ela termina o laço carregando um resultado agregado — no exemplo, a soma total de tudo o que foi digitado — em vez de apenas quantas vezes algo aconteceu.

O mecanismo é sempre o mesmo: `soma += numero` (equivalente a `soma = soma + numero`) a cada iteração, combinando o valor já acumulado com o novo valor processado naquela volta. Ao final do laço, `soma` contém o total de todos os números digitados — inclusive o próprio `0` que encerrou o laço, já que ele é somado antes de a condição ser reavaliada. Sem essa variável, cada número lido existiria apenas durante sua própria iteração: a leitura seguinte sobrescreveria a anterior, e não haveria como recuperar, depois que o laço terminasse, o total de tudo o que passou por ali.

Como no contador, é essencial declarar a variável acumuladora antes do laço, e não dentro dele — se `soma` fosse declarada dentro do bloco do `while`, ela seria reinicializada a cada iteração, perdendo o valor acumulado até ali e guardando sempre apenas o último número lido. Outro cuidado é o valor inicial: aqui `0` é o "elemento neutro" da soma, mas a escolha depende da operação — seria `1` para um produto acumulado, ou uma string vazia `""` para uma concatenação.

E é exatamente aí que acumuladores vão além dos contadores: não precisam se limitar a números. Uma string pode ser "acumulada" por concatenação (`textoFinal += palavra`), e estruturas mais complexas, como listas, podem funcionar de forma equivalente, recebendo um novo elemento a cada iteração. O princípio é sempre o mesmo — uma variável que carrega, de uma iteração para a próxima, o resultado combinado de tudo que já foi processado, seja esse resultado uma soma, um texto ou uma coleção.

### Loops infinitos

Um loop infinito é um laço de repetição cuja condição nunca se torna falsa, fazendo com que o bloco de código dentro dele continue executando indefinidamente, sem que o programa avance para o que vem depois do laço. No `while`, isso acontece sempre que nada dentro do bloco tem capacidade de alterar o resultado da condição avaliada — seja porque a variável envolvida nunca é modificada, seja porque a lógica de modificação está incorreta e nunca produz o valor que encerraria o laço.

Antes de identificar essa causa, um loop infinito costuma se manifestar como um sintoma: o programa "trava", parece não responder, ou consome cada vez mais recursos do computador (memória, processamento), sem nunca terminar sua execução nem produzir a saída esperada. É um dos erros mais comuns para quem está começando a escrever laços, justamente porque o código muitas vezes compila e roda normalmente — só não termina.

```java
int contador = 0;

while (contador < 5) {
    System.out.println("Iteração: " + contador);
    // faltou "contador++;" aqui — o laço nunca avança
}
```

Nesse exemplo, `contador` começa em `0` e a condição `contador < 5` é verdadeira — mas como nada dentro do bloco altera o valor de `contador`, essa condição permanece verdadeira para sempre, e a mensagem "Iteração: 0" seria impressa indefinidamente, sem nunca chegar à iteração 1.

A forma de evitar isso é garantir, ao escrever qualquer `while`, que a condição dependa de alguma variável que é de fato modificada dentro do bloco, em um caminho de código que sempre é alcançado — não apenas dentro de um `if` que pode nunca ser satisfeito. Revisar mentalmente "o que muda a cada volta, e esse algo realmente aproxima a condição de se tornar falsa?" é um hábito útil antes de rodar qualquer laço novo.

Nem todo loop infinito é um erro, porém: alguns programas — como um servidor que fica esperando por conexões, ou um jogo que roda continuamente até o jogador sair — são propositalmente escritos com uma condição sempre verdadeira (`while (true)`), contando com um mecanismo interno, geralmente um `break` (visto no capítulo "Controle dos loops" adiante), para encerrar a repetição quando a condição de parada real for satisfeita, ainda que ela não apareça explicitamente na cláusula do `while`.

A diferença entre um loop infinito intencional e um por engano está, portanto, na existência de uma saída controlada: se existe um caminho garantido para sair do laço (seja pela condição do `while`, seja por um `break` interno), o comportamento é previsível; se não existe, o programa simplesmente nunca terminará aquela parte da execução, exigindo que o processo seja interrompido manualmente de fora.

## do while

O `while`, visto no capítulo anterior, testa sua condição antes de executar o bloco pela primeira vez — o que significa que, se a condição já começar falsa, o bloco nunca roda nem uma vez. Este capítulo apresenta uma variação pensada exatamente para os casos em que é preciso garantir que o bloco execute pelo menos uma vez, independentemente da condição: o `do while`.

O capítulo explica essa diferença estrutural — a condição do `do while` é uma pós-condição, avaliada depois do bloco, e não antes — e mostra situações típicas em que essa ordem faz diferença prática, como pedir uma entrada ao usuário e só validar essa entrada depois de já tê-la recebido pelo menos uma vez. Comparar diretamente `while` e `do while` ajuda a fixar quando cada forma é a mais natural para o problema em questão.

### Pós-condição

O `do while` é uma variação do `while` (visto no capítulo anterior) em que a condição de continuidade é avaliada depois de cada execução do bloco, em vez de antes — por isso ela é chamada de pós-condição. A sintaxe inverte a ordem das partes: primeiro vem a palavra-chave `do` seguida do bloco entre chaves, e só depois vem `while (condição)`, terminando com ponto e vírgula.

```java
int opcao;
java.util.Scanner leitor = new java.util.Scanner(System.in);

do {
    System.out.println("Digite 1 para continuar, 0 para sair:");
    opcao = leitor.nextInt();
} while (opcao != 0);

System.out.println("Programa encerrado.");
```

O comportamento prático dessa inversão é que o bloco do `do while` sempre executa pelo menos uma vez, independentemente do valor de qualquer variável envolvida na condição — porque, na primeira execução, a condição ainda nem foi avaliada. No exemplo acima, o menu é exibido e a opção é lida ao menos uma vez, mesmo que `opcao` ainda não tenha nenhum valor definido antes do laço começar (por isso ela é apenas declarada, sem inicialização, já que será atribuída dentro do bloco antes de qualquer leitura).

Essa garantia é útil justamente em situações onde a ação do laço precisa acontecer pelo menos uma vez por definição — como exibir um menu para o usuário escolher uma opção: não faz sentido "verificar antes" se o usuário quer ver o menu, porque ele só pode manifestar essa vontade depois de já ter visto as opções pela primeira vez. O `do while` modela exatamente esse tipo de fluxo, em que a primeira iteração é sempre garantida e as decisões de continuar ou parar só fazem sentido a partir da segunda. A seguir, vale comparar essa estrutura lado a lado com o `while` comum, para deixar claro exatamente quando escolher cada uma.

### Diferença para while

Como acabamos de ver, a diferença central entre `while` e `do while` está em quando a condição é avaliada em relação à execução do bloco: no `while`, ela é verificada antes de cada iteração, inclusive da primeira; no `do while`, depois de cada iteração, também a primeira. Colocando os dois lado a lado, essa diferença de posição tem uma consequência prática direta: o `while` pode nunca executar seu bloco, se a condição já começar falsa, enquanto o `do while` sempre executa o seu pelo menos uma vez, não importa o estado inicial de qualquer variável envolvida.

Sem entender essa diferença, é fácil escolher a estrutura errada e produzir um comportamento sutilmente incorreto — por exemplo, usar um `while` para exibir um menu pode, em teoria, resultar em o menu nunca aparecer, caso a variável usada na condição já comece com um valor que a torna falsa por engano, algo que passaria despercebido até algum caso de teste específico revelar o problema.

```java
int x = 10;

while (x < 5) {
    System.out.println("while: " + x);
    x++;
}
// nada é impresso, porque a condição já começa falsa

do {
    System.out.println("do while: " + x);
    x++;
} while (x < 5);
// imprime "do while: 10" uma vez, mesmo com a condição falsa
```

Nesse exemplo, os dois laços partem do mesmo valor de `x` (`10`) e da mesma condição (`x < 5`), mas produzem resultados diferentes: o `while` não imprime nada, porque a condição já é falsa na primeira verificação; o `do while` imprime uma linha, porque a execução do bloco acontece antes de a condição ser sequer olhada.

A escolha entre as duas formas depende exclusivamente de uma pergunta: o bloco precisa necessariamente rodar pelo menos uma vez, independentemente de qualquer condição? Se a resposta for sim — como em menus, leituras de entrada que serão validadas depois, ou simulações com uma rodada inicial obrigatória — o `do while` é a escolha correta. Se a resposta for não — como somar valores de uma lista que pode estar vazia, ou processar itens que podem simplesmente não existir — o `while` é mais seguro, porque respeita a possibilidade de zero execuções quando a condição já não se sustenta desde o início.

Na prática do dia a dia em Java, o `while` é usado com muito mais frequência que o `do while`, precisamente porque a maioria dos cenários de repetição não exige a garantia de "pelo menos uma vez" — mas quando essa garantia é exatamente o que se precisa, forçar essa lógica dentro de um `while` comum exigiria duplicar a primeira execução do bloco fora do laço, o que o `do while` evita de forma mais direta e legível.

## for

Muitos loops seguem um padrão bem definido: uma variável começa em um valor inicial, uma condição determina até quando repetir, e a cada repetição essa variável é atualizada de forma previsível — por exemplo, contar de 0 até 9. Este capítulo apresenta o `for`, uma estrutura de repetição pensada exatamente para condensar esse padrão em uma única linha, reunindo inicialização, condição e incremento no cabeçalho do loop.

O capítulo detalha cada uma dessas três partes do `for` — a inicialização (geralmente a criação de uma variável contadora), a condição (que determina se o loop continua) e o incremento (a atualização da variável a cada volta) — mostrando como elas se relacionam com o que já foi visto no `while`, mas de forma mais compacta e menos propensa a erros como esquecer de atualizar o contador.

### Inicialização

A inicialização é a primeira das três partes que compõem o cabeçalho de um laço `for`, escrita logo após o parêntese de abertura e separada das demais por ponto e vírgula. Ela é executada uma única vez, antes de qualquer iteração do laço, e normalmente é usada para declarar e dar um valor inicial à variável que controlará a repetição — geralmente chamada de variável de controle ou índice.

Em um `while` (visto em capítulo anterior), essa mesma variável de controle precisaria ser declarada e inicializada em uma linha separada, antes do laço começar — o que funciona, mas espalha em pontos diferentes do código informações que estão logicamente ligadas: onde a repetição começa, até onde ela vai, e como ela avança a cada passo.

O `for` resolve isso reunindo essas três informações no próprio cabeçalho do laço, começando pela inicialização. Isso deixa explícito, já na primeira leitura do código, qual é o ponto de partida da repetição, sem que seja necessário procurar essa informação em uma linha anterior e separada.

```java
for (int i = 0; i < 5; i++) {
    System.out.println("Valor de i: " + i);
}
```

Nesse exemplo, `int i = 0` é a inicialização: ela declara a variável `i`, do tipo `int`, com valor inicial `0`. Essa parte roda exatamente uma vez, antes de a condição (`i < 5`, tratada no próximo conceito) ser avaliada pela primeira vez. A variável declarada na inicialização existe apenas dentro do escopo do laço `for` — ou seja, `i` não pode ser usada em código depois do laço, ao contrário de uma variável declarada fora dele com um `while`.

É possível inicializar mais de uma variável na mesma parte de inicialização, separando-as por vírgula, embora isso seja usado com pouca frequência fora de casos específicos, como percorrer duas sequências em direções opostas simultaneamente. Também é possível deixar a inicialização vazia, se a variável de controle já tiver sido declarada antes do laço — mas isso é raro, porque abre mão justamente da vantagem de reunir as três partes do laço em um único lugar.

Quando o valor inicial da variável de controle é simplesmente `0` (contagem a partir do início de uma sequência, como os índices de um array ou de uma lista), a inicialização costuma ser a parte mais previsível do `for` — a maior parte das decisões de projeto do laço geralmente recai sobre a condição e o incremento, tratados nos próximos dois conceitos.

### Condição

A condição é a segunda parte do cabeçalho de um `for`, escrita entre o primeiro e o segundo ponto e vírgula, logo após a inicialização (vista no conceito anterior). Assim como no `while`, ela é uma expressão booleana avaliada antes de cada iteração — incluindo antes da primeira, já depois de a inicialização ter rodado — e o laço continua se repetindo enquanto essa expressão for verdadeira, parando assim que ela se tornar falsa.

O papel da condição no `for` é o mesmo que no `while`: decidir, a cada volta, se o laço continua ou não. A diferença está em onde ela é escrita e no que normalmente envolve — como o `for` já traz consigo uma variável de controle criada na inicialização, a condição do `for` costuma comparar diretamente essa variável com algum limite, algo que, no `while`, exigiria lembrar de escrever essa comparação manualmente, sem o apoio visual de estar ao lado da inicialização e do incremento no mesmo cabeçalho.

```java
for (int i = 0; i < 5; i++) {
    System.out.println("Valor de i: " + i);
}
// imprime i de 0 a 4 — a condição "i < 5" impede a iteração com i igual a 5
```

Nesse exemplo, `i < 5` é a condição: ela é avaliada antes de cada execução do bloco, com o valor atual de `i`. Quando `i` vale `0`, `1`, `2`, `3` ou `4`, a condição é verdadeira e o bloco executa; quando `i` chega a `5` (depois do incremento da quinta iteração, tratado no próximo conceito), a condição se torna falsa e o laço para, sem que o bloco chegue a executar com `i` igual a `5`.

Um erro comum ao escrever a condição de um `for` é confundir `<` com `<=`, alterando por um a quantidade de iterações — por exemplo, `i <= 5` no lugar de `i < 5` faria o laço rodar uma vez a mais, incluindo `i` igual a `5`. Esse tipo de engano, conhecido informalmente como "erro de um a mais" (off-by-one), é uma das causas mais comuns de bugs em laços, especialmente ao percorrer posições de arrays ou listas, cujos índices costumam ir de `0` até o tamanho menos um.

Assim como no `while`, se a condição do `for` já começar falsa (por exemplo, `for (int i = 10; i < 5; i++)`), o bloco nunca executa, nem mesmo uma vez — o comportamento de "verificar antes de agir" é o mesmo do `while`, apenas reorganizado dentro do cabeçalho do `for`. E, também como no `while`, deixar a condição vazia (sem nenhuma expressão entre os pontos e vírgulas) faz com que ela seja tratada como sempre verdadeira, criando um laço infinito — um recurso às vezes usado propositalmente, mas que deve ser combinado com um `break` interno (visto no capítulo seguinte) para ter uma saída controlada.

### Incremento

Depois da inicialização e da condição, falta a última peça do cabeçalho de um `for`: o incremento, escrito depois do segundo ponto e vírgula. Diferente das outras duas partes, ele não roda nem uma única vez no início, nem antes de cada iteração — o incremento é executado depois de cada execução do bloco, e só então a condição é reavaliada para decidir se haverá uma próxima volta.

No `while`, o equivalente ao incremento precisa ser escrito manualmente dentro do próprio bloco do laço — geralmente na última linha, como visto nos exemplos de contador (capítulo anterior). Isso funciona, mas cria um risco real: como essa linha está misturada com o resto da lógica do bloco, é fácil esquecê-la, especialmente se o bloco tiver várias instruções, ou se algum `if` interno criar um caminho que pula essa linha por engano — e esse esquecimento é justamente a causa mais comum de loops infinitos, como visto no capítulo do `while`.

O `for` resolve isso reservando um espaço fixo e sempre executado para essa atualização, separado do corpo do laço. Isso não elimina totalmente o risco de erro, mas reduz bastante a chance de esquecimento, porque o incremento fica junto da inicialização e da condição, no mesmo cabeçalho, formando um conjunto visualmente compacto e fácil de revisar.

```java
for (int i = 0; i < 10; i += 2) {
    System.out.println("Valor de i: " + i);
}
// imprime 0, 2, 4, 6, 8
```

Nesse exemplo, `i += 2` é o incremento: em vez de somar `1` a cada volta (o mais comum, geralmente escrito como `i++`), esse laço soma `2`, produzindo apenas números pares. Isso mostra que "incremento" é um nome um pouco genérico — a expressão nessa parte do cabeçalho pode ser qualquer atualização da variável de controle, incluindo diminuir seu valor (`i--`), multiplicá-lo, ou aplicar qualquer outra operação válida, desde que, eventualmente, ela leve a condição a se tornar falsa.

Um laço `for` que conta de forma decrescente, por exemplo, usa uma inicialização com o maior valor, uma condição que compara com um limite inferior, e um incremento que na verdade diminui:

```java
for (int i = 5; i > 0; i--) {
    System.out.println("Contagem regressiva: " + i);
}
// imprime 5, 4, 3, 2, 1
```

É possível, ainda, atualizar mais de uma variável na parte de incremento, separando as expressões por vírgula — de forma parecida com múltiplas inicializações, isso é usado com pouca frequência, tipicamente em laços que percorrem duas sequências relacionadas ao mesmo tempo, avançando ambas as variáveis a cada volta.

## Controle dos loops

Nem toda repetição precisa rodar do início ao fim exatamente como planejada: às vezes é preciso interromper um loop antes da condição normal de parada ser atingida, ou pular apenas uma repetição sem interromper as demais. Este capítulo, depois de apresentar as três estruturas de loop de Java nos capítulos anteriores, mostra como controlar o comportamento delas em tempo de execução.

O capítulo cobre o `break` (que interrompe o loop imediatamente), o `continue` (que pula o restante da iteração atual e avança para a próxima), como esse controle se comporta em loops aninhados (um loop dentro de outro) e o uso de labels (rótulos) para direcionar um `break` ou `continue` a um loop específico quando há mais de um nível de aninhamento. São ferramentas que dão mais precisão sobre exatamente quando e como uma repetição deve parar ou avançar.

### `break`

O `break` é uma palavra-chave que interrompe imediatamente a execução do laço em que está inserido (`while`, `do while` ou `for`), fazendo o programa pular direto para a primeira instrução depois do laço, sem completar a iteração atual nem avaliar a condição mais uma vez. Ele já apareceu de forma relacionada no `switch` tradicional, em módulos anteriores deste livro, mas dentro de um laço seu efeito é interromper a repetição inteira, não apenas um bloco de `case`.

Encerrar um laço apenas através de sua condição normal (a cláusula do `while` ou do `for`) funciona bem quando a decisão de parar depende de algo que já está naturalmente ligado a essa condição. Nem sempre é o caso, porém: às vezes o motivo para parar surge no meio do bloco, a partir de uma verificação sem relação direta com a variável de controle do laço, e forçar essa lógica dentro da condição do cabeçalho deixaria o código artificial. É para essa situação que existe o `break` — ele permite uma saída antecipada e imediata, a partir de qualquer ponto dentro do bloco do laço, tipicamente dentro de um `if` que detecta a condição especial de parada.

```java
int[] numeros = {4, 8, 15, 16, 23, 42};
int alvo = 16;
int posicaoEncontrada = -1;

for (int i = 0; i < numeros.length; i++) {
    if (numeros[i] == alvo) {
        posicaoEncontrada = i;
        break; // não há motivo para continuar procurando depois de encontrar
    }
}

System.out.println("Encontrado na posição: " + posicaoEncontrada);
```

Nesse exemplo, o laço percorreria normalmente todas as posições do array até `i` chegar a `numeros.length`, mas assim que o valor procurado é encontrado, continuar comparando as posições restantes seria desperdício — o `break` interrompe o laço imediatamente, e a execução segue a partir do `System.out.println` depois dele.

Quando usado dentro de laços aninhados (um laço dentro de outro, tratado em conceito adiante), o `break` interrompe apenas o laço mais interno em que está diretamente inserido, deixando o laço externo continuar normalmente — para interromper um laço externo a partir de dentro de um laço interno, é necessário o uso de labels, também tratado adiante neste capítulo.

O `break` é uma ferramenta útil, mas usá-lo em excesso, ou para simular condições de parada que poderiam ser expressas com clareza na própria condição do laço, tende a deixar o código mais difícil de acompanhar — quando a lógica de parada é simples e direta (como "repita 10 vezes" ou "repita até o usuário digitar sair"), colocá-la na condição do laço, em vez de dentro de um `if` com `break`, costuma resultar em um código mais fácil de ler de cima a baixo.

### `continue`

Assim como o `break`, visto no conceito anterior, o `continue` é uma palavra-chave usada dentro de laços — mas o efeito dos dois é diferente. Em vez de encerrar o laço inteiro, o `continue` interrompe apenas a iteração atual: pula o restante das instruções do bloco naquela volta específica e vai direto para a próxima etapa do laço — no `while` e no `do while`, isso significa ir direto para a reavaliação da condição; no `for`, significa ir primeiro para o incremento, e só depois para a reavaliação da condição. A repetição em si não é interrompida — ela prossegue normalmente a partir da próxima iteração.

Sem o `continue`, pular parte da lógica de uma iteração específica, mantendo o restante do laço funcionando normalmente, exigiria aninhar o restante do bloco dentro de um `if` que verificasse o contrário da condição de exclusão — o que funciona, mas pode deixar blocos grandes com um nível extra de indentação só para acomodar esse desvio, tornando o código visualmente mais denso do que o necessário.

O `continue` resolve isso permitindo declarar explicitamente "esta iteração específica não precisa continuar", logo no ponto em que essa decisão é identificada, sem precisar envolver o restante do bloco em um `if` adicional.

```java
for (int i = 1; i <= 10; i++) {
    if (i % 2 != 0) {
        continue; // pula os números ímpares
    }
    System.out.println("Número par: " + i);
}
```

Nesse exemplo, quando `i` é ímpar, o `continue` interrompe aquela iteração imediatamente, sem chegar até o `System.out.println` — a execução vai direto para o incremento (`i++`) e depois para a reavaliação da condição (`i <= 10`). Como resultado, apenas os números pares entre 1 e 10 são impressos, sem que seja necessário envolver o `System.out.println` dentro de um `if (i % 2 == 0)`.

Vale a mesma restrição vista para o `break`: dentro de laços aninhados, o `continue` afeta apenas o laço mais interno em que está diretamente inserido — pular uma iteração do laço externo a partir de dentro de um laço interno exige o uso de labels, tratado no próximo conceito.

O `continue` costuma ser preferível ao equivalente com `if` quando a condição de exclusão é simples e a lógica que seria pulada é a maior parte do bloco — nesses casos, o `continue` evita afundar quase todo o corpo do laço dentro de um nível extra de indentação. Quando, ao contrário, apenas uma pequena parte do bloco depende da condição, geralmente é mais claro simplesmente colocar essa parte dentro de um `if` comum, sem recorrer ao `continue`.

### Loops aninhados

Loops aninhados são laços de repetição escritos um dentro do bloco de outro, de forma que, a cada iteração do laço externo, o laço interno é executado por completo, do início ao fim, antes que o laço externo avance para sua próxima iteração. Não existe nenhuma palavra-chave nova envolvida — qualquer combinação de `while`, `do while` e `for` pode ser aninhada dentro de qualquer outra, bastando que o laço interno esteja escrito dentro do bloco de código do laço externo.

Um único laço percorre uma sequência de valores em uma dimensão — uma lista de números, os caracteres de um texto, uma faixa de contagem. Muitos problemas reais, porém, envolvem duas dimensões relacionadas entre si — as linhas e colunas de uma tabela, os pares possíveis entre dois conjuntos de itens, ou os dias e horários de uma agenda semanal — e um único laço não tem como representar essa combinação de duas variáveis percorrendo faixas independentes ao mesmo tempo.

Aninhar um laço dentro do outro resolve isso: para cada valor do laço externo, todo o intervalo do laço interno é percorrido, produzindo, ao final, todas as combinações possíveis entre os valores das duas variáveis de controle.

```java
for (int linha = 1; linha <= 3; linha++) {
    for (int coluna = 1; coluna <= 3; coluna++) {
        System.out.print(linha + "," + coluna + "  ");
    }
    System.out.println(); // pula para a próxima linha da tabela
}
```

Nesse exemplo, para cada valor de `linha` (1, depois 2, depois 3), o laço interno percorre `coluna` inteiramente (1, 2 e 3) antes que `linha` avance — o resultado é a impressão de todos os nove pares possíveis entre `linha` e `coluna`, organizados como uma grade. O `System.out.println()` sem argumentos, colocado fora do laço interno mas dentro do externo, garante que cada linha da grade seja impressa em uma linha de saída separada.

Um ponto importante sobre laços aninhados é o custo: se o laço externo roda `n` vezes e o laço interno roda `m` vezes a cada uma dessas voltas, o bloco mais interno acaba executando `n` vezes `m` no total — um número que cresce rapidamente conforme `n` e `m` aumentam. Para tabelas pequenas, como no exemplo, isso não é um problema perceptível, mas para faixas grandes (milhares de valores em cada laço), o tempo total de execução pode se tornar um fator relevante a considerar.

Também é importante usar nomes de variáveis diferentes para cada nível de aninhamento — `linha` e `coluna` no exemplo, em vez de `i` em ambos — já que reutilizar o mesmo nome não é permitido pelo compilador quando um laço está dentro do escopo do outro, e mesmo quando fosse permitido em outro contexto, nomes diferentes deixam claro qual variável pertence a qual nível do aninhamento, facilitando a leitura do código.

### Labels

Labels são identificadores colocados imediatamente antes de um laço, seguidos de dois-pontos, que dão um nome a esse laço específico — permitindo que um `break` ou `continue` (vistos em conceitos anteriores deste capítulo) façam referência explícita a ele, mesmo estando escritos dentro de um laço mais interno. A sintaxe é `nomeDoLabel: for (...) { ... }`, e o mesmo nome é então usado como `break nomeDoLabel;` ou `continue nomeDoLabel;`.

Como visto nos conceitos de `break` e `continue`, dentro de laços aninhados essas duas palavras-chave afetam, por padrão, apenas o laço mais interno em que estão diretamente inseridas. Isso é suficiente na maioria dos casos, mas surge uma limitação real quando a decisão de parar (ou pular uma iteração) precisa afetar o laço externo, a partir de uma verificação feita dentro do laço interno — sem um recurso específico para isso, seria necessário recorrer a variáveis auxiliares do tipo `boolean`, verificadas manualmente na condição do laço externo, apenas para sinalizar "pare também", o que adiciona complexidade ao código.

Labels resolvem isso permitindo que um `break` ou `continue` declarem explicitamente a qual laço, entre os aninhados, eles se referem — não precisa ser necessariamente o laço mais externo de todos, apenas qualquer um dos laços que envolvem o ponto onde o `break` ou `continue` está escrito.

```java
busca:
for (int linha = 0; linha < 3; linha++) {
    for (int coluna = 0; coluna < 3; coluna++) {
        if (linha == 1 && coluna == 1) {
            System.out.println("Encontrado em " + linha + "," + coluna);
            break busca; // interrompe o laço externo, não só o interno
        }
    }
}
```

Nesse exemplo, sem o label `busca`, um `break` simples interromperia apenas o laço de `coluna`, e o laço de `linha` continuaria normalmente para seu próximo valor — com `break busca;`, no entanto, o laço externo (identificado pelo label) também é interrompido, encerrando a busca por completo assim que a condição é satisfeita, independentemente de quantos níveis de aninhamento existam entre o `break` e o laço referenciado.

O mesmo mecanismo se aplica a `continue`: um `continue nomeDoLabel;` pula o restante da iteração atual do laço identificado pelo label, não apenas do laço mais interno, avançando diretamente para a próxima iteração daquele laço específico.

Labels são um recurso pouco usado no dia a dia — a maior parte dos laços aninhados não precisa dessa comunicação entre níveis, e um uso excessivo de labels tende a tornar o fluxo do código mais difícil de acompanhar, já que quebra a expectativa comum de que um `break` ou `continue` afeta apenas o laço mais próximo. Ainda assim, em situações específicas, como interromper uma busca em uma estrutura de duas dimensões assim que o item é encontrado, o label é a forma mais direta e legível de expressar essa intenção, evitando variáveis de sinalização auxiliares.

## Problemas clássicos de repetição

Este capítulo fecha o módulo aplicando tudo que foi visto — `while`, `do while`, `for`, `break`, `continue` e labels — a um conjunto de problemas que aparecem, com pequenas variações, em praticamente qualquer programa que use repetição. Em vez de introduzir sintaxe nova, o foco aqui é reconhecer padrões de solução e praticar a escolha da estrutura de loop mais adequada para cada tipo de problema.

O capítulo percorre contagem (quantas vezes algo ocorre), somatórios (acumular um total ao longo de várias repetições), busca (percorrer um conjunto de dados até encontrar um valor específico), e validação (repetir a leitura de uma entrada até que ela atenda a um critério). O capítulo fecha revisando esses padrões iterativos de forma consolidada, para que o leitor saiba reconhecer, diante de um problema novo, qual desses padrões conhecidos se aplica.

### Contagem

Contagem, como problema clássico resolvido com laços de repetição, é a tarefa de determinar quantos elementos de um conjunto satisfazem uma determinada condição — por exemplo, quantos números em uma lista são pares, quantos caracteres de um texto são vogais, ou quantas notas em uma turma são maiores ou iguais a sete. Esse problema combina diretamente dois conceitos já vistos neste módulo: um laço que percorre os elementos (`for` ou `while`) e um contador (visto no capítulo do `while`) que é incrementado sempre que a condição de interesse se verifica.

Sem um laço, contar elementos que satisfazem uma condição exigiria examinar cada valor manualmente, um por um — algo inviável para qualquer conjunto de dados com mais do que um punhado de elementos, e completamente impraticável quando o tamanho do conjunto só é conhecido em tempo de execução.

A solução segue sempre o mesmo formato: um contador inicializado em `0` antes do laço, um laço que percorre todos os elementos do conjunto, e um `if` dentro do laço que incrementa o contador apenas quando a condição desejada é verdadeira para o elemento da iteração atual.

```java
int[] notas = {8, 5, 9, 4, 7, 10, 6};
int aprovados = 0;

for (int i = 0; i < notas.length; i++) {
    if (notas[i] >= 7) {
        aprovados++;
    }
}

System.out.println("Alunos aprovados: " + aprovados);
```

Nesse exemplo, o laço percorre cada nota do array, e `aprovados` só é incrementado quando a nota da iteração atual é maior ou igual a `7` — ao final do laço, `aprovados` contém exatamente a quantidade de notas que satisfizeram essa condição, sem que seja necessário guardar quais notas específicas foram elas.

A contagem é considerada um problema "clássico" porque sua estrutura — laço mais `if` mais incremento condicional — se repete, com pequenas variações, em uma quantidade enorme de situações práticas: contar itens em estoque abaixo de um limite mínimo, contar linhas de um arquivo que contêm uma palavra específica, contar quantas tentativas de login falharam. Reconhecer esse padrão facilita resolver problemas novos que, à primeira vista, parecem diferentes, mas seguem exatamente essa mesma estrutura por baixo.

Uma variação comum é contar todos os elementos do conjunto sem nenhuma condição — nesse caso, o `if` simplesmente não é necessário, e o contador é incrementado a cada iteração do laço, sem exceção; embora, para essa variação específica (contar todos os elementos de um array), Java já oferece a propriedade `.length`, tornando um laço dedicado apenas a contar desnecessário nesse caso particular — o laço só se justifica quando a contagem depende de alguma condição sobre os elementos, não do tamanho total do conjunto.

### Somatórios

O somatório é outro problema clássico de repetição, e talvez o mais direto de todos: em vez de contar quantos elementos satisfazem uma condição, a ideia é acumular o valor de cada elemento em uma soma total — a soma de todos os números de uma lista, o total dos preços de um carrinho de compras, ou a soma apenas dos valores que atendem a algum critério, como as vendas realizadas em um mês específico. A estrutura é a mesma vista no acumulador (capítulo do `while`): uma variável de soma inicializada em `0` antes do laço, que recebe cada valor da sequência somado a si mesma, uma iteração de cada vez.

Assim como na contagem, tentar somar uma quantidade de valores que só é conhecida em tempo de execução sem usar um laço seria inviável — não haveria como escrever, diretamente no código-fonte, uma sequência fixa de somas (`valor1 + valor2 + valor3 + ...`) quando o número de parcelas muda a cada execução, dependendo dos dados de entrada recebidos pelo programa.

```java
double[] precos = {29.90, 15.50, 8.00, 42.75};
double total = 0;

for (int i = 0; i < precos.length; i++) {
    total += precos[i];
}

System.out.println("Total do carrinho: R$ " + total);
```

O array de preços tem tamanho fixo nesse exemplo, mas o código não depende disso: `total` acumula a soma de cada posição percorrida, e a condição do `for` está amarrada a `precos.length`, não a um número escrito manualmente — o mesmo laço soma corretamente um array de qualquer tamanho, com qualquer quantidade de preços.

Uma variação frequente combina somatório com contagem (visto no conceito anterior): somar apenas os valores que satisfazem uma condição, usando um `if` dentro do laço antes de somar, exatamente como no exemplo de contagem de aprovados — a diferença é que, em vez de incrementar um contador em `1`, o valor específico do elemento é somado ao acumulador.

Outra combinação comum é calcular uma média a partir de um somatório: depois que o laço termina e a soma total está pronta, basta dividir esse total pela quantidade de elementos somados (obtida por `.length`, ou por um contador separado, se nem todos os elementos entraram na soma) — o somatório, nesse caso, é apenas o primeiro passo de um cálculo maior, e não o resultado final desejado.

É importante que a variável acumuladora tenha um tipo compatível com a precisão necessária — somar valores do tipo `double`, como no exemplo dos preços, exige que `total` também seja `double`, e não `int`, já que somar valores decimais em uma variável inteira descartaria a parte fracionária de cada parcela, produzindo um resultado incorreto silenciosamente, sem gerar nenhum erro de compilação.

### Busca

Percorrer um conjunto de elementos até localizar um valor específico — e parar imediatamente ao encontrá-lo, ou concluir que ele não existe ali depois de examinar tudo — é o problema da busca, mais um item do repertório de padrões clássicos resolvidos com laços. Esse problema combina um laço que percorre os elementos com o `break` (visto no capítulo "Controle dos loops"), já que continuar procurando depois de já ter encontrado o que se buscava seria trabalho desnecessário.

Sem um mecanismo de parada antecipada, uma busca implementada apenas com um laço comum percorreria sempre todos os elementos do conjunto, mesmo depois de já ter encontrado o valor procurado logo no início — o que funciona (o resultado final estaria correto), mas desperdiça tempo de execução proporcional ao tamanho do restante do conjunto, algo que se torna perceptível em conjuntos de dados grandes.

A solução combina um laço, uma variável para guardar o resultado da busca (inicializada com um valor que represente "ainda não encontrado", como `-1` para posições ou `null` para objetos), um `if` que compara cada elemento com o valor procurado, e um `break` para interromper o laço assim que a comparação for bem-sucedida.

```java
String[] nomes = {"Ana", "Bruno", "Carla", "Diego"};
String procurado = "Carla";
boolean encontrado = false;

for (int i = 0; i < nomes.length; i++) {
    if (nomes[i].equals(procurado)) {
        encontrado = true;
        break; // já encontrou, não precisa continuar
    }
}

System.out.println(encontrado ? "Nome encontrado!" : "Nome não encontrado.");
```

Nesse exemplo, o laço para imediatamente na terceira posição (índice 2, onde está "Carla"), sem examinar a quarta posição ("Diego") — o `break` evita essa comparação desnecessária. A variável `encontrado`, inicializada como `false`, só se torna `true` se algum elemento igual ao procurado for de fato localizado; se o laço percorrer todo o array sem nunca satisfazer o `if`, `encontrado` permanece `false` até o fim, indicando corretamente que o valor não existe naquele conjunto.

Uma variação da busca guarda não apenas se o elemento foi encontrado, mas em qual posição — como visto no exemplo de `break` no capítulo anterior, usando uma variável `int` inicializada em `-1` (uma posição que nunca ocorre de verdade em um array) para representar "não encontrado ainda", e atribuindo o índice real assim que o elemento é localizado.

É importante notar que, para tipos de dados como `String`, a comparação correta usa o método `.equals()`, e não o operador `==` — um tema já tratado em módulos anteriores deste livro, relembrado aqui porque buscas em conjuntos de texto são um dos contextos mais comuns em que esse cuidado se torna necessário. Comparar `String`s com `==` dentro de uma busca é um erro sutil, já que pode aparentar funcionar em alguns testes simples e falhar de forma imprevisível em outros.

### Validação

Validação, como problema clássico de repetição, é o uso de um laço para insistir em obter uma entrada até que ela satisfaça algum critério de aceitação — normalmente aplicado a dados fornecidos pelo usuário, como pedir repetidamente uma idade até que um número positivo seja digitado, ou repetir uma pergunta de "sim ou não" até receber uma resposta reconhecida. Esse problema é a aplicação mais direta do `do while` (visto em capítulo anterior deste módulo), já que a entrada precisa ser lida pelo menos uma vez antes de qualquer verificação fazer sentido.

Um programa que lê a entrada uma única vez, sem repetir a leitura enquanto ela não for válida, fica à mercê de qualquer valor que o usuário forneça — um valor inválido (uma idade negativa, uma opção fora do menu, um texto vazio onde um nome era esperado) seria aceito e propagado para o restante do programa, podendo causar erros ou resultados incorretos mais adiante, longe do ponto em que o dado problemático foi originalmente lido.

A solução repete a leitura da entrada dentro de um laço, cuja condição de continuidade é justamente "o valor lido ainda não é válido" — assim que um valor aceitável é fornecido, a condição se torna falsa e o laço, junto com a insistência por uma entrada correta, termina.

```java
java.util.Scanner leitor = new java.util.Scanner(System.in);
int idade;

do {
    System.out.println("Digite sua idade (deve ser um valor positivo):");
    idade = leitor.nextInt();
    if (idade <= 0) {
        System.out.println("Idade inválida, tente novamente.");
    }
} while (idade <= 0);

System.out.println("Idade registrada: " + idade);
```

Nesse exemplo, o programa insiste em pedir a idade enquanto o valor lido não for positivo — cada tentativa inválida gera uma mensagem de erro e uma nova solicitação, e o laço só termina quando `idade` finalmente recebe um valor maior que zero. O `do while` é a escolha natural aqui porque a primeira leitura precisa acontecer incondicionalmente, antes mesmo de haver qualquer valor para avaliar.

É comum também limitar essa insistência a um número máximo de tentativas, combinando a validação com um contador (visto no capítulo do `while`) que interrompe o laço e direciona o programa para um caminho alternativo quando o limite é atingido — como no próprio exemplo de contador daquele capítulo, com a senha limitada a três tentativas.

Validação também pode envolver múltiplos critérios combinados em uma única condição, usando os operadores lógicos `&&` e `||` (de módulos anteriores) — por exemplo, exigir que um número esteja dentro de uma faixa específica (`idade > 0 && idade < 120`), em vez de apenas ser positivo. O padrão estrutural, porém, permanece o mesmo: repetir a leitura enquanto a condição de invalidade se mantiver verdadeira.

### Padrões iterativos

Padrões iterativos são o reconhecimento de que os problemas clássicos vistos neste capítulo — contagem, somatório, busca e validação — não são técnicas isoladas, mas variações de uma mesma estrutura repetida: um laço percorrendo uma sequência de valores, com uma variável auxiliar (contador, acumulador, sinalizador de encontrado) sendo atualizada a cada iteração de acordo com uma condição avaliada sobre o elemento atual. Entender essa estrutura comum é o que permite resolver um problema novo, nunca visto antes, simplesmente identificando qual das variações ele mais se parece.

Sem reconhecer esse padrão, cada novo problema de repetição pareceria exigir uma solução inteiramente original, mesmo quando, no fundo, ele é apenas uma pequena adaptação de algo já resolvido antes — por exemplo, "contar quantos produtos estão em falta no estoque" e "contar quantos alunos foram aprovados" são, estruturalmente, o mesmo problema de contagem, apesar de tratarem de dados completamente diferentes.

A forma de aplicar esse reconhecimento é, diante de um problema novo, fazer perguntas que revelam a qual padrão ele pertence: o resultado final é uma quantidade de ocorrências (contagem)? É um total acumulado (somatório)? É a localização de um elemento específico, com possibilidade de parar assim que ele for achado (busca)? É a insistência em obter um dado até que ele seja aceitável (validação)? Frequentemente, um problema real combina mais de um desses padrões ao mesmo tempo.

```java
int[] vendas = {150, 0, 320, 0, 275, 0, 410};
int diasComVenda = 0;
int totalVendido = 0;
int primeiroDiaSemVenda = -1;

for (int dia = 0; dia < vendas.length; dia++) {
    if (vendas[dia] > 0) {
        diasComVenda++;           // contagem
        totalVendido += vendas[dia]; // somatório
    } else if (primeiroDiaSemVenda == -1) {
        primeiroDiaSemVenda = dia; // busca
    }
}

System.out.println("Dias com venda: " + diasComVenda);
System.out.println("Total vendido: " + totalVendido);
System.out.println("Primeiro dia sem venda: " + primeiroDiaSemVenda);
```

Nesse exemplo, um único laço combina três padrões diferentes ao mesmo tempo: conta quantos dias tiveram venda, soma o total vendido nesses dias, e localiza o primeiro dia sem nenhuma venda — cada variável auxiliar (`diasComVenda`, `totalVendido`, `primeiroDiaSemVenda`) segue exatamente a lógica já vista em seu padrão individual, apenas coexistindo dentro do mesmo percurso pelos dados, em vez de exigir três laços separados percorrendo o mesmo array três vezes.

Reconhecer padrões iterativos é uma habilidade que se desenvolve com prática, mais do que com regras memorizadas — quanto mais problemas desse tipo forem resolvidos, mais rápido fica o processo de identificar, diante de um enunciado novo, quais variáveis auxiliares serão necessárias, como elas devem ser inicializadas, e sob qual condição cada uma deve ser atualizada a cada iteração. Esse é, em essência, o núcleo prático de se tornar fluente com laços de repetição em qualquer linguagem, não apenas em Java.

# Módulo 6 — Arrays

Até aqui, cada variável guardava um único valor de cada vez. Este módulo apresenta os arrays, a primeira estrutura de dados do livro capaz de armazenar múltiplos valores do mesmo tipo sob um único nome, acessados por posição (índice). É um passo importante porque muitos problemas reais envolvem coleções de dados — uma lista de notas, uma grade de posições em um jogo, uma tabela de valores — e não apenas valores isolados.

O módulo começa pelos arrays unidimensionais (a forma mais simples, uma sequência linear de valores), segue mostrando como percorrê-los usando as estruturas de repetição já conhecidas, depois cobre operações comuns sobre esses dados (busca, soma, máximo, mínimo, alteração de elementos), avança para arrays multidimensionais (como matrizes) e termina explicando como arrays se relacionam com métodos, incluindo como copiá-los corretamente. Ao final deste módulo, o leitor será capaz de organizar e processar coleções de dados de tamanho fixo, uma habilidade que prepara o terreno para estruturas de dados mais flexíveis vistas em módulos futuros do livro.

## Arrays unidimensionais

Este capítulo abre o módulo apresentando a forma mais simples de array: uma sequência linear e de tamanho fixo de valores do mesmo tipo, guardados sob um único nome de variável e acessados por posição numérica (índice), começando sempre do zero. É a estrutura de dados básica sobre a qual todo o restante do módulo — percurso, operações, arrays multidimensionais — é construído.

O capítulo cobre como declarar um array, como efetivamente criá-lo (reservando espaço de memória para uma quantidade definida de elementos), como inicializá-lo já com valores conhecidos no momento da criação, e como acessar cada posição através de índices. Entender bem essa mecânica básica de índices é essencial, já que é também a fonte de um dos erros mais comuns ao trabalhar com arrays: tentar acessar uma posição que não existe.

### Declaração

Declarar um array é informar ao compilador que uma variável vai guardar não um único valor, mas uma coleção de valores do mesmo tipo, acessados por posição. A sintaxe usa colchetes `[]` junto ao tipo (ou, menos comumente, junto ao nome da variável) para marcar que aquele identificador é um array daquele tipo — por exemplo, `int[] notas;` declara uma variável chamada `notas` que, quando existir de fato, vai conter uma sequência de valores `int`.

Antes de arrays, cada dado teria que morar em sua própria variável — para guardar as cinco notas de um aluno, seria preciso declarar `nota1`, `nota2`, `nota3`, `nota4`, `nota5`, cada uma isolada das demais. Isso funciona para quantidades pequenas e fixas, mas não escala: se o número de notas depender de uma entrada do usuário, ou se forem cem notas em vez de cinco, nomear uma variável para cada valor se torna impraticável, e qualquer operação que precise tratar "todas as notas juntas" (somar, ordenar, buscar) exigiria repetir o mesmo código para cada variável isolada.

O array resolve isso ao dar um único nome para a coleção inteira, com as posições individuais acessadas por um índice numérico em vez de por nomes diferentes — a variável `notas` passa a representar o conjunto todo, e cada nota específica é referenciada como `notas[0]`, `notas[1]`, e assim por diante. A declaração em si, porém, apenas cria a variável e informa seu tipo; ela ainda não aloca espaço para os elementos nem define quantos existem — isso é papel da criação do array, tratada na próxima seção.

```java
int[] notas;
double[] precos;
String[] nomes;
```

Java também aceita a forma `int notas[];`, com os colchetes depois do nome, herdada da sintaxe de C — mas colocar os colchetes junto ao tipo (`int[] notas`) é a convenção preferida, porque deixa mais claro que "array de int" é o tipo da variável, e não uma característica do nome `notas`. Essa distinção importa quando várias variáveis são declaradas na mesma linha: em `int[] a, b;`, ambas `a` e `b` são arrays; já em `int a[], b;`, apenas `a` é array e `b` é um `int` comum — outro motivo para preferir a forma com colchetes junto ao tipo.

Uma variável de array declarada mas ainda não criada vale `null`, o mesmo valor padrão de qualquer variável de tipo referência em Java — arrays, afinal, são objetos, não tipos primitivos, mesmo quando guardam valores primitivos como `int` ou `double`. Tentar acessar um elemento de um array que ainda é `null` gera um erro em tempo de execução, já que não há, de fato, nenhum espaço de memória alocado para os elementos ainda.

### Criação

Criar um array é o passo que efetivamente aloca espaço na memória para guardar seus elementos, usando a palavra-chave `new` seguida do tipo e do tamanho desejado entre colchetes — por exemplo, `new int[5]` cria um array capaz de guardar cinco valores `int`. Esse passo normalmente acompanha a declaração na mesma linha, mas também pode ser feito depois, atribuindo o resultado de `new` a uma variável já declarada anteriormente.

Como já vimos, uma variável de array declarada mas ainda não criada vale `null` — a criação é justamente o passo que preenche essa referência vazia com um espaço de memória de fato existente. Tentar usar um array nessas condições, como acessar `notas[0]`, resulta especificamente em um `NullPointerException`: o nome que Java dá ao erro de tentar usar uma referência que ainda não aponta para memória nenhuma.

O `new` resolve isso pedindo ao Java Virtual Machine (JVM) que reserve, de uma vez, um bloco contínuo de memória grande o suficiente para todos os elementos do tamanho informado, e associe esse bloco à variável do array. O tamanho é definido no momento da criação e não muda depois — um array em Java tem tamanho fixo durante toda a sua vida; se for necessário um número diferente de elementos, é preciso criar um array novo, com o tamanho correto, e (se for o caso) copiar os dados do array antigo para o novo.

```java
int[] notas = new int[5];
double[] precos = new double[10];
String[] nomes = new String[3];
```

Nesse exemplo, `notas` passa a apontar para um espaço capaz de guardar cinco valores `int`, `precos` para dez valores `double`, e `nomes` para três referências a objetos `String`. O tamanho pode ser um valor fixo escrito diretamente no código, como nesses exemplos, ou uma variável calculada em tempo de execução — por exemplo, `new int[quantidadeDeAlunos]`, onde `quantidadeDeAlunos` foi lida do usuário ou calculada por outra parte do programa. Essa flexibilidade é justamente o que resolve a limitação de variáveis isoladas: o tamanho do array não precisa ser conhecido enquanto o código é escrito, apenas quando ele é executado.

Ao criar um array com `new`, todos os seus elementos já recebem automaticamente um valor padrão, mesmo sem inicialização explícita — tema tratado com mais detalhe na próxima seção. Um erro comum de iniciantes é tentar criar um array com tamanho negativo (por exemplo, se `quantidadeDeAlunos` for calculado incorretamente e resultar em um valor menor que zero); isso gera um erro em tempo de execução (`NegativeArraySizeException`), já que não existe array com tamanho negativo.

### Inicialização

Depois de declarada e criada, falta à variável de array só uma coisa: conteúdo. Preencher cada posição reservada pela criação com um valor concreto é o que se chama de inicializar um array, e isso acontece de duas formas — automaticamente, com valores padrão fornecidos pelo próprio Java no momento da criação, ou explicitamente, quando o programador informa cada valor desejado. Para a inicialização explícita, Java oferece duas formas principais: atribuir valor a cada posição individualmente depois de criar o array, ou usar uma sintaxe literal que cria e preenche o array em um único passo.

Quando um array é criado apenas com `new tipo[tamanho]`, sem indicar valores, cada elemento já recebe um valor padrão automaticamente: zero para tipos numéricos (`int`, `double`, etc.), `false` para `boolean`, e `null` para tipos referência como `String`. Sem essa garantia, um array recém-criado teria elementos com "lixo de memória" — valores residuais imprevisíveis — o que tornaria qualquer leitura antes de uma atribuição explícita uma fonte de bugs silenciosos e difíceis de rastrear.

Além do preenchimento automático, é comum inicializar cada posição individualmente por atribuição direta:

```java
int[] notas = new int[3];
notas[0] = 8;
notas[1] = 7;
notas[2] = 9;
```

Quando os valores já são conhecidos no momento em que o código é escrito, Java oferece uma forma mais direta, a sintaxe de inicializador de array, que cria o array e já define seu conteúdo e tamanho em uma única expressão entre chaves:

```java
int[] notas = {8, 7, 9};
String[] diasUteis = {"Segunda", "Terça", "Quarta", "Quinta", "Sexta"};
```

Nessa forma, o `new int[]` fica implícito — o próprio compilador conta quantos valores existem entre as chaves e cria um array daquele tamanho exato, já preenchido na ordem em que os valores foram escritos. Essa sintaxe só é permitida no momento da declaração da variável; não é possível usá-la para reatribuir um array já existente (nesse caso, seria preciso escrever `notas = new int[]{8, 7, 9};`, com o `new` explícito).

A escolha entre preenchimento automático (zero/`false`/`null`), atribuição posição a posição, e a sintaxe literal depende de quando os valores ficam conhecidos: dados fixos e conhecidos de antemão favorecem a sintaxe literal, por ser mais compacta; dados que dependem de cálculo, entrada do usuário ou repetição favorecem a atribuição individual, tipicamente dentro de um laço, tema do próximo capítulo.

### Índices

Índice é o número inteiro usado para identificar a posição de um elemento dentro de um array, e é através dele que cada valor individual é lido ou alterado, usando a notação `array[indice]`. Em Java, como na maioria das linguagens derivadas de C, os índices começam em zero, não em um — o primeiro elemento de um array `notas` é `notas[0]`, o segundo é `notas[1]`, e assim sucessivamente até o último elemento, que ocupa a posição `notas[tamanho - 1]`.

Sem índices, cada elemento do array precisaria ser acessado por algum outro mecanismo — nome próprio, por exemplo, o que na prática desfaria toda a vantagem de usar um array em vez de variáveis separadas. O índice é o que permite acessar qualquer posição de forma genérica e calculável: em vez de escrever o nome de uma variável específica, basta calcular ou informar um número, o que possibilita, por exemplo, percorrer um array inteiro variando o índice de zero até seu último valor válido dentro de um laço.

```java
int[] notas = {8, 7, 9, 10, 6};
System.out.println(notas[0]); // 8, primeiro elemento
System.out.println(notas[4]); // 6, último elemento
notas[2] = 9.5 > 9 ? 10 : 9;  // altera o terceiro elemento
```

A contagem a partir de zero costuma confundir quem está começando, já que soa mais natural pensar no "primeiro elemento" como posição um. A forma mais simples de internalizar essa contagem é lembrar que o índice representa quantos elementos existem *antes* daquela posição: o elemento na posição `0` não tem nenhum elemento antes dele (por isso é o primeiro), o da posição `1` tem exatamente um elemento antes (por isso é o segundo), e assim por diante. Outra forma prática é lembrar que, para um array de tamanho `n`, o último índice válido é sempre `n - 1`, nunca `n` — um array de 5 elementos, como `notas` no exemplo acima, tem índices válidos de `0` a `4`.

Um índice fora desse intervalo válido — seja negativo, seja igual ou maior que o tamanho do array — não é simplesmente ignorado ou arredondado para a posição mais próxima: ele produz um erro em tempo de execução, tratado com detalhe na seção "Erros de índice" do próximo capítulo. Por isso, calcular corretamente os limites de um índice, especialmente dentro de laços que o incrementam ou decrementam automaticamente, é uma das fontes mais comuns de erro ao se trabalhar com arrays, e merece atenção redobrada sempre que o tamanho do array não é um número fixo e conhecido de antemão.

## Percorrendo arrays

Criar um array e preenchê-lo manualmente, posição por posição, funciona para exemplos pequenos, mas na prática quase sempre é necessário processar todos os elementos de um array de forma sistemática — e é aí que as estruturas de repetição do módulo anterior se encontram com os arrays deste módulo. Este capítulo mostra como percorrer um array, elemento por elemento, usando um loop.

O capítulo compara duas formas de fazer isso: o `for` tradicional, que usa um índice numérico e por isso permite tanto ler quanto alterar elementos por posição, e o enhanced `for` (também chamado de for-each), uma forma mais simples de percorrer todos os elementos quando não é necessário saber a posição de cada um. O capítulo também explica como obter o comprimento de um array e os erros de índice que ocorrem ao tentar acessar uma posição fora dos limites válidos — um erro comum quando o percurso não respeita corretamente o tamanho do array.

### `for`

O laço `for` tradicional, já visto em módulo anterior como estrutura de repetição controlada por contador, encontra em arrays um de seus usos mais naturais: percorrer todas as posições de um array, de `0` até seu último índice válido, aplicando alguma operação a cada elemento visitado. A variável de controle do `for` assume, nesse uso, o papel de índice — ela começa em `0`, é comparada com o tamanho do array a cada iteração, e é incrementada em uma unidade a cada passo, exatamente cobrindo os índices válidos do array.

Sem um laço, percorrer um array exigiria escrever uma linha de código para cada posição individualmente — viável para um array de três elementos, mas inviável para um array de tamanho variável ou desconhecido em tempo de escrita do código, como um array cujo tamanho vem de uma entrada do usuário. O `for` resolve isso generalizando o acesso: a mesma linha de código, com o índice substituído pela variável de controle do laço, é executada uma vez para cada posição do array, sem que o número de posições precise ser conhecido de antemão.

```java
int[] notas = {8, 7, 9, 10, 6};
int soma = 0;

for (int i = 0; i < notas.length; i++) {
    System.out.println("Nota " + (i + 1) + ": " + notas[i]);
    soma += notas[i];
}

System.out.println("Soma total: " + soma);
```

Nesse exemplo, `i` percorre os índices de `0` a `4` (o tamanho de `notas` é `5`, e a condição `i < notas.length` garante que o laço para antes de `i` chegar a `5`), acessando `notas[i]` a cada iteração para imprimir a nota correspondente e acumulá-la em `soma`. O uso de `notas.length` na condição, em vez de um número fixo como `5`, é o que torna esse `for` reutilizável para arrays de qualquer tamanho — se `notas` tivesse cem elementos em vez de cinco, o mesmo código funcionaria sem nenhuma alteração, já que `.length` sempre reflete o tamanho real do array (assunto da próxima seção).

O `for` é a escolha certa quando o índice em si é necessário dentro do laço — para imprimir a posição junto com o valor, como no exemplo acima, para comparar elementos de posições diferentes entre si, ou para percorrer apenas parte do array (por exemplo, começando em uma posição diferente de zero, ou pulando de dois em dois). Quando o índice não é necessário e o objetivo é apenas visitar cada elemento uma vez, Java oferece uma forma mais direta e menos propensa a erros, o `for` aprimorado, tratado a seguir.

### Enhanced `for`

O enhanced `for` (também chamado de "for-each") é uma variação simplificada do laço `for` voltada especificamente para percorrer todos os elementos de uma coleção — como um array — sem a necessidade de declarar, comparar e incrementar manualmente uma variável de índice. Sua sintaxe declara uma variável que, a cada iteração, recebe diretamente o valor do próximo elemento do array, em vez de um índice a partir do qual o valor precisaria ser buscado.

No `for` tradicional, mesmo quando o objetivo é apenas ler cada valor do array, é preciso gerenciar manualmente a variável de índice — declará-la, definir a condição de parada corretamente em relação a `.length`, e lembrar de incrementá-la a cada volta. Esse gerenciamento manual é uma fonte comum de erros de índice, detalhados adiante na seção "Erros de índice" deste mesmo capítulo — riscos que existem mesmo quando o índice em si não tem nenhuma utilidade para a lógica do laço.

Esconder essa gestão é justamente o que o enhanced `for` faz: o Java cuida internamente de percorrer as posições do array, uma a uma, entregando a cada iteração apenas o valor do elemento atual.

```java
int[] notas = {8, 7, 9, 10, 6};
int soma = 0;

for (int nota : notas) {
    soma += nota;
}

System.out.println("Soma total: " + soma);
```

A leitura dessa sintaxe é "para cada `nota` em `notas`" — a cada iteração, a variável `nota` (que pode ter qualquer nome, escolhido livremente) recebe o valor do próximo elemento de `notas`, começando pelo primeiro e terminando no último, sem que nenhum índice precise ser mencionado. O tipo declarado antes do nome da variável (`int`, nesse exemplo) precisa ser compatível com o tipo dos elementos do array — para um array de `String`, seria `for (String nome : nomes)`.

Essa forma é preferível sempre que o índice não é necessário dentro do laço, tornando o código mais curto e eliminando de vez os erros de "off-by-one" (índice um a mais ou um a menos que o correto) que acompanham o gerenciamento manual do `for` tradicional. Sua limitação é justamente não expor o índice: se for preciso saber "em qual posição" um valor está, alterar o array durante o percurso, ou percorrer apenas parte dele, o `for` tradicional continua sendo a ferramenta correta. O enhanced `for` também não permite alterar os elementos do array através da variável de iteração — atribuir um novo valor a `nota` dentro do laço acima, por exemplo, não muda o conteúdo de `notas`, já que `nota` recebe apenas uma cópia do valor lido, não uma referência à posição original.

### Comprimento

O comprimento de um array — obtido pelo atributo `.length` — é o número de posições que ele possui, definido no momento em que o array foi criado com `new` e fixo durante toda a sua existência. Diferente de métodos, que são invocados com parênteses (como `.length()` em `String`), `.length` em arrays é um atributo, acessado sem parênteses: `notas.length`, não `notas.length()`.

Sem uma forma de consultar o tamanho de um array, qualquer código que precisasse percorrê-lo, validar um índice, ou comparar seu tamanho com outro array teria que depender de um valor de tamanho guardado separadamente à mão — repetido em toda parte do código que precisasse dele, e sujeito a ficar desatualizado caso o array fosse recriado com um tamanho diferente em algum ponto do programa.

`.length` resolve isso ao manter o tamanho sempre disponível diretamente a partir da própria variável do array, sempre correto e atualizado, já que é uma propriedade inerente ao array, não um valor separado calculado e guardado manualmente.

```java
int[] notas = {8, 7, 9, 10, 6};
System.out.println("Quantidade de notas: " + notas.length); // 5

int[] vazio = new int[0];
System.out.println(vazio.length); // 0
```

O uso mais comum de `.length` já apareceu nas seções anteriores: como limite da condição de parada em um `for` que percorre o array (`i < notas.length`), garantindo que o laço nunca tente acessar um índice inválido, mesmo que o tamanho do array mude entre execuções diferentes do programa. Também é comum usar `.length` para validar um índice antes de acessá-lo (`if (indice >= 0 && indice < notas.length)`), ou para calcular posições relativas ao final do array, como o último elemento (`notas[notas.length - 1]`).

Vale notar que um array pode ter tamanho zero — `new int[0]` é um array válido, apenas sem nenhuma posição — e nesse caso `.length` vale `0`, um valor perfeitamente normal, não um erro. É importante também não confundir `.length` (atributo de array) com `.length()` (método de `String`) nem com `.size()` (método usado por outras coleções em Java, como `ArrayList`, ainda não vistas neste nível) — cada um pertence a um tipo diferente, com sintaxe própria, apesar de todos representarem, conceitualmente, "quantos elementos existem".

### Erros de índice

Um erro de índice acontece quando o código tenta acessar uma posição de um array que não existe — um índice negativo, ou um índice igual ou maior que o tamanho do array (`.length`). Em Java, isso não passa silenciosamente nem retorna um valor padrão: o programa lança uma exceção, `ArrayIndexOutOfBoundsException`, que interrompe a execução naquele ponto (a menos que seja tratada, assunto de módulo futuro sobre tratamento de exceções).

Sem essa verificação, um índice inválido levaria o programa a ler ou escrever em uma posição de memória que não pertence ao array — comportamento perigoso e imprevisível, presente em linguagens de mais baixo nível como C, onde acessar um índice fora dos limites pode corromper dados vizinhos na memória sem gerar nenhum aviso imediato, criando bugs extremamente difíceis de rastrear até sua causa real.

Em vez de deixar isso acontecer, Java verifica em tempo de execução todo acesso a uma posição de array, lançando imediatamente uma exceção sempre que o índice usado está fora do intervalo válido (`0` até `length - 1`). Essa verificação tem um pequeno custo de desempenho, mas em troca garante que um erro de índice nunca passe despercebido nem corrompa dados — ele sempre se manifesta como uma falha visível, apontando exatamente a linha onde o acesso inválido ocorreu.

```java
int[] notas = {8, 7, 9, 10, 6};

System.out.println(notas[5]); // erro: índices válidos vão de 0 a 4
```

Executar esse trecho produz algo como `Exception in thread "main" java.lang.ArrayIndexOutOfBoundsException: Index 5 out of bounds for length 5`, indicando exatamente o índice usado (`5`) e o tamanho do array (`5`) — informação suficiente para localizar e corrigir o erro rapidamente. A causa mais comum desse erro é o chamado "off-by-one": usar `<=` em vez de `<` na condição de um `for` (`i <= notas.length` percorreria até o índice `notas.length`, um a mais do que o último válido), ou confundir "quantidade de elementos" com "último índice válido", esquecendo que este último é sempre `length - 1`.

A forma de evitar esse erro é sempre validar índices calculados dinamicamente antes de usá-los — especialmente quando vêm de entrada do usuário, cálculo aritmético, ou outra fonte externa ao próprio laço de percurso — e usar `.length` (em vez de um número fixo escrito manualmente) como limite em qualquer laço que percorra um array, garantindo que o limite acompanhe o tamanho real da estrutura mesmo que ele mude entre execuções diferentes do programa.

## Operações sobre arrays

Com a capacidade de percorrer um array já estabelecida, este capítulo mostra o que fazer com esse percurso na prática: um conjunto de operações que aparecem repetidamente sempre que se trabalha com coleções de dados, muitas delas retomando os padrões de repetição (contagem, busca, acumuladores) já vistos no módulo anterior, agora aplicados especificamente a arrays.

O capítulo cobre como buscar um valor dentro de um array, como somar todos os seus elementos, como encontrar o maior (máximo) e o menor (mínimo) valor presente, e como alterar elementos existentes em posições específicas. Esses padrões, embora simples individualmente, são a base de operações bem mais complexas que aparecerão em módulos futuros do livro, quando estruturas de dados mais ricas forem introduzidas.

### Busca

Busca, aplicada a arrays, é o problema de percorrer os elementos até encontrar (ou confirmar a ausência de) um valor específico, retornando geralmente a posição onde ele foi encontrado, ou algum sinal (como `-1`) indicando que ele não está presente. É a mesma lógica do padrão de busca já vista em módulo anterior sobre laços, aqui aplicada especificamente à estrutura de um array, com o índice do laço servindo diretamente como candidato à posição do valor procurado.

Sem uma busca sistemática, localizar um valor dentro de um array exigiria inspecionar manualmente cada posição — inviável para arrays grandes ou cujo conteúdo não é conhecido de antemão, como um array preenchido a partir de dados do usuário ou de um arquivo. A busca resolve isso automatizando essa inspeção: um laço percorre o array comparando cada elemento com o valor procurado, e interrompe assim que uma correspondência é encontrada, guardando a posição correspondente.

```java
int[] notas = {8, 7, 9, 10, 6};
int procurada = 9;
int posicaoEncontrada = -1;

for (int i = 0; i < notas.length; i++) {
    if (notas[i] == procurada) {
        posicaoEncontrada = i;
        break;
    }
}

if (posicaoEncontrada != -1) {
    System.out.println("Encontrado na posição " + posicaoEncontrada);
} else {
    System.out.println("Valor não encontrado");
}
```

Nesse exemplo, `posicaoEncontrada` começa em `-1`, um valor que nenhum índice válido de array jamais assume, servindo como sinal de "ainda não encontrado" — se o laço terminar sem que a condição `notas[i] == procurada` nunca seja verdadeira, `posicaoEncontrada` permanece `-1`, indicando ausência. O `break` (também de módulo anterior) interrompe o laço assim que o valor é localizado, evitando continuar percorrendo posições que já não importam mais.

Essa busca, feita elemento por elemento do início ao fim, é chamada de busca linear, e funciona sobre qualquer array, ordenado ou não. Quando o array está ordenado, existem alternativas mais eficientes, como a busca binária (que descarta metade das posições restantes a cada comparação, em vez de uma por uma) — mas essa técnica está fora do escopo deste nível introdutório, e a classe `Arrays`, tratada no último capítulo deste módulo, já oferece um método pronto para busca binária quando ela for necessária. Para arrays pequenos ou não ordenados, a busca linear manual, como no exemplo acima, é perfeitamente adequada e é a forma mais direta de entender o mecanismo por trás de qualquer busca.

### Soma

Somar os elementos de um array retoma o padrão de somatório visto em módulo anterior sobre laços: uma variável acumuladora começa em zero e, a cada iteração de um laço que percorre o array, o valor do elemento atual é adicionado a ela, resultando, ao final do percurso, no total de todos os elementos somados.

Sem esse acúmulo progressivo, somar os elementos de um array exigiria escrever uma expressão fixa somando cada posição manualmente (`notas[0] + notas[1] + notas[2] + ...`), o que só é viável quando o tamanho do array é pequeno e conhecido de antemão — para um array de tamanho variável, essa abordagem simplesmente não funciona, já que o número de termos a somar não é conhecido enquanto o código é escrito.

```java
int[] notas = {8, 7, 9, 10, 6};
int soma = 0;

for (int nota : notas) {
    soma += nota;
}

System.out.println("Soma: " + soma);
double media = (double) soma / notas.length;
System.out.println("Média: " + media);
```

O enhanced `for`, visto anteriormente neste módulo, é a escolha natural para esse tipo de laço, já que a soma não depende do índice de cada elemento, apenas de seu valor — cada `nota` visitada é simplesmente somada à variável `soma`, sem necessidade de acessar `notas[i]` explicitamente. Esse exemplo também mostra um uso típico logo após a soma: calcular a média dividindo o total pela quantidade de elementos (`notas.length`), com um cast para `double` garantindo uma divisão com casas decimais em vez de uma divisão inteira truncada (assunto de módulo anterior sobre tipos numéricos).

Um cuidado importante ao somar arrays é escolher um tipo de variável acumuladora compatível com a magnitude esperada do resultado — somar muitos valores `int` grandes pode ultrapassar o limite do tipo `int` e causar overflow (também assunto de módulo anterior), sendo necessário, nesses casos, usar `long` como tipo da variável `soma`. Fora essa ressalva, o padrão de soma sobre arrays é direto e amplamente reutilizável: a mesma estrutura de laço, apenas trocando o array percorrido, serve para somar preços de um carrinho de compras, notas de uma turma, ou qualquer outra coleção numérica representada como array.

### Máximo

O máximo de um array é o maior valor entre todos os seus elementos, encontrado percorrendo o array e mantendo, em uma variável auxiliar, o maior valor visto até o momento — atualizada sempre que um elemento maior que o valor atualmente guardado aparece.

Sem esse acompanhamento contínuo, descobrir o maior valor de um array exigiria comparar todos os elementos entre si de alguma forma — o que rapidamente se torna inviável à mão conforme o array cresce, e não existe atalho sem, no mínimo, observar cada elemento uma vez, já que o maior valor pode estar em qualquer posição.

A solução inicializa uma variável (`maior`, por exemplo) com o primeiro elemento do array, e então percorre os elementos restantes comparando cada um com o valor atual de `maior`: sempre que um elemento maior é encontrado, ele substitui o valor guardado em `maior`. Ao final do percurso, `maior` contém, necessariamente, o maior valor de todo o array, já que todo elemento foi comparado com o candidato até então mais alto.

```java
int[] notas = {8, 7, 9, 10, 6};
int maior = notas[0];

for (int i = 1; i < notas.length; i++) {
    if (notas[i] > maior) {
        maior = notas[i];
    }
}

System.out.println("Maior nota: " + maior);
```

Nesse exemplo, `maior` começa com `notas[0]` (o primeiro elemento, `8`), e o laço começa em `i = 1`, já que o elemento na posição `0` já foi considerado na inicialização — não há necessidade de compará-lo consigo mesmo novamente. A cada iteração, se o elemento atual (`notas[i]`) for maior que `maior`, ele se torna o novo valor de `maior`; caso contrário, nada muda e o laço segue para a próxima posição.

Um erro comum é inicializar `maior` com `0` em vez do primeiro elemento do array — isso parece funcionar quando todos os valores são positivos, mas falha silenciosamente se todos os elementos forem negativos (por exemplo, em um array de saldos negativos ou variações de temperatura abaixo de zero), já que nenhum elemento superaria o `0` inicial, e o resultado reportado seria incorretamente `0`. Inicializar sempre com o primeiro elemento real do array, como no exemplo acima, evita esse problema, funcionando corretamente independentemente do sinal ou da magnitude dos valores envolvidos.

### Mínimo

Encontrar o mínimo de um array segue exatamente a mesma lógica do máximo, tratado na seção anterior, apenas invertendo a comparação: em vez de guardar e atualizar o maior valor visto, a variável auxiliar guarda e atualiza o menor valor visto, substituída sempre que um elemento menor que o valor atual é encontrado durante o percurso do array.

O problema que motiva essa técnica também já foi visto: sem percorrer e comparar cada elemento, não há atalho para descobrir o menor valor de um array, pela mesma razão apontada para o máximo — o menor pode estar em qualquer posição, sem nenhuma garantia de ordenação.

```java
int[] notas = {8, 7, 9, 10, 6};
int menor = notas[0];

for (int i = 1; i < notas.length; i++) {
    if (notas[i] < menor) {
        menor = notas[i];
    }
}

System.out.println("Menor nota: " + menor);
```

A estrutura desse laço é praticamente idêntica à do máximo, só que invertida: `menor` também começa no primeiro elemento do array, pelo mesmo motivo já explicado ali (com `0`, o teste falharia sempre que o array só tivesse valores positivos, da mesma forma que falharia com `0` no máximo se o array só tivesse valores negativos). O laço então começa em `i = 1`, e a cada iteração troca `menor` pelo elemento atual sempre que este for estritamente menor que o valor guardado até então.

É comum, na prática, calcular máximo e mínimo no mesmo laço, em uma única passagem pelo array, em vez de dois laços separados — cada elemento é comparado tanto com `maior` quanto com `menor` dentro da mesma iteração, economizando uma segunda passagem completa pelos dados. Essa combinação é um exemplo direto do que a seção "Padrões iterativos", vista em módulo anterior, descreve: problemas estruturalmente parecidos (aqui, máximo e mínimo) podem ser resolvidos dentro do mesmo laço, com variáveis auxiliares independentes sendo atualizadas segundo suas próprias condições, sem que isso exija percorrer o array mais de uma vez.

### Alteração de elementos

Alterar um elemento de um array é atribuir um novo valor a uma posição já existente, usando a mesma notação de índice usada para leitura (`array[indice]`), agora do lado esquerdo de uma atribuição — `notas[2] = 10;`, por exemplo, substitui o valor anteriormente guardado na posição `2` pelo valor `10`, descartando o valor anterior daquela posição.

Diferente de uma variável comum, cujo valor é substituído inteiramente a cada atribuição, um array permite alterar posições individuais sem afetar as demais — o restante do array permanece intacto, apenas a posição indicada pelo índice é modificada. Sem essa capacidade, corrigir ou atualizar um único valor dentro de uma coleção de dados exigiria recriar o array inteiro do zero, copiando manualmente todos os elementos que deveriam permanecer iguais, apenas para mudar um único valor — trabalho desnecessário e propenso a erros.

```java
int[] notas = {8, 7, 9, 10, 6};
notas[2] = 10; // corrige a terceira nota, antes 9, agora 10

for (int i = 0; i < notas.length; i++) {
    if (notas[i] < 7) {
        notas[i] = 7; // eleva qualquer nota abaixo de 7 até 7 (arredondamento mínimo)
    }
}
```

O segundo trecho desse exemplo mostra um padrão comum de alteração em massa: percorrer o array com um laço `for` tradicional (necessário aqui porque o índice é preciso para escrever de volta na posição, algo que o enhanced `for` não permite, como visto anteriormente) e alterar condicionalmente cada elemento que satisfizer algum critério — nesse caso, elevando qualquer nota abaixo de `7` para exatamente `7`.

Essa capacidade de alterar elementos é o que diferencia, na prática, um array de uma simples cópia imutável de dados: como arrays são objetos passados por referência entre métodos (assunto do capítulo "Arrays e métodos", adiante neste módulo), uma alteração feita em um array dentro de um método pode ser vista por qualquer outro trecho de código que também referencie o mesmo array, o que torna essa operação tanto poderosa quanto uma fonte potencial de efeitos colaterais inesperados quando usada sem cuidado — um tema retomado com mais profundidade adiante, ao tratar de como arrays se comportam quando passados como parâmetros de métodos.

## Arrays multidimensionais

Os arrays vistos até aqui são unidimensionais — uma única sequência linear de valores. Muitos problemas, no entanto, são naturalmente representados em duas ou mais dimensões, como um tabuleiro de jogo, uma planilha ou uma imagem em pixels. Este capítulo apresenta os arrays multidimensionais, que estendem a ideia de array para representar esse tipo de estrutura organizada em linhas e colunas (ou mais dimensões).

O capítulo mostra como declarar e usar matrizes (arrays bidimensionais), explica que, internamente, Java trata isso como um array de arrays — ou seja, cada linha é, ela mesma, um array independente, o que permite inclusive linhas de tamanhos diferentes — e como percorrer essas estruturas usando loops aninhados, uma extensão direta do percurso de arrays unidimensionais já visto, mas agora com um nível extra de repetição para cobrir cada dimensão.

### Matrizes

Uma matriz, em Java, é um array de duas dimensões — uma estrutura organizada em linhas e colunas, na qual cada elemento é acessado por dois índices em vez de um só: `matriz[linha][coluna]`. É a representação natural para dados que já são naturalmente bidimensionais, como uma tabuada, um tabuleiro de jogo, ou uma planilha de valores organizados em grade.

Um array unidimensional, visto no restante deste módulo, é suficiente para representar uma sequência simples de valores, mas se torna inadequado para dados que têm uma estrutura de grade — representar um tabuleiro de jogo da velha, por exemplo, como um único array de nove posições, exigiria calcular manualmente, a cada acesso, qual índice linear corresponde a qual linha e coluna, tornando o código mais confuso do que precisa ser.

A matriz resolve isso declarando um segundo par de colchetes, tanto na declaração do tipo quanto na criação com `new`, indicando o tamanho de cada dimensão:

```java
int[][] tabuada = new int[10][10];

for (int linha = 0; linha < tabuada.length; linha++) {
    for (int coluna = 0; coluna < tabuada[linha].length; coluna++) {
        tabuada[linha][coluna] = linha * coluna;
    }
}

System.out.println(tabuada[3][4]); // 12, resultado de 3 * 4
```

Nesse exemplo, `new int[10][10]` cria uma matriz com dez linhas e dez colunas, totalizando cem posições `int`, cada uma inicializada com o valor padrão `0` até ser explicitamente atribuída. O acesso `tabuada[3][4]` busca o elemento na linha `3`, coluna `4` — a ordem dos índices importa, e por convenção o primeiro índice representa a linha e o segundo, a coluna, embora essa seja apenas uma convenção de leitura, não uma regra imposta pela linguagem.

Percorrer uma matriz por completo, como no exemplo acima, exige dois laços aninhados: o externo percorre as linhas, e o interno, para cada linha, percorre as colunas — padrão detalhado com mais profundidade na seção "Percurso" deste mesmo capítulo. Assim como arrays unidimensionais podem ser inicializados com a sintaxe literal de chaves, matrizes também podem: `int[][] matriz = {{1, 2}, {3, 4}};` cria uma matriz de duas linhas e duas colunas, com a primeira linha contendo `1, 2` e a segunda, `3, 4`. Matrizes com mais de duas dimensões também são possíveis em Java (`int[][][]`, por exemplo), mas raramente aparecem fora de contextos matemáticos ou científicos específicos, e estão fora do escopo deste nível introdutório.

### Arrays de arrays

Em Java, o que parece uma matriz de duas dimensões é, na realidade, um array cujos elementos são, eles mesmos, outros arrays — um "array de arrays". Isso significa que cada linha de uma matriz é, tecnicamente, um array independente, e nada obriga essas linhas a terem o mesmo tamanho, ao contrário do que aconteceria em uma matriz "verdadeira" de dimensões fixas e uniformes, como existe em outras linguagens.

Sem entender essa característica, seria fácil supor que `int[][] tabuada = new int[10][10]` cria uma estrutura rígida, bidimensional, com exatamente cem posições organizadas em uma grade uniforme e imutável em formato — o que é uma simplificação útil na maior parte do tempo, mas não reflete exatamente o que acontece internamente, o que pode gerar confusão ao lidar com casos em que as linhas de uma matriz precisam ter tamanhos diferentes entre si (uma matriz "triangular" ou "irregular", às vezes chamada de jagged array).

Java permite criar arrays de arrays cujas linhas têm tamanhos diferentes justamente por não impor que a segunda dimensão seja fixa — apenas a primeira dimensão (o número de linhas) precisa ser definida no `new`; cada linha, individualmente, é um array separado que pode ser criado com seu próprio tamanho:

```java
int[][] triangular = new int[5][];

for (int linha = 0; linha < triangular.length; linha++) {
    triangular[linha] = new int[linha + 1];
    for (int coluna = 0; coluna < triangular[linha].length; coluna++) {
        triangular[linha][coluna] = linha + coluna;
    }
}

System.out.println(triangular[4].length); // 5, a última linha tem cinco colunas
System.out.println(triangular[0].length); // 1, a primeira linha tem uma coluna
```

Nesse exemplo, `new int[5][]` cria um array de cinco posições, cada uma capaz de guardar um array de `int` — mas nenhuma dessas posições é criada de fato ainda (o segundo colchete fica vazio propositalmente); cada linha é criada individualmente dentro do laço, com um tamanho que cresce conforme o índice da linha (`linha + 1`), resultando em uma estrutura triangular, onde a primeira linha tem uma coluna, a segunda tem duas, e assim sucessivamente.

Essa flexibilidade é o motivo pelo qual, ao acessar `.length` de uma matriz de duas dimensões, é importante saber a qual dimensão aquele `.length` se refere: `tabuada.length` dá o número de linhas, enquanto `tabuada[i].length` dá o número de colunas *daquela linha específica* — que pode, em uma estrutura irregular como a do exemplo acima, ser diferente do número de colunas de outra linha. Para a maioria dos usos práticos deste nível introdutório, matrizes retangulares (todas as linhas com o mesmo tamanho, como no exemplo da seção anterior) são suficientes, mas entender que Java as implementa como arrays de arrays explica por que a linguagem permite, quando necessário, estruturas irregulares como a do exemplo acima.

### Percurso

Percorrer uma matriz (ou, de forma mais geral, um array de arrays) significa visitar cada um de seus elementos, o que exige um laço para cada dimensão da estrutura — tipicamente, um laço externo percorrendo as linhas e, dentro dele, um laço interno percorrendo as colunas daquela linha específica, com os dois laços aninhados um dentro do outro.

Cada posição de uma matriz depende de dois índices, linha e coluna — por isso um único laço, do tipo usado para arrays unidimensionais no início deste módulo, consegue variar apenas um desses dois valores por vez. Escrever um laço separado para cada linha, sem aninhamento algum, até funcionaria para uma matriz de tamanho fixo e conhecido de antemão, mas deixa de ser viável assim que o número de linhas varia ou só é conhecido em tempo de execução — não há como escrever, com antecedência, um laço para cada linha de uma matriz cujo tamanho ainda não se sabe.

O percurso aninhado resolve isso deixando o laço externo fixar uma linha por vez, e o laço interno, para aquela linha fixada, variar a coluna do início ao fim — o resultado é que, ao final da execução completa dos dois laços, toda combinação de linha e coluna terá sido visitada exatamente uma vez.

```java
int[][] tabuleiro = {
    {1, 0, 0},
    {0, 1, 0},
    {0, 0, 1}
};

for (int linha = 0; linha < tabuleiro.length; linha++) {
    for (int coluna = 0; coluna < tabuleiro[linha].length; coluna++) {
        System.out.print(tabuleiro[linha][coluna] + " ");
    }
    System.out.println(); // quebra de linha ao final de cada linha da matriz
}
```

Nesse exemplo, o laço externo (`linha`) executa uma vez para cada linha de `tabuleiro`, e para cada uma dessas execuções, o laço interno (`coluna`) executa uma vez para cada coluna daquela linha, imprimindo o valor em `tabuleiro[linha][coluna]`. O `System.out.println()` vazio, colocado fora do laço interno mas dentro do externo, quebra a linha da saída no console ao final de cada linha da matriz, produzindo uma exibição visualmente organizada em grade.

O uso de `tabuleiro[linha].length` (em vez de `tabuleiro.length`, que daria o número de linhas) como limite do laço interno é o que torna esse percurso seguro mesmo para estruturas irregulares, vistas na seção anterior, onde cada linha pode ter um número diferente de colunas — usar `tabuleiro.length` por engano nos dois laços é um erro comum que, em uma matriz retangular, ainda funciona (já que todas as linhas têm o mesmo tamanho), mas falha silenciosamente ou gera erro de índice em uma estrutura irregular. Além do enhanced `for` aninhado (`for (int[] linha : tabuleiro) { for (int valor : linha) { ... } }`), que também é uma alternativa válida quando os índices não são necessários, o percurso com `for` tradicional continua sendo a forma mais comum quando a posição de cada elemento importa para a lógica do programa.

## Arrays e métodos

Este capítulo fecha o módulo conectando arrays com o módulo de métodos visto anteriormente no livro: como passar um array como argumento para um método, como um método pode devolver um array como resultado, e um cuidado especial que arrays exigem por serem objetos — diferente dos tipos primitivos, copiá-los exige atenção redobrada.

O capítulo relembra que, como qualquer objeto, um array é passado por valor de referência, o que significa que alterações feitas dentro de um método afetam o array original (diferente do que aconteceria com um tipo primitivo). A partir daí, mostra como retornar um array de dentro de um método, como fazer cópias verdadeiramente independentes de um array quando isso é necessário, e apresenta a classe utilitária `Arrays`, da biblioteca padrão, que oferece métodos prontos para tarefas comuns como copiar, comparar e ordenar arrays, evitando reescrever manualmente lógica que a própria linguagem já disponibiliza.

### Passagem para métodos

Passar um array como argumento para um método funciona de forma diferente de passar um valor primitivo (como `int` ou `double`), já vistos em módulo anterior sobre métodos: em vez de o método receber uma cópia independente do valor, ele recebe uma referência para o mesmo array que existe fora dele — o que significa que qualquer alteração feita nos elementos do array dentro do método é visível também fora dele, depois que o método termina.

Com tipos primitivos, um parâmetro funciona como uma cópia isolada: alterar o valor de um parâmetro `int` dentro de um método não afeta a variável original passada como argumento, já que cada uma ocupa seu próprio espaço de memória, independente da outra. Se arrays se comportassem da mesma forma, qualquer método que precisasse modificar o conteúdo de um array (por exemplo, um método que dobra todos os valores de um array de preços) teria que devolver um array novo e o código chamador teria que se lembrar de substituir o array antigo pelo retornado — mais verboso e menos direto do que simplesmente alterar o array já existente.

Como arrays são objetos, o que é passado como argumento não é uma cópia dos elementos, mas uma referência (o "endereço", conceitualmente) para o mesmo array que existe na memória — por isso, o método e quem o chamou compartilham o mesmo array, e alterações feitas por um lado são visíveis pelo outro.

```java
public static void dobrarValores(int[] precos) {
    for (int i = 0; i < precos.length; i++) {
        precos[i] = precos[i] * 2;
    }
}

public static void main(String[] args) {
    int[] precos = {10, 20, 30};
    dobrarValores(precos);
    System.out.println(precos[0]); // 20, o array original foi alterado
}
```

Nesse exemplo, `dobrarValores` recebe `precos` como parâmetro e altera cada elemento diretamente com `precos[i] = precos[i] * 2`; como o parâmetro `precos` dentro do método referencia o mesmo array que a variável `precos` em `main`, a alteração feita dentro do método permanece visível depois que `dobrarValores` termina e a execução volta para `main` — o `System.out.println(precos[0])` imprime `20`, não `10`.

Esse comportamento é poderoso — permite que um método modifique dados sem precisar devolver nada explicitamente — mas também exige cuidado: um método que recebe um array como parâmetro e o altera está produzindo um efeito colateral sobre o código que o chamou, o que pode ser surpreendente se não for intencional ou documentado. Métodos que devem apenas ler dados de um array, sem alterá-lo, devem evitar reatribuir seus elementos; quando é necessário preservar o array original intacto, a técnica de cópia, tratada adiante neste capítulo, é a ferramenta apropriada.

### Retorno de arrays

Assim como um método pode receber um array como parâmetro, ele também pode devolver um array como resultado, usando o tipo do array (`int[]`, `String[]`, etc.) como tipo de retorno na assinatura do método, e a instrução `return` seguida de uma variável de array ou de uma expressão que cria um novo array.

Sem essa possibilade, um método que precisa produzir uma coleção inteira de valores como resultado — por exemplo, um método que gera os primeiros `n` números pares — teria que devolver esses valores de outra forma menos direta, como imprimindo cada um individualmente dentro do próprio método (misturando a responsabilidade de calcular com a de exibir) ou recebendo um array já criado por quem chama, como parâmetro, apenas para preenchê-lo (o que exige que quem chama já saiba, de antemão, o tamanho exato necessário).

O retorno de array resolve isso permitindo que o próprio método decida o tamanho e o conteúdo do array resultante, devolvendo-o como um valor único e coeso para quem o chamou usar como preferir — imprimir, somar, passar adiante para outro método, e assim por diante.

```java
public static int[] gerarPares(int quantidade) {
    int[] pares = new int[quantidade];
    for (int i = 0; i < quantidade; i++) {
        pares[i] = i * 2;
    }
    return pares;
}

public static void main(String[] args) {
    int[] numeros = gerarPares(5);
    for (int numero : numeros) {
        System.out.println(numero);
    }
}
```

Nesse exemplo, `gerarPares` cria um array local, `pares`, preenche cada posição com um número par calculado a partir do índice, e devolve esse array completo com `return pares`. Em `main`, o array retornado é atribuído à variável `numeros`, que passa a referenciar exatamente o mesmo array criado dentro do método — não uma cópia, mas o mesmo objeto, que continua existindo na memória mesmo depois que a execução de `gerarPares` termina, justamente porque uma referência a ele (agora guardada em `numeros`) ainda está em uso.

Esse comportamento — o array sobreviver ao término do método que o criou, desde que alguma referência a ele ainda exista — é o mesmo princípio de gerenciamento de memória que vale para qualquer objeto em Java, não é uma regra especial para arrays; um array local, criado dentro de um método e nunca devolvido nem guardado em nenhuma outra variável, deixa de ser acessível assim que o método termina, e sua memória é eventualmente liberada pelo coletor de lixo da JVM. Métodos que retornam array são especialmente úteis para encapsular um cálculo (como gerar uma sequência, filtrar valores que satisfazem uma condição, ou transformar cada elemento de um array em outro) em uma unidade reutilizável e nomeada, em vez de repetir a mesma lógica de laço em vários pontos diferentes do programa.

### Cópias

Copiar um array significa criar um segundo array, independente do primeiro, com os mesmos valores — de forma que alterar um dos dois não afete o outro, ao contrário do que acontece ao simplesmente atribuir uma variável de array a outra (`array2 = array1`), que apenas faz as duas variáveis apontarem para o mesmo array na memória, sem duplicar nada.

Como já vimos em "Passagem para métodos", arrays são compartilhados por referência entre variáveis que apontam para o mesmo objeto — é exatamente esse comportamento que torna a atribuição direta insuficiente aqui. Quando o objetivo é, de fato, preservar o conteúdo original intacto enquanto se trabalha em uma versão modificável separada — por exemplo, ordenar uma cópia de um array de notas para exibição, mantendo a ordem original de cadastro intacta no array original — uma atribuição simples não resolve, já que qualquer alteração feita através de `copia` também apareceria em `original`.

A forma manual de copiar é percorrer o array original com um laço, copiando cada elemento individualmente para as posições correspondentes de um novo array já criado com o mesmo tamanho:

```java
int[] original = {8, 7, 9, 10, 6};
int[] copia = new int[original.length];

for (int i = 0; i < original.length; i++) {
    copia[i] = original[i];
}

copia[0] = 100; // altera apenas a cópia
System.out.println(original[0]); // 8, o array original permanece intacto
```

Java também oferece formas prontas de copiar um array sem escrever esse laço manualmente, como o método estático `System.arraycopy(...)`, ou os métodos utilitários da classe `Arrays` (tratada na próxima seção), como `Arrays.copyOf(original, original.length)`, que fazem o mesmo trabalho de forma mais concisa e, geralmente, mais eficiente internamente do que um laço escrito à mão.

Um detalhe importante: para arrays de tipos primitivos (`int`, `double`, etc.), copiar cada elemento individualmente, como no exemplo acima, já garante independência total entre original e cópia. Para arrays de tipos referência (como `String[]` ou arrays de objetos definidos pelo próprio programador), essa cópia elemento a elemento copia as referências, não os objetos apontados por elas — os dois arrays passam a ser independentes entre si, mas ainda compartilham os mesmos objetos internamente (uma cópia dita "rasa", ou *shallow copy*); duplicar também os objetos internos, quando necessário, exige uma lógica adicional que foge do escopo deste nível introdutório.

### `Arrays`

`Arrays` é uma classe utilitária da biblioteca padrão de Java (pacote `java.util`) que reúne métodos estáticos prontos para operações comuns sobre arrays — preencher, copiar, ordenar, buscar, comparar e converter para texto, entre outras — evitando que cada uma dessas operações precise ser reescrita manualmente com um laço toda vez que for necessária.

Todas as operações vistas ao longo deste módulo — soma, busca, cópia, percurso — foram implementadas manualmente, com laços escritos à mão, propositalmente, para entender o mecanismo por trás de cada uma. Na prática do dia a dia, porém, reescrever essas mesmas operações repetidamente, para cada array diferente em cada programa diferente, é um desperdício de esforço para problemas já resolvidos de forma testada e otimizada pela própria biblioteca padrão da linguagem — sem contar que o código manual está mais sujeito a pequenos erros, como os de índice já discutidos.

A classe `Arrays` resolve isso concentrando essas operações comuns em métodos prontos, bastando importar a classe (`import java.util.Arrays;`) e chamá-los diretamente:

```java
import java.util.Arrays;

int[] notas = {8, 7, 9, 10, 6};

Arrays.sort(notas);
System.out.println(Arrays.toString(notas)); // [6, 7, 8, 9, 10]

int posicao = Arrays.binarySearch(notas, 9);
System.out.println(posicao); // posição de 9 no array já ordenado

int[] copia = Arrays.copyOf(notas, notas.length);
boolean saoIguais = Arrays.equals(notas, copia);
System.out.println(saoIguais); // true, mesmo conteúdo

int[] preenchido = new int[5];
Arrays.fill(preenchido, 7);
System.out.println(Arrays.toString(preenchido)); // [7, 7, 7, 7, 7]
```

Esse exemplo mostra alguns dos métodos mais usados: `Arrays.sort` ordena os elementos do array em ordem crescente diretamente (alterando o array original); `Arrays.toString` converte um array em uma representação textual legível para impressão, já que imprimir um array diretamente com `System.out.println(notas)` mostraria apenas um código interno de memória, não seus elementos; `Arrays.binarySearch` faz uma busca eficiente (mais rápida que a busca linear vista anteriormente, mas exige que o array já esteja ordenado); `Arrays.copyOf` cria uma cópia independente, poupando o laço manual visto na seção anterior; `Arrays.equals` compara dois arrays elemento a elemento, retornando `true` apenas se ambos tiverem o mesmo tamanho e os mesmos valores nas mesmas posições (diferente do operador `==`, que compara apenas se as duas variáveis apontam para o mesmo array na memória, não se têm o mesmo conteúdo); e `Arrays.fill` preenche todas as posições de um array com um único valor informado.

Conhecer a classe `Arrays` não substitui entender como cada uma dessas operações funciona por dentro — o que foi construído ao longo deste módulo inteiro — mas, uma vez entendido o mecanismo, usar os métodos prontos da biblioteca padrão é a prática recomendada no dia a dia, por ser mais confiável, mais legível e, frequentemente, mais eficiente do que reescrever a mesma lógica manualmente em cada novo programa.
# Módulo 7 — Strings

Até aqui, o livro trabalhou principalmente com números, valores booleanos, caracteres isolados e arrays. Este módulo passa a tratar de um dos tipos de dado mais presentes em programas reais: textos. Em Java, textos são representados principalmente pela classe `String`, que oferece uma forma própria de criar, comparar, consultar, recortar, substituir e formatar sequências de caracteres. Entender bem esse tipo é essencial porque nomes, mensagens, documentos, comandos, dados recebidos de usuários e uma enorme quantidade de informações de negócio chegam ao programa como texto.

Os cinco capítulos foram agrupados para construir esse domínio em etapas: primeiro o leitor conhece a classe `String` e sua característica fundamental de imutabilidade; depois aprende a comparar textos corretamente; em seguida passa às operações de consulta e manipulação; então vê quando a concatenação simples deixa de ser adequada e entra o `StringBuilder`; por fim, aprende recursos de formatação e representação de textos. Ao final do módulo, o leitor será capaz de trabalhar com texto de forma segura e consciente, escolhendo a operação adequada tanto para casos simples quanto para construções mais intensivas.

## A classe String

Programas lidam com texto o tempo todo, mas em Java um texto não é apenas uma sequência informal de caracteres: ele é representado por objetos da classe `String`. Isso significa que textos possuem uma estrutura definida pela linguagem e uma coleção extensa de métodos próprios para consultar tamanho, acessar partes do conteúdo, transformar representações e realizar outras operações frequentes. Antes de manipular uma `String`, portanto, é importante entender o que ela é e como seu comportamento difere do de tipos primitivos.

Este capítulo apresenta essa base começando pelas formas de criar strings, passando pela imutabilidade — uma característica central que explica por que certas operações parecem "alterar" um texto, mas na verdade produzem outro objeto — e chegando aos métodos mais usados da classe. A meta aqui não é decorar a API inteira, mas formar um modelo mental correto de `String`, para que os capítulos seguintes sobre comparação, manipulação e construção de texto façam sentido.

### Criação

Uma `String` em Java representa uma sequência de caracteres — texto, basicamente: um nome, uma frase, um número digitado por um usuário antes de ser convertido, um trecho de arquivo lido linha a linha. Diferente dos tipos primitivos vistos anteriormente (`int`, `double`, `boolean`), `String` é uma classe: toda `String` é, por trás dos panos, um objeto, com métodos próprios que podem ser chamados sobre ela, mesmo que a sintaxe para criá-la se pareça, à primeira vista, com a de um primitivo.

A forma mais comum de criar uma `String` é atribuir diretamente um texto entre aspas duplas a uma variável:

```java
String nome = "Maria";
```

Essa forma usa o chamado *pool de strings* (também chamado de *string pool* ou *string constant pool*): uma área especial de memória, mantida internamente pela JVM, onde literais de texto (textos escritos diretamente entre aspas no código) são armazenados e reaproveitados. Se, em outro ponto do mesmo programa, o texto `"Maria"` for escrito novamente entre aspas, a JVM não cria um segundo objeto — ela reaproveita o mesmo objeto já existente no pool, e a nova variável passa a referenciar exatamente o mesmo texto em memória que a primeira. Esse comportamento existe porque texto é extremamente comum em qualquer programa, e evitar duplicar o mesmo conteúdo repetidamente na memória economiza espaço.

Também é possível criar uma `String` explicitamente com a palavra-chave `new`, da mesma forma como se criaria qualquer outro objeto:

```java
String outroNome = new String("Maria");
```

Nesse caso, a JVM força a criação de um objeto novo, fora do pool, mesmo que o texto `"Maria"` já exista lá — ou seja, `nome` e `outroNome`, apesar de terem o mesmo conteúdo textual, passam a apontar para objetos diferentes na memória. Na prática do dia a dia, essa forma com `new` é raramente necessária e geralmente evitada, justamente porque abre mão do reaproveitamento automático do pool sem trazer nenhum benefício em troca; a forma direta com aspas é a recomendada quase sempre. Essa distinção entre "mesmo conteúdo" e "mesmo objeto em memória" volta a aparecer, de forma central, no capítulo sobre comparação de strings, onde faz diferença prática ao decidir entre `==` e `equals`.

Uma `String` também pode ser criada a partir da concatenação de outras strings, da conversão de outro tipo (como um número) em texto, ou como o retorno de um método — situações que serão vistas em detalhe mais adiante neste módulo. Por ora, o ponto central é: criar uma `String` normalmente é tão simples quanto escrever um texto entre aspas, mas por trás dessa simplicidade há um mecanismo de reaproveitamento de memória que já começa a diferenciar `String` de um primitivo comum.

### Imutabilidade

Uma característica central de `String` é a imutabilidade: uma vez criado, o conteúdo de um objeto `String` nunca muda. Isso pode soar estranho à primeira vista, já que é perfeitamente comum escrever código como:

```java
String saudacao = "Olá";
saudacao = saudacao + ", mundo!";
System.out.println(saudacao); // Olá, mundo!
```

Esse código parece "alterar" o conteúdo de `saudacao`, mas não é isso que acontece por dentro. A expressão `saudacao + ", mundo!"` cria um objeto `String` totalmente novo, com o conteúdo `"Olá, mundo!"`, e é esse novo objeto que passa a ser referenciado pela variável `saudacao` — o objeto original, `"Olá"`, continua existindo em memória, intacto, exatamente como era, apenas sem mais nenhuma variável apontando para ele (e por isso, eventualmente, coletado pelo garbage collector da JVM, mecanismo já visto no módulo sobre arrays). A variável mudou o que ela referencia; o objeto em si nunca mudou.

Isso é diferente do que ocorre com um array, por exemplo, onde `array[0] = 10` altera o conteúdo do mesmo objeto em memória, sem criar um array novo. Nenhum método de `String` — nem `replace`, nem `toUpperCase`, nem `trim`, nem qualquer outro dos vistos adiante neste módulo — altera o objeto original sobre o qual é chamado; todos eles, sem exceção, retornam um novo objeto `String` com o resultado, deixando o original intocado. Um erro comum de quem está começando é escrever `texto.toUpperCase();` esperando que `texto` tenha sido alterado, e se surpreender ao ver que nada mudou — o retorno do método precisa ser capturado, como em `texto = texto.toUpperCase();`, para que o efeito seja de fato observado na variável.

A razão de projeto para essa imutabilidade tem a ver, entre outras coisas, com o próprio pool de strings visto na seção anterior: se `String` fosse mutável, e um trecho de código alterasse o conteúdo do objeto `"Maria"` compartilhado no pool, qualquer outra variável no programa que também apontasse para esse mesmo objeto (por reaproveitá-lo do pool) seria afetada silenciosamente, mesmo sem ter nenhuma relação direta com o código que fez a alteração — um efeito colateral perigoso e difícil de rastrear. A imutabilidade elimina esse risco: um objeto `String` pode ser compartilhado livremente entre variáveis, threads e partes diferentes de um programa sem risco de que uma parte altere silenciosamente o que outra parte enxerga. Esse trade-off — segurança e previsibilidade em troca de sempre criar um objeto novo a cada operação — é também o motivo pelo qual, quando um programa precisa montar um texto grande por meio de muitas modificações sucessivas, a classe `StringBuilder`, vista no capítulo sobre construção eficiente, é preferível a `String`.

### Métodos principais

Como `String` é uma classe, todo objeto `String` carrega consigo um conjunto de métodos prontos para consultar ou derivar informações sobre o texto que ele representa, sem a necessidade de escrever manualmente, por exemplo, um laço que percorra cada caractere. Alguns dos métodos mais usados no dia a dia são:

```java
String texto = "  Livro de Java  ";

System.out.println(texto.length());          // 17 (conta os espaços também)
System.out.println(texto.toUpperCase());     // "  LIVRO DE JAVA  "
System.out.println(texto.toLowerCase());     // "  livro de java  "
System.out.println(texto.charAt(2));          // 'C', caractere na posição 2
System.out.println(texto.isEmpty());          // false, o texto tem conteúdo
System.out.println("".isEmpty());             // true, texto vazio
System.out.println(texto.isBlank());          // false, tem caracteres não vazios
System.out.println("   ".isBlank());          // true, só tem espaços
System.out.println(texto.indexOf("Java"));    // 12, posição onde "Java" começa
```

`length()` devolve a quantidade de caracteres do texto, incluindo espaços — é o equivalente, para `String`, ao atributo `.length` de um array, mas como método, com parênteses, já que `String` é uma classe e `length` internamente é calculado, não um campo público direto. `toUpperCase()` e `toLowerCase()` devolvem uma nova `String` com todas as letras convertidas para maiúsculas ou minúsculas, respectivamente, mantendo os demais caracteres (números, espaços, pontuação) inalterados. `charAt(indice)` devolve o único caractere (tipo `char`) na posição informada, contando a partir de zero, da mesma forma como índices de array já vistos no módulo anterior — e, assim como em arrays, um índice fora do intervalo válido lança uma exceção em tempo de execução.

`isEmpty()` verifica se o texto não tem nenhum caractere (comprimento zero), enquanto `isBlank()` (mais recente na linguagem) verifica se o texto, mesmo tendo caracteres, é composto só por espaços em branco — a diferença importa, por exemplo, ao validar se um usuário realmente digitou algo em um campo de formulário: um texto só com espaços passaria por `isEmpty()` sem ser detectado, mas seria pego por `isBlank()`. `indexOf(sub)` devolve a posição onde uma sub-sequência de texto começa dentro do texto original, ou `-1` caso ela não seja encontrada — um padrão de retorno (`-1` para "não encontrado") comum a vários métodos de busca em Java, inclusive fora de `String`.

Esses métodos são apenas uma amostra inicial; os capítulos seguintes deste módulo aprofundam grupos específicos de métodos — comparação, manipulação de trechos do texto, e construção — que também fazem parte do repertório essencial da classe `String`.

## Comparação de Strings

Comparar textos parece uma tarefa simples: verificar se duas palavras ou frases são iguais. Em Java, porém, essa operação exige distinguir duas perguntas diferentes: se duas variáveis apontam para o mesmo objeto e se os textos armazenados por esses objetos possuem o mesmo conteúdo. Essa diferença explica por que usar `==` com strings pode produzir resultados surpreendentes para quem ainda está pensando apenas em termos de valores.

Neste capítulo, essa distinção será construída passo a passo por meio de `==`, `equals` e `equalsIgnoreCase`, além da comparação por ordem lexicográfica. Ao final, o leitor saberá escolher a forma de comparação conforme a intenção do código — identidade, igualdade de conteúdo, igualdade ignorando maiúsculas e minúsculas ou ordenação — evitando um dos erros mais comuns de iniciantes em Java.

### `==`

O operador `==`, já conhecido de tipos primitivos e de arrays, também pode ser aplicado a `String`, mas seu comportamento aqui exige atenção redobrada, porque o resultado nem sempre é o que a intuição sugere. Para tipos primitivos, `==` compara valores diretamente (`5 == 5` é sempre `true`). Para `String`, que é um tipo referência (um objeto), `==` compara se as duas variáveis apontam para exatamente o mesmo objeto em memória — não se os textos têm o mesmo conteúdo.

```java
String a = "Java";
String b = "Java";
System.out.println(a == b); // true

String c = new String("Java");
System.out.println(a == c); // false
```

`a == b` retorna `true` não porque Java está comparando os caracteres "J", "a", "v", "a" de um lado e do outro, mas porque, como visto na seção sobre criação, ambos os literais `"Java"` foram reaproveitados do mesmo objeto no pool de strings — `a` e `b` apontam, de fato, para o mesmo objeto na memória. Já `a == c` retorna `false` porque `c` foi criado com `new String(...)`, forçando um objeto separado, fora do pool, mesmo tendo o conteúdo textual idêntico a `a`.

Esse comportamento é uma armadilha comum para quem está começando, especialmente porque o código com `==` frequentemente "funciona" durante os testes iniciais — já que a maioria das strings literais realmente vem do pool e, por isso, `==` costuma dar `true` quando os conteúdos coincidem — até que uma string vinda de outro lugar (lida de um arquivo, digitada pelo usuário, resultado de um método como `substring` ou concatenação) quebra essa expectativa silenciosamente, porque objetos criados dessas formas não necessariamente reaproveitam o pool. Um caso típico: comparar uma entrada do usuário, capturada com `Scanner`, contra um valor esperado usando `==`, funciona por acaso em alguns testes e falha de forma imprevisível em produção.

Por essa razão, `==` deve ser evitado para comparar o conteúdo de duas strings — seu uso correto se limita a situações específicas, como verificar se uma variável referencia exatamente `null` (`if (texto == null)`), onde comparar identidade de referência é de fato a intenção. Para comparar se duas strings têm o mesmo conteúdo textual, independentemente de serem ou não o mesmo objeto em memória, o método correto é `equals`, tratado na próxima seção — essa é, de longe, a comparação mais comum no dia a dia com texto, e a confusão entre os dois é um dos erros mais frequentes entre iniciantes em Java.

### `equals`

O método `equals`, herdado por toda classe em Java (inclusive `String`) e redefinido especificamente por `String` para comparar conteúdo, verifica se duas strings têm exatamente a mesma sequência de caracteres, na mesma ordem, com a mesma capitalização — independentemente de serem ou não o mesmo objeto na memória. É o método correto e recomendado para comparar o conteúdo de duas strings na esmagadora maioria das situações práticas.

```java
String a = "Java";
String b = "Java";
String c = new String("Java");

System.out.println(a.equals(b)); // true
System.out.println(a.equals(c)); // true, mesmo conteúdo, mesmo vindo de objetos diferentes
System.out.println(a.equals("java")); // false, maiúscula/minúscula importa
```

Diferente de `==`, que como visto compara identidade de referência, `equals` resolve exatamente o problema descrito na seção anterior: `a.equals(c)` retorna `true` mesmo `c` tendo sido criado com `new String(...)`, porque o método compara caractere a caractere o conteúdo dos dois textos, não a posição de memória de cada objeto. Esse é o comportamento esperado na prática: ao validar se a senha digitada por um usuário confere com a senha armazenada, ou se uma opção escolhida em um menu corresponde a um dos valores esperados, o que importa é o conteúdo do texto, não se os dois objetos foram criados no mesmo lugar do código.

Um detalhe de sintaxe que merece atenção: como `equals` é um método chamado sobre um objeto, ele exige que esse objeto não seja `null` — chamar `.equals(...)` sobre uma variável que vale `null` lança uma exceção em tempo de execução (`NullPointerException`). Por isso, quando existe a possibilidade de uma das strings ser `null`, uma prática comum e mais segura é chamar `equals` a partir do literal fixo, quando ele existir, em vez da variável potencialmente nula: `"Java".equals(variavel)` em vez de `variavel.equals("Java")` — dessa forma, mesmo que `variavel` seja `null`, o método simplesmente retorna `false` em vez de lançar exceção, já que `"Java"` (o literal) nunca é nulo.

Também vale notar que `equals` diferencia maiúsculas de minúsculas: `"Java".equals("java")` é `false`, porque `J` e `j` são caracteres diferentes. Quando essa distinção não é desejada — por exemplo, ao validar um comando digitado pelo usuário que deveria funcionar tanto como "sim" quanto "SIM" — existe um método específico para isso, tratado na próxima seção.

### `equalsIgnoreCase`

`equalsIgnoreCase` funciona exatamente como `equals`, comparando o conteúdo de duas strings caractere a caractere, com a única diferença de que ignora a distinção entre maiúsculas e minúsculas durante a comparação. Enquanto `"SIM".equals("sim")` retorna `false`, por serem sequências de caracteres tecnicamente diferentes, `"SIM".equalsIgnoreCase("sim")` retorna `true`, porque o método trata letras equivalentes (como `S` e `s`) como iguais para fins da comparação.

```java
String resposta = "SIM";

System.out.println(resposta.equals("sim"));            // false
System.out.println(resposta.equalsIgnoreCase("sim"));  // true
System.out.println(resposta.equalsIgnoreCase("Sim"));  // true
System.out.println(resposta.equalsIgnoreCase("não"));  // false, conteúdo diferente
```

Esse método é especialmente útil ao lidar com texto digitado por um usuário, onde não há garantia — nem deveria haver exigência — de que ele respeite exatamente a capitalização esperada pelo programa. Um exemplo típico é validar uma confirmação em um menu de terminal: pedir para o usuário digitar "sim" ou "não", e aceitar a resposta independentemente de ele ter digitado "Sim", "SIM", "sim" ou qualquer combinação de maiúsculas e minúsculas. Usar `equals` nesse caso obrigaria o usuário a acertar exatamente a capitalização esperada, criando uma experiência frustrante e desnecessariamente rígida para uma distinção que, na maioria dos contextos de entrada de usuário, não tem relevância nenhuma.

Internamente, `equalsIgnoreCase` normaliza a capitalização das duas strings antes de compará-las (de forma equivalente, ainda que não idêntica na implementação, a chamar `toUpperCase()` ou `toLowerCase()` em ambas antes de usar `equals`), mas faz isso de forma mais direta e sem criar objetos `String` intermediários desnecessários, sendo por isso a forma preferida sempre que a comparação deve ignorar capitalização, em vez de compor manualmente `a.toUpperCase().equals(b.toUpperCase())`.

Vale reforçar que `equalsIgnoreCase`, assim como `equals`, também está sujeito ao mesmo risco de `NullPointerException` ao ser chamado sobre uma referência nula — a mesma prática de preferir chamar o método a partir de um literal fixo, quando disponível, se aplica aqui também.

### Ordem lexicográfica

Além de verificar se duas strings são iguais, muitas vezes é necessário saber qual delas viria antes da outra em uma ordenação — por exemplo, para ordenar uma lista de nomes alfabeticamente. O método `compareTo` resolve isso, comparando duas strings segundo a chamada ordem lexicográfica: essencialmente, a ordem alfabética, mas definida com mais precisão a partir do valor numérico de cada caractere (seu código Unicode).

```java
String a = "banana";
String b = "laranja";
String c = "banana";

System.out.println(a.compareTo(b)); // negativo, "banana" vem antes de "laranja"
System.out.println(b.compareTo(a)); // positivo, "laranja" vem depois de "banana"
System.out.println(a.compareTo(c)); // 0, strings iguais
```

`compareTo` devolve um número inteiro (`int`), não um `boolean`: um valor negativo indica que a string sobre a qual o método foi chamado viria antes da string passada como argumento na ordenação; um valor positivo indica o contrário; e zero indica que as duas strings são iguais (equivalente, nesse caso específico, a `equals`). O valor exato desse número não costuma ter significado próprio — o que importa, na prática, é apenas o seu sinal (negativo, positivo ou zero) — embora, tecnicamente, ele corresponda à diferença entre os códigos Unicode do primeiro par de caracteres em que as duas strings divergem.

A comparação acontece caractere a caractere, da esquerda para a direita, até encontrar a primeira posição onde as strings diferem; se uma string é prefixo exato da outra (como `"casa"` e `"casaco"`), a mais curta é considerada "menor" e viria antes. Um ponto que costuma surpreender: como a ordem lexicográfica se baseia no código numérico de cada caractere, letras maiúsculas têm valor numérico menor do que as minúsculas correspondentes, o que faz com que, por exemplo, `"Zebra".compareTo("abacate")` dê um resultado negativo (indicando que "Zebra" viria antes de "abacate"), mesmo sendo "abacate" alfabeticamente anterior do ponto de vista humano — porque `Z` maiúsculo tem código menor que `a` minúsculo. Quando a ordenação deve ignorar essa distinção, existe a variante `compareToIgnoreCase`, que funciona da mesma forma, mas normalizando a capitalização antes de comparar, de forma análoga ao que `equalsIgnoreCase` faz para igualdade.

Esse método é usado, entre outras aplicações, internamente pelos algoritmos de ordenação de coleções de strings em Java (como `Arrays.sort` aplicado a um array de `String`, já visto no módulo anterior), que ordenam o conteúdo justamente seguindo essa ordem lexicográfica por padrão.

## Manipulação

Uma vez que um texto está dentro de uma `String`, frequentemente é necessário extrair apenas uma parte, descobrir se determinado trecho existe, substituir conteúdo, dividir uma informação em partes ou remover espaços indesejados. Essas operações aparecem em tarefas cotidianas como tratar nomes digitados por usuários, separar campos de uma linha, limpar dados recebidos de arquivos ou verificar padrões simples em uma mensagem.

Este capítulo reúne os principais métodos de manipulação justamente porque todos operam sobre o conteúdo de uma string sem mudar sua natureza imutável. Serão estudados recursos como `substring`, `contains`, `replace`, `split`, `trim` e `strip`, observando tanto o resultado de cada operação quanto o fato de que uma nova `String` costuma ser produzida. Ao final, o leitor conseguirá decompor e transformar textos de maneira previsível.

### `substring`

`substring` extrai uma parte específica de uma `String`, devolvendo um novo objeto `String` com apenas o trecho solicitado — o texto original permanece intacto, por conta da imutabilidade já vista. Existem duas formas de chamá-lo: passando apenas o índice inicial, ou passando o índice inicial e o final.

```java
String texto = "Programação em Java";

System.out.println(texto.substring(15));     // "Java", do índice 15 até o fim
System.out.println(texto.substring(0, 12));  // "Programação", do índice 0 até 11
System.out.println(texto.substring(15, 19)); // "Java"
```

Com um único argumento, `substring(indiceInicial)` devolve tudo a partir daquele índice (inclusive) até o final do texto. Com dois argumentos, `substring(indiceInicial, indiceFinal)` devolve o trecho começando no índice inicial (inclusive) e terminando logo antes do índice final (ou seja, o índice final é exclusivo) — esse padrão de "início inclusivo, fim exclusivo" é comum em várias APIs de Java, e vale a pena internalizá-lo: o comprimento do trecho extraído é sempre `indiceFinal - indiceInicial`.

Um uso real comum é extrair partes estruturadas de um texto que segue um formato conhecido — por exemplo, separar o código de área e o número de um telefone formatado como `"(11) 98888-7777"`, ou extrair a extensão de um nome de arquivo a partir da posição do último ponto. Combinado com `indexOf` (visto anteriormente), `substring` permite localizar e extrair um trecho sem precisar saber sua posição de antemão:

```java
String email = "usuario@exemplo.com";
int posicaoArroba = email.indexOf("@");
String usuario = email.substring(0, posicaoArroba);
String dominio = email.substring(posicaoArroba + 1);

System.out.println(usuario); // "usuario"
System.out.println(dominio); // "exemplo.com"
```

Um erro comum é fornecer um índice fora dos limites válidos do texto (negativo, ou maior que o comprimento da string), o que lança uma exceção em tempo de execução (`StringIndexOutOfBoundsException`), assim como acontecia com índices inválidos em arrays. Ao trabalhar com posições calculadas dinamicamente (como o resultado de `indexOf`, que pode devolver `-1` quando o trecho buscado não é encontrado), é importante validar esse valor antes de usá-lo em `substring`, para evitar que um `-1` não tratado propague o erro adiante.

### `contains`

`contains` verifica se uma `String` contém, em algum lugar dentro dela, uma determinada sequência de caracteres, devolvendo um valor `boolean`: `true` se a sequência aparece em qualquer posição do texto, `false` caso contrário. É a forma mais direta de responder à pergunta "esse texto aparece dentro deste outro texto?", sem se importar com a posição exata em que isso acontece.

```java
String frase = "O livro de Java é introdutório";

System.out.println(frase.contains("Java"));       // true
System.out.println(frase.contains("Python"));     // false
System.out.println(frase.contains("java"));       // false, diferencia maiúsculas
```

Internamente, `contains` pode ser entendido como equivalente a verificar se `indexOf` (visto anteriormente) devolve um valor diferente de `-1` — de fato, é exatamente assim que `contains` costuma ser implementado por trás dos panos —, mas `contains` expressa a intenção do código de forma mais direta e legível quando a única informação necessária é "existe ou não", sem interesse pela posição exata. Escrever `if (frase.contains("Java"))` comunica a intenção de forma mais clara do que `if (frase.indexOf("Java") != -1)`, mesmo os dois produzindo o mesmo resultado.

Na prática, isso aparece bastante ao filtrar uma lista de itens (nomes de arquivos, produtos, mensagens de log) que contenham uma determinada palavra-chave — por exemplo, verificar se uma linha de log contém a palavra `"ERRO"` antes de destacá-la, ou se um nome de arquivo contém a extensão `".java"` antes de processá-lo. Assim como `equals`, `contains` diferencia maiúsculas de minúsculas por padrão; quando essa distinção não é desejada, uma abordagem comum é normalizar ambos os textos com `toLowerCase()` antes de comparar, já que não existe uma variante `containsIgnoreCase` pronta na classe `String`.

Vale notar que `contains` também lança `NullPointerException` se for chamado com `null` como argumento, e que uma string vazia (`""`) é considerada, por convenção, como contida em qualquer texto, inclusive em outra string vazia — comportamento coerente com `indexOf("")` devolver sempre `0`.

### `replace`

`replace` devolve uma nova `String` na qual todas as ocorrências de um trecho de texto são substituídas por outro trecho especificado — o texto original, como sempre em `String`, permanece intocado, e o método devolve o resultado como um novo objeto.

```java
String frase = "Java é fácil, Java é poderoso";

String resultado = frase.replace("Java", "Kotlin");
System.out.println(resultado); // "Kotlin é fácil, Kotlin é poderoso"
System.out.println(frase);     // "Java é fácil, Java é poderoso" (inalterado)
```

Diferente do que o nome poderia sugerir para quem vem de outras ferramentas, `replace` substitui *todas* as ocorrências do trecho buscado, não apenas a primeira — como visto no exemplo acima, ambas as ocorrências de `"Java"` foram trocadas. `replace` também tem uma variante que aceita um único caractere (`char`) no lugar de uma sequência inteira, útil para trocas simples, como substituir todos os espaços por hífens (`texto.replace(' ', '-')`).

Um cenário típico de aplicação é a normalização de um texto antes de processá-lo — por exemplo, remover formatação de um número de telefone antes de validá-lo ou armazená-lo, trocando parênteses, espaços e hífens por texto vazio:

```java
String telefone = "(11) 98888-7777";
String somenteDigitos = telefone.replace("(", "")
                                 .replace(")", "")
                                 .replace(" ", "")
                                 .replace("-", "");
System.out.println(somenteDigitos); // "11988887777"
```

Esse exemplo também ilustra um padrão comum ao trabalhar com métodos de `String`: como cada chamada devolve um novo objeto `String`, é possível encadear várias chamadas em sequência (chamado de *method chaining*), cada uma operando sobre o resultado da anterior, em vez de criar uma variável intermediária para cada etapa — tornando o código mais direto quando várias transformações precisam ser aplicadas em sequência.

Existe ainda `replaceAll` e `replaceFirst`, que aceitam expressões regulares (um mini-linguagem para descrever padrões de texto mais complexos do que uma sequência fixa) no lugar do trecho a ser buscado — um tópico que foge do escopo introdutório deste módulo, mas que vale saber que existe para buscas mais sofisticadas do que uma substituição literal.

### `split`

`split` divide uma `String` em várias partes, usando um separador informado como critério de corte, e devolve o resultado como um array de `String` — cada posição do array correspondendo a um trecho do texto original, entre uma ocorrência do separador e a seguinte.

```java
String csv = "João,25,São Paulo";
String[] campos = csv.split(",");

System.out.println(campos.length);   // 3
System.out.println(campos[0]);       // "João"
System.out.println(campos[1]);       // "25"
System.out.println(campos[2]);       // "São Paulo"
```

Esse é um dos usos mais comuns de `String` no dia a dia: interpretar uma linha de texto que segue um formato estruturado e simples, como um arquivo CSV (valores separados por vírgula), uma frase que precisa ser quebrada em palavras individuais (`frase.split(" ")`), ou uma data no formato `"dd/mm/aaaa"` (`data.split("/")`). Combinar `split` com o laço `for` sobre arrays, já visto no módulo anterior, permite processar cada parte do texto individualmente:

```java
String frase = "o livro de java é introdutório";
String[] palavras = frase.split(" ");

for (String palavra : palavras) {
    System.out.println(palavra.toUpperCase());
}
```

Um detalhe importante: o argumento de `split`, tecnicamente, também é interpretado como uma expressão regular, não como um texto literal fixo — o que raramente muda o resultado para separadores simples como `","` ou `" "`, mas pode surpreender com caracteres que têm significado especial em expressões regulares, como o ponto (`.`), que precisa ser "escapado" (`split("\\.")`) para ser tratado literalmente ao dividir, por exemplo, um endereço IP como `"192.168.0.1"`.

Outro ponto que merece atenção é o comportamento com separadores repetidos ou nas extremidades do texto: dividir `"a,,b"` por vírgula produz `["a", "", "b"]`, com um elemento vazio no meio, refletindo a ausência de conteúdo entre as duas vírgulas consecutivas; e trechos vazios ao final do texto (mas não no início) são, por padrão, descartados do resultado. Esse comportamento raramente precisa de ajuste fino em casos simples, mas é bom saber que existe para não se surpreender ao lidar com dados de entrada menos regulares do que o esperado, como um CSV exportado de forma inconsistente.

### `trim`

`trim` devolve uma nova `String` com os espaços em branco removidos apenas das extremidades do texto — início e fim —, mantendo intactos quaisquer espaços que existam no meio do conteúdo. É um dos métodos mais usados ao processar texto vindo de fontes externas, como entrada de usuário ou leitura de arquivo, onde espaços indesejados no começo ou no final são extremamente comuns.

```java
String entrada = "   Maria Silva   ";
String limpo = entrada.trim();

System.out.println("[" + entrada + "]"); // [   Maria Silva   ]
System.out.println("[" + limpo + "]");   // [Maria Silva]
```

O uso dos colchetes no exemplo acima é um truque didático comum para visualizar espaços que, de outra forma, seriam invisíveis na saída do console. Note que os espaços entre "Maria" e "Silva", no meio do texto, não são afetados por `trim` — apenas os espaços nas bordas são removidos.

Um cenário real muito comum é validar ou comparar uma entrada digitada por um usuário em um terminal ou formulário: é fácil que o usuário, sem perceber, digite um espaço extra antes ou depois do texto (por exemplo, ao copiar e colar de outro lugar), o que faria uma comparação com `equals` falhar silenciosamente mesmo com o conteúdo "certo" visualmente. Aplicar `trim()` antes de validar ou armazenar a entrada elimina essa fonte comum de erro:

```java
Scanner leitor = new Scanner(System.in);
String resposta = leitor.nextLine().trim();

if (resposta.equalsIgnoreCase("sim")) {
    System.out.println("Confirmado!");
}
```

Tecnicamente, `trim` remove das extremidades qualquer caractere cujo código Unicode seja menor ou igual ao espaço comum (código 32) — o que cobre o espaço em si, mas também tabulações (`\t`) e quebras de linha (`\n`), caso apareçam ali. Esse critério, definido há muito tempo na linguagem, é suficiente para a esmagadora maioria dos casos práticos; a próxima seção apresenta `strip`, um método mais recente que amplia esse critério e por isso costuma ser preferido em código novo.

### `strip`

`strip` cumpre o mesmo papel de `trim` — remover espaços em branco das extremidades de uma `String`, preservando o conteúdo interno —, mas foi adicionado mais recentemente à linguagem como uma versão atualizada e mais correta do ponto de vista de internacionalização. Enquanto `trim`, como visto na seção anterior, reconhece como "espaço em branco" apenas caracteres com código igual ou inferior ao espaço comum, `strip` usa a definição mais ampla e moderna de espaço em branco do padrão Unicode, capaz de reconhecer corretamente certos espaços especiais usados em outros idiomas e sistemas de escrita que `trim` não identificaria.

```java
String texto = "   Livro de Java   ";

System.out.println("[" + texto.strip() + "]");        // [Livro de Java]
System.out.println("[" + texto.stripLeading() + "]");  // [Livro de Java   ]
System.out.println("[" + texto.stripTrailing() + "]"); // [   Livro de Java]
```

Além de `strip`, que remove espaços de ambas as extremidades (assim como `trim`), existem as variantes `stripLeading`, que remove apenas os espaços do início do texto, preservando os do final, e `stripTrailing`, que faz o inverso — útil quando o programa precisa de controle mais fino sobre qual lado do texto deve ser "limpo", algo que `trim` não oferece diretamente.

Na prática do dia a dia, para a esmagadora maioria dos textos em português ou em outros idiomas ocidentais comuns, `trim` e `strip` produzem exatamente o mesmo resultado, já que espaços comuns, tabulações e quebras de linha são reconhecidos igualmente por ambos. A diferença só se manifesta em casos específicos envolvendo caracteres Unicode menos comuns. Ainda assim, como `strip` é a adição mais recente e tecnicamente mais correta, ele é geralmente preferido em código novo, sendo `trim` mantido principalmente por compatibilidade com código já existente escrito antes de `strip` existir na linguagem. Para o nível introdutório deste livro, a escolha entre um e outro raramente muda o comportamento observado, mas vale conhecer `strip` como a opção mais atual — e como um exemplo de como a linguagem evolui, adicionando alternativas mais robustas sem remover as anteriores, para não quebrar código já escrito.

## Construção eficiente

Juntar textos com o operador `+` é simples e perfeitamente adequado para muitos casos. O problema aparece quando essa construção acontece repetidamente — por exemplo, dentro de um loop que acrescenta centenas ou milhares de trechos — porque `String` é imutável: cada alteração aparente cria um novo objeto, e o custo dessas criações pode se acumular. Para entender quando isso importa, é preciso relacionar concatenação, imutabilidade e desempenho.

Este capítulo faz essa transição da solução mais simples para a ferramenta adequada a construções intensivas de texto. Primeiro será retomada a concatenação comum; depois entra o `StringBuilder`, um objeto mutável criado justamente para montar textos progressivamente sem produzir uma nova `String` a cada passo. A discussão de performance será mantida em nível básico, suficiente para o leitor reconhecer quando cada abordagem faz sentido.

### Concatenação

Concatenar strings significa juntar duas ou mais delas, formando um texto único e maior. A forma mais direta e comum de fazer isso em Java é com o operador `+`, o mesmo símbolo usado para soma entre números, mas que, quando pelo menos um dos operandos é uma `String`, passa a significar concatenação de texto em vez de soma aritmética:

```java
String nome = "Maria";
int idade = 28;

String mensagem = "Olá, " + nome + "! Você tem " + idade + " anos.";
System.out.println(mensagem); // Olá, Maria! Você tem 28 anos.
```

Um detalhe útil desse comportamento: quando o `+` encontra uma `String` de um lado e um valor de outro tipo do outro lado (como o `int idade` no exemplo acima), Java converte automaticamente esse outro valor para sua representação em texto antes de concatenar — não é necessário chamar manualmente nenhum método de conversão para "colar" um número dentro de uma frase. A concatenação também pode ser feita com `+=`, útil para ir construindo um texto aos poucos, como em `mensagem += "!"`.

Como visto no capítulo sobre imutabilidade, cada operação de concatenação com `+` não altera nenhuma das strings envolvidas — ela cria um objeto `String` totalmente novo, contendo a junção do conteúdo de ambas. Para uma única concatenação, ou poucas, isso não representa problema algum, e o código com `+` é o mais simples e legível para o caso comum. O problema aparece quando a concatenação acontece repetidamente dentro de um laço, construindo um texto grande aos poucos:

```java
String resultado = "";
for (int i = 1; i <= 1000; i++) {
    resultado = resultado + i + ", ";
}
```

Nesse padrão, cada volta do laço descarta o objeto `String` anterior e cria um novo, um pouco maior, copiando todo o conteúdo já acumulado além do trecho novo — a 1000ª iteração recria, do zero, um texto quase inteiro só para acrescentar um pedaço pequeno ao final. Para poucas iterações isso passa despercebido, mas conforme o número de repetições cresce, esse padrão se torna sensivelmente mais custoso do que precisaria ser, porque o trabalho de recriar o texto inteiro se repete a cada volta. É exatamente esse cenário — construção de texto em muitas etapas sucessivas, tipicamente dentro de laços — que motiva o uso de `StringBuilder`, tratado na próxima seção, como alternativa mais eficiente à concatenação repetida com `+`.

### `StringBuilder`

`StringBuilder` é uma classe da biblioteca padrão de Java projetada especificamente para construir um texto de forma eficiente ao longo de várias etapas. Diferente de uma `String` comum, ele é mutável — a próxima seção detalha o que isso significa na prática e por que faz diferença.

```java
StringBuilder construtor = new StringBuilder();

for (int i = 1; i <= 5; i++) {
    construtor.append(i).append(", ");
}

String resultado = construtor.toString();
System.out.println(resultado); // "1, 2, 3, 4, 5, "
```

O método `append` acrescenta conteúdo ao final do texto já armazenado no `StringBuilder`, modificando o próprio objeto em vez de criar um novo a cada chamada — e, como o próprio método devolve o `StringBuilder` já atualizado, é possível encadear várias chamadas de `append` em sequência, como no exemplo acima (`construtor.append(i).append(", ")`). Ao final da construção, o método `toString()` converte o conteúdo acumulado em um objeto `String` comum, pronto para ser usado como qualquer outro texto — impresso, comparado, armazenado.

Internamente, `StringBuilder` mantém um espaço de memória (um array de caracteres, na prática) reservado com uma capacidade um pouco maior do que o conteúdo atual exige, de forma que `append` normalmente só precisa escrever o novo trecho nesse espaço já reservado, sem precisar copiar todo o conteúdo anterior — só quando esse espaço reservado se esgota é que o `StringBuilder` precisa expandir sua capacidade internamente (o que também tem um custo, mas ocorre com muito menos frequência do que uma recriação a cada `append`). É esse mecanismo que torna `StringBuilder` sensivelmente mais eficiente do que concatenação repetida com `+` dentro de um laço, especialmente quando o número de etapas de construção é grande.

Além de `append`, `StringBuilder` oferece outros métodos úteis para manipular o conteúdo em construção, como `insert` (inserir texto em uma posição específica), `delete` (remover um trecho), `reverse` (inverter a ordem dos caracteres) e `replace` (substituir um trecho por outro) — todos operando diretamente sobre o mesmo objeto, sem criar cópias intermediárias.

Um exemplo real de uso é montar dinamicamente uma mensagem de log ou um relatório de texto a partir de várias partes calculadas ao longo da execução de um programa, como percorrer uma lista de erros encontrados e ir acumulando uma descrição de cada um em um único texto final, sem saber de antemão quantos erros existirão nem qual será o tamanho final do texto — cenário exatamente equivalente ao do laço de concatenação visto na seção anterior, mas agora resolvido de forma eficiente.

### Mutabilidade

Como adiantado na seção anterior, a diferença central entre `StringBuilder` e `String` — e o motivo de sua existência — é a mutabilidade: um objeto `StringBuilder` pode ter seu conteúdo alterado diretamente, sem que isso implique criar um novo objeto a cada mudança. Métodos como `append`, `insert` e `delete`, vistos na seção anterior, modificam o próprio objeto sobre o qual são chamados — e é justamente por isso que, diferente de `String`, chamar um desses métodos sem capturar nenhum retorno ainda produz efeito visível:

```java
StringBuilder sb = new StringBuilder("Java");
sb.append(" é divertido");
System.out.println(sb); // "Java é divertido", mesmo sem reatribuir sb
```

Note que, ao contrário do padrão visto com métodos de `String` (onde era necessário escrever `texto = texto.toUpperCase();` para capturar o resultado), aqui `sb.append(" é divertido");`, sozinho, já é suficiente — o objeto `sb` referenciado pela variável foi de fato alterado por dentro, e a mesma variável continua apontando para ele, agora com conteúdo diferente.

Essa mutabilidade tem um custo em termos de cuidado no design de um programa: como um `StringBuilder` pode ser alterado por qualquer trecho de código que tenha uma referência a ele, compartilhar o mesmo objeto `StringBuilder` entre partes diferentes de um programa — por exemplo, passando-o como argumento para vários métodos diferentes — exige atenção a quem pode estar modificando seu conteúdo e quando, algo que simplesmente não é uma preocupação com `String`, justamente por sua imutabilidade, como visto no capítulo anterior deste módulo. Essa é a razão pela qual `String` continua sendo o tipo padrão e recomendado para representar texto na maior parte do código Java — variáveis, parâmetros de método, retornos —, com `StringBuilder` reservado especificamente para o momento pontual de construção de um texto, sendo descartado (ou convertido para `String` via `toString()`) assim que essa construção termina.

Em outras palavras, `StringBuilder` não é um substituto de `String` para uso geral, mas sim uma ferramenta auxiliar, de vida curta e propósito específico, usada internamente durante a montagem de um texto e depois convertida para uma `String` comum e imutável, que é então o que efetivamente circula pelo restante do programa — armazenada em variáveis, comparada, retornada por métodos, exibida ao usuário.

### Performance básica

Entender quando preferir `StringBuilder` a concatenação com `+` é, no fundo, uma questão de reconhecer o padrão: uma única concatenação, ou um número pequeno e fixo delas, não justifica a complexidade adicional de um `StringBuilder` — o código com `+`, mais simples e direto, é preferível nesses casos, e a diferença de desempenho entre as duas abordagens é irrelevante na prática. O ponto de virada é a construção de texto dentro de um laço, especialmente um laço cujo número de repetições pode ser grande ou não é conhecido de antemão.

```java
// Prática a evitar para laços grandes
String texto = "";
for (int i = 0; i < n; i++) {
    texto = texto + dados[i] + "\n";
}

// Prática recomendada
StringBuilder sb = new StringBuilder();
for (int i = 0; i < n; i++) {
    sb.append(dados[i]).append("\n");
}
String texto = sb.toString();
```

Como visto no capítulo sobre concatenação, cada `texto = texto + ...` dentro do laço recria o texto inteiro do zero, copiando tudo que já havia sido acumulado além do trecho novo — um custo que cresce junto com o tamanho do texto já construído, tornando o laço inteiro significativamente mais lento à medida que `n` aumenta, comparado à versão com `StringBuilder`, que reaproveita o mesmo espaço de memória reservado, evitando essa cópia repetida na maioria das voltas do laço.

Vale um esclarecimento importante: o compilador de Java, em muitos casos, já otimiza automaticamente uma sequência simples de concatenações escritas em uma única expressão — como `"Olá, " + nome + "! Você tem " + idade + " anos."`, do exemplo visto anteriormente — convertendo-a internamente em algo equivalente ao uso de `StringBuilder`, então esse tipo de concatenação direta, fora de um laço, não sofre o problema descrito aqui. O cuidado é especificamente com concatenação repetida *ao longo de múltiplas iterações de um laço*, onde essa otimização automática do compilador não se aplica da mesma forma, porque cada iteração é uma instrução separada, não uma única expressão que o compilador possa enxergar de uma vez e otimizar como um todo.

Como orientação prática para o dia a dia: usar `+` livremente para concatenações pontuais e legíveis fora de laços, e usar `StringBuilder` sempre que um texto for construído ao longo de repetições — mesmo sem medir o tempo de execução, essa é uma prática já consolidada na comunidade Java, reconhecida inclusive por ferramentas automáticas de análise de código, que costumam sinalizar concatenação de `String` dentro de laços como um ponto de atenção a corrigir.

## Formatação de texto

Nem todo texto é apenas armazenado ou recortado; muitas vezes ele precisa ser apresentado em um formato específico. Um programa pode precisar alinhar valores, inserir números dentro de uma mensagem, produzir textos com várias linhas ou representar caracteres que não podem ser digitados diretamente de forma comum dentro de uma string. A formatação é o conjunto de recursos que torna essa apresentação mais expressiva e controlada.

Neste capítulo, o leitor verá formas complementares de resolver esse problema: `String.format` e `formatted` para compor mensagens com marcadores de formato, text blocks para escrever textos multilinha com menos ruído visual e sequências de escape para representar caracteres especiais dentro de literais. Ao final, será possível construir saídas textuais mais legíveis tanto no código-fonte quanto para quem executa o programa.

### `String.format`

`String.format` é um método estático (chamado diretamente sobre a classe `String`, sem precisar de um objeto já existente) que constrói uma nova `String` a partir de um modelo de texto com marcadores especiais, substituídos pelos valores informados como argumentos adicionais — uma forma de montar frases com dados variáveis de forma mais legível do que encadear várias concatenações com `+`.

```java
String nome = "Maria";
int idade = 28;
double saldo = 1500.5;

String mensagem = String.format("Nome: %s, Idade: %d, Saldo: R$ %.2f", nome, idade, saldo);
System.out.println(mensagem); // Nome: Maria, Idade: 28, Saldo: R$ 1500,50
```

Cada marcador no texto-modelo começa com `%` e indica o tipo do valor a ser inserido naquela posição: `%s` para texto (`String`, mas também qualquer outro tipo, já que Java converte automaticamente para sua representação textual), `%d` para números inteiros, `%f` para números de ponto flutuante, entre outros. Os marcadores são substituídos, na ordem em que aparecem no texto-modelo, pelos argumentos passados após ele, também na ordem em que foram escritos — o primeiro marcador pelo primeiro argumento extra, o segundo pelo segundo, e assim por diante.

O marcador `%.2f`, usado no exemplo acima para `saldo`, ilustra um recurso importante: o número entre `%` e `f` controla a quantidade de casas decimais exibidas, arredondando o valor conforme necessário — sem esse controle, um `double` poderia ser exibido com uma quantidade de casas decimais inconsistente ou excessiva, algo especialmente indesejável ao exibir valores monetários, como no exemplo. Outros ajustes comuns incluem controlar a largura mínima de um campo (útil para alinhar colunas de texto, como em uma tabela impressa no console) e preencher números com zeros à esquerda.

Um uso real muito comum de `String.format` é gerar mensagens de saída consistentes e bem formatadas — recibos, relatórios, mensagens de log, ou qualquer texto que combine dados fixos com valores calculados em tempo de execução — sem depender de várias concatenações manuais com `+`, que tendem a ficar difíceis de ler e de ajustar conforme o número de valores combinados cresce. Comparado à concatenação, `String.format` também separa claramente a estrutura do texto (o modelo, com seus marcadores) dos valores que o preenchem, o que facilita revisar ou traduzir o texto do modelo sem se perder em meio a operadores `+` espalhados pelo código.

Vale notar que `String.format` não altera nada — como todo método relacionado a `String`, ele apenas devolve um novo objeto `String` com o resultado já formatado, que precisa ser capturado em uma variável ou usado diretamente, como em `System.out.println(String.format(...))`.

### `formatted`

`formatted` cumpre exatamente o mesmo papel de `String.format`, com a mesma sintaxe de marcadores (`%s`, `%d`, `%.2f`, e assim por diante), mas com uma diferença de forma: em vez de ser um método estático chamado sobre a classe `String`, passando o texto-modelo como argumento, `formatted` é chamado diretamente sobre o próprio texto-modelo, como se fosse mais um método comum de `String`, como `toUpperCase` ou `trim`.

```java
String nome = "Maria";
int idade = 28;

String comFormat = String.format("Nome: %s, Idade: %d", nome, idade);
String comFormatted = "Nome: %s, Idade: %d".formatted(nome, idade);

System.out.println(comFormat);     // Nome: Maria, Idade: 28
System.out.println(comFormatted);  // Nome: Maria, Idade: 28 (mesmo resultado)
```

As duas formas produzem exatamente o mesmo resultado — a escolha entre uma e outra é, na maior parte dos casos, uma questão de estilo e legibilidade, não de comportamento. `formatted` costuma ser considerado mais natural de ler quando o texto-modelo já está fixo e visível diretamente no código (como no exemplo acima), porque aproxima visualmente o modelo dos marcadores que ele contém do restante da expressão, no estilo "sujeito.método()" já familiar de outros métodos de `String`. `String.format`, por sua vez, é mais adequado quando o texto-modelo vem de uma variável, de uma constante nomeada, ou de outra fonte que não seja um literal fixo escrito ali mesmo — nesses casos, `variavelDoModelo.formatted(...)` é perfeitamente válido também, mas `String.format(variavelDoModelo, ...)` tende a comunicar a intenção de forma um pouco mais explícita para quem lê.

Por ser um método de instância mais recente na linguagem (adicionado bem depois de `String.format`, que já existe há muito mais tempo), `formatted` também se encaixa particularmente bem com os text blocks, tratados na próxima seção — o motivo fica claro lá. Fora essa diferença estilística e a naturalidade de encadeamento em certos contextos, não há nenhuma distinção prática relevante entre os dois para o nível deste livro — dominar os marcadores de formatação (`%s`, `%d`, `%.2f`, entre outros) é o que realmente importa, já que esse conhecimento se aplica igualmente às duas formas de chamá-los.

### Text blocks

Text blocks (blocos de texto) são uma forma de escrever uma `String` que ocupa várias linhas no código-fonte de forma direta e legível, sem precisar concatenar várias linhas menores com `+` e sem precisar inserir manualmente o caractere de quebra de linha (`\n`) entre elas. Um text block é delimitado por três aspas duplas (`"""`) no início e no fim, em vez das aspas simples usadas para uma `String` comum de uma linha só:

```java
String comConcatenacao = "Prezado cliente,\n" +
                          "\n" +
                          "Seu pedido foi confirmado.\n" +
                          "Obrigado pela preferência.";

String comTextBlock = """
        Prezado cliente,

        Seu pedido foi confirmado.
        Obrigado pela preferência.""";

System.out.println(comTextBlock);
```

Os dois exemplos acima produzem o mesmo conteúdo textual, mas o text block dispensa completamente os `\n` explícitos e os `+` de concatenação entre linhas — cada quebra de linha real no código-fonte, dentro do bloco, já se torna uma quebra de linha no texto resultante. Isso torna text blocks especialmente úteis para representar, diretamente no código, textos que naturalmente ocupam várias linhas: mensagens de e-mail, trechos de HTML ou JSON de exemplo, consultas SQL, ou qualquer texto extenso onde a legibilidade da formatação original (com suas quebras de linha visíveis) importa para quem lê o código.

Um detalhe de sintaxe que merece atenção é a indentação: Java calcula automaticamente, a partir da posição das aspas triplas de fechamento, qual parte da indentação usada no código-fonte é apenas para manter o código organizado (e deve ser descartada) e qual faz parte do conteúdo real do texto — permitindo escrever o text block alinhado de forma natural com o restante do código, sem que espaços de indentação indesejados apareçam no resultado final.

Text blocks podem ser combinados livremente com os recursos já vistos neste módulo: é possível chamar `.formatted(...)` diretamente ao final de um text block para preencher marcadores dentro dele, ou aplicar métodos como `strip()` ou `replace()` sobre o resultado, exatamente como sobre qualquer outra `String` — afinal, um text block, uma vez processado pelo compilador, produz um objeto `String` comum, idêntico em tudo ao que seria produzido por uma `String` de uma linha só com o mesmo conteúdo. A diferença entre os dois existe apenas no código-fonte, como uma forma mais confortável de escrever texto multilinha; em tempo de execução, não há distinção alguma entre uma `String` criada por um text block e uma criada de qualquer outra forma.

### Escape sequences

Alguns caracteres são difíceis ou impossíveis de escrever diretamente dentro de uma `String`, seja porque já têm um significado especial na sintaxe de Java (como a própria aspa dupla, que delimita o início e o fim do texto), seja porque não são caracteres visíveis (como uma quebra de linha ou uma tabulação). É para esses casos que existe a escape sequence (sequência de escape): uma combinação de caracteres, sempre iniciada por uma barra invertida (`\`), usada dentro de uma `String` para representar esse tipo de caractere.

```java
String comAspas = "Ela disse: \"Olá!\"";
String comQuebraDeLinha = "Primeira linha\nSegunda linha";
String comTabulacao = "Nome:\tMaria";
String comBarraInvertida = "Caminho: C:\\Usuários\\Maria";

System.out.println(comAspas);          // Ela disse: "Olá!"
System.out.println(comQuebraDeLinha);  // duas linhas separadas
System.out.println(comTabulacao);      // Nome:    Maria
System.out.println(comBarraInvertida); // Caminho: C:\Usuários\Maria
```

O caso mais imediato é o das próprias aspas duplas: escrever uma aspa dupla como parte do conteúdo do texto (por exemplo, para reproduzir uma fala) exigiria que o compilador entendesse "isso é uma aspa de verdade, parte do texto, não o fim da string" — o que `\"` resolve, "escapando" o significado especial que a aspa dupla teria sozinha. Da mesma forma, como a própria barra invertida é o caractere usado para iniciar uma escape sequence, representar uma barra invertida literal no texto (como em um caminho de arquivo do Windows) exige escrevê-la duas vezes, `\\`, onde a primeira "escapa" a segunda.

`\n` representa uma quebra de linha, e `\t` representa uma tabulação — ambos caracteres que não têm uma tecla visível própria de forma direta no meio de um texto entre aspas, mas que fazem parte de praticamente qualquer texto real com mais de uma linha ou com necessidade de alinhamento simples em colunas. Vale notar que text blocks, vistos na seção anterior, tornam `\n` desnecessário para quebras de linha simples (já que a própria quebra de linha no código-fonte já produz esse efeito dentro de um text block), mas as demais escape sequences, como `\"` e `\t`, continuam válidas e úteis também dentro de text blocks, quando necessárias.

Outras escape sequences existem para casos mais específicos, como representar um caractere Unicode qualquer pelo seu código numérico (`\u` seguido de dígitos), mas `\"`, `\\`, `\n` e `\t` cobrem a grande maioria das situações práticas do dia a dia ao trabalhar com texto em Java, e são as que valem a pena memorizar neste nível introdutório.



# Módulo 8 — Classes e objetos

Os módulos anteriores ensinaram a manipular dados e organizar comportamentos em métodos, mas os exemplos ainda podiam ser vistos como partes relativamente soltas de um programa. Este módulo introduz a ideia que organiza praticamente todo o Java profissional: reunir dados e comportamentos relacionados em objetos, definidos a partir de classes. É aqui que o leitor começa a deixar de pensar apenas em variáveis e instruções e passa a representar entidades e responsabilidades do problema dentro do próprio código.

Os capítulos foram organizados para acompanhar essa mudança de raciocínio: primeiro se estabelece o que é um objeto e como estado, comportamento e identidade se relacionam; depois vem a criação efetiva de classes e instâncias; em seguida, os construtores mostram como objetos nascem em um estado válido; o encapsulamento ensina a proteger e controlar esse estado; e a modelagem inicial junta tudo em exemplos como cliente, produto e pedido. Ao final do módulo, o leitor conseguirá criar classes coerentes, instanciar objetos e começar a transformar situações do mundo real em modelos de software.

## O que é um objeto?

Até este ponto, o livro apresentou variáveis, métodos, arrays e strings como recursos que podem ser usados para resolver problemas. A orientação a objetos adiciona uma forma de organizar esses recursos: representar uma entidade por meio de um objeto que reúne os dados que descrevem sua situação atual e os comportamentos que ela pode executar. Antes de escrever classes, é importante entender essa ideia sem ficar preso à sintaxe.

Este capítulo constrói esse modelo mental a partir de quatro noções: estado, comportamento, identidade e classe como modelo. Elas explicam por que dois objetos podem ter os mesmos dados e ainda assim serem objetos distintos, por que métodos pertencem naturalmente às entidades sobre as quais atuam e como uma classe descreve o tipo de objeto que poderá ser criado. Essa base conceitual será usada diretamente nos capítulos seguintes.

### Estado

Todo objeto, em Java, carrega consigo um conjunto de informações que descrevem a situação em que ele se encontra em um dado momento — isso é o que chamamos de estado. Até aqui, neste livro, trabalhamos com variáveis soltas: um `int idade`, uma `String nome`, um `double saldo`, cada um vivendo de forma independente dentro de um método. O estado de um objeto é diferente: é um conjunto de variáveis que pertencem a ele, viajam junto com ele e continuam existindo enquanto o objeto existir, mesmo depois que o método que o criou já tiver terminado.

Pense em uma conta bancária. Sem o conceito de objeto, seria preciso manter várias variáveis soltas e sincronizadas manualmente — `saldoContaJoao`, `titularContaJoao`, `saldoContaMaria`, `titularContaMaria` — e a cada nova conta, mais um punhado de variáveis desconectadas entre si, sem nada que garanta que elas realmente pertencem à mesma conta. Não há como dizer "estas três variáveis, juntas, representam uma conta" — é só uma convenção de nomes que o programador precisa lembrar e manter.

Um objeto resolve isso agrupando essas variáveis dentro de uma mesma estrutura: uma conta bancária, como objeto, carrega seu próprio saldo e seu próprio titular como partes de si mesma, não como variáveis soltas que por acaso têm nomes parecidos. Cada conta criada tem seu próprio estado, independente do estado de qualquer outra conta:

```java
Conta contaDoJoao = new Conta("João", 1500.0);
Conta contaDaMaria = new Conta("Maria", 300.0);

contaDoJoao.depositar(200.0);
// o saldo da Maria continua 300.0, intocado
```

O estado de `contaDoJoao` (seu saldo, seu titular) é só dela; alterar um não afeta o outro objeto, porque cada objeto tem sua própria cópia dessas informações. É essa separação que faz sentido termos duas contas ao mesmo tempo no sistema sem que uma interfira na outra.

Vale notar que o estado de um objeto normalmente muda ao longo do tempo: um depósito altera o saldo, uma mudança de endereço altera um campo de endereço, e assim por diante. O objeto continua sendo "a mesma conta", mesmo que os valores que ela guarda internamente sejam diferentes de um instante para o outro — voltaremos a essa ideia de permanência ao falar de identidade, mais adiante neste capítulo. Por ora, o que importa reter é que estado é a soma das informações que um objeto guarda sobre si mesmo, e é ela que diferencia um objeto do outro mesmo quando ambos são construídos a partir do mesmo modelo.

### Comportamento

Se o estado descreve o que um objeto sabe sobre si mesmo, o comportamento descreve o que ele é capaz de fazer. Comportamento é o conjunto de ações que um objeto pode executar — normalmente ações que leem ou modificam o próprio estado dele. Continuando o exemplo da conta bancária: depositar, sacar e consultar o saldo são comportamentos de uma conta. Eles não existem soltos no programa; existem porque uma conta específica os executa, sobre o próprio estado dela.

Imagine a alternativa: uma função solta `depositar(saldoConta, valor)`, recebendo o saldo como parâmetro, fazendo a conta e devolvendo (ou tentando alterar) um valor externo a ela. Nada impede que essa função seja chamada com o saldo errado, de outra conta, por engano — lógica e dado ficam desconectados, cada um vivendo por conta própria, sem nada que os prenda um ao outro.

É exatamente essa desconexão que o objeto elimina: o comportamento vive dentro da própria classe, e ao ser executado, opera naturalmente sobre o estado daquele objeto específico, sem que ninguém precise passar o saldo como argumento manualmente:

```java
Conta contaDoJoao = new Conta("João", 1500.0);
contaDoJoao.depositar(200.0);
contaDoJoao.sacar(50.0);
System.out.println(contaDoJoao.consultarSaldo()); // 1650.0
```

Note que `depositar` e `sacar` não recebem o saldo como parâmetro — eles já sabem qual saldo alterar, porque estão sendo chamados sobre `contaDoJoao`. É como pedir a uma pessoa específica que faça algo, em vez de descrever a ação em abstrato e depois torcer para que ela seja aplicada à pessoa certa: dizer "João, deposite 200 reais" já deixa claro, pela própria forma como a frase é construída, em qual conta o dinheiro deve entrar.

O comportamento de um objeto é o que o torna ativo, e não apenas um amontoado de dados. Uma classe bem desenhada normalmente expõe como comportamento exatamente as operações que fazem sentido para aquele tipo de objeto — uma conta pode depositar e sacar, mas não faria sentido ela "imprimir um relatório de vendas", por exemplo, porque isso não é uma ação natural de uma conta bancária. Reconhecer que ações pertencem a um tipo de objeto e não a outro é parte central de desenhar bem uma classe, tema que será aprofundado no capítulo seguinte, sobre a criação de classes propriamente dita.

### Identidade

Identidade é o que faz de um objeto um objeto específico e diferente de qualquer outro, mesmo que dois objetos tenham exatamente o mesmo estado no momento da comparação. Imagine duas contas bancárias diferentes, ambas pertencentes a pessoas chamadas "João", ambas com saldo de R$ 1000,00 no exato mesmo instante. Elas têm o mesmo estado — mesmo nome, mesmo saldo — mas continuam sendo duas contas diferentes: um depósito em uma não afeta a outra, e cada uma tem sua própria existência dentro do programa.

Em Java, cada `new` cria um objeto distinto, com sua própria identidade, independentemente do que ele contém:

```java
Conta contaA = new Conta("João", 1000.0);
Conta contaB = new Conta("João", 1000.0);

System.out.println(contaA == contaB); // false
```

O operador `==`, quando aplicado a objetos (diferente do que ocorre com tipos primitivos como `int` ou `double`), compara identidade, não conteúdo — ele pergunta "essas duas variáveis apontam para o mesmo objeto na memória?", não "esses dois objetos têm os mesmos valores?". Como `contaA` e `contaB` foram criadas por dois `new` separados, são dois objetos diferentes, mesmo com estado idêntico, e por isso a comparação retorna `false`.

Isso tem uma consequência prática importante: uma variável de objeto, em Java, não guarda o objeto em si, mas uma referência a ele — algo como um endereço que diz onde encontrar aquele objeto específico. Se copiarmos essa referência para outra variável, as duas variáveis passam a apontar para o mesmo objeto, e alterações feitas através de uma são visíveis através da outra:

```java
Conta contaC = contaA;
contaC.depositar(500.0);

System.out.println(contaA.consultarSaldo()); // 1500.0, porque contaC é o mesmo objeto que contaA
```

Aqui não foi criado um novo objeto — `contaC` é apenas outro nome para o mesmo objeto que `contaA` já referenciava, daí a alteração aparecer nos dois. Sem identidade, não haveria como distinguir "a mesma conta, referenciada de dois jeitos" de "duas contas diferentes, que por acaso têm os mesmos dados" — e isso é essencial em qualquer sistema real, onde é preciso saber com certeza se uma operação está afetando o objeto certo.

A identidade de um objeto persiste ao longo de toda a vida dele, mesmo quando seu estado muda várias vezes: a conta do João continua sendo a mesma conta, identificável como tal, mesmo depois de dezenas de depósitos e saques que alteraram seu saldo repetidamente. É essa permanência de identidade, combinada com um estado que pode mudar, que caracteriza um objeto em Java.

### Classe como modelo

Uma classe é a planta, o molde, a partir do qual objetos são construídos. Ela descreve, em abstrato, quais informações um tipo de objeto vai carregar (seu estado) e quais ações ele vai ser capaz de executar (seu comportamento) — sem descrever nenhum objeto concreto em particular. `Conta` é uma classe: ela diz que toda conta bancária tem um titular e um saldo, e que toda conta sabe depositar, sacar e informar seu saldo. Mas a classe `Conta`, sozinha, não é nenhuma conta bancária real — ninguém consegue depositar dinheiro "na classe Conta".

A melhor analogia aqui é a planta de uma casa. Uma planta arquitetônica descreve onde ficam os cômodos, quantos quartos a casa tem, onde entra a luz — mas ninguém mora dentro da planta. É só depois que a planta é usada para construir uma casa de verdade, em um terreno específico, que existe algo onde alguém pode efetivamente morar. A mesma planta pode dar origem a muitas casas diferentes, em terrenos diferentes, cada uma existindo de forma independente das outras, mas todas seguindo a mesma estrutura básica descrita pela planta original.

Da mesma forma, a classe `Conta` pode dar origem a muitos objetos `Conta` diferentes — a conta do João, a conta da Maria, a conta de uma empresa — cada um com seu próprio estado (saldo e titular diferentes) e sua própria identidade, mas todos seguindo a mesma estrutura descrita pela classe: todos têm titular e saldo, todos sabem depositar e sacar, porque isso foi definido uma única vez, na classe.

```java
public class Conta {
    String titular;
    double saldo;

    void depositar(double valor) {
        saldo = saldo + valor;
    }
}
```

Esse trecho é a classe — o molde. Ele não representa nenhuma conta específica; representa a ideia geral de conta, válida para qualquer conta que venha a ser criada a partir dele. É só quando executamos `new Conta()` que nasce um objeto de verdade, com seu próprio `titular` e `saldo`, independente de qualquer outro objeto `Conta` que já exista no programa.

Essa distinção entre classe (o molde, definido uma única vez no código) e objeto (uma instância concreta daquele molde, podendo haver muitas ao mesmo tempo) é a base de tudo o que vem a seguir neste módulo. Os capítulos seguintes vão detalhar como escrever uma classe de fato — definindo seus campos e métodos — e como transformar esse molde em objetos reais através da instanciação.

## Criando classes

Depois de compreender conceitualmente o que é um objeto, o passo seguinte é representar essa ideia em código. Em Java, a classe é a declaração que define quais dados um determinado tipo de objeto pode guardar e quais operações ele pode realizar. Criar uma classe significa, portanto, descrever uma nova estrutura que passa a fazer parte do vocabulário do próprio programa.

Este capítulo mostra essa construção por meio de campos, métodos, instanciação e do operador `new`. O leitor verá a diferença entre definir uma classe e criar efetivamente objetos a partir dela, entendendo que uma única classe pode servir de modelo para várias instâncias independentes. Ao final, será possível sair de tipos prontos da linguagem e começar a criar tipos próprios para representar problemas reais.

### Campos

Campos são as variáveis declaradas diretamente dentro de uma classe, fora de qualquer método, e são elas que definem o estado que cada objeto daquela classe vai carregar. Se no capítulo anterior falamos de estado como conceito, campo é a forma concreta, em código Java, de declarar esse estado dentro de uma classe.

```java
public class Conta {
    String titular;
    double saldo;
}
```

Aqui, `titular` e `saldo` são campos da classe `Conta`. A diferença central entre um campo e uma variável local (daquelas declaradas dentro de um método, como vimos no Módulo 2) é o tempo de vida: uma variável local existe só enquanto o método que a declarou está em execução, e desaparece quando o método termina. Um campo, por outro lado, pertence ao objeto — ele nasce quando o objeto é criado e continua existindo, guardando seu valor, enquanto o objeto existir, mesmo que nenhum método esteja em execução no momento.

Sem campos, seria impossível um objeto "lembrar" de nada entre uma chamada de método e outra. Imagine se `saldo` fosse uma variável local dentro do método `depositar`: a cada chamada, ela seria recriada do zero, o depósito anterior seria esquecido, e a conta nunca acumularia valor algum — cada depósito começaria sempre do mesmo saldo inicial. É justamente por `saldo` ser um campo, e não uma variável local, que ele persiste entre uma chamada de `depositar` e a próxima, permitindo que o saldo realmente se acumule ao longo do tempo.

Cada objeto criado a partir da classe tem sua própria cópia de cada campo — isso já foi mencionado ao falar de estado e identidade, mas vale reforçar aqui, no nível do código: declarar `double saldo;` na classe não cria uma única variável `saldo` compartilhada por todas as contas; cria, para cada objeto `Conta` que vier a existir, um espaço próprio de memória reservado para guardar o saldo daquele objeto específico.

```java
public class Produto {
    String nome;
    double preco;
    int quantidadeEmEstoque;
}
```

Campos podem ser de qualquer tipo já visto neste livro — tipos primitivos como `int`, `double` e `boolean` (Módulo 2), ou tipos de referência como `String` (Módulo 7) e até outras classes definidas pelo próprio programador, como veremos ao tratar de relacionamentos entre objetos, no último capítulo deste módulo. Quando um campo não recebe um valor explícito no momento da criação do objeto, Java atribui a ele um valor padrão automaticamente: `0` para tipos numéricos, `false` para `boolean`, e `null` para tipos de referência como `String` — um valor especial que significa "nenhum objeto aqui ainda". O capítulo sobre construtores, mais adiante neste módulo, mostra a forma correta e controlada de dar valores iniciais aos campos no momento em que o objeto é criado, em vez de depender apenas desses valores padrão.

Escolher bem quais campos uma classe deve ter é uma das decisões mais importantes ao desenhá-la: cada campo deve representar uma informação que realmente faz parte do estado daquele tipo de objeto — nem informação demais (campos que nunca são usados, ou que pertencem a outro conceito) nem de menos (faltando algo que o comportamento da classe precisaria para funcionar corretamente).

### Métodos

Já usamos métodos ao longo de todo o livro, principalmente no Módulo 3, mas sempre como blocos de código soltos ou como o método `main`. Dentro de uma classe, eles ganham um papel adicional — e é essa forma que dá corpo, em código, ao que o capítulo anterior chamou de comportamento: quando declarados dentro de uma classe, os métodos operam naturalmente sobre os campos daquele mesmo objeto, sem precisar recebê-los como parâmetro.

```java
public class Conta {
    String titular;
    double saldo;

    void depositar(double valor) {
        saldo = saldo + valor;
    }

    void sacar(double valor) {
        saldo = saldo - valor;
    }

    double consultarSaldo() {
        return saldo;
    }
}
```

Repare que `depositar` acessa `saldo` diretamente, sem que `saldo` apareça na lista de parâmetros do método. Isso é possível porque, quando um método de objeto é chamado (como `contaDoJoao.depositar(200.0)`), Java já sabe, pelo próprio ponto em que o método foi chamado, sobre qual objeto ele deve operar — e portanto qual `saldo` específico alterar: o saldo da conta do João, não o de nenhuma outra conta. É essa ligação implícita entre o método e o objeto sobre o qual ele foi chamado que diferencia um método de classe de uma função solta que recebesse o saldo como argumento explícito.

Sem essa capacidade, toda operação sobre os dados de um objeto precisaria ser escrita como uma função solta, recebendo o estado inteiro do objeto como parâmetros separados, e devolvendo um novo estado — perdendo completamente a organização que vimos ao falar de estado e comportamento: os dados de um lado, as operações de outro, sem nada que amarre as duas coisas como pertencentes ao mesmo objeto.

Um método declarado dentro de uma classe segue as mesmas regras de assinatura já vistas no Módulo 3 — tipo de retorno, nome, lista de parâmetros entre parênteses, corpo entre chaves — a única diferença é o lugar onde ele é escrito (dentro da classe, e não solto) e o fato de ele poder acessar diretamente os campos daquela classe. Um método pode também receber parâmetros normalmente, como `valor` em `depositar(double valor)`, para representar informações que vêm de fora e não fazem parte do estado permanente do objeto — o valor a depositar não é algo que a conta "sabe" de antemão; é uma informação passada no momento da chamada.

Vale notar também que um método pode chamar outro método do mesmo objeto internamente, e que o corpo de um método tem acesso a todo o restante da linguagem já vista até aqui: condicionais (Módulo 4), laços (Módulo 5), arrays (Módulo 6) e manipulação de texto (Módulo 7) funcionam normalmente dentro do corpo de um método de classe, exatamente como funcionariam dentro do `main`. O que muda não é a linguagem disponível dentro do método, mas o contexto: agora o método tem, à sua disposição, os campos do objeto ao qual pertence.

### Instanciação

Uma classe, sozinha, não faz nada rodar: `public class Conta { ... }` apenas descreve como uma conta deveria ser — é o molde discutido no capítulo anterior. Instanciação é o nome do processo que dá o próximo passo: é o momento em que esse molde dá origem a algo real, que passa a existir na memória do programa, com estado e identidade próprios. É só através da instanciação que uma conta de verdade passa a existir, pronta para ser usada.

Um programador limitado a variáveis de tipos primitivos e, no máximo, `String`s e arrays não teria como agrupar estado e comportamento customizados como uma classe permite. Sem instanciação, não haveria como ter "várias contas diferentes, cada uma com seu próprio saldo" ao mesmo tempo dentro do mesmo programa, usando a mesma estrutura de código.

Instanciar em Java se faz com a palavra-chave `new`, que será detalhada em profundidade no próximo conceito deste capítulo — aqui o foco é entender o resultado desse processo, não ainda a sintaxe exata. O resultado de uma instanciação é um objeto novo, distinto de qualquer outro já existente, com seus campos inicializados (seja pelos valores padrão, seja pelos valores definidos em um construtor, tema do próximo capítulo) e pronto para que seus métodos sejam chamados sobre ele.

```java
Conta contaDoJoao = new Conta();
Conta contaDaMaria = new Conta();

contaDoJoao.titular = "João";
contaDaMaria.titular = "Maria";
```

Cada chamada a `new Conta()` produz um objeto independente. `contaDoJoao` e `contaDaMaria` são duas instâncias da mesma classe `Conta`, mas são dois objetos totalmente separados — o que muitas vezes é resumido dizendo que "instância" é sinônimo de "objeto": um objeto é sempre uma instância de alguma classe, e instanciar é o ato de produzir essa instância. É comum, na prática do dia a dia com Java, os termos "objeto" e "instância" serem usados de forma intercambiável quando o contexto já deixa claro de qual classe se está falando.

A variável à esquerda da atribrução (`contaDoJoao`) não é o objeto em si, mas uma referência a ele, como já mencionado ao falar de identidade — ela guarda o caminho até onde o objeto está na memória, não o objeto propriamente. Isso explica por que copiar essa variável para outra (como visto no exemplo `Conta contaC = contaA;`, no primeiro capítulo) não cria um novo objeto: apenas cria uma segunda forma de acessar o mesmo objeto já instanciado.

Um programa Java típico instancia muitos objetos ao longo de sua execução — às vezes centenas ou milhares — cada um vivendo de forma independente, sendo usado enquanto for necessário, e eventualmente deixando de ser referenciado por qualquer variável, momento em que a própria JVM (Módulo 1) se encarrega de liberar a memória que ele ocupava, um processo automático chamado coleta de lixo, que foge do escopo deste módulo introdutório mas que garante que o programador não precise gerenciar manualmente a memória usada por cada objeto instanciado.

### `new`

O conceito anterior descreveu o resultado da instanciação; falta ver a peça de sintaxe que efetivamente a realiza. É a palavra-chave `new`: o operador que, aplicado ao nome de uma classe seguido de parênteses, produz um objeto novo daquele tipo. A sintaxe básica é:

```java
Conta contaDoJoao = new Conta();
```

Aqui há duas partes distintas acontecendo na mesma linha, e vale a pena separá-las com clareza. `Conta contaDoJoao` declara uma variável do tipo `Conta` — isso, sozinho, apenas reserva um nome capaz de guardar uma referência a um objeto `Conta`, sem ainda apontar para nenhum objeto real (seu valor, se usado nesse ponto, seria `null`). Já `new Conta()`, do lado direito do `=`, é o que efetivamente cria o objeto: aloca espaço de memória para os campos da classe `Conta`, executa o construtor daquela classe (tema do próximo capítulo) e devolve uma referência ao objeto recém-criado. O `=` então atribui essa referência à variável `contaDoJoao`.

Sem o `new`, não existe forma de obter um objeto de uma classe definida pelo programador em Java — diferente de tipos primitivos, que já nascem prontos ao serem declarados (`int idade;` já é utilizável, mesmo que com valor padrão `0`), uma variável do tipo de uma classe precisa passar pelo `new` antes de apontar para algo real e utilizável com segurança.

```java
public class Produto {
    String nome;
    double preco;
}

Produto notebook = new Produto();
notebook.nome = "Notebook";
notebook.preco = 3500.0;

Produto mouse = new Produto();
mouse.nome = "Mouse";
mouse.preco = 45.0;
```

Como já vimos com `contaDoJoao` e `contaDaMaria` no conceito anterior, `new` nunca reaproveita um objeto existente: cada chamada aloca um objeto novo, mesmo que os campos preenchidos depois sejam idênticos aos de outro. Por isso `notebook == mouse` seria `false`.

`new` é usado tipicamente logo após a declaração do tipo da variável, como nos exemplos acima, mas também pode aparecer em outros contextos — como argumento direto de uma chamada de método, ou dentro de uma expressão maior — sempre com o mesmo efeito: produzir um objeto novo, ali, naquele ponto do código. É importante não confundir `new NomeDaClasse()` com uma simples chamada de método: apesar da aparência similar (parênteses após um nome), `new` é sempre seguido do nome de uma classe, e o que ele executa entre parênteses é, tecnicamente, o construtor daquela classe — um tipo especial de "método" dedicado exclusivamente a preparar o objeto recém-alocado antes de ele ser entregue ao restante do programa, assunto que o próximo capítulo detalha por completo.

## Construtores

Criar um objeto com `new` reserva e prepara uma nova instância, mas em programas reais normalmente não basta ter um objeto "vazio" e preencher seus dados depois de qualquer maneira. Muitas entidades já precisam nascer com informações essenciais — um produto com nome e preço, por exemplo — e pode ser importante garantir que essa configuração inicial aconteça de forma padronizada. É esse papel que os construtores cumprem.

Neste capítulo, o leitor aprenderá como a inicialização de objetos é controlada, o que acontece quando nenhum construtor é declarado, como oferecer mais de uma forma de construção por sobrecarga e como usar `this` para se referir ao próprio objeto e reaproveitar inicializações. Ao final, a criação de instâncias deixará de ser apenas um `new` e passará a ser entendida como a definição do estado inicial válido de cada objeto.

### Inicialização

Inicialização é o processo de dar aos campos de um objeto seus primeiros valores válidos, no exato momento em que ele é criado, garantindo que nenhum objeto comece a existir em um estado incompleto ou incoerente. Vimos, no capítulo anterior, que `new` cria um objeto e reserva espaço para seus campos — mas reservar espaço não é o mesmo que preencher esse espaço com valores que façam sentido para aquele objeto específico.

Sem uma forma dedicada de inicialização, um objeto recém-criado ficaria apenas com os valores padrão de Java: `0` para campos numéricos, `false` para `boolean`, `null` para `String` e outras referências. Uma conta bancária criada assim nasceria sem titular (`null`) e com saldo zero, mesmo que o programador já soubesse, no momento da criação, quem é o titular e qual deveria ser o saldo inicial:

```java
Conta contaDoJoao = new Conta();
// agora seria preciso lembrar de preencher cada campo manualmente:
contaDoJoao.titular = "João";
contaDoJoao.saldo = 1500.0;
```

Esse padrão tem dois problemas sérios. Primeiro, ele depende inteiramente da disciplina do programador que usa a classe: nada impede que alguém crie uma `Conta` e esqueça de definir o `titular`, deixando um objeto com `titular` igual a `null` circulando pelo sistema, e esse esquecimento só vai se manifestar como erro muito mais tarde, quando algum código tentar usar esse `titular` e falhar de forma inesperada. Segundo, ele expõe diretamente os campos do objeto para serem alterados de fora, de qualquer jeito, a qualquer momento — o que antecipa um problema que será tratado com mais profundidade no capítulo sobre encapsulamento, adiante neste módulo.

A forma correta de resolver isso em Java é o construtor: um bloco de código especial, definido dentro da própria classe, que é executado automaticamente sempre que `new` é usado, e cuja responsabilidade é justamente inicializar os campos do objeto antes de ele ser entregue pronto para uso:

```java
public class Conta {
    String titular;
    double saldo;

    Conta(String titularInicial, double saldoInicial) {
        titular = titularInicial;
        saldo = saldoInicial;
    }
}

Conta contaDoJoao = new Conta("João", 1500.0);
```

Agora não é mais possível criar uma `Conta` sem informar titular e saldo inicial — a própria sintaxe de `new Conta(...)` exige esses argumentos, porque foi assim que o construtor foi definido. O objeto nasce já em um estado válido e completo, sem depender de nenhum passo manual posterior. Essa garantia — de que todo objeto, assim que criado, já está em um estado coerente e utilizável — é um dos benefícios centrais de se trabalhar com classes bem construídas, e é a razão pela qual construtores existem como um mecanismo separado dos métodos comuns, mesmo compartilhando parte da sintaxe deles, como os próximos conceitos deste capítulo detalham.

### Construtor padrão

Toda classe em Java, mesmo que o programador não escreva nenhum construtor explicitamente, tem um construtor: se nenhum for declarado, Java fornece automaticamente um construtor padrão (também chamado de "construtor default"), sem parâmetros, cujo único efeito é criar o objeto com os campos em seus valores padrão de Java, sem realizar nenhuma inicialização customizada.

```java
public class Produto {
    String nome;
    double preco;
}

Produto p = new Produto();
System.out.println(p.nome);  // null
System.out.println(p.preco); // 0.0
```

Aqui, como a classe `Produto` não declarou nenhum construtor, `new Produto()` funciona porque Java oferece esse construtor padrão implicitamente, e o objeto resultante tem `nome` igual a `null` e `preco` igual a `0.0` — os valores padrão de `String` e `double`, respectivamente, exatamente como aconteceria com campos não inicializados discutidos no capítulo sobre campos.

O ponto importante — e uma armadilha comum para quem está aprendendo — é que esse construtor padrão só existe enquanto a classe não define nenhum construtor próprio. No momento em que o programador escreve pelo menos um construtor com parâmetros, como o `Conta(String titularInicial, double saldoInicial)` visto no conceito anterior, Java deixa de fornecer o construtor padrão automaticamente. Isso significa que, a partir daí, `new Conta()` sem argumentos deixa de compilar, a menos que o programador também tenha escrito, explicitamente, um construtor sem parâmetros:

```java
public class Conta {
    String titular;
    double saldo;

    Conta(String titularInicial, double saldoInicial) {
        titular = titularInicial;
        saldo = saldoInicial;
    }
}

Conta c = new Conta(); // erro de compilação: não existe construtor sem argumentos
```

Esse comportamento não é um defeito da linguagem, mas uma escolha deliberada: se a classe já define uma forma específica e completa de ser inicializada (com titular e saldo obrigatórios, no exemplo), normalmente não faz sentido também permitir que ela seja criada sem nenhuma dessas informações, correndo o risco de reintroduzir exatamente o problema que a inicialização por construtor foi criada para evitar — um objeto incompleto, com campos em valores padrão que não representam nenhuma conta real.

Quando um construtor sem parâmetros é realmente desejado, mesmo já existindo outros construtores com parâmetros na mesma classe, ele precisa ser escrito manualmente, lado a lado com os demais — o que nos leva diretamente ao próximo conceito, sobrecarga, que trata exatamente da possibilidade de uma classe ter mais de um construtor ao mesmo tempo, cada um servindo a uma forma diferente e igualmente válida de criar um objeto daquele tipo.

### Sobrecarga

Sobrecarga é a possibilidade de uma classe ter mais de um construtor (ou, de forma mais geral, mais de um método com o mesmo nome), desde que cada versão tenha uma lista de parâmetros diferente das demais — em quantidade de parâmetros, ou nos tipos deles. Java decide automaticamente, no momento da chamada, qual dessas versões usar, com base nos argumentos efetivamente fornecidos.

No conceito anterior, vimos que uma classe pode ficar "presa" a um único construtor com parâmetros obrigatórios, perdendo a opção de criação sem argumentos. Sobrecarga resolve isso oferecendo múltiplos caminhos de criação, cada um adequado a uma situação diferente, sem que nenhum precise sacrificar o outro:

```java
public class Conta {
    String titular;
    double saldo;

    Conta(String titularInicial, double saldoInicial) {
        titular = titularInicial;
        saldo = saldoInicial;
    }

    Conta(String titularInicial) {
        titular = titularInicial;
        saldo = 0.0;
    }
}

Conta contaDoJoao = new Conta("João", 1500.0);
Conta contaNova = new Conta("Maria"); // saldo inicial assumido como 0.0
```

Java escolhe automaticamente qual construtor executar comparando os argumentos passados em cada chamada com as listas de parâmetros disponíveis: `new Conta("João", 1500.0)` tem dois argumentos (uma `String` e um `double`), então casa com o primeiro construtor; `new Conta("Maria")` tem um único argumento, então casa com o segundo. Não é preciso — e nem seria possível — indicar explicitamente qual dos dois se quer usar; a própria forma da chamada já decide isso.

Um construtor pode, ainda, chamar outro construtor da mesma classe usando a palavra-chave `this` seguida de parênteses (uma forma de uso de `this` diferente da que o próximo conceito deste capítulo detalha, mas que vale mencionar aqui), evitando repetir lógica de inicialização entre os construtores sobrecarregados:

```java
Conta(String titularInicial) {
    this(titularInicial, 0.0);
}
```

Aqui, o construtor de um parâmetro simplesmente delega para o de dois parâmetros, passando `0.0` como saldo inicial — evitando duplicar a linha `titular = titularInicial;` em dois lugares diferentes. Essa chamada com `this(...)`, quando usada, precisa ser sempre a primeira instrução dentro do construtor.

Sobrecarga não se limita a construtores: métodos comuns também podem ser sobrecarregados da mesma forma, e o mesmo raciocínio de "Java decide pela lista de parâmetros" se aplica igualmente a eles — um exemplo já familiar é `System.out.println`, que aceita `int`, `double`, `String`, `boolean` e outros tipos, todos através de versões sobrecarregadas do mesmo método `println`, cada uma sabendo formatar seu próprio tipo de argumento para exibição.

O cuidado ao sobrecarregar é garantir que cada versão realmente represente uma forma sensata e diferente de fazer a mesma coisa — criar uma `Conta`, no exemplo — e não abusar da técnica a ponto de ter tantas variações que fique difícil, para quem lê o código, saber qual construtor está sendo chamado em cada ponto do programa.

### `this`

`this` é uma palavra-chave especial em Java que, dentro de um método ou construtor de uma classe, se refere ao próprio objeto sobre o qual aquele método ou construtor está sendo executado no momento. Ela resolve um problema de ambiguidade que surge com frequência ao escrever construtores: quando o nome de um parâmetro coincide com o nome do campo que ele deveria inicializar.

Nos exemplos anteriores deste capítulo, os parâmetros do construtor tinham nomes deliberadamente diferentes dos campos (`titularInicial` para inicializar `titular`, `saldoInicial` para inicializar `saldo`) exatamente para evitar essa ambiguidade. Mas é comum, e em geral mais natural de ler, que o parâmetro tenha o mesmo nome que o campo que representa:

```java
public class Conta {
    String titular;
    double saldo;

    Conta(String titular, double saldo) {
        titular = titular; // não faz o que parece fazer!
        saldo = saldo;
    }
}
```

Esse código compila, mas não funciona como se espera: dentro do construtor, como o parâmetro `titular` e o campo `titular` têm o mesmo nome, a instrução `titular = titular;` acaba se referindo ao próprio parâmetro dos dois lados — Java, ao encontrar um nome ambíguo entre parâmetro local e campo, prioriza o parâmetro local. O resultado é que o campo `titular` do objeto nunca é de fato preenchido, permanecendo `null`, o mesmo problema de inicialização incompleta que o capítulo sobre inicialização apresentou como algo a ser evitado.

`this` resolve exatamente essa ambiguidade, permitindo dizer explicitamente "o campo do objeto", em contraste com o parâmetro de mesmo nome:

```java
public class Conta {
    String titular;
    double saldo;

    Conta(String titular, double saldo) {
        this.titular = titular;
        this.saldo = saldo;
    }
}
```

Aqui, `this.titular` deixa claro que se trata do campo `titular` pertencente ao objeto sendo construído, enquanto `titular`, sozinho, à direita do `=`, se refere ao parâmetro recebido pelo construtor. A leitura de `this.titular = titular;` fica então: "o titular deste objeto recebe o valor do parâmetro titular" — exatamente o comportamento desejado, e este é, de longe, o uso mais comum de `this` no dia a dia de quem escreve classes em Java.

`this` também pode ser usado dentro de métodos comuns, não apenas em construtores, com o mesmo propósito de distinguir um campo de um parâmetro ou variável local de mesmo nome, embora nesses casos seja um pouco menos frequente, já que muitas vezes os parâmetros de métodos comuns já recebem nomes distintos dos campos, evitando a ambiguidade desde o início. Fora desse uso principal, `this` também aparece, como mencionado no conceito de sobrecarga, na forma `this(...)`, para um construtor chamar outro construtor da mesma classe — um uso mais específico, mas que se apoia na mesma ideia central: `this` sempre se refere ao objeto que está sendo criado ou manipulado naquele momento, nunca a um objeto qualquer ou a outra instância da mesma classe.

## Encapsulamento

Quando todos os dados de um objeto ficam livremente acessíveis, qualquer parte do programa pode alterá-los sem respeitar regras. Um saldo poderia ser definido como negativo de forma indevida, uma idade poderia receber um valor impossível ou dois campos que deveriam manter relação entre si poderiam ficar inconsistentes. Encapsulamento é a ideia de controlar o acesso ao estado interno para que o próprio objeto possa preservar suas regras.

Este capítulo apresenta essa proteção usando `private`, getters e setters, mas vai além da mecânica de criar métodos de acesso. O foco é mostrar como validações e invariantes transformam o objeto em responsável por manter a própria consistência. Ao final, o leitor deverá enxergar encapsulamento não como a obrigação de esconder campos e gerar getters e setters automaticamente, mas como uma ferramenta de desenho para impedir estados inválidos.

### `private`

`private` é um modificador de acesso em Java que restringe a visibilidade de um campo (ou método) apenas ao interior da própria classe onde ele foi declarado — nenhum código fora da classe consegue acessar ou alterar diretamente um campo marcado como `private`. Até este ponto do módulo, todos os campos dos exemplos foram declarados sem nenhum modificador, o que os deixa acessíveis livremente de fora da classe, como em `contaDoJoao.saldo = -500.0;` — uma linha que, tecnicamente, o compilador aceita sem reclamar, mesmo representando um saldo negativo sem qualquer verificação.

Esse acesso livre é o problema que `private` existe para resolver. Sem ele, qualquer parte do programa pode alterar diretamente o estado interno de um objeto, ignorando por completo qualquer lógica ou regra que a própria classe deveria impor sobre seus dados — nada garante, por exemplo, que um `saldo` permaneça sempre não-negativo, ou que um campo `email` sempre contenha algo no formato de um e-mail válido, se qualquer código externo pode simplesmente escrever `objeto.campo = qualquerValor;` sem passar por nenhuma verificação.

```java
public class Conta {
    private String titular;
    private double saldo;

    Conta(String titular, double saldo) {
        this.titular = titular;
        this.saldo = saldo;
    }

    void depositar(double valor) {
        saldo = saldo + valor;
    }
}

Conta contaDoJoao = new Conta("João", 1500.0);
contaDoJoao.saldo = -500.0; // erro de compilação: saldo tem acesso private
contaDoJoao.depositar(200.0); // permitido: depositar é acessado de fora, mas manipula saldo por dentro
```

Com `saldo` marcado como `private`, a linha `contaDoJoao.saldo = -500.0;` deixa de compilar — o campo simplesmente não pode ser acessado a partir de código fora da classe `Conta`. Mas isso não significa que o saldo se torne inacessível ou inutilizável: o método `depositar`, por ser declarado dentro da própria classe `Conta`, continua tendo acesso normal ao campo `saldo`, porque `private` restringe o acesso vindo de fora da classe, não o acesso de dentro dela.

Essa é a essência do encapsulamento, o tema deste capítulo: esconder os detalhes internos de como um objeto guarda seus dados, e obrigar qualquer interação externa a passar pelos métodos que a própria classe expõe deliberadamente — métodos que podem, então, aplicar validações e garantir regras antes de efetivamente alterar o estado interno. Os próximos conceitos deste capítulo (getters, setters e validação) mostram exatamente como oferecer, de forma controlada, o acesso que o mundo externo eventualmente precisa ter aos dados de um objeto, mesmo com os campos marcados como `private`.

Uma boa prática comum em Java, e que vale adotar desde já, é declarar todos os campos de uma classe como `private` por padrão, e só abrir acesso externo a eles de forma deliberada e controlada, através de métodos — nunca deixando um campo acessível diretamente de fora sem uma razão clara para isso.

### Getters

Getter é o nome dado, por convenção, a um método cuja única responsabilidade é devolver o valor de um campo `private`, permitindo que código externo à classe leia essa informação mesmo sem ter acesso direto ao campo. Como vimos no conceito anterior, marcar um campo como `private` bloqueia totalmente o acesso externo a ele — inclusive para leitura, não apenas para escrita — o que, sozinho, tornaria impossível, por exemplo, que outro trecho do programa descubra qual é o saldo atual de uma conta.

```java
public class Conta {
    private String titular;
    private double saldo;

    Conta(String titular, double saldo) {
        this.titular = titular;
        this.saldo = saldo;
    }

    double getSaldo() {
        return saldo;
    }

    String getTitular() {
        return titular;
    }
}

Conta contaDoJoao = new Conta("João", 1500.0);
System.out.println(contaDoJoao.getSaldo()); // 1500.0
```

O padrão de nomenclatura `getNomeDoCampo()` (com a primeira letra do nome do campo em maiúscula após o `get`) é uma convenção amplamente adotada na comunidade Java, não uma exigência da linguagem em si — o compilador aceitaria perfeitamente um método chamado `consultarSaldo()` fazendo a mesma coisa, como aliás apareceu em exemplos anteriores deste módulo. Mas seguir a convenção `getX()` torna o código mais previsível para quem já conhece Java, e é a forma esperada por diversas ferramentas e bibliotecas do ecossistema.

O ponto central de um getter é que ele expõe a leitura de um valor sem expor o campo em si para escrita externa: `contaDoJoao.getSaldo()` permite consultar o saldo, mas não existe forma, a partir dele, de alterar esse saldo diretamente — a única via de leitura não vira, de brinde, uma via de escrita. Isso preserva a proteção que `private` oferece, ao mesmo tempo em que disponibiliza a informação para quem precisa dela de fora da classe, resolvendo o problema de um encapsulamento total (sem getters) tornar um objeto praticamente inútil para o restante do programa, já que nada poderia ler seu estado.

Um getter, por ser um método comum, pode fazer mais do que apenas devolver o campo cru, embora a forma mais simples e mais comum seja exatamente essa. É possível, por exemplo, que um getter calcule algo a partir de mais de um campo antes de devolver o resultado — um `getSaldoFormatado()` que devolve o saldo já formatado como texto de moeda, por exemplo — mas isso já é uma variação sobre o padrão básico, e não muda a ideia central: um getter é o canal controlado, definido pela própria classe, por onde informações saem de um objeto para o mundo exterior, em contraste com um campo `private` acessado diretamente, que não daria à classe nenhuma chance de intermediar ou preparar esse valor antes de expô-lo.

### Setters

Setter é o análogo do getter para escrita: um método cuja responsabilidade é receber um novo valor e usá-lo para atualizar um campo `private`, oferecendo um canal controlado por onde código externo pode alterar o estado de um objeto, mesmo sem ter acesso direto ao campo correspondente. Se o getter resolve o problema de ler um dado que está trancado atrás de `private`, o setter resolve o problema equivalente para escrita: como alterar um dado que está protegido, sem simplesmente reabrir o campo para acesso livre?

```java
public class Conta {
    private String titular;
    private double saldo;

    Conta(String titular, double saldo) {
        this.titular = titular;
        this.saldo = saldo;
    }

    double getSaldo() {
        return saldo;
    }

    void setTitular(String novoTitular) {
        titular = novoTitular;
    }
}

Conta contaDoJoao = new Conta("João", 1500.0);
contaDoJoao.setTitular("João da Silva");
```

Assim como o getter, o setter segue uma convenção de nomenclatura amplamente adotada — `setNomeDoCampo(valor)` — e por convenção não devolve nenhum valor (`void`), já que sua função é apenas alterar o estado interno do objeto, não produzir um resultado para ser usado em seguida.

A diferença crucial entre um setter e simplesmente deixar o campo público para escrita é que o setter é um método, e um método pode conter lógica — pode verificar se o valor recebido é aceitável antes de efetivamente usá-lo para alterar o campo, pode rejeitar um valor inválido, pode ajustá-lo, pode até decidir não alterar nada em certas condições. Um campo público, por outro lado, aceita qualquer atribuição sem nenhuma chance de intervenção da classe.

Vale notar que nem todo campo `private` precisa necessariamente ganhar um setter. Um saldo bancário, por exemplo, normalmente não deveria ter um `setSaldo(double novoSaldo)` genérico, que permitiria trocar o saldo para qualquer valor arbitrário de uma vez — em vez disso, a classe `Conta`, como já vimos, oferece métodos mais específicos como `depositar` e `sacar`, que alteram o saldo de forma controlada e sempre em relação ao valor anterior, nunca substituindo-o livremente. Decidir quais campos merecem um setter simples, quais merecem métodos mais específicos como `depositar`, e quais não deveriam ser alteráveis de fora da classe (não tendo nenhum setter) é uma decisão de desenho de classe tão importante quanto decidir quais campos a classe deve ter, e é justamente esse tipo de decisão — o que pode e o que não pode mudar de fora de um objeto — que caracteriza um bom encapsulamento.

O próximo conceito deste capítulo, validação, detalha exatamente como um setter (ou qualquer método que altere o estado do objeto) pode usar sua natureza de método, em vez de simples atribuição direta, para impedir que o objeto entre em um estado inválido.

### Validação

Validação é a verificação, feita dentro de um método (tipicamente um construtor ou um setter), de que um valor recebido é aceitável antes de ele ser efetivamente usado para alterar o estado de um objeto — e a rejeição explícita desse valor, normalmente lançando uma exceção, quando ele não é aceitável. É aqui que o encapsulamento mostra por completo sua utilidade: de nada adiantaria esconder um campo atrás de `private` e de um setter, se o próprio setter aceitasse cegamente qualquer valor recebido, sem questionar nada.

```java
public class Conta {
    private String titular;
    private double saldo;

    Conta(String titular, double saldoInicial) {
        if (saldoInicial < 0) {
            throw new IllegalArgumentException("Saldo inicial não pode ser negativo");
        }
        this.titular = titular;
        this.saldo = saldoInicial;
    }

    void depositar(double valor) {
        if (valor <= 0) {
            throw new IllegalArgumentException("Valor de depósito deve ser positivo");
        }
        saldo = saldo + valor;
    }

    void sacar(double valor) {
        if (valor > saldo) {
            throw new IllegalArgumentException("Saldo insuficiente");
        }
        saldo = saldo - valor;
    }
}
```

Sem validação, uma `Conta` poderia ser criada com saldo inicial negativo, poderia receber um "depósito" de valor negativo (que na prática funcionaria como um saque disfarçado, sem passar pela verificação de saldo suficiente que `sacar` faz), ou poderia sacar mais dinheiro do que realmente possui, deixando o saldo negativo sem nenhum controle. Cada uma dessas situações representa o objeto entrando em um estado que não faz sentido para o que uma conta bancária deveria representar no mundo real.

A validação resolve isso colocando, no início de cada método que altera o estado, uma verificação explícita das condições que o novo valor precisa satisfazer, usando estruturas condicionais já conhecidas do Módulo 4. Quando a condição de invalidez é detectada, o método interrompe sua execução imediatamente através de `throw`, lançando uma exceção — um mecanismo de sinalização de erro que o programa chamador pode tratar (ou que, se não tratado, interrompe a execução com uma mensagem de erro clara) — sem chegar a executar a linha que alteraria o estado do objeto.

Isso significa que, com essas validações no lugar, é literalmente impossível — não apenas indesejável, mas impossível dentro das regras impostas pelo próprio código — criar uma `Conta` com saldo inicial negativo, ou fazer um saque maior que o saldo disponível, usando apenas os métodos que a classe expõe. Qualquer tentativa nesse sentido é barrada no próprio ponto onde o dado entraria no objeto, e não silenciosamente aceita para só causar problemas mais tarde, em algum outro lugar do programa que dependa daquele saldo estar correto.

Validação é, portanto, o mecanismo que efetivamente aproveita a proteção oferecida por `private` combinada com setters e construtores: não basta esconder os campos, é preciso também que os poucos pontos de entrada permitidos (construtor, setters, métodos como `depositar` e `sacar`) façam seu trabalho de guardiões, questionando cada valor recebido antes de aceitá-lo como novo estado do objeto.

### Invariantes

Invariante é uma condição sobre o estado de um objeto que deve permanecer sempre verdadeira, em qualquer momento da vida daquele objeto — antes e depois de qualquer método ser chamado sobre ele. "O saldo de uma conta nunca é negativo" é um exemplo de invariante da classe `Conta` que vimos ao longo deste capítulo: essa afirmação deveria ser verdade assim que a conta é criada, e deveria continuar verdade depois de qualquer sequência de depósitos e saques, não importa quantos nem em qual ordem.

Os conceitos anteriores deste capítulo — `private`, getters, setters e validação — são, juntos, as ferramentas usadas para garantir que uma invariante seja de fato mantida ao longo de toda a vida de um objeto. `private` impede que o campo `saldo` seja alterado por qualquer caminho que não passe pelos métodos da própria classe; validação, dentro desses métodos, impede que uma alteração inválida (como um saque maior que o saldo) chegue a ser efetivada. A combinação das duas coisas é o que transforma "o saldo nunca é negativo" de uma esperança informal em uma garantia real, imposta pelo próprio código.

```java
public class Conta {
    private double saldo;

    Conta(double saldoInicial) { /* valida saldoInicial >= 0, como já vimos em validação */ }

    void depositar(double valor) { /* valida valor > 0 */ }

    void sacar(double valor) { /* valida valor <= saldo */ }

    // toda alteração de saldo passa por um destes três pontos, e todos
    // preservam saldo >= 0 — a invariante nunca é quebrada
}
```

Pensar em termos de invariantes é útil como uma forma de verificar, sistematicamente, se uma classe está bem protegida: para cada campo, vale perguntar "existe alguma condição que este campo deveria sempre respeitar?" — e, se existir, garantir que todo método capaz de alterar aquele campo respeite essa condição antes de aplicar a mudança. Uma invariante quebrada em algum ponto do programa costuma se manifestar como um bug difícil de rastrear: o erro aparece muito depois, em algum código distante que assumia (corretamente, do ponto de vista de quem o escreveu) que "o saldo nunca é negativo", mas que na prática recebeu um objeto cuja invariante já havia sido violada em algum ponto anterior, por falta de uma validação.

Sem o conceito de invariante orientando o desenho da classe, é fácil escrever validações soltas e inconsistentes — validar o saldo inicial no construtor, mas esquecer de validar em `sacar`; ou validar em `sacar`, mas esquecer que um `setSaldo` genérico, se existisse, contornaria toda a proteção construída. Pensar primeiro na invariante ("o que precisa ser sempre verdade?") e só depois decidir quais métodos existem e o que cada um valida é uma ordem de raciocínio mais segura do que validar caso a caso sem uma visão de conjunto — e é exatamente esse tipo de raciocínio, aplicado a objetos que representam conceitos do mundo real, que o capítulo seguinte, sobre modelagem inicial, começa a exercitar de forma mais ampla.

## Modelagem inicial

Saber declarar classes isoladas é diferente de saber modelar um problema. Em um sistema real, as entidades se relacionam: um pedido pertence a um cliente, contém produtos e precisa distribuir responsabilidades entre diferentes objetos. Modelagem é o processo de decidir quais conceitos merecem virar classes, que dados pertencem a cada uma e qual objeto deve ser responsável por cada comportamento.

Este capítulo fecha o módulo aplicando os fundamentos anteriores a um pequeno domínio formado por cliente, produto e pedido. A partir desses exemplos, serão discutidos relacionamentos e responsabilidades, sem ainda entrar em técnicas avançadas de design. O objetivo é fazer o leitor começar a traduzir uma descrição de negócio em um conjunto coerente de objetos, em vez de simplesmente criar classes como recipientes de dados.

### Cliente

Modelar um `Cliente` como classe é um exercício direto de aplicar tudo o que este módulo cobriu até aqui a um conceito do mundo real, familiar a qualquer sistema de vendas ou cadastro. Um cliente, no mundo real, tem características que o identificam e o descrevem — nome, e-mail, talvez um documento de identificação — e é exatamente esse conjunto de informações que se torna o estado da classe, seguindo a mesma lógica vista no capítulo sobre criação de classes.

```java
public class Cliente {
    private String nome;
    private String email;
    private String cpf;

    Cliente(String nome, String email, String cpf) {
        if (nome == null || nome.isBlank()) {
            throw new IllegalArgumentException("Nome não pode ser vazio");
        }
        if (!email.contains("@")) {
            throw new IllegalArgumentException("E-mail inválido");
        }
        this.nome = nome;
        this.email = email;
        this.cpf = cpf;
    }

    String getNome() {
        return nome;
    }

    String getEmail() {
        return email;
    }

    void setEmail(String novoEmail) {
        if (!novoEmail.contains("@")) {
            throw new IllegalArgumentException("E-mail inválido");
        }
        email = novoEmail;
    }
}
```

Repare como esse exemplo reúne, de forma natural, quase tudo visto neste módulo: os campos são `private` (encapsulamento), o construtor garante que nenhum `Cliente` nasça com nome vazio ou e-mail em formato claramente inválido (validação, protegendo a invariante "todo cliente tem um e-mail com formato válido"), há um getter para consulta e um setter que revalida o e-mail antes de aceitar uma troca — reaproveitando, dentro do setter, a mesma verificação já usada no construtor.

Sem modelar `Cliente` como uma classe própria, um sistema precisaria manipular clientes através de variáveis soltas ou de arrays paralelos — um array de nomes, outro de e-mails, outro de CPFs, todos sincronizados pela posição, mas sem nenhuma garantia estrutural de que a posição 3 do array de nomes realmente corresponde à posição 3 do array de e-mails. Um `Cliente` como objeto elimina esse risco: cada instância carrega, coesamente, todas as informações de um único cliente, e a identidade de cada objeto (vista no primeiro capítulo deste módulo) já resolve, sozinha, o problema de "a que cliente esses dados pertencem".

Um detalhe que vale observar em `Cliente`: nem todo campo precisa de setter. Aqui, `nome` não tem um `setNome`, uma escolha razoável se o sistema modelado considerar que o nome de um cliente, uma vez cadastrado, não deveria mudar livremente (ou deveria passar por um processo mais cuidadoso do que uma simples troca) — enquanto `email`, mais sujeito a atualização legítima pelo próprio cliente, ganha um setter validado. Essa é exatamente a decisão de desenho discutida no capítulo sobre encapsulamento: nem todo campo `private` precisa ficar igualmente aberto para alteração externa, e cada classe deve refletir, através da presença ou ausência de setters, quais mudanças realmente fazem sentido para aquele tipo de objeto no domínio que ele representa.

### Produto

Um `Produto`, em um sistema de vendas, é outro candidato natural a virar classe, com estado próprio (nome, preço, quantidade em estoque) e um comportamento que precisa proteger certas invariantes específicas desse domínio — sobretudo a de que o preço não pode ser negativo, e a de que a quantidade em estoque não pode cair abaixo de zero.

```java
public class Produto {
    private String nome;
    private double preco;
    private int quantidadeEmEstoque;

    Produto(String nome, double preco) {
        if (preco < 0) {
            throw new IllegalArgumentException("Preço não pode ser negativo");
        }
        this.nome = nome;
        this.preco = preco;
        this.quantidadeEmEstoque = 0;
    }

    String getNome() {
        return nome;
    }

    double getPreco() {
        return preco;
    }

    int getQuantidadeEmEstoque() {
        return quantidadeEmEstoque;
    }

    void reporEstoque(int quantidade) {
        if (quantidade <= 0) {
            throw new IllegalArgumentException("Quantidade a repor deve ser positiva");
        }
        quantidadeEmEstoque += quantidade;
    }

    void retirarDoEstoque(int quantidade) {
        if (quantidade > quantidadeEmEstoque) {
            throw new IllegalArgumentException("Estoque insuficiente");
        }
        quantidadeEmEstoque -= quantidade;
    }
}
```

Note que esse construtor usa sobrecarga de forma implícita ao só receber `nome` e `preco`, deixando `quantidadeEmEstoque` sempre começar em zero — uma decisão de modelagem razoável, já que um produto recém-cadastrado no sistema tipicamente ainda não tem itens físicos no estoque; a entrada de itens acontece depois, através de `reporEstoque`. Se fizesse sentido também permitir cadastrar um produto já com uma quantidade inicial em estoque, um segundo construtor sobrecarregado (como visto no capítulo sobre construtores) resolveria isso sem quebrar o construtor mais simples já existente.

Repare também como `retirarDoEstoque` e `reporEstoque`, assim como `depositar` e `sacar` na classe `Conta` vista ao longo deste módulo, evitam expor um `setQuantidadeEmEstoque(int)` genérico: alterar a quantidade em estoque só faz sentido como uma operação relativa (somar ou subtrair uma quantidade), nunca como uma substituição arbitrária do valor atual por outro qualquer, e modelar os métodos dessa forma torna impossível, pela própria estrutura da classe, que o estoque seja definido para um valor sem relação com o histórico de entradas e saídas.

Um sistema de vendas raramente lida com um `Produto` isolado — ele existe para ser vendido, comprado, listado em um pedido. É justamente essa relação entre `Produto` e outras classes, como a que será vista no próximo conceito (`Pedido`), que mostra por que vale a pena ter `Produto` como uma classe própria e não apenas como um conjunto de variáveis soltas: um `Pedido` pode referenciar `Produto`s diretamente como parte do seu próprio estado, algo que arrays paralelos desconectados jamais representariam com a mesma clareza e segurança que um objeto oferece.

### Pedido

`Pedido` é um exemplo de classe cujo estado não é feito só de tipos simples como `String`, `double` ou `int`, mas inclui, como parte de si mesma, referências a outros objetos já modelados — no caso, um `Cliente` e um ou mais `Produto`s, ambos vistos nos dois conceitos anteriores. Isso é possível porque, em Java, um campo pode ser de qualquer tipo, inclusive de uma classe definida pelo próprio programador, exatamente como mencionado ao final do conceito sobre campos, no início deste módulo.

```java
public class Pedido {
    private Cliente cliente;
    private Produto produto;
    private int quantidade;

    Pedido(Cliente cliente, Produto produto, int quantidade) {
        if (quantidade <= 0) {
            throw new IllegalArgumentException("Quantidade deve ser positiva");
        }
        if (quantidade > produto.getQuantidadeEmEstoque()) {
            throw new IllegalArgumentException("Estoque insuficiente para este pedido");
        }
        this.cliente = cliente;
        this.produto = produto;
        this.quantidade = quantidade;
        produto.retirarDoEstoque(quantidade);
    }

    double calcularTotal() {
        return produto.getPreco() * quantidade;
    }

    String getResumo() {
        return cliente.getNome() + " comprou " + quantidade + "x " + produto.getNome();
    }
}
```

Repare como o construtor de `Pedido` valida a quantidade solicitada não apenas em relação a si mesma (deve ser positiva), mas em relação ao estado de outro objeto — consultando `produto.getQuantidadeEmEstoque()` antes de aceitar o pedido, e efetivamente retirando a quantidade do estoque do produto (`produto.retirarDoEstoque(quantidade)`) como parte da criação do pedido. Isso é a invariante do capítulo anterior operando entre objetos: um `Pedido` só deveria existir se o produto correspondente tinha estoque suficiente no momento da compra, e criar o pedido já reflete essa retirada, mantendo os dois objetos — `Produto` e `Pedido` — coerentes entre si.

Sem modelar `Pedido` como classe, essa lógica de negócio — verificar estoque, descontar do estoque, calcular o total, registrar quem comprou o quê — ficaria espalhada em algum lugar solto do programa, provavelmente repetida em vários pontos diferentes sempre que fosse necessário processar uma compra, sem um lugar único e claro que reúna as regras específicas de um pedido. Como classe, `Pedido` concentra essa responsabilidade: qualquer parte do sistema que precise criar um pedido passa, obrigatoriamente, pelo construtor de `Pedido`, herdando automaticamente todas as validações ali definidas.

Note ainda que `calcularTotal()` não guarda o preço do produto como um campo próprio de `Pedido` — ele consulta `produto.getPreco()` no momento do cálculo, através da referência ao objeto `Produto` que o pedido guarda, em vez de copiar esse valor para dentro de si. Essa escolha evita duplicar a informação de preço em dois lugares; o motivo mais profundo por trás dela — o fato de `Pedido` guardar uma referência ao `Produto`, não uma cópia independente dele — é justamente o que o próximo conceito, relacionamentos, examina de forma mais geral.

### Relacionamentos

Relacionamento é o nome dado, de forma geral, à situação em que uma classe guarda, entre seus campos, uma referência a objetos de outra classe — exatamente o que já apareceu no conceito anterior, com `Pedido` guardando referências a um `Cliente` e a um `Produto`. Este conceito fecha o módulo generalizando essa ideia: entender os tipos mais comuns de relacionamento entre classes ajuda a modelar sistemas mais próximos da complexidade real, além dos exemplos isolados vistos até aqui.

O relacionamento mais simples é o "um para um" ou "um para muitos" através de uma única referência, como `Pedido` guardando um único `Produto`. Mas é comum, na prática, que um pedido real contenha vários produtos, não apenas um — e isso se modela naturalmente combinando classes com arrays, já vistos no Módulo 6:

```java
public class Pedido {
    private Cliente cliente;
    private Produto[] produtos;
    private int[] quantidades;

    Pedido(Cliente cliente, Produto[] produtos, int[] quantidades) {
        this.cliente = cliente;
        this.produtos = produtos;
        this.quantidades = quantidades;
    }

    double calcularTotal() {
        double total = 0;
        for (int i = 0; i < produtos.length; i++) {
            total += produtos[i].getPreco() * quantidades[i];
        }
        return total;
    }
}
```

Aqui, `Pedido` se relaciona com múltiplos objetos `Produto` ao mesmo tempo, guardados em um array — um relacionamento "um para muitos", em que um único `Pedido` está associado a vários `Produto`s. O método `calcularTotal` percorre esse array com um laço `for`, somando o subtotal de cada produto, exatamente como se percorreria qualquer outro array de dados vistos no Módulo 6, com a diferença de que cada elemento agora é um objeto completo, com seu próprio comportamento (`getPreco()`), não apenas um valor primitivo solto.

Um segundo padrão de relacionamento comum é o mútuo, em que duas classes se referenciam uma à outra: um `Cliente` poderia guardar uma lista dos seus próprios pedidos, ao mesmo tempo em que cada `Pedido` guarda uma referência de volta ao `Cliente` que o fez. Relacionamentos assim exigem atenção redobrada para manter os dois lados consistentes entre si — se um pedido é adicionado à lista do cliente, mas o pedido não aponta de volta corretamente para aquele cliente, os dois objetos passam a descrever uma realidade contraditória.

Vale reforçar, como fechamento deste módulo, que relacionamentos são referências, não cópias — um `Pedido` que guarda um `Produto` guarda o mesmo objeto `Produto` que existe em outras partes do sistema (talvez em uma lista geral de produtos cadastrados), não uma cópia independente dele. Isso significa que, se o preço daquele produto for alterado através de outra referência a ele, o `Pedido` que o referencia "verá" automaticamente o novo preço na próxima vez que consultar `getPreco()`, exatamente porque ambos apontam para o mesmo objeto na memória — a mesma propriedade de identidade e referência discutida no início deste módulo, agora aplicada ao desenho de sistemas com várias classes conectadas entre si, que é a base sobre a qual sistemas orientados a objetos maiores e mais realistas são construídos.

### Responsabilidades

Responsabilidade, no contexto de desenhar classes, é a ideia de que cada classe deve ter um propósito claro e delimitado dentro do sistema, cuidando de um conjunto coerente de informações e comportamentos, e não se tornando um repositório genérico que faz um pouco de tudo. Esse conceito fecha o módulo amarrando, em uma única ideia orientadora, todas as decisões de desenho tomadas ao longo dos capítulos anteriores — o que vira campo, o que vira método, o que fica `private`, o que se torna relacionamento com outra classe.

Nos exemplos deste último capítulo, cada classe recebeu responsabilidades específicas: `Cliente` cuida de identificar e validar dados de uma pessoa; `Produto` cuida de descrever um item vendável e controlar seu próprio estoque; `Pedido` cuida de associar um cliente a produtos, calcular totais e validar que a compra é possível no momento em que é feita. Repare que `Pedido` não reimplementa a validação de e-mail (que é responsabilidade de `Cliente`) nem controla diretamente a lógica interna de estoque (que é responsabilidade de `Produto`, através de `retirarDoEstoque`) — ele apenas usa os métodos que essas outras classes já expõem, confiando que cada uma cuida bem da sua própria parte.

```java
// Pedido delega a Produto o controle do próprio estoque:
produto.retirarDoEstoque(quantidade);

// em vez de manipular o campo diretamente
// (o que nem seria possível, já que quantidadeEmEstoque é private)
```

Essa divisão evita um problema comum em sistemas mal modelados: uma única classe (às vezes literalmente chamada de algo genérico) acumulando responsabilidades de várias áreas diferentes do sistema ao mesmo tempo — validando e-mails, calculando preços, controlando estoque, formatando relatórios, tudo misturado no mesmo lugar. Esse tipo de classe fica difícil de entender (por reunir assuntos sem relação direta entre si), difícil de modificar com segurança (uma alteração feita para resolver um problema de estoque pode, por descuido, quebrar algo relacionado a e-mail, só porque as duas coisas moravam na mesma classe) e difícil de reaproveitar em outro contexto.

Uma pergunta prática para avaliar se uma classe está com responsabilidades bem definidas é: "se eu tivesse que explicar o que esta classe faz em uma frase só, ela faria sentido sem usar a palavra 'e' várias vezes?". `Produto` representa um item vendável e controla seu estoque — ainda é uma frase coesa, porque estoque é parte natural do que é ser um produto vendável. Já uma classe que "cadastra clientes, calcula frete, envia e-mails e gera relatórios financeiros" evidentemente reúne responsabilidades que deveriam estar em classes separadas.

Pensar em responsabilidades desde o início — antes mesmo de escrever o primeiro campo de uma classe nova — é o que transforma a lista de técnicas vistas ao longo deste módulo (campos, métodos, construtores, encapsulamento) em um desenho de sistema coerente, em vez de apenas um conjunto de regras sintáticas aplicadas sem critério. É esse tipo de julgamento, construído com prática, que separa um conjunto de classes que apenas "funciona" de um conjunto de classes que continua fácil de entender e de estender à medida que um sistema cresce — e é exatamente esse julgamento que continuará sendo exercitado nos módulos seguintes deste livro, sobre uma base sólida de classes e objetos já estabelecida aqui.

# Módulo 9 — Fundamentos do modelo de objetos

Depois de aprender a criar classes e objetos, é preciso entender algumas regras mais profundas que todos esses objetos compartilham dentro da linguagem. Este módulo consolida o modelo de objetos do Java mostrando comportamentos que vêm da classe `Object`, as diferentes noções de igualdade, a relação entre `equals` e `hashCode`, a diferença entre membros pertencentes a uma instância e membros pertencentes à classe, e as restrições introduzidas por `final`.

Esses assuntos aparecem juntos porque deixam de tratar apenas de "como criar um objeto" e passam a explicar como objetos se comportam dentro do ecossistema Java. Ao final do módulo, o leitor será capaz de distinguir identidade de igualdade lógica, compreender por que objetos usados em determinadas estruturas precisam obedecer ao contrato de `hashCode`, decidir quando um dado ou comportamento deve ser `static`, e usar `final` e as regras de inicialização de forma consciente. Isso prepara a base para recursos mais avançados de orientação a objetos nos níveis seguintes.

## Object

Todo objeto criado em Java participa de uma hierarquia comum, mesmo quando o programador não escreve nenhuma relação explícita de herança. Na raiz dessa hierarquia está a classe `Object`, da qual todas as classes derivam direta ou indiretamente. Isso explica por que objetos de classes completamente diferentes possuem certos métodos em comum e por que alguns comportamentos básicos podem ser tratados de forma uniforme pela linguagem.

Este capítulo apresenta essa base por meio da própria superclasse `Object`, dos métodos `toString` e `getClass` e da noção de identidade. A intenção é mostrar de onde vêm operações que parecem "existir em todo objeto" e preparar o terreno para os capítulos seguintes, nos quais igualdade e código hash serão analisados com mais precisão.

### Superclasse Object

Toda classe criada em Java, mesmo sem que isso seja escrito explicitamente, estende uma classe chamada `Object`. Quando se escreve `public class Cliente { ... }`, o compilador entende isso como se fosse `public class Cliente extends Object { ... }`, ainda que a palavra `extends Object` nunca apareça no código. `Object` é o topo de toda a hierarquia de classes em Java: todas as classes, sejam as do próprio Java (`String`, `Integer`, `ArrayList`) ou as criadas pelo programador (`Cliente`, `Produto`, `Pedido`, vistos no Módulo 8), descendem dela, direta ou indiretamente.

Isso resolve um problema prático: sem uma superclasse comum, cada classe teria que reimplementar do zero um conjunto mínimo de comportamentos que praticamente todo objeto precisa ter, como uma forma de se representar como texto ou uma forma de ser comparado a outro objeto. `Object` define esse conjunto mínimo de métodos — entre eles `toString()`, `equals()`, `hashCode()` e `getClass()`, cada um tratado em detalhe nos próximos conceitos — e toda classe já nasce com uma implementação padrão deles, herdada automaticamente, sem que o programador precise escrever nada.

```java
public class Produto {
    private String nome;
    private double preco;
    // nenhum método de Object foi escrito aqui,
    // mas Produto já possui toString(), equals(), hashCode() e getClass()
}

Produto p = new Produto();
System.out.println(p.toString()); // funciona mesmo sem Produto ter escrito toString
```

Esse comportamento tem uma consequência importante para quem já viu herança no Módulo 8: assim como uma classe filha herda campos e métodos de uma superclasse escrita pelo programador, toda classe herda automaticamente os métodos de `Object`, ainda que ninguém tenha escrito explicitamente essa relação de herança. É por isso que é possível chamar `.toString()` ou `.equals()` em qualquer objeto, de qualquer classe, mesmo em classes recém-criadas que não implementaram nada disso — o método existe porque veio de `Object`, e pode ser usado com o comportamento padrão ou sobrescrito (usando `@Override`, já visto no Módulo 8) para se comportar de um jeito mais adequado àquela classe específica.

Uma analogia útil é pensar em `Object` como um "contrato mínimo" que toda classe em Java assina automaticamente ao ser criada — como um documento de identidade que toda pessoa recebe ao nascer, contendo alguns campos padrão preenchidos de forma genérica, que podem depois ser complementados ou corrigidos, mas que já garantem que a pessoa "existe" de um jeito reconhecível pelo sistema. Não é possível "não estender" `Object` — mesmo classes que já estendem outra classe (`Gerente extends Funcionario`, por exemplo) continuam, no topo dessa cadeia, chegando em `Object`, porque a hierarquia de herança em Java sempre termina ali. Entender esse ponto de partida é a base para os próximos conceitos deste capítulo, que exploram individualmente os métodos mais importantes que `Object` fornece.

### toString

`toString()` é um método herdado de `Object` que converte um objeto em uma representação textual (`String`). Ele é chamado automaticamente em situações como `System.out.println(objeto)` ou quando um objeto é concatenado com uma `String` usando `+`, e também pode ser chamado explicitamente com `objeto.toString()`.

A implementação padrão herdada de `Object`, quando nenhuma outra é escrita, produz um texto pouco útil: o nome completo da classe seguido de um código em hexadecimal derivado do hash do objeto, como `Produto@1b6d3586`. Esse texto identifica tecnicamente o objeto, mas não diz nada sobre o que ele representa — não mostra o nome do produto, o preço, ou qualquer informação relevante para quem está lendo a saída do programa. Antes de sobrescrever `toString`, depurar um programa (por exemplo, imprimir um objeto para conferir seu estado) resultava exatamente nesse texto genérico, forçando o programador a imprimir manualmente cada campo, um por um, com várias chamadas a `System.out.println`.

A solução é sobrescrever `toString()` na própria classe, retornando uma `String` construída a partir dos campos que fazem sentido mostrar:

```java
public class Produto {
    private String nome;
    private double preco;

    @Override
    public String toString() {
        return "Produto{nome='" + nome + "', preco=" + preco + "}";
    }
}

Produto p = new Produto("Teclado", 150.0);
System.out.println(p); // Produto{nome='Teclado', preco=150.0}
```

Com essa sobrescrita, qualquer lugar do programa que exiba o objeto — um `println`, um log de erro, uma mensagem de depuração — passa a mostrar automaticamente uma descrição legível, sem que o programador precise lembrar de formatar isso manualmente toda vez. É um dos métodos mais frequentemente sobrescritos em Java, porque o ganho em clareza durante o desenvolvimento e a depuração é imediato e praticamente sem custo.

Uma alternativa parcial é não sobrescrever `toString` e, em vez disso, escrever métodos específicos de exibição (como `imprimirResumo()`), mas isso exige lembrar de chamar esse método toda vez, enquanto `toString()` é aproveitado automaticamente por `println` e pela concatenação com `String`, por já fazer parte do contrato herdado de `Object`. Quando não sobrescrever: em classes puramente internas, usadas só como estrutura de apoio dentro do próprio código e nunca exibidas ao usuário nem inspecionadas em depuração, a implementação padrão costuma ser suficiente e sobrescrever seria esforço sem retorno prático.

### getClass

`getClass()` é outro método herdado de `Object`, presente em todo objeto Java, que devolve um objeto do tipo `Class` representando a classe exata daquele objeto em tempo de execução. Diferente de `toString`, que descreve o conteúdo de um objeto, `getClass()` descreve o tipo do objeto — qual classe, entre todas as existentes no programa, foi usada para criá-lo com `new`.

Sem esse método, não haveria uma forma direta, em tempo de execução, de perguntar a um objeto "de que classe você realmente é". Isso importa em situações de herança e polimorfismo, já vistas no Módulo 8: uma variável do tipo `Funcionario` pode, na prática, estar guardando um objeto da subclasse `Gerente`. O tipo declarado da variável (`Funcionario`) é fixo em tempo de compilação, mas `getClass()` revela o tipo real do objeto armazenado ali, em tempo de execução:

```java
Funcionario f = new Gerente("Ana", 8000);
System.out.println(f.getClass());       // class Gerente
System.out.println(f.getClass().getSimpleName()); // Gerente
```

`getClass().getName()` devolve o nome completo da classe (incluindo o pacote, se houver), enquanto `getClass().getSimpleName()` devolve apenas o nome da classe, sem o pacote — geralmente mais útil em mensagens voltadas ao usuário final ou em logs de depuração. `getClass()` também é usado internamente por implementações corretas de `equals()` (visto no próximo capítulo), para verificar se dois objetos comparados são realmente da mesma classe antes de comparar seus campos.

Uma analogia é pensar em `getClass()` como uma etiqueta de fábrica presa a cada objeto, indicando exatamente qual "linha de produção" (classe) o originou — mesmo que o objeto esteja guardado em uma caixa rotulada de forma mais genérica (o tipo declarado da variável), a etiqueta interna sempre revela a origem exata. Esse método raramente precisa ser sobrescrito (na verdade, `getClass()` nem pode ser sobrescrito, ao contrário de `toString` e `equals`) — ele é usado como está, principalmente em código que precisa tomar decisões baseadas no tipo real de um objeto, em comparações de igualdade, ou em ferramentas de depuração e logging que precisam relatar informações precisas sobre os objetos que manipulam.

### Identidade

Identidade — a propriedade de que cada `new` cria um objeto único na memória, mesmo que dois objetos tenham exatamente os mesmos valores em todos os campos — já foi vista em detalhe no Módulo 8, ao estudar o que é um objeto. Vale retomá-la aqui, de forma breve, porque ela é a peça que faltava para entender por completo os métodos que `Object` fornece: assim como a implementação padrão de `toString()` produz um texto genérico por não saber nada sobre o conteúdo específico de cada classe, a implementação padrão de `equals()` — ainda não vista em detalhe — também recorre à identidade como critério de comparação, e não há como entender por que isso é um problema sem antes ter identidade bem clara na cabeça.

```java
Produto p1 = new Produto("Teclado", 150.0);
Produto p2 = new Produto("Teclado", 150.0);
Produto p3 = p1;

// p1 e p2: objetos diferentes, mesmo conteúdo — identidades distintas
// p1 e p3: a MESMA identidade — p3 aponta para o mesmo objeto que p1
```

Repare que, do ponto de vista de quem lê o código, `p1` e `p2` representam "o mesmo produto" em termos de conteúdo, mas continuam sendo dois objetos separados na memória — é exatamente essa tensão, entre "são o mesmo objeto" e "representam a mesma coisa", que os próximos capítulos (Igualdade e hashCode) exploram em detalhe: como o operador `==` compara identidade explicitamente, como e por que sobrescrever `equals()` para comparar conteúdo em vez de identidade, e por que `hashCode()` precisa acompanhar essa mudança quando ela acontece.

Sem essa distinção fixada, é fácil confundir "dois objetos com o mesmo conteúdo" com "o mesmo objeto" — um erro que gera bugs sutis, como alterar um campo através de `p3` e ver essa alteração refletida em `p1` (porque são o mesmo objeto), enquanto alterar `p2` nunca afeta `p1` (porque são objetos independentes que só coincidem em conteúdo no momento da criação). Fixar identidade agora é o que permite, no capítulo seguinte, entender com precisão o que `==` e `equals()` realmente comparam por padrão — e por que, às vezes, comparar por identidade não é o que o programa precisa.

## Igualdade

Dizer que dois objetos são "iguais" pode significar coisas diferentes. Duas variáveis podem apontar exatamente para a mesma instância na memória, ou podem apontar para objetos distintos que representam, segundo as regras do domínio, a mesma informação. Em Java, essas duas ideias não são tratadas automaticamente como equivalentes, e confundi-las leva a comparações incorretas.

Este capítulo separa identidade de igualdade lógica usando `==` e `equals`. O leitor verá que `==` responde sobre referências, enquanto `equals` pode ser definido para expressar quando dois objetos devem ser considerados equivalentes pelo conteúdo ou significado. Essa distinção é fundamental antes de estudar `hashCode`, porque os dois conceitos formam um contrato usado por várias estruturas da biblioteca Java.

### `==`

O operador `==`, já usado desde os primeiros módulos do livro para comparar valores de tipos primitivos (`int`, `double`, `boolean`), tem um comportamento diferente quando aplicado a objetos: em vez de comparar conteúdo, `==` compara identidade — ou seja, verifica se as duas variáveis apontam exatamente para o mesmo objeto na memória, conforme o conceito de identidade visto no capítulo anterior.

```java
Produto p1 = new Produto("Teclado", 150.0);
Produto p2 = new Produto("Teclado", 150.0);
Produto p3 = p1;

System.out.println(p1 == p2); // false — objetos diferentes, mesmo conteúdo
System.out.println(p1 == p3); // true — mesma identidade
```

O problema que costuma surgir aqui é usar `==` esperando uma comparação de conteúdo, como se estivesse comparando dois `int`. Um programador vindo da intuição de que "`==` compara se duas coisas são iguais" pode escrever `if (p1 == p2)` esperando `true` quando os dois produtos têm o mesmo nome e preço, e se surpreender ao receber `false`, porque `p1` e `p2`, apesar do conteúdo idêntico, são dois objetos distintos criados por dois `new` separados.

`==` continua sendo o operador correto para comparar tipos primitivos (que não têm identidade, apenas valor) e também é útil, propositalmente, quando se quer mesmo verificar identidade entre objetos — por exemplo, checar se uma variável aponta para `null`, ou verificar se duas referências apontam deliberadamente para o mesmo objeto compartilhado, algo comum ao trabalhar com relacionamentos entre classes, como visto no Módulo 8:

```java
if (produtoSelecionado == null) {
    System.out.println("Nenhum produto selecionado.");
}
```

Uma analogia: comparar dois objetos com `==` é como perguntar "essas duas chaves abrem a mesma porta física?", em vez de perguntar "essas duas chaves têm o mesmo formato e cor?". Duas chaves podem ser fisicamente idênticas em aparência (mesmo conteúdo) sem serem a mesma chave (identidades diferentes), assim como uma única chave, emprestada e devolvida, continua sendo a mesma chave todo o tempo (mesma identidade), não importa quantas vezes ela mude de mão.

Quando o objetivo é comparar se dois objetos representam "a mesma coisa" em termos de conteúdo — o mesmo produto, o mesmo cliente, o mesmo CPF —, `==` não é a ferramenta certa, porque avalia identidade, não conteúdo. Esse é exatamente o problema que o próximo conceito, `equals`, resolve.

### equals

`equals()` é um método herdado de `Object` (visto no capítulo anterior) que existe justamente para permitir comparar objetos por conteúdo, e não apenas por identidade como `==` faz. A implementação padrão herdada de `Object`, quando ninguém a sobrescreve, na verdade se comporta exatamente como `==`: compara se as duas referências apontam para o mesmo objeto. Ou seja, sem nenhuma customização, `equals` e `==` produzem o mesmo resultado — o que costuma surpreender quem espera que `equals` já compare conteúdo "de graça".

```java
Produto p1 = new Produto("Teclado", 150.0);
Produto p2 = new Produto("Teclado", 150.0);

System.out.println(p1.equals(p2)); // false, sem sobrescrita — comporta-se como ==
```

O problema, antes de sobrescrever `equals`, é que não há forma direta de perguntar "esses dois produtos representam a mesma coisa?" quando o que importa é o conteúdo (mesmo nome, mesmo preço), não a identidade do objeto na memória. A solução é sobrescrever `equals()` na classe, comparando os campos relevantes:

```java
@Override
public boolean equals(Object outro) {
    if (this == outro) return true;
    if (outro == null || getClass() != outro.getClass()) return false;
    Produto p = (Produto) outro;
    return preco == p.preco && nome.equals(p.nome);
}
```

Essa implementação segue um padrão comum: primeiro verifica identidade (`this == outro`, um atalho rápido — se são o mesmo objeto, com certeza são "iguais"); depois verifica se `outro` é `null` ou de uma classe diferente (usando `getClass()`, visto no capítulo anterior); por fim, faz o cast para o tipo correto e compara os campos que definem a igualdade lógica daquela classe. Repare que a comparação de `nome`, que é uma `String`, usa `.equals()` recursivamente, e não `==`, porque `String` também é um objeto — comparar duas `String` com `==` teria o mesmo problema de identidade descrito no conceito anterior.

Depois de sobrescrito, `equals` passa a ser usado automaticamente por estruturas do próprio Java, como listas, ao procurar um elemento (`lista.contains(produto)`), tornando a comparação por conteúdo algo nativo do código, sem exigir lógica manual repetida em todo lugar que precise comparar dois objetos daquela classe.

### Igualdade por identidade

Igualdade por identidade é o nome dado ao tipo de comparação que `==` realiza sobre objetos, já vista no conceito anterior: comparar se duas referências levam ao mesmo objeto na memória. Isolar esse conceito com nome próprio ajuda a diferenciá-lo claramente do próximo, igualdade lógica, já que os dois convivem em Java e é fácil confundi-los.

```java
Cliente c1 = new Cliente("Ana", "ana@email.com");
Cliente c2 = c1;

System.out.println(c1 == c2); // true — igualdade por identidade
```

Sem essa noção nomeada, seria fácil tratar `==` como "só mais uma forma de comparar", sem entender por que ela às vezes concorda com `equals` (quando as duas variáveis apontam para o mesmo objeto) e às vezes diverge completamente dele (quando os objetos têm conteúdo igual mas identidades diferentes). Igualdade por identidade resolve exatamente o caso em que o que importa é saber se duas referências levam ao mesmíssimo objeto — por exemplo, verificar se uma variável de controle ainda aponta para o mesmo objeto que foi armazenado antes em uma estrutura, ou detectar dois nomes diferentes (`c1` e `c2`) usados para a mesma entidade.

Um exemplo real de uso: em um sistema de cache, é comum querer saber se o objeto retornado agora é literalmente o mesmo objeto já guardado antes (evitando recriar algo que já existe), e não apenas um objeto com o mesmo conteúdo — nesse caso, `==` é exatamente a ferramenta certa, e usar `equals` sobrescrito seria enganoso, porque poderia dizer "iguais" mesmo para dois objetos fisicamente diferentes.

A analogia da chave física, usada no conceito de `==`, se aplica diretamente aqui: igualdade por identidade pergunta se duas chaves são fisicamente a mesma chave, não se têm o mesmo formato. Quando não usar igualdade por identidade: sempre que o objetivo do programa for comparar o significado ou o conteúdo de dois objetos — dois `Produto` que descrevem o mesmo item, dois `Cliente` com o mesmo CPF — usar apenas `==` levaria a resultados tecnicamente corretos, porém inúteis para a lógica de negócio, porque quase nunca dois objetos criados separadamente serão literalmente o mesmo objeto, mesmo representando "a mesma coisa" do ponto de vista do problema que o programa resolve.

### Igualdade lógica

```java
Produto p1 = new Produto("Teclado", 150.0);
Produto p2 = new Produto("Teclado", 150.0);

System.out.println(p1 == p2);       // false — identidades diferentes
System.out.println(p1.equals(p2));  // true — igualdade lógica, mesmo conteúdo
```

Repare a diferença entre as duas linhas: mesmo com identidades diferentes, `p1.equals(p2)` retorna `true`, porque `Produto` sobrescreve `equals()` (como visto no conceito `equals`) para comparar conteúdo em vez de posição na memória. Esse resultado tem nome: igualdade lógica, o tipo de comparação que `equals()` sobrescrito realiza, considerando "iguais" objetos cujo conteúdo relevante é equivalente, independentemente de serem ou não o mesmo objeto na memória — o complemento direto da igualdade por identidade vista no conceito anterior. Juntas, as duas formam o par de comparações que Java oferece para objetos.

O problema que a igualdade lógica resolve é justamente a limitação da igualdade por identidade em contextos onde o que importa, para o programa, é o significado do objeto, não sua posição na memória. Um sistema de pedidos que precisa verificar "este cliente já fez este pedido antes?" não está interessado em saber se os dois objetos `Pedido` comparados são literalmente o mesmo objeto — está interessado em saber se representam a mesma compra, com os mesmos dados. `equals()`, sobrescrito para comparar os campos relevantes (como visto no conceito `equals` acima), resolve exatamente esse tipo de pergunta.

Um exemplo real de uso é comparar dois objetos `Cliente` pelo CPF, ignorando outros campos que possam variar sem alterar a identidade da pessoa (como um telefone atualizado):

```java
@Override
public boolean equals(Object outro) {
    if (this == outro) return true;
    if (outro == null || getClass() != outro.getClass()) return false;
    Cliente c = (Cliente) outro;
    return cpf.equals(c.cpf); // igualdade lógica baseada só no CPF
}
```

Repare que essa implementação decide, deliberadamente, que dois `Cliente` são "iguais" quando têm o mesmo CPF, mesmo que outros campos sejam diferentes — essa decisão de quais campos definem a igualdade lógica de uma classe é sempre do programador, e deve refletir o que "ser igual" significa de fato no domínio do problema, não uma regra genérica aplicável a qualquer classe.

Uma alternativa a implementar igualdade lógica manualmente é usar registros (`record`, um recurso mais recente de Java) ou bibliotecas que geram `equals` automaticamente a partir dos campos, mas o entendimento de como a comparação funciona por baixo continua sendo o mesmo. Quando não sobrescrever `equals` para obter igualdade lógica: em classes onde nunca faz sentido comparar dois objetos por conteúdo — por exemplo, classes que representam um recurso único e não substituível, como uma conexão de rede aberta — a igualdade por identidade padrão, herdada de `Object`, já é o comportamento correto, e sobrescrever `equals` nesses casos criaria uma noção de igualdade sem utilidade real para o problema.

## hashCode

Objetos nem sempre são comparados percorrendo todos os seus dados um a um. Algumas estruturas precisam localizar valores rapidamente e, para isso, usam um número calculado a partir do objeto: o hash code. Esse número não substitui a igualdade, mas funciona como uma forma de organizar objetos em grupos prováveis antes de uma comparação mais precisa.

Este capítulo apresenta a ideia de hash e, principalmente, o contrato que liga `hashCode` a `equals`: objetos considerados iguais precisam produzir códigos hash compatíveis. Serão discutidas as consequências práticas de quebrar essa regra, preparando o leitor para entender futuramente estruturas como `HashSet` e `HashMap`. O objetivo não é estudar algoritmos de hashing em profundidade, mas compreender por que esse método existe em todo objeto Java.

### Contrato com equals

`hashCode()` é outro método herdado de `Object`, que devolve um número inteiro (um "código hash") calculado a partir do objeto. Ele está diretamente ligado a `equals()`, através de uma regra obrigatória conhecida como o contrato entre `equals` e `hashCode`: se dois objetos são considerados iguais por `equals()` (`a.equals(b)` retorna `true`), então `a.hashCode()` e `b.hashCode()` devem obrigatoriamente devolver o mesmo valor. O contrário não precisa ser verdade — dois objetos podem ter o mesmo `hashCode` sem serem `equals`, mas nunca o oposto.

O problema que motiva essa regra é que várias estruturas do próprio Java (apresentadas de forma introdutória neste livro ao tratar de coleções) usam `hashCode()` como um atalho de desempenho: antes de comparar dois objetos em detalhe com `equals()`, elas primeiro comparam os `hashCode()`, porque calcular e comparar um número inteiro é muito mais rápido do que comparar todos os campos de um objeto. Se uma classe sobrescreve `equals()` (dizendo que dois objetos com o mesmo CPF são iguais, por exemplo) mas não sobrescreve `hashCode()` de forma coerente, essas estruturas podem tratar como diferentes dois objetos que o próprio programa considera iguais, produzindo bugs difíceis de rastrear.

```java
public class Cliente {
    private String cpf;

    @Override
    public boolean equals(Object outro) {
        if (this == outro) return true;
        if (outro == null || getClass() != outro.getClass()) return false;
        return cpf.equals(((Cliente) outro).cpf);
    }

    @Override
    public int hashCode() {
        return cpf.hashCode(); // coerente com equals: mesmo cpf => mesmo hashCode
    }
}
```

A regra prática, portanto, é: toda vez que `equals()` é sobrescrito, `hashCode()` deve ser sobrescrito junto, baseado exatamente nos mesmos campos usados para decidir a igualdade lógica. Se `equals` compara apenas `cpf`, `hashCode` deve ser calculado apenas a partir de `cpf` — nunca incluir um campo em `hashCode` que não participa de `equals`, e vice-versa, sob pena de quebrar o contrato.

Uma analogia: pensar no `hashCode` como o CEP de um endereço e no `equals` como a conferência completa do endereço. Duas casas no mesmo CEP não são necessariamente a mesma casa (hashCodes iguais não implicam objetos iguais), mas duas casas que são comprovadamente a mesma (iguais por `equals`) obrigatoriamente compartilham o mesmo CEP — seria uma contradição dizer que são a mesma casa, mas em CEPs diferentes. É essa consistência entre os dois métodos que o contrato exige, e ela é a base para o próximo conceito, que trata de como os hashes são efetivamente usados na prática.

### Hashes

Um hash, no sentido em que `hashCode()` o produz, é um número inteiro calculado a partir do conteúdo de um objeto, funcionando como uma espécie de "resumo numérico" desse conteúdo. A ideia central é que objetos com o mesmo conteúdo relevante (segundo `equals`) produzam sempre o mesmo hash, e que objetos com conteúdos diferentes tendam, na maioria dos casos, a produzir hashes diferentes — embora colisões (dois conteúdos diferentes gerando o mesmo hash) sejam possíveis e até esperadas, apenas menos frequentes.

Como já vimos no conceito anterior, comparar `hashCode()` é mais rápido do que comparar campo por campo — mas isso explica só metade do ganho. A outra metade vem de como o hash é usado na prática: ele permite agrupar objetos em "regiões" (ou "baldes"), de modo que localizar um objeto não exige percorrer a coleção inteira, apenas calcular seu hash e ir direto à região correspondente, em vez de comparar um por um até achar o certo. É esse agrupamento — e não só a comparação mais rápida — que torna estruturas baseadas em hash eficientes mesmo com milhões de itens armazenados.

```java
public class Produto {
    private String codigo;
    private double preco;

    @Override
    public int hashCode() {
        return java.util.Objects.hash(codigo, preco);
    }

    @Override
    public boolean equals(Object outro) {
        if (this == outro) return true;
        if (outro == null || getClass() != outro.getClass()) return false;
        Produto p = (Produto) outro;
        return codigo.equals(p.codigo) && preco == p.preco;
    }
}
```

O método utilitário `Objects.hash(...)`, usado no exemplo, combina os hashes de vários campos em um único número, sendo a forma mais comum e segura de implementar `hashCode()` manualmente sem correr o risco de errar a fórmula matemática por conta própria. Ele calcula automaticamente um hash combinado a partir da lista de campos passada, exatamente os mesmos campos usados em `equals()`, respeitando o contrato visto no conceito anterior.

Uma analogia: pensar no hash como o número de um armário em um sistema de armários numerados por sobrenome — várias pessoas com sobrenomes diferentes podem, por coincidência, cair no mesmo número (colisão), mas a numeração ainda assim reduz drasticamente o tempo de busca, porque em vez de vasculhar todos os armários, basta calcular o número esperado e checar apenas aquela região. Alternativas a implementar `hashCode` manualmente incluem herdar a implementação padrão de `Object` (baseada na identidade do objeto), mas essa alternativa só é válida quando `equals` também não foi sobrescrito — caso contrário, o contrato é quebrado. Quando não se preocupar tanto com hashes: em classes que nunca serão usadas dentro de estruturas que dependem de hash, o cuidado extra de escrever `hashCode` manualmente traz pouco benefício prático imediato, embora seja sempre boa prática mantê-lo coerente com `equals` por segurança futura.

### Consequências práticas

Quebrar o contrato entre `equals` e `hashCode` — sobrescrever um sem sobrescrever o outro de forma coerente — não gera um erro de compilação nem uma exceção em tempo de execução: o programa continua rodando normalmente, o que torna esse tipo de bug particularmente traiçoeiro, porque ele só aparece em situações específicas, silenciosamente, sem nenhuma mensagem de erro apontando a causa.

O cenário mais comum em que isso se manifesta é ao usar um objeto como referência de busca dentro de uma estrutura que organiza itens por hash: se `equals()` foi sobrescrito para comparar por CPF, mas `hashCode()` não foi sobrescrito (continuando com a implementação padrão baseada em identidade, herdada de `Object`), dois objetos `Cliente` com o mesmo CPF — logicamente "iguais" pelo `equals` — terão hashes diferentes, porque a implementação padrão calcula o hash a partir da identidade de cada objeto, não do CPF. O resultado prático é que uma busca por aquele cliente pode "não encontrá-lo", mesmo que um cliente com aquele CPF exato já esteja armazenado, porque a estrutura de busca olhou primeiro na região de hash errada.

```java
// Cliente com equals sobrescrito (por cpf) mas SEM hashCode sobrescrito:
Cliente c1 = new Cliente("111.111.111-11");
Cliente c2 = new Cliente("111.111.111-11"); // mesmo CPF, objeto diferente

System.out.println(c1.equals(c2)); // true — iguais por CPF
// mas c1.hashCode() != c2.hashCode() (herdado de Object, baseado em identidade)
// => uma busca por c2 pode não localizar c1, mesmo sendo "iguais"
```

A prevenção é simples e já foi apresentada no primeiro conceito deste capítulo: sempre sobrescrever `hashCode()` junto com `equals()`, usando exatamente os mesmos campos, de preferência com `Objects.hash(...)`. Muitos ambientes de desenvolvimento (IDEs) oferecem geração automática de ambos os métodos ao mesmo tempo, exatamente para evitar que um seja esquecido enquanto o outro é escrito.

Uma analogia final para fixar a consequência prática: é como catalogar livros em uma biblioteca por um código nas costas do livro (o hash), mas, ao decidir se dois livros são "a mesma edição" (equals), usar o título impresso na capa em vez do código. Um funcionário que confia apenas no código para localizar rapidamente o livro pode nunca encontrar duas cópias que são, na prática, a mesma edição, porque elas foram catalogadas com códigos diferentes por engano. Esse tipo de inconsistência é exatamente o que o contrato entre `equals` e `hashCode` existe para evitar, e por isso os dois métodos devem sempre ser tratados como um par inseparável, nunca sobrescritos isoladamente.

## static

Até agora, campos e métodos foram apresentados principalmente como partes de objetos: cada instância possui seu próprio estado e os métodos atuam sobre ela. Há situações, porém, em que determinado dado ou comportamento pertence ao tipo como um todo, e não a uma instância específica — por exemplo, um contador compartilhado por todos os objetos ou uma função utilitária que não depende do estado de nenhum deles.

Este capítulo apresenta `static` como o mecanismo usado para representar membros da classe em vez de membros da instância. Serão comparados campos de classe, métodos estáticos, estado compartilhado e o acesso por instância ou pela própria classe. Ao final, o leitor deverá conseguir decidir se determinado membro faz sentido existir uma vez por objeto ou uma única vez para a classe inteira.

### Campos de classe

Um campo declarado com o modificador `static` deixa de pertencer a cada objeto individualmente e passa a pertencer à classe como um todo — existe uma única cópia daquele campo, compartilhada por todos os objetos daquela classe, em vez de uma cópia por objeto, como acontece com os campos de instância vistos no Módulo 8.

```java
public class Produto {
    private String nome;         // campo de instância: cada Produto tem o seu
    static int totalCriados = 0; // campo de classe: um único valor, compartilhado

    Produto(String nome) {
        this.nome = nome;
        totalCriados++; // incrementa o único contador compartilhado
    }
}
```

O problema que campos `static` resolvem é o de manter uma informação que diz respeito à classe como um todo, não a um objeto específico. Sem `static`, a única forma de guardar "quantos produtos já foram criados" seria através de uma variável externa a todas as instâncias, manuseada manualmente em algum outro ponto do programa, sem nenhuma garantia de que ela realmente refletisse todos os `new Produto(...)` feitos pelo sistema — cada objeto, isoladamente, não tem como saber quantos outros objetos da mesma classe existem, porque cada um só enxerga seus próprios campos de instância.

Um campo `static` é acessado preferencialmente pelo nome da classe, não por uma variável de objeto, justamente para deixar claro que ele não pertence a nenhum objeto específico:

```java
Produto p1 = new Produto("Teclado");
Produto p2 = new Produto("Mouse");

System.out.println(Produto.totalCriados); // 2 — acessado pela classe
```

Ainda que Java também permita acessar um campo `static` através de uma variável de objeto (`p1.totalCriados`), isso é considerado má prática exatamente porque sugere, erradamente, que o campo pertence àquele objeto específico — usar `NomeDaClasse.campo` deixa a intenção explícita para quem lê o código.

Uma analogia útil: pensar em campos de instância como o número de identificação pessoal de cada funcionário de uma empresa (cada um tem o seu, individual) e em um campo `static` como o placar total de funcionários contratados, afixado na parede da recepção — um único placar, visível e compartilhado por todos, que qualquer funcionário pode consultar, mas que não pertence a nenhum funcionário em particular; atualizar esse placar afeta o que todos veem, porque é o mesmo placar para todo mundo.

Um exemplo real de uso é um contador de instâncias (como acima), uma configuração compartilhada por todos os objetos de uma classe (como uma taxa de imposto aplicada a todos os `Produto`), ou um valor padrão usado como base por todos os objetos até serem individualmente alterados. Nem toda informação deve virar `static`, porém: dados que variam legitimamente de objeto para objeto — o nome, o preço, o estoque de cada `Produto` — precisam continuar sendo campos de instância. Os riscos concretos de errar essa escolha são justamente o assunto do próximo conceito, sobre estado compartilhado.

### Métodos estáticos

Assim como campos podem ser `static`, métodos também podem receber esse modificador. Um método `static` pertence à classe, não a um objeto específico, e por isso pode ser chamado sem que exista nenhum objeto criado daquela classe — basta o nome da classe seguido do método, como já é feito, por exemplo, com métodos utilitários vistos ao longo do livro (`Math.sqrt(...)`, `Integer.parseInt(...)`), que na verdade sempre foram métodos estáticos, ainda que isso não tenha sido nomeado explicitamente até agora.

```java
public class Conversor {
    static double reaisParaDolares(double reais, double cotacao) {
        return reais / cotacao;
    }
}

double valor = Conversor.reaisParaDolares(100.0, 5.0); // sem nenhum "new Conversor()"
```

O problema que métodos `static` resolvem é o de expressar uma operação que não depende do estado de nenhum objeto específico — uma conversão de unidades, um cálculo matemático, uma validação genérica de formato — como uma ação isolada, sem forçar o programador a criar um objeto só para executar aquela lógica, o que seria desnecessário e confuso quando não existe nenhum "estado" individual envolvido na operação.

Uma restrição importante de métodos `static` é que eles não podem acessar diretamente campos ou métodos de instância (aqueles que pertencem a cada objeto individualmente), porque, ao ser chamado sem nenhum objeto, um método `static` simplesmente não tem um `this` para se referir — não haveria como saber "de qual objeto" pegar aquele campo de instância. Métodos `static` só podem acessar diretamente outros membros `static` da mesma classe.

```java
public class Produto {
    private String nome;      // campo de instância
    static int totalCriados;  // campo de classe

    static void mostrarTotal() {
        System.out.println(totalCriados); // ok, static acessando static
        // System.out.println(nome);      // erro de compilação: nome é de instância
    }
}
```

Uma analogia: um método de instância é como uma ação que só um funcionário específico pode realizar, usando informações pessoais dele (seu crachá, sua mesa); um método `static` é como um procedimento padrão da empresa, documentado em um manual, que qualquer pessoa pode executar sem precisar ser um funcionário específico — o manual não faz referência a nenhuma mesa ou crachá individual.

Métodos `static` são amplamente usados para operações utilitárias (conversões, validações, cálculos), para métodos de fábrica que criam objetos de uma forma controlada, e para o próprio método `main`, que já é `static` desde os primeiros programas escritos neste livro, precisamente porque a JVM precisa chamá-lo antes de qualquer objeto existir. Quando não usar `static`: sempre que o método precisa ler ou alterar o estado individual de um objeto — como `calcularTotal()` de um `Pedido`, que depende dos produtos específicos daquele pedido —, ele deve continuar sendo um método de instância, porque sua lógica é indissociável dos dados daquele objeto específico.

### Estado compartilhado

Como já vimos no conceito anterior, um campo `static` existe em uma única cópia para toda a classe. A consequência direta disso é o que chamamos de estado compartilhado: qualquer alteração feita nesse campo através de um objeto é imediatamente visível para todos os outros objetos da mesma classe — e até mesmo para código que não tem referência a nenhum objeto específico, já que o campo pertence à classe, não a um objeto.

```java
public class ContaBancaria {
    static double taxaJuros = 0.02;
    private double saldo;

    void aplicarJuros() {
        saldo += saldo * taxaJuros;
    }
}

ContaBancaria.taxaJuros = 0.05; // altera a taxa para TODAS as contas de uma vez
```

O problema que estado compartilhado resolve é manter sincronizada uma informação que deve valer igualmente para todos os objetos de uma classe, sem depender de cada objeto individual ser atualizado manualmente, um por um. No exemplo acima, alterar `taxaJuros` uma única vez através da classe faz com que todo `aplicarJuros()` chamado depois, em qualquer conta, já use o novo valor — não seria necessário (nem seria correto) percorrer cada `ContaBancaria` já criada, alterando seu campo individualmente.

Por outro lado, estado compartilhado é também uma fonte comum de bugs quando usado sem cuidado, precisamente pela mesma razão que o torna útil: como todos os objetos compartilham a mesma cópia, uma alteração feita em um ponto do programa, muitas vezes inesperado, pode afetar o comportamento de partes completamente diferentes do sistema que também dependem daquele mesmo campo `static`, dificultando rastrear a origem de um valor incorreto — é o preço de ter um único ponto de verdade compartilhado por todo o programa.

Uma analogia: pensar em estado compartilhado como o termostato central de um prédio inteiro (o campo `static`), em contraste com o termostato individual de cada apartamento (um campo de instância). Ajustar o termostato central afeta a temperatura de todos os apartamentos ao mesmo tempo, o que é conveniente quando se quer uma mudança uniforme, mas também significa que um morador não pode, sozinho, alterar a temperatura só do seu apartamento através daquele controle central — e um ajuste feito por qualquer morador afeta todos os outros, às vezes de forma indesejada.

Um exemplo real de uso responsável de estado compartilhado é uma configuração global de um sistema, como uma taxa de câmbio, um modo de depuração ligado ou desligado, ou um limite máximo de conexões simultâneas — informações que, por definição, devem ser as mesmas para todo o programa. Quando evitar estado compartilhado: em qualquer situação onde os objetos precisam de independência real entre si — cada `ContaBancaria` com seu próprio saldo já é assim, corretamente, por ser um campo de instância — transformar esse tipo de dado em `static` por engano é um erro comum e sério, que faz todos os objetos passarem a compartilhar o que deveria ser exclusivo de cada um.

### Instância × classe

Este conceito fecha o capítulo amarrando, em contraste direto, tudo o que foi visto sobre `static`: a diferença fundamental entre pertencer à instância (a cada objeto, individualmente) e pertencer à classe (um único elemento, compartilhado por todos os objetos e acessível mesmo sem nenhum objeto existir).

| | Instância | Classe (`static`) |
|---|---|---|
| Quantas cópias existem | uma por objeto | uma única, para toda a classe |
| Como se acessa | `objeto.campo` | `NomeDaClasse.campo` |
| Precisa de `new` para existir | sim | não |
| Métodos podem acessar | campos de instância e static | apenas campos static |

```java
public class Funcionario {
    private String nome;           // instância: cada funcionário, o seu
    static String empresa = "TechCorp"; // classe: uma só, para todos

    Funcionario(String nome) {
        this.nome = nome;
    }

    void apresentar() {
        System.out.println(nome + " trabalha na " + empresa);
    }
}
```

No exemplo, `nome` varia de funcionário para funcionário — é exatamente o tipo de dado que os campos de instância existem para representar, como já visto extensivamente no Módulo 8. Já `empresa` é a mesma para todos os objetos `Funcionario` criados, então faz sentido que exista uma única cópia dela, compartilhada, em vez de repetir o mesmo texto "TechCorp" em cada objeto individualmente — o que seria redundante e, pior, arriscado: se a empresa mudasse de nome, seria preciso atualizar esse valor em cada objeto separadamente, campo por campo, em vez de alterar um único valor compartilhado.

A decisão de tornar um campo `static` ou de instância nunca é arbitrária — depende diretamente da pergunta "esse valor varia de objeto para objeto, ou é o mesmo para todos?". Um erro comum de quem está começando é usar `static` só porque simplifica o acesso (não precisa de um objeto para chamar), sem checar se aquele dado realmente deveria ser único e compartilhado; o resultado é um campo que deveria ser individual (como o saldo de uma conta, ou o nome de um produto) virando, por engano, uma única cópia compartilhada por todos os objetos, quebrando a independência que objetos deveriam ter entre si — exatamente o problema descrito no conceito anterior sobre estado compartilhado.

Uma forma prática de decidir, na hora de declarar um novo campo ou método: perguntar se a informação faz sentido existir mesmo antes de qualquer objeto ser criado, ou se é uma configuração global do "tipo" de coisa que a classe representa (nesse caso, `static`); ou se, ao contrário, só faz sentido no contexto de um objeto individual específico, com seu próprio valor (nesse caso, campo de instância, sem `static`). Essa distinção entre instância e classe, bem compreendida, é a base sobre a qual o próximo capítulo constrói mais um modificador importante: `final`, que trata não de onde um campo mora, mas de se ele pode ou não ser alterado depois de definido.

## final e inicialização

Algumas partes de um programa precisam poder mudar; outras devem permanecer fixas depois de definidas. Java oferece `final` para expressar diferentes formas dessa restrição, aplicando-a a campos, métodos e classes. Ao mesmo tempo, para usar corretamente campos que só podem ser atribuídos uma vez, é necessário entender quando e em que ordem a inicialização de uma classe e de seus objetos acontece.

Este capítulo reúne esses assuntos porque `final`, constantes e inicialização se encontram justamente no momento em que valores são definidos. Serão vistos campos `final`, métodos e classes finais, `static final`, blocos de inicialização e a ordem em que diferentes partes da classe são preparadas. Ao final, o leitor terá uma visão mais precisa do ciclo de inicialização de objetos e de como impor imutabilidade ou restrições onde elas são realmente necessárias.

### Campos final

Um campo declarado com o modificador `final` só pode receber um valor uma única vez — depois de atribuído (seja diretamente na declaração, seja dentro de um construtor), esse valor não pode mais ser alterado durante toda a vida do objeto. Diferente de `static`, que decide onde um campo mora (instância ou classe), `final` decide se um campo, uma vez definido, pode ou não mudar depois.

```java
public class Produto {
    private final String codigo; // não pode mudar depois de definido

    Produto(String codigo) {
        this.codigo = codigo; // atribuição única, permitida dentro do construtor
    }

    void alterarCodigo(String novoCodigo) {
        // this.codigo = novoCodigo; // erro de compilação: codigo é final
    }
}
```

O problema que campos `final` resolvem é o de garantir, de forma verificada pelo próprio compilador, que certos dados de um objeto não sejam alterados acidentalmente depois de definidos — sem `final`, nada impediria que qualquer método da classe reatribuísse `codigo` a qualquer momento, mesmo quando a lógica do sistema exige que aquele valor seja imutável desde a criação do objeto (um código de produto, um CPF, um identificador único, que não fazem sentido "mudar" depois de definidos).

Um campo `final` pode ser inicializado diretamente na declaração, ou deixado sem valor na declaração e obrigatoriamente atribuído dentro de todo construtor da classe — o compilador verifica, em tempo de compilação, que existe exatamente uma atribuição garantida, nunca zero (o que deixaria o campo sem valor) nem mais de uma (o que contradiria a promessa de `final`).

```java
public class Circulo {
    static final double PI_APROXIMADO = 3.14159; // inicializado direto na declaração
    private final double raio;                   // inicializado no construtor

    Circulo(double raio) {
        this.raio = raio;
    }
}
```

Uma analogia: um campo `final` é como o número de série gravado permanentemente em um aparelho eletrônico no momento da fabricação — pode ser lido quantas vezes forem necessárias, mas nenhum processo posterior, seja qual for, consegue regravá-lo, porque a gravação foi feita para ser definitiva por definição.

Um exemplo real de uso é qualquer campo que representa uma identidade ou uma característica que não deve mudar depois que o objeto existe: o CPF de um `Cliente`, o código de um `Produto`, a data de criação de um registro. Alternativas a `final` incluem simplesmente não fornecer um método que altere aquele campo (confiando na disciplina do programador para nunca escrever tal método), mas isso não é verificado pelo compilador e pode ser quebrado por um descuido futuro, enquanto `final` transforma essa garantia em uma regra da linguagem, detectada automaticamente. Quando não usar `final`: em qualquer campo que legitimamente muda ao longo da vida do objeto — o saldo de uma conta, a quantidade em estoque de um produto — declarar como `final` impediria até mesmo as atualizações normais e esperadas daquele campo, quebrando a funcionalidade básica da classe.

### Métodos

O modificador `final` também pode ser aplicado a métodos, com um efeito diferente do aplicado a campos: um método `final` não pode ser sobrescrito (`@Override`) por nenhuma subclasse, mesmo que a classe permita herança normalmente. Isso conecta diretamente com o conceito de herança e sobrescrita já estudado no Módulo 8, adicionando uma forma de restringi-la seletivamente, método por método.

```java
public class ContaBancaria {
    private double saldo;

    final void registrarTransacao(String tipo, double valor) {
        System.out.println("[LOG] " + tipo + ": " + valor);
        // lógica de auditoria que não deve ser alterada por subclasses
    }
}

public class ContaPoupanca extends ContaBancaria {
    // void registrarTransacao(...) { ... } // erro de compilação: método é final
}
```

Sem esse modificador, nada impediria uma subclasse de sobrescrever `registrarTransacao` e alterar — ou até remover — a lógica de auditoria ali presente, comprometendo uma garantia que a classe base pretendia ser inviolável. É exatamente esse risco que `final` em métodos elimina: um comportamento que a classe base considera crítico ou definitivo, como o do exemplo acima, passa a se comportar sempre da mesma forma, não importa qual subclasse esteja em uso, mantendo os registros de transação confiáveis e consistentes em todo o sistema.

Isso é diferente de simplesmente "não documentar que o método não deveria ser sobrescrito" — uma orientação em comentário não é verificada por ninguém, enquanto `final` é imposto pelo compilador, que rejeita qualquer tentativa de sobrescrita com um erro claro, detectado antes mesmo do programa rodar.

Pensar nisso como uma cláusula contratual marcada como "não negociável" em um contrato-modelo usado por uma franquia ajuda a visualizar a ideia: cada unidade franqueada (subclasse) pode personalizar diversos aspectos do próprio funcionamento, mas certas cláusulas centrais (métodos `final`), definidas pela matriz (a classe base), permanecem exatamente as mesmas em todas as unidades, sem exceção.

Um exemplo real de uso são métodos que implementam regras de negócio críticas, validações de segurança, ou lógica que faz parte do "núcleo" do comportamento de uma classe e que, se alterada de forma inconsistente entre subclasses, quebraria pressupostos que outras partes do sistema fazem sobre aquele comportamento. Quando não usar `final` em métodos: sempre que a intenção de projeto é justamente permitir que subclasses personalizem aquele comportamento — como métodos pensados para polimorfismo, discutidos no Módulo 8 — marcar como `final` bloquearia exatamente a flexibilidade que a herança deveria oferecer, contrariando o propósito da hierarquia de classes.

### Classes

`final` também pode ser aplicado à própria classe, na sua declaração. Uma classe `final` não pode ser estendida por nenhuma outra classe — ou seja, nenhuma subclasse pode ser criada a partir dela usando `extends`, o que é uma restrição ainda mais ampla do que um método `final` isolado: em vez de proteger um comportamento específico, protege a classe inteira contra herança.

```java
public final class Configuracao {
    private String versao;
    // ...
}

// public class ConfiguracaoEspecial extends Configuracao { } // erro de compilação
```

O problema que classes `final` resolvem é impedir que uma classe, cujo comportamento e estrutura devem permanecer exatamente como definidos, seja estendida de formas imprevistas por outras partes do código (ou por outros programadores, em projetos maiores), o que poderia introduzir comportamentos inesperados em lugares que esperam usar aquela classe exatamente como foi projetada. Sem essa restrição, qualquer classe estaria sempre aberta a receber subclasses, mesmo quando isso nunca fez parte da intenção de quem a projetou.

Um exemplo bem conhecido no próprio Java é a classe `String`: ela é declarada como `final` justamente porque grande parte do sistema depende de que `String` se comporte sempre de um jeito absolutamente previsível e imutável — se qualquer código pudesse criar uma subclasse de `String` sobrescrevendo métodos como `equals` ou `length`, essa garantia de previsibilidade se perderia, com consequências sérias para qualquer código que dependesse dela.

Uma analogia: uma classe `final` é como uma peça de fundação de um prédio, projetada para não receber nenhuma modificação estrutural depois de instalada — diferente de uma parede interna (uma classe comum, aberta a herança), que pode ser adaptada, ampliada ou reconfigurada por quem constrói sobre ela, a fundação precisa permanecer exatamente como foi projetada, porque tudo o resto depende da sua estabilidade.

Um exemplo real de uso é qualquer classe que representa um valor imutável e completo em si mesma (parecido com o motivo pelo qual campos `final` protegem valores individuais, mas aplicado à classe inteira), ou classes utilitárias que só existem para agrupar métodos `static`, sem nenhuma razão para serem estendidas. Quando não usar `final` em classes: sempre que o próprio propósito da classe é servir de base para variações — como `Funcionario`, pensado desde o Módulo 8 para ser estendido por `Gerente` ou outras subclasses — marcar a classe como `final` eliminaria completamente essa possibilidade, contrariando o próprio motivo pelo qual ela foi desenhada daquela forma.

### static final

A combinação `static final`, aplicada a um campo, une os dois modificadores estudados neste módulo: o campo pertence à classe como um todo (uma única cópia compartilhada, não uma por objeto, como visto no capítulo anterior) e, além disso, não pode ser alterado depois de inicializado (como qualquer campo `final`). É a combinação mais comum usada para representar valores fixos e globais dentro de uma classe.

```java
public class Circulo {
    static final double PI = 3.14159265;
    private final double raio;

    Circulo(double raio) {
        this.raio = raio;
    }

    double calcularArea() {
        return PI * raio * raio;
    }
}
```

Usar apenas `static`, sem `final`, deixaria um valor como `PI` tecnicamente sujeito a ser reatribuído em algum ponto do código — o que quebraria a expectativa de que aquele número seja sempre o mesmo em todo o programa. `static final` fecha essa brecha: representa um valor que não varia entre objetos (por isso `static` — não faz sentido cada `Circulo` ter seu próprio "PI") e que também nunca deveria mudar depois de definido (por isso `final` — PI é uma constante matemática, alterá-lo em algum ponto seria um erro grave).

Por convenção amplamente seguida em Java, campos `static final` são nomeados inteiramente em letras maiúsculas, com palavras separadas por underscore (`PI`, `TAXA_MAXIMA`, `LIMITE_TENTATIVAS`), justamente para sinalizar visualmente, só pelo nome, que aquele campo é uma constante fixa e compartilhada, diferenciando-o à primeira vista de campos comuns de instância.

Pense em `static final` como uma medida oficial gravada em uma placa de metal fixada na parede de um laboratório: não pertence a nenhum experimento individual conduzido ali (é compartilhada por todos os experimentos que precisarem consultá-la), e nenhum pesquisador tem permissão de alterá-la, porque é uma referência fixa que todos precisam poder confiar que permanece igual, sempre.

Um exemplo real de uso é justamente o `PI` do exemplo acima: uma constante matemática que todo `Circulo` do programa precisa consultar, sempre com o mesmo valor. O próximo conceito, Constantes, aprofunda essa prática — nomear e centralizar valores fixos usados em vários pontos do código, em vez de duplicá-los manualmente — e o motivo pelo qual ela é tão valorizada. Quando não usar `static final`: sempre que o valor varia de objeto para objeto (então não deveria ser `static`) ou precisa poder mudar ao longo da execução do programa (então não deveria ser `final`) — forçar essa combinação nesses casos tornaria impossível representar corretamente o dado que a classe precisa guardar.

### Constantes

Uma constante, em Java, é o nome dado na prática a um campo declarado como `static final` (visto no conceito anterior) usado para representar um valor fixo, conhecido de antemão, que não muda durante a execução do programa. Este conceito fecha o capítulo e o módulo amarrando a técnica de `static final` a uma prática de programação concreta e amplamente recomendada: nunca espalhar valores fixos "soltos" pelo código, mas concentrá-los em constantes nomeadas.

```java
public class Pedido {
    static final double TAXA_ENTREGA = 15.0;
    static final int LIMITE_ITENS = 50;

    double calcularFrete(int quantidadeItens) {
        if (quantidadeItens > LIMITE_ITENS) {
            throw new IllegalArgumentException("Quantidade excede o limite permitido.");
        }
        return TAXA_ENTREGA;
    }
}
```

O problema que constantes resolvem é o de valores mágicos: números ou textos fixos escritos diretamente no meio da lógica do programa, sem nome nem explicação, como `if (quantidadeItens > 50)`. Um valor assim, espalhado em vários pontos do código, é difícil de entender (o que significa "50" ali, sem contexto?) e ainda mais difícil de manter — se o limite precisar mudar de 50 para 100, seria necessário localizar manualmente cada lugar onde o número "50" foi usado com esse significado específico, correndo o risco real de esquecer algum, já que o número "50" pode aparecer em outros lugares do código com significados completamente diferentes.

Substituir esse valor solto por uma constante nomeada (`LIMITE_ITENS`) resolve os dois problemas de uma vez: o nome já documenta o que aquele número representa, sem precisar de comentário adicional, e alterar o valor em um único lugar (a declaração da constante) propaga automaticamente a mudança para todo lugar do código que usa `LIMITE_ITENS`, porque todos consultam a mesma referência compartilhada.

Uma analogia: constantes são como os preços fixados em um cardápio impresso — em vez de o garçom decorar e repetir de memória, em cada mesa, o preço de cada prato (arriscando errar ou divergir de mesa para mesa), todos consultam a mesma fonte única, o cardápio, e uma eventual atualização de preço é feita uma única vez, na origem, refletindo automaticamente para qualquer pessoa que consulte o cardápio depois.

Um exemplo real de uso são limites de sistema, taxas fixas, mensagens padronizadas, nomes de configuração, ou qualquer valor repetido em múltiplos pontos da lógica de um programa. Uma alternativa mais avançada, fora do escopo deste módulo, é centralizar constantes usadas por várias classes em um arquivo de configuração externo, carregado em tempo de execução — mas o princípio de nomear e centralizar valores fixos é o mesmo. Quando não criar uma constante: para um valor usado uma única vez, em um único lugar do código, sem nenhum risco de ambiguidade sobre seu significado — nesse caso, extrair uma constante pode ser um passo a mais sem ganho real, embora, na dúvida, nomear o valor quase sempre melhore a legibilidade do código.

### Initialization blocks

Um bloco de inicialização (initialization block) é um trecho de código, delimitado apenas por chaves `{ }`, escrito diretamente no corpo de uma classe, fora de qualquer método ou construtor, que é executado automaticamente sempre que um objeto daquela classe é criado — antes do corpo do construtor ser executado. Existem também blocos de inicialização estáticos, marcados com a palavra `static` antes das chaves, que são executados uma única vez, quando a classe é carregada pela primeira vez, antes de qualquer objeto dela ser criado.

```java
public class Relatorio {
    private String cabecalho;
    static int contadorGlobal;

    static {
        contadorGlobal = 100; // executado uma única vez, ao carregar a classe
        System.out.println("Classe Relatorio carregada.");
    }

    {
        cabecalho = "Relatório padrão"; // executado toda vez que um objeto é criado
    }

    Relatorio() {
        System.out.println("Construtor executado, cabecalho já é: " + cabecalho);
    }
}
```

Quando uma classe tem dois ou três construtores diferentes, mas todos precisam executar a mesma preparação inicial antes de suas particularidades, repetir esse código idêntico em cada construtor é exatamente o tipo de duplicação que blocos de inicialização evitam: um bloco de instância executa essa parte comum automaticamente, antes de qualquer construtor. Já o bloco `static` resolve um problema análogo, mas para preparar algo relacionado à classe como um todo, uma única vez, independentemente de quantos objetos venham a ser criados depois — útil, por exemplo, para calcular um valor inicial complexo para um campo `static`, que não caberia em uma simples atribuição direta na linha de declaração.

Uma analogia: um bloco de inicialização de instância é como uma checagem de segurança padrão que todo passageiro passa antes de embarcar, independentemente de qual portão de embarque (construtor) ele usou — a checagem acontece sempre, de forma idêntica, antes de qualquer procedimento específico daquele portão. Já um bloco `static` é como a preparação única do próprio aeroporto antes de abrir para o primeiro voo do dia, feita uma vez só, não repetida a cada passageiro.

Um exemplo real de uso é inicializar estruturas de dados complexas em campos `static` (como preencher um mapa de valores fixos) logo quando a classe é carregada, ou garantir que todo objeto, não importa qual construtor use, comece com uma preparação básica comum. Uma alternativa amplamente preferida na prática é simplesmente colocar essa lógica comum diretamente dentro de um construtor principal e fazer os demais construtores chamá-lo (usando `this(...)`, técnica de encadeamento de construtores), o que costuma ser mais claro de ler do que um bloco solto no meio da classe — por isso blocos de inicialização, embora válidos e ocasionalmente úteis (principalmente os `static`, para preparar dados de classe), são menos comuns no dia a dia do que construtores bem escritos. Quando não usá-los: quando a mesma lógica pode ser expressa de forma mais legível dentro de um construtor comum, o que costuma ser o caso na maioria das classes simples.

### Ordem de inicialização

Este último conceito do módulo amarra, em sequência clara, tudo que foi visto sobre `static`, `final` e blocos de inicialização: a ordem exata em que Java prepara uma classe e cria um objeto dela, algo que passa despercebido na maior parte do tempo, mas que se torna importante para entender exatamente quando cada parte de uma classe está pronta para uso.

Quando uma classe é usada pela primeira vez em um programa (seja para criar um objeto, seja para acessar um membro `static`), Java primeiro carrega a classe e executa, nessa ordem: (1) inicialização dos campos `static` na ordem em que aparecem no código, e (2) os blocos de inicialização `static`, também na ordem em que aparecem — tudo isso acontece uma única vez, por classe, não importa quantos objetos venham a ser criados depois.

Só então, ao criar cada novo objeto com `new`, Java executa, nessa ordem: (1) inicialização dos campos de instância na ordem em que aparecem no código, (2) os blocos de inicialização de instância, também na ordem em que aparecem, e por fim (3) o corpo do construtor.

```java
public class Demonstracao {
    static int a = inicializar("campo static a");
    static { System.out.println("bloco static"); }

    int b = inicializar("campo de instância b");
    { System.out.println("bloco de instância"); }

    Demonstracao() {
        System.out.println("construtor");
    }

    static int inicializar(String msg) {
        System.out.println(msg);
        return 0;
    }
}
// new Demonstracao() imprime, nessa ordem:
// campo static a
// bloco static
// campo de instância b
// bloco de instância
// construtor
```

Sem entender essa ordem, é fácil escrever código que depende, sem perceber, de um campo que ainda não foi inicializado no momento em que é usado — por exemplo, um bloco de inicialização de instância que tenta usar um campo declarado logo depois dele no código, e que portanto ainda não recebeu valor algum, porque a inicialização segue estritamente a ordem em que os elementos aparecem no arquivo, de cima para baixo.

Uma analogia final para o módulo inteiro: pensar nessa ordem como a preparação de um teatro antes de uma peça. Primeiro, o prédio e a infraestrutura são preparados uma única vez, antes de qualquer sessão (a parte `static` da classe, carregada uma vez só); depois, para cada sessão individual (cada objeto criado), o palco é montado (campos e blocos de instância) e só então a peça começa de fato (o construtor), com tudo já preparado ao seu redor.

Compreender essa ordem completa o que este módulo construiu: partindo de `Object`, a base comum de todo objeto, passando por igualdade e hash, que definem como objetos se comparam entre si, até `static` e `final`, que definem onde um dado mora e se pode mudar — a ordem de inicialização é o fio que amarra tudo isso no momento exato em que um objeto, ou uma classe, passa a existir e estar pronto para uso, servindo de base para os módulos seguintes deste livro.

# Módulo 10 — Organização, erros e primeiro projeto

Integra os conhecimentos do nível.

Depois de aprender a construir objetos e compreender fundamentos do modelo de objetos, o passo seguinte é organizar esse código em uma estrutura de projeto mais próxima do que aparece em aplicações reais e aprender a lidar com situações em que a execução não segue o caminho esperado. Por isso, este módulo reúne visibilidade entre classes, packages e imports, tratamento inicial de exceções e assertions, encerrando o nível com um projeto que exige usar vários conhecimentos já estudados em conjunto.

Os capítulos formam uma transição entre exercícios isolados e uma aplicação pequena, mas organizada: primeiro o leitor controla quem pode acessar cada parte do código, depois distribui classes em packages, aprende a reconhecer e tratar falhas de execução e usa assertions para verificar premissas internas. No projeto final, essas ferramentas se juntam aos métodos, decisões, repetições, arrays, strings e objetos vistos anteriormente. Ao concluir o módulo, o leitor terá percorrido o ciclo completo do nível introdutório e será capaz de estruturar uma pequena aplicação Java com múltiplas classes e responsabilidades.

## Modificadores de acesso

À medida que um programa cresce e passa a ter várias classes, nem tudo deveria ficar acessível de qualquer lugar. Algumas partes representam a interface pública que outras classes podem usar; outras são detalhes internos que devem permanecer escondidos; e há situações intermediárias em que o acesso deve ser permitido apenas dentro do mesmo pacote ou em contextos específicos de herança.

Este capítulo apresenta os quatro níveis de acesso usados em Java — `public`, `private`, `protected` e o acesso package-private — e explica como eles controlam a visibilidade de classes e membros. O objetivo é dar ao leitor critérios para limitar dependências e proteger detalhes internos, preparando o caminho para o capítulo seguinte, em que as classes serão organizadas em packages.

### `public`

`public` é o modificador de acesso mais permissivo em Java: um membro (classe, campo, método ou construtor) marcado como `public` pode ser acessado de qualquer lugar do programa, sem restrição nenhuma — de dentro da mesma classe, de outras classes do mesmo pacote, ou de classes em pacotes completamente diferentes, desde que o pacote onde a classe `public` vive seja importado corretamente (assunto do próximo capítulo).

Até este ponto do livro, a maior parte dos exemplos já usava `public` de forma quase automática — em declarações como `public class Pessoa` ou `public void imprimir()` — sem que o motivo fosse explicado a fundo. O problema que `public` resolve é simples: sem alguma forma de expor deliberadamente uma classe, método ou campo, nada em Java seria utilizável fora do arquivo onde foi escrito. Um programa real é composto por muitas classes que precisam se comunicar — uma classe `Pedido` precisa que outras partes do sistema consigam criar objetos dela, chamar seus métodos e ler certos dados. Se tudo fosse fechado por padrão e nada pudesse ser aberto, cada classe viveria isolada, incapaz de colaborar com o resto do programa.

`public` resolve isso concedendo acesso irrestrito, mas o mecanismo carrega uma responsabilidade importante: tudo que é declarado `public` se torna parte do "contrato" da classe com o resto do mundo — outras partes do código passam a depender daquele nome, daquela assinatura de método, daquele campo, exatamente como estão. Por isso a prática recomendada é usar `public` de forma seletiva e consciente: expor apenas o que realmente precisa ser usado de fora, mantendo os detalhes internos de implementação protegidos com outros modificadores (vistos a seguir).

Um exemplo real: uma classe `ContaBancaria` normalmente declara sua classe como `public class ContaBancaria` (para que outras partes do sistema possam criar contas) e seus métodos de operação como `public void depositar(double valor)` e `public void sacar(double valor)` (para que sejam chamados de fora), mas mantém o campo `saldo` fora do `public` — o motivo exato dessa escolha fica claro a seguir, ao estudar `private`.

```java
public class ContaBancaria {
    private double saldo;

    public void depositar(double valor) {
        saldo += valor;
    }
}
```

Uma analogia: `public` é como a porta principal de uma loja, aberta a qualquer cliente que queira entrar e comprar — mas isso não significa que o estoque nos fundos, ou o cofre, também fiquem visíveis; a loja escolhe deliberadamente o que fica na vitrine (`public`) e o que fica reservado.

Não existe uma "alternativa" direta a `public` no sentido de outro modificador que faça a mesma coisa — os outros três modificadores (`private`, `protected` e package-private) existem justamente para restringir o que `public` libera totalmente. A única "alternativa" real é simplesmente não usar `public` quando o acesso amplo não é necessário, preferindo um modificador mais restrito. Quando não usar `public`: em qualquer campo ou método que representa um detalhe interno de implementação, que pode mudar no futuro sem que o resto do programa precise saber — nesses casos, `public` expõe mais do que deveria e reduz a liberdade de evoluir a classe depois.

### `private`

`private` é o modificador mais restritivo: um membro marcado como `private` só pode ser acessado de dentro da própria classe onde foi declarado — nem outras classes do mesmo pacote, nem subclasses, conseguem enxergá-lo diretamente.

O problema que `private` resolve já apareceu de forma implícita em módulos anteriores deste livro, quando se falou em encapsulamento: sem uma forma de esconder o estado interno de um objeto, qualquer parte do programa poderia alterar diretamente os campos de uma classe, ignorando validações e regras de negócio. Por exemplo, sem `private`, nada impediria um código externo de fazer `conta.saldo = -1000000`, colocando o objeto em um estado inválido, sem passar pelo método `sacar` que verifica se há saldo suficiente.

`private` resolve isso simplesmente proibindo esse acesso direto de fora da classe. O campo `saldo`, marcado `private`, só pode ser lido ou alterado por código escrito dentro da própria classe `ContaBancaria` — e é essa mesma classe que expõe métodos `public` controlados (como `depositar` e `sacar`) que decidem exatamente como e quando o campo pode mudar. A melhor forma de usar `private` é como padrão: declarar campos como `private` por padrão, e só relaxar essa restrição (para `protected` ou `public`) quando houver uma razão concreta para isso.

```java
public class ContaBancaria {
    private double saldo;

    public void sacar(double valor) {
        if (valor > saldo) {
            throw new IllegalStateException("Saldo insuficiente");
        }
        saldo -= valor;
    }

    private void registrarLog(String mensagem) {
        System.out.println("[LOG] " + mensagem);
    }
}
```

No exemplo, `registrarLog` também é `private`: é um método auxiliar interno, útil apenas para a própria classe organizar seu código, sem sentido nenhum de ser chamado de fora.

Uma analogia: `private` é como o diário pessoal de alguém — só o dono tem acesso, e qualquer informação nele só pode ser modificada por ele mesmo, nunca lida ou alterada diretamente por outra pessoa; quem quiser saber algo do diário depende de o dono decidir contar (equivalente a expor um método `public` que devolve uma informação controlada).

A principal alternativa a `private` são os outros três níveis de acesso, cada um relaxando a restrição em um grau diferente — `protected` e package-private permitem visibilidade parcial, `public` remove a restrição por completo. Quando não usar `private`: quando o membro realmente precisa ser acessado de fora da classe para que o programa funcione — nesse caso, `private` demais só forçaria a criação de métodos de acesso desnecessários só para contornar a própria restrição, o que indica que talvez outro modificador seja mais adequado.

### `protected`

`protected` ocupa uma posição intermediária entre `private` e `public`: um membro `protected` pode ser acessado de dentro da própria classe, de outras classes do mesmo pacote, e também de subclasses, mesmo que essas subclasses estejam em um pacote diferente.

Esse conceito só faz sentido pleno quando pensado ao lado de herança — tema que este nível introdutório apresenta de forma inicial e que será aprofundado em módulos futuros do livro. O problema que `protected` resolve aparece exatamente nesse cenário: imagine uma classe `Veiculo` com um campo interno que representa, por exemplo, a quilometragem acumulada. Se esse campo fosse `private`, nenhuma subclasse (como `Carro` ou `Moto`, que estendem `Veiculo`) conseguiria acessá-lo diretamente, mesmo herdando o comportamento da classe-mãe — o que às vezes é exagerado demais, pois a subclasse é, em certo sentido, uma extensão natural da classe original e legitimamente precisa desse acesso.

`protected` resolve esse impasse abrindo uma exceção específica para subclasses (além do acesso já concedido a classes do mesmo pacote, que o package-private já oferece). A melhor forma de usá-lo é reservada a membros que fazem sentido como parte do "contrato interno" de uma família de classes relacionadas por herança — algo que uma subclasse precisa herdar e manipular, mas que ainda não deveria ficar aberto a qualquer código externo não relacionado.

```java
public class Veiculo {
    protected int quilometragem;

    protected void registrarViagem(int km) {
        quilometragem += km;
    }
}

public class Carro extends Veiculo {
    void rodar(int km) {
        registrarViagem(km); // acesso permitido: Carro herda de Veiculo
    }
}
```

Uma analogia: `protected` é como uma receita de família passada apenas para quem nasce ou entra nessa família (as subclasses) e para quem convive na mesma casa (o mesmo pacote) — qualquer pessoa de fora da família e da casa não tem acesso a ela, mas um parente distante que "herda" o sobrenome consegue usá-la, mesmo morando em outro lugar (outro pacote).

A alternativa mais próxima é package-private, que oferece acesso a classes do mesmo pacote mas não estende esse acesso a subclasses fora dele; `public` seria uma alternativa exagerada, e `private` uma alternativa restritiva demais para esse cenário específico de herança. Quando não usar `protected`: em qualquer classe que não faz parte de uma hierarquia de herança pensada deliberadamente — usar `protected` sem um motivo ligado a subclasses só adiciona uma exposição desnecessária, sem trazer benefício real, já que o mesmo efeito dentro do pacote já seria alcançado com package-private.

### Package-private

Package-private é o nome dado ao nível de acesso que um membro recebe quando **nenhum** modificador (`public`, `private` ou `protected`) é escrito antes dele — por isso também é chamado de acesso "padrão" (default). Um membro package-private pode ser acessado por qualquer classe do mesmo pacote, mas não por classes de outros pacotes, mesmo que sejam subclasses.

Sem esse nível intermediário, o programador ficaria preso a apenas dois extremos: `private` (só a própria classe) ou `public` (o mundo inteiro), sem uma opção de meio-termo para o caso comum de várias classes que trabalham juntas dentro do mesmo pacote, colaborando de perto, mas que não precisam (nem devem) expor esses detalhes para o resto do programa. Um sistema real costuma organizar classes relacionadas no mesmo pacote — por exemplo, um pacote `pedidos` com as classes `Pedido`, `ItemPedido` e `CalculadoraDeFrete` — e é natural que essas classes compartilhem alguns detalhes internos entre si sem que esses detalhes vazem para outros pacotes do sistema, como `relatorios` ou `pagamentos`.

Package-private resolve exatamente isso: basta omitir o modificador para que o membro fique acessível dentro do pacote, mas isolado de fora dele.

```java
class CalculadoraDeFrete { // package-private: sem modificador antes de "class"
    double calcular(ItemPedido item) {
        return item.getPeso() * 0.5;
    }
}
```

Nesse exemplo, tanto a classe `CalculadoraDeFrete` quanto o método `calcular` são package-private — qualquer classe dentro do pacote `pedidos` pode usá-los livremente, mas uma classe do pacote `relatorios` não teria acesso, mesmo importando o pacote.

Uma analogia: package-private é como uma conversa entre colegas de um mesmo escritório, ouvida por qualquer pessoa que trabalhe ali, mas inaudível para quem está em outra sala ou em outro andar do prédio — não é um segredo tão fechado quanto `private` (que seria uma conversa só consigo mesmo), mas também não é uma declaração pública gritada para o prédio inteiro.

Como alternativa, `protected` oferece o mesmo acesso dentro do pacote e ainda estende esse acesso a subclasses externas; `private` seria mais restritivo, `public` mais permissivo. Quando não usar package-private: quando classes de outros pacotes legitimamente precisam acessar aquele membro — nesse caso, a ausência de modificador bloquearia um acesso necessário, e `public` (ou eventualmente `protected`) seria a escolha correta.

## Packages e imports

Um projeto com poucas classes ainda pode caber confortavelmente em uma única pasta, mas essa organização deixa de funcionar à medida que o código cresce. Java usa packages para agrupar classes relacionadas em espaços de nomes, evitando conflitos entre nomes iguais e deixando explícita a organização lógica do sistema. Os imports, por sua vez, permitem usar classes de outros packages sem repetir seus nomes completos a cada referência.

Neste capítulo, o leitor aprenderá como declarar um `package`, usar `import`, organizar arquivos de acordo com essa estrutura e compreender como os modificadores de acesso interagem com os limites entre pacotes. Ao final, será possível distribuir as classes de um projeto em grupos coerentes e entender por que o nome completo de uma classe inclui também o package ao qual ela pertence.

### `package`

A palavra-chave `package`, escrita na primeira linha (não em branco e sem comentário) de um arquivo `.java`, declara a qual pacote aquela classe pertence. Um pacote é, na prática, uma pasta no sistema de arquivos e, ao mesmo tempo, um espaço de nomes lógico que organiza e agrupa classes relacionadas — a declaração `package com.empresa.pedidos;` no topo de um arquivo indica que a classe ali definida faz parte do pacote `com.empresa.pedidos`, e o arquivo `.java` correspondente deve estar fisicamente na pasta `com/empresa/pedidos/`.

Sem pacotes, todas as classes de um programa Java viveriam em um único espaço "global" (o chamado pacote-padrão, sem nome). Isso funciona para programas pequenos e exemplos isolados, como boa parte dos vistos até aqui neste livro, mas se torna insustentável em qualquer projeto real: duas bibliotecas diferentes poderiam ter uma classe chamada `Utils` ou `Cliente`, e o compilador não teria como diferenciá-las, gerando conflito de nomes. Além disso, sem pacotes não haveria uma forma natural de agrupar classes relacionadas — tudo ficaria misturado em uma lista só, dificultando localizar e entender a organização de um sistema com centenas de classes.

`package` resolve isso dando a cada classe um nome completo e único, composto pelo nome do pacote mais o nome da classe (`com.empresa.pedidos.Pedido`, por exemplo) — mesmo que existam duas classes `Pedido` no mundo, desde que estejam em pacotes diferentes, seus nomes completos nunca colidem. A convenção usada pela comunidade Java é nomear pacotes com o domínio da organização invertido (`com.empresa.projeto`), tudo em letras minúsculas, o que reduz drasticamente a chance de colisão entre pacotes de organizações diferentes.

```java
package com.empresa.pedidos;

public class Pedido {
    // ...
}
```

Pense no sistema de sobrenomes e endereços de uma cidade grande: pode haver muitas pessoas chamadas "João" na cidade inteira, mas "João, que mora na Rua das Flores, 12" é uma identificação única; da mesma forma, pode haver muitas classes `Pedido`, mas `com.empresa.pedidos.Pedido` é uma só.

Não existe alternativa real a `package` em Java para organizar classes em escala — é o único mecanismo nativo da linguagem para isso. A alternativa mais próxima seria simplesmente não organizar nada e deixar tudo no pacote-padrão, o que é aceitável apenas para protótipos rápidos ou exemplos isolados de estudo, exatamente o tipo de código visto nos módulos iniciais deste livro. Vale a pena declarar `package` explicitamente assim que o projeto passa de um único arquivo isolado; só em exercícios curtos, scripts de teste ou classes soltas de aprendizado essa organização formal ainda não compensa a complexidade extra.

### `import`

`import` é a instrução, escrita no topo de um arquivo `.java` (depois do `package`, se houver), que permite referenciar uma classe de outro pacote pelo nome simples, sem precisar escrever o nome completo (qualificado) toda vez que ela é usada.

Sem `import`, para usar uma classe de outro pacote seria necessário escrever seu nome completo em cada ocorrência: `java.util.ArrayList<String> lista = new java.util.ArrayList<>();`, por exemplo, em vez do familiar `ArrayList<String> lista = new ArrayList<>();` usado em módulos anteriores deste livro ao trabalhar com coleções. Em um arquivo que usa dezenas de classes de bibliotecas diferentes, escrever o caminho completo repetidamente tornaria o código verboso e difícil de ler.

`import` resolve isso com uma única declaração no topo do arquivo, que "ensina" ao compilador qual classe completa corresponde a um nome simples usado no restante do código.

```java
package com.empresa.pedidos;

import java.util.ArrayList;
import java.util.List;

public class ProcessadorPedidos {
    void processar() {
        List<String> nomes = new ArrayList<>();
        nomes.add("Camiseta");
    }
}
```

Também é possível importar todas as classes de um pacote de uma vez com `import java.util.*;`, mas a prática recomendada é evitar esse formato em código de produção, preferindo importar cada classe individualmente — isso deixa explícito, só de olhar o topo do arquivo, exatamente quais classes externas aquele arquivo depende, o que ajuda tanto na leitura quanto em ferramentas automáticas que analisam dependências.

Um caso à parte, que não precisa de `import`, são as classes do pacote `java.lang` (como `String`, `System` e `Object`, já usadas extensivamente neste livro) — esse pacote é importado automaticamente e implicitamente em todo arquivo Java, por conter os tipos mais fundamentais da linguagem.

É como adicionar um contato à agenda do celular com um apelido curto: em vez de digitar o número completo toda vez que for ligar, basta usar o apelido, e o celular sabe automaticamente a quem se refere.

Como alternativa, sempre existe a opção de escrever o nome completo (qualificado) da classe diretamente no código, sem `import` nenhum — algo raramente feito, exceto no caso específico de duas classes com o mesmo nome simples vindas de pacotes diferentes, onde usar o nome completo em pelo menos uma delas é necessário para desambiguar. Dois casos dispensam o `import`: quando a classe já pertence ao mesmo pacote do arquivo atual, e quando ela faz parte de `java.lang` (importado automaticamente) — declará-lo nessas situações é redundante e normalmente sinalizado como desnecessário pela própria IDE.

### Organização

Organização, neste contexto, se refere às convenções práticas que orientam como dividir um projeto Java real em pacotes — decisões que vão além da sintaxe de `package` e `import` já vistas, e que fazem a diferença entre um projeto navegável e um amontoado de classes soltas.

O problema que essas convenções resolvem aparece à medida que um projeto cresce. Um projeto pequeno, como os exercícios isolados vistos em módulos anteriores deste livro, pode viver perfeitamente sem estrutura de pacotes. Mas um sistema real, com dezenas ou centenas de classes, precisa de algum critério consistente para decidir onde cada classe mora — sem isso, encontrar uma classe específica, entender como as partes do sistema se relacionam, ou saber onde adicionar uma nova funcionalidade se torna cada vez mais difícil conforme o projeto cresce.

Existem, na prática, duas estratégias comuns de organização, e ambas resolvem o problema de formas diferentes. A organização "por camada técnica" agrupa classes pelo papel técnico que exercem: um pacote `modelo` (ou `entidades`) para as classes que representam dados, um pacote `servicos` para a lógica de negócio, um pacote `utils` para utilitários genéricos. A organização "por funcionalidade" (ou "por domínio") agrupa, em vez disso, tudo que está relacionado a uma mesma parte do negócio no mesmo pacote — um pacote `pedidos` conteria a entidade `Pedido`, o serviço que processa pedidos e utilitários específicos de pedidos, todos juntos; um pacote `clientes` faria o mesmo para tudo relacionado a clientes.

```
com.empresa.loja
├── pedidos
│   ├── Pedido.java
│   └── ServicoPedido.java
└── clientes
    ├── Cliente.java
    └── ServicoCliente.java
```

Uma analogia: organizar por camada técnica é como uma biblioteca que separa livros por formato (todos os livros de capa dura numa ala, todos os de bolso em outra), enquanto organizar por funcionalidade é como separar por assunto (todos os livros de história juntos, não importa o formato) — cada critério facilita encontrar coisas de um jeito diferente, dependendo do que se está procurando.

Um exemplo real: projetos que crescem bastante costumam migrar, com o tempo, de organização por camada técnica para organização por funcionalidade, porque fica mais fácil localizar e entender tudo relacionado a uma parte específica do negócio quando essas classes vivem próximas, em vez de espalhadas entre pacotes técnicos diferentes.

Não existe uma resposta única e universalmente correta — a alternativa a cada uma dessas estratégias é sempre a outra, e a escolha depende do tamanho e da natureza do projeto. Quando não se preocupar tanto com essa organização: em projetos pequenos, de aprendizado ou protótipos rápidos, onde a estrutura de pacotes tende a atrapalhar mais do que ajudar — o valor da organização cresce junto com o tamanho do projeto, e é menor no início da jornada representada por este livro.

### Acesso entre pacotes

Acesso entre pacotes junta, na prática, dois assuntos já vistos separadamente: os modificadores de acesso (capítulo anterior) e a divisão do código em pacotes (início deste capítulo). A pergunta que este conceito responde é: dado que uma classe está em um pacote, o que exatamente uma classe de **outro** pacote consegue enxergar dela?

Sem entender essa interação, é fácil escrever código que compila dentro do próprio pacote durante os testes, mas falha ao ser usado de outro lugar do sistema — um erro comum é declarar uma classe inteira como package-private (sem `public` antes de `class`), sem perceber que isso a torna completamente invisível para qualquer código fora daquele pacote, mesmo que seus métodos individuais estejam marcados como `public`.

A regra combina os dois níveis: primeiro, a própria classe precisa ser `public` para ser acessível de outro pacote — se a classe não for `public`, nenhum membro dela importa, porque a classe inteira já está fora de alcance. Satisfeita essa condição, cada membro individual (campo, método, construtor) segue sua própria regra: `public` continua acessível de qualquer lugar; `protected` fica acessível a subclasses de outros pacotes e a classes do mesmo pacote; package-private e `private` permanecem inacessíveis de fora do pacote (package-private também é inacessível de fora, mesmo para subclasses; `private` é inacessível até mesmo dentro do mesmo pacote, exceto na própria classe).

```java
// arquivo: com/empresa/pedidos/Pedido.java
package com.empresa.pedidos;

public class Pedido {
    public String descricao;
    String codigoInterno; // package-private
}

// arquivo: com/empresa/relatorios/Relatorio.java
package com.empresa.relatorios;

import com.empresa.pedidos.Pedido;

public class Relatorio {
    void gerar(Pedido pedido) {
        System.out.println(pedido.descricao);   // ok: public
        // System.out.println(pedido.codigoInterno); // erro: package-private, outro pacote
    }
}
```

Uma analogia: pense em um prédio (o pacote) com uma porta de entrada (a classe). Se a porta do prédio está trancada (classe não `public`), não importa que salas internas estejam com a porta aberta — ninguém de fora consegue nem entrar no prédio para chegar até elas. Só depois de entrar pela porta principal aberta é que as regras de cada sala interna (cada membro) passam a valer.

Um exemplo real de uso correto dessa combinação: bibliotecas Java expõem deliberadamente apenas algumas classes como `public` (a "interface pública" da biblioteca) e mantêm classes auxiliares internas como package-private, para poder alterar esses detalhes internos livremente entre versões, sem quebrar o código de quem usa a biblioteca — ninguém de fora depende do que nunca esteve acessível.

Não há uma alternativa a essa combinação de regras — é assim que a linguagem funciona. O único "ajuste" possível é decidir, para cada classe e membro, qual nível de acesso é apropriado, sabendo que essas duas camadas (classe e membro) trabalham juntas. Quando isso não é uma preocupação central: em código dentro de um único pacote pequeno, onde tudo é usado internamente e a fronteira entre pacotes ainda não entra em jogo.

## Primeiras exceções

Programas falham por motivos que nem sempre podem ser evitados apenas escrevendo a lógica principal corretamente: uma conversão pode receber um valor inválido, uma operação pode encontrar uma condição inesperada ou um método pode não conseguir cumprir o que foi solicitado. Java representa muitos desses problemas por meio de exceções, objetos que carregam informações sobre uma situação anormal ocorrida durante a execução.

Este capítulo faz uma primeira introdução ao mecanismo de exceções sem tentar esgotá-lo. O leitor verá o que é uma exceção, como interpretar um stack trace e como `try` e `catch` permitem interceptar determinadas falhas e decidir o que fazer em seguida. O foco é aprender a reconhecer e tratar erros básicos de execução, criando uma base para um estudo mais aprofundado de exceções em níveis posteriores.

### Exceção

Uma exceção, em Java, é um objeto que representa um evento anormal ocorrido durante a execução de um programa — um erro, uma condição inesperada, algo que impede o código de continuar seu fluxo normal naquele ponto. Exceções são a forma que Java usa para sinalizar que algo deu errado: uma divisão por zero, um índice de array fora dos limites, uma tentativa de abrir um arquivo que não existe.

Antes de existir um mecanismo de exceções (em linguagens mais antigas, como C), erros eram tipicamente sinalizados por meio de valores de retorno especiais — uma função podia retornar `-1` ou `null` para indicar falha, cabendo a quem chamou a função verificar manualmente, toda vez, se aquele valor especial apareceu. O problema dessa abordagem é que é fácil esquecer de checar, e o erro simplesmente passa despercebido, propagando um estado inválido silenciosamente pelo resto do programa, até causar um problema em um lugar completamente diferente de onde o erro realmente aconteceu — dificultando enormemente descobrir a causa raiz.

Exceções resolvem isso de um jeito fundamentalmente diferente: quando algo dá errado, o código "lança" (`throw`) um objeto de exceção, e a execução normal é imediatamente interrompida naquele ponto, subindo pela cadeia de chamadas de método até encontrar um trecho de código preparado para tratar aquele tipo de problema (usando `try`/`catch`, vistos nos próximos dois conceitos). Se nenhum trecho do programa tratar a exceção, o programa termina, exibindo informações detalhadas sobre o que aconteceu e onde — impossível de ignorar silenciosamente, ao contrário de um valor de retorno especial esquecido.

```java
public class Calculadora {
    int dividir(int a, int b) {
        if (b == 0) {
            throw new ArithmeticException("Divisão por zero");
        }
        return a / b;
    }
}
```

No exemplo, em vez de retornar um valor arbitrário (como `0` ou `-1`) para sinalizar o erro de divisão por zero, o método lança explicitamente uma exceção, tornando o problema impossível de passar despercebido.

Uma analogia: uma exceção é como o alarme de incêndio de um prédio — quando algo grave acontece, o alarme dispara e interrompe imediatamente a rotina normal, forçando todo mundo a reagir àquilo, em vez de deixar que o problema seja descoberto, silenciosamente, muito depois, já fora de controle.

Java oferece diversas classes de exceção prontas para situações comuns (`ArithmeticException`, `NullPointerException`, `IllegalArgumentException`, entre outras vistas ao longo do livro) e também permite criar classes de exceção personalizadas — assunto que ganhará mais profundidade em módulos futuros. Como alternativa mais simples a exceções, ainda existe a opção de retornar códigos de erro ou valores especiais (como `null` ou `Optional`, visto em módulos anteriores), mas essa alternativa costuma ser reservada a situações onde a "falha" é uma possibilidade normal e esperada do fluxo, não um evento verdadeiramente excepcional. Quando não usar exceções: para controlar fluxo normal e previsível do programa (como decidir se um loop deve continuar) — exceções são relativamente custosas e devem ser reservadas para situações genuinamente excepcionais, não como substituto de estruturas condicionais comuns.

### Stack trace

O stack trace é o relatório detalhado que Java produz automaticamente quando uma exceção não tratada chega ao topo do programa (ou quando é impresso manualmente) — ele lista, em ordem, toda a cadeia de chamadas de método que estava ativa no momento em que a exceção foi lançada, do ponto exato do erro até o método inicial que começou tudo.

Sem o stack trace, ao ver uma exceção o programador saberia apenas que algo deu errado e, talvez, uma mensagem genérica — mas não teria como saber exatamente onde, no meio de um programa com dezenas de métodos se chamando uns aos outros, o problema realmente começou. Depurar um erro sem essa informação seria como tentar encontrar uma agulha em um palheiro sem nenhuma pista de por onde começar a procurar.

O stack trace resolve isso registrando, no momento em que a exceção é criada, o estado exato da pilha de chamadas (a "call stack") — cada método que chamou outro método, até chegar ao ponto do erro. Quando a exceção não é tratada, Java imprime essa pilha inteira no console, do método mais interno (onde o erro aconteceu) até o mais externo, cada linha indicando a classe, o método e o número exato da linha de código envolvidos.

```java
public class Programa {
    public static void main(String[] args) {
        processarPedido();
    }

    static void processarPedido() {
        calcularTotal();
    }

    static void calcularTotal() {
        int resultado = 10 / 0;
    }
}
```

Executar esse código produz um stack trace parecido com:

```
Exception in thread "main" java.lang.ArithmeticException: / by zero
    at Programa.calcularTotal(Programa.java:11)
    at Programa.processarPedido(Programa.java:7)
    at Programa.main(Programa.java:3)
```

Lendo de cima para baixo, fica claro exatamente onde o erro ocorreu (`calcularTotal`, linha 11) e todo o caminho de chamadas que levou até ali (`processarPedido` chamou `calcularTotal`, e `main` chamou `processarPedido`) — informação essencial para localizar e corrigir o problema rapidamente.

Uma analogia: o stack trace é como a caixa-preta de um avião depois de um incidente — não diz só que algo deu errado, mas reconstrói passo a passo a sequência exata de eventos que levou até o problema, permitindo investigar a causa raiz com precisão, em vez de adivinhar.

Uma alternativa (complementar, não substituta) ao stack trace padrão é usar ferramentas de log mais estruturadas, que registram esse tipo de informação de forma organizada e pesquisável em arquivos, especialmente útil em sistemas grandes rodando em produção. Quando o stack trace completo não é a informação mais útil: em mensagens voltadas ao usuário final de uma aplicação, que não deveria ver detalhes técnicos internos como nomes de classes e números de linha — nesses casos, o stack trace é registrado internamente (em log) enquanto o usuário recebe uma mensagem mais simples e compreensível.

### `try`

O bloco `try` delimita uma região de código que pode potencialmente lançar uma exceção, sinalizando à JVM: "monitore este trecho — se algo der errado aqui dentro, não deixe o programa simplesmente parar, dê a chance de um bloco `catch` associado tratar o problema".

Sem `try`, qualquer exceção lançada dentro de um método se propagaria automaticamente para quem chamou aquele método (e assim por diante, subindo a pilha de chamadas), até ser tratada em algum lugar ou até derrubar o programa inteiro, como descrito no conceito de stack trace. Isso é exatamente o comportamento correto em muitos casos — nem todo trecho de código precisa (ou deve) tratar seus próprios erros. Mas quando existe uma forma razoável de reagir a um erro específico, sem precisar interromper todo o programa, é preciso um mecanismo para "capturar" a exceção ali mesmo, no ponto certo.

`try` resolve isso demarcando explicitamente onde essa possível interrupção deve ser observada. O bloco `try` sozinho, sem um `catch` (ou `finally`, visto em módulos futuros) associado, não compila — ele precisa vir acompanhado de pelo menos um desses blocos, porque delimitar uma região "monitorada" só faz sentido se existir também algum plano do que fazer quando algo dá errado ali dentro.

```java
public class LeitorDeNumero {
    void processar(String texto) {
        try {
            int numero = Integer.parseInt(texto);
            System.out.println("Número: " + numero);
        } catch (NumberFormatException e) {
            System.out.println("Texto inválido: " + texto);
        }
    }
}
```

Nesse exemplo, `Integer.parseInt(texto)` lançaria uma exceção se `texto` não representar um número válido — colocando essa chamada dentro de um `try`, o programa consegue reagir a essa falha (imprimindo uma mensagem amigável) em vez de encerrar abruptamente.

Vale manter o bloco `try` o mais restrito possível, envolvendo apenas as linhas que de fato podem lançar a exceção que se pretende tratar — colocar código demais dentro de um `try` torna mais difícil saber exatamente qual linha causou qual problema, e pode acabar capturando (ou escondendo) exceções de outras partes do código sem intenção.

Pense em uma rede de segurança sob um trapezista durante um número específico do espetáculo — a rede só precisa estar ali durante a parte arriscada, não o show inteiro; da mesma forma, o `try` só deve envolver o trecho realmente sujeito a falhar.

Java não oferece um mecanismo alternativo a `try` para capturar exceções — é a única forma de fazer isso na linguagem. Quando não usar `try`: em código onde não há nenhuma ação sensata a se tomar diante de uma falha específica além de deixá-la propagar — nesses casos, é mais honesto deixar a exceção subir naturalmente (sem `try`) do que capturá-la só para não fazer nada com ela, o que esconderia o problema sem resolvê-lo.

### `catch`

O bloco `catch`, sempre associado a um `try`, especifica o tipo de exceção que deve ser tratado e contém o código que reage a ela, caso uma exceção daquele tipo (ou de um subtipo dela) seja lançada dentro do `try` correspondente.

Sem `catch`, o `try` não teria propósito nenhum — delimitar uma região monitorada sem nenhuma instrução do que fazer quando um problema é detectado ali seria inútil; é justamente o `catch` que dá significado prático ao monitoramento do `try`, fornecendo a resposta ao problema.

`catch` resolve isso recebendo o objeto de exceção lançado (atribuído a uma variável, como `e` no exemplo do conceito anterior) e executando um bloco de código específico para lidar com aquela situação — seja exibindo uma mensagem, tentando uma abordagem alternativa, registrando o erro em log, ou qualquer outra reação apropriada ao contexto. Um `try` pode ter vários blocos `catch` associados, cada um tratando um tipo diferente de exceção, permitindo reações específicas para problemas diferentes.

```java
public class ConversorDeIdade {
    void processar(String texto) {
        try {
            int idade = Integer.parseInt(texto);
            if (idade < 0) {
                throw new IllegalArgumentException("Idade não pode ser negativa");
            }
            System.out.println("Idade válida: " + idade);
        } catch (NumberFormatException e) {
            System.out.println("Erro: texto não é um número válido.");
        } catch (IllegalArgumentException e) {
            System.out.println("Erro: " + e.getMessage());
        }
    }
}
```

Nesse exemplo, o primeiro `catch` trata o caso de `texto` não ser um número (`NumberFormatException`), e o segundo trata o caso de a idade ser negativa (`IllegalArgumentException`, lançada manualmente com `throw`) — cada problema recebe seu próprio tratamento apropriado, em vez de um único `catch` genérico tentando lidar com tudo da mesma forma.

A melhor forma de usar `catch` é capturando o tipo mais específico de exceção que faz sentido para aquele contexto, e evitando capturar genericamente `Exception` (a superclasse de praticamente todas as exceções) a menos que exista uma razão clara para isso — capturar de forma ampla demais corre o risco de silenciar e esconder problemas completamente diferentes daquele que se pretendia tratar, dificultando encontrá-los depois.

É como diferentes protocolos de resposta a emergência — o protocolo para incêndio é diferente do protocolo para vazamento de água, mesmo que ambos sejam "emergências"; um `catch` bem escrito reage de forma apropriada ao tipo específico de problema, em vez de aplicar a mesma resposta genérica a qualquer coisa que dê errado.

Não existe alternativa a `catch` dentro do mecanismo de `try`/`catch` — é a peça que completa o par. Quando não usar `catch` (ou melhor, quando evitar capturar um tipo específico): quando não existe nenhuma ação razoável a se tomar para aquele tipo particular de erro no ponto atual do código — nesse caso, é preferível deixar a exceção se propagar para ser tratada em um nível mais apropriado, em vez de escrever um `catch` vazio ou que apenas ignora o problema silenciosamente, prática considerada uma má prática séria em Java.

## Assertions

Nem toda condição incorreta durante a execução deve ser tratada como entrada inválida do usuário ou como uma exceção recuperável. Às vezes, o programador quer registrar no próprio código uma suposição interna que deveria ser sempre verdadeira se a lógica estiver correta — por exemplo, que um cálculo já validado nunca produza determinado estado impossível. Assertions existem para verificar esse tipo de premissa durante desenvolvimento e testes.

Este capítulo apresenta a instrução `assert`, mostra como ela é habilitada com `-ea` e discute seu uso para verificar invariantes internas. Também será feita a distinção essencial entre assertion e validação normal: assertions podem ser desativadas em execução e, por isso, não devem substituir verificações necessárias para dados externos ou regras de negócio. Ao final, o leitor saberá quando uma assertion ajuda a detectar bugs e quando ela seria a ferramenta errada.

### `assert`

`assert` é uma palavra-chave que declara uma condição que o programador espera que seja **sempre** verdadeira naquele ponto do código, durante a execução — se a condição avaliar como falsa, a JVM lança um `AssertionError`, interrompendo o programa.

Diferente de `try`/`catch`, que lida com condições de erro esperadas e recuperáveis (como um usuário digitando um texto inválido), `assert` foi criado para um problema distinto: verificar suposições internas do próprio código, coisas que, se o programa estiver correto, nunca deveriam ser falsas — bugs de lógica, não erros do mundo externo. Antes de existir `assert`, essas verificações eram feitas com comentários (`// aqui, x sempre deveria ser positivo`) que documentavam a intenção, mas não verificavam nada de fato, ou com `if`s manuais que lançavam exceções, misturando checagens de depuração com lógica de tratamento de erro real.

`assert` resolve isso oferecendo uma forma dedicada de expressar "eu acredito que isto é verdade aqui; se não for, há um bug". A sintaxe é `assert condicao;` ou `assert condicao : mensagem;`, onde a mensagem (opcional) aparece no erro caso a condição falhe.

```java
public class CalculadoraDeMedia {
    double calcularMedia(int[] notas) {
        assert notas.length > 0 : "Array de notas não pode estar vazio";
        int soma = 0;
        for (int nota : notas) {
            soma += nota;
        }
        return (double) soma / notas.length;
    }
}
```

Nesse exemplo, o `assert` documenta e verifica a suposição de que `calcularMedia` nunca deveria ser chamado com um array vazio — se isso acontecer, é sinal de um bug em algum outro lugar do programa que está chamando o método incorretamente.

Uma analogia: `assert` é como uma checagem de sanidade que um engenheiro faz durante a construção de uma ponte — "esta viga deveria estar perfeitamente nivelada neste ponto"; se não estiver, algo saiu errado em uma etapa anterior da construção, e é melhor descobrir isso imediatamente do que só quando a ponte já estiver pronta e sob uso.

A melhor forma de usar `assert` é justamente para essas suposições internas — nunca para validar dados vindos de fora do programa (entrada do usuário, arquivos, rede), tema aprofundado no último conceito deste capítulo. Como alternativa, um `if` combinado com `throw` de uma exceção continua sendo válido e é a escolha certa quando a verificação precisa acontecer sempre, mesmo em produção (assertions podem ser desativadas, como visto no próximo conceito). Quando não usar `assert`: para validar entrada de usuário, argumentos de métodos `public` de uma biblioteca, ou qualquer condição que precise ser garantida mesmo quando assertions estiverem desligadas — nesses casos, uma exceção lançada manualmente é a ferramenta correta.

### `-ea`

`-ea` (de "enable assertions") é a flag de linha de comando que ativa a execução das instruções `assert` ao rodar um programa Java. Por padrão, sem essa flag, a JVM **ignora completamente** todas as instruções `assert` do código — elas são compiladas, mas simplesmente não são executadas em tempo de execução, como se não existissem.

Esse comportamento padrão surpreende muita gente que está começando a usar `assert`, e existe por uma razão histórica e prática deliberada: assertions foram pensadas como uma ferramenta de desenvolvimento e depuração, não como parte da lógica de produção de um programa. Se `assert` estivesse sempre ativo, o custo de processamento de todas essas verificações (que podem ser numerosas em um programa grande) recairia sobre todo mundo o tempo todo, mesmo em ambientes de produção onde a maioria dos bugs já deveria ter sido detectada durante o desenvolvimento e os testes.

`-ea` resolve isso deixando essa decisão explícita, sob controle de quem executa o programa. Para ativar, basta adicionar a flag ao comando `java`:

```
java -ea MinhaClasse
```

Sem `-ea`, o mesmo comando (`java MinhaClasse`) roda normalmente, mas qualquer `assert` no código é silenciosamente pulado, mesmo que a condição seria falsa. Isso significa que um programador pode escrever e testar seu código extensivamente com `-ea` ativado durante o desenvolvimento — pegando bugs de lógica cedo, através dos `AssertionError`s — e depois rodar o mesmo código em produção sem essa flag, sem o custo de processamento extra e sem risco de um `AssertionError` inesperado derrubar um sistema já em uso por usuários reais.

Um exemplo real de uso: em ambientes de teste automatizado e durante o desenvolvimento ativo de uma aplicação, é comum configurar a execução para sempre incluir `-ea`, exatamente para aproveitar ao máximo essas checagens internas antes que o código chegue à produção.

Uma analogia: `-ea` é como o modo de "checagem extra" que algumas ferramentas de engenharia oferecem durante testes de fábrica — ligado durante os testes internos, para pegar qualquer defeito antes de o produto sair da linha de produção, e desligado depois, quando o produto já foi validado e está em uso normal pelo cliente.

Não existe um comando alternativo direto a `-ea` para esse propósito específico — é a forma padrão da JVM de controlar assertions. Quando `-ea` não é necessário: em execuções normais de produção de um software já maduro e testado, onde o custo das verificações extras não se justifica mais — mas mesmo nesses casos, manter `-ea` ativo durante o desenvolvimento e os testes automatizados continua sendo uma prática recomendada.

### Invariantes internas

Uma invariante interna é uma condição sobre o estado de um programa (o valor de uma variável, o tamanho de uma coleção, a relação entre dois campos de um objeto) que deveria permanecer sempre verdadeira em determinado ponto do código, segundo a lógica que o próprio programador projetou — é exatamente o tipo de suposição que `assert` foi criado para verificar.

O problema que o conceito de invariante ajuda a resolver é dar um nome preciso a um tipo específico de suposição: aquela sobre o funcionamento correto do próprio código, e não sobre dados vindos de fora do programa — a diferença completa entre esses dois tipos de erro é o assunto do último conceito deste capítulo. Sem esse nome, é comum um programador tratar uma suposição interna como se precisasse da mesma checagem rigorosa de um dado externo, ou o contrário, aplicando a ferramenta errada ao problema errado.

Reconhecer uma invariante interna resolve essa confusão: ao identificar que uma condição é uma suposição sobre o funcionamento correto do próprio código fica claro que `assert` é a ferramenta certa para verificá-la, em vez de uma validação com `if`/`throw`.

```java
public class Pilha {
    private int[] elementos = new int[10];
    private int topo = -1;

    void empilhar(int valor) {
        assert topo < elementos.length - 1 : "Pilha cheia, empilhar não deveria ter sido chamado";
        elementos[++topo] = valor;
    }

    int desempilhar() {
        assert topo >= 0 : "Pilha vazia, desempilhar não deveria ter sido chamado";
        return elementos[topo--];
    }
}
```

Nesse exemplo, as condições verificadas (`topo` dentro dos limites do array) são invariantes internas da estrutura `Pilha`: se o restante do código que usa essa classe estiver correto, essas condições nunca deveriam falhar. Se falharem, é sinal de um bug em outro lugar do programa — não de um dado externo inválido.

Uma analogia: invariantes internas são como as regras físicas que um engenheiro assume ao projetar uma máquina — "esta engrenagem nunca deveria girar mais rápido que aquela outra"; se isso acontecer durante o funcionamento, não é o usuário da máquina que errou, é um defeito de projeto ou fabricação que precisa ser investigado e corrigido pelo próprio engenheiro.

A alternativa a formalizar invariantes com `assert` é simplesmente confiar na lógica do código sem verificação explícita nenhuma, documentando a suposição apenas em comentários — o que funciona até que um bug sutil quebre essa suposição silenciosamente, sem nenhum aviso. Quando invariantes internas não precisam de `assert`: quando a própria estrutura da linguagem já garante a condição de forma que ela simplesmente não pode ser violada (por exemplo, um tipo que só aceita valores dentro de um intervalo) — nesses casos, o compilador ou o sistema de tipos já faz esse trabalho, tornando a assertion redundante.

### Assertion × validação

Este último conceito do capítulo consolida a distinção mais importante sobre assertions: a diferença entre usar `assert` para verificar invariantes internas (como visto no conceito anterior) e usar validação — tipicamente feita com `if` e `throw`, como no capítulo de exceções — para checar dados que vêm de fora do programa.

A confusão entre esses dois usos é o erro mais comum que iniciantes cometem ao aprender assertions, e o motivo é sério o suficiente para merecer destaque próprio: como visto no conceito de `-ea`, assertions podem estar **desativadas** em produção (e frequentemente estão, pois essa é a configuração padrão da JVM). Se um programador usar `assert` para validar, por exemplo, a entrada de um usuário — checando se um valor digitado é positivo, se um texto não está vazio, se um parâmetro de um método `public` de uma biblioteca é válido — essa checagem simplesmente desaparece quando o programa roda sem `-ea`, deixando o sistema exposto a dados inválidos sem nenhuma proteção, de forma silenciosa e perigosa.

A regra prática que resolve essa confusão é simples: `assert` verifica suposições sobre o **próprio código** (algo que só pode ser falso se houver um bug de lógica); validação com exceção verifica dados que vêm de **fora do controle do programador** (entrada de usuário, arquivos, chamadas de rede, parâmetros de métodos expostos publicamente) — e essa checagem precisa acontecer sempre, incondicionalmente, porque dados do mundo externo são imprevisíveis por natureza.

```java
public class ContaBancaria {
    private double saldo;

    // Validação: saldo vem de uma chamada externa, precisa sempre ser checado
    void depositar(double valor) {
        if (valor <= 0) {
            throw new IllegalArgumentException("Valor de depósito deve ser positivo");
        }
        saldo += valor;
    }

    // Assertion: suposição interna sobre a lógica da própria classe
    private void verificarConsistencia() {
        assert saldo >= 0 : "Saldo não deveria ficar negativo — bug na lógica interna";
    }
}
```

Uma analogia: validação é como a checagem de segurança de um aeroporto, aplicada a todo passageiro, sempre, sem exceção, porque ninguém pode confiar de antemão em quem vem de fora; uma assertion é como um funcionário do próprio aeroporto conferindo, só durante um treinamento interno, se um procedimento interno está sendo seguido corretamente — uma checagem que só existe para ajudar a treinar e aperfeiçoar a própria operação, não para proteger contra o público externo, e que pode ser dispensada quando a operação já está rodando de forma madura e confiável.

Não existe uma "alternativa" a essa distinção — ela é a regra fundamental que determina qual ferramenta usar em cada situação. Quando a distinção fica menos óbvia: em métodos privados que só são chamados internamente pela própria classe, onde às vezes é discutível se um parâmetro é "externo" (validação) ou uma suposição interna sobre como a própria classe se chama (assertion) — nesses casos limítrofes, a pergunta prática a fazer é "isso pode falhar por causa de um bug no meu código, ou por causa de algo que vem de fora do meu controle?", e a resposta aponta a ferramenta certa.

## Projeto introdutório

Este capítulo final do Nível 1 — Introdutório integra, em uma única aplicação de linha de comando (CLI), os principais conceitos construídos ao longo dos dez módulos deste livro: classes e objetos, métodos, arrays, estruturas de decisão e repetição, encapsulamento e um primeiro uso de exceções.

Em vez de introduzir uma técnica isolada nova, o objetivo aqui é obrigar os conhecimentos anteriores a cooperarem dentro de um mesmo problema. O leitor precisará organizar dados e comportamentos em classes, dividir operações em métodos, percorrer coleções, tomar decisões, validar informações e lidar com falhas simples sem perder de vista a estrutura geral do programa. Ao final, o projeto funciona como uma síntese prática do nível e como preparação para exercícios maiores nos níveis seguintes.

### classes

Em um projeto CLI real, classes deixam de ser exemplos isolados (como os vistos em módulos anteriores) e passam a colaborar entre si para formar um sistema coeso — cada classe representando uma responsabilidade clara dentro do problema que o programa resolve.

O caminho mais fácil — e mais arriscado — na ausência de uma divisão pensada de responsabilidades entre classes é concentrar tudo em uma única classe gigante, com um único método `main` fazendo leitura de entrada, processamento de regras de negócio e exibição de resultado, tudo misturado. Esse tipo de código funciona no início, mas rapidamente se torna difícil de entender, testar e estender — qualquer mudança pequena arrisca quebrar partes completamente diferentes do programa, porque tudo está entrelaçado.

Um projeto introdutório bem estruturado resolve isso separando responsabilidades em classes distintas. Uma aplicação CLI simples de gerenciamento de tarefas, por exemplo, pode ter uma classe `Tarefa` (representando cada tarefa individual, com seus dados), uma classe `GerenciadorDeTarefas` (responsável pela coleção de tarefas e pelas operações sobre elas, como adicionar e remover) e uma classe `Principal` (responsável apenas por ler comandos do usuário e acionar as outras classes).

```java
public class Tarefa {
    private String descricao;
    private boolean concluida;

    public Tarefa(String descricao) {
        this.descricao = descricao;
        this.concluida = false;
    }

    public String getDescricao() {
        return descricao;
    }

    public boolean isConcluida() {
        return concluida;
    }

    public void concluir() {
        concluida = true;
    }
}
```

Uma analogia: dividir um programa em classes bem definidas é como montar uma equipe de trabalho onde cada pessoa tem um papel claro (quem atende o cliente, quem organiza os registros, quem toma as decisões finais), em vez de uma única pessoa tentando fazer tudo sozinha — cada papel bem definido facilita tanto o trabalho do dia a dia quanto trocar ou ajustar uma parte sem afetar as demais.

Nesse projeto, a classe `GerenciadorDeTarefas` guarda a coleção de objetos `Tarefa` (normalmente em um array, como será visto adiante) e expõe métodos para manipular essa coleção, mantendo a classe `Principal` livre de conhecer os detalhes internos de como as tarefas são armazenadas — apenas chamando os métodos disponibilizados. Essa divisão reaproveita diretamente o que os Módulos 1 a 9 já ensinaram sobre definição de classes, construtores e organização de código, agora aplicado a um problema completo, do início ao fim, em vez de exemplos isolados.

### métodos

Dentro de cada classe do projeto CLI, métodos são a unidade que organiza e nomeia cada ação que o programa realiza — desde operações simples, como adicionar uma tarefa, até operações mais elaboradas, como calcular quantas tarefas ainda estão pendentes.

O problema que uma boa divisão de métodos evita é o mesmo, em menor escala, do problema resolvido por dividir responsabilidades entre classes: um único método enorme, fazendo várias coisas diferentes ao mesmo tempo, é difícil de ler, de testar isoladamente e de reutilizar. Se toda a lógica de listar tarefas, marcar como concluída e calcular estatísticas estiver espremida dentro de um só método gigante dentro do `main`, qualquer ajuste em uma parte arrisca afetar as outras sem intenção.

A solução é a mesma já praticada em módulos anteriores deste livro: cada método deve fazer uma coisa bem definida, com um nome que descreva claramente essa ação, recebendo os parâmetros necessários e devolvendo (quando fizer sentido) um resultado.

```java
public class GerenciadorDeTarefas {
    private Tarefa[] tarefas = new Tarefa[100];
    private int quantidade = 0;

    public void adicionar(String descricao) {
        tarefas[quantidade] = new Tarefa(descricao);
        quantidade++;
    }

    public int contarPendentes() {
        int total = 0;
        for (int i = 0; i < quantidade; i++) {
            if (!tarefas[i].isConcluida()) {
                total++;
            }
        }
        return total;
    }

    public void listar() {
        for (int i = 0; i < quantidade; i++) {
            String status = tarefas[i].isConcluida() ? "[x]" : "[ ]";
            System.out.println(status + " " + tarefas[i].getDescricao());
        }
    }
}
```

Cada método acima — `adicionar`, `contarPendentes`, `listar` — tem uma única responsabilidade clara, e o método `main` do programa (na classe `Principal`) só precisa chamar esses métodos pelo nome, sem precisar saber como cada um funciona por dentro; essa é a mesma ideia de abstração já explorada em módulos anteriores, agora aplicada em conjunto.

Um paralelo ajuda a visualizar isso: métodos bem divididos dentro de uma classe são como os botões de um controle remoto — cada botão tem uma função específica e clara ("ligar", "aumentar volume"), e quem usa o controle não precisa saber a eletrônica interna por trás de cada botão, só precisa saber qual apertar para o efeito desejado.

Um exemplo real do valor dessa divisão: se depois for necessário mudar como `contarPendentes` calcula o total (por exemplo, ignorando tarefas marcadas como "arquivadas", uma regra nova), a mudança fica isolada dentro daquele único método, sem exigir tocar em `adicionar` ou `listar`. Esse projeto reaproveita diretamente o conteúdo sobre definição, parâmetros e retorno de métodos construído ao longo do livro, agora orquestrando várias classes que conversam entre si através de métodos bem definidos.

### arrays

O projeto CLI usa um array (`Tarefa[] tarefas`, como visto no exemplo do conceito anterior) como a estrutura que guarda a coleção de tarefas do programa — a mesma estrutura de dados de tamanho fixo estudada em módulos anteriores, agora aplicada para armazenar objetos completos, não apenas valores simples.

Usar um array aqui exige lidar com uma limitação já conhecida: seu tamanho é fixo, definido no momento da criação (`new Tarefa[100]`), e não pode crescer depois. Sem cuidado, o programa poderia tentar adicionar uma tarefa além dessa capacidade e falhar com um erro de índice fora dos limites — o mesmo tipo de problema que motivou, em módulos anteriores, a introdução de estruturas de tamanho dinâmico, mas que neste projeto introdutório é usado deliberadamente da forma mais simples, com um array de tamanho fixo, para consolidar o conceito antes de partir para alternativas mais flexíveis em módulos futuros.

A solução prática dentro desse projeto é controlar manualmente, com uma variável separada (`quantidade`), quantas posições do array já estão de fato em uso — o array reserva espaço para 100 tarefas, mas `quantidade` diz exatamente até onde os dados são válidos, e todo laço que percorre o array usa esse limite, não o tamanho total do array.

```java
public void adicionar(String descricao) {
    if (quantidade == tarefas.length) {
        System.out.println("Limite de tarefas atingido.");
        return;
    }
    tarefas[quantidade] = new Tarefa(descricao);
    quantidade++;
}
```

Essa checagem antes de adicionar evita o erro de índice fora dos limites, tratando de forma segura o caso em que o array já está cheio — uma aplicação direta do controle de limites já treinado em exercícios anteriores com arrays, agora dentro de um cenário real de uso.

Uma analogia: usar um array de tamanho fixo para guardar as tarefas é como reservar uma prateleira com exatamente 100 espaços numerados para caixas — a prateleira não estica, então é preciso sempre saber quantos espaços já estão ocupados antes de tentar colocar mais uma caixa, para não tentar empilhar além do que a prateleira suporta.

Um exemplo real de uso: o array percorrido em `listar` e em `contarPendentes` (mostrados no conceito de métodos) usa exatamente esse padrão — um laço `for` controlado por `quantidade`, não por `tarefas.length`, evitando processar posições vazias (ainda não preenchidas) do array. Esse projeto reaproveita diretamente a manipulação de arrays construída em módulos anteriores, incluindo o cuidado com seus limites, mas agora dentro do contexto de um programa maior e com propósito prático definido.

### decisões

Estruturas de decisão (`if`, `else`, e o operador ternário, todos vistos em módulos anteriores) aparecem por todo o projeto CLI, sempre que o programa precisa se comportar de forma diferente dependendo de uma condição — desde decidir o que fazer com um comando digitado pelo usuário até decidir qual símbolo exibir para uma tarefa concluída ou pendente.

Sem decisões, um programa só poderia executar uma sequência fixa de instruções, sempre a mesma, independentemente da situação — o que tornaria impossível construir algo minimamente interativo, como uma aplicação CLI que responde de forma diferente a comandos diferentes digitados pelo usuário (`adicionar`, `listar`, `concluir`, `sair`).

O núcleo da interação de uma CLI simples costuma ser resolvido com uma cadeia de decisões que examina o comando digitado e decide qual ação executar:

```java
public class Principal {
    public static void main(String[] args) {
        GerenciadorDeTarefas gerenciador = new GerenciadorDeTarefas();
        Scanner leitor = new Scanner(System.in);
        boolean rodando = true;

        while (rodando) {
            String comando = leitor.nextLine();

            if (comando.equals("adicionar")) {
                System.out.println("Descrição da tarefa:");
                gerenciador.adicionar(leitor.nextLine());
            } else if (comando.equals("listar")) {
                gerenciador.listar();
            } else if (comando.equals("sair")) {
                rodando = false;
            } else {
                System.out.println("Comando desconhecido.");
            }
        }
    }
}
```

Cada ramo do `if`/`else if` decide uma ação diferente com base no texto digitado, e o ramo final `else` cobre o caso de um comando não reconhecido — evitando que o programa simplesmente trave ou se comporte de forma imprevisível diante de uma entrada inesperada.

Fora da programação, isso se parece com o atendimento de uma recepção que escuta o pedido de um visitante e o direciona: "se é para retirar um documento, vá à sala 1; se é para agendar uma reunião, vá à sala 2; se não reconheço o pedido, explico que não entendi" — cada decisão direciona o fluxo para a ação certa.

Um exemplo já visto no conceito de métodos usa o operador ternário para uma decisão mais curta: `tarefas[i].isConcluida() ? "[x]" : "[ ]"`, escolhendo entre dois textos curtos com base em uma condição booleana, uma alternativa mais compacta ao `if`/`else` para casos simples como esse. Este projeto reaproveita diretamente as estruturas de decisão construídas em módulos anteriores, agora coordenando o fluxo inteiro de uma aplicação interativa, não apenas trechos isolados de exemplo.

### repetições

Estruturas de repetição (`for`, `while`, vistas em módulos anteriores) sustentam duas partes essenciais do projeto CLI: o laço principal que mantém o programa rodando, aceitando comandos repetidamente até o usuário pedir para sair, e os laços internos que percorrem a coleção de tarefas guardada no array.

Sem repetição, o programa só conseguiria processar um único comando e encerrar imediatamente — cada `listar`, `adicionar` ou `concluir` exigiria reiniciar o programa do zero, o que tornaria uma aplicação CLI interativa impraticável. Da mesma forma, sem um laço para percorrer o array de tarefas, seria necessário escrever manualmente uma linha de código para cada posição possível do array, uma abordagem que não escalaria além de poucos itens.

O projeto resolve o primeiro problema com um `while (rodando)` (mostrado no conceito de decisões), que mantém o programa lendo e processando comandos indefinidamente, até que a variável `rodando` seja alterada para `false` pelo próprio comando `sair`. Resolve o segundo problema com laços `for` controlados pela variável `quantidade`, como já mostrado nos métodos `listar` e `contarPendentes`.

```java
public void concluir(int indice) {
    if (indice < 0 || indice >= quantidade) {
        System.out.println("Índice inválido.");
        return;
    }
    tarefas[indice].concluir();
}

public void listarPendentes() {
    for (int i = 0; i < quantidade; i++) {
        if (!tarefas[i].isConcluida()) {
            System.out.println((i + 1) + ". " + tarefas[i].getDescricao());
        }
    }
}
```

O método `listarPendentes` combina repetição (`for`, percorrendo todas as tarefas) com decisão (`if`, filtrando apenas as pendentes) — um padrão extremamente comum, que já apareceu em módulos anteriores e volta a se repetir neste projeto: repetir uma ação sobre uma coleção, decidindo a cada passo se ela se aplica àquele item específico.

Uma analogia: o laço principal do programa (`while (rodando)`) é como o expediente de uma loja que permanece aberta, atendendo um cliente após o outro, até a hora de fechar as portas; os laços internos que percorrem o array de tarefas são como um funcionário conferindo item por item uma lista de estoque, um de cada vez, do início ao fim.

Este projeto reaproveita diretamente o controle de laços, condições de parada e a combinação entre repetição e decisão já construídos ao longo do livro, agora aplicados de forma coordenada dentro de um programa completo, em vez de exercícios isolados de um só laço.

### encapsulamento

Encapsulamento, tema já apresentado no Módulo 9 (através de `private` e métodos de acesso controlado) e retomado neste módulo com os modificadores de acesso, se manifesta no projeto CLI através da forma como as classes `Tarefa` e `GerenciadorDeTarefas` escondem seus dados internos, expondo apenas métodos controlados para manipulá-los.

Na ausência de encapsulamento, qualquer parte do programa poderia acessar e alterar diretamente o array `tarefas` ou o campo `concluida` de uma `Tarefa`, ignorando completamente as regras que essas classes deveriam impor — por exemplo, alguém poderia inserir um elemento diretamente em uma posição do array sem atualizar a variável `quantidade`, deixando a estrutura interna em um estado inconsistente, ou marcar uma tarefa como concluída sem passar pelo método pensado para isso.

O projeto resolve isso mantendo os campos internos de cada classe como `private` (o array `tarefas`, a variável `quantidade`, o campo `concluida` de `Tarefa`) e expondo apenas métodos `public` cuidadosamente pensados para cada operação permitida — `adicionar`, `concluir`, `listar`, `contarPendentes`. Toda a interação externa com essas classes passa, obrigatoriamente, por esses métodos.

```java
public class Tarefa {
    private String descricao; // não acessível diretamente de fora
    private boolean concluida; // idem

    public void concluir() {
        concluida = true; // única forma controlada de mudar esse estado
    }

    public boolean isConcluida() {
        return concluida; // leitura controlada, sem permitir escrita direta
    }
}
```

Repare que não existe um método `setConcluida(boolean valor)`: a classe `Tarefa` escolhe deliberadamente oferecer apenas `concluir()`, que só permite marcar uma tarefa como concluída, nunca reverter esse estado — uma decisão de design que o encapsulamento torna possível impor, algo que seria impossível de garantir se o campo `concluida` fosse `public` e qualquer código pudesse alterá-lo livremente para `true` ou `false`.

Um jeito de visualizar isso: encapsulamento na classe `Tarefa` funciona como um carimbo de "concluído" em um formulário físico — uma vez carimbado por um processo oficial (o método `concluir`), não existe uma borracha disponível para qualquer pessoa apagar o carimbo livremente; só o processo formal e controlado pode alterar aquele estado.

Este projeto consolida, na prática, tudo que os Módulos 9 e 10 ensinaram sobre encapsulamento: cada classe decide exatamente o que expõe e o que protege, e essa decisão molda diretamente como o restante do programa pode (ou não pode) interagir com os dados internos, prevenindo estados inconsistentes que seriam possíveis se tudo estivesse aberto.

### exceções básicas

O projeto CLI aplica um primeiro uso prático de `try`/`catch` (vistos neste mesmo módulo) para lidar com um problema recorrente em qualquer aplicação que lê entrada de texto do usuário: a entrada pode não estar no formato esperado, e o programa precisa reagir a isso sem travar.

Sem tratamento de exceções, um comando como `concluir` (que provavelmente pede ao usuário para digitar o número de uma tarefa) quebraria o programa inteiro assim que o usuário digitasse algo que não é um número válido — por exemplo, um texto qualquer em vez de um dígito — porque `Integer.parseInt` lançaria uma `NumberFormatException` não tratada, encerrando a execução com um stack trace bruto, uma experiência ruim para quem está apenas usando o programa.

O projeto resolve isso envolvendo a conversão de texto para número em um bloco `try`/`catch`, reagindo à falha com uma mensagem compreensível em vez de deixar o programa quebrar:

```java
} else if (comando.equals("concluir")) {
    System.out.println("Número da tarefa:");
    try {
        int numero = Integer.parseInt(leitor.nextLine());
        gerenciador.concluir(numero - 1);
    } catch (NumberFormatException e) {
        System.out.println("Por favor, digite um número válido.");
    }
}
```

Esse trecho se encaixa exatamente na cadeia de decisões já mostrada no conceito de decisões, adicionando tratamento de exceção apenas onde existe risco real de falha (a conversão do texto digitado em número) — o restante do fluxo do programa não precisa desse cuidado, reforçando a prática, já discutida no conceito de `try`, de manter blocos de tratamento restritos ao trecho realmente arriscado.

Comparando com algo do cotidiano: esse tratamento de exceção funciona como um atendente que pede a alguém para repetir um pedido mal compreendido, em vez de simplesmente encerrar o atendimento porque não entendeu a primeira tentativa — o programa dá uma segunda chance com uma mensagem clara, em vez de travar por completo diante do primeiro erro de digitação.

Este projeto encerra o Nível 1 — Introdutório demonstrando, de forma concreta e integrada, como classes, métodos, arrays, decisões, repetições, encapsulamento e exceções — cada um estudado isoladamente ao longo dos dez módulos deste livro — se combinam na prática para formar uma aplicação funcional, ainda que simples. É exatamente essa integração, mais do que qualquer conceito individual, que prepara o terreno para os livros dos níveis seguintes, nos quais essas mesmas bases sustentarão temas mais avançados.
