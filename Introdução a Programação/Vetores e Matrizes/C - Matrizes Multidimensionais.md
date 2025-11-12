# Matrizes em C - Guio Completo

> [!note] ## O que são Matrizes?
> 
> Matrizes são **arrays multidimensionais** - basicamente, uma "tabela" de dados com linhas e colunas. Enquanto um array normal é uma lista (1D), uma matriz é uma grade (2D ou mais).
> 
> **Analogia:** Pense em uma planilha Excel ou uma tabela onde cada célula pode armazenar um valor.
> 
> ```c
> // Array unidimensional (vetor)
> int lista[5] = {1, 2, 3, 4, 5};
> 
> // Array bidimensional (matriz)
> int tabela[3][3] = {{1, 2, 3},
>                     {4, 5, 6},
>                     {7, 8, 9}};
> ```

---

## 1. Declaração e Inicialização de Matrizes

### Sintaxe Básica

```c
tipo nome_matriz[linhas][colunas];
```

**Explicação:** A primeira dimensão representa as **linhas** e a segunda as **colunas**. Matrizes são armazenadas na memória em **ordem de linhas** (row-major).

### Formas de Inicialização

```c
#include <stdio.h>

int main() {
    // Método 1: Inicialização direta
    int matriz1[2][3] = {{1, 2, 3}, 
                         {4, 5, 6}};
    
    // Método 2: Inicialização parcial (restante preenchido com 0)
    int matriz2[3][3] = {{1}, 
                         {4, 5}, 
                         {7, 8, 9}};
    
    // Método 3: Inicialização linear (C organiza por linhas)
    int matriz3[2][3] = {1, 2, 3, 4, 5, 6};
    
    // Método 4: Declaração sem tamanho (compiler infere)
    int matriz4[][3] = {{1, 2, 3}, 
                        {4, 5, 6}};  // 2 linhas inferidas
    
    // Método 5: Declaração sem inicialização
    int matriz5[2][3];  // Lixo de memória - inicialize depois!
    
    return 0;
}
```

### Exemplo Prático: Matriz de Notas

```c
#include <stdio.h>

int main() {
    // Matriz 3x4: 3 alunos, 4 notas cada
    float notas[3][4] = {
        {7.5, 8.0, 6.5, 9.0},  // Aluno 1
        {5.5, 6.0, 7.5, 8.0},  // Aluno 2  
        {8.5, 9.0, 9.5, 10.0}  // Aluno 3
    };
    
    printf("Matriz de notas dos alunos:\n");
    for (int i = 0; i < 3; i++) {
        printf("Aluno %d: ", i + 1);
        for (int j = 0; j < 4; j++) {
            printf("%.1f ", notas[i][j]);
        }
        printf("\n");
    }
    
    return 0;
}
```
**Saída:**
```
Matriz de notas dos alunos:
Aluno 1: 7.5 8.0 6.5 9.0 
Aluno 2: 5.5 6.0 7.5 8.0 
Aluno 3: 8.5 9.0 9.5 10.0 
```

---

## 2. Acesso e Modificação de Elementos

### Sintaxe de Acesso

```c
matriz[linha][coluna] = valor;      // Escrita
variavel = matriz[linha][coluna];   // Leitura
```

**Exemplo Completo:**
```c
#include <stdio.h>

int main() {
    int matriz[2][3];
    
    // Preenchendo a matriz
    printf("Preencha a matriz 2x3:\n");
    for (int i = 0; i < 2; i++) {
        for (int j = 0; j < 3; j++) {
            printf("Elemento [%d][%d]: ", i, j);
            scanf("%d", &matriz[i][j]);
        }
    }
    
    // Exibindo a matriz
    printf("\nMatriz digitada:\n");
    for (int i = 0; i < 2; i++) {
        for (int j = 0; j < 3; j++) {
            printf("%d\t", matriz[i][j]);
        }
        printf("\n");
    }
    
    // Acessando elementos específicos
    printf("\nElementos específicos:\n");
    printf("Canto superior esquerdo: %d\n", matriz[0][0]);
    printf("Canto inferior direito: %d\n", matriz[1][2]);
    printf("Elemento do meio: %d\n", matriz[0][1]);
    
    // Modificando elementos
    matriz[1][1] = 999;
    printf("\nApós modificar [1][1] para 999:\n");
    for (int i = 0; i < 2; i++) {
        for (int j = 0; j < 3; j++) {
            printf("%d\t", matriz[i][j]);
        }
        printf("\n");
    }
    
    return 0;
}
```

