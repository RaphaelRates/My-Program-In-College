## Introdução Simples

Funções em C são blocos de código que realizam tarefas específicas. Elas permitem organizar o código em partes reutilizáveis, facilitando a leitura, manutenção e evitando repetição.

**Conceito Básico:**
```c
// Declaração (diz ao compiler que a função existe)
int soma(int a, int b);

// Definição (implementa o que a função faz)
int soma(int a, int b) {
    return a + b;
}

// Chamada (usa a função)
int resultado = soma(5, 3);  // resultado = 8
```

> [!note]
> ## Conceito Fundamental: Funções em C
> 
> Funções são o coração da programação modular em C. Elas encapsulam comportamentos, promovem reutilização de código e facilitam testes. Em C, toda função deve ser **declarada** antes do uso e **definida** em algum lugar do programa.

---

## 1. Estrutura Básica de uma Função

### Anatomia Completa

```c
#include <stdio.h>

// DECLARAÇÃO (protótipo) - opcional se a definição vier antes do uso
int calcular_area(int largura, int altura);

// DEFINIÇÃO (implementação)
int calcular_area(int largura, int altura) {
    // Corpo da função
    int area = largura * altura;
    return area;  // Retorna o valor calculado
}

// Função sem retorno (void)
void imprimir_mensagem(char *mensagem) {
    printf("Mensagem: %s\n", mensagem);
    // Não precisa de return (ou pode usar return; sem valor)
}

// Função sem parâmetros
void cumprimentar(void) {
    printf("Olá! Bem-vindo ao programa.\n");
}

int main() {
    // CHAMADA das funções
    int resultado = calcular_area(10, 5);
    printf("Área: %d\n", resultado);
    
    imprimir_mensagem("Funções são úteis!");
    cumprimentar();
    
    return 0;
}
```

> [!important]
> ### Partes de uma Função
> 
> | Componente | Exemplo | Propósito |
> |------------|---------|-----------|
> | **Tipo de Retorno** | `int` | Tipo do valor que a função retorna |
> | **Nome** | `calcular_area` | Identificador único da função |
> | **Parâmetros** | `(int l, int h)` | Dados que a função recebe |
> | **Corpo** | `{ return l * h; }` | Implementação da função |
> | **Return** | `return area;` | Retorna valor e termina execução |

---

## 2. Declaração vs Definição

### Diferenças Cruciais

```c
#include <stdio.h>

// DECLARAÇÃO (Protótipo) - APENAS a assinatura
// Diz ao compiler: "esta função existe, confia em mim"
int multiplicar(int x, int y);
void exibir_resultado(int valor);
float dividir(float a, float b);

int main() {
    int resultado = multiplicar(7, 6);  //  Compiler confia na declaração
    exibir_resultado(resultado);
    
    float quociente = dividir(10.0, 3.0);
    printf("Divisão: %.2f\n", quociente);
    
    return 0;
}

// DEFINIÇÃO - Implementação REAL da função
int multiplicar(int x, int y) {
    return x * y;
}

void exibir_resultado(int valor) {
    printf("Resultado: %d\n", valor);
}

float dividir(float a, float b) {
    if (b != 0) {
        return a / b;
    } else {
        printf("Erro: Divisão por zero!\n");
        return 0;
    }
}
```

> [!tip]
> ### Quando Usar Declarações?
> 
> **Use declarações (protótipos) quando:**
> - Função é definida **após** ser usada
> - Função está em **arquivo diferente**
> - Criando **bibliotecas**
> 
> ```c
> // Arquivo: main.c
> #include <stdio.h>
> 
> // Declarações das funções em outros arquivos
> int funcao_do_arquivo_a(void);
> void funcao_do_arquivo_b(int x);
> 
> int main() {
>     funcao_do_arquivo_a();  //  Compiler sabe da declaração
>     return 0;
> }
> ```

---

## 3. Tipos de Funções

### Classificação por Comportamento

