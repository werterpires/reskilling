# Tópico 1 — Fundamentos do Git e versionamento local

## Do código que funciona ao histórico que você controla

Nos tópicos de Java, você preparou o ambiente, criou classes, deu comportamento aos objetos e fez esses objetos trabalharem juntos. Na aplicação de **Gestão de Biblioteca**, `Livro`, `Usuario`, `Emprestimo` e `Biblioteca` passaram a dividir responsabilidades.

Agora surge uma pergunta que todo projeto de software acaba encontrando: **o que acontece com esse código conforme ele muda?**

Você já conhece o problema por outro ângulo: em ambientes corporativos, uma alteração em um fonte precisa ser identificada, justificada e, se necessário, desfeita. A pergunta não é nova. O que muda é a ferramenta: no desenvolvimento Java moderno, a mais usada é o **Git**.

Neste tópico, você o instalará no Windows e trabalhará em um repositório local, sem GitHub e sem internet. Ao final, a pasta `gestao-biblioteca` terá um histórico organizado.

## O que você fará neste tópico

- entenderá por que versionar código e o que diferencia Git de GitHub;
- instalará o Git no Windows, compreendendo as telas relevantes do instalador;
- usará o Git Bash e configurará sua identidade;
- criará um repositório e entenderá as três áreas do Git;
- usará `git status`, `git add`, `git commit`, `git log` e `git diff`;
- impedirá que arquivos gerados entrem no histórico com `.gitignore`.

> Não vamos estudar GitHub, remotos, branches nem desfazer alterações neste tópico. O foco é construir e ler um histórico local.

---

## 1. Por que versionar código?

Imagine que o empréstimo da biblioteca funciona. Alguém altera `Livro` para incluir a renovação, e a devolução deixa de funcionar. Surgem perguntas simples:

- o que mudou desde a última versão que funcionava?
- quem mudou, quando e por quê?
- como voltar a olhar o estado anterior?
- e se duas pessoas alterarem o mesmo projeto ao mesmo tempo?

Sem uma ferramenta, as equipes improvisam: copiam pastas, criam nomes como `biblioteca_final`, `biblioteca_final_v2` e `biblioteca_final_corrigida`, fazem backups periódicos e enviam arquivos por e-mail ou mensageiro.

Os problemas são previsíveis: não se sabe qual cópia é a correta, é fácil sobrescrever o trabalho de outra pessoa e a autoria se perde. Um backup responde "como estava em tal dia". Um controle de versão responde "o que mudou, por quê e quem mudou".

### Relação com o mundo mainframe

Você provavelmente já trabalhou com bibliotecas de código, ferramentas de gerência de configuração, promoção entre ambientes e registros de quem alterou o quê. Esse mundo tem ferramentas próprias e processos maduros: o mainframe **não** é um ambiente sem controle de versão.

O Git não pretende substituir o que a sua empresa usa hoje. O que muda no desenvolvimento Java moderno é o modelo de trabalho:

| Característica | Como costuma ser em ambientes tradicionais | Como costuma ser com Git |
|---|---|---|
| Onde fica o histórico | em um repositório central, administrado pela empresa | em **cada cópia** do projeto, inclusive na sua máquina |
| Ferramenta | específica do ecossistema e da instalação | a mesma ferramenta, em qualquer sistema operacional |
| Trabalho em paralelo | frequentemente coordenado por bloqueios ou processos de promoção | cada pessoa trabalha de forma independente e integra depois |
| Operações do dia a dia | dependem da conexão com o servidor | a maioria funciona sem rede |

A tabela descreve tendências, não regras.

---

## 2. Git, GitHub e o que significa "distribuído"

O **Git** é um sistema distribuído de controle de versão. Ele registra as alterações feitas nos arquivos de um projeto, permite consultar o que aconteceu e permite recuperar estados anteriores.

"Distribuído" quer dizer que cada repositório contém o **histórico completo**. Você não precisa de um servidor central para registrar uma versão, consultar o passado ou comparar duas alterações. Por isso, quase tudo que faremos neste tópico funciona com o computador desconectado.

| Nome | O que é |
|---|---|
| **Git** | a ferramenta de controle de versão, instalada na sua máquina |
| **GitHub** | uma plataforma na internet que hospeda repositórios Git e oferece recursos de colaboração |

