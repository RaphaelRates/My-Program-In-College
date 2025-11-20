
> [!note]  
> O conjunto de **4 vizinhos de um pixel**, denotado por ( N_4(p) ), representa os **pixels adjacentes na horizontal e na vertical** em relação a um pixel central ( p(x, y) ).
> 
> Esses vizinhos são chamados de **vizinhos retos**, e suas coordenadas são:
> 
> - ( (x + 1, , y) ) → à direita
>     
> - ( (x - 1, , y) ) → à esquerda
>     
> - ( (x, , y + 1) ) → abaixo
>     
> - ( (x, , y - 1) ) → acima
>     
> 
> ![[Pasted image 20251101232705.png]]
> 
> Além desses, temos também os **vizinhos diagonais**, que formam o conjunto ( N_D(p) ). Eles são os pixels localizados nas **diagonais imediatas** do pixel ( p ):
> 
> - ( (x + 1, , y + 1) ) → diagonal inferior direita
>     
> - ( (x + 1, , y - 1) ) → diagonal superior direita
>     
> - ( (x - 1, , y + 1) ) → diagonal inferior esquerda
>     
> - ( (x - 1, , y - 1) ) → diagonal superior esquerda
>     
> 
> ![[Pasted image 20251101233108.png]]
> 
> 🔹 Em resumo:
> 
> - ( N_4(p) ) → apenas conexões **retas** (horizontal/vertical).
>     
> - ( N_D(p) ) → apenas conexões **diagonais**.
>     
> 
> Juntos, eles formam o conjunto **de 8 vizinhos**, ( N_8(p) = N_4(p) \cup N_D(p) ).
> 
> ![[Pasted image 20251101234732.png]]

próximo: [[Conexão - 8 vizinhos]]

