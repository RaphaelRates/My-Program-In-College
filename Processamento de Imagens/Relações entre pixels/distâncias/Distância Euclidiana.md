> [!note]
> **Distância Euclidiana entre Pixels**
> 
> A **distância euclidiana** é a medida de distância "em linha reta" entre dois pontos em um espaço bidimensional, seguindo o **Teorema de Pitágoras**.

> [!abstract] **Fórmula Matemática:**
> Para dois pixels **$p(x₁, y₁)$** e **$q(x₂, y₂)$**:
> 
> $$
> D_e(p, q) = \sqrt{(x_2 - x_1)^2 + (y_2 - y_1)^2}
> $$
> 
> **Exemplo Prático:**
> Se $p(1, 2)$ e $q(4, 6)$:
> $$
> D_e = \sqrt{(4-1)^2 + (6-2)^2} = \sqrt{9 + 16} = \sqrt{25} = 5
> $$

![[Pasted image 20251101235923.png]]

> [!info]  ### **Características Principais:**
>
> - ✅ **Mais intuitiva**: Representa a distância "real" entre pontos
> - ✅ **Rotacionalmente invariante**: A rotação do sistema de coordenadas não altera a distância
> - ✅ **Métrica válida**: Satisfaz todas as propriedades de uma métrica matemática
> - ❌ **Computacionalmente custosa**: Envolve cálculo de raiz quadrada

> [!note] ### **Aplicações em Processamento de Imagens:**
> - **Segmentação por crescimento de regiões**
> - **Análise de agrupamentos** (clustering)
> - **Transformada de distância** para medições morfológicas
> - **Interpolação e warping** de imagens
> - **Cálculo de diâmetros** de objetos

> [!note]  ###  **Comparação com Outras Métricas:**
> - **D4 (City-Block)**: |Δx| + |Δy|
> - **D8 (Chessboard)**: max(|Δx|, |Δy|)
> - **Euclidiana**: √(Δx² + Δy²) → **Mais precisa geometricamente**
> 
> **Otimização Computacional:**
> Muitos algoritmos usam a **distância euclidiana ao quadrado** para evitar o cálculo da raiz quadrada:
> $$
> D_e^2 = (x_2 - x_1)^2 + (y_2 - y_1)^2
> $$

A distância euclidiana é fundamental para operações que requerem precisão geométrica e fidelidade espacial no processamento de imagens digitais.

próximo: [[DIstancia City Block (Quarteirão)]]

