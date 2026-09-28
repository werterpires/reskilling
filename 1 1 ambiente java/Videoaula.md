# Roteiro de videoaula — Tópico 1: Preparar o ambiente Java

## Informações gerais

- **Tema:** preparação do ambiente Java 21.
- **Público:** profissionais em transição de COBOL para Java.
- **Exemplo central:** aplicação de gestão bancária.
- **Duração sugerida:** 45 a 60 minutos.
- **Formato:** exposição breve + demonstração na tela.
- **Objetivo:** terminar a videoaula com Java 21 e VS Code configurados e com `AplicacaoBanco` compilada e executada.
- **Fora do escopo:** lógica de programação, regras bancárias, decisões, repetições, métodos em profundidade e modelagem completa de orientação a objetos.

---

## 1. Abertura — você não está começando do zero

### Pontos para comentar

- Mudança de stack não apaga a experiência acumulada.
- Conhecimentos que continuam válidos:
  - lógica;
  - regras de negócio;
  - sistemas corporativos;
  - compilação;
  - análise de erros;
  - cuidado com dados críticos;
  - rastreabilidade.
- Novidades principais:
  - vocabulário;
  - ferramentas;
  - estrutura dos arquivos;
  - organização orientada a objetos.
- Meta concreta da videoaula:

```text
Escrever → compilar → executar → ver o resultado
```

### Mensagem de transição

- “Hoje não vamos aprender Java inteiro.”
- “Vamos apenas preparar o terreno e fazer o primeiro programa funcionar.”
- “O domínio será familiar: uma aplicação bancária.”

---

## 2. Apresentar o resultado final

### Mostrar rapidamente

- Pasta `gestao-banco` aberta no VS Code.
- Arquivo `AplicacaoBanco.java`.
- Terminal exibindo:

```text
Sistema Bancário iniciado.
Ambiente Java configurado com sucesso.
```

### Pontos para comentar

- Resultado simples e proposital.
- Nenhuma regra bancária neste momento.
- Objetivo: comprovar que todas as ferramentas estão conectadas.
- Entidades futuras do domínio:
  - `Cliente`;
  - `Conta`;
  - `Transacao`;
  - `Banco`.

---

## 3. O que é o JDK

### Definição simples

- JDK: *Java Development Kit*.
- Tradução: Kit de Desenvolvimento Java.
- Comparação visual: “caixa de ferramentas do desenvolvedor Java”.

### Ferramentas que serão usadas

| Ferramenta | Função |
|---|---|
| `javac` | Compilar o código-fonte |
| `java` | Iniciar a execução do programa |

### Ponte com COBOL

- COBOL também possui:
  - arquivo-fonte;
  - compilação;
  - artefato gerado;
  - ambiente de execução.
- Ideia conhecida; nomes novos.

### Fluxo para mostrar

```text
AplicacaoBanco.java
        ↓ javac
AplicacaoBanco.class
        ↓ java / JVM
Programa em execução
```

---

## 4. JDK, JVM e JRE

### Explicação curta

- **JDK:** ferramentas completas para desenvolver.
- **JVM:** máquina virtual que executa bytecode.
- **JRE:** ambiente necessário para executar aplicações Java.

### Ideia que deve permanecer

> Para desenvolver durante a trilha, será instalado o JDK.

### Bytecode

- Arquivo Java: `.java`.
- Arquivo compilado: `.class`.
- `.class` não é o código que será editado.
- `.class` contém bytecode para a JVM.
- Não aprofundar detalhes internos da JVM neste momento.

### Fluxo visual

```text
Código Java → bytecode → JVM → sistema operacional
```

---

## 5. Instalar o Java 21

### Configuração adotada

- Java 21.
- Versão LTS.
- Eclipse Temurin.
- JDK.
- Windows 11 Enterprise.
- Arquitetura x64.
- Instalador MSI.

### Aviso importante

- Instalar no computador físico.
- Não instalar dentro da VDI.
- Em caso de permissão, bloqueio ou tela diferente: procurar o instrutor.

### Demonstração na tela

1. Acessar o Eclipse Adoptium.
2. Selecionar **JDK 21 — LTS**.
3. Selecionar Windows x64.
4. Baixar o MSI.
5. Executar o instalador.
6. Aceitar a licença.
7. Manter o diretório de instalação.
8. Concluir.

### Mostrar no Explorador de Arquivos

```text
C:\Program Files\Eclipse Adoptium\jdk-21.0.x.x-hotspot
```

### Destacar a pasta `bin`

```text
C:\Program Files\Eclipse Adoptium\jdk-21.0.x.x-hotspot\bin
```

### Arquivos a mostrar

- `java.exe`;
- `javac.exe`.

### Observação

