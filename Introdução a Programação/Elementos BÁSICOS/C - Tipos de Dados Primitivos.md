
> [!note] Para Iniciantes
> Vamos começar do zero! Se você nunca programou antes, pense em variáveis como **caixinhas** onde guardamos informações. Cada caixinha tem um nome e guarda um tipo específico de coisa.

## Conceito Básico: O que são Variáveis?

> [!example] Analogia do Armário
> Imagine um armário com várias gavetas:
> - Cada gaveta tem um **nome** (etiqueta)
> - Cada gaveta guarda um **tipo** de objeto (roupas, documentos, etc.)
> - Você pode **colocar** coisas nas gavetas
> - Você pode **pegar** coisas das gavetas

**Na programação:**
```c
int idade = 25;        // Gaveta chamada "idade" guarda número 25
float altura = 1.75;   // Gaveta chamada "altura" guarda número 1.75
char letra = 'A';      // Gaveta chamada "letra" guarda a letra A
```

### Seu Primeiro Programa com Variáveis

> [!code] Hello World com Variáveis
> 
> ```c
> #include <stdio.h>
> 
> int main() {
>     // Declarando variáveis (criando as gavetas)
>     char mensagem[] = "Olá, Mundo!";
>     int numero = 42;
>     float decimal = 3.14;
>     
>     // Usando as variáveis
>     printf("%s\n", mensagem);
>     printf("Meu número favorito: %d\n", numero);
>     printf("O valor de pi é: %.2f\n", decimal);
>     
>     return 0;
> }
> ```
> 
> **Saída:**
> ```
> Olá, Mundo!
> Meu número favorito: 42
> O valor de pi é: 3.14
> ```

## Nível Intermediário: Tipos e Operações

> [!info] Tipos de Variáveis em C
> 
> ```c
> #include <stdio.h>
> 
> int main() {
>     /* Tipos Inteiros - para números sem decimal */
>     int idade = 30;                    // Números inteiros
>     short ano = 2024;                  // Números menores
>     long populacao = 7800000000L;      // Números muito grandes
>     
>     /* Tipos Decimais - para números com ponto */
>     float peso = 68.5f;                // Precisão simples
>     double salario = 3500.75;          // Precisão dupla
>     
>     /* Tipo Caractere - para letras e símbolos */
>     char letra_inicial = 'M';          // Um único caractere
>     char nome[] = "Maria";             // Vários caracteres (texto)
>     
>     /* Mostrando os valores */
>     printf("Idade: %d anos\n", idade);
>     printf("Peso: %.1f kg\n", peso);
>     printf("Letra inicial: %c\n", letra_inicial);
>     printf("Nome: %s\n", nome);
>     
>     return 0;
> }
> ```

### Operações com Variáveis

> [!tip] Trabalhando com Variáveis
> 
> ```c
> #include <stdio.h>
> 
> int main() {
>     // Declaração e inicialização
>     int a = 10;
>     int b = 5;
>     
>     // Operações matemáticas
>     int soma = a + b;
>     int subtracao = a - b;
>     int multiplicacao = a * b;
>     int divisao = a / b;
>     int resto = a % b;
>     
>     // Mostrando resultados
>     printf("a = %d, b = %d\n", a, b);
>     printf("Soma: %d + %d = %d\n", a, b, soma);
>     printf("Subtração: %d - %d = %d\n", a, b, subtracao);
>     printf("Multiplicação: %d * %d = %d\n", a, b, multiplicacao);
>     printf("Divisão: %d / %d = %d\n", a, b, divisao);
>     printf("Resto: %d %% %d = %d\n", a, b, resto);
>     
>     // Modificando variáveis
>     a = a + 1;      // Incrementa a em 1
>     b += 3;         // Adiciona 3 ao valor de b
>     
>     printf("Novo a: %d, Novo b: %d\n", a, b);
>     
>     return 0;
> }
> ```

### Constantes e Boas Práticas

> [!important] Constantes e Convenções de Nome
> 
> ```c
> #include <stdio.h>
> 
> // Constante global - valor que não muda
> #define PI 3.14159
> #define TAXA_JUROS 0.05
> 
> int main() {
>     // Constante local
>     const int DIAS_SEMANA = 7;
>     const float GRAVIDADE = 9.8f;
>     
>     // Boas práticas de nomeação
>     int idade_usuario;           // snake_case
>     float saldoConta;           // camelCase
>     char nome_completo[50];     // arrays para textos longos
>     
>     // Exemplo de cálculo com constantes
>     float raio = 5.0f;
>     float area_circulo = PI * raio * raio;
>     
>     printf("Área do círculo: %.2f\n", area_circulo);
>     printf("Dias na semana: %d\n", DIAS_SEMANA);
>     
>     return 0;
> }
> ```

## Nível Avançado: Conceitos Profundos

> [!abstract] Arquitetura de Memória e Variáveis
> 
> ```c
> #include <stdio.h>
> #include <stdlib.h>
> 
> // Variável global - armazenada no segmento DATA
> int global_var = 100;
> 
> // Variável não inicializada - vai para BSS
> static int bss_var;
> 
> void memory_analysis() {
>     // Variável automática - na STACK
>     int stack_var = 42;
>     
>     // Variável estática - mantém valor entre chamadas
>     static int static_local = 0;
>     static_local++;
>     
>     // Alocação dinâmica - na HEAP
>     int *heap_var = (int*)malloc(sizeof(int));
>     *heap_var = 999;
>     
>     printf("Stack: %p -> %d\n", (void*)&stack_var, stack_var);
>     printf("Static local: %p -> %d\n", (void*)&static_local, static_local);
>     printf("Heap: %p -> %d\n", (void*)heap_var, *heap_var);
>     printf("Global: %p -> %d\n", (void*)&global_var, global_var);
>     
>     free(heap_var); // Liberar memória alocada
> }
> ```

