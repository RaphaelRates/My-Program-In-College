# Bibliotecas Padrão

> [!note] 
> Aqui darei exemplos de usos de bibliotecas padrão da linguagem C, também chamada de libs.
> 
> Toda linguagem de programação possui recursos já prontos para os desenvolvedores usares, esse recursos são Constantes, funções, estruturas, e dependendo da linguagem (se ela tiver o paradigma de [[Programação Orientada a Objetos]]) classes e interfaces. 

## 1. stdio.h - Entrada e Saída

### Funções de Saída

```c
#include <stdio.h>

int main() {
    // printf - Saída formatada
    int idade = 25;
    float altura = 1.75;
    char nome[] = "João";
    
    printf("Nome: %s, Idade: %d, Altura: %.2f\n", nome, idade, altura);
    
    // Especificadores de formato comuns:
    // %d - inteiro, %f - float, %c - char, %s - string
    // %x - hexadecimal, %o - octal, %p - ponteiro
    
    // puts - Saída simples com \n automático
    puts("Texto com quebra de linha automática");
    
    // putchar - Imprime um caractere
    putchar('A');
    putchar('\n');
    
    // fprintf - Saída para arquivo
    FILE *arquivo = fopen("saida.txt", "w");
    if (arquivo != NULL) {
        fprintf(arquivo, "Escrevendo no arquivo: %s\n", nome);
        fclose(arquivo);
    }
    
    // sprintf - Saída para string
    char buffer[100];
    sprintf(buffer, "Nome: %s, Idade: %d", nome, idade);
    printf("Buffer: %s\n", buffer);
    
    return 0;
}
```

### Funções de Entrada

```c
#include <stdio.h>

int main() {
    int numero;
    float decimal;
    char caractere;
    char palavra[20];
    char linha[100];
    
    // scanf - Entrada formatada
    printf("Digite um número inteiro: ");
    scanf("%d", &numero);
    
    printf("Digite um decimal: ");
    scanf("%f", &decimal);
    
    // Limpeza de buffer
    while (getchar() != '\n');  // Limpa o \n deixado pelo scanf
    
    printf("Digite um caractere: ");
    scanf("%c", &caractere);
    
    printf("Digite uma palavra: ");
    scanf("%19s", palavra);  // Limita a 19 caracteres + \0
    
    // Limpeza de buffer novamente
    while (getchar() != '\n');
    
    // fgets - Entrada de linha completa
    printf("Digite uma frase: ");
    fgets(linha, sizeof(linha), stdin);
    
    // Remove o \n do final se existir
    if (linha[strlen(linha) - 1] == '\n') {
        linha[strlen(linha) - 1] = '\0';
    }
    
    printf("Você digitou: '%s'\n", linha);
    
    // getchar - Lê um caractere
    printf("Pressione uma tecla: ");
    int c = getchar();
    printf("Caractere: %c (ASCII: %d)\n", c, c);
    
    // fscanf - Leitura formatada de arquivo
    FILE *arquivo = fopen("dados.txt", "r");
    if (arquivo != NULL) {
        char nome_arquivo[50];
        int idade_arquivo;
        fscanf(arquivo, "%s %d", nome_arquivo, &idade_arquivo);
        printf("Do arquivo: %s, %d anos\n", nome_arquivo, idade_arquivo);
        fclose(arquivo);
    }
    
    return 0;
}
```

### Manipulação de Arquivos

```c
#include <stdio.h>

int main() {
    FILE *arquivo;
    char texto[] = "Hello File World!\nSegunda linha.\n";
    char buffer[100];
    
    // Escrita em arquivo
    arquivo = fopen("exemplo.txt", "w");
    if (arquivo == NULL) {
        perror("Erro ao abrir arquivo");
        return 1;
    }
    
    fputs(texto, arquivo);
    fclose(arquivo);
    
    // Leitura de arquivo
    arquivo = fopen("exemplo.txt", "r");
    if (arquivo == NULL) {
        perror("Erro ao abrir arquivo");
        return 1;
    }
    
    printf("Conteúdo do arquivo:\n");
    while (fgets(buffer, sizeof(buffer), arquivo) != NULL) {
        printf("%s", buffer);
    }
    fclose(arquivo);
    
    // Modo append (adicionar ao final)
    arquivo = fopen("exemplo.txt", "a");
    fputs("Terceira linha (append).\n", arquivo);
    fclose(arquivo);
    
    // Leitura binária
    int dados[] = {10, 20, 30, 40, 50};
    arquivo = fopen("binario.bin", "wb");
    fwrite(dados, sizeof(int), 5, arquivo);
    fclose(arquivo);
    
    // Leitura binária
    int lidos[5];
    arquivo = fopen("binario.bin", "rb");
    fread(lidos, sizeof(int), 5, arquivo);
    fclose(arquivo);
    
    printf("Dados lidos: ");
    for (int i = 0; i < 5; i++) {
        printf("%d ", lidos[i]);
    }
    printf("\n");
    
    return 0;
}
```

## 2. stdlib.h - Utilitários Gerais

### Conversão de Strings

```c
#include <stdlib.h>
#include <stdio.h>
#include <errno.h>

int main() {
    // Conversão para inteiros
    char str_int[] = "12345";
    char str_int_hex[] = "1A3F";
    char str_int_invalida[] = "123abc";
    
    int num1 = atoi(str_int);                    // String para int
    long num2 = atol(str_int);                   // String para long
    long num3 = strtol(str_int_hex, NULL, 16);   // Hexadecimal
    long num4 = strtol(str_int_invalida, NULL, 10);
    
    printf("atoi: %d\n", num1);
    printf("strtol hex: %ld\n", num3);
    printf("strtol inválida: %ld (erro: %d)\n", num4, errno);
    
    // Conversão para floats
    char str_float[] = "3.14159";
    char str_float_cien[] = "2.5e2";
    
    float f1 = atof(str_float);                  // String para float
    double d1 = strtod(str_float_cien, NULL);    // String para double
    
    printf("atof: %.5f\n", f1);
    printf("strtod científico: %.1f\n", d1);
    
    // Conversão para unsigned
    char str_unsigned[] = "255";
    unsigned long u1 = strtoul(str_unsigned, NULL, 10);
    printf("strtoul: %lu\n", u1);
    
    return 0;
}
```

### Alocação Dinâmica de Memória

```c
#include <stdlib.h>
#include <stdio.h>
#include <string.h>

int main() {
    // malloc - Alocação básica
    int *array1 = (int*)malloc(5 * sizeof(int));
    if (array1 == NULL) {
        printf("Erro na alocação!\n");
        return 1;
    }
    
    for (int i = 0; i < 5; i++) {
        array1[i] = i * 10;
    }
    
    printf("Array malloc: ");
    for (int i = 0; i < 5; i++) {
        printf("%d ", array1[i]);
    }
    printf("\n");
    
    // calloc - Alocação com inicialização zero
    int *array2 = (int*)calloc(5, sizeof(int));
    printf("Array calloc (inicializado com 0): ");
    for (int i = 0; i < 5; i++) {
        printf("%d ", array2[i]);
    }
    printf("\n");
    
    // realloc - Realocação
    array1 = (int*)realloc(array1, 10 * sizeof(int));
    for (int i = 5; i < 10; i++) {
        array1[i] = i * 10;
    }
    
    printf("Array após realloc: ");
    for (int i = 0; i < 10; i++) {
        printf("%d ", array1[i]);
    }
    printf("\n");
    
    // Liberação de memória
    free(array1);
    free(array2);
    
    // Alocação de strings
    char *str = (char*)malloc(50 * sizeof(char));
    if (str != NULL) {
        strcpy(str, "String alocada dinamicamente");
        printf("%s\n", str);
        free(str);
    }
    
    return 0;
}
```

### Controle de Processo e Ambiente

```c
#include <stdlib.h>
#include <stdio.h>

int main() {
    // Números aleatórios
    printf("Números aleatórios:\n");
    srand(123);  // Semente fixa para resultados reproduzíveis
    
    for (int i = 0; i < 5; i++) {
        printf("%d ", rand() % 100);  // 0-99
    }
    printf("\n");
    
    // Semente baseada no tempo (mais aleatório)
    // srand(time(NULL));
    
    // Variáveis de ambiente
    char *path = getenv("PATH");
    if (path != NULL) {
        printf("PATH: %s\n", path);
    }
    
    // setenv - Definir variável de ambiente (não padrão C, comum em Unix)
    // setenv("MINHA_VAR", "meu_valor", 1);
    
    // system - Executar comando do sistema
    int resultado = system("echo Hello from system command!");
    printf("Comando system retornou: %d\n", resultado);
    
    // exit e atexit
    void funcao_saida() {
        printf("Executando antes de sair...\n");
    }
    
    atexit(funcao_saida);
    
    printf("Programa terminando...\n");
    // exit(0);  // Saída imediata
    
    // abort() - Terminação anormal
    // abort();
    
    return 0;
}
```

### Busca e Ordenação

```c
#include <stdlib.h>
#include <stdio.h>
#include <string.h>

// Função de comparação para inteiros
int comparar_int(const void *a, const void *b) {
    return (*(int*)a - *(int*)b);
}

// Função de comparação para strings
int comparar_str(const void *a, const void *b) {
    return strcmp(*(const char**)a, *(const char**)b);
}

int main() {
    // Ordenação com qsort
    int numeros[] = {64, 34, 25, 12, 22, 11, 90};
    int n = sizeof(numeros) / sizeof(numeros[0]);
    
    printf("Array original: ");
    for (int i = 0; i < n; i++) {
        printf("%d ", numeros[i]);
    }
    printf("\n");
    
    qsort(numeros, n, sizeof(int), comparar_int);
    
    printf("Array ordenado: ");
    for (int i = 0; i < n; i++) {
        printf("%d ", numeros[i]);
    }
    printf("\n");
    
    // Ordenação de strings
    const char *nomes[] = {"Zeca", "Ana", "Carlos", "Bruno"};
    int n_nomes = sizeof(nomes) / sizeof(nomes[0]);
    
    qsort(nomes, n_nomes, sizeof(char*), comparar_str);
    
    printf("Nomes ordenados: ");
    for (int i = 0; i < n_nomes; i++) {
        printf("%s ", nomes[i]);
    }
    printf("\n");
    
    // Busca binária
    int chave = 22;
    int *item = (int*)bsearch(&chave, numeros, n, sizeof(int), comparar_int);
    
    if (item != NULL) {
        printf("Encontrado: %d\n", *item);
    } else {
        printf("Não encontrado: %d\n", chave);
    }
    
    return 0;
}
```

## 3. string.h - Manipulação de Strings

### Funções Básicas

```c
#include <string.h>
#include <stdio.h>

int main() {
    char str1[50] = "Hello";
    char str2[50] = "World";
    char str3[50];
    char str4[] = "Hello World, Welcome to C Programming!";
    
    // strlen - Comprimento da string
    printf("strlen('%s') = %zu\n", str1, strlen(str1));
    
    // strcpy - Copiar string
    strcpy(str3, str1);
    printf("strcpy: %s\n", str3);
    
    // strncpy - Copiar com limite
    strncpy(str3, "ABCDEFGHIJ", 5);
    str3[5] = '\0';  // Importante adicionar null terminator
    printf("strncpy: %s\n", str3);
    
    // strcat - Concatenar
    strcpy(str3, str1);
    strcat(str3, " ");
    strcat(str3, str2);
    printf("strcat: %s\n", str3);
    
    // strncat - Concatenar com limite
    strcpy(str3, "Start");
    strncat(str3, " and this is too long", 8);
    printf("strncat: %s\n", str3);
    
    // strcmp - Comparação
    printf("strcmp('%s', '%s') = %d\n", str1, str2, strcmp(str1, str2));
    printf("strcmp('%s', '%s') = %d\n", str1, "Hello", strcmp(str1, "Hello"));
    
    // strncmp - Comparação com limite
    printf("strncmp('%s', '%s', 3) = %d\n", "ABC", "ABD", strncmp("ABC", "ABD", 3));
    
    return 0;
}
```

