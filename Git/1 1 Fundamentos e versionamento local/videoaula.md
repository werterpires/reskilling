# Aula 1 — Git: fundamentos e versionamento local

- Módulo: Git Básico → Intermediário + Maven
- Aula: 1 de 4
- Tema: Instalação, configuração e fundamentos do Git
- Formato: Apresentação + demonstração prática
- Ambiente: Windows + Git Bash
- Duração sugerida: 2 horas

**Objetivo geral:** capacitar o aluno a instalar e configurar o Git, criar um repositório local, acompanhar alterações nos arquivos, preparar modificações, registrar commits e consultar o histórico de versões.

**Projeto utilizado durante a aula:** Sistema Bancário (Java).

## 1. Introdução — Por que versionar código?

### 1.1. Apresentar o problema

- Como controlar as alterações realizadas em um projeto?
- O que acontece quando uma alteração compromete uma funcionalidade que anteriormente funcionava?
- Como recuperar uma versão anterior?
- Como identificar quais alterações foram realizadas?
- Como trabalhar com vários desenvolvedores modificando o mesmo projeto?

Exemplo para apresentar:

- Desenvolvedor implementa uma funcionalidade.
- Outra alteração modifica um comportamento que já funcionava.
- Surge um defeito.
- É necessário descobrir o que mudou e recuperar o estado anterior.

Apresentar soluções improvisadas:

- Cópias manuais de pastas.
- Arquivos com nomes como projeto_final, projeto_final_v2 e projeto_final_corrigido.
- Backups periódicos.
- Envio de arquivos por e-mail ou mensageiros.

Problemas dessas abordagens:

- Dificuldade de identificar a versão correta.
- Falta de histórico organizado.
- Risco de sobrescrever alterações.
- Dificuldade de identificar a autoria.
- Recuperação trabalhosa de versões anteriores.

### 1.2. Comparar o desenvolvimento tradicional e o moderno

Modelo tradicional de trabalho:

- Ambientes mais centralizados.
- Processos controlados de desenvolvimento e implantação.
- Ferramentas específicas para gerenciamento de código.
- Fluxos de trabalho frequentemente vinculados à infraestrutura corporativa.

Modelo moderno de desenvolvimento:

- Repositórios locais.
- Histórico de alterações.
- Desenvolvimento independente.
- Compartilhamento de código.
- Integração de alterações.
- Revisão de código entre desenvolvedores.

**Atenção:** não afirmar que mainframe não possui controle de versão. Existem ferramentas próprias para esse ecossistema. O objetivo é apresentar o Git como parte de um ambiente de desenvolvimento moderno.

### 1.3. Introduzir o Git

- Sistema distribuído de controle de versão.
- Registra alterações realizadas nos arquivos.
- Permite consultar o histórico do projeto.
- Permite recuperar estados anteriores.
- Cada repositório possui seu próprio histórico.
- Grande parte das operações pode ser realizada sem conexão com a internet.

Diferenciar:

- Git: ferramenta de controle de versão.
- GitHub: plataforma de hospedagem de repositórios Git e colaboração.

- Nesta aula: Git local.
- Na próxima aula: GitHub, branches e colaboração.

**Pergunta para a turma:** qual é a diferença entre armazenar uma cópia de um arquivo e registrar uma versão desse arquivo em um sistema de controle de versão?

## 2. Instalação do Git no Windows

> **Realizar junto com todos os alunos**

### 2.1. Preparação do ambiente

Antes de iniciar:

- Verificar se todos estão utilizando Windows.
- Verificar se possuem permissão para instalar programas.
- Verificar se já existe alguma instalação anterior do Git.
- Fechar terminais abertos que possam utilizar versões antigas.
- Orientar os alunos a acompanhar cada etapa.

**Site oficial:** https://git-scm.com/

### 2.2. Download

- Acessar o site oficial.
- Selecionar a opção de download para Windows.
- Baixar o instalador correspondente à arquitetura do computador.
- Executar o arquivo baixado.
- Caso apareça uma solicitação de permissão do Windows, autorizar a execução conforme as políticas do ambiente corporativo.

**Ponto de atenção:** em computadores corporativos, políticas de segurança podem impedir instalações. Se isso acontecer, verificar previamente com o suporte de TI, em vez de tentar contornar as restrições.

### 2.3. Percorrer as opções do instalador

Não avançar automaticamente pelas telas. Apresentar as opções relevantes:

| Configuração | Escolha recomendada |
| --- | --- |
| Select Components | Manter opções padrão |
| Default editor | VS Code, se instalado; caso contrário, editor padrão |
| Initial branch name | main |
| PATH environment | Git from the command line and also from 3rd-party software |
| SSH executable | Use bundled OpenSSH |
| HTTPS transport | Use the OpenSSL library |
| Line endings | Checkout Windows-style, commit Unix-style |
| Terminal emulator | Use MinTTY |
| Default behavior of git pull | Manter padrão |
| Credential helper | Git Credential Manager |
| Additional options | Manter recomendações |

