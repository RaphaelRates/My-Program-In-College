# Sistemas de Coordenadas em Processamento de Imagens

> [!NOTE] Conceito Fundamental
> As imagens digitais usam sistemas de coordenadas cartesianas onde:
> - **Eixo X**: Horizontal (colunas)
> - **Eixo Y**: Vertical (linhas)
> - **Origem (0,0)**: Canto superior esquerdo
> - **Coordenadas**: `(y, x)` em NumPy / `(x, y)` em sistemas matemáticos

## 📋 Navegação em Imagens com Loops Aninhados

### Percorrendo Todos os Pixels da Imagem

```python
import cv2
import numpy as np

def percorrer_imagem_pixel_a_pixel(imagem):
    """
    Percorre cada pixel da imagem usando loops aninhados
    """
    altura, largura = imagem.shape[:2]
    print(f"Imagem: {largura}x{altura} pixels")
    
    # CONTADOR: contar pixels claros (intensidade > 127)
    pixels_claros = 0
    
    for y in range(altura):          # Loop pelas LINHAS (eixo Y)
        for x in range(largura):     # Loop pelas COLUNAS (eixo X)
            # Acessar pixel na posição (y, x)
            intensidade = imagem[y, x]
            
            # Exemplo: contar pixels claros
            if intensidade > 127:
                pixels_claros += 1
            
            # Exemplo: modificar pixels específicos
            if x == y:  # Diagonal principal
                imagem[y, x] = 255  # Branco
    
    print(f"Pixels claros: {pixels_claros}")
    print(f"Percentual: {(pixels_claros/(altura*largura))*100:.1f}%")
    return imagem

# Carregar e processar imagem
imagem = cv2.imread('documento.jpg', cv2.IMREAD_GRAYSCALE)
imagem_processada = percorrer_imagem_pixel_a_pixel(imagem.copy())
```

---

## 💡 Aplicações Práticas com Coordenadas

### 1. Detecção de Bordas em Documentos

```python
def detectar_bordas_documento(imagem, margem=20):
    """
    Encontra as bordas de um documento digitalizado
    Aplica-se: Scanner, processamento de documentos
    """
    altura, largura = imagem.shape
    bordas_detectadas = np.zeros_like(imagem)
    
    # Percorrer apenas as margens da imagem
    for y in range(altura):
        for x in range(largura):
            # Verificar se está nas bordas
            if (x < margem or x > largura - margem or 
                y < margem or y > altura - margem):
                
                # Se pixel é escuro na borda, provavelmente é borda do documento
                if imagem[y, x] < 100:
                    bordas_detectadas[y, x] = 255
    
    return bordas_detectadas

# Uso real:
# documento = cv2.imread('contrato.jpg', 0)
# bordas = detectar_bordas_documento(documento)
# if np.sum(bordas) > 1000:
#     print("Documento bem posicionado no scanner")
# else:
#     print("Ajustar posição do documento")
```

### 2. Análise de Gráficos e Chart Data

```python
def extrair_dados_grafico_barras(imagem_grafico):
    """
    Extrai valores de um gráfico de barras simples
    Aplica-se: Digitalização de relatórios, análise de dados
    """
    altura, largura = imagem_grafico.shape
    dados_barras = []
    
    # Área onde estão as barras (ajustar conforme o gráfico)
    y_base = altura - 50  # Base do gráfico
    x_inicio = 100
    x_fim = largura - 100
    
    # Amostrar a cada 20 pixels na horizontal
    for x in range(x_inicio, x_fim, 20):
        altura_barra = 0
        
        # Medir altura da barra de baixo para cima
        for y in range(y_base, 50, -1):
            if imagem_grafico[y, x] < 50:  # Pixel preto (barra)
                altura_barra = y_base - y
                break
        
        dados_barras.append(altura_barra)
    
    return dados_barras

# Uso em escritório:
# grafico = cv2.imread('vendas_trimestre.jpg', 0)
# valores = extrair_dados_grafico_barras(grafico)
# print(f"Dados extraídos: {valores}")
# # Converter para valores reais (escala do gráfico)
```

### 3. Sistema de Calibração de Câmera

```python
def calibrar_camera_por_angulo(imagem_calibracao):
    """
    Encontra pontos de calibração usando coordenadas
    Aplica-se: Visão computacional, robótica
    """
    altura, largura = imagem_calibracao.shape
    pontos_calibracao = []
    
    # Procurar marcadores de calibração (cruz ou círculo)
    for y in range(0, altura, 10):  # Amostrar a cada 10 pixels
        for x in range(0, largura, 10):
            
            # Verificar padrão de cruz (pixel central escuro)
            if (imagem_calibracao[y, x] < 50 and 
                imagem_calibracao[y-1, x] > 200 and 
                imagem_calibracao[y+1, x] > 200 and
                imagem_calibracao[y, x-1] > 200 and
                imagem_calibracao[y, x+1] > 200):
                
                pontos_calibracao.append((x, y))
    
    return pontos_calibracao

# Uso em fábrica:
# imagem = camera.capturar()
# pontos = calibrar_camera_por_angulo(imagem)
# if len(pontos) >= 4:
#     calcular_matriz_calibracao(pontos)
# else:
#     print("Marcadores de calibração não encontrados")
```

### 4. Leitor de Display de 7 Segmentos

```python
def ler_display_7_segmentos(imagem_display):
    """
    Lê números em displays de 7 segmentos (relógios, medidores)
    Aplica-se: Instrumentação industrial, automação
    """
    altura, largura = imagem_display.shape
    segmentos_ativos = [0] * 7  # [A, B, C, D, E, F, G]
    
    # Coordenadas aproximadas de cada segmento (ajustar conforme display)
    posicoes_segmentos = [
        (10, 5, 30, 10),    # Segmento A (topo)
        (35, 10, 40, 25),   # Segmento B (direita superior)
        (35, 30, 40, 45),   # Segmento C (direita inferior)
        (10, 45, 30, 50),   # Segmento D (base)
        (5, 30, 10, 45),    # Segmento E (esquerda inferior)
        (5, 10, 10, 25),    # Segmento F (esquerda superior)
        (10, 25, 30, 30)    # Segmento G (meio)
    ]
    
    # Verificar cada segmento
    for i, (x1, y1, x2, y2) in enumerate(posicoes_segmentos):
        pixels_ativos = 0
        
        # Percorrer área do segmento
        for y in range(y1, y2):
            for x in range(x1, x2):
                if 0 <= y < altura and 0 <= x < largura:
                    if imagem_display[y, x] > 200:  # Segmento aceso
                        pixels_ativos += 1
        
        # Se mais de 50% dos pixels estão acesos, segmento está ativo
        area_segmento = (x2 - x1) * (y2 - y1)
        if pixels_ativos > area_segmento * 0.5:
            segmentos_ativos[i] = 1
    
    # Mapear segmentos para números
    numeros = {
        (1,1,1,1,1,1,0): '0',
        (0,1,1,0,0,0,0): '1',
        (1,1,0,1,1,0,1): '2',
        (1,1,1,1,0,0,1): '3',
        (0,1,1,0,0,1,1): '4',
        (1,0,1,1,0,1,1): '5',
        (1,0,1,1,1,1,1): '6',
        (1,1,1,0,0,0,0): '7',
        (1,1,1,1,1,1,1): '8',
        (1,1,1,1,0,1,1): '9'
    }
    
    return numeros.get(tuple(segmentos_ativos), '?')

# Uso em indústria:
# display = cv2.imread('medidor_pressao.jpg', 0)
# numero = ler_display_7_segmentos(display)
# print(f"Leitura: {numero}")
# registrar_no_sistema(numero)
```

---

## 🛠️ Técnicas Avançadas de Navegação

### Navegação por Regiões de Interesse (ROI)

