tags: #machine-learning #decision-trees #classification #regression #supervised-learning

# 🌳 Árvores de Decisão: Teoria e Implementação

> [!abstract] Visão Geral **Árvores de Decisão** são algoritmos de machine learning supervisionado que tomam decisões através de uma estrutura hierárquica de perguntas. Funcionam tanto para classificação quanto para regressão, sendo altamente interpretáveis e intuitivos - cada previsão pode ser explicada através do caminho percorrido na árvore.

## 🎯 A Ideia Fundamental

> [!question] Como Funciona? Imagine que você quer decidir se deve jogar tênis baseado no clima. Você começa perguntando: "Está chovendo?". Se sim, não joga. Se não, pergunta: "Está muito quente?". E assim por diante. Cada pergunta divide o problema em casos menores, até chegar a uma resposta definitiva.

As árvores de decisão funcionam exatamente assim. O algoritmo constrói automaticamente uma série de perguntas (testes em features) que dividem os dados de forma ótima. Cada divisão busca separar os exemplos de classes diferentes (classificação) ou reduzir a variância dos valores (regressão).

> [!tip] Intuição Visual
> 
> ```
>                   [Chovendo?]
>                   /         \
>               Sim /           \ Não
>                 /               \
>           [Não Joga]      [Temp > 30°C?]
>                            /           \
>                        Sim /             \ Não
>                          /                 \
>                  [Não Joga]              [Joga]
> ```

A beleza está na simplicidade: cada decisão é uma pergunta binária clara. O caminho da raiz até a folha conta uma história compreensível sobre por que aquela previsão foi feita.

## 📊 Fundamentos Matemáticos

> [!important] Conceitos Chave Para construir uma árvore ótima, precisamos de uma forma matemática de medir "quão boa" é uma divisão. Isso nos leva a três conceitos fundamentais: entropia, ganho de informação e impureza de Gini.

### Entropia: Medindo a Desordem

A entropia vem da teoria da informação e mede o grau de "surpresa" ou "desordem" em um conjunto de dados. Se todos os exemplos são da mesma classe, não há surpresa - a entropia é zero. Se as classes estão igualmente distribuídas, a entropia é máxima.

> [!formula] Entropia
> 
> ```
> H(S) = -Σ pᵢ · log₂(pᵢ)
> 
> Onde:
> - S é o conjunto de exemplos
> - pᵢ é a proporção da classe i
> - A soma percorre todas as classes
> ```

Para um problema binário com proporção p de positivos:

```
H = -p·log₂(p) - (1-p)·log₂(1-p)
```

> [!example] Interpretando Entropia
> 
> - **H = 0**: Todos exemplos são da mesma classe (certeza total)
> - **H = 1**: Classes perfeitamente balanceadas em binário (máxima incerteza)
> - **0 < H < 1**: Alguma predominância de uma classe

```python
import numpy as np

def entropy(y):
    """
    Calcula a entropia de um conjunto de labels.
    Quanto maior a entropia, mais "misturadas" as classes.
    """
    # Conta a frequência de cada classe
    _, counts = np.unique(y, return_counts=True)
    
    # Calcula proporções
    probabilities = counts / len(y)
    
    # Aplica a fórmula da entropia
    # Evita log(0) filtrando probabilidades zero
    entropy_value = -np.sum([p * np.log2(p) for p in probabilities if p > 0])
    
    return entropy_value

# Exemplo: dataset perfeitamente balanceado
y_balanced = np.array([0, 0, 1, 1])
print(f"Entropia (balanceado): {entropy(y_balanced):.3f}")  # ~1.0

# Exemplo: dataset desbalanceado
y_skewed = np.array([0, 0, 0, 1])
print(f"Entropia (desbalanceado): {entropy(y_skewed):.3f}")  # ~0.81

# Exemplo: dataset puro
y_pure = np.array([0, 0, 0, 0])
print(f"Entropia (puro): {entropy(y_pure):.3f}")  # 0.0
```

### Ganho de Informação

O ganho de informação mede quanto uma divisão reduz a entropia. É a diferença entre a entropia antes e depois de dividir os dados por uma feature específica.

> [!formula] Ganho de Informação
> 
> ```
> IG(S, A) = H(S) - Σ (|Sᵥ|/|S|) · H(Sᵥ)
> 
> Onde:
> - S é o conjunto original
> - A é o atributo usado para dividir
> - Sᵥ são os subconjuntos resultantes
> - |Sᵥ|/|S| pondera pela proporção de exemplos
> ```