O Git funciona sem o GitHub. Neste tópico, trabalharemos somente com o Git local. O Git também não é exclusivo de Java: versiona qualquer arquivo de texto.

---

## 3. Instalar o Git no Windows

### Prepare o ambiente

Antes de instalar, use o **computador físico**, não a VDI, e feche terminais abertos, que não enxergam programas instalados depois de iniciados. Se a política de segurança bloquear a instalação, fale com o suporte de TI ou com o instrutor, sem tentar contornar a restrição.

No site oficial, `https://git-scm.com/`, baixe o instalador para Windows (normalmente 64 bits) e execute-o.

### As telas do instalador

O instalador faz várias perguntas, e algumas escolhas afetam o seu dia a dia. Não avance clicando em *Next* sem ler.

| Tela | Escolha recomendada |
|---|---|
| Select Components | manter as opções padrão |
| Default editor | Visual Studio Code, se estiver instalado |
| Initial branch name | **Override** com o nome `main` |
| PATH environment | *Git from the command line and also from 3rd-party software* |
| SSH executable | usar o OpenSSH incluído (*bundled OpenSSH*) |
| HTTPS transport backend | OpenSSL |
| Line endings | *Checkout Windows-style, commit Unix-style* |
| Terminal emulator | MinTTY |
| Default behavior of `git pull` | manter o padrão |
| Credential helper | Git Credential Manager |
| Additional options | manter as sugestões |

Se a sua empresa usa certificados próprios, a TI pode orientar outra escolha para o transporte HTTPS.

### Por que essas escolhas?

**PATH.** É a lista de pastas onde o sistema procura programas quando você digita um comando. Você já viu isso com `java` e `javac`. A opção recomendada deixa o comando `git` disponível em qualquer terminal e também para outras ferramentas, como o VS Code e as IDEs, que chamam o Git por você.

**Editor padrão.** Às vezes o Git abre um editor, por exemplo quando você faz um commit sem informar a mensagem na linha de comando. Sem escolha, ele pode abrir um editor de terminal desconhecido, no qual é fácil ficar preso. Com o VS Code, você usa um ambiente que já conhece. Se perder essa tela, configure depois:

```bash
git config --global core.editor "code --wait"
```

**Nome da branch inicial.** A linha principal de um repositório precisa de um nome. Historicamente o Git usava `master`; hoje o padrão da comunidade e das plataformas é `main`. Com `main`, o nome local coincide com o que veremos no GitHub. Branches ficam para o próximo tópico.

**Final de linha.** O Windows marca o fim de uma linha de texto com dois caracteres (CRLF); Linux e macOS usam um só (LF). Se cada pessoa gravar de um jeito, o Git enxerga linhas inteiras como alteradas só porque o formato mudou. A opção *Checkout Windows-style, commit Unix-style* converte para CRLF ao trazer arquivos para a sua pasta e para LF ao registrá-los. O histórico fica uniforme, e o Windows continua recebendo os finais de linha habituais. Em alguns momentos, o Git exibirá um aviso do tipo *LF will be replaced by CRLF*; ele apenas informa essa conversão.

**Credential Manager.** O *Git Credential Manager* guarda credenciais com segurança e só importará quando você se conectar a repositórios remotos, no próximo tópico. Como o GitHub não aceita mais a senha da conta em operações Git por HTTPS, o Credential Manager abre o navegador para você entrar na conta e guarda o resultado. Por ora, mantenha a opção marcada.

As demais telas aceitam o padrão. *MinTTY* é o terminal do Git Bash, e o comportamento de `git pull` só importará com repositórios remotos.

### Confirme a instalação

Abra o **Git Bash** pelo menu Iniciar e execute:

```bash
git --version
```

O resultado mostra a versão instalada, por exemplo:

```text
git version 2.53.0
```

Se aparecer `command not found`, feche e abra o terminal, confirme que a instalação terminou, verifique a opção de PATH e repita o comando. Se persistir, chame o instrutor com a mensagem completa do terminal.

---

## 4. Git Bash, PowerShell e Prompt de Comando

O Git funciona em qualquer terminal do Windows, mas o curso usará o **Git Bash**, para que os comandos sejam iguais para todos e iguais aos de materiais de Linux e macOS.

