# Tópico 3 — Comparar, desfazer e reorganizar o histórico

## Do histórico que só cresce ao histórico que você controla

Nos tópicos anteriores, você registrou versões com `git add` e `git commit`, publicou o projeto no GitHub, sincronizou com `fetch`, `pull` e `push`, trabalhou em branches e resolveu conflitos com `merge`. Em todos esses passos, o histórico só cresceu: cada comando acrescentava commits.

No dia a dia, porém, surgem situações em que é preciso **voltar atrás** ou **arrumar** o que já foi feito:

- você quer ver exatamente o que mudou entre duas versões, e não só entre o arquivo e o último commit;
- alterou um arquivo, se arrependeu e quer a versão anterior;
- fez um commit com a mensagem errada ou esqueceu um arquivo;
- registrou dois commits que deveriam ser um só;
- publicou um erro na `main` e precisa desfazê-lo sem atrapalhar quem já baixou;
- está no meio de uma alteração e precisa trazer uma novidade do GitHub;
- sua branch ficou para trás em relação à `main` e você quer atualizá-la sem um commit de merge;
- apagou um commit sem querer e quer recuperá-lo.

Neste tópico, você continuará com o projeto **Gestão de Biblioteca** (`gestao-biblioteca`) do Tópico 2, já publicado no GitHub. O objetivo não é decorar comandos, mas entender **o que cada um faz com as três áreas e com o histórico**. Com essa clareza, você consegue escolher o comando certo e, sobretudo, evitar o comando perigoso.

## O que você fará neste tópico

- apontar para qualquer commit com hash, `HEAD`, `HEAD~N` e nomes de branch;
- comparar versões com as variações de `git diff` e consultar versões antigas com `git show`;
- descartar alterações com `git restore`;
- corrigir o último commit com `git commit --amend`;
- mover a branch para trás com `git reset --soft`, `--mixed` e `--hard`;
- recuperar commits "perdidos" com `git reflog`;
- desfazer um commit já publicado com `git revert`;
- guardar trabalho pela metade com `git stash`;
- reaplicar commits sobre outra base com `git rebase` e `git pull --rebase`;
- publicar um histórico reescrito com `git push --force-with-lease`;
- reorganizar commits com o rebase interativo;
- copiar um commit específico com `git cherry-pick`.

> Pull Requests, revisão de código e Maven ficam para o próximo tópico.

Um fio conduz o tópico inteiro, e ele aparece explicado na seção 8: **o que ainda não foi publicado pode ser reescrito; o que já foi publicado se corrige com um commit novo.**

---

## 1. Como apontar para um commit

Para comparar ou desfazer, primeiro você precisa dizer ao Git **de qual commit** está falando. Existem várias formas.

| Forma | Significado | Exemplo |
|---|---|---|
| hash | identificador do commit; bastam os 7 primeiros caracteres | `a95d4a0` |
| `HEAD` | o commit em que você está agora | `HEAD` |
| `HEAD~1` | o commit anterior ao `HEAD` (o "pai") | `HEAD~1` |
| `HEAD~3` | três commits antes do `HEAD` | `HEAD~3` |
| nome de branch | o último commit daquela branch | `main`, `feature/obter-titulo` |
| branch remota | o último estado conhecido da branch no remoto | `origin/main` |

`HEAD` é um ponteiro: ele indica a branch atual, que por sua vez indica o seu último commit. Quando você faz um commit, a branch avança e o `HEAD` vai junto.

```bash
git log --oneline -4
```

```text
0be1c63 (HEAD -> main, origin/main) Resolve conflito na descricao do projeto
1f870a4 Descreve o foco em leitores e devolucoes
a95d4a0 Adiciona obterTitulo em Livro
a4cf836 Update README.md
```

### Commits de merge e o primeiro pai

Um commit de merge tem **dois pais**. O `~` segue sempre o **primeiro**, que é a linha da branch que recebeu o merge. Por isso, depois de um merge, `HEAD~1` pode não ser a segunda linha de `git log --oneline`, que mistura os commits das duas branches. Para ver a lista na mesma ordem do `~`, use:

```bash
git log --oneline --first-parent -4
```

### Ver um commit e uma versão antiga de arquivo

`git show` exibe um commit: autor, data, mensagem e a diferença que ele introduziu.

```bash
git show HEAD~1
git show --stat HEAD~1
```

Com `:` e um caminho, ele mostra **o conteúdo de um arquivo naquela versão**, sem alterar nada na sua pasta:

```bash
git show HEAD~3:README.md
```

Esse é o jeito mais seguro de consultar o passado: só leitura.

---

## 2. Comparar versões: as variações de `git diff`

No Tópico 1, você usou `git diff` e `git diff --staged`. Elas comparam as três áreas entre si. O mesmo comando também compara commits e branches.

### As três comparações do dia a dia

```text
diretório de trabalho ──► área de preparação ──► último commit (HEAD)
        └──── git diff ────┘        └── git diff --staged ──┘
        └──────────────────── git diff HEAD ────────────────────┘
```

| Comando | Compara | Responde |
|---|---|---|
| `git diff` | diretório de trabalho × área de preparação | o que mudei e ainda não preparei? |
| `git diff --staged` | área de preparação × último commit | o que vai entrar no próximo commit? |
| `git diff HEAD` | diretório de trabalho × último commit | o que mudei desde o último commit, preparado ou não? |

### Comparar commits

Com dois commits, o `diff` mostra o que mudou de um para o outro:

```bash
git diff HEAD~3 HEAD
git diff a4cf836 0be1c63
```

A ordem importa: o primeiro é o "antes" (linhas com `-`) e o segundo, o "depois" (linhas com `+`). Para limitar a comparação a um arquivo, acrescente `--` e o caminho:

```bash
git diff HEAD~3 HEAD -- README.md
```

### Resumir a diferença

Em uma comparação grande, comece pelo resumo:

| Opção | Mostra |
|---|---|
| `--stat` | arquivos alterados e a quantidade de linhas acrescentadas e removidas |
| `--name-only` | só os nomes dos arquivos alterados |
| `--word-diff` | a diferença palavra a palavra, útil em textos longos |

```bash
git diff --stat HEAD~3 HEAD
```

```text
 README.md                                       |  2 +-
 src/br/com/curso/biblioteca/dominio/Livro.java  |  4 ++++
 2 files changed, 5 insertions(+), 1 deletion(-)
```

Com `--word-diff`, em vez de uma linha inteira removida e outra acrescentada, o Git marca só as palavras trocadas: `[-antigas-]{+novas+}`.

### Comparar branches: dois pontos e três pontos

Ao comparar branches, duas perguntas diferentes aparecem com frequência:

| Comando | Pergunta |
|---|---|
| `git log --oneline main..feature/x` | quais commits a feature tem e a `main` ainda não? |
| `git log --oneline feature/x..main` | quais commits a `main` recebeu depois que a feature nasceu? |
| `git diff main feature/x` | qual a diferença entre as duas pontas, agora? |
| `git diff main...feature/x` | o que a feature mudou desde que saiu da `main`? |

A diferença entre as duas formas de `diff` engana. Com dois argumentos, o Git compara as duas pontas e mostra também, ao contrário, tudo o que a `main` recebeu depois. Com **três pontos**, ele compara a feature com o ponto em que ela nasceu, e mostra só o trabalho da branch. Para revisar uma feature, use os três pontos.

### Comparar com o remoto

Depois de um `git fetch`, `origin/main` representa o estado do GitHub. Assim você revisa o que chegou antes de integrar:

```bash
git fetch
git log --oneline main..origin/main
git diff main origin/main
```

### O histórico de um arquivo

```bash
git log --oneline -- README.md
git log -p -- README.md
```

O primeiro lista os commits que alteraram o arquivo. O segundo (`-p`, de *patch*) mostra também a diferença de cada um.

---

## 3. Desfazer antes do commit: `git restore`

Se a alteração ainda não virou commit, desfazê-la é simples. O próprio `git status` sugere os comandos.

| Situação | Comando | Efeito |
|---|---|---|
| preparei com `git add` e quero tirar da preparação | `git restore --staged arquivo` | o arquivo volta para "não preparado"; a alteração continua no arquivo |
| quero descartar a alteração do arquivo | `git restore arquivo` | o arquivo volta a ser igual ao da área de preparação, ou seja, igual ao último commit se nada estiver preparado |
| quero trazer o arquivo como era em outro commit | `git restore --source=HEAD~2 arquivo` | o arquivo da pasta fica igual ao daquele commit; depois, você decide se faz commit |

Use os dois primeiros juntos para descartar tudo de um arquivo já preparado: primeiro `--staged`, depois sem a opção.

> **Atenção.** `git restore arquivo` descarta a alteração **sem volta**. O que nunca foi commitado não fica guardado em lugar nenhum, nem no `reflog`, que você verá adiante.

