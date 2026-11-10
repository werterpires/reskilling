# Exercícios — Tópico 2: GitHub, repositórios remotos e branches

## Projeto prático: Gestor de Tarefas no GitHub

No Tópico 1, o `gestor-tarefas` ganhou um repositório Git local, com histórico, `.gitignore` e commits descritivos. Esse histórico ainda existe só no seu computador: ninguém mais o vê e, se a máquina falhar, ele se perde.

Nesta sequência, você levará o mesmo projeto para o **GitHub**, aprenderá a sincronizar os dois lados e passará a desenvolver funcionalidades em **branches**, sem mexer direto na linha principal.

Ao longo dos exercícios, você irá:

- conferir que o repositório do Tópico 1 está pronto para ser publicado;
- usar o **Source Control** do VS Code, que já traz a integração Git, sem extensão adicional;
- criar um repositório vazio no GitHub e conectá-lo ao repositório local com `origin`;
- publicar a branch `main` com `git push -u origin main`;
- enviar alterações locais e trazer uma alteração feita pelo navegador;
- diferenciar `git fetch` de `git pull`;
- criar, publicar e integrar branches com `git merge`;
- fazer uma integração que gera um commit de merge;
- provocar, resolver e registrar um conflito no `README.md`.

Esta etapa não usa `rebase`, `reset` nem `revert`: eles são assunto do próximo tópico. Pull Requests, revisão de código e Maven ficam para o Tópico 4.

> Continue trabalhando no **computador físico**, não na VDI. Se algum comando não apresentar o resultado esperado, pare e procure o instrutor com o código aberto e a mensagem **completa** exibida no terminal.

Todos os comandos usam o **Git Bash**, com barras normais (`/`). As mensagens de commit dos exemplos não têm acentos, apenas para simplificar a digitação no terminal.

---

## Ponto de partida

Use o resultado do Tópico 1. A pasta `gestor-tarefas` deve estar assim:

```text
gestor-tarefas/
├── .git/
├── .gitignore
├── README.md
├── modelo-inicial.txt
├── out/                  ← ignorada pelo Git
└── src/
    └── br/
        └── com/
            └── curso/
                └── tarefas/
                    ├── AplicacaoGestorTarefas.java
                    └── dominio/
                        ├── GestorTarefas.java
                        ├── Priorizavel.java
                        ├── Responsavel.java
                        ├── Tarefa.java
                        ├── TarefaComum.java
                        └── TarefaUrgente.java
```

A pasta deve estar na branch `main`, com o `status` limpo e pelo menos cinco commits. Se `Priorizavel.java` não existir no seu projeto, isso não impede a prática.

Se você não tiver a pasta `gestor-tarefas` com o conteúdo do Tópico 1, **procure o instrutor** antes de continuar.

---

## Exercício 16 — Confirmar o ponto de partida

Abra o Git Bash na pasta do projeto. Uma forma simples é clicar com o botão direito sobre `gestor-tarefas` no Explorador de Arquivos e escolher **Open Git Bash here**. Confirme o local:

```bash
pwd
ls -a
```

Resultado esperado, com o seu caminho no lugar do exemplo:

```text
/c/Users/SEU_USUARIO/gestor-tarefas
.  ..  .git  .gitignore  README.md  modelo-inicial.txt  out  src
```

Consulte o estado do repositório:

```bash
git status
git branch
git remote -v
git log --oneline
```

Resultado esperado:

```text
On branch main
nothing to commit, working tree clean
* main
cf44c7e Documenta compilacao no README
0849699 Atualiza modelo inicial
bae0125 Adiciona codigo-fonte do gestor de tarefas
26a43be Adiciona gitignore e modelo inicial
cfcab74 Cria README do projeto
```

Observações:

- os hashes e as mensagens serão diferentes, mas devem existir **pelo menos cinco** commits;
- `git remote -v` não imprime nada: ainda não há nenhum remoto configurado;
- se `git status` listar arquivos alterados, volte ao Tópico 1 e finalize esse commit antes de continuar;
- um aviso como `LF will be replaced by CRLF` é comportamento normal do Git no Windows e não é erro.

Confirme também que o projeto continua compilando e executando:

```bash
javac -d out src/br/com/curso/tarefas/dominio/*.java src/br/com/curso/tarefas/AplicacaoGestorTarefas.java
java -cp out br.com.curso.tarefas.AplicacaoGestorTarefas
```

As primeiras linhas da saída devem ser:

```text
=== GESTOR DE TAREFAS ===
Quantidade de tarefas: 3
```

Depois da compilação, rode `git status` de novo. Ele deve continuar limpo, porque `out/` está no `.gitignore`.

---

## Exercício 17 — Abrir o projeto no VS Code e reconhecer o Source Control

Abra o VS Code e use **File > Open Folder** para abrir a pasta `gestor-tarefas` inteira.