> [!note] Interpretação O ganho de informação responde: "Quanto de incerteza eu removo ao fazer esta pergunta?". Queremos escolher a pergunta que remove o máximo de incerteza possível.

```python
def information_gain(X, y, feature_idx, threshold):
    """
    Calcula o ganho de informação ao dividir dados por um threshold.
    
    Este é o critério usado pelo algoritmo ID3 clássico.
    """
    # Entropia antes da divisão
    parent_entropy = entropy(y)
    
    # Divide os dados
    left_mask = X[:, feature_idx] <= threshold
    right_mask = ~left_mask
    
    # Se a divisão não separa nada, ganho é zero
    if len(y[left_mask]) == 0 or len(y[right_mask]) == 0:
        return 0
    
    # Entropia ponderada após a divisão
    n = len(y)
    n_left, n_right = len(y[left_mask]), len(y[right_mask])
    
    child_entropy = (n_left/n) * entropy(y[left_mask]) + \
                    (n_right/n) * entropy(y[right_mask])
    
    # Ganho = redução na entropia
    return parent_entropy - child_entropy
```

### Índice de Gini

O índice de Gini é uma alternativa à entropia, computacionalmente mais eficiente. Mede a probabilidade de classificar incorretamente um elemento escolhido aleatoriamente.

> [!formula] Impureza de Gini
> 
> ```
> Gini(S) = 1 - Σ pᵢ²
> 
> Onde pᵢ é a proporção da classe i
> ```

> [!tip] Comparação: Gini vs Entropia
> 
> - **Gini** é mais rápido de calcular (sem logaritmos)
> - **Entropia** é teoricamente mais fundamentada (teoria da informação)
> - Na prática, ambos produzem árvores muito similares
> - Gini tende a isolar a classe mais frequente, entropia produz árvores mais balanceadas

```python
def gini_impurity(y):
    """
    Calcula o índice de Gini - alternativa mais rápida à entropia.
    """
    _, counts = np.unique(y, return_counts=True)
    probabilities = counts / len(y)
    
    # Gini = 1 - soma dos quadrados das probabilidades
    return 1 - np.sum(probabilities ** 2)

# Comparando Gini e Entropia
y_test = np.array([0, 0, 1, 1, 1])
print(f"Gini: {gini_impurity(y_test):.3f}")
print(f"Entropia: {entropy(y_test):.3f}")
```

## 🌲 Construindo a Árvore

> [!warning] O Algoritmo de Construção A construção da árvore é um processo recursivo chamado **divisão recursiva binária**. Começamos com todos os dados na raiz e repetidamente escolhemos a melhor divisão até atingir um critério de parada.

### O Processo Passo a Passo

O algoritmo segue uma estratégia gulosa (greedy): a cada nó, escolhe a melhor divisão possível naquele momento, sem olhar para o futuro. Isso não garante a árvore globalmente ótima, mas é computacionalmente viável.

> [!info] Etapas da Construção
> 
> 1. **Avaliar todas divisões possíveis**: Para cada feature, testa todos os valores únicos como threshold
> 2. **Escolher a melhor divisão**: Aquela com maior ganho de informação (ou redução de Gini)
> 3. **Dividir os dados**: Cria dois nós filhos com os subconjuntos resultantes
> 4. **Recursão**: Repete o processo para cada filho
> 5. **Parar quando**: Atingir pureza, profundidade máxima, ou mínimo de exemplos

