# ✨ Design Pattern: Um Breve Resumo

Os **Design Patterns** (Padrões de Projeto) são soluções reutilizáveis para problemas comuns no desenvolvimento de software. Eles são baseados em boas práticas e servem como guias para projetar sistemas mais robustos, flexíveis e fáceis de manter.

---

## 📝 Classificação
Os padrões de projeto são divididos em três categorias principais:

1. **Criacionais** 🏗️  
   Focados em como os objetos são criados, garantindo flexibilidade e reutilização.  
   Exemplos: Singleton, Factory, Builder.

2. **Estruturais** 🧱  
   Tratam da composição de classes e objetos para formar estruturas maiores.  
   Exemplos: Adapter, Composite, Decorator.

3. **Comportamentais** 🔄  
   Enfocam a interação entre objetos e a forma como responsabilidades são distribuídas.  
   Exemplos: Observer, Strategy, State.

---

## 🎯 Benefícios
- **Reutilização de Código:** Soluções testadas e aprovadas.  
- **Manutenção Facilitada:** Estruturas claras e organizadas.  
- **Colaboração Eficiente:** Termos padronizados para comunicação entre desenvolvedores.

---

## 🌟 Por Que Usar?
Os Design Patterns ajudam a resolver problemas recorrentes de forma eficiente, promovendo qualidade no design do software e acelerando o desenvolvimento com soluções já validadas.




# 🔄 Padrão State: Uma Explicação Detalhada

## 📝 Definição
O padrão State é um padrão de projeto comportamental que permite que um objeto altere seu comportamento com base no seu estado interno. A principal ideia é encapsular os estados possíveis de um objeto em diferentes classes, delegando a lógica de comportamento para essas classes específicas. Como resultado, o objeto principal aparenta mudar sua classe dinamicamente ao longo do tempo, mas, na realidade, ele apenas alterna entre diferentes estados.

## 🎯 Objetivo Principal
O objetivo do padrão State é fornecer uma maneira organizada de representar os estados de um objeto, removendo condicionais extensas e facilitando a manutenção e extensão do código. Ele promove um design mais limpo e modular ao delegar responsabilidades para objetos especializados em um determinado estado.

---

## 🛠 Como Funciona
### Contexto (Context)
O objeto principal que possui o estado atual e delega as ações para o estado associado.

### Estado (State)
Uma interface que define o comportamento esperado para cada estado.

### Estados Concretos (Concrete States)
Implementações específicas do estado que alteram o comportamento do objeto Context.

### Transição de Estados
O objeto Context alterna entre os estados concretos conforme necessário, geralmente chamando um método do estado atual que retorna o próximo estado.

---

## 🔄 Exemplo Prático: Máquina de Venda Automática
Imagine uma máquina de venda automática que possui três estados: **Esperando Inserção de Moeda**, **Selecionando Produto**, e **Entregando Produto**.

- **Contexto**: A máquina de venda automática.
- **Estados Concretos**:
  - *Esperando Inserção de Moeda*: Aguarda o cliente inserir dinheiro.
  - *Selecionando Produto*: Permite a seleção do produto após a inserção da moeda.
  - *Entregando Produto*: Dispensa o produto e retorna ao estado inicial.

---

## 🖥️ Código Representativo em TypeScript

### Sem o Padrão State

```typescript
class MaquinaVenda {
  private estado: string = "esperandoMoeda";

  inserirMoeda() {
    if (this.estado === "esperandoMoeda") {
      console.log("Moeda inserida. Selecione um produto.");
      this.estado = "selecionandoProduto";
    } else {
      console.log("Operação inválida no estado atual.");
    }
  }

  selecionarProduto() {
    if (this.estado === "selecionandoProduto") {
      console.log("Produto selecionado. Entregando produto...");
      this.estado = "entregandoProduto";
    } else {
      console.log("Operação inválida no estado atual.");
    }
  }

  entregarProduto() {
    if (this.estado === "entregandoProduto") {
      console.log("Produto entregue. Voltando ao estado inicial.");
      this.estado = "esperandoMoeda";
    } else {
      console.log("Operação inválida no estado atual.");
    }
  }
}

// Uso
const maquina = new MaquinaVenda();
maquina.inserirMoeda(); // Moeda inserida. Selecione um produto.
maquina.selecionarProduto(); // Produto selecionado. Entregando produto...
maquina.entregarProduto(); // Produto entregue. Voltando ao estado inicial.
```