### Exemplo: Tabuleiro de Jogo da Velha

```c
#include <stdio.h>

int main() {
    char tabuleiro[3][3] = {
        {' ', ' ', ' '},
        {' ', ' ', ' '},
        {' ', ' ', ' '}
    };
    
    // Fazendo algumas jogadas
    tabuleiro[0][0] = 'X';
    tabuleiro[1][1] = 'O';
    tabuleiro[2][2] = 'X';
    tabuleiro[0][2] = 'O';
    
    // Exibindo o tabuleiro
    printf("Jogo da Velha:\n");
    printf("  0   1   2\n");
    for (int i = 0; i < 3; i++) {
        printf("%d ", i);
        for (int j = 0; j < 3; j++) {
            printf(" %c ", tabuleiro[i][j]);
            if (j < 2) printf("|");
        }
        printf("\n");
        if (i < 2) printf("  -----------\n");
    }
    
    return 0;
}
```
**Saída:**
```
Jogo da Velha:
  0   1   2
0  X |   | O 
  -----------
1    | O |   
  -----------
2    |   | X 
```

---

## 3. Percorrendo Matrizes com Loops Aninhados

### Padrão Fundamental

Para trabalhar com matrizes, quase sempre usamos **loops aninhados**: um loop externo para linhas e um loop interno para colunas.

```c
#include <stdio.h>

#define LINHAS 3
#define COLUNAS 3

int main() {
    int matriz[LINHAS][COLUNAS];
    
    // Preenchimento padrão
    printf("Preenchendo matriz:\n");
    for (int i = 0; i < LINHAS; i++) {
        for (int j = 0; j < COLUNAS; j++) {
            matriz[i][j] = (i * COLUNAS) + j + 1;
        }
    }
    
    // Exibição padrão
    printf("Matriz resultante:\n");
    for (int i = 0; i < LINHAS; i++) {
        for (int j = 0; j < COLUNAS; j++) {
            printf("%d\t", matriz[i][j]);
        }
        printf("\n");
    }
    
    return 0;
}
```
**Saída:**
```
Preenchendo matriz:
Matriz resultante:
1	2	3	
4	5	6	
7	8	9	
```

### Exemplo: Sistema de Temperaturas por Cidade

```c
#include <stdio.h>

int main() {
    // Matriz: 4 cidades x 7 dias da semana
    float temperaturas[4][7] = {
        {25.0, 26.5, 24.0, 27.0, 23.5, 26.0, 25.5},  // Cidade A
        {30.0, 31.5, 29.0, 32.0, 28.5, 31.0, 30.5},  // Cidade B
        {18.0, 19.5, 17.0, 20.0, 16.5, 19.0, 18.5},  // Cidade C
        {22.0, 23.5, 21.0, 24.0, 20.5, 23.0, 22.5}   // Cidade D
    };
    
    char *cidades[] = {"São Paulo", "Rio de Janeiro", "Porto Alegre", "Belo Horizonte"};
    char *dias[] = {"Dom", "Seg", "Ter", "Qua", "Qui", "Sex", "Sab"};
    
    printf("=== TEMPERATURAS DA SEMANA ===\n\n");
    
    // Exibir tabela completa
    printf("Cidade/Dia\t");
    for (int j = 0; j < 7; j++) {
        printf("%s\t", dias[j]);
    }
    printf("\n");
    
    for (int i = 0; i < 4; i++) {
        printf("%-15s", cidades[i]);
        for (int j = 0; j < 7; j++) {
            printf("%.1f°C\t", temperaturas[i][j]);
        }
        printf("\n");
    }
    
    // Calcular médias por cidade
    printf("\n=== MÉDIAS POR CIDADE ===\n");
    for (int i = 0; i < 4; i++) {
        float soma = 0;
        for (int j = 0; j < 7; j++) {
            soma += temperaturas[i][j];
        }
        float media = soma / 7;
        printf("%s: %.2f°C\n", cidades[i], media);
    }
    
    return 0;
}
```

