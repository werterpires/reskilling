# Aula 3 — Comparar, desfazer e reorganizar o histórico

- Módulo: Git Básico → Intermediário + Maven
- Aula: 3 de 4
- Tema: Comparação de versões, desfazer alterações em cada nível, rebase e reorganização de commits
- Formato: Apresentação + demonstração prática
- Ambiente: Windows + Git Bash + VS Code + GitHub
- Duração sugerida: 2h30
- Projeto: Gestor de Tarefas (repositório publicado na aula anterior)

**Objetivo geral:** capacitar o aluno a ler o histórico com precisão, comparar versões, desfazer alterações no nível adequado (arquivo, commit local ou commit publicado), recuperar commits pelo reflog e reorganizar commits com rebase, sabendo quando cada comando é seguro.

## 1. Objetivos da aula

Ao final da aula, os participantes deverão ser capazes de:

- Apontar para commits com hash, `HEAD`, `HEAD~N`, branches e `origin/main`.
- Comparar áreas, commits e branches com as variações de `git diff`.
- Descartar alterações não commitadas com `git restore`.
- Corrigir o último commit com `git commit --amend`.
- Explicar e usar `git reset --soft`, `--mixed` e `--hard`.
- Recuperar commits desfeitos com `git reflog`.
- Desfazer um commit publicado com `git revert`.
- Guardar trabalho pela metade com `git stash`.
- Atualizar uma branch com `git rebase` e `git pull --rebase`.
- Publicar uma branch própria reescrita com `git push --force-with-lease`.
- Juntar commits com o rebase interativo e copiar um commit com `git cherry-pick`.

O objetivo central não é decorar comandos. É entender **o que cada comando faz com as três áreas e com o histórico** e aplicar a regra de ouro: reescrever só o que ainda não foi publicado.

## 2. Abertura: o histórico só cresce?

Comece retomando a aula anterior:

- O que `git push` faz? E `git fetch`? E `git pull`?
- O que é uma branch, na prática?
- Qual a diferença entre um fast-forward e um commit de merge?

Depois apresente situações que todo desenvolvedor enfrenta e pergunte à turma o que fariam:

- "Fiz um commit e a mensagem saiu errada."
- "Esqueci um arquivo no último commit."
- "Alterei um arquivo e quero a versão de ontem."
- "Publiquei um erro na `main`, e um colega já baixou."
- "Estou no meio de uma alteração e preciso dar `pull`."
- "Apaguei um commit sem querer."

Deixe a turma perceber que, até agora, só sabem **acrescentar** commits. Esta aula mostra como **ler**, **desfazer** e **arrumar**.

Apresente a regra de ouro logo no início e escreva-a no quadro, porque ela será retomada várias vezes:

> O que ainda não foi publicado pode ser reescrito. O que já foi publicado se corrige com um commit novo.

## 3. Apontar para commits

**Tempo sugerido: 10 minutos.**

Explique que, para comparar ou desfazer, é preciso dizer ao Git de qual commit se fala:

- hash (bastam 7 caracteres);
- `HEAD`, o commit atual;
- `HEAD~1`, `HEAD~3`, os commits anteriores;
- nomes de branch e `origin/main`.

Demonstre:

```bash
git log --oneline --first-parent -5
git show --stat HEAD~3
git show HEAD~8:README.md
```

**Ponto de atenção:** em commits de merge, o `~` segue o primeiro pai. Mostre a diferença entre `git log --oneline -5` e `git log --oneline --first-parent -5` no repositório do Gestor de Tarefas, que tem dois merges da aula anterior.

Destaque que `git show commit:arquivo` é só leitura: a forma mais segura de olhar o passado.

## 4. Comparar versões

**Tempo sugerido: 20 minutos.**

### 4.1. As três comparações

Desenhe no quadro as três áreas e as setas:

```text
diretório de trabalho ──► área de preparação ──► último commit (HEAD)
        └──── git diff ────┘        └── git diff --staged ──┘
        └──────────────────── git diff HEAD ────────────────────┘
```

Demonstre com uma alteração preparada e outra não preparada no mesmo arquivo. Mostre que cada comando exibe uma parte diferente.

### 4.2. Comparar commits

```bash
git diff --stat HEAD~3 HEAD
git diff HEAD~3 HEAD -- README.md
git diff --word-diff HEAD~1 HEAD
```

Explique `--stat`, `--name-only` e `--word-diff`. Reforce que a ordem dos argumentos define o "antes" e o "depois".

### 4.3. Comparar branches

Apresente a tabela:

| Comando | Pergunta |
|---|---|
| `git log main..feature` | o que a feature tem e a `main` não? |
| `git diff main feature` | qual a diferença entre as duas pontas? |
| `git diff main...feature` | o que a feature mudou desde que nasceu? |