- Os números depois de `21` podem variar conforme a atualização.
- Confirmar o caminho real da máquina.
- Não copiar automaticamente o caminho exibido no vídeo.

---

## 6. Primeiro contato com `java` e `javac`

### Demonstração

- Abrir o Prompt de Comando.
- Executar:

```bat
java -version
```

### Explicar

- `java`: ferramenta de execução.
- `-version`: pedido para mostrar a versão.
- Resultado esperado: versão principal `21`.

### Executar em seguida

```bat
javac -version
```

### Explicar

- `javac`: compilador Java.
- Resultado esperado: versão principal `21`.
- Os comandos verificam ferramentas diferentes.

### Se o comando não for reconhecido

- Não reinstalar imediatamente.
- Provável causa: Windows ainda não localizou a pasta do executável.
- Introduzir `JAVA_HOME` e `PATH` como solução.

---

## 7. Configurar `JAVA_HOME`

### Conceito

- Variável de ambiente.
- Nome conhecido por ferramentas Java.
- Indica a pasta principal do JDK.

### Valor correto

```text
JAVA_HOME=C:\Program Files\Eclipse Adoptium\jdk-21.0.x.x-hotspot
```

### Valor incorreto

```text
JAVA_HOME=C:\Program Files\Eclipse Adoptium\jdk-21.0.x.x-hotspot\bin
```

### Destacar

- `JAVA_HOME` não termina em `\bin`.
- A variável aponta para o JDK completo.

### Demonstração no Windows

1. Pesquisar por “variáveis de ambiente”.
2. Abrir **Editar as variáveis de ambiente do sistema**.
3. Abrir **Variáveis de Ambiente**.
4. Em variáveis do usuário, clicar em **Novo**.
5. Nome: `JAVA_HOME`.
6. Valor: caminho principal do JDK.
7. Confirmar.

### Validar em um novo Prompt de Comando

```bat
echo %JAVA_HOME%
```

### Ponte com COBOL

```cobol
DISPLAY "AMBIENTE CONFIGURADO"
```

```bat
echo Ambiente configurado
```

### Observação durante a explicação

- `echo` é comando do terminal, não Java.
- Comparação apenas pela finalidade: exibir informação.
- `%JAVA_HOME%`: solicitar ao Windows o valor da variável.

---

## 8. Configurar o `PATH`

### Conceito

- Lista de diretórios pesquisados pelo Windows.
- Ao digitar `java`, o Windows procura `java.exe` nesses diretórios.

### Entrada a adicionar

```text
%JAVA_HOME%\bin
```

### Relação entre as configurações

| Configuração | Responsabilidade |
|---|---|
| `JAVA_HOME` | Informar onde está o JDK |
| `%JAVA_HOME%\bin` no `PATH` | Informar onde estão os comandos |

### Demonstração

1. Selecionar `Path` nas variáveis do usuário.
2. Clicar em **Editar**.
3. Clicar em **Novo**.
4. Adicionar `%JAVA_HOME%\bin`.
5. Confirmar todas as janelas.
6. Fechar o Prompt de Comando antigo.
7. Abrir um novo Prompt de Comando.

### Repetir os testes

```bat
java -version
javac -version
```

---

## 9. Validar completamente o ambiente

### Executar na ordem

```bat
echo %JAVA_HOME%
java -version
javac -version
where java
where javac
```

### Explicar cada resultado

- `echo %JAVA_HOME%`: pasta principal configurada.
- `java -version`: versão usada na execução.
- `javac -version`: versão usada na compilação.
- `where java`: caminho do `java.exe` encontrado.
- `where javac`: caminho do `javac.exe` encontrado.

### Alerta

- Versão diferente de 21: possível instalação antiga antes no `PATH`.
- Mais de um caminho: o primeiro tende a ser utilizado.
- Não remover entradas aleatoriamente.
- Procurar o instrutor em caso de conflito.

---

## 10. Configurar o VS Code

### Ideias principais

- VS Code: editor extensível.
- Suporte Java fornecido por extensões.
- JDK já instalado separadamente.

### Demonstração

1. Abrir o VS Code.
2. Abrir Extensions com `Ctrl+Shift+X`.
3. Pesquisar **Extension Pack for Java**.
4. Confirmar fornecedor Microsoft.
5. Instalar.

### Recursos do pacote

- suporte à linguagem;
- execução e depuração;
- testes;
- Maven;
- gerenciamento de projetos.

### Confirmar o JDK

- Abrir a Paleta de Comandos: `Ctrl+Shift+P`.
- Pesquisar:

```text
Java: Configure Java Runtime
```

- Confirmar JDK 21.
- Reiniciar o VS Code se necessário.

### Mencionar alternativas

