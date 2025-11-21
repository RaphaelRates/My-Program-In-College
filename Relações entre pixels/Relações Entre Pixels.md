> [!abstract] 
> Para analisar e processar imagens, é essencial entender como os pixels se relacionam entre si.
> 
> Esses relacionamentos definem estruturas, formas e regiões dentro da imagem, sendo a base para operações como segmentação e análises de objetos

## Conceito de vizinhança
> [!note] Vizinhança
> a **Vizinhança** de um pixel é o conjunto de outro pixels próximos a ele, definido pruma regra de proximidade. 
> ![[Pasted image 20251120210021.png]]
> 
> Sendo um pixel $p$ nas coordenadas $(x, y)$. temos alguns conceitos como:

### [[Conexão - 4 Vizinhos]]

> [!abstract]  
> Define os **4 vizinhos diretos** de um pixel — aqueles que estão **acima, abaixo, à esquerda e à direita**.
> 
> 🔹 Usado para conexões **ortogonais** (sem diagonais).
> 
> 📐 Ideal em análises de **contorno** ou **segmentação** que evitam "pontes" diagonais.

---

### [[Conexão - 8 vizinhos]]

> [!abstract]  
> Inclui os **4 vizinhos diretos** e os **4 diagonais**, formando o total de **8 vizinhos**.
> 
> 🔹 Permite conexões **em todas as direções** — útil para objetos com bordas inclinadas.
> 
> ⚙️ Base comum em **processamento morfológico** e **rastreamento de regiões**.

---

### [[Conexão - Mista]]

> [!note]  
> Combina os conceitos de **4-vizinhos** e **8-vizinhos**, adaptando-se conforme o contexto da imagem.
> 
> 📊 Usada para evitar **conexões ambíguas** — garantindo consistência na análise topológica.

---
> [!done]  Conectividade/adjacência
> Determina se pixels estão conectados entre si, considerando a proximidade espacial (vizinhança) e a similaridade de valor. É bastante usado para agrupar pixels próximos em regiões ou objetos 
> 
> Portanto, dois pixels são considerados conectados se:
>  
>  - a) São vizinhos segundo um tipo de proximidade espacial: N4 (p), ND (p) ou N8 (p) ○
>  - b) Obedecem um critério de similaridade, ou seja, seus níveis de cinza pertencem a um conjunto pré-definido V que define a faixa de conectividade

---
### [[Distância Euclidiana]]

> [!abstract]  
> Mede a **distância direta** entre dois pontos (a “reta” mais curta).
> 
> 🧮 Fórmula:  
> $$( d = \sqrt{(x_2 - x_1)^2 + (y_2 - y_1)^2} )$$
> 
> 💡 Mais realista, porém mais **custosa computacionalmente**.

---

### [[DIstancia City Block (Quarteirão)]]

> [!abstract]  
> Mede a distância considerando **movimentos horizontais e verticais**, como andar em um quarteirão.
> 
> 🧮 Fórmula:  
> $$( d = |x_2 - x_1| + |y_2 - y_1| )$$
> 
> 🏙️ Ideal para grids onde só se pode andar em linha reta.

---

### [[DIstancia City Block (Quarteirão)]]

> [!abstract]  
> Considera o **maior deslocamento** entre as coordenadas — como o movimento de um rei no xadrez.
> 
> 🧮 Fórmula:  
> $$( d = \max(|x_2 - x_1|, |y_2 - y_1|) )$$
> 
> ♟️ Boa para **movimentos diagonais uniformes**.

---

> [!note] 
> A mensuração da distância entre pixels possui diversas aplicações.
> 
> **Segmentação e classificação de objetos**: permite agrupar pixels próximos com características semelhantes (como cor e textura), formando regiões conectadas. Essas regiões ajudam a identificar e classificar objetos na imagem.
> 
> **Filtragem de imagens**: pode ser usada para suavizar ou realçar certas características da imagem. Por exemplo, em técnicas de filtragem de bordas, a diferença na intensidade dos pixels adjacentes é usada para detectar e realçar bordas na imagem

> [!info] 
> A mensuração da distância entre pixels possui diversas aplicações.
> 
> **Análise e medição geométrica de objetos**: ajuda a calcular perímetros, áreas, comprimentos e larguras de objetos presentes na imagem. Isso permite quantificar propriedades geométricas para tomadas de decisões.
> 
> **Detecção de movimento**: mede o deslocamento de um objeto entre frames em vídeos para análise de movimento ou rastreamento.
> 
   **Extração de características**: pode ajudar a extrair características espaciais para descrever padrões texturais (técnicas como Matriz de Coocorrência de Níveis de Cinza - GLCM) o que auxilia na classificação de objetos
   
___
> [!tip]  Dada uma imagem digital, podemos aplicar diversas operações que alteram os valores dos pixels.
> Por exemplo, operações aritméticas — normalmente usadas em números decimais — também podem ser aplicadas às imagens digitais, como:
> 
>  - Adição
>  - Subtração
>  - Multiplicação
>  - Divisão
### [[Operação de Adição]]

> [!note]  
> Soma pixel a pixel de duas imagens.
> 
> 🧠 Usada para **misturar imagens**, **aumentar brilho** ou **combinar filtros**.

---

### [[Operação de Subtração]]

> [!note]  
> Subtrai os valores de pixels entre imagens.
> 
> 📸 Muito usada para **detecção de movimento** ou **diferença entre frames**.

---

### [[Operação de Multiplicação]]

> [!note]  
> Multiplica pixel a pixel.
> 
> 💡 Usada para **máscaras**, **realce seletivo** ou **ajustes de contraste local**.

---

### [[Operação de Divisão]]

> [!note]  
> Divide os pixels correspondentes entre duas imagens.
> 
> ⚙️ Pode realçar **mudanças sutis** em iluminação ou **corrigir sombras**.

---

### [[Operações Lógicas em Imagens]]

> [!abstract]  
> Operações binárias entre imagens: **AND, OR, XOR, NOT**.
> 
> 🧩 Fundamentais para **segmentação**, **máscaras** e **processamento binário**.