---

## 4. Operações com Matrizes

### Soma de Matrizes

```c
#include <stdio.h>

#define LINHAS 2
#define COLUNAS 3

int main() {
    int A[LINHAS][COLUNAS] = {{1, 2, 3},
                             {4, 5, 6}};
                             
    int B[LINHAS][COLUNAS] = {{6, 5, 4},
                             {3, 2, 1}};
                             
    int C[LINHAS][COLUNAS];  // Matriz resultado
    
    // Soma: C = A + B
    printf("Soma das matrizes A + B:\n");
    for (int i = 0; i < LINHAS; i++) {
        for (int j = 0; j < COLUNAS; j++) {
            C[i][j] = A[i][j] + B[i][j];
            printf("%d\t", C[i][j]);
        }
        printf("\n");
    }
    
    return 0;
}
```
**Saída:**
```
Soma das matrizes A + B:
7	7	7	
7	7	7	
```

### Multiplicação por Escalar

```c
#include <stdio.h>

#define LINHAS 2
#define COLUNAS 2

int main() {
    int matriz[LINHAS][COLUNAS] = {{1, 2},
                                  {3, 4}};
    int escalar = 3;
    int resultado[LINHAS][COLUNAS];
    
    printf("Matriz original:\n");
    for (int i = 0; i < LINHAS; i++) {
        for (int j = 0; j < COLUNAS; j++) {
            printf("%d\t", matriz[i][j]);
        }
        printf("\n");
    }
    
    printf("\nMultiplicação por %d:\n", escalar);
    for (int i = 0; i < LINHAS; i++) {
        for (int j = 0; j < COLUNAS; j++) {
            resultado[i][j] = matriz[i][j] * escalar;
            printf("%d\t", resultado[i][j]);
        }
        printf("\n");
    }
    
    return 0;
}
```

### Transposição de Matriz

```c
#include <stdio.h>

#define LINHAS 3
#define COLUNAS 2

int main() {
    int original[LINHAS][COLUNAS] = {{1, 2},
                                    {3, 4},
                                    {5, 6}};
                                    
    int transposta[COLUNAS][LINHAS];
    
    printf("Matriz original (%dx%d):\n", LINHAS, COLUNAS);
    for (int i = 0; i < LINHAS; i++) {
        for (int j = 0; j < COLUNAS; j++) {
            printf("%d\t", original[i][j]);
        }
        printf("\n");
    }
    
    // Transpondo: troca linhas por colunas
    for (int i = 0; i < LINHAS; i++) {
        for (int j = 0; j < COLUNAS; j++) {
            transposta[j][i] = original[i][j];
        }
    }
    
    printf("\nMatriz transposta (%dx%d):\n", COLUNAS, LINHAS);
    for (int i = 0; i < COLUNAS; i++) {
        for (int j = 0; j < LINHAS; j++) {
            printf("%d\t", transposta[i][j]);
        }
        printf("\n");
    }
    
    return 0;
}
```
**Saída:**
```
Matriz original (3x2):
1	2	
3	4	
5	6	

Matriz transposta (2x3):
1	3	5	
2	4	6	
```

---

## 5. Matrizes como Parâmetros de Função

### Passando Matrizes para Funções

Quando passamos matrizes para funções, precisamos especificar pelo menos a segunda dimensão (número de colunas).

