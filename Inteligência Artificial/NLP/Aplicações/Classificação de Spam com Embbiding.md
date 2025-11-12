# Implementação de Rede Neural com Embedding para Classificação de Spam

## 📋 Visão Geral do Processo

Neste projeto, adaptamos uma rede neural para classificação de mensagens como "spam" ou "não spam" (ham), utilizando uma abordagem mais sofisticada com camadas de embedding em vez da vetorização tradicional CountVectorizer.

## 🔄 Adaptação do Notebook Original

### Criação da Cópia do Ambiente
```python
# Arquivo → Salvar uma cópia no Drive
# Nome: "Neural_dois_Neuro"
```
**Objetivo:** Criar um ambiente isolado para experimentação sem afetar o código original. O nome "Neural_dois" indica que esta é uma segunda versão neural melhorada.

## 🧩 Modificações nas Importações

### Bibliotecas Adicionadas
```python
from keras.layers import Embedding, Flatten
from tensorflow.keras.preprocessing.text import Tokenizer
from tensorflow.keras.preprocessing.sequence import pad_sequences
```

### 🔍 Análise das Novas Importações

#### **Embedding Layer**
- **Função:** Cria representações vetoriais densas das palavras
- **Vantagem:** Aprende embeddings específicos para o domínio durante o treinamento
- **Comparação:** Substitui o CountVectorizer que criava representações esparsas

#### **Flatten Layer**
- **Função:** "Achata" a saída multidimensional do embedding para conectar com camadas densas
- **Necessidade:** Converte formato 2D+ para 1D, exigido pelas camadas Dense

#### **Tokenizer**
- **Substitui:** CountVectorizer do approach anterior
- **Funcionalidade:** Converte texto em sequências numéricas (tokens)

#### **pad_sequences**
- **Problema resolvido:** Mensagens têm comprimentos variáveis
- **Solução:** Padroniza o tamanho das sequências para alimentar a rede neural

## 📊 Upload e Preparação dos Dados

### Reinicialização do Ambiente
```python
# Editar → Limpar todas as saídas
# Upload do arquivo spam.csv novamente
```
**Motivo:** Cada sessão do Colab é isolada, necessitando reupload dos dados.

### Processo de Label Encoding
```python
labelEncoder = LabelEncoder()
y = labelEncoder.fit_transform(spam['Category'])
```
**Transformação:** Converte labels textuais ("spam", "ham") para numéricos (1, 0)

## 🔤 Tokenização do Texto

### Configuração do Tokenizer
```python
token = Tokenizer(num_words=1000)
```

#### **Parâmetro num_words=1000**
- **Função:** Define o vocabulário máximo como 1000 palavras
- **Seleção:** Mantém as 1000 palavras mais frequentes
- **Descarte:** Palavras menos frequentes são ignoradas

#### **Processo de Fit e Transform**
```python
token.fit_on_texts(X_train)
X_train = token.texts_to_sequences(X_train)
X_test = token.texts_to_sequences(X_test)
```

### 🔍 Métodos do Tokenizer

#### **fit_on_texts()**
- **Função:** Aprende o vocabulário a partir dos textos de treino
- **Processo interno:** Cria um mapeamento palavra → índice

#### **texts_to_sequences()**
- **Função:** Converte textos em sequências numéricas
- **Exemplo:** "Hello world" → [12, 45] (onde 12="hello", 45="world")

## 📏 Padronização de Sequências

### Aplicação do pad_sequences
```python
X_train = pad_sequences(X_train, maxlen=500, padding='post')
X_test = pad_sequences(X_test, maxlen=500, padding='post')
```

### 📐 Parâmetros Detalhados

#### **maxlen=500**
- **Função:** Define comprimento máximo de 500 tokens por mensagem
- **Truncamento:** Mensagens >500 tokens são cortadas
- **Preenchimento:** Mensagens <500 tokens recebem padding

#### **padding='post'**
- **Comportamento:** Adiciona zeros no final da sequência
- **Alternativa:** `padding='pre'` (pad no início - padrão)
- **Vantagem do 'post':** Preserva o início da mensagem que geralmente contém informação mais relevante

### 🎯 Propósito do Padding
```python
# Antes do padding: sequências de tamanhos variáveis
[ [1, 45, 23, 67], 
  [12, 8], 
  [45, 23, 67, 89, 12, 34, 56] ]

# Depois do padding: matriz uniforme
[ [1, 45, 23, 67, 0, 0, 0], 
  [12, 8, 0, 0, 0, 0, 0], 
  [45, 23, 67, 89, 12, 34, 56] ]
```

## 🏗️ Construção da Arquitetura Neural

