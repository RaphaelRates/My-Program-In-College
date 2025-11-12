## 📋 Visão Geral

A Programação Orientada a Objetos é um paradigma fundamental que revolucionou o desenvolvimento de software. Java é a linguagem ideal para aprender POO devido à sua natureza puramente orientada a objetos e sintaxe clara.

---

## 🏛️ Os Quatro Pilares da POO

### [[POO - Pilar 1 - Encapsulamento]]

**Definição:** Ocultar detalhes internos e expor apenas interfaces necessárias

**Conceitos Chave:**

- [[Java - Modificadores de Acesso]] (`private`, `protected`, `public`)
- [[Java - Getters e Setters]]
- [[Java - Validação de Dados]]
- [[Java - Imutabilidade]]

**Benefícios:**

- ✅ Segurança dos dados
- ✅ Manutenibilidade
- ✅ Flexibilidade de implementação
- ✅ Redução de acoplamento

```java
public class ContaBancaria {
    private double saldo;  // Encapsulado
    private String titular;
    
    // Getter controlado
    public double getSaldo() {
        return saldo;
    }
    
    // Setter com validação
    public void depositar(double valor) {
        if (valor > 0) {
            this.saldo += valor;
        }
    }
}
```

### [[POO - Pilar 2 - Herança]]

**Definição:** Mecanismo para reutilizar código através de relacionamentos "é-um"

**Conceitos Chave:**

- [[Java - Palavra-chave extends]]
- [[Java - Sobrescrita de Métodos]] (`@Override`)
- [[Java - Super e This]]
- [[Java - Hierarquias de Classes]]
- [[Java - Classe Object]]

**Vantagens:**

- ✅ Reutilização de código
- ✅ Organização hierárquica
- ✅ Especialização de comportamento
- ✅ Polimorfismo

```java
// Classe base
public class Veiculo {
    protected String marca;
    protected int ano;
    
    public void acelerar() {
        System.out.println("Acelerando...");
    }
}

// Especialização
public class Carro extends Veiculo {
    private int numeroPortas;
    
    @Override
    public void acelerar() {
        System.out.println("Carro acelerando com motor...");
    }
}
```

### [[POO - Pilar 3 - Polimorfismo]]

**Definição:** Capacidade de objetos diferentes responderem à mesma interface

**Tipos de Polimorfismo:**

- [[Java - Polimorfismo de Sobrecarga]] (Overloading)
- [[Java - Polimorfismo de Sobrescrita]] (Overriding)
- [[Java - Polimorfismo de Interface]]
- [[Java - Late Binding (Ligação Dinâmica)]]

**Implementação:**

```java
// Polimorfismo em ação
public void processar(List<Veiculo> veiculos) {
    for (Veiculo v : veiculos) {
        v.acelerar(); // Cada tipo executará sua versão
    }
}
```

### [[POO - Pilar 4 - Abstração]]

**Definição:** Ocultar complexidade e fornecer interfaces simplificadas

**Mecanismos em Java:**

- [[Java - Classes Abstratas]]
- [[Java - Interfaces]]
- [[Java - Métodos Abstratos]]
- [[Java - Default Methods]]

**Comparação:**

|Aspecto|**Classes Abstratas**|**Interfaces**|
|---|---|---|
|**Herança**|Simples (`extends`)|Múltipla (`implements`)|
|**Métodos**|Concretos + Abstratos|Abstratos + Default (Java 8+)|
|**Atributos**|Todos os tipos|`public static final` apenas|
|**Construtor**|Sim|Não|
|**Uso**|Relação "é-um" forte|Contrato/"pode-fazer"|

```java
// Interface - Contrato
public interface Voador {
    void voar(); // Implicitamente public abstract
    
    default void pousar() { // Java 8+
        System.out.println("Pousando...");
    }
}

// Classe abstrata - Base parcial
public abstract class Animal {
    protected String nome;
    
    public abstract void emitirSom(); // Deve ser implementado
    
    public void dormir() { // Implementação padrão
        System.out.println(nome + " está dormindo");
    }
}
```

---

## 🔧 Fundamentos da Linguagem Java

### [[Java - Ambiente de Desenvolvimento]]

**Componentes Essenciais:**

- **[[JDK]]** (Java Development Kit) - Compilador + Bibliotecas
- **[[JRE]]** (Java Runtime Environment) - Execução
- **[[JVM]]** (Java Virtual Machine) - Máquina virtual

**IDEs Recomendadas:**

