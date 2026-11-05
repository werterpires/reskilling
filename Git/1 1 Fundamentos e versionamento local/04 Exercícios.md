# Exercícios — Tópico 1: Fundamentos e versionamento local

## Projeto prático: versionar o Gestor de Tarefas

Nos tópicos de Java, você construiu o **Gestor de Tarefas**: `Tarefa`, `Responsavel`, `GestorTarefas`, `TarefaComum` e `TarefaUrgente`. Até agora, o único histórico desse projeto era a pasta no seu computador. Se uma alteração quebrasse algo, não haveria como saber o que mudou nem como comparar com a versão anterior.

Nesta sequência, você colocará essa mesma pasta sob controle do Git, **somente no seu computador**. Nada será enviado para a internet: GitHub, repositórios remotos e branches ficam para o próximo tópico.

Ao longo dos exercícios, você irá:

- confirmar o Git e o Git Bash instalados no Windows;
- configurar nome, e-mail e a branch inicial `main`;
- transformar a pasta `gestor-tarefas` em um repositório com `git init`;
- distinguir diretório de trabalho, área de preparação e repositório;
- acompanhar o estado dos arquivos com `git status`;
- preparar arquivos com `git add` e registrá-los com `git commit`;
- escrever um `.gitignore` para que a pasta `out/` não seja versionada;
- consultar o histórico com `git log` e comparar alterações com `git diff`;
- separar duas alterações no mesmo arquivo em dois commits;
- terminar com um repositório limpo e pelo menos cinco commits descritivos.

Esta etapa não usará GitHub, `git push`, `git pull`, `git clone`, branches, `git reset`, `git revert` nem `git cherry-pick`.

> Trabalhe no **computador físico**, não na VDI. Se algum comando não apresentar o resultado esperado, pare e procure o instrutor com a **mensagem completa** exibida no terminal.

Neste tópico, todos os comandos são digitados no **Git Bash**, com barras normais (`/`), inclusive para compilar e executar o Java.

---

## Resultado final esperado

Ao concluir, o comando abaixo deverá listar pelo menos cinco commits. Os códigos de sete caracteres (hashes) serão diferentes no seu computador; as mensagens podem ser parecidas com estas:

```text
18a81b9 (HEAD -> main) Documenta como compilar e executar
e3e134b Adiciona obterDataLimite em Tarefa
bfa453d Adiciona obterDescricao em Tarefa
fcdc2e5 Adiciona codigo-fonte do gestor de tarefas
563f9d7 Adiciona modelo inicial do gestor de tarefas
ccaecc1 Adiciona .gitignore para arquivos gerados
92fb812 Cria README do gestor de tarefas
```

E o estado do repositório deverá ser:

```text
On branch main
nothing to commit, working tree clean
```

Esse repositório será o ponto de partida do próximo tópico, que o publicará no GitHub.

---

## Ponto de partida

Use a pasta concluída nos tópicos de Java:

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
                        ├── Priorizavel.java      (opcional)
                        ├── Responsavel.java
                        ├── Tarefa.java
                        ├── TarefaComum.java
                        └── TarefaUrgente.java
