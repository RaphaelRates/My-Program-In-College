
> [!seealso] ## 📋 Visão Geral
>  disciplina de Introdução à Programação em C é fundamental para construir a base algorítmica necessária para todo o curso. Foca na lógica de programação e conceitos fundamentais da ciência da computação.

---
> [!abstract] 
> ## Computação
> Uma área com possui o objetivo de auxiliar as pessoas com trabalhos que podem ser automatizados e diminuir esforços e economizar tempo.
> 
> O computador é capaz de auxiliar em qualquer coisa que lhe seja solicitada, mas;
> 
>  - ○ Não tem iniciativa
>  -  ○ Não é independente
>  - ○ Não é criativo nem inteligente
>  
>  É necessário que o computador receba suas instruções nos mínimos detalhes, para que tenha condições de realizar suas tarefas

> [!tip] 
>## Finalidade de um computador
>O computador deve receber, manipular e armazenar dados (todas essas operações são realizadas por meio de programas) ● Quando construímos um software para realizar determinado processamento de dados, devemos escrever um programa ou vários programas interligados ● Para que o computador consiga ler o programa e entender o que fazer, este programa deve ser escrito em uma linguagem que o computador entenda →LINGUAGEM DE PROGRAMAÇÃO
## 📚 Conteúdo Programático

### C - Módulo 1 - Fundamentos

- **1.1** [[C - História da Linguagem]] e compiladores
- **1.2** [[C - Ambiente de Desenvolvimento]] (DevC++, Code::Blocks, GCC)
- **1.3** [[C - Estrutura Básica de um Programa]]
- **1.4** [[C - Processo de Compilação e Linkagem]]

### C - Módulo 2 - Elementos Básicos

- **2.1** [[C - Tipos de Dados Primitivos]] (`int`, `float`, `double`, `char`)
- **2.2* [[C - Constantes e Diretivas de Preprocessador]] (`#define`)
- **2.3** [[C - Operadores]] (aritméticos, relacionais e lógicos)
- **2.4** [[C - Precedência de Operadores]]

### C - Módulo 3 - Entrada e Saída

- **3.1** [[C - Funções printf e scanf]]
- **3.2** [[C - Validação de Entrada]]

### C - Módulo 4 - Estruturas de Controle

- **4.1** [[C - Estruturas Condicionais]]:
    - `if`, `else if`, `else`
    - `switch-case`
    - Operador ternário
- **4.2** [[C - Estruturas de Repetição]]:
    - `for`, `while`, `do-while`
    - `break` e `continue`
    - Loops aninhados

### C - Módulo 5 - Vetores e Matrizes

- **5.1** [[C - Declaração e Inicialização de Arrays]]
- **5.2** [[C - Matrizes Multidimensionais]]
- **5.2** [[C - Strings como Arrays de Caracteres]]

### C - Módulo 6 - Funções

- **6.1** [[C - Definição e Declaração de Funções]]
- **6.2** [[C - Passagem de Parâmetros por Valor]]
- **6.3** [[C - Recursão]]
- **6.4** [[C - Bibliotecas Padrão]]

### C - Módulo 7 - Ponteiros

- **7.1** [[C - Conceito de Endereço de Memória]]
- **7.2** [[C - Declaração e Inicialização de Ponteiros]]
- **7.3** [[C - Operadores de Ponteiros]] (`&` e `*`)
- **7.4** [[C - Passagem por Referência]]
- **7.5** [[C - Ponteiros e Arrays]]
- **7.6** [[C - Aritmética de Ponteiros]]

### C - Módulo 8 - Alocação Dinâmica

- **8.1** [[C - Funções de Alocação]] (`malloc()`, `calloc()`, `realloc()`)
- **8.2** [[C - Função free()]]
- **8.3** [[C - Vazamentos de Memória]]
- **8.4** [[C - Arrays Dinâmicos]]

### C - Módulo 9 - Estruturas (Structs)

- **9.1** [[C - Definição de Tipos de Dados Compostos]]
- **9.2** [[C - Acesso aos Membros de Estruturas]]
- **9.3** [[C - Arrays de Estruturas]]
- **9.4** [[C - Ponteiros para Estruturas]]
- **9.5** [[C - Estruturas Aninhadas]]

### C - Módulo 10 - Arquivos

- **10.1** [[C - Funções fopen e fclose]]
- **10.2** [[C - Funções de Leitura]] (`fgetc()`, `fgets()`, `fscanf()`)
- **10.3** [[C - Funções de Escrita]] (`fputc()`, `fputs()`, `fprintf()`)
- **10.4** [[C - Posicionamento em Arquivos]] (`fseek()`, `ftell()`)

---

## 🛠️ Projetos Práticos

### Projetos Básicos

