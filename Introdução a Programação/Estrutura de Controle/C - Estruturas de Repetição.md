# Estruturas de Repetição em C - Guia Completo

> [!note] ## O que são Estruturas de Repetição?
> 
> Estruturas de repetição (também chamadas de **loops**) permitem que um bloco de código seja executado **múltiplas vezes**. Elas são essenciais para:
> - Processar listas de dados
> - Repetir operações até que uma condição seja satisfeita
> - Automatizar tarefas repetitivas
> - Iterar sobre arrays e estruturas de dados
> 
> **Analogia:** "Enquanto houver pratos sujos, continue lavando." ou "Para cada prato na pilha, lave-o."

---

## 1. A Estrutura WHILE

>[!abstract] ### O Loop Mais Fundamental
>
> O `while` executa um bloco de código **enquanto** uma condição for verdadeira. É ideal quando não sabemos quantas vezes precisaremos repetir.

**Sintaxe:**
```c
while (condicao) {
    // código a ser repetido
    // atualizar condicao (importante!)
}
```

> [!tip] 
> **Explicação:** Antes de cada iteração, a condição é verificada. Se for verdadeira, o bloco é executado. Se for falsa desde o início, o bloco pode nunca ser executado.

**Exemplo Prático: Contador Simples**
```c
#include <stdio.h>

int main() {
    int contador = 1;  // Inicialização
    
    while (contador <= 5) {  // Condição
        printf("Número: %d\n", contador);
        contador++;  // Atualização (CRUCIAL!)
    }
    
    printf("Fim do loop!\n");
    return 0;
}
```
**Saída:**
```
Número: 1
Número: 2
Número: 3
Número: 4
Número: 5
Fim do loop!
```

**Exemplo: Validação de Entrada**
```c
#include <stdio.h>

int main() {
    int numero;
    
    printf("Digite um número positivo: ");
    scanf("%d", &numero);
    
    // Continua pedindo enquanto o número for inválido
    while (numero <= 0) {
        printf("Número inválido! Digite um número positivo: ");
        scanf("%d", &numero);
    }
    
    printf("Você digitou: %d\n", numero);
    return 0;
}
```

**CUIDADO: Loop Infinito!**
```c
// ⚠️ PERIGO: Loop infinito!
int x = 1;
while (x > 0) {  // Condição sempre verdadeira
    printf("Preso no loop! %d\n", x);
    x++;  // Mas x sempre será > 0
}

// Correto: condição que eventualmente se torna falsa
int x = 1;
while (x <= 10) {
    printf("%d\n", x);
    x++;  // Eventualmente x será 11 e sairá do loop
}
```

---

## 2. A Estrutura DO-WHILE

> [!note] ### Execute Primeiro, Verifique Depois
>
> O `do-while` garante que o bloco de código seja executado **pelo menos uma vez**, pois a verificação da condição acontece no final.

**Sintaxe:**
```c
do {
    // código a ser repetido
    // atualizar condicao
} while (condicao);
```

> [!note] 
> **Explicação:** Diferente do `while`, o `do-while` executa o bloco primeiro e depois verifica a condição. Isso é útil para menus e validações onde precisamos executar pelo menos uma vez.

**Exemplo: Menu Interativo**
```c
#include <stdio.h>

int main() {
    int opcao;
    
    do {
        printf("\n=== MENU ===\n");
        printf("1. Ver saldo\n");
        printf("2. Depositar\n");
        printf("3. Sacar\n");
        printf("0. Sair\n");
        printf("Escolha uma opção: ");
        scanf("%d", &opcao);
        
        switch (opcao) {
            case 1:
                printf("Saldo: R$ 1000,00\n");
                break;
            case 2:
                printf("Depositando...\n");
                break;
            case 3:
                printf("Sacando...\n");
                break;
            case 0:
                printf("Saindo...\n");
                break;
            default:
                printf("Opção inválida!\n");
        }
    } while (opcao != 0);  // Continua até usuário escolher 0
    
    printf("Programa encerrado!\n");
    return 0;
}
```

**Exemplo: Calculadora com Repetição**
```c
#include <stdio.h>

int main() {
    char continuar;
    float num1, num2;
    
    do {
        printf("\n=== CALCULADORA ===\n");
        printf("Digite dois números: ");
        scanf("%f %f", &num1, &num2);
        
        printf("Soma: %.2f\n", num1 + num2);
        printf("Subtração: %.2f\n", num1 - num2);
        printf("Multiplicação: %.2f\n", num1 * num2);
        
        if (num2 != 0) {
            printf("Divisão: %.2f\n", num1 / num2);
        } else {
            printf("Divisão: Indefinida (divisão por zero)\n");
        }
        
        printf("\nDeseja continuar? (s/n): ");
        scanf(" %c", &continuar);  // Espaço antes do %c para ignorar \n
        
    } while (continuar == 's' || continuar == 'S');
    
    printf("Obrigado por usar a calculadora!\n");
    return 0;
}
```

