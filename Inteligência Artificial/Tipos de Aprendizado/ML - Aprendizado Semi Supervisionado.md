> [!abstract] Tu mete uma IA lidar com dados que são PARCIALMENTE rotulados, literalmente isso, um supervisionado imcompleto.

> [!abstract] Imagine um sistema de inteligência artificial capaz de lidar com dados que são **parcialmente rotulados** — em outras palavras, um aprendizado supervisionado incompleto.

> [!note] Isso resulta em uma pequena quantidade de dados rotulados acompanhada por uma imensa quantidade de dados não rotulados.

---
> [!note] Alguns algoritmos conseguem manejar conjuntos de dados de treinamento que contêm principalmente dados não rotulados e uma pequena parcela de dados rotulados. Esse processo é conhecido como **aprendizado semi-supervisionado** .

---
![[Pasted image 20250810183742.png]]
> [!example] Exemplo prático: Google Fotos
> Quando você faz o upload de todas as fotos da sua família para o serviço, o sistema automaticamente reconhece que a mesma pessoa **A** aparece nas fotos 1, 5 e 11, enquanto outra pessoa **B** aparece nas fotos 2, 5 e 7. Essa é a parte **não supervisionada** do algoritmo, chamada **clustering** (agrupamento).

> [!success] 💡 **Dica:** A partir de apenas **uma etiqueta por pessoa**, o sistema é capaz de nomear todos os indivíduos em todas as fotos, facilitando a busca por imagens.

---

> [!tldr] Como funciona sapoha
>A maioria dos algoritmos de aprendizado semi-supervisionado combina técnicas supervisionadas e não supervisionadas. Por exemplo:
>
> - **Deep Belief Networks (DBNs)**: compostas por componentes não supervisionados chamados **Restricted Boltzmann Machines (RBMs)** empilhados em camadas.
> - As RBMs são treinadas sequencialmente de forma não supervisionada.
> - O sistema é então refinado com aprendizado supervisionado para melhorar a acurácia.