```python
def processar_regioes_interesse(imagem, regioes):
    """
    Processa apenas regiões específicas da imagem
    """
    resultados = {}
    
    for nome, (x, y, w, h) in regioes.items():
        # Extrair ROI (Region of Interest)
        roi = imagem[y:y+h, x:x+w]
        
        # Processar apenas essa região
        estatisticas = analisar_roi(roi)
        resultados[nome] = estatisticas
    
    return resultados

def analisar_roi(roi):
    """
    Analisa uma região específica da imagem
    """
    altura, largura = roi.shape
    total_pixels = altura * largura
    
    claros = escuros = 0
    for y in range(altura):
        for x in range(largura):
            if roi[y, x] > 127:
                claros += 1
            else:
                escuros += 1
    
    return {
        'percentual_claros': (claros/total_pixels)*100,
        'percentual_escuros': (escuros/total_pixels)*100,
        'intensidade_media': np.mean(roi)
    }

# Exemplo de uso:
# regioes = {
#     'assinatura': (100, 500, 200, 100),
#     'carimbo': (300, 400, 150, 150),
#     'codigo_barras': (50, 600, 300, 50)
# }
# resultados = processar_regioes_interesse(documento, regioes)
```

### Sistema de Coordenadas Relativas

```python
def criar_sistema_coordenadas_relativas(imagem, ponto_referencia):
    """
    Cria sistema de coordenadas relativo a um ponto de referência
    """
    altura, largura = imagem.shape
    x_ref, y_ref = ponto_referencia
    
    # Converter coordenadas absolutas para relativas
    def absoluta_para_relativa(x_abs, y_abs):
        x_rel = x_abs - x_ref
        y_rel = y_abs - y_ref
        return (x_rel, y_rel)
    
    def relativa_para_absoluta(x_rel, y_rel):
        x_abs = x_rel + x_ref
        y_abs = y_rel + y_ref
        return (x_abs, y_abs)
    
    return absoluta_para_relativa, relativa_para_absoluta

# Uso em rastreamento:
# referencia = (320, 240)  # Centro da imagem 640x480
# para_relativa, para_absoluta = criar_sistema_coordenadas_relativas(imagem, referencia)
# 
# objeto_x, objeto_y = 400, 300  # Coordenadas absolutas
# rel_x, rel_y = para_relativa(objeto_x, objeto_y)
# print(f"Objeto em: ({rel_x}, {rel_y}) relativo ao centro")
```

---

## 📊 Resumo de Aplicações por Setor

| Setor | Aplicação | Técnica de Coordenadas |
|-------|-----------|------------------------|
| **Documentos** | Verificar assinaturas | ROI + Loop em áreas específicas |
| **Indústria** | Ler displays | Mapeamento por segmentos |
| **Scanner** | Detectar bordas | Loop nas margens da imagem |
| **Automação** | Calibrar câmeras | Busca por padrões geométricos |

> [!TIP] Boas Práticas
> 1. **Evite loops completos** em imagens grandes - use NumPy vectorization quando possível
> 2. **Use ROIs** para processar apenas áreas relevantes
> 3. **Cache coordenadas** que serão reutilizadas
> 4. **Valide limites** para evitar acessos fora da imagem

> [!SUMMARY] Conclusão
> Dominar sistemas de coordenadas permite:
> - **Navegação precisa** por pixels específicos
> - **Processamento eficiente** de regiões de interesse
> - **Automação inteligente** baseada em posicionamento
> - **Integração com sistemas** físicos através de coordenadas mapeadas
> 
> O loop duplo `for y in range(altura): for x in range(largura):` é a base para qualquer processamento pixel-a-pixel personalizado. # Sistemas de Coordenadas Polares em Processamento de Imagens

> [!NOTE] Conceito Fundamental
> **Conversão Cartesiano → Polar**:
> - Cartesianas: `(x, y)` 
> - Polares: `(r, θ)`
> - `r = √(x² + y²)` → distância do centro
> - `θ = atan2(y, x)` → ângulo em radianos

## 🔄 Conversão entre Sistemas de Coordenadas

### Funções Básicas de Conversão

```python
import cv2
import numpy as np
import math

def cartesiano_para_polar(x, y, centro_x, centro_y):
    """
    Converte coordenadas cartesianas para polares
    """
    # Coordenadas relativas ao centro
    dx = x - centro_x
    dy = y - centro_y
    
    # Calcular raio e ângulo
    r = math.sqrt(dx**2 + dy**2)
    theta = math.atan2(dy, dx)  # ângulo em radianos [-π, π]
    
    return r, theta

def polar_para_cartesiano(r, theta, centro_x, centro_y):
    """
    Converte coordenadas polares para cartesianas
    """
    x = centro_x + r * math.cos(theta)
    y = centro_y + r * math.sin(theta)
    
    return int(x), int(y)

def imagem_para_polar(imagem, centro=None, raio_maximo=None):
    """
    Converte imagem inteira para coordenadas polares
    """
    altura, largura = imagem.shape[:2]
    
    if centro is None:
        centro = (largura // 2, altura // 2)
    
    if raio_maximo is None:
        raio_maximo = min(centro[0], centro[1], largura - centro[0], altura - centro[1])
    
    # Criar imagem de saída (ângulo x raio)
    imagem_polar = np.zeros((raio_maximo, 360), dtype=imagem.dtype)
    
    for theta_graus in range(360):
        theta_rad = math.radians(theta_graus)
        for r in range(raio_maximo):
            # Converter polar → cartesiano
            x = centro[0] + r * math.cos(theta_rad)
            y = centro[1] + r * math.sin(theta_rad)
            
            # Verificar limites e interpolar
            if 0 <= x < largura and 0 <= y < altura:
                imagem_polar[r, theta_graus] = imagem[int(y), int(x)]
    
    return imagem_polar
```

---

## 💡 Aplicações Práticas com Coordenadas Polares

### 1. Análise de Padrões Circulares

```python
def analisar_anéis_concentricos(imagem):
    """
    Analisa padrões circulares como anéis de árvores, alvos, discos
    Aplica-se: Biologia, controle de qualidade, balística
    """
    altura, largura = imagem.shape
    centro = (largura // 2, altura // 2)
    raio_maximo = min(centro[0], centro[1])
    
    # Perfil radial (intensidade média por raio)
    perfil_radial = []
    
    for r in range(raio_maximo):
        intensidades = []
        
        # Amostrar vários ângulos para cada raio
        for theta_graus in range(0, 360, 10):  # Amostrar a cada 10 graus
            theta_rad = math.radians(theta_graus)
            x = centro[0] + r * math.cos(theta_rad)
            y = centro[1] + r * math.sin(theta_rad)
            
            if 0 <= x < largura and 0 <= y < altura:
                intensidades.append(imagem[int(y), int(x)])
        
        if intensidades:
            perfil_radial.append(np.mean(intensidades))
    
    return perfil_radial

# Uso em análise de madeira:
# anel_madeira = cv2.imread('corte_arvore.jpg', 0)
# perfil = analisar_anéis_concentricos(anel_madeira)
# picos = encontrar_picos(perfil)  # Cada pico = um anel de crescimento
# idade_arvore = len(picos)
# print(f"Árvore com aproximadamente {idade_arvore} anos")
```

### 2. Inspeção de Peças Rotacionais

```python
def inspecionar_peca_circular(imagem_peca):
    """
    Inspeciona peças circulares (engrenagens, rolamentos, discos)
    Aplica-se: Indústria automotiva, manufatura
    """
    altura, largura = imagem_peca.shape
    centro = (largura // 2, altura // 2)
    
    # Encontrar defeitos em coordenadas polares
    defeitos = []
    
    # Converter para coordenadas polares
    for theta_graus in range(360):
        theta_rad = math.radians(theta_graus)
        
        # Medir intensidade ao longo do raio
        raios = []
        for r in range(50, 200):  # Raio de interesse
            x = centro[0] + r * math.cos(theta_rad)
            y = centro[1] + r * math.sin(theta_rad)
            
            if 0 <= x < largura and 0 <= y < altura:
                raios.append(imagem_peca[int(y), int(x)])
        
        # Verificar descontinuidades (possíveis trincas)
        if len(raios) > 10:
            variacao = np.std(raios)
            if variacao > 50:  # Alta variação = possível defeito
                defeitos.append(theta_graus)
    
    return defeitos

# Uso em linha de produção:
# engrenagem = cv2.imread('engrenagem.jpg', 0)
# angulos_defeito = inspecionar_peca_circular(engrenagem)
# if angulos_defeito:
#     print(f"Defeitos encontrados nos ângulos: {angulos_defeito}")
#     rejeitar_peca()
# else:
#     print("Peça aprovada")
```

