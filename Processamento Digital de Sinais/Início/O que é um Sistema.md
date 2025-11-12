# ⚙️ Definição de Sistema

> [!cite] “Um **sistema** é um conjunto de elementos que **interagem entre si** a fim de **realizar uma determinada tarefa**.”

---

> [!seealso] ## 🧩 Componentes Fundamentais de um Sistema
>
> Um sistema pode ser entendido como a combinação de **elementos**, **interações** e **objetivos**:
>
> - **Elementos:** partes que trocam informações entre si para atingir o propósito do sistema.  
> - **Conexão:** meios que permitem a comunicação e troca de informação entre elementos.  
> - **Variáveis:** grandezas que armazenam as informações manipuladas pelos elementos.  
> - **Função:** o objetivo ou razão pela qual o sistema existe.
> 
> 💡 *A interação implica a existência de uma informação a ser trocada, a própria troca e a presença de conexões que a possibilitem.*

---

> [!seealso] ## 📦 Diagrama de Caixa Preta (Black Box Diagram)
>
> Um **diagrama de caixa preta** descreve o sistema apenas em termos de:
> - **Entradas:** informações ou sinais que o sistema recebe.
> - **Saídas:** respostas geradas pelo sistema.
>
> 🖼️ *A estrutura interna é ocultada; o foco é na relação entrada → saída.*

---

> [!seealso] ## ⚡ Sistemas Físicos Elétricos
>
> Nos sistemas elétricos, as variáveis e elementos são representações ideais de fenômenos físicos.
>
> - **Variáveis:**  
  > - **Tensão (V):** associada ao acúmulo de carga ou energia potencial.  
  > - **Corrente (I):** associada ao movimento de cargas ou energia cinética.  
  > - A combinação de ambas define **potência (P)** e **energia (E)**.
>
> - **Elementos:**  
  > - Fontes independentes e dependentes.  
  > - **Resistor (R)**, **Capacitor (C)** e **Indutor (L)** (auto ou mútuo).  
  > - Cada elemento é descrito por sua **equação característica** (ex: Lei de Ohm).
>
>- **Conexão:**  
  > - As relações entre elementos seguem as **Leis de Kirchhoff** (tensão e corrente).  
  > - A **Teoria dos Grafos** é usada para modelar matematicamente essas conexões.

---

> [!note] ## 💻 Sistemas Matemáticos Digitais
>
> Nos sistemas digitais, a informação é representada numericamente e processada de forma lógica.
> 
> ![[Pasted image 20251110132756.png]]
>
> - **Variáveis:**  
  > - Quantidades numéricas codificadas em um **sistema de numeração**.  
  > - Normalmente utiliza-se o **Sistema Posicional Binário (base 2)** e **complemento de dois** para representar números.
> 
> - **Elementos:**  
  > - **Portas lógicas** (AND, OR, NOT, XOR, etc.) — blocos fundamentais da lógica clássica.  
  > - A partir delas, formam-se **flip-flops**, **registradores**, **memórias** e outros componentes.
>
> - **Conexão:**  
  > - Definida por **equações lógicas** ou circuitos booleanos.  
  > - Descreve o fluxo e a manipulação da informação entre os elementos.

---

Um **sistema**, em qualquer contexto (físico ou digital), é um **modelo de transformação de informações**:
$$
\text{Entrada} \rightarrow \text{Processamento} \rightarrow \text{Saída}
$$

Essa visão é a base do **Processamento Digital de Sinais**, onde o objetivo é entender e manipular **como os sinais se transformam ao atravessar sistemas**.

---

> [!tip] # 💡 Por que “sistema” é tão importante em Processamento de Sinais?
>
> Porque **tudo o que fazemos em PDS é aplicar sistemas sobre sinais.**
>
> Simples assim. O sinal é a _matéria-prima_, e o sistema é a _ferramenta que o transforma._
>
> ---
> 
> ### 🎛️ 1. O sistema é o que **processa**
> 
Um **sistema** define **como um sinal de entrada é transformado em outro sinal (saída)**.  
> Matematicamente:  
> $$
y(t) = T{x(t)}  
> $$ 
> ou, no caso digital:  
> $$
> y[n] = T{x[n]}  
> $$ 
> onde ( T ) é o **sistema** — o operador que realiza a transformação.
> 
👉 Exemplo:
>
> - Um **filtro passa-baixa** é um sistema que remove altas frequências de um som.
> - Um **compressor de áudio** é um sistema que altera a amplitude do sinal.
> - Um **autotune** é um sistema que ajusta as frequências pra afinar a voz.
>
> Sem o conceito de sistema, PDS vira só uma lista de operações sem estrutura.
>
> ---
>
> ### ⚙️ 2. Sistemas permitem **modelar fenômenos reais**
>
> Um sistema representa **como o mundo físico reage a sinais**.  
Exemplo:
>
> - O ambiente em que você fala atua como um **sistema acústico**: reflete, absorve e distorce o som.
> - Um **microfone** é um sistema que converte pressão sonora em voltagem.
> - O **circuito eletrônico** que amplifica o sinal também é um sistema.
>
> Tudo isso pode ser modelado, simulado e otimizado matematicamente.
>
> ---
>
> ### 📈 3. Sistemas são a base da **análise e do design**
>
> Saber o comportamento de um sistema (ex: resposta em frequência, impulso, fase) permite:
>
> - **Projetar filtros** específicos (equalizadores, canceladores de ruído, etc.);
> - **Prever a saída** para qualquer entrada;
> - **Controlar** o que o sinal ganha, perde ou mantém.
>
> Sem o conceito de sistema, não dá pra falar de Fourier, convolução, nem de resposta em frequência — que são o coração do PDS.
>
> ---
>
> ### 🧩 4. Tudo em PDS é “Sistema + Sinal”
>
> Resumo visual:
>
> | **Componente** | **O que é**                         | **Exemplo**                                     |
> |----------------|-------------------------------------|------------------------------------------------|
> | **Sinal**      | Informação que varia no tempo       | Som, imagem, temperatura                       |
> | **Sistema**    | Processo que transforma o sinal     | Filtro, amplificador, codificador              |
> | **Saída**      | Resultado do processamento          | Som filtrado, imagem limpa, sinal amplificado  |
>
> ---
> 
> ### 🔁 5. Em resumo:
> 
> Um sistema é importante porque ele:
>
> 1. **Processa** sinais (transforma entrada → saída);
> 2. **Modela** o comportamento físico e digital;
> 3. **Permite projetar** e entender transformações matemáticas;
> 4. **É a base de tudo**: sem ele, não há processamento digital de sinais.
> 
> 
> ---

