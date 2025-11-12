> [!info] ## 💬 O que é NLP
> **NLP** (*Natural Language Processing* — Processamento de Linguagem Natural) é um ramo da **Inteligência Artificial** que permite que computadores **compreendam**, **interpretem** e **gerem** linguagem humana — seja em português, inglês, japonês ou qualquer outra.
>
> Em resumo, sistemas de NLP são capazes de:
> - 🧠 **Entender** texto (ex.: análise de sentimento, classificação de intenções)
> - ✍️ **Gerar** texto (ex.: chatbots, resumos, traduções)
> - 🔍 **Extrair** informação (ex.: reconhecimento de entidades, busca semântica)

> [!tip] ## 🧩 Pipeline de NLP
> ![[Pasted image 20251009112304.png]]
>
> ### ⚙️ Etapa 1 — Ingestão de Dados
> A **ingestão** é o ponto de partida de qualquer pipeline de NLP.  
> Aqui, os dados brutos são **coletados**, **centralizados** e **preparados** para processamento posterior.
>
> 🔗 **Fontes comuns de dados:**
> - 🗄️ **Bancos de dados** — registros estruturados (ex.: SQL, NoSQL)
> - 📄 **Documentos digitais** — PDFs, textos, planilhas, relatórios
> - ☁️ **Serviços em nuvem** — buckets (S3, GCS), APIs, logs
> - 🧾 **OCR (Reconhecimento Óptico de Caracteres)** — digitaliza texto a partir de imagens ou scans
> - 🌐 **Web Scraping** — coleta automática de conteúdo na web
>
> > [!attention] 💡 *Dica:* Nessa fase, o foco é **obter dados com qualidade** e **garantir padronização** (codificação, idioma, formato).  
> > Dados sujos aqui = modelo ruim lá na frente.
> 
> ---
> 
>  ### 🧼 2. Pré-processamento
> É aqui que o texto cru passa por uma faxina cirúrgica pra virar dado útil.  
> O objetivo é **eliminar ruídos**, **padronizar formatos** e **destacar o que realmente importa**.
>
> 🔧 **Principais etapas:**
> - 🧹 Remoção de **pontuação**, **[[NLP - Stop Words]]** e **caracteres inúteis**
> - 🔡 **Normalização** de texto (minúsculas, acentuação, emojis, etc.)
> - ✂️ **Tokenização** — quebra o texto em palavras ou sentenças
> - 🧬 **Lematização / [[NLP - Stemming]]** — reduz palavras à forma base
>
> 🧠 **Conceitos-chave aplicados nessa fase:**
> - 🏷️ **[[NLP - POS Tagging (Part-of-Speech)]]:** identifica a função de cada palavra (substantivo, verbo, adjetivo...)  
> - 🪶 **[[NLP - Annotation]]:** rotula partes do texto com informações semânticas, sintáticas ou contextuais  
> - ⚙️ **Engenharia de Atributos:** cria novas features a partir do texto (ex.: contagem de palavras, polaridade, frequência)
>
> > [!attention] 💡 *Dica:* Pré-processar é como afiar a lâmina antes de cortar — se pular essa parte, o modelo só vai mastigar o texto sem entender
> ---
> ### 🧱 3. Representação Textual - Da Linguagem para a Matemática
>
> #### 🎯 **A Ponte entre Humanos e Máquinas**
> Transformamos palavras em **vetores numéricos** que preservam relações semânticas e sintáticas.
>
> #### 📊 **Evolução das Técnicas de Embedding**
> 
### **1. 🎪 Métodos Estatísticos Clássicos**
| Técnica | Mecanismo | Casos de Uso |
|---------|-----------|-------------|
| **Bag of Words (BoW)** | Matriz de contagem de termos | Baseline rápido para classificação |
| **TF-IDF** | Ponderação por relevância (frequência inversa) | Sistemas de recomendação, search |
| **One-Hot Encoding** | Vetores esparsos binários | Pré-processamento para redes neurais |

**💡 Insight:** `"rei" - "homem" + "mulher" = "rainha"` → **NÃO FUNCIONA** com esses métodos

### **2. 🧠 Word Embeddings Estáticos**
| Modelo | Arquitetura | Vantagem |
|--------|-------------|----------|
| **Word2Vec** | Skip-gram/CBOW | Captura relações semânticas |
| **GloVe** | Fatorização de matriz co-ocorrência | Combina estatística + aprendizado |
| **FastText** | N-gramas de caracteres | Lida com palavras desconhecidas |