```c
#include <stdio.h>
#include <math.h>

// 1. Função COM retorno e COM parâmetros
int potencia(int base, int expoente) {
    int resultado = 1;
    for (int i = 0; i < expoente; i++) {
        resultado *= base;
    }
    return resultado;
}

// 2. Função COM retorno e SEM parâmetros
int ler_numero_usuario(void) {
    int num;
    printf("Digite um número: ");
    scanf("%d", &num);
    return num;
}

// 3. Função SEM retorno e COM parâmetros
void desenhar_linha(int comprimento) {
    for (int i = 0; i < comprimento; i++) {
        printf("-");
    }
    printf("\n");
}

// 4. Função SEM retorno e SEM parâmetros
void exibir_menu(void) {
    printf("\n=== MENU ===\n");
    printf("1. Calcular potência\n");
    printf("2. Sair\n");
    printf("=============\n");
}

// 5. Função que retorna ponteiro
char* obter_saudacao(int hora) {
    if (hora < 12) return "Bom dia";
    else if (hora < 18) return "Boa tarde";
    else return "Boa noite";
}

int main() {
    // Testando diferentes tipos de funções
    exibir_menu();
    
    int num = ler_numero_usuario();
    desenhar_linha(20);
    
    int pot = potencia(num, 3);
    printf("%d³ = %d\n", num, pot);
    
    char *saudacao = obter_saudacao(14);
    printf("%s!\n", saudacao);
    
    return 0;
}
```

> [!example]
> ### Exemplo Prático: Calculadora Científica
> 
> ```c
> #include <stdio.h>
> #include <math.h>
> 
> // Declarações
> double calcular_seno(double angulo_graus);
> double calcular_logaritmo(double numero);
> void exibir_opcoes(void);
> 
> int main() {
>     int opcao;
>     double valor, resultado;
>     
>     do {
>         exibir_opcoes();
>         printf("Escolha uma opção: ");
>         scanf("%d", &opcao);
>         
>         switch (opcao) {
>             case 1:
>                 printf("Ângulo em graus: ");
>                 scanf("%lf", &valor);
>                 resultado = calcular_seno(valor);
>                 printf("Seno(%.2f°) = %.4f\n", valor, resultado);
>                 break;
>                 
>             case 2:
>                 printf("Número: ");
>                 scanf("%lf", &valor);
>                 resultado = calcular_logaritmo(valor);
>                 printf("Log(%.2f) = %.4f\n", valor, resultado);
>                 break;
>                 
>             case 0:
>                 printf("Saindo...\n");
>                 break;
>                 
>             default:
>                 printf("Opção inválida!\n");
>         }
>     } while (opcao != 0);
>     
>     return 0;
> }
> 
> // Definições
> double calcular_seno(double angulo_graus) {
>     double radianos = angulo_graus * M_PI / 180.0;
>     return sin(radianos);
> }
> 
> double calcular_logaritmo(double numero) {
>     if (numero > 0) {
>         return log10(numero);
>     } else {
>         printf("Erro: Número deve ser positivo!\n");
>         return 0;
>     }
> }
> 
> void exibir_opcoes(void) {
>     printf("\n=== CALCULADORA CIENTÍFICA ===\n");
>     printf("1. Calcular seno\n");
>     printf("2. Calcular logaritmo\n");
>     printf("0. Sair\n");
>     printf("==============================\n");
> }
> ```

---

## 4. Parâmetros e Retorno

### Trabalhando com Diferentes Tipos

