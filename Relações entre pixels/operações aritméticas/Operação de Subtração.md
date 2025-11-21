# Operação de Subtração em Processamento de Imagens

> [!NOTE] Conceito Fundamental
> A **subtração de imagens** é uma operação aritmética onde os valores de pixels correspondentes de duas imagens são subtraídos para produzir uma nova imagem. Matematicamente:
> $$C(x, y) = A(x, y) - B(x, y)$$
> onde $C(x, y)$ é o pixel resultante na posição $(x, y)$.

## 📋 Fundamentos da Operação de Subtração

### Princípio Básico

> [!ABSTRACT] Características da Subtração
> - **Requisito**: As imagens devem ter as mesmas dimensões (largura e altura)
> - **Domínio**: Operação aplicada a cada pixel individualmente
> - **Resultado**: Valores de pixel podem ser negativos
> - **Pós-processamento**: Normalmente requer ajuste de escala ou valor absoluto

### Formulação Matemática

> [!IMPORTANT] Equações da Subtração
> 
> **Subtração simples:**
> $$C(i,j) = A(i,j) - B(i,j)$$
> 
> **Subtração com valor absoluto:**
> $$C(i,j) = |A(i,j) - B(i,j)|$$
> 
> **Subtração com deslocamento:**
> $$C(i,j) = A(i,j) - B(i,j) + K$$

---

## 🔧 Implementação e Técnicas

### Controle de Faixa Dinâmica

> [!CAUTION] Problema de Valores Negativos
> Em imagens 8-bit, os valores variam de 0 a 255. A subtração pode resultar em valores negativos:
> ```python
> # Exemplo: subtração sem controle
> pixel_A = 100
> pixel_B = 200
> subtracao = pixel_A - pixel_B  # Resultado: -100 → PROBLEMA!
> ```

> [!TIP] Soluções para Valores Negativos
> 
> **1. Valor Absoluto:**
> ```python
> resultado = abs(A - B)  # Elimina valores negativos
> ```
> 
> **2. Deslocamento:**
> ```python
> resultado = (A - B) + 128  # Centraliza em 128
> ```
> 
> **3. Clipping:**
> ```python
> resultado = max(0, A - B)  # Limita mínimo a 0
> ```

---

## 💡 Aplicações Práticas da Subtração de Imagens

### 1. Detecção de Movimento

> [!SUCCESS] Aplicação Mais Importante
> **Problema**: Identificar objetos em movimento entre frames consecutivos
> **Solução**: Subtrair frames para detectar mudanças

**Implementação:**
```python
import cv2
import numpy as np

def detectar_movimento(frame_atual, frame_anterior, threshold=30):
    """
    Detecta movimento entre dois frames usando subtração
    """
    # Converter para escala de cinza se necessário
    if len(frame_atual.shape) == 3:
        frame_atual = cv2.cvtColor(frame_atual, cv2.COLOR_BGR2GRAY)
    if len(frame_anterior.shape) == 3:
        frame_anterior = cv2.cvtColor(frame_anterior, cv2.COLOR_BGR2GRAY)
    
    # Subtração absoluta
    diferenca = cv2.absdiff(frame_atual, frame_anterior)
    
    # Aplicar threshold para criar máscara binária
    _, mascara = cv2.threshold(diferenca, threshold, 255, cv2.THRESH_BINARY)
    
    # Operações morfológicas para remover ruído
    kernel = np.ones((5,5), np.uint8)
    mascara = cv2.morphologyEx(mascara, cv2.MORPH_OPEN, kernel)
    mascara = cv2.morphologyEx(mascara, cv2.MORPH_CLOSE, kernel)
    
    return mascara, diferenca

# Exemplo de uso em vídeo surveillance
cap = cv2.VideoCapture(0)
ret, frame_anterior = cap.read()

while True:
    ret, frame_atual = cap.read()
    if not ret:
        break
    
    mascara_movimento, diferenca = detectar_movimento(frame_atual, frame_anterior)
    
    # Mostrar resultados
    cv2.imshow('Video', frame_atual)
    cv2.imshow('Movimento', mascara_movimento)
    cv2.imshow('Diferença', diferenca)
    
    frame_anterior = frame_atual.copy()
    
    if cv2.waitKey(1) & 0xFF == ord('q'):
        break

cap.release()
cv2.destroyAllWindows()
```