### 3. Reconhecimento de Assinaturas Radiais

```python
def analisar_assinatura_radial(imagem, centro, raio_interno, raio_externo):
    """
    Analisa padrões radiais como raios de roda, pétalas de flor
    Aplica-se: Biologia, engenharia, astronomia
    """
    altura, largura = imagem.shape
    assinatura_angular = []
    
    # Para cada ângulo, calcular intensidade média no raio
    for theta_graus in range(360):
        theta_rad = math.radians(theta_graus)
        intensidades = []
        
        for r in range(raio_interno, raio_externo):
            x = centro[0] + r * math.cos(theta_rad)
            y = centro[1] + r * math.sin(theta_rad)
            
            if 0 <= x < largura and 0 <= y < altura:
                intensidades.append(imagem[int(y), int(x)])
        
        if intensidades:
            assinatura_angular.append(np.mean(intensidades))
    
    return assinatura_angular

def detectar_simetria_radial(assinatura_angular):
    """
    Detecta simetria radial através da assinatura angular
    """
    # Calcular autocorrelação para detectar periodicidade
    correlacao = np.correlate(assinatura_angular, assinatura_angular, mode='full')
    correlacao = correlacao[len(correlacao)//2:]
    
    # Encontrar picos (simetrias)
    picos = []
    for i in range(2, len(correlacao)-2):
        if (correlacao[i] > correlacao[i-1] and 
            correlacao[i] > correlacao[i+1] and
            correlacao[i] > np.mean(correlacao) * 1.2):
            picos.append(i)
    
    return picos

# Uso em botânica:
# flor = cv2.imread('girassol.jpg', 0)
# centro = encontrar_centro_flor(flor)
# assinatura = analisar_assinatura_radial(flor, centro, 50, 150)
# simetrias = detectar_simetria_radial(assinatura)
# print(f"Flor com {len(simetrias)} pétalas detectadas")
```

### 4. Alinhamento e Registro de Imagens

```python
def registrar_imagens_polar(imagem_referencia, imagem_alvo):
    """
    Registra/alinha imagens usando transformada polar
    Aplica-se: Medicina (ressonância), astronomia, satélites
    """
    altura, largura = imagem_referencia.shape
    centro = (largura // 2, altura // 2)
    
    # Converter ambas para coordenadas polares
    polar_ref = imagem_para_polar(imagem_referencia, centro)
    polar_alvo = imagem_para_polar(imagem_alvo, centro)
    
    # Encontrar melhor deslocamento angular
    melhor_angulo = 0
    melhor_correlacao = -1
    
    for deslocamento in range(360):
        # Deslocar circularmente a imagem polar
        polar_deslocada = np.roll(polar_alvo, deslocamento, axis=1)
        
        # Calcular correlação
        correlacao = np.correlate(polar_ref.flatten(), polar_deslocada.flatten())[0]
        
        if correlacao > melhor_correlacao:
            melhor_correlacao = correlacao
            melhor_angulo = deslocamento
    
    return melhor_angulo

def rotacionar_imagem(imagem, angulo_graus, centro=None):
    """
    Rotaciona imagem usando transformada polar
    """
    if centro is None:
        altura, largura = imagem.shape
        centro = (largura // 2, altura // 2)
    
    matriz_rotacao = cv2.getRotationMatrix2D(centro, angulo_graus, 1.0)
    imagem_rotacionada = cv2.warpAffine(imagem, matriz_rotacao, (imagem.shape[1], imagem.shape[0]))
    
    return imagem_rotacionada

# Uso em medicina:
# ressonancia_antes = cv2.imread('ressonancia_01.jpg', 0)
# ressonancia_depois = cv2.imread('ressonancia_02.jpg', 0)
# angulo_correcao = registrar_imagens_polar(ressonancia_antes, ressonancia_depois)
# ressonancia_alinhada = rotacionar_imagem(ressonancia_depois, -angulo_correcao)
# print(f"Imagem rotacionada {angulo_correcao} graus para alinhamento")
```

---

## 🛠️ Técnicas Avançadas com Coordenadas Polares

### Transformada de Hough para Círculos

```python
def detectar_circulos_personalizado(imagem, raio_min, raio_max):
    """
    Detecta círculos usando abordagem polar personalizada
    """
    altura, largura = imagem.shape
    bordas = cv2.Canny(imagem, 50, 150)
    
    # Acumulador 3D (x, y, raio)
    acumulador = np.zeros((altura, largura, raio_max - raio_min))
    
    for y in range(altura):
        for x in range(largura):
            if bordas[y, x] > 0:  # Ponto de borda
                # Para cada raio possível
                for r_idx, raio in enumerate(range(raio_min, raio_max)):
                    # Para cada ângulo no círculo
                    for theta_graus in range(0, 360, 5):  # Passo de 5 graus
                        theta_rad = math.radians(theta_graus)
                        
                        # Centro do círculo candidato
                        x_centro = int(x - raio * math.cos(theta_rad))
                        y_centro = int(y - raio * math.sin(theta_rad))
                        
                        if 0 <= x_centro < largura and 0 <= y_centro < altura:
                            acumulador[y_centro, x_centro, r_idx] += 1
    
    # Encontrar círculos com mais votos
    circulos = []
    limiar_votos = 30  # Ajustar conforme necessidade
    
    for r_idx, raio in enumerate(range(raio_min, raio_max)):
        for y in range(altura):
            for x in range(largura):
                if acumulador[y, x, r_idx] > limiar_votos:
                    circulos.append((x, y, raio))
    
    return circulos
```

### Análise de Textura Radial

```python
def analisar_textura_radial(imagem, centro, num_anéis=10, num_setores=36):
    """
    Analisa textura em coordenadas polares (anéis e setores)
    """
    altura, largura = imagem.shape
    raio_max = min(centro[0], centro[1], largura - centro[0], altura - centro[1])
    
    # Dividir em anéis concêntricos e setores
    matriz_textura = np.zeros((num_anéis, num_setores))
    
    for i_anel in range(num_anéis):
        raio_interno = (i_anel * raio_max) // num_anéis
        raio_externo = ((i_anel + 1) * raio_max) // num_anéis
        
        for i_setor in range(num_setores):
            angulo_inicio = (i_setor * 360) // num_setores
            angulo_fim = ((i_setor + 1) * 360) // num_setores
            
            # Coletar pixels deste setor/anel
            pixels_setor = []
            
            for r in range(raio_interno, raio_externo):
                for theta_graus in range(angulo_inicio, angulo_fim):
                    theta_rad = math.radians(theta_graus)
                    x = centro[0] + r * math.cos(theta_rad)
                    y = centro[1] + r * math.sin(theta_rad)
                    
                    if 0 <= x < largura and 0 <= y < altura:
                        pixels_setor.append(imagem[int(y), int(x)])
            
            if pixels_setor:
                # Calcular entropia como medida de textura
                hist = np.histogram(pixels_setor, bins=10, range=(0, 255))[0]
                hist = hist / np.sum(hist)
                entropia = -np.sum(hist * np.log2(hist + 1e-8))
                matriz_textura[i_anel, i_setor] = entropia
    
    return matriz_textura
```

---

## 📊 Vantagens das Coordenadas Polares

| Problema | Solução Cartesianas | Solução Polares | Vantagem |
|----------|-------------------|-----------------|----------|
| **Análise circular** | Complexa | Natural | Simplificação |
| **Rotação** | Reamostragem | Deslocamento circular | Eficiência |
| **Simetria radial** | Difícil | Fácil detecção | Robustez |
| **Padrões angulares** | Complexos | Diretos | Intuição |

> [!TIP] Quando Usar Coordenadas Polares
> 1. **Objetos circulares** ou com simetria radial
> 2. **Análise de rotação** e alinhamento
> 3. **Padrões periódicos** angulares
> 4. **Operações invariantes** a rotação

