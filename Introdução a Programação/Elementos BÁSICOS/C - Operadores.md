
> [!note] O que são Operadores?
> Operadores são símbolos que realizam operações sobre variáveis e valores. Pense neles como "verbos" na linguagem da programação - eles fazem coisas com os dados!

## Operadores Aritméticos

> [!example] Matemática Básica
> 
> ```c
> #include <stdio.h>
> 
> int main() {
>     int a = 15, b = 4;
>     
>     printf("=== OPERADORES ARITMÉTICOS ===\n");
>     printf("a = %d, b = %d\n\n", a, b);
>     
>     printf("Adição:        %d + %d = %d\n", a, b, a + b);
>     printf("Subtração:     %d - %d = %d\n", a, b, a - b);
>     printf("Multiplicação: %d * %d = %d\n", a, b, a * b);
>     printf("Divisão:       %d / %d = %d\n", a, b, a / b);
>     printf("Resto:         %d %% %d = %d\n", a, b, a % b);
>     
>     // Incremento e Decremento
>     int x = 5;
>     printf("\nIncremento:\n");
>     printf("x = %d\n", x);
>     x++; // Incrementa x em 1
>     printf("x++ = %d\n", x);
>     x--; // Decrementa x em 1
>     printf("x-- = %d\n", x);
>     
>     return 0;
> }
> ```
> 
> **Saída:**
> ```
> === OPERADORES ARITMÉTICOS ===
> a = 15, b = 4
> 
> Adição:        15 + 4 = 19
> Subtração:     15 - 4 = 11
> Multiplicação: 15 * 4 = 60
> Divisão:       15 / 4 = 3
> Resto:         15 % 4 = 3
> 
> Incremento:
> x = 5
> x++ = 6
> x-- = 5
> ```

### Diferença entre Pré e Pós Incremento

> [!tip] ++x vs x++
> 
> ```c
> #include <stdio.h>
> 
> int main() {
>     int x = 5, y = 5;
>     int a, b;
>     
>     printf("=== PRÉ vs PÓS INCREMENTO ===\n");
>     
>     // Pós-incremento: usa o valor, depois incrementa
>     a = x++;
>     printf("a = x++ → a = %d, x = %d\n", a, x);
>     
>     // Pré-incremento: incrementa primeiro, depois usa o valor
>     b = ++y;
>     printf("b = ++y → b = %d, y = %d\n", b, y);
>     
>     printf("\nExemplo prático:\n");
>     int i = 3;
>     printf("i = %d\n", i);
>     printf("i++ = %d (e agora i vale %d)\n", i++, i);
>     printf("++i = %d (e agora i vale %d)\n", ++i, i);
>     
>     return 0;
> }
> ```
> 
> **Saída:**
> ```
> === PRÉ vs PÓS INCREMENTO ===
> a = x++ → a = 5, x = 6
> b = ++y → b = 6, y = 6
> 
> Exemplo prático:
> i = 3
> i++ = 3 (e agora i vale 4)
> ++i = 5 (e agora i vale 5)
> ```

## Operadores Relacionais

> [!info] Comparando Valores
> 
> ```c
> #include <stdio.h>
> 
> int main() {
>     int a = 10, b = 5, c = 10;
>     
>     printf("=== OPERADORES RELACIONAIS ===\n");
>     printf("a = %d, b = %d, c = %d\n\n", a, b, c);
>     
>     printf("Igualdade:        %d == %d → %d\n", a, c, a == c);
>     printf("Diferença:        %d != %d → %d\n", a, b, a != b);
>     printf("Maior que:        %d > %d  → %d\n", a, b, a > b);
>     printf("Menor que:        %d < %d  → %d\n", b, a, b < a);
>     printf("Maior ou igual:   %d >= %d → %d\n", a, c, a >= c);
>     printf("Menor ou igual:   %d <= %d → %d\n", b, a, b <= a);
>     
>     return 0;
> }
> ```
> 
> **Saída:**
> ```
> === OPERADORES RELACIONAIS ===
> a = 10, b = 5, c = 10
> 
> Igualdade:        10 == 10 → 1
> Diferença:        10 != 5 → 1
> Maior que:        10 > 5  → 1
> Menor que:        5 < 10  → 1
> Maior ou igual:   10 >= 10 → 1
> Menor ou igual:   5 <= 10 → 1
> ```
> 
> **Lembrete:** Em C, `1` significa Verdadeiro e `0` significa Falso

