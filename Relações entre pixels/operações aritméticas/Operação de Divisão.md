# Operação de Divisão em Processamento de Imagens

> [!NOTE] Conceito Fundamental
> A **divisão de imagens** é uma operação aritmética que divide valores de pixels para criar novas imagens. Duas abordagens principais:
> - **Pixel-wise**: $C(x,y) = \frac{A(x,y)}{B(x,y)}$
> - **Escalar**: $C(x,y) = \frac{A(x,y)}{\alpha}$

## 📋 Tipos de Divisão

### Divisão Pixel a Pixel

> [!ABSTRACT] Características
> - Cada pixel da imagem A é dividido pelo pixel correspondente da imagem B
> - Requer cuidado com divisão por zero
> - Útil para normalização e correção

### Divisão por Escalar

> [!ABSTRACT] Características
> - Todos os pixels são divididos por um valor constante
> - Reduz intensidade, ajusta contraste
> - Operação global simples

---

## 🔧 Implementação e Controles

### Gerenciamento de Divisão por Zero

> [!CAUTION] Problema Crítico
> ```python
> # Problema: divisão por zero
> pixel_A = 100
> pixel_B = 0
> resultado = pixel_A / pixel_B  # Erro! Divisão por zero
> ```

> [!TIP] Estratégias de Proteção
> 
> **Adição de Épsilon:**
> ```python
> resultado = imagem_A / (imagem_B + 1e-8)
> ```
> 
> **Máscara de Proteção:**
> ```python
> mask = imagem_B > 0
> resultado = np.zeros_like(imagem_A, dtype=float)
> resultado[mask] = imagem_A[mask] / imagem_B[mask]
> ```
> 
> **Substituição de Valores:**
> ```python
> resultado = np.divide(imagem_A, imagem_B, out=np.zeros_like(imagem_A), where=imagem_B!=0)
> ```

---

## 💡 Aplicações Práticas com Pixels e Escalares

### 1. Normalização de Imagens

> [!SUCCESS] Aplicação Fundamental
> **Divisão para normalizar intensidades:**

```python
import cv2
import numpy as np
import matplotlib.pyplot as plt

def normalizar_imagem(imagem, metodo='max'):
    """
    Normaliza imagem usando divisão
    """
    imagem_float = imagem.astype(np.float32)
    
    if metodo == 'max':
        # Divisão pelo valor máximo
        max_val = np.max(imagem_float)
        if max_val > 0:
            imagem_normalizada = imagem_float / max_val
        else:
            imagem_normalizada = imagem_float
    elif metodo == 'media':
        # Divisão pela média
        mean_val = np.mean(imagem_float)
        if mean_val > 0:
            imagem_normalizada = imagem_float / mean_val
        else:
            imagem_normalizada = imagem_float
    elif metodo == 'std':
        # Normalização Z-score (usa divisão)
        mean_val = np.mean(imagem_float)
        std_val = np.std(imagem_float)
        if std_val > 0:
            imagem_normalizada = (imagem_float - mean_val) / std_val
        else:
            imagem_normalizada = imagem_float
    
    # Escalar para [0, 255] se necessário
    if metodo != 'std':
        imagem_normalizada = np.clip(imagem_normalizada * 255, 0, 255)
    
    return imagem_normalizada.astype(np.uint8)

# Exemplo com diferentes métodos de normalização
imagem_original = cv2.imread('imagem.jpg', cv2.IMREAD_GRAYSCALE)

metodos = ['max', 'media', 'std']
resultados = []

for metodo in metodos:
    resultado = normalizar_imagem(imagem_original, metodo)
    resultados.append((metodo, resultado))

# Visualizar resultados
plt.figure(figsize=(15, 5))
for i, (metodo, img) in enumerate(resultados):
    plt.subplot(1, 4, i+1)
    plt.imshow(img, cmap='gray')
    plt.title(f'Normalização: {metodo}')
    plt.axis('off')

plt.subplot(1, 4, 4)
plt.imshow(imagem_original, cmap='gray')
plt.title('Original')
plt.axis('off')
plt.tight_layout()
plt.show()
```

### 2. Correção de Iluminação

> [!EXAMPLE] Divisão para Remover Variações de Iluminação
> **Usando divisão para corrigir padrões de iluminação:**

