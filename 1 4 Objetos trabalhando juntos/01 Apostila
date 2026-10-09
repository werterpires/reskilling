# Tópico 4 — Fazer objetos trabalharem juntos

## O sistema deixa de ser um conjunto de objetos isolados

Nos tópicos anteriores, você preparou o ambiente Java, começou a transição do pensamento procedural para a orientação a objetos, criou classes e deu comportamento aos objetos.

Na aplicação de Gestão de Biblioteca, classes como `Livro`, `Usuario`, `Emprestimo` e `Biblioteca` passaram a representar conceitos diferentes. Os atributos registraram estado, enquanto os métodos protegeram regras como emprestar, renovar e devolver um livro.

Agora chegamos ao último passo desta introdução: fazer esses objetos trabalharem juntos.

Uma aplicação orientada a objetos não é formada por uma classe gigantesca, mas também não é formada por dezenas de objetos completamente isolados. Os objetos precisam se relacionar, formar conjuntos e participar de fluxos coordenados.

Em uma biblioteca:

- um empréstimo relaciona um usuário a um item do acervo;
- uma biblioteca administra vários usuários e vários itens;
- livros e revistas possuem diferenças, mas participam do mesmo processo de empréstimo;
- uma operação pode trabalhar com o tipo geral `ItemAcervo` sem precisar conhecer antecipadamente todos os tipos específicos;
- diferentes classes podem assumir o mesmo papel por meio de uma interface.

É nesse ponto que a orientação a objetos começa a mostrar sua força de organização. O objetivo não é transformar um programa simples em uma estrutura complicada. É permitir que o sistema cresça mantendo responsabilidades compreensíveis.

Você já conhece processos extensos, registros com várias ocorrências, chamadas entre rotinas e fluxos que tratam diferentes situações. Essa experiência continua útil. A mudança é passar de um fluxo que conhece todos os detalhes para uma colaboração na qual cada objeto conhece sua parte.

Neste tópico, estudaremos:

- relacionamentos entre objetos;
- coleções;
- herança;
- sobrescrita de métodos;
- polimorfismo;
- interfaces, como extensão final;
- consolidação do pensamento orientado a objetos.

Cada conceito será apresentado antes de aparecer na versão consolidada do exemplo.

---

## 1. De referências isoladas para relacionamentos

No Tópico 2, vimos que uma variável de tipo por referência pode apontar para um objeto:

```java
Livro livro = new Livro();
Usuario usuario = new Usuario();
```

Esses objetos existem, mas ainda não há uma ligação entre eles. Quando um empréstimo é realizado, precisamos representar uma informação essencial: **qual usuário está com qual livro?**

Uma classe pode usar outra classe como tipo de atributo:

```java
public class Emprestimo {

    private Usuario usuario;
    private Livro livro;
}
```

Esse é um relacionamento entre objetos. O objeto `Emprestimo` não guarda uma cópia completa dos dados do usuário e do livro. Ele guarda referências para os objetos relacionados.

Podemos registrar a relação por meio de um método:

```java
public void registrar(Usuario usuario, Livro livro) {
    this.usuario = usuario;
    this.livro = livro;
}
```

Depois de uma chamada como:

```java
emprestimo.registrar(usuario, livro);
```

o empréstimo consegue solicitar comportamentos aos objetos relacionados:

```java
public String obterResumo() {
    return usuario.obterNome() + " está com " + livro.obterTitulo();
}
```

Observe a colaboração:

1. `Emprestimo` conhece o usuário relacionado;
2. solicita ao usuário seu nome;
3. conhece o livro relacionado;
4. solicita ao livro seu título;
5. monta o resumo do empréstimo.

`Emprestimo` não precisa acessar atributos privados de outras classes. Ele usa os métodos públicos que essas classes oferecem.

### Relacionar objetos não é copiar campos

Uma alternativa problemática seria colocar dentro de `Emprestimo`:

```java
private String matriculaUsuario;
private String nomeUsuario;
private String isbnLivro;
private String tituloLivro;
```

Em alguns sistemas, guardar dados históricos copiados pode ser uma decisão consciente. Porém, se o objetivo é representar o relacionamento atual, repetir todos os campos cria duplicação. Uma alteração no nome do usuário poderia deixar valores diferentes espalhados pelo sistema.