---

## 3. A Estrutura FOR

> [!note] ### O Loop Mais Controlado
>
> O `for` é ideal quando sabemos **antecipadamente** quantas vezes queremos repetir o bloco. Ele combina inicialização, condição e atualização em uma linha.

**Sintaxe:**
```c
for (inicializacao; condicao; atualizacao) {
    // código a ser repetido
}
```

**Explicação:** O `for` é como um `while` mais organizado:
1. **Inicialização:** Executada uma vez no início
2. **Condição:** Verificada antes de cada iteração
3. **Atualização:** Executada após cada iteração

**Exemplo: Contador Clássico**
```c
#include <stdio.h>

int main() {
    // for (início; condição; passo)
    for (int i = 1; i <= 10; i++) {
        printf("%d ", i);
    }
    printf("\n");
    
    return 0;
}
```
**Saída:** `1 2 3 4 5 6 7 8 9 10`

**Exemplo: Tabuada**
```c
#include <stdio.h>

int main() {
    int numero;
    
    printf("Digite um número para ver sua tabuada: ");
    scanf("%d", &numero);
    
    printf("\nTabuada do %d:\n", numero);
    for (int i = 1; i <= 10; i++) {
        printf("%d x %d = %d\n", numero, i, numero * i);
    }
    
    return 0;
}
```

**Variações do FOR:**
```c
// Contagem regressiva
for (int i = 10; i >= 1; i--) {
    printf("%d... ", i);
}
printf("Fogo!\n");

// De 2 em 2
for (int i = 0; i <= 20; i += 2) {
    printf("%d ", i);
}

// Múltiplas variáveis
for (int i = 0, j = 10; i < j; i++, j--) {
    printf("i = %d, j = %d\n", i, j);
}

// Loop infinito com for
for (;;) {  // Condição omitida = sempre verdadeira
    printf("Loop infinito! Use Ctrl+C para parar.\n");
}
```

---

## 4. Comparação Entre os Loops

| Característica | WHILE | DO-WHILE | FOR |
|----------------|-------|-----------|-----|
| **Verificação** | Antes do bloco | Depois do bloco | Antes do bloco |
| **Execução mínima** | 0 vezes | 1 vez | 0 vezes |
| **Uso ideal** | Condições desconhecidas | Menus, validações | Contagens conhecidas |
| **Controle** | Manual da condição | Manual da condição | Automático (inicialização, condição, atualização) |

**Equivalência entre FOR e WHILE:**
```c
// FOR
for (int i = 0; i < 10; i++) {
    printf("%d ", i);
}

// WHILE equivalente
int i = 0;
while (i < 10) {
    printf("%d ", i);
    i++;
}
```

---

## 5. Comandos de Controle: BREAK e CONTINUE

### Alterando o Fluxo dos Loops

**BREAK:** Sai imediatamente do loop
**CONTINUE:** Pula para a próxima iteração

**Exemplo com BREAK: Busca em Array**
```c
#include <stdio.h>

int main() {
    int numeros[] = {10, 25, 8, 42, 15, 30};
    int busca;
    int encontrado = 0;
    
    printf("Digite um número para buscar: ");
    scanf("%d", &busca);
    
    for (int i = 0; i < 6; i++) {
        if (numeros[i] == busca) {
            printf("Número encontrado na posição %d!\n", i);
            encontrado = 1;
            break;  // Sai do loop imediatamente
        }
    }
    
    if (!encontrado) {
        printf("Número não encontrado!\n");
    }
    
    return 0;
}
```

**Exemplo com CONTINUE: Números Ímpares**
```c
#include <stdio.h>

int main() {
    printf("Números pares de 1 a 20:\n");
    
    for (int i = 1; i <= 20; i++) {
        if (i % 2 != 0) {  // Se for ímpar
            continue;       // Pula para a próxima iteração
        }
        printf("%d ", i);   // Só executa para números pares
    }
    printf("\n");
    
    return 0;
}
```
**Saída:** `2 4 6 8 10 12 14 16 18 20`

**Exemplo Prático: Processamento com Exceções**
```c
#include <stdio.h>

int main() {
    printf("Processando números (ignorando negativos e maiores que 100):\n");
    
    for (int i = -5; i <= 105; i++) {
        // Ignora números negativos
        if (i < 0) {
            continue;
        }
        
        // Para se encontrar número > 100
        if (i > 100) {
            printf("Número muito alto encontrado. Parando...\n");
            break;
        }
        
        // Processa apenas números válidos
        printf("Processando: %d\n", i);
    }
    
    return 0;
}
```

---