```c
#include <stdio.h>
#include <string.h>

// Parâmetros por VALOR (cópia)
int dobrar_valor(int x) {
    x = x * 2;  // Modifica apenas a cópia local
    return x;
}

// Parâmetros por REFERÊNCIA (usa ponteiros)
void dobrar_referencia(int *x) {
    *x = *x * 2;  // Modifica a variável original
}

// Retornando múltiplos valores através de parâmetros
void calcular_estatisticas(int array[], int tamanho, int *min, int *max, double *media) {
    if (tamanho == 0) return;
    
    *min = array[0];
    *max = array[0];
    *media = 0;
    
    for (int i = 0; i < tamanho; i++) {
        if (array[i] < *min) *min = array[i];
        if (array[i] > *max) *max = array[i];
        *media += array[i];
    }
    *media /= tamanho;
}

// Função com parâmetros opcionais (usando valores padrão)
int potencia_personalizada(int base, int expoente) {
    // Se expoente não for especificado, usa 2
    return (expoente == 0) ? (base * base) : potencia(base, expoente);
}

// Função com número variável de parâmetros
#include <stdarg.h>
double media_variavel(int count, ...) {
    va_list args;
    va_start(args, count);
    
    double soma = 0;
    for (int i = 0; i < count; i++) {
        soma += va_arg(args, double);
    }
    
    va_end(args);
    return soma / count;
}

int main() {
    // Teste parâmetros por valor vs referência
    int numero = 5;
    
    printf("Original: %d\n", numero);
    printf("Dobrado (valor): %d\n", dobrar_valor(numero));
    printf("Ainda original: %d\n", numero);
    
    dobrar_referencia(&numero);
    printf("Agora modificado: %d\n", numero);
    
    // Teste múltiplos retornos
    int vetor[] = {10, 5, 8, 3, 15};
    int min, max;
    double media;
    
    calcular_estatisticas(vetor, 5, &min, &max, &media);
    printf("Mín: %d, Máx: %d, Média: %.2f\n", min, max, media);
    
    // Teste parâmetros variáveis
    printf("Média de 3 números: %.2f\n", media_variavel(3, 10.5, 20.3, 15.2));
    
    return 0;
}
```

> [!warning]
> ### Cuidado com Parâmetros!
> 
> **Problemas comuns e soluções:**
> 
> ```c
> // ❌ PROBLEMA: Parâmetro por valor não modifica original
> void trocar_errado(int a, int b) {
>     int temp = a;
>     a = b;
>     b = temp;  // Só modifica as cópias locais
> }
> 
> // ✅ SOLUÇÃO: Use ponteiros para modificar originais
> void trocar_correto(int *a, int *b) {
>     int temp = *a;
>     *a = *b;
>     *b = temp;  // Modifica as variáveis originais
> }
> 
> // ❌ PROBLEMA: Array como parâmetro perde informação de tamanho
> void processar_array(int arr[]) {  // arr é tratado como ponteiro
>     // sizeof(arr) retorna tamanho do ponteiro, não do array!
> }
> 
> // ✅ SOLUÇÃO: Sempre passe o tamanho explicitamente
> void processar_array_correto(int arr[], int tamanho) {
>     for (int i = 0; i < tamanho; i++) {
>         // Processa arr[i]
>     }
> }
> ```

---

## 5. Escopo e Variáveis

### Visibilidade de Variáveis em Funções

```c
#include <stdio.h>

// Variável GLOBAL - visível em todo o programa
int contador_global = 0;

// Variável STATIC - mantém valor entre chamadas
void funcao_com_static(void) {
    static int contador_static = 0;  // Inicializada UMA vez
    contador_static++;
    printf("Static: %d\n", contador_static);
}

// Demonstração de escopos
void demonstrar_escopo(int parametro) {
    // Parâmetro - escopo dentro da função
    int variavel_local = 10;  // Local - só existe nesta função
    
    {
        // Escopo de bloco - variável só existe neste bloco
        int variavel_bloco = 20;
        printf("Dentro do bloco: %d\n", variavel_bloco);
    }
    // printf("%d\n", variavel_bloco);  // ❌ ERRO - fora do escopo
    
    contador_global++;  // ✅ Pode acessar variável global
    printf("Local: %d, Parâmetro: %d, Global: %d\n", 
           variavel_local, parametro, contador_global);
}

// Função que mostra shadowing (sombreamento)
void demonstrar_shadowing(int x) {
    printf("Parâmetro x: %d\n", x);
    
    int x = 100;  // ❌ AVISO: Sombreia o parâmetro
    printf("Local x: %d\n", x);
}

int main() {
    // Teste variável static
    printf("=== Variável Static ===\n");
    funcao_com_static();  // Static: 1
    funcao_com_static();  // Static: 2
    funcao_com_static();  // Static: 3
    
    printf("\n=== Escopos ===\n");
    demonstrar_escopo(5);
    demonstrar_escopo(8);
    
    printf("\n=== Shadowing ===\n");
    demonstrar_shadowing(50);
    
    return 0;
}
```

