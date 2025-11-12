> [!note] 
> ## Introdução Simples
>
> Recursão é quando uma **função chama a si mesma** para resolver um problema. É como aqueles bonecos russos que contêm versões menores de si mesmos - cada chamada resolve uma parte do problema até chegar a um caso simples.

```c
#include <stdio.h>

// Função recursiva mais simples: contagem regressiva
void contagem_regressiva(int n) {
    if (n == 0) {  // CASO BASE - para a recursão
        printf("Fogo!\n");
        return;
    }
    
    printf("%d...\n", n);
    contagem_regressiva(n - 1);  // CHAMADA RECURSIVA
}

int main() {
    printf("Contagem regressiva:\n");
    contagem_regressiva(5);
    return 0;
}
```

**Saída:**
```
Contagem regressiva:
5...
4...
3...
2...
1...
Fogo!
```

> [!note]
> ## Os Dois Pilares da Recursão
> 
> 1. **Caso Base**: Condição que para a recursão (evita loop infinito)
> 2. **Passo Recursivo**: Chamada à própria função com problema menor
> 
> ```c
> void funcao_recursiva(problema) {
>     if (problema é simples) {  // CASO BASE
>         resolve diretamente;
>         return;
>     }
>     
>     // PASSO RECURSIVO
>     funcao_recursiva(problema_menor);
> }
> ```

---
## 1. Anatomia de uma Função Recursiva

### Estrutura Completa

```c
#include <stdio.h>

// Exemplo clássico: Fatorial
// n! = n × (n-1) × (n-2) × ... × 1
unsigned long long fatorial(int n) {
    // 1. CASO BASE - para a recursão
    if (n <= 1) {
        printf("Caso base: fatorial(1) = 1\n");
        return 1;
    }
    
    // 2. PASSO RECURSIVO - chama a si mesma
    printf("Calculando fatorial(%d) = %d × fatorial(%d)\n", n, n, n-1);
    
    unsigned long long resultado = n * fatorial(n - 1);
    
    printf("fatorial(%d) = %llu\n", n, resultado);
    return resultado;
}

// Versão simplificada (mais comum)
unsigned long long fatorial_simples(int n) {
    if (n <= 1) return 1;           // Caso base
    return n * fatorial_simples(n - 1);  // Passo recursivo
}

int main() {
    printf("=== FATORIAL RECURSIVO ===\n");
    int numero = 5;
    unsigned long long resultado = fatorial(numero);
    printf("\nResultado final: %d! = %llu\n", numero, resultado);
    
    printf("\nVersão simples: %d! = %llu\n", numero, fatorial_simples(numero));
    
    return 0;
}
```

**Saída detalhada:**
```
=== FATORIAL RECURSIVO ===
Calculando fatorial(5) = 5 × fatorial(4)
Calculando fatorial(4) = 4 × fatorial(3)
Calculando fatorial(3) = 3 × fatorial(2)
Calculando fatorial(2) = 2 × fatorial(1)
Caso base: fatorial(1) = 1
fatorial(2) = 2
fatorial(3) = 6
fatorial(4) = 24
fatorial(5) = 120

Resultado final: 5! = 120
```

> [!important]
> ### Como a Pilha de Chamadas Funciona
> 
> Cada chamada recursiva empilha um novo contexto:
> 
> ```
> fatorial(5)
> │→ fatorial(4)  
> │  │→ fatorial(3)
> │  │  │→ fatorial(2)
> │  │  │  │→ fatorial(1)  ← CASO BASE
> │  │  │  │  return 1
> │  │  │  return 2 × 1 = 2
> │  │  return 3 × 2 = 6
> │  return 4 × 6 = 24
> return 5 × 24 = 120
> ```
> 
> **Cuidado:** Muita recursão pode causar **stack overflow**!

---

## 2. Exemplos Clássicos de Recursão

### Problemas Tradicionais