### Escopo e Tempo de Vida

> [!summary] Visibilidade das Variáveis
> 
> ```c
> #include <stdio.h>
> 
> int global = 100; // Visível em todo o programa
> 
> void test_scope() {
>     int local_func = 50; // Visível apenas nesta função
>     static int persistent = 0; // Mantém valor entre chamadas
>     
>     persistent++;
>     printf("Dentro da função:\n");
>     printf("Local: %d, Static: %d, Global: %d\n", 
>            local_func, persistent, global);
>     
>     {
>         // Bloco interno
>         int block_var = 999; // Só existe neste bloco
>         printf("Dentro do bloco: %d\n", block_var);
>     }
>     // block_var não existe mais aqui!
> }
> 
> int main() {
>     test_scope();
>     test_scope();
>     test_scope();
>     
>     // local_func não é visível aqui
>     // persistent não é visível aqui  
>     // global é visível aqui
>     
>     return 0;
> }
> ```

### Tipos Avançados e Qualificadores

> [!info] Sistema de Tipos Completo
> 
> ```c
> #include <stdint.h>
> #include <stdbool.h>
> 
> void advanced_types() {
>     /* Tipos de tamanho fixo - para portabilidade */
>     int8_t pequeno = 127;
>     int16_t medio = 32767;
>     int32_t grande = 2147483647;
>     int64_t enorme = 9223372036854775807LL;
>     
>     /* Tipos sem sinal */
>     uint8_t byte = 255;
>     uint32_t unsigned_grande = 4294967295U;
>     
>     /* Booleanos */
>     bool verdadeiro = true;
>     bool falso = false;
>     
>     /* Qualificadores */
>     const int READ_ONLY = 100;      // Valor não pode mudar
>     volatile int sensor_value;      // Pode mudar externamente
>     register int counter;           // Sugestão para registro
> }
> ```

### Ponteiros e Referências

> [!caution] Trabalhando com Endereços de Memória
> 
> ```c
> #include <stdio.h>
> 
> void pointer_demo() {
>     int valor = 42;
>     int *ponteiro = &valor;  // Ponteiro guarda endereço de valor
>     
>     printf("Valor: %d\n", valor);
>     printf("Endereço: %p\n", (void*)&valor);
>     printf("Ponteiro aponta para: %p\n", (void*)ponteiro);
>     printf("Valor através do ponteiro: %d\n", *ponteiro);
>     
>     // Modificando através do ponteiro
>     *ponteiro = 100;
>     printf("Novo valor: %d\n", valor);
> }
> 
> int main() {
>     pointer_demo();
>     return 0;
> }
> ```

### Estruturas e Arrays

> [!tool] Variáveis Compostas
> 
> ```c
> #include <stdio.h>
> #include <string.h>
> 
> // Estrutura para agrupar variáveis
> struct Pessoa {
>     char nome[50];
>     int idade;
>     float altura;
> };
> 
> int main() {
>     // Array - coleção de variáveis do mesmo tipo
>     int numeros[5] = {10, 20, 30, 40, 50};
>     
>     // Estrutura - variável com múltiplos campos
>     struct Pessoa pessoa1;
>     strcpy(pessoa1.nome, "João");
>     pessoa1.idade = 25;
>     pessoa1.altura = 1.75f;
>     
>     // Acessando elementos
>     printf("Array: ");
>     for(int i = 0; i < 5; i++) {
>         printf("%d ", numeros[i]);
>     }
>     printf("\n");
>     
>     printf("Pessoa: %s, %d anos, %.2fm\n", 
>            pessoa1.nome, pessoa1.idade, pessoa1.altura);
>     
>     return 0;
> }
> ```

## Exercícios Práticos

> [!example] Para Fixar o Conhecimento
> 
> **Exercício 1:** Calculadora Simples
> ```c
> #include <stdio.h>
> 
> int main() {
>     float num1, num2;
>     
>     printf("Digite o primeiro número: ");
>     scanf("%f", &num1);
>     
>     printf("Digite o segundo número: ");
>     scanf("%f", &num2);
>     
>     printf("\nResultados:\n");
>     printf("Soma: %.2f\n", num1 + num2);
>     printf("Subtração: %.2f\n", num1 - num2);
>     printf("Multiplicação: %.2f\n", num1 * num2);
>     
>     if(num2 != 0) {
>         printf("Divisão: %.2f\n", num1 / num2);
>     } else {
>         printf("Divisão: Não é possível dividir por zero!\n");
>     }
>     
>     return 0;
> }
> ```
> 
> **Exercício 2:** Conversor de Temperatura
> ```c
> #include <stdio.h>
> 
> int main() {
>     float celsius, fahrenheit;
>     
>     printf("Digite a temperatura em Celsius: ");
>     scanf("%f", &celsius);
>     
>     fahrenheit = (celsius * 9/5) + 32;
>     
>     printf("%.1f°C = %.1f°F\n", celsius, fahrenheit);
>     
>     return 0;
> }
> ```

> [!success] Conclusão
> **Para Iniciantes:** Variáveis são como caixinhas que guardam informações. Cada uma tem um nome e um tipo.
> 
> **Para Intermediários:** Entenda os diferentes tipos, operações e como organizar seu código com constantes e boas práticas.
> 
> **Para Avançados:** Domine a arquitetura de memória, ponteiros, estruturas e técnicas de otimização.
> 
> **Próximos Passos:** 
> - Pratique com exercícios
> - Explore estruturas de controle (if, for, while)
> - Aprenda sobre funções
> - Estude alocação dinâmica de memória

Lembre-se: a prática é essencial para dominar variáveis em C! 🚀