Abra o painel de controle de versão com `Ctrl + Shift + G` ou clicando no ícone **Source Control** da barra lateral. Como o `status` está limpo, a lista de alterações deve estar vazia.

Abra o terminal integrado com `` Ctrl + ` `` (ou **Terminal > New Terminal**). Se ele abrir em PowerShell, use a seta ao lado do botão `+` e escolha **Git Bash**. Em seguida, execute:

```bash
git --version
pwd
```

O primeiro comando deve exibir a versão instalada no Tópico 1, e o segundo, o caminho da pasta do projeto.

### Sobre a integração

O VS Code possui **integração nativa com Git**. Ele usa o Git que você já instalou no Windows. **Não é necessário instalar uma extensão chamada "Git"**. Se o painel disser que não encontrou o Git ou que a pasta não é um repositório, confira estes pontos, nesta ordem:

1. a pasta aberta contém `.git` (`ls -a` mostra);
2. `git --version` funciona no terminal;
3. feche e abra o VS Code novamente.

Extensões como **Git Graph** ou **GitHub Pull Requests** são complementos opcionais e não substituem essa integração.

---

## Exercício 18 — Fazer um commit pelo Source Control

Abra `README.md` no VS Code e **substitua todo o conteúdo** por:

```markdown
# Gestor de Tarefas

Descrição: Gestor de tarefas em Java para praticar Git.

## Como executar

1. Compile com javac, gerando as classes na pasta out.
2. Execute a classe br.com.curso.tarefas.AplicacaoGestorTarefas.
```

Salve com `Ctrl + S`. Observe o painel **Source Control**: o arquivo aparece em **Changes**, com a letra `M` (modified). Clique nele para abrir a comparação lado a lado.

Passe o mouse sobre o arquivo e clique no sinal **+** (**Stage Changes**). O arquivo passa para **Staged Changes**. Escreva no campo de mensagem:

```text
Reorganiza README com descricao e instrucoes
```

e clique em **Commit**.

Agora confirme no terminal que o painel executou comandos do Git que você já conhece:

```bash
git status
git log --oneline -3
```

Resultado esperado:

```text
On branch main
nothing to commit, working tree clean
bb78cd8 Reorganiza README com descricao e instrucoes
cf44c7e Documenta compilacao no README
0849699 Atualiza modelo inicial
```

| No Source Control | Comando equivalente |
|---|---|
| **+** (Stage Changes) | `git add README.md` |
| botão **Commit** | `git commit -m "mensagem"` |
| lista **Changes** | `git status` e `git diff` |

A interface ajuda a enxergar os estados, mas **não substitui** os comandos: quando algo der errado, a mensagem do terminal permite diagnosticar o problema.

---

## Exercício 19 — Preparar a conta do GitHub com segurança

Acesse `https://github.com/` e entre na sua conta. Se ainda não tiver uma, crie-a pela opção **Sign up**.

Siga estas regras durante todo o curso:

- use a **sua conta pessoal**, a menos que o instrutor informe uma política diferente;
- **não compartilhe** senhas, códigos de acesso ou tokens com colegas nem com o instrutor;
- **não publique código, dados ou documentos da empresa**; o único projeto publicado aqui será o `gestor-tarefas`, de uso didático;
- **não escreva credenciais em arquivos** do projeto, nem no `README.md`.

Confirme que você consegue ver:

- a página inicial depois do login;
- o seu perfil (clique na foto no canto superior direito e escolha **Your profile**);
- o botão para criar um repositório novo (o sinal **+** no cabeçalho, opção **New repository**).

Anote o seu nome de usuário. Nos exemplos, ele aparece como `SEU_USUARIO`.

### Ponte com COBOL

No ambiente mainframe, o código-fonte costuma ficar em um local central e controlado, acessado por ferramentas da própria instalação. O GitHub tem uma intenção parecida: servir como ponto de encontro compartilhado do código. A diferença é que, no Git, cada cópia local carrega o histórico completo, e é você quem decide quando enviar ou buscar alterações.

---

## Exercício 20 — Criar um repositório remoto vazio

No GitHub, clique em **New repository** e preencha:

| Campo | Valor |
|---|---|
| Repository name | `gestor-tarefas` |
| Description | `Gestor de tarefas em Java, projeto do curso de Git` |
| Visibilidade | **Public** ou **Private**, conforme a orientação do instrutor |
| Add a README file | **desmarcado** |
| Add .gitignore | **No .gitignore** |
| Choose a license | **No license** |

Clique em **Create repository**.

### Por que o repositório precisa ficar vazio

O seu projeto já tem arquivos, `README.md`, `.gitignore` e histórico local. Se o GitHub criar outro `README.md` ou outro commit inicial, o remoto terá uma história que o seu computador não conhece, e o primeiro `push` será recusado. Resolver isso exigiria uma integração extra, desnecessária nesta etapa.

A página que aparece em seguida mostra uma seção **Quick setup**. Alterne para **HTTPS** e copie o endereço que termina em `.git`:

```text
https://github.com/SEU_USUARIO/gestor-tarefas.git
```

O repositório existe, mas está vazio: nenhum arquivo seu chegou ao GitHub.

---

## Exercício 21 — Conectar o repositório local ao GitHub

No terminal integrado do VS Code, na pasta do projeto, confirme o nome da branch principal:

```bash
git branch
git branch -M main
git branch
```

A saída será a mesma nas duas consultas:

```text
* main
* main
```

`git branch -M main` renomeia a branch atual para `main`, substituindo um nome existente se necessário. No seu projeto ela já se chama assim, então nada muda. O comando é a forma segura de garantir o nome padrão.

Agora registre o repositório do GitHub como um **remoto** chamado `origin`, trocando `SEU_USUARIO` pelo seu nome de usuário:

```bash
git remote add origin https://github.com/SEU_USUARIO/gestor-tarefas.git
git remote -v
```

Resultado esperado:

```text
origin	https://github.com/SEU_USUARIO/gestor-tarefas.git (fetch)
origin	https://github.com/SEU_USUARIO/gestor-tarefas.git (push)
```

| Parte | Significado |
|---|---|
| `git remote` | gerencia referências a repositórios remotos |
| `add` | cria uma nova referência |
| `origin` | nome convencional do remoto principal |
| URL | endereço do repositório no GitHub |

`origin` é só um nome, um apelido para a URL. Cada linha de `git remote -v` indica um uso: buscar (`fetch`) e enviar (`push`).

Se o Git responder `error: remote origin already exists.`, o remoto já foi cadastrado. Confira o endereço com `git remote -v`. Para corrigir uma URL errada, use `git remote set-url origin URL_CORRETA`.

### Ponte com COBOL

Em um programa COBOL, o nome lógico de um arquivo no código é ligado ao arquivo físico fora dele. Aqui, `origin` é o nome curto que você usa nos comandos, e a URL é o endereço real. As situações não são equivalentes, mas a intenção é parecida: usar um nome estável no lugar de repetir o endereço.

---

## Exercício 22 — Publicar a branch `main`

Envie o histórico local ao GitHub:

```bash
git push -u origin main
```

Na primeira vez, o **Git Credential Manager** pode abrir uma janela ou o navegador para você entrar no GitHub e autorizar o acesso. Autentique-se apenas nessa tela do navegador. **Não digite a senha da conta no terminal** e não compartilhe nenhum código ou token.

As últimas linhas da saída devem ser parecidas com:

```text
To https://github.com/SEU_USUARIO/gestor-tarefas.git
 * [new branch]      main -> main
branch 'main' set up to track 'origin/main'.
```

Antes dessas linhas, o Git exibe linhas de progresso (`Enumerating objects` e outras), que variam com o projeto.

| Parte | Significado |
|---|---|
| `push` | envia os commits locais para o remoto |
| `origin` | destino do envio |
| `main` | branch enviada |
| `-u` | liga `main` local a `origin/main`, para que `git push` e `git pull` funcionem sem argumentos |

Confirme:

```bash
git status
git branch -vv
```

Resultado esperado:

```text
On branch main
Your branch is up to date with 'origin/main'.

nothing to commit, working tree clean
* main bb78cd8 [origin/main] Reorganiza README com descricao e instrucoes
```

No navegador, atualize a página do repositório e confira:

- `README.md`, `.gitignore`, `modelo-inicial.txt` e `src` aparecem na lista de arquivos;
- a pasta `out` **não** aparece, porque está no `.gitignore`;
- o `README.md` é exibido embaixo da lista de arquivos;
- o botão **Commits** mostra o histórico que você criou no Tópico 1;
- a branch exibida é `main`.

O commit registra a alteração **localmente**. O `push` compartilha esse commit com o GitHub.

---

## Exercício 23 — Enviar uma alteração local

Acrescente ao final do `README.md`:

```markdown

## Funcionalidades

- Cadastro de responsáveis
- Cadastro de tarefas comuns e urgentes
- Busca de tarefa por código
```

Salve. Pelo Source Control, confira a alteração, prepare o arquivo com **+** e faça o commit com a mensagem:

```text
Lista funcionalidades no README
```

Antes de enviar, pergunte ao Git em que situação está a branch:

```bash
git status
```

```text
On branch main
Your branch is ahead of 'origin/main' by 1 commit.
  (use "git push" to publish your local commits)

nothing to commit, working tree clean
```

`ahead by 1 commit` significa que existe um commit local que o GitHub ainda não possui. Envie:

```bash
git push
git status -sb
```

Resultado esperado:

```text
To https://github.com/SEU_USUARIO/gestor-tarefas.git
   bb78cd8..7b3e8f9  main -> main
## main...origin/main
```