### 2. Segmentação por Fundo Fixo

> [!EXAMPLE] Aplicação em Visão Computacional
> **Problema**: Isolar objetos em primeiro plano de um fundo estático
> **Solução**: Subtrair imagem de fundo da cena atual

```python
def segmentar_foreground(cena_atual, fundo, threshold=25):
    """
    Segmenta objetos em primeiro plano subtraindo o fundo
    """
    # Garantir que as imagens estão em escala de cinza
    if len(cena_atual.shape) == 3:
        cena_atual = cv2.cvtColor(cena_atual, cv2.COLOR_BGR2GRAY)
    if len(fundo.shape) == 3:
        fundo = cv2.cvtColor(fundo, cv2.COLOR_BGR2GRAY)
    
    # Subtração absoluta
    diferenca = cv2.absdiff(cena_atual, fundo)
    
    # Threshold para criar máscara
    _, mascara = cv2.threshold(diferenca, threshold, 255, cv2.THRESH_BINARY)
    
    return mascara, diferenca

# Aplicação: contagem de pessoas, análise de tráfego
fundo = cv2.imread('fundo_fixo.jpg', cv2.IMREAD_GRAYSCALE)
cena_atual = cv2.imread('cena_com_objetos.jpg', cv2.IMREAD_GRAYSCALE)

mascara_foreground, diferenca = segmentar_foreground(cena_atual, fundo)
```

### 3. Comparação de Imagens Médicas

> [!SUCCESS] Aplicação em Diagnóstico
> **Problema**: Detectar mudanças em exames médicos sequenciais
> **Solução**: Subtrair imagens para realçar diferenças

```python
def comparar_imagens_medicas(imagem_base, imagem_atual, metodo='absoluto'):
    """
    Compara imagens médicas para detectar mudanças
    """
    # Garantir mesmo tamanho e tipo
    imagem_base = imagem_base.astype(np.float32)
    imagem_atual = imagem_atual.astype(np.float32)
    
    if metodo == 'absoluto':
        diferenca = cv2.absdiff(imagem_base, imagem_atual)
    elif metodo == 'diferenca_direta':
        diferenca = imagem_atual - imagem_base  # Pode ter valores negativos
    elif metodo == 'normalizada':
        diferenca = (imagem_atual - imagem_base) + 128  # Centralizada
    
    # Realçar diferenças para visualização
    diferenca_visual = cv2.normalize(diferenca, None, 0, 255, cv2.NORM_MINMAX)
    
    return diferenca.astype(np.uint8), diferenca_visual.astype(np.uint8)

# Aplicação: comparação de radiografias, ressonâncias
raio_x_antes = cv2.imread('raio_x_antes.png', cv2.IMREAD_GRAYSCALE)
raio_x_depois = cv2.imread('raio_x_depois.png', cv2.IMREAD_GRAYSCALE)

diferenca, diferenca_visual = comparar_imagens_medicas(raio_x_antes, raio_x_depois)
```

### 4. Detecção de Defeitos em Inspeção Industrial

> [!EXAMPLE] Controle de Qualidade
> **Problema**: Identificar defeitos em produtos manufacturados
> **Solução**: Subtrair imagem do produto ideal da imagem do produto real

```python
def inspecionar_defeitos(produto_ideal, produto_real, tolerancia=15):
    """
    Detecta defeitos subtraindo produto real do ideal
    """
    # Subtração absoluta
    diferenca = cv2.absdiff(produto_ideal, produto_real)
    
    # Threshold para identificar defeitos significativos
    _, defeitos = cv2.threshold(diferenca, tolerancia, 255, cv2.THRESH_BINARY)
    
    # Calcular área total de defeitos
    area_defeitos = np.sum(defeitos) / 255
    
    # Encontrar contornos dos defeitos
    contornos, _ = cv2.findContours(defeitos, cv2.RETR_EXTERNAL, cv2.CHAIN_APPROX_SIMPLE)
    
    return defeitos, area_defeitos, contornos

# Aplicação em linha de produção
produto_ideal = cv2.imread('produto_perfeito.jpg', cv2.IMREAD_GRAYSCALE)
produto_real = cv2.imread('produto_testado.jpg', cv2.IMREAD_GRAYSCALE)

defeitos, area_defeitos, contornos = inspecionar_defeitos(produto_ideal, produto_real)

print(f"Área total de defeitos: {area_defeitos} pixels")
```

