> [!abstract] O Aprendizado por Reforço é uma fera completamente distinta. Nesse contexto, o sistema de aprendizado, chamado de **agente**, pode observar o ambiente, selecionar e executar ações, e receber recompensas (ou penalidades na forma de recompensas negativas, como ilustrado na Figura 1-12).

> [!note] O agente deve aprender por conta própria qual é a melhor estratégia, chamada de **política**, para obter a maior recompensa ao longo do tempo. Uma política define qual ação o agente deve escolher em uma dada situação.

---
![[Pasted image 20250810184134.png]]
> [!example] ## Exemplo prático: Robótica e AlphaGo
> Muitos robôs utilizam algoritmos de Aprendizado por Reforço para aprender a andar. Um exemplo emblemático é o programa **AlphaGo**, da DeepMind, que ganhou destaque em maio de 2017 ao derrotar o campeão mundial Ke Jie no jogo de Go.
> 
> - AlphaGo aprendeu sua política vencedora analisando milhões de partidas e jogando diversas partidas contra si mesmo.
> - Durante as partidas contra o campeão, o aprendizado foi desativado; AlphaGo apenas aplicava a política previamente aprendida.

---

## Como funciona o Aprendizado por Reforço?

- O **agente** interage com o **ambiente** em ciclos contínuos.
- Ele executa uma **ação** e recebe uma **recompensa** ou **penalidade**.
- O objetivo do agente é maximizar a soma total das recompensas ao longo do tempo.
- A estratégia do agente é definida pela **política**, que mapeia situações para ações.

---

### Glossário

| Termo              | Definição                                                                                 |
|--------------------|-------------------------------------------------------------------------------------------|
| **Agente**         | Entidade que executa ações no ambiente e aprende com as recompensas recebidas.             |
| **Ambiente**       | Mundo ou sistema com o qual o agente interage.                                            |
| **Política**       | Estratégia que determina qual ação o agente deve tomar em cada estado.                     |
| **Recompensa**     | Feedback positivo ou negativo recebido após uma ação, usado para guiar o aprendizado.     |

---

### Referência Visual

![Figura 1-12: Aprendizado por Reforço](URL_da_imagem_aqui)

---

Se desejar, posso ajudar a elaborar exemplos de código, diagramas explicativos ou aprofundar conceitos específicos!
