
> [!note] ## Por que Validar Entrada?
> 
> A validação de entrada é **crítica** em C porque:
> - **Evita comportamentos indefinidos** e falhas no programa
> - **Previne vulnerabilidades** de segurança (buffer overflow, etc)
> - **Garante que os dados** estejam no formato esperado
> - **Melhora a experiência** do usuário com feedback adequado
> 
> **Exemplo do problema:**
> ```c
> int idade;
> printf("Digite sua idade: ");
> scanf("%d", &idade);  // E se usuário digitar "abc"?
> ```
> **Resultado:** Comportamento indefinido! O programa pode crashar ou continuar com valores lixo.

---

## 1. Validação Básica com scanf

### Verificando o Retorno do scanf

O `scanf()` retorna o **número de itens** lidos com sucesso:

```c
int num;
printf("Digite um número: ");

int itens_lidos = scanf("%d", &num);

if (itens_lidos == 1) {
    printf("Número válido: %d\n", num);
} else {
    printf("Entrada inválida!\n");
    // Limpar buffer de entrada
    while (getchar() != '\n'); 
}
```

**Tabela de retornos do scanf:**

| Situação | Retorno | Significado |
|----------|---------|-------------|
| Sucesso | `> 0` | Número de variáveis preenchidas |
| Falha na conversão | `0` | Dado incompatível com especificador |
| Fim de arquivo (Ctrl+D/Z) | `EOF` | Nenhum item lido |
| Erro de leitura | `EOF` | Erro no dispositivo de entrada |

---

## 2. Validação de Números Inteiros

### Validação com Limites

```c
#include <limits.h>

int ler_int(int min, int max) {
    int num;
    int tentativas = 0;
    
    while (tentativas < 3) {
        printf("Digite um número entre %d e %d: ", min, max);
        
        if (scanf("%d", &num) != 1) {
            printf("Erro: Digite um número válido!\n");
            while (getchar() != '\n'); // Limpa buffer
            tentativas++;
            continue;
        }
        
        // Verifica limites
        if (num >= min && num <= max) {
            return num; // Sucesso!
        } else {
            printf("Erro: Número fora do intervalo permitido!\n");
            tentativas++;
        }
    }
    
    printf("Muitas tentativas falhas. Usando valor padrão.\n");
    return min; // Valor padrão após falhas
}
```

**Uso:**
```c
int idade = ler_int(0, 150);
int nota = ler_int(0, 10);
```

---

## 3. Validação de Números Decimais

### Validação de Float/Double

```c
double ler_double(double min, double max) {
    double num;
    char buffer[100];
    
    while (1) {
        printf("Digite um número decimal entre %.2f e %.2f: ", min, max);
        
        // Lê como string primeiro para melhor controle
        if (fgets(buffer, sizeof(buffer), stdin) == NULL) {
            printf("Erro de leitura!\n");
            continue;
        }
        
        // Tenta converter para double
        char *endptr;
        num = strtod(buffer, &endptr);
        
        // Verifica se a conversão foi completa
        if (endptr == buffer) {
            printf("Erro: Nenhum número encontrado!\n");
        } else if (*endptr != '\n' && *endptr != '\0') {
            printf("Erro: Caracteres extras após o número!\n");
        } else if (num < min || num > max) {
            printf("Erro: Fora do intervalo permitido!\n");
        } else {
            return num; // Sucesso!
        }
    }
}
```

---

## 4. Validação de Strings e Texto

### Evitando Buffer Overflow

```c
#include <string.h>

void ler_string_segura(char *buffer, int tamanho_max) {
    printf("Digite um texto: ");
    
    if (fgets(buffer, tamanho_max, stdin) == NULL) {
        buffer[0] = '\0';
        return;
    }
    
    // Remove o \n do final, se existir
    size_t len = strlen(buffer);
    if (len > 0 && buffer[len-1] == '\n') {
        buffer[len-1] = '\0';
    } else {
        // Buffer estava cheio - limpar entrada restante
        while (getchar() != '\n');
    }
}
```