### 5. Análise de Mudanças Temporais

> [!SUCCESS] Aplicação em Sensoriamento Remoto
> **Problema**: Monitorar mudanças em imagens de satélite
> **Solução**: Subtrair imagens de diferentes períodos

```python
def analisar_mudancas_satelite(imagem_periodo1, imagem_periodo2):
    """
    Analisa mudanças em imagens de satélite
    """
    # Converter para float para evitar overflow
    img1 = imagem_periodo1.astype(np.float32)
    img2 = imagem_periodo2.astype(np.float32)
    
    # Subtração normalizada (evita valores negativos)
    diferenca = cv2.absdiff(img1, img2)
    
    # Aplicar colormap para melhor visualização
    diferenca_color = cv2.applyColorMap(diferenca.astype(np.uint8), cv2.COLORMAP_JET)
    
    # Calcular índice de mudança
    mudanca_total = np.sum(diferenca) / (imagem_periodo1.shape[0] * imagem_periodo1.shape[1])
    
    return diferenca.astype(np.uint8), diferenca_color, mudanca_total

# Aplicação: monitoramento ambiental, urbanização
satelite_2020 = cv2.imread('satelite_2020.tif', cv2.IMREAD_GRAYSCALE)
satelite_2023 = cv2.imread('satelite_2023.tif', cv2.IMREAD_GRAYSCALE)

diferenca, diferenca_color, indice_mudanca = analisar_mudancas_satelite(satelite_2020, satelite_2023)
print(f"Índice de mudança: {indice_mudanca:.2f}")
```

---

## 🖥️ Implementação em Python/OpenCV

### Classe Completa para Subtração de Imagens

> [!TIP] Implementação Robusta
```python
import cv2
import numpy as np
import matplotlib.pyplot as plt

class SubtratorImagens:
    def __init__(self):
        self.metodos = {
            'absoluto': self.subtracao_absoluta,
            'diferenca_direta': self.subtracao_direta,
            'normalizada': self.subtracao_normalizada,
            'threshold': self.subtracao_com_threshold
        }
    
    def subtracao_absoluta(self, img1, img2):
        """
        Subtração com valor absoluto (elimina negativos)
        """
        return cv2.absdiff(img1, img2)
    
    def subtracao_direta(self, img1, img2):
        """
        Subtração direta (pode resultar em valores negativos)
        """
        resultado = img1.astype(np.float32) - img2.astype(np.float32)
        return resultado
    
    def subtracao_normalizada(self, img1, img2, centro=128):
        """
        Subtração com deslocamento para evitar negativos
        """
        resultado = img1.astype(np.float32) - img2.astype(np.float32) + centro
        return np.clip(resultado, 0, 255).astype(np.uint8)
    
    def subtracao_com_threshold(self, img1, img2, threshold=30):
        """
        Subtração com threshold para criar máscara binária
        """
        diferenca = cv2.absdiff(img1, img2)
        _, mascara = cv2.threshold(diferenca, threshold, 255, cv2.THRESH_BINARY)
        return mascara
    
    def processar_video_movimento(self, video_path, output_path=None):
        """
        Processa vídeo completo para detecção de movimento
        """
        cap = cv2.VideoCapture(video_path)
        
        # Ler primeiro frame
        ret, frame_anterior = cap.read()
        if not ret:
            return
        
        frame_anterior_cinza = cv2.cvtColor(frame_anterior, cv2.COLOR_BGR2GRAY)
        
        if output_path:
            fourcc = cv2.VideoWriter_fourcc(*'XVID')
            out = cv2.VideoWriter(output_path, fourcc, 20.0, 
                                (frame_anterior.shape[1], frame_anterior.shape[0]))
        
        while True:
            ret, frame_atual = cap.read()
            if not ret:
                break
            
            frame_atual_cinza = cv2.cvtColor(frame_atual, cv2.COLOR_BGR2GRAY)
            
            # Detectar movimento
            mascara_movimento = self.subtracao_com_threshold(
                frame_atual_cinza, frame_anterior_cinza)
            
            # Aplicar máscara ao frame original
            frame_com_movimento = frame_atual.copy()
            frame_com_movimento[mascara_movimento == 255] = [0, 255, 0]  # Verde para movimento
            
            if output_path:
                out.write(frame_com_movimento)
            
            cv2.imshow('Movimento Detectado', frame_com_movimento)
            cv2.imshow('Máscara', mascara_movimento)
            
            frame_anterior_cinza = frame_atual_cinza.copy()
            
            if cv2.waitKey(1) & 0xFF == ord('q'):
                break
        
        cap.release()
        if output_path:
            out.release()
        cv2.destroyAllWindows()

# Exemplo de uso
subtrator = SubtratorImagens()

# Carregar imagens para teste
img1 = cv2.imread('cena1.jpg', cv2.IMREAD_GRAYSCALE)
img2 = cv2.imread('cena2.jpg', cv2.IMREAD_GRAYSCALE)

# Aplicar diferentes métodos de subtração
resultado_absoluto = subtrator.subtracao_absoluta(img1, img2)
resultado_normalizado = subtrator.subtracao_normalizada(img1, img2)
mascara_mudancas = subtrator.subtracao_com_threshold(img1, img2, threshold=25)
```