### 2.4. Explicar as configurações importantes

**PATH:**

- Permite executar comandos Git em diferentes terminais.
- Facilita a integração com IDEs e outras ferramentas.
- Escolher a opção que disponibiliza Git na linha de comando e em programas externos.

**Editor padrão:**

- Utilizado pelo Git quando precisa abrir um editor para mensagens ou outras operações.
- Selecionar VS Code caso esteja instalado.
- Evitar que o aluno fique preso em um editor desconhecido.

**Initial branch name:**

- Define o nome inicial da branch principal.
- Utilizar main para acompanhar o padrão que será trabalhado no curso.

**Line endings:**

- Windows tradicionalmente utiliza CRLF.
- Sistemas Unix e Linux utilizam LF.
- A configuração recomendada evita diferenças desnecessárias entre ambientes.

**Credential Manager:**

- Ajuda a armazenar e gerenciar credenciais.
- Será útil quando o aluno começar a interagir com repositórios remotos.

**Observação:** não aprofundar SSH, autenticação ou configuração de chaves nesta aula.

### 2.5. Finalizar e verificar a instalação

- Concluir a instalação.
- Abrir o Git Bash pelo menu Iniciar.

Executar:

```bash
git --version
```

**Resultado esperado:** apresentação da versão instalada.

Demonstrar também:

- Como abrir o Git Bash.
- Como utilizar o terminal.
- Diferença entre Git Bash, PowerShell e Prompt de Comando.
- Como navegar entre diretórios.

Se houver problemas:

- Fechar e abrir novamente o terminal.
- Verificar se a instalação foi concluída.
- Conferir se o Git foi adicionado ao PATH.
- Repetir git --version.

**Critério de conclusão:** todos os alunos devem conseguir executar um comando Git no terminal.

## 3. Configuração inicial do Git

### 3.1. Identidade do desenvolvedor

Explicar que cada commit possui informações sobre seu autor.

Configurar nome e e-mail:

```bash
git config --global user.name "Nome do Aluno"
git config --global user.email "email@exemplo.com"
```

Explicar os parâmetros:

- `git config`: consulta ou modifica configurações.
- `user.name`: nome associado aos commits.
- `user.email`: endereço de e-mail associado aos commits.
- `--global`: aplica a configuração a todos os repositórios do usuário.

Pontos importantes:

- A configuração global é um padrão para o computador.
- É possível configurar outra identidade dentro de um repositório específico.
- O e-mail utilizado pode ser posteriormente associado à conta GitHub.
- Não confundir identidade do commit com autenticação em serviços remotos.

### 3.2. Verificar as configurações

Executar:

```bash
git config --list
```

Conferir:

- `user.name`
- `user.email`
- `core.editor`, caso configurado.
- `init.defaultBranch`, caso configurado.

Demonstração adicional:

```bash
git config --global --get user.name
git config --global --get user.email
```

**Critério de conclusão:** cada aluno deve ter nome e e-mail configurados corretamente.

## 4. Criando o primeiro repositório

### 4.1. Criar o diretório do projeto

**Projeto utilizado durante o módulo:** sistema-bancario.

Navegar até a pasta de documentos:

```bash
cd /c/Users/Usuario/Documents
```

Criar o diretório:

```bash
mkdir sistema-bancario
cd sistema-bancario
```

Explicar:

- `cd`: navegação entre diretórios.
- `mkdir`: criação de diretórios.
- Caminhos no Git Bash utilizam uma representação diferente da habitual no Windows.
- `/c/Users/...` corresponde a `C:\Users\....`

### 4.2. Inicializar o repositório

Executar:

```bash
git init
```

Explicar:

- Inicializa um repositório Git no diretório atual.
- Cria o diretório oculto .git.
- A pasta .git armazena os dados internos do repositório.
- O diretório de trabalho continua sendo a pasta normal do projeto.

Verificar:

```bash
ls -la
```

Mostrar a presença da pasta .git.

**Atenção:** não excluir ou modificar manualmente a pasta .git. Ela contém as informações necessárias ao funcionamento do repositório.

### 4.3. Conceito de repositório

Reforçar:

- Uma pasta comum não é necessariamente um repositório.
- `git init` inicializa o controle de versão.
- Um projeto pode conter vários arquivos, mas todos pertencem ao mesmo repositório.
- O Git não é exclusivo de Java: também versiona documentos, configurações, scripts e outros tipos de arquivo.

## 5. Criando os primeiros arquivos

Criar os arquivos na raiz do projeto.

**README.md**

```markdown
# Sistema Bancário

Projeto desenvolvido durante o curso de Git.

## Funcionalidades

- Cadastro de clientes
- Consulta de saldo
- Transferência entre contas
```

