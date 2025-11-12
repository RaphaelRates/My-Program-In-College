# C - Constantes e Diretivas de Pré-processador

> [!note] O que é o Pré-processador?
> Antes do compilador transformar seu código em programa, o **pré-processador** age como um "assistente inteligente" que prepara o código. Ele processa todas as linhas que começam com `#` (sustenido).

## O Básico do `#define`

> [!example] Constantes Simples
> 
> ```c
> #include <stdio.h>
> 
> // Definindo constantes
> #define PI 3.14159
> #define TAXA_JUROS 0.05
> #define NOME_EMPRESA "Tech Solutions"
> #define ANO_ATUAL 2024
> 
> int main() {
>     float raio = 5.0;
>     float area = PI * raio * raio;
>     
>     printf("Empresa: %s\n", NOME_EMPRESA);
>     printf("Área do círculo: %.2f\n", area);
>     printf("Taxa de juros: %.1f%%\n", TAXA_JUROS * 100);
>     
>     return 0;
> }
> ```
> 
> **Saída:**
> ```
> Empresa: Tech Solutions
> Área do círculo: 78.54
> Taxa de juros: 5.0%
> ```

### Por que usar `#define`?

> [!tip] Vantagens das Constantes
> 
> ```c
> #include <stdio.h>
> 
> // ❌ SEM CONSTANTES (ruim)
> // float area = 3.14159 * raio * raio;
> // float perimetro = 2 * 3.14159 * raio;
> 
> // ✅ COM CONSTANTES (bom)
> #define PI 3.14159
> #define DIAS_SEMANA 7
> #define LIMITE_IDADE 18
> 
> int main() {
>     float raio = 3.0;
>     
>     // Código mais legível e fácil de manter
>     float area = PI * raio * raio;
>     float perimetro = 2 * PI * raio;
>     
>     printf("Área: %.2f\n", area);
>     printf("Perímetro: %.2f\n", perimetro);
>     
>     // Se precisar mudar o valor, mudo só no #define!
>     return 0;
> }
> ```

## Macros com Parâmetros

> [!info] Criando "Funções" com `#define`
> 
> ```c
> #include <stdio.h>
> 
> // Macros com parâmetros
> #define QUADRADO(x) ((x) * (x))
> #define MAXIMO(a, b) ((a) > (b) ? (a) : (b))
> #define E_PAR(n) ((n) % 2 == 0)
> 
> int main() {
>     int numero = 5;
>     int a = 10, b = 20;
>     
>     printf("Quadrado de %d: %d\n", numero, QUADRADO(numero));
>     printf("Máximo entre %d e %d: %d\n", a, b, MAXIMO(a, b));
>     printf("%d é par? %s\n", numero, E_PAR(numero) ? "Sim" : "Não");
>     
>     // Macros podem ser usadas em expressões
>     int resultado = QUADRADO(3) + QUADRADO(4);
>     printf("3² + 4² = %d\n", resultado);
>     
>     return 0;
> }
> ```
> 
> **Saída:**
> ```
> Quadrado de 5: 25
> Máximo entre 10 e 20: 20
> 5 é par? Não
> 3² + 4² = 25
> ```

### Cuidado com Macros!

> [!warning] Armadilhas Comuns
> 
> ```c
> #include <stdio.h>
> 
> // ❌ MACRO PERIGOSA
> #define QUADRADO_RUIM(x) x * x
> 
> // ✅ MACRO CORRETA  
> #define QUADRADO_BOM(x) ((x) * (x))
> 
> int main() {
>     int valor = 5;
>     
>     // Funciona bem em casos simples
>     printf("Quadrado ruim de %d: %d\n", valor, QUADRADO_RUIM(valor));
>     printf("Quadrado bom de %d: %d\n", valor, QUADRADO_BOM(valor));
>     
>     // Problema com expressões!
>     printf("Quadrado ruim de %d: %d\n", valor + 1, QUADRADO_RUIM(valor + 1));
>     printf("Quadrado bom de %d: %d\n", valor + 1, QUADRADO_BOM(valor + 1));
>     
>     return 0;
> }
> ```
> 
> **Saída:**
> ```
> Quadrado ruim de 5: 25
> Quadrado bom de 5: 25
> Quadrado ruim de 6: 11    // 😱 ERRADO!
> Quadrado bom de 6: 36     // ✅ CORRETO
> ```
> 
> **Por que deu errado?**
> `QUADRADO_RUIM(valor + 1)` vira `valor + 1 * valor + 1` = `5 + 5 + 1` = 11

## Diretivas Úteis do Pré-processador

> [!abstract] Controle de Compilação
> 
> ```c
> #include <stdio.h>
> 
> // Define o modo de debug
> #define DEBUG 1
> #define VERSAO "1.0.0"
> 
> int main() {
>     // Compilação condicional
>     #if DEBUG
>         printf("🚧 MODO DEBUG ATIVADO\n");
>         printf("Versão: %s\n", VERSAO);
>     #endif
>     
>     printf("Programa em execução...\n");
>     
>     #ifdef VERSAO
>         printf("Compilado com versão definida\n");
>     #else
>         printf("Compilado sem versão definida\n");
>     #endif
>     
>     return 0;
> }
> ```

### Exemplo Prático: Configurações

> [!code] Sistema de Configuração
> 
> ```c
> #include <stdio.h>
> 
> // Configurações do sistema
> #define VERSAO "2.1.0"
> #define MAX_USUARIOS 100
> #define TIMEOUT 30
> #define LINGUAGEM "pt_BR"
> 
> // Features opcionais
> #define FEATURE_LOG 1
> #define FEATURE_BACKUP 0
> 
> int main() {
>     printf("=== SISTEMA CONFIGURADO ===\n");
>     printf("Versão: %s\n", VERSAO);
>     printf("Máx. usuários: %d\n", MAX_USUARIOS);
>     printf("Timeout: %d segundos\n", TIMEOUT);
>     printf("Linguagem: %s\n", LINGUAGEM);
>     
>     #if FEATURE_LOG
>         printf("📝 Sistema de log: ATIVADO\n");
>     #else
>         printf("📝 Sistema de log: DESATIVADO\n");
>     #endif
>     
>     #if FEATURE_BACKUP
>         printf("💾 Backup automático: ATIVADO\n");
>     #else
>         printf("💾 Backup automático: DESATIVADO\n");
>     #endif
>     
>     return 0;
> }
> ```
> 
> **Saída:**
> ```
> === SISTEMA CONFIGURADO ===
> Versão: 2.1.0
> Máx. usuários: 100
> Timeout: 30 segundos
> Linguagem: pt_BR
> 📝 Sistema de log: ATIVADO
> 💾 Backup automático: DESATIVADO
> ```

## Macros para Código Mais Limpo

> [!tip] Simplificando Código Repetitivo
> 
> ```c
> #include <stdio.h>
> 
> // Macros para verificação
> #define VERIFICAR_NULL(ptr) if ((ptr) == NULL) { \
>     printf("Erro: ponteiro nulo!\n"); \
>     return -1; \
> }
> 
> #define VERIFICAR_NEGATIVO(valor) if ((valor) < 0) { \
>     printf("Erro: valor negativo!\n"); \
>     return -1; \
> }
> 
> // Macro para debug
> #ifdef DEBUG
>     #define DEBUG_LOG(mensagem) printf("DEBUG: %s\n", mensagem)
> #else
>     #define DEBUG_LOG(mensagem) // Vazia em produção
> #endif
> 
> int processar_dados(int *dados, int tamanho) {
>     VERIFICAR_NULL(dados);
>     VERIFICAR_NEGATIVO(tamanho);
>     
>     DEBUG_LOG("Iniciando processamento");
>     
>     // Processamento normal...
>     printf("Processando %d elementos\n", tamanho);
>     
>     DEBUG_LOG("Processamento concluído");
>     return 0;
> }
> 
> int main() {
>     int array[5] = {1, 2, 3, 4, 5};
>     
>     processar_dados(array, 5);
>     // processar_dados(NULL, 5);      // Isso geraria erro
>     // processar_dados(array, -1);    // Isso também geraria erro
>     
>     return 0;
> }
> ```

## Constantes vs Variáveis Const

> [!important] Diferenças Importantes
> 
> ```c
> #include <stdio.h>
> 
> // CONSTANTE do pré-processador
> #define PI_DEFINE 3.14159
> 
> // Variável const (constante em tempo de execução)
> const float PI_CONST = 3.14159;
> 
> int main() {
>     // #define - substituição textual
>     float area1 = PI_DEFINE * 5 * 5;
>     
>     // const - variável de verdade, mas read-only
>     float area2 = PI_CONST * 5 * 5;
>     
>     printf("Área com #define: %.2f\n", area1);
>     printf("Área com const: %.2f\n", area2);
>     
>     // Diferenças:
>     printf("Tipo de PI_DEFINE: o pré-processador remove!\n");
>     printf("Tipo de PI_CONST: %zu bytes\n", sizeof(PI_CONST));
>     printf("Endereço de PI_CONST: %p\n", (void*)&PI_CONST);
>     
>     // PI_CONST = 4.0;  // ❌ ERRO: assignment of read-only variable
>     
>     return 0;
> }
> ```

## Exemplo do Mundo Real

> [!success] Sistema de Mensagens
> 
> ```c
> #include <stdio.h>
> 
> // Códigos de erro
> #define SUCESSO 0
> #define ERRO_ARQUIVO 1
> #define ERRO_MEMORIA 2
> #define ERRO_REDE 3
> 
> // Configurações
> #define MAX_TENTATIVAS 3
> #define TIMEOUT_CONEXAO 5000  // 5 segundos
> #define TAMANHO_BUFFER 1024
> 
> // Mensagens do sistema
> #define MSG_SUCESSO "Operação concluída com sucesso!"
> #define MSG_ERRO_ARQUIVO "Erro: não foi possível abrir o arquivo"
> #define MSG_ERRO_MEMORIA "Erro: memória insuficiente"
> 
> int realizar_operacao(int tipo) {
>     #ifdef DEBUG
>         printf("🔧 Debug: Iniciando operação tipo %d\n", tipo);
>     #endif
>     
>     if (tipo == 0) {
>         printf("✅ %s\n", MSG_SUCESSO);
>         return SUCESSO;
>     } else if (tipo == 1) {
>         printf("❌ %s\n", MSG_ERRO_ARQUIVO);
>         return ERRO_ARQUIVO;
>     } else {
>         printf("❌ %s\n", MSG_ERRO_MEMORIA);
>         return ERRO_MEMORIA;
>     }
> }
> 
> int main() {
>     printf("=== SISTEMA INICIADO ===\n");
>     printf("Tentativas máximas: %d\n", MAX_TENTATIVAS);
>     printf("Timeout: %d ms\n", TIMEOUT_CONEXAO);
>     printf("Buffer: %d bytes\n", TAMANHO_BUFFER);
>     printf("\n");
>     
>     // Testando diferentes cenários
>     realizar_operacao(0);
>     realizar_operacao(1);
>     realizar_operacao(2);
>     
>     return 0;
> }
> ```

> [!note] Resumo Final
> **`#define`** - Cria constantes e macros que o pré-processador substitui antes da compilação
> 
> **Vantagens:**
> - Código mais legível
> - Fácil manutenção (muda em um lugar só)
> - Compilação condicional
> - Elimina "números mágicos" no código
> 
> **Cuidados:**
> - Use parênteses em macros com parâmetros
> - Macros não têm verificação de tipo
> - Podem gerar código inesperado se mal escritas
> 
> **Use quando:**
> - Valores que nunca mudam
> - Configurações do sistema
> - Código condicional para debug/diferentes plataformas
> - Simplificar padrões repetitivos

**Próximo**: [[C - Operadores]]

