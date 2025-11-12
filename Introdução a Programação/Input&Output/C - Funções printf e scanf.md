
> [!note] ## Printf
> 
> É uma função usada para **mostrar dados no terminal**.  
Com ela, podemos exibir variáveis de diferentes tipos formatando a saída.

### Exemplo básico:

```c
#include <stdio.h>

int main() {
    int number = 10;
    float f = 10.5f;
    double d = 20.123456;
    char c = 'A';
    char str[] = "Hello";
    int *ptr = &number;

    printf("=== Demonstração de tipos de dados ===\n\n");
    printf("int: %d\n", number);
    printf("float: %.2f\n", f);
    printf("double: %.6lf\n", d);
    printf("char: %c\n", c);
    printf("string: %s\n", str);
    printf("hexadecimal (int): %x\n", number);
    printf("octal (int): %o\n", number);
    printf("endereço (ponteiro): %p\n", (void*)ptr);

    printf("\nOutras formas de exibir o mesmo número (%d):\n", number);
    printf("  Como char (tabela ASCII): %c\n", number);
    printf("  Como float: %f\n", (float)number);
    printf("  Como double: %lf\n", (double)number);
    printf("  Como hexadecimal: %#x\n", number);
    printf("  Como octal: %#o\n", number);
    printf("  Como endereço (forçado): %p\n", (void*)&number);

    return 0;
}
```

---

## Especificadores de Formato Básicos

```c
%d, %i    // inteiro decimal (signed)
%u        // inteiro decimal (unsigned)
%o        // octal
%x, %X    // hexadecimal (minúsculo/MAIÚSCULO)
%f        // float/double (ponto fixo)
%e, %E    // notação científica
%g, %G    // escolhe entre %f ou %e automaticamente
%c        // caractere único
%s        // string
%p        // ponteiro (endereço de memória)
%%        // imprime o símbolo %
```

---

## Modificadores de Largura e Precisão

```c
printf("%5d", 42);        // "   42" (largura mínima 5)
printf("%-5d", 42);       // "42   " (alinhado à esquerda)
printf("%05d", 42);       // "00042" (preenche com zeros)
printf("%.2f", 3.14159);  // "3.14" (2 casas decimais)
printf("%8.2f", 3.14);    // "    3.14" (largura 8, 2 decimais)
printf("%.3s", "hello");  // "hel" (primeiros 3 caracteres)
```

---

## Modificadores de Tamanho

```c
%hd       // short int
%ld       // long int
%lld      // long long int
%lu       // unsigned long
%lf       // double (em scanf)
%Lf       // long double
%zu       // size_t
%zd       // ssize_t
```

---

## Flags Especiais

```c
printf("%+d", 42);     // "+42" (sempre mostra sinal)
printf("% d", 42);     // " 42" (espaço se positivo)
printf("%#x", 255);    // "0xff" (adiciona prefixo)
printf("%#o", 8);      // "010" (adiciona prefixo octal)
printf("%'d", 1000000); // "1,000,000" (separador de milhares - depende da locale)
```

---

> [!note]  ## Caracteres Especiais de Escape
> `printf` permite usar **sequências de escape**, que começam com `\` (barra invertida).  
Elas **não são impressas literalmente**, mas **executam uma ação**, como pular linha, tabular, ou até mudar estilo no terminal.

|Sequência|Efeito|Exemplo no código|Saída esperada|
|---|---|---|---|
|`\n`|Nova linha|`printf("Olá\nMundo");`|Olá<br>Mundo|
|`\t`|Tab horizontal|`printf("A\tB\tC");`|A B C|
|`\v`|Tab vertical|—|—|
|`\r`|Retorno de carro (volta pro início da linha)|`printf("ABC\rX");`|XBC|
|`\b`|Backspace (apaga o caractere anterior)|`printf("AB\bC");`|AC|
|`\f`|Form feed (nova página)|—|—|
|`\a`|Alerta sonoro (beep) — depende do terminal|`printf("\a");`|🔊 (ou nada, se desativado)|
|`\\`|Barra invertida literal|`printf("\\");`|`\`|
|`\"`|Aspas duplas|`printf("\"Oi\"");`|"Oi"|
|`\'`|Aspas simples|`printf("\'Oi\'");`|'Oi'|
|`\0`|Caractere nulo (fim de string)|usado internamente|—|
|`\xHH`|Caractere em hexadecimal|`\x41` = 'A'|—|
|`\ooo`|Caractere em octal|`\101` = 'A'|—|