### Funções de Busca

```c
#include <string.h>
#include <stdio.h>

int main() {
    char texto[] = "The quick brown fox jumps over the lazy dog";
    char *resultado;
    
    // strchr - Encontrar primeira ocorrência do caractere
    resultado = strchr(texto, 'q');
    if (resultado != NULL) {
        printf("strchr 'q': %s\n", resultado);
    }
    
    // strrchr - Encontrar última ocorrência do caractere
    resultado = strrchr(texto, 'o');
    if (resultado != NULL) {
        printf("strrchr 'o': %s\n", resultado);
    }
    
    // strstr - Encontrar substring
    resultado = strstr(texto, "brown");
    if (resultado != NULL) {
        printf("strstr 'brown': %s\n", resultado);
    }
    
    // strpbrk - Encontrar qualquer caractere do conjunto
    resultado = strpbrk(texto, "aeiou");
    if (resultado != NULL) {
        printf("strpbrk vogal: %s\n", resultado);
    }
    
    // strspn - Comprimento do prefixo contendo apenas caracteres do conjunto
    size_t comprimento = strspn(texto, "The quick");
    printf("strspn: %zu\n", comprimento);
    
    // strcspn - Comprimento do prefixo sem caracteres do conjunto
    comprimento = strcspn(texto, "0123456789");
    printf("strcspn dígitos: %zu\n", comprimento);
    
    // strtok - Tokenização (quebra em tokens)
    char frase[] = "maçã,banana,uva;laranja";
    char *token = strtok(frase, ",;");
    
    printf("Tokens: ");
    while (token != NULL) {
        printf("'%s' ", token);
        token = strtok(NULL, ",;");
    }
    printf("\n");
    
    return 0;
}
```

### Funções de Memória e Versões Seguras

```c
#include <string.h>
#include <stdio.h>

int main() {
    // memcpy - Copiar memória
    int src[] = {1, 2, 3, 4, 5};
    int dest[5];
    
    memcpy(dest, src, 5 * sizeof(int));
    printf("memcpy: ");
    for (int i = 0; i < 5; i++) {
        printf("%d ", dest[i]);
    }
    printf("\n");
    
    // memmove - Copiar memória com overlapp
    char str[] = "abcdefghij";
    memmove(str + 3, str, 5);
    printf("memmove: %s\n", str);
    
    // memcmp - Comparar memória
    int arr1[] = {1, 2, 3};
    int arr2[] = {1, 2, 4};
    printf("memcmp: %d\n", memcmp(arr1, arr2, 3 * sizeof(int)));
    
    // memset - Preencher memória com valor
    char buffer[20];
    memset(buffer, 'A', 10);
    buffer[10] = '\0';
    printf("memset: %s\n", buffer);
    
    // memchr - Encontrar caractere na memória
    char *pos = (char*)memchr(texto, 'x', strlen(texto));
    if (pos != NULL) {
        printf("memchr 'x': Encontrado na posição %ld\n", pos - texto);
    }
    
    // Versões seguras (C11)
    char dest_seguro[10];
    // strcpy_s(dest_seguro, sizeof(dest_seguro), "Hello"); // C11
    
    return 0;
}
```

## 4. math.h - Matemática

### Funções Básicas e Potências

```c
#include <math.h>
#include <stdio.h>
#include <errno.h>

int main() {
    double x = 4.0;
    double y = 2.5;
    double z = -3.7;
    
    // Potências e raízes
    printf("pow(%.1f, %.1f) = %.2f\n", x, y, pow(x, y));
    printf("sqrt(%.1f) = %.2f\n", x, sqrt(x));
    printf("cbrt(8.0) = %.2f\n", cbrt(8.0));        // Raiz cúbica
    printf("hypot(3, 4) = %.2f\n", hypot(3, 4));    // Hipotenusa
    
    // Exponenciais e logaritmos
    printf("exp(1.0) = %.2f (e^1)\n", exp(1.0));    // e^x
    printf("log(2.718) = %.2f (ln)\n", log(2.718)); // Log natural
    printf("log10(100) = %.2f\n", log10(100));      // Log base 10
    printf("log2(8) = %.2f\n", log2(8));            // Log base 2
    
    // Arredondamento
    printf("ceil(%.1f) = %.1f\n", y, ceil(y));      // Teto
    printf("floor(%.1f) = %.1f\n", y, floor(y));    // Piso
    printf("round(%.1f) = %.1f\n", y, round(y));    // Arredonda
    printf("trunc(%.1f) = %.1f\n", y, trunc(y));    // Trunca
    
    // Valor absoluto e resto
    printf("fabs(%.1f) = %.1f\n", z, fabs(z));      // Valor absoluto
    printf("fmod(7.5, 3.2) = %.2f\n", fmod(7.5, 3.2)); // Resto divisão
    
    return 0;
}
```

### Trigonometria

```c
#include <math.h>
#include <stdio.h>

int main() {
    double angulo_graus = 45.0;
    double angulo_radianos = angulo_graus * M_PI / 180.0;
    
    printf("Ângulo: %.0f° (%.2f rad)\n", angulo_graus, angulo_radianos);
    
    // Funções trigonométricas básicas
    printf("sin(%.2f) = %.3f\n", angulo_radianos, sin(angulo_radianos));
    printf("cos(%.2f) = %.3f\n", angulo_radianos, cos(angulo_radianos));
    printf("tan(%.2f) = %.3f\n", angulo_radianos, tan(angulo_radianos));
    
    // Funções trigonométricas inversas
    printf("asin(0.707) = %.2f rad\n", asin(0.707));
    printf("acos(0.707) = %.2f rad\n", acos(0.707));
    printf("atan(1.0) = %.2f rad\n", atan(1.0));
    
    // atan2 - arco tangente de y/x
    printf("atan2(1, 1) = %.2f rad (45°)\n", atan2(1, 1));
    
    // Funções hiperbólicas
    printf("sinh(1.0) = %.3f\n", sinh(1.0));
    printf("cosh(1.0) = %.3f\n", cosh(1.0));
    printf("tanh(1.0) = %.3f\n", tanh(1.0));
    
    return 0;
}
```

### Funções de Classificação e Erros

```c
#include <math.h>
#include <stdio.h>
#include <errno.h>

int main() {
    double valido = 25.0;
    double invalido = -1.0;
    double infinito = 1.0 / 0.0;  // Infinito
    double nan_val = 0.0 / 0.0;    // NaN
    
    // Classificação de números
    printf("isfinite(%.1f): %d\n", valido, isfinite(valido));
    printf("isinf(infinito): %d\n", isinf(infinito));
    printf("isnan(nan): %d\n", isnan(nan_val));
    printf("isnormal(%.1f): %d\n", valido, isnormal(valido));
    
    // Teste de erro com sqrt
    errno = 0;
    double resultado = sqrt(invalido);
    
    if (errno != 0) {
        perror("Erro na função sqrt");
    } else if (isnan(resultado)) {
        printf("sqrt(-1) = NaN\n");
    }
    
    // Cópia de sinal
    printf("copysign(5.0, -1.0) = %.1f\n", copysign(5.0, -1.0));
    
    // Diferença absoluta
    printf("fdim(5.0, 3.0) = %.1f\n", fdim(5.0, 3.0));  // max(0, x-y)
    
    // Múltiplo mais próximo
    printf("remainder(10.5, 3.0) = %.1f\n", remainder(10.5, 3.0));
    
    return 0;
}
```

## 5. ctype.h - Classificação de Caracteres

```c
#include <ctype.h>
#include <stdio.h>

int main() {
    char chars[] = {'A', 'b', '5', ' ', '!', '\n', '0'};
    int n = sizeof(chars) / sizeof(chars[0]);
    
    printf("Teste de classificação de caracteres:\n");
    for (int i = 0; i < n; i++) {
        printf("'%c' (ASCII %d): ", chars[i], chars[i]);
        
        if (isalnum(chars[i])) printf("isalnum ");
        if (isalpha(chars[i])) printf("isalpha ");
        if (islower(chars[i])) printf("islower ");
        if (isupper(chars[i])) printf("isupper ");
        if (isdigit(chars[i])) printf("isdigit ");
        if (isxdigit(chars[i])) printf("isxdigit ");
        if (iscntrl(chars[i])) printf("iscntrl ");
        if (isgraph(chars[i])) printf("isgraph ");
        if (isspace(chars[i])) printf("isspace ");
        if (isblank(chars[i])) printf("isblank ");
        if (isprint(chars[i])) printf("isprint ");
        if (ispunct(chars[i])) printf("ispunct ");
        
        printf("\n");
    }
    
    printf("\nConversão de caracteres:\n");
    for (int i = 0; i < n; i++) {
        printf("'%c' -> toupper: '%c'", chars[i], toupper(chars[i]));
        printf(", tolower: '%c'\n", tolower(chars[i]));
    }
    
    // Exemplo prático: converter string para minúsculas
    char texto[] = "Hello World 123!";
    printf("\nOriginal: %s\n", texto);
    
    printf("Minúsculas: ");
    for (int i = 0; texto[i] != '\0'; i++) {
        putchar(tolower(texto[i]));
    }
    printf("\n");
    
    printf("Maiúsculas: ");
    for (int i = 0; texto[i] != '\0'; i++) {
        putchar(toupper(texto[i]));
    }
    printf("\n");
    
    return 0;
}
```

## 6. time.h - Data e Hora

```c
#include <time.h>
#include <stdio.h>

int main() {
    // time - Tempo atual em segundos desde a época
    time_t agora = time(NULL);
    printf("Segundos desde 1/1/1970: %ld\n", agora);
    
    // ctime - String legível da data/hora
    printf("Data/hora atual: %s", ctime(&agora));
    
    // localtime - Tempo local decomposto
    struct tm *tm_local = localtime(&agora);
    printf("Local: %02d:%02d:%02d %02d/%02d/%04d\n",
           tm_local->tm_hour, tm_local->tm_min, tm_local->tm_sec,
           tm_local->tm_mday, tm_local->tm_mon + 1, tm_local->tm_year + 1900);
    
    // gmtime - Tempo UTC decomposto
    struct tm *tm_utc = gmtime(&agora);
    printf("UTC: %02d:%02d:%02d %02d/%02d/%04d\n",
           tm_utc->tm_hour, tm_utc->tm_min, tm_utc->tm_sec,
           tm_utc->tm_mday, tm_utc->tm_mon + 1, tm_utc->tm_year + 1900);
    
    // mktime - Converte struct tm para time_t
    struct tm data_manual = {0};
    data_manual.tm_year = 124;  // 2024 - 1900
    data_manual.tm_mon = 0;     // Janeiro (0-11)
    data_manual.tm_mday = 1;    // Dia 1
    data_manual.tm_hour = 12;   // 12:00
    
    time_t data_custom = mktime(&data_manual);
    printf("Data customizada: %s", ctime(&data_custom));
    
    // strftime - Formatação customizada
    char buffer[100];
    strftime(buffer, sizeof(buffer), "Hoje é %A, %d de %B de %Y", tm_local);
    printf("%s\n", buffer);
    
    strftime(buffer, sizeof(buffer), "Hora: %H:%M:%S", tm_local);
    printf("%s\n", buffer);
    
    // clock - Tempo de CPU
    clock_t inicio = clock();
    
    // Simula algum processamento
    for (long i = 0; i < 10000000; i++);
    
    clock_t fim = clock();
    double tempo_cpu = ((double)(fim - inicio)) / CLOCKS_PER_SEC;
    printf("Tempo de CPU: %.4f segundos\n", tempo_cpu);
    
    // difftime - Diferença entre tempos
    time_t inicio_t = time(NULL);
    
    // Pequena pausa
    for (long i = 0; i < 100000000; i++);
    
    time_t fim_t = time(NULL);
    printf("Tempo decorrido: %.2f segundos\n", difftime(fim_t, inicio_t));
    
    return 0;
}
```