```

Se `Priorizavel.java` não existir na sua pasta, siga normalmente: ele era opcional. Se a pasta `gestor-tarefas` não existir ou não compilar, **procure o instrutor** antes de continuar. Versionar um projeto quebrado só preserva o erro.

Ao final, a pasta terá mais dois itens: `README.md` e `.gitignore`. O Git também criará a pasta oculta `.git`.

---

## Exercício 1 — Abrir o Git Bash e confirmar a instalação

Abra o **Git Bash** pelo menu Iniciar. É uma janela de terminal semelhante ao Prompt de Comando, mas com comandos no estilo Unix e caminhos com barras normais. Execute:

```bash
git --version
```

Resultado esperado (o número muda conforme a versão instalada):

```text
git version 2.53.0.windows.1
```

Se aparecer `command not found`, feche o Git Bash, abra-o novamente e repita. Se continuar, a instalação não terminou ou o Git não está no `PATH`. Chame o instrutor com a mensagem completa. Em computador corporativo, se a instalação for bloqueada por política de segurança, fale com o suporte de TI em vez de tentar contornar a restrição.

Experimente três comandos de navegação:

```bash
pwd
ls
cd ..
```

| Comando | O que faz |
|---|---|
| `pwd` | mostra a pasta atual |
| `ls` | lista o conteúdo da pasta atual |
| `cd` | muda de pasta |

Neste ambiente, `C:\Users\seu-usuario\Documents` aparece como `/c/Users/seu-usuario/Documents`. A letra do disco vira uma pasta minúscula e as barras passam a ser `/`.

### Ponte com COBOL

No ambiente mainframe também existem ferramentas próprias para guardar versões e controlar mudanças em fontes. Aqui o objetivo é outro: aprender o Git, que é o padrão do desenvolvimento Java moderno. Trate a comparação como intenção parecida (guardar e rastrear mudanças), não como ferramentas equivalentes.

---

## Exercício 2 — Configurar sua identidade e a branch inicial

Cada commit registra quem o fez. Informe seu nome e seu e-mail, trocando os valores entre aspas:

```bash
git config --global user.name "Marina Souza"
git config --global user.email "marina.souza@exemplo.com"
```

Use um e-mail que você aceite ver gravado no histórico do projeto. **Não** coloque senhas ou tokens em nenhuma configuração do Git.

Garanta também que os novos repositórios comecem na branch `main`:

```bash
git config --global init.defaultBranch main
```

Confira o que ficou gravado:

```bash
git config --global --get user.name
git config --global --get user.email
git config --global --list
```

Resultado esperado (a última lista pode ter outras linhas, definidas pelo instalador):

```text
Marina Souza
marina.souza@exemplo.com
user.name=Marina Souza
user.email=marina.souza@exemplo.com
init.defaultbranch=main
```

| Parte do comando | Significado |
|---|---|
| `git config` | consulta ou altera configurações |
| `--global` | vale para todos os repositórios do seu usuário no computador |
| `user.name`, `user.email` | autor que aparecerá nos commits |
| `--get` | mostra apenas o valor de uma chave |

A identidade do commit **não é login**. Ela apenas identifica o autor no histórico. A autenticação em serviços remotos é outro assunto e será tratada quando houver um remoto.

Se o editor padrão tiver sido configurado na instalação, ele aparecerá como `core.editor`. Não é obrigatório conferir isso agora, mas é útil saber: quando você rodar `git commit` sem `-m`, o Git abrirá esse editor.

---

## Exercício 3 — Entrar na pasta do projeto e verificar o ponto de partida

No Git Bash, vá até a pasta do projeto. Substitua o caminho pelo local real de `gestor-tarefas`:

```bash
cd /c/Users/seu-usuario/Documents/gestor-tarefas
pwd
ls
```

Resultado esperado:

```text
/c/Users/seu-usuario/Documents/gestor-tarefas
modelo-inicial.txt  out  src
```

Dicas para o caminho:

- no Explorador de Arquivos, abra a pasta, clique na barra de endereço e leia o caminho; depois converta `C:\` em `/c/` e as barras invertidas em `/`;
- se algum nome tiver espaço, coloque o caminho entre aspas: `cd "/c/Users/seu-usuario/Meus Documentos/gestor-tarefas"`;
- se `ls` não mostrar `src`, você está na pasta errada.

Antes de versionar, compile e execute o projeto, para saber que a base funciona:

```bash
javac -d out src/br/com/curso/tarefas/dominio/*.java src/br/com/curso/tarefas/AplicacaoGestorTarefas.java
java -cp out br.com.curso.tarefas.AplicacaoGestorTarefas
```

O programa deve exibir o título `=== GESTOR DE TAREFAS ===`, o resumo das três tarefas e terminar com as três tarefas `EM ANDAMENTO`. Se houver erro de compilação, chame o instrutor.

Agora pergunte ao Git se esta pasta já está sob controle de versão:

```bash
git status
```

Resultado esperado:

```text
fatal: not a git repository (or any parent up to mount point /)
```

O trecho depois de `repository` varia conforme a versão do Git e o local da pasta, e pode ser seguido de uma segunda linha. O importante é começar com `fatal: not a git repository`, e a ideia é esta: **uma pasta comum não é um repositório**. Você tem arquivos, mas o Git ainda não sabe nada sobre eles.

---

## Exercício 4 — Inicializar o repositório

Dentro da pasta `gestor-tarefas`, execute:

```bash
git init
```

Resultado esperado:

```text
Initialized empty Git repository in C:/Users/seu-usuario/Documents/gestor-tarefas/.git/
```

Liste todos os itens, inclusive os ocultos:

```bash
ls -la
```

Procure a pasta `.git` entre `modelo-inicial.txt`, `out` e `src`:

```text
drwxr-xr-x 1 seu-usuario ... .git
-rw-r--r-- 1 seu-usuario ... modelo-inicial.txt
drwxr-xr-x 1 seu-usuario ... out
drwxr-xr-x 1 seu-usuario ... src
```

A pasta `.git` guarda o banco de dados do repositório: histórico, configurações locais e metadados. Os arquivos que você edita continuam na pasta normal do projeto.

> Não apague nem edite manualmente a pasta `.git`. Apagá-la remove o histórico e deixa a pasta de volta a um simples diretório.

Confirme a branch inicial:

```bash
git status
```

A primeira linha deve ser:

```text
On branch main
```

Se aparecer `On branch master`, a configuração do Exercício 2 não foi aplicada. Como ainda não há nenhum commit, renomeie a branch atual:

```bash
git branch -M main
```

Esse é o único uso de branch neste tópico. Confira com `git status` novamente.

---

## Exercício 5 — Ler o estado inicial e identificar as três áreas

O Git organiza o trabalho em três áreas:

```text
Diretório de trabalho  →  Área de preparação (staging)  →  Repositório
(arquivos que você edita)   (o que irá no próximo commit)   (histórico de commits)
```

Execute:

```bash
git status
```

Resultado esperado:

```text
On branch main

No commits yet

Untracked files:
  (use "git add <file>..." to include in what will be committed)
	modelo-inicial.txt
	out/
	src/

nothing added to commit but untracked files present (use "git add" to track)
```

Se existir uma pasta `.vscode/` no projeto, ela também aparecerá na lista. Você tratará dela no Exercício 7.

Leia a saída:

| O que aparece | O que significa |
|---|---|
| `No commits yet` | o repositório ainda não tem nenhum registro |
| `Untracked files` | arquivos que o Git vê, mas que não fazem parte do histórico |
| `nothing added to commit` | nada foi preparado para o próximo commit |

Os arquivos existem no **diretório de trabalho**, mas ainda não foram preparados nem registrados. Criar ou salvar um arquivo não o versiona.

Responda mentalmente antes de seguir:

1. Em que área estão `src/` e `modelo-inicial.txt` agora? (diretório de trabalho)
2. Quantas versões do projeto estão no repositório? (nenhuma)
3. Salvar o arquivo no VS Code cria um commit? (não)

---

## Exercício 6 — Preparar e registrar o primeiro commit

Abra o VS Code na pasta `gestor-tarefas` e crie, na raiz do projeto, o arquivo `README.md`:

```markdown
# Gestor de Tarefas

Projeto do curso de Java: cadastro e acompanhamento de tarefas por linha de comando.

## Funcionalidades

- Cadastro de responsável
- Tarefas comuns e urgentes
- Busca por código e por prioridade
```

Antes de preparar qualquer coisa, tente registrar:

```bash
git commit -m "teste"
```

O Git recusa. A última linha da resposta é:

```text
nothing added to commit but untracked files present (use "git add" to track)
```

Ele não registra o que não foi preparado. Agora prepare **somente** o README:

```bash
git add README.md
git status
```

Resultado esperado:

```text
On branch main

No commits yet

Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
	new file:   README.md

Untracked files:
  (use "git add <file>..." to include in what will be committed)
	modelo-inicial.txt
	out/
	src/

```

O README está na área de preparação; o restante continua apenas no diretório de trabalho. Registre:

```bash
git commit -m "Cria README do gestor de tarefas"
```

Resultado esperado:

```text
[main (root-commit) 92fb812] Cria README do gestor de tarefas
 1 file changed, 9 insertions(+)
 create mode 100644 README.md
```

`root-commit` indica o primeiro commit do repositório. O número de linhas pode variar conforme o texto que você escreveu.

Observações importantes:

- `-m` fornece a mensagem na própria linha. Sem ele, o Git abre o editor padrão; se isso acontecer, escreva a mensagem, salve e feche, ou feche com mensagem vazia para cancelar.
- Neste tópico, as mensagens de commit estão sem acentos, para não depender da codificação do terminal.
- Se aparecer um aviso `LF will be replaced by CRLF`, não é erro. Ele vem da regra de finais de linha escolhida na instalação (Windows usa CRLF; o histórico guarda LF).

---

## Exercício 7 — Escrever o `.gitignore` antes de versionar o código

A pasta `out/` contém arquivos `.class`, que o `javac` gera a partir do código-fonte. Eles podem ser recriados a qualquer momento. Versioná-los poluiria o histórico: cada compilação geraria alterações sem valor.

Na raiz do projeto, crie o arquivo `.gitignore` (com o ponto no início e **sem** extensão `.txt`):

```text
# Resultado da compilacao
out/
*.class

# Logs
*.log

# Configuracoes locais do VS Code
.vscode/
```

| Regra | O que ignora |
|---|---|
| `out/` | a pasta `out` e tudo dentro dela |
| `*.class` | qualquer arquivo com extensão `.class` |
| `*.log` | qualquer arquivo de log |
| `.vscode/` | configurações locais do editor |

Verifique o efeito:

```bash
git status
```

Resultado esperado:

```text
On branch main
Untracked files:
  (use "git add <file>..." to include in what will be committed)
	.gitignore
	modelo-inicial.txt
	src/

nothing added to commit but untracked files present (use "git add" to track)
```

`out/` desapareceu da lista. Para ver **qual regra** está valendo, use `git check-ignore`:

```bash
git check-ignore -v out/br/com/curso/tarefas/AplicacaoGestorTarefas.class
```

Resultado esperado:

```text
.gitignore:2:out/	out/br/com/curso/tarefas/AplicacaoGestorTarefas.class
```

A resposta indica o arquivo de regras, o número da linha e o padrão que bateu. Se não houver saída, o arquivo não está sendo ignorado.

Registre o `.gitignore`: ele também faz parte do projeto, e outras pessoas precisarão das mesmas regras.

```bash
git add .gitignore
git commit -m "Adiciona .gitignore para arquivos gerados"
```

Resultado esperado:

```text
[main ccaecc1] Adiciona .gitignore para arquivos gerados
 1 file changed, 9 insertions(+)
 create mode 100644 .gitignore
```

> O `.gitignore` só impede que arquivos **ainda não rastreados** sejam adicionados. Se um arquivo já tiver sido registrado, a regra não o remove do histórico. Por isso, o `.gitignore` foi escrito antes de `git add` no código.

### Ponte com COBOL

Em COBOL, o programa compilado (módulo de carga) é produzido a partir do fonte e pode ser regenerado. A lógica é parecida: o que você guarda como fonte de verdade é o código-fonte, não o resultado da compilação. Em Java, esse resultado fica em `out/`.

---

## Exercício 8 — Registrar o `modelo-inicial.txt`

O Git não é exclusivo de código Java: ele versiona qualquer arquivo de texto. Registre o modelo do projeto num commit só dele:

```bash
git add modelo-inicial.txt
git status
```

Resultado esperado:

```text
On branch main
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
	new file:   modelo-inicial.txt

Untracked files:
  (use "git add <file>..." to include in what will be committed)
	src/

```

Registre:

```bash
git commit -m "Adiciona modelo inicial do gestor de tarefas"
```

Resultado esperado (o número de linhas depende do seu arquivo):

```text
[main 563f9d7] Adiciona modelo inicial do gestor de tarefas
 1 file changed, 11 insertions(+)
 create mode 100644 modelo-inicial.txt
```

Perceba que `src/` continuou fora do commit. O commit registra exatamente o que estava na área de preparação.

---

## Exercício 9 — Registrar o código-fonte

Antes de preparar o código, **sempre leia o status**:

```bash
git status
```

Deve restar apenas `src/` como não rastreado. Existe também a forma `git add .`, que prepara tudo o que mudou na pasta atual e abaixo dela. Ela é prática, mas só é segura quando você conferiu o que está pendente. Aqui, prepare apenas a pasta de código:

```bash
git add src/
git status
```

Resultado esperado (7 arquivos se você criou `Priorizavel.java`; 6 caso contrário):

```text
On branch main
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
	new file:   src/br/com/curso/tarefas/AplicacaoGestorTarefas.java
	new file:   src/br/com/curso/tarefas/dominio/GestorTarefas.java
	new file:   src/br/com/curso/tarefas/dominio/Priorizavel.java
	new file:   src/br/com/curso/tarefas/dominio/Responsavel.java
	new file:   src/br/com/curso/tarefas/dominio/Tarefa.java
	new file:   src/br/com/curso/tarefas/dominio/TarefaComum.java
	new file:   src/br/com/curso/tarefas/dominio/TarefaUrgente.java

```

Registre e confira:

```bash
git commit -m "Adiciona codigo-fonte do gestor de tarefas"
git status
```

Resultado esperado do commit (o total de linhas varia):

```text
[main fcdc2e5] Adiciona codigo-fonte do gestor de tarefas
 7 files changed, 422 insertions(+)
 create mode 100644 src/br/com/curso/tarefas/AplicacaoGestorTarefas.java
 ...
```

E do status:

```text
On branch main
nothing to commit, working tree clean
```

`working tree clean` significa que o diretório de trabalho e o último commit são iguais. Note que `out/` não aparece, nem como pendência: o `.gitignore` está funcionando.

---

## Exercício 10 — Consultar o histórico

Veja o histórico completo:

```bash
git log
```

Resultado esperado (hashes, datas e nome variam; os mais recentes ficam no topo):

```text
commit fcdc2e5766d24a0a1613a6a661b1b4386c0f0318 (HEAD -> main)
Author: Marina Souza <marina.souza@exemplo.com>
Date:   Thu Nov 5 18:08:56 2026 -0300

    Adiciona codigo-fonte do gestor de tarefas

commit 563f9d754387271336349309011375807d580904
...
```

Se a saída ocupar mais que a tela, o Git abre um paginador. Use as setas para rolar e a tecla `q` para sair.

Cada commit mostra o **hash** (identificador único), o **autor**, a **data** e a **mensagem**. Veja agora a versão resumida:

```bash
git log --oneline
```

Resultado esperado:

```text
fcdc2e5 (HEAD -> main) Adiciona codigo-fonte do gestor de tarefas
563f9d7 Adiciona modelo inicial do gestor de tarefas
ccaecc1 Adiciona .gitignore para arquivos gerados
92fb812 Cria README do gestor de tarefas
```

Cada linha é um commit: hash abreviado e mensagem. `HEAD -> main` indica que você está na branch `main` e que esse é o commit mais recente. Releia as quatro mensagens: elas descrevem o que mudou, sem recorrer a palavras como "teste", "alterações" ou "versão final".

Se quiser limitar a lista, experimente `git log -2`.

### Ponte com COBOL

Em um ambiente corporativo de mainframe, também é comum registrar quem alterou um fonte, quando e por quê. A intenção é a mesma que a do histórico do Git: responder a essas perguntas sem depender de memória ou de cópias de pastas. Em Git, essa resposta é o próprio `git log`.

---

## Exercício 11 — Alterar o código e revisar com `git diff`

O atributo `descricao` existe em `Tarefa`, mas não há como consultá-lo fora da classe. Abra `src/br/com/curso/tarefas/dominio/Tarefa.java` e acrescente, logo abaixo de `obterTitulo()`:

```java
    public String obterDescricao() {
        return descricao;
    }
```

Salve, compile e execute para confirmar que o projeto continua funcionando:

```bash
javac -d out src/br/com/curso/tarefas/dominio/*.java src/br/com/curso/tarefas/AplicacaoGestorTarefas.java
java -cp out br.com.curso.tarefas.AplicacaoGestorTarefas
```

Veja o que o Git percebeu:

```bash
git status
```

Resultado esperado (a pasta `out/`, recompilada, não aparece):

```text
On branch main
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
	modified:   src/br/com/curso/tarefas/dominio/Tarefa.java

no changes added to commit (use "git add" and/or "git commit -a")
```

Agora compare o conteúdo:

```bash
git diff
```

Resultado esperado (os hashes em `index` e os números em `@@` podem variar):

```text
diff --git a/src/br/com/curso/tarefas/dominio/Tarefa.java b/src/br/com/curso/tarefas/dominio/Tarefa.java
index a31dbe5..96fd34c 100644
--- a/src/br/com/curso/tarefas/dominio/Tarefa.java
+++ b/src/br/com/curso/tarefas/dominio/Tarefa.java
@@ -79,6 +79,10 @@ public abstract class Tarefa implements Priorizavel {
         return titulo;
     }
 
+    public String obterDescricao() {
+        return descricao;
+    }
+
     public int obterEstimativaHoras() {
         return estimativaHoras;
     }
```

| Marca | Significado |
|---|---|
| `---` / `+++` | versão anterior (`a/`) e versão nova (`b/`) do arquivo |
| `@@ ... @@` | trecho do arquivo onde a alteração aconteceu |
| linha com `+` | linha acrescentada |
| linha com `-` | linha removida (não há nenhuma aqui) |
| linha sem marca | contexto, sem mudança |

Use `git diff` como **revisão** antes de registrar: ele deve conter só o que você pretendia mudar.

---

## Exercício 12 — Preparar uma alteração e alterar o arquivo de novo

Prepare a mudança revisada e confira o que está na área de preparação:

```bash
git add src/br/com/curso/tarefas/dominio/Tarefa.java
git diff --staged
```

O resultado deve ser o mesmo trecho do exercício anterior. `git diff` compara o diretório de trabalho com a área de preparação; `git diff --staged` compara a área de preparação com o último commit.

Agora, **sem fazer commit**, volte a `Tarefa.java` e acrescente outro método, logo abaixo de `obterDescricao()`:

```java
    public LocalDate obterDataLimite() {
        return dataLimite;
    }
```

`LocalDate` já é importado nessa classe. Salve e execute:

```bash
git status
```

Resultado esperado:

```text
On branch main
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
	modified:   src/br/com/curso/tarefas/dominio/Tarefa.java

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
	modified:   src/br/com/curso/tarefas/dominio/Tarefa.java

```

O **mesmo arquivo** aparece duas vezes: uma versão já preparada e outra alteração ainda fora da preparação. Compare:

```bash
git diff
git diff --staged
```

- `git diff` deve mostrar apenas o método `obterDataLimite()`;
- `git diff --staged` deve mostrar apenas `obterDescricao()`.

Isso responde a uma dúvida frequente: o commit registra o que está **preparado**, não tudo o que existe na pasta.

---

## Exercício 13 — Fazer dois commits do mesmo arquivo

Registre agora somente o que está preparado:

```bash
git commit -m "Adiciona obterDescricao em Tarefa"
git status
```

Resultado esperado:

```text
[main bfa453d] Adiciona obterDescricao em Tarefa
 1 file changed, 4 insertions(+)
On branch main
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
	modified:   src/br/com/curso/tarefas/dominio/Tarefa.java

no changes added to commit (use "git add" and/or "git commit -a")
```

O `obterDataLimite()` ficou de fora, como esperado. Prepare e registre a segunda alteração:

```bash
git add src/br/com/curso/tarefas/dominio/Tarefa.java
git commit -m "Adiciona obterDataLimite em Tarefa"
```

Resultado esperado:

```text
[main e3e134b] Adiciona obterDataLimite em Tarefa
 1 file changed, 4 insertions(+)
```

Compile e execute novamente. Dois commits pequenos, cada um com uma única intenção, são mais fáceis de entender e de revisar do que um commit com várias mudanças misturadas.

---

## Exercício 14 — Documentar no README e provar o `.gitignore`

Abra o `README.md` e acrescente ao final, depois de uma linha em branco:

````markdown
## Como compilar e executar

```bash
javac -d out src/br/com/curso/tarefas/dominio/*.java src/br/com/curso/tarefas/AplicacaoGestorTarefas.java
java -cp out br.com.curso.tarefas.AplicacaoGestorTarefas
```

A pasta `out/` é gerada pela compilação e não é versionada.
````

Revise com `git diff` e confirme que só há linhas com `+`. Registre em um commit exclusivo de documentação:

```bash
git add README.md
git commit -m "Documenta como compilar e executar"
```

Resultado esperado:

```text
[main 18a81b9] Documenta como compilar e executar
 1 file changed, 9 insertions(+)
```

Agora prove que o `.gitignore` está cumprindo seu papel. Recompile, crie um arquivo de log qualquer e consulte o status:

```bash
javac -d out src/br/com/curso/tarefas/dominio/*.java src/br/com/curso/tarefas/AplicacaoGestorTarefas.java
echo "teste" > execucao.log
git status
```

Resultado esperado:

```text
On branch main
nothing to commit, working tree clean
```

Os arquivos `.class` foram recriados e `execucao.log` existe, mas o Git não os mostra. Remova o arquivo de teste:

```bash
rm execucao.log
```

---

## Exercício 15 — Conferência final do repositório

Execute cada comando e compare com os itens abaixo.

```bash
git status
git log --oneline
git log --oneline | wc -l
git ls-files
```

O terceiro comando conta as linhas do histórico: deve imprimir `7` se você seguiu o roteiro.

- [ ] `git status` mostra `On branch main` e `nothing to commit, working tree clean`.
- [ ] `git log --oneline` lista pelo menos cinco commits (no roteiro, sete).
- [ ] Todas as mensagens descrevem a mudança; nenhuma é "teste", "alterações" ou "versão final".
- [ ] `git ls-files` lista `.gitignore`, `README.md`, `modelo-inicial.txt` e os arquivos de `src/`.
- [ ] `git ls-files` **não** lista nenhum arquivo de `out/`.
- [ ] `git config --global --get user.name` e `user.email` mostram seus dados.
- [ ] O projeto compila e executa com os comandos do README.
- [ ] `Tarefa.java` possui `obterDescricao()` e `obterDataLimite()` em commits separados.
- [ ] Você consegue explicar a diferença entre `git add` e `git commit`.
- [ ] Você consegue explicar a diferença entre `git diff` e `git diff --staged`.
- [ ] Nenhum arquivo foi enviado para a internet.

Resultado esperado de `git ls-files` (com `Priorizavel.java` apenas se você o criou):

```text
.gitignore
README.md
modelo-inicial.txt
src/br/com/curso/tarefas/AplicacaoGestorTarefas.java
src/br/com/curso/tarefas/dominio/GestorTarefas.java
src/br/com/curso/tarefas/dominio/Priorizavel.java
src/br/com/curso/tarefas/dominio/Responsavel.java
src/br/com/curso/tarefas/dominio/Tarefa.java
src/br/com/curso/tarefas/dominio/TarefaComum.java
src/br/com/curso/tarefas/dominio/TarefaUrgente.java
```

---

## Entrega

Entregue a pasta inteira `gestor-tarefas`, **incluindo a pasta oculta `.git`**, que contém o histórico. A estrutura esperada é:

```text
gestor-tarefas/
├── .git/
├── .gitignore
├── README.md
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
                        ├── Priorizavel.java      (se existir)
                        ├── Responsavel.java
                        ├── Tarefa.java
                        ├── TarefaComum.java
                        └── TarefaUrgente.java
```

Também entregue a saída de `git log --oneline` copiada do terminal. O estado da entrega é:

- repositório na branch `main`;
- `README.md`, `.gitignore`, `modelo-inicial.txt` e `src/` versionados;
- `out/` ignorada;
- `git status` limpo;
- pelo menos cinco commits com mensagens descritivas.

Não publique este repositório em lugar algum ainda. No próximo tópico, você o publicará no GitHub.

---

## Comandos de referência

```bash
git --version                      # confirma a instalação
git config --global user.name "Seu Nome"
git config --global user.email "seu.email@exemplo.com"
git init                           # cria o repositório na pasta atual
git status                         # estado das áreas
git add arquivo-ou-pasta           # prepara para o próximo commit
git commit -m "Mensagem"           # registra o que está preparado
git log --oneline                  # histórico resumido
git diff                           # alterações ainda não preparadas
git diff --staged                  # alterações preparadas
git check-ignore -v caminho        # mostra qual regra ignora o caminho
```

O ciclo básico do dia a dia é:

```text
editar → compilar/testar → git status → git diff → git add → git commit
```

---

## Síntese final

Neste tópico, o Gestor de Tarefas ganhou memória. A pasta virou um repositório, e cada commit passou a registrar uma versão com autor, data e mensagem. Você viu as três áreas do Git trabalhando juntas: arquivos são editados no diretório de trabalho, escolhidos na área de preparação e gravados no repositório. `git status` informa onde cada coisa está, `git diff` permite revisar antes de registrar e `git log` conta a história do projeto.

Você também aprendeu três hábitos que serão úteis em qualquer equipe: separar alterações de natureza diferente em commits diferentes, escrever mensagens que descrevem a mudança e manter fora do histórico o que pode ser recriado, como a pasta `out/`.

Por enquanto, esse histórico existe apenas no seu computador. No próximo tópico, você publicará o mesmo repositório no GitHub, usará o VS Code para acompanhar as mudanças e criará branches para trabalhar com segurança.