---

## Códigos ANSI — Estilização e Cores

### 🎨 Cores de texto (Foreground)

|Cor|Código|Exemplo|
|---|---|---|
|Preto|`\033[30m`|`printf("\033[30mPreto\033[0m\n");`|
|Vermelho|`\033[31m`|`printf("\033[31mVermelho\033[0m\n");`|
|Verde|`\033[32m`|`printf("\033[32mVerde\033[0m\n");`|
|Amarelo|`\033[33m`|`printf("\033[33mAmarelo\033[0m\n");`|
|Azul|`\033[34m`|`printf("\033[34mAzul\033[0m\n");`|
|Magenta|`\033[35m`|`printf("\033[35mMagenta\033[0m\n");`|
|Ciano|`\033[36m`|`printf("\033[36mCiano\033[0m\n");`|
|Branco (cinza claro)|`\033[37m`|`printf("\033[37mBranco\033[0m\n");`|
|**Reset (voltar padrão)**|`\033[0m`|—|

### Cores brilhantes (90-97 para texto)

```c
printf("\033[91m Vermelho Brilhante \033[0m\n");
printf("\033[92m Verde Brilhante \033[0m\n");
```

---

### 🌈 Cores de fundo (Background)

|Cor|Código|Exemplo|
|---|---|---|
|Preto|`\033[40m`|`printf("\033[40mFundo Preto\033[0m\n");`|
|Vermelho|`\033[41m`|`printf("\033[41mFundo Vermelho\033[0m\n");`|
|Verde|`\033[42m`|`printf("\033[42mFundo Verde\033[0m\n");`|
|Amarelo|`\033[43m`|`printf("\033[43mFundo Amarelo\033[0m\n");`|
|Azul|`\033[44m`|`printf("\033[44mFundo Azul\033[0m\n");`|
|Magenta|`\033[45m`|`printf("\033[45mFundo Magenta\033[0m\n");`|
|Ciano|`\033[46m`|`printf("\033[46mFundo Ciano\033[0m\n");`|
|Branco|`\033[47m`|`printf("\033[47mFundo Branco\033[0m\n");`|

### Fundos brilhantes (100-107)

```c
printf("\033[101m Fundo Vermelho Brilhante \033[0m\n");
printf("\033[102m Fundo Verde Brilhante \033[0m\n");
```

---

### 🧢 Estilos de texto

|Estilo|Código|Exemplo|
|---|---|---|
|Reset (padrão)|`\033[0m`|`printf("\033[0mTexto normal\n");`|
|Negrito|`\033[1m`|`printf("\033[1mNegrito\033[0m\n");`|
|Fraco (dim) / Escurecido|`\033[2m`|`printf("\033[2mMais fraco\033[0m\n");`|
|Itálico|`\033[3m`|`printf("\033[3mItálico\033[0m\n");`|
|Sublinhado|`\033[4m`|`printf("\033[4mSublinhado\033[0m\n");`|
|Piscar / Piscante|`\033[5m`|`printf("\033[5mPiscando\033[0m\n");`|
|Invertido (fundo/texto trocam)|`\033[7m`|`printf("\033[7mInverso\033[0m\n");`|
|Oculto (invisível)|`\033[8m`|`printf("\033[8mOculto\033[0m\n");`|
|Riscado|`\033[9m`|`printf("\033[9mRiscado\033[0m\n");`|

---