## 7. Exemplo Integrado Completo

```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <math.h>
#include <ctype.h>
#include <time.h>

// Estrutura para dados do aluno
typedef struct {
    char nome[50];
    int idade;
    float notas[3];
    float media;
} Aluno;

// Função para calcular média
float calcular_media(float notas[], int n) {
    float soma = 0;
    for (int i = 0; i < n; i++) {
        soma += notas[i];
    }
    return soma / n;
}

// Função para classificar desempenho
const char* classificar_desempenho(float media) {
    if (media >= 9.0) return "Excelente";
    if (media >= 7.0) return "Bom";
    if (media >= 5.0) return "Regular";
    return "Insuficiente";
}

int main() {
    srand(time(NULL));  // Inicializa gerador aleatório
    
    int num_alunos;
    printf("Quantos alunos? ");
    scanf("%d", &num_alunos);
    
    // Alocação dinâmica do array de alunos
    Aluno *alunos = (Aluno*)malloc(num_alunos * sizeof(Aluno));
    if (alunos == NULL) {
        printf("Erro na alocação de memória!\n");
        return 1;
    }
    
    // Entrada de dados
    for (int i = 0; i < num_alunos; i++) {
        printf("\n--- Aluno %d ---\n", i + 1);
        
        // Limpa buffer
        while (getchar() != '\n');
        
        printf("Nome: ");
        fgets(alunos[i].nome, sizeof(alunos[i].nome), stdin);
        alunos[i].nome[strcspn(alunos[i].nome, "\n")] = '\0';  // Remove \n
        
        printf("Idade: ");
        scanf("%d", &alunos[i].idade);
        
        printf("Digite 3 notas: ");
        for (int j = 0; j < 3; j++) {
            scanf("%f", &alunos[i].notas[j]);
        }
        
        // Calcula média
        alunos[i].media = calcular_media(alunos[i].notas, 3);
    }
    
    // Processamento e saída
    printf("\n=== RELATÓRIO FINAL ===\n");
    printf("%-20s %-5s %-6s %-6s %-6s %-8s %-12s\n", 
           "Nome", "Idade", "Nota1", "Nota2", "Nota3", "Média", "Desempenho");
    printf("%-20s %-5s %-6s %-6s %-6s %-8s %-12s\n",
           "--------------------", "-----", "------", "------", "------", "--------", "------------");
    
    float media_geral = 0;
    for (int i = 0; i < num_alunos; i++) {
        // Primeira letra do nome em maiúscula
        if (isalpha(alunos[i].nome[0])) {
            alunos[i].nome[0] = toupper(alunos[i].nome[0]);
        }
        
        printf("%-20s %-5d %-6.1f %-6.1f %-6.1f %-8.2f %-12s\n",
               alunos[i].nome,
               alunos[i].idade,
               alunos[i].notas[0],
               alunos[i].notas[1],
               alunos[i].notas[2],
               alunos[i].media,
               classificar_desempenho(alunos[i].media));
        
        media_geral += alunos[i].media;
    }
    
    media_geral /= num_alunos;
    
    printf("\n=== ESTATÍSTICAS ===\n");
    printf("Média geral da turma: %.2f\n", media_geral);
    printf("Desvio padrão aproximado: %.2f\n", 
           sqrt(pow(media_geral - 7.0, 2)));  // Simulação
    
    // Ordenação por média (bubble sort simples)
    for (int i = 0; i < num_alunos - 1; i++) {
        for (int j = 0; j < num_alunos - i - 1; j++) {
            if (alunos[j].media < alunos[j + 1].media) {
                Aluno temp = alunos[j];
                alunos[j] = alunos[j + 1];
                alunos[j + 1] = temp;
            }
        }
    }
    
    printf("\n=== TOP 3 MELHORES MÉDIAS ===\n");
    int limite = (num_alunos < 3) ? num_alunos : 3;
    for (int i = 0; i < limite; i++) {
        printf("%dº: %s (%.2f)\n", i + 1, alunos[i].nome, alunos[i].media);
    }
    
    // Geração de arquivo de relatório
    FILE *arquivo = fopen("relatorio_alunos.txt", "w");
    if (arquivo != NULL) {
        fprintf(arquivo, "RELATÓRIO DE ALUNOS - %s", ctime(&(time_t){time(NULL)}));
        fprintf(arquivo, "Total de alunos: %d\n", num_alunos);
        fprintf(arquivo, "Média geral: %.2f\n\n", media_geral);
        
        for (int i = 0; i < num_alunos; i++) {
            fprintf(arquivo, "Aluno: %s\n", alunos[i].nome);
            fprintf(arquivo, "Idade: %d\n", alunos[i].idade);
            fprintf(arquivo, "Notas: %.1f, %.1f, %.1f\n", 
                    alunos[i].notas[0], alunos[i].notas[1], alunos[i].notas[2]);
            fprintf(arquivo, "Média: %.2f - %s\n\n", 
                    alunos[i].media, classificar_desempenho(alunos[i].media));
        }
        fclose(arquivo);
        printf("\nRelatório salvo em 'relatorio_alunos.txt'\n");
    }
    
    // Liberação de memória
    free(alunos);
    
    return 0;
}
```

## 8. stdarg.h - Argumentos Variáveis

```c
#include <stdarg.h>
#include <stdio.h>

// Função com número variável de argumentos
int soma_variavel(int count, ...) {
    va_list args;
    va_start(args, count);
    
    int soma = 0;
    for (int i = 0; i < count; i++) {
        soma += va_arg(args, int);
    }
    
    va_end(args);
    return soma;
}

// Função que aceita diferentes tipos
void imprimir_variavel(const char *formato, ...) {
    va_list args;
    va_start(args, formato);
    
    vprintf(formato, args);
    
    va_end(args);
}

// Exemplo mais complexo - sistema de logging
void log_message(const char *nivel, const char *arquivo, int linha, const char *formato, ...) {
    printf("[%s] %s:%d - ", nivel, arquivo, linha);
    
    va_list args;
    va_start(args, formato);
    vprintf(formato, args);
    va_end(args);
    
    printf("\n");
}

#define LOG_INFO(...) log_message("INFO", __FILE__, __LINE__, __VA_ARGS__)
#define LOG_ERROR(...) log_message("ERROR", __FILE__, __LINE__, __VA_ARGS__)

int main() {
    printf("Soma de 3 números: %d\n", soma_variavel(3, 10, 20, 30));
    printf("Soma de 5 números: %d\n", soma_variavel(5, 1, 2, 3, 4, 5));
    
    imprimir_variavel("Inteiro: %d, Float: %.2f, String: %s\n", 42, 3.14, "Hello");
    
    LOG_INFO("Aplicação iniciada");
    LOG_INFO("Usuário %s conectou", "João");
    LOG_ERROR("Erro ao acessar recurso %s", "/api/dados");
    
    return 0;
}
```

## 9. assert.h - Verificação de Assertivas

```c
#include <assert.h>
#include <stdio.h>
#include <stdlib.h>

// Função que calcula fatorial
int fatorial(int n) {
    // Verificação de pré-condição
    assert(n >= 0 && "Número deve ser não negativo");
    assert(n <= 12 && "Número muito grande para int");
    
    if (n == 0) return 1;
    return n * fatorial(n - 1);
}

// Função que aloca matriz
int** criar_matriz(int linhas, int colunas) {
    assert(linhas > 0 && colunas > 0);
    
    int **matriz = (int**)malloc(linhas * sizeof(int*));
    assert(matriz != NULL && "Falha na alocação de memória");
    
    for (int i = 0; i < linhas; i++) {
        matriz[i] = (int*)malloc(colunas * sizeof(int));
        assert(matriz[i] != NULL && "Falha na alocação de linha");
    }
    
    return matriz;
}

// Para desativar asserts, definir NDEBUG antes de incluir assert.h
// #define NDEBUG

int main() {
    printf("Fatorial de 5: %d\n", fatorial(5));
    
    // Isso causará assert falhar:
    // printf("Fatorial de -1: %d\n", fatorial(-1));
    // printf("Fatorial de 15: %d\n", fatorial(15));
    
    int **matriz = criar_matriz(3, 4);
    
    // Inicializar matriz
    for (int i = 0; i < 3; i++) {
        for (int j = 0; j < 4; j++) {
            matriz[i][j] = i * 4 + j;
        }
    }
    
    // Liberar memória
    for (int i = 0; i < 3; i++) {
        free(matriz[i]);
    }
    free(matriz);
    
    // Verificação de invariantes
    int x = 10;
    assert(x == 10 && "Invariante violado");
    
    printf("Todos os asserts passaram!\n");
    return 0;
}
```

## 10. setjmp.h - Controle de Fluxo Não Local

```c
#include <setjmp.h>
#include <stdio.h>
#include <stdlib.h>

jmp_buf contexto_retorno;

void funcao_risco(int valor) {
    printf("Executando função de risco com valor: %d\n", valor);
    
    if (valor < 0) {
        printf("Erro: valor negativo! Retornando ao ponto seguro...\n");
        longjmp(contexto_retorno, 1);  // Retorna com código 1
    }
    
    if (valor > 100) {
        printf("Erro: valor muito grande! Retornando ao ponto seguro...\n");
        longjmp(contexto_retorno, 2);  // Retorna com código 2
    }
    
    printf("Processamento bem-sucedido!\n");
}

// Exemplo com tratamento de erro estruturado
typedef struct {
    jmp_buf contexto;
    int codigo_erro;
    char mensagem[100];
} ErrorHandler;

ErrorHandler erro_global;

#define TRY do { if ((erro_global.codigo_erro = setjmp(erro_global.contexto)) == 0)
#define CATCH else
#define END_TRY } while(0)

void operacao_perigosa(int x) {
    if (x < 0) {
        strcpy(erro_global.mensagem, "Valor negativo não permitido");
        longjmp(erro_global.contexto, 1);
    }
    
    if (x == 0) {
        strcpy(erro_global.mensagem, "Divisão por zero detectada");
        longjmp(erro_global.contexto, 2);
    }
    
    printf("Resultado: %d\n", 100 / x);
}

int main() {
    // Uso básico de setjmp/longjmp
    printf("=== Teste setjmp/longjmp ===\n");
    
    int retorno = setjmp(contexto_retorno);
    
    if (retorno == 0) {
        // Primeira execução - chama função normal
        funcao_risco(50);
        funcao_risco(-5);  // Isso causará longjmp
    } else {
        // Retorno via longjmp
        printf("Capturado retorno de emergência com código: %d\n", retorno);
    }
    
    // Sistema de tratamento de erro estilo try-catch
    printf("\n=== Sistema de Tratamento de Erro ===\n");
    
    TRY {
        printf("Teste 1: ");
        operacao_perigosa(10);
        
        printf("Teste 2: ");
        operacao_perigosa(0);  // Isso causará erro
        
        printf("Teste 3: ");  // Não será executado
        operacao_perigosa(5);
    }
    CATCH {
        printf("ERRO %d: %s\n", erro_global.codigo_erro, erro_global.mensagem);
    }
    END_TRY;
    
    return 0;
}
```

## 11. signal.h - Tratamento de Sinais

