# Operação de Multiplicação em Processamento de Imagens

> [!NOTE] Conceito Fundamental
> A **multiplicação de imagens** envolve multiplicar pixels ou aplicar escalares para modificar propriedades da imagem. Existem duas abordagens principais:
> - **Pixel-wise**: $C(x,y) = A(x,y) \times B(x,y)$
> - **Escalar**: $C(x,y) = \alpha \times A(x,y)$

## 📋 Tipos de Multiplicação

### Multiplicação Pixel a Pixel

> [!ABSTRACT] Características
> - Cada pixel da imagem A é multiplicado pelo pixel correspondente da imagem B
> - Requer imagens de mesma dimensão
> - Útil para aplicação de máscaras e filtros

### Multiplicação por Escalar

> [!ABSTRACT] Características
> - Todos os pixels são multiplicados por um valor constante
> - Altera brilho, contraste e outras propriedades globais
> - Mais simples computacionalmente

---

## 🔧 Implementação e Controles

### Gerenciamento de Faixa Dinâmica

> [!CAUTION] Problema de Saturação
> ```python
> # Problema: overflow em imagens 8-bit
> pixel = 200
> escalar = 2.0
> resultado = pixel * escalar  # 400 → fora da faixa 0-255
> ```

> [!TIP] Estratégias de Normalização
> 
> **Normalização Linear:**
> ```python
> resultado = (imagem * escalar).clip(0, 255)
> ```
> 
> **Normalização com Preservação:**
> ```python
> resultado = (imagem.astype(float) * escalar * 255 / imagem.max()).astype(uint8)
> ```
> 
> **Mapeamento Logarítmico:**
> ```python
> resultado = (np.log1p(imagem * escalar) * 255 / np.log1p(255)).astype(uint8)
> ```

---

## 💡 Aplicações Práticas com Pixels e Escalares

### 1. Ajuste de Brilho com Multiplicação Escalar

> [!SUCCESS] Controle de Intensidade
> **Multiplicação por escalar para ajuste de brilho:**

```python
import cv2
import numpy as np
import matplotlib.pyplot as plt

def ajuste_brilho_multiplicativo(imagem, fator_brilho):
    """
    Ajusta brilho multiplicando todos os pixels por um escalar
    """
    # Converter para float para evitar overflow
    imagem_float = imagem.astype(np.float32)
    
    # Aplicar multiplicação escalar
    imagem_ajustada = imagem_float * fator_brilho
    
    # Manter na faixa válida
    imagem_ajustada = np.clip(imagem_ajustada, 0, 255)
    
    return imagem_ajustada.astype(np.uint8)

# Exemplo com diferentes fatores de escala
imagem_original = cv2.imread('imagem.jpg', cv2.IMREAD_COLOR)
imagem_cinza = cv2.cvtColor(imagem_original, cv2.COLOR_BGR2GRAY)

# Testar diferentes escalares
fatores = [0.5, 1.0, 1.5, 2.0, 2.5]
resultados = []

for fator in fatores:
    resultado = ajuste_brilho_multiplicativo(imagem_cinza, fator)
    resultados.append((fator, resultado))

# Visualizar resultados
plt.figure(figsize=(15, 8))
for i, (fator, img) in enumerate(resultados):
    plt.subplot(2, 3, i+1)
    plt.imshow(img, cmap='gray')
    plt.title(f'Fator: {fator}')
    plt.axis('off')
plt.tight_layout()
plt.show()
```

### 2. Aplicação de Máscaras com Multiplicação Pixel-wise

> [!EXAMPLE] Segmentação com Máscaras
> **Multiplicação pixel a pixel para isolamento de regiões:**