```c
#include <stdio.h>

// Protótipos das funções
void exibir_matriz(int mat[][3], int linhas);
void preencher_matriz(int mat[][3], int linhas);
int somar_matriz(int mat[][3], int linhas);

int main() {
    int matriz[2][3];
    
    preencher_matriz(matriz, 2);
    exibir_matriz(matriz, 2);
    
    int soma = somar_matriz(matriz, 2);
    printf("Soma de todos os elementos: %d\n", soma);
    
    return 0;
}

// Função para exibir matriz
void exibir_matriz(int mat[][3], int linhas) {
    printf("Matriz %dx%d:\n", linhas, 3);
    for (int i = 0; i < linhas; i++) {
        for (int j = 0; j < 3; j++) {
            printf("%d\t", mat[i][j]);
        }
        printf("\n");
    }
}

// Função para preencher matriz
void preencher_matriz(int mat[][3], int linhas) {
    printf("Preencha a matriz %dx%d:\n", linhas, 3);
    for (int i = 0; i < linhas; i++) {
        for (int j = 0; j < 3; j++) {
            printf("Elemento [%d][%d]: ", i, j);
            scanf("%d", &mat[i][j]);
        }
    }
}

// Função para somar todos os elementos
int somar_matriz(int mat[][3], int linhas) {
    int soma = 0;
    for (int i = 0; i < linhas; i++) {
        for (int j = 0; j < 3; j++) {
            soma += mat[i][j];
        }
    }
    return soma;
}
```

### Exemplo: Operações com Matrizes usando Funções

```c
#include <stdio.h>

#define MAX 10

// Protótipos
void ler_matriz(int mat[][MAX], int linhas, int cols);
void exibir_matriz(int mat[][MAX], int linhas, int cols);
void somar_matrizes(int A[][MAX], int B[][MAX], int C[][MAX], int linhas, int cols);
int encontrar_maior(int mat[][MAX], int linhas, int cols);

int main() {
    int linhas, colunas;
    
    printf("Digite o número de linhas e colunas: ");
    scanf("%d %d", &linhas, &colunas);
    
    int A[MAX][MAX], B[MAX][MAX], C[MAX][MAX];
    
    printf("\n=== Matriz A ===\n");
    ler_matriz(A, linhas, colunas);
    
    printf("\n=== Matriz B ===\n");
    ler_matriz(B, linhas, colunas);
    
    printf("\nMatriz A:\n");
    exibir_matriz(A, linhas, colunas);
    
    printf("\nMatriz B:\n");
    exibir_matriz(B, linhas, colunas);
    
    // Soma
    somar_matrizes(A, B, C, linhas, colunas);
    printf("\nSoma A + B:\n");
    exibir_matriz(C, linhas, colunas);
    
    // Maior elemento
    int maior_A = encontrar_maior(A, linhas, colunas);
    int maior_B = encontrar_maior(B, linhas, colunas);
    printf("\nMaior elemento de A: %d\n", maior_A);
    printf("Maior elemento de B: %d\n", maior_B);
    
    return 0;
}

void ler_matriz(int mat[][MAX], int linhas, int cols) {
    for (int i = 0; i < linhas; i++) {
        for (int j = 0; j < cols; j++) {
            printf("Elemento [%d][%d]: ", i, j);
            scanf("%d", &mat[i][j]);
        }
    }
}

void exibir_matriz(int mat[][MAX], int linhas, int cols) {
    for (int i = 0; i < linhas; i++) {
        for (int j = 0; j < cols; j++) {
            printf("%d\t", mat[i][j]);
        }
        printf("\n");
    }
}

void somar_matrizes(int A[][MAX], int B[][MAX], int C[][MAX], int linhas, int cols) {
    for (int i = 0; i < linhas; i++) {
        for (int j = 0; j < cols; j++) {
            C[i][j] = A[i][j] + B[i][j];
        }
    }
}

int encontrar_maior(int mat[][MAX], int linhas, int cols) {
    int maior = mat[0][0];
    for (int i = 0; i < linhas; i++) {
        for (int j = 0; j < cols; j++) {
            if (mat[i][j] > maior) {
                maior = mat[i][j];
            }
        }
    }
    return maior;
}
```

---

## 6. Matrizes Dinâmicas

### Alocação Dinâmica de Matrizes

Para matrizes de tamanho desconhecido em tempo de compilação, usamos alocação dinâmica.