```c
#include <signal.h>
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>

// Handler para SIGINT (Ctrl+C)
void handler_sigint(int sig) {
    printf("\nRecebido SIGINT (Ctrl+C). Finalizando gracefuly...\n");
    exit(0);
}

// Handler para SIGALRM
void handler_sigalrm(int sig) {
    printf("Alarme! Timeout atingido.\n");
}

// Handler para SIGUSR1
void handler_sigusr1(int sig) {
    printf("Recebido sinal personalizado SIGUSR1\n");
}

// Handler para segmentation fault
void handler_sigsegv(int sig) {
    printf("Segmentation fault detectado! Endereço inválido de memória.\n");
    exit(1);
}

int main() {
    printf("PID do processo: %d\n", getpid());
    printf("Envie sinais com: kill -SINAL %d\n", getpid());
    
    // Registrar handlers
    signal(SIGINT, handler_sigint);
    signal(SIGALRM, handler_sigalrm);
    signal(SIGUSR1, handler_sigusr1);
    signal(SIGSEGV, handler_sigsegv);
    
    // Configurar alarme
    printf("Configurando alarme para 5 segundos...\n");
    alarm(5);
    
    // Programa principal
    printf("Pressione Ctrl+C para testar SIGINT\n");
    printf("Aguardando sinais...\n");
    
    // Loop infinito para demonstrar sinais
    int contador = 0;
    while(1) {
        printf("Executando... %d\r", contador++);
        fflush(stdout);
        sleep(1);
        
        // Para demonstrar SIGSEGV (descomente com cuidado)
        // if (contador == 10) {
        //     int *ptr = NULL;
        //     *ptr = 42;  // Causará segmentation fault
        // }
    }
    
    return 0;
}
```

## 12. locale.h - Internacionalização

```c
#include <locale.h>
#include <stdio.h>
#include <time.h>
#include <stdlib.h>

void demonstrar_localidade(const char *localidade) {
    printf("\n=== Localidade: %s ===\n", localidade);
    
    if (setlocale(LC_ALL, localidade) == NULL) {
        printf("Localidade %s não disponível\n", localidade);
        return;
    }
    
    // Números
    double numero = 1234567.89;
    printf("Número: %'f\n", numero);
    
    // Moeda (onde suportado)
    struct lconv *lc = localeconv();
    printf("Símbolo monetário: %s\n", lc->currency_symbol);
    printf("Separador decimal: '%s'\n", lc->decimal_point);
    printf("Separador milhar: '%s'\n", lc->thousands_sep);
    
    // Data/hora localizada
    time_t agora = time(NULL);
    struct tm *tm_info = localtime(&agora);
    
    char buffer[100];
    strftime(buffer, sizeof(buffer), "%x %X", tm_info);
    printf("Data/hora local: %s\n", buffer);
    
    strftime(buffer, sizeof(buffer), "%A, %d de %B de %Y", tm_info);
    printf("Data extensa: %s\n", buffer);
}

int main() {
    printf("Localidade atual: %s\n", setlocale(LC_ALL, NULL));
    
    // Testar diferentes localidades
    demonstrar_localidade("C");  // Padrão C
    demonstrar_localidade("en_US.UTF-8");  // Inglês EUA
    demonstrar_localidade("pt_BR.UTF-8");  // Português Brasil
    demonstrar_localidade("de_DE.UTF-8");  // Alemão
    demonstrar_localidade("ja_JP.UTF-8");  // Japonês
    
    // Configurações específicas por categoria
    printf("\n=== Configurações Específicas ===\n");
    
    setlocale(LC_NUMERIC, "de_DE.UTF-8");
    printf("Números alemães: %.2f\n", 1234.56);
    
    setlocale(LC_TIME, "pt_BR.UTF-8");
    time_t agora = time(NULL);
    struct tm *tm_info = localtime(&agora);
    
    char buffer[100];
    strftime(buffer, sizeof(buffer), "Data BR: %A, %d/%m/%Y", tm_info);
    printf("%s\n", buffer);
    
    // Restaurar padrão C
    setlocale(LC_ALL, "C");
    printf("\nRestaurado padrão C: %.2f\n", 1234.56);
    
    return 0;
}
```

## 13. errno.h - Tratamento de Erros

```c
#include <errno.h>
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <math.h>

void demonstrar_erro(int resultado, const char *operacao) {
    if (resultado == -1 && errno != 0) {
        printf("Erro em %s: %s (código %d)\n", 
               operacao, strerror(errno), errno);
    } else {
        printf("%s: Sucesso\n", operacao);
    }
}

int main() {
    FILE *arquivo;
    
    printf("=== Demonstração de errno ===\n");
    
    // Tentar abrir arquivo inexistente
    errno = 0;  // Reset errno
    arquivo = fopen("arquivo_inexistente.txt", "r");
    demonstrar_erro(-1, "Abrir arquivo inexistente");
    if (arquivo) fclose(arquivo);
    
    // Operação matemática inválida
    errno = 0;
    double resultado = sqrt(-1.0);
    if (errno != 0) {
        printf("Erro matemático: %s\n", strerror(errno));
    }
    
    // Divisão por zero
    errno = 0;
    int x = 1;
    int y = 0;
    // int z = x / y;  // Isso causaria exception, não errno
    
    // Alocação de memória falha
    errno = 0;
    void *ptr = malloc(1000000000000000ULL);  // Valor muito grande
    if (ptr == NULL && errno != 0) {
        printf("Erro de alocação: %s\n", strerror(errno));
    } else {
        free(ptr);
    }
    
    // Lista de códigos de erro comuns
    printf("\n=== Códigos de Erro Comuns ===\n");
    printf("EPERM (%d): %s\n", EPERM, strerror(EPERM));
    printf("ENOENT (%d): %s\n", ENOENT, strerror(ENOENT));
    printf("EINTR (%d): %s\n", EINTR, strerror(EINTR));
    printf("EIO (%d): %s\n", EIO, strerror(EIO));
    printf("EBADF (%d): %s\n", EBADF, strerror(EBADF));
    printf("EAGAIN (%d): %s\n", EAGAIN, strerror(EAGAIN));
    printf("EACCES (%d): %s\n", EACCES, strerror(EACCES));
    printf("EFAULT (%d): %s\n", EFAULT, strerror(EFAULT));
    printf("EINVAL (%d): %s\n", EINVAL, strerror(EINVAL));
    printf("ENOMEM (%d): %s\n", ENOMEM, strerror(ENOMEM));
    
    return 0;
}
```

## 14. float.h - Limites de Ponto Flutuante

```c
#include <float.h>
#include <stdio.h>
#include <math.h>

void demonstrar_limites_float() {
    printf("\n=== LIMITES PARA float ===\n");
    printf("FLT_RADIX (base): %d\n", FLT_RADIX);
    printf("FLT_MANT_DIG (dígitos mantissa): %d\n", FLT_MANT_DIG);
    printf("FLT_DIG (dígitos decimais de precisão): %d\n", FLT_DIG);
    printf("FLT_MIN_EXP (expoente mínimo): %d\n", FLT_MIN_EXP);
    printf("FLT_MAX_EXP (expoente máximo): %d\n", FLT_MAX_EXP);
    printf("FLT_MIN (valor mínimo positivo): %e\n", FLT_MIN);
    printf("FLT_MAX (valor máximo): %e\n", FLT_MAX);
    printf("FLT_EPSILON (diferença mínima): %e\n", FLT_EPSILON);
}

void demonstrar_limites_double() {
    printf("\n=== LIMITES PARA double ===\n");
    printf("DBL_MANT_DIG (dígitos mantissa): %d\n", DBL_MANT_DIG);
    printf("DBL_DIG (dígitos decimais de precisão): %d\n", DBL_DIG);
    printf("DBL_MIN_EXP (expoente mínimo): %d\n", DBL_MIN_EXP);
    printf("DBL_MAX_EXP (expoente máximo): %d\n", DBL_MAX_EXP);
    printf("DBL_MIN (valor mínimo positivo): %e\n", DBL_MIN);
    printf("DBL_MAX (valor máximo): %e\n", DBL_MAX);
    printf("DBL_EPSILON (diferença mínima): %e\n", DBL_EPSILON);
}

void demonstrar_limites_long_double() {
    printf("\n=== LIMITES PARA long double ===\n");
    printf("LDBL_MANT_DIG (dígitos mantissa): %d\n", LDBL_MANT_DIG);
    printf("LDBL_DIG (dígitos decimais de precisão): %d\n", LDBL_DIG);
    printf("LDBL_MIN (valor mínimo positivo): %Le\n", LDBL_MIN);
    printf("LDBL_MAX (valor máximo): %Le\n", LDBL_MAX);
    printf("LDBL_EPSILON (diferença mínima): %Le\n", LDBL_EPSILON);
}

void testar_precisao() {
    printf("\n=== TESTES DE PRECISÃO ===\n");
    
    float f1 = 1.0f;
    float f2 = 1.0f + FLT_EPSILON;
    float f3 = 1.0f + FLT_EPSILON / 2.0f;
    
    printf("1.0 == 1.0 + FLT_EPSILON? %s\n", f1 == f2 ? "sim" : "não");
    printf("1.0 == 1.0 + FLT_EPSILON/2? %s\n", f1 == f3 ? "sim" : "não");
    
    // Demonstração de underflow
    float pequeno = FLT_MIN;
    printf("Valor pequeno: %e\n", pequeno);
    printf("Metade do valor pequeno: %e\n", pequeno / 2.0f);
    
    // Demonstração de overflow
    float grande = FLT_MAX;
    printf("Valor grande: %e\n", grande);
    printf("Dobro do valor grande: %e (infinito? %d)\n", 
           grande * 2.0f, isinf(grande * 2.0f));
}

void classificar_numeros_especiais() {
    printf("\n=== NÚMEROS ESPECIAIS ===\n");
    
    float positivo_infinito = 1.0f / 0.0f;
    float negativo_infinito = -1.0f / 0.0f;
    float nan_val = 0.0f / 0.0f;
    
    printf("Infinito positivo: %f (isinf: %d)\n", positivo_infinito, isinf(positivo_infinito));
    printf("Infinito negativo: %f (isinf: %d)\n", negativo_infinito, isinf(negativo_infinito));
    printf("NaN: %f (isnan: %d)\n", nan_val, isnan(nan_val));
    
    // Operações com números especiais
    printf("Infinito + 100 = %f\n", positivo_infinito + 100.0f);
    printf("Infinito * 0 = %f\n", positivo_infinito * 0.0f);
    printf("NaN == NaN? %d\n", nan_val == nan_val);  // NaN nunca é igual a nada
}

int main() {
    demonstrar_limites_float();
    demonstrar_limites_double();
    demonstrar_limites_long_double();
    testar_precisao();
    classificar_numeros_especiais();
    
    return 0;
}
```

## 15. limits.h - Limites Inteiros

