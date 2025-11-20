> [!note] **Conjunto de 8 Vizinhos - N₈(p)**
>
> O conjunto de **8 vizinhos** de um pixel **p(x, y)** inclui **todos os pixels adjacentes** nas direções horizontal, vertical e diagonal.
>
> **Coordenadas dos 8 Vizinhos:**
> 
> **Vizinhos Retos (N₄(p)):**
> - (x + 1, y) → direita
> - (x - 1, y) → esquerda  
> - (x, y + 1) → abaixo
> - (x, y - 1) → acima
> 
> **Vizinhos Diagonais (N_D(p)):**
> - (x + 1, y + 1) → diagonal inferior direita
> - (x + 1, y - 1) → diagonal superior direita
> - (x - 1, y + 1) → diagonal inferior esquerda
> - (x - 1, y - 1) → diagonal superior esquerda
> 
 > **Relação:**
> N₈(p) = N₄(p) ∪ N_D(p)
> 
![[Pasted image 20251101233602.png]]
> 
> **Características da 8-Vizinhança:**
> - **Conectividade completa**: Inclui todas as direções possíveis
> - **Maior sensibilidade**: Detecta bordas diagonais com mais precisão
> - **Risco de conexões excessivas**: Pode conectar objetos que deveriam estar separados
> 
> **Aplicações Típicas:**
> - Operadores de detecção de bordas (como Prewitt, Sobel)
> - Algoritmos de crescimento de regiões
> - Análise de texturas e padrões
> - Processamento morfológico avançado
> 
> A 8-vizinhança é mais "generosa" em termos de conectividade, o que pode ser vantajoso ou problemático dependendo da aplicação específica.
> 
> 
> ![[Pasted image 20251101234732.png]]
> ![[Pasted image 20251101234750.png]]

próximo: [[Conexão - Mista]]