> [!WARNING] Limitações
> 1. **Não isotrópico** - resolução varia com o raio
> 2. **Singularidade** no centro
> 3. **Computacionalmente custoso** para conversões
> 4. **Complexidade** em implementações

> [!SUMMARY] Conclusão
> Coordenadas polares são essenciais quando:
> - **A geometria do problema é circular**
> - **Precisa-se de invariância a rotação**
> - **Padrões angulares são mais relevantes** que lineares
> - **Análise radial** é mais intuitiva
> 
> A conversão entre sistemas permite escolher a representação mais adequada para cada aplicação específica em processamento de imagens.
> # Sistemas de Coordenadas Cilíndricas em Processamento de Imagens

> [!NOTE] Conceito Fundamental
> **Conversão Cartesiano → Cilíndrico**:
> - Cartesianas: `(x, y, z)` ou `(x, y)` em 2D + profundidade
> - Cilíndricas: `(r, θ, z)`
> - `r = √(x² + y²)` → raio do cilindro
> - `θ = atan2(y, x)` → ângulo azimutal
> - `z` → altura (mantida da coordenada cartesiana)

## 🔄 Conversão entre Sistemas de Coordenadas

### Funções Básicas de Conversão

```python
import cv2
import numpy as np
import math

def cartesiano_para_cilindrico(x, y, z, centro_x, centro_y):
    """
    Converte coordenadas cartesianas para cilíndricas
    """
    # Coordenadas relativas ao eixo do cilindro
    dx = x - centro_x
    dy = y - centro_y
    
    # Calcular raio e ângulo
    r = math.sqrt(dx**2 + dy**2)
    theta = math.atan2(dy, dx)  # ângulo em radianos [-π, π]
    
    return r, theta, z

def cilindrico_para_cartesiano(r, theta, z, centro_x, centro_y):
    """
    Converte coordenadas cilíndricas para cartesianas
    """
    x = centro_x + r * math.cos(theta)
    y = centro_y + r * math.sin(theta)
    
    return int(x), int(y), z

def imagem_para_cilindrico(imagem, centro=None, raio_maximo=None):
    """
    Converte imagem 2D para projeção cilíndrica
    Útil para "desenrolar" objetos cilíndricos
    """
    altura, largura = imagem.shape[:2]
    
    if centro is None:
        centro = (largura // 2, altura // 2)
    
    if raio_maximo is None:
        raio_maximo = min(centro[0], centro[1], largura - centro[0], altura - centro[1])
    
    # Criar imagem de saída (altura x ângulo)
    # Assumindo que a altura (z) é a mesma da imagem original
    imagem_cilindrica = np.zeros((altura, 360), dtype=imagem.dtype)
    
    for theta_graus in range(360):
        theta_rad = math.radians(theta_graus)
        for z in range(altura):  # z = altura na imagem original
            # Para cada altura, calcular posição no cilindro
            x = centro[0] + raio_maximo * math.cos(theta_rad)
            y = centro[1] + raio_maximo * math.sin(theta_rad)
            
            # Projeção: mapear ponto do cilindro para imagem plana
            # Esta é uma simplificação - na prática depende da câmera
            if 0 <= x < largura and 0 <= y < altura:
                imagem_cilindrica[z, theta_graus] = imagem[z, int(x)]
    
    return imagem_cilindrica
```

---

## 💡 Aplicações Práticas com Coordenadas Cilíndricas

### 1. Inspeção de Latas e Embalagens Cilíndricas

```python
def inspecionar_lata_bebida(imagem_lata):
    """
    Inspeciona superfície lateral de latas cilíndricas
    Aplica-se: Indústria de bebidas, controle de qualidade
    """
    altura, largura = imagem_lata.shape
    
    # "Desenrolar" a superfície cilíndrica
    superficie_desenrolada = np.zeros((altura, 360), dtype=imagem_lata.dtype)
    centro_x = largura // 2
    raio = largura // 4  # Raio aproximado da lata
    
    for theta_graus in range(360):
        theta_rad = math.radians(theta_graus)
        for z in range(altura):
            # Mapear coordenada cilíndrica para cartesiana
            x = centro_x + raio * math.cos(theta_rad)
            y = z  # Altura mantida
            
            if 0 <= x < largura:
                superficie_desenrolada[z, theta_graus] = imagem_lata[y, int(x)]
    
    # Procurar defeitos na superfície desenrolada
    defeitos = []
    for z in range(altura):
        for theta_graus in range(360):
            if superficie_desenrolada[z, theta_graus] < 50:  # Pixel muito escuro
                # Verificar se é um defeito (área contínua)
                vizinhos_escuros = 0
                for dz in [-1, 0, 1]:
                    for dtheta in [-1, 0, 1]:
                        nz, ntheta = z + dz, (theta_graus + dtheta) % 360
                        if 0 <= nz < altura and superficie_desenrolada[nz, ntheta] < 50:
                            vizinhos_escuros += 1
                
                if vizinhos_escuros >= 3:  # Área de defeito
                    defeitos.append((theta_graus, z))
    
    return superficie_desenrolada, defeitos

# Uso em linha de produção:
# lata = cv2.imread('lata_refrigerante.jpg', 0)
# superficie, defeitos = inspecionar_lata_bebida(lata)
# if defeitos:
#     print(f"Defeitos encontrados: {len(defeitos)}")
#     rejeitar_lata()
# else:
#     print("Lata aprovada")
```

### 2. Análise de Colunas e Estruturas Arquitetônicas

```python
def analisar_coluna_arquitetonica(imagem_coluna, altura_coluna):
    """
    Analisa colunas cilíndricas em imagens arquitetônicas
    Aplica-se: Restauração, arquitetura, engenharia civil
    """
    altura, largura = imagem_coluna.shape
    
    # Criar modelo cilíndrico da coluna
    perfil_radial = np.zeros((altura_coluna, 360))
    centro_x = largura // 2
    raio_estimado = largura // 3
    
    for z in range(altura_coluna):
        for theta_graus in range(360):
            theta_rad = math.radians(theta_graus)
            
            # Posição na superfície da coluna
            x = centro_x + raio_estimado * math.cos(theta_rad)
            y = z * altura // altura_coluna  # Escalar altura
            
            if 0 <= x < largura and 0 <= y < altura:
                perfil_radial[z, theta_graus] = imagem_coluna[int(y), int(x)]
    
    # Analisar desgaste/erosão
    erosao_por_altura = []
    for z in range(altura_coluna):
        intensidade_media = np.mean(perfil_radial[z, :])
        erosao_por_altura.append(intensidade_media)
    
    # Identificar áreas com maior desgaste
    limiar_erosao = np.mean(erosao_por_altura) * 0.7
    areas_desgastadas = []
    
    for z, intensidade in enumerate(erosao_por_altura):
        if intensidade < limiar_erosao:
            areas_desgastadas.append(z)
    
    return perfil_radial, erosao_por_altura, areas_desgastadas

# Uso em restauração:
# coluna = cv2.imread('coluna_romana.jpg', 0)
# perfil, erosao, areas_desgaste = analisar_coluna_arquitetonica(coluna, altura_coluna=100)
# print(f"Áreas desgastadas: {len(areas_desgaste)}")
# priorizar_restauracao(areas_desgaste)
```

### 3. Processamento de Imagens Médicas (Tomografia)