**Ponto importante:** a forma com dois argumentos mostra também, ao contrário, o que a `main` recebeu depois. Para revisar uma feature, use três pontos. Se possível, deixe essa demonstração para o momento do rebase (seção 9), quando haverá uma branch atrasada em relação à `main`.

## 5. Desfazer em três níveis

**Tempo sugerido: 25 minutos.**

Organize a explicação pelos níveis, do mais leve ao mais forte.

### 5.1. Antes do commit: `git restore`

```bash
git restore --staged README.md
git restore README.md
```

- `--staged` tira da preparação e mantém a alteração no arquivo.
- Sem a opção, descarta a alteração do arquivo.

**Atenção:** o que nunca foi commitado e foi descartado não volta.

### 5.2. O último commit: `git commit --amend`

Demonstre os dois usos:

```bash
git commit --amend -m "Mensagem corrigida"
git add arquivo-esquecido
git commit --amend --no-edit
```

Mostre que o hash muda. Pergunte à turma: "se o commit antigo já estivesse no GitHub, o que aconteceria no próximo push?" Use a pergunta para preparar a seção 7.

### 5.3. Mover a branch: `git reset`

Desenhe a tabela no quadro:

| Modo | Branch | Preparação | Arquivos |
|---|---|---|---|
| `--soft` | volta | mantém | mantém |
| `--mixed` (padrão) | volta | volta | mantém |
| `--hard` | volta | volta | volta |

Demonstre um caso para cada modo:

- `--soft`: dois commits pequenos que viram um.
- `--mixed`: um commit que incluiu um arquivo por engano.
- `--hard`: um experimento descartado.

**Ponto de atenção:** o `--hard` descarta alterações não commitadas sem volta. Antes dele, sempre `git status`.

Explique a diferença entre `restore` e `reset`: o primeiro age sobre arquivos, o segundo move a branch.

## 6. A rede de segurança: `git reflog`

**Tempo sugerido: 10 minutos.**

Logo depois do `reset --hard`, mostre que o commit sumiu do `git log` e pergunte: "perdemos o trabalho?"

```bash
git reflog -5
git branch resgate HEAD@{1}
git log --oneline -1 resgate
```

Explique:

- o reflog registra cada movimento do `HEAD`;
- ele é local e expira depois de algumas semanas;
- ele guarda commits, nunca alterações não commitadas.

Mostre também o `git branch -d` recusando uma branch não integrada e o `-D` apagando mesmo assim. A recusa é uma proteção.

**Mensagem para a turma:** antes de um comando arriscado, faça commit. O que foi commitado quase sempre pode ser recuperado.

## 7. Desfazer o que já foi publicado: `git revert`

**Tempo sugerido: 20 minutos.**

### 7.1. Provocar o problema

Publique um erro proposital na `main`:

```bash
git commit -am "Atualiza descricao do projeto"
git push
```

Tente desfazer com `reset` e mostre a recusa:

```bash
git reset --hard HEAD~1
git push
```

```text
 ! [rejected]        main -> main (non-fast-forward)
```

Explique que o GitHub está protegendo o commit, que outras pessoas podem já ter baixado. Forçar o envio na `main` está fora de questão.

### 7.2. Ficar igual ao remoto

```bash
git reset --hard origin/main
```

Explique que esse é o comando para "descartar o meu local e ficar igual ao GitHub", sempre depois de um `git fetch`.

**Ponto importante:** esclareça o mito do `git pull --force`. A opção existe, mas não sobrescreve arquivos nem descarta commits locais. Ela apenas repassa o `--force` ao `fetch`, para casos raros de atualização de referências.

### 7.3. O commit inverso

```bash
git revert HEAD --no-edit
git log --oneline -3
git push
```

Mostre no GitHub que os dois commits aparecem e que o `push` foi comum.

**Ponte com COBOL:** compare com o estorno de um lançamento financeiro. O lançamento errado não é apagado: um lançamento inverso o anula, e os dois ficam registrados.

### 7.4. Consolidar a regra de ouro

| Situação | Ferramenta |
|---|---|
| alteração não commitada | `restore`, `stash` |
| commit não publicado | `amend`, `reset`, `rebase` |
| commit publicado em branch compartilhada | `revert` |

## 8. Guardar trabalho pela metade: `git stash`

**Tempo sugerido: 10 minutos.**

Monte a situação: uma alteração feita pelo GitHub e outra local, não commitada, no mesmo arquivo. Mostre o `git pull` recusando:

```text
error: Your local changes to the following files would be overwritten by merge:
```

Resolva com:

```bash
git stash push -m "Trabalho em andamento"
git stash list
git pull
git stash pop
```

Explique:

- `pop` reaplica e remove da pilha; `apply` reaplica e mantém.
- `-u` inclui arquivos novos.
- Em caso de conflito no `pop`, o item fica na pilha e deve ser removido com `git stash drop` depois da resolução.
- O stash é para pausas curtas. Para pausas longas, use um commit numa branch.

## 9. Rebase

**Tempo sugerido: 25 minutos.**

### 9.1. Conceito

Desenhe no quadro:

```text
antes:
      C ── D   feature
     /
A ── B ── E ── F   main

depois de git rebase main (na feature):
A ── B ── E ── F   main
                \
                 C' ── D'   feature
```

Explique:

- o rebase reaplica os commits da branch sobre a ponta da outra;
- os commits reaplicados são novos, com hashes novos;
- o histórico fica linear e o merge seguinte vira um fast-forward.

Compare com o merge da aula anterior:

| | merge | rebase |
|---|---|---|
| histórico | duas linhas e commit de merge | linear |
| commits existentes | não mudam | recriados |
| uso seguro | sempre | só em commits não compartilhados |

Reforce que o padrão do curso continua sendo o merge (`pull.rebase false`). O rebase é a ferramenta para arrumar a própria branch.

### 9.2. Demonstração com uma feature atrasada

Use uma branch publicada que ficou para trás da `main`:

```bash
git switch feature/contar-em-andamento
git log --oneline --graph --all
git diff --stat main feature/contar-em-andamento
git diff --stat main...feature/contar-em-andamento
git rebase main
git log --oneline --graph --all
```

Aproveite para mostrar a diferença entre dois e três pontos, combinada na seção 4.3.

### 9.3. `git pull --rebase`

Simule um commit local e outro feito pelo GitHub na mesma branch. Mostre o `push` recusado com `fetch first` e resolva com:

```bash
git pull --rebase
git push
```

Destaque que o `push` foi comum: o commit reescrito nunca tinha sido publicado.

### 9.4. Conflito durante o rebase

Provoque o conflito alterando a mesma linha nos dois lados. Mostre:

- os marcadores no arquivo;
- `git status` indicando *rebase in progress*;
- a resolução com `git add` e `git rebase --continue`;
- a saída de emergência com `git rebase --abort`.

**Ponto de atenção principal:** no rebase, **Current Change** é a versão da base (o remoto), e **Incoming Change** é o commit do aluno. É o contrário do merge. Peça que leiam o conteúdo antes de clicar em qualquer botão.

## 10. Publicar um histórico reescrito: `--force-with-lease`

**Tempo sugerido: 10 minutos.**

Volte à feature da seção 9.2, já reescrita pelo rebase. Mostre:

```bash
git status
git push
```

O `git status` sugere `git pull`. **Pare e explique** por que, nesse caso, seguir a sugestão duplicaria os commits.

Depois:

```bash
git push --force-with-lease
```

Compare as duas opções de força:

- `--force`: substitui o remoto sempre, mesmo que alguém tenha enviado algo.
- `--force-with-lease`: substitui só se o remoto ainda estiver onde você o viu.

Regras para a turma:

- Nunca forçar a `main`.
- Forçar apenas uma branch própria, sabendo que ninguém trabalha nela.

Finalize integrando a feature na `main` com um fast-forward e apagando a branch local e a remota.

## 11. Rebase interativo

**Tempo sugerido: 15 minutos.**

Confira que todos configuraram o VS Code como editor do Git:

```bash
git config --global core.editor "code --wait"
```

Explique o `--wait` e a saída de emergência do Vim (`Esc`, `:q!`, `Enter`) para quem ainda o vir.

Crie três commits pequenos e não publicados para uma mesma seção do README e execute:

```bash
git rebase -i HEAD~3
```

Mostre a lista, do mais antigo ao mais recente, e as palavras `pick`, `reword`, `squash`, `fixup` e `drop`. Troque as duas últimas por `squash`, feche a aba, escreva a mensagem final e feche de novo.

**Ponto importante:** só reorganizar commits não publicados. Conferir com `git status -sb` (`ahead N`) antes de começar.

## 12. Copiar um commit: `git cherry-pick`

**Tempo sugerido: 10 minutos.**

Crie uma branch `experimentos` com dois commits, um descartável e um útil. Na `main`:

```bash
git log --oneline main..experimentos
git cherry-pick HASH
git log --oneline --graph --all
```

Explique:

- o commit copiado tem outro hash e a mesma alteração;
- a branch de origem não muda;
- usar com moderação, porque copiar commits entre branches que depois serão integradas duplica alterações.