Tutoriais antigos usam `git checkout -- arquivo` e `git reset HEAD arquivo` para essas mesmas tarefas. O `git restore` foi criado, assim como o `git switch`, para separar ações que o `checkout` acumulava.

---

## 4. Corrigir o último commit: `git commit --amend`

Dois erros acontecem logo depois de um commit: a mensagem saiu errada ou faltou um arquivo. O `--amend` **substitui o último commit** por um novo, com a correção.

Para corrigir só a mensagem:

```bash
git commit --amend -m "Adiciona obterAutor em Livro"
```

Para incluir um arquivo esquecido, mantendo a mensagem:

```bash
git add src/br/com/curso/biblioteca/AplicacaoBiblioteca.java
git commit --amend --no-edit
```

`--no-edit` aproveita a mensagem do commit anterior, sem abrir o editor.

O detalhe importante é que o commit corrigido **tem outro hash**. O Git não edita commits: ele cria um novo e faz a branch apontar para ele. O antigo deixa de fazer parte da branch.

```text
antes:   a95d4a0 ── 3c1e2f9  (main)   "Adiciona obterAtor em Livro"
depois:  a95d4a0 ── 8d04b7a  (main)   "Adiciona obterAutor em Livro"
```

Se o commit antigo já estava no GitHub, o remoto ainda o tem, e o próximo `push` será recusado. Por isso, **use `--amend` apenas em commits que ainda não foram publicados**. A seção 8 explica o porquê.

---

## 5. Mover a branch para trás: `git reset`

O `git reset` faz a branch atual **apontar para outro commit**, normalmente um anterior. Os commits que ficaram depois dele deixam de fazer parte da branch. A diferença entre os modos está no que acontece com a área de preparação e com os seus arquivos.

```bash
git reset --soft HEAD~1
git reset HEAD~1           # o mesmo que --mixed, o modo padrão
git reset --hard HEAD~1
```

| Modo | Branch | Área de preparação | Arquivos da pasta | Resultado prático |
|---|---|---|---|---|
| `--soft` | volta | mantém | mantém | as alterações dos commits desfeitos ficam **preparadas**, prontas para um novo commit |
| `--mixed` | volta | volta | mantém | as alterações ficam nos arquivos, **não preparadas** |
| `--hard` | volta | volta | **volta** | as alterações **somem** da pasta |

Pense no `reset` como uma pergunta: até onde o "voltar" chega? O `--soft` volta só a branch, o `--mixed` volta também a preparação e o `--hard` volta tudo.

### Quando usar cada um

**`--soft`: juntar commits.** Você fez dois commits pequenos que deveriam ser um. `git reset --soft HEAD~2` desfaz os dois, mas mantém tudo preparado. Um único `git commit` registra a soma.

**`--mixed`: refazer a separação.** Você fez um commit que misturou duas intenções ou incluiu um arquivo que não devia. `git reset HEAD~1` desfaz o commit e deixa as alterações nos arquivos. Então você prepara e registra só o que deve entrar.

**`--hard`: descartar.** Você fez um experimento, registrou e decidiu que não quer nada dele. `git reset --hard HEAD~1` faz a branch, a preparação e os arquivos voltarem ao commit anterior.

> **Cuidado com o `--hard`.** Ele também descarta as alterações **ainda não commitadas** dos arquivos rastreados, e essas não podem ser recuperadas. Antes de usá-lo, rode `git status`: se houver algo modificado que você queira, faça commit ou guarde com `git stash` (seção 9). Arquivos nunca adicionados ao Git (*untracked*) não são apagados pelo `reset --hard`.

### `reset` × `restore`

Os dois "desfazem", mas em níveis diferentes. O `restore` age sobre **arquivos** e não mexe no histórico. O `reset` move a **branch** e, por isso, muda o histórico da branch. Se a dúvida for "quero o arquivo como estava", pense em `restore`. Se for "quero que a branch volte a um commit anterior", pense em `reset`.

---

## 6. A rede de segurança: `git reflog`

Depois de um `reset --hard`, o commit desfeito some do `git log`. Mas ele não foi apagado: o Git ainda o guarda, e o **reflog** registra cada posição pela qual o `HEAD` passou.

```bash
git reflog -5
```

```text
a95d4a0 HEAD@{0}: reset: moving to HEAD~1
7c2d9e1 HEAD@{1}: commit: Testa listagem experimental
a95d4a0 HEAD@{2}: commit: Adiciona obterTitulo em Livro
a4cf836 HEAD@{3}: pull: Fast-forward
5b67b8b HEAD@{4}: commit: Lista funcionalidades no README
```

