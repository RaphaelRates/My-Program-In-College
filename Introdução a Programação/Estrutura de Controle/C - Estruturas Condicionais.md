
> [!note] ## O que são Estruturas Condicionais?
> 
> Estruturas condicionais permitem que o programa **tome decisões** e execute diferentes blocos de código baseado em condições. Elas são fundamentais para criar programas inteligentes que respondem a diferentes situações.
> 
> **Analogia:** "Se estiver chovendo, leve guarda-chuva. Senão, use óculos de sol."
> 
> ```c
> if (chovendo) {
>     levar_guarda_chuva();
> } else {
>     usar_oculos_sol();
> }
> ```

---

## 1. A Estrutura IF Básica

### Sintaxe do IF Simples

```c
if (condicao) {
    // código executado se condicao for VERDADEIRA
}
```

**Exemplo Prático:**
```c
#include <stdio.h>

int main() {
    int idade;
    
    printf("Digite sua idade: ");
    scanf("%d", &idade);
    
    if (idade >= 18) {
        printf("Você é maior de idade!\n");
    }
    
    return 0;
}
```

### Operadores de Comparação

| Operador | Significado    | Exemplo  | Resultado   |
| -------- | -------------- | -------- | ----------- |
| ==       | Igual a        | `5 == 5` | `1` (true)  |
| `!=`     | Diferente de   | `5 != 3` | `1` (true)  |
| `>`      | Maior que      | `5 > 3`  | `1` (true)  |
| `<`      | Menor que      | `5 < 3`  | `0` (false) |
| `>=`     | Maior ou igual | `5 >= 5` | `1` (true)  |
| `<=`     | Menor ou igual | `5 <= 3` | `0` (false) |

**Exemplos:**
```c
int a = 10, b = 20;

printf("%d\n", a == b);  // 0 (false)
printf("%d\n", a != b);  // 1 (true)
printf("%d\n", a < b);   // 1 (true)
printf("%d\n", a >= b);  // 0 (false)
```

---
## 2. IF-ELSE

### Decisão Entre Duas Opções

```c
if (condicao) {
    // Bloco IF - executado se condicao for verdadeira
} else {
    // Bloco ELSE - executado se condicao for falsa
}
```

**Exemplo Completo:**
```c
#include <stdio.h>

int main() {
    int numero;
    
    printf("Digite um número: ");
    scanf("%d", &numero);
    
    if (numero % 2 == 0) {
        printf("%d é PAR\n", numero);
    } else {
        printf("%d é ÍMPAR\n", numero);
    }
    
    return 0;
}
```

### Exemplo: Sistema de Notas

```c
#include <stdio.h>

int main() {
    float nota;
    
    printf("Digite a nota (0-10): ");
    scanf("%f", &nota);
    
    if (nota >= 6.0) {
        printf("APROVADO! 🎉\n");
    } else {
        printf("REPROVADO 😞\n");
        printf("Estude mais para a recuperação!\n");
    }
    
    return 0;
}
```

---

## 3. ELSE-IF para Múltiplas Condições

### Cadeia de Decisões

```c
if (condicao1) {
    // Executado se condicao1 for verdadeira
} else if (condicao2) {
    // Executado se condicao1 for falsa E condicao2 verdadeira
} else if (condicao3) {
    // Executado se condicoes anteriores falsas E condicao3 verdadeira
} else {
    // Executado se todas as condições forem falsas
}
```

**Exemplo: Sistema de Conceitos**

```c
#include <stdio.h>

int main() {
    float nota;
    
    printf("Digite a nota (0-10): ");
    scanf("%f", &nota);
    
    if (nota >= 9.0) {
        printf("Conceito: A 👍\n");
    } else if (nota >= 7.0) {
        printf("Conceito: B 👌\n");
    } else if (nota >= 5.0) {
        printf("Conceito: C 🤔\n");
    } else {
        printf("Conceito: D ❌\n");
    }
    
    return 0;
}
```

### Exemplo: Calculadora de IMC

```c
#include <stdio.h>

int main() {
    float peso, altura, imc;
    
    printf("Digite seu peso (kg): ");
    scanf("%f", &peso);
    printf("Digite sua altura (m): ");
    scanf("%f", &altura);
    
    imc = peso / (altura * altura);
    printf("Seu IMC é: %.2f\n", imc);
    
    if (imc < 18.5) {
        printf("Classificação: Abaixo do peso\n");
    } else if (imc < 25) {
        printf("Classificação: Peso normal\n");
    } else if (imc < 30) {
        printf("Classificação: Sobrepeso\n");
    } else if (imc < 35) {
        printf("Classificação: Obesidade Grau I\n");
    } else if (imc < 40) {
        printf("Classificação: Obesidade Grau II\n");
    } else {
        printf("Classificação: Obesidade Grau III\n");
    }
    
    return 0;
}
```