### Camada de Embedding
```python
modelo.add(Embedding(
    input_dim=len(token.word_index) + 1,
    output_dim=50,
    input_length=500
))
```

#### **input_dim**
- **Cálculo:** `len(token.word_index) + 1`
- **Função:** Tamanho do vocabulário + 1 (para o índice 0 usado no padding)
- **Explicação:** Define quantas palavras diferentes a camada precisa mapear

#### **output_dim=50**
- **Significado:** Tamanho do vetor de embedding para cada palavra
- **Interpretação:** Cada palavra é representada por um vetor de 50 dimensões
- **Vantagem:** Captura relações semânticas entre palavras

#### **input_length=500**
- **Correspondência:** Mesmo valor do `maxlen` no pad_sequences
- **Função:** Informa o comprimento fixo das sequências de entrada

### 🔄 Processo do Embedding

#### **Transformação Realizada:**
```
Entrada: [12, 45, 23, 0, 0, ...]  # 500 elementos
↓ Embedding Layer
Saída: [
    [0.1, 0.4, -0.2, ..., 0.8],  # vetor para palavra 12
    [0.3, -0.1, 0.9, ..., -0.4], # vetor para palavra 45  
    [0.7, 0.2, -0.5, ..., 0.1],  # vetor para palavra 23
    [0.0, 0.0, 0.0, ..., 0.0],   # vetor para padding
    ...
]  # Formato: (500, 50)
```

### Camada Flatten
```python
modelo.add(Flatten())
```

#### **Necessidade:**
- **Problema:** Saída do Embedding é 2D (500, 50)
- **Solução:** Flatten transforma em 1D (500 * 50 = 25000 elementos)
- **Propósito:** Compatibilidade com camadas Dense seguintes

### Camadas Dense e Dropout
```python
modelo.add(Dense(128, activation='relu'))
modelo.add(Dropout(0.3))
modelo.add(Dense(1, activation='sigmoid'))
```

#### **Arquitetura Final:**
1. **Embedding:** (500, 50) → Aprende representações de palavras
2. **Flatten:** (500, 50) → (25000) → Prepara para camadas densas
3. **Dense(128):** Processamento não-linear
4. **Dropout(0.3):** Regularização (evita overfitting)
5. **Dense(1):** Classificação binária final

## 🎯 Compilação do Modelo

```python
modelo.compile(
    loss='binary_crossentropy',
    optimizer='adam',
    metrics=['accuracy']
)
```

### **Configuração Mantida:**
- Mesmos parâmetros do approach anterior para comparação justa

## 🏋️ Processo de Treinamento

### Hiperparâmetros
```python
modelo.fit(
    X_train, y_train,
    epochs=20,
    batch_size=32,
    validation_data=(X_test, y_test)
)
```

### 📊 Análise do Treinamento

#### **epochs=20**
- **Significado:** 20 passadas completas pelo dataset de treino
- **Monitoramento:** Acompanhamento da loss e accuracy em tempo real

#### **batch_size=32**
- **Processamento:** 32 amostras por vez antes de atualizar pesos
- **Vantagem:** Balance entre eficiência e estabilidade

#### **Dados de Validação**
- **Uso:** `validation_data=(X_test, y_test)`
- **Propósito:** Monitorar performance em dados não vistos durante treino

## 📈 Resultados e Performance

### Métricas Finais
- **Loss final:** ~0.09
- **Acurácia:** ~0.98
- **Comparação com approach anterior:** Performance similar

### Matriz de Confusão
```
[ [Verdadeiros Negativos, Falsos Positivos],
  [Falsos Negativos, Verdadeiros Positivos] ]
```

**Resultado Observado:**
- 19 erros de classificação
- Performance consistente entre treino e validação

## 💡 Insights e Considerações

### Vantagens do Approach com Embedding
1. **Aprendizado Específico:** Embeddings adaptados ao domínio de spam
2. **Representação Rica:** Vetores densos capturam nuances semânticas
3. **Escalabilidade:** Melhor para vocabulários grandes

### Comparação com Approach Anterior
- **Performance:** Similar nos dois métodos
- **Complexidade:** Embedding é mais sofisticado
- **Aplicabilidade:** Embedding é preferível para textos complexos

### ⚠️ Observações Importantes
- **Não há diferença significativa** de performance entre os métodos para este dataset específico
- A escolha do método depende do contexto e complexidade do problema
- O embedding oferece mais flexibilidade para problemas de NLP mais complexos

## 🎯 Conclusão

A implementação com camadas de embedding demonstra uma abordagem mais moderna e poderosa para processamento de linguagem natural, embora para este caso específico de detecção de spam os resultados tenham sido equivalentes ao método tradicional. A arquitetura desenvolvida serve como base para problemas de NLP mais complexos.