**Conta.java**

```java
public class Conta {
    private String titular;
    private double saldo;

    public Conta(String titular, double saldo) {
        this.titular = titular;
        this.saldo = saldo;
    }

    public void depositar(double valor) {
        saldo += valor;
    }
}
```

Explicar:

- README apresenta informações sobre o projeto.
- Conta.java representa uma classe Java.
- O conteúdo ainda não foi registrado no histórico.
- Criar um arquivo dentro do repositório não significa que ele já está versionado.

Pergunta para a turma: se desligarmos o computador agora, o arquivo continuará existindo? E o histórico de alterações já terá sido criado?

## 6. Os três estados fundamentais do Git

> **Conceito central da aula**

Apresentar o fluxo:

Working Directory → Staging Area → Repository

### 6.1. Working Directory

- Diretório de trabalho.
- Local onde os arquivos são criados, modificados e excluídos.
- Alterações podem existir sem estarem preparadas para commit.

### 6.2. Staging Area

- Área de preparação.
- Recebe as alterações selecionadas para o próximo commit.
- Permite selecionar arquivos individualmente.
- Possibilita organizar diferentes alterações em commits separados.

### 6.3. Repository

- Área que contém o histórico versionado.
- Armazena os commits.
- Permite consultar registros anteriores.

Reforçar as diferenças:

- Salvar um arquivo não cria um commit.
- `git add` não cria um commit.
- `git commit` registra as alterações preparadas.
- O Git não registra automaticamente todas as modificações.

Sugestão visual: desenhar as três áreas no quadro e representar o caminho de um arquivo modificado até seu registro definitivo.

## 7. Acompanhando alterações — git status

Executar:

```bash
git status
```

Explicar o resultado inicial:

- Arquivos não rastreados (untracked).
- Arquivos que ainda não fazem parte do histórico.
- Orientação apresentada pelo próprio Git sobre como adicioná-los.

### 7.1. Preparar arquivos

Adicionar individualmente:

```bash
git add README.md
git add Conta.java
```

Ou adicionar todas as alterações:

```bash
git add .
```

Executar novamente:

```bash
git status
```

Demonstrar:

- Mudança de untracked para staged.
- Diferença entre adicionar um arquivo e adicionar todos.
- Importância de conferir o status antes de criar um commit.

**Ponto de atenção:** `git add .` adiciona alterações do diretório atual e seus subdiretórios. É importante conferir o que será incluído antes de utilizá-lo.

## 8. Registrando alterações — git commit

### 8.1. Criar o primeiro commit

```bash
git commit -m "Cria estrutura inicial do sistema bancario"
```

### 8.2. Explicar a estrutura de um commit

- Identificador único (hash).
- Autor.
- Data.
- Mensagem descritiva.
- Conteúdo registrado.

### 8.3. Boas práticas de mensagens

- Descrever a alteração realizada.
- Utilizar mensagens objetivas.
- Evitar mensagens genéricas como "alterações", "teste" e "versão final".
- Separar mudanças independentes em commits diferentes.

Exemplos:

```bash
git commit -m "Adiciona classe Conta"
git commit -m "Implementa metodo de deposito"
git commit -m "Atualiza documentacao do projeto"
```

Demonstração importante:

- Modificar Conta.java.
- Executar `git status`.
- Mostrar a alteração como modificada.
- Executar `git add Conta.java`.
- Modificar novamente o arquivo.
- Executar `git status`.
- Mostrar que a segunda alteração ainda não foi preparada.

**Conclusão:** o commit registra o conteúdo preparado na Staging Area, não todas as modificações existentes no diretório.

## 9. Consultando o histórico — git log

Executar:

```bash
git log
```

Identificar:

- Hash do commit.
- Autor.
- Data.
- Mensagem.

Apresentar versão resumida:

```bash
git log --oneline
```

Explicar:

- Cada linha representa um commit.
- O hash identifica o registro.
- O histórico permite compreender a evolução do projeto.
- As mensagens ajudam a localizar alterações específicas.

**Não aprofundar nesta aula:** reset, revert, cherry-pick e recuperação avançada de versões. O foco é compreender e consultar o histórico.

## 10. Comparando alterações — git diff

### 10.1. Demonstrar uma modificação

Adicionar à classe Conta:

```java
    public void sacar(double valor) {
        saldo -= valor;
    }
```

Executar:

```bash
git diff
```

Explicar:

- Compara alterações ainda não preparadas.
- Linhas adicionadas aparecem com `+`.
- Linhas removidas aparecem com `-`.
- Permite revisar modificações antes de registrá-las.

### 10.2. Comparar alterações preparadas

```bash
git add Conta.java
git diff --staged
```

Diferenciar:

- `git diff`: alterações fora da Staging Area.
- `git diff --staged`: alterações preparadas para o próximo commit.

Finalizar:

```bash
git commit -m "Adiciona metodo de saque"
```

**Ponto importante:** apresentar o diff como ferramenta de revisão, não apenas de diagnóstico.

## 11. Ignorando arquivos — .gitignore

### 11.1. Apresentar o problema

Nem todos os arquivos devem ser versionados.

Exemplos:

- Arquivos temporários.
- Arquivos compilados.
- Logs.
- Diretórios de build.
- Configurações locais de ferramentas.

### 11.2. Criar o arquivo

Na raiz do projeto, criar `.gitignore`:

```gitignore
# Arquivos de compilacao
*.class

# Diretorios de build
target/
out/

# Arquivos de log
*.log

# Configuracoes locais
.vscode/
```

### 11.3. Explicar as regras

- `*.class`: ignora arquivos com extensão .class.
- `target/`: ignora o diretório target.
- `out/`: ignora o diretório out.
- `*.log`: ignora arquivos de log.
- `.vscode/`: ignora configurações locais do VS Code.

### 11.4. Pontos de atenção

- O .gitignore deve ficar na raiz do projeto, salvo necessidade de regras específicas em outros diretórios.
- As regras não removem arquivos que já estão sendo rastreados.
- O próprio .gitignore deve ser versionado.
- Não ignorar arquivos necessários para construir ou executar o projeto.
- As regras serão ampliadas conforme conhecermos Maven e a estrutura de projetos Java.

Registrar:

```bash
git add .gitignore
git commit -m "Adiciona regras de gitignore"
```

## 12. Exercício prático de consolidação

**Objetivo:** construir um histórico organizado de um pequeno projeto Java.

### Etapa 1 — Estrutura inicial

- Criar README.md.
- Criar Conta.java.
- Inicializar o repositório.
- Adicionar os arquivos.
- Criar o primeiro commit.

### Etapa 2 — Evolução do código

- Implementar método de depósito.
- Implementar método de saque.
- Consultar alterações com `git diff`.
- Registrar cada funcionalidade em um commit.

### Etapa 3 — Documentação

- Atualizar README.
- Acrescentar consulta de saldo às funcionalidades.
- Criar commit específico para a documentação.

### Etapa 4 — Organização

- Criar .gitignore.
- Ignorar arquivos .class, logs e diretórios de build.
- Registrar o arquivo.

### Etapa 5 — Conferência

```bash
git status
git log --oneline
```

Critérios de avaliação:

- Repositório inicializado corretamente.
- Commits com mensagens descritivas.
- Histórico contendo diferentes etapas.
- Alterações preparadas corretamente.
- .gitignore configurado.
- Nenhuma alteração pendente ao final.

## 13. Encerramento e transição para a aula 2

### 13.1. Revisão dos conceitos

Retomar oralmente:

- Diferença entre Git e GitHub.
- Diferença entre repositório local e remoto.
- Working Directory, Staging Area e Repository.
- Diferença entre `git add` e `git commit`.
- Função do `git status`.
- Consulta do histórico com `git log`.
- Comparação de alterações com `git diff`.
- Finalidade do .gitignore.

### 13.2. Verificar a compreensão

Perguntas rápidas:

- O que acontece quando executamos `git init`?
- Qual é a finalidade da Staging Area?
- Qual é a diferença entre `git add` e `git commit`?
- Como consultar os commits realizados?
- Como visualizar as alterações de um arquivo?
- Para que serve o .gitignore?
- O Git depende do GitHub para funcionar?

### 13.3. Preparação para a próxima aula

**Situação atual:** cada aluno possui um repositório local, mas ainda não compartilhou seu projeto com outros desenvolvedores.

**Próximo problema:** como disponibilizar o código para uma equipe e permitir que diferentes pessoas contribuam?

Antecipar os próximos assuntos:

- GitHub e repositórios remotos.
- Instalação/configuração da extensão Git do VS Code.
- Painel Source Control.
- Clonagem e sincronização.
- Branches.
- Merge e conflitos.

Verificações antes de encerrar:

- Todos instalaram o Git.
- Todos configuraram nome e e-mail.
- Todos criaram um repositório local.
- Todos realizaram commits.
- Todos conseguiram consultar o histórico.
- Todos criaram e versionaram um .gitignore.
- Todos possuem o projeto sistema-bancario salvo.

**Resultado esperado da aula:** o aluno consegue instalar e configurar o Git no Windows, criar um repositório local, acompanhar alterações, preparar arquivos, registrar commits, consultar o histórico e configurar regras básicas de .gitignore.

**Mensagem de transição:** nesta aula aprendemos a controlar as versões do nosso próprio código. Na próxima, vamos aprender a compartilhar esse código e trabalhar com outros desenvolvedores utilizando GitHub e VS Code.