**Uso seguro:**
```c
char nome[51]; // 50 caracteres + \0
ler_string_segura(nome, sizeof(nome));
printf("Nome: %s\n", nome);
```

### Validação de Formato Específico

```c
int validar_email(const char *email) {
    int tem_arroba = 0;
    int tem_ponto = 0;
    int comprimento = strlen(email);
    
    if (comprimento < 5 || comprimento > 254) {
        return 0; // Comprimento inválido
    }
    
    for (int i = 0; email[i] != '\0'; i++) {
        if (email[i] == '@') {
            if (tem_arroba) return 0; // Mais de um @
            tem_arroba = 1;
        } else if (email[i] == '.' && tem_arroba) {
            tem_ponto = 1;
        }
    }
    
    return (tem_arroba && tem_ponto);
}
```

---

## 5. Validação de Caracteres

### Menu com Validação

```c
char ler_opcao(const char *opcoes_validas) {
    char opcao;
    char buffer[10];
    
    while (1) {
        printf("Escolha uma opção (%s): ", opcoes_validas);
        
        if (fgets(buffer, sizeof(buffer), stdin) == NULL) {
            continue;
        }
        
        if (strlen(buffer) == 2 && buffer[1] == '\n') {
            opcao = buffer[0];
            
            // Verifica se a opção é válida
            if (strchr(opcoes_validas, opcao) != NULL) {
                return opcao;
            }
        }
        
        printf("Opção inválida! Tente novamente.\n");
    }
}
```

**Uso:**
```c
char opcao = ler_opcao("SsNn");
if (opcao == 'S' || opcao == 's') {
    printf("Você escolheu SIM\n");
}
```

---

## 6. Técnicas Avançadas de Validação

### Validação com Expressões Regulares (regex.h)

```c
#include <regex.h>

int validar_com_regex(const char *padrao, const char *texto) {
    regex_t regex;
    int resultado;
    
    // Compila a expressão regular
    if (regcomp(&regex, padrao, REG_EXTENDED) != 0) {
        return 0; // Falha ao compilar
    }
    
    // Executa a validação
    resultado = regexec(&regex, texto, 0, NULL, 0);
    
    regfree(&regex); // Libera memória
    
    return (resultado == 0); // 0 = match encontrado
}
```

**Exemplos de uso:**
```c
// Valida telefone (XX) XXXXX-XXXX
if (validar_com_regex("^\\([0-9]{2}\\) [0-9]{5}-[0-9]{4}$", telefone)) {
    printf("Telefone válido!\n");
}

// Valida data DD/MM/AAAA
if (validar_com_regex("^[0-9]{2}/[0-9]{2}/[0-9]{4}$", data)) {
    printf("Data válida!\n");
}
```

### Validação de CPF

```c
int validar_cpf(const char *cpf) {
    int digito1, digito2, temp = 0;
    int cpf_int[11];
    
    // Converte string para array de inteiros
    for (int i = 0; i < 11; i++) {
        if (cpf[i] < '0' || cpf[i] > '9') return 0;
        cpf_int[i] = cpf[i] - '0';
    }
    
    // Calcula primeiro dígito verificador
    for (int i = 0; i < 9; i++) {
        temp += cpf_int[i] * (10 - i);
    }
    temp %= 11;
    digito1 = (temp < 2) ? 0 : (11 - temp);
    
    // Calcula segundo dígito verificador
    temp = 0;
    for (int i = 0; i < 10; i++) {
        temp += cpf_int[i] * (11 - i);
    }
    temp %= 11;
    digito2 = (temp < 2) ? 0 : (11 - temp);
    
    // Verifica se os dígitos calculados batem com os informados
    return (cpf_int[9] == digito1 && cpf_int[10] == digito2);
}
```

---

## 7. Limpeza do Buffer de Entrada

### Funções Úteis para Limpar Buffer

