## 🎯 **MÓDULO 1: INTRODUÇÃO À COMPUTAÇÃO GRÁFICA**

### 1.1 Fundamentos Básicos
- [[História e Evolução da Computação Gráfica]]
- [[Áreas de Aplicação (Jogos, CAD, Visualização Científica, etc.)]]
- [[Pipeline Gráfico Básico]]
- [[Rasterização vs Ray Tracing]]
- [[Dispositivos de Entrada e Saída Gráfica]]
- [[APIs Gráficas]]
### 1.2 Introdução
- [[Libs para  OpenGL ]]
- [[Transformações  Geométricas 2D]]
- [[Sistemas de Coordenadas do Dispositivo]]
- [[Transformações entre Sistemas de Coordenadas]]
- [[Sistemas de Coordenadas Homogêneas]]
### 1.3 Sistemas de Coordenadas
- [[Sistemas de Coordenadas do Mundo]]
- [[Sistemas de Coordenadas do Dispositivo]]
- [[Transformações entre Sistemas de Coordenadas]]
- [[Sistemas de Coordenadas Homogêneas]]

## 📐 **MÓDULO 2: PRIMITIVAS GRÁFICAS BÁSICAS**

### 2.1 Algoritmos de Rasterização
- [[Algoritmo de Linha DDA]]
- [[Algoritmo de Linha Bresenham]]
- [[Algoritmo de Círculo de Bresenham]]
- [[Algoritmo de Preenchimento de Áreas]]
  - [[Scan-line]]
  - [[Flood Fill]]
  - [[Preenchimento por Contorno]]

### 2.2 Atributos de Primitivas Gráficas
- [[Atributos de Linha (Espessura, Padrão)]]
- [[Atributos de Preenchimento]]
- [[Cores e Padrões]]
- [[Fontes e Texto Gráfico]]

## 🔄 **MÓDULO 3: TRANSFORMAÇÕES GEOMÉTRICAS 2D**

### 3.1 Transformações Básicas
- [[Transformações de Translação]]
- [[Transformações de Escala]]
- [[Transformações de Rotação]]
- [[Transformações de Reflexão]]
- [[Transformações de Cisalhamento]]

### 3.2 Transformações Compostas
- [[Coordenadas Homogêneas]]
- [[Composição de Transformações]]
- [[Transformações em Relação a Ponto Arbitrário]]
- [[Eficiência Computacional em Transformações]]

## 🏗️ **MÓDULO 4: SISTEMAS GRÁFICOS 2D**

### 4.1 Sistemas de Janela e Viewport
- [[Mapeamento Janela-Viewport]]
- [[Clipping 2D]]
  - [[Algoritmo de Clipping de Linha Cohen-Sutherland]]
  - [[Algoritmo de Clipping de Linha Liang-Barsky]]
  - [[Algoritmo de Clipping de Polígono Sutherland-Hodgman]]

### 4.2 Hierarquia de Imagens
- [[Estruturas Hierárquicas]]
- [[Gráficos por Instância]]
- [[Sistemas de Coordenadas Locais]]

## 📊 **MÓDULO 5: REPRESENTAÇÃO DE CURVAS E SUPERFÍCIES**

### 5.1 Curvas 2D
- [[Representação Explícita, Implícita e Paramétrica]]
- [[Curvas de Hermite]]
- [[Curvas de Bézier]]
- [[Curvas B-Spline]]
- [[Curvas NURBS (Non-Uniform Rational B-Spline)]]

### 5.2 Superfícies 3D
- [[Superfícies de Bézier]]
- [[Superfícies B-Spline]]
- [[Superfícies NURBS]]
- [[Operações em Superfícies]]

## 🎭 **MÓDULO 6: MODELAGEM GEOMÉTRICA 3D**

### 6.1 Representação de Objetos 3D
- [[Representação por Boundary Representation (B-Rep)]]
- [[Representação por Constructive Solid Geometry (CSG)]]
- [[Malhas de Polígonos]]
- [[Sistemas de Partículas]]

### 6.2 Técnicas de Modelagem
- [[Extrusão e Revolução]]
- [[Modelagem por Subdivisão]]
- [[Modelagem por Escultura Digital]]
- [[Operações Booleanas em Sólidos]]

## 🔄 **MÓDULO 7: TRANSFORMAÇÕES GEOMÉTRICAS 3D**

### 7.1 Transformações 3D Básicas
- [[Translação 3D]]
- [[Escala 3D]]
- [[Rotação 3D (em torno dos eixos principais)]]
- [[Rotação 3D (em torno de eixo arbitrário)]]

