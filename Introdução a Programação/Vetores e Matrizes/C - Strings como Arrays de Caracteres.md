## Introdução Simples

Em C, **strings são arrays de caracteres** terminados com o caractere especial `\0` (null character). Pense nelas como uma sequência de caracteres armazenados em posições consecutivas de memória, onde o último elemento sempre é zero para marcar o fim da string.

```c
// Exemplo básico
char nome[6] = {'M', 'a', 'r', 'i', 'a', '\0'};
// Ou de forma mais simples:
char nome[] = "Maria";  // O \0 é adicionado automaticamente
```

> [!note]
> ## Conceito Fundamental: Strings em C
> 
> Ao contrário de linguagens modernas que têm um tipo "string" dedicado, em C as strings são **implementadas como arrays de caracteres** terminados por um **caractere nulo** (`\0`). Esta abordagem de baixo nível oferece controle total, mas exige cuidado do programador para gerenciar memória e evitar erros comuns.

---

## 1. Declaração e Inicialização

### Formas de Criar Strings

```c
#include <stdio.h>

int main() {
    // Método 1: Array de caracteres com tamanho fixo
    char str1[20] = "Hello World";
    
    // Método 2: Array sem tamanho explícito (compiler calcula)
    char str2[] = "Hello World";  // Tamanho: 12 (11 chars + \0)
    
    // Método 3: Array de caracteres individualmente
    char str3[] = {'H', 'e', 'l', 'l', 'o', '\0'};
    
    // Método 4: Declaração sem inicialização
    char str4[50];
    
    // Método 5: Ponteiro para string literal
    char *str5 = "Hello World";  // Armazenada em memória readonly
    
    printf("str1: %s\n", str1);
    printf("str2: %s\n", str2);
    printf("str3: %s\n", str3);
    printf("str5: %s\n", str5);
    
    return 0;
}
```

> [!important]
> ### Diferença Crucial: Array vs Ponteiro
> 
> | Característica | `char str[] = "hello"` | `char *str = "hello"` |
> |----------------|------------------------|----------------------|
> | **Memória** | Array na stack | Ponteiro para memória readonly |
> | **Modificação** | ✅ Pode modificar | ❌ Não pode modificar |
> | **Tamanho** | `sizeof(str)` retorna tamanho do array | `sizeof(str)` retorna tamanho do ponteiro |
> | **Reatribuição** | ❌ Não pode reatribuir | ✅ Pode apontar para outra string |
> 
> ```c
> char array[] = "hello";
> char *ponteiro = "hello";
> 
> array[0] = 'H';     // ✅ VÁLIDO
> ponteiro[0] = 'H';  // ❌ ERRO - Segmentação fault
> ```

---

## 2. Funções Essenciais da Biblioteca string.h

### Principais Funções e Seu Uso

```c
#include <stdio.h>
#include <string.h>

int main() {
    char str1[20] = "Hello";
    char str2[20] = "World";
    char str3[30];
    
    // strlen - Comprimento da string (sem contar \0)
    printf("strlen: %zu\n", strlen(str1));  // 5
    
    // strcpy - Copiar string
    strcpy(str3, str1);
    printf("strcpy: %s\n", str3);  // Hello
    
    // strcat - Concatenar strings
    strcat(str1, " ");
    strcat(str1, str2);
    printf("strcat: %s\n", str1);  // Hello World
    
    // strcmp - Comparar strings
    printf("strcmp: %d\n", strcmp("abc", "abc"));  // 0 (iguais)
    printf("strcmp: %d\n", strcmp("abc", "abd"));  // -1 (primeira menor)
    printf("strcmp: %d\n", strcmp("abd", "abc"));  // 1 (primeira maior)
    
    // strchr - Encontrar caractere
    char *pos = strchr("Hello", 'e');
    if (pos != NULL) {
        printf("strchr: Encontrado 'e' na posição %ld\n", pos - "Hello");
    }
    
    return 0;
}
```

