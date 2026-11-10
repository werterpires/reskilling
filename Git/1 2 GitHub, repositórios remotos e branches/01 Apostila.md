# Tópico 2 — GitHub, repositórios remotos e branches

## Do histórico local ao código compartilhado

No Tópico 1, você instalou o Git, criou o repositório do projeto, usou as três áreas (diretório de trabalho, área de preparação e repositório) e registrou versões com `git add` e `git commit`. Também aprendeu a consultar o histórico com `git log` e a comparar alterações com `git diff`.

Tudo isso aconteceu dentro do seu computador. O repositório local é uma ferramenta excelente para guardar a história do seu trabalho, mas ele deixa algumas perguntas sem resposta:

- como outra pessoa recebe o código que você escreveu, e você o dela?
- como o histórico sobrevive se o seu computador falhar?
- como desenvolver uma funcionalidade nova sem desestabilizar a versão que já funciona?

Neste tópico, você responderá a essas perguntas com o projeto **Gestão de Biblioteca** (`gestao-biblioteca`), com classes como `Livro`, `Usuario`, `Emprestimo` e `Biblioteca`. Você parte do repositório local que já possui commits e o publicará no **GitHub**.

## O que você fará neste tópico

- diferenciar Git e GitHub;
- usar a integração Git do Visual Studio Code e o painel Source Control;
- criar um repositório remoto no GitHub;
- conectar o repositório local ao remoto com `git remote` e enviar o projeto com `git push`;
- trazer alterações com `git fetch` e `git pull`;
- criar, listar e trocar de branches com `git branch` e `git switch`;
- integrar branches com `git merge`;
- reconhecer e resolver um conflito.

> Comparar versões e desfazer alterações (`reset`, `revert`, `rebase` e outros) é o assunto do próximo tópico. Pull Requests, Code Review e Maven ficam para o Tópico 4.

---

## 1. Git e GitHub: duas coisas diferentes

É comum ouvir "subi no Git" quando alguém publicou código no GitHub. Ao aprender, é preciso separar as duas peças.

**Git** é o sistema de controle de versão. Ele roda no seu computador, mantém o histórico e executa comandos como `commit`, `branch` e `merge`. Funciona mesmo sem internet.

**GitHub** é uma plataforma que hospeda repositórios Git em servidores e acrescenta recursos de colaboração. É um serviço, não o Git em si. Existem outros serviços parecidos, como GitLab e Bitbucket, e o Git funciona da mesma maneira com todos eles.

| Termo | Significado |
|---|---|
| Git | sistema de controle de versão, instalado no seu computador |
| GitHub | plataforma que hospeda repositórios e oferece recursos de colaboração |
| Repositório local | repositório que existe no seu computador, com a pasta `.git` |
| Repositório remoto | repositório hospedado em outro lugar, como o GitHub |
| `clone` | cria no seu computador uma cópia de um repositório remoto, já ligada a ele |
| `push` | envia seus commits para o repositório remoto |
| `fetch` | busca do remoto os commits novos, sem alterar o seu trabalho |
| `pull` | busca os commits novos e os integra à branch atual |
| Branch | linha independente de desenvolvimento |

O ponto central: **o Git continua sendo a ferramenta que versiona**. O GitHub guarda uma cópia do repositório e oferece um lugar comum para compartilhar commits.

### Relação com o mundo mainframe

Em ambientes mainframe, o código-fonte também costuma ser protegido por uma ferramenta de gestão de mudanças ou de configuração, com controle de quem altera, promoção entre ambientes e histórico de versões. Você já conhece a intenção: saber o que mudou, quem mudou e poder voltar atrás.

O Git, porém, é **distribuído**: cada pessoa tem uma cópia completa do histórico no próprio computador, e o remoto é uma outra cópia, que as pessoas combinam usar como ponto de encontro. Não existe a ideia de "trazer o fonte da biblioteca central, travá-lo e devolvê-lo". Cada pessoa trabalha na sua cópia e as alterações são combinadas depois.

---

## 2. A integração Git do Visual Studio Code

A partir deste tópico, o VS Code será sua interface principal. Abra a pasta do projeto com **File → Open Folder** e selecione `gestao-biblioteca`.

O VS Code possui **integração nativa com o Git**. Você não precisa instalar uma extensão chamada simplesmente "Git". O que ele usa é o Git que você já instalou no Tópico 1, o mesmo que responde ao comando `git --version` no terminal.