> [!note]
> ### Regras de Escopo em Funções
> 
> | Tipo de Variável | Escopo | Tempo de Vida | Inicialização |
> |------------------|--------|---------------|---------------|
> | **Local** | Dentro da função | Chamada da função | Cada chamada |
> | **Parâmetro** | Dentro da função | Chamada da função | Valor passado |
> | **Static Local** | Dentro da função | Programa inteiro | Uma vez |
> | **Global** | Arquivo inteiro | Programa inteiro | Início do programa |
> | **Global Static** | Arquivo atual | Programa inteiro | Início do programa |

---

## 6. Funções Recursivas

### Funções que Chamam a Si Mesmas

```c
#include <stdio.h>

// Exemplo clássico: Fatorial
unsigned long long fatorial(int n) {
    // Caso base - para a recursão
    if (n <= 1) {
        return 1;
    }
    // Caso recursivo - chama a si mesma
    return n * fatorial(n - 1);
}

// Fibonacci recursivo (ineficiente para grandes números)
int fibonacci(int n) {
    if (n <= 1) return n;
    return fibonacci(n - 1) + fibonacci(n - 2);
}

// Busca binária recursiva
int busca_binaria(int arr[], int inicio, int fim, int alvo) {
    if (inicio > fim) return -1;  // Não encontrado
    
    int meio = inicio + (fim - inicio) / 2;
    
    if (arr[meio] == alvo) return meio;  // Encontrado
    else if (arr[meio] > alvo) 
        return busca_binaria(arr, inicio, meio - 1, alvo);  // Busca na esquerda
    else 
        return busca_binaria(arr, meio + 1, fim, alvo);     // Busca na direita
}

// Torre de Hanoi
void torre_hanoi(int n, char origem, char destino, char auxiliar) {
    if (n == 1) {
        printf("Mover disco 1 de %c para %c\n", origem, destino);
        return;
    }
    
    torre_hanoi(n - 1, origem, auxiliar, destino);
    printf("Mover disco %d de %c para %c\n", n, origem, destino);
    torre_hanoi(n - 1, auxiliar, destino, origem);
}

int main() {
    // Teste fatorial
    printf("Fatorial de 5: %llu\n", fatorial(5));
    printf("Fatorial de 10: %llu\n", fatorial(10));
    
    // Teste Fibonacci
    printf("Fibonacci(7): %d\n", fibonacci(7));
    
    // Teste busca binária
    int array[] = {2, 5, 8, 12, 16, 23, 38, 45, 67, 89};
    int tamanho = sizeof(array) / sizeof(array[0]);
    int posicao = busca_binaria(array, 0, tamanho - 1, 23);
    printf("Busca binária - 23 encontrado na posição: %d\n", posicao);
    
    // Teste Torre de Hanoi
    printf("\nTorre de Hanoi com 3 discos:\n");
    torre_hanoi(3, 'A', 'C', 'B');
    
    return 0;
}
```

> [!warning]
> ### Cuidado com Recursão!
> 
> **Problemas da recursão:**
> - **Stack overflow** - muitas chamadas recursivas
> - **Ineficiência** - cálculos repetidos (ex: Fibonacci)
> - **Dificuldade de debug**
> 
> **Soluções:**
> ```c
> // ❌ Fibonacci recursivo puro (muito lento)
> int fib_lento(int n) {
>     if (n <= 1) return n;
>     return fib_lento(n-1) + fib_lento(n-2);
> }
> 
> // ✅ Fibonacci com memoização (cache)
> int fib_cache[100] = {0};
> 
> int fib_rapido(int n) {
>     if (n <= 1) return n;
>     if (fib_cache[n] != 0) return fib_cache[n];
>     
>     fib_cache[n] = fib_rapido(n-1) + fib_rapido(n-2);
>     return fib_cache[n];
> }
> 
> // ✅ Versão iterativa (mais eficiente)
> int fib_iterativo(int n) {
>     if (n <= 1) return n;
>     
>     int a = 0, b = 1, c;
>     for (int i = 2; i <= n; i++) {
>         c = a + b;
>         a = b;
>         b = c;
>     }
>     return b;
> }
> ```