- [ ] Projeto - Calculadora Simples em C
- [ ] Projeto - Sistema de Notas
- [ ] Projeto - Controle de Estoque

### Projetos Intermediários

- [ ] Projeto - Jogo da Velha em C
- [ ] Projeto - Sistema de Cadastro com Arquivos
- [ ] Projeto - Implementação de Algoritmos de Ordenação

### Projeto Final

- [ ] Projeto - Sistema de Biblioteca em C
- [ ] Projeto - Sistema Guanabara em C

---

## 📖 Recursos de Estudo

### Livros Recomendados

- Livro - C How to Program (Deitel)
- Livro - Linguagem C (Luis Damas)
- Livro - C A Linguagem de Programação (Kernighan & Ritchie)

### Ambientes de Desenvolvimento

- IDE - Dev-C++
- [[IDE - Code::Blocks]]
- Compilador - GCC
- Editor - Visual Studio Code

### Plataformas de Prática

- URI Online Judge
- HackerRank - C
- CodeWars
- Beecrowd

---

## 🔗 Conexões com Outras Disciplinas

### Pré-requisitos

- [[Matemática Discreta]]

### Disciplinas Relacionadas

- [[Estruturas de Dados]] → Utiliza conceitos de C
- [[Sistemas Operacionais]] → Programação em baixo nível
- [[Arquitetura de Computadores]] → Relação com hardware

### Próximas Disciplinas

- [[Programação Orientada a Objetos]] → Evolução para paradigma OO
- [[Estruturas de Dados]] → Estruturas avançadas
- [[Banco de Dados]] → Manipulação de dados

---

## 📝 Anotações e Lembretes

### Conceitos Importantes

```c
// Exemplo de programa básico em C
#include <stdio.h>

int main() {
    printf("Hello, World!\n");
    return 0;
}
```

### Dicas de Estudo

> **💡 Dica:** Pratique programação todos os dias, mesmo que por 30 minutos **⚠️ Atenção:** Sempre inicializar variáveis antes de usar **🔍 Debug:** Use `printf()` para debugar seus programas

### Armadilhas Comuns

- **Ponteiros não inicializados** → Podem causar segmentation fault
- **Esquecer `&` no `scanf()`** → Erro comum de iniciantes
- **Buffer overflow** → Sempre verificar limites de arrays
- **Memory leaks** → Sempre usar `free()` após `malloc()`

---

## 📊 Cronograma de Estudos

### Semana 1-2: Fundamentos

- [ ] História da linguagem C
- [ ] Configurar ambiente de desenvolvimento
- [ ] Primeiro programa "Hello World"
- [ ] Compilação e execução

### Semana 3-4: Elementos Básicos

- [ ] Tipos de dados e variáveis
- [ ] Operadores e expressões
- [ ] Entrada e saída básica

### Semana 5-6: Controle de Fluxo

- [ ] Estruturas condicionais
- [ ] Loops e iterações
- [ ] Exercícios práticos

### Semana 7-8: Arrays e Strings

- [ ] Vetores unidimensionais
- [ ] Matrizes
- [ ] Manipulação de strings

### Semana 9-10: Funções

- [ ] Definição e chamada de funções
- [ ] Recursão
- [ ] Bibliotecas padrão

### Semana 11-12: Ponteiros

- [ ] Conceitos de memória
- [ ] Operações com ponteiros
- [ ] Relação ponteiros-arrays

### Semana 13-14: Avançado

- [ ] Alocação dinâmica
- [ ] Estruturas (structs)
- [ ] Manipulação de arquivos

### Semana 15-16: Projeto Final

- [ ] Desenvolvimento do projeto
- [ ] Testes e debug
- [ ] Apresentação

---

## 🎯 Metas de Aprendizagem

### Nível Iniciante ✅

- [x] Entender sintaxe básica
- [x] Criar programas simples
- [x] Usar estruturas de controle

### Nível Intermediário 🔄

- [ ] Trabalhar com arrays e strings
- [ ] Implementar funções recursivas
- [ ] Usar ponteiros efetivamente

### Nível Avançado ❌

- [ ] Gerenciar memória dinamicamente
- [ ] Trabalhar com estruturas complexas
- [ ] Manipular arquivos

---

## 📈 Progresso Atual

**Data de Início:** [Data] **Progresso:** ▓▓▓░░░░░░░ 30% **Módulos Concluídos:** 3/10 **Projetos Finalizados:** 1/6

---

## 🏷️ Tags e Referências

#linguagem-c #programacao-estruturada #algoritmos #ponteiros #memoria #arquivos #funcoes #arrays #estruturas-de-dados-basicas #compilacao #debugging

**Última Atualização:** [Data Atual] **Revisão:** Pendente