Três áreas merecem atenção:

- **Explorer**: a árvore de arquivos do projeto;
- **terminal integrado** (**Terminal → New Terminal**), onde você pode executar os mesmos comandos do Git Bash;
- **Source Control**, aberto pelo ícone de ramificação na barra lateral ou por `Ctrl + Shift + G`.

Se o VS Code não reconhecer o repositório, confira três pontos: se a pasta aberta contém o diretório `.git` na raiz, se o Git está instalado e acessível, e se o VS Code foi reiniciado depois da instalação.

### O painel Source Control

O painel mostra com imagens o que você já estudou com comandos:

| Elemento do painel | Estado equivalente |
|---|---|
| **Changes** | arquivos modificados, ainda no diretório de trabalho |
| **Staged Changes** | arquivos na área de preparação, prontos para o commit |
| botão **+** em um arquivo | `git add arquivo` |
| campo de mensagem e botão **Commit** | `git commit -m "mensagem"` |
| clique sobre um arquivo | exibe o `git diff` do arquivo, lado a lado |

Para testar: altere uma linha do `README.md`, veja o arquivo em **Changes**, clique nele para ver a diferença, use o **+**, escreva a mensagem e clique em **Commit**.

O painel não faz nada que o Git não faça por comando. Ele facilita a leitura do estado. Não dependa exclusivamente da interface: os comandos ajudam a entender o que acontece e a diagnosticar problemas. Diante de algo inesperado, volte ao terminal e use `git status`.

### Extensões complementares

Extensões como **Git Graph**, que desenha o histórico e as branches, e **GitHub Pull Requests**, que pode ajudar no Tópico 4, são complementares. A integração básica com o Git não depende delas.

---

## 3. Conhecendo o GitHub

Acesse `https://github.com/`, crie uma conta pessoal, se ainda não tiver, e faça login. Antes de publicar qualquer coisa, três cuidados:

1. **confirme a orientação da sua empresa ou da turma** sobre o uso de contas pessoais;
2. **não compartilhe credenciais**: cada pessoa usa a própria conta;
3. **não publique código corporativo ou informações confidenciais** em repositórios pessoais. Use apenas o projeto didático deste curso.

Código de produção, dados de clientes e nomes de sistemas internos não pertencem a um repositório pessoal na internet.

### Criar um repositório vazio

No GitHub, use **New repository** e preencha:

- **Repository name**: `gestao-biblioteca`;
- **Description**: uma frase curta, por exemplo, "Projeto desenvolvido durante o curso de Git";
- **Public** ou **Private**, conforme a orientação recebida;
- **não marque** "Add a README file";
- **não adicione** `.gitignore`;
- **não escolha** licença.

A razão para o repositório nascer **vazio** é que o seu projeto já possui arquivos e histórico locais. Se o GitHub criar um README próprio, o remoto terá um commit que o seu repositório local não conhece, e os dois históricos começarão em pontos diferentes. Isso exigiria uma integração desnecessária neste momento.

Depois de criado, o repositório mostra instruções e a URL no formato HTTPS:

```text
https://github.com/SEU-USUARIO/gestao-biblioteca.git
```

Neste ponto, o GitHub já tem um repositório, mas ele ainda não recebeu nenhum arquivo do seu projeto local.

---

## 4. Conectar o repositório local ao GitHub

Para o Git enviar commits a outro lugar, ele precisa de um **remote**: um nome que aponta para a URL de um repositório remoto. Por convenção, o remoto principal se chama **`origin`**. O nome é apenas uma convenção, mas é tão comum que a documentação e as mensagens do Git o assumem.

### Conferir o nome da branch principal

No terminal integrado, dentro da pasta do projeto:

```bash
git branch
```

```text
* main
```

O asterisco marca a branch atual. Se a sua branch principal tiver outro nome, como `master`, padronize:

```bash
git branch -M main
```

`-M` renomeia a branch atual, inclusive quando já existir uma com esse nome. `main` é o nome adotado pelo GitHub para novos repositórios e pelo que você configurou no Tópico 1.

### Registrar o endereço remoto

```bash
git remote add origin https://github.com/SEU-USUARIO/gestao-biblioteca.git
```

Leia por partes: `git remote` gerencia referências remotas, `add` cria uma, `origin` é o nome e a URL é o endereço. Esse comando **não envia nada**. Ele só registra a ligação. Confira:

```bash
git remote -v
```

```text
origin	https://github.com/SEU-USUARIO/gestao-biblioteca.git (fetch)
origin	https://github.com/SEU-USUARIO/gestao-biblioteca.git (push)
```

As duas linhas mostram o endereço usado para buscar (`fetch`) e para enviar (`push`). Para corrigir a URL, use `git remote set-url origin NOVA-URL`.

### Enviar o projeto

```bash
git push -u origin main
```

| Parte | Significado |
|---|---|
| `git push` | enviar commits para um repositório remoto |
| `origin` | destino: o remoto registrado antes |
| `main` | a branch local que será enviada |
| `-u` | grava que a `main` local acompanha a `main` do `origin` |

O `-u` (`--set-upstream`) é usado na primeira vez. Depois dele, o Git sabe qual é a branch remota correspondente, e um simples `git push` ou `git pull` passa a funcionar. A saída termina assim:

```text
To https://github.com/SEU-USUARIO/gestao-biblioteca.git
 * [new branch]      main -> main
branch 'main' set up to track 'origin/main'.
```

Antes dessas linhas, o Git exibe contadores de progresso, cujos números variam.

### Autenticação

Na primeira vez, o Git precisa saber quem você é para o GitHub. Em HTTPS, quem cuida disso é o **Git Credential Manager**, que você manteve ativo na instalação do Tópico 1. Ele abre o navegador, você autoriza o acesso com a sua conta e o gerenciador guarda a autorização, de modo que os próximos envios não pedem nada.

Dois alertas:

- o GitHub **não aceita a senha da conta** em operações Git por HTTPS. O caminho é a autorização pelo navegador ou um *personal access token*, criado nas configurações da conta;
- **não grave senhas ou tokens em arquivos do projeto**, nem os envie por mensagem. Credenciais pertencem ao gerenciador de credenciais.

Depois do `push`, atualize a página do repositório no GitHub: os arquivos, o `README.md`, a branch `main` e o histórico de commits estarão lá.

> O `commit` registra alterações **localmente**. O `push` é o que as compartilha.

---

## 5. Sincronizar alterações

Agora há dois lugares com o mesmo histórico: o seu repositório e o do GitHub. Sincronizar é mantê-los alinhados.

### Enviar uma alteração local

Edite o `README.md`, acrescentando uma seção:

```markdown
## Funcionalidades

- Cadastro de livros
- Cadastro de usuários
```

Prepare e registre o commit pelo Source Control (ou por `git add` e `git commit`) e envie:

```bash
git status -sb
```

```text
## main...origin/main [ahead 1]
```

`ahead 1` informa que a sua `main` está um commit à frente da `main` remota. Como o `-u` já estabeleceu o acompanhamento, basta:

```bash
git push
```

Recarregue o repositório no GitHub e o README aparece com a nova seção.

No VS Code, o botão **Sync Changes** executa um `pull` seguido de um `push`. Só o use depois de conhecer os dois comandos separados.

### Receber uma alteração feita no GitHub

Alterações no GitHub não chegam automaticamente ao seu computador. Para simular, abra o `README.md` no navegador, use o ícone de lápis, acrescente uma seção "Como executar" e confirme com **Commit changes**, direto na `main`. O GitHub propõe a mensagem `Update README.md`.

Seu repositório local ainda não sabe disso. Existem duas maneiras de buscar a novidade, e a diferença entre elas é importante.

### `git fetch`: buscar sem integrar

```bash
git fetch
```

```text
From https://github.com/SEU-USUARIO/gestao-biblioteca
   5b67b8b..a4cf836  main       -> origin/main
```

O `fetch` baixa os commits novos e atualiza as referências do tipo `origin/main`, que representam **o último estado conhecido do remoto**. A sua branch `main` e os seus arquivos **não mudam**. Agora você pode inspecionar com tranquilidade:

```bash
git status
```

```text
On branch main
Your branch is behind 'origin/main' by 1 commit, and can be fast-forwarded.
  (use "git pull" to update your local branch)

nothing to commit, working tree clean
```

```bash
git log --oneline --decorate --graph --all
```

```text
* a4cf836 (origin/main, origin/HEAD) Update README.md
* 5b67b8b (HEAD -> main) Lista funcionalidades no README
* cf34def Cria estrutura inicial da biblioteca
```

