# Pixels e Matrizes

> [!abstract] Resumo Pixels são a menor unidade de uma imagem digital, e sua representação matemática fundamental é através de matrizes. Cada pixel contém valores numéricos que representam intensidades de cor, organizados em estruturas matriciais que permitem operações matemáticas sofisticadas para processamento e manipulação de imagens.

## Fundamentos da Representação

> [!note] Uma imagem digital é essencialmente uma matriz bidimensional (ou tridimensional, no caso de imagens coloridas) onde cada elemento representa um pixel. A posição de um pixel na imagem corresponde aos índices da matriz.

**Representação Matemática:**

- Imagem em escala de cinza: $I \in \mathbb{R}^{m \times n}$, onde $m$ é a altura e $n$ é a largura
- Cada elemento $I(i,j)$ representa a intensidade luminosa no pixel na posição $(i,j)$
- Valores tipicamente normalizados no intervalo $[0, 255]$ (8 bits) ou $[0, 1]$

## Canais de Cor e Representação Matricial

### Modelo RGB (Red, Green, Blue)

Imagens coloridas são representadas por **três matrizes** simultâneas, uma para cada canal de cor:

$$I_{RGB} = [R, G, B]$$

Onde:

- $R \in \mathbb{R}^{m \times n}$ - Canal vermelho
- $G \in \mathbb{R}^{m \times n}$ - Canal verde
- $B \in \mathbb{R}^{m \times n}$ - Canal azul

Matematicamente, podemos representar como um tensor tridimensional: $I \in \mathbb{R}^{m \times n \times 3}$

### Outros Modelos de Cor

**HSV (Hue, Saturation, Value):**

- Matiz (H): ângulo no espaço de cor $[0°, 360°]$
- Saturação (S): pureza da cor $[0, 1]$
- Valor (V): brilho $[0, 1]$

**CMYK (Cyan, Magenta, Yellow, Key/Black):**

- Usado em impressão
- Tensor $I \in \mathbb{R}^{m \times n \times 4}$

## Operações Matriciais em Processamento de Imagens

### 1. Operações Pontuais (Element-wise)

**Ajuste de Brilho:** $$I_{nova}(i,j) = I(i,j) + \beta$$ onde $\beta$ é o fator de brilho

**Ajuste de Contraste:** $$I_{nova}(i,j) = \alpha \cdot I(i,j)$$ onde $\alpha$ é o fator de contraste

**Negativo da Imagem:** $$I_{negativo}(i,j) = 255 - I(i,j)$$

### 2. Convolução Matricial

A operação mais importante em processamento de imagens. Definida como:

$$(I * K)(i,j) = \sum_{m}\sum_{n} I(i-m, j-n) \cdot K(m,n)$$

Onde $K$ é o **kernel** (matriz de convolução).

**Exemplos de Kernels:**

_Suavização (Blur) - Média 3×3:_ $$K_{blur} = \frac{1}{9}\begin{bmatrix} 1 & 1 & 1 \ 1 & 1 & 1 \ 1 & 1 & 1 \end{bmatrix}$$

_Detecção de Bordas (Sobel horizontal):_ $$K_{sobel_x} = \begin{bmatrix} -1 & 0 & 1 \ -2 & 0 & 2 \ -1 & 0 & 1 \end{bmatrix}$$

_Nitidez (Sharpening):_ $$K_{sharp} = \begin{bmatrix} 0 & -1 & 0 \ -1 & 5 & -1 \ 0 & -1 & 0 \end{bmatrix}$$

### 3. Transformações Geométricas

**Translação:** $$\begin{bmatrix} x' \ y' \ 1 \end{bmatrix} = \begin{bmatrix} 1 & 0 & t_x \ 0 & 1 & t_y \ 0 & 0 & 1 \end{bmatrix} \begin{bmatrix} x \ y \ 1 \end{bmatrix}$$

**Rotação (ângulo $\theta$):** $$\begin{bmatrix} x' \ y' \ 1 \end{bmatrix} = \begin{bmatrix} \cos\theta & -\sin\theta & 0 \ \sin\theta & \cos\theta & 0 \ 0 & 0 & 1 \end{bmatrix} \begin{bmatrix} x \ y \ 1 \end{bmatrix}$$

**Escalonamento:** $$\begin{bmatrix} x' \ y' \ 1 \end{bmatrix} = \begin{bmatrix} s_x & 0 & 0 \ 0 & s_y & 0 \ 0 & 0 & 1 \end{bmatrix} \begin{bmatrix} x \ y \ 1 \end{bmatrix}$$

### 4. Operações Morfológicas

Baseadas em teoria de conjuntos e álgebra de matrizes:

**Dilatação:** $$I \oplus B = {z | (\hat{B})_z \cap I \neq \emptyset}$$

**Erosão:** $$I \ominus B = {z | B_z \subseteq I}$$

Onde $B$ é o elemento estruturante (matriz binária).

### 5. Transformações no Espaço de Frequência

**Transformada de Fourier 2D:** $$F(u,v) = \sum_{x=0}^{M-1}\sum_{y=0}^{N-1} I(x,y) \cdot e^{-j2\pi(\frac{ux}{M}+\frac{vy}{N})}$$

Permite filtragem no domínio da frequência através de multiplicação matricial.

## Conceitos Matemáticos Importantes

### 1. Produto Interno e Normas

**Norma Euclidiana** (usada para medir diferenças entre imagens): $$||I||_2 = \sqrt{\sum_{i,j} I(i,j)^2}$$

**Produto Interno** (similaridade entre imagens): $$\langle I_1, I_2 \rangle = \sum_{i,j} I_1(i,j) \cdot I_2(i,j)$$

### 2. Decomposição em Valores Singulares (SVD)

Para uma matriz imagem $I$: $$I = U\Sigma V^T$$

Aplicações:

- Compressão de imagens
- Redução de ruído
- Extração de características

### 3. Álgebra Linear Aplicada

**Vetorização:** Transformar matriz $I_{m \times n}$ em vetor $\vec{v} \in \mathbb{R}^{mn}$

**Produto de Kronecker:** Usado em operações de convolução eficientes

**Autovalores e Autovetores:** Análise de componentes principais (PCA) em imagens

## Operações entre Canais de Cor

### Conversão RGB para Escala de Cinza

$$I_{gray} = 0.299 \cdot R + 0.587 \cdot G + 0.114 \cdot B$$

Os pesos refletem a sensibilidade do olho humano a diferentes cores.

### Combinações de Canais

**Adição de canais:** $$I_{combined} = \alpha R + \beta G + \gamma B$$

**Separação de canais:** Análise independente de cada matriz de canal para processamento específico.

## Aplicações Práticas

1. **Filtros de Instagram/Photoshop**: Combinação de operações matriciais e ajustes de canais
2. **Compressão JPEG**: Transformada de cosseno discreta (DCT) - operação matricial
3. **Reconhecimento Facial**: Eigenfaces usando PCA (álgebra linear)
4. **Inteligência Artificial**: Redes convolucionais (CNN) - múltiplas convoluções matriciais
5. **Realidade Aumentada**: Transformações geométricas em tempo real

próximos: [[Relações Entre Pixels]]