```c
#include <limits.h>
#include <stdio.h>

void demonstrar_limites_inteiros() {
    printf("=== LIMITES DE TIPOS INTEIROS ===\n\n");
    
    // Tipos com sinal
    printf("CHAR_BIT (bits em um char): %d\n", CHAR_BIT);
    printf("SCHAR_MIN (char mínimo com sinal): %d\n", SCHAR_MIN);
    printf("SCHAR_MAX (char máximo com sinal): %d\n", SCHAR_MAX);
    printf("UCHAR_MAX (char máximo sem sinal): %u\n", UCHAR_MAX);
    
    printf("\n--- short ---\n");
    printf("SHRT_MIN: %d\n", SHRT_MIN);
    printf("SHRT_MAX: %d\n", SHRT_MAX);
    printf("USHRT_MAX: %u\n", USHRT_MAX);
    
    printf("\n--- int ---\n");
    printf("INT_MIN: %d\n", INT_MIN);
    printf("INT_MAX: %d\n", INT_MAX);
    printf("UINT_MAX: %u\n", UINT_MAX);
    
    printf("\n--- long ---\n");
    printf("LONG_MIN: %ld\n", LONG_MIN);
    printf("LONG_MAX: %ld\n", LONG_MAX);
    printf("ULONG_MAX: %lu\n", ULONG_MAX);
    
    printf("\n--- long long ---\n");
    printf("LLONG_MIN: %lld\n", LLONG_MIN);
    printf("LLONG_MAX: %lld\n", LLONG_MAX);
    printf("ULLONG_MAX: %llu\n", ULLONG_MAX);
}

void testar_overflow() {
    printf("\n=== TESTES DE OVERFLOW ===\n");
    
    int max_int = INT_MAX;
    int min_int = INT_MIN;
    
    printf("INT_MAX = %d\n", max_int);
    printf("INT_MAX + 1 = %d (overflow!)\n", max_int + 1);
    printf("INT_MIN = %d\n", min_int);
    printf("INT_MIN - 1 = %d (underflow!)\n", min_int - 1);
    
    // Operações seguras
    printf("\nVerificações seguras:\n");
    if (max_int > INT_MAX - 100) {
        printf("Cuidado: operação pode causar overflow!\n");
    }
    
    // Uso de unsigned para evitar comportamento indefinido
    unsigned int u_max = UINT_MAX;
    printf("UINT_MAX = %u\n", u_max);
    printf("UINT_MAX + 1 = %u (wrap-around)\n", u_max + 1);
}

void demonstrar_tamanhos() {
    printf("\n=== TAMANHOS DOS TIPOS ===\n");
    printf("sizeof(char) = %zu byte\n", sizeof(char));
    printf("sizeof(short) = %zu bytes\n", sizeof(short));
    printf("sizeof(int) = %zu bytes\n", sizeof(int));
    printf("sizeof(long) = %zu bytes\n", sizeof(long));
    printf("sizeof(long long) = %zu bytes\n", sizeof(long long));
    printf("sizeof(float) = %zu bytes\n", sizeof(float));
    printf("sizeof(double) = %zu bytes\n", sizeof(double));
    printf("sizeof(long double) = %zu bytes\n", sizeof(long double));
}

int main() {
    demonstrar_limites_inteiros();
    testar_overflow();
    demonstrar_tamanhos();
    
    return 0;
}
```

## 16. Exemplo Integrado Avançado

```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <math.h>
#include <time.h>
#include <locale.h>
#include <errno.h>
#include <assert.h>
#include <signal.h>
#include <setjmp.h>

// Sistema de logging avançado
typedef enum {
    LOG_DEBUG,
    LOG_INFO,
    LOG_WARNING,
    LOG_ERROR,
    LOG_CRITICAL
} LogLevel;

jmp_buf contexto_emergencia;

void logger(LogLevel nivel, const char *arquivo, int linha, const char *formato, ...) {
    const char *niveis[] = {"DEBUG", "INFO", "WARNING", "ERROR", "CRITICAL"};
    time_t agora = time(NULL);
    struct tm *tm_info = localtime(&agora);
    
    char timestamp[20];
    strftime(timestamp, sizeof(timestamp), "%Y-%m-%d %H:%M:%S", tm_info);
    
    printf("[%s] %s %s:%d - ", timestamp, niveis[nivel], arquivo, linha);
    
    va_list args;
    va_start(args, formato);
    vprintf(formato, args);
    va_end(args);
    
    printf("\n");
    
    // Para erros críticos, salvar em arquivo também
    if (nivel >= LOG_ERROR) {
        FILE *log_file = fopen("erros.log", "a");
        if (log_file) {
            fprintf(log_file, "[%s] %s %s:%d - ", timestamp, niveis[nivel], arquivo, linha);
            va_start(args, formato);
            vfprintf(log_file, formato, args);
            va_end(args);
            fprintf(log_file, "\n");
            fclose(log_file);
        }
    }
    
    // Erro crítico ativa modo de emergência
    if (nivel == LOG_CRITICAL) {
        longjmp(contexto_emergencia, 1);
    }
}

#define LOG(level, ...) logger(level, __FILE__, __LINE__, __VA_ARGS__)

// Sistema de configuração com tratamento de erro
typedef struct {
    int max_connections;
    double timeout;
    char log_level[20];
    char locale[20];
} Config;

Config carregar_configuracao(const char *arquivo_config) {
    Config config = {10, 30.0, "INFO", "C"};
    
    FILE *file = fopen(arquivo_config, "r");
    if (!file) {
        LOG(LOG_WARNING, "Arquivo de configuração %s não encontrado, usando padrões", 
            arquivo_config);
        return config;
    }
    
    char linha[256];
    while (fgets(linha, sizeof(linha), file)) {
        char chave[50], valor[50];
        if (sscanf(linha, "%49[^=]=%49s", chave, valor) == 2) {
            if (strcmp(chave, "max_connections") == 0) {
                config.max_connections = atoi(valor);
            } else if (strcmp(chave, "timeout") == 0) {
                config.timeout = atof(valor);
            } else if (strcmp(chave, "log_level") == 0) {
                strncpy(config.log_level, valor, sizeof(config.log_level) - 1);
            } else if (strcmp(chave, "locale") == 0) {
                strncpy(config.locale, valor, sizeof(config.locale) - 1);
            }
        }
    }
    
    fclose(file);
    LOG(LOG_INFO, "Configuração carregada de %s", arquivo_config);
    return config;
}

// Handler de sinais
void handler_sinal(int sig) {
    LOG(LOG_INFO, "Recebido sinal %d, finalizando gracefuly...", sig);
    exit(0);
}

// Função matemática segura
double calcular_raiz_segura(double valor) {
    errno = 0;
    double resultado = sqrt(valor);
    
    if (errno != 0) {
        LOG(LOG_ERROR, "Erro ao calcular raiz de %f: %s", valor, strerror(errno));
        return -1.0;
    }
    
    if (isnan(resultado)) {
        LOG(LOG_WARNING, "Raiz quadrada de número negativo: %f", valor);
        return -1.0;
    }
    
    return resultado;
}

// Sistema de estatísticas
typedef struct {
    double soma;
    double soma_quadrados;
    int contador;
    double min;
    double max;
} Estatisticas;

void inicializar_estatisticas(Estatisticas *stats) {
    memset(stats, 0, sizeof(Estatisticas));
    stats->min = DBL_MAX;
    stats->max = DBL_MIN;
}

void adicionar_amostra(Estatisticas *stats, double amostra) {
    stats->soma += amostra;
    stats->soma_quadrados += amostra * amostra;
    stats->contador++;
    
    if (amostra < stats->min) stats->min = amostra;
    if (amostra > stats->max) stats->max = amostra;
}

void imprimir_estatisticas(const Estatisticas *stats, const char *nome) {
    if (stats->contador == 0) {
        LOG(LOG_WARNING, "Nenhuma amostra para %s", nome);
        return;
    }
    
    double media = stats->soma / stats->contador;
    double variancia = (stats->soma_quadrados / stats->contador) - (media * media);
    double desvio_padrao = sqrt(fabs(variancia));
    
    LOG(LOG_INFO, "Estatísticas de %s:", nome);
    LOG(LOG_INFO, "  Amostras: %d, Média: %.4f", stats->contador, media);
    LOG(LOG_INFO, "  Min: %.4f, Max: %.4f", stats->min, stats->max);
    LOG(LOG_INFO, "  Desvio Padrão: %.4f", desvio_padrao);
}

int main() {
    // Configurar handlers de sinal
    signal(SIGINT, handler_sinal);
    signal(SIGTERM, handler_sinal);
    
    // Configurar sistema de emergência
    if (setjmp(contexto_emergencia) == 0) {
        LOG(LOG_INFO, "Sistema inicializado em modo normal");
    } else {
        LOG(LOG_CRITICAL, "Sistema em modo de emergência!");
        printf("Executando procedimentos de emergência...\n");
        // Salvar estado, fechar conexões, etc.
        exit(1);
    }
    
    // Carregar configuração
    Config config = carregar_configuracao("app.conf");
    
    // Configurar localidade
    if (setlocale(LC_ALL, config.locale) == NULL) {
        LOG(LOG_WARNING, "Localidade %s não disponível, usando padrão C", config.locale);
        setlocale(LC_ALL, "C");
    }
    
    LOG(LOG_INFO, "Sistema iniciado com:");
    LOG(LOG_INFO, "  Conexões máximas: %d", config.max_connections);
    LOG(LOG_INFO, "  Timeout: %.1f segundos", config.timeout);
    LOG(LOG_INFO, "  Nível de log: %s", config.log_level);
    LOG(LOG_INFO, "  Localidade: %s", config.locale);
    
    // Demonstrar limites do sistema
    LOG(LOG_DEBUG, "Limite máximo de int: %d", INT_MAX);
    LOG(LOG_DEBUG, "Limite máximo de double: %e", DBL_MAX);
    
    // Coletar estatísticas
    Estatisticas stats_tempo, stats_memoria;
    inicializar_estatisticas(&stats_tempo);
    inicializar_estatisticas(&stats_memoria);
    
    srand(time(NULL));
    
    LOG(LOG_INFO, "Iniciando simulação...");
    
    for (int i = 0; i < 100; i++) {
        // Simular processamento
        double tempo_execucao = (rand() % 1000) / 100.0;
        double uso_memoria = (rand() % 500) + 100.0;
        
        adicionar_amostra(&stats_tempo, tempo_execucao);
        adicionar_amostra(&stats_memoria, uso_memoria);
        
        // Simular erro ocasional
        if (rand() % 20 == 0) {
            LOG(LOG_WARNING, "Erro simulado na iteração %d", i);
        }
        
        // Teste de raiz segura
        double valor_teste = (rand() % 200) - 50.0;  // Alguns negativos
        double raiz = calcular_raiz_segura(valor_teste);
        
        if (raiz >= 0) {
            LOG(LOG_DEBUG, "Raiz de %.1f = %.4f", valor_teste, raiz);
        }
        
        // Verificação de assert para invariantes
        assert(tempo_execucao >= 0 && "Tempo de execução não pode ser negativo");
        
        // Pequena pausa
        usleep(10000);  // 10ms
    }
    
    // Relatório final
    LOG(LOG_INFO, "Simulação concluída");
    imprimir_estatisticas(&stats_tempo, "Tempo de Execução");
    imprimir_estatisticas(&stats_memoria, "Uso de Memória");
    
    // Teste crítico (descomente para testar modo emergência)
    // LOG(LOG_CRITICAL, "Teste de erro crítico!");
    
    LOG(LOG_INFO, "Sistema finalizado com sucesso");
    return 0;
}
```

# C - Bibliotecas Adicionais Essenciais

## 17. pthread.h - Programação Multithread