```c
#include <stdio.h>
#include <stdlib.h>

int main() {
    int linhas, colunas;
    
    printf("Digite o número de linhas e colunas: ");
    scanf("%d %d", &linhas, &colunas);
    
    // Alocação dinâmica de matriz
    int **matriz = (int**)malloc(linhas * sizeof(int*));
    for (int i = 0; i < linhas; i++) {
        matriz[i] = (int*)malloc(colunas * sizeof(int));
    }
    
    // Preenchimento
    printf("Preencha a matriz %dx%d:\n", linhas, colunas);
    for (int i = 0; i < linhas; i++) {
        for (int j = 0; j < colunas; j++) {
            printf("Elemento [%d][%d]: ", i, j);
            scanf("%d", &matriz[i][j]);
        }
    }
    
    // Exibição
    printf("\nMatriz criada:\n");
    for (int i = 0; i < linhas; i++) {
        for (int j = 0; j < colunas; j++) {
            printf("%d\t", matriz[i][j]);
        }
        printf("\n");
    }
    
    // Liberação da memória
    for (int i = 0; i < linhas; i++) {
        free(matriz[i]);
    }
    free(matriz);
    
    return 0;
}
```

---

## 7. Exemplos Práticos Avançados

### Sistema de Reservas de Cinema

```c
#include <stdio.h>

#define FILEIRAS 5
#define POLTRONAS 10

int main() {
    char cinema[FILEIRAS][POLTRONAS];
    int fileira, poltrona;
    char opcao;
    
    // Inicializar todas as poltronas como livres ('L')
    for (int i = 0; i < FILEIRAS; i++) {
        for (int j = 0; j < POLTRONAS; j++) {
            cinema[i][j] = 'L';  // Livre
        }
    }
    
    printf("=== SISTEMA DE RESERVAS DE CINEMA ===\n");
    
    do {
        // Exibir mapa do cinema
        printf("\nMapa do Cinema:\n");
        printf("   ");
        for (int j = 0; j < POLTRONAS; j++) {
            printf("%2d ", j + 1);
        }
        printf("\n");
        
        for (int i = 0; i < FILEIRAS; i++) {
            printf("%2d ", i + 1);
            for (int j = 0; j < POLTRONAS; j++) {
                printf(" %c ", cinema[i][j]);
            }
            printf("\n");
        }
        printf("Legenda: L = Livre, R = Reservado, O = Ocupado\n");
        
        // Menu de opções
        printf("\nOpções:\n");
        printf("R - Reservar poltrona\n");
        printf("C - Cancelar reserva\n");
        printf("O - Ocupar poltrona\n");
        printf("L - Liberar poltrona\n");
        printf("S - Sair\n");
        printf("Escolha: ");
        scanf(" %c", &opcao);
        
        if (opcao == 'S' || opcao == 's') {
            break;
        }
        
        printf("Digite fileira (1-%d) e poltrona (1-%d): ", FILEIRAS, POLTRONAS);
        scanf("%d %d", &fileira, &poltrona);
        
        // Ajustar índices (usuário digita 1-based)
        fileira--;
        poltrona--;
        
        if (fileira < 0 || fileira >= FILEIRAS || poltrona < 0 || poltrona >= POLTRONAS) {
            printf("Poltrona inválida!\n");
            continue;
        }
        
        switch (opcao) {
            case 'R': case 'r':
                if (cinema[fileira][poltrona] == 'L') {
                    cinema[fileira][poltrona] = 'R';
                    printf("Poltrona reservada com sucesso!\n");
                } else {
                    printf("Poltrona não está livre!\n");
                }
                break;
                
            case 'C': case 'c':
                if (cinema[fileira][poltrona] == 'R') {
                    cinema[fileira][poltrona] = 'L';
                    printf("Reserva cancelada!\n");
                } else {
                    printf("Poltrona não estava reservada!\n");
                }
                break;
                
            case 'O': case 'o':
                if (cinema[fileira][poltrona] == 'R' || cinema[fileira][poltrona] == 'L') {
                    cinema[fileira][poltrona] = 'O';
                    printf("Poltrona ocupada!\n");
                } else {
                    printf("Poltrona já está ocupada!\n");
                }
                break;
                
            case 'L': case 'l':
                if (cinema[fileira][poltrona] == 'O') {
                    cinema[fileira][poltrona] = 'L';
                    printf("Poltrona liberada!\n");
                } else {
                    printf("Poltrona não estava ocupada!\n");
                }
                break;
                
            default:
                printf("Opção inválida!\n");
        }
        
    } while (1);
    
    printf("Sistema encerrado. Obrigado!\n");
    return 0;
}
```