Usar os próprios tipos comunica melhor o modelo:

```java
private Usuario usuario;
private Livro livro;
```

O código passa a dizer que um empréstimo possui relação com um usuário e um livro.

### Relação com estruturas COBOL

Em COBOL, você pode trabalhar com chaves, índices, áreas de comunicação e registros que conectam informações de arquivos diferentes. Também pode reunir grupos relacionados dentro de uma estrutura maior.

A ponte é útil: o sistema precisa representar relações independentemente da linguagem. Em Java, porém, uma referência pode apontar diretamente para outro objeto, e a colaboração ocorre por chamadas como:

```java
emprestimo.obterUsuario().obterNome();
```

Não trate uma referência Java como se fosse apenas um campo textual contendo uma chave. A referência permite alcançar o próprio objeto e solicitar seus comportamentos.

---

## 2. Quantos objetos participam do relacionamento?

Relacionamentos podem envolver quantidades diferentes.

### Um objeto relacionado a outro

Um empréstimo possui um usuário responsável:

```java
private Usuario usuario;
```

Em determinado objeto `Emprestimo`, essa referência aponta para um objeto `Usuario`.

### Um objeto relacionado a vários

Uma biblioteca administra vários itens do acervo. Um único atributo `Livro` não representa essa necessidade:

```java
private Livro livro;
```

Precisamos de uma estrutura capaz de reunir muitos objetos. É aqui que entram as coleções.

Antes disso, guarde duas perguntas simples para modelar relacionamentos:

1. **Qual conceito conhece qual conceito?**
2. **Trata-se de um objeto ou de vários objetos?**

Para o exemplo:

| Classe | Relacionamento |
|---|---|
| `Emprestimo` | um `Usuario` e um item do acervo |
| `Biblioteca` | vários itens, usuários e empréstimos |
| `Livro` | seus próprios dados e comportamentos |
| `Usuario` | seus próprios dados e situação |

Nem toda relação precisa existir nos dois sentidos. Se `Emprestimo` conhece `Usuario`, não é obrigatório que `Usuario` mantenha uma coleção de todos os empréstimos. Essa decisão depende das operações necessárias. Acrescentar relações sem necessidade aumenta o estado que precisa ser mantido consistente.

---

## 3. Coleções: vários objetos sob uma mesma responsabilidade

Uma **coleção** é uma estrutura usada para reunir objetos. A biblioteca precisa manter vários itens do acervo:

```java
private List<Livro> livros;
```

`List` representa uma lista ordenada que aceita elementos repetidos. O trecho `<Livro>` informa o tipo dos elementos aceitos.

Para criar uma lista, podemos usar `ArrayList`:

```java
private List<Livro> livros = new ArrayList<>();
```

São necessários imports:

```java
import java.util.ArrayList;
import java.util.List;
```

Leia a declaração por partes:

| Trecho | Significado |
|---|---|
| `List<Livro>` | tipo da variável: lista de livros |
| `livros` | nome do atributo |
| `new ArrayList<>()` | criação do objeto que implementa a lista |

Neste momento, não precisamos aprofundar todas as diferenças entre `List` e `ArrayList`. Use a leitura inicial:

- `List` expressa o tipo geral da coleção;
- `ArrayList` é a implementação concreta criada.

Essa separação já prepara o terreno para interfaces, que serão apresentadas mais adiante.

### Adicionar elementos

O método `add` inclui um objeto:

```java
livros.add(livro);
```

Uma classe `Biblioteca` pode oferecer uma operação mais significativa:

```java
public void adicionarLivro(Livro livro) {
    livros.add(livro);
}
```

A aplicação solicita:

```java
biblioteca.adicionarLivro(livro);
```

`Biblioteca` controla a coleção. A aplicação não precisa manipular diretamente o atributo privado.

### Consultar tamanho e posição

```java
int quantidade = livros.size();
Livro primeiroLivro = livros.get(0);
```

`size()` devolve a quantidade de elementos. `get(0)` devolve o primeiro elemento, pois as posições começam em zero.

