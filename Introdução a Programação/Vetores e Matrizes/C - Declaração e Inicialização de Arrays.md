

> [!note] ## O que são Vetores?
> 
> Vetores são **arrays unidimensionais** - estruturas de dados que armazenam uma **coleção ordenada** de elementos do mesmo tipo. Pense em uma fileira de caixas numeradas onde cada caixa guarda um valor.
> 
> **Analogia:** Uma rua com casas numeradas sequencialmente, onde cada casa pode armazenar algo.
> 
> ```c
> // Vetor de 5 inteiros
> int numeros[5] = {10, 20, 30, 40, 50};
> // Índices:        0    1    2    3    4
> ```

---

## 1. Declaração e Inicialização de Vetores

### Sintaxe Básica

```c
tipo nome_vetor[tamanho];
```

**Explicação:** Os vetores em C têm **índices que começam em 0** e vão até `tamanho-1`. O tamanho deve ser uma constante inteira positiva.

### Formas de Inicialização

```c
#include <stdio.h>

int main() {
    // Método 1: Inicialização com valores
    int vetor1[5] = {10, 20, 30, 40, 50};
    
    // Método 2: Inicialização parcial (restante com 0)
    int vetor2[5] = {10, 20};  // [10, 20, 0, 0, 0]
    
    // Método 3: Inicialização sem tamanho explícito
    int vetor3[] = {1, 2, 3, 4, 5};  // Tamanho 5 inferido
    
    // Método 4: Inicialização com zeros
    int vetor4[5] = {0};  // [0, 0, 0, 0, 0]
    
    // Método 5: Declaração sem inicialização (contém lixo)
    int vetor5[5];
    
    // Método 6: Inicialização com loop
    int vetor6[5];
    for(int i = 0; i < 5; i++) {
        vetor6[i] = (i + 1) * 10;  // [10, 20, 30, 40, 50]
    }
    
    return 0;
}
```

### Exemplo Prático: Notas de Alunos

```c
#include <stdio.h>

int main() {
    // Vetor para armazenar notas de 5 alunos
    float notas[5] = {7.5, 8.0, 6.5, 9.0, 5.5};
    
    printf("Notas dos alunos:\n");
    for(int i = 0; i < 5; i++) {
        printf("Aluno %d: %.1f\n", i + 1, notas[i]);
    }
    
    return 0;
}
```
**Saída:**
```
Notas dos alunos:
Aluno 1: 7.5
Aluno 2: 8.0
Aluno 3: 6.5
Aluno 4: 9.0
Aluno 5: 5.5
```

---

## 2. Acesso e Modificação de Elementos

### Sintaxe de Acesso

```c
vetor[indice] = valor;        // Escrita
valor = vetor[indice];        // Leitura
```

**Exemplo Completo:**
```c
#include <stdio.h>

int main() {
    int numeros[5] = {10, 20, 30, 40, 50};
    
    // Acesso para leitura
    printf("Primeiro elemento: %d\n", numeros[0]);
    printf("Último elemento: %d\n", numeros[4]);
    printf("Elemento do meio: %d\n", numeros[2]);
    
    // Acesso para escrita
    numeros[1] = 99;     // Modifica o segundo elemento
    numeros[4] = 100;    // Modifica o último elemento
    
    printf("\nApós modificações:\n");
    for(int i = 0; i < 5; i++) {
        printf("numeros[%d] = %d\n", i, numeros[i]);
    }
    
    // CUIDADO: Acesso fora dos limites!
    // printf("%d\n", numeros[5]);  // COMPORTAMENTO INDEFINIDO!
    // printf("%d\n", numeros[-1]); // COMPORTAMENTO INDEFINIDO!
    
    return 0;
}
```
**Saída:**
```
Primeiro elemento: 10
Último elemento: 50
Elemento do meio: 30

Após modificações:
numeros[0] = 10
numeros[1] = 99
numeros[2] = 30
numeros[3] = 40
numeros[4] = 100
```

### Exemplo: Sistema de Temperaturas

```c
#include <stdio.h>

int main() {
    float temperaturas[7];  // 7 dias da semana
    char *dias[] = {"Dom", "Seg", "Ter", "Qua", "Qui", "Sex", "Sab"};
    
    // Entrada de dados
    printf("Digite as temperaturas da semana:\n");
    for(int i = 0; i < 7; i++) {
        printf("%s: ", dias[i]);
        scanf("%f", &temperaturas[i]);
    }
    
    // Exibição e cálculos
    printf("\n=== RELATÓRIO DE TEMPERATURAS ===\n");
    float soma = 0, maior = temperaturas[0], menor = temperaturas[0];
    
    for(int i = 0; i < 7; i++) {
        printf("%s: %.1f°C\n", dias[i], temperaturas[i]);
        soma += temperaturas[i];
        
        if(temperaturas[i] > maior) maior = temperaturas[i];
        if(temperaturas[i] < menor) menor = temperaturas[i];
    }
    
    printf("\nMédia da semana: %.1f°C\n", soma / 7);
    printf("Maior temperatura: %.1f°C\n", maior);
    printf("Menor temperatura: %.1f°C\n", menor);
    
    return 0;
}
```