- IntelliJ IDEA:
  - IDE completa;
  - navegação e refatoração avançadas.
- Eclipse IDE:
  - tradicional no ecossistema Java;
  - presença histórica em sistemas corporativos.
- VS Code:
  - mais leve;
  - suporte adicionado por extensões.
- Fundamentos Java independem da IDE.

---

## 11. Criar a aplicação bancária

### Criar a pasta

```text
gestao-banco
```

### Demonstração no VS Code

- **File > Open Folder**.
- Abrir `gestao-banco`.
- Destacar: abrir a pasta inteira, não apenas o arquivo `.java`.

### Estrutura a criar

```text
gestao-banco/
└── src/
    └── br/
        └── com/
            └── curso/
                └── banco/
                    └── AplicacaoBanco.java
```

### Pontos para comentar

- Primeiro contato com pacotes.
- Pacote: `br.com.curso.banco`.
- Estrutura de pastas acompanha o pacote.
- Não aprofundar pacotes neste momento.
- Projeto preparado para crescer com outras classes.

---

## 12. Escrever `AplicacaoBanco.java`

### Código

```java
package br.com.curso.banco;

public class AplicacaoBanco {

    public static void main(String[] args) {
        System.out.println("Sistema Bancário iniciado.");
        System.out.println("Ambiente Java configurado com sucesso.");
    }
}
```

### Explicar somente o necessário

- `package br.com.curso.banco`:
  - pacote da classe;
  - correspondente às pastas.
- `public class AplicacaoBanco`:
  - declaração da classe;
  - nome igual ao arquivo.
- `main`:
  - ponto de entrada;
  - local pelo qual a execução começa.
- `System.out.println`:
  - exibir uma linha no terminal.

### Evitar neste momento

- explicar detalhadamente `public`;
- explicar detalhadamente `static`;
- explicar detalhadamente `void`;
- decompor internamente `System.out`;
- transformar o exemplo em conteúdo de lógica.

---

## 13. Comparar COBOL e Java

### COBOL

```cobol
IDENTIFICATION DIVISION.
PROGRAM-ID. APLICACAO-BANCO.

PROCEDURE DIVISION.
    DISPLAY "SISTEMA BANCARIO INICIADO".
    DISPLAY "AMBIENTE JAVA CONFIGURADO COM SUCESSO".
    STOP RUN.
```

### Java

```java
package br.com.curso.banco;

public class AplicacaoBanco {

    public static void main(String[] args) {
        System.out.println("Sistema Bancário iniciado.");
        System.out.println("Ambiente Java configurado com sucesso.");
    }
}
```

### Pontos para comentar

- Mesmo objetivo: iniciar, exibir mensagens e terminar.
- `DISPLAY` e `System.out.println`:
  - mesma finalidade neste exemplo;
  - sintaxes diferentes.
- `PROGRAM-ID` e nome da classe:
  - ambos ajudam a identificar o programa;
  - não são equivalentes perfeitos.
- `PROCEDURE DIVISION` e `main`:
  - ambos participam da entrada no fluxo de execução;
  - estruturas das linguagens são diferentes.
- Comparação conceitual, não tradução linha por linha.

---

## 14. Compilar com `javac`

### Abrir o terminal integrado

- Menu **Terminal > New Terminal**.
- Confirmar diretório `gestao-banco`.

### Criar pasta de saída

```bat
mkdir out
```

### Compilar

```bat
javac -d out src\br\com\curso\banco\AplicacaoBanco.java
```

### Explicar o comando

| Parte | Explicação |
|---|---|
| `javac` | Compilador Java |
| `-d out` | Destino do arquivo compilado |
| caminho `.java` | Arquivo-fonte a ser compilado |

### Mostrar o resultado

```text
out/
└── br/
    └── com/
        └── curso/
            └── banco/
                └── AplicacaoBanco.class
```

### Ponte com COBOL

- Fonte e artefato compilado são coisas diferentes.
- Alteração acontece no `.java`.
- Nova alteração exige nova compilação manual.

---

## 15. Executar com `java`

### Comando

```bat
java -cp out br.com.curso.banco.AplicacaoBanco
```

### Resultado esperado

```text
Sistema Bancário iniciado.
Ambiente Java configurado com sucesso.
```

### Explicar o comando

| Parte | Explicação |
|---|---|
| `java` | Inicia a JVM |
| `-cp out` | Informa onde estão as classes |
| `br.com.curso.banco.AplicacaoBanco` | Nome completo da classe |

### Destacar

- Não escrever `.class` no comando.
- Usar pontos no nome completo da classe.
- Compilação e execução são etapas distintas.

### Retomar o fluxo

