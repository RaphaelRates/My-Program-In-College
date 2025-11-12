> [!note]
> **Conexão Mista (M-Vizinhança)**
> 
> A **conexão mista** é uma abordagem híbrida que combina características da 4-vizinhança e 8-vizinhança para resolver problemas de **conectividade ambígua** em processamento de imagens.
> 
> **Princípio Fundamental:**
> Dois pixels **p** e **q** são m-vizinhos se:
> 1. **q** está em N₄(p) **OU**
> 2. **q** está em N_D(p) **E** N₄(p) ∩ N₄(q) = ∅


> [!note] **Em outras palavras:**
> - Aceita **todas as conexões horizontais/verticais** (4-vizinhança)
> - Aceita **conexões diagonais apenas quando NÃO há vizinhos retos comuns**
> 
> **Exemplo de Aplicação:**
> ```
>  0  1  0
>  1  p  1
>  0  1  0
> ```
> Neste caso, os pixels diagonais **NÃO** seriam considerados m-vizinhos de **p**, pois compartilham vizinhos retos.
> 
> **Vantagens:**
> - Elimina caminhos múltiplos entre pixels
> - Preserva a topologia da imagem
> - Evita o problema da "conectividade excessiva" da 8-vizinhança
> - Mantém algumas vantagens da conectividade diagonal
> 
> **Desvantagens:**
> - Mais complexa de implementar
> - Pode ser muito restritiva em alguns casos
> - Requer verificação adicional de vizinhança
>   
> ![[Pasted image 20251101234732.png]]
> ![[Pasted image 20251101234750.png]]
> ![[Pasted image 20251101234958.png]]
> 

**Aplicações Práticas:**
- **Esqueletização** de imagens binárias
- **Análise topológica** de formas
- **Algoritmos de thinning** (afinamento)
- Processamento onde a preservação da estrutura é crítica

A conexão mista oferece um **equilíbrio inteligente** entre a conservatividade da 4-vizinhança e a completude da 8-vizinhança.

próximo: [[Distância Euclidiana]]
