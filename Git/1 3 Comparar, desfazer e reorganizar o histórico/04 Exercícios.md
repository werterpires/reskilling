# Exercícios — Tópico 3: Comparar, desfazer e reorganizar o histórico

## Projeto prático: arrumar o histórico do Gestor de Tarefas

No Tópico 2, o `gestor-tarefas` foi publicado no GitHub e ganhou branches, merges e um conflito resolvido. Até aqui, todo comando acrescentou commits. Nesta sequência, você vai **ler** o histórico com precisão, **desfazer** em cada nível e **reorganizar** commits, sempre no mesmo repositório.

Ao longo dos exercícios, você irá:

- apontar para commits com `HEAD~N` e consultar versões antigas com `git show`;
- comparar commits, áreas e branches com as variações de `git diff`;
- descartar alterações com `git restore`;
- corrigir um commit com `git commit --amend`;
- usar `git reset --soft`, `--mixed` e `--hard`, e recuperar um commit pelo `git reflog`;
- desfazer um erro já publicado com `git revert`;
- guardar trabalho pela metade com `git stash` para conseguir dar `pull`;
- atualizar uma branch com `git rebase` e publicá-la com `git push --force-with-lease`;
- resolver um conflito durante um `git pull --rebase`;
- juntar commits com o rebase interativo;
- copiar um único commit com `git cherry-pick`.

Esta etapa não usa Pull Requests nem Maven: eles são assunto do próximo tópico.

> Continue trabalhando no **computador físico**, não na VDI. Se algum comando não apresentar o resultado esperado, pare e procure o instrutor com a mensagem **completa** exibida no terminal.

> **Rede de segurança.** Vários comandos deste tópico reescrevem o histórico. Antes de cada um, rode `git status`. Se algo sumir, **não tente consertar no impulso**: rode `git reflog` e chame o instrutor. O que foi commitado quase sempre pode ser recuperado.

Todos os comandos usam o **Git Bash**, com barras normais (`/`). As mensagens de commit não têm acentos, como no Tópico 2.

---

## Ponto de partida

Use o resultado do Tópico 2: a pasta `gestor-tarefas`, na branch `main`, sincronizada com o GitHub, com `status` limpo, pelo menos 16 commits e dois commits de merge. O `README.md` deve conter a descrição resolvida no Exercício 31 e as seções **Como executar**, **Funcionalidades**, **Autoria** e **Próximos passos**. `GestorTarefas.java` deve ter `contarPendentes` e `contarConcluidas`.

Se você não tiver esse repositório, **procure o instrutor** antes de continuar.

---

## Exercício 34 — Confirmar o ponto de partida e configurar o editor

Abra a pasta `gestor-tarefas` no VS Code e use o terminal integrado com **Git Bash**. Confira o estado:

```bash
git status -sb
git fetch
git status -sb
git log --oneline -3
```

Resultado esperado (os hashes serão outros):

```text
## main...origin/main
## main...origin/main
48057eb Resolve conflito na descricao do README
6bfbe80 Reescreve descricao com foco em tipos de tarefa
e1219cd Reescreve descricao com foco em equipe
```

A linha `## main...origin/main`, sem `ahead` nem `behind`, mostra que o local e o GitHub estão no mesmo ponto. **Anote o hash do primeiro commit da lista.** Ele é o seu ponto de partida e será usado na conferência final:

```text
Ponto de partida: ________
```

Compile e execute para confirmar que o projeto funciona:

```bash
javac -d out src/br/com/curso/tarefas/dominio/*.java src/br/com/curso/tarefas/AplicacaoGestorTarefas.java
java -cp out br.com.curso.tarefas.AplicacaoGestorTarefas
```

A saída deve conter `Tarefas pendentes: 3`.

### Configurar o VS Code como editor do Git

Alguns comandos deste tópico abrem um editor de texto. No Git Bash, o padrão é o Vim, difícil para quem não o conhece. Configure o VS Code:

```bash
code --version
git config --global core.editor "code --wait"
git config --global --get core.editor
```

O primeiro comando deve exibir a versão do VS Code. O último deve imprimir `code --wait`. O `--wait` faz o Git aguardar até você **fechar a aba** do arquivo aberto.

Se `code --version` responder `command not found`, use o Bloco de Notas:

```bash
git config --global core.editor notepad
```

Se algum dia o Vim abrir mesmo assim, saia sem alterar nada com `Esc`, depois `:q!` e `Enter`.

---

## Exercício 35 — Apontar para commits e consultar versões antigas

`HEAD` é o commit atual, e `HEAD~N` é o commit `N` passos atrás. Em commits de merge, o `~` segue o **primeiro pai**, que é a linha da branch que recebeu o merge. A opção `--first-parent` lista os commits na mesma ordem:

```bash
git log --oneline --first-parent -5
```

```text
48057eb Resolve conflito na descricao do README
6bfbe80 Reescreve descricao com foco em tipos de tarefa
429256f Integra contagem de tarefas concluidas
3900f9e Registra proximos passos no README
3b9ac09 Exibe tarefas pendentes na aplicacao
```

Nessa lista, a primeira linha é `HEAD`, a segunda é `HEAD~1`, e assim por diante. Compare com `git log --oneline -5`, sem a opção: o commit `Reescreve descricao com foco em equipe`, que veio da outra branch, aparece no meio.

Veja um commit inteiro:

```bash
git show --stat HEAD~3
git show HEAD~3
```

Resultado esperado do segundo comando (autor e data serão os seus):

```text
commit 3900f9eb2ee80ae2104ed6b3d31777568056e7ab
Author: SEU NOME <seu-email>
Date:   ...

    Registra proximos passos no README

diff --git a/README.md b/README.md
index 9bb4582..69fed13 100644
--- a/README.md
+++ b/README.md
@@ -16,3 +16,8 @@ Descrição: Gestor de tarefas em Java para praticar Git.
 ## Autoria

 Projeto didático mantido por SEU NOME.
+
+## Próximos passos
+
+- Contar tarefas por situação
+- Listar tarefas por responsável
```

