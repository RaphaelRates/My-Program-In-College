# Métodos de Classificação

> [!abstract] ## 📚 Definição 
> **Classificação** é um tipo de aprendizado supervisionado onde o objetivo é prever rótulos discretos (categorias) com base em features de entrada.
> 
> Ela divide objetos com base em atributos conhecidos.

> [!example] **Exemplos de Aplicações**:
> - Diagnóstico médico (doente/saudável)
> - Detecção de spam (spam/não-spam)
> - Reconhecimento de dígitos manuscritos (0-9)
> - Filtragem de spam  
> - Detecção de idioma  
> - Pesquisa de documentos semelhantes  
> - Análise de sentimentos  
> - Reconhecimento de caracteres e números

---
## 🧠 Principais Algoritmos

### 1. 📈 [[ML - Regressão Linear]]
```python
from sklearn.linear_model import LogisticRegression
model = LogisticRegression()
model.fit(X_train, y_train)
```
- **Prós**: Simples, interpretável, bom para problemas lineares
- **Contras**: Não captura relações não-lineares complexas

### 2. 🌳 [[ML - Arvores de Decisão]]
```python
from sklearn.tree import DecisionTreeClassifier
model = DecisionTreeClassifier(max_depth=3)
```
- **Prós**: Interpretável, lida bem com features categóricas
- **Contras**: Tendência a overfitting

### 3. 🌲[[ML - Random Florest]]
```python
from sklearn.ensemble import RandomForestClassifier
model = RandomForestClassifier(n_estimators=100)
```
- **Prós**: Reduz overfitting, boa performance geral
- **Contras**: Menos interpretável que árvores simples

### 4. 🎯 [[ML - SVM Maquina Vetorial de Suporte]]
```python
from sklearn.svm import SVC
model = SVC(kernel='rbf')
```
- **Prós**: Eficaz em espaços de alta dimensão
- **Contras**: Computacionalmente intensivo

### 5. 🧠 [[ML - Naive Bayes]]
```python
from sklearn.naive_bayes import GaussianNB
model = GaussianNB()
```
- **Prós**: Captura padrões complexos
- **Contras**: Requer muitos dados e tuning
 

---

## 📊 Métricas de Avaliação
| Métrica               | Fórmula                     | Quando Usar                 |
|-----------------------|----------------------------|----------------------------|
| **Acurácia**          | (TP+TN)/(TP+TN+FP+FN)      | Classes balanceadas         |
| **Precisão**          | TP/(TP+FP)                 | Custo de FP alto           |
| **Recall**            | TP/(TP+FN)                 | Custo de FN alto           |
| **F1-Score**          | 2*(Precision*Recall)/(Precision+Recall) | Balancear P/R |
| **Matriz de Confusão**| -                          | Análise detalhada          |

---

## 🛠️ Fluxo de Trabalho Típico
1. **Pré-processamento**  
   - [[Limpeza de Dados]]
   - [[Feature Engineering]]
   - [[Balanceamento de Classes]] (SMOTE/Undersampling)

2. **Modelagem**  
   - [[Seleção de Algoritmo]]
   - [[Validação Cruzada]]

3. **Avaliação**  
   - [[Teste com Dados Não Vistos]]
   - [[Interpretação de Resultados]]

---
## 💡 Dicas Práticas
- Comece sempre com **modelos simples** antes de ir para redes neurais
- Use **regularização** (L1/L2) para evitar overfitting
- Para classes desbalanceadas, priorize **F1-Score** em vez de acurácia
- Visualize decisões com **SHAP/LIME** para modelos complexos

> [!example] Exemplo Prático  
> ```python
> # Pipeline completo com Scikit-learn
> from sklearn.pipeline import make_pipeline
> from sklearn.preprocessing import StandardScaler
> 
> pipe = make_pipeline(
>     StandardScaler(),
>     RandomForestClassifier()
> )
> pipe.fit(X_train, y_train)
> print(pipe.score(X_test, y_test))
> ```