```c
#include <stdio.h>

// 1. Fibonacci: 0, 1, 1, 2, 3, 5, 8, 13, ...
int fibonacci(int n) {
    if (n <= 1) return n;  // Caso base: fib(0)=0, fib(1)=1
    return fibonacci(n - 1) + fibonacci(n - 2);
}

// 2. Soma de dígitos de um número
int soma_digitos(int n) {
    if (n == 0) return 0;  // Caso base
    return (n % 10) + soma_digitos(n / 10);
}

// 3. Potência: base^expoente
double potencia(double base, int expoente) {
    if (expoente == 0) return 1;  // Caso base
    if (expoente < 0) return 1 / potencia(base, -expoente);  // Expoente negativo
    
    return base * potencia(base, expoente - 1);
}

// 4. Máximo Divisor Comum (Euclides)
int mdc(int a, int b) {
    if (b == 0) return a;  // Caso base
    return mdc(b, a % b);  // Algoritmo de Euclides
}

void demonstrar_exemplos() {
    printf("=== EXEMPLOS CLÁSSICOS ===\n");
    
    printf("Fibonacci(7) = %d\n", fibonacci(7));
    printf("Soma dígitos(12345) = %d\n", soma_digitos(12345));
    printf("Potência(2, 8) = %.0f\n", potencia(2, 8));
    printf("Potência(2, -3) = %.3f\n", potencia(2, -3));
    printf("MDC(48, 18) = %d\n", mdc(48, 18));
}

int main() {
    demonstrar_exemplos();
    return 0;
}
```

> [!example]
> ### Visualizando Fibonacci
> 
> ```c
> // Árvore de chamadas do fibonacci(4):
> 
> fib(4)
> │→ fib(3)
> │  │→ fib(2)
> │  │  │→ fib(1) = 1
> │  │  │→ fib(0) = 0
> │  │  = 1 + 0 = 1
> │  │→ fib(1) = 1
> │  = 1 + 1 = 2
> │→ fib(2)
> │  │→ fib(1) = 1
> │  │→ fib(0) = 0
> │  = 1 + 0 = 1
> = 2 + 1 = 3
> ```
> 
> **Problema:** Muitos cálculos repetidos! fib(2) é calculado 2 vezes.

---

## 3. Recursão vs Iteração

### Comparação Detalhada

```c
#include <stdio.h>

// VERSÃO RECURSIVA
int fatorial_recursivo(int n) {
    printf("Chamando fatorial_recursivo(%d)\n", n);
    if (n <= 1) return 1;
    return n * fatorial_recursivo(n - 1);
}

// VERSÃO ITERATIVA
int fatorial_iterativo(int n) {
    int resultado = 1;
    for (int i = 2; i <= n; i++) {
        resultado *= i;
        printf("Multiplicando por %d → resultado = %d\n", i, resultado);
    }
    return resultado;
}

// Fibonacci recursivo (ineficiente)
int fib_recursivo(int n) {
    if (n <= 1) return n;
    return fib_recursivo(n - 1) + fib_recursivo(n - 2);
}

// Fibonacci iterativo (eficiente)
int fib_iterativo(int n) {
    if (n <= 1) return n;
    
    int a = 0, b = 1, c;
    for (int i = 2; i <= n; i++) {
        c = a + b;
        a = b;
        b = c;
        printf("fib(%d) = %d\n", i, b);
    }
    return b;
}

void comparar_abordagens() {
    printf("=== FATORIAL: RECURSIVO vs ITERATIVO ===\n");
    printf("Recursivo: %d\n", fatorial_recursivo(5));
    printf("\nIterativo: %d\n", fatorial_iterativo(5));
    
    printf("\n=== FIBONACCI: RECURSIVO vs ITERATIVO ===\n");
    printf("Recursivo - fib(7) = %d\n", fib_recursivo(7));
    printf("Iterativo - fib(7) = %d\n", fib_iterativo(7));
}

int main() {
    comparar_abordagens();
    return 0;
}
```

> [!warning]
> ### Quando EVITAR Recursão
> 
> **Use iteração quando:**
> - **Performance crítica** - recursão é mais lenta
> - **Profundidade grande** - risco de stack overflow
> - **Cálculos repetidos** - como Fibonacci puro
> 
> **Use recursão quando:**
> - **Código mais legível** - problemas naturalmente recursivos
> - **Estruturas recursivas** - árvores, grafos
> - **Divide and conquer** - merge sort, quick sort

---

## 4. Tipos de Recursão

### Diferentes Padrões

