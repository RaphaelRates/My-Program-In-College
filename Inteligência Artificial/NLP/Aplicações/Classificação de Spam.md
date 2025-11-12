> [!note] okokok

# Importação de Libs

```python
import pandas as pd
from sklearn.model_selection import train_test_split
from sklearn import metrics
from sklearn.metrics import confusion_matrix, accuracy_score
from sklearn.emseble import RandomFlorestClassifier
from sklearn.feature_extraction.text import TfidVectorizer
from google.colab import files
```
 
## **Pandas**

Usado para lidar com **dados tabulares** (planilhas, CSVs, etc). Permite ler, organizar, filtrar e manipular conjuntos de dados de forma eficiente.

Exemplo:

```python
import pandas as pd
df = pd.read_csv('dados.csv')
```

---

## 🧠 **Scikit-learn (sklearn)**

Biblioteca poderosa para **treinamento e avaliação de modelos de IA**.

### 🔹 **train_test_split**

Divide o dataset em partes de **treino**, **teste** e, opcionalmente, **validação**.

> Serve pra garantir que o modelo aprenda com uma parte e seja avaliado em outra.

### 🔹 **metrics**

Conjunto de **métricas de avaliação** (recall, precisão, F1, ROC, etc). Usado pra medir quão bem o modelo está performando.

### 🔹 **confusion_matrix e accuracy_score**

- `confusion_matrix`: mostra quantos acertos e erros o modelo cometeu para cada classe.
    
- `accuracy_score`: mede a **porcentagem total de acertos** do modelo.
    

### 🔹 **RandomForestClassifier**

Modelo de **classificação baseado em várias árvores de decisão**. Cada árvore dá seu voto, e o resultado final é a maioria — tornando o modelo mais robusto e menos propenso a overfitting.

### 🔹 **TfidfVectorizer**

Transforma **texto em números** usando o método **TF-IDF (Term Frequency – Inverse Document Frequency)**.

> Palavras frequentes em um documento, mas raras no corpus, recebem maior peso — representando melhor sua importância no contexto.


### 🔹 **Colab**

Funções do próprio editor do cola, que será usado nesse exemplo 

---

## 📊 **Resumo rápido**

| Módulo                            | Função principal                    |
| --------------------------------- | ----------------------------------- |
| pandas                            | Manipular dados tabulares           |
| train_test_split                  | Dividir treino/teste                |
| metrics                           | Avaliar desempenho                  |
| confusion_matrix / accuracy_score | Métricas específicas                |
| RandomForestClassifier            | Modelo de machine learning          |
| TfidfVectorizer                   | Converter texto em vetor numérico   |
| Colab                             | Lidar com o Editor de código online |

# Leitura do arquivo

> [!abstract] 
> usaremos o Pandas para ler o arquivo **spam.csv** e cria um **DataFrame** chamado `span` com os dados da planilha (geralmente, mensagens e rótulos tipo _spam_ / _ham_). Depois, Mostramos  as **primeiras 5 linhas** do DataFrame, só pra inspecionar o formato e ver se os dados estão ok. 
> 
> Também Retornamos o **tamanho da tabela** (linhas, colunas). Exemplo: `(5572, 2)` → 5572 mensagens e 2 colunas, e por último, contamos **quantas mensagens há em cada categoria**.
> ```python
   span = pd.read_csv("spam.csv")
   span.head()
   span.shape
   span['Category'].value_counts()
> ```
> [!element] **Definição das variáveis e vetorização do texto**
>
> Primeiro, definimos as **mensagens** (entradas) e as **classes** (rótulos):
> 
> ```python
> previ = span['Message']   # textos das mensagens
> classe = span['Category'] # rótulo: 'spam' ou 'ham'
> ```
> 
> Depois criamos o **vetorizador TF-IDF**, que transforma texto em números para o modelo poder entender:
> 
> ```python
> from sklearn.feature_extraction.text import TfidfVectorizer
> 
> vetorizador = TfidfVectorizer()
> previsores = vetorizador.fit_transform(previ)
> print(previsores.shape)
> print(vetorizador.get_feature_names_out()[10:100])
> ```
>
> 📊 **O que acontece aqui:**
> 
> - `fit_transform(previ)` analisa todas as palavras do dataset (`fit`) e cria uma matriz numérica (`transform`);
   >  
> - Cada linha representa uma mensagem;
   >  
> - Cada coluna, uma palavra única;
>
> - `print(previsores.shape)` mostra o tamanho dessa matriz (ex.: `(5572, 8713)` → 5572 mensagens e 8713 palavras únicas).    

> [!element] **Treinamento do modelo Random Forest**
> 
> Agora dividimos os dados entre **treino** e **teste**, e treinamos o modelo.

```python
from sklearn.model_selection import train_test_split
from sklearn.ensemble import RandomForestClassifier

# Divide os dados (70% treino, 30% teste)
X_treinamento, X_teste, y_treinamento, y_teste = train_test_split(
    previsores, classe, test_size=0.3
)

print(X_teste.shape)
```

Em seguida, criamos o modelo **Random Forest** com 500 árvores e treinamos ele nos dados:

```python
floresta = RandomForestClassifier(n_estimators=500)
floresta.fit(X_treinamento, y_treinamento)
```

---

📊 **Explicação rápida:**

- `train_test_split`: separa os dados em treino e teste pra avaliar o desempenho do modelo.
    
- `RandomForestClassifier`: cria um conjunto de árvores de decisão.
    
- `n_estimators=500`: define **quantas árvores** o modelo vai usar (quanto mais, geralmente mais estável).
    
- `fit(...)`: treina o modelo com os dados de treino.
    

💡 Dica: depois disso, tu pode avaliar o modelo assim:

```python
from sklearn.metrics import accuracy_score, confusion_matrix

previsoes = floresta.predict(X_teste)
print('Acurácia:', accuracy_score(y_teste, previsoes))
print('Matriz de confusão:\\n', confusion_matrix(y_teste, previsoes))
print(accuracy_score(y_test, previsoes))
print(metrics.classification_report(y_test, previsoes))
```