```python
def criar_mascara_gradual(forma, centro, raio_interno, raio_externo):
    """
    Cria máscara com transição suave (não binária)
    """
    y, x = np.ogrid[:forma[0], :forma[1]]
    dist_centro = np.sqrt((x - centro[0])**2 + (y - centro[1])**2)
    
    # Criar máscara com transição suave
    mascara = np.zeros(forma, dtype=np.float32)
    
    # Dentro do raio interno: valor 1
    mascara[dist_centro <= raio_interno] = 1.0
    
    # Entre raio interno e externo: transição suave
    mask_transicao = (dist_centro > raio_interno) & (dist_centro <= raio_externo)
    mascara[mask_transicao] = 1.0 - (dist_centro[mask_transicao] - raio_interno) / (raio_externo - raio_interno)
    
    # Fora do raio externo: valor 0
    mascara[dist_centro > raio_externo] = 0.0
    
    return mascara

def aplicar_mascara_gradual(imagem, mascara):
    """
    Aplica máscara gradual usando multiplicação pixel-wise
    """
    if len(imagem.shape) == 3:
        # Imagem colorida - multiplicar cada canal
        mascara_3d = mascara[:, :, np.newaxis]
        resultado = imagem.astype(np.float32) * mascara_3d
    else:
        # Imagem em escala de cinza
        resultado = imagem.astype(np.float32) * mascara
    
    return np.clip(resultado, 0, 255).astype(np.uint8)

# Exemplo de uso
imagem = cv2.imread('imagem.jpg')
if len(imagem.shape) == 3:
    imagem = cv2.cvtColor(imagem, cv2.COLOR_BGR2RGB)

altura, largura = imagem.shape[:2]
centro = (largura//2, altura//2)

# Criar máscara gradual
mascara = criar_mascara_gradual((altura, largura), centro, 
                               raio_interno=100, raio_externo=300)

# Aplicar máscara
imagem_com_mascara = aplicar_mascara_gradual(imagem, mascara)

# Visualizar
fig, axes = plt.subplots(1, 3, figsize=(15, 5))
axes[0].imshow(imagem)
axes[0].set_title('Imagem Original')
axes[0].axis('off')

axes[1].imshow(mascara, cmap='gray')
axes[1].set_title('Máscara Gradual')
axes[1].axis('off')

axes[2].imshow(imagem_com_mascara)
axes[2].set_title('Imagem com Máscara')
axes[2].axis('off')
plt.show()
```

### 3. Blend de Imagens com Multiplicação

> [!SUCCESS] Combinação de Imagens
> **Multiplicação para criar efeitos de blend:**

```python
def blend_multiplicativo(imagem1, imagem2, peso1=0.5, peso2=0.5):
    """
    Combina duas imagens usando multiplicação ponderada
    """
    # Garantir mesmo tamanho
    if imagem1.shape != imagem2.shape:
        imagem2 = cv2.resize(imagem2, (imagem1.shape[1], imagem1.shape[0]))
    
    # Converter para float
    img1_float = imagem1.astype(np.float32)
    img2_float = imagem2.astype(np.float32)
    
    # Aplicar blend multiplicativo
    resultado = (img1_float * peso1) * (img2_float * peso2) / 255.0
    
    return np.clip(resultado, 0, 255).astype(np.uint8)

def criar_ruido_multiplicativo(forma, intensidade=0.1):
    """
    Cria ruído multiplicativo (ruído speckle)
    """
    ruido = np.random.normal(1.0, intensidade, forma)
    return np.clip(ruido, 0, 2)  # Limitar faixa

# Exemplo: Aplicar ruído multiplicativo
imagem_original = cv2.imread('imagem.jpg', cv2.IMREAD_GRAYSCALE)

# Criar ruído multiplicativo
ruido = criar_ruido_multiplicativo(imagem_original.shape)

# Aplicar ruído (multiplicação pixel-wise)
imagem_ruidosa = (imagem_original.astype(np.float32) * ruido).astype(np.uint8)

# Visualizar
plt.figure(figsize=(12, 4))
plt.subplot(1, 3, 1)
plt.imshow(imagem_original, cmap='gray')
plt.title('Original')
plt.axis('off')

plt.subplot(1, 3, 2)
plt.imshow(ruido, cmap='gray')
plt.title('Ruído Multiplicativo')
plt.axis('off')

plt.subplot(1, 3, 3)
plt.imshow(imagem_ruidosa, cmap='gray')
plt.title('Imagem com Ruído')
plt.axis('off')
plt.show()
```

### 4. Correção Gamma com Multiplicação Não-linear

> [!EXAMPLE] Transformação de Intensidade
> **Multiplicação não-linear para correção gamma:**