---

## 3. Operações Comuns com Vetores

### Percorrendo Vetores

```c
#include <stdio.h>

int main() {
    int vetor[8] = {5, 2, 8, 1, 9, 3, 7, 4};
    
    printf("Vetor original: ");
    for(int i = 0; i < 8; i++) {
        printf("%d ", vetor[i]);
    }
    printf("\n");
    
    // Percorrendo do final para o início
    printf("Vetor invertido: ");
    for(int i = 7; i >= 0; i--) {
        printf("%d ", vetor[i]);
    }
    printf("\n");
    
    // Percorrendo de 2 em 2
    printf("Elementos pares (índices): ");
    for(int i = 0; i < 8; i += 2) {
        printf("%d ", vetor[i]);
    }
    printf("\n");
    
    return 0;
}
```

### Busca em Vetor

```c
#include <stdio.h>

int main() {
    int numeros[10] = {23, 45, 12, 67, 89, 34, 56, 78, 90, 11};
    int busca, posicao = -1;
    
    printf("Vetor: ");
    for(int i = 0; i < 10; i++) {
        printf("%d ", numeros[i]);
    }
    printf("\n");
    
    printf("Digite um número para buscar: ");
    scanf("%d", &busca);
    
    // Busca linear
    for(int i = 0; i < 10; i++) {
        if(numeros[i] == busca) {
            posicao = i;
            break;
        }
    }
    
    if(posicao != -1) {
        printf("Número encontrado na posição %d\n", posicao);
    } else {
        printf("Número não encontrado no vetor\n");
    }
    
    return 0;
}
```

### Ordenação (Bubble Sort)

```c
#include <stdio.h>

int main() {
    int vetor[6] = {64, 34, 25, 12, 22, 11};
    int n = 6;
    
    printf("Vetor original: ");
    for(int i = 0; i < n; i++) {
        printf("%d ", vetor[i]);
    }
    printf("\n");
    
    // Algoritmo Bubble Sort
    for(int i = 0; i < n - 1; i++) {
        for(int j = 0; j < n - i - 1; j++) {
            if(vetor[j] > vetor[j + 1]) {
                // Troca os elementos
                int temp = vetor[j];
                vetor[j] = vetor[j + 1];
                vetor[j + 1] = temp;
            }
        }
    }
    
    printf("Vetor ordenado: ");
    for(int i = 0; i < n; i++) {
        printf("%d ", vetor[i]);
    }
    printf("\n");
    
    return 0;
}
```

---

## 4. Vetores como Parâmetros de Função

### Passando Vetores para Funções

Quando passamos vetores para funções, eles são passados **por referência** (na verdade, passamos o endereço do primeiro elemento).

```c
#include <stdio.h>

// Protótipos das funções
void imprimir_vetor(int vet[], int tamanho);
void preencher_vetor(int vet[], int tamanho);
int somar_vetor(int vet[], int tamanho);
int encontrar_maior(int vet[], int tamanho);

int main() {
    const int TAMANHO = 5;
    int numeros[TAMANHO];
    
    preencher_vetor(numeros, TAMANHO);
    imprimir_vetor(numeros, TAMANHO);
    
    int soma = somar_vetor(numeros, TAMANHO);
    int maior = encontrar_maior(numeros, TAMANHO);
    
    printf("Soma dos elementos: %d\n", soma);
    printf("Maior elemento: %d\n", maior);
    
    return 0;
}

void imprimir_vetor(int vet[], int tamanho) {
    printf("Vetor: ");
    for(int i = 0; i < tamanho; i++) {
        printf("%d ", vet[i]);
    }
    printf("\n");
}

void preencher_vetor(int vet[], int tamanho) {
    printf("Digite %d números:\n", tamanho);
    for(int i = 0; i < tamanho; i++) {
        printf("Elemento %d: ", i);
        scanf("%d", &vet[i]);
    }
}

int somar_vetor(int vet[], int tamanho) {
    int soma = 0;
    for(int i = 0; i < tamanho; i++) {
        soma += vet[i];
    }
    return soma;
}

int encontrar_maior(int vet[], int tamanho) {
    int maior = vet[0];
    for(int i = 1; i < tamanho; i++) {
        if(vet[i] > maior) {
            maior = vet[i];
        }
    }
    return maior;
}
```