> [!warning]
> ### Cuidado com Buffer Overflow!
> 
> As funções tradicionais como `strcpy` e `strcat` não verificam limites. Use as versões seguras:
> 
> ```c
> char dest[10];
> char src[] = "Esta string é muito longa";
> 
> // ❌ PERIGOSO - Pode causar buffer overflow
> strcpy(dest, src);
> 
> // ✅ SEGURO - Especifica tamanho máximo
> strncpy(dest, src, sizeof(dest) - 1);
> dest[sizeof(dest) - 1] = '\0';  // Garante terminação
> 
> // ✅ AINDA MELHOR - Funções seguras (C11)
> strcpy_s(dest, sizeof(dest), src);
> ```

---

## 3. Percorrendo e Manipulando Strings

### Iteração Caractere por Caractere

```c
#include <stdio.h>
#include <ctype.h>

void manipular_string(char *str) {
    printf("String original: %s\n", str);
    
    // Contar caracteres
    int count = 0;
    while (str[count] != '\0') {
        count++;
    }
    printf("Comprimento manual: %d\n", count);
    
    // Converter para maiúsculas
    for (int i = 0; str[i] != '\0'; i++) {
        str[i] = toupper(str[i]);
    }
    printf("Em maiúsculas: %s\n", str);
    
    // Inverter string
    int inicio = 0;
    int fim = count - 1;
    while (inicio < fim) {
        char temp = str[inicio];
        str[inicio] = str[fim];
        str[fim] = temp;
        inicio++;
        fim--;
    }
    printf("Invertida: %s\n", str);
}

int main() {
    char texto[] = "Hello World";
    manipular_string(texto);
    return 0;
}
```

> [!example]
> ### Exemplo Prático: Processador de Texto Simples
> 
> ```c
> #include <stdio.h>
> #include <string.h>
> #include <ctype.h>
> 
> void processar_texto(char *texto) {
>     int palavras = 0, letras = 0, espacos = 0;
>     int i = 0;
>     
>     while (texto[i] != '\0') {
>         // Contar letras
>         if (isalpha(texto[i])) {
>             letras++;
>         }
>         // Contar espaços e detectar palavras
>         if (isspace(texto[i])) {
>             espacos++;
>             // Se próximo caractere não é espaço, é início de palavra
>             if (i > 0 && !isspace(texto[i-1])) {
>                 palavras++;
>             }
>         }
>         i++;
>     }
>     // Última palavra (se não terminar com espaço)
>     if (i > 0 && !isspace(texto[i-1])) {
>         palavras++;
>     }
>     
>     printf("Estatísticas do texto:\n");
>     printf("Caracteres totais: %d\n", i);
>     printf("Letras: %d\n", letras);
>     printf("Espaços: %d\n", espacos);
>     printf("Palavras: %d\n", palavras);
> }
> 
> int main() {
>     char texto[] = "C programming is powerful and efficient";
>     processar_texto(texto);
>     return 0;
> }
> ```

---

## 4. Entrada e Saída de Strings

### Lendo e Escrevendo Strings

```c
#include <stdio.h>

int main() {
    char nome[50];
    char frase[100];
    
    // Leitura básica com scanf
    printf("Digite seu nome: ");
    scanf("%49s", nome);  // Limita a 49 caracteres + \0
    printf("Nome: %s\n", nome);
    
    // Limpar buffer de entrada
    int c;
    while ((c = getchar()) != '\n' && c != EOF);
    
    // Leitura de linha inteira com fgets
    printf("Digite uma frase: ");
    fgets(frase, sizeof(frase), stdin);
    
    // Remover \n do final se existir
    int len = strlen(frase);
    if (len > 0 && frase[len-1] == '\n') {
        frase[len-1] = '\0';
    }
    
    printf("Frase: %s\n", frase);
    
    // Escrita com puts (adiciona \n automaticamente)
    puts("=== Usando puts ===");
    puts(nome);
    puts(frase);
    
    return 0;
}
```

