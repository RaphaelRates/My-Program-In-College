 tags: #machine-learning #regression #linear-models #statistics #python

# 📈 Regressão Linear: Teoria e Implementação

> [!abstract] Visão Geral **Regressão Linear** é um algoritmo de machine learning supervisionado usado para prever valores contínuos. O objetivo é encontrar uma relação linear entre as variáveis de entrada (features) e a variável de saída (target), permitindo fazer previsões sobre novos dados.

## 🎯 Fundamentos Teóricos

A regressão linear parte de uma ideia simples: modelar a relação entre variáveis através de uma linha reta (ou hiperplano, em múltiplas dimensões). Imagine que você quer prever o preço de uma casa baseado em seu tamanho. Intuitivamente, sabemos que casas maiores tendem a ser mais caras, e essa relação pode ser aproximada por uma reta.

### A Equação Fundamental

No caso mais simples, com uma única variável de entrada, temos:

```
y = β₀ + β₁x + ε
```

Aqui, **y** é o valor que queremos prever (como o preço da casa), **x** é a variável que conhecemos (o tamanho), **β₀** é onde a reta corta o eixo vertical (intercepto), e **β₁** determina a inclinação da reta. O termo **ε** representa o erro aleatório, reconhecendo que nosso modelo nunca será perfeito.

Quando temos múltiplas variáveis de entrada, a equação se expande naturalmente:

```
y = β₀ + β₁x₁ + β₂x₂ + ... + βₙxₙ + ε
```

Agora, em vez de uma linha em 2D, estamos trabalhando com um hiperplano em múltiplas dimensões. Cada **βᵢ** nos diz quanto **y** muda quando **xᵢ** aumenta em uma unidade, mantendo as outras variáveis constantes.

## 📊 O Método dos Mínimos Quadrados

A grande questão é: como encontrar os melhores valores para **β₀** e **β₁**? O método dos mínimos quadrados responde isso de forma elegante. A ideia é minimizar a soma dos quadrados dos erros de previsão.

### Entendendo os Resíduos

Para cada ponto de dado que temos, podemos calcular a diferença entre o valor real e o valor previsto pelo nosso modelo. Essa diferença é chamada de **resíduo**:

```
resíduo = y_observado - y_previsto
```

O método dos mínimos quadrados busca os parâmetros que minimizam a soma dos quadrados desses resíduos. Por que elevar ao quadrado? Primeiro, porque assim erros positivos e negativos não se cancelam. Segundo, porque penaliza mais fortemente erros grandes, o que geralmente é desejável.

Matematicamente, queremos minimizar:

```
SSE = Σ(yᵢ - ŷᵢ)² = Σ(yᵢ - β₀ - β₁xᵢ)²
```

### A Solução Analítica

A beleza da regressão linear é que existe uma solução fechada, derivada através de cálculo diferencial. Quando organizamos nossos dados em matrizes, a solução fica:

```
β = (XᵀX)⁻¹Xᵀy
```

Esta é a famosa **Equação Normal**. Aqui, **X** é nossa matriz de features (com uma coluna de 1s adicionada para o intercepto), **y** é o vetor de targets, e **β** contém todos os coeficientes que procuramos. A operação **Xᵀ** significa transposta de X, e o termo **(XᵀX)⁻¹** é a inversa da matriz XᵀX.

A intuição por trás dessa fórmula vem do cálculo multivariado. Quando derivamos o SSE em relação a cada coeficiente e igualamos a zero (buscando o mínimo), chegamos naturalmente a esta expressão. É análogo a encontrar o ponto mais baixo de um vale - onde a derivada é zero.

```python
import numpy as np

def linear_regression_closed_form(X, y):
    """
    Calcula os coeficientes da regressão usando a equação normal.
    Esta é a forma analítica exata - não requer iterações.
    """
    # Adiciona coluna de 1s para o intercepto
    X_with_intercept = np.column_stack([np.ones(X.shape[0]), X])
    
    # Aplica a equação normal: β = (XᵀX)⁻¹Xᵀy
    beta = np.linalg.inv(X_with_intercept.T @ X_with_intercept) @ X_with_intercept.T @ y
    
    return beta

# Exemplo prático
X = np.array([[1], [2], [3], [4], [5]])
y = np.array([2, 4, 5, 4, 5])

coeficientes = linear_regression_closed_form(X, y)
print(f"Intercepto (β₀): {coeficientes[0]:.2f}")
print(f"Coeficiente (β₁): {coeficientes[1]:.2f}")
```