## 6. Loops Aninhados

> [!summary] ### Loops Dentro de Loops
>
> Podemos colocar um loop dentro de outro para trabalhar com estruturas bidimensionais como matrizes.

**Exemplo: Tabuada Completa**
```c
#include <stdio.h>

int main() {
    printf("=== TABUADA COMPLETA ===\n\n");
    
    // Loop externo: números de 1 a 10
    for (int i = 1; i <= 10; i++) {
        printf("Tabuada do %d:\n", i);
        
        // Loop interno: multiplicadores de 1 a 10
        for (int j = 1; j <= 10; j++) {
            printf("%d x %d = %d\n", i, j, i * j);
        }
        
        printf("\n");  // Linha em branco entre tabuadas
    }
    
    return 0;
}
```

**Exemplo: Padrões com Asteriscos**
```c
#include <stdio.h>

int main() {
    int linhas;
    
    printf("Digite o número de linhas: ");
    scanf("%d", &linhas);
    
    printf("\nTriângulo:\n");
    for (int i = 1; i <= linhas; i++) {
        for (int j = 1; j <= i; j++) {
            printf("* ");
        }
        printf("\n");
    }
    
    printf("\nQuadrado:\n");
    for (int i = 1; i <= linhas; i++) {
        for (int j = 1; j <= linhas; j++) {
            printf("* ");
        }
        printf("\n");
    }
    
    return 0;
}
```

---

## 7. Exemplos Práticos Avançados

### Sistema de Banco com Loop
```c
#include <stdio.h>

int main() {
    float saldo = 1000.0;
    int opcao;
    
    printf("=== BANCO DIGITAL ===\n");
    
    do {
        printf("\nSaldo atual: R$ %.2f\n", saldo);
        printf("1. Depositar\n");
        printf("2. Sacar\n");
        printf("3. Extrato\n");
        printf("0. Sair\n");
        printf("Escolha: ");
        scanf("%d", &opcao);
        
        switch (opcao) {
            case 1: {
                float valor;
                printf("Valor para depositar: ");
                scanf("%f", &valor);
                
                if (valor > 0) {
                    saldo += valor;
                    printf("Depósito realizado!\n");
                } else {
                    printf("Valor inválido!\n");
                }
                break;
            }
                
            case 2: {
                float valor;
                printf("Valor para sacar: ");
                scanf("%f", &valor);
                
                if (valor > 0 && valor <= saldo) {
                    saldo -= valor;
                    printf("Saque realizado!\n");
                } else {
                    printf("Saldo insuficiente ou valor inválido!\n");
                }
                break;
            }
                
            case 3:
                printf("\n=== EXTRATO ===\n");
                printf("Saldo: R$ %.2f\n", saldo);
                break;
                
            case 0:
                printf("Obrigado por usar nossos serviços!\n");
                break;
                
            default:
                printf("Opção inválida!\n");
        }
        
    } while (opcao != 0);
    
    return 0;
}
```

### Jogo de Adivinhação
```c
#include <stdio.h>
#include <stdlib.h>
#include <time.h>

int main() {
    srand(time(NULL));  // Inicializa gerador de números aleatórios
    
    int numero_secreto = rand() % 100 + 1;  // Número entre 1 e 100
    int tentativa;
    int tentativas = 0;
    int max_tentativas = 7;
    
    printf("=== JOGO DA ADIVINHAÇÃO ===\n");
    printf("Estou pensando em um número entre 1 e 100.\n");
    printf("Você tem %d tentativas!\n\n", max_tentativas);
    
    while (tentativas < max_tentativas) {
        printf("Tentativa %d/%d: ", tentativas + 1, max_tentativas);
        scanf("%d", &tentativa);
        
        if (tentativa == numero_secreto) {
            printf("\n🎉 PARABÉNS! Você acertou em %d tentativas!\n", tentativas + 1);
            break;
        } else if (tentativa < numero_secreto) {
            printf("📈 Muito BAIXO! Tente um número maior.\n");
        } else {
            printf("📉 Muito ALTO! Tente um número menor.\n");
        }
        
        tentativas++;
        
        // Dica final
        if (tentativas == max_tentativas - 1) {
            printf("\n💡 DICA: O número está entre %d e %d\n", 
                   numero_secreto - 10, numero_secreto + 10);
        }
    }
    
    if (tentativas == max_tentativas) {
        printf("\n😞 Fim de jogo! O número era: %d\n", numero_secreto);
    }
    
    return 0;
}
```

---

## 8. Loops com Arrays

### Processando Coleções de Dados