| Terminal | Característica |
|---|---|
| Git Bash | ambiente estilo Linux instalado com o Git; usa `/` nos caminhos |
| PowerShell | terminal moderno do Windows; usa `\` nos caminhos |
| Prompt de Comando | terminal tradicional do Windows (`cmd`) |

A diferença mais visível está nos caminhos: no Git Bash, `C:\Users\Ana\Documents` vira `/c/Users/Ana/Documents`.

Alguns comandos de navegação:

```bash
pwd                  # mostra a pasta atual
ls                   # lista arquivos
ls -a                # lista também arquivos ocultos
cd projetos          # entra na pasta projetos
cd ..                # sobe um nível
mkdir gestao-biblioteca   # cria uma pasta
```

Use sempre barras normais (`/`) e coloque entre aspas nomes com espaços. Para seus projetos, escolha uma pasta como `~/projetos`; evite pastas sincronizadas automaticamente, como as do OneDrive, pois o sincronizador e o Git podem mexer nos mesmos arquivos.

---

## 5. Dizer ao Git quem é você

Cada commit registra seu autor. Antes do primeiro, o Git precisa saber seu nome e seu e-mail:

```bash
git config --global user.name "Ana Martins"
git config --global user.email "ana.martins@exemplo.com"
```

Use o seu nome e o seu e-mail. Leia o comando por partes:

| Trecho | Significado |
|---|---|
| `git config` | consulta ou altera configurações |
| `--global` | vale para todos os repositórios do seu usuário no computador |
| `user.name` | nome associado aos commits |
| `user.email` | e-mail associado aos commits |

Sem essa configuração, o primeiro commit é recusado com a mensagem `Author identity unknown`. A configuração global é um padrão; dentro de um repositório, `git config` sem `--global` define outra identidade. O e-mail poderá mais tarde ser associado à sua conta do GitHub.

> **Cuidado:** identidade de commit **não é autenticação**. Informar um nome e um e-mail não prova quem você é nem dá acesso a um serviço. Ela apenas rotula o que você registra.

Para conferir:

```bash
git config --global --list
```

```text
user.name=Ana Martins
user.email=ana.martins@exemplo.com
init.defaultbranch=main
core.editor=code --wait
```

A lista exibe as chaves em minúsculas, e a sua pode ter outras linhas. Para consultar uma única chave, use `git config --global --get user.name`.

---

## 6. Criar um repositório

Um **repositório** é uma pasta de projeto acompanhada pelo Git, junto com seu histórico. Uma pasta comum só vira repositório quando você pede.

Crie a pasta do projeto e inicialize o controle de versão:

```bash
mkdir -p ~/projetos
cd ~/projetos
mkdir gestao-biblioteca
cd gestao-biblioteca
git init
```

O Git confirma a criação:

```text
Initialized empty Git repository in C:/Users/Ana/projetos/gestao-biblioteca/.git/
```

O caminho depende de onde você criou a pasta. O importante é que foi criada uma pasta oculta chamada `.git`. Para vê-la:

```bash
ls -a
```

```text
.  ..  .git
```

A pasta `.git` guarda a base de dados do repositório. O restante continua sendo uma pasta comum, onde você edita arquivos normalmente; a diferença é que agora o Git a observa.

> **Cuidado:** não apague nem edite manualmente a pasta `.git`. Se ela for apagada, o histórico desaparece, e o projeto volta a ser uma pasta comum.

Para verificar o estado do repositório recém-criado:

```bash
git status
```

```text
On branch main

No commits yet

nothing to commit (create/copy files and use "git add" to track)
```

Se aparecer `On branch master`, o nome inicial não foi configurado. Corrija com `git branch -M main` e, para os próximos repositórios, `git config --global init.defaultBranch main`. No restante do curso, usaremos `main`.

---

## 7. As três áreas do Git

Este é o conceito central do tópico:

```text
Diretório de trabalho  →  Área de preparação  →  Repositório
  (working directory)        (staging area)       (histórico)
        git add →                  git commit →