```python
def reconstruir_tomografia_cilindrica(projecoes, num_angulos, raio_maximo):
    """
    Simula reconstrução tomográfica usando coordenadas cilíndricas
    Aplica-se: Medicina, tomografia computadorizada
    """
    # projecoes: lista de imagens de projeção em diferentes ângulos
    tamanho_imagem = 2 * raio_maximo + 1
    imagem_reconstruida = np.zeros((tamanho_imagem, tamanho_imagem))
    
    centro = raio_maximo, raio_maximo
    
    # Reconstrução por retroprojeção simples
    for i, projecao in enumerate(projecoes):
        angulo = 2 * math.pi * i / num_angulos
        
        for x in range(tamanho_imagem):
            for y in range(tamanho_imagem):
                # Calcular raio e ângulo do pixel
                dx = x - centro[0]
                dy = y - centro[1]
                r = math.sqrt(dx**2 + dy**2)
                
                if r <= raio_maximo:
                    # Encontrar posição na projeção
                    theta = math.atan2(dy, dx)
                    angulo_relativo = (theta - angulo) % (2 * math.pi)
                    
                    # Posição na projeção (simplificado)
                    pos_projecao = int((angulo_relativo / (2 * math.pi)) * len(projecao))
                    
                    if 0 <= pos_projecao < len(projecao):
                        imagem_reconstruida[y, x] += projecao[pos_projecao]
    
    # Normalizar
    imagem_reconstruida = imagem_reconstruida / len(projecoes)
    return imagem_reconstruida

def gerar_projecoes_sinteticas(objeto_3d, num_angulos=180):
    """
    Gera projeções 2D de um objeto 3D para simulação
    """
    projecoes = []
    
    for i in range(num_angulos):
        angulo = 2 * math.pi * i / num_angulos
        projecao = np.zeros(objeto_3d.shape[1])
        
        # Simular projeção (soma ao longo do eixo)
        for x in range(objeto_3d.shape[1]):
            for y in range(objeto_3d.shape[0]):
                # Rotacionar coordenadas
                x_rot = x * math.cos(angulo) - y * math.sin(angulo)
                if 0 <= x_rot < objeto_3d.shape[1]:
                    projecao[int(x_rot)] += objeto_3d[y, x]
        
        projecoes.append(projecao)
    
    return projecoes

# Uso em simulação médica:
# objeto = np.random.rand(100, 100)  # Objeto 2D de exemplo
# projecoes = gerar_projecoes_sinteticas(objeto)
# reconstrucao = reconstruir_tomografia_cilindrica(projecoes, num_angulos=180, raio_maximo=50)
# cv2.imshow('Reconstrução', reconstrucao)
```

### 4. Visão Robótica para Manipulação de Objetos Cilíndricos

```python
def calcular_ponto_agarre_cilindrico(nuvem_pontos, raio_objeto):
    """
    Calcula pontos ideais para agarre robótico em objetos cilíndricos
    Aplica-se: Robótica industrial, automação
    """
    # Assumindo nuvem_pontos como array Nx3 (x, y, z)
    pontos_validos = []
    
    for ponto in nuvem_pontos:
        x, y, z = ponto
        r = math.sqrt(x**2 + y**2)
        
        # Verificar se ponto está na superfície do cilindro
        if abs(r - raio_objeto) < 2:  # Tolerância de 2 unidades
            pontos_validos.append((x, y, z))
    
    if not pontos_validos:
        return None
    
    # Encontrar pontos opostos para agarre
    pontos_validos = np.array(pontos_validos)
    
    # Agrupar por altura (z)
    alturas_unicas = np.unique(pontos_validos[:, 2])
    melhores_agarres = []
    
    for altura in alturas_unicas:
        pontos_altura = pontos_validos[pontos_validos[:, 2] == altura]
        
        if len(pontos_altura) >= 2:
            # Encontrar par de pontos mais opostos
            melhor_par = None
            maior_distancia = 0
            
            for i in range(len(pontos_altura)):
                for j in range(i+1, len(pontos_altura)):
                    p1, p2 = pontos_altura[i], pontos_altura[j]
                    
                    # Calcular distância angular
                    theta1 = math.atan2(p1[1], p1[0])
                    theta2 = math.atan2(p2[1], p2[0])
                    diff_angular = abs(theta1 - theta2)
                    
                    # Normalizar para [0, pi]
                    if diff_angular > math.pi:
                        diff_angular = 2 * math.pi - diff_angular
                    
                    # Preferir pontos opostos (180 graus)
                    if abs(diff_angular - math.pi) < 0.2:  # ~180 graus
                        distancia = math.sqrt((p1[0]-p2[0])**2 + (p1[1]-p2[1])**2)
                        if distancia > maior_distancia:
                            maior_distancia = distancia
                            melhor_par = (p1, p2)
            
            if melhor_par:
                melhores_agarres.append((melhor_par[0], melhor_par[1], altura))
    
    return melhores_agarres

# Uso em robótica:
# nuvem_pontos = sensor_3d.capturar_nuvem_pontos()
# agarres = calcular_ponto_agarre_cilindrico(nuvem_pontos, raio_objeto=5.0)
# if agarres:
#     melhor_agarre = agarres[0]  # Usar o primeiro agarre encontrado
#     robo.mover_para_agarre(melhor_agarre[0], melhor_agarre[1])
```

---

## 🛠️ Técnicas Avançadas com Coordenadas Cilíndricas

### Mapeamento Textura para Superfícies Cilíndricas

```python
def mapear_textura_cilindro(imagem_textura, altura_cilindro, circunferencia):
    """
    Mapeia textura 2D para superfície cilíndrica 3D
    Aplica-se: Computação gráfica, realidade virtual
    """
    # Criar malha cilíndrica
    imagem_3d = np.zeros((altura_cilindro, circunferencia, 3), dtype=np.uint8)
    
    for z in range(altura_cilindro):
        for theta_graus in range(circunferencia):
            # Mapear coordenadas da textura
            u = theta_graus / circunferencia  # Coordenada U [0, 1]
            v = z / altura_cilindro           # Coordenada V [0, 1]
            
            # Amostrar textura
            x_textura = int(u * imagem_textura.shape[1])
            y_textura = int(v * imagem_textura.shape[0])
            
            if (0 <= x_textura < imagem_textura.shape[1] and 
                0 <= y_textura < imagem_textura.shape[0]):
                imagem_3d[z, theta_graus] = imagem_textura[y_textura, x_textura]
    
    return imagem_3d

def projetar_cilindro_para_2d(imagem_cilindrica, raio, distancia_camera):
    """
    Projeta superfície cilíndrica de volta para 2D
    """
    altura, largura, _ = imagem_cilindrica.shape
    imagem_2d = np.zeros((altura, largura, 3), dtype=np.uint8)
    
    for z in range(altura):
        for x in range(largura):
            # Calcular ângulo correspondente
            theta = 2 * math.pi * x / largura
            
            # Projeção perspectiva simples
            x_proj = distancia_camera * math.tan(theta)
            y_proj = z
            
            # Mapear para coordenadas de imagem
            x_img = int((x_proj + largura/2) % largura)
            y_img = int(y_proj)
            
            if 0 <= y_img < altura:
                imagem_2d[y_img, x_img] = imagem_cilindrica[z, x]
    
    return imagem_2d
```

### Análise de Deformações em Tubulações

```python
def analisar_deformacao_tubulacao(imagem_tubulacao, raio_nominal):
    """
    Analisa deformações em tubulações cilíndricas
    Aplica-se: Manutenção predial, indústria petroquímica
    """
    altura, largura = imagem_tubulacao.shape
    centro_x, centro_y = largura // 2, altura // 2
    
    perfil_raio = []
    angulos_analisados = []
    
    # Amostrar a cada 5 graus
    for theta_graus in range(0, 360, 5):
        theta_rad = math.radians(theta_graus)
        raio_medido = None
        
        # Medir raio ao longo deste ângulo
        for r in range(raio_nominal - 10, raio_nominal + 10):
            x = centro_x + r * math.cos(theta_rad)
            y = centro_y + r * math.sin(theta_rad)
            
            if 0 <= x < largura and 0 <= y < altura:
                if imagem_tubulacao[int(y), int(x)] < 100:  # Borda da tubulação
                    raio_medido = r
                    break
        
        if raio_medido:
            perfil_raio.append(raio_medido)
            angulos_analisados.append(theta_graus)
    
    # Calcular ovalização
    if len(perfil_raio) >= 4:
        raio_max = max(perfil_raio)
        raio_min = min(perfil_raio)
        ovalizacao = (raio_max - raio_min) / raio_nominal * 100
        
        return ovalizacao, perfil_raio, angulos_analisados
    
    return None, None, None

# Uso em manutenção:
# tubulacao = cv2.imread('tubulacao_industrial.jpg', 0)
# ovalizacao, perfil, angulos = analisar_deformacao_tubulacao(tubulacao, raio_nominal=50)
# if ovalizacao and ovalizacao > 5:  # Mais de 5% de ovalização
#     print(f"ALERTA: Tubulação com {ovalizacao:.1f}% de ovalização")
#     programar_manutencao()
```