## Operadores Lógicos

> [!abstract] Combinando Condições
> 
> ```c
> #include <stdio.h>
> #include <stdbool.h>
> 
> int main() {
>     bool tem_idade = true;    // 18 anos ou mais
>     bool tem_dinheiro = false; // Tem dinheiro suficiente
>     bool tem_convite = true;   // Tem convite especial
>     
>     printf("=== OPERADORES LÓGICOS ===\n");
>     printf("Tem idade: %d, Tem dinheiro: %d, Tem convite: %d\n\n", 
>            tem_idade, tem_dinheiro, tem_convite);
>     
>     printf("E (AND): idade E dinheiro → %d && %d = %d\n", 
>            tem_idade, tem_dinheiro, tem_idade && tem_dinheiro);
>     
>     printf("OU (OR):  idade OU convite → %d || %d = %d\n", 
>            tem_idade, tem_convite, tem_idade || tem_convite);
>     
>     printf("NÃO (NOT): NÃO tem dinheiro → !%d = %d\n", 
>            tem_dinheiro, !tem_dinheiro);
>     
>     // Exemplo prático: pode entrar na festa?
>     bool pode_entrar = (tem_idade && tem_dinheiro) || tem_convite;
>     printf("\nPode entrar na festa? %s\n", 
>            pode_entrar ? "SIM! 🎉" : "NÃO! 😢");
>     
>     return 0;
> }
> ```
> 
> **Saída:**
> ```
> === OPERADORES LÓGICOS ===
> Tem idade: 1, Tem dinheiro: 0, Tem convite: 1
> 
> E (AND): idade E dinheiro → 1 && 0 = 0
> OU (OR):  idade OU convite → 1 || 1 = 1
> NÃO (NOT): NÃO tem dinheiro → !0 = 1
> 
> Pode entrar na festa? SIM! 🎉
> ```

### Tabela Verdade dos Operadores Lógicos

> [!code] Demonstração Completa
> 
> ```c
> #include <stdio.h>
> 
> int main() {
>     printf("=== TABELA VERDADE ===\n");
>     printf("A\tB\tA && B\tA || B\t!A\n");
>     printf("--------------------------------\n");
>     
>     // Todas as combinações possíveis
>     int A, B;
>     
>     A = 0; B = 0;
>     printf("%d\t%d\t%d\t%d\t%d\n", A, B, A && B, A || B, !A);
>     
>     A = 0; B = 1;
>     printf("%d\t%d\t%d\t%d\t%d\n", A, B, A && B, A || B, !A);
>     
>     A = 1; B = 0;
>     printf("%d\t%d\t%d\t%d\t%d\n", A, B, A && B, A || B, !A);
>     
>     A = 1; B = 1;
>     printf("%d\t%d\t%d\t%d\t%d\n", A, B, A && B, A || B, !A);
>     
>     return 0;
> }
> ```
> 
> **Saída:**
> ```
> === TABELA VERDADE ===
> A       B       A && B  A || B  !A
> --------------------------------
> 0       0       0       0       1
> 0       1       0       1       1
> 1       0       0       1       0
> 1       1       1       1       0
> ```

## Operadores de Atribuição