```c
#include <stdio.h>

// 1. RECURSÃO DIRETA: função chama a si mesma diretamente
int soma_direta(int n) {
    if (n == 0) return 0;
    return n + soma_direta(n - 1);
}

// 2. RECURSÃO INDIRETA: A chama B, B chama A
int funcao_b(int n);  // Declaração antecipada

int funcao_a(int n) {
    if (n <= 0) return 0;
    printf("A(%d) → ", n);
    return funcao_b(n - 1);
}

int funcao_b(int n) {
    if (n <= 0) return 0;
    printf("B(%d) → ", n);
    return funcao_a(n - 1);
}

// 3. RECURSÃO DE CAUDA (Tail Recursion)
// A chamada recursiva é a ÚLTIMA operação
int soma_cauda(int n, int acumulador) {
    if (n == 0) return acumulador;
    return soma_cauda(n - 1, acumulador + n);  // Última operação
}

// 4. RECURSÃO ANINHADA (Nested Recursion)
int funcao_h(int n) {
    if (n == 0) return 0;
    if (n > 100) return n - 10;
    return funcao_h(funcao_h(n + 11));  // Recursão dentro da recursão
}

// 5. RECURSÃO MÚTUA Múltiplas funções se chamando
int par(int n);
int impar(int n);

int par(int n) {
    if (n == 0) return 1;  // 0 é par
    return impar(n - 1);
}

int impar(int n) {
    if (n == 0) return 0;  // 0 não é ímpar
    return par(n - 1);
}

void demonstrar_tipos() {
    printf("=== TIPOS DE RECURSÃO ===\n");
    
    printf("1. Direta - soma(5) = %d\n", soma_direta(5));
    
    printf("2. Indireta: ");
    funcao_a(5);
    printf("FIM\n");
    
    printf("3. Cauda - soma_cauda(5, 0) = %d\n", soma_cauda(5, 0));
    
    printf("4. Aninhada - h(99) = %d\n", funcao_h(99));
    
    printf("5. Mútua - 7 é %s\n", par(7) ? "par" : "ímpar");
}

int main() {
    demonstrar_tipos();
    return 0;
}
```

> [!important]
> ### Recursão de Cauda e Otimização
> 
> **Recursão de cauda** pode ser otimizada pelo compiler para usar **iteração**:
> 
> ```c
> // ✅ RECURSÃO DE CAUDA (pode ser otimizada)
// int fatorial_cauda(int n, int acc) {
//     if (n == 0) return acc;
//     return fatorial_cauda(n - 1, n * acc);  // ÚLTIMA operação
// }
> 
> // ❌ NÃO É DE CAUDA (não pode ser otimizada)
// int fatorial_nao_cauda(int n) {
//     if (n == 0) return 1;
//     return n * fatorial_nao_cauda(n - 1);  // Multiplicação DEPOIS da recursão
// }
> ```
> 
> Compilers modernos convertem recursão de cauda em loops automaticamente!

---

## 5. Aplicações Práticas

### Problemas do Mundo Real

```c
#include <stdio.h>
#include <string.h>

// 1. Busca Binária Recursiva
int busca_binaria(int arr[], int inicio, int fim, int alvo) {
    if (inicio > fim) return -1;  // Não encontrado
    
    int meio = inicio + (fim - inicio) / 2;
    
    printf("Buscando em [%d-%d], meio=%d\n", inicio, fim, meio);
    
    if (arr[meio] == alvo) return meio;  // Encontrado!
    
    if (arr[meio] > alvo) 
        return busca_binaria(arr, inicio, meio - 1, alvo);  // Busca esquerda
    else 
        return busca_binaria(arr, meio + 1, fim, alvo);     // Busca direita
}

// 2. Torres de Hanoi
void torres_hanoi(int n, char origem, char destino, char auxiliar) {
    if (n == 1) {
        printf("Mover disco 1 de %c para %c\n", origem, destino);
        return;
    }
    
    torres_hanoi(n - 1, origem, auxiliar, destino);
    printf("Mover disco %d de %c para %c\n", n, origem, destino);
    torres_hanoi(n - 1, auxiliar, destino, origem);
}

// 3. Permutações de uma string
void permutacoes(char *str, int inicio, int fim) {
    if (inicio == fim) {
        printf("%s\n", str);
        return;
    }
    
    for (int i = inicio; i <= fim; i++) {
        // Troca caracteres
        char temp = str[inicio];
        str[inicio] = str[i];
        str[i] = temp;
        
        // Recursão
        permutacoes(str, inicio + 1, fim);
        
        // Backtrack - desfaz a troca
        temp = str[inicio];
        str[inicio] = str[i];
        str[i] = temp;
    }
}

// 4. Caminhos em grade (m x n)
int caminhos_grade(int m, int n) {
    if (m == 1 || n == 1) return 1;  // Só pode ir em linha reta
    return caminhos_grade(m - 1, n) + caminhos_grade(m, n - 1);
}

void demonstrar_aplicacoes() {
    printf("=== APLICAÇÕES PRÁTICAS ===\n");
    
    // Busca binária
    int array[] = {2, 5, 8, 12, 16, 23, 38, 45, 67, 89};
    int tamanho = sizeof(array) / sizeof(array[0]);
    int alvo = 23;
    
    printf("1. Busca Binária - procurando %d:\n", alvo);
    int pos = busca_binaria(array, 0, tamanho - 1, alvo);
    printf("Encontrado na posição: %d\n\n", pos);
    
    // Torres de Hanoi
    printf("2. Torres de Hanoi com 3 discos:\n");
    torres_hanoi(3, 'A', 'C', 'B');
    printf("\n");
    
    // Permutações
    printf("3. Permutações de 'ABC':\n");
    char str[] = "ABC";
    permutacoes(str, 0, strlen(str) - 1);
    
    // Caminhos em grade
    printf("\n4. Caminhos em grade 3x3: %d\n", caminhos_grade(3, 3));
}

int main() {
    demonstrar_aplicacoes();
    return 0;
}
```

