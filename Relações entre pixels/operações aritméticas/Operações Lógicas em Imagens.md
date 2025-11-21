# Operações Lógicas em Processamento de Imagens Binárias

> [!NOTE] Conceito Fundamental
> As **operações lógicas** são aplicadas em imagens binárias onde cada pixel possui apenas 2 valores: 0 (preto/falso) ou 255 (branco/verdadeiro). Estas operações seguem a álgebra booleana e são fundamentais para morfologia matemática.

## 📋 Operações Lógicas Básicas

### Operadores Booleanos em Imagens

> [!ABSTRACT] Operações Fundamentais
> ![[Pasted image 20251121103137.png]]
> 
> | Operação | Símbolo | Descrição | Resultado |
> |----------|---------|-----------|-----------|
> | **AND** | `A ∧ B` | Interseção | Branco onde ambas são brancas |
> | **OR** | `A ∨ B` | União | Branco onde pelo menos uma é branca |
> | **NOT** | `¬A` | Complemento | Inverte os valores |
> | **XOR** | `A ⊕ B` | OU Exclusivo | Branco onde apenas uma é branca |
> 
> ![[Pasted image 20251121102724.png]]
---

## 🔧 Implementação das Operações Lógicas

### Conversão para Binário

> [!CAUTION] Pré-requisito
> ```python
> # Converter imagem para binária (0 ou 255)
> def binarizar_imagem(imagem, threshold=127):
>     _, binaria = cv2.threshold(imagem, threshold, 255, cv2.THRESH_BINARY)
>     return binaria
> 
> # Garantir que as imagens são binárias
> img1_bin = binarizar_imagem(img1)
> img2_bin = binarizar_imagem(img2)
> ```
> ![[Pasted image 20251121102958.png]]

### Implementação das Operações

```python
import cv2
import numpy as np
import matplotlib.pyplot as plt

class OperacoesLogicas:
    def __init__(self):
        self.operacoes = {
            'AND': self.operacao_and,
            'OR': self.operacao_or,
            'XOR': self.operacao_xor,
            'NOT': self.operacao_not,
            'NAND': self.operacao_nand,
            'NOR': self.operacao_nor
        }
    
    def verificar_binaria(self, imagem):
        """Verifica se a imagem é binária (apenas 0 e 255)"""
        valores_unicos = np.unique(imagem)
        return set(valores_unicos).issubset({0, 255})
    
    def operacao_and(self, img1, img2):
        """Operação AND: Resultado é 255 apenas onde ambas imagens são 255"""
        if not self.verificar_binaria(img1) or not self.verificar_binaria(img2):
            raise ValueError("Imagens devem ser binárias (0 ou 255)")
        
        # Converter para boolean (True/False) para operação lógica
        img1_bool = img1 == 255
        img2_bool = img2 == 255
        
        resultado_bool = np.logical_and(img1_bool, img2_bool)
        return resultado_bool.astype(np.uint8) * 255
    
    def operacao_or(self, img1, img2):
        """Operação OR: Resultado é 255 onde pelo menos uma imagem é 255"""
        img1_bool = img1 == 255
        img2_bool = img2 == 255
        
        resultado_bool = np.logical_or(img1_bool, img2_bool)
        return resultado_bool.astype(np.uint8) * 255
    
    def operacao_xor(self, img1, img2):
        """Operação XOR: Resultado é 255 onde apenas uma imagem é 255"""
        img1_bool = img1 == 255
        img2_bool = img2 == 255
        
        resultado_bool = np.logical_xor(img1_bool, img2_bool)
        return resultado_bool.astype(np.uint8) * 255
    
    def operacao_not(self, img1):
        """Operação NOT: Inverte os valores (255→0, 0→255)"""
        return cv2.bitwise_not(img1)
    
    def operacao_nand(self, img1, img2):
        """Operação NAND: NOT AND"""
        and_result = self.operacao_and(img1, img2)
        return self.operacao_not(and_result)
    
    def operacao_nor(self, img1, img2):
        """Operação NOR: NOT OR"""
        or_result = self.operacao_or(img1, img2)
        return self.operacao_not(or_result)
    
    def aplicar_operacao(self, img1, img2, operacao):
        """Aplica uma operação específica"""
        if operacao.upper() in self.operacoes:
            return self.operacoes[operacao.upper()](img1, img2)
        else:
            raise ValueError(f"Operação {operacao} não suportada")

# Exemplo de uso
operador = OperacoesLogicas()
```