## 🔄 Gradiente Descendente: Uma Abordagem Alternativa

Embora a equação normal forneça a solução exata, ela tem limitações práticas. Quando temos milhões de features, calcular **(XᵀX)⁻¹** torna-se computacionalmente inviável (complexidade O(n³)). É aqui que o gradiente descendente se torna valioso.

### A Ideia Geométrica

Imagine que você está em uma montanha enevoada e quer chegar ao vale (o ponto de erro mínimo). Sem enxergar o caminho completo, você sente a inclinação do terreno sob seus pés e dá um passo na direção mais íngreme para baixo. Repete esse processo até não conseguir descer mais. Isso é gradiente descendente.

Matematicamente, começamos com valores aleatórios para os parâmetros e os atualizamos iterativamente:

```
β := β - α · ∇J(β)
```

Aqui, **α** é a taxa de aprendizado (o tamanho do passo), e **∇J(β)** é o gradiente da função de custo (a direção da "ladeira"). O gradiente nos diz como o erro muda quando alteramos cada parâmetro.

Para a regressão linear, os gradientes são:

```
∂J/∂β₀ = (1/m) · Σ(ŷᵢ - yᵢ)
∂J/∂βⱼ = (1/m) · Σ(ŷᵢ - yᵢ) · xᵢⱼ
```

Onde **m** é o número de amostras. Note que o gradiente é proporcional ao erro e à feature correspondente.

```python
class LinearRegressionGD:
    def __init__(self, learning_rate=0.01, n_iter=1000):
        self.learning_rate = learning_rate
        self.n_iter = n_iter
        self.coef_ = None
        self.intercept_ = None
        self.loss_history = []
        
    def fit(self, X, y):
        """
        Treina o modelo iterativamente usando gradiente descendente.
        A cada iteração, damos um passo na direção que reduz o erro.
        """
        n_samples, n_features = X.shape
        
        # Inicializa parâmetros com zeros (ponto de partida arbitrário)
        self.coef_ = np.zeros(n_features)
        self.intercept_ = 0
        
        for iteration in range(self.n_iter):
            # Calcula previsões com parâmetros atuais
            y_pred = self.intercept_ + X @ self.coef_
            
            # Calcula o erro
            error = y_pred - y
            
            # Calcula gradientes (direção da "ladeira")
            grad_intercept = (1/n_samples) * np.sum(error)
            grad_coef = (1/n_samples) * (X.T @ error)
            
            # Atualiza parâmetros dando um passo contrário ao gradiente
            self.intercept_ -= self.learning_rate * grad_intercept
            self.coef_ -= self.learning_rate * grad_coef
            
            # Registra o erro (para monitorar convergência)
            loss = np.mean(error ** 2)
            self.loss_history.append(loss)
            
            if iteration % 100 == 0:
                print(f"Iteração {iteration}, Loss: {loss:.4f}")
        
        return self
    
    def predict(self, X):
        return self.intercept_ + X @ self.coef_

# Exemplo de uso
X = np.array([[1], [2], [3], [4], [5]])
y = np.array([2, 4, 5, 4, 5])

model_gd = LinearRegressionGD(learning_rate=0.01, n_iter=1000)
model_gd.fit(X, y)

print(f"\nIntercepto final: {model_gd.intercept_:.2f}")
print(f"Coeficiente final: {model_gd.coef_[0]:.2f}")
```

## 🛠️ Implementação Completa

Agora, vamos construir uma classe completa de regressão linear que inclui todos os métodos necessários para treinamento, previsão e avaliação:

```python
import numpy as np
import matplotlib.pyplot as plt

class LinearRegression:
    def __init__(self):
        self.coef_ = None
        self.intercept_ = None
        
    def fit(self, X, y):
        """
        Treina o modelo usando a equação normal.
        É um processo direto: organiza os dados e resolve o sistema linear.
        """
        X_with_intercept = np.column_stack([np.ones(X.shape[0]), X])
        
        # Resolve o sistema linear de uma vez
        coefficients = np.linalg.inv(X_with_intercept.T @ X_with_intercept) @ X_with_intercept.T @ y
        
        self.intercept_ = coefficients[0]
        self.coef_ = coefficients[1:]
        
        return self
    
    def predict(self, X):
        """
        Faz previsões: simplesmente aplica a equação linear aprendida.
        """
        if self.coef_ is None:
            raise ValueError("Modelo não foi treinado ainda")
            
        return self.intercept_ + X @ self.coef_
    
    def score(self, X, y):
        """
        Calcula o R² (coeficiente de determinação).
        
        R² mede a proporção da variância explicada pelo modelo.
        R² = 1 significa previsão perfeita.
        R² = 0 significa que o modelo não é melhor que prever a média.
        """
        y_pred = self.predict(X)
        ss_res = np.sum((y - y_pred) ** 2)  # Soma dos quadrados dos resíduos
        ss_tot = np.sum((y - np.mean(y)) ** 2)  # Variância total
        r_squared = 1 - (ss_res / ss_tot)
        
        return r_squared

# Demonstração com dados sintéticos
np.random.seed(42)
X = 2 * np.random.rand(100, 1)
y = 4 + 3 * X + np.random.randn(100, 1)  # Relação linear verdadeira: y = 4 + 3x + ruído

model = LinearRegression()
model.fit(X, y)

print(f"Coeficientes aprendidos:")
print(f"  Intercepto: {model.intercept_:.2f} (real: 4.0)")
print(f"  Coeficiente: {model.coef_[0]:.2f} (real: 3.0)")
print(f"  R² Score: {model.score(X, y):.2f}")
```

## 📊 Avaliando a Qualidade do Modelo

### Métricas de Erro

Diferentes métricas capturam aspectos diferentes do desempenho do modelo. O **MSE** (Mean Squared Error) penaliza fortemente erros grandes devido ao quadrado, enquanto o **MAE** (Mean Absolute Error) trata todos os erros de forma mais uniforme. O **RMSE** (Root Mean Squared Error) é o MSE na mesma unidade da variável target, facilitando a interpretação.

```python
class RegressionMetrics:
    @staticmethod
    def mse(y_true, y_pred):
        """
        MSE: média dos quadrados dos erros.
        Penaliza mais fortemente erros grandes.
        """
        return np.mean((y_true - y_pred) ** 2)
    
    @staticmethod
    def rmse(y_true, y_pred):
        """
        RMSE: raiz do MSE.
        Mesma unidade da variável target, mais interpretável.
        """
        return np.sqrt(RegressionMetrics.mse(y_true, y_pred))
    
    @staticmethod
    def mae(y_true, y_pred):
        """
        MAE: média dos valores absolutos dos erros.
        Mais robusto a outliers que MSE.
        """
        return np.mean(np.abs(y_true - y_pred))
    
    @staticmethod
    def r2_score(y_true, y_pred):
        """
        R²: proporção da variância explicada.
        Varia de -∞ a 1, onde 1 é perfeito.
        """
        ss_res = np.sum((y_true - y_pred) ** 2)
        ss_tot = np.sum((y_true - np.mean(y_true)) ** 2)
        return 1 - (ss_res / ss_tot) if ss_tot != 0 else 0

# Avaliação completa
y_pred = model.predict(X)

print("\n=== Métricas de Avaliação ===")
print(f"MSE:  {RegressionMetrics.mse(y, y_pred):.4f}")
print(f"RMSE: {RegressionMetrics.rmse(y, y_pred):.4f}")
print(f"MAE:  {RegressionMetrics.mae(y, y_pred):.4f}")
print(f"R²:   {RegressionMetrics.r2_score(y, y_pred):.4f}")
```

## 📈 Análise de Resíduos

Os resíduos (diferenças entre valores reais e previstos) revelam muito sobre a qualidade do modelo. Idealmente, eles devem ser aleatórios, com média zero e variância constante. Padrões nos resíduos indicam problemas: se eles formam uma curva, talvez a relação não seja linear; se a dispersão aumenta com os valores previstos, temos heterocedasticidade.