Leia de cima para baixo, do mais recente ao mais antigo. `HEAD@{1}` é "onde o `HEAD` estava um movimento atrás", que é justamente o commit desfeito. Para recuperá-lo, há dois caminhos:

```bash
git branch resgate HEAD@{1}     # cria uma branch apontando para o commit
git reset --hard 7c2d9e1        # ou faz a branch atual voltar para ele
```

Criar uma branch é o caminho mais calmo: o commit volta a ter um nome, e você decide com tranquilidade o que fazer com ele.

Limites do reflog:

- ele é **local**: existe só no seu computador e não vai para o GitHub;
- ele guarda **commits**, nunca alterações que você descartou sem commitar;
- as entradas expiram depois de algumas semanas (o padrão fica entre 30 e 90 dias).

Por isso, um hábito simples protege muito: **antes de um comando arriscado, faça commit**. O que foi commitado quase sempre pode ser recuperado.

---

## 7. Desfazer algo já publicado: `git revert`

Imagine que um commit com erro já foi para a `main` do GitHub e outras pessoas já o baixaram. Usar `reset` agora apagaria um commit que existe no computador delas também. O caminho seguro é outro: o `git revert` cria **um commit novo que faz o inverso** do commit escolhido.

```bash
git revert HEAD --no-edit
```

```text
[main 4e7a1b2] Revert "Atualiza descricao do projeto"
 1 file changed, 1 insertion(+), 1 deletion(-)
```

Sem `--no-edit`, o Git abre o editor com a mensagem proposta, para você confirmar ou explicar o motivo. O histórico passa a ter os dois commits, o do erro e o da correção:

```text
4e7a1b2 Revert "Atualiza descricao do projeto"
9b3f0c8 Atualiza descricao do projeto
0be1c63 Resolve conflito na descricao do projeto
```

Nada foi apagado. Por isso o `revert` não exige nenhum cuidado especial no `push`: é um commit como outro qualquer.

O `revert` pode desfazer qualquer commit, não só o último: `git revert 9b3f0c8`. Se commits posteriores mexeram nas mesmas linhas, pode surgir um conflito, resolvido como você já sabe. Desfazer um commit de **merge** exige a opção `-m 1` e tem consequências para merges futuros. Isso fica fora deste tópico.

### Ponte com COBOL

Quem trabalha com sistemas financeiros conhece o **estorno**: um lançamento errado não é apagado do movimento, e sim anulado por um lançamento inverso, e os dois ficam registrados. O `revert` segue a mesma intenção: o erro e a correção ficam no histórico, e quem já tinha a versão anterior não perde a referência. O `reset`, ao contrário, seria como remover o lançamento do arquivo, aceitável enquanto o movimento ainda não foi processado por ninguém.

---

## 8. A regra de ouro: publicado ou não publicado

`--amend`, `reset` e o `rebase` da seção 10 **reescrevem o histórico**: trocam commits por outros, com hashes novos. Enquanto esses commits só existem no seu computador, ninguém é afetado. Depois do `push`, outras pessoas podem ter baixado os commits antigos e construído trabalho em cima deles.

| Situação | Ferramentas adequadas |
|---|---|
| commit **ainda não publicado** | `commit --amend`, `reset`, `rebase`, `rebase -i` |
| commit **já publicado** em branch compartilhada, como a `main` | `revert` |
| alteração ainda **não commitada** | `restore`, `stash` |

Para saber se um commit já foi publicado, use `git status -sb` ou `git log --oneline --decorate`. Commits **depois** de `origin/main` (o `ahead`) ainda são só seus.

### O que acontece se você reescrever algo publicado

Se você der `reset` em um commit que já estava no GitHub e tentar enviar, o Git recusa:

```text
 ! [rejected]        main -> main (non-fast-forward)
error: failed to push some refs to 'https://github.com/SEU-USUARIO/gestao-biblioteca.git'
hint: Updates were rejected because the tip of your current branch is behind
hint: its remote counterpart.
```

O remoto está protegendo os commits que você tentou remover. A recusa é uma ajuda, não um obstáculo.

### Ficar igual ao remoto

Às vezes você quer o contrário: **descartar o seu local e ficar exatamente como o GitHub**. Por exemplo, depois de um experimento que deu errado ou de um `reset` indevido como o acima. O caminho é buscar e mover a branch para `origin/main`:

```bash
git fetch
git reset --hard origin/main
```

