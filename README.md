# Abstract Factory

## Intenção
Permite a criação de famílias de objetos relacionados ou dependentes sem especificar suas classes concretas.

## Também conhecido como
Kit de fábrica
Fábrica de fábricas

## Motivação
O código a seguir representa um problema clássico de alto acoplamento e dificuldade de manutenção. 

@import "devicesExample/badCode/src/service/DeviceFactory.ts"


O uso de estruturas como if ou switch para determinar o tipo de dispositivo e suas variantes gera as seguintes limitações:
1. **Complexidade do Cliente**: A lógica para determinar o tipo de dispositivo está embutida na classe DeviceFactory, tornando-a mais difícil de manter e testar.
2. **Dificuldade para Adicionar Novos Produtos**: Sempre que um novo tipo de dispositivo (ou variante) é introduzido, é necessário modificar o método createDevice, violando o princípio aberto/fechado (Open/Closed Principle).

   
`💡 Um design mais modular e flexível pode ser alcançado encapsulando a criação dos dispositivos em fábricas específicas e criando assim um nível de abstração, eliminando a necessidade de lógica condicional dentro do cliente.`


## Aplicabilidade
Use o padrão Abstract Factory quando:
- Um sistema precisa ser independente, gerando uma solução desacoplada para criar produtos relacionados.
- Um sistema precisa ser configurado com uma dentre várias famílias de produtos.
- Desejar garantir que objetos de uma mesma família sejam usados em conjunto.
- Desejar fornecer uma biblioteca de classes de produtos sem alterar o código do cliente e sem expor suas interfaces e implementação.


## Estrutura

```plantuml

    class WidgetFactory {
        +CreateScrollBar()
        +CreateWindow()
    }
    class MotifWidgetFactory extends WidgetFactory  {
        +CreateScrollBar()
        +CreateWindow()
    }
    class PMWidgetFactory  extends WidgetFactory{
        +CreateScrollBar()
        +CreateWindow()
    }
    class Client {
        +operation()
    }
    class ScrollBar
    class MotifScrollBar extends ScrollBar
    class PMScrollBar extends ScrollBar
    class Window
    class MotifWindow extends Window
    class PMWindow  extends Window

    Client --> Window
    Client --> ScrollBar
    MotifWidgetFactory --> MotifScrollBar
    MotifWidgetFactory --> MotifWindow
    PMWidgetFactory --> PMScrollBar
    PMWidgetFactory --> PMWindow
```

## Participantes

### WidgetFactory (Fábrica Abstrata)
- Define uma interface para criar famílias de objetos relacionados, como `CreateScrollBar()` e `CreateWindow()`.

### MotifWidgetFactory e PMWidgetFactory (Fábricas Concretas)
- Implementam a interface `WidgetFactory` para criar produtos específicos.
  - **MotifWidgetFactory**: Cria instâncias de `MotifScrollBar` e `MotifWindow`.
  - **PMWidgetFactory**: Cria instâncias de `PMScrollBar` e `PMWindow`.

### ScrollBar e Window (Produtos Abstratos)
- Representam interfaces ou classes abstratas para os tipos de produtos que serão criados.
  - **ScrollBar**: Interface para barras de rolagem.
  - **Window**: Interface para janelas.

### MotifScrollBar, PMScrollBar, MotifWindow e PMWindow (Produtos Concretos)
- Implementam os produtos abstratos definidos por `ScrollBar` e `Window`.
  - **MotifScrollBar** e **PMScrollBar**: Implementações concretas do produto `ScrollBar`.
  - **MotifWindow** e **PMWindow**: Implementações concretas do produto `Window`.

### Client
- Utiliza apenas as interfaces fornecidas por `WidgetFactory`, `ScrollBar` e `Window` para criar e usar os objetos. 


## Outro exemplo

```mermaid
---
title: Fábrica de Marcas
---
classDiagram
    class Actor

    class IDeviceFactory {
        +createPhones()
        +createWatch()
    }

    class AndroidFactory {
        +createPhones() AndroidPhone
        +createWatch() AndroidWatch
    }

    class AppleFactory {
        +createPhones() IPhone
        +createWatch() IWatch
    }

    class Phone {
        +getDetails() string
    }

    class Watch {
        +getDetails() string
    }

    class AndroidPhone {
        +getDetails() string
    }

    class IPhone {
        +getDetails() string
    }

    class AndroidWatch {
        +getDetails() string
    }

    class IWatch {
        +getDetails() string
    }

    %% Relações
    Actor --> IDeviceFactory

    IDeviceFactory <|-- AndroidFactory
    IDeviceFactory <|-- AppleFactory

    AndroidFactory --> AndroidPhone
    AndroidFactory --> AndroidWatch

    AppleFactory --> IPhone
    AppleFactory --> IWatch

    AndroidPhone --|> Phone
    IPhone --|> Phone

    AndroidWatch --|> Watch
    IWatch --|> Watch

# Singleton e Multiton - Padrões de Projeto

## Intenção

A intenção do **Singleton** é garantir que uma classe tenha uma única instância e fornecer um ponto global de acesso a ela. O **Multiton**, por outro lado, é utilizado para garantir que uma classe tenha uma única instância para cada chave específica, criando um conjunto de instâncias, mas ainda assim controlando seu número.

## Também Conhecido Como

- **Singleton**: Padrão de design de instância única.
- **Multiton**: Padrão de design de instâncias múltiplas controladas.

## Motivação

### Problema Resolvido pelo Singleton

Em alguns cenários, é necessário ter uma única instância de uma classe em todo o sistema. Um exemplo disso é o gerenciamento de conexões com banco de dados ou configuração de log, onde ter várias instâncias pode gerar problemas, como a perda de dados ou uso excessivo de recursos. O **Singleton** resolve isso ao garantir que só exista uma instância e forneça acesso a ela de qualquer parte do sistema.

### Exemplo de Aplicação do Singleton:

- **Problema**: O sistema de logs precisa de uma instância única de uma classe `Logger` para registrar todas as mensagens de log. Ter várias instâncias de `Logger` pode gerar conflitos de acesso e performance.
- **Solução com Singleton**: O padrão Singleton garante que apenas uma instância do `Logger` exista, e ela pode ser acessada de qualquer parte do código.

### Problema Resolvido pelo Multiton

Quando se precisa de várias instâncias de uma classe, mas cada uma com um contexto único (por exemplo, um banco de dados diferente ou configuração de log por módulo), o **Multiton** é necessário. Diferente do Singleton, o Multiton cria instâncias separadas para diferentes chaves, garantindo que não se criem instâncias desnecessárias ou repetidas.

### Exemplo de Aplicação do Multiton:

- **Problema**: Um sistema de configuração com várias instâncias para diferentes módulos (ex.: módulo de pagamento, módulo de relatórios) precisa de um objeto único para cada módulo, mas cada um deve ser independente.
- **Solução com Multiton**: O Multiton cria uma instância única para cada módulo, controlando a criação de instâncias de forma eficiente.

## Aplicabilidade

- **Use Singleton** quando você precisa garantir que uma classe tenha apenas uma instância em todo o sistema, como para configurações globais ou para recursos compartilhados como conexões de banco de dados.
- **Use Multiton** quando você precisar de uma instância única para cada chave ou contexto, mas ainda assim com um número controlado de instâncias.

## Estrutura

### Diagrama UML do Singleton:

```mermaid
---
title: Singleton Logger Exemplo
---
classDiagram
    class Logger {
        - static Logger instanciaUnica
        - String nomeArquivo
        - Logger(String nomeArquivo)
        + static Logger getInstance()
        + void log(String mensagem)
        + static void main(String[] args)
    }

    Logger o-- "1" logger : instanciaUnica
```

### Diagrama UML do Multiton:

```mermaid
---
title: Multiton Configuração Exemplo
---
classDiagram
    class ConfiguracaoComMultiton {
        - Map<String, ConfiguracaoComMultiton> INSTANCIAS
        - Map<String, String> configuracoes
        - ConfiguracaoComMultiton()
        + static ConfiguracaoComMultiton getInstance(String modulo)
        + void adicionarConfiguracao(String chave, String valor)
        + String obterConfiguracao(String chave)
        + static Integer getSizeInstancias()
        + static void main(String[] args)
    }

    ConfiguracaoComMultiton o-- "1..*" configuracaoComMultiton : INSTANCIAS

    ConfiguracaoComMultiton ..> "1..*" Map : configuracoes


