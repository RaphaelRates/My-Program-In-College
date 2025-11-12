> [!abstract] Classificação Bayes
> Um modelo de Probabilidade Simples baseado no Teorema de Bayes, onde **Calcula a probabilidade** de um objeto sobre uma determinada rotulação

![[Pasted image 20250811174704.png]]

> [!info] ## Teorema de Bayes
> 
> O Teorema de Bayes é dado por:
> 
> $$P(\text{Classe} \mid \text{Características}) = \frac{P(\text{Características} \mid \text{Classe}) \times P(\text{Classe})}{P(\text{Características})}$$
> - $P(\text{Classe} \mid \text{Características})$: probabilidade *posterior* — chance da amostra pertencer a uma classe dado as características observadas.  
> - $P(\text{Características} \mid \text{Classe})$: probabilidade *verossímil* — chance das características aparecerem na classe.  
> - $P(\text{Classe})$: probabilidade *a priori* — conhecimento prévio ou frequência da classe.  
> - $P(\text{Características})$: probabilidade das características — fator de normalização.

> [!tip] ESTIMATIVA DE VEROSIMILHANÇA
>  A estimativa de máxima verossimilhança (MLE) é um método estatístico para estimar parâmetros de um modelo, encontrando os valores que tornam os dados observados mais prováveis sob esse modelo. Essa abordagem é amplamente utilizada por sua intuição e flexibilidade, podendo ser resolvida analiticamente quando a função de verossimilhança é diferenciável. Na inferência bayesiana, a MLE corresponde à estimativa máxima a posteriori (MAP) com uma priori uniforme, e, no paradigma frequentista, é um tipo de estimador de extremo que maximiza a verossimilhança dos dados.

---

> [!example] ## Exemplo ilustrativo — filtragem de e-mails
> 
> Suponha que queremos determinar a probabilidade de um e-mail ser **spam** com base nas palavras **“oferta”** e **“grátis”**.
> - Probabilidade a priori:  
  $$P(\text{spam}) = 0.2$$