> [!important] Modificando Variáveis
> 
> ```c
> #include <stdio.h>
> 
> int main() {
>     int x = 10;
>     
>     printf("=== OPERADORES DE ATRIBUIÇÃO ===\n");
>     printf("Valor inicial: x = %d\n\n", x);
>     
>     x = 5;  // Atribuição simples
>     printf("x = 5  → x = %d\n", x);
>     
>     x += 3; // Equivale a: x = x + 3
>     printf("x += 3 → x = %d\n", x);
>     
>     x -= 2; // Equivale a: x = x - 2
>     printf("x -= 2 → x = %d\n", x);
>     
>     x *= 4; // Equivale a: x = x * 4
>     printf("x *= 4 → x = %d\n", x);
>     
>     x /= 2; // Equivale a: x = x / 2
>     printf("x /= 2 → x = %d\n", x);
>     
>     x %= 3; // Equivale a: x = x % 3
>     printf("x %%= 3 → x = %d\n", x);
>     
>     return 0;
> }
> ```
> 
> **Saída:**
> ```
> === OPERADORES DE ATRIBUIÇÃO ===
> Valor inicial: x = 10
> 
> x = 5  → x = 5
> x += 3 → x = 8
> x -= 2 → x = 6
> x *= 4 → x = 24
> x /= 2 → x = 12
> x %= 3 → x = 0
> ```

## Operador Ternário

> [!tip] If-Else em Uma Linha
> 
> ```c
> #include <stdio.h>
> 
> int main() {
>     int idade = 17;
>     
>     printf("=== OPERADOR TERNÁRIO ===\n");
>     printf("Idade: %d anos\n\n", idade);
>     
>     // Sintaxe: condição ? valor_se_verdadeiro : valor_se_falso
>     char *situacao = (idade >= 18) ? "maior de idade" : "menor de idade";
>     printf("Situação: %s\n", situacao);
>     
>     // Exemplo com números
>     int a = 10, b = 20;
>     int maior = (a > b) ? a : b;
>     printf("Maior entre %d e %d: %d\n", a, b, maior);
>     
>     // Exemplo com cálculo
>     int nota = 85;
>     char *resultado = (nota >= 60) ? "Aprovado" : "Reprovado";
>     printf("Nota: %d → %s\n", nota, resultado);
>     
>     return 0;
> }
> ```
> 
> **Saída:**
> ```
> === OPERADOR TERNÁRIO ===
> Idade: 17 anos
> 
> Situação: menor de idade
> Maior entre 10 e 20: 20
> Nota: 85 → Aprovado
> ```

## Operadores Bit a Bit (Bitwise)

> [!caution] Trabalhando com Bits
> 
> ```c
> #include <stdio.h>
> 
> void print_binary(int num) {
>     for(int i = 7; i >= 0; i--) {
>         printf("%d", (num >> i) & 1);
>     }
> }
> 
> int main() {
>     unsigned char a = 0b10101010; // 170 em decimal
>     unsigned char b = 0b11001100; // 204 em decimal
>     
>     printf("=== OPERADORES BIT A BIT ===\n");
>     printf("a = "); print_binary(a); printf(" (%d)\n", a);
>     printf("b = "); print_binary(b); printf(" (%d)\n\n", b);
>     
>     printf("AND:   a & b  = "); print_binary(a & b); printf("\n");
>     printf("OR:    a | b  = "); print_binary(a | b); printf("\n");
>     printf("XOR:   a ^ b  = "); print_binary(a ^ b); printf("\n");
>     printf("NOT:   ~a     = "); print_binary(~a); printf("\n");
>     printf("Shift left:  a << 1 = "); print_binary(a << 1); printf("\n");
>     printf("Shift right: a >> 1 = "); print_binary(a >> 1); printf("\n");
>     
>     return 0;
> }
> ```
> 
> **Saída:**
> ```
> === OPERADORES BIT A BIT ===
> a = 10101010 (170)
> b = 11001100 (204)
> 
> AND:   a & b  = 10001000
> OR:    a | b  = 11101110
> XOR:   a ^ b  = 01100110
> NOT:   ~a     = 01010101
> Shift left:  a << 1 = 01010100
> Shift right: a >> 1 = 01010101
> ```

## Precedência de Operadores