---

## 💡 Aplicações Práticas

### 1. Detecção de Mudanças com XOR

> [!SUCCESS] Aplicação de Vigilância
> **XOR para detectar pixels que mudaram:**

```python
def detectar_mudancas_xor(imagem1, imagem2, limiar_binarizacao=127):
    """
    Detecta mudanças entre duas imagens usando XOR
    """
    # Binarizar imagens
    _, img1_bin = cv2.threshold(imagem1, limiar_binarizacao, 255, cv2.THRESH_BINARY)
    _, img2_bin = cv2.threshold(imagem2, limiar_binarizacao, 255, cv2.THRESH_BINARY)
    
    # Aplicar XOR - pixels que mudaram serão brancos
    mudancas = operador.operacao_xor(img1_bin, img2_bin)
    
    return mudancas, img1_bin, img2_bin

# Criar imagens de exemplo
def criar_imagens_exemplo():
    """Cria imagens binárias de exemplo para demonstração"""
    # Imagem 1: Círculo no centro
    img1 = np.zeros((300, 300), dtype=np.uint8)
    cv2.circle(img1, (150, 150), 50, 255, -1)
    
    # Imagem 2: Círculo deslocado
    img2 = np.zeros((300, 300), dtype=np.uint8)
    cv2.circle(img2, (180, 150), 50, 255, -1)
    
    return img1, img2

# Testar detecção de mudanças
img1, img2 = criar_imagens_exemplo()
mudancas, img1_bin, img2_bin = detectar_mudancas_xor(img1, img2)

# Visualizar
fig, axes = plt.subplots(1, 4, figsize=(16, 4))
axes[0].imshow(img1, cmap='gray')
axes[0].set_title('Imagem 1')
axes[0].axis('off')

axes[1].imshow(img2, cmap='gray')
axes[1].set_title('Imagem 2')
axes[1].axis('off')

axes[2].imshow(mudancas, cmap='gray')
axes[2].set_title('Mudanças (XOR)')
axes[2].axis('off')

# Destacar áreas que mudaram
img_color = cv2.cvtColor(img1, cv2.COLOR_GRAY2BGR)
img_color[mudancas == 255] = [0, 0, 255]  # Vermelho para mudanças
axes[3].imshow(cv2.cvtColor(img_color, cv2.COLOR_BGR2RGB))
axes[3].set_title('Mudanças Destacadas')
axes[3].axis('off')

plt.tight_layout()
plt.show()
```

### 2. Máscaras Compostas com AND/OR

> [!EXAMPLE] Segmentação Avançada
> **Combinar múltiplas máscaras usando operações lógicas:**