### 7.2 Sistemas de Coordenadas 3D
- [[Sistemas de Coordenadas de Mundo]]
- [[Sistemas de Coordenadas de Visualização]]
- [[Sistemas de Coordenadas do Dispositivo]]
- [[Transformações entre Sistemas 3D]]

## 👁️ **MÓDULO 8: PROJEÇÕES E VISUALIZAÇÃO 3D**

### 8.1 Projeções Geométricas
- [[Projeções Paralelas]]
  - [[Projeção Ortográfica]]
  - [[Projeção Oblíqua]]
- [[Projeções em Perspectiva]]
  - [[Projeção de Um Ponto de Fuga]]
  - [[Projeção de Dois Pontos de Fuga]]
  - [[Projeção de Três Pontos de Fuga]]

### 8.2 Pipeline de Visualização 3D
- [[Transformação de Visualização]]
- [[Transformação de Projeção]]
- [[Transformação de Viewport]]
- [[Pipeline Gráfico Completo]]

## 🎨 **MÓDULO 9: ILUMINAÇÃO E SOMBREAMENTO**

### 9.1 Modelos de Iluminação
- [[Modelo de Iluminação de Phong]]
- [[Modelo de Iluminação de Blinn-Phong]]
- [[Componentes de Luz (Ambiente, Difusa, Especular)]]
- [[Propriedades de Materiais]]

### 9.2 Técnicas de Sombreamento
- [[Sombreamento Flat (Plano)]]
- [[Sombreamento Gouraud]]
- [[Sombreamento Phong]]
- [[Mapeamento de Normais]]

## 🌈 **MÓDULO 10: TÉCNICAS DE REALISMO VISUAL**

### 10.1 Mapeamento de Texturas
- [[Coordenadas de Textura]]
- [[Mapeamento Planar, Cilíndrico e Esférico]]
- [[Filtragem de Texturas (Bilinear, Trilinear)]]
- [[Mipmapping]]

### 10.2 Técnicas Avançadas de Realismo
- [[Mapeamento de Relevo (Bump Mapping)]]
- [[Mapeamento de Normais (Normal Mapping)]]
- [[Mapeamento de Ambiente (Environment Mapping)]]
- [[Mapeamento de Deslocamento (Displacement Mapping)]]

## 🕶️ **MÓDULO 11: REMOÇÃO DE SUPERFÍCIES OCULTAS**