---

## 📊 Comparação: Polar vs Cilíndrico vs Esférico

| Sistema | Coordenadas | Aplicação Típica | Vantagens |
|---------|-------------|------------------|-----------|
| **Polar** | (r, θ) | Objetos 2D circulares | Simplicidade |
| **Cilíndrico** | (r, θ, z) | Objetos com simetria axial | Mantém altura linear |
| **Esférico** | (ρ, θ, φ) | Objetos esféricos | Simetria completa |

> [!TIP] Quando Usar Coordenadas Cilíndricas
> 1. **Objetos com simetria axial** (latas, colunas, tubos)
> 2. **Processamento de superfícies curvas** que podem ser "desenroladas"
> 3. **Tomografia e reconstrução 3D**
> 4. **Texturização de objetos cilíndricos** em CG

> [!WARNING] Considerações Importantes
> 1. **Singularidade** no eixo central
> 2. **Distorção** nas extremidades quando "desenrolado"
> 3. **Complexidade computacional** aumentada
> 4. **Precisa de calibração** precisa do eixo central

> [!SUMMARY] Conclusão
> Coordenadas cilíndricas são ideais para:
> - **Inspeção industrial** de objetos cilíndricos
> - **Reconstrução tomográfica** em medicina
> - **Análise estrutural** de colunas e tubulações  
> - **Texturização 3D** em computação gráfica
> 
> A capacidade de "desenrolar" superfícies cilíndricas para análise plana é particularmente poderosa em aplicações de visão computacional industrial e médica. # Sistemas de Coordenadas Esféricas em Processamento de Imagens

> [!NOTE] Conceito Fundamental
> **Conversão Cartesiano → Esférico**:
> - Cartesianas: `(x, y, z)`
> - Esféricas: `(ρ, θ, φ)`
> - `ρ = √(x² + y² + z²)` → distância da origem
> - `θ = atan2(y, x)` → ângulo azimutal [0, 2π]
> - `φ = acos(z/ρ)` → ângulo polar/inclinação [0, π]

## 🔄 Conversão entre Sistemas de Coordenadas

### Funções Básicas de Conversão

```python
import cv2
import numpy as np
import math

def cartesiano_para_esferico(x, y, z, centro_x, centro_y, centro_z):
    """
    Converte coordenadas cartesianas para esféricas
    """
    # Coordenadas relativas ao centro
    dx = x - centro_x
    dy = y - centro_y  
    dz = z - centro_z
    
    # Calcular raio e ângulos
    rho = math.sqrt(dx**2 + dy**2 + dz**2)
    theta = math.atan2(dy, dx)  # azimute [0, 2π]
    phi = math.acos(dz / rho) if rho > 0 else 0  # inclinação [0, π]
    
    return rho, theta, phi

def esferico_para_cartesiano(rho, theta, phi, centro_x, centro_y, centro_z):
    """
    Converte coordenadas esféricas para cartesianas
    """
    x = centro_x + rho * math.sin(phi) * math.cos(theta)
    y = centro_y + rho * math.sin(phi) * math.sin(theta)
    z = centro_z + rho * math.cos(phi)
    
    return int(x), int(y), int(z)

def imagem_para_esferico(imagem, centro=None, raio_maximo=None):
    """
    Converte imagem 2D para projeção esférica (simplificada)
    Útil para criar panoramas ou mapear texturas esféricas
    """
    altura, largura = imagem.shape[:2]
    
    if centro is None:
        centro = (largura // 2, altura // 2)
    
    if raio_maximo is None:
        raio_maximo = min(centro[0], centro[1], largura - centro[0], altura - centro[1])
    
    # Criar imagem esférica (equirectangular)
    # 360 graus em theta (azimute), 180 graus em phi (inclinação)
    imagem_esferica = np.zeros((180, 360), dtype=imagem.dtype)
    
    for theta_graus in range(360):
        theta_rad = math.radians(theta_graus)
        for phi_graus in range(180):
            phi_rad = math.radians(phi_graus)
            
            # Projeção esférica para plano
            x = centro[0] + raio_maximo * math.sin(phi_rad) * math.cos(theta_rad)
            y = centro[1] + raio_maximo * math.sin(phi_rad) * math.sin(theta_rad)
            
            if 0 <= x < largura and 0 <= y < altura:
                imagem_esferica[phi_graus, theta_graus] = imagem[int(y), int(x)]
    
    return imagem_esferica
```

---

## 💡 Aplicações Práticas com Coordenadas Esféricas

### 1. Criacao de Panoramas 360°

```python
def criar_panorama_360(imagens_parciais):
    """
    Combina múltiplas imagens para criar panorama esférico 360°
    Aplica-se: Fotografia, turismo virtual, imóveis
    """
    # Assumindo que as imagens foram tiradas cobrindo 360°
    altura, largura = imagens_parciais[0].shape[:2]
    panorama = np.zeros((altura, 360, 3), dtype=np.uint8)
    
    angulo_por_imagem = 360 // len(imagens_parciais)
    
    for i, imagem in enumerate(imagens_parciais):
        angulo_inicio = i * angulo_por_imagem
        angulo_fim = (i + 1) * angulo_por_imagem
        
        for theta_graus in range(angulo_inicio, angulo_fim):
            # Mapear coluna do panorama para coluna da imagem original
            coluna_original = int((theta_graus - angulo_inicio) * largura / angulo_por_imagem)
            
            if 0 <= coluna_original < largura:
                panorama[:, theta_graus] = imagem[:, coluna_original]
    
    return panorama

def visualizar_panorama_interativo(panorama):
    """
    Simula visualização interativa de panorama 360°
    """
    altura, largura = panorama.shape[:2]
    
    # Para visualizar uma "janela" do panorama
    def obter_vista(angulo_visao, campo_visao=90):
        inicio = angulo_visao
        fim = (angulo_visao + campo_visao) % 360
        
        if inicio < fim:
            vista = panorama[:, inicio:fim]
        else:
            # Caso cruze o limite 360°
            parte1 = panorama[:, inicio:]
            parte2 = panorama[:, :fim]
            vista = np.hstack([parte1, parte2])
        
        # Redimensionar para proporção normal
        return cv2.resize(vista, (800, 600))
    
    return obter_vista

# Uso em turismo virtual:
# fotos = [cv2.imread(f'foto_{i}.jpg') for i in range(6)]  # 6 fotos cobrindo 360°
# panorama = criar_panorama_360(fotos)
# cv2.imwrite('panorama_360.jpg', panorama)
# 
# # Visualização interativa
# obter_vista = visualizar_panorama_interativo(panorama)
# for angulo in range(0, 360, 30):
#     vista = obter_vista(angulo)
#     cv2.imshow(f'Vista {angulo}°', vista)
#     cv2.waitKey(500)
```

### 2. Análise de Imagens Astronômicas

