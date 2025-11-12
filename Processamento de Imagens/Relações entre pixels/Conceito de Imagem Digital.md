# Conceito de Imagem Digital

> [!abstract] Definição Fundamental Uma imagem digital é uma representação discreta e quantizada de uma cena visual contínua do mundo real, matematicamente expressa como uma função bidimensional $f(x,y)$ onde $(x,y)$ são coordenadas espaciais discretas e $f$ representa a amplitude (intensidade ou cor) naquele ponto. O valor de $f$ em qualquer par de coordenadas é uma quantidade finita e discreta, resultado de dois processos fundamentais: amostragem espacial e quantização de amplitude.

## A Natureza Discreta da Imagem Digital

A principal característica que define uma imagem digital é sua natureza discreta, em contraste radical com as imagens analógicas. Uma imagem analógica é contínua tanto em coordenadas espaciais quanto em amplitude, podendo ser representada matematicamente como $f_{continua}(x,y)$ onde $x,y \in \mathbb{R}^2$. Já a imagem digital é definida sobre um domínio discreto finito:

$$f(x,y), \quad x \in {0,1,2,...,M-1}, \quad y \in {0,1,2,...,N-1}$$

onde $M$ e $N$ definem as dimensões da imagem em pixels. Cada par ordenado $(x,y)$ identifica unicamente um elemento da imagem chamado pixel (picture element). O valor total de pixels em uma imagem é dado pelo produto $M \times N$.

> [!info] Amostragem Espacial A amostragem espacial transforma uma função contínua em uma função discreta através da multiplicação com uma função de amostragem. Matematicamente, isso pode ser expresso como: $$f_d(x,y) = f_c(x,y) \cdot s(x,y)$$ onde $f_c(x,y)$ é a função contínua original e $s(x,y)$ é a função de amostragem, tipicamente modelada como: $$s(x,y) = \sum_{i=0}^{M-1}\sum_{j=0}^{N-1}\delta(x-i\Delta x, y-j\Delta y)$$ Aqui, $\delta$ representa a função impulso de Dirac, e $\Delta x$ e $\Delta y$ são os intervalos de amostragem espacial. Quanto menores esses intervalos, maior a densidade de amostragem e, consequentemente, maior a resolução espacial da imagem resultante.

## Quantização de Intensidade

Após discretizar o espaço, precisamos discretizar também as amplitudes. No mundo físico, a intensidade luminosa é uma grandeza contínua que pode assumir infinitos valores dentro de um intervalo. A quantização mapeia esse intervalo contínuo $[I_{min}, I_{max}]$ em um conjunto finito de $L$ níveis discretos. Para imagens digitais, tipicamente usamos:

$$L = 2^b$$

onde $b$ é o número de bits usados para representar cada pixel. O processo de quantização pode ser expresso como:

$$f_q = Q[f_d] = \text{round}\left(\frac{f_d - I_{min}}{I_{max} - I_{min}} \cdot (L-1)\right)$$

Esta operação introduz um erro de quantização $e_q = f_d - f_q$, que é limitado a $\pm\frac{1}{2}$ do intervalo de quantização.

> [!warning] Teorema da Amostragem de Nyquist-Shannon Este teorema estabelece que para reconstruir perfeitamente um sinal contínuo a partir de suas amostras, a frequência de amostragem $f_s$ deve satisfazer: $$f_s \geq 2f_{max}$$ onde $f_{max}$ é a frequência máxima presente no sinal original. Em imagens bidimensionais, isso se aplica independentemente em cada direção espacial. Quando violamos esta condição ($f_s < 2f_{max}$), ocorre aliasing, criando artefatos visuais. A frequência mínima $f_{Nyquist} = 2f_{max}$ é chamada frequência de Nyquist.

## Profundidade de Bits e Faixa Dinâmica

A profundidade de bits $b$ determina quantos níveis distintos de intensidade podem ser representados. A relação fundamental é $L = 2^b$, onde algumas profundidades comuns incluem:

Para $b = 1$: $L = 2^1 = 2$ níveis (imagem binária pura) Para $b = 8$: $L = 2^8 = 256$ níveis (padrão para cada canal RGB) Para $b = 16$: $L = 2^{16} = 65536$ níveis (imagens médicas e fotografia profissional)

A faixa dinâmica de uma imagem é definida como a razão entre a maior e a menor intensidade mensurável:

$$D = \frac{I_{max}}{I_{min}}$$

Frequentemente expressa em decibéis:

$$D_{dB} = 20\log_{10}\left(\frac{I_{max}}{I_{min}}\right)$$

Uma câmera com sensor de 12 bits possui faixa dinâmica teórica de $20\log_{10}(4096) \approx 72$ dB.

> [!important] Representação Numérica Normalizada Embora internamente as imagens sejam armazenadas como inteiros (0-255 para 8 bits), em processamento matemático frequentemente normalizamos os valores para o intervalo $[0,1]$: $$I_{norm}(x,y) = \frac{I(x,y)}{L-1}$$ Para imagens de 8 bits, isso significa $I_{norm} = I/255$. Esta normalização simplifica muitas operações matemáticas e torna algoritmos independentes da profundidade de bits específica. Para reverter: $I(x,y) = \text{round}(I_{norm}(x,y) \cdot (L-1))$.