>
> - Probabilidades condicionais:  $$P(\text{oferta} \mid \text{spam} = 0.5$$$$P(\text{grátis} \mid \text{spam}) = 0.3$$
>   
> ## Hipótese de independência ingênua
> O classificador assume que as palavras “oferta” e “grátis” são independentes dentro da classe spam.
> 
> ## Cálculo da probabilidade conjunta
> 
> A probabilidade do e-mail ser spam dado que contém “oferta” e “grátis” é proporcional a:$$P(\text{spam} \mid \text{oferta}, \text{grátis}) \propto P(\text{oferta} \mid \text{spam}) \times P(\text{grátis} \mid \text{spam}) \times P(\text{spam}) = 0.5 \times 0.3 \times 0.2 = 0.03$$
> 
> Compara-se com a probabilidade do e-mail não ser spam para decidir a classificação.

tags: #machine-learning, #classification, #naive-bayes
# Tipos de Classificadores Naive Bayes

> [!note] Contexto
> Os classificadores Naive Bayes variam conforme o tipo de dado analisado. Cada tipo assume uma distribuição probabilística diferente para as features.

## 1. Multinomial Naive Bayes
> [!summary] Uso principal
> **Dados discretos de contagem**, especialmente em processamento de linguagem natural (NLP).

**Características**:
- Assume distribuição multinomial (contagens de eventos).
- Ideal para **bag-of-words** em textos.
- Exemplo: Classificar e-mails como spam com base na frequência de palavras.

**Fórmula Chave**:
$$
P(x_i \mid y) = \frac{N_{x_i,y} + \alpha}{N_y + \alpha n}
$$
![[Pasted image 20250811182206.png]]
- $\alpha$: Suavização de Laplace
- $n$: Número de features
![[Pasted image 20250811182241.png]]

  ### Implementações de Naive Bayes no scikit-learn
```python
# Importações básicas
import numpy as np
from sklearn.naive_bayes import MultinomialNB, GaussianNB, BernoulliNB
from sklearn.feature_extraction.text import CountVectorizer
from sklearn.model_selection import train_test_split
from sklearn.metrics import accuracy_score

emails = [
    "oferta incrível grátis ganhe prêmio",
    "reunião projeto quarta-feira",
    "ganhe dinheiro rápido clique aqui",
    "relatório mensal anexado"
]
labels = [1, 0, 1, 0]  
# Vetorização (contagem de palavras)
vectorizer = CountVectorizer()
X = vectorizer.fit_transform(emails)
y = np.array(labels)

# Divisão treino-teste
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.25)

# Modelo
model = MultinomialNB(alpha=1.0)  # alpha = suavização Laplace
model.fit(X_train, y_train)

# Predição
pred = model.predict(X_test)
print(f"Acurácia: {accuracy_score(y_test, pred):.2f}")

# Exemplo de uso
novo_email = ["ganhe prêmio grátis agora"]
print(model.predict(vectorizer.transform(novo_email)))  # Saída: [1] (spam)
# Dados de exemplo - [pressão_sanguínea, glicose, idade]
```
## 2. Gaussian Naive Bayes
> [!summary] Uso principal
> **Features contínuas** que seguem distribuição normal.

```python
# Importações básicas
import numpy as np
from sklearn.naive_bayes import MultinomialNB, GaussianNB, BernoulliNB
from sklearn.feature_extraction.text import CountVectorizer
from sklearn.model_selection import train_test_split
from sklearn.metrics import accuracy_score

dados = np.array([
    [120, 90, 35],
    [140, 150, 55],
    [115, 85, 30],
    [160, 180, 60]
])
diagnosticos = np.array([0, 1, 0, 1])  # 0 = saudável, 1 = diabético

# Divisão
X_train, X_test, y_train, y_test = train_test_split(dados, diagnosticos)

# Modelo
model = GaussianNB()
model.fit(X_train, y_train)

# Avaliação
pred = model.predict(X_test)
print(f"Acurácia: {accuracy_score(y_test, pred):.2f}")

# Exemplo
novo_paciente = [[130, 140, 50]]
print(model.predict(novo_paciente))  # Saída: [1] (diabético)
```
**Características**:
- Assume que os dados são gerados por uma distribuição gaussiana.
- Parâmetros estimados: média ($\mu$) e variância ($\sigma^2$).
- Exemplo: Diagnóstico médico com exames laboratoriais (valores contínuos).

**Fórmula Chave**:
$$
P(x_i \mid y) = \frac{1}{\sqrt{2\pi\sigma_y^2}} \exp\left(-\frac{(x_i - \mu_y)^2}{2\sigma_y^2}\right)
$$
![[Pasted image 20250811182358.png]]
## 3. Bernoulli Naive Bayes
> [!summary] Uso principal
> **Dados binários** (presença/ausência de features).

**Características**:
- Modela features como variáveis booleanas.
- Comum em análise de textos com vetores binários (ex.: "contém a palavra? Sim/Não").
- Exemplo: Detecção de sentimentos em reviews (palavras-chave presentes/ausentes).

**Fórmula Chave**:
$$
P(x_i \mid y) = P(i \mid y)^{x_i} \times (1 - P(i \mid y))^{1-x_i}
$$

![[Pasted image 20250811182309.png]]
```python
# Importações básicas
import numpy as np
from sklearn.naive_bayes import MultinomialNB, GaussianNB, BernoulliNB
from sklearn.feature_extraction.text import CountVectorizer
from sklearn.model_selection import train_test_split
from 
## Comparação entre Tipos
# Dados de exemplo - reviews (1 = palavra presente)
# Features: ["bom", "ruim", "excelente", "péssimo"]
reviews = np.array([
    [1, 0, 1, 0],  # "bom excelente"
    [0, 1, 0, 1],  # "ruim péssimo"
    [1, 0, 0, 0],  # "bom"
    [0, 0, 1, 0]   # "excelente"
])
sentimentos = np.array([1, 0, 1, 1])  # 1 = positivo, 0 = negativo

# Modelo
model = BernoulliNB(binarize=None)  # Já está binarizado
model.fit(reviews, sentimentos)

# Predição
novo_review = [[1, 0, 0, 0]]  # Contém "bom"
print(model.predict(novo_review))  # Saída: [1] (positivo)
```

| Tipo          | Dados               | Aplicação Típica       | Vantagens                          | Limitações                  |
|---------------|---------------------|------------------------|------------------------------------|-----------------------------|
| [[#Multinomial Naive Bayes\|Multinomial]] | Contagens discretas | NLP, classificação de texto | Lida bem com múltiplas ocorrências | Não aplicável a contínuos   |
| [[#Gaussian Naive Bayes\|Gaussian]]   | Contínuos           | Dados numéricos (ex.: saúde) | Funciona com distribuições normais | Sensível a outliers         |
| [[#Bernoulli Naive Bayes\|Bernoulli]] | Binários (0/1)      | Features booleanas      | Simples para dados de presença     | Perde informação de frequência |

> [!warning] Nota Importante
> A escolha do tipo depende **diretamente** da natureza dos dados:
> - Use `Gaussian` para medidas físicas (ex.: temperatura, peso).
> - Prefira `Multinomial` para contagens (ex.: palavras em documentos).
> - `Bernoulli` é ideal para features binárias categóricas.

---

> [!example] ## Exemplo 
```python
# Exemplo de código vinculado (opcional)
from sklearn.naive_bayes import MultinomialNB
X = [[1, 2, 0], [0, 1, 3]]  # Contagens de palavras
y = ['spam', 'não-spam']
model = MultinomialNB().fit(X, y)
```