```text
AplicacaoBanco.java
        ↓ javac
AplicacaoBanco.class
        ↓ java
Programa executado
```

---

## 16. Executar pelo VS Code

### Demonstração

- Localizar **Run** próximo ao `main`.
- Executar.
- Mostrar a saída no terminal.

### Pontos para comentar

- VS Code automatiza compilação e execução.
- Mesma linguagem e mesmo JDK.
- Botão não substitui a compreensão do processo.
- Terminal ajuda no diagnóstico de problemas futuros.

---

## 17. Introduzir a mudança procedural → orientação a objetos

### Manter breve

- Aplicação já começa dentro da classe `AplicacaoBanco`.
- O sistema crescerá com classes do domínio:
  - `Cliente`;
  - `Conta`;
  - `Transacao`;
  - `Banco`.
- Evitar uma única classe com todo o sistema.
- Distribuição futura de responsabilidades.

### Contraste de perguntas

- Procedural:
  - “Quais passos o programa deve executar?”
- Orientação a objetos:
  - “Quais elementos existem no domínio?”
  - “Qual responsabilidade pertence a cada elemento?”

### Mensagem principal

- Experiência em processos continua útil.
- Nova habilidade: modelar participantes e responsabilidades.
- Mudança gradual ao longo da aplicação bancária.

---

## 18. Demonstração de uma pequena alteração

### Alterar a segunda mensagem

```java
System.out.println("Aplicação pronta para receber novos módulos.");
```

### Executar sem recompilar manualmente

- Mostrar que o `.class` antigo ainda representa a versão anterior, se executado pelo terminal.

### Recompilar

```bat
javac -d out src\br\com\curso\banco\AplicacaoBanco.java
```

### Executar novamente

```bat
java -cp out br.com.curso.banco.AplicacaoBanco
```

### Ponte com COBOL

- Alterar `DISPLAY`.
- Recompilar.
- Executar o novo artefato.
- Mesmo ciclo geral, ferramentas diferentes.

---

## 19. Problemas comuns

### `java` não reconhecido

- Conferir `%JAVA_HOME%\bin` no `PATH`.
- Abrir novo terminal.

### `javac` não reconhecido

- Executar:

```bat
where java
where javac
```

- Possível Java antigo ou apenas ambiente de execução.

### Versão diferente de 21

- Outra instalação aparece primeiro no `PATH`.
- Não remover entradas sem confirmar.

### `%JAVA_HOME%` exibido literalmente

- Variável não encontrada.
- Conferir nome e valor.
- Abrir novo terminal.

### Arquivo-fonte não encontrado

- Confirmar diretório atual.
- Confirmar estrutura:

```text
src\br\com\curso\banco\AplicacaoBanco.java
```

### Nome da classe diferente do arquivo

- Arquivo: `AplicacaoBanco.java`.
- Classe: `AplicacaoBanco`.
- Maiúsculas e minúsculas importam.

### VS Code não reconhece Java

- Validar `java -version`.
- Validar `javac -version`.
- Confirmar Extension Pack for Java.
- Confirmar pasta aberta.
- Confirmar **Java: Configure Java Runtime**.
- Reiniciar VS Code.

---

## 20. Encerramento

### Recapitular

- JDK instalado.
- `JAVA_HOME` configurado.
- `PATH` configurado.
- `java` validado.
- `javac` validado.
- VS Code preparado.
- Aplicação bancária criada.
- Fonte compilado em bytecode.
- Classe executada pela JVM.

### Reforçar

- Não foi um recomeço do zero.
- Conceitos conhecidos foram conectados a novas ferramentas.
- Primeira aplicação Java funcionando.
- Próximos passos: fazer a aplicação bancária crescer com classes e objetos.

### Checklist final para mostrar na tela

- [ ] Java 21 instalado no computador físico.
- [ ] `JAVA_HOME` configurado.
- [ ] `%JAVA_HOME%\bin` presente no `PATH`.
- [ ] `java -version` retorna 21.
- [ ] `javac -version` retorna 21.
- [ ] VS Code reconhece o JDK.
- [ ] `AplicacaoBanco.java` compilada.
- [ ] `AplicacaoBanco` executada.

---

## Referências para deixar na descrição da videoaula

- [Java no Visual Studio Code](https://code.visualstudio.com/docs/languages/java)
- [Primeiros passos com Java no VS Code](https://code.visualstudio.com/docs/java/java-tutorial)
- [Instalação do Eclipse Temurin no Windows](https://adoptium.net/installation/windows/)
- [Comando `java` no JDK 21](https://docs.oracle.com/en/java/javase/21/docs/specs/man/java.html)
- [Comando `javac` no JDK 21](https://docs.oracle.com/en/java/javase/21/docs/specs/man/javac.html)