Se a lista tiver três elementos, as posições serão `0`, `1` e `2`. Tentar acessar uma posição inexistente provoca erro durante a execução. Por isso, não se deve usar `get` com um índice arbitrário sem garantir que ele existe.

### Coleção e `OCCURS`

Em COBOL, `OCCURS` permite declarar várias ocorrências de uma estrutura:

```cobol
05 WS-LIVRO OCCURS 100 TIMES.
   10 WS-LIVRO-CODIGO PIC X(10).
   10 WS-LIVRO-TITULO PIC X(100).
```

Uma `List<Livro>` também reúne várias ocorrências, mas não é uma tradução de `OCCURS`. Algumas diferenças iniciais:

| `OCCURS` | `List` |
|---|---|
| quantidade frequentemente definida na estrutura | tamanho pode crescer durante a execução |
| ocorrências pertencem à tabela declarada | elementos são referências para objetos |
| acesso costuma usar índice | oferece métodos como `add`, `get`, `size` e `remove` |
| descrição dos campos fica na própria estrutura | cada elemento já é um objeto de uma classe |

A experiência com tabelas ajuda a compreender conjuntos e percursos. A novidade é trabalhar com uma estrutura pronta da biblioteca Java que reúne referências tipadas.

---

## 4. Percorrendo uma coleção

Quando precisamos executar uma operação para cada elemento, podemos usar o `for` aprimorado:

```java
for (Livro livro : livros) {
    System.out.println(livro.obterTitulo());
}
```

Leia:

> Para cada objeto `Livro` da coleção `livros`, coloque a referência atual na variável `livro` e execute o bloco.

Esse `for` não apresenta explicitamente um contador. Ele é adequado quando o objetivo é percorrer os elementos, e não trabalhar com suas posições.

Em COBOL, uma tabela pode ser percorrida com `PERFORM VARYING` e um índice. A intenção continua conhecida: visitar várias ocorrências. Em Java, o `for` aprimorado esconde o controle de posição quando ele não é importante para a regra.

### Buscar um livro

Uma busca pode percorrer a lista e comparar o código:

```java
public Livro buscarPorCodigo(String codigo) {
    for (Livro livro : livros) {
        if (livro.possuiCodigo(codigo)) {
            return livro;
        }
    }

    return null;
}
```

O método examina os livros. Quando encontra um objeto compatível, devolve a referência imediatamente. Se terminar o percurso sem encontrar, devolve `null`.

`null` significa ausência de referência. Antes de chamar um método sobre o resultado, a aplicação precisa verificar:

```java
Livro encontrado = biblioteca.buscarPorCodigo("L001");

if (encontrado != null) {
    System.out.println(encontrado.obterTitulo());
} else {
    System.out.println("Livro não encontrado");
}
```

Mais adiante, APIs modernas podem representar ausência de outras formas, como `Optional`. Isso não será necessário nesta introdução.

### A coleção deve ficar encapsulada

Evite devolver a lista apenas para permitir que qualquer classe altere o acervo livremente:

```java
public List<Livro> obterLivros() {
    return livros;
}
```

Esse retorno expõe a coleção mutável. Para este estágio, prefira oferecer operações que expressem necessidades reais:

```java
adicionarLivro(livro)
buscarPorCodigo(codigo)
exibirAcervo()
obterQuantidadeLivros()
```

O encapsulamento também se aplica às coleções.

---

## 5. Quando os itens compartilham características

A biblioteca não possui apenas livros. Ela pode administrar revistas. Ambos possuem:

- código;
- título;
- disponibilidade;
- possibilidade de empréstimo;
- prazo de empréstimo.

Também possuem diferenças:

- `Livro` pode ter ISBN e autor;
- `Revista` pode ter número da edição;
- o prazo permitido pode variar.

Uma primeira solução seria duplicar todos os atributos e métodos em duas classes. Funcionaria, mas criaria repetição. Uma mudança na regra comum precisaria ser feita em vários lugares.

A **herança** permite declarar uma classe com características compartilhadas e classes mais específicas que a estendem.

```text
ItemAcervo
├── Livro
└── Revista
```

`ItemAcervo` representa o conceito geral. `Livro` e `Revista` representam especializações.

### Relação “é um”

Uma pergunta útil é:

- um livro **é um** item do acervo? Sim.
- uma revista **é um** item do acervo? Sim.
- uma biblioteca **é um** livro? Não.

Herança representa uma relação de especialização, frequentemente lida como “é um”. Para “tem um” ou “possui vários”, usamos relacionamentos e coleções.

| Afirmação | Modelo adequado |
|---|---|
| empréstimo tem um usuário | atributo `Usuario` |
| biblioteca possui vários itens | `List<ItemAcervo>` |
| livro é um item do acervo | `Livro extends ItemAcervo` |

Herança não deve ser escolhida apenas para reaproveitar algumas linhas. O relacionamento conceitual precisa fazer sentido.

---

## 6. Criando a classe geral `ItemAcervo`

Podemos declarar:

```java
package br.com.curso.biblioteca.dominio;

public abstract class ItemAcervo {

    private String codigo;
    private String titulo;
    private boolean disponivel;

    protected void cadastrarItem(String codigo, String titulo) {
        this.codigo = codigo;
        this.titulo = titulo;
        this.disponivel = true;
    }

    public boolean emprestar() {
        if (!disponivel) {
            return false;
        }

        disponivel = false;
        return true;
    }

    public boolean devolver() {
        if (disponivel) {
            return false;
        }

        disponivel = true;
        return true;
    }

    public String obterCodigo() {
        return codigo;
    }

    public String obterTitulo() {
        return titulo;
    }

    public boolean estaDisponivel() {
        return disponivel;
    }

    public abstract int calcularPrazoEmprestimo();
}
```

A palavra `abstract` informa que `ItemAcervo` serve como uma base incompleta. Não criaremos um “item genérico” com:

```java
new ItemAcervo();
```

Criaremos objetos das classes específicas, como `Livro` e `Revista`.

### Método abstrato

```java
public abstract int calcularPrazoEmprestimo();
```

O método possui assinatura, mas não possui corpo. A classe geral afirma que todo item do acervo precisa informar um prazo, porém cada tipo específico definirá o cálculo.

### O modificador `protected`

O método `cadastrarItem` foi declarado como `protected`. Ele pode ser usado pela própria classe e pelas classes que a estendem. Não fica aberto como uma operação geral da aplicação.

Os atributos continuam `private`. Herança não exige transformar todos os atributos em `protected`. As subclasses podem usar os comportamentos oferecidos pela classe base.

---

## 7. Especializando com `extends`

A classe `Livro` estende `ItemAcervo`:

```java
package br.com.curso.biblioteca.dominio;

public class Livro extends ItemAcervo {

    private String isbn;
    private String autor;

    public void cadastrarLivro(
            String codigo,
            String titulo,
            String isbn,
            String autor
    ) {
        cadastrarItem(codigo, titulo);
        this.isbn = isbn;
        this.autor = autor;
    }

    @Override
    public int calcularPrazoEmprestimo() {
        return 14;
    }
}
```

`Livro` recebe os métodos públicos de `ItemAcervo`, como `emprestar`, `devolver`, `obterTitulo` e `estaDisponivel`. Também acrescenta estado e comportamento próprios.

A revista segue a mesma base:

```java
package br.com.curso.biblioteca.dominio;

public class Revista extends ItemAcervo {

    private int numeroEdicao;

    public void cadastrarRevista(
            String codigo,
            String titulo,
            int numeroEdicao
    ) {
        cadastrarItem(codigo, titulo);
        this.numeroEdicao = numeroEdicao;
    }

    @Override
    public int calcularPrazoEmprestimo() {
        return 7;
    }
}
```

### Sobrescrita

Quando a subclasse fornece sua versão de um método da classe base, ocorre **sobrescrita**.

```java
@Override
public int calcularPrazoEmprestimo() {
    return 7;
}
```

`@Override` ajuda o compilador a verificar se realmente existe um método compatível sendo sobrescrito. Se o nome ou os parâmetros estiverem errados, o compilador poderá apontar o problema.

Não confunda sobrescrita com sobrecarga. Sobrescrita redefine um método herdado. Sobrecarga usa o mesmo nome com listas diferentes de parâmetros. A sobrecarga não é necessária para o objetivo deste tópico.

---

## 8. Polimorfismo: tratar diferentes objetos por um tipo comum