Esse comando descarta commits locais não publicados e alterações não commitadas. Use-o sabendo disso.

### E o `git pull --force`?

O nome engana. A opção existe, mas apenas repassa o `--force` ao `fetch` interno, para casos raros de atualização de referências. Ela **não** substitui os seus arquivos pelos do remoto nem descarta commits locais. Para "ficar igual ao remoto", use `git fetch` seguido de `git reset --hard origin/main`.

---

## 9. Guardar trabalho pela metade: `git stash`

Você está no meio de uma alteração, sem nada pronto para commit, e precisa fazer outra coisa: trazer uma novidade do GitHub, trocar de branch ou corrigir algo urgente. Às vezes o Git até recusa:

```text
error: Your local changes to the following files would be overwritten by merge:
	README.md
Please commit your changes or stash them before you merge.
Aborting
```

O `git stash` guarda as alterações não commitadas numa pilha e devolve a pasta ao estado do último commit.

| Comando | Efeito |
|---|---|
| `git stash push -m "descricao"` | guarda as alterações dos arquivos rastreados, com uma descrição |
| `git stash push -u -m "descricao"` | inclui também arquivos novos, ainda não rastreados |
| `git stash list` | lista o que está guardado (`stash@{0}` é o mais recente) |
| `git stash show -p` | mostra a diferença guardada |
| `git stash pop` | reaplica o mais recente e o remove da pilha |
| `git stash apply` | reaplica, mas mantém na pilha |
| `git stash drop` | descarta o mais recente |

Um fluxo típico:

```bash
git stash push -m "Listagem por autor em andamento"
git pull
git stash pop
```

Se a alteração guardada e a que chegou tocarem as mesmas linhas, o `pop` gera um conflito, resolvido como de costume. Nesse caso, o Git mantém o item na pilha. Depois de resolver, apague-o com `git stash drop`.

O stash é para pausas curtas. Ele fica só no seu computador e não tem o contexto de um commit. Se a pausa for longa, prefira um commit numa branch.

---

## 10. Reaplicar commits sobre outra base: `git rebase`

No Tópico 2, quando a `main` e a sua branch avançaram ao mesmo tempo, o `merge` uniu as duas linhas com um commit de merge. O **rebase** resolve a mesma situação de outra forma: ele **reaplica os commits da sua branch, um a um, sobre a ponta atual da outra branch**.

```text
antes do rebase:

      C ── D   feature/obter-autor
     /
A ── B ── E ── F   main

git switch feature/obter-autor
git rebase main

depois do rebase:

A ── B ── E ── F   main
                \
                 C' ── D'   feature/obter-autor
```

`C'` e `D'` trazem as mesmas alterações de `C` e `D`, mas são commits novos, com hashes novos, porque agora têm outro pai. O histórico fica linear, como se você tivesse começado a feature depois de `F`. Integrar na `main` vira um simples *fast-forward*:

```bash
git switch main
git merge feature/obter-autor
```

### Merge ou rebase?

| | `merge` | `rebase` |
|---|---|---|
| histórico | preserva as duas linhas e cria um commit de merge | fica linear, sem commit de merge |
| commits existentes | não mudam | são recriados, com hashes novos |
| risco | nenhum para quem já baixou | reescreve histórico: só para commits não compartilhados |
| conflitos | resolvidos uma vez, no commit de merge | podem aparecer a cada commit reaplicado |

Nenhum dos dois é "o certo". Cada equipe combina o seu fluxo. Neste curso, o padrão continua sendo o merge, como você configurou no Tópico 2 com `pull.rebase false`. O rebase é a ferramenta para **arrumar a sua branch antes de compartilhá-la ou integrá-la**.

### `git pull --rebase`

Você fez um commit local, e alguém publicou outro na mesma branch. O `push` é recusado com `fetch first`. Um `git pull` comum resolveria com um commit de merge. Já o `git pull --rebase` busca o remoto e **reaplica o seu commit local sobre o que chegou**:

```bash
git pull --rebase
git push
```

Como o seu commit nunca tinha sido publicado, reescrevê-lo não afeta ninguém, e o `push` seguinte é comum, sem força. A opção na linha de comando vale só para aquela execução e não altera a configuração `pull.rebase false`.

### Conflitos durante o rebase

Se um commit reaplicado tocar as mesmas linhas que a nova base, o rebase **para** naquele commit:

```text
CONFLICT (content): Merge conflict in README.md
error: could not apply 1d9c3a7... Menciona autores na descricao
hint: Resolve all conflicts manually, mark them as resolved with
hint: "git add/rm <conflicted_files>", then run "git rebase --continue".
```