```c
void limpar_buffer() {
    int c;
    while ((c = getchar()) != '\n' && c != EOF);
}

void limpar_buffer_ate_novo_line() {
    int c;
    do {
        c = getchar();
    } while (c != '\n' && c != EOF);
}

// Versão mais segura para uso após scanf
int ler_e_validar_int(int *valor, int min, int max) {
    char buffer[100];
    
    if (fgets(buffer, sizeof(buffer), stdin) == NULL) {
        return 0; // Falha
    }
    
    char *endptr;
    *valor = strtol(buffer, &endptr, 10);
    
    // Verifica se a conversão foi bem-sucedida
    if (endptr == buffer || *endptr != '\n') {
        return 0; // Caracteres inválidos
    }
    
    return (*valor >= min && *valor <= max);
}
```

---

## 8. Validação de Arquivos

### Verificando Abertura de Arquivos

```c
FILE* abrir_arquivo_seguro(const char *filename, const char *mode) {
    FILE *arquivo = fopen(filename, mode);
    
    if (arquivo == NULL) {
        perror("Erro ao abrir arquivo");
        return NULL;
    }
    
    // Verifica se é um arquivo regular (não diretório, etc)
    struct stat st;
    if (fstat(fileno(arquivo), &st) == 0) {
        if (!S_ISREG(st.st_mode)) {
            printf("Erro: Não é um arquivo regular!\n");
            fclose(arquivo);
            return NULL;
        }
    }
    
    return arquivo;
}
```

---

## 9. Sistema de Validação Modular

### Estrutura para Validações Complexas

```c
typedef struct {
    int sucesso;
    char mensagem_erro[100];
    union {
        int int_valor;
        double double_valor;
        char string_valor[100];
    } dado;
} ResultadoValidacao;

ResultadoValidacao validar_idade(int idade) {
    ResultadoValidacao resultado = {0, "", {0}};
    
    if (idade < 0) {
        strcpy(resultado.mensagem_erro, "Idade não pode ser negativa");
    } else if (idade > 150) {
        strcpy(resultado.mensagem_erro, "Idade muito alta");
    } else {
        resultado.sucesso = 1;
        resultado.dado.int_valor = idade;
    }
    
    return resultado;
}

ResultadoValidacao validar_nome(const char *nome) {
    ResultadoValidacao resultado = {0, "", {0}};
    size_t comprimento = strlen(nome);
    
    if (comprimento < 2) {
        strcpy(resultado.mensagem_erro, "Nome muito curto");
    } else if (comprimento > 50) {
        strcpy(resultado.mensagem_erro, "Nome muito longo");
    } else {
        resultado.sucesso = 1;
        strcpy(resultado.dado.string_valor, nome);
    }
    
    return resultado;
}
```

---

## 10. Exemplo Completo: Sistema de Cadastro