> [!tip]
> ### Quando Recursão é a Melhor Escolha
> 
> **Problemas naturalmente recursivos:**
> - **Estruturas de dados recursivas**: árvores, listas encadeadas
> - **Algoritmos divide and conquer**: merge sort, quick sort
> - **Backtracking**: labirintos, permutações, sudoku
> - **Problemas combinatorios**: subconjuntos, caminhos
> - **Parsing**: expressões matemáticas, XML/JSON

---

## 6. Otimização de Recursão

### Técnicas para Melhorar Performance

```c
#include <stdio.h>
#include <string.h>

// 1. MEMOIZATION - Cache de resultados
#define MAX 100
long long fib_cache[MAX] = {0};

long long fibonacci_memo(int n) {
    if (n <= 1) return n;
    
    // Se já calculou, retorna do cache
    if (fib_cache[n] != 0) return fib_cache[n];
    
    // Calcula e armazena no cache
    fib_cache[n] = fibonacci_memo(n - 1) + fibonacci_memo(n - 2);
    return fib_cache[n];
}

// 2. RECURSÃO DE CAUDA OTIMIZADA
int fatorial_cauda_otimizado(int n, int acumulador) {
    if (n == 0) return acumulador;
    return fatorial_cauda_otimizado(n - 1, n * acumulador);
}

// Wrapper para esconder o acumulador
int fatorial_otimizado(int n) {
    return fatorial_cauda_otimizado(n, 1);
}

// 3. ITERAÇÃO HÍBRIDA - Recursão até certo ponto, depois iteração
int fibonacci_hibrido(int n) {
    if (n <= 1) return n;
    
    // Para n pequeno, usa recursão; para n grande, iteração
    if (n > 40) {
        int a = 0, b = 1, c;
        for (int i = 2; i <= n; i++) {
            c = a + b;
            a = b;
            b = c;
        }
        return b;
    } else {
        return fibonacci_hibrido(n - 1) + fibonacci_hibrido(n - 2);
    }
}

// 4. STACK PERSONALIZADO - Evita stack overflow para recursão profunda
struct Stack {
    int data[1000];
    int top;
};

void fibonacci_iterativo_profundo(int n) {
    if (n <= 1) {
        printf("fib(%d) = %d\n", n, n);
        return;
    }
    
    struct Stack stack;
    stack.top = -1;
    
    // Empilha os casos base
    stack.data[++stack.top] = 0;  // fib(0)
    stack.data[++stack.top] = 1;  // fib(1)
    
    for (int i = 2; i <= n; i++) {
        int fib_i = stack.data[stack.top] + stack.data[stack.top - 1];
        stack.data[++stack.top] = fib_i;
        printf("fib(%d) = %d\n", i, fib_i);
    }
}

void demonstrar_otimizacoes() {
    printf("=== OTIMIZAÇÕES DE RECURSÃO ===\n");
    
    printf("1. Fibonacci com Memoization (n=50):\n");
    printf("fib(50) = %lld\n\n", fibonacci_memo(50));
    
    printf("2. Fatorial com Recursão de Cauda:\n");
    printf("10! = %d\n\n", fatorial_otimizado(10));
    
    printf("3. Fibonacci Híbrido:\n");
    printf("fib(45) = %d (hibrido)\n", fibonacci_hibrido(45));
    
    printf("\n4. Fibonacci Iterativo Profundo:\n");
    fibonacci_iterativo_profundo(10);
}

int main() {
    demonstrar_otimizacoes();
    return 0;
}
```

