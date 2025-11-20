> [!note]
> **Distância de Quarteirão (City-Block ou D4)**
> 
> A **distância de quarteirão**, também conhecida como **distância Manhattan** ou **D4**, mede a distância entre dois pixels considerando apenas **movimentos na horizontal e vertical**, como se estivesse percorrendo um quarteirão em uma cidade.
> 
> **Fórmula Matemática:**
> Para dois pixels **p(x₁, y₁)** e **q(x₂, y₂)**:
> 
> $$
> D_4(p, q) = |x_2 - x_1| + |y_2 - y_1|
> $$
> 
> **Exemplo Prático:**
> Se p(1, 2) e q(4, 6):
> $$
> D_4 = |4-1| + |6-2| = 3 + 4 = 7
> $$

![[Pasted image 20251102001459.png]]

>[!note] ###  **Interpretação Geométrica:**
> - Representa o **número mínimo de passos** necessários para ir de p até q
> - Cada movimento horizontal ou vertical conta como **1 unidade**
> - Movimentos diagonais **não são permitidos**
> 
> **Características Principais:**
> - ✅ **Computacionalmente eficiente**: Apenas soma e valor absoluto
> - ✅ **Intuitiva**: Fácil de entender e visualizar
> - ✅ **Corresponde à 4-vizinhança**: Cada passo vai para um N₄(p)
> - ❌ **Superestima distâncias**: Sempre ≥ distância euclidiana
> 
> **Propriedades:**
> - **Disco unitário**: Forma um "diamante" ao redor do pixel central
> - **Métrica válida**: Satisfaz desigualdade triangular
> - **Invariante a rotações de 90°**

> [!note] ### **Aplicações em Processamento de Imagens:**
> - **Transformadas de distância** em imagens binárias
> - **Algoritmos de preenchimento** (flood fill)
> - **Medição de perímetros** aproximados
> - **Processamento morfológico** básico
> - **Análise de conectividade** 4-vizinhança
> 
> **Visualização do Disco Unitário (D4 = 1):**
> ```
>     •    
>   • P •  
>     •    
> ```
> 
> **Comparação com Outras Métricas:**
> Para p(0,0) e q(3,4):
> - **D4**: |3| + |4| = 7
> - **D8**: max(|3|, |4|) = 4  
> - **Euclidiana**: √(9 + 16) = 5

A distância de quarteirão é amplamente utilizada em algoritmos que priorizam eficiência computacional sobre precisão geométrica absoluta.