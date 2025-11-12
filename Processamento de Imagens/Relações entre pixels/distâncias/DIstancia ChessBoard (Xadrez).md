> [!note]
> **Distância de Xadrez (Chessboard ou D8)**
> 
> A **distância de xadrez**, também conhecida como **distância de Chebyshev** ou **D8**, mede a distância entre dois pixels considerando o **movimento do rei no xadrez** - podendo se mover em todas as 8 direções.
> 
> **Fórmula Matemática:**
> Para dois pixels **$p(x₁, y₁)$** e **$q(x₂, y₂)$**:
> 
> $$
> D_8(p, q) = \max(|x_2 - x_1|, |y_2 - y_1|)
> $$
> 
> **Exemplo Prático:**
> Se $p(1, 2)$ e $q(4, 6)$:
> $$
> D_8 = \max(|4-1|, |6-2|) = \max(3, 4) = 4
> $$

![[Pasted image 20251102002124.png]]

> [!note] ### **Interpretação Geométrica:**
> - Representa o **número mínimo de movimentos do rei no xadrez**
> - Cada movimento (horizontal, vertical ou diagonal) conta como **1 unidade**
> - Corresponde ao **máximo** entre as diferenças absolutas nas coordenadas
> 
> **Características Principais:**
> - ✅ **Computacionalmente muito eficiente**: Apenas max e valor absoluto
> - ✅ **Subestima distâncias**: Sempre ≤ distância euclidiana
> - ✅ **Corresponde à 8-vizinhança**: Cada passo vai para um N₈(p)
> - ❌ **Menos precisa geometricamente**: Não reflete bem distâncias reais
> 
> **Propriedades:**
> - **Disco unitário**: Forma um "quadrado" 3×3 ao redor do pixel central
> - **Métrica válida**: Satisfaz desigualdade triangular
> - **Invariante a rotações de 45°**


> [!note] ### **Aplicações em Processamento de Imagens:**
> - **Transformadas de distância** rápidas
> - **Dilatação e erosão** em processamento morfológico
> - **Expansão de regiões** e preenchimento
> - **Algoritmos de esqueletização**
> - **Medição de diâmetros** aproximados
> 
> **Visualização do Disco Unitário (D8 = 1):**
> ```
>   • • •
>   • P •
>   • • •
> ```
> 
> **Comparação com Outras Métricas:**
> Para $p(0,0)$ e $q(3,4)$:
> - **D4**: |3| + |4| = 7
> - **D8**: max(|3|, |4|) = 4
> - **Euclidiana**: √(9 + 16) = 5
> 
> **Relação de Desigualdade:**
> $$
> D_8(p,q) \leq D_e(p,q) \leq D_4(p,q)
> $$

A distância de xadrez é ideal para aplicações que requerem máxima eficiência computacional e onde a conectividade 8-vizinhança é desejada.