Veja `origin/main` à frente de `main`. Para ver o que mudou antes de integrar, use `git diff main origin/main`.

### `git pull`: buscar e integrar

```bash
git pull
```

```text
Updating 5b67b8b..a4cf836
Fast-forward
 README.md | 4 ++++
 1 file changed, 4 insertions(+)
```

O `pull` executa um `fetch` e depois integra o que veio à branch atual. Neste caso, o Git precisou apenas avançar a `main` até o commit novo (*fast-forward*), e o README local passou a ter a seção que você escreveu no navegador.

| `git fetch` | `git pull` |
|---|---|
| busca commits do remoto | busca commits do remoto |
| atualiza `origin/main` | atualiza `origin/main` |
| não altera sua branch nem seus arquivos | integra os commits à branch atual |
| seguro em qualquer momento | pode modificar arquivos ou gerar conflito |

### Quando o `push` é recusado

Se o remoto recebeu commits que você não possui e você tentar enviar os seus, o Git recusa:

```text
 ! [rejected]        main -> main (fetch first)
error: failed to push some refs to 'https://github.com/SEU-USUARIO/gestao-biblioteca.git'
hint: Updates were rejected because the remote contains work that you do not
hint: have locally. This is usually caused by another repository pushing to
hint: the same ref. If you want to integrate the remote changes, use
hint: 'git pull' before pushing again.
```

O Git protege o trabalho do remoto. Faça o que ele sugere: `git pull` (resolvendo um conflito, se houver) e depois `git push`.

Quando os dois lados têm commits diferentes, o `pull` pode exigir que você declare como combiná-los:

```text
fatal: Need to specify how to reconcile divergent branches.
```

Neste curso, combine por *merge*. Configure uma vez:

```bash
git config --global pull.rebase false
```

Estratégias alternativas de sincronização, como o *rebase*, ficam para o próximo tópico.

---

## 6. Branches: trabalhar sem desestabilizar a versão principal

Imagine que `gestao-biblioteca` já tenha uma versão funcional. Uma pessoa vai implementar a consulta de títulos e outra vai criar a renovação de empréstimos. Se ambas alterarem diretamente a `main`, ela passa a conter trabalho incompleto, e qualquer erro afeta quem só queria usar a versão estável.

A solução é a **branch**: uma linha independente de desenvolvimento com seu próprio histórico de commits. Alterações feitas nela não aparecem na `main` até que você decida integrá-las. No Git, uma branch é basicamente um ponteiro para um commit, por isso criar e descartar branches é barato.

### Convenções de nome

| Nome | Uso |
|---|---|
| `main` | versão principal do projeto |
| `feature/...` | desenvolvimento de funcionalidade, como `feature/obter-titulo` |
| `fix/...` | correção de defeito, como `fix/data-devolucao` |

Esses prefixos são combinações entre as pessoas, não regras do Git.

### Consultar, criar e trocar

```bash
git branch
```

lista as branches locais e marca a atual com `*`. Para criar uma nova branch e já passar a trabalhar nela:

```bash
git switch -c feature/obter-titulo
```

```text
Switched to a new branch 'feature/obter-titulo'
```

`switch` troca a branch atual. `-c` (*create*) cria a branch antes de trocar. Para voltar a uma branch existente, use `git switch main`. Você também pode trocar de branch pela barra de status do VS Code, clicando no nome da branch no canto inferior esquerdo.

Tutoriais mais antigos usam `git checkout -b nome` para o mesmo fim. O `git switch` foi criado para deixar a troca de branches mais clara.

### Desenvolver na branch

Dentro da nova branch, o trabalho segue como você já sabe. Na classe `Livro`, acrescente um método:

```java
public String obterTitulo() {
    return titulo;
}
```

Prepare e registre pelo Source Control com uma mensagem como `Adiciona obterTitulo em Livro`. Esse commit pertence à branch atual. A `main` continua sem ele.

### Publicar a branch

Uma branch criada localmente existe apenas no seu computador. Para disponibilizá-la no GitHub:

```bash
git push -u origin feature/obter-titulo
```

```text
To https://github.com/SEU-USUARIO/gestao-biblioteca.git
 * [new branch]      feature/obter-titulo -> feature/obter-titulo
branch 'feature/obter-titulo' set up to track 'origin/feature/obter-titulo'.
```

