> [!abstract] 
> Um modelo de aprendizado onde os dados não são rotulados, ele aprendem sem supervisor.

![[Pasted image 20250810165714.png]]
> [!example] 
> Digamos que você tenha muitos dados sobre os visitantes do seu blog. Você pode querer executar um algoritmo de **agrupamento** (_Clusterização_) para tentar detectar grupos de visitantes semelhantes. Em nenhum momento você informa ao algoritmo a qual grupo um visitante pertence: ele encontra essas conexões sem a sua ajuda.
>
 Por exemplo, ele pode perceber que 40% dos seus visitantes são homens que adoram histórias em quadrinhos e geralmente leem seu blog à noite, enquanto 20% são jovens fãs de ficção científica que visitam durante os fins de semana, e assim por diante. Se você usar um algoritmo de **agrupamento hierárquico** (_hierarchical clustering_), ele também pode subdividir cada grupo em grupos menores. Isso pode ajudar você a direcionar suas postagens para cada grupo.

> [!example] Algoritmos de Vizualização
> Otimos exemplos, prncupalmente para ter ume representação 3D ou 2D, sem ter sobreposição de grupos separados.

![[Pasted image 20250810173042.png]]
> [!note] Ás vezes utilizamos um metodo chamado Dimensionality Reduction
> 

> [!abstract] [[ML - Redução de Dimensionalidade]]
> Uma forma de simplificar os dados sem perder muita informação impritante, unindo certos grupos/Categorias em um(a) só. Por exemplo, quilometragem e idade de um carro podem ser combinados em um único atributo e representa o seu desgaste
> 
> Isso chamamos de Extração de Recursos

> [!check] É uma boa ideia tentar fazer essa redução e dimensionalidade dos dados antes de usá-los para a IA aprender pois
>  - Reduz o tempo
>  - Reduz memória

> [!note] Temos tabém outra atividade no treinamento Não Supervisionado chamada de **Anomaly Detection**

![[Pasted image 20250810180323.png]]
> [!abstract] [[ML - Detecção de Anomalias]]
> consiste no processo de identificar padrões, dados ou eventos que divergem significativamente do comportamento esperado ou padrão habitual de um sistema ou conjunto de dados. 

> [!example] Como identificar se um valor  sair do intervalo de desvio padrão de uma média de resultados

>[!important] Um muito semelhante tarefa é detecção de novidade
> a diferença é que os algoritmos de detecção de novidades esperam.
> Veja apenas dados normais durante o treinamento, enquanto os algoritmos de detecção de anomalia são geralmente
> Mais tolerantes, eles geralmente podem ter um bom desempenho, mesmo com uma pequena porcentagem de outliers em um conjunto de treinamento.

> [!note] Uma das tarefas clássicas do **aprendizado não supervisionado** é a **aprendizagem de regras de associação**, cujo propósito é **esmiuçar** grandes volumes de dados para **desvelar** relações significativas entre atributos.
> - **Regras de associação**: são expressões do tipo "se A e B, então C", que indicam relações frequentes entre conjuntos de itens.

> [!example] Supermercado
>  Imagine que você gerencie um supermercado e possua vastos registros das vendas diárias. Ao aplicar regras de associação, é possível descobrir insights valiosos, tais como:
> 
> Clientes que compram **molho de churrasco** e **batata chips** também tendem a adquirir **bife**.
> 
>Esse conhecimento permite a otimização do layout da loja, por exemplo, posicionando esses produtos próximos uns dos outros para estimular compras adicionais.
### Aqui estão alguns algoritmos que são de modo supervisionado

 - [[ML - K Means]]
 - [[ML - DBSCAN]]
- [[ML - HCA (Análise de Hierarquia de Clusters)]]
 - [[ML - Detecção de Anomalias]]
  - [[ML - PCA (Análise de componentes principais)]]
 - [[ML - Kernel PCA]]
- [[ML - LLE (Incorporação localmente-linear)]]
 - [[ML - t-SNE (incorporação estocástica distribuída em T)]]
 - [[ML - Eclat]]
 - [[ML - Apriori]]