### 💥 Combinações poderosas

Você pode juntar **estilo + cor de texto + cor de fundo**:

```c
printf("\033[1;33;44mTexto Amarelo Negrito com Fundo Azul\033[0m\n");
printf("\033[3;35mItálico Magenta\033[0m\n");
printf("\033[4;32mSublinhado Verde\033[0m\n");
printf("\033[7;31mInverso Vermelho\033[0m\n");
printf("\033[1;4;31m Negrito Sublinhado Vermelho \033[0m\n");
printf("\033[1;3;38;5;208m Negrito Itálico Laranja \033[0m\n");
```

---

### 🌌 Cores avançadas (8-bit e 24-bit)

Alguns terminais modernos aceitam **256 cores** ou até **RGB real (24 bits)**:

**256 cores (RGB):**

```c
// 38;5 para texto, 48;5 para fundo
printf("\033[38;5;196mVermelho forte (256 cores)\033[0m\n");
printf("\033[48;5;27mFundo Azul neon (256 cores)\033[0m\n");
printf("\033[38;5;196m Vermelho RGB \033[0m\n");
printf("\033[48;5;21m Fundo Azul RGB \033[0m\n");
```

**RGB (24 bits) - True Color:**

```c
// 38;2 para texto, 48;2 para fundo
printf("\033[38;2;255;100;0mLaranja RGB\033[0m\n");
printf("\033[48;2;0;0;255mFundo Azul RGB\033[0m\n");
printf("\033[38;2;255;100;50m Laranja True Color \033[0m\n");
printf("\033[48;2;0;128;255m Fundo Azul True Color \033[0m\n");
```

---

## 7. Controle de Cursor

```c
printf("\033[H");          // Move cursor para home (0,0)
printf("\033[2J");         // Limpa a tela
printf("\033[K");          // Limpa até o fim da linha
printf("\033[10;20H");     // Move cursor para linha 10, coluna 20
printf("\033[A");          // Move cursor 1 linha acima
printf("\033[B");          // Move cursor 1 linha abaixo
printf("\033[C");          // Move cursor 1 coluna à direita
printf("\033[D");          // Move cursor 1 coluna à esquerda
printf("\033[s");          // Salva posição do cursor
printf("\033[u");          // Restaura posição do cursor
```

---

## 8. Exemplos Práticos Avançados

```c
// Barra de progresso
printf("\r[%-50s] %d%%", "####################", 40);

// Tabela formatada
printf("%-10s | %8s | %5s\n", "Nome", "Idade", "ID");
printf("%-10s | %8d | %5d\n", "João", 25, 1);

// Hexdump
printf("%08X: %02X %02X %02X %02X\n", addr, b1, b2, b3, b4);

// Alinhamento de números
printf("%10.2f\n", 123.456);  // "    123.46"
printf("%-10.2f\n", 123.456); // "123.46    "

// Largura e precisão dinâmicas
int largura = 10, precisao = 2;
printf("%*.*f\n", largura, precisao, 3.14159);
```

---

## 9. Macros Úteis para Cores

```c
#define RESET   "\033[0m"
#define RED     "\033[31m"
#define GREEN   "\033[32m"
#define YELLOW  "\033[33m"
#define BLUE    "\033[34m"
#define BOLD    "\033[1m"

printf(RED "Erro: " RESET "Arquivo não encontrado\n");
printf(GREEN BOLD "Sucesso!" RESET "\n");
```

---

## 10. Dicas Importantes

- **SEMPRE** use `\033[0m` para resetar as cores/estilos
- Códigos ANSI funcionam em Linux/Mac e Windows 10+ (com suporte habilitado)
- Para Windows antigo, use biblioteca `windows.h` com `SetConsoleTextAttribute`
- `printf` retorna o número de caracteres impressos
- Use `fflush(stdout)` após printf sem `\n` para forçar saída imediata

---