Herança prepara um recurso mais importante: o **polimorfismo**.

Uma variável do tipo geral pode guardar referência para um objeto específico:

```java
ItemAcervo item1 = new Livro();
ItemAcervo item2 = new Revista();
```

O tipo da variável é `ItemAcervo`, mas os objetos concretos continuam sendo `Livro` e `Revista`.

Também podemos criar uma coleção comum:

```java
private List<ItemAcervo> itens = new ArrayList<>();
```

Ela aceita ambos:

```java
itens.add(livro);
itens.add(revista);
```

Ao percorrer:

```java
for (ItemAcervo item : itens) {
    System.out.println(item.obterTitulo());
    System.out.println(item.calcularPrazoEmprestimo());
}
```

a mesma chamada produz comportamentos específicos:

- se o objeto for `Livro`, o prazo será 14;
- se o objeto for `Revista`, o prazo será 7.

O Java seleciona a implementação de acordo com o objeto real. Isso é polimorfismo: objetos diferentes respondem à mesma operação segundo seu próprio comportamento.

### O ganho não é apenas escrever menos

Sem polimorfismo, a biblioteca poderia perguntar o tipo e decidir manualmente:

```java
if (item é livro) {
    prazo = 14;
} else if (item é revista) {
    prazo = 7;
}
```

Esse pseudocódigo concentra na classe coordenadora um conhecimento que pertence aos tipos específicos. A cada novo tipo, o bloco precisaria crescer.

Com polimorfismo:

```java
int prazo = item.calcularPrazoEmprestimo();
```

`Biblioteca` sabe que todo `ItemAcervo` oferece a operação. Cada classe sabe produzir sua resposta.

### Ponte com COBOL

Em um fluxo procedural, é comum usar um código de tipo seguido por `EVALUATE`:

```cobol
EVALUATE WS-TIPO-ITEM
    WHEN 'L'
        MOVE 14 TO WS-PRAZO
    WHEN 'R'
        MOVE 7 TO WS-PRAZO
END-EVALUATE
```

Essa solução não é automaticamente ruim. Ela representa uma decisão explícita. O ponto é perceber outra possibilidade de organização: em Java, cada tipo pode fornecer sua própria implementação, enquanto o fluxo usa o contrato geral.

Polimorfismo não elimina todas as condições. Ele reduz condições usadas apenas para descobrir o tipo e escolher um comportamento que o próprio objeto poderia conhecer.

---

## 9. A biblioteca trabalhando com o tipo geral

Agora `Biblioteca` pode administrar qualquer item do acervo:

```java
package br.com.curso.biblioteca.dominio;

import java.util.ArrayList;
import java.util.List;

public class Biblioteca {

    private List<ItemAcervo> itens = new ArrayList<>();

    public void adicionarItem(ItemAcervo item) {
        itens.add(item);
    }

    public ItemAcervo buscarPorCodigo(String codigo) {
        for (ItemAcervo item : itens) {
            if (item.obterCodigo().equals(codigo)) {
                return item;
            }
        }

        return null;
    }

    public void exibirAcervo() {
        for (ItemAcervo item : itens) {
            System.out.println(
                    item.obterTitulo()
                    + " — prazo: "
                    + item.calcularPrazoEmprestimo()
                    + " dias"
            );
        }
    }

    public int obterQuantidadeItens() {
        return itens.size();
    }
}
```

Observe o que `Biblioteca` não precisa fazer:

- não precisa manter uma lista exclusiva para cada tipo;
- não precisa perguntar se o objeto é livro ou revista para exibir o prazo;
- não precisa conhecer ISBN ou número da edição;
- não precisa alterar a própria classe quando apenas a implementação do prazo muda.

Ela trabalha com aquilo que é comum a todos os itens.

---

## 10. Relacionamento polimórfico em `Emprestimo`

O empréstimo também pode depender do tipo geral:

```java
package br.com.curso.biblioteca.dominio;

import java.time.LocalDate;

public class Emprestimo {

    private Usuario usuario;
    private ItemAcervo item;
    private LocalDate dataEmprestimo;
    private LocalDate dataPrevistaDevolucao;

    public boolean registrar(Usuario usuario, ItemAcervo item, LocalDate data) {
        boolean realizado = item.emprestar();

        if (!realizado) {
            return false;
        }

        this.usuario = usuario;
        this.item = item;
        this.dataEmprestimo = data;
        this.dataPrevistaDevolucao = data.plusDays(
                item.calcularPrazoEmprestimo()
        );
        return true;
    }

    public String obterResumo() {
        return usuario.obterNome()
                + " — "
                + item.obterTitulo()
                + " — devolver até "
                + dataPrevistaDevolucao;
    }
}
```

O parâmetro:

```java
ItemAcervo item
```

aceita um `Livro` ou uma `Revista`. O empréstimo solicita o prazo ao objeto recebido. Não precisa de duas versões completas do método.

Essa colaboração reúne os conceitos do tópico:

- relacionamento: `Emprestimo` possui referências para `Usuario` e `ItemAcervo`;
- herança: `Livro` e `Revista` estendem `ItemAcervo`;
- polimorfismo: o prazo depende do objeto concreto;
- encapsulamento: o empréstimo solicita `item.emprestar()`;
- retorno: o resultado informa se o registro foi realizado.

---

## 11. Aplicação consolidada

```java
package br.com.curso.biblioteca;

import java.time.LocalDate;

import br.com.curso.biblioteca.dominio.Biblioteca;
import br.com.curso.biblioteca.dominio.Emprestimo;
import br.com.curso.biblioteca.dominio.ItemAcervo;
import br.com.curso.biblioteca.dominio.Livro;
import br.com.curso.biblioteca.dominio.Revista;
import br.com.curso.biblioteca.dominio.Usuario;

public class AplicacaoBiblioteca {

    public static void main(String[] args) {
        Usuario usuario = new Usuario();
        usuario.cadastrar("U001", "Ana Martins");

        Livro livro = new Livro();
        livro.cadastrarLivro(
                "I001",
                "Dom Casmurro",
                "9788535914849",
                "Machado de Assis"
        );

        Revista revista = new Revista();
        revista.cadastrarRevista(
                "I002",
                "Ciência Hoje",
                412
        );

        Biblioteca biblioteca = new Biblioteca();
        biblioteca.adicionarItem(livro);
        biblioteca.adicionarItem(revista);

        biblioteca.exibirAcervo();

        ItemAcervo encontrado = biblioteca.buscarPorCodigo("I002");

        if (encontrado != null) {
            Emprestimo emprestimo = new Emprestimo();
            boolean realizado = emprestimo.registrar(
                    usuario,
                    encontrado,
                    LocalDate.of(2026, 10, 7)
            );

            if (realizado) {
                System.out.println(emprestimo.obterResumo());
            }
        }
    }
}
```

Saída aproximada:

```text
Dom Casmurro — prazo: 14 dias
Ciência Hoje — prazo: 7 dias
Ana Martins — Ciência Hoje — devolver até 2026-10-14
```

O `main` monta o cenário e inicia o fluxo. As regras permanecem distribuídas:

- `Usuario` cuida de seus dados;
- `ItemAcervo` protege disponibilidade e informações comuns;
- `Livro` e `Revista` definem comportamentos específicos;
- `Biblioteca` administra a coleção;
- `Emprestimo` relaciona usuário, item e datas.

---

## 12. Interfaces: um contrato de capacidade

Esta seção pode ser usada se houver tempo. O restante do tópico já funciona sem ela.

Herança responde principalmente à ideia de especialização: `Livro` **é um** `ItemAcervo`. Uma **interface** descreve uma capacidade ou contrato que uma classe assume.

Podemos declarar:

```java
package br.com.curso.biblioteca.dominio;

public interface Emprestavel {

    boolean emprestar();

    boolean devolver();

    int calcularPrazoEmprestimo();
}
```

A interface informa quais operações um objeto emprestável oferece, mas não mantém os atributos do item.

`ItemAcervo` pode assumir o contrato:

```java
public abstract class ItemAcervo implements Emprestavel {
```

Como `ItemAcervo` já implementa `emprestar` e `devolver`, as subclasses herdam essas implementações. Como `calcularPrazoEmprestimo` continua abstrato, `Livro` e `Revista` fornecem suas versões.

Uma variável pode usar o tipo da interface:

```java
Emprestavel item = new Livro();
```

E uma coleção pode reunir qualquer objeto que cumpra o contrato:

```java
List<Emprestavel> itensEmprestaveis = new ArrayList<>();
```

### Classe abstrata e interface não são a mesma coisa

| Classe abstrata | Interface |
|---|---|
| pode manter estado comum | representa principalmente um contrato |
| pode fornecer implementação compartilhada | declara capacidades e também pode possuir métodos próprios permitidos pela linguagem |
| participa da hierarquia com `extends` | é assumida com `implements` |
| uma classe estende apenas uma classe | uma classe pode implementar várias interfaces |

Para esta introdução, use a distinção:

- `ItemAcervo`: base conceitual com estado e comportamento compartilhados;
- `Emprestavel`: contrato das operações necessárias para empréstimo.

Não crie interfaces apenas para aumentar a quantidade de arquivos. Elas são úteis quando existe um contrato relevante que pode ser cumprido por diferentes classes.

---

## 13. Composição antes de herança automática

Ao conhecer herança, é comum tentar usá-la em toda relação. Isso cria modelos artificiais.

Considere:

```text
Biblioteca possui livros
Emprestimo possui usuário
Livro é item do acervo
```

Somente a terceira afirmação expressa especialização.

As duas primeiras são relacionamentos por composição ou associação: um objeto trabalha com outros objetos. Não faria sentido escrever:

```java
class Biblioteca extends Livro
```

nem:

```java
class Emprestimo extends Usuario
```

Uma orientação prática é preferir objetos colaborando e usar herança quando houver uma relação “é um” clara, com comportamento realmente compartilhado.

Herança cria uma ligação forte entre as classes. Mudanças na classe base podem afetar as subclasses. Ela é valiosa, mas não deve ser o mecanismo padrão de reaproveitamento.

---

## 14. Consolidação da mudança procedural → OO

Chegamos ao ponto em que podemos observar a mudança completa.

### O pensamento procedural continua presente

Dentro dos métodos ainda existem:

- instruções em sequência;
- parâmetros;
- retornos;
- condições;
- repetições;
- chamadas para outras operações.

Nada disso foi descartado. Saber decompor processos continua sendo uma vantagem.

### O que mudou

A pergunta principal deixou de ser apenas:

> Qual é a próxima etapa do procedimento?

Agora também perguntamos:

> Qual objeto possui os dados necessários e deve proteger essa regra?

O fluxo não precisa conhecer todos os detalhes. Ele solicita comportamentos:

```java
biblioteca.adicionarItem(livro);
item.emprestar();
emprestimo.registrar(usuario, item, data);
```

### Da tabela para a coleção de objetos

Uma tabela COBOL reúne ocorrências de uma estrutura conhecida. Uma coleção Java reúne referências para objetos. Quando o tipo da coleção é geral, como `List<ItemAcervo>`, os elementos podem pertencer a classes específicas diferentes.

### Do código de tipo ao polimorfismo

Em muitos sistemas procedurais, um código identifica o tipo e um `EVALUATE` escolhe a regra. Em OO, comportamentos que variam por tipo podem ser sobrescritos:

```java
item.calcularPrazoEmprestimo();
```

O objeto correto responde.

### Da área aberta para o estado protegido

Em vez de alterar dados de qualquer lugar, classes oferecem operações públicas e mantêm atributos privados. Isso reduz estados incoerentes e aproxima regras dos dados que elas controlam.

### Do programa centralizador para a colaboração

`AplicacaoBiblioteca` inicia. `Biblioteca` coordena o acervo. `Emprestimo` representa a relação. `ItemAcervo` protege regras comuns. `Livro` e `Revista` especializam comportamentos.

Orientação a objetos não significa ausência de fluxo central. Significa que o fluxo central não precisa executar pessoalmente todas as responsabilidades.

---

## 15. Erros de compreensão que vale evitar

### “Relacionamento é copiar os campos de outra classe”

Um atributo pode usar a própria classe como tipo e guardar referência para o objeto relacionado.

### “Toda coleção deve ser pública para facilitar”

Expor uma lista mutável permite alterações fora das regras. Ofereça operações coerentes na classe responsável.

### “`List` é apenas um `OCCURS` sem limite”