Como `-u` foi usado no Exercício 22, basta `git push`. A linha `## main...origin/main` mostra que as duas pontas estão no mesmo ponto. Atualize a página no GitHub e confira o novo trecho do `README.md`.

---

## Exercício 24 — Alterar pelo GitHub e buscar com `fetch`

Agora a alteração nascerá no GitHub. No navegador:

1. abra `README.md` no repositório;
2. clique no ícone de lápis (**Edit this file**);
3. acrescente ao final:

   ```markdown

   ## Autoria

   Projeto didático mantido por SEU NOME.
   ```

4. clique em **Commit changes...**;
5. escreva a mensagem `Acrescenta autoria no README pelo GitHub`, mantenha **Commit directly to the `main` branch** e confirme.

Volte ao terminal. O seu computador **ainda não sabe** disso:

```bash
git status
```

```text
On branch main
Your branch is up to date with 'origin/main'.

nothing to commit, working tree clean
```

O Git não consulta o GitHub sozinho. Peça explicitamente que ele busque informações novas:

```bash
git fetch
git status
```

Resultado esperado:

```text
From https://github.com/SEU_USUARIO/gestor-tarefas
   7b3e8f9..280ef6c  main       -> origin/main
On branch main
Your branch is behind 'origin/main' by 1 commit, and can be fast-forwarded.
  (use "git pull" to update your local branch)

nothing to commit, working tree clean
```

Veja o que chegou, sem alterar seus arquivos:

```bash
git log --oneline --all
git diff --stat main origin/main
tail -3 README.md
```

```text
280ef6c (origin/main, origin/HEAD) Acrescenta autoria no README pelo GitHub
7b3e8f9 (HEAD -> main) Lista funcionalidades no README
...
 README.md | 4 ++++
 1 file changed, 4 insertions(+)
- Cadastro de responsáveis
- Cadastro de tarefas comuns e urgentes
- Busca de tarefa por código
```

O `fetch` trouxe o commit para `origin/main`, mas a branch `main` e o arquivo local **não mudaram**: o `tail` ainda mostra o final antigo.

---

## Exercício 25 — Integrar com `pull`

Integre a alteração à branch atual:

```bash
git pull
git log --oneline --decorate -3
tail -4 README.md
```

Resultado esperado:

```text
Updating 7b3e8f9..280ef6c
Fast-forward
 README.md | 4 ++++
 1 file changed, 4 insertions(+)
280ef6c (HEAD -> main, origin/main, origin/HEAD) Acrescenta autoria no README pelo GitHub
7b3e8f9 Lista funcionalidades no README
bb78cd8 Reorganiza README com descricao e instrucoes

## Autoria

Projeto didático mantido por SEU NOME.
```

`fetch` busca e atualiza `origin/main`, sem integrar. `pull` busca e integra a alteração à branch atual. A linha `Fast-forward` indica que a sua `main` apenas avançou, porque você não tinha commits novos que o GitHub desconhecesse.

### Se o `push` for recusado

Se você fizer um commit local e o GitHub tiver um commit que você ainda não baixou, o `git push` será recusado:

```text
 ! [rejected]        main -> main (fetch first)
error: failed to push some refs to 'https://github.com/SEU_USUARIO/gestor-tarefas.git'
hint: Updates were rejected because the remote contains work that you do not
hint: have locally.
```

A solução é executar `git pull`, conferir o resultado e só então repetir o `git push`. Se o `pull` reclamar de `divergent branches`, use `git pull --no-rebase --no-edit`: ele integra os dois históricos por merge, tema do Exercício 29.

---

## Exercício 26 — Criar uma branch e desenvolver nela

Até aqui, todos os commits entraram em `main`. Isso é arriscado quando uma funcionalidade está pela metade: ela pode comprometer a versão principal. A solução é desenvolver em uma **branch**, uma linha independente de commits.

Verifique a branch atual e crie a nova:

```bash
git branch
git switch -c feature/contar-pendentes
git branch
```

Resultado esperado:

```text
* main
Switched to a new branch 'feature/contar-pendentes'
* feature/contar-pendentes
  main
```

O asterisco marca a branch atual. `git switch` troca de branch, e `-c` cria a nova e já passa a usá-la. O prefixo `feature/` é uma **convenção** de equipe: o Git aceita qualquer nome válido. Para corrigir defeitos, usa-se `fix/`.

Em `GestorTarefas.java`, inclua o método abaixo logo antes de `exibirTarefasDoResponsavel`:

```java
    public int contarPendentes() {
        int quantidade = 0;

        for (Tarefa tarefa : tarefas) {
            if (tarefa.estaPendente()) {
                quantidade++;
            }
        }

        return quantidade;
    }
```

Salve e confira a alteração no Source Control. Prepare o arquivo e faça o commit:

```text
Implementa contagem de tarefas pendentes
```

Em `AplicacaoGestorTarefas.java`, insira logo acima da linha `gestor.iniciarTarefasPendentes();`:

```java
        System.out.println(
                "Tarefas pendentes: " + gestor.contarPendentes()
        );
```

Compile, execute e confira que aparece `Tarefas pendentes: 3` antes do resumo final:

```bash
javac -d out src/br/com/curso/tarefas/dominio/*.java src/br/com/curso/tarefas/AplicacaoGestorTarefas.java
java -cp out br.com.curso.tarefas.AplicacaoGestorTarefas
```

Faça o segundo commit:

```text
Exibe tarefas pendentes na aplicacao
```

Confira:

```bash
git log --oneline --decorate -4
```

```text
113db62 (HEAD -> feature/contar-pendentes) Exibe tarefas pendentes na aplicacao
529a50d Implementa contagem de tarefas pendentes
280ef6c (origin/main, origin/HEAD, main) Acrescenta autoria no README pelo GitHub
7b3e8f9 Lista funcionalidades no README
```

Os dois commits novos pertencem à branch atual. A `main` continua em `280ef6c`.

### Ponte com COBOL

Em ambientes mainframe, é comum falar em desenvolvimento, homologação e produção. Uma **branch** não é nada disso: não é um lugar onde o programa executa. É uma linha de histórico do código-fonte. Uma mesma branch pode ser compilada e executada em qualquer máquina que possua uma cópia dela.

---

## Exercício 27 — Publicar a branch no GitHub

Uma branch local não vai para o GitHub sozinha. Publique-a:

```bash
git push -u origin feature/contar-pendentes
git branch -vv
```

Resultado esperado:

```text
To https://github.com/SEU_USUARIO/gestor-tarefas.git
 * [new branch]      feature/contar-pendentes -> feature/contar-pendentes
branch 'feature/contar-pendentes' set up to track 'origin/feature/contar-pendentes'.
* feature/contar-pendentes 113db62 [origin/feature/contar-pendentes] Exibe tarefas pendentes na aplicacao
  main                     280ef6c [origin/main] Acrescenta autoria no README pelo GitHub
```

O GitHub pode exibir mensagens como `remote: Create a pull request for...`. Ignore-as por enquanto: Pull Requests são assunto do Tópico 4.

No navegador, confira:

1. o seletor de branches (botão com o nome `main`) lista também `feature/contar-pendentes`;
2. ao selecionar a nova branch, os dois commits aparecem em **Commits**;
3. ao voltar para `main`, o método `contarPendentes` **não** existe em `GestorTarefas.java`.

A funcionalidade está compartilhada, mas ainda isolada da linha principal.

---

## Exercício 28 — Integrar com `merge` (fast-forward)

O **merge** integra as alterações de uma branch à branch em que você está. Por isso, primeiro mude para a que **receberá** o resultado:

```bash
git switch main
git status
git log --oneline main..feature/contar-pendentes
```

Resultado esperado:

```text
Switched to branch 'main'
Your branch is up to date with 'origin/main'.
On branch main
Your branch is up to date with 'origin/main'.

nothing to commit, working tree clean
113db62 Exibe tarefas pendentes na aplicacao
529a50d Implementa contagem de tarefas pendentes
```

O último comando lista os commits que existem na feature e ainda não existem em `main`. Integre:

```bash
git merge feature/contar-pendentes
```

```text
Updating 280ef6c..113db62
Fast-forward
 src/br/com/curso/tarefas/AplicacaoGestorTarefas.java |  3 +++
 src/br/com/curso/tarefas/dominio/GestorTarefas.java  | 12 ++++++++++++
 2 files changed, 15 insertions(+)
```

`Fast-forward` ocorre porque `main` não recebeu nenhum commit novo desde que a feature nasceu. O Git só avança o ponteiro de `main` para o último commit da feature, sem criar um commit de merge.

Veja o desenho do histórico e envie o resultado:

```bash
git log --oneline --graph --all --decorate
git push
```

```text
* 113db62 (HEAD -> main, origin/feature/contar-pendentes) Exibe tarefas pendentes na aplicacao
* 529a50d Implementa contagem de tarefas pendentes
* 280ef6c (origin/main, origin/HEAD) Acrescenta autoria no README pelo GitHub
* 7b3e8f9 Lista funcionalidades no README
...
To https://github.com/SEU_USUARIO/gestor-tarefas.git
   280ef6c..113db62  main -> main
```

`--graph` desenha as linhas de desenvolvimento e `--all` inclui todas as branches. Como a história é linear, o desenho é uma linha reta.

Com a funcionalidade integrada, a branch local não é mais necessária:

```bash
git branch -d feature/contar-pendentes
```

```text
Deleted branch feature/contar-pendentes (was 113db62).
```

`-d` só apaga uma branch que já foi integrada. A cópia `origin/feature/contar-pendentes` continua no GitHub e não atrapalha.

---

## Exercício 29 — Integrar históricos que seguiram caminhos diferentes