```python
def analisar_ceu_estrelado(imagem_ceu, centro_observacao):
    """
    Analisa posições estelares usando coordenadas esféricas
    Aplica-se: Astronomia, navegação, satélites
    """
    altura, largura = imagem_ceu.shape
    estrelas_detectadas = []
    
    # Detectar estrelas (pixels muito brilhantes)
    _, binaria = cv2.threshold(imagem_ceu, 200, 255, cv2.THRESH_BINARY)
    contornos, _ = cv2.findContours(binaria, cv2.RETR_EXTERNAL, cv2.CHAIN_APPROX_SIMPLE)
    
    for contorno in contornos:
        # Centro da estrela
        M = cv2.moments(contorno)
        if M["m00"] != 0:
            cx = int(M["m10"] / M["m00"])
            cy = int(M["m01"] / M["m00"])
            
            # Converter para coordenadas esféricas
            # Assumindo que a imagem cobre 180° de campo de visão
            dx = cx - centro_observacao[0]
            dy = cy - centro_observacao[1]
            
            # Calcular ângulos
            theta = math.atan2(dy, dx)  # Azimute
            # Calcular phi baseado na distância do centro (simplificado)
            dist_centro = math.sqrt(dx**2 + dy**2)
            dist_maxima = min(centro_observacao[0], centro_observacao[1], 
                            largura - centro_observacao[0], altura - centro_observacao[1])
            phi = (dist_centro / dist_maxima) * math.pi  # [0, π]
            
            estrelas_detectadas.append({
                'posicao_pixel': (cx, cy),
                'coordenadas_esfericas': (theta, phi),
                'intensidade': np.mean(imagem_ceu[cy-1:cy+2, cx-1:cx+2]),
                'tamanho': cv2.contourArea(contorno)
            })
    
    return estrelas_detectadas

def identificar_constelacoes(estrelas, catalogo_constelacoes):
    """
    Tenta identificar constelações baseado nas posições esféricas
    """
    constelacoes_identificadas = []
    
    for nome, padrao in catalogo_constelacoes.items():
        correspondencias = 0
        
        for estrela_ref in padrao:
            theta_ref, phi_ref = estrela_ref
            
            # Procurar estrela próxima no céu observado
            for estrela_obs in estrelas:
                theta_obs, phi_obs = estrela_obs['coordenadas_esfericas']
                
                # Verificar proximidade angular
                diff_theta = abs(theta_ref - theta_obs) % (2 * math.pi)
                diff_phi = abs(phi_ref - phi_obs)
                
                if diff_theta < 0.1 and diff_phi < 0.1:  # ~5.7 graus
                    correspondencias += 1
                    break
        
        # Se encontrou pelo menos 70% das estrelas do padrão
        if correspondencias >= len(padrao) * 0.7:
            constelacoes_identificadas.append(nome)
    
    return constelacoes_identificadas

# Uso em astronomia amadora:
# ceu = cv2.imread('ceu_noturno.jpg', 0)
# estrelas = analisar_ceu_estrelado(ceu, centro_observacao=(400, 300))
# catalogo = {
#     'Orion': [(1.5, 0.8), (1.6, 0.7), (1.4, 0.6)],  # Posições exemplo
#     'Ursa Maior': [(2.0, 1.0), (2.1, 1.1), (2.2, 1.2)]
# }
# constelacoes = identificar_constelacoes(estrelas, catalogo)
# print(f"Constelações identificadas: {constelacoes}")
```

### 3. Processamento de Imagens Médicas (Ressonância)

```python
def analisar_ressonancia_cerebral(volume_3d):
    """
    Analisa imagens de ressonância magnética cerebral usando coordenadas esféricas
    Aplica-se: Neurologia, diagnóstico médico
    """
    # volume_3d: array 3D (altura, largura, profundidade) da ressonância
    profundidade, altura, largura = volume_3d.shape
    centro = (largura // 2, altura // 2, profundidade // 2)
    
    # Criar mapa esférico do cérebro
    raio_maximo = min(centro[0], centro[1], centro[2])
    mapa_esferico = np.zeros((180, 360))  # phi x theta
    
    for theta_graus in range(360):
        theta_rad = math.radians(theta_graus)
        for phi_graus in range(180):
            phi_rad = math.radians(phi_graus)
            
            # Amostrar ao longo do raio
            intensidades = []
            for rho in range(raio_maximo):
                x, y, z = esferico_para_cartesiano(rho, theta_rad, phi_rad, *centro)
                
                if (0 <= x < largura and 0 <= y < altura and 0 <= z < profundidade):
                    intensidades.append(volume_3d[z, y, x])
            
            if intensidades:
                # Usar intensidade média ao longo do raio
                mapa_esferico[phi_graus, theta_graus] = np.mean(intensidades)
    
    # Analisar simetria cerebral
    def calcular_simetria_hemisferios(mapa):
        # Dividir em hemisférios (esquerdo/direito)
        hemisferio_esquerdo = mapa[:, 90:270]   # 180 graus
        hemisferio_direito = np.flip(mapa[:, 270:360], axis=1)  # Espelhar
        
        # Calcular diferença
        diferenca = np.abs(hemisferio_esquerdo - hemisferio_direito)
        return np.mean(diferenca)
    
    assimetria = calcular_simetria_hemisferios(mapa_esferico)
    
    return mapa_esferico, assimetria

def detectar_anomalias_cerebrais(mapa_esferico, limiar_assimetria=20):
    """
    Detecta possíveis anomalias baseado na assimetria cerebral
    """
    assimetria = calcular_simetria_hemisferios(mapa_esferico)
    
    if assimetria > limiar_assimetria:
        # Encontrar regiões com maior assimetria
        hemisferio_esquerdo = mapa_esferico[:, 90:270]
        hemisferio_direito = np.flip(mapa_esferico[:, 270:360], axis=1)
        mapa_diferenca = np.abs(hemisferio_esquerdo - hemisferio_direito)
        
        # Identificar regiões problemáticas
        regioes_problematicas = []
        limiar_local = np.mean(mapa_diferenca) + 2 * np.std(mapa_diferenca)
        
        for phi in range(mapa_diferenca.shape[0]):
            for theta in range(mapa_diferenca.shape[1]):
                if mapa_diferenca[phi, theta] > limiar_local:
                    regioes_problematicas.append((phi, theta))
        
        return "ANORMAL", assimetria, regioes_problematicas
    else:
        return "NORMAL", assimetria, []

# Uso em diagnóstico:
# ressonancia = carregar_volume_3d('paciente.nii')
# mapa, assimetria = analisar_ressonancia_cerebral(ressonancia)
# status, _, regioes = detectar_anomalias_cerebrais(mapa)
# print(f"Status: {status}, Assimetria: {assimetria:.2f}")
# if regioes:
#     print(f"Regiões com possível anomalia: {len(regioes)}")
```

### 4. Visão Computacional para Robótica Espacial

```python
def navegacao_robot_esferica(nuvem_pontos, ponto_destino):
    """
    Sistema de navegação para robôs em ambientes 3D usando coordenadas esféricas
    Aplica-se: Robótica espacial, drones, veículos autônomos 3D
    """
    # nuvem_pontos: array Nx3 de pontos no espaço
    # ponto_destino: (x, y, z) do destino
    
    # Calcular centro de massa como ponto de referência
    centro = np.mean(nuvem_pontos, axis=0)
    
    # Converter destino para coordenadas esféricas relativas ao centro
    dx, dy, dz = ponto_destino - centro
    rho_dest = math.sqrt(dx**2 + dy**2 + dz**2)
    theta_dest = math.atan2(dy, dx)
    phi_dest = math.acos(dz / rho_dest) if rho_dest > 0 else 0
    
    # Analisar obstáculos ao longo da trajetória
    obstaculos_trajetoria = []
    
    # Amostrar pontos ao longo da trajetória esférica
    for rho in np.linspace(0, rho_dest, 50):
        for theta_offset in np.linspace(-0.1, 0.1, 5):  # Variação angular pequena
            for phi_offset in np.linspace(-0.1, 0.1, 5):
                theta = theta_dest + theta_offset
                phi = phi_dest + phi_offset
                
                # Converter para cartesiano
                x = centro[0] + rho * math.sin(phi) * math.cos(theta)
                y = centro[1] + rho * math.sin(phi) * math.sin(theta)
                z = centro[2] + rho * math.cos(phi)
                
                # Verificar se há obstáculos próximos
                ponto_trajetoria = np.array([x, y, z])
                distancias = np.linalg.norm(nuvem_pontos - ponto_trajetoria, axis=1)
                
                if np.any(distancias < 1.0):  # Obstáculo a menos de 1 unidade
                    obstaculos_trajetoria.append(ponto_trajetoria)
    
    # Calcular rota segura
    if obstaculos_trajetoria:
        # Encontrar direção alternativa
        obstaculos_array = np.array(obstaculos_trajetoria)
        direcao_media_obstaculos = np.mean(obstaculos_array - centro, axis=0)
        
        # Calcular direção perpendicular para desvio
        direcao_destino = ponto_destino - centro
        direcao_desvio = np.cross(direcao_destino, direcao_media_obstaculos)
        direcao_desvio = direcao_desvio / np.linalg.norm(direcao_desvio)
        
        rota_segura = ponto_destino + direcao_desvio * 2.0  # Desvio de 2 unidades
        
    else:
        rota_segura = ponto_destino
    
    return rota_segura, obstaculos_trajetoria

def mapeamento_ambiente_3d(leituras_sensor, posicao_robo):
    """
    Cria mapa 3D do ambiente usando coordenadas esféricas do sensor
    """
    mapa_3d = []
    
    for leitura in leituras_sensor:
        rho, theta_sensor, phi_sensor = leitura
        
        # Converter para coordenadas globais (considerando orientação do robô)
        # Esta é uma simplificação - na prática envolveria rotação 3D
        x = posicao_robo[0] + rho * math.sin(phi_sensor) * math.cos(theta_sensor)
        y = posicao_robo[1] + rho * math.sin(phi_sensor) * math.sin(theta_sensor)
        z = posicao_robo[2] + rho * math.cos(phi_sensor)
        
        mapa_3d.append((x, y, z))
    
    return np.array(mapa_3d)

# Uso em robótica espacial:
# while robo_em_operacao:
#     leituras = sensor_3d.ler()
#     posicao_atual = robo.obter_posicao()
#     mapa = mapeamento_ambiente_3d(leituras, posicao_atual)
#     
#     destino = (10, 5, 3)  # Coordenada destino
#     rota_segura, obstaculos = navegacao_robot_esferica(mapa, destino)
#     
#     robo.mover_para(rota_segura)
```