```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <ctype.h>

typedef struct {
    char nome[51];
    int idade;
    double salario;
    char email[101];
} Pessoa;

int validar_email(const char *email) {
    int tem_arroba = 0, tem_ponto_apos_arroba = 0;
    
    for (int i = 0; email[i] != '\0'; i++) {
        if (email[i] == '@') {
            if (tem_arroba) return 0;
            tem_arroba = 1;
        } else if (email[i] == '.' && tem_arroba) {
            tem_ponto_apos_arroba = 1;
        }
    }
    
    return (tem_arroba && tem_ponto_apos_arroba);
}

void ler_string_segura(char *buffer, int tamanho) {
    if (fgets(buffer, tamanho, stdin) == NULL) {
        buffer[0] = '\0';
        return;
    }
    
    size_t len = strlen(buffer);
    if (len > 0 && buffer[len-1] == '\n') {
        buffer[len-1] = '\0';
    }
}

int ler_int_validado(const char *prompt, int min, int max) {
    int valor;
    char buffer[100];
    
    while (1) {
        printf("%s", prompt);
        
        if (fgets(buffer, sizeof(buffer), stdin) == NULL) {
            continue;
        }
        
        char *endptr;
        valor = strtol(buffer, &endptr, 10);
        
        if (endptr == buffer || (*endptr != '\n' && *endptr != '\0')) {
            printf("Erro: Digite apenas números!\n");
        } else if (valor < min || valor > max) {
            printf("Erro: Valor deve estar entre %d e %d!\n", min, max);
        } else {
            return valor;
        }
    }
}

double ler_double_validado(const char *prompt, double min, double max) {
    double valor;
    char buffer[100];
    
    while (1) {
        printf("%s", prompt);
        
        if (fgets(buffer, sizeof(buffer), stdin) == NULL) {
            continue;
        }
        
        char *endptr;
        valor = strtod(buffer, &endptr, 10);
        
        if (endptr == buffer || (*endptr != '\n' && *endptr != '\0')) {
            printf("Erro: Digite um número decimal válido!\n");
        } else if (valor < min || valor > max) {
            printf("Erro: Valor deve estar entre %.2f e %.2f!\n", min, max);
        } else {
            return valor;
        }
    }
}

void cadastrar_pessoa(Pessoa *p) {
    printf("\n=== CADASTRO DE PESSOA ===\n");
    
    // Nome
    while (1) {
        printf("Nome: ");
        ler_string_segura(p->nome, sizeof(p->nome));
        
        if (strlen(p->nome) >= 2) {
            break;
        }
        printf("Erro: Nome deve ter pelo menos 2 caracteres!\n");
    }
    
    // Idade
    p->idade = ler_int_validado("Idade: ", 0, 150);
    
    // Salário
    p->salario = ler_double_validado("Salário: ", 0.0, 1000000.0);
    
    // Email
    while (1) {
        printf("Email: ");
        ler_string_segura(p->email, sizeof(p->email));
        
        if (validar_email(p->email)) {
            break;
        }
        printf("Erro: Email inválido! Use o formato usuario@dominio.com\n");
    }
}

int main() {
    Pessoa pessoa;
    
    cadastrar_pessoa(&pessoa);
    
    printf("\n=== DADOS CADASTRADOS ===\n");
    printf("Nome: %s\n", pessoa.nome);
    printf("Idade: %d anos\n", pessoa.idade);
    printf("Salário: R$ %.2f\n", pessoa.salario);
    printf("Email: %s\n", pessoa.email);
    
    return 0;
}
```

---

## 11. Tabela Resumo de Validações

| Tipo de Dado | Técnica | Funções Úteis | Cuidados |
|-------------|---------|---------------|----------|
| **Inteiros** | Verificar retorno scanf, strtol | `scanf()`, `strtol()`, `getchar()` | Overflow, caracteres extras |
| **Decimais** | strtod, verificar limites | `strtod()`, `modf()` | Precisão, notação científica |
| **Strings** | fgets com limite, strlen | `fgets()`, `strlen()`, `strncpy()` | Buffer overflow, injeção |
| **Caracteres** | Leitura individual, strchr | `getchar()`, `fgetc()`, `strchr()` | Buffer, case sensitivity |
| **Arquivos** | Verificar NULL, fstat | `fopen()`, `fstat()`, `perror()` | Permissões, existência |
| **Formato** | Expressões regulares | `regex.h`, validação manual | Complexidade, performance |

---

## 12. Dicas Finais de Validação

✅ **Sempre verifique o retorno** das funções de entrada  
✅ **Use fgets + conversão** em vez de scanf para mais controle  
✅ **Estabeleça limites claros** para todos os inputs  
✅ **Limpe o buffer** após entradas inválidas  
✅ **Forneça feedback claro** ao usuário sobre erros  
✅ **Valide tanto o formato quanto o conteúdo**  
✅ **Use funções modularizadas** para reutilização  
✅ **Considere segurança** contra buffer overflow  
✅ **Teste casos extremos** (valores muito altos/baixos)  
✅ **Documente as regras** de validação usadas  

**Exemplo de abordagem defensiva:**
```c
// RUIM - sem validação
scanf("%s", buffer);

// BOM - com validação completa
if (fgets(buffer, sizeof(buffer), stdin)) {
    buffer[strcspn(buffer, "\n")] = '\0'; // Remove \n
    if (validar_conteudo(buffer)) {
        // Processa dados
    }
}
```

A validação robusta de entrada é essencial para criar programas C confiáveis e seguros!