```python
def estimar_fundo_iluminacao(imagem, tamanho_kernel=51):
    """
    Estima o padrão de iluminação usando filtro de média
    """
    kernel = np.ones((tamanho_kernel, tamanho_kernel), np.float32) / (tamanho_kernel * tamanho_kernel)
    fundo = cv2.filter2D(imagem.astype(np.float32), -1, kernel)
    return fundo

def corrigir_iluminacao_divisao(imagem, fundo_iluminacao=None):
    """
    Corrige iluminação dividindo pelo padrão de fundo
    """
    if fundo_iluminacao is None:
        fundo_iluminacao = estimar_fundo_iluminacao(imagem)
    
    # Adicionar epsilon para evitar divisão por zero
    epsilon = 1e-8
    imagem_corrigida = (imagem.astype(np.float32) + epsilon) / (fundo_iluminacao + epsilon)
    
    # Normalizar para faixa visível
    imagem_corrigida = imagem_corrigida * 128  # Escalar para melhor visualização
    return np.clip(imagem_corrigida, 0, 255).astype(np.uint8), fundo_iluminacao.astype(np.uint8)

# Criar imagem com iluminação não uniforme para demonstração
def criar_iluminacao_nao_uniforme(forma, centro, intensidade_maxima=200):
    """
    Cria padrão de iluminação não uniforme (vignette)
    """
    y, x = np.ogrid[:forma[0], :forma[1]]
    dist_centro = np.sqrt((x - centro[0])**2 + (y - centro[1])**2)
    max_dist = np.sqrt(centro[0]**2 + centro[1]**2)
    
    # Criar iluminação que diminui das bordas para o centro
    iluminacao = intensidade_maxima * (1 - dist_centro / max_dist * 0.7)
    return np.clip(iluminacao, 50, intensidade_maxima).astype(np.uint8)

# Exemplo: corrigir iluminação não uniforme
imagem_original = cv2.imread('imagem.jpg', cv2.IMREAD_GRAYSCALE)
altura, largura = imagem_original.shape

# Criar iluminação não uniforme
iluminacao_nao_uniforme = criar_iluminacao_nao_uniforme((altura, largura), 
                                                       (largura//2, altura//2))

# Aplicar iluminação não uniforme (multiplicação)
imagem_nao_uniforme = (imagem_original.astype(np.float32) * 
                      (iluminacao_nao_uniforme.astype(np.float32) / 255)).astype(np.uint8)

# Corrigir usando divisão
imagem_corrigida, fundo_estimado = corrigir_iluminacao_divisao(imagem_nao_uniforme)

# Visualizar
fig, axes = plt.subplots(2, 2, figsize=(12, 10))
axes[0, 0].imshow(imagem_original, cmap='gray')
axes[0, 0].set_title('Imagem Original')
axes[0, 0].axis('off')

axes[0, 1].imshow(iluminacao_nao_uniforme, cmap='gray')
axes[0, 1].set_title('Padrão de Iluminação')
axes[0, 1].axis('off')

axes[1, 0].imshow(imagem_nao_uniforme, cmap='gray')
axes[1, 0].set_title('Com Ilumação Não Uniforme')
axes[1, 0].axis('off')

axes[1, 1].imshow(imagem_corrigida, cmap='gray')
axes[1, 1].set_title('Após Correção (Divisão)')
axes[1, 1].axis('off')

plt.tight_layout()
plt.show()
```

### 3. Cálculo de Razão entre Imagens

> [!SUCCESS] Análise Comparativa
> **Divisão para calcular razões entre imagens:**