- [[IDE - IntelliJ IDEA]] ⭐ Mais popular
- [[IDE - Eclipse]] - Open source clássico
- [[IDE - NetBeans]] - Oracle oficial
- [[IDE - Visual Studio Code]] - Leve + extensões

### [[Java - Sintaxe Básica]]

#### **Estrutura de uma Classe:**

```java
package com.exemplo.projeto;

import java.util.List;
import java.util.ArrayList;

/**
 * Documentação Javadoc
 * @author Seu Nome
 * @version 1.0
 */
public class MinhaClasse {
    // Atributos (fields)
    private String nome;
    private static int contador = 0;
    
    // Construtor
    public MinhaClasse(String nome) {
        this.nome = nome;
        contador++;
    }
    
    // Métodos
    public String getNome() {
        return nome;
    }
    
    public static int getContador() {
        return contador;
    }
}
```

#### **Tipos de Dados:**

|**Categoria**|**Tipos Primitivos**|**Classes Wrapper**|**Características**|
|---|---|---|---|
|**Inteiros**|`byte`, `short`, `int`, `long`|`Byte`, `Short`, `Integer`, `Long`|Números inteiros|
|**Ponto Flutuante**|`float`, `double`|`Float`, `Double`|Números decimais|
|**Caractere**|`char`|`Character`|Unicode 16-bit|
|**Lógico**|`boolean`|`Boolean`|true/false|
|**Referência**|-|`String`, `Object`, Arrays|Objetos|

### [[Java - Construtores e Sobrecarga]]

```java
public class Pessoa {
    private String nome;
    private int idade;
    private String email;
    
    // Construtor padrão
    public Pessoa() {
        this("Sem nome", 0, "sem@email.com");
    }
    
    // Construtor com nome
    public Pessoa(String nome) {
        this(nome, 0, "sem@email.com");
    }
    
    // Construtor completo
    public Pessoa(String nome, int idade, String email) {
        this.nome = nome;
        this.idade = idade;
        this.email = email;
    }
    
    // Sobrecarga de métodos
    public void apresentar() {
        System.out.println("Olá, eu sou " + nome);
    }
    
    public void apresentar(String saudacao) {
        System.out.println(saudacao + ", eu sou " + nome);
    }
}
```

---

## 📦 Estruturas de Dados Java

### [[Java - Collections Framework]]

**Hierarquia Principal:**

```
Collection
├── List (ordenado, permite duplicatas)
│   ├── ArrayList (array dinâmico)
│   ├── LinkedList (lista ligada)
│   └── Vector (thread-safe)
├── Set (sem duplicatas)
│   ├── HashSet (hash table)
│   ├── LinkedHashSet (ordem de inserção)
│   └── TreeSet (ordenado)
└── Queue (FIFO)
    ├── PriorityQueue (heap)
    └── LinkedList

Map (chave-valor)
├── HashMap (hash table)
├── LinkedHashMap (ordem de inserção)
├── TreeMap (ordenado por chave)
└── Hashtable (thread-safe)
```

#### **Comparação de Performance:**

| **Operação**          | **[[ArrayList]]** | **[[LinkedList]]** | **[[HashSet]]** | **[[TreeSet]]** |
| --------------------- | ----------------- | ------------------ | --------------- | --------------- |
| **Inserção**          | O(1) amortized    | O(1)               | O(1)            | O(log n)        |
| **Busca**             | O(n)              | O(n)               | O(1)            | O(log n)        |
| **Acesso por índice** | O(1)              | O(n)               | N/A             | N/A             |
| **Remoção**           | O(n)              | O(1)               | O(1)            | O(log n)        |

### [[Java - Generics (Tipos Genéricos)]]

```java
// Classe genérica
public class Caixa<T> {
    private T conteudo;
    
    public void guardar(T item) {
        this.conteudo = item;
    }
    
    public T retirar() {
        return conteudo;
    }
}

// Método genérico
public static <T extends Comparable<T>> T maximo(T a, T b) {
    return a.compareTo(b) > 0 ? a : b;
}

// Wildcards
public void processar(List<? extends Number> numeros) {
    // Pode ler, mas não modificar
}
```

---

## 🎨 Padrões de Projeto (Design Patterns)

### Padrões Criacionais

#### **[[Singleton]]**

**Problema:** Garantir apenas uma instância de uma classe