Nem todo merge é uma simples avançada. Quando `main` e a branch receberam commits **diferentes**, o Git precisa combinar os dois e cria um **commit de merge**.

Parta de `main`, limpa e publicada, e crie uma nova branch:

```bash
git switch -c feature/contar-concluidas
```

Em `GestorTarefas.java`, inclua logo antes de `exibirTarefasDoResponsavel`:

```java
    public int contarConcluidas() {
        int quantidade = 0;

        for (Tarefa tarefa : tarefas) {
            if (tarefa.estaConcluida()) {
                quantidade++;
            }
        }

        return quantidade;
    }
```

Compile para garantir que não há erros e faça o commit:

```bash
javac -d out src/br/com/curso/tarefas/dominio/*.java src/br/com/curso/tarefas/AplicacaoGestorTarefas.java
git add src/br/com/curso/tarefas/dominio/GestorTarefas.java
git commit -m "Implementa contagem de tarefas concluidas"
```

Volte para `main` e faça uma alteração **em outro arquivo**: acrescente ao final do `README.md`.

```markdown

## Próximos passos

- Contar tarefas por situação
- Listar tarefas por responsável
```

```bash
git switch main
git add README.md
git commit -m "Registra proximos passos no README"
```

Agora as duas linhas avançaram:

```bash
git log --oneline --graph --all --decorate
```

```text
* a5fc90e (feature/contar-concluidas) Implementa contagem de tarefas concluidas
| * a2c25df (HEAD -> main) Registra proximos passos no README
|/
* 113db62 (origin/main, origin/feature/contar-pendentes, origin/HEAD) Exibe tarefas pendentes na aplicacao
...
```

Integre. A opção `-m` define a mensagem do commit de merge e evita abrir um editor:

```bash
git merge feature/contar-concluidas -m "Integra contagem de tarefas concluidas"
```

```text
Merge made by the 'ort' strategy.
 src/br/com/curso/tarefas/dominio/GestorTarefas.java | 12 ++++++++++++
 1 file changed, 12 insertions(+)
```

Não houve conflito porque cada lado mexeu em **arquivos diferentes**. Observe o desenho do histórico:

```bash
git log --oneline --graph --all --decorate
```

```text
*   72db62e (HEAD -> main) Integra contagem de tarefas concluidas
|\
| * a5fc90e (feature/contar-concluidas) Implementa contagem de tarefas concluidas
* | a2c25df Registra proximos passos no README
|/
* 113db62 (origin/main, origin/feature/contar-pendentes, origin/HEAD) Exibe tarefas pendentes na aplicacao
```

As linhas se separam e voltam a se juntar no commit de merge. Envie e limpe a branch local:

```bash
git push
git branch -d feature/contar-concluidas
```

---

## Exercício 30 — Provocar um conflito

Um **conflito** acontece quando o Git não consegue decidir sozinho entre duas alterações. O caso mais comum é duas pessoas, ou duas branches, alterarem a **mesma linha** de maneiras diferentes. Para produzir um conflito de verdade, as duas alterações precisam estar na mesma linha.

Confirme que `main` está limpa e publicada:

```bash
git status -sb
```

```text
## main...origin/main
```

Crie a branch e altere a linha que começa com `Descrição:` no `README.md` (a terceira linha):

```bash
git switch -c feature/descricao-alternativa
```

Troque:

```text
Descrição: Gestor de tarefas em Java para praticar Git.
```

por:

```text
Descrição: Gestor para acompanhar tarefas e responsáveis de uma equipe.
```

Registre:

```bash
git add README.md
git commit -m "Reescreve descricao com foco em equipe"
```

Volte a `main` e altere **a mesma linha** com outro texto:

```bash
git switch main
```

```text
Descrição: Gestor de tarefas urgentes e comuns, desenvolvido em Java.
```

```bash
git add README.md
git commit -m "Reescreve descricao com foco em tipos de tarefa"
```

As duas branches partiram da mesma versão da linha e a modificaram de formas diferentes. Tente integrar:

```bash
git merge feature/descricao-alternativa
```

Resultado esperado:

```text
Auto-merging README.md
CONFLICT (content): Merge conflict in README.md
Automatic merge failed; fix conflicts and then commit the result.
```

Consulte a situação:

```bash
git status
```

```text
On branch main
Your branch is ahead of 'origin/main' by 1 commit.
  (use "git push" to publish your local commits)

You have unmerged paths.
  (fix conflicts and run "git commit")
  (use "git merge --abort" to abort the merge)

Unmerged paths:
  (use "git add <file>..." to mark resolution)
	both modified:   README.md

no changes added to commit (use "git add" and/or "git commit -a")
```

O conflito não significa que o projeto foi perdido. O Git integrou tudo o que pôde e marcou apenas o ponto que exige decisão humana. No Source Control, `README.md` aparece no grupo **Merge Changes**, com a letra `C`.

### Ponte com COBOL