```

## Participantes


### IDeviceFactory (Fábrica Abstrata)
- Define a interface para a criação de famílias de produtos relacionados, como `createPhones()` e `createWatch()`.

### AndroidFactory e AppleFactory (Fábricas Concretas)
- Implementam a interface `IDeviceFactory` para criar produtos específicos.
  - **AndroidFactory**: Cria instâncias de `AndroidPhone` e `AndroidWatch`.
  - **AppleFactory**: Cria instâncias de `IPhone` e `IWatch`.

### Phone e Watch (Produtos Abstratos)
- São classes ou interfaces que definem os tipos de produtos criados pelas fábricas.
  - **Phone**: Interface base para os diferentes tipos de telefones.
  - **Watch**: Interface base para os diferentes tipos de relógios.

### AndroidPhone, IPhone, AndroidWatch e IWatch (Produtos Concretos)
- Implementam as interfaces ou classes abstratas definidas por `Phone` e `Watch`.
  - **AndroidPhone**: Implementação concreta do produto `Phone` para dispositivos Android.
  - **IPhone**: Implementação concreta do produto `Phone` para dispositivos Apple.
  - **AndroidWatch**: Implementação concreta do produto `Watch` para dispositivos Android.
  - **IWatch**: Implementação concreta do produto `Watch` para dispositivos Apple.


## Colaborações

- **Cliente e Fábrica Abstrata**: O cliente interage com a interface da Fábrica Abstrata (`AbstractFactory`) para criar famílias de produtos relacionados, sem conhecer as classes concretas.
- **Fábrica Concreta e Produtos Concretos**: Cada Fábrica Concreta (`ConcreteFactory`) cria uma família específica de produtos concretos.
- **Produtos Abstratos e Produtos Concretos**: Os produtos concretos implementam interfaces ou classes abstratas, garantindo que as fábricas concretas possam ser substituídas sem impacto no cliente.

O cliente utiliza a Fábrica Abstrata para criar os objetos, e as Fábricas Concretas instanciam os produtos concretos necessários.


## Consequências

### Benefícios

1. **Consistência entre produtos**: Garante que os produtos criados por uma fábrica pertencem à mesma família e funcionam bem juntos.
   - Exemplo: Um sistema gráfico com botões e barras de rolagem consistentes em estilo.

2. **Isolamento da implementação**: O cliente interage apenas com interfaces ou classes abstratas, deixando o código mais flexível e desacoplado.

3. **Facilidade para introduzir novas famílias de produtos**: Adicionar uma nova família requer apenas criar uma nova Fábrica Concreta e seus produtos concretos.

4. **Organização por famílias**: Estrutura sistemas que precisam criar objetos agrupados logicamente.


### Desvantagens

1. **Aumento da complexidade**: Implementar uma Fábrica Abstrata pode gerar muitas classes (Fábricas Concretas e Produtos Concretos).

2. **Dificuldade em adicionar novos produtos**: Alterar a Fábrica Abstrata para incluir um novo produto afeta todas as Fábricas Concretas existentes.


## Implementação

1. **Definir a Fábrica Abstrata**: Declare métodos para criar cada tipo de produto relacionado.
   ```java
   interface DeviceFactory {
       Phone createPhone();
       Watch createWatch();
   }
   ```

2. **Implementar as Fábricas Concretas**: Implemente a interface da Fábrica Abstrata, criando objetos específicos de uma família.
   ```java
   class AndroidFactory implements DeviceFactory {
       public Phone createPhone() {
           return new AndroidPhone();
       }

       public Watch createWatch() {
           return new AndroidWatch();
       }
   }

   class AppleFactory implements DeviceFactory {
       public Phone createPhone() {
           return new IPhone();
       }

       public Watch createWatch() {
           return new IWatch();
       }
   }
   ```

3. **Definir os Produtos Abstratos**: Crie interfaces ou classes abstratas para os produtos.
   ```java
   interface Phone {
       void getDetails();
   }

   interface Watch {
       void getDetails();
   }
   ```

4. **Implementar os Produtos Concretos**: Implemente os produtos abstratos.
   ```java
   class AndroidPhone implements Phone {
       public void getDetails() {
           System.out.println("Este é um telefone Android.");
       }
   }

   class IPhone implements Phone {
       public void getDetails() {
           System.out.println("Este é um iPhone.");
       }
   }
   ```

5. **Usar o Padrão**: O cliente cria a fábrica concreta desejada e utiliza para criar os produtos.
   ```java
   public class Client {
       public static void main(String[] args) {
           DeviceFactory factory = new AndroidFactory();
           Phone phone = factory.createPhone();
           Watch watch = factory.createWatch();

           phone.getDetails();
           watch.getDetails();
       }
   }
   ```

## Exemplo Completo

Imagine que você quer criar um sistema para produzir telefones e relógios de diferentes marcas:

```java
public interface DeviceFactory {
    Phone createPhone();
    Watch createWatch();
}

public class AndroidFactory implements DeviceFactory {
    public Phone createPhone() {
        return new AndroidPhone();
    }

    public Watch createWatch() {
        return new AndroidWatch();
    }
}