```python
def calcular_razao_imagens(imagem1, imagem2, metodo='razao_simples'):
    """
    Calcula razão entre duas imagens usando divisão
    """
    # Garantir mesmo tamanho
    if imagem1.shape != imagem2.shape:
        imagem2 = cv2.resize(imagem2, (imagem1.shape[1], imagem1.shape[0]))
    
    imagem1_float = imagem1.astype(np.float32) + 1e-8  # Evitar divisão por zero
    imagem2_float = imagem2.astype(np.float32) + 1e-8
    
    if metodo == 'razao_simples':
        razao = imagem1_float / imagem2_float
    elif metodo == 'razao_log':
        razao = np.log(imagem1_float / imagem2_float)
    elif metodo == 'razao_normalizada':
        razao = imagem1_float / imagem2_float
        razao = razao / np.max(razao)  # Normalizar
    
    # Converter para visualização
    if metodo == 'razao_log':
        # Log ratio pode ter valores negativos - escalar para [0,255]
        razao_visual = cv2.normalize(razao, None, 0, 255, cv2.NORM_MINMAX)
    else:
        razao_visual = np.clip(razao * 128, 0, 255)  # Escalar para melhor visualização
    
    return razao_visual.astype(np.uint8), razao

# Aplicação: análise de mudanças temporais
imagem_periodo1 = cv2.imread('periodo1.jpg', cv2.IMREAD_GRAYSCALE)
imagem_periodo2 = cv2.imread('periodo2.jpg', cv2.IMREAD_GRAYSCALE)

# Calcular diferentes tipos de razão
razao_simples, _ = calcular_razao_imagens(imagem_periodo1, imagem_periodo2, 'razao_simples')
razao_log, _ = calcular_razao_imagens(imagem_periodo1, imagem_periodo2, 'razao_log')
razao_norm, _ = calcular_razao_imagens(imagem_periodo1, imagem_periodo2, 'razao_normalizada')

# Visualizar resultados
fig, axes = plt.subplots(2, 3, figsize=(15, 10))
axes[0, 0].imshow(imagem_periodo1, cmap='gray')
axes[0, 0].set_title('Período 1')
axes[0, 0].axis('off')

axes[0, 1].imshow(imagem_periodo2, cmap='gray')
axes[0, 1].set_title('Período 2')
axes[0, 1].axis('off')

axes[0, 2].axis('off')  # Espaço vazio

axes[1, 0].imshow(razao_simples, cmap='jet')
axes[1, 0].set_title('Razão Simples')
axes[1, 0].axis('off')

axes[1, 1].imshow(razao_log, cmap='jet')
axes[1, 1].set_title('Razão Logarítmica')
axes[1, 1].axis('off')

axes[1, 2].imshow(razao_norm, cmap='jet')
axes[1, 2].set_title('Razão Normalizada')
axes[1, 2].axis('off')

plt.tight_layout()
plt.show()
```

### 4. Homomorfismo e Filtragem no Domínio da Frequência

> [!EXAMPLE] Processamento no Domínio da Frequência
> **Divisão para filtragem homomórfica:**

```python
def filtro_homomorfico(imagem, gamma_l=0.5, gamma_h=2.0, cutoff=30, c=1.0):
    """
    Aplica filtro homomórfico usando divisão no domínio da frequência
    """
    # Converter para float e adicionar epsilon
    img_float = np.log(imagem.astype(np.float32) + 1e-8)
    
    # Transformada de Fourier
    dft = np.fft.fft2(img_float)
    dft_shift = np.fft.fftshift(dft)
    
    # Criar filtro homomórfico
    rows, cols = img_float.shape
    crow, ccol = rows // 2, cols // 2
    u, v = np.meshgrid(np.arange(cols) - ccol, np.arange(rows) - crow)
    d = np.sqrt(u**2 + v**2)
    
    # Filtro homomórfico
    H = (gamma_h - gamma_l) * (1 - np.exp(-c * (d**2 / cutoff**2))) + gamma_l
    
    # Aplicar filtro (multiplicação no domínio da frequência)
    filtered_dft = dft_shift * H
    
    # Transformada inversa
    idft_shift = np.fft.ifftshift(filtered_dft)
    img_back = np.fft.ifft2(idft_shift)
    img_back = np.real(img_back)
    
    # Exponencial para voltar ao domínio espacial
    resultado = np.exp(img_back)
    return np.clip(resultado, 0, 255).astype(np.uint8), H

# Aplicação: melhorar imagem com iluminação pobre
imagem_escura = cv2.imread('imagem_escura.jpg', cv2.IMREAD_GRAYSCALE)

# Aplicar diferentes parâmetros do filtro homomórfico
parametros = [
    (0.3, 2.0, 30),   # Alto contraste
    (0.5, 1.5, 50),   # Médio contraste
    (0.7, 1.2, 80),   # Baixo contraste
]

resultados_homomorfico = []

for gamma_l, gamma_h, cutoff in parametros:
    resultado, filtro = filtro_homomorfico(imagem_escura, gamma_l, gamma_h, cutoff)
    resultados_homomorfico.append((f"γL={gamma_l}, γH={gamma_h}", resultado, filtro))

# Visualizar
fig, axes = plt.subplots(2, 4, figsize=(16, 8))
axes[0, 0].imshow(imagem_escura, cmap='gray')
axes[0, 0].set_title('Imagem Original')
axes[0, 0].axis('off')

for i, (titulo, resultado, filtro) in enumerate(resultados_homomorfico):
    axes[0, i+1].imshow(resultado, cmap='gray')
    axes[0, i+1].set_title(titulo)
    axes[0, i+1].axis('off')
    
    axes[1, i+1].imshow(filtro, cmap='viridis')
    axes[1, i+1].set_title(f'Filtro {titulo}')
    axes[1, i+1].axis('off')

axes[1, 0].axis('off')
plt.tight_layout()
plt.show()
```