```

| Área | O que é | Quem a alimenta |
|---|---|---|
| **Diretório de trabalho** | os arquivos da pasta, como você os vê e edita | você, no editor |
| **Área de preparação** (*staging area*) | a seleção do que irá no próximo commit | `git add` |
| **Repositório** | o histórico de commits | `git commit` |

Três consequências importantes:

- **salvar um arquivo não cria uma versão.** O arquivo existe na pasta, mas o histórico não o conhece;
- **`git add` não cria um commit.** Ele só marca o que irá no próximo;
- **`git commit` registra o que está preparado**, e não tudo o que mudou na pasta.

### Ponte com COBOL

Uma analogia de intenção: separar o conjunto de fontes de uma mudança antes de submetê-lo a um processo de promoção. A área de preparação do Git, porém, é local, sem aprovação de terceiros e sem relação com ambientes de execução; serve para você montar o próximo registro.

---

## 8. Observar o que mudou: `git status`

O `git status` é o comando que você mais usará. Ele informa o que o Git sabe sobre cada arquivo e sugere os próximos comandos. Faça dele um hábito antes de qualquer `add` ou `commit`.

Crie, dentro de `gestao-biblioteca`, o arquivo `README.md`:

```markdown
# Gestão de Biblioteca

Projeto desenvolvido durante o curso de Java e Git.

## Funcionalidades

- Cadastro de livros
- Cadastro de usuários
```

E a classe `Livro`, em `src/br/com/curso/biblioteca/dominio/Livro.java`:

```java
package br.com.curso.biblioteca.dominio;

public class Livro {

    private String codigo;
    private String titulo;
    private boolean disponivel;

    public void cadastrar(String codigo, String titulo) {
        this.codigo = codigo;
        this.titulo = titulo;
        this.disponivel = true;
    }

    public String obterTitulo() {
        return titulo;
    }
}
```

Os arquivos existem, mas ainda não fazem parte do histórico:

```bash
git status
```

```text
On branch main

No commits yet

Untracked files:
  (use "git add <file>..." to include in what will be committed)
	README.md
	src/

nothing added to commit but untracked files present (use "git add" to track)
```

*Untracked* significa **não rastreado**: o Git vê os arquivos, mas não os acompanha. Para uma pasta nova, ele mostra apenas `src/`.

---

## 9. Preparar arquivos: `git add`

O `git add` coloca arquivos na área de preparação. Você pode indicar um arquivo:

```bash
git add README.md
```

Um novo `git status` mostra `README.md` em *Changes to be committed*, ou seja, **preparado**, enquanto `src/` continua em *Untracked files*. Prepare também a pasta de código:

```bash
git add src
```

Você pode preparar tudo de uma vez com:

```bash
git add .
```

O ponto significa "a pasta atual e tudo dentro dela". É prático, mas arriscado: inclui **tudo**, inclusive o que você não pretendia versionar, como classes compiladas ou arquivos temporários. Rode `git status` antes e depois, e, por ora, prefira nomear o que será preparado.

As mensagens do Git sugerem comandos como `git rm --cached` e `git restore`. Servem para desfazer operações e ficam fora do escopo deste tópico.

---

## 10. Registrar uma versão: `git commit`

Um **commit** é um registro permanente de um conjunto de alterações preparadas, com autor, data e mensagem. Crie o primeiro:

```bash
git commit -m "Cria README e classe Livro"
```

```text
[main (root-commit) 9de23a5] Cria README e classe Livro
 2 files changed, 26 insertions(+)
 create mode 100644 README.md
 create mode 100644 src/br/com/curso/biblioteca/dominio/Livro.java
```

Na primeira linha, o commit foi feito na branch `main`, é o primeiro (`root-commit`), recebeu o identificador `9de23a5` e tem a mensagem informada. O identificador é um **hash**, que distingue o commit de todos os outros; o seu será diferente.

Confira:

```bash
git status
```

```text
On branch main
nothing to commit, working tree clean
```

*Working tree clean* quer dizer que não há diferença entre a pasta e o último commit.

Sem o `-m`, o Git abre o editor padrão para você digitar a mensagem.

### Boas mensagens de commit

A mensagem é a forma de a pessoa que ler o histórico entender a alteração sem abrir os arquivos. Descreva o que foi feito e separe mudanças independentes em commits diferentes.

| Evite | Prefira |
|---|---|
| `alterações` | `Implementa emprestimo de livro` |
| `teste` | `Adiciona consulta de disponibilidade` |
| `versão final` | `Atualiza README com funcionalidades` |

Nos exemplos, as mensagens não têm acento para evitar problemas de codificação em terminais diferentes. É uma escolha, não uma exigência do Git.

### O commit registra o que foi preparado

Vamos provar a diferença entre as áreas. Acrescente a `Livro`, depois de `obterTitulo`, um método para emprestar:

```java
    public boolean emprestar() {
        if (!disponivel) {
            return false;
        }

        disponivel = false;
        return true;
    }
