# C - Precedência de Operadores

> [!question] O que é Precedência?
> Imagine que você está lendo uma receita de bolo. Você não mistura todos os ingredientes de uma vez, certo? Segue uma ordem! Na programação é a mesma coisa: a **precedência** determina qual operação o computador executa primeiro.

## O Básico: Ordem Natural

> [!example] Matemática do Dia a Dia
> 
> ```c
> #include <stdio.h>
> 
> int main() {
>     int resultado = 2 + 3 * 4;
>     
>     printf("Quanto é 2 + 3 * 4?\n");
>     printf("Pensamento humano: %d\n", (2 + 3) * 4);   // 20
>     printf("Pensamento do C:   %d\n", resultado);      // 14
>     
>     printf("\nPor quê?\n");
>     printf("O C segue a regra matemática: multiplicação antes da adição!\n");
>     printf("3 * 4 = 12, depois 2 + 12 = 14\n");
>     
>     return 0;
> }
> ```
> 
> **Saída:**
> ```
> Quanto é 2 + 3 * 4?
> Pensamento humano: 20
> Pensamento do C:   14
> 
> Por quê?
> O C segue a regra matemática: multiplicação antes da adição!
> 3 * 4 = 12, depois 2 + 12 = 14
> ```

## Os "Grupos" de Precedência

> [!summary] Famílias de Operadores
> 
> ```c
> #include <stdio.h>
> 
> int main() {
>     int a = 10, b = 5, c = 2;
>     
>     printf("=== DEMONSTRAÇÃO DOS GRUPOS ===\n\n");
>     
>     // Grupo 1: Multiplicação/Divisão vs Adição/Subtração
>     printf("1. Multiplicação/Divisão VEM PRIMEIRO:\n");
>     printf("   %d + %d * %d = %d\n", a, b, c, a + b * c);
>     printf("   (%d + %d) * %d = %d\n\n", a, b, c, (a + b) * c);
>     
>     // Grupo 2: Relacionais vs Lógicos
>     printf("2. Comparações VEM ANTES dos E/OU:\n");
>     int x = 5, y = 10, z = 15;
>     int teste = x < y && y < z;
>     printf("   %d < %d && %d < %d = %d\n", x, y, y, z, teste);
>     printf("   (Primeiro faz as comparações, depois o AND)\n\n");
>     
>     // Grupo 3: Atribuição é quase sempre o ÚLTIMO
>     printf("3. Atribuição é o ÚLTIMO da festa:\n");
>     int valor = a + b * c;
>     printf("   int valor = %d + %d * %d;\n", a, b, c);
>     printf("   Primeiro calcula %d * %d, depois soma, depois atribui\n", b, c);
>     
>     return 0;
> }
> ```

## A Regra de Ouro: Parênteses

> [!tip] Quando Estiver em Dúvida, Use Parênteses!
> 
> ```c
> #include <stdio.h>
> 
> int main() {
>     int a = 8, b = 4, c = 2, d = 1;
>     
>     printf("=== PARÊNTESES SALVAM VIDAS ===\n\n");
>     
>     // Expressão confusa sem parênteses
>     int confuso = a + b * c - d / 2 + 3;
>     
>     // Mesma expressão com parênteses explícitos
>     int claro = a + (b * c) - (d / 2) + 3;
>     
>     printf("Expressão CONFUSA:\n");
>     printf("a + b * c - d / 2 + 3 = %d\n\n", confuso);
>     
>     printf("Expressão CLARA:\n");
>     printf("a + (b * c) - (d / 2) + 3 = %d\n\n", claro);
>     
>     printf("São iguais? %s\n", (confuso == claro) ? "SIM! ✅" : "NÃO! ❌");
>     
>     printf("\nPasso a passo:\n");
>     printf("1. b * c = %d * %d = %d\n", b, c, b * c);
>     printf("2. d / 2 = %d / %d = %d\n", d, 2, d / 2);
>     printf("3. a + %d = %d + %d = %d\n", b * c, a, b * c, a + b * c);
>     printf("4. %d - %d = %d\n", a + b * c, d / 2, (a + b * c) - (d / 2));
>     printf("5. %d + 3 = %d\n", (a + b * c) - (d / 2), confuso);
>     
>     return 0;
> }
> ```

## Casos Especiais que Confundem