```python
class Node:
    """
    Representa um nó na árvore de decisão.
    Pode ser um nó interno (com pergunta) ou folha (com predição).
    """
    def __init__(self, feature=None, threshold=None, left=None, right=None, value=None):
        self.feature = feature        # Índice da feature usada para dividir
        self.threshold = threshold    # Valor de corte
        self.left = left             # Subárvore esquerda (≤ threshold)
        self.right = right           # Subárvore direita (> threshold)
        self.value = value           # Valor previsto (apenas em folhas)
    
    def is_leaf(self):
        """Verifica se é um nó folha (sem filhos)"""
        return self.value is not None

class DecisionTreeClassifier:
    def __init__(self, max_depth=None, min_samples_split=2, criterion='gini'):
        """
        Parâmetros de controle da árvore:
        - max_depth: Profundidade máxima (evita overfitting)
        - min_samples_split: Mínimo de exemplos para dividir um nó
        - criterion: 'gini' ou 'entropy'
        """
        self.max_depth = max_depth
        self.min_samples_split = min_samples_split
        self.criterion = criterion
        self.root = None
    
    def _impurity(self, y):
        """Calcula impureza baseado no critério escolhido"""
        if self.criterion == 'gini':
            return gini_impurity(y)
        else:
            return entropy(y)
    
    def _best_split(self, X, y):
        """
        Encontra a melhor divisão possível para este nó.
        Testa todas features e todos thresholds.
        """
        best_gain = -1
        best_feature = None
        best_threshold = None
        
        n_features = X.shape[1]
        parent_impurity = self._impurity(y)
        
        # Para cada feature
        for feature_idx in range(n_features):
            # Pega valores únicos como candidatos a threshold
            thresholds = np.unique(X[:, feature_idx])
            
            # Para cada threshold possível
            for threshold in thresholds:
                # Divide os dados
                left_mask = X[:, feature_idx] <= threshold
                right_mask = ~left_mask
                
                # Ignora divisões que não separam
                if np.sum(left_mask) == 0 or np.sum(right_mask) == 0:
                    continue
                
                # Calcula impureza dos filhos
                n = len(y)
                n_left, n_right = np.sum(left_mask), np.sum(right_mask)
                left_impurity = self._impurity(y[left_mask])
                right_impurity = self._impurity(y[right_mask])
                
                # Impureza ponderada
                child_impurity = (n_left/n) * left_impurity + (n_right/n) * right_impurity
                
                # Ganho de informação
                gain = parent_impurity - child_impurity
                
                # Atualiza melhor divisão
                if gain > best_gain:
                    best_gain = gain
                    best_feature = feature_idx
                    best_threshold = threshold
        
        return best_feature, best_threshold, best_gain
    
    def _build_tree(self, X, y, depth=0):
        """
        Constrói a árvore recursivamente.
        Este é o coração do algoritmo.
        """
        n_samples, n_features = X.shape
        n_classes = len(np.unique(y))
        
        # Critérios de parada
        if (depth >= self.max_depth if self.max_depth else False) or \
           n_samples < self.min_samples_split or \
           n_classes == 1:
            # Cria nó folha com a classe mais comum
            leaf_value = np.argmax(np.bincount(y))
            return Node(value=leaf_value)
        
        # Encontra melhor divisão
        best_feature, best_threshold, best_gain = self._best_split(X, y)
        
        # Se não há ganho, cria folha
        if best_gain == 0:
            leaf_value = np.argmax(np.bincount(y))
            return Node(value=leaf_value)
        
        # Divide os dados
        left_mask = X[:, best_feature] <= best_threshold
        right_mask = ~left_mask
        
        # Constrói subárvores recursivamente
        left_subtree = self._build_tree(X[left_mask], y[left_mask], depth + 1)
        right_subtree = self._build_tree(X[right_mask], y[right_mask], depth + 1)
        
        # Retorna nó interno
        return Node(
            feature=best_feature,
            threshold=best_threshold,
            left=left_subtree,
            right=right_subtree
        )
    
    def fit(self, X, y):
        """Treina a árvore"""
        self.root = self._build_tree(X, y)
        return self
    
    def _predict_sample(self, x, node):
        """Percorre a árvore para um único exemplo"""
        if node.is_leaf():
            return node.value
        
        # Decide qual caminho seguir
        if x[node.feature] <= node.threshold:
            return self._predict_sample(x, node.left)
        else:
            return self._predict_sample(x, node.right)
    
    def predict(self, X):
        """Faz previsões para múltiplos exemplos"""
        return np.array([self._predict_sample(x, self.root) for x in X])
```

## 🎨 Visualização e Interpretação

> [!success] A Grande Vantagem Diferente de modelos "caixa-preta", árvores de decisão são completamente interpretáveis. Você pode visualizar exatamente como cada decisão é tomada.

```python
def print_tree(node, feature_names=None, indent=""):
    """
    Imprime a árvore em formato texto hierárquico.
    Cada nível de indentação representa um nível na árvore.
    """
    if node.is_leaf():
        print(f"{indent}Predição: Classe {node.value}")
        return
    
    feature_name = f"Feature {node.feature}" if feature_names is None else feature_names[node.feature]
    print(f"{indent}Se {feature_name} <= {node.threshold:.2f}:")
    print_tree(node.left, feature_names, indent + "  ")
    print(f"{indent}Senão:")
    print_tree(node.right, feature_names, indent + "  ")

# Exemplo de uso
from sklearn.datasets import load_iris

iris = load_iris()
X, y = iris.data[:, :2], iris.target  # Usa apenas 2 features para simplicidade

tree = DecisionTreeClassifier(max_depth=3)
tree.fit(X, y)

print("=== Estrutura da Árvore ===")
print_tree(tree.root, iris.feature_names[:2])
```