```python
def correcao_gamma(imagem, gamma=1.0):
    """
    Aplica correção gamma usando multiplicação não-linear
    """
    # Normalizar para [0, 1]
    imagem_normalizada = imagem.astype(np.float32) / 255.0
    
    # Aplicar correção gamma
    imagem_corrigida = np.power(imagem_normalizada, gamma)
    
    # Voltar para [0, 255]
    return (imagem_corrigida * 255).astype(np.uint8)

def criar_lut_gamma(gamma, bits=8):
    """
    Cria Look-Up Table para correção gamma
    """
    niveis = 2 ** bits
    lut = np.arange(0, niveis, dtype=np.float32)
    lut = lut / (niveis - 1)  # Normalizar
    lut = np.power(lut, gamma)  # Aplicar gamma
    lut = lut * (niveis - 1)   # Desnormalizar
    return lut.astype(np.uint8)

# Testar diferentes valores de gamma
gammas = [0.5, 0.8, 1.0, 1.5, 2.2, 3.0]
imagem_teste = cv2.imread('imagem.jpg', cv2.IMREAD_GRAYSCALE)

plt.figure(figsize=(15, 10))
for i, gamma in enumerate(gammas):
    imagem_corrigida = correcao_gamma(imagem_teste, gamma)
    
    plt.subplot(2, 3, i+1)
    plt.imshow(imagem_corrigida, cmap='gray')
    plt.title(f'Gamma = {gamma}')
    plt.axis('off')

plt.tight_layout()
plt.show()

# Mostrar curvas de correção gamma
plt.figure(figsize=(10, 6))
x = np.linspace(0, 1, 256)
for gamma in [0.5, 0.8, 1.0, 1.5, 2.2]:
    y = np.power(x, gamma)
    plt.plot(x, y, label=f'γ = {gamma}', linewidth=2)

plt.xlabel('Intensidade de Entrada')
plt.ylabel('Intensidade de Saída')
plt.title('Curvas de Correção Gamma')
plt.legend()
plt.grid(True, alpha=0.3)
plt.show()
```

### 5. Multiplicação com Mapas de Textura

> [!SUCCESS] Aplicação de Texturas
> **Multiplicação para aplicar texturas em imagens:**

```python
def aplicar_textura_multiplicativa(imagem_base, textura, intensidade=0.5):
    """
    Aplica textura usando multiplicação pixel-wise
    """
    # Redimensionar textura se necessário
    if textura.shape != imagem_base.shape:
        textura = cv2.resize(textura, (imagem_base.shape[1], imagem_base.shape[0]))
    
    # Normalizar textura
    textura_norm = textura.astype(np.float32) / 255.0
    
    # Aplicar textura multiplicativamente
    if len(imagem_base.shape) == 3:
        textura_3d = textura_norm[:, :, np.newaxis]
        resultado = imagem_base.astype(np.float32) * (1 - intensidade + intensidade * textura_3d)
    else:
        resultado = imagem_base.astype(np.float32) * (1 - intensidade + intensidade * textura_norm)
    
    return np.clip(resultado, 0, 255).astype(np.uint8)

# Criar texturas programaticamente
def criar_textura_ruido(forma, escala=10.0):
    """
    Cria textura de ruído Perlin-like
    """
    textura = np.random.rand(forma[0], forma[1]).astype(np.float32)
    
    # Suavizar o ruído
    kernel_size = max(3, int(min(forma) / escala))
    kernel_size = kernel_size + 1 if kernel_size % 2 == 0 else kernel_size
    textura = cv2.GaussianBlur(textura, (kernel_size, kernel_size), 0)
    
    return (textura * 255).astype(np.uint8)

# Exemplo: aplicar textura a imagem
imagem_base = cv2.imread('imagem.jpg', cv2.IMREAD_GRAYSCALE)
textura = criar_textura_ruido(imagem_base.shape)

# Aplicar com diferentes intensidades
intensidades = [0.2, 0.5, 0.8]
resultados_textura = []

for intensidade in intensidades:
    resultado = aplicar_textura_multiplicativa(imagem_base, textura, intensidade)
    resultados_textura.append((intensidade, resultado))

# Visualizar
fig, axes = plt.subplots(2, 3, figsize=(15, 8))
axes[0, 0].imshow(imagem_base, cmap='gray')
axes[0, 0].set_title('Imagem Base')
axes[0, 0].axis('off')

axes[0, 1].imshow(textura, cmap='gray')
axes[0, 1].set_title('Textura')
axes[0, 1].axis('off')

axes[0, 2].axis('off')  # Espaço vazio

for i, (intensidade, img) in enumerate(resultados_textura):
    axes[1, i].imshow(img, cmap='gray')
    axes[1, i].set_title(f'Intensidade: {intensidade}')
    axes[1, i].axis('off')

plt.tight_layout()
plt.show()
```