> [!warning] Pegadinhas Comuns
> 
> ```c
> #include <stdio.h>
> 
> int main() {
>     printf("=== CUIDADO COM ESSES! ===\n\n");
>     
>     // Caso 1: Operadores de comparação vs bit a bit
>     int a = 1, b = 2, c = 3;
>     int resultado1 = a & b == c;      // 😱 Perigoso!
>     int resultado2 = (a & b) == c;    // ✅ Seguro!
>     
>     printf("1. Bitwise vs Comparação:\n");
>     printf("   a & b == c   → %d (FEITO: b == c PRIMEIRO!)\n", resultado1);
>     printf("   (a & b) == c → %d (FEITO: a & b PRIMEIRO!)\n\n", resultado2);
>     
>     // Caso 2: Operadores lógicos vs atribuição
>     int x = 5, y = 10;
>     int teste = x > 0 && y < 20;      // ✅ Correto
>     int perigoso = x = 5 && y < 20;   // 😱 Muito perigoso!
>     
>     printf("2. Lógicos vs Atribuição:\n");
>     printf("   x > 0 && y < 20 → %d\n", teste);
>     printf("   x = 5 && y < 20 → %d (ATRIBUI x = 0/1!)\n\n", perigoso);
>     
>     return 0;
> }
> ```

## Tabela de Precedência Simplificada

> [!abstract] Do Mais "Forte" ao Mais "Fraco"
> 
> ```c
> #include <stdio.h>
> 
> int main() {
>     printf("=== RANKING DOS OPERADORES ===\n\n");
>     
>     printf("🏆 CAMPEÕES (executam primeiro):\n");
>     printf("   () [] -> .     (parênteses, colchetes, acesso)\n");
>     printf("   ! ~ ++ --      (NOT, incremento, decremento)\n");
>     printf("   * / %%          (multiplicação, divisão, resto)\n\n");
>     
>     printf("🥈 VICE-CAMPEÕES:\n");
>     printf("   + -            (adição, subtração)\n");
>     printf("   < <= > >=      (comparações)\n");
>     printf("   == !=          (igualdade, diferença)\n\n");
>     
>     printf("🥉 TERCEIRO LUGAR:\n");
>     printf("   && ||          (E lógico, OU lógico)\n\n");
>     
>     printf("🎯 ÚLTIMOS COLOCADOS:\n");
>     printf("   = += -= etc.   (atribuições)\n");
>     
>     return 0;
> }
> ```

## Exemplo Prático: Calculadora Inteligente

> [!success] Aplicando na Vida Real
> 
> ```c
> #include <stdio.h>
> 
> int main() {
>     printf("=== CALCULADORA DE IMC ===\n\n");
>     
>     float peso = 70.5;
>     float altura = 1.75;
>     
>     // ❌ JEITO PERIGOSO
>     float imc_ruim = peso / altura * altura;  // ERRADO!
>     
>     // ✅ JEITO CORRETO
>     float imc_bom = peso / (altura * altura); // CERTO!
>     
>     printf("Peso: %.1f kg, Altura: %.2f m\n\n", peso, altura);
>     
>     printf("Cálculo ERRADO:\n");
>     printf("peso / altura * altura = %.1f / %.2f * %.2f\n", peso, altura, altura);
>     printf("Primeiro: %.1f / %.2f = %.2f\n", peso, altura, peso/altura);
>     printf("Depois: %.2f * %.2f = %.2f\n", peso/altura, altura, imc_ruim);
>     printf("IMC calculado errado: %.1f\n\n", imc_ruim);
>     
>     printf("Cálculo CORRETO:\n");
>     printf("peso / (altura * altura) = %.1f / (%.2f * %.2f)\n", peso, altura, altura);
>     printf("Primeiro: %.2f * %.2f = %.2f\n", altura, altura, altura*altura);
>     printf("Depois: %.1f / %.2f = %.1f\n", peso, altura*altura, imc_bom);
>     printf("IMC calculado certo: %.1f\n", imc_bom);
>     
>     return 0;
> }
> ```

## Dica Final: Teste seus Conhecimentos

> [!example] Desafio Mental
> 
> ```c
> #include <stdio.h>
> 
> int main() {
>     printf("=== TESTE SEU CONHECIMENTO ===\n\n");
>     
>     int a = 5, b = 3, c = 2;
>     
>     printf("Qual o resultado de cada expressão?\n\n");
>     
>     printf("1. a + b * c = ?\n");
>     printf("2. (a + b) * c = ?\n");
>     printf("3. a * b / c = ?\n");
>     printf("4. a * (b / c) = ?\n");
>     printf("5. a > b && b < c = ?\n");
>     printf("6. a == b || b != c = ?\n\n");
>     
>     printf("Pense primeiro, depois rode o programa!\n");
>     
>     return 0;
> }
> ```

> [!note] Resumo da Precedência
> 
> **Regra Básica:** 
> - **Parênteses** sempre primeiro
> - **Multiplicação/Divisão** antes de Adição/Subtração  
> - **Comparações** antes de Operadores Lógicos
> - **Atribuição** quase sempre por último
> 
> **Conselho Sábio:**
> ```c
> // ❌ Díficil de ler
> resultado = a + b * c - d / e + f;
> 
> // ✅ Fácil de entender
> resultado = a + (b * c) - (d / e) + f;
> ```
> 
> **Quando não tiver certeza, use parênteses!** É melhor ser explícito do que ter bugs misteriosos no código.

**Próximos**: [[C - Funções printf e scanf]]