> [!note] ## Scanf 
>
> É uma função usada para **ler dados digitados pelo usuário** no terminal.  
Você precisa indicar o **tipo do dado** que será lido e passar o **endereço da variável** onde ele será armazenado (usando `&`).

---

### 1. Especificadores de Formato Básicos


```c
%d, %i    // inteiro decimal (signed)
%u        // inteiro decimal (unsigned)
%o        // octal
%x, %X    // hexadecimal
%f        // float
%lf       // double (IMPORTANTE: use %lf para double no scanf!)
%Lf       // long double
%c        // caractere único
%s        // string (para até encontrar espaço)
%[...]    // conjunto de caracteres permitidos
%[^...]   // conjunto de caracteres NÃO permitidos (ler até encontrar)
%%        // lê o símbolo % literal
```

**Exemplo básico:**


```c
#include <stdio.h>

int main() {
    int idade;
    float altura;
    double salario;
    char nome[50];

    printf("Digite sua idade: ");
    scanf("%d", &idade);

    printf("Digite sua altura: ");
    scanf("%f", &altura);

    printf("Digite seu salário: ");
    scanf("%lf", &salario);  // Note o %lf para double!

    printf("Digite seu nome: ");
    scanf("%s", nome);  // String NÃO precisa de &

    printf("\n--- Dados Informados ---\n");
    printf("Nome: %s\n", nome);
    printf("Idade: %d anos\n", idade);
    printf("Altura: %.2f m\n", altura);
    printf("Salário: %.2lf\n", salario);

    return 0;
}
```

---

### 2. Modificadores de Tamanho


```c
%hd       // short int
%ld       // long int
%lld      // long long int
%lu       // unsigned long
%llu      // unsigned long long
%Lf       // long double
%zu       // size_t
```

**Exemplo:**


```c
short s;
long l;
long long ll;
unsigned long ul;

scanf("%hd", &s);
scanf("%ld", &l);
scanf("%lld", &ll);
scanf("%lu", &ul);
```

---

### 3. Largura Máxima de Leitura

Você pode limitar quantos caracteres serão lidos:


```c
char str[10];
scanf("%9s", str);  // Lê no máximo 9 caracteres (deixa 1 para \0)

int num;
scanf("%3d", &num);  // Lê no máximo 3 dígitos
```

**Exemplo prático:**

```c
char codigo[5];
printf("Digite um código de 4 dígitos: ");
scanf("%4s", codigo);  // Garante no máximo 4 caracteres
```

---

### 4. Supressão de Atribuição (*)

Use `*` para **ler mas NÃO armazenar** o valor:

```c
int dia, ano;
scanf("%d%*c%*d%*c%d", &dia, &ano);  // Lê "25/12/2024" mas só pega dia e ano
```

**Exemplo útil - ignorar espaços:**

```c
int a, b, c;
scanf("%d%*c%d%*c%d", &a, &b, &c);  // Lê "10 20 30" ignorando espaços
```

---

### 5. Lendo Strings com Espaços

Por padrão, `%s` para no primeiro espaço. Para ler strings com espaços:

#### Método 1: `fgets()` (recomendado)

```c
char nome[50];
printf("Digite seu nome completo: ");
fgets(nome, 50, stdin);  // Lê até 49 caracteres ou \n
nome[strcspn(nome, "\n")] = '\0';  // Remove o \n do final
```

#### Método 2: `scanf` com `%[^\n]`

```c
char nome[50];
printf("Digite seu nome completo: ");
scanf(" %[^\n]", nome);  // Lê tudo até encontrar \n (nova linha)
```

#### Método 3: `gets()` (NÃO USE - inseguro!)

```c
// NÃO FAÇA ISSO - gets() é perigoso!
gets(nome);  // Causa buffer overflow!
```

---

### 6. Conjunto de Caracteres `%[...]`

Permite especificar exatamente quais caracteres aceitar:

```c
char letras[50];
scanf("%[a-zA-Z]", letras);  // Lê apenas letras

char numeros[50];
scanf("%[0-9]", numeros);  // Lê apenas números

char palavra[50];
scanf("%[abcde]", palavra);  // Lê apenas a, b, c, d ou e
```

---

### 7. Conjunto Negado `%[^...]`

Lê tudo **EXCETO** os caracteres especificados:

```c
char texto[100];
scanf("%[^\n]", texto);  // Lê tudo até encontrar \n (linha inteira)

char ate_virgula[50];
scanf("%[^,]", ate_virgula);  // Lê até encontrar vírgula

char ate_espaco[50];
scanf("%[^ ]", ate_espaco);  // Lê até encontrar espaço (igual %s)
```

**Exemplo prático - ler CSV:**

```c
char nome[50], cidade[50];
int idade;

printf("Digite: nome,idade,cidade\n");
scanf("%[^,],%d,%[^\n]", nome, &idade, cidade);
// Entrada: "João Silva,25,São Paulo"
```

---

### 8. Limpando o Buffer de Entrada

Problema comum: caracteres residuais (como `\n`) ficam no buffer.

#### Solução 1: Espaço antes do especificador

```c
char c1, c2;
scanf("%c", &c1);   // Lê caractere
scanf(" %c", &c2);  // O espaço consome \n residual
```

#### Solução 2: Limpar manualmente

```c
// Limpa tudo até o \n
while (getchar() != '\n');

// Ou use esta função:
void limpar_buffer() {
    int c;
    while ((c = getchar()) != '\n' && c != EOF);
}
```

#### Solução 3: `fflush(stdin)` (NÃO PORTÁVEL)

```c
fflush(stdin);  // Funciona no Windows, mas não é padrão ANSI C
```

---

### 9. Retorno do scanf

`scanf` retorna o **número de itens lidos com sucesso**:

```c
int idade, peso;
int resultado = scanf("%d %d", &idade, &peso);

if (resultado == 2) {
    printf("Leitura bem-sucedida!\n");
} else {
    printf("Erro na leitura!\n");
}
```

**Validação robusta:**

```c
int num;
printf("Digite um número: ");

if (scanf("%d", &num) != 1) {
    printf("Entrada inválida!\n");
    while (getchar() != '\n');  // Limpa o buffer
} else {
    printf("Você digitou: %d\n", num);
}
```

---

### 10. Lendo Múltiplos Valores

```c
int a, b, c;

// Com espaços
scanf("%d %d %d", &a, &b, &c);  // "10 20 30"

// Com vírgulas
scanf("%d,%d,%d", &a, &b, &c);  // "10,20,30"

// Com traços
scanf("%d-%d-%d", &a, &b, &c);  // "10-20-30"
```

---

### 11. Lendo Caracteres Específicos

```c
char c;
int dia, mes, ano;

// Lê formato "DD/MM/AAAA"
scanf("%d/%d/%d", &dia, &mes, &ano);

// Lê formato "(XX) XXXXX-XXXX"
char ddd[3], parte1[6], parte2[5];
scanf("(%2s) %5s-%4s", ddd, parte1, parte2);
```

---

### 12. Lendo Hexadecimal, Octal e Binário

```c
int hex, oct;

printf("Digite um número em hexadecimal: ");
scanf("%x", &hex);  // Entrada: FF (255 em decimal)

printf("Digite um número em octal: ");
scanf("%o", &oct);  // Entrada: 77 (63 em decimal)

// C não tem formato binário nativo no scanf
// Para ler binário, use strtol():
char bin[33];
scanf("%s", bin);
int decimal = strtol(bin, NULL, 2);  // Converte binário para decimal
```

---

### 13. Lendo Ponteiros (Endereços)

```c
void *ptr;
scanf("%p", &ptr);  // Lê um endereço de memória
printf("Endereço lido: %p\n", ptr);
```

---

### 14. Exemplos Práticos Avançados

#### Ler data completa:

```c
int dia, mes, ano;
scanf("%d/%d/%d", &dia, &mes, &ano);
printf("Data: %02d/%02d/%04d\n", dia, mes, ano);
```