A intenção de reunir elementos é semelhante, mas a coleção possui API própria e reúne referências para objetos.

### “Herança serve sempre que duas classes têm campos parecidos”

Sem uma relação conceitual “é um”, a herança pode criar uma hierarquia falsa. Semelhança isolada não basta.

### “A subclasse deve acessar diretamente todos os atributos da classe base”

A classe base pode manter atributos privados e oferecer métodos protegidos ou públicos adequados.

### “Polimorfismo é apenas guardar objetos diferentes na mesma lista”

A lista comum ajuda, mas o ponto central é chamar a mesma operação e obter o comportamento correspondente ao objeto real.

### “Polimorfismo elimina `if` e `switch`”

Não. Ele reduz decisões usadas apenas para descobrir tipos e selecionar comportamentos que pertencem às classes.

### “Interface e classe abstrata são sinônimos”

Uma classe abstrata pode compartilhar estado e implementação. Uma interface representa um contrato de capacidade.

### “Orientação a objetos exige abandonar o raciocínio de processos”

Os processos continuam existindo. Eles passam a ser executados por objetos que colaboram e protegem suas responsabilidades.

---

## 16. Checklist do Tópico 4

- [ ] Entendo que um atributo pode usar outra classe como tipo.
- [ ] Sei que relacionar objetos não significa copiar todos os campos.
- [ ] Consigo distinguir uma relação com um objeto de uma relação com vários.
- [ ] Sei declarar uma `List` com um tipo de elemento.
- [ ] Compreendo o papel inicial de `List` e `ArrayList`.
- [ ] Sei adicionar, contar e percorrer elementos.
- [ ] Entendo o `for` aprimorado.
- [ ] Sei verificar `null` antes de usar uma referência possivelmente ausente.
- [ ] Entendo por que a coleção também deve ser encapsulada.
- [ ] Reconheço herança como uma relação de especialização.
- [ ] Distingo “é um” de “tem um”.
- [ ] Entendo o uso de `extends`.
- [ ] Compreendo o propósito de uma classe abstrata.
- [ ] Sei o que é um método abstrato.
- [ ] Entendo a sobrescrita e o uso de `@Override`.
- [ ] Sei que uma referência do tipo geral pode apontar para um objeto específico.
- [ ] Entendo que a implementação executada depende do objeto real.
- [ ] Consigo explicar por que isso é polimorfismo.
- [ ] Entendo a finalidade inicial de uma interface.
- [ ] Distingo classe abstrata e interface.
- [ ] Reconheço que composição costuma ser preferível a uma herança artificial.
- [ ] Consigo explicar como a experiência com COBOL continua útil.
- [ ] Consigo explicar o que mudou na organização das responsabilidades.

---

## Conclusão

Ao longo destes quatro tópicos, o Java deixou de ser apenas um novo conjunto de comandos.

Primeiro, você preparou o ambiente e executou um programa. Depois, passou a representar o domínio com classes, objetos, atributos, tipos e pacotes. Em seguida, colocou comportamento nos objetos com métodos, encapsulamento, condições e repetições. Por fim, conectou esses objetos por relacionamentos, coleções, herança, polimorfismo e contratos.

A experiência com COBOL não foi apagada. Ela forneceu referências importantes para compreender dados, processos, decisões, repetições, tabelas e chamadas. O cuidado necessário foi não transformar Java em uma tradução linha a linha.

O pensamento orientado a objetos pode ser resumido por algumas perguntas:

1. Quais conceitos existem neste domínio?
2. Qual estado pertence a cada conceito?
3. Qual comportamento cada objeto deve proteger?
4. Como os objetos se relacionam?
5. Quais objetos formam coleções?
6. Quais diferenças podem ser tratadas por um tipo comum e polimorfismo?
7. O fluxo está coordenando objetos ou concentrando responsabilidades que deveriam estar neles?

Essas perguntas não resolvem automaticamente todo projeto, mas criam uma direção. A mudança de stack deixa de ser uma troca forçada de sintaxe e passa a ser a ampliação de uma experiência já construída.

Você não está começando do zero. Está aprendendo uma nova forma de organizar aquilo que já sabe fazer: transformar regras de negócio em software confiável.