### Exemplo: Operações Matemáticas com Vetores

```c
#include <stdio.h>
#include <math.h>

// Protótipos
float calcular_media(float vet[], int n);
float calcular_desvio_padrao(float vet[], int n);
void normalizar_vetor(float vet[], int n);

int main() {
    float valores[5];
    int n = 5;
    
    printf("Digite 5 valores:\n");
    for(int i = 0; i < n; i++) {
        printf("Valor %d: ", i + 1);
        scanf("%f", &valores[i]);
    }
    
    float media = calcular_media(valores, n);
    float desvio = calcular_desvio_padrao(valores, n);
    
    printf("\nMédia: %.2f\n", media);
    printf("Desvio padrão: %.2f\n", desvio);
    
    normalizar_vetor(valores, n);
    printf("Vetor normalizado: ");
    for(int i = 0; i < n; i++) {
        printf("%.2f ", valores[i]);
    }
    printf("\n");
    
    return 0;
}

float calcular_media(float vet[], int n) {
    float soma = 0;
    for(int i = 0; i < n; i++) {
        soma += vet[i];
    }
    return soma / n;
}

float calcular_desvio_padrao(float vet[], int n) {
    float media = calcular_media(vet, n);
    float soma_quadrados = 0;
    
    for(int i = 0; i < n; i++) {
        soma_quadrados += (vet[i] - media) * (vet[i] - media);
    }
    
    return sqrt(soma_quadrados / n);
}

void normalizar_vetor(float vet[], int n) {
    float maior = vet[0];
    
    // Encontrar o maior valor absoluto
    for(int i = 1; i < n; i++) {
        if(fabs(vet[i]) > fabs(maior)) {
            maior = vet[i];
        }
    }
    
    // Normalizar dividindo pelo maior
    if(maior != 0) {
        for(int i = 0; i < n; i++) {
            vet[i] /= maior;
        }
    }
}
```

---

## 5. Vetores e Ponteiros

### Relação entre Vetores e Ponteiros

Em C, vetores e ponteiros estão intimamente relacionados. O nome do vetor é um ponteiro para seu primeiro elemento.

```c
#include <stdio.h>

int main() {
    int vetor[5] = {10, 20, 30, 40, 50};
    
    printf("Endereços e valores:\n");
    printf("vetor    = %p\n", (void*)vetor);
    printf("&vetor[0] = %p\n", (void*)&vetor[0]);
    printf("*vetor   = %d\n", *vetor);
    
    printf("\nAcessando com diferentes sintaxes:\n");
    for(int i = 0; i < 5; i++) {
        printf("vetor[%d] = %d\n", i, vetor[i]);
        printf("*(vetor + %d) = %d\n", i, *(vetor + i));
        printf("&vetor[%d] = %p\n", i, (void*)&vetor[i]);
        printf("vetor + %d = %p\n\n", i, (void*)(vetor + i));
    }
    
    return 0;
}
```

### Aritmética de Ponteiros com Vetores

```c
#include <stdio.h>

int main() {
    int numeros[6] = {100, 200, 300, 400, 500, 600};
    int *ptr = numeros;  // ptr aponta para o primeiro elemento
    
    printf("Acessando com aritmética de ponteiros:\n");
    for(int i = 0; i < 6; i++) {
        printf("*(ptr + %d) = %d\n", i, *(ptr + i));
    }
    
    printf("\nPercorrendo com incremento do ponteiro:\n");
    for(int i = 0; i < 6; i++) {
        printf("*ptr = %d (endereço: %p)\n", *ptr, (void*)ptr);
        ptr++;  // Avança para o próximo elemento
    }
    
    return 0;
}
```

---

## 6. Vetores de Diferentes Tipos

### Vetores de Caracteres (Strings)

```c
#include <stdio.h>
#include <string.h>

int main() {
    // Vetor de caracteres (string)
    char nome[20] = "Maria Silva";
    char cidade[30];
    
    printf("Nome: %s\n", nome);
    printf("Tamanho da string: %zu\n", strlen(nome));
    
    // Entrada de string
    printf("Digite sua cidade: ");
    scanf("%29s", cidade);  // Limita a 29 caracteres + \0
    
    printf("Cidade: %s\n", cidade);
    
    // Percorrendo caractere por caractere
    printf("Letras do nome: ");
    for(int i = 0; nome[i] != '\0'; i++) {
        printf("%c ", nome[i]);
    }
    printf("\n");
    
    return 0;
}
```

### Vetores de Estruturas