Quando duas pessoas alteram o mesmo fonte, o risco é uma sobrescrever o trabalho da outra sem perceber. Diferentes ferramentas e processos enfrentam isso à sua maneira. No Git, a intenção é impedir que isso aconteça em silêncio: ele mostra exatamente quais linhas divergem e pede a decisão de quem conhece o objetivo do arquivo.

---

## Exercício 31 — Resolver o conflito pelo VS Code

Abra `README.md`. A região em conflito terá este formato:

```text
<<<<<<< HEAD
Descrição: Gestor de tarefas urgentes e comuns, desenvolvido em Java.
=======
Descrição: Gestor para acompanhar tarefas e responsáveis de uma equipe.
>>>>>>> feature/descricao-alternativa
```

| Marcador | Significado |
|---|---|
| `<<<<<<< HEAD` | início da versão da branch atual (`main`) |
| `=======` | separação entre as duas versões |
| `>>>>>>> feature/descricao-alternativa` | fim da versão da outra branch |

Acima do bloco, o VS Code oferece as ações **Accept Current Change**, **Accept Incoming Change**, **Accept Both Changes** e **Compare Changes**. Versões recentes também mostram o botão **Resolve in Merge Editor**, que é opcional. Os nomes podem aparecer traduzidos.

Essas ações são atalhos para escolher texto; não decidem por você. **Accept Both** não é automaticamente correto: manteria duas descrições para o mesmo projeto.

Escreva o resultado final como uma única linha coerente, removendo os três marcadores:

```text
Descrição: Gestor de tarefas comuns e urgentes para acompanhar o trabalho de uma equipe.
```

Você pode usar as ações do VS Code e depois ajustar o texto, ou editar o arquivo diretamente. Salve e confirme que **nenhum marcador sobrou**:

```bash
grep -nE '^(<<<<<<<|=======|>>>>>>>)' README.md
echo "codigo de saida: $?"
```

```text
codigo de saida: 1
```

O `grep` não imprime nada porque não achou os marcadores, e o código `1` informa isso. Se ele imprimir linhas, ainda restam marcadores no arquivo. Abra o arquivo, remova-os e repita a verificação. Nunca deixe `<<<<<<<` em um arquivo que será versionado.

---

## Exercício 32 — Registrar a resolução e publicar

Resolver o arquivo ainda não encerra o merge. O Git precisa saber que você terminou, e isso é feito com `add` e `commit`.

```bash
git add README.md
git status
```

Resultado esperado:

```text
On branch main
Your branch is ahead of 'origin/main' by 1 commit.
  (use "git push" to publish your local commits)

All conflicts fixed but you are still merging.
  (use "git commit" to conclude merge)

Changes to be committed:
	modified:   README.md
```

Conclua:

```bash
git commit -m "Resolve conflito na descricao do README"
git status
git log --oneline --graph --all --decorate -4
```

```text
[main 2864ad0] Resolve conflito na descricao do README
On branch main
Your branch is ahead of 'origin/main' by 3 commits.
  (use "git push" to publish your local commits)

nothing to commit, working tree clean
*   2864ad0 (HEAD -> main) Resolve conflito na descricao do README
|\
| * 0629f4b (feature/descricao-alternativa) Reescreve descricao com foco em equipe
* | 5281e67 Reescreve descricao com foco em tipos de tarefa
|/
*   72db62e (origin/main, origin/HEAD) Integra contagem de tarefas concluidas
```

Os 3 commits à frente são os dois da linha de conflito e o commit de merge. Antes de publicar, confirme que o projeto ainda compila e executa:

```bash
javac -d out src/br/com/curso/tarefas/dominio/*.java src/br/com/curso/tarefas/AplicacaoGestorTarefas.java
java -cp out br.com.curso.tarefas.AplicacaoGestorTarefas
```

Publique e limpe:

```bash
git push
git branch -d feature/descricao-alternativa
git branch -a
```

```text
To https://github.com/SEU_USUARIO/gestor-tarefas.git
   72db62e..2864ad0  main -> main
Deleted branch feature/descricao-alternativa (was 0629f4b).
* main
  remotes/origin/HEAD -> origin/main
  remotes/origin/feature/contar-pendentes
  remotes/origin/main
```

No GitHub, abra o `README.md` e confirme a descrição escolhida. Em **Commits**, localize o commit `Resolve conflito na descricao do README`.

### Se você quiser desistir

Se ao ver o conflito você decidir cancelar a integração, o comando é:

```bash
git merge --abort
```

Ele **desfaz a tentativa de merge** e devolve a branch ao estado anterior. Use-o quando quiser desistir, e não como solução automática para qualquer conflito.

---

## Exercício 33 — Conferência final do Tópico 2

### Ambiente

- [ ] O projeto `gestor-tarefas` está aberto como pasta no VS Code.
- [ ] O painel **Source Control** mostra o repositório sem alterações pendentes.
- [ ] Você sabe que a integração Git do VS Code é nativa e não depende de uma extensão chamada "Git".