```java
public class Singleton {
    private static volatile Singleton instance;
    private static final Object lock = new Object();
    
    private Singleton() {
        // Construtor privado
    }
    
    public static Singleton getInstance() {
        if (instance == null) {
            synchronized (lock) {
                if (instance == null) {
                    instance = new Singleton();
                }
            }
        }
        return instance;
    }
}
```

#### **[[Factory Method]]**

**Problema:** Criar objetos sem especificar classes concretas

```java
// Interface do produto
public interface Veiculo {
    void acelerar();
}

// Factory abstrata
public abstract class VeiculoFactory {
    public abstract Veiculo criarVeiculo();
    
    public void processar() {
        Veiculo veiculo = criarVeiculo();
        veiculo.acelerar();
    }
}

// Factory concreta
public class CarroFactory extends VeiculoFactory {
    @Override
    public Veiculo criarVeiculo() {
        return new Carro();
    }
}
```

#### **[[Builder]]**

**Problema:** Construir objetos complexos passo a passo

```java
public class Pessoa {
    private String nome;
    private int idade;
    private String endereco;
    private String telefone;
    
    private Pessoa(Builder builder) {
        this.nome = builder.nome;
        this.idade = builder.idade;
        this.endereco = builder.endereco;
        this.telefone = builder.telefone;
    }
    
    public static class Builder {
        private String nome;
        private int idade;
        private String endereco;
        private String telefone;
        
        public Builder nome(String nome) {
            this.nome = nome;
            return this;
        }
        
        public Builder idade(int idade) {
            this.idade = idade;
            return this;
        }
        
        public Pessoa build() {
            return new Pessoa(this);
        }
    }
}

// Uso
Pessoa pessoa = new Pessoa.Builder()
    .nome("João")
    .idade(30)
    .build();
```

### Padrões Estruturais

#### **[[Adapter]]**

**Problema:** Fazer classes incompatíveis trabalharem juntas

```java
// Interface esperada
public interface MediaPlayer {
    void play(String filename);
}

// Classe incompatível
public class AdvancedMediaPlayer {
    public void playVlc(String filename) { /* ... */ }
    public void playMp4(String filename) { /* ... */ }
}

// Adapter
public class MediaAdapter implements MediaPlayer {
    private AdvancedMediaPlayer advancedPlayer;
    
    public MediaAdapter(String audioType) {
        if (audioType.equalsIgnoreCase("vlc") || 
            audioType.equalsIgnoreCase("mp4")) {
            advancedPlayer = new AdvancedMediaPlayer();
        }
    }
    
    @Override
    public void play(String filename) {
        if (filename.endsWith(".vlc")) {
            advancedPlayer.playVlc(filename);
        } else if (filename.endsWith(".mp4")) {
            advancedPlayer.playMp4(filename);
        }
    }
}
```

### Padrões Comportamentais

#### **[[Observer]]**

**Problema:** Notificar múltiplos objetos sobre mudanças de estado

```java
import java.util.*;

// Subject
public class NewsAgency {
    private String news;
    private List<Observer> channels = new ArrayList<>();
    
    public void addObserver(Observer channel) {
        channels.add(channel);
    }
    
    public void removeObserver(Observer channel) {
        channels.remove(channel);
    }
    
    public void setNews(String news) {
        this.news = news;
        notifyAllObservers();
    }
    
    private void notifyAllObservers() {
        for (Observer channel : channels) {
            channel.update(news);
        }
    }
}

// Observer
public interface Observer {
    void update(String news);
}

// Concrete Observer
public class NewsChannel implements Observer {
    private String news;
    
    @Override
    public void update(String news) {
        this.news = news;
        System.out.println("News received: " + news);
    }
}
```

#### **[[Strategy]]**

**Problema:** Escolher algoritmos dinamicamente

```java
// Strategy interface
public interface PaymentStrategy {
    void pay(double amount);
}

// Concrete strategies
public class CreditCardPayment implements PaymentStrategy {
    private String cardNumber;
    
    public CreditCardPayment(String cardNumber) {
        this.cardNumber = cardNumber;
    }
    
    @Override
    public void pay(double amount) {
        System.out.println("Paid $" + amount + " with credit card " + cardNumber);
    }
}

public class PayPalPayment implements PaymentStrategy {
    private String email;
    
    public PayPalPayment(String email) {
        this.email = email;
    }
    
    @Override
    public void pay(double amount) {
        System.out.println("Paid $" + amount + " with PayPal " + email);
    }
}

// Context
public class ShoppingCart {
    private PaymentStrategy paymentStrategy;
    
    public void setPaymentStrategy(PaymentStrategy strategy) {
        this.paymentStrategy = strategy;
    }
    
    public void checkout(double amount) {
        paymentStrategy.pay(amount);
    }
}
```