**🚀 Breakthrough:** `vetor("rei") - vetor("homem") + vetor("mulher") ≈ vetor("rainha")`

### **3. 🎭 Embeddings Contextuais (Revolução)**
| Modelo | Inovação | Impacto |
|--------|----------|---------|
| **BERT** | Bidirecional + Masked LM | Entende contexto completo |
| **GPT** | Decoder-only autoregressivo | Geração de texto coerente |
| **T5** | Text-to-Text unified framework | Transforma qualquer tarefa em texto |

**🎯 Diferença Crucial:** 
- **Static:** "banco" = mesmo vetor em todos contextos
- **Contextual:** "banco financeiro" ≠ "banco de praça"

---

# 🤖 4. Modelagem - O Coração do Aprendizado

## 🎯 **Seleção do Paradigma de Aprendizado**

### **Supervisionado** 🎯 → **Dados Rotulados**
```python
# Tarefas Típicas
- Classificação: spam/ham, positivo/negativo
- Regressão: score de sentimento (0-10)
- NER: Reconhecimento de entidades
- POS Tagging: Análise gramatical
```

**Algoritmos:** SVM, Random Forest, BERT Fine-tuning

### **Não Supervisionado** 🔍 → **Padrões Naturais**
```python
# Tarefas Típicas  
- Clustering: agrupamento de tópicos
- Topic Modeling: LDA, NMF
- Anomaly Detection: textos fora do padrão
```

**Algoritmos:** K-means, LDA, Autoencoders

### **Semi/Auto-Supervisionado** 🧩 → **Escalabilidade**
```python
# Estratégias
- Self-training: modelo se auto-rotula
- Pre-training: BERT/GPT em corpus gigante
- Data augmentation: back-translation
```

**Vantagem:** Aproveita dados não rotulados abundantes

## 🏗️ **Arquiteturas Modernas**

### **Encoder-Decoder** (Seq2Seq)
```python
# Aplicações: Tradução, Resumo, Q&A
Input: "Como está o tempo?" 
→ Encoder: [context_vector]
→ Decoder: "Está ensolarado hoje."
```

### **Transformer-Based**
```python
# Mecanismo: Self-Attention + Feed Forward
- BERT: Encoder-only → entendimento
- GPT: Decoder-only → geração  
- T5: Encoder-Decoder → tarefas diversas
```

### **Fine-tuning Estratégico**
```python
# Técnicas Avançadas
1. Feature-based: Usar embeddings congelados
2. Full fine-tuning: Ajustar todos pesos
3. LoRA: Ajuste eficiente de grandes modelos
```

---

# 🚀 5. Deploy - Do Laboratório para o Mundo Real

## 🎯 **Pipeline de Produção**

### **1. Empacotamento do Modelo**
```python
# Formatos de Serialização
- Pickle (sklearn) → Rápido mas frágil
- ONNX → Cross-platform
- TensorFlow SavedModel → Production-ready
- Hugging Face Pipeline → NLP especializado
```

### **2. Infraestrutura de Serviço**
```python
# Opções de Deploy
├── API REST (FastAPI/Flask)
├── Microserviços (Docker/K8s)  
├── Serverless (AWS Lambda)
└── Edge (Ort, TensorFlow Lite)
```

### **3. Monitoramento Contínuo** 📊
| Métrica | O que Monitorar | Alerta |
|---------|-----------------|--------|
| **Data Drift** | Distribuição inputs muda | Retreinar modelo |
| **Concept Drift** | Relação X-y muda | Reavaliar features |
| **Performance** | Acurácia/F1 em produção | Intervenção imediata |
| **Latência** | Tempo resposta > SLA | Otimizar código |

### **4. CI/CD para ML** 🔄
```yaml
# Pipeline Automatizado
1. Data Validation → 2. Model Training →
2. Model Evaluation → 4. A/B Testing →
3. Canary Deployment → 6. Monitoring
```

### **5. Manutenção Proativa** 🛠️
```python
# Estratégias de Atualização
- Retreino agendado: Semanal/Mensal
- Retreino sob demanda: Drift detection
- Shadow mode: Teste sem impacto
- A/B testing: Comparação de versões
```

## 🎯 **Princípio Fundamental**
**"Deploy não é o final do desenvolvimento, mas o início da observação do comportamento do modelo no mundo real."**

Cada interação do usuário é um novo ponto de dados que pode (e deve) alimentar melhorias contínuas no sistema.