```c
#include <pthread.h>
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <string.h>

#define NUM_THREADS 5

// Estrutura para passar dados para as threads
typedef struct {
    int id;
    int contador;
    char mensagem[100];
} ThreadData;

// Variáveis globais para demonstração
int contador_global = 0;
pthread_mutex_t mutex_global = PTHREAD_MUTEX_INITIALIZER;
pthread_cond_t cond_var = PTHREAD_COND_INITIALIZER;
int recurso_pronto = 0;

// Função executada por cada thread
void* funcao_thread(void* arg) {
    ThreadData* data = (ThreadData*) arg;
    
    printf("Thread %d iniciada: %s\n", data->id, data->mensagem);
    
    // Simular trabalho
    for (int i = 0; i < data->contador; i++) {
        printf("Thread %d: trabalhando... %d/%d\n", data->id, i + 1, data->contador);
        usleep(100000); // 100ms
        
        // Acesso seguro à variável global
        pthread_mutex_lock(&mutex_global);
        contador_global++;
        printf("Thread %d: contador_global = %d\n", data->id, contador_global);
        pthread_mutex_unlock(&mutex_global);
    }
    
    printf("Thread %d finalizada\n", data->id);
    pthread_exit(NULL);
}

// Thread produtora
void* produtor(void* arg) {
    printf("Produtor: iniciando...\n");
    
    for (int i = 0; i < 3; i++) {
        sleep(1); // Simular produção
        
        pthread_mutex_lock(&mutex_global);
        recurso_pronto = 1;
        printf("Produtor: recurso %d produzido\n", i + 1);
        
        // Sinalizar que o recurso está pronto
        pthread_cond_signal(&cond_var);
        pthread_mutex_unlock(&mutex_global);
    }
    
    pthread_exit(NULL);
}

// Thread consumidora
void* consumidor(void* arg) {
    printf("Consumidor: iniciando...\n");
    
    for (int i = 0; i < 3; i++) {
        pthread_mutex_lock(&mutex_global);
        
        // Esperar até que o recurso esteja pronto
        while (recurso_pronto == 0) {
            printf("Consumidor: esperando recurso...\n");
            pthread_cond_wait(&cond_var, &mutex_global);
        }
        
        printf("Consumidor: recurso %d consumido\n", i + 1);
        recurso_pronto = 0;
        pthread_mutex_unlock(&mutex_global);
    }
    
    pthread_exit(NULL);
}

void demonstrar_threads_basicas() {
    printf("=== THREADS BÁSICAS ===\n");
    
    pthread_t threads[NUM_THREADS];
    ThreadData thread_data[NUM_THREADS];
    
    // Criar threads
    for (int i = 0; i < NUM_THREADS; i++) {
        thread_data[i].id = i;
        thread_data[i].contador = 3;
        snprintf(thread_data[i].mensagem, sizeof(thread_data[i].mensagem), 
                "Thread número %d", i);
        
        int result = pthread_create(&threads[i], NULL, funcao_thread, (void*)&thread_data[i]);
        if (result != 0) {
            printf("Erro ao criar thread %d: %s\n", i, strerror(result));
        }
    }
    
    // Aguardar todas as threads terminarem
    for (int i = 0; i < NUM_THREADS; i++) {
        pthread_join(threads[i], NULL);
    }
    
    printf("Contador global final: %d\n", contador_global);
}

void demonstrar_produtor_consumidor() {
    printf("\n=== PADRÃO PRODUTOR-CONSUMIDOR ===\n");
    
    pthread_t produtor_thread, consumidor_thread;
    
    pthread_create(&produtor_thread, NULL, produtor, NULL);
    pthread_create(&consumidor_thread, NULL, consumidor, NULL);
    
    pthread_join(produtor_thread, NULL);
    pthread_join(consumidor_thread, NULL);
}

// Thread com atributos customizados
void demonstrar_atributos_threads() {
    printf("\n=== ATRIBUTOS DE THREAD ===\n");
    
    pthread_t thread;
    pthread_attr_t attr;
    
    // Inicializar atributos
    pthread_attr_init(&attr);
    
    // Configurar thread como joinable
    pthread_attr_setdetachstate(&attr, PTHREAD_CREATE_JOINABLE);
    
    // Configurar tamanho da pilha
    size_t stack_size;
    pthread_attr_getstacksize(&attr, &stack_size);
    printf("Tamanho padrão da pilha: %zu bytes\n", stack_size);
    
    pthread_attr_setstacksize(&attr, 1024*1024); // 1MB
    
    // Criar thread com atributos customizados
    int data = 42;
    pthread_create(&thread, &attr, funcao_thread, &data);
    
    pthread_join(thread, NULL);
    pthread_attr_destroy(&attr);
}

int main() {
    printf("=== PROGRAMAÇÃO MULTITHREAD EM C ===\n");
    
    demonstrar_threads_basicas();
    demonstrar_produtor_consumidor();
    demonstrar_atributos_threads();
    
    // Limpar recursos
    pthread_mutex_destroy(&mutex_global);
    pthread_cond_destroy(&cond_var);
    
    return 0;
}
```

## 18. sys/time.h - Tempo e Timers de Alta Precisão

```c
#include <sys/time.h>
#include <stdio.h>
#include <unistd.h>
#include <time.h>

// Função para obter timestamp de alta precisão
double get_timestamp() {
    struct timeval tv;
    gettimeofday(&tv, NULL);
    return tv.tv_sec + tv.tv_usec / 1000000.0;
}

void demonstrar_gettimeofday() {
    printf("=== gettimeofday - TEMPO DE ALTA PRECISÃO ===\n");
    
    struct timeval inicio, fim;
    long segundos, microsegundos;
    double elapsed;
    
    gettimeofday(&inicio, NULL);
    
    // Simular algum processamento
    for (int i = 0; i < 1000000; i++) {
        volatile double x = i * 3.14159;
    }
    
    gettimeofday(&fim, NULL);
    
    segundos = fim.tv_sec - inicio.tv_sec;
    microsegundos = fim.tv_usec - inicio.tv_usec;
    elapsed = segundos + microsegundos / 1000000.0;
    
    printf("Tempo decorrido: %ld segundos, %ld microsegundos\n", segundos, microsegundos);
    printf("Tempo total: %.6f segundos\n", elapsed);
}

void benchmark_operacao() {
    printf("\n=== BENCHMARK DE OPERAÇÕES ===\n");
    
    double inicio = get_timestamp();
    
    // Operação 1: Acesso a array
    int array[1000];
    for (int i = 0; i < 1000000; i++) {
        array[i % 1000] = i;
    }
    
    double fim1 = get_timestamp();
    printf("Acesso a array: %.6f segundos\n", fim1 - inicio);
    
    // Operação 2: Cálculo matemático
    double resultado = 0;
    for (int i = 0; i < 1000000; i++) {
        resultado += sin(i * 0.001) * cos(i * 0.001);
    }
    
    double fim2 = get_timestamp();
    printf("Cálculos matemáticos: %.6f segundos\n", fim2 - fim1);
    printf("Resultado (para evitar otimização): %f\n", resultado);
}

void demonstrar_timeval_operations() {
    printf("\n=== OPERAÇÕES COM timeval ===\n");
    
    struct timeval tv1, tv2, resultado;
    
    // Definir dois tempos
    tv1.tv_sec = 10;
    tv1.tv_usec = 500000; // 10.5 segundos
    
    tv2.tv_sec = 5;
    tv2.tv_usec = 750000; // 5.75 segundos
    
    // Adicionar tempos
    resultado.tv_sec = tv1.tv_sec + tv2.tv_sec;
    resultado.tv_usec = tv1.tv_usec + tv2.tv_usec;
    
    // Ajustar se microsegundos > 1 milhão
    if (resultado.tv_usec >= 1000000) {
        resultado.tv_sec += resultado.tv_usec / 1000000;
        resultado.tv_usec %= 1000000;
    }
    
    printf("tv1: %ld.%06ld\n", tv1.tv_sec, tv1.tv_usec);
    printf("tv2: %ld.%06ld\n", tv2.tv_sec, tv2.tv_usec);
    printf("Soma: %ld.%06ld\n", resultado.tv_sec, resultado.tv_usec);
}

// Timer usando setitimer
void handler_timer(int sig) {
    printf("Timer disparado! (%s)\n", 
           sig == SIGALRM ? "SIGALRM" : "Outro sinal");
}

void demonstrar_setitimer() {
    printf("\n=== TIMERS COM setitimer ===\n");
    
    struct itimerval timer;
    
    // Configurar handler para SIGALRM
    signal(SIGALRM, handler_timer);
    
    // Configurar timer para disparar após 2 segundos
    timer.it_value.tv_sec = 2;
    timer.it_value.tv_usec = 0;
    
    // E a cada 1 segundo após o primeiro disparo
    timer.it_interval.tv_sec = 1;
    timer.it_interval.tv_usec = 0;
    
    printf("Iniciando timer: primeiro disparo em 2s, depois a cada 1s\n");
    
    if (setitimer(ITIMER_REAL, &timer, NULL) == -1) {
        perror("setitimer");
        return;
    }
    
    // Aguardar alguns disparos
    for (int i = 0; i < 5; i++) {
        pause(); // Espera por sinal
    }
    
    // Desativar timer
    timer.it_value.tv_sec = 0;
    timer.it_value.tv_usec = 0;
    timer.it_interval.tv_sec = 0;
    timer.it_interval.tv_usec = 0;
    setitimer(ITIMER_REAL, &timer, NULL);
    
    printf("Timer desativado\n");
}

int main() {
    printf("=== SYS/TIME.H - TEMPO E TIMERS ===\n");
    
    demonstrar_gettimeofday();
    benchmark_operacao();
    demonstrar_timeval_operations();
    // demonstrar_setitimer(); // Descomente para testar (irá pausar execução)
    
    return 0;
}
```

## 19. dirent.h - Manipulação de Diretórios

```c
#include <dirent.h>
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <sys/stat.h>
#include <unistd.h>

void listar_diretorio(const char *caminho) {
    printf("=== LISTANDO DIRETÓRIO: %s ===\n", caminho);
    
    DIR *dir = opendir(caminho);
    if (dir == NULL) {
        perror("opendir");
        return;
    }
    
    struct dirent *entry;
    int count = 0;
    
    printf("%-20s %-10s %s\n", "Nome", "Tipo", "Inode");
    printf("%-20s %-10s %s\n", "--------------------", "----------", "-----");
    
    while ((entry = readdir(dir)) != NULL) {
        const char *tipo;
        
        switch (entry->d_type) {
            case DT_REG:  tipo = "Arquivo"; break;
            case DT_DIR:  tipo = "Diretório"; break;
            case DT_LNK:  tipo = "Link"; break;
            case DT_FIFO: tipo = "FIFO"; break;
            case DT_SOCK: tipo = "Socket"; break;
            case DT_CHR:  tipo = "Char"; break;
            case DT_BLK:  tipo = "Block"; break;
            default:      tipo = "Unknown"; break;
        }
        
        printf("%-20s %-10s %lu\n", entry->d_name, tipo, entry->d_ino);
        count++;
    }
    
    closedir(dir);
    printf("Total de entradas: %d\n", count);
}

void buscar_arquivos(const char *caminho, const char *extensao) {
    printf("\n=== BUSCANDO *.%s EM %s ===\n", extensao, caminho);
    
    DIR *dir = opendir(caminho);
    if (dir == NULL) {
        perror("opendir");
        return;
    }
    
    struct dirent *entry;
    int encontrados = 0;
    
    while ((entry = readdir(dir)) != NULL) {
        if (entry->d_type == DT_REG) {
            char *ponto = strrchr(entry->d_name, '.');
            if (ponto && strcmp(ponto + 1, extensao) == 0) {
                printf("Encontrado: %s\n", entry->d_name);
                encontrados++;
            }
        }
    }
    
    closedir(dir);
    printf("Arquivos .%s encontrados: %d\n", extensao, encontrados);
}

void informacoes_arquivo(const char *caminho) {
    printf("\n=== INFORMAÇÕES DO ARQUIVO ===\n");
    
    struct stat file_info;
    
    if (stat(caminho, &file_info) == -1) {
        perror("stat");
        return;
    }
    
    printf("Arquivo: %s\n", caminho);
    printf("Tamanho: %ld bytes\n", file_info.st_size);
    printf("Inode: %ld\n", file_info.st_ino);
    printf("Links: %ld\n", file_info.st_nlink);
    printf("UID: %d\n", file_info.st_uid);
    printf("GID: %d\n", file_info.st_gid);
    
    printf("Permissões: ");
    printf((S_ISDIR(file_info.st_mode)) ? "d" : "-");
    printf((file_info.st_mode & S_IRUSR) ? "r" : "-");
    printf((file_info.st_mode & S_IWUSR) ? "w" : "-");
    printf((file_info.st_mode & S_IXUSR) ? "x" : "-");
    printf((file_info.st_mode & S_IRGRP) ? "r" : "-");
    printf((file_info.st_mode & S_IWGRP) ? "w" : "-");
    printf((file_info.st_mode & S_IXGRP) ? "x" : "-");
    printf((file_info.st_mode & S_IROTH) ? "r" : "-");
    printf((file_info.st_mode & S_IWOTH) ? "w" : "-");
    printf((file_info.st_mode & S_IXOTH) ? "x" : "-");
    printf("\n");
    
    printf("Último acesso: %ld\n", file_info.st_atime);
    printf("Última modificação: %ld\n", file_info.st_mtime);
    printf("Última mudança status: %ld\n", file_info.st_ctime);
}

void navegar_diretorios_recursivo(const char *caminho, int nivel) {
    DIR *dir = opendir(caminho);
    if (dir == NULL) return;
    
    struct dirent *entry;
    
    while ((entry = readdir(dir)) != NULL) {
        // Pular diretórios . e ..
        if (strcmp(entry->d_name, ".") == 0 || strcmp(entry->d_name, "..") == 0) {
            continue;
        }
        
        // Recriar caminho completo
        char caminho_completo[1024];
        snprintf(caminho_completo, sizeof(caminho_completo), 
                "%s/%s", caminho, entry->d_name);
        
        // Indentar baseado no nível
        for (int i = 0; i < nivel; i++) {
            printf("  ");
        }
        
        if (entry->d_type == DT_DIR) {
            printf("📁 %s/\n", entry->d_name);
            navegar_diretorios_recursivo(caminho_completo, nivel + 1);
        } else {
            printf("📄 %s\n", entry->d_name);
        }
    }
    
    closedir(dir);
}

int main() {
    printf("=== DIRENT.H - MANIPULAÇÃO DE DIRETÓRIOS ===\n");
    
    // Listar diretório atual
    listar_diretorio(".");
    
    // Buscar arquivos específicos
    buscar_arquivos(".", "c");
    buscar_arquivos(".", "h");
    
    // Informações de um arquivo específico
    informacoes_arquivo("exemplo.c"); // Altere para um arquivo existente
    
    // Navegação recursiva
    printf("\n=== ESTRUTURA DE DIRETÓRIOS (recursiva) ===\n");
    navegar_diretorios_recursivo(".", 0);
    
    // Criar e remover diretório
    printf("\n=== OPERAÇÕES DE DIRETÓRIO ===\n");
    
    if (mkdir("diretorio_teste", 0755) == 0) {
        printf("Diretório 'diretorio_teste' criado\n");
        
        // Listar novamente para ver o novo diretório
        listar_diretorio(".");
        
        // Remover diretório
        if (rmdir("diretorio_teste") == 0) {
            printf("Diretório 'diretorio_teste' removido\n");
        }
    }
    
    return 0;
}
```