> [!warning]
> ### Limitações e Perigos
> 
> ```c
> // ❌ RECURSÃO INFINITA (Stack Overflow)
// void recursao_infinita() {
//     recursao_infinita();  // Nunca para!
// }
> 
> // ❌ PROFUNDIDADE EXCESSIVA
// int fatorial_grande(int n) {
//     if (n <= 1) return 1;
//     return n * fatorial_grande(n - 1);  // Stack overflow para n grande
// }
> 
> // ❌ MEMÓRIA INSUFICIENTE
// // Cada chamada consome stack space
// // Muitas chamadas = stack overflow
> 
> // ✅ SOLUÇÕES:
> // 1. Sempre tenha caso base
> // 2. Use iteração para problemas grandes  
> // 3. Considere recursão de cauda + otimização
> // 4. Use memoization para cálculos repetidos
> ```

---

## 7. Exercícios Práticos

### Para Fixar o Conhecimento

```c
#include <stdio.h>
#include <string.h>

// Exercício 1: Contar caracteres em string
int contar_caracteres(char *str) {
    if (*str == '\0') return 0;  // String vazia
    return 1 + contar_caracteres(str + 1);
}

// Exercício 2: Inverter string
void inverter_string(char *str, int inicio, int fim) {
    if (inicio >= fim) return;  // Caso base
    
    // Troca caracteres
    char temp = str[inicio];
    str[inicio] = str[fim];
    str[fim] = temp;
    
    // Recursão
    inverter_string(str, inicio + 1, fim - 1);
}

// Exercício 3: Verificar palíndromo
int eh_palindromo(char *str, int inicio, int fim) {
    if (inicio >= fim) return 1;  // É palíndromo
    
    if (str[inicio] != str[fim]) return 0;  // Não é palíndromo
    
    return eh_palindromo(str, inicio + 1, fim - 1);
}

// Exercício 4: Buscar elemento em array
int buscar_elemento(int arr[], int tamanho, int alvo, int index) {
    if (index >= tamanho) return -1;  // Não encontrado
    
    if (arr[index] == alvo) return index;  // Encontrado
    
    return buscar_elemento(arr, tamanho, alvo, index + 1);
}

// Exercício 5: Soma de array
int soma_array(int arr[], int tamanho) {
    if (tamanho == 0) return 0;  // Array vazio
    
    return arr[0] + soma_array(arr + 1, tamanho - 1);
}

void resolver_exercicios() {
    printf("=== EXERCÍCIOS PRÁTICOS ===\n");
    
    // Exercício 1
    char texto[] = "Recursão";
    printf("1. '%s' tem %d caracteres\n", texto, contar_caracteres(texto));
    
    // Exercício 2
    char str_inverter[] = "ABCDE";
    printf("2. Original: %s -> ", str_inverter);
    inverter_string(str_inverter, 0, strlen(str_inverter) - 1);
    printf("Invertida: %s\n", str_inverter);
    
    // Exercício 3
    char palindromo[] = "radar";
    char nao_palindromo[] = "casa";
    printf("3. '%s' é palíndromo? %s\n", palindromo, 
           eh_palindromo(palindromo, 0, strlen(palindromo)-1) ? "Sim" : "Não");
    printf("   '%s' é palíndromo? %s\n", nao_palindromo,
           eh_palindromo(nao_palindromo, 0, strlen(nao_palindromo)-1) ? "Sim" : "Não");
    
    // Exercício 4
    int numeros[] = {10, 20, 30, 40, 50};
    int alvo = 30;
    int pos = buscar_elemento(numeros, 5, alvo, 0);
    printf("4. Buscando %d: encontrado na posição %d\n", alvo, pos);
    
    // Exercício 5
    printf("5. Soma do array: %d\n", soma_array(numeros, 5));
}

int main() {
    resolver_exercicios();
    return 0;
}
```

> [!success]
> ## Conclusão
> 
> **Recursão é uma ferramenta poderosa quando usada corretamente:**
> 
> ✅ **Vantagens:**
> - Código mais limpo e expressivo
> - Solução natural para problemas recursivos
> - Facilita implementação de algoritmos complexos
> 
> ❌ **Desvantagens:**
> - Overhead de chamadas de função
> - Risco de stack overflow
> - Pode ser menos eficiente que iteração
> 
> **Dica final:** Comece pensando no **caso base** e depois no **passo recursivo**. Use **memoization** para cálculos repetidos e considere **iteração** para problemas de grande escala!