#### Ler CPF:

```c
char cpf[12];
scanf("%11[0-9]", cpf);  // Lê apenas números, máximo 11 dígitos
printf("CPF: %.3s.%.3s.%.3s-%.2s\n", cpf, cpf+3, cpf+6, cpf+9);
```

#### Ler menu de opções:

```c
char opcao;
printf("Escolha (A/B/C): ");
scanf(" %c", &opcao);  // Espaço antes para ignorar \n

switch(opcao) {
    case 'A':
    case 'a':
        printf("Opção A\n");
        break;
    // ...
}
```

#### Ler múltiplas linhas:

```c
char linhas[5][100];
int i;

printf("Digite 5 linhas:\n");
getchar();  // Limpa buffer inicial

for (i = 0; i < 5; i++) {
    fgets(linhas[i], 100, stdin);
    linhas[i][strcspn(linhas[i], "\n")] = '\0';
}

for (i = 0; i < 5; i++) {
    printf("Linha %d: %s\n", i+1, linhas[i]);
}
```

---

### 15. Alternativas ao scanf

#### `getchar()` - Lê um único caractere

```c
char c = getchar();
```

#### `gets()` - NÃO USE (inseguro)

```c
// NUNCA USE ISSO!
gets(str);  // Buffer overflow!
```

#### `fgets()` - Alternativa segura (RECOMENDADO)

```c
char linha[100];
fgets(linha, 100, stdin);
linha[strcspn(linha, "\n")] = '\0';  // Remove \n
```

#### `sscanf()` - Lê de uma string

```c
char texto[] = "João 25 1.75";
char nome[50];
int idade;
float altura;

sscanf(texto, "%s %d %f", nome, &idade, &altura);
```

---
### 16. Problemas Comuns e Soluções

#### Problema: scanf não lê string após número

```c
int num;
char str[50];

scanf("%d", &num);      // Deixa \n no buffer
scanf("%s", str);       // Pula por causa do \n

// SOLUÇÃO:
scanf("%d", &num);
getchar();              // Consome o \n
scanf("%s", str);
```

#### Problema: ler char após scanf

```c
int num;
char c;

scanf("%d", &num);   // Deixa \n
scanf("%c", &c);     // Lê o \n, não o que você quer!

// SOLUÇÃO 1:
scanf("%d", &num);
scanf(" %c", &c);    // Espaço antes de %c

// SOLUÇÃO 2:
scanf("%d", &num);
getchar();
c = getchar();
```

#### Problema: string com espaços

```c
// ERRADO:
scanf("%s", nome);  // Para no primeiro espaço

// CORRETO:
scanf(" %[^\n]", nome);  // Lê linha inteira
// OU
fgets(nome, 50, stdin);
```

---

### 17. Validação Completa de Entrada

```c
int ler_inteiro() {
    int num;
    int resultado;
    
    do {
        printf("Digite um número inteiro: ");
        resultado = scanf("%d", &num);
        
        if (resultado != 1) {
            printf("Entrada inválida! Tente novamente.\n");
            while (getchar() != '\n');  // Limpa buffer
        }
    } while (resultado != 1);
    
    return num;
}

int main() {
    int numero = ler_inteiro();
    printf("Você digitou: %d\n", numero);
    return 0;
}
```

---

### 18. Dicas Importantes

- **SEMPRE** use `&` antes da variável (exceto arrays e ponteiros)
- Para `double`, use `%lf` no scanf (não `%f`)
- String não precisa de `&` (arrays já são ponteiros)
- Use espaço antes de `%c` para ignorar whitespace
- Valide SEMPRE o retorno do scanf
- Prefira `fgets()` para strings com espaços
- Limpe o buffer após scanf quando necessário
- Use `%[^\n]` para ler linha completa com scanf
- Limite o tamanho de strings com `%Ns` (ex: `%49s` para array de 50)