```

Depois de alterar o arquivo, `git status` o mostra como *modified*: já rastreado e alterado, mas não preparado. Prepare-o:

```bash
git add src/br/com/curso/biblioteca/dominio/Livro.java
```

Agora faça **outra** alteração no mesmo arquivo, sem preparar: acrescente, antes da chave final, o método a seguir.

```java
    public boolean estaDisponivel() {
        return disponivel;
    }
```

```bash
git status
```

```text
On branch main
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
	modified:   src/br/com/curso/biblioteca/dominio/Livro.java

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
	modified:   src/br/com/curso/biblioteca/dominio/Livro.java

```

O mesmo arquivo aparece **duas vezes**: a versão que você preparou (com `emprestar`) e uma alteração posterior (com `estaDisponivel`) que ainda não foi preparada. Faça o commit:

```bash
git commit -m "Implementa emprestimo de livro"
```

```text
[main f184258] Implementa emprestimo de livro
 1 file changed, 9 insertions(+)
```

Só as 9 linhas do método `emprestar` entraram. O método `estaDisponivel` permanece na pasta, e `git status` continua mostrando o arquivo como *modified*.

A conclusão é que o commit registra o **conteúdo preparado**, e não todas as modificações existentes na pasta. Para registrar a segunda alteração, repita o ciclo:

```bash
git add src/br/com/curso/biblioteca/dominio/Livro.java
git commit -m "Adiciona consulta de disponibilidade"
```

---

## 11. Consultar o histórico: `git log`

O `git log` mostra os commits, do mais recente para o mais antigo:

```bash
git log
```

```text
commit ba9ff8994c8b80ef303b72f7e7e221f049d78afb (HEAD -> main)
Author: Ana Martins <ana.martins@exemplo.com>
Date:   Thu Nov 5 18:13:10 2026 -0300

    Adiciona consulta de disponibilidade

commit f184258c527f65f901ac77bd7ab5763a77484b96
Author: Ana Martins <ana.martins@exemplo.com>
Date:   Thu Nov 5 18:13:10 2026 -0300

    Implementa emprestimo de livro

commit 9de23a5d86f3d9b43a703bc5cc0d734114d382b7
Author: Ana Martins <ana.martins@exemplo.com>
Date:   Thu Nov 5 18:13:10 2026 -0300

    Cria README e classe Livro
```

Cada bloco traz o hash completo, o autor, a data e a mensagem. O trecho `(HEAD -> main)` indica o commit em que você está agora, na branch `main`. Os valores de hash, data e autor serão os seus.

Quando o histórico é longo, o Git mostra uma página por vez. Use as setas ou a barra de espaço para navegar e **`q` para sair**.

A versão resumida costuma bastar:

```bash
git log --oneline
```

```text
ba9ff89 (HEAD -> main) Adiciona consulta de disponibilidade
f184258 Implementa emprestimo de livro
9de23a5 Cria README e classe Livro
```

Cada linha é um commit, com seu hash abreviado e a mensagem. Lendo só estas três linhas, você reconstrói a evolução do projeto. Recuperar versões antigas e desfazer commits ficam para depois.

---

## 12. Comparar alterações: `git diff`

Antes de registrar uma alteração, vale revê-la. Acrescente a `Livro`, depois de `obterTitulo`, um método de consulta do código:

```java
    public String obterCodigo() {
        return codigo;
    }
```

O `git diff` mostra o que mudou na pasta e ainda não foi preparado:

```bash
git diff
```

```text
diff --git a/src/br/com/curso/biblioteca/dominio/Livro.java b/src/br/com/curso/biblioteca/dominio/Livro.java
index 2640ca1..9da8f79 100644
--- a/src/br/com/curso/biblioteca/dominio/Livro.java
+++ b/src/br/com/curso/biblioteca/dominio/Livro.java
@@ -16,6 +16,10 @@ public class Livro {
         return titulo;
     }
 