```c
#include <stdio.h>

int main() {
    int notas[5];
    float soma = 0, media;
    
    printf("Digite 5 notas:\n");
    
    // Entrada de dados
    for (int i = 0; i < 5; i++) {
        printf("Nota %d: ", i + 1);
        scanf("%d", &notas[i]);
        soma += notas[i];
    }
    
    // Cálculo da média
    media = soma / 5;
    
    // Exibição dos resultados
    printf("\n=== RESULTADOS ===\n");
    printf("Notas: ");
    for (int i = 0; i < 5; i++) {
        printf("%d ", notas[i]);
    }
    printf("\nMédia: %.2f\n", media);
    
    // Notas acima da média
    printf("Notas acima da média: ");
    for (int i = 0; i < 5; i++) {
        if (notas[i] > media) {
            printf("%d ", notas[i]);
        }
    }
    printf("\n");
    
    return 0;
}
```

---

## 9. Boas Práticas e Dicas

### 1. Evite Loops Infinitos Acidentais
```c
// ⚠️ PERIGO: esquecer de atualizar a variável
int i = 0;
while (i < 10) {
    printf("%d\n", i);
    // i++;  // ESQUECIDO - loop infinito!
}

// ✅ CORRETO: sempre atualize
int i = 0;
while (i < 10) {
    printf("%d\n", i);
    i++;
}
```

### 2. Use FOR para Contagens Conhecidas
```c
// ✅ MELHOR: claro e organizado
for (int i = 0; i < 10; i++) {
    printf("%d\n", i);
}

// 🤔 FUNCIONA mas menos claro
int i = 0;
while (i < 10) {
    printf("%d\n", i);
    i++;
}
```

### 3. Prefira DO-WHILE para Validações
```c
// ✅ IDEAL: executa pelo menos uma vez
int numero;
do {
    printf("Digite um número positivo: ");
    scanf("%d", &numero);
} while (numero <= 0);
```

### 4. Use Nomes Significativos para Variáveis de Loop
```c
// ✅ BOM: nomes descritivos
for (int aluno = 0; aluno < total_alunos; aluno++) {
    for (int nota = 0; nota < total_notas; nota++) {
        // processa notas
    }
}

// ❌ RUIM: nomes genéricos
for (int i = 0; i < x; i++) {
    for (int j = 0; j < y; j++) {
        // difícil de entender
    }
}
```

---

## 10. Exercícios de Fixação

### Exercício 1: Fatorial
```c
#include <stdio.h>

int main() {
    int numero;
    long long fatorial = 1;
    
    printf("Digite um número para calcular o fatorial: ");
    scanf("%d", &numero);
    
    if (numero < 0) {
        printf("Fatorial não definido para números negativos.\n");
    } else {
        for (int i = 1; i <= numero; i++) {
            fatorial *= i;
        }
        printf("%d! = %lld\n", numero, fatorial);
    }
    
    return 0;
}
```

### Exercício 2: Série Fibonacci
```c
#include <stdio.h>

int main() {
    int n, primeiro = 0, segundo = 1, proximo;
    
    printf("Digite quantos termos da série Fibonacci: ");
    scanf("%d", &n);
    
    printf("Série Fibonacci: ");
    
    for (int i = 0; i < n; i++) {
        if (i <= 1) {
            proximo = i;
        } else {
            proximo = primeiro + segundo;
            primeiro = segundo;
            segundo = proximo;
        }
        printf("%d ", proximo);
    }
    printf("\n");
    
    return 0;
}
```

### Exercício 3: Números Primos
```c
#include <stdio.h>

int main() {
    int numero, eh_primo = 1;
    
    printf("Digite um número: ");
    scanf("%d", &numero);
    
    if (numero <= 1) {
        eh_primo = 0;
    } else {
        for (int i = 2; i * i <= numero; i++) {
            if (numero % i == 0) {
                eh_primo = 0;
                break;
            }
        }
    }
    
    if (eh_primo) {
        printf("%d é primo! ✅\n", numero);
    } else {
        printf("%d não é primo! ❌\n", numero);
    }
    
    return 0;
}
```

---

## 11. Tabela Resumo

| Loop | Quando Usar | Exemplo |
|------|-------------|---------|
| `while` | Condições complexas, repetições desconhecidas | `while (condicao) { ... }` |
| `do-while` | Menus, validações (executa ≥1 vez) | `do { ... } while (condicao);` |
| `for` | Contagens conhecidas, arrays | `for (int i=0; i<n; i++) { ... }` |
| `break` | Sair do loop imediatamente | `if (condicao) break;` |
| `continue` | Pular para próxima iteração | `if (condicao) continue;` |

**Dica Final:** Escolha a estrutura baseada no problema:
- **Sabemos quantas vezes repetir?** → Use `for`
- **Precisa executar pelo menos uma vez?** → Use `do-while`  
- **Condição complexa ou desconhecida?** → Use `while`

Domine esses conceitos e você terá o controle total sobre a execução repetitiva em seus programas! 🔄