A resolução é a mesma do Tópico 2: abrir o arquivo, decidir, remover os marcadores e usar `git add`. A conclusão é diferente: em vez de `git commit`, use `git rebase --continue`. O Git pode abrir o editor com a mensagem do commit para você confirmar.

| Comando | Quando usar |
|---|---|
| `git rebase --continue` | resolveu o conflito e preparou os arquivos |
| `git rebase --abort` | quer desistir e voltar ao estado anterior ao rebase |
| `git rebase --skip` | quer descartar o commit que está sendo aplicado (raro) |

> **"Current" e "Incoming" se invertem.** Num merge, `HEAD` é a sua branch. Num rebase, o Git está montando a branch nova sobre a outra base. Por isso, `HEAD`, que o VS Code chama de **Current Change**, é a versão **da base** (o que veio do remoto ou da `main`), e **Incoming Change** é o **seu** commit. Leia o conteúdo antes de clicar.

### Ponte com COBOL

Imagine que você alterou um programa a partir da versão de segunda-feira, mas, antes de promovê-lo, outra mudança entrou em produção na quarta. Uma forma de trabalhar é reaplicar a sua alteração sobre a versão de quarta, como se tivesse começado dela, e só então promover. O rebase faz isso com os commits. A diferença é que o Git reaplica automaticamente e só pede a sua decisão quando as alterações se sobrepõem.

---

## 11. Publicar um histórico reescrito: `--force-with-lease`

Às vezes reescrever uma branch já publicada é legítimo. O caso típico é uma **feature branch que só você usa**: você a publicou como cópia de segurança e depois fez rebase na `main` atualizada. O `push` comum é recusado, porque o remoto tem os commits antigos e você, os novos:

```text
 ! [rejected]        feature/obter-autor -> feature/obter-autor (non-fast-forward)
```

Para substituir a branch remota pela sua, é preciso **forçar** o envio. Existem duas opções, e a diferença é importante:

| Comando | Comportamento |
|---|---|
| `git push --force` | substitui a branch remota **sempre**, mesmo que alguém tenha enviado commits que você não viu |
| `git push --force-with-lease` | substitui **só se** a branch remota ainda estiver onde você a viu pela última vez; se alguém enviou algo, recusa |

```bash
git push --force-with-lease
```

```text
 + 5e2a9c1...b71d044 feature/obter-autor -> feature/obter-autor (forced update)
```

O `+` e o `(forced update)` indicam que o histórico remoto foi substituído. Prefira sempre o `--force-with-lease`: ele força o envio, mas não apaga, sem aviso, o trabalho de outra pessoa.

Regras de uso:

- **nunca force a `main`** nem qualquer branch que outras pessoas usam;
- force apenas uma branch sua, e só quando souber que ninguém trabalha nela;
- **depois de um rebase, não rode `git pull` na branch.** O `git status` dirá que a branch e a remota "divergiram" e sugerirá `git pull`. Se você obedecer, o Git juntará os commits antigos com os novos, e cada alteração aparecerá duas vezes. O passo correto é o `push --force-with-lease`.

---

## 12. Reorganizar commits: o rebase interativo

Antes de compartilhar uma branch, é comum querer arrumar a sequência de commits: juntar um "corrige erro de digitação" ao commit que ele corrige, reescrever uma mensagem ou remover um commit de teste. O **rebase interativo** permite editar os últimos commits como uma lista.

### Configurar o editor

O rebase interativo abre um arquivo para você editar. No Git Bash, o editor padrão é o **Vim**, que confunde quem nunca o usou. Configure o VS Code como editor do Git uma única vez:

```bash
git config --global core.editor "code --wait"
```

O `--wait` faz o Git esperar até você **fechar a aba** do arquivo no VS Code. Se o comando `code` não for reconhecido no Git Bash, use o Bloco de Notas: `git config --global core.editor notepad`.

Se algum dia o Vim abrir mesmo assim, saia sem alterar nada com `Esc`, depois `:q!` e `Enter`.

### Editar a lista

```bash
git rebase -i HEAD~3
```

O editor abre com os três últimos commits, **do mais antigo para o mais recente**:

```text
pick 2f1a7c3 Adiciona secao de requisitos
pick 9e40b18 Inclui JDK 21 nos requisitos
pick c5d27a6 Corrige titulo da secao de requisitos
```

