
tags #nlp, #lemmatization

# Lemmatization — o que é

> [!summary] **Lemmatization**  
> Transformação de palavras para sua **forma base (lema)**, considerando contexto linguístico e classe gramatical.
> 
> Exemplo: _amando_ → _amar_; _better_ → _good_.

## Resumo direto

Lemmatização reduz palavras à forma canônica (lema) usando dicionários e análise morfossintática. Ao contrário do _stemming_, preserva palavras reais e respeita classe gramatical.

## Diferença prática: lemmatization vs stemming

- **Stemming**: corta sufixos por regra (ex.: _running_ → _run_ ou _runn_ dependendo do stemmer). Rápido, às vezes impreciso.
    
- **Lemmatization**: usa dicionários e POS tagging (_part-of-speech_). Mais preciso, mais lento.
    

|Palavra|Stemmer (ex.)|Lemmatizer|
|---|--:|---|
|running|run|run|
|better|bett|good|
|studies|studi|study|

## Quando usar

- Pré-processamento para tarefas que exigem significado mais fiel (NLP clássico, recuperação de informação, análise semântica).
    
- Quando precisão morfológica importa: tradução, perguntas e respostas, análise de sentimento fina.
    

Quando não usar

- Aplicações em tempo real com orçamento de latência muito apertado (stemmers são mais rápidos).
    
- Casos onde formas flexivas carregam informação útil (ex.: tempo verbal para análise temporal).
    

## Exemplos em Python

### spaCy (recomendado para produção)

```python
import spacy
nlp = spacy.load("en_core_web_sm")
doc = nlp("The children are playing better than yesterday.")
lemmas = [(token.text, token.lemma_, token.pos_) for token in doc]
print(lemmas)
```

Saída esperada (exemplo):

```
[('The','the','DET'), ('children','child','NOUN'), ('are','be','AUX'), ('playing','play','VERB'), ('better','good','ADJ'), ('than','than','ADP'), ('yesterday','yesterday','NOUN'), ('.','.', 'PUNCT')]
```

### NLTK (WordNet Lemmatizer)

```python
from nltk.stem import WordNetLemmatizer
from nltk.corpus import wordnet

lemmatizer = WordNetLemmatizer()
# precisa mapear POS manualmente para melhores resultados
lemmatizer.lemmatize('running', pos='v')  # -> 'run'
lemmatizer.lemmatize('better', pos='a')   # -> 'good'
```

## Pontos práticos e armadilhas

- **POS tagging importa**: lemmatizers sem POS tendem a falhar em palavras ambíguas (ex.: _saw_ → _see_ ou _saw_?).
    
- **Línguas diferentes exigem ferramentas diferentes**: spaCy tem bons modelos para várias línguas; para português, ver `pt_core_news_sm` ou `stanza` e `UDPipe`.
    
- **Nomes próprios e slang**: podem não ter lemas confiáveis.
    
- **Casos multi-palavra**: expressões idiomáticas não são resolvidas pelo simples lemmatizer.
    

## Dicas de implementação

1. Tokenize → POS tag → Lemmatize com contexto.
    
2. Para performance, processe em lotes e reuse o pipeline do spaCy.
    
3. Combine lemmatização com normalização (lowercasing, remoção de acentos quando apropriado).
    

## Exemplo simples em português (spaCy)

```python
import spacy
nlp = spacy.load("pt_core_news_sm")
doc = nlp("As crianças estavam brincando mais feliz ontem.")
[(t.text, t.lemma_, t.pos_) for t in doc]
```

## Recursos e bibliotecas úteis

- spaCy — pipeline robusto, modelos por idioma
    
- NLTK — WordNet lemmatizer (bom para inglês)
    
- Stanza / UDPipe — ótimo para línguas com recursos morfológicos variados
    
- Hugging Face tokenizers + modelos Transformer — quando quiser embeddings e técnicas modernas combinadas com normalização avançada
    

---

# Anotações rápidas (uso no Obsidian)

- Tagueie com `#nlp` e `#lemmatization`
    
- Use backlinks para notas como `Tokenization`, `POS tagging`, `spaCy`.
    

### Checklist para testar lemmatização em um projeto

-  Definir idioma e modelo
    
-  Validar com exemplos reais do domínio
    
-  Medir impacto em tarefa (ex.: F1, precisão)
    
-  Otimizar pipeline para batch
    