Agora consulte o `README.md` como ele era no primeiro commit do Tópico 2, sem alterar nada na pasta:

```bash
git show HEAD~8:README.md
```

```text
# Gestor de Tarefas

Descrição: Gestor de tarefas em Java para praticar Git.

## Como executar

1. Compile com javac, gerando as classes na pasta out.
2. Execute a classe br.com.curso.tarefas.AplicacaoGestorTarefas.
```

Se a saída for outra, o seu histórico tem um número diferente de commits. Use `git log --oneline --first-parent` para achar o hash de `Reorganiza README com descricao e instrucoes` e rode `git show HASH:README.md`.

Por fim, liste só os commits que alteraram o README:

```bash
git log --oneline -- README.md
```

Confirme com `git status` que nada mudou: `git show` apenas lê.

---

## Exercício 36 — Comparar commits

Compare a versão de três commits atrás com a atual, começando pelo resumo:

```bash
git diff --stat HEAD~3 HEAD
git diff --name-only HEAD~3 HEAD
```

```text
 README.md                                           |  2 +-
 src/br/com/curso/tarefas/dominio/GestorTarefas.java | 12 ++++++++++++
 2 files changed, 13 insertions(+), 1 deletion(-)
README.md
src/br/com/curso/tarefas/dominio/GestorTarefas.java
```

Entre esses dois pontos, entraram o método `contarConcluidas` e a nova descrição. Veja só a diferença do README:

```bash
git diff HEAD~3 HEAD -- README.md
```

O `--` separa os commits do caminho do arquivo. Agora veja o que o commit de resolução do conflito mudou em relação à versão da `main`:

```bash
git diff HEAD~1 HEAD
```

```text
@@ -1,6 +1,6 @@
 # Gestor de Tarefas

-Descrição: Gestor de tarefas urgentes e comuns, desenvolvido em Java.
+Descrição: Gestor de tarefas comuns e urgentes para acompanhar o trabalho de uma equipe.

 ## Como executar
```

Numa linha longa, fica difícil ver o que mudou. Peça a diferença palavra a palavra:

```bash
git diff --word-diff HEAD~1 HEAD
```

```text
Descrição: Gestor de tarefas [-urgentes-]{+comuns+} e [-comuns, desenvolvido em Java.-]{+urgentes para acompanhar o trabalho de uma equipe.+}
```

`[-...-]` é o texto removido e `{+...+}` é o texto acrescentado.

| Comando | Para que serve |
|---|---|
| `git diff A B` | diferença entre dois commits; `A` é o "antes" |
| `git diff A B -- arquivo` | a mesma diferença, só de um arquivo |
| `--stat` | resumo por arquivo |
| `--name-only` | só os nomes dos arquivos |
| `--word-diff` | diferença palavra a palavra |

---

## Exercício 37 — As três comparações e o `git restore`

Abra o `README.md` e, na seção **Autoria**, acrescente ao fim da frase o trecho `, durante o curso de Git`. A linha fica, com o seu nome:

```text
Projeto didático mantido por SEU NOME, durante o curso de Git.
```

Salve e prepare:

```bash
git add README.md
```

Agora, **sem fazer commit**, acrescente ao final da seção **Próximos passos**:

```text
- Exportar a lista de tarefas
```

Salve e consulte:

```bash
git status
```

```text
Changes to be committed:
	modified:   README.md

Changes not staged for commit:
	modified:   README.md
```

O mesmo arquivo tem uma parte preparada e outra não. Compare as três formas:

```bash
git diff
git diff --staged
git diff HEAD
```

- `git diff` mostra só `+- Exportar a lista de tarefas`, a parte **não preparada**;
- `git diff --staged` mostra só a troca da linha de autoria, a parte **preparada**;
- `git diff HEAD` mostra as duas, tudo o que mudou desde o último commit.

| Comando | Compara |
|---|---|
| `git diff` | diretório de trabalho × área de preparação |
| `git diff --staged` | área de preparação × último commit |
| `git diff HEAD` | diretório de trabalho × último commit |

Desista das duas alterações. Primeiro, tire o arquivo da preparação:

```bash
git restore --staged README.md
git status -s
```

```text
 M README.md
```

As duas alterações continuam no arquivo, agora sem preparação. Descarte-as:

```bash
git restore README.md
git status
```

```text
On branch main
Your branch is up to date with 'origin/main'.

nothing to commit, working tree clean
```

Abra o `README.md` e confirme que as duas alterações sumiram. `git restore` sem `--staged` descarta **sem volta** o que nunca foi commitado.

---

## Exercício 38 — Corrigir o último commit com `--amend`

Crie uma branch para uma nova funcionalidade:

```bash
git switch -c feature/contar-em-andamento
```

Em `GestorTarefas.java`, inclua logo antes de `exibirTarefasDoResponsavel`:

```java
    public int contarEmAndamento() {
        int quantidade = 0;

        for (Tarefa tarefa : tarefas) {
            if (tarefa.estaEmAndamento()) {
                quantidade++;
            }
        }

        return quantidade;
    }
```

Registre com uma mensagem **propositalmente errada**:

```bash
git add src/br/com/curso/tarefas/dominio/GestorTarefas.java
git commit -m "Implementa contagem de tarefas em andamnto"
```

```text
[feature/contar-em-andamento ef138ad] Implementa contagem de tarefas em andamnto
 1 file changed, 12 insertions(+)
```

Corrija a mensagem:

```bash
git commit --amend -m "Implementa contagem de tarefas em andamento"
git log --oneline -2
```

