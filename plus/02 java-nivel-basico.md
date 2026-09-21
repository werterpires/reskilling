# Java - Nível Básico

# Módulo 1 — Herança e polimorfismo

A herança é um dos pilares da orientação a objetos e permite que uma classe herde propriedades e comportamentos de outra, criando uma hierarquia natural entre tipos. Este módulo explora como construir essas hierarquias, como especializar o comportamento através da sobrescrita de métodos, e como usar polimorfismo para escrever código flexível e reutilizável. Esses conceitos constituem a base para qualquer código Java bem estruturado, desde aplicações simples até frameworks corporativos complexos.

## Herança

Este capítulo apresenta o mecanismo mais fundamental de especialização e reutilização de código em Java: a possibilidade de uma classe herdar, de outra já existente, campos e métodos que não precisa reimplementar. Compreender herança significa entender não apenas a sintaxe (`extends`) e os papéis de superclasse e subclasse, mas também o conceito de "relação é um" que governa quando herança é apropriada — e, igualmente importante, quando não é. Este é o ponto de partida para polimorfismo, que virá nos capítulos seguintes.

### `extends`

Em Java, `extends` é a palavra-chave que cria uma relação de herança entre duas classes. Quando você escreve `class Gerente extends Funcionario`, está dizendo ao compilador que `Gerente` é uma extensão de `Funcionario`: tudo o que `Funcionario` oferece em termos de campos e métodos não privados passa a existir também em `Gerente`, sem precisar ser reescrito. É o ponto de partida de toda hierarquia de classes e o elo que conecta os dois papéis que aparecem logo em seguida, o de superclasse e o de subclasse.

Sem esse mecanismo, reaproveitar o comportamento de uma classe existente exigiria copiar o código de um lado para o outro ou recriar manualmente cada método. Imagine um sistema de folha de pagamento em que `Funcionario` já calcula salário anual, desconto de imposto e tempo de casa. Se `Gerente`, `Vendedor` e `Estagiario` precisam desse mesmo comportamento com pequenas variações, copiar essas regras para cada classe cria três cópias que envelhecem de forma independente: corrigir um erro no cálculo do imposto passa a exigir três correções, e mais cedo ou mais tarde uma delas será esquecida.

Com `extends`, esse comportamento comum fica em um único lugar. A subclasse declara apenas o que a diferencia:

```java
class Funcionario {
    String nome;
    double salario;

    double salarioAnual() {
        return salario * 12;
    }
}

class Gerente extends Funcionario {
    double bonus;

    double remuneracaoTotal() {
        return salarioAnual() + bonus; // salarioAnual() veio da superclasse
    }
}
```

`Gerente` não repete `nome`, `salario` nem `salarioAnual()`; ela recebe tudo isso pronto e acrescenta o campo `bonus` e um método próprio. Uma regra importante da linguagem: uma classe só pode usar `extends` sobre uma única classe por vez — não existe herança múltipla de classes em Java —, ainda que uma mesma classe possa implementar várias interfaces, assunto de outro módulo.

A principal alternativa a `extends` é a composição: em vez de `Gerente` ser um `Funcionario`, ela teria um campo do tipo `Funcionario` e encaminharia as chamadas para ele. A escolha entre herdar e compor se decide no conceito de "relação é um", ainda neste capítulo, e volta a aparecer mais adiante no curso. Fora isso, quando não há relação de tipo nenhuma entre as classes e se quer apenas reaproveitar uma função de cálculo isolada, um método estático utilitário resolve sem herança e sem composição.

Evite `extends` quando a ligação entre as classes for de posse e não de identidade — a distinção entre "é um" e "tem um" que o capítulo fecha mais adiante —, quando você herdaria apenas para aproveitar um ou dois métodos convenientes, ou quando a superclasse não foi projetada para ser estendida e expõe detalhes internos que a subclasse poderia quebrar sem perceber.

### Superclasse

A superclasse é a classe que está acima na hierarquia de herança: aquela cujos campos e métodos são herdados por outras. Quando escrevemos `class Gerente extends Funcionario`, `Funcionario` é a superclasse — também chamada de classe-base, classe-mãe ou classe-pai. Ela concentra o que há de comum entre várias classes mais específicas e funciona como um molde geral a partir do qual as variações são construídas. A divisão de papéis entre superclasse e subclasse é o eixo de todo o capítulo: a superclasse define o comum, a subclasse define o particular.

Sem o conceito de superclasse, cada classe seria uma ilha. Um cadastro de veículos com `Carro`, `Moto` e `Caminhao` repetiria em cada uma os mesmos campos `placa`, `ano` e `quilometragem`, além de métodos como `registrarViagem()`. Qualquer mudança de regra — por exemplo, passar a validar a placa em um novo formato — teria de ser propagada manualmente por todas as classes, com o risco de esquecer alguma.

Reunindo esse núcleo comum em uma superclasse `Veiculo`, o código passa a ter uma única fonte de verdade:

```java
class Veiculo {
    String placa;
    int ano;
    double quilometragem;

    void registrarViagem(double km) {
        quilometragem += km;
    }
}

class Caminhao extends Veiculo {
    double capacidadeCarga;
}
```

`Caminhao` já nasce com `placa`, `ano`, `quilometragem` e `registrarViagem()`. Em Java, mesmo quando uma classe não declara `extends`, ela ainda tem uma superclasse implícita: `Object`, a raiz de toda a hierarquia. É de lá que todo objeto herda métodos como `toString()` e `equals()`. Uma superclasse também pode ter a sua própria superclasse, formando uma cadeia (`Caminhao` → `Veiculo` → `Object`).

Nem todo comportamento comum precisa morar numa superclasse: ele pode ficar em uma classe utilitária separada, acionada por composição, ou virar apenas um contrato em uma interface, com a implementação escrita onde for necessário. Cada opção tem seu lugar, discutido no módulo de abstração.

Convém não inchar a superclasse com comportamento que serve só a algumas subclasses. Se `Veiculo` ganha um método `emitirNotaDeFrete()` que só faz sentido para `Caminhao`, todas as demais subclasses passam a carregar algo irrelevante. O sinal de uma boa superclasse é que tudo nela faz sentido para qualquer subclasse, sem exceção.

### Subclasse

A subclasse é o outro lado da relação de herança: a classe que herda de outra, recebe seus campos e métodos e pode acrescentar os seus próprios. Em `class Gerente extends Funcionario`, `Gerente` é a subclasse — também chamada de classe derivada, classe-filha ou classe estendida. Ela representa uma versão mais especializada da superclasse: mantém tudo o que a classe-base oferece e adiciona aquilo que a torna distinta.

O problema que a subclasse resolve aparece quando já existe um tipo geral que funciona, mas é preciso ter variações dele. Sem herança, criar essa variação significaria uma de duas coisas: modificar a classe geral para tentar acomodar todos os casos especiais — enchendo-a de condicionais e de campos que só valem às vezes — ou recriar a classe inteira do zero só para mudar um detalhe. A primeira alternativa torna a classe geral confusa; a segunda multiplica código.

Como subclasse, a variação fica enxuta e focada:

```java
class ContaBancaria {
    protected double saldo;

    void sacar(double valor) {
        if (valor <= saldo) {
            saldo -= valor;
        }
    }
}

class ContaEspecial extends ContaBancaria {
    double limite;

    double saldoDisponivel() {
        return saldo + limite; // 'saldo' foi herdado
    }
}
```

`ContaEspecial` reaproveita `saldo` e `sacar()` e descreve apenas o que é novo. A subclasse pode acrescentar campos e métodos e também pode redefinir um método herdado para se comportar de outra forma — isso é a sobrescrita, tema do próximo capítulo. Um detalhe de acesso: a subclasse enxerga os membros `public` e `protected` da superclasse, mas não os `private`; para expor algo apenas à descendência, usa-se `protected`.

Fora a herança, essa variação poderia ser modelada por composição — a `ContaEspecial` manteria uma `ContaBancaria` interna e delegaria chamadas a ela — ou por um campo de configuração na própria classe geral, um atributo `tipo` com comportamento condicional, o que costuma ser aceitável quando as diferenças são pequenas e pontuais.

Criar uma subclasse deixa de ser boa ideia quando ela precisa "desfazer" coisas da superclasse: ignorar métodos herdados, lançar erro em operações que a base permite ou esconder campos. Isso é sinal de que ela não é de fato uma especialização, e forçar a herança nesse caso produz hierarquias frágeis e difíceis de entender.

### Relação "é um"

A relação "é um" (em inglês, *is-a*) é o critério que indica quando a herança é apropriada. Antes de escrever `extends`, vale completar a frase: "todo X é um Y". Se ela é verdadeira sem malabarismos — "todo gerente é um funcionário", "toda conta especial é uma conta bancária", "todo triângulo é uma figura geométrica" —, então a herança modela bem o problema, com Y como superclasse e X como subclasse. É essa relação que dá sentido a todos os termos vistos no capítulo.

O ponto que essa ideia resolve é a tentação de usar herança apenas como atalho para reaproveitar código. É comum ver alguém escrever `class Carrinho extends ArrayList` só porque o carrinho precisa guardar uma lista de itens. Tecnicamente compila e funciona, mas "um carrinho é uma lista" é falso: um carrinho *tem* uma lista de itens, além de ter um dono, um valor total e um cupom de desconto. Ao herdar de `ArrayList`, o carrinho passa a expor dezenas de métodos que não fazem sentido para ele (`removeRange`, `ensureCapacity`) e fica preso a uma estrutura interna difícil de trocar depois.

O contraste é com a relação "tem um" (*has-a*), que pede composição — um campo — em vez de herança:

```java
// "é um": herança faz sentido
class Fornecedor extends Empresa { }

// "tem um": composição, não herança
class Pedido {
    Cliente cliente;          // um pedido tem um cliente
    List<ItemPedido> itens;   // um pedido tem itens
}
```

Na dúvida, o teste da substituição ajuda: se todo ponto do código que espera um `Y` pudesse receber um `X` sem surpresas, a relação "é um" se sustenta. Se colocar um `X` no lugar de um `Y` quebraria expectativas — porque `X` proíbe operações que `Y` permite, ou muda o significado delas —, a herança está sendo forçada.

A alternativa nesses casos é sempre a composição, que o curso aprofunda adiante sob o lema "composição sobre herança": costuma ser mais flexível por permitir trocar a parte interna e por não acoplar a classe a toda a superfície pública de uma classe-base. A herança por "é um" continua sendo a escolha certa quando existe de fato uma hierarquia de tipos estável e quando se quer aproveitar polimorfismo — tratar vários `X` diferentes de maneira uniforme através do tipo `Y` —, assunto dos próximos capítulos.

## `super`

Escolhida a herança para modelar um "é um", a subclasse não vive isolada da classe-mãe: ela precisa, em vários momentos, conversar com ela — pedir que a parte herdada seja inicializada, reaproveitar um comportamento que foi sobrescrito, ou alcançar um membro que ficou encoberto. A palavra-chave `super` é o canal dessa conversa. Este capítulo mostra as três situações em que ela aparece: na chamada de construtores da superclasse, na chamada explícita de métodos da superclasse e no acesso a campos herdados.

### Construtores

Quando um objeto de subclasse é criado, ele precisa ser inicializado em duas frentes: a parte que veio da superclasse e a parte própria da subclasse. Na forma `super(...)`, usada dentro de um construtor, `super` é a maneira de a subclasse chamar explicitamente um construtor da superclasse. É o primeiro elo do diálogo com a classe-mãe e o que garante que a herança comece a partir de uma base já corretamente construída.

A superclasse quase sempre tem campos que só ela sabe inicializar do jeito certo: valores padrão, validações, invariantes que precisam valer desde o primeiro instante. Se a subclasse pudesse simplesmente ignorar essa etapa, o objeto nasceria pela metade — com os campos herdados em estado indefinido e as validações da classe-base nunca executadas. Por isso a linguagem impõe uma regra: todo construtor de subclasse, antes de rodar o próprio corpo, executa um construtor da superclasse. Se você não escrever nada, o compilador insere `super()` sem argumentos como primeira instrução. Se a superclasse não oferece um construtor sem argumentos — porque declarou pelo menos um construtor com parâmetros e nenhum outro —, você é obrigado a chamar `super(...)` você mesmo, passando os argumentos corretos, e essa chamada tem de ser a primeira linha do construtor.

```java
class Funcionario {
    private final String nome;
    private final double salario;

    Funcionario(String nome, double salario) {
        if (salario < 0) throw new IllegalArgumentException("salário negativo");
        this.nome = nome;
        this.salario = salario;
    }
}

class Gerente extends Funcionario {
    private final double bonus;

    Gerente(String nome, double salario, double bonus) {
        super(nome, salario);   // primeira instrução: constrói a parte Funcionario
        this.bonus = bonus;
    }
}
```

Sem a linha `super(nome, salario)`, esse código nem compila, porque `Funcionario` não tem construtor sem argumentos. Com ela, a validação do salário roda antes de `Gerente` acrescentar o bônus. A cadeia sobe até o topo: o construtor de `Gerente` aciona o de `Funcionario`, que aciona o de `Object`.

Há uma variação relacionada: em vez de `super(...)`, um construtor pode chamar `this(...)` para delegar a outro construtor da mesma classe. Vale um ou outro, nunca os dois, e sempre como primeira instrução; o construtor de destino é quem, no fim, chamará `super`. A alternativa a lidar com `super(...)` explícito é dar à superclasse um construtor sem argumentos, deixando o `super()` implícito resolver — cabível quando a inicialização da base é trivial. Um cuidado importante: não chame, a partir de um construtor, métodos que possam estar sobrescritos; durante a execução de `super(...)` a parte da subclasse ainda não foi inicializada, e o método sobrescrito enxergaria campos zerados.

### Métodos

Na forma `super.metodo(...)`, `super` chama a versão de um método definida na superclasse, mesmo que a subclasse a tenha sobrescrito. É o segundo uso da palavra-chave: enquanto `super(...)` fala com o construtor da mãe, `super.algumMetodo()` fala com o comportamento da mãe. Aparece quase sempre dentro de um método que está sobrescrevendo outro.

Ao sobrescrever um método, muitas vezes a intenção não é substituir o que a superclasse faz, e sim acrescentar algo ao redor. Sem uma forma de invocar a versão original, a saída seria copiar o corpo do método da superclasse para dentro da subclasse — recriando a duplicação que a herança serve para eliminar e quebrando no dia em que a superclasse mudar. Com `super.metodo()`, o método sobrescrito executa o código da superclasse (um nível acima) e combina esse resultado com o trabalho próprio da subclasse. Essa chamada não passa pelo mecanismo de dynamic dispatch: ela vai direto para a implementação da classe-mãe.

```java
class Relatorio {
    String gerar() {
        return "Cabeçalho\nCorpo";
    }
}

class RelatorioAuditado extends Relatorio {
    @Override
    String gerar() {
        String base = super.gerar();          // aproveita o que a mãe já faz
        return base + "\nAssinado em " + LocalDate.now();
    }
}
```

`RelatorioAuditado` não reescreve cabeçalho e corpo: delega isso a `super.gerar()` e apenas acrescenta o rodapé de auditoria. O mesmo padrão aparece com frequência no `toString()` de uma subclasse, que devolve `super.toString() + " bonus=" + bonus`.

Dois detalhes de sintaxe: `super` sobe um único nível — não existe `super.super`. Numa cadeia `A` → `B` → `C`, se `C.m()` chama `super.m()`, executa `B.m()`; se este também chamar `super.m()`, aí sim alcança `A.m()`. E `super` não vale em métodos `static`. Quanto a alternativas: se a subclasse vai substituir o comportamento por inteiro, basta não chamar `super`. Quando o ponto de extensão é previsível, a superclasse pode oferecer um template method — um método `final` que, internamente, chama um método `protected` de gancho que a subclasse implementa —, poupando a subclasse de lembrar de chamar `super`. Se você se pega chamando `super.m()` e logo desfazendo o efeito dele, o comportamento herdado não servia, e isso é sinal de hierarquia mal escolhida.

### Campos herdados

Os campos `public` e `protected` declarados na superclasse passam a integrar a subclasse e são acessados diretamente pelo nome, sem nenhuma sintaxe especial: dentro de `ContaEspecial`, escrever `saldo` já alcança o campo que veio de `ContaBancaria`. `super` só entra em cena num caso específico: quando a subclasse declara um campo com o mesmo nome de um campo da superclasse e é preciso se referir ao da superclasse, escrevendo `super.campo`.

Na maior parte do tempo não há problema algum — herdar campo é transparente. A situação em que `super.campo` importa é a ocultação de campo (*field hiding*): se `Subclasse` declara `int contador` e `Superclasse` também tem `int contador`, o objeto passa a carregar dois campos distintos com o mesmo nome. Dentro da subclasse, `contador` e `this.contador` apontam para o novo; `super.contador` aponta para o herdado. Sem `super`, o campo da superclasse ficaria inacessível pelo nome dentro da subclasse.

```java
class Conta {
    protected double saldo = 0;
}

class ContaComReserva extends Conta {
    double saldo = 100;   // OCULTA Conta.saldo — quase sempre um erro

    double total() {
        return super.saldo + this.saldo;   // 0 + 100
    }
}
```

O exemplo mostra por que ocultar campo costuma ser um defeito e não um recurso: quem lê espera um único `saldo`, mas os métodos herdados de `Conta` continuam mexendo no `saldo` da superclasse, enquanto os métodos de `ContaComReserva` mexem no outro, e os dois valores divergem silenciosamente. Diferente dos métodos, campos não têm dynamic dispatch: qual campo é acessado depende do tipo declarado da referência, não do objeto apontado — mais um motivo para não duplicar nomes. A recomendação prática é direta: nunca reaproveite o nome de um campo herdado. Seguida essa regra, `super.campo` deixa de ser necessário e todo acesso ao campo da base se faz apenas por `saldo` ou `this.saldo`.

A regra de visibilidade já vista na aula de herança vale igual para campos: a subclasse alcança os `public` e `protected`, nunca os `private` — para estes, recorre aos getters e setters que a superclasse expõe. Aliás, muitas equipes preferem manter todos os campos `private` e oferecer esses acessores à descendência, o que preserva a liberdade de mudar a representação interna depois — uma alternativa ao `protected` aberto. E quando a subclasse precisa de um valor parecido, porém distinto, o caminho certo é um campo com outro nome, não um homônimo. Em resumo: `super` aplicado a campos é uma saída de emergência para um nome mal escolhido, não um estilo de programação.

## Sobrescrita

Com a herança estabelecida e a palavra-chave `super` disponível para conversar com a classe-mãe, falta a peça que torna a herança realmente expressiva: a capacidade de a subclasse não apenas acrescentar comportamento, mas redefinir um comportamento que herdou. Este capítulo trata da sobrescrita de métodos — o que significa "sobrescrever" (Override), como a anotação `@Override` transforma um erro silencioso em erro de compilação, e como a JVM escolhe sozinha, em tempo de execução, qual versão de um método chamar (dynamic dispatch). Juntos, esses três pontos são a base do polimorfismo, tema do próximo capítulo.

### Override

Override — em português, sobrescrita — é a redefinição, dentro de uma subclasse, de um método herdado da superclasse, preservando a mesma assinatura e substituindo apenas o corpo. É o passo seguinte natural depois de `extends` e `super`: a herança já trouxe o método pronto, e agora a subclasse diz "para mim, esse comportamento é outro". Assinatura igual significa mesmo nome e mesma lista de parâmetros; o tipo de retorno pode ser o mesmo ou um subtipo dele (retorno covariante), o modificador de acesso não pode ser mais restritivo que o da superclasse, e o método sobrescrito não pode declarar exceções verificadas mais amplas do que as originais. Métodos `static`, `final` ou `private` ficam de fora: `final` proíbe a sobrescrita, `private` não é visível para a subclasse, e `static` pertence à classe, não ao objeto.

Sem sobrescrita, toda subclasse ficaria presa ao comportamento da classe-base. A saída seria encher a superclasse de condicionais — um campo `tipo` e um `if` extenso dentro do método, decidindo o que fazer conforme o valor desse campo. Cada nova variação exigiria abrir a superclasse e acrescentar mais um ramo ao `if`, e a classe que deveria ser o "molde geral" viraria um depósito de casos particulares que não se conhecem entre si.

Com a sobrescrita, cada especialização descreve o próprio comportamento no próprio lugar:

```java
class Funcionario {
    double salario;

    double calcularBonus() {
        return salario * 0.10;   // regra geral: 10%
    }
}

class Gerente extends Funcionario {
    @Override
    double calcularBonus() {
        return salario * 0.20 + 500;   // gerente: 20% + adicional fixo
    }
}
```

`Gerente` mantém tudo o que `Funcionario` oferece, mas quando alguém pedir `calcularBonus()` a um gerente, é a versão de `Gerente` que roda. Se a subclasse quiser aproveitar parte do cálculo original, pode chamar `super.calcularBonus()` de dentro da versão nova, como visto no capítulo anterior.

É importante não confundir sobrescrita com sobrecarga (overloading). Sobrecarga é ter, na mesma classe, vários métodos com o mesmo nome e listas de parâmetros diferentes — `imprimir(int)` e `imprimir(String)`. A sobrescrita exige assinatura idêntica e acontece entre superclasse e subclasse. Trocar o tipo de um parâmetro ao "sobrescrever" não sobrescreve nada: cria um método novo que apenas divide o nome com o herdado.

Como alternativa à sobrescrita, quando as diferenças de comportamento são muitas e mudam com frequência, costuma-se extrair o trecho variável para um objeto à parte, injetado por composição (o padrão Strategy): a classe recebe uma "política de bônus" e delega o cálculo a ela, em vez de depender de subclasses. Para variações pontuais e estáveis, porém, a sobrescrita é mais direta.

Evite sobrescrever um método para fazê-lo contradizer o contrato da superclasse — lançar exceção onde a base sempre retornava um valor, ou devolver algo com significado diferente do esperado. Quem escreveu código contra o tipo da superclasse conta com aquele comportamento, e uma sobrescrita que o quebra produz bugs difíceis de rastrear. Também não use sobrescrita quando, na verdade, você quer um método novo: se o comportamento não é uma versão diferente de algo herdado, dê a ele outro nome em vez de forçar uma assinatura igual.

### `@Override`

`@Override` é uma anotação que se coloca imediatamente antes de um método para declarar que ele tem a intenção de sobrescrever um método de uma superclasse ou de uma interface. Ela não muda o que o método faz — em tempo de execução, o programa se comporta exatamente igual com ou sem a anotação. O que ela faz é dar ao compilador uma afirmação para verificar e, ao leitor humano, um aviso claro de intenção logo na primeira linha do método.

O problema que ela resolve é sutil e frequente. Sobrescrever depende de acertar a assinatura ao pé da letra. Se você erra o nome do método, troca o tipo de um parâmetro ou esquece um argumento, o compilador não reclama: ele entende que você quis criar um método novo, que por acaso mora numa subclasse. O código compila, o programa roda, e a versão herdada — não a sua — é a que executa. Bugs assim consomem horas, porque tudo parece certo.

O caso clássico é o `equals`:

```java
class Ponto {
    int x, y;

    // SEM @Override — e com erro: o parâmetro deveria ser Object
    public boolean equals(Ponto outro) {
        return this.x == outro.x && this.y == outro.y;
    }
}
```

Esse `equals(Ponto)` não sobrescreve o `equals(Object)` herdado de `Object` — é uma sobrecarga. Coleções como `HashSet` e `ArrayList` chamam `equals(Object)`, então continuam usando a comparação por identidade herdada, e o `Ponto` se comporta de forma inesperada dentro delas. Com `@Override` na linha de cima, o compilador teria recusado o código na hora, apontando que não existe `equals(Ponto)` para sobrescrever.

Colocando a anotação, a regra passa a trabalhar a seu favor: o compilador confirma que existe mesmo um método com aquela assinatura na hierarquia acima. Se não existir, é erro de compilação, e o problema aparece em segundos, no seu editor, em vez de semanas depois em produção. Desde o Java 6, `@Override` vale também para métodos que implementam uma interface, então pode ser usada nesses casos.

Não há alternativa técnica equivalente: as únicas outras linhas de defesa são o aviso que a IDE mostra e a atenção de quem revisa o código — ambos mais fáceis de ignorar do que um erro de compilação. A recomendação prática é usar `@Override` em todo método que você acredita estar sobrescrevendo, sem exceção.

O único "quando não usar" é literal: não é possível colocar `@Override` em um método que não sobrescreve nada — um método genuinamente novo da subclasse. Fazer isso provoca justamente o erro de compilação que a anotação existe para gerar. Fora esse caso, não há motivo para omiti-la.

### Dynamic dispatch

Dynamic dispatch — despacho dinâmico, também chamado de late binding, ligação tardia ou invocação de método virtual — é o mecanismo pelo qual a JVM decide, no momento da chamada, qual versão de um método sobrescrito deve executar, olhando para o tipo real do objeto e não para o tipo declarado da variável que o referencia. É a engrenagem que faz a sobrescrita valer a pena e o que dá sentido ao polimorfismo do próximo capítulo.

Para entender o que ele resolve, vale imaginar a alternativa: ligação estática, em que a decisão é tomada na compilação, a partir do tipo escrito na declaração da variável. Nesse cenário, uma variável declarada como `Funcionario` sempre chamaria `Funcionario.calcularBonus()`, mesmo que, em tempo de execução, apontasse para um `Gerente`. Sobrescrever métodos seria quase inútil, e para obter comportamento específico por tipo você teria de espalhar verificações `instanceof` seguidas de casts pelo código, um bloco de `if`/`else` para cada operação.

Com dynamic dispatch, isso desaparece. Todo objeto carrega, em tempo de execução, a informação da classe da qual foi instanciado. Quando um método de instância é chamado, a JVM parte dessa classe real e procura a implementação do método ali; se não encontrar, sobe na hierarquia até achar. O tipo da variável serve apenas para o compilador checar que o método existe e é acessível — quem ele chama de fato é definido pelo objeto.

```java
List<Funcionario> equipe = List.of(
    new Funcionario(),
    new Gerente(),
    new Diretor()
);

for (Funcionario f : equipe) {
    System.out.println(f.calcularBonus());
    // roda a versão de Funcionario, de Gerente e de Diretor,
    // nessa ordem — mesmo o tipo da variável sendo Funcionario
}
```

O laço trata todos como `Funcionario`, e ainda assim cada elemento responde com o próprio cálculo. Adicionar amanhã uma classe `Estagiario` com o seu `calcularBonus()` não exige tocar nesse laço: o despacho dinâmico já vai chamar a versão certa.

Dois limites importantes. Primeiro, isso vale só para métodos de instância. Campos não têm despacho dinâmico: o campo acessado depende do tipo declarado da referência, como visto no capítulo de `super`. Métodos `static` também são resolvidos pelo tipo declarado, não pelo objeto. Segundo, `super.metodo()` não passa por esse mecanismo — é justamente a forma de chamar deliberadamente a versão da superclasse, ignorando a do objeto.

A "alternativa" ao dynamic dispatch é aquele despacho manual já esboçado acima: as cadeias de `instanceof` com cast, um `switch` sobre o tipo, ou funções e objetos de estratégia passados explicitamente. Versões modernas do Java oferecem pattern matching em `switch`, que torna esse estilo manual mais legível quando ele é mesmo necessário — por exemplo, quando os tipos não estão sob o seu controle ou não formam uma hierarquia. Para código orientado a objetos comum, porém, deixar a JVM despachar é mais simples e mais fácil de estender.

Não há como "desligar" o dynamic dispatch para métodos de instância, mas há situações em que você quer evitar o efeito dele. Uma é chamar, de dentro de um construtor da superclasse, um método que a subclasse sobrescreve: o despacho dinâmico vai executar a versão da subclasse antes de os campos dela terem sido inicializados, expondo valores zerados. A defesa é não chamar métodos sobrescrevíveis em construtores. Outra é quando um método simplesmente não deve variar entre subclasses — nesse caso, marque-o como `final`, e o compilador garante que ninguém quebre essa expectativa.

## Polimorfismo

A sobrescrita e o despacho dinâmico do capítulo anterior mostraram *como* a JVM escolhe, em tempo de execução, qual versão de um método rodar. Este capítulo mostra *para quê* isso serve: o polimorfismo é a capacidade de escrever um trecho de código uma única vez e vê-lo funcionar com qualquer subclasse — inclusive com subclasses que ainda não existem. Para entender o mecanismo é preciso separar duas coisas que costumam ser confundidas: o tipo da variável, que o compilador enxerga, e o tipo do objeto, que a JVM executa. Sobre essa distinção se apoiam o upcasting, a conversão que liga uma coisa à outra, e o princípio de substituição, que diz quando tudo isso é seguro.

### Tipo da variável

O tipo da variável — também chamado de tipo declarado, tipo estático ou *compile-time type* — é o tipo que aparece à esquerda do nome quando a variável é declarada: o `Funcionario` em `Funcionario f = ...`. Ele é fixo, escrito no código-fonte, e é a única informação que o compilador tem sobre aquela referência enquanto verifica o programa. Tudo o que o compilador decide a respeito de `f` — quais métodos podem ser chamados, quais campos existem, se uma atribuição é válida — parte desse tipo, nunca do objeto que `f` vai apontar em tempo de execução.

O problema que essa ideia organiza é o de conciliar segurança e flexibilidade. Se a linguagem exigisse que cada variável fosse declarada exatamente com a classe do objeto instanciado, uma lista que guarda `Gerente`, `Diretor` e `Estagiario` seria impossível de escrever, e um método que aceita "qualquer funcionário" teria de ser reescrito para cada subtipo. Por outro lado, se não houvesse checagem nenhuma, só se descobriria em produção que um método não existe.

O tipo da variável resolve isso funcionando como um contrato mínimo: ao declarar `Funcionario f`, você promete ao compilador que `f` sempre apontará para algo que é, no mínimo, um `Funcionario`, e em troca ele libera o uso de tudo o que `Funcionario` declara — e só isso. A regra prática é declarar a variável pelo tipo mais geral que ainda atende ao que o código precisa fazer.

```java
Funcionario f = new Gerente();

f.calcularBonus();       // compila: calcularBonus() existe em Funcionario
f.aprovarFerias(joao);   // NÃO compila: aprovarFerias() só existe em Gerente,
                         // mesmo o objeto sendo um Gerente
```

O compilador recusa a segunda linha olhando apenas para `Funcionario`, o tipo de `f`. O objeto por trás é um `Gerente` de fato, mas isso não conta nessa decisão. Vale como analogia um crachá de acesso: o crachá que você carrega (o tipo da variável) determina quais portas o leitor deixa você tentar abrir; quem você é de verdade dentro da empresa (o tipo do objeto) é outra questão, que só importa depois que a porta abre.

A alternativa mais comum é declarar a variável com o tipo concreto — `Gerente g = new Gerente()` —, o que dá acesso a toda a superfície da subclasse sem cast, à custa de amarrar aquele trecho a uma classe específica. Desde o Java 10 há também `var`, que infere o tipo da variável a partir do lado direito: em `var g = new Gerente()`, o tipo da variável passa a ser `Gerente`, não `Funcionario`.

Não force um tipo muito geral quando o código realmente depende, o tempo todo, de recursos específicos da subclasse: se cada uso de `f` exige um cast de volta para `Gerente`, o tipo declarado está mais atrapalhando do que protegendo, e declarar diretamente como `Gerente` é mais honesto.

### Tipo do objeto

O tipo do objeto — também chamado de tipo real, tipo dinâmico ou *runtime type* — é a classe que foi usada no `new` para criar aquele objeto: o `Gerente` em `new Gerente()`. Diferente do tipo da variável, que muda conforme a referência é reatribuída, o tipo do objeto nasce com ele e não muda nunca, por mais que a referência passe por variáveis de tipos diferentes. É esse tipo que a JVM consulta em tempo de execução para decidir qual implementação de um método sobrescrito vai rodar.

Sem essa distinção, a sobrescrita perderia o sentido. Como o capítulo anterior mostrou, se a chamada olhasse apenas para o tipo da variável, `Funcionario f = new Gerente(); f.calcularBonus();` rodaria sempre a versão de `Funcionario` — e redefinir o método na subclasse não teria efeito nenhum justamente quando o objeto é manipulado por uma referência mais geral, que é o caso interessante.

O mecanismo que garante o contrário é o despacho dinâmico já visto. O que vale reter aqui é a divisão de trabalho entre os dois tipos: o tipo da variável garante, em tempo de compilação, que o método existe e é acessível; o tipo do objeto decide qual corpo de método de fato executa.

```java
Funcionario f = new Gerente();

f.calcularBonus();                 // roda Gerente.calcularBonus() — decidido pelo tipo do objeto
System.out.println(f.getClass()); // class Gerente
```

Retomando a analogia do crachá: se o tipo da variável é o crachá, o tipo do objeto é a pessoa que o está usando. O crachá diz "Funcionario" e por isso libera certas portas, mas depois que a porta abre quem faz o trabalho é a pessoa real, com a competência que ela tem — um gerente age como gerente mesmo estando com um crachá genérico. O método `getClass()`, herdado de `Object`, é o que revela essa identidade em código.

Um contraponto importante: nem tudo segue o tipo do objeto. Campos acessados pelo nome e métodos `static` são resolvidos pelo tipo da variável, na compilação. Assim, se `Funcionario` e `Gerente` declaram cada um um campo `codigo`, a expressão `f.codigo` lê o campo de `Funcionario`, porque `f` é declarada como `Funcionario` — mesmo o objeto sendo `Gerente`. Só chamadas a métodos de instância passam pelo despacho dinâmico.

A forma de inspecionar o tipo do objeto de propósito é o `instanceof`, com ou sem pattern matching, assunto do próximo capítulo. Convém, porém, não construir a lógica do programa em torno do tipo exato do objeto: escrever `if (f.getClass() == Gerente.class)` para tratar cada classe à mão desfaz o ganho do polimorfismo e cria um ponto que precisa ser editado a cada nova subclasse. Usar `getClass()` para comparação estrita dentro de `equals`, em particular, torna a igualdade incompatível com qualquer subclasse — um efeito quase sempre indesejado.

### Upcasting

Upcasting é a atribuição de uma referência de um tipo mais específico a uma variável de um tipo mais geral na mesma linha de herança — uma subclasse sendo vista como sua superclasse, ou uma classe sendo vista como uma interface que ela implementa. É uma conversão que o Java faz sozinho, sem exigir sintaxe de cast e sem nenhum risco de falhar em tempo de execução, porque a relação "todo `Gerente` é um `Funcionario`" já foi garantida pelo `extends`. O nome vem da imagem da hierarquia desenhada com a superclasse no topo: apontar a referência "para cima".

O que o upcasting viabiliza é o código genérico. Sem ele, cada estrutura e cada método teria de falar de um tipo concreto: uma lista de `Gerente`, outra de `Diretor`, um método `pagar(Gerente)` e outro `pagar(Diretor)` com corpos praticamente idênticos. Toda vez que surgisse uma nova subclasse, seria preciso criar mais uma variação de tudo.

Com o upcasting, você escreve uma vez contra o tipo geral e passa qualquer subtipo:

```java
void processarFolha(List<Funcionario> equipe) {
    for (Funcionario f : equipe) {
        System.out.println(f.calcularBonus());
    }
}

List<Funcionario> equipe = new ArrayList<>();
equipe.add(new Gerente());     // upcast implícito Gerente     -> Funcionario
equipe.add(new Estagiario());  // upcast implícito Estagiario  -> Funcionario
processarFolha(equipe);
```

Cada `add` faz um upcast: o objeto continua sendo um `Gerente` ou um `Estagiario` — o `new` não muda —, apenas passa a ser referenciado por uma variável de tipo `Funcionario`. E, graças ao despacho dinâmico, `f.calcularBonus()` ainda executa a versão de cada subclasse; o upcast estreita o que o compilador deixa você chamar, não o comportamento do objeto. É como apresentar um cardiologista simplesmente como "médico": ele não deixou de ser cardiologista, você só escolheu um rótulo mais amplo, que serve em mais situações.

A alternativa ao upcasting para escrever código reutilizável é declarar as variáveis e parâmetros já com o tipo de uma interface — `List` em vez de `ArrayList`, `Comparable` em vez da classe concreta —, o que é a mesma ideia levada ao limite: programe para o tipo mais abstrato que resolve. Generics com *bounded types* (`<T extends Funcionario>`) atacam o mesmo problema quando é preciso, além de tratar os elementos de forma uniforme, preservar o tipo específico de cada um.

O upcasting deixa de ser suficiente quando, depois de generalizar, você precisa de volta um recurso que só existe na subclasse — chamar `aprovarFerias()` em algo que está guardado como `Funcionario`. Aí é preciso o caminho inverso, o downcasting, que exige cast explícito e verificação com `instanceof`, tema do próximo capítulo.

### Substituição

O princípio da substituição diz que, em qualquer ponto do programa onde se espera um objeto de um determinado tipo, deve ser possível usar um objeto de qualquer subtipo dele sem que o programa deixe de funcionar corretamente. Formulado por Barbara Liskov, é conhecido como Princípio da Substituição de Liskov (LSP) e corresponde à letra "L" do conjunto SOLID. Se os conceitos anteriores apresentaram os mecanismos do polimorfismo — upcasting e despacho dinâmico —, a substituição é a condição que torna esses mecanismos confiáveis: ela define quando uma hierarquia de herança é sólida.

O problema aparece porque o compilador verifica pouca coisa. Ele garante que a subclasse tem os métodos com as assinaturas certas, mas não tem como saber se eles *se comportam* como o código cliente espera. Um trecho que recebe um `Funcionario` e chama `calcularBonus()` conta com um número de volta; se alguma subclasse resolve lançar uma exceção nesse método, ou devolver um valor negativo, ou alterar o estado do objeto de um jeito que a superclasse nunca faria, todo código escrito contra `Funcionario` passa a ter um comportamento imprevisível que, ainda assim, compila sem reclamação.

Respeitar a substituição significa que a subclasse, ao sobrescrever, não pode exigir mais do que a superclasse exigia (não fortalecer pré-condições), não pode entregar menos do que ela prometia (não enfraquecer pós-condições), deve preservar as invariantes da classe-base e não deve lançar exceções novas onde a original não lançava. Em resumo: a subclasse pode fazer diferente, mas não pode surpreender quem só conhece a superclasse.

O exemplo clássico da violação é o do quadrado e do retângulo:

```java
class Retangulo {
    protected int largura, altura;
    void setLargura(int l) { this.largura = l; }
    void setAltura(int a)  { this.altura = a; }
    int area() { return largura * altura; }
}

class Quadrado extends Retangulo {
    @Override void setLargura(int l) { this.largura = this.altura = l; }
    @Override void setAltura(int a)  { this.largura = this.altura = a; }
}
```

Matematicamente todo quadrado é um retângulo, mas o código que depende de `Retangulo` assume que mexer na largura não altera a altura. Um método que faça `r.setLargura(5); r.setAltura(4);` e espere `r.area() == 20` funciona com `Retangulo` e falha com `Quadrado`, que devolve 16. A herança é válida para o compilador e errada para o programa.

Quando a substituição não se sustenta, a saída é não usar herança: modelar por composição (o `Quadrado` tem um lado e expõe só `area()`, sem se dizer um `Retangulo`), ou dividir o contrato em interfaces menores, de modo que nenhuma implementação seja obrigada a suportar operações que não fazem sentido para ela. Às vezes vale até inverter a direção da herança, porque o subtipo seguro é o contrário do que a intuição sugere.

Não vale a pena aplicar o princípio como perfeccionismo teórico sobre código que ninguém trata de forma polimórfica: se uma classe nunca é usada através da superclasse, a violação não tem como se manifestar. Mas, no momento em que o polimorfismo entra — coleções de tipo geral, parâmetros de superclasse, frameworks que chamam o seu código —, a substituição deixa de ser opcional.

## Casting de referências

O capítulo de polimorfismo mostrou o caminho de ida: o upcasting trata uma subclasse pelo tipo da superclasse e permite escrever código genérico. Este capítulo trata do caminho de volta. Há situações em que o código genérico não basta — é preciso alcançar um campo ou um método que só existe no subtipo específico. Para isso a linguagem oferece três peças que trabalham juntas: o downcasting, a conversão de uma referência geral para um tipo mais específico da mesma hierarquia; o `instanceof`, o operador que verifica o tipo real do objeto antes da conversão; e a noção de segurança de tipos, que explica por que essa verificação não é dispensável e o que a JVM faz quando ela é ignorada.

### Downcasting

Downcasting é a conversão explícita de uma referência de um tipo mais geral para um tipo mais específico dentro da mesma hierarquia de herança — o oposto do upcasting. Onde `Funcionario f = new Gerente()` sobe a referência para o tipo geral sem qualquer esforço de sintaxe, `Gerente g = (Gerente) f` desce de volta para o tipo específico, e isso exige o cast entre parênteses. A diferença de tratamento não é arbitrária: o upcast é sempre seguro, porque todo `Gerente` é um `Funcionario`; o downcast pode falhar, porque nem todo `Funcionario` é um `Gerente`.

O problema que ele resolve nasce justamente do upcasting. Depois de guardar objetos numa `List<Funcionario>` ou de recebê-los num parâmetro do tipo `Funcionario`, o compilador só libera o que `Funcionario` declara. Se em algum ponto você precisa chamar `aprovarFerias()`, um método que só `Gerente` possui, não há como pedir isso a uma referência de tipo `Funcionario` — o compilador recusa, mesmo que o objeto por trás seja, de fato, um `Gerente`. Sem uma forma de reconverter, o recurso específico ficaria fora de alcance.

O downcast reabre essa porta. Ao escrever `(Gerente) f`, você afirma ao compilador que aquela referência aponta para um `Gerente`, e ele passa a liberar toda a superfície de `Gerente` sobre o resultado. Em tempo de execução, a JVM confere se a afirmação é verdadeira: se o objeto for mesmo um `Gerente`, a conversão passa; se não for, o programa lança `ClassCastException` na própria linha do cast.

```java
Funcionario f = buscarFuncionario();   // tipo da variável: Funcionario
// f.aprovarFerias(joao);              // não compila: método não existe em Funcionario

Gerente g = (Gerente) f;               // downcast explícito
g.aprovarFerias(joao);                 // agora o compilador libera o método de Gerente
```

Vale a analogia do crachá usada no capítulo anterior: se apresentar um cardiologista apenas como "médico" é o upcast, o downcast é voltar a tratá-lo como cardiologista para pedir a leitura de um eletrocardiograma. Mas a parte técnica não termina na imagem: essa reconversão só é legítima se a pessoa for mesmo cardiologista — caso contrário, o pedido não tem sentido e o sistema reage com erro em vez de obedecer.

A alternativa preferível ao downcast é não precisar dele: se o comportamento específico puder virar um método sobrescrito na hierarquia, o despacho dinâmico resolve sem cast nenhum. Quando os tipos vêm de fora do seu controle, o pattern matching com `instanceof` e o `switch` sobre tipos organizam melhor a conversão. Separar as coleções por tipo desde a origem também elimina a necessidade.

Evite downcasting como rotina. Se um trecho faz upcast e, poucas linhas depois, o downcast de volta ao mesmo tipo, o tipo geral não era o adequado ali. E nunca faça o cast sem ter certeza do tipo — seja porque acabou de criar o objeto, seja porque verificou antes com `instanceof`, assunto da próxima seção.

### `instanceof`

`instanceof` é um operador que devolve um valor booleano indicando se o objeto referenciado por uma variável é instância de um determinado tipo — ou de algum subtipo dele. A expressão `f instanceof Gerente` resulta em `true` quando o objeto por trás de `f` foi criado como `Gerente` ou como uma subclasse de `Gerente`, e `false` em qualquer outro caso, inclusive quando `f` é `null`. É a ferramenta que permite inspecionar o tipo real do objeto — aquele fixado no `new` — em vez de confiar apenas no tipo declarado da variável.

Sem essa verificação, o downcasting seria um salto no escuro. Escrever `(Gerente) f` quando `f` aponta para um `Estagiario` compila normalmente, mas quebra em execução com `ClassCastException`. Numa coleção que mistura vários subtipos de `Funcionario`, não há como saber de antemão qual é qual, e converter tudo às cegas transformaria o laço numa sequência de exceções.

`instanceof` resolve isso protegendo o cast: você só converte depois de confirmar o tipo. Desde o Java 16, o operador ganhou uma forma mais enxuta, o pattern matching, que une o teste, o cast e a declaração da variável num gesto só. Em `if (f instanceof Gerente g)`, se o teste passa, a variável `g` já nasce com o tipo `Gerente` e o valor convertido, dispensando a linha `Gerente g = (Gerente) f` logo abaixo. O alcance de `g` acompanha o fluxo: ela existe onde o compilador consegue garantir que o teste foi verdadeiro.

```java
for (Funcionario f : equipe) {
    if (f instanceof Gerente g) {   // testa e, se passar, entrega g já convertido
        g.aprovarFerias(joao);
    } else {
        System.out.println(f.getNome() + " não aprova férias");
    }
}
```

O laço trata a lista inteira como `Funcionario` e, para cada elemento, decide em tempo de execução se aquele objeto merece o tratamento de `Gerente`. Uma analogia: é a portaria conferindo, um a um, quem tem credencial de gerente antes de liberar a entrada na sala de reunião — quem não tem segue outro caminho, sem que a fila trave. Fechada a checagem, o código volta a ser polimórfico normal.

Como alternativa, `getClass() == Gerente.class` também compara tipos, mas de forma estrita: rejeita subclasses de `Gerente`, o que quase nunca é o desejado. E, quando há muitos tipos a distinguir, o `switch` com pattern matching fica mais legível do que uma escada de `instanceof`.

Justamente essa escada é o sinal de "quando não usar": se o código traz uma sequência longa de `if (x instanceof A) ... else if (x instanceof B) ...` decidindo comportamento por tipo, o problema provavelmente pede um método sobrescrito na hierarquia, deixando o despacho dinâmico fazer a seleção. `instanceof` rende melhor nos casos pontuais — um único subtipo que precisa de tratamento extra — e não como substituto do polimorfismo.

### Segurança de tipos

Segurança de tipos (em inglês, *type safety*) é a garantia de que uma operação só será executada sobre um objeto que de fato a suporta. Em Java essa garantia se apoia em duas camadas: o compilador, que usa o tipo declarado das variáveis para barrar chamadas a métodos e acessos a campos que o tipo não possui; e a JVM, que, nas conversões de referência, confere em tempo de execução se o objeto é mesmo do tipo pretendido. O casting de referências é o ponto em que essas duas camadas se encontram, e a `ClassCastException` é a rede que a JVM estende quando a checagem do compilador não foi suficiente.

O risco que essa garantia neutraliza fica claro na comparação com linguagens de mais baixo nível, onde um cast mal feito entre tipos é aceito sem conferência: o programa passa a interpretar aquela região de memória como se fosse outro tipo, e o resultado é comportamento indefinido — leitura de lixo, corrupção de dados, falhas difíceis de reproduzir. Em Java isso não ocorre. O pior caso de um downcast errado é uma exceção imediata, clara, apontando a linha exata e os dois tipos envolvidos.

Na prática, manter a segurança de tipos ao converter referências significa combinar `instanceof` e downcast — ou usar o pattern matching, que já faz as duas coisas de forma integrada. O compilador ajuda antes mesmo da execução: um cast entre tipos sem nenhuma relação de herança nem compila (`(String) umFuncionario` é recusado de imediato). O que sobra para a JVM são os casos em que a conversão é plausível pela hierarquia, mas pode não corresponder ao objeto real.

```java
Funcionario f = new Estagiario();
Gerente g = (Gerente) f;   // compila — Gerente é subtipo de Funcionario —,
                           // mas lança ClassCastException em execução
```

Trocar esse cast direto por `if (f instanceof Gerente g) { ... }` faz a conversão acontecer só quando é segura, e o ramo `else` cuida do resto. É a diferença entre o eletricista testar o fio antes de encostar a mão e simplesmente confiar que está desligado.

A alternativa mais forte é empurrar a verificação para o tempo de compilação com generics: uma `List<Gerente>` nunca deixa entrar um `Estagiario`, então ler dela dispensa qualquer cast e qualquer `instanceof`. Quando o conjunto de subtipos é fechado e conhecido, classes `sealed` combinadas com `switch` exaustivo dão garantia equivalente, também verificada pelo compilador. Ambos reduzem a superfície em que uma `ClassCastException` poderia surgir.

Não trate a segurança de tipos como algo a contornar com `try/catch` de `ClassCastException` no fluxo normal: capturar essa exceção para seguir em frente esconde um erro de modelagem em vez de corrigi-lo. E, se você se pega verificando tipos o tempo todo, o sinal é de que as variáveis foram declaradas gerais demais — a resposta costuma ser um tipo mais específico ou generics, não mais verificações.

---

# Módulo 2 — Abstração e interfaces

A abstração é o princípio de expor apenas aquilo que importa e ocultar detalhes internos; é o complemento natural da herança que permite criar camadas de responsabilidade bem definidas e código fácil de estender. Este módulo explora dois mecanismos principais: classes abstratas, que combinam herança com a promessa de que certos métodos _têm_ de ser implementados por subclasses, e interfaces, que definem _apenas_ contratos sem exigir uma base de implementação comum. Compreender quando usar cada um, e quando preferir composição, é essencial para arquitetura Java profissional.

## Classes abstratas

O Módulo 1 mostrou como uma classe herda de outra e como o despacho dinâmico faz cada subclasse responder à sua maneira. Este capítulo dá o passo seguinte: e se a classe-base não devesse existir como objeto concreto, servindo apenas de molde? Uma `Forma` genérica não tem área definida, um `Funcionario` "puro" nunca é contratado sem cargo. A palavra-chave `abstract` permite declarar exatamente isso — uma classe incompleta, que reúne o que há de comum e obriga as subclasses a preencher o resto. Veremos como marcar a classe com `abstract`, como declarar métodos sem corpo que as subclasses são forçadas a implementar, e como misturar comportamento pronto com lacunas propositais.

### `abstract`

`abstract`, aplicado a uma classe (`abstract class Forma { ... }`), marca essa classe como incompleta: ela não pode ser instanciada diretamente com `new`. Serve apenas como superclasse — um ponto de partida a partir do qual outras classes, essas sim concretas, são construídas. É o elemento central do capítulo e o que distingue uma classe-base comum de um molde que só existe para ser estendido. Uma classe abstrata continua sendo uma classe de verdade: tem construtores (chamados via `super` pelas subclasses), campos, e métodos com corpo normal. O que ela não permite é `new Forma()`.

Sem `abstract`, nada impede a criação de objetos que não fazem sentido. Se `Forma` é uma classe concreta com um método `area()` que retorna `0`, alguém em algum lugar vai escrever `new Forma()` e obter um objeto sem significado — uma forma sem lados, sem raio, com área zero — que se infiltra em listas e cálculos até causar um bug difícil de rastrear. A única defesa seria um comentário pedindo "não instancie esta classe" ou um construtor que lança exceção, ambos frágeis: o primeiro é ignorado, o segundo só falha em tempo de execução.

Com `abstract`, a proibição passa a ser do compilador. `new Forma()` vira erro de compilação, apontado no editor. Só `Circulo`, `Retangulo` e outras subclasses concretas podem ser instanciadas, e cada uma nasce completa. A referência do tipo abstrato continuaEste capítulo apresenta o mecanismo mais fundamental de especialização e reutilização de código em Java: a possibilidade de uma classe herdar, de outra já existente, campos e métodos que não precisa reimplementar. Compreender herança significa entender não apenas a sintaxe (`extends`) e os papéis de superclasse e subclasse, mas também o conceito de "relação é um" que governa quando herança é apropriada — e, igualmente importante, quando não é. Este é o ponto de partida para polimorfismo, que virá nos capítulos seguintes.
 valendo normalmente para polimorfismo:

```java
abstract class Forma {
    private final String cor;

    Forma(String cor) {          // construtor: usado pelas subclasses via super
        this.cor = cor;
    }

    String descricao() {         // método concreto, herdado por todas
        return "Forma " + cor;
    }
}

class Circulo extends Forma {
    private final double raio;

    Circulo(String cor, double raio) {
        super(cor);
        this.raio = raio;
    }
}

Forma f = new Circulo("azul", 3);   // ok: variável do tipo abstrato, objeto concreto
// Forma g = new Forma("verde");    // não compila: Forma é abstract
```

Uma analogia: `abstract` é como a planta baixa de um apartamento na maquete de um prédio. A planta define paredes, cômodos e a posição das tomadas — informação real e reaproveitável —, mas ninguém mora na planta; só nas unidades construídas a partir dela. Amarrando de volta ao código: os métodos concretos e os campos da classe abstrata são a parte já desenhada, herdada de graça; a construção efetiva acontece em cada subclasse.

A principal alternativa é a interface, quando a classe-base não precisa de estado nem de construtor e serve só como contrato — assunto do próximo capítulo. Quando existe um conjunto fixo e conhecido de variações, um `enum` com comportamento por constante também substitui a hierarquia. E se a intenção for apenas restringir, sem proibir totalmente, um construtor `protected` numa classe concreta limita a criação ao próprio pacote e às subclasses.

Não marque como `abstract` uma classe que é útil por si mesma e que alguém legitimamente instancia — fazê-lo obriga a criar uma subclasse vazia só para poder usá-la. Da mesma forma, se a classe não tem métodos abstratos nem representa um conceito genérico demais para existir sozinho, ela provavelmente é só uma classe comum, e `abstract` ali só atrapalha quem quer usá-la.

### Métodos abstratos

Um método abstrato é declarado com a palavra `abstract`, sem corpo, terminando em ponto e vírgula: `abstract double area();`. Ele diz o nome, os parâmetros e o tipo de retorno de uma operação, mas não como executá-la. Só pode aparecer dentro de uma classe abstrata (ou de uma interface). Seu efeito é uma obrigação: toda subclasse concreta precisa fornecer uma implementação para esse método — sobrescrevê-lo com corpo — ou então ser declarada `abstract` também, empurrando a obrigação adiante. É o que transforma a classe abstrata do capítulo anterior em um molde com encaixes bem definidos.

A classe-base, sem métodos abstratos, fica obrigada a inventar um corpo para algo que ela não tem como saber. As saídas usuais são ruins: retornar um valor falso (`return 0;` numa `area()` que não conhece a forma), retornar `null`, ou lançar `throw new UnsupportedOperationException()`. Nos três casos, se o autor de uma subclasse esquecer de sobrescrever o método, o compilador não reclama — o código herda o corpo inútil e o problema só aparece quando `area()` é chamada de verdade, longe da causa.

Declarado o método como abstrato, esse esquecimento passa a ser barrado já na compilação. Ao escrever `class Circulo extends Forma` sem implementar `area()`, o compilador recusa a classe na hora, dizendo que `Circulo` não é abstrata mas não implementa `area()`. E, uma vez implementado em cada subclasse, o despacho dinâmico faz o resto: código escrito contra o tipo `Forma` chama `area()` e recebe a resposta correta de cada objeto.

```java
abstract class Forma {
    abstract double area();       // sem corpo: cada subclasse decide
    abstract double perimetro();

    String resumo() {             // método concreto que usa os abstratos
        return "área=" + area() + " perímetro=" + perimetro();
    }
}

class Circulo extends Forma {
    private final double raio;

    Circulo(double raio) { this.raio = raio; }

    @Override double area()      { return Math.PI * raio * raio; }
    @Override double perimetro() { return 2 * Math.PI * raio; }
}
```

Repare que `resumo()` chama `area()` e `perimetro()` sem saber quem vai respondê-las — a classe abstrata programa contra as próprias lacunas. É como um formulário oficial com campos obrigatórios em branco: o documento já traz o texto fixo, a numeração e a formatação, mas não pode ser protocolado enquanto os campos marcados não forem preenchidos. Cada subclasse é uma pessoa diferente preenchendo os mesmos campos com os próprios dados.

Valem aqui as mesmas alternativas vistas em `abstract`, a interface à frente delas: sem estado nem lógica comum a compartilhar, seus métodos já são abstratos por natureza. Mais próprio deste conceito: quando existe um comportamento-padrão razoável, o melhor é oferecer um método concreto que a subclasse sobrescreve se quiser, em vez de forçá-la a escrever algo. E quando as variações são muitas e mudam com frequência, extrair o trecho variável para um objeto injetado por composição (padrão Strategy) evita multiplicar subclasses.

Não declare um método abstrato quando há um padrão sensato que serve para a maioria dos casos — obrigar cada subclasse a reescrever a mesma coisa é desperdício e fonte de divergência. E se todos os métodos da classe são abstratos, sem nenhum campo nem método concreto, a classe abstrata não agrega nada sobre uma interface.

### Implementações parciais

Uma implementação parcial é uma classe abstrata que combina, de propósito, métodos concretos já prontos com métodos abstratos ainda em aberto. A parte concreta descreve o que é igual para todas as subclasses — normalmente a sequência de passos de um processo —, e os métodos abstratos marcam os pontos em que cada subclasse precisa entrar com a sua diferença. É a forma mais comum de usar tudo o que os dois conceitos anteriores apresentaram, e corresponde ao padrão de projeto conhecido como Template Method: um método "molde" fixa a ordem das operações e delega os detalhes.

Sem esse arranjo, sobram duas opções ruins. Uma é repetir o esqueleto do processo em cada subclasse: abrir o arquivo, tratar erros, fechar o arquivo — o mesmo código copiado em `ImportadorCsv`, `ImportadorXml` e `ImportadorJson`, com a duplicação e o risco de manutenção que a herança deveria eliminar. A outra é jogar tudo numa interface e deixar cada implementação remontar o fluxo inteiro por conta própria, sem garantia de que a ordem e o tratamento de erro sejam iguais entre elas.

Feito assim, o fluxo mora num único lugar — muitas vezes num método `final`, para que nenhuma subclasse possa alterar a ordem — e chama os trechos variáveis através de métodos abstratos, geralmente `protected`:

```java
abstract class ImportadorDeArquivo {

    final int importar(Path caminho) {      // o molde: ordem fixa, não sobrescrevível
        List<String> linhas = ler(caminho);
        int gravados = 0;
        for (String linha : linhas) {
            Registro r = converter(linha);  // passo variável
            if (valido(r)) {                 // passo variável
                salvar(r);
                gravados++;
            }
        }
        return gravados;
    }

    private List<String> ler(Path caminho) { /* ... comum a todos ... */ }
    private void salvar(Registro r)        { /* ... comum a todos ... */ }

    protected abstract Registro converter(String linha);  // cada formato preenche
    protected abstract boolean valido(Registro r);
}

class ImportadorCsv extends ImportadorDeArquivo {
    @Override protected Registro converter(String linha) {
        String[] campos = linha.split(",");
        return new Registro(campos[0], campos[1]);
    }
    @Override protected boolean valido(Registro r) {
        return r.nome() != null && !r.nome().isBlank();
    }
}
```

`ImportadorCsv` não sabe nada sobre abrir arquivo, iterar linhas ou contar gravações — só descreve como um formato CSV vira `Registro` e o que torna esse registro válido. Um `ImportadorXml` reaproveitaria todo o `importar(...)` mudando apenas esses dois métodos. A analogia é a de uma receita de bolo impressa com a técnica completa — bater, assar a 180°C por 40 minutos, desenformar frio — deixando em branco só o tipo de farinha e o recheio; cada variação da receita preenche essas duas linhas e herda todo o resto do preparo. No código, o "resto do preparo" é o método `final`, que garante que ninguém pule uma etapa.

A alternativa moderna é uma interface com _default methods_, que também oferece implementação parcial, porém sem poder guardar estado — útil quando não há campos a compartilhar. Quando a parte que varia é grande ou precisa mudar em tempo de execução, injetar um objeto de estratégia por composição costuma ser mais flexível do que amarrar cada variação a uma subclasse. E, se as subclasses não precisam de uma ordem imposta, basta deixar os métodos concretos como sobrescrevíveis, sem o molde `final`.

Esse padrão não compensa quando a suposta "parte fixa" ainda não estabilizou e muda a cada nova subclasse — nesse caso o método-molde vira um estorvo, editado toda hora. Também não vale quando as subclasses precisam variar a própria estrutura do processo, e não só passos isolados: aí a herança engessa mais do que ajuda, e composição devolve a liberdade.

## Interfaces

O capítulo anterior fechou a classe abstrata como um molde: uma base que reúne o que há de comum e deixa buracos marcados para as subclasses preencherem. Este capítulo apresenta o caso extremo dessa ideia, em que a base não tem _nada_ pronto — nenhum campo, nenhum construtor, nenhum corpo de método —, apenas a lista das operações que alguém promete oferecer. Esse tipo "só de contrato" chama-se interface. Veremos como declará-la com `interface`, como uma classe se compromete a cumpri-la usando `implements`, e por que enxergar a interface como um contrato — uma promessa vinculante sobre comportamento, independente de quem o fornece — muda a forma de estruturar o código.

### `interface`

Uma `interface`, em Java, é um tipo de referência declarado com a palavra-chave `interface` no lugar de `class`. Diferente de uma classe — mesmo de uma classe abstrata —, ela não guarda estado: não tem campos de instância, não tem construtor e, no Java SE clássico, não traz nenhum método com corpo. O que ela reúne é uma lista de assinaturas de métodos (`double calcularImposto();`), cada uma implicitamente `public` e `abstract`. É a forma mais enxuta de abstração da linguagem: descreve o que um objeto sabe fazer sem dizer uma linha sobre como. Encaixa-se neste módulo como a contraparte "pura" da classe abstrata do capítulo anterior — quando a base comum não precisa de nenhuma implementação compartilhada, só de um conjunto de operações que alguém garante oferecer.

O problema que a interface resolve aparece quando tentamos usar só classes. Em Java, uma classe estende no máximo uma superclasse. Se `NotaFiscal` precisa ser, ao mesmo tempo, algo que pode ser impresso, algo que pode ser comparado por valor e algo que pode ser gravado em disco, não há como empilhar três classes abstratas para dar essas três capacidades. Além disso, forçar tipos sem parentesco real a herdar de uma mesma classe-base só para compartilhar um método cria uma hierarquia artificial: `Funcionario` e `Arquivo` não têm nada em comum, mas ambos podem precisar de um método `resumo()`.

A interface desfaz esse nó porque uma classe pode implementar quantas interfaces quiser, e porque nada além das assinaturas viaja junto. O bom uso é declarar contratos pequenos e focados, nomeados pela capacidade que representam — em Java é comum o sufixo `-vel` ou, no acervo da própria linguagem, `-able`: `Comparable`, `Iterable`, `Runnable`.

```java
public interface Tributavel {
    double BASE_MINIMA = 1000.0;      // implicitamente public static final

    double calcularImposto();          // implicitamente public abstract
    String descricaoTributaria();
}
```

Qualquer objeto de um tipo que implemente `Tributavel` pode ser tratado apenas como `Tributavel`, e um cálculo de arrecadação percorre uma `List<Tributavel>` sem saber se cada item é uma conta bancária, um imóvel ou um salário. É o mesmo polimorfismo já visto com herança, mas apoiado num contrato em vez de numa classe-base.

Uma analogia: a interface é como a norma da tomada elétrica que os fabricantes de eletrodomésticos concordam em respeitar. A norma não diz como o aparelho funciona por dentro — pode ser um liquidificador ou um carregador —, só fixa o formato dos pinos e a tensão. Quem projeta a instalação da parede trabalha contra a norma, não contra cada aparelho. Trazendo de volta ao código: quem chama `calcularImposto()` programa contra `Tributavel`, e cada classe implementadora é um aparelho diferente com os mesmos pinos.

A alternativa principal é a classe abstrata, preferível quando existe estado ou lógica comum de verdade a compartilhar — construtores, campos, um método-molde. Quando o conjunto de variações é fechado e conhecido, um `enum` com comportamento por constante também dispensa a interface. E, para um único tipo sem perspectiva de polimorfismo, uma classe concreta comum basta.

Não crie uma interface quando há só uma implementação plausível e nenhum sinal de que surgirão outras: o arquivo extra vira uma camada a mais para navegar sem nada em troca. Também evite interfaces grandes, que juntam muitas operações não relacionadas — cada classe implementadora fica obrigada a fornecer métodos que não lhe dizem respeito, muitas vezes com corpos vazios ou que lançam exceção.

### `implements`

`implements` é a palavra-chave que liga uma classe a uma ou mais interfaces. Escrevendo `class ContaCorrente implements Tributavel`, a classe assume o compromisso de fornecer um corpo concreto para cada método declarado na interface. É o elo equivalente ao `extends` da herança, mas com duas diferenças importantes: uma classe pode implementar várias interfaces de uma vez, separadas por vírgula (`implements Tributavel, Comparable<ContaCorrente>`), e `implements` não traz nenhum código pronto junto — só a obrigação. Quando há herança e implementação ao mesmo tempo, `extends` vem primeiro: `class Gerente extends Funcionario implements Tributavel`.

Sem um mecanismo assim, a ligação entre uma classe e um contrato ficaria só na documentação ou na confiança de que "esse método existe". O compilador não teria como verificar se a classe realmente oferece todas as operações prometidas, e — mais concreto — não permitiria atribuir a instância a uma variável do tipo da interface, que é o que destrava o polimorfismo. Faltaria também um ponto único onde o programador declara a intenção: "esta classe se propõe a cumprir este papel".

Com `implements`, a verificação passa a ser do compilador. Se `ContaCorrente` declara que implementa `Tributavel` mas esquece `descricaoTributaria()`, a classe não compila, com a mensagem de que o método abstrato não foi implementado; assim como acontece com os métodos abstratos de uma classe-base, a saída é marcar a própria classe como `abstract` e deixar que uma subclasse resolva a pendência. Uma vez implementados todos os métodos — anotados com `@Override`, o que faz o compilador conferir a assinatura —, os objetos da classe podem ser usados em qualquer lugar que espere a interface:

```java
public class ContaCorrente implements Tributavel {
    private double saldo;

    @Override
    public double calcularImposto() {
        return saldo > BASE_MINIMA ? saldo * 0.001 : 0;
    }

    @Override
    public String descricaoTributaria() {
        return "IOF sobre conta corrente";
    }
}

Tributavel t = new ContaCorrente();   // ok: ContaCorrente cumpre o contrato
double imposto = t.calcularImposto(); // chama a versão da ContaCorrente
```

Repare que os métodos implementados precisam ser `public`: a interface os declara com essa visibilidade, e uma implementação não pode reduzir o acesso. Implementar duas interfaces é só continuar a lista — `class Imovel implements Tributavel, Comparable<Imovel>` obriga a classe a satisfazer os dois contratos, e a instância de `Imovel` pode circular ora como `Tributavel`, ora como `Comparable`.

A analogia é a de assinar um contrato de prestação de serviço. Assinar (`implements`) não executa o trabalho; apenas registra formalmente que aquela empresa se responsabiliza por entregar todos os itens listados. Se um item não for entregue, o contrato é quebrado — e aqui a "quebra" é detectada já na compilação, não no dia da cobrança. Amarrando ao código: a assinatura é a cláusula `implements`, os itens são os métodos da interface, e a entrega é cada corpo de método na classe.

A alternativa própria deste ponto aparece nas interfaces funcionais, de um único método abstrato: nesse caso uma expressão lambda ou uma classe anônima dispensa a classe nomeada com `implements`, tema de módulos adiante. As outras opções são as já pesadas em `interface` — estender uma classe abstrata quando há implementação a herdar, ou usar a classe diretamente quando não interessa tratar o objeto pelo tipo do contrato.

Não use `implements` só para herdar as constantes de uma interface — o chamado "constant interface antipattern", retomado adiante no conceito de constantes. E, pelo motivo já visto em `interface`, implementar um contrato inflado do qual a classe só aproveita um pedaço indica que ele deveria ser quebrado em interfaces menores.

### Contratos

Chamar uma interface de contrato é dizer que ela é uma promessa vinculante sobre o comportamento disponível, independente de quem o fornece. Esse contrato tem duas camadas. A sintática são as assinaturas que o compilador obriga a cumprir: nomes, parâmetros, tipos de retorno. A semântica é o que a documentação do tipo estabelece e o compilador não tem como checar: o que cada método promete devolver, o que ele espera receber, quais invariantes precisa preservar. `Comparable`, por exemplo, exige que a ordenação seja consistente e transitiva; `equals` e `hashCode` têm um contrato clássico em que objetos iguais precisam produzir o mesmo hash. Cumprir uma interface é honrar as duas camadas.

Sem um contrato explícito, o código passa a depender de classes concretas. Um serviço que grava clientes chamando diretamente `new ClienteJdbcDao()` fica preso àquela implementação: trocar o banco por outro armazenamento exige editar o serviço; testar o serviço exige um banco de verdade no ambiente de teste; e duas pessoas não conseguem trabalhar em paralelo nas duas pontas porque não há uma fronteira acordada entre elas.

A interface-contrato fixa essa fronteira. O serviço declara que precisa de "algo que saiba salvar e buscar clientes" e nada mais; qualquer implementação que respeite o contrato serve, e as implementações se tornam intercambiáveis. Esse é o princípio da inversão de dependência: o código de alto nível depende da abstração, não do detalhe.

```java
public interface RepositorioClientes {
    void salvar(Cliente c);
    Optional<Cliente> buscarPorId(long id);
}

public class ServicoDeCadastro {
    private final RepositorioClientes repo;   // depende do contrato, não da classe

    public ServicoDeCadastro(RepositorioClientes repo) {
        this.repo = repo;
    }

    public void cadastrar(Cliente c) {
        repo.buscarPorId(c.id()).ifPresent(existente -> {
            throw new IllegalStateException("cliente já existe");
        });
        repo.salvar(c);
    }
}
```

Em produção injeta-se um `RepositorioClientesJdbc`; num teste, um `RepositorioClientesEmMemoria` de dez linhas guardando um `HashMap`. O `ServicoDeCadastro` não muda uma vírgula entre os dois cenários, porque só conhece o contrato.

A analogia é a de um contrato de aluguel. Ele define o que cada lado pode esperar — valor, prazo, estado do imóvel na entrega — sem descrever a rotina do inquilino nem a vida do proprietário. Enquanto as cláusulas forem respeitadas, o proprietário pode ser trocado por um herdeiro sem que o inquilino precise renegociar nada. No código, o "inquilino" é quem chama a interface, e trocar o "proprietário" é substituir a implementação por outra que cumpra as mesmas cláusulas.

Vale insistir que a parte semântica do contrato é responsabilidade de quem implementa, já que o compilador só garante as assinaturas. Uma implementação de `RepositorioClientes.buscarPorId` que devolvesse `null` em vez de `Optional.empty()` compilaria sem reclamação, mas quebraria o contrato e derrubaria quem confia nele.

A alternativa que este conceito acrescenta às já vistas é o teste: ferramentas de mock conseguem simular uma classe concreta sem nenhuma interface de permeio, embora com mais fricção e presas ao tipo real. Fora isso, valem as opções de sempre — uma classe abstrata cumpre o papel de contrato quando há implementação comum a carregar, gastando a vaga de herança, e depender direto da classe concreta continua aceitável em código pequeno e estável.

Como já observado em `interface`, não vale introduzir um contrato quando há uma única implementação e nenhuma perspectiva realista de outra. Se, além disso, nenhum teste ganha com a troca de implementação, a abstração é só indireção — um arquivo repetindo a API pública de uma classe, sem devolver flexibilidade que alguém vá usar.

## Recursos das interfaces

O capítulo anterior descreveu a interface clássica do Java: um tipo só de assinaturas, sem corpo de método e sem estado de instância. Essa descrição valeu integralmente até o Java 7. A partir do Java 8, e depois no Java 9, a interface ganhou recursos que a aproximam de uma classe em alguns pontos, sem deixar de ser um contrato. Este capítulo cobre quatro deles, na ordem em que aparecem no roteiro: os _default methods_, que trazem um corpo herdável; os métodos estáticos, que hospedam funções utilitárias no próprio tipo; os métodos privados, que permitem compartilhar lógica entre os _defaults_ sem expô-la; e as constantes, que, ao contrário dos outros três, sempre existiram — e cujo mau uso tem nome próprio. O fio comum é que a interface deixou de ser apenas uma lista de promessas e passou a poder carregar parte da implementação dessas promessas.

### Default methods

Um _default method_ é um método de interface que, ao contrário dos demais, vem com corpo. Ele é declarado com a palavra-chave `default` antes do tipo de retorno — `default String resumo() { ... }` — e passou a existir no Java 8. Toda classe que implementa a interface herda esse método pronto, exatamente como herdaria um método concreto de uma superclasse, e só precisa escrever a sua própria versão se quiser um comportamento diferente. É o recurso que quebrou a regra antiga de que "interface não tem implementação".

O problema que motivou sua criação foi a evolução de interfaces já publicadas. Antes do Java 8, acrescentar um método a uma interface era uma mudança que quebrava todo o mundo: cada classe que implementava aquela interface parava de compilar até fornecer um corpo para o método novo. Isso engessava as bibliotecas. Quando a equipe do Java quis adicionar `stream()` e `forEach()` a `Collection` e `Iterable` — interfaces implementadas por milhares de classes dentro e fora do JDK —, não havia como fazê-lo sem inviabilizar a atualização para uma quantidade imensa de código já existente.

O `default` resolve isso oferecendo uma implementação de reserva. A interface ganha o método com um corpo que funcione para o caso geral; as implementações antigas continuam compilando e passam a ter o método de graça; e quem precisar de algo específico sobrescreve normalmente. Foi assim que `Iterable.forEach()` chegou implementado em termos de um laço sobre o iterador, deixando cada coleção livre para oferecer uma versão mais eficiente.

```java
public interface Notificador {
    void enviar(String destino, String mensagem);

    default void enviarParaTodos(List<String> destinos, String mensagem) {
        for (String d : destinos) {
            enviar(d, mensagem);   // usa o método abstrato que a classe implementa
        }
    }
}

public class NotificadorEmail implements Notificador {
    @Override
    public void enviar(String destino, String mensagem) { /* ... */ }
    // enviarParaTodos vem pronto, sem escrever nada
}
```

`NotificadorEmail` só implementa `enviar`; `enviarParaTodos` é herdado. Um `NotificadorSms` que consiga mandar um lote numa única chamada de API pode sobrescrever `enviarParaTodos` para aproveitar isso.

A alternativa tradicional é a classe abstrata com método concreto — o padrão Template Method visto no capítulo de classes abstratas —, preferível quando há estado a compartilhar, já que interface não tem campos de instância. Métodos utilitários estáticos numa classe à parte também cobrem parte dos casos, sem entrar na hierarquia de tipos.

Evite usar `default` para escrever lógica de negócio central, que fica escondida num lugar onde ninguém espera implementação. Há ainda o conflito de herança múltipla: se uma classe implementa duas interfaces que trazem um _default_ com a mesma assinatura, o código não compila até a classe desempatar explicitamente, sobrescrevendo o método e, se quiser, chamando uma das versões com `Notificador.super.enviarParaTodos(...)`. E um _default_ nunca deve depender de detalhes que só algumas implementações têm — ele precisa fazer sentido para qualquer classe que assine o contrato.

### Static methods

Uma interface também pode conter métodos estáticos, igualmente desde o Java 8: métodos com corpo, marcados com `static`, chamados pelo nome da interface — `Comparator.naturalOrder()`, `List.of(1, 2, 3)`, `Path.of("/tmp")`. Diferente do _default method_, o método estático não pertence a nenhuma instância e não é herdado: uma classe que implementa a interface não "ganha" esse método, e nem uma subinterface o enxerga pelo nome simples. Ele fica preso ao tipo onde foi declarado.

Antes do Java 8, funções auxiliares ligadas a uma interface não tinham onde morar junto dela, porque interface só admitia assinaturas. A solução consagrada era criar uma classe utilitária irmã, quase sempre com o nome no plural: `Collection` e `Collections`, `Path` e `Paths`, `Executor` e `Executors`. Quem lia o código precisava saber que os métodos de fábrica e os _helpers_ de `Collection` estavam, na verdade, em outra classe, com outro nome. Essa separação era burocracia de linguagem, não uma decisão de design.

O método estático em interface elimina esse desvio: o utilitário fica no mesmo tipo que ele serve. Os principais usos são métodos de fábrica — que criam instâncias de implementações privadas sem expor as classes — e funções que combinam ou adaptam objetos do contrato.

```java
public interface Desconto {
    double aplicar(double valor);

    static Desconto percentual(double taxa) {
        return valor -> valor * (1 - taxa);      // implementação anônima, escondida
    }

    static Desconto nenhum() {
        return valor -> valor;
    }
}

double preco = Desconto.percentual(0.1).aplicar(200);   // 180.0
```

Quem usa `Desconto` nunca vê as classes que implementam o cálculo; pede a `Desconto.percentual(...)` e recebe algo que cumpre o contrato. O método de fábrica ainda dá um nome legível a cada variação, melhor do que um construtor sobrecarregado.

A velha classe utilitária `final` com construtor privado continua válida quando os _helpers_ são muitos ou não se encaixam bem no tipo. Um _default method_ resolve casos parecidos quando a operação precisa de uma instância para trabalhar; um `enum` serve quando as variações formam um conjunto fechado.

O método estático não é polimórfico: não pode ser sobrescrito e é resolvido pelo tipo escrito no código, nunca pelo objeto. Não o use quando o comportamento deveria variar conforme a implementação — esse é o papel dos métodos de instância. E resista a acumular na interface uma coleção de utilitários que só têm relação vaga com o contrato; a interface deve continuar legível como a descrição de uma capacidade.

### Private methods

O método privado em interface, disponível a partir do Java 9, é um método com corpo visível apenas dentro da própria interface. Ele não faz parte do contrato: nenhuma classe implementadora o herda, e nenhum código externo consegue chamá-lo. Existe em duas formas — `private` de instância, que os _default methods_ podem chamar, e `private static`, que tanto os _defaults_ quanto os métodos estáticos da interface podem chamar.

O recurso nasceu de um efeito colateral dos _default methods_. Assim que interfaces passaram a ter vários métodos com corpo, apareceu duplicação entre eles — a mesma validação, a mesma montagem de mensagem, o mesmo laço repetidos em dois ou três _defaults_. No Java 8 não havia como fatorar esse trecho comum sem uma de duas saídas ruins: copiar o código de um _default_ para o outro, ou extraí-lo para um método `default` público, que então passava a integrar o contrato — toda classe implementadora o herdava e podia sobrescrevê-lo, expondo como API algo que era só um detalhe interno.

O método privado fecha essa lacuna. O trecho compartilhado vai para um método `private`, invisível de fora, e os _defaults_ o chamam à vontade.

```java
public interface Validador {

    default boolean nomeValido(String nome) {
        return preenchido(nome) && nome.length() <= 100;
    }

    default boolean emailValido(String email) {
        return preenchido(email) && email.contains("@");
    }

    private static boolean preenchido(String s) {   // detalhe interno, não é contrato
        return s != null && !s.isBlank();
    }
}
```

`preenchido` some da visão de quem implementa `Validador`; os dois _defaults_ compartilham a checagem sem que ela vire uma operação pública do tipo.

Quando a lógica comum é maior ou também interessa às implementações, cabe uma classe auxiliar _package-private_ com métodos estáticos. Uma classe abstrata resolve o mesmo problema quando já se está disposto a gastar a única vaga de herança da implementação. Se o trecho repetido for trivial, às vezes é mais claro simplesmente aceitar a repetição.

Não há muito "quando não usar": o método privado só faz sentido a partir do momento em que existem dois ou mais membros com corpo na interface partilhando lógica. Com um único _default method_, não há o que extrair. E se você percebe que a interface acumulou tanta implementação privada a ponto de parecer uma classe disfarçada, o sinal é que aquele comportamento deveria estar numa classe de verdade, com a interface voltando a ser um contrato enxuto.

### Constantes

Um campo declarado dentro de uma interface é, sempre e automaticamente, uma constante: o compilador trata todo campo de interface como `public static final`, mesmo que esses modificadores não sejam escritos. Não existe campo de instância em interface. Por isso a linha `double BASE_MINIMA = 1000.0;` dentro de uma interface cria uma constante pública, associada ao tipo e não a objetos, que precisa ser inicializada ali mesmo na declaração. Ao contrário dos _default_, _static_ e _private methods_, isso não é novidade de nenhuma versão recente — funciona assim desde o Java 1.

O uso legítimo é guardar valores fixos diretamente ligados ao contrato, referenciados pelo nome da interface: `Tributavel.BASE_MINIMA`. Como o campo é `static final`, ele serve para constantes de configuração do próprio contrato — limites, chaves, valores-padrão que os _default methods_ ou a documentação das assinaturas precisam mencionar.

```java
public interface CanalPagamento {
    int TIMEOUT_SEGUNDOS = 30;
    int MAX_TENTATIVAS = 3;

    Recibo cobrar(BigDecimal valor);

    default Recibo cobrarComRetentativa(BigDecimal valor) {
        for (int i = 1; i <= MAX_TENTATIVAS; i++) {
            try {
                return cobrar(valor);
            } catch (FalhaTemporaria e) {
                if (i == MAX_TENTATIVAS) throw e;
            }
        }
        throw new IllegalStateException();
    }
}
```

Aqui as constantes existem para o _default method_ que vive na mesma interface — a ligação com o contrato é real.

A alternativa para agrupar constantes que não pertencem a um contrato é uma classe `final` com construtor privado e campos `static final`, que é a forma canônica em Java para "um punhado de valores fixos relacionados". Quando as constantes são um conjunto fechado de opções nomeadas — dias da semana, status de um pedido —, o certo é um `enum`, que dá segurança de tipo em vez de um `int` ou uma `String` solta. E uma constante usada só por uma classe deve ser um `static final` dentro dessa própria classe.

O que se deve evitar tem nome consagrado: o _constant interface antipattern_. Consiste em criar uma interface sem nenhum método, só com constantes, e fazer as classes a implementarem apenas para escrever os nomes curtos sem qualificação. O problema é que `implements` passa a afirmar algo falso — a classe não se compromete com nenhum comportamento —, e essas constantes vazam para a API pública do tipo e para todas as suas subclasses, um detalhe de implementação que não deveria estar visível. Se o objetivo é apenas encurtar os nomes, o recurso correto é o `import static` sobre uma classe utilitária. Essa recomendação é o Item 22 de _Effective Java_.

## Interface × classe abstrata

Os dois capítulos anteriores apresentaram a classe abstrata e a interface cada uma por si, com comparações só de passagem: "a interface é a contraparte pura", "a classe abstrata serve quando há estado a compartilhar". Este capítulo junta as duas e encara a decisão de frente, por quatro ângulos: o papel da composição, o papel da herança, o que cada construção significa como contrato, e as perguntas práticas que orientam a escolha num projeto real. A regra de fundo, que os quatro conceitos vão detalhar, é curta de enunciar — interface quando se quer declarar uma capacidade que tipos sem parentesco podem oferecer; classe abstrata quando existe uma base de implementação e um estado genuinamente comuns a uma família de classes aparentadas.

### Composição

Na comparação entre interface e classe abstrata, a composição é a terceira opção que muda o peso dos dois lados. Compor é, em vez de herdar de uma base, manter dentro da classe uma referência a outro objeto e encaminhar chamadas a ele. Isso pesa na decisão do capítulo porque adotar uma classe abstrata como mecanismo de reúso gasta a única vaga de herança da classe e a prende a uma linhagem; adotar uma interface deixa a classe livre para obter comportamento compondo colaboradores. Por isso a dupla "interface + composição" é a combinação flexível, e a classe abstrata é o reúso por parentesco. O tratamento completo de composição — a relação "tem um", delegação, baixo acoplamento — é o próximo capítulo; aqui interessa apenas o efeito dela sobre a decisão interface × classe abstrata.

O problema aparece quando a classe abstrata é a única ferramenta de reúso à mão. Suponha que vários tipos de relatório precisem de um mesmo passo de formatação de números. Colocando esse passo numa superclasse abstrata `RelatorioBase`, todo relatório passa a ser um `RelatorioBase` e não pode mais estender nenhuma outra coisa — mesmo que "relatório de vendas é um relatório-base" seja uma verdade só de conveniência, inventada para não repetir a formatação. A hierarquia nasce torta e a base fica frágil: mexer nela repercute em toda a descendência.

Modelando a capacidade como interface e obtendo a implementação comum por composição, a amarra some:

```java
interface Tributavel {
    double calcularImposto();
}

class Pedido implements Tributavel {
    private final CalculadoraImposto calculadora;   // colaborador composto
    private final BigDecimal total;

    Pedido(CalculadoraImposto calculadora, BigDecimal total) {
        this.calculadora = calculadora;
        this.total = total;
    }

    @Override
    public double calcularImposto() {
        return calculadora.sobre(total);            // delega ao colaborador
    }
}
```

`Pedido` cumpre o contrato `Tributavel` (o tipo) e reaproveita o cálculo por delegação (a implementação), sem gastar `extends`. Pode implementar outras interfaces e compor outros colaboradores da mesma forma. É a diferença entre um aparelho de som modular, em que o amplificador é uma peça separada que se troca sem desmontar o resto, e um rádio portátil com tudo soldado na mesma placa: a interface é o encaixe padronizado, e o colaborador composto é a peça que se pluga nele.

As alternativas são as já conhecidas. Quando a família de classes é de fato um "é um" com estado e invariantes comuns, a classe abstrata com template method é mais simples do que espalhar composição por toda parte. Quando há comportamento a compartilhar mas nenhum estado, um _default method_ na interface entrega o corpo pronto sem exigir um objeto auxiliar.

Compor tem custo: cada capacidade acrescenta um campo e métodos de encaminhamento que só repassam a chamada, e um excesso de delegação polui a classe com burocracia. Se a relação entre os tipos é genuinamente hierárquica e a base carrega estado real, a herança de uma classe abstrata expressa isso com menos cerimônia. A composição rende quando o vínculo é "usa um" e quando a liberdade de trocar a peça interna compensa a indireção.

### Herança

Herança é o eixo em que interface e classe abstrata mais se distinguem. Uma classe abstrata oferece herança única de implementação: a subclasse recebe campos, construtores, métodos concretos e a obrigação dos métodos abstratos — tudo isso de uma só classe. Uma interface oferece herança de tipo e, desde o Java 8, de comportamento através dos _default methods_, mas nunca de estado; e uma classe pode herdar de quantas interfaces quiser. Entender a escolha entre as duas é entender o que cada lado permite herdar.

O motivo de a linguagem separar as duas coisas é o problema do diamante. Se Java permitisse `extends` de duas classes e ambas tivessem um campo `nome` ou um método `salvar()` com corpos diferentes, não haveria resposta clara para qual versão a subclasse usaria. Herança múltipla de estado é ambígua, e por isso é proibida. Só que uma classe real quase sempre precisa ser várias coisas ao mesmo tempo: um `Gerente` é um funcionário, mas também é comparável por salário e pode ser serializado. Com classes abstratas para cada uma dessas capacidades, a combinação seria impossível.

A interface desfaz o nó porque não há estado para entrar em conflito. Quando duas interfaces trazem um _default method_ de mesma assinatura — situação já vista no capítulo anterior —, o compilador não escolhe sozinho: obriga a classe a desempatar, sobrescrevendo o método. Já a classe abstrata é a opção certa quando o que se precisa herdar é justamente campo e lógica de construtor — uma espinha "é um" única e estável.

```java
abstract class Funcionario {              // espinha: estado + construtor
    protected final String nome;
    protected double salario;

    Funcionario(String nome, double salario) {
        this.nome = nome;
        this.salario = salario;
    }

    abstract double calcularBonus();
}

class Gerente extends Funcionario
        implements Tributavel, Comparable<Gerente> {   // uma espinha, dois contratos

    Gerente(String nome, double salario) { super(nome, salario); }

    @Override double calcularBonus()          { return salario * 0.2; }
    @Override public double calcularImposto() { return salario * 0.11; }
    @Override public int compareTo(Gerente o) { return Double.compare(salario, o.salario); }
}
```

`Funcionario` carrega `nome`, `salario` e o construtor; `Tributavel` e `Comparable` não carregam estado nenhum, só assinaturas. A analogia é a do sobrenome contra as certificações profissionais: o sobrenome vem de uma linhagem única e não se acumula, enquanto as habilitações de uma carteira de trabalho são várias e se somam sem conflito. No código, `extends Funcionario` é o sobrenome, e cada `implements` é uma habilitação a mais.

Duas saídas contornam a herança de classe: a composição, quando mesmo a herança única acopla demais, e a hierarquia `sealed`, quando o conjunto de subtipos é fechado e conhecido. Não recorra a uma classe abstrata só para compartilhar dois métodos utilitários — isso consome a vaga de `extends` para sempre, em troca de pouco. Evite também hierarquias abstratas profundas, de quatro ou cinco níveis: cada camada nova torna a base mais rígida e qualquer mudança nela mais arriscada. E não conte com estado herdado de uma interface, porque ele não existe.

### Contratos

Tanto uma interface quanto uma classe abstrata definem um contrato — o conjunto de operações que quem chama pode invocar com segurança. A diferença está em quanto cada uma entrega junto do contrato. A interface é contrato e, opcionalmente, algum comportamento-padrão via _default_, sem estado. A classe abstrata é contrato mais implementação parcial, mais campos, mais construtor. A distinção entre a camada sintática do contrato (as assinaturas, que o compilador cobra) e a camada semântica (o que cada método promete e o compilador não verifica) já foi tratada no capítulo de interfaces; aqui a pergunta é com qual das duas construções expressar o contrato.

Sem uma visão clara disso, a classe abstrata acaba virando o "tipo-base padrão": todo contrato passa a arrastar uma implementação e a queimar a vaga de herança de quem for implementá-lo. O erro oposto também acontece — escolher interface e depois repetir o mesmo esqueleto de código em cada implementação, porque o contrato tinha, na verdade, uma sequência de passos comum que pedia um lugar para morar.

O critério é direto. Se o contrato é só "isto é o que se pode chamar, o como é problema seu" e tipos sem parentesco podem querer cumpri-lo, use interface. Se cumprir o contrato envolve sempre a mesma ordem de passos ou o mesmo estado, use classe abstrata com template method. Dá também para combinar os dois, no estilo que o próprio JDK adota: uma interface pública como contrato e uma classe abstrata "esqueleto" opcional ao lado, que implementa a parte repetitiva.

```java
public interface Lista<T> {                 // o contrato
    int tamanho();
    T get(int i);
    void adicionar(T item);
}

public abstract class ListaBase<T> implements Lista<T> {   // conveniência opcional
    @Override
    public boolean equals(Object o) { /* compara elemento a elemento, igual para todas */ }

    @Override
    public String toString() { /* [a, b, c] — mesma lógica para qualquer lista */ }
}
```

Quem implementa `Lista` estende `ListaBase` se lhe for conveniente herdar `equals` e `toString` prontos, ou implementa `Lista` diretamente se já tiver outra superclasse. É exatamente o padrão de `Collection` com `AbstractCollection`, de `Map` com `AbstractMap`. O código cliente sempre declara a dependência pelo tipo da interface; a classe abstrata é só um atalho para quem escreve implementações. A imagem é a de uma franquia: o contrato de franquia diz o que a loja deve entregar, e o manual de operação que vem junto é opcional — o franqueado pode segui-lo ou montar a própria cozinha, desde que cumpra o contrato.

Fora dessas, valem as alternativas de sempre: `record` ou `enum` para contratos de valor fechados, e classe concreta direta em código pequeno e estável, sem polimorfismo.

Não exprima um contrato como classe abstrata se qualquer implementador plausível já tiver uma superclasse — você tornou o contrato inútil para ele. E não acrescente a classe-esqueleto quando não há implementação comum de verdade para colocar dentro: uma camada vazia só aumenta a distância entre quem lê o código e o que ele faz.

### Escolhas arquiteturais

Escolhas arquiteturais, aqui, é o procedimento prático que consolida o capítulo: um punhado de perguntas cujas respostas apontam para interface, para classe abstrata ou para composição. Elas importam porque a decisão é cara de reverter — transformar uma interface em classe abstrata, ou o contrário, depois que dezenas de classes já dependem do tipo é uma mudança que quebra código alheio.

As perguntas, na ordem em que costumam resolver o caso:

- **Tipos sem parentesco precisam desta capacidade, ou de várias capacidades ao mesmo tempo?** Se sim, interface — só ela se acumula sem conflito.
- **Existe estado comum de verdade — campos, lógica de construtor, invariantes?** Se sim, classe abstrata; interface não guarda estado.
- **Há uma sequência fixa de passos com poucos pontos variáveis?** Classe abstrata com template method: um método `final` fixa a ordem e chama ganchos `protected`.
- **Você controla todas as implementações, e o conjunto é pequeno e fechado?** Classe abstrata ou hierarquia `sealed` servem; se o conjunto é aberto, prefira interface pela liberdade de evolução.
- **O tipo vai ganhar operações novas com o tempo?** Interface com _default methods_ absorve acréscimos sem quebrar quem já implementa.
- **A relação é "é um", "consegue fazer" ou "usa um"?** "É um" com base comum → classe abstrata; "consegue fazer" → interface; "usa um" → composição.

A orientação moderna que resume tudo isso: comece com uma interface; acrescente uma classe abstrata esqueleto só quando a duplicação realmente aparecer; e prefira composição a qualquer uma das duas quando o que você quer é apenas reaproveitar comportamento, não montar uma hierarquia de tipos.

Um exemplo de cada lado. Ao desenhar um `MeioDePagamento`, com muitos provedores sem relação entre si, um conjunto de operações que ainda vai crescer e nenhum estado partilhado, a resposta é interface — talvez com um ou dois _default methods_ e uma classe auxiliar _package-private_ para a lógica repetida. Já ao desenhar o `ImportadorDeArquivo` do capítulo de classes abstratas — mesmo esqueleto de abrir, iterar e fechar, estado de _buffer_ compartilhado, família fechada de formatos —, a resposta é classe abstrata com template method.

A combinação "interface pública + classe abstrata esqueleto", detalhada no conceito de contratos, entrega contrato e reúso ao mesmo tempo, ao custo de manter dois tipos em vez de um.

Não superprojete: para uma única implementação, sem polimorfismo à vista, não é preciso nem interface nem classe abstrata — uma classe concreta basta. E não trate a escolha como uma sentença definitiva: declarar a dependência pública pelo tipo de uma interface é o que mantém a porta aberta para rever a decisão depois com o mínimo de estrago.

## Composição sobre herança

O módulo apresentou classes abstratas e interfaces, dois jeitos de organizar tipos — um por herança com implementação parcial, outro por contrato puro. Este último capítulo fecha com um princípio que apareceu de passagem várias vezes ao longo do curso: quando o objetivo é apenas reaproveitar comportamento, e não montar uma hierarquia de tipos, compor costuma ser mais seguro do que herdar. Veremos o critério que identifica esse caso — a relação "tem um" —, a técnica que o concretiza — a delegação — e a propriedade que faz dele a escolha mais flexível — o baixo acoplamento.

### Relação "tem um"

A relação "tem um" (em inglês, *has-a*) é o critério que indica quando um objeto deve conter outro como parte, em vez de herdar dele. Assim como "é um" governa a herança — "todo gerente é um funcionário" —, "tem um" governa a composição: "um carro tem um motor", "um pedido tem um cliente", "uma fatura tem itens". Onde a frase "todo X tem um Y" descreve a ligação melhor do que "todo X é um Y", o Y entra como campo dentro de X, e não como superclasse.

O problema que esse critério evita é o uso da herança como simples atalho de reaproveitamento. É comum ver `class Pilha extends ArrayList` só porque uma pilha precisa guardar elementos numa lista, ou `class Cache extends HashMap` pelo mesmo motivo. Compila e funciona no começo, mas "uma pilha é uma lista" é falso: uma pilha *tem* uma lista por dentro. Ao herdar, a pilha passa a expor `add(int, E)`, `remove(int)` e `set(int, E)` — métodos que permitem furar a disciplina LIFO que ela deveria garantir. Além disso, a única vaga de herança da classe foi gasta, e a pilha fica amarrada para sempre a `ArrayList`, sem poder trocar a estrutura interna.

Modelando como "tem um", a estrutura interna vira um detalhe privado:

```java
class Pilha<T> {
    private final List<T> itens = new ArrayList<>();   // a pilha TEM uma lista

    void empilhar(T item) {
        itens.add(item);
    }

    T desempilhar() {
        if (itens.isEmpty()) throw new NoSuchElementException();
        return itens.remove(itens.size() - 1);
    }

    boolean vazia() {
        return itens.isEmpty();
    }
}
```

A `Pilha` expõe só as operações que fazem sentido para ela; o `ArrayList` fica escondido e pode ser trocado por um `LinkedList` ou por um array puro sem que nenhum código cliente perceba. Vale a imagem de um carro: o carro tem um motor, mas o painel não mostra pistões nem válvulas — mostra um pedal e um velocímetro. Trocar o motor a combustão por um elétrico não muda a forma de dirigir. No código, a `Pilha` é o painel, e o `ArrayList` é o motor sob o capô: a interface pública permanece enquanto a peça interna é livre para mudar.

A alternativa a "tem um" é justamente "é um", com herança, e ela é a escolha certa quando existe uma hierarquia de tipos estável e se quer tratamento polimórfico — vários subtipos manipulados através da superclasse. Para reaproveitar uma função de cálculo isolada, sem relação de tipo nem de posse, um método estático utilitário resolve sem nenhum dos dois.

Não force "tem um" a ponto de quebrar um objeto coeso em uma dúzia de peças minúsculas, cada uma com uma responsabilidade trivial: a indireção extra custa mais do que rende. E quando a relação é genuinamente "é um" e o código depende de polimorfismo, trocar herança por composição só para seguir o lema obriga a reescrever à mão o encaminhamento que a herança daria de graça.

### Delegação

Delegação é a técnica que dá vida à composição: um objeto recebe uma chamada e a repassa para um colaborador interno, devolvendo o resultado — às vezes puro, às vezes com algo a mais em volta. Se a relação "tem um" diz que a classe guarda um objeto como campo, a delegação é *como* ela reaproveita o comportamento desse objeto sem herdar dele.

Sem delegação, o comportamento do objeto interno ficaria fora de alcance. O código cliente teria de alcançá-lo por conta própria, escrevendo `carro.getMotor().ligar()` — expondo a peça interna que a composição deveria esconder e criando uma dependência em cadeia: no dia em que `Motor` mudar, todo mundo que chamou `getMotor()` quebra. A saída fácil seria voltar à herança, para que os métodos do colaborador aparecessem "de graça" na classe — de volta a todos os problemas de herdar só para reusar.

Com delegação, a classe externa publica um método próprio que, por dentro, chama o método do colaborador:

```java
class Playlist {
    private final List<Musica> musicas = new ArrayList<>();

    void adicionar(Musica m) {        // encaminha direto
        musicas.add(m);
    }

    int tamanho() {                   // encaminha direto
        return musicas.size();
    }

    Duration duracaoTotal() {         // encaminha e agrega
        return musicas.stream()
                      .map(Musica::duracao)
                      .reduce(Duration.ZERO, Duration::plus);
    }
}
```

`adicionar` e `tamanho` são encaminhamentos de uma linha; `duracaoTotal` usa a lista interna para calcular algo que a lista sozinha não oferece. Em todos os casos, quem controla o que fica visível é a `Playlist`, não a `List`. É a diferença entre um gerente que atende o cliente e resolve internamente com a equipe e um que manda o cliente falar direto com cada funcionário: no primeiro há uma fachada única e estável; no segundo, o cliente precisa conhecer a estrutura inteira do time. A delegação é esse gerente — e, amarrando de volta ao código, a classe externa é a fachada única, enquanto as chamadas internas ao colaborador ficam invisíveis para quem chama.

A alternativa é a herança, que encaminha tudo automaticamente, sem escrever método nenhum, ao custo de expor toda a superfície da superclasse, inclusive o que não interessa. Java não tem uma palavra-chave de delegação automática (como o `by` de Kotlin), então os métodos de encaminhamento são escritos à mão — as IDEs geram esse código repetitivo a partir do campo. Quando o contrato a repassar é grande e padronizado, uma interface comum com uma classe abstrata "esqueleto" ao lado, como visto no capítulo anterior, evita reescrever o mesmo encaminhamento em cada classe.

Delegação deixa de compensar quando a classe externa não faz nada além de repassar dezenas de métodos idênticos, sem esconder, adaptar nem agregar nada: nesse caso o objeto interno poderia ser exposto diretamente, ou a herança seria mais honesta. E encaminhar um método só para, logo em seguida, desfazer o efeito dele é sinal de que o colaborador escolhido não era o certo.

### Baixo acoplamento

Acoplamento é o grau em que uma classe depende de detalhes de outra para funcionar. Baixo acoplamento significa que essa dependência é pequena e, sobretudo, apoiada em abstrações — uma interface — e não na implementação concreta. É a propriedade que torna a composição, na maioria dos casos, mais flexível do que a herança, e o argumento central do lema "composição sobre herança".

A herança é a forma mais forte de acoplamento entre duas classes. A subclasse enxerga os membros `protected` da superclasse, depende da ordem em que os métodos internos se chamam e quebra quando esses detalhes mudam, ainda que a interface pública continue igual — é o problema da classe-base frágil. Como a ligação é fixada em tempo de compilação por `extends`, também não há como trocar a "parte herdada" por outra durante a execução nem substituí-la por uma versão falsa num teste.

A composição afrouxa esses laços: o colaborador entra por um campo do tipo de uma interface, recebido no construtor, e pode ser qualquer implementação dela.

```java
interface Exportador {
    void exportar(Relatorio r, OutputStream saida);
}

class GeradorDeRelatorio {
    private final Exportador exportador;

    GeradorDeRelatorio(Exportador exportador) {   // recebe a dependência pronta
        this.exportador = exportador;
    }

    void publicar(Relatorio r) {
        exportador.exportar(r, destino());        // delega sem saber qual é
    }
}
```

`GeradorDeRelatorio` não sabe se o `Exportador` gera PDF, CSV ou HTML; depende apenas do contrato. Em produção recebe um `ExportadorPdf`; num teste recebe um `ExportadorFalso` que só registra o que foi chamado; amanhã, um `ExportadorXlsx` novo entra sem tocar numa linha do gerador. Pense num aparelho com tomada padrão contra um com o fio soldado direto na rede: o primeiro você liga onde quiser e troca sem chamar eletricista. A interface `Exportador` é essa tomada — e, ligando de volta ao conceito, é ela que mantém o gerador ignorante quanto ao que está do outro lado, que é o que permite trocar esse outro lado à vontade.

As alternativas giram em torno da mesma ideia levada adiante: injetar dependências pelo construtor à mão, como acima, ou por um framework de injeção que monta o grafo de objetos; e declarar campos e parâmetros pelo tipo mais abstrato que resolve. Quanto à herança, vale o critério já visto no conceito de "tem um": ela só compensa o acoplamento forte diante de uma hierarquia de tipos estável em que o polimorfismo é o objetivo.

O cuidado é não confundir baixo acoplamento com abstração infinita. Criar uma interface para uma classe que tem uma só implementação e nunca terá outra, ou injetar o que poderia ser um simples `new`, adiciona camadas que o leitor precisa atravessar sem ganho real. Algum acoplamento é inevitável e saudável: o objetivo é depender de contratos estáveis, não eliminar toda dependência.

---

# Módulo 3 — Classes especiais e objetos imutáveis

Classes aninhadas, locais, anônimas e records são construções que refinam o modelo de objetos do Java, permitindo modelar estruturas mais complexas, encapsular contexto local e — no caso dos records — eliminar cerimônia ao trabalhar com dados imutáveis. Este módulo explora esses mecanismos e o conceito fundamental de imutabilidade: objetos cujo estado não muda após a criação, o que reduz bugs, simplifica concorrência e torna o código mais previsível.

## Classes aninhadas

O Módulo 2 tratou de abstração: interfaces e classes abstratas que definem contratos e moldes para hierarquias inteiras de tipos. Este capítulo muda o foco da relação entre tipos para a relação de escopo entre eles — o que acontece quando uma classe é declarada dentro do corpo de outra. Java chama essas construções de classes aninhadas, e elas aparecem em duas variedades que se parecem muito no código e se comportam de formas bem diferentes: a static nested class, que apenas mora dentro da outra por organização, e a inner class, cujas instâncias ficam presas a uma instância da classe que as contém. Ver as duas lado a lado é a melhor forma de entender por que o modificador `static` muda tudo nesse contexto.

### Static nested class

Uma static nested class é uma classe declarada dentro do corpo de outra classe e marcada com o modificador `static`. Na prática, ela é uma classe de nível superior comum que foi guardada dentro de outra apenas por questão de organização e de nome: o `static` indica que ela não tem vínculo nenhum com instâncias da classe que a envolve. Você se refere a ela como `Externa.Aninhada` e a instancia com `new Externa.Aninhada()`, sem precisar de nenhum objeto da classe externa. É uma das duas formas de classe aninhada que este capítulo compara; a outra, a inner class, faz justamente o vínculo que esta dispensa.

Sem esse recurso, uma classe auxiliar que só faz sentido ao lado de outra precisa ser declarada como classe de nível superior no mesmo pacote. O resultado é um pacote povoado de tipos como `PedidoBuilder`, `PedidoItem` e `PedidoStatus`, todos girando em torno de `Pedido` mas visualmente soltos, sem nada no código que amarre um ao outro além do prefixo no nome. Pior: se `PedidoItem` só deveria ser criado e manipulado por `Pedido`, não há como esconder isso — qualquer classe do pacote enxerga e usa `PedidoItem` diretamente.

Declarar a auxiliar como static nested class resolve os dois pontos. O nome passa a carregar a relação (`Pedido.Item`, `Pedido.Builder`), deixando explícito a quem aquele tipo serve. E a classe aninhada pode ser `private`, ficando completamente invisível fora da classe externa — nesse caso ela é um detalhe de implementação que você altera sem afetar ninguém. Uma static nested class também tem acesso aos membros `private` da classe externa, e vice-versa, o que permite dividir responsabilidades entre as duas sem abrir esses membros para o resto do sistema. Fora isso, ela se comporta como qualquer classe: tem construtores, pode estender outra classe, implementar interfaces e declarar seus próprios membros estáticos. É como a receita de um bolo impressa no verso da embalagem do fermento: vive junto do produto a que serve, e não numa gaveta qualquer, mas continua sendo uma receita comum, que qualquer pessoa lê e segue sem precisar do resto da embalagem.

O uso mais frequente é o padrão Builder, em que uma classe aninhada monta passo a passo um objeto da classe externa:

```java
public class Pizza {
    private final String tamanho;
    private final List<String> ingredientes;

    private Pizza(Builder b) {              // só o Builder constrói uma Pizza
        this.tamanho = b.tamanho;
        this.ingredientes = b.ingredientes;
    }

    public static class Builder {
        private String tamanho = "média";
        private final List<String> ingredientes = new ArrayList<>();

        public Builder tamanho(String t) { this.tamanho = t; return this; }
        public Builder adicionar(String i) { ingredientes.add(i); return this; }
        public Pizza construir() { return new Pizza(this); }
    }
}

Pizza p = new Pizza.Builder()
    .tamanho("grande")
    .adicionar("queijo")
    .construir();
```

`Builder` mora dentro de `Pizza` porque só existe para montá-la, e consegue chamar o construtor `private` de `Pizza` justamente porque classe externa e classe aninhada compartilham acesso aos membros privados uma da outra. O mesmo desenho aparece na biblioteca padrão em `Map.Entry`: um tipo aninhado que representa um par chave-valor e não teria sentido fora de um `Map`.

A alternativa direta é manter a auxiliar como classe de nível superior com visibilidade de pacote (sem `public`), o que faz sentido quando ela é usada por várias classes do pacote e não só por uma. Se a auxiliar existe só para agrupar alguns dados imutáveis, um `record` aninhado diz a mesma coisa com muito menos código. E quando a auxiliar precisa mesmo ler o estado de uma instância da classe externa a cada passo, a escolha certa não é uma static nested class com uma referência passada à mão no construtor, e sim a inner class da próxima seção.

Evite a static nested class quando o tipo tem vida própria e é usado em contextos que nada têm a ver com a classe externa — forçá-lo para dentro dela só cria um nome longo e um acoplamento visual enganoso. Evite também empilhar níveis: uma classe aninhada dentro de outra aninhada dentro de uma terceira rapidamente fica ilegível, e aí vale promover alguma delas a nível superior. O caso em que a instância aninhada precisa mesmo do objeto externo — já apontado acima — é justamente o que motiva a inner class da próxima seção.

### Inner class

Uma inner class é uma classe aninhada declarada sem o modificador `static`. A ausência dessa única palavra muda a natureza da classe: cada objeto de uma inner class nasce ligado a um objeto da classe que a contém e não pode existir sem ele. Por isso a inner class enxerga e usa diretamente todos os membros de instância da classe externa, inclusive os `private`, como se fossem seus. É a segunda variedade de classe aninhada; onde a static nested class da seção anterior apenas compartilha o nome da classe externa, a inner class compartilha o estado de uma instância dela.

Sem esse vínculo automático, uma classe auxiliar que precisa trabalhar sobre os dados de um objeto específico teria de receber esse objeto no construtor e guardá-lo num campo, repetindo `externa.campo` a cada acesso e, quando os campos são privados, dependendo de getters abertos só para isso. Um iterador escrito para uma coleção sua, por exemplo, precisa alcançar o array interno, o tamanho atual e o contador de modificações da coleção — tudo detalhe privado. Passar essa referência à mão funciona, mas é cerimônia repetida e deixa o acoplamento entre as duas classes implícito.

A inner class elimina essa cerimônia. O compilador insere, de forma invisível, uma referência para o objeto externo que criou a instância; dentro da inner class, escrever `tamanho` já alcança o campo `tamanho` da instância externa, e `Externa.this` dá acesso explícito a ela quando há ambiguidade de nomes. A instância é criada a partir de um objeto externo existente, com a sintaxe `externa.new Inner()` — em código dentro da própria classe externa basta `new Inner()`, e o `this` atual é usado. Uma restrição histórica: até o Java 16, uma inner class não podia declarar membros `static` (fora constantes de compilação); desde então isso foi liberado.

```java
public class Turma {
    private Aluno[] alunos = new Aluno[10];
    private int tamanho = 0;

    public void matricular(Aluno a) { alunos[tamanho++] = a; }

    public Iterator<Aluno> iterator() {
        return new IteradorTurma();
    }

    private class IteradorTurma implements Iterator<Aluno> {
        private int indice = 0;

        public boolean hasNext() {
            return indice < tamanho;          // 'tamanho' é o campo da Turma
        }
        public Aluno next() {
            return alunos[indice++];          // 'alunos' também
        }
    }
}
```

`IteradorTurma` lê `alunos` e `tamanho` diretamente, ambos privados da `Turma`, sem nenhum getter, porque toda instância de `IteradorTurma` está atrelada à `Turma` que a criou dentro de `iterator()`. Cada chamada a `iterator()` produz um iterador ligado àquela turma específica. A relação lembra a de um profissional terceirizado alocado dentro de uma empresa cliente: enquanto dura o contrato, ele circula pelas salas e acessa os sistemas internos daquela empresa como se fossem dele, mas não faz sentido falar desse posto de trabalho sem a empresa que o abriga. A inner class é esse posto: existe sempre dentro de uma instância hospedeira e opera com o acesso dela — na `Turma`, cada iterador enxerga o array privado, mas pertence a uma turma e desaparece de vista junto com ela.

A alternativa mais comum hoje é a classe anônima ou a expressão lambda, quando o que se precisa é uma implementação curta e única de uma interface — assunto do próximo capítulo —, já que ambas também capturam o contexto ao redor. Para lógica mais longa, uma static nested class que recebe a instância externa por construtor faz o mesmo trabalho de forma explícita, útil quando você quer o acoplamento visível ou quando a classe às vezes é usada sem um objeto externo. E se a auxiliar não toca em nenhum membro de instância da classe externa, ela simplesmente deveria ser `static`.

O erro clássico é deixar uma classe como inner por esquecimento, sem que ela use nada da instância externa. A referência oculta é criada mesmo assim e mantém o objeto externo vivo enquanto a inner class existir; se a instância da inner class for guardada em algum lugar de vida longa — um cache, um listener registrado —, o objeto externo inteiro fica preso na memória junto, um vazamento silencioso. A regra prática é começar com `static` e só remover o modificador quando o compilador cobrar acesso a um membro de instância. Inner classes também complicam a serialização, porque arrastam a instância externa junto, e devem ser evitadas quando a auxiliar precisa de tempo de vida independente do hospedeiro.

## Classes locais e anônimas

O capítulo anterior manteve as classes auxiliares dentro do corpo de outra classe, mas ainda visíveis para todos os métodos dela. Este capítulo aperta o escopo mais um nível: classes declaradas dentro de um único método, que passam a existir quando o método começa a executar e somem quando ele retorna. São duas formas. A local class tem nome e pode ser instanciada várias vezes dentro do método. A anonymous class junta declaração e criação numa única expressão, sem nome, e sai pronta em uma instância só. As duas compartilham um recurso que as classes aninhadas comuns não têm: enxergar as variáveis locais do método em que nasceram. Esse mecanismo — a captura de contexto — fecha o capítulo e explica tanto a conveniência quanto as pegadinhas dessas construções.

### Local classes

Uma local class tem nome, como qualquer classe, mas vive inteiramente dentro de um bloco de código — na prática o corpo de um método, embora também valha para um construtor ou um bloco de inicialização. Esse nome só existe dentro do bloco onde a classe foi declarada: nenhuma outra parte do programa, nem mesmo outro método da mesma classe, consegue se referir a ela. É o grau mais extremo de encapsulamento de um tipo em Java — mais fechado até que uma inner class `private`, que pelo menos é visível para a classe externa inteira. Onde o capítulo anterior colocava a auxiliar dentro do corpo de outra classe, a local class a coloca dentro de um único método.

O problema que ela resolve aparece quando um método precisa de um tipo auxiliar que nenhum outro método usa. Sem local classes, a saída é declarar esse tipo como classe aninhada `private` da classe externa. Funciona, mas espalha pelo arquivo um tipo que só faz sentido no contexto de um método específico, e quem lê a classe precisa descer até esse método para entender por que aquele `private class` existe. Se três métodos diferentes têm, cada um, sua própria auxiliar, a classe externa acumula três tipos aninhados soltos, e a ligação entre cada auxiliar e o método que a usa fica só na cabeça de quem escreveu.

Declarar a classe dentro do método aproxima a definição do uso. O tipo nasce, é usado e some no mesmo bloco; quem lê o método vê tudo o que precisa sem sair dali, e quem lê o resto da classe não é distraído por um tipo que não lhe diz respeito. Como toda classe aninhada, a local class enxerga os membros — inclusive `private` — da instância da classe externa; e, diferente das aninhadas comuns, enxerga também as variáveis locais e os parâmetros do método, desde que sejam efetivamente finais (assunto da última seção). Ela pode implementar interfaces, estender outra classe, ter vários construtores e ser instanciada quantas vezes você quiser dentro do bloco — é isso que a separa da classe anônima da próxima seção, que sai pronta numa única instância.

Um caso concreto: um método que monta um relatório e precisa de uma pequena estrutura para agrupar as linhas já validadas antes de construir o objeto final.

```java
public Relatorio gerar(List<Registro> registros, LocalDate corte) {
    class Linha {
        final String rotulo;
        final BigDecimal valor;
        Linha(String rotulo, BigDecimal valor) {
            this.rotulo = rotulo;
            this.valor = valor;
        }
        boolean dentroDoPrazo(Registro r) {
            return !r.data().isAfter(corte);   // 'corte' é o parâmetro do método
        }
    }

    List<Linha> linhas = new ArrayList<>();
    for (Registro r : registros) {
        Linha l = new Linha(r.rotulo(), r.valor());
        if (l.dentroDoPrazo(r)) linhas.add(l);
    }
    return new Relatorio(linhas);
}
```

`Linha` só tem sentido dentro de `gerar`, usa o parâmetro `corte` diretamente e não precisa aparecer no resto da classe. Fora desse padrão de preparação de dados, local classes aparecem também em implementações de `Iterator` ou `Comparator` que exigem alguma lógica de montagem antes de devolver o objeto.

As alternativas seguem uma escala de formalidade. Se a auxiliar é só um punhado de dados imutáveis, um `record` local — permitido desde o Java 16 e declarado da mesma forma dentro do método — diz o mesmo em uma linha. Se ela implementa uma interface e é usada uma única vez, a classe anônima da próxima seção elimina o nome. Se o alvo for uma interface funcional, uma lambda elimina quase tudo. E se o tipo passa a ser útil a outros métodos, ele deve subir para classe aninhada `private` ou para classe de nível superior.

Evite a local class quando o método já está grande: enfiar uma definição de classe no meio dele piora a leitura em vez de melhorar, e extrair a auxiliar para um tipo aninhado nomeado, deixando o método principal enxuto, costuma ser mais claro. Evite-a também quando perceber que copiou a mesma local class para dois métodos — isso é sinal de que ela quer ser um tipo compartilhado, não local.

### Anonymous classes

Uma anonymous class (classe anônima) é uma expressão que declara uma classe e cria uma instância dela ao mesmo tempo, sem lhe dar nome. A sintaxe parte de um `new` seguido de uma interface ou classe existente e de um corpo entre chaves: `new Comparator<String>() { ... }`. O que está entre as chaves é uma implementação (ou subclasse) definida ali e instanciada na mesma linha. Você nunca escreve o nome dela porque ela não tem nome; a única referência que existe é o objeto devolvido pela expressão. É o encerramento natural da progressão do capítulo: a local class sem nem o nome.

Implementar uma interface para usar uma única vez, sem esse recurso, exige uma local class nomeada — ou, pior, uma classe aninhada — só para ser instanciada uma linha depois e nunca mais mencionada. O nome, nesse caso, é burocracia: não documenta nada, porque o tipo não é reutilizado, e ainda obriga quem lê a procurar onde ele aparece para descobrir que é uma vez só. Antes das lambdas, praticamente todo callback em Java — um listener de botão, uma `Runnable` para uma thread, um `Comparator` para ordenar — passava por esse ritual.

A classe anônima corta o intermediário. A implementação fica escrita exatamente no ponto onde o objeto é necessário, normalmente como argumento de um método:

```java
List<String> nomes = new ArrayList<>(List.of("Ana", "bianca", "Carlos"));

nomes.sort(new Comparator<String>() {
    @Override
    public int compare(String a, String b) {
        return a.compareToIgnoreCase(b);
    }
});

Thread t = new Thread(new Runnable() {
    @Override
    public void run() {
        System.out.println("rodando em " + Thread.currentThread().getName());
    }
});
```

Cada expressão dessas produz um objeto único, de um tipo que o compilador nomeia internamente com algo como `Classe$1`. A classe anônima pode ter campos, blocos de inicialização de instância e métodos próprios além dos que sobrescreve, e enxerga tanto os membros da instância externa quanto as variáveis locais efetivamente finais ao redor — a mesma captura de contexto da última seção. O que ela não pode ter é um construtor (não há nome para dar a ele), nem implementar mais de uma interface, nem ser instanciada uma segunda vez.

A alternativa que domina o código moderno é a expressão lambda, disponível quando o alvo é uma interface funcional — uma que tem só um método abstrato. `nomes.sort((a, b) -> a.compareToIgnoreCase(b))` e `new Thread(() -> System.out.println("..."))` fazem o mesmo que os blocos acima com uma fração do texto. A lambda vence quase sempre nesse caso. A classe anônima continua necessária quando a interface tem mais de um método a implementar, quando você precisa de campos ou de um bloco de inicialização, quando quer estender uma classe concreta em vez de implementar uma interface, ou quando precisa que a palavra `this`, dentro do corpo, se refira à própria instância criada — numa lambda, `this` é o `this` do método que a contém, não o objeto da lambda. Se a lógica já cabe num método existente, uma referência de método (`String::compareToIgnoreCase`) é ainda mais curta.

O corpo curto é parte da definição de "vale a pena": passando de algumas linhas, o bloco anônimo fica encravado numa chamada de método, sem nome para ancorar a leitura e sem como ser testado isoladamente — nesse ponto uma local class ou um tipo aninhado nomeado, com teste próprio, ganha. Vale ainda o alerta da seção anterior: implementação que reaparece em vários lugares está pedindo para ser um tipo nomeado, não uma expressão solta. E cada classe anônima é mais uma classe gerada no pacote, carregando a referência implícita à instância externa — se o objeto anônimo for guardado por muito tempo, valem os mesmos riscos de retenção de memória já descritos na inner class.

### Captura de contexto

A captura de contexto é o mecanismo que permite a uma local class, a uma classe anônima ou a uma lambda usar variáveis que não são suas: os parâmetros e as variáveis locais do método em que foram escritas, além dos campos da instância externa. Quando o corpo dessas construções menciona uma variável local do método ao redor, diz-se que ela "captura" essa variável. É o que dá sentido prático às duas seções anteriores — sem captura, uma classe declarada dentro de um método não teria vantagem nenhuma sobre uma declarada fora dele.

Sem esse mecanismo, todo dado do método que a auxiliar precisa usar teria de entrar explicitamente: um parâmetro no construtor da classe, guardado num campo, para cada valor. Uma `Runnable` que só imprime uma mensagem calculada no método viraria uma classe com um campo `String`, um construtor que o recebe e a linha de `new` passando o valor. Multiplicado pelos três ou quatro valores que um callback real costuma usar, é muito código para transportar informação que está logo ali, algumas linhas acima.

Na captura, o compilador faz esse transporte por você. Ao gerar a classe interna, ele cria campos ocultos — um para cada variável local capturada — e os preenche no momento em que a instância é criada, copiando o valor que a variável tinha naquele instante. Por isso a variável capturada precisa ser `final` ou efetivamente final, ou seja, receber valor uma única vez. Se o código pudesse alterá-la depois, haveria duas verdades: o valor novo no método e a cópia antiga dentro do objeto. Java elimina a ambiguidade proibindo a alteração. A captura de campos da instância externa funciona diferente: o que é copiado é a referência `this` do objeto externo, e através dela a classe interna lê e escreve os campos sempre com o valor atual — é por isso que um campo pode ser alterado à vontade e uma variável local não.

```java
public Runnable preparar(String usuario) {
    int tentativas = 3;                       // efetivamente final
    return new Runnable() {
        @Override
        public void run() {
            System.out.println(usuario + " tem " + tentativas + " tentativas");
        }
    };
}
```

`usuario` e `tentativas` deixam de existir quando `preparar` retorna, mas a `Runnable` devolvida continua imprimindo os dois valores muito depois disso, porque cada um foi copiado para dentro do objeto no `new`. Se você tentasse escrever `tentativas = 2;` em qualquer ponto de `preparar`, o código deixaria de compilar.

A restrição do "efetivamente final" tem contornos. O legítimo é reorganizar o código para não precisar reatribuir: calcular o valor definitivo antes e capturá-lo já pronto. Quando é mesmo necessário acumular algo a partir de dentro da construção — somar valores num laço, marcar que um evento ocorreu —, a saída é capturar um objeto mutável em vez de uma variável mutável: um array de um elemento (`int[] soma = {0}`), um `AtomicInteger`, um `StringBuilder` ou um campo de instância. A referência ao objeto não muda, continua efetivamente final; o conteúdo dele, sim. Use isso com parcimônia, porque costuma ser sinal de que um laço comum resolveria melhor.

Um exemplo real em que a cópia importa é o laço. No `for` clássico com índice, a variável de controle é uma só, reatribuída a cada volta; capturá-la nem é permitido, justamente porque seu valor mudaria sob os pés do objeto. Já o `for-each` cria uma variável nova a cada iteração, então capturá-la funciona e cada objeto criado no laço guarda o item daquela volta:

```java
List<Runnable> acoes = new ArrayList<>();
for (String tarefa : tarefas) {
    acoes.add(() -> System.out.println("executando " + tarefa));
}
// cada Runnable imprime a sua própria 'tarefa'
```

A alternativa à captura, sempre disponível, é a passagem explícita por construtor ou por parâmetro do método — mais verbosa, porém mais visível, e obrigatória quando o valor precisa mesmo mudar depois de criado o objeto.

O maior cuidado é com a captura implícita de `this`. Basta a classe anônima ou a lambda mencionar um campo ou um método de instância da classe externa para que a referência ao objeto externo inteiro seja capturada. Se esse objeto interno tiver vida longa — um listener registrado, uma entrada de cache, um executor —, ele prende junto o objeto externo inteiro pelo mesmo tempo, mesmo que já não sirva a mais nada: o mesmo vazamento silencioso descrito na inner class. Quando a construção só precisa de um campo, copie esse campo para uma variável local antes e capture a variável; quando não precisa de nada da instância, uma lambda ou uma classe estática não captura `this` e o problema não existe.

## Imutabilidade

As seções anteriores mostraram construções que capturam e carregam estado de um método para dentro de um objeto. Este capítulo trata de uma decisão anterior a qualquer uma dessas: se o estado de um objeto, uma vez montado, pode ou não ser alterado. Java permite os dois modelos. Um objeto mutável tem campos que mudam de valor ao longo da vida do objeto; um objeto imutável tem o estado fixado no construtor e nunca mais mexido. A escolha entre os dois afeta a segurança contra bugs de compartilhamento, a facilidade de teste e o comportamento sob concorrência, e é por isso que ela aparece aqui como princípio, e não como detalhe de implementação.

O capítulo compara os dois modelos e depois trata da técnica que torna a imutabilidade real quando um objeto guarda referências para outros objetos mutáveis: a cópia defensiva. A ordem é proposital — só dá para entender por que a cópia defensiva é necessária depois de ver o que a falta dela quebra.

### Objetos mutáveis

Um objeto mutável é aquele cujo estado — o conjunto de valores guardados em seus campos — pode mudar depois de o objeto ter sido criado. Na prática, é toda classe que expõe setters ou métodos que alteram seus campos internos: um `ArrayList` ao qual se adicionam elementos, um `StringBuilder` que cresce a cada `append`, uma classe `Conta` com `depositar` e `sacar`. É o modelo padrão em Java: se você declara uma classe com campos comuns e gera getters e setters, tem um objeto mutável. Ele se relaciona diretamente com o encapsulamento visto no Nível 1 — os setters são o ponto onde a validação de invariantes acontece a cada mudança — e com a noção de identidade: um objeto mutável continua sendo "o mesmo objeto", com a mesma referência, mesmo depois de todos os seus valores terem sido trocados.

A mutabilidade existe porque muitas entidades do mundo real de fato mudam de estado ao longo do tempo, e modelar isso alterando o objeto é o caminho direto. Sem objetos mutáveis, registrar que uma conta recebeu um depósito exigiria criar uma conta nova, com o saldo atualizado, e então encontrar e atualizar toda parte do programa que segura uma referência para a conta antiga. Para um objeto compartilhado por muitos pontos do sistema, propagar a nova referência a todos eles é trabalhoso e fácil de errar. O objeto mutável evita isso mantendo a referência estável: quem aponta para a conta hoje continua apontando para ela depois do depósito, e enxerga o novo saldo automaticamente.

```java
public class Carrinho {
    private final List<Item> itens = new ArrayList<>();
    private BigDecimal total = BigDecimal.ZERO;

    public void adicionar(Item item) {
        itens.add(item);
        total = total.add(item.preco());   // o estado do mesmo objeto muda
    }
}

Carrinho c = new Carrinho();
c.adicionar(new Item("café", new BigDecimal("18.90")));
c.adicionar(new Item("filtro", new BigDecimal("7.50")));
// 'c' é a mesma referência o tempo todo; o conteúdo é que evoluiu
```

O carrinho de compras é um exemplo de mutabilidade justificada: ele existe para acumular itens, tem um ciclo de vida claro (aberto, em edição, finalizado) e nenhuma vantagem em virar um objeto novo a cada item adicionado. A analogia é a de uma lousa branca numa sala de reunião: é sempre a mesma lousa presa na parede, e o que muda é o que está escrito nela — apagar e reescrever é exatamente o motivo de se ter uma lousa, e não um cartaz impresso.

A alternativa é o objeto imutável da próxima seção, que troca a alteração no lugar pela criação de uma nova instância a cada mudança. Existem ainda modelos intermediários, como manter o objeto mutável apenas durante a montagem e "congelá-lo" depois, ou registrar cada mudança como um evento separado em vez de sobrescrever o estado.

Objetos mutáveis se tornam um problema quando são compartilhados. Se duas partes do código seguram a referência para o mesmo carrinho, uma alteração feita por uma é vista pela outra sem aviso — o chamado aliasing, fonte de bugs difíceis de rastrear porque a mudança "vem de longe". Em código concorrente, dois threads alterando o mesmo objeto mutável sem sincronização produzem resultados imprevisíveis. E um objeto mutável nunca deve ser usado como chave de `HashMap` ou elemento de `HashSet`: se o seu estado mudar depois de inserido, o `hashCode` muda junto e a estrutura passa a não encontrá-lo. Nesses cenários — compartilhamento amplo, concorrência, uso como chave — a imutabilidade costuma ser a escolha mais segura.

### Objetos imutáveis

Um objeto imutável tem todo o seu estado definido no momento da construção e nunca mais alterado enquanto existir. Não há setters nem métodos que mudem campos: uma vez que o construtor termina, o objeto é uma fotografia fixa. As classes imutáveis mais usadas de Java são justamente as que aparecem em todo programa — `String`, os wrappers como `Integer` e `Double`, `LocalDate` e as demais classes do pacote `java.time`, `BigDecimal`. Quando você chama `"casa".toUpperCase()`, a `String` original não muda; o método devolve uma `String` nova. Esse conceito fecha a base do Módulo 3 porque sustenta os Value Objects e os records, os dois capítulos seguintes.

O problema que a imutabilidade resolve é o do compartilhamento inseguro descrito na seção anterior. Sem ela, cada referência que atravessa uma fronteira — passada para um método, guardada num campo, colocada numa coleção — obriga a uma de duas defesas: copiar o objeto em cada passagem, o que espalha cópias pelo código, ou, sob concorrência, proteger cada acesso com sincronização. O objeto imutável dispensa as duas: se ninguém pode alterá-lo, ele pode ser compartilhado à vontade, passado sem cópia, lido por vários threads ao mesmo tempo sem trava e usado como chave de mapa sem risco.

A receita para tornar uma classe imutável tem cinco pontos, popularizados pelo livro *Effective Java*: não oferecer nenhum método que altere o estado; impedir que a classe seja estendida (marcá-la `final`, ou tornar os construtores privados e expor fábricas), para que uma subclasse não reintroduza mutabilidade; declarar todos os campos `final`; declarar todos os campos `private`; e, quando algum campo aponta para um objeto mutável, garantir que nenhum código de fora obtenha essa referência — o que se faz com cópia defensiva, tema da próxima seção.

```java
public final class Dinheiro {
    private final long centavos;
    private final String moeda;

    public Dinheiro(long centavos, String moeda) {
        this.centavos = centavos;
        this.moeda = moeda;
    }

    public Dinheiro somar(Dinheiro outro) {
        if (!moeda.equals(outro.moeda))
            throw new IllegalArgumentException("moedas diferentes");
        return new Dinheiro(this.centavos + outro.centavos, moeda);  // instância nova
    }

    public long centavos() { return centavos; }
    public String moeda()  { return moeda; }
}

Dinheiro preco = new Dinheiro(1890, "BRL");
Dinheiro comFrete = preco.somar(new Dinheiro(1500, "BRL"));
// 'preco' continua valendo 18,90; 'comFrete' é um novo objeto de 33,90
```

Métodos que "mudam" um objeto imutável na verdade devolvem uma cópia modificada — a convenção é chamá-los de `withX` ou dar-lhes nomes como `somar` e `mais`. A analogia é a de um cheque preenchido e assinado: para corrigir um valor você não rasura o cheque, emite outro e cancela o primeiro. O documento antigo continua existindo, íntegro, como registro do que foi combinado naquele momento — e é essa estabilidade que torna seguro entregá-lo a terceiros.

A alternativa é o objeto mutável com disciplina de uso, confiando que ninguém altere o que não deve; funciona em bases de código pequenas e controladas, mas não escala. Coleções têm um meio-termo: `List.of(...)` cria uma lista imutável, e `Collections.unmodifiableList(...)` embrulha uma lista existente numa visão que recusa alterações. Para objetos com muitos campos opcionais, o padrão Builder monta a instância aos poucos e produz um objeto imutável no final.

A imutabilidade não é gratuita. Cada alteração aloca um objeto novo, e se o objeto é grande ou muda muitas vezes por segundo, essa alocação pesa — um buffer de texto que recebe milhares de `append` deve ser um `StringBuilder` mutável, não uma `String` recriada a cada passo. Entidades com identidade própria e ciclo de vida longo, como as que um ORM mapeia para linhas de banco, também não se encaixam bem: elas representam algo que muda no mundo e cujo "mesmo objeto" precisa refletir essas mudanças. Para dados que fluem entre camadas, são comparados por valor e raramente mudam depois de criados, a imutabilidade é quase sempre o melhor padrão; para estado que evolui continuamente, o objeto mutável ainda é a ferramenta certa.

### Defensive copying

Cópia defensiva é a prática de, ao receber ou ao entregar um objeto mutável, guardar ou devolver uma cópia dele em vez da referência original. Ela existe para fechar o furo mais comum em classes que se pretendem imutáveis: campos `final` de tipo mutável. Marcar um campo `final` impede que ele passe a apontar para outro objeto, mas não impede que o objeto apontado seja alterado por dentro. Se esse objeto veio de fora, ou se uma referência a ele escapa por um getter, a imutabilidade da classe é só aparente.

O caso clássico aparece com uma classe que guarda um intervalo de datas usando o antigo `java.util.Date`, que é mutável:

```java
public final class Periodo {
    private final Date inicio;
    private final Date fim;

    public Periodo(Date inicio, Date fim) {
        this.inicio = inicio;    // guarda a referência que veio de fora
        this.fim = fim;
    }
    public Date inicio() { return inicio; }   // devolve a referência interna
    public Date fim()    { return fim; }
}

Date d1 = new Date();
Date d2 = new Date(d1.getTime() + 86_400_000);
Periodo p = new Periodo(d1, d2);

d1.setTime(0);          // quem chamou ainda tem 'd1' e altera o interior de 'p'
p.inicio().setTime(0);  // e o getter entrega a referência para qualquer um alterar
```

Apesar dos campos `final` e da ausência de setters, `Periodo` pode ser modificado de duas direções: por quem passou as datas ao construtor e ficou com as referências, e por quem chama os getters. A classe não controla o próprio estado.

A correção é copiar nas duas pontas. No construtor, cria-se uma nova instância a partir do valor recebido, antes de qualquer validação — copiar depois de validar abre uma janela em que outro thread altera o objeto entre a checagem e a cópia. Nos getters, devolve-se uma cópia, nunca o campo:

```java
public Periodo(Date inicio, Date fim) {
    this.inicio = new Date(inicio.getTime());   // cópia na entrada
    this.fim = new Date(fim.getTime());
    if (this.inicio.after(this.fim))
        throw new IllegalArgumentException("início depois do fim");
}
public Date inicio() { return new Date(inicio.getTime()); }   // cópia na saída
public Date fim()    { return new Date(fim.getTime()); }
```

Para coleções, a cópia na entrada é `new ArrayList<>(lista)` ou `List.copyOf(lista)`; para arrays, `array.clone()`. É importante que a cópia seja profunda o suficiente: copiar uma `List` mas continuar compartilhando os objetos mutáveis guardados dentro dela apenas move o problema um nível abaixo. A analogia é a de um cartório que recebe um documento para arquivar: ele não guarda a folha que a pessoa trouxe — ela poderia vir buscá-la ou alterá-la depois — e sim tira uma cópia autenticada, arquiva a cópia e, quando alguém pede para consultar, fornece outra cópia, nunca o arquivo original.

Há alternativas mais baratas. A melhor é não aceitar tipos mutáveis: se `Periodo` recebesse e devolvesse `LocalDate`, que é imutável, não haveria nada a copiar — por isso o `java.time` moderno elimina boa parte da necessidade de cópia defensiva. Na saída, `Collections.unmodifiableList` devolve uma visão que recusa alterações sem custo de cópia, embora não proteja contra quem ainda tenha a referência à lista original nem contra alterações nos elementos. Um `record` cujos componentes são todos imutáveis dispensa o assunto inteiro. E documentar o contrato ("a lista passada não deve ser alterada depois") é aceitável entre código do mesmo time, em caminhos quentes onde a cópia pesaria.

Não faça cópia defensiva quando os campos já são de tipos imutáveis — copiar uma `String` ou um `LocalDate` é desperdício, e é por isso que essas classes são tão convenientes. Também não compensa quando o objeto claramente não escapa nem é compartilhado — algo criado e consumido dentro do mesmo método — ou quando a semântica desejada é justamente o compartilhamento, como num cache em que várias partes do sistema devem ver a mesma instância viva. Fora esses casos, numa classe imutável que recebe ou expõe qualquer coisa mutável, a cópia nas duas pontas é obrigatória, não opcional.

## Value Objects

O capítulo anterior mostrou como fixar o estado de um objeto no construtor e nunca mais alterá-lo. Um objeto imutável assim, além de seguro para compartilhar, costuma existir por um único motivo: representar um dado — uma quantia, uma data, um documento. Quando é esse o caso, a pergunta "dois desses objetos são o mesmo?" deixa de ser sobre qual referência você tem na mão e passa a ser sobre o que eles guardam. Esse é o território dos value objects: objetos que valem pelo conteúdo, não pela identidade, e que por isso precisam ensinar ao Java como se comparar.

Este capítulo parte da distinção entre identidade e valor — as duas maneiras de um objeto "ser ele mesmo" —, passa pela igualdade estrutural, que é como a semântica de valor se traduz em código através de `equals` e `hashCode`, e termina na modelagem por valor, a decisão de projeto de transformar conceitos do domínio em tipos pequenos, imutáveis e comparados por conteúdo. Os records do próximo capítulo são, no fundo, um atalho de sintaxe para tudo o que se descreve aqui.

### Identidade × valor

Todo objeto em Java pode ser encarado de dois ângulos. Pelo ângulo da identidade, o que define o objeto é a sua existência individual: ele ocupa um lugar próprio na memória, tem uma referência única e continua sendo "ele" mesmo que todos os seus campos mudem de valor — é o mesmo raciocínio da lousa da seção anterior, sempre a mesma lousa por mais que se apague e reescreva. Pelo ângulo do valor, o que define o objeto é o conjunto de dados que ele carrega: dois objetos com exatamente os mesmos campos são indistinguíveis para efeitos práticos, e tanto faz qual dos dois você usa. Um value object é uma classe deliberadamente projetada para o segundo ângulo — ela não tem identidade própria, só conteúdo.

Java, por padrão, trata tudo pela identidade. O operador `==` compara referências, e o `equals` herdado de `Object` faz a mesma coisa. Isso é o certo para um `Usuario`, uma `Conta`, um `Pedido` — coisas que existem uma vez, têm ciclo de vida e precisam ser rastreadas individualmente. Mas é o comportamento errado para uma quantia em dinheiro, um CPF, um par de coordenadas, um intervalo de datas. Se o programa cria `new Dinheiro(1890, "BRL")` em dois pontos distintos para representar a mesma quantia, esses dois objetos são referências diferentes; `==` dá `false` e o `equals` herdado também. A partir daí, toda pergunta sobre conteúdo — "são a mesma quantia?" — passa a exigir comparação manual, e o objeto se comporta mal dentro de coleções e buscas. Essas consequências concretas são o tema da próxima seção; o ponto aqui é anterior a elas: reconhecer que esse tipo de objeto deveria valer pelo conteúdo, não pela referência.

Distinguir os dois casos é uma decisão de modelagem que se toma classe a classe, antes de escrever qualquer método. A pergunta é: se eu trocar este objeto por outro com os mesmos dados, alguma coisa quebra? Se a resposta é não — uma nota de dez reais serve tão bem quanto qualquer outra nota de dez reais —, o objeto é um valor. Se a resposta é sim — o contrato assinado por você não pode ser substituído por uma cópia com o mesmo texto, porque é aquela via específica que foi registrada —, o objeto tem identidade.

```java
Conta a = new Conta("0001", 100);
Conta b = new Conta("0001", 100);
// a e b NÃO devem ser considerados o mesmo objeto: são cadastros distintos
// que por acaso partilham número; tratá-los como um só arriscaria que a
// gravação de um sobrescrevesse o estado do outro

Dinheiro x = new Dinheiro(1890, "BRL");
Dinheiro y = new Dinheiro(1890, "BRL");
// x e y representam a mesma quantia; um serve no lugar do outro sem consequência
```

Essa escolha define tudo o que vem depois. Um objeto de valor quase sempre é imutável (um valor que muda deixa de ser aquele valor), não tem um campo de `id` e precisa ensinar o Java a compará-lo campo a campo, sobrescrevendo `equals` e `hashCode` — o mecanismo que a seção seguinte detalha. Um objeto com identidade normalmente é mutável, costuma ter um identificador estável (número de conta, chave primária) e, se sobrescreve `equals`, compara só por esse identificador, nunca por todos os campos.

A alternativa a fazer essa distinção é não fazê-la, e há ecossistemas que empurram nessa direção: frameworks de persistência tendem a tratar toda classe mapeada como entidade, com id de banco, mesmo quando ela seria melhor modelada como valor embutido. No extremo oposto, estilos mais funcionais tratam quase tudo como valor imutável. O desenho que o mundo Java adotou, popularizado pelo Domain-Driven Design, é o meio-termo explícito: separar as classes em "entidades" (identidade) e "value objects" (valor) e ser consciente de qual é qual.

Não modele por valor aquilo que o domínio enxerga como uma coisa individual e acompanhável no tempo. "Esta conta", "aquele funcionário", "o chamado nº 4821" são entidades: forçar semântica de valor nelas faz dois registros diferentes com dados momentaneamente iguais colidirem como se fossem um. E, como valor pressupõe imutabilidade, não tente dar semântica de valor a um objeto cujos campos você ainda pretende alterar — os dois conceitos não convivem.

### Igualdade estrutural

Igualdade estrutural é a forma de igualdade em que dois objetos são considerados iguais quando seus campos correspondentes são iguais, um a um — e, se algum campo é ele próprio um objeto, quando esses também são estruturalmente iguais. É a tradução em código da semântica de valor da seção anterior: para um value object se comportar de fato como um valor, não basta a intenção do projetista; a classe precisa sobrescrever `equals` para comparar conteúdo e `hashCode` para acompanhar essa decisão. Sem isso, ela herda de `Object` a igualdade por identidade, que compara referências e ignora os campos.

O estrago de deixar a igualdade herdada num objeto que deveria ser de valor aparece assim que ele encosta nas coleções. Um `HashSet<Cpf>` aceita o mesmo CPF várias vezes, porque cada `new Cpf("111...")` é uma referência nova e o `Set` não vê duplicata. `lista.contains(new Cpf("111..."))` devolve `false` mesmo com aquele CPF na lista. Um `HashMap` cuja chave é um value object nunca reencontra o valor guardado, porque a chave da consulta é um objeto diferente da chave da inserção. E qualquer comparação de "são a mesma coisa?" tem de ser feita à mão, campo por campo, espalhada por todo lugar que precise dela.

A correção é sobrescrever os dois métodos juntos — sempre os dois, nunca só um —, usando os mesmos campos nos dois. O `equals` precisa respeitar o contrato definido em `Object`: ser reflexivo (`x.equals(x)` é `true`), simétrico (se `x.equals(y)`, então `y.equals(x)`), transitivo, consistente entre chamadas, e `x.equals(null)` deve dar `false`. O `hashCode` tem uma regra que o amarra ao `equals`: objetos iguais são obrigados a ter o mesmo código de hash. O contrário não vale — objetos diferentes podem colidir no mesmo hash —, mas violar a regra principal faz o objeto sumir dentro de um `HashMap`. As utilidades `Objects.equals` e `Objects.hash` cuidam dos detalhes, inclusive de `null`.

```java
public final class Cpf {
    private final String digitos;

    public Cpf(String digitos) {
        this.digitos = Objects.requireNonNull(digitos);
    }

    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (!(o instanceof Cpf outro)) return false;
        return digitos.equals(outro.digitos);
    }

    @Override
    public int hashCode() {
        return Objects.hash(digitos);
    }
}
```

Com esses dois métodos no lugar, `new Cpf("111...").equals(new Cpf("111..."))` dá `true`, o `HashSet` passa a rejeitar duplicatas de verdade, e o `Cpf` funciona como chave de mapa. É a diferença entre conferir duas listas de compras perguntando "é a mesma folha de papel?" e conferir item por item: para decidir se as compras são as mesmas, ninguém olha o papel, olha o conteúdo — e é exatamente esse item por item que o `equals` estrutural faz com os campos.

Um detalhe do teste de tipo: usar `instanceof` no `equals`, como no exemplo, aceita instâncias de subclasses; usar `getClass() == o.getClass()` exige o tipo exato. Para value objects, o mais comum é marcar a classe `final` — o que dispensa a preocupação — e usar `instanceof`.

Escrever esse par de métodos à mão é repetitivo e fácil de errar, então quase ninguém mais faz isso do zero: a IDE gera o esqueleto, bibliotecas como o Lombok o produzem a partir de uma anotação e — a forma moderna — o `record` do próximo capítulo já vem com `equals` e `hashCode` estruturais prontos, calculados sobre todos os componentes. O conteúdo desta seção é o que esses atalhos automatizam.

A igualdade estrutural é para value objects, não para entidades. Um objeto com identidade e um campo de `id` deve comparar só pelo `id`: dois `Usuario` com o mesmo nome e a mesma data de nascimento não são a mesma pessoa. Há ainda dois cuidados. Se algum campo do value object for mutável e for alterado depois de o objeto entrar num `HashSet`, o `hashCode` muda e o objeto se perde na estrutura — mais uma razão para value objects serem imutáveis. E comparar campo a campo objetos muito grandes num laço quente tem custo; nesses casos vale medir antes de assumir que é irrelevante.

### Modelagem por valor

Modelagem por valor é a decisão de projeto de representar um conceito do domínio como um tipo próprio — pequeno, imutável e com igualdade estrutural — em vez de deixá-lo espalhado em tipos genéricos como `String`, `double` ou `BigDecimal` soltos. Em vez de passar um e-mail como `String`, cria-se um tipo `Email`; em vez de carregar `valor` e `moeda` como dois parâmetros que andam sempre juntos, cria-se `Dinheiro`; e o mesmo para `Cpf`, `Cep`, `Coordenada`, `Intervalo`. É o capítulo aplicando na prática o que as seções anteriores apresentaram em separado: imutabilidade, ausência de identidade e igualdade por conteúdo, reunidas numa escolha consciente de como desenhar as classes.

O problema que essa modelagem ataca tem até nome: obsessão por primitivos. Quando todo dado é uma `String` ou um número, as assinaturas de método perdem significado — `cadastrar(String, String, String)` não impede que quem chama troque a ordem do nome e do e-mail, e o compilador não reclama. A validação de que "um CPF tem 11 dígitos" ou de que "só se soma dinheiro da mesma moeda" acaba copiada para cada lugar que mexe com esses dados, e cada cópia envelhece por conta própria. Não há um ponto único onde a regra do conceito mora.

Encapsular o conceito num tipo resolve os dois lados. A validação roda uma vez, no construtor, e a partir daí é impossível ter em mãos uma instância inválida. As operações que fazem sentido para aquele valor viram métodos dele, devolvendo novas instâncias, já que o objeto é imutável. E as assinaturas passam a se explicar sozinhas: `transferir(Dinheiro valor, Conta destino)` não deixa mais o `valor` ser confundido com outra coisa nem aceita uma quantia sem moeda.

```java
public final class Email {
    private final String valor;

    public Email(String valor) {
        if (valor == null || !valor.matches("[^@\\s]+@[^@\\s]+\\.[^@\\s]+"))
            throw new IllegalArgumentException("e-mail inválido: " + valor);
        this.valor = valor.toLowerCase();
    }

    public String dominio() {
        return valor.substring(valor.indexOf('@') + 1);
    }
    public String valor() { return valor; }

    @Override public boolean equals(Object o) {
        return o instanceof Email e && valor.equals(e.valor);
    }
    @Override public int hashCode() { return valor.hashCode(); }
}
```

O tipo `Email` normaliza a entrada (tudo minúsculo), recusa texto que não seja e-mail e traz junto o comportamento que pertence ao conceito — `dominio()` fica ao lado do dado em vez de virar um utilitário solto. Um método `cadastrar(Email email)` agora só pode ser chamado com um e-mail que já passou pela validação. É a diferença entre trabalhar com uma peça padronizada — um parafuso M6, especificado, com a rosca certa — e com um pedaço de metal genérico que cada montador precisa limar na hora: o value object chega pronto e encaixa igual em todos os pontos do sistema.

A forma moderna e enxuta de escrever isso é o `record`, tema do próximo capítulo, que elimina o construtor repetitivo, os acessores e o par `equals`/`hashCode`, deixando só a validação. Para um conceito usado num único ponto e sem comportamento próprio, um método de validação estático sobre uma `String` crua ainda é aceitável. Para domínios ricos, costuma haver bibliotecas dedicadas — como as de representação de dinheiro — que já trazem o tipo pronto com arredondamento e conversão de câmbio. E há uma evolução da linguagem em andamento, as *value classes* do Project Valhalla, voltada a baratear a alocação desses objetos pequenos.

O risco da técnica é o excesso. Nem todo dado merece um tipo: uma `int quantidade` local, sem regra associada e usada num só método, não ganha nada em virar uma classe `Quantidade` — só acrescenta uma camada de embrulho e desembrulho. Envolver num tipo de valor um conceito que tem identidade e ciclo de vida próprios é o erro inverso, já discutido na abertura do capítulo: aquilo é entidade. E, em trechos que criam milhões dessas instâncias por segundo, o custo de alocação pode pesar o suficiente para reconsiderar. Fora esses casos, transformar os conceitos centrais do domínio em tipos de valor é uma das formas mais baratas de deixar o código difícil de usar errado.

## Records

Os capítulos anteriores deste módulo montaram, peça por peça, o padrão do objeto imutável que representa um dado: estado fixado no construtor, cópias defensivas, ausência de identidade e igualdade calculada sobre o conteúdo. Escrever tudo isso à mão, classe após classe, é trabalhoso e propenso a erro. O `record`, adicionado à linguagem no Java 16, é o atalho oficial para esse padrão: uma forma de declarar uma classe portadora de dados em que o compilador gera o construtor, os acessores e os métodos `equals`, `hashCode` e `toString` a partir de uma única lista de campos.

Esta aula percorre as partes desse mecanismo. Começa pela declaração `record` em si e pelo que ela permite e proíbe; passa pelos componentes, que são a lista de valores no cabeçalho e a fonte única do estado do tipo; pelos acessores gerados para ler esses valores; e termina nos três métodos que o compilador escreve de graça — `equals` e `hashCode`, que dão ao `record` a igualdade estrutural, e `toString`, que o torna legível em logs e testes.

### `record`

Um `record` é uma forma abreviada de declarar uma classe cujo propósito é transportar dados: você escreve o nome do tipo e a lista de valores que ele guarda, e o compilador gera o resto. A declaração `public record Ponto(int x, int y) {}` produz uma classe imutável, com dois campos `private final`, um construtor que recebe `x` e `y`, métodos de acesso a esses campos, e implementações de `equals`, `hashCode` e `toString` coerentes entre si. Numa linha está o que os capítulos de imutabilidade e value objects montaram à mão: estado fixado no construtor, ausência de identidade, comparação por conteúdo.

Sem `record`, escrever um tipo de dados em Java custa muito texto para pouca informação. Uma classe `Ponto` equivalente tem a declaração dos dois campos, um construtor que só copia parâmetro para campo, dois acessores, um `equals` longo e cheio de regras, um `hashCode` que tem de acompanhá-lo campo a campo, e um `toString`. São quase quarenta linhas em que a única informação relevante — "um ponto é um par de inteiros x e y" — fica diluída. Pior: cada um desses métodos é uma oportunidade de erro. Esquecer um campo no `equals`, deixar o `hashCode` fora de sincronia, não atualizar o `toString` quando um campo novo entra. O leitor da classe também perde: para saber se aquilo é um dado imutável ou uma entidade com regras, ele precisa ler o corpo inteiro.

O `record` resolve isso tornando a intenção explícita e a implementação automática. A linha `record Ponto(int x, int y)` é ao mesmo tempo a documentação — "este tipo é exatamente estes dois valores" — e a especificação a partir da qual o compilador deriva os membros. Os campos são sempre `private final`; não há setter, não há como criar um `Ponto` pela metade. O construtor gerado, chamado construtor canônico, recebe um parâmetro por componente, na ordem declarada. Você ainda pode adicionar métodos, implementar interfaces, declarar campos estáticos e sobrescrever qualquer um dos métodos gerados quando precisar — o que o `record` proíbe é justamente o que atrapalharia seu papel: estender outra classe e ter campos de instância fora da lista de componentes.

```java
public record Dinheiro(long centavos, String moeda) {
    public Dinheiro somar(Dinheiro outro) {
        if (!moeda.equals(outro.moeda))
            throw new IllegalArgumentException("moedas diferentes");
        return new Dinheiro(centavos + outro.centavos, moeda);
    }
}

var a = new Dinheiro(1890, "BRL");
var b = new Dinheiro(1890, "BRL");
a.equals(b);        // true — comparação por conteúdo, de graça
a.centavos();       // 1890
a.toString();       // Dinheiro[centavos=1890, moeda=BRL]
```

A alternativa histórica era gerar esse código com a IDE, que continua sendo uma opção quando o tipo não pode ser um `record`. Bibliotecas como o Lombok fazem o mesmo a partir de anotações, ao custo de uma dependência de terceiros e de processamento na compilação. Antes dos records, o padrão de fato eram os "POJOs" com getters e setters, mutáveis, que são o oposto do que se quer para um dado. Comparado a todos eles, o `record` tem a vantagem de ser da própria linguagem: qualquer pessoa que lê Java sabe o que ele significa, sem depender de convenção de projeto.

Não use `record` para objetos que têm identidade e ciclo de vida — um `Usuario`, uma `Conta`, um `Pedido` que nasce, muda de estado e é rastreado por um `id`; para esses, a igualdade correta é por identificador, não por todos os campos. Não use quando o objeto precisa ser mutável, quando precisa herdar de uma classe concreta, ou quando a API pública do tipo deve ser diferente da sua representação interna — um `record` expõe seus componentes por definição. Fora disso, para qualquer agregado imutável de dados, o `record` é a escolha padrão.

### Componentes

Cada valor listado no cabeçalho de um `record` é um componente. Em `record Intervalo(LocalDate inicio, LocalDate fim) {}`, `inicio` e `fim` são os dois componentes, e é a partir dessa lista que o compilador deriva tudo: para cada componente nascem um campo `private final` de mesmo nome e tipo, um parâmetro no construtor canônico e um método de acesso. A lista de componentes é a definição do estado do `record` — não existe estado de instância fora dela. Onde, numa classe comum, "quais campos formam este objeto" é uma informação implícita, espalhada entre a declaração dos atributos e o corpo do `equals`, num `record` ela está concentrada numa linha, e é a única fonte da verdade.

O problema que isso evita aparece quando o estado de uma classe e os métodos que dependem dele saem de sincronia. Alguém adiciona um campo `observacao` a uma classe de dados, ajusta o construtor, mas esquece de incluí-lo no `equals` e no `hashCode`; a partir daí dois objetos com observações diferentes são considerados iguais, e o defeito só aparece quando um deles some dentro de um `HashSet`. Com componentes, esse desencontro é impossível por construção: incluir `observacao` na lista de componentes já faz o compilador regerar o construtor, o acessor, o `equals`, o `hashCode` e o `toString` levando o campo novo em conta. Um só lugar para mudar.

Um componente pode ser de qualquer tipo — primitivo, `String`, outro `record`, uma coleção, um tipo do domínio. Os nomes seguem as convenções normais de campo e viram diretamente os nomes dos acessores, então vale escolhê-los pensando em como `pedido.total()` vai ler no código que usa o `record`. Aqui entra um cuidado herdado do capítulo de imutabilidade: o `record` garante que a referência de cada componente não muda depois de construído, mas não congela o objeto apontado. Um componente do tipo `List<Item>` pode ter seus elementos alterados por quem tiver a mesma referência; se a imutabilidade precisa ser real, o construtor deve copiar a lista com `List.copyOf`, e o acessor deve devolver uma cópia ou uma visão imutável.

```java
public record Pedido(String cliente, List<String> itens) {
    public Pedido {
        itens = List.copyOf(itens); // cópia defensiva no construtor canônico
    }
}
```

A alternativa é a classe comum, onde você lista os campos e depois repete essa lista, à mão, em cada método derivado. Não há uma alternativa "parcial": ou o tipo é um `record` e todos os seus dados de instância são componentes, ou é uma classe. Justamente por isso o `record` não serve quando parte do estado não deveria contar para a igualdade — um cache interno, um contador de acessos, um instante de "última leitura". Esses campos teriam de ser componentes e entrariam no `equals` e no `toString`, o que quase nunca é o desejado. Nesse caso, ou o dado extra vira responsabilidade de outra classe, ou o tipo não é um bom candidato a `record`.

### Acessores

Para cada componente, o `record` gera um método público de leitura com exatamente o mesmo nome do componente: `record Ponto(int x, int y)` produz `x()` e `y()`, não `getX()` e `getY()`. Esse método devolve o valor do campo correspondente e é a única forma de ler o estado de fora do `record`. Como não há setters e os campos são `private final`, os acessores são a interface de leitura completa do tipo.

Sem essa geração, ler os dados de um objeto significa escrever um getter por campo — três linhas cada, sem nenhuma lógica — ou expor os campos como `public`, o que abre mão de qualquer controle sobre o acesso. A convenção `getX`/`isX` dos JavaBeans padronizou esses métodos, mas à custa de prefixos que não acrescentam informação: `pedido.getTotal()` não diz nada que `pedido.total()` não diga. Os records adotam a forma curta.

Quando o acessor precisa fazer mais do que devolver o campo, você o escreve à mão e o compilador respeita a sua versão. O uso mais comum é a cópia defensiva: um componente mutável não deve ser entregue por referência.

```java
public record Turma(String nome, List<String> alunos) {
    public List<String> alunos() {
        return List.copyOf(alunos); // ninguém altera a lista interna
    }
}
```

Também é possível expor um valor derivado como se fosse um acessor comum, embora um método com outro nome (`pedido.total()` versus `pedido.subtotalComDesconto()`) costume comunicar melhor a diferença entre dado guardado e dado calculado.

A alternativa é o getter manual, ainda necessário quando o tipo não pode ser `record`. Frameworks antigos que exigiam rigorosamente o padrão `getX` já foram, em sua maioria, atualizados para reconhecer acessores de `record`; ainda assim, ao integrar com uma biblioteca muito velha de serialização ou de binding, vale confirmar que ela entende o formato curto antes de modelar a classe como `record`. Fora esse caso de compatibilidade, não há motivo para preferir `getX` num `record`.

### `equals`

O compilador gera para todo `record` um método `equals` que compara dois objetos pelo conteúdo: dois records são iguais quando têm o mesmo tipo e cada componente de um é igual ao componente correspondente do outro. Para componentes primitivos a comparação é por valor (com tratamento especial para `double` e `float`, via `Double.compare`, para lidar com `NaN` e zero negativo); para componentes de referência, usa-se `Objects.equals`, que cuida de `null`. É exatamente a igualdade estrutural que o capítulo de value objects implementou à mão — agora automática e garantidamente coerente com o `hashCode`.

Escrito à mão, o `equals` é um dos trechos mais traiçoeiros de Java: precisa satisfazer todo o contrato de `Object` detalhado na seção de igualdade estrutural — reflexividade, simetria, transitividade, consistência, `false` para `null` —, testar o tipo do argumento e usar exatamente o mesmo conjunto de campos que o `hashCode`. Cada uma dessas exigências é uma chance de erro sutil que não quebra a compilação e só se manifesta muito depois: um objeto que não se encontra num `HashMap`, um `HashSet` que aceita duplicatas, um `contains` que devolve `false` para um item presente. Num `record`, nada disso é escrito, então nada disso pode ser esquecido.

```java
public record Cpf(String digitos) {}

var x = new Cpf("11111111111");
var y = new Cpf("11111111111");

x.equals(y);             // true
Set.of(x).contains(y);   // true — funciona como chave e em coleções
```

Com o `equals` estrutural pronto, o `record` se comporta como um valor de verdade: serve de chave de `Map`, é deduplicado por `Set`, e responde corretamente a `List.contains`. Você pode sobrescrever o `equals` gerado, mas isso é raro e arriscado — ao fazê-lo, você reassume a obrigação de manter o contrato com `Object` e a sincronia com o `hashCode`, que era justamente o que o `record` resolvia.

A alternativa é o `equals` manual (da IDE ou do Lombok) numa classe comum, necessária quando o tipo tem identidade e deve comparar só pelo `id`, ou quando não pode ser `record`. Há duas armadilhas a conhecer. Componentes do tipo array são comparados por referência, não por conteúdo: um `record` com um `int[]` entre os componentes não terá a igualdade que se espera — prefira uma `List` ou um tipo imutável. E, se algum componente for mutável e for alterado depois que o objeto entrou num `HashSet`, a igualdade e o hash mudam sob os pés da estrutura e o objeto se perde; é mais uma razão para manter os componentes imutáveis.

### `hashCode`

Junto com o `equals`, o compilador gera um `hashCode` que calcula um inteiro a partir de todos os componentes do `record`. O ponto central é que os dois métodos nascem sincronizados: usam o mesmo conjunto de componentes, então dois records considerados iguais por `equals` sempre produzem o mesmo `hashCode` — o contrato que `Object` exige e do qual estruturas baseadas em hash, como `HashMap` e `HashSet`, dependem para funcionar.

Essa sincronia é o que a escrita manual erra com mais frequência: sobrescrever `equals` e esquecer o `hashCode`, ou usar em cada um um conjunto de campos ligeiramente diferente. O sintoma foi descrito na seção anterior — o objeto guardado num `HashMap` não é reencontrado, porque a busca cai em outro balde. Derivando os dois métodos da mesma lista de componentes, o `record` fecha essa porta por construção.

```java
public record Coordenada(int linha, int coluna) {}

var mapa = new HashMap<Coordenada, String>();
mapa.put(new Coordenada(2, 3), "torre");
mapa.get(new Coordenada(2, 3)); // "torre" — mesma chave lógica, mesmo hash
```

O algoritmo exato do `hashCode` gerado não é especificado e pode variar entre versões do Java; o que a linguagem garante é a coerência com o `equals` daquele mesmo `record`. Por isso não se deve persistir nem trafegar um valor de `hashCode` esperando que ele seja estável entre execuções ou versões — hash serve para indexação em memória, não para identidade duradoura.

Sobrescrever o `hashCode` obriga a sobrescrever o `equals` no mesmo gesto e a manter os dois de acordo por conta própria — o oposto do que o `record` oferece. As mesmas ressalvas do `equals` valem aqui: componentes do tipo array usam o hash de identidade, e componentes mutáveis alterados após a inserção numa estrutura de hash tornam o objeto irrecuperável nela. A alternativa manual — `Objects.hash(campo1, campo2)` numa classe comum — continua sendo o caminho para tipos com identidade, que costumam calcular o hash só sobre o `id`.

### `toString`

O terceiro método gerado é o `toString`, que devolve uma representação textual do `record` no formato nome do tipo seguido dos componentes entre colchetes, cada um como `nome=valor`: `Dinheiro[centavos=1890, moeda=BRL]`. Ele lista todos os componentes, na ordem declarada, chamando `toString` em cada um.

O valor disso fica claro quando se lembra do comportamento herdado de `Object`, que produz algo como `Dinheiro@7a81197d` — o nome da classe e o hash em hexadecimal, inútil num log ou numa sessão de depuração. Escrever um `toString` legível à mão é simples, mas é mais um método que precisa ser lembrado quando um campo novo entra na classe; quem esquece acaba com logs que mentem por omissão, mostrando só parte do objeto. No `record`, o `toString` acompanha a lista de componentes automaticamente.

```java
record Pedido(String cliente, int itens, long total) {}

System.out.println(new Pedido("Ana", 3, 4990));
// Pedido[cliente=Ana, itens=3, total=4990]
```

Essa saída pronta é ótima para logs, mensagens de erro e testes — ao comparar um valor esperado com um obtido, o framework de teste imprime os dois `toString` e a diferença salta aos olhos.

Há um caso em que o `toString` automático é um risco: dados sensíveis. Um `record` que carrega senha, token ou número de cartão vai despejar esses valores em qualquer log que imprima o objeto. Nesses tipos, sobrescreva o `toString` para omitir ou mascarar os campos críticos.

```java
record Credencial(String usuario, String senha) {
    @Override public String toString() {
        return "Credencial[usuario=" + usuario + ", senha=***]";
    }
}
```

As alternativas — `toString` manual, o do Lombok, construtores de string dedicados — fazem sentido quando você quer um formato específico (JSON, uma linha de CSV) ou quando o tipo não é um `record`. Para inspeção e diagnóstico do dia a dia, o formato padrão do `record` já basta.

---

# Módulo 4 — Enums, wrappers e tipos utilitários

Tipos primitivos como `int` e `boolean` têm análogos de referência (wrapper classes como `Integer` e `Boolean`) que permitem armazená-los em coleções e passar `null`. Enums são tipos que definem um conjunto fechado de constantes nomeadas, cada uma podendo carregar estado e comportamento. Este módulo explora esses mecanismos e como usá-los para código mais expressivo e seguro.

## Wrapper classes

Cada tipo primitivo de Java tem uma classe de referência correspondente que o "embrulha": `int` tem `Integer`, `long` tem `Long`, `double` tem `Double`, `boolean` tem `Boolean`, e assim por diante para `byte`, `short`, `float` e `char`. Essas classes vivem no pacote `java.lang`, são todas imutáveis — no espírito do que o Módulo 3 tratou sobre objetos imutáveis — e existem por dois motivos práticos: um valor primitivo puro não pode entrar em uma coleção genérica como `List` ou `Map`, não pode ser `null`, e não carrega métodos. Ao transformar o número ou o booleano em um objeto, o wrapper resolve essas três limitações de uma vez.

Este capítulo percorre as quatro wrapper classes mais usadas no dia a dia. Cada seção mostra o que a classe oferece de específico — constantes de limite, métodos de conversão de texto, cuidados na comparação — e quando é melhor continuar usando o primitivo em vez do wrapper. O mecanismo de conversão automática entre um e outro (autoboxing e unboxing) tem capítulo próprio logo em seguida; aqui o foco é conhecer as classes em si.

### `Integer`

`Integer` é a wrapper class do tipo primitivo `int`. É uma classe do pacote `java.lang`, imutável, cujo objeto guarda internamente um único valor `int` de 32 bits. Ela pertence à mesma família de `Long`, `Double`, `Byte`, `Short` e `Float`, todas subclasses de `Number`, e é de longe a wrapper mais presente no código Java comum, porque `int` é o tipo numérico padrão da linguagem.

O problema que ela resolve aparece assim que você precisa tratar um número inteiro como objeto. Os genéricos de Java não aceitam primitivos: não existe `List<int>` nem `Map<String, int>`. Um `int` também não tem como valer "ausente" — ele sempre é algum número, nunca `null` —, então não há como distinguir "quantidade igual a zero" de "quantidade ainda não informada". E como `int` não é objeto, você não pode chamar métodos a partir dele para, por exemplo, converter o texto `"42"` que veio de um arquivo de configuração em número.

`Integer` cobre essas lacunas. Como é uma classe, serve de argumento de tipo genérico (`List<Integer>`, `Map<String, Integer>`), pode ser `null` para representar ausência, e reúne métodos e constantes úteis: `Integer.parseInt("42")` transforma texto em `int`; `Integer.MAX_VALUE` e `Integer.MIN_VALUE` informam os limites do tipo (2 147 483 647 e -2 147 483 648); `Integer.compare(a, b)` compara dois valores sem risco de overflow; `Integer.toBinaryString(n)` e `Integer.toHexString(n)` produzem representações em outras bases.

Um uso concreto é contar ocorrências com um mapa:

```java
Map<String, Integer> contagem = new HashMap<>();
for (String palavra : texto.split(" ")) {
    contagem.merge(palavra, 1, Integer::sum);
}
int repeticoesDeJava = contagem.getOrDefault("java", 0);
```

Aqui `Integer` é obrigatório: o `HashMap` não guardaria `int`. Um ponto de atenção importante nasce do cache interno da classe: `Integer` mantém em memória instâncias já prontas para os valores de -128 a 127. Por isso `Integer a = 100; Integer b = 100; a == b` dá `true`, mas com `200` o mesmo código dá `false`, porque aí são dois objetos distintos. A comparação de valor entre wrappers deve sempre usar `.equals()` ou desempacotar para `int` — nunca `==`.

A alternativa natural é o próprio `int`: sempre que o número existe de fato, não vai para uma coleção e não precisa ser nulo, o primitivo é mais rápido e ocupa menos memória. Para processar grandes volumes de inteiros, `IntStream` evita criar milhões de objetos `Integer`. Quando o valor pode passar de pouco mais de dois bilhões, `long`/`Long` é o tipo certo, e para números realmente gigantes existe `BigInteger`. Evite `Integer` em laços numéricos intensos e em arrays grandes: `int[] numeros` é muito mais eficiente que `Integer[] numeros`, que guarda referências para objetos espalhados na memória e ainda paga o custo de empacotar e desempacotar a cada operação.

### `Long`

`Long` é a wrapper class do primitivo `long`, o tipo inteiro de 64 bits de Java. Assim como `Integer`, é uma classe imutável de `java.lang`, subclasse de `Number`, e seu objeto carrega um único valor `long`. A diferença essencial em relação a `Integer` é a faixa de valores: enquanto `int` vai até pouco mais de 2 bilhões, `long` alcança cerca de 9,2 quintilhões (`Long.MAX_VALUE` é 9 223 372 036 854 775 807).

O problema que motiva `Long` surge quando um inteiro de 32 bits não é suficiente. Marcas de tempo em milissegundos desde 1970, identificadores numéricos de banco de dados, contadores de eventos de sistemas de alto tráfego, tamanhos de arquivo em bytes — todos esses valores estouram o limite de `int` com facilidade, e um overflow silencioso faz o número "dar a volta" e virar negativo sem aviso. Além disso, `long` esbarra nas mesmas limitações que `Integer` já detalhou — fora de coleções genéricas, sem `null` —, então representar "ID ainda não atribuído" exige a forma de objeto.

`Long` resolve os dois lados: dá a capacidade de 64 bits em forma de objeto, aceitando `null` e servindo como `List<Long>` ou `Map<Long, Cliente>`. Os utilitários são os mesmos de `Integer`, trocando o prefixo — `Long.parseLong`, `Long.MAX_VALUE`/`Long.MIN_VALUE`, `Long.compare`. Um detalhe só de `long`: literais precisam do sufixo `L` — `1_000_000_000_000L` —, senão o compilador os lê como `int` e acusa erro.

Um exemplo típico é lidar com o instante atual:

```java
long agora = System.currentTimeMillis();       // retorna long
Map<Long, String> nomePorId = new HashMap<>();
nomePorId.put(900719925474099L, "Ana");        // ID que não caberia em int

Long idRecebido = requisicao.getIdOuNull();     // pode ser null
if (idRecebido != null && idRecebido.equals(900719925474099L)) {
    // ...
}
```

O `HashMap` força o uso de `Long`; `System.currentTimeMillis()` devolve `long` justamente porque o número de milissegundos já passou da casa dos trilhões. `Long` tem o mesmo cache de -128 a 127 e a mesma armadilha do `==`: para comparar valor, use `.equals()` ou desempacote para `long`.

Quando o valor existe de fato e não vai para uma coleção, fique no primitivo `long`. Quando os números são pequenos e nunca vão crescer, `int`/`Integer` economiza metade da memória. E se nem 64 bits bastam — fatoriais grandes, criptografia, somatórios astronômicos — `BigInteger` trabalha com inteiros de tamanho arbitrário. Não use `Long` por precaução quando `int` claramente resolve — valem as mesmas ressalvas de desempenho vistas em `Integer`: em laços intensos e arrays grandes, `long[]` supera de longe `Long[]`.

### `Double`

`Double` é a wrapper class do primitivo `double`, o tipo de ponto flutuante de 64 bits usado em Java para representar números com casas decimais. Como `Integer` e `Long`, é imutável, vive em `java.lang` e estende `Number`; cada objeto guarda um valor `double`. Aparece sempre que um valor fracionário precisa ser tratado como objeto.

Sem `Double`, um valor decimal fica preso às três restrições já conhecidas — fora de coleções genéricas, sem `null` para indicar "medição ainda não feita", sem métodos. O que é próprio do ponto flutuante são os casos especiais: `0.0 / 0.0` dá `NaN` ("não é um número") e `1.0 / 0.0` dá infinito, situações que um `double` cru não ajuda a detectar.

`Double` preenche essas lacunas. Além do uso em coleções e do `null`, converte texto com `Double.parseDouble("3.14")`; expõe `Double.isNaN(x)` e `Double.isInfinite(x)` para checar os casos especiais; oferece `Double.compare(a, b)`, que ordena corretamente até `NaN` e o `-0.0`, coisa que os operadores `<` e `>` não fazem de forma consistente; e traz constantes como `Double.MAX_VALUE`, `Double.POSITIVE_INFINITY` e `Double.NaN`.

Um uso comum é acumular leituras que podem faltar:

```java
List<Double> temperaturas = new ArrayList<>();
temperaturas.add(21.5);
temperaturas.add(null);          // sensor sem leitura neste horário

double soma = 0;
int validas = 0;
for (Double t : temperaturas) {
    if (t != null && !t.isNaN()) {
        soma += t;
        validas++;
    }
}
double media = validas > 0 ? soma / validas : Double.NaN;
```

Dois cuidados são decisivos. Primeiro, `Double` não tem cache de instâncias como `Integer` e `Long`: cada valor empacotado é um objeto novo, então `==` entre dois `Double` quase sempre dá `false` mesmo com valores iguais — compare sempre com `.equals()` ou desempacotando. Segundo, ponto flutuante é impreciso por natureza: `0.1 + 0.2` não resulta exatamente em `0.3`. Por isso `Double` (e `double`) nunca deve ser usado para dinheiro.

O primitivo `double` continua sendo a escolha simples quando o valor existe e dispensa coleção e `null`. Para cálculos monetários e qualquer situação que exija exatidão decimal, use `BigDecimal`. Quando memória é crítica e a precisão de 32 bits basta, existe `float`/`Float`. E, como nos outros wrappers numéricos, evite `Double` em laços pesados e grandes arrays de cálculo: `double[]` é muito mais eficiente.

### `Boolean`

`Boolean` é a wrapper class do primitivo `boolean`. É uma classe imutável de `java.lang` cujo objeto representa um de dois valores lógicos, `true` ou `false` — ou, na forma de objeto, também `null`. Diferente das wrappers numéricas, ela não estende `Number`; seu papel é dar forma de objeto ao tipo lógico da linguagem.

As três limitações do primitivo valem aqui como nos demais wrappers, mas uma delas pesa mais. Ficar fora de `Map<String, boolean>` e não ter um método para interpretar o texto `"true"` de um arquivo de propriedades são inconvenientes conhecidos; o que muda de figura é o `null`. Para um número, `null` significa apenas "ausente"; para um valor lógico, é um terceiro estado — "o usuário ainda não respondeu se aceita receber e-mails" —, diferente tanto de `true` quanto de `false`.

`Boolean` cobre os três pontos. Pode ser usado como `List<Boolean>` ou `Map<String, Boolean>`; aceita `null` para o estado indefinido; e traz `Boolean.parseBoolean("true")`, que devolve `true` apenas para a palavra `true` (ignorando maiúsculas) e `false` para qualquer outra coisa. Tem ainda as constantes `Boolean.TRUE` e `Boolean.FALSE`, os únicos dois objetos `Boolean` que a JVM realmente cria — o "cache" aqui é total, já que só há dois valores possíveis.

Um exemplo é uma tabela de funcionalidades ligadas ou desligadas, com a possibilidade de uma ainda não ter sido configurada:

```java
Map<String, Boolean> flags = new HashMap<>();
flags.put("modoEscuro", true);
flags.put("betaRelatorios", false);

Boolean beta = flags.get("betaRelatorios");   // pode devolver null
if (Boolean.TRUE.equals(beta)) {
    habilitarRelatoriosBeta();
}
```

O uso de `Boolean.TRUE.equals(beta)` em vez de `if (beta)` não é preciosismo: se `beta` for `null`, escrever `if (beta)` faz Java tentar desempacotar `null` para `boolean` e lança `NullPointerException`. Essa é a armadilha clássica de `Boolean` — um `Boolean` nulo em um `if`, um `while` ou uma expressão `&&` quebra em tempo de execução.

A alternativa mais óbvia é o primitivo `boolean`, que deve ser a escolha padrão sempre que o valor é sempre conhecido: é mais rápido, ocupa menos e nunca provoca `NullPointerException` ao ser testado. Quando o "terceiro estado" é importante para o domínio, muitas vezes um `enum` com constantes como `SIM`, `NAO` e `NAO_RESPONDIDO` comunica a intenção melhor do que um `Boolean` que pode ser `null`, porque força quem lê o código a tratar o caso indefinido. Evite `Boolean` nullable espalhado pela lógica de negócio: cada ponto onde ele é lido precisa de uma checagem de `null`, e esquecer uma delas é um defeito silencioso à espera de acontecer.

## Boxing

O capítulo anterior apresentou as wrapper classes como objetos que "embrulham" um primitivo. Este capítulo trata do movimento entre os dois mundos: como Java converte um `int` em `Integer` e de volta, e por que essa conversão, quase sempre invisível, esconde três surpresas que derrubam programas em produção.

Os quatro conceitos a seguir formam uma sequência. Primeiro o empacotamento automático (autoboxing) e o desempacotamento automático (unboxing), que são as duas metades do mecanismo. Depois o cache de instâncias, uma otimização que reaproveita objetos wrapper para valores pequenos e, com isso, torna o operador `==` traiçoeiro. Por fim a nullabilidade, o fato de que um wrapper pode ser `null` e de que desempacotar esse `null` lança exceção.

### Autoboxing

Autoboxing é a conversão automática que o compilador Java faz de um valor primitivo para um objeto da wrapper class correspondente, sem que você escreva a chamada de conversão. Onde o seu código coloca um `int` num lugar que espera um `Integer` — dentro de uma coleção, numa variável do tipo wrapper, num argumento declarado como `Object`, num tipo genérico —, o compilador insere por baixo dos panos uma chamada a `Integer.valueOf(...)`. O recurso existe desde o Java 5 e é a primeira metade do que se chama "boxing": pôr o primitivo dentro de uma "caixa" que é o objeto.

Antes do Java 5 esse embrulho era manual. Para guardar o número 42 numa lista era preciso escrever `lista.add(new Integer(42))`, e cada número que entrava numa `List` ou virava chave de um `Map` exigia essa cerimônia. O código ficava poluído de `new Integer`, `new Double`, `new Boolean`, e era fácil esquecer uma conversão e receber um erro de compilação difícil de ler. Como os genéricos de Java não aceitam primitivos — não existe `List<int>` —, não havia como escapar do trabalho repetitivo.

O autoboxing elimina essa cerimônia. Você escreve o primitivo e o compilador gera a conversão no ponto exato em que ela é necessária:

```java
List<Integer> numeros = new ArrayList<>();
numeros.add(42);                       // compilador gera numeros.add(Integer.valueOf(42))

Integer total = 0;                     // Integer.valueOf(0)

Map<String, Integer> idadePorNome = new HashMap<>();
idadePorNome.put("Ana", 30);           // o 30 é empacotado em Integer
```

Uma analogia ajuda: mandar algo pelo correio. O conteúdo — o número — é o mesmo, mas o sistema de transporte (as coleções, os genéricos) só manuseia caixas padronizadas com etiqueta. O autoboxing é o funcionário que embala automaticamente cada item antes de despachar. Só que, como no correio, cada embalagem tem um custo: toda vez que um primitivo é empacotado, ou a JVM reaproveita um objeto do cache (assunto de duas seções adiante) ou cria um objeto novo na memória.

Esse custo é o motivo para não deixar o autoboxing acontecer em toda parte. A alternativa é ficar nos primitivos sempre que o valor não precisa ser objeto: um `int` de contador, um total que nunca vai para uma coleção, um cálculo intermediário. Para processar muitos números há `IntStream` e `int[]`, que não empacotam nada. E quando o desempenho é crítico existem bibliotecas de coleções de primitivos, como Eclipse Collections, com tipos do gênero `IntList`.

O ponto de atenção maior é o laço quente. Um trecho como `Integer soma = 0; for (int i = 0; i < 1_000_000; i++) soma += i;` parece inocente, mas cada `soma += i` desempacota `soma`, faz a conta com `int` e empacota o resultado num `Integer` novo — um milhão de objetos descartáveis, pressão desnecessária no coletor de lixo. Em código sensível a performance, mantenha o acumulador como primitivo (`int soma = 0`) e só empacote no final, se precisar. E nunca conte com o autoboxing para preservar identidade: dois valores iguais empacotados separadamente podem ou não ser o mesmo objeto, como a próxima discussão sobre cache deixa claro.

### Unboxing

Unboxing é o caminho inverso do autoboxing: a conversão automática de um objeto wrapper de volta para o primitivo que ele carrega. Quando o seu código usa um `Integer` num lugar que espera um `int` — numa conta aritmética, numa comparação com outro número, na condição de um `if` ou `while`, num `switch`, como índice de array, ou na atribuição a uma variável primitiva —, o compilador insere a chamada correspondente: `.intValue()` para `Integer`, `.doubleValue()` para `Double`, `.booleanValue()` para `Boolean`, e assim por diante.

Sem esse recurso, extrair o primitivo era explícito e repetitivo. Antes do Java 5 escrevia-se `int q = quantidade.intValue();` a cada uso, e somar dois `Integer` obrigava a desempacotar os dois operandos na mão. O autoboxing sozinho resolveria só metade do problema — colocar valores em coleções —, mas não o uso desses valores depois de recuperados.

Com unboxing, o compilador cuida da extração no ponto de uso:

```java
Map<String, Integer> estoque = new HashMap<>();
estoque.put("caneta", 12);

int quantidade = estoque.get("caneta");   // unboxing: .intValue()
if (quantidade > 10) {
    System.out.println("estoque suficiente");
}
```

Se autoboxing é fechar a caixa para despachar, unboxing é abri-la para usar o conteúdo. O compilador abre automaticamente sempre que precisa do número em si — para somar, comparar, indexar. Mas essa abertura automática carrega um risco que o embrulho não tem: se a caixa estiver vazia, ou seja, se o wrapper for `null`, não há primitivo para colocar no lugar e o programa lança `NullPointerException` — é justamente o tema da última seção deste capítulo.

Há uma sutileza que confunde muita gente: nem toda operação com wrappers faz unboxing. O operador `==` entre dois wrappers **não** desempacota — ele compara referências, isto é, pergunta se são o mesmo objeto. Já os operadores `<`, `>`, `<=` e `>=` desempacotam os dois lados e comparam como primitivos. E `==` entre um wrapper e um primitivo desempacota o wrapper.

```java
Integer x = 1000;
Integer y = 1000;

System.out.println(x == y);        // false — compara referências, são dois objetos
System.out.println(x.equals(y));   // true  — compara valor
System.out.println(x <= y);        // true  — unboxing dos dois lados, compara int
System.out.println(x == 1000);     // true  — o lado primitivo força unboxing de x
```

Um caso concreto onde o unboxing pesa é somar valores tirados de uma coleção. O trecho abaixo desempacota um `Integer` a cada volta do laço só para fazer a conta:

```java
List<Integer> vendas = List.of(120, 90, 300, 45);

int totalDia = 0;
for (Integer v : vendas) {
    totalDia += v;                 // unboxing de v a cada iteração
}
```

Com poucas dezenas de elementos isso é irrelevante; com milhões, cada `+= v` paga o custo de `.intValue()`. Nesses volumes, `vendas.stream().mapToInt(Integer::intValue).sum()` deixa claro o desempacotamento e trabalha com `int` puro no somatório. Outro ponto de atenção é misturar tipos: comparar um `Integer` com um `Long` com `==` não compila, e mesmo depois de desempacotados um `int` e um `long` só se comparam após promoção numérica — cuidado ao guardar IDs ora como `Integer`, ora como `Long`.

A alternativa ao unboxing automático é chamar `.intValue()` (ou equivalente) você mesmo, o que raramente compensa, ou manter o valor como `Optional` e extrair com `orElse`. Onde o unboxing deve ser evitado é, de novo, nos laços quentes — desempacotar milhões de vezes tem custo — e em qualquer ponto onde o wrapper possa ser `null`. Nesses casos, ou se garante o `null` antes com uma checagem, ou se trabalha diretamente com primitivos desde o início.

### Cache

Para não criar um objeto novo toda vez que um valor pequeno e comum é empacotado, as wrapper classes mantêm um conjunto de instâncias prontas em memória. O método `Integer.valueOf(int)` — o mesmo que o autoboxing chama — devolve sempre a **mesma** instância compartilhada para valores entre −128 e 127; fora dessa faixa, cria um objeto novo a cada chamada. Esse conjunto pré-fabricado é o "cache" de wrappers.

O motivo é estatístico. Empacotar inteiros é uma das operações mais frequentes num programa Java real: contadores de laço, chaves de mapa, códigos de status, quantidades pequenas. Se cada `lista.add(1)` alocasse um objeto, o resultado seria uma enxurrada de objetos minúsculos e efêmeros, todos dando trabalho ao coletor de lixo. Como os inteiros pequenos dominam o uso real, guardar os mais comuns já prontos rende uma economia grande com um custo de memória fixo e desprezível.

O mecanismo é simples: `Integer.valueOf` verifica se o valor está na faixa do cache e, se estiver, retorna o elemento correspondente de um array interno; senão, faz `new Integer(v)`. `Long`, `Short` e `Byte` seguem a mesma faixa de −128 a 127 (no caso de `Byte`, isso cobre todos os valores possíveis); `Character` cacheia de 0 a 127; `Boolean` cacheia seus dois únicos valores. `Float` e `Double` **não têm cache** — todo valor de ponto flutuante empacotado é um objeto novo. O limite superior do cache de `Integer` pode ser aumentado pela opção de JVM `-XX:AutoBoxCacheMax=<n>`, mas mexer nisso é raro e não deve virar hábito.

A consequência prática é a armadilha mais conhecida dos wrappers:

```java
Integer a = 127;
Integer b = 127;
System.out.println(a == b);        // true  — mesma instância vinda do cache

Integer c = 128;
Integer d = 128;
System.out.println(c == d);        // false — 128 está fora do cache, são dois objetos

System.out.println(c.equals(d));   // true  — comparação de valor, a forma correta
```

O cenário real onde isso morde: um código compara dois identificadores lidos de mapas ou de um banco usando `==`. Nos testes, com IDs pequenos como 1, 2, 3, tudo passa, porque esses valores vêm do cache e `==` acerta por acidente. Em produção, com IDs reais acima de 127, o `==` passa a dar `false` para valores iguais e o sistema quebra de forma intermitente e difícil de reproduzir.

Pense numa papelaria que mantém na prateleira, prontas, as canetas dos modelos mais vendidos — digamos, os de número −128 a 127. Quando você pede um desses, recebe exatamente a mesma unidade que qualquer outro cliente receberia: é literalmente o mesmo objeto físico. Se pede um modelo fora dessa lista, a loja fabrica um sob encomenda a cada pedido, e o seu nunca é "o mesmo" que o de outra pessoa. Por isso o operador `==`, que pergunta "é a mesmíssima caneta?", responde de um jeito para 127 e de outro para 128 — e, exatamente por depender do cache, nunca serve para comparar o valor guardado.

A alternativa é única e sem exceção: para comparar o conteúdo de dois wrappers, use `.equals()`, ou `Objects.equals(a, b)` quando algum lado pode ser `null`, ou desempacote os dois para primitivo e compare com `==`. Muitos analisadores estáticos e configurações de lint sinalizam `==` entre tipos wrapper justamente por causa dessa pegadinha. E, ao contrário: não escreva código cuja correção dependa de o objeto vir do cache, não ajuste o tamanho do cache como solução de rotina, e não suponha que `Double` ou `Float` reaproveitem instâncias — eles nunca reaproveitam.

### Nullabilidade

Nullabilidade, no contexto de boxing, é o fato de que um wrapper, por ser uma referência a objeto, pode valer `null` — e de que desempacotar esse `null` lança `NullPointerException` em tempo de execução. É a terceira surpresa do mecanismo: o autoboxing e o unboxing são silenciosos, o cache torna o `==` traiçoeiro, e a nullabilidade transforma um valor ausente num erro que estoura longe de onde nasceu.

Um primitivo nunca tem esse problema porque sempre contém algum valor: um `int` é sempre um número, um `boolean` é sempre `true` ou `false`. O wrapper acrescenta um estado a mais, o `null`, e ele aparece com facilidade: `map.get(chave)` devolve `null` quando a chave não existe; uma coluna de banco marcada como `NULL` vira um `Integer` nulo ao ser lida por um ORM; um campo de objeto do tipo wrapper que ninguém inicializou vale `null`, não `0`; uma propriedade ausente num JSON desserializa para `null`. A partir daí, qualquer conta, comparação ou `if` sobre esse valor dispara o unboxing — e o unboxing de `null` quebra.

```java
Map<String, Integer> pontos = new HashMap<>();
pontos.put("ana", 10);

int p = pontos.get("bia");                 // "bia" não existe → get devolve null
                                           // → unboxing de null → NullPointerException

int q = pontos.getOrDefault("bia", 0);     // seguro: devolve 0 quando falta a chave

Integer bonus = usuario.getBonus();        // pode ser null
if (bonus != null && bonus > 100) {        // checa null ANTES de comparar
    aplicarBonus();
}

int total = bonus == null ? 0 : bonus;     // converte o ausente num valor concreto
```

Há uma variação especialmente sorrateira no operador ternário. Em `Integer resultado = condicao ? 1 : podeSerNull;`, um ramo é o primitivo `1` e o outro é um `Integer`. Nessa situação o compilador decide unificar os dois ramos como `int` e desempacota os dois — então, quando `podeSerNull` é `null` e a condição escolhe esse ramo, o código lança `NullPointerException` mesmo parecendo que você só fez uma atribuição de wrapper para wrapper.

Pense no wrapper como uma caixa que pode estar cheia, com um número dentro, ou vazia, representando `null`. O primitivo é um número escrito direto na mesa: sempre está lá. Quando o compilador precisa do número para uma conta e encontra a caixa vazia, não tem o que pôr na mesa, e o programa para na hora. Por isso todo ponto do código que recebe um wrapper de uma fonte externa — um `Map`, um banco, uma requisição, um arquivo — deveria verificar `null` antes de deixar o valor cair numa expressão aritmética ou numa condição.

As formas de lidar com isso são conhecidas: `getOrDefault` e `Objects.requireNonNullElse` para substituir o ausente por um padrão; uma checagem `if (x != null)` explícita antes do uso; `Optional<Integer>` ou `OptionalInt` quando a ausência é parte legítima do contrato e você quer forçar quem chama a tratá-la; e anotações `@Nullable`/`@NonNull` combinadas com análise estática, que apontam o risco antes de rodar. Quando o "valor ausente" é um estado real do domínio — como "ainda não respondido" —, às vezes um `enum` de três constantes comunica melhor do que um wrapper que pode ser `null`.

O melhor remédio, porém, é não introduzir a nullabilidade onde ela não faz falta. Em campos e variáveis cujo valor é sempre conhecido, use o primitivo: isso elimina a classe inteira de bug de uma vez. Reserve o wrapper para os casos em que o valor de fato precisa entrar numa coleção, ser genérico, ou representar ausência — e, nesses casos, trate o `null` em cada ponto de leitura: como já observado a propósito de `Boolean`, basta uma checagem esquecida para o programa quebrar em produção, longe de onde o valor ausente surgiu.

## Enums

Os capítulos anteriores trataram de dar forma de objeto a valores primitivos. Este capítulo muda o foco para outro problema de modelagem: representar um conjunto de opções fixo e conhecido de antemão — os dias da semana, os naipes de um baralho, os estados possíveis de um pedido. Java tem um tipo próprio para isso, o `enum`, declarado com uma palavra-chave dedicada, que transforma essas opções em constantes nomeadas com verificação feita pelo compilador.

As quatro seções a seguir cobrem o essencial. Primeiro a declaração, a sintaxe que cria o tipo. Depois as constantes, os valores nomeados que ele contém e como compará-los e identificá-los. Em seguida `values`, o método que devolve todos os valores para iteração. Por fim `valueOf`, o método que faz o caminho inverso: do texto de um nome para a constante correspondente.

### Declaração

Um `enum` é um tipo especial de Java, declarado com a palavra-chave `enum`, que define um conjunto fechado e fixo de valores constantes nomeados. "Fechado" significa que todos os valores possíveis são escritos no próprio código, no momento da declaração, e nenhum outro pode existir em tempo de execução. O recurso entrou na linguagem no Java 5 e ocupa o mesmo lugar que uma classe: fica em seu próprio arquivo `.java`, ou aninhado dentro de outra classe, ou ao lado de uma. Por baixo, o compilador gera uma classe que estende `java.lang.Enum`, mas você não escreve essa herança — ela é automática, e não é possível estender um `enum` nem instanciá-lo com `new`.

Antes dos enums, um conjunto fixo de opções era representado por constantes inteiras: `public static final int SEGUNDA = 0;`, `TERCA = 1`, e assim por diante — o chamado "padrão int enum". Esse arranjo tem falhas sérias. Não há segurança de tipo: um método que espera "um dia da semana" declara o parâmetro como `int` e aceita `-7`, `42` ou o mês de nascimento de alguém sem reclamar. Não há espaço de nomes: se outro grupo de constantes também começa em `0`, os dois se confundem numa comparação. E a depuração é pobre: imprimir a constante mostra `0`, não `SEGUNDA`, porque no fim é só um número. Constantes de `String` resolvem a legibilidade, mas trazem erros de digitação silenciosos e comparação mais cara.

O `enum` cobre tudo isso de uma vez. A declaração lista os valores entre chaves:

```java
public enum DiaDaSemana {
    SEGUNDA, TERCA, QUARTA, QUINTA, SEXTA, SABADO, DOMINGO
}
```

A partir daí, `DiaDaSemana` é um tipo como qualquer outro. Uma variável desse tipo só pode receber uma das sete constantes ou `null`; qualquer outra coisa é erro de compilação. Um método que recebe `DiaDaSemana dia` tem a garantia, dada pelo compilador, de que o valor é um dia válido — a categoria inteira de bug "número fora da faixa" desaparece. Além disso, o tipo se integra a construções feitas para ele: pode ser usado direto num `switch`, e existem `EnumSet` e `EnumMap`, coleções otimizadas para usar constantes de enum como elementos e chaves.

```java
DiaDaSemana hoje = DiaDaSemana.QUARTA;

boolean fimDeSemana = switch (hoje) {
    case SABADO, DOMINGO -> true;
    default -> false;
};
```

A alternativa continua sendo constantes `int` ou `String` — aceitável apenas em código muito antigo ou na interoperabilidade com sistemas que só falam números. Para um conjunto que não é conhecido em tempo de compilação, o `enum` não serve: categorias de produto cadastradas por um administrador, papéis de usuário lidos de uma tabela, unidades de medida que um cliente pode adicionar. Nesses casos o conjunto varia em tempo de execução, e o certo é uma classe comum com instâncias carregadas de onde os dados vivem. Use `enum` quando a lista de valores é estável, pequena o suficiente para caber no código, e faz parte das regras do programa — não dos dados que ele processa.

### Constantes

As constantes de um `enum` são os valores nomeados escritos no corpo da declaração — `SEGUNDA`, `TERCA`, `COPAS`, `PENDENTE`. Cada uma é, ao mesmo tempo, um campo `public static final` do tipo do enum e uma instância única desse tipo, criada uma só vez pela JVM quando o enum é carregado. Ou seja: existe exatamente um objeto `DiaDaSemana.QUARTA` em toda a execução do programa, e toda referência a `QUARTA` aponta para esse mesmo objeto. Por convenção, os nomes vão em maiúsculas com palavras separadas por sublinhado (`EM_ANDAMENTO`), como qualquer constante em Java. A lista é separada por vírgulas; um ponto e vírgula ao final só é obrigatório quando há mais membros no corpo do enum depois dela.

Como a seção anterior mostrou, o antigo padrão de constantes `int` não dava segurança de tipo nem espaço de nomes. Falta o ponto que mais pesa aqui: naquele padrão, `SEGUNDA` era apenas um rótulo colado no número `0`, sem identidade própria — `0` era qualquer coisa, e nada distinguia uma constante da outra além do valor que alguém escolheu.

Nos enums, cada constante é um objeto distinto e irrepetível, e isso muda a forma de compará-las. Como só existe uma instância de cada, a comparação correta e preferida é com `==`:

```java
if (pedido.getStatus() == Status.PENDENTE) {
    enviarLembrete();
}
```

`==` entre enums é seguro — nunca lança `NullPointerException`, ao contrário de `.equals` chamado sobre um valor possivelmente nulo —, é mais rápido, e o compilador ainda verifica se os dois lados são do mesmo tipo de enum: comparar `Status.PENDENTE == Naipe.COPAS` nem compila. Cada constante também traz dois métodos herdados: `name()` devolve o texto exato do identificador (`"PENDENTE"`), e `ordinal()` devolve a posição na declaração, começando em zero. Dentro de um `switch`, as constantes são escritas sem o nome do tipo (`case PENDENTE ->`).

Um cuidado central é não depender de `ordinal()` para nada que seja gravado em algum lugar. Se você persiste o `ordinal()` num banco e mais tarde alguém insere uma constante nova no meio da lista ou reordena os valores, todos os números já gravados passam a apontar para a constante errada, sem nenhum erro visível. Para persistir, grave `name()`, que só muda se alguém renomear a constante — uma alteração bem mais evidente e rara. O `ordinal()` existe para uso interno de estruturas como `EnumSet` e `EnumMap`, não para servir de chave em armazenamento externo.

A alternativa continua sendo a mesma da seção anterior — literais de `String` ou constantes `int` —, agora com o agravante de não terem a identidade garantida que cada constante de enum traz. Quando o conjunto de valores é grande demais para escrever à mão, ou quando cada valor carrega muitos dados próprios que vêm de fora, o modelo certo passa a ser dados numa tabela, não constantes no código. E quando cada constante precisa de estado ou comportamento específico — um símbolo, um fator de cálculo, uma regra própria —, isso também é possível com enums e é justamente o tema do próximo capítulo, "Enums avançados".

### `values`

`values()` é um método estático que o compilador gera automaticamente em todo `enum`. Ele não existe em `java.lang.Enum` nem é escrito por você: é sintetizado, um para cada enum, e devolve um array com todas as constantes daquele tipo, na ordem exata em que foram declaradas. Para o `DiaDaSemana` das seções anteriores, `DiaDaSemana.values()` devolve um array de sete posições, de `SEGUNDA` a `DOMINGO`.

Sem esse método, trabalhar com "todos os valores" de um conjunto de constantes `int` exigia manter, à mão, um array ou lista paralela — `int[] TODOS = { SEGUNDA, TERCA, ... }` — e lembrar de atualizá-la toda vez que uma constante nova entrasse. Esquecer essa sincronização era um erro comum e silencioso: o novo valor existia, mas nenhum laço que percorria "todos" o enxergava.

Com `values()`, a lista completa vem sempre correta e atualizada, porque é o próprio compilador que a monta a partir da declaração. O uso mais frequente é iterar:

```java
for (DiaDaSemana dia : DiaDaSemana.values()) {
    System.out.println(dia.ordinal() + " - " + dia.name());
}

int quantosDias = DiaDaSemana.values().length;
```

Isso serve para preencher um menu de seleção, montar um relatório com uma linha por status, validar uma entrada contra todos os valores aceitos, ou aplicar uma operação a cada constante. Combinado com streams, `Arrays.stream(DiaDaSemana.values())` abre caminho para filtros e transformações.

Há um detalhe de implementação que importa: `values()` cria um **array novo a cada chamada**. Isso é proposital — arrays são mutáveis, e devolver sempre o mesmo array permitiria que qualquer trecho de código alterasse a lista de constantes vista por todo o programa. A consequência é que chamar `values()` dentro de um laço que roda milhões de vezes gera um array descartável a cada volta. Quando isso pesa, guarde o resultado uma vez:

```java
private static final DiaDaSemana[] DIAS = DiaDaSemana.values();
```

e itere sobre `DIAS`. Uma alternativa mais expressiva quando você quer o conjunto todo como coleção é `EnumSet.allOf(DiaDaSemana.class)`, que devolve um `Set` eficiente; e quando a intenção é um subconjunto fixo, `EnumSet.of(SABADO, DOMINGO)` expressa isso melhor do que iterar `values()` e filtrar. Para percorrer de trás para frente ou em ordem alfabética, ordene ou inverta uma cópia — nunca o array que `values()` devolveu, mesmo que ele seja, na prática, exclusivo daquela chamada.

### `valueOf`

`valueOf(String)` é o outro método estático que o compilador gera automaticamente em todo `enum`. Ele faz o caminho inverso de `name()`: recebe uma `String` e devolve a constante cujo nome é exatamente igual a esse texto. `DiaDaSemana.valueOf("QUARTA")` devolve `DiaDaSemana.QUARTA`. A correspondência é exata e sensível a maiúsculas — o texto precisa bater caractere por caractere com o identificador escrito na declaração.

O problema que ele resolve aparece toda vez que um valor chega ao programa como texto e precisa virar um tipo. Um parâmetro de URL `?status=PENDENTE`, um campo de um JSON, uma coluna de banco, uma linha de arquivo de configuração: tudo isso é `String` na entrada. Sem `valueOf`, transformar esse texto na constante certa era uma cadeia de `if` com `equals`, ou um `switch` sobre strings, repetido em todo ponto onde a conversão fosse necessária, e sempre com o risco de esquecer um caso.

Com `valueOf`, a conversão é uma linha:

```java
Status status = Status.valueOf(request.getParametro("status"));
```

O ponto de atenção decisivo é o comportamento quando não há correspondência: `valueOf` **lança `IllegalArgumentException`** se nenhuma constante tem aquele nome, e `NullPointerException` se o argumento for `null`. Como a entrada quase sempre vem de fora e não é confiável, chamar `valueOf` direto sobre ela deixa o programa à mercê de qualquer texto inválido. O tratamento usual é capturar a exceção e devolver um valor padrão ou um `Optional`:

```java
static Optional<Status> parseStatus(String texto) {
    if (texto == null) return Optional.empty();
    try {
        return Optional.of(Status.valueOf(texto.trim().toUpperCase()));
    } catch (IllegalArgumentException e) {
        return Optional.empty();
    }
}
```

O `toUpperCase()` acima contorna a sensibilidade a maiúsculas — `valueOf("pendente")` sozinho lançaria exceção. Quando os valores recebidos não seguem exatamente o formato das constantes — têm espaços, hifens, acentos, ou vêm de um sistema externo com nomes próprios —, a saída melhor é montar uma vez um `Map<String, Status>` de tradução e consultar esse mapa, em vez de forçar o texto no molde de `valueOf`.

As alternativas são justamente essas: um `Map` de busca construído a partir de `values()`; um laço sobre `values()` comparando `name()` ou um campo próprio da constante; bibliotecas como Apache Commons (`EnumUtils.getEnum`), que devolvem `null` em vez de lançar exceção; e, na desserialização, ferramentas como Jackson, que já convertem `String` em enum automaticamente. Onde `valueOf` não deve ser usado é como fluxo de controle para um caso que acontece o tempo todo: se "valor ausente ou desconhecido" é comum e esperado, capturar exceção a cada ocorrência é caro e obscuro — um `Map` que devolve `null`, ou um `Optional`, expressa melhor a intenção e não paga o custo de criar e lançar exceções.

## Enums avançados

O capítulo anterior tratou o `enum` como uma lista de nomes: um conjunto fechado de rótulos que o compilador verifica. Mas um `enum` é, por baixo, uma classe completa, e cada constante é um objeto dessa classe. Isso abre a porta para constantes que carregam dados próprios e sabem fazer coisas — não apenas serem comparadas. É a diferença entre `MOEDA_REAL` ser só um nome e `Moeda.REAL` saber seu código ISO, seu símbolo e como formatar um valor.

As quatro seções a seguir sobem esse degrau em ordem. Primeiro os campos, os dados que cada constante guarda. Depois os construtores, o mecanismo que inicializa esses campos quando o enum é carregado. Em seguida os métodos, o comportamento comum que opera sobre os campos. Por fim os comportamentos específicos, o recurso pelo qual cada constante fornece sua própria versão de um método, comportando-se de forma diferente das demais.

### Campos

Um campo de enum é uma variável de instância declarada no corpo do `enum`, exatamente como numa classe comum: `private final String simbolo;`. Como cada constante do enum é um objeto distinto e único, cada uma tem seu próprio valor para esse campo. Para fornecer esses valores, a lista de constantes deixa de ser só nomes soltos e passa a incluir argumentos entre parênteses, um conjunto por constante; quando o corpo do enum tem mais membros depois da lista, ela precisa terminar com ponto e vírgula. Os campos quase sempre são `final`: a constante é criada uma única vez na carga da classe e nunca mais muda, então seu estado é fixo.

Sem esse recurso, quando cada valor de um conjunto fixo tem dados associados — um código numérico para gravar no banco, um símbolo para exibir, um fator de conversão — esses dados ficam em estruturas paralelas ao enum. Um `Map<Moeda, String>` para os símbolos aqui, um `switch (moeda)` para os códigos ali, cada um num arquivo diferente, e nada garante que os dois estejam completos e sincronizados quando alguém adiciona uma moeda nova. É a mesma dispersão que o padrão de constantes `int` sofria, só que agora com dados em vez de só nomes.

Com campos, os dados moram na própria constante, num lugar só:

```java
public enum Moeda {
    REAL("BRL", "R$", 2),
    DOLAR("USD", "$", 2),
    IENE("JPY", "¥", 0);

    private final String codigoIso;
    private final String simbolo;
    private final int casasDecimais;

    // construtor e métodos nas próximas seções
}
```

A partir daí, tudo que se sabe sobre uma moeda está ao lado do nome dela. Adicionar `EURO("EUR", "€", 2)` obriga, na mesma linha, a informar os três dados — não há como esquecer metade.

A alternativa continua sendo um `Map` externo, montado uma vez a partir de `values()`, ou uma classe comum com instâncias, quando o conjunto de valores não é fixo em tempo de compilação. E há um limite importante: campo de enum é para dado estrutural e estável — o código ISO de uma moeda, quantas casas decimais ela usa. Não serve para dado que varia em tempo de execução ou por ambiente, como a cotação do dia: isso muda a cada minuto e não pode estar fixado num `final` escrito no código-fonte. Esse tipo de valor vem de fora, de um serviço ou de uma tabela, e o enum no máximo serve de chave para buscá-lo.

### Construtores

O construtor de um `enum` é o método que inicializa os campos de cada constante. Ele recebe os argumentos escritos entre parênteses depois do nome da constante e os atribui aos campos, igual ao construtor de qualquer classe. A diferença está em quem o chama e quando: você nunca escreve `new Moeda(...)`. A JVM chama o construtor uma vez para cada constante, na ordem em que elas aparecem na declaração, no momento em que a classe do enum é carregada. Depois disso, o conjunto de constantes está fechado e nenhuma outra instância pode ser criada.

Por isso o construtor de enum tem regras próprias. Ele é sempre privado na prática — declarar `public` ou `protected` é erro de compilação, e omitir o modificador é o normal. Não é possível chamar `super(...)` explicitamente, porque a superclasse `java.lang.Enum` é controlada pelo compilador. E pode haver sobrecarga: se algumas constantes precisam de menos dados que outras, você escreve dois construtores, e cada constante usa o que combina com seus argumentos.

Retomando o enum da seção anterior, o construtor amarra cada trio de literais aos campos:

```java
public enum Moeda {
    REAL("BRL", "R$", 2),
    DOLAR("USD", "$", 2),
    IENE("JPY", "¥", 0);

    private final String codigoIso;
    private final String simbolo;
    private final int casasDecimais;

    Moeda(String codigoIso, String simbolo, int casasDecimais) {
        this.codigoIso = codigoIso;
        this.simbolo = simbolo;
        this.casasDecimais = casasDecimais;
    }
}
```

Quando a classe `Moeda` é carregada, o construtor roda três vezes: `("BRL", "R$", 2)`, depois `("USD", "$", 2)`, depois `("JPY", "¥", 0)`.

Fora do construtor, a única forma de preencher os campos seria um bloco `static` que monta um `Map` de dados depois que as constantes já existem — mais verboso e sem a checagem de "informou todos os dados?" que os parênteses na declaração dão. O cuidado central é não colocar lógica pesada no construtor de enum: ele roda na inicialização da classe, e qualquer exceção lançada ali vira um `ExceptionInInitializerError`, um erro que aborta o carregamento e costuma ser difícil de rastrear até a causa. Nada de ler arquivo, abrir conexão ou fazer cálculo que possa falhar — só atribuição direta dos campos. Outra pegadinha: não acesse campos `static` do próprio enum de dentro do construtor, porque as constantes são construídas antes de o resto da inicialização estática terminar, e esses campos ainda podem estar nulos.

### Métodos

Um `enum` pode ter métodos de instância no seu corpo, escritos como em qualquer classe. Todas as constantes compartilham a mesma implementação, e o método opera sobre os campos da constante através da qual foi chamado. Há três usos típicos: expor os campos com métodos de acesso (`getSimbolo()`), derivar informação a partir deles (`formatar(valor)`, que usa `casasDecimais` e `simbolo`), e — com `static` — oferecer buscas sobre o conjunto (`porCodigoIso("USD")`, que varre `values()`).

Quando o enum não tem métodos, a lógica sobre o que cada constante significa acaba fora dele, espalhada. Todo lugar que precisa formatar um valor monetário repete o mesmo `switch (moeda)` decidindo casas decimais e posição do símbolo; se a regra muda, é preciso caçar todas as cópias. Trazer esse comportamento para dentro do enum deixa cada rotina ao lado dos campos que ela usa, e quem chama passa a escrever só `moeda.formatar(valor)`.

```java
public enum Moeda {
    // ... constantes, campos e construtor das seções anteriores

    public String getCodigoIso() {
        return codigoIso;
    }

    public String formatar(BigDecimal valor) {
        return simbolo + " " + valor.setScale(casasDecimais, RoundingMode.HALF_UP);
    }

    public static Moeda porCodigoIso(String iso) {
        for (Moeda m : values()) {
            if (m.codigoIso.equals(iso)) {
                return m;
            }
        }
        throw new IllegalArgumentException("Moeda desconhecida: " + iso);
    }
}
```

Agora `Moeda.REAL.formatar(new BigDecimal("10.5"))` devolve `"R$ 10.50"` e `Moeda.IENE.formatar(...)` devolve o valor sem casas decimais, cada constante aplicando seus próprios campos à mesma lógica.

Dá para deixar essa lógica numa classe utilitária separada, como `MoedaUtils.formatar(moeda, valor)` — funciona, mas separa o comportamento do tipo e não aproveita o acesso direto aos campos privados. Para a busca por código, um `Map<String, Moeda>` estático montado uma vez substitui o laço com vantagem quando o enum tem muitas constantes. O limite é o mesmo dos campos: métodos de enum devem conter lógica pura sobre os próprios campos. Se o método precisa de um repositório, um cliente HTTP ou qualquer dependência injetada, ele não cabe no enum — o enum é instanciado pela JVM e não participa de injeção de dependências. Regras que mudam conforme o cliente ou o ambiente também pertencem a um serviço, não ao enum.

### Comportamentos específicos

Até aqui todos os métodos do enum tinham uma implementação só, compartilhada por todas as constantes. O recurso de comportamento específico por constante — em inglês, *constant-specific method body* — permite que cada constante forneça a sua própria versão de um método. A sintaxe é um bloco entre chaves logo após o nome (e os argumentos) da constante, contendo a implementação daquele método para aquela constante. Por baixo dos panos, cada constante com corpo próprio vira uma subclasse anônima do enum, e a chamada do método despacha para a implementação correta conforme a constante — o mesmo mecanismo de polimorfismo que o Módulo 1 tratou.

O uso mais limpo declara o método como `abstract` no enum, o que obriga toda constante a implementá-lo:

```java
public enum Operacao {
    SOMA("+")           { public double aplicar(double a, double b) { return a + b; } },
    SUBTRACAO("-")      { public double aplicar(double a, double b) { return a - b; } },
    MULTIPLICACAO("*")  { public double aplicar(double a, double b) { return a * b; } },
    DIVISAO("/")        { public double aplicar(double a, double b) { return a / b; } };

    private final String simbolo;

    Operacao(String simbolo) {
        this.simbolo = simbolo;
    }

    public abstract double aplicar(double a, double b);

    @Override
    public String toString() {
        return simbolo;
    }
}
```

Com isso, `Operacao.SOMA.aplicar(6, 2)` devolve `8` e `Operacao.DIVISAO.aplicar(6, 2)` devolve `3`, e um laço sobre `values()` consegue imprimir toda a tabuada de uma expressão.

O problema que isso resolve aparece quando a alternativa é um único método com um `switch` sobre `this`: `switch (this) { case SOMA -> a + b; ... }`. Esse `switch` compila mesmo incompleto. No dia em que alguém adiciona `POTENCIA("^")` e esquece de acrescentar o caso, o código segue compilando e cai no `default` — um erro silencioso descoberto só em produção. Com o método `abstract`, esquecer a implementação de `POTENCIA` é erro de compilação: o programa não sobe sem que o caso novo seja tratado.

A alternativa moderna mais comum é guardar o comportamento como um campo do tipo função, passado no construtor: `SOMA("+", (a, b) -> a + b)`, com um campo `DoubleBinaryOperator`. É mais enxuto que abrir um bloco por constante e costuma ser preferido quando cada implementação é uma expressão curta. O corpo por constante ainda vale a pena quando a lógica de cada uma é maior que uma linha, ou quando ela precisa chamar outros membros do enum.

Quando não usar: se apenas uma ou duas constantes divergem e o resto se comporta igual, escrever um corpo para cada uma é repetição desnecessária — melhor deixar uma implementação padrão no enum e sobrescrevê-la só nas exceções. E se as constantes começam a acumular muito comportamento próprio e divergente, com vários métodos específicos cada, o sinal é que aquilo talvez não devesse ser um `enum`, e sim uma hierarquia de classes ou uma interface selada, onde cada tipo tem espaço para crescer.

## Records aprofundados

O Módulo 3 apresentou o `record` como o atalho da linguagem para o objeto imutável de dados: uma linha declara os componentes e o compilador gera construtor, acessores, `equals`, `hashCode` e `toString`. Aquela visão bastava para usar records com dados já limpos e confiáveis. Esta aula abre a tampa desse mecanismo para o caso real, em que os dados chegam precisando de normalização, de checagem, e em que o tipo não vive isolado.

Os cinco capítulos tratam de: o construtor canônico, que é o portão único por onde toda instância passa e que você pode reescrever; sua forma curta, o construtor compacto; o uso desse portão para validar invariantes; a implementação de interfaces, que insere o `record` em hierarquias e em `switch` com pattern matching; e o limite da promessa de imutabilidade, que é superficial — os campos são `final`, mas o que eles apontam pode continuar mutável.

### Construtor canônico

Todo `record` tem exatamente um construtor canônico: aquele cuja lista de parâmetros é igual à lista de componentes, na mesma ordem e com os mesmos tipos. Em `record Intervalo(LocalDate inicio, LocalDate fim)`, o construtor canônico é `Intervalo(LocalDate inicio, LocalDate fim)`. Por padrão o compilador o gera de forma invisível, apenas copiando cada parâmetro para o campo correspondente. Mas você pode escrevê-lo à mão para inserir lógica entre a chegada dos valores e a fixação do estado — e é esse o gancho que torna records úteis fora do cenário de dados perfeitos.

Sem poder tocar nesse construtor, o `record` só serviria para dados que já chegam corretos: não haveria onde rejeitar um valor inválido, aparar espaços de uma `String`, arredondar um número ou fazer cópia defensiva de uma coleção. Numa classe comum você tem liberdade total sobre construtores, mas também a responsabilidade inteira; no `record`, o construtor canônico concentra essa liberdade num ponto só, e como ele é o único caminho de criação, o que você garante ali vale para toda instância.

A forma explícita repete a lista de componentes como parâmetros, faz o trabalho necessário e termina atribuindo cada campo com `this.campo = campo`. Reatribuir a variável do parâmetro antes disso é o que permite normalizar o valor guardado.

```java
public record Intervalo(LocalDate inicio, LocalDate fim) {
    public Intervalo(LocalDate inicio, LocalDate fim) {
        if (inicio.isAfter(fim)) {          // normaliza: sempre início <= fim
            LocalDate tmp = inicio;
            inicio = fim;
            fim = tmp;
        }
        this.inicio = inicio;
        this.fim = fim;
    }
}

new Intervalo(LocalDate.of(2025, 3, 10), LocalDate.of(2025, 1, 1));
// guarda inicio=2025-01-01, fim=2025-03-10
```

Pense no construtor canônico como a recepção única na entrada de um prédio: todo visitante passa por aquela porta, você pode conferir e ajustar o crachá antes de liberar, mas há só uma porta e ninguém entra sem estar totalmente registrado — a regra de que todos os campos precisam terminar atribuídos é o equivalente a isso.

A principal alternativa à forma explícita é o construtor compacto, tratado no próximo capítulo, que elimina a repetição da lista de parâmetros e as atribuições finais quando você só quer normalizar ou validar. Métodos fábrica estáticos (`Intervalo.de(...)`) oferecem outros caminhos de criação, mas por baixo ainda chamam `new` e passam pelo canônico. Construtores adicionais, não canônicos, precisam delegar ao canônico com `this(...)`.

Prefira a forma compacta na maioria dos casos: a forma longa é mais verbosa e ainda permite o erro de esquecer a atribuição de um campo. Reserve o construtor canônico explícito para quando a lista completa de `this.campo = campo` tornar o código mais claro para quem lê, ou quando alguma ferramenta de geração de código exigir essa forma.

### Construtor compacto

O construtor compacto é uma sintaxe que só existe em records: escreve-se o nome do tipo seguido direto de chaves, sem lista de parâmetros e sem parênteses — `public Email { ... }`. Os parâmetros são, implicitamente, os componentes do `record`; e ao final do bloco o compilador insere, também de forma implícita, a atribuição de cada parâmetro ao seu campo. Dentro do bloco você escreve apenas o "antes": validação e normalização, esta feita reatribuindo as variáveis dos parâmetros.

Ele existe para remover dois incômodos da forma longa do construtor canônico. Primeiro, a repetição da lista de componentes na assinatura — informação que já está no cabeçalho do `record`. Segundo, a sequência de `this.campo = campo`, que além de ruído é a porta para o clássico esquecimento de um campo, que fica então com `null` ou zero. O construtor compacto não tem assinatura para repetir nem atribuições para esquecer.

```java
public record Email(String valor) {
    public Email {
        valor = valor.strip().toLowerCase();   // normaliza a variável do parâmetro
        if (!valor.contains("@")) {
            throw new IllegalArgumentException("email inválido: " + valor);
        }
        // this.valor = valor;  <- inserido pelo compilador, não se escreve
    }
}

new Email("  ANA@Exemplo.COM ").valor();   // "ana@exemplo.com"
```

Dentro do bloco, os nomes dos componentes estão em escopo como parâmetros que podem ser reatribuídos. Não dá para ler `this.valor` antes do fim do bloco, porque os campos ainda não foram atribuídos. E escrever as atribuições finais à mão é erro — elas são responsabilidade do compilador.

É como preencher um formulário cujos campos já vêm impressos: você não recopia a lista de campos, apenas corrige as respostas antes de entregar; o arquivamento — a atribuição — acontece sozinho quando você sai do balcão.

Do outro lado fica o construtor canônico na forma longa, útil quando a clareza da lista explícita compensa. Métodos fábrica estáticos de parsing (`Email.parse(String)`) fazem sentido quando a criação pode falhar de um jeito que você prefira sinalizar com um `Optional` ou um tipo de resultado, em vez de exceção.

Cuidados: o construtor compacto roda em toda criação, inclusive na cópia no estilo `new Email(outro.valor())` — trabalho pesado ali custa em todo lugar. Se você precisa de uma lista de parâmetros realmente diferente (menos parâmetros, valores padrão), isso é um construtor não canônico separado que delega com `this(...)`, não o compacto. E evite efeitos colaterais no bloco — registrar em log, inserir numa coleção externa: construtor que faz esse tipo de coisa surpreende quem chama `new`.

### Validação

Validar um `record` é garantir, no momento da construção, que nenhum objeto daquele tipo consiga existir em estado inválido. Como o construtor canônico (na forma compacta ou longa) é o portão único de criação, uma checagem ali cobre todas as instâncias. É a aplicação direta, em records, da ideia de invariante vista no capítulo de encapsulamento do nível introdutório: uma condição que passa a ser verdadeira para todo objeto do tipo, do nascimento em diante.

Pule a validação na construção e os dados inválidos passam a se espalhar, estourando longe da origem — uma `quantidade` negativa, um `nome` nulo, um CPF com letras. O defeito aparece num relatório ou na camada de persistência, e você percorre várias chamadas de volta para achar por onde o valor ruim entrou. Além disso, checagens defensivas acabam repetidas em cada método que lê o campo.

No construtor compacto, use `Objects.requireNonNull` para nulos e lance `IllegalArgumentException` para violações de regra de domínio. Depois que o construtor retorna, o objeto está garantidamente válido, e os demais métodos podem confiar nos campos sem reconferir.

```java
public record Produto(String nome, int precoCentavos, int estoque) {
    public Produto {
        Objects.requireNonNull(nome, "nome");
        if (nome.isBlank())     throw new IllegalArgumentException("nome vazio");
        if (precoCentavos < 0)  throw new IllegalArgumentException("preço negativo");
        if (estoque < 0)        throw new IllegalArgumentException("estoque negativo");
    }
}
```

A validação no construtor é o controle de qualidade no fim da linha de montagem: nada defeituoso sai da fábrica, então cada estação seguinte não precisa do próprio inspetor. Na prática, isso aparece em value objects como `Cpf`, `Email`, `Percentual` (0 a 100), `Quantidade` (maior que zero). Combinado com o sistema de tipos, um método que recebe um `Percentual` nunca mais precisa checar o intervalo.

Entre as alternativas está o Bean Validation, com anotações como `@NotNull` e `@Min` processadas por um framework: é declarativo, mas só roda quando alguém invoca o validador, não na construção — então um objeto inválido ainda pode existir. Um método fábrica devolvendo `Optional<Produto>` ou um tipo de resultado cabe quando "entrada inválida" é um desfecho esperado e recuperável, como ao ler texto digitado pelo usuário, e não um bug.

Quando não usar: não faça validação cara ou externa no construtor — uma consulta ao banco para saber se o `cliente` existe acopla a criação do objeto a I/O e torna o `record` difícil de instanciar em testes; esse tipo de checagem pertence a um serviço. Restrinja a validação do construtor ao que dá para decidir só com os valores em mãos. Evite também lançar exceções verificadas: o construtor canônico não as acomoda bem, e quem chama `new` não as espera.

### Interfaces

Um `record` não pode estender uma classe — ele já estende implicitamente `java.lang.Record` —, mas pode implementar quantas interfaces quiser, com `implements`, exatamente como uma classe comum. É assim que um `record` entra em polimorfismo e se encaixa em APIs que esperam um tipo, e não uma implementação específica.

Se records fossem fechados a interfaces, seriam portadores de dados isolados: não daria para ter uma `List<Forma>` com `Circulo` e `Retangulo`, nem passar um `record` onde se espera um `Comparable` ou um `Runnable`, nem agrupar records relacionados sob um contrato comum. As interfaces devolvem tudo isso.

Você declara a interface, escreve `implements` no `record` e fornece os corpos dos métodos no corpo do `record`; os acessores de componente podem, por si, satisfazer métodos da interface. O padrão mais expressivo é uma `sealed interface` que permite um conjunto pequeno de records — o "tipo soma" moderno, que brilha em `switch` com pattern matching, pois o compilador verifica que todos os casos foram cobertos.

```java
public sealed interface Evento permits Login, Compra {}

public record Login(String usuario, Instant quando) implements Evento {}
public record Compra(String usuario, long valorCentavos) implements Evento {}

static String descreve(Evento e) {
    return switch (e) {
        case Login l  -> l.usuario() + " entrou";
        case Compra c -> c.usuario() + " gastou " + c.valorCentavos() + " centavos";
    };
}
```

Outro caso comum é `Comparable`, para dar ordem natural ao tipo:

```java
public record Versao(int maior, int menor) implements Comparable<Versao> {
    private static final Comparator<Versao> ORDEM =
        Comparator.comparingInt(Versao::maior).thenComparingInt(Versao::menor);

    @Override public int compareTo(Versao outro) {
        return ORDEM.compare(this, outro);
    }
}
```

A interface é a descrição do cargo; o `record` é um funcionário que por acaso mantém um currículo fixo e público — os seus componentes. Ser um `record` limita como ele é construído, não os papéis que ele pode assumir.

A alternativa é uma classe comum implementando a interface, necessária quando o tipo também precisa estender uma classe base ou guardar estado que não é componente. Um `enum` implementando a interface serve quando o conjunto de valores é fixo e sem dados associados.

Quando não usar: não implemente uma interface cujo contrato pressupõe mutação — um `record` não faz sentido como uma `List` que cresce. Não force um `record` sob uma interface só em nome da abstração se há uma única implementação e nenhum polimorfismo à vista. E cuidado com interfaces que trazem `default methods` presumindo semântica de identidade: elas podem destoar da semântica de valor do `record`.

### Imutabilidade superficial

O `record` garante que os campos dos componentes são `final`: terminado o construtor, você não consegue reatribuir `pedido.itens` para apontar para outra lista. Essa é uma imutabilidade superficial — rasa. Ela não garante que o objeto para o qual o componente aponta seja imutável. Se `itens` é um `ArrayList`, quem tiver essa mesma referência ainda pode chamar `itens.add(...)`, e o conteúdo "imutável" do `record` muda por baixo.

O risco é concreto porque muita gente lê "record é imutável" e assume imutabilidade profunda. Aí aparece o defeito: um `record` usado como chave de `HashMap` cujo componente do tipo lista é alterado depois da inserção, mudando o `hashCode` e fazendo a entrada se perder; ou um `record` devolvido por um método cuja lista interna quem chamou esvazia. Dois records antes iguais deixam de ser iguais sem que nenhum tenha sido "reatribuído".

Para imutabilidade de verdade, há dois caminhos, geralmente usados juntos. Primeiro, prefira componentes que já são tipos imutáveis: `String`, `int`, `LocalDate`, outros records profundamente imutáveis. Segundo, quando um componente for coleção, faça cópia na entrada, no construtor compacto, com `List.copyOf`, `Map.copyOf` ou `Set.copyOf` — que além de copiar recusam `null` e devolvem uma coleção não modificável.

```java
public record Pedido(String cliente, List<String> itens) {
    public Pedido {
        itens = List.copyOf(itens);   // cópia independente e não modificável
    }
    // o acessor gerado devolve essa lista de List.copyOf, então ler também é seguro
}

var originais = new ArrayList<>(List.of("café", "pão"));
var p = new Pedido("Ana", originais);
originais.add("bolo");     // não afeta p
p.itens().add("suco");     // UnsupportedOperationException
```

Para componentes do tipo array não há um `copyOf` que devolva forma imutável — seria `clone()` na entrada e na saída, e ainda assim o array continua mutável; o melhor é trocar o array por uma `List`.

O `record` é um envelope lacrado com o endereço escrito em tinta permanente — a referência `final`. Se dentro do envelope você põe uma lousa em vez de uma folha, qualquer um com um apagador que alcance essa lousa reescreve a mensagem: o envelope continuou lacrado, o conteúdo é que não ficou fixo.

Como alternativa às cópias, existem os embrulhos não modificáveis como `Collections.unmodifiableList`, mas eles são uma visão sobre uma lista de fundo ainda mutável — se alguém retém a referência de fundo, a visão muda junto; `List.copyOf` é mais forte por ser cópia independente de verdade. Bibliotecas de coleções imutáveis, como o `ImmutableList` do Guava, servem como tipo de componente. Ou, mais simples, projetar os componentes como primitivos e value types, e o problema não surge.

Quando isso pesa menos: se o `record` é um DTO local de vida curta, que nunca vira chave de mapa, nunca cruza fronteira entre threads e nunca é exposto a código que o mutaria, dá para pular a cerimônia das cópias defensivas — mas é um julgamento caso a caso, e o padrão seguro, para qualquer `record` que escape do método onde nasceu, é copiar.

---

# Módulo 5 — Collections fundamentais

Coleções (collections) são estruturas de dados que agrupam múltiplos valores sob uma única abstração. Java oferece o Collection Framework, um conjunto de interfaces (`Collection`, `List`, `Set`, `Map`) e implementações (`ArrayList`, `HashMap`, etc.) que resolvem os padrões mais comuns de armazenamento e busca de dados. Este módulo explora essas interfaces e suas implementações principais, ensinando quando usar cada uma.

## Collection Framework

Antes de entrar em `List`, `Set`, `Map` e as demais coleções específicas, vale entender o desenho geral que sustenta todas elas. O Collection Framework não é uma classe nem uma coleção: é uma família de tipos organizada em torno de algumas interfaces raiz, um catálogo de estruturas de dados prontas e uma convenção de uso que separa o que você promete (a interface) do como aquilo é feito por dentro (a implementação).

Este capítulo apresenta os três pilares dessa organização. Primeiro, a interface `Collection`, o contrato comum que quase toda coleção do Java cumpre. Depois, a ideia de estruturas de dados, o conceito de computação que o framework empacota para você não ter que reimplementar. Por fim, a distinção entre interfaces e implementações, o hábito de código que faz o resto do módulo valer a pena na prática.

### `Collection`

`Collection<E>` é a interface que está na raiz de quase toda a hierarquia de coleções do Java. `List`, `Set` e `Queue` herdam dela, e portanto todo `ArrayList`, todo `HashSet` e todo `ArrayDeque` é, ao mesmo tempo, uma `Collection`. Ela define o vocabulário mínimo que qualquer agrupamento de elementos precisa oferecer: `add`, `remove`, `contains`, `size`, `isEmpty`, `clear`, `iterator` e, desde o Java 8, `stream`. A única grande ausente da hierarquia é `Map`, que agrupa pares chave-valor e por isso não cabe no contrato de `Collection` — mas suas visões (`keySet`, `values`, `entrySet`) são `Collection`s.

Sem uma interface raiz assim, cada tipo de coleção teria sua própria API. Foi mais ou menos o que aconteceu no Java 1.0 e 1.1: `Vector`, `Hashtable`, `array` e `Enumeration` eram mundos separados, com nomes de método diferentes para a mesma ideia, e um método que quisesse aceitar "qualquer conjunto de valores" não tinha como declarar isso. Você acabava escrevendo uma sobrecarga para `Vector`, outra para array, e convertendo na mão.

Com `Collection`, um método declara `Collection<Pedido>` no parâmetro e passa a aceitar lista, conjunto, fila — qualquer coisa que agrupe `Pedido`. O código que consome não precisa saber qual é a implementação concreta:

```java
double total(Collection<Pedido> pedidos) {
    double soma = 0;
    for (Pedido p : pedidos) {   // funciona para List, Set, Queue...
        soma += p.valor();
    }
    return soma;
}

total(new ArrayList<>(lista));
total(new HashSet<>(conjunto));
```

Pense em `Collection` como o padrão de encaixe de uma tomada: aparelhos muito diferentes por dentro — uma geladeira, um carregador, um abajur — cabem na mesma tomada porque todos respeitam o mesmo contrato de pinos. O método `total` é a tomada; `ArrayList` e `HashSet` são os aparelhos.

As alternativas a programar contra `Collection` são descer um nível (usar `List` ou `Set` quando você realmente depende de ordem ou de unicidade) ou subir um nível (usar `Iterable`, ainda mais genérico, quando só precisa percorrer os elementos com `for-each` e nada mais). Fora da biblioteca padrão, coleções de terceiros como as do Guava e do Eclipse Collections também implementam `Collection`, então o mesmo método continua servindo.

Quando não usar: se o seu dado é um mapeamento chave-valor, o tipo certo é `Map`, que não é `Collection`. E quando desempenho com primitivos é crítico — somar dez milhões de `int` num laço apertado —, um `int[]` puro evita o custo de boxing que uma `Collection<Integer>` impõe.

### Estruturas de dados

Estrutura de dados é o nome que a computação dá às diferentes maneiras de organizar valores na memória: array contíguo, lista ligada, tabela de espalhamento (hash), árvore balanceada, heap. Cada arranjo tem um custo diferente para as operações básicas — inserir, buscar, remover, percorrer em ordem — e nenhum é o melhor em tudo. O Collection Framework é, antes de mais nada, um catálogo dessas estruturas já implementadas, testadas e afinadas ao longo de décadas.

Não fosse esse catálogo, cada projeto reescreveria as mesmas estruturas. Um array que cresce sozinho quando enche, uma lista ligada com ponteiros de ida e volta, uma tabela hash que trata colisões e redimensiona quando fica cheia demais — cada uma é dezenas de linhas de código sutil, com casos de borda que só aparecem em produção. É trabalho que não agrega valor ao seu sistema, porque já foi feito e está na `java.util`.

O framework mapeia estrutura em classe de forma bem direta: `ArrayList` é um array dinâmico; `LinkedList` é uma lista duplamente ligada; `HashMap` e `HashSet` são tabelas hash; `TreeMap` e `TreeSet` são árvores rubro-negras (balanceadas, mantêm ordem); `ArrayDeque` é um buffer circular; `PriorityQueue` é um heap binário. Escolher a coleção certa é, no fundo, conhecer o custo de cada uma:

| Operação | `ArrayList` | `LinkedList` | `HashSet` | `TreeSet` |
|---|---|---|---|---|
| acesso por índice | O(1) | O(n) | — | — |
| `contains` | O(n) | O(n) | O(1) | O(log n) |
| inserir no fim | O(1)* | O(1) | O(1) | O(log n) |
| percorrer em ordem | inserção | inserção | nenhuma | ordenada |

O impacto é concreto. Imagine remover duplicatas de uma lista de cem mil nomes verificando, para cada um, se ele já apareceu. Com `list.contains` dentro do laço, são cem mil buscas lineares — bilhões de comparações, segundos de espera. Trocando a verificação para um `HashSet`, cada `contains` é praticamente instantâneo e o programa termina antes de você piscar. É a diferença entre procurar um nome folheando a lista telefônica página por página e consultar um índice que aponta direto para a página certa.

Abrir mão do catálogo pronto leva a três caminhos: implementar a estrutura você mesmo — justificável só em estudo ou em nichos de desempenho extremo —, recorrer a bibliotecas especializadas (coleções para primitivos sem boxing, estruturas persistentes) ou, quando os dados não cabem na memória, delegar a organização a um banco de dados com seus índices.

Quando não se preocupar com isso: para uma coleção de três ou quatro elementos, a diferença de custo entre estruturas é irrelevante e escolher a "ótima" é over-engineering. Um `ArrayList` resolve a imensa maioria dos casos; só vale trocar quando o perfil de uso (muitas buscas, necessidade de ordem, muitas remoções no meio) e o volume de dados pedem.

### Interfaces × implementações

No Collection Framework, os tipos vêm em dois papéis. As interfaces — `Collection`, `List`, `Set`, `Map`, `Queue`, `Deque` — descrevem o que a coleção faz. As implementações — `ArrayList`, `LinkedList`, `HashSet`, `TreeSet`, `HashMap`, `ArrayDeque` — decidem como isso é feito por dentro, com qual estrutura de dados e com quais custos. A recomendação de projeto é declarar variáveis, parâmetros e retornos usando a interface, e citar a classe concreta só na hora de instanciar, no `new`.

Ignorada essa disciplina, o tipo concreto vaza para todo lado. Um campo declarado como `ArrayList<Cliente> clientes` amarra a classe inteira àquela implementação: o dia em que você precisar de uma lista thread-safe, ou de uma `LinkedList` porque há muitas remoções no início, a troca não é uma linha — é mudar a declaração do campo, as assinaturas dos métodos que recebem ou devolvem esse campo, e possivelmente o código de quem chama esses métodos. Um método que declara `HashSet<String>` no parâmetro obriga quem chama a ter exatamente um `HashSet`, mesmo que um `TreeSet` servisse igual.

Declarando pela interface, a implementação fica trocável num ponto só:

```java
public class Agenda {
    private final List<Contato> contatos = new ArrayList<>();  // troca aqui, só aqui

    public void adicionar(Contato c) { contatos.add(c); }
    public List<Contato> todos() { return List.copyOf(contatos); }
}

void importar(Collection<Contato> novos) {   // aceita List, Set, o que vier
    novos.forEach(this::adicionar);
}
```

Se `ArrayList` virar `CopyOnWriteArrayList` para uso concorrente, muda a linha do `new` e nada mais: o resto da classe fala com `List`, e `List` continua sendo `List`. É o mesmo princípio de dirigir um carro pela interface "volante, pedais, câmbio" sem saber se o motor é a combustão ou elétrico — trocar o motor não muda como você dirige.

A alternativa mais comum hoje é `var` em variáveis locais: `var contatos = new ArrayList<Contato>()` infere o tipo concreto e é perfeitamente aceitável para uma variável de vida curta dentro de um método, onde nenhuma assinatura pública é afetada. Outra situação legítima de usar o tipo concreto é quando você depende de métodos que só a implementação oferece — `ArrayDeque` usado pelos seus métodos de pilha, por exemplo, embora mesmo aí o mais comum seja declarar como `Deque`.

Quando relaxar a regra: em scripts, protótipos e código de teste curto, a cerimônia rende pouco. Mas para campos de classe, e sobretudo para parâmetros e retornos de métodos que outras partes do sistema consomem, declarar pela interface mais geral que ainda expressa o que você precisa é o que mantém o código aberto a mudança.

## List

Entre as coleções do Java, `List` é a que mais se parece com o array que você já conhece: uma sequência de elementos em que cada posição tem um número e a ordem de entrada é preservada. A diferença é que uma `List` cresce e encolhe sozinha, oferece dezenas de operações prontas e é um tipo do Collection Framework, integrado a tudo o que gira em torno de `Collection` e `Iterable`.

Este capítulo parte da interface `List` e da sua implementação mais usada, o `ArrayList`, e depois destrincha as três características que definem o comportamento de uma lista no dia a dia: a ordem estável dos elementos, o acesso por índice e a aceitação de valores duplicados. São esses três traços que decidem quando uma `List` é a coleção certa e quando outra estrutura serve melhor.

### `List`

`List<E>` é a interface que representa uma coleção ordenada e indexada de elementos. Ela estende `Collection<E>` e acrescenta a esse contrato tudo o que depende de posição: `get(int)`, `set(int, E)`, `add(int, E)`, `remove(int)`, `indexOf(Object)`, `lastIndexOf(Object)` e `subList(int, int)`. Uma `List` mantém os elementos na ordem em que foram inseridos, permite elementos repetidos e admite `null` na maioria das implementações. É o tipo que você declara sempre que precisa de "uma porção de coisas, nessa ordem, e quero poder falar da primeira, da terceira, da última".

Sem `List`, o recurso equivalente na linguagem é o array. O array resolve o armazenamento sequencial, mas tem tamanho fixo: definido na criação, nunca muda. Para uma coleção que cresce conforme o programa roda — pedidos que chegam, linhas lidas de um arquivo, resultados de uma busca —, o array obriga a estimar um tamanho máximo e a controlar na mão quantas posições estão de fato ocupadas, ou a criar um array novo e copiar tudo toda vez que ele enche. Além disso, o array não tem métodos: inserir no meio, remover um elemento pelo valor, procurar a posição de algo, tudo isso é laço escrito à mão.

`List` resolve os dois problemas. O tamanho é dinâmico: `add` acrescenta ao fim e a lista se encarrega de arrumar espaço. E a API cobre as operações comuns sem laço explícito:

```java
List<String> tarefas = new ArrayList<>();
tarefas.add("comprar pão");
tarefas.add("pagar conta");
tarefas.add("comprar pão");        // duplicata aceita

tarefas.add(1, "ligar para o médico");  // insere na posição 1
String primeira = tarefas.get(0);        // "comprar pão"
int onde = tarefas.indexOf("pagar conta"); // 2, após a inserção
tarefas.remove("comprar pão");           // remove a primeira ocorrência
List<String> duas = tarefas.subList(0, 2);
```

Vale aqui a disciplina já vista em Interfaces × implementações: declarar a variável, o parâmetro ou o retorno como `List<E>` e deixar a implementação concreta só no `new`.

As alternativas a `List` são as outras coleções, cada uma abrindo mão de algo que a lista garante. `Set` não aceita duplicatas e, na forma `HashSet`, não preserva ordem. `Queue` e `Deque` privilegiam inserção e remoção nas pontas em vez de acesso por índice. Um array puro continua fazendo sentido quando o tamanho é realmente fixo e conhecido, ou quando se guarda um grande volume de primitivos e o custo de embrulhar cada valor num objeto (`Integer`, `Double`) pesa.

Quando não usar `List`: se o que importa é garantir que não haja repetição, o tipo certo é `Set`; se você nunca acessa por posição e só percorre os elementos, `Collection` ou `Iterable` no parâmetro deixam o método mais genérico sem custo nenhum.

### `ArrayList`

`ArrayList<E>` é a implementação de `List` construída sobre um array interno que cresce automaticamente. Por dentro, ela guarda um `Object[]` e um contador de quantas posições estão ocupadas. Enquanto sobra espaço, `add` só escreve na próxima posição livre e incrementa o contador. Quando o array enche, o `ArrayList` cria um array maior — tipicamente cerca de 1,5 vez o tamanho anterior —, copia os elementos para ele e passa a usá-lo. Esse redimensionamento acontece poucas vezes ao longo da vida da lista, e por isso se diz que o custo de acrescentar no fim é O(1) *amortizado*: a maioria dos `add` é instantânea e o custo raro da cópia se dilui.

O problema que o `ArrayList` resolve é justamente o trabalho manual de fazer um array crescer. Sem ele, você escreveria toda vez a mesma lógica: detectar que encheu, alocar um novo, `System.arraycopy`, substituir a referência, e ainda manter à parte a contagem de elementos válidos, porque `array.length` diz o tamanho alocado, não quantos você usou. É código sutil, cheio de erros de índice, que não tem nada a ver com o problema do seu sistema.

O ponto forte do `ArrayList` é o acesso por índice: como os elementos ficam num array contíguo, `get(i)` e `set(i, x)` calculam o endereço direto e retornam em tempo constante, O(1). O ponto fraco é mexer no meio: `add(i, x)` e `remove(i)` para um `i` no começo ou no miolo precisam deslocar todos os elementos seguintes uma casa, o que é O(n).

```java
List<Integer> notas = new ArrayList<>(100); // capacidade inicial evita cópias
for (int i = 0; i < 100; i++) {
    notas.add(i * i);
}
int centesima = notas.get(99); // acesso imediato
```

Passar a capacidade inicial no construtor, quando você tem ideia do tamanho final, elimina os redimensionamentos e as cópias intermediárias.

No lugar dele entram `LinkedList`, uma lista duplamente ligada em que inserir e remover nas pontas é O(1), mas o acesso por índice é O(n) porque exige percorrer nó a nó; `CopyOnWriteArrayList`, versão thread-safe pensada para cenários com muitas leituras e pouquíssimas escritas concorrentes; e o array puro, mais enxuto quando o tamanho não muda. Na prática, `ArrayList` é a escolha padrão e atende à grande maioria dos casos.

Quando não usar `ArrayList`: quando o padrão de uso é inserir e remover muito no início ou no meio de uma lista grande — aí o deslocamento constante pesa e vale medir uma `LinkedList` ou repensar a estrutura —, ou quando várias threads escrevem na mesma lista ao mesmo tempo, situação em que um `ArrayList` sem sincronização externa corrompe o estado interno.

### Ordem

A ordem de uma `List` é a ordem de inserção: os elementos ficam guardados na sequência em que foram adicionados e permanecem assim até que você os mova, insira algo no meio ou remova algo. Percorrer a lista duas vezes seguidas — com `for` indexado, `for-each` ou `iterator` — produz sempre a mesma sequência. Essa previsibilidade é uma garantia da interface `List`, não um detalhe de implementação: vale para `ArrayList`, `LinkedList` e qualquer outra lista.

Uma coleção que não preserva ordem deixa você à mercê do seu arranjo interno. Um `HashSet`, por exemplo, distribui os elementos conforme o `hashCode` de cada um; a ordem em que eles saem no laço não tem relação com a ordem em que entraram e pode até mudar entre execuções. Para exibir uma lista de resultados na sequência em que o usuário digitou, montar um histórico de eventos ou processar linhas de um arquivo de cima para baixo, essa bagunça é inaceitável.

`List` resolve isso mantendo cada novo elemento no fim por padrão (`add(e)`) e permitindo inserção em posição específica (`add(indice, e)`), que empurra os demais para a frente. Quando você precisa de uma ordem diferente da de inserção — alfabética, numérica, por data —, a lista pode ser reordenada explicitamente com `list.sort(comparador)` ou `Collections.sort(list)`, e ela passa a refletir essa nova ordem até a próxima alteração.

```java
List<String> entrada = new ArrayList<>(List.of("banana", "abacaxi", "caju"));
// ordem de inserção preservada
System.out.println(entrada); // [banana, abacaxi, caju]

entrada.sort(Comparator.naturalOrder());
System.out.println(entrada); // [abacaxi, banana, caju]

entrada.add(0, "damasco");   // insere no início
System.out.println(entrada); // [damasco, abacaxi, banana, caju]
```

Pense na diferença entre uma fila de pessoas e um saco de bolinhas numeradas. A `List` é a fila: existe um primeiro, um segundo, um último, e a posição de cada um só muda se alguém entrar, sair ou for deslocado. O `HashSet` é o saco: todas as bolinhas estão lá, mas não há "a terceira bolinha".

As alternativas quando você quer ordem, mas não a de inserção: `TreeSet` e `TreeMap` mantêm os elementos permanentemente ordenados pelo valor, reordenando a cada inserção; `LinkedHashSet` e `LinkedHashMap` preservam a ordem de inserção sem aceitar duplicatas; `PriorityQueue` mantém o menor elemento sempre acessível, sem ordenar o resto.

Quando a ordem não importa — você só vai contar os elementos, somar, verificar se algo existe — insistir numa `List` ordenada não traz benefício, e uma coleção que não paga o custo de manter ordem (um `HashSet` para testes de pertinência, por exemplo) pode ser mais adequada.

### Índices

Cada elemento de uma `List` ocupa uma posição identificada por um número inteiro chamado índice. A contagem começa em zero: o primeiro elemento está no índice `0`, o segundo no `1`, e o último no índice `size() - 1`. É o índice que dá à `List` a sua parte da API que `Collection` não tem: `get(i)` lê o elemento da posição, `set(i, x)` troca o que está lá sem mudar o tamanho, `add(i, x)` insere empurrando o resto, `remove(i)` retira e fecha o buraco, `indexOf(x)` responde em que posição um valor aparece pela primeira vez.

Antes das coleções, o índice é o mesmo conceito do array: `array[3]`. O que a `List` acrescenta é fazer isso conviver com o tamanho dinâmico e com as operações que reorganizam a sequência. Sem acesso por índice, referir-se a "o item da posição 5" exigiria percorrer a coleção contando elementos até chegar lá — que é, aliás, o que acontece por baixo dos panos quando você usa `get(i)` numa `LinkedList`.

O acesso por índice permite laços clássicos com contador, iteração de trás para frente, pular de dois em dois, comparar o elemento `i` com o `i + 1`, ou modificar a lista durante o percurso controlando o índice na mão:

```java
List<String> nomes = new ArrayList<>(List.of("Ana", "Beto", "Ana", "Caio"));

for (int i = 0; i < nomes.size(); i++) {
    System.out.println(i + ": " + nomes.get(i));
}

// substituir sem alterar o tamanho
nomes.set(1, "Roberto");

// percorrer de trás para frente removendo com segurança
for (int i = nomes.size() - 1; i >= 0; i--) {
    if (nomes.get(i).equals("Ana")) {
        nomes.remove(i);
    }
}
```

O maior perigo do índice é o `IndexOutOfBoundsException`: acessar `get(size())`, esquecer o `-1` ao pegar o último elemento, ou usar dentro do laço um índice que já foi invalidado por uma remoção. A regra é que índices válidos para leitura vão de `0` a `size() - 1`; para `add(i, x)`, o `i` pode chegar a `size()` (inserir no fim).

Quando o índice não é necessário, entram o `for-each`, para visitar cada elemento uma vez na ordem natural; o `Iterator`, que permite remover com segurança durante o percurso via `it.remove()`; e as streams, para transformar e filtrar de forma declarativa. Todas evitam o risco de errar a conta do índice.

Quando não usar índice: para um simples percurso do início ao fim, o laço indexado é mais verboso e mais sujeito a erro que o `for-each`. E sobre uma `LinkedList`, um laço que chama `get(i)` a cada volta transforma um percurso O(n) num O(n²), porque cada `get` recomeça a contagem desde a ponta — nesse caso, o `for-each` ou o `Iterator` são obrigatórios.

### Duplicidade

Uma `List` aceita elementos duplicados: você pode adicionar o mesmo valor — ou valores considerados iguais por `equals` — quantas vezes quiser, e cada inserção vira uma entrada própria, com seu próprio índice. Uma lista `["pão", "leite", "pão"]` tem tamanho 3, e os dois `"pão"` estão nas posições 0 e 2, independentes um do outro. Essa é uma escolha deliberada da interface `List`: numa sequência, repetição é informação legítima.

Quando a coleção não aceita duplicatas, representar certas situações fica torto. Um carrinho de compras com duas unidades do mesmo produto, um registro de cada vez que um botão foi clicado, as notas de um aluno em que dois `7.5` são dois acertos diferentes — em todos esses casos, jogar fora a repetição é jogar fora dado. Estruturas que impõem unicidade, como `Set`, colapsariam as ocorrências numa só.

A `List` lida com duplicatas de forma coerente em toda a API. `add` nunca recusa um elemento por já existir. `contains(x)` e `indexOf(x)` usam `equals` e respondem sobre a primeira ocorrência. `remove(Object)` — a versão que recebe o elemento, não o índice — remove apenas a primeira ocorrência, deixando as demais. Quando você de fato quer eliminar repetições, o caminho usual é passar por um `Set` que preserve ordem ou usar `distinct()` numa stream:

```java
List<String> cliques = new ArrayList<>();
cliques.add("salvar");
cliques.add("salvar");
cliques.add("cancelar");
System.out.println(cliques.size()); // 3 — as duas ocorrências contam

cliques.remove("salvar");           // remove só a primeira
System.out.println(cliques);        // [salvar, cancelar]

// remover todas as duplicatas preservando a ordem
List<String> semRepetir = new ArrayList<>(new LinkedHashSet<>(cliques));

// contar quantas vezes cada valor aparece
Map<String, Long> contagem = cliques.stream()
    .collect(Collectors.groupingBy(v -> v, Collectors.counting()));
```

A alternativa direta é o `Set`, quando a repetição não faz sentido no seu modelo: um conjunto de CPFs cadastrados, as tags de um artigo, os IDs já processados. Se você precisa ao mesmo tempo de unicidade e de saber quantas vezes cada item apareceu, um `Map<Elemento, Integer>` de contagem — ou `Map<Elemento, List<Ocorrência>>` — costuma modelar melhor do que uma lista.

Quando não usar uma `List` por causa da duplicidade: se a presença de dois elementos iguais é sempre um bug — um cadastro em dobro, um item somado duas vezes por engano —, deixar a `List` aceitar isso silenciosamente esconde o problema. Um `Set` transforma a tentativa de inserir repetido num `add` que retorna `false`, tornando a regra de unicidade explícita e verificável.

## Set

Depois de `List`, `Set` é a segunda grande família de coleções do Collection Framework e a que troca a ideia de sequência pela de conjunto: uma coleção em que cada elemento aparece no máximo uma vez e em que, na forma mais comum, não existe noção de ordem nem de posição. Onde a `List` responde "o que está na posição 3?", o `Set` responde "esse elemento está aqui?".

Este capítulo apresenta a interface `Set` e a implementação que você vai usar quase sempre, o `HashSet`, e então examina os dois pilares que fazem um conjunto funcionar: a garantia de unicidade dos elementos e o par `equals`/`hashCode`, que é quem de fato decide se dois objetos contam como "o mesmo" para efeito de conjunto.

### `Set`

`Set<E>` é a interface do Collection Framework que representa um conjunto: uma coleção sem elementos repetidos, no sentido matemático da palavra. Ela estende `Collection<E>` e não acrescenta nenhum método novo — o que muda é o contrato. O `add` passa a recusar um elemento que já esteja presente, e o `size` nunca conta o mesmo valor duas vezes. As implementações concretas são `HashSet` (a padrão), `LinkedHashSet` (mantém a ordem de inserção) e `TreeSet` (mantém os elementos ordenados).

Sem `Set`, para representar "o conjunto de CPFs cadastrados", "as tags de um artigo" ou "os IDs já processados" você usaria uma `List` e teria que checar `contains` na mão antes de cada `add` — código repetido e fácil de esquecer. E, como a `List` aceita repetição por contrato, uma inserção acidental em dobro passa despercebida.

`Set` resolve isso embutindo a regra na estrutura. `add(e)` devolve `boolean`: `false` quando o elemento já existia, e nesse caso nada é inserido. A operação central passa a ser `contains`, rápida no `HashSet`. E as operações clássicas de conjunto saem de graça: `addAll` faz união, `retainAll` faz interseção, `removeAll` faz diferença.

```java
Set<String> tagsVistas = new HashSet<>();
boolean nova = tagsVistas.add("java");   // true
tagsVistas.add("java");                   // false, já existia
tagsVistas.add("collections");            // true
System.out.println(tagsVistas.size());    // 2

if (!tagsVistas.contains("python")) {
    // processa a tag inédita...
}

Set<String> a = new HashSet<>(List.of("a", "b", "c"));
Set<String> b = new HashSet<>(List.of("b", "c", "d"));
Set<String> intersecao = new HashSet<>(a);
intersecao.retainAll(b);                  // fica [b, c]
```

Pense na lista de convidados na portaria de um evento. Apresentar um nome que já está na lista não muda nada; o que importa é se o nome consta, não quantas vezes nem em que ordem foi anotado. Esse é exatamente o contrato do `Set`: presença, não posição nem contagem.

As alternativas são as outras coleções. `List` quando ordem, índice ou repetição são informação que você quer guardar. `Map` quando cada elemento único precisa carregar um valor associado — e vale lembrar que `keySet()` devolve justamente um `Set` com as chaves de um `Map`. Para dados estáticos e muito consultados, uma `List` ordenada com busca binária também resolve o teste de pertinência.

Quando não usar `Set`: quando a ordem de inserção ou o acesso por posição importam, o tipo é `List`; quando você precisa saber quantas vezes cada elemento apareceu, um `Map<E, Integer>` de contagem modela melhor; e para um punhado de valores fixos, percorrer uma `List` pequena é trivial e às vezes mais legível.

### `HashSet`

`HashSet<E>` é a implementação padrão de `Set`, construída sobre uma tabela de espalhamento — internamente, um `HashMap` em que os elementos são as chaves e o valor é uma constante de enfeite. Teste de pertinência, inserção e remoção custam O(1) em média. Em troca, não há garantia de ordem: a sequência em que os elementos saem no laço depende dos códigos de espalhamento e do tamanho da tabela, e pode mudar conforme o conjunto cresce.

O problema que ele resolve é o custo de manter unicidade. Um conjunto improvisado sobre uma `List` faz o `contains` varrer elemento por elemento, O(n); deduplicar um volume grande de dados ou testar pertinência dentro de um laço com essa estrutura é a diferença entre instantâneo e inviável.

`HashSet` resolve distribuindo cada elemento num "balde" escolhido pelo seu `hashCode`. Para responder `contains(x)`, ele calcula `x.hashCode()`, vai direto ao balde correspondente e compara com `equals` apenas os poucos elementos que estão lá. `add` e `remove` funcionam do mesmo jeito. O custo de crescer: quando a tabela passa do fator de carga (0,75 por padrão), ela é redimensionada e os elementos redistribuídos — algo raro, cujo custo se dilui.

```java
Set<Long> processados = new HashSet<>();
for (Pedido p : pedidos) {
    if (processados.add(p.getId())) {   // add devolve false se o id repetir
        processar(p);
    }
}

// remover duplicatas de uma lista, sem garantir ordem
List<String> semRepetir = new ArrayList<>(new HashSet<>(listaComRepeticoes));

// capacidade inicial evita redimensionamentos quando o tamanho é previsível
Set<String> nomes = new HashSet<>(1000);
```

É como um chapelário com ganchos numerados em que o número vem do seu ticket. Em vez de percorrer a arara inteira procurando seu casaco, o atendente lê o número do ticket e vai a um gancho só. O `hashCode` é o número do ticket; o balde é o gancho.

As outras implementações de `Set` cobrem o que o `HashSet` abre mão. `LinkedHashSet` preserva a ordem de inserção a um pequeno custo de memória. `TreeSet` mantém os elementos ordenados, com operações O(log n), e exige que os elementos sejam `Comparable` ou que você forneça um `Comparator`. `Set.of(...)` cria conjuntos imutáveis pequenos. Para acesso concorrente, `ConcurrentHashMap.newKeySet()`.

Quando não usar `HashSet`: quando você precisa de ordem previsível na iteração (use `LinkedHashSet` ou `TreeSet`); quando os elementos têm `hashCode` ausente ou mal feito, caso em que todos caem no mesmo balde e o desempenho volta a O(n); quando várias threads escrevem ao mesmo tempo, já que `HashSet` não é thread-safe; e quando o conjunto é minúsculo e fixo, situação em que `Set.of` é mais simples.

### Unicidade

Unicidade é a propriedade que define o `Set`: o conjunto guarda no máximo um elemento de cada grupo de valores considerados iguais. A pergunta "esse elemento já está aqui?" é respondida por `equals` — e, nos conjuntos baseados em hash, pré-filtrada por `hashCode`. Tentar adicionar um elemento igual a um que já existe é uma operação sem efeito, que retorna `false`; o `size` jamais contabiliza o mesmo valor duas vezes.

Sem essa garantia embutida, a unicidade volta a ser o `if (!lista.contains(x)) lista.add(x)` manual e espalhado que já vimos ao apresentar `Set`. O que a propriedade de unicidade acrescenta a essa discussão é a pergunta de fundo: *qual* critério de "igual" está em jogo? O que o seu domínio entende por igual pode não ser o que o código faz, caso você não tenha sobrescrito `equals` na classe do elemento.

Quem decide a unicidade é inteiramente o `equals` — apoiado no `hashCode` no caso do `HashSet` —, o par de métodos que o próximo conceito detalha. Duas instâncias distintas na memória que sejam `equals` contam como uma só. Se a classe do elemento não sobrescreve `equals`, vale a comparação de identidade herdada de `Object`, e aí cada `new` é um elemento diferente — um `Set<Ponto>` com `new Ponto(1, 2)` inserido duas vezes fica com os dois, o que quase sempre é um bug.

```java
record CPF(String numero) {}

Set<CPF> cadastrados = new HashSet<>();
cadastrados.add(new CPF("111"));
boolean inserido = cadastrados.add(new CPF("111")); // false: record define equals
System.out.println(cadastrados.size());             // 1

// sem equals/hashCode adequados:
class Ponto {
    int x, y;
    Ponto(int x, int y) { this.x = x; this.y = y; }
}
Set<Ponto> pontos = new HashSet<>();
pontos.add(new Ponto(1, 2));
pontos.add(new Ponto(1, 2));
System.out.println(pontos.size());                  // 2 — cada new é "único"
```

Há uma armadilha extra: alterar um elemento depois de inseri-lo num `HashSet`, de modo que o `hashCode` mude, "perde" o elemento — ele passa a estar no balde errado e nem `contains` o encontra mais. Por isso, os campos que definem a igualdade de um elemento de `Set` devem ser, na prática, imutáveis.

Pense num controle de presença identificado pelo número de matrícula. Duas fichas preenchidas pela mesma pessoa, com a mesma matrícula, contam como uma presença. Mas, se você identificar cada presença pela folha de papel em si, e não pela matrícula, duas folhas viram duas pessoas. O critério de identidade é o `equals`.

Alternativas a depender da unicidade do `Set`: um `Map<Chave, Valor>` quando a unicidade recai sobre uma chave derivada do objeto, não sobre o objeto inteiro; `distinct()` numa stream para uma deduplicação pontual; uma restrição `unique` no banco quando a fonte da verdade é o dado persistido.

Quando tomar cuidado: se você de fato precisa contar ocorrências, o `Set` joga essa informação fora — use um `Map` de contagem ou `Collectors.counting()`. E se comparar dois elementos por igualdade é caro para objetos grandes, indexe o `Set` ou o `Map` por uma chave natural mais barata em vez do objeto todo.

### `equals/hashCode`

`equals` e `hashCode` são dois métodos herdados de `Object` que, juntos, definem quando dois objetos são "o mesmo" para as coleções. `equals(Object)` responde se dois objetos são logicamente iguais. `hashCode()` devolve um `int` que resume o objeto para fins de espalhamento. Eles têm um contrato: objetos iguais por `equals` são obrigados a ter o mesmo `hashCode`; objetos diferentes idealmente têm `hashCode` diferente, mas não é exigido; e ambos os resultados têm que ser consistentes enquanto o objeto não muda. A regra de ouro é sobrescrever os dois juntos — mexer em um sem o outro quebra toda coleção baseada em hash.

Sem sobrescrever nada, `Object.equals` compara identidade (`==`) e `Object.hashCode` deriva da identidade do objeto. Assim, duas instâncias "logicamente iguais" são consideradas diferentes e vão para baldes diferentes. Num `HashSet` ou `HashMap`, você não consegue recuperar o que inseriu usando uma instância nova mas equivalente, e duplicatas por valor não são detectadas.

Por que os dois métodos, e não um só: o `HashSet` primeiro usa `hashCode` para escolher o balde e depois `equals` para comparar os elementos de dentro dele. Se você sobrescreve `equals` mas não `hashCode`, dois objetos iguais podem cair em baldes diferentes e o conjunto nunca chega a compará-los — guarda os dois. Se você faz `hashCode` retornar uma constante e não toca em `equals`, tudo se amontoa num balde só e a busca degrada para O(n), além de as duplicatas por valor continuarem passando.

Para escrevê-los, inclua os mesmos campos nos dois métodos. No Java atual, um `record` gera ambos a partir dos componentes; para classes escritas à mão, `Objects.equals` e `Objects.hash` fazem o trabalho pesado; a IDE também gera; Lombok oferece `@EqualsAndHashCode`.

```java
public final class Produto {
    private final String sku;
    private final String nome;

    public Produto(String sku, String nome) {
        this.sku = sku;
        this.nome = nome;
    }

    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (!(o instanceof Produto outro)) return false;
        return sku.equals(outro.sku) && nome.equals(outro.nome);
    }

    @Override
    public int hashCode() {
        return Objects.hash(sku, nome);
    }
}
```

```java
Set<Produto> catalogo = new HashSet<>();
catalogo.add(new Produto("A1", "Caneta"));
catalogo.contains(new Produto("A1", "Caneta")); // true — só funciona com os dois métodos
```

Um `record Produto(String sku, String nome) {}` reduz tudo isso a uma linha.

`hashCode` é o CEP e `equals` é o endereço completo. O sistema de triagem dos Correios usa o CEP para levar a carta rápido ao centro de distribuição certo; depois, uma pessoa lê o endereço inteiro para achar a casa exata. Um endereço certo com CEP errado manda a carta para o centro errado e ela nunca chega — é o `equals` sem um `hashCode` compatível. O `HashSet` faz essa mesma busca em duas etapas.

As alternativas: `record` para classes que só transportam dados; `Comparable`/`Comparator` quando a coleção é um `TreeSet` ou `TreeMap`, porque essas usam `compareTo`/`compare` — e não `equals`/`hashCode` — para decidir identidade, o que às vezes surpreende; e comparar diretamente um campo de chave quando você não controla a classe do elemento.

Quando tomar cuidado: não baseie `equals`/`hashCode` num ID gerado pelo banco que é nulo antes de persistir — é uma cilada conhecida em JPA; não inclua campos derivados ou muito grandes; e, se as instâncias nunca são comparadas por valor nem colocadas em coleções de hash, o comportamento de identidade padrão já serve, e sobrescrever os métodos só adiciona ruído.

## Map

Depois de `List` e `Set`, `Map` completa o trio de estruturas que você vai usar no dia a dia — e a mais diferente das três. Enquanto `List` e `Set` guardam elementos soltos, um `Map` guarda pares: cada entrada liga uma chave a um valor, e a chave serve de endereço para chegar ao valor sem percorrer a coleção. `Map` nem estende `Collection`, justamente porque o que ele modela não é "um monte de coisas", e sim "uma coisa indexada por outra".

Este capítulo apresenta a interface `Map` e sua implementação padrão, o `HashMap`, e depois separa os dois papéis que toda entrada tem — a chave, que precisa ser única e estável, e o valor, que pode repetir e mudar à vontade. Fecha com `entrySet`, a forma correta de percorrer um `Map` quando você precisa de chave e valor ao mesmo tempo.

### `Map`

`Map<K, V>` é a interface do Collection Framework que representa uma associação entre chaves e valores: uma estrutura em que você guarda pares e depois recupera, remove ou atualiza um valor a partir da sua chave. `K` é o tipo da chave e `V` o tipo do valor — por exemplo, `Map<String, Integer>` para associar nomes a idades. Cada chave aparece no máximo uma vez; a ela corresponde exatamente um valor. Diferente de `List` e `Set`, `Map` não estende `Collection`, porque a unidade que ele armazena não é um elemento, e sim uma entrada (chave mais valor). As implementações mais comuns são `HashMap` (a padrão), `LinkedHashMap` (mantém a ordem de inserção) e `TreeMap` (mantém as chaves ordenadas).

Sem `Map`, para responder "qual o preço do produto com código X?" você manteria duas listas paralelas — uma de códigos, outra de preços — e faria `indexOf` na primeira para achar a posição e ler a segunda. É frágil: as listas podem sair de sincronia, a busca é O(n) e o código fica ilegível. A alternativa de criar uma classe com os dois campos e varrer uma `List` dela resolve a sincronia, mas continua percorrendo tudo a cada consulta.

`Map` resolve isso com uma API centrada na chave. `put(k, v)` insere ou substitui; `get(k)` devolve o valor associado ou `null` se a chave não existir; `containsKey(k)` testa a presença; `remove(k)` apaga a entrada. Há ainda os métodos que evitam o `if` manual: `getOrDefault(k, padrao)`, `putIfAbsent(k, v)`, `computeIfAbsent(k, funcao)` e `merge(k, v, funcao)`, muito usados para acumular valores.

```java
Map<String, Integer> estoque = new HashMap<>();
estoque.put("caneta", 30);
estoque.put("caderno", 12);
estoque.put("caneta", 45);            // substitui o valor anterior

int qtd = estoque.getOrDefault("lapis", 0);   // 0, chave ausente
estoque.merge("caderno", 5, Integer::sum);     // caderno passa a 17

if (estoque.containsKey("caneta")) {
    System.out.println(estoque.get("caneta")); // 45
}
```

Pense na lista telefônica: você procura pelo nome (a chave) e lê o número (o valor). Não faz sentido perguntar "qual é a quinta entrada" nem existe o mesmo nome duas vezes com números diferentes na mesma linha — é exatamente o contrato do `Map`.

As alternativas são as outras coleções e formas de indexação. `List` quando só há elementos, sem nada associado a eles. `Set` quando você quer apenas saber se algo pertence ao conjunto. Um array simples quando a "chave" é um inteiro pequeno e contíguo, que serve de índice direto. E um banco de dados quando os pares precisam sobreviver ao fim do programa.

Quando não usar `Map`: quando não existe um valor a associar (é `Set`); quando a ordem ou a posição dos elementos é a informação central (é `List`); e quando o conjunto de chaves é fixo e conhecido em tempo de compilação, caso em que um `enum` com campos, ou um `EnumMap`, costuma ser mais claro e eficiente.

### `HashMap`

`HashMap<K, V>` é a implementação padrão de `Map`, construída sobre uma tabela de espalhamento (hash table). É a classe que você instancia na esmagadora maioria das vezes que declara uma variável do tipo `Map`. Suas operações principais — `put`, `get`, `containsKey`, `remove` — custam O(1) em média, ou seja, o tempo praticamente não cresce com a quantidade de entradas. O preço disso é não haver garantia nenhuma de ordem: a sequência em que as chaves aparecem num laço não tem relação com a ordem de inserção nem com a ordem natural das chaves, e pode mudar quando o mapa cresce. `HashMap` aceita uma chave `null` e vários valores `null`.

O problema que ele resolve é o custo de localizar uma entrada. Num mapa improvisado sobre uma `List` de pares, cada `get` varre a lista inteira até casar a chave — O(n). Dentro de um laço que consulta o mapa a cada iteração, isso vira O(n²) e trava com volume de dados moderado.

`HashMap` resolve distribuindo as entradas em "baldes" escolhidos pelo `hashCode` da chave. Para um `get(k)`, ele calcula `k.hashCode()`, deriva daí o índice do balde, vai direto a ele e usa `equals` para comparar apenas as poucas chaves que ali estão. `put` e `remove` seguem o mesmo caminho. Quando o número de entradas passa do fator de carga (0,75 por padrão da capacidade), a tabela dobra de tamanho e as entradas são redistribuídas — uma operação rara cujo custo se dilui entre muitas inserções baratas.

```java
Map<String, Usuario> porEmail = new HashMap<>();
for (Usuario u : usuarios) {
    porEmail.put(u.getEmail(), u);
}
Usuario achado = porEmail.get("ana@exemplo.com");  // acesso direto, sem varrer

// contagem de ocorrências, padrão clássico com HashMap
Map<String, Integer> frequencia = new HashMap<>();
for (String palavra : texto.split(" ")) {
    frequencia.merge(palavra, 1, Integer::sum);
}

// capacidade inicial quando o tamanho é previsível, evita redimensionamentos
Map<Long, String> nomes = new HashMap<>(10_000);
```

É como o guarda-volumes de um cinema com chaves numeradas: você entrega a mochila e recebe a chave 47; na saída, apresenta a chave 47 e o atendente vai direto ao compartimento 47, sem abrir os outros. O número da chave é o `hashCode`; o compartimento é o balde.

As alternativas cobrem o que o `HashMap` abre mão. `LinkedHashMap` preserva a ordem de inserção (ou de acesso, útil para caches LRU) a um custo pequeno de memória. `TreeMap` mantém as chaves ordenadas e oferece consultas por faixa, com operações O(log n). `ConcurrentHashMap` é a escolha quando várias threads leem e escrevem ao mesmo tempo. `Map.of(...)` cria mapas imutáveis pequenos. `EnumMap` é imbatível quando as chaves são de um `enum`.

Quando não usar `HashMap`: quando você precisa de ordem previsível na iteração (use `LinkedHashMap` ou `TreeMap`); quando várias threads o modificam concorrentemente, pois `HashMap` não é thread-safe e pode até entrar em laço infinito num resize concorrente (use `ConcurrentHashMap`); e quando a classe da chave tem `hashCode` ausente ou mal distribuído, o que joga tudo em poucos baldes e degrada o desempenho para O(n).

### Chave

A chave é a parte da entrada que serve de identificador: é por ela que você insere, busca, atualiza e remove um valor no `Map`. Cada chave é única dentro do mapa — chamar `put` com uma chave que já existe não cria uma segunda entrada, apenas troca o valor associado. O conjunto de todas as chaves de um mapa é, conceitualmente, um `Set`, e o método `keySet()` devolve exatamente isso. Por isso a chave carrega as mesmas exigências de um elemento de `Set`: precisa ter `equals` e `hashCode` coerentes, e esses campos precisam ser estáveis enquanto a chave estiver no mapa.

Sem entender o papel da chave, o erro típico é usar como chave um objeto cuja identidade não está bem definida. Se a classe da chave não sobrescreve `equals`/`hashCode`, vale a comparação por identidade herdada de `Object`: você guarda um valor com `new Ponto(1, 2)` e nunca mais o recupera, porque `map.get(new Ponto(1, 2))` cria outra instância, "diferente" da primeira aos olhos do mapa. O `get` devolve `null` e o bug é silencioso.

A regra prática é usar como chave um tipo imutável e com igualdade por valor: `String`, os wrappers numéricos (`Integer`, `Long`), `UUID`, `enum`, `LocalDate`, ou um `record` que você mesmo definiu — o `record` já gera `equals` e `hashCode` a partir dos componentes. Se a chave é uma classe escrita à mão, sobrescreva os dois métodos com os mesmos campos. E nunca altere um campo que participa da igualdade depois de a chave entrar no mapa: o `hashCode` muda, a entrada fica "presa" no balde antigo e some das buscas.

```java
record Coordenada(int linha, int coluna) {}

Map<Coordenada, String> tabuleiro = new HashMap<>();
tabuleiro.put(new Coordenada(0, 0), "torre");
tabuleiro.put(new Coordenada(1, 4), "peão");

// recupera com uma instância nova, mas equivalente — funciona por causa do record
String peca = tabuleiro.get(new Coordenada(0, 0));  // "torre"

// chave composta sem record daria trabalho: teria que concatenar "0,0" numa String
```

Pense no número de matrícula de um aluno numa secretaria escolar. É ele que aponta para a ficha (o valor). Dois cadastros com a mesma matrícula não coexistem — o segundo atualiza o primeiro. E a matrícula não muda no meio do semestre; se mudasse, ninguém acharia a ficha pelo número antigo.

Alternativas na hora de escolher a chave: quando a identificação natural tem várias partes, um `record` como chave composta costuma ser melhor que concatenar tudo numa `String`; quando as chaves são valores de um `enum`, `EnumMap` dispensa até o `hashCode`; quando a chave seria um inteiro pequeno e sequencial, um array ou `List` indexada é mais direto.

Quando repensar a chave: se o objeto candidato a chave é mutável e você não controla isso, prefira derivar uma chave imutável dele (um ID, um código) em vez de usá-lo inteiro; e se a comparação de igualdade da chave é cara (objetos grandes), use uma chave natural mais barata.

### Valor

O valor é a parte da entrada que o `Map` armazena e devolve — a informação útil que você quer alcançar a partir da chave. Ao contrário da chave, o valor não tem exigência nenhuma: pode se repetir entre entradas diferentes (dois funcionários com o mesmo salário, dois países com a mesma capital fictícia), pode ser `null` num `HashMap`, e não precisa de `equals` nem `hashCode` bem-feitos para o mapa funcionar — só precisa deles se você for chamar `containsValue` ou comparar valores por conta própria. O valor também pode ser um objeto composto: uma lista, outro mapa, um `record` com vários campos.

O ponto de atenção ao lidar com valores é o `null` ambíguo. `map.get(k)` devolve `null` tanto quando a chave não existe quanto quando ela existe e está mapeada para `null`. Sem cuidado, o código confunde "não há entrada" com "há entrada vazia" e toma o caminho errado.

A forma de resolver isso é usar os métodos que tornam a intenção explícita: `getOrDefault(k, padrao)` devolve um valor de fallback quando a chave falta; `containsKey(k)` separa os dois casos de `null`; `computeIfAbsent(k, f)` cria e insere o valor só na primeira vez que a chave é vista, ideal para mapas cujo valor é uma coleção que vai crescendo; `merge(k, v, f)` combina o valor novo com o que já estava lá. Como regra geral, evite guardar `null` como valor — um `Optional`, uma lista vazia ou um objeto "nulo" do domínio comunicam melhor.

```java
// valor composto: cada chave aponta para uma lista que cresce
Map<String, List<String>> alunosPorTurma = new HashMap<>();
alunosPorTurma.computeIfAbsent("3A", k -> new ArrayList<>()).add("Bruno");
alunosPorTurma.computeIfAbsent("3A", k -> new ArrayList<>()).add("Carla");
// "3A" -> ["Bruno", "Carla"]

// valores podem repetir à vontade
Map<String, String> capital = new HashMap<>();
capital.put("Brasil", "Brasília");
capital.put("Austrália", "Camberra");
capital.put("EUA", "Washington");

int idade = mapaIdades.getOrDefault("visitante", 0);  // sem NullPointerException
```

Pense de novo na lista telefônica: o número de telefone é o valor. Nada impede que duas pessoas dividam o mesmo número, e o número em si não precisa ser "único" nem "comparável" — ele só precisa estar lá para ser lido quando você procura pelo nome.

Alternativas quanto ao formato do valor: quando o valor pode faltar de forma legítima, `Map<K, Optional<V>>` ou simplesmente ausência da chave é mais honesto que `null`; quando você precisa de vários dados por chave, um `record` como valor é mais claro que um `Object[]` ou uma `String` concatenada; quando o valor é uma coleção, considere `Map<K, List<V>>` (um multimap manual) ou a `Multimap` do Guava.

Quando reconsiderar: se você se pega procurando "qual chave tem tal valor" com frequência, o `Map` está no sentido errado — talvez precise de um índice inverso, um segundo mapa `V -> K`; e se todo valor é sempre o mesmo objeto de enfeite, o que você quer é um `Set` de chaves, não um `Map`.

### `entrySet`

`entrySet()` é o método de `Map` que devolve um `Set<Map.Entry<K, V>>` — uma visão do mapa como um conjunto de entradas, em que cada `Map.Entry` é um objeto que segura uma chave e o valor correspondente, acessíveis por `getKey()` e `getValue()`. É a forma canônica de percorrer um `Map` quando o laço precisa da chave e do valor ao mesmo tempo. Ao lado dele existem `keySet()`, que devolve só as chaves, e `values()`, que devolve só os valores; os três são *visões*, não cópias — refletem o mapa em tempo real e remover um elemento da visão remove a entrada do mapa.

O problema que `entrySet` resolve aparece quando você itera com `keySet()` e, dentro do laço, chama `map.get(chave)` para obter o valor. Isso faz uma segunda busca por hash a cada volta — trabalho duplicado e desnecessário, já que a entrada inteira já estava à mão. Com mapas grandes e laços quentes, a diferença é mensurável; e o código fica mais verboso do que precisa.

`entrySet` resolve entregando chave e valor juntos, num único passo de iteração, sem nenhum `get` extra. O `for-each` sobre `entrySet()` é o idioma padrão. A partir do Java 8, `map.forEach((k, v) -> ...)` faz o mesmo de forma ainda mais enxuta quando você não precisa interromper o laço. Dentro de um `for-each` sobre `entrySet()`, também é seguro remover a entrada atual com `iterator.remove()`, e alguns tipos de `Entry` permitem `setValue(novoValor)` para alterar o mapa durante a iteração.

```java
Map<String, Integer> pontuacao = new HashMap<>();
pontuacao.put("Ana", 42);
pontuacao.put("Bia", 37);
pontuacao.put("Caio", 55);

// idioma padrão: chave e valor de uma vez
for (Map.Entry<String, Integer> e : pontuacao.entrySet()) {
    System.out.println(e.getKey() + " fez " + e.getValue() + " pontos");
}

// equivalente conciso, sem controle de fluxo
pontuacao.forEach((nome, pts) -> System.out.println(nome + ": " + pts));

// remover durante a iteração, com segurança
Iterator<Map.Entry<String, Integer>> it = pontuacao.entrySet().iterator();
while (it.hasNext()) {
    if (it.next().getValue() < 40) it.remove();   // tira quem fez menos de 40
}
```

É a diferença entre receber uma planilha com as colunas "nome" e "nota" lado a lado e receber só a coluna "nome", tendo que consultar outro documento para descobrir a nota de cada um. `entrySet` já traz as duas colunas na mesma linha.

As alternativas: `keySet()` quando o laço só usa as chaves; `values()` quando só interessa somar ou filtrar os valores, sem saber de quem são; `forEach` quando o corpo é uma ação simples sem `break` nem `return`; e `map.entrySet().stream()` quando você quer filtrar, mapear ou agrupar as entradas com a API de streams.

Quando não usar `entrySet`: quando você precisa apenas das chaves ou apenas dos valores, as visões específicas dizem melhor a intenção; quando vai modificar a estrutura do mapa (adicionar ou remover chaves) fora do `iterator.remove`, iterar e mexer ao mesmo tempo lança `ConcurrentModificationException` — colete o que mudar e aplique depois; e para um mapa de pouquíssimas entradas com chaves conhecidas, acessar cada uma pelo nome é mais legível que um laço.

---

## Queue e Deque

`List`, `Set` e `Map` cobriram três formas de organizar dados: por posição, por unicidade e por chave. Falta a família que organiza os elementos pela ordem em que saem da coleção — as filas. `Queue` e `Deque` são as interfaces do Collection Framework feitas para inserir de um lado e retirar do outro, um padrão que aparece toda vez que existe trabalho pendente esperando para ser processado: tarefas em espera, mensagens a entregar, eventos a tratar, requisições numa fila de atendimento.

Este capítulo apresenta a interface `Queue`, que modela a fila clássica (o primeiro a entrar é o primeiro a sair), e sua extensão `Deque`, a fila de duas pontas, que também sabe se comportar como pilha. Em seguida vêm as duas implementações que você vai usar na prática: `ArrayDeque`, a escolha padrão para fila e para pilha, e `PriorityQueue`, que troca a ordem de chegada pela ordem de prioridade.

### `Queue`

`Queue<E>` é a interface do Collection Framework que representa uma fila: uma coleção em que os elementos entram por uma ponta e saem pela outra, normalmente na ordem FIFO (*first in, first out*) — o primeiro a entrar é o primeiro a sair, como numa fila de banco. Ela estende `Collection`, então também dá para percorrer e consultar o tamanho, mas a intenção é outra: você quase sempre mexe só nas pontas. As implementações mais comuns são `LinkedList`, `ArrayDeque` e `PriorityQueue`.

A `Queue` define seis operações, em três pares. Cada par faz a mesma coisa, mas reage de formas diferentes quando a operação falha. Para inserir: `add(e)` lança exceção se não couber, `offer(e)` devolve `false`. Para remover a cabeça: `remove()` lança exceção se a fila estiver vazia, `poll()` devolve `null`. Para espiar a cabeça sem remover: `element()` lança exceção, `peek()` devolve `null`. Na maioria dos casos você quer a versão que devolve `null` ou `false`, porque fila vazia é uma situação normal, não um erro.

Sem uma interface de fila, o jeito improvisado é usar uma `List`: `add` no fim e `remove(0)` no começo. Com `ArrayList` isso é caro — remover a posição 0 empurra todos os outros elementos uma casa para trás, uma operação O(n) a cada retirada. Com `LinkedList` o custo some, mas o tipo `List` no código não comunica que aquilo é uma fila, e nada impede alguém de chamar `get(50)` ou `add(3, x)` e quebrar a semântica.

Declarar a variável como `Queue` resolve os dois problemas: você ganha uma API pequena e clara, e o compilador passa a barrar acesso por índice.

```java
Queue<String> impressao = new LinkedList<>();
impressao.offer("relatorio.pdf");
impressao.offer("contrato.docx");
impressao.offer("foto.png");

while (!impressao.isEmpty()) {
    String arquivo = impressao.poll();   // sempre o mais antigo
    System.out.println("imprimindo " + arquivo);
}
// se a fila esvaziar antes, poll() devolve null em vez de estourar
```

Pense na fila do caixa do supermercado: quem chega primeiro é atendido primeiro, ninguém "pula" para o meio, e você só olha para as duas pontas — quem está sendo atendido e quem acabou de chegar.

As alternativas dependem do que você precisa além do FIFO simples. `Deque` quando quiser inserir ou remover nas duas pontas. `PriorityQueue` quando a saída deve seguir prioridade, não ordem de chegada. `BlockingQueue` (do pacote `java.util.concurrent`) quando várias threads produzem e consomem a mesma fila e uma precisa esperar a outra.

Quando não usar `Queue`: quando o código precisa de acesso aleatório ou de percorrer por índice — aí o tipo certo é `List`; e quando não há nenhuma noção de ordem de processamento, só um conjunto de elementos, caso em que `Set` ou `List` dizem melhor a intenção.

### `Deque`

`Deque<E>` (lê-se "deque", de *double-ended queue*, fila de duas pontas) é a interface que estende `Queue` e permite inserir, remover e espiar elementos nas **duas** extremidades. Onde a `Queue` só mexe na cabeça e insere na cauda, a `Deque` oferece o conjunto completo: `addFirst`/`addLast`, `offerFirst`/`offerLast`, `pollFirst`/`pollLast`, `peekFirst`/`peekLast`, e assim por diante. Com isso, uma mesma estrutura sabe se comportar como fila (FIFO: insere numa ponta, tira da outra) e como pilha (LIFO, *last in, first out*: insere e tira da mesma ponta).

O problema que a `Deque` resolve tem duas faces. A primeira é a pilha: até o Java 6, a classe para isso era `Stack`, que herda de `Vector`. Isso traz dois defeitos — todos os métodos são `synchronized`, o que custa desempenho mesmo num programa de uma thread só, e, por ser uma `List`, `Stack` permite inserir no meio, `get` por índice e outras operações que não fazem sentido numa pilha. A própria documentação da `Stack` hoje recomenda usar `Deque` no lugar. A segunda face é a fila de duas pontas em si: sem `Deque`, adicionar no início de uma coleção sequencial ou é impossível ou é O(n).

`Deque` resolve com uma API simétrica e sem sincronização embutida. Para usar como pilha, ela ainda oferece os nomes tradicionais `push`, `pop` e `peek`, que operam todos na primeira ponta. As implementações padrão são `ArrayDeque` (recomendada) e `LinkedList`.

```java
// como pilha: histórico de navegação com "voltar"
Deque<String> historico = new ArrayDeque<>();
historico.push("home");
historico.push("produtos");
historico.push("produto/42");

String atual = historico.pop();       // "produto/42" — sai o último que entrou
String anterior = historico.peek();   // "produtos" — sem remover

// como fila de duas pontas: inserir uma tarefa urgente na frente
Deque<String> tarefas = new ArrayDeque<>();
tarefas.addLast("backup");
tarefas.addLast("relatorio");
tarefas.addFirst("incidente-critico");   // fura a fila, legitimamente
String proxima = tarefas.pollFirst();    // "incidente-critico"
```

Uma analogia para a pilha é uma pilha de pratos: você põe e tira sempre do topo, e o último prato colocado é o primeiro a ser usado. Para a fila de duas pontas, pense num vagão de metrô com portas nas duas extremidades — pessoas podem entrar e sair por qualquer lado, conforme a necessidade. Em código, isso é útil quando a maioria dos itens entra pelo fim, mas alguns casos excepcionais precisam ser tratados antes de todo o resto.

As alternativas: `Queue` como tipo declarado quando você só precisa de FIFO puro, para não expor operações que não vai usar; `Stack` só em código legado que já a utiliza; `ConcurrentLinkedDeque` quando várias threads compartilham a estrutura.

Quando não usar `Deque`: quando o acesso por índice ou a iteração posicional são centrais (é `List`); e quando o problema é estritamente FIFO — declarar `Queue` deixa a intenção mais estreita e clara, mesmo que a implementação concreta seja um `ArrayDeque`.

### `ArrayDeque`

`ArrayDeque<E>` é a implementação de `Deque` (e, portanto, também de `Queue`) construída sobre um **array circular redimensionável**. Um array circular é um vetor comum em que os índices "dão a volta": quando a cauda chega ao fim do array, ela continua na posição 0, desde que haja espaço livre lá. Isso permite adicionar e remover nas duas pontas em tempo O(1) amortizado, sem nunca precisar deslocar os outros elementos. Quando o array enche, ele dobra de tamanho, exatamente como o `ArrayList`. É a implementação recomendada tanto para filas quanto para pilhas.

O problema que ele resolve é de eficiência frente às alternativas. `LinkedList` também implementa `Deque`, mas guarda cada elemento em um nó separado, com dois ponteiros; isso gasta bem mais memória por elemento e espalha os dados pela heap, o que provoca *cache misses* e deixa a iteração mais lenta. `Stack`, além de ser uma `List` mal-comportada, é sincronizada. `ArrayDeque` fica no meio-termo ideal: contíguo na memória como um array, mas com inserção e remoção baratas nas duas pontas.

Há duas restrições importantes. Primeira: `ArrayDeque` **não aceita `null`**. O valor `null` é usado internamente como sinal de "não há elemento aqui", então `poll()` e `peek()` devolvem `null` justamente para indicar fila vazia — permitir um `null` de verdade tornaria isso ambíguo. Segunda: ele **não é thread-safe**; para uso concorrente, existem `ConcurrentLinkedDeque` e `ArrayBlockingQueue`.

```java
// percorrer uma árvore de diretórios sem recursão, usando ArrayDeque como pilha
Deque<Path> pilha = new ArrayDeque<>();
pilha.push(raiz);

while (!pilha.isEmpty()) {
    Path atual = pilha.pop();
    System.out.println(atual);
    if (Files.isDirectory(atual)) {
        try (Stream<Path> filhos = Files.list(atual)) {
            filhos.forEach(pilha::push);   // empilha os filhos para visitar depois
        }
    }
}
```

Pense no `ArrayDeque` como uma esteira transportadora curva que liga as duas pontas de uma bancada: você coloca e retira caixas de qualquer um dos lados, e a esteira simplesmente reaproveita o trecho que ficou vago do outro lado, sem ninguém ter que empurrar a fila inteira.

Outras implementações entram conforme a necessidade extra: `LinkedList` quando você precisa de `Deque` **e** de operações de `List` (inserir no meio, acesso por índice) na mesma estrutura; `PriorityQueue` quando a saída deve seguir prioridade; `ArrayBlockingQueue` ou `ConcurrentLinkedDeque` em cenários com várias threads.

Há três situações em que `ArrayDeque` não serve: código multithread sem sincronização externa; necessidade real de armazenar `null` como valor legítimo; e padrão de acesso por índice, caso em que `ArrayList` é a escolha certa.

### `PriorityQueue`

`PriorityQueue<E>` é a implementação de `Queue` em que a ordem de saída **não** é a ordem de chegada, e sim a ordem de prioridade dos elementos. `poll()` e `peek()` sempre devolvem o menor elemento segundo um critério de comparação: ou a ordem natural do tipo (se ele implementa `Comparable`), ou um `Comparator` que você passa no construtor. Por baixo, ela é um *heap binário* — uma árvore quase completa guardada num array, em que todo pai é menor ou igual aos filhos. Essa estrutura mantém o menor elemento sempre acessível na raiz, sem manter a coleção inteira ordenada.

Sem `PriorityQueue`, para "processar sempre o item mais urgente primeiro" você teria duas opções ruins. Manter uma `List` ordenada e inserir cada novo elemento na posição certa custa O(n) por inserção. Ou inserir tudo no fim e ordenar a lista antes de cada retirada, o que é O(n log n) repetido. O heap resolve os dois lados: `offer()` custa O(log n), `poll()` custa O(log n) e `peek()` custa O(1).

Duas características surpreendem quem está começando. A primeira: a `PriorityQueue` **não é ordenada quando você a percorre**. Só a cabeça tem posição garantida; um `for-each` ou o `toString()` mostram os elementos na ordem interna do heap, que parece embaralhada. Para obter todos em ordem, é preciso ir chamando `poll()` até esvaziar. A segunda: ela não aceita `null`, e todos os elementos precisam ser comparáveis entre si, senão o `offer` lança `ClassCastException`.

```java
// atendimento por prioridade: número menor = mais urgente
record Chamado(int prioridade, String descricao) {}

PriorityQueue<Chamado> fila =
    new PriorityQueue<>(Comparator.comparingInt(Chamado::prioridade));

fila.offer(new Chamado(3, "impressora sem toner"));
fila.offer(new Chamado(1, "servidor fora do ar"));
fila.offer(new Chamado(2, "e-mail lento"));

while (!fila.isEmpty()) {
    System.out.println(fila.poll().descricao());
}
// imprime: servidor fora do ar, e-mail lento, impressora sem toner

// para um "max-heap" (maior primeiro), inverta o comparador:
PriorityQueue<Integer> maiores = new PriorityQueue<>(Comparator.reverseOrder());
```

A analogia é o pronto-socorro: os pacientes não são atendidos pela ordem de chegada, e sim pela gravidade — quem chega com um caso grave passa na frente de quem chegou antes com um caso leve. A recepção (o heap) não precisa manter a sala inteira ordenada; basta saber, a cada momento, quem é o próximo a entrar.

As alternativas: `TreeSet` quando não pode haver elementos duplicados e você precisa percorrer **todos** em ordem com frequência; uma `List` com `Collections.sort` quando você insere tudo de uma vez e depois só lê, sem mais inserções; `PriorityBlockingQueue` para uso concorrente.

Quando não usar `PriorityQueue`: quando a ordem de chegada é o que importa — aí é uma `Queue` comum sobre `ArrayDeque`; quando você precisa iterar o conteúdo inteiro em ordem a toda hora, caso em que uma estrutura sempre ordenada compensa mais; e quando há poucos elementos e a diferença de desempenho é irrelevante, situação em que ordenar uma lista simples é mais legível.

---

# Módulo 6 — Números e precisão

Números são centrais à programação. Java oferece tipos primitivos (`int`, `double`) que resolvem a maioria dos casos, mas para valores muito grandes, para precisão decimal exata e para cálculos monetários, existem classes como `BigInteger` e `BigDecimal`. Este módulo explora a classe `Math`, os detalhes da representação de ponto flutuante, e como usar classes de precisão arbitrária de forma segura.

## `Math`

Os tipos primitivos `int` e `double` sabem somar, subtrair, multiplicar e dividir, mas a linguagem não traz operador para elevar a uma potência, extrair uma raiz, calcular um seno ou sortear um número. Essas operações moram na classe `java.lang.Math`, um conjunto de métodos `static` que acompanha qualquer programa Java sem precisar de `import`. Ela funciona como uma calculadora científica embutida: você chama `Math.sqrt(2)` do mesmo jeito que apertaria a tecla da raiz quadrada, e recebe o resultado como um `double`.

Este capítulo percorre os grupos de operações que mais aparecem no dia a dia: potências e logaritmos, raízes, as três formas de arredondar um valor fracionário para um inteiro, as funções trigonométricas e seus cuidados com radianos, e por fim `Math.random`, o gerador de números aleatórios mais simples da plataforma. O foco é prático — como chamar cada método, o que ele devolve e onde ele engana. Os detalhes de por que um `double` às vezes não representa o número exato ficam para o próximo capítulo.

### Potências

Elevar um número a um expoente é a operação `Math.pow(base, expoente)`. Ela recebe dois `double` e devolve um `double`: `Math.pow(2, 10)` resulta em `1024.0`, `Math.pow(5, 3)` em `125.0`, `Math.pow(2, 0.5)` em `1.4142...` (ou seja, a raiz quadrada de 2). O expoente pode ser fracionário ou negativo — `Math.pow(2, -1)` dá `0.5` —, o que torna `pow` uma ferramenta geral para qualquer expressão do tipo "base elevado a algo". No mesmo grupo estão `Math.exp(x)`, que calcula o número de Euler elevado a `x`, e os logaritmos `Math.log(x)` (base natural) e `Math.log10(x)` (base 10), que são a operação inversa da potência.

Sem esses métodos, elevar a uma potência inteira exigiria escrever o laço à mão: multiplicar a base por ela mesma `n` vezes dentro de um `for`. Funciona para expoentes inteiros e pequenos, mas não cobre expoente fracionário nem negativo, e espalha pelo código um trecho repetitivo que é fácil errar (começar o acumulador em `0` em vez de `1`, contar uma iteração a mais). Já um logaritmo ou uma exponencial de base `e` seria inviável de implementar sem uma biblioteca matemática.

`Math.pow` resolve tudo isso com uma única chamada, mas cobra um preço que precisa ser conhecido: o resultado é sempre `double`, então operações que "deveriam" dar um inteiro exato podem devolver algo como `124.99999999999999`. Quando o expoente é um inteiro conhecido e pequeno, muitas vezes é mais claro e mais seguro escrever a multiplicação direta: `x * x` em vez de `Math.pow(x, 2)`. E quando a base e o expoente são inteiros e o resultado pode estourar o alcance de `long` — como em `2` elevado a `200` —, a ferramenta certa é `BigInteger`, que tem o método `pow(int)` e trabalha com inteiros de tamanho arbitrário sem perder um único dígito.

```java
double area   = Math.pow(raio, 2) * Math.PI;   // área do círculo
double montante = capital * Math.pow(1 + taxa, meses); // juros compostos
double lado   = Math.pow(volumeCubo, 1.0 / 3);  // aresta a partir do volume

BigInteger enorme = BigInteger.TWO.pow(200);    // 2^200 exato, sem overflow
```

Uma analogia: `Math.pow` é a tecla `x^y` da calculadora científica. Ela aceita qualquer par de números no visor e sempre responde em ponto flutuante — inclusive quando você digitou dois inteiros e esperava um inteiro de volta. Para contas de bolso simples, como um número ao quadrado, escrever a multiplicação continua sendo mais rápido do que procurar a tecla; para expoentes grandes e exatos, a calculadora comum não serve e é preciso a de precisão arbitrária. No código, essas três situações correspondem a `Math.pow`, à multiplicação direta e a `BigInteger.pow`.

Não use `Math.pow` dentro de laços muito quentes quando um `x * x` resolve: a chamada de função e a conversão para `double` custam mais do que a multiplicação. E evite depender da igualdade exata do resultado (`Math.pow(10, 2) == 100`) — mesmo quando funciona hoje, comparar `double` por `==` é frágil, assunto retomado no capítulo de ponto flutuante.

### Raízes

A raiz quadrada tem método próprio: `Math.sqrt(x)` devolve o `double` cujo quadrado é `x`. `Math.sqrt(16)` dá `4.0`, `Math.sqrt(2)` dá `1.4142135623730951`. Para a raiz cúbica existe `Math.cbrt(x)` — `Math.cbrt(27)` resulta em `3.0`. Qualquer outra raiz se obtém como caso particular da potência: a raiz n-ésima de `x` é `x` elevado a `1.0/n`, ou seja `Math.pow(x, 1.0 / n)`. O detalhe do `1.0` é importante: escrever `1 / 4` em Java é divisão inteira e dá `0`, o que faria `Math.pow(x, 0)` devolver sempre `1`.

Sem um método dedicado, calcular uma raiz quadrada na mão significa implementar um algoritmo iterativo de aproximação (o método de Newton, por exemplo): chutar um valor, refiná-lo repetidamente até a diferença ficar pequena o bastante, e decidir quando parar. É código delicado, com risco de laço infinito ou de precisão insuficiente, para resolver um problema que a plataforma já resolve de forma otimizada.

`Math.sqrt` e `Math.cbrt` entregam o resultado direto e rápido, apoiados em instruções da própria CPU. O ponto de atenção fica com números negativos: não existe raiz quadrada real de um número negativo, então `Math.sqrt(-1)` não lança exceção — devolve o valor especial `NaN` (*Not a Number*). Esse `NaN` se propaga silenciosamente por todas as contas seguintes (qualquer operação com `NaN` dá `NaN`) e tem a peculiaridade de não ser igual a nada, nem a si mesmo, de modo que `resultado == Double.NaN` é sempre falso; a checagem correta é `Double.isNaN(resultado)`. `Math.cbrt`, por outro lado, aceita negativos normalmente, porque a raiz cúbica de um número negativo é real: `Math.cbrt(-8)` dá `-2.0`.

```java
double hipotenusa = Math.sqrt(cateto1 * cateto1 + cateto2 * cateto2);
double desvioPadrao = Math.sqrt(variancia);
double raizQuinta = Math.pow(x, 1.0 / 5);       // raiz quinta de x

double r = Math.sqrt(valor);
if (Double.isNaN(r)) {
    throw new IllegalArgumentException("valor negativo: " + valor);
}
```

A analogia é a mesma da calculadora: `Math.sqrt` é a tecla do radical, `Math.cbrt` é a raiz cúbica, e as demais raízes você monta usando a tecla de potência com um expoente fracionário. A diferença é que a calculadora costuma mostrar "erro" ao pedir a raiz de um negativo, enquanto o Java segue em frente com `NaN` no visor e só revela o problema lá na frente. Por isso, quando a entrada pode ser negativa, vale validar antes de chamar `Math.sqrt`, amarrando a verificação ao ponto onde o valor entra.

Uma alternativa aparece quando você precisa apenas saber se um inteiro é quadrado perfeito, ou trabalhar com raízes de números gigantescos: `BigInteger` tem `sqrt()` desde o Java 9, que devolve a parte inteira da raiz sem passar por `double` e sem perder precisão. Para todo o resto — geometria, estatística, física —, `Math.sqrt` é a escolha padrão.

### Arredondamento

Transformar um valor fracionário em um valor "redondo" tem três respostas diferentes em `Math`, e escolher a errada muda o resultado. `Math.round(x)` arredonda para o inteiro mais próximo, com o critério de que o meio (`.5`) sobe: `Math.round(2.4)` dá `2`, `Math.round(2.5)` dá `3`, `Math.round(2.6)` dá `3`. `Math.floor(x)` sempre desce para o inteiro imediatamente abaixo ou igual: `Math.floor(2.9)` dá `2.0`. `Math.ceil(x)` sempre sobe: `Math.ceil(2.1)` dá `3.0`. Há ainda `Math.rint(x)`, parecido com `round` mas que, no empate do `.5`, vai para o inteiro par mais próximo — critério usado em estatística para não enviesar somas.

Os tipos de retorno diferem e isso pega quem está começando. `Math.round(double)` devolve `long` (e `Math.round(float)` devolve `int`), já pronto para usar como índice ou contador. Já `floor`, `ceil` e `rint` devolvem `double` — `Math.floor(2.9)` é `2.0`, não `2` —, então costuma ser necessário um cast: `(int) Math.floor(x)`. Um detalhe extra é o comportamento com negativos: `Math.floor(-2.1)` é `-3.0` (o inteiro abaixo), enquanto o simples `(int) -2.1` trunca para `-2` (joga a parte fracionária fora, aproximando de zero). Truncar com cast e "arredondar para baixo" só coincidem para números positivos.

Sem esses métodos, arredondar significaria manipular a parte fracionária na mão — somar `0.5` e truncar com cast para simular o `round`, o que erra com negativos e com valores muito grandes. É código sutil, fácil de errar em um caso de borda e difícil de ler depois.

```java
int paginas       = (int) Math.ceil(totalItens / (double) itensPorPagina); // sempre arredonda para cima
long notaFinal    = Math.round(media);                                      // 7.5 vira 8
int degrausInteiros = (int) Math.floor(altura / alturaDegrau);              // quantos degraus cabem
```

Pense numa sala: `floor` e `ceil` são o piso e o teto — dado um ponto qualquer no ar, `floor` é o piso logo abaixo e `ceil` é o teto logo acima. `round` é perguntar "de qual dos dois estou mais perto?". A imagem ajuda a lembrar dos nomes, mas a decisão real é de negócio: número de páginas para exibir tudo pede `ceil`; quantas caixas cheias dá para montar pede `floor`; converter uma média em conceito pede `round`.

A alternativa importante entra quando o arredondamento não é para inteiro, mas para um número de casas decimais — preço com dois dígitos, percentual com uma casa. Aí `Math` não serve: multiplicar por `100`, arredondar e dividir de volta reintroduz erro de ponto flutuante. O caminho correto é `BigDecimal` com um `RoundingMode` explícito, tema dos capítulos finais deste módulo. Para dinheiro, em particular, nunca use `Math.round` sobre `double`.

### Trigonometria

As funções trigonométricas básicas estão em `Math.sin(x)`, `Math.cos(x)` e `Math.tan(x)`, com as inversas `Math.asin`, `Math.acos` e `Math.atan`. Todas recebem e devolvem `double`. O ponto que mais causa erro é a unidade do ângulo: essas funções trabalham em **radianos**, não em graus. `Math.sin(90)` não é `1` — é o seno de 90 radianos, um valor aparentemente aleatório. O seno de 90 graus se calcula como `Math.sin(Math.toRadians(90))`, que dá `1.0`. Os métodos `Math.toRadians(graus)` e `Math.toDegrees(radianos)` fazem a ponte entre as duas unidades, e a constante `Math.PI` está disponível para as contas de conversão feitas à mão (180 graus = `Math.PI` radianos).

Há também `Math.atan2(y, x)`, que merece destaque próprio: dado um ponto `(x, y)`, ela devolve o ângulo do vetor que vai da origem até esse ponto, no intervalo de `-π` a `π`, já resolvendo corretamente em qual dos quatro quadrantes o ponto está. O `atan` comum recebe só a razão `y/x` e não consegue distinguir, por exemplo, o primeiro quadrante do terceiro. Sempre que o objetivo é "descobrir a direção de um ponto", `atan2` é a função certa.

Sem esses métodos, seria preciso aproximar seno e cosseno por séries matemáticas — somar muitos termos de uma expansão até a precisão bastar. É exatamente o tipo de rotina numérica que uma biblioteca-padrão existe para poupar.

```java
double anguloGraus = 30;
double rad = Math.toRadians(anguloGraus);
double deslocX = distancia * Math.cos(rad);   // projeção horizontal
double deslocY = distancia * Math.sin(rad);   // projeção vertical

// direção (em graus) de um alvo em relação ao jogador
double dx = alvoX - jogadorX;
double dy = alvoY - jogadorY;
double direcao = Math.toDegrees(Math.atan2(dy, dx));
```

Uma analogia útil: pense num relógio de ponteiros. O ângulo pode ser dito em "horas no mostrador" ou em graus, e misturar as duas linguagens dá resultado sem sentido — pedir "o ponteiro na posição 90" quando o mostrador só vai até 12. Radianos e graus são essas duas linguagens; `Math.toRadians` é o tradutor que você precisa aplicar antes de conversar com `Math.sin` e companhia, e a explicação técnica é que a biblioteca padronizou tudo em radianos porque é a unidade natural do cálculo.

As alternativas ficam fora do escopo comum: bibliotecas como Apache Commons Math trazem funções hiperbólicas e vetoriais mais completas, e engines gráficas costumam ter suas próprias versões otimizadas ou tabeladas. Para geometria de tela, física simples de jogos e qualquer conta de ângulo do dia a dia, `Math` basta. Quando não há ângulo nenhum envolvido — e é comum tentar usar trigonometria onde uma conta de proporção resolveria —, o melhor é não trazer `sin` e `cos` para a jogada.

### `Math.random`

`Math.random()` devolve um `double` pseudoaleatório maior ou igual a `0.0` e menor que `1.0`. É a forma mais curta de obter aleatoriedade em Java: não exige criar objeto nem `import`. Para transformar esse valor num intervalo útil, multiplica-se pela largura da faixa e, quando se quer um inteiro, aplica-se `(int)` para truncar. Um número de `0` a `99` é `(int) (Math.random() * 100)`; um de `1` a `6`, como num dado, é `(int) (Math.random() * 6) + 1`.

Sem esse método, seria preciso instanciar `new java.util.Random()` e chamar `nextInt`, `nextDouble` e afins — mais verboso para um sorteio único e rápido. `Math.random()` existe justamente como atalho: internamente, na primeira chamada, ele cria um `Random` compartilhado e guarda para as próximas.

Esse compartilhamento é também a sua principal limitação. Como todas as chamadas usam o mesmo gerador interno, não há como fixar uma semente para reproduzir a mesma sequência — algo essencial em testes e simulações, onde você quer rodar de novo exatamente o mesmo "acaso". Além disso, em programas com muitas threads sorteando ao mesmo tempo, esse gerador único vira ponto de disputa e perde desempenho. E o número não é criptograficamente seguro: dá para prever a sequência conhecendo o estado interno, então nunca use `Math.random()` para gerar senhas, tokens ou chaves.

```java
int face = (int) (Math.random() * 6) + 1;              // dado de 1 a 6
boolean caraOuCoroa = Math.random() < 0.5;             // metade das vezes true
double ruido = (Math.random() * 2 - 1) * amplitude;    // valor entre -amplitude e +amplitude

// sortear um elemento de uma lista
String escolhido = nomes.get((int) (Math.random() * nomes.size()));
```

Vale a imagem do dado de plástico que já vem dentro do jogo de tabuleiro: é o que `Math.random()` oferece. Serve para uma partida casual, mas se você precisa que duas partidas caiam nos mesmos números para conferir uma jogada, ou que vários jogadores rolem dados ao mesmo tempo sem esbarrar na mesma mão, ou que ninguém consiga prever o próximo lançamento numa aposta séria, você troca por um dado próprio e controlado. No código, esses "dados próprios" são as alternativas: `new Random(semente)` quando a reprodutibilidade importa, `ThreadLocalRandom.current()` para código concorrente (cada thread com seu gerador), e `SecureRandom` quando o valor precisa ser imprevisível por segurança.

Quando não usar `Math.random()`: em testes que dependem de resultado determinístico, em qualquer contexto multithread com volume alto de sorteios, e em toda geração de material sensível. Fora isso — um exemplo didático, um embaralhamento simples, um efeito visual aleatório —, ele é a opção mais direta e legível.


## Representação de ponto flutuante

O capítulo anterior usou `double` à vontade e adiou uma pergunta incômoda: por que `Math.pow(10, 2)` às vezes não dá exatamente `100`, ou por que somar dez vezes `0.1` não fecha em `1.0`. A resposta está em como o computador guarda números fracionários na memória, e é isso que este capítulo abre.

Primeiro vem o padrão que define o formato — IEEE-754 — e como os 64 bits de um `double` são repartidos. Depois, o que "precisão" significa nesse contexto e onde ela termina. Em seguida, por que números decimais simples como `0.1` não cabem exatos em binário e produzem pequenos erros de representação. E, por fim, a consequência prática que mais aparece no dia a dia: nunca comparar dois `double` com `==`. Toda a explicação fica no nível de entender o mecanismo e se defender dele; aritmética decimal exata — para dinheiro, por exemplo — é assunto de `BigDecimal`, nos capítulos seguintes deste módulo.

### IEEE-754

IEEE-754 é o padrão internacional que define como computadores armazenam e operam números reais (fracionários) em binário. Os dois tipos de ponto flutuante do Java seguem esse padrão à risca: `float` é o formato de 32 bits (chamado *binary32*) e `double` é o de 64 bits (*binary64*). Sempre que o capítulo anterior devolveu um `double` de `Math.sqrt` ou `Math.pow`, era um número no formato IEEE-754 que estava sendo produzido.

A ideia central é guardar o número em notação científica, só que na base 2. Da mesma forma que se escreve `6.022 × 10^23` separando os dígitos significativos da ordem de grandeza, o IEEE-754 quebra os 64 bits de um `double` em três campos: 1 bit de sinal, 11 bits de expoente e 52 bits de fração (a mantissa). O valor representado é, aproximadamente, `(-1)^sinal × 1,fração × 2^(expoente − 1023)`. É esse arranjo que permite a um `double` cobrir uma faixa enorme — de cerca de `10^-308` a `10^308` — com apenas 64 bits: o expoente compra alcance, a mantissa compra precisão, e os dois campos disputam um orçamento fixo de bits.

Sem um padrão, cada fabricante de processador tinha o seu próprio formato de número fracionário. O mesmo programa produzia resultados diferentes em máquinas diferentes, e conceitos como "infinito" e "não é número" eram tratados de forma inconsistente ou simplesmente não existiam. Portar código numérico de um computador para outro era imprevisível.

O IEEE-754 resolveu isso definindo um formato único, que todos implementam, incluindo valores especiais que aparecem quando uma conta escapa do normal: `+Infinity` e `-Infinity` (de overflow ou de `1.0 / 0.0`), `NaN` — *Not a Number* — (de `0.0 / 0.0` ou `Math.sqrt(-1)`) e até um zero com sinal (`+0.0` e `-0.0`). O Java expõe esses valores como `Double.POSITIVE_INFINITY`, `Double.NaN` e afins. O modo de arredondamento também é padronizado: por padrão, "arredonda para o mais próximo, empate para o par".

```java
System.out.println(0.1 + 0.2);         // 0.30000000000000004
System.out.println(1.0 / 0.0);         // Infinity
System.out.println(0.0 / 0.0);         // NaN
System.out.println(Double.MAX_VALUE);  // 1.7976931348623157E308

long bits = Double.doubleToLongBits(1.0);
System.out.println(Long.toBinaryString(bits)); // os 64 bits crus do double 1.0
```

Uma analogia: pense num visor de calculadora que reserva um espaço para os dígitos e outro, menor, para "×10 elevado a quanto". A calculadora nunca mostra todos os dígitos de `1/3` — ela para onde o visor acaba. O IEEE-754 faz o mesmo com um espaço fixo de bits para a mantissa e para o expoente, e é dessa limitação de espaço que nascem os erros dos próximos tópicos. A explicação técnica por trás da analogia é justamente a repartição 1 + 11 + 52.

As alternativas ao ponto flutuante binário existem para casos em que essa aproximação não serve: aritmética de ponto fixo (guardar centavos como `long`, com a vírgula subentendida), `BigDecimal` (base 10, precisão arbitrária) e representações racionais (pares numerador/denominador, em bibliotecas). Todas trocam velocidade por exatidão ou por faixa de valores. Não use `double` — e, portanto, o IEEE-754 — para dinheiro, para qualquer valor que precise ser decimal-exato, para contagens ou quando você depende de igualdade exata entre dois resultados; nesses casos, `long` de centavos ou `BigDecimal` são o caminho.

### Precisão

Precisão, aqui, é quantos dígitos significativos o tipo consegue guardar de forma confiável. Um `double` sustenta cerca de 15 a 17 dígitos decimais significativos; um `float`, apenas 6 ou 7. Esse número não é arbitrário: vem do orçamento de bits da mantissa. Os 52 bits de fração de um `double` equivalem a aproximadamente 15,95 dígitos decimais, e os 23 bits do `float` a pouco menos de 7.

O erro comum é tratar `double` como se ele guardasse infinitos dígitos, e então se surpreender quando `1e16 + 1` resulta no mesmo `1e16` — o `1` que deveria ser somado simplesmente cai fora do fim da mantissa — ou quando uma cadeia longa de somas vai desviando do valor esperado.

O ponto que organiza o entendimento é que a precisão de um `double` é **relativa**, não absoluta. Ele tem sempre uns 16 dígitos significativos, independentemente da magnitude do número. Isso significa que, perto de `1.0`, a distância entre dois números representáveis consecutivos é minúscula (algo como `10^-16`); perto de `10^16`, essa distância já é de cerca de `1,0`; e perto de `10^17`, dois `double` seguidos ficam a 16 unidades um do outro. Essa "distância que cresce com a magnitude" é o modelo mental a carregar — o valor dela chama-se ULP, *unit in the last place*. A analogia é uma régua cujas marcas vão se afastando conforme os números crescem: de perto você mede ao milímetro, longe só ao metro.

```java
double grande = 1e16;
System.out.println(grande + 1 == grande);  // true — o + 1 se perde

System.out.println(123456789f);            // 1.23456792E8 — já erra no 8º dígito

float f = 0.1f + 0.2f;                      // resultado só confiável até ~7 dígitos
```

Entre `float` e `double`, o `float` economiza memória (metade do tamanho) e é útil em vetores gigantescos, como em gráficos e aprendizado de máquina, mas 7 dígitos se esgotam rápido. Por isso o padrão é usar `double`, e por isso um literal como `3.14` já é `double` em Java a menos que você escreva `3.14f`.

Não trate a precisão do `double` como garantia em totais financeiros, em números de identificação longos ou em qualquer situação em que o 16º dígito importe ou em que os erros se acumulem por milhões de operações. Aí entram as alternativas já descritas em IEEE-754 — com a ressalva de que `BigDecimal` ainda aceita uma precisão sob medida via `MathContext` — ou uma reestruturação da conta. Para ciência, engenharia e geometria, onde 15 dígitos sobram, `double` é a escolha certa.

### Erros de representação

Muitos números que são simples e exatos na base 10 não têm representação exata na base 2. O `double` guarda, então, o valor representável mais próximo — ligeiramente diferente do que você escreveu. O caso clássico é o `0.1`: assim como `1/3` vira a dízima `0,333...` em decimal, `0,1`, `0,2` e `0,3` são dízimas infinitas em binário.

O motivo é aritmético. A base 2 só representa de forma exata as frações cujo denominador é uma potência de 2: `0,5`, `0,25`, `0,125`, `0,75` e combinações delas. Mas `0,1` é `1/10`, e 10 tem o fator 5, que não é potência de 2 — logo a fração se repete para sempre em binário. O Java armazena a aproximação de 52 bits mais próxima, que vale cerca de `0,1000000000000000055511151231257827`.

A consequência aparece o tempo todo: `0.1 + 0.2` imprime `0.30000000000000004`; `0.1 * 3` não é `0.3`; somar `0.10` cem vezes não dá exatamente `10.00`. Quem está começando desconfia de um bug no Java — mas é o formato funcionando exatamente como foi projetado.

```java
System.out.println(0.1 + 0.2);          // 0.30000000000000004
System.out.println(0.1 + 0.2 == 0.3);   // false

double total = 0;
for (int i = 0; i < 10; i++) total += 0.1;
System.out.println(total);              // 0.9999999999999999
System.out.printf("%.2f%n", total);     // 1,00  — correção só para exibição
```

Lidar com isso passa por três frentes. A primeira é saber o que é seguro: inteiros até `2^53` e somas ou produtos de frações binárias exatas continuam exatos. A segunda é arredondar na hora de exibir, com `String.format` ou `printf("%.2f", x)`, sem mexer no valor armazenado. A terceira, quando a aritmética em si precisa ser decimal-exata, é migrar para `BigDecimal` construído a partir de uma *string* (`new BigDecimal("0.1")`), nunca a partir de um `double` — porque o `double` já entrou com o erro embutido.

A analogia é escrever `1/3` no papel em decimal: você é obrigado a parar em algum dígito e aceitar que `0,3333` não é exatamente um terço. Cada vez que você soma esses terços truncados, afasta-se mais de `1`. O computador faz o mesmo com décimos em binário; trazendo de volta ao concreto, o erro por operação é pequeno mas real, e pode se acumular numa soma longa.

As alternativas são as já descritas em IEEE-754. O que vale acrescentar aqui é que há casos em que o erro simplesmente não importa: uma medição que já carrega incerteza (um sensor lendo `20.1` °C), posições de elementos na tela, agregados estatísticos. Um desvio no 16º dígito está muito abaixo do ruído desses dados, e puxar `BigDecimal` para lá só deixa o código mais lento e mais verboso sem nenhum ganho.

### Comparação de doubles

Esta é a regra prática que decorre de tudo o que veio antes: não compare dois valores `double` (ou `float`) com `==` e `!=` quando qualquer um deles veio de uma conta. Como os resultados carregam pequenos erros de representação, dois números matematicamente iguais quase sempre diferem no último bit.

O sintoma mais direto é `0.1 + 0.2 == 0.3` retornar `false`. Um laço escrito como `for (double d = 0.0; d != 1.0; d += 0.1)` nunca termina, porque `d` pula de `0.9999999999999999` para um valor logo acima de `1.0` sem nunca ser exatamente `1.0`. E uma verificação inocente como `if (saldo == 0.0)` deixa passar um saldo que, na prática, é zero mas está guardado como `1e-17`.

A comparação correta pergunta se a diferença entre os dois valores cabe dentro de uma tolerância pequena, o *epsilon*: `Math.abs(a - b) < 1e-9`. A escolha do epsilon depende da escala. Uma tolerância absoluta funciona quando as magnitudes são conhecidas e próximas de `1`; quando os valores variam por muitas ordens de grandeza, use uma comparação relativa, dividindo a diferença pelo maior dos dois em módulo. Para `NaN`, lembre que `NaN == NaN` é `false` — a checagem é `Double.isNaN(x)`. Já `Double.compare(a, b)` e `Double.equals` impõem uma ordem total (tratam `NaN` como igual a si mesmo e `-0.0` como distinto de `0.0`), o que serve para ordenar uma lista, não para "perto o suficiente".

```java
double a = 0.1 + 0.2;
double b = 0.3;
System.out.println(a == b);                  // false
System.out.println(Math.abs(a - b) < 1e-9);  // true

// contador de laço: use int, nunca double
for (int i = 0; i <= 10; i++) {
    double d = i * 0.1;
    // ...
}

// tolerância relativa, para magnitudes quaisquer
static boolean quaseIgual(double x, double y, double eps) {
    return Math.abs(x - y) <= eps * Math.max(Math.abs(x), Math.abs(y));
}
```

A analogia é comparar duas medições da mesma mesa feitas com trena: ninguém espera ler "exatamente 1,532000 m" duas vezes seguidas; a pergunta é "as duas medidas estão dentro de uns poucos milímetros uma da outra?". O epsilon é esse "poucos milímetros", e precisa ser escolhido para a escala do que se mede. O que na trena são os traços finitos da fita, no `double` são os 52 bits finitos da mantissa.

Quando você precisa de igualdade decimal exata — dinheiro, de novo — a alternativa é `BigDecimal` e o seu `compareTo`. Contadores de laço e identificadores devem ser inteiros. Frameworks de teste oferecem `assertEquals(esperado, obtido, delta)` justamente porque `==` entre `double` está errado nesse contexto. O `==` só é aceitável sobre um `double` que você atribuiu diretamente, sem nenhuma conta no meio (`if (fator == 0.0)` logo após `double fator = 0.0;`), ou ao testar os valores especiais pelos métodos próprios. Mesmo aí, uma tolerância costuma ser mais segura e mais clara para quem for ler o código depois.

## `BigInteger`

Os capítulos anteriores mostraram os limites do ponto flutuante. Este trata de um limite diferente: o dos inteiros. `int` guarda no máximo cerca de 2,1 bilhões e `long` chega a uns 9,2 quintilhões, mas há contas — fatoriais, combinatória, chaves de criptografia, identificadores de sistemas enormes — em que esses tetos são pequenos demais. Quando o valor passa do teto, o resultado não dá erro: ele "dá a volta" e vira um número errado, muitas vezes negativo. A classe `java.math.BigInteger` existe para representar inteiros sem teto nenhum, com todos os dígitos sempre exatos.

Este capítulo tem duas partes. A primeira apresenta o que é um inteiro de tamanho arbitrário, como criá-lo e por que ele é necessário. A segunda mostra como fazer contas com esses objetos, já que os operadores `+`, `-`, `*` e `/` não funcionam com eles e tudo passa a ser feito por métodos.

### Inteiros arbitrariamente grandes

Um `BigInteger` é um objeto que representa um número inteiro de qualquer tamanho, positivo ou negativo, sem o limite fixo dos tipos primitivos. Ele pertence ao pacote `java.math`, o mesmo de `BigDecimal`, e é **imutável**: uma vez criado, seu valor nunca muda; qualquer operação produz um novo objeto. Internamente ele guarda a magnitude do número num vetor de inteiros que cresce conforme a necessidade, alocando mais memória à medida que o número aumenta. É esse crescimento dinâmico que substitui os 32 ou 64 bits fixos de `int` e `long`.

Sem `BigInteger`, o programador esbarra no estouro (*overflow*). Um `int` que vale `2_147_483_647` e recebe `+ 1` passa a valer `-2_147_483_648`, sem aviso. O caso clássico é o fatorial: `13!` já não cabe em `int`, e `21!` já não cabe em `long`. Uma contagem de combinações, o número de arranjos possíveis de um baralho (`52!`, um número com 68 dígitos) ou a multiplicação de dois primos grandes na geração de uma chave RSA simplesmente não têm como ser representados nos primitivos. A alternativa histórica — usar `double` — troca o estouro por outro problema: a partir de `2^53` o `double` perde a capacidade de distinguir inteiros consecutivos, então o resultado fica aproximado.

`BigInteger` resolve isso guardando o número inteiro, dígito a dígito, sem arredondar e sem estourar. Cria-se um a partir de um `long` com `BigInteger.valueOf(1_000_000)`, a partir de texto com `new BigInteger("9007199254740993")` — útil quando o número nem cabe num literal `long` — e há as constantes prontas `BigInteger.ZERO`, `ONE`, `TWO` e `TEN`. A forma de texto é a única saída quando o valor vem de um arquivo ou de uma requisição e já nasce grande demais para qualquer primitivo.

```java
BigInteger fatorial = BigInteger.ONE;
for (int i = 2; i <= 100; i++) {
    fatorial = fatorial.multiply(BigInteger.valueOf(i));
}
System.out.println(fatorial);
// 933262154439441526816992388562667004907159682643816214685929...
// (um número com 158 dígitos, exato)
```

A analogia é a diferença entre um odômetro de carro e uma folha de papel. O odômetro tem um número fixo de rodinhas: ao passar de `999999` ele volta a `000000`, e você perde a informação de quantas voltas o carro realmente deu — é o `int` estourando. A folha de papel não tem casas pré-fixadas: se o número cresce, você escreve mais um dígito à direita e, se preciso, pega outra folha. `BigInteger` é a folha de papel — mais lento de manusear que as rodinhas do odômetro, mas sem o risco de "dar a volta".

A alternativa mais simples continua sendo `long`, sempre que o valor comprovadamente cabe nele; para detectar estouro em vez de preveni-lo, existem `Math.addExact` e `Math.multiplyExact`, que lançam exceção em vez de dar a volta silenciosamente. Não use `BigInteger` quando os números cabem folgadamente em `long`: cada operação aloca um objeto novo e é ordens de grandeza mais lenta que uma soma de primitivos. Em laços muito quentes, com milhões de iterações sobre valores pequenos, essa troca pesa. E se o que você precisa é de casas decimais exatas — dinheiro, por exemplo —, a classe certa é `BigDecimal`, não esta.

### Operações

Como `BigInteger` é um objeto e Java não permite redefinir operadores, não existe `a + b` nem `a * b` entre dois `BigInteger`. Toda a aritmética é feita por métodos com nome: `add`, `subtract`, `multiply`, `divide`, `mod`, `remainder`, `pow`, `negate`, `abs`, `gcd`. Cada um recebe outro `BigInteger` (ou, no caso de `pow`, um `int`) e **devolve um novo objeto**, porque o valor original é imutável. Isso permite encadear as chamadas: `a.add(b).multiply(c)` calcula `(a + b) * c`.

O problema que os métodos resolvem é a ausência de sobrecarga de operadores. Em C++ ou Python, uma classe de inteiro grande poderia ser usada com `+` normalmente; em Java, tentar isso não compila. A saída da linguagem foi expor cada operação como um método, o que deixa o código mais verboso, porém explícito: fica claro na leitura que ali há uma operação de precisão arbitrária, não uma soma barata de primitivos.

Alguns detalhes mudam em relação aos primitivos. `divide` faz divisão inteira e trunca em direção a zero, como o `/` entre `int`; para obter quociente e resto de uma vez há `divideAndRemainder`, que retorna um vetor de dois elementos. `mod` exige um divisor positivo e sempre devolve resultado não negativo (útil para aritmética modular), enquanto `remainder` acompanha o sinal do dividendo — os dois diferem quando há negativos envolvidos. A comparação se faz com `compareTo` (retorna negativo, zero ou positivo) ou, para igualdade pura, com `equals`; nunca com `==`, que compara referências de objeto. Para voltar a um primitivo depois da conta, prefira `longValueExact` e `intValueExact`, que lançam exceção se o número não couber, em vez de `longValue`, que trunca em silêncio.

```java
BigInteger a = new BigInteger("123456789012345678901234567890");
BigInteger b = BigInteger.valueOf(987654321);

BigInteger soma     = a.add(b);
BigInteger produto  = a.multiply(b);
BigInteger potencia = BigInteger.TWO.pow(64);        // 18446744073709551616
BigInteger[] qr     = a.divideAndRemainder(b);       // [quociente, resto]
BigInteger mdc      = a.gcd(b);

if (a.compareTo(b) > 0) {
    System.out.println("a é maior");
}
```

Um uso concreto: validar um dígito verificador (CPF, código de barras, IBAN) costuma exigir tratar toda a sequência de dígitos como um único número e tirar o resto da divisão por um módulo — `new BigInteger(sequencia).mod(BigInteger.valueOf(97))` faz isso mesmo quando a sequência tem 30 caracteres e não caberia em `long`. Outro: `pow` combinado com `mod` permite exponenciação modular via `modPow`, a operação central de RSA e Diffie-Hellman.

Pense em conferir uma conta grande à mão em vez de na calculadora de bolso: cada passo — somar, multiplicar, dividir — é escrito por extenso, um de cada vez, e o resultado de um passo é a entrada do próximo. É mais trabalhoso do que apertar `+`, mas nenhum dígito se perde no caminho. Os nomes dos métodos são esse "por extenso".

O conceito anterior já cobriu quando evitar `BigInteger` — valores que cabem em `long`, laços quentes sobre números pequenos —, e cada método encadeado aqui, alocando um objeto por etapa, é exatamente o custo que pesa nesses casos. O acréscimo próprio das operações é outro: quando o número tem parte fracionária, a aritmética equivalente vive em `BigDecimal`, cujos métodos têm nomes quase iguais (`add`, `multiply`, `divide`) mas pedem um argumento a mais para controlar escala e arredondamento.

## `BigDecimal`

Este capítulo fecha o módulo tratando do número decimal exato. Os capítulos sobre ponto flutuante mostraram que `double` erra ao representar frações comuns como 0,1; o capítulo sobre `BigInteger` mostrou como guardar inteiros sem teto, dígito a dígito. `BigDecimal` junta as duas ideias: representa um número com casas decimais em que todos os dígitos são exatos, sem o arredondamento binário do `double` e sem o limite de tamanho dos primitivos. É a classe padrão da plataforma para valores monetários e para qualquer conta em que "perto o suficiente" não seja aceitável.

O capítulo tem três partes. A primeira explica o que é precisão decimal e como `BigDecimal` a garante. A segunda trata da escala — o número de casas depois da vírgula —, que é o parâmetro central da classe e a origem da maioria das confusões de quem começa a usá-la. A terceira aplica tudo ao caso mais frequente na prática: representar e calcular dinheiro.

### Precisão decimal

Precisão decimal é a garantia de que um número com parte fracionária seja guardado com todos os seus dígitos, na base 10, sem nenhuma aproximação. `BigDecimal`, do pacote `java.math`, oferece essa garantia. Ele é imutável e guarda o valor como um par: um inteiro sem escala (o *unscaled value*, internamente um `BigInteger`) e uma escala inteira. O número representado é `unscaledValue × 10^(-scale)`. Assim, `new BigDecimal("3.14")` guarda o inteiro `314` com escala `2`. Como a base é 10, os dígitos que você escreve são exatamente os dígitos que ficam armazenados.

O problema que isso resolve aparece com `double` e `float`, que trabalham na base 2. Frações como 0,1, 0,2 ou 0,01 não têm representação binária finita — viram dízimas em base 2 e são truncadas. O sintoma clássico é `0.1 + 0.2` resultar em `0.30000000000000004`. Somar `0.10` dez vezes com `double` não dá exatamente `1.00`. Esses pequenos desvios se acumulam: num relatório com milhares de linhas, um erro de frações de centavo por linha vira uma diferença visível no total, e o extrato não fecha.

`BigDecimal` elimina o desvio porque nunca converte para base 2. Ele guarda `1` com escala `1` para representar 0,1, e pronto — a soma, a subtração e a multiplicação entre `BigDecimal` são operações exatas, sem perda de dígitos. O cuidado essencial é na criação: use sempre a forma de texto, `new BigDecimal("0.1")`, ou `BigDecimal.valueOf(0.1)` (que internamente passa por `Double.toString`). O construtor `new BigDecimal(0.1)` que recebe um `double` copia a aproximação binária inteira e guarda `0.1000000000000000055511151231257827021181583404541015625` — o defeito que você queria evitar entra pela porta da frente.

```java
double a = 0.1 + 0.2;
System.out.println(a);                       // 0.30000000000000004

BigDecimal b = new BigDecimal("0.1").add(new BigDecimal("0.2"));
System.out.println(b);                       // 0.3
```

A analogia é a de duas réguas. Uma é marcada em milímetros — base 10; medir "um décimo de centímetro" cai exatamente sobre um traço. A outra é marcada em frações de polegada por potências de dois: 1/2, 1/4, 1/8, 1/16. Nessa segunda régua, "um décimo" nunca coincide com traço nenhum; você sempre lê o traço mais próximo e aceita um errinho. `double` é a régua de polegadas; `BigDecimal` é a régua milimétrica.

A alternativa mais enxuta a `BigDecimal` é representar o valor como um inteiro na menor unidade — centavos em um `long` — e cuidar da vírgula por conta própria; é exato e rápido, ao custo de você mesmo gerenciar formatação e arredondamento. Outra alternativa, arriscada, é usar `double` durante as contas e arredondar só no fim, o que funciona até o dia em que não funciona. E quando o problema for inteiros gigantes sem parte fracionária, a classe é `BigInteger`, vista antes. Não use `BigDecimal` em cálculo científico ou estatístico pesado, em física de jogos ou em aprendizado de máquina: ali o `double` é ordens de grandeza mais rápido e o erro relativo minúsculo é irrelevante. Cada operação com `BigDecimal` aloca objetos e é bem mais lenta; em laços muito quentes, isso pesa.

### Escala

A escala (*scale*) de um `BigDecimal` é o número de dígitos à direita do ponto decimal. `new BigDecimal("2.50")` tem escala 2; `new BigDecimal("2.5")` tem escala 1; `new BigDecimal("250")` tem escala 0. A escala pode até ser negativa: `new BigDecimal("2.5E3")` guarda o inteiro `25` com escala `-2`, representando 2500. O método `scale()` devolve esse número e `precision()` devolve a contagem total de dígitos significativos. A escala é o segundo componente do par que define o valor, e é ela que você precisa controlar em quase todo uso sério da classe.

Ignorar a escala leva a dois enganos frequentes. O primeiro: `equals` compara também a escala, então `new BigDecimal("2.0").equals(new BigDecimal("2.00"))` é `false`, mesmo os dois valendo dois. Para comparar apenas o valor numérico, use `a.compareTo(b) == 0`. O segundo: `a.divide(b)` sem mais argumentos lança `ArithmeticException` quando o resultado é uma dízima, como `1 / 3`, porque a classe se recusa a arredondar sem que você diga onde parar e como.

A saída é sempre declarar a escala desejada. `setScale(2, RoundingMode.HALF_UP)` fixa o valor em duas casas, arredondando o que sobra. `divide(divisor, 2, RoundingMode.HALF_EVEN)` divide já informando quantas casas manter e qual regra aplicar. Nas demais operações a escala do resultado segue regras fixas: em `add` e `subtract` ela é a maior das duas escalas de entrada; em `multiply` é a **soma** das escalas, então `new BigDecimal("0.10").multiply(new BigDecimal("0.10"))` dá `0.0100`, escala 4. O método `stripTrailingZeros()` remove zeros à direita e ajusta a escala (2,500 vira 2,5); cuidado que, para inteiros com muitos zeros, ele pode produzir notação científica (`"600"` vira `6E+2`) — encadeie `toPlainString()` quando for exibir.

```java
BigDecimal x = new BigDecimal("10.00");
BigDecimal y = new BigDecimal("3");

// x.divide(y);                       // ArithmeticException: non-terminating
BigDecimal z = x.divide(y, 2, RoundingMode.HALF_UP);   // 3.33

System.out.println(new BigDecimal("2.0").equals(new BigDecimal("2.00")));      // false
System.out.println(new BigDecimal("2.0").compareTo(new BigDecimal("2.00")));   // 0
```

Pense no mostrador de uma balança digital. "2 kg", "2,0 kg" e "2,00 kg" indicam o mesmo peso, mas comunicam precisões de medição diferentes. Trocar o mostrador de duas casas para uma casa — o que `setScale` faz — obriga a decidir o que acontece com o dígito que desaparece: cai fora, sobe, vai para o par mais próximo. Essa decisão é o `RoundingMode`, e não existe troca de escala honesta sem ela.

Uma alternativa a controlar casas fixas é o `MathContext`, que limita o total de dígitos significativos em vez do número de casas decimais — conveniente para contas de engenharia, inadequado para dinheiro, onde o que importa é a casa do centavo e não a quantidade de algarismos. Outra abordagem é nunca mexer na escala: manter todo valor com uma escala fixa combinada e jamais dividir. Nesse cenário restrito a escala se preserva sozinha nas somas e subtrações, e não há decisão de arredondamento a tomar — mas basta uma divisão ou um percentual entrar em cena para a questão voltar.

### Dinheiro

Dinheiro é o caso de uso que justifica a existência de `BigDecimal` na maioria dos sistemas. Valores monetários são decimais exatos por definição legal — a menor unidade é o centavo —, têm escala fixa determinada pela moeda (2 casas para real, dólar e euro; 0 para o iene; 3 para o dinar kuwaitiano) e exigem uma regra de arredondamento explícita e auditável. `BigDecimal` atende aos três requisitos; `double` e `float` não atendem a nenhum e nunca devem guardar dinheiro.

Sem `BigDecimal`, os defeitos já vistos em "Precisão decimal" reaparecem no balancete: dez centavos em `double` não são dez centavos, mil lançamentos de dez centavos não somam cem reais, e a auditoria aponta a diferença no fim do mês. Construir o valor com `new BigDecimal(precoDouble)` reintroduz esse mesmo erro binário. E dividir um valor — um rateio de frete, um parcelamento — sem definir o arredondamento resulta em exceção ou, pior, em um centavo que some ou sobra sem ninguém notar.

Dois cuidados valem desde já. Construa o valor a partir de texto ou de centavos inteiros — `new BigDecimal("19.90")` ou `BigDecimal.valueOf(1990, 2)` —, nunca de um `double`; e, depois de cada operação que altera a escala (multiplicar por quantidade, aplicar um percentual), volte à escala da moeda com `setScale(2, modo)`. A lista fechada de práticas — qual `RoundingMode` adotar como política da casa, como distribuir a sobra de um rateio, como comparar e persistir valores — está reunida em "Boas práticas financeiras", o capítulo que encerra o módulo.

```java
BigDecimal preco    = new BigDecimal("19.90");
BigDecimal subtotal = preco.multiply(new BigDecimal("3"));               // 59.70
BigDecimal desconto = subtotal.multiply(new BigDecimal("0.10"))
                              .setScale(2, RoundingMode.HALF_UP);        // 5.97
BigDecimal total    = subtotal.subtract(desconto);                      // 53.73
```

Pense no caixa de banco conferindo cédulas e moedas. Nunca é "mais ou menos cem reais"; é o valor exato até o centavo — e é essa exigência de exatidão, não a conveniência, que decide quando `BigDecimal` entra no lugar de `double`.

A alternativa mais comum é guardar o valor como um inteiro de centavos em `long` — exato e rápido, ao custo de você cuidar da formatação e do ponto decimal —, adotada às vezes até dentro de finanças, em trechos de frequência altíssima de operações. Bibliotecas dedicadas como JavaMoney e Joda-Money são outra saída; "Boas práticas financeiras" volta a elas. Vale aqui o limite já apontado em "Precisão decimal": fora de dinheiro e de outros decimais exatos, `BigDecimal` só custa desempenho sem contrapartida.

## Arredondamento decimal

Este capítulo fecha o Módulo 6 juntando as pontas deixadas pelos anteriores. O capítulo sobre `BigDecimal` mostrou que a classe se recusa a descartar dígitos por conta própria: toda vez que uma conta produz mais casas do que cabem, é você quem precisa dizer quantas manter e para que lado o dígito que sobra vai. Essa decisão tem nome — arredondamento — e a plataforma oferece um vocabulário fechado para expressá-la.

O capítulo tem quatro partes. `RoundingMode` é o enum que lista as regras possíveis de arredondamento. `MathContext` é o objeto que embrulha uma regra dessas junto com uma quantidade de dígitos, para reaproveitar a mesma política em várias operações. `Divisões` trata da operação que mais obriga a arredondar, porque é a única que pode gerar uma dízima infinita. E `Boas práticas financeiras` reúne, em forma de checklist, tudo o que o módulo ensinou aplicado ao caso que motiva quase todo esse cuidado: dinheiro.

### `RoundingMode`

`RoundingMode` é um enum do pacote `java.math` com oito constantes, cada uma descrevendo uma regra para eliminar os dígitos que não cabem no resultado. Ele aparece como argumento em `setScale`, em quase todas as sobrecargas de `divide`, no construtor de `MathContext` e no método `round` de `BigDecimal`. Sempre que uma operação com `BigDecimal` precisa encurtar um número, é um valor de `RoundingMode` que diz como.

O problema que ele resolve é a ambiguidade da frase "arredonde para duas casas". O número `2,345` arredondado para dois dígitos pode virar `2,34` ou `2,35`, dependendo do critério; `2,5` convertido para inteiro pode ser `2` ou `3`. `BigDecimal` não escolhe por você — sem um `RoundingMode`, ele lança `ArithmeticException` em vez de adivinhar. Antes do Java 5, quando não havia enums, essas regras eram constantes inteiras soltas em `BigDecimal` (`ROUND_HALF_UP`, `ROUND_DOWN`…); qualquer `int` compilava, e passar o número errado só aparecia em produção. O enum fecha esse conjunto: só os oito valores válidos existem, e o compilador barra o resto.

As constantes se dividem em dois grupos. As **direcionais** ignoram qual metade está mais perto e sempre vão para um lado: `UP` afasta de zero (arredonda a magnitude para cima), `DOWN` aproxima de zero (trunca), `CEILING` vai para o infinito positivo, `FLOOR` vai para o infinito negativo. Para números positivos, `DOWN` e `FLOOR` coincidem, assim como `UP` e `CEILING`; com negativos elas se separam. As de **desempate** só agem quando o dígito descartado é exatamente `5` seguido de nada: `HALF_UP` nesse empate afasta de zero (o arredondamento "comercial", o que a maioria das pessoas aprendeu na escola), `HALF_DOWN` aproxima de zero, e `HALF_EVEN` manda para o vizinho par mais próximo — o "arredondamento do banqueiro", que ao longo de muitos valores não puxa o total sistematicamente para cima. A oitava, `UNNECESSARY`, é uma afirmação: "esta operação não deve precisar arredondar"; se precisar, lança `ArithmeticException`, funcionando como uma trava de segurança.

```java
BigDecimal v = new BigDecimal("2.345");
v.setScale(2, RoundingMode.HALF_UP);    // 2.35
v.setScale(2, RoundingMode.HALF_DOWN);  // 2.34
v.setScale(2, RoundingMode.DOWN);       // 2.34
v.setScale(2, RoundingMode.CEILING);    // 2.35

new BigDecimal("2.5").setScale(0, RoundingMode.HALF_EVEN);   // 2
new BigDecimal("3.5").setScale(0, RoundingMode.HALF_EVEN);   // 4
new BigDecimal("2.34").setScale(2, RoundingMode.UNNECESSARY); // 2.34, sem erro
new BigDecimal("2.345").setScale(2, RoundingMode.UNNECESSARY); // ArithmeticException
```

A analogia é a de arredondar a hora ao contar para alguém. "Direcional" é quem sempre arredonda para trás — "cheguei umas duas" quando eram 14h40 — ou sempre para frente. "Meio" é quem olha os minutos e decide pelo mais próximo; e a regra de desempate é o que essa pessoa faz quando são exatamente 14h30. `HALF_UP` sobe para as 15h, `HALF_EVEN` decide pelo horário "par" mais próximo. A escolha muda pouco em uma medição isolada, mas em um relatório com milhares de linhas o viés acumulado de sempre subir no empate aparece no total.

As alternativas são as constantes inteiras legadas (`BigDecimal.ROUND_*`), hoje desencorajadas e mantidas só por compatibilidade; `Math.round`, que serve para `double` e sempre usa um critério fixo equivalente a `HALF_CEILING`, sem opção; e `DecimalFormat`, que também aceita um `RoundingMode`, mas para formatar texto de saída, não para calcular. Não há motivo para usar `RoundingMode` quando a operação é exata — somar, subtrair ou multiplicar dois `BigDecimal` nunca descarta dígito, então nenhum modo é consultado. E evite `UNNECESSARY` por hábito: ele só faz sentido quando você realmente espera um resultado exato e quer que uma dízima inesperada estoure em vez de passar batida.

### `MathContext`

`MathContext`, também de `java.math`, é um objeto imutável que agrupa dois parâmetros de arredondamento: uma **precisão** (o número de dígitos significativos que o resultado pode ter) e um `RoundingMode` para aplicar quando esse limite for ultrapassado. Ele é aceito no construtor de `BigDecimal` e em praticamente todos os métodos aritméticos da classe — `add`, `subtract`, `multiply`, `divide`, `pow`, `round` —, servindo para impor a mesma política de precisão a uma sequência inteira de contas sem repetir os argumentos toda vez.

Sem `MathContext`, a aritmética de `BigDecimal` opera em modo exato, e isso tem dois inconvenientes. O primeiro é que `divide` sem parâmetros de arredondamento lança exceção assim que o quociente é uma dízima. O segundo é mais sutil: `multiply` exato faz a escala crescer sem parar — multiplicar dez valores com duas casas cada produz um resultado com vinte casas decimais, a maioria sem significado prático, ocupando memória e tempo. Passar `scale` e `RoundingMode` em cada chamada resolve, mas polui o código quando há muitas operações encadeadas com a mesma intenção de "trabalhar com N dígitos".

`MathContext` empacota essa intenção. Você cria uma vez — `new MathContext(10, RoundingMode.HALF_EVEN)` ou, com o modo padrão `HALF_UP`, só `new MathContext(10)` — e passa o mesmo objeto adiante. A precisão `0` significa "ilimitada", ou seja, aritmética exata. A classe traz constantes prontas alinhadas ao padrão IEEE 754-2008: `MathContext.DECIMAL32` (7 dígitos), `DECIMAL64` (16 dígitos, equivalente em precisão a um `double`), `DECIMAL128` (34 dígitos) e `UNLIMITED`. O ponto que mais confunde é que a precisão conta **dígitos significativos no número todo**, não casas depois da vírgula — é uma medida diferente da escala controlada por `setScale`.

```java
BigDecimal um = new BigDecimal("1");
BigDecimal tres = new BigDecimal("3");

um.divide(tres, new MathContext(5));     // 0.33333  (5 dígitos significativos)
um.divide(tres, 5, RoundingMode.HALF_UP); // 0.33333  (5 casas decimais)

BigDecimal grande = new BigDecimal("1234567.891");
grande.round(new MathContext(4, RoundingMode.HALF_UP)); // 1.235E+6
grande.setScale(2, RoundingMode.HALF_UP);               // 1234567.89
```

Pense na diferença entre "me diga esse número com cinco algarismos" e "me diga esse número com dois centavos". A primeira instrução — `MathContext` — vale tanto para `3,1416` quanto para `31416000`: conta algarismos, não posição da vírgula. A segunda — `setScale` — fixa a casa decimal e é o que uma moeda exige. Para um cálculo de engenharia em que interessa a precisão relativa (cinco algarismos bons, esteja o número na casa dos milhares ou dos milionésimos), `MathContext` é natural. Para dinheiro, ele é a ferramenta errada: `new MathContext(3)` sobre `1234.56` devolve `1.23E+3` e os centavos evaporam.

A alternativa direta é justamente `setScale`, ou as sobrecargas de `divide` que recebem `scale` e `RoundingMode` — melhores sempre que o que importa é a casa decimal fixa. Outra é uma convenção combinada no projeto ("todo valor com 8 casas") aplicada manualmente. Não use `MathContext` para valores monetários nem em qualquer contexto em que a quantidade de casas decimais seja parte do contrato; reserve-o para contas científicas e de engenharia onde a precisão relativa é o que conta.

### Divisões

Divisão é a operação de `BigDecimal` que praticamente sempre obriga a decidir sobre arredondamento, porque é a única das quatro operações básicas que pode gerar um resultado com infinitas casas decimais. Somar, subtrair e multiplicar dois números com um número finito de dígitos sempre dá um número com um número finito de dígitos; dividir, não — `1 / 3` é `0,333…` sem fim.

Por isso `a.divide(b)`, a sobrecarga sem mais argumentos, lança `ArithmeticException` com a mensagem "Non-terminating decimal expansion; no exact representable decimal result" sempre que o quociente não termina: `1/3`, `10/3`, `1/7`, `2/11` e a maioria das divisões reais. Só passa sem erro quando o resultado é exato, como `1/8 = 0,125` ou `10/4 = 2,5`. Com `double` a divisão nunca reclama — ela devolve silenciosamente a aproximação binária mais próxima, e o erro só aparece lá na frente, no total que não fecha. `BigDecimal` inverte isso: ou você diz como arredondar, ou o programa para na hora.

As sobrecargas que resolvem são três. `divide(divisor, RoundingMode)` divide e arredonda mantendo a escala do dividendo. `divide(divisor, escala, RoundingMode)` deixa você fixar exatamente quantas casas o resultado terá — a forma mais explícita e a preferida para dinheiro. `divide(divisor, MathContext)` limita por dígitos significativos, como visto no capítulo anterior. Há ainda operações que separam o quociente inteiro do resto: `divideToIntegralValue` devolve só a parte inteira da divisão, `remainder` devolve o que sobra, e `divideAndRemainder` devolve os dois num array — úteis para converter uma quantia em "quantas notas de 50 e quanto de troco". Para dividir por potências de 10, não use `divide`: `movePointLeft(2)` desloca a vírgula duas casas e é exato, sem arredondamento nenhum.

```java
BigDecimal total = new BigDecimal("10.00");

// total.divide(new BigDecimal("3"));                 // ArithmeticException
total.divide(new BigDecimal("3"), 2, RoundingMode.HALF_UP);   // 3.33
total.divide(new BigDecimal("4"));                            // 2.50  (exato, sem erro)

BigDecimal preco = new BigDecimal("59.90");
BigDecimal qtd   = new BigDecimal("7");
BigDecimal unit  = preco.divide(qtd, 2, RoundingMode.HALF_UP); // 8.56 por unidade

new BigDecimal("1250").movePointLeft(2);   // 12.50, sem RoundingMode
```

A analogia é a de repartir uma pizza. Somar fatias, tirar fatias ou triplicar a pizza dá sempre um número inteiro de fatias. Dividir uma pizza entre três pessoas, não: sobra um pedaço que não parte igual, e alguém tem de decidir o que fazer com ele — cortar mais fino (mais casas decimais), dar para um dos três (arredondar para cima), ou deixar na caixa (truncar). `BigDecimal` exige que essa decisão esteja escrita antes de cortar.

A alternativa mais comum, em dinheiro, é trabalhar com centavos inteiros num `long` e usar o operador `%` para achar o resto de forma exata, distribuindo-o depois. Também é possível multiplicar pelo inverso do divisor em vez de dividir, mas só ajuda quando esse inverso tem representação decimal exata, o que é raro. Evite encadear várias divisões arredondando em cada etapa: cada arredondamento intermediário introduz um pequeno erro que se soma. Quando der, divida uma única vez, no fim da conta. E para deslocamento de vírgula por potência de 10, prefira sempre `movePointLeft`/`movePointRight`, que não perdem precisão.

### Boas práticas financeiras

Este capítulo não apresenta uma API nova: ele consolida, em forma de regras práticas, tudo o que o módulo mostrou sobre `BigDecimal`, escala, `RoundingMode` e divisão, aplicado ao domínio que motiva quase todo esse cuidado — valores monetários. A ideia é ter uma lista curta de decisões que, tomadas uma vez e seguidas de forma consistente, evitam a classe inteira de bugs de "o extrato não fecha por um centavo".

Sem essas práticas, os defeitos reaparecem sempre pelos mesmos caminhos: alguém guarda dinheiro em `double` ou `float` e dez centavos deixam de ser dez centavos; alguém constrói o valor com `new BigDecimal(valorDouble)` e copia o erro binário para dentro do `BigDecimal`; alguém chama `divide` sem `RoundingMode` e o sistema quebra em produção, ou fixa um modo diferente em cada ponto do código e os totais divergem conforme o caminho percorrido; um rateio de parcelas soma um centavo a menos que o valor original; uma comparação usa `equals` e considera `2.5` diferente de `2.50`.

As regras que previnem isso são poucas:

- **Nunca use `double` ou `float` para dinheiro.** Use `BigDecimal` ou um inteiro de centavos (`long`).
- **Construa o valor a partir de texto ou de centavos inteiros:** `new BigDecimal("19.90")` ou `BigDecimal.valueOf(1990, 2)`, nunca a partir de um `double`.
- **Fixe a escala da moeda após cada operação que a altera.** Multiplicar por quantidade ou aplicar um percentual muda a escala; volte a ela com `setScale(2, modo)` (2 casas para real, dólar e euro; 0 para o iene; 3 para o dinar kuwaitiano).
- **Escolha um `RoundingMode` como política da casa e documente-o.** `HALF_UP` é o arredondamento comercial esperado pela maioria; `HALF_EVEN` é exigido por várias normas contábeis por não enviesar grandes volumes. O importante é ser o mesmo em todo o sistema.
- **Em rateios, distribua a sobra.** Arredonde cada parcela, some-as e jogue a diferença de centavos na última parcela, para o total bater com o valor original.
- **Compare valores com `compareTo(x) == 0`, não com `equals`,** que também compara a escala.
- **Persista como `DECIMAL`/`NUMERIC` no banco,** nunca como `FLOAT` ou `DOUBLE`, e mantenha `BigDecimal` de ponta a ponta na aplicação.

```java
BigDecimal preco    = new BigDecimal("19.90");
BigDecimal subtotal = preco.multiply(new BigDecimal("3"));                 // 59.70
BigDecimal imposto  = subtotal.multiply(new BigDecimal("0.08"))
                              .setScale(2, RoundingMode.HALF_UP);          // 4.78
BigDecimal total    = subtotal.add(imposto);                              // 64.48

// dividir 64.48 em 3 parcelas sem perder centavo
BigDecimal parcela = total.divide(new BigDecimal("3"), 2, RoundingMode.DOWN);  // 21.49
BigDecimal ultima  = total.subtract(parcela.multiply(new BigDecimal("2")));    // 21.50
```

A analogia é a do fechamento de caixa de uma loja. Ninguém encerra o dia com "deu mais ou menos isso"; confere-se cédula por cédula até o centavo, e existe um procedimento escrito para o troco que não divide igual. As boas práticas acima são esse procedimento traduzido para código: um tipo exato (`BigDecimal`), uma escala fixa (a da moeda), uma regra de arredondamento única e auditável, e uma política explícita para a sobra do rateio.

As alternativas dependem da escala do sistema. Em volumes altíssimos, guardar o valor como centavos inteiros num `long` é mais rápido e igualmente exato, ao custo de você cuidar da formatação e da vírgula. Para aplicações que lidam com várias moedas, bibliotecas dedicadas — JavaMoney (a especificação JSR 354, com o tipo `MonetaryAmount`) e Joda-Money — embrulham `BigDecimal` junto com a moeda e as regras de arredondamento, reduzindo erro manual. E parte da aritmética pode ser feita no próprio banco de dados, com colunas `DECIMAL`. Nada disso se aplica fora de dinheiro e de outros decimais exatos: em cálculo científico, em relatórios aproximados ou em laços muito quentes, o custo de desempenho do `BigDecimal` não se paga, e `double` ou o `long` de centavos são as escolhas certas.

---

# Módulo 7 — Data e hora

Trabalhar com datas e horas em programas é intrinsecamente complexo: fusos horários, horário de verão, formatos variados, e a realidade de que "um dia nem sempre tem 24 horas". Java oferece, desde o Java 8, a API `java.time`, que resolve a maioria desses problemas com tipos imutáveis e um design claro. Este módulo explora `LocalDate`, `LocalTime`, `Duration`, `Period`, `Instant`, `ZonedDateTime` e como formatar/fazer parse de datas.

## Datas locais

O ponto de entrada da API `java.time` são os três tipos "locais": `LocalDate`, `LocalTime` e `LocalDateTime`. O adjetivo *local* quer dizer que nenhum deles carrega fuso horário — eles representam a data e a hora como estão escritas num calendário de parede ou num relógio de pulso, sem responder à pergunta "em que fuso?". São os tipos que você vai usar na esmagadora maioria dos casos do dia a dia: guardar uma data de nascimento, marcar o horário de abertura de uma loja, registrar quando um pedido foi criado.

Este capítulo apresenta os três lado a lado, porque a diferença entre eles é só quais campos cada um guarda: `LocalDate` só a data, `LocalTime` só a hora, `LocalDateTime` os dois juntos. Todos compartilham o mesmo estilo de uso — fábricas `of` e `now`, `parse` a partir de texto ISO, e métodos `plus`/`minus`/`with` que devolvem uma cópia modificada porque o objeto original é imutável. Os tipos que lidam com fuso e com instantes universais (`ZonedDateTime`, `Instant`) ficam para os capítulos seguintes.

### `LocalDate`

`LocalDate` é a classe de `java.time` que representa uma data de calendário — ano, mês e dia — sem hora e sem fuso horário. É um objeto imutável: cada método que "muda" a data na verdade devolve uma nova instância, deixando a original intacta. Ela se encaixa como o tipo padrão para tudo que é dia sem horário: data de nascimento, data de vencimento de uma fatura, feriado, data de um evento.

Antes do Java 8, o mesmo trabalho era feito com `java.util.Date` e `java.util.Calendar`, e a experiência era ruim. `Date`, apesar do nome, guarda um instante completo com horas, minutos e milissegundos, então não existia um tipo que fosse "só a data". `Calendar` tinha os meses numerados de `0` a `11` (janeiro era `0`), era mutável — qualquer parte do código com uma referência podia alterá-lo por baixo dos panos — e o formatador `SimpleDateFormat` não podia ser compartilhado entre threads. Calcular "trinta dias a partir de hoje" exigia manipular campos numéricos na mão e lembrar de acertar o resto.

`LocalDate` resolve isso com uma API pequena e previsível. Você cria uma data com `LocalDate.now()` (data atual), `LocalDate.of(2026, 9, 3)` (mês vai de `1` a `12`, como se espera) ou `LocalDate.parse("2026-09-03")` (formato ISO). A partir daí, `plusDays`, `minusMonths`, `plusYears` e `withDayOfMonth` produzem novas datas; `getDayOfWeek`, `isLeapYear`, `lengthOfMonth` respondem perguntas; `isBefore` e `isAfter` comparam. Para distância entre duas datas há `ChronoUnit.DAYS.between(a, b)` ou `Period.between(a, b)`.

```java
LocalDate hoje = LocalDate.now();
LocalDate nascimento = LocalDate.of(1994, 3, 18);
LocalDate vencimento = hoje.plusDays(30);

boolean venceu = vencimento.isBefore(LocalDate.now());
long idade = ChronoUnit.YEARS.between(nascimento, hoje);
DayOfWeek diaDaSemana = vencimento.getDayOfWeek(); // ex.: FRIDAY
```

Um caso real recorrente: em um sistema de cobrança, cada fatura tem `LocalDate dataEmissao` e a regra "vence em 15 dias" vira `dataEmissao.plusDays(15)`. Um job diário percorre as faturas e marca como atrasadas aquelas cujo `getVencimento().isBefore(LocalDate.now())`. Nenhum desses cálculos precisa saber que horas são nem em que fuso o servidor está.

As alternativas dentro da própria API são os outros tipos deste capítulo: `LocalDateTime` quando o horário do dia também importa, e `ZonedDateTime` quando a data depende do fuso (a virada de ano acontece em momentos diferentes em Tóquio e em São Paulo). Para gravar em bancos por JDBC antigo às vezes ainda aparece `java.sql.Date`, mas o driver moderno aceita `LocalDate` diretamente. Não use `LocalDate` quando você precisa marcar um instante preciso no tempo — o momento exato em que algo aconteceu, comparável entre máquinas do mundo todo —; esse é o papel de `Instant`.

### `LocalTime`

`LocalTime` representa uma hora do dia — horas, minutos, segundos e até nanossegundos — sem nenhuma data associada e sem fuso. Como `LocalDate`, é imutável. Serve para quando o que interessa é o ponteiro do relógio, não o dia: o horário em que uma loja abre, a hora de disparar um alarme diário, o intervalo de silêncio de um sistema de notificações.

Sem um tipo dedicado, representar "14:30" era desajeitado. Com `Date` ou `Calendar` era preciso escolher uma data qualquer só para pendurar a hora nela, e depois lembrar de ignorar essa data em todas as comparações. Isso levava a bugs sutis: dois horários "iguais" que não batiam porque tinham sido criados em dias diferentes.

`LocalTime` tem as mesmas fábricas dos outros tipos locais: `LocalTime.of(9, 0)`, `LocalTime.now()`, `LocalTime.parse("18:30")`. Os métodos seguem o padrão imutável — `plusHours`, `minusMinutes`, `withSecond` devolvem novas instâncias —, e `isBefore`/`isAfter` comparam pela posição no dia. Há constantes úteis como `LocalTime.NOON`, `LocalTime.MIDNIGHT`, `LocalTime.MIN` e `LocalTime.MAX`, e `truncatedTo(ChronoUnit.MINUTES)` para zerar os campos mais finos quando você não se importa com segundos.

```java
LocalTime abertura = LocalTime.of(9, 0);
LocalTime fechamento = LocalTime.of(18, 0);
LocalTime agora = LocalTime.now();

boolean lojaAberta = !agora.isBefore(abertura) && agora.isBefore(fechamento);
LocalTime proximoLembrete = agora.plusMinutes(15).truncatedTo(ChronoUnit.MINUTES);
```

Um exemplo concreto: numa agenda de consultório, cada horário disponível é um `LocalTime` (`09:00`, `09:30`, `10:00`…). A tela mostra a lista de horas livres para o dia que o paciente escolheu; a data vem separada, de um `LocalDate`. Manter os dois campos independentes deixa claro que "todo dia útil tem os mesmos horários" — a regra não precisa ser repetida para cada data.

A alternativa mais próxima é `LocalDateTime`, para quando a hora sozinha não basta e o dia importa junto. É importante não confundir `LocalTime` com `Duration`: `LocalTime` é um ponto no mostrador do relógio ("são 14:30"), enquanto `Duration` é uma quantidade de tempo ("faltam 90 minutos"). Some `Duration` a um `LocalTime` para obter outro `LocalTime`, mas não use um no lugar do outro. E `LocalTime` não serve quando o período atravessa a meia-noite — um turno das `22:00` às `06:00` — porque sem data não dá para saber que `06:00` é do dia seguinte; nesse caso o certo é trabalhar com `LocalDateTime`.

### `LocalDateTime`

`LocalDateTime` junta num só objeto uma data e uma hora — tudo que `LocalDate` e `LocalTime` guardam separadamente —, ainda sem fuso horário. É imutável como os outros. Você o usa quando precisa dos dois campos ao mesmo tempo e o contexto de fuso é único ou irrelevante: o instante em que um registro foi gravado no log de um servidor, o horário agendado de uma reunião interna, o carimbo `created_at` de uma linha de banco de dados que não guarda timezone.

Antes de `java.time`, a única opção que trazia data e hora juntas era `Calendar`, com os mesmos defeitos já vistos em `LocalDate` — mutabilidade, meses começando em zero, verbosidade. A alternativa era manter um `Date` e torcer para que todo o código concordasse sobre qual fuso ele representava — o que quase nunca acontecia, gerando as clássicas diferenças de "três horas" nos relatórios.

`LocalDateTime` combina as APIs dos dois tipos locais. Cria-se com `LocalDateTime.of(2026, 9, 10, 15, 0)`, `LocalDateTime.now()` ou `LocalDateTime.parse("2026-09-10T15:00:00")` (o `T` entre data e hora é o separador do formato ISO). Todos os métodos `plus`/`minus`/`with` de dias e de horas estão disponíveis e devolvem cópias. `toLocalDate()` e `toLocalTime()` extraem cada metade, e `atZone(ZoneId)` transforma o objeto num `ZonedDateTime` quando chega a hora de considerar o fuso.

```java
LocalDateTime reuniao = LocalDateTime.of(2026, 9, 10, 15, 0);
LocalDateTime termino = reuniao.plusMinutes(90);

LocalDate dia = reuniao.toLocalDate();     // 2026-09-10
LocalTime hora = reuniao.toLocalTime();    // 15:00
ZonedDateTime comFuso = reuniao.atZone(ZoneId.of("America/Sao_Paulo"));
```

Num sistema de reservas de salas, cada reserva é `LocalDateTime inicio` e `LocalDateTime fim`. Para detectar conflito de agenda, basta comparar: duas reservas se sobrepõem se `a.inicio.isBefore(b.fim) && b.inicio.isBefore(a.fim)`. Como todas as salas estão no mesmo prédio, e portanto no mesmo fuso, não há motivo para carregar essa informação em cada objeto.

A alternativa entra exatamente quando esse pressuposto cai. Se os participantes estão em cidades diferentes, use `ZonedDateTime`, que sabe o fuso e ajusta o horário de verão. Se o objetivo é registrar de forma inequívoca "o momento em que isto aconteceu", para comparar entre máquinas, use `Instant` ou `OffsetDateTime`. O maior risco de `LocalDateTime` é justamente esse: ele não sabe a que instante UTC corresponde, então dois `LocalDateTime` "iguais" em servidores de fusos distintos são momentos diferentes. E, nas transições de horário de verão, um mesmo `LocalDateTime` pode ser ambíguo (acontece duas vezes) ou não existir — outra razão para não usá-lo em nada que dependa de linha do tempo absoluta.

## Intervalos

Os tipos locais do capítulo anterior respondem "que data é" e "que horas são". Este capítulo trata da pergunta complementar: "quanto tempo separa dois desses pontos?". A API `java.time` divide essa ideia de "quantidade de tempo" em duas classes, porque tempo tem duas escalas que não se convertem uma na outra de forma exata. `Duration` mede tempo pela escala do relógio — segundos e nanossegundos, onde um dia sempre vale 86 400 segundos. `Period` mede tempo pela escala do calendário — anos, meses e dias, onde "um mês" pode ter 28, 30 ou 31 dias e "um ano" pode ter 365 ou 366.

Entender por que são dois tipos diferentes é o ponto central da aula. Somar "um mês" a 31 de janeiro tem que dar 28 de fevereiro, não "31 de janeiro mais 30 dias"; isso é trabalho de `Period`. Já cronometrar quanto uma requisição demorou exige a precisão fixa de `Duration`. As seções a seguir apresentam cada classe, e a última, "Cálculos temporais", mostra as ferramentas que amarram tudo — `ChronoUnit`, `until` e os ajustadores — e os erros clássicos de quem mistura as duas escalas.

### `Duration`

`Duration` é a classe de `java.time` que representa uma quantidade de tempo medida em segundos e nanossegundos — a escala do relógio, não a do calendário. É imutável, como todo o resto da API. Ela combina naturalmente com os tipos que também vivem nessa escala: `Instant`, `LocalTime` e `LocalDateTime`. Pense nela como a resposta para "quanto durou?" ou "daqui a quanto tempo?", quando a resposta é dada em horas, minutos e segundos.

Antes de `java.time`, esse tipo de medida era feito com um `long` de milissegundos. O código ficava cheio de números mágicos: um timeout de cinco minutos virava `5 * 60 * 1000`, e quem lesse depois tinha que decodificar a multiplicação para entender a intenção. Pior, nada no tipo `long` impedia somar um valor em milissegundos com outro em segundos — o compilador aceitava, e o bug só aparecia em produção.

`Duration` resolve isso dando nome às coisas. Você cria com fábricas explícitas — `Duration.ofMinutes(5)`, `Duration.ofHours(2)`, `Duration.ofSeconds(30)`, `Duration.ofMillis(200)` — ou calcula a diferença entre dois instantes com `Duration.between(inicio, fim)`. A partir daí, `plus`, `minus`, `multipliedBy`, `dividedBy` e `negated` produzem novas durações; `toMillis`, `toSeconds`, `toMinutes`, `toHoursPart` extraem valores; `isZero` e `isNegative` testam. Somar uma `Duration` a um `Instant` ou a um `LocalDateTime` desloca o ponto no tempo.

```java
Instant inicio = Instant.now();
processarLote();
Duration decorrido = Duration.between(inicio, Instant.now());
System.out.println("Levou " + decorrido.toMillis() + " ms");

Duration timeout = Duration.ofSeconds(30);
Duration backoff = timeout.multipliedBy(2); // 60 s

if (decorrido.compareTo(timeout) > 0) {
    log.warn("Processamento acima do limite");
}
```

Um uso real recorrente: configuração de timeouts e de espera entre tentativas. Um cliente HTTP recebe `Duration connectTimeout = Duration.ofSeconds(10)`; uma rotina de retry calcula o intervalo da próxima tentativa como `base.multipliedBy(1L << tentativa)`, dobrando a cada falha. O código fica legível sem comentário porque `ofSeconds(10)` já diz tudo.

A alternativa dentro da API é `Period`, para quando a quantidade é expressa em dias de calendário, meses ou anos. Para obter só um número inteiro de uma unidade — "quantos minutos ao todo" — `ChronoUnit.MINUTES.between(a, b)` devolve um `long` direto, sem criar objeto. Não use `Duration` para representar quantidades de calendário: `Duration.ofDays(30)` significa exatamente 30 × 24 horas e ignora que, no meio desse intervalo, pode ter havido uma virada de horário de verão que fez o dia ter 23 ou 25 horas. Para "daqui a 30 dias no calendário", o certo é `Period.ofDays(30)` somado a um `LocalDate`.

### `Period`

`Period` é a classe que representa uma quantidade de tempo na escala do calendário: uma combinação de anos, meses e dias. Também é imutável. Seu par natural é o `LocalDate` — ela existe para responder "quanto tempo de calendário separa duas datas?" e para deslocar uma data por uma quantidade expressa em termos humanos ("três meses depois", "um ano e meio atrás").

Sem um tipo assim, calcular idade ou vencimento era um exercício de aritmética manual sobre campos de `Calendar`: subtrair anos, depois corrigir se o mês atual ainda não chegou no mês de nascimento, depois corrigir de novo pelo dia, lembrando que fevereiro tem 28 ou 29 e que nem todo mês tem 31 dias. Era fácil errar um dos ajustes e só descobrir quando alguém nascido em 29 de fevereiro reclamava da idade errada.

`Period` embute essas regras. Cria-se com `Period.of(1, 6, 0)` (um ano e seis meses), `Period.ofMonths(3)`, `Period.ofWeeks(2)` (guardado como 14 dias) ou, o mais comum, `Period.between(dataInicial, dataFinal)`. O resultado é decomposto por `getYears()`, `getMonths()` e `getDays()`. Somar `Period` a um `LocalDate` respeita o calendário: `LocalDate.of(2026, 1, 31).plusMonths(1)` devolve `2026-02-28`, ajustando o dia que não existe. Há ainda `normalized()`, que converte 12 meses acumulados em 1 ano.

```java
LocalDate nascimento = LocalDate.of(1994, 3, 18);
Period idade = Period.between(nascimento, LocalDate.now());
System.out.println(idade.getYears() + " anos e " + idade.getMonths() + " meses");

LocalDate inicioTrial = LocalDate.now();
LocalDate fimTrial = inicioTrial.plus(Period.ofDays(14));
LocalDate proximaCobranca = inicioTrial.plusMonths(1);
```

Um exemplo concreto: num sistema de assinaturas, o plano guarda `Period cicloCobranca` (mensal é `Period.ofMonths(1)`, anual é `Period.ofYears(1)`). A data da próxima fatura é sempre `ultimaFatura.plus(cicloCobranca)` — e o dia 31 se acomoda sozinho nos meses curtos, sem código especial.

A alternativa é `Duration`, quando a quantidade precisa ser um número fixo de segundos ou horas. Se você quer só o total em uma unidade — "quantos meses completos entre as duas datas" — use `ChronoUnit.MONTHS.between(a, b)`, que devolve um `long`; `Period.between` é para quando você quer a divisão em anos + meses + dias separados. Não use `Period` para medir tempo decorrido real entre instantes (a linha do tempo absoluta), nem o combine com `Instant`: como um mês não tem duração fixa, `Period` não sabe dizer "quantos segundos isto representa". E cuidado ao comparar `Period`: dois períodos como `Period.ofDays(30)` e `Period.ofMonths(1)` não são iguais por `equals`, mesmo que às vezes cubram o mesmo intervalo.

### Cálculos temporais

"Cálculos temporais" é o nome que damos ao conjunto de operações que combinam datas, horas, `Duration` e `Period` para produzir uma resposta: quantos dias faltam para um prazo, qual a próxima segunda-feira, qual o último dia útil do mês, se um instante já passou. A API `java.time` foi desenhada para que todas essas contas usem os mesmos poucos mecanismos, em vez de cada uma exigir uma gambiarra própria.

Antes disso, cada cálculo desses era resolvido no braço. "Dias até o prazo" virava a diferença de dois `getTime()` em milissegundos dividida por `86 400 000`, o que dava errado nas viradas de horário de verão. "Próxima sexta-feira" era um laço que incrementava um `Calendar` dia a dia testando `get(DAY_OF_WEEK)`. O código era difícil de ler e cada projeto reinventava o seu.

As ferramentas que unificam isso são três. Primeira, `ChronoUnit.<UNIDADE>.between(inicio, fim)`, que devolve um `long` com o total de dias, horas, minutos, meses etc. entre dois pontos — ideal quando você quer um número, não um objeto. Segunda, os métodos `plus`/`minus` que aceitam um `Duration` ou um `Period` (ambos implementam `TemporalAmount`), além das variантes `plusDays`, `minusWeeks` e afins. Terceira, `with(TemporalAdjuster)` junto da classe `TemporalAdjusters`, que traz ajustadores prontos: `next(DayOfWeek.MONDAY)`, `lastDayOfMonth()`, `firstDayOfNextYear()`. Há ainda `data.until(outraData, ChronoUnit.DAYS)`, uma forma enxuta de `ChronoUnit.between`.

```java
LocalDate hoje = LocalDate.now();
LocalDate prazo = LocalDate.of(2026, 12, 31);

long diasRestantes = ChronoUnit.DAYS.between(hoje, prazo);
LocalDate proximaSegunda = hoje.with(TemporalAdjusters.next(DayOfWeek.MONDAY));
LocalDate fechamento = hoje.with(TemporalAdjusters.lastDayOfMonth());

boolean venceu = prazo.isBefore(hoje);
```

Um caso real: um painel de tarefas mostra, para cada item, "vence em X dias" usando `ChronoUnit.DAYS.between(LocalDate.now(), tarefa.getPrazo())`, pintando de vermelho quando o valor fica negativo. A rotina de fechamento contábil roda todo dia e só age de verdade quando `LocalDate.now().equals(hoje.with(TemporalAdjusters.lastDayOfMonth()))`.

A alternativa a montar esses cálculos à mão sempre foi usar uma biblioteca externa — o Joda-Time cumpria esse papel antes do Java 8 e ainda aparece em sistemas legados —, mas hoje a `java.time` cobre o mesmo terreno sem dependência. O erro a evitar é misturar as duas escalas de tempo: somar um `Period` quando o resultado precisa ser um instante exato, ou um `Duration` quando o resultado precisa respeitar o calendário. E, para qualquer cálculo que atravesse fusos ou datas de mudança de horário de verão, faça a conta sobre um `ZonedDateTime`, não sobre um `LocalDateTime` — só o tipo com fuso sabe que aquele dia teve 23 ou 25 horas.

---

## Instantes

Os capítulos anteriores olharam o tempo pela perspectiva humana: uma data no calendário, uma hora no relógio, a quantidade de dias entre dois eventos. Este capítulo troca de perspectiva e passa a olhar o tempo como a máquina o enxerga — um único ponto na linha do tempo absoluta, sem calendário, sem fuso, sem "que horas são aqui". É essa a visão que serve para carimbar quando um evento aconteceu, ordenar registros de log e guardar em banco de dados um `criado_em` que signifique a mesma coisa em qualquer servidor do mundo.

Três ideias sustentam o capítulo. `Instant` é a classe de `java.time` que representa esse ponto na linha do tempo. Epoch é o marco zero a partir do qual esse ponto é contado. E UTC é a referência universal de tempo em relação à qual tudo isso é medido, antes de qualquer conversão para o horário local. As seções a seguir tratam de cada uma na ordem em que se apoiam.

### `Instant`

`Instant` é a classe de `java.time` que representa um único instante na linha do tempo, guardado internamente como uma contagem de segundos e nanossegundos a partir de um marco fixo. É o "carimbo de tempo da máquina": não tem dia da semana, não tem fuso, não tem a noção de "manhã" ou "tarde". Vive na mesma escala do relógio que `Duration`, e é com `Duration` que ele se combina naturalmente. Como todo o resto da API, é imutável.

Antes dele, marcar um instante em Java era feito com `System.currentTimeMillis()`, que devolve um `long`. O problema do `long` é que ele não carrega significado: o mesmo tipo serve para um instante e para uma duração, e nada impede somar os dois por engano. Não há método para avançar cinco minutos, para comparar, para formatar — tudo vira aritmética manual sobre o número de milissegundos. A outra opção, `java.util.Date`, era mutável e misturava a representação do instante com a exibição em fuso, o que gerava confusão e boa parte dos seus métodos acabou marcada como obsoleta.

`Instant` resolve isso sendo um tipo com identidade. Você obtém o instante atual com `Instant.now()`, ou reconstrói um instante conhecido com `Instant.ofEpochSecond(segundos)` e `Instant.ofEpochMilli(millis)`. A partir daí, `plus` e `minus` aceitam uma `Duration` e devolvem um novo `Instant`; `isBefore`, `isAfter` e `compareTo` comparam dois instantes; `getEpochSecond` e `toEpochMilli` extraem o número para persistir ou transmitir. Para saber quanto tempo separou dois instantes, `Duration.between(inicio, fim)`.

```java
Instant inicio = Instant.now();
enviarRelatorio();
Instant fim = Instant.now();

Duration decorrido = Duration.between(inicio, fim);
System.out.println("Envio levou " + decorrido.toMillis() + " ms");

Instant expiraEm = Instant.now().plus(Duration.ofHours(24));
boolean aindaValido = Instant.now().isBefore(expiraEm);
```

O uso real mais comum é o registro de quando algo aconteceu. Uma linha de log guarda o `Instant` do evento; uma coluna `criado_em` no banco recebe `Instant.now()` no momento da inserção; um token de sessão guarda o `Instant` em que expira, e cada requisição confere se `Instant.now()` já passou desse ponto. Em todos esses casos, o valor precisa significar o mesmo instante independentemente de onde o código roda — e é exatamente isso que `Instant` garante.

As alternativas dependem do que você precisa. Se o objetivo é mostrar uma data e hora para uma pessoa, `Instant` sozinho não basta: falta o fuso, e o caminho é `instante.atZone(ZoneId.of("America/Sao_Paulo"))`, que produz um `ZonedDateTime`. Se você quer campos de calendário para fazer contas de meses ou anos, use `LocalDateTime` ou `LocalDate` com `Period`. Para medir tempo decorrido em micro-benchmarks, `System.nanoTime()` é mais barato e monotônico — não sofre ajustes de relógio no meio da medição, coisa que `Instant.now()` pode sofrer. E, quando o código precisa ser testável com o tempo "congelado", injete um `Clock` e chame `Instant.now(clock)`, em vez de depender do relógio real.

### Epoch

Epoch é o marco zero da contagem de tempo — o instante a partir do qual todos os outros são medidos. Em Java, como no Unix e na maioria dos sistemas modernos, esse marco é a meia-noite de 1º de janeiro de 1970 em UTC. Contar o tempo "desde o epoch" transforma qualquer instante num único número inteiro: os segundos (ou milissegundos) que se passaram desde aquele ponto. Esse número é o que se chama de "timestamp Unix" ou "epoch time", e é a origem do sistema de coordenadas que o `Instant` usa por dentro.

Sem um marco combinado, cada sistema escolheria o seu, e trocar datas entre eles viraria um problema de tradução. A alternativa histórica era guardar datas como texto formatado — "10/09/2026 15:00" —, o que exige decidir o formato, tratar idioma, cuidar da ordenação (texto ordena errado: "02/01" vem depois de "01/12") e fazer parsing toda vez que se quer comparar. Planilhas e linguagens antigas chegaram a adotar epochs diferentes (algumas contam a partir de 1900), e converter entre elas é fonte clássica de erro de um dia.

Com um epoch único, uma data vira um `long`: compacto, sem fuso embutido, trivial de comparar e subtrair. `Instant.getEpochSecond()` devolve esse número; `Instant.ofEpochSecond(n)` faz o caminho de volta; a constante `Instant.EPOCH` é o próprio instante zero.

```java
Instant agora = Instant.now();
long timestamp = agora.getEpochSecond();   // ex.: 1789000000
System.out.println("Guardando no banco: " + timestamp);

Instant reconstruido = Instant.ofEpochSecond(timestamp);
```

Na prática, é assim que muita coisa viaja. Uma coluna `criado_em` definida como `BIGINT` guarda epoch seconds; os campos `iat` e `exp` de um token JWT são epoch seconds; uma resposta de API devolve `"timestamp": 1789000000` em vez de uma string de data. Todos concordam porque todos contam a partir do mesmo 1970.

A alternativa é o texto no padrão ISO-8601 ("2026-09-10T15:00:00Z"), que é legível por humanos e ordena corretamente quando bem formado, ao custo de ocupar mais espaço e exigir parsing. Para resolução mais fina que o segundo, guarda-se epoch em milissegundos, microssegundos ou nanossegundos — e aqui mora a pegadinha mais comum: misturar segundos com milissegundos, errando por um fator de mil (`ofEpochSecond` recebendo um valor que era `ofEpochMilli`). O outro cuidado é o tamanho do inteiro: epoch seconds em um `int` de 32 bits com sinal estoura em janeiro de 2038 — o "problema do ano 2038". Guarde sempre em 64 bits (`long`), nunca em `int`.

### UTC

UTC, sigla de Tempo Universal Coordenado, é o padrão mundial de tempo — a referência de deslocamento zero em relação à qual todos os fusos horários são definidos. O horário de Brasília é "UTC menos 3 horas"; o de Tóquio é "UTC mais 9". Um `Instant` está sempre implicitamente em UTC, e é o "Z" (lido "Zulu") que aparece no fim de uma data ISO-8601 como `2026-09-10T18:00:00Z`. UTC é o que permite que o instante seja um ponto absoluto, e não "seis da tarde em algum lugar".

O problema de não adotar UTC aparece assim que o sistema cresce. Se cada servidor grava horário local, "14:00" não diz nada sozinho — 14:00 onde? Dois servidores em regiões diferentes discordam sobre a ordem de dois eventos. Pior, no dia em que o relógio local adianta ou atrasa por horário de verão, uma hora local pode não existir ou se repetir, e comparar registros gravados nesse intervalo dá resultado errado.

A solução padrão é: armazene e calcule em UTC, e converta para o fuso local só na hora de exibir. `Instant` já resolve o armazenamento, porque é sempre UTC. Quando você precisa de campos de calendário ainda em UTC, `ZonedDateTime.now(ZoneOffset.UTC)` ou `OffsetDateTime.now(ZoneOffset.UTC)`. Para mostrar ao usuário, `instante.atZone(ZoneId.of("America/Sao_Paulo"))` aplica o fuso e o horário de verão daquela região.

```java
Instant evento = Instant.now();   // já é UTC

// Guardar / logar: em UTC
log.info("Pedido criado em {}", evento);

// Exibir para o usuário de São Paulo
ZonedDateTime local = evento.atZone(ZoneId.of("America/Sao_Paulo"));
System.out.println("Criado às " + local.getHour() + "h");
```

Um caso concreto: num sistema distribuído, todos os servidores registram log em UTC, de modo que juntar os arquivos de várias máquinas produz uma linha do tempo única e coerente. O banco guarda timestamps em UTC; o front-end recebe o instante e converte para o fuso do navegador do usuário.

A alternativa é `OffsetDateTime` guardando o deslocamento explícito, útil quando você precisa preservar o offset original de onde o dado veio (uma API que informa "-03:00"). Quando o que importa é a hora "de parede" e não o instante absoluto — um alarme que deve tocar às 09:00 todo dia, um feriado — o certo é guardar hora local mais o `ZoneId`, não UTC. E datas puramente de calendário, como um aniversário, não precisam de UTC nem de instante: um `LocalDate` é a modelagem correta. Vale notar ainda que UTC ignora os "segundos bissextos" que a Terra ocasionalmente exige; a API `java.time` também os ignora, o que é adequado para praticamente todo software de negócio.

## Fusos horários

O capítulo anterior tratou o tempo como um ponto absoluto na linha do tempo, sem calendário e sem lugar. Este capítulo faz o caminho de volta: pega esse instante absoluto e devolve a ele a informação de "onde", para poder dizer que horas eram naquele momento em São Paulo, em Lisboa ou em Tóquio. É essa conversão que aparece toda vez que um sistema guarda um evento em UTC e precisa exibi-lo no relógio de parede do usuário.

Cinco ideias sustentam o capítulo. `ZoneId` identifica uma região do mundo e carrega todas as regras de horário dela. `ZoneOffset` é o caso mais simples: apenas um deslocamento fixo em relação ao UTC, como "menos três horas". `ZonedDateTime` junta data, hora e `ZoneId` num carimbo humano completo, com as regras da região aplicadas. `OffsetDateTime` faz quase o mesmo, mas guardando só o deslocamento, sem a região. E horário de verão é o fenômeno que torna tudo isso complicado e explica por que essas classes precisam existir separadas.

### `ZoneId`

`ZoneId` é a classe de `java.time` que representa um fuso horário como uma região geográfica — `"America/Sao_Paulo"`, `"Europe/Lisbon"`, `"Asia/Tokyo"`. Não é um número: é uma chave para um conjunto de regras mantido no "tz database" (a base de fusos da IANA, que vem embutida no JDK e é atualizada a cada versão). Essas regras dizem qual é o deslocamento atual da região em relação ao UTC, quando esse deslocamento já mudou no passado e quando muda por horário de verão.

Sem `ZoneId`, as classes vistas até aqui não fecham a conta. `LocalDateTime` tem os campos de calendário mas não sabe a que fuso pertencem; `Instant` sabe o instante absoluto mas não tem calendário. Para transformar um `Instant` gravado no banco em "10h30 da manhã do dia 15" para um usuário específico, é preciso informar a região — e junto vêm todas as particularidades históricas dela, como o fato de o Brasil ter tido horário de verão até 2019 e não ter mais.

Você obtém um `ZoneId` com `ZoneId.of("America/Sao_Paulo")`, pega o do sistema com `ZoneId.systemDefault()` e lista todos com `ZoneId.getAvailableZoneIds()`. Ele entra em cena nos métodos de conversão: `instante.atZone(zona)` e `localDateTime.atZone(zona)`.

```java
Instant evento = Instant.now();                 // absoluto, em UTC
ZoneId saoPaulo = ZoneId.of("America/Sao_Paulo");

ZonedDateTime local = evento.atZone(saoPaulo);
System.out.println("Aconteceu às " + local.getHour() + "h para o usuário");
```

O uso real é a camada de exibição: o back-end trabalha em UTC, e só na hora de montar a tela ou o e-mail aplica o `ZoneId` do usuário. Uma analogia ajuda a fixar a diferença para o próximo conceito: um `ZoneId` de região é como um endereço postal — "São Paulo, Brasil" —, que continua válido mesmo quando as regras locais mudam; um deslocamento fixo é como anotar apenas a coordenada de longitude naquele instante. O endereço sabe se responder a uma pergunta sobre outra data do ano; a coordenada isolada, não.

Como essas regras mudam sempre que algum país altera sua legislação, manter a versão do Java atualizada (ou aplicar o utilitário `tzupdater`) é uma tarefa operacional real: uma regra desatualizada faz conversões futuras saírem com uma hora de erro.

A alternativa é `ZoneOffset`, um deslocamento fixo — mais leve, mas sem as regras que mudam ao longo do ano; o próximo conceito trata de quando ele basta e quando trai. O cuidado oposto é não adicionar `ZoneId` a dados que não têm lugar: uma data de aniversário é `LocalDate`, e um carimbo de log para armazenar é `Instant` — colocar fuso neles é modelagem errada.

### `ZoneOffset`

`ZoneOffset` é uma subclasse de `ZoneId` que representa a forma mais simples de fuso: um deslocamento fixo em relação ao UTC, expresso em horas, minutos e segundos, como `-03:00` ou `+09:00`. Ele não tem região, não tem nome de cidade e não tem regras que mudam ao longo do ano — é só o número. `ZoneOffset.UTC` é a constante para o deslocamento zero.

O problema que ele resolve aparece quando tudo o que você tem, ou tudo o que precisa, é o deslocamento em si. Uma API externa devolve `"2026-09-03T15:00:00-03:00"`: o que está ali é um offset, não a informação de que a origem é São Paulo, Buenos Aires ou qualquer outra cidade que naquele momento estava a três horas do UTC. Forçar um `ZoneId` de região nesse caso seria inventar uma informação que o dado não trouxe.

Você cria um `ZoneOffset` com `ZoneOffset.ofHours(-3)`, `ZoneOffset.ofHoursMinutes(5, 30)` ou `ZoneOffset.of("-03:00")`. Ele é o tipo de fuso que o `OffsetDateTime` guarda por dentro, e também serve onde um `ZoneId` é esperado — `Instant.now().atZone(ZoneOffset.UTC)` é válido. Como não há regras a consultar, dois `ZoneOffset` iguais em valor são iguais entre si, e a comparação é só aritmética de segundos.

```java
OffsetDateTime agoraUtc = OffsetDateTime.now(ZoneOffset.UTC);
OffsetDateTime recebido = OffsetDateTime.parse("2026-09-03T15:00:00-03:00");

Instant instante = recebido.toInstant();   // volta ao ponto absoluto
```

Na prática, `ZoneOffset` é o que se usa para carimbar algo "em UTC com campos de calendário" (`ZoneOffset.UTC`) e para preservar o offset exato que veio de fora. Um exemplo concreto: ao processar o cabeçalho de um e-mail ou um registro de outro sistema que já traz `-0300`, você guarda esse `ZoneOffset` para não perder de que deslocamento o dado veio, sem afirmar qual cidade o produziu.

A alternativa é o `ZoneId` de região, obrigatório quando a conversão precisa acompanhar o horário de verão. E é justamente aí que `ZoneOffset` não serve: se a região observa horário de verão, um offset fixo vai estar certo em metade do ano e errado na outra metade, porque o deslocamento real muda de `-03:00` para `-02:00` e volta. Guardar `ZoneOffset` para representar "o fuso do usuário" é, por isso, um erro comum — o que se quer ali é o `ZoneId`.

### `ZonedDateTime`

`ZonedDateTime` é o carimbo de tempo humano completo: data, hora e um `ZoneId`, com as regras da região já aplicadas para resolver qual é o deslocamento válido naquele momento. É a junção de um `LocalDateTime` com um `ZoneId`, e dele se pode extrair tanto os campos de calendário ("15 de fevereiro, quarta-feira, 10h") quanto o `Instant` absoluto correspondente.

Sem ele, sobra uma escolha ruim. `LocalDateTime` com o valor `2026-02-15T10:00` não diz nada globalmente — 10h de onde? Já o `Instant` é um ponto absoluto sem dia da semana e sem "manhã". `ZonedDateTime` é o tipo que carrega as duas coisas ao mesmo tempo, e por isso é o certo para representar um compromisso: "reunião às 9h no horário de Brasília". É como uma passagem aérea, que precisa dizer ao mesmo tempo a hora que aparece no relógio do aeroporto e um instante real inequívoco, para que a conexão feche.

Os pontos de entrada são `ZonedDateTime.now(zona)`, `instante.atZone(zona)`, `localDateTime.atZone(zona)` e `ZonedDateTime.of(2026, 2, 15, 9, 0, 0, 0, zona)`. A aritmética dele respeita o horário de verão: `plusDays(1)` mantém a hora de parede (continua 9h mesmo que o relógio da região tenha mudado no intervalo), enquanto `plusHours(24)` avança 24 horas reais e pode cair em outra hora de parede. Para converter para outro fuso preservando o instante, `withZoneSameInstant(outraZona)`.

```java
ZoneId brasilia = ZoneId.of("America/Sao_Paulo");
ZoneId toquio = ZoneId.of("Asia/Tokyo");

ZonedDateTime reuniao = ZonedDateTime.of(2026, 3, 10, 9, 0, 0, 0, brasilia);
ZonedDateTime emToquio = reuniao.withZoneSameInstant(toquio);

System.out.println("9h em Brasília = " + emToquio.getHour() + "h em Tóquio");
```

O uso real é agendamento e exibição envolvendo mais de um fuso: marcar um evento no horário de um participante e mostrar a cada outro participante o horário local dele; ou uma tarefa que deve rodar "todo dia à meia-noite no horário da sede" e precisa acompanhar as viradas de relógio dessa região. A alternativa mais enxuta é `OffsetDateTime`, quando basta o deslocamento e não interessa a região. E `ZonedDateTime` não é o tipo para guardar em banco: além de ocupar mais espaço, ele grava o nome da região, que pode não existir ou ter sido renomeada em outro sistema — para armazenamento, converta para `Instant` (ou `OffsetDateTime` em UTC) e deixe o `ZoneId` do usuário na camada de apresentação.

### `OffsetDateTime`

`OffsetDateTime` é data mais hora mais um `ZoneOffset` fixo — como o `ZonedDateTime`, mas sem a região e sem o conjunto de regras que vêm com ela. O que ele guarda é exatamente `2026-09-03T15:00:00-03:00`: os campos de calendário e o deslocamento em relação ao UTC, nada além disso. Esse formato corresponde ao `TIMESTAMP WITH TIME ZONE` do SQL e à representação ISO-8601 com offset que a maioria das APIs REST usa.

O problema que ele resolve é de proporção. `ZonedDateTime` carrega uma região inteira e todo o histórico de mudanças de relógio dela — mais informação do que se precisa quando o objetivo é apenas registrar "isto aconteceu neste deslocamento". Serializar um `ZoneId` também é mais pesado e pode variar entre sistemas (o nome `"America/Sao_Paulo"` precisa existir e significar o mesmo dos dois lados); um offset `-03:00` é inequívoco em qualquer lugar. `OffsetDateTime` é, então, o meio-termo entre `LocalDateTime` (campos de calendário sem nenhuma âncora) e `Instant` (âncora absoluta sem campos de calendário): tem os dois, sem o peso da região.

A construção parte de `OffsetDateTime.now()`, `OffsetDateTime.now(ZoneOffset.UTC)`, `OffsetDateTime.of(dataHoraLocal, offset)` ou `zonedDateTime.toOffsetDateTime()`. A conversão para o ponto absoluto é direta com `toInstant()`.

```java
// API devolvendo um instante com o offset de origem preservado
OffsetDateTime criadoEm = OffsetDateTime.parse("2026-09-03T15:00:00-03:00");

// Persistir de forma canônica: em UTC
OffsetDateTime emUtc = criadoEm.withOffsetSameInstant(ZoneOffset.UTC);
```

Na prática, `OffsetDateTime` é o tipo natural na fronteira do sistema: o que entra e sai de uma API JSON, o que vai para uma coluna de timestamp com fuso no banco. Drivers JDBC modernos mapeiam `OffsetDateTime` diretamente para colunas `TIMESTAMP WITH TIME ZONE`, e frameworks de serialização o convertem para a string ISO-8601 com offset sem configuração extra — é por essa integração pronta que ele costuma ser a primeira escolha em DTOs e entidades.

A alternativa mais rigorosa para armazenamento é `Instant`, que é sempre UTC e não tem campos locais para alguém interpretar errado; a mais completa é `ZonedDateTime`, quando a região importa. E `OffsetDateTime` é a escolha errada quando o cálculo precisa cruzar uma virada de horário de verão mantendo a hora de parede correta — aí só `ZonedDateTime`, com sua `ZoneId`, sabe quando o relógio muda. Um `OffsetDateTime` com `-03:00` não faz ideia de que amanhã aquele lugar passará a `-02:00`.

### Horário de verão

Horário de verão é a prática de adiantar o relógio, normalmente em uma hora, durante parte do ano, para aproveitar melhor a luz do dia. Não é uma classe de Java: é o fenômeno que a API `java.time` modela por dentro das regras de cada `ZoneId`. O Brasil aboliu o horário de verão em 2019, mas as regras continuam relevantes para converter datas anteriores a essa mudança e para lidar com países que ainda o adotam, como boa parte da Europa e dos Estados Unidos.

O problema que ele cria é que, duas vezes por ano, a linha do tempo local deixa de ser contínua. No dia em que o relógio adianta, existe um buraco: das 00h às 01h simplesmente não acontece, e um horário como 00h30 não existe naquela data. No dia em que o relógio atrasa, há uma sobreposição: 23h30 acontece duas vezes, com deslocamentos diferentes. Código que ignora isso grava instantes errados, dispara uma tarefa agendada duas vezes ou lança exceção ao tentar montar uma data impossível.

`java.time` resolve esses casos de forma previsível quando você usa `ZonedDateTime`. Se a hora local cai no buraco, ela é empurrada para frente pelo tamanho do salto; se cai na sobreposição, a API escolhe o primeiro dos dois deslocamentos, e você pode forçar o outro com `withLaterOffsetAtOverlap()`. Somar tempo também tem duas semânticas: `plusDays(1)` mantém a hora de parede atravessando a virada, enquanto somar via `Duration` ou converter para `Instant` mantém o número de horas reais. Para inspecionar as regras diretamente, existe `zona.getRules()`.

```java
ZoneId ny = ZoneId.of("America/New_York");

// 2026-03-08 02:30 não existe em Nova York (relógio pula de 02h para 03h)
LocalDateTime local = LocalDateTime.of(2026, 3, 8, 2, 30);
ZonedDateTime ajustado = local.atZone(ny);

System.out.println(ajustado);   // vira 03:30-04:00, empurrado para frente
```

Um caso concreto: um alarme diário às 02h30 configurado para um usuário de Nova York precisa decidir o que fazer no dia em que 02h30 não existe — e a regra de empurrar para frente dá a resposta. Uma reunião recorrente "toda segunda às 9h" deve ser guardada como hora local mais `ZoneId`, para que continue às 9h de parede mesmo depois da virada. Já um evento que é um instante absoluto — o horário exato de um lançamento simultâneo mundial — deve ser guardado como `Instant`, e nesse caso o horário de verão não entra na conta em momento nenhum: instantes absolutos e datas puras de calendário (`LocalDate`) nunca precisam desse raciocínio.

## Formatação temporal

Os capítulos anteriores construíram os objetos de data e hora e ensinaram a calcular com eles. Falta a ponte com o mundo externo: nenhum sistema real trabalha só com objetos `LocalDate` na memória. Datas chegam como texto — vindas de um formulário, de um arquivo CSV, de um corpo JSON, de um cabeçalho HTTP — e precisam sair como texto para serem exibidas na tela ou gravadas num relatório. Este capítulo trata dessas duas conversões, texto para objeto e objeto para texto, e da classe que faz as duas: `DateTimeFormatter`.

As três primeiras seções são o núcleo prático. `DateTimeFormatter` apresenta a classe, seus formatadores prontos e como montar um padrão próprio. "Parsing" foca no sentido texto para objeto e nos erros que ele provoca. "Formatação" foca no sentido inverso, objeto para texto, com localização por idioma. As duas últimas seções mudam de assunto: `Clock` e "Testabilidade temporal" tratam de como o programa lê a hora atual — um detalhe de design que decide se o código que usa data e hora pode ser testado de forma confiável.

### `DateTimeFormatter`

`DateTimeFormatter` é a classe da API `java.time` responsável por traduzir entre objetos temporais e texto, nas duas direções. Ela substitui o antigo `SimpleDateFormat`, que era a ferramenta equivalente na API de datas anterior ao Java 8. A diferença que mais importa no dia a dia é que `DateTimeFormatter` é imutável e seguro para uso concorrente: uma única instância pode ser guardada em um campo `static final` e compartilhada por todas as threads da aplicação. `SimpleDateFormat` não oferecia isso — cada thread precisava da sua cópia, e esquecer disso gerava bugs difíceis de reproduzir, com datas saindo corrompidas sob carga.

Sem um formatador explícito, você fica preso ao formato ISO 8601, que é o que os métodos `toString` e `parse` dos tipos `java.time` usam por padrão: `2026-09-03`, `2026-09-03T14:30:00`. É um formato ótimo para troca de dados entre sistemas, mas quase nunca é o que se mostra a uma pessoa ou o que um sistema legado espera receber. Para qualquer coisa diferente de ISO — uma data no formato brasileiro `03/09/2026`, um mês por extenso, um carimbo com fuso para um log — é preciso um `DateTimeFormatter` configurado.

Há três formas de obter um. A primeira são as constantes prontas da própria classe, como `DateTimeFormatter.ISO_LOCAL_DATE` e `DateTimeFormatter.ISO_OFFSET_DATE_TIME`, úteis quando o formato desejado já é um padrão ISO específico. A segunda, e mais comum, é `DateTimeFormatter.ofPattern("dd/MM/yyyy")`, que monta o formatador a partir de uma string de padrão, onde cada letra tem um significado: `d` é dia, `M` é mês, `y` é ano, `H` é hora em 24h, `h` é hora em 12h, `m` é minuto, `s` é segundo. A repetição da letra controla o formato: `MM` dá `09`, `MMM` dá `set.`, `MMMM` dá `setembro`. Letras têm caixa significativa — `M` maiúsculo é mês, `m` minúsculo é minuto, e trocar os dois é o erro de digitação clássico. A terceira forma é `DateTimeFormatterBuilder`, para casos que o padrão em string não cobre, como campos opcionais ou valores padrão.

```java
DateTimeFormatter BR = DateTimeFormatter.ofPattern("dd/MM/yyyy");

LocalDate data = LocalDate.of(2026, 9, 3);
String texto = data.format(BR);        // "03/09/2026"

LocalDate volta = LocalDate.parse("25/12/2026", BR);   // 2026-12-25
```

Um caso real: uma API REST recebe no corpo JSON a data `"2026-09-03"` e responde para a tela do usuário com `"3 de setembro de 2026"`. São dois formatadores — um ISO na entrada, um localizado na saída — e ambos vivem como constantes na classe de serviço, criados uma vez.

A alternativa a construir um `DateTimeFormatter` é deixar a biblioteca de serialização cuidar disso. Frameworks como Jackson, quando configurados com o módulo `jackson-datatype-jsr310`, convertem `LocalDate` de e para JSON automaticamente, usando ISO por padrão ou um padrão declarado por anotação. Nesse cenário você quase não toca em `DateTimeFormatter` diretamente. Ele também é desnecessário quando ISO já serve: se os dois lados da comunicação são sistemas seus e ninguém precisa ler a data com os olhos, `data.toString()` e `LocalDate.parse(texto)` bastam, sem formatador nenhum.

### Parsing

*Parsing* é a conversão de texto em objeto temporal — o sentido de entrada de dados. Todo tipo da API `java.time` tem um método estático `parse`: `LocalDate.parse`, `LocalDateTime.parse`, `ZonedDateTime.parse` e assim por diante. Com um argumento só, o método assume que o texto está em formato ISO. Com um segundo argumento `DateTimeFormatter`, ele usa o padrão que você forneceu. O `DateTimeFormatter` também tem um `parse` próprio, de nível mais baixo, que devolve um `TemporalAccessor` genérico; na prática, chamar `parse` diretamente no tipo desejado é mais claro e menos sujeito a erro.

O problema central do parsing é que a entrada vem de fora e pode estar errada de muitas maneiras: uma data que não existe (`31/02/2026`), um mês fora de faixa, um texto num formato diferente do esperado, uma string vazia, um dia e mês trocados. Quando o texto não bate com o formato, `parse` lança `DateTimeParseException`, uma exceção não checada. Ignorar essa possibilidade significa deixar a exceção subir e derrubar a operação — o que pode ser aceitável num script, mas não numa API que precisa responder "campo data_nascimento inválido" com um código HTTP 400.

A forma correta de lidar com isso é envolver a chamada num `try`/`catch` de `DateTimeParseException` na fronteira onde o texto externo entra, e traduzir a falha para o vocabulário da sua aplicação — uma mensagem de validação, um valor default, um registro de log. A exceção carrega o texto que falhou e a posição do erro, úteis para diagnóstico. Um detalhe frequentemente esquecido: por padrão o parser é estrito quanto ao número de dígitos, então `ofPattern("dd/MM/yyyy")` rejeita `3/9/2026` porque espera dois dígitos em dia e mês. Se a entrada pode vir com um dígito só, o padrão precisa ser `d/M/yyyy`.

```java
DateTimeFormatter BR = DateTimeFormatter.ofPattern("dd/MM/yyyy");

try {
    LocalDate nascimento = LocalDate.parse(entradaDoUsuario, BR);
    // segue o fluxo normal
} catch (DateTimeParseException e) {
    throw new CampoInvalidoException("data_nascimento", "use o formato dd/mm/aaaa");
}
```

Um caso concreto: uma rotina que importa um arquivo CSV de 50 mil linhas, cada uma com uma coluna de data preenchida à mão. Fazer `parse` de cada célula dentro de um `try`/`catch` permite acumular as linhas defeituosas num relatório de erros e processar o resto, em vez de abortar a importação inteira na primeira data torta.

A alternativa ao parsing manual é, de novo, delegar à camada de serialização — Jackson, Spring — que faz o `parse` e devolve o erro de validação por você. Quando isso está em jogo, você configura o formato e deixa o framework chamar `parse`. E há o caso em que parsing simplesmente não deveria existir: se o dado nasce como data no banco, com uma coluna `DATE`, o driver JDBC entrega um `LocalDate` pronto via `getObject`, e transformar isso em texto para depois fazer `parse` de volta é retrabalho que só adiciona pontos de falha.

### Formatação

Formatação é o sentido de saída: transformar um objeto temporal em texto legível. O método está em todo tipo da API — `data.format(formatador)` — e existe também `formatador.format(data)`, que faz o mesmo. O resultado é sempre uma `String`, pronta para ir para a tela, um PDF, um e-mail ou uma célula de planilha.

Sem um formatador, a única saída disponível é `toString()`, que produz ISO 8601. `2026-09-03T14:30` é inequívoco para uma máquina, mas um usuário brasileiro espera `03/09/2026 14:30`, um americano espera `9/3/2026 2:30 PM`, e um relatório formal pode pedir `3 de setembro de 2026`. Cada público quer um formato diferente da mesma informação, e é isso que a formatação resolve.

Além do padrão em letras já visto, a formatação tem uma dimensão de localização. `DateTimeFormatter.ofLocalizedDate(FormatStyle.LONG)` não fixa um padrão: ele pede à JVM o formato longo de data convencional para o `Locale` ativo. Com `Locale` brasileiro, sai "3 de setembro de 2026"; com `Locale` americano, "September 3, 2026". Você aplica o idioma com `.withLocale(Locale.of("pt", "BR"))` sobre o formatador. Isso vale também para nomes de mês e de dia da semana num padrão comum: `ofPattern("EEEE, dd 'de' MMMM", Locale.of("pt", "BR"))` produz "quinta-feira, 03 de setembro". As aspas simples no padrão marcam texto literal, que não deve ser interpretado como letras de campo.

```java
DateTimeFormatter EXTENSO =
    DateTimeFormatter.ofPattern("dd 'de' MMMM 'de' yyyy", Locale.of("pt", "BR"));

LocalDate data = LocalDate.of(2026, 9, 3);
System.out.println(data.format(EXTENSO));   // "03 de setembro de 2026"
```

Um caso real: um sistema de emissão de boletos gera o texto "Vencimento: 15 de outubro de 2026" no documento e, ao mesmo tempo, grava `2026-10-15` no arquivo de remessa para o banco. Mesmo objeto `LocalDate`, dois formatadores, um localizado para a pessoa e um ISO para o sistema.

A alternativa mais tosca é montar a string na mão com concatenação — `dia + "/" + mes + "/" + ano` — que funciona para o caso trivial mas ignora zero à esquerda, nomes de mês e localização, e vira uma bola de neve assim que o formato muda. Formatação explícita também é desnecessária quando o destino do texto é outro sistema que espera ISO: aí `toString()` já entrega exatamente o que é preciso.

### `Clock`

`Clock` é a classe da API `java.time` que representa a fonte da hora atual. Todos os métodos `now()` — `LocalDate.now()`, `Instant.now()`, `ZonedDateTime.now()` — existem em duas versões: sem argumento, que consulta o relógio do sistema operacional no fuso default da JVM, e recebendo um `Clock`, que consulta o objeto que você passou. O `Clock` encapsula duas informações: de onde vem o instante atual e qual o fuso horário associado.

O problema que ele resolve aparece quando `now()` sem argumento está espalhado pelo código. Cada chamada dessas é uma dependência escondida de dois estados globais e mutáveis: o relógio da máquina e o fuso configurado na JVM. Isso torna o comportamento do programa não determinístico — o mesmo método roda diferente conforme o dia — e praticamente impossível de testar cenários que dependem de data, como "o que acontece no último dia do mês" ou "esta promoção expira à meia-noite". Também dificulta rodar a mesma aplicação com um fuso fixo independente da configuração da máquina, algo que servidores frequentemente precisam.

A solução é tratar o `Clock` como uma dependência explícita: a classe recebe um `Clock` no construtor e, internamente, sempre chama `LocalDate.now(clock)` em vez de `LocalDate.now()`. Em produção, injeta-se `Clock.systemDefaultZone()` ou `Clock.system(ZoneId.of("America/Sao_Paulo"))`. Em teste, injeta-se `Clock.fixed(instante, zona)`, que sempre devolve o mesmo instante, ou `Clock.offset(base, duração)`, que desloca um relógio existente. Frameworks de injeção de dependência facilitam isso ao registrar um `Clock` como bean único da aplicação.

```java
public class AssinaturaService {
    private final Clock clock;

    public AssinaturaService(Clock clock) {
        this.clock = clock;
    }

    public boolean estaAtiva(Assinatura a) {
        return LocalDate.now(clock).isBefore(a.getVencimento());
    }
}
```

Um caso concreto: um serviço que aplica juros a partir do primeiro dia útil após o vencimento. Sem `Clock`, testar isso exige esperar a data certa ou mexer no relógio da máquina de build; com um `Clock` injetado, o teste fixa o dia exato — o próximo conceito mostra esse teste verificando os dois lados da fronteira.

A alternativa é passar a data como parâmetro do método em vez de injetar um `Clock` — `estaAtiva(Assinatura a, LocalDate hoje)`. Funciona e é ainda mais explícito, mas empurra a responsabilidade de obter "hoje" para quem chama, o que espalha o problema em vez de centralizá-lo. Para aplicações pequenas, ou trechos onde a hora exata não muda o resultado — um log de depuração, uma métrica aproximada —, `Instant.now()` direto é aceitável e o `Clock` seria cerimônia sem retorno.

### Testabilidade temporal

Testabilidade temporal é a propriedade de um código que lida com data e hora poder ser verificado por testes automatizados de forma rápida e repetível. Ela não é uma classe nem um método: é uma consequência de decisões de design, e é o motivo pelo qual `Clock` foi adicionado à API `java.time` desde o início. O tema fecha o capítulo porque amarra tudo o que veio antes — formatar, fazer parse, calcular com datas — à pergunta prática de como garantir que essa lógica está correta.

A dependência escondida de `now()` sem argumento já apareceu com `Clock`; do lado do teste, ela cobra um preço próprio. Um teste de código assim só consegue afirmar coisas frágeis: "o resultado não é nulo", "a data está dentro de uma faixa de alguns segundos". Cenários que são exatamente os que mais dão bug — a virada do mês, o ano bissexto, o fim do horário de verão, a expiração no segundo exato — ficam fora de alcance, porque o teste não tem como colocar o programa naquele instante. O resultado é que a parte mais escorregadia do sistema é a menos coberta.

A técnica que resolve é a inversão de controle sobre o tempo já vista com `Clock`: nenhuma classe de regra de negócio chama `now()` sem argumento, todas recebem a fonte da hora de fora. O que muda no teste é o proveito disso — injeta-se `Clock.fixed(Instant.parse("2026-02-28T23:59:59Z"), ZoneOffset.UTC)` e verifica-se o comportamento exatamente naquele ponto e um segundo depois. Como o relógio é fixo, o teste roda em microssegundos e dá o mesmo resultado toda vez, em qualquer máquina, em qualquer fuso.

```java
@Test
void juroComecaNoDiaSeguinteAoVencimento() {
    Clock noVencimento = Clock.fixed(
        Instant.parse("2026-10-15T12:00:00Z"), ZoneOffset.UTC);
    Clock umDiaDepois = Clock.fixed(
        Instant.parse("2026-10-16T12:00:00Z"), ZoneOffset.UTC);

    var titulo = new Titulo(LocalDate.of(2026, 10, 15));

    assertFalse(new CalculadoraJuros(noVencimento).temJuros(titulo));
    assertTrue(new CalculadoraJuros(umDiaDepois).temJuros(titulo));
}
```

Um caso concreto: um relatório mensal que roda no primeiro dia de cada mês e consolida o mês anterior. O bug clássico é errar a fronteira em janeiro, quando "mês anterior" é dezembro do ano passado. Com `Clock.fixed` em 1º de janeiro, o teste pega esse erro sem esperar a virada do ano.

A alternativa de passar `LocalDate`/`Instant` como parâmetro dos métodos, em vez de injetar um `Clock`, já apareceu no conceito anterior e vale igual aqui: o código fica testável do mesmo jeito. Bibliotecas como o `MutableClock` de projetos de teste, ou ferramentas mais invasivas que interceptam chamadas estáticas, existem mas raramente compensam a complexidade. E há código onde investir nisso não se paga: um utilitário que só formata uma data para exibição não tem lógica dependente de "agora" e não precisa de `Clock` nenhum para ser testado — basta passar uma data de entrada e conferir a string de saída.

---

# Módulo 8 — Exceções profundamente

Erros acontecem. Java oferece um mecanismo robusto de exceções que força o programador a pensar sobre recuperação e falhas. Este módulo explora a hierarquia de exceções, a distinção entre checked e unchecked, como lançar e propagar exceções, como criar tipos de exceção customizados, e como usar recursos (`AutoCloseable`, try-with-resources) de forma segura.

## Hierarquia das exceções

Toda falha sinalizada em Java é um objeto, e todos esses objetos descendem de uma mesma raiz. Entender essa árvore — quem herda de quem e o que cada nível representa — é o que permite escrever `catch` que captura exatamente o que se pretende tratar, nem mais nem menos. Um `catch` largo demais engole problemas que deveriam derrubar o programa; um `catch` estreito demais deixa passar o que se queria recuperar.

Esta aula percorre os quatro tipos que estruturam a hierarquia, de cima para baixo: `Throwable` na raiz, `Error` e `Exception` como os dois grandes ramos logo abaixo dela, e `RuntimeException` como a subdivisão de `Exception` que muda a forma como o compilador trata o seu código. Os quatro nomes aparecem o tempo todo em mensagens de erro e assinaturas de método, e reconhecer a posição de cada um na árvore é pré-requisito para tudo o que vem nas aulas seguintes.

### `Throwable`

`Throwable` é a classe raiz de toda a hierarquia de exceções. Ela é a única coisa que a instrução `throw` aceita lançar e a única coisa que um bloco `catch` consegue capturar: se um objeto não é um `Throwable` nem descende de um, ele simplesmente não participa do mecanismo de exceções da linguagem. Todas as outras classes desta aula — `Error`, `Exception`, `RuntimeException` — herdam dela, direta ou indiretamente.

Um `Throwable` carrega quatro informações. A mensagem de detalhe, um texto curto que descreve a falha, recuperável por `getMessage()`. A causa, outro `Throwable` que originou este, acessível por `getCause()` e central na aula sobre encadeamento. O rastro de pilha (*stack trace*), a sequência de chamadas de método que levou até o ponto do lançamento, disponível por `getStackTrace()` e impressa por `printStackTrace()`. E o vetor de exceções suprimidas, preenchido pelo try-with-resources e detalhado na última aula deste módulo.

Sem uma raiz comum, cada `catch` precisaria enumerar tipos sem parentesco entre si, e nenhum código genérico — um logger, um handler global de requisições, um framework de testes — conseguiria dizer "me entregue qualquer falha que aconteça aqui". A raiz única é o que torna possível a linha `catch (Throwable t)` no topo de uma thread, para registrar o que quer que tenha dado errado antes de o programa encerrar. É uma situação parecida com a de um sistema de arquivos que define um tipo base "documento": cada formulário específico continua sendo um documento, então o setor de expedição consegue transportar qualquer um deles sem saber o que é. Da mesma forma, o handler de topo transporta qualquer `Throwable` sem saber qual subtipo chegou.

```java
public void run() {
    try {
        processarLote();
    } catch (Throwable t) {          // pega Error e Exception
        log.error("Falha não tratada no worker", t);
        throw t;                     // relança para não mascarar
    }
}
```

Estender `Throwable` diretamente é possível, mas quase nunca é o que se quer: as exceções de aplicação herdam de `Exception` ou de `RuntimeException`, que já trazem o comportamento certo. Capturar `Throwable` no meio do código normal também é desaconselhado, porque nessa altura você está pegando `Error` junto — coisas como falta de memória, que o seu `catch` não tem como consertar. O uso legítimo de `Throwable` como tipo de captura fica restrito a pontos de fronteira: o topo de uma thread, um loop de servidor que não pode morrer por causa de uma requisição, um teste que quer afirmar que nada foi lançado.

### `Error`

`Error` é a subclasse de `Throwable` reservada para problemas graves originados fora do controle da aplicação, quase sempre na própria máquina virtual. Os exemplos mais conhecidos são `OutOfMemoryError`, lançado quando o *heap* se esgota; `StackOverflowError`, quando a pilha de chamadas estoura, tipicamente por recursão infinita; e `NoClassDefFoundError`, quando uma classe que existia em tempo de compilação some em tempo de execução. São situações em que o ambiente onde o programa roda deixou de oferecer as condições mínimas para continuar.

A convenção da plataforma é que `Error` não deve ser capturado nem tratado pelo código de aplicação. O raciocínio é direto: se o heap acabou, um `catch (OutOfMemoryError e)` provavelmente não consegue nem alocar a string da mensagem de log; se a pilha estourou, qualquer chamada de método adicional volta a estourar. Não há recuperação sensata a fazer no ponto da falha. Diferente de `Exception`, `Error` é não checado — o compilador não obriga a declarar nem a capturar — justamente porque não se espera que você faça nada a respeito.

O problema que `Error` resolve é de classificação. Sem essa separação, um `catch (Exception e)` genérico — comum em laços de processamento que não podem parar por causa de um item ruim — acabaria engolindo também a notícia de que a JVM está morrendo, e o programa seguiria tentando trabalhar num ambiente inviável, produzindo resultados errados em silêncio. Ao deixar `Error` num ramo irmão de `Exception`, a linguagem garante que `catch (Exception e)` nunca captura uma falha de infraestrutura por acidente.

```java
for (Pedido p : pedidos) {
    try {
        processar(p);
    } catch (Exception e) {                 // item ruim: registra e segue
        log.warn("Pedido {} falhou", p.id(), e);
    }
    // OutOfMemoryError NÃO cai aqui: interrompe o laço e sobe
}
```

É como a diferença entre um caixa de supermercado que rejeita um produto sem código de barras e continua o atendimento, e um incêndio na loja: o primeiro é um caso previsto da operação, o segundo exige evacuar, não "tratar e seguir". `Error` é o incêndio.

Há duas exceções práticas a essa regra. Frameworks de teste como o JUnit capturam `Throwable` (portanto `Error`) para reportar a falha do teste em vez de derrubar o runner. E `StackOverflowError`, por ser recuperável em alguns cenários — a pilha se desfaz ao voltar — é ocasionalmente capturado em interpretadores e parsers que impõem um limite de profundidade. Fora disso, o código de aplicação ignora `Error` e deixa a JVM seguir seu curso.

### `Exception`

`Exception` é o ramo da hierarquia destinado a condições anormais que um programa bem-escrito pode antecipar e das quais pode se recuperar. É daqui que descende praticamente tudo o que você vai capturar e tratar de propósito: `IOException` quando um arquivo não abre, `SQLException` quando uma consulta falha, `ParseException` quando um texto não segue o formato esperado. Também é a superclasse indireta de `RuntimeException`, tratada na próxima seção, e a base natural das exceções de domínio da aula sobre exceções customizadas.

O traço que define `Exception` — e que a distingue de `RuntimeException`, sua subclasse — é ser *checada*. O compilador exige que todo método que possa lançar uma `Exception` que não seja `RuntimeException` ou declare isso na assinatura com `throws`, ou capture a exceção internamente. Não há como ignorar a possibilidade por descuido: o código nem compila. Essa obrigação é o mecanismo pelo qual a linguagem transforma "este método pode falhar" num contrato visível, verificado, que acompanha a assinatura.

O problema que isso ataca é o da falha silenciosamente ignorada. Em linguagens sem exceções checadas, é comum um programador chamar uma função de I/O sem perceber que ela pode falhar, e só descobrir em produção. Com `Exception` checada, a chamada de `Files.readString(path)` não passa pela compilação enquanto você não decidir, ali, o que fazer se o arquivo não existir. A decisão pode ser tratar na hora, pode ser propagar para quem chamou — mas tem que ser tomada.

```java
public Config carregar(Path arquivo) throws IOException {
    String texto = Files.readString(arquivo);   // pode lançar IOException
    return Config.parse(texto);
}
```

Aqui o método assume que não sabe lidar com um arquivo ausente e declara `throws IOException`, empurrando a decisão para o chamador. Quem chamar `carregar` enfrenta a mesma escolha, e assim por diante até algum ponto onde tratar faça sentido — por exemplo, a camada que mostra "arquivo de configuração não encontrado" para o usuário.

A analogia é a de um formulário que exige assinatura em cada página: você não consegue avançar o processo deixando uma página em branco. É burocrático, e essa é exatamente a crítica que se faz às exceções checadas quando usadas em excesso. A alternativa que a própria linguagem oferece é `RuntimeException`, para falhas que representam defeitos de programação e não condições que o chamador deva tratar caso a caso. Quando não usar `Exception` checada: para erros que indicam bug — um argumento nulo onde não podia, um índice fora de faixa — em que obrigar cada chamador a escrever `try`/`catch` só polui o código sem tornar nada mais robusto.

### `RuntimeException`

`RuntimeException` é uma subclasse de `Exception` que reverte a regra da obrigatoriedade: exceções deste ramo são *não checadas*. O compilador não exige declará-las com `throws` nem capturá-las. Elas podem ser lançadas de qualquer método e propagam-se livremente pela pilha até encontrar um `catch` compatível ou chegar ao topo da thread. Os exemplos são o vocabulário diário de quem programa em Java: `NullPointerException`, `IllegalArgumentException`, `IllegalStateException`, `IndexOutOfBoundsException`, `ArithmeticException`, `ClassCastException`, `NumberFormatException`.

A intenção por trás desse ramo é separar dois tipos de falha que têm naturezas diferentes. `Exception` checada modela condições externas previsíveis — o arquivo pode não existir, a rede pode cair. `RuntimeException` modela defeitos no próprio código — você desreferenciou algo que era nulo, passou um argumento inválido, chamou um método num objeto em estado errado. A diferença prática é o que o chamador deveria fazer a respeito. Diante de uma `IOException`, faz sentido ter um plano B. Diante de uma `NullPointerException`, não há plano B razoável no ponto da chamada: o certo é corrigir o bug que produziu o nulo.

Se `RuntimeException` também fosse checada, cada acesso a um método — que sempre pode, em tese, encontrar um estado inválido — precisaria ser embrulhado em `try`/`catch` ou declarado com `throws`. O código ficaria dominado por tratamento de erros para situações que nunca deveriam acontecer se o programa estivesse correto. Ao deixá-las não checadas, a linguagem permite que esses defeitos apareçam de forma barulhenta durante o desenvolvimento — a `NullPointerException` estoura, você vê o stack trace, conserta a origem — sem impor cerimônia ao código correto.

```java
public void transferir(Conta origem, Conta destino, BigDecimal valor) {
    if (valor.signum() <= 0)
        throw new IllegalArgumentException("valor deve ser positivo: " + valor);
    if (origem.saldo().compareTo(valor) < 0)
        throw new IllegalStateException("saldo insuficiente na conta " + origem.id());
    origem.debitar(valor);
    destino.creditar(valor);
}
```

Os dois `throw` aqui sinalizam usos incorretos da API. Nenhum chamador é obrigado pelo compilador a tratá-los, e é assim que se quer: quem chama `transferir` com valor negativo tem um bug para corrigir, não um caso de negócio para contornar.

Pense na diferença entre a fechadura da porta emperrar por causa do frio — um imprevisto do ambiente, para o qual você tem um WD-40 no armário — e você tentar abrir a porta com a chave do carro. O segundo caso não se resolve com um plano B na hora; resolve-se não fazendo. `RuntimeException` é a chave errada. A alternativa, quando a falha *é* um caso previsto de negócio que o chamador deve tratar, é usar `Exception` checada ou um tipo de retorno como `Optional`. E é possível abusar de `RuntimeException`: transformar toda falha em não checada para "não ter que declarar `throws`" devolve o problema das falhas silenciosamente ignoradas que as exceções checadas existiam para evitar.

---

## Checked × unchecked

A aula anterior mostrou *onde* na hierarquia mora a fronteira entre exceções checadas e não checadas: tudo que descende de `Exception` sem passar por `RuntimeException` é checado; o resto não é. Esta aula trata do *porquê* e do *como decidir*. A distinção é uma das escolhas de projeto mais características — e mais debatidas — da linguagem Java, e afeta diretamente como você desenha as assinaturas dos seus métodos e os tipos de exceção que cria.

As três seções vão da teoria à prática. "Filosofia" explica a ideia original por trás das exceções checadas e a crítica que ela recebeu. "Propagação" mostra o comportamento concreto de cada tipo ao subir pela pilha de chamadas. "Escolhas de design" fecha com um critério aplicável para decidir, ao criar uma exceção sua, se ela deve ser checada ou não.

### Filosofia

A ideia por trás das exceções checadas é tornar a possibilidade de falha parte do contrato de um método, verificada pelo compilador. Quando um método declara `throws IOException`, ele está dizendo, de forma que a máquina confere, "chamar isto pode dar errado desta maneira específica, e você precisa decidir o que fazer". O objetivo é impedir a classe de bug mais silenciosa que existe: a falha que ninguém tratou porque ninguém percebeu que ela era possível.

Java foi das poucas linguagens de grande adoção a levar essa ideia até a obrigatoriedade. C++, C#, Python, JavaScript — todas têm exceções, nenhuma força o chamador a lidar com elas em tempo de compilação. A aposta dos projetistas de Java, no fim dos anos 1990, foi que o custo de escrever `throws` e `try`/`catch` se pagaria em robustez, sobretudo em sistemas grandes onde ninguém conhece todo o código.

A crítica veio com o uso em escala. Exceções checadas não se combinam bem com abstração: se um método recebe um `Runnable` ou uma `Function` e o código dentro dele lança `IOException`, a assinatura da interface funcional não permite propagar, e você é empurrado para o embrulho em `RuntimeException`. Elas também tendem a "vazar" para cima — uma `SQLException` da camada de dados aparecendo na assinatura de um método de serviço que não deveria saber que existe banco. E há o anti-padrão do `catch (IOException e) {}` vazio, escrito só para o código compilar, que troca uma falha ignorada por silêncio por uma falha ignorada de propósito — pior, porque agora está documentada como intencional.

```java
// A tentação que as exceções checadas criam:
try {
    arquivo.close();
} catch (IOException e) {
    // não faço nada — só para compilar
}
```

O consenso atual, visível em bibliotecas modernas e no design de APIs recentes da própria plataforma, é usar exceções checadas com parcimônia: reservá-las para casos em que o chamador realmente tem uma ação de recuperação plausível, e preferir não checadas para o resto. É uma correção de rota em relação ao entusiasmo original, não um abandono da ideia — a noção de "falha como contrato" continua valiosa onde a recuperação é real.

### Propagação

Propagação é o que acontece com uma exceção depois de lançada e antes de ser capturada: ela sobe pela pilha de chamadas, método a método, na ordem inversa em que os métodos foram chamados, até encontrar um `catch` de tipo compatível. Se chegar ao `main` ou à raiz da thread sem ser capturada, a thread encerra e a JVM imprime o stack trace. Esse comportamento de subida é idêntico para exceções checadas e não checadas — a diferença entre elas não está em como propagam, mas em quais exigências o compilador faz ao longo do caminho.

Para uma exceção checada, cada método na cadeia de propagação precisa reconhecê-la explicitamente. Se o método `a` chama `b`, que chama `c`, e `c` lança `IOException` sem tratar, então `b` precisa declarar `throws IOException` para que a exceção passe por ele, e `a` enfrenta a mesma escolha. A exceção só atravessa um método se aquele método assumir isso na assinatura. É uma cadeia de consentimento: ninguém deixa uma `IOException` passar sem assinar embaixo.

Para uma exceção não checada, a propagação é invisível na assinatura. Uma `NullPointerException` lançada em `c` atravessa `b` e `a` sem que nenhum dos dois mencione nada. Isso é o que torna `RuntimeException` conveniente para defeitos de programação — o código correto não carrega o peso — e também o que torna necessário ler a documentação ou o código para saber o que um método pode lançar.

```java
void a() {
    try {
        b();
    } catch (IOException e) {              // capturada aqui, 2 níveis acima
        log.warn("leitura falhou, usando default", e);
    }
}

void b() throws IOException { c(); }        // precisa declarar para deixar passar

void c() throws IOException {
    throw new IOException("disco cheio");
}
```

Dois recursos ajudam a lidar com a propagação. O *multi-catch*, `catch (IOException | SQLException e)`, trata tipos diferentes no mesmo bloco quando a reação é a mesma. E o *rethrow* — capturar, fazer algo como registrar em log, e relançar com `throw e` — permite reagir num nível sem interromper a subida. Quando não interferir: se um método não tem nada útil a fazer com a exceção, o melhor é deixá-la propagar limpa, sem um `catch` que só registra e relança, o que polui o log com a mesma falha repetida em cada camada.

### Escolhas de design

Ao criar uma exceção própria, a decisão entre estender `Exception` (checada) ou `RuntimeException` (não checada) define como todo o código que a encontra vai ter que se comportar. É uma decisão de API, não um detalhe interno: mudá-la depois quebra todos os chamadores. Vale gastar um minuto nela.

O critério clássico, atribuído às diretrizes da própria plataforma, é a pergunta: *o chamador pode, de forma razoável, se recuperar dessa falha?* Se a resposta é sim — existe uma ação alternativa plausível, como tentar outra fonte de dados, pedir novo input ao usuário, usar um valor padrão — então a exceção descreve uma condição que o chamador deveria tratar, e checada faz sentido: o compilador vai lembrá-lo. Se a resposta é não — a falha significa que algo está programado errado, ou que uma pré-condição foi violada — então não checada é o certo, porque forçar `try`/`catch` para um caso que nunca deveria ocorrer só adiciona ruído.

Aplicado a exemplos: `SaldoInsuficienteException` num sistema bancário é uma boa candidata a checada, porque quem inicia uma transferência tem o que fazer diante dela — avisar o usuário, cancelar a operação. Já `ConfiguracaoInvalidaException`, lançada na inicialização quando um arquivo obrigatório está malformado, funciona melhor não checada: não há recuperação, o programa não deve subir, e obrigar cada método do caminho a declarar `throws` não ajuda ninguém.

```java
// Recuperável pelo chamador → checada
public class SaldoInsuficienteException extends Exception {
    public SaldoInsuficienteException(String contaId, BigDecimal falta) {
        super("Conta " + contaId + " precisa de mais " + falta);
    }
}

// Defeito / pré-condição violada → não checada
public class ConfiguracaoInvalidaException extends RuntimeException {
    public ConfiguracaoInvalidaException(String msg, Throwable causa) {
        super(msg, causa);
    }
}
```

A tendência das bibliotecas modernas pende para não checadas mesmo em casos duvidosos, para não impor cerimônia e para conviver melhor com lambdas e streams — Spring, por exemplo, converteu toda a família `SQLException` em `DataAccessException` não checada. Isso não é permissão para tornar tudo não checado por preguiça: quando a recuperação é real e comum, uma exceção checada bem colocada é o que impede o chamador de esquecer o caso. A alternativa a ambos, para falhas que são parte esperada do fluxo normal e não excepcionais, é não usar exceção nenhuma — devolver um `Optional`, um objeto de resultado, um enum de status — e reservar o mecanismo de exceções para o que de fato foge do caminho feliz.

---

## `throw` e `throws`

Os dois nomes se parecem, aparecem lado a lado e fazem coisas opostas. `throw`, sem `s`, é uma instrução: ela lança uma exceção agora, neste ponto do código. `throws`, com `s`, é uma cláusula na assinatura de um método: ela declara que aquele método pode deixar escapar certos tipos de exceção sem tratá-los. Um é ação, o outro é aviso. Confundir os dois é um erro comum de quem está começando, e escrever `throws new IOException()` ou `throw IOException` não compila.

Esta aula separa os três aspectos. "Lançamento" trata da instrução `throw`: sintaxe, o que acontece com o fluxo de execução, o que se pode lançar. "Propagação" trata da cláusula `throws` e de como uma exceção declarada viaja pela pilha. "Regras" reúne as restrições que o compilador impõe — herança, sobrescrita de métodos, código inalcançável — que costumam pegar o programador de surpresa.

### Lançamento

`throw` é a instrução que dispara uma exceção. A forma é `throw expressão;`, onde a expressão precisa avaliar para uma referência de `Throwable` — quase sempre uma instância recém-criada com `new`. No momento em que a instrução executa, o fluxo normal do método é abandonado imediatamente: nenhuma linha após o `throw` roda, o método não retorna valor, e a exceção começa a subir pela pilha em busca de um `catch`.

O objeto lançado é criado como qualquer outro. `new IllegalArgumentException("id nulo")` chama um construtor, e é nele que a mensagem é definida e o stack trace é capturado — o instantâneo da pilha é tirado na *construção* do `Throwable`, não no `throw`, um detalhe que importa quando se pensa em reaproveitar instâncias de exceção (não faça: o stack trace ficaria congelado no ponto de criação). Também se pode lançar uma exceção recebida por parâmetro ou capturada antes, o que é a base do relançamento.

Sem `throw`, a única forma de um método comunicar falha seria pelo valor de retorno — um `null`, um `-1`, um código de status — que o chamador precisa lembrar de checar e que se perde no meio de expressões. `throw` torna a falha impossível de ignorar por acidente: ela interrompe tudo e exige tratamento em algum ponto.

```java
static int idade(LocalDate nascimento) {
    if (nascimento == null)
        throw new IllegalArgumentException("nascimento não pode ser nulo");
    if (nascimento.isAfter(LocalDate.now()))
        throw new IllegalArgumentException("nascimento no futuro: " + nascimento);
    return Period.between(nascimento, LocalDate.now()).getYears();
}
```

Um uso muito comum é a *validação de argumentos* no início de um método público — o chamado *fail-fast*: checar as pré-condições e lançar `IllegalArgumentException` ou `NullPointerException` na primeira linha, para que um valor inválido seja barrado onde é fácil descobrir a origem, em vez de causar um erro obscuro dez chamadas adiante. `Objects.requireNonNull(x, "x")` é um atalho da biblioteca padrão para exatamente esse padrão.

É como o botão de parada de emergência de uma esteira: apertá-lo interrompe tudo na hora, sem terminar o ciclo atual. A alternativa a `throw`, para falhas que são parte esperada do fluxo, é retornar um `Optional` ou um objeto de resultado. Quando não usar: para controle de fluxo normal — sair de dois laços aninhados, por exemplo — lançar e capturar uma exceção funciona, mas é lento e obscuro; um `break` rotulado ou uma refatoração em método com `return` é mais claro.

### Propagação

A cláusula `throws` na assinatura de um método é a contrapartida declarativa do `throw`. Onde `throw` lança, `throws` avisa: "este método pode terminar lançando exceções destes tipos, e não vou tratá-las aqui dentro". A lista vem depois dos parênteses dos parâmetros: `public String ler(Path p) throws IOException, InterruptedException`. Ela só é obrigatória para exceções checadas; declarar uma `RuntimeException` em `throws` é permitido e às vezes útil como documentação, mas o compilador não cobra.

O efeito de `throws` é habilitar a propagação da exceção checada por aquele método. Como visto na aula anterior, uma `IOException` só consegue atravessar um método rumo ao chamador se aquele método a declarar. `throws` é, portanto, a forma de dizer "a decisão sobre esta falha não é minha, é de quem me chamou". Isso empurra o tratamento para cima, idealmente até a camada que tem contexto para decidir — a que fala com o usuário, a que controla a transação, a que sabe se vale a pena tentar de novo.

```java
// Camada baixa: sabe ler, não sabe o que fazer se falhar → declara e propaga
byte[] baixar(URI uri) throws IOException {
    try (var in = uri.toURL().openStream()) {
        return in.readAllBytes();
    }
}

// Camada alta: tem contexto → aqui o catch faz sentido
void sincronizar(List<URI> fontes) {
    for (URI f : fontes) {
        try {
            processar(baixar(f));
        } catch (IOException e) {
            log.warn("Fonte {} indisponível, pulando", f, e);
        }
    }
}
```

Dois pontos práticos. Um método pode declarar mais tipos do que realmente lança — às vezes se declara uma superclasse como `throws Exception` para não listar cinco subtipos — ao custo de obrigar os chamadores a um tratamento mais genérico. E ao capturar para relançar, a partir do Java 7 o compilador faz *análise precisa de relançamento*: se dentro do `try` só podem ocorrer `IOException` e `SQLException`, um `catch (Exception e) { ...; throw e; }` é aceito como se relançasse apenas esses dois tipos, permitindo que o método declare `throws IOException, SQLException` em vez de `throws Exception`. Quando não usar `throws`: se o método consegue e deve tratar a falha localmente — tem um valor padrão, um recurso alternativo — trate ali e não empurre para cima.

### Regras

Em torno de `throw` e `throws` há um conjunto de regras que o compilador aplica e que costumam surpreender. Reuni-las ajuda a entender mensagens de erro que, isoladas, parecem arbitrárias.

A regra da *sobrescrita*: um método que sobrescreve outro não pode declarar exceções checadas mais amplas do que o método da superclasse ou da interface. Pode declarar as mesmas, pode declarar subtipos, pode declarar menos, pode não declarar nenhuma — mas não pode adicionar uma `IOException` se o método original não a previa. O motivo é o polimorfismo: quem chama pela referência do tipo base escreveu `try`/`catch` conforme a assinatura base, e uma implementação não pode lançar algo que esse `catch` não cobre. Exceções não checadas não entram nessa regra — uma sobrescrita pode lançar qualquer `RuntimeException`.

A regra do *catch inalcançável*: colocar um `catch` de um tipo checado que o `try` comprovadamente não lança é erro de compilação. `catch (IOException e)` em volta de um bloco que só faz aritmética não compila. Para `RuntimeException` isso é permitido, porque o compilador não rastreia onde elas podem surgir.

A regra da *ordem dos catch*: blocos `catch` são testados de cima para baixo, então um `catch` de superclasse antes de um `catch` de subclasse torna o segundo inalcançável — e isso também não compila. `catch (Exception e)` sempre vem depois de `catch (IOException e)`, nunca antes.

```java
class Base {
    void salvar() throws IOException { /* ... */ }
}
class EmMemoria extends Base {
    @Override
    void salvar() { /* sem throws: permitido, é "menos" */ }
}
class EmDisco extends Base {
    @Override
    void salvar() throws SQLException {   // ERRO: SQLException não é subtipo de IOException
        /* ... */
    }
}
```

Há ainda a interação com `finally`: se um `finally` executa um `return` ou um `throw`, ele descarta qualquer exceção que estivesse subindo pelo `try` — a falha original desaparece sem rastro. É um anti-padrão sério, detalhado na última aula do módulo. E a regra da *atribuição definitiva*: depois de um `throw` incondicional, o compilador considera o restante do bloco inalcançável, o que permite, por exemplo, não inicializar uma variável no ramo `else` que só lança. Essas regras não têm alternativa a escolher — são restrições da linguagem — mas conhecê-las evita perder tempo com mensagens de compilação que parecem não fazer sentido.

---

## Exceções customizadas

As exceções que a biblioteca padrão oferece — `IllegalArgumentException`, `IOException`, `IllegalStateException` — descrevem falhas em termos técnicos genéricos. Elas servem, mas não dizem nada sobre o *seu* problema. Quando o log mostra `IllegalStateException: invalid`, quem lê ainda precisa investigar o que estava inválido e por quê. Uma exceção customizada — uma classe sua que estende `Exception` ou `RuntimeException` — permite nomear a falha no vocabulário do domínio: `PedidoJaFaturadoException`, `EstoqueInsuficienteException`.

Esta aula tem três seções. "Exceções de domínio" trata de quando e como criar esses tipos e o que colocar dentro deles. "Causa" trata do `Throwable` que uma exceção guarda para apontar o que a originou. "Encadeamento" trata do padrão de capturar uma exceção de baixo nível e relançá-la embrulhada numa de nível mais alto, sem perder a informação original.

### Exceções de domínio

Uma exceção de domínio é uma classe de exceção criada por você para representar uma falha específica da área de negócio da aplicação, com um nome que descreve essa falha na linguagem do domínio. Em vez de lançar `IllegalStateException("pedido já foi faturado")`, você define `PedidoJaFaturadoException` e lança isso. A informação é a mesma; o que muda é que agora existe um *tipo* para essa condição, e tipos podem ser capturados seletivamente, documentados na assinatura e tratados de formas diferentes.

O problema com exceções genéricas aparece quando o chamador precisa reagir de forma distinta a falhas distintas. Se `processarPedido` pode falhar por estoque, por pagamento recusado ou por pedido duplicado, e as três saem como `IllegalStateException`, o único jeito de diferenciá-las no `catch` é inspecionar o texto da mensagem — frágil, quebra se alguém corrigir uma vírgula, impossível de traduzir. Com tipos próprios, `catch (EstoqueInsuficienteException e)` e `catch (PagamentoRecusadoException e)` são blocos separados, cada um com sua reação.

Uma exceção de domínio também carrega dados estruturados sobre a falha, não só um texto. Se o estoque é insuficiente, a exceção pode guardar o SKU e a quantidade faltante como campos, acessíveis por getters, para que o tratador monte uma mensagem, dispare uma reposição ou registre uma métrica sem ter que extrair números de uma string.

```java
public class EstoqueInsuficienteException extends RuntimeException {
    private final String sku;
    private final int faltam;

    public EstoqueInsuficienteException(String sku, int faltam) {
        super("SKU %s: faltam %d unidades".formatted(sku, faltam));
        this.sku = sku;
        this.faltam = faltam;
    }
    public String sku()  { return sku; }
    public int faltam()  { return faltam; }
}
```

A escolha entre estender `Exception` ou `RuntimeException` segue o critério da aula sobre checked × unchecked: recuperável e esperado pelo chamador → checada; violação de regra que o chamador não deveria contornar item a item → não checada. Muitos times padronizam uma exceção-base de domínio própria — `class DominioException extends RuntimeException` — e fazem todas as demais herdarem dela, o que permite um `catch (DominioException e)` genérico na borda da aplicação, separando falhas de negócio de falhas técnicas.

É a diferença entre um erro de sistema que diz "código 500" e um que diz "cartão vencido": o segundo é acionável por quem o recebe. Quando não criar uma exceção de domínio: para checagens triviais de argumento — nulo, número negativo, string vazia — as exceções padrão `NullPointerException` e `IllegalArgumentException` são universalmente entendidas e criar `IdNuloException` só adiciona classes sem ganho. E se só existe um tipo de falha possível e ninguém trata de forma diferenciada, uma exceção padrão com boa mensagem basta.

### Causa

A causa de uma exceção é outro `Throwable`, guardado dentro dela, que representa a falha que a originou. Todo `Throwable` tem esse espaço: `getCause()` devolve a causa, ou `null` se não houver. Ela é preenchida por um dos construtores que aceitam um `Throwable` — `new IllegalStateException("falha ao carregar", e)` — ou, mais raramente, pelo método `initCause(e)` quando a classe não oferece o construtor adequado.

O problema que a causa resolve é a perda de informação quando uma falha é transformada em outra. Imagine uma camada de repositório que captura `SQLException` e, por não querer expor o banco, lança `RepositorioException`. Se essa nova exceção não guardar a `SQLException` original, tudo o que o banco informou — o código do erro, a constraint violada, o SQL que falhou — some. O log mostra `RepositorioException: erro ao salvar cliente` e a investigação para aí. A causa preserva essa trilha: a `SQLException` viaja dentro da `RepositorioException`, e o stack trace impresso mostra as duas, ligadas pela linha `Caused by:`.

```java
try {
    jdbc.insert(cliente);
} catch (SQLException e) {
    throw new RepositorioException("erro ao salvar cliente " + cliente.id(), e);
}                                                                          // ^ causa
```

O stack trace resultante tem duas partes:

```
RepositorioException: erro ao salvar cliente 4021
    at ClienteRepo.salvar(ClienteRepo.java:58)
    at CadastroService.registrar(CadastroService.java:31)
Caused by: java.sql.SQLIntegrityConstraintViolationException: Duplicate entry '4021'
    at com.mysql.cj.jdbc...
    at ClienteRepo.salvar(ClienteRepo.java:56)
```

A parte de cima diz *onde no seu código* a falha se manifestou; o `Caused by` diz *o que de fato deu errado* lá embaixo. Ler exceções encadeadas é quase sempre começar pelo último `Caused by`, que costuma ter a informação mais concreta.

Recuperar a raiz de uma cadeia longa é seguir `getCause()` em laço até chegar a `null`; bibliotecas utilitárias como o `ExceptionUtils.getRootCause` do Apache Commons fazem isso. Quando não passar causa: se você está lançando uma exceção nova por uma condição que detectou você mesmo — um `if` de validação que dispara `IllegalArgumentException` — não há causa, porque nada "deu errado antes"; a falha começa ali. Passar `null` como causa nesse caso é o correto e é o que o construtor de dois argumentos com `null` faz implicitamente.

### Encadeamento

Encadeamento de exceções — também chamado de *tradução de exceções* — é o padrão de capturar uma exceção de uma camada inferior e relançar uma exceção de outro tipo, mais adequada à camada atual, guardando a original como causa. É a aplicação prática do conceito de causa da seção anterior, elevado a princípio de arquitetura: cada camada expõe falhas no seu próprio vocabulário, mas nenhuma joga fora o que a camada de baixo informou.

Sem encadeamento, há dois caminhos ruins. O primeiro é deixar a exceção de baixo nível vazar: a camada de serviço declara `throws SQLException`, e agora o controlador web, a classe de teste e o código cliente todos dependem de `java.sql`, uma abstração que deveria estar escondida. Trocar o banco por um repositório em memória passa a quebrar assinaturas. O segundo caminho ruim é capturar e relançar *sem* a causa — `catch (SQLException e) { throw new ServiceException("falhou"); }` — que esconde o banco mas destrói a trilha de diagnóstico. O encadeamento é o meio-termo: novo tipo, causa preservada.

```java
// Repositório: traduz a falha técnica para o vocabulário do domínio
public Usuario buscar(long id) {
    try {
        return jdbc.queryForObject(SQL_BUSCA, mapper, id);
    } catch (EmptyResultDataAccessException e) {
        throw new UsuarioNaoEncontradoException(id, e);   // domínio + causa
    } catch (DataAccessException e) {
        throw new RepositorioIndisponivelException(e);    // técnico + causa
    }
}
```

Quem chama `buscar` lida com `UsuarioNaoEncontradoException` e `RepositorioIndisponivelException` — nomes que fazem sentido na camada de negócio — e nunca precisa importar nada de JDBC. Se um bug de infraestrutura acontecer, o `Caused by` no log ainda mostra a `DataAccessException` e, abaixo dela, a `SQLException` de verdade.

O anti-padrão a evitar é o `catch (Exception e) { throw new RuntimeException(e); }` aplicado indiscriminadamente em toda camada: isso "encadeia", mas não *traduz* — o tipo novo não carrega significado nenhum, e você acaba com três ou quatro `RuntimeException` genéricas empilhadas no stack trace, cada uma adicionando um nível de ruído sem informação. Encadear vale a pena quando o novo tipo comunica algo que o antigo não comunicava na linguagem daquela camada.

Pense num tradutor numa reunião internacional: ele converte a fala para o idioma da sala, mas não inventa nem omite — se preciso, aponta "ele disse isto no original". Quando não encadear: dentro de uma mesma camada, se a exceção que subiu já está no vocabulário certo, deixe-a passar intacta; re-embrulhar `PedidoInvalidoException` em outra `PedidoInvalidoException` não agrega. E em utilitários de baixo nível que não têm um "domínio" próprio, propagar a exceção original checada costuma ser mais honesto do que inventar um tipo novo sem significado.

---

## Recursos

Um recurso, no sentido desta aula, é qualquer coisa que o programa abre e precisa fechar depois: um arquivo, uma conexão de banco, um socket, um stream. O sistema operacional concede um número limitado deles, e esquecer de fechar — um *vazamento de recurso* — leva a erros de "too many open files", conexões travadas e arquivos que não podem ser apagados. O problema é que a linha que fecha o recurso precisa executar *mesmo quando* o código entre a abertura e o fechamento lança uma exceção.

Esta aula cobre os quatro mecanismos que Java oferece para isso, em ordem histórica e de preferência. `finally` é a ferramenta original, manual. `AutoCloseable` é a interface que marca um objeto como "fechável". Try-with-resources é a sintaxe que fecha automaticamente. E *suppressed exceptions* é o detalhe que resolve o caso peculiar de uma exceção acontecer durante o próprio fechamento.

### `finally`

`finally` é um bloco opcional anexado a um `try` que executa sempre, aconteça o que acontecer no `try` — se o bloco terminou normalmente, se lançou uma exceção, se executou um `return`, se fez um `break`. Essa garantia de execução é o que faz do `finally` o lugar histórico para código de limpeza: fechar um arquivo, liberar um lock, restaurar um estado.

Antes do `finally`, garantir limpeza diante de exceções exigia duplicar o código de fechamento — uma vez no caminho de sucesso, outra dentro de cada `catch` — e ainda assim uma exceção inesperada podia escapar por um caminho não previsto. O `finally` centraliza isso num único bloco que o runtime se compromete a executar.

```java
Connection con = pool.get();
try {
    con.setAutoCommit(false);
    inserir(con, pedido);
    inserir(con, itens);
    con.commit();
} catch (SQLException e) {
    con.rollback();
    throw new PersistenciaException("falha ao gravar pedido", e);
} finally {
    con.setAutoCommit(true);
    pool.devolver(con);          // executa com sucesso OU com exceção
}
```

Aqui a devolução da conexão ao pool acontece nos dois cenários. Sem o `finally`, um `throw` no `catch` pularia a devolução e vazaria a conexão.

`finally` tem duas armadilhas sérias. A primeira: um `return` ou um `throw` dentro do `finally` *substitui* qualquer coisa que o `try` ou o `catch` estivessem retornando ou lançando. Se o `try` estava propagando uma `SQLException` e o `finally` faz `return false`, a `SQLException` é descartada silenciosamente — o chamador recebe `false` e nunca fica sabendo da falha. A segunda, relacionada: chamar um `close()` que pode lançar dentro do `finally` sem envolvê-lo em outro `try` faz com que a exceção do `close` mascare a exceção original do `try`. Esse segundo problema é exatamente o que o try-with-resources e as suppressed exceptions foram criados para resolver.

É como uma cláusula de contrato que vale "em qualquer circunstância": útil para garantir a devolução das chaves, perigosa se escrita de forma que anule as outras cláusulas. Quando não usar `finally` diretamente: se o objetivo é só fechar um recurso que implementa `AutoCloseable`, o try-with-resources da seção seguinte faz o mesmo com menos código e sem as armadilhas. `finally` explícito fica para limpezas que não são "fechar um recurso" — restaurar um flag, medir tempo decorrido, desfazer uma mudança temporária de estado.

### `AutoCloseable`

`AutoCloseable` é uma interface da biblioteca padrão com um único método: `void close() throws Exception`. Seu papel é marcar uma classe como "algo que possui um recurso e sabe liberá-lo". Qualquer objeto cuja classe implemente `AutoCloseable` pode ser usado no cabeçalho de um try-with-resources, que chamará `close()` automaticamente. Todas as classes de I/O, JDBC e rede da plataforma a implementam.

Existe também `Closeable`, mais antiga, do pacote `java.io`. A relação entre as duas: `Closeable` estende `AutoCloseable`, restringe o `close()` a lançar apenas `IOException` (em vez de `Exception`) e exige que `close()` seja idempotente — chamar duas vezes não faz mal. Para código novo que precisa declarar a interface, `AutoCloseable` é a escolha padrão; `Closeable` só quando o recurso é genuinamente de I/O e a compatibilidade com APIs antigas importa.

O problema que `AutoCloseable` resolve é de padronização. Antes dela, cada biblioteca tinha seu próprio método de encerramento — `close()`, `dispose()`, `release()`, `shutdown()` — e não havia como uma construção da linguagem saber qual chamar. Ao definir um contrato único, `AutoCloseable` permitiu que o try-with-resources funcionasse com qualquer recurso, inclusive os que você mesmo cria.

```java
public class ArquivoTemporario implements AutoCloseable {
    private final Path caminho;

    public ArquivoTemporario(String prefixo) throws IOException {
        this.caminho = Files.createTempFile(prefixo, ".tmp");
    }
    public Path caminho() { return caminho; }

    @Override
    public void close() throws IOException {
        Files.deleteIfExists(caminho);      // libera o recurso
    }
}
```

Com essa classe, `try (var tmp = new ArquivoTemporario("export"))` garante que o arquivo é apagado ao final do bloco, com ou sem exceção. Implementar `AutoCloseable` é o jeito de dar às suas abstrações — um cliente HTTP com pool interno, uma sessão, um span de tracing — a mesma ergonomia dos recursos da plataforma.

Duas recomendações do contrato: torne `close()` idempotente sempre que possível, porque em código real ele às vezes é chamado mais de uma vez; e evite declarar `close()` lançando exceção se puder, porque isso complica o uso — daí a interface `Closeable` restringir a `IOException` e algumas implementações declararem `close()` sem `throws` nenhum. Quando não implementar `AutoCloseable`: se o objeto não possui recurso externo — é só dados na memória, coletados pelo garbage collector normalmente — não há o que fechar, e adicionar `close()` só sugere falsamente que há um ciclo de vida a gerenciar.

### Try-with-resources

Try-with-resources é uma forma de `try` que declara um ou mais recursos entre parênteses no cabeçalho e garante que cada um deles tenha `close()` chamado ao final do bloco, automaticamente. A sintaxe é `try (Tipo r = inicializador) { ... }`. Introduzida no Java 7, ela substitui, na maioria dos casos, o par `try`/`finally` manual para fechamento de recursos, e elimina as armadilhas daquele padrão.

O código que ela poupa é notoriamente propenso a erro. O fechamento manual "correto" de dois recursos exige `try` aninhados, verificação de `null` e um `try`/`catch` em volta de cada `close()` para que a falha de um não impeça o fechamento do outro nem mascare a exceção do corpo. Quase ninguém escrevia isso certo. Try-with-resources gera tudo isso para você.

```java
// Manual, feito certo — verboso e fácil de errar:
BufferedReader r = null;
try {
    r = Files.newBufferedReader(entrada);
    return r.readLine();
} finally {
    if (r != null) r.close();     // e se close() lançar? mascara o try
}

// Try-with-resources — equivalente e correto:
try (BufferedReader r = Files.newBufferedReader(entrada)) {
    return r.readLine();
}
```

As garantias: os recursos são fechados na ordem *inversa* da declaração, o que respeita dependências entre eles (o que foi aberto por último, e pode depender do anterior, fecha primeiro). O `close()` é chamado mesmo que o corpo lance ou execute `return`. E se tanto o corpo quanto um `close()` lançarem, a exceção do corpo é a que propaga — a do `close()` não some, vira uma *suppressed exception* anexada a ela, assunto da próxima seção. Isso é o oposto do que o `try`/`finally` ingênuo faz, onde o `close()` mascara o corpo.

Vários recursos vão no mesmo cabeçalho, separados por ponto e vírgula:

```java
try (var in = Files.newInputStream(origem);
     var out = Files.newOutputStream(destino)) {
    in.transferTo(out);
}   // out.close() primeiro, depois in.close()
```

A partir do Java 9, se o recurso já está numa variável `final` ou *efetivamente final*, pode-se referenciá-la direto no cabeçalho — `try (recursoJaExistente) { ... }` — sem redeclarar. Quando não usar: se o objeto no `try` não implementa `AutoCloseable`, o cabeçalho não o aceita — use `finally`. E se o ciclo de vida do recurso ultrapassa o bloco — uma conexão que fica aberta enquanto o objeto que a contém viver — try-with-resources não serve, porque ele fecha ao sair do bloco; nesse caso o fechamento pertence ao `close()` da classe que segura o recurso.

### Suppressed exceptions

Uma *suppressed exception* (exceção suprimida) é uma exceção que aconteceu mas foi posta de lado para não ocultar outra mais importante que já estava em curso. O cenário que a origina é específico do try-with-resources: o corpo do `try` lança uma exceção e, durante o fechamento automático que se segue, o `close()` de um recurso *também* lança. Há agora duas exceções e só um caminho de propagação. Java resolve propagando a do corpo — a causa primária, a que o programador quer ver — e anexando a do `close()` a ela como suprimida, via o método `addSuppressed(Throwable)`.

Antes do Java 7, esse conflito era resolvido da pior forma: no `try`/`finally` manual, a exceção do `close()` no `finally` simplesmente substituía a do corpo. Você tinha um bug real — digamos, um erro de parsing no meio do arquivo — e o que aparecia no log era `IOException: stream closed`, uma consequência trivial, enquanto a causa de verdade desaparecia. Horas de depuração se perdiam perseguindo a exceção errada.

As exceções suprimidas ficam acessíveis por `getSuppressed()`, que devolve um vetor, e aparecem no stack trace impresso sob a marca `Suppressed:`, indentadas abaixo da exceção principal:

```
ParseException: linha 42: campo 'valor' não numérico
    at RelatorioParser.parseLinha(RelatorioParser.java:88)
    at RelatorioParser.processar(RelatorioParser.java:51)
    Suppressed: java.io.IOException: erro ao fechar conexão temporária
        at TempConn.close(TempConn.java:34)
        at RelatorioParser.processar(RelatorioParser.java:49)
```

A exceção de cima é a que o `catch` do chamador recebe e a que importa para o diagnóstico; a `Suppressed` está registrada, não foi perdida, mas não roubou o protagonismo. Se o corpo do `try` termina normalmente e só o `close()` lança, então não há conflito — a exceção do `close()` propaga normalmente, sem ser suprimida.

Também é possível adicionar exceções suprimidas manualmente com `e.addSuppressed(outra)`, útil quando se implementa um mecanismo de limpeza próprio que precisa tentar várias operações e reportar todas as falhas sem deixar a primeira abortar o resto. É o comportamento de um socorrista que prioriza: estabiliza o ferimento grave primeiro e anota os arranhões para depois, em vez de tratar o arranhão e deixar o resto sangrar. Quando você não precisa pensar nisso: no uso comum de try-with-resources, as suppressed exceptions são geradas e exibidas sozinhas — basta ler o stack trace inteiro, incluindo as linhas `Suppressed:`, em vez de parar na primeira. O cuidado ativo só aparece se você escreve o próprio `close()` ou o próprio código de limpeza em lote.

---

# Módulo 9 — Arquivos e I/O básico

Trabalhar com arquivos e fluxos de dados é essencial. Java oferece a API de NIO (New I/O) com `Path` e `Files` para manipulação de caminhos e operações simples, e streams de bytes/caracteres para leitura e escrita de dados brutos e de texto. Este módulo cobre essas APIs fundamentais e como usá-las com segurança via try-with-resources.

## `Path`

Antes de ler ou escrever qualquer arquivo, é preciso dizer ao programa *onde* ele está. Em Java moderno, esse endereço é representado por um objeto `java.nio.file.Path`, e não mais por uma `String` solta ou pela antiga classe `java.io.File`. Um `Path` é uma sequência de nomes — pastas e, no fim, opcionalmente o nome do arquivo — que o sistema operacional sabe resolver. Ele é imutável e não toca o disco: criar um `Path` para `/etc/hosts` não verifica se o arquivo existe, apenas descreve o caminho.

Esta aula cobre três ideias que andam juntas: como se constrói e se inspeciona um `Path` (Caminhos), a diferença entre um endereço que parte da raiz do sistema e um que parte do diretório de trabalho (Absoluto × relativo), e como limpar um caminho cheio de `.` e `..` para obter sua forma canônica (Normalização). Dominar isso evita a maior fonte de bugs de I/O: o programa procurar o arquivo no lugar errado.

### Caminhos

`Path` é a interface de `java.nio.file` que representa, de forma estruturada, um caminho no sistema de arquivos. Você o cria com `Path.of("relatorios", "2026", "vendas.csv")` (ou o equivalente `Paths.get(...)`), passando os segmentos separados — o próprio `Path` os junta com o separador correto do sistema operacional (`/` no Linux e macOS, `\` no Windows). Ele se encaixa como o primeiro passo de qualquer operação de arquivo: `Files`, os streams e os readers todos recebem um `Path` para saber sobre o que trabalhar.

Sem esse tipo, o caminho era uma `String` montada na mão. Concatenar `pasta + "/" + arquivo` quebrava no Windows, produzia barras duplicadas quando `pasta` já terminava em `/`, e extrair "só o nome do arquivo" ou "só a pasta pai" exigia `lastIndexOf` e `substring` — código frágil que errava nos casos de borda. A classe `java.io.File` cobria parte disso, mas com uma API pobre e presa ao modelo antigo de I/O.

`Path` resolve com métodos nomeados para cada pergunta: `getFileName()` devolve o último segmento, `getParent()` devolve o caminho sem ele, `getName(i)` e `getNameCount()` percorrem os segmentos, `getRoot()` devolve a raiz. Para combinar caminhos há `resolve` (anexa um trecho ao fim), `resolveSibling` (troca o último segmento) e `relativize` (calcula o caminho relativo de um até o outro).

```java
Path base = Path.of("/srv/app/dados");
Path arquivo = base.resolve("clientes/lista.txt");
System.out.println(arquivo.getFileName()); // lista.txt
System.out.println(arquivo.getParent());   // /srv/app/dados/clientes

Path a = Path.of("/srv/app/dados/clientes/lista.txt");
Path b = Path.of("/srv/app/dados");
System.out.println(b.relativize(a));        // clientes/lista.txt
```

A analogia útil é a de um endereço postal separado em campos — rua, número, bairro, cidade — em vez de uma linha única de texto: com os campos, você pergunta "qual é a cidade?" sem fatiar nada. No `Path`, cada componente do caminho é acessível por um método próprio, sem manipular a string.

As alternativas são `java.io.File` (legado, ainda aceito e conversível com `file.toPath()` / `path.toFile()`) e a `String` pura, que só vale a pena numa fronteira em que outro sistema exige texto. `Path` não é a ferramenta certa para endereços que não pertencem ao sistema de arquivos local: uma URL `http` é `URI`/`URL`, e entradas dentro de um arquivo `.zip` pedem um `FileSystem` específico obtido via `FileSystems.newFileSystem`.

### Absoluto × relativo

Um caminho **absoluto** começa na raiz do sistema — `/` no Linux e macOS, `C:\` (ou outra letra) no Windows — e identifica o arquivo sem nenhuma ambiguidade, não importa de onde o programa foi iniciado. Um caminho **relativo** não começa na raiz; ele é interpretado a partir do *diretório de trabalho atual* do processo, que em Java é o valor de `System.getProperty("user.dir")` — normalmente a pasta de onde a JVM foi lançada, e **não** a pasta do `.jar` nem a do arquivo-fonte. O método `path.isAbsolute()` diz em qual dos dois casos você está.

O problema aparece quando o diretório de trabalho muda entre ambientes. O programador escreve `Path.of("config.txt")`, roda pela IDE — cujo diretório de trabalho é a raiz do projeto — e tudo funciona. Em produção, o serviço é iniciado a partir de `/` ou de `/opt/app/bin`, e o mesmo código lança `NoSuchFileException` porque `config.txt` não está ali. É um bug que não depende do código, e sim de quem chamou o programa e de onde.

A forma de lidar é nunca depender do diretório de trabalho implícito para arquivos que importam. Defina uma base explícita — vinda de configuração, de variável de ambiente ou de argumento de linha de comando — e resolva os relativos contra ela: `BASE_DIR.resolve("config.txt")`. Para diagnosticar, registre `path.toAbsolutePath()` no log, que mostra contra que diretório o relativo foi resolvido.

```java
Path rel = Path.of("dados/entrada.csv");
System.out.println(rel.isAbsolute());     // false
System.out.println(rel.toAbsolutePath()); // <working dir>/dados/entrada.csv

Path base = Path.of("/opt/meuapp");
Path abs = base.resolve(rel);             // /opt/meuapp/dados/entrada.csv

// resolve() com um argumento já absoluto ignora a base:
base.resolve(Path.of("/etc/hosts"));      // /etc/hosts
```

Pense no caminho absoluto como "Av. Paulista, 1000, São Paulo": qualquer pessoa chega lá. O relativo é "duas portas à direita" — só significa alguma coisa se você souber de onde a pessoa está partindo, e esse ponto de partida, no programa, é o diretório de trabalho da JVM.

A alternativa a um relativo mal-comportado é um absoluto derivado de configuração; um absoluto *fixo* no código é igualmente frágil, porque muda de máquina para máquina. Caminhos relativos são aceitáveis em ferramentas de linha de comando interativas, em que o usuário está num diretório e espera que o programa aja ali; para serviços, tarefas agendadas e testes que precisam ser determinísticos, prefira sempre o absoluto resolvido a partir de uma base conhecida.

### Normalização

Normalizar um caminho é reduzi-lo à sua forma mais simples, eliminando os elementos redundantes: `.` (que significa "o próprio diretório atual") e `..` (que significa "o diretório pai"). O método `path.normalize()` faz isso de maneira puramente textual, sem acessar o disco: `/a/b/../c` vira `/a/c`, e `/a/./b` vira `/a/b`. É uma operação de string estruturada, não uma consulta ao sistema de arquivos.

Sem normalização, caminhos montados a partir de pedaços de origens diferentes acumulam `.` e `..` e ficam difíceis de comparar: `/dados/rel/../rel/x.txt` e `/dados/rel/x.txt` apontam para o mesmo arquivo, mas não são `equals()` entre si, o que atrapalha usar `Path` como chave de mapa ou detectar duplicatas. Há um problema mais sério: se parte do caminho vem de entrada do usuário, um valor como `../../etc/passwd` faz o programa acessar um arquivo fora da pasta permitida — o ataque conhecido como *path traversal*.

`normalize()` resolve o primeiro problema entregando uma forma canônica comparável. Para o segundo, a defesa é normalizar **e depois** verificar que o resultado ainda está dentro do diretório permitido, com `startsWith`. Quando o arquivo precisa existir de verdade e você quer o caminho real inclusive com links simbólicos resolvidos, existe `toRealPath()`, que consulta o disco e lança `IOException` se o alvo não existe.

```java
Path p = Path.of("/srv/uploads/../uploads/./ana/foto.png");
System.out.println(p.normalize()); // /srv/uploads/ana/foto.png

// defesa contra path traversal
Path raiz = Path.of("/srv/uploads").toAbsolutePath().normalize();
Path pedido = raiz.resolve(nomeVindoDoUsuario).normalize();
if (!pedido.startsWith(raiz)) {
    throw new SecurityException("caminho fora da área permitida");
}
```

A analogia é a de reescrever um roteiro: "saia de casa, volte para casa, saia de novo, vá à padaria" descreve o mesmo destino que "vá à padaria", só que com voltas inúteis. `normalize()` corta as voltas e deixa o trajeto direto — mas fazendo isso apenas sobre o texto do caminho, sem andar de fato.

A alternativa quando o arquivo existe é `toRealPath()`, que dá o caminho canônico verdadeiro; `toAbsolutePath()` sozinho **não** remove `..`, só prefixa o diretório de trabalho. Evite `toRealPath()` quando existem links simbólicos que você deliberadamente não quer resolver. E cuidado com o limite de `normalize()` para segurança: ele é textual, então um link simbólico dentro da raiz apontando para fora dela passa despercebido pelo `startsWith` — nesse cenário, só `toRealPath()` fecha a brecha.

## `Files`

`java.nio.file.Files` é uma classe utilitária composta apenas de métodos estáticos que operam sobre objetos `Path`. Se `Path` é o endereço, `Files` é quem de fato vai até lá: cria, lê, escreve, copia, move e apaga arquivos e diretórios. Ela reúne num único lugar operações que, na API antiga, estavam espalhadas por `File`, `FileInputStream`, `FileWriter` e código repetido de projeto para projeto.

Esta aula percorre as seis operações do dia a dia — Criar, Ler, Escrever, Copiar, Mover e Excluir —, cada uma com seus métodos, suas opções (`StandardOpenOption`, `StandardCopyOption`) e seus modos de falhar. Quase tudo em `Files` lança `IOException`, que é *checked*, então essas chamadas aparecem sempre dentro de um `try`/`catch` ou de um método que declara `throws IOException`.

### Criar

Para trazer um arquivo ou diretório à existência, `Files` oferece `createFile(path)` (cria um arquivo vazio), `createDirectory(path)` (cria uma pasta, exigindo que a pasta pai já exista), `createDirectories(path)` (cria a cadeia inteira de pastas que faltar) e o par `createTempFile(...)` / `createTempDirectory(...)` para itens temporários com nome único gerado pelo sistema. Todos recebem um `Path` e devolvem o `Path` criado.

Na API antiga, criar pastas era `new File(caminho).mkdirs()`, um método que devolvia um `boolean` que quase ninguém verificava — quando falhava, falhava em silêncio, e não havia como saber se foi por falta de permissão, por o caminho já existir como arquivo ou por outro motivo. Criar um arquivo temporário seguro, com nome imprevisível e sem colidir com outro processo, exigia lógica manual sujeita a condições de corrida.

`Files` resolve isso lançando exceções específicas: `FileAlreadyExistsException`, `NoSuchFileException` quando o diretório pai não existe, `AccessDeniedException` quando falta permissão. E `createDirectories` é idempotente — se a pasta já está lá, ele não reclama, o que o torna seguro de chamar no início de qualquer rotina que vá gravar algo.

```java
Path dir = Path.of("/var/meuapp/relatorios/2026/09");
Files.createDirectories(dir); // cria tudo que faltar, sem erro se já existir

Path novo = dir.resolve("resumo.txt");
try {
    Files.createFile(novo);
} catch (FileAlreadyExistsException e) {
    System.out.println("já existia, seguindo em frente");
}

Path temp = Files.createTempFile("upload-", ".tmp"); // ex.: /tmp/upload-8471923.tmp
```

A diferença entre os dois métodos de diretório é como abrir uma gaveta: `createDirectory` supõe que o armário já está montado e só falta a gaveta; `createDirectories` monta o armário inteiro — todas as pastas intermediárias do caminho — até chegar na gaveta pedida.

Como alternativa, os métodos de escrita (`Files.write`, `Files.writeString`) já criam o arquivo se ele não existir, então chamar `createFile` antes de escrever costuma ser redundante — o que eles não criam é a pasta pai. Use `createFile` explicitamente quando quiser *reservar* um nome, garantir que ele ainda não existia (a exceção serve de sinal), ou criar um arquivo que ficará vazio por enquanto.

### Ler

Para ler o conteúdo de um arquivo, `Files` cobre desde o caso trivial até o arquivo gigante. `Files.readString(path)` (Java 11+) devolve o arquivo inteiro como uma `String`, decodificado em UTF-8 por padrão. `Files.readAllBytes(path)` devolve um `byte[]`. `Files.readAllLines(path)` devolve uma `List<String>` com as linhas. `Files.lines(path)` devolve um `Stream<String>` preguiçoso, que lê sob demanda e precisa ser fechado. E `Files.newBufferedReader(path, charset)` entrega um `BufferedReader` pronto para o loop clássico.

Antes desses atalhos, ler um arquivo de texto pequeno significava abrir um `FileInputStream`, envolvê-lo num `InputStreamReader` com o charset certo, envolver isso num `BufferedReader`, fazer o laço de `readLine`, e fechar tudo num `finally` — mais de uma dúzia de linhas para uma tarefa banal. Esquecer de informar o charset produzia texto diferente dependendo da máquina onde o programa rodava.

Os métodos de `Files` reduzem o caso comum a uma linha. Para arquivos que não cabem confortavelmente na memória, `Files.lines` é a saída: ele entrega uma linha por vez ao processamento, mantendo apenas uma de cada vez viva.

```java
// arquivo de configuração pequeno
String texto = Files.readString(Path.of("config/app.properties"));

// arquivo de log grande: conta erros sem carregar tudo
try (Stream<String> linhas = Files.lines(Path.of("/var/log/app.log"))) {
    long erros = linhas.filter(l -> l.contains("ERROR")).count();
    System.out.println(erros);
}
```

A imagem é a de encher um balde: `readString` e `readAllBytes` despejam o conteúdo inteiro de uma vez, o que é ótimo enquanto o balde é pequeno; `Files.lines` é abrir uma torneira e deixar pingar enquanto você usa cada gota, sem nunca precisar do reservatório inteiro em mãos.

As alternativas são os streams de caracteres montados manualmente (assunto da aula 4), o `Scanner` quando você quer ler valores tipados token a token, e bibliotecas de CSV ou JSON que fazem a leitura e o parsing por conta própria. O que evitar: `readString` e `readAllBytes` em arquivos que podem ser enormes — carregar 2 GB de uma vez derruba a aplicação com `OutOfMemoryError`. Nesses casos, use `Files.lines` ou um `BufferedReader`, e lembre de fechar o `Stream` de `Files.lines` (por isso ele aparece sempre num try-with-resources).

### Escrever

Para gravar conteúdo, `Files.writeString(path, texto)` escreve uma `String`, `Files.write(path, bytes)` escreve um `byte[]`, `Files.write(path, listaDeLinhas)` escreve uma coleção de linhas, e `Files.newBufferedWriter(path, ...)` devolve um `BufferedWriter` para escrita contínua. O comportamento padrão de `writeString` e `write` é: cria o arquivo se ele não existe e **sobrescreve** (trunca para zero e regrava) se ele já existe.

Esse padrão se ajusta com o parâmetro varargs `StandardOpenOption`: `APPEND` acrescenta ao fim em vez de truncar; `CREATE_NEW` faz a operação falhar se o arquivo já existir (útil para evitar corrida entre processos); `CREATE` combinado com `APPEND` cria-ou-acrescenta; `TRUNCATE_EXISTING` é o comportamento padrão tornado explícito. Na API antiga, o equivalente era `FileWriter` com um segundo argumento `boolean append` fácil de esquecer, sem controle mais fino e usando o charset da plataforma.

```java
Path saida = Path.of("relatorios/hoje.txt");
Files.writeString(saida, "Total de vendas: 1.234\n"); // cria ou sobrescreve

// acrescentar sem apagar o que já está lá
Files.writeString(saida, "linha extra\n",
        StandardOpenOption.CREATE, StandardOpenOption.APPEND);

// falhar de propósito se já existir
Files.writeString(Path.of("app.pid"), "42", StandardOpenOption.CREATE_NEW);
```

Sem opções, escrever é substituir a folha inteira do caderno por uma nova; com `APPEND`, é continuar escrevendo na primeira linha em branco depois do que já havia. Um cuidado adicional: para que um leitor externo nunca veja o arquivo pela metade, o padrão profissional é escrever num arquivo temporário e depois renomeá-lo para o nome final com `Files.move` — assunto da seção "Mover".

As alternativas são os `Writer` com buffer, quando a escrita é longa e feita em muitas etapas, o `PrintWriter` quando você quer `printf`, e bibliotecas de serialização. O que evitar: chamar `Files.writeString` dentro de um laço que grava milhares de vezes — cada chamada abre e fecha o arquivo do zero. Para isso, abra um `BufferedWriter` uma única vez (aula 5). E tenha sempre em mente que, sem `APPEND`, a escrita apaga todo o conteúdo anterior.

### Copiar

`Files.copy(origem, destino, opções...)` duplica um arquivo, deixando o original intacto. Há também as sobrecargas `copy(InputStream, Path)` — para gravar em arquivo o que vem de uma conexão ou de outra fonte — e `copy(Path, OutputStream)` — para despejar um arquivo num fluxo de saída. Por padrão, a cópia falha com `FileAlreadyExistsException` se o destino já existe.

As opções são do tipo `StandardCopyOption`: `REPLACE_EXISTING` autoriza sobrescrever o destino; `COPY_ATTRIBUTES` preserva metadados como data de modificação e permissões; `NOFOLLOW_LINKS` copia o link simbólico em si em vez do arquivo que ele aponta. Um detalhe importante: `Files.copy` sobre um diretório copia apenas o diretório (vazio), **não** o seu conteúdo — copiar uma árvore inteira exige percorrer com `Files.walk` ou `Files.walkFileTree`.

Na API antiga não existia método de cópia: todo projeto reescrevia o mesmo laço `while ((n = in.read(buf)) != -1) out.write(buf, 0, n)`, com buffer, dois streams e fechamento manual — repetitivo e fácil de errar num dos detalhes.

```java
Path orig = Path.of("dados/planilha.xlsx");
Path bkp = Path.of("backup/planilha.xlsx");
Files.createDirectories(bkp.getParent());
Files.copy(orig, bkp,
        StandardCopyOption.REPLACE_EXISTING,
        StandardCopyOption.COPY_ATTRIBUTES);

// gravar em arquivo o que chega de uma conexão
try (InputStream in = url.openStream()) {
    Files.copy(in, Path.of("download.bin"), StandardCopyOption.REPLACE_EXISTING);
}
```

É a fotocópia de um documento: o original continua na mesa, uma duplicata sai na bandeja. `COPY_ATTRIBUTES` é fotocopiar preservando também o carimbo de data do documento; sem essa opção, a cópia nasce com a data e a hora do momento em que foi feita.

As alternativas são `FileChannel.transferTo` para arquivos muito grandes, que delega a transferência ao sistema operacional, e ferramentas externas como `cp` ou `rsync` acionadas via `ProcessBuilder` para árvores de diretórios grandes. O que evitar: contar com `Files.copy` para copiar pastas com conteúdo (não é recursivo) e para sincronização incremental de muitos arquivos, onde uma ferramenta dedicada como `rsync` faz um trabalho melhor.

### Mover

`Files.move(origem, destino, opções...)` transfere um arquivo de um caminho para outro. Quando origem e destino estão no mesmo sistema de arquivos, é apenas uma renomeação: rápida, sem copiar bytes. Quando estão em sistemas de arquivos diferentes — partições ou discos distintos —, a JVM não tem como renomear e faz internamente uma cópia seguida da exclusão do original.

As opções são `REPLACE_EXISTING`, com o mesmo sentido de `copy`, e `ATOMIC_MOVE`, que garante que nenhum observador veja um estado intermediário: no caminho de destino existe ou o arquivo antigo, ou o novo já completo, nunca um arquivo em construção. Como `ATOMIC_MOVE` depende de a operação ser uma renomeação de verdade, ela pode lançar `AtomicMoveNotSupportedException` quando a movimentação cruza sistemas de arquivos. Na API antiga, o equivalente era `File.renameTo(File)`, que devolvia um `boolean`, tinha comportamento dependente do sistema operacional e não oferecia nenhuma garantia de atomicidade.

```java
// padrão "escreve temporário e publica": leitores nunca veem arquivo incompleto
Path tmp = Path.of("saida/relatorio.tmp");
Path fim = Path.of("saida/relatorio.pdf");
gerarRelatorio(tmp);
Files.move(tmp, fim,
        StandardCopyOption.REPLACE_EXISTING,
        StandardCopyOption.ATOMIC_MOVE);

// avançar um item na esteira de processamento
Files.move(Path.of("fila/novos/pedido-12.json"),
           Path.of("fila/processando/pedido-12.json"));
```

Mover, no mesmo sistema de arquivos, é trocar a etiqueta de endereço da caixa — o conteúdo não é reembalado, só o rótulo muda. Levar a caixa para outro depósito (outro sistema de arquivos) obriga a reembalar tudo, e é por isso que a movimentação atômica deixa de ser possível nesse caso.

As alternativas são copiar e depois excluir manualmente, quando você precisa de controle sobre cada etapa, e o legado `renameTo`, que é melhor evitar. Não use `ATOMIC_MOVE` quando origem e destino podem estar em sistemas de arquivos diferentes e você não pode tratar a exceção — mova sem a opção nesse cenário. E lembre que mover não mantém as duas cópias: se você quer preservar o original, a operação é `copy`, não `move`.

### Excluir

`Files.delete(path)` remove um arquivo ou diretório vazio. Ele lança `NoSuchFileException` se o alvo não existe e `DirectoryNotEmptyException` se é uma pasta com conteúdo — sinais claros de o que deu errado. A variante `Files.deleteIfExists(path)` devolve um `boolean` (`true` se apagou, `false` se não havia nada) e não reclama da ausência do alvo, o que a torna conveniente para limpeza de temporários.

Não existe um método pronto para apagar uma árvore de diretórios com conteúdo; o padrão é percorrer com `Files.walk` e apagar de dentro para fora, ordenando os caminhos em ordem reversa para que os filhos sejam removidos antes dos pais. Na API antiga, `File.delete()` devolvia um `boolean` que era rotineiramente ignorado, sem distinguir "não existia" de "não tenho permissão" — o que tornava qualquer diagnóstico impossível.

```java
Files.deleteIfExists(Path.of("cache/sessao.tmp")); // silêncio se não estiver lá

// apagar um diretório e todo o conteúdo
Path alvo = Path.of("build/tmp");
if (Files.exists(alvo)) {
    try (Stream<Path> paths = Files.walk(alvo)) {
        paths.sorted(Comparator.reverseOrder()) // filhos antes dos pais
             .forEach(p -> {
                 try { Files.delete(p); }
                 catch (IOException e) { throw new UncheckedIOException(e); }
             });
    }
}
```

`delete` é jogar fora um papel específico e reclamar se o papel — ou a lixeira — não está onde deveria; `deleteIfExists` é a instrução "se houver esse papel na mesa, jogue fora", que não gera drama nenhum se a mesa já estava limpa. A diferença entre os dois está inteiramente em como cada um reage à ausência do alvo.

Uma alternativa à exclusão imediata é mover o arquivo para uma pasta de quarentena própria, o que permite desfazer; outra é apenas marcá-lo como obsoleto num índice e limpar em lote mais tarde. Há ainda `File.deleteOnExit()`, que adia a remoção para o encerramento da JVM, mas ele acumula referências em memória e não roda se o processo morre de forma abrupta. Não use `delete` direto em dados que possam precisar de auditoria ou recuperação, e nunca rode `Files.walk` + `delete` sobre um caminho vindo de entrada não validada: um `/` ou `C:\` mal filtrado apaga o sistema inteiro.

## Streams de bytes

Os métodos de conveniência de `Files` resolvem o caso comum, mas por baixo deles há um modelo mais fundamental: os *streams*. Um stream é um fluxo de dados lido ou escrito em sequência, um pedaço de cada vez, sem precisar caber inteiro na memória. Os streams de bytes trabalham na unidade mais crua possível — o byte, um valor de 0 a 255 — e servem para qualquer conteúdo: imagens, PDFs, áudio, arquivos compactados e também texto, que no fundo é bytes mais uma codificação.

Esta aula apresenta as duas classes abstratas que são a base dessa família: `InputStream`, de onde se lê, e `OutputStream`, para onde se escreve. Todas as demais — `FileInputStream`, `ByteArrayOutputStream`, os streams de rede, os de criptografia e compressão — herdam delas e podem ser encaixadas umas nas outras como camadas.

### `InputStream`

`InputStream` é a classe abstrata de `java.io` que está no topo da hierarquia de leitura de bytes. Seu contrato central tem três métodos: `int read()` devolve o próximo byte como um valor de `0` a `255`, ou `-1` quando o fluxo acaba; `read(byte[] b)` preenche um bloco do array e devolve quantos bytes leu; `close()` libera o recurso subjacente. As implementações concretas incluem `FileInputStream` (lê de um arquivo), `ByteArrayInputStream` (lê de um array em memória), `System.in` (entrada padrão) e os streams devolvidos por sockets de rede.

Sem uma abstração comum, cada fonte de dados — arquivo, rede, memória — teria uma API própria, e nenhum código de processamento seria reaproveitável entre elas. O stream unifica: uma rotina que recebe um `InputStream` e conta bytes, calcula um hash ou procura um padrão funciona igual, sem saber nem se importar de onde os bytes vêm.

O uso correto é um laço até `-1`, lendo em blocos, dentro de um try-with-resources. Versões modernas acrescentam `readAllBytes()` (Java 9+), que traz tudo de uma vez, e `transferTo(OutputStream)`, que despeja o fluxo inteiro em uma saída.

```java
try (InputStream in = Files.newInputStream(Path.of("foto.jpg"))) {
    byte[] buffer = new byte[8192];
    int lidos;
    long total = 0;
    while ((lidos = in.read(buffer)) != -1) {
        total += lidos;              // processa buffer[0..lidos)
    }
    System.out.println("bytes lidos: " + total);
}
```

Repare que `read()` devolve `int`, não `byte`: é isso que permite o valor `-1` de "acabou" conviver com os 256 valores de byte válidos sem ambiguidade.

Um `InputStream` é um canudo ligado a um reservatório: você suga o conteúdo em goles, sempre na mesma ordem, sem nunca ver o reservatório inteiro, e em algum gole percebe que secou — esse é o `-1`. O reservatório do outro lado do canudo tanto faz ser um arquivo, uma conexão de rede ou um bloco de memória; o jeito de beber é o mesmo.

As alternativas são `Files.readAllBytes` para arquivos pequenos, o par `ByteBuffer` + `FileChannel` para I/O de alto desempenho, e o `Reader` quando o conteúdo é texto e você quer trabalhar em `char` já decodificado. Não use um `InputStream` cru diretamente para texto — empilhe um `InputStreamReader` por cima, senão você mesmo terá de decodificar bytes em caracteres. E não faça leituras byte a byte sem um `BufferedInputStream` no meio: sem ele, cada `read()` é uma chamada ao sistema operacional, e o desempenho despenca.

### `OutputStream`

`OutputStream` é a classe abstrata simétrica, no topo da hierarquia de escrita de bytes. Seu contrato tem `write(int b)` para gravar um byte, `write(byte[] b, int off, int len)` para gravar um bloco, `flush()` para forçar a saída de dados que estejam retidos e `close()` para fechar — o que normalmente inclui um `flush`. As implementações concretas incluem `FileOutputStream` (grava em arquivo), `ByteArrayOutputStream` (acumula em memória e entrega tudo com `toByteArray()`), as saídas de sockets e `System.out`.

O motivo de existir é o mesmo do `InputStream`: sem uma base comum, "escrever para arquivo" e "escrever para a rede" seriam APIs diferentes. Com `OutputStream`, um gerador de PDF ou de relatório escreve num `OutputStream` genérico, e quem chama decide se aquele fluxo vira um arquivo no disco, o corpo de uma resposta HTTP ou um array em memória.

O uso correto é escrever em blocos, chamar `flush()` quando for preciso garantir que os dados já saíram antes do fechamento, e sempre fechar — de novo, via try-with-resources.

```java
try (OutputStream out = Files.newOutputStream(Path.of("saida.bin"),
        StandardOpenOption.CREATE, StandardOpenOption.TRUNCATE_EXISTING)) {
    byte[] cabecalho = { 0x4D, 0x5A };
    out.write(cabecalho);
    out.write(corpo, 0, corpo.length);
}   // close() faz flush e libera o arquivo

// montar bytes em memória, sem tocar o disco
var buf = new ByteArrayOutputStream();
buf.write("linha\n".getBytes(StandardCharsets.UTF_8));
byte[] resultado = buf.toByteArray();
```

Sobre o `flush`: streams podem segurar dados num buffer interno em vez de enviá-los na hora; `flush()` empurra esse conteúdo retido para o destino, e `close()` chama `flush()` antes de fechar. O risco real é escrever tudo e nunca fechar — aí o trecho final some.

Um `OutputStream` é uma esteira que leva caixas até um caminhão: `write` põe caixas na esteira, `flush` manda o caminhão partir mesmo sem estar cheio, e `close` encerra o expediente da doca depois de despachar o que restava. Sem `flush` nem `close`, as últimas caixas ficam paradas na esteira e nunca chegam ao destino.

As alternativas são `Files.write` quando o conteúdo já está pronto num `byte[]`, o `Writer` quando o conteúdo é texto, e `FileChannel` quando se busca o máximo de desempenho. Não use `OutputStream` cru para texto — coloque um `OutputStreamWriter` por cima, com charset explícito. Para muitas chamadas `write` pequenas, envolva num `BufferedOutputStream`. E, se o conteúdo já está inteiro num array, `Files.write` é mais simples do que abrir e gerenciar um stream.

## Streams de caracteres

Texto não é a mesma coisa que bytes. Entre os dois há sempre uma *codificação de caracteres* — UTF-8, ISO-8859-1, Windows-1252 — que decide como cada letra, acento ou emoji vira uma sequência de bytes, e vice-versa. Os streams de caracteres existem para cuidar dessa tradução: eles trabalham em `char`, a unidade de texto, em vez de `byte`, e carregam um `Charset` que faz a conversão nos bastidores.

Esta aula cobre as duas classes-base: `Reader`, para ler texto, e `Writer`, para escrever texto. Elas espelham `InputStream` e `OutputStream`, só que na dimensão de caracteres — e a ponte entre os dois mundos são as classes `InputStreamReader` e `OutputStreamWriter`, que recebem um stream de bytes mais um charset e fazem a conversão.

### `Reader`

`Reader` é a classe abstrata de `java.io` para leitura de fluxos de caracteres. Seu `int read()` devolve o próximo `char` como um valor de `0` a `65535`, ou `-1` no fim; `read(char[])` lê um bloco de caracteres de uma vez. As subclasses incluem `FileReader`, `StringReader` (lê de uma `String`), `CharArrayReader`, `InputStreamReader` (adapta um fluxo de bytes para caracteres) e `BufferedReader`, tema da próxima aula.

Com apenas um `InputStream`, ler a palavra `"café"` de um arquivo UTF-8 traz cinco bytes, e transformá-los de volta em texto exige aplicar à mão as regras da codificação — algo inviável de fazer manualmente para UTF-8 ou UTF-16. Usar o charset errado nessa conversão produz o clássico `cafÃ©`, o fenômeno conhecido como *mojibake*.

O `Reader` resolve isso decodificando byte em `char` de acordo com o `Charset` informado. A regra prática é inegociável: **sempre passe o charset explicitamente**. O padrão da plataforma varia de máquina para máquina — embora o Java 18 e versões seguintes fixem UTF-8 como default, código que precisa rodar em qualquer versão não pode contar com isso.

```java
Path p = Path.of("mensagens.txt");
try (Reader r = Files.newBufferedReader(p, StandardCharsets.UTF_8)) {
    char[] buf = new char[1024];
    int n;
    StringBuilder sb = new StringBuilder();
    while ((n = r.read(buf)) != -1) {
        sb.append(buf, 0, n);
    }
}

// adaptando um fluxo de bytes qualquer (ex.: de rede) para texto
try (Reader r = new InputStreamReader(conexao.getInputStream(), StandardCharsets.UTF_8)) {
    // ...
}
```

Se o `InputStream` entrega o conteúdo num "idioma de bytes", o `Reader` é o intérprete que o traduz para caracteres legíveis — e, para acertar a tradução, ele precisa saber qual idioma está ouvindo. Informar o `Charset` é exatamente dizer ao intérprete a língua de origem.

As alternativas são `Files.readString` e `Files.readAllLines` para arquivos pequenos lidos de uma vez, o `Scanner` para extrair valores tipados (`nextInt`, `nextLine`), e bibliotecas de parsing de CSV ou JSON que já embutem o `Reader`. Não use `Reader` para conteúdo binário — decodificar bytes arbitrários de uma imagem ou de um `.zip` como se fossem caracteres corrompe os dados; aí o certo é `InputStream`. E, para leitura linha a linha eficiente, use um `BufferedReader`, não o `Reader` cru.

### `Writer`

`Writer` é a classe abstrata simétrica, para escrever caracteres. Ela oferece `write(String)`, `write(char[])`, `write(int c)`, `append(CharSequence)`, além de `flush()` e `close()`, e codifica cada `char` em bytes segundo o `Charset`. As subclasses incluem `FileWriter`, `StringWriter` (acumula o texto numa `String` em memória), `OutputStreamWriter` (a ponte de `char` para bytes), `BufferedWriter` e `PrintWriter`, que acrescenta `printf` e `println`.

Escrever texto por um `OutputStream` obriga a converter cada `String` em `byte[]` manualmente, com o charset certo, em toda chamada — repetitivo e fácil de esquecer, o que gera arquivos cujos acentos aparecem quebrados quando abertos em outro sistema. O `Writer` faz esse `getBytes(charset)` por você, de forma consistente, para tudo que passar por ele. De novo, a regra é usar **charset explícito**.

```java
Path saida = Path.of("relatorio.csv");
try (Writer w = Files.newBufferedWriter(saida, StandardCharsets.UTF_8,
        StandardOpenOption.CREATE, StandardOpenOption.TRUNCATE_EXISTING)) {
    w.write("nome,idade\n");
    w.write("José,42\n");
    w.append("Ana,29\n");
}   // close() faz flush do que faltava e fecha

// gerar texto em memória (útil em testes)
var sw = new StringWriter();
sw.write("conteúdo");
String texto = sw.toString();
```

Quando você quer formatação, empilha um `PrintWriter` por cima:

```java
try (var pw = new PrintWriter(Files.newBufferedWriter(saida, StandardCharsets.UTF_8))) {
    pw.printf("Total: %.2f%n", 1234.5);
}
```

Como todo stream com buffer, o `Writer` pode reter caracteres em memória; se o programa termina sem `close`, as últimas linhas se perdem. O try-with-resources garante o fechamento e, com ele, o `flush` final.

O `Writer` é o tradutor na direção inversa do `Reader`: você fala em caracteres, ele escreve no "idioma de bytes" do arquivo usando o charset como dicionário. É esse `Charset` que decide como cada `char` é gravado em bytes no disco.

As alternativas são `Files.writeString` e `Files.write(List)` para conteúdo já pronto, o `OutputStream` para dados binários, e bibliotecas que serializam objetos diretamente para arquivo. Não use `Writer` para conteúdo binário — enviar o `byte[]` de uma imagem por um `Writer` faz a codificação corromper os dados. E, para uma única escrita curta de texto já montado, `Files.writeString` é mais direto do que abrir e gerenciar um `Writer`.

## Buffers

Ler ou escrever um byte — ou um caractere — de cada vez diretamente no disco é lento: cada operação vira uma chamada ao sistema operacional, e o disco responde em blocos, não em unidades mínimas. A solução é o *buffer*: uma área de memória intermediária que acumula muitos bytes e conversa com o disco de uma vez só. As classes `Buffered*` de `java.io` são decoradores — você embrulha um stream cru dentro de uma delas e ganha o buffer sem mudar como o resto do código lê ou escreve.

Esta aula cobre `BufferedReader` e `BufferedWriter`, a versão para texto (com o valioso `readLine()`), os *buffered streams* de bytes, e o `try-with-resources` — a construção da linguagem que garante que todo esse encanamento seja fechado corretamente, mesmo quando uma exceção interrompe o fluxo.

### `BufferedReader`

`BufferedReader` é um `Reader` que embrulha outro `Reader` e lhe adiciona duas coisas: um buffer interno e o método `readLine()`, que devolve uma linha inteira de texto — já sem o `\n` do fim — ou `null` quando o arquivo acaba. Ele também expõe `lines()`, que entrega um `Stream<String>` das linhas.

Sem ele, ler linha a linha a partir de um `Reader` cru significa acumular `char` num `StringBuilder` até encontrar o `\n`, um trecho de código chato que se repete em todo projeto. E ler caractere por caractere direto do arquivo é lento, porque cada leitura pode virar uma chamada ao sistema operacional.

O `BufferedReader` resolve os dois lados. O buffer traz um bloco grande do disco de uma vez — 8 KB por padrão —, e as chamadas seguintes de `read` ou `readLine` se servem da memória, sem novo acesso ao disco. E `readLine()` entrega a abstração de "linha" já pronta, que é como quase todo arquivo de texto é processado.

```java
Path log = Path.of("/var/log/app.log");
try (BufferedReader br = Files.newBufferedReader(log, StandardCharsets.UTF_8)) {
    String linha;
    int total = 0;
    while ((linha = br.readLine()) != null) {
        if (linha.contains("ERROR")) total++;
    }
    System.out.println("erros: " + total);
}

// a mesma coisa, com Stream
try (BufferedReader br = Files.newBufferedReader(log, StandardCharsets.UTF_8)) {
    long erros = br.lines().filter(l -> l.contains("ERROR")).count();
}
```

Quando a origem é uma conexão de rede, o padrão é montar à mão: `new BufferedReader(new InputStreamReader(in, StandardCharsets.UTF_8))`.

Em vez de ir até a caixa d'água encher um copo toda vez que sente sede — uma ida ao disco por caractere —, você enche uma jarra de uma vez e serve vários copos dela. Essa jarra é o bloco de 8 KB que o `BufferedReader` mantém em memória entre as leituras.

As alternativas são `Files.readAllLines`, que entrega uma lista pronta mas só serve para arquivos pequenos, `Files.lines`, que dá um `Stream` e se fecha sozinho no try-with-resources, e o `Scanner` para ler tokens tipados. Não vale a pena empilhar um `BufferedReader` se você já lê o arquivo inteiro de uma vez com `Files.readString`. E `readLine()` não faz sentido para conteúdo binário nem para formatos em que "linha" não significa nada.

### `BufferedWriter`

`BufferedWriter` é um `Writer` que embrulha outro `Writer`, acumula a saída num buffer interno e só a envia ao destino em blocos — quando o buffer enche, ou quando `flush()` / `close()` é chamado. Ele acrescenta o método `newLine()`, que insere o separador de linha próprio do sistema operacional em vez de um `\n` fixo.

Sem o buffer, cada `write` pequeno vira uma escrita no disco. Um laço que grava cem mil linhas curtas direto num `FileWriter` faz cem mil idas ao sistema operacional — ordens de magnitude mais lento do que agrupar essas escritas. O `BufferedWriter` junta tudo e despeja de uma vez quando o buffer se enche.

```java
Path saida = Path.of("export/clientes.csv");
Files.createDirectories(saida.getParent());
try (BufferedWriter bw = Files.newBufferedWriter(saida, StandardCharsets.UTF_8,
        StandardOpenOption.CREATE, StandardOpenOption.TRUNCATE_EXISTING)) {
    bw.write("id,nome");
    bw.newLine();
    for (Cliente c : clientes) {
        bw.write(c.id() + "," + c.nome());
        bw.newLine();
    }
}   // close() faz flush do restante e fecha
```

Há uma ressalva sobre o `flush`: se outro processo precisa enxergar o conteúdo *enquanto* o seu programa ainda está rodando — um arquivo de progresso lido por um painel, por exemplo —, chame `bw.flush()` nos pontos certos, senão os dados ficam presos no buffer. No fluxo normal, em que só interessa o arquivo final, o `close` do try-with-resources já cobre isso.

Em vez de atravessar a rua para postar cada carta assim que termina de escrevê-la, você junta a correspondência do dia numa sacola e faz uma única viagem aos Correios. A sacola é o buffer; a viagem é a escrita real no disco, feita em bloco.

As alternativas são `Files.write(path, List<String>)` ou `Files.writeString` quando todo o conteúdo já está montado em memória, e o `PrintWriter` sobre o `BufferedWriter` quando você quer `printf`. Para uma única escrita de um texto já pronto, `Files.writeString` é mais simples e igualmente eficiente. E não esqueça: um `BufferedWriter` sem `close` — ou ao menos sem `flush` — pode terminar com o arquivo incompleto; nunca confie no buffer sem garantir o fechamento.

### Buffered streams

`BufferedInputStream` e `BufferedOutputStream` são os equivalentes de bytes das classes anteriores — decoradores que colocam um buffer entre o seu código e um `InputStream` ou `OutputStream` cru. O princípio é idêntico ao da versão de caracteres, só que a unidade acumulada é o byte.

Sem eles, `FileInputStream.read()` byte a byte é uma chamada ao sistema operacional por byte; copiar um arquivo de 10 MB assim pode levar segundos onde deveria levar milissegundos. `FileOutputStream.write(b)` sofre do mesmo problema na escrita. O `BufferedInputStream` lê um bloco grande do arquivo de uma vez e serve os `read` seguintes a partir da memória; o `BufferedOutputStream` acumula os `write` e descarrega em bloco. Se o seu código já lê e escreve em blocos grandes com `read(byte[])` e `write(byte[])`, o ganho é pequeno — o buffer brilha justamente quando as chamadas são miúdas e frequentes.

```java
try (var in = new BufferedInputStream(Files.newInputStream(origem));
     var out = new BufferedOutputStream(Files.newOutputStream(destino,
             StandardOpenOption.CREATE, StandardOpenOption.TRUNCATE_EXISTING))) {
    int b;
    while ((b = in.read()) != -1) {   // byte a byte, mas rápido: vem do buffer
        out.write(b);
    }
}   // o close() de 'out' faz flush; o try-with-resources fecha os dois
```

Repare que o try-with-resources fecha os recursos na ordem inversa da declaração — `out` primeiro, com o seu `flush`, e depois `in` —, que é exatamente a ordem desejada aqui.

É a mesma jarra d'água e a mesma sacola de cartas das seções anteriores, agora medindo o conteúdo em bytes em vez de caracteres: o buffer de bytes evita uma ida ao sistema operacional por byte, assim como o de caracteres evitava uma por `char`.

As alternativas são ler e escrever com um `byte[]` grande manualmente, sem decorador, o que por si só já reduz o número de chamadas ao sistema, `InputStream.transferTo(OutputStream)`, que copia com um buffer interno numa linha, `Files.copy` para o caso arquivo-para-arquivo, e `FileChannel.transferTo` para o máximo desempenho com arquivos grandes. Não empilhe um buffered stream sobre uma fonte que já vive em memória, como `ByteArrayInputStream` — é só gasto de memória sem ganho — nem quando você faz poucas leituras grandes, em que o buffer extra não muda nada.

### Try-with-resources

O `try-with-resources` é a forma do `try` que declara, entre parênteses, um ou mais recursos — objetos que implementam `AutoCloseable` — e chama `close()` em cada um ao sair do bloco, tanto na saída normal quanto quando uma exceção interrompe a execução. Ele já foi coberto no Módulo 8, junto com as garantias de ordem de fechamento e o mecanismo de *suppressed exceptions*, que faz a exceção do corpo prevalecer sobre uma falha no próprio `close()` em vez de ser mascarada por ela.

No contexto de arquivos, o ponto a fixar é que **todo stream, `Reader`, `Writer` e decorador `Buffered*` implementa `AutoCloseable`**: todo o encanamento de I/O deste módulo se encaixa direto no try-with-resources. E esquecer o `close` tem consequência concreta aqui — cada arquivo aberto e não fechado vaza um descritor, e depois de muitos o processo falha com "too many open files".

Vários recursos vão no mesmo cabeçalho, separados por `;`, e são fechados na ordem inversa da declaração — no par leitor/escritor abaixo, o `BufferedWriter` fecha primeiro, fazendo seu `flush`, e só depois o `BufferedReader`:

```java
Path entrada = Path.of("dados/bruto.csv");
Path saida = Path.of("dados/limpo.csv");
try (BufferedReader r = Files.newBufferedReader(entrada, StandardCharsets.UTF_8);
     BufferedWriter w = Files.newBufferedWriter(saida, StandardCharsets.UTF_8)) {
    String linha;
    while ((linha = r.readLine()) != null) {
        if (!linha.isBlank()) {
            w.write(linha.strip());
            w.newLine();
        }
    }
}   // w.close() (com flush) e depois r.close(), aconteça o que acontecer
```

É como uma porta com mola de retorno: você entra para fazer o que precisa e ela se fecha sozinha quando você sai, mesmo que a saída seja às pressas por causa de um imprevisto — a exceção. Esse "fechar sozinho" é a chamada automática de `close()` em cada recurso declarado.

Nem toda leitura ou escrita precisa dele: os métodos de conveniência de `Files` — `readString`, `writeString`, `write` e afins — abrem e fecham o arquivo internamente. A exceção é `Files.lines`, cujo `Stream` mantém o arquivo aberto e por isso exige um try-with-resources. Fora esses utilitários, todo stream, `Reader` ou `Writer` que você abrir explicitamente deve estar num try-with-resources — não há motivo para não usar.

---

# Módulo 10 — Serialização e cópia de objetos

Serialização é o processo de converter um objeto em bytes para armazenar ou transmitir, e desserialização é o reverso. Java oferece um mecanismo pronto via `Serializable`, mas com armadilhas de segurança e compatibilidade. Cópia de objetos também é uma necessidade frequente com seus próprios perigos. Este módulo explora ambas as operações, suas segurança e as alternativas.

## `Serializable`

A serialização nativa do Java começa por um contrato mínimo: marcar a classe com a interface `java.io.Serializable`. Sem essa marca, tentar transformar o objeto em bytes falha na hora. Com ela, a plataforma assume a tarefa de percorrer os campos, gravá-los e depois reconstruir tudo.

Esta aula cobre os três momentos do ciclo mais básico: o que a marca significa e por que ela é só uma etiqueta (Interface marker), o que acontece quando um objeto vira uma sequência de bytes (Serialização) e como esses bytes voltam a ser um objeto vivo na memória (Desserialização). Os streams concretos, o controle de versão e as defesas de segurança vêm nas aulas seguintes.

### Interface marker

`Serializable` é uma *interface de marcação* (marker interface): uma interface que não declara nenhum método. Ela não obriga a classe a implementar comportamento algum; serve apenas como uma etiqueta que o runtime consulta, por reflexão, para decidir se aquele tipo pode ou não ser serializado. Implementá-la é escrever `class Pedido implements Serializable {}` e nada mais.

Sem esse mecanismo de opt-in, a plataforma teria duas opções ruins: serializar qualquer objeto por padrão, ou nunca serializar nada. A primeira é perigosa — muitos objetos guardam estado que não faz sentido fora do processo (uma conexão de banco, uma `Thread`, um handle de arquivo) ou informação sensível (uma senha em memória), e transformá-los em bytes silenciosamente quebraria invariantes e vazaria dados. A segunda inviabilizaria persistência e comunicação remota.

A interface de marcação resolve isso exigindo uma decisão consciente do autor da classe. Quando você chama `writeObject`, o `ObjectOutputStream` testa `objeto instanceof Serializable`; se o teste falha, ele lança `NotSerializableException` imediatamente, dizendo qual classe não estava marcada.

```java
import java.io.*;

class Produto implements Serializable {
    String nome;
    double preco;
}

class Sessao { // NÃO implementa Serializable
    Socket conexao;
}
```

Tentar gravar um `Produto` funciona; tentar gravar uma `Sessao` lança `NotSerializableException: Sessao`. Se `Produto` tivesse um campo do tipo `Sessao` não marcado como `transient`, a gravação também falharia ao chegar nesse campo.

A analogia é o adesivo "frágil — pode ser transportada" colado numa caixa de mudança. O adesivo não muda o conteúdo da caixa nem como ela é feita; ele apenas autoriza e orienta quem for manuseá-la. Aqui, "quem manuseia" é a própria JVM durante a serialização, e a ausência do adesivo é lida como "não transporte".

Alternativas à marca simples existem: `Externalizable` é uma sub-interface que, além de marcar, exige métodos de leitura e escrita; e bibliotecas como Jackson ou Gson serializam para JSON sem exigir nenhuma interface, guiando-se por getters ou pelos próprios campos. Não faz sentido implementar `Serializable` em classes que só trafegam como JSON numa API REST, em objetos de vida puramente local, ou em tipos cujo estado não é portável — nesses casos a marca só cria uma falsa expectativa de que serializar aquilo é seguro.

### Serialização

Serialização é o ato de converter um objeto — e, por tabela, todos os objetos que ele referencia — em uma sequência linear de bytes no formato binário definido pela plataforma Java. O ponto de entrada é `ObjectOutputStream.writeObject(objeto)`, que grava esses bytes em qualquer `OutputStream`: um arquivo, um socket, um `ByteArrayOutputStream` na memória. É a metade "de ida" do módulo, usada para persistir estado em disco, guardar objetos em cache, enviá-los pela rede em RMI ou produzir uma cópia profunda passando pelos bytes.

Sem um mecanismo automático, gravar um objeto significa escrever, campo a campo, cada valor em algum formato próprio e manter esse código sincronizado com a classe: toda vez que um atributo nasce, some ou muda de tipo, o método de gravação precisa acompanhar, sob pena de arquivos corrompidos ou incompletos. Para grafos com muitos objetos aninhados, esse código manual cresce rápido e erra nos casos de borda.

A serialização nativa resolve percorrendo a classe por reflexão. Ela grava um *descritor* — nome da classe, seu `serialVersionUID` e a lista de campos com seus tipos — seguido dos valores dos campos de instância que não são `static` nem `transient`. Campos primitivos vão direto; campos que são objetos disparam a serialização recursiva desses objetos. O resultado é autossuficiente: carrega consigo a estrutura necessária para a remontagem.

```java
List<Produto> catalogo = List.of(new Produto("Café", 21.90), new Produto("Chá", 15.00));

try (var out = new ObjectOutputStream(Files.newOutputStream(Path.of("catalogo.ser")))) {
    out.writeObject(catalogo);
}
```

A analogia é desmontar um armário para uma mudança: cada prateleira, dobradiça e parafuso vai etiquetado dentro de uma caixa, junto de um manual que diz como tudo se encaixa de volta. A caixa (o arquivo de bytes) não é o armário, mas contém o suficiente para reconstruí-lo idêntico noutro lugar.

As alternativas são formatos explícitos: JSON via Jackson ou Gson, quando o dado precisa ser legível ou consumido por outras linguagens; Protocol Buffers ou Avro, quando tamanho e velocidade importam e o esquema é versionado com disciplina; XML, em integrações legadas. Evite a serialização nativa quando o arquivo ou a mensagem for lido por sistemas fora do mundo Java, quando o formato precisar permanecer estável por anos de forma controlada, ou quando você precisar inspecionar o conteúdo a olho nu — o formato binário do Java não atende nenhum desses casos.

### Desserialização

Desserialização é o caminho de volta: a partir de uma sequência de bytes produzida pela serialização, reconstruir na memória o objeto original e o grafo que pendia dele. O método é `ObjectInputStream.readObject()`, que devolve um `Object` — você faz o *cast* para o tipo esperado. Junto com a serialização, fecha o ciclo de persistir e recuperar estado.

Um detalhe decisivo distingue a desserialização de criar um objeto normalmente: **o construtor da classe serializável não é chamado**. A plataforma aloca a instância por um mecanismo interno especial e preenche os campos diretamente, por reflexão, com os valores lidos do stream. O único construtor executado é o construtor sem argumentos da primeira superclasse *não* serializável na hierarquia. Isso significa que qualquer validação ou inicialização que você tenha colocado nos seus construtores é simplesmente pulada.

Sem cuidado, isso vira um problema: os bytes podem ter sido adulterados, podem representar uma versão antiga e incompatível da classe, ou podem exigir uma classe que não está no *classpath*. A desserialização sinaliza esses casos com exceções — `ClassNotFoundException` quando o tipo não é encontrado, `InvalidClassException` quando o `serialVersionUID` do stream não bate com o da classe carregada, `StreamCorruptedException` quando os bytes não formam um stream válido.

A forma de reagir a isso é tratar o resultado como não confiável até prova em contrário. Se a classe tem invariantes (um campo que nunca pode ser nulo, uma data de início sempre anterior à de fim), implemente um método `readObject` privado que revalide essas condições depois da leitura e lance exceção se algo estiver errado — o assunto da aula de serialização avançada.

```java
try (var in = new ObjectInputStream(Files.newInputStream(Path.of("catalogo.ser")))) {
    @SuppressWarnings("unchecked")
    List<Produto> catalogo = (List<Produto>) in.readObject();
    System.out.println(catalogo.size());
} catch (InvalidClassException e) {
    System.err.println("versão da classe incompatível com o arquivo");
}
```

A analogia é remontar o armário da mudança a partir da caixa de peças e do manual: o resultado se parece com o móvel de origem, mas você não o comprou de novo na loja — ou seja, não passou pela "linha de montagem" (o construtor). Se faltar um parafuso ou o manual for de outro modelo, a remontagem trava.

Alternativas que constroem objetos passando pelos construtores ou por *builders* — os parsers de JSON, por exemplo — são mais previsíveis nesse ponto, porque a inicialização normal da classe acontece. E há um cenário em que a desserialização nativa simplesmente não deve ser usada: sobre bytes vindos de uma fonte não confiável, como uma requisição de rede, sem antes aplicar filtros — o motivo é o tema dos riscos de segurança, mais adiante.

## Object Streams

A aula anterior falou do contrato; esta apresenta as ferramentas concretas que o executam. `ObjectOutputStream` e `ObjectInputStream` são os *object streams*: decoradores que se encaixam sobre um stream de bytes comum e sabem traduzir objetos Java de e para o formato de serialização.

Você vai ver como cada um se conecta a um stream subjacente e quais métodos usa (`ObjectOutputStream` e `ObjectInputStream`), e o que acontece quando o objeto gravado referencia outros objetos, que por sua vez referenciam outros — a estrutura que a serialização precisa tratar com cuidado para não se perder (Grafos de objetos).

### `ObjectOutputStream`

`ObjectOutputStream` é a classe que implementa a serialização. Ela é um *stream de filtro*: não escreve em lugar nenhum sozinha, mas embrulha outro `OutputStream` — um `FileOutputStream`, um `ByteArrayOutputStream`, o *output* de um `Socket` — e adiciona a esse destino a capacidade de receber objetos inteiros. No construtor você passa o stream de baixo; a partir daí, cada `writeObject` converte um objeto em bytes e os empurra para lá.

Sem essa classe, você teria em mãos apenas os métodos de escrever bytes e primitivos crus. Transformar um objeto nesses bytes — gravar o descritor da classe, percorrer os campos, tratar referências repetidas, lidar com herança — seria trabalho manual repetido em cada projeto e refeito a cada mudança de classe.

`ObjectOutputStream` encapsula tudo isso. Além de `writeObject(Object)`, ela oferece `writeInt`, `writeUTF`, `writeDouble` e afins para escrever primitivos avulsos no mesmo stream, e o método `defaultWriteObject`, usável apenas de dentro de um `writeObject` customizado. Um cuidado prático: ela mantém internamente uma tabela dos objetos já gravados (veja "Grafos de objetos"); se você serializa um objeto, altera seu estado e o serializa de novo no mesmo stream, a segunda escrita reaproveita a referência antiga e ignora a mudança. Para esses casos existe `reset()`, que limpa a tabela.

```java
try (var oos = new ObjectOutputStream(
         new BufferedOutputStream(Files.newOutputStream(Path.of("estado.ser"))))) {
    oos.writeUTF("cabeçalho v1");
    oos.writeInt(42);
    oos.writeObject(catalogo);
    oos.flush();
}
```

Pense nela como uma máquina de empacotamento a vácuo instalada na saída de uma esteira: os produtos (objetos) chegam soltos, a máquina os sela em plástico padronizado (bytes) e o pacote segue pela esteira que já existia (o stream subjacente). A máquina não move a esteira; ela só transforma o que passa.

Valem aqui as mesmas alternativas e ressalvas da seção *Serialização* — bibliotecas de JSON ou de formato binário próprio que escrevem direto no `OutputStream`. O motivo extra para preferi-las no lado da escrita é o volume: o formato nativo é mais verboso e lento do que um binário projetado para densidade.

### `ObjectInputStream`

`ObjectInputStream` é o par simétrico: implementa a desserialização. Também é um stream de filtro — envolve um `InputStream` qualquer (arquivo, rede, memória) e lê dele a sequência de bytes produzida por um `ObjectOutputStream`, reconstruindo os objetos. O método central é `readObject()`, que retorna `Object` e exige *cast*; para os primitivos gravados avulsos há `readInt`, `readUTF` e companhia, chamados **na mesma ordem** em que foram escritos.

Sem essa classe, você teria os bytes e nenhuma forma prática de voltar deles a um grafo de objetos: seria preciso interpretar o descritor de classe, alocar instâncias, casar campos por nome e tipo, refazer referências compartilhadas — exatamente o serviço que ela presta.

Ao ler, `ObjectInputStream` carrega a classe correspondente pelo nome (daí `ClassNotFoundException` ser possível), confere o `serialVersionUID`, aloca a instância sem chamar o construtor da classe serializável e preenche os campos. Se a classe define um `readObject` privado, ele é invocado para dar os retoques finais. É também aqui que mora o maior risco de segurança do mecanismo: `readObject` pode acabar instanciando *qualquer* classe presente no *classpath* mencionada pelo stream, o que abre espaço para ataques — por isso existem os filtros de desserialização, vistos adiante.

```java
try (var ois = new ObjectInputStream(
         new BufferedInputStream(Files.newInputStream(Path.of("estado.ser"))))) {
    String cabecalho = ois.readUTF();   // mesma ordem da escrita
    int versao = ois.readInt();
    @SuppressWarnings("unchecked")
    List<Produto> catalogo = (List<Produto>) ois.readObject();
}
```

Se a máquina de empacotamento a vácuo era o `ObjectOutputStream`, o `ObjectInputStream` é a estação que recebe os pacotes selados, corta o plástico e devolve cada produto à bancada — desde que os pacotes tenham vindo daquela máquina e no formato esperado. Um pacote de outra fábrica, ou danificado, trava a estação.

A alternativa é desserializar com um parser que constrói os objetos pelos construtores normais, o que costuma ser mais seguro e previsível. E a regra prática mais importante: nunca aponte um `ObjectInputStream` para bytes de origem não confiável sem um `ObjectInputFilter` restringindo quais classes podem ser instanciadas.

### Grafos de objetos

Um objeto quase nunca está sozinho. Um `Pedido` referencia um `Cliente`, uma lista de `ItemPedido`, cada item referencia um `Produto`, e o `Produto` pode referenciar de volta uma `Categoria`. Esse emaranhado de objetos ligados por referências é um *grafo de objetos*: os objetos são os nós, as referências entre eles são as arestas. Serializar "um" objeto significa, na prática, serializar todo o grafo alcançável a partir dele.

O desafio surge de duas propriedades comuns desses grafos. Primeira, o *compartilhamento*: dois itens de pedido diferentes podem apontar para o mesmo objeto `Produto`. Segunda, os *ciclos*: se `Produto` aponta para `Categoria` e `Categoria` mantém uma lista dos seus produtos, seguir as referências ingenuamente entra em recursão infinita. Uma serialização mal feita duplicaria o `Produto` compartilhado (voltariam dois objetos distintos na leitura) ou travaria no ciclo.

A serialização nativa do Java trata os dois casos automaticamente. O `ObjectOutputStream` mantém uma tabela que associa cada objeto já gravado a um número de série (um *handle*). Na primeira vez que encontra um objeto, grava-o por inteiro e registra o *handle*; nas vezes seguintes, grava apenas uma referência a esse *handle*. Na leitura, o `ObjectInputStream` reconstrói a mesma tabela, de modo que as referências repetidas voltam a apontar para a **mesma** instância, e os ciclos se fecham corretamente.

```java
Categoria bebidas = new Categoria("Bebidas");
Produto cafe = new Produto("Café", 21.90, bebidas);
bebidas.produtos.add(cafe);            // ciclo: categoria -> produto -> categoria

var pedido = new Pedido();
pedido.itens.add(new ItemPedido(cafe, 2));
pedido.itens.add(new ItemPedido(cafe, 1)); // mesmo Produto em dois itens

// depois de gravar e reler 'pedido':
ItemPedido a = pedido2.itens.get(0);
ItemPedido b = pedido2.itens.get(1);
System.out.println(a.produto == b.produto); // true — a identidade compartilhada é preservada
```

A analogia é a de mapear uma rede de amizades: se você anota "João conhece Maria" e "Maria conhece João", seguir as setas para sempre não termina; a solução é numerar cada pessoa ao encontrá-la e, quando ela reaparecer, escrever só o número em vez de recopiar a ficha inteira. O *handle* da serialização é esse número.

A alternativa manual — atravessar o grafo você mesmo mantendo um `IdentityHashMap` de visitados — é o que bibliotecas de cópia profunda e alguns serializadores JSON fazem internamente, e o que você faria à mão numa cópia profunda sob medida. O ponto de atenção: como o grafo alcançável é serializado por inteiro, um campo aparentemente inocente pode arrastar meio sistema para os bytes. Marque com `transient` tudo que não deve ir junto, para não serializar mais do que pretende.

## Controle da serialização

A serialização automática é conveniente, mas amarra o formato dos bytes à estrutura interna da classe — e classes evoluem. Esta aula trata dos mecanismos que dão controle sobre essa relação ao longo do tempo.

Você vai ver o identificador que funciona como impressão digital da versão da classe (`serialVersionUID`), a palavra-chave que exclui um campo da serialização (`transient`) e as regras que dizem quais mudanças em uma classe mantêm ou quebram a capacidade de ler bytes gravados por uma versão anterior (Compatibilidade de versões).

### `serialVersionUID`

`serialVersionUID` é um número `long` que identifica a versão da definição de uma classe serializável. Ele é gravado no stream durante a serialização e conferido na desserialização: se o valor presente nos bytes não for igual ao da classe carregada na JVM que está lendo, o `readObject` lança `InvalidClassException` e a leitura falha. A ideia é impedir que bytes gravados por uma versão da classe sejam interpretados por outra versão incompatível.

O problema está no que acontece quando você **não** declara esse campo. Nesse caso a JVM calcula um valor automaticamente, a partir de um *hash* do nome da classe, seus modificadores, interfaces, campos e métodos. Esse cálculo é extremamente sensível: adicionar um método, mudar a visibilidade de um campo de `private` para `public` ou até a ordem de compilação pode alterar o número — e diferentes versões de compilador ou de JVM podem calculá-lo de forma diferente. O resultado é que arquivos gravados ontem deixam de abrir hoje por uma mudança que não afetava dado nenhum.

A solução é declarar o campo explicitamente, sempre, em toda classe serializável:

```java
class Produto implements Serializable {
    private static final long serialVersionUID = 1L;

    String nome;
    double preco;
}
```

Com o valor fixado à mão, você passa a controlar quando a incompatibilidade é declarada. Enquanto as mudanças na classe forem compatíveis (ver a próxima seção), mantenha o número; só o incremente quando fizer uma mudança que deliberadamente deve invalidar os bytes antigos. A maioria das IDEs e o *linter* `serial` do compilador (`javac -Xlint:serial`) avisam quando o campo está faltando.

A analogia é o número de edição de um manual de peças. Enquanto a edição 1 do manual descreve a mesma máquina, qualquer oficina consegue usar as peças embaladas com "manual ed. 1". Se a máquina muda de verdade, publica-se a edição 2, e os kits antigos passam a ser recusados de propósito — não por acidente de impressão.

A alternativa de deixar o cálculo automático só é aceitável em objetos que nunca sobrevivem a um reinício: serialização usada como transporte efêmero entre dois processos da mesma versão exata, por exemplo. Para qualquer coisa que seja gravada em disco ou trocada entre sistemas que versionam de forma independente, o UID explícito é obrigatório.

### `transient`

`transient` é uma palavra-chave de modificação de campo. Marcar um atributo como `transient` diz à serialização nativa para **ignorá-lo**: seu valor não é escrito no stream e, na desserialização, o campo recebe o valor padrão do seu tipo — `null` para referências, `0` para numéricos, `false` para `boolean`.

Sem esse mecanismo, a serialização levaria todos os campos de instância, e isso causa três tipos de problema. Primeiro, *dados sensíveis*: uma senha, um token ou uma chave em memória seriam gravados em claro no arquivo ou na rede. Segundo, *campos não serializáveis*: se a classe guarda um `Thread`, um `Socket` ou um `Connection`, a serialização falharia com `NotSerializableException` ao chegar neles. Terceiro, *estado derivado*: um cache, um total pré-calculado ou um valor que só faz sentido no processo atual seriam persistidos à toa, podendo até voltar inconsistentes.

`transient` resolve excluindo o campo da gravação. Quando o campo precisa de um valor coerente depois da leitura — e não apenas o padrão —, você o recompõe num `readObject` privado, chamando antes `defaultReadObject()` para ler os campos normais e depois reconstruindo os `transient`.

```java
class Conexao implements Serializable {
    private static final long serialVersionUID = 1L;

    String host;
    int porta;
    transient Socket socket;        // não faz sentido serializar
    transient volatile int tentativas; // estado derivado, some na volta

    private void readObject(ObjectInputStream in) throws IOException, ClassNotFoundException {
        in.defaultReadObject();
        this.socket = null;        // será reaberto sob demanda
        this.tentativas = 0;
    }
}
```

A analogia é o formulário da alfândega ao despachar uma mala: você lista as roupas e os presentes, mas não declara — nem manda junto — o seu passaporte ou o carregador que só serve na tomada de casa. Alguns itens não atravessam a fronteira com a mala; do outro lado, você providencia equivalentes locais.

Alternativas para "não serializar isto": tornar o campo `static` (mas `static` muda a semântica — o valor passa a ser da classe, não da instância, o que raramente é o que você quer), ou assumir controle total com `Externalizable` e simplesmente não escrever aquele campo. `transient` é a ferramenta certa na esmagadora maioria dos casos; a única situação em que ele "atrapalha" é quando o campo realmente precisava ser persistido e foi marcado por engano — aí o dado volta zerado sem nenhum aviso.

### Compatibilidade de versões

Compatibilidade de versões, no contexto da serialização, é a pergunta: *bytes gravados pela versão antiga de uma classe ainda podem ser lidos pela versão nova* (e, idealmente, o contrário)? Como o formato nativo carrega a descrição dos campos junto dos dados, a plataforma consegue tolerar algumas diferenças entre a classe que gravou e a que lê — mas não todas.

Sem conhecer essas regras, qualquer refatoração de uma classe serializável vira um risco. Você renomeia um campo para deixar o código mais claro, sobe a nova versão, e de repente nenhum dos arquivos ou mensagens antigas abre, porque para a serialização um campo renomeado é um campo *removido* mais um campo *novo*.

As regras, definidas na especificação de serialização do Java, separam mudanças **compatíveis** de **incompatíveis**. São compatíveis, mantendo o `serialVersionUID`: adicionar campos (na leitura de bytes antigos eles ficam com o valor padrão); remover campos (os dados correspondentes no stream são ignorados); adicionar ou remover métodos e construtores; adicionar interfaces; mudar o acesso de um campo (`private`/`public` etc.); tornar um campo `static` ou `transient` — o que equivale a removê-lo do formato. São incompatíveis: mudar o *tipo* declarado de um campo; renomear um campo; mover um campo para uma superclasse ou subclasse; mudar a posição da classe na hierarquia; trocar um campo de instância não-`transient` por `static`/`transient` e esperar ler o valor antigo.

```java
// Versão 1 — bytes gravados assim
class Cliente implements Serializable {
    private static final long serialVersionUID = 1L;
    String nome;
    String telefone;
}

// Versão 2 — ainda lê os bytes da v1
class Cliente implements Serializable {
    private static final long serialVersionUID = 1L; // inalterado: mudança compatível
    String nome;
    String telefone;
    String email;          // novo: bytes antigos -> email == null
    transient String cacheBusca; // novo e transient: nunca entra no formato
}
```

A analogia é a de um formulário de papel que ganha uma linha nova: fichas antigas preenchidas sem aquela linha continuam válidas, e o campo extra fica simplesmente em branco. Mas se você trocar a coluna "telefone" (texto) por "telefone" (só dígitos, tipo numérico), as fichas antigas com hífens e parênteses deixam de ser processáveis — mudou o *tipo* do campo.

A alternativa a depender dessas regras é assumir controle explícito da evolução: um `serialPersistentFields`, um par `writeObject`/`readObject` que lê formatos antigos e novos, ou o padrão *serialization proxy*. E a alternativa mais robusta continua sendo trocar o formato nativo por um esquema versionado de propósito — Protobuf, Avro — quando os dados precisam durar muito tempo e a classe vai mudar bastante.

## Serialização avançada

As aulas anteriores usaram a serialização como a plataforma a entrega por padrão. Esta reúne os pontos de extensão que permitem assumir o controle — e os perigos que justificam esse controle existir.

São sete tópicos: a interface que transfere toda a responsabilidade do formato para você (`Externalizable`); os métodos privados especiais que ajustam a serialização e a desserialização sem abrir mão do mecanismo automático (`writeObject`, `readObject`); os *hooks* que trocam o objeto gravado ou o objeto reconstruído por outro (`writeReplace`, `readResolve`); o mecanismo que restringe quais classes um stream pode instanciar (Filtros de desserialização); e o panorama de por que desserializar dados não confiáveis é uma das falhas de segurança mais graves da plataforma (Riscos de segurança).

### `Externalizable`

`Externalizable` é uma sub-interface de `Serializable` que, em vez de apenas marcar a classe, exige que ela implemente dois métodos: `writeExternal(ObjectOutput out)` e `readExternal(ObjectInput in)`. Ao declará-la, você abre mão por completo da serialização automática de campos: **nada** é gravado ou lido a não ser o que esses dois métodos escreverem e lerem, explicitamente, byte a byte.

O mecanismo padrão, guiado por reflexão, tem custos: ele grava metadados de classe relativamente verbosos, percorre campos por reflexão (mais lento que acesso direto) e produz um formato que você não controla. Para classes serializadas em volume muito alto, ou quando o formato de bytes precisa seguir uma especificação externa exata, esse "piloto automático" atrapalha.

`Externalizable` resolve entregando o volante. Você decide a ordem dos campos, a codificação de cada um, se grava um número de versão próprio no início — tudo. Há duas diferenças de comportamento importantes em relação a `Serializable`: primeiro, a classe **precisa ter um construtor público sem argumentos**, porque a desserialização cria a instância chamando esse construtor e só depois invoca `readExternal`; segundo, os campos `transient` deixam de ter efeito especial — como você escreve tudo à mão, incluí-los ou não é decisão sua.

```java
class Ponto implements Externalizable {
    private int x, y;

    public Ponto() { }   // obrigatório: público e sem argumentos

    public Ponto(int x, int y) { this.x = x; this.y = y; }

    @Override public void writeExternal(ObjectOutput out) throws IOException {
        out.writeByte(1);   // versão do formato, controlada por você
        out.writeInt(x);
        out.writeInt(y);
    }

    @Override public void readExternal(ObjectInput in) throws IOException {
        int versao = in.readByte();
        this.x = in.readInt();
        this.y = in.readInt();
    }
}
```

A analogia: a serialização padrão é como mandar a mudança com uma transportadora que embala tudo do seu jeito; `Externalizable` é alugar o caminhão e empacotar você mesmo — mais trabalho e mais chance de errar, mas você sabe exatamente o que vai em cada caixa e cabe mais coisa.

As alternativas são o par `writeObject`/`readObject` (mantém a automação e ajusta só o necessário) e o padrão *serialization proxy* via `writeReplace`. Evite `Externalizable`: o esforço de manutenção é alto — todo campo novo tem que ser adicionado nos dois métodos, sob risco de silenciosamente sumir —, a herança complica (a subclasse precisa chamar os métodos da superclasse à mão) e o construtor público sem argumentos enfraquece as invariantes da classe. Use apenas quando um ganho medido de desempenho ou uma exigência de formato realmente justificar.

### `writeObject`

`writeObject` é um método **privado** com a assinatura exata `private void writeObject(ObjectOutputStream out) throws IOException`. Quando presente numa classe `Serializable`, a plataforma o detecta por reflexão e o chama no lugar da rotina padrão de gravação daquela classe — sem que ele apareça em nenhuma interface. É o ponto de extensão para ajustar a serialização mantendo a automação.

O problema que ele resolve aparece quando a gravação campo a campo padrão não basta: um campo `transient` que precisa ser persistido de forma especial (uma senha que você quer gravar cifrada, não em claro), uma estrutura de dados que fica mais compacta num formato próprio (o `HashMap`, por exemplo, grava suas entradas via `writeObject` em vez de despejar a tabela interna), ou a necessidade de escrever um marcador de versão antes dos dados.

Dentro de `writeObject`, você quase sempre começa chamando `out.defaultWriteObject()`, que grava normalmente todos os campos não-`transient`, e em seguida escreve à mão o que faltou:

```java
class Credencial implements Serializable {
    private static final long serialVersionUID = 1L;
    private String usuario;
    private transient char[] senha;   // não deve ir em claro

    private void writeObject(ObjectOutputStream out) throws IOException {
        out.defaultWriteObject();          // grava 'usuario'
        byte[] cifrada = Cripto.cifrar(senha);
        out.writeInt(cifrada.length);
        out.write(cifrada);               // grava a senha protegida
    }
}
```

A analogia é a de despachar uma encomenda: a transportadora embala o grosso do conteúdo de forma padrão (`defaultWriteObject`), e você, antes de fechar a caixa, coloca à mão o item frágil dentro de uma proteção especial que só você sabe montar. A caixa fecha com as duas coisas dentro.

A alternativa mais radical é `Externalizable`, que dispensa a automação inteira; a mais conservadora é não customizar nada e usar `transient` + reinicialização. Não implemente `writeObject` sem implementar o `readObject` correspondente lendo **na mesma ordem** — os dois formam um par, e um sem o outro produz streams que ninguém consegue reler. E lembre que `writeObject` é chamado por classe na hierarquia: ele cuida apenas dos campos declarados na própria classe, não dos herdados.

### `readObject`

`readObject` é o espelho de `writeObject`: um método **privado** com a assinatura `private void readObject(ObjectInputStream in) throws IOException, ClassNotFoundException`, chamado pela plataforma durante a desserialização daquela classe, no lugar da rotina padrão de leitura. Ele existe para desfazer, na leitura, exatamente o que `writeObject` fez na escrita — e para uma segunda tarefa igualmente importante: revalidar o objeto reconstruído.

O problema central já apareceu na aula *Serializable*: a desserialização não passa pelos seus construtores, então toda a validação que eles fariam — datas em ordem, coleções não nulas, valores dentro de faixas — é ignorada quando o objeto vem de bytes. Se esses bytes foram adulterados, você acaba com uma instância que o resto do código supõe válida e não é. Além disso, campos de referência mutáveis lidos do stream podem ser compartilhados com um atacante que mantém uma referência ao mesmo objeto.

`readObject` fecha essas brechas. O padrão é chamar `in.defaultReadObject()` para preencher os campos automáticos, reconstruir os campos especiais gravados à mão pelo `writeObject`, **revalidar as invariantes** lançando `InvalidObjectException` se algo estiver errado, e fazer **cópia defensiva** dos campos mutáveis.

```java
class Periodo implements Serializable {
    private static final long serialVersionUID = 1L;
    private final Date inicio;
    private final Date fim;

    private void readObject(ObjectInputStream in) throws IOException, ClassNotFoundException {
        in.defaultReadObject();
        // cópia defensiva: quebra qualquer referência externa aos mesmos Date
        // (feita por reflexão porque os campos são final)
        // ... setInicio(new Date(inicio.getTime())) via setAccessible ...
        if (inicio.compareTo(fim) > 0) {          // revalida invariante
            throw new InvalidObjectException("início posterior ao fim");
        }
    }
}
```

A analogia é a inspeção na chegada de uma carga internacional: a transportadora entregou a caixa (os bytes), mas antes de liberar para uso, um fiscal confere se o conteúdo bate com a nota, se nada foi trocado no caminho e se os itens frágeis chegaram inteiros. Sem essa inspeção, você assume que está tudo certo só porque a caixa chegou.

Para casos em que a validação depende de todo o grafo já estar montado, existe `ObjectInputValidation` com `registerValidation`, que roda ao fim da desserialização inteira. A alternativa estrutural é o *serialization proxy pattern*, que evita a maior parte dessas armadilhas de uma vez. Não escreva um `readObject` que apenas chama `defaultReadObject()` e nada mais — nesse caso ele é inútil e melhor removido.

### `writeReplace`

`writeReplace` é um método com a assinatura `Object writeReplace() throws ObjectStreamException` (qualquer modificador de acesso serve) que, se presente, é chamado **antes** da serialização: o objeto que ele retorna é serializado no lugar do objeto original. Ou seja, a classe pode dizer "não me grave; grave este outro objeto no meu lugar".

O uso mais valioso é o *serialization proxy pattern*. Serializar diretamente uma classe com invariantes delicadas, campos `final`, cópias defensivas e um grafo interno complexo é arriscado — cada um dos *hooks* anteriores existe para tapar um buraco desse processo. O padrão inverte o problema: em vez de blindar a serialização da classe real, você serializa uma classe auxiliar simples, um "proxy", que carrega só o estado lógico mínimo.

```java
class Periodo implements Serializable {
    private final Date inicio, fim;

    private Object writeReplace() {
        return new Proxy(this);      // grava o Proxy, nunca o Periodo
    }

    private void readObject(ObjectInputStream in) throws InvalidObjectException {
        throw new InvalidObjectException("use o proxy"); // barra ataques diretos
    }

    private static class Proxy implements Serializable {
        private static final long serialVersionUID = 1L;
        private final long inicio, fim;
        Proxy(Periodo p) { this.inicio = p.inicio.getTime(); this.fim = p.fim.getTime(); }

        private Object readResolve() {
            return new Periodo(new Date(inicio), new Date(fim)); // passa pelo construtor real
        }
    }
}
```

A analogia é a de enviar uma maquete desmontável em vez da escultura de mármore original: a maquete (o proxy) leva as medidas e as instruções essenciais, viaja sem risco de lascar, e no destino um artesão reproduz a escultura usando as ferramentas normais do ateliê (o construtor).

`writeReplace` também aparece em usos mais simples: uma classe pode substituir uma instância por uma forma canônica antes de gravar. As alternativas são os *hooks* individuais (`writeObject`/`readObject` + `transient` + validação), que dão o mesmo resultado com mais peças móveis. Não use `writeReplace` quando a classe é `final`, sem herança, sem campos mutáveis e sem invariantes — aí o padrão proxy é cerimônia sem ganho. E cuidado com herança: se uma superclasse define `writeReplace`, as subclasses o herdam e podem acabar sendo substituídas pelo proxy da superclasse sem querer.

### `readResolve`

`readResolve` é o par de `writeReplace` no lado da leitura: um método `Object readResolve() throws ObjectStreamException` (qualquer acesso) chamado **depois** que o objeto foi desserializado, mas antes de ser devolvido a quem chamou `readObject`. O objeto que `readResolve` retorna substitui o objeto recém-criado como resultado final.

O problema clássico que ele resolve é o de manter a *unicidade* de instâncias que deveriam ser únicas. Um *singleton* implementado como um campo `static final` deixa de ser único assim que a classe é serializável: cada `readObject` cria uma instância nova, alocada sem passar pelo construtor privado, e de repente existem vários "singletons". O mesmo vale para constantes tipadas feitas à mão (o padrão *typesafe enum* pré-`enum`) e para caches de instâncias imutáveis.

`readResolve` corrige devolvendo sempre a instância canônica:

```java
class Configuracao implements Serializable {
    private static final long serialVersionUID = 1L;
    static final Configuracao INSTANCIA = new Configuracao();

    private Configuracao() { }

    private Object readResolve() {
        return INSTANCIA;   // descarta o objeto recém-desserializado
    }
}
```

Para que isso seja realmente à prova de adulteração, todos os campos de referência de um *singleton* que usa `readResolve` devem ser `transient` — senão um atacante pode "roubar" a instância desserializada intermediária antes de `readResolve` trocá-la.

A analogia é a portaria de um clube que só tem um cartão de sócio-fundador verdadeiro: se alguém chega com uma cópia impressa em casa (a instância desserializada), a portaria confere no cadastro e devolve o cartão oficial guardado no cofre, ignorando a cópia. O portador nunca sai com a cópia na mão.

A alternativa moderna, e quase sempre superior, é declarar o tipo como `enum`: a plataforma garante que constantes de `enum` sejam *singletons* mesmo através de serialização, sem uma linha de `readResolve` e sem a brecha dos campos não-`transient`. Um `enum` de um único valor é o *singleton* recomendado em Java atual. Reserve `readResolve` para quando `enum` não serve — por exemplo, quando a instância precisa estender uma classe concreta, ou quando você está implementando o *serialization proxy pattern* e o `readResolve` do proxy reconstrói o objeto real.

### Filtros de desserialização

Um filtro de desserialização é um objeto `java.io.ObjectInputFilter` que o `ObjectInputStream` consulta **antes de instanciar cada classe** encontrada no stream. Para cada tipo prestes a ser criado, o filtro responde `ALLOWED`, `REJECTED` ou `UNDECIDED`; um `REJECTED` aborta a leitura com `InvalidClassException`. O mecanismo foi introduzido no JDK 9 (JEP 290) e refinado depois com filtros de fábrica no JDK 17.

O problema que motivou os filtros: `readObject`, ao processar um stream, pode instanciar qualquer classe do *classpath* que os bytes mencionem, executando o `readObject` dela no caminho. Um atacante que controla os bytes encadeia essas classes até produzir um efeito perigoso — a próxima seção, *Riscos de segurança*, detalha como essa cadeia chega a executar comandos do sistema. Sem um filtro, a única defesa era não desserializar dados não confiáveis; o filtro adiciona uma barreira quando isso não é totalmente evitável.

O filtro é configurável em três níveis. Por *stream*, chamando `setObjectInputFilter` no `ObjectInputStream`. Por aplicação, via a propriedade `jdk.serialFilter`. E por padrão de fábrica, para novos streams. A sintaxe baseada em *string* aceita curingas de pacote e limites numéricos:

```java
var filtro = ObjectInputFilter.Config.createFilter(
    "com.minhaapp.dto.*;java.util.*;java.lang.*;" +  // pacotes permitidos
    "!*;" +                                          // tudo mais: rejeitado
    "maxdepth=10;maxrefs=500;maxbytes=100000");      // limites contra DoS

var ois = new ObjectInputStream(in);
ois.setObjectInputFilter(filtro);
Pedido p = (Pedido) ois.readObject();
```

A analogia é a lista de convidados na entrada de um evento: cada pessoa que chega tem o nome conferido contra a lista; quem está nela entra, quem não está é barrado, e há um limite de lotação (os `max...`) para o salão não estourar. Sem a lista, qualquer um entra — inclusive quem veio para causar problema.

Trocar o formato na fronteira — sair da serialização nativa — é uma defesa mais forte do que filtrar, e a próxima seção a coloca no topo da lista. Os filtros são para quando você **precisa** manter a serialização nativa: protocolos legados, RMI, bibliotecas que a exigem. Nesse caso, um filtro só com *blocklist* (proibir classes conhecidamente perigosas) envelhece mal — prefira sempre a *allowlist*: liste o que pode e rejeite o resto com `!*`.

### Riscos de segurança

Desserializar dados vindos de uma fonte não confiável é, historicamente, uma das falhas de segurança mais graves da plataforma Java — a ponto de a documentação oficial afirmar que a desserialização de dados não confiáveis é "inerentemente perigosa e deve ser evitada". O risco não é teórico: várias das invasões de maior repercussão em servidores Java exploraram exatamente esse ponto.

A raiz do problema já apareceu nas seções anteriores: `readObject` não é uma leitura passiva de dados. Ao reconstruir o grafo, ela **instancia classes** que os bytes indicam e **executa código** dessas classes — os `readObject`, `readResolve` e `finalize` delas. Um atacante que controla os bytes não precisa da sua classe: ele monta o que se chama de *gadget chain*, uma sequência de objetos de classes que já estão no seu *classpath* (de bibliotecas comuns como Commons Collections, Spring, Groovy) cujos métodos de desserialização, chamados em cascata, terminam executando um comando do sistema operacional. O resultado é execução remota de código (RCE) a partir de um único `readObject` sobre bytes hostis.

Há ainda ataques mais simples de negação de serviço (DoS): um stream pequeno pode descrever um grafo com bilhões de objetos aninhados (`HashSet` dentro de `HashSet`...) que estoura memória ou trava a CPU calculando *hashcodes* durante a montagem — o análogo do "billion laughs" de XML.

```java
// PADRÃO PERIGOSO — nunca faça isto com dados de rede/usuário:
ServerSocket servidor = new ServerSocket(9000);
Socket cliente = servidor.accept();
var ois = new ObjectInputStream(cliente.getInputStream());
Object comando = ois.readObject();   // bytes do cliente -> possível RCE
```

As defesas, em ordem de eficácia: **não desserializar dados não confiáveis** — troque o formato na fronteira por JSON, Protobuf ou similar, que carregam dados e não instruções de instanciação; se a serialização nativa for inevitável, aplique um `ObjectInputFilter` com *allowlist* estrita e limites de profundidade, referências e bytes; mantenha as bibliotecas atualizadas, já que muitas *gadget chains* dependem de versões vulneráveis; e reduza o *classpath* de processos que desserializam, para diminuir o material disponível para as cadeias.

A analogia: aceitar um stream de serialização de fonte desconhecida e chamar `readObject` é como receber um pacote de um remetente anônimo cujas instruções de "montagem" o seu próprio pessoal vai executar sem ler antes — se as instruções mandarem abrir a porta dos fundos e deixar entrar, é o que acontece. Um formato de dados puro é como receber uma carta: você lê o conteúdo, mas ele não comanda ninguém.

Quando o dado é comprovadamente confiável — gravado pelo seu próprio processo, num arquivo sob seu controle, sem exposição a terceiros — a serialização nativa é aceitável; ainda assim, para dados que precisam durar, um formato versionado costuma valer mais do que a conveniência.

## Cópia de objetos

Precisar de "outro objeto igual a este, mas independente" é rotina: para não deixar o chamador alterar o seu estado interno, para guardar um instantâneo antes de uma operação arriscada, para variar uma configuração sem tocar na original. Java não tem uma resposta única e boa para isso — tem várias, com armadilhas.

Esta aula percorre as duas profundidades de cópia — a que só duplica o objeto de cima (Shallow copy) e a que duplica o grafo inteiro (Deep copy) — e depois os mecanismos concretos: a interface e o método herdados de `Object` (`Cloneable`, `clone`), os construtores que recebem um objeto do próprio tipo (Copy constructors), os métodos estáticos que fazem o mesmo com outro nome e mais liberdade (Factory methods), e o fechamento do módulo: por que, na prática, o caminho de `Cloneable` costuma ser o pior dos disponíveis (Por que frequentemente evitar `Cloneable`).

### Shallow copy

Uma cópia rasa (*shallow copy*) é uma nova instância cujos campos recebem **os mesmos valores** dos campos do original — valor por valor, exatamente como estão. Para campos primitivos, isso copia o número. Para campos de referência, isso copia a *referência*: o novo objeto e o antigo passam a apontar para os **mesmos** objetos aninhados. Duplica-se a "casca" de cima; o conteúdo referenciado continua compartilhado.

O problema aparece assim que um desses objetos compartilhados é mutável. Você faz uma cópia rasa de um `Pedido` para guardar um histórico, alguém adiciona um item à lista de itens do pedido atual, e o "histórico" muda junto — porque as duas cópias apontam para a **mesma** `List`. A independência que motivou a cópia não existe para nada além dos campos primitivos e das referências trocadas por completo.

```java
class Pedido {
    String codigo;
    List<String> itens;

    Pedido shallowCopy() {
        Pedido c = new Pedido();
        c.codigo = this.codigo;      // String é imutável: seguro
        c.itens = this.itens;        // MESMA lista dos dois lados
        return c;
    }
}

Pedido a = /* ... itens = ["café"] */;
Pedido b = a.shallowCopy();
b.itens.add("chá");
System.out.println(a.itens);   // [café, chá] — vazou para o original
```

A analogia é fotocopiar um documento que tem, grampeada, uma pasta com anexos: a folha de rosto vira duas, mas o grampo da cópia aponta para a **mesma** pasta de anexos. Quem mexer nos anexos por um lado mexe pelos dois.

A cópia rasa é a alternativa certa — e mais barata — quando todos os campos de referência são imutáveis (`String`, `Integer`, `LocalDate`, tipos `record` de campos imutáveis) ou quando o compartilhamento é intencional. `Object.clone()`, `Arrays.copyOf`, o construtor `new ArrayList<>(outraLista)` e `record`s copiados por *wither* fazem cópia rasa. Não a use quando o objeto tem campos mutáveis que o chamador não deve enxergar nem alterar — aí é *deep copy* ou cópia defensiva campo a campo.

### Deep copy

Uma cópia profunda (*deep copy*) duplica não só o objeto de cima, mas **recursivamente** todos os objetos mutáveis que ele alcança, de modo que o resultado seja completamente independente do original: nenhuma alteração em um lado, por mais fundo que esteja no grafo, é visível no outro. Onde a cópia rasa compartilha as referências aninhadas, a profunda cria um objeto novo para cada uma.

O problema que ela resolve é o vazamento de estado da cópia rasa, mas o custo é maior e as decisões são mais sutis. Até onde ir? Se um `Pedido` referencia um `Cliente`, a cópia profunda do pedido deve clonar o cliente também? Normalmente não — o cliente é uma entidade compartilhada de propósito. "Profundo" quase nunca significa "clonar o universo alcançável"; significa "clonar tudo que, se compartilhado, quebraria a independência que eu preciso". E há o risco dos ciclos: seguir referências sem controle numa estrutura com ciclos entra em recursão infinita.

Há várias técnicas. A cópia profunda **manual**, campo a campo, é a mais controlada: rápida, explícita, sem dependências, mas precisa ser atualizada quando a classe ganha campos. O **round-trip de serialização** — serializar o objeto e desserializar de volta — produz uma cópia profunda de graça, tratando grafos e ciclos automaticamente, ao custo de exigir tudo `Serializable`, ser lento e carregar os riscos do módulo. Bibliotecas de *deep clone* fazem o mesmo por reflexão.

```java
class Pedido {
    String codigo;
    List<String> itens;
    Endereco entrega;        // Endereco é mutável

    Pedido deepCopy() {
        Pedido c = new Pedido();
        c.codigo = this.codigo;                       // imutável: copia direto
        c.itens = new ArrayList<>(this.itens);        // nova lista (elementos String, imutáveis)
        c.entrega = new Endereco(this.entrega);       // novo Endereco via copy constructor
        return c;
    }
}
```

A analogia retoma o documento com a pasta grampeada: a cópia profunda fotocopia a folha de rosto **e** cada anexo, e grampeia a pilha nova de anexos na cópia. Agora são dois conjuntos completos; riscar um anexo de um não altera o outro. Mas você fotocopia só os anexos relevantes — o catálogo telefônico citado numa nota de rodapé não precisa ser reproduzido.

A alternativa a escrever cópia profunda é projetar para *imutabilidade*: se os objetos aninhados não podem mudar, a cópia rasa já é segura e a profunda perde o motivo de existir. Não faça cópia profunda por reflexo — ela é desperdício quando o grafo é raso ou imutável, e o *round-trip* de serialização em particular não deve ser usado em código quente nem sobre classes que você não quer tornar serializáveis.

### `Cloneable`

`Cloneable` é uma interface de marcação — sem métodos — do pacote `java.lang`. Seu papel é estranho: ela não declara `clone()`, e no entanto é ela que **habilita** a cópia. O método `clone()` mora em `Object`, é `protected`, e sua implementação nativa verifica se o objeto implementa `Cloneable`; se **não** implementa, lança `CloneNotSupportedException`. Então `Cloneable` funciona como um interruptor que muda o comportamento de um método herdado de outra classe — um design que a própria documentação da linguagem reconhece como atípico.

Sem `Cloneable`, chamar `super.clone()` (a forma correta de implementar cópia via esse mecanismo, porque só a versão nativa de `Object` cria uma instância do tipo certo sem invocar construtor) resulta em exceção. Com ela, `super.clone()` devolve uma cópia rasa, campo a campo, da instância.

```java
class Ponto implements Cloneable {
    int x, y;

    @Override
    public Ponto clone() {
        try {
            return (Ponto) super.clone();   // cópia rasa dos campos
        } catch (CloneNotSupportedException e) {
            throw new AssertionError(e);     // impossível: implementamos Cloneable
        }
    }
}
```

O contrato esperado de `x.clone()` é: `x.clone() != x`, `x.clone().getClass() == x.getClass()` e `x.clone().equals(x)` — mas nada disso é *garantido* pela linguagem; é convenção que cada implementação precisa honrar por conta própria.

A analogia é a de uma fechadura cujo mecanismo já está montado na porta (o `clone()` de `Object`), mas que só destranca se houver um adesivo específico colado na porta (`Cloneable`). O adesivo não tem engrenagem nenhuma — ele só sinaliza para o mecanismo, que existe independentemente dele, que pode operar. Quem vê o adesivo não sabe, olhando, o que a fechadura faz.

As alternativas — *copy constructor* e *factory method* — resolvem o mesmo problema sem esse acoplamento indireto. Implemente `Cloneable` apenas quando precisar interoperar com código que espera receber objetos "clonáveis", ou ao copiar arrays, onde `array.clone()` é idiomático e eficiente. Para classes novas de domínio, a próxima seção e o fechamento do módulo explicam por que quase sempre há escolha melhor.

### `clone`

`clone()` é o método, herdado de `Object`, que produz a cópia quando o mecanismo `Cloneable` está em jogo. Para usá-lo bem, uma classe precisa: implementar `Cloneable`; sobrescrever `clone()` tornando-o `public` (em `Object` ele é `protected`) e, por convenção moderna, estreitar o tipo de retorno para o da própria classe; chamar `super.clone()` para obter a instância; e então **corrigir à mão** os campos que a cópia rasa de `super.clone()` deixou compartilhados.

Esse último passo é o que torna `clone()` traiçoeiro. `super.clone()` copia os campos exatamente como a cópia rasa: se a classe tem uma `List`, um array ou qualquer objeto mutável, a cópia e o original apontam para o mesmo. Você precisa, dentro de `clone()`, substituir cada um desses campos por uma cópia própria.

```java
class Turma implements Cloneable {
    String nome;
    List<String> alunos;
    int[] notas;

    @Override
    public Turma clone() {
        try {
            Turma c = (Turma) super.clone();          // rasa: alunos e notas ainda compartilhados
            c.alunos = new ArrayList<>(this.alunos);  // conserta a lista
            c.notas = this.notas.clone();             // conserta o array
            return c;
        } catch (CloneNotSupportedException e) {
            throw new AssertionError(e);
        }
    }
}
```

Um agravante: o mecanismo **não funciona bem com campos `final` mutáveis**. Você não pode reatribuir `c.alunos` se `alunos` for `final`, então classes que querem ser clonáveis acabam abrindo mão do `final` em campos que deveriam tê-lo — perdendo garantias de imutabilidade só para acomodar `clone()`. Além disso, `clone()` **não chama construtor nenhum**, então qualquer inicialização ou validação que os construtores fariam é pulada, à semelhança da desserialização.

A analogia: `super.clone()` é uma impressora 3D que reproduz a peça externa perfeitamente, mas todos os fios internos da cópia ainda estão soldados na placa da peça original. Antes de entregar, você tem que ir lá e refazer cada solda para a placa nova — e se esquecer um fio, os dois aparelhos compartilham um circuito sem que ninguém perceba até dar defeito.

As alternativas são, de novo, *copy constructor* e *factory method*, que criam a cópia por um caminho normal e explícito. `array.clone()` é a exceção em que `clone()` é a melhor opção: conciso, rápido e sem os problemas de herança. Para tudo mais, a seção final do módulo detalha por que evitar.

### Copy constructors

Um *copy constructor* é um construtor que recebe como argumento outra instância do mesmo tipo e inicializa o novo objeto a partir dela: `public Pedido(Pedido outro)`. É a forma que C++ consagrou e que Java suporta sem nenhum mecanismo especial — é só um construtor comum cujo parâmetro é do próprio tipo da classe.

Ele resolve os problemas de `clone()` justamente por **não** ser mágico. A criação passa pelo construtor, então toda a inicialização e validação normais acontecem. Campos `final` podem ser atribuídos, porque a atribuição ocorre na construção — a classe não precisa relaxar a imutabilidade para ser copiável. Você decide, campo a campo e de forma explícita no código, o que é cópia rasa e o que é profunda, sem depender de sobrescrever nada certo.

```java
class Pedido {
    private final String codigo;
    private final List<String> itens;
    private final Endereco entrega;

    Pedido(String codigo, List<String> itens, Endereco entrega) {
        this.codigo = Objects.requireNonNull(codigo);
        this.itens = new ArrayList<>(itens);
        this.entrega = entrega;
    }

    // copy constructor
    Pedido(Pedido outro) {
        this.codigo = outro.codigo;                    // String imutável
        this.itens = new ArrayList<>(outro.itens);     // lista nova
        this.entrega = new Endereco(outro.entrega);    // Endereco novo (profundo)
    }
}
```

Uma vantagem extra: a mesma ideia estende-se a *conversion constructors*, que recebem um tipo relacionado em vez do próprio — `new ArrayList<>(umaColeçãoQualquer)`, `new HashSet<>(umaLista)`. Toda a *framework* de coleções do Java oferece esse construtor, e é por isso que copiar uma coleção quase nunca precisa de `clone()`.

A analogia é a de um marceneiro que constrói um armário novo usando outro como referência de medidas e acabamento: ele passa pela bancada, pelas ferramentas e pela inspeção de qualidade de sempre (o construtor), e no caminho decide o que replicar fielmente e o que fazer diferente. Não é um decalque cego da peça original.

A limitação: um *copy constructor* copia para o **tipo estático** que você escreveu. Se você tem uma referência `Forma` que na verdade aponta para um `Circulo`, `new Forma(aquilo)` não existe, e mesmo que existisse não devolveria um `Circulo`. Quando a cópia precisa preservar o subtipo real de um objeto acessado por uma superclasse ou interface, é `clone()` (com suas ressalvas) ou um *factory method* polimórfico que resolve — o tema da próxima seção.

### Factory methods

Um *factory method* de cópia é um método **estático** que recebe uma instância e devolve uma cópia dela: `public static Ponto copyOf(Ponto original)`. Faz o mesmo trabalho que um *copy constructor* — cria um objeto novo a partir de um existente — mas por ser um método nomeado, e não um construtor, ganha liberdades que o construtor não tem.

A primeira liberdade é o **nome**. `Ponto.copyOf(p)`, `List.copyOf(lista)`, `Endereco.snapshot(e)` dizem o que fazem; uma classe com vários construtores de um parâmetro só se distingue pelos tipos, e dois construtores que recebem o mesmo tipo com semânticas diferentes são impossíveis. A segunda é que o *factory method* **não precisa criar um objeto novo a cada chamada**: pode devolver o próprio argumento quando ele já é imutável (é o que `List.copyOf` faz se a lista de entrada já é imutável), pode devolver uma instância em cache, pode aplicar *pooling*. A terceira é o **polimorfismo de retorno**: o método pode inspecionar o argumento e devolver a subclasse apropriada.

```java
class Forma {
    static Forma copyOf(Forma f) {
        return switch (f) {
            case Circulo c -> new Circulo(c.raio);
            case Retangulo r -> new Retangulo(r.largura, r.altura);
            default -> throw new IllegalArgumentException("tipo desconhecido: " + f);
        };
    }
}

List<Integer> original = new ArrayList<>(List.of(1, 2, 3));
List<Integer> copia = List.copyOf(original);   // cópia imutável e independente
```

A analogia é a de um balcão de atendimento em vez de uma porta única: você diz ao atendente "quero uma cópia disto", e ele decide nos bastidores se imprime uma nova, se pega uma pronta da gaveta ou se, como o documento já é laminado e não muda, simplesmente devolve o seu. A porta única (o construtor) sempre te obriga a fabricar do zero.

As alternativas são o *copy constructor* (mais convencional, esperado por algumas *frameworks*, direto ao ponto quando não há cache nem polimorfismo em jogo) e, evitando, `clone()`. A desvantagem do *factory method* é ser menos "descoberto" por convenção — o leitor precisa saber que `copyOf` existe — e não ser reconhecido automaticamente por ferramentas que instanciam via construtor. Ainda assim, para APIs públicas e classes imutáveis, o *factory method* de cópia é hoje a recomendação predominante em Java.

### Por que frequentemente evitar `Cloneable`

Reunindo o que as seções anteriores mostraram: o mecanismo `Cloneable`/`clone()` acumula tantos defeitos de projeto que, para classes novas, ele quase nunca é a resposta certa. A recomendação de referência — de *Effective Java*, de Joshua Bloch — é direta: não implemente `Cloneable`; ofereça um *copy constructor* ou um *factory method* de cópia no lugar.

O primeiro defeito é a **interface que não declara o método que promete**. `Cloneable` não tem `clone()`; ela só liga, por um efeito colateral, o `clone()` `protected` de `Object`. Uma interface que modifica o comportamento de um método `protected` de uma superclasse é um uso da linguagem que não se repete em nenhum outro lugar, e que confunde: declarar `implements Cloneable` não dá a ninguém, de fora, a capacidade de chamar `clone()`, porque o método continua `protected` a menos que você o sobrescreva e promova a `public`.

O segundo é a **exceção verificada sem propósito**. `Object.clone()` declara `throws CloneNotSupportedException`, então todo `clone()` que chama `super.clone()` precisa de um `try/catch` para uma exceção que, numa classe que implementa `Cloneable`, jamais ocorre. É *boilerplate* puro em cada implementação.

O terceiro, e mais grave, é o **conflito com campos `final`**. Como `clone()` produz a instância via `super.clone()` e só depois conserta os campos mutáveis por reatribuição, qualquer campo que precise de conserto não pode ser `final`. Classes que adotam `Cloneable` frequentemente removem o `final` de campos que deveriam tê-lo, enfraquecendo suas garantias de imutabilidade apenas para acomodar o mecanismo.

O quarto é a **fragilidade na herança**. Se você escreve uma classe não-`final` e a torna `Cloneable`, obriga toda subclasse a implementar `clone()` corretamente — cada uma precisa lembrar de chamar `super.clone()` e de copiar em profundidade os seus próprios campos mutáveis. Esquecer um campo em qualquer nível da hierarquia produz uma cópia parcialmente compartilhada que só se manifesta como bug muito depois. E `clone()`, como não chama construtor, pula qualquer validação que os construtores fariam.

```java
// O caminho recomendado: sem Cloneable, sem clone(), sem try/catch inútil
final class Turma {
    private final String nome;
    private final List<String> alunos;

    Turma(String nome, List<String> alunos) {
        this.nome = Objects.requireNonNull(nome);
        this.alunos = List.copyOf(alunos);       // imutável e independente
    }

    Turma(Turma outra) {                          // copy constructor
        this(outra.nome, outra.alunos);
    }

    static Turma copyOf(Turma t) {                // factory method
        return new Turma(t);
    }
}
```

A analogia final: `Cloneable` é uma ferramenta que só funciona se você segurar de um jeito específico, vem com um manual cheio de "cuidado para não...", e ainda transfere a mesma lista de cuidados para quem herdar seu trabalho. Um *copy constructor* é uma ferramenta comum, que se usa como todas as outras da bancada, e cujo funcionamento qualquer um entende de imediato.

Há exceções legítimas, já citadas: `array.clone()` é a maneira idiomática e eficiente de copiar arrays e não tem os problemas de herança; e você às vezes precisa **implementar** `clone()` para satisfazer uma API antiga que exige objetos `Cloneable`. Fora esses casos, quando quiser dar a uma classe a capacidade de ser copiada, escreva um construtor de cópia ou um método de fábrica — mais simples de escrever, de ler, de manter e de herdar, e sem nenhuma das armadilhas deste módulo.