Agora a lista de branches do GitHub mostra duas linhas de desenvolvimento, `main` e `feature/obter-titulo`. No VS Code, uma branch ainda não enviada exibe o botão **Publish Branch**, equivalente a esse comando.

Para listar também as branches remotas conhecidas pelo seu repositório, use:

```bash
git branch -a
```

```text
* feature/obter-titulo
  main
  remotes/origin/HEAD -> origin/main
  remotes/origin/feature/obter-titulo
  remotes/origin/main
```

### Relação com o ciclo de manutenção em COBOL

Ao alterar um fonte COBOL, a versão de produção não é tocada até que a mudança seja testada. O ciclo de desenvolvimento, teste e promoção protege o que já funciona. A intenção é a mesma: isolar o trabalho em andamento.

A diferença está no mecanismo. Em muitas rotinas de mudança, a proteção vem de um processo e de ambientes separados. No Git, o isolamento é uma operação leve dentro do próprio repositório. Isso não substitui testes nem a disciplina de promoção da sua organização.

---

## 7. Integrar com `merge`

Quando a funcionalidade estiver pronta, é preciso levar suas alterações para a `main`. A operação é o **merge**. A regra mais importante é que você executa o merge **estando na branch que vai receber** as alterações.

```bash
git switch main
git merge feature/obter-titulo
```

```text
Switched to branch 'main'
Your branch is up to date with 'origin/main'.
```

```text
Updating a4cf836..a95d4a0
Fast-forward
 src/br/com/curso/biblioteca/dominio/Livro.java | 4 ++++
 1 file changed, 4 insertions(+)
```

A mensagem **Fast-forward** aparece quando a `main` não recebeu nenhum commit novo desde que a branch foi criada. O histórico continua sendo uma linha reta, então o Git simplesmente avança o ponteiro da `main`, sem criar commit novo.

Veja o resultado:

```bash
git log --oneline --graph --all --decorate
```

```text
* a95d4a0 (HEAD -> main, origin/feature/obter-titulo, feature/obter-titulo) Adiciona obterTitulo em Livro
* a4cf836 (origin/main, origin/HEAD) Update README.md
* 5b67b8b Lista funcionalidades no README
* cf34def Cria estrutura inicial da biblioteca
```

`--graph` desenha as linhas de desenvolvimento, `--all` considera todas as branches e `--decorate` mostra onde cada branch e cada `origin/...` aponta. Em seguida, compartilhe o resultado:

```bash
git push
```

```text
To https://github.com/SEU-USUARIO/gestao-biblioteca.git
   a4cf836..a95d4a0  main -> main
```

### Quando a `main` também avançou

Se a `main` recebeu commits enquanto a branch era desenvolvida, o histórico dos dois lados divergiu e o Git não pode apenas avançar o ponteiro. Ele cria um **commit de merge**, com dois pais, que une as duas linhas. O Git propõe uma mensagem como `Merge branch 'feature/...'` para você confirmar. Se as alterações tocaram regiões diferentes, tudo acontece sem intervenção. O problema só surge quando tocaram a mesma região.

### Limpeza

Depois do merge, a branch cumpriu seu papel e pode ser removida:

```bash
git branch -d feature/obter-titulo
```

```text
Deleted branch feature/obter-titulo (was a95d4a0).
```

O `-d` só apaga se a branch já foi integrada. A cópia remota é removida com `git push origin --delete feature/obter-titulo`. Os commits continuam na `main`.

---

## 8. Conflitos

Um **conflito** acontece quando o Git precisa combinar duas versões e não consegue decidir sozinho qual deve prevalecer. O caso típico é este: duas branches alteram **a mesma região do mesmo arquivo**, de maneiras diferentes.

Um conflito não indica que o projeto está perdido nem que alguém errou. O Git marca as regiões que precisam de decisão humana. Em linhas diferentes, ele combinaria tudo automaticamente.

### Provocar um conflito de propósito

Para conhecer o conflito num ambiente seguro, a receita exige que **as duas branches alterem exatamente a mesma linha** do README. A terceira linha do arquivo contém a descrição do projeto.

Parta da `main` limpa e crie a branch com a primeira versão da linha:

```bash
git switch -c feature/descricao-acervo
```

Edite a linha 3 do `README.md` para:

```text
Sistema para controlar o acervo e os empréstimos de uma biblioteca.
```

Registre e volte à `main`:

```bash
git commit -am "Descreve o foco em acervo e emprestimos"
git switch main
```

Agora, na `main`, altere **a mesma linha 3** para um texto diferente:

```text
Sistema para atender leitores e registrar devoluções de livros.
```

```bash
git commit -am "Descreve o foco em leitores e devolucoes"
```

(`-am` combina `-a`, que prepara arquivos já rastreados e modificados, com `-m`. Funciona apenas para arquivos que o Git já conhece.)

Integre:

```bash
git merge feature/descricao-acervo
```

```text
Auto-merging README.md
CONFLICT (content): Merge conflict in README.md
Automatic merge failed; fix conflicts and then commit the result.
```

### Ler o que o Git sinalizou

Execute `git status`. A saída informa `You have unmerged paths` e lista o arquivo:

```text
Unmerged paths:
  (use "git add <file>..." to mark resolution)
	both modified:   README.md
```

O arquivo aparece como `both modified`. Ao abri-lo, você encontra marcadores escritos pelo Git:

```text
# Gestão de Biblioteca

<<<<<<< HEAD
Sistema para atender leitores e registrar devoluções de livros.
=======
Sistema para controlar o acervo e os empréstimos de uma biblioteca.
>>>>>>> feature/descricao-acervo

## Funcionalidades
```

| Marcador | Significado |
|---|---|
| `<<<<<<< HEAD` | início da versão da branch em que você está (a "atual") |
| `=======` | separação entre as duas versões |
| `>>>>>>> feature/descricao-acervo` | fim da versão da outra branch (a "que chega") |

### Resolver pelo VS Code

O VS Code reconhece os marcadores e coloca botões acima do trecho em conflito:

- **Accept Current Change**: mantém a versão da branch atual;
- **Accept Incoming Change**: mantém a versão que chega;
- **Accept Both Changes**: mantém as duas, uma após a outra;
- **Compare Changes**: mostra as versões lado a lado.

Esses botões são **atalhos de edição**. Eles não decidem pelo que é correto. Em um README, aceitar ambas as versões pode produzir um texto contraditório. Em um código Java, pode produzir um método duplicado que nem compila. Quem decide é você, entendendo o que cada lado pretendia.

O resultado final pode ser uma das versões, as duas combinadas ou um texto novo. Neste caso, uma redação que une a intenção das duas:

```text
Sistema para controlar o acervo, atender leitores e registrar empréstimos e devoluções.
```

O arquivo final **não pode conter** nenhum dos marcadores `<<<<<<<`, `=======` ou `>>>>>>>`.

### Concluir o merge

Resolver o texto não encerra a operação. Você precisa informar ao Git que a resolução foi concluída e registrá-la:

```bash
git add README.md
git commit -m "Resolve conflito na descricao do projeto"
```

```text
[main 0be1c63] Resolve conflito na descricao do projeto
```

Confira o histórico:

```bash
git log --oneline --graph --all --decorate
```

```text
*   0be1c63 (HEAD -> main) Resolve conflito na descricao do projeto
|\  
| * 7e999e6 (feature/descricao-acervo) Descreve o foco em acervo e emprestimos
* | 1f870a4 Descreve o foco em leitores e devolucoes
|/  
* a95d4a0 (origin/main, origin/feature/obter-titulo, origin/HEAD) Adiciona obterTitulo em Livro
* a4cf836 Update README.md
* 5b67b8b Lista funcionalidades no README
* cf34def Cria estrutura inicial da biblioteca
```

O grafo mostra a divergência e o commit de merge, com dois pais, unindo as linhas. Faça `git push` para compartilhar o resultado.

Se você quiser desistir e voltar ao estado anterior ao merge, `git merge --abort` cancela a operação. Ele serve para desistir, não é a reação automática a um conflito: joga fora a tentativa de integração, e o conflito continuará esperando por você.

Quando houver código no meio, compile e execute o programa depois de resolver.

### Relação com COBOL

Quem já alterou um programa COBOL ao mesmo tempo que outra pessoa conhece o problema de duas mudanças sobre o mesmo trecho. A intenção é a mesma: combiná-las com cuidado. O Git detecta a sobreposição e marca no próprio arquivo o ponto exato que precisa de decisão, mas decidir continua sendo da pessoa.

---

## 9. Erros de compreensão que vale evitar

### “GitHub é o Git”

O Git é o sistema de versionamento, e funciona sem o GitHub. O GitHub hospeda repositórios e oferece colaboração.