```text
[feature/contar-em-andamento ffc525f] Implementa contagem de tarefas em andamento
 1 file changed, 12 insertions(+)
ffc525f Implementa contagem de tarefas em andamento
48057eb Resolve conflito na descricao do README
```

Repare no hash: `ef138ad` virou `ffc525f`. O `--amend` não edita o commit, ele o **substitui** por outro.

Agora o segundo erro clássico, o arquivo esquecido. Em `AplicacaoGestorTarefas.java`, insira logo **abaixo** da linha `gestor.iniciarTarefasPendentes();`:

```java
        System.out.println(
                "Tarefas em andamento: " + gestor.contarEmAndamento()
        );
```

Inclua essa alteração no mesmo commit, mantendo a mensagem:

```bash
git add src/br/com/curso/tarefas/AplicacaoGestorTarefas.java
git commit --amend --no-edit
git show --stat --oneline HEAD
```

```text
[feature/contar-em-andamento d8416cf] Implementa contagem de tarefas em andamento
 2 files changed, 15 insertions(+)
d8416cf Implementa contagem de tarefas em andamento
 src/br/com/curso/tarefas/AplicacaoGestorTarefas.java |  3 +++
 src/br/com/curso/tarefas/dominio/GestorTarefas.java  | 12 ++++++++++++
 2 files changed, 15 insertions(+)
```

Compile e execute. A saída deve conter `Tarefas em andamento: 3` logo depois de `Tarefas pendentes: 3`.

Publique a branch, como cópia de segurança:

```bash
git push -u origin feature/contar-em-andamento
git switch main
```

O `--amend` foi seguro porque o commit ainda não estava no GitHub. A partir de agora, mudar esse commit exigiria um envio forçado, que você fará no Exercício 44.

---

## Exercício 39 — Juntar dois commits com `reset --soft`

Na `main`, acrescente ao final de **Próximos passos** a linha abaixo e registre:

```text
- Exportar a lista de tarefas
```

```bash
git commit -am "Planeja exportacao de tarefas"
```

Acrescente outra linha e registre:

```text
- Gerar relatório por responsável
```

```bash
git commit -am "Planeja relatorio por responsavel"
git log --oneline -3
```

```text
b988dfb Planeja relatorio por responsavel
eb8c81d Planeja exportacao de tarefas
48057eb Resolve conflito na descricao do README
```

Os dois commits têm a mesma intenção e deveriam ser um só. Volte a branch dois commits, mantendo tudo preparado:

```bash
git reset --soft HEAD~2
git log --oneline -1
git status
git diff --staged
```

```text
48057eb Resolve conflito na descricao do README
On branch main
Your branch is up to date with 'origin/main'.

Changes to be committed:
	modified:   README.md
...
+- Exportar a lista de tarefas
+- Gerar relatório por responsável
```

A branch voltou ao ponto de partida, mas as duas linhas continuam no arquivo e **preparadas**. Registre um único commit:

```bash
git commit -m "Amplia proximos passos no README"
git log --oneline -2
```

```text
588350b Amplia proximos passos no README
48057eb Resolve conflito na descricao do README
```

---

## Exercício 40 — Refazer um commit com `reset --mixed`

Na seção **Como executar** do `README.md`, acrescente um terceiro passo:

```text
3. Confira o resumo das tarefas no terminal.
```

Crie também, na raiz do projeto, um arquivo de anotações pessoais que **não deveria** ir para o repositório:

```bash
echo "lembrar de perguntar ao instrutor sobre o rebase" > rascunho.txt
```

Registre tudo de uma vez, como quem tem pressa:

```bash
git add .
git commit -m "Detalha execucao no README"
git show --stat --oneline HEAD
```

```text
75824cc Detalha execucao no README
 README.md    | 1 +
 rascunho.txt | 1 +
 2 files changed, 2 insertions(+)
```

O `rascunho.txt` entrou por engano. Desfaça o commit, mantendo as alterações nos arquivos:

```bash
git reset HEAD~1
git status
```

```text
Unstaged changes after reset:
M	README.md
On branch main
Your branch is ahead of 'origin/main' by 1 commit.
...
Changes not staged for commit:
	modified:   README.md

Untracked files:
	rascunho.txt
```

Sem opção, o `reset` usa o modo `--mixed`: a branch e a preparação voltam, e os arquivos ficam como estavam. Agora registre só o que deve entrar e apague o rascunho:

```bash
git add README.md
git commit -m "Detalha execucao no README"
rm rascunho.txt
git status
```

```text
On branch main
Your branch is ahead of 'origin/main' by 2 commits.
  (use "git push" to publish your local commits)

nothing to commit, working tree clean
```

| Modo | Branch | Preparação | Arquivos |
|---|---|---|---|
| `--soft` | volta | mantém | mantém |
| `--mixed` (padrão) | volta | volta | mantém |
| `--hard` | volta | volta | **volta** |

---

## Exercício 41 — Descartar com `reset --hard` e recuperar com o `reflog`

Em `AplicacaoGestorTarefas.java`, insira logo acima da linha `System.out.println("=== GESTOR DE TAREFAS ===");`:

```java
        System.out.println("Versao experimental do gestor");
```

Registre o experimento:

```bash
git commit -am "Testa mensagem experimental"
git log --oneline -3
```

```text
05c4d6e Testa mensagem experimental
e8260bf Detalha execucao no README
588350b Amplia proximos passos no README
```

Você decidiu que não quer nada dele. Descarte o commit **e** a alteração do arquivo:

```bash
git reset --hard HEAD~1
git log --oneline -3
```

```text
HEAD is now at e8260bf Detalha execucao no README
e8260bf Detalha execucao no README
588350b Amplia proximos passos no README
48057eb Resolve conflito na descricao do README
```

O commit sumiu do `log` e a linha sumiu do arquivo. Mas o Git ainda o guarda. Consulte o registro de movimentos do `HEAD`:

```bash
git reflog -5
```