```python
def criar_mascaras_geometricas(forma):
    """
    Cria diferentes máscaras geométricas para demonstração
    """
    altura, largura = forma
    
    # Máscara circular
    mascara_circular = np.zeros(forma, dtype=np.uint8)
    cv2.circle(mascara_circular, (largura//2, altura//2), 80, 255, -1)
    
    # Máscara retangular
    mascara_retangular = np.zeros(forma, dtype=np.uint8)
    cv2.rectangle(mascara_retangular, (50, 50), (200, 200), 255, -1)
    
    # Máscara triangular
    mascara_triangular = np.zeros(forma, dtype=np.uint8)
    pontos = np.array([[150, 50], [50, 250], [250, 250]], np.int32)
    cv2.fillPoly(mascara_triangular, [pontos], 255)
    
    return mascara_circular, mascara_retangular, mascara_triangular

# Criar máscaras
forma = (300, 300)
circular, retangular, triangular = criar_mascaras_geometricas(forma)

# Aplicar operações lógicas
resultado_and = operador.operacao_and(circular, retangular)
resultado_or = operador.operacao_or(circular, triangular)
resultado_xor = operador.operacao_xor(retangular, triangular)

# Visualizar combinações
fig, axes = plt.subplots(2, 4, figsize=(16, 8))

# Linha 1: Máscaras individuais
axes[0, 0].imshow(circular, cmap='gray')
axes[0, 0].set_title('Máscara Circular')
axes[0, 0].axis('off')

axes[0, 1].imshow(retangular, cmap='gray')
axes[0, 1].set_title('Máscara Retangular')
axes[0, 1].axis('off')

axes[0, 2].imshow(triangular, cmap='gray')
axes[0, 2].set_title('Máscara Triangular')
axes[0, 2].axis('off')

axes[0, 3].axis('off')

# Linha 2: Combinações
axes[1, 0].imshow(resultado_and, cmap='gray')
axes[1, 0].set_title('Circular AND Retangular\n(Interseção)')
axes[1, 0].axis('off')

axes[1, 1].imshow(resultado_or, cmap='gray')
axes[1, 1].set_title('Circular OR Triangular\n(União)')
axes[1, 1].axis('off')

axes[1, 2].imshow(resultado_xor, cmap='gray')
axes[1, 2].set_title('Retangular XOR Triangular\n(Diferença)')
axes[1, 2].axis('off')

# Combinação tripla
resultado_triplo = operador.operacao_and(
    operador.operacao_or(circular, retangular), 
    operador.operacao_not(triangular)
)
axes[1, 3].imshow(resultado_triplo, cmap='gray')
axes[1, 3].set_title('(Circular OR Retangular)\nAND NOT Triangular')
axes[1, 3].axis('off')

plt.tight_layout()
plt.show()
```

### 3. Operações Morfológicas com Lógica

> [!SUCCESS] Processamento de Formas
> **Implementar operações morfológicas usando operações lógicas:**

```python
def erosao_logica(imagem, kernel_size=3):
    """
    Implementa erosão usando operações lógicas AND
    """
    if not operador.verificar_binaria(imagem):
        raise ValueError("Imagem deve ser binária")
    
    kernel = np.ones((kernel_size, kernel_size), np.uint8) * 255
    margem = kernel_size // 2
    
    resultado = np.zeros_like(imagem)
    altura, largura = imagem.shape
    
    for i in range(margem, altura - margem):
        for j in range(margem, largura - margem):
            # Extrair região de interesse
            regiao = imagem[i-margem:i+margem+1, j-margem:j+margem+1]
            
            # Aplicar AND entre região e kernel
            and_result = np.logical_and(regiao == 255, kernel == 255)
            
            # Se todos os pixels sob o kernel forem 255, resultado é 255
            if np.all(and_result):
                resultado[i, j] = 255
    
    return resultado

def dilatacao_logica(imagem, kernel_size=3):
    """
    Implementa dilatação usando operações lógicas OR
    """
    kernel = np.ones((kernel_size, kernel_size), np.uint8) * 255
    margem = kernel_size // 2
    
    resultado = np.zeros_like(imagem)
    altura, largura = imagem.shape
    
    for i in range(margem, altura - margem):
        for j in range(margem, largura - margem):
            # Extrair região de interesse
            regiao = imagem[i-margem:i+margem+1, j-margem:j+margem+1]
            
            # Aplicar AND entre região e kernel
            and_result = np.logical_and(regiao == 255, kernel == 255)
            
            # Se algum pixel sob o kernel for 255, resultado é 255
            if np.any(and_result):
                resultado[i, j] = 255
    
    return resultado

# Testar operações morfológicas
# Criar imagem de teste com ruído
imagem_teste = np.zeros((200, 200), dtype=np.uint8)
cv2.rectangle(imagem_teste, (50, 50), (150, 150), 255, -1)

# Adicionar ruído (pixels isolados)
imagem_ruidosa = imagem_teste.copy()
imagem_ruidosa[30:35, 30:35] = 255  # Ruído externo
imagem_ruidosa[120:125, 80:85] = 0   # Buraco interno

# Aplicar operações
erosao_result = erosao_logica(imagem_ruidosa, 5)
dilatacao_result = dilatacao_logica(imagem_ruidosa, 5)

# Visualizar
fig, axes = plt.subplots(1, 4, figsize=(16, 4))
axes[0].imshow(imagem_teste, cmap='gray')
axes[0].set_title('Imagem Original Limpa')
axes[0].axis('off')

axes[1].imshow(imagem_ruidosa, cmap='gray')
axes[1].set_title('Imagem com Ruído')
axes[1].axis('off')

axes[2].imshow(erosao_result, cmap='gray')
axes[2].set_title('Após Erosão\n(Remove ruído)')
axes[2].axis('off')

axes[3].imshow(dilatacao_result, cmap='gray')
axes[3].set_title('Após Dilatação\n(Preenche buracos)')
axes[3].axis('off')

plt.tight_layout()
plt.show()
```