### Com o Padrão State

```typescript
// Interface de Estado
interface Estado {
  inserirMoeda(): void;
  selecionarProduto(): void;
  entregarProduto(): void;
}

// Classe Contexto
class MaquinaVenda {
  private estadoAtual: Estado;

  constructor(estadoInicial: Estado) {
    this.estadoAtual = estadoInicial;
  }

  setEstado(estado: Estado) {
    this.estadoAtual = estado;
  }

  inserirMoeda() {
    this.estadoAtual.inserirMoeda();
  }

  selecionarProduto() {
    this.estadoAtual.selecionarProduto();
  }

  entregarProduto() {
    this.estadoAtual.entregarProduto();
  }
}

// Estados Concretos
class EstadoEsperandoMoeda implements Estado {
  constructor(private contexto: MaquinaVenda) {}

  inserirMoeda(): void {
    console.log("Moeda inserida. Selecione um produto.");
    this.contexto.setEstado(new EstadoSelecionandoProduto(this.contexto));
  }

  selecionarProduto(): void {
    console.log("Insira uma moeda primeiro.");
  }

  entregarProduto(): void {
    console.log("Insira uma moeda primeiro.");
  }
}

class EstadoSelecionandoProduto implements Estado {
  constructor(private contexto: MaquinaVenda) {}

  inserirMoeda(): void {
    console.log("Moeda já inserida. Selecione um produto.");
  }

  selecionarProduto(): void {
    console.log("Produto selecionado. Entregando produto...");
    this.contexto.setEstado(new EstadoEntregandoProduto(this.contexto));
  }

  entregarProduto(): void {
    console.log("Selecione um produto primeiro.");
  }
}

class EstadoEntregandoProduto implements Estado {
  constructor(private contexto: MaquinaVenda) {}

  inserirMoeda(): void {
    console.log("Espere até o produto ser entregue.");
  }

  selecionarProduto(): void {
    console.log("Espere até o produto ser entregue.");
  }

  entregarProduto(): void {
    console.log("Produto entregue. Voltando ao estado inicial.");
    this.contexto.setEstado(new EstadoEsperandoMoeda(this.contexto));
  }
}

// Uso
const maquina = new MaquinaVenda(new EstadoEsperandoMoeda(new MaquinaVenda(null!)));
maquina.inserirMoeda(); // Moeda inserida. Selecione um produto.
maquina.selecionarProduto(); // Produto selecionado. Entregando produto...
maquina.entregarProduto(); // Produto entregue. Voltando ao estado inicial.

```

## ✅ Vantagens do Padrão State

- **Código Limpo e Modular:** Cada estado possui sua própria classe, facilitando a manutenção e a extensão.
- **Fácil Adição de Novos Estados:** Basta criar uma nova classe de estado e integrá-la ao contexto.
- **Elimina Condicionais Complexas:** Substitui grandes estruturas condicionais por um design baseado em classes.

## ❌ Desvantagens do Padrão State

- **Complexidade Adicional:** Introduz várias classes para representar cada estado, o que pode aumentar a complexidade em sistemas pequenos.
- **Custo de Transições:** Pode ser necessário gerenciar cuidadosamente as transições para evitar inconsistências.


## 🌟 Quando Usar o Padrão State

- Quando um objeto possui múltiplos estados e seu comportamento muda com base no estado atual.
- Para substituir condicionais complexas ou código spaghetti que gerencia estados.
- Em sistemas onde os estados precisam ser adicionados ou modificados frequentemente, como máquinas de estados finitos ou fluxos de trabalho baseados em estados.


# 🔔 Padrão Observer: Uma Explicação Detalhada

## 📝 Definição
O Observer é um padrão de projeto comportamental que estabelece uma relação um-para-muitos entre objetos. Isso significa que, quando um objeto (chamado de Sujeito ou Subject) muda seu estado, todos os objetos dependentes (chamados de Observadores ou Observers) são notificados automaticamente. Este padrão é amplamente usado para implementar sistemas de eventos ou sistemas reativos.

## 🎯 Objetivo Principal
Desacoplar o objeto que gera eventos (o Sujeito) dos objetos que respondem a esses eventos (Observadores), permitindo que ambos evoluam de forma independente. Assim, novas funcionalidades podem ser adicionadas ou alteradas sem modificar diretamente os dois lados da relação.