### 11.1 Algoritmos de Visibilidade
- [[Algoritmo do Pintor (Painter's Algorithm)]]
- [[Algoritmo Z-Buffer]]
- [[Algoritmo Scan-line Z-Buffer]]
- [[Algoritmo de Warnock]]

### 11.2 Técnicas de Otimização
- [[Back-Face Culling]]
- [[Frustum Culling]]
- [[Occlusion Culling]]
- [[Sistema de Level of Detail (LOD)]]

## ✨ **MÓDULO 12: COMPUTAÇÃO GRÁFICA AVANÇADA**

### 12.1 Renderização em Tempo Real
- [[Pipeline Gráfico Moderno (GPU)]]
- [[Shaders e Linguagens de Shading]]
- [[Programação de Vertex e Fragment Shaders]]
- [[Técnicas de Pós-processamento]]

### 12.2 Técnicas de Renderização Avançada
- [[Ray Tracing]]
- [[Path Tracing]]
- [[Photon Mapping]]
- [[Radiosidade]]

## 🎮 **MÓDULO 13: GRÁFICOS INTERATIVOS E EM TEMPO REAL**

### 13.1 Técnicas de Interação
- [[Seleção e Picking]]
- [[Manipulação Direta de Objetos]]
- [[Sistemas de Partículas Interativas]]
- [[Deformação de Malhas]]

### 13.2 Gráficos para Jogos
- [[Otimização para Tempo Real]]
- [[Técnicas de Animação de Personagens]]
- [[Sistemas de Partículas para Efeitos]]
- [[Renderização de Cenários Grandes]]

## 🌊 **MÓDULO 14: ANIMAÇÃO POR COMPUTADOR**

### 14.1 Técnicas de Animação
- [[Animação por Keyframing]]
- [[Interpolação de Animação]]
- [[Cinética Direta e Inversa]]
- [[Animação Baseada em Física]]

### 14.2 Animação de Personagens
- [[Sistemas de Ossos (Bone Systems)]]
- [[Skinning e Peso de Vértices]]
- [[Animação Facial]]
- [[Captura de Movimento]]

## 🔬 **MÓDULO 15: VISUALIZAÇÃO CIENTÍFICA**

### 15.1 Visualização de Dados Científicos
- [[Visualização de Campos Vetoriais]]
- [[Visualização de Campos Escalares]]
- [[Visualização de Dados Volumétricos]]
- [[Técnicas de Iso-superfícies]]

### 15.2 Visualização de Informação
- [[Técnicas de Visualização de Dados Multidimensionais]]
- [[Visualização de Redes e Grafos]]
- [[Visualização de Hierarquias]]
- [[Visualização de Séries Temporais]]

## 💻 **MÓDULO 16: FERRAMENTAS E APIS GRÁFICAS**

### 16.1 APIs Gráficas
- [[OpenGL e Pipeline Fixo]]
- [[OpenGL Moderno (Shader-Based)]]
- [[Vulkan]]
- [[DirectX]]
- [[WebGL]]

### 16.2 Ferramentas de Desenvolvimento
- [[Blender para Modelagem]]
- [[Unity para Desenvolvimento]]
- [[Unreal Engine]]
- [[Ferramentas de Profiling e Debug]]

## 🧮 **MÓDULO 17: MATEMÁTICA PARA COMPUTAÇÃO GRÁFICA**

### 17.1 Álgebra Linear Aplicada
- [[Vetores e Operações Vetoriais]]
- [[Matrizes e Transformações]]
- [[Quatérnios para Rotações]]
- [[Espaços Vetoriais e Bases]]

### 17.2 Geometria Computacional
- [[Testes de Interseção]]
- [[Algoritmos de Convexidade]]
- [[Triangulação de Polígonos]]
- [[Diagramas de Voronoi e Triangulação de Delaunay]]

## 🌐 **MÓDULO 18: TÓPICOS ESPECIAIS E TENDÊNCIAS**

### 18.1 Tópicos Avançados
- [[Realidade Virtual e Aumentada]]
- [[Processamento de Imagens Médicas 3D]]
- [[Gráficos para Dispositivos Móveis]]
- [[Renderização em Nuvem]]

### 18.2 Pesquisa em Computação Gráfica
- [[Renderização Baseada em Física (PBR)]]
- [[Machine Learning em Computação Gráfica]]
- [[Gráficos em Tempo Real com Ray Tracing]]
- [[Técnicas de Compressão Gráfica]]

---

## 🗂️ **LABORATÓRIOS PRÁTICOS**

### 🧪 Sessões de Laboratório:
1. [[Lab 1 - Ambiente OpenGL/WebGL e Primitivas Básicas]]
2. [[Lab 2 - Algoritmos de Rasterização]]
3. [[Lab 3 - Transformações 2D e Hierarquia]]
4. [[Lab 4 - Curvas e Superfícies]]
5. [[Lab 5 - Modelagem 3D Básica]]
6. [[Lab 6 - Pipeline de Visualização 3D]]
7. [[Lab 7 - Iluminação e Sombreamento]]
8. [[Lab 8 - Mapeamento de Texturas]]
9. [[Lab 9 - Técnicas de Realismo]]
10. [[Lab 10 - Animação Básica]]
11. [[Lab 11 - Shaders e GPU Programming]]
12. [[Lab 12 - Projeto Final Integrador]]

---

## 📋 **AVALIAÇÕES E PROJETOS**

### 🎓 Sistema de Avaliação:
- **Exercícios de Programação**: 25%
- **Laboratórios Práticos**: 30%
- **Projeto Final**: 30%
- **Prova Teórica**: 15%

### 💡 Projetos Sugeridos:
- [[Motor Gráfico Simplificado]]
- [[Jogo 2D com Gráficos Personalizados]]
- [[Visualizador 3D de Dados Científicos]]
- [[Sistema de Animação de Personagens]]
- [[Aplicação de Realidade Aumentada]]

---

## 🔗 **PRÉ-REQUISITOS E CONEXÕES**

### 📚 Conhecimentos Prévios:
- [[Programação em C++/Python/JavaScript]]
- [[Álgebra Linear]]
- [[Cálculo Vetorial]]
- [[Estruturas de Dados]]

### 🔄 Disciplinas Relacionadas:
- [[Processamento de Imagens]]
- [[Visão Computacional]]
- [[Inteligência Artificial]]
- [[Interação Humano-Computador]]
- [[Realidade Virtual e Aumentada]]

---

*Cada tópico listado representa uma área de estudo que pode ser expandida em páginas específicas do Obsidian com teoria detalhada, exemplos de código, exercícios práticos e referências para aprofundamento!* 🚀✨

**📆 Duração Sugerida:** 2 semestres (Básico + Avançado)
**🎯 Nível:** Graduação em Ciência da Computação/Engenharia
**💼 Mercado:** Indústria de Games, CAD, Visualização Científica, Efeitos Visuais