### “O que alterei no GitHub aparece sozinho no meu computador”

Não aparece. É necessário buscar com `fetch` ou `pull`.

### “`fetch` e `pull` são a mesma coisa”

O `fetch` só atualiza o conhecimento sobre o remoto. O `pull` também integra à branch atual.

### “Crio o repositório no GitHub com README para já ter algo”

Para publicar um projeto com histórico local, o remoto deve nascer vazio. Um README criado no GitHub é um commit que o seu repositório desconhece, e o `push` será recusado.

### “Criar uma branch já a envia ao GitHub”

A criação é local. O `git push -u origin nome-da-branch` publica.

### “Conflito significa que o repositório quebrou”

Significa que há duas decisões sobre o mesmo trecho e o Git precisa que uma pessoa escolha.

### “Accept Both Changes resolve tudo”

É um atalho de edição. Sempre confira se o resultado faz sentido e, no caso de código, se compila.

### “Aborte o merge sempre que aparecer conflito”

O `git merge --abort` é para desistir da integração. A solução para um conflito é analisá-lo, resolvê-lo e registrar.

---

## 10. Checklist do Tópico 2

- [ ] Diferencio Git (sistema de versionamento) de GitHub (plataforma de hospedagem e colaboração).
- [ ] Sei que o VS Code possui integração Git nativa, sem extensão chamada Git.
- [ ] Reconheço Changes e Staged Changes no painel Source Control.
- [ ] Relaciono o botão **+** ao `git add` e o botão **Commit** ao `git commit`.
- [ ] Sei criar um repositório vazio no GitHub e explico por que ele deve nascer sem README.
- [ ] Não publico código corporativo nem compartilho credenciais.
- [ ] Sei o que é `origin` e conecto o local ao remoto com `git remote add`.
- [ ] Confiro o remote com `git remote -v`.
- [ ] Executo `git push -u origin main` e explico o papel do `-u`.
- [ ] Entendo a autenticação pelo navegador e que a senha da conta não vale para o Git.
- [ ] Diferencio `fetch` de `pull`.
- [ ] Interpreto `ahead` e `behind` em `git status -sb`.
- [ ] Sei o que fazer quando o `push` é recusado.
- [ ] Explico para que serve uma branch e conheço as convenções `feature/` e `fix/`.
- [ ] Crio e troco de branch com `git switch` e `git switch -c`.
- [ ] Publico uma branch com `git push -u origin nome`.
- [ ] Faço merge estando na branch que recebe as alterações.
- [ ] Explico a diferença entre fast-forward e commit de merge.
- [ ] Leio o grafo de `git log --oneline --graph --all --decorate`.
- [ ] Reconheço os marcadores `<<<<<<<`, `=======` e `>>>>>>>`.
- [ ] Resolvo um conflito, removo os marcadores e registro a resolução com `git add` e `git commit`.
- [ ] Sei quando usar `git merge --abort` e quando não usar.

---

## Conclusão

Neste tópico, o repositório deixou de ser apenas um histórico guardado no seu computador. Ele ganhou uma cópia remota no GitHub, um caminho para enviar e receber alterações e um modo de organizar o trabalho em branches.

Você separou Git e GitHub, conectou o projeto com `git remote add` e publicou a `main` com `git push -u origin main`. Aprendeu a sincronizar com `fetch` e `pull` e a ler `ahead` e `behind`. Com as branches, passou a desenvolver sem alterar a versão principal e, com o `merge`, a incorporar o que ficou pronto. Por fim, enfrentou o conflito, que deixou de ser um incidente e virou uma etapa previsível: ler os marcadores, decidir, remover, registrar.

A experiência do mainframe continua útil. A preocupação com proteger o que está em produção, isolar uma mudança até testá-la e combinar alterações concorrentes é a mesma. O Git oferece outros mecanismos para a mesma preocupação.

Até agora, porém, o histórico só cresceu: você ainda não sabe corrigir um commit, desfazer um erro já publicado ou atualizar uma branch sem um commit de merge. No próximo tópico, você aprenderá a **comparar** versões com precisão, **desfazer** alterações em cada nível e **reorganizar** commits com `rebase`. Depois, no Tópico 4, usará **Pull Requests** e **Code Review** para organizar a colaboração e conhecerá o **Maven** para estruturar projetos Java em equipe.