```c
#include <stdio.h>

struct Pessoa {
    char nome[50];
    int idade;
    float altura;
};

int main() {
    struct Pessoa pessoas[3];
    
    // Entrada de dados
    printf("Cadastro de 3 pessoas:\n");
    for(int i = 0; i < 3; i++) {
        printf("\nPessoa %d:\n", i + 1);
        printf("Nome: ");
        scanf("%49s", pessoas[i].nome);
        printf("Idade: ");
        scanf("%d", &pessoas[i].idade);
        printf("Altura: ");
        scanf("%f", &pessoas[i].altura);
    }
    
    // Exibição
    printf("\n=== PESSOAS CADASTRADAS ===\n");
    for(int i = 0; i < 3; i++) {
        printf("%s, %d anos, %.2fm\n", 
               pessoas[i].nome, pessoas[i].idade, pessoas[i].altura);
    }
    
    return 0;
}
```

---

## 7. Exemplos Práticos Avançados

### Sistema de Gerenciamento de Tarefas

```c
#include <stdio.h>
#include <string.h>

#define MAX_TAREFAS 10
#define TAM_DESCRICAO 100

struct Tarefa {
    char descricao[TAM_DESCRICAO];
    int concluida;  // 0 = não, 1 = sim
};

int main() {
    struct Tarefa tarefas[MAX_TAREFAS];
    int num_tarefas = 0;
    int opcao;
    
    printf("=== GERENCIADOR DE TAREFAS ===\n");
    
    do {
        printf("\nMenu:\n");
        printf("1. Adicionar tarefa\n");
        printf("2. Listar tarefas\n");
        printf("3. Marcar como concluída\n");
        printf("4. Remover tarefa\n");
        printf("0. Sair\n");
        printf("Escolha: ");
        scanf("%d", &opcao);
        getchar();  // Limpa o \n do buffer
        
        switch(opcao) {
            case 1:  // Adicionar tarefa
                if(num_tarefas < MAX_TAREFAS) {
                    printf("Descrição da tarefa: ");
                    fgets(tarefas[num_tarefas].descricao, TAM_DESCRICAO, stdin);
                    // Remove o \n do final
                    tarefas[num_tarefas].descricao[strcspn(tarefas[num_tarefas].descricao, "\n")] = '\0';
                    tarefas[num_tarefas].concluida = 0;
                    num_tarefas++;
                    printf("Tarefa adicionada!\n");
                } else {
                    printf("Limite de tarefas atingido!\n");
                }
                break;
                
            case 2:  // Listar tarefas
                printf("\n=== LISTA DE TAREFAS ===\n");
                for(int i = 0; i < num_tarefas; i++) {
                    printf("%d. [%c] %s\n", i + 1, 
                           tarefas[i].concluida ? 'X' : ' ', 
                           tarefas[i].descricao);
                }
                break;
                
            case 3:  // Marcar como concluída
                if(num_tarefas > 0) {
                    int idx;
                    printf("Número da tarefa a marcar: ");
                    scanf("%d", &idx);
                    if(idx >= 1 && idx <= num_tarefas) {
                        tarefas[idx - 1].concluida = 1;
                        printf("Tarefa marcada como concluída!\n");
                    } else {
                        printf("Tarefa inválida!\n");
                    }
                } else {
                    printf("Nenhuma tarefa cadastrada!\n");
                }
                break;
                
            case 0:
                printf("Saindo...\n");
                break;
                
            default:
                printf("Opção inválida!\n");
        }
        
    } while(opcao != 0);
    
    return 0;
}
```

### Simulador de Dados

```c
#include <stdio.h>
#include <stdlib.h>
#include <time.h>

int main() {
    int lancamentos[6] = {0};  // Contador para cada face (1-6)
    int total_lancamentos;
    
    srand(time(NULL));  // Inicializa gerador aleatório
    
    printf("Quantos lançamentos de dado? ");
    scanf("%d", &total_lancamentos);
    
    // Simula os lançamentos
    for(int i = 0; i < total_lancamentos; i++) {
        int resultado = rand() % 6 + 1;  // Número entre 1-6
        lancamentos[resultado - 1]++;
    }
    
    // Exibe resultados
    printf("\n=== RESULTADOS DOS LANÇAMENTOS ===\n");
    for(int i = 0; i < 6; i++) {
        float percentual = (float)lancamentos[i] / total_lancamentos * 100;
        printf("Face %d: %d vezes (%.1f%%)\n", i + 1, lancamentos[i], percentual);
    }
    
    // Encontra a face mais frequente
    int mais_frequente = 0;
    for(int i = 1; i < 6; i++) {
        if(lancamentos[i] > lancamentos[mais_frequente]) {
            mais_frequente = i;
        }
    }
    
    printf("\nFace mais frequente: %d (%d vezes)\n", 
           mais_frequente + 1, lancamentos[mais_frequente]);
    
    return 0;
}
```