Dependendo da versão do Git, aparece um `#` antes da mensagem. Abaixo da lista vêm instruções em comentário. Troque a palavra do início de cada linha para dizer o que fazer:

| Palavra | Efeito |
|---|---|
| `pick` | mantém o commit como está |
| `reword` | mantém o commit, mas abre o editor para trocar a mensagem |
| `squash` | junta o commit ao anterior e permite escrever uma mensagem para o conjunto |
| `fixup` | junta o commit ao anterior e descarta a mensagem dele |
| `drop` | remove o commit |

Para transformar os três em um só, mantenha `pick` no primeiro e use `squash` nos outros dois. Salve e feche a aba. O Git abre outra aba com as três mensagens. Apague-as, escreva a mensagem final, salve e feche:

```text
Documenta requisitos do projeto
```

```text
Successfully rebased and updated refs/heads/main.
```

Se você se perder no meio, `git rebase --abort` devolve tudo ao estado anterior. Como todo rebase, o interativo **reescreve o histórico**: use-o só em commits ainda não publicados.

---

## 13. Copiar um commit: `git cherry-pick`

Às vezes você quer **um único commit** de outra branch, e não a branch inteira. Por exemplo, numa branch de experimentos, um commit ficou bom e os outros não. O `cherry-pick` aplica as alterações daquele commit na branch atual, como um commit novo:

```bash
git switch main
git log --oneline main..experimentos
git cherry-pick 8a3f1d2
```

```text
[main e19b4c0] Implementa busca de livro por autor
 1 file changed, 12 insertions(+)
```

O commit criado tem **outro hash**, porque tem outro pai, mas a mesma alteração e a mesma mensagem. A branch de origem não muda. Se houver conflito, resolva, use `git add` e conclua com `git cherry-pick --continue`. Para desistir, use `git cherry-pick --abort`.

Use o `cherry-pick` com moderação. Copiar commits entre branches que depois serão integradas pode deixar a mesma alteração duas vezes no histórico. Ele é ideal para casos pontuais, como levar uma correção específica para outra linha de desenvolvimento.

### Apagar a branch que sobrou

Depois de aproveitar um commit, a branch de experimentos pode ser descartada. Como ela tem commits que nunca foram integrados, o `-d` recusa:

```text
error: the branch 'experimentos' is not fully merged
```

O aviso protege trabalho não integrado. Se você tem certeza, use `-D`, que apaga mesmo assim:

```bash
git branch -D experimentos
```

---

## 14. Erros de compreensão que vale evitar

### "`git reset --hard` é o jeito normal de desfazer"

É o mais drástico. Para arquivos, use `restore`. Para juntar ou refazer commits, `--soft` ou `--mixed`. Para algo publicado, `revert`. Antes de qualquer `--hard`, confira o `git status`.

### "Se apaguei o commit, perdi o trabalho"

Commits desfeitos continuam acessíveis pelo `git reflog` por algumas semanas. O que se perde de verdade é o que **nunca foi commitado**.

### "O `revert` apaga o commit errado"

Ele cria um commit inverso. O erro continua no histórico, e é isso que torna o `revert` seguro para o que já foi publicado.

### "Depois do rebase, o Git mandou dar `pull`"

Numa branch que você acabou de reescrever, o `pull` traz de volta os commits antigos e duplica o histórico. Confira se a branch é só sua e use `git push --force-with-lease`.

### "`--force` e `--force-with-lease` são a mesma coisa"

O primeiro substitui o remoto às cegas. O segundo recusa se alguém tiver enviado algo que você ainda não viu.

### "Posso fazer rebase da `main` para deixar o histórico bonito"

A `main` é compartilhada. Reescrevê-la obriga todos a reparar as próprias cópias. Rebase é para a sua branch, antes de compartilhá-la ou integrá-la.

### "`git pull --force` deixa meu projeto igual ao GitHub"

Não deixa. O caminho é `git fetch` seguido de `git reset --hard origin/main`, sabendo que isso descarta o que é só local.

### "`git diff main feature` mostra o que a feature fez"

Mostra a diferença entre as duas pontas, incluindo, ao contrário, o que a `main` recebeu depois. Para ver só o trabalho da feature, use `git diff main...feature`.

### "O stash é um lugar seguro para guardar trabalho por dias"

O stash é local e fácil de esquecer. Para pausas longas, faça um commit numa branch.

---

## 15. Checklist do Tópico 3