## Função Imagem e Domínio Matemático

Uma imagem digital monocromática é formalmente definida como uma função:

$$f: D \rightarrow R$$

onde o domínio $D = {0,1,...,M-1} \times {0,1,...,N-1} \subset \mathbb{Z}^2$ é o conjunto de coordenadas espaciais discretas, e o contradomínio $R = {0,1,...,L-1} \subset \mathbb{Z}$ é o conjunto de intensidades possíveis. Esta função mapeia cada posição espacial para um valor de intensidade único.

A imagem completa pode ser representada como uma matriz:

$$\mathbf{I} = \begin{bmatrix} f(0,0) & f(0,1) & \cdots & f(0,N-1) \ f(1,0) & f(1,1) & \cdots & f(1,N-1) \ \vdots & \vdots & \ddots & \vdots \ f(M-1,0) & f(M-1,1) & \cdots & f(M-1,N-1) \end{bmatrix}$$

Esta representação matricial é fundamental pois permite aplicar toda a álgebra linear ao processamento de imagens.

> [!example] Imagens Coloridas e Estrutura Tensorial Para imagens coloridas no modelo RGB, expandimos nossa função para três componentes: $$\mathbf{f}(x,y) = [f_R(x,y), f_G(x,y), f_B(x,y)]^T$$ onde cada componente é uma função independente $f_c: D \rightarrow R$ para $c \in {R,G,B}$. Matematicamente, isso forma um tensor de terceira ordem: $$\mathbf{I} \in \mathbb{Z}^{M \times N \times 3}$$ O tamanho total em memória (bytes) para uma imagem RGB de 8 bits é calculado por: $$S_{bytes} = M \times N \times 3 \times \frac{b}{8} = M \times N \times 3 \quad \text{(para b=8)}$$ Por exemplo, uma imagem Full HD (1920×1080) ocupa: $1920 \times 1080 \times 3 = 6.220.800$ bytes $\approx 6.2$ MB sem compressão.

## Resolução Espacial e Densidade de Pixels

A resolução espacial quantifica a capacidade de distinguir detalhes finos em uma imagem. Ela depende não apenas do número total de pixels, mas também da densidade de pixels por unidade física. A densidade de pixels (pixel density) é expressa como:

$$\rho = \frac{N_{pixels}}{d}$$

onde $N_{pixels}$ é o número de pixels ao longo de uma dimensão e $d$ é a distância física. As unidades comuns são PPI (pixels per inch) ou PPmm (pixels per millimeter). Para uma tela com diagonal de $D$ polegadas e resolução $M \times N$:

$$\rho_{diagonal} = \frac{\sqrt{M^2 + N^2}}{D}$$

Por exemplo, um smartphone com tela de 6 polegadas e resolução 2340×1080 tem:

$$\rho = \frac{\sqrt{2340^2 + 1080^2}}{6} = \frac{\sqrt{5.475.600 + 1.166.400}}{6} = \frac{2575.3}{6} \approx 429 \text{ PPI}$$

> [!note] Critério de Nyquist para Imagens Em processamento de imagens, a frequência espacial máxima representável sem aliasing é: $$f_{max} = \frac{1}{2\Delta x}$$ onde $\Delta x$ é o espaçamento entre pixels. Frequências espaciais acima deste limite causarão aliasing. Para uma imagem com pixels separados por 1mm, a frequência máxima representável é 0.5 ciclos/mm. Este princípio explica por que padrões finos (como tecidos listrados ou grades) frequentemente aparecem distorcidos em fotografias digitais quando sua frequência espacial excede o limite de Nyquist do sensor.

## Modelos de Cor e Transformações

O modelo RGB representa cores como combinações lineares de três primárias aditivas. Matematicamente, qualquer cor pode ser expressa como:

$$\mathbf{C} = \begin{bmatrix} R \ G \ B \end{bmatrix}, \quad R,G,B \in [0, L-1]$$

Para converter RGB em escala de cinza, utilizamos uma média ponderada baseada na sensibilidade do olho humano:

$$I_{gray} = 0.299R + 0.587G + 0.114B$$

Esta ponderação vem dos padrões de transmissão de TV (ITU-R BT.601). A conversão pode ser expressa matricialmente:

$$I_{gray} = \begin{bmatrix} 0.299 & 0.587 & 0.114 \end{bmatrix} \begin{bmatrix} R \ G \ B \end{bmatrix}$$

> [!tip] Espaço de Cor HSV O modelo HSV (Hue, Saturation, Value) oferece uma representação mais intuitiva. A conversão de RGB para HSV envolve: $$V = \max(R,G,B)$$ $$S = \begin{cases} 0 & \text{se } V = 0 \ \frac{V - \min(R,G,B)}{V} & \text{caso contrário} \end{cases}$$ $$H = \begin{cases} 60° \times \frac{G-B}{V-\min(R,G,B)} & \text{se } V = R \ 60° \times (2 + \frac{B-R}{V-\min(R,G,B)}) & \text{se } V = G \ 60° \times (4 + \frac{R-G}{V-\min(R,G,B)}) & \text{se } V = B \end{cases}$$ onde $H \in [0°, 360°]$, $S \in [0,1]$ e $V \in [0,1]$. Este modelo separa informação cromática (H,S) da informação de luminância (V), facilitando operações como ajuste de brilho sem alterar a tonalidade.