+    public String obterCodigo() {
+        return codigo;
+    }
+
     public boolean emprestar() {
         if (!disponivel) {
             return false;
```

Para ler a saída:

- linhas com `+` foram **adicionadas**, e linhas com `-` foram **removidas**;
- linhas sem sinal são contexto;
- `@@ -16,6 +16,10 @@` indica o trecho: a partir da linha 16, a versão antiga tinha 6 linhas, e a nova tem 10.

O `git diff` simples olha apenas o que **não** foi preparado. Para ver o que **foi** preparado, existe a opção `--staged`:

| Comando | Compara | Responde a |
|---|---|---|
| `git diff` | pasta × área de preparação | "o que mudei e ainda não preparei?" |
| `git diff --staged` | área de preparação × último commit | "o que vai entrar no próximo commit?" |

Depois de `git add`, o primeiro deixa de mostrar o arquivo, e o segundo passa a mostrá-lo:

```bash
git add src/br/com/curso/biblioteca/dominio/Livro.java
git diff
git diff --staged
```

O primeiro `git diff` não imprime nada. O segundo repete a diferença anterior. Use `git diff --staged` antes do commit como **revisão final**: ele mostra exatamente o que será registrado. Registre a alteração:

```bash
git commit -m "Adiciona consulta de codigo"
```

### Ponte com COBOL

Se você já usou utilitários de comparação de fontes no seu ambiente, a intenção é a mesma: mostrar o que difere entre duas versões. O Git compara a pasta, a área de preparação e o último commit.

---

## 13. Ignorar o que não deve ser versionado: `.gitignore`

Nem tudo na pasta do projeto merece entrar no histórico, como arquivos compilados (`.class`), pastas de saída ou build (`out/`, `target/`), logs e configurações locais de ferramentas. Arquivos assim podem ser **regenerados** a partir do código-fonte, e versioná-los polui o histórico com mudanças que não são decisões suas. Configurações locais, além disso, podem não servir a outra pessoa.

Veja o problema acontecer. Compile a classe, como você aprendeu nos tópicos de Java:

```bash
javac -d out src/br/com/curso/biblioteca/dominio/Livro.java
git status
```

```text
On branch main
Untracked files:
  (use "git add <file>..." to include in what will be committed)
	out/

nothing added to commit but untracked files present (use "git add" to track)
```

A pasta `out/` virou candidata a versionar, e um `git add .` distraído a incluiria.

Crie na **raiz** do projeto um arquivo chamado `.gitignore`, com o ponto no início:

```text
# Arquivos compilados
*.class
out/

# Diretório de build do Maven
target/

# Logs
*.log

# Configuração local do VS Code
.vscode/
```

| Regra | Efeito |
|---|---|
| `# ...` | comentário |
| `*.class` | ignora qualquer arquivo terminado em `.class` |
| `out/` | ignora a pasta `out` e seu conteúdo |
| `target/` | ignora a pasta `target`, usada pelo Maven |
| `*.log` | ignora arquivos de log |
| `.vscode/` | ignora configurações locais do VS Code |

Confira com `git status`: `out/` desapareceu da lista, restando apenas o próprio `.gitignore`. O arquivo `.gitignore` **deve ser versionado**, para que quem receber o projeto use as mesmas regras:

```bash
git add .gitignore
git commit -m "Adiciona regras de gitignore"
```

```text
[main 7824212] Adiciona regras de gitignore
 1 file changed, 12 insertions(+)
 create mode 100644 .gitignore
```

Alguns cuidados:

- as regras não afetam arquivos que **já foram** registrados; por isso crie o `.gitignore` cedo, antes do primeiro `git add .`;
- não ignore o que é necessário para construir ou executar o projeto, como o código-fonte;
- as regras crescerão conforme você conhecer outras ferramentas.

### Ponte com COBOL

Existe uma analogia de intenção: no mundo COBOL, o que se preserva e se controla é o **fonte**, enquanto módulos objeto e de carga podem ser gerados novamente a partir dele. No Java, a lógica é parecida: o código `.java` é o que se versiona, e o `.class` é um produto da compilação. O `.gitignore` apenas registra, para o Git, essa distinção.

---

## 14. O fluxo completo

O ciclo a seguir reúne o tópico e será repetido centenas de vezes:

```text
1. alterar arquivos no editor
2. git status         → ver o que mudou
3. git diff           → revisar as alterações
4. git add <arquivo>  → preparar o que pertence ao mesmo assunto
5. git diff --staged  → conferir o que será registrado
6. git commit -m "..." → registrar a versão
7. git log --oneline  → acompanhar o histórico
```

Ao fim deste tópico, `git status` está limpo e `git log --oneline` mostra:

```text
7824212 (HEAD -> main) Adiciona regras de gitignore
a2f32d4 Adiciona consulta de codigo
ba9ff89 Adiciona consulta de disponibilidade
f184258 Implementa emprestimo de livro
9de23a5 Cria README e classe Livro
```

Commits pequenos, com um assunto cada, tornam um histórico útil.

---

## 15. Erros de compreensão que vale evitar

### "Salvei o arquivo, então a versão está guardada"

Salvar grava o arquivo na pasta. Só o `git commit` cria uma versão no histórico.

### "`git add` e `git commit` fazem a mesma coisa"

`git add` seleciona o que irá no próximo registro. `git commit` registra apenas o que foi selecionado: alterações feitas depois do `git add` ficam de fora. Cuidado também com `git add .`, que inclui tudo.

### "Git e GitHub são a mesma coisa"

O Git é a ferramenta local. O GitHub é um serviço que hospeda repositórios Git. Você usou o Git inteiro neste tópico sem o GitHub.

### "Nome e e-mail do `git config` são o meu login"

São apenas o rótulo gravado nos commits. Autenticar-se em um serviço remoto é outro assunto.

### "O `.gitignore` apaga arquivos que já estavam no histórico"

Ele só impede que arquivos **ainda não rastreados** sejam sugeridos. Quem já foi registrado continua sendo acompanhado.

### "Mainframe não tem controle de versão, então Git é uma novidade total"

Há controle de mudanças no seu ambiente atual. O Git traz um modelo diferente, distribuído e muito usado fora do mainframe.

---

## 16. Checklist do Tópico 1

- [ ] Sei explicar por que copiar pastas não substitui um controle de versão.
- [ ] Sei dizer a diferença entre Git e GitHub e o que significa "distribuído".
- [ ] Instalei o Git no computador físico e `git --version` funciona no Git Bash.
- [ ] Sei por que escolhi `main`, o editor padrão, o PATH e o tratamento de finais de linha.
- [ ] Sei navegar com `pwd`, `ls`, `cd` e `mkdir` no Git Bash.
- [ ] Configurei `user.name` e `user.email`, e sei que isso não é autenticação.
- [ ] Sei criar um repositório com `git init` e reconheço a pasta `.git`.
- [ ] Sei explicar diretório de trabalho, área de preparação e repositório.
- [ ] Sei interpretar *untracked*, *modified* e *Changes to be committed* no `git status`.
- [ ] Sei usar `git add`, conheço o risco do `git add .` e sei que o commit leva só o que está preparado.
- [ ] Sei registrar commits com mensagens descritivas.
- [ ] Sei consultar o histórico com `git log` e `git log --oneline`, e sair com `q`.
- [ ] Sei usar `git diff` e `git diff --staged` e explicar a diferença.
- [ ] Sei criar e versionar um `.gitignore`.
- [ ] Meu repositório termina com `git status` limpo.

---

## Conclusão

Este tópico deu um primeiro passo concreto: o código da biblioteca passou a ter memória. Você instalou e configurou o Git, criou um repositório, viu o trabalho passar do diretório de trabalho para a área de preparação e depois para o histórico, e aprendeu a consultar e revisar o que aconteceu.

A ideia mais importante é que o Git não grava tudo automaticamente. Você decide o que entra em cada versão e descreve o motivo. Essa escolha consciente, sustentada por `status`, `diff`, `add` e `commit`, transforma uma pasta de arquivos em um histórico útil.

A sua experiência com mudanças controladas em fontes continua valendo: responsabilidade sobre o que se altera, rastreabilidade e cuidado com o que é fonte e o que é gerado já estavam com você. O Git é a ferramenta nova para praticá-las no desenvolvimento Java.

Por ora, o seu projeto existe apenas no seu computador. O próximo problema é natural: como disponibilizá-lo a outras pessoas e permitir que várias contribuam? No Tópico 2, você usará o recurso de controle de versão do Visual Studio Code, que já vem integrado ao editor, para trabalhar com o GitHub, repositórios remotos e branches.