### Jogo Campo Minado Simplificado

```c
#include <stdio.h>
#include <stdlib.h>
#include <time.h>

#define TAMANHO 5
#define MINAS 5

void inicializar_tabuleiro(char tabuleiro[][TAMANHO], char visivel[][TAMANHO]) {
    // Inicializar com espaços vazios
    for (int i = 0; i < TAMANHO; i++) {
        for (int j = 0; j < TAMANHO; j++) {
            tabuleiro[i][j] = ' ';
            visivel[i][j] = '.';
        }
    }
    
    // Colocar minas aleatoriamente
    srand(time(NULL));
    for (int m = 0; m < MINAS; m++) {
        int i, j;
        do {
            i = rand() % TAMANHO;
            j = rand() % TAMANHO;
        } while (tabuleiro[i][j] == '*');
        
        tabuleiro[i][j] = '*';
    }
}

void exibir_tabuleiro(char visivel[][TAMANHO]) {
    printf("\n  ");
    for (int j = 0; j < TAMANHO; j++) {
        printf("%d ", j);
    }
    printf("\n");
    
    for (int i = 0; i < TAMANHO; i++) {
        printf("%d ", i);
        for (int j = 0; j < TAMANHO; j++) {
            printf("%c ", visivel[i][j]);
        }
        printf("\n");
    }
}

int contar_minas_vizinhas(char tabuleiro[][TAMANHO], int linha, int coluna) {
    int count = 0;
    for (int i = linha - 1; i <= linha + 1; i++) {
        for (int j = coluna - 1; j <= coluna + 1; j++) {
            if (i >= 0 && i < TAMANHO && j >= 0 && j < TAMANHO) {
                if (tabuleiro[i][j] == '*') {
                    count++;
                }
            }
        }
    }
    return count;
}

int main() {
    char tabuleiro[TAMANHO][TAMANHO];
    char visivel[TAMANHO][TAMANHO];
    int linha, coluna;
    int jogadas = 0;
    int max_jogadas = TAMANHO * TAMANHO - MINAS;
    
    inicializar_tabuleiro(tabuleiro, visivel);
    
    printf("=== CAMPO MINADO ===\n");
    printf("Encontre todas as casas sem minas!\n");
    
    while (jogadas < max_jogadas) {
        exibir_tabuleiro(visivel);
        
        printf("\nDigite linha e coluna (0-%d): ", TAMANHO - 1);
        scanf("%d %d", &linha, &coluna);
        
        if (linha < 0 || linha >= TAMANHO || coluna < 0 || coluna >= TAMANHO) {
            printf("Posição inválida!\n");
            continue;
        }
        
        if (visivel[linha][coluna] != '.') {
            printf("Posição já revelada!\n");
            continue;
        }
        
        if (tabuleiro[linha][coluna] == '*') {
            printf("💥 BOOM! Você atingiu uma mina!\n");
            visivel[linha][coluna] = '*';
            exibir_tabuleiro(visivel);
            printf("Fim de jogo! Pontuação: %d\n", jogadas);
            break;
        } else {
            int minas_vizinhas = contar_minas_vizinhas(tabuleiro, linha, coluna);
            visivel[linha][coluna] = minas_vizinhas + '0';  // Converter para char
            jogadas++;
            printf("✅ Casa segura! Minas vizinhas: %d\n", minas_vizinhas);
        }
    }
    
    if (jogadas == max_jogadas) {
        printf("🎉 PARABÉNS! Você venceu o jogo!\n");
    }
    
    return 0;
}
```

---

## 8. Boas Práticas e Dicas

### 1. Use #define para Tamanhos
```c
// ✅ BOM
#define LINHAS 5
#define COLUNAS 5
int matriz[LINHAS][COLUNAS];

// ❌ RUIM
int matriz[5][5];  // Números mágicos
```