---

## 8. Exemplos de Álgebra Vetorial e Geometria Analítica

### Operações Básicas com Vetores Matemáticos

```c
#include <stdio.h>
#include <math.h>

#define DIM 3  // Dimensão dos vetores (3D)

// Protótipos das funções
void somar_vetores(double v1[], double v2[], double resultado[], int dim);
double produto_escalar(double v1[], double v2[], int dim);
double norma_vetor(double vetor[], int dim);
void produto_por_escalar(double vetor[], double escalar, double resultado[], int dim);
void imprimir_vetor(double vetor[], int dim, char* nome);

int main() {
    double vetorA[DIM] = {1.0, 2.0, 3.0};
    double vetorB[DIM] = {4.0, 5.0, 6.0};
    double resultado[DIM];
    
    printf("=== OPERAÇÕES VETORIAIS EM R³ ===\n\n");
    
    imprimir_vetor(vetorA, DIM, "Vetor A");
    imprimir_vetor(vetorB, DIM, "Vetor B");
    
    // Soma de vetores
    somar_vetores(vetorA, vetorB, resultado, DIM);
    imprimir_vetor(resultado, DIM, "A + B");
    
    // Produto escalar
    double escalar = produto_escalar(vetorA, vetorB, DIM);
    printf("Produto escalar A·B = %.2f\n\n", escalar);
    
    // Norma (módulo) dos vetores
    printf("Norma de A: ||A|| = %.2f\n", norma_vetor(vetorA, DIM));
    printf("Norma de B: ||B|| = %.2f\n\n", norma_vetor(vetorB, DIM));
    
    // Produto por escalar
    produto_por_escalar(vetorA, 2.5, resultado, DIM);
    imprimir_vetor(resultado, DIM, "2.5 × A");
    
    // Vetor nulo
    double vetorNulo[DIM] = {0};
    imprimir_vetor(vetorNulo, DIM, "Vetor nulo");
    
    return 0;
}

void somar_vetores(double v1[], double v2[], double resultado[], int dim) {
    for(int i = 0; i < dim; i++) {
        resultado[i] = v1[i] + v2[i];
    }
}

double produto_escalar(double v1[], double v2[], int dim) {
    double resultado = 0;
    for(int i = 0; i < dim; i++) {
        resultado += v1[i] * v2[i];
    }
    return resultado;
}

double norma_vetor(double vetor[], int dim) {
    double soma_quadrados = 0;
    for(int i = 0; i < dim; i++) {
        soma_quadrados += vetor[i] * vetor[i];
    }
    return sqrt(soma_quadrados);
}

void produto_por_escalar(double vetor[], double escalar, double resultado[], int dim) {
    for(int i = 0; i < dim; i++) {
        resultado[i] = vetor[i] * escalar;
    }
}

void imprimir_vetor(double vetor[], int dim, char* nome) {
    printf("%s = (", nome);
    for(int i = 0; i < dim; i++) {
        printf("%.1f", vetor[i]);
        if(i < dim - 1) printf(", ");
    }
    printf(")\n");
}
```

### Produto Vetorial e Ângulo entre Vetores