Finalize apagando a branch com `-D`.

## 13. Exercício prático de consolidação

**Objetivo:** aplicar todos os comandos da aula no repositório `gestor-tarefas`, seguindo os Exercícios 34 a 48 da folha de exercícios.

### Etapa 1 — Ler e comparar (Exercícios 34 a 37)

- Confirmar o ponto de partida e anotar o hash.
- Configurar o editor do Git.
- Usar `HEAD~N`, `git show` e as variações de `git diff`.
- Descartar alterações com `git restore`.

### Etapa 2 — Desfazer (Exercícios 38 a 42)

- Corrigir um commit com `--amend`.
- Usar os três modos de `reset`.
- Recuperar um commit pelo `reflog`.
- Publicar um erro e desfazê-lo com `revert`.

### Etapa 3 — Reorganizar (Exercícios 43 a 47)

- Usar `stash` para conseguir dar `pull`.
- Fazer rebase de uma feature e publicá-la com `--force-with-lease`.
- Resolver um conflito no `pull --rebase`.
- Juntar commits com o rebase interativo.
- Copiar um commit com `cherry-pick`.

### Etapa 4 — Conferência (Exercício 48)

- Apagar as branches que sobraram.
- Conferir os onze commits novos e o estado final.

### Critérios de conclusão

- `git status -sb` limpo e sincronizado com `origin/main`.
- Onze commits novos desde o ponto de partida.
- Nenhum commit de teste ou de depuração na `main`.
- Um commit de `Revert` no histórico.
- Feature integrada sem commit de merge.
- Apenas a branch `main` no GitHub.

## 14. Encerramento e revisão

Finalize perguntando:

- Qual a diferença entre `git diff`, `git diff --staged` e `git diff HEAD`?
- Por que `git diff main...feature` é melhor para revisar uma feature?
- Quando usar `restore`, `reset` e `revert`?
- O que cada modo do `reset` faz com a branch, a preparação e os arquivos?
- Como recuperar um commit depois de um `reset --hard`? E uma alteração nunca commitada?
- Por que não se faz `reset` na `main` publicada?
- O que o rebase faz com os hashes dos commits?
- Por que não dar `git pull` depois de reescrever uma branch publicada?
- Qual a diferença entre `--force` e `--force-with-lease`?
- No conflito de um rebase, quem é "Current" e quem é "Incoming"?

Retome a ideia principal: o Git permite desfazer quase tudo, mas cada nível tem a ferramenta certa. Reescrever é seguro enquanto o histórico é só seu; depois de compartilhado, a correção é um commit novo.

## Transição para a aula 4

Na próxima aula, a turma sai do repositório individual e passa a trabalhar em trios.

O foco será:

- Repositório compartilhado e colaboradores.
- Uma branch por responsabilidade.
- Ciclo de `fetch`, `pull` e `push` com outras pessoas no mesmo repositório.
- Pull Requests e revisão de código.
- Merge local: trazer a `main` para a branch e integrar sem o botão do GitHub.
- Introdução ao Maven: estrutura padrão, `pom.xml` e `mvn compile`.

**Mensagem de transição:** agora sabemos ler, desfazer e arrumar o histórico. Na próxima aula, vamos usar tudo isso com outras pessoas mexendo no mesmo repositório, onde a regra de ouro deixa de ser teoria.

## Notas para a condução

**Preparação do ambiente de demonstração.** Prepare antes da aula uma cópia do `gestor-tarefas` no estado final da aula anterior e um segundo navegador logado no GitHub para simular as alterações remotas. Várias demonstrações dependem de uma alteração feita "por outra pessoa".

**Configure o editor antes de tudo.** Peça a todos que executem `git config --global core.editor "code --wait"` na abertura, mesmo que o rebase interativo só apareça no fim. Um `git rebase --continue` ou um `git revert` sem `--no-edit` pode abrir o editor antes disso, e o Vim trava quem nunca o usou.

**Use o grafo o tempo todo.** Depois de cada comando que mexe no histórico, rode `git log --oneline --graph --all`. Ver os ponteiros se moverem é o que faz a turma entender `reset`, `rebase` e `cherry-pick`.

**Não aprofundar nesta aula:** revert de commits de merge (`-m 1`), `rebase --onto`, `git bisect`, `git filter-repo`, `reset` de arquivos específicos com caminhos e reescrita de histórico em branches compartilhadas.

**Minha recomendação para a sequência:** não apresente os comandos como uma lista. Apresente-os como respostas às perguntas da abertura, sempre na ordem "situação → comando → o que aconteceu com as três áreas e com o histórico". A tabela da regra de ouro (seção 7.4) deve ficar visível no quadro até o fim da aula.