---

## 🖥️ Classe Completa para Multiplicação

```python
import cv2
import numpy as np
from typing import Union, Tuple

class MultiplicadorImagens:
    def __init__(self):
        self.metodos = {
            'escalar': self.multiplicacao_escalar,
            'pixel_wise': self.multiplicacao_pixel_wise,
            'gamma': self.correcao_gamma,
            'textura': self.aplicar_textura
        }
    
    def multiplicacao_escalar(self, imagem: np.ndarray, escalar: float) -> np.ndarray:
        """
        Multiplica todos os pixels por um escalar
        """
        resultado = imagem.astype(np.float32) * escalar
        return np.clip(resultado, 0, 255).astype(np.uint8)
    
    def multiplicacao_pixel_wise(self, imagem1: np.ndarray, imagem2: np.ndarray, 
                               normalizar: bool = True) -> np.ndarray:
        """
        Multiplicação pixel a pixel entre duas imagens
        """
        # Garantir mesmo tamanho
        if imagem1.shape != imagem2.shape:
            imagem2 = cv2.resize(imagem2, (imagem1.shape[1], imagem1.shape[0]))
        
        resultado = imagem1.astype(np.float32) * imagem2.astype(np.float32)
        
        if normalizar:
            resultado = resultado * 255.0 / np.max(resultado) if np.max(resultado) > 0 else resultado
        
        return np.clip(resultado, 0, 255).astype(np.uint8)
    
    def correcao_gamma(self, imagem: np.ndarray, gamma: float) -> np.ndarray:
        """
        Aplica correção gamma
        """
        imagem_norm = imagem.astype(np.float32) / 255.0
        resultado = np.power(imagem_norm, gamma)
        return (resultado * 255).astype(np.uint8)
    
    def criar_mascara_radial(self, forma: Tuple[int, int], centro: Tuple[int, int], 
                           raio_interno: int, raio_externo: int) -> np.ndarray:
        """
        Cria máscara radial com transição suave
        """
        y, x = np.ogrid[:forma[0], :forma[1]]
        dist = np.sqrt((x - centro[0])**2 + (y - centro[1])**2)
        
        mascara = np.ones(forma, dtype=np.float32)
        mascara[dist <= raio_interno] = 1.0
        
        mask_trans = (dist > raio_interno) & (dist <= raio_externo)
        mascara[mask_trans] = 1.0 - (dist[mask_trans] - raio_interno) / (raio_externo - raio_interno)
        
        mascara[dist > raio_externo] = 0.0
        return mascara

# Exemplo de uso
multiplicador = MultiplicadorImagens()
imagem = cv2.imread('imagem.jpg', cv2.IMREAD_GRAYSCALE)

# Diferentes operações
resultado_escalar = multiplicador.multiplicacao_escalar(imagem, 1.5)
resultado_gamma = multiplicador.correcao_gamma(imagem, 0.8)

# Criar e aplicar máscara radial
mascara = multiplicador.criar_mascara_radial(imagem.shape, 
                                           (imagem.shape[1]//2, imagem.shape[0]//2),
                                           100, 300)
resultado_mascara = multiplicador.multiplicacao_pixel_wise(imagem, 
                                                         (mascara * 255).astype(np.uint8))
```

---

## 📊 Resumo das Técnicas

| Técnica | Tipo | Aplicação | Vantagens |
|---------|------|-----------|-----------|
| **Multiplicação Escalar** | Global | Ajuste de brilho | Simplicidade |
| **Pixel-wise** | Local | Máscaras, blends | Controle preciso |
| **Correção Gamma** | Não-linear | Correção de contraste | Preserva detalhes |
| **Aplicação de Textura** | Local | Efeitos visuais | Realismo |

> [!SUMMARY] Conclusão
> A multiplicação em processamento de imagens oferece versatilidade única:
> - **Controle global** através de escalares para ajustes de intensidade
> - **Operações locais** precisas com multiplicação pixel-wise
> - **Transformações não-lineares** para correção tonal
> - **Combinação criativa** de imagens e texturas
> 
> O cuidado com normalização e faixa dinâmica é essencial para resultados profissionais.