```c
#include <stdio.h>
#include <math.h>

#define PI 3.14159265358979323846

void produto_vetorial(double v1[], double v2[], double resultado[]) {
    // Apenas para R³
    resultado[0] = v1[1] * v2[2] - v1[2] * v2[1];
    resultado[1] = v1[2] * v2[0] - v1[0] * v2[2];
    resultado[2] = v1[0] * v2[1] - v1[1] * v2[0];
}

double angulo_entre_vetores(double v1[], double v2[], int dim) {
    double produto = 0;
    double norma1 = 0, norma2 = 0;
    
    for(int i = 0; i < dim; i++) {
        produto += v1[i] * v2[i];
        norma1 += v1[i] * v1[i];
        norma2 += v2[i] * v2[i];
    }
    
    norma1 = sqrt(norma1);
    norma2 = sqrt(norma2);
    
    if(norma1 == 0 || norma2 == 0) return 0;
    
    double cos_theta = produto / (norma1 * norma2);
    // Garante que cos_theta está no intervalo [-1, 1]
    if(cos_theta > 1.0) cos_theta = 1.0;
    if(cos_theta < -1.0) cos_theta = -1.0;
    
    return acos(cos_theta) * 180.0 / PI;  // Retorna em graus
}

int main() {
    double u[3] = {1, 0, 0};  // Vetor unitário no eixo X
    double v[3] = {0, 1, 0};  // Vetor unitário no eixo Y
    double w[3] = {1, 1, 0};  // Vetor a 45 graus
    double resultado[3];
    
    printf("=== GEOMETRIA ANALÍTICA AVANÇADA ===\n\n");
    
    // Produto vetorial
    produto_vetorial(u, v, resultado);
    printf("u × v = (%.1f, %.1f, %.1f)\n", resultado[0], resultado[1], resultado[2]);
    
    // Ângulos entre vetores
    printf("Ângulo entre u e v: %.1f°\n", angulo_entre_vetores(u, v, 3));
    printf("Ângulo entre u e w: %.1f°\n", angulo_entre_vetores(u, w, 3));
    printf("Ângulo entre v e w: %.1f°\n", angulo_entre_vetores(v, w, 3));
    
    // Vetores paralelos e ortogonais
    double paralelo[3] = {2, 4, 6};
    double ortogonal[3] = {2, -1, 0};
    
    printf("\nVerificação de ortogonalidade:\n");
    double produto = 0;
    for(int i = 0; i < 3; i++) {
        produto += paralelo[i] * ortogonal[i];
    }
    printf("Produto escalar entre vetores 'paralelo' e 'ortogonal': %.1f\n", produto);
    printf("São ortogonais? %s\n", produto == 0 ? "SIM" : "NÃO");
    
    return 0;
}
```

### Sistema de Coordenadas e Transformações

```c
#include <stdio.h>
#include <math.h>

#define DIM 3

typedef struct {
    double componentes[DIM];
} Vetor;

// Funções para operações com vetores
Vetor criar_vetor(double x, double y, double z) {
    Vetor v;
    v.componentes[0] = x;
    v.componentes[1] = y;
    v.componentes[2] = z;
    return v;
}

Vetor soma_vetorial(Vetor a, Vetor b) {
    Vetor resultado;
    for(int i = 0; i < DIM; i++) {
        resultado.componentes[i] = a.componentes[i] + b.componentes[i];
    }
    return resultado;
}

Vetor subtracao_vetorial(Vetor a, Vetor b) {
    Vetor resultado;
    for(int i = 0; i < DIM; i++) {
        resultado.componentes[i] = a.componentes[i] - b.componentes[i];
    }
    return resultado;
}

double produto_escalar(Vetor a, Vetor b) {
    double resultado = 0;
    for(int i = 0; i < DIM; i++) {
        resultado += a.componentes[i] * b.componentes[i];
    }
    return resultado;
}

Vetor produto_por_escalar(Vetor v, double k) {
    Vetor resultado;
    for(int i = 0; i < DIM; i++) {
        resultado.componentes[i] = v.componentes[i] * k;
    }
    return resultado;
}

double norma(Vetor v) {
    return sqrt(produto_escalar(v, v));
}

Vetor normalizar(Vetor v) {
    double n = norma(v);
    if(n == 0) return v;  // Evita divisão por zero
    return produto_por_escalar(v, 1.0/n);
}

void imprimir_vetor(Vetor v, char* nome) {
    printf("%s = (%.2f, %.2f, %.2f)\n", nome, 
           v.componentes[0], v.componentes[1], v.componentes[2]);
}

int main() {
    printf("=== SISTEMA DE COORDENADAS 3D ===\n\n");
    
    // Vetores base canônica
    Vetor i = criar_vetor(1, 0, 0);
    Vetor j = criar_vetor(0, 1, 0);
    Vetor k = criar_vetor(0, 0, 1);
    
    printf("Base canônica:\n");
    imprimir_vetor(i, "i");
    imprimir_vetor(j, "j");
    imprimir_vetor(k, "k");
    
    // Vetores de exemplo
    Vetor a = criar_vetor(3, 4, 0);
    Vetor b = criar_vetor(1, -2, 5);
    
    printf("\nVetores de exemplo:\n");
    imprimir_vetor(a, "a");
    imprimir_vetor(b, "b");
    
    // Operações
    printf("\nOperações vetoriais:\n");
    Vetor soma = soma_vetorial(a, b);
    imprimir_vetor(soma, "a + b");
    
    Vetor subtracao = subtracao_vetorial(a, b);
    imprimir_vetor(subtracao, "a - b");
    
    double escalar = produto_escalar(a, b);
    printf("a · b = %.2f\n", escalar);
    
    printf("\nNormas:\n");
    printf("||a|| = %.2f\n", norma(a));
    printf("||b|| = %.2f\n", norma(b));
    
    // Vetores unitários
    printf("\nVetores unitários:\n");
    Vetor a_unit = normalizar(a);
    Vetor b_unit = normalizar(b);
    imprimir_vetor(a_unit, "a_unit");
    imprimir_vetor(b_unit, "b_unit");
    printf("Norma de a_unit: %.6f\n", norma(a_unit));
    printf("Norma de b_unit: %.6f\n", norma(b_unit));
    
    // Combinação linear
    printf("\nCombinação linear:\n");
    printf("a = %.1f*i + %.1f*j + %.1f*k\n", 
           a.componentes[0], a.componentes[1], a.componentes[2]);
    
    return 0;
}
```