## 20. unistd.h - Chamadas de Sistema POSIX

```c
#include <unistd.h>
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <sys/wait.h>
#include <fcntl.h>

void demonstrar_processos() {
    printf("=== GERENCIAMENTO DE PROCESSOS ===\n");
    
    pid_t pid = fork();
    
    if (pid == -1) {
        perror("fork");
        return;
    } else if (pid == 0) {
        // Processo filho
        printf("Processo FILHO: PID = %d, PPID = %d\n", 
               getpid(), getppid());
        
        // Filho faz algo diferente
        for (int i = 0; i < 3; i++) {
            printf("Filho trabalhando... %d/3\n", i + 1);
            sleep(1);
        }
        
        printf("Filho terminando\n");
        exit(42); // Código de saída do filho
    } else {
        // Processo pai
        printf("Processo PAI: PID = %d, Filho PID = %d\n", 
               getpid(), pid);
        
        int status;
        pid_t child_pid = wait(&status);
        
        if (WIFEXITED(status)) {
            printf("Filho %d terminou com código: %d\n", 
                   child_pid, WEXITSTATUS(status));
        } else if (WIFSIGNALED(status)) {
            printf("Filho %d terminou por sinal: %d\n", 
                   child_pid, WTERMSIG(status));
        }
    }
}

void demonstrar_exec() {
    printf("\n=== FUNÇÕES EXEC ===\n");
    
    pid_t pid = fork();
    
    if (pid == 0) {
        // Processo filho - executar comando ls
        printf("Executando 'ls -l' via execvp:\n");
        
        char *args[] = {"ls", "-l", NULL};
        execvp("ls", args);
        
        // Se execvp retornar, houve erro
        perror("execvp");
        exit(1);
    } else {
        wait(NULL); // Aguardar filho terminar
    }
}

void demonstrar_io_baixo_nivel() {
    printf("\n=== I/O DE BAIXO NÍVEL ===\n");
    
    const char *filename = "teste_io.bin";
    
    // Criar e escrever no arquivo
    int fd = open(filename, O_WRONLY | O_CREAT | O_TRUNC, 0644);
    if (fd == -1) {
        perror("open");
        return;
    }
    
    char buffer[] = "Hello, Low-level I/O!";
    ssize_t bytes_escritos = write(fd, buffer, strlen(buffer));
    printf("Bytes escritos: %zd\n", bytes_escritos);
    close(fd);
    
    // Ler do arquivo
    fd = open(filename, O_RDONLY);
    if (fd == -1) {
        perror("open");
        return;
    }
    
    char read_buffer[100];
    ssize_t bytes_lidos = read(fd, read_buffer, sizeof(read_buffer) - 1);
    if (bytes_lidos > 0) {
        read_buffer[bytes_lidos] = '\0';
        printf("Conteúdo lido: %s\n", read_buffer);
    }
    close(fd);
    
    // Remover arquivo
    unlink(filename);
    printf("Arquivo %s removido\n", filename);
}

void demonstrar_controle_arquivos() {
    printf("\n=== CONTROLE DE ARQUIVOS ===\n");
    
    int fd = open("exemplo_controle.txt", O_WRONLY | O_CREAT | O_TRUNC, 0644);
    if (fd == -1) {
        perror("open");
        return;
    }
    
    // Obter flags atuais
    int flags = fcntl(fd, F_GETFL);
    printf("Flags do arquivo: %d\n", flags);
    
    // Escrever algo
    write(fd, "Teste de controle\n", 18);
    
    // Obter informações do descritor
    struct stat file_info;
    if (fstat(fd, &file_info) == 0) {
        printf("Tamanho do arquivo: %ld bytes\n", file_info.st_size);
    }
    
    // Duplicar descritor
    int new_fd = dup(fd);
    printf("Novo descritor: %d (original: %d)\n", new_fd, fd);
    
    close(new_fd);
    close(fd);
    
    unlink("exemplo_controle.txt");
}

void demonstrar_sistema_arquivos() {
    printf("\n=== OPERAÇÕES DE SISTEMA DE ARQUIVOS ===\n");
    
    // Obter diretório atual
    char cwd[1024];
    if (getcwd(cwd, sizeof(cwd)) {
        printf("Diretório atual: %s\n", cwd);
    }
    
    // Mudar diretório
    if (chdir("..") == 0) {
        getcwd(cwd, sizeof(cwd));
        printf("Após chdir('..'): %s\n", cwd);
        
        // Voltar ao original
        chdir("."); // Volte para o diretório do executável
    }
    
    // Informações do usuário
    printf("UID real: %d\n", getuid());
    printf("UID efetivo: %d\n", geteuid());
    printf("GID real: %d\n", getgid());
    printf("GID efetivo: %d\n", getegid());
    
    // Variáveis de ambiente
    extern char **environ;
    printf("\nPrimeiras 3 variáveis de ambiente:\n");
    for (int i = 0; i < 3 && environ[i] != NULL; i++) {
        printf("  %s\n", environ[i]);
    }
}

void pipeline_example() {
    printf("\n=== PIPELINE ENTRO PROCESSOS ===\n");
    
    int pipefd[2];
    if (pipe(pipefd) == -1) {
        perror("pipe");
        return;
    }
    
    pid_t pid = fork();
    
    if (pid == 0) {
        // Processo filho - escrever no pipe
        close(pipefd[0]); // Fechar extremidade de leitura
        
        const char *mensagem = "Mensagem do filho para o pai via pipe!";
        write(pipefd[1], mensagem, strlen(mensagem));
        close(pipefd[1]);
        exit(0);
    } else {
        // Processo pai - ler do pipe
        close(pipefd[1]); // Fechar extremidade de escrita
        
        char buffer[100];
        ssize_t bytes_lidos = read(pipefd[0], buffer, sizeof(buffer) - 1);
        if (bytes_lidos > 0) {
            buffer[bytes_lidos] = '\0';
            printf("Pai recebeu: %s\n", buffer);
        }
        close(pipefd[0]);
        wait(NULL);
    }
}

int main() {
    printf("=== UNISTD.H - CHAMADAS DE SISTEMA POSIX ===\n");
    
    demonstrar_processos();
    demonstrar_exec();
    demonstrar_io_baixo_nivel();
    demonstrar_controle_arquivos();
    demonstrar_sistema_arquivos();
    pipeline_example();
    
    return 0;
}
```

## 21. sys/socket.h - Programação de Rede

```c
#include <sys/socket.h>
#include <netinet/in.h>
#include <arpa/inet.h>
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <unistd.h>

#define PORT 8080
#define BUFFER_SIZE 1024

void demonstrar_conversao_enderecos() {
    printf("=== CONVERSÃO DE ENDEREÇOS ===\n");
    
    // IPv4
    struct in_addr addr_ipv4;
    inet_pton(AF_INET, "192.168.1.1", &addr_ipv4);
    
    char str_ipv4[INET_ADDRSTRLEN];
    inet_ntop(AF_INET, &addr_ipv4, str_ipv4, INET_ADDRSTRLEN);
    printf("IPv4: %s -> %s\n", "192.168.1.1", str_ipv4);
    
    // IPv6
    struct in6_addr addr_ipv6;
    inet_pton(AF_INET6, "2001:db8::1", &addr_ipv6);
    
    char str_ipv6[INET6_ADDRSTRLEN];
    inet_ntop(AF_INET6, &addr_ipv6, str_ipv6, INET6_ADDRSTRLEN);
    printf("IPv6: %s -> %s\n", "2001:db8::1", str_ipv6);
}

void servidor_tcp() {
    printf("\n=== SERVIDOR TCP SIMPLES ===\n");
    
    int server_fd, new_socket;
    struct sockaddr_in address;
    int opt = 1;
    int addrlen = sizeof(address);
    char buffer[BUFFER_SIZE] = {0};
    
    // Criar socket
    if ((server_fd = socket(AF_INET, SOCK_STREAM, 0)) == 0) {
        perror("socket failed");
        exit(EXIT_FAILURE);
    }
    
    // Configurar opções do socket
    if (setsockopt(server_fd, SOL_SOCKET, SO_REUSEADDR | SO_REUSEPORT, 
                   &opt, sizeof(opt))) {
        perror("setsockopt");
        exit(EXIT_FAILURE);
    }
    
    address.sin_family = AF_INET;
    address.sin_addr.s_addr = INADDR_ANY;
    address.sin_port = htons(PORT);
    
    // Bind
    if (bind(server_fd, (struct sockaddr *)&address, sizeof(address)) < 0) {
        perror("bind failed");
        exit(EXIT_FAILURE);
    }
    
    // Listen
    if (listen(server_fd, 3) < 0) {
        perror("listen");
        exit(EXIT_FAILURE);
    }
    
    printf("Servidor ouvindo na porta %d...\n", PORT);
    
    // Aceitar uma conexão (em um processo filho para não bloquear)
    pid_t pid = fork();
    
    if (pid == 0) {
        // Processo filho - servidor
        if ((new_socket = accept(server_fd, (struct sockaddr *)&address, 
                               (socklen_t*)&addrlen)) < 0) {
            perror("accept");
            exit(EXIT_FAILURE);
        }
        
        // Ler dados do cliente
        ssize_t bytes_lidos = read(new_socket, buffer, BUFFER_SIZE);
        printf("Servidor recebeu: %s\n", buffer);
        
        // Enviar resposta
        const char *resposta = "Hello from server!";
        send(new_socket, resposta, strlen(resposta), 0);
        printf("Resposta enviada para cliente\n");
        
        close(new_socket);
        exit(0);
    } else {
        // Processo pai - cliente de teste
        sleep(1); // Dar tempo para o servidor iniciar
        
        int sock = 0;
        struct sockaddr_in serv_addr;
        
        if ((sock = socket(AF_INET, SOCK_STREAM, 0)) < 0) {
            perror("Socket creation error");
            return;
        }
        
        serv_addr.sin_family = AF_INET;
        serv_addr.sin_port = htons(PORT);
        
        // Converter IPv4 from text to binary
        inet_pton(AF_INET, "127.0.0.1", &serv_addr.sin_addr);
        
        if (connect(sock, (struct sockaddr *)&serv_addr, sizeof(serv_addr)) < 0) {
            perror("Connection Failed");
            return;
        }
        
        // Enviar mensagem para servidor
        const char *hello = "Hello from client!";
        send(sock, hello, strlen(hello), 0);
        printf("Mensagem enviada para servidor\n");
        
        // Ler resposta
        char buffer[BUFFER_SIZE] = {0};
        ssize_t bytes_lidos = read(sock, buffer, BUFFER_SIZE);
        printf("Cliente recebeu: %s\n", buffer);
        
        close(sock);
        wait(NULL); // Aguardar processo filho
    }
    
    close(server_fd);
}

void demonstrar_dns_lookup() {
    printf("\n=== RESOLUÇÃO DNS ===\n");
    
    struct hostent *host_info;
    const char *hostname = "google.com";
    
    host_info = gethostbyname(hostname);
    
    if (host_info == NULL) {
        herror("gethostbyname");
        return;
    }
    
    printf("Nome oficial: %s\n", host_info->h_name);
    printf("Tipo de endereço: %s\n", 
           host_info->h_addrtype == AF_INET ? "IPv4" : "IPv6");
    
    printf("Endereços IP:\n");
    for (int i = 0; host_info->h_addr_list[i] != NULL; i++) {
        struct in_addr *addr = (struct in_addr*)host_info->h_addr_list[i];
        printf("  %s\n", inet_ntoa(*addr));
    }
}

int main() {
    printf("=== SYS/SOCKET.H - PROGRAMAÇÃO DE REDE ===\n");
    
    demonstrar_conversao_enderecos();
    servidor_tcp();
    demonstrar_dns_lookup();
    
    return 0;
}
```