### 4. Análise de Sobreposição com AND

> [!EXAMPLE] Detecção de Colisão/Interseção
> **Usar AND para detectar sobreposição de objetos:**

```python
def analisar_sobreposicao_objetos(objeto1, objeto2):
    """
    Analisa sobreposição entre dois objetos usando operações lógicas
    """
    # Área de cada objeto
    area_obj1 = np.sum(objeto1 == 255)
    area_obj2 = np.sum(objeto2 == 255)
    
    # Interseção (AND)
    interseccao = operador.operacao_and(objeto1, objeto2)
    area_interseccao = np.sum(interseccao == 255)
    
    # União (OR)
    uniao = operador.operacao_or(objeto1, objeto2)
    area_uniao = np.sum(uniao == 255)
    
    # Calcular métricas
    iou = area_interseccao / area_uniao if area_uniao > 0 else 0
    sobreposicao_obj1 = area_interseccao / area_obj1 if area_obj1 > 0 else 0
    sobreposicao_obj2 = area_interseccao / area_obj2 if area_obj2 > 0 else 0
    
    return {
        'interseccao': interseccao,
        'uniao': uniao,
        'iou': iou,
        'sobreposicao_obj1': sobreposicao_obj1,
        'sobreposicao_obj2': sobreposicao_obj2,
        'area_interseccao': area_interseccao
    }

# Criar objetos de exemplo em movimento
def simular_movimento_objetos():
    """Simula movimento de objetos para análise de sobreposição"""
    frames = []
    forma = (200, 200)
    
    # Objeto 1: se move da esquerda para direita
    for x in range(30, 120, 10):
        obj1 = np.zeros(forma, dtype=np.uint8)
        cv2.circle(obj1, (x, 100), 25, 255, -1)
        frames.append(obj1)
    
    # Objeto 2: se move de cima para baixo
    for y in range(30, 170, 10):
        obj2 = np.zeros(forma, dtype=np.uint8)
        cv2.rectangle(obj2, (100, y), (150, y+30), 255, -1)
        frames.append(obj2)
    
    return frames

# Analisar sobreposição ao longo do tempo
objetos_movel = simular_movimento_objetos()
resultados_sobreposicao = []

for i, objeto in enumerate(objetos_movel[:8]):  # Analisar primeiros 8 frames
    # Usar objeto fixo para comparação
    objeto_fixo = np.zeros((200, 200), dtype=np.uint8)
    cv2.circle(objeto_fixo, (100, 100), 30, 255, -1)
    
    analise = analisar_sobreposicao_objetos(objeto_fixo, objeto)
    resultados_sobreposicao.append((i, analise))

# Visualizar análise de sobreposição
fig, axes = plt.subplots(2, 4, figsize=(16, 8))
for idx, (i, analise) in enumerate(resultados_sobreposicao):
    linha = idx // 4
    coluna = idx % 4
    
    # Mostrar interseção
    axes[linha, coluna].imshow(analise['interseccao'], cmap='gray')
    axes[linha, coluna].set_title(f'Frame {i}\nIoU: {analise["iou"]:.2f}')
    axes[linha, coluna].axis('off')

plt.tight_layout()
plt.show()

# Gráfico de métricas de sobreposição
frames = [r[0] for r in resultados_sobreposicao]
ious = [r[1]['iou'] for r in resultados_sobreposicao]
areas = [r[1]['area_interseccao'] for r in resultados_sobreposicao]

plt.figure(figsize=(12, 4))
plt.subplot(1, 2, 1)
plt.plot(frames, ious, 'bo-', linewidth=2, markersize=8)
plt.xlabel('Frame')
plt.ylabel('IoU (Intersection over Union)')
plt.title('Evolução da Sobreposição (IoU)')
plt.grid(True, alpha=0.3)

plt.subplot(1, 2, 2)
plt.plot(frames, areas, 'ro-', linewidth=2, markersize=8)
plt.xlabel('Frame')
plt.ylabel('Área de Interseção (pixels)')
plt.title('Área de Sobreposição')
plt.grid(True, alpha=0.3)

plt.tight_layout()
plt.show()
```