---

## 4. IF's Aninhados

### Decisões Dentro de Decisões

```c
if (condicao_externa) {
    // Código executado se condicao_externa for verdadeira
    
    if (condicao_interna) {
        // Código executado se AMBAS condições forem verdadeiras
    }
}
```

**Exemplo: Sistema de Login**

```c
#include <stdio.h>
#include <string.h>

int main() {
    char usuario[20];
    char senha[20];
    int idade;
    
    printf("Usuário: ");
    scanf("%s", usuario);
    printf("Senha: ");
    scanf("%s", senha);
    printf("Idade: ");
    scanf("%d", &idade);
    
    if (strcmp(usuario, "admin") == 0) {
        if (strcmp(senha, "1234") == 0) {
            if (idade >= 18) {
                printf("Login realizado com sucesso! ✅\n");
                printf("Bem-vindo ao sistema!\n");
            } else {
                printf("Acesso negado: Menor de idade! 🔞\n");
            }
        } else {
            printf("Senha incorreta! ❌\n");
        }
    } else {
        printf("Usuário não encontrado! ❌\n");
    }
    
    return 0;
}
```

---

## 5. Operadores Lógicos

### Combinando Múltiplas Condições

| Operador | Significado | Exemplo |
|----------|-------------|---------|
| `&&` | E (AND) | `(a > 0) && (a < 10)` |
| `||` | OU (OR) | `(x == 5) || (y == 5)` |
| `!` | NÃO (NOT) | `!(a == b)` |

**Tabela Verdade AND (&&):**
| A | B | A && B |
|---|---|--------|
| 0 | 0 | 0 |
| 0 | 1 | 0 |
| 1 | 0 | 0 |
| 1 | 1 | 1 |

**Tabela Verdade OR (||):**
| A | B | A || B |
|---|---|--------|
| 0 | 0 | 0 |
| 0 | 1 | 1 |
| 1 | 0 | 1 |
| 1 | 1 | 1 |

**Exemplos Práticos:**

```c
int idade = 25;
float salario = 5000.0;

// AND - ambas condições devem ser verdadeiras
if (idade >= 18 && salario > 2000) {
    printf("Pode solicitar empréstimo\n");
}

// OR - pelo menos uma condição verdadeira
if (idade < 12 || idade > 65) {
    printf("Passagem gratuita\n");
}

// NOT - inverte o resultado
if (!(idade < 18)) {
    printf("Maior de idade\n");
}
```

### Exemplo Completo com Operadores Lógicos

```c
#include <stdio.h>

int main() {
    int hora;
    
    printf("Digite a hora (0-23): ");
    scanf("%d", &hora);
    
    if (hora >= 6 && hora < 12) {
        printf("Bom dia! ☀️\n");
    } else if (hora >= 12 && hora < 18) {
        printf("Boa tarde! 🌤️\n");
    } else if ((hora >= 18 && hora <= 23) || (hora >= 0 && hora < 6)) {
        printf("Boa noite! 🌙\n");
    } else {
        printf("Hora inválida! ❌\n");
    }
    
    // Verificando se é horário comercial
    if (hora >= 8 && hora <= 18 && !(hora == 12)) {
        printf("Estamos em horário comercial 📊\n");
    }
    
    return 0;
}
```

---

## 6. Operador Ternário

### IF-ELSE em Uma Linha

**Sintaxe:**
```c
condicao ? expressao_se_verdadeiro : expressao_se_falso;
```

**Exemplos:**
```c
int a = 10, b = 20;
int maior;

// Forma tradicional
if (a > b) {
    maior = a;
} else {
    maior = b;
}

// Forma ternária
maior = (a > b) ? a : b;
```

**Mais Exemplos:**
```c
int idade = 17;
char *status = (idade >= 18) ? "adulto" : "menor";

float nota = 7.5;
char *situacao = (nota >= 6.0) ? "aprovado" : "reprovado";

int numero = 15;
printf("%d é %s\n", numero, (numero % 2 == 0) ? "par" : "ímpar");
```

### Exemplo Prático com Ternário

```c
#include <stdio.h>

int main() {
    float preco;
    int quantidade;
    
    printf("Preço unitário: ");
    scanf("%f", &preco);
    printf("Quantidade: ");
    scanf("%d", &quantidade);
    
    // Aplica desconto se quantidade > 10
    float total = (quantidade > 10) 
                 ? (preco * quantidade * 0.9)  // 10% de desconto
                 : (preco * quantidade);       // preço normal
    
    printf("Total: R$ %.2f\n", total);
    printf("%s\n", (quantidade > 10) ? "Desconto aplicado! 🎉" : "Sem desconto");
    
    return 0;
}
```

