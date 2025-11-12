
> [!note] Para Iniciantes Absolutos
> Se você está começando na programação, pense em um programa de computador como uma receita de bolo. Assim como uma receita tem ingredientes e passos para fazer o bolo, um programa em C tem elementos que dizem ao computador o que fazer, passo a passo.

## Seu Primeiro Programa: "Hello World"

> [!example] Vamos Começar pelo Básico
> 
> ```c
> #include <stdio.h>
> 
> int main() {
>     printf("Hello, World!\n");
>     return 0;
> }
> ```
> 
> **Saída:**
> ```
> Hello, World!
> ```

Vamos entender cada parte desse programa, peça por peça:

## 1. Incluindo Bibliotecas (`#include`)

> [!info] As "Ferramentas" do Nosso Programa
> 
> ```c
> #include <stdio.h>
> ```
> 
> **O que isso significa?**
> - `#include` significa "inclua" ou "adicione"
> - `<stdio.h>` é uma **biblioteca padrão** que contém funções para entrada e saída
> - É como pegar ferramentas da nossa caixa de ferramentas antes de começar um trabalho

**Pense assim:** Se você fosse fazer um bolo, primeiro pegaria os utensílios necessários (forma, batedeira, etc.). O `#include` é exatamente isso!

## 2. A Função Principal (`main`)

> [!important] O Coração do Programa
> 
> ```c
> int main() {
>     // O código vai aqui
> }
> ```
> 
> **Explicação simples:**
> - `int` significa que a função retorna um número inteiro
> - `main` é o nome da função **principal** - todo programa em C precisa dela
> - `()` significa que a função não recebe nenhum parâmetro
> - `{ }` são chaves que delimitam onde o código da função começa e termina

**Analogia:** A função `main` é como a receita principal do bolo. O computador sempre começa executando o que está dentro dela.

## 3. Escrevendo na Tela (`printf`)

> [!tip] Mostrando Mensagens
> 
> ```c
> printf("Hello, World!\n");
> ```
> 
> **Parte por parte:**
> - `printf` significa "print formatado" - é como dizer "mostre na tela"
> - `("Hello, World!\n")` é o texto que queremos mostrar
> - `\n` é um **caractere especial** que significa "pule para a próxima linha"
> - `;` é o **ponto e vírgula** - em C, quase toda linha termina com ele

**Importante:** O `\n` no final faz com que depois de mostrar "Hello, World!", o cursor vá para a linha de baixo. Sem ele, a próxima coisa que apareceria ficaria colada.

## 4. Retornando um Valor (`return`)

> [!abstract] Indicando que Terminou
> 
> ```c
> return 0;
> ```
> 
> **O que isso faz?**
> - `return` significa "retorne" ou "devolva"
> - `0` significa "sucesso" - está dizendo ao sistema operacional que o programa terminou corretamente
> - Se algo der errado, poderíamos retornar um número diferente de zero

## Estrutura Completa Explicada

> [!code] Programa Completo com Comentários
> 
> ```c
> // Inclui a biblioteca para entrada/saída
> // (como pegar ferramentas da caixa)
> #include <stdio.h>
> 
> // Função principal - onde o programa começa
> // (a receita principal do bolo)
> int main() {
>     
>     // Mostra "Hello, World!" na tela
>     // e pula para a próxima linha (\n)
>     printf("Hello, World!\n");
>     
>     // Diz que o programa terminou com sucesso
>     return 0;
>     
> } // Fim da função main
> ```

## Variações do Hello World

> [!example] Diferentes Maneiras de Escrever
> 
> ```c
> // Versão 1 - Básica (que já vimos)
> #include <stdio.h>
> int main() {
>     printf("Hello, World!\n");
>     return 0;
> }
> 
> // Versão 2 - Com múltiplas mensagens
> #include <stdio.h>
> int main() {
>     printf("Olá, mundo!\n");
>     printf("Este é meu primeiro programa.\n");
>     printf("Estou aprendendo C!\n");
>     return 0;
> }
> 
> // Versão 3 - Mensagem na mesma linha
> #include <stdio.h>
> int main() {
>     printf("Primeira parte ");
>     printf("Segunda parte\n");  // Só o último tem \n
>     return 0;
> }
> ```

## Passo a Passo para Criar e Executar

> [!success] Como Fazer Funcionar na Prática
> 
> **1. Escreva o código:**
> - Abra um editor de texto (Bloco de Notas, VS Code, etc.)
> - Digite o programa Hello World
> - Salve como `hello.c`
> 
> **2. Compile (transforme em executável):**
> ```bash
> gcc hello.c -o hello
> ```
> 
> **3. Execute (rode o programa):**
> ```bash
> ./hello    # No Linux/Mac
> hello.exe  # No Windows
> ```

## Erros Comuns de Iniciantes

> [!warning] Cuidado com Esses Detalhes!
> 
> ```c
> // ERRADO - esqueceu o ponto e vírgula
> #include <stdio.h>
> int main() {
>     printf("Hello, World!\n")  // FALTANDO ;
>     return 0;
> }
> 
> // ERRADO - esqueceu as chaves
> #include <stdio.h>
> int main() 
>     printf("Hello, World!\n");  // FALTANDO { }
>     return 0;
> 
> // ERRADO - escreveu errado
> #include <stdio.h>
> int main() {
>     print("Hello, World!\n");  // É printf, não print!
>     return 0;
> }
> ```

## Próximos Passos

> [!tip] O que Aprender Depois
> Depois de entender a estrutura básica, você pode:
> 
> 1. **Variáveis** - Para guardar informações
> 2. **Entrada de dados** - Para o usuário digitar coisas
> 3. **Cálculos** - Para fazer operações matemáticas
> 4. **Decisões** - Para o programa tomar diferentes caminhos

> [!note] Resumo Final
> - **`#include <stdio.h>`** = Pegue as ferramentas de entrada/saída
> - **`int main()`** = Esta é a função principal onde tudo começa  
> - **`{ }`** = Tudo dentro das chaves é o "corpo" do programa
> - **`printf()`** = Use para mostrar coisas na tela
> - **`\n`** = Pule para a próxima linha
> - **`return 0;`** = Programa terminou com sucesso
> - **`;`** = Não esqueça do ponto e vírgula no final das linhas!

**Parabéns!** Você acabou de entender a estrutura básica de qualquer programa em C. Agora você tem a fundação para aprender conceitos mais avançados! 🎉

Próximo: [[C - Processo de Compilação e Linkagem]]