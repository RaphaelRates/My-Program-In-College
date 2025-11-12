---
alias: [Endereços de Memória C]
tags: [c, ponteiros, memória, programação]
---

> [!note] ### 🧠 O que é Endereço de Memória?  
> Pense na memória do computador como uma **rua gigante com casas numeradas**.  
> Cada “casa” é uma posição de memória, e o número dela é o **endereço de memória**.

```c
#include <stdio.h>

int main() {
    int numero = 42;
    
    printf("Valor da variável: %d\n", numero);
    printf("Endereço da variável: %p\n", &numero);
    
    return 0;
}
```

**Saída:**

```
Valor da variável: 42
Endereço da variável: 0x7ffd42a1b2ac
```

> [!example] ### 📬 Analogia Prática: Correio
> 
> - **Variável** → A carta
>     
> - **Valor** → O conteúdo da carta (ex: “42”)
>     
> - **Endereço** → O endereço da casa onde a carta está guardada
>     
> - **Operador `&`** → Perguntar “onde mora esta carta?”
>     

---

## 🧩 Conceitos Fundamentais

### Variáveis, Valores e Endereços

```c
#include <stdio.h>

int main() {
    // Declaração de variáveis
    int idade = 25;
    float altura = 1.75;
    char letra = 'A';
    
    // Mostrando VALORES
    printf("=== VALORES ===\n");
    printf("Idade: %d\n", idade);
    printf("Altura: %.2f\n", altura);
    printf("Letra: %c\n", letra);
    
    // Mostrando ENDEREÇOS
    printf("\n=== ENDEREÇOS ===\n");
    printf("Idade mora em: %p\n", &idade);
    printf("Altura mora em: %p\n", &altura);
    printf("Letra mora em: %p\n", &letra);
    
    // Tamanho ocupado na memória
    printf("\n=== TAMANHOS ===\n");
    printf("int ocupa: %zu bytes\n", sizeof(idade));
    printf("float ocupa: %zu bytes\n", sizeof(altura));
    printf("char ocupa: %zu bytes\n", sizeof(letra));
    
    return 0;
}
```

---

## 🧠 Visualizando a Memória

### Mapa de Memória

```c
#include <stdio.h>

int main() {
    int a = 10;
    int b = 20;
    int c = 30;
    
    printf("=== MAPA DE MEMÓRIA ===\n");
    printf("| Variável | Valor | Endereço |\n");
    printf("|-----------|--------|------------------|\n");
    printf("| a | %2d | %p |\n", a, &a);
    printf("| b | %2d | %p |\n", b, &b);
    printf("| c | %2d | %p |\n", c, &c);
    
    printf("\n=== OBSERVAÇÕES ===\n");
    printf("Diferença entre endereços consecutivos: %ld bytes\n", (char*)&b - (char*)&a);
    
    return 0;
}
```

**Saída típica:**

```
=== MAPA DE MEMÓRIA ===
| Variável | Valor | Endereço         |
|-----------|--------|------------------|
| a         | 10     | 0x7ffd42a1b2ac   |
| b         | 20     | 0x7ffd42a1b2a8   |
| c         | 30     | 0x7ffd42a1b2a4   |

=== OBSERVAÇÕES ===
Diferença entre endereços consecutivos: 4 bytes
(Indicando que cada variável ocupa 4 bytes na memória)
```

---

> [!example] ### 🧱 Como a Memória é Organizada
> 
> ```
> ENDEREÇO    CONTEÚDO    VARIÁVEL
> 0x1000      30          c
> 0x1004      20          b
> 0x1008      10          a
> ```
> 
> - **Endereços** são números em **hexadecimal**
>     
> - **Cada variável** tem seu próprio endereço
>     
> - Variáveis do mesmo tipo geralmente têm **endereços consecutivos**
>     
> - A **stack cresce para baixo** (endereços menores)
>     

---

## 📋 Tabela Resumo

|Conceito|Definição|Exemplo|
|---|---|---|
|**Endereço**|Localização na memória|`0x7ffd42a1b2ac`|
|**Valor**|Dado armazenado|`42`|
|**Operador `&`**|Obtém o endereço de uma variável|`&variavel`|
|**Formatador `%p`**|Exibe o endereço na tela|`printf("%p", &x)`|

---

**Próximo:** [[C - Declaração e Inicialização de Ponteiros]]  
**Relacionados:** [[C - Tipos de Dados Primitivos]], [[C - Escopo de Variáveis]]