### 5. Análise de Textura por Divisão Local

> [!SUCCESS] Caracterização de Texturas
> **Divisão para análise de padrões locais:**

```python
def analise_textura_divisao(imagem, tamanho_janela=15):
    """
    Analisa textura usando divisão por média local
    """
    # Calcular média local
    kernel = np.ones((tamanho_janela, tamanho_janela), np.float32) / (tamanho_janela * tamanho_janela)
    media_local = cv2.filter2D(imagem.astype(np.float32), -1, kernel)
    
    # Divisão pela média local (normalização local)
    epsilon = 1e-8
    textura_normalizada = imagem.astype(np.float32) / (media_local + epsilon)
    
    # Estatísticas da textura
    contraste_local = np.std(textura_normalizada)
    uniformidade = np.mean(textura_normalizada)
    
    # Visualização
    textura_visual = np.clip(textura_normalizada * 128, 0, 255)
    
    return textura_visual.astype(np.uint8), media_local.astype(np.uint8), contraste_local, uniformidade

def criar_imagem_texturizada(forma, tipo='ruido'):
    """
    Cria imagens com diferentes texturas para teste
    """
    if tipo == 'ruido':
        return np.random.randint(0, 256, forma, dtype=np.uint8)
    elif tipo == 'listras':
        img = np.zeros(forma, dtype=np.uint8)
        for i in range(0, forma[0], 20):
            img[i:i+10, :] = 255
        return img
    elif tipo == 'xadrez':
        img = np.zeros(forma, dtype=np.uint8)
        for i in range(0, forma[0], 20):
            for j in range(0, forma[1], 20):
                if (i // 20 + j // 20) % 2 == 0:
                    img[i:i+10, j:j+10] = 255
        return img

# Testar em diferentes texturas
formas = [(256, 256)]
tipos_textura = ['ruido', 'listras', 'xadrez']

plt.figure(figsize=(15, 12))
linha = 0

for tipo in tipos_textura:
    imagem_textura = criar_imagem_texturizada((256, 256), tipo)
    textura_analisada, media_local, contraste, uniformidade = analise_textura_divisao(imagem_textura)
    
    # Plot original
    plt.subplot(3, 3, linha*3 + 1)
    plt.imshow(imagem_textura, cmap='gray')
    plt.title(f'Textura: {tipo}\nOriginal')
    plt.axis('off')
    
    # Plot média local
    plt.subplot(3, 3, linha*3 + 2)
    plt.imshow(media_local, cmap='gray')
    plt.title('Média Local')
    plt.axis('off')
    
    # Plot textura analisada
    plt.subplot(3, 3, linha*3 + 3)
    plt.imshow(textura_analisada, cmap='gray')
    plt.title(f'Textura Normalizada\nContraste: {contraste:.2f}')
    plt.axis('off')
    
    linha += 1

plt.tight_layout()
plt.show()
```

---

## 🖥️ Classe Completa para Divisão de Imagens