## 🔧 Árvore de Regressão

> [!note] Adaptação para Valores Contínuos Para regressão, mudamos apenas dois aspectos: o critério de divisão (usamos variância em vez de impureza) e a predição nas folhas (média dos valores em vez de classe mais comum).

> [!formula] Critério para Regressão
> 
> ```
> MSE(S) = (1/n) · Σ(yᵢ - ȳ)²
> 
> Onde ȳ é a média dos valores em S
> ```

```python
class DecisionTreeRegressor:
    def __init__(self, max_depth=None, min_samples_split=2):
        self.max_depth = max_depth
        self.min_samples_split = min_samples_split
        self.root = None
    
    def _mse(self, y):
        """Calcula o erro quadrático médio (variância)"""
        if len(y) == 0:
            return 0
        return np.var(y)
    
    def _best_split(self, X, y):
        """Encontra divisão que minimiza MSE"""
        best_mse_reduction = -1
        best_feature = None
        best_threshold = None
        
        parent_mse = self._mse(y)
        n_features = X.shape[1]
        
        for feature_idx in range(n_features):
            thresholds = np.unique(X[:, feature_idx])
            
            for threshold in thresholds:
                left_mask = X[:, feature_idx] <= threshold
                right_mask = ~left_mask
                
                if np.sum(left_mask) == 0 or np.sum(right_mask) == 0:
                    continue
                
                # MSE ponderado dos filhos
                n = len(y)
                left_mse = self._mse(y[left_mask])
                right_mse = self._mse(y[right_mask])
                child_mse = (np.sum(left_mask)/n) * left_mse + (np.sum(right_mask)/n) * right_mse
                
                # Redução no MSE
                mse_reduction = parent_mse - child_mse
                
                if mse_reduction > best_mse_reduction:
                    best_mse_reduction = mse_reduction
                    best_feature = feature_idx
                    best_threshold = threshold
        
        return best_feature, best_threshold, best_mse_reduction
    
    def _build_tree(self, X, y, depth=0):
        """Constrói árvore de regressão"""
        n_samples = X.shape[0]
        
        # Critérios de parada
        if (depth >= self.max_depth if self.max_depth else False) or \
           n_samples < self.min_samples_split:
            # Folha: retorna média dos valores
            return Node(value=np.mean(y))
        
        best_feature, best_threshold, best_reduction = self._best_split(X, y)
        
        if best_reduction == 0:
            return Node(value=np.mean(y))
        
        # Divide e constrói subárvores
        left_mask = X[:, best_feature] <= best_threshold
        right_mask = ~left_mask
        
        left_subtree = self._build_tree(X[left_mask], y[left_mask], depth + 1)
        right_subtree = self._build_tree(X[right_mask], y[right_mask], depth + 1)
        
        return Node(
            feature=best_feature,
            threshold=best_threshold,
            left=left_subtree,
            right=right_subtree
        )
    
    def fit(self, X, y):
        self.root = self._build_tree(X, y)
        return self
    
    def _predict_sample(self, x, node):
        if node.is_leaf():
            return node.value
        
        if x[node.feature] <= node.threshold:
            return self._predict_sample(x, node.left)
        else:
            return self._predict_sample(x, node.right)
    
    def predict(self, X):
        return np.array([self._predict_sample(x, self.root) for x in X])
```

## ⚠️ Overfitting e Regularização

> [!danger] O Problema do Overfitting Árvores de decisão têm uma tendência natural ao overfitting. Sem restrições, elas crescem até memorizar perfeitamente os dados de treino, criando regras extremamente específicas que não generalizam.

> [!example] Sintoma de Overfitting Imagine uma árvore que cria uma regra como: "Se idade = 27.3 anos E altura = 1.73m E peso = 72.1kg, então classe A". Isso é memorização, não aprendizado.

### Técnicas de Poda

A poda controla a complexidade da árvore, evitando que ela fique excessivamente detalhada:

> [!tip] Estratégias de Regularização **Pré-poda (Pre-pruning)**:
> 
> - Limita `max_depth` (profundidade máxima)
> - Define `min_samples_split` (mínimo de exemplos para dividir)
> - Estabelece `min_samples_leaf` (mínimo de exemplos em folha)
> - Requer `min_impurity_decrease` (ganho mínimo para dividir)
> 
> **Pós-poda (Post-pruning)**:
> 
> - Constrói árvore completa primeiro
> - Remove subárvores que não melhoram performance em validação
> - Mais custoso computacionalmente, mas teoricamente melhor