---

## 🛠️ Técnicas Avançadas com Coordenadas Esféricas

### Projeção Equiretangular para Texturas 3D

```python
def criar_textura_esferica_equiretangular(imagens_cubemap):
    """
    Converte cubemap (6 faces) para projeção equiretangular esférica
    Aplica-se: Computação gráfica, jogos, realidade virtual
    """
    # imagens_cubemap: lista de 6 imagens [frente, trás, esquerda, direita, topo, base]
    frente, tras, esquerda, direita, topo, base = imagens_cubemap
    
    tamanho_face = frente.shape[0]  # Assumindo faces quadradas
    textura_esferica = np.zeros((tamanho_face, 2 * tamanho_face, 3), dtype=np.uint8)
    
    for theta_graus in range(360):
        for phi_graus in range(180):
            theta_rad = math.radians(theta_graus)
            phi_rad = math.radians(phi_graus)
            
            # Determinar qual face do cubemap usar
            x = math.sin(phi_rad) * math.cos(theta_rad)
            y = math.sin(phi_rad) * math.sin(theta_rad)
            z = math.cos(phi_rad)
            
            # Encontrar coordenadas na face apropriada
            abs_x, abs_y, abs_z = abs(x), abs(y), abs(z)
            
            if abs_x >= abs_y and abs_x >= abs_z:
                if x > 0:  # Face direita
                    u = (z / abs_x + 1) / 2
                    v = (y / abs_x + 1) / 2
                    cor = direita[int(v * tamanho_face), int(u * tamanho_face)]
                else:  # Face esquerda
                    u = (-z / abs_x + 1) / 2
                    v = (y / abs_x + 1) / 2
                    cor = esquerda[int(v * tamanho_face), int(u * tamanho_face)]
            elif abs_y >= abs_x and abs_y >= abs_z:
                if y > 0:  # Face topo
                    u = (x / abs_y + 1) / 2
                    v = (-z / abs_y + 1) / 2
                    cor = topo[int(v * tamanho_face), int(u * tamanho_face)]
                else:  # Face base
                    u = (x / abs_y + 1) / 2
                    v = (z / abs_y + 1) / 2
                    cor = base[int(v * tamanho_face), int(u * tamanho_face)]
            else:
                if z > 0:  # Face frente
                    u = (x / abs_z + 1) / 2
                    v = (y / abs_z + 1) / 2
                    cor = frente[int(v * tamanho_face), int(u * tamanho_face)]
                else:  # Face trás
                    u = (-x / abs_z + 1) / 2
                    v = (y / abs_z + 1) / 2
                    cor = tras[int(v * tamanho_face), int(u * tamanho_face)]
            
            textura_esferica[phi_graus, theta_graus] = cor
    
    return textura_esferica
```

### Análise de Distribuições Angulares

```python
def analisar_distribuicao_angular(pontos_3d, centro):
    """
    Analisa distribuição angular de pontos no espaço 3D
    Aplica-se: Física, análise de dados espaciais, astronomia
    """
    distribuicao_azimutal = np.zeros(360)  # Theta
    distribuicao_polar = np.zeros(180)     # Phi
    
    for ponto in pontos_3d:
        x, y, z = ponto
        dx, dy, dz = x - centro[0], y - centro[1], z - centro[2]
        
        rho = math.sqrt(dx**2 + dy**2 + dz**2)
        if rho > 0:
            theta = math.atan2(dy, dx)
            phi = math.acos(dz / rho)
            
            # Converter para graus e discretizar
            theta_graus = int(math.degrees(theta) % 360)
            phi_graus = int(math.degrees(phi))
            
            distribuicao_azimutal[theta_graus] += 1
            if 0 <= phi_graus < 180:
                distribuicao_polar[phi_graus] += 1
    
    # Normalizar
    distribuicao_azimutal = distribuicao_azimutal / len(pontos_3d)
    distribuicao_polar = distribuicao_polar / len(pontos_3d)
    
    return distribuicao_azimutal, distribuicao_polar

def detectar_padroes_angulares(dist_azimutal, dist_polar):
    """
    Detecta padrões como simetrias ou aglomerados angulares
    """
    # Encontrar direções predominantes
    picos_azimutal = []
    picos_polar = []
    
    # Suavizar distribuições
    dist_az_suave = np.convolve(dist_azimutal, np.ones(5)/5, mode='same')
    dist_pol_suave = np.convolve(dist_polar, np.ones(5)/5, mode='same')
    
    # Encontrar picos
    for i in range(1, len(dist_az_suave)-1):
        if (dist_az_suave[i] > dist_az_suave[i-1] and 
            dist_az_suave[i] > dist_az_suave[i+1] and
            dist_az_suave[i] > np.mean(dist_az_suave) * 1.5):
            picos_azimutal.append(i)
    
    for i in range(1, len(dist_pol_suave)-1):
        if (dist_pol_suave[i] > dist_pol_suave[i-1] and 
            dist_pol_suave[i] > dist_pol_suave[i+1] and
            dist_pol_suave[i] > np.mean(dist_pol_suave) * 1.5):
            picos_polar.append(i)
    
    return picos_azimutal, picos_polar
```

---

## 📊 Comparação: Polar vs Cilíndrico vs Esférico

| Sistema | Dimensões | Aplicação Típica | Vantagens |
|---------|-----------|------------------|-----------|
| **Polar** | 2D (r, θ) | Objetos circulares planos | Simplicidade |
| **Cilíndrico** | 3D (r, θ, z) | Objetos com simetria axial | Mantém coordenada linear |
| **Esférico** | 3D (ρ, θ, φ) | Objetos esféricos, ambientes 360° | Simetria completa |

> [!TIP] Quando Usar Coordenadas Esféricas
> 1. **Panoramas 360°** e fotografia esférica
> 2. **Astronomia** e navegação celestial
> 3. **Imagens médicas 3D** (ressonância, tomografia)
> 4. **Robótica espacial** e navegação 3D
> 5. **Computação gráfica** e texturização esférica

> [!WARNING] Considerações Importantes
> 6. **Singularidades** nos polos (φ = 0, φ = π)
> 7. **Distorção** nas projeções equiretangulares
> 8. **Complexidade computacional** aumentada
> 9. **Problemas de precisão** próximo aos polos

> [!SUMMARY] Conclusão
> Coordenadas esféricas são essenciais para:
> - **Visualização imersiva** (VR/AR)
> - **Análise científica** de distribuições espaciais
> - **Navegação 3D** em ambientes complexos
> - **Processamento médico** de órgãos esféricos
> 
> A capacidade de representar direções e orientações de forma natural faz das coordenadas esféricas uma ferramenta poderosa para qualquer aplicação envolvendo visão 3D ou ambientes esféricos.