## 🛠 Como Funciona
- **Sujeito (Subject):**  
  É o objeto principal que mantém uma lista de seus observadores. Quando ocorre uma mudança significativa no estado do sujeito, ele notifica todos os observadores registrados.

- **Observadores (Observers):**  
  São objetos que desejam ser notificados sempre que o estado do sujeito mudar. Cada observador implementa uma interface ou método padrão para receber notificações.

- **Notificação:**  
  Quando o sujeito detecta uma alteração em seu estado, ele chama o método de notificação. Esse método percorre a lista de observadores e os notifica.

## 🔄 Ciclo de Vida do Observer
1. Um observador se registra no sujeito para receber atualizações.
2. O sujeito mantém uma lista de todos os observadores registrados.
3. Quando o estado do sujeito muda, ele dispara uma notificação para todos os observadores.
4. Os observadores atualizam seu estado ou comportamento em resposta à notificação.

## 🖥 Exemplo Prático: Sistema de Notificações
Imagine um sistema onde você tem um aplicativo de clima que deve notificar os usuários sobre mudanças de temperatura.

- **Sujeito:** O sistema de monitoramento de temperatura (o Subject) rastreia as condições climáticas e mantém uma lista de usuários inscritos para notificações.
- **Observadores:** Cada usuário (ou app no dispositivo deles) é um Observer que será notificado sempre que houver uma mudança significativa na temperatura.

### Código Representativo em TypeScript

#### Sem o Padrão Observer:
```typescript
class SistemaClima {
  private temperatura: number = 0;

  setTemperatura(novaTemp: number) {
    this.temperatura = novaTemp;
    console.log(`Temperatura atualizada para ${novaTemp}°C`);
    // Notifica diretamente os dispositivos (baixo acoplamento).
    console.log('Notificando dispositivos...');
  }
}

const sistema = new SistemaClima();
sistema.setTemperatura(25); // Temperatura atualizada para 25°C
```
#### Com o Padrão Observer:

```typescript
interface Observador {
  atualizar(temperatura: number): void;
}

class DispositivoUsuario implements Observador {
  private nome: string;

  constructor(nome: string) {
    this.nome = nome;
  }

  atualizar(temperatura: number): void {
    console.log(`Dispositivo ${this.nome} notificado: Nova temperatura é ${temperatura}°C`);
  }
}

class SistemaClima {
  private temperatura: number = 0;
  private observadores: Observador[] = [];

  adicionarObservador(observador: Observador) {
    this.observadores.push(observador);
  }

  removerObservador(observador: Observador) {
    this.observadores = this.observadores.filter(obs => obs !== observador);
  }

  notificarObservadores() {
    this.observadores.forEach(obs => obs.atualizar(this.temperatura));
  }

  setTemperatura(novaTemp: number) {
    this.temperatura = novaTemp;
    console.log(`Temperatura atualizada para ${novaTemp}°C`);
    this.notificarObservadores();
  }
}

// Uso
const sistema = new SistemaClima();
const dispositivo1 = new DispositivoUsuario('Celular');
const dispositivo2 = new DispositivoUsuario('Tablet');

sistema.adicionarObservador(dispositivo1);
sistema.adicionarObservador(dispositivo2);

sistema.setTemperatura(25);
// Temperatura atualizada para 25°C
// Dispositivo Celular notificado: Nova temperatura é 25°C
// Dispositivo Tablet notificado: Nova temperatura é 25°C

```

## ✅ Vantagens do Padrão Observer

- **Desacoplamento:**O sujeito não precisa saber quais observadores estão registrados, apenas notifica todos eles.
- **Reatividade:**Torna o sistema responsivo a mudanças de estado, ideal para cenários como notificações, atualizações de interface ou eventos em tempo real.
- **Flexibilidade:**Permite adicionar novos observadores sem alterar o código do sujeito.

## ❌ Desvantagens do Padrão Observer

- **Sobrecarga com Muitos Observadores:**Se houver muitos observadores registrados, pode haver uma sobrecarga de notificações e processamento.
- **Dificuldade no Gerenciamento de Ordem:**Não é trivial controlar a ordem em que os observadores recebem notificações.
- **Possíveis Fugas de Memória:**Se os observadores não forem removidos corretamente, podem causar problemas de memória.

## 🌟 Quando Usar o Padrão Observer

- Sistemas de eventos, como interfaces gráficas ou notificações.
- Atualizações automáticas, como feeds de notícias ou dashboards.
- Aplicações distribuídas ou sistemas com componentes altamente desacoplados.


