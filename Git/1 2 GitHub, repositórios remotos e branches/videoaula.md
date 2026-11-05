# Aula 2 — Git e GitHub: repositórios remotos e branches

- Módulo: Git Básico → Intermediário + Maven
- Aula: 2 de 4
- Tema: GitHub, VS Code, branches e integração de alterações
- Formato: Apresentação + demonstração prática
- Ambiente: Windows + Git Bash + VS Code + GitHub
- Duração sugerida: 2 horas

**Objetivo geral:** capacitar o aluno a conectar repositórios locais ao GitHub, utilizar a integração Git do VS Code, compartilhar alterações, trabalhar com branches e realizar merges, incluindo a resolução de conflitos.

**Projeto utilizado durante a aula:** Sistema Bancário (Java), desenvolvido na aula anterior.

## 1. Introdução — Do versionamento local à colaboração

### 1.1. Recuperar o conhecimento da aula anterior

Revisar rapidamente:

- O que é Git?
- O que é um repositório?
- Diferença entre Working Directory, Staging Area e Repository.
- Como preparar alterações com `git add`.
- Como registrar versões com `git commit`.
- Como consultar o histórico com `git log`.

Verificação inicial:

- Confirmar que os alunos possuem o projeto sistema-bancario.
- Executar `git status`.
- Executar `git log --oneline`.
- Verificar se os commits da aula anterior estão presentes.

### 1.2. Apresentar o novo problema

Até aqui, cada desenvolvedor possui seu próprio repositório local.

Questões para apresentar:

- Como disponibilizar o código para outros desenvolvedores?
- Como compartilhar alterações?
- Como obter atualizações realizadas por outras pessoas?
- Como permitir que vários desenvolvedores trabalhem no mesmo projeto?
- Como evitar que uma funcionalidade em desenvolvimento comprometa o código principal?

Introduzir a ideia de repositório remoto:

- Repositório hospedado em um servidor.
- Permite compartilhar o código.
- Possibilita sincronizar alterações.
- Facilita o trabalho colaborativo.

### 1.3. Diferenciar os conceitos

| Conceito | Significado |
| --- | --- |
| Git | Sistema de controle de versão |
| GitHub | Plataforma de hospedagem e colaboração |
| Local repository | Repositório existente no computador |
| Remote repository | Repositório hospedado remotamente |
| Push | Enviar commits para o remoto |
| Pull | Buscar e integrar atualizações do remoto |
| Clone | Criar uma cópia local de um repositório remoto |
| Branch | Linha independente de desenvolvimento |

**Conceito-chave:** Git continua sendo a ferramenta responsável pelo versionamento. GitHub oferece infraestrutura e funcionalidades para compartilhar e colaborar sobre os repositórios.

## 2. Conhecendo o VS Code e sua integração com Git

> **A partir desta aula, utilizar o VS Code como interface principal**

### 2.1. Abrir o projeto

- Abrir o VS Code.
- Selecionar File → Open Folder.
- Abrir a pasta sistema-bancario.
- Verificar se o VS Code reconhece automaticamente o repositório Git.

Demonstrar:

- Painel Explorer.
- Terminal integrado.
- Painel Source Control (atalho Ctrl + Shift + G).
- Indicador de alterações nos arquivos.

**Explicar:** O VS Code possui integração Git nativa.

- Não é necessário instalar uma extensão chamada simplesmente Git.
- O VS Code utiliza a instalação do Git existente no sistema.
- O painel Source Control permite executar operações de versionamento visualmente.

### 2.2. Verificar a integração

- Abrir Source Control.
- Conferir os arquivos modificados, se houver.
- Verificar se o VS Code reconheceu o repositório.
- Abrir o terminal integrado.
- Executar `git --version`.

Se o repositório não for reconhecido:

- Confirmar se a pasta aberta contém o diretório .git.
- Verificar se o Git está instalado.
- Conferir se o VS Code consegue localizar o executável Git.
- Reiniciar o VS Code, se necessário.

### 2.3. Apresentar o Source Control

Identificar os principais elementos:

- Changes: arquivos modificados ainda não preparados.
- Staged Changes: arquivos preparados para commit.
- Campo de mensagem: descrição do commit.
- Commit: registra as alterações preparadas.
- Menu de opções: operações adicionais do Git.

Demonstrar uma alteração:

- Modificar uma linha do README.
- Observar a mudança no Source Control.
- Clicar no arquivo para visualizar as diferenças.
- Clicar no sinal + para preparar o arquivo.
- Escrever uma mensagem.
- Realizar o commit.

Comparar com a aula anterior:

- Stage corresponde ao `git add`.
- Commit corresponde ao `git commit`.
- O painel apresenta visualmente os estados já estudados.

**Atenção:** não ensinar o aluno a depender exclusivamente da interface. Os comandos continuam sendo importantes para compreender o funcionamento e diagnosticar problemas.

### 2.4. Extensões complementares

Apresentar, se houver tempo:

- Git Graph: visualização gráfica do histórico e das branches.
- GitHub Pull Requests: integração com GitHub para trabalhar com Pull Requests, assunto da próxima aula.

Não confundir extensões complementares com a integração Git nativa do VS Code.

## 3. Conhecendo o GitHub

### 3.1. Acessar a plataforma

Site: https://github.com/

- Acessar o GitHub.
- Criar uma conta, caso o aluno ainda não possua.
- Realizar o login.
- Apresentar a página inicial.
- Mostrar o perfil do usuário.
- Identificar a área de repositórios.

Pontos de atenção:

- Verificar se a turma pode utilizar contas pessoais ou se existe uma política corporativa específica.
- Não utilizar credenciais compartilhadas.
- Não publicar código corporativo ou informações confidenciais em repositórios pessoais.
- Para esta aula, utilizar exclusivamente o projeto didático.

### 3.2. Criar o primeiro repositório remoto

No GitHub:

- Clicar em New repository.
- Nome: sistema-bancario.
- Descrição: Projeto desenvolvido durante o curso de Git.
- Selecionar Public ou Private conforme a orientação da turma.
- Não marcar Add a README.
- Não adicionar .gitignore.
- Não selecionar uma licença.
- Criar o repositório.

**Por que criar vazio?**

O projeto já possui arquivos, commits e histórico local. Criar outro README ou outros arquivos iniciais no remoto pode gerar históricos diferentes e exigir uma integração adicional desnecessária para esta demonstração.

### 3.3. Apresentar a estrutura do repositório

Mostrar:

- Nome e descrição.
- Visibilidade pública ou privada.
- Arquivos armazenados.
- Histórico de commits.
- Branch principal.
- Botão Code e opções de clonagem.

**Explicar:** o GitHub agora possui um repositório remoto, mas ainda não recebeu os arquivos do projeto local.

## 4. Conectando o repositório local ao GitHub

### 4.1. Entender a conexão

Apresentar os conceitos:

- Repositório local: projeto existente no computador.
- Repositório remoto: projeto criado no GitHub.
- Remote: referência utilizada pelo Git para identificar um repositório remoto.
- origin: nome convencionalmente utilizado para identificar o remoto principal.

### 4.2. Verificar a branch principal

No terminal integrado do VS Code:

```bash
git branch
```

Se necessário, padronizar o nome da branch principal:

```bash
git branch -M main
```

Explicar:

- `git branch`: lista as branches locais.
- `-M`: renomeia a branch, inclusive substituindo um nome existente quando necessário.
- `main`: nome convencional da branch principal.

### 4.3. Adicionar o endereço remoto

Copiar a URL HTTPS do repositório no GitHub.

Executar:

```bash
git remote add origin https://github.com/USUARIO/sistema-bancario.git
```

Explicar:

- `git remote`: gerencia referências remotas.
- `add`: adiciona uma referência.
- `origin`: nome atribuído ao remoto.
- URL: endereço do repositório no GitHub.

Conferir:

```bash
git remote -v
```

**Resultado esperado:** apresentação dos endereços utilizados para buscar e enviar alterações.

### 4.4. Enviar o projeto ao GitHub

Executar:

```bash
git push -u origin main
```

Explicar:

- `git push`: envia commits ao repositório remoto.
- `origin`: identifica o repositório de destino.
- `main`: identifica a branch que será enviada.
- `-u`: estabelece a referência de acompanhamento entre a branch local e a remota.

Autenticação:

- O Git Credential Manager pode abrir um fluxo de autenticação.
- O aluno poderá ser direcionado ao navegador para autorizar o acesso.
- Não solicitar que os alunos compartilhem senhas ou tokens.
- Não armazenar credenciais em arquivos do projeto.

Conferir no GitHub:

- Arquivos publicados.
- README.
- Classe Conta.
- Histórico de commits.
- Branch principal `main`.

**Conceito-chave:** o commit registra alterações localmente; o push compartilha os commits com o repositório remoto.

## 5. Sincronizando alterações

### 5.1. Entender push e pull

Push:

- Envia commits locais para o remoto.
- Atualiza a branch correspondente no GitHub.
- Não envia simplesmente arquivos isolados: envia objetos e histórico necessários aos commits.

Pull:

- Busca atualizações do remoto.
- Integra essas atualizações à branch local.
- Pode resultar em merge ou outra forma de integração, conforme a configuração.

### 5.2. Realizar uma alteração local

Modificar o README:

```markdown
## Objetivo

Sistema bancário desenvolvido para praticar Git e GitHub.

## Funcionalidades

- Cadastro de clientes
- Consulta de saldo
- Transferência entre contas
```

No VS Code:

- Abrir Source Control.
- Conferir a alteração.
- Preparar o arquivo.
- Criar o commit.

Enviar:

```bash
git push
```

**Demonstrar:** o GitHub apresenta a nova versão do README.

### 5.3. Realizar uma alteração pelo GitHub

- Abrir o README no navegador.
- Utilizar a opção de edição.
- Acrescentar uma informação à documentação.
- Criar um commit diretamente pelo GitHub.
- Voltar ao VS Code.

Executar:

```bash
git pull
```

**Conferir:** o README local deverá receber a alteração realizada no GitHub.

**Ponto importante:** alterações realizadas remotamente não aparecem automaticamente no computador. É necessário sincronizar o repositório.

### 5.4. Diferenciar fetch e pull

Apresentar:

```bash
git fetch
```

Fetch:

- Busca informações e commits do remoto.
- Atualiza as referências remotas.
- Não integra automaticamente as alterações à branch local.

```bash
git pull
```

Pull:

- Busca atualizações.
- Integra as alterações à branch atual.

**Não aprofundar:** estratégias avançadas de sincronização, rebase e resolução de divergências complexas.

## 6. Trabalhando com branches

> **Conceito central da segunda metade da aula**

### 6.1. Apresentar o problema

Imagine que o sistema bancário já possui uma versão funcional.

Um desenvolvedor precisa implementar a consulta de saldo. Outro precisa implementar transferências.

Se ambos modificarem diretamente a branch principal, podem introduzir alterações incompletas ou comprometer funcionalidades existentes.

Apresentar a solução:

- Criar branches.
- Desenvolver funcionalidades separadamente.
- Registrar commits independentes.
- Integrar as alterações quando estiverem prontas.

### 6.2. Conceito de branch

- Linha independente de desenvolvimento.
- Permite trabalhar em funcionalidades isoladas.
- Possui seu próprio histórico de commits.
- Pode receber alterações sem modificar imediatamente a branch principal.
- Posteriormente, pode ser integrada por meio de merge.

Explicar a convenção:

- `main`: branch principal.
- `feature/...`: branch destinada ao desenvolvimento de funcionalidades.
- `fix/...`: branch destinada à correção de defeitos.

Esses nomes são convenções, não exigências técnicas do Git.

### 6.3. Consultar branches

```bash
git branch
```

Explicar:

- Lista as branches locais.
- O asterisco identifica a branch atual.

### 6.4. Criar uma branch

Demonstrar:

```bash
git switch -c feature/consulta-saldo
```

Explicar:

- `switch`: altera a branch atual.
- `-c`: cria uma nova branch e já muda para ela.
- `feature/consulta-saldo`: nome da nova branch.

Conferir:

```bash
git branch
```

### 6.5. Desenvolver na nova branch

Modificar Conta.java:

```java
    public double consultarSaldo() {
        return saldo;
    }
```

No VS Code:

- Abrir Source Control.
- Conferir a alteração.
- Preparar o arquivo.

Criar um commit:

Implementa consulta de saldo

**Explicar:** esse commit pertence à branch atual. A branch principal ainda não recebeu essa implementação.

### 6.6. Publicar a branch no GitHub

Executar:

```bash
git push -u origin feature/consulta-saldo
```

Demonstrar no GitHub:

- A nova branch aparece na lista de branches.
- A branch main continua existindo.
- Os commits da funcionalidade estão associados à nova branch.

**Conceito-chave:** uma branch local não é automaticamente publicada no GitHub. O push é necessário para disponibilizá-la remotamente.

## 7. Integrando alterações com merge

### 7.1. Apresentar o conceito

Merge é a operação que integra alterações de uma branch a outra.

Exemplo:

- main: versão principal.
- feature/consulta-saldo: desenvolvimento de uma funcionalidade.
- Merge: integração da funcionalidade à branch principal.

**Explicar:** o merge deve ser executado estando na branch que receberá as alterações.

### 7.2. Realizar um merge simples

Primeiro, retornar à branch principal:

```bash
git switch main
```

Conferir:

```bash
git status
```

Integrar a funcionalidade:

```bash
git merge feature/consulta-saldo
```

Observar:

- O Git identifica os commits da branch de funcionalidade.
- Integra as alterações.
- Pode realizar um fast-forward quando o histórico permite.
- Em outras situações, pode criar um commit de merge.

Conferir:

```bash
git log --oneline --graph --all
```

Explicar:

- `--graph`: representação visual das linhas de desenvolvimento.
- `--all`: considera todas as branches.
- Permite visualizar a evolução do histórico.

Enviar a atualização:

```bash
git push
```

**Conferir no GitHub:** a funcionalidade agora também está disponível na branch principal.

## 8. Resolvendo conflitos

### 8.1. Apresentar o problema

Um conflito acontece quando o Git não consegue integrar automaticamente determinadas alterações.

Exemplo:

- Dois desenvolvedores modificam a mesma região de um arquivo.
- As alterações são diferentes.
- O Git não consegue determinar sozinho qual versão deve prevalecer.

Explicar:

- Conflito não significa necessariamente que o projeto está perdido.
- O Git sinaliza as regiões que precisam de decisão humana.
- O desenvolvedor analisa as alternativas e define o resultado correto.

### 8.2. Preparar uma situação de conflito

Utilizar o arquivo README.md.

Na branch main, modificar a descrição do projeto para:

Sistema bancário corporativo para gerenciamento de contas.

Registrar:

```bash
git add README.md
git commit -m "Atualiza descricao principal do projeto"
```

Criar uma nova branch:

```bash
git switch -c feature/descricao-alternativa
```

Modificar exatamente a mesma linha para:

Sistema bancário para gerenciamento financeiro de clientes.

Registrar:

```bash
git add README.md
git commit -m "Atualiza descricao do sistema"
```

Retornar à principal:

```bash
git switch main
```

Modificar a mesma linha novamente, com outro conteúdo:

Sistema bancário para operações de contas e transferencias.

Registrar:

```bash
git add README.md
git commit -m "Reformula descricao do sistema"
```

### 8.3. Provocar o conflito

Executar:

```bash
git merge feature/descricao-alternativa
```

**Resultado esperado:** o Git informa que existe um conflito no README.

### 8.4. Resolver pelo VS Code

- Abrir o arquivo em conflito.

Identificar os marcadores:

```text
<<<<<<< HEAD
Sistema bancário para operações de contas e transferencias.
=======
Sistema bancário para gerenciamento financeiro de clientes.
>>>>>>> feature/descricao-alternativa
```

Explicar:

- `<<<<<<< HEAD`: início da versão atual da branch.
- `=======`: separação entre as versões.
- `>>>>>>>`: identificação da outra branch.

No VS Code, apresentar as opções:

- Accept Current Change.
- Accept Incoming Change.
- Accept Both Changes.
- Compare Changes.