> [!warning] Ordem das Operações
> 
> ```c
> #include <stdio.h>
> 
> int main() {
>     int a = 10, b = 5, c = 2;
>     
>     printf("=== PRECEDÊNCIA DE OPERADORES ===\n");
>     printf("a = %d, b = %d, c = %d\n\n", a, b, c);
>     
>     int resultado1 = a + b * c;      // Multiplicação primeiro
>     int resultado2 = (a + b) * c;    // Parênteses muda a ordem
>     
>     printf("a + b * c     = %d + %d * %d = %d\n", a, b, c, resultado1);
>     printf("(a + b) * c   = (%d + %d) * %d = %d\n", a, b, c, resultado2);
>     
>     // Exemplo mais complexo
>     int x = 8, y = 3, z = 2;
>     int complexo = x / y + z * 2 - 1;
>     int com_parenteses = (x / y) + (z * 2) - 1;
>     
>     printf("\nExpressão complexa:\n");
>     printf("x / y + z * 2 - 1 = %d\n", complexo);
>     printf("(x / y) + (z * 2) - 1 = %d\n", com_parenteses);
>     
>     return 0;
> }
> ```
> 
> **Saída:**
> ```
> === PRECEDÊNCIA DE OPERADORES ===
> a = 10, b = 5, c = 2
> 
> a + b * c     = 10 + 5 * 2 = 20
> (a + b) * c   = (10 + 5) * 2 = 30
> 
> Expressão complexa:
> x / y + z * 2 - 1 = 7
> (x / y) + (z * 2) - 1 = 7
> ```

## Exemplo Prático: Calculadora

> [!success] Aplicando Todos os Conceitos
> 
> ```c
> #include <stdio.h>
> 
> int main() {
>     int num1, num2;
>     char operacao;
>     
>     printf("=== CALCULADORA SIMPLES ===\n");
>     printf("Digite o primeiro número: ");
>     scanf("%d", &num1);
>     
>     printf("Digite a operação (+, -, *, /, %%): ");
>     scanf(" %c", &operacao);
>     
>     printf("Digite o segundo número: ");
>     scanf("%d", &num2);
>     
>     int resultado;
>     char *descricao;
>     
>     switch(operacao) {
>         case '+':
>             resultado = num1 + num2;
>             descricao = "Soma";
>             break;
>         case '-':
>             resultado = num1 - num2;
>             descricao = "Subtração";
>             break;
>         case '*':
>             resultado = num1 * num2;
>             descricao = "Multiplicação";
>             break;
>         case '/':
// ... (código anterior continua)
>             if(num2 != 0) {
>                 resultado = num1 / num2;
>                 descricao = "Divisão";
>             } else {
>                 printf("Erro: Divisão por zero!\n");
>                 return 1;
>             }
>             break;
>         case '%':
>             if(num2 != 0) {
>                 resultado = num1 % num2;
>                 descricao = "Resto da divisão";
>             } else {
>                 printf("Erro: Divisão por zero!\n");
>                 return 1;
>             }
>             break;
>         default:
>             printf("Operação inválida!\n");
>             return 1;
>     }
>     
>     printf("\n%s: %d %c %d = %d\n", descricao, num1, operacao, num2, resultado);
>     
>     // Verificações adicionais usando operadores relacionais e lógicos
>     if(resultado > 100) {
>         printf("Resultado é maior que 100! 🎉\n");
>     } else if(resultado < 0) {
>         printf("Resultado é negativo! 📉\n");
>     } else {
>         printf("Resultado está entre 0 e 100 📊\n");
>     }
>     
>     // Usando operador ternário para verificar se é par ou ímpar
>     printf("O resultado é %s\n", (resultado % 2 == 0) ? "PAR" : "ÍMPAR");
>     
>     return 0;
> }
> ```

> [!note] Resumo dos Operadores
> 
> | Categoria | Operadores | Exemplo | Descrição |
> |-----------|------------|---------|-----------|
> | Aritméticos | `+ - * / % ++ --` | `a + b` | Operações matemáticas |
> | Relacionais | `== != > < >= <=` | `a == b` | Comparações |
> | Lógicos | `&& \|\| !` | `a && b` | Operações booleanas |
> | Atribuição | `= += -= *= /= %=` | `a += 5` | Modificar variáveis |
> | Bit a Bit | `& \| ^ ~ << >>` | `a & b` | Manipulação de bits |
> | Ternário | `? :` | `a > b ? a : b` | If-else compacto |
> 
> **Dica Importante:** Use parênteses para deixar claro a ordem das operações!

**Próximo**: [[C - Precedência de Operadores]]