- [ ] Aponto para commits com hash, `HEAD`, `HEAD~N`, nome de branch e `origin/main`.
- [ ] Sei que, num merge, `~` segue o primeiro pai, e uso `--first-parent` para conferir.
- [ ] Uso `git show` para ver um commit e `git show commit:arquivo` para ver uma versão antiga.
- [ ] Diferencio `git diff`, `git diff --staged` e `git diff HEAD`.
- [ ] Comparo dois commits e limito a comparação a um arquivo com `--`.
- [ ] Uso `--stat`, `--name-only` e `--word-diff`.
- [ ] Explico a diferença entre `git diff main feature` e `git diff main...feature`.
- [ ] Listo commits de uma branch que faltam na outra com `main..feature`.
- [ ] Descarto alterações com `git restore` e tiro da preparação com `git restore --staged`.
- [ ] Corrijo a mensagem ou incluo um arquivo no último commit com `--amend`.
- [ ] Explico o que `--soft`, `--mixed` e `--hard` fazem com a branch, a preparação e os arquivos.
- [ ] Sei que o `reset --hard` descarta alterações não commitadas sem volta.
- [ ] Recupero um commit desfeito com `git reflog` e `git branch nome HEAD@{N}`.
- [ ] Desfaço um commit publicado com `git revert`.
- [ ] Sei por que reescrever histórico publicado é perigoso e reconheço o `push` recusado por `non-fast-forward`.
- [ ] Deixo minha branch igual ao remoto com `git fetch` e `git reset --hard origin/main`.
- [ ] Sei que `git pull --force` não descarta o trabalho local.
- [ ] Guardo e recupero trabalho pela metade com `git stash push`, `list` e `pop`.
- [ ] Explico o que o rebase faz e por que os commits ganham hashes novos.
- [ ] Comparo merge e rebase.
- [ ] Uso `git pull --rebase` e sei que o `push` seguinte não precisa de força.
- [ ] Resolvo conflitos de rebase com `git add` e `git rebase --continue`, e desisto com `--abort`.
- [ ] Sei que, no rebase, "Current" é a base e "Incoming" é o meu commit.
- [ ] Publico uma branch minha reescrita com `--force-with-lease` e nunca forço a `main`.
- [ ] Configurei o VS Code como editor do Git.
- [ ] Junto e reorganizo commits com `git rebase -i`.
- [ ] Copio um commit com `git cherry-pick`.
- [ ] Apago uma branch não integrada com `git branch -D`, consciente do que perco.

---

## Conclusão

Até o Tópico 2, o histórico era algo que você ia acumulando. Neste tópico, ele passou a ser algo que você **lê com precisão** e **ajusta com critério**.

Você aprendeu a apontar para qualquer versão e a comparar o que quiser: áreas, commits, branches e o remoto. Aprendeu a desfazer em cada nível, com `restore` para arquivos, `amend` e `reset` para commits locais e `revert` para o que já foi publicado. O `reflog` mostrou que um commit raramente se perde. O `stash` permitiu pausar um trabalho, e o `rebase` ofereceu outra forma de integrar, com histórico linear. O `cherry-pick` completou a caixa de ferramentas.

Acima de todos os comandos fica a regra de ouro: **reescreva apenas o que ainda é só seu; corrija o que já é de todos com um commit novo**. Quando precisar substituir uma branch sua já publicada, use `--force-with-lease`, nunca na `main`.

No próximo tópico, essas ferramentas vão para o trabalho em equipe. Três pessoas vão desenvolver juntas no mesmo repositório, com branches, `fetch`, `pull`, `push`, Pull Requests e merges locais, num projeto organizado com **Maven**.

---

## Referências oficiais

- Pro Git, Reescrevendo o histórico: <https://git-scm.com/book/pt-br/v2/Ferramentas-do-Git-Reescrevendo-o-Hist%C3%B3rico>
- Pro Git, Reset desmistificado: <https://git-scm.com/book/pt-br/v2/Ferramentas-do-Git-Reset-Desmistificado>
- Pro Git, Rebase: <https://git-scm.com/book/pt-br/v2/Branches-no-Git-Rebase>
- Documentação do `git reset`: <https://git-scm.com/docs/git-reset>
- Documentação do `git revert`: <https://git-scm.com/docs/git-revert>
- Documentação do `git rebase`: <https://git-scm.com/docs/git-rebase>
- Documentação do `git stash`: <https://git-scm.com/docs/git-stash>
- Documentação do `git reflog`: <https://git-scm.com/docs/git-reflog>
- Documentação do `git push` (`--force-with-lease`): <https://git-scm.com/docs/git-push>