```python
import cv2
import numpy as np
from typing import Union, Tuple, Optional

class DivisorImagens:
    def __init__(self):
        self.metodos = {
            'escalar': self.divisao_escalar,
            'pixel_wise': self.divisao_pixel_wise,
            'normalizacao': self.normalizar_imagem,
            'razao': self.calcular_razao
        }
    
    def divisao_escalar(self, imagem: np.ndarray, escalar: float, 
                       protecao_zero: bool = True) -> np.ndarray:
        """
        Divide todos os pixels por um escalar
        """
        if protecao_zero and abs(escalar) < 1e-8:
            escalar = 1e-8
        
        resultado = imagem.astype(np.float32) / escalar
        return np.clip(resultado, 0, 255).astype(np.uint8)
    
    def divisao_pixel_wise(self, imagem1: np.ndarray, imagem2: np.ndarray,
                          metodo_protecao: str = 'epsilon') -> np.ndarray:
        """
        Divisão pixel a pixel entre duas imagens
        """
        # Garantir mesmo tamanho
        if imagem1.shape != imagem2.shape:
            imagem2 = cv2.resize(imagem2, (imagem1.shape[1], imagem1.shape[0]))
        
        img1_float = imagem1.astype(np.float32)
        img2_float = imagem2.astype(np.float32)
        
        if metodo_protecao == 'epsilon':
            resultado = img1_float / (img2_float + 1e-8)
        elif metodo_protecao == 'mascara':
            mask = img2_float > 1e-8
            resultado = np.zeros_like(img1_float)
            resultado[mask] = img1_float[mask] / img2_float[mask]
        elif metodo_protecao == 'substituicao':
            resultado = np.divide(img1_float, img2_float, 
                                out=np.zeros_like(img1_float), 
                                where=img2_float>1e-8)
        
        return np.clip(resultado, 0, 255).astype(np.uint8)
    
    def normalizar_imagem(self, imagem: np.ndarray, 
                         referencia: Optional[np.ndarray] = None) -> np.ndarray:
        """
        Normaliza imagem por divisão
        """
        if referencia is not None:
            # Normalizar por imagem de referência
            return self.divisao_pixel_wise(imagem, referencia)
        else:
            # Normalização automática
            max_val = np.max(imagem)
            if max_val > 0:
                return self.divisao_escalar(imagem, max_val/255.0)
            else:
                return imagem
    
    def calcular_razao(self, imagem1: np.ndarray, imagem2: np.ndarray,
                      tipo_razao: str = 'simples') -> np.ndarray:
        """
        Calcula razão entre imagens
        """
        razao = self.divisao_pixel_wise(imagem1, imagem2)
        
        if tipo_razao == 'log':
            razao = np.log(razao.astype(np.float32) + 1e-8)
            razao = cv2.normalize(razao, None, 0, 255, cv2.NORM_MINMAX)
        elif tipo_razao == 'normalizada':
            max_razao = np.max(razao)
            if max_razao > 0:
                razao = razao / max_razao * 255
        
        return np.clip(razao, 0, 255).astype(np.uint8)
    
    def corrigir_iluminacao(self, imagem: np.ndarray, 
                           tamanho_kernel: int = 51) -> np.ndarray:
        """
        Corrige iluminação dividindo pelo padrão de fundo
        """
        # Estimar fundo de iluminação
        kernel = np.ones((tamanho_kernel, tamanho_kernel), np.float32)
        kernel /= np.sum(kernel)
        fundo = cv2.filter2D(imagem.astype(np.float32), -1, kernel)
        
        # Corrigir por divisão
        return self.divisao_pixel_wise(imagem, fundo.astype(np.uint8))

# Exemplo de uso
divisor = DivisorImagens()
imagem = cv2.imread('imagem.jpg', cv2.IMREAD_GRAYSCALE)

# Diferentes operações de divisão
resultado_escalar = divisor.divisao_escalar(imagem, 2.0)
resultado_normalizado = divisor.normalizar_imagem(imagem)

# Criar imagem de referência para divisão
referencia = cv2.GaussianBlur(imagem, (51, 51), 0)
resultado_razao = divisor.calcular_razao(imagem, referencia)
resultado_corrigido = divisor.corrigir_iluminacao(imagem)
```

---

## ⚠️ Considerações Importantes

> [!WARNING] Desafios da Divisão
> 1. **Divisão por zero**: Sempre implementar proteções
> 2. **Amplificação de ruído**: Ruído pode ser amplificado em regiões escuras
> 3. **Precisão numérica**: Usar float32 ou float64 para evitar perda
> 4. **Interpretação**: Resultados podem precisar de pós-processamento

> [!TIP] Boas Práticas
> 1. **Sempre use proteção** contra divisão por zero
> 2. **Considere usar log** para razões com grande variação
> 3. **Normalize resultados** para visualização adequada
> 4. **Teste com dados sintéticos** antes de aplicar a imagens reais

---

## 📊 Resumo das Aplicações

| Aplicação | Método | Uso | Resultado |
|-----------|--------|-----|-----------|
| **Normalização** | Divisão por máximo | Padronização | Imagem [0,1] ou [0,255] |
| **Correção Iluminação** | Divisão por fundo | Remover vignette | Iluminação uniforme |
| **Razão de Imagens** | Divisão pixel-wise | Análise comparativa | Mapas de mudança |
| **Filtro Homomórfico** | Divisão no domínio freq | Melhorar contraste | Realce de detalhes |
| **Análise de Textura** | Divisão por média local | Caracterização | Padrões normalizados |

> [!SUMMARY] Conclusão
> A divisão em processamento de imagens é poderosa porém requer cuidados:
> - **Normalização e padronização** de intensidades
> - **Correção sistemática** de artefatos de iluminação
> - **Análise quantitativa** através de razões entre imagens
> - **Processamento avançado** no domínio da frequência
> 
> Quando aplicada com as devidas proteções e pós-processamento, a divisão se torna uma ferramenta essencial para análise quantitativa e correção de imagens.