---

## 🔧 Tratamento de Exceções

### [[Java - Hierarquia de Exceções]]

```
Throwable
├── Error (sistema - não deve ser capturado)
│   ├── OutOfMemoryError
│   └── StackOverflowError
└── Exception
    ├── Checked Exceptions (obrigatório tratar)
    │   ├── IOException
    │   ├── SQLException
    │   └── ClassNotFoundException
    └── RuntimeException (Unchecked)
        ├── NullPointerException
        ├── ArrayIndexOutOfBoundsException
        └── IllegalArgumentException
```

### [[Java - Try-Catch-Finally]]

```java
public class ExceptionExample {
    
    public void exemploCompleto() {
        FileInputStream file = null;
        try {
            file = new FileInputStream("arquivo.txt");
            // Operações que podem falhar
            int data = file.read();
            
        } catch (FileNotFoundException e) {
            System.err.println("Arquivo não encontrado: " + e.getMessage());
        } catch (IOException e) {
            System.err.println("Erro de E/S: " + e.getMessage());
        } catch (Exception e) {
            System.err.println("Erro geral: " + e.getMessage());
        } finally {
            // Sempre executa
            if (file != null) {
                try {
                    file.close();
                } catch (IOException e) {
                    System.err.println("Erro ao fechar arquivo");
                }
            }
        }
    }
    
    // Try-with-resources (Java 7+)
    public void exemploModerno() {
        try (FileInputStream file = new FileInputStream("arquivo.txt");
             BufferedReader reader = new BufferedReader(
                 new InputStreamReader(file))) {
            
            String line = reader.readLine();
            // Recursos fechados automaticamente
            
        } catch (IOException e) {
            System.err.println("Erro: " + e.getMessage());
        }
    }
}
```

### [[Java - Exceções Customizadas]]

```java
// Checked Exception customizada
public class SaldoInsuficienteException extends Exception {
    private double saldoAtual;
    private double valorSolicitado;
    
    public SaldoInsuficienteException(double saldoAtual, double valorSolicitado) {
        super("Saldo insuficiente. Atual: " + saldoAtual + 
              ", Solicitado: " + valorSolicitado);
        this.saldoAtual = saldoAtual;
        this.valorSolicitado = valorSolicitado;
    }
    
    // Getters...
}

// Uso
public void sacar(double valor) throws SaldoInsuficienteException {
    if (valor > saldo) {
        throw new SaldoInsuficienteException(saldo, valor);
    }
    saldo -= valor;
}
```

---

## 🧵 Programação Concorrente (Threads)

### [[Java - Threads Básicas]]

```java
// Implementando Runnable (recomendado)
public class MinhaTask implements Runnable {
    private String nome;
    
    public MinhaTask(String nome) {
        this.nome = nome;
    }
    
    @Override
    public void run() {
        for (int i = 0; i < 5; i++) {
            System.out.println(nome + " executando: " + i);
            try {
                Thread.sleep(1000); // 1 segundo
            } catch (InterruptedException e) {
                Thread.currentThread().interrupt();
                break;
            }
        }
    }
}

// Uso
public class ThreadExample {
    public static void main(String[] args) {
        Thread t1 = new Thread(new MinhaTask("Thread-1"));
        Thread t2 = new Thread(new MinhaTask("Thread-2"));
        
        t1.start();
        t2.start();
        
        try {
            t1.join(); // Espera t1 terminar
            t2.join(); // Espera t2 terminar
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
}
```

### [[Java - Sincronização]]

```java
public class ContadorSincronizado {
    private int contador = 0;
    private final Object lock = new Object();
    
    // Método sincronizado
    public synchronized void incrementar() {
        contador++;
    }
    
    // Bloco sincronizado
    public void decrementar() {
        synchronized (lock) {
            contador--;
        }
    }
    
    // Volatile para visibilidade
    private volatile boolean ativo = true;
    
    public void parar() {
        ativo = false;
    }
}
```

---

## 📊 Projeto Prático: Sistema Bancário

### Projeto - Sistema Bancário POO

**Requisitos:**

- Diferentes tipos de conta (Corrente, Poupança)
- Operações (depósito, saque, transferência)
- Histórico de transações
- Clientes com múltiplas contas
- Relatórios