# 🔍 Comparação entre os Padrões State e Observer

Os padrões **State** e **Observer** são frequentemente usados em contextos diferentes, mas ambos ajudam a criar sistemas mais flexíveis e desacoplados. Abaixo está uma análise comparativa detalhada entre os dois padrões.

## 📝 Definição dos Padrões
- **State:**  
  É um padrão comportamental que permite que um objeto altere seu comportamento com base no seu estado interno. O padrão encapsula os estados em diferentes classes, permitindo a troca dinâmica entre esses estados.

- **Observer:**  
  É um padrão comportamental que estabelece uma relação um-para-muitos entre objetos, onde mudanças no estado de um objeto (Subject) são notificadas automaticamente para todos os objetos dependentes (Observers).

---

## 🎯 Propósitos
| Aspecto          | **State**                                                   | **Observer**                                                 |
|-------------------|-------------------------------------------------------------|--------------------------------------------------------------|
| **Objetivo**      | Alterar dinamicamente o comportamento de um objeto conforme seu estado. | Notificar automaticamente mudanças de estado a múltiplos dependentes. |
| **Foco**          | Transições de estado e comportamento do objeto.             | Comunicação e sincronização entre objetos.                   |

---

## 🛠 Como Funcionam
| Aspecto                  | **State**                                                                                  | **Observer**                                                                                  |
|---------------------------|--------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------|
| **Estrutura Principal**   | - Contexto que mantém o estado atual. <br> - Classes de estado que implementam comportamentos específicos. | - Sujeito que notifica observadores. <br> - Observadores que reagem às mudanças do sujeito.   |
| **Interação**             | O contexto delega operações para a classe de estado ativa.                                | O sujeito chama os métodos de atualização de todos os observadores registrados.             |
| **Fluxo de Mudança**      | A troca de estados ocorre internamente, alterando o comportamento do objeto no momento.    | Observadores externos reagem a eventos ou mudanças no estado do sujeito.                    |

---

## 🖥 Exemplos Práticos
| Aspecto                | **State**                                                                             | **Observer**                                                                            |
|-------------------------|---------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------|
| **Caso de Uso**         | Um sistema de pedidos que muda o comportamento dependendo do estado (Novo, Pago, Cancelado). | Um sistema de notificações que alerta vários dispositivos sobre mudanças climáticas.   |
| **Código Representativo** | Classes de estado que implementam comportamentos como `NovoPedido`, `PedidoPago`, etc.      | Observadores que implementam um método `atualizar` e reagem a notificações do sujeito. |

---

## ✅ Vantagens
| Padrão       | **State**                                                                                  | **Observer**                                                                              |
|--------------|--------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------|
| **Vantagens** | - Evita grandes estruturas de controle com múltiplos `if-else`. <br> - Torna fácil adicionar novos estados. | - Facilita a comunicação desacoplada entre objetos. <br> - Permite reatividade eficiente. |

---

## ❌ Desvantagens
| Padrão        | **State**                                                                                     | **Observer**                                                                                  |
|---------------|-----------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------|
| **Desvantagens** | - Pode aumentar a complexidade do sistema com muitas classes de estado.                    | - Pode causar sobrecarga com muitos observadores registrados. <br> - Gerenciar observadores pode ser difícil. |

---

## 🌟 Quando Usar
| Situação                          | **State**                                                                                  | **Observer**                                                                                |
|-----------------------------------|--------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------|
| **Cenário Ideal**                 | - Quando o objeto tem diferentes comportamentos com base no estado atual.                 | - Quando várias partes do sistema precisam reagir a uma mudança de estado de um objeto.    |
| **Exemplo Prático**               | - Máquinas de estado, como um sistema de login ou um fluxo de pedidos.                    | - Sistemas de eventos, como notificações ou atualizações automáticas de interface gráfica. |

---

## 🔗 Relação Entre Eles
Embora **State** e **Observer** sejam padrões distintos, eles podem ser usados juntos em alguns casos:
- Um sistema pode usar o **Observer** para notificar múltiplos objetos sobre a mudança de estado em um contexto gerenciado pelo **State**.
- Por exemplo, um aplicativo que altera seu comportamento com base no estado atual (State) pode notificar outros módulos da aplicação sobre essas mudanças (Observer).

Ambos os padrões desempenham papéis importantes no design de software, promovendo flexibilidade e desacoplamento!