---

## 7. Funções com Arrays e Strings

### Trabalhando com Estruturas de Dados

```c
#include <stdio.h>
#include <string.h>

// Função que recebe array de inteiros
void imprimir_array(int arr[], int tamanho) {
    printf("Array: [");
    for (int i = 0; i < tamanho; i++) {
        printf("%d", arr[i]);
        if (i < tamanho - 1) printf(", ");
    }
    printf("]\n");
}

// Função que modifica array
void dobrar_elementos(int arr[], int tamanho) {
    for (int i = 0; i < tamanho; i++) {
        arr[i] *= 2;  // Modifica o array original
    }
}

// Função com array multidimensional
void imprimir_matriz(int linhas, int colunas, int matriz[linhas][colunas]) {
    for (int i = 0; i < linhas; i++) {
        for (int j = 0; j < colunas; j++) {
            printf("%3d ", matriz[i][j]);
        }
        printf("\n");
    }
}

// Funções com strings
void processar_string(char *str) {
    // Converte para maiúsculas
    for (int i = 0; str[i] != '\0'; i++) {
        if (str[i] >= 'a' && str[i] <= 'z') {
            str[i] = str[i] - 'a' + 'A';
        }
    }
}

// Função que retorna string
char* inverter_string(char *str) {
    static char resultado[100];  // static para retornar ponteiro válido
    int len = strlen(str);
    
    for (int i = 0; i < len; i++) {
        resultado[i] = str[len - 1 - i];
    }
    resultado[len] = '\0';
    
    return resultado;
}

int main() {
    // Teste com arrays
    int numeros[] = {1, 2, 3, 4, 5};
    int tamanho = sizeof(numeros) / sizeof(numeros[0]);
    
    imprimir_array(numeros, tamanho);
    dobrar_elementos(numeros, tamanho);
    imprimir_array(numeros, tamanho);
    
    // Teste com matriz
    int matriz[2][3] = {{1, 2, 3}, {4, 5, 6}};
    printf("\nMatriz:\n");
    imprimir_matriz(2, 3, matriz);
    
    // Teste com strings
    char texto[] = "Hello World";
    printf("\nOriginal: %s\n", texto);
    processar_string(texto);
    printf("Maiúsculas: %s\n", texto);
    
    char *invertida = inverter_string("Programação");
    printf("Invertida: %s\n", invertida);
    
    return 0;
}
```

---

## 8. Tabela de Boas Práticas

### Guia Rápido para Funções em C

| Prática | Exemplo Bom | Exemplo Ruim |
|---------|-------------|--------------|
| **Nomes descritivos** | `calcular_media()` | `func1()` |
| **Uma responsabilidade** | Uma função, um propósito | Função faz tudo |
| **Parâmetros claros** | `(int idade, float altura)` | `(int a, float b)` |
| **Documentação** | Comentários antes da função | Sem comentários |
| **Tamanho moderado** | 10-30 linhas | 200+ linhas |
| **Validação** | Verifica entradas | Assume entradas válidas |
| **Retorno significativo** | Códigos de erro claros | Sem padrão de retorno |

> [!success]
> ## Conclusão
> 
> Dominar funções em C é essencial para escrever código limpo, eficiente e maintainable. Lembre-se:
> 
> 1. **Declare antes de usar** - use protótipos
> 2. **Uma função, uma tarefa** - mantenha o foco
> 3. **Nomeie com clareza** - funções são verbos
> 4. **Documente o propósito** - comentários ajudam
> 5. **Teste individualmente** - verifique cada função
> 6. **Considere recursão com cuidado** - prefira iteração quando possível
> 
> Com prática, você desenvolverá a habilidade de decompor problemas complexos em funções simples e reutilizáveis!