**Estrutura de Classes:**

```java
// Classe abstrata base
public abstract class Conta {
    protected String numero;
    protected double saldo;
    protected Cliente titular;
    protected List<Transacao> historico;
    
    public abstract boolean sacar(double valor);
    public abstract void aplicarJuros();
    
    public void depositar(double valor) {
        if (valor > 0) {
            saldo += valor;
            historico.add(new Transacao("DEPÓSITO", valor));
        }
    }
}

// Implementações concretas
public class ContaCorrente extends Conta {
    private double limiteChequeEspecial;
    
    @Override
    public boolean sacar(double valor) {
        if (valor > 0 && (saldo + limiteChequeEspecial) >= valor) {
            saldo -= valor;
            historico.add(new Transacao("SAQUE", valor));
            return true;
        }
        return false;
    }
    
    @Override
    public void aplicarJuros() {
        // Juros negativos se estiver no vermelho
        if (saldo < 0) {
            saldo *= 1.05; // 5% de juros
        }
    }
}

public class ContaPoupanca extends Conta {
    private double taxaRendimento;
    
    @Override
    public boolean sacar(double valor) {
        if (valor > 0 && saldo >= valor) {
            saldo -= valor;
            historico.add(new Transacao("SAQUE", valor));
            return true;
        }
        return false;
    }
    
    @Override
    public void aplicarJuros() {
        saldo *= (1 + taxaRendimento);
    }
}

// Classe de domínio
public class Cliente {
    private String cpf;
    private String nome;
    private String email;
    private List<Conta> contas;
    
    public void adicionarConta(Conta conta) {
        contas.add(conta);
    }
    
    public double getSaldoTotal() {
        return contas.stream()
                    .mapToDouble(Conta::getSaldo)
                    .sum();
    }
}

// Classe de serviço
public class BancoService {
    private Map<String, Cliente> clientes;
    private Map<String, Conta> contas;
    
    public void transferir(String contaOrigem, String contaDestino, double valor)
            throws ContaNotFoundException, SaldoInsuficienteException {
        
        Conta origem = contas.get(contaOrigem);
        Conta destino = contas.get(contaDestino);
        
        if (origem == null || destino == null) {
            throw new ContaNotFoundException("Conta não encontrada");
        }
        
        if (!origem.sacar(valor)) {
            throw new SaldoInsuficienteException("Saldo insuficiente");
        }
        
        destino.depositar(valor);
    }
}
```

---

## 🎯 Projetos Práticos por Complexidade

### **Nível Básico**

- [ ] Projeto - Calculadora OO
- [ ] Projeto - Sistema de Biblioteca Simples
- [ ] Projeto - Jogo da Velha OO
- [ ] Projeto - Controle de Estoque

### **Nível Intermediário**

- [ ] Projeto - Sistema Bancário Completo
- [ ] Projeto - Loja Virtual com Padrões
- [ ] Projeto - Sistema de Reservas
- [ ] Projeto - Editor de Texto com Swing

### **Nível Avançado**

- [ ] Projeto - Framework MVC Próprio
- [ ] Projeto - Sistema Distribuído com RMI
- [ ] Projeto - Game Engine 2D
- [ ] Projeto - Sistema de Workflow

---

## 📚 Recursos Complementares

### **Livros Essenciais**

- Livro - Effective Java (Joshua Bloch) ⭐ Obrigatório
- Livro - Head First Design Patterns
- Livro - Clean Code (Robert Martin)
- Livro - Java: The Complete Reference

### **Documentação e APIs**

- Oracle Java Documentation
- Java API Specification
- OpenJDK Documentation

### **Ferramentas de Build**

- Maven - Gerenciamento de dependências
- Gradle - Build automation
- Ant - Build tool clássico

### **Testing Frameworks**

- JUnit 5 - Unit testing
- Mockito - Mocking framework
- TestNG - Testing framework
- AssertJ - Fluent assertions

---

## 🔗 Conexões Interdisciplinares

### **Pré-requisitos**

- [[Introdução à Programação]] → Base algorítmica
- [[Matemática Discreta]] → Lógica e estruturas

### **Disciplinas Relacionadas**

- [[Estruturas de Dados]] → Implementação OO
- [[Engenharia de Software]] → Metodologias OO
- [[Banco de Dados]] → Mapeamento objeto-relacional
- [[Interface Humano-Computador]] → GUIs com Swing/JavaFX