### 5. Sistema de Regras com Operações Lógicas

> [!SUCCESS] Lógica Combinacional em Imagens
> **Implementar sistemas de decisão baseados em múltiplas condições:**

```python
class SistemaRegrasBinario:
    def __init__(self):
        self.regras = {}
    
    def adicionar_regra(self, nome, expressao):
        """Adiciona uma regra baseada em operações lógicas"""
        self.regras[nome] = expressao
    
    def avaliar_regras(self, mascaras):
        """Avalia todas as regras com as máscaras fornecidas"""
        resultados = {}
        
        for nome, expressao in self.regras.items():
            try:
                # Avaliar expressão no contexto das máscaras
                resultado = eval(expressao, {'__builtins__': None}, mascaras)
                resultados[nome] = resultado
            except Exception as e:
                print(f"Erro na regra {nome}: {e}")
        
        return resultados
    
    def combinar_regras(self, resultados, expressao_combinacao):
        """Combina resultados de múltiplas regras"""
        try:
            return eval(expressao_combinacao, {'__builtins__': None}, resultados)
        except Exception as e:
            print(f"Erro na combinação: {e}")
            return None

# Exemplo: Sistema de detecção de formas
sistema = SistemaRegrasBinario()

# Criar máscaras de características
forma = (300, 300)
mascaras = {}

# Característica 1: Objeto grande
mascaras['grande'] = np.zeros(forma, dtype=np.uint8)
cv2.circle(mascaras['grande'], (150, 150), 80, 255, -1)

# Característica 2: Objeto central
mascaras['central'] = np.zeros(forma, dtype=np.uint8)
cv2.rectangle(mascaras['central'], (100, 100), (200, 200), 255, -1)

# Característica 3: Objeto alongado
mascaras['alongado'] = np.zeros(forma, dtype=np.uint8)
cv2.rectangle(mascaras['alongado'], (50, 120), (250, 180), 255, -1)

# Característica 4: Cantos preenchidos
mascaras['cantos'] = np.zeros(forma, dtype=np.uint8)
cv2.rectangle(mascaras['cantos'], (0, 0), (50, 50), 255, -1)
cv2.rectangle(mascaras['cantos'], (250, 0), (300, 50), 255, -1)

# Definir regras
sistema.adicionar_regra('regra_circular', 'grande AND NOT alongado')
sistema.adicionar_regra('regra_retangular', 'alongado AND central')
sistema.adicionar_regra('regra_complexa', '(grande OR central) AND NOT cantos')

# Avaliar regras
resultados = sistema.avaliar_regras(mascaras)

# Combinar regras
resultado_final = sistema.combinar_regras(
    resultados, 
    'regra_circular OR regra_retangular OR regra_complexa'
)

# Visualizar sistema de regras
fig, axes = plt.subplots(2, 4, figsize=(16, 8))

# Linha 1: Características individuais
caracteristicas = list(mascaras.keys())
for i, carac in enumerate(caracteristicas):
    axes[0, i].imshow(mascaras[carac], cmap='gray')
    axes[0, i].set_title(f'Característica: {carac}')
    axes[0, i].axis('off')

# Linha 2: Resultados das regras
for i, (nome_regra, resultado) in enumerate(list(resultados.items())[:3]):
    axes[1, i].imshow(resultado, cmap='gray')
    axes[1, i].set_title(f'Regra: {nome_regra}')
    axes[1, i].axis('off')

# Resultado final
if resultado_final is not None:
    axes[1, 3].imshow(resultado_final, cmap='gray')
    axes[1, 3].set_title('Combinação Final')
else:
    axes[1, 3].axis('off')

plt.tight_layout()
plt.show()
```

---

## 🖥️ Implementação Completa com OpenCV