> [!tip]
> ### Dicas de Entrada/Saída
> 
> **Problemas comuns e soluções:**
> 
> 1. **scanf para strings:** Para apenas uma palavra
> 2. **fgets:** Para linhas completas, mas captura o `\n`
> 3. **gets:** ❌ **NUNCA USE** - Extremamente perigoso
> 
> **Alternativas seguras:**
> ```c
> // Ler linha segura
> char buffer[100];
> printf("Entrada: ");
> if (fgets(buffer, sizeof(buffer), stdin) != NULL) {
>     // Remover \n se necessário
>     buffer[strcspn(buffer, "\n")] = '\0';
> }
> 
> // Ou criar função helper
> void ler_linha_segura(char *buffer, int tamanho) {
>     if (fgets(buffer, tamanho, stdin) != NULL) {
>         buffer[strcspn(buffer, "\n")] = '\0';
>     }
> }
> ```

---

## 5. Arrays de Strings (Matriz de Caracteres)

### Trabalhando com Múltiplas Strings

```c
#include <stdio.h>
#include <string.h>

int main() {
    // Array de strings - duas dimensões
    char nomes[5][50] = {
        "Ana Silva",
        "Carlos Santos", 
        "Maria Oliveira",
        "João Pereira",
        "Lucia Costa"
    };
    
    // Percorrer array de strings
    printf("Lista de nomes:\n");
    for (int i = 0; i < 5; i++) {
        printf("%d: %s\n", i + 1, nomes[i]);
    }
    
    // Buscar nome específico
    char busca[50];
    printf("\nDigite um nome para buscar: ");
    scanf("%49s", busca);
    
    int encontrado = 0;
    for (int i = 0; i < 5; i++) {
        if (strstr(nomes[i], busca) != NULL) {
            printf("Encontrado: %s\n", nomes[i]);
            encontrado = 1;
        }
    }
    
    if (!encontrado) {
        printf("Nome não encontrado.\n");
    }
    
    return 0;
}
```

> [!note]
> ### Arrays 2D vs Array de Ponteiros
> 
> **Duas abordagens para arrays de strings:**
> 
> ```c
> // Método 1: Array bidimensional (matriz)
> char nomes[5][50];  // 5 strings de até 49 caracteres
> // Vantagem: Memória contígua, fácil gerenciamento
> // Desvantagem: Tamanho fixo, pode desperdiçar espaço
> 
> // Método 2: Array de ponteiros
> char *nomes[] = {"Ana", "Carlos", "Maria", NULL};
> // Vantagem: Strings de tamanhos diferentes, eficiente
> // Desvantagem: Strings em memória readonly (se literais)
> 
> // Método 3: Array de ponteiros com alocação dinâmica
> char **nomes = malloc(5 * sizeof(char*));
> for (int i = 0; i < 5; i++) {
>     nomes[i] = malloc(50 * sizeof(char));
> }
> // Mais flexível mas requer gerenciamento manual de memória
> ```

---

## 6. Funções Avançadas e Manipulação Complexa

### Implementando Funções Úteis

```c
#include <stdio.h>
#include <string.h>
#include <ctype.h>

// Função para contar ocorrências de um caractere
int contar_caractere(const char *str, char c) {
    int count = 0;
    for (int i = 0; str[i] != '\0'; i++) {
        if (str[i] == c) {
            count++;
        }
    }
    return count;
}

// Função para extrair substring
int substring(const char *origem, int inicio, int comprimento, char *destino) {
    int len_origem = strlen(origem);
    
    // Validar parâmetros
    if (inicio < 0 || inicio >= len_origem || comprimento <= 0) {
        destino[0] = '\0';
        return 0;
    }
    
    // Ajustar comprimento se necessário
    if (inicio + comprimento > len_origem) {
        comprimento = len_origem - inicio;
    }
    
    // Copiar substring
    strncpy(destino, &origem[inicio], comprimento);
    destino[comprimento] = '\0';
    
    return comprimento;
}

// Função para remover espaços extras
void remover_espacos_extras(char *str) {
    int i = 0, j = 0;
    int espaco_anterior = 0;
    
    while (str[i] != '\0') {
        if (!isspace(str[i])) {
            str[j++] = str[i];
            espaco_anterior = 0;
        } else {
            if (!espaco_anterior && j > 0) {
                str[j++] = ' ';  // Manter um espaço
            }
            espaco_anterior = 1;
        }
        i++;
    }
    
    // Remover espaço final se existir
    if (j > 0 && isspace(str[j-1])) {
        j--;
    }
    
    str[j] = '\0';
}

int main() {
    char texto[] = "   C   programming   is   fun!   ";
    char resultado[100];
    
    printf("Original: '%s'\n", texto);
    
    // Contar espaços
    printf("Espaços: %d\n", contar_caractere(texto, ' '));
    
    // Extrair substring
    substring(texto, 5, 11, resultado);
    printf("Substring(5,11): '%s'\n", resultado);
    
    // Remover espaços extras
    remover_espacos_extras(texto);
    printf("Sem espaços extras: '%s'\n", texto);
    
    return 0;
}
```