```text
e8260bf HEAD@{0}: reset: moving to HEAD~1
05c4d6e HEAD@{1}: commit: Testa mensagem experimental
e8260bf HEAD@{2}: commit: Detalha execucao no README
588350b HEAD@{3}: reset: moving to HEAD~1
75824cc HEAD@{4}: commit: Detalha execucao no README
```

Leia de cima para baixo: o `reset` atual, antes dele o commit experimental, antes dele o commit refeito no Exercício 40. Até o commit com o `rascunho.txt` (`75824cc`) ainda está ali.

Recupere o experimento numa branch, sem mexer na `main`:

```bash
git branch resgate HEAD@{1}
git show --stat --oneline resgate
```

```text
05c4d6e Testa mensagem experimental
 src/br/com/curso/tarefas/AplicacaoGestorTarefas.java | 1 +
 1 file changed, 1 insertion(+)
```

O commit voltou a ter um nome. Como você não vai mesmo usá-lo, apague a branch:

```bash
git branch -d resgate
```

```text
error: the branch 'resgate' is not fully merged
```

O `-d` recusa porque o commit não foi integrado em lugar nenhum. É uma proteção. Como você tem certeza, use `-D`:

```bash
git branch -D resgate
```

```text
Deleted branch resgate (was 05c4d6e).
```

### O que o reflog não salva

Acrescente uma linha qualquer ao final do `README.md`, salve e **não** faça commit. Depois:

```bash
git status -s
git reset --hard
git status -s
```

A linha sumiu e não aparece no `reflog`, porque nunca foi commitada. O `reset --hard` descarta sem volta as alterações não commitadas. Por isso, antes de usá-lo, rode `git status`.

Publique os dois commits dos Exercícios 39 e 40:

```bash
git push
```

---

## Exercício 42 — Desfazer um erro já publicado com `git revert`

Simule um erro que chega ao GitHub. Troque a linha da descrição no `README.md` por:

```text
Descrição: TEXTO PROVISORIO - NAO PUBLICAR.
```

```bash
git commit -am "Atualiza descricao do projeto"
git push
```

Abra o repositório no navegador e confirme: o erro está público. A partir de agora, qualquer pessoa pode ter baixado esse commit.

### A tentação do `reset`

Tente desfazer como nos exercícios anteriores:

```bash
git reset --hard HEAD~1
git push
```

```text
 ! [rejected]        main -> main (non-fast-forward)
error: failed to push some refs to 'https://github.com/SEU_USUARIO/gestor-tarefas.git'
hint: Updates were rejected because the tip of your current branch is behind
hint: its remote counterpart. If you want to integrate the remote changes,
hint: use 'git pull' before pushing again.
```

O GitHub recusou: ele tem um commit que você tentou remover. Forçar o envio apagaria esse commit do remoto e quebraria a cópia de quem já o baixou. Na `main`, isso **não se faz**.

Volte a ficar exatamente igual ao GitHub:

```bash
git status
git reset --hard origin/main
git status -sb
```

```text
On branch main
Your branch is behind 'origin/main' by 1 commit, and can be fast-forwarded.
...
HEAD is now at a1fdfd9 Atualiza descricao do projeto
## main...origin/main
```

`git reset --hard origin/main` é o comando para "descartar o meu local e ficar igual ao remoto". Note que o `git pull --force` **não** faz isso.

### O caminho certo: um commit inverso

```bash
git revert HEAD --no-edit
git log --oneline -3
```

```text
[main d75d99b] Revert "Atualiza descricao do projeto"
 1 file changed, 1 insertion(+), 1 deletion(-)
d75d99b Revert "Atualiza descricao do projeto"
a1fdfd9 Atualiza descricao do projeto
e8260bf Detalha execucao no README
```

Veja o que o commit novo faz:

```bash
git show HEAD
```

```text
    Revert "Atualiza descricao do projeto"

    This reverts commit a1fdfd99cc192b592e477a697dbc416d6d48cb9c.
...
-Descrição: TEXTO PROVISORIO - NAO PUBLICAR.
+Descrição: Gestor de tarefas comuns e urgentes para acompanhar o trabalho de uma equipe.
```

Publique. É um `push` comum, porque nada foi apagado:

```bash
git push
```

No GitHub, o README voltou à descrição correta, e **Commits** mostra o erro e o `Revert`.

### Ponte com COBOL

Num sistema financeiro, um lançamento errado não é apagado do movimento: lança-se um **estorno**, e os dois ficam registrados. O `revert` segue a mesma intenção. O `reset` seria remover o lançamento do arquivo, aceitável apenas enquanto ninguém o processou, ou seja, enquanto o commit ainda não foi publicado.

---

## Exercício 43 — Guardar trabalho pela metade com `git stash`

Primeiro, alguém altera o projeto no GitHub. No navegador, edite o `README.md` e, na seção **Funcionalidades**, acrescente depois de `- Busca de tarefa por código`:

```text
- Contagem de tarefas por situação
```

Use a mensagem `Lista contagens nas funcionalidades` e mantenha **Commit directly to the `main` branch**.

Enquanto isso, no seu computador, você começa outra alteração. Acrescente ao final de **Próximos passos**:

```text
- Destacar tarefas atrasadas
```

Salve, **não** faça commit e tente trazer a novidade:

```bash
git status -s
git pull
```

```text
 M README.md
From https://github.com/SEU_USUARIO/gestor-tarefas
   d75d99b..b82e1d2  main       -> origin/main
Updating d75d99b..b82e1d2
error: Your local changes to the following files would be overwritten by merge:
	README.md
Please commit your changes or stash them before you merge.
Aborting
```

O Git se recusa a integrar porque a alteração que chegou mexe num arquivo que você está editando. Ele não arrisca sobrescrever o seu trabalho. A alteração ainda não está pronta para commit, então guarde-a:

```bash
git stash push -m "Proximos passos em andamento"
git stash list
git status
```

```text
Saved working directory and index state On main: Proximos passos em andamento
stash@{0}: On main: Proximos passos em andamento
On branch main
Your branch is behind 'origin/main' by 1 commit, and can be fast-forwarded.
...
nothing to commit, working tree clean
```

A pasta voltou ao último commit, e a alteração está guardada na pilha. Agora o `pull` funciona:

```bash
git pull
```

```text
Updating d75d99b..b82e1d2
Fast-forward
 README.md | 1 +
 1 file changed, 1 insertion(+)
```

Devolva a alteração guardada:

```bash
git stash pop
git stash list
git diff
```

```text
Auto-merging README.md
...
Dropped refs/stash@{0} (9380060be91380232f346a51d8940eb2346a78cc)
...
+- Destacar tarefas atrasadas
```

O Git combinou a sua alteração com a que veio do GitHub porque estão em linhas diferentes, e removeu o item da pilha (`Dropped`). `git stash list` não imprime mais nada. Conclua:

```bash
git commit -am "Planeja destaque de tarefas atrasadas"
git push
```

Se as duas alterações estivessem na mesma linha, o `pop` geraria um conflito, resolvido como no Tópico 2. Nesse caso, o item fica na pilha, e você o apaga com `git stash drop` depois de resolver.

---

## Exercício 44 — Atualizar uma branch com `rebase` e publicar com `--force-with-lease`

A branch `feature/contar-em-andamento`, do Exercício 38, nasceu antes de tudo o que a `main` recebeu depois. Vá até ela e veja o desenho:

```bash
git switch feature/contar-em-andamento
git log --oneline --graph --all -9
```

```text
* 665e28c (origin/main, main) Planeja destaque de tarefas atrasadas
* b82e1d2 Lista contagens nas funcionalidades
* d75d99b Revert "Atualiza descricao do projeto"
* a1fdfd9 Atualiza descricao do projeto
* e8260bf Detalha execucao no README
* 588350b Amplia proximos passos no README
| * d8416cf (HEAD -> feature/contar-em-andamento, origin/feature/contar-em-andamento) Implementa contagem de tarefas em andamento
|/
*   48057eb Resolve conflito na descricao do README
```

### Comparar as branches antes de integrar

```bash
git log --oneline main..feature/contar-em-andamento
git log --oneline feature/contar-em-andamento..main
```

O primeiro lista o que a feature tem e a `main` não (1 commit). O segundo, o que a `main` recebeu depois que a feature nasceu (6 commits). Agora compare o conteúdo de duas formas:

```bash
git diff --stat main feature/contar-em-andamento
git diff --stat main...feature/contar-em-andamento
```

```text
 README.md                                            |  5 -----
 src/br/com/curso/tarefas/AplicacaoGestorTarefas.java |  3 +++
 src/br/com/curso/tarefas/dominio/GestorTarefas.java  | 12 ++++++++++++
 3 files changed, 15 insertions(+), 5 deletions(-)
 src/br/com/curso/tarefas/AplicacaoGestorTarefas.java |  3 +++
 src/br/com/curso/tarefas/dominio/GestorTarefas.java  | 12 ++++++++++++
 2 files changed, 15 insertions(+)
```

Com dois argumentos, o Git compara as duas pontas: o `README.md` aparece com linhas "removidas" só porque a feature não tem o que a `main` ganhou. Com **três pontos**, ele mostra apenas o que a feature mudou desde que nasceu. Para revisar uma branch, use os três pontos.

### Reaplicar a feature sobre a `main`

```bash
git rebase main
git log --oneline --graph --all -9
```

```text
Successfully rebased and updated refs/heads/feature/contar-em-andamento.
* 3dda105 (HEAD -> feature/contar-em-andamento) Implementa contagem de tarefas em andamento
* 665e28c (origin/main, main) Planeja destaque de tarefas atrasadas
* b82e1d2 Lista contagens nas funcionalidades
...
| * d8416cf (origin/feature/contar-em-andamento) Implementa contagem de tarefas em andamento
|/
*   48057eb Resolve conflito na descricao do README
```

O seu commit foi reaplicado no topo da `main`, com um hash novo (`d8416cf` virou `3dda105`). A cópia antiga ainda aparece porque o GitHub continua com ela (`origin/feature/...`). Compile e execute: a saída deve conter `Tarefas em andamento: 3`.

### Publicar o histórico reescrito

```bash
git status
```

```text
On branch feature/contar-em-andamento
Your branch and 'origin/feature/contar-em-andamento' have diverged,
and have 7 and 1 different commits each, respectively.
  (use "git pull" if you want to integrate the remote branch with yours)
```

> **Não siga a sugestão do `git pull` aqui.** Ele juntaria o commit antigo ao novo, e a alteração apareceria duas vezes no histórico.

Tente o envio comum:

```bash
git push
```

```text
 ! [rejected]        feature/contar-em-andamento -> feature/contar-em-andamento (non-fast-forward)
```

A recusa é esperada: você reescreveu a branch. Como ela é **só sua**, substitua a versão remota com segurança:

```bash
git push --force-with-lease
git status -sb
```

```text
 + d8416cf...3dda105 feature/contar-em-andamento -> feature/contar-em-andamento (forced update)
## feature/contar-em-andamento...origin/feature/contar-em-andamento
```

O `--force-with-lease` só substitui se ninguém tiver enviado nada à branch desde a última vez que você a viu. O `--force` puro substituiria às cegas. **Nunca force a `main`.**

### Integrar e limpar

Como a feature agora começa na ponta da `main`, o merge é um simples avanço:

```bash
git switch main
git merge feature/contar-em-andamento
git push
git branch -d feature/contar-em-andamento
git push origin --delete feature/contar-em-andamento
```