---

## 7. A Estrutura SWITCH-CASE

### Para Múltiplas Opções Discretas

**Sintaxe:**
```c
switch (variavel) {
    case valor1:
        // código para valor1
        break;
    case valor2:
        // código para valor2
        break;
    // ...
    default:
        // código se nenhum case corresponder
}
```

**Exemplo: Menu de Opções**

```c
#include <stdio.h>

int main() {
    int opcao;
    
    printf("=== MENU ===\n");
    printf("1. Cadastrar\n");
    printf("2. Consultar\n");
    printf("3. Excluir\n");
    printf("4. Sair\n");
    printf("Escolha uma opção: ");
    scanf("%d", &opcao);
    
    switch (opcao) {
        case 1:
            printf("Cadastrando usuário...\n");
            // código do cadastro
            break;
            
        case 2:
            printf("Consultando dados...\n");
            // código da consulta
            break;
            
        case 3:
            printf("Excluindo registro...\n");
            // código da exclusão
            break;
            
        case 4:
            printf("Saindo do sistema...\n");
            break;
            
        default:
            printf("Opção inválida! ❌\n");
    }
    
    return 0;
}
```

### Switch com Múltiplos Cases

```c
#include <stdio.h>

int main() {
    char conceito;
    
    printf("Digite o conceito (A-F): ");
    scanf(" %c", &conceito);
    
    switch (conceito) {
        case 'A':
        case 'a':
            printf("Excelente! 🎉\n");
            printf("Nota: 9.0 - 10.0\n");
            break;
            
        case 'B':
        case 'b':
            printf("Muito bom! 👍\n");
            printf("Nota: 8.0 - 8.9\n");
            break;
            
        case 'C':
        case 'c':
            printf("Bom 👌\n");
            printf("Nota: 7.0 - 7.9\n");
            break;
            
        case 'D':
        case 'd':
            printf("Regular 🤔\n");
            printf("Nota: 6.0 - 6.9\n");
            break;
            
        case 'F':
        case 'f':
            printf("Reprovado ❌\n");
            printf("Nota: 0.0 - 5.9\n");
            break;
            
        default:
            printf("Conceito inválido! ❌\n");
    }
    
    return 0;
}
```

---

## 8. Diferenças Entre SWITCH e IF-ELSE

| Característica | SWITCH | IF-ELSE |
|----------------|--------|---------|
| **Tipo de dados** | Inteiros e char | Qualquer tipo |
| **Condições** | Igualdade (`==`) | Qualquer operador |
| **Múltiplas condições** | Cases múltiplos | Operadores lógicos |
| **Intervalos** | Não suporta | Suporta com operadores |
| **Legibilidade** | Boa para opções discretas | Boa para condições complexas |

**Quando usar cada um:**

```c
// SWITCH - melhor para opções discretas
int dia_semana = 3;
switch (dia_semana) {
    case 1: printf("Domingo\n"); break;
    case 2: printf("Segunda\n"); break;
    // ...
}

// IF-ELSE - melhor para intervalos e condições complexas
int temperatura = 25;
if (temperatura < 0) {
    printf("Congelante! ❄️\n");
} else if (temperatura < 20) {
    printf("Frio 🧥\n");
} else if (temperatura < 30) {
    printf("Agradável 😊\n");
} else {
    printf("Quente! 🔥\n");
}
```

---

## 9. Exemplos Práticos Avançados

### Sistema de Autenticação

```c
#include <stdio.h>
#include <string.h>

int main() {
    char username[20];
    char password[20];
    int tentativas = 3;
    
    while (tentativas > 0) {
        printf("\nTentativas restantes: %d\n", tentativas);
        printf("Usuário: ");
        scanf("%s", username);
        printf("Senha: ");
        scanf("%s", password);
        
        if (strcmp(username, "admin") == 0 && strcmp(password, "1234") == 0) {
            printf("\nAcesso concedido! ✅\n");
            
            // Sistema principal
            int opcao;
            printf("\n=== SISTEMA PRINCIPAL ===\n");
            printf("1. Ver relatórios\n");
            printf("2. Configurações\n");
            printf("3. Sair\n");
            printf("Escolha: ");
            scanf("%d", &opcao);
            
            switch (opcao) {
                case 1:
                    printf("Gerando relatórios...\n");
                    break;
                case 2:
                    printf("Abrindo configurações...\n");
                    break;
                case 3:
                    printf("Saindo...\n");
                    break;
                default:
                    printf("Opção inválida!\n");
            }
            
            break;
        } else {
            printf("Credenciais inválidas! ❌\n");
            tentativas--;
            
            if (tentativas == 0) {
                printf("Conta bloqueada! 🔒\n");
            }
        }
    }
    
    return 0;
}
```