---

## 7. Tabela de Funções de String Comuns

### Referência Rápida

| Função | Protótipo | Descrição | Exemplo |
|--------|-----------|-----------|---------|
| **strlen** | `size_t strlen(const char *s)` | Comprimento da string | `strlen("hello") = 5` |
| **strcpy** | `char* strcpy(char *dest, const char *src)` | Copia string | `strcpy(dest, "hello")` |
| **strncpy** | `char* strncpy(char *dest, const char *src, size_t n)` | Copia até n caracteres | `strncpy(dest, src, 10)` |
| **strcat** | `char* strcat(char *dest, const char *src)` | Concatena strings | `strcat(str, " world")` |
| **strncat** | `char* strncat(char *dest, const char *src, size_t n)` | Concatena até n caracteres | `strncat(str, add, 5)` |
| **strcmp** | `int strcmp(const char *s1, const char *s2)` | Compara strings | `strcmp("a", "b") = -1` |
| **strncmp** | `int strncmp(const char *s1, const char *s2, size_t n)` | Compara até n caracteres | `strncmp(s1, s2, 3)` |
| **strchr** | `char* strchr(const char *s, int c)` | Encontra primeira ocorrência de c | `strchr("hello", 'l')` |
| **strstr** | `char* strstr(const char *haystack, const char *needle)` | Encontra substring | `strstr("hello", "ell")` |
| **sprintf** | `int sprintf(char *str, const char *format, ...)` | Formata string | `sprintf(buf, "%d", 123)` |

---

## 8. Exercícios Práticos

### Para Fixar o Conhecimento

```c
#include <stdio.h>
#include <string.h>
#include <ctype.h>

// Exercício 1: Verificar palíndromo
int eh_palindromo(const char *str) {
    int inicio = 0;
    int fim = strlen(str) - 1;
    
    while (inicio < fim) {
        if (str[inicio] != str[fim]) {
            return 0;  // Não é palíndromo
        }
        inicio++;
        fim--;
    }
    return 1;  // É palíndromo
}

// Exercício 2: Contar vogais
int contar_vogais(const char *str) {
    int count = 0;
    for (int i = 0; str[i] != '\0'; i++) {
        char c = tolower(str[i]);
        if (c == 'a' || c == 'e' || c == 'i' || c == 'o' || c == 'u') {
            count++;
        }
    }
    return count;
}

// Exercício 3: Inverter palavras em uma frase
void inverter_palavras(char *frase) {
    // Implementação deixada como exercício
    // Dica: inverter string toda, depois inverter cada palavra individualmente
}

int main() {
    // Teste dos exercícios
    char teste1[] = "radar";
    char teste2[] = "Hello World";
    
    printf("'%s' é palíndromo? %s\n", teste1, 
           eh_palindromo(teste1) ? "Sim" : "Não");
    
    printf("Vogais em '%s': %d\n", teste2, contar_vogais(teste2));
    
    return 0;
}
```

> [!success]
> ## Conclusão
> 
> Strings em C, embora sejam conceitualmente simples (apenas arrays de caracteres), oferecem um poder e controle impressionantes. A chave para dominá-las é:
> 
> 1. **Sempre lembrar do `\0`** - o terminador nulo
> 2. **Gerenciar memória cuidadosamente** - evitar buffer overflows
> 3. **Usar funções da biblioteca padrão** - mas entender seus limites
> 4. **Praticar manipulação manual** - para entender o que acontece nos bastidores
> 
> Com prática, você desenvolverá a intuição necessária para trabalhar eficientemente com strings em C, uma habilidade fundamental para qualquer programador de sistemas.