```text
Updating 665e28c..3dda105
Fast-forward
 src/br/com/curso/tarefas/AplicacaoGestorTarefas.java |  3 +++
 src/br/com/curso/tarefas/dominio/GestorTarefas.java  | 12 ++++++++++++
 2 files changed, 15 insertions(+)
...
 - [deleted]         feature/contar-em-andamento
```

### Ponte com COBOL

É como pegar uma alteração feita sobre a versão de segunda-feira de um programa e reaplicá-la sobre a versão de quarta, que já recebeu outras mudanças, antes de promovê-la. O resultado é o mesmo trabalho, mas apoiado na base atual. A diferença é que o Git faz a reaplicação sozinho e só pede a sua decisão quando as alterações se sobrepõem.

---

## Exercício 45 — Resolver um conflito durante o `git pull --rebase`

No navegador, edite o `README.md` e troque a linha da descrição por:

```text
Descrição: Gestor de tarefas em Java para acompanhar o trabalho de uma equipe.
```

Use a mensagem `Simplifica descricao pelo GitHub`, direto na `main`.

No seu computador, **sem buscar nada antes**, troque **a mesma linha** por:

```text
Descrição: Gestor de tarefas comuns e urgentes, com contagem por situação.
```

```bash
git commit -am "Menciona contagens na descricao"
git push
```

```text
 ! [rejected]        main -> main (fetch first)
```

O GitHub tem um commit que você não tem. Desta vez, traga-o com rebase, para reaplicar o seu commit por cima do que chegou:

```bash
git pull --rebase
```

```text
Auto-merging README.md
CONFLICT (content): Merge conflict in README.md
error: could not apply c2b193e... Menciona contagens na descricao
hint: Resolve all conflicts manually, mark them as resolved with
hint: "git add/rm <conflicted_files>", then run "git rebase --continue".
hint: You can instead skip this commit: run "git rebase --skip".
hint: To abort and get back to the state before "git rebase", run "git rebase --abort".
```

As duas alterações estão na mesma linha. Consulte a situação:

```bash
git status
```

```text
interactive rebase in progress; onto 695ae7e
...
You are currently rebasing branch 'main' on '695ae7e'.
  (fix conflicts and then run "git rebase --continue")
  (use "git rebase --skip" to skip this patch)
  (use "git rebase --abort" to check out the original branch)

Unmerged paths:
	both modified:   README.md
```

Abra o `README.md`:

```text
<<<<<<< HEAD
Descrição: Gestor de tarefas em Java para acompanhar o trabalho de uma equipe.
=======
Descrição: Gestor de tarefas comuns e urgentes, com contagem por situação.
>>>>>>> c2b193e (Menciona contagens na descricao)
```

> **Atenção à inversão.** Num rebase, `HEAD` (o **Current Change** do VS Code) é a versão que **veio do GitHub**, e o bloco de baixo (**Incoming Change**) é o **seu** commit. É o contrário do merge do Tópico 2.

Escreva uma única linha que una as duas intenções, removendo os marcadores:

```text
Descrição: Gestor de tarefas em Java para acompanhar uma equipe, com contagem por situação.
```

Confira que não sobrou nenhum marcador e marque o arquivo como resolvido:

```bash
grep -nE '^(<<<<<<<|=======|>>>>>>>)' README.md
echo "codigo de saida: $?"
git add README.md
git rebase --continue
```

O VS Code abre a mensagem do commit (`Menciona contagens na descricao`). Mantenha-a, **feche a aba** e volte ao terminal:

```text
codigo de saida: 1
[detached HEAD 758a25e] Menciona contagens na descricao
 1 file changed, 1 insertion(+), 1 deletion(-)
Successfully rebased and updated refs/heads/main.
```

Veja o histórico e publique:

```bash
git log --oneline --graph -3
git status -sb
git push
```

```text
* 758a25e Menciona contagens na descricao
* 695ae7e Simplifica descricao pelo GitHub
* 3dda105 Implementa contagem de tarefas em andamento
## main...origin/main [ahead 1]
```

O histórico ficou linear, sem commit de merge. O `push` foi comum, sem força, porque o commit reescrito nunca tinha sido publicado.

Se você quiser desistir no meio de um rebase, `git rebase --abort` devolve tudo ao estado anterior.

---

## Exercício 46 — Juntar commits com o rebase interativo

Acrescente ao **final** do `README.md`, com um erro de digitação no título:

```text

## Requisito

- Git Bash
```

```bash
git commit -am "Adiciona secao de requisitos"
```

Acrescente uma linha à lista:

```text
- JDK 21
```

```bash
git commit -am "Inclui JDK 21 nos requisitos"
```

Corrija o título para `## Requisitos`:

```bash
git commit -am "Corrige titulo da secao de requisitos"
git log --oneline -4
git status -sb
```

```text
41df906 Corrige titulo da secao de requisitos
c1357ee Inclui JDK 21 nos requisitos
1cf5082 Adiciona secao de requisitos
758a25e Menciona contagens na descricao
## main...origin/main [ahead 3]
```

Três commits para uma única seção. Como **nenhum foi publicado** (`ahead 3`), você pode juntá-los:

```bash
git rebase -i HEAD~3
```

O VS Code abre a lista, do commit mais antigo para o mais recente. Dependendo da versão do Git, aparece um `#` antes da mensagem:

```text
pick 1cf5082 Adiciona secao de requisitos
pick c1357ee Inclui JDK 21 nos requisitos
pick 41df906 Corrige titulo da secao de requisitos
```

Se a extensão **GitLens** estiver instalada, ela pode mostrar uma tela própria com menus no lugar do texto. Nesse caso, escolha `squash` nos menus das duas últimas linhas e clique em **Start Rebase**. Sem a extensão, edite o texto.

Troque `pick` por `squash` nas **duas últimas linhas**, salve e **feche a aba**:

```text
pick 1cf5082 Adiciona secao de requisitos
squash c1357ee Inclui JDK 21 nos requisitos
squash 41df906 Corrige titulo da secao de requisitos
```

O Git abre uma segunda aba com as três mensagens. Apague todas as linhas que não começam com `#` e escreva só:

```text
Documenta requisitos do projeto
```

Salve e feche a aba. Confira:

```bash
git log --oneline -3
git show --stat --oneline HEAD
```

```text
Successfully rebased and updated refs/heads/main.
23be84b Documenta requisitos do projeto
758a25e Menciona contagens na descricao
695ae7e Simplifica descricao pelo GitHub
23be84b Documenta requisitos do projeto
 README.md | 5 +++++
 1 file changed, 5 insertions(+)
```

O erro de digitação desapareceu do histórico, e a seção entra num único commit. Publique:

```bash
git push
```

| Palavra | Efeito no rebase interativo |
|---|---|
| `pick` | mantém o commit |
| `reword` | mantém e permite trocar a mensagem |
| `squash` | junta ao commit anterior e combina as mensagens |
| `fixup` | junta ao commit anterior e descarta a mensagem |
| `drop` | remove o commit |

Se algo sair do previsto durante o rebase, `git rebase --abort` cancela tudo.

---

## Exercício 47 — Copiar um único commit com `git cherry-pick`

Crie uma branch de experimentos:

```bash
git switch -c experimentos
```

Primeiro experimento, que **não** deve ir para a `main`. Em `AplicacaoGestorTarefas.java`, insira logo acima de `System.out.println("=== GESTOR DE TAREFAS ===");`:

```java
        System.out.println("DEBUG: inicio da aplicacao");
```

```bash
git commit -am "Adiciona mensagem de depuracao"
```

Segundo experimento, que ficou bom. Em `GestorTarefas.java`, inclua logo antes de `exibirTarefasDoResponsavel`:

```java
    public int contarTarefasDoResponsavel(String matricula) {
        int quantidade = 0;

        for (Tarefa tarefa : tarefas) {
            if (tarefa.possuiResponsavel(matricula)) {
                quantidade++;
            }
        }

        return quantidade;
    }
```

Compile para garantir que não há erros e registre:

```bash
git commit -am "Implementa contagem de tarefas por responsavel"
git switch main
git log --oneline main..experimentos
```

```text
bda5db5 Implementa contagem de tarefas por responsavel
fee249b Adiciona mensagem de depuracao
```

Fazer merge de `experimentos` traria os dois commits. Copie só o segundo, usando o **hash que aparece na sua tela**:

```bash
git cherry-pick bda5db5
git log --oneline --graph --all -4
```

```text
[main 85d2f4b] Implementa contagem de tarefas por responsavel
 1 file changed, 12 insertions(+)
* bda5db5 (experimentos) Implementa contagem de tarefas por responsavel
* fee249b Adiciona mensagem de depuracao
| * 85d2f4b (HEAD -> main) Implementa contagem de tarefas por responsavel
|/
* 23be84b (origin/main) Documenta requisitos do projeto
```

O commit na `main` tem outro hash (`85d2f4b`), mas a mesma alteração e a mesma mensagem. Confira no código: `GestorTarefas.java` tem `contarTarefasDoResponsavel`, e `AplicacaoGestorTarefas.java` **não** tem a linha `DEBUG`. Compile, execute e publique:

```bash
git push
```

Descarte a branch de experimentos. Como ela tem um commit nunca integrado, o `-d` recusa, e é preciso o `-D`:

```bash
git branch -d experimentos
git branch -D experimentos
```

```text
error: the branch 'experimentos' is not fully merged
Deleted branch experimentos (was bda5db5).
```

---

## Exercício 48 — Limpar as branches e conferir o resultado

No Tópico 2, a branch `feature/contar-pendentes` ficou no GitHub. Remova-a e atualize as referências locais:

```bash
git branch -a
git push origin --delete feature/contar-pendentes
git fetch --prune
git branch -a
```

```text
* main
  remotes/origin/HEAD -> origin/main
  remotes/origin/feature/contar-pendentes
  remotes/origin/main
To https://github.com/SEU_USUARIO/gestor-tarefas.git
 - [deleted]         feature/contar-pendentes
* main
  remotes/origin/HEAD -> origin/main
  remotes/origin/main
```

Execute a conferência, trocando `PONTO_DE_PARTIDA` pelo hash anotado no Exercício 34:

```bash
git status -sb
git stash list
git log --oneline PONTO_DE_PARTIDA..main
git log --oneline PONTO_DE_PARTIDA..main | wc -l
git ls-remote --heads origin
```

Resultado esperado:

```text
## main...origin/main
85d2f4b Implementa contagem de tarefas por responsavel
23be84b Documenta requisitos do projeto
758a25e Menciona contagens na descricao
695ae7e Simplifica descricao pelo GitHub
3dda105 Implementa contagem de tarefas em andamento
665e28c Planeja destaque de tarefas atrasadas
b82e1d2 Lista contagens nas funcionalidades
d75d99b Revert "Atualiza descricao do projeto"
a1fdfd9 Atualiza descricao do projeto
e8260bf Detalha execucao no README
588350b Amplia proximos passos no README
11
85d2f4bc2c102c7e1da93b58a433038c8914de1d	refs/heads/main
```

`git stash list` não imprime nada, e `git ls-remote` lista só a `main`, com o mesmo hash do primeiro commit da lista.

### Checklist

**Comparar e consultar**

- [ ] Você usou `HEAD~N` e `--first-parent` para apontar para commits.
- [ ] Você consultou uma versão antiga com `git show commit:README.md`.
- [ ] Você sabe a diferença entre `git diff`, `git diff --staged` e `git diff HEAD`.
- [ ] Você sabe por que `git diff main...feature` mostra só o trabalho da feature.