### Calculadora Completa

```c
#include <stdio.h>

int main() {
    float num1, num2, resultado;
    char operador;
    
    printf("=== CALCULADORA ===\n");
    printf("Digite a expressão (ex: 5 + 3): ");
    scanf("%f %c %f", &num1, &operador, &num2);
    
    switch (operador) {
        case '+':
            resultado = num1 + num2;
            printf("Resultado: %.2f\n", resultado);
            break;
            
        case '-':
            resultado = num1 - num2;
            printf("Resultado: %.2f\n", resultado);
            break;
            
        case '*':
        case 'x':
            resultado = num1 * num2;
            printf("Resultado: %.2f\n", resultado);
            break;
            
        case '/':
            if (num2 != 0) {
                resultado = num1 / num2;
                printf("Resultado: %.2f\n", resultado);
            } else {
                printf("Erro: Divisão por zero! ❌\n");
            }
            break;
            
        case '%':
            if ((int)num2 != 0) {
                resultado = (int)num1 % (int)num2;
                printf("Resultado: %.0f\n", resultado);
            } else {
                printf("Erro: Divisão por zero! ❌\n");
            }
            break;
            
        default:
            printf("Operador inválido! ❌\n");
            printf("Use: +, -, *, /, %%\n");
    }
    
    return 0;
}
```

---

## 10. Boas Práticas e Dicas

### 1. Use Chaves Sempre
```c
// RUIM - propenso a erro
if (condicao)
    printf("verdadeiro");
    printf("sempre executado");  // Ops! Sempre executado!

// BOM - claro e seguro
if (condicao) {
    printf("verdadeiro");
}
printf("sempre executado");
```

### 2. Evite Condições Complexas
```c
// DIFÍCIL de ler
if ((x > 0 && y < 10) || (z == 5 && !(a < b)) && c != 0)

// MELHOR - divida em variáveis
int cond1 = (x > 0 && y < 10);
int cond2 = (z == 5 && a >= b);
int cond3 = (c != 0);

if ((cond1 || cond2) && cond3)
```

### 3. Use Constantes para Valores Mágicos
```c
// RUIM
if (idade >= 18 && idade <= 65) { ... }

// BOM
#define IDADE_MINIMA 18
#define IDADE_MAXIMA 65

if (idade >= IDADE_MINIMA && idade <= IDADE_MAXIMA) { ... }
```

### 4. Prefira Switch para Múltiplas Opções
```c
// IF-ELSE verboso
if (opcao == 1) { ... }
else if (opcao == 2) { ... }
else if (opcao == 3) { ... }

// SWITCH mais limpo
switch (opcao) {
    case 1: ... break;
    case 2: ... break;
    case 3: ... break;
}
```

---

## 11. Exercícios Práticos

### Exercício 1: Classificador de Triângulos
```c
#include <stdio.h>

int main() {
    float a, b, c;
    
    printf("Digite os 3 lados do triângulo: ");
    scanf("%f %f %f", &a, &b, &c);
    
    if (a + b > c && a + c > b && b + c > a) {
        if (a == b && b == c) {
            printf("Triângulo Equilátero\n");
        } else if (a == b || a == c || b == c) {
            printf("Triângulo Isósceles\n");
        } else {
            printf("Triângulo Escaleno\n");
        }
    } else {
        printf("Não é um triângulo válido!\n");
    }
    
    return 0;
}
```

### Exercício 2: Verificador de Ano Bissexto
```c
#include <stdio.h>

int main() {
    int ano;
    
    printf("Digite um ano: ");
    scanf("%d", &ano);
    
    if ((ano % 4 == 0 && ano % 100 != 0) || (ano % 400 == 0)) {
        printf("%d é bissexto! ✅\n", ano);
    } else {
        printf("%d não é bissexto! ❌\n", ano);
    }
    
    return 0;
}
```

---

## 12. Tabela Resumo

| Estrutura | Uso | Exemplo |
|-----------|-----|---------|
| `if` | Condição simples | `if (idade >= 18)` |
| `if-else` | Duas alternativas | `if (nota >= 6) else` |
| `else-if` | Múltiplas condições | `if (x>0) else if (x<0) else` |
| `switch` | Opções discretas | `switch (opcao) { case 1: ... }` |
| `ternário` | IF-ELSE simples | `maior = (a>b) ? a : b` |

**Operadores Importantes:**
- **Comparação:** `==, !=, >, <, >=, <=`
- **Lógicos:** `&&, ||, !`
- **Ternário:** `? :`

As estruturas condicionais são o coração da lógica de programação em C! Domine-as para criar programas inteligentes e responsivos. 🎯