```python
def analyze_residuals(X, y, model):
    """
    Analisa resíduos para verificar se o modelo é adequado.
    """
    y_pred = model.predict(X)
    residuals = y - y_pred
    
    fig, axes = plt.subplots(1, 2, figsize=(12, 5))
    
    # Resíduos vs valores previstos
    # Devemos ver pontos aleatórios em torno de zero
    axes[0].scatter(y_pred, residuals, alpha=0.6)
    axes[0].axhline(y=0, color='red', linestyle='--')
    axes[0].set_xlabel('Valores Previstos')
    axes[0].set_ylabel('Resíduos')
    axes[0].set_title('Resíduos vs Preditos')
    axes[0].grid(True, alpha=0.3)
    
    # Distribuição dos resíduos
    # Deve ser aproximadamente normal (simétrica, centrada em zero)
    axes[1].hist(residuals, bins=20, alpha=0.7, edgecolor='black')
    axes[1].set_xlabel('Resíduos')
    axes[1].set_ylabel('Frequência')
    axes[1].set_title('Distribuição dos Resíduos')
    axes[1].grid(True, alpha=0.3)
    
    plt.tight_layout()
    plt.show()
    
    print(f"Média dos resíduos: {np.mean(residuals):.4f} (deve estar próximo de 0)")
    print(f"Desvio padrão: {np.std(residuals):.4f}")

analyze_residuals(X, y, model)
```

## 🎯 Pressupostos e Limitações

A regressão linear faz algumas suposições importantes sobre os dados. Primeiro, assume que existe uma relação linear entre as variáveis - se a relação verdadeira é exponencial ou logarítmica, o modelo terá baixo desempenho. Segundo, assume que as observações são independentes umas das outras. Terceiro, requer que a variância dos erros seja constante (homocedasticidade) - erros maiores para valores grandes indicam violação. Quarto, idealmente os resíduos seguem uma distribuição normal. Por fim, quando há múltiplas features, elas não devem ser altamente correlacionadas (multicolinearidade), pois isso torna os coeficientes instáveis.

Verificar esses pressupostos é crucial. Um modelo que viola essas suposições pode fazer previsões ruins ou fornecer inferências estatísticas incorretas, mesmo que os números de avaliação pareçam bons à primeira vista.

## 🚀 Exemplo Prático: Previsão de Preços

Vamos aplicar tudo isso a um exemplo realista - prever preços de imóveis baseado no tamanho:

```python
# Simula dados de preços de casas
np.random.seed(42)
tamanho_m2 = np.random.normal(150, 50, 100)  # Tamanhos em m²
preco_mil = 50 + 3 * tamanho_m2 + np.random.normal(0, 20, 100)  # Preço em milhares

X_casas = tamanho_m2.reshape(-1, 1)
y_casas = preco_mil

# Treina modelo
model_casas = LinearRegression()
model_casas.fit(X_casas, y_casas)

print("=== Modelo de Previsão de Preços ===")
print(f"Preço base: R$ {model_casas.intercept_:.2f} mil")
print(f"Valor por m²: R$ {model_casas.coef_[0]:.2f} mil/m²")
print(f"R²: {model_casas.score(X_casas, y_casas):.2f}")

# Prevê preço de casa de 200m²
nova_casa = np.array([[200]])
preco_previsto = model_casas.predict(nova_casa)
print(f"\nPrevisão para casa de 200m²: R$ {preco_previsto[0]:.2f} mil")
```

---

> [!summary] Conclusão A regressão linear combina simplicidade conceitual com rigor matemático. Sua solução fechada através da equação normal fornece resultados exatos e interpretáveis, enquanto o gradiente descendente oferece escalabilidade para grandes datasets. Compreender profundamente este modelo é fundamental, pois serve de base para técnicas mais avançadas de machine learning.

## 🔗 Tópicos Relacionados

- [[Gradient-Descent-Explained]] - Aprofundamento em otimização
- [[Regularization-Methods]] - Ridge, Lasso e ElasticNet
- [[Polynomial-Regression]] - Capturando relações não-lineares
- [[Feature-Engineering]] - Preparação de dados para regressão