## 22. Exemplo Integrado Final - Sistema Completo

```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <unistd.h>
#include <pthread.h>
#include <sys/time.h>
#include <dirent.h>
#include <sys/socket.h>
#include <netinet/in.h>
#include <arpa/inet.h>
#include <signal.h>
#include <errno.h>

#define MAX_CLIENTS 10
#define BUFFER_SIZE 1024
#define PORT 9090

typedef struct {
    int socket;
    struct sockaddr_in address;
    pthread_t thread_id;
    time_t connect_time;
} ClientInfo;

typedef struct {
    ClientInfo clients[MAX_CLIENTS];
    int client_count;
    pthread_mutex_t mutex;
    int server_running;
} ServerState;

ServerState server_state;

void logger(const char *level, const char *message) {
    struct timeval tv;
    gettimeofday(&tv, NULL);
    
    struct tm *tm_info = localtime(&tv.tv_sec);
    
    printf("[%02d:%02d:%02d.%06ld] [%s] %s\n",
           tm_info->tm_hour, tm_info->tm_min, tm_info->tm_sec, tv.tv_usec,
           level, message);
}

void* client_handler(void* arg) {
    ClientInfo* client = (ClientInfo*) arg;
    char client_ip[INET_ADDRSTRLEN];
    char buffer[BUFFER_SIZE];
    
    inet_ntop(AF_INET, &client->address.sin_addr, client_ip, INET_ADDRSTRLEN);
    
    logger("INFO", strcat(strcpy(buffer, "Cliente conectado: "), client_ip));
    
    // Enviar mensagem de boas-vindas
    const char* welcome = "Bem-vindo ao servidor! Comandos: TIME, LIST, EXIT\n";
    send(client->socket, welcome, strlen(welcome), 0);
    
    while (server_state.server_running) {
        ssize_t bytes_received = recv(client->socket, buffer, BUFFER_SIZE - 1, 0);
        
        if (bytes_received <= 0) {
            break;
        }
        
        buffer[bytes_received] = '\0';
        
        // Remover newline
        if (buffer[strlen(buffer) - 1] == '\n') {
            buffer[strlen(buffer) - 1] = '\0';
        }
        
        logger("COMMAND", strcat(strcpy(buffer + BUFFER_SIZE/2, "Comando recebido: "), buffer));
        
        // Processar comandos
        if (strcmp(buffer, "TIME") == 0) {
            struct timeval tv;
            gettimeofday(&tv, NULL);
            
            struct tm *tm_info = localtime(&tv.tv_sec);
            char time_str[100];
            strftime(time_str, sizeof(time_str), 
                    "Hora atual: %Y-%m-%d %H:%M:%S", tm_info);
            
            send(client->socket, time_str, strlen(time_str), 0);
            
        } else if (strcmp(buffer, "LIST") == 0) {
            DIR *dir = opendir(".");
            if (dir == NULL) {
                send(client->socket, "Erro ao listar diretório", 24);
            } else {
                struct dirent *entry;
                char file_list[BUFFER_SIZE] = "Arquivos no diretório:\n";
                
                while ((entry = readdir(dir)) != NULL) {
                    if (entry->d_type == DT_REG) {
                        strncat(file_list, entry->d_name, 
                               BUFFER_SIZE - strlen(file_list) - 1);
                        strncat(file_list, "\n", 
                               BUFFER_SIZE - strlen(file_list) - 1);
                    }
                }
                
                closedir(dir);
                send(client->socket, file_list, strlen(file_list));
            }
            
        } else if (strcmp(buffer, "EXIT") == 0) {
            send(client->socket, "Goodbye!", 8);
            break;
            
        } else {
            const char* response = "Comando desconhecido. Use: TIME, LIST, EXIT\n";
            send(client->socket, response, strlen(response), 0);
        }
    }
    
    close(client->socket);
    
    pthread_mutex_lock(&server_state.mutex);
    for (int i = 0; i < server_state.client_count; i++) {
        if (server_state.clients[i].socket == client->socket) {
            // Remover cliente do array
            for (int j = i; j < server_state.client_count - 1; j++) {
                server_state.clients[j] = server_state.clients[j + 1];
            }
            server_state.client_count--;
            break;
        }
    }
    pthread_mutex_unlock(&server_state.mutex);
    
    logger("INFO", strcat(strcpy(buffer, "Cliente desconectado: "), client_ip));
    free(client);
    
    return NULL;
}

void* server_main(void* arg) {
    int server_fd;
    struct sockaddr_in address;
    int opt = 1;
    
    // Criar socket
    if ((server_fd = socket(AF_INET, SOCK_STREAM, 0)) == 0) {
        perror("socket failed");
        exit(EXIT_FAILURE);
    }
    
    // Configurar socket
    if (setsockopt(server_fd, SOL_SOCKET, SO_REUSEADDR | SO_REUSEPORT, 
                   &opt, sizeof(opt))) {
        perror("setsockopt");
        exit(EXIT_FAILURE);
    }
    
    address.sin_family = AF_INET;
    address.sin_addr.s_addr = INADDR_ANY;
    address.sin_port = htons(PORT);
    
    // Bind
    if (bind(server_fd, (struct sockaddr *)&address, sizeof(address)) < 0) {
        perror("bind failed");
        exit(EXIT_FAILURE);
    }
    
    // Listen
    if (listen(server_fd, 3) < 0) {
        perror("listen");
        exit(EXIT_FAILURE);
    }
    
    logger("INFO", "Servidor iniciado e ouvindo conexões...");
    
    while (server_state.server_running) {
        int new_socket;
        struct sockaddr_in client_address;
        int addrlen = sizeof(client_address);
        
        // Aceitar nova conexão
        new_socket = accept(server_fd, (struct sockaddr *)&client_address, 
                           (socklen_t*)&addrlen);
        
        if (new_socket < 0) {
            if (server_state.server_running) {
                perror("accept");
            }
            continue;
        }
        
        pthread_mutex_lock(&server_state.mutex);
        
        if (server_state.client_count < MAX_CLIENTS) {
            ClientInfo* new_client = malloc(sizeof(ClientInfo));
            new_client->socket = new_socket;
            new_client->address = client_address;
            new_client->connect_time = time(NULL);
            
            server_state.clients[server_state.client_count] = *new_client;
            
            // Criar thread para o cliente
            if (pthread_create(&server_state.clients[server_state.client_count].thread_id,
                              NULL, client_handler, (void*)new_client) == 0) {
                server_state.client_count++;
                logger("INFO", "Nova thread de cliente criada");
            } else {
                free(new_client);
                close(new_socket);
            }
        } else {
            const char* msg = "Servidor cheio. Tente novamente mais tarde.\n";
            send(new_socket, msg, strlen(msg), 0);
            close(new_socket);
        }
        
        pthread_mutex_unlock(&server_state.mutex);
    }
    
    close(server_fd);
    logger("INFO", "Servidor finalizado");
    
    return NULL;
}

void signal_handler(int sig) {
    logger("INFO", "Sinal de desligamento recebido. Finalizando servidor...");
    server_state.server_running = 0;
}

void print_server_status() {
    printf("\n=== STATUS DO SERVIDOR ===\n");
    printf("Clientes conectados: %d\n", server_state.client_count);
    printf("Servidor %s\n", server_state.server_running ? "RODANDO" : "PARADO");
    
    for (int i = 0; i < server_state.client_count; i++) {
        char client_ip[INET_ADDRSTRLEN];
        inet_ntop(AF_INET, &server_state.clients[i].address.sin_addr, 
                 client_ip, INET_ADDRSTRLEN);
        
        printf("Cliente %d: %s (conectado há %ld segundos)\n",
               i + 1, client_ip, time(NULL) - server_state.clients[i].connect_time);
    }
}

int main() {
    printf("=== SISTEMA DE SERVIDOR MULTITHREAD COMPLETO ===\n");
    
    // Inicializar estado do servidor
    memset(&server_state, 0, sizeof(server_state));
    server_state.server_running = 1;
    pthread_mutex_init(&server_state.mutex, NULL);
    
    // Configurar handlers de sinal
    signal(SIGINT, signal_handler);
    signal(SIGTERM, signal_handler);
    
    // Iniciar thread do servidor
    pthread_t server_thread;
    if (pthread_create(&server_thread, NULL, server_main, NULL) != 0) {
        perror("Erro ao criar thread do servidor");
        return 1;
    }
    
    // Menu de controle interativo
    char command[100];
    while (server_state.server_running) {
        printf("\nComandos: status, stop, exit\n");
        printf("> ");
        
        if (fgets(command, sizeof(command), stdin) == NULL) {
            break;
        }
        
        // Remover newline
        if (command[strlen(command) - 1] == '\n') {
            command[strlen(command) - 1] = '\0';
        }
        
        if (strcmp(command, "status") == 0) {
            print_server_status();
        } else if (strcmp(command, "stop") == 0) {
            logger("INFO", "Parando servidor por comando...");
            server_state.server_running = 0;
        } else if (strcmp(command, "exit") == 0) {
            server_state.server_running = 0;
            break;
        } else {
            printf("Comando desconhecido: %s\n", command);
        }
    }
    
    // Aguardar thread do servidor terminar
    pthread_join(server_thread, NULL);
    
    // Aguardar todas as threads de cliente terminarem
    pthread_mutex_lock(&server_state.mutex);
    for (int i = 0; i < server_state.client_count; i++) {
        pthread_join(server_state.clients[i].thread_id, NULL);
    }
    pthread_mutex_unlock(&server_state.mutex);
    
    pthread_mutex_destroy(&server_state.mutex);
    logger("INFO", "Sistema finalizado com sucesso");
    
    return 0;
}
```

> [!success] ## Conclusão
> Este guia completo cobre a maioria das funções essenciais das bibliotecas padrão do C, com exemplos práticos e um projeto integrado final que demonstra o uso combinado de várias funções.