### GitHub e remoto

- [ ] Existe um repositório `gestor-tarefas` na sua conta pessoal do GitHub.
- [ ] Ele foi criado vazio, sem README, `.gitignore` ou licença.
- [ ] `git remote -v` mostra `origin` com a URL HTTPS correta.
- [ ] Nenhuma senha, token ou código da empresa foi enviado ou digitado em arquivo.
- [ ] A pasta `out` não aparece no GitHub.

### Sincronização

- [ ] `git push -u origin main` publicou a branch `main`.
- [ ] Você enviou uma alteração local com `git push`.
- [ ] Você alterou o `README.md` pelo GitHub e a trouxe com `git fetch` seguido de `git pull`.
- [ ] Você sabe explicar a diferença entre `fetch` e `pull`.

### Branches e merge

- [ ] Você criou `feature/contar-pendentes` com `git switch -c` e a publicou com `git push -u`.
- [ ] A integração feita com `git merge` resultou em `Fast-forward`.
- [ ] Você criou um commit de merge sem conflito (Exercício 29).
- [ ] O método `contarPendentes` e o método `contarConcluidas` existem em `GestorTarefas`.

### Conflito

- [ ] Duas alterações na mesma linha do `README.md` geraram `CONFLICT (content)`.
- [ ] Os marcadores foram removidos e `grep` não encontrou nenhum.
- [ ] A resolução foi registrada com `git add` e `git commit`.
- [ ] O resultado foi publicado com `git push`.

### Estado final

Execute:

```bash
git status -sb
git log --oneline | wc -l
git log --oneline --merges
git ls-remote --heads origin
```

Confira:

- [ ] `git status -sb` exibe `## main...origin/main`, sem arquivos pendentes.
- [ ] O histórico possui pelo menos 16 commits.
- [ ] Existem pelo menos dois commits de merge.
- [ ] `git ls-remote --heads origin` lista `refs/heads/main`, com o mesmo hash que `git log --oneline -1` mostra.
- [ ] O programa compila e exibe `Tarefas pendentes: 3`.

---

## Entrega

Entregue o **endereço do repositório** no GitHub:

```text
https://github.com/SEU_USUARIO/gestor-tarefas
```

Se o repositório for privado, siga a orientação do instrutor para que ele consiga acessar. Não envie senha, token ou código de acesso.

Antes de entregar, abra o endereço e confirme:

- a branch `main` tem o `README.md` com a descrição resolvida;
- os métodos de contagem estão em `GestorTarefas.java`;
- o histórico mostra os commits de merge e a resolução do conflito.

---

## Comandos de referência

Use a partir da pasta `gestor-tarefas`.

| Objetivo | Comando |
|---|---|
| Ver estado da árvore e do remoto | `git status` |
| Listar branches locais | `git branch` |
| Listar branches e seus remotos | `git branch -vv` |
| Cadastrar o remoto | `git remote add origin URL` |
| Conferir remotos | `git remote -v` |
| Publicar a branch pela primeira vez | `git push -u origin NOME_DA_BRANCH` |
| Enviar commits novos | `git push` |
| Buscar sem integrar | `git fetch` |
| Buscar e integrar | `git pull` |
| Criar e trocar de branch | `git switch -c feature/nome` |
| Trocar de branch | `git switch nome` |
| Integrar uma branch na atual | `git merge feature/nome` |
| Merge com mensagem | `git merge feature/nome -m "mensagem"` |
| Apagar uma branch já integrada | `git branch -d feature/nome` |
| Desistir de um merge em andamento | `git merge --abort` |
| Ver o desenho do histórico | `git log --oneline --graph --all --decorate` |

Método usado no Exercício 26, para consulta:

```java
    public int contarPendentes() {
        int quantidade = 0;

        for (Tarefa tarefa : tarefas) {
            if (tarefa.estaPendente()) {
                quantidade++;
            }
        }

        return quantidade;
    }
```

---

## Síntese final do tópico

No Tópico 1, o `gestor-tarefas` ganhou um histórico local. Neste, ele passou a existir também no GitHub, e você aprendeu o ciclo completo entre os dois lados:

- `origin` é o apelido do repositório remoto;
- `push` envia os seus commits e `fetch` traz informação sem alterar o seu trabalho;
- `pull` busca e integra;
- uma branch isola uma funcionalidade e precisa ser publicada para existir no GitHub;
- `merge` traz o trabalho de uma branch para outra, avançando o ponteiro (`Fast-forward`) ou criando um commit de merge;
- conflito é um pedido de decisão humana, não um erro: você lê os marcadores, escolhe o texto final, remove-os e registra a resolução.

O VS Code mostra tudo isso de forma visual, mas os comandos explicam o que acontece por trás. No próximo tópico, o mesmo repositório servirá para praticar como comparar, desfazer e reorganizar o histórico.