**Desfazer**

- [ ] O histórico **não** contém `Testa mensagem experimental`, `Adiciona mensagem de depuracao` nem a mensagem com `andamnto`.
- [ ] Os commits `Planeja exportacao...` e `Planeja relatorio...` viraram `Amplia proximos passos no README`.
- [ ] `git ls-files` **não** lista `rascunho.txt`.
- [ ] Você recuperou um commit pelo `git reflog` e sabe que alterações nunca commitadas não aparecem nele.
- [ ] O histórico contém `Atualiza descricao do projeto` e o `Revert` correspondente.
- [ ] Você sabe por que o `reset` foi recusado na `main` publicada e por que o `revert` não foi.

**Reorganizar**

- [ ] Você usou `git stash push` e `git stash pop` para conseguir dar `pull`.
- [ ] `Implementa contagem de tarefas em andamento` aparece depois de `Planeja destaque de tarefas atrasadas`, sem commit de merge.
- [ ] Você publicou a branch reescrita com `--force-with-lease` e nunca forçou a `main`.
- [ ] Você resolveu o conflito do `pull --rebase` com `git add` e `git rebase --continue`.
- [ ] A seção **Requisitos** entrou num único commit, `Documenta requisitos do projeto`.
- [ ] `contarTarefasDoResponsavel` está na `main` e a linha `DEBUG` não está.

**Estado final**

- [ ] `git status -sb` mostra `## main...origin/main`, sem arquivos pendentes.
- [ ] `git log --oneline PONTO_DE_PARTIDA..main | wc -l` imprime `11`.
- [ ] O GitHub tem apenas a branch `main`.
- [ ] O programa compila e exibe `Tarefas pendentes: 3` e `Tarefas em andamento: 3`.

---

## Entrega

Entregue:

1. o **endereço do repositório** no GitHub, o mesmo do Tópico 2:

   ```text
   https://github.com/SEU_USUARIO/gestor-tarefas
   ```

2. a saída de `git log --oneline PONTO_DE_PARTIDA..main`, copiada do terminal.

Não envie senha, token ou código de acesso. Antes de entregar, abra o endereço e confirme:

- a descrição do `README.md` é a resolvida no Exercício 45;
- o `README.md` tem a seção **Requisitos**;
- em **Commits**, aparecem o `Revert` e os onze commits deste tópico;
- a lista de branches tem apenas `main`.

---

## Comandos de referência

| Objetivo | Comando |
|---|---|
| Listar commits na ordem do `~` | `git log --oneline --first-parent` |
| Ver um commit | `git show HASH` |
| Ver um arquivo numa versão antiga | `git show HASH:caminho/arquivo` |
| Comparar dois commits | `git diff A B` |
| Comparar só um arquivo | `git diff A B -- arquivo` |
| Resumir a comparação | `git diff --stat A B` |
| Diferença palavra a palavra | `git diff --word-diff A B` |
| Trabalho × último commit | `git diff HEAD` |
| Commits da feature que faltam na `main` | `git log --oneline main..feature` |
| O que a feature mudou desde que nasceu | `git diff main...feature` |
| Tirar da preparação | `git restore --staged arquivo` |
| Descartar alteração não commitada | `git restore arquivo` |
| Corrigir a mensagem do último commit | `git commit --amend -m "mensagem"` |
| Incluir arquivo no último commit | `git add arquivo` e `git commit --amend --no-edit` |
| Desfazer commits mantendo tudo preparado | `git reset --soft HEAD~N` |
| Desfazer commits mantendo os arquivos | `git reset HEAD~N` |
| Desfazer commits e descartar alterações | `git reset --hard HEAD~N` |
| Ficar igual ao remoto | `git fetch` e `git reset --hard origin/main` |
| Ver os movimentos do `HEAD` | `git reflog` |
| Recuperar um commit numa branch | `git branch nome HEAD@{N}` |
| Desfazer um commit publicado | `git revert HASH --no-edit` |
| Guardar trabalho pela metade | `git stash push -m "descricao"` |
| Listar e recuperar | `git stash list` e `git stash pop` |
| Reaplicar a branch atual sobre a `main` | `git rebase main` |
| Trazer o remoto reaplicando os commits locais | `git pull --rebase` |
| Continuar ou desistir de um rebase | `git rebase --continue` ou `git rebase --abort` |
| Publicar uma branch sua reescrita | `git push --force-with-lease` |
| Reorganizar os últimos commits | `git rebase -i HEAD~N` |
| Copiar um commit para a branch atual | `git cherry-pick HASH` |
| Apagar branch não integrada | `git branch -D nome` |
| Apagar branch remota | `git push origin --delete nome` |
| Limpar referências remotas apagadas | `git fetch --prune` |

---

## Síntese final do tópico

No Tópico 2, o histórico do `gestor-tarefas` só cresceu. Neste, você aprendeu a lê-lo com precisão e a ajustá-lo com critério:

- `HEAD~N`, `git show` e as variações de `git diff` respondem "o que mudou, onde e quando";
- `restore` desfaz o que não foi commitado; `amend` e `reset` desfazem o que ainda é só seu;
- o `reflog` guarda os movimentos do `HEAD` e permite recuperar commits desfeitos;
- `revert` corrige o que já foi publicado sem apagar nada;
- `stash` pausa um trabalho para dar lugar a outro;
- `rebase` e `pull --rebase` reaplicam commits sobre uma nova base, com histórico linear;
- `--force-with-lease` publica uma branch sua reescrita sem apagar, às cegas, o trabalho de outra pessoa;
- o rebase interativo e o `cherry-pick` dão controle fino sobre quais commits ficam e como.

A regra de ouro resume o tópico: **reescreva só o que ainda é seu; corrija o que já é de todos com um commit novo.** No próximo tópico, você levará essas ferramentas para o trabalho em trio, num projeto novo com Maven.