### 2. Verifique Limites Sempre
```c
// ✅ SEGURO
for (int i = 0; i < LINHAS; i++) {
    for (int j = 0; j < COLUNAS; j++) {
        // acesso seguro
    }
}

// ❌ PERIGOSO
for (int i = 0; i <= LINHAS; i++) {  // Ultrapassa limite!
    for (int j = 0; j <= COLUNAS; j++) {
        // acesso perigoso
    }
}
```

### 3. Prefira Loops Aninhados Claros
```c
// ✅ CLARO
for (int linha = 0; linha < TOTAL_LINHAS; linha++) {
    for (int coluna = 0; coluna < TOTAL_COLUNAS; coluna++) {
        // processamento
    }
}

// ❌ CONFUSO
for (int i = 0; i < x; i++) {
    for (int j = 0; j < y; j++) {
        // difícil de entender
    }
}
```

### 4. Inicialize Sempre suas Matrizes
```c
// ✅ SEGURO
int matriz[3][3] = {0};  // Todos zeros

// ❌ ARRISCADO
int matriz[3][3];  // Lixo de memória
```

---

## 9. Exercícios de Fixação

### Exercício 1: Diagonal Principal
```c
#include <stdio.h>

#define N 3

int main() {
    int matriz[N][N] = {{1, 2, 3},
                       {4, 5, 6},
                       {7, 8, 9}};
    
    printf("Diagonal principal: ");
    for (int i = 0; i < N; i++) {
        printf("%d ", matriz[i][i]);
    }
    printf("\n");
    
    printf("Diagonal secundária: ");
    for (int i = 0; i < N; i++) {
        printf("%d ", matriz[i][N-1-i]);
    }
    printf("\n");
    
    return 0;
}
```

### Exercício 2: Matriz Identidade
```c
#include <stdio.h>

#define N 4

int main() {
    int identidade[N][N];
    
    // Preencher matriz identidade
    for (int i = 0; i < N; i++) {
        for (int j = 0; j < N; j++) {
            identidade[i][j] = (i == j) ? 1 : 0;
        }
    }
    
    printf("Matriz Identidade %dx%d:\n", N, N);
    for (int i = 0; i < N; i++) {
        for (int j = 0; j < N; j++) {
            printf("%d ", identidade[i][j]);
        }
        printf("\n");
    }
    
    return 0;
}
```

### Exercício 3: Busca em Matriz
```c
#include <stdio.h>

#define LINHAS 3
#define COLUNAS 3

int main() {
    int matriz[LINHAS][COLUNAS] = {{1, 2, 3},
                                  {4, 5, 6},
                                  {7, 8, 9}};
    int busca, encontrado = 0;
    
    printf("Digite um número para buscar: ");
    scanf("%d", &busca);
    
    for (int i = 0; i < LINHAS; i++) {
        for (int j = 0; j < COLUNAS; j++) {
            if (matriz[i][j] == busca) {
                printf("Encontrado na posição [%d][%d]\n", i, j);
                encontrado = 1;
            }
        }
    }
    
    if (!encontrado) {
        printf("Número não encontrado na matriz.\n");
    }
    
    return 0;
}
```

---

## 10. Tabela Resumo

| Operação               | Sintaxe               | Exemplo                                 |
| ---------------------- | --------------------- | --------------------------------------- |
| **Declaração**         | `tipo nome[lin][col]` | `int mat[3][3]`                         |
| **Inicialização**      | = {{valores}}         | `int mat[2][2] = {{1,2},{3,4}}`         |
| **Acesso**             | `mat[lin][col]`       | `valor = mat[0][1]`                     |
| **Percorrer**          | loops aninhados       | `for(i=0;i<lin;i++) for(j=0;j<col;j++)` |
| **Passar para função** | `tipo mat[][col]`     | `void func(int mat[][3])`               |

**Dica Final:** Pratique muito com matrizes! Elas são fundamentais para:
- Processamento de imagens
- Sistemas de coordenadas
- Jogos 2D
- Planilhas e tabelas
- Álgebra linear

Domine matrizes e você terá uma ferramenta poderosa para resolver problemas complexos! 🎯