### Visualização Comparativa

> [!CAUTION] Código para Análise de Resultados
```python
def analisar_resultados_subtracao(imagens, titulos):
    """
    Exibe múltiplos resultados de subtração para comparação
    """
    plt.figure(figsize=(15, 10))
    
    for i, (img, titulo) in enumerate(zip(imagens, titulos)):
        plt.subplot(2, 3, i+1)
        
        if len(img.shape) == 2:
            plt.imshow(img, cmap='gray')
        else:
            plt.imshow(cv2.cvtColor(img, cv2.COLOR_BGR2RGB))
            
        plt.title(titulo)
        plt.axis('off')
    
    plt.tight_layout()
    plt.show()

# Visualizar resultados
imagens = [img1, img2, resultado_absoluto, resultado_normalizado, mascara_mudancas]
titulos = ['Imagem 1', 'Imagem 2', 'Subtração Absoluta', 
           'Subtração Normalizada', 'Máscara de Mudanças']

analisar_resultados_subtracao(imagens, titulos)
```

---

## ⚠️ Considerações e Limitações

> [!WARNING] Problemas Comuns
> 1. **Alinhamento**: Imagens devem estar perfeitamente alinhadas
> 2. **Iluminação**: Mudanças na iluminação causam falsos positivos
> 3. **Ruído**: Ruído pode ser interpretado como movimento
> 4. **Oclusão**: Objetos que aparecem/desaparecem

> [!TIP] Técnicas de Melhoria
> 1. **Pré-processamento**: Filtragem para reduzir ruído
> 2. **Alinhamento**: Usar técnicas de registro de imagem
> 3. **Background Modeling**: Modelar fundo dinâmico
> 4. **Morphological Operations**: Limpar máscaras binárias

---

## 📊 Resumo das Aplicações

| Aplicação | Método | Saída | Uso Típico |
|-----------|--------|-------|------------|
| **Detecção de Movimento** | `abs(A - B)` | Máscara binária | Vigilância |
| **Segmentação** | `A - fundo` | Objetos primeiro plano | Análise de cena |
| **Comparação Médica** | `A - B + 128` | Diferença realçada | Diagnóstico |
| **Inspeção Industrial** | `abs(ideal - real)` | Mapas de defeitos | Controle qualidade |
| **Sensoriamento Remoto** | `abs(t1 - t2)` | Mapas de mudança | Monitoramento |

> [!SUMMARY] Conclusão
> A operação de subtração é essencial no processamento de imagens porque:
> - **Detecta mudanças** de forma eficiente e computacionalmente leve
> - **Isola objetos** em movimento ou em primeiro plano
> - **Compara evoluções** temporais em imagens médicas e de satélite
> - **Identifica anomalias** em processos industriais
> 
> Sua simplicidade matemática combinada com técnicas de pós-processamento a torna uma ferramenta poderosa para análise de diferenças e detecção de mudanças em diversas aplicações práticas.