### Projeção de Vetores e Componentes

```c
#include <stdio.h>
#include <math.h>

#define DIM 3

typedef double Vetor[DIM];

void projetar_vetor(Vetor v, Vetor sobre, Vetor projecao) {
    // proj_u(v) = ((v·u)/(u·u)) * u
    
    double produto_vu = 0, produto_uu = 0;
    
    for(int i = 0; i < DIM; i++) {
        produto_vu += v[i] * sobre[i];
        produto_uu += sobre[i] * sobre[i];
    }
    
    if(produto_uu == 0) {
        // Vetor nulo - projeção indefinida
        for(int i = 0; i < DIM; i++) {
            projecao[i] = 0;
        }
        return;
    }
    
    double escalar = produto_vu / produto_uu;
    
    for(int i = 0; i < DIM; i++) {
        projecao[i] = escalar * sobre[i];
    }
}

void componente_ortogonal(Vetor v, Vetor sobre, Vetor componente) {
    Vetor projecao;
    projetar_vetor(v, sobre, projecao);
    
    for(int i = 0; i < DIM; i++) {
        componente[i] = v[i] - projecao[i];
    }
}

double produto_escalar(Vetor a, Vetor b) {
    double resultado = 0;
    for(int i = 0; i < DIM; i++) {
        resultado += a[i] * b[i];
    }
    return resultado;
}

void imprimir_vetor(Vetor v, char* nome) {
    printf("%s = (", nome);
    for(int i = 0; i < DIM; i++) {
        printf("%.2f", v[i]);
        if(i < DIM - 1) printf(", ");
    }
    printf(")\n");
}

int main() {
    printf("=== PROJEÇÕES E COMPONENTES VETORIAIS ===\n\n");
    
    Vetor v = {4, 3, 0};      // Vetor a ser projetado
    Vetor u = {1, 0, 0};      // Direção da projeção (eixo X)
    Vetor projecao, componente;
    
    printf("Decomposição do vetor v em componentes paralela e ortogonal a u:\n\n");
    imprimir_vetor(v, "v");
    imprimir_vetor(u, "u");
    
    projetar_vetor(v, u, projecao);
    componente_ortogonal(v, u, componente);
    
    printf("\nResultados:\n");
    imprimir_vetor(projecao, "Projeção de v sobre u");
    imprimir_vetor(componente, "Componente ortogonal");
    
    // Verificação: projecao + componente = v
    Vetor soma;
    for(int i = 0; i < DIM; i++) {
        soma[i] = projecao[i] + componente[i];
    }
    printf("\nVerificação (projeção + componente):\n");
    imprimir_vetor(soma, "Soma");
    
    // Verificação de ortogonalidade
    double produto = produto_escalar(projecao, componente);
    printf("\nProduto escalar entre projeção e componente: %.6f\n", produto);
    printf("São ortogonais? %s\n", fabs(produto) < 1e-10 ? "SIM" : "NÃO");
    
    // Exemplo 2: Projeção em vetor arbitrário
    printf("\n=== EXEMPLO 2 ===\n");
    Vetor v2 = {2, 4, 1};
    Vetor u2 = {1, 1, 0};
    
    imprimir_vetor(v2, "v2");
    imprimir_vetor(u2, "u2");
    
    projetar_vetor(v2, u2, projecao);
    componente_ortogonal(v2, u2, componente);
    
    imprimir_vetor(projecao, "Projeção de v2 sobre u2");
    imprimir_vetor(componente, "Componente ortogonal");
    
    return 0;
}
```

### Cálculo de Áreas e Volumes