**Atenção:** essas opções são atalhos para selecionar conteúdo. O desenvolvedor precisa compreender o código e decidir qual resultado é correto. Aceitar ambas nem sempre faz sentido.

### 8.5. Finalizar a resolução

Escolher uma descrição final coerente e remover os marcadores.

**Exemplo:** Sistema bancário para gerenciamento de contas e operações financeiras.

Depois:

```bash
git add README.md
git commit -m "Resolve conflito na descricao do projeto"
```

Conferir:

```bash
git status
git log --oneline --graph --all
```

Pontos importantes:

- Não deixar marcadores de conflito no arquivo.
- Conferir o resultado final.
- Executar testes quando houver código envolvido.
- Um conflito resolvido precisa ser registrado.
- Não utilizar `git merge --abort` como solução automática; esse comando cancela a operação de merge quando se deseja desistir dela.

## 9. Exercício prático de consolidação

**Objetivo:** trabalhar com GitHub, branches e integração de alterações utilizando o projeto Sistema Bancário.

### Etapa 1 — Publicação

- Criar repositório no GitHub.
- Conectar o repositório local.
- Publicar a branch main.
- Conferir os arquivos no navegador.

### Etapa 2 — Sincronização

- Alterar o README localmente.
- Criar commit pelo VS Code.
- Executar push.
- Realizar uma alteração pelo GitHub.
- Executar pull.

### Etapa 3 — Branches

- Criar feature/cadastro-cliente.
- Implementar uma funcionalidade simples.
- Criar commit.
- Publicar a branch no GitHub.

### Etapa 4 — Merge

- Retornar à branch main.
- Integrar a funcionalidade.
- Conferir o histórico.
- Enviar as alterações ao GitHub.

### Etapa 5 — Conflito

- Criar duas branches com alterações na mesma linha do README.
- Modificar a mesma região de maneiras diferentes.
- Tentar integrar as branches.
- Identificar o conflito.
- Resolver pelo VS Code.
- Registrar a resolução.

### Critérios de conclusão

- Repositório local conectado ao GitHub.
- Projeto publicado.
- Operações de push e pull executadas.
- Integração Git do VS Code utilizada.
- Branch criada e publicada.
- Merge realizado.
- Conflito provocado e resolvido.
- Histórico conferido.

## 10. Encerramento e transição para a aula 3

### 10.1. Revisão dos conceitos

Retomar:

- Git e GitHub.
- Repositórios locais e remotos.
- Integração Git do VS Code.
- Source Control.
- Push, pull e fetch.
- Branches.
- Merge.
- Conflitos.

### 10.2. Apresentar o próximo desafio

Até aqui, aprendemos a trabalhar com código versionado, compartilhar alterações e desenvolver funcionalidades em branches.

Entretanto, ainda precisamos estabelecer um processo organizado para que as alterações sejam analisadas antes de integrar o código à branch principal.

Próxima aula:

- Fluxo colaborativo.
- Feature branches.
- Pull Requests.
- Code Review.
- Issues.
- Introdução ao Maven.
- Estrutura de projetos Java com pom.xml.

### 10.3. Verificações finais

- Todos possuem acesso ao GitHub.
- Todos conseguem utilizar o Source Control.
- Todos publicaram o projeto.
- Todos trabalharam com branches.
- Todos realizaram merge.
- Todos compreenderam a resolução de conflitos.
- Todos mantêm o projeto sistema-bancario atualizado.

**Resultado esperado:** o aluno consegue utilizar o Git integrado ao VS Code, conectar projetos ao GitHub, sincronizar repositórios, criar e publicar branches, integrar alterações com merge e resolver conflitos simples.

**Mensagem de transição:** agora sabemos desenvolver e integrar funcionalidades. Na próxima aula, vamos estabelecer um fluxo de colaboração profissional, utilizando Pull Requests e Code Review, antes de começar a estruturar nossos projetos Java com Maven.

## Notas para a condução

**Observação para sua preparação:** na prática de conflito, mantenha a alteração na mesma linha do README nas duas branches. Se forem linhas diferentes, o Git provavelmente fará o merge automaticamente, e a demonstração não atingirá o objetivo.