```python
# Exemplo: comparando árvores com diferentes profundidades
from sklearn.model_selection import train_test_split
from sklearn.metrics import accuracy_score

X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.3, random_state=42)

depths = [1, 2, 3, 5, 10, None]
print("=== Impacto da Profundidade ===")

for depth in depths:
    tree = DecisionTreeClassifier(max_depth=depth)
    tree.fit(X_train, y_train)
    
    train_acc = accuracy_score(y_train, tree.predict(X_train))
    test_acc = accuracy_score(y_test, tree.predict(X_test))
    
    depth_str = str(depth) if depth else "Ilimitada"
    print(f"Profundidade {depth_str:10s} - Treino: {train_acc:.3f}, Teste: {test_acc:.3f}")
```

## 🎯 Vantagens e Limitações

> [!success] Vantagens **Interpretabilidade**: Cada previsão pode ser explicada por um caminho claro de decisões. Perfeito quando é necessário justificar decisões para stakeholders ou requisitos regulatórios.
> 
> **Não-paramétrico**: Não assume distribuição específica dos dados. Funciona com relações não-lineares e interações complexas entre features.
> 
> **Features mistas**: Lida naturalmente com features numéricas e categóricas sem necessidade de encoding elaborado.
> 
> **Pouco pré-processamento**: Não requer normalização ou padronização. É robusto a outliers e valores faltantes.

> [!warning] Limitações **Instabilidade**: Pequenas mudanças nos dados podem gerar árvores completamente diferentes. Isso prejudica a confiabilidade e reprodutibilidade.
> 
> **Overfitting fácil**: Tende a criar modelos muito complexos que memorizam o treino. Requer cuidado com regularização.
> 
> **Viés em features**: Favorece features com mais valores únicos. Pode ignorar features importantes com poucos valores.
> 
> **Fronteiras de decisão limitadas**: Só cria fronteiras paralelas aos eixos (ortogonais). Não consegue representar diagonais diretamente, precisando de múltiplas divisões.

## 🚀 Aplicação Prática Completa

```python
from sklearn.datasets import make_classification
from sklearn.model_selection import cross_val_score
import matplotlib.pyplot as plt

# Gera dataset sintético
X, y = make_classification(
    n_samples=500,
    n_features=2,
    n_informative=2,
    n_redundant=0,
    n_clusters_per_class=1,
    random_state=42
)

# Treina modelo
tree = DecisionTreeClassifier(max_depth=5, min_samples_split=10)
tree.fit(X, y)

# Validação cruzada
scores = cross_val_score(tree, X, y, cv=5)
print(f"Acurácia média (CV): {scores.mean():.3f} (±{scores.std():.3f})")

# Visualiza fronteiras de decisão
def plot_decision_boundary(model, X, y):
    h = 0.02
    x_min, x_max = X[:, 0].min() - 1, X[:, 0].max() + 1
    y_min, y_max = X[:, 1].min() - 1, X[:, 1].max() + 1
    xx, yy = np.meshgrid(np.arange(x_min, x_max, h),
                         np.arange(y_min, y_max, h))
    
    Z = model.predict(np.c_[xx.ravel(), yy.ravel()])
    Z = Z.reshape(xx.shape)
    
    plt.figure(figsize=(10, 8))
    plt.contourf(xx, yy, Z, alpha=0.4, cmap='RdYlBu')
    plt.scatter(X[:, 0], X[:, 1], c=y, cmap='RdYlBu', edgecolors='black')
    plt.xlabel('Feature 1')
    plt.ylabel('Feature 2')
    plt.title('Fronteiras de Decisão da Árvore')
    plt.show()

plot_decision_boundary(tree, X, y)
```

---

> [!summary] Conclusão Árvores de Decisão são algoritmos poderosos e elegantes que combinam simplicidade conceitual com capacidade de modelar relações complexas. Sua interpretabilidade as torna ideais para domínios onde transparência é crucial. Embora sofram de instabilidade e tendência ao overfitting, servem de base fundamental para métodos ensemble como Random Forest e Gradient Boosting, que superam essas limitações mantendo as vantagens.

## 🔗 Tópicos Relacionados

- [[ML - Random Florest]] - Ensemble de árvores para maior robustez
- [[Gradient-Boosting]] - Combinação sequencial de árvores