```c
#include <stdio.h>
#include <math.h>

#define DIM 3

typedef double Vetor[DIM];

double produto_escalar(Vetor a, Vetor b) {
    double resultado = 0;
    for(int i = 0; i < DIM; i++) {
        resultado += a[i] * b[i];
    }
    return resultado;
}

void produto_vetorial(Vetor a, Vetor b, Vetor resultado) {
    resultado[0] = a[1] * b[2] - a[2] * b[1];
    resultado[1] = a[2] * b[0] - a[0] * b[2];
    resultado[2] = a[0] * b[1] - a[1] * b[0];
}

double norma(Vetor v) {
    return sqrt(produto_escalar(v, v));
}

double area_triangulo(Vetor a, Vetor b, Vetor c) {
    // Vetores dos lados
    Vetor ab, ac;
    for(int i = 0; i < DIM; i++) {
        ab[i] = b[i] - a[i];
        ac[i] = c[i] - a[i];
    }
    
    // Produto vetorial
    Vetor produto;
    produto_vetorial(ab, ac, produto);
    
    // Área = 1/2 da norma do produto vetorial
    return norma(produto) / 2.0;
}

double area_paralelogramo(Vetor a, Vetor b) {
    Vetor produto;
    produto_vetorial(a, b, produto);
    return norma(produto);
}

double volume_paralelepipedo(Vetor a, Vetor b, Vetor c) {
    // Produto misto: a · (b × c)
    Vetor produto_vetorial_bc;
    produto_vetorial(b, c, produto_vetorial_bc);
    return fabs(produto_escalar(a, produto_vetorial_bc));
}

void imprimir_vetor(Vetor v, char* nome) {
    printf("%s = (", nome);
    for(int i = 0; i < DIM; i++) {
        printf("%.1f", v[i]);
        if(i < DIM - 1) printf(", ");
    }
    printf(")\n");
}

int main() {
    printf("=== CÁLCULO DE ÁREAS E VOLUMES ===\n\n");
    
    // Exemplo 1: Triângulo no plano XY
    Vetor A = {0, 0, 0};
    Vetor B = {4, 0, 0};
    Vetor C = {0, 3, 0};
    
    printf("Triângulo retângulo:\n");
    imprimir_vetor(A, "A");
    imprimir_vetor(B, "B");
    imprimir_vetor(C, "C");
    
    double area = area_triangulo(A, B, C);
    printf("Área do triângulo ABC: %.2f\n", area);
    printf("(Esperado: 6.00)\n\n");
    
    // Exemplo 2: Paralelogramo
    Vetor u = {3, 0, 0};
    Vetor v = {1, 2, 0};
    
    printf("Paralelogramo definido por:\n");
    imprimir_vetor(u, "u");
    imprimir_vetor(v, "v");
    
    double area_para = area_paralelogramo(u, v);
    printf("Área do paralelogramo: %.2f\n\n", area_para);
    
    // Exemplo 3: Paralelepípedo
    Vetor a = {2, 0, 0};
    Vetor b = {0, 3, 0};
    Vetor c = {0, 0, 4};
    
    printf("Paralelepípedo definido por:\n");
    imprimir_vetor(a, "a");
    imprimir_vetor(b, "b");
    imprimir_vetor(c, "c");
    
    double volume = volume_paralelepipedo(a, b, c);
    printf("Volume do paralelepípedo: %.2f\n", volume);
    printf("(Esperado: 24.00)\n");
    
    return 0;
}
```

---

## 9. Boas Práticas e Dicas

### 1. Use Constantes para Tamanhos
```c
// ✅ BOM
#define MAX_ALUNOS 50
int notas[MAX_ALUNOS];

// ❌ RUIM  
int notas[50];  // Número mágico
```

### 2. Sempre Verifique Limites
```c
// ✅ SEGURO
for(int i = 0; i < tamanho; i++) {
    // acesso seguro
}

// ❌ PERIGOSO
for(int i = 0; i <= tamanho; i++) {  // Ultrapassa limite!
    // comportamento indefinido
}
```

### 3. Inicialize Vetores
```c
// ✅ SEGURO
int vetor[5] = {0};  // Todos zeros

// ❌ ARRISCADO
int vetor[5];  // Lixo de memória
```

### 4. Use Nomes Significativos
```c
// ✅ CLARO
float temperaturas_diarias[7];
int idades_alunos[30];

// ❌ CONFUSO
float temp[7];
int a[30];
```

---

## 10. Tabela Resumo

| Operação | Sintaxe | Exemplo |
|----------|---------|---------|
| **Declaração** | `tipo nome[tam]` | `int vet[10]` |
| **Inicialização** | `= {valores}` | `int vet[3] = {1,2,3}` |
| **Acesso** | `vetor[indice]` | `valor = vet[0]` |
| **Percorrer** | loop com índice | `for(i=0;i<n;i++)` |
| **Passar para função** | `tipo vet[]` | `void func(int vet[])` |

**Dica Final:** Vetores são a base para estruturas de dados mais complexas. Domine bem os vetores antes de partir para listas, pilhas e outras estruturas! 🎯

Os exemplos de álgebra vetorial mostram como a programação pode ser usada para resolver problemas matemáticos complexos de forma elegante e eficiente! 📐✨