```python
import cv2
import numpy as np

class OperacoesLogicasOpenCV:
    """
    Versão usando funções nativas do OpenCV para melhor performance
    """
    
    @staticmethod
    def and_cv2(img1, img2):
        """Operação AND usando OpenCV"""
        return cv2.bitwise_and(img1, img2)
    
    @staticmethod
    def or_cv2(img1, img2):
        """Operação OR usando OpenCV"""
        return cv2.bitwise_or(img1, img2)
    
    @staticmethod
    def xor_cv2(img1, img2):
        """Operação XOR usando OpenCV"""
        return cv2.bitwise_xor(img1, img2)
    
    @staticmethod
    def not_cv2(img1):
        """Operação NOT usando OpenCV"""
        return cv2.bitwise_not(img1)
    
    @staticmethod
    def aplicar_mascara(imagem, mascara):
        """Aplica máscara usando operação AND"""
        return cv2.bitwise_and(imagem, imagem, mask=mascara)
    
    @staticmethod
    def combinar_imagens(imagem1, imagem2, mascara):
        """Combina imagens usando máscara"""
        # imagem1 onde máscara é branca, imagem2 onde máscara é preta
        fundo = cv2.bitwise_and(imagem1, imagem1, mask=cv2.bitwise_not(mascara))
        primeiro_plano = cv2.bitwise_and(imagem2, imagem2, mask=mascara)
        return cv2.add(fundo, primeiro_plano)

# Exemplo de performance
def comparar_performance():
    """Compara performance entre implementação NumPy e OpenCV"""
    forma = (1000, 1000)
    img1 = np.random.choice([0, 255], forma).astype(np.uint8)
    img2 = np.random.choice([0, 255], forma).astype(np.uint8)
    
    operador_numpy = OperacoesLogicas()
    operador_opencv = OperacoesLogicasOpenCV()
    
    # Teste AND
    import time
    
    inicio = time.time()
    resultado_numpy = operador_numpy.operacao_and(img1, img2)
    tempo_numpy = time.time() - inicio
    
    inicio = time.time()
    resultado_opencv = operador_opencv.and_cv2(img1, img2)
    tempo_opencv = time.time() - inicio
    
    print(f"AND - NumPy: {tempo_numpy:.4f}s, OpenCV: {tempo_opencv:.4f}s")
    print(f"OpenCV é {tempo_numpy/tempo_opencv:.1f}x mais rápido")

comparar_performance()
```

---

## ⚠️ Considerações Importantes

> [!WARNING] Limitações e Cuidados
> 1. **Imagens devem ser binárias** - valores diferentes de 0/255 causam problemas
> 2. **Mesmo tamanho** - operações requerem imagens de dimensões idênticas
> 3. **Performance** - implementações NumPy podem ser lentas para imagens grandes
> 4. **Interpretação** - resultados podem precisar de pós-processamento

> [!TIP] Boas Práticas
> 1. **Sempre verifique** se as imagens são binárias antes das operações
> 2. **Use OpenCV** para melhor performance em operações básicas
> 3. **Documente as regras** em sistemas complexos de lógica
> 4. **Teste com casos simples** antes de aplicar a problemas complexos

---

## 📊 Tabela de Verdade para Operações Lógicas

| A | B | AND | OR | XOR | NAND | NOR |
|---|---|-----|----|-----|------|-----|
| 0 | 0 | 0 | 0 | 0 | 1 | 1 |
| 0 | 1 | 0 | 1 | 1 | 1 | 0 |
| 1 | 0 | 0 | 1 | 1 | 1 | 0 |
| 1 | 1 | 1 | 1 | 0 | 0 | 0 |

> [!SUMMARY] Conclusão
> Operações lógicas em imagens binárias são fundamentais para:
> - **Análise de mudanças** através de XOR entre frames
> - **Combinação de máscaras** usando AND/OR para segmentação complexa
> - **Operações morfológicas** como erosão e dilatação
> - **Sistemas de regras** para classificação e detecção
> - **Análise de sobreposição** e detecção de colisões
> 
> Estas operações formam a base para técnicas mais avançadas de processamento de imagens e visão computacional, oferecendo controle preciso sobre pixels binários através da álgebra booleana.