public class AppleFactory implements DeviceFactory {
    public Phone createPhone() {
        return new IPhone();

# Builder

## Intenção
Separar a construção de um objeto complexo da sua representação de modo que o
mesmo processo de construção possa criar diferentes representações.
## Motivação 

Imagine desenvolver um sistema onde objetos complexos podem ter diferentes configurações ou versões. Gerenciar a criação desses objetos sem duplicar código e mantendo a flexibilidade para mudanças futuras pode ser um grande desafio. É nesse contexto que o padrão de projeto **Builder** se torna essencial: ele organiza o processo de construção de objetos, separando a lógica de montagem dos detalhes específicos. Isso não apenas facilita a manutenção, mas também permite reutilizar o mesmo processo de construção para criar diversas representações, promovendo clareza e modularidade no código.

## Exemplo Builder:
Imagine que você está construindo casas. Cada casa pode ter diferentes características, como materiais, design, número de cômodos, ou até mesmo o estilo arquitetônico (moderno, clássico, minimalista).

Em vez de construir cada casa do zero manualmente e misturar todos os detalhes da construção, você contrata um arquiteto (o Builder). Esse arquiteto é especializado em planejar e organizar os passos para criar casas específicas de acordo com as suas necessidades. Um gerente de obra (o Director) coordena o trabalho do arquiteto, garantindo que a construção siga o plano correto.


Se você quiser construir uma casa moderna, contrata um arquiteto especializado em design moderno. Se preferir uma casa clássica, escolhe outro arquiteto. O gerente de obras é sempre o mesmo, mas ele coordena o trabalho com base no arquiteto selecionado.

Aplicando ao software:
O Builder é útil quando você precisa criar diferentes representações ou versões de um objeto complexo, mas quer manter o processo de criação (a lógica de montagem) separado dos detalhes específicos de cada versão. Ele permite que você:

Reaproveite a lógica do "gerente" (o Director) para criar objetos diferentes.
Simplifique a manutenção e a adição de novos tipos de representações sem modificar o processo principal.

## Aplicabilidade

- Use o padrão Builder quando:
    - " o algoritmo para criação de um objeto complexo deve ser independente das partes que compõem o objeto e de como elas são montadas."
    - " o processo de construção deve permitir diferentes representações para o objeto que é construído."

## Participantes do Builder: references GOF

- **Builder**
    -  especifica uma interface abstrata para criação de partes de um objetoproduto.
    
- **ConcreteBuilder**
    -  constrói e monta partes do produto pela implementação da interface de Builder;
    -  define e mantém a representação que cria;
    -  Fornece uma interface para recuperação do produto (por exemplo, GetASCIIText, GetTextWidget).
    
- **Director**
    - constrói um objeto usando a interface de Builder.
    
- **Product**.
    -  representa o objeto complexo em construção. ConcreteBuilder constrói a representação interna do produto e define o processo pelo qual ele é montado;
    -  inclui classes que definem as partes constituintes, inclusive as interfaces para a montagem das partes no resultado final.
      
## Estrutura
![image](https://github.com/user-attachments/assets/61d9ebe7-e9a8-4beb-845a-50c1f66427c7)

## Participantes da motivação:

- **Builder (Arquiteto)**  
  - Especifica uma interface abstrata para projetar e montar as partes de uma casa (ou produto).  
  - **Exemplo**: Planeja os cômodos, o telhado, as portas e outros detalhes estruturais.

- **ConcreteBuilder (Arquiteto de Casa Moderna, Arquiteto de Casa Clássica)**  
  - Implementa a interface do Builder para criar partes específicas da casa.  
  - Define e mantém os detalhes e o design específico da casa sendo construída.  
  - Fornece o método para recuperar a casa pronta (por exemplo, `GetModernHouse()` ou `GetClassicHouse()`).  
  - **Exemplo**: Um arquiteto especializado em casas modernas cria designs com janelas amplas e linhas retas, enquanto o de casas clássicas prioriza ornamentos e telhados inclinados.

- **Director (Gerente de Obras)**  
  - Coordena o processo de construção seguindo um plano pré-definido.  
  - Não se preocupa com os detalhes de cada tipo de casa, apenas segue o plano do arquiteto escolhido.  
  - **Exemplo**: O gerente supervisiona os trabalhadores para garantir que as casas modernas ou clássicas sejam construídas corretamente.

- **Product (Casa)**  
  - Representa o objeto final criado.  
  - É o resultado do trabalho coordenado pelo Director e definido pelo ConcreteBuilder.  
  - **Exemplo**: A casa moderna com janelas amplas e linhas retas, ou a casa clássica com ornamentos e telhado inclinado.
 
## Exemplo sem Builder: 

![image](https://github.com/user-attachments/assets/c78625c2-d546-4d12-97ec-9571d5346562)

## Exemplo com Builder: 

@startuml
class Casa {
  - fundacao: String
  - paredes: String
  - telhado: String
}

abstract class ConstrutorCasa {
  # casa: Casa
  + criarNovaCasa()
  + getCasa(): Casa
  + {abstract} construirFundacao()
  + {abstract} construirParedes()
  + {abstract} construirTelhado()
}

class ConstrutorCasaModerna extends ConstrutorCasa
class ConstrutorCasaClassica extends ConstrutorCasa

class Diretor {
  - construtor: ConstrutorCasa
  + setConstrutor(construtor: ConstrutorCasa)
  + construirCasa()
  + getCasa(): Casa
}

Diretor --> ConstrutorCasa : usa
ConstrutorCasa --> Casa : cria

note right of ConstrutorCasa
  Construtor concreto
  define como a casa é construída
end note
@enduml


## Colaborações: 

- O Gerente de Obras depende de um arquiteto específico para executar os passos necessários à construção de uma casa. Ele coordena o processo, chamando os métodos do arquiteto em uma sequência definida, sem se preocupar com os detalhes da implementação de cada passo.
- O arquiteto é responsável por construir as partes individuais da casa (fundação, paredes, telhado) e entregar o resultado final. Cada implementação do arquiteto conhece os detalhes específicos de um estilo de casa.
- O Gerente de Obras obtém a casa finalizada do arquiteto e a entrega ao cliente ou a utiliza em outra parte do sistema.
- O cliente especifica ao Gerente de Obras qual tipo de casa deseja construir. O Gerente de Obras então escolhe o arquiteto apropriado para realizar o trabalho

## Consequências:

1. Separação entre o processo de construção e a representação final:
- O padrão isola a lógica de criação da estrutura de um objeto, permitindo modificar a maneira como os objetos são construídos sem alterar sua lógica interna.
- A lógica para construir o objeto é centralizada no Director, enquanto os detalhes específicos ficam nos Builders. Isso facilita a criação de diferentes representações (casas modernas, clássicas, etc.) sem duplicar código.
Manutenção facilitada:

2. Novos tipos de representações podem ser adicionados criando novos Builders, sem modificar o código do Director, promovendo o princípio Open/Closed.
Facilidade na construção de objetos complexos:
- O padrão organiza a construção de objetos que possuem muitos passos e dependências, mantendo o código mais limpo e legível.
- Permite que o Director controle a sequência e os detalhes da construção sem se preocupar com os atributos específicos de cada tipo de objeto.
- A implementação do padrão adiciona várias classes (como Builder, ConcreteBuilders e Director), o que pode ser considerado um overhead desnecessário em sistemas simples.
- Para sistemas pequenos ou com objetos simples, o uso do padrão pode parecer excessivo, já que criar uma classe para cada variação de objeto pode ser desnecessário

## Implementação:

O Cliente escolhe o tipo de objeto que deseja construir e passa essa decisão para o Director.
O Director recebe um Builder (por exemplo, ArquitetoCasaModerna) e chama os métodos do Builder em uma sequência pré-definida.
O ConcreteBuilder executa os métodos e configura as partes do objeto (Casa), armazenando o estado internamente.
Após completar o processo, o ConcreteBuilder retorna o objeto final para o Cliente.


### Exemplo:

```java

class Casa {
    private String fundacao;
    private String paredes;
    private String telhado;

    public void setFundacao(String fundacao) {
        this.fundacao = fundacao;
    }

    public void setParedes(String paredes) {
        this.paredes = paredes;
    }

    public void setTelhado(String telhado) {
        this.telhado = telhado;
    }

    @Override
    public String toString() {
        return "Casa com fundação: " + fundacao + ", paredes: " + paredes + ", telhado: " + telhado;
    }
}

// Interface Builder
abstract class ConstrutorCasa {
    protected Casa casa;

    public void criarNovaCasa() {
        casa = new Casa();
    }

    public Casa getCasa() {
        return casa;
    }

    public abstract void construirFundacao();
    public abstract void construirParedes();
    public abstract void construirTelhado();
}

// ConcreteBuilder: ConstrutorCasaModerna
class ConstrutorCasaModerna extends ConstrutorCasa {

    @Override
    public void construirFundacao() {
        casa.setFundacao("Fundação de concreto reforçado");
    }

    @Override
    public void construirParedes() {
        casa.setParedes("Paredes de vidro e aço");
    }

    @Override
    public void construirTelhado() {
        casa.setTelhado("Telhado plano com painéis solares");
    }
}

// ConcreteBuilder: ConstrutorCasaClassica
class ConstrutorCasaClassica extends ConstrutorCasa {

    @Override
    public void construirFundacao() {
        casa.setFundacao("Fundação de concreto tradicional");
    }

    @Override
    public void construirParedes() {
        casa.setParedes("Paredes de tijolo");
    }

    @Override
    public void construirTelhado() {
        casa.setTelhado("Telhado inclinado de telhas cerâmicas");
    }
}

    public Watch createWatch() {
        return new IWatch();

# Prototype
### Padrão de Projeto Criacional

## Intenção

Tem como objetivo permitir a criação de novos objetos a partir de um modelo (template) ou protótipo existente, em vez de criar uma nova instância do zero. Facilitando na criação de novos objetos que são complexos ou tem inicialização custosa, permitindo uma clonagem eficiente dos objetos, com a possibilidade de modificar ou customizar os novos objetos conforme necessário.



## Motivação

Imagine que você trabalha em uma empresa de automação de processos, e o desafio do momento é criar um sistema de gerenciamento de documentos. Cada documento pode ter um título e conteúdo personalizado, como contratos, atas de reunião e propostas comerciais.

Inicialmente, você planejou criar novas instâncias de documentos usando construtores para preencher manualmente o título e o conteúdo. Mas, à medida que os requisitos cresceram, ficou claro que certos documentos, como um contrato padrão ou uma ata de reunião básica, eram frequentemente reutilizados com pequenas alterações. Cada vez que um usuário queria criar um novo contrato, precisava preencher todos os detalhes novamente, o que era ineficiente e sujeito a erros.

Então, surge a ideia: e se você pudesse simplesmente clonar um modelo existente de documento, ajustando apenas as informações necessárias? É aqui que o padrão Prototype entra em cena.

No seu sistema, você implementa uma classe abstrata Documento que define o título e o conteúdo. Essa classe também implementa a interface Cloneable para permitir que instâncias de documentos sejam copiadas. Com isso, você pode criar protótipos de documentos (como "Contrato Padrão" ou "Ata Básica") e simplesmente cloná-los quando necessário.

Por exemplo:

Quando o departamento de vendas precisa de um novo contrato, o sistema clona o protótipo do contrato padrão e insere as informações do cliente.
Para atas de reunião, o sistema utiliza o protótipo da ata básica, ajustando o conteúdo com os detalhes da reunião específica.


## Aplicabilidade
O padrão Prototype é ideal em cenários onde se busca maior flexibilidade e eficiência no processo de criação de objetos. Ele é recomendado nos seguintes casos:

Independência da criação de objetos: Quando é necessário que o sistema funcione de forma independente de como os objetos são criados, compostos ou representados. Isso reduz o acoplamento e simplifica a manutenção.

Definição dinâmica das classes a serem instanciadas: Em situações onde as classes de objetos precisam ser especificadas durante a execução do programa, como em sistemas que utilizam carregamento dinâmico de dados ou componentes.

Evitar hierarquias de classes complexas: Quando seria necessário criar uma hierarquia paralela de classes de fábrica para suportar a criação de produtos. O Prototype elimina essa necessidade ao permitir que os objetos sejam clonados diretamente.

Gerenciamento eficiente de estados diferentes: Quando uma classe pode assumir apenas um número limitado de combinações de estados, é mais prático criar protótipos para cada configuração inicial e cloná-los conforme necessário, em vez de configurar manualmente cada instância repetidamente.


## Estrutura

```mermaid
---
title: Sistema de Documentos
---
classDiagram
    Documento <|-- Relatorio
    Documento <|-- Contrato
    class Documento {
        +String titulo
        +String conteudo
        +clone() Documento
        +exibirDetalhes() void
    }
    class Relatorio {
        +String autor
        +exibirDetalhes() void
    }
    note for Relatorio "Relatórios personalizados como Relatório Financeiro"
    class Contrato {
        +String nomeCliente
        +exibirDetalhes() void
    }
    note for Contrato "Contratos específicos como Compra e Venda"


```
## Participantes 

- Documento (abstrato): Define os atributos e métodos comuns de um documento. 
- Contrato (clone): Utiliza como modelo (template) a classe documento, e adiciona um atrituto específico do contrato.
- Relatorio (clone): Utiliza como modelo (template) a classe documento, e adiciona um atrituto específico do relatório.

## Outro Exemplo
```mermaid
---
title: Criação de Personagens
---
classDiagram
    Personagem <|-- Guerreiro
    Personagem <|-- Mago
    class Personagem {
        +String nome
        +int nivel
        +int vida
        +int ataque
        +int defesa
        +clone() Personagem
        +exibirDetalhes() void
    }
    class Guerreiro {
        +int força
        +int armadura
        +exibirDetalhes() void
    }
    class Mago {
        +int mana
        +int poderMagico
        +exibirDetalhes() void
    }
    note for Guerreiro "Personagem com alta defesa e força física"
    note for Mago "Personagem com habilidades mágicas e alto poder de ataque"
```
## Participantes

- Personagem (abstrato): Define os atributos e métodos comuns para todos os personagens.
- Guerreiro (clone): Representa um personagem do tipo Guerreiro, com atributos relacionados à força e defesa.
- Mago (clone): Representa um personagem do tipo Mago, com atributos relacionados à mana e poder mágico.


## Consequências
Prototype tem muitas das mesmas consequências que o Abstract Factory e Builder:
- Oculta as classes de produtos concretos do cliente, reduzindo a quantidade de informações que ele precisa conhecer, permitindo que o cliente crie novos objetos a partir de protótipos existentes, sem precisar entender ou interagir diretamente com o código das classes concretas.
  
### Benefícios adicionais do Prototype:

1. Modificação dinâmica de protótipos: O padrão permite modificar ou estender protótipos de objetos durante a execução do programa.
2. Criação de objetos com valores variados: Permite criar novos objetos com diferentes valores baseados em um protótipo, ajustando suas propriedades conforme necessário.
3. Variação de estrutura através de clonagem: A estrutura de um objeto pode ser alterada ao cloná-lo a partir de um protótipo e adicionar ou modificar seus atributos.
4. Redução de subclasses: Evita a criação de múltiplas subclasses, criando objetos a partir de protótipos e personalizando-os conforme necessário.
5. Criação dinâmica de objetos: O padrão Prototype permite criar e configurar objetos de maneira dinâmica, sem a necessidade de uma hierarquia rígida de classes.

- O ponto fraco do Prototype é a complexidade envolvida na clonagem de objetos com estruturas internas complexas. Quando um objeto possui referências a outros objetos ou contém um estado interno complexo, pode ser difícil garantir que a clonagem seja feita corretamente, sem gerar problemas como cópias superficiais em vez de cópias profundas (deep copies). Isso pode resultar em erros, como a modificação indesejada de objetos compartilhados entre o protótipo e suas cópias, além de aumentar a complexidade do código para gerenciar essas clonagens de maneira eficaz.





## Implementação 
 ### Pode ser um desafio implementar de maneira correta o padrão prototype, dentre eles:
1. Implementar a operação de clonagem corretamente : O padrão Prototype exige a implementação de uma operação de clonagem precisa para garantir que o novo objeto seja uma cópia exata do protótipo, sem causar problemas como referências compartilhadas inadvertidas.

2. Gerenciar protótipos de forma eficiente : Em sistemas complexos, pode ser difícil organizar e manter os protótipos de maneira eficiente, garantindo que eles sejam facilmente reutilizáveis e adaptáveis para diferentes tipos de objetos.

3. Garantir a inicialização adequada dos clones: Quando se clona um objeto, é importante garantir que a inicialização do clone seja feita corretamente, com todos os atributos e estados sendo copiados ou ajustados de acordo com o comportamento desejado.

### Processo de clonagem
  
O processo de clonagem de um objeto pode ser feito usando duas abordagens:

1. Shallow Copy (ou cópia superficial): 
Copia os valores primitivos e as referências dos objetos, mas não os objetos em si. As referências no novo objeto apontam para os mesmos objetos que as do original, ou seja, o objeto pai é clonado, mas seus filhos são compartilhados entre os objetos.

2. Deep Copy (ou cópia profunda):
Copia o objeto e todos os objetos aos quais ele se refere, criando novas instâncias para todos os elementos. O objeto pai e todos os objetos contidos nele são clonados, garantindo que não haja referências compartilhadas.

- Na implementação, você pode achar o prototype bastante parecido com a **Herança**, pois ambos permitem que objetos ou classes compartilhem propriedades e métodos. No entanto, eles são diferentes em sua implementação e estrutura.
  
- A herança clássica é baseada em classes e estabelece uma hierarquia fixa entre elas, onde as subclasses herdam os métodos e propriedades das classes pai. Já o prototype é baseado em objetos e referências de protótipos, permitindo que objetos compartilhem comportamento dinamicamente. A herança clássica é mais rígida e hierárquica, enquanto o prototype oferece flexibilidade, pois as relações podem ser modificadas em tempo de execução.
  
- Dito isso, no JavaScript, é possível simular o mecanismo da herança, utilizando o prototype para compartilhar propriedades e métodos entre objetos. Embora o JavaScript não tenha um sistema de classes como em linguagens tradicionais, ele permite que objetos "herdem" comportamentos de outros objetos por meio das suas referências de protótipo. Isso é feito de forma dinâmica, permitindo flexibilidade, como a capacidade de modificar ou substituir o protótipo de um objeto a qualquer momento, sem a necessidade de uma hierarquia fixa de classes.

## Exemplo de código 
### 1. Criando classe abstrata, que servirá como protótipo
```java
 public abstract class Documento implements Cloneable {
    private String titulo;
    private String conteudo;

    public Documento(String titulo, String conteudo) {
        this.titulo = titulo;
        this.conteudo = conteudo;
    }

    public String getTitulo() {
        return titulo;
    }

    public void setTitulo(String titulo) {
        this.titulo = titulo;
    }
}

public interface Phone {
    void getDetails();
}

public class AndroidPhone implements Phone {
    public void getDetails() {
        System.out.println("Telefone Android criado.");
    }
}

public class IPhone implements Phone {
    public void getDetails() {
        System.out.println("iPhone criado.");
    }
}

public String getConteudo() {
        return conteudo;
    }

    public void setConteudo(String conteudo) {
        this.conteudo = conteudo;
    }

    @Override
    public Documento clone() {
        try {
            return (Documento) super.clone();
        } catch (CloneNotSupportedException e) {
            throw new RuntimeException("Erro ao clonar documento!", e);
        }
    }

    public abstract void exibirDetalhes();
}
 
```
**Explicação: A classe Documento é abstrata e implementa a interface Cloneable, permitindo que objetos derivados sejam clonados. Ela possui dois atributos, titulo e conteudo, com métodos getters e setters para acesso e modificação. O método clone() é sobrescrito para criar uma cópia do documento utilizando o método super.clone(), realizando uma clonagem superficial. Além disso, a classe contém um método abstrato exibirDetalhes(), que deve ser implementado pelas subclasses para exibir informações específicas do documento. Essa estrutura facilita a criação de diferentes tipos de documentos com base em um modelo comum.**

### 2. Criando a classe Contrato, que é uma cópia da classe Documento
```java
public class Contrato extends Documento {
    private String nomeCliente;

    public Contrato(String titulo, String conteudo, String nomeCliente) {
        super(titulo, conteudo);
        this.nomeCliente = nomeCliente;
    }

    public String getNomeCliente() {
        return nomeCliente;
    }

    public void setNomeCliente(String nomeCliente) {
        this.nomeCliente = nomeCliente;
    }

    @Override
    public void exibirDetalhes() {
        System.out.println("Contrato:");
        System.out.println("Título: " + getTitulo());
        System.out.println("Conteúdo: " + getConteudo());
        System.out.println("Cliente: " + nomeCliente);
        System.out.println();
    }
}
```
**Explicação: A classe Contrato estende a classe Documento e adiciona um atributo específico, nomeCliente, que representa o nome do cliente associado ao contrato. O construtor da classe Contrato chama o construtor da classe pai Documento para inicializar os atributos titulo e conteudo, além de inicializar nomeCliente. Os métodos getNomeCliente() e setNomeCliente() permitem acessar e modificar o nome do cliente. O método exibirDetalhes() é uma implementação do método abstrato da classe pai, exibindo as informações completas do contrato, incluindo o título, o conteúdo e o nome do cliente, proporcionando uma exibição dos detalhes do contrato.**

### 3. Conclusão
O código define uma estrutura de documentos utilizando o padrão Prototype. A classe Documento é abstrata e implementa a interface Cloneable, permitindo a clonagem de objetos. Ela possui atributos básicos como titulo e conteudo, com métodos para acessá-los e modificá-los, e um método abstrato exibirDetalhes() que deve ser implementado nas subclasses. A classe Contrato estende Documento, adicionando o atributo nomeCliente e implementando o método exibirDetalhes() para mostrar informações específicas do contrato. Essa estrutura permite a criação de documentos clonáveis e personalizados, como contratos, facilitando a reutilização e modificação de objetos sem a necessidade de reescrever todo o conteúdo.






## Usos Conhecidos 

### O padrão Prototype está bastante presente nos dias atuais, exploraremos alguns exemplos práticos utilizados no dia a dia, tanto no desenvolvimento de software quanto em outras áreas, que por muitas vezes acabamos nem percebendo.

1. Desenvolvimento de Jogos:
No desenvolvimento de jogos, o Prototype é utilizado para criar personagens e objetos semelhantes, mas com variações. Em vez de recriar objetos complexos de personagens, como magos ou guerreiros, a partir do zero, é possível criar um personagem genérico (protótipo) e, a partir dele, gerar clones com atributos diferentes, como pontos de vida ou força.

2. Gerenciamento de Documentos e Relatórios:
O padrão Prototype é útil para duplicar documentos e relatórios com estruturas semelhantes, mas com dados específicos modificados. Em sistemas de geração de relatórios ou processamento de texto, é comum utilizar um protótipo de documento para criar novas instâncias de forma rápida.

3. Interfaces de Usuário (UI):
Em sistemas de interface de usuário, o Prototype pode ser usado para criar componentes reutilizáveis. Elementos como botões, caixas de texto e tabelas podem ser definidos como protótipos, e a partir deles, clones podem ser criados com diferentes propriedades, como cor, tamanho ou comportamento.

4. Gerenciamento de Configurações:
Em sistemas que exigem configurações dinâmicas, como em plataformas de e-commerce ou jogos, o padrão Prototype pode ser usado para criar novas configurações baseadas em um modelo inicial. Isso é útil quando as configurações de diferentes módulos ou componentes são semelhantes, mas com pequenas variações.

5. Fluxos de Trabalho e Processos:
O conceito de clonagem de fluxos de trabalho ou processos é uma aplicação interessante do padrão Prototype. Isso permite a criação de diferentes versões de um processo com variações mínimas, com base em um modelo genérico.

6. Produção em Massa de Produtos:
No mundo físico, o Prototype é utilizado em linhas de produção para criar produtos com variações, mas com uma base comum. O padrão permite a criação de diferentes versões de um produto, clonando um protótipo e personalizando detalhes conforme necessário.


## Padrões Relacionados 
Prototype e Abstract Factory têm em comum o objetivo de **abstrair a criação de objetos**, permitindo ao cliente criar instâncias sem conhecer detalhes de implementação. O Prototype cria objetos clonando um protótipo existente, enquanto o Abstract Factory cria famílias de objetos relacionados. Eles podem ser usados em conjunto, com o Abstract Factory coordenando e armazenando a criação de produtos (protótipos) e o Prototype permitindo clonar e personalizar esses objetos conforme necessário.

## Referências 

GAMMA, Erich; HELM, Richard; JOHNSON, Ralph; VLISSIDES, John. Padrões de Projetos: Soluções Reutilizáveis de Software Orientados a Objetos. Trad. Luiz A. Meirelles Salgado; Fabiano Borges Paulo. 1. ed. Porto Alegre: Bookman, 2000.


[Macoratti.net](https://macoratti.net/21/08/c_prototype1.htm#:~:text=Deep%20Copy%20(ou%20c%C3%B3pia%20profunda,os%20objetos%20que%20o%20cont%C3%AAm. )

## Usos Conhecidos

1. **Sistemas gráficos multiplataforma**: Criar interfaces adaptadas para diferentes sistemas operacionais.
2. **Bibliotecas de persistência**: Fornecer implementações diferentes para diversos tipos de bancos de dados.
3. **Frameworks de jogos**: Configurar texturas, sons e objetos específicos para cada plataforma.


## Conclusão

O padrão **Abstract Factory** é uma solução poderosa para criar famílias de objetos relacionados, mantendo a consistência e a flexibilidade do código. Ele é ideal para sistemas que precisam suportar múltiplas variações de produtos, desde que a complexidade adicional seja gerenciável.



// Diretor
class Diretor {
    private ConstrutorCasa construtor;

    public void setConstrutor(ConstrutorCasa construtor) {
        this.construtor = construtor;
    }

    public void construirCasa() {
        construtor.criarNovaCasa();
        construtor.construirFundacao();
        construtor.construirParedes();
        construtor.construirTelhado();
    }

    public Casa getCasa() {
        return construtor.getCasa();
    }
}

public class BuilderExample {
    public static void main(String[] args) {
        Diretor diretor = new Diretor();

        // Construir uma casa moderna
        ConstrutorCasa construtorModerno = new ConstrutorCasaModerna();
        diretor.setConstrutor(construtorModerno);
        diretor.construirCasa();
        System.out.println(diretor.getCasa());

        // Construir uma casa clássica
        ConstrutorCasa construtorClassico = new ConstrutorCasaClassica();
        diretor.setConstrutor(construtorClassico);
        diretor.construirCasa();
        System.out.println(diretor.getCasa());
    }
}
```

## Usos conhecidos:

O padrão **Builder** é amplamente utilizado em várias situações onde objetos complexos precisam ser criados de maneira flexível e reutilizável.

###  Interface de Criação de GUI (Interfaces Gráficas de Usuário)

**Exemplo**: **Frameworks de interface de usuário como Swing ou JavaFX**
- Em frameworks de interface gráfica, onde você pode criar painéis, botões e menus com várias opções, como texto, ícones, eventos, cores, etc.
- O padrão Builder ajuda a criar componentes complexos de interface sem a necessidade de construir cada elemento manualmente.

**Aplicação**: Frameworks de interface de usuário em Java ou C#, onde você deseja permitir a criação de uma interface flexível e modular.

---

### Construção de Consultas Complexas (Query Builders)

**Exemplo**: Consulta a bancos de dados SQL
- Quando você precisa construir consultas SQL complexas com diversas condições, joins, agrupamentos, etc., o Builder pode ser usado para criar essas consultas de forma legível e modular.
- Isso permite que você adicione facilmente novas cláusulas ou condições sem quebrar o código.

**Aplicação**: Bibliotecas como **Hibernate Criteria API** ou **JPA Criteria API**, que permitem construir consultas dinâmicas e flexíveis sem concatenar strings SQL manualmente.

---

### . Criação de Objetos de Documentos (Exemplo: PDF ou HTML)

**Exemplo**: **Geradores de Documentos**
- Geradores de documentos (PDF, Word, HTML) onde cada documento pode ter diferentes seções, tabelas, listas, parágrafos, imagens, etc.
- O Builder permite que você construa esses documentos de forma modular e eficiente, sem ter que lidar com a complexidade de cada componente individual.

**Aplicação**: Geradores de relatórios PDF em bibliotecas como **Apache PDFBox** ou **iText**, onde os documentos podem ser construídos passo a passo (tabelas, textos, imagens, etc.) utilizando um único objeto `Builder`.

---

###  Criando APIs Fluentes

- Muitas vezes, o padrão Builder é usado para criar **APIs fluentes**, onde você pode encadear chamadas de método de forma legível e fácil de usar, configurando um objeto de forma incremental.
- O padrão é ideal para cenários onde você tem muitas opções de configuração e deseja permitir um fluxo contínuo de chamadas de métodos.

**Aplicação**: Configuração de APIs em bibliotecas Java ou frameworks como **Spring**, onde você pode configurar beans ou objetos de forma incremental, por exemplo, ao definir uma configuração de serviço, a configuração de banco de dados, ou a configuração de segurança.

---

### Construção de Objetos Imutáveis

- Em muitas linguagens de programação, o padrão Builder é utilizado para construir objetos imutáveis, onde os objetos não podem ser alterados após sua criação. 
- O Builder é usado para preencher os valores do objeto durante a construção, e uma vez que o objeto está pronto, ele não pode ser modificado.

**Aplicação**: Uso do Builder em bibliotecas como **Guava** ou **Java**, quando se deseja criar objetos imutáveis com um número variável de parâmetros.

--- 
### Compilação de Modelos de Arquitetura
Arquiteturas de software complexas
O padrão Builder é útil na criação de componentes de software que exigem configuração detalhada, como componentes de sistemas distribuídos ou microserviços, onde cada serviço pode ter diferentes opções de configuração e implementação.
Arquitetura de sistemas distribuídos em que você precisa construir a estrutura de um sistema com múltiplos nós e serviços, cada um com suas próprias configurações e opções.

.
[Mermaid Class Diagram.html](https://mermaid.js.org/syntax/classDiagram.html)


# Object Pool (Padrão Criacional)

## Intenção

Gerenciar a criação, armazenamento, emprestimo, retomada e reutilização de instancias de objeto, com o objetivo de controlar a quantidade de instancias existentes ou previnir o processo de criação e destruição recorentes quando estes forem considerados caros.

## Também conhecido como

Pool de recursos

## Motivação

Para se comunicar com um banco de dados, é necessario estabelecer uma "conexão" com ele:

```java
Connection connection = DriverManager.getConnection(
                "jdbc:mysql://localhost:3306/meu_banco",
                "usuario",
                "senha" )

Statement statement = connection.createStatement();

ResultSet resultSet = statement.executeQuery("SELECT id, nome FROM clientes");
```

Em uma aplicação como um sistema web, onde varias requisições chegam o tempo todo, e para cada requisição é comum termos que acessar o banco de dados uma ou mais vezes, nesse caso, para cada acesso precisariamos instanciar a conexão.

```plantuml
@startuml
actor Client
participant "Web Server" as Server
participant "Database" as DB

Client -> Server: HTTP Request
Server -> Server: Instantiate Connection
Server -> DB: Query Data
DB --> Server: Return Data
Server -> Server: Destroy Connection
Server --> Client: HTTP Response

@enduml
```
![FluxoSemPool](./images/FluxoSemPool.png)

Isso rapidamente apresenta um problema, estabelecer uma conexão com banco de dados é um processo relativamente caro e demorado, é necessario a realização de diversas etapas tanto no servidor de banco quanto no cliente que está se conectando. Além disso, servidores de banco de dados possuem um numero maximo de conexões simultaneas que ele pode manter.

Em um cenario em que por exemplo uma aplicação receba 1000 requisições/s, e para cada requisição sejam necessarias em media 2 consultas ao banco de dados, isso significa que estariamos instanciando e destruindo 2000 conexões por segundo, um numero que facilmente extrapolaria o limite de conexões de um banco de dados.

Em vez disso, usando o pattern de object pool, podemos implementar uma classe que sirva como pool de conexões, dessa forma, ao precisarmos de uma conexão solicitamos ao pool, que ira nos fornecer uma que ja existe. Ao terminamos de usar a conexão, devolvemos ela ao pool.


```plantuml
@startuml
actor Client
participant "Web Server" as Server
participant "Connection Pool" as Pool
participant "Database" as DB

Client -> Server: HTTP Request
Server -> Pool: Request Connection
Pool --> Server: Provide Connection
Server -> DB: Query Data
DB --> Server: Return Data
Server -> Pool: Return Connection
Server --> Client: HTTP Response

@enduml
```
![FluxoSemPool](./images/FluxoComPool.png)


## Aplicabilidade

Use object pool quando:

- For **demorado** criar uma instancia
- For **caro** em recursos criar uma instancia
- For **demorado** destruir uma instancia
- For **caro** em recursos destruir uma instancia

- Existe um **limite** de quantas instancias possam existir simultaneamente

Com **recursos**, queremos dizer por exemplo cpu, ram, disco e rede por exemplo

Não use object pool quando:

- O **custo** de **manter** a instancia, mesmo quando não está sendo usada, supera o custo de instanciala.

## Estrutura


```plantuml
@startuml
left to right direction
interface PoolInterface {
    + acquire(): Object
    + release(Object): void
}

interface PooledObjectFactoryInterface {
    + createObject(): Object
}

class PoolConcrete implements PoolInterface {
    - PooledObjectFactoryInterface factory
    + acquire(): Object
    + release(Object): void
}

class ObjectFactoryConcrete implements PooledObjectFactoryInterface {
    + createObject(): Object
}

class Client {
    - PoolInterface pool
    + usePool(): void
}

PoolConcrete --> ObjectFactoryInterface : uses
Client --> PoolInterface : interacts with
@enduml
```
![Texto alternativo](./images/Estrutura.png)

## Participantes

- **PoolInterface**
    - Define uma interface comum para todas as implementações de classes de pool de objetos
- **ObjectFactoryInterface** 
    - Define uma interface comum para todas as implementações de classes fabricas de objetos que seram guardadas em pool.
- **Client**
   - Aquele que necessita das instancias do objeto que sera guardado em pool.

## Implementação

- Implementação de um pool

```java
package com.example.implementations.simple;

import java.util.ArrayList;
import java.util.List;

import com.example.interfaces.PoolInterface;
import com.example.interfaces.PooledObjectFactory;

public class SimplePool<T> implements PoolInterface<T> {
    private List<T> instanciasLivres; 
    private List<T> instanciasEmUso;  
    private PooledObjectFactory<T> factory;

    public SimplePool(int size, PooledObjectFactory<T> factory) {
        this.factory = factory;
        this.instanciasLivres = new ArrayList<>();
        this.instanciasEmUso = new ArrayList<>();

        for (int i = 0; i < size; i++) {
            instanciasLivres.add(factory.create());
        }
    }

    @Override
    public T acquire() {
        if (!instanciasLivres.isEmpty()) {
            T instance = instanciasLivres.remove(0);  
            instanciasEmUso.add(instance);  
            return instance;
        }
        return null;
    }

    @Override
    public void release(T instance) {
        if (instanciasEmUso.remove(instance)) {
            instanciasLivres.add(instance);
        }
    }

    @Override
    public void destroyAll() {
        for (T instance : instanciasLivres) {
            factory.destroy(instance);
        }
        for (T instance : instanciasEmUso) {
            factory.destroy(instance);
        }
    }
}
```

## Exemplo de código

- Uso de um pool

```java
PoolInterface<CheapObject> pool = new SimplePool<CheapObject>(1, new CheapObjectFactory());

        for (int i = 0; i < 100; i++) {
            CheapObject cheapObject = pool.acquire();
            cheapObject.doSomething();
            pool.release(cheapObject);
        }
```

## Usos conhecidos

- Conexões com bancos de dados geralmente são gerenciados por um object pool

- Servidores web e de aplicação implementam um pool de threads para o processamento de requisições

- Em aplicações multithreads, threads de trabalho são gerenciadas por um object pool

## Padrão relacionados


## Referências

### No diagrama do Singleton:

- **Logger**: Classe responsável por controlar a criação e gerenciar a instância única. Contém um método `static getInstance()`, que será utilizado para o retorno da instância única. 

### No diagrama do Multiton:

- **ConfiguracaoComMultiton**: Gerencia múltiplas instâncias da classe com base em uma chave. Ele garante que cada chave única corresponde a uma única instância, permitindo compartilhar configurações específicas por módulo.

- **Instâncias**: Mantidas no mapa INSTANCIAS, que associa cada chave (String) a sua respectiva instância de ConfiguracaoComMultiton. Este mapa é o núcleo do padrão Multiton, garantindo controle centralizado das instâncias.

- **Configurações**: Dados armazenados dentro de cada instância em configuracoes. Este mapa contém pares chave-valor para configurações específicas ao contexto do módulo correspondente.

## Outro Exemplo

### Sem Singleton:

```java
public class SemSingleton {
    public class Logger {
    private String nomeArquivo;

    public Logger(String nomeArquivo) {
        this.nomeArquivo = nomeArquivo;
    }

    public void log(String mensagem) {
        System.out.println("Escrevendo no arquivo " + nomeArquivo + ": " + mensagem);
        // Simulando escrita no arquivo (sem implementar realmente)
    }
}

public class Aplicacao {
    public static void main(String[] args) {
        // Criando múltiplas instâncias do Logger
        Logger logger1 = new Logger("log1.txt");
        Logger logger2 = new Logger("log2.txt");

        // PROBLEMA 1: Logs inconsistentes
        // Cada instância escreve em um arquivo diferente. Se o sistema
        // depende de um único arquivo centralizado, isso causa confusão.
        logger1.log("Primeira mensagem de logger1");
        logger2.log("Primeira mensagem de logger2");

        // PROBLEMA 2: Conflito de arquivos
        // Se várias instâncias tentarem gravar no mesmo arquivo, dados podem ser corrompidos.
        Logger logger3 = new Logger("log1.txt");
        logger3.log("Mensagem de logger3");

        // PROBLEMA 3: Perda de centralização
        // Não há um único ponto de acesso ao Logger. Alterar configurações em uma
        // instância (ex.: mudar o arquivo de log) não afeta as outras instâncias.

        // PROBLEMA 4: Sobrecarga de recursos
        // Criar muitas instâncias pode consumir memória desnecessariamente
        // e aumentar a complexidade do sistema.
    }
}

}
```

### Com Singleton:

```java
// Implementação de um Logger usando Singleton para resolver problemas anteriores
public class Logger {
    private static Logger instanciaUnica; // Garantindo uma única instância
    private String nomeArquivo;

    // Construtor privado para evitar criação de múltiplas instâncias
    private Logger(String nomeArquivo) {
        this.nomeArquivo = nomeArquivo;
    }

    static {
        instanciaUnica = new Logger("logCentral.txt");
    }

    // Método público para obter a única instância do Logger
    public static Logger getInstance() {
        return instanciaUnica;
    }

    public void log(String mensagem) {
        System.out.println("Escrevendo no arquivo " + nomeArquivo + ": " + mensagem);
        // Simulando escrita no arquivo (sem implementação real para simplicidade)
    }

    public static void main(String[] args) {
        // Obtendo a única instância do Logger
        System.out.println(Logger.getInstance());

        // PROBLEMA 1 RESOLVIDO: Logs consistentes
        // Todos os logs são centralizados em um único arquivo
        Logger.getInstance().log("Primeira mensagem de log");
        Logger.getInstance().log("Segunda mensagem de log");

        // PROBLEMA 2 RESOLVIDO: Conflito de arquivos
        // Como não há outro arquivo, o log é escrito no arquivo original
        Logger.getInstance().log("Terceira mensagem de log");

        // PROBLEMA 3 RESOLVIDO: Centralização
        // O acesso ao Logger é feito por meio do método `getInstance`, garantindo
        // que alterações na configuração (como nome do arquivo) sejam feitas na
        // instância única.

        // PROBLEMA 4 RESOLVIDO: Economia de recursos
        // Apenas uma instância do Logger é criada, reduzindo consumo de memória.
    }
}

```

### Sem Multiton:

```java
import java.util.HashMap;
import java.util.Map;

public class ConfiguracaoSemMultiton {
    private String modulo;
    private Map<String, String> configuracoes;

    public ConfiguracaoSemMultiton(String modulo) {
        this.modulo = modulo;
        this.configuracoes = new HashMap<>();
    }

    public void adicionarConfiguracao(String chave, String valor) {
        configuracoes.put(chave, valor);
    }

    public String obterConfiguracao(String chave) {
        return configuracoes.get(chave);
    }

    public String getModulo() {
        return modulo;
    }

    public static void main(String[] args) {
        // Criando instâncias separadas para diferentes módulos
        ConfiguracaoSemMultiton configPagamento = new ConfiguracaoSemMultiton("Pagamento");
        configPagamento.adicionarConfiguracao("endpoint", "https://api.pagamentos.com");
        configPagamento.adicionarConfiguracao("timeout", "30s");

        ConfiguracaoSemMultiton configRelatorio = new ConfiguracaoSemMultiton("Relatorio");
        configRelatorio.adicionarConfiguracao("endpoint", "https://api.relatorios.com");
        configRelatorio.adicionarConfiguracao("timeout", "60s");

        // Problema 1: Instâncias duplicadas podem ser criadas para o mesmo módulo
        ConfiguracaoSemMultiton configPagamentoDuplicado = new ConfiguracaoSemMultiton("Pagamento");
        configPagamentoDuplicado.adicionarConfiguracao("timeout", "15s");

        // Problema 2: Não há controle centralizado das instâncias
        System.out.println("Timeout do módulo Pagamento (instância 1): " + configPagamento.obterConfiguracao("timeout"));
        System.out.println("Timeout do módulo Pagamento (instância duplicada): " + configPagamentoDuplicado.obterConfiguracao("timeout"));

        // Problema 3: Difícil garantir que cada módulo tenha uma única configuração global
        System.out.println("Endpoint do módulo Relatorio: " + configRelatorio.obterConfiguracao("endpoint"));
    }
}

```

### Com Multiton:

```java
import java.util.HashMap;
import java.util.Map;

public class ConfiguracaoComMultiton {
    private static final Map<String, ConfiguracaoComMultiton> INSTANCIAS = new HashMap<>();

    private Map<String, String> configuracoes;

    // Construtor privado para evitar instância externa
    private ConfiguracaoComMultiton() {
        this.configuracoes = new HashMap<>();
    }

    static {
        INSTANCIAS.put("Pagamento", new ConfiguracaoComMultiton());

        INSTANCIAS.put("Relatorio", new ConfiguracaoComMultiton());
    }

    // Método estático para obter a instância única de cada módulo
    public static ConfiguracaoComMultiton getInstance(String modulo) {
        return INSTANCIAS.get(modulo);
    }

    public void adicionarConfiguracao(String chave, String valor) {
        configuracoes.put(chave, valor);
    }

    public String obterConfiguracao(String chave) {
        return configuracoes.get(chave);
    }

    public static Integer getSizeInstancias() {
        return INSTANCIAS.size();
    }

    public static void main(String[] args) {

        // Garantindo que sempre obtemos a mesma instância para um módulo
        ConfiguracaoComMultiton configPagamentoRef = ConfiguracaoComMultiton.getInstance("Pagamento");
        configPagamentoRef.adicionarConfiguracao("timeout", "30s");
        System.out.println("Timeout do módulo Pagamento (única instância): " + configPagamentoRef.obterConfiguracao("timeout"));
   
        // Sem duplicação de instâncias
        ConfiguracaoComMultiton configRelatorioRef = ConfiguracaoComMultiton.getInstance("Relatorio");
        configRelatorioRef.adicionarConfiguracao("endpoint", "/api/relatorio");
        System.out.println("Endpoint do módulo Relatorio: " + configRelatorioRef.obterConfiguracao("endpoint"));
   
        // Todas as instâncias são controladas centralmente pelo Multiton
        System.out.println("Número de módulos registrados: " + ConfiguracaoComMultiton.getSizeInstancias());
    }
   
}
```

## Colaborações

- **Singleton**: Colabora com a classe que precisa de uma instância única, garantindo que todas as requisições acessem a mesma instância.
- **Multiton**: Colabora com a classe que precisa de diferentes instâncias dependendo de um contexto ou chave.

## Consequências

### Vantagens e Desvantagens do Singleton:

- **Vantagens**: Controle total sobre a criação da instância, economizando recursos.
- **Desvantagens**: Pode ser difícil de testar (por causa da instância única), além de ser potencialmente uma dependência global.

### Vantagens e Desvantagens do Multiton:

- **Vantagens**: Controla a criação de instâncias únicas por chave, sem redundâncias.
- **Desvantagens**: Pode consumir mais memória ao criar múltiplas instâncias, dependendo de como as chaves são gerenciadas.

## Exemplos de Código

### Exemplo de Singleton:
#### Exemplo Jogo 

 - **Problema**:No cenário de um jogo 2D, temos os personagens principais: o Player (representado por uma bola roxa), a Moeda (representada por uma bola amarela) e o Inimigo (um hexágono vermelho).
 O objetivo do jogador é coletar o máximo de moedas enquanto evita colisões com os inimigos. Caso o Player colida com o Inimigo, ele perde sua vida, e, se a vida chegar a zero, ele "morre", precisando ressurgir no ponto inicial da fase, mas mantendo as moedas já coletadas.
 Para que as informações do Player (como quantidade de moedas e pontos de vida) não sejam perdidas durante as transições de cena ou reinicialização da fase, precisamos garantir que os dados permaneçam intactos. Além disso, o Player não deve ter múltiplas instâncias no jogo, independentemente de quantas vezes a cena for reiniciada ou trocada.
 
 - **Solução com Singleton**: Para resolver o problema, utilizamos o Padrão Singleton. O Player terá uma única instância em todo o jogo, que persistirá entre as cenas e fases. Isso é feito utilizando a lógica de Singleton no método Awake, onde garantimos que, se já existir uma instância do Player, a nova será destruída.
Com isso, as informações como quantidade de moedas e pontos de vida não serão perdidas entre as transições de cena ou reinicializações da fase. Além disso, o Player mantém o ponto de respawn inicial, garantindo uma lógica eficiente e controlada.
-- script do player (bola roxa) --
```C#
    using UnityEngine;

public class Player : MonoBehaviour
{
    // Singleton
    public static Player Instance { get; private set; }

    // Atributos do player
    public int moedas { get; private set; } = 0;
    public int vida { get; private set; } = 100;

    public float velocidade = 5f; // Velocidade de movimento do player
    private Vector3 spawnPoint;  // Ponto de ressurgimento

    private Rigidbody2D rb; // Rigidbody2D para movimento físico

    private void Awake()
    {
        // Garantir Singleton
        if (Instance == null)
        {
            Instance = this;
            DontDestroyOnLoad(gameObject); // Persistir entre cenas
        }
        else
        {
            Destroy(gameObject); // Evitar múltiplas instâncias
        }
    }

    private void Start()
    {
        spawnPoint = transform.position; // Salvar o ponto inicial como ponto de ressurgimento
        rb = GetComponent<Rigidbody2D>(); // Obter referência ao Rigidbody2D
    }

    private void Update()
    {
        // Capturar entrada do teclado e movimentar o player
        Mover();
    }

    // Método para movimentar o player
    private void Mover()
    {
        float horizontal = Input.GetAxis("Horizontal"); // Entrada nas setas/A-D
        float vertical = Input.GetAxis("Vertical");     // Entrada nas setas/W-S

        Vector2 movimento = new Vector2(horizontal, vertical) * velocidade;
        rb.velocity = movimento; // Aplicar movimento ao Rigidbody2D
    }

    // Atualizar as moedas
    public void ColetarMoeda(int quantidade)
    {
        moedas += quantidade;
        Debug.Log("Moedas coletadas: " + moedas);
    }

    // Lidar com dano
    public void ReceberDano(int dano)
    {
        vida -= dano;
        if (vida <= 0)
        {
            Morrer();
        }
    }

    // Morrer e ressurgir
    private void Morrer()
    {
        Debug.Log("Player morreu!");
        vida = 100; // Restaurar vida
        Ressurgir();
    }

    private void Ressurgir()
    {
        transform.position = spawnPoint; // Voltar para o ponto de spawn
        Debug.Log("Player ressurgiu com " + moedas + " moedas.");
    }
}
```
-- script da moeda (bola amarela) --

```C#
using UnityEngine;

public class Moeda : MonoBehaviour
{
    public int valor = 1;

    private void OnTriggerEnter2D(Collider2D other)
    {
        if (other.CompareTag("Player"))
        {
            Player.Instance.ColetarMoeda(valor);
            Destroy(gameObject); // Destruir moeda após coletar
        }
    }
}
```

--script do inimigo (hexágono vermelho)--

```C#
using UnityEngine;

public class Inimigo : MonoBehaviour
{
    public int dano = 100;

    private void OnTriggerEnter2D(Collider2D other)
    {
        if (other.CompareTag("Player"))
        {
            Player.Instance.ReceberDano(dano);
        }
    }
}
```
### Exemplo de Multiton -Enum 

 - Tendo em vista que, o Enum é imutavel (em java) ele se enquadra como outro exemplo do padrão Multiton, uma vez que, cada constante de enum é, na prática, uma instância estática e única da própria classe. As instancias de enum's são estabelecidas em tempo de execução.
 Quem chama o enum fornece uma chave para o multion para obter a instância desejada.

 - **Problema** 
    - O sistema precisa calcular impostos (por exemplo, imposto de vendas e imposto sobre a terra) para várias regiões.
    - Cada região tem seu próprio algoritmo de cálculo de imposto, e as tarifas específicas para cada região são carregadas de forma remota (o que é caro e demorado).
    - Cada vez que o sistema é invocado, ele recebe um conjunto de regiões para calcular os impostos, mas nem todas as regiões são necessárias de imediato.
    - Para evitar o custo de carregar todas as tarifas de todas as regiões, precisamos carregar as tarifas apenas quando elas forem solicitadas, mantendo uma cópia na memória para reutilização.
    - As regiões são fixas (NORTH, SOUTH, etc.), mas os algoritmos para cada região podem mudar dinamicamente.
 
 - **Solução com Multiton** 
    - Quando o sistema solicitar o cálculo de impostos para uma determinada região (por exemplo, NORTH), o sistema verificará se já existe uma instância associada a essa região.
    - Se a instância não existir, o sistema a criará e carregará as tarifas de forma remota (isso pode ser demorado, mas só acontecerá uma vez).
    - Se a instância já existir, o sistema reutilizará a instância já carregada, evitando o custo de reprocessamento.
    - Isso garante que para cada região (NORTH, SOUTH, etc.), haverá apenas uma única instância em memória, e o acesso será rápido para as chamadas subsequentes.


#### Diagrama UML:

## Motivação

```java
public class Pessoa{
    private String nome;
}
```


```mermaid
---
title: Multiton Tax Calculation
---
classDiagram
    class TaxCalculation {
        <<interface>>
        + float calculateSalesTax(SaleData data)
        + float calculateLandTax(LandData data)
    }

    class TaxRegion {
        <<enumeration>>
        + NORTH
        + NORTH_EAST
        + SOUTH
        + EAST
        + WEST
        + CENTRAL
        - Map~TaxRegion, TaxCalculation~ instances
        - Map~String, Float~ regionalData
        + synchronized static TaxCalculation getInstance(TaxRegion region)
        - TaxCalculation loadRegionalDataFromRemoteServer()
        + float calculateSalesTax(SaleData data)
        + float calculateLandTax(LandData data)
    }

    class SaleData {
        - float saleAmount
        + SaleData(float saleAmount)
        + float getSaleAmount()
    }

    class LandData {
        - float landValue
        + LandData(float landValue)
        + float getLandValue()
    }

    %% Relacionamentos
    TaxRegion o-- "1..*" TaxCalculation : instances
    TaxRegion ..> "1..*" Map : regionalData

    TaxRegion --> TaxCalculation : implements
    TaxCalculation <|.. TaxRegion

    SaleData --> TaxRegion : usado por
    LandData --> TaxRegion : usado por

```
#### Diagrama de fluxo 


```mermaid
sequenceDiagram
    participant Cliente
    participant TaxRegion
    participant ServidorRemoto
    participant SaleData
    
    Cliente->>+TaxRegion: getInstance(NORTH)
    alt Se a instância de NORTH não existir
        TaxRegion->>+ServidorRemoto: loadRegionalDataFromRemoteServer()
        ServidorRemoto-->>-TaxRegion: Dados Regionais (salesTaxRate, landTaxRate)
        TaxRegion->>TaxRegion: Armazena instância no mapa "instances"
    end
    Cliente->>+SaleData: new SaleData(saleAmount)
    Cliente->>TaxRegion: calculateSalesTax(SaleData)
    TaxRegion->>TaxRegion: Obtém "salesTaxRate" de regionalData
    TaxRegion-->>Cliente: Valor do imposto de vendas
```

#### Interface
```Java
public interface TaxCalculation {
    float calculateSalesTax(SaleData data);
    float calculateLandTax(LandData data);
}
```
#### Enum
```Java
import java.util.HashMap;
import java.util.Map;

public enum TaxRegion implements TaxCalculation {
    NORTH, 
    NORTH_EAST, 
    SOUTH, 
    EAST, 
    WEST, 
    CENTRAL;

    // Mapa do Multiton para armazenar uma instância única de cada região
    private static final Map<TaxRegion, TaxCalculation> instances = new HashMap<>();

    // Dados de tarifas específicas de cada região
    private Map<String, Float> regionalData;

    // Método para obter a instância (Multiton)
    public static TaxCalculation getInstance(TaxRegion region) {
        if (!instances.containsKey(region)) {
            synchronized (TaxRegion.class) {
                if (!instances.containsKey(region)) {
                    TaxCalculation instance = region.loadRegionalDataFromRemoteServer();
                    instances.put(region, instance);
                }
            }
        }
        return instances.get(region);
    }

    // Simula o carregamento de dados regionais do servidor remoto
    private TaxCalculation loadRegionalDataFromRemoteServer() {
        System.out.println("Carregando dados para a região: " + this.name());
        this.regionalData = new HashMap<>();
        this.regionalData.put("salesTaxRate", (float) Math.random() * 10); // Simula a taxa de imposto de vendas
        this.regionalData.put("landTaxRate", (float) Math.random() * 20);  // Simula a taxa de imposto de terras
        return this;
    }

    // Implementações de cálculos de impostos (Exemplos simplificados)
    @Override
    public float calculateSalesTax(SaleData data) {
        float rate = regionalData.getOrDefault("salesTaxRate", 0f);
        return data.getSaleAmount() * rate / 100; // Cálculo do imposto de vendas
    }

    @Override
    public float calculateLandTax(LandData data) {
        float rate = regionalData.getOrDefault("landTaxRate", 0f);
        return data.getLandValue() * rate / 100; // Cálculo do imposto sobre terras
    }
}

```
#### Camada de dados 
```java
public class SaleData {
    private float saleAmount;

    public SaleData(float saleAmount) {
        this.saleAmount = saleAmount;
    }

    public float getSaleAmount() {
        return saleAmount;
    }
}

public class LandData {
    private float landValue;

    public LandData(float landValue) {
        this.landValue = landValue;
    }

    public float getLandValue() {
        return landValue;
    }
}
```
#### Exemplo de uso Multiton
```Java 
public class Main {
    public static void main(String[] args) {
        
        // Solicitar cálculos para duas regiões: NORTH e SOUTH
        TaxCalculation northRegion = TaxRegion.getInstance(TaxRegion.NORTH);
        TaxCalculation southRegion = TaxRegion.getInstance(TaxRegion.SOUTH);

        // Calcular imposto de vendas em ambas as regiões
        SaleData sale = new SaleData(1000f); // Venda de 1000 unidades monetárias
        System.out.println("Imposto de vendas na região NORTH: " + northRegion.calculateSalesTax(sale));
        System.out.println("Imposto de vendas na região SOUTH: " + southRegion.calculateSalesTax(sale));

        // Solicitar novamente a instância da região NORTH (não deve recarregar os dados)
        TaxCalculation northAgain = TaxRegion.getInstance(TaxRegion.NORTH);
        System.out.println("Reutilizando a instância de NORTH (imposto de vendas): " + northAgain.calculateSalesTax(sale));
    }
}
```

## Usos Conhecidos

O padrão **Singleton** é amplamente utilizado para garantir que apenas uma instância de uma classe exista durante o ciclo de vida do programa, sendo uma solução eficiente para o gerenciamento de recursos globais. Um exemplo clássico é encontrado em **Smalltalk-80**, onde o conjunto de mudanças no código é gerenciado pela única instância `ChangeSet current`. Outro exemplo sutil é o relacionamento entre classes e suas metaclasses: cada metaclasse possui uma única instância, registrada e acompanhada de maneira centralizada, assegurando que outras não sejam criadas.

No toolkit para construção de interfaces de usuário **InterViews**, o Singleton é usado para acessar instâncias únicas das classes `Session` e `WidgetKit`. A classe `Session` gerencia o ciclo de eventos principais da aplicação, armazena as preferências de estilo do usuário e administra conexões com dispositivos físicos de display. Já `WidgetKit` atua como uma **Abstract Factory**, definindo widgets específicos de interação. A seleção da subclasse de `WidgetKit` a ser instanciada depende de uma variável de ambiente definida por `Session`, que também configura a instância de `Session` com base no suporte a displays monocromáticos ou coloridos.

O padrão Singleton também é amplamente utilizado em frameworks modernos. Por exemplo, o **Spring Framework** utiliza Singletons para instanciar e gerenciar *beans* que armazenam configurações globais. Em sistemas de logging, bibliotecas como **Log4j** e **SLF4J** implementam Singletons para evitar a criação de múltiplos loggers, assegurando uma escrita de logs sincronizada. Frameworks como o **Hibernate** empregam esse padrão para gerenciar conexões a bancos de dados, otimizando recursos e prevenindo conflitos. Em cenários de hardware, drivers e interfaces de dispositivos frequentemente utilizam Singletons para controlar o acesso a recursos limitados. Já nos motores de jogos, como o **Unity**, o Singleton é usado para gerenciar sistemas centrais, como o loop principal do jogo ou a troca de cenas.

Por outro lado, o padrão **Multiton** amplia o conceito do Singleton, permitindo múltiplas instâncias de uma classe, controladas por uma chave única. Esse padrão é especialmente útil em sistemas que precisam gerenciar diferentes contextos. Um exemplo é o **Apache Kafka**, que utiliza Multitons para gerenciar conexões a múltiplos *brokers*, onde cada instância é identificada por uma chave distinta. Frameworks de cache como o **Ehcache** e o **Spring Cache** implementam Multitons para organizar e acessar dados temporários com eficiência. Em servidores web, como o **Tomcat** e o **Jetty**, o Multiton gerencia sessões de usuários, criando instâncias únicas para cada sessão baseada no ID correspondente. No desenvolvimento de jogos, motores como o **Unreal Engine** usam Multitons para gerenciar recursos por regiões ou zonas, garantindo controle e eficiência.

## Padrões Relacionados

- **Factory Method**: O Singleton pode ser usado em conjunto com o Factory Method para fornecer instâncias de uma classe de forma controlada.
- **Abstract Factory**: O Multiton pode ser usado em uma implementação de Abstract Factory para controlar instâncias de diferentes tipos de objetos.

## Referências

- GAMMA, Erich. et al. Padrões de projetos: Soluções reutilizáveis de software orientados a objetos Bookman editora, 2009.

- K19. Design Patterns em Java. 2012
