# 🌈 Modelos de Cor – Hub Central

> *Mapa mental interativo dos principais modelos de cor utilizados em **processamento de imagens** e **visão computacional**.*

---

## 🎯 **Navegação Rápida**

>[!note]- Tabela de Referência Rápida
> | Modelo | Tipo | Aplicação Principal |
> |--------|------|---------------------|
> | RGB | Aditivo | Displays, Câmeras |
> | HSV | Perceptual | Segmentação |
> | YCbCr | Compressão | JPEG/Video |
> | CIELab | Científico | Medição Precisão |

---

## 🖤 **[[Escala de Cinza]]**

>[!info]- Características Principais
> - **Representação**: Intensidade luminosa (0 = preto, 255 = branco)
> - **Aplicações**: Análise de textura, segmentação, pré-processamento
> - **Vantagens**: Simplicidade computacional
>
> **Links**: [[Processamento Tons Cinza]] | [[Conversão RGB para Cinza]]

---

## 🎨 **Modelos Baseados em Dispositivos**

### [[Modelo RGB]] (Red, Green, Blue)

>[!summary]- Resumo RGB
> - **Tipo**: Modelo aditivo
> - **Uso**: Monitores, câmeras, imagens digitais
> - **Limitação**: Pouco robusto a variações de iluminação
>
> **Links**: [[Espaço RGB]] | [[Operações RGB]]

### [[Modelo RGBA]] (RGB + Alpha)

>[!tip]- Dica Prática
> Use RGBA quando precisar de transparência em sobreposições de imagens ou gráficos interativos.

### [[Modelo CMY]]/[[Modelo CMYK]]

>[!warning]- Atenção
> O modelo CMYK é essencial para impressão, mas não deve ser usado para processamento de imagens digitais.

---

## 👁️ **Modelos Baseados na Percepção Humana**

### Família [[Modelo HSV]]/[[Modelo HSL]]/[[Modelo HSI]]/[[Modelo HSB]]

>[!example]- Exemplo de Uso
> ```python
> # Converter RGB para HSV no OpenCV
> hsv = cv2.cvtColor(imagem, cv2.COLOR_RGB2HSV)
> ```
> Ideal para segmentação por cor!

### [[Modelo YUV]] / [[Modelo YIQ]]/ [[Modelo YCbCr]]

>[!abstract]- Resumo Técnico
> Separa luminância (informação de brilho) da crominância (informação de cor), permitindo compressão eficiente.

---

## 🔬 **Modelos Científicos Avançados**

### [[Modelo CIE XYZ]]

>[!quote]- Contexto Histórico
> "O modelo CIE 1931 XYZ foi o primeiro espaço de cor matematicamente definido baseado na percepção humana."

### [[Modelo CIELab]] / [[Modelo CIELuv]]

>[!success]- Vantagem Crítica
> Perceptualmente uniforme - distâncias iguais no espaço Lab correspondem a diferenças perceptivas iguais.

---

## 🚀 **Modelos Especializados**

### [[Modelo OHTA]] / [[Modelo I1I2I3]]

>[!todo]- Para Implementar
> - [ ] Estudar transformação linear
> - [ ] Testar em segmentação
> - [ ] Comparar com HSV

### [[Modelo ICtCp]]

>[!faq]- Por que usar ICtCp?
> - Melhor para HDR
> - Preserva detalhes em altas dinâmicas
> - Padrão emergente para vídeo 4K/8K

### [[Modelo LMS]]

>[!question]- Pergunta Reflexiva
> Como a modelagem da resposta dos cones oculares pode melhorar algoritmos de visão computacional?

---

## 📊 **Comparação Prática**

>[!note]- Guia de Escolha
> | Cenário | Modelo Recomendado | Motivo |
> |---------|-------------------|---------|
> | Segmentação por cor | **HSV** | Separação intuitiva |
> | Compressão | **YCbCr** | Eficiência |
> | Medição precisa | **CIELab** | Uniformidade |
> | HDR/Video | **ICtCp** | Alcance dinâmico |

---

## 🔗 **Conexões com Outras Áreas**

>[!seealso]- Veja Também
> - [[Processamento de Imagens]] ← Aplicações práticas
> - [[Visão Computacional]] ← Uso em algoritmos  
> - [[Computação Gráfica]] ← Renderização e displays
> - [[Colorimetria]] ← Fundamentos científicos

---

>[!tip]- Dica Final
> **Regra prática**: Use HSV para segmentação, YCbCr para compressão e CIELab para medição precisa.

---
*Tags: #modelos-cor #processamento-imagens #visao-computacional #hub*

próximo: [[Formatos de Arquivo]]