## Vizinhança e Conectividade

Em processamento de imagens, frequentemente precisamos considerar pixels vizinhos. Para um pixel $p$ na posição $(x,y)$, definimos diferentes tipos de vizinhança:

**Vizinhança-4 (conectividade vertical e horizontal):** $$N_4(p) = {(x\pm 1, y), (x, y\pm 1)}$$

Esta vizinhança contém 4 pixels adjacentes nas direções cardeais. A distância entre $p$ e seus vizinhos em $N_4$ é chamada distância city-block ou $L_1$:

$$D_4(p,q) = |x_p - x_q| + |y_p - y_q|$$

**Vizinhança-8 (inclui diagonais):** $$N_8(p) = N_4(p) \cup {(x\pm 1, y\pm 1)}$$

Contém os 8 pixels que cercam $p$ completamente. A distância correspondente é a distância de Chebyshev ou $L_\infty$:

$$D_8(p,q) = \max(|x_p - x_q|, |y_p - y_q|)$$

**Distância Euclidiana:** $$D_E(p,q) = \sqrt{(x_p - x_q)^2 + (y_p - y_q)^2}$$

Esta é a distância geométrica tradicional entre dois pontos no plano, usada em muitos algoritmos de processamento de imagens.

> [!info] Conectividade e Componentes Conexas Dois pixels $p$ e $q$ são conectados se existe um caminho de pixels adjacentes ligando-os. Para imagens binárias, definimos componentes conexas como conjuntos maximais de pixels conectados com mesmo valor. O número de componentes conexas $C$ em uma imagem binária é uma característica topológica importante, calculada através de algoritmos como flood-fill ou union-find. A relação de Euler para imagens digitais relaciona componentes conexas $C$ com buracos $H$: $$\chi = C - H$$ onde $\chi$ é a característica de Euler da imagem.

## Histograma e Estatísticas de Imagem

O histograma de uma imagem digital é uma função que mapeia cada nível de intensidade para sua frequência de ocorrência:

$$h(r_k) = n_k$$

onde $r_k$ é o $k$-ésimo nível de intensidade ($k = 0,1,...,L-1$) e $n_k$ é o número de pixels com intensidade $r_k$. A soma de todos os valores do histograma é igual ao número total de pixels:

$$\sum_{k=0}^{L-1} h(r_k) = M \times N$$

O histograma normalizado, que representa a probabilidade de cada intensidade, é:

$$p(r_k) = \frac{h(r_k)}{M \times N} = \frac{n_k}{M \times N}$$

onde $\sum_{k=0}^{L-1} p(r_k) = 1$.

> [!example] Momentos Estatísticos da Imagem Usando o histograma, podemos calcular diversos momentos estatísticos que caracterizam a distribuição de intensidades:
> 
> **Média (primeiro momento):** $$\mu = \sum_{k=0}^{L-1} r_k \cdot p(r_k) = \frac{1}{MN}\sum_{x=0}^{M-1}\sum_{y=0}^{N-1}f(x,y)$$
> 
> **Variância (segundo momento central):** $$\sigma^2 = \sum_{k=0}^{L-1} (r_k - \mu)^2 \cdot p(r_k)$$
> 
> **Desvio padrão:** $$\sigma = \sqrt{\sigma^2}$$
> 
> **Assimetria (terceiro momento normalizado):** $$\gamma_1 = \frac{1}{\sigma^3}\sum_{k=0}^{L-1}(r_k - \mu)^3 \cdot p(r_k)$$
> 
> **Curtose (quarto momento normalizado):** $$\gamma_2 = \frac{1}{\sigma^4}\sum_{k=0}^{L-1}(r_k - \mu)^4 \cdot p(r_k) - 3$$
> 
> Estes momentos fornecem informação quantitativa sobre brilho médio ($\mu$), contraste ($\sigma$), distribuição assimétrica de intensidades ($\gamma_1$) e presença de valores extremos ($\gamma_2$).

## Entropia e Conteúdo Informacional

A entropia de Shannon mede o conteúdo informacional de uma imagem. Para uma imagem com distribuição de probabilidade $p(r_k)$, a entropia é definida como:

$$H = -\sum_{k=0}^{L-1} p(r_k) \log_2 p(r_k)$$

medida em bits por pixel. A entropia máxima ocorre quando todos os níveis de intensidade são equiprováveis:

$$H_{max} = \log_2 L = b$$

Uma imagem com entropia próxima de $H_{max}$ contém muita informação e é difícil de comprimir. Imagens com baixa entropia (como áreas uniformes) comprimem facilmente. A redundância relativa da imagem é:

$$R = 1 - \frac{H}{H_{max}}$$

Valores altos de $R$ indicam alta redundância e bom potencial de